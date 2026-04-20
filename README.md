# Display driving example on the STM32H757I-EVAL board by ST
This is an attempt at a minimal working example to run the display on the [STM32H757I-EVAL board](https://www.st.com/en/evaluation-tools/stm32h757i-eval.html).

The project skeleton was created using the STM32CubeMX code generator.
Please refer to the [document how the .ioc-file was created](docs/Recreating_IOC.md).

# Compilation
## Project initilisation
To be able to build the project, you need pull in the driver libraries for the external SDRAM "[IS42S32800J](https://github.com/STMicroelectronics/stm32-is42s32800j)" as well as the display driver IC "[OTM8009A](https://github.com/STMicroelectronics/stm32-otm8009a)", configured as git submodules. After cloning this project, run in your preferred console:
```bash
git submodule update --init --recursive
```
