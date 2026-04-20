# Recreating the STM32CubeMX IOC file for this project.

Open the STM32CubeMX application and create a new project via File->New Project.
Via "Board Selector" chose the STM32H757I-EVAL board and start a new project.
When asked
- "*Initialize all peripherals with their default Mode?*"
- "*For this Cortex-M7 device, it is highly recommended to preconfigure the Memory Protection Unit to optimize the Speculative Read access of the core. Do you want to apply now such default configuration?*"

click "Yes" both times.

All configurations that follow are changes made to the defaults values from the above selection.
**Clock Configuration**
- DIVM2: /2
	- DIVN2: x12
	- DIVP2: /1
	- fracn2: 0
- DIVM3: /5
	- DIVN3: x160
	- DIVR3: /19
- HSE
	- /IDF: /5
	- NDIV: x100
	- /ODF: /1
	- TX Prescaler: /4

**Pinout & Configuration**
Due to what options become visible depends on what other components you enable first, the changes are in order of what they seemingly unlock further down the line (somewhat):
- Middleware and Software Packs
	- FREERTOS_M7
		- Interface: CMSIS_V2
	- FREERTOS_M4
		- Interface CMSIS_V2
- Multimedia
	- DSIHOST: Cortex-M7 selected
		- DSIHost: Adapted Command mode with TE Pin
		- Tab Commands
			- Generic Short Write Zero Parameter: Low Power Transmission
			- Generic Short Write One Parameter: Low Power Transmission
			- Generic Short Write Two parameter: Low Power Transmission
			- Generic Short Read Zero parameter: Low Power Transmission
			- Generic Short Read One parameter: Low Power Transmission
			- Generic Short Read Two parameter: Low Power Transmission
			- Generic Long Write: Low Power Transmission
			- DCS Short Write Zero parameter: Low Power Transmission
			- DCS Short Write One parameter: Low Power Transmission
			- DCS Short Read Zero parameter: Low Power Transmission
			- DCS Long Write: Low Power Transmission
			- Maximum Read Packet Size Command: High Speed Transmission
		- Tab Display Interface
			- Maximum command Size: 800 Pixels
			- The Refresh of the Display Frame Buffer is Triggered: manually by Enabling the LTDC
			- Tearing Effect Source: DSI Link
		- Tab Data and Clock Lanes
			- Number of Lanes: Two Data Lanes
			- Tearing Effect Acknowledge Request Enable: Enabled
	- LTDC: Cortex-M7 selected
		- Display Type: RGB888 (24 bits) - DSI mode
			- Tab Parameter Settings:
				- Horizontal Synchronization Width: 2 Pixels
				- Horizontal Back Porch: 24 Pixels
				- Active Width: 800 pixels
				- Horizontal Front Porch: 34 pixels
				- Vertical Synchronization Height: 4 lines
				- Vertical Back Porch: 2 lines
				- Active Height: 480 lines
				- Vertical Front Porch: 2 lines
			- Tab Layer Settings
				- Number of Layers: 1 layer
				- Layer 0 - Window Horizontal Stop: 800
				- Layer 0 - Window Vertical Stop: 480
				- Layer 0 - Blending Factor1: Alpha constant x Pixel Alpha
				- Layer 0 - Blending Factor2: Alpha constant x Pixel Alpha
				- Layer 0 - Color Frame Buffer Start Adress: 0xD0000000
				- Layer 0 - Color Frame Buffer Line Length (Image Width): 800
				- Layer 0 - Color Frame Buffer Number of Lines (Image Height): 480
			- Tab NVIC Settings
				- LTDC global interrupt: Enabled ticked
	- DMA2D: Cortex-M7 selected
		- Activated selected
			- Tab NVIC Settings
				- DMA2D global interrupt: Enabled ticked
- Connectivity
	- FMC: Cortex-M7 and Cortex-M4 ticked, Initializer: Cortex-M7
		- Tab SDRAM1
			- CAS latency: 3 memory clock cycles
			- SDRAM common clock: 2 HCLK clock cycles
			- SDRAM common burst read: Enabled
			- Load mode register to active delay: 3
			- Exit self-refresh delay: 7
			- Self-refresh time: 4
			- SDRAM common row cycle delay: 7
			- Write recovery time: 4
			- SDRAM common row precharge delay: 3
			- Row to column delay: 3
- Timers
	- TIM1: M7 and M4 ticked, Initializer Cortex-M7
	- TIM6: M4 ticked: Initializer Cortex-M4
- System Core 
	- Cortex_M7
		- Tab Parameter Settings
			- Cortex Memory Protection Unit Region 1 Settings
				- MPU Region: Enabled
				- MPU Region Base Address: 0x08000000
				- MPU Region Size: 1MB
				- MPU TEX field level: level 1
				- MPU Access Permission: ALL ACCESS PERMITTED
				- MPU Cacheable Permission: ENABLE
			- Cortex Memory Protection Unit Region 2 Settings
				- MPU Region: Enabled
				- MPU Region Base Address: 0x30000000
				- MPU Region Size: 256KB
				- MPU TEX field level: level 1
				- MPU Access Permission: ALL ACCESS PERMITTED
				- MPU Shareability Permission: ENABLED
				- MPU Cachable Permission: ENABLED
				- MPU Bufferable Permission: ENABLED
			- Cortex Memory Protection Unit Region 3 Settings
				- MPU Region: Enabled
				- MPU Region Base Address: 0xD0000000
				- MPU Region Size: 8MB
				- MPU Access Permission: ALL ACCESS PERMITTED
				- MPU Cachable Permission: ENABLED
	- SYS
		- Timebase Source: TIM1
	- SYS_M4
		- Timebase Source: TIM6
	- GPIO
		- Tab GPIO
			- PA4: Pin Context Assignment: ARM Cortex-M4
			- PA6: Pin Context Assignment: ARM Cortex-M7
			- PC13: Pin Context Assignment: CortexM4
			- PD3: Pin Context Assignment: ARM Cortex-M4, User Label empty
			- PF10: Pin Context Assignment: ARM Cortex-M7
			- PI8: Pin Context Assignment: ARM Cortex-M7

**Project Manager**
- Project
	- Project Name: H757_with_display
	- Toolchain Folder Location: `somewhere/you/choose`
	- Toolchain/IDE: CMake
	- Default Compiler/Linker: GCC
	- Enable multi-threaded support: ticked
- Code Generator
	- STM32Cube MCU packages and embedded software packs: Copy all used libraries into the project folder
- Advanced Settings
	- Generated Function Calls CortexM7
		- Rank 7, MX_I2C1_Init, I2C1: Do Not Generate Fucntion Call ticked, Visibility (Static) Ticked
		- Rank 8, MX_USART1_UART_Init, USART1: Do Not Generate Fucntion Call ticked, Visibility (Static) Ticked
	- Register CallBack
		- LTDC: ENABLE

With this we generate the code, clicking "GENERATE CODE"
