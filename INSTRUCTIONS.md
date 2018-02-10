# Build-your-own-google-home
DIY google home smart speaker using raspberry pi   
 Few things about google home https://www.youtube.com/watch?v=r0iLfAV0pIg


Google home is nothing but it is a smart speaker with google assistant built in.This speaker have the same voice assistant that in a android phone.

Recently Goole released its Assistant API for the raspberry pi.This means that makers,hobbyist and educationalist can now build the google assistant into project using the raspberry pi,also Google and raspberry pi foundation released the "Voice Kit"with a special bit of hardware called Voice Hat.Voice Hat is basically a speaker driver and microphone.The voice kit is released with magpi(the official magazine of raspberry pi).The box contains a Push Button,Voice Hat,Pcb with two microphone,Speaker and some connecting wires.

But in this project we are not using the voice kit..


# WHAT YOU NEED
1.ARaspberry pi (along with all the normal bits and pieces like a microSD card,mouse,keyboard etc..)


2.Aspeaker with a 3.5mm connector


3.AUSB microphone (see recomented https://www.adafruit.com/product/3367),if you don't have a microphone like this this will works with web cam with mic.



# BASICS
Google releases a modified raspbian with the Voice Kit,but that distribution only works with voice hat.What we are going to do is take the Voice kit software and modify it to work wit the pi's internal sound card and a USB microphone 


First thing to do is download the Voice Kit microSD card image for the raspberrypi.You can download it directly from this link https://goo.gl/qwsB6D

Once the .img.xz file has been downloaded you need to write it to the microSD card using card writing utility(recomented ETCHER)


When the microSD card is ready then you need to connect your raspberry pi3 to an monitor and hook up a mouse and keyboard and boot up your raspberry pi 


After booting you will see the standard pixel desktop,how ever background has been changed tofeature the AIY projects logo..
# step-1
click on the raspberry pi logo at the top left of the display.Move to preferences and then click on Raspberry PI configuration.In the program go to"interfaces"and enable SSH and pess OK.

# step-2
Then click on the wifi symbol at the top right corner and connect to your wifi network...
# step-3
Double click on the start dev terminal icon and  type sudo leafpad/boot/config.txt and scroll down and remove the "#" symbol in front of the line dtparam=audio=on and insert a"#" in front of the two lines below it.Save the file and exit from leafpad..Actually here we are blocking the sound card on the voice hat.


From step-3 above the last few lines of/boot/config.txt should look like this:

```python
# Enable audio (loads snd_bcm2835)
dtparam=audio=on
#dtoverlay=i2s-mmap
#dtoverlay=googlevoicehat-soundcard
```


# AUDIO SETUP


The next step is to get the audio working.You are going to need a speaker with a 3.5mm connector and a USB microphone.I didn't have a dedicated USB microphone at hand so i plugged in a spare web cam that i had.The PI was able to use the microphone from the webcam as a standalone mic!.Plug the 3.5mm jack on the port and connect the mic via USB port.

Now we are going to tell the raspberry pi to play sound through the 3.5mm port and get sound from USBport by sending the address of our mic and speaker..
for this type the command in the terminal

sudo leafpad/etc/asound.conf

Delete all the contents inside the file and replace with this
```java


pcm.!default {
  type asym
  capture.pcm "mic"
  playback.pcm "speaker"
}
pcm.mic {
  type plug
  slave {
    pcm "hw:1,0"
  }
}
pcm.speaker {
  type plug
  slave {
    pcm "hw:0,0"
  }
}
```


Save the file and exit from leafpad.

Now it is time to reboot,click on raspberry symbol(top left)and click on shutdown......followed by reboot.


When your pi has rebooted it is time to run Google's test scripts to make sure that everything is working...


Goto file manager and open the "AIY_VOICE_KIT"folder and open the "checkpoints"folder and right click on the check_audio.py file and open that file with a text editor..

Near the top of the file change the line "VOICEHAT_ID='googlevoicehat' to   VOICEHAT_ID='bcm2835'  save the file and exit..


On the desktop there are three files for checking your configuration,double click on the"check audio"file and follow the instructions given in the black window.


our hardware hacking part is done.In order for the Google Assistant to work your PI needs to be configured to work with google's cloud services

for cloud setup follow the instructions given in the google's website and start for SETTING UP YOUR DEVICE section link:https://goo.gl/zZDq4M


If you done that say"ok google" to activate the assistant and ask your query



# have some fun
The voice kit contains a button for activating the assistant without using the hot word "ok google"
but we don't have hvave the voice hat to connect the button 
After following the pcb track i found that the button is connected to GPIO 23 and ground...








 
