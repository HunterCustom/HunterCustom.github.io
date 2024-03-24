---
title: Archiving [HOW TO] VSS from ABS sensor on NA/NB
author: hunter
date: 2024-03-23 18:57:00 +0800
categories: [Cars, Miata]
tags: [miata, abs, archiving]
---
> Post was orginaly made by user [toast](https://www.miataturbo.net/members/toast-46191/) on 12/12/2022 on Miataturbo.net. I am purly reposting this for archive in case the website ever goes down.

# [HOW TO] VSS from ABS sensor on NA/NB

I got this working and would like to share with how to connect a stock ABS sensor to be used as a Vehicle Speed Sensor

Requirements:
1. Car without ABS or with a broken one - You will be taking the sensor away from ABS (there are ways to share it, but i do not know how)
2. Sensor mounting (this can be done by swapping a hub assembly from a car that came with ABS)
3. Haltech ECU


**Step 1: Mount the thing.**
If a car never came with ABS, it will still have holes provisioned in the chassis for the stock sensor. In my case I have swapped a 1.8 diff and half shafts. One of the hubs had holes and the ring for ABS sensor. Got a sensor from a junker and used 3 bolts in stock locations to secure it. There's a hole in the trunk that the sensor wiring can plug into.

**Step 2: Wiring it**
This is a weird one but one cable goes to ground and another one to SP* input. That's because the sensor generates it's own voltage spikes that ECU can read.

**Step 3: Configuring it**
- Enable Vehicle Speed Sensor in the Sensors tab
- In Vehicle Speed tab select which wheel you mounted the sensor on
- In the wiring tab, assing your SP* pin (SP1, SP2, etc)
- Unde wiring for it:
Sensor Type: Digital - Frequency (no choise)
Edge Select: Rising
Sensor Type: Reluctor
Frequency Time Constant: 1ms

![photo of settings inside Haltech NSP Software](https://raw.githubusercontent.com/HunterCustom/HunterCustom.github.io/master/assets/img/image_2024-03-23_221953358.png)

**Step 4: Calibration**
Easier with a passenger. In the Vehicle Speed tab, set your calibration speed and drive at that speed for a few seconds before pressing "Calibrate"

Fun stuff:

**Gear detection**
Under Transmission -> Functions, there's a Gear Ratio, enable it.
To calibrate IT DOES NOT HAVE TO BE AT 1000RPM, just the exacttly the same RPM for all gear calibrations
If on driven wheels you can lift up the rear and essentially idle the car at each gear before hitting "Calibrate"
If on front wheels, you can drive the car at the exact same RPM in differnet gears

**Launch Control**
Vehicle Functions -> Motorsport, Lunch Control
Clutch switch on NA's can be weird but you can use the VSS to enable Launch Control when car is stationery

**Idle Control**
Engine Functions -> Controller, there's "Max Vehicle Speed"
Keeps the idle a bit higher than usual until car becomes stationery, helps preventing idle dips stalling the car

**Boost by gear**
I do not have this figured out yet but it would probably go like this:
Engine Functions -> Boost control, enable
Mode: Closed Loop
Boost Control -> Closed Loop Target Pressure, edit table axis
Add another axis "Gear"
Insert Value: 1, 2, 3, 4, 5, (6)
Edit the boost max boost pressure for each gear in each RPM
