---
sidebar_position: 30
---

# HDMI RX Usage

The ROCK 5B has an HDMI-IN connector that supports the standard HDMI 2.0 protocol and can support video inputs up to 2160p@60Hz.

- Preparing the ROCK 5B
- Preparing the Micro HDMI Cable

## Usage Steps

After the HDMI RX device is connected to the ROCK 5B, it will be registered as a video device in the kernel, and the generated node is /dev/video0. You can use the v4l2-ctl command to get the device information and capture frames.

- Check the device information

```bash
$ v4l2-ctl -d /dev/video0 -D
Driver Info:
       Driver name      : rk_hdmirx
       Card type        : rk_hdmirx
       Bus info         : fdee0000.hdmirx-controller
       Driver version   : 5.10.66
       Capabilities     : 0x84201000
               Video Capture Multiplanar
               Streaming
               Extended Pix Format
               Device Capabilities
       Device Caps      : 0x04201000
               Video Capture Multiplanar
               Streaming
               Extended Pix Format
```

- Confirmation of resolution and image format

```bash
$ v4l2-ctl -d /dev/video0 --get-fmt-video
Format Video Capture Multiplanar:
       Width/Height      : 3840/2160
       Pixel Format      : 'NV12' (Y/CbCr 4:2:0)
       Field             : None
       Number of planes  : 1
       Flags             : premultiplied-alpha, 0x000000fe
       Colorspace        : SMPTE 170M
       Transfer Function : Default
       YCbCr/HSV Encoding: Unknown (0x000000ff)
       Quantization      : Default
       Plane 0           :
          Bytes per Line : 3840
          Size Image     : 12441600
```

- Get the digital video timings

```bash
$ v4l2-ctl -d /dev/video0 --get-dv-timings
DV timings:
       Active width: 3840
       Active height: 2160
       Total width: 4120
       Total height: 2250
       Frame format: progressive
       Polarities: -vsync -hsync
       Pixelclock: 297000000 Hz (32.04 frames per second)
       Horizontal frontporch: 88
       Horizontal sync: 44
       Horizontal backporch: 148
       Vertical frontporch: 8
       Vertical sync: 10
       Vertical backporch: 72
       Standards:
       Flags:
```

- Capture image files by setting resolution and pixel formats

Set the HDMI RX to a 4k display and connect it to the HDMI RX.

```bash
$ v4l2-ctl --verbose -d /dev/video0 --set-fmt-video=width=3840,height=2160,pixelformat='NV12' --stream-mmap=4 --stream-skip=3 --stream-count=5 --stream-to=/home/rock/hdmiin4k.yuv --stream-poll
```

- Check image files

Using the 7yuv on Windows

Using the ffplay on Linux

```bash
$ ffplay -f rawvideo -video_size 3840x2160 -pixel_format nv12 /home/rock/hdmiin4k.yuv
```

- Detect Audio Function

When your HDMI RX has an audio input, you can use arecord to get audio from the HDMI RX port and play it on your headphones.

```bash
$ arecord -l
**** List of CAPTURE Hardware Devices ****
card 2: rockchipes8316 [rockchip-es8316], device 0: fe470000.i2s-ES8316 HiFi es8316.7-0011-0 [fe470000.i2s-ES8316 HiFi es8316.7-0011-0]
 Subdevices: 1/1
 Subdevice #0: subdevice #0
card 3: rockchiphdmiin [rockchip,hdmiin], device 0: fddf8000.i2s-dummy_codec hdmiin-dc-0 [fddf8000.i2s-dummy_codec hdmiin-dc-0]
 Subdevices: 1/1
 Subdevice #0: subdevice #0
```

You can see that the sound card number for HDMI RX (HDMI IN) is 2. You can run the following commands to record and playback audio when there is audio input to HDMI RX.

```bash
# get 5 seconds audio file
$ arecord -Dhw:3,0 -d 5 -f cd -r 44100 -c 2 -t wav /tmp/hdmiin_audio.wav
# play on headphone
$ aplay -D plughw:2,0 /tmp/hdmiin_audio.wav
```
