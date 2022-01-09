##### 什么是gopacket？

gopacket是由google开发的go第三方包，用于解码数据包，重组流量，读取和写入.pcap文件，检查各层数据，以及操作数据包，该包还允许我们使用Berkeley数据包过滤器过滤流量。

##### 安装gopacket

gopacket包依赖于外部库和驱动程序来绕过操作系统的协议栈。所以在使用gopacket之前，请确保你的系统是否安装了libpcap开发库

windows系统需要安装npcap或winpcap

linux或mac系统需要安装libpcap

```bash
// ubuntu
sudo apt-get install libpcap-dev

// archlinux
sudo pacman -S libpcap

// mac
sudo brew install libpcap-dev
```

在安装libpcap开发库后，则可以在装gopacket库了

```bash
go get github.com/google/gopacket
```

##### 枚举设备

我们可以通过调用FindAlldevs()方法来枚举设备。然后遍历找到的设备。代码如下所示

```go
package main

import (
    "fmt"
    "log"
    "github.com/google/gopacket/pcap"
)

func main() {
    devices, err := pcap.FindAllDevs()
    if err != nil {
        log.Panicln(err)
    }
    
    for _, device := range devices {
        fmt.Println(device.Name)
        for _, address := range device.Addresses {
            fmt.Printf("IP : %s\n", address.IP)
            fmt.Printf("NetMask : %s\n", address.Netmask)
        }
    }
}
```

https://github.com/google/gopacket/blob/master/pcap/pcap.go

pcap.go文件下定义了Interface和InterfaceAddress这两个结构体，这两个结构体里包含了网络设备的一些常见的信息，如Name(设备名称), Description(描述)等字段，FindAllDevs()方法最终返回的是一个Interface结构体数组和error.它的源码如下所示:

```go
// Interface describes a single network interface on a machine.
type Interface struct {
	Name        string
	Description string
	Flags       uint32
	Addresses   []InterfaceAddress
}

// InterfaceAddress describes an address associated with an Interface.
// Currently, it's IPv4/6 specific.
type InterfaceAddress struct {
	IP        net.IP
	Netmask   net.IPMask // Netmask may be nil if we were unable to retrieve it.
	Broadaddr net.IP     // Broadcast address for this IP may be nil
	P2P       net.IP     // P2P destination address for this IP may be nil
}

// FindAllDevs attempts to enumerate all interfaces on the current machine.
func FindAllDevs() (ifs []Interface, err error) {
	alldevsp, err := pcapFindAllDevs()
	if err != nil {
		return nil, err
	}
	defer alldevsp.free()

	for alldevsp.next() {
		var iface Interface
		iface.Name = alldevsp.name()
		iface.Description = alldevsp.description()
		iface.Addresses = findalladdresses(alldevsp.addresses())
		iface.Flags = alldevsp.flags()
		ifs = append(ifs, iface)
	}
	return
}
```

##### 实时捕获流量

实时捕获流量的关键函数是OpenLive(device string, snaplen int32, promisc bool, timeout time.Duration)方法，这个方法接受4个参数分别是:

1. device: 设备名称

2. snaplen: 每个数据包读取的最大长度
3. promisc: 是否将设备接口设置为混杂模式
4. timeout: 超市时间，如果设置为30秒，则每三十秒刷新一次数据包。设置成负数则会立刻刷新数据包

OpenLive方法返回一个*pcap.Handle类型的结构体和error,这个Handle类型的结构体它允许我们读取和注入数据包。使用这个handle，可以应用一个BPF过滤器并创建一个新的包数据源，可以从中读完数据包。

调用handle.SetBPFFilter方法为handle设置BPF过滤器

调用gopacket.NewPacketSource方法创建新的包数据源，这个方法接受两个参数，第一个参数是Openlive方法返回的handle类型的指针，第二个参数则是处理数据包是要使用的解码器，这里使用handle.LinkType()，此参数默认是以太网链路

最后遍历souce.Packets()它返回的是一个通道，通过这个通道读取数据包。

```go
package main

import (
    "fmt"
    "log"
    "github.com/google/gopacket"
    "github.com/google/gopacket/pcap"
)

var (
    iface = "eth0"
    snaplen = 1024
    promisc = false
    timeout = pcap.BlockForever
    filter = "tcp and port 80"
    devFound = false
)

func main() {
    devices, err := pcap.FindAllDevs()
    if err != nil {
        log.Panicln(err)
    }
    
    for _, device := range devices {
        if device.Name == iface {
            devFound = true
        }
    }
    if !devFound {
        log.Panicf("device named %s does not exist\n", iface)
    }
    
    handle, err := pcap.OpenLive(iface, snaplen, promisc, timeout)
    if err != nil {
        log.Panicln(err)
    }
    
    defer handle.Close()
    
    if err := handle.SetBPFFilter(fileter); err != nil {
        log.Panicln(err)
    }
    
    source := gopacket.NewPacketSource(handle, handle.LinkType())
    
    for packet := range source.Packets() {
        fmt.Println(packet)
    }
}
```

##### 解码数据包

gopacket提供layers包。该包可以让我们将原始数据包强制转换为其他协议层的格式

1. 调用gopacket.Layer()方法，该方法接受一个协议层的类型
2. 使用类型断言，断言其协议层的类型

```go
package main

import (
    "time"
    "github.com/google/gopacket"
    "github.com/google/gopakcet/pcap"
    "github.com/google/gopacket/layers"
    "fmt"
)

var (
    device string = "eth0"
    snapshotlen int32 = 1024
    promis bool = false
    err error
    timeout time.Duration = 30 * time.Second
    handle *pcap.Handle
)

func decodePacket(packet *gopacket.Packet) {
    ethernetLayer := packet.Layer(layers.LayerTypeEthernet) // 调用Layer方法
    if ethernetLayer != nil {
        ethernetPacket, _ := ethernetLayer.(*layers.Ethernet) // 对其进行类型断言，断言其对应的协议层
        fmt.Printf("source mac address : %s\n", ethernetPacket.SrcMAC)
    }
}

func main() {
    handle, err := pcap.OpenLive(device, snapshotlen, promis, timeout)
    if err != nil {
        panic(err)
    }
    defer handle.Close()
    
    packetSource := gopacket.NewPacketSource(handle, handle.LinkType())
    for _, source := range packetSource.Packets() {
        decodePacket(source)
    }
}
```

查看源码可知道每个协议层都实现了Layer接口

https://github1s.com/google/gopacket/blob/HEAD/base.go#L19

```go
// Layer represents a single decoded packet layer (using either the
// OSI or TCP/IP definition of a layer).  When decoding, a packet's data is
// broken up into a number of layers.  The caller may call LayerType() to
// figure out which type of layer they've received from the packet.  Optionally,
// they may then use a type assertion to get the actual layer type for deep
// inspection of the data.
type Layer interface {
	// LayerType is the gopacket type for this layer.
	LayerType() LayerType
	// LayerContents returns the set of bytes that make up this layer.
	LayerContents() []byte
	// LayerPayload returns the set of bytes contained within this layer, not
	// including the layer itself.
	LayerPayload() []byte
}
```

https://github1s.com/google/gopacket/blob/HEAD/packet.go#L56

```go
// Ethernet is the layer for Ethernet frame headers.
type Ethernet struct {
	BaseLayer
	SrcMAC, DstMAC net.HardwareAddr
	EthernetType   EthernetType
	// Length is only set if a length field exists within this header.  Ethernet
	// headers follow two different standards, one that uses an EthernetType, the
	// other which defines a length the follows with a LLC header (802.3).  If the
	// former is the case, we set EthernetType and Length stays 0.  In the latter
	// case, we set Length and EthernetType = EthernetTypeLLC.
	Length uint16
}
```

https://github1s.com/google/gopacket/blob/HEAD/packet.go#L56-L100

```go
type Packet interface {
	//// Functions for outputting the packet as a human-readable string:
	//// ------------------------------------------------------------------
	// String returns a human-readable string representation of the packet.
	// It uses LayerString on each layer to output the layer.
	String() string
	// Dump returns a verbose human-readable string representation of the packet,
	// including a hex dump of all layers.  It uses LayerDump on each layer to
	// output the layer.
	Dump() string

	//// Functions for accessing arbitrary packet layers:
	//// ------------------------------------------------------------------
	// Layers returns all layers in this packet, computing them as necessary
	Layers() []Layer
	// Layer returns the first layer in this packet of the given type, or nil
	Layer(LayerType) Layer
	// LayerClass returns the first layer in this packet of the given class,
	// or nil.
	LayerClass(LayerClass) Layer

	//// Functions for accessing specific types of packet layers.  These functions
	//// return the first layer of each type found within the packet.
	//// ------------------------------------------------------------------
	// LinkLayer returns the first link layer in the packet
	LinkLayer() LinkLayer
	// NetworkLayer returns the first network layer in the packet
	NetworkLayer() NetworkLayer
	// TransportLayer returns the first transport layer in the packet
	TransportLayer() TransportLayer
	// ApplicationLayer returns the first application layer in the packet
	ApplicationLayer() ApplicationLayer
	// ErrorLayer is particularly useful, since it returns nil if the packet
	// was fully decoded successfully, and non-nil if an error was encountered
	// in decoding and the packet was only partially decoded.  Thus, its output
	// can be used to determine if the entire packet was able to be decoded.
	ErrorLayer() ErrorLayer

	//// Functions for accessing data specific to the packet:
	//// ------------------------------------------------------------------
	// Data returns the set of bytes that make up this entire packet.
	Data() []byte
	// Metadata returns packet metadata associated with this packet.
	Metadata() *PacketMetadata
}
```

##### 重组数据包

1、创建 httpStreamFactory 结构体，实现 tcpassembly.StreamFactory 接口

2、创建连接池

streamPool := tcpassembly.NewStreamPool(streamFactory)

3、创建重组器

assembler := tcpassembly.NewAssembler(streamPool)

4、将数据包添加到重组器中

assembler.AssembleWithTimestamp(packet.NetworkLayer().NetworkFlow(), tcp, packet.Metadata().Timestamp)

```go
package main

import (
    "time"
    "fmt"
    "log"
    "net/http"
    "bufio"
    "io"
    
    "github.com/google/gopacket"
    "github.com/google/gopacket/pcap"
    "github.com/google/gopacket/layers"
    "github.com/google/gopacket/tcpassembly"
    "github.com/google/gopacket/tcpassembly/tcpreader"
)

var (
    device string = "eth0"
    snapshotlen int32 = 1024
    promis bool = true
    handle *pcap.Handle
    timeout time.Duration = 30 * time.Second
    err error
)

type httpStreamFactory struct{}

type httpStream struct {
    net, transport gopacket.Flow
    r			   tcpreader.ReaderStream
}

func (h *httpStreamFactory)New(net, transport gopacket.Flow) tcpassembly.Stream {
    hstream := &httpStream {
        net: net,
        transport: transport,
        r: tcpreader.NewReaderStream(),
    }
    go hstream.run()
    
    return &hstream.r
}

func (h *httpStream)run() {
    buffer := bufio.NewReader(&h.r)
    for {
        req, err := http.ReadRequest(buffer)
        if err == io.EOF {
            return
        } else if err != nil {
            return
        } else {
            body := tcpreader.DiscardBytesToEOF(req.Body)
            defer req.Body.Close()
        }
    }
}

func main() {
    handle, err = pcap.OPenLive(device, snapshotlen, promis, timeout)
    if err != nil {
        log.Panicln(err)
    }
    
    streamFactory := &httpStreamFactory{}
    streamPool := tcpassmbly.NewStreamPool(streamFactory)
    assembler := tcpassmbly.NewAssembler(streamPool)
    
    packetSource := gopacket.NewPacketSource(handle, handle.LinkType())
    
    packets := packetSource.Packets()
    ticker := time.Tick(time.Minute)
    
    for {
        select {
            case packet := <-packets:
            if packet.NewworkLayer() == nil || packet.TransportLayer() == nil || packet.TransportLayer().LayerType() != layers.LayerTypeTCP {
                continue
            }
            tcp := packet.TransportLayer().(*layers.TCP)
            // 重组数据包
            assembler.AssembleWithTimestamp(packet.NewworkLayer().NetworkFlow(), tcp, packet.Metadata().Timestamp)
            case <-ticker:
            assembler.FlushOlderThan(time.Now().Add(time.Minute * -2))
        }
    }
}
```



---

that's all

