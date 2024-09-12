---
layout: default
permalink: /plugin-tool
---

# LLM Tools on Vivado Setup Guide

*Siyu Qiu*

*Posted on Aug, 2024*
    
## Table of Contents
1. Overview
2. At UNSW? Using the CSE Virtual Machine
3. On your own machine? Install and Setup Environment
4. Hardware Project Setup
    1. Create a Project in Vivado
    2. Add the plugin Button
    3. Configure the Tcl Script
5. Usage
6. Video Tutorial


## Overview

This guide provides a step-by-step approach to setting up and using a custom Large Language Model (LLM) integration within Xilinx Vivado. By following these instructions, users will be able to create a new project in Vivado, add custom commands, and configure Tcl scripts to facilitate seamless interactions with LLM tools. This setup aims to enhance the hardware design process by leveraging LLM capabilities to provide instant responses and solutions directly within the Vivado environment.

## At UNSW? Using the CSE Virtual Machine
    
For users using the CSE virtual machine, the Vivado environment is pre-configured, and the plugin tool is already set up for you. However, before you use the tool, you need to follow step 4 (Configure the Tcl Script) to ensure everything is working correctly.

After entering those two commands, the plugin tool is ready.
    
## On your own machine? Install and Setup Environment

To set up the environment for this tool, follow these steps:

### 1. Download and Install Required Files:

Use the following command to download the necessary installation script:

**If using Linux:**

```bash
curl -L -O https://raw.githubusercontent.com/annnnie-qiu/download/master/install.sh
```

**If using Windows (Powershell):**
```
Invoke-WebRequest -Uri https://raw.githubusercontent.com/annnnie-qiu/download/master/install.bat -OutFile install.bat
```

After downloading, run the script to install the required packages and files (in WSL or in Git):
```
sh install.sh
```

For Windows users, open the batch file and install the environment.

This will:
* Install the LLM4HW package.
* Download and unzip the required files
* Delete the downloaded zip file.


### 2. Alternative Installation Method:

If you choose not to use the installation script (`install.sh`), you can manually perform the following steps:

- Install <a href="https://pypi.org/project/LLM4HW/">LLM4HW</a> the `LLM4HW` with the necessary packages and system dependencies:
  ```
  pip install LLM4HW
  ```

- Download and unzip the required files:
  ```
  curl -L -O https://github.com/annnnie-qiu/download/raw/master/provide_to_students.zip
  unzip provide_to_students.zip
  rm provide_to_students.zip
  ```
Following these steps will ensure that all necessary packages and files are installed and set up correctly.

### 3. Finalize Setup and Configuration:
After completing the installation steps, you will find three essential files in your project directory:

*  **.env**: This file is crucial for configuring your environment variables. You need to update the ```LLM4HW_ACCOUNT``` variable with a key provided by your professor or the academic in charge. This key is necessary to access certain features and functionalities within the LLM4HW tool.
    
*  **script.tcl**: This script is designed to be used within Vivado. While no immediate changes are necessary, specific instructions on how to adjust the script for your project’s requirements will be provided in the following sections.
     
*  **new.py**: This Python script contains the core logic for your project. No modifications are required, and it’s ready to be used as-is.

    
    
#### <span style="color: red;">Important:</span>
*  **Request the ```LLM4HW_ACCOUNT``` Key**: **Contact your professor or the academic in charge** to obtain the necessary key for the ```.env``` file. Without this key, certain operations may not function correctly.

*  **Verify Configuration**: Before running any scripts, double-check that all environment variables are correctly set up in the ```.env``` file.

By following these steps, your environment will be fully configured and ready for development or deployment. Further instructions for using ```script.tcl``` in Vivado will be provided in the next sections.


## Hardware Project Setup
### Create a Project in Vivado:
Open Vivado and create a new project.

### Add a Custom Button:
* Navigate to Tools &gt; Custom Commands &gt; Customize Commands… (shown as Figure 1)
  
* Create your own Tcl button by clicking on the “+” to add a new Custom Command.
    <img src="/picture/picture1.png" alt="Figure 1" style="width: 90%;">
    
* Enter a unique command name, e.g., LLM4HW, and press Enter.
    <img src="/picture/picture2.png" alt="Figure 2" style="width: 90%;">
    
* Set up the custom command
        - **Menu Name:** Give a distinctive name to the button (e.g., LLM4HW).
        - **Description:** Enter "Waiting LLM response."
        - **Source Tcl File:** Browse and select the direction of script.tcl file you download before.
        - Click on “Add to the Toolbar” and then click Apply.
        - Click OK.
    <img src="/picture/picture3.png" alt="Figure 3" style="width: 90%;">

Now, you should see a new button on the top toolbar in Vivado.
    <img src="/picture/picture4.png" alt="Figure 4">


## Configure the Tcl Script:
1. Determine the Tcl and Tk versions used by Python's Tkinter:
```
python -c "
import tkinter as tk
import os

root = tk.Tk()
tcl_lib = root.tk.eval('info library')
tk_lib = root.tk.eval('info library')

print('Tcl version:', root.tk.call('info', 'patchlevel'))
print('Tk version:', root.tk.call('info', 'patchlevel'))
print('Tcl library location:', tcl_lib)
print('Tk library location:', tk_lib)

root.destroy()
"
```
    
2. Find the location of the Python executable:
Use the command: ```where python``` or ```where.exe python```

<span style="color: red;">HITS:</span>
the output of where python is signal '\' and we need to change it to '\\'.

4. Open script.tcl and modify the commands according to the output of the previous steps.

```
unset -nocomplain ::env(PYTHONHOME)
unset -nocomplain ::env(PYTHONPATH)
#! /usr/bin/tclsh
proc call_python {} {
    set env(TCL_LIBRARY) &lt;tcl library location&gt;
    set env(TK_LIBRARY) &lt;tk library loaction&gt;
    set python_script_path &lt;the location path you download for new.py&gt;
    set python_exe &lt;location of the python.exe on your system&gt;
    set project_path [get_property DIRECTORY [current_project]]
    set output [exec $python_exe $python_script_path $project_path]
    puts $output
}
call_python
```

For example, if you follow this step-by-step guide, you will expect the commands to look like the following:
```
unset -nocomplain ::env(PYTHONHOME)
unset -nocomplain ::env(PYTHONPATH)
#! /usr/bin/tclsh
proc call_python {} {
    set env(TCL_LIBRARY) "D:\\app\\tcl\\tcl8.6"
    set env(TK_LIBRARY) "D:\\app\\tcl\\tk8.6"
    set python_script_path "D:\\chip chat\\llm-hw-help-annie\\new.py"
    set python_exe "D:\\app\\python.exe"
    set project_path [get_property DIRECTORY [current_project]]
    set output [exec $python_exe $python_script_path $project_path]
    puts $output
}
call_python
```

4. Before you use it, type the two commands (in script.tcl file) to TCL console first
    ```set env(TCL_LIBRARY) &lt;tcl library location&gt;```
    ```set env(TK_LIBRARY) &lt;tk library location&gt;```
    <img src="/picture/picture5.png" alt="Figure 5" style="width: 90%;">

5. Now, the plugin tool is ready to be used in Vivado!

## Usage
**Operation:**
Press the newly added button to open a new window. A default question is preset, and you can wait for your response.
    <img src="/picture/picture6.png" alt="Figure 6" style="width: 90%;">
    
If you have more questions, type them into the “Ask Follow Up Question” box.
    <img src="/picture/picture7.png" alt="Figure 7" style="width: 90%;">
    
**Completion:**
Once you have received your response and know how to proceed, press the exit button to close the tool.

We appreciate your feedback on the responses!

**Please share your thoughts so we can continue to improve.**
    <img src="/picture/picture8.png" alt="Figure 8" style="width: 90%;">

## Video Tutorial
    <video controls width="90%">
            <source src="video/LLM_response.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>


