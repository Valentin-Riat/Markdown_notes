# Windows

## Powershell
### Windows Terminal
Best terminal ever using the *Windows terminal* app (available on Microsoft store)

**Steps to customize**

1) To add *Quake mode* go into `Parameters->Actions->add Action` and add the display Quake window 
    This mode enables quick drop-down terminal access with one shortcut (e.g. `ctrl + space`)

1) Get the [Meslo LMG font](https://ohmyposh.dev/docs/installation/fonts) (Nerd Font)  and install it on the computer using right-click->install for all users
1) Set the terminal to use it by changing the profile params
1) Install oh-my-posh using `winget install JanDeDobbeleer.OhMyPosh -s winget`
1) In the profile params->command line, add `-ExecutionPolicy RemoteSigned -NoExit -command "oh-my-posh init pwsh | Invoke-Expression` after the `[...]\powershell.exe`


## PowerToys
A set of utilities for power users to tune and streamline their Windows experience
[see here](https://learn.microsoft.com/en-us/windows/powertoys/)

To be installed using the microsoft store and searching for **PowerToys**

The PowerToys app is used to activate any fonctionnality and change settings (They are all activated by default)

### Always on top

Allows you to pin windows to the top of all your windows (useful for the calculator)


**Shortcut :** `Win + Ctrl + T`

### Run

Type `Alt + space` to make a cool searchbar appear 

type ?filename to find a file 
type =1+1 to use calculator
type %% to use unit convertor (100 hp(i) to kw)



Don't forget to go in the parameters under unit converter and select *integate in general results*

## Sysinternals
[see here](https://learn.microsoft.com/en-us/sysinternals/)

Usefull poweruser stuff like a better task manager called *Process explorer*


## Windows subsystem for linux


## Misc
`shutdown -t 0 -r -o` : restart in advanced settings (for trubleshoot and resetting Windows) can be used after `Win + R`

`winget install [program]` can be used to install windows programs using the command line


