---
title: "Vytvoření virtuální síti Azure partnerského vztahu – Resource Manager - stejného předplatného. | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků, které existují ve stejném předplatném Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: a32a6b33e04c603325ab3612f61e5852682eac7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a><span data-ttu-id="dbd75-103">Vytvoření virtuální sítě partnerský vztah – Resource Manager, stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="dbd75-103">Create a virtual network peering - Resource Manager, same subscription</span></span>

<span data-ttu-id="dbd75-104">V tomto kurzu zjistíte vytvořit virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="dbd75-104">In this tutorial, you learn to create a virtual network peering between virtual networks created through Resource Manager.</span></span> <span data-ttu-id="dbd75-105">Obě virtuální sítě existovat ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="dbd75-105">Both virtual networks exist in the same subscription.</span></span> <span data-ttu-id="dbd75-106">Partnerský vztah dva prostředky umožňuje virtuální sítě v různých virtuálních sítích ke komunikaci mezi sebou stejným šířky pásma a latence, jako by byl prostředky ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="dbd75-106">Peering two virtual networks enables resources in different virtual networks to communicate with each other with the same bandwidth and latency as though the resources were in the same virtual network.</span></span> <span data-ttu-id="dbd75-107">Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dbd75-107">Learn more about [Virtual network peering](virtual-network-peering-overview.md).</span></span> 

<span data-ttu-id="dbd75-108">Postup vytvoření virtuální sítě partnerského vztahu se liší v závislosti na tom, jestli virtuální sítě jsou ve stejné nebo jiné, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuální sítě se vytvářejí pomocí.</span><span class="sxs-lookup"><span data-stu-id="dbd75-108">The steps to create a virtual network peering are different, depending on whether the virtual networks are in the same, or different, subscriptions, and which [Azure deployment model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) the virtual networks are created through.</span></span> <span data-ttu-id="dbd75-109">Naučte se vytvořit virtuální síť partnerský vztah v jiných scénářích kliknutím na scénář z v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="dbd75-109">Learn how to create a virtual network peering in other scenarios by clicking the scenario from the following table:</span></span>

|<span data-ttu-id="dbd75-110">Model nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="dbd75-110">Azure deployment model</span></span>  | <span data-ttu-id="dbd75-111">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="dbd75-111">Azure subscription</span></span>  |
|--------- |---------|
|[<span data-ttu-id="dbd75-112">I Resource Manager</span><span class="sxs-lookup"><span data-stu-id="dbd75-112">Both Resource Manager</span></span>](create-peering-different-subscriptions.md) |<span data-ttu-id="dbd75-113">Různé</span><span class="sxs-lookup"><span data-stu-id="dbd75-113">Different</span></span>|
|[<span data-ttu-id="dbd75-114">Jeden Resource Manager, jeden classic</span><span class="sxs-lookup"><span data-stu-id="dbd75-114">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models.md) |<span data-ttu-id="dbd75-115">stejné</span><span class="sxs-lookup"><span data-stu-id="dbd75-115">Same</span></span>|
|[<span data-ttu-id="dbd75-116">Jeden Resource Manager, jeden classic</span><span class="sxs-lookup"><span data-stu-id="dbd75-116">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models-subscriptions.md) |<span data-ttu-id="dbd75-117">Různé</span><span class="sxs-lookup"><span data-stu-id="dbd75-117">Different</span></span>|

<span data-ttu-id="dbd75-118">Virtuální síť partnerský vztah nelze vytvořit mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="dbd75-118">A virtual network peering cannot be created between two virtual networks deployed through the classic deployment model.</span></span> <span data-ttu-id="dbd75-119">Partnerský vztah virtuální síť lze vytvořit pouze mezi dvěma virtuálními sítěmi, které existují ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="dbd75-119">A virtual network peering can only be created between two virtual networks that exist in the same Azure region.</span></span> <span data-ttu-id="dbd75-120">Pokud potřebujete připojit virtuální sítě, které byly obě vytvořené pomocí modelu nasazení classic nebo které existovat v různých oblastech Azure, můžete použít Azure [brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) připojení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="dbd75-120">If you need to connect virtual networks that were both created through the classic deployment model, or that exist in different Azure regions, you can use an Azure [VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) to connect the virtual networks.</span></span> 

<span data-ttu-id="dbd75-121">Můžete použít [portál Azure](#portal), Azure [rozhraní příkazového řádku](#cli) (CLI) Azure [prostředí PowerShell](#powershell), nebo [šablony Azure Resource Manageru](#template)vytvoření virtuální sítě partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-121">You can use the [Azure portal](#portal), the Azure [command-line interface](#cli) (CLI), Azure [PowerShell](#powershell), or an [Azure Resource Manager template](#template) to create a virtual network peering.</span></span> <span data-ttu-id="dbd75-122">Klikněte na libovolný předchozí odkaz nástroj přejít přímo na kroky pro vytvoření virtuální sítě partnerský vztah nástroji vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="dbd75-122">Click any of the previous tool links to go directly to the steps for creating a virtual network peering using your tool of choice.</span></span>

## <span data-ttu-id="dbd75-123"><a name="portal"></a>Vytvoření partnerského vztahu – portál Azure</span><span class="sxs-lookup"><span data-stu-id="dbd75-123"><a name="portal"></a>Create peering - Azure portal</span></span>

1. <span data-ttu-id="dbd75-124">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dbd75-124">Log in to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="dbd75-125">Účet, ke kterému se přihlásíte, musí mít potřebná oprávnění k vytvoření virtuální sítě partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-125">The account you log in with must have the necessary permissions to create a virtual network peering.</span></span> <span data-ttu-id="dbd75-126">Najdete v článku [oprávnění](#permissions) tohoto článku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="dbd75-126">See the [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="dbd75-127">Klikněte na tlačítko **+ nový**, klikněte na tlačítko **sítě**, pak klikněte na tlačítko **virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-127">Click **+ New**, click **Networking**, then click **Virtual network**.</span></span>
3. <span data-ttu-id="dbd75-128">V **vytvořit virtuální síť** okno, zadejte, nebo vyberte hodnoty pro následující nastavení a potom klikněte na tlačítko **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="dbd75-128">In the **Create virtual network** blade, enter, or select values for the following settings, then click **Create**:</span></span>
    - <span data-ttu-id="dbd75-129">**Název**: *myVnet1*</span><span class="sxs-lookup"><span data-stu-id="dbd75-129">**Name**: *myVnet1*</span></span>
    - <span data-ttu-id="dbd75-130">**Adresní prostor**: *10.0.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="dbd75-130">**Address space**: *10.0.0.0/16*</span></span>
    - <span data-ttu-id="dbd75-131">**Název podsítě**: *výchozí*</span><span class="sxs-lookup"><span data-stu-id="dbd75-131">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="dbd75-132">**Rozsah adres podsítě**: *10.0.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="dbd75-132">**Subnet address range**: *10.0.0.0/24*</span></span>
    - <span data-ttu-id="dbd75-133">**Předplatné**: Vyberte předplatné</span><span class="sxs-lookup"><span data-stu-id="dbd75-133">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="dbd75-134">**Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="dbd75-134">**Resource group**: Select **Create new** and enter *myResourceGroup*</span></span>
    - <span data-ttu-id="dbd75-135">**Umístění**: *východní USA*</span><span class="sxs-lookup"><span data-stu-id="dbd75-135">**Location**: *East US*</span></span>
4. <span data-ttu-id="dbd75-136">Proveďte kroky 2 až 3 znovu zadat následující hodnoty v kroku 3:</span><span class="sxs-lookup"><span data-stu-id="dbd75-136">Complete steps 2-3 again specifying the following values in step 3:</span></span>
    - <span data-ttu-id="dbd75-137">**Název**: *myVnet2*</span><span class="sxs-lookup"><span data-stu-id="dbd75-137">**Name**: *myVnet2*</span></span>
    - <span data-ttu-id="dbd75-138">**Adresní prostor**: *10.1.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="dbd75-138">**Address space**: *10.1.0.0/16*</span></span>
    - <span data-ttu-id="dbd75-139">**Název podsítě**: *výchozí*</span><span class="sxs-lookup"><span data-stu-id="dbd75-139">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="dbd75-140">**Rozsah adres podsítě**: *10.1.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="dbd75-140">**Subnet address range**: *10.1.0.0/24*</span></span>
    - <span data-ttu-id="dbd75-141">**Předplatné**: Vyberte předplatné</span><span class="sxs-lookup"><span data-stu-id="dbd75-141">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="dbd75-142">**Skupina prostředků**: vyberte **použít existující** a vyberte *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="dbd75-142">**Resource group**: Select **Use existing** and select *myResourceGroup*</span></span>
    - <span data-ttu-id="dbd75-143">**Umístění**: *východní USA*</span><span class="sxs-lookup"><span data-stu-id="dbd75-143">**Location**: *East US*</span></span>
5. <span data-ttu-id="dbd75-144">V **vyhledávání prostředků** pole v horní části portálu, typ *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="dbd75-144">In the **Search resources** box at the top of the portal, type *myResourceGroup*.</span></span> <span data-ttu-id="dbd75-145">Klikněte na tlačítko **myResourceGroup** při zobrazí ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="dbd75-145">Click **myResourceGroup** when it appears in the search results.</span></span> <span data-ttu-id="dbd75-146">Zobrazí okno **myresourcegroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="dbd75-146">A blade appears for the **myresourcegroup** resource group.</span></span> <span data-ttu-id="dbd75-147">Skupina prostředků obsahuje dvě virtuální sítě vytvořené v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="dbd75-147">The resource group contains the two virtual networks created in previous steps.</span></span>
6. <span data-ttu-id="dbd75-148">Klikněte na tlačítko **myVNet1**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-148">Click **myVNet1**.</span></span>
7. <span data-ttu-id="dbd75-149">V **myVnet1** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** ze seznamu svislé možností na levé straně okna.</span><span class="sxs-lookup"><span data-stu-id="dbd75-149">In the **myVnet1** blade that appears, click **Peerings** from the vertical list of options on the left side of the blade.</span></span>
8. <span data-ttu-id="dbd75-150">V **myVnet1 - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**</span><span class="sxs-lookup"><span data-stu-id="dbd75-150">In the **myVnet1 - Peerings** blade that appeared, click **+ Add**</span></span>
9. <span data-ttu-id="dbd75-151">V **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte následující možnosti a potom klikněte na tlačítko **OK**:</span><span class="sxs-lookup"><span data-stu-id="dbd75-151">In the **Add peering** blade that appears, enter, or select the following options, then click **OK**:</span></span>
     - <span data-ttu-id="dbd75-152">**Název**: *myVnet1ToMyVnet2*</span><span class="sxs-lookup"><span data-stu-id="dbd75-152">**Name**: *myVnet1ToMyVnet2*</span></span>
     - <span data-ttu-id="dbd75-153">**Virtuální síť modelu nasazení**: vyberte **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-153">**Virtual network deployment model**:  Select **Resource Manager**.</span></span> 
     - <span data-ttu-id="dbd75-154">**Předplatné**: Vyberte předplatné</span><span class="sxs-lookup"><span data-stu-id="dbd75-154">**Subscription**: Select your subscription</span></span>
     - <span data-ttu-id="dbd75-155">**Virtuální síť**: klikněte na tlačítko **vyberte virtuální síť**, pak klikněte na tlačítko **myVnet2**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-155">**Virtual network**:  Click **Choose a virtual network**, then click **myVnet2**.</span></span>
     - <span data-ttu-id="dbd75-156">**Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="dbd75-156">**Allow virtual network access:** Ensure that **Enabled** is selected.</span></span>
    <span data-ttu-id="dbd75-157">Žádné další nastavení použitá v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-157">No other settings are used in this tutorial.</span></span> <span data-ttu-id="dbd75-158">Další informace o všech nastaveních partnerského vztahu, přečtěte si [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="dbd75-158">To learn about all peering settings, read [Manage virtual network peerings](virtual-network-manage-peering.md#create-a-peering).</span></span>
10. <span data-ttu-id="dbd75-159">Po kliknutí na **OK** v předchozím kroku, **partnerský vztah přidat** okno se zavře a zobrazí **myVnet1 - partnerských vztahů** okno znovu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-159">After clicking **OK** in the previous step, the **Add peering** blade closes and you see the **myVnet1 - Peerings** blade again.</span></span> <span data-ttu-id="dbd75-160">Za několik sekund partnerského vztahu, kterou jste vytvořili se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="dbd75-160">After a few seconds, the peering you created appears in the blade.</span></span> <span data-ttu-id="dbd75-161">**Iniciované** , je uvedena ve **stav partnerského vztahu** sloupec pro **myVnet1ToMyVnet2** partnerského vztahu, můžete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="dbd75-161">**Initiated** is listed in the **PEERING STATUS** column for the **myVnet1ToMyVnet2** peering you created.</span></span> <span data-ttu-id="dbd75-162">Jste peered Vnet1 k Vnet2, ale teď musí peer myVnet2 k myVnet1.</span><span class="sxs-lookup"><span data-stu-id="dbd75-162">You've peered Vnet1 to Vnet2, but now you must peer myVnet2 to myVnet1.</span></span> <span data-ttu-id="dbd75-163">Partnerský vztah, musí být vytvořený v obou směrech povolit prostředky ve virtuálních sítích ke komunikaci mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="dbd75-163">The peering must be created in both directions to enable resources in the virtual networks to communicate with each other.</span></span>
11. <span data-ttu-id="dbd75-164">Proveďte kroky 5 až 10 znovu pro myVnet2.</span><span class="sxs-lookup"><span data-stu-id="dbd75-164">Complete steps 5-10 again for myVnet2.</span></span>  <span data-ttu-id="dbd75-165">Název partnerského vztahu *myVnet2ToMyVnet1*.</span><span class="sxs-lookup"><span data-stu-id="dbd75-165">Name the peering *myVnet2ToMyVnet1*.</span></span>
12. <span data-ttu-id="dbd75-166">Několik sekund po kliknutí na **OK** vytvoření partnerského vztahu pro MyVnet2, **myVnet2ToMyVnet1** partnerský vztah, kterou jste právě vytvořili je označené **připojeno** v  **Partnerský vztah stav** sloupce.</span><span class="sxs-lookup"><span data-stu-id="dbd75-166">A few seconds after clicking **OK** to create the peering for MyVnet2, the **myVnet2ToMyVnet1** peering you just created is listed with **Connected** in the **PEERING STATUS** column.</span></span>
13. <span data-ttu-id="dbd75-167">Znovu proveďte kroky 5 až 7 pro MyVnet1.</span><span class="sxs-lookup"><span data-stu-id="dbd75-167">Complete steps 5-7 again for MyVnet1.</span></span> <span data-ttu-id="dbd75-168">**Stav partnerského vztahu** pro **myVnet1ToVNet2** partnerského vztahu je nyní také **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-168">The **PEERING STATUS** for the **myVnet1ToVNet2** peering is now also **Connected**.</span></span> <span data-ttu-id="dbd75-169">Partnerského vztahu je úspěšně vytvořeno po uvidíte **připojeno** v **stav partnerského vztahu** sloupec pro obě virtuální sítě v partnerském vztahu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-169">The peering is successfully established after you see **Connected** in the **PEERING STATUS** column for both virtual networks in the peering.</span></span>
14. <span data-ttu-id="dbd75-170">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.</span><span class="sxs-lookup"><span data-stu-id="dbd75-170">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
15. <span data-ttu-id="dbd75-171">**Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="dbd75-171">**Optional**: To delete the resources that you create in this tutorial, complete the steps in the [Delete resources](#delete-portal) section of this article.</span></span>

<span data-ttu-id="dbd75-172">Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je nyní možné vzájemně komunikovat prostřednictvím jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="dbd75-172">Any Azure resources you create in either virtual network are now able to communicate with each other through their IP addresses.</span></span> <span data-ttu-id="dbd75-173">Pokud používáte překlad výchozí Azure pro virtuální sítě, nejsou prostředky ve virtuálních sítích překládat názvy virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="dbd75-173">If you're using default Azure name resolution for the virtual networks, the resources in the virtual networks are not able to resolve names across the virtual networks.</span></span> <span data-ttu-id="dbd75-174">Pokud chcete překládat názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS.</span><span class="sxs-lookup"><span data-stu-id="dbd75-174">If you want to resolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="dbd75-175">Zjistěte, jak nastavit [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="dbd75-175">Learn how to set up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="dbd75-176"><a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="dbd75-176"><a name="cli"></a>Create peering - Azure CLI</span></span>

<span data-ttu-id="dbd75-177">Následující skript:</span><span class="sxs-lookup"><span data-stu-id="dbd75-177">The following script:</span></span>

- <span data-ttu-id="dbd75-178">Vyžaduje Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dbd75-178">Requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="dbd75-179">Chcete-li najít verzi, spusťte `az --version` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dbd75-179">To find the version, run the `az --version` command.</span></span> <span data-ttu-id="dbd75-180">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dbd75-180">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
- <span data-ttu-id="dbd75-181">Funguje v prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="dbd75-181">Works in a Bash shell.</span></span> <span data-ttu-id="dbd75-182">Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dbd75-182">For options on running Azure CLI scripts on Windows client, see [Running the Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> 

<span data-ttu-id="dbd75-183">Místo instalace rozhraní příkazového řádku a jeho závislé součásti, můžete použít prostředí cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="dbd75-183">Instead of installing the CLI and its dependencies, you can use the Azure Cloud Shell.</span></span> <span data-ttu-id="dbd75-184">Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dbd75-184">The Azure Cloud Shell is a free Bash shell that you can run directly within the Azure portal.</span></span> <span data-ttu-id="dbd75-185">Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="dbd75-185">It has the Azure CLI preinstalled and configured to use with your account.</span></span> <span data-ttu-id="dbd75-186">Klikněte **vyzkoušet** tlačítko ve skriptu, který způsobem, které vyvolá prostředí cloudu, který vám umožní přihlásit se k účtu Azure s.</span><span class="sxs-lookup"><span data-stu-id="dbd75-186">Click the **Try it** button in the script that follows, which invokes a Cloud Shell that logs you can log in to your Azure account with.</span></span> <span data-ttu-id="dbd75-187">Chcete-li spustit skript, klikněte na tlačítko **kopie** tlačítko a umožňuje vložit obsah do své cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="dbd75-187">To execute the script, click the **Copy** button and paste the contents into your Cloud Shell.</span></span>

1. <span data-ttu-id="dbd75-188">Vytvořte skupinu prostředků a dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="dbd75-188">Create a resource group and two virtual networks.</span></span>

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. <span data-ttu-id="dbd75-189">Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="dbd75-189">Create a virtual network peering between the two virtual networks.</span></span>
 
    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get the id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 to VNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. <span data-ttu-id="dbd75-190">Po spuštění skriptu, zkontrolujte partnerských vztahů pro každou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="dbd75-190">After the script executes, review the peerings for each virtual network.</span></span> 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    <span data-ttu-id="dbd75-191">Předchozí příkaz spusťte znovu, nahraďte *myVnet1* s *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="dbd75-191">Run the previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="dbd75-192">Výstup z obou příkazů ukazuje **připojeno** v **PeeringState** sloupce.</span><span class="sxs-lookup"><span data-stu-id="dbd75-192">The output of both commands shows **Connected** in the **PeeringState** column.</span></span>

     <span data-ttu-id="dbd75-193">Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je nyní možné vzájemně komunikovat prostřednictvím jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="dbd75-193">Any Azure resources you create in either virtual network are now able to communicate with each other through their IP addresses.</span></span> <span data-ttu-id="dbd75-194">Pokud používáte překlad výchozí Azure pro virtuální sítě, nejsou prostředky ve virtuálních sítích překládat názvy virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="dbd75-194">If you're using default Azure name resolution for the virtual networks, the resources in the virtual networks are not able to resolve names across the virtual networks.</span></span> <span data-ttu-id="dbd75-195">Pokud chcete překládat názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS.</span><span class="sxs-lookup"><span data-stu-id="dbd75-195">If you want to resolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="dbd75-196">Zjistěte, jak nastavit [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="dbd75-196">Learn how to set up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

4. <span data-ttu-id="dbd75-197">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.</span><span class="sxs-lookup"><span data-stu-id="dbd75-197">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
5. <span data-ttu-id="dbd75-198">**Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete-cli) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="dbd75-198">**Optional**: To delete the resources that you create in this tutorial, complete the steps in [Delete resources](#delete-cli) in this article.</span></span>


## <span data-ttu-id="dbd75-199"><a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="dbd75-199"><a name="powershell"></a>Create peering - PowerShell</span></span>

1. <span data-ttu-id="dbd75-200">Nainstalujte nejnovější verzi modulu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) pro PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dbd75-200">Install the latest version of the PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="dbd75-201">Pokud s Azure PowerShellem začínáte, podívejte se na [Přehled Azure PowerShellu](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dbd75-201">If you're new to Azure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="dbd75-202">Pokud chcete spustit relaci PowerShellu, přejděte do nabídky **Start**, zadejte **powershell** a potom klikněte na **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-202">To start a PowerShell session, go to **Start**, enter **powershell**, and then click **PowerShell**.</span></span>
3. <span data-ttu-id="dbd75-203">V PowerShellu se přihlaste k Azure zadáním příkazu `login-azurermaccount`.</span><span class="sxs-lookup"><span data-stu-id="dbd75-203">In PowerShell, log in to Azure by entering the `login-azurermaccount` command.</span></span> <span data-ttu-id="dbd75-204">Účet, ke kterému se přihlásíte, musí mít potřebná oprávnění k vytvoření virtuální sítě partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-204">The account you log in with must have the necessary permissions to create a virtual network peering.</span></span> <span data-ttu-id="dbd75-205">Najdete v článku [oprávnění](#permissions) tohoto článku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="dbd75-205">See the [Permissions](#permissions) section of this article for details.</span></span>
4. <span data-ttu-id="dbd75-206">Vytvořte skupinu prostředků a dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="dbd75-206">Create a resource group and two virtual networks.</span></span> <span data-ttu-id="dbd75-207">Spustit skript, zkopírujte následující skript, vložte jej do prostředí PowerShell a potom stiskněte klávesu `Enter` po poslední řádek se zobrazí na obrazovce:</span><span class="sxs-lookup"><span data-stu-id="dbd75-207">To execute the script, copy the following script, paste it into PowerShell, and then press `Enter` after the last line appears on the screen:</span></span>

    ```powershell
    # Variables for common values used throughout the script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. <span data-ttu-id="dbd75-208">Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="dbd75-208">Create a virtual network peering between the two virtual networks.</span></span> <span data-ttu-id="dbd75-209">Zkopírujte následující skript, vložte v prostředí PowerShell a potom stiskněte klávesu `Enter` po poslední řádek se zobrazí na obrazovce:</span><span class="sxs-lookup"><span data-stu-id="dbd75-209">Copy the following script, paste in to PowerShell, and then press `Enter` after the last line appears on the screen:</span></span>
    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 to VNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. <span data-ttu-id="dbd75-210">Ke kontrole podsítě virtuální sítě, zkopírujte následující příkaz, vložte v prostředí PowerShell a stiskněte klávesu `Enter`:</span><span class="sxs-lookup"><span data-stu-id="dbd75-210">To review the subnets for the virtual network, copy the following command, paste in to PowerShell, and then press `Enter`:</span></span>

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    <span data-ttu-id="dbd75-211">Předchozí příkaz spusťte znovu, nahraďte *myVnet1* s *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="dbd75-211">Run the previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="dbd75-212">Výstup z obou příkazů ukazuje **připojeno** v **PeeringState** sloupce.</span><span class="sxs-lookup"><span data-stu-id="dbd75-212">The output of both commands shows **Connected** in the **PeeringState** column.</span></span>
7. <span data-ttu-id="dbd75-213">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.</span><span class="sxs-lookup"><span data-stu-id="dbd75-213">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
8. <span data-ttu-id="dbd75-214">**Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete-powershell) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="dbd75-214">**Optional**: To delete the resources that you create in this tutorial, complete the steps in [Delete resources](#delete-powershell) in this article.</span></span>

<span data-ttu-id="dbd75-215">Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je nyní možné vzájemně komunikovat prostřednictvím jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="dbd75-215">Any Azure resources you create in either virtual network are now able to communicate with each other through their IP addresses.</span></span> <span data-ttu-id="dbd75-216">Pokud používáte překlad výchozí Azure pro virtuální sítě, nejsou prostředky ve virtuálních sítích překládat názvy virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="dbd75-216">If you're using default Azure name resolution for the virtual networks, the resources in the virtual networks are not able to resolve names across the virtual networks.</span></span> <span data-ttu-id="dbd75-217">Pokud chcete překládat názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS.</span><span class="sxs-lookup"><span data-stu-id="dbd75-217">If you want to resolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="dbd75-218">Zjistěte, jak nastavit [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="dbd75-218">Learn how to set up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="dbd75-219"><a name="template"></a>Vytvoření partnerského vztahu - šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="dbd75-219"><a name="template"></a>Create peering - Resource Manager template</span></span>

1. <span data-ttu-id="dbd75-220">Referenční dokumentace [vytvoření virtuální sítě partnerského vztahu](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="dbd75-220">Reference [Create a virtual network peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager template.</span></span> <span data-ttu-id="dbd75-221">Součástí šablony jsou pokyny pro její nasazení pomocí webu Azure Portal, PowerShellu nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="dbd75-221">Instructions are provided with the template for deploying the template using the Azure portal, PowerShell, or the Azure CLI.</span></span> <span data-ttu-id="dbd75-222">Přihlaste se ke podle toho, co nástroj, můžete zvolit pro nasazení šablony s použitím účtu, který má nezbytná oprávnění k vytvoření virtuální sítě partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-222">Log in to whichever tool you choose to deploy the template with using an account that has the necessary permissions to create a virtual network peering.</span></span> <span data-ttu-id="dbd75-223">Najdete v článku [oprávnění](#permissions) tohoto článku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="dbd75-223">See the [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="dbd75-224">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.</span><span class="sxs-lookup"><span data-stu-id="dbd75-224">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
3. <span data-ttu-id="dbd75-225">**Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete) tohoto článku, pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dbd75-225">**Optional**: To delete the resources that you create in this tutorial, complete the steps in the [Delete resources](#delete) section of this article, using either the Azure portal, PowerShell, or the Azure CLI.</span></span>

## <span data-ttu-id="dbd75-226"><a name="permissions"></a>Oprávnění</span><span class="sxs-lookup"><span data-stu-id="dbd75-226"><a name="permissions"></a>Permissions</span></span>

<span data-ttu-id="dbd75-227">Účty, které použijete k vytvoření virtuální sítě partnerského vztahu musí mít potřebné role nebo oprávnění.</span><span class="sxs-lookup"><span data-stu-id="dbd75-227">The accounts you use to create a virtual network peering must have the necessary role or permissions.</span></span> <span data-ttu-id="dbd75-228">Například pokud dvě virtuální sítě s názvem VNet1 a VNet2 byly partnerský vztah, musí váš účet se přiřazenou následující minimální role nebo oprávnění pro každou virtuální síť:</span><span class="sxs-lookup"><span data-stu-id="dbd75-228">For example, if you were peering two virtual networks named VNet1 and VNet2, your account must be assigned the following minimum role or permissions for each virtual network:</span></span>
    
|<span data-ttu-id="dbd75-229">Virtuální síť</span><span class="sxs-lookup"><span data-stu-id="dbd75-229">Virtual network</span></span>|<span data-ttu-id="dbd75-230">Role</span><span class="sxs-lookup"><span data-stu-id="dbd75-230">Role</span></span>|<span data-ttu-id="dbd75-231">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="dbd75-231">Permissions</span></span>|
|---|---|---|
|<span data-ttu-id="dbd75-232">VNet1</span><span class="sxs-lookup"><span data-stu-id="dbd75-232">VNet1</span></span>|[<span data-ttu-id="dbd75-233">Přispěvatel sítě</span><span class="sxs-lookup"><span data-stu-id="dbd75-233">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="dbd75-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span><span class="sxs-lookup"><span data-stu-id="dbd75-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span></span>|
|<span data-ttu-id="dbd75-235">VNet2</span><span class="sxs-lookup"><span data-stu-id="dbd75-235">VNet2</span></span>|[<span data-ttu-id="dbd75-236">Přispěvatel sítě</span><span class="sxs-lookup"><span data-stu-id="dbd75-236">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="dbd75-237">Microsoft.Network/virtualNetworks/peer</span><span class="sxs-lookup"><span data-stu-id="dbd75-237">Microsoft.Network/virtualNetworks/peer</span></span>|

<span data-ttu-id="dbd75-238">Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení specifické oprávnění pro [vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="dbd75-238">Learn more about [built-in roles](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) and assigning specific permissions to [custom roles](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Resource Manager only).</span></span>

## <span data-ttu-id="dbd75-239"><a name="delete"></a>Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="dbd75-239"><a name="delete"></a>Delete resources</span></span>
<span data-ttu-id="dbd75-240">Po dokončení tohoto kurzu, můžete chtít odstranit z prostředků, které jste vytvořili v tomto kurzu, aby vám zbytečně nenabíhaly poplatky za používání.</span><span class="sxs-lookup"><span data-stu-id="dbd75-240">When you've finished this tutorial, you might want to delete the resources you created in the tutorial, so you don't incur usage charges.</span></span> <span data-ttu-id="dbd75-241">Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="dbd75-241">Deleting a resource group also deletes all resources that are in the resource group.</span></span>

### <span data-ttu-id="dbd75-242"><a name="delete-portal"></a>Portál Azure</span><span class="sxs-lookup"><span data-stu-id="dbd75-242"><a name="delete-portal"></a>Azure portal</span></span>

1. <span data-ttu-id="dbd75-243">V dialogovém okně hledání portálu zadejte **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-243">In the portal search box, enter **myResourceGroup**.</span></span> <span data-ttu-id="dbd75-244">Ve výsledcích hledání klikněte na tlačítko **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-244">In the search results, click **myResourceGroup**.</span></span>
2. <span data-ttu-id="dbd75-245">Na **myResourceGroup** okně klikněte **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dbd75-245">On the **myResourceGroup** blade, click the **Delete** icon.</span></span>
3. <span data-ttu-id="dbd75-246">Potvrďte odstranění, v **název skupiny prostředků typu** zadejte **myResourceGroup**a potom klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="dbd75-246">To confirm the deletion, in the **TYPE THE RESOURCE GROUP NAME** box, enter **myResourceGroup**, and then click **Delete**.</span></span>

### <span data-ttu-id="dbd75-247"><a name="delete-cli"></a>Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="dbd75-247"><a name="delete-cli"></a>Azure CLI</span></span>

<span data-ttu-id="dbd75-248">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dbd75-248">Enter the following command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <span data-ttu-id="dbd75-249"><a name="delete-powershell"></a>Prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="dbd75-249"><a name="delete-powershell"></a>PowerShell</span></span>

<span data-ttu-id="dbd75-250">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dbd75-250">Enter the following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a><span data-ttu-id="dbd75-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dbd75-251">Next steps</span></span>

- <span data-ttu-id="dbd75-252">Důkladně Seznamte se s důležité [partnerského vztahu omezení virtuální sítě a chování](virtual-network-manage-peering.md#requirements-and-constraints) před vytvořením virtuální sítě partnerský vztah pro produkční použití.</span><span class="sxs-lookup"><span data-stu-id="dbd75-252">Thoroughly familiarize yourself with important [virtual network peering constraints and behaviors](virtual-network-manage-peering.md#requirements-and-constraints) before creating a virtual network peering for production use.</span></span>
- <span data-ttu-id="dbd75-253">Další informace o všech [partnerského vztahu nastavení virtuální sítě](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="dbd75-253">Learn about all [virtual network peering settings](virtual-network-manage-peering.md#create-a-peering).</span></span>
- <span data-ttu-id="dbd75-254">Zjistěte, jak [vytvořit rozbočovače a příčkou topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) s partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="dbd75-254">Learn how to [create a hub and spoke network topology](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) with virtual network peering.</span></span>
