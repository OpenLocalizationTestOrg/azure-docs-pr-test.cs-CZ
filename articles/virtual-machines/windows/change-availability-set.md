---
title: "Změna nastavení dostupnosti virtuální počítače | Microsoft Docs"
description: "Zjistěte, jak změnit sadu dostupnosti pro virtuální počítače pomocí Azure PowerShell a modelu nasazení Resource Manager."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 44c90f90-bc9a-4260-a36f-5465e2a1ef94
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/15/2016
ms.author: drewm
ms.openlocfilehash: d1daa01191480eaeb81727416b2134b00c698dc3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="change-the-availability-set-for-a-windows-vm"></a><span data-ttu-id="50860-103">Změňte sadu dostupnosti pro virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="50860-103">Change the availability set for a Windows VM</span></span>
<span data-ttu-id="50860-104">Následující kroky popisují postup změna nastavení dostupnosti virtuálního počítače pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50860-104">The following steps describe how to change the availability set of a VM using Azure PowerShell.</span></span> <span data-ttu-id="50860-105">Virtuální počítač můžete přidat jenom při vytvoření sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="50860-105">A VM can only be added to an availability set when it is created.</span></span> <span data-ttu-id="50860-106">Chcete-li změnit dostupnost nastaven, je potřeba odstranit a znovu vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="50860-106">In order to change the availability set, you need to delete and recreate the virtual machine.</span></span> 

## <a name="change-the-availability-set-using-powershell"></a><span data-ttu-id="50860-107">Změnit skupinu dostupnosti pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="50860-107">Change the availability set using PowerShell</span></span>
1. <span data-ttu-id="50860-108">Zaznamenejte následující podrobnosti klíče z virtuálního počítače, který má být změněn.</span><span class="sxs-lookup"><span data-stu-id="50860-108">Capture the following key details from the VM to be modified.</span></span>
   
    <span data-ttu-id="50860-109">Název virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="50860-109">Name of the VM</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
   
    <span data-ttu-id="50860-110">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="50860-110">VM Size</span></span>
   
    ```powershell
    $vm.HardwareProfile.VmSize
    ```
   
    <span data-ttu-id="50860-111">Primární síťové rozhraní sítě a volitelné síťových rozhraní, pokud existují ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="50860-111">Network primary network interface and optional network interfaces if they exist on the VM</span></span>
   
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```
   
    <span data-ttu-id="50860-112">Disk s operačním systémem profilu</span><span class="sxs-lookup"><span data-stu-id="50860-112">OS Disk Profile</span></span>
   
    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```
   
    <span data-ttu-id="50860-113">Profily disku pro každý datový disk</span><span class="sxs-lookup"><span data-stu-id="50860-113">Disk profiles for each data disk</span></span> 
   
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```
   
    <span data-ttu-id="50860-114">Nainstalovat rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="50860-114">VM extensions installed</span></span> 
   
    ```powershell
    $vm.Extensions
    ```
2. <span data-ttu-id="50860-115">Odstranění virtuálního počítače bez odstranění disky nebo síťová rozhraní.</span><span class="sxs-lookup"><span data-stu-id="50860-115">Delete the VM without deleting any of the disks or the network interfaces.</span></span>
   
    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```
3. <span data-ttu-id="50860-116">Vytvořit skupinu dostupnosti Pokud ještě neexistuje</span><span class="sxs-lookup"><span data-stu-id="50860-116">Create the availability set if it does not already exist</span></span>
   
    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```
4. <span data-ttu-id="50860-117">Znovu vytvořte virtuální počítač pomocí nové sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="50860-117">Recreate the VM using the new availability set</span></span>
   
    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>
   
    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]
   
    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 
   
    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 
5. <span data-ttu-id="50860-118">Přidejte datových disků a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="50860-118">Add data disks and extensions.</span></span> <span data-ttu-id="50860-119">Další informace najdete v tématu [připojit datový Disk k virtuálnímu počítači](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [rozšíření v šablonách Resource Manageru](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="50860-119">For more information, see [Attach Data Disk to VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Extensions in Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span> <span data-ttu-id="50860-120">Datové disky a rozšíření lze přidat do virtuálního počítače pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="50860-120">Data disks and extensions can be added to the VM using PowerShell or Azure CLI.</span></span>

## <a name="example-script"></a><span data-ttu-id="50860-121">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="50860-121">Example Script</span></span>
<span data-ttu-id="50860-122">Následující skript představuje příklad shromažďování požadované informace, odstranit původní virtuální počítač a potom je znovu vytvořit v nové sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="50860-122">The following script provides an example of gathering the required information, deleting the original VM and then recreating it in a new availability set.</span></span>

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a><span data-ttu-id="50860-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50860-123">Next steps</span></span>
<span data-ttu-id="50860-124">Přidejte další úložiště k virtuálnímu počítači tak, že přidáte další [datový disk](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="50860-124">Add additional storage to your VM by adding an additional [data disk](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

