---
tags:
  - 杂谈
  - mpv
---

# mpv 配置文件

另见：

- <https://github.com/hooke007/MPV_lazy/blob/main/portable_config/mpv.conf>
- <https://vcb-s.com/archives/7594>
- <https://hooke007.github.io/unofficial/mpv_start.html>

## for Windows

```shell
## 基础部分 ##

# 视频输出驱动
vo=gpu
# 图形输出后端；非特殊情况下 Win&NV 用户应使用 d3d11。
gpu-context=d3d11
# 启用原生硬解
hwdec=auto
# 使用一个内置的画质方案预设
profile=gpu-hq
# 对限定范围内的编码尝试硬解，特殊值 all 即任意格式都尝试硬解
hwdec-codecs=all


## 功能 ##

# [补帧时推荐设置为no] 跳转时允许丢帧，默认 yes。
# 禁用它利于修正音频延迟
hr-seek-framedrop=no
# 退出时记住播放状态，默认 no
save-position-on-quit=yes
# 限制保存播放状态的属性列表（示例表示：播放位置、视频 音频 字幕轨号）
watch-later-options=start,vid,aid,sid


## 窗口相关 ##

# 贴边吸附（限 windows 平台）
snap-window=yes
# 播完后保持打开
keep-open=yes
# 是否执行 HIDPI 缩放
#hidpi-window-scale=yes


## 缓存相关 ##

# 最大向后缓存大小（KiB 或 MiB），默认 150MiB
demuxer-max-bytes=200MiB


## OSD ##

# 设置 OSD 文本信息的持续时间（毫秒）（默认值：1000）
osd-duration=5000


## 音频 ##

# 指定音频输出驱动程序的优先级列表。
# win10 优先使用 wasapi，其它可用的有 openal 和 sdl
ao=wasapi
# 此项用于指定启动时的音频输出设备，默认 auto。
audio-device=auto
# 启动默认音量
volume=100
# 变速播放时的音调修正，默认 yes
audio-pitch-correction=yes
# 自动加载含有视频文件名的音频文件
audio-file-auto=fuzzy


## 视频 ##

# 自动校色，默认 no
# 如果做过专业校色应开启（系统目录存在对应的icm档）。
#icc-profile-auto=yes
# [不与 --icc-* 一起使用] 当不使用 ICC 时，视频颜色将适应此颜色空间
# 例如 99% 的 argb 屏幕写 adobe 。90%+ 的 p3 色域屏写 display-p3 （标准 srgb 屏无需更改此默认值）
target-prim=auto
# 默认值 audio（与音频/系统时钟同步）通常兼容性最好但有偶尔的丢帧和重复
# 当值为 display-resample 时具有类 "ReClock" 作用，视频帧匹配刷新率（帧采样），自动调节音频速度补偿偏移
video-sync=display-resample
# 减少由于视频帧率和刷新率不匹配而引起的颤动
interpolation=yes
# [仅当 --interpolation=yes 时生效] 时间插值算法（帧混合），默认 mitchell
# 追求原始观感可以使用 oversample（该效果类似 MADVR 的 smoothmotion）
tscale=oversample


## 字幕 ##

# 自动加载含有视频文件名的字幕文件
sub-auto=fuzzy
# <yes|video|默认no> 在插值和颜色管理之前，将字幕混合到视频帧上。
# 值 video 类似于 yes，但是以视频的原始分辨率绘制字幕，并与视频一起缩放启用此功能会将字幕限制在视频的可见部分（不能出现在视频下方的黑色空白处）
blend-subtitles=video


## 截图 ##

# 截屏文件格式（可选：png、ppm、pgm、pgmyuv、tga、jpg、jpeg）
screenshot-format=png
# 截屏文件保存路径（保存至桌面）
screenshot-directory="~~desktop/"
```

## for Linux

```shell
## 基础部分 ##

# 视频输出驱动
vo=gpu
# 图形输出后端；非特殊情况下 Win&NV 用户应使用 d3d11。
gpu-context=auto
# 启用原生硬解
hwdec=auto
# 使用一个内置的画质方案预设
profile=gpu-hq
# 对限定范围内的编码尝试硬解，特殊值 all 即任意格式都尝试硬解
hwdec-codecs="h264,vc1,hevc,vp8,vp9,av1,prores"


## 功能 ##

# [补帧时推荐设置为no] 跳转时允许丢帧，默认 yes。
# 禁用它利于修正音频延迟
hr-seek-framedrop=no
# 退出时记住播放状态，默认 no
save-position-on-quit=yes
# 限制保存播放状态的属性列表（示例表示：播放位置、视频 音频 字幕轨号）
watch-later-options=start,vid,aid,sid


## 窗口相关 ##

# 贴边吸附（限 windows 平台）
#snap-window=yes
# 播完后保持打开
keep-open=yes
# 是否执行 HIDPI 缩放
#hidpi-window-scale=yes


## 缓存相关 ##

# 最大向后缓存大小（KiB 或 MiB），默认 150MiB
demuxer-max-bytes=200MiB


## OSD ##

# 设置 OSD 文本信息的持续时间（毫秒）（默认值：1000）
osd-duration=5000


## 音频 ##

# 指定音频输出驱动程序的优先级列表。
# win10 优先使用 wasapi，其它可用的有 openal 和 sdl
ao=openal
# 此项用于指定启动时的音频输出设备，默认 auto。
audio-device=auto
# 启动默认音量
volume=100
# <100.0-1000.0> 最大音量。默认 130（该响度约为100的两倍 1.3^3≈2）
volume-max = 130
# 变速播放时的音调修正，默认 yes
audio-pitch-correction=yes
# 自动加载含有视频文件名的音频文件
audio-file-auto=fuzzy


## 视频 ##

# 自动校色，默认 no
# 如果做过专业校色应开启（系统目录存在对应的icm档）。
#icc-profile-auto=yes
# [不与 --icc-* 一起使用] 当不使用 ICC 时，视频颜色将适应此颜色空间
# 例如 99% 的 argb 屏幕写 adobe 。90%+ 的 p3 色域屏写 display-p3 （标准 srgb 屏无需更改此默认值）
#target-prim=auto
# 默认值 audio（与音频/系统时钟同步）通常兼容性最好但有偶尔的丢帧和重复
# 当值为 display-resample 时具有类 "ReClock" 作用，视频帧匹配刷新率（帧采样），自动调节音频速度补偿偏移
video-sync=display-resample
# 减少由于视频帧率和刷新率不匹配而引起的颤动
interpolation=yes
# [仅当 --interpolation=yes 时生效] 时间插值算法（帧混合），默认 mitchell
# 追求原始观感可以使用 oversample（该效果类似 MADVR 的 smoothmotion）
tscale=oversample


## 字幕 ##

# 自动加载含有视频文件名的字幕文件
sub-auto=fuzzy
# <yes|video|默认no> 在插值和颜色管理之前，将字幕混合到视频帧上。
# 值 video 类似于 yes，但是以视频的原始分辨率绘制字幕，并与视频一起缩放启用此功能会将字幕限制在视频的可见部分（不能出现在视频下方的黑色空白处）
blend-subtitles=video


## 截图 ##

# 截屏文件格式（可选：png、ppm、pgm、pgmyuv、tga、jpg、jpeg）
screenshot-format=png
# 截屏文件保存路径（保存至桌面）
screenshot-directory="~/图片/截图"
```