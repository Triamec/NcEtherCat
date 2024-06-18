# TwinCAT Example using NC and EtherCAT
[![TwinCAT - EtherCAT](https://img.shields.io/static/v1?label=TwinCAT&message=EtherCAT&color=b51839)](https://www.triamec.com/de/beckhoff-tam-integration-ethercat.html)

This TwinCAT 3 application example helps you getting started to use a *Triamec* drive with *EtherCAT* fieldbus.

> [!NOTE]
> Example tested with TwinCAT *v3.1.4024.56* and following libraries:

![Library](./doc/Library.png)

## Prerequisites

The following prerequisites have to be met using this example code.

- A TwinCAT 3 runtime system (3.1.4024 or newer) with an EtherCAT-Master.
- *Triamec EtherCAT Lib* 2.1.0 or newer installed. Available as download from [triamec.com](https://www.triamec.com/en/ethercat.html).
- Drive commissioned. Checkout the *Servo Drive Setup Guide* from the [documents](https://www.triamec.com/en/documents.html) page.
- Drive controlled homing configured on the drive (*Axes[0]/Parameters/Homing*). Checkout *AN141* from the [documents](https://www.triamec.com/en/documents.html) page.
- All drives must be powered and connected to the EtherCAT-Master.

## EtherCAT-Master Adapter
Open adapter settings for the EtherCAT-Master in **I/O > Devices > Device 1 (EtherCAT) > Adapter**.

Press **Search...** fo find the adapter on your system.

![EtherCAT Master Adapter](./doc/EtherCATMaster.png)

> [!NOTE]
> Make sure, that TwinCAT is in **Config Mode** before you search the *EtherCAT Master* Adapter.

## TwinCAT SYSTEM

Make sure to use an isolated core on the TwinCAT PC. As each PC is different, ensure to set up the **SYSTEM > Real-Time > Settings** accordingly. 

## Global Variable List (Triamec_GVL)

The following global variables have been defined to control and monitor the axes.

| Variable              | Description                                 |
| --------------------- | ------------------------------------------- |
| `gEnableAxes`         | variable to enable all axes                 |
| `gStatusAxesEnabled`  | variable indicates that all axes enabled    |
| `gHoming`             | variable to execute drive controlled homing |

## NC Axis Function Block (NC_Axis)

The function block extends *TE_AxisBase0* available in the *Triamec EtherCAT Lib* by the NC specific interfaces. It makes use of TwinCAT motion control PLC library function blocks (MC_Power / MC_Reset) to control the axis
The **NC_Axis** contains an instance of **DriveControlledHoming** function block, which allows to execute the *drive controlled homing*.

## Test the Example

- **Save** and **Rebuild** the Solution.
- **Activate** the configuration and set TwinCAT to *Run Mode*.
- **Login** and **Start** the PLC.
- Open *Triamec_GVL* and enable the axes by setting **gEnableAxes** to **TRUE**, check **gStatusAxesEnabled** if all axes are enabled.

### Ready to move the axes

You should now be able to control the axes over NC Axis GUI (Online GUI).
The buttons **-- F1**, **- F2**, **+ F3** and **++ F4** are in the **MOTION > NC-Task 1 SAF > Axes > Axis N > Online** dialog.

![Online Dialog](./doc/OnlineDialog.png)

### Drive controlled homing
While axes are enabled, **gEnableAxes** and **gStatusAxesEnabled** are **TRUE**, start the drive controlled homing by setting **gHoming** to **TRUE**. The configured homing will start. An successful homing will be indicated by the **NC_Axis.referenced** output.

> [!NOTE]
> For drive controlled homing, the following two signals are mapped to the I/O variables of **DriveControlledHoming* function block:
> I/O MainOutputs/Mode of Operation --> MAIN.Axis[0].drvHoming.modeOfOperation
> I/O MainInputs/Status Word --> MAIN.Axis[0].drvHoming.statusWord (and to NC Axis)

