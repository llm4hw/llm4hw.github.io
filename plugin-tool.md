---
layout: others
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


                
        <h2>Table of Contents</h2>
        <ul>
            <li><a href="#overall">Overall</a></li>
            <li><a href="#install-and-setup-environment">Install and Setup Environment</a></li>
            <li><a href="#hardware-project-setup">Hardware Project Setup</a>
                <ul>
                    <li><a href="#create-a-project-in-vivado">Create a Project in Vivado</a></li>
                    <li><a href="#add-a-custom-button">Add a Custom Button</a></li>
                    <li><a href="#configure-the-tcl-script">Configure the Tcl Script</a></li>
                </ul>
            </li>
            <li><a href="#usage">Usage</a></li>
        </ul>
        
        <h2 id="overall">Overall</h2>
        <p>This guide provides a step-by-step approach to setting up and using a custom Large Language Model (LLM) integration within Xilinx Vivado. By following these instructions, users will be able to create a new project in Vivado, add custom commands, and configure Tcl scripts to facilitate seamless interactions with LLM tools. This setup aims to enhance the hardware design process by leveraging LLM capabilities to provide instant responses and solutions directly within the Vivado environment. </p>
        
        <h2 id="install-and-setup-environment">Install and setup environment</h2>
        
        <p>Follow the instructions provided at <a href="https://pypi.org/project/LLM4HW/">LLM4HW</a> to install the necessary packages and system dependencies.</p>
        <p>You can download using <code>pip install LLM4HW</code></p>
        
        
        <h2 id="hardware-project-setup">Hardware Project Setup</h2>
        <h2>Create a Project in Vivado:</h2>
        <p>Open Vivado and create a new project.</p>
        
        <h2>Add a Custom Button:</h2>
        <p>- Navigate to Tools &gt; Custom Commands &gt; Customize Commands… (shown as Figure 1)</p>
        <p>- Create your own Tcl button by clicking on the “+” to add a new Custom Command.</p>
        <img src="/picture/picture1.png" alt="Figure 1" style="width: 90%;">
        
        <p>Enter a unique command name, e.g., LLM4HW, and press Enter.</p>
        <img src="/picture/picture2.png" alt="Figure 2" style="width: 90%;">
        
        <p>Set up the custom command</p>
        <div style="padding-left: 20px;">
            <p>- <strong>Menu Name:</strong> Give a distinctive name to the button (e.g., LLM4HW).</p>
            <p>- <strong>Description:</strong> Enter "Waiting LLM response."</p>
            <p>- <strong>Source Tcl File:</strong> Browse and select the direction of your script.tcl file.</p>
            <p>- Click on “Add to the Toolbar” and then click Apply.</p>
            <p>- Click OK.</p>
        </div>
        <img src="/picture/picture3.png" alt="Figure 3" style="width: 90%;">

        <p>Now, you should see a new button on the top toolbar in Vivado.</p>
        <img src="/picture/picture4.png" alt="Figure 4">

        
        <h2>Configure the Tcl Script:</h2>
        <p>1. Determine the Tcl and Tk versions used by Python's Tkinter:</p>
            <div style="padding-left: 20px;">
                <p>- Open your command prompt and type the following:</p>
            </div>
<pre class="codeStyle">
    <code>
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
    </code>
</pre>
        
        <p>2. Find the location of the Python executable:</p>
            <div style="padding-left: 20px;">
                <p>Use the command: <code>where python</code></p>
            </div>
        <p>3. Open script.tcl and modify the commands according to the output of the previous steps.</p>
        
<pre class="codeStyle"><code>
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
</code></pre>
        <div style="padding-left: 20px;">
            For example, if you follow this step-by-step guide, you will expect the commands to look like the following:
        </div>
    <pre class="codeStyle"><code>
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
    </code></pre>
        <p>4. Before you use it, type the two commands (in script.tcl file) to TCL console first</p>
        <code>set env(TCL_LIBRARY) &lt;tcl library location&gt;</code>
        <code>set env(TK_LIBRARY) &lt;tk library location&gt;</code>
        <img src="/picture/picture5.png" alt="Figure 5" style="width: 90%;">
        <p>5. Now, the plugin tool is ready to be used in Vivado! </p>
        
        <h2 id="usage">Usage</h2>
        <p><strong>Operation:</strong>
    Press the newly added button to open a new window. A default question is preset, and you can wait for your response.
        </p>
        <img src="/picture/picture6.png" alt="Figure 6" style="width: 90%;">
        
    If you have more questions, type them into the “Ask Follow Up Question” box.
        <img src="/picture/picture7.png" alt="Figure 7" style="width: 90%;">
        
        <p><strong>Completion:</strong></p>
    Once you have received your response and know how to proceed, press the exit button to close the tool.
    We appreciate your feedback on the responses!
    <strong>Please share your thoughts so we can continue to improve.</strong>
        <img src="/picture/picture8.png" alt="Figure 8" style="width: 90%;">

