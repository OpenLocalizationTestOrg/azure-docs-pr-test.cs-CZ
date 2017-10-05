---
title: "Vytvořte bitovou kopii spravovaných v Azure | Microsoft Docs"
description: "Vytvořte bitovou kopii spravované zobecněný virtuální počítač nebo virtuální pevný disk v Azure. Obrázky lze použít k vytvoření více virtuálních počítačů, které používají spravovaný disky."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: cynthn
ms.openlocfilehash: f64b81489ab426b50ec89af369e1581ac71848be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a><span data-ttu-id="4642b-104">Vytvořte bitovou kopii spravované zobecněný virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="4642b-104">Create a managed image of a generalized VM in Azure</span></span>

<span data-ttu-id="4642b-105">Z zobecněný virtuální počítač, který je uložený jako spravovaných disků nebo diskem nespravované v účtu úložiště můžete vytvořit prostředek spravované bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4642b-105">A managed image resource can be created from a generalized VM that is stored as either a managed disk or an unmanaged disk in a storage account.</span></span> <span data-ttu-id="4642b-106">Obrázek pak lze vytvořit víc virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4642b-106">The image can then be used to create multiple VMs.</span></span> 


## <a name="generalize-the-windows-vm-using-sysprep"></a><span data-ttu-id="4642b-107">Generalize virtuální počítač s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="4642b-107">Generalize the Windows VM using Sysprep</span></span>

<span data-ttu-id="4642b-108">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="4642b-108">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="4642b-109">Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="4642b-109">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="4642b-110">Ujistěte se, že role serveru spuštěná na tomto počítači jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4642b-110">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="4642b-111">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="4642b-111">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4642b-112">Pokud používáte nástroj Sysprep před nahráním svůj disk VHD do Azure poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4642b-112">If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="4642b-113">Přihlaste se k virtuálnímu počítači Windows.</span><span class="sxs-lookup"><span data-stu-id="4642b-113">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="4642b-114">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="4642b-114">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="4642b-115">Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="4642b-115">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="4642b-116">V **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="4642b-116">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="4642b-117">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="4642b-117">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="4642b-118">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4642b-118">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="4642b-120">Po dokončení nástroj Sysprep vypne virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4642b-120">When Sysprep completes, it shuts down the virtual machine.</span></span> <span data-ttu-id="4642b-121">Virtuální počítač nerestartuje.</span><span class="sxs-lookup"><span data-stu-id="4642b-121">Do not restart the VM.</span></span>


## <a name="create-a-managed-image-in-the-portal"></a><span data-ttu-id="4642b-122">Vytvoření bitové kopie spravované na portálu</span><span class="sxs-lookup"><span data-stu-id="4642b-122">Create a managed image in the portal</span></span> 

1. <span data-ttu-id="4642b-123">Otevřete [portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4642b-123">Open the [portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4642b-124">Klikněte na symbol plus k vytvoření nového prostředku.</span><span class="sxs-lookup"><span data-stu-id="4642b-124">Click the plus sign to create a new resource.</span></span>
3. <span data-ttu-id="4642b-125">Filtr hledání, zadejte **Image**.</span><span class="sxs-lookup"><span data-stu-id="4642b-125">In the filter search, type **Image**.</span></span>
4. <span data-ttu-id="4642b-126">Vyberte **Image** z výsledků.</span><span class="sxs-lookup"><span data-stu-id="4642b-126">Select **Image** from the results.</span></span>
5. <span data-ttu-id="4642b-127">V **Image** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4642b-127">In the **Image** blade, click **Create**.</span></span>
6. <span data-ttu-id="4642b-128">V **název**, zadejte název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4642b-128">In **Name**, type a name for the image.</span></span>
7. <span data-ttu-id="4642b-129">Pokud máte více než jedno předplatné, vyberte tu správnou z **předplatné** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4642b-129">If you have more than one subscription, select the correct one from the **Subscription** drop-down.</span></span>
7. <span data-ttu-id="4642b-130">V **skupiny prostředků** vyberte buď **vytvořit nový** a zadejte název, nebo vyberte **z existujícího** a vyberte skupinu prostředků z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4642b-130">In **Resource Group** either select **Create new** and type in a name, or select **From existing** and select a resource group to use from the drop-down list.</span></span>
8. <span data-ttu-id="4642b-131">V **umístění**, vyberte umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="4642b-131">In **Location**, choose the location of your resource group.</span></span>
9. <span data-ttu-id="4642b-132">V **typ operačního systému** vyberte typ operačního systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4642b-132">In **OS type** select the type of operating system, either Windows or Linux.</span></span>
11. <span data-ttu-id="4642b-133">V **objektu blob Storage**, klikněte na tlačítko **Procházet** hledání virtuální pevný disk ve službě Azure storage.</span><span class="sxs-lookup"><span data-stu-id="4642b-133">In **Storage blob**, click **Browse** to look for the VHD in your Azure storage.</span></span>
12. <span data-ttu-id="4642b-134">V **typ účtu** zvolte Standard_LRS nebo Premium_LRS.</span><span class="sxs-lookup"><span data-stu-id="4642b-134">In **Account type** choose Standard_LRS or Premium_LRS.</span></span> <span data-ttu-id="4642b-135">Jednotky pevného disku používá standard a Premium jednotky SSD.</span><span class="sxs-lookup"><span data-stu-id="4642b-135">Standard uses hard-disk drives and Premium uses solid-state drives.</span></span> <span data-ttu-id="4642b-136">Jak používat místně redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4642b-136">Both use locally-redundant storage.</span></span>
13. <span data-ttu-id="4642b-137">V **ukládání do mezipaměti disku** vyberte příslušný disk možnost ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4642b-137">In **Disk caching** select the appropriate disk caching option.</span></span> <span data-ttu-id="4642b-138">Možnosti jsou **žádné**, **jen pro čtení** a **čtení/zápis**.</span><span class="sxs-lookup"><span data-stu-id="4642b-138">The options are **None**, **Read-only** and **Read\write**.</span></span>
14. <span data-ttu-id="4642b-139">Volitelné: Také přidáním stávající datový disk do bitové kopie kliknutím **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="4642b-139">Optional: You can also add an existing data disk to the image by clicking **+ Add data disk**.</span></span>  
15. <span data-ttu-id="4642b-140">Po dokončení provedení výběru klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4642b-140">When you are done making your selections, click **Create**.</span></span>
16. <span data-ttu-id="4642b-141">Po vytvoření image se bude zobrazovat jako **Image** prostředku v seznamu prostředků ve skupině prostředků, který jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="4642b-141">After the image is created, you will see it as an **Image** resource in the list of resources in the resource group you chose.</span></span>



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a><span data-ttu-id="4642b-142">Vytvoření spravované image virtuálního počítače pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="4642b-142">Create a managed image of a VM using Powershell</span></span>

<span data-ttu-id="4642b-143">Vytvoření bitové kopie přímo z virtuálního počítače zajistí, že image obsahuje všechny disky, které jsou spojené s virtuálním Počítačem, včetně disku operačního systému a všech datových disků.</span><span class="sxs-lookup"><span data-stu-id="4642b-143">Creating an image directly from the VM ensures that the image includes all of the disks associated with the VM, including the OS Disk and any data disks.</span></span>


<span data-ttu-id="4642b-144">Než začnete, ujistěte se, že máte nejnovější verzi modulu AzureRM.Compute prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4642b-144">Before you begin, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="4642b-145">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="4642b-145">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="4642b-146">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4642b-146">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


1. <span data-ttu-id="4642b-147">Vytvořte některé proměnné.</span><span class="sxs-lookup"><span data-stu-id="4642b-147">Create some variables.</span></span>

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. <span data-ttu-id="4642b-148">Zajistěte, aby že bylo zrušeno přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4642b-148">Make sure the VM has been deallocated.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="4642b-149">Nastavte stav virtuálního počítače na **zobecněno**.</span><span class="sxs-lookup"><span data-stu-id="4642b-149">Set the status of the virtual machine to **Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. <span data-ttu-id="4642b-150">Získáte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4642b-150">Get the virtual machine.</span></span> 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. <span data-ttu-id="4642b-151">Vytvořte konfiguraci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4642b-151">Create the image configuration.</span></span>

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. <span data-ttu-id="4642b-152">Vytvoření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4642b-152">Create the image.</span></span>

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a><span data-ttu-id="4642b-153">Vytvoříte spravované bitovou kopii virtuálního pevného disku v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4642b-153">Create a managed image of a VHD in PowerShell</span></span>

<span data-ttu-id="4642b-154">Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4642b-154">Create a managed image using your generalized OS VHD.</span></span>


1.  <span data-ttu-id="4642b-155">Nastavte nejprve, běžné parametry:</span><span class="sxs-lookup"><span data-stu-id="4642b-155">First, set the common parameters:</span></span>

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. <span data-ttu-id="4642b-156">Step\deallocate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4642b-156">Step\deallocate the VM.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="4642b-157">Virtuální počítač označte jako zobecněn.</span><span class="sxs-lookup"><span data-stu-id="4642b-157">Mark the VM as generalized.</span></span>

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  <span data-ttu-id="4642b-158">Vytvořte bitovou kopii pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4642b-158">Create the image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a><span data-ttu-id="4642b-159">Vytvoření bitové kopie spravované ze snímku pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="4642b-159">Create a managed image from a snapshot using Powershell</span></span>

<span data-ttu-id="4642b-160">Můžete také vytvořit bitovou kopii spravované z snímek virtuálního pevného disku z zobecněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4642b-160">You can also create a managed image from a snapshot of the VHD from a generalized VM.</span></span>

    
1. <span data-ttu-id="4642b-161">Vytvořte některé proměnné.</span><span class="sxs-lookup"><span data-stu-id="4642b-161">Create some variables.</span></span> 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. <span data-ttu-id="4642b-162">Pořízení snímku.</span><span class="sxs-lookup"><span data-stu-id="4642b-162">Get the snapshot.</span></span>

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. <span data-ttu-id="4642b-163">Vytvořte konfiguraci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4642b-163">Create the image configuration.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. <span data-ttu-id="4642b-164">Vytvoření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4642b-164">Create the image.</span></span>

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a><span data-ttu-id="4642b-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4642b-165">Next steps</span></span>
- <span data-ttu-id="4642b-166">Teď můžete [vytvořte virtuální počítač z bitové kopie zobecněný spravované](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4642b-166">Now you can [create a VM from the generalized managed image](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>  

