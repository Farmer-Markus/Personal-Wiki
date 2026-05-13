# Webcam

## Using your android phone as webcam
I wanted to use my android phone as a webcam for my pc and it worked really well. <br>
I've used **_[Scrcpy](https://github.com/Genymobile/scrcpy)_** and the **_[v4l2loopback](https://wiki.archlinux.org/title/V4l2loopback)_** kernel module to provide a virtual cam for discord and other applications. <br>

I've tested this on **_[Archlinux](https://archlinux.org/)_** with an Samsung A21S with a custom android 16 rom. <br>

To get started, first install the following: (Replace **_linux-headers_** if you're using not the default arch kernel. More [here](https://wiki.archlinux.org/title/V4l2loopback))
``` bash
sudo pacman -S v4l2loopback-dkms linux-headers scrcpy android-tools
```

Then load the kernel module to create a viritual camera device: (The label will show up as camera info in eg. discord)
``` bash
sudo modprobe v4l2loopback exclusive_caps=1 card_label="Android-Webcam"
```

Now you have a virtual camera. Now we will use the phone as camera. <br>
First of all, **_Very important!_** You need to enable **_[developer options](https://developer.android.com/studio/debug/dev-options)_** on your phone.S


### Wired
To use your phone as usb cam, enable **_Usb Debugging_** in the developer settings.
Plug your phone into your computer using a usb cable and use the following command to activate the camera streaming:
``` bash
scrcpy --video-source=camera --camera-size=1920x1080 --v4l2-sink=/dev/video0 -N
```

In my example I'm using a camera resolution of **_1920x1080_** but you can change this however you want.
There should appear a popup on your phone to allow the wired connection to use debugging. You need to **_allow_** usb debugging. <br>
**You may need to run the the command again.** <br>
Now your android camera should directly stream to your virtual camera device.


### Wireless
To use your phone as wireless wifi cam, enable **_Wifi Debugging_** in the developer settings.
To use your phone camera using wifi connection, you need to first connect your phone to your computer using a usb cable and run the following command:
``` batch
adb tcpip 5555
```
Now unplug your phone find out the ip address(ipV4 address eg. "192.168.171.4"). You can check it in the system information tab in the settings. <br>
Now connect your phone via **_[adb](https://wiki.archlinux.org/title/Android_Debug_Bridge)_**:
``` bash
adb connect <your phones ip>:5555
```

You may need to accept a popup on your phone.
Now start the video stream with **_Scrcpy_** and your desired resolution. I'm using **_1920x1080_** but you can change this however you want.
``` bash
scrcpy --video-source=camera --camera-size=1920x1080 --v4l2-sink=/dev/video0 -N
```

Now your android camera should directly stream to your virtual camera device.
