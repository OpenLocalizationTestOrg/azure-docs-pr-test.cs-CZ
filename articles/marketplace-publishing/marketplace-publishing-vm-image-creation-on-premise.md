---
title: "Vytvoření bitové kopie virtuálního počítače místní pro Azure Marketplace | Microsoft Docs"
description: "Pochopení a provést kroky pro vytvoření image virtuálního počítače místní a nasadit do Azure Marketplace pro ostatní k nákupu."
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
ms.openlocfilehash: 8f6b9a9293dc149586e6e5fd55028170ea825b07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="e58a8-103">Vytvořte bitovou kopii virtuálního počítače místní pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="e58a8-103">Develop an on-premises virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="e58a8-104">Důrazně doporučujeme vývoji Azure virtuální pevné disky (VHD) přímo v cloudu pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="e58a8-104">We strongly recommend that you develop Azure virtual hard disks (VHDs) directly in the cloud by using Remote Desktop Protocol.</span></span> <span data-ttu-id="e58a8-105">Ale pokud je potřeba, je možné stáhnout virtuální pevný disk a vytvořte ho pomocí místní infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e58a8-105">However, if you must, it is possible to download a VHD and develop it by using on-premises infrastructure.</span></span>  

<span data-ttu-id="e58a8-106">Pro místní vývoj je nutné stáhnout operační systém virtuálního pevného disku vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e58a8-106">For on-premises development, you must download the operating system VHD of the created VM.</span></span> <span data-ttu-id="e58a8-107">Tyto kroky by proběhnout jako součást kroku 3.3, výše.</span><span class="sxs-lookup"><span data-stu-id="e58a8-107">These steps would take place as part of step 3.3, above.</span></span>  

## <a name="download-a-vhd-image"></a><span data-ttu-id="e58a8-108">Stažení bitové kopie virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e58a8-108">Download a VHD image</span></span>
### <a name="locate-a-blob-url"></a><span data-ttu-id="e58a8-109">Vyhledejte adresu URL objektu blob</span><span class="sxs-lookup"><span data-stu-id="e58a8-109">Locate a blob URL</span></span>
<span data-ttu-id="e58a8-110">Chcete-li stáhnout virtuální pevný disk, nejprve vyhledejte adresu URL objektu blob pro disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e58a8-110">In order to download the VHD, first locate the blob URL for the operating system disk.</span></span>

<span data-ttu-id="e58a8-111">Vyhledejte adresu URL objektu blob z nové [portálu Microsoft Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="e58a8-111">Locate the blob URL from the new [Microsoft Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="e58a8-112">Přejděte na **Procházet** > **virtuální počítače**a potom vyberte virtuální počítač nasazený.</span><span class="sxs-lookup"><span data-stu-id="e58a8-112">Go to **Browse** > **VMs**, and then select the deployed VM.</span></span>
2. <span data-ttu-id="e58a8-113">V části **konfigurace**, vyberte **disky** dlaždici, což otevře okno disky.</span><span class="sxs-lookup"><span data-stu-id="e58a8-113">Under **Configure**, select the **Disks** tile, which opens the Disks blade.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. <span data-ttu-id="e58a8-115">Vyberte **Disk s operačním systémem**, což otevře další okno, které zobrazuje vlastnosti disku, včetně umístění virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e58a8-115">Select the **OS Disk**, which opens another blade that displays disk properties, including the VHD location.</span></span>
4. <span data-ttu-id="e58a8-116">Zkopírujte tuto adresu URL objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e58a8-116">Copy this blob URL.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. <span data-ttu-id="e58a8-118">Nyní odstraňte nasazených virtuálních počítačů bez odstranění základní disky.</span><span class="sxs-lookup"><span data-stu-id="e58a8-118">Now, delete the deployed VM without deleting the backing disks.</span></span> <span data-ttu-id="e58a8-119">Můžete také zastavit virtuální počítač místo odstranění.</span><span class="sxs-lookup"><span data-stu-id="e58a8-119">You can also stop the VM instead of deleting it.</span></span> <span data-ttu-id="e58a8-120">Nestahovat operačního systému virtuálního pevného disku, když je virtuální počítač spuštěný.</span><span class="sxs-lookup"><span data-stu-id="e58a8-120">Do not download the operating system VHD when the VM is running.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a><span data-ttu-id="e58a8-122">Stažení virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e58a8-122">Download a VHD</span></span>
<span data-ttu-id="e58a8-123">Po zjištění adresy URL objektu blob, si můžete stáhnout virtuální pevný disk pomocí [portál Azure](http://manage.windowsazure.com/) nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e58a8-123">After you know the blob URL, you can download the VHD by using the [Azure portal](http://manage.windowsazure.com/) or PowerShell.</span></span>  

> [!NOTE]
> <span data-ttu-id="e58a8-124">V době vytvoření této příručce funkce pro stažení virtuální pevný disk ještě není součástí nového portálu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e58a8-124">At the time of this guide’s creation, the functionality to download a VHD is not yet present in the new Microsoft Azure portal.</span></span>  
> 
> 

<span data-ttu-id="e58a8-125">**Stáhnout operační systém virtuálního pevného disku prostřednictvím aktuální [portálu Azure](http://manage.windowsazure.com/)**</span><span class="sxs-lookup"><span data-stu-id="e58a8-125">**Download the operating system VHD via the current [Azure portal](http://manage.windowsazure.com/)**</span></span>

1. <span data-ttu-id="e58a8-126">Pokud jste tak již neučinili, přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e58a8-126">Sign in to the Azure portal if you have not done so already.</span></span>
2. <span data-ttu-id="e58a8-127">Klikněte **úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="e58a8-127">Click the **Storage** tab.</span></span>
3. <span data-ttu-id="e58a8-128">Vyberte účet úložiště, ve kterém je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e58a8-128">Select the storage account within which the VHD is stored.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. <span data-ttu-id="e58a8-130">Zobrazí vlastnosti účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e58a8-130">This displays storage account properties.</span></span> <span data-ttu-id="e58a8-131">Vyberte **kontejnery** kartě.</span><span class="sxs-lookup"><span data-stu-id="e58a8-131">Select the **Containers** tab.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. <span data-ttu-id="e58a8-133">Vyberte kontejner, ve kterém je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e58a8-133">Select the container in which the VHD is stored.</span></span> <span data-ttu-id="e58a8-134">Ve výchozím nastavení když vytvořená na portálu, virtuální pevný disk uložený v kontejner virtuálních pevných disků.</span><span class="sxs-lookup"><span data-stu-id="e58a8-134">By default, when created from the portal, the VHD is stored in a vhds container.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. <span data-ttu-id="e58a8-136">Vyberte správný operační systém virtuálního pevného disku tak, že porovnáte adresu URL, které jste uložili.</span><span class="sxs-lookup"><span data-stu-id="e58a8-136">Select the correct operating system VHD by comparing the URL to the one you saved.</span></span>
7. <span data-ttu-id="e58a8-137">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="e58a8-137">Click **Download**.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a><span data-ttu-id="e58a8-139">Stáhnout virtuální pevný disk pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e58a8-139">Download a VHD by using PowerShell</span></span>
<span data-ttu-id="e58a8-140">Kromě pomocí portálu Azure, můžete použít [uložit AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) rutiny stáhnout operační systém virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e58a8-140">In addition to using the Azure portal, you can use the [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet to download the operating system VHD.</span></span>

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
<span data-ttu-id="e58a8-141">Například uložit AzureVhd-zdroje "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - klíč úložiště<String></span><span class="sxs-lookup"><span data-stu-id="e58a8-141">For example, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String></span></span>

> [!NOTE]
> <span data-ttu-id="e58a8-142">**Uložit AzureVhd** má také **NumberOfThreads** možnost, která můžete použít ke zvýšení paralelismus za účelem co nejlepší využití dostupné šířky pásma pro stažení.</span><span class="sxs-lookup"><span data-stu-id="e58a8-142">**Save-AzureVhd** also has a **NumberOfThreads** option that can be used to increase parallelism to make the best use of available bandwidth for the download.</span></span>
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a><span data-ttu-id="e58a8-143">Nahrání virtuálních pevných disků do účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="e58a8-143">Upload VHDs to an Azure storage account</span></span>
<span data-ttu-id="e58a8-144">Pokud jste připravili virtuální pevné disky místní, budete muset odešlete do účtu úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="e58a8-144">If you prepared your VHDs on-premises, you need to upload them into a storage account in Azure.</span></span> <span data-ttu-id="e58a8-145">Tento krok se provádí po vytvoření svůj disk VHD místní ale před jeho získání certifikační pro bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e58a8-145">This step takes place after creating your VHD on-premises but before obtaining certification for your VM image.</span></span>

### <a name="create-a-storage-account-and-container"></a><span data-ttu-id="e58a8-146">Vytvoření účtu úložiště a kontejneru</span><span class="sxs-lookup"><span data-stu-id="e58a8-146">Create a storage account and container</span></span>
<span data-ttu-id="e58a8-147">Doporučujeme vám, že virtuální pevné disky se nahraje do účtu úložiště v oblasti ve Spojených státech amerických.</span><span class="sxs-lookup"><span data-stu-id="e58a8-147">We recommend that VHDs be uploaded into a storage account in a region in the United States.</span></span> <span data-ttu-id="e58a8-148">Všechny virtuální pevné disky pro jednu SKU musí být umístěny v jednom kontejneru v rámci účtu jedno úložiště.</span><span class="sxs-lookup"><span data-stu-id="e58a8-148">All VHDs for a single SKU should be placed in a single container within a single storage account.</span></span>

<span data-ttu-id="e58a8-149">Pokud chcete vytvořit účet úložiště, můžete použít [portálu Microsoft Azure](https://portal.azure.com/), prostředí PowerShell nebo sady Linux nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e58a8-149">To create a storage account, you can use the [Microsoft Azure portal](https://portal.azure.com/), PowerShell, or the Linux command-line tool.</span></span>  

<span data-ttu-id="e58a8-150">**Vytvořit účet úložiště z portálu Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="e58a8-150">**Create a storage account from the Microsoft Azure portal**</span></span>

1. <span data-ttu-id="e58a8-151">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="e58a8-151">Click **New**.</span></span>
2. <span data-ttu-id="e58a8-152">Vyberte **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e58a8-152">Select **Storage**.</span></span>
3. <span data-ttu-id="e58a8-153">Zadejte název účtu úložiště a pak vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="e58a8-153">Fill in the storage account name, and then select a location.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. <span data-ttu-id="e58a8-155">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e58a8-155">Click **Create**.</span></span>
5. <span data-ttu-id="e58a8-156">V okně pro účet vytvořený úložiště by se mělo otevřít.</span><span class="sxs-lookup"><span data-stu-id="e58a8-156">The blade for the created storage account should be open.</span></span> <span data-ttu-id="e58a8-157">Pokud ne, vyberte **Procházet** > **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e58a8-157">If not, select **Browse** > **Storage Accounts**.</span></span> <span data-ttu-id="e58a8-158">V okně účtu úložiště vyberte účet úložiště vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e58a8-158">On the Storage account blade, select the storage account created.</span></span>
6. <span data-ttu-id="e58a8-159">Vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="e58a8-159">Select **Containers**.</span></span>
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. <span data-ttu-id="e58a8-161">V okně kontejnery vyberte **přidat**a pak zadejte název kontejneru a kontejneru oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e58a8-161">On the Containers blade, select **Add**, and then enter a container name and the container permissions.</span></span> <span data-ttu-id="e58a8-162">Vyberte **privátní** kontejneru oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e58a8-162">Select **Private** for container permissions.</span></span>

> [!TIP]
> <span data-ttu-id="e58a8-163">Doporučujeme, abyste vytvořili jednoho kontejneru typu jedné SKU, které chcete publikovat.</span><span class="sxs-lookup"><span data-stu-id="e58a8-163">We recommend that you create one container per SKU that you are planning to publish.</span></span>
> 
> 

  ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a><span data-ttu-id="e58a8-165">Vytvořit účet úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e58a8-165">Create a storage account by using PowerShell</span></span>
<span data-ttu-id="e58a8-166">Pomocí prostředí PowerShell vytvořit účet úložiště pomocí [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e58a8-166">Using PowerShell, create a storage account by using the [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span></span>

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

<span data-ttu-id="e58a8-167">Potom můžete vytvořit kontejner v rámci tohoto účtu úložiště pomocí [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e58a8-167">Then you can create a container within that storage account by using the [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span></span>

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> <span data-ttu-id="e58a8-168">Těchto příkazů se předpokládá, že aktuální kontext účtu úložiště již byla nastavena v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e58a8-168">Those commands assume that the current storage account context has already been set in PowerShell.</span></span>   <span data-ttu-id="e58a8-169">Odkazovat na [nastavení prostředí Azure PowerShell](marketplace-publishing-powershell-setup.md) Další informace o instalaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e58a8-169">Refer to [Setting up Azure PowerShell](marketplace-publishing-powershell-setup.md) for more details on PowerShell setup.</span></span>  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="e58a8-170">Vytvořit účet úložiště pomocí nástroje příkazového řádku pro Mac a Linux</span><span class="sxs-lookup"><span data-stu-id="e58a8-170">Create a storage account by using the command-line tool for Mac and Linux</span></span>
> <span data-ttu-id="e58a8-171">Z [nástroj příkazového řádku Linux](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), vytvoření účtu úložiště následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e58a8-171">From [Linux command-line tool](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), create a storage account as follows.</span></span>
> 
> 

        azure storage account create mystorageaccount --location "West US"

<span data-ttu-id="e58a8-172">Vytvořte kontejner následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e58a8-172">Create a container as follows.</span></span>

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a><span data-ttu-id="e58a8-173">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e58a8-173">Upload a VHD</span></span>
<span data-ttu-id="e58a8-174">Po vytvoření účtu úložiště a kontejneru, můžete nahrát připravit virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="e58a8-174">After the storage account and container are created, you can upload your prepared VHDs.</span></span> <span data-ttu-id="e58a8-175">Můžete použít PowerShell, nástroje příkazového řádku systému Linux nebo jiné nástroje pro správu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e58a8-175">You can use PowerShell, the Linux command-line tool, or other Azure Storage management tools.</span></span>

### <a name="upload-a-vhd-via-powershell"></a><span data-ttu-id="e58a8-176">Nahrání virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e58a8-176">Upload a VHD via PowerShell</span></span>
<span data-ttu-id="e58a8-177">Použití [přidat AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e58a8-177">Use the [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span></span>

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="e58a8-178">Nahrát VHD pomocí nástroje příkazového řádku pro Mac a Linux</span><span class="sxs-lookup"><span data-stu-id="e58a8-178">Upload a VHD by using the command-line tool for Mac and Linux</span></span>
<span data-ttu-id="e58a8-179">S [nástroj příkazového řádku Linux](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), použijte následující: vytvoření bitové kopie virtuálního počítače azure <image name> – umístění <Location of the data center> – operačního systému Linux<LocationOfLocalVHD></span><span class="sxs-lookup"><span data-stu-id="e58a8-179">With the [Linux command-line tool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), use the following: azure vm image create <image name> --location <Location of the data center> --OS Linux <LocationOfLocalVHD></span></span>

## <a name="see-also"></a><span data-ttu-id="e58a8-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="e58a8-180">See also</span></span>
* [<span data-ttu-id="e58a8-181">Vytváření bitové kopie virtuálního počítače pro Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e58a8-181">Creating a virtual machine image for the Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)
* [<span data-ttu-id="e58a8-182">Nastavení prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e58a8-182">Setting up Azure PowerShell</span></span>](marketplace-publishing-powershell-setup.md)

