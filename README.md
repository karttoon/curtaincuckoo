## CurtainCuckoo

### [+] INTRO [+]

CurtainCuckoo is a Cuckoo module interpretation of the Curtain standalone script that I created for analzying obfuscated PowerShell. It works by taking advantage of the PowerShell ScriptBlock Logging feature which records the PowerShell code that gets executed. A PowerShell script is dropped which pulls/parses the events from the guest machine and Cuckoo will transfer the log file to the host machine. The Cuckoo module will then further parse and display the results in a new tab so that you can see each level of ScriptBlock events and the deobfuscated PowerShell in the process.

Not all PowerShell will be debofuscated, such as token-replacement, but in general you will be able to see the full execution flow of the PowerShell and generally remove multiple layers of obfuscation or easier analysis.

![Curtain Cuckoo](https://github.com/karttoon/curtaincuckoo/curtain_27AUG2018.png)

### [+] INSTALL [+]

There are multiple files that need to be setup for the module to be installed in Cuckoo. This module was designed and tested on Cuckoo 2.0.

The following files can just be dropped into their respective locations:

```
index.html -> /usr/local/lib/python2.7/dist-packages/cuckoo/web/templates/analysis/pages/curtain/index.html

processing_curtain.py -> /usr/local/lib/python2.7/dist-packages/cuckoo/processing/curtain.py

auxiliary_curtain.py -> .cuckoo/analyzer/windows/modules/auxiliary/curtain.py
```

The following files need to be merged with existing files. I've included a breakdown of the level where they need to go within the files, incase it's not obvious. I feel that putting the tab in the sidebar above the "Extracted" button makes the most sense. This allows you to quickly pivot from the extracted initial PowerShell command (the fully obfuscated one run on the CLI) to the "Curtain" button that shows the events.

```
nav-sidebar.html -> /usr/local/lib/python2.7/dist-packages/cuckoo/web/templates/analysis/pages/nav-sidebar.html
  nav -
    ul -
      X

sidebar.html -> /usr/local/lib/python2.7/dist-packages/cuckoo/web/templates/analysis/pages/sidebar.html
  div -
    div -
      div -
        ul -
          X

config.py -> /usr/local/lib/python2.7/dist-packages/cuckoo/common/config.py
  class Config -
    configuration -
      processing -
        X
```

The following files can just be appended to the existing ones, there are no levels.

```
auxiliary.conf -> .cuckoo/conf/auxiliary.conf

processing.conf -> .cuckoo/conf/processing.conf
```
