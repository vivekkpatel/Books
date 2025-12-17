---
title: Digging Deeper into AutoHotkey
---

# Digging Deeper into AutoHotkey by Jack Dunning

### **Send Symbols using AutohotKey using Unicode**

```
^+o::SendInput {U+00BA}  ; Degrees

```

### Add Quotes to Selected Text

```
#+Q::       ;Add smart quotes
Send ^c
Sleep 100
SendInput {U+201C}%clipboard%{U+201D}
return

```

### Context-sensitive Hotstrings

The directives `#IfWinActive/Exist` can be used to make selected hotstrings context sensitive. Such hotstrings send a different replacement, perform a different action, or do nothing at all depending on the type of window that is active or exists. For example:

```
#IfWinActive ahk_class Notepad
::btw::This replacement text will appear only in Notepad.
#IfWinActive
::btw::This replacement text appears in windows other than Notepad.

```

**if an asterisk (\*) is placed between the first two colons like so:**

**:\*:j@::jdunning@computoredge.com the e-mail is instantly substituted when the @ character is hit—not requiring nor adding any extra space or punctuation.**

You can also use the letter O to drop the activating space or character, but you would still be required to type it. This would be used if you wanted to add an ending without needing to backspace.

### Quick Web Search

```
^+F::
  Send, ^c
  Sleep 100
  Run, <http://www.google.com/search?q=%clipboard%>

```

### WinMove

Changes the position and/or size of the specified window.

```
WinMove, X, Y
WinMove, WinTitle, WinText, X, Y , Width, Height, ExcludeTitle, ExcludeText

```

### Example

```
Run, calc.exe
WinWait, Calculator
WinMove, 0, 0 ; Move the window found by WinWait to the upper-left corner of the screen.
 
SplashTextOn, 400, 300, Clipboard, The clipboard contains:`n%clipboard%
WinMove, Clipboard, , 0, 0 ; Move the splash window to the top left corner.
MsgBox, Press OK to dismiss the SplashText
SplashTextOff
 
; The following function centers the specified window on the screen:
CenterWindow(WinTitle)
{
  WinGetPos,,, Width, Height, %WinTitle%
  WinMove, %WinTitle%,, (A_ScreenWidth/2)-(Width/2), (A_ScreenHeight/2)-(Height/2)
}

```