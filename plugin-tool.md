---
layout: others
title: Plugin Tool Setup
permalink: /plugin-tool
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1 class="tutorial-title">LLM Tools on Vivado Setup Guide </h1>
        <h2 class="tutorial-subtitle">Siyu Qiu </h2>
        <p class="tutorial-date">Posted on Aug, 2024</p>
      
        <p class="tutorial-tags">Tags: tutorial, LLM, vivado, tooling, debugging, HDL</p>
    </div>
</body>
</html>


## Table of Contents
- [Overall](#overall)
- [Install and Setup Environment](#install-and-setup-environment)
- [Hardware Project Setup](#hardware-project-setup)
  - [Create a Project in Vivado](#create-a-project-in-vivado)
  - [Add a Custom Button](#add-a-custom-button)
  - [Configure the Tcl Script](#configure-the-tcl-script)
- [Usage](#usage)
### Overall 

This guide provides a step-by-step approach to setting up and using a custom Large Language Model (LLM) integration within Xilinx Vivado. By following these instructions, users will be able to create a new project in Vivado, add custom commands, and configure Tcl scripts to facilitate seamless interactions with LLM tools. This setup aims to enhance the hardware design process by leveraging LLM capabilities to provide instant responses and solutions directly within the Vivado environment.

### Install and setup environment 

Follow the instructions provided at [LLM4HW](https://pypi.org/project/LLM4HW/) to install the necessary packages and system dependencies.

You can download using `pip install LLM4HW`

### Hardware Project Setup  

#### Create a Project in Vivado: 

- Open Vivado and create a new project.

#### Add a Custom Button: 

- Navigate to Tools > Custom Commands > Customize Commands… (shown as Figure 1)
- Create your own Tcl button by clicking on the “+” to add a new Custom Command. 

<img src="/picture/picture1.png" alt="Figure 1" style="width: 90%;">

- Enter a unique command name, e.g., LLM4HW, and press Enter.

  <img src="/picture/picture2.png" alt="Figure 2" style="width: 90%;">

- Set up the custom command
- **Menu Name:** Give a distinctive name to the button (e.g., LLM4HW). 
- **Description:** Enter "Waiting LLM response." 
- **Source Tcl File:** Browse and select the direction of your script.tcl file. 
- Click on “Add to the Toolbar” and then click Apply. 
- Click OK. 

<img src="/picture/picture3.png" alt="Figure 3" style="width: 90%;">

Now, you should see a new button on the top toolbar in Vivado.

![Figure 4](/picture/picture4.png)

### Configure the Tcl Script: 

1. Determine the Tcl and Tk versions used by Python's Tkinter:
- Open your command prompt and type the following:
<pre><code>
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
</code></pre>

2. Find the location of the Python executable:
   - Use the command: `where python`
3. Open script.tcl and modify the commands according to the output of the previous steps.
```   
unset -nocomplain ::env(PYTHONHOME)
    unset -nocomplain ::env(PYTHONPATH)
    #! /usr/bin/tclsh
    proc call_python {} {
        set env(TCL_LIBRARY) <tcl library location>
        set env(TK_LIBRARY) <tk library loaction>
        set python_script_path <the location path you download for client.py>
        set python_exe <location of the python.exe on your system>
        set project_path [get_property DIRECTORY [current_project]]
        set output [exec $python_exe $python_script_path $project_path]
        puts $output
    }
    call_python
 ```

  - For example, if you follow this step-by-step guide, you will expect the commands to look like the following: 
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

`set env(TCL\_LIBRARY) <tcl library location> `

`set env(TK\_LIBRARY) <tk library loaction>`

<img src="/picture/picture5.png" alt="Figure 5" style="width: 90%;">

5. Now, the plugin tool is ready to be used in Vivado! 

### Usage 

**Operation:** 

- Press the newly added button to open a new window. A default question is preset, and you can wait for your response.

<img src="/picture/picture6.png" alt="Figure 6" style="width: 90%;">


- If you have more questions, type them into the “Ask Follow Up Question” box.

<img src="/picture/picture7.png" alt="Figure 7" style="width: 90%;">

**Completion:** 

- Once you have received your response and know how to proceed, press the exit button to close the tool. 
- We appreciate your feedback on the responses! 
- **Please share your thoughts so we can continue to improve.**

<img src="/picture/picture8.png" alt="Figure 8" style="width: 90%;">
