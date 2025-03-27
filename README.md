# Ethernet XMClib freertos lwip mbedtls

This repo comprises core components needed for ethernet connectivity support. The library bundles FreeRTOS, lwIP TCP/IP stack, mbed TLS for security, connectivity utilities and configuration files.

## Features and functionality

This library provides the configuration files for lwIP network stack and mbedTLS security stack. It also includes following libraries as dependees in the ModusToolbox&trade; manifest system. Using ModusToolbox&trade; manifest system the dependees are automatically pulled when an application uses this library in ModusToolbox&trade; environment.

- **FreeRTOS for Infineon MCUs:** FreeRTOS kernel, distributed as standard C source files with the configuration header file, for use with the Infineon MCUs. See 
[Readme](https://github.com/Infineon/freertos/blob/master/README.md) for details.

- **CLib FreeRTOS support library:** This library provides the necessary hooks to make C library functions such as malloc and free thread-safe. This implementation is specific to FreeRTOS; this library is required for building your application. See the [CLib FreeRTOS support library](https://github.com/Infineon/clib-support) web site for details.

- **lwIP:** A Lightweight open-source TCP/IP stack, version: 2.1.2. See the [lwIP](https://savannah.nongnu.org/projects/lwip/) web site for details.

   **Note:** Using this library in a project will cause lwIP to be downloaded on your computer. It is your responsibility to understand and accept the lwIP license.

- **lwIP FreeRTOS integration library:** This repo contains the FreeRTOS dependancies needed by the Lightweight open-source TCP/IP stack. See [lwIP FreeRTOS integration library](https://github.com/Infineon/lwip-freertos-integration) for details.

- **lwIP network interface integration library:** This library is an integration layer that links lwIP network stack with underlying Wi-Fi Host Driver(WHD) and Ethernet driver. See [lwIP network interface integration library](https://github.com/Infineon/lwip-network-interface-integration) for details.

- **mbed TLS:** An open-source, portable, easy-to-use, readable and flexible SSL library that has cryptographic capabilities, version: 3.4.0. See the [mbed TLS](https://tls.mbed.org/) web site for details.

   **Note:** Using this library in a project will cause mbed TLS to be downloaded on your computer. It is your responsibility to understand and accept the mbed TLS license and regional use restrictions (including abiding by all applicable export control laws).

- **RTOS abstraction layer:** The RTOS abstraction APIs allow middleware to be written to be RTOS-aware, but without depending on any particular RTOS. See the [RTOS abstraction layer](https://github.com/Infineon/abstraction-rtos) repository for details.

- **Connectivity utilities:** The connectivity utilities library is a collection of general purpose middleware utilities. See the [Connectivity utilities](https://github.com/Infineon/connectivity-utilities) repository for details.

- **Predefined configuration files:** For FreeRTOS, lwIP, and mbed TLS for typical embedded IoT use cases. See **Quick Start** section for details.

## Supported platforms

This library and its features are supported on the following platforms:

- [XMC4700 Relax Kit (KIT_XMC47_RELAX_V1)](https://www.infineon.com/cms/en/product/evaluation-boards/kit_xmc47_relax_v1/)

- [XMC4800 Relax EtherCAT Kit (KIT_XMC48_RELAX_ECAT_V1)](https://www.infineon.com/cms/en/product/evaluation-boards/kit_xmc48_relax_ecat_v1/)

- [XMC4500 Relax Kit (KIT_XMC48_RELAX_V1)](https://www.infineon.com/cms/en/product/evaluation-boards/kit_xmc45_relax_v1/)

## Quick start
 
A set of pre-defined configuration files have been bundled with this library for lwIP, and mbed TLS. These files are located in the configs folder.

You should do the following:

1. Copy *lwipopts.h*, and *mbedtls_user_config.h* files from the *configs* directory to the top-level code example directory in the project.
   
2. Configure the `MBEDTLS_USER_CONFIG_FILE` C macro to mbedtls_user_config.h in the Makefile to provide the user configuration to the mbed TLS library. The Makefile entry should look like as follows:
   
    ```
    DEFINES+=MBEDTLS_USER_CONFIG_FILE='"mbedtls_user_config.h"'
    ```
   
3. Add the `CYBSP_ETHERNET_CAPABLE` build configuration to enable the ethernet functionality. The Makefile entry should look like as follows:

    ```
    DEFINES+=CYBSP_ETHERNET_CAPABLE
    ```

4. Add the `XMC_ETH_PHY_KSZ8081RNB` build configuration to enable the supported ethernet driver. The Makefile entry should look like as follows:

    ```
    DEFINES+=XMC_ETH_PHY_KSZ8081RNB
    ```

5. Add the `CY_RTOS_AWARE` build configuration to inform the HAL that an RTOS environment is being used. The Makefile entry should look like as follows:

    ```
    DEFINES+=CY_RTOS_AWARE
    ```

6. If your application uses automatic private IP addressing (Auto IP), enable `LWIP_AUTOIP` and `LWIP_DHCP_AUTOIP_COOP` in *lwipopts.h* like as follows:

    ```
    #define AUTOIP 1
    #define LWIP_DHCP_AUTOIP_COOP 1
    ```

7. Add the following to `COMPONENTS` in the code example project's Makefile: `FREERTOS`, `LWIP`, and `MBEDTLS`.

    For example:
    
    ```
    COMPONENTS=FREERTOS LWIP MBEDTLS
    ``` 

8. To configure the Ethernet on [XMC4700/XMC4800 Relax Kit](https://www.infineon.com/cms/en/product/evaluation-boards/kit_xmc47_relax_v1/) and [XMC4500 Relax Kit](https://www.infineon.com/cms/en/product/evaluation-boards/kit_xmc45_relax_v1/), open design.modus file and do the following configuration settings in the ModusToolbox&trade; Device Configurator.
    - Switch to the "System" tab.
    - Select "Clock Control Unit->"Ethernet Clock". <br>
    **Note:**  The minimum acceptable Ethernet clock frequency is 50 MHz.<br>
    - For XMC4700 device, default Ethernet Clock frequency is 72 MHz.
    - For XMC4500 device, default Ethernet Clock frequency is 36 MHz. Follow below steps to configure Ethernet Clock frequency.
        - Select "Clock Sources"->"System PLL". Set Desired Frequency (MHZ) to 120MHz.
        - Select "Clock Control Unit"->"System Clock". Set Divider to 1.
        - Select "Clock Control Unit"->"Ethernet Clock". Set Divider to 2. This will configure Ethernet clock frequency to 60 MHz.
    - Switch to the "Peripherals" tab.
    - Select Communication->"Ethernet (ETH)" for XMC4700 device.
    - In "Ethernet (ETH) - Personality" tab, select "ETH_MAC_MW-1.0" from the drop-down.
    - In "Ethernet (ETH) - Parameters" pane, configure interface type "PHY Interconnect" to "RMII" from the drop-down.
    - Configure PHY driver "PHY Device" to "KSZ8081RNB" from the drop-down.
    - Enable/Disable "Autonegotiation" (default is enable).
    - Configure "PHY speed" (10/100 Mbits/sec) based on the device. <br>
    **Note:** "PHY speed" and "PHY Duplex Mode" options are visible only when the "Autonegotiation" is disabled. <br>
    - Configure "Mac Address".
    - Enable "Promiscuous mode" (default is disable).
    - Enable "Accept Broadcast Frames" (default is enable).
    - Configure static IP or use DHCP (default is use DHCP).
    - To configure pin connections, refer "Ethernet" section of [XMC4700/XMC4800 Relax Kit User Manual](https://www.infineon.com/dgdl/Infineon-Board_User_Manual_XMC4700_XMC4800_Relax_Kit_Series-UserManual-v01_06-EN.pdf?fileId=5546d46250cc1fdf01513f8e052d07fc) or [XMC4500 Relax Kit User Manual](https://www.infineon.com/dgdl/Infineon-xmc4500_rm_v1.6_2016-UM-v01_06-EN.pdf?fileId=db3a30433580b3710135a5f8b7bc6d13). Below is an example for pin configurations.
        - For ETH_CRS_DV pin, Select "CYBSP_ETH_CRS" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input" from the drop-down.
            - Configure "Input mode" to "Tristate" from the drop-down.
        - For ETH_RX_ER pin, Select "CYBSP_ETH_RXER" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input" from the drop-down.
            - Configure "Input mode" to "Tristate" from the drop-down.
        - For ETH_RXD0 pin, Select "CYBSP_ETH_RXD0" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input" from the drop-down.
            - Configure "Input mode" to "Tristate" from the drop-down.
        - For ETH_RXD1 pin, Select "CYBSP_ETH_RXD1" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input" from the drop-down.
            - Configure "Input mode" to "Tristate" from the drop-down.
        - For ETH_TX_EN pin, Select "CYBSP_ETH_TXEN" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input/ouptut" from the drop-down.
            - Configure "Driver Strength" to "Strong Driver Sharp Edge" from the drop-down.
        - For ETH_TXD0 pin, Select "CYBSP_ETH_TXD0" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input/ouptut" from the drop-down.
            - Configure "Driver Strength" to "Strong Driver Sharp Edge" from the drop-down.
        - For ETH_TXD1 pin, Select "CYBSP_ETH_TXD1" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input/ouptut" from the drop-down.
            - Configure "Driver Strength" to "Strong Driver Sharp Edge" from the drop-down.
        - For ETH_CLKRMII pin, Select "CYBSP_ETH_CLK" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input" from the drop-down.
            - Configure "Input mode" to "Tristate" from the drop-down.
        - For ETH_MDC pin, Select "CYBSP_ETH_MDC" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "input/ouptut" from the drop-down.
            - Configure "Driver Strength" to "Strong Driver Sharp Edge" from the drop-down.
        - For ETH_MDO pin, Select "CYBSP_ETH_MDIO" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "Hardware Controlled" from the drop-down.
            - Configure "Driver Strength" to "Strong Driver Sharp Edge" from the drop-down.
        - For ETH_MDI pin, Select "CYBSP_ETH_MDIO" from the drop-down. 
            - Click on "Go to signal" of selected pin,
            - Configure "Pin Direction" to "Hardware Controlled" from the drop-down.
            - Configure "Driver Strength" to "Strong Driver Sharp Edge" from the drop-down.	
    - Save the configuration to generate the necessary code.

9. XMC4000 devices supports both Rx Polling and Interrupt mode for handling receive data. By default Interrupt mode is enabled. To enable Polling mode select the option "Poll For Received Data" in the device configurator.

10. If Rx Polling mode is enabled along with Non-RTOS mode in device configurator, the user has to call the below function periodically to receive data.
    ```
    ETH_LWIP_Poll();
    ```

11. All the log messages are disabled by default. Do the following to enable log messages:

   1. Add the `ENABLE_CONNECTIVITY_MIDDLEWARE_LOGS` macro to the *DEFINES* in the code example's Makefile to enable logs for lwIP network interface integration library. The Makefile entry should look like as follows:
       ```
       DEFINES+=ENABLE_CONNECTIVITY_MIDDLEWARE_LOGS
       ```

   2. Call the `cy_log_init()` function provided by the *cy-log* module. cy-log is part of the *connectivity-utilities* library. See [connectivity-utilities library API documentation](https://cypresssemiconductorco.github.io/connectivity-utilities/api_reference_manual/html/group__logging__utils.html) for cy-log details.

lwIP, and mbed TLS libraries contain reference and test applications. To ensure that these applications do not conflict with the code examples, a *.cyignore* file is also included with this library.

## Additional information

- [Ethernet XMClib FreeRTOS lwIP mbedtls RELEASE.md](./RELEASE.md)

- [Connectivity Utilities API documentation - for cy-log details](https://Infineon.github.io/connectivity-utilities/api_reference_manual/html/group__logging__utils.html)

- [ModusToolbox&trade; software environment, quick start guide, documentation, and videos](https://www.cypress.com/products/modustoolbox-software-environment)

- [Ethernet XMClib FreeRTOS lwIP mbedtls version](./version.xml)

- [ModusToolbox&trade; code examples]( https://github.com/Infineon/Code-Examples-for-ModusToolbox-Software )
