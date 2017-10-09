---
title: "aaaTroubleshoot nasazení problémy Linux virtuálního počítače v Azure | Microsoft Docs"
description: "Řešení potíží nasazení Linux virtuálního počítače v modelu nasazení Azurethe Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
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
ms.openlocfilehash: d1092ca3d9d51af7510b19c8c9a79e6dda588697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a><span data-ttu-id="2e742-103">Problémy s nasazení Linux virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="2e742-103">Troubleshoot deploying Linux virtual machine issues in Azure</span></span>

<span data-ttu-id="2e742-104">problémy při nasazení virtuálního počítače (VM) tootroubleshoot v Azure, zkontrolujte hello [top problémy](#top-issues) pro běžné chyby a řešení.</span><span class="sxs-lookup"><span data-stu-id="2e742-104">tootroubleshoot virtual machine (VM) deployment issues in Azure, review hello [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="2e742-105">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="2e742-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="2e742-106">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="2e742-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="2e742-107">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="2e742-107">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="2e742-108">Nejdůležitější problémy</span><span class="sxs-lookup"><span data-stu-id="2e742-108">Top issues</span></span>
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a><span data-ttu-id="2e742-109">Hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2e742-109">hello cluster cannot support hello requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="2e742-110">Opakujte žádost hello pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e742-110">Retry hello request using a smaller VM size.</span></span>
- <span data-ttu-id="2e742-111">Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="2e742-111">If hello size of hello requested VM cannot be changed:</span></span>
    - <span data-ttu-id="2e742-112">Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="2e742-112">Stop all hello VMs in hello availability set.</span></span> <span data-ttu-id="2e742-113">Klikněte na tlačítko **skupiny prostředků** > vaší skupiny prostředků > **prostředky** > vaše skupina dostupnosti > **virtuální počítače** > virtuálního počítače > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="2e742-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="2e742-114">Po všech hello zastavit virtuální počítače, vytvořte hello virtuálních počítačů v hello potřeby velikost.</span><span class="sxs-lookup"><span data-stu-id="2e742-114">After all hello VMs stop, create hello VM in hello desired size.</span></span>
    - <span data-ttu-id="2e742-115">Spusťte nejprve hello nový virtuální počítač a potom vyberete jednotlivé hello zastaví virtuální počítače a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="2e742-115">Start hello new VM first, and then select each of hello stopped VMs and click Start.</span></span>


## <a name="hello-cluster-does-not-have-free-resources"></a><span data-ttu-id="2e742-116">Hello cluster nebude mít uvolnění prostředků</span><span class="sxs-lookup"><span data-stu-id="2e742-116">hello cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="2e742-117">Opakujte žádost hello později.</span><span class="sxs-lookup"><span data-stu-id="2e742-117">Retry hello request later.</span></span>
- <span data-ttu-id="2e742-118">Pokud můžete technologie hello nový virtuální počítač součástí jiné dostupnosti nastavit</span><span class="sxs-lookup"><span data-stu-id="2e742-118">If hello new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="2e742-119">Vytvoření virtuálního počítače v sadě dostupnosti jinou (v hello stejné oblasti).</span><span class="sxs-lookup"><span data-stu-id="2e742-119">Create a VM in a different availability set (in hello same region).</span></span>
    - <span data-ttu-id="2e742-120">Přidat nový virtuální počítač toohello hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="2e742-120">Add hello new VM toohello same virtual network.</span></span>

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="2e742-121">Jak lze aktivovat moje měsíční kredit pro sadu Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="2e742-121">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="2e742-122">tooactivate vaše měsíční úvěrové, najdete [článku](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="2e742-122">tooactivate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="why-can-i-not-install-hello-gpu-driver-for-an-ubuntu-nv-vm"></a><span data-ttu-id="2e742-123">Proč nelze nainstalovat ovladač GPU hello pro virtuálního počítače s Ubuntu vs?</span><span class="sxs-lookup"><span data-stu-id="2e742-123">Why can I not install hello GPU driver for an Ubuntu NV VM?</span></span>

<span data-ttu-id="2e742-124">V současné době Linux GPU podpora je k dispozici pouze na virtuálních počítačích Azure NC systémem Ubuntu Server 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="2e742-124">Currently, Linux GPU support is only available on Azure NC VMs running Ubuntu Server 16.04 LTS.</span></span> <span data-ttu-id="2e742-125">Další informace najdete v tématu [nastavení GPU ovladače pro N-series virtuální počítače se systémem Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="2e742-125">For more information, see [Set up GPU drivers for N-series VMs running Linux](n-series-driver-setup.md).</span></span>

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a><span data-ttu-id="2e742-126">Moje ovladače chybí pro virtuální počítač Linux N-Series</span><span class="sxs-lookup"><span data-stu-id="2e742-126">My drivers are missing for my Linux N-Series VM</span></span>

<span data-ttu-id="2e742-127">Ovladače pro virtuální počítače na bázi systému Linux jsou umístěné [zde](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="2e742-127">Drivers for Linux-based VMs are located [here](n-series-driver-setup.md).</span></span> 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="2e742-128">Nelze najít instanci GPU v rámci virtuální počítač N-Series</span><span class="sxs-lookup"><span data-stu-id="2e742-128">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="2e742-129">tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, je nutné nainstalovat NVIDIA grafické ovladače pro každý virtuální počítač po nasazení.</span><span class="sxs-lookup"><span data-stu-id="2e742-129">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="2e742-130">Informace o instalaci ovladačů je k dispozici pro [virtuálních počítačů Windows](../windows/n-series-driver-setup.md) a [virtuální počítače s Linuxem](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="2e742-130">Driver setup information is available for [Windows VMs](../windows/n-series-driver-setup.md) and [Linux VMs](n-series-driver-setup.md).</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="2e742-131">Je k dispozici N-Series virtuálních počítačů v mojí oblasti?</span><span class="sxs-lookup"><span data-stu-id="2e742-131">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="2e742-132">Můžete zkontrolovat dostupnost hello z hello [produkty, které jsou dostupné podle oblasti tabulku](https://azure.microsoft.com/regions/services)a ceny [zde](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="2e742-132">You can check hello availability from hello [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="2e742-133">Nejsem řady možné toosee velikost virtuálního počítače, který chci, aby při změně velikosti virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2e742-133">I am not able toosee VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="2e742-134">Když je spuštěný virtuální počítač, je nasazené tooa fyzický server.</span><span class="sxs-lookup"><span data-stu-id="2e742-134">When a VM is running, it is deployed tooa physical server.</span></span> <span data-ttu-id="2e742-135">Hello fyzické servery v oblastech Azure jsou seskupené v clusterech běžné fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="2e742-135">hello physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="2e742-136">Změna velikosti virtuálního počítače, který vyžaduje hello virtuální počítač přesunout toobe toodifferent hardwaru clustery se liší v závislosti na modelu nasazení bylo použité toodeploy hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e742-136">Resizing a VM that requires hello VM toobe moved toodifferent hardware clusters is different depending on which deployment model was used toodeploy hello VM.</span></span>

- <span data-ttu-id="2e742-137">Virtuální počítače nasazené v modelu nasazení Classic, nasazení hello cloudové služby musí být odebrány a znovu nasadit toochange hello virtuální počítače tooa velikost v jiné rodiny velikost.</span><span class="sxs-lookup"><span data-stu-id="2e742-137">VMs deployed in Classic deployment model, hello cloud service deployment must be removed and redeployed toochange hello VMs tooa size in another size family.</span></span>

- <span data-ttu-id="2e742-138">Virtuální počítače nasazené v modelu nasazení Resource Manager, je potřeba zastavit všechny virtuální počítače ve skupině před změnou velikosti hello žádné virtuální počítače v sadě dostupnosti hello dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="2e742-138">VMs deployed in Resource Manager deployment model, you must stop all VMs in hello availability set before changing hello size of any VM in hello availability set.</span></span>

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="2e742-139">Hello uvedené velikost virtuálního počítače není podporován při nasazení ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2e742-139">hello listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="2e742-140">Zvolte velikost, která je podporována v nastavení dostupnosti hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="2e742-140">Choose a size that is supported on hello availability set's cluster.</span></span> <span data-ttu-id="2e742-141">Při vytváření doporučujeme, že sadu dostupnosti toochoose hello největší velikost virtuálního počítače, které se domníváte, že je třeba, a mít které bude vaše první toohello nasazení, které skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2e742-141">It is recommended when creating an availability set toochoose hello largest VM size you think you need, and have that be your first deployment toohello Availability set.</span></span>

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a><span data-ttu-id="2e742-142">Jaké distribucí Linux nebo verze jsou podporovány v Azure?</span><span class="sxs-lookup"><span data-stu-id="2e742-142">What Linux distributions/versions are supported on Azure?</span></span>

<span data-ttu-id="2e742-143">Seznam hello v Linux najdete na [Azure-Endorsed distribuce](endorsed-distros.md).</span><span class="sxs-lookup"><span data-stu-id="2e742-143">You can find hello list at Linux on [Azure-Endorsed Distributions](endorsed-distros.md).</span></span>

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a><span data-ttu-id="2e742-144">Můžete přidat stávající sadu dostupnosti tooan Classic virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="2e742-144">Can I add an existing Classic VM tooan availability set?</span></span>

<span data-ttu-id="2e742-145">Ano.</span><span class="sxs-lookup"><span data-stu-id="2e742-145">Yes.</span></span> <span data-ttu-id="2e742-146">Můžete přidat existující klasické virtuální počítač tooa nové nebo stávající sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2e742-146">You can add an existing classic VM tooa new or existing Availability Set.</span></span> <span data-ttu-id="2e742-147">Další informace najdete v části [přidat stávající sadu dostupnosti virtuálního počítače tooan](../windows/classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="2e742-147">For more information see [Add an existing virtual machine tooan availability set](../windows/classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="2e742-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e742-148">Next steps</span></span>
<span data-ttu-id="2e742-149">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="2e742-149">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="2e742-150">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="2e742-150">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="2e742-151">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="2e742-151">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
