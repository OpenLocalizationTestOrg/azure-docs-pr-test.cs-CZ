---
title: "Vytvoření sady škálování virtuálního počítače pomocí portálu Azure | Microsoft Docs"
description: "Nasazení sad škálování pomocí portálu Azure."
keywords: "Sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7157a429829974b45dad29ac53fb5fb46c71f821
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a><span data-ttu-id="f3828-104">Jak vytvořit sadu škálování virtuálního počítače pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f3828-104">How to create a Virtual Machine Scale Set with the Azure portal</span></span>
<span data-ttu-id="f3828-105">Tento kurz ukazuje, jak je snadné vytvořit sadu škálování virtuálního počítače v několika málo minut pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f3828-105">This tutorial shows you how easy it is to create a Virtual Machine Scale Set in just a few minutes, by using the Azure portal.</span></span> <span data-ttu-id="f3828-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="f3828-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="choose-the-vm-image-from-the-marketplace"></a><span data-ttu-id="f3828-107">Volba image virtuálního počítače z Marketplace</span><span class="sxs-lookup"><span data-stu-id="f3828-107">Choose the VM image from the marketplace</span></span>
<span data-ttu-id="f3828-108">Z portálu můžete snadno nasadit škálování s CentOS, CoreOS, Debian, otevřete Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server nebo bitové kopie systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f3828-108">From the portal, you can easily deploy a scale set with CentOS, CoreOS, Debian, Open Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server, or Windows Server images.</span></span>

<span data-ttu-id="f3828-109">První, přejděte na [portál Azure](https://portal.azure.com) ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f3828-109">First, navigate to the [Azure portal](https://portal.azure.com) in a web browser.</span></span> <span data-ttu-id="f3828-110">Klikněte na tlačítko `New`, vyhledejte `scale set`a pak vyberte `Virtual machine scale set` položku:</span><span class="sxs-lookup"><span data-stu-id="f3828-110">Click `New`, search for `scale set`, and then select the `Virtual machine scale set` entry:</span></span>

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-scale-set"></a><span data-ttu-id="f3828-112">Vytvoření škálovací sady</span><span class="sxs-lookup"><span data-stu-id="f3828-112">Create the scale set</span></span>
<span data-ttu-id="f3828-113">Teď můžete použít výchozí nastavení a rychle vytvořit byly sadou škálování.</span><span class="sxs-lookup"><span data-stu-id="f3828-113">Now you can use the default settings and quickly create the scale set.</span></span>

* <span data-ttu-id="f3828-114">Na `Basics` okno, zadejte název pro sadu škálování.</span><span class="sxs-lookup"><span data-stu-id="f3828-114">On the `Basics` blade, enter a name for the scale set.</span></span> <span data-ttu-id="f3828-115">Tento název bude základem plně kvalifikovaný název domény služby Vyrovnávání zatížení před byly sadou škálování, proto se ujistěte, že název je jedinečný všechny Azure.</span><span class="sxs-lookup"><span data-stu-id="f3828-115">This name becomes the base of the FQDN of the load balancer in front of the scale set, so make sure the name is unique across all Azure.</span></span>
* <span data-ttu-id="f3828-116">Vyberte požadovanou operačním systému zadejte, zadejte své požadované uživatelské jméno a vyberte typ ověřování, které dávají přednost.</span><span class="sxs-lookup"><span data-stu-id="f3828-116">Select your desired OS type, enter your desired username, and select which authentication type you prefer.</span></span> <span data-ttu-id="f3828-117">Pokud si zvolíte heslo, musí být nejméně 12 znaků dlouhé a splňovat tři ze čtyř následujících složitost: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak.</span><span class="sxs-lookup"><span data-stu-id="f3828-117">If you choose a password, it must be at least 12 characters long and meet three out of the four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="f3828-118">Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="f3828-118">See more about [username and password requirements](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span></span> <span data-ttu-id="f3828-119">Pokud se rozhodnete `SSH public key`, nezapomeňte pouze vložení v veřejný klíč, ne privátní klíč:</span><span class="sxs-lookup"><span data-stu-id="f3828-119">If you choose `SSH public key`, be sure to only paste in your public key, NOT your private key:</span></span>

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* <span data-ttu-id="f3828-121">Zvolte, zda chcete omezit sad do jednoho umístění skupiny škálování nebo jestli pokrývat více skupin pro umístění.</span><span class="sxs-lookup"><span data-stu-id="f3828-121">Choose whether you would like to limit the scale set to a single placement group or whether it should span multiple placement groups.</span></span> <span data-ttu-id="f3828-122">Což sad rozložit umístění skupiny škálování umožňuje škálování nastaví více než 100 virtuálních počítačů v kapacity (až 1 000) s některými omezeními.</span><span class="sxs-lookup"><span data-stu-id="f3828-122">Allowing the scale set to span placement groups allows for scale sets over 100 VMs in capacity (up to 1,000) with certain limitations.</span></span> <span data-ttu-id="f3828-123">Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-placement-groups.md).</span><span class="sxs-lookup"><span data-stu-id="f3828-123">For more information, see [this documentation](./virtual-machine-scale-sets-placement-groups.md).</span></span>
* <span data-ttu-id="f3828-124">Zadejte název skupiny požadovaných prostředků a umístění a pak klikněte na tlačítko `OK`.</span><span class="sxs-lookup"><span data-stu-id="f3828-124">Enter your desired resource group name and location, and then click `OK`.</span></span>
* <span data-ttu-id="f3828-125">Na `Virtual machine scale set service settings` okno: Zadejte vaše Popisek názvu domény požadované (základ plně kvalifikovaný název domény pro vyrovnávání zatížení před byly sadou škálování).</span><span class="sxs-lookup"><span data-stu-id="f3828-125">On the `Virtual machine scale set service settings` blade: enter your desired domain name label (the base of the FQDN for the load balancer in front of the scale set).</span></span> <span data-ttu-id="f3828-126">Tento popisek musí být jedinečný v rámci všech Azure.</span><span class="sxs-lookup"><span data-stu-id="f3828-126">This label must be unique across all Azure.</span></span>
* <span data-ttu-id="f3828-127">Zvolte si image disku požadovaný operační systém, počet instancí a velikost počítače.</span><span class="sxs-lookup"><span data-stu-id="f3828-127">Choose your desired operating system disk image, instance count, and machine size.</span></span>
* <span data-ttu-id="f3828-128">Vyberte typ požadovaný disk: spravované nebo nespravované.</span><span class="sxs-lookup"><span data-stu-id="f3828-128">Choose your desired disk type: managed or unmanaged.</span></span> <span data-ttu-id="f3828-129">Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-managed-disks.md).</span><span class="sxs-lookup"><span data-stu-id="f3828-129">For more information, see [this documentation](./virtual-machine-scale-sets-managed-disks.md).</span></span> <span data-ttu-id="f3828-130">Pokud jste vybrali možnost sad škálování rozmístěny v několika skupinách pro umístění, tato možnost nebude dostupná, protože je vyžadován pro sady škálování rozložit umístění skupin spravovaných disků.</span><span class="sxs-lookup"><span data-stu-id="f3828-130">If you chose to have the scale set span multiple placement groups, this option will not be available because managed disk is required for scale sets to span placement groups.</span></span>
* <span data-ttu-id="f3828-131">Povolit nebo zakázat automatické škálování a nakonfigurujte, pokud je povoleno:</span><span class="sxs-lookup"><span data-stu-id="f3828-131">Enable or disable autoscale and configure if enabled:</span></span>

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* <span data-ttu-id="f3828-133">Na `Summary` okno, pokud se provádí ověřování, klikněte na tlačítko `OK` zahájíte měřítka nastavit nasazení.</span><span class="sxs-lookup"><span data-stu-id="f3828-133">On the `Summary` blade, when validation is done, click `OK` to start the scale set deployment.</span></span>


## <a name="connect-to-a-vm-in-the-scale-set"></a><span data-ttu-id="f3828-134">Připojit k virtuálnímu počítači v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="f3828-134">Connect to a VM in the scale set</span></span>
<span data-ttu-id="f3828-135">Pokud jste zvolili pro omezení vaší škálování nastavit do jednoho umístění skupiny, pak byly sadou škálování se nasadí s nakonfigurovaná na umožňují připojit se ke stupnici snadno nastavit (v opačném případě se připojit k virtuálním počítačům v sadě škálování, je pravděpodobně potřeba vytvořit jumpbox ve stejné pravidla NAT  virtuální síť jako sada škálování).</span><span class="sxs-lookup"><span data-stu-id="f3828-135">If you chose to limit your scale set to a single placement group, then the scale set is deployed with NAT rules configured to let you connect to the scale set easily (if not, to connect to the virtual machines in the scale set, you likely need to create a jumpbox in the same virtual network as the scale set).</span></span> <span data-ttu-id="f3828-136">Chcete-li vidět, přejděte na `Inbound NAT Rules` kartě Nástroje pro vyrovnávání zatížení pro škálovací sadu:</span><span class="sxs-lookup"><span data-stu-id="f3828-136">To see them, navigate to the `Inbound NAT Rules` tab of the load balancer for the scale set:</span></span>

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

<span data-ttu-id="f3828-138">Pro každý virtuální počítač ve škálovací nastavit pomocí těchto pravidel NAT můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="f3828-138">You can connect to each VM in the scale set using these NAT rules.</span></span> <span data-ttu-id="f3828-139">Pro instanci pro sadu škálování systému Windows, pokud je pravidlo NAT na příchozí port 50000, můžete se připojit k tomuto počítači pomocí protokolu RDP na `<load-balancer-ip-address>:50000`.</span><span class="sxs-lookup"><span data-stu-id="f3828-139">For instance, for a Windows scale set, if there is a NAT rule on incoming port 50000, you could connect to that machine via RDP on `<load-balancer-ip-address>:50000`.</span></span> <span data-ttu-id="f3828-140">Pro sadu škálování Linux byste připojili pomocí příkazu `ssh -p 50000 <username>@<load-balancer-ip-address>`.</span><span class="sxs-lookup"><span data-stu-id="f3828-140">For a Linux scale set, you would connect using the command `ssh -p 50000 <username>@<load-balancer-ip-address>`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3828-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3828-141">Next steps</span></span>
<span data-ttu-id="f3828-142">Dokumentace k nasazení škálování nastaví z rozhraní příkazového řádku, najdete v části [této dokumentace](virtual-machine-scale-sets-cli-quick-create.md).</span><span class="sxs-lookup"><span data-stu-id="f3828-142">For documentation on how to deploy scale sets from the CLI, see [this documentation](virtual-machine-scale-sets-cli-quick-create.md).</span></span>

<span data-ttu-id="f3828-143">Dokumentace k nasazení škálování nastaví z prostředí PowerShell, najdete v části [této dokumentace](virtual-machine-scale-sets-windows-create.md).</span><span class="sxs-lookup"><span data-stu-id="f3828-143">For documentation on how to deploy scale sets from PowerShell, see [this documentation](virtual-machine-scale-sets-windows-create.md).</span></span>

<span data-ttu-id="f3828-144">Dokumentace k nasazení škálování nastaví ze sady Visual Studio, najdete v části [této dokumentace](virtual-machine-scale-sets-vs-create.md).</span><span class="sxs-lookup"><span data-stu-id="f3828-144">For documentation on how to deploy scale sets from Visual Studio, see [this documentation](virtual-machine-scale-sets-vs-create.md).</span></span>

<span data-ttu-id="f3828-145">Pro obecnou dokumentací, podívejte se [stránky přehled dokumentace pro sady škálování](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3828-145">For general documentation, check out the [documentation overview page for scale sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="f3828-146">Obecné informace, podívejte se [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="f3828-146">For general information, check out the [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

