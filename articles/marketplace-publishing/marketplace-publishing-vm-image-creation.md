---
title: "Vytváření bitové kopie virtuálního počítače pro Azure Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvořit bitovou kopii virtuálního počítače pro Azure Marketplace pro ostatní k nákupu."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 046ce7af40301014746c6aef07d08d81ab4adcc2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="aaa7c-103">Průvodce pro vytvoření bitové kopie virtuálního počítače pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="aaa7c-103">Guide to create a virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="aaa7c-104">Tento článek **kroku 2**, vás provede procesem přípravy virtuálních pevných disků (VHD), které nasadíte do Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-104">This article, **Step 2**, walks you through preparing the virtual hard disks (VHDs) that you will deploy to the Azure Marketplace.</span></span> <span data-ttu-id="aaa7c-105">Virtuální pevné disky jsou základ pro vaše SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-105">Your VHDs are the foundation of your SKU.</span></span> <span data-ttu-id="aaa7c-106">Proces se liší v závislosti na tom, jestli tím SKU systémem Linux nebo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-106">The process differs depending on whether you are providing a Linux-based or Windows-based SKU.</span></span> <span data-ttu-id="aaa7c-107">Tento článek se týká obou scénářů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-107">This article covers both scenarios.</span></span> <span data-ttu-id="aaa7c-108">Tento postup lze provést paralelně s [vytváření účtů a registrace][link-acct-creation].</span><span class="sxs-lookup"><span data-stu-id="aaa7c-108">This process can be performed in parallel with [Account creation and registration][link-acct-creation].</span></span>

## <a name="1-define-offers-and-skus"></a><span data-ttu-id="aaa7c-109">1. Definování nabídky a SKU</span><span class="sxs-lookup"><span data-stu-id="aaa7c-109">1. Define Offers and SKUs</span></span>
<span data-ttu-id="aaa7c-110">V této části se dozvíte k definování nabídky a jejich přidružené SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-110">In this section, you learn to define the offers and their associated SKUs.</span></span>

<span data-ttu-id="aaa7c-111">Nabídka je „nadřazený objekt“ všech skladových jednotek příslušné nabídky.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-111">An offer is a "parent" to all of its SKUs.</span></span> <span data-ttu-id="aaa7c-112">Nabídek může být víc.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-112">You can have multiple offers.</span></span> <span data-ttu-id="aaa7c-113">Je jenom na vás, jak se rozhodnete svoje nabídky strukturovat.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-113">How you decide to structure your offers is up to you.</span></span> <span data-ttu-id="aaa7c-114">Když se nabídka převede do přípravy, převede se se všemi příslušnými skladovými jednotkami.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-114">When an offer is pushed to staging, it is pushed along with all of its SKUs.</span></span> <span data-ttu-id="aaa7c-115">Pečlivě zvažte vaše identifikátory SKU, protože se budou viditelné v adrese URL:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-115">Carefully consider your SKU identifiers, because they will be visible in the URL:</span></span>

* <span data-ttu-id="aaa7c-116">Azure.com: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}</span><span class="sxs-lookup"><span data-stu-id="aaa7c-116">Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}</span></span>
* <span data-ttu-id="aaa7c-117">Portál Azure preview: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}</span><span class="sxs-lookup"><span data-stu-id="aaa7c-117">Azure preview portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}</span></span>  

<span data-ttu-id="aaa7c-118">SKU je komerční název image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-118">A SKU is the commercial name for a VM image.</span></span> <span data-ttu-id="aaa7c-119">Image virtuálního počítače obsahuje disk jeden operační systém a nula nebo více datových disků.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-119">A VM image contains one operating system disk and zero or more data disks.</span></span> <span data-ttu-id="aaa7c-120">Jde prakticky o kompletní profil úložiště pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-120">It is essentially the complete storage profile for a virtual machine.</span></span> <span data-ttu-id="aaa7c-121">Jeden virtuální pevný disk je potřeba na disk.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-121">One VHD is needed per disk.</span></span> <span data-ttu-id="aaa7c-122">Data i prázdné disky se vyžaduje virtuální pevný disk, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-122">Even blank data disks require a VHD to be created.</span></span>

<span data-ttu-id="aaa7c-123">Bez ohledu na použitý operační systém přidávejte jenom nejmenší počet datových disků, které skladová jednotka potřebuje.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-123">Regardless of which operating system you use, add only the minimum number of data disks needed by the SKU.</span></span> <span data-ttu-id="aaa7c-124">Zákazníci disky, které jsou součástí bitové kopie v době nasazení nelze odebrat, ale můžete vždy přidat disky během nebo po nasazení Pokud je potřebují.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-124">Customers cannot remove disks that are part of an image at the time of deployment but can always add disks during or after deployment if they need them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aaa7c-125">**Neměňte počet disků v nové verzi bitové kopie.**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-125">**Do not change disk count in a new image version.**</span></span> <span data-ttu-id="aaa7c-126">Pokud je nutné překonfigurovat datových disků v imagi, definujte nové SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-126">If you must reconfigure Data disks in the image, define a new SKU.</span></span> <span data-ttu-id="aaa7c-127">Publikování nové verze bitové kopie s počty jiný disk bude mít potenciálně narušující nové nasazení založené na novou verzi bitové kopie v případech automatické škálování, automatické nasazení řešení pomocí šablony ARM a v dalších scénářích.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-127">Publishing a new image version with different disk counts will have the potential of breaking new deployment based on the new image version in cases of auto-scaling, automatic deployments of solutions through ARM templates and other scenarios.</span></span>
>
>

### <a name="11-add-an-offer"></a><span data-ttu-id="aaa7c-128">1.1 přidání nabídky</span><span class="sxs-lookup"><span data-stu-id="aaa7c-128">1.1 Add an offer</span></span>
1. <span data-ttu-id="aaa7c-129">Přihlaste se k [publikování portál] [ link-pubportal] pomocí účtu prodejce.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-129">Sign in to the [Publishing Portal][link-pubportal] by using your seller account.</span></span>
2. <span data-ttu-id="aaa7c-130">Vyberte **virtuální počítače** karta publikování portálu.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-130">Select the **Virtual Machines** tab of the Publishing Portal.</span></span> <span data-ttu-id="aaa7c-131">Pole výzvami položka zadejte název nabídky.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-131">In the prompted entry field, enter your offer name.</span></span> <span data-ttu-id="aaa7c-132">Název nabídky je obvykle název produktu nebo služby, který chcete prodeje v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-132">The offer name is typically the name of the product or service that you plan to sell in the Azure Marketplace.</span></span>
3. <span data-ttu-id="aaa7c-133">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-133">Select **Create**.</span></span>

### <a name="12-define-a-sku"></a><span data-ttu-id="aaa7c-134">1.2 definice SKU</span><span class="sxs-lookup"><span data-stu-id="aaa7c-134">1.2 Define a SKU</span></span>
<span data-ttu-id="aaa7c-135">Po přidání nabídku, musíte definovat a identifikaci vaší SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-135">After you have added an offer, you need to define and identify your SKUs.</span></span> <span data-ttu-id="aaa7c-136">Můžete mít více nabídkami a každý nabídka může mít více SKU v něm.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-136">You can have multiple offers, and each offer can have multiple SKUs under it.</span></span> <span data-ttu-id="aaa7c-137">Když se nabídka převede do přípravy, převede se se všemi příslušnými skladovými jednotkami.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-137">When an offer is pushed to staging, it is pushed along with all of its SKUs.</span></span>

1. <span data-ttu-id="aaa7c-138">**Přidáte SKU.**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-138">**Add a SKU.**</span></span> <span data-ttu-id="aaa7c-139">Verze SKU vyžaduje identifikátor, který se používá v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-139">The SKU requires an identifier, which is used in the URL.</span></span> <span data-ttu-id="aaa7c-140">Identifikátor musí být jedinečný v rámci vaší profil publikování, ale neexistuje žádné riziko identifikátor kolizí s jinými vydavateli.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-140">The identifier must be unique within your publishing profile, but there is no risk of identifier collision with other publishers.</span></span>

   > [!NOTE]
   > <span data-ttu-id="aaa7c-141">Identifikátory SKU a nabídky se zobrazí v adrese URL nabídky na Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-141">The offer and SKU identifiers are displayed in the offer URL in the Marketplace.</span></span>
   >
   >
2. <span data-ttu-id="aaa7c-142">**Přidejte souhrnný popis pro vaše SKU.**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-142">**Add a summary description for your SKU.**</span></span> <span data-ttu-id="aaa7c-143">Souhrn popisy jsou viditelné pro zákazníky, takže si vytvořit snadno čitelné.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-143">Summary descriptions are visible to customers, so you should make them easily readable.</span></span> <span data-ttu-id="aaa7c-144">Tyto informace nemusí být uzamčena dokud fázi "Push do přípravy".</span><span class="sxs-lookup"><span data-stu-id="aaa7c-144">This information does not need to be locked until the "Push to Staging" phase.</span></span> <span data-ttu-id="aaa7c-145">Do té doby je můžete upravovat.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-145">Until then, you are free to edit it.</span></span>
3. <span data-ttu-id="aaa7c-146">Pokud používáte skladové jednotky založené na Windows, přejděte na navrhované odkazy, kde získáte schválené verze Windows Serveru.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-146">If you are using Windows-based SKUs, follow the suggested links to acquire the approved versions of Windows Server.</span></span>

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a><span data-ttu-id="aaa7c-147">2. Vytvoření virtuálního pevného disku kompatibilní s Azure (systémem Linux)</span><span class="sxs-lookup"><span data-stu-id="aaa7c-147">2. Create an Azure-compatible VHD (Linux-based)</span></span>
<span data-ttu-id="aaa7c-148">Tato část se zaměřuje na osvědčené postupy pro vytváření bitové kopie virtuálních počítačů na bázi systému Linux pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-148">This section focuses on best practices for creating a Linux-based VM image for the Azure Marketplace.</span></span> <span data-ttu-id="aaa7c-149">Podrobný postup najdete v následující dokumentaci: [vytváření a odesílání virtuální pevný Disk, který obsahuje operační systém Linux](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="aaa7c-149">For a step-by-step walkthrough, refer to the following documentation: [Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a><span data-ttu-id="aaa7c-150">3. Vytvoření virtuálního pevného disku kompatibilní s Azure (založené na Windows)</span><span class="sxs-lookup"><span data-stu-id="aaa7c-150">3. Create an Azure-compatible VHD (Windows-based)</span></span>
<span data-ttu-id="aaa7c-151">Tato část se zaměřuje na postup vytvoření SKU, založené na Windows serveru pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-151">This section focuses on the steps to create a SKU based on Windows Server for the Azure Marketplace.</span></span>

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a><span data-ttu-id="aaa7c-152">3.1 Ujistěte se, že používáte správné základní virtuální pevné disky</span><span class="sxs-lookup"><span data-stu-id="aaa7c-152">3.1 Ensure that you are using the correct base VHDs</span></span>
<span data-ttu-id="aaa7c-153">Operační systém virtuálního pevného disku pro bitové kopie virtuálního počítače musí být založená na schválení Azure Základní bitová kopie obsahující systém Windows Server nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-153">The operating system VHD for your VM image must be based on an Azure-approved base image that contains Windows Server or SQL Server.</span></span>

<span data-ttu-id="aaa7c-154">Chcete-li začít, vytvoření virtuálního počítače z jednoho z následujících bitových kopií, nachází na [portálu Microsoft Azure][link-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-154">To begin, create a VM from one of the following images, located at the [Microsoft Azure portal][link-azure-portal]:</span></span>

* <span data-ttu-id="aaa7c-155">Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1] [link-datactr-2008-r2])</span><span class="sxs-lookup"><span data-stu-id="aaa7c-155">Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])</span></span>
* <span data-ttu-id="aaa7c-156">SQL Server 2014 ([Enterprise][link-sql-2014-ent], [standardní][link-sql-2014-std], [webové] [ link-sql-2014-web])</span><span class="sxs-lookup"><span data-stu-id="aaa7c-156">SQL Server 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])</span></span>
* <span data-ttu-id="aaa7c-157">SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [standardní][link-sql-2012-std], [webové] [ link-sql-2012-web])</span><span class="sxs-lookup"><span data-stu-id="aaa7c-157">SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])</span></span>

<span data-ttu-id="aaa7c-158">Tyto odkazy se dají najít i na Portálu publikování na stránce skladové jednotky.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-158">These links can also be found in the Publishing Portal under the SKU page.</span></span>

> [!TIP]
> <span data-ttu-id="aaa7c-159">Pokud používáte aktuální portál Azure nebo prostředí PowerShell, jsou schváleny bitové kopie systému Windows Server publikované na 8. září 2014 a novější.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-159">If you are using the current Azure portal or PowerShell, Windows Server images published on September 8, 2014 and later are approved.</span></span>
>
>

### <a name="32-create-your-windows-based-vm"></a><span data-ttu-id="aaa7c-160">3.2 Vytvoření virtuálního počítače založené na systému Windows</span><span class="sxs-lookup"><span data-stu-id="aaa7c-160">3.2 Create your Windows-based VM</span></span>
<span data-ttu-id="aaa7c-161">Z portálu Microsoft Azure můžete vytvořit virtuální počítač na základě schválené základní bitové kopie v několika jednoduchých krocích.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-161">From the Microsoft Azure portal, you can create your VM based on an approved base image in just a few simple steps.</span></span> <span data-ttu-id="aaa7c-162">Toto je přehled procesu:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-162">The following is an overview of the process:</span></span>

1. <span data-ttu-id="aaa7c-163">Na stránce základní bitovou kopii, vyberte **vytvořit virtuální počítač** přesměrovat na nový [portálu Microsoft Azure][link-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="aaa7c-163">From the base image page, select **Create Virtual Machine** to be directed to the new [Microsoft Azure portal][link-azure-portal].</span></span>

    ![Kreslení][img-acom-1]
2. <span data-ttu-id="aaa7c-165">Přihlaste se k portálu účtu Microsoft a heslo pro předplatné Azure, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-165">Sign in to the portal with the Microsoft account and password for the Azure subscription you want to use.</span></span>
3. <span data-ttu-id="aaa7c-166">Postupujte podle výzev a vytvořte virtuální počítač s použitím základní bitové kopie, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-166">Follow the prompts to create a VM by using the base image you have selected.</span></span> <span data-ttu-id="aaa7c-167">Budete muset poskytnout hostiteli name (název počítače), uživatelské jméno (zaregistrované jako správce) a heslo pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-167">You need to provide a host name (name of the computer), user name (registered as an administrator), and password for the VM.</span></span>

    ![Kreslení][img-portal-vm-create]
4. <span data-ttu-id="aaa7c-169">Vyberte velikost virtuálního počítače pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-169">Select the size of the VM to deploy:</span></span>

    <span data-ttu-id="aaa7c-170">a.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-170">a.</span></span>    <span data-ttu-id="aaa7c-171">Pokud máte v plánu vyvíjet virtuálního pevného disku na místě, velikost, nezáleží.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-171">If you plan to develop the VHD on-premises, the size does not matter.</span></span> <span data-ttu-id="aaa7c-172">Zvažte možnost použít jeden z menších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-172">Consider using one of the smaller VMs.</span></span>

    <span data-ttu-id="aaa7c-173">b.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-173">b.</span></span>    <span data-ttu-id="aaa7c-174">Pokud se chystáte vyvíjet image v Azure, zvažte možnost použít pro zvolenou image jednu z doporučených velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-174">If you plan to develop the image in Azure, consider using one of the recommended VM sizes for the selected image.</span></span>

    <span data-ttu-id="aaa7c-175">c.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-175">c.</span></span>    <span data-ttu-id="aaa7c-176">Informace o cenách najdete v části **doporučená cenové úrovně** selektor zobrazená na portálu.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-176">For pricing information, refer to the **Recommended pricing tiers** selector displayed on the portal.</span></span> <span data-ttu-id="aaa7c-177">Poskytne tři doporučené velikosti poskytnuté vydavatelem.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-177">It will provide the three recommended sizes provided by the publisher.</span></span> <span data-ttu-id="aaa7c-178">(V tomto případě je vydavatelem Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="aaa7c-178">(In this case, the publisher is Microsoft.)</span></span>

    ![Kreslení][img-portal-vm-size]
5. <span data-ttu-id="aaa7c-180">Nastavte vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-180">Set properties:</span></span>

    <span data-ttu-id="aaa7c-181">a.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-181">a.</span></span>    <span data-ttu-id="aaa7c-182">Pro rychlé nasazení, můžete ponechat výchozí hodnoty pro vlastnosti v rámci **volitelné konfiguraci** a **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-182">For quick deployment, you can leave the default values for the properties under **Optional Configuration** and **Resource Group**.</span></span>

    <span data-ttu-id="aaa7c-183">b.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-183">b.</span></span>    <span data-ttu-id="aaa7c-184">V části **účet úložiště**, můžete volitelně vybrat účet úložiště, ve kterém bude uložen operační systém virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-184">Under **Storage Account**, you can optionally select the storage account in which the operating system VHD will be stored.</span></span>

    <span data-ttu-id="aaa7c-185">c.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-185">c.</span></span>    <span data-ttu-id="aaa7c-186">V části **skupiny prostředků**, můžete volitelně vybrat logickou skupinu, do kterého chcete umístit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-186">Under **Resource Group**, you can optionally select the logical group in which to place the VM.</span></span>
6. <span data-ttu-id="aaa7c-187">Vyberte **umístění** pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-187">Select the **Location** for deployment:</span></span>

    <span data-ttu-id="aaa7c-188">a.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-188">a.</span></span>    <span data-ttu-id="aaa7c-189">Pokud máte v plánu vyvíjet virtuálního pevného disku na místě, umístění není důležité, protože odešlete bitovou kopii do Azure později.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-189">If you plan to develop the VHD on-premises, the location does not matter because you will upload the image to Azure later.</span></span>

    <span data-ttu-id="aaa7c-190">b.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-190">b.</span></span>    <span data-ttu-id="aaa7c-191">Pokud se chystáte vyvíjet image v Azure, zvažte možnost od začátku používat jednu z oblastí Microsoft Azure ve Spojených státech.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-191">If you plan to develop the image in Azure, consider using one of the US-based Microsoft Azure regions from the beginning.</span></span> <span data-ttu-id="aaa7c-192">Tím se urychlí proces kopírování virtuálního pevného disku, který provádí Microsoft vaším jménem při odesílání bitové kopie pro certifikaci.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-192">This speeds up the VHD copying process that Microsoft performs on your behalf when you submit your image for certification.</span></span>

    ![Kreslení][img-portal-vm-location]
7. <span data-ttu-id="aaa7c-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-194">Click **Create**.</span></span> <span data-ttu-id="aaa7c-195">Virtuální počítač spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-195">The VM starts to deploy.</span></span> <span data-ttu-id="aaa7c-196">Během několika minut budete mít úspěšné nasazení a můžete začít vytvářet image pro svoje skladové jednotky.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-196">Within minutes, you will have a successful deployment and can begin to create the image for your SKU.</span></span>

### <a name="33-develop-your-vhd-in-the-cloud"></a><span data-ttu-id="aaa7c-197">3.3 vyvíjet svůj disk VHD v cloudu</span><span class="sxs-lookup"><span data-stu-id="aaa7c-197">3.3 Develop your VHD in the cloud</span></span>
<span data-ttu-id="aaa7c-198">Důrazně doporučujeme vývoji svůj disk VHD v cloudu pomocí protokolu RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-198">We strongly recommend that you develop your VHD in the cloud by using Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="aaa7c-199">Připojit k protokolu RDP s uživatelské jméno a heslo zadané při zřizování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-199">You connect to RDP with the user name and password specified during provisioning.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aaa7c-200">Pokud vyvíjíte svůj disk VHD najdete v části místní (což nedoporučujeme), [vytváření bitové kopie virtuálního počítače místní](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-200">If you develop your VHD on-premises (which is not recommended), see [Creating a virtual machine image on-premises](marketplace-publishing-vm-image-creation-on-premise.md).</span></span> <span data-ttu-id="aaa7c-201">Stahování svůj disk VHD není nutný, pokud vyvíjíte v cloudu.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-201">Downloading your VHD is not necessary if you are developing in the cloud.</span></span>
>
>

<span data-ttu-id="aaa7c-202">**Připojení prostřednictvím protokolu RDP pomocí [portálu Microsoft Azure][link-azure-portal]**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-202">**Connect via RDP using the [Microsoft Azure portal][link-azure-portal]**</span></span>

1. <span data-ttu-id="aaa7c-203">Vyberte **Procházet** > **virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-203">Select **Browse** > **VMs**.</span></span>
2. <span data-ttu-id="aaa7c-204">Otevře se okno virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-204">The Virtual machines blade opens.</span></span> <span data-ttu-id="aaa7c-205">Zajistěte, aby virtuální počítač, který chcete propojit s běží a vyberte ho ze seznamu nasazených virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-205">Ensure that the VM that you want to connect with is running, and then select it from the list of deployed VMs.</span></span>
3. <span data-ttu-id="aaa7c-206">Otevře se okno popisující vybraný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-206">A blade opens that describes the selected VM.</span></span> <span data-ttu-id="aaa7c-207">V horní části, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-207">At the top, click **Connect**.</span></span>
4. <span data-ttu-id="aaa7c-208">Zobrazí se výzva k zadání uživatelského jména a hesla, který jste určili během zřizování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-208">You are prompted to enter the user name and password that you specified during provisioning.</span></span>

<span data-ttu-id="aaa7c-209">**Připojení přes RDP pomocí prostředí PowerShell**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-209">**Connect via RDP using PowerShell**</span></span>

<span data-ttu-id="aaa7c-210">Chcete-li stáhnout soubor vzdálené plochy na místní počítač, použijte [rutiny Get-AzureRemoteDesktopFile][link-technet-2].</span><span class="sxs-lookup"><span data-stu-id="aaa7c-210">To download a remote desktop file to a local machine, use the [Get-AzureRemoteDesktopFile cmdlet][link-technet-2].</span></span> <span data-ttu-id="aaa7c-211">Chcete-li použít tuto rutinu, musíte znát název služby a název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-211">In order to use this cmdlet, you need to know the name of the service and name of the VM.</span></span> <span data-ttu-id="aaa7c-212">Pokud jste vytvořili virtuální počítač z [portálu Microsoft Azure][link-azure-portal], můžete najít tyto informace v části Vlastnosti virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-212">If you created the VM from the [Microsoft Azure portal][link-azure-portal], you can find this information under VM properties:</span></span>

1. <span data-ttu-id="aaa7c-213">Na portálu Microsoft Azure, vyberte **Procházet** > **virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-213">In the Microsoft Azure portal, select **Browse** > **VMs**.</span></span>
2. <span data-ttu-id="aaa7c-214">Otevře se okno virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-214">The Virtual machines blade opens.</span></span> <span data-ttu-id="aaa7c-215">Vyberte virtuální počítač, který jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-215">Select the VM that you deployed.</span></span>
3. <span data-ttu-id="aaa7c-216">Otevře se okno popisující vybraný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-216">A blade opens that describes the selected VM.</span></span>
4. <span data-ttu-id="aaa7c-217">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-217">Click **Properties**.</span></span>
5. <span data-ttu-id="aaa7c-218">První část názvu domény je název služby.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-218">The first portion of the domain name is the service name.</span></span> <span data-ttu-id="aaa7c-219">Názvem hostitele je název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-219">The host name is the VM name.</span></span>

    ![Kreslení][img-portal-vm-rdp]
6. <span data-ttu-id="aaa7c-221">Stáhnout soubor RDP pro virtuální počítač vytvořený na plochu místní správce rutiny je následující.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-221">The cmdlet to download the RDP file for the created VM to the administrator's local desktop is as follows.</span></span>

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

<span data-ttu-id="aaa7c-222">Další informace o protokolu RDP naleznete na webu MSDN v článku [připojit k virtuální počítač Azure s protokolu RDP nebo SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-222">More information about RDP can be found on MSDN in the article [Connect to an Azure VM with RDP or SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).</span></span>

<span data-ttu-id="aaa7c-223">**Nakonfigurujte virtuální počítač a vytvořte vaše SKU**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-223">**Configure a VM and create your SKU**</span></span>

<span data-ttu-id="aaa7c-224">Po operační systém, který je stáhli VHD použijte Hyper-v a konfigurace virtuálního počítače zahájíte proces vytváření vašeho SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-224">After the operating system VHD is downloaded, use Hyper­V and configure a VM to begin creating your SKU.</span></span> <span data-ttu-id="aaa7c-225">Podrobné kroky najdete na následující odkaz na webu TechNet: [Hyper-v instalovat a konfigurovat virtuální počítač](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-225">Detailed steps can be found at the following TechNet link: [Install Hyper­V and Configure a VM](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="34-choose-the-correct-vhd-size"></a><span data-ttu-id="aaa7c-226">3.4 vyberte správnou velikost virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="aaa7c-226">3.4 Choose the correct VHD size</span></span>
<span data-ttu-id="aaa7c-227">Operační systém Windows virtuálního pevného disku v bitové kopii virtuálního počítače by se vytvořit jako virtuální pevný disk pevného formátu 128 GB.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-227">The Windows operating system VHD in your VM image should be created as a 128-GB fixed-format VHD.</span></span>  

<span data-ttu-id="aaa7c-228">Pokud je velikost fyzické méně než 128 GB, by měla být zhuštěných virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-228">If the physical size is less than 128 GB, the VHD should be sparse.</span></span> <span data-ttu-id="aaa7c-229">Poskytuje základní Image pro Windows a systému SQL Server již splňovat tyto požadavky, takže neměňte formát nebo velikost virtuálního pevného disku získat.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-229">The base Windows and SQL Server images provided already meet these requirements, so do not change the format or the size of the VHD obtained.</span></span>  

<span data-ttu-id="aaa7c-230">Datových disků může být větší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-230">Data disks can be as large as 1 TB.</span></span> <span data-ttu-id="aaa7c-231">Při rozhodování o velikosti disku, mějte na paměti, že zákazníci nemůže změnit velikost virtuálních pevných disků v rámci bitovou kopii v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-231">When deciding on the disk size, remember that customers cannot resize VHDs within an image at the time of deployment.</span></span> <span data-ttu-id="aaa7c-232">Datový disk virtuálních pevných disků by se vytvořit jako virtuální pevný disk pevného formátu.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-232">Data disk VHDs should be created as a fixed-format VHD.</span></span> <span data-ttu-id="aaa7c-233">Měly by být zhuštěných.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-233">They should also be sparse.</span></span> <span data-ttu-id="aaa7c-234">Datové disky můžou být prázdné nebo můžou obsahovat data.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-234">Data disks can be empty or contain data.</span></span>

### <a name="35-install-the-latest-windows-patches"></a><span data-ttu-id="aaa7c-235">3.5 instalaci nejnovějších oprav systému Windows</span><span class="sxs-lookup"><span data-stu-id="aaa7c-235">3.5 Install the latest Windows patches</span></span>
<span data-ttu-id="aaa7c-236">Základní image obsahují opravy, které byly v době publikace imagí nejnovější.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-236">The base images contain the latest patches up to their published date.</span></span> <span data-ttu-id="aaa7c-237">Před publikováním operačního systému virtuálního pevného disku, které jste vytvořili, ujistěte se, že Windows Update byl spuštěn a zda byly nainstalovány všechny nejnovější kritická a důležitá aktualizace zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-237">Before publishing the operating system VHD you have created, ensure that Windows Update has been run and that all the latest Critical and Important security updates have been installed.</span></span>

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a><span data-ttu-id="aaa7c-238">3.6 proveďte další úlohy konfigurace a plán, podle potřeby</span><span class="sxs-lookup"><span data-stu-id="aaa7c-238">3.6 Perform additional configuration and schedule tasks as necessary</span></span>
<span data-ttu-id="aaa7c-239">Pokud není nutná další konfigurace, zvažte použití naplánovanou úlohu, která se spouští při spuštění proveďte změny v konečný k virtuálnímu počítači po jeho nasazení:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-239">If additional configuration is needed, consider using a scheduled task that runs at startup to make any final changes to the VM after it has been deployed:</span></span>

* <span data-ttu-id="aaa7c-240">Osvědčilo se takové nastavení, kdy se úloha po úspěšném spuštění odstraní.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-240">It is a best practice to have the task delete itself upon successful execution.</span></span>
* <span data-ttu-id="aaa7c-241">Žádná konfigurace se spoléhají na jednotkách než jednotky jazyka C nebo D, protože se jedná pouze dvě jednotky, které jsou vždy zaručit existovat.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-241">No configuration should rely on drives other than drives C or D, because these are the only two drives that are always guaranteed to exist.</span></span> <span data-ttu-id="aaa7c-242">Jednotka C je disk operačního systému a jednotky D je dočasný místní disk.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-242">Drive C is the operating system disk, and drive D is the temporary local disk.</span></span>

### <a name="37-generalize-the-image"></a><span data-ttu-id="aaa7c-243">3.7 Zobecněte bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="aaa7c-243">3.7 Generalize the image</span></span>
<span data-ttu-id="aaa7c-244">Všechny bitové kopie v Azure Marketplace musí být znovu použitelné obecné způsobem.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-244">All images in the Azure Marketplace must be reusable in a generic fashion.</span></span> <span data-ttu-id="aaa7c-245">Jinými slovy musí být zobecněn operačního systému virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-245">In other words, the operating system VHD must be generalized:</span></span>

* <span data-ttu-id="aaa7c-246">Pro systém Windows, musí být bitovou kopii "Sysprep" a by mělo být provedeno žádné konfigurace, které nepodporují **sysprep** příkaz.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-246">For Windows, the image should be "sysprepped," and no configurations should be done that do not support the **sysprep** command.</span></span>
* <span data-ttu-id="aaa7c-247">Spuštěním následujícího příkazu z windir%\System32\Sysprep % adresáře.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-247">You can run the following command from the directory %windir%\System32\Sysprep.</span></span>

        sysprep.exe /generalize /oobe /shutdown

  <span data-ttu-id="aaa7c-248">Informace o tom, jak operační systém pro nástroj sysprep najdete v krok v následujícím článku na webu MSDN: [vytvoření a nahrání virtuálního pevného disku serveru Windows Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-248">Guidance on how to sysprep the operating system is provided in Step of the following MSDN article: [Create and upload a Windows Server VHD to Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="4-deploy-a-vm-from-your-vhds"></a><span data-ttu-id="aaa7c-249">4. Nasadit virtuální počítač z virtuální pevné disky</span><span class="sxs-lookup"><span data-stu-id="aaa7c-249">4. Deploy a VM from your VHDs</span></span>
<span data-ttu-id="aaa7c-250">Po odeslání virtuální pevné disky (zobecněný virtuální pevný disk operačního systému a nula nebo více dat na disku VHD) pro účet úložiště Azure je můžete zaregistrovat jako uživatelské image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-250">After you have uploaded your VHDs (the generalized operating system VHD and zero or more data disk VHDs) to an Azure storage account, you can register them as a user VM image.</span></span> <span data-ttu-id="aaa7c-251">Potom můžete otestovat této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-251">Then you can test that image.</span></span> <span data-ttu-id="aaa7c-252">Všimněte si, že vzhledem k tomu, že je zobecněný virtuální pevný disk operačního systému, nelze nasadit přímo virtuálního počítače tím, že poskytuje adresu URL virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-252">Note that because your operating system VHD is generalized, you cannot directly deploy the VM by providing the VHD URL.</span></span>

<span data-ttu-id="aaa7c-253">Další informace o bitové kopie virtuálního počítače, projděte si následující příspěvky blogu:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-253">To learn more about VM images, review the following blog posts:</span></span>

* [<span data-ttu-id="aaa7c-254">Image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aaa7c-254">VM Image</span></span>](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [<span data-ttu-id="aaa7c-255">Jak prostředí PowerShell bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aaa7c-255">VM Image PowerShell How To</span></span>](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [<span data-ttu-id="aaa7c-256">O Image virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="aaa7c-256">About VM images in Azure</span></span>](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a><span data-ttu-id="aaa7c-257">Nastavit potřebné nástroje, prostředí PowerShell a rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="aaa7c-257">Set up the necessary tools, PowerShell and Azure CLI</span></span>
* [<span data-ttu-id="aaa7c-258">Jak nastavit prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="aaa7c-258">How to setup PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="aaa7c-259">Postup instalace rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="aaa7c-259">How to setup Azure CLI</span></span>](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a><span data-ttu-id="aaa7c-260">4.1 vytvořit uživatelské image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aaa7c-260">4.1 Create a user VM image</span></span>
#### <a name="capture-vm"></a><span data-ttu-id="aaa7c-261">Zachycení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aaa7c-261">Capture VM</span></span>
<span data-ttu-id="aaa7c-262">Přečtěte si prosím odkazy níže uvedené pokyny k zachycení virtuálního počítače pomocí rozhraní API, Powershellu nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-262">Please read the links given below for guidance on capturing the VM using API/PowerShell/Azure CLI.</span></span>

* [<span data-ttu-id="aaa7c-263">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="aaa7c-263">API</span></span>](https://msdn.microsoft.com/library/mt163560.aspx)
* [<span data-ttu-id="aaa7c-264">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aaa7c-264">PowerShell</span></span>](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="aaa7c-265">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aaa7c-265">Azure CLI</span></span>](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a><span data-ttu-id="aaa7c-266">Zobecněte Image</span><span class="sxs-lookup"><span data-stu-id="aaa7c-266">Generalize Image</span></span>
<span data-ttu-id="aaa7c-267">Přečtěte si prosím odkazy níže uvedené pokyny k zachycení virtuálního počítače pomocí rozhraní API, Powershellu nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-267">Please read the links given below for guidance on capturing the VM using API/PowerShell/Azure CLI.</span></span>

* [<span data-ttu-id="aaa7c-268">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="aaa7c-268">API</span></span>](https://msdn.microsoft.com/library/mt269439.aspx)
* [<span data-ttu-id="aaa7c-269">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aaa7c-269">PowerShell</span></span>](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="aaa7c-270">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aaa7c-270">Azure CLI</span></span>](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a><span data-ttu-id="aaa7c-271">4.2 nasaďte virtuální počítač z uživatelské image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aaa7c-271">4.2 Deploy a VM from a user VM image</span></span>
<span data-ttu-id="aaa7c-272">Chcete-li nasadit virtuální počítač z uživatelské image virtuálního počítače, můžete použít aktuální [portál Azure](https://manage.windowsazure.com) nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-272">To deploy a VM from a user VM image, you can use the current [Azure portal](https://manage.windowsazure.com) or PowerShell.</span></span>

<span data-ttu-id="aaa7c-273">**Nasadit virtuální počítač z aktuální portál Azure**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-273">**Deploy a VM from the current Azure portal**</span></span>

1. <span data-ttu-id="aaa7c-274">Přejděte na **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-274">Go to **New** > **Compute** > **Virtual machine** > **From gallery**.</span></span>

    ![Kreslení][img-manage-vm-new]
2. <span data-ttu-id="aaa7c-276">Přejděte na **Moje image**a potom vyberte image virtuálního počítače, ze kterého chcete nasadit virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-276">Go to **My images**, and then select the VM image from which to deploy a VM:</span></span>

   1. <span data-ttu-id="aaa7c-277">Zaměřit se na která image můžete vybrat, protože **Moje image** zobrazení obsahuje bitové kopie operačního systému a Image virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-277">Pay close attention to which image you select, because the **My images** view lists both operating system images and VM images.</span></span>
   2. <span data-ttu-id="aaa7c-278">Prohlížení počet disků může pomoct určit, jaký typ obrázku nasazujete, protože většina Image virtuálních počítačů mají více než jeden disk.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-278">Looking at the number of disks can help determine what type of image you are deploying, because the majority of VM images have more than one disk.</span></span> <span data-ttu-id="aaa7c-279">Je však stále možné, že image virtuálního počítače s pouze jednoho operačního systému disku, což by pak znamenalo **počet disků** nastavena na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-279">However, it is still possible to have a VM image with only a single operating system disk, which would then have **Number of disks** set to 1.</span></span>

      ![Kreslení][img-manage-vm-select]
3. <span data-ttu-id="aaa7c-281">Postupujte podle Průvodce vytvořením virtuálního počítače a zadejte virtuální počítač název, virtuální počítač velikost, umístění, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-281">Follow the VM creation wizard and specify the VM name, VM size, location, user name, and password.</span></span>

<span data-ttu-id="aaa7c-282">**Nasadit virtuální počítač z prostředí PowerShell**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-282">**Deploy a VM from PowerShell**</span></span>

<span data-ttu-id="aaa7c-283">Pokud chcete nasadit velký virtuální počítač z zobecněný image virtuálního počítače, které jsou právě vytvořili, můžete použít následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-283">To deploy a large VM from the generalized VM image just created, you can use the following cmdlets.</span></span>

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> <span data-ttu-id="aaa7c-284">O další pomoc naleznete [Poradce při potížích s běžné problémy došlo při vytváření virtuálního pevného disku].</span><span class="sxs-lookup"><span data-stu-id="aaa7c-284">Please refer [Troubleshooting common issues encountered during VHD creation] for additional assistance.</span></span>
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a><span data-ttu-id="aaa7c-285">5. Získat certifikační pro bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aaa7c-285">5. Obtain certification for your VM image</span></span>
<span data-ttu-id="aaa7c-286">Dalším krokem při přípravě image virtuálních počítačů pro Azure Marketplace je jej certifikaci.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-286">The next step in preparing your VM image for the Azure Marketplace is to have it certified.</span></span>

<span data-ttu-id="aaa7c-287">Tento proces zahrnuje spuštění nástroje speciální certifikační, odesílání výsledky ověření ke kontejneru Azure, kde jsou umístěny virtuální pevné disky, přidání nabídku, definování vaší SKU a odesílání bitové kopie virtuálních počítačů pro certifikaci.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-287">This process includes running a special certification tool, uploading the verification results to the Azure container where your VHDs reside, adding an offer, defining your SKU, and submitting your VM image for certification.</span></span>

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a><span data-ttu-id="aaa7c-288">5.1 Stáhněte a spusťte nástroj pro testování certifikační pro certifikaci Azure</span><span class="sxs-lookup"><span data-stu-id="aaa7c-288">5.1 Download and run the Certification Test Tool for Azure Certified</span></span>
<span data-ttu-id="aaa7c-289">Spuštění nástroje certifikační v spuštěný virtuální počítač, zajištěného z vaší uživatelské image virtuálního počítače, pro zajištění, že image virtuálního počítače je kompatibilní s Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-289">The certification tool runs on a running VM, provisioned from your user VM image, to ensure that the VM image is compatible with Microsoft Azure.</span></span> <span data-ttu-id="aaa7c-290">Ověří, že jsou splněné pokyny a požadavky na přípravu vašeho virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-290">It will verify that the guidance and requirements about preparing your VHD have been met.</span></span> <span data-ttu-id="aaa7c-291">Výstup nástroje je zpráva o kompatibilitě, které musí být nahrán na portálu publikování při požadavku certifikační.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-291">The output of the tool is a compatibility report, which should be uploaded on the Publishing Portal while requesting certification.</span></span>

<span data-ttu-id="aaa7c-292">Nástroj certifikační lze použít s Windows a virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-292">The certification tool can be used with both Windows and Linux VMs.</span></span> <span data-ttu-id="aaa7c-293">Připojení k systému Windows virtuální počítače pomocí prostředí PowerShell a připojí k virtuální počítače s Linuxem pomocí SSH.Net:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-293">It connects to Windows-based VMs via PowerShell and connects to Linux VMs via SSH.Net:</span></span>

1. <span data-ttu-id="aaa7c-294">První, stáhněte si nástroj Certifikační na [společnosti Microsoft stažení][link-msft-download].</span><span class="sxs-lookup"><span data-stu-id="aaa7c-294">First, download the certification tool at the [Microsoft download site][link-msft-download].</span></span>
2. <span data-ttu-id="aaa7c-295">Otevřete nástroj certifikační a klikněte **spustit nový Test** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-295">Open the certification tool, and then click the **Start New Test** button.</span></span>
3. <span data-ttu-id="aaa7c-296">Z **testování informace** obrazovky, zadejte název pro test spustit.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-296">From the **Test Information** screen, enter a name for the test run.</span></span>
4. <span data-ttu-id="aaa7c-297">Zvolte, jestli váš virtuální počítač používá Linux nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-297">Choose whether your VM is on Linux or Windows.</span></span> <span data-ttu-id="aaa7c-298">Podle toho, který systém zvolíte, vyberte další možnosti.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-298">Depending on which you choose, select the subsequent options.</span></span>

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a><span data-ttu-id="aaa7c-299">**Připojení nástroje certifikační do bitové kopie virtuálního počítače s Linuxem**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-299">**Connect the certification tool to a Linux VM image**</span></span>
1. <span data-ttu-id="aaa7c-300">Zvolte režim ověřování SSH: heslo nebo soubor klíče.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-300">Select the SSH authentication mode: password or key file.</span></span>
2. <span data-ttu-id="aaa7c-301">Pokud používáte ověřování pomocí hesla, zadejte název, uživatelské jméno a heslo systému DNS (Domain Name).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-301">If using password-­based authentication, enter the Domain Name System (DNS) name, user name, and password.</span></span>
3. <span data-ttu-id="aaa7c-302">Pokud používáte ověřování soubor klíče, zadejte název DNS, uživatelské jméno a umístění privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-302">If using key file authentication, enter the DNS name, user name, and private key location.</span></span>

   ![Ověřování hesla bitové kopie virtuálních počítačů Linux][img-cert-vm-pswd-lnx]

   ![Soubor klíče ověřování bitové kopie virtuálních počítačů Linux][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a><span data-ttu-id="aaa7c-305">**Připojení nástroje certifikační do bitové kopie virtuálních počítačů na bázi Windows**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-305">**Connect the certification tool to a Windows-based VM image**</span></span>
1. <span data-ttu-id="aaa7c-306">Zadejte plně kvalifikovaný název DNS virtuálního počítače (například MyVMName.Cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-306">Enter the fully qualified VM DNS name (for example, MyVMName.Cloudapp.net).</span></span>
2. <span data-ttu-id="aaa7c-307">Zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-307">Enter the user name and password.</span></span>

   ![Ověřování hesla bitové kopie systému Windows virtuálního počítače][img-cert-vm-pswd-win]

<span data-ttu-id="aaa7c-309">Po výběru se správnými možnostmi pro bitové kopie virtuálního počítače založené na systému Windows nebo Linux, vyberte **Test připojení** zajistit, že SSH.Net nebo prostředí PowerShell není platné připojení pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-309">After you have selected the correct options for your Linux or Windows-based VM image, select **Test Connection** to ensure that SSH.Net or PowerShell has a valid connection for testing purposes.</span></span> <span data-ttu-id="aaa7c-310">Po vytvoření připojení, vyberte **Další** spustíte test.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-310">After a connection is established, select **Next** to start the test.</span></span>

<span data-ttu-id="aaa7c-311">Po dokončení testu dostanete výsledky (Prošel/Neprošel/Upozornění) pro každý prvek testu.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-311">When the test is complete, you will receive the results (Pass/Fail/Warning) for each test element.</span></span>

![Testovací případy bitové kopie virtuálních počítačů Linux][img-cert-vm-test-lnx]

![Testovací případy pro Image virtuálního počítače Windows][img-cert-vm-test-win]

<span data-ttu-id="aaa7c-314">Pokud některý z testů selže, nebude musí být certifikovaná bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-314">If any of the tests fail, your image will not be certified.</span></span> <span data-ttu-id="aaa7c-315">Pokud k tomu dojde, zkontrolujte požadavky na a proveďte potřebné změny.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-315">If this occurs, review the requirements and make any necessary changes.</span></span>

<span data-ttu-id="aaa7c-316">Po dokončení automatizovaných testů budete vyzváni k zadání dalších na image virtuálního počítače prostřednictvím dotazník obrazovky.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-316">After the automated test, you are asked to provide additional input on your VM image via a questionnaire screen.</span></span>  <span data-ttu-id="aaa7c-317">Dokončení otázky a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-317">Complete the questions, and then select **Next**.</span></span>

![Dotazník nástroj certifikace][img-cert-vm-questionnaire]

![Dotazník nástroj certifikace][img-cert-vm-questionnaire-2]

<span data-ttu-id="aaa7c-320">Po dokončení dotazník, můžete poskytovat další informace, jako jsou informace o přístupu SSH pro virtuální počítač s Linuxem bitové kopie a vysvětlení pro všechny neúspěšné vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-320">After you have completed the questionnaire, you can provide additional information such as SSH access information for the Linux VM image and an explanation for any failed assessments.</span></span> <span data-ttu-id="aaa7c-321">Kromě vaše odpovědi na dotazník můžete stáhnout výsledků testů a soubory protokolu pro spuštění testovací případy.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-321">You can download the test results and log files for the executed test cases in addition to your answers to the questionnaire.</span></span> <span data-ttu-id="aaa7c-322">Výsledky uložte ve stejném kontejneru jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-322">Save the results in the same container as your VHDs.</span></span>

![Uložit certifikační výsledky testů][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a><span data-ttu-id="aaa7c-324">5.2 získáte sdílený přístupový podpis identifikátor URI pro vaše Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="aaa7c-324">5.2 Get the shared access signature URI for your VM images</span></span>
<span data-ttu-id="aaa7c-325">Během procesu publikování zadejte identifikátory URI (Identifier), které vést ke každému z virtuálních pevných disků jste vytvořili pro vaše SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-325">During the publishing process, you specify the uniform resource identifiers (URIs) that lead to each of the VHDs you have created for your SKU.</span></span> <span data-ttu-id="aaa7c-326">Microsoft během certifikačního procesu potřebuje přístup k těmto virtuálním pevným diskům.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-326">Microsoft needs access to these VHDs during the certification process.</span></span> <span data-ttu-id="aaa7c-327">Proto musíte vytvořit sdílený přístupový podpis identifikátor URI pro každý virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-327">Therefore, you need to create a shared access signature URI for each VHD.</span></span> <span data-ttu-id="aaa7c-328">Toto je identifikátor URI, který by měly být zadány ve **bitové kopie** na portálu publikování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-328">This is the URI that should be entered in the **Images** tab in the Publishing Portal.</span></span>

<span data-ttu-id="aaa7c-329">Sdílený přístupový podpis, který vytvořili identifikátoru URI by měl splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-329">The shared access signature URI created should adhere to the following requirements:</span></span>

* <span data-ttu-id="aaa7c-330">Při generování sdílený přístupový podpis identifikátory URI pro virtuální pevné disky, je dostatečné oprávnění seznamu a pro čtení.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-330">When generating shared access signature URIs for your VHDs, List and Read­ permissions are sufficient.</span></span> <span data-ttu-id="aaa7c-331">Neposkytujte přístup pro psaní nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-331">Do not provide Write or Delete access.</span></span>
* <span data-ttu-id="aaa7c-332">Dobu trvání pro přístup musí být minimálně týdny tří (3) ze při vytvoření sdílený přístupový podpis identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-332">The duration for access should be a minimum of three (3) weeks from when the shared access signature URI is created.</span></span>
* <span data-ttu-id="aaa7c-333">Aby se předešlo pro čas UTC, vyberte den, než aktuální datum.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-333">To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="aaa7c-334">Například pokud je aktuální datum 6. října 2014, vyberte 5/10/2014.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-334">For example, if the current date is October 6, 2014, select 10/5/2014.</span></span>

<span data-ttu-id="aaa7c-335">SAS adresa URL může být generována v několika způsoby, jak sdílet svůj disk VHD pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-335">SAS URL can be generated in multiple ways to share your VHD for Azure Marketplace.</span></span>
<span data-ttu-id="aaa7c-336">Následují 3 doporučených nástrojů:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-336">Following are the 3 recommended tools:</span></span>

1.  <span data-ttu-id="aaa7c-337">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="aaa7c-337">Azure Storage Explorer</span></span>
2.  <span data-ttu-id="aaa7c-338">Průzkumník úložišť Microsoft</span><span class="sxs-lookup"><span data-stu-id="aaa7c-338">Microsoft Storage Explorer</span></span>
3.  <span data-ttu-id="aaa7c-339">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aaa7c-339">Azure CLI</span></span>

<span data-ttu-id="aaa7c-340">**Azure Storage Explorer (doporučeno pro uživatele systému Windows)**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-340">**Azure Storage Explorer (Recommended for Windows Users)**</span></span>

<span data-ttu-id="aaa7c-341">Toto jsou kroky pro vytvoření adresy URL SAS pomocí Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="aaa7c-341">Following are the steps for generating SAS URL by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="aaa7c-342">Stáhněte si [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) na webu CodePlex.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-342">Download [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) from CodePlex.</span></span> <span data-ttu-id="aaa7c-343">Přejděte na [Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) a klikněte na tlačítko **"Server"**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-343">Go to [Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) and click **"Downloads"**.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. <span data-ttu-id="aaa7c-345">Stáhněte si [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) a po rozbalení ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-345">Download [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) and install after unzipping it.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. <span data-ttu-id="aaa7c-347">Po instalaci, otevřete aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-347">After it is installed, open the application.</span></span>
4. <span data-ttu-id="aaa7c-348">Klikněte na tlačítko **přidejte účet**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-348">Click **Add Account**.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. <span data-ttu-id="aaa7c-350">Zadejte název účtu úložiště, klíč účtu úložiště a úložiště koncové body domény.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-350">Specify the storage account name, storage account key, and storage endpoints domain.</span></span> <span data-ttu-id="aaa7c-351">Toto je účet úložiště ve vašem předplatném Azure, kde mají uchovávat svůj disk VHD na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-351">This is the storage account in your Azure subscription where you have kept your VHD on Azure portal.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. <span data-ttu-id="aaa7c-353">Azure Storage Explorer po připojení k vašemu účtu konkrétní úložiště, zahájí se zobrazuje všechny obsahuje v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-353">Once Azure Storage Explorer is connected to your specific storage account, it will start showing all of the contains within the storage account.</span></span> <span data-ttu-id="aaa7c-354">Vyberte kontejner, kam jste zkopírovali soubor VHD disku operačního systému (také datových disků Pokud jsou k dispozici pro váš scénář).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-354">Select the container where you copied the operating system disk VHD file (also data disks if they are applicable for your scenario).</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. <span data-ttu-id="aaa7c-356">Po výběru kontejneru objektů blob, Azure Storage Explorer spustí zobrazující soubory v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-356">After selecting the blob container, Azure Storage Explorer starts showing the files within the container.</span></span> <span data-ttu-id="aaa7c-357">Vyberte soubor bitové kopie (VHD), který musí být odeslána.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-357">Select the image file (.vhd) that needs to be submitted.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  <span data-ttu-id="aaa7c-359">Po výběru souboru VHD v kontejneru, klikněte na **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-359">After selecting the .vhd file in the container, click the **Security** tab.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  <span data-ttu-id="aaa7c-361">V **zabezpečení kontejneru objektů Blob** dialogové okno pole, ponechte výchozí hodnoty na **úroveň přístupu** a pak klikněte **sdílené přístupové podpisy** kartě.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-361">In the **Blob Container Security** dialog box, leave the defaults on the **Access Level** tab, and then click **Shared Access Signatures** tab.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. <span data-ttu-id="aaa7c-363">Postupujte podle následujících kroků a vygenerovat sdílený přístupový podpis identifikátor URI pro image VHD:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-363">Follow the steps below to generate a shared access signature URI for the .vhd image:</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    <span data-ttu-id="aaa7c-365">a.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-365">a.</span></span> <span data-ttu-id="aaa7c-366">**Přístup povolen ze:** aby se předešlo pro čas UTC, vyberte den před aktuálním datem.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-366">**Access permitted from:** To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="aaa7c-367">Například pokud je aktuální datum 6. října 2014, vyberte 5/10/2014.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-367">For example, if the current date is October 6, 2014, select 10/5/2014.</span></span>

    <span data-ttu-id="aaa7c-368">b.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-368">b.</span></span> <span data-ttu-id="aaa7c-369">**Umožnit přístup:** vyberte datum, které je minimálně 3 týdny po **přístup povolen ze** datum.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-369">**Access permitted to:** Select a date that is at least 3 weeks after the **Access permitted from** date.</span></span>

    <span data-ttu-id="aaa7c-370">c.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-370">c.</span></span> <span data-ttu-id="aaa7c-371">**Akce povolené:** vyberte **seznamu** a **čtení** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-371">**Actions permitted:** Select the **List** and **Read** permissions.</span></span>

    <span data-ttu-id="aaa7c-372">d.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-372">d.</span></span> <span data-ttu-id="aaa7c-373">Pokud jste zvolili souboru VHD správně, pak se zobrazí v souboru **název objektu Blob pro přístup k** s příponou VHD.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-373">If you have selected your .vhd file correctly, then your file appears in **Blob name to access** with extension .vhd.</span></span>

    <span data-ttu-id="aaa7c-374">e.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-374">e.</span></span> <span data-ttu-id="aaa7c-375">Klikněte na tlačítko **generování podpisu**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-375">Click **Generate Signature**.</span></span>

    <span data-ttu-id="aaa7c-376">f.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-376">f.</span></span> <span data-ttu-id="aaa7c-377">V **vygenerovat sdílený přístup podpis identifikátor URI tohoto kontejneru**, zkontrolujte následující kroky jako zvýrazněná výše:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-377">In **Generated Shared Access Signature URI of this container**, check for the following as highlighted above:</span></span>

       - <span data-ttu-id="aaa7c-378">Ujistěte se, že název souboru bitové kopie a **"VHD"** jsou v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-378">Make sure that your image file name and **".vhd"** are in the URI.</span></span>
       - <span data-ttu-id="aaa7c-379">Na konci podpis, ujistěte se, že **"= rl"** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-379">At the end of the signature, make sure that **"=rl"** appears.</span></span> <span data-ttu-id="aaa7c-380">To znamená, že přístup pro čtení a seznamu zadaná úspěšně.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-380">This demonstrates that Read and List access was provided successfully.</span></span>
       - <span data-ttu-id="aaa7c-381">Uprostřed podpis, ujistěte se, že **"sr = c"** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-381">In middle of the signature, make sure that **"sr=c"** appears.</span></span> <span data-ttu-id="aaa7c-382">Tento příklad ukazuje, abyste měli přístup na úrovni kontejneru</span><span class="sxs-lookup"><span data-stu-id="aaa7c-382">This demonstrates that you have container level access</span></span>

11. <span data-ttu-id="aaa7c-383">Aby se zajistilo, že vygenerovaného sdílet přístup podpis URI funguje, klikněte na tlačítko **Test v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-383">To ensure that the generated shared access signature URI works, click **Test in Browser**.</span></span> <span data-ttu-id="aaa7c-384">By se měl spustit proces stahování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-384">It should start the download process.</span></span>

12. <span data-ttu-id="aaa7c-385">Zkopírujte sdílený přístupový podpis identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-385">Copy the shared access signature URI.</span></span> <span data-ttu-id="aaa7c-386">Je to identifikátor, který se má vložit do Portálu publikování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-386">This is the URI to paste into the Publishing Portal.</span></span>

13. <span data-ttu-id="aaa7c-387">Opakujte kroky 6-10 pro každý virtuální pevný disk v verze SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-387">Repeat steps 6-10 for each VHD in the SKU.</span></span>

<span data-ttu-id="aaa7c-388">**Microsoft Azure Storage Explorer (Windows nebo MAC/Linux)**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-388">**Microsoft Azure Storage Explorer (Windows/MAC/Linux)**</span></span>

<span data-ttu-id="aaa7c-389">Toto jsou kroky pro vytvoření adresy URL SAS pomocí Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="aaa7c-389">Following are the steps for generating SAS URL by using Microsoft Azure Storage Explorer</span></span>

1.  <span data-ttu-id="aaa7c-390">Stáhněte si Microsoft Azure Storage Explorer formuláře [http://storageexplorer.com/](http://storageexplorer.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-390">Download Microsoft Azure Storage Explorer form [http://storageexplorer.com/](http://storageexplorer.com/) website.</span></span> <span data-ttu-id="aaa7c-391">Přejděte na [Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) a klikněte na tlačítko **"Stáhnout pro systém Windows"**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-391">Go to [Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) and click **“Download for Windows”**.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  <span data-ttu-id="aaa7c-393">Po instalaci, otevřete aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-393">After it is installed, open the application.</span></span>

3.  <span data-ttu-id="aaa7c-394">Klikněte na tlačítko **přidejte účet**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-394">Click **Add Account**.</span></span>

4.  <span data-ttu-id="aaa7c-395">Nakonfigurovat k předplatnému Microsoft Azure Storage Explorer přihlášení ke svému účtu</span><span class="sxs-lookup"><span data-stu-id="aaa7c-395">Configure Microsoft Azure Storage Explorer to your subscription by sign in to your account</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  <span data-ttu-id="aaa7c-397">Přejděte k účtu úložiště a pak vyberte kontejner</span><span class="sxs-lookup"><span data-stu-id="aaa7c-397">Go to storage account and select the Container</span></span>

6.  <span data-ttu-id="aaa7c-398">Vyberte **"Get přístupový podpis sdílenou složku..."**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-398">Select **“Get Share Access Signature..”**</span></span> <span data-ttu-id="aaa7c-399">Klikněte pravým tlačítkem z pomocí **kontejneru**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-399">by using Right Click of the **container**</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  <span data-ttu-id="aaa7c-401">Čas spuštění aktualizace, čas vypršení platnosti a oprávnění podle následující</span><span class="sxs-lookup"><span data-stu-id="aaa7c-401">Update Start time, Expiry time and Permissions as per following</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    <span data-ttu-id="aaa7c-403">a.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-403">a.</span></span>  <span data-ttu-id="aaa7c-404">**Čas spuštění:** aby se předešlo pro čas UTC, vyberte den před aktuálním datem.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-404">**Start Time:** To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="aaa7c-405">Například pokud je aktuální datum 6. října 2014, vyberte 5/10/2014.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-405">For example, if the current date is October 6, 2014, select 10/5/2014.</span></span>

    <span data-ttu-id="aaa7c-406">b.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-406">b.</span></span>  <span data-ttu-id="aaa7c-407">**Čas vypršení platnosti:** vyberte datum, které je minimálně 3 týdny po **čas spuštění** datum.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-407">**Expiry Time:** Select a date that is at least 3 weeks after the **Start Time** date.</span></span>

    <span data-ttu-id="aaa7c-408">c.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-408">c.</span></span>  <span data-ttu-id="aaa7c-409">**Oprávnění:** vyberte **seznamu** a **čtení** oprávnění</span><span class="sxs-lookup"><span data-stu-id="aaa7c-409">**Permissions:** Select the **List** and **Read** permissions</span></span>

8.  <span data-ttu-id="aaa7c-410">Zkopírujte URI sdílený přístupový podpis kontejneru</span><span class="sxs-lookup"><span data-stu-id="aaa7c-410">Copy Container shared access signature URI</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    <span data-ttu-id="aaa7c-412">Adresa URL generovaného SAS pro kontejner úroveň a nyní budeme muset přidat název virtuálního pevného disku v ní.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-412">Generated SAS URL is for container Level and now we need to add VHD name in it.</span></span>

    <span data-ttu-id="aaa7c-413">Formát adresy URL úrovně SAS kontejneru:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="aaa7c-413">Format of Container Level SAS URL: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    <span data-ttu-id="aaa7c-414">Za název kontejneru v adrese URL SAS, jak je uvedeno níže vložit název disku VHD`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="aaa7c-414">Insert VHD name after the container name in SAS URL as below `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    <span data-ttu-id="aaa7c-415">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-415">Example:</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    <span data-ttu-id="aaa7c-417">TestRGVM201631920152.vhd je název virtuálního pevného disku, pak bude mít adresu URL SAS virtuálního pevného disku`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="aaa7c-417">TestRGVM201631920152.vhd is the VHD Name then VHD SAS URL will be `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    - <span data-ttu-id="aaa7c-418">Ujistěte se, že název souboru bitové kopie a **"VHD"** jsou v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-418">Make sure that your image file name and **".vhd"** are in the URI.</span></span>
    - <span data-ttu-id="aaa7c-419">Uprostřed podpis, ujistěte se, že **"sp = rl"** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-419">In middle of the signature, make sure that **"sp=rl"** appears.</span></span> <span data-ttu-id="aaa7c-420">To znamená, že přístup pro čtení a seznamu zadaná úspěšně.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-420">This demonstrates that Read and List access was provided successfully.</span></span>
    - <span data-ttu-id="aaa7c-421">Uprostřed podpis, ujistěte se, že **"sr = c"** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-421">In middle of the signature, make sure that **"sr=c"** appears.</span></span> <span data-ttu-id="aaa7c-422">Tento příklad ukazuje, abyste měli přístup na úrovni kontejneru</span><span class="sxs-lookup"><span data-stu-id="aaa7c-422">This demonstrates that you have container level access</span></span>

9.  <span data-ttu-id="aaa7c-423">Aby se zajistilo, že vygenerovaného sdílet přístup podpis URI funguje, otestujte ji v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-423">To ensure that the generated shared access signature URI works, test it in browser.</span></span> <span data-ttu-id="aaa7c-424">By se měl spustit proces stahování</span><span class="sxs-lookup"><span data-stu-id="aaa7c-424">It should start the download process</span></span>

10. <span data-ttu-id="aaa7c-425">Zkopírujte sdílený přístupový podpis identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-425">Copy the shared access signature URI.</span></span> <span data-ttu-id="aaa7c-426">Je to identifikátor, který se má vložit do Portálu publikování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-426">This is the URI to paste into the Publishing Portal.</span></span>

11. <span data-ttu-id="aaa7c-427">Tyto kroky zopakujte pro každý virtuální pevný disk ve skladové jednotce.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-427">Repeat these steps for each VHD in the SKU.</span></span>

<span data-ttu-id="aaa7c-428">**Rozhraní příkazového řádku Azure (doporučeno pro jiný systém než Windows & nepřetržité integrace)**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-428">**Azure CLI (Recommended for Non-Windows & Continuous Integration)**</span></span>

<span data-ttu-id="aaa7c-429">Toto jsou kroky pro vytvoření adresy URL SAS pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="aaa7c-429">Following are the steps for generating SAS URL by using Azure CLI</span></span>

1.  <span data-ttu-id="aaa7c-430">Stáhněte si Microsoft Azure CLI z [zde](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-430">Download Microsoft Azure CLI from [here](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/).</span></span> <span data-ttu-id="aaa7c-431">Můžete také najít jiné odkazy pro  **[Windows](http://aka.ms/webpi-azure-cli)**  a  **[MAC OS](http://aka.ms/mac-azure-cli)**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-431">You can also find different links for **[Windows](http://aka.ms/webpi-azure-cli)** and **[MAC OS](http://aka.ms/mac-azure-cli)**.</span></span>

2.  <span data-ttu-id="aaa7c-432">Po jeho stažení, nainstalujte prosím</span><span class="sxs-lookup"><span data-stu-id="aaa7c-432">Once it is downloaded, please install</span></span>

3.  <span data-ttu-id="aaa7c-433">Vytvořte soubor prostředí PowerShell s následujícím kódem a uložit na místní</span><span class="sxs-lookup"><span data-stu-id="aaa7c-433">Create a PowerShell file with following code and save it in local</span></span>

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    <span data-ttu-id="aaa7c-434">Aktualizujte následující parametry ve výše</span><span class="sxs-lookup"><span data-stu-id="aaa7c-434">Update the following parameters in above</span></span>

    <span data-ttu-id="aaa7c-435">a.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-435">a.</span></span> <span data-ttu-id="aaa7c-436">**`<StorageAccountName>`**: Zadejte název účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="aaa7c-436">**`<StorageAccountName>`**: Give your storage account name</span></span>

    <span data-ttu-id="aaa7c-437">b.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-437">b.</span></span> <span data-ttu-id="aaa7c-438">**`<Storage Account Key>`**: Zadejte klíč účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="aaa7c-438">**`<Storage Account Key>`**: Give your storage account key</span></span>

    <span data-ttu-id="aaa7c-439">c.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-439">c.</span></span> <span data-ttu-id="aaa7c-440">**`<Permission Start Date>`**: Aby se předešlo pro čas UTC, vyberte den, než aktuální datum.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-440">**`<Permission Start Date>`**: To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="aaa7c-441">Například pokud je aktuální datum 26 října 2016, pak hodnota by měla být 10/25/2016</span><span class="sxs-lookup"><span data-stu-id="aaa7c-441">For example, if the current date is October 26, 2016, then value should be 10/25/2016</span></span>

    <span data-ttu-id="aaa7c-442">d.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-442">d.</span></span> <span data-ttu-id="aaa7c-443">**`<Permission End Date>`**: Vyberte datum, které je minimálně 3 týdny po **počáteční datum**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-443">**`<Permission End Date>`**: Select a date that is at least 3 weeks after the **Start Date**.</span></span> <span data-ttu-id="aaa7c-444">Měla by být hodnota **11/02/2016**.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-444">Then value should be **11/02/2016**.</span></span>

    <span data-ttu-id="aaa7c-445">Následuje příklad kódu po aktualizaci správné parametry</span><span class="sxs-lookup"><span data-stu-id="aaa7c-445">Following is the example code after updating proper parameters</span></span>

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  <span data-ttu-id="aaa7c-446">Otevřete editor prostředí Powershell s režimem "Spustit jako správce" a otevřete soubor v kroku #3.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-446">Open Powershell editor with “Run as Administrator” mode and open file in step #3.</span></span>

5.  <span data-ttu-id="aaa7c-447">Spusťte skript a jeho získáte adresu SAS pro kontejner úrovně přístupu</span><span class="sxs-lookup"><span data-stu-id="aaa7c-447">Run the script and it will provide you the SAS URL for container level access</span></span>

    <span data-ttu-id="aaa7c-448">Následující bude výstup podpis SAS a zkopírujte části zvýrazněných v programu Poznámkový blok</span><span class="sxs-lookup"><span data-stu-id="aaa7c-448">Following will be the output of the SAS Signature and copy the highlighted part in a notepad</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  <span data-ttu-id="aaa7c-450">Nyní budete mít úroveň kontejneru SAS adresa URL a budete muset přidat název disku VHD v ní.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-450">Now you will get container level SAS URL and you need to add VHD name in it.</span></span>

    <span data-ttu-id="aaa7c-451">Kontejner úrovně SAS URL #</span><span class="sxs-lookup"><span data-stu-id="aaa7c-451">Container level SAS URL #</span></span>

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  <span data-ttu-id="aaa7c-452">Název disku VHD vložení za název kontejneru v adrese URL SAS, jak je uvedeno níže`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`</span><span class="sxs-lookup"><span data-stu-id="aaa7c-452">Insert VHD name after the container name in SAS URL as shown below `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`</span></span>

    <span data-ttu-id="aaa7c-453">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-453">Example:</span></span>

    <span data-ttu-id="aaa7c-454">TestRGVM201631920152.vhd je název virtuálního pevného disku, pak bude mít adresu URL SAS virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="aaa7c-454">TestRGVM201631920152.vhd is the VHD Name then VHD SAS URL will be</span></span>

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - <span data-ttu-id="aaa7c-455">Ujistěte se, že název souboru obrázku a "VHD" jsou v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-455">Make sure that your image file name and ".vhd" are in the URI.</span></span>
    -   <span data-ttu-id="aaa7c-456">Uprostřed podpis, ujistěte se, že se zobrazí "sp = rl".</span><span class="sxs-lookup"><span data-stu-id="aaa7c-456">In middle of the signature, make sure that "sp=rl" appears.</span></span> <span data-ttu-id="aaa7c-457">To znamená, že přístup pro čtení a seznamu zadaná úspěšně.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-457">This demonstrates that Read and List access was provided successfully.</span></span>
    -   <span data-ttu-id="aaa7c-458">Uprostřed podpis, ujistěte se, že "sr = c" se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-458">In middle of the signature, make sure that "sr=c" appears.</span></span> <span data-ttu-id="aaa7c-459">Tento příklad ukazuje, abyste měli přístup na úrovni kontejneru</span><span class="sxs-lookup"><span data-stu-id="aaa7c-459">This demonstrates that you have container level access</span></span>

8.  <span data-ttu-id="aaa7c-460">Aby se zajistilo, že vygenerovaného sdílet přístup podpis URI funguje, otestujte ji v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-460">To ensure that the generated shared access signature URI works, test it in browser.</span></span> <span data-ttu-id="aaa7c-461">By se měl spustit proces stahování</span><span class="sxs-lookup"><span data-stu-id="aaa7c-461">It should start the download process</span></span>

9.  <span data-ttu-id="aaa7c-462">Zkopírujte sdílený přístupový podpis identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-462">Copy the shared access signature URI.</span></span> <span data-ttu-id="aaa7c-463">Je to identifikátor, který se má vložit do Portálu publikování.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-463">This is the URI to paste into the Publishing Portal.</span></span>

10. <span data-ttu-id="aaa7c-464">Tyto kroky zopakujte pro každý virtuální pevný disk ve skladové jednotce.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-464">Repeat these steps for each VHD in the SKU.</span></span>


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a><span data-ttu-id="aaa7c-465">5.3 poskytují informace o image virtuálního počítače a požádat o certifikaci publikování portálu</span><span class="sxs-lookup"><span data-stu-id="aaa7c-465">5.3 Provide information about the VM image and request certification in the Publishing Portal</span></span>
<span data-ttu-id="aaa7c-466">Po vytvoření nabídku a SKU, měli byste zadat podrobnosti bitové kopie, které jsou přidružené k této SKU:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-466">After you have created your offer and SKU, you should enter the image details associated with that SKU:</span></span>

1. <span data-ttu-id="aaa7c-467">Přejděte na [publikování portál][link-pubportal]a pak se přihlaste pomocí účtu prodejce.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-467">Go to the [Publishing Portal][link-pubportal], and then sign in with your seller account.</span></span>
2. <span data-ttu-id="aaa7c-468">Vyberte **Image virtuálních počítačů** kartě.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-468">Select the **VM images** tab.</span></span>
3. <span data-ttu-id="aaa7c-469">Identifikátor uvedených v horní části stránky, je ve skutečnosti identifikátor nabídka a není identifikátor SKU.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-469">The identifier listed at the top of the page is actually the offer identifier and not the SKU identifier.</span></span>
4. <span data-ttu-id="aaa7c-470">Vyplňte vlastnosti v rámci **SKU** části.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-470">Fill out the properties under the **SKUs** section.</span></span>
5. <span data-ttu-id="aaa7c-471">V části **operační systém řady**, klikněte na typ operačního systému, které jsou spojené s operačním systémem virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-471">Under **Operating system family**, click the operating system type associated with the operating system VHD.</span></span>
6. <span data-ttu-id="aaa7c-472">V **operačního systému** pole, popisují operačního systému.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-472">In the **Operating system** box, describe the operating system.</span></span> <span data-ttu-id="aaa7c-473">Vezměte v úvahu formátu například řada operačního systému, typ, verze a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-473">Consider a format such as operating system family, type, version, and updates.</span></span> <span data-ttu-id="aaa7c-474">Příkladem může být "Windows Server Datacenter 2014 R2."</span><span class="sxs-lookup"><span data-stu-id="aaa7c-474">An example is "Windows Server Datacenter 2014 R2."</span></span>
7. <span data-ttu-id="aaa7c-475">Vyberte až šest velikosti doporučené virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-475">Select up to six recommended virtual machine sizes.</span></span> <span data-ttu-id="aaa7c-476">Toto jsou doporučení, které získat zobrazují zákazníkovi v okně cenová úroveň na portálu Azure, když se rozhodnete koupit a nasazení bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-476">These are recommendations that get displayed to the customer in the Pricing tier blade in the Azure Portal when they decide to purchase and deploy your image.</span></span> <span data-ttu-id="aaa7c-477">**Jsou to jenom doporučení. Zákazník je možné vybrat libovolnou velikost virtuálního počítače, která bude vyhovovat disky zadané v bitové kopii.**</span><span class="sxs-lookup"><span data-stu-id="aaa7c-477">**These are only recommendations. The customer is able to select any VM size that accommodates the disks specified in your image.**</span></span>
8. <span data-ttu-id="aaa7c-478">Zadejte verzi.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-478">Enter the version.</span></span> <span data-ttu-id="aaa7c-479">Pole verze zapouzdří sémantickou verzi k identifikaci produktu a jeho aktualizací:</span><span class="sxs-lookup"><span data-stu-id="aaa7c-479">The version field encapsulates a semantic version to identify the product and its updates:</span></span>
   * <span data-ttu-id="aaa7c-480">Verze musí být ve tvaru X.Y.Z, kde X, Y a jsou celá čísla.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-480">Versions should be of the form X.Y.Z, where X, Y, and Z are integers.</span></span>
   * <span data-ttu-id="aaa7c-481">Bitové kopie v různých SKU může mít jiný hlavní verze a podverze.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-481">Images in different SKUs can have different major and minor versions.</span></span>
   * <span data-ttu-id="aaa7c-482">Verze v rámci SKU by měl být pouze přírůstkové změny, které zvyšují oprav verze (Z z X.Y.Z).</span><span class="sxs-lookup"><span data-stu-id="aaa7c-482">Versions within a SKU should only be incremental changes, which increase the patch version (Z from X.Y.Z).</span></span>
9. <span data-ttu-id="aaa7c-483">V **URL virtuálního pevného disku operačního systému** zadejte sdílený přístupový podpis URI vytvořený pro operační systém virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-483">In the **OS VHD URL** box, enter the shared access signature URI created for the operating system VHD.</span></span>
10. <span data-ttu-id="aaa7c-484">Pokud existují datových disků přidružených tento SKU, vyberte číslo logické jednotky (LUN), do kterého chcete tento disk data k upevnění po nasazení.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-484">If there are data disks associated with this SKU, select the logical unit number (LUN) to which you would like this data disk to be mounted upon deployment.</span></span>
11. <span data-ttu-id="aaa7c-485">V **LUN X virtuálního pevného disku URL** zadejte sdílený přístupový podpis URI vytvořený pro první data virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-485">In the **LUN X VHD URL** box, enter the shared access signature URI created for the first data VHD.</span></span>

    ![Kreslení](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a><span data-ttu-id="aaa7c-487">Běžné problémy SAS URL & opravy</span><span class="sxs-lookup"><span data-stu-id="aaa7c-487">Common SAS URL issues & fixes</span></span>

|<span data-ttu-id="aaa7c-488">Problém</span><span class="sxs-lookup"><span data-stu-id="aaa7c-488">Issue</span></span>|<span data-ttu-id="aaa7c-489">Zpráva o selhání</span><span class="sxs-lookup"><span data-stu-id="aaa7c-489">Failure Message</span></span>|<span data-ttu-id="aaa7c-490">Oprava</span><span class="sxs-lookup"><span data-stu-id="aaa7c-490">Fix</span></span>|<span data-ttu-id="aaa7c-491">Dokumentace k propojení</span><span class="sxs-lookup"><span data-stu-id="aaa7c-491">Documentation Link</span></span>|
|---|---|---|---|
|<span data-ttu-id="aaa7c-492">Chyba při kopírování bitové kopie - "?" nebyl nalezen v adrese url SAS</span><span class="sxs-lookup"><span data-stu-id="aaa7c-492">Failure in copying images - "?" is not found in SAS url</span></span>|<span data-ttu-id="aaa7c-493">Chyba: Kopírování bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-493">Failure: Copying Images.</span></span> <span data-ttu-id="aaa7c-494">Nepodařilo se stáhnout blob pomocí zadaný identifikátor Uri pro SAS.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-494">Not able to download blob using provided SAS Uri.</span></span>|<span data-ttu-id="aaa7c-495">Aktualizace, pomocí SAS adresa Url, které se doporučuje nástroje</span><span class="sxs-lookup"><span data-stu-id="aaa7c-495">Update the SAS Url using recommended tools</span></span>|[<span data-ttu-id="aaa7c-496">https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/</span><span class="sxs-lookup"><span data-stu-id="aaa7c-496">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="aaa7c-497">Chyba při kopírování bitové kopie - parametry "st" a "se" není v adrese url SAS</span><span class="sxs-lookup"><span data-stu-id="aaa7c-497">Failure in copying images - “st” and “se” parameters not in SAS url</span></span>|<span data-ttu-id="aaa7c-498">Chyba: Kopírování bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-498">Failure: Copying Images.</span></span> <span data-ttu-id="aaa7c-499">Nepodařilo se stáhnout blob pomocí zadaný identifikátor Uri pro SAS.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-499">Not able to download blob using provided SAS Uri.</span></span>|<span data-ttu-id="aaa7c-500">Aktualizovat adresu Url SAS s počátečním a koncovým datem na něm</span><span class="sxs-lookup"><span data-stu-id="aaa7c-500">Update the SAS Url with Start and End dates on it</span></span>|[<span data-ttu-id="aaa7c-501">https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/</span><span class="sxs-lookup"><span data-stu-id="aaa7c-501">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="aaa7c-502">Chyba při kopírování bitové kopie - "sp = rl" není v adrese url SAS</span><span class="sxs-lookup"><span data-stu-id="aaa7c-502">Failure in copying images - “sp=rl” not in SAS url</span></span>|<span data-ttu-id="aaa7c-503">Chyba: Kopírování bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-503">Failure: Copying Images.</span></span> <span data-ttu-id="aaa7c-504">Nepodařilo se stáhnout blob pomocí zadaný identifikátor Uri pro SAS</span><span class="sxs-lookup"><span data-stu-id="aaa7c-504">Not able to download blob using provided SAS Uri</span></span>|<span data-ttu-id="aaa7c-505">Aktualizovat adresu Url SAS s oprávněními nastavenými jako "Číst" a "seznamu</span><span class="sxs-lookup"><span data-stu-id="aaa7c-505">Update the SAS Url with permissions set as “Read” & “List</span></span>|[<span data-ttu-id="aaa7c-506">https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/</span><span class="sxs-lookup"><span data-stu-id="aaa7c-506">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="aaa7c-507">Chyba při kopírování bitové kopie - SAS url obsahovat prázdné znaky v názvu virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="aaa7c-507">Failure in copying images - SAS url have white spaces in vhd name</span></span>|<span data-ttu-id="aaa7c-508">Chyba: Kopírování bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-508">Failure: Copying Images.</span></span> <span data-ttu-id="aaa7c-509">Nepodařilo se stáhnout blob pomocí zadaný identifikátor Uri pro SAS.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-509">Not able to download blob using provided SAS Uri.</span></span>|<span data-ttu-id="aaa7c-510">Aktualizujte adresu Url SAS, bez mezer</span><span class="sxs-lookup"><span data-stu-id="aaa7c-510">Update the SAS Url without white spaces</span></span>|[<span data-ttu-id="aaa7c-511">https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/</span><span class="sxs-lookup"><span data-stu-id="aaa7c-511">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="aaa7c-512">Chyba při kopírování bitové kopie – Chyba autorizace adres Url SAS</span><span class="sxs-lookup"><span data-stu-id="aaa7c-512">Failure in copying images – SAS Url Authorization error</span></span>|<span data-ttu-id="aaa7c-513">Chyba: Kopírování bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-513">Failure: Copying Images.</span></span> <span data-ttu-id="aaa7c-514">Nepodařilo se stáhnout blob kvůli chybě autorizace</span><span class="sxs-lookup"><span data-stu-id="aaa7c-514">Not able to download blob due to authorization error</span></span>|<span data-ttu-id="aaa7c-515">Znovu vygenerovat adresu SAS Url</span><span class="sxs-lookup"><span data-stu-id="aaa7c-515">Regenerate the SAS Url</span></span>|[<span data-ttu-id="aaa7c-516">https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/</span><span class="sxs-lookup"><span data-stu-id="aaa7c-516">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a><span data-ttu-id="aaa7c-517">Další krok</span><span class="sxs-lookup"><span data-stu-id="aaa7c-517">Next step</span></span>
<span data-ttu-id="aaa7c-518">Jakmile jste hotovi s podrobnostmi SKU, můžete přesunout dál [marketing vodítko obsahu Azure Marketplace][link-pushstaging].</span><span class="sxs-lookup"><span data-stu-id="aaa7c-518">After you are done with the SKU details, you can move forward to the [Azure Marketplace marketing content guide][link-pushstaging].</span></span> <span data-ttu-id="aaa7c-519">V tomto kroku procesu publikování zadáte marketing obsahu, ceny a jiné informace, které jsou potřebné před **krok 3: testování virtuální počítač v pracovní nabízejí**, kde můžete testovat různé scénáře případ použití před nasazením nabídnout Azure Marketplace pro veřejné viditelnost a nákup.</span><span class="sxs-lookup"><span data-stu-id="aaa7c-519">In that step of the publishing process, you provide the marketing content, pricing, and other information necessary prior to **Step 3: Testing your VM offer in staging**, where you test various use-case scenarios before deploying the offer to the Azure Marketplace for public visibility and purchase.</span></span>  

## <a name="see-also"></a><span data-ttu-id="aaa7c-520">Viz také</span><span class="sxs-lookup"><span data-stu-id="aaa7c-520">See also</span></span>
* [<span data-ttu-id="aaa7c-521">Začínáme: postup publikování nabídky pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="aaa7c-521">Getting started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
