# AutoHotkey Applications--Ideas and Tips for Writing Practical AutoHotkey Scripts by Jack Dunning
 
## Include command

The `#Include` command is designed primarily to bring in libraries of function and subroutines.

The `#Include` command simply acts as a conduit for bringing code from another file into the main script. It inserts the code from the source file into the main file at the point where the #Include command is found. For example, to add the AutoCorrection app to fix common misspellings anywhere, anytime it can be added to any other script with the line: `#include C:\\Users\\Jack\\AutoHotkey\\autocorrect.ahk` If you want a little more universal path, you can use: `#include %A_ScriptDir%\\autocorrect.ahk`

If you only use the `#Include` command to add functions and subroutines, then you most likely won't have problems.

### Windows Process ID

The Run command (Run Notepad,, Hide, npPid) launches Notepad as a hidden window—in the background. The Process ID is saved to the variable npPid. This variable is how this instance of Notepad is identified in the remainder of the script. Using the Process ID rather than the title (or part of the title) of a window is more reliable since the Process ID is unique to the window.

The WinWait command pauses the script until the new Notepad window exists. Next the ControlSetText command (ControlSetText, Edit1, Some text, % "ahk_pid " npPid) is used to send the text "Some text" to the Notepad window. The purpose of the MsgBox command is solely to pause the script so you can check the various places you might find Notepad running. Before clicking OK the only sign of the Notepad instance will be found in the Processes tab of Windows Task Manager.

### Macro Recorders

There are a number of AutoHotkey recorders available. Some of the best known are Pulover's Macro Creator, AutoScriptWriter, and Recorder by Titan. Pulover's Macro Creator looks like the most powerful tool, but on the downside for beginners it has a bit of a learning curve. For newbies, I would recommend starting with either (or both) AutoScriptWriter or Recorder by Titan.

### Difference between IfWinActive and #IfWinActive

```
#IfWinActive lets all following hotkeys only be active for different windows.

;might need titlemode 2
#ifWinActive notepad
a:: send notepad

#ifWinActive MSWord
a:: send Word!

ifWinActive is a logical check.

;hotkey always fires
a::
ifWinActive notepad
{
 ;specific lines only when active window is true
  send Notepad...
}
ifWinActive MSWord
{
  send word...
}

```

The `#IfWinActive` command allows selective blocking of hotkeys when only the offending application is open. To use `#IfWinActive` you need to know the title or class of the application. The easiest way to find this information is with AutoIt Window Spy which is included with the AutoHotkey installation.

It's important to note that `#IfWinActive` is a status type of command which executes immediately when the AutoHotkey script is loaded. It affects every hotkey that follows it in the script until another `#IfWinActive` is encountered, but cannot be used to change settings and conditions again once the script is running. If you need an action type conditional which can react while the script is running, then `IfWinActive` (without the #) is required.

### #NoTrayIcon command

You could use the `#NoTrayIcon` command, or the Menu, Tray, NoIcon command to remove the System Tray icons, but this is dangerous since you will no longer have a method for exiting the app without without rebooting your computer or stopping the process in Task Manager (see Chapter Thirty-nine). Without the System Tray icon, there is no right-click context menu with the Exit option.

### ToolTip command

The ToolTip command is used to create the pop-up window with the information from the target window. Notice that the escape character `n is used to create new lines. The last routine executed with the key combination ^#T (CNTL++T) is there to make it easy to end the script. The SetTimer command is used to turn the timer off and the ToolTip command is executed without any parameters turning the ToolTip off. Otherwise, the ToolTip would stay on the screen until the script is ended. You can add the original SetTimer command to a hotkey combination to allow you to turn the ToolTip feature on again without reloading the script

### AppsKey

Apps key which appears on many Windows keyboards is a more reliable way to open right-click context menus in AutoHotkey. Merely replace the Click Right with `SendInput {AppsKey}`

### To repeat a keystroke

Enclose in braces the name of the key followed by the number of times to repeat it. For example:

```
Send {DEL 4}  ; Presses the Delete key 4 times.
Send {S 30}   ; Sends 30 uppercase S characters.
Send +{TAB 4}  ; Presses Shift-Tab 4 times.

```

### To hold down or release a key

Enclose in braces the name of the key followed by the word Down or Up. For example:

```
Send {b down}{b up}
Send {TAB down}{TAB up}
Send {Up down}  ; Press down the up-arrow key.
Sleep 1000  ; Keep it down for one second.
Send {Up up}  ; Release the up-arrow key.

```

### Window Spy

Window Spy or AU3_Spy.exe is a program which can be used to retrieve information about a window for scripting.
While running AHK, right-click the AHK icon in the notification area and select Window Spy.
You can now click any window to retrieve useful information such as window title, mouse position, and controls.