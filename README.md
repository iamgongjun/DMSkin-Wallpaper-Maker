# DMSkin-LiveWallpaper
Windows 桌面动态壁纸 视频壁纸

![](https://img.shields.io/badge/.NET-%3E%3D3.5-brightgreen.svg)
![](https://img.shields.io/badge/version-1.0.0.0-blue.svg)
![](https://img.shields.io/badge/license-MIT-green.svg)

#### 开源的动态桌面壁纸。

<a href="http://on9mnekns.bkt.clouddn.com/desktopdemo.mp4" target="_blank">
   <img src="https://raw.githubusercontent.com/944095635/DMSkin-LiveWallpaper/master/Screenshot/demo.gif">
</a>

<img src="https://raw.githubusercontent.com/944095635/DMSkin-LiveWallpaper/master/Screenshot/Debug.png">

## 【前言】 
Wallpaper.Maker 采用WIN32 接口实现视频嵌入桌面。

Wallpaper.Maker 最开始采用的是迅雷Aplayer,CPU使用率颇高15%-30%,现阶段改为[Vlc.DotNet](https://github.com/ZeBobo5/Vlc.DotNet)CPU使用率降低至1%-5%(跟个人电脑配置有关)
#### 【项目结构】


| 项目名称                | 描述   |特性   |缺点      |
| :----:              | :---:          | :----:     | :----:     |
| Wallpaper.Maker |主程序   |  -   |  -  |
| DMSkin.WPF  |  UI库       | UI的一些封装 |  开发时需要从Github或Nuget获取       |
| Wallpaper.Player |Vlc解码器   |  HTTP支持,超多格式(GIF,MP4.AVI.FLV.WEBM.RMVB)  |  解码库体积(+~30MB)  |
| Wallpaper.Server |通讯协议   |  负责主程序和解码器之间的通讯  |  NamedPipe命名管道通讯  |

#### 【执行逻辑】
- `服务`是作为程序之间数据交互的基础,通讯原理是NamedPipe命名管道通讯。

- `主程序`程序启动的时候，会根据`服务PlayServer`检测系统进程中是否存在`VLC解码器`。

- 如果存在对应`解码器`进程，程序不会执行任何操作(如果不存在，`主程序`会启动解码器)。

- `主程序`与`解码器`分离，减少内存消耗。主程序退出之后`解码器`依然会运行(主程序中可以`关闭解码器进程`)。

#### 【注意 源码编译&亚克力玻璃材质】

<del>- 基于VS 2017 旗舰版开发，.NET 4.5.5开发环境（理论可修改至.NET 3.5），源码包括一些c# 6.0+语法，如果你在VS 2015甚至更低的VS版本上编译不通过的话，请自行修改中源码不兼容的部分。

- 截图是Windows 10 秋季创作者更新 中的亚克力玻璃 效果,其他系统请自行测试,相关内容请自行搜素Fluent Design System</del>

#### 【使用&修改】

#### 1. 下载使用

你可以直接下载压缩包解压运行。

[迷你版](https://github.com/944095635/DMSkin-LiveWallpaper/releases/download/1.0.0.0/Build-MINI.zip)

[完整版](https://github.com/944095635/DMSkin-LiveWallpaper/releases/download/1.0.0.0/Build-Max.zip) & [解码库](http://aplayer.open.xunlei.com/codecs.zip)

#### 2. 下载源码 然后自己编译
[下载源码](https://codeload.github.com/944095635/DMSkin-LiveWallpaper/zip/master) 点击 `DMSkin.Wallpaper.sln` 打开项目。

#### 【迅雷解码器 开发环境】 [迅雷Aplayer官网](http://aplayer.open.xunlei.com/)
编译DMSkin.Player.Xunlei需要安装SDK

###### 安装SDK用于开发 [下载开发SDK](http://aplayer.open.xunlei.com/APlayerSDK.zip)
````csharp
将SDK下载解压到D盘根目录 D:\APlayerSDK
以管理员启动命令提示行 CMD.EXE
输入命令:
    D: 执行
    CD D:\APlayerSDK 执行
    install.bat 执行
结果:
    复制了8个文件 OK
````

###### 下载完整解码库用于播放 [下载解码库CodeCS](http://aplayer.open.xunlei.com/codecs.zip)
````csharp
将codecs下载解压到软件根目录(保留codecs文件夹)
````

#### 【自定义解码器】
虽然目前只内置了2种解码器(MediaElement+Aplayer)

但是你可以通过编写代码实现自己的解码器(例如用VLC解码器)
````csharp
/// <summary>
/// 放到解码器初始化的地方-解码器必须一直处于运行状态 Winform/WPF/Console 都支持
/// </summary>
NamedPipeListenServer server = new NamedPipeListenServer("Play.Server")
{
    ProcessMessage = ProcessMessage
};
server.Run();

/// <summary>
/// 处理请求-例如处理客户端 Open URL的请求
/// </summary>
public void ProcessMessage(ServerMsg msg, NamedPipeServerStream pipeServer)
{
    switch (msg.ServerMsgType)
    {
         case ServerMsgType.OpenUrl:
            ///让你的播放器执行播放视频操作
            break;
    }
    pipeServer.Close();
}
````

## 【联系】
欢迎加入我们：

- **[C# .NET (2000人) QQ交流群](http://qm.qq.com/cgi-bin/qm/qr?k=reTIeglEELMIW267mOO7amouFFwhJwwP)**

- **DMSkin QQ交流群: 194684812**

- **WPF 课程学习群 (收费): 611509631**
- **<a href="http://dmskin.lolimay.cn" target="_blank">联系作者</a>**
- **[DMSkin官方网站](http://www.dmskin.com)**

## 捐赠
如果你觉得这个框架真的对你很有帮助，欢迎给我捐赠，这将鼓励我做的更好!

<img src="http://dmskin.com/pay.jpg" width="500">

## 【更新日志】

### 1.0.0.1 (2018-11-24)

1. 因迅雷Aplayer解码器性能低,系统兼容性差，故舍弃。
2. 解码器改用[Vlc.DotNet](https://github.com/ZeBobo5/Vlc.DotNet),CPU使用率从15-30%降低到3%-5%。

### 1.0.0.0 (2018-10-25)

1. 解码器初步完成。
2. 操作软件初步完成。

## MIT License
Copyright © 2018 <copyright holders>

Permission is hereby granted， free of charge， to any person obtaining a copy of this software and associated documentation files (the “Software”)， to deal in the Software without restriction， including without limitation the rights to use， copy， modify， merge， publish， distribute， sublicense， and/or sell copies of the Software， and to permit persons to whom the Software is furnished to do so， subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”， WITHOUT WARRANTY OF ANY KIND， EXPRESS OR IMPLIED， INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY， FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM， DAMAGES OR OTHER LIABILITY， WHETHER IN AN ACTION OF CONTRACT， TORT OR OTHERWISE， ARISING FROM， OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
