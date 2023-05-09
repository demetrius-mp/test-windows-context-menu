# test-windows-context-menu

Explaining the contents of the `adicionar-comando.reg` file.

```sh
Windows Registry Editor Version 5.00

# Creating a context menu action named "Novo Job", that can be executed when
# clicking on the background of a directory.
[HKEY_CLASSES_ROOT\Directory\Background\shell\Novo Job]
@="&Novo Job"

# Defining the script that will be executed when running the action
# it is assumed that the script will be under "C:\\Scripts", so make sure to
# create the "Scripts" folder in the correct location
[HKEY_CLASSES_ROOT\Directory\Background\shell\Novo Job\command]
@="\"C:\\Scripts\\novo-job.bat\" \"%1\""
```

Explaining the contents of the `remover-comando.reg` file. This file is exactly the same as above, except for the `-` prefix on the `HKEY_CLASSES_ROOT`, which means that we are unsetting the registry key (or, to be more clear, removing the context menu action).

```sh
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\Directory\Background\shell\Novo Job]
@="&Novo Job"

[-HKEY_CLASSES_ROOT\Directory\Background\shell\Novo Job\command]
@="\"C:\\Scripts\\novo-job.bat\" \"%1\""
```

Explaining the contents of the `novo-job.bat`. This is the script (program) that will be executed when running the context menu action.

```bat
REM disables the output
REM I think this prevents the CMD from opening and closing, which is kinda creepy.
@echo off

REM `cd` stands for "Change Directory"
REM Navigating to the directory where the context menu action will be executed
REM in this case, the folder from which the user right clicked
cd %1

REM `md` stands for "Make Directory", so this command will create a folder named "Novo Job" (without the quotes)
md "Novo Job"

REM Navigating to the folder we just created
cd "Novo Job"

REM Now, inside the folder, we create the 4 following folders
md Apoio
md Layout
md Final
md Preview

REM End of the script
pause
```