# Foreword
This guide is for those like me who received a fake HM-11 from eBay. If you can, buy genuine products to support the development. From the [iBeacon datasheet](http://www.jnhuamao.cn/iBeacon_en.zip):
>Condemn the copycat company copied behavior on HM-10!!!!!!
If you buy a fake, please apply for a refund guarantee your legitimate rights and interests.
JNHuaMao Technology Company.

# How to spot a fake

To decide is this guide is for you you have to first establish that you have indeed pruchased a fake. There are two indiactors for fake HM-10/11
## Phisical Appearance
#TODO
## Line Endings
#TODO
# Materials
#TODO
# Arduino as USB to Serial
Figuring how to properly connect the HM-11 to my PC caused a lot of head scratching.
First, your HM-11 works at 3.3v. While there are many people reporting no damage in applying 5v to the chip si best not to. Luckly is very easy to convert 5v TX signals into 3.3v. All you need is a voltage divider made with one 1K ohm and one 2K ohm resistors.

![Image from randomnerdtutorials.com](https://i1.wp.com/randomnerdtutorials.com/wp-content/uploads/2015/09/voltage-divider-circuit.png)

You will need one of these for every signal going from the arduino to the HM-11.



## Empty Sketch
With genuine HM-11 all you need is to upload and empty Sketch on you Arduino. You can remark the fact that Pin 0 and 1 are INPUTS but by default they all are, unless specified otherwise. You can use the Arduino Serial Monitor or the HMComAssistant.exe. Try this first.

    void setup(){
      pinMode(0,INPUT);
      pinMode(1,INPUT);
    }
    void loop(){
    }

## Soft-Serial
Soft-serial gave was the first thing that worked with pre-conversion chips. Try this if the empty sketch doesen't work. The code will tell you which pin goes where. In this case you will need to connect the pins in a TX-RX, RX-TX fashion. 
E.g Arduino_pin11 -> voltage divider -> RX_MH-11

    #include <SoftwareSerial.h>
    
    SoftwareSerial BTSerial(10, 11); // RX | TX
    String rxString;
    void setup()
    {
      Serial.begin(57600);
      Serial.println("Enter AT commands:");
      BTSerial.begin(9600);  // HM-10 default speed in AT command more
      BTSerial.write("AT+HELP"); // entered each command here and uploaded 1 at a time!
    }    
    void loop()
    {
      // Keep reading from HC-05 and send to Arduino Serial Monitor
      if (BTSerial.available())
        Serial.write(BTSerial.read());
    
      // Keep reading from Arduino Serial Monitor and send to HC-05
      if (Serial.available())  {
        char c = Serial.read();  //gets one byte from serial buffer
        if (c == '\r') {  //looks for end of data packet marker
          Serial.read(); //gets rid of following \n
          //do something with rx string
          BTSerial.println(rxString);
          rxString = ""; //clear variable for new input
        }
        else {
          rxString += c; //add the char to rxString
        }
      }
    }
The code above is from [this post](http://forum.arduino.cc/index.php?topic=348216.0) on Arduino forums with [this tweak](https://forum.arduino.cc/index.php?topic=342245.0) to remove the CR LN.
# Pins needed
#TODO

# Flashing the CC2541
Following [this post](https://forum.arduino.cc/index.php?topic=393655.msg2709528#msg2709528) and [this feedback](https://forum.arduino.cc/index.php?topic=393655.msg3376643#msg3376643) on the same thread I proceded as follows:
### 1. Wiring
Using three voltage deviders connect the HM-11 to the Arduino Uno as follows
### 2. Download the software
You will need thew following files from [RedBearLab/CCLoader](https://github.com/RedBearLab/CCLoader):
 - executable for your environment (in my case CCloader.exe)
 - Arduino sketch (CCloader.ino)
 - the full bin file (CC2541hm10v540.bin)
Make sure the executable and the bin file are in the same folder.
 The HM-10 bin file works fine for the HM-11.
 ### 3. Load CCloader.ino on the Arduino Uno
 You will need the Arduino IDE to do this but I hope that's obvious.
  ### 4. Run
 Open a terminal window at the folder, then run:

     CCLoader.exe <COM Port> <firmware.bin> [0/1]

 I was using COM7 and an Arduino Uno so I ran:
  
    CCLoader.exe 7 CC2541hm10v540.bin 0
You should see this:

    Copyright (c) 2013 RedBearLab.com
    CCLoader.exe version 0.5
    Comport : COM7
    Bin file: CC2541hm10v540.bin
    Device  : Default (e.g. UNO)
    
    Comport open!
    <Baud:115200> <data:8> <parity:none> <stopbit:1> <DTR:off> <RTS:off>
    
    File open!
    Block total: 512
    
    Enable transmission...
    Request sent already!
    /********************************************************************/
    * If there is no respond last for 3s, please press "Ctrl+C" to exit!
    * And pay attention to :
    *   1. The connection between computer and Arduino;
    *   2. The connection between Arduino and CC2540;
    *   3. Whether the device you using is Leonardo or not;
    *   4. Other unexpected errors.
    /********************************************************************/
    
    Waiting for respond from Arduino...
    
    Uploading firmware...
    
    1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  [...] 511 512 
    Upload successfully!
    File closed!
    Comport closed!
Well done you now have a genuine firmware. Disconnect all debug cables.
# Updating the firmware
It is now a good idea to update the firmware to the latest version. For this you will need:

 - From [suryasundarraj/hm-10-firmware/](https://github.com/suryasundarraj/hm-10-firmware/):
	 - [HMComAssistant.exe](https://github.com/suryasundarraj/hm-10-firmware/blob/master/HMComAssistant.exe "HMComAssistant.exe")
	 - [HMSoft.exe](https://github.com/suryasundarraj/hm-10-firmware/blob/master/HMSoft.exe "HMSoft.exe")

 - From [JNHuaMao's website](http://www.jnhuamao.cn/download_rom_en.asp?id=1):
    - The latest firmware: [HM-11 HMSoft CC2541 V703 firmware upgrade file.........[2019-02]](http://www.jnhuamao.cn/rom/HMSoft-11-2541-V702.zip)

# Setting up the iBeacon
Follow [this Instructable](https://www.instructables.com/id/make-iBeacon/).
Reference guide from [JNHuaMao's website](http://www.jnhuamao.cn/iBeacon_en.zip).

# Notes

 - The iBeacon will sometimes only start working once you disconnect the RX-TX cables

# References

 - [How to flash genuine HM-10 firmware on CC2541 (make genuine HM-10
   from CC41)](https://forum.arduino.cc/index.php?topic=393655.0) see also [post #94](https://forum.arduino.cc/index.php?topic=393655.msg3376643#msg3376643)
  - [Connecting an Arduino to a Myo armband using a HM-10 /
   HM-11](https://blog.raquenaengineering.com/arduino-and-the-myo-armband/) for the pinout of DEBUG_CLOCK, DEBUG_DATA, and RESET on the HM-11
-  [bjoerke/HM-10-Firmware]([https://github.com/bjoerke/HM-10-Firmware/wiki/flash-firmware](https://github.com/bjoerke/HM-10-Firmware/wiki/flash-firmware)) for extra instructions
- [suryasundarraj/hm-10-firmware](https://github.com/suryasundarraj/hm-10-firmware/) for HMComAssistant.exe
- [Martyn Currey HM-10 Bluetooth 4 BLE Modules](http://www.martyncurrey.com/hm-10-bluetooth-4ble-modules/) for an extensive descripion of the modules
- [JNHuaMao](http://www.jnhuamao.cn/bluetooth.asp?id=1) website for images of genuine products
- [Upgrading Firmware to HM-10 CC2541 BLE 4.0](https://techienoise.blogspot.com/2016/02/upgrading-firmware-to-hm-10-cc2541-ble.html)
- [HM-10 BLE responds only to AT](http://forum.arduino.cc/index.php?topic=348216.0)
- [Realising I had a fake](https://arduino.stackexchange.com/questions/34858/ftdi-to-hm-10cc2541-serial-command-no-response)
- [Arduino Program for Bluetooth](http://forum.arduino.cc/index.php?topic=262387.0)
- [Make IBeacon With HM10/HM11](https://www.instructables.com/id/make-iBeacon/)
- [Using HM-10 BLE Modules as Low-Cost iBeacons](http://www.blueluminance.com/HM-10-as-iBeacon.pdf)
- [What is iBeacon?](https://developer.estimote.com/ibeacon/#uuid-major-minor-and-beacon-regions)
- [Removing \r\n from Serial Input](https://forum.arduino.cc/index.php?topic=342245.0)
- [How to Level Shift 5V to 3.3V](https://randomnerdtutorials.com/how-to-level-shift-5v-to-3-3v/)
