---
title: "PWN environment setup on Win11 using WSL2"
date: 2024-12-11
draft: false
summary: "Setting up a binary exploitation environment on Windows 11 using WSL2"
tags: ["pwn", "blog", "tutorial"]
---

Having a good environment is a must when you want to do binary exploitaton. Here I will show you how to set up a simple environment using Ubuntu and WSL2
so you can straight up jump into finding and writing your exploits.

## Agenda

Things we will be setting up:
- Ubuntu inside our Windows WSL2
- Pwntools library for writing exploits
- PwnDbg for debugging and finding vulnerability on programms

## Setting up Ubuntu on WSL2

Change WSL version to 2
```bash
wsl --set-default-version 2
```
<img src="img/Pasted image 20241209090017.png">
<hr>

Searching for our distribution in the online marketplace
```bash
wsl --list --online
```
<img src="img/Pasted image 20241209090045.png">
<hr>

Installing our distribution with a suitable version. Here I will be instaling Ubuntu 24.04 which comes under their LTS
```bash
 wsl --install -d Ubuntu-24.04
```
<img src="img/Pasted image 20241209090109.png">
<hr>

After the installation, it will prompt you to set up a user with a password. After which it will drop you to your user shell
and you should be able to see something like this.
<img src="img/Pasted image 20241209091105.png">

### Configuring 32-bit environment
Since the distribution we installed is of 64-bit, we must set it up to run 32-bit programs as well. Its very common in CTFs that we receive 32-bit binaries.

Installing all the necessary packages and libraries.
```bash
sudo dpkg --add-architecture i386 
sudo apt-get update
```
<img src="img/Pasted image 20241209091403.png">
<hr>

```bash
sudo apt install build-essential
```
<img src="img/Pasted image 20241209091547.png">
<hr>

```bash
sudo apt install gcc-multilib
```
<img src="img/Pasted image 20241209091733.png">

## Setting up PwnTools library
Pwntools is a CTF framework and exploit development library. Written in Python, it is designed for rapid prototyping and development, and intended to make exploit writing as simple as possible.
```bash
sudo apt-get update
sudo apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade pwntools
```
<img src="img/Pasted image 20241209092006.png">

### Working around the PIP upgrade error
<img src="img/Pasted image 20241209092036.png">

There are many solutions to it. The best and the recommended one is to set up a virtual environment. 
The second would be to use the `--break-system-packages` tag with our pip command and
the third is to set up a config so that we dont have to pass the tag everytime we use pip.

{{< alert iconColor="#FADFA1" >}}
**NOTE!** I would still recommend to set up an virtual environment!
{{< /alert >}}

Lets set up a config for our pip
```bash
mkdir .config
mkdir .config/pip
touch .config/pip/pip.conf
```

edit `pip.conf`
```ini
[global]
break-system-packages = true
```

Now we should be able to continue our installing without any errors.
<img src="img/Pasted image 20241209092615.png">
<img src="img/Pasted image 20241209092706.png">
<hr>

We can confirm our installation by loading the library into Python
<img src="img/Pasted image 20241209093218.png">

## Setting up PwnDbg
PwnDbg is a GDB plug-in that makes debugging with GDB suck less, with a focus on features needed by low-level software developers, hardware hackers, reverse-engineers and exploit developers.

First lets start by installing gdb
```bash
sudo apt install gdb
```
<img src="img/Pasted image 20241209115301.png">
<hr>

Installing and setting up PwnDbg
```bash
git clone https://github.com/pwndbg/pwndbg 
cd pwndbg 
./setup.sh
```
<img src="img/Pasted image 20241209115504.png">
<img src="img/Pasted image 20241209115611.png">

## Resources

- PwnTools - <http://pwntools.com/>
- PwnDbg - <https://pwndbg.re/>
- [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
- [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/)

## FAQ
### What to do after this?

Well, I would suggest installing a code exitor such as [VSCode](https://code.visualstudio.com/) on your host machine for you to write exploits or you can be a chad and install neovim on your linux.

After that you can set up a disassembler and decompiler of your choice. There are many to choose from but these are some of my picks:
- [Ghidra by NSA (yes the NSA)](https://ghidra-sre.org/)
- [IDA by Hex Rays](https://hex-rays.com/)
- [Binary Ninja by Vector 35](https://binary.ninja/)
- [Cutter by Rizin](https://cutter.re/)

Now that you are all set, the only thing left is to start hacking and writing exploits 