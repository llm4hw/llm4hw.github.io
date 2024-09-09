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
            <li><a href="#cse-virtual-machine">Using CSE Virtual Machine</a></li>
            <li><a href="#install-and-setup-environment">Install and Setup Environment</a></li>
            <li><a href="#hardware-project-setup">Hardware Project Setup</a>
                <ul>
                    <li><a href="#create-a-project-in-vivado">Create a Project in Vivado</a></li>
                    <li><a href="#add-a-custom-button">Add a Custom Button</a></li>
                    <li><a href="#configure-the-tcl-script">Configure the Tcl Script</a></li>
                </ul>
            </li>
            <li><a href="#usage">Usage</a></li>
            <li><a href="#video-tutorial">Video Tutorial</a></li>
        </ul>
        
        <h2 id="overall">Overall</h2>
        <p>This guide provides a step-by-step approach to setting up and using a custom Large Language Model (LLM) integration within Xilinx Vivado. By following these instructions, users will be able to create a new project in Vivado, add custom commands, and configure Tcl scripts to facilitate seamless interactions with LLM tools. This setup aims to enhance the hardware design process by leveraging LLM capabilities to provide instant responses and solutions directly within the Vivado environment. </p>

        <h2 id="cse-virtual-machine">Using CSE Virtual Machine</h2>
        <p>For users using the CSE virtual machine, the Vivado environment is pre-configured, and the plugin tool is already set up for you. However, before you use the tool, you need to execute the <strong>following step 4 in Configure the Tcl Script</strong> to ensure everything is working correctly</p>
        
        <p>After entering those two commands, the plugin tool is ready</p>
        

        <h2 id="install-and-setup-environment">Install and setup environment</h2>

        <p>To set up the environment for this tool, follow these steps:</p>

        <p><strong>1. Download and Install Required Files:</strong></p>
        <p>Use the following command to download the necessary installation script:</p>
        <p><strong> For using Linux: </strong></p>
        <pre><code>curl -L -O https://raw.githubusercontent.com/annnnie-qiu/download/master/install.sh</code></pre>
        <p><strong> For using Windows(Power Shell): </strong></p>
        <pre><code>Invoke-WebRequest -Uri https://raw.githubusercontent.com/annnnie-qiu/download/master/install.bat -OutFile install.bat</code></pre>
        <p>After downloading, run the script to install the required packages and files(in WSL or in Git):</p>
        <pre><code>sh install.sh</code></pre>
        <p>For Windows user, open the batch file and install the environment.</p>


        <p>This will:</p>
        <div style="padding-left: 20px;">
            <p>- Install the <code>LLM4HW</code> package.</p>
            <p>- Download and unzip the required files (including the LLM4HW content).</p>
            <p>- Clean up by removing the downloaded zip file.</p>
        </div>

        <p><strong>2. Alternative Installation Method:</strong></p>
        <p>If you choose not to use the installation script (<code>install.sh</code>), you can manually perform the following steps:</p>
        <div style="padding-left: 20px;">
            <p>- Install <a href="https://pypi.org/project/LLM4HW/">LLM4HW</a> the <code>LLM4HW</code> with the necessary packages and system dependencies:</p>
            <pre><code>pip install LLM4HW</code></pre>
            <p>- Download and unzip the required files:</p>

<style>
    pre {
        font-size: 0.9em;
        white-space: pre-wrap;
        word-wrap: break-word;
    }
</style>

<pre>
    <code>
        curl -L -O https://github.com/annnnie-qiu/download/raw/master/provide_to_students.zip
        unzip provide_to_students.zip
        rm provide_to_students.zip
    </code>
</pre>
        </div>
        <p>Following these steps will ensure that all necessary packages and files are installed and set up correctly.</p>

        <p><strong>3. Finalize Setup and Configuration:</strong></p>
        <p>After completing the installation steps, you will find three essential files in your project directory:</p>

        <div style="padding-left: 20px;">
            <p><strong>- .env</strong>: This file is crucial for configuring your environment variables. You need to update the <code>LLM4HW_ACCOUNT</code> variable with a key provided by your professor or the academic in charge. This key is necessary to access certain features and functionalities within the LLM4HW tool.</p>
        
            <p><strong>- script.tcl</strong>: This script is designed to be used within Vivado. While no immediate changes are necessary, specific instructions on how to adjust the script for your project’s requirements will be provided in the following sections.</p>
        
            <p><strong>- new.py</strong>: This Python script contains the core logic for your project. No modifications are required, and it’s ready to be used as-is.</p>
        </div>
        
        <p style="color: red;"><strong>Important:</strong></p>
        <div style="padding-left: 20px;">
            <p><strong>Request the <code>LLM4HW_ACCOUNT</code> Key:</strong> <strong>Contact your professor or the academic in charge</strong> to obtain the necessary key for the <code>.env</code> file. Without this key, certain operations may not function correctly.</p>
            <p><strong>Verify Configuration:</strong> Before running any scripts, double-check that all environment variables are correctly set up in the <code>.env</code> file.</p>
        </div>
        
        <p>By following these steps, your environment will be fully configured and ready for development or deployment. Further instructions for using <code>script.tcl</code> in Vivado will be provided in the next sections.</p>




        
        <h2 id="hardware-project-setup">Hardware Project Setup</h2>
        <h2 id="create-a-project-in-vivado">Create a Project in Vivado:</h2>
        <p>Open Vivado and create a new project.</p>
        
        <h2 id="add-a-custom-button">Add a Custom Button:</h2>
        <p>- Navigate to Tools &gt; Custom Commands &gt; Customize Commands… (shown as Figure 1)</p>
        <p>- Create your own Tcl button by clicking on the “+” to add a new Custom Command.</p>
        <img src="/picture/picture1.png" alt="Figure 1" style="width: 90%;">
        
        <p>Enter a unique command name, e.g., LLM4HW, and press Enter.</p>
        <img src="/picture/picture2.png" alt="Figure 2" style="width: 90%;">
        
        <p>Set up the custom command</p>
        <div style="padding-left: 20px;">
            <p>- <strong>Menu Name:</strong> Give a distinctive name to the button (e.g., LLM4HW).</p>
            <p>- <strong>Description:</strong> Enter "Waiting LLM response."</p>
            <p>- <strong>Source Tcl File:</strong> Browse and select the direction of script.tcl file you download before.</p>
            <p>- Click on “Add to the Toolbar” and then click Apply.</p>
            <p>- Click OK.</p>
        </div>
        <img src="/picture/picture3.png" alt="Figure 3" style="width: 90%;">

        <p>Now, you should see a new button on the top toolbar in Vivado.</p>
        <img src="/picture/picture4.png" alt="Figure 4">

        
        <h2 id="configure-the-tcl-script">Configure the Tcl Script:</h2>
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
                <p>Use the command: <code>where python</code> or <code>where.exe python</code></p>
            </div>
            <p style="color: red;"><strong> HITS:</strong></p> <p>the output of where python is signal '/' and we need to change it to '//'.</p>
        <p>3. Open script.tcl and modify the commands according to the output of the previous steps.</p>


<pre class="codeStyle">
    <code>
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
    </code>
</pre>

        <div style="padding-left: 20px;">
             <p>For example, if you follow this step-by-step guide, you will expect the commands to look like the following: </p>
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
        <p><code>set env(TCL_LIBRARY) &lt;tcl library location&gt;</code></p>
        <p><code>set env(TK_LIBRARY) &lt;tk library location&gt;</code></p>
        <img src="/picture/picture5.png" alt="Figure 5" style="width: 90%;">
        <p>5. Now, the plugin tool is ready to be used in Vivado! </p>
        
        <h2 id="usage">Usage</h2>
        <p><strong>Operation:</strong></p>
    <p>Press the newly added button to open a new window. A default question is preset, and you can wait for your response.</p>
        <img src="/picture/picture6.png" alt="Figure 6" style="width: 90%;">
        
    <p>If you have more questions, type them into the “Ask Follow Up Question” box.</p>
        <img src="/picture/picture7.png" alt="Figure 7" style="width: 90%;">
        
        <p><strong>Completion:</strong></p>
    <p>Once you have received your response and know how to proceed, press the exit button to close the tool.</p>
    <p>We appreciate your feedback on the responses!</p>
    <p><strong>Please share your thoughts so we can continue to improve.</strong></p>
        <img src="/picture/picture8.png" alt="Figure 8" style="width: 90%;">


        <h2 id="video-tutorial">Video Tutorial</h2>
        <video controls width="90%">
                <source src="video/LLM_response.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>


