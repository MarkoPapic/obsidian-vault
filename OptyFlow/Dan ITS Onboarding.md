## 2025-12-18

Remote debugger (GDB) SSH
- SSH the file to the target, and then debug remotely

Terminlogy:
* Host machine: My linux machine
* Target machine: Controller
* Econolite SDK: Build tooling (gcc and stuff) that cross-compiles, builds and links C++ code for PowerPC

All ISM SDKs are for Intel x64 platform running Linux (Dan uses Debian).
- Then they cross-compile to PowerPC and send it to the target for debugging.
- For Miovision, it is cross-compiled to ARM64

He uses CodeLite but I can use whatever.

I can get up and running with the codebase using C1 I/O. I don't need an actual cabinet for start.

Places to start:
- Aashto spec for ATC
	- Controller: 52.01
	- Cabinet: 53.01
	- API: 54.01
- Dan's test program (maybe start directly from here)
- The process would be the following:
	- I cross-compile the code
	- I deploy it to the target machine (controller)
	- I run Dan's test program which invokes the ISM SDK APIs on the controller
	- I install the debugger on the target machine and run a debugging session

Dan will send me:
* Econolite SDK
* Test program