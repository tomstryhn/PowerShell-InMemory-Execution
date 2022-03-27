# PowerShell-InMemory-Execution

You might have heard "InMemory Execution", mentioned at some point if you work with CyberSecurity or IT in general, if not, it doesn't matter, I will try and get you up to date on the term, at least in regards to PowerShell and InMemory Execution of scripts. 


## Content

[Script execution explained](#script-execution-explained)

- [Normal PowerShell script execution](#normal-powershell-script-execution)

- [InMemory Execution](#inmemory-execution)

[Final thoughts](#final-thoughts)


## Script execution explained

To get the big picture let's start with the basics:

#### Normal PowerShell script execution

The normal way of creating and running a PowerShell script, is to use some sort of code editor, some uses the PowerShell ISE, Notepad++, personally I use the VS Code. Basically you write a snippet of code, and saves the file with the '.ps1' extension, this is the default PowerShell script file extension. For this demonstration you can write the following in your preffered editor:

```PowerShell

Write-Host '####################'
Write-Host '### Hello World! ###'
Write-Host '####################'

```

Save the file as 'HelloWorld.ps1'

Now you are able to run this code from a PowerShell session, of course depending on the Execution Policy, whole different talk, but assuming you wrote some code that actually works, and the Execution Policy on your computer lets you run the script, you should be able to run the newly created script by simply opening PowerShell on your computer, and navigating to the file location, and typing the name of the file like this:

```PowerShell

PS C:\> .\HelloWorld.ps1

####################
### Hello World! ###
####################

```

This is a very basic example, to get the idea of how a normal script execution works. So what did you do? You **SAVED** a PowerShell scriptfile on your harddrive and ran it in a PowerShell session. The big word here is **SAVED**, why? Well most Anti-Virus and Anti-Malware programs, even older or cheap ones, will almost certainly scan any new files on your harddrive, especially scriptfiles. Some Administrators actually blocks all script-files outside predefined known paths, which in some cases actually can prevent you from saving the file with the '.ps1' extension. This makes the job of hackers a little bit more difficult, but they found away to get around this issue:

#### InMemory Execution

Basically, what InMemory Execution means, is that the code will only exist in the memory of the session, where it will be executed. But how can you get the script onto the computer without saving it to disk, you might ask? Variables is the quick and easy answer, you get your code loaded into a variable, which only exists in the memory, and then you Invoke the code into your session.

Sample time:

```PowerShell

$remoteURL = 'https://raw.githubusercontent.com/tomstryhn/PowerShell-InMemory-Execution/main/codesamples/VeryFriendlyCode.ps1'       
$remoteCode = (Invoke-WebRequest -Uri $remoteURL -UseBasicParsing).Content  
Invoke-Expression -Command $remoteCode

```

Now remember to check the remote code you are about to run, you can do this by copying the url in the `$remoteURL`, and opening in a new tab in your browser, to be sure what you are about to execute. When you have validated that it's not something dangerous, you can copy the above code into a new PowerShell session, you should see something like what you generated in the sample from earlier.

But now it's red. *MAGIC*

What happened? Well on a lowlevel explanation line-by-line, first we created a variable `$remoteURL` and put in the URL for the code we want to execute, without saving it onto the disk. Then by using a builtin feature of PowerShell, called `Invoke-WebRequest`, which simply acts as a browser and fetches all the information from the `$remoteURL`. We then select the content, since that's all we need, by using the `(In..RL).Content`. Now we actually have the code we want to execute in 'memory', so now all we need to do is execute it, this is done by using the `Invoke-Expression`, which runs the code, and returns the result. All this is done by using a variable, so we skipped the part where we had to save the script onto our harddrive, hence **InMemory Execution**.

## Final thoughts

Now keep in mind that this example was done with a simple and safe script, you could verify before executing, but by nesting functions and scripts inside a 'container' function or scriptfile, hackers are able to execute malicious code and tools like BloodHound for reconnaissance in victims enviroments. More and more, of the AntiVirus, AntiMalware and Defender software, are able to detect these attempts, some might be blocked based of the content of the variable, since some of the software are monitoring the memory aswell. The securitysoftware will recognize known 'signatures' of these malicious 'packages' when they are attempted executed, some already when they are attempted fetched, but the hackers are also getting better and better at cloaking their scripts, by rewriting them or rearrange the code, so the signatures changes.

So it's more less a cat and mouse game.


