---
layout: default
title: Plugin Tool Setup
permalink: /plugin-tool
---

## LLM Tools on Vivado Setup Guide 

### Siyu Qiu 

* TOC
{:toc}

<details>
  <summary>Overall</summary>
  <p>This guide provides a step-by-step approach to setting up and using a custom Large Language Model (LLM) integration within Xilinx Vivado. By following these instructions, users will be able to create a new project in Vivado, add custom commands, and configure Tcl scripts to facilitate seamless interactions with LLM tools. This setup aims to enhance the hardware design process by leveraging LLM capabilities to provide instant responses and solutions directly within the Vivado environment.</p>
</details>

<details>
  <summary>Install and Setup Environment</summary>
  <p>Follow the instructions provided at <a href="https://pypi.org/project/LLM4HW/">LLM4HW</a> to install the necessary packages and system dependencies. You can download using <code>pip install LLM4HW</code>.</p>
</details>

<details>
  <summary>Hardware Project Setup</summary>
  <details>
    <summary>Create a Project in Vivado</summary>
    <p>Open Vivado and create a new project.</p>
  </details>
  <details>
    <summary>Add a Custom Button</summary>
    <ul>
      <li>Navigate to Tools &gt; Custom Commands &gt; Customize Commands…</li>
      <li>Create your own Tcl button by clicking on the “+” to add a new Custom Command.</li>
      <img src="/picture/picture1.png" alt="Figure 1">
      <li>Enter a unique command name, e.g., LLM4HW, and press Enter.</li>
      <img src="/picture/picture2.png" alt="Figure 2">
      <li>Set up the custom command:
        <ul>
          <li><strong>Menu Name:</strong> Give a distinctive name to the button (e.g., LLM4HW).</li>
          <li><strong>Description:</strong> Enter "Waiting LLM response."</li>
          <li><strong>Source Tcl File:</strong> Browse and select the direction of your script.tcl file.</li>
          <li>Click on “Add to the Toolbar” and then click Apply.</li>
          <li>Click OK.</li>
        </ul>
      </li>
      <img src="/picture/picture3.png" alt="Figure 3">
    </ul>
    <p>Now, you should see a new button on the top toolbar in Vivado.</p>
    <img src="/picture/picture4.png" alt="Figure 4">
  </details>
  <details>
    <summary>Configure the Tcl Script</summary>
    <p>Determine the Tcl and Tk versions used by Python's Tkinter:</p>
    <pre><code>python -c "import tkinter as tk; root = tk.Tk(); print('Tcl version:', root.tk.call('info', 'patchlevel')); print('Tk version:', root.tk.call('info', 'patchlevel')); root.destroy()"</code></pre>
    <p>Find the location of the Python executable:
    <pre><code>where python</code></pre>
    <p>Modify the commands in your script.tcl according to the output of the previous steps.</p>
    <pre><code>unset -nocomplain ::env(PYTHONHOME)
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
  </details>
</details>

<details>
  <summary>Usage</summary>
  <p>Press the newly added button to open a new window. A default question is preset, and you can await the response.</p>
  <img src="/picture/picture6.png" alt="Figure 6">
  <p>If you have more questions, type them into the “Ask Follow Up Question” box.</p>
  <img src="/picture/picture7.png" alt="Figure 7">
  <p>Once you have received your response and know how to proceed, press the exit button to close the tool.</p>
  <p>We appreciate your feedback on the responses! Please share your thoughts so we can continue to improve.</p>
  <img src="/picture/picture8.png" alt="Figure 8">
</details>
