+++
title = "Arch Linux Audio Setup on MSI GP63"
date = 2018-08-16T06:02:36Z
tags = []
categories = []
+++

### There are 2 problems after fresh install.
 1. No HDMI audio.
 2. speaker no sound but headphone is good.

### Fixing the "no HDMI audio" problem.
This [thread](https://devtalk.nvidia.com/default/topic/1024022/linux/gtx-1060-no-audio-over-hdmi-only-hda-intel-detected-azalia/post/5211273/#5211273) carries the solution for the first problem.

I followed the thread and made a systemd service to rescan and modprode nvidia_drm and nvidia_uvm. And now my laptop show the HDMI audio.
```
$ lspci -k | grep -i audio
00:1f.3 Audio device: Intel Corporation Cannon Lake PCH cAVS (rev 10)
01:00.1 Audio device: NVIDIA Corporation GP106 High Definition Audio Controller (rev a1)
```

### The "speaker no sound" problem.
pulseaudio is what is using in the notebook, and I couldn't find good solution to permanent fix it. The only solution I did is manually changing the config in the file /usr/share/pulseaudio/alsa-mixer/paths/analog-output-speaker.conf as below. The down side for it is when updating the pulseaudio, this file will be overwrote.
```
[Element Headphone]
switch = off
volume = merge
override-map.1 = all
override-map.2 = all-left,all-right

[Element Speaker]
required-any = any
switch = mute
volume = off
```

and then restart pulseaudio.
```
pulseaudio -k && pulseaudio --start
```

### Final
Hopefully, I didn't miss anything, and you got the audio with no issue.

