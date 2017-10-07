---
title: "aaaCreate spravované bitové kopie v Azure | Microsoft Docs"
description: "Vytvořte bitovou kopii spravované zobecněný virtuální počítač nebo virtuální pevný disk v Azure. Bitové kopie může být použité toocreate víc virtuálních počítačů, které používají spravovaný disky."
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
ms.openlocfilehash: d8cd6c2ce8c5d704de2c845abced85139944d682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a><span data-ttu-id="1e483-104">Vytvořte bitovou kopii spravované zobecněný virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="1e483-104">Create a managed image of a generalized VM in Azure</span></span>

<span data-ttu-id="1e483-105">Z zobecněný virtuální počítač, který je uložený jako spravovaných disků nebo diskem nespravované v účtu úložiště můžete vytvořit prostředek spravované bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1e483-105">A managed image resource can be created from a generalized VM that is stored as either a managed disk or an unmanaged disk in a storage account.</span></span> <span data-ttu-id="1e483-106">Hello image může pak možné použít toocreate víc virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e483-106">hello image can then be used toocreate multiple VMs.</span></span> 


## <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="1e483-107">Generalize hello virtuální počítač s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="1e483-107">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="1e483-108">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="1e483-108">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="1e483-109">Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e483-109">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="1e483-110">Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="1e483-110">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="1e483-111">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="1e483-111">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e483-112">Pokud používáte nástroj Sysprep před nahráním vaší tooAzure virtuálního pevného disku pro hello poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="1e483-112">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="1e483-113">Přihlaste se toohello virtuálního počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="1e483-113">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="1e483-114">Otevřete okno příkazového řádku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="1e483-114">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="1e483-115">Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="1e483-115">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="1e483-116">V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="1e483-116">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="1e483-117">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="1e483-117">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="1e483-118">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e483-118">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="1e483-120">Po dokončení nástroj Sysprep vypne hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e483-120">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="1e483-121">Nerestartovat hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e483-121">Do not restart hello VM.</span></span>


## <a name="create-a-managed-image-in-hello-portal"></a><span data-ttu-id="1e483-122">Vytvoření bitové kopie spravované hello portálu</span><span class="sxs-lookup"><span data-stu-id="1e483-122">Create a managed image in hello portal</span></span> 

1. <span data-ttu-id="1e483-123">Otevřete hello [portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1e483-123">Open hello [portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1e483-124">Klikněte na tlačítko hello znaménko plus toocreate nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="1e483-124">Click hello plus sign toocreate a new resource.</span></span>
3. <span data-ttu-id="1e483-125">Hello filtru hledání, zadejte **Image**.</span><span class="sxs-lookup"><span data-stu-id="1e483-125">In hello filter search, type **Image**.</span></span>
4. <span data-ttu-id="1e483-126">Vyberte **Image** z výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="1e483-126">Select **Image** from hello results.</span></span>
5. <span data-ttu-id="1e483-127">V hello **Image** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e483-127">In hello **Image** blade, click **Create**.</span></span>
6. <span data-ttu-id="1e483-128">V **název**, zadejte název bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="1e483-128">In **Name**, type a name for hello image.</span></span>
7. <span data-ttu-id="1e483-129">Pokud máte více než jedno předplatné, vyberte hello správné jeden z hello **předplatné** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1e483-129">If you have more than one subscription, select hello correct one from hello **Subscription** drop-down.</span></span>
7. <span data-ttu-id="1e483-130">V **skupiny prostředků** vyberte buď **vytvořit nový** a zadejte název, nebo vyberte **z existujícího** a vyberte z rozevíracího seznamu hello toouse skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1e483-130">In **Resource Group** either select **Create new** and type in a name, or select **From existing** and select a resource group toouse from hello drop-down list.</span></span>
8. <span data-ttu-id="1e483-131">V **umístění**, vyberte umístění hello vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1e483-131">In **Location**, choose hello location of your resource group.</span></span>
9. <span data-ttu-id="1e483-132">V **typ operačního systému** vyberte hello typ operačního systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="1e483-132">In **OS type** select hello type of operating system, either Windows or Linux.</span></span>
11. <span data-ttu-id="1e483-133">V **objektu blob Storage**, klikněte na tlačítko **Procházet** toolook pro hello virtuální pevný disk ve službě Azure storage.</span><span class="sxs-lookup"><span data-stu-id="1e483-133">In **Storage blob**, click **Browse** toolook for hello VHD in your Azure storage.</span></span>
12. <span data-ttu-id="1e483-134">V **typ účtu** zvolte Standard_LRS nebo Premium_LRS.</span><span class="sxs-lookup"><span data-stu-id="1e483-134">In **Account type** choose Standard_LRS or Premium_LRS.</span></span> <span data-ttu-id="1e483-135">Jednotky pevného disku používá standard a Premium jednotky SSD.</span><span class="sxs-lookup"><span data-stu-id="1e483-135">Standard uses hard-disk drives and Premium uses solid-state drives.</span></span> <span data-ttu-id="1e483-136">Jak používat místně redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="1e483-136">Both use locally-redundant storage.</span></span>
13. <span data-ttu-id="1e483-137">V **ukládání do mezipaměti disku** vyberte hello příslušný disk možnost ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1e483-137">In **Disk caching** select hello appropriate disk caching option.</span></span> <span data-ttu-id="1e483-138">Možnosti Hello jsou **žádné**, **jen pro čtení** a **čtení/zápis**.</span><span class="sxs-lookup"><span data-stu-id="1e483-138">hello options are **None**, **Read-only** and **Read\write**.</span></span>
14. <span data-ttu-id="1e483-139">Volitelné: Můžete také přidat stávající image disku toohello dat kliknutím **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="1e483-139">Optional: You can also add an existing data disk toohello image by clicking **+ Add data disk**.</span></span>  
15. <span data-ttu-id="1e483-140">Po dokončení provedení výběru klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e483-140">When you are done making your selections, click **Create**.</span></span>
16. <span data-ttu-id="1e483-141">Po vytvoření bitové kopie hello se bude zobrazovat jako **Image** prostředku v seznamu hello prostředků ve skupině prostředků hello jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="1e483-141">After hello image is created, you will see it as an **Image** resource in hello list of resources in hello resource group you chose.</span></span>



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a><span data-ttu-id="1e483-142">Vytvoření spravované image virtuálního počítače pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="1e483-142">Create a managed image of a VM using Powershell</span></span>

<span data-ttu-id="1e483-143">Vytvoření bitové kopie přímo z virtuálních počítačů zajistí této bitové kopie hello hello zahrnuje všechny disky hello přidružené hello virtuálních počítačů, včetně hello Disk operačního systému a všech datových disků.</span><span class="sxs-lookup"><span data-stu-id="1e483-143">Creating an image directly from hello VM ensures that hello image includes all of hello disks associated with hello VM, including hello OS Disk and any data disks.</span></span>


<span data-ttu-id="1e483-144">Než začnete, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="1e483-144">Before you begin, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="1e483-145">Spustit hello následující příkaz tooinstall.</span><span class="sxs-lookup"><span data-stu-id="1e483-145">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="1e483-146">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1e483-146">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


1. <span data-ttu-id="1e483-147">Vytvořte některé proměnné.</span><span class="sxs-lookup"><span data-stu-id="1e483-147">Create some variables.</span></span>

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. <span data-ttu-id="1e483-148">Ujistěte se, zda text hello, které bylo zrušeno přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e483-148">Make sure hello VM has been deallocated.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="1e483-149">Nastavte stav hello hello virtuálního počítače příliš**zobecněno**.</span><span class="sxs-lookup"><span data-stu-id="1e483-149">Set hello status of hello virtual machine too**Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. <span data-ttu-id="1e483-150">Získáte hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1e483-150">Get hello virtual machine.</span></span> 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. <span data-ttu-id="1e483-151">Vytvořte konfiguraci hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1e483-151">Create hello image configuration.</span></span>

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. <span data-ttu-id="1e483-152">Vytvoření bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="1e483-152">Create hello image.</span></span>

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a><span data-ttu-id="1e483-153">Vytvoříte spravované bitovou kopii virtuálního pevného disku v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e483-153">Create a managed image of a VHD in PowerShell</span></span>

<span data-ttu-id="1e483-154">Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1e483-154">Create a managed image using your generalized OS VHD.</span></span>


1.  <span data-ttu-id="1e483-155">Nastavte nejprve, hello společné parametry:</span><span class="sxs-lookup"><span data-stu-id="1e483-155">First, set hello common parameters:</span></span>

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. <span data-ttu-id="1e483-156">Step\deallocate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e483-156">Step\deallocate hello VM.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="1e483-157">Označte hello virtuálního počítače jako zobecněn.</span><span class="sxs-lookup"><span data-stu-id="1e483-157">Mark hello VM as generalized.</span></span>

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  <span data-ttu-id="1e483-158">Vytvoření image hello pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1e483-158">Create hello image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a><span data-ttu-id="1e483-159">Vytvoření bitové kopie spravované ze snímku pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="1e483-159">Create a managed image from a snapshot using Powershell</span></span>

<span data-ttu-id="1e483-160">Můžete také vytvořit bitovou kopii spravované z snímek hello virtuálního pevného disku z zobecněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1e483-160">You can also create a managed image from a snapshot of hello VHD from a generalized VM.</span></span>

    
1. <span data-ttu-id="1e483-161">Vytvořte některé proměnné.</span><span class="sxs-lookup"><span data-stu-id="1e483-161">Create some variables.</span></span> 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. <span data-ttu-id="1e483-162">Získáte hello snímku.</span><span class="sxs-lookup"><span data-stu-id="1e483-162">Get hello snapshot.</span></span>

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. <span data-ttu-id="1e483-163">Vytvořte konfiguraci hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1e483-163">Create hello image configuration.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. <span data-ttu-id="1e483-164">Vytvoření bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="1e483-164">Create hello image.</span></span>

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a><span data-ttu-id="1e483-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e483-165">Next steps</span></span>
- <span data-ttu-id="1e483-166">Teď můžete [vytvořte virtuální počítač z bitové kopie spravované hello zobecněn](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e483-166">Now you can [create a VM from hello generalized managed image](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>    

