### HMC5883L 3D magnetometer header-only library for pico c/c++ sdk

- Suported magnetometers HMC5883L(mag), BNO055 (9dof IMU)
- The original bosch sensortec driver for bno055 is used as is (under imu/bno055 folder)
- imu.h in the root folder is just a wraper over the original bno055 driver with pico specific i2c read/write callbacks.
- HMC5883L uses NED reference frame (x is the yaw/heading vector) while bno055's deafult is ENU (y is the yaw/heading vector)
- Clone this repository at the same level as other examples under the pico-examples directory.
- Edit the root CMakeLists.txt of the pico-examples repository and include this repository in the build using add_subdirectory(hmc5883l).
- Build the examples as usual using 'cmake --build ..'
- Calibrate the magnetometer first using the calibrate function.
- Copy the resulting UF2 file (main.uf2) onto the Pico and use PuTTY to check the heading values.
- Reported heading can be cross-verified using a floating magnetized needle or something equivalent.
- The heading should vary from 0 degrees (true geographic north) all the way to 360 degrees (one full x-y plane rotation), and then wrap back to 0.
- all mag/imu headings are pointing to magnetic north. As such, local declination needs to be accounted for.
- The app directory has a c++ proxy app that reads messages off pico's COM port (usb) connection and publishes them over a websocket server
- App directory also has a three.js visualizer that maintains a websocket connection to the c++ proxy app.
- The axis helper gets rotate around the z axis based on the yaw reported by the IMU over USB/UART.
- There's some issue on windows with reading the com port using proxy app first.. just start putty first, close it and then launch the proxy app.

c++ proxy app
```
vcpkg install websocketpp
cmake -DCMAKE_TOOLCHAIN_FILE=D:/vcpkg/scripts/buildsystems/vcpkg.cmake ..
cmake --build . --config Debug
app.exe \\.\COM4
```

three.js visualizer
```
npm install http-server -g
http-server -p 1234 --cors
launch http://localhost:1234/
```