# Online

## Drawing
* [ExcaliDraw](https://excalidraw.com/)
* [draw.io](https://drawio-app.com/) 

# Windows Tools

## Editors and IDE
* Notepad++

### x86
Add
```
reg add "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v "Debugger" /t REG_SZ /d "\"%ProgramFiles(x86)%\Notepad++\notepad++.exe\" -notepadStyleCmdline -z" /f
```
### x64
Add
```
REG ADD “HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe” /v “Debugger” /t REG_SZ /d “\”%ProgramFiles%\Notepad++\notepad++.exe\” -notepadStyleCmdline -z” /f
```
Undo
```
REG DELETE “HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe” /v “Debugger” /f
```

* [Visual Studio Code](https://code.visualstudio.com/)
* [Visual Studio](https://visualstudio.microsoft.com/) + [Resharper](https://www.jetbrains.com/resharper/)
* [Rider](https://www.jetbrains.com/rider/)

## Development tools
* [TailViewer](https://kittyfisto.github.io/Tailviewer/)

## Source code managers
* GitHub Desktop
* SourceTree

## File managers
* TotalCommander
* WinDirStat

## Video and Picture:
* VLC
* IrfanView

## Other tools
* FoxitReader
* Tools
* Ditto
* Volume2
* BleachBit
* WinSCP
* Putty
* Advanced IP Scanner
* VeraCrypt
* TaskbarX - for centering start menu icons
* Chrome
* MS Office
* 7zip
* ESET

## Backup and Restore
* Marcium Reflect Free edition

## Wndows Server admin
* Windows Admin Center

## Network monitoring
* NetSpot (WiFi channels monitor)
* WireShark
