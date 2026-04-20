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

We use VSCode and the [STM32CubeIDE Extension](https://marketplace.visualstudio.com/items?itemName=stmicroelectronics.stm32-vscode-extension) by ST.
Install the extension, open the project directory, click on the STM32 Extension on the left, and then Setup STM32Cube project(s).
By default it should have selected "Board/Device" as STM32H757I-EVAL, and "Toolchain" as GCC. Click Configure.

## On recreating this project
For completion's sake it is to note, that initially a config header has to be created for the is42s32800j driver to compile. Simply copy the file `CM7/is42s32800j/is42s32800j_conf_template.h` to `CM7/is42s32800j_conf.h`.

The file ``CM7/CMakeLists.txt`` was extended to build the both external device drivers.

## Build
Once the STM32 extension has recognised the project, you can click on the small "Build" button at the bottom status bar to (re)configure and build. It is recommended to also install the [Microsoft's CMake extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) for more control over the CMake configuration steps.
