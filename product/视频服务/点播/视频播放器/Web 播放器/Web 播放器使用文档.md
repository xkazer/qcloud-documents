## 前言
本文档是介绍腾讯云视频点播服务的 Web 播放器，它可以帮助腾讯云客户通过灵活的接口，快速与自有 Web 应用集成，实现视频播放功能，本文档适合有一定 Javascript 语言基础的开发人员阅读。
## 能力介绍
腾讯云视频点播 Web 播放器是通过 HTML5  `<video>`  标签以及 Flash 实现视频播放。在浏览器不劫持视频播放的情况下，实现了视频播放效果的多平台统一体验，并结合腾讯云点播视频服务，提供防盗链、HLS 加密播放等功能。

### 视频格式支持

| 播放格式    | PC 浏览器环境                      | 移动端浏览器环境                        |
|------------|-----------------------------------|---------------------------------------|
| HLS（m3u8）| 支持（IE11/10/9/8 需要开启 Flash） | 支持（Android 4+、iOS 原生支持）        |
| MP4   	 | 支持（IE8 需要开启 Flash）          | 支持（Android 4.4+、iOS 原生支持）       |

### 平台支持
**桌面端**支持最新版本的现代浏览器 Chrome，Firefox，Safari ，Edge，QQ 浏览器，以及非现代浏览器 IE11/10/9/8（IE11/10/9/8 需要开启 Flash，只支持 Win7 IE8）
**移动端**支持 HTML5 `<video>` 标准的浏览器，比如 Android Chrome，iOS Safari，微信，手机 QQ，手机 QQ 浏览器等
使用本播放器，同一段代码可以自动实现桌面浏览器和手机浏览器的自适应切换，播放器内部会自动区分平台，并使用最优的播放方案。例如：在 IE11/10/9/8 浏览器中会使用 Flash 播放器以实现其不支持 HTML5 播放 HLS 的能力，在 Chrome 等现代浏览器中优先使用 HTML5 技术实现视频播放，而手机浏览器上会使用 HTML5 技术实现视频播放。
### 点播平台的转码服务
由于 MP4 和 HLS（m3u8）是目前在桌面浏览器和手机浏览器上支持程度最广泛的格式，所以腾讯云的视频点播平台最终会把上传的视频发布为 MP4 和 HLS（m3u8） 格式。
## 准备工作
### step 1：开通服务
在 [腾讯云官网](https://cloud.tencent.com/) 注册腾讯云账号，然后开通**点播**服务。
### step 2：上传文件
点播服务开通之后，进入 [点播视频管理](https://console.cloud.tencent.com/video/videolist) 就可以上传新的视频文件，如果您没有开通点播服务，则无法操作这一步骤。
### step 3：获取 fileID 与 appID
上传完视频并转码之后，您就可以在视频管理页面查到文件的 fileID ，这个是播放器播放视频的最基本信息，同时您的 appID 也可以在视频管理页面查看到。下图中的两个 ID，左边一个是视频文件的 fileID，另一个是您的 appID。
![](//mc.qcloudimg.com/static/img/fcad44c3392b229f3a53d5f8b2c52961/image.png)
## 初始化 Web 播放器
在准备工作完成后，通过以下 3 个步骤，您就可以在您的网页上添加一个视频播放器。
### step 1：在页面中引入文件
在合适的地方引入播放器样式文件与脚本文件
```
 <link href="//imgcache.qq.com/open/qcloud/video/tcplayer/tcplayer.css" rel="stylesheet">
 <script src="//imgcache.qq.com/open/qcloud/video/tcplayer/tcplayer.min.js"></script>
```
>**注意事项：**
> * 如果需要在 Chrome Firefox 等现代浏览器中通过 HTML5 播放 hls，需要在 tcplayer.min.js 之前引入 hls.js。
```
 <script src="//imgcache.qq.com/open/qcloud/video/tcplayer/lib/hls.min.0.8.8.js"></script>
```

### step 2：放置播放器容器
在需要展示播放器的页面位置加入播放器容器，例如：在 index.html 中加入如下代码（容器 ID 以及宽高都可以自定义）。
```
<video id="player-container-id" width="414" height="270" preload="auto" playsinline webkit-playsinline>
</video>
```
>**注意事项：**
> * 播放器容器必须为 `<video>` 标签。
> * 示例中的 player-container-id 为播放器容器的ID，可自行设置。
> * 播放器容器区域的尺寸，建议通过 CSS 进行设置，通过 CSS 设置比属性设置更灵活，可以实现例如铺满全屏、容器自适应等效果。
> * 示例中的 preload 属性规定是否在页面加载后载入视频，通常为了更快的播放视频，会设置为 auto，其他可选值：meta（当页面加载后只载入元数据），none（当页面加载后不载入视频），移动端由于系统限制不会自动加载视频。
> * playsinline webkit-playsinline 这几个属性是为了在标准移动端浏览器不劫持视频播放的情况下实现行内播放，此处仅作示例，请按需使用。
> * 设置 x5-playsinline 属性在 TBS 内核会使用 X5 UI 的播放器。

### step 3：初始化代码
在页面初始化的代码中加入以下初始化脚本，传入在准备工作中获取到的 fileID 与 appID。
```
var player = TCPlayer('player-container-id', { // player-container-id 为播放器容器ID，必须与html中一致
    fileID: '4564972818956091133', // 请传入需要播放的视频filID 必须
    appID: '1253668508' // 请传入点播账号的appID 必须
  });
```
>**注意事项：**
>* 需要播放的视频需经过腾讯云转码，原始视频无法保证在浏览器中正常播放。

### 完整的示例页面：
[示例代码链接](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-base.html)
## 功能使用说明
下面将对播放器的部分功能进行详细说明，包括最佳实践与注意事项。
### 播放器尺寸设置
这里介绍几种给播放器设置尺寸的方法

* 可以给 `<video>` 标签设置 width height 属性，width height 的属性值是以像素计量的（比如 width = "100px" 或 width= 100），不能设置百分比。
*	可以通过 CSS 设置尺寸，支持像素、百分比等类型的值（比如：width:"100px" 或 width:"100%" ）。

如果不设置宽高，播放器在获取到视频的分辨率后，将会以视频的分辨率设置播放器的显示尺寸，如果浏览器的可视区域的尺寸小于视频分辨率，会造成播放器区域超出浏览器的可视区域，所以通常不建议这样做。最佳实践为通过 CSS 设置播放器的尺寸。
熟练运用 CSS 可以实现铺满全屏、容器自适应等效果。

示例：
[CSS 设置尺寸](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-size.html)
[铺满网页可视区域](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-size-full-viewport.html)
[等比率自适应](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-size-adaptive.html)

### 播放多种清晰度
1、 首先需要在上传视频时对视频进行转码操作，如下图
![](https://main.qcloudimg.com/raw/2f171baf670cd87b2e5248c9e93d9d60.png)

2、 视频转码后，将会生成多种清晰度的文件，在【控制台】>【视频管理】视频列表中单击视频将会看到如下图
![](https://mc.qcloudimg.com/static/img/3a60f37c5c6d429bffb7e96023c948e9/image.png)

经过以上两个步骤，该视频已具备多种清晰度，使用 fileID appID 在腾讯云点播播放器中播放即可。
清晰度选择效果如下图：
![](https://mc.qcloudimg.com/static/img/d35731fae08327c66602ee3b7be77c2c/image.png)

>**注意事项：**
> * 在浏览器劫持视频播放的情况下，该功能无法使用。通常在移动浏览器中，浏览器会劫持视频播放，用浏览器自带的播放器替代。

示例：
[多种清晰度](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-base.html)
### 指定播放清晰度
这里分为两种情况：指定播放某个清晰度和让播放器默认播放某个清晰度

#### 指定播放某个清晰度
通过播放器的 definition 参数指定播放某个清晰度

| 可选值 | 说明     |
|-------|----------|
| 10    | MP4 手机 |
| 20    | MP4 标清 |
| 30    | MP4 高清 |
| 40    | MP4 超清 |
| 210   | HLS 手机 |
| 220   | HLS 标清 |
| 230   | HLS 高清 |
| 240   | HLS 超清 |

在下面的示例中，将指定播放 MP4 手机清晰度视频：
[指定播放清晰度](http://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-definition.html)

>**注意事项：**
> * 通过该方式指定播放某个清晰度，播放器将不会出现清晰度切换菜单，如需要指定播放某个清晰度的同时使用清晰度切换菜单，请采用下面“指定播放器默认播放某个清晰度”的方法。

#### 指定播放器默认播放某个清晰度

* 在“【控制台】-【点播】-【分发播放设置】-【Web 播放器管理】”选定某个播放器配置进行设置默认画质
![](https://main.qcloudimg.com/raw/471ba3bad21e06832f472b0899244ae4.png)

* 在“【控制台】-【视频管理】”将视频与某个播放器配置进行关联，在使用腾讯云播放器播放该视频时，将会使用关联的播放器配置。
![](https://mc.qcloudimg.com/static/img/82dc40ee75db110ab2d77749ec059d80/image.png)

>**注意事项：**
> * 如果默认清晰度的视频不存在，则获取该视频的清晰度列表中第一个文件进行播放。比如播放器配置默认播放超高清，但是视频只有标清和高清，这时会播放标清视频。
> * 控制台播放器配置在设置后，大概需要10分钟使所有 CDN 节点生效该配置。
> * 在浏览器劫持视频播放的情况下，该功能无法使用。

### 续播功能
开启续播功能的前提，必须通过 fileID 播放。有了唯一的 fileID，播放器才能记录该视频的播放时长，当播放未结束时关闭页面，在同一个浏览器中再次打开播放页面，可从上次观看的时间点继续播放。
通过以下参数开启续播功能：

```
var player = TCPlayer('player-container-id', {
    fileID: '', // 请传入需要播放的视频 filID 必须
    appID: '', // 请传入点播账号的 appID 必须
    plugins:{
        ContinuePlay: { // 开启续播功能
          // auto: true, //[可选] 是否在视频播放后自动续播
          // text:'上次播放至 ', //[可选] 提示文案
          // btnText: '恢复播放' //[可选] 按钮文案
        },
      }
  });
```
开启成功后将会看到的效果如下图：
![](https://mc.qcloudimg.com/static/img/e155be329a6fec959e1ad6b361add390/image.png)

示例：
[续播](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-continue-play.html)

>**注意事项：**
> - 必须通过 fileID appID 播放经过腾讯云转码后的视频，才能使用该功能。
> - 该功能通过通过 localStorage 存储播放时间点，浏览器需支持该特性。
> - 在浏览器劫持视频播放的情况下，该功能无法使用。
> - 该功能不是多端多浏览器互通的，比如在 PC 浏览器上没看完，不能在移动端浏览器上续播或者在 PC 上另一个浏览器续播，需额外的接口，可以自行开发。

### 设置播放器 logo
腾讯云点播播放器支持配置播放器 logo，可以在【控制台】>【Web 播放器管理】选定某个播放器配置，单击外观栏目进行设置 logo 信息。
设置 logo 信息后，使用该播放器配置播放视频时，将会在指定位置显示 logo。

示例：
[显示 Logo](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-logo.html)

>**注意事项：**
> - 控制台播放器配置在设置后，大概需要10分钟使所有 CDN 节点生效该配置。
> - 在浏览器劫持视频播放的情况下，设置的 logo 将无法显示。

### 图片贴片功能
腾讯云点播播放器支持配置片头、片中暂停、片尾显示图片贴片,并且可以添加超链接。可以在【控制台】>【Web 播放器管理】选定某个播放器配置，单击贴片栏目进行设置贴片信息。
默认的贴片显示样式为水平垂直居中显示，如果图片的尺寸大于播放器显示区域，将按播放器的宽度等比缩放图片，水平居中显示图片，图片超出播放器区域部分将无法显示。
可以通过 CSS 自定义贴片的显示样式。
```
.tcp-image-patch-start{} /* 片头贴片样式Class */
.tcp-image-patch-pause{} /* 片中贴片样式Class */
.tcp-image-patch-ended{} /* 片尾贴片样式Class */
```

示例：
[图片贴片](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-image-patch.html)

>**注意事项：**
> * 贴片建议使用体积不超过50KB 且尺寸不超过播放器显示区域的图片，避免因图片过大影响视频初始化速度。
> * 控制台播放器配置在设置后，大概需要10分钟使所有 CDN 节点生效该配置。
> * 在浏览器劫持视频播放的情况下，设置的贴片将无法显示。

### 缩略图预览
腾讯云点播播放器支持缩略图预览，开启该功能有两种方式：
1. 通过服务端 API 生成视频的缩略图与 VTT 文件，相关文档[视频截图综述-雪碧图](https://cloud.tencent.com/document/product/266/11702)
2. 自行生成缩略图文件与 VTT 文件，并将两个文件的 URL 传递给播放器，参考示例“缩略图预览-传入缩略图与 VTT 文件”

开启成功的效果如下图：
![](https://main.qcloudimg.com/raw/cf668bbf1a991c347fbeacb6555831c1.png)

示例：
[缩略图预览-服务端生成](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-vtt-thumbnail.html)
[缩略图预览-传入缩略图与 VTT 文件](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-vtt-thumbnail-src.html)
>**注意事项：**
> * 该功能仅支持桌面端浏览器。
> * 在浏览器劫持视频播放的情况下，该功能无法使用。
> * 生成的缩略图越多，进度条预览越精确，而缩略图越多，图片越大加载越慢，需要取舍平衡。

### 切换 fileID 播放
通过实例化对象的 loadVideoByID(args) 方法，可以更换视频进行播放。该方法支持的参数如下：
```
player.loadVideoByID({
  fileID: '', // 请传入需要播放的视频 filID 必须
  appID: '', // 请传入点播账号的 appID 必须
  t: '', // 参考 Key 防盗链说明
  us: '', // 参考 Key 防盗链说明
  sign:'', // 参考 Key 防盗链说明
  exper:'' // 参考 试看功能说明
});
```

示例：
[切换 fileID 播放](http://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-change-file.html)

### 镜像功能
激活镜像功能，可以让视频画面镜像翻转，如下图所示：
![](https://main.qcloudimg.com/raw/d5886d7d550be72b608077f341299610.png)

开启右键菜单镜像选项：
```
var player = TCPlayer('player-container-id', {
  fileID: '', // 请传入需要播放的视频 filID 必须
  appID: '', // 请传入点播账号的 appID 必须
  plugins: {
    ContextMenu: {
      mirror: true
    }
  }
});
```

示例：
[镜像功能](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-mirror.html)

>**注意事项：**
> * 在浏览器劫持视频播放的情况下，该功能无法使用。

### 进度条标记
通过服务端 API 对视频[增加打点信息](https://cloud.tencent.com/document/product/266/14190)，可以在播放器中开启显示进度条标记，如下图所示：
![](https://main.qcloudimg.com/raw/70d880065adce22cb64270f4999558f8.png)

播放器开启方式：
```
var player = TCPlayer('player-container-id', {
  fileID: '', // 请传入需要播放的视频 filID 必须
  appID: '', // 请传入点播账号的 appID 必须
  plugins: {
    ProgressMarker: true
  }
});
```

示例：
[进度条标记](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-progress-marker.html)

>**注意事项：**
> * 该功能仅支持桌面端浏览器。
> * 在浏览器劫持视频播放的情况下，该功能无法使用。

### HLS 自适应码率播放
HLS 规范的 Master Playlist 可以根据网络速度自适应码率播放，在视频下载过程中，如果网络速度满足下载高码率的 ts 分片时，播放器将切换播放高码率的 ts 分片，反之播放低码率的 ts 分片。移动端和桌面端大部分浏览器都支持该特性。
使用 HLS Master Playlist 需要通过服务端 API 对视频进行转码，视频转码生成 HLS Master Playlist 才可以开启该特性，请查看相关文档[视频转码综述](https://cloud.tencent.com/document/product/266/11701)。
播放 HLS Master Playlist 时，播放器的清晰度选择功能将会变成选择特定的码率或者根据网络速度自动选择。如下图所示：
![](https://main.qcloudimg.com/raw/339d7dfb3a4d247deb70460edac35a0e.png)

>**注意事项：**
> * 由于移动端浏览器没有提供相应的接口，移动端无法手动选择特定的码率。
> * Flash 播放模式下不支持手动选择特定的码率。
> * 如果视频转码时输出了 HLS Master Playlist 播放器将会优先使用 HLS Master Playlist 进行播放。

示例：
[HLS Master Playlist](https://imgcache.qq.com/open/qcloud/video/tcplayer/examples/vod/tcplayer-vod-hls-masterplaylist.html)

### Referer 防盗链
开启流程请看 [Referer 防盗链说明文档](https://cloud.tencent.com/document/product/266/14046)

播放器初始化需增加参数如下：
```
var player = TCPlayer('player-container-id', {
     fileID: '', // 请传入需要播放的视频filID 必须
     appID: '', // 请传入点播账号的appID 必须
     flash:{
         swf: '//[腾讯云隔离域名]/vod-player/[appID]/[fileID]/tcplayer/player.swf' //swf 文件地址 必须
     }
   });
```
需传入 swf url，如果浏览器使用 Flash 播放，将会去这个地址获取 Flash 播放器。Flash 播放器发起视频请求时，请求的 Referer 会带上该 url 或者带上页面的 url。

>**注意事项：**
> * 播放器在 Flash 模式下发起视频请求的 Referer 在 IE、Firefox 浏览器中会带上 swf url，与 Chrome 浏览器会带上页面的 url 的情况不同。
> * 您也可以将 player.swf 文件下载后，存放到您的 CDN 服务器中，swf 参数传入指向您的 CDN 服务器路径。
> * 腾讯云提供的隔离域名是每个用户独有的域名，一个 appID 对应一个域名，通常格式为 [appID].vod2.myqcloud.com。
> * 需要将播放器 swf url 的域名添加到白名单内，开启了 Referer 防盗链的视频才能在 Flash 模式下播放。
> * 播放器的 Flash swf 文件默认存放在 imgcache.qq.com 域名下，如需部署到自己的服务器上，可自行下载并部署，[swf 文件地址](https://imgcache.qq.com/open/qcloud/video/tcplayer/player.swf)。
> * iframe 嵌入播放器页面，视频请求的 Referer 会带上 iframe src。

### Key 防盗链
开启流程请看 [Key 防盗链说明文档](https://cloud.tencent.com/document/product/266/14047)

播放器初始化需增加参数如下：
```
var player = TCPlayer('player-container-id', {
     fileID: '', // 请传入需要播放的视频 filID 必须
     appID: '', // 请传入点播账号的 appID 必须
     t: '',
     us: '',
     sign:''
   });
```
参数 t、us、sign的具体含义请查看 [Key 防盗链说明文档](https://cloud.tencent.com/document/product/266/14047)

>**注意事项：**
> * sign 的计算方法为：sign = md5(KEY+appId+fileId+t+us)，与 [Key 防盗链说明文档](https://cloud.tencent.com/document/product/266/14047)中的计算方式不同，其余参数一致。
> * 如果同时开启了 Referer 防盗链，在 Referer 防盗链配置的示例代码基础上增加参数即可。

### 试看功能
使用试看功能需要先开启 Key 防盗链，开启流程请看 [Key 防盗链说明文档](https://cloud.tencent.com/document/product/266/14047)

播放器初始化需增加参数如下：
```
var player = TCPlayer('player-container-id', {
     fileID: '', // 请传入需要播放的视频 filID 必须
     appID: '', // 请传入点播账号的 appID 必须
     t: '',
     us: '',
     sign:'',
     exper:''
   });
```
参数 t、us、sign、exper 的具体含义请查看 [Key 防盗链说明文档](https://cloud.tencent.com/document/product/266/14047)

>**注意事项：**
> * 带试看的 sign 计算方法为：sign = md5(KEY+appId+fileId+t+exper+us)，与 [Key 防盗链说明文档](https://cloud.tencent.com/document/product/266/14047)中的计算方式不同，其余参数一致。
> * 播放器播放的视频时长是 exper 参数指定的长度，与已往在播放端控制播放时长的试看功能不同，播放器不会获取完整的视频。

### HLS 加密播放
开启流程请看[视频加密文档](https://cloud.tencent.com/document/product/266/9638)

>**注意事项：**
> * 如果播放页面或者 Flash swf url 与解密密钥服务器域名不一致，Key 服务器需要部署 corssdomain.xml 和 CORS（"跨域资源共享" Cross-origin resource sharing），允许 Flash 和 JavaScript 跨域获取解密密钥。
> * crossdomain.xml 中配置的是 swf url 的域名，并且 xml 文件必须放置在 Key 服务器的根目录。
> * 播放器的 Flash swf 文件默认存放在 imgcache.qq.com 域名下，如需部署到自己的服务器上，可自行下载并部署，[swf 文件地址](https://imgcache.qq.com/open/qcloud/video/tcplayer/player.swf)。
> * 视频只能进行一次加密，不可多次加密，严格按照视频加密文档操作。
> * 解密密钥正确长度为16字节，起始和末尾位置不能有空白字符。
