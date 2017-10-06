---
title: "aaaCreating obrázek místní virtuální počítač pro hello Azure Marketplace | Microsoft Docs"
description: "Pochopit a provedení kroků toocreate hello bitovou kopii virtuálního počítače místní a nasadit toohello Azure Marketplace pro ostatní toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c7a265330f1e494db8d0e981a38ee00d85746bb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="fe8b0-103">Vývoj bitové kopie virtuálního počítače místní pro hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="fe8b0-103">Develop an on-premises virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="fe8b0-104">Důrazně doporučujeme vývoj Azure virtuální pevné disky (VHD) přímo v cloudu hello pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-104">We strongly recommend that you develop Azure virtual hard disks (VHDs) directly in hello cloud by using Remote Desktop Protocol.</span></span> <span data-ttu-id="fe8b0-105">Pokud je to nutné, ale je možné toodownload virtuální pevný disk a vývoj pomocí místní infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-105">However, if you must, it is possible toodownload a VHD and develop it by using on-premises infrastructure.</span></span>  

<span data-ttu-id="fe8b0-106">Pro místní vývoj, je nutné stáhnout hello operačního systému hello vytvořit virtuální pevný disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-106">For on-premises development, you must download hello operating system VHD of hello created VM.</span></span> <span data-ttu-id="fe8b0-107">Tyto kroky by proběhnout jako součást kroku 3.3, výše.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-107">These steps would take place as part of step 3.3, above.</span></span>  

## <a name="download-a-vhd-image"></a><span data-ttu-id="fe8b0-108">Stažení bitové kopie virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="fe8b0-108">Download a VHD image</span></span>
### <a name="locate-a-blob-url"></a><span data-ttu-id="fe8b0-109">Vyhledejte adresu URL objektu blob</span><span class="sxs-lookup"><span data-stu-id="fe8b0-109">Locate a blob URL</span></span>
<span data-ttu-id="fe8b0-110">V pořadí toodownload hello virtuálního pevného disku nejprve vyhledejte adresy URL objektu blob hello hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-110">In order toodownload hello VHD, first locate hello blob URL for hello operating system disk.</span></span>

<span data-ttu-id="fe8b0-111">Vyhledejte nové adresy URL objektu blob hello z hello [portálu Microsoft Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="fe8b0-111">Locate hello blob URL from hello new [Microsoft Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="fe8b0-112">Přejděte příliš**Procházet** > **virtuální počítače**, a pak vyberte hello nasazení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-112">Go too**Browse** > **VMs**, and then select hello deployed VM.</span></span>
2. <span data-ttu-id="fe8b0-113">V části **konfigurace**, vyberte hello **disky** dlaždici, což otevře okno disky hello.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-113">Under **Configure**, select hello **Disks** tile, which opens hello Disks blade.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. <span data-ttu-id="fe8b0-115">Vyberte hello **Disk s operačním systémem**, což otevře další okno, které zobrazuje vlastnosti disku, včetně umístění virtuálního pevného disku hello.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-115">Select hello **OS Disk**, which opens another blade that displays disk properties, including hello VHD location.</span></span>
4. <span data-ttu-id="fe8b0-116">Zkopírujte tuto adresu URL objektu blob.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-116">Copy this blob URL.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. <span data-ttu-id="fe8b0-118">Nyní, odstranit hello nasazení virtuálních počítačů bez odstranění hello základní disky.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-118">Now, delete hello deployed VM without deleting hello backing disks.</span></span> <span data-ttu-id="fe8b0-119">Neodstraňovat, ale můžete také zastavit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-119">You can also stop hello VM instead of deleting it.</span></span> <span data-ttu-id="fe8b0-120">Nestahovat hello operačního systému virtuálního pevného disku, když běží hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-120">Do not download hello operating system VHD when hello VM is running.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a><span data-ttu-id="fe8b0-122">Stažení virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="fe8b0-122">Download a VHD</span></span>
<span data-ttu-id="fe8b0-123">Po zjištění adresy URL objektu blob hello si můžete stáhnout hello virtuální pevný disk pomocí hello [portál Azure](http://manage.windowsazure.com/) nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-123">After you know hello blob URL, you can download hello VHD by using hello [Azure portal](http://manage.windowsazure.com/) or PowerShell.</span></span>  

> [!NOTE]
> <span data-ttu-id="fe8b0-124">V době hello Tato příručka vytvoření hello funkce toodownload virtuální pevný disk ještě není součástí hello nového portálu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-124">At hello time of this guide’s creation, hello functionality toodownload a VHD is not yet present in hello new Microsoft Azure portal.</span></span>  
> 
> 

<span data-ttu-id="fe8b0-125">**Stáhnout hello operačního systému virtuálního pevného disku prostřednictvím hello aktuální [portálu Azure](http://manage.windowsazure.com/)**</span><span class="sxs-lookup"><span data-stu-id="fe8b0-125">**Download hello operating system VHD via hello current [Azure portal](http://manage.windowsazure.com/)**</span></span>

1. <span data-ttu-id="fe8b0-126">Pokud jste tak již neučinili, přihlaste toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-126">Sign in toohello Azure portal if you have not done so already.</span></span>
2. <span data-ttu-id="fe8b0-127">Klikněte na tlačítko hello **úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-127">Click hello **Storage** tab.</span></span>
3. <span data-ttu-id="fe8b0-128">Vyberte účet úložiště hello v rámci které hello je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-128">Select hello storage account within which hello VHD is stored.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. <span data-ttu-id="fe8b0-130">Zobrazí vlastnosti účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-130">This displays storage account properties.</span></span> <span data-ttu-id="fe8b0-131">Vyberte hello **kontejnery** kartě.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-131">Select hello **Containers** tab.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. <span data-ttu-id="fe8b0-133">Vyberte hello kontejner, ve které hello se k uložení virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-133">Select hello container in which hello VHD is stored.</span></span> <span data-ttu-id="fe8b0-134">Ve výchozím nastavení při vytváření z portálu hello hello virtuální pevný disk uložený v kontejner virtuálních pevných disků.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-134">By default, when created from hello portal, hello VHD is stored in a vhds container.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. <span data-ttu-id="fe8b0-136">Vyberte hello správný operační systém virtuálního pevného disku tak, že porovnáte toohello hello adresu URL, jeden, který jste uložili.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-136">Select hello correct operating system VHD by comparing hello URL toohello one you saved.</span></span>
7. <span data-ttu-id="fe8b0-137">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-137">Click **Download**.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a><span data-ttu-id="fe8b0-139">Stáhnout virtuální pevný disk pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe8b0-139">Download a VHD by using PowerShell</span></span>
<span data-ttu-id="fe8b0-140">Kromě toho toousing hello portálu Azure, můžete použít hello [uložit AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) rutiny toodownload hello operačního systému virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-140">In addition toousing hello Azure portal, you can use hello [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet toodownload hello operating system VHD.</span></span>

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
<span data-ttu-id="fe8b0-141">Například uložit AzureVhd-zdroje "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - klíč úložiště<String></span><span class="sxs-lookup"><span data-stu-id="fe8b0-141">For example, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String></span></span>

> [!NOTE]
> <span data-ttu-id="fe8b0-142">**Uložit AzureVhd** má také **NumberOfThreads** možnost, která může být použit tooincrease paralelismus toomake hello nejlepší využití šířky pásma k dispozici ke stažení hello.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-142">**Save-AzureVhd** also has a **NumberOfThreads** option that can be used tooincrease parallelism toomake hello best use of available bandwidth for hello download.</span></span>
> 
> 

## <a name="upload-vhds-tooan-azure-storage-account"></a><span data-ttu-id="fe8b0-143">Nahrát účtu úložiště Azure tooan virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="fe8b0-143">Upload VHDs tooan Azure storage account</span></span>
<span data-ttu-id="fe8b0-144">Pokud jste připravili virtuální pevné disky místní, musíte tooupload je do úložiště účtu v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-144">If you prepared your VHDs on-premises, you need tooupload them into a storage account in Azure.</span></span> <span data-ttu-id="fe8b0-145">Tento krok se provádí po vytvoření svůj disk VHD místní ale před jeho získání certifikační pro bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-145">This step takes place after creating your VHD on-premises but before obtaining certification for your VM image.</span></span>

### <a name="create-a-storage-account-and-container"></a><span data-ttu-id="fe8b0-146">Vytvoření účtu úložiště a kontejneru</span><span class="sxs-lookup"><span data-stu-id="fe8b0-146">Create a storage account and container</span></span>
<span data-ttu-id="fe8b0-147">Doporučujeme vám, že virtuální pevné disky se nahraje do účtu úložiště v oblasti ve Spojených státech amerických hello.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-147">We recommend that VHDs be uploaded into a storage account in a region in hello United States.</span></span> <span data-ttu-id="fe8b0-148">Všechny virtuální pevné disky pro jednu SKU musí být umístěny v jednom kontejneru v rámci účtu jedno úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-148">All VHDs for a single SKU should be placed in a single container within a single storage account.</span></span>

<span data-ttu-id="fe8b0-149">toocreate účet úložiště, můžete použít hello [portálu Microsoft Azure](https://portal.azure.com/), prostředí PowerShell nebo nástroj příkazového řádku Linux hello.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-149">toocreate a storage account, you can use hello [Microsoft Azure portal](https://portal.azure.com/), PowerShell, or hello Linux command-line tool.</span></span>  

<span data-ttu-id="fe8b0-150">**Vytvořit účet úložiště z portálu Microsoft Azure hello**</span><span class="sxs-lookup"><span data-stu-id="fe8b0-150">**Create a storage account from hello Microsoft Azure portal**</span></span>

1. <span data-ttu-id="fe8b0-151">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-151">Click **New**.</span></span>
2. <span data-ttu-id="fe8b0-152">Vyberte **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-152">Select **Storage**.</span></span>
3. <span data-ttu-id="fe8b0-153">Zadejte název účtu úložiště hello a pak vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-153">Fill in hello storage account name, and then select a location.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. <span data-ttu-id="fe8b0-155">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-155">Click **Create**.</span></span>
5. <span data-ttu-id="fe8b0-156">by se mělo otevřít okno Hello hello vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-156">hello blade for hello created storage account should be open.</span></span> <span data-ttu-id="fe8b0-157">Pokud ne, vyberte **Procházet** > **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-157">If not, select **Browse** > **Storage Accounts**.</span></span> <span data-ttu-id="fe8b0-158">Na hello úložiště účet okně, vyberte účet úložiště hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-158">On hello Storage account blade, select hello storage account created.</span></span>
6. <span data-ttu-id="fe8b0-159">Vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-159">Select **Containers**.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. <span data-ttu-id="fe8b0-161">V okně hello kontejnery, vyberte **přidat**a potom zadejte oprávnění pro kontejner název a hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-161">On hello Containers blade, select **Add**, and then enter a container name and hello container permissions.</span></span> <span data-ttu-id="fe8b0-162">Vyberte **privátní** kontejneru oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-162">Select **Private** for container permissions.</span></span>

> [!TIP]
> <span data-ttu-id="fe8b0-163">Doporučujeme, abyste vytvořili jednoho kontejneru typu jedné plánování toopublish SKU.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-163">We recommend that you create one container per SKU that you are planning toopublish.</span></span>
> 
> 

  ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a><span data-ttu-id="fe8b0-165">Vytvořit účet úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe8b0-165">Create a storage account by using PowerShell</span></span>
<span data-ttu-id="fe8b0-166">Pomocí prostředí PowerShell vytvořit účet úložiště pomocí hello [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-166">Using PowerShell, create a storage account by using hello [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span></span>

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

<span data-ttu-id="fe8b0-167">Potom můžete vytvořit kontejner v rámci tohoto účtu úložiště pomocí hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-167">Then you can create a container within that storage account by using hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span></span>

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> <span data-ttu-id="fe8b0-168">Těchto příkazů se předpokládá, že tento kontext účtu úložiště aktuální hello již byla nastavena v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-168">Those commands assume that hello current storage account context has already been set in PowerShell.</span></span>   <span data-ttu-id="fe8b0-169">Odkazovat příliš[nastavení prostředí Azure PowerShell](marketplace-publishing-powershell-setup.md) Další informace o instalaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-169">Refer too[Setting up Azure PowerShell](marketplace-publishing-powershell-setup.md) for more details on PowerShell setup.</span></span>  
> 
> ### <a name="create-a-storage-account-by-using-hello-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="fe8b0-170">Vytvořit účet úložiště pomocí nástroje příkazového řádku hello pro Mac a Linux</span><span class="sxs-lookup"><span data-stu-id="fe8b0-170">Create a storage account by using hello command-line tool for Mac and Linux</span></span>
> <span data-ttu-id="fe8b0-171">Z [nástroj příkazového řádku Linux](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), vytvoření účtu úložiště následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-171">From [Linux command-line tool](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), create a storage account as follows.</span></span>
> 
> 

        azure storage account create mystorageaccount --location "West US"

<span data-ttu-id="fe8b0-172">Vytvořte kontejner následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-172">Create a container as follows.</span></span>

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a><span data-ttu-id="fe8b0-173">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="fe8b0-173">Upload a VHD</span></span>
<span data-ttu-id="fe8b0-174">Po vytvoření účtu úložiště hello a kontejneru, můžete nahrát připravit virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-174">After hello storage account and container are created, you can upload your prepared VHDs.</span></span> <span data-ttu-id="fe8b0-175">Můžete použít PowerShell, nástroje příkazového řádku hello Linux nebo jiné nástroje pro správu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-175">You can use PowerShell, hello Linux command-line tool, or other Azure Storage management tools.</span></span>

### <a name="upload-a-vhd-via-powershell"></a><span data-ttu-id="fe8b0-176">Nahrání virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe8b0-176">Upload a VHD via PowerShell</span></span>
<span data-ttu-id="fe8b0-177">Použití hello [přidat AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-177">Use hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span></span>

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-hello-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="fe8b0-178">Nahrát VHD pomocí nástroje příkazového řádku hello pro Mac a Linux</span><span class="sxs-lookup"><span data-stu-id="fe8b0-178">Upload a VHD by using hello command-line tool for Mac and Linux</span></span>
<span data-ttu-id="fe8b0-179">S hello [nástroj příkazového řádku Linux](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), použijte hello: vytvoření bitové kopie virtuálního počítače azure <image name> – umístění <Location of hello data center> – operačního systému Linux<LocationOfLocalVHD></span><span class="sxs-lookup"><span data-stu-id="fe8b0-179">With hello [Linux command-line tool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), use hello following: azure vm image create <image name> --location <Location of hello data center> --OS Linux <LocationOfLocalVHD></span></span>

## <a name="see-also"></a><span data-ttu-id="fe8b0-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="fe8b0-180">See also</span></span>
* [<span data-ttu-id="fe8b0-181">Vytváření bitové kopie virtuálního počítače pro hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fe8b0-181">Creating a virtual machine image for hello Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)
* [<span data-ttu-id="fe8b0-182">Nastavení prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe8b0-182">Setting up Azure PowerShell</span></span>](marketplace-publishing-powershell-setup.md)

