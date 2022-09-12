# Microservices Architecture


## Diagram Displaying Containerisation Vs Virtualisation

![image](https://user-images.githubusercontent.com/97620055/189631914-d7955ea6-32ca-44fc-ab7a-91a5b2a6b22c.png)



## What Is Containerisation And Docker ?
   - Containerisation --> is a form of virtualization where applications run in isolated user spaces, called containers, while using the same shared operating system (OS).
    
   - Docker --> a software platform simplifies the process of building, running, managing and distributing applications. It does this by virtualizing the operating system of the computer on which it is installed and running.

## Benefits of Containerisation and Docker:



### Docker Pros:

  1. **Multi-application compatability** >  as long same OS, multiple applicatons with different requirements and dependencies can be hosted together on same host. 
  2. **Storage Optimized** > Occupy little disk space (few megabytes), so can have large number of applications can be hosted on the same host.
  3. **Robustness** > container does not have OS installed or running on it, so boot time is faster as compared to virtual machines.  
  4. **Reduces costs**. Docker is less demanding when it comes to the hardware required to run it.

### Docker Cons:

  1. Does not provide cross-platform compatability.
  2. Secuirty risks > if you are moving multiple pieces within  large scale dynamic docker enviromnent.
  3. Limitation on running applications with graphical interfaces > docker designed for hosting applications on command line interface. So need richer interfaces for applications with graphical interfaces. 
  4. Data recovery and strategy required > this is needed in case docker container goes down. Solutions are in place but not very scalable and are not automated.