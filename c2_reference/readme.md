# Problem with cobalt strike cracking

https://www.cobaltstrike.com/blog/how-to-crack-cobalt-strike-and-backdoor-it/

# installing covenant

https://github.com/cobbr/Covenant/wiki/Installation-And-Startup

## evading covenant

https://apr4h.github.io/2021-02-25-Antivirus-and-AMSI-Evasion-With-Covenant/


looking at msbuild bypass : https://www.hackingarticles.in/windows-exploitation-msbuild/

https://github.com/matterpreter/DefenderCheck

we can try and check which part of grunt xml is being detected.

```
C:\Users\HP\Documents\DefenderCheck\DefenderCheck\DefenderCheck\bin\Debug>DefenderCheck.exe "C:\Users\HP\Downloads\GruntHTTP.xml"
[-] C:\Temp doesn't exist. Creating it...
Target file size: 8119 bytes
Analyzing...

[!] Identified end of bad bytes at offset 0x252 in the original file
File matched signature: "Backdoor:MSIL/TurtleLoader.BSC!dha"

00000000   47 72 6F 75 70 2F 3E 0D  0A 20 20 20 20 3C 54 61   Group/>··    <Ta
00000010   73 6B 3E 0D 0A 20 20 20  20 20 20 3C 43 6F 64 65   sk>··      <Code
00000020   20 54 79 70 65 3D 22 46  72 61 67 6D 65 6E 74 22    Type="Fragment"
00000030   20 4C 61 6E 67 75 61 67  65 3D 22 63 73 22 3E 0D    Language="cs">·
00000040   0A 20 20 20 20 20 20 20  20 3C 21 5B 43 44 41 54   ·        <![CDAT
00000050   41 5B 0D 0A 20 20 20 20  20 20 20 20 20 20 20 20   A[··
00000060   76 61 72 20 6F 6D 73 20  3D 20 6E 65 77 20 53 79   var oms = new Sy
00000070   73 74 65 6D 2E 49 4F 2E  4D 65 6D 6F 72 79 53 74   stem.IO.MemorySt
00000080   72 65 61 6D 28 29 3B 0D  0A 20 20 20 20 20 20 20   ream();··
00000090   20 20 20 20 20 76 61 72  20 64 73 20 3D 20 6E 65        var ds = ne
000000A0   77 20 53 79 73 74 65 6D  2E 49 4F 2E 43 6F 6D 70   w System.IO.Comp
000000B0   72 65 73 73 69 6F 6E 2E  44 65 66 6C 61 74 65 53   ression.DeflateS
000000C0   74 72 65 61 6D 28 6E 65  77 20 53 79 73 74 65 6D   tream(new System
000000D0   2E 49 4F 2E 4D 65 6D 6F  72 79 53 74 72 65 61 6D   .IO.MemoryStream
000000E0   28 53 79 73 74 65 6D 2E  43 6F 6E 76 65 72 74 2E   (System.Convert.
000000F0   46 72 6F 6D 42 61 73 65  36 34 53 74 72 69 6E 67   FromBase64String
```

in my case `system.IO.Compression.DeflateStream(new System.IO.MemoryStream(System.Convert.FromBase64String))` is being detected.

we can maybe replace the existing string in the code at msbiilder location.

I will try and use sektor 7 course to obfuscate the binary generated by covenant

# installing Havoc C2

188.68.249.11

https://4pfsec.com/havoc-c2-first-look/

![](./havoc.png)

# using dark armour

https://github.com/bats3c/darkarmour

https://github.com/icyguider/Nimcrypt2

