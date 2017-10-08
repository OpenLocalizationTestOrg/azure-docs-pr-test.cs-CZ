---
title: "aaaTroubleshoot nasazení problémy virtuální počítač Windows v Azure | Microsoft Docs"
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
ms.openlocfilehash: 30de25827c050cc266761cfc14548bcc64237dac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a><span data-ttu-id="56150-103">Problémy s nasazení systému Windows virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="56150-103">Troubleshoot deploying Windows virtual machine issues in Azure</span></span>

<span data-ttu-id="56150-104">problémy při nasazení virtuálního počítače (VM) tootroubleshoot v Azure, zkontrolujte hello [top problémy](#top-issues) pro běžné chyby a řešení.</span><span class="sxs-lookup"><span data-stu-id="56150-104">tootroubleshoot virtual machine (VM) deployment issues in Azure, review hello [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="56150-105">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="56150-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="56150-106">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="56150-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="56150-107">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="56150-107">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="56150-108">Nejdůležitější problémy</span><span class="sxs-lookup"><span data-stu-id="56150-108">Top issues</span></span>
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a><span data-ttu-id="56150-109">Hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="56150-109">hello cluster cannot support hello requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="56150-110">Opakujte žádost hello pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="56150-110">Retry hello request using a smaller VM size.</span></span>
- <span data-ttu-id="56150-111">Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="56150-111">If hello size of hello requested VM cannot be changed:</span></span>
    - <span data-ttu-id="56150-112">Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="56150-112">Stop all hello VMs in hello availability set.</span></span> <span data-ttu-id="56150-113">Klikněte na tlačítko **skupiny prostředků** > vaší skupiny prostředků > **prostředky** > vaše skupina dostupnosti > **virtuální počítače** > virtuálního počítače > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="56150-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="56150-114">Po všech hello zastavit virtuální počítače, vytvořte hello virtuálních počítačů v hello potřeby velikost.</span><span class="sxs-lookup"><span data-stu-id="56150-114">After all hello VMs stop, create hello VM in hello desired size.</span></span>
    - <span data-ttu-id="56150-115">Spusťte nejprve hello nový virtuální počítač a potom vyberete jednotlivé hello zastaví virtuální počítače a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="56150-115">Start hello new VM first, and then select each of hello stopped VMs and click Start.</span></span>


## <a name="hello-cluster-does-not-have-free-resources"></a><span data-ttu-id="56150-116">Hello cluster nebude mít uvolnění prostředků</span><span class="sxs-lookup"><span data-stu-id="56150-116">hello cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="56150-117">Opakujte žádost hello později.</span><span class="sxs-lookup"><span data-stu-id="56150-117">Retry hello request later.</span></span>
- <span data-ttu-id="56150-118">Pokud můžete technologie hello nový virtuální počítač součástí jiné dostupnosti nastavit</span><span class="sxs-lookup"><span data-stu-id="56150-118">If hello new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="56150-119">Vytvoření virtuálního počítače v sadě dostupnosti jinou (v hello stejné oblasti).</span><span class="sxs-lookup"><span data-stu-id="56150-119">Create a VM in a different availability set (in hello same region).</span></span>
    - <span data-ttu-id="56150-120">Přidat nový virtuální počítač toohello hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="56150-120">Add hello new VM toohello same virtual network.</span></span>

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a><span data-ttu-id="56150-121">Jak můžete používat a nasazení bitové kopie klienta systému windows do Azure?</span><span class="sxs-lookup"><span data-stu-id="56150-121">How can I use and deploy a windows client image into Azure?</span></span>

<span data-ttu-id="56150-122">Můžete Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování Pokud máte předplatné příslušné sadě Visual Studio (dříve MSDN).</span><span class="sxs-lookup"><span data-stu-id="56150-122">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios if you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> <span data-ttu-id="56150-123">To [článku](client-images.md) jsou podrobněji popsány dále hello způsobilosti požadavky pro spuštění klienta systému Windows v Azure a používá hello Image Azure Gallery.</span><span class="sxs-lookup"><span data-stu-id="56150-123">This [article](client-images.md) outlines hello eligibility requirements for running Windows client in Azure and uses of hello Azure Gallery images.</span></span>

## <a name="how-can-i-deploy-a-virtual-machine-using-hello-hybrid-use-benefit-hub"></a><span data-ttu-id="56150-124">Nasazení virtuálního počítače pomocí hello hybridní použití zvýhodnění (ROZBOČOVAČ)</span><span class="sxs-lookup"><span data-stu-id="56150-124">How can I deploy a virtual machine using hello Hybrid Use Benefit (HUB)?</span></span>

<span data-ttu-id="56150-125">Existují dvě různé způsoby toodeploy Windows virtuálních počítačů s hello výhody použití hybridní Azure.</span><span class="sxs-lookup"><span data-stu-id="56150-125">There are a couple of different ways toodeploy Windows virtual machines with hello Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="56150-126">Předplatné Enterprise Agreement:</span><span class="sxs-lookup"><span data-stu-id="56150-126">For an Enterprise Agreement subscription:</span></span>

<span data-ttu-id="56150-127">• Nasazení virtuálních počítačů z konkrétní Marketplace bitové kopie, které jsou předem nakonfigurovaným rozhraním výhody použití hybridní Azure.</span><span class="sxs-lookup"><span data-stu-id="56150-127">•   Deploy VMs from specific Marketplace images that are pre-configured with Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="56150-128">Pro smlouvy Enterprise:</span><span class="sxs-lookup"><span data-stu-id="56150-128">For Enterprise agreement:</span></span>

<span data-ttu-id="56150-129">• Nahrát vlastní virtuální počítač a nasadit pomocí šablony Resource Manageru nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="56150-129">•   Upload a custom VM and deploy using a Resource Manager template or Azure PowerShell.</span></span>

<span data-ttu-id="56150-130">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="56150-130">For more information, see hello following resources:</span></span>

 - [<span data-ttu-id="56150-131">Přehled hybridní použití výhody služby Azure</span><span class="sxs-lookup"><span data-stu-id="56150-131">Azure Hybrid Use Benefit overview </span></span>](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [<span data-ttu-id="56150-132">Nejčastější dotazy ke stažení</span><span class="sxs-lookup"><span data-stu-id="56150-132">Downloadable FAQ</span></span>](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - <span data-ttu-id="56150-133">[Výhody použití Azure hybridní pro Windows Server a klienta Windows](hybrid-use-benefit-licensing.md).</span><span class="sxs-lookup"><span data-stu-id="56150-133">[Azure Hybrid Use Benefit for Windows Server and Windows Client](hybrid-use-benefit-licensing.md).</span></span>

 - [<span data-ttu-id="56150-134">Jak můžete použít hello výhody použití hybridní v Azure</span><span class="sxs-lookup"><span data-stu-id="56150-134">How can I use hello Hybrid Use Benefit in Azure</span></span>](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="56150-135">Jak lze aktivovat moje měsíční kredit pro sadu Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="56150-135">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="56150-136">tooactivate vaše měsíční úvěrové, najdete [článku](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="56150-136">tooactivate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="how-tooadd-enterprise-devtest-toomy-enterprise-agreement-ea-tooget-access-toowindow-client-images"></a><span data-ttu-id="56150-137">Jak tooadd Enterprise pro vývoj/testování toomy Enterprise Agreement (EA) tooget přistupovat k tooWindow obrazy klientů?</span><span class="sxs-lookup"><span data-stu-id="56150-137">How tooadd Enterprise Dev/Test toomy Enterprise Agreement (EA) tooget access tooWindow client images?</span></span>

<span data-ttu-id="56150-138">Hello možnost toocreate odběry podle hello Enterprise pro vývoj/testování nabízejí je omezená tooAccount vlastníky, kteří mají oprávnění toodo Ano pomocí Správce podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="56150-138">hello ability toocreate subscriptions based on hello Enterprise Dev/Test offer is restricted tooAccount Owners who have been given permission toodo so by an Enterprise Administrator.</span></span> <span data-ttu-id="56150-139">Hello vlastníka účtu vytvoří předplatné prostřednictvím hello portálu účtů Azure a poté by měl přidejte active odběratele Visual Studio jako spolusprávce.</span><span class="sxs-lookup"><span data-stu-id="56150-139">hello Account Owner creates subscriptions via hello Azure Account Portal, and then should add active Visual Studio subscribers as co-administrators.</span></span> <span data-ttu-id="56150-140">Tak, aby můžou spravovat a používat hello prostředky potřebné pro vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="56150-140">So that they can manage and use hello resources needed for development and testing.</span></span> <span data-ttu-id="56150-141">Další informace najdete v tématu [Enterprise pro vývoj/testování](https://azure.microsoft.com/offers/ms-azr-0148p/).</span><span class="sxs-lookup"><span data-stu-id="56150-141">For more information, see [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).</span></span>

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a><span data-ttu-id="56150-142">Moje ovladače chybí pro virtuální počítač Windows N-Series</span><span class="sxs-lookup"><span data-stu-id="56150-142">My drivers are missing for my Windows N-Series VM</span></span>

<span data-ttu-id="56150-143">Ovladače pro virtuální počítače na bázi Windows, které jsou umístěné [zde](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="56150-143">Drivers for Windows-based VMs are located [here](n-series-driver-setup.md).</span></span>

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="56150-144">Nelze najít instanci GPU v rámci virtuální počítač N-Series</span><span class="sxs-lookup"><span data-stu-id="56150-144">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="56150-145">tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, je nutné nainstalovat NVIDIA grafické ovladače pro každý virtuální počítač po nasazení.</span><span class="sxs-lookup"><span data-stu-id="56150-145">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="56150-146">Informace o instalaci ovladačů je k dispozici pro [virtuálních počítačů Windows](n-series-driver-setup.md) a [virtuální počítače s Linuxem](../linux/n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="56150-146">Driver setup information is available for [Windows VMs](n-series-driver-setup.md) and [Linux VMs](../linux/n-series-driver-setup.md).</span></span>

## <a name="are-client-images-supported-for-n-series"></a><span data-ttu-id="56150-147">Jsou podporované Image klienta pro N-Series?</span><span class="sxs-lookup"><span data-stu-id="56150-147">Are client images supported for N-Series?</span></span>

<span data-ttu-id="56150-148">Azure v současné době podporuje pouze N-Series na virtuálních počítačích s operačními systémy Windows Server a Linux.</span><span class="sxs-lookup"><span data-stu-id="56150-148">Currently, Azure only supports N-Series on VMs running Windows Server and Linux operating systems.</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="56150-149">Je k dispozici N-Series virtuálních počítačů v mojí oblasti?</span><span class="sxs-lookup"><span data-stu-id="56150-149">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="56150-150">Můžete zkontrolovat dostupnost hello z hello [produkty, které jsou dostupné podle oblasti tabulku](https://azure.microsoft.com/regions/services)a ceny [zde](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="56150-150">You can check hello availability from hello [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-tooi-get-them"></a><span data-ttu-id="56150-151">Jaké obrazy klientů lze použít a nasadit v Azure a jak je tooI získat?</span><span class="sxs-lookup"><span data-stu-id="56150-151">What client images can I use and deploy in Azure, and how tooI get them?</span></span>

<span data-ttu-id="56150-152">Můžete použít systém Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování za předpokladu, že máte příslušné předplatné sady Visual Studio (dříve MSDN).</span><span class="sxs-lookup"><span data-stu-id="56150-152">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios provided you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> 

- <span data-ttu-id="56150-153">Bitové kopie systému Windows 10 jsou dostupné z Galerie Azure hello v rámci [nabízí vhodné pro vývoj/testování](client-images.md#eligible-offers).</span><span class="sxs-lookup"><span data-stu-id="56150-153">Windows 10 images are available from hello Azure Gallery within [eligible dev/test offers](client-images.md#eligible-offers).</span></span> 
- <span data-ttu-id="56150-154">Visual Studio odběratele v rámci libovolného typu nabídky můžete také [adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a pak [nahrát tooAzure](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="56150-154">Visual Studio subscribers within any type of offer can also [adequately prepare and create](prepare-for-upload-vhd-image.md) a 64-bit Windows 7, Windows 8, or Windows 10 image and then [upload tooAzure](upload-generalized-managed.md).</span></span> <span data-ttu-id="56150-155">použití Hello zůstane omezené toodev a testovací odběratelům active Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56150-155">hello use remains limited toodev/test by active Visual Studio subscribers.</span></span>

<span data-ttu-id="56150-156">To [článku](client-images.md) jsou podrobněji popsány dále hello způsobilosti požadavky pro spuštění klienta systému Windows Azure a použít Dobrý den Image Azure Gallery.</span><span class="sxs-lookup"><span data-stu-id="56150-156">This [article](client-images.md) outlines hello eligibility requirements for running Windows client in Azure and use of hello Azure Gallery images.</span></span>

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="56150-157">Nejsem řady možné toosee velikost virtuálního počítače, který chci, aby při změně velikosti virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="56150-157">I am not able toosee VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="56150-158">Když je spuštěný virtuální počítač, je nasazené tooa fyzický server.</span><span class="sxs-lookup"><span data-stu-id="56150-158">When a VM is running, it is deployed tooa physical server.</span></span> <span data-ttu-id="56150-159">Hello fyzické servery v oblastech Azure jsou seskupené v clusterech běžné fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="56150-159">hello physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="56150-160">Změna velikosti virtuálního počítače, který vyžaduje hello virtuální počítač přesunout toobe toodifferent hardwaru clustery se liší v závislosti na modelu nasazení bylo použité toodeploy hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56150-160">Resizing a VM that requires hello VM toobe moved toodifferent hardware clusters is different depending on which deployment model was used toodeploy hello VM.</span></span>

- <span data-ttu-id="56150-161">Virtuální počítače nasazené v modelu nasazení Classic, nasazení hello cloudové služby musí být odebrány a znovu nasadit toochange hello virtuální počítače tooa velikost v jiné rodiny velikost.</span><span class="sxs-lookup"><span data-stu-id="56150-161">VMs deployed in Classic deployment model, hello cloud service deployment must be removed and redeployed toochange hello VMs tooa size in another size family.</span></span>

- <span data-ttu-id="56150-162">Virtuální počítače nasazené v modelu nasazení Resource Manager, je potřeba zastavit všechny virtuální počítače ve skupině před změnou velikosti hello žádné virtuální počítače v sadě dostupnosti hello dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="56150-162">VMs deployed in Resource Manager deployment model, you must stop all VMs in hello availability set before changing hello size of any VM in hello availability set.</span></span>

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="56150-163">Hello uvedené velikost virtuálního počítače není podporován při nasazení ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="56150-163">hello listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="56150-164">Zvolte velikost, která je podporována v nastavení dostupnosti hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="56150-164">Choose a size that is supported on hello availability set's cluster.</span></span> <span data-ttu-id="56150-165">Při vytváření doporučujeme, že sadu dostupnosti toochoose hello největší velikost virtuálního počítače, které se domníváte, že je třeba, a mít které bude vaše první toohello nasazení, které skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="56150-165">It is recommended when creating an availability set toochoose hello largest VM size you think you need, and have that be your first deployment toohello Availability set.</span></span>

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a><span data-ttu-id="56150-166">Můžete přidat stávající sadu dostupnosti tooan Classic virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="56150-166">Can I add an existing Classic VM tooan availability set?</span></span>

<span data-ttu-id="56150-167">Ano.</span><span class="sxs-lookup"><span data-stu-id="56150-167">Yes.</span></span> <span data-ttu-id="56150-168">Můžete přidat existující klasické virtuální počítač tooa nové nebo stávající sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="56150-168">You can add an existing classic VM tooa new or existing Availability Set.</span></span> <span data-ttu-id="56150-169">Další informace najdete v části [přidat stávající sadu dostupnosti virtuálního počítače tooan](classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="56150-169">For more information see [Add an existing virtual machine tooan availability set](classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="56150-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56150-170">Next steps</span></span>
<span data-ttu-id="56150-171">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="56150-171">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="56150-172">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="56150-172">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="56150-173">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="56150-173">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
