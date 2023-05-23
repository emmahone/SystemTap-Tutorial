# What is SystemTap?
SystemTap is a powerful dynamic tracing tool for Linux systems that allows you to monitor and gather information about the system's operation without requiring the need for code modification or recompilation. It provides a scripting language that enables you to write custom scripts called "SystemTap scripts" to trace and analyze various aspects of the system, such as kernel function calls, system events, and performance metrics.

A SystemTap script is a program written in the SystemTap scripting language, which is similar to C programming language syntax. It consists of a set of instructions that specify what events or functions to trace and how to process the gathered data. These scripts can be used to investigate system behavior, identify performance bottlenecks, debug issues, and gain insights into the system's inner workings.

SystemTap scripts are written using a high-level abstraction layer, making it easier for developers, system administrators, and performance engineers to analyze and understand the system's behavior. The scripts can access and manipulate various system resources, such as process information, network activity, file I/O, memory usage, and more.

Once a SystemTap script is written, it needs to be compiled into a kernel module before it can be executed. The SystemTap toolchain takes care of the compilation process, generating a loadable kernel module that can be dynamically loaded into the running kernel. This allows the script to start tracing the specified events and collecting data in real-time.

SystemTap provides a vast set of built-in functions and probes that can be used within scripts to perform operations like printing output, filtering events, aggregating data, and more. Additionally, it offers a flexible API that allows users to define custom functions and probes for specific monitoring needs. SystemTap scripts provide a powerful means to observe and analyze the behavior of a Linux system at runtime, allowing users to gain insights into system performance, troubleshoot issues, and optimize system behavior.

# What are SystemTap scripts used for?
SystemTap scripts are often used for various purposes in Linux systems. Some common use cases include:

1. Performance analysis: SystemTap scripts can be used to gather performance-related data such as CPU usage, memory usage, disk I/O, network activity, and system call latency. By tracing specific functions or events, these scripts can help identify performance bottlenecks, analyze resource utilization, and optimize system performance.

2. Debugging and troubleshooting: SystemTap scripts are valuable for diagnosing and troubleshooting issues in a Linux system. They can be used to trace specific functions or system events to identify the root cause of errors, crashes, or unexpected behavior. The scripts can provide detailed information about the system state, function call sequences, variable values, and more.

3. Kernel exploration: SystemTap scripts allow developers and system administrators to gain insights into the Linux kernel's behavior. They can trace and analyze kernel function calls, system events, and interactions between different kernel components. This can be helpful for understanding the inner workings of the kernel, identifying potential bugs, or studying specific kernel subsystems.

4. Security analysis: SystemTap can be used for security-related tasks such as monitoring system calls, tracking file accesses, analyzing network traffic, and detecting anomalies. By writing custom SystemTap scripts, security analysts can gather information about potential security threats, unauthorized activities, or abnormal behavior on the system.

5. System monitoring and auditing: SystemTap scripts can be used to create custom monitoring tools or augment existing monitoring solutions. They can track system-wide activities, monitor specific processes, generate metrics, and collect data for auditing purposes. System administrators can use these scripts to monitor system performance, track resource usage, or enforce security policies.

6. Tracing application behavior: SystemTap can help in understanding the behavior of specific applications by tracing their function calls, system interactions, and resource usage. It can provide insights into how an application utilizes system resources, identifies performance bottlenecks within the application, or troubleshoots issues specific to the application's runtime environment.

# How do you write a simple SystemTap script in RHEL?
To write a SystemTap script in Red Hat Enterprise Linux (RHEL), you'll need to follow these general steps:

1. Install SystemTap: Ensure that the SystemTap package is installed on your RHEL system. You can use the package manager (e.g., `yum` or `dnf`) to install the necessary packages. For example:
```bash
sudo yum install systemtap systemtap-runtime
```
2. Write the SystemTap script: Create a new file with a `.stp` extension (e.g., `script.stp`) and open it in a text editor. This file will contain your SystemTap script code.

Here's an example of a simple SystemTap script that prints a message when a specific function is called:
```c
#!/usr/bin/stap

probe kernel.function("my_function") {
  printf("my_function called\n");
}
```

3. Save the SystemTap script: Save the script file in a convenient location on your system.
4. Compile and run the SystemTap script: To compile and run the script, you need to use the `stap` command followed by the script's filename. Run the following command in a terminal:
```bash
sudo stap script.stp
```
If the script requires additional privileges, you may need to run it with sudo or as the root user.

The `stap` command will compile the script into a kernel module, load it into the running kernel, and start tracing the specified events or functions. You should see the output of the script in the terminal as the events occur.

Note: The first time you run a SystemTap script, it may prompt you to build and load debuginfo packages. Follow the instructions provided to install the necessary debuginfo packages.
5. Terminate the script: To stop the script's execution, press `Ctrl + C` in the terminal where it's running. The SystemTap script will be unloaded from the kernel, and the tracing will be stopped.

Remember to adjust the script according to your specific tracing requirements and the events or functions you want to monitor. The SystemTap documentation ([here](https://sourceware.org/systemtap/documentation.html) and [here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/systemtap_beginners_guide/index)) provides detailed information on the available probes, functions, and scripting capabilities, allowing you to write more complex scripts for advanced tracing and analysis.

# Example using `net.stp`
To create a simple SystemTap script to inspect the network traffic of an application running on a RHEL machine, you can use the net.stp script that comes with the SystemTap examples. Here are the steps to follow:
1. Locate the `net.stp` script: The `net.stp` script is provided as an example script with SystemTap. You can find it in the SystemTap examples directory. On RHEL, the path is typically `/usr/share/systemtap/examples/network/net.stp`.
2. Copy the `net.stp` script: Copy the `net.stp` script to a directory of your choice on your RHEL machine. For example, you can create a `systemtap` directory in your home folder and copy the script there:
```bash
mkdir ~/systemtap
cp /usr/share/systemtap/examples/network/net.stp ~/systemtap/
```
3. Modify the script (optional): Open the copied `net.stp` script in a text editor. You can modify it to focus on the specific application you want to inspect. For example, you can add filters based on process names or process IDs to narrow down the traced traffic. Edit the script to fit your requirements.
4. Run the SystemTap script: In a terminal, navigate to the directory where you copied the net.stp script. Run the script using the stap command:
```bash
sudo stap net.stp
```
The `stap` command compiles and loads the SystemTap script into the running kernel. It starts tracing the network traffic according to the specified rules.
5. Observe the network traffic: As the application runs, you should see the network traffic being monitored and displayed in the terminal. The script captures information such as source and destination IP addresses, port numbers, packet sizes, and more.

Note: Running SystemTap scripts usually requires root privileges (sudo) as it involves accessing and tracing system resources.

6. Terminate the script: To stop the SystemTap script, press `Ctrl + C` in the terminal where it's running. The script will be unloaded from the kernel, and the tracing will be stopped.
By default, the net.stp script captures network traffic system-wide. However, you can customize it to focus on specific processes or network conditions by modifying the script accordingly. Refer to the comments within the net.stp script for further guidance on its usage and customization options.

# Example reading through systemtap output
Let's assume you have executed the net.stp script as described in the previous steps, and it is currently running and displaying network traffic information in the terminal. The output may look similar to the following:
```yaml
Source IP: 192.168.0.1 | Destination IP: 10.0.0.2 | Port: 8080 | Packet Size: 1024 bytes
Source IP: 10.0.0.2 | Destination IP: 192.168.0.1 | Port: 443 | Packet Size: 512 bytes
...
```
To read and interpret the output:

1. Source IP: The "Source IP" field represents the IP address of the sender of the network traffic.

2. Destination IP: The "Destination IP" field indicates the IP address of the receiver of the network traffic.

3. Port: The "Port" field specifies the port number associated with the network traffic. It can indicate the source or destination port depending on the direction of the traffic.

4. Packet Size: The "Packet Size" field shows the size of the network packet in bytes.

You can observe these fields to understand the flow of network traffic and gather insights about the communication happening in your system.

Additionally, you can leverage the SystemTap script to customize the output and include more information if needed. For instance, you can include timestamps, protocol information, or additional metadata related to the network traffic.

By examining the output and analyzing the network traffic patterns, you can gain insights into the behavior of the application or system you are monitoring. This information can be useful for troubleshooting, performance analysis, or security analysis purposes.
