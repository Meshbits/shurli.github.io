---
layout: default
---


### [Dev Update: June 30, 2020 (NZST)](#update-june-30-2020)

```shell
➜  go_dart_ffi_c_shared git:(master) ✗ dart client.dart
awesome.Add(12, 99) = 111
➜  go_dart_ffi_c_shared git:(master) ✗ ls -lha
total 5376
drwxr-xr-x  10 satinder  staff   320B 30 Jun 01:49 .
drwxr-xr-x@ 29 satinder  staff   928B 29 Jun 20:20 ..
-rw-r--r--   1 satinder  staff   614B 29 Jun 20:25 awesome.go
-rw-r--r--   1 satinder  staff   1.7K 29 Jun 20:25 awesome.h
-rw-r--r--   1 satinder  staff   2.6M 30 Jun 01:49 awesome.so
-rwxr-xr-x   1 satinder  staff    13K 29 Jun 22:44 client
-rw-r--r--   1 satinder  staff   354B 30 Jun 01:50 client.dart
-rw-r--r--   1 satinder  staff   780B 29 Jun 22:14 client1.c
-rwxr-xr-x   1 satinder  staff    13K 29 Jun 22:43 client2
-rw-r--r--   1 satinder  staff   1.8K 29 Jun 22:43 client2.c
➜  go_dart_ffi_c_shared git:(master) 
```
  
okay, fixed it to understand it better  
I just removed all other unnecessary example function codes to minimise the scope to test and verify the process of executing a function from a library file using Dart  
just included Add function from .so file which was compiled from go 🙂   
and called it from .dart file, and it seem to work as expected  
my requirement is just that small as I see  
I only need to execute the main() function of my gRPC server go code, and I guess just calling a single function to execute should be enough to start gRPC as the Dart based flutter app starts  
will do that test next, with a simple hello greeter gRPC example code, again keeping it's scope less for this testing 😁  

right now, I'm only trying to solve the challenges I have in mind to make sure I don't get stuck too long on those  
so made gRPC with 2 APIs to make sure I understand and it works as expected  
then tested FFI code of go, called the functions from compiled library in C language file  
and then in Dart  
Also, followed up Flutter's documentation to test the Add function in flutter for both iOS and Android  
so at least these steps I have tested and validated that I can at least do something and what kind of challenges there are further  
so, adding more further APIs to gRPC seems easy now, since I have experienced and tested that  
the challenge after testing FFI I identified is to make the gRPC server start by calling the main function which will start the gRPC server within App!  
it is a challenge, since if just starting the `go run server.go` it stops the stdout on the command prompt and waits for the keyboard input signal like CTRL+C to cancel or kill the process  
so this behaviour I need to manage with the Dart code  
but I have already tried this earlier 🙂  
with Shurli  
when I made it a daemon and just gave it a start/stop command over command line  
so I can just make the start/stop argument for the library file too, and let the gRPC server run in the background process and also kill it  
so this whole mindmap of problems and solutions been a long trail of efforts in last few months  
I still expect challenges, since Dart's FFI is in beta stage! and I found that advanced Go to Dart interaction a bit tricky, which I'll need to wrap my head around to properly fix any issues if arise in future


### [Dev Update: June 29, 2020 (NZST)](#update-june-29-2020)
  
Dart language only interoperable with C language, so any language exports it's functions and datatypes for C and those are called in Dart.  
I just tried Golang code and called the functions in C language code, with Dynamic library linking and Dynamically loading the .so library. Read the C code from sample code where I learned it from, and can understand the C language code part fine.  
Also got the Dart sample code to use FFI, but it did not work as expected:  
https://github.com/vladimirvivien/go-cshared-examples/issues/21  
  
Gonna try sorting this out first before I move ahead....  


### [Dev Update: June 28, 2020 (NZST)](#update-june-28-2020)
  
a little update:  
Since I'm doing gRPC, I see that my existing RPC API is pretty much as is being ported to gRPC, since all the logic code is written in the existing API.  
and gRPC is too easy to define and work as I learned and practicing now.  
  
But I found there is some pointer error which is thrown if any single coin's RPC Is busy while other coins' RPC is responding.  
For example if KMD is doing `importprivkey` command and busy doing the rescan, and all other coins like PIRATE, DEX, MCL etc are responding, my gRPC code throws that pointer error and ends the process.  
  
I tried to solve that, but feels lacking in my debugging skills with `dlv` which is similar to `gdb`.  
will try to solve that sooner or later coz it's kind of critical bug to fix.  
Right now, I'm checking on FFI ( foreign function interface) so that I can compile my gRPC code to a library and call it's functions/methods/variables etc from Dart Language.  
looking at latest developments happening in Flutter, at the moment, it's Desktop rendering engine is experimental, but really hoping that by the end of 2020, or early 2021, this Desktop Rendering engine will be mature enough to be out in public use with some level of stability.  
I'd prefer to use the official Desktop Rendering engine instead of going the unofficial Hover Go, the Flutter's unofficial Desktop rendering engine  
It's known that Flutter's official Desktop rendering engine is good enough state for Linux, and is in development for Mac and Windows. I need to check on it's latest development updates.  
But as I suspect since Apple is now moving to Apple's own Silicon Chips, I suspect it won't take long enough for Flutter team to support Mac with full support for Desktops too  
that means new Mac machines will have full support of official Desktop app rendering engines  
since Flutter is already stable and supported on mobile devices, and Apple Silicon will be on same chipset/framework for desktops  
EDITED  
feeling like I'm playing with a lot and lots of new technologies in their development state 😅  
so, using FFI, and calling Golang library from Dart, I'll be able to execute the gRPC on the device itself where the application will run.  
and since on iOS it is not supported to bundle an executable on iOS Apps, using FFI is the only route as I understand, so being already future ready with the code architecture  
so, this Dart code of Flutter will always invoke the main function from Golang library code to start gRPC server and Dart will act as a client, where the purpose of golang's gRPC server will be interact with Native coins' RPC or nSPV or Electrum or etc etc.  
Kind clearing my mind to a debugging duck here if anyone reads 😅


### [Dev Update: June 25, 2020 (NZST)](#update-june-25-2020)
  
things are progressing slowly but consistently in Shurli development.  
I did start Flutter front-end code and made stub front end for testing. it's just very basic so far.  
Thought it would be better if I instead use existing data than stub values and doing the gRPC API to finish first, where I'm at right now.  
In progress Flutter UI of Shurli looks like this:  
  
![flutter_shurli_ui_test](https://github.com/meshbits/shurli.github.io/blob/master/images/20200625/flutter_shurli_ui_test.png?raw=true)  
  
  
When @khatana will have the designs for frontend ready, I'll do the GUI code according to those designs  
I think it's best until then I finish up on the gRPC work  
hopefully by then the Shurli Logo will be finalised and we will be then working on the GUI design work  
thought I give couple of you guys some update here. been long time 🙂

