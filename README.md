# norobo
Norobo is a spam phone call blocker for land lines.  It uses a modem (remember those old things?) connected to a PC, RPi, etc. and plugged into any phone jack in your house.

### Motivation
Spam / robo calling an epidemic and relativly little has been done to stop it.  There are some great services available like Nomorobo but unfortunately not all carriers offer the simulring feature it requires.  There are also features I want that that type of service may not offer.  E.g., temporarily block a number if it calls more than once in a minute, simultaneously check multiple online blacklist services, web interface for the call log, etc.

### How it works
When a call comes in, your phone rings once, norobo gets the caller ID from the modem, checks a list of blocked numbers / rules, and hangs up on them if it matches the list.

### Compatibility
It's been tested on Linux using a "Zoom 3095 USB Mini External Modem" but it should (in theory) work with any Hayes compatible modem that support caller ID.  It may also work on Windows and OS X.

### Install
Currently, it must be built from source.  You'll need the [Go tools](https://golang.org/doc/install), if you don't already have them installed.  Then clone this repo and build the code in `cmd/norobod`.

### Configuration
None at the moment.  How's that for easy?  The default filter is set up to block the most common patterns I've seen.  However, that means you'll have to edit `cmd/norobod/norobod.go` if you want to add or remove a rule from the call block filter.

### Running
On Linux:
- Plug the modem in to the PC
- `ls /dev` to find the modem's name. It's `/dev/ttyACM0` for me.
- `./norobod -c "/dev/ttyACM0,19200,n,8,1"`

If it exists immediately with `permission denied`, you probably need to add yourself to the `dialout` group.  Check who owns the modem and what group it belongs to first:
```
$ ls -l /dev/ttyACM0
crw-rw---- 1 root dialout 166, 0 Dec 30 21:52 /dev/ttyACM0
```
Then confirm our suspicion that your user is not in the modem's group:
```
$ groups dgnorton
dgnorton : dgnorton adm cdrom sudo
```
Then add yourself to the `dialout` group:
```
$ usermod -a -G dialout dgnorton
```
