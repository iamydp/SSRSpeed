


<h1 align="center">
    <br>SSRSpeed
</h1>
<p align="center">
Batch speed measuring tool based on Shadowsocks(R) and V2Ray
</p>
<p align="center">
   <img alt="GitHub tag (latest SemVer)" src="https://img.shields.io/github/tag/NyanChanMeow/SSRSpeed.svg">
    <img alt="GitHub release" src="https://img.shields.io/github/release/NyanChanMeow/SSRSpeed.svg">
    <img src="https://img.shields.io/github/license/NyanChanMeow/SSRSpeed.svg">
</p>

<p></p>

## Links
 - <del>中文文档</del>

## Important Hint
 - The test results are for reference only and do not guarantee versatility.
 - Before you publicly release your speed test results, be sure to ask the node owner if they agree to the release to avoid unnecessary disputes.
 - SpeedTestNet and Fast.com is no longer supported.
 - MacOS has not found a suitable way to detect libsodium, so be sure to ensure that libsodium is installed before testing nodes that use encryption methods such as chacha20.
 - Shadowsocks-libev and Simple-Obfs are recommended to be installed using a compiled installation. It is known that the Shadowsocks-libev version in the Debian repository is too low to use some new encryption methods.
 - If your hostname has non-ASCII characters, Web-UI will not work.

## Features

- Support for asynchronous file download testing.
- Support SpeedTestNet, Fast.com and socket download.
- Support for exporting result as json and png.
- Support batch import of configuration from GUI configuration file and SSPanel-v2, v3 subscription link.
- Support for importing data from any Json export file and re-exporting files of the specified format.
- Support WebUI
- Support Web page simulation test(Result export as HTML from template.).
- Automatic identification of proxy types.

## Requirements 

Universal dependency
See `requirements.txt`

Linux dependency
 - [libsodium](https://github.com/jedisct1/libsodium)
 - [Shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
 - [Simple-Obfs](https://github.com/shadowsocks/simple-obfs)

## Platform Support
### Platform Tested
1. Windows 10 x64
2. Ubuntu 18.04 LTS
3.  Termux on Android

### Theoretical Platform Support 
The platform that ability to run Python and Shadowsocks, ShadowsocksR, V2Ray.

## Developers
- <del>Removed as requested by the developer</del>

## Acknowledgement
 - New color scheme
   - Chunxiaoyi 纯小亦
-  Bugs Report
   - [Professional-V1](https://t.me/V1_BLOG)
   - [Julydate 七夏浅笑](https://www.julydate.com/)
 - Sponsors
    - [YoYu - Better Network Acceleration](https://home.yoyu.dev/aff.php?aff=115)
    - [N3RO Network](https://n3ro.io/register?ref=5027)
- This project uses the following open source projects
   -  [speedtest-cli](https://github.com/sivel/speedtest-cli)
   -  [Fast.com-cli](https://github.com/nkgilley/fast.com)

## Getting started

### Console Usage
`pip(pip3) install -r requirements.txt`

    python .\main.py
    Usage: main.py [options] arg1 arg2...
    
    Options:
      --version             show program's version number and exit
      -h, --help            show this help message and exit
      -c GUICONFIG, --config=GUICONFIG
                            Load config generated by shadowsocksr-csharp.
      -u URL, --url=URL     Load ssr config from subscription url.
      -m TEST_METHOD, --method=TEST_METHOD
                            Select test method in Select test method in [speedtestnet, fast, socket, stasync].
      -M TEST_MODE, --mode=TEST_MODE
                            Select test mode in [all,pingonly,wps].
      --include             Filter nodes by group and remarks using keyword.
      --include-remark      Filter nodes by remarks using keyword.
      --include-group       Filter nodes by group name using keyword.
      --exclude             Exclude nodes by group and remarks using keyword.
      --exclude-group       Exclude nodes by group using keyword.
      --exclude-remark      Exclude nodes by remarks using keyword.
      --use-ssr-cs          Replace the ShadowsocksR-libev with the ShadowsocksR-C# (Only Windows)
      -g GROUP              Manually set group.
      -y, --yes             Skip node list confirmation before test.
      -C RESULT_COLOR, --color=RESULT_COLOR
                        Set the colors when exporting images..
      -S SORT_METHOD, --sort=SORT_METHOD
                            Select sort method in
                            [speed,rspeed,ping,rping],default not sorted.
      -i IMPORT_FILE, --import=IMPORT_FILE
                            Import test result from json file and export it.
      --skip-requirements-check
                            Skip requirements check.
      --debug               Run program in debug mode.


Example usage :
- `python main.py -c gui-config.json --include 韩国 --include-remark Azure --include-group YoYu`
- `python main.py -u https://home.yoyu.dev/subscriptionlink --include 香港 Azure --include-group YoYu --exclude Azure`
- `python main.py -u https://home.yoyu.dev/subscriptionlink -t ss`

The parameter priority is as follows:

> -i > -c > -u
> The above sequence indicates that if the parameter has a higher priority, the parameter will be used first, and other parameters will be ignored.
>
> --include > --include-group > --include-remark
> --exclude > --exclude-group > --exclude-remark
> The above sequence indicates that node filtering will be performed in descending order of priority.

### Web UI Usage

```
python web.py
You can now access the WebUI through http://127.0.0.1:10870 
```

## Modify the speed source of Socket mode
  -  Just modify the **link**, **size** attribute in the object with the tag "Default" and the tag "Google", where "link" is the download source and "size" is the file corresponding to the download source. Size (MBytes)
  - For more advanced usage, please refer to the following [Advanced Usage]

## Advanced Usage

 - **Rules**
   - The program has a built-in rule matching mode that allows specific ISPs or nodes in specific regions to use specific speed sources through custom rules for "Socket" test mode.Rules need to be written in config.py. Please see config.py for more details.
- **Custom color**
   - Users can customize the color of the resulting image in config.py. See the config.py file for sample configuration.

## Web Apis
The interfaces encapsulated in web.py are as follows:

|Path| Method | Remark 
|:-:|:-:|:-:|
|/getversion | GET | Get backend version information|
|/status|GET|Get backend status|
|/readsubscriptions|POST|Get subscriptions|
|/readfileconfig|POST|Upload configuration file |
|/getcolors|GET|Get the color configuration owned by the backend|
|/start|POST|Start the test, note that this is a **block request**, will return "done" after the test is completed, **will not have any response**|
|/getresults|GET|Get the current test result. If the "/start" is blocked, if you need to get the result in real time, you should poll the interface, but the time should not be less than 1s, preventing the backend from taking too much performance impact test.

 - /getversion The interface is requested without any parameters. The return example after the request is successful is as follows:

~~~~
{
  "main" : "2.4.1",
  "webapi" : "0.4.1-beta"
}
~~~~
 - /status The interface is requested without any parameters. The return example after the request is successful is as follows:
~~~~
running => Running test.
stopped => Connected to the backend but no test is running
~~~~
 - /readsubscriptions The parameters and examples required to request this interface are returned as follows:
~~~~
POST => {
  "url" : "subscription url"
}
Response (Success) => [
  {
  "type": "Proxy type of this configuration.",
  "config": {"Standard Configuration"}
  }
]
Response (running) => "running" 
Response (Invalid URL) => "invalid url"
~~~~

 - /readfileconfig The parameters and examples required to request this interface are returned as follows:
~~~~
POST => {
  file: file
}
Response (Success) => [
  {
  "type": "Proxy type of this configuration.",
  "config": {"Standard Configuration"}
  }
]
Response (running) => "running" 
Response (Invalid File) => "File type not allowed"
~~~~

 - /getcolors The interface is requested without any parameters. The return example after the request is successful is as follows:
~~~~
[
  {
  "Color Config"
  }
]
~~~~
 - /getresults The interface is requested without any parameters. The return example after the request is successful is as follows:
~~~~
{
  "status" : "running" or "pending" or "stopped",
  "current" : {},  //Node information currently being tested
  "results" : [] //The result array, please refer to the JSON file of the exported result for details.
}
~~~~
 - /start The parameters required to request the interface and return are as follows, **NOTE**, this interface is a **blocking interface**
~~~~
POST => {
  "testMethod" : "Test Method",
  "testMode" : "Test Mode",
  "colors" : "Color Name", //Ignorable
  "sortMethod" : "Sort Method", //Ignorable
  "useSsrCSharp": "bool", //Ignorable
  "group": "Group", //Ignorable
  "configs":[] //Standard configuration array
}
Response (Task Done) => "done"
Response (running) => "running" 
Response (No Configs) => "no configs"
~~~~

- Test Modes

|Mode|Remark|
|:-:|:-:|
|TCP_PING|Only tcp ping, no speed test|
|WEB_PAGE_SIMULATION|Web page simulation test|
|ALL|Full speed test (exclude web page simulation)|

 - Test Methods

|Methods|Remark|
|:-:|:-:|
|ST_ASYNC|Asynchronous download with single thread|
|SOCKET|Raw socket with multithreading|
|SPEED_TEST_NET|Speed Test Net speed test|
|FAST|Fast.com speed test|

