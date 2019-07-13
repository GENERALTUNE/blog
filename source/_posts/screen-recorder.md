---
title: 视频录制
date: 2019-07-13 15:46:31
tags: ffmpeg  simpleScreenRecorder
---

很多时候我们需要录屏，我往往会选择一些更开源更DIY的工具

### simplescreenrecorder
虽然Ubuntu用户数量少，但是有需求总有人去开发，凭着自由理念，github上就有这么一项开源项目，SimpleScreenRecorder是Linux下较受欢迎的录屏软件，功能比较齐全。
1. 安装SimpleScreenRecorder
按Ctrl+ALt+T打开终端
添加源：sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
更新源：sudo apt-get update
安装：sudo apt-get install simplescreenrecorder
2. 打开SimpleScreenRecorder
点击左上角启动器，在查找框输入：sim
点击SimpleScreenRecorder
PS：也可以把SimpleScreenRecorder拖到桌面快捷方式，方便以后打开

3. 录制窗口设置
默认的情况下，是所有屏幕全屏录制，30FPS
video input下面4个选项
第一个是全屏录制，右边的选项，第一个是所有显示器，下面其他选项是针对某个显示器
第二个是固定的坐标范围录制，比如屏幕的某一部分位置
第三个是跟随鼠标录制，比如设定一个长度与宽度，鼠标去到那，录制的范围就跟到哪
第四个是录制openGL应用
Frame rate是录制的帧率，希望观看流畅尽量设置高点（一般显示器60帧，别超过这个数字），帧数高，视频体积也相对增大
Scald video是把视频压缩到指定尺寸，一般不推荐使用
Record cursor是记录鼠标，建议使用
音频默认可以了
点击continue
4. 选择画质
画质有几个预设的选项
high quality intermediate：高清视频
Live Stream（1000kbps）：直播流，码率为1000kbps
Live Stream（2000kbps）：直播流，码率为2000kbps
Live Stream（3000kbps）：直播流，码率为3000kbps
YouTube：高质量视频
除了预选择，也可以自己设置画质，下面会说到
5. 选择视频保存位置
点击browse浏览文件夹，选择一个文件夹以及输入默认名称，点击保存
视频的命名方式是，你预设的名称+日期时间

6. 设置视频格式、编码、码率，帧率，设置声音格式、码率
Video：
   Container：封装视频的格式有MKV，MP4等
   Codec：编码方式有很多，推荐H264，高压缩，省硬盘空间
   Constant rate factor：范围0-51，数字越低画质越好
   Preset：设置码率，这些选项里面，越往上越高（码率高细节好）
Audio：
   Codec：一般是MP3或者AAC
   Bit rate：一般128或者192，麦克风不行的话设置太高也是浪费存储空间
设置完成后，点击底部的continue

7. 录制视频
设置完成后，到了开始录制视频这一步
顶部的Start recording就是开始录制视频，点击就开始录制了
中间的Start preview就是开始预览，比如把当前录制内容显示出来
底部的Save recording就是结束当前录制并且保存
另外，通过点击托盘栏的软件图标，也是可以快速开始录制与保存
### [FFmpeg](https://ffmpeg.org/)屏幕录像和录音


#### Windows
先安装dshow软件 Screen Capturer Recorder， 项目地址：https://sourceforge.net/projects/screencapturer/files/ 。然后查看可用设备名字：
```
ffmpeg -list_devices true -f dshow -i dummy
```
会显示

```
DirectShow video devices (some may be both video and audio devices)
“screen-capture-recorder”    //视频设备
DirectShow audio devices
“virtual-audio-capturer”  //音频设备
```
就能看到咱刚安装的Screen Capturer Recorder，如果你有其他的设备，比如摄像头，麦克风等，也会显示。
录制视频（默认参数）
```
ffmpeg -f dshow -i video="screen-capture-recorder" v-out.mp4
```
录制声音（默认参数）
```
ffmpeg -f dshow -i audio="virtual-audio-capturer" a-out.aac
```
同时录制声音和视频（默认参数）
```
ffmpeg -f dshow -i video="screen-capture-recorder":audio="virtual-audio-capturer" av-out.mp4
```
查看视频录制的可选参数
```
ffmpeg -f dshow -list_options true -i video="screen-capture-recorder"
```
示例视频录制（依次设置：分辨率 帧率 像素格式
```
ffmpeg -f dshow -video_size 1680x1050 -framerate 30 -pixel_format yuv420p -i video="screen-capture-recorder" v-out.mp4
```
查看音频设备可选参数
```
ffmpeg -f dshow -list_options true -i audio=virtual-audio-capturer
```
指定参数录制音视频
```
ffmpeg -f dshow -video_size 1680x1050 -framerate 30 -pixel_format yuv420p -i video="screen-capture-recorder":audio="virtual-audio-capturer" av-out.mp4
```
#### Linux
使用[x11grab](https://ffmpeg.org/ffmpeg-devices.html#x11grab)，相信Linux用户动手能力的比较强，自行查看安装方法吧，点我查看。 
安装完之后，可以录制了
```
ffmpeg -video_size 1024x768 -framerate 25 -f x11grab -i :0.0+100,200 v-out.mp4
```
上面的参数，指的是从屏幕的左上角（x=100, y=200）的位置，录制分辨率为1024×768的视频。

可以使用[ALSA](https://ffmpeg.org/ffmpeg-devices.html#alsa-1)同时录制声音
```
ffmpeg -video_size 1024x768 -framerate 25 -f x11grab -i :0.0+100,200 -f alsa -ac 2 -i hw:0 av-out.mkv
```
也可以使用[Pulse](https://ffmpeg.org/ffmpeg-devices.html#pulse)声音输入设备
```
ffmpeg -video_size 1024x768 -framerate 25 -f x11grab -i :0.0+100,200 -f pulse -ac 2 -i default av-out.mkv
```

提示
如果电脑配置比较低，可能不能很好的录制屏幕的同时进行音视频编码。这种情况下，可以先录制未压缩的音视频，最后再进行音视频编码压缩。
Linux
```
ffmpeg -framerate 25 -video_size 1024x768 -f x11grab -i :0.0+100,200 -f alsa -ac 2 -i pulse -vcodec libx264 -crf 0 -preset ultrafast -acodec pcm_s16le output.mkv ffmpeg -i output.mkv -acodec ... -vcodec ... final.mkv
```

windows
```
ffmpeg -f dshow -i video="screen-capture-recorder":audio="Microphone" -vcodec libx264 -crf 0 -preset ultrafast -acodec pcm_s16le output.mkv
ffmpeg -i output.mkv -acodec ... -vcodec ... final.mkv
```






#### 无损格式录制
如果想要完美的屏幕录制效果，可以使用x264进行无损编码
```
ffmpeg -video_size 1920x1080 -framerate 30 -f x11grab -i :0.0 -c:v libx264 -qp 0 -preset ultrafast capture.mkv
```
“-qp 0″是x264无损编码模式，“-preset ultrafast”表示最快的速度编码。

参考[http://trac.ffmpeg.org/wiki/Capture/Desktop](http://trac.ffmpeg.org/wiki/Capture/Desktop)

### 去麦克风的噪音
   使用 Ubuntu 18.04 操作系统做屏幕录制的时候发现一个问题：每次在屏幕录制的过程中，麦克风收音会有很大的背景噪音，而且不能去除。但在相同的录音环境中，使用相同的麦克风设备，使用 MacBook 录制也没有发现有噪音。
   使用 ffmpge 提取视频流、音频流

安装 ffmoeg

我们先使用 SimpleScreamRecord 进行屏幕录制，保存为 mkv 格式的视频。然后我们将会使用 ffmpeg 工具进行视频音频的提取操作。

在开始分离视频音频之前，我们需要先检查以下我们是否已经安装 ffmpeg 工具，如果没有安装，我们可以先安装 ffmpeg 工具。

sudo apt install ffmpeg

 

分离音频

如果我们想要对视频里面的音频进行处理，首先要把音频提出出来，我们这里会将使用 ffmoeg 工具将视频中的音频提出并保存为 mp3 格式。

ffmpeg -i original_video.mp4 original_audio.mp3

 

视频流分离

我们最终是需要把处理好的音频与视频重新打包成一个视频文件，那么，很显然，我们是需要一个没有声音的纯视频文件的，我们同样可以使用 ffmpeg 工具来完成视频的提取。

ffmpeg -i original_video.mkv -vcodec copy -an video_without_sound.mkv

 

使用 Audacity 对音频降噪

接下来，我们会使用 Audacity 音频处理软件进行降噪处理。如果我们没有安装 Audacity，可以使用软件中心安装，或者使用命令行安装。

sudo apt-get install audacity

 

降噪处理

使用 Audacity 进行降噪，方法也比较简单。步骤如下：

1.打开软件，并导入音频

2.选取一段背景噪音，并选择 (命令路径)，点击获取噪音

3.全选音轨，选择(命令路径)，通过调整参数和预览效果，点击确认降噪。

4.导出处理完后的音轨

将处理完成后的音轨与视频打包

到这里，我们还差最后一步就能完成目标了。我们只需要把处理好的音频与刚才提取出来的视频打包即可。这里，我们会再次使用 ffmpeg 工具完成任务。

合并：

ffmpeg -i video_without_sound.mkv video_sound_clean.mp3 -vcodec copy video_clean.mp4

 

Ubuntu 设置麦克风降噪

以上使用软件对音频进行降噪，是比较常规的操作，这种方法虽然操作上比较复杂，但无论是对屏幕录制、还是手机、摄像机录制的视频都有效。但是，如果是经常需要使用 Ubuntu 进行麦克风收音、录制的工作，那每次都需要完成上面一连串的套路，显然不方便。如果能做到一劳永逸那自然是最好的。

下面，将演示如何通过修改配置文件从而实现设置麦克风降噪的效果。

完成这一设置我们需要修改 /etc/pulse/default.pa 这一配置文件。一般，我们在修改配置文件之前，最好都先对配置文件进行备份。

sudo cp /etc/pulse/default.pa /etc/pulse/default.pa.bak

然后，我们使用 vim 打开这个配置文件：

sudo vim /etc/pulse/default.pa

然后我们在配置文件的最末尾添加以下配置内容，这里有个 Tips，vim 按 Shift + G 可以直接跳到文章的末尾，按 a 即进入编辑模式，然后将配置内容复制即可。

#Active Noise Removal

.ifexists module-echo-cancel.so

load-module module-echo-cancel aec_method=webrtc source_name=mic source_properties=device.description=MicHD

set-default-source "mic"

.endif

完成之后，我们还需要重启以下 pulse 服务。

 

附录（CoolEdit 噪音消除）

以下为在 Windows 平台下使用 CoolEdit 对音频进行降噪处理的操作步骤。

首先，我们需要先录制一段音频，或加载一段音频到 CoolEdit 上。

然后，我们先选取一段背景声音，选中。

通过菜单栏打开 效果 噪音消除 降噪器，点击采集噪音。

随后关闭降噪器选项栏，然后选取全部音轨。

再次打开 降噪器，点击 加载噪音，选择刚才保存好的噪音样本。

点击确认即可。

[参考](https://ywnz.com/linuxjc/2346.html)

    
