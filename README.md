# nxcstorm
nxcstorm grew out of two tools I relied on for OSCP credential spraying, each with a gap the other happened to fill.

[nxc-sweep](https://github.com/corey-farley/nxc-sweep/blob/main/nxc-sweep) (Corey Farley) had exactly the philosophy I wanted. Smart per-protocol defaults arguments, skip nxc calls on protocols whose ports aren't open and allows for `--local-auth` if so pleased. But it only takes one target IP, and sprays ALL pre-defined protocols in the script by default.

[nxcspray](https://github.com/NTHSec/nxcspray) (Noah H.) did the parts nxc-sweep could not. Pick exactly which protocols to run; one, some or all, and let nxc handle the target list natively. But it's locked to just `-u`/`-p`, no other choice of arguments, no port-skipping to reduce overhead of nxc process spawning.


## Features
1. **Multi-target support** - single target or list of targets
2. **Protocol Flexibility** - select one, multipl, or all protocols to spray credentials against
2. **Default arguments per protocol (nxc-sweep style)** - e.g. `--shares` for smb, `--ls` for ftp, a default query for mssql. Modify script easily as per your requirement.
3. **Optional arguments** - users can toggle `--local-auth`, which auto-applies on smb|winrm|rdp|mssql only, and/or `--continue-on-success`.
4. **Improved checks prior to nxc call** - Extends nxc-sweep port check idea to work across a full target list, checking protocol's port across every host concurrently via `xargs -P` instead of sequentially via `nc`. So, checks stay fast even as your target list grows.

## Installation
Clone this repo or download the script directly.

Add the script to /usr/local/bin/ to execute it from anywhere on your machine, or use it in a local directory of your choice.
```
sudo mv ~/Downloads/nxcstorm /usr/local/bin
chmod +x /usr/local/bin/nxcstorm
```

## Usage
```
└─$ nxcstorm -h                 
[-] Usage: nxcstorm <protocols|all> <targets> -u <username> -p <password> [--local-auth] [--continue-on-success]

Note: each protocol sprays with sensible built-in defaults automatically (e.g. --shares for smb, --ls for ftp) - no flags to configure per protocol.

Toggles:
  --local-auth	            	Authenticate against local accounts instead of domain accounts. Only applied to smb, winrm, rdp, mssql.
  --continue-on-success	    	Keep testing remaining credentials/hosts after a successful login.
```

Example Usage

```
nxcstorm all targets.txt -u usernames -p 'EricLikesRunning800' --continue-on-success
```
<img width="1280" height="519" alt="image" src="https://github.com/user-attachments/assets/60b7f2c5-032e-4e24-97d4-4e4351ec561e" />


```
nxcstorm smb,rdp,ssh 192.168.1.150 -u usernames -p 'SamSuperSwimmer' --local-auth
```
<img width="1280" height="275" alt="image" src="https://github.com/user-attachments/assets/871a05e0-3d57-4c35-bbef-d6b99f31a254" />


```
nxcstorm rdp targets.txt -u usernames -p 'SamSuperSwimmer800' --continue-on-success
```
<img width="1246" height="151" alt="image" src="https://github.com/user-attachments/assets/64f00263-72e9-461e-9323-faad568553f7" />


```
nxcstorm ssh targets.txt -u demouser -p 'iliketodemonstrate'
```
<img width="1243" height="145" alt="image" src="https://github.com/user-attachments/assets/b9183037-40b1-4beb-ba14-04c8ec9cf454" />





