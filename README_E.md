# InvokeV Container

InvokeV Container provides container virtual machine clone management and enables more faster and more small footprint VMs on your familiar Hyper-V environment.
InvokeV Container enables linked clone similar feature on your Hyper-V environment.

## Feature
* Enables following virtual machine recycle process with easy way. 
  create, dispose, imaging
* Enables minimize container image using vhdx differencial file technology
* All OS supported by HyperV can available (Windows,NanoServer,Linux...etc)
* If you use NanoServer image for base, deployment enables more faster and more small footprint virtual machine.
* It can be used in the same specifications as the virtual machine, such as network and security on Hyper-V.
* Existing virtual machines with Hyper-v on mixed use is possible.
* Create a container image yourself.
* Enables both PowerShell and GUI tool management
* Enables GUI tool management via Remote desktop
* Enables create new image by combine parent's container image and child container image
* This container doesn't concern with Windows container and Docer container. 


## Supported Environment
Windows Server 2016 Hyper-V  
Windows Server 2012 R2 Hyper-V  
Windows 10 Hyper-V  
  
  
## Install
1.Download the following files on same folder [InvokeVContainer.psm1](/InvokeVContainer.psm1) , [Setup.ps1](/Setup.ps1) 
If you recieve the security alert, unlock these scripts.  
 
    PS C:\Users\Administrator\Downloads> Unblock-File .\Setup.ps1   
     
2.Execute Setup.ps1 with repository path.

    PS C:\Users\Administrator\Downloads> .\Setup.ps1 "D:\"

 Under the targeted path "InvokeVContainer" folder (example D:\InvokeVContainer) was created.
 InvoleVContainer.psm1 file were copyed to the following path. 
"C:\Users\(logined user name)\Documents\WindowsPowerShell\Modules\InvokeVContainer\InvokeVContainer.psm1"

Note:
- If you have already installed Docker-PowerShell of Windows container, you need uninstall Docker-PowerShell with the command "Uninstall-Module Docker" or "Remove-Module Docker".
- If you change InvokeVContainer.psm1, You shold reload the module with command "Remove-Module *"
- When you Uninstall, just delete repositry folder and InvokeVContainer.psm1 file.
  
  
## Confirmation Commands 

    > Get-Command -Module InvokeVContainer

    CommandType     Name                                               Version    Source                         
    -----------     ----                                               -------    ------                         
    Function        Correct-ContainerImage                             0.0        InvokeVContainer               
    Function        Export-ContainerImage                              0.0        InvokeVContainer               
    Function        Get-Container                                      0.0        InvokeVContainer               
    Function        Get-ContainerImage                                 0.0        InvokeVContainer               
    Function        Get-ContainerIPAddress                             0.0        InvokeVContainer               
    Function        Get-InvokeVContanerRoot                            0.0        InvokeVContainer               
    Function        Get-TreeView                                       0.0        InvokeVContainer               
    Function        Import-ContainerImage                              0.0        InvokeVContainer               
    Function        Merge-ContainerImage                               0.0        InvokeVContainer               
    Function        New-Container                                      0.0        InvokeVContainer               
    Function        New-ContainerImage                                 0.0        InvokeVContainer               
    Function        Remove-Container                                   0.0        InvokeVContainer               
    Function        Remove-ContainerImage                              0.0        InvokeVContainer               
    Function        Run-Container                                      0.0        InvokeVContainer               
    Function        Set-ContainerIPConfig                              0.0        InvokeVContainer               
    Function        Set-TreeView                                       0.0        InvokeVContainer               
    Function        Start-Container                                    0.0        InvokeVContainer               
    Function        Stop-Container                                     0.0        InvokeVContainer               
    Function        Wait-ContainerBoot                                 0.0        InvokeVContainer               
  
  
## Base Container Virtual Machine Image
Base Container Virtual Machine is OS installed vhdx file. vhdx file were import as a Virtual Machine Image.   

    > Import-ContainerImage -FilePath "D:\Hyper-V\Win2016\Win2016.vhdx"

GUID assigned to vhdx file were copied to D:\InvokeVContainer\Imageg (This process takes time depends on its vhdx file size)

  
## Confirmation Container Virtual Machine Image

    > Get-ContainerImage

    Name       : IMG_01
    Path       : D:\InvokeVContainer\Images\IMG_01_2110bc02-d624-4c78-879b-dd6f5601fabc_1d0ac088-4547-4a49-bfa5-7f542e08a386.vhdx
    Size(MB)   : 1947
    Created    : 2017/01/16 21:49:01
    ParentPath : D:\InvokeVContainer\Images\Win2016__2110bc02-d624-4c78-879b-dd6f5601fabc.vhdx

    Name       : MyNewIMG_01
    Path       : D:\InvokeVContainer\Images\MyNewIMG_01__ac2e69c7-c406-42a3-a0a9-b7d6cbc8f116.vhdx
    Size(MB)   : 18308
    Created    : 2016/10/13 12:09:56
    ParentPath : 

    Name       : Win2016
    Path       : D:\InvokeVContainer\Images\Win2016__2110bc02-d624-4c78-879b-dd6f5601fabc.vhdx
    Size(MB)   : 18308
    Created    : 2016/10/13 12:09:56
    ParentPath : 
  
  
## Create first container

    > New-Container -ContainerName "CON_01" -ImageName "Win2016" -Memory 2048MB -Processer 1 -SwitchName "NAT"

-SwithName: Virtual switch name created with Hyper-V manager.
When you use NAT virtual switch created with New-NetNat command, IP address assignment is needed. 

    > Run-Container -ContainerName "CON_01" -ImageName "Win2016" -Memory 2048MB -Processer 1 -SwitchName "NAT" -IPAddress 172.16.0.1 -Subnet 255.255.255.240 -Gateway 172.16.0.254 -DNS 8.8.8.8

If you use Run-Container command, you can easy to execute following steps (create container virtual image, run container virtual image, configuration IP address).

Note:
IP address assignment were only available with Container Virtual Machine Image supported by OS integrated services.
  
  
## Confirm Container Virtual Machine:

    > Get-Container

    Name     State Path                                              ParentPath                                                                   
    ----     ----- ----                                              ----------                                                                   
    CON_01 Running D:\InvokeVContainer\Containers\CON_01\CON_01.vhdx D:\InvokeVContainer\Images\Win2016__2110bc02-d624-4c78-879b-dd6f5601fabc.vhdx
  
  
## Start Container Virtual Machine:

    > Start-Container "CON_01" 
  
  
## Stop Container Virtual Machine:

    > Stop-Container "CON_01" 
  
  
## Connect Container Virtual Machine:

    > vmconnect (hostname) "CON_01" 

Connect container Virtual Machine via vmconnect.exe similar way from Hyper-V manager.
    
  
## Create New Container Virtual Machine Image:

    > New-ContainerImage -ContainerName "CON_01" -ImageName "IMG_01" 

Create New Container Virtual Machine from created first Container Virtual Machine Image.
This process doesn't depend on if Container Virtual Machine is running or not running.
Created Container Virtual Image is child file (differential file) of parent virtual machine image file.
  
  
## Merge Container Virtual Machine Image:

    > Merge-ContainerImage -ImageName "IMG_01" -NewImageName "MyNewIMG_01"
 
You can merge container virtual machine image and create new container virtual machine image.
Related container virtual machine file and pre-merged both parent and child file left.  
  
  
## Delete Container Virtual MachineImage:

    > Remove-ContainerImage "IMG_01"
  
  
## InvokeV Container Manager：
[InvokeV Container Manager](/InvokeVContainerManager.exe) is enables visualized GUI contaner management with PowerShell Command.
Because Container has complicate parent-child file relationship, this tool is usuful to manage its relationship.
Basical operation starts right mouse click.
Note: You need Administrative privilege
![Invoke VContainer Manager](./images/cap.png "Invoke VContainer Manager")


## InvokeV Container Overview：
<a href="https://youtu.be/FElIdcLgcdY" target="_blank">InvokeV Container Overview (YouTube)</a>
