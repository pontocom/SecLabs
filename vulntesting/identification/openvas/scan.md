# Scanning a target

In this example we will configure a scanning task to identify vulnerabilities on a given target. This provides a simple example to demonstrated the basic usage of OpenVAS.

## Necessary Requirements

In order to do this, you will need to use the following:

- **Kali Linux**: Kali Linux virtual machine with the OpenVAS tool installed
- **Metasploitable**: Metasploitable 2 virtual machine.

You will need to know the IP address of the target machine. For the sake of this guide, we will assume that the IP address is **192.168.8.142**.

## Starting a new scanning task

In order to accomplish this task, it is necessary to open the OpenVAS and to go to the “**Scans**” tab.

On the top left of the page you find some icons that allow the creation of tasks.

![](../assets/../../assets/openvas12.png)

Select “New Task…” and then fill the required information.

![](../assets/../../assets/openvas13.png)

You should provide a name for the task and you should select the appropriate target (this depends on your configuration, but I’m assuming it is **192.168.8.142**). 

If this is a new target, the target needs to be created. To do that, select this icon:

![](../assets/../../assets/openvas14.png)

This will popup a new window, where you will be able to create a new target. Check this window below, where you should fill all the required information.

![](../assets/../../assets/openvas15.png)

So, after filling all the required information on the “New Task” window, you can “Save” this new task.

![](../assets/../../assets/openvas16.png)

And a “New” task will appear on the scan task list.

![](../assets/../../assets/openvas17.png)

From here, we can start this newly created task. Simply press play button on the right side of the task.

![](../assets/../../assets/openvas18.png)

Now, simply sit and wait. Depending on the target, this may take a huge amount of time. You can watch and control the progress of the task and you’ll be notified when it ends.

## Checking results

After the scanning tasks completes, you may look at the details of the scanning conducted on the target. The status of the task is “Done”, and you might check the severity of the vulnerabilities encountered.

![](../assets/../../assets/openvas19.png)

If you select the name of the task, you’ll see the details of it, including the duration the task took to complete.

![](../assets/../../assets/openvas20.png)

If you press the date of the specific task, you’ll get the results of the selected task.

![](../assets/../../assets/openvas21.png)

From here, it is possible to observe the different results of the vulnerability scanning task. You cab look at the specific results list ordered by its severity.

![](../assets/../../assets/openvas22.png)

You can select a specific vulnerability to know about its details.

![](../assets/../../assets/openvas23.png)

Now try look at the different vulnerabilities that OpenVAS was able to identify.