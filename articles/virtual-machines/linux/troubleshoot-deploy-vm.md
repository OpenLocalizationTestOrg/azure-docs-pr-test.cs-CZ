---
title: "Problémy s nasazení Linux virtuálního počítače v Azure | Microsoft Docs"
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
ms.openlocfilehash: 8bc9de90a49caf9155073caaa160585c6cc3728b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a><span data-ttu-id="bc9e4-103">Problémy s nasazení Linux virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="bc9e4-103">Troubleshoot deploying Linux virtual machine issues in Azure</span></span>

<span data-ttu-id="bc9e4-104">Chcete-li vyřešit problémy při nasazení virtuálního počítače (VM) v Azure, zkontrolujte [top problémy](#top-issues) pro běžné chyby a řešení.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-104">To troubleshoot virtual machine (VM) deployment issues in Azure, review the [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="bc9e4-105">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-105">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="bc9e4-106">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="bc9e4-107">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-107">Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="bc9e4-108">Nejdůležitější problémy</span><span class="sxs-lookup"><span data-stu-id="bc9e4-108">Top issues</span></span>
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a><span data-ttu-id="bc9e4-109">Cluster nemůže podporovat požadovaná velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bc9e4-109">The cluster cannot support the requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="bc9e4-110">Opakujte tuto žádost pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-110">Retry the request using a smaller VM size.</span></span>
- <span data-ttu-id="bc9e4-111">Pokud velikost požadovaný virtuální počítač nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="bc9e4-111">If the size of the requested VM cannot be changed:</span></span>
    - <span data-ttu-id="bc9e4-112">Zastavte všechny virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-112">Stop all the VMs in the availability set.</span></span> <span data-ttu-id="bc9e4-113">Klikněte na tlačítko **skupiny prostředků** > vaší skupiny prostředků > **prostředky** > vaše skupina dostupnosti > **virtuální počítače** > virtuálního počítače > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="bc9e4-114">Po zastavit všechny virtuální počítače, vytvořte v požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-114">After all the VMs stop, create the VM in the desired size.</span></span>
    - <span data-ttu-id="bc9e4-115">Nejprve spusťte nový virtuální počítač a vyberte jednotlivé zastaven virtuálních počítačů a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-115">Start the new VM first, and then select each of the stopped VMs and click Start.</span></span>


## <a name="the-cluster-does-not-have-free-resources"></a><span data-ttu-id="bc9e4-116">Cluster nemá uvolnění prostředků</span><span class="sxs-lookup"><span data-stu-id="bc9e4-116">The cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="bc9e4-117">Opakujte požadavek později.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-117">Retry the request later.</span></span>
- <span data-ttu-id="bc9e4-118">Pokud nový virtuální počítač může být součástí skupiny s jinou dostupnosti</span><span class="sxs-lookup"><span data-stu-id="bc9e4-118">If the new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="bc9e4-119">Vytvoření virtuálního počítače v různých dostupnosti nastavit (ve stejné oblasti).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-119">Create a VM in a different availability set (in the same region).</span></span>
    - <span data-ttu-id="bc9e4-120">Přidáte nový virtuální počítač do stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-120">Add the new VM to the same virtual network.</span></span>

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="bc9e4-121">Jak lze aktivovat moje měsíční kredit pro sadu Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="bc9e4-121">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="bc9e4-122">Aktivace měsíčního kreditu, najdete v části to [článku](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-122">To activate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a><span data-ttu-id="bc9e4-123">Proč nelze nainstalovat ovladač GPU pro virtuálního počítače s Ubuntu vs?</span><span class="sxs-lookup"><span data-stu-id="bc9e4-123">Why can I not install the GPU driver for an Ubuntu NV VM?</span></span>

<span data-ttu-id="bc9e4-124">V současné době Linux GPU podpora je k dispozici pouze na virtuálních počítačích Azure NC systémem Ubuntu Server 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-124">Currently, Linux GPU support is only available on Azure NC VMs running Ubuntu Server 16.04 LTS.</span></span> <span data-ttu-id="bc9e4-125">Další informace najdete v tématu [nastavení GPU ovladače pro N-series virtuální počítače se systémem Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-125">For more information, see [Set up GPU drivers for N-series VMs running Linux](n-series-driver-setup.md).</span></span>

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a><span data-ttu-id="bc9e4-126">Moje ovladače chybí pro virtuální počítač Linux N-Series</span><span class="sxs-lookup"><span data-stu-id="bc9e4-126">My drivers are missing for my Linux N-Series VM</span></span>

<span data-ttu-id="bc9e4-127">Ovladače pro virtuální počítače na bázi systému Linux jsou umístěné [zde](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-127">Drivers for Linux-based VMs are located [here](n-series-driver-setup.md).</span></span> 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="bc9e4-128">Nelze najít instanci GPU v rámci virtuální počítač N-Series</span><span class="sxs-lookup"><span data-stu-id="bc9e4-128">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="bc9e4-129">Abyste mohli využívat možnosti GPU Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, je nutné nainstalovat NVIDIA grafické ovladače pro každý virtuální počítač po nasazení.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-129">To take advantage of the GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="bc9e4-130">Informace o instalaci ovladačů je k dispozici pro [virtuálních počítačů Windows](../windows/n-series-driver-setup.md) a [virtuální počítače s Linuxem](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-130">Driver setup information is available for [Windows VMs](../windows/n-series-driver-setup.md) and [Linux VMs](n-series-driver-setup.md).</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="bc9e4-131">Je k dispozici N-Series virtuálních počítačů v mojí oblasti?</span><span class="sxs-lookup"><span data-stu-id="bc9e4-131">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="bc9e4-132">Můžete zkontrolovat dostupnost z [produkty, které jsou dostupné podle oblasti tabulku](https://azure.microsoft.com/regions/services)a ceny [zde](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-132">You can check the availability from the [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="bc9e4-133">Nejsem moci zobrazit rodiny velikost virtuálního počítače, které chcete při změně velikosti virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-133">I am not able to see VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="bc9e4-134">Když je spuštěný virtuální počítač, je nasazen na fyzickém serveru.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-134">When a VM is running, it is deployed to a physical server.</span></span> <span data-ttu-id="bc9e4-135">Fyzické servery v oblastech Azure jsou seskupené v clusterech běžné fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-135">The physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="bc9e4-136">Změna velikosti virtuálních počítačů, které vyžaduje virtuální počítač se přesunul na jiný hardware clustery se liší v závislosti na modelu nasazení byl použit k nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-136">Resizing a VM that requires the VM to be moved to different hardware clusters is different depending on which deployment model was used to deploy the VM.</span></span>

- <span data-ttu-id="bc9e4-137">Virtuální počítače nasazené v modelu nasazení Classic, nasazení cloudové služby musí být odebrány a znovu nasadit změnit virtuální počítače na velikost v jiné rodiny velikost.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-137">VMs deployed in Classic deployment model, the cloud service deployment must be removed and redeployed to change the VMs to a size in another size family.</span></span>

- <span data-ttu-id="bc9e4-138">Virtuální počítače nasazené v modelu nasazení Resource Manager, je potřeba zastavit všechny virtuální počítače ve skupině před změnou velikosti žádné virtuální počítače v sadě dostupnosti dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-138">VMs deployed in Resource Manager deployment model, you must stop all VMs in the availability set before changing the size of any VM in the availability set.</span></span>

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="bc9e4-139">Uvedené velikosti virtuálního počítače není podporován při nasazení ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-139">The listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="bc9e4-140">Zvolte velikost, která je podporována v sadě dostupnosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-140">Choose a size that is supported on the availability set's cluster.</span></span> <span data-ttu-id="bc9e4-141">Doporučujeme při vytváření dostupnosti nastavit zvolit největší velikost virtuálního počítače, které se domníváte, že je třeba a mít které bude první nasazení do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-141">It is recommended when creating an availability set to choose the largest VM size you think you need, and have that be your first deployment to the Availability set.</span></span>

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a><span data-ttu-id="bc9e4-142">Jaké distribucí Linux nebo verze jsou podporovány v Azure?</span><span class="sxs-lookup"><span data-stu-id="bc9e4-142">What Linux distributions/versions are supported on Azure?</span></span>

<span data-ttu-id="bc9e4-143">V seznamu na Linux můžete najít na [Azure-Endorsed distribuce](endorsed-distros.md).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-143">You can find the list at Linux on [Azure-Endorsed Distributions](endorsed-distros.md).</span></span>

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a><span data-ttu-id="bc9e4-144">Můžete přidat existující klasické virtuální počítač na sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="bc9e4-144">Can I add an existing Classic VM to an availability set?</span></span>

<span data-ttu-id="bc9e4-145">Ano.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-145">Yes.</span></span> <span data-ttu-id="bc9e4-146">Existující klasické virtuální počítač můžete přidat do nový nebo existující skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-146">You can add an existing classic VM to a new or existing Availability Set.</span></span> <span data-ttu-id="bc9e4-147">Další informace najdete v části [přidat existující virtuální počítač do skupiny dostupnosti](../windows/classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-147">For more information see [Add an existing virtual machine to an availability set](../windows/classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="bc9e4-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc9e4-148">Next steps</span></span>
<span data-ttu-id="bc9e4-149">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="bc9e4-149">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="bc9e4-150">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-150">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="bc9e4-151">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="bc9e4-151">Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
