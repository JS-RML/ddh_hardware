# Direct-Drive Gripper

The repo contains the hardware design and documentation of our in-house 2-finger 4-DoF direct-dirve gripper.

![ddh](images/ddh.gif)



# Table of Contents

- [Preparation](#preparation)
  - [Bill of Materials (BOM)](#bom)
    - [Purchase](#purchase)
    - [3D Printing](#3d-printing)
    - [Mounting]()
  - [Label Components]()
- [Actuators](#actuators)
  - [Actuator Assembly ⨉4](#assemble-actuators)
  - [Wiring](#wiring)
    - [Encoder Connection](#encoder-connection)
    - [Power Connection](#power-connection)
  - [Actuator Calibration](#actuator-calibration)
    - [Calibrate ODrives](#calibrate-odrives)
    - [Calibrate Encoders](#calibrate-encoders)
- [Gripper](#Gripper)
  - [Finger Assembly ⨉2](#finger)
  - [Gripper Assembly](#gripper-assembly)
  - [Mounting](#mounting)
  - [Gripper Calibration](#configuration-and-calibration)
    - [Calibrate Motor Direction](#calibrate-motor-direction)
    - [Calibrate Linkages](#calibrate-linkages)
    - [Configure Gripper Geometry](#configure-gripper-geometry)



# Preparation


<a name="bom"></a>
## Bill of Materials (BOM)



### Purchase

- [ODrive 3.6-56V](https://odriverobotics.com/shop/odrive-v36) ⨉2
- [T-Motor GB54-2](https://store.tmotor.com/goods.php?id=445) ⨉4
- [AS5048A Encoder + Diametrical Magenet](https://item.taobao.com/item.htm?id=619004953504) ⨉4
- [Bearing - outer-diameter = 10mm, inner-diameter = 6mm](https://item.taobao.com/item.htm?id=649671875461) ⨉12
- [Dowel Pin - diameter = 6mm, length = 10mm](https://detail.tmall.com/item.htm?id=522002486638) ⨉6
- [14-Core Shielded Cable TRVVSP4](https://detail.tmall.com/item.htm?id=649477061772) ⨉2 meters
- [MX1.25-6P 150mm Cable](https://item.taobao.com/item.htm?id=607231799768) ⨉4
- [AMASS Braided 3-Phase 90cm Cable](https://item.taobao.com/item.htm?id=520248392055) ⨉8
- Mounting Couplers (*)
- Various Fasteners from M2 to M4



### 3D Printing 

- Magnet Holder ⨉4
- Motor Plate ⨉4
- Proximal Link ⨉4
- Distal Link ⨉2
- Distal Tip Link ⨉2
- Finger Tip ⨉2
- Actuator Mount ⨉1
- Coupler ⨉1




### Mounting (\*) 

The mounting coupler is designed to work with the mounts for Robhotiq 3f Adaptive robot gripper and Universal robhot UR10 (50mm PCD with 4 x M6). You will need to design your own coupler if you use other robot models. 




## Label Components


There are two ways to mount the gripper on the robot arm. They are equivalent as long as you keep it consistent.



![ref_frame](images/ref_frame.png)



# Actuators


<a name="assemble-actuator"></a>
## Actuator Assembly ⨉4


![motor_with_magnet](images/motor_with_magnet.png)

![motor-plate](images/motor-plate.png)

![actuator_module](images/actuator-module.png)


## Wiring

In this step, you need to connect the actuators to the ODrive boards. Each actuator has an encoder port and a 3-phase power port. Both need to be connected.


### Encoder Connection

For the encoder connection, you need to construct a encoder cable accroding the schematics below. This cable is quite tricky to make. Test the connectivity and resistance of each connection afterwards to make sure the cable is soldered properly. It is also recommanded to physically label each connector with its designated identifier.

![drag_cable](images/wiring.png)

Plug-in the encoder cable to the corresponding actuators. There is only one corret orientation for the plugs. Connect the other end of the encoder cable to the corresponding ODrive pins.

### Power Connection

For the power connection, connect each actuator to its designeated ODrive axis. 

| Actuator | ODrive | Axis |
|----|------|-----|
| R0 | ODrive_R | M0 |
| R1 | ODrive_R | M1 |
| L0 | ODrive_L | M0 |
| L1 | ODrive_L | M1 |

Keep the 3-phase connection consistent following the convention shown below.

![wireing_power](images/wiring-power.png)



## Actuator Calibration



### Calibrate ODrives



### Calibrate Encoders

The following values in the configuration will be calibrated in this step

- `/motors/R0/offset`
- `/motors/R1/offset`
- `/motors/L0/offset`
- `/motors/L1/offset`



![](/Users/sheep/Code/ddh_hardware/images/motor-calib-stand.png)

![](/Users/sheep/Code/ddh_hardware/images/calib-zero.png)

# Gripper

<a name="finger"></a>

## Finger Assembly ⨉2

![linkage_joint](images/linkage_joint.png)

![finger](images/finger.png)


## Gripper Assembly

![gripper_shell](images/gripper_shell.png)

![gripper](images/gripper.png)

## Mounting

![mounting](images/mounting.png)

![gripper_mounted](images/gripper_mounted.png)



## Gripper Calibration



### Calibrate Motor Direction

The following values in the configuration will be calibrated in this step

- `/motors/R0/dir`
- `/motors/R1/dir`
- `/motors/L0/dir`
- `/motors/L1/dir`



### Calibrate Linkages

The following values in the configuration will be calibrated in this step

- `/linkages/R0`
- `/linkages/R1`
- `/linkages/L0`
- `/linkages/L1`

```yaml
linkages:
  R0: -45
  R1: 45
  L0: 45
  L1: -45
```



### Configure Gripper Geometry

![geometry](images/ddh_geometry.png)

```yaml
geometry:
  l1: 50 # length of proximal links
  l2: 35 # length of distal links
  beta: 150 # angle from l2 to finger surface 
  l3: 80.04 # distance from distal joint to fingertip
  gamma: 160.66 # angle from l2 to fingertip
  # r: distance of distal joint to motor
  r_min_offset: 0 # r_min = sqrt(l1**2 - l2**2) + r_min_offset
  r_max_offset: 1 # r_max = l1 + l2 - r_max_offset
```

