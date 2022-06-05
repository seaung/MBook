##### osquery简介

osquery是由facebook推出的一款基于sqlite的，开源的，能让你的设备像数据库一样进行查询的数据库

##### osquery安装

osquery的安装非常的简单，我们直接从它的官网下载对应平台的安装包即可。目前支持的平台有Linux,MacOSX,Ubuntu,Centos,Windows等。

我选用的平台是arch linux所以我使用以下命令就可在我的机器上安装osquery了

```bash
sudo pacman -S osquery
```

##### osqueryi的使用

下面是osqueryi交互式界面上的基本使用，在终端中输入`.help`查看帮助信息

```bash
osquery> .help
Welcome to the osquery shell. Please explore your OS!
You are connected to a transient 'in-memory' virtual database.

.all [TABLE]     Select all from a table
.bail ON|OFF     Stop after hitting an error
.connect PATH    Connect to an osquery extension socket
.disconnect      Disconnect from a connected extension socket
.echo ON|OFF     Turn command echo on or off
.exit            Exit this program
.features        List osquery's features and their statuses
.headers ON|OFF  Turn display of headers on or off
.help            Show this message
.mode MODE       Set output mode where MODE is one of:
                   csv      Comma-separated values
                   column   Left-aligned columns see .width
                   line     One value per line
                   list     Values delimited by .separator string
                   pretty   Pretty printed SQL results (default)
.nullvalue STR   Use STRING in place of NULL values
.print STR...    Print literal STRING
.quit            Exit this program
.schema [TABLE]  Show the CREATE statements
.separator STR   Change separator used by output mode
.socket          Show the local osquery extensions socket path
.show            Show the current values for various settings
.summary         Alias for the show meta command
.tables [TABLE]  List names of tables
.types [SQL]     Show result of getQueryColumns for the given query
.width [NUM1]+   Set column widths for "column" mode
.timer ON|OFF      Turn the CPU timer measurement on or off
```



---

that‘s all