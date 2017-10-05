---
title: "Vytvoření a nahrání image virtuálního počítače pomocí prostředí Powershell | Microsoft Docs"
description: "Naučte se vytvořit a odeslat zobecněný Windows serverovou bitovou kopii (VHD) pomocí modelu nasazení classic a Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: bc75c8cdd98b0ea0fbff6483c0e3c9d4468d3941
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a><span data-ttu-id="0efb7-103">Vytvoření virtuálního pevného disku s Windows Serverem a jeho nahrání do Azure</span><span class="sxs-lookup"><span data-stu-id="0efb7-103">Create and upload a Windows Server VHD to Azure</span></span>
<span data-ttu-id="0efb7-104">Tento článek ukazuje, jak nahrát vlastní zobecněný image virtuálního počítače jako virtuální pevný disk (VHD), takže ho můžete použít k vytvoření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0efb7-104">This article shows you how to upload your own generalized VM image as a virtual hard disk (VHD) so you can use it to create virtual machines.</span></span> <span data-ttu-id="0efb7-105">Další podrobnosti o disky a virtuální pevné disky ve službě Microsoft Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0efb7-105">For more details about disks and VHDs in Microsoft Azure, see [About Disks and VHDs for Virtual Machines](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0efb7-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0efb7-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0efb7-107">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="0efb7-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="0efb7-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0efb7-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="0efb7-109">Můžete také [nahrát](../upload-generalized-managed.md) virtuálního počítače pomocí modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0efb7-109">You can also [upload](../upload-generalized-managed.md) a virtual machine using the Resource Manager model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0efb7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0efb7-110">Prerequisites</span></span>
<span data-ttu-id="0efb7-111">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="0efb7-111">This article assumes you have:</span></span>

* <span data-ttu-id="0efb7-112">**Předplatné Azure** – pokud ji nemáte, můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="0efb7-112">**An Azure subscription** - If you don't have one, you can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
* <span data-ttu-id="0efb7-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)**  -máte modulu Microsoft Azure PowerShell nainstalovaný a nakonfigurovaný na používání vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="0efb7-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)** - You have the Microsoft Azure PowerShell module installed and configured to use your subscription.</span></span>
* <span data-ttu-id="0efb7-114">**A. Soubor VHD** – podporované Windows operačního systému uložené v souboru VHD a připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="0efb7-114">**A .VHD file** - supported Windows operating system stored in a .vhd file and attached to a virtual machine.</span></span> <span data-ttu-id="0efb7-115">Zkontrolujte, jestli se role serveru, který je spuštěn na virtuální pevný disk podporovaný pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="0efb7-115">Check to see if the server roles running on the VHD are supported by Sysprep.</span></span> <span data-ttu-id="0efb7-116">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="0efb7-116">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0efb7-117">Formát VHDX není podporované v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0efb7-117">The VHDX format is not supported in Microsoft Azure.</span></span> <span data-ttu-id="0efb7-118">Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo [rutiny Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="0efb7-118">You can convert the disk to VHD format using Hyper-V Manager or the [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="0efb7-119">Podrobnosti najdete v tématu to [blogový příspěvek](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span><span class="sxs-lookup"><span data-stu-id="0efb7-119">For details, see this [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span></span>

## <a name="step-1-prep-the-vhd"></a><span data-ttu-id="0efb7-120">Krok 1: Příprava virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="0efb7-120">Step 1: Prep the VHD</span></span>
<span data-ttu-id="0efb7-121">Než budete nahrávat VHD do Azure, musí být zobecněn pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="0efb7-121">Before you upload the VHD to Azure, it needs to be generalized by using the Sysprep tool.</span></span> <span data-ttu-id="0efb7-122">To připraví virtuální pevný disk, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="0efb7-122">This prepares the VHD to be used as an image.</span></span> <span data-ttu-id="0efb7-123">Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="0efb7-123">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span> <span data-ttu-id="0efb7-124">Zálohování virtuálního počítače před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="0efb7-124">Back up the VM before running Sysprep.</span></span>

<span data-ttu-id="0efb7-125">Z virtuálního počítače, který byl pro nainstalovaný operační systém proveďte následující postup:</span><span class="sxs-lookup"><span data-stu-id="0efb7-125">From the virtual machine that the operating system was installed to, complete the following procedure:</span></span>

1. <span data-ttu-id="0efb7-126">Přihlaste se k operačnímu systému.</span><span class="sxs-lookup"><span data-stu-id="0efb7-126">Sign in to the operating system.</span></span>
2. <span data-ttu-id="0efb7-127">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="0efb7-127">Open a command prompt window as an administrator.</span></span> <span data-ttu-id="0efb7-128">Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="0efb7-128">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>

    ![Otevřete okno příkazového řádku](./media/createupload-vhd/sysprep_commandprompt.png)
3. <span data-ttu-id="0efb7-130">**Nástroj pro přípravu systému** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0efb7-130">The **System Preparation Tool** dialog box appears.</span></span>

   ![Spusťte nástroj Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. <span data-ttu-id="0efb7-132">V **nástroj pro přípravu systému**, vyberte **zadejte systému mimo pole prostředí Spouštěného** a ujistěte se, že **generalizace** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="0efb7-132">In the **System Preparation Tool**, select **Enter System Out of Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span>
5. <span data-ttu-id="0efb7-133">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="0efb7-133">In **Shutdown Options**, select **Shutdown**.</span></span>
6. <span data-ttu-id="0efb7-134">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0efb7-134">Click **OK**.</span></span>

## <a name="step-2-create-a-storage-account-and-a-container"></a><span data-ttu-id="0efb7-135">Krok 2: Vytvoření účtu úložiště a kontejner</span><span class="sxs-lookup"><span data-stu-id="0efb7-135">Step 2: Create a storage account and a container</span></span>
<span data-ttu-id="0efb7-136">Účet úložiště v Azure je nutné, abyste získali místo, kde můžete nahrát soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="0efb7-136">You need a storage account in Azure so you have a place to upload the .vhd file.</span></span> <span data-ttu-id="0efb7-137">Tento krok ukazuje, jak vytvořit účet, nebo získat informace, které je nutné z existující účet.</span><span class="sxs-lookup"><span data-stu-id="0efb7-137">This step shows you how to create an account, or get the info you need from an existing account.</span></span> <span data-ttu-id="0efb7-138">Nahrazení proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="0efb7-138">Replace the variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

1. <span data-ttu-id="0efb7-139">Přihlásit</span><span class="sxs-lookup"><span data-stu-id="0efb7-139">Login</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="0efb7-140">Nastavte vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="0efb7-140">Set your Azure subscription.</span></span>

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. <span data-ttu-id="0efb7-141">Vytvořte nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0efb7-141">Create a new storage account.</span></span> <span data-ttu-id="0efb7-142">Název účtu úložiště musí být jedinečný, 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="0efb7-142">The name of the storage account should be unique, 3-24 characters.</span></span> <span data-ttu-id="0efb7-143">Název může být libovolnou kombinací písmen a číslic.</span><span class="sxs-lookup"><span data-stu-id="0efb7-143">The name can be any combination of letters and numbers.</span></span> <span data-ttu-id="0efb7-144">Budete taky muset zadat umístění jako "Východ USA"</span><span class="sxs-lookup"><span data-stu-id="0efb7-144">You also need to specify a location like "East US"</span></span>

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. <span data-ttu-id="0efb7-145">Nový účet úložiště nastavte jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="0efb7-145">Set the new storage account as the default.</span></span>

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. <span data-ttu-id="0efb7-146">Vytvořte nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="0efb7-146">Create a new container.</span></span>

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a><span data-ttu-id="0efb7-147">Krok 3: Odeslání souboru VHD</span><span class="sxs-lookup"><span data-stu-id="0efb7-147">Step 3: Upload the .vhd file</span></span>
<span data-ttu-id="0efb7-148">Použití [přidat AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) odeslání disku VHD.</span><span class="sxs-lookup"><span data-stu-id="0efb7-148">Use the [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) to upload the VHD.</span></span>

<span data-ttu-id="0efb7-149">V okně Azure PowerShell jste použili v předchozím kroku, zadejte následující příkaz a nahraďte proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="0efb7-149">From the Azure PowerShell window you used in the previous step, type the following command and replace the variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a><span data-ttu-id="0efb7-150">Krok 4: Přidejte bitovou kopii do seznamu vlastních bitových kopií</span><span class="sxs-lookup"><span data-stu-id="0efb7-150">Step 4: Add the image to your list of custom images</span></span>
<span data-ttu-id="0efb7-151">Použití [přidat AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) rutiny přidat bitovou kopii do seznamu vlastních bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="0efb7-151">Use the [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet to add the image to the list of your custom images.</span></span>

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a><span data-ttu-id="0efb7-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0efb7-152">Next steps</span></span>
<span data-ttu-id="0efb7-153">Teď můžete [vytvořit vlastní virtuální](createportal.md) pomocí bitové kopie, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="0efb7-153">You can now [create a custom VM](createportal.md) using the image you uploaded.</span></span>
