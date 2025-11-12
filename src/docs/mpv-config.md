# 个人自用的 mpv 配置

```shell
poplar@Greysia:~> tree ~/.config/mpv
/home/poplar/.config/mpv
├── mpv.conf
├── shaders_cache
└── watch-later

3 directories, 1 file
poplar@Greysia:~> cat ~/.config/mpv/mpv.conf
vo=gpu
# 视频输出驱动
profile=high-quality
# 使用一个内置的画质方案预设
vd-lavc-dr=auto
# 解码到显存中
hwdec=yes
# 使用硬解（原生模式）

gpu-api=vulkan
vulkan-async-compute=yes
vulkan-async-transfer=yes
vulkan-queue-count=1
# vulkan 或者 opengl
gpu-shader-cache-dir=/home/poplar/.config/mpv/shaders_cache
# 着色器缓存目录

video-sync=display-resample
interpolation=yes
tscale=oversample
# 帧数超采样
# 如果耗电量大，可以考虑关掉

#log-file="/home/poplar/.config/mpv/mpv.log"
# 日志文件目录（debug 用）

vd-lavc-dr=yes
opengl-pbo=yes
# 可能对 4K 解码有帮助

save-position-on-quit=yes
watch-later-directory=/home/poplar/.config/mpv/watch-later
# 退出时保存当前的播放状态
keep-open=yes
# 播放结束后不退出
watch-later-options=start,vid,aid,sid
# 指定保存播放状态的属性列表（示例表示：播放位置、视频 音频 字幕轨号）

audio-file-auto=fuzzy
alang=jpn,ja,eng,en
# 自动加载近似名的外置音轨
sub-auto=fuzzy
slang=chi,zh-CN,sc,chs
# 自动加载近似名的外置字幕

screenshot-format=png
# 截图格式
screenshot-dir="/home/poplar/Pictures/screenshots"
# 截图输出路径

deband=yes
# 启用去色带
cscale=catmull_rom
# 色度平面的拉伸

blend-subtitles=video
# 设置将字幕渲染到视频源分辨率并随视频一起缩放并进行色彩管理

#audio-channels=stereo
# 如果双声道系统播放多声道影片时有的声道声音没出现，尝试强制设定为双声道
```