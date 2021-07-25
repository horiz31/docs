## How to setup VLC to playback multicast stream from vehicle

1. Download and install VLC https://www.videolan.org/vlc/
2. Download the sdp file, for vehicle "Demo 1" (169.216)  https://github.com/horiz31/docs/blob/master/horizon31_169_216.sdp
3. Double click the sdp file and the stream should open

## To add an overlay graphic
1. Go to Tools > Effects and Filters. [ Shortcut: CTRL + E or Command + E]
2. Switch over to the Video Effects.
3. Under it, choose the Overlay.
4. Check the Add logo option
5. Brose and select the logo (gif or png with transparency)
6. Choose the position center, top and opacity as desired
7. Hit Close when you are done.

## To view fullscreen
1. Turn on minimal Interface (View > Minimal Interface (Ctrl-H))
2. Go to fullscreen (View > Fullscreen Interface (F11))

#### Troubleshooting:
If the stream does not show, it is likley related to the networking or firewall on the local machine. VLC may require a firewall exception when it first loads, but if not an exception should be entered for UDP port 5600. 

It is not fully understood how VLC picks an interface (or interfaces) to use for mulicate group joins. Therefore we recommend that you use this setup on a PC with only one network interface. If you need to disable other interfaces (e.g. WiFi), then reboot after you disable. 
 
It is unlikely, but if you cannot disable other network interfaces, you may be able to add a persistent route on the desired interface.  For Windows, open a command prompt as administrator:    
```netsh interface ipv4 show interfaces```

note the interface Idx for the edge and ethernet interface (this is assuming edge is already running of course). Assuming the Idx was 5 for edge and 7 for ethernet, then

```route -p ADD 224.0.0.0 MASK 255.0.0.0 0.0.0.0 metric 1 IF 7```

#### Latency
VLC has a default network cache of about 1s. This should be fine for most video playback demonstrations. It can be reduced significantly by running vlc from the command line with the network-caching paramter, e.g.:

```vlc --network-caching=150 horizon31_169_216.sdp```

The above reduces the networking caching to 150ms. We do not recommend going any lower than that because we have encountered playback problems. If you need real-time low-latency video, we recommend using GStreamer or Horizon31's GCS which is built on gstreamer.


