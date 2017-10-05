---
title: "Problémy s nasazení systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Řešení potíží nasazení systému Windows virtuálního počítače v modelu nasazení Azurethe Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: 6800c137097c2803f28dec7365f6d3f2a173b411
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a><span data-ttu-id="bbbf2-103">Problémy s nasazení systému Windows virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="bbbf2-103">Troubleshoot deploying Windows virtual machine issues in Azure</span></span>

<span data-ttu-id="bbbf2-104">Chcete-li vyřešit problémy při nasazení virtuálního počítače (VM) v Azure, zkontrolujte [top problémy](#top-issues) pro běžné chyby a řešení.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-104">To troubleshoot virtual machine (VM) deployment issues in Azure, review the [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="bbbf2-105">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-105">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="bbbf2-106">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="bbbf2-107">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-107">Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="bbbf2-108">Nejdůležitější problémy</span><span class="sxs-lookup"><span data-stu-id="bbbf2-108">Top issues</span></span>
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a><span data-ttu-id="bbbf2-109">Cluster nemůže podporovat požadovaná velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bbbf2-109">The cluster cannot support the requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="bbbf2-110">Opakujte tuto žádost pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-110">Retry the request using a smaller VM size.</span></span>
- <span data-ttu-id="bbbf2-111">Pokud velikost požadovaný virtuální počítač nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="bbbf2-111">If the size of the requested VM cannot be changed:</span></span>
    - <span data-ttu-id="bbbf2-112">Zastavte všechny virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-112">Stop all the VMs in the availability set.</span></span> <span data-ttu-id="bbbf2-113">Klikněte na tlačítko **skupiny prostředků** > vaší skupiny prostředků > **prostředky** > vaše skupina dostupnosti > **virtuální počítače** > virtuálního počítače > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="bbbf2-114">Po zastavit všechny virtuální počítače, vytvořte v požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-114">After all the VMs stop, create the VM in the desired size.</span></span>
    - <span data-ttu-id="bbbf2-115">Nejprve spusťte nový virtuální počítač a vyberte jednotlivé zastaven virtuálních počítačů a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-115">Start the new VM first, and then select each of the stopped VMs and click Start.</span></span>


## <a name="the-cluster-does-not-have-free-resources"></a><span data-ttu-id="bbbf2-116">Cluster nemá uvolnění prostředků</span><span class="sxs-lookup"><span data-stu-id="bbbf2-116">The cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="bbbf2-117">Opakujte požadavek později.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-117">Retry the request later.</span></span>
- <span data-ttu-id="bbbf2-118">Pokud nový virtuální počítač může být součástí skupiny s jinou dostupnosti</span><span class="sxs-lookup"><span data-stu-id="bbbf2-118">If the new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="bbbf2-119">Vytvoření virtuálního počítače v různých dostupnosti nastavit (ve stejné oblasti).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-119">Create a VM in a different availability set (in the same region).</span></span>
    - <span data-ttu-id="bbbf2-120">Přidáte nový virtuální počítač do stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-120">Add the new VM to the same virtual network.</span></span>

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a><span data-ttu-id="bbbf2-121">Jak můžete používat a nasazení bitové kopie klienta systému windows do Azure?</span><span class="sxs-lookup"><span data-stu-id="bbbf2-121">How can I use and deploy a windows client image into Azure?</span></span>

<span data-ttu-id="bbbf2-122">Můžete Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování Pokud máte předplatné příslušné sadě Visual Studio (dříve MSDN).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-122">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios if you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> <span data-ttu-id="bbbf2-123">To [článku](client-images.md) popisuje požadavky na podmínky pro spuštěné klienta systému Windows v Azure a používá Azure Galerie obrázků.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-123">This [article](client-images.md) outlines the eligibility requirements for running Windows client in Azure and uses of the Azure Gallery images.</span></span>

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a><span data-ttu-id="bbbf2-124">Nasazení virtuálního počítače pomocí výhody použití hybridní (ROZBOČOVAČ)</span><span class="sxs-lookup"><span data-stu-id="bbbf2-124">How can I deploy a virtual machine using the Hybrid Use Benefit (HUB)?</span></span>

<span data-ttu-id="bbbf2-125">Existuje několik různých způsobů, jak nasadit virtuální počítače s Windows s výhodou použití hybridní Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-125">There are a couple of different ways to deploy Windows virtual machines with the Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="bbbf2-126">Předplatné Enterprise Agreement:</span><span class="sxs-lookup"><span data-stu-id="bbbf2-126">For an Enterprise Agreement subscription:</span></span>

<span data-ttu-id="bbbf2-127">• Nasazení virtuálních počítačů z konkrétní Marketplace bitové kopie, které jsou předem nakonfigurovaným rozhraním výhody použití hybridní Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-127">•   Deploy VMs from specific Marketplace images that are pre-configured with Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="bbbf2-128">Pro smlouvy Enterprise:</span><span class="sxs-lookup"><span data-stu-id="bbbf2-128">For Enterprise agreement:</span></span>

<span data-ttu-id="bbbf2-129">• Nahrát vlastní virtuální počítač a nasadit pomocí šablony Resource Manageru nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-129">•   Upload a custom VM and deploy using a Resource Manager template or Azure PowerShell.</span></span>

<span data-ttu-id="bbbf2-130">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="bbbf2-130">For more information, see the following resources:</span></span>

 - [<span data-ttu-id="bbbf2-131">Přehled hybridní použití výhody služby Azure</span><span class="sxs-lookup"><span data-stu-id="bbbf2-131">Azure Hybrid Use Benefit overview </span></span>](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [<span data-ttu-id="bbbf2-132">Nejčastější dotazy ke stažení</span><span class="sxs-lookup"><span data-stu-id="bbbf2-132">Downloadable FAQ</span></span>](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - <span data-ttu-id="bbbf2-133">[Výhody použití Azure hybridní pro Windows Server a klienta Windows](hybrid-use-benefit-licensing.md).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-133">[Azure Hybrid Use Benefit for Windows Server and Windows Client](hybrid-use-benefit-licensing.md).</span></span>

 - [<span data-ttu-id="bbbf2-134">Jak můžete použít výhodou použití hybridní v Azure</span><span class="sxs-lookup"><span data-stu-id="bbbf2-134">How can I use the Hybrid Use Benefit in Azure</span></span>](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="bbbf2-135">Jak lze aktivovat moje měsíční kredit pro sadu Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="bbbf2-135">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="bbbf2-136">Aktivace měsíčního kreditu, najdete v části to [článku](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-136">To activate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a><span data-ttu-id="bbbf2-137">Postup přidání Enterprise pro vývoj/testování pro moje Enterprise Agreement (EA) získat přístup k bitové kopie klienta okna?</span><span class="sxs-lookup"><span data-stu-id="bbbf2-137">How to add Enterprise Dev/Test to my Enterprise Agreement (EA) to get access to Window client images?</span></span>

<span data-ttu-id="bbbf2-138">Schopnost vytvářet odběry podle nabídku Enterprise pro vývoj/testování je omezeno na vlastníky účet, který jste dostali k tomu oprávnění pomocí Správce podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-138">The ability to create subscriptions based on the Enterprise Dev/Test offer is restricted to Account Owners who have been given permission to do so by an Enterprise Administrator.</span></span> <span data-ttu-id="bbbf2-139">Vlastníka účtu vytvoří předplatné prostřednictvím portálu účtů Azure a poté by měl přidejte active odběratele Visual Studio jako spolusprávce.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-139">The Account Owner creates subscriptions via the Azure Account Portal, and then should add active Visual Studio subscribers as co-administrators.</span></span> <span data-ttu-id="bbbf2-140">Tak, aby můžou spravovat a používat prostředky potřebné pro vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-140">So that they can manage and use the resources needed for development and testing.</span></span> <span data-ttu-id="bbbf2-141">Další informace najdete v tématu [Enterprise pro vývoj/testování](https://azure.microsoft.com/offers/ms-azr-0148p/).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-141">For more information, see [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).</span></span>

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a><span data-ttu-id="bbbf2-142">Moje ovladače chybí pro virtuální počítač Windows N-Series</span><span class="sxs-lookup"><span data-stu-id="bbbf2-142">My drivers are missing for my Windows N-Series VM</span></span>

<span data-ttu-id="bbbf2-143">Ovladače pro virtuální počítače na bázi Windows, které jsou umístěné [zde](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-143">Drivers for Windows-based VMs are located [here](n-series-driver-setup.md).</span></span>

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="bbbf2-144">Nelze najít instanci GPU v rámci virtuální počítač N-Series</span><span class="sxs-lookup"><span data-stu-id="bbbf2-144">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="bbbf2-145">Abyste mohli využívat možnosti GPU Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, je nutné nainstalovat NVIDIA grafické ovladače pro každý virtuální počítač po nasazení.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-145">To take advantage of the GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="bbbf2-146">Informace o instalaci ovladačů je k dispozici pro [virtuálních počítačů Windows](n-series-driver-setup.md) a [virtuální počítače s Linuxem](../linux/n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-146">Driver setup information is available for [Windows VMs](n-series-driver-setup.md) and [Linux VMs](../linux/n-series-driver-setup.md).</span></span>

## <a name="are-client-images-supported-for-n-series"></a><span data-ttu-id="bbbf2-147">Jsou podporované Image klienta pro N-Series?</span><span class="sxs-lookup"><span data-stu-id="bbbf2-147">Are client images supported for N-Series?</span></span>

<span data-ttu-id="bbbf2-148">Azure v současné době podporuje pouze N-Series na virtuálních počítačích s operačními systémy Windows Server a Linux.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-148">Currently, Azure only supports N-Series on VMs running Windows Server and Linux operating systems.</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="bbbf2-149">Je k dispozici N-Series virtuálních počítačů v mojí oblasti?</span><span class="sxs-lookup"><span data-stu-id="bbbf2-149">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="bbbf2-150">Můžete zkontrolovat dostupnost z [produkty, které jsou dostupné podle oblasti tabulku](https://azure.microsoft.com/regions/services)a ceny [zde](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-150">You can check the availability from the [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a><span data-ttu-id="bbbf2-151">Jaké obrazy klientů lze použít a nasadit v Azure a jak chcete získat je?</span><span class="sxs-lookup"><span data-stu-id="bbbf2-151">What client images can I use and deploy in Azure, and how to I get them?</span></span>

<span data-ttu-id="bbbf2-152">Můžete použít systém Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování za předpokladu, že máte příslušné předplatné sady Visual Studio (dříve MSDN).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-152">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios provided you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> 

- <span data-ttu-id="bbbf2-153">Bitové kopie systému Windows 10 jsou dostupné z Galerie Azure v rámci [nabízí vhodné pro vývoj/testování](client-images.md#eligible-offers).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-153">Windows 10 images are available from the Azure Gallery within [eligible dev/test offers](client-images.md#eligible-offers).</span></span> 
- <span data-ttu-id="bbbf2-154">Visual Studio odběratele v rámci libovolného typu nabídky můžete také [adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a pak [nahrát do Azure](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-154">Visual Studio subscribers within any type of offer can also [adequately prepare and create](prepare-for-upload-vhd-image.md) a 64-bit Windows 7, Windows 8, or Windows 10 image and then [upload to Azure](upload-generalized-managed.md).</span></span> <span data-ttu-id="bbbf2-155">Použití zůstane omezená na vývoj nebo testování odběratelům active Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-155">The use remains limited to dev/test by active Visual Studio subscribers.</span></span>

<span data-ttu-id="bbbf2-156">To [článku](client-images.md) popisuje požadavky na podmínky pro spuštěné klienta systému Windows Azure a použít Azure Galerie obrázků.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-156">This [article](client-images.md) outlines the eligibility requirements for running Windows client in Azure and use of the Azure Gallery images.</span></span>

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="bbbf2-157">Nejsem moci zobrazit rodiny velikost virtuálního počítače, které chcete při změně velikosti virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-157">I am not able to see VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="bbbf2-158">Když je spuštěný virtuální počítač, je nasazen na fyzickém serveru.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-158">When a VM is running, it is deployed to a physical server.</span></span> <span data-ttu-id="bbbf2-159">Fyzické servery v oblastech Azure jsou seskupené v clusterech běžné fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-159">The physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="bbbf2-160">Změna velikosti virtuálních počítačů, které vyžaduje virtuální počítač se přesunul na jiný hardware clustery se liší v závislosti na modelu nasazení byl použit k nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-160">Resizing a VM that requires the VM to be moved to different hardware clusters is different depending on which deployment model was used to deploy the VM.</span></span>

- <span data-ttu-id="bbbf2-161">Virtuální počítače nasazené v modelu nasazení Classic, nasazení cloudové služby musí být odebrány a znovu nasadit změnit virtuální počítače na velikost v jiné rodiny velikost.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-161">VMs deployed in Classic deployment model, the cloud service deployment must be removed and redeployed to change the VMs to a size in another size family.</span></span>

- <span data-ttu-id="bbbf2-162">Virtuální počítače nasazené v modelu nasazení Resource Manager, je potřeba zastavit všechny virtuální počítače ve skupině před změnou velikosti žádné virtuální počítače v sadě dostupnosti dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-162">VMs deployed in Resource Manager deployment model, you must stop all VMs in the availability set before changing the size of any VM in the availability set.</span></span>

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="bbbf2-163">Uvedené velikosti virtuálního počítače není podporován při nasazení ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-163">The listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="bbbf2-164">Zvolte velikost, která je podporována v sadě dostupnosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-164">Choose a size that is supported on the availability set's cluster.</span></span> <span data-ttu-id="bbbf2-165">Doporučujeme při vytváření dostupnosti nastavit zvolit největší velikost virtuálního počítače, které se domníváte, že je třeba a mít které bude první nasazení do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-165">It is recommended when creating an availability set to choose the largest VM size you think you need, and have that be your first deployment to the Availability set.</span></span>

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a><span data-ttu-id="bbbf2-166">Můžete přidat existující klasické virtuální počítač na sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="bbbf2-166">Can I add an existing Classic VM to an availability set?</span></span>

<span data-ttu-id="bbbf2-167">Ano.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-167">Yes.</span></span> <span data-ttu-id="bbbf2-168">Existující klasické virtuální počítač můžete přidat do nový nebo existující skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-168">You can add an existing classic VM to a new or existing Availability Set.</span></span> <span data-ttu-id="bbbf2-169">Další informace najdete v části [přidat existující virtuální počítač do skupiny dostupnosti](classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-169">For more information see [Add an existing virtual machine to an availability set](classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="bbbf2-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbbf2-170">Next steps</span></span>
<span data-ttu-id="bbbf2-171">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="bbbf2-171">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="bbbf2-172">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-172">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="bbbf2-173">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="bbbf2-173">Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
