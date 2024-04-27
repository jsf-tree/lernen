
##
```batch
REM This is a comment

REM This is turns off auto echo
@echo off  

echo:    This gets printed, considering the leading spaces.

REM This guarantees the terminal properly print the PT-BR encoding (windows-1252)
chcp 1252 > nul
```
--- 
## Get all files and modified date into result.txt  
Check in the last 5 mins, create variable  
```bat
dir /T:W /O:D /S /P "" | findstr /R "[0-9][0-9]:[0-9][0-9]" > result.txt
for /f "tokens=1,2,3,4" %a in (result.txt) do @(if "%a"=="%DATE%" (
    set stored_time=10:35
    set "current_hours=%time:~0,2%"
    set "current_minutes=%time:~3,2%"
    set "stored_hours=%b:~0,2%"
    set "stored_minutes=%b:~3,2%"
    set /a "diff = (current_hours - stored_hours) * 60 + (current_minutes -  stored_minutes)"
    if %diff% LEQ 5 (echo "%a %b %d")
    )
```


----------------------------------------------------------------
IN SHORT:  
! is a more powerful % used to call variables outside blocks (like for) where they were defined. For this to work, one needs to "setlocal enabledelayedexpansion"

> When to use ! in batch?  
First of all: The delayed variable expansion has to be set (setlocal EnableDelayedExpansion) in order to be even able to use exclamation marks instead of percent signs.

> Why delayed expansion?  
Normally variables are expanded to their value before a command line (or an in parentheses enclosed block of command lines) is executed. That means you can't access the new value of a variable if it was changed during the runtime of a command line or block. To avoid this "early" expansion you have to enable the delayed variable expansion and enclose variable names in ! rather than %.

----------------------------------------------------------------

'echo' prints things  
IF EXIST filename.txt (echo exists!) ELSE (echo it doesnt exist!)
dir src /b /s *.java > sources.txt 			<- saves to sources.txt all paths that end up with *.java and also their directories

----------------------------------------------------------------

COMMENTING IN BATCH  
A batch file can be commented using either two colons :: or a REM command.
The main difference is that the lines commented out using the REM command 
will be displayed during execution of the batch file (can be avoided by 
setting @echo off ) while the lines commented out using :: , won't be printed.

----------------------------------------------------------------

TIMER - AFTER 5 SECONDS, CHOICES "yes" AND RETURN NULL  
choice /d y /t 5 > nul

----------------------------------------------------------------
```bat
@echo off
echo Hi, I'm doing some stuff
echo OK, now I need to take a breather for 1 second...
choice /d y /t 5 > nul
echo Times up! Here I go again...
pause > null
```

----------------------------------------------------------------

RENAME ALL:
```bat
@echo off
dir
setlocal enabledelayedexpansion

echo: __________________________________________
set /p Pattern_input="  Pattern: "
echo:
set /p Replace_input="  Replace: "
echo: __________________________________________

set "Pattern=%Pattern_input%"
set "Replace=%Replace_input%"


echo:
for %%F in (*) do (
                   set "File=%%~F"
                   echo: %%F
                   echo: !File:%Pattern%=%Replace%!
                   echo:
                   ren "%%F" "!File:%Pattern%=%Replace%!"
                   )

echo:
echo:
echo: Pressione qualquer tecla para continuar
pause > nul
```

----------------------------------------------------------------

Iteratables start with %%
"@command" runs command without echoing its output

---------------
