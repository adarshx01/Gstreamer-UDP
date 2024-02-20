# Gstreamer-UDP
Streaming LIVE webcam feed using GStreamer . A Python-based GStreamer application for streaming video between a sender and receiver using UDP transmission over same network.  

##  Required:
-  Linux System
*  Ensure both sender and receiver are on the same network.
+  Adjust firewall settings if necessary to allow UDP communication.

##  Files
-  sender.py: Python script for streaming video from a device to a specified IP address and port.
*  receiver.py: Python script for receiving and displaying video streams on a specified port.

##  Check Network connection of both Receiver and Sender
-  In terminal type `ip addr` or `ipconfig` find your ipv4 address from there.
-  Now in Receiver's terminal type `ping sender_IP` and in Sender's terminal type `ping receiver_IP`.
-  If both are able to communicate with each other then Proceed.
##  Installation:

###  Install dependencies:
`sudo apt-get install python3`
`sudo apt install python3-pip`
`pip install -r requirements.txt`   (On Both Sender and Receiver Side)
##  Usage:

###  Sender Side (sender.py)
###  To run the sender script, execute the following command:

`python3 sender.py -ip <destination_ip> -port <port_number> -device <device_index>`

```e.g python3 sender.py -ip 192.168.1.1 -port 8081 -device 2```
You can resize the stream size also by changing  `width=640,height=480`

Replace `<destination_ip>` with Receiver IP Address, `<port_number>` with any Open or Free Port.
Replace `<device_index>` with (0,1,2...) `(default is 0)` , check using `/dev/device*` in terminal for the device index.

###  Receiver Side (receiver.py)
###  To run the receiver script, execute the following command:

```python3 receiver.py -port <port_number>```
```e.g python3 receiver.py -port 8081 ```
Replace <port_number> with the same port used by the sender.

#  Streamimng Multiple Webcams:
-  Use New Terminal Window everytime for another webcam streaming while Transferring other webcam Stream or Viewing stream.
*  Use different ports while executing the sender side command, if the receiver end is not same use IP Address of the other receive and another Unused/Open Port.
*  Execute the Receiver Side command by Specifying the used port for particular required stream into the new terminal window.
*  New Window will Pop-Up use that to view streams
*  You can resize the stream size also by changing  `width=640,height=480`
  
##  ALTERNATE METHOD TO RUN THE SAME-Streaming SETUP

##  On Sender Side-

###  In Terminal
```
sudo apt-get update
sudo apt-get install python3
sudo apt-get install gstreamer1.0-tools

gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw,width=640,height=480 ! jpegenc ! rtpjpegpay ! udpsink host=RECIEVER_IP_ADDR port=ANY_OPEN_PORT
```
Here `/dev/video0` — gives the stream of default webcam.
For using Multiple Cameres change the device ( ` /dev/video[otherNumber] ` ) for other connected webcam. By checking using (` /dev/video* `)


##  On Receiver's Side - 

`gst-launch-1.0 -e -v udpsrc port=PREVIOUS_USED_PORT  ! application/x-rtp,encoding-name=JPEG,payload=26 ! rtpjpegdepay ! jpegdec ! autovideosink`

