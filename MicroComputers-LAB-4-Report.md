# Microcomputers - Lab 4 - Group 13
### Water Controlling System Using PIC16F877A

	Group Members: 
	M.A.Rashid - EN21498756
	D.M.N.A.T.Deheragoda - EN21483974
	D.G.M.A. De Silva - EN21488238


## Introduction:

The main goal of this lab is to build a simple water level control system for a water tank using the PIC16f877A's interrupts and other programming techniques. For a better result, knowledge of PCB fabrication is also applied to this project.  
A printed circuit board, or PCB, is a device that holds electronic components mechanically and connects them electrically using copper-cut conductive channels bonded to an insulating substrate. The two water pumps at their respective water level can be managed by the electronic circuit that we have created for this project.



## Procedure:

First of all, the function that we needed for the PIC  to run as follows, 

https://drive.google.com/file/d/1iybqHZAVzWQbBDCXy13ovRKAeQ5FwjRY/view?usp=sharing

When the lower level sensor detects digital high, the input motor should start to pump water to the tank, the function remains the same even when the middle level water sensor detects digital high, as soon as the top level water sensor detects logic high, the input motor should stop working and the output motor should turn on for 500ms and stop. (500ms is 0.5s)
Design the PCB  - After converting the circuit’s schematic diagram into a PCB layout, placed the components as required using Proteus Professional software, Then drill holes were inserted in suitable places on PCB design. After rooting the traces the PCB design was completed.  
Then took a printout of the PCB layout using a laser printer on the glossy side of a sticker paper.

We bought a copper board of suitable size for our PCB layout. The copper board was rubbed using a steel wool, to remove the top oxide and smooth it.
Etching the PCB - On the copper side of the board, keep the laser printed PCB design touching the copper board, and use a iron to imprint the PCB layout to the Copper board, put into a water container for about a min and remove the printed paper, Use a small Fecl3 solution, dip the imprinted copper boardcopper solution, until all the Copper layers other than the imprinted place remains, finally wash the board and check for any errors.

Solder the components together. 

Applied the code to the Pic16f877A using Pickit 3. 



## How the system works:

The image of the Simulation:-
https://drive.google.com/file/d/16wkifChMjYEpcTdzgOn8arK50jxofBWs/view?usp=sharing


For the digital sensors, soil moisture sensors are being used here. They can give both a digital and an analog signal, since PIC16F877A can only detect digital signals through its pins (without using ADC_converter), the digital output is considered. 


The PCB Layout that we printed as follows,
https://drive.google.com/file/d/1YAVGnp-lG2UeCNIRzJWQl6rsJWoeWoFJ/view?usp=sharing

	1.The place for the push button (Reset button for the PIC16F877A - MCLR) 
	2.The power input and the place for the large resistor which is for the push button
	3.Place for the Oscillator and the Capacitors
	4.Output digital signal for the motor driver
	5.Input from the sensors
	6.Interrupt trigger pin and pin for eliminating floating values

Finally, by soldering and adding the components we were able to finalize the system as follows,

https://drive.google.com/file/d/1H87HUK6hs-FgQ8gjonDJ9F60K89HBOWY/view?usp=sharing

### Some pictures of the implementation:

https://drive.google.com/file/d/1RHqjGneW1jiFYtXKjLWVvMFkqbLbWdbr/view?usp=sharing
https://drive.google.com/file/d/1Zqbi_D8yuiDWRrSDI3vmwjJN2EHjo4XI/view?usp=sharing

A 12V input is given to the motor driver, since it can handle upto 32V, there is a voltage regulator in the motor module to reduce the voltage to 5V, by taking that 5V from the motor driver, the PIC microcontroller and the sensors was powered on.



## Demo (Results) and the Code:

As anticipated before the system works perfectly, as soon as the soil moisture sensor 1 detects water, the a logic high is sent through the Digital output pin, which the microcontroller detects and turns on the input motor which is the motor with the blue fins.
Next, when the sensor 2 detects the water, the motor 1 continues to run, as soon as the sensor 3 is detected the input motor stops and output motor runs for 0.5s and stops as well.
This is the complete procedure cycle of the system that we have made.

The code for the system is uploaded in a Github repository.

https://github.com/Rashid7536/Microcomputers-Lab-4/blob/0dd1911774dc7c5ba5ddfb66b585d901e2b06965/Code%20for%20the%20lab4.md

Logic of the Code as follows,

### In the void main part:-

All the bits needed for the system and external interrupt bits are initialized.

	1.Input/Output Pins
	2.External Interrupt Enable
	3.Global Interrupt Enable
	4.Interrupt Edge (Rising or Falling)
	5.Disable the interrupt Flag

inside a while loop

	if(Sensor 1 is high and Sensor 2 is low and Sensor 3 is low){
			Run Motor 01
	}
	if(Sensor 1 is high and Sensor 2 is high and Sensor 3 is low){
			Run Motor 01
	}
	else{
			Dont run any motors
	}



## Discussion:

Compared to other digital sensors in the market the soil moisture sensor gives a digital high when there’s no water detection, which means when the soil moisture detects water there’s no voltage output, and when the soil moisture is in the air, it gives highest voltage (logic high) as output.
Therefore we changed the code to detect drops in voltage instead of detecting logic high’s.

The Interrupt had many issues while running, at first we planned to have an interrupt trigger pin and trigger the interrupt as soon as the sensor 003 is detected, but there were some issues, it can be because of residual current, issues with the etching or even soldering mistakes. 
Therefore we changed the system such that the sensor 003 is directly connected  to the RB0 pin which is the interrupt trigger pin and made the interrupt edge detection to detect for the voltage drops or falling edge.

We were unable to get 22pF Capacitors from the market, therefore we bought 4x10pF and 2x2pF capacitors and connected them parallely, 10pF||10pF||2pF = 22pF.
 

