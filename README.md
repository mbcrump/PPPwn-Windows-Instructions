# PPPwn - PlayStation 4 PPPoE RCE - Windows 10/11 Instructions

Learn how to use PPPwn on Windows 10/11 without installing Virtualbox or using WSL (Windows Subsystem For Linux).

# What is it?

PPPwn is a kernel remote code execution exploit for PlayStation 4 developed by The Flow. It works up to firmware 11.00. The official repo is maintained here : https://github.com/TheOfficialFloW/PPPwn

Supported PS4 versions are:
- FW 9.00
- FW 9.03/9.04
- FW 9.50 / 9.60
- FW 10.00/10.01
- FW 10.50 / 10.70 / 10.71
- FW 11.00

## Homebrew Status

You can enable debug settings and install FPKS but not run them. See [LM implementation](https://github.com/LightningMods/PPPwn). 

## Requirements

In order to get started, you'll need the following items. Make sure to install all the applications listed before proceeding.

- Computer with Ethernet port (USB adapter also works)
- Ethernet cable connected to your PC/PS4
- [Python 3](https://www.python.org/downloads/)
- [Git for Windows](https://git-scm.com/download/win)
- [NPCap 1.79](https://npcap.com/#download)

## Usage

1. On your computer, navigate to a folder where you'd like to store the exploit. 

  Now clone the repository:
  
  ```sh
  git clone --recursive https://github.com/TheOfficialFloW/PPPwn
  ```

2. Install the requirements:

  ```sh
  pip install -r requirements.txt
  ```

3. Download the  `PPPwn.rar` from this [Mega.nz](https://mega.nz/file/Y4VgEBQa#t4BiPCu5PJOt57loTDa2ycxCb_OAhd-qWQDpvSyrEpA) site and copy the `stage1` and `stage2` folder to where you installed the exploit overwritting any files. 

4. Run the exploit:

  ```sh
  python3 pppwn.py --interface=Ethernet --fw=900
  ```

For other firmwares, e.g. FW 9.03, pass `--fw=903`.

5. On your PS4:

  - Go to `Settings` and then `Network`
  - Select `Set Up Internet connection` and choose `Use a LAN Cable`
  - Choose `Custom` setup and choose `PPPoE` for `IP Address Settings`
  - Enter anything for `PPPoE User ID` and `PPPoE Password`
  - Choose `Automatic` for `DNS Settings` and `MTU Settings`
  - Choose `Do Not Use` for `Proxy Server`
  - Click `Test Internet Connection` to communicate with your computer

If the exploit fails or the PS4 crashes, you can skip the internet setup and simply click on `Test Internet Connection`. If the `pppwn.py` script is stuck waiting for a request/response, abort it and run it again on your computer, and then click on `Test Internet Connection` on your PS4.

If the exploit works, you should see an output similar to below, and you should see `Cannot connect to network.` followed by `PPPwned` printed on your PS4.

### Video Tutorial

[![Step-by-step tutorial](https://img.youtube.com/vi/-_hu8sdxV9U/0.jpg)](https://www.youtube.com/watch?v=-_hu8sdxV9U)

### Sample Run

```sh
D:\PS4\exploit\PPPwn>python3 pppwn.py --interface=Ethernet --fw=900
[+] PPPwn - PlayStation 4 PPPoE RCE by theflow
[+] args: interface=Ethernet fw=900 stage1=stage1/stage1.bin stage2=stage2/stage2.bin
[+] STAGE 0: Initialization
[*] Waiting for PADI...
[+] pppoe_softc: 0xffffa01507996400
[+] Target MAC: x
[+] Source MAC: 07:64:99:07:15:a0
[+] AC cookie length: 0x4e0
[*] Sending PADO...
[*] Waiting for PADR...
[*] Sending PADS...
[*] Sending LCP configure request...
[*] Waiting for LCP configure ACK...
[*] Waiting for LCP configure request...
[*] Sending LCP configure ACK...
[*] Sending IPCP configure request...
[*] Waiting for IPCP configure ACK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure NAK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure ACK...
[*] Waiting for interface to be ready...
[+] Target IPv6: fe80::729e:29ff:fea6:5ae4
[+] Heap grooming...done

[+] STAGE 1: Memory corruption
[+] Pinning to CPU 0...done
[*] Sending malicious LCP configure request...
[*] Waiting for LCP configure reject...
[*] Sending LCP configure request...
[*] Waiting for LCP configure ACK...
[*] Waiting for LCP configure request...
[*] Sending LCP configure ACK...
[*] Sending IPCP configure request...
[*] Waiting for IPCP configure ACK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure NAK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure ACK...
[+] Scanning for corrupted object...found fe80::0219:4141:4141:4141

[+] STAGE 2: KASLR defeat
[*] Defeating KASLR...
[+] pppoe_softc_list: 0xffffffffd2e299f8
[+] kaslr_offset: 0x4ea3c000

[+] STAGE 3: Remote code execution
[*] Sending LCP terminate request...
[*] Waiting for PADI...
[+] pppoe_softc: 0xffffa01507996400
[+] Target MAC: x
[+] Source MAC: 03:26:3d:d1:ff:ff
[+] AC cookie length: 0x511
[*] Sending PADO...
[*] Waiting for PADR...
[*] Sending PADS...
[*] Triggering code execution...
[*] Waiting for stage1 to resume...
[*] Sending PADT...
[*] Waiting for PADI...
[+] pppoe_softc: 0xffffa01507995600
[+] Target MAC: x
[+] AC cookie length: 0x0
[*] Sending PADO...
[*] Waiting for PADR...
[*] Sending PADS...
[*] Sending LCP configure request...
[*] Waiting for LCP configure ACK...
[*] Waiting for LCP configure request...
[*] Sending LCP configure ACK...
[*] Sending IPCP configure request...
[*] Waiting for IPCP configure ACK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure NAK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure ACK...

[+] STAGE 4: Arbitrary payload execution
[*] Sending stage2 payload...
[+] Done!

D:\PS4\exploit\PPPwn>
```
