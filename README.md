# BPM2OSC(4GMA3)

This is a neat little tool to analyze the audio input, count the BPM and send it out via OSC-protocol, mainly to developped for [grandMA3](https://www.malighting.com/grandma3/) consoles or software.  
It is very useful for grandMA3-setups to get an proper BPM speed-value from audio, as the integrated audio-analysis in grandMA3-software still is... let's say... not working properly. ;)

This is a simple beat detector built with [aubio](https://github.com/aubio/aubio), forked/cloned from https://github.com/zak-45/WLEDAudioSyncRTBeat.

It will detect the beat and BPM on the given audio input. On every beat, the current BPM is sent to one or more OSC servers.

Command line only.  

Hopefully, one day this whole project will be obsolete when [MA Lighting](https://www.malighting.com) fixes the internal audio/BPM analysis... :)  


## Installation

_Win / Mac / Linux_

Take your release from there : [https://github.com/salopeknet/BPM2OSC/releases]

No python needed.  
This is a portable version, put it on a nice folder and just run it according your OS.  
Tested for Windows (10) & macOS (Sonoma), should also run on Ubuntu.  
<br>
> [!NOTE]
> Some (Windows-) antivirus could warn you, this is false positive.
If you do not trust you can still proceed with step below.

> [!NOTE]
> On macOS you'll have to make the downloaded file executable and run it with [option]+[Right Click]->Run the first time to confirm. I think, it should be the same for Linux.
<br>

_Other OS / all OS with Python installed_

Install required modules
```
pip install -r requirements.txt
```

download BPM2OSC.py file and run it:
```
python BPM2OSC.py
``` 

## Usage

```
BPM2OSC-{OS} beat|list [-h] -s IP PORT MODE ADDRESS [-b BUFSIZE] [-v] [-d DEVICE]

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         Print BPM on beat / dB
  -s IP PORT MODE ADDRESS, --server IP PORT MODE ADDRESS
                        IP OSC-Server IP
                        PORT OSC-Server Port
                        MODE=PLAIN for plain BPM-Value, MODE=HALF for half of BPM-Value, MODE=GMA3 for GrandMA3 Speed (100 percent=240BPM), MODE=PULSE for Pulse/Flash 1/0 only, MODE=GMA3MASTER: type SPEED1 (or 1-15) in Address
  -b BUFSIZE, --bufsize BUFSIZE
                        Size of audio buffer for beat detection (default: 512)
  -d DEVICE, --device DEVICE
                        Input audio device index (use list command to see available devices)

```

### `-v`/`--verbose`
Output a handy beat indicator and the current BPM / dB to stdout. Without it just does it work in the background silently.

### `-s`/`--server`
Add an `IP`, `PORT` , `MODE` and OSC `ADDRESS` to which the BPM beat signal will be sent to. Example: `-s 127.0.0.1 8100 PLAIN /foo/beat`

`MODE` can be:
  
PLAIN (sends counted bpm as an flaoting number)    
HALF (bpm / 2)  
PULSE (sends every single beat as integer 1/0, perhaps to a GMA3 Flash-key or Learn-key)  
GMA3 (sends calculated BPM to match GMA3-Faders, where 240BPM is 100%) to any given fader/OSC-Adress  
GMA3MASTER (sends BPM directly to the internal SpeedMasters 1-15)

`ADDRESS`               OSC-Adress like /gMA3/Page1/Fader211, /gMA3/Page3/Key111, /MadMapper/GlobalSpeed  
In case of MODE=GMA3MASTER type e.g. /SPEED1 and the SPEEDx gets converted to GMA3-SpeedMasters Object-Adress like 13.12.3.x  
Don't forget to include an optionally set global prefix in MA-Software, like "gMA3" at the beginning of the OSC-address.

### `-b`/`--bufsize`
Select the size of the buffer used for beat detection.  
A larger buffer is more accurate, but also more sluggish.  
Refer to the [aubio](https://github.com/aubio/aubio) documentation of the tempo module for more details.  
Example: `-b 128`

### `-d`/`--device`
Specify the index of audio input device to be used.
If not provided, the default system input is used.  
Run `BPM2OSC` to get all available devices.


## Commandline-Example

```
$ BPM2OSC-Windows beat -v -s 127.0.0.1 GMA3MASTER 8000 /gMA3/SPEED1 -s 10.20.0.53 8100 PLAIN /MadMapper/GlobalSpeed -s 127.0.0.1 PULSE 8000 /gMA3/Page1/Key111
```

This will send beat messages to the OSC address `/gMA3/SPEED1` on `127.0.0.1:8000` as GMA3-compatible BPM-value to global SpeedMaster 1 and `/MadMapper/GlobalSpeed` on `10.20.0.53:8100` as PLAIN BPM value, furthermore it sends every beat to `/gMA3/Page1/Key111` on `127.0.0.1:8000` to trigger e.g. a Flash-Button. Additionally, the current BPM will be printed to stdout.


## Screenshot
t.b.a.

## Info 

```
First time you run BPM2OSC-{OS},
this will create folder ./BPM2OSC and extract all files on it.

To save some space and time,
you can then delete BPM2OSC-* and run the app from created folder.
```

## ToDo

- Add some screenshots here
- perhaps build a simple GUI
- testing for Linux, perhaps create a special Raspberry build

## Credits

Thanks to:  zak-45 for his patience and his time! ;)
