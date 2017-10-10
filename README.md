## 金山云短视频MV主页
金山云提供业内一流的短视频MV效果。

* 提供制作MV的所有技术文档；
* 提供多种MV 资源包，同时适配iOS/Android平台；


### 1. 短视频SDK
* Android短视频home page：[KSYMediaEditorKit Android][android]
* iOS短视频 home page：[KSYMediaEditorKit iOS][iOS]


[android]:https://github.com/ksvc/KSYMediaEditorKit_Android
[iOS]:https://github.com/ksvc/KSYMediaEditorKit_iOS

### 2. MV制作教程
1.0版本中，每个MV资源文件包含以下内容：
* MV配置文件 config.json
* MV需要的 *.mp3资源
* MV需要的 *.mp4资源
* MV缩略图icon.png(仅用于MV选择时的UI显示)

配置好以上资源后，打包成__zip__以备使用

> 注意:**Demo中 文件夹打包的名称必须和zip 包名称一致,否则解压不出来**    
> 注意:可以不把icon.png打包到zip中，直接在demo里面使用


目前我们提供三套主题：Fashion Shots；Lucky You；Party Time，具体效果请参考对应Demo

### 2.1 配置config.json文件
每个zip中必须包含一个config.json 文件，该文件负责配置所有资源的使用

``` json
{
    "ver": "1.0", 
    "loop": true,
    "music": "music.mp3",
    "movie": "movie.mp4",
    "plat": "iOS",
    "animations": {
        "frames": [
            {
                "eid": 1,
                "t": 5000,
                "vtrack": "a",
                "fid": 1,
                "fs": true
            },
            {
                "eid": 1,
                "t": 6000,
                "vtrack": "a",
                "fid": 1,
                "fs": false
            },
            {
                "eid": 2,
                "t": 8000,
                "vtrack": "a",
                "fid": 2,
                "fs": true
            },
            {
                "eid": 2,
                "t": 9000,
                "vtrack": "a",
                "fid": 2,
                "fs": false
            }
        ]
    },
    "filters": [
        {
            "vertex": "",
            "fragment": "\nprecision highp float;\nuniform sampler2D inputImageTexture;\nvarying vec2 textureCoordinate;\nuniform float timeInfo;\nvoid main()\n{\nvec2 newcoor = textureCoordinate;\nfloat timediff = 1.0 - timeInfo;\nif (textureCoordinate.y > timediff){\nnewcoor.y = textureCoordinate.y - timediff;\n}else{\nnewcoor.y = 1.0 - (timediff - textureCoordinate.y);\n}\nvec4 video = texture2D(inputImageTexture, newcoor);\ngl_FragColor = video;\n} ",
            "name": "Up",
            "fid": 1
        },
        {
            "vertex": "",
            "fragment": "\nprecision highp float;\nuniform sampler2D inputImageTexture;\nvarying vec2 textureCoordinate;\nuniform float timeInfo;\nvoid main()\n{\nvec2 newcoor = textureCoordinate;\nif (textureCoordinate.y > timeInfo){\nnewcoor.y = textureCoordinate.y - timeInfo;\n}\nif (textureCoordinate.y > 0.5){\nnewcoor.y = textureCoordinate.y - 0.5;\n}\nvec4 video = texture2D(inputImageTexture, newcoor);\ngl_FragColor = video;\n} ",
            "name": "HalfDown",
            "fid": 2
        }
    ]
}
```

* config.json结构如下：

|key|类型|含义|用途|
|:----:|:------:|:------:|:------:|
|ver|String|版本信息|当前版本1.0|
|loop|Bool|是否循环|true：以mv主视频为基准循环，当mv主音频文件不足时主动循环主音频文件|
|movie |String|主视频路径|MV素材的主视频文件路径，1.0版本不能为空，支持网络文件|
|music|String|主音频路径|MV素材的主音频文件路径，支持网络文件|    
|plat|String|平台信息|Android/iOS|
|animations|Array|mv frams信息|滤镜或其他效果的时间等信息，作为MV的事件驱动|
|filters|Array|滤镜数组信息|作用于MV视频文件或者原始视频的滤镜信息|  

* animations中frames结构如下：

|key|类型|含义|用途|
|:----:|:------:|:------:|:------:|
|eid|int|事件的id| 标识某个事件 (滤镜配对出现, 可不唯一)|
|fid|int|滤镜id|当前frame作用的滤镜的id,和 filters 中的 fid 一一对应(相当于数据库里面的外键)|
|vtrack|String|滤镜作用源|a：作用于原始视频  b：作用于素材主视频|
|t |long|事件的时间点|frame作用的时间点，以mv主视频文件的时间为基准时间，单位ms|  
|fs |bool|标识滤镜开关状态|如果无滤镜 该字段无效(filter switch/filter status/filter show)|  

* filters结构如下：

|key|类型|含义|用途|
|:----:|:------:|:------:|:------:|
|vertex|String|顶点着色器|滤镜的顶点着色器|
|fragment|String|片源着色器|滤镜的片源着色器|
|name|String|滤镜名称| 滤镜名称，若使用SDK内部滤镜，需要为滤镜的类名(iOS)或者包名(Android)|  
|fid|int|滤镜id|用于唯一标识一个滤镜(filter index)|  

如果想自己定制 可以参考这些字段DIY 自己的 MV 资源包 压缩成 zip


也可以持续关注我们的 MV 功能 并在 QQ 群中咨询 后续会推出更多mv主题包

注意:__*zip资源包的名称要和解压完文件夹保持一致否则会导致找不到文件无法加载(短视频demo是这样做的)*__ 这就要求打包zip的时候要保证 __文件夹名称和 zip 包名称一致__


例如: xxxx/xxx/a.zip  解压完的文件夹应该是:xxxx/xxx/a/


### 3. MV资源下载

目前我们支持从 github 上托管的方式去下载和使用 MV,可以把 MV资源搞到工程里面 然后解压到沙盒,但这种搞法会导致 包大小很大.

最好的方式是:自己搞成 H5页面来管理MV的下载，这样动态下载的情况下可以很有效的解决包大小的问题.

如果需要我们提供MV相关的下载和更高级的制作,请联系我们商务人员.


### 4. 反馈与建议
### 4.1 反馈模板  

| 类型    | 描述|
| :---: | :---:| 
|SDK名称|KSYMediaEditorMV|
| SDK版本 | v1.1.0|
| 设备型号  | oppo r9s  |
| OS版本  | Android 6.0.1 |
| 问题描述  | 描述问题出现的现象  |
| 操作描述  | 描述经过如何操作出现上述问题                     |
| 额外附件   | 文本形式控制台log、crash报告、其他辅助信息（界面截屏或录像等） |

### 4.2 联系方式
- 主页：[金山云](http://www.ksyun.com/)
- 邮箱：<zengfanping@kingsoft.com>
- QQ讨论群：574179720
- Issues: <https://github.com/ksvc/KSYMediaEditorMV/issues>
