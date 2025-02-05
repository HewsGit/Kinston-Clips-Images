
<style>
    /* initialise the counter */
    body { counter-reset: figureCounter; }
    /* increment the counter for every instance of a figure even if it doesn't have a caption */
    figure { counter-increment: figureCounter; }
    /* prepend the counter to the figcaption content */
    figure figcaption:before {
        content: "Figure " counter(figureCounter) ": "
    }
</style>







# **1. Introduction**

This manual has been created for Spirit AeroSystems. The objective of this document is to deliver the reader general operating knowledge relating to the Taurus Robotic system. The contents of this manual specify the components, processes, and usability for implementing automated inspections. 

# **2. Safety**

The system is furnished with safety elements that consistently monitor the conditions of the cell envelope and its equipment. The safety devices are communicating directly with the Beckhoff safety cards using dual redundant signals to ensure that operation and communication is monitored continuously; the signals use test pulses to connect with the safety inputs thus creating a safety rated interface.

The communication is then branched from the PLC to the Safety Interface Board (SIB) which is housed in the KRC5. The KRC5 will be referred to as the Robot Controller. If the dual signals between the equipment and the safety input cards is present, the PLC relays this information to the SIB and all functionalities of the robot are enabled. In the case communication is lost, the PLC issues this message to the SIB and robot movements are prohibited until the droppage in communication is resolved. Loss in communication will be a resultant of the safety equipment being triggered or a physical connection issue. Safety equipment being triggered refers to an Estop being pressed, breaking a laser barrier or an anticollision sensor being tripped. 

If any of these events occur, a reset from both the laser barrier and operator console will be required to re-initialize the operator safety. This operation will be detailed.

- Safety light curtain for access protection
- Sender & receiver
- Anti-collision sensors
    - Probe holders
    - EOAT’s
- Emergency Stops
    - SmartPad
    - Operator Console
    - Power Cabinet
    - Control Cabinet
    - Water Cabinet
    - Laser Barrier Reset
    
**All safety components have a rating of PL level 4 and SIL level d.**

The status of each safety instrument can be viewed on the “Home” tab of the Innerspec HMI. Refer to section 8.1.1 for more detail.

For visual representation, two stack lights are placed on the safety fence, one above the KUKA Cabinet, one above the operator station. The stack lights provide general status of the cell. The legend for the indicators lights is located on the “Maintenance” tab of the Innerspec HMI. Refer to section 8.1.8 for more detail. 



<div style="text-align:center">

<span style="color:red"> __Explain how safety works when turntable is moving__</span>

<figure>
    <img src="SafetySwitches Smartpad.jpg"
         alt="SafetySwitches Smartpad.jpg" width= "300">
    
    <figcaption>SafetySwitches, Smartpad.jpg</figcaption>
</figure>

</div>

Available operating modes for the SmartPad are as follows:

- T1
    -   Speed reduced (250mm/s)
    - No operator safety required
    - Deadman switches must be pressed
    - SmartPad is the only location allowing robot movement
- T2
    - Max velocity allowed
    - Operator safety required
    - Deadman switches must be pressed
    - SmartPad is the only location allowing robot movement
- AUT
    - Max velocity allowed
    - Operator safety required
    - Deadman switch does not need to be pressed
    - SmartPad is the only location allowing robot movement
- EXT
    - Max velocity allowed
    - Operator safety required
    - Deadman switch does not need to be pressed
    - Robot movement is controlled via PLC and external PC

For more information regarding the SmartPad and its functionalities, reference section 7.1.

<div style="text-align:center">

<span style="color:red">__Refer to *‘Safety_Mechanical_components_V11_en’* for details about safety__ </span>

</div>

## **2.1 Collisions**

Each EOAT is equipped with an anti-collision device, an adjustable pneumatic sensor. If triggered, the robot will halt. Refer to section 7.5 for instructions on moving the robot after a collision. Refer to the individual tool section for information on adjusting the pneumatic sensor.

## **2.1 SafeOperation**

Each KRC5 controller is equipped with Kuka’s SafeOperation software option, the software add-on is rated at a category 3 and performance level d, according to ISO 13849-1. The purpose of this section is to inform the reader of the activated configuration governing the cell and robot movements. __This is strictly an informational section, there are no instructions included. In no case should the SafeOperation be tampered with. As a user, do not adjust any of the activated safety parameters.__

The SafeOperation Package provides the KRC5 with the ability to monitor the robot’s Cartesian position, conducted movements and velocity. The software package functions by continuously comparing these values against the configured limits. Primarily, the package is implemented for constant observance of the robot’s TCP position and its relation to the activated cell area. In short, SafeOperation ensures the robot will not violate established movement limits, thus guaranteeing the safety of all personnel and equipment. The cell boundary is exactly aligned with the perimeter fence and wall with all set values using the WORLD coordinate system. Do not attempt to adjust these values. 

SafeOperation uses two primary factors for configuration, the cell area and ‘Safe Tools’. The cell area is the defined space in which the robot is allowed to conduct movements. The cell area aligns with the length, width, and enclosed floor of the cell’s surrounding fence. Movements are halted if they are deemed at risk of violating this boundary. 

Each mountable tool has a configured monitoring space comprised of modeled spheres; these are referred to as ‘Safe Tools’. The modeled spheres are monitored against the limits of the Cartesian control space or the cell area. Activating the monitoring spheres yields a ‘Safe TCP’. The Safe TCP is the true value monitored against the configured velocity limits and will signal to the robot a halt if the limit is achieved. 
This is a safety rated level, not an axis limit monitor. 

<div style="text-align:center">

 <figure>
    <img src="Images/SafeOperationVisual.jpg"
         alt="Safe Operation Visual" width= "X">
    <figcaption>SafeOperation Visual</figcaption>
</figure>

<span style="color:red">__Refer to *‘KRC5_SafeOperation_3_1’* for details about SafeOperation__ </span>

</div>


# 3. Installation


<span style="color:red">
-	Leveling 
    - Robot first, then table
    - 
-	Anchoring
-	What to remove for transport
    - Water system
    - turntable
</span>

# 4. System Components

__<ins>Robot</ins>__

One KUKA KR 70 R2100 robot is mounted on a pedestal in the system cell. The system performs inspections with commands sent from the operator table located on the outer perimeter of the safety fence. Manual teaching, jogging, and running of programs can be done via the SmartPad.

__<ins>Cabinets</ins>__

There are four cabinets surrounding the system: Computer Cabinet, System Power Cabinet, Controls Cabinet, and the KUKA KRC5. Refer to section 3.2 for further details about specific functionalities. 

__<ins>Tool Hooks</ins>__

The tool hooks are constructed to hold 1 tool each. Tool hooks are designated for each individual tool and are equipped with a proximity sensor to detect when the tool is stored or in use. The status of the hooks and the tools’ present can be viewed from the Innerspec HMI (Section 8.1.5)

__<ins>Turntable</ins>__

The KP1-V500 is a single axis positioner used to rotate the two nest stations from the loading side to the cell. It has a payload capacity of 500kg. The turntable is controlled in the Innerspec HMI. (Section 8.1.1 – Execution Control)

__<ins>Operator Desk</ins>__

The Operator Desk has three monitors displaying 2 computers. Two monitors are dedicated to the ACQ computer displaying InspectionWare and the Innerspec HMI. A separate computer runs the NVR, displaying live camera feeds from the cell and the inspection tank.

__<ins>Water System</ins>__

The water system consists of pumps, filters, and pressure stabilizers. The water system continuously circulates water in the immersion tank, ensuring a clean inspection environment. For further details on the water system, refer to section 4.

## 4.1 Layout

<div style="text-align:center">

 <figure>
    <img src="Images/System Layout.jpg"
         alt="Safe Operation Visual" width= "700">
    <figcaption>System Layout</figcaption>
</figure>

</div>

## 4.2 Cabinets

......

### 4.2.1 Power Cabinet

<div style="text-align:center">

 <figure>
    <img src="Images/Power Cabinet.jpg"
         alt="Power Cabinet" width= "">
    <figcaption>Power Cabinet</figcaption>
</figure>

</div>

The power cabinet contains the connections dedicated to splitting the incoming 480V power to neighboring subsystems and the robot. The power is split into lines of 12V and 24V. System held instruments are connected to the 12V or 24V output depending on their individual specification.

To prevent a possible power surge, the power cabinet is equipped with three, 70 A, fuses. Within the cabinet, each neighboring cabinet is supplied the correct amount of power and are protected by circuit breakers. 

All elements requiring power are grounded at multiple locations within the cell. These grounds are all then fed to the ground bar located at the base of the power cabinet. The final grounding was determined and wired by Spirit’s Electricians.

__DO NOT WORK WITH CONNECTIONS IF THE INCOMING POWER LINE IS CONNECTED TO POWER SOURCE. DO NOT TAMPER WITH THE POWER CABINET. CONSULT INNERSPEC IF PROBLEMS OCCUR.__


### 4.2.2 Controls Cabinet

<div style="text-align:center">

 <figure>
    <img src="Images/Controls Cabinet.jpg"
         alt="Controls Cabinet" width= "">
    <figcaption>Controls Cabinet</figcaption>
</figure>

</div>

The Control Cabinet holds the majority of Beckhoff equipment used to run and communicate with the EtherCAT devices featured with the system. The cabinet holds the CX PC, power supply, input cards and network switches for the PLC to communicate with the systems components. 

Circuit breaker cards are implemented in the control cabinet to protect all necessary equipment. There should be little to no reason for operators to open the controls cabinet nor tamper with any of its devices. Consult Innerspec or the Maintenance Manual for further detail.

### 4.2.3.	Computer Cabinet

The computer cabinet is home to the following components:

__24V Power Supply__

<mark>Used to power all ... modules. Modules that are supplied power include the ... Refer to later sections of this document for detail regarding each module. This power supply can be switched off and on using the .... In the case the power is switched off...</mark>

__NVR Setup__

Network Video Recorder. NVR is the console where camera feed is being directed. This console has a dedicated computer monitor located on the operator’s desk. The monitor displays real-time footage from the cameras surrounding the system and inside the inspection tank. 

__UPS__

Uninterruptible Power Supply. The UPS allows for the computer cabinet and all its components to stay powered for 5 minutes following a system power failure or shutdown. The ACQ, Barcode scanner, PDU, and the NVR are powered by the UPS. In the event of a power failure, the UPS will begin to beep signaling the timer has begun until its reservoir is depleted. This gives the operator time to save data and perform any due diligence before computer power is lost.


__PDU__

Power Distribution Unit. Powers both NDT monitors and the NVR monitor from the UPS.

<mark>__Acquisition Computer (ACQ)__

This computer hosts the main programs interfaced by the user.

- InspectionWare:  Communicates with NDT equipment to trigger sensors, collect data, and display results. 
-	Innerspec HMI: An instance of the HMI is displayed on the ACQ for the operator. The Innerspec HMI resides on the TwinCat PLC

## 4.2.4.	KRC5

The robot is paired with a KRC5 robot controller. The KRC5 holds all vital bus devices necessary for the robot to operate including:

1.	Kuka Power Pack (KPP)

    - The internal and external drive controllers’ power supply

2.	Kuka Servo Pack (KSP)

    - Drive controller for the robot’s axes

3.	Control PC

    - Interfaced with the SmartPad, the PC is responsible for providing a graphical user interface, program creation/correction, sequence control, safety etc.

4.	Cabinet Control Unit

    - Central power distributor for all of the controller’s components. 

5.	Safety Interface Board

    - Incorporating sensing, control and switching; the SIB is the integral part of the safety interface. Used for axis monitoring, SafeOperation and safety I/O.

6.	Resolver Digital Converter

    - Used to obtain the motor position of each axis

<div style="text-align:center">

 <figure>
    <img src="Images/Internal View of Robot Controller.jpg"
         alt="Internal View of Robot Controller" width= "">
    <figcaption>Internal View of Robot Controller</figcaption>
</figure>

</div>

Externally, the controller is constructed with a power supply connection, heat exchanger and a fan. 

The front side of the controller is constructed with a power switch mounting and the controller system panel. The controller system panel displays an LED layout describing the status of the controller. Note: The following image was taken directly from Kuka’s manual, <mark>BA-KR-C4-NA-UL-en. 

<div style="text-align:center">

 <figure>
    <img src="Images/Controller System Panel.jpg"
         alt="Controller System Panel" width= "">
    <figcaption>Controller System Panel (KUKA Documentation)</figcaption>
</figure>

<span style="color:red">__Refer to __‘BA-KR-C4-NA-UL-en’__ for details about the KRC5__ </span>

</div>

# 5.	Water System

Elements of the water system include the skimmer, pump, strainer, pressure sensors, water filters, UV system, flow sensor, and air separator. The water system can be controlled, and the status checked through the Innerspec HMI. 

<div style="text-align:center">

 <figure>
    <img src="Images/Water System Layout.jpg"
         alt="Water System Layout" width= "">
    <figcaption>Water System Layout</figcaption>
</figure>

</div>

__Tank Water Level__

Water is contained in a single location, the tank. The water level of the tank is set at the filling stage, and should be at the midpoint of the skimmer for optimal operation. 

__Water Circulation__

The water circulation system operates by continuously pumping water from the tank through the filtration system. Water is drawn from both the Skimmer and the Tank Outlet, with the flow rate of the skimmer being controlled by Valve 3, located at the Tank Outlet. Ensure that the skimmer weir flap is positioned just below the water surface to allow for laminar flow into the skimmer basket. If the flap is positioned too low, debris will not be effectively trapped in the skimmer basket. To reduce the flow in the skimmer, adjust Valve 3 by opening the Tank Outlet valve.

The water exiting the tank first passes through a preliminary strainer that removes coarse debris (840-170µm) that could damage the device. After straining, the water flows through the pump and then to the first of two pressure switches. The filtration system includes two standard filters, rated at 10 and 25 microns. The water pressure is measured again after passing through the filters by the second pressure switch to monitor the pressure differential. If the pressure differential exceeds 10 PSI, it is recommended to replace one or both of the filters. The pressure differential and other parameters can be viewed from the main dashboard of the Innerspec HMI. 

<div style="text-align:center">

 <figure>
    <img src="Images/Pressure Differential Reading.jpg"
         alt="Pressure Differential Reading" width= "">
    <figcaption>Pressure Differential Reading</figcaption>
</figure>

</div>

Several factors influence the health of the filter including run time, idle-time and ambient conditions. These factors deem it impossible to determine the exact instance in which a filter will need to be exchanged. 

The filter will not remove algae or any microorganisms that accumulate in the water. Following filtration, water is directed from the filter into a UV lamp to eliminate the chances of microorganism over growth. 

An air separator is included in the system to remove any trapped air, optimizing system performance. Finally, the water passes through a flow sensor before being returned to the tank.

<div style="text-align:center">

 <figure>
    <img src="Images/Water System Diagram.jpg"
         alt="Water System Diagram" width= "">
    <figcaption>Water System Diagram</figcaption>
</figure>

</div>

## 5.1. Tank Empty Procedure

-	Close valve 1 and 2. Make sure valve 3 is completely open.
-	Connect a hose to spigot 2 or open valve 4 to empty the tank via the barb fitting.
-	Turn on the pump

## 5.2. Tank Refill Procedure

-	Close valve 2, 3, and 4. 
-	Open valve 1.
-	Connect a hose to spigot 1.
-	Turn on the pump.

## 5.3. Filter Replacement Procedure

Step 1: Close valves 1, 2, and 3 to prevent additional water from entering the system.

Step 2: Place a bucket under or attach a hose to valve 4 and drain water from the system.

Step 3: Once all water is drained, place a bucket under Filter 1 and unscrew it from the manifold using the filter wrench. 

Step 4: Remove the old filter and replace. 

Step 5: Screw the filter onto the manifold. Take care to align the filter with the circular protrusion inside the manifold to prevent damage to the filter. 

Step 6: Follow Steps 3-5 for the second filter.

Step 7: Turn off valve 4 and turn valves 1, 2, and 3 back on. 

Step 8: Bleed air using the bleeder valves on top of the filter manifolds.

Step 9: Click Reset Total Flow in the Maintenance section of the Innerspec HMI (Section 9.8.2) and turn on the pump.  

# 6.	Pneumatics

The supply point of the system’s air is located between the power cabinet and the water tank. Compressed air introduced from the facilities should be connected here. The inlet of air is controlled and regulated by a SMC pressure valve and dial, respectively. 

The pressure valve has two positions, ‘EXH’ and ‘SUP’. ‘EXH’ will close the valve, exhausting the supply. ‘SUP’ labels the valve as open allowing air to be fed into the system.

The pressure dial rests atop of the air filter, the dial controls the total pressure flowing into the system. The total pressure to the system should remain at <mark>90-100</mark> PSI for functionality. This is the minimum air pressure that must be provided by the facility. 

The attached filter is the only air filter located on the system. It should be checked regularly for any particle, water, or oil build up. To clean the filter, refer to the maintenance manual. 

The air is sent directly from the pressure switch to the air assembly at the robot base where it is then distributed throughout the cell. 

|             | Description                         | Tool Port#  | Label       |
| ----------- | -----------                         | ----------- | ----------- |
| Valve 1     | Master Tool Lock/Unlock             | NA          |             |
| Valve 2     | Master Tool Collision Sensor        |  1          |             |
| Valve 3     | Master Tool Gripper Close           |  2          |             |
|             | Master Tool Gripper Open            |  3          |             |
| Valve 4     | Master Tool Vacuum Generator 1      |  4          |             |
| Valve 5     | Master Tool Vacuum Generator 2      |  5          |             |
| Valve 6     | Tank Flat Probe UP                  | NA          |             |
|             | Tank Flat Probe DOWN                | NA          |             |
| Valve 7     | Tank Curved Probe UP                | NA          |             |
|             | Tank Curved Probe DOWN              | NA          |             |
| Valve 8     | Tank Flat Probe Collision           | NA          |             |
| Valve 9     | Tank Curved Probe Collision         | NA          |             |
| Valve 10    | Tank Flat Probe Rotate Horizontal   | NA          |             |
|             | Tank Flat Probe Rotate Vertical     | NA          |             |
| Valve 11    | Regrip Vacuum Generator 3           | NA          |             |

<figure>
<figcaption>Air Distribution</figcaption>
</figure>

<mark> Include, how to use and where they’re at. Sandwich regulators. 

<div style="text-align:center">

 <figure>
    <img src="Images/System Air Supply and Manifold.jpg"
         alt="System Air Supply and Manifold" width= "500">
    <figcaption>System Air Supply and Manifold</figcaption>
</figure>

</div>

