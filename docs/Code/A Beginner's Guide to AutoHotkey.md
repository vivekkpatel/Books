# A Beginner's Guide to AutoHotkey by Jack Dunning

### Windows Always on Top

`SetWorkingDir %A_ScriptDir% ;` Ensures a consistent starting directory.

```
; The original alwayw-on-top Hotkey in one line:
; ^#F8::WinSet, AlwaysOnTop, toggle, A

```

**Add ToolTip to tell whether always-on-top turned on or off**

```
^#*F8*::
 WinSet, AlwaysOnTop, toggle, A
 WinGet, ExStyle, ExStyle, A
 Tooltip, % (ExStyle & 0x8 = 0) ? "Always-On-Top OFF" : "Always-On-Top ON"
 Sleep, 1500
 ToolTip
Return

```

**Windows TIP**

> In Windows Explorer hold down right mouse key while dragging a file or folder to new location. When cursor is in the target folder, release the right mouse button and menu of option will pop up: move here, copy here, create shortcut
> 

```
Shell Command
Shell:Startup 'User startup
Shell:commonStartup'All user startup
Shell:programFiles
Shell:programs

```

### Tools

[Pulover's Macro Creator](http://www.autohotkey.com/board/topic/79763-macro-creator-v400-automation-tool-recorder-writer/)

AutoScriptWriter

[Recorder by Titan](http://www.autohotkey.com/board/topic/21866-macro-recorder/)

Pulover's Macro Creator looks like the most powerful tool, but on the downside for beginners it has a bit of a learning curve. For newbies, I would recommend starting with either (or both) AutoScriptWriter or Recorder by Titan.