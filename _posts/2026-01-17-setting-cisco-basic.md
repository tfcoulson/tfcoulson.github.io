---
title: Cisco Router Basic Setup
date: 2026-01-17
categories: [networking, cisco]
tags: [beginner,networking,cisco,switches,ccna]
---

In this post I am going to go through the basic setup on a Cisco router to get the router working in a network.
Topics covered will be:
* Setting the hostname
* Setting up security
* Setting up IP's and connectivity

For all the topics in the CCNA we can either use Packet Tracer or physical kit.

Setting the hostname:
To set the hostname, you need to enter global configuration mode. Hostname is a command that affects whole the switch or router, and not just an interface.

```console
Switch>
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname Sw1
Sw1(config)#
```

Once the hostname has been set, the prompt will change to the new hostname.

```console
Switch#
!New hostname
Sw1#
```

Setting up passwords:
There are a number of different password settings that can be configured: console passwords, enable passwords and vty line passwords.
The console password is used to restrict access to the device when connecting via a console cable. ie you have physical access to the switch. The console password is set in 'line console 0'. You must set the password and also include the setting 'login' to enable the password:

```console
Sw1#
Sw1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Sw1(config)#line console 0
Sw1(config-line)#password ?
  7     Specifies a HIDDEN password will follow
  LINE  The UNENCRYPTED (cleartext) line password
Sw1(config-line)#password console_Pa$$word
Sw1(config-line)#login
Sw1(config-line)#end
Sw1#exit
Sw1 con0 is now available

Press RETURN to get started.

User Access Verification

Password: 

```

```console
Sw1#
Sw1#show run
Building configuration...
!
line con 0
 password console_Pa$$word
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
```

The password is displayed in cleartext. To encrypt the password, another command is needed:

```console
Sw1# configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Sw1(config)#service password-encryption 
Sw1(config)#end
Sw1#
Sw1#show run
Building configuration...
!
line con 0
 password 7 082243401A1609122D3B0D406E3C2B3A37
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
```

The encrpyted password however uses weak encryption that can easily be cracked

<a href="https://developer.cisco.com/codeexchange/github/repo/derek-shnosh/c7_decrypt/">c7-decrypt</a>

```powershell
python.exe .\c7_decrypt.py -s 082243401A1609122D3B0D406E3C2B3A37
```

Output:

```console
Insecure Password: console_Pa$$word
```

Another password that can be set is the enable password. This requires anyone who has access to the device who tries to make changes in privilege exec mode to input the password.
It can be set either clear text or encrypted by using 'enable password xxx' or 'enable secret xxx'. If both are used, the 'enable secret' overides the 'enable password'.

```console
Sw1#
Sw1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Sw1(config)#enable password ?
  7      Specifies a HIDDEN password will follow
  LINE   The UNENCRYPTED (cleartext) 'enable' password
  level  Set exec level password
Sw1(config)#enable password enable_Pa$$word
Sw1(config)#
Sw1(config)#end
Sw1#
```

Output:

```console
Sw1>exit

Sw1 con0 is now available

Press RETURN to get started.

User Access Verification

Password: (console_Pa$$word)

Sw1>enable
Password: (enable_Pa$$word)
Sw1#
```


To use an encrpyted password, use 'enable secret' instead. This uses md5 to create a hash of the password and store it in the configuration so it cant be read. Md5 is a hashing algorythm so cant be cracked like an encrypted password can. It can however be 'cracked' using a rainbow table containing a list of md5 hashes and their corresponding passwords.

```console
Sw1>enable
Password: 
Sw1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Sw1(config)#enable secret secret_Pa$$word
Sw1(config)#end
Sw1#
%SYS-5-CONFIG_I: Configured from console by console

Sw1#show run
Building configuration...

Current configuration : 1212 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Sw1
!
enable secret 5 $1$mERr$1AKnkKzOtm0ezRWxHiMGF1
enable password enable_Pa$$word
!
!
!
```

Output:

```console
Sw1>exit

Sw1 con0 is now available

Press RETURN to get started.

User Access Verification

Password: (console_Pa$$word)

Sw1>enable
Password: (secret_Pa$$word)
Sw1#
```

The last passwords that I will run through are the vty line passwords. These are used for telnet and ssh and set in 'line vty' mode. If setting up telnet, the 'login' command is also required. If setting up ssh, the 'login local' command is required along with a local username set.

```console
Sw1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Sw1(config)#line vty 0 15
Sw1(config-line)#ena
Sw1(config-line)#pass
Sw1(config-line)#password ?
  7     Specifies a HIDDEN password will follow
  LINE  The UNENCRYPTED (cleartext) line password
Sw1(config-line)#sec
Sw1(config-line)#secr
Sw1(config-line)#pass
Sw1(config-line)#password vty_Pa$$word
Sw1(config-line)#end
Sw1#
```

Output:

```console
!
line vty 0 4
 password vty_Pa$$word
 login
line vty 5 15
 password vty_Pa$$word
 login
!
!
!
!
end
```

