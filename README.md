# PowerShell-InMemory-Execution

PowerShell InMemory Execution explained, with samples.
You might have heard "InMemory Execution", mentioned at some point if you work with IT Security, if not, it doesn't matter, I will try and get you up to date on the term, at least in regards to PowerShell and InMemory Execution of scripts. 

## How do you do InMemory Execution?

To get the big picture let's start with the basics:

#### Normal PowerShell script execution

The normal way of creating and running a PowerShell script, is to use some sort of code editor, some uses the PowerShell ISE, Notepad++, personally I use the Microsoft VS Code. Basically you write a snippet of code, and saves the file with the '.ps1' extension, this is the default PowerShell script file extension. For this demonstration you can write the following in your preffered editor:

```PowerShell
Write-Host '####################'
Write-Host '### Hello World! ###'
Write-Host '####################'
```
Save the file as 'HelloWorld.ps1'

Now you are able to run this code from a PowerShell session, of course depending on the Execution Policy, whole different talk, but assuming you wrote some code that actually works, and the Execution Policy on your computer lets you run the script, you should be able to run the newly created script by simply opening PowerShell on your computer, and typing the name of the file like this:

```PowerShell
PS C:\> .\HelloWorld.ps1

####################
### Hello World! ###
####################
```

This is a very basic example, to get the idea of how a normal script execution works. So what did you do? You **SAVED** a PowerShell scriptfile on your harddrive and ran it in a PowerShell session. The big word here is **SAVED**, why? Well most Anti-Virus and Anti-Malware programs, even older or cheap ones, will almost certainly scan any new files on your harddrive, especially scriptfiles. Some Administrators actually blocks all script-files outside predefined known paths, which in some cases actually can prevent you from saving the file with the '.ps1' extension. This makes the job of hackers a little bit more difficult, but they found away to get around this issue:

#### InMemory Execution

Basically, what InMemory Execution means, is that the code will only exist in the memory of the session, where it will be executed. But how can you get the script onto the computer without saving it to disk, you might ask? Variables is the quick and easy answer, you get your code loaded into a variable, which only exists in the memory, and then you Invoke the code into your session. Sample time:

```PowerShell
$remoteURL  = 'https://raw.githubusercontent.com/tomstryhn/PowerShell-InMemory-Execution/main/codesamples/New-RandomPassword.ps1'       
$remoteCode = (Invoke-WebRequest -Uri $remoteURL).Content  
Invoke-Expression -Command $remoteCode
New-RandomPassword
```
