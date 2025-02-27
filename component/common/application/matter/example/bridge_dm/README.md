# Bridge-app using Ameba Data Model Example
This example is an implementation of the *Bridge* device type. The Bridge will be communicating with the other non-Matter peripherals via TCP sockets.
You will need 2 non-Matter peripherals running TCP client socket.

## Ameba Data Model
This example demonstrates adding and removing endpoints dynamically using the *Ameba Data Model*.
A `Root Node` device type will be created on Endpoint0, a `Aggregator` device type on Endpoint1 and a `Dimmable Light` device type on Endpoint2.
After 20 seconds delay, the `Dimmable Light` endpoint on Endpoint2 will be removed.

Additionally a new thread will be created for user to input their code to communicate with non-matter device based on the protocol (e.g., IP-based (TCP,UDP), BLE, zigbee and etc) they wish to use.

## How to build

### Configurations
Enable `CONFIG_EXAMPLE_MATTER` and `CONFIG_EXAMPLE_MATTER_BRIDGE` in `platform_opts.h`.
Ensure that `CONFIG_EXAMPLE_MATTER_CHIPTEST` is disabled.

### PSRAM usage
Due to insufficient memory in SRAM, we will use the PSRAM for dynamic allocation.
To run this example without error, you need to enable PSRAM.
In `rtl8721dhp_intfcfg.c`, set the below configurations

    PSRAMCFG_TypeDef psram_dev_config = {
      .psram_dev_enable = TRUE,			//enable psram
      .psram_dev_cal_enable = TRUE,			//enable psram calibration function
      .psram_dev_retention = TRUE,
    }

### Setup the Build Environment
  
    cd connectedhomeip
    source scripts/activate.sh
  
### Build Matter Libraries

    cd ambd_matter/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp
    make -C asdk bridge_dm
    
### Build the Final Firmware

    cd ambd_matter/project/realtek_amebaD_va0_example/GCC-RELEASE/project_lp
    make all
    cd ambd_matter/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp
    make all
    
### Flash the Image
Refer to this [guide](https://github.com/ambiot/ambd_matter/blob/main/tools/AmebaD/Image_Tool_Linux/README.txt) to flash the image with the Linux Image Tool

### Clean Matter Libraries and Firmware

    cd ambd_matter/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp
    make clean
