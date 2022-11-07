I don't have docker compose, but I do have `docker`. So I ran the following to bring up the rotorflight container: 

```bash
docker build -t rotorflight_docker . # Build the container image 
docker run -it -v "/$(pwd):/rotorflight" --name rotorflight --rm rotorflight_docker /bin/bash
```

Then configured the `arm` build environment (I'm guessing)

```
root@45d102a27445:/rotorflight# make arm_sdk_install
```

Commented out the `#undef USE_OSD` and `#undef use_MAX7456` lines in [STM32_UNIFIED/target.h](https://github.com/rotorflight/rotorflight-firmware/blob/master/src/main/target/STM32_UNIFIED/target.h). I'm not strictly sure commenting out `#undef use_MAX7456` is necessary for digital, but I guess I'm erring on the side of caution here. 

And compiled the `STM32F7X2` target (presuming this is appropriate for my FC running a `STM32F722`):

```
root@45d102a27445:/rotorflight# make STM32F7X2
```



- Flash this firmware 
- Apply BETAFLIGHT defaults/target (RUSH-BLADE_F7_HD_betaflight.config)
- -> Battery voltage is present. 
- Apply [My Rotorflight mapping](https://docs.google.com/spreadsheets/d/1GOPNIWBpl4pt3RZKh50ooZDOG34uL7X_9s8GLVSmO5M/edit#gid=153958966) `Blade_Rotorflight_Mapping.txt`
- -> Battery voltage *still* present. 
- `set osd_displayport_device = MSP` (no effect)
- 
```
set osd_displayport_device = AUTO
set vcd_video_system = NTSC
```

**Try**
- [ ] Flash this (FPV) firmware 
- [ ] Apply MY RotorFlight target (`My_Blade_Rotorflight_target.txt`)
- [ ] Battery voltage is present?
- [ ] Run commands
    ```
    set osd_displayport_device = AUTO
    set vcd_video_system = NTSC
    ```
- [ ] Update DJI Goggles and Air Unit firmware. 


**Try**
- [x] Flash the **REGULAR**  Blade firmware 
- [x] Apply custom defaults 
- [x] Battery voltage is present? (Yes)
- [ ] ~~Apply MY RotorFlight target (`My_Blade_Rotorflight_target.txt`)~~
- [ ] ~~Battery voltage is present?~~
- [x] Apply [MY RotorFlight Mapping](https://docs.google.com/spreadsheets/d/1GOPNIWBpl4pt3RZKh50ooZDOG34uL7X_9s8GLVSmO5M/edit#gid=153958966) (`Blade_Rotorflight_Mapping.txt`)
- [x] Battery voltage is present? (Yes)
- [ ] **What the fuck turning on the receiver breaks the MSP connection to the DJI goggles.**
  - Wait no now it's working. 
  - Omg I'm such a fucking idiot. 
  - You need to use the onboard ADC to measure the battery voltage, OR have the motor plugged in and it set to ESC sensor (which I don't, at the moment). Also, it may just not work at all with the ESC sensor method. 
- [ ] Run commands
    ```
    set osd_displayport_device = AUTO
    set vcd_video_system = NTSC
    ```
- [ ] Update DJI Goggles and Air Unit firmware. 

