# Hall sensor wheel tick to ESF-MEAS conversion box.

This small project converts the 6 Hall sensor signals from two motors (e.g from a small robot) to a ``UBX-ESF-MEAS`` mesage that can be directly injected into the UART port (``ZED-RXI``) of the HPG solution board. You can probably use any Arduino that has 7 spare GPIOs and at least one UART. Any u-blox Arduino compatible modules like NINA-B10x, ANNA-B10x, NINA-W10x or NORA.W10x will probably work well.

Each motor has three hall sensors that represent 6 states, if the state is increading it means it is moving forward, if decreasing it means it moved backwards. For vehicles with two independent wwheels one of the motor might be reversed as it is mounted in 180 degrees compared to the other one. . 
```
State:  -000-111-222-333-444-555-000-111-222-333-444-555-000-111-222-333-444-555-
        |   .   .   .   .   .   |   .   .   .   .   .   |   .   .   .   .   .   | 
Hall C: ________/-----------\___________/-----------\___________/-----------\____
        |   .   .   .   .   .   |   .   .   .   .   .   |   .   .   .   .   .   |  
Hall B: ----\___________/-----------\___________/-----------\___________/--------
        |   .   .   .   .   .   |   .   .   .   .   .   |   .   .   .   .   .   | 
Hall A: ------------\___________/-----------\___________/-----------\___________/
        |   .   .   .   .   .   |   .   .   .   .   .   |   .   .   .   .   .   | 
``` 

The following picture shows how the board is connected to the wires, at the time of this photo the non default ``D8`` was used for ``D4 TX1 -> ESF-MEAS -> ZED RXI``, this was later changed to the default ``D4 TX1`` location which links with the status LED. I used a WEMOS D1 mini that was just lying around in my drawer. and connected all signals with 1k Ohm resoistors to have them a bit decoupled. 

![Mower conversion box](../../docu/Mower_WtBox.png)

Here is the default pin assignement of the WT conversion code when running on the board I used. 

```
                          --- ANTENNA ---
                          |        LED  |
                          RST          TX -> CDC @ 115200
                          ADC0         RX 
      BLUE / HALL_RL_A -> D0,WAKE      D1 <- HALL_RL_B / YELLOW 
     GREEN / HALL_RR_C -> D5           D2 <- HALL_RL_C / GREEN
    YELLOW / HALL_RR_B -> D6   CHIPS   D3
      BLUE / HALL_RR_A -> D7   LED,TXI,D4 -> ESF-MEAS @ 38400 -> ZED RXI
                          D8          GND
                          3v3          5V <- IN
                          ---------------
                                USB
```

Please check the [Mower](../../docu/Mower.md) documentation for an example use of this conversion box. 
