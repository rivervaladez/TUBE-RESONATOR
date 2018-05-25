### TUBE-RESONATOR
Tube resonator project for raspberry pi. Finds, through the Alvin Lucier "I am sitting in a room" technique, the resonant frequencies of a given tube and plays them back through a sound that is convolved with the ambient tone of a given space.

### SET UP

Plug 1/8 inch audio out the the raspberry pi into speaker/amplifier.

From computer `ssh pi@raspberrypi.local` and download and install puredata with `apt-get install puredata`

Run `exit` when signed into the pi to sign out.

Copy the TUBERESONATOR.pd patch from your computer to your raspberry pi with 
`scp /path/to/TUBERESONATOR.pd pi@raspberrypi.local:/home/pi/TUBERESONATOR.pd`
Make sure you are on the same wifi network as your pi. You will be propted to enter your pi's password.

Plug in usb-microphone to the raspberry pi, reboot, SSH into your pi with `ssh pi@raspberrypi.local`, and run `cat /proc/asound/cards` in the command line to find your microphones device number. Make sure microphone is places near speaker for effect to work.

Run `sudo nano /etc/asound.conf` and set the default sound device to that of the microphone by replacing the audio device number with the device number of the microphone noted from above.

Check the audio input devices that puredata is recognizing with
`pd -nogui -listdev`. Make note of the usb microphones device number.

Add the following to the bottom of your raspberry pi's cron table with `sudo crontab -e`.

```
@reboot amixer sset 'Mic' 100%
@reboot pd -nogui -audioindev 3 -open /home/pi/TUBERESONATOR.pd
```
The argument after `-audioindev` should be the number noted from `pd -nogui -listdev`.
Feel free to adjust the microphone level percentage to your liking.

Make sure the microphone is inside of the desired tube and that the speaker is amplified enough for the microphone to hear it.

The patch should open and start recording automatically after running `sudo reboot`.
