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

## 2026-01-05

### Google Drive files

- ***ATC Specifications*:** ATC standard files
- ***Toolchain for x64 host with PowerPC target***: Econolite toolchain for building for target architecture.
	- The included PDF file contains the "Cross Compiling Procedures" section that explains the cross-compiling process
- ***ATC Econolite 3_12_74 Setup Script.txt***: A setup script executed by TOAD that sets up a target Linux machine. It does the stuff explained in *Target Setup.docx*
- ***Dev Host Setup.docx***: Instructions on how to setup a development environment.
	- He uses a CodeLite out-of-the-box setup which autogenerates the Makefile for him
	- If I use VSCode, I would need to create my own Makefile
- ***ISM project for PowerPC and x86 target.zip***: source code
	- This folder contains the latest version of source code
	- The GitHub version is a couple of revisions behind this
	- Dan only pushes stable versons to GitHub
	- The GitHub version doesn't contain the Makefile or the CodeLite project. It is just source code.
- ***Protocol.h***: TOAD communication protocol
- ***Target Setup.docx***: Instructions on how to setup a target machine
- ***Test project for PowerPC target.zip***: Dan's project used for testing controller functionality.
	- You are supposed to transfer it on the target machine and then execute it.
	- It is just an executable with no CLI arguments. If you want granularity, you un/comment parts of code.

### Other

`ism.h-> lines 70 and 71` allow you to set up debug flags.

