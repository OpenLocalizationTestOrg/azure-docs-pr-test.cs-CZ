---
title: "aaaCreate a nahrání virtuálního počítače bitové kopie pomocí prostředí Powershell | Microsoft Docs"
description: "Další informace toocreate a nahrajte zobecněný Windows serverovou bitovou kopii (VHD) pomocí modelu nasazení classic hello a prostředí Azure Powershell."
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
ms.openlocfilehash: 093b57c9157cea5f348c8ba02b5700c917adbcdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-windows-server-vhd-tooazure"></a><span data-ttu-id="abf72-103">Vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure</span><span class="sxs-lookup"><span data-stu-id="abf72-103">Create and upload a Windows Server VHD tooAzure</span></span>
<span data-ttu-id="abf72-104">Tento článek ukazuje, jak tooupload zobecněný virtuální počítač bitové kopie jako virtuální pevný disk (VHD), můžete ho toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="abf72-104">This article shows you how tooupload your own generalized VM image as a virtual hard disk (VHD) so you can use it toocreate virtual machines.</span></span> <span data-ttu-id="abf72-105">Další podrobnosti o disky a virtuální pevné disky ve službě Microsoft Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="abf72-105">For more details about disks and VHDs in Microsoft Azure, see [About Disks and VHDs for Virtual Machines](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abf72-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="abf72-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="abf72-107">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="abf72-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="abf72-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="abf72-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="abf72-109">Můžete také [nahrát](../upload-generalized-managed.md) virtuálního počítače pomocí modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="abf72-109">You can also [upload](../upload-generalized-managed.md) a virtual machine using hello Resource Manager model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abf72-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="abf72-110">Prerequisites</span></span>
<span data-ttu-id="abf72-111">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="abf72-111">This article assumes you have:</span></span>

* <span data-ttu-id="abf72-112">**Předplatné Azure** – pokud ji nemáte, můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="abf72-112">**An Azure subscription** - If you don't have one, you can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
* <span data-ttu-id="abf72-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)**  -máte hello Microsoft Azure PowerShell modul nainstalovaný a nakonfigurovaný toouse vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="abf72-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)** - You have hello Microsoft Azure PowerShell module installed and configured toouse your subscription.</span></span>
* <span data-ttu-id="abf72-114">**A. Soubor VHD** – podporované Windows operačního systému uložené v souboru VHD a připojené tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abf72-114">**A .VHD file** - supported Windows operating system stored in a .vhd file and attached tooa virtual machine.</span></span> <span data-ttu-id="abf72-115">Toosee zkontrolujte, zda role serveru hello systémem hello virtuálního pevného disku jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="abf72-115">Check toosee if hello server roles running on hello VHD are supported by Sysprep.</span></span> <span data-ttu-id="abf72-116">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="abf72-116">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="abf72-117">Formát VHDX Hello není podporovaný v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="abf72-117">hello VHDX format is not supported in Microsoft Azure.</span></span> <span data-ttu-id="abf72-118">Můžete převést na formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello [rutiny Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="abf72-118">You can convert hello disk tooVHD format using Hyper-V Manager or hello [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="abf72-119">Podrobnosti najdete v tématu to [blogový příspěvek](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span><span class="sxs-lookup"><span data-stu-id="abf72-119">For details, see this [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span></span>

## <a name="step-1-prep-hello-vhd"></a><span data-ttu-id="abf72-120">Krok 1: Příprava aplikace hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="abf72-120">Step 1: Prep hello VHD</span></span>
<span data-ttu-id="abf72-121">Před nahráním tooAzure hello virtuální pevný disk, musí toobe zobecněn pomocí nástroje Sysprep hello.</span><span class="sxs-lookup"><span data-stu-id="abf72-121">Before you upload hello VHD tooAzure, it needs toobe generalized by using hello Sysprep tool.</span></span> <span data-ttu-id="abf72-122">To připraví toobe hello virtuálního pevného disku použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="abf72-122">This prepares hello VHD toobe used as an image.</span></span> <span data-ttu-id="abf72-123">Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="abf72-123">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span> <span data-ttu-id="abf72-124">Zálohujte hello virtuálních počítačů před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="abf72-124">Back up hello VM before running Sysprep.</span></span>

<span data-ttu-id="abf72-125">Z hello virtuální počítač, který hello operačního systému je nainstalovaná, aby dokončení hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="abf72-125">From hello virtual machine that hello operating system was installed to, complete hello following procedure:</span></span>

1. <span data-ttu-id="abf72-126">Přihlaste se toohello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="abf72-126">Sign in toohello operating system.</span></span>
2. <span data-ttu-id="abf72-127">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="abf72-127">Open a command prompt window as an administrator.</span></span> <span data-ttu-id="abf72-128">Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="abf72-128">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>

    ![Otevřete okno příkazového řádku](./media/createupload-vhd/sysprep_commandprompt.png)
3. <span data-ttu-id="abf72-130">Hello **nástroj pro přípravu systému** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="abf72-130">hello **System Preparation Tool** dialog box appears.</span></span>

   ![Spusťte nástroj Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. <span data-ttu-id="abf72-132">V hello **nástroj pro přípravu systému**, vyberte **zadejte systému mimo pole prostředí Spouštěného** a ujistěte se, že **generalizace** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="abf72-132">In hello **System Preparation Tool**, select **Enter System Out of Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span>
5. <span data-ttu-id="abf72-133">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="abf72-133">In **Shutdown Options**, select **Shutdown**.</span></span>
6. <span data-ttu-id="abf72-134">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="abf72-134">Click **OK**.</span></span>

## <a name="step-2-create-a-storage-account-and-a-container"></a><span data-ttu-id="abf72-135">Krok 2: Vytvoření účtu úložiště a kontejner</span><span class="sxs-lookup"><span data-stu-id="abf72-135">Step 2: Create a storage account and a container</span></span>
<span data-ttu-id="abf72-136">Účet úložiště v Azure je nutné, abyste získali soubor VHD místní tooupload hello.</span><span class="sxs-lookup"><span data-stu-id="abf72-136">You need a storage account in Azure so you have a place tooupload hello .vhd file.</span></span> <span data-ttu-id="abf72-137">Tento krok ukazuje, jak toocreate účet nebo get hello informace je třeba z existující účet.</span><span class="sxs-lookup"><span data-stu-id="abf72-137">This step shows you how toocreate an account, or get hello info you need from an existing account.</span></span> <span data-ttu-id="abf72-138">Nahraďte hello proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="abf72-138">Replace hello variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

1. <span data-ttu-id="abf72-139">Přihlásit</span><span class="sxs-lookup"><span data-stu-id="abf72-139">Login</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="abf72-140">Nastavte vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="abf72-140">Set your Azure subscription.</span></span>

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. <span data-ttu-id="abf72-141">Vytvořte nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="abf72-141">Create a new storage account.</span></span> <span data-ttu-id="abf72-142">název účtu úložiště hello Hello by měl být jedinečný, 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="abf72-142">hello name of hello storage account should be unique, 3-24 characters.</span></span> <span data-ttu-id="abf72-143">Název Hello může být jakoukoli kombinací písmen a číslic.</span><span class="sxs-lookup"><span data-stu-id="abf72-143">hello name can be any combination of letters and numbers.</span></span> <span data-ttu-id="abf72-144">Musíte taky toospecify umístění jako "Východ USA"</span><span class="sxs-lookup"><span data-stu-id="abf72-144">You also need toospecify a location like "East US"</span></span>

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. <span data-ttu-id="abf72-145">Nastavit jako výchozí hello hello nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="abf72-145">Set hello new storage account as hello default.</span></span>

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. <span data-ttu-id="abf72-146">Vytvořte nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="abf72-146">Create a new container.</span></span>

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-hello-vhd-file"></a><span data-ttu-id="abf72-147">Krok 3: Odeslání souboru VHD hello</span><span class="sxs-lookup"><span data-stu-id="abf72-147">Step 3: Upload hello .vhd file</span></span>
<span data-ttu-id="abf72-148">Použití hello [přidat AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="abf72-148">Use hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD.</span></span>

<span data-ttu-id="abf72-149">Z jste použili v předchozím kroku hello okno Azure PowerShell text hello, typ hello následující příkaz a nahraďte hello proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="abf72-149">From hello Azure PowerShell window you used in hello previous step, type hello following command and replace hello variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-hello-image-tooyour-list-of-custom-images"></a><span data-ttu-id="abf72-150">Krok 4: Přidejte hello image tooyour seznamu vlastních bitových kopií</span><span class="sxs-lookup"><span data-stu-id="abf72-150">Step 4: Add hello image tooyour list of custom images</span></span>
<span data-ttu-id="abf72-151">Použití hello [přidat AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) rutiny tooadd hello image toohello seznam vlastních bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="abf72-151">Use hello [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet tooadd hello image toohello list of your custom images.</span></span>

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a><span data-ttu-id="abf72-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abf72-152">Next steps</span></span>
<span data-ttu-id="abf72-153">Teď můžete [vytvořit vlastní virtuální](createportal.md) pomocí hello image nahrát.</span><span class="sxs-lookup"><span data-stu-id="abf72-153">You can now [create a custom VM](createportal.md) using hello image you uploaded.</span></span>
