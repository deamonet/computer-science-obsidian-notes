

ffmpeg -i video.m4s -i audio.m4s -codec copy Output.mp4
格式转换。可用于视频

ffmpeg -i audio.m4s -codec copy output.m4a
用于音频

```shell
for i in `ls`; do if [ $i[-3,-1] = "m4s" ]; then ffmpeg -i "${i}" -codec copy "${i[1, -5]}.m4a"; mv "${i}" ../m4s; mv "${i[1, -5]}.m4a" ../../Music; fi; done;
```