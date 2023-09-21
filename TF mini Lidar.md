# TF mini Lidar
Here I list sources, sheets, and guides related to TF mini Lidar.

# Sheets
These links are for Micro LIDAR TFMINI-S Benewake UART (12m) :

Purchase page: [click here](https://ca.robotshop.com/fr/products/benewake-tfmini-s-micro-lidar-module-uart-12m)

Technical sheet (Data sheet): [click here](https://cdn.robotshop.com/media/b/ben/rb-ben-15/pdf/benewake-tfmini-s-micro-lidar-module-uart-12m-datasheet.pdf)

Manual: [click here](https://cdn.robotshop.com/media/b/ben/rb-ben-15/pdf/sj-pm-tfmini-s_a00_product_mannual_en_1_.pdf)

TFmini configuration on PX4_Autopilot: [click here](https://docs.px4.io/main/en/sensor/tfmini.html)


# Code
TF miniLidar in firmware (PX4-Autopilot): [click here](https://github.com/PX4/PX4-Autopilot/blob/78eb0cdb728031fd315d6491b33328a3cad1cbf9/src/drivers/distance_sensor/tfmini/TFMINI.cpp)

# Read distance from miniLidar connected to mRo pixracer using MAVLink Inspector of QGroundControl

## Wiring
To make physical connections (wiring) between pixracer and TF Mini Lidar,  a [kit](/Figures/kit_between_pixracer_and_Lidar.jpg) has been used with which not only does it enable making connections between mRo Pixracer and TF miniLidar, but also it allows making connections between this flight controller and many other modules, sensors, actuators, etc. as pixracer ports are mostly from GH 1.25 type.

According to the [TF miniLidar Manual](https://cdn.robotshop.com/media/b/ben/rb-ben-15/pdf/sj-pm-tfmini-s_a00_product_mannual_en_1_.pdf), the GND, TX, RX, and +5V pins of the the TF miniLIdar are as the following, 

![TF miniLidar map](/Figures/TF_mini_Lidar.png)

 Moreover, from [Pxracer wiring quick start](https://docs.px4.io/main/en/assembly/quick_start_pixracer.html) page of PX4-Autopilot manual, we have the following mapping for the Pixracer board,

 ![Pxracer map](/Figures/Pixracer_map_zoomed.jpg)

 By paying attention to these infographs and using the aforementioned kit with its provided tool, the following wire has been made one side of which has male 4-pinned GH 1.25 and another side has a male 6-pinned GH 1.25,

 ![4_to_6 GH 1.25 wire](/Figures/4_to_6_wire.jpg)

 Please note that like any UART serial port, the TX pin of the TF mini Lidar should be connected to the RX pin of mRo Pixracer and the RX pin of it should be connected to the TX pin of mRo Pixracer.

since TELEM1 port of mRo pixracer is used for the TF miniLidar connection, which has 6 pins, the other end of the created wire is a 6-pinned GH 1.25 and not 4-pinned.

## firmware and the QGroundControl

mRo Pixracer has been connected to a local PC machine having QGroundControl installed. 

![pixracer_Attached_to_PC_and_mini_Lidar](/Figures/Pixracer_attached_to_PC_and_miniLidar.jpg)

Having more tutorials and being more stable, PX4_Autopilot version 1.12.3 has been uploaded to the board using these commands in the terminal:

``` C++
cd PX4-Autopilot
make px4_fmu-v4_default upload
```
Refering to [this page](https://docs.px4.io/main/en/sensor/tfmini.html) of PX4-Autopilot manual which is about configuring TF miniLidar, after uploading the firmware to the flight controller, parameter SENS_TFMINI_CFG should be configured to TELEM1 in QGroundControl. This parameter can be found in Vehicle Setup --> Parameters --> Sensors --> SENS_TFMINI_CFG, or it can be found simply by searching through parameters in the parameters panel of the vehicle setup in QGroundControl.

![SENS_TFMINI_CFG](/Figures/SENS_TFMINI_CFG.png)

Afterward, from the main page of QGroundCOntrol, please go to the Analyze tools and then MAVLink Inspector. Then, find DISTANCE_SENSOR. In fact, the 'current distance' row in the table shows the real time distance measured by TF miniLidar.

![QGC distance_sensor](/Figures/QGC_sensor_data.png)

Note that the distance measurement unit is in centimeter. It can measure distances from 10 centimeter to 1200 centimeter with the resolution of one centimeter.

## Next step

Although the task has been acheived, before trying to integrate multiple miniLidar to the pixracer, I prefer to delve into PX4-Autopilot source code deeper, and manually subscribe to the distance_sensor.msg message in the PX4-AUTOPILOT/msg path, as finally this distance needs to be recorded and published to other nodes.




