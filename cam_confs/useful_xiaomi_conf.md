If you want to use your Xiaomi camera (or any mobile device camera) as a webcam on Ubuntu, here’s a step-by-step guide using tools that work well on Linux:

## **Option 1: Using DroidCam (for Android Phones)**

DroidCam is a popular solution that lets you use your Android phone as a webcam in Ubuntu. Here’s how:

1. **Install DroidCam on your Android device**  
   - Download it from the Google Play Store.

2. **Install DroidCam client on Ubuntu**  
   - Download the client from the official DroidCam website or use these commands:
     ```bash
     cd /tmp/
     wget https://files.dev47apps.net/linux/droidcam_latest.zip
     unzip droidcam_latest.zip -d droidcam
     cd droidcam && sudo ./install-client
     sudo ./install-video
     droidcam
     ```
     This will install the necessary drivers and client[3][4].

3. **Connect your phone to Ubuntu**  
   - Open the DroidCam app on your phone. It will show an IP address and port.
   - Open the DroidCam client on Ubuntu, enter the IP and port, and click "Connect".

4. **Select DroidCam as your webcam**  
   - In your video conferencing app (Zoom, Teams, etc.), select "DroidCam" as the video source[4][5].

## **Option 2: Using RTSP Stream (for IP Cameras like some Xiaomi models)**

If your Xiaomi camera supports RTSP streaming, you can make it appear as a webcam using `ffmpeg` and `v4l2loopback`:

1. **Install dependencies**
   ```bash
   sudo apt install v4l2loopback-dkms ffmpeg
   ```

2. **Create a virtual webcam device**
   ```bash
   sudo modprobe v4l2loopback card_label="Fake Webcam" exclusive_caps=1
   ```

3. **Stream the RTSP feed to the virtual webcam**
   - Replace `rtsp://ip:port/h264_pcm.sdp` with your Xiaomi camera’s RTSP URL.
   - Find your video device (e.g., `/dev/video2`) with `ls -1 /dev/video*`.
   ```bash
   ffmpeg -stream_loop -1 -re -i rtsp://ip:port/h264_pcm.sdp -vcodec rawvideo -vsync 2 -threads 0 -f v4l2 /dev/videoX
   ```
   - Now, `/dev/videoX` will be available as a webcam in Linux apps[2].

## **Troubleshooting and Tips**

- **RTSP URL:** Not all Xiaomi cameras expose an RTSP stream by default. You may need to enable it in the camera settings or use custom firmware.
- **DroidCam:** Works reliably with Android devices but not with Xiaomi security/IP cameras.
- **OBS Studio Alternative:** You can also use OBS Studio with the v4l2loopback plugin to route any video stream to a virtual webcam device[5].

## **Summary Table**

| Method                | Works with Xiaomi IP Cam | Works with Android Phone | Webcam in Ubuntu Apps | Difficulty |
|-----------------------|:-----------------------:|:-----------------------:|:--------------------:|:----------:|
| DroidCam              |            No           |           Yes           |         Yes          |   Easy     |
| RTSP + v4l2loopback   |           Yes           |           Yes*          |         Yes          | Moderate   |

(*If the phone can provide an RTSP stream.)

---

If you’re using a Xiaomi *security camera*, the RTSP method is your best shot. For Xiaomi *phones*, use DroidCam for the easiest experience[2][3][4][5].

Citations:
[1] https://citizenside.com/technology/connecting-xiaomi-camera-to-pc-a-guide/
[2] https://gist.github.com/furryablack/181edc9554a27f8138ce93a683760e40?permalink_comment_id=4793626
[3] https://www.youtube.com/watch?v=nRXdvKP2cck
[4] https://www.kentfaith.com/blog/article_how-to-use-mobile-camera-as-webcam-in-ubuntu_18810
[5] https://askubuntu.com/questions/1235731/can-i-use-an-android-phone-as-webcam-for-an-ubuntu-device
[6] https://gist.github.com/Realiserad/e66a8fc4639da0c4fd5ed3eff62f8227
[7] https://dev.to/thekalderon/use-spare-android-phone-as-webcam-for-your-pc-58o5
[8] https://www.youtube.com/watch?v=x4C3cyWBtRQ
[9] https://www.youtube.com/watch?v=1e7dbiRdtWk
[10] https://www.youtube.com/watch?v=ao70MI8--FY

---
Answer from Perplexity: pplx.ai/share