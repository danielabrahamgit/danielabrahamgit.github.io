---
layout: page
title: Magnetic Levitation
permalink: /Projects/Maglev
---

#### Goal 
What we are trying to achieve here is magnetic levitation by *pulling* the magnet upwards (and repelling it downwards) via an electromagnet. Here is the final result:
<p style="text-align:center;"><a href="Maglev"><img src="../../images/magnet.gif" alt="Maglev" style="width:500px" class="center"></a></p>

#### Hardware 
Here is a diagram of the hardware setup:
<p style="text-align:center;"><img src="../../images/schematic.jpg" alt="schematic" style="width:600px" class="center"></p>
Let's walk through the diagram. First and foremost, a hall effect sensor (a magnetic feild sensor) will measure the magnetic field above the magnet. The higher the magnetic field, the closer the magnet is.   

Once we have information on where the magnet is (not an exact measurement, but gives us an idea), we can start controlling how strong we should push or pull the magnet. An electro-magnet is used to push or pull on the magnet (depending on the polarity of the applied voltage).   

We control the electro-magnet by using the L293D motor driver. The reason why we use a motor driver is because:
1. The high output current capabilities, which is a requirement for the electro-magnet
2. The ability to switch the direction of the current, useful for pushing and pulling the magnet quickly.   

There is still a slight issue in our design so far. The L293D is an all or nothing driver. That is, the electro-magnet is either fully powered, or not powered at all. So how can we turn the electro-magnet on 50% power? We can use a technique called Pulse Width Modulation (PWM).   

The Arduino Nano will apply a PWM signal to the L293D, which now lets us push or pull the floating magnet with variable strength.   

#### Software
To recap, the hardware gave us the ability to be able to do two major things:
1. Push or pull the floating magnet with a variable strength
2. Get a rough reading for how close the floating magnet is to the electro-magnet

These two abilities unlock the ability to levitate a magnet! However we need something called the PID algorithm. This is a very important algorithm in control theory. In short, the PID algorithm is governed by the following equation: 
<p style="text-align:center;"><img src="../../images/equation.jpg" alt="equation" style="width:600px" class="center"></p>
Notice the difference between the continuous and discrete controller. The software will impliment the discrete PID controller. The basic idea of a PID controller is to take into acount the error, the rate of change of the error, and the integral of the error. Then, the system will respond by taking a weighted combination of the error, the rate of change of the error, and the integral of the error as shown in the equation.    

The PID algorithm will take in the hall effect sensor measurement subtracted by the reference value (set by the potentiometer) as the error signal and spit out the control response. This value is a measure of how strong of a current we need to apply to the electro-magnet. We can use PWM to translate the control response to the applied voltage signal on the electro-magnet.    

But, as with everything, there is a catch. The Arduino's *analogWrite()*  function (the build in PWM function) is very slow. We need a faster PWM function, but how do we do this? After much analysis of the Arduino's Atmel 328P datasheet, we can find hardware functionality for fast PWM outputs, exactly what we need! This involves a few setup registers and setting a register called *OCR2A*, which holds an integer value proportional to the duty cycle of the output signal. Here is the datasheet's diagram on this very functionality:
<p style="text-align:center;"><img src="../../images/ocr.jpg" alt="OCR2A" style="width:600px" class="center"></p>

For anyone intersted in the project, I have attatched the code below as well:
<a href="../../code/MagLev.zip"> Source Code</a>