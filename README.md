# Foreword
This guide is for those like me who received a fake HM-11 from eBay. If you can, buy genuine products to support the development. From the [iBeacon datasheet](http://www.jnhuamao.cn/iBeacon_en.zip):
>Condemn the copycat company copied behavior on HM-10!!!!!!
If you buy a fake, please apply for a refund guarantee your legitimate rights and interests.
JNHuaMao Technology Company.

# How to spot a fake

To decide is this guide is for you you have to first establish that you have indeed pruchased a fake. There are two indiactors for fake HM-10/11
## Phisical Appearance

## Line Endings

# Materials

# Arduino as USB to Serial
Figuring how to properly connect the HM-11 to my PC caused a lot of head scratching.
First, your HM-11 works at 3.3v. While there are many people reporting no damage in applying 5v to the chip si best not to. Luckly is very easy to convert 5v TX signals into 3.3v. All you need is a voltage divider made with one 1K ohm and one 2K ohm resistors.

![Image from randomnerdtutorials.com](https://i1.wp.com/randomnerdtutorials.com/wp-content/uploads/2015/09/voltage-divider-circuit.png)

You will need one of these for every signal going from the arduino to the HM-11.
## Before the conversion
The 


## After the conversion 
With Genuine HM-11 all you need is to upload and empty Sketch on you Arduino. You can remark the fact that Pin 0 and 1 are INPUTS but by default they all are, unless specified otherwise. You can use the Arduino Serial Monitor or the 

    void setup(){
      pinMode(0,INPUT);
      pinMode(1,INPUT);
    }
    void loop(){
    }

# Pins needed


# Procedure

### 1. Manage file synchronization

Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking **File synchronization** in the **Synchronize** sub-menu. This allows you to list and remove synchronized locations that are linked to your file.


# Notes

 - The iBeacon will only start working once you disconnect the RX-TX cables

# References

