# ffmpeg_commands
## *Examples to using ffmpeg via terminal*


* *Dependencies:*
- [x] sudo apt install ffmpeg

> Record WebCam
```shell
ffmpeg -f v4l2 -framerate 30 -video_size 320x240 -i /dev/video0
```


> Record Microphone
```shell
ffmpeg -f alsa -ac 2 -ar 44100 -i default
```


> Record Desktop
```shell
ffmpeg -f x11grab -framerate 30 -video_size 1366x768 -i :0.0 
```


> Record Audio Desktop
```shell
pactl list sources | grep 'analog-stereo.monitor' | awk '{print $2}' > audio 
file_object  = open( "audio", "r" )
driver = file_object.read().split()
driver = driver[0] 
file_object.close()

ffmpeg -f pulse -i driver
```


> Record Desktop (1366x768) and Microphone
```shell
ffmpeg -f x11grab -framerate 30 -video_size 1366x768 -i :0.0  -f alsa -ac 2 -ar 44100 -i default 
```


> Record WebCam and Desktop
```shell
ffmpeg -f v4l2 -framerate 30 -video_size 320x240 -i /dev/video0  -filter_complex  "[0:v]pad=iw:768:0:(oh-ih)/3[left];[left][1:v]hstack"
```


> Merge videos
```shell
ffmpeg -i video_01.mp4 -i video_02.mp4 -filter_complex "[0:v:0][0:a:0][1:v:0][1:a:0]concat=n=2:v=1:a=1[outv][outa]" -map "[outv]" -map "[outa]" out.mp4
```
