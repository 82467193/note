# Executive Summary: What is RedLine Stealer
RedLine is a stealer distributed as cracked games, applications, and services. The malware steals information from web browsers, cryptocurrency wallets, and applications such as FileZilla, Discord, Steam, Telegram, and VPN clients. The binary also gathers data about the infected machine, such as the running processes, antivirus products, installed programs, the Windows product name, the processor architecture, etc. The stealer implements the following actions that extend its functionality: Download, RunPE, DownloadAndEx, OpenLink, and Cmd. The extracted information is converted to the XML format and exfiltrated to the C2 server via SOAP messages.
## SHA256: 64c4cbafa8293577b9617a5e3f7f1041fb9f9b9251c1efbf5e70fe9a9b30b1a
The sample grabbed from MalwareBazaar for educational purpose.
The initial executable was used as a dropper and it contains a portion of shellcode and the main payload(known as rudders.exe) which is stored in data section.

![](https://i.imgur.com/EIk9cb2.png)

From the above image we can see the entry point of shellcode.

![](https://i.imgur.com/IPR5uvq.png)
It also use the process hollowing technique to evade detection. AppLaunch.exe is the process being hollowed out.

Next we take a look at pe studio which is an excellent tool for static analysis on checking signatures or anything else.

![](https://i.imgur.com/G9NzegZ.png)
![](https://i.imgur.com/TDzESW6.png)

we can see that the main payload was flagged as Rudders.exe and can determine that the payload was written in C#.

![](https://i.imgur.com/sXbBcof.png)

We can found the IP address at very beginning this infer that this malware probably definitely contains C2 server
**IP:45.15.156.223:42971**

At the time I analyze this sample the IP has already been taken down so I will use the static analysis for the rest of the analysis

![](https://i.imgur.com/BvoMWPX.png)

Let's first check the classes with the panel on dnSpy, it gave us some hint what this program is probably doing. We can see some classes start with BDECRYPT and it looks like encyption for sth(Crypto wallet I guess) or decryption.

And there's ConnectionProvider which is used for C2 server as I previously mentioned and there is also Discord, FileCopier, GameLauncher and even VPN. Apparently this malware is a typical Info stealer.

![](https://i.imgur.com/ubnc6dT.png)

From the picture we know that the program is searching a file called wallet.dat which stores the private key and trade information about cryptocurrency