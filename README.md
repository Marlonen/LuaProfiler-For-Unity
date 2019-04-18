## Lua Profiler For Unity
<br/>

[中文说明](#jump)

### Purpose
**Unity + Lua** script is now most popular incremental update frameWork for mobile game in China,However, since there haven't been good tool to monitor the cpu and memory usage of lua vm, lots of developers have no idea to optimize their code,so there are many potential risks in lua codes.<br>
this tool is designed to support an **easy-to-use profiler** for Unity that help finding bottleneck and make your game more fast and stable.

### Use renderings
![](doc/use.gif)

##

### Contact
If you find any bug or have any suggests join the QQ group：[882425563](https://jq.qq.com/?_wv=1027&k=5QkOBSc) to contact us

### Deploy and Install
Lua Profiler For Unity supports **XLua**、**SLua**、**ToLua** and also a remote profiler tool so it supports **Windows**、**Android**、**IOS** On-device Profiler.

- You must open two Unity projects,one for game client ,one for editor server
- Open **LuaProfiler** folder
- Copy **LuaProfilerClient** folder to you game project content,if your C# Lua script is in **Plugins** folder,Copy **LuaProfilerClient** to **Plugins**.This Tool must make sure That code must in the same DLL which has C# lua codes.
- Use **Unity5.6 or newer version** Unity version to open **LuaProfilerServer** as a Unity project
- If your Unity version is below 5,call the following code before start the game.
```
MikuLuaProfiler.HookLuaSetup.OnStartGame();
```

### Datas Descriptions

| Name                    | Descriptions                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------------   |
| `Overview`              | function name                                                                                             |
| `totalLuaMemory`        | The sum of all Lua GCs produced by this function If GC happens then the value will not be very accurate   |
| `self`                  | The amount of GC produced by the function itself                                                          |
| `totalMonoMemory`       | The sum of all Mono GCs produced by this function If GC happens then the value will not be very accurate  |
| `self`                  | The amount of GC produced by the function itself                                                          |
| `currentTime`           | The time it takes for the function to run in current frame                                                |
| `averageTime`           | Count the average value of the time spent on the function                                                 |
| `totalTime`             | All the time consumed by this function                                                                    |
| `LuaGC`                 | Lua GC generated by the current frame                                                                     |
| `MonoGC`                | Mono GC generated by the current frame                                                                    |
| `totalCalls`            | The number of runs of this function after the game starts                                                 |
| `Calls`                 | The number of executions of the current frame of the function                                             |

### Theory
It use mono.ceil's IL inject feature(also use in XLua),inject the profiler code to game code

## 

### Tutors

#### Config your client

Open windows by **"Window->Lua Profiler Window"**, toggle profiler's feature and configure the server ip address.
## 
![](doc/config_client.png)<br/>

Select the kind of code you want profiler,C# code color is green,and lua code color is blue.<br/>

#### Open server
Also open windows by **"Window->Lua Profiler Window"**, then click **OpenService**,wait for client connects
## 
![](doc/config_server.png)

## 
![](doc/profiler.gif)
## 

#### Menu in Game
Press `DEL` or touch `four fingers` in screen to show this menu,you can change ip to connect and show more details of profiler info</br>
![](doc/menu.png)</br>


#### Monitor registry
Programmers tend to forget to release Lua objects that are cached by C#.For example, when XLua calls LuaEnv.Dispose, it throws the exception "try to dispose a LuaEnv with C# callback!"<br/>

The reason is often caused by the following code
```
LuaTable tb = ...
Action action = tb.rawget<Action>("action");
a();
tb.Dispose(); //This line of code tends to forget to call
a= null;      //This line of code tends to forget to call
```
in Lua
```
// The lua table tends to forget to call dispose
CS.UIBehaviour.luauiTable = {}
// The callback tends to Unregister
CS.UIButton.OnClick = function() print("test") end
```
They can be released by C#'s destructor, but because in C# is just an index, the memory footprint is small then C# will not be GC immediately, but the memory usage on Lua is very large.
This tool will provide real-time detection of the registry to help locate memory leaks quickly.<br/>

![](doc/register.gif)<br/>

You can search history in record line<br/>
![](doc/register_record.gif)<br/>

And the `add`、`remove` history<br/>
![](doc/reg_history.png)<br/>

You can set index '__name' to the lua table,monitor will replace the table code by the value of this index
```
CS.UIBehaviour.loginUI = { __name = "LoginUI" }
```

#### Charts
- Toggle `LuaChart` to open lua memory chart,line color is blue.
- Toggle `MonoChart` to open mono memory chart,line color is green.
- Toggle `FpsChart` to open fps chart,line color is orange.
- Toggle `PssChart` to open pss chart,line color is red.
- Toggle `PowerChart` to open power chart,line color is brown.

#### Record mode
Click **Record** button, when game connect to server, toggle **StartRecord** to start or stop record.

##### Record button feature

##
![](doc/use3.gif)<br/>

- drag slider to modify samples
- click __'<'__ 、 __'>'__ to increase or discrease frames one by one
- click __'<<'__ 、 __'>>'__ to fast locate the frames control by 
**Capture Lua GC**、**Capture Mono GC**、**Frame Count**
- stop record and press left or right arrow keybord to increase or discrease frames one by one

##
![](doc/record.gif)

### On-device Profiler
Set macro **USE_LUA_PROFILER** to inject profiler code in you App.</br>

#### Android
set IP:127.0.0.1 port:2333. Connect your android phone with a USB cable.Execute the following instructions in cmd windows.</br>
```
adb reverse tcp:2333 tcp:2333
```
Execute your app.</br>

If you want to use **luac code or luajit bytecode** ,use **InjectLua.exe** in folder tools To inject the lua profiler code.</br>

```
InjectLua.exe "inpath" "outpath"
```

------
<span id="jump"></span>

### 目的
**Unity + Lua** 脚本现在是中国最流行的增量更新框架，但是，由于没有很好的工具来监控lua vm的cpu和内存使用情况，很多开发人员都不知道如何优化他们的代码，所以在lua代码中存在许多潜在的风险。
此工具旨给Unity 提供一个易于使用的lua性能分析器用于查找代码中的性能瓶颈并使您的游戏更快速、稳定。

### 使用示意图
![](doc/use.gif)

##

### 联系
如果您发现任何错误或有任何建议加入QQ群：[882425563](https://jq.qq.com/?_wv=1027&k=5QkOBSc) 与我们联系

### 部署和安装
Lua Profiler For Unity支持 **XLua**、**SLua**、**ToLua** ，本工具是基于远程Socket的Profiler工具，因此它支持Android，IOS 的真机 Profiler。

- 您必须打开两个Unity项目，一个放进游戏客户端，一个用于展示数据
- 打开 **LuaProfiler** 文件夹
- 将 **LuaProfilerClient** 文件夹复制到游戏项目内容，如果您的C＃Lua脚本位于Plugins文件夹中，则将 **LuaProfilerClient** 复制到插件。此工具必须确保该代码必须位于具有C＃lua代码的同一DLL中。
- 使用 **Unity5.6 or newer version** Unity版本将 **LuaProfilerServer** 作为Unity项目打开
- 如果Unity版本低于5，请在开始游戏前调用以下代码。
```
MikuLuaProfiler.HookLuaSetup.OnStartGame();
```

### 数据说明

| Name                    | Descriptions                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------------   |
| `Overview`              | 函数名称                                                                                                  |
| `totalLuaMemory`        | 此函数生成的所有Lua GC的总和                                                                              |
| `self`                  | 函数本身产生的GC量                                                                                        |
| `totalMonoMemory`       | 此函数生成的所有Mono GC的总和                                                                             |
| `self`                  | 函数本身产生的GC量                                                                                        |
| `currentTime`           | 函数在当前帧中运行所需的时间                                                                              |
| `averageTime`           | 计算在函数上花费的时间的平均值                                                                            |
| `totalTime`             | 此功能消耗的所有时间                                                                                      |
| `LuaGC`                 | 由当前帧生成的Lua GC                                                                                      |
| `MonoGC`                | 由当前帧生成的Mono GC                                                                                     |
| `totalCalls`            | 游戏开始后此功能的运行次数                                                                                |
| `Calls`                 | 函数当前帧的执行次数                                                                                      |

### 理论
它使用mono.ceil的IL注入功能（也用于XLua），将profiler代码注入游戏代码

## 

### 使用教程

#### 配置客户端

通过 **"Window->Lua Profiler Window"**打开窗口，切换分析器的功能并配置服务器IP地址。
## 
![](doc/config_client.png)<br/>

选择想要分析器的代码类型，C＃代码颜色为绿色，lua代码颜色为蓝色。<br/>

#### Open server
同理可以通过 **Window->Lua Profiler Window**打开窗口，然后单击 **OpenService**，等待客户端连接
## 
![](doc/config_server.png)

## 
![](doc/profiler.gif)
## 

#### 游戏中的菜单
按 `DEL` 或 四手指触碰屏幕 以显示此菜单，您可以在游戏中随时中断或者重启连接 以及更换IP地址</br>
![](doc/menu.png)</br>


#### 监控注册表
程序员往往忘记释放由C＃缓存的Lua对象。例如，当XLua调用LuaEnv.Dispose时，它会抛出异常 "try to dispose a LuaEnv with C# callback!"<br/>

原因通常是由以下代码引起的
```
LuaTable tb = ...
Action action = tb.rawget<Action>("action");
a();
tb.Dispose(); // 忘记Dipose了
a= null;      //忘记置空变量了
```
在Lua
```
// 忘记置空变量了
CS.UIBehaviour.luauiTable = {}
// 忘记取消注册函数了
CS.UIButton.OnClick = function() print("test") end
```
它们可以被C＃的析构函数释放，但是因为在C＃中只是一个索引，内存占用很小，所以C＃不会立即成为GC，但是Lua上的内存使用量非常大。此工具将提供对注册表的实时检测，以帮助快速定位内存泄漏。<br/>

![](doc/register.gif)<br/>

您可以在记录行中搜索历史记录<br/>
![](doc/register_record.gif)<br/>

以及 `add`、`remove` 的变化历史<br/>
![](doc/reg_history.png)<br/>

您可以将索引'__name'设置为lua表，monitor将使用此索引的值替换表代码
```
CS.UIBehaviour.loginUI = { __name = "LoginUI" }
```

#### 图表
- 点击 `LuaChart` 打开lua内存图表，线条颜色为蓝色。
- 点击 `MonoChart` 打开单声道内存图表，线条颜色为绿色。
- 点击 `FpsChart` 打开fps图表，线条颜色为橙色。
- 点击 `PssChart` 打开pss图表，线条颜色为红色。
- 点击 `PowerChart` 打开电量图表，线条颜色为棕色。

#### Record mode
单击 **Record** 按钮，当游戏连接到服务器时，切换 **StartRecord** 以开始或停止录制。

##
![](doc/use3.gif)<br/>

- 拖动滑块以修改样本
- 单击 __'<'__ 、 __'>'__ 逐个增加或分离帧
- 单击 __'<<'__ 、 __'>>'__ 以快速定位
**Capture Lua GC**、**Capture Mono GC**、**Frame Count**
- 停止记录并按左或右箭头键盘逐个增加或分离帧

##
![](doc/record.gif)

### DIFF 两个不同时期的Lua变量
选取一个适当的时机,比如配置表加载完后，准备打开一个新的UI的时候点击**MarkLuaRecord**按钮
##
![](doc/mark.png)<br/>
打开UI然后关闭并卸载掉UI资源，点击**DiffRecord**，工具将会对Mark时候的Lua变量与**DiffRecord**时候的变量进行差异比较
##
![](doc/diff.png)<br/>
点击**ShowLog**按钮，将会把文件存盘打开之后将把对于变量的类型以及引用路径打印出来。 注意** _G **表示全局表,** _R **表示被C#引用的对象
##
![](doc/diff_log.png)<br/>
**Destroy null values**为Unity已经将资源释放，而Lua仍然引用的变量，这个是主要的资源泄漏要重点关注。
##
![](doc/null_object.png)<br/>
![](doc/search.png)<br/>


### 真机Profiler
设置宏 **USE_LUA_PROFILER** 以在App中注入探查器代码。</br>

#### Android
设置IP：127.0.0.1 port:2333. 。使用USB线连接Android手机。在cmd窗口中执行以下指令</br>
```
adb reverse tcp:2333 tcp:2333
```
执行你的应用程序。</br>

如果要使用luac代码或luajit字节码，请在文件夹工具中使用InjectLua.exe注入lua profiler代码。</br>

```
InjectLua.exe "inpath" "outpath"
```

### Use Case
![](doc/ljjc.jpg)

## 
### Thanks
[easy66](https://github.com/easy66) <br/>
[Xavier](https://github.com/starwing) <br/>
[Jay](https://github.com/Jayatubi) <br/>
[ZhangDi](https://github.com/ZhangDi2018) <br/>
and all members in qq group [LuaProfiler](https://jq.qq.com/?_wv=1027&k=5QkOBSc)

## 
![](doc/meizi.gif)
