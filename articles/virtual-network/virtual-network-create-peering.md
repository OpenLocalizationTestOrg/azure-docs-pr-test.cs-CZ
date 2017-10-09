---
title: "aaaCreate virtuální síti Azure partnerský vztah – Resource Manager - stejného předplatného. | Microsoft Docs"
description: "Zjistěte, jak virtuální sítě vztahy mezi virtuálními sítěmi toocreate vytváří pomocí Správce prostředků, který neexistuje v hello stejné předplatné Azure."
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
ms.openlocfilehash: c2d24fdc8103c09c3bfb8e59be12e301d9e9a55a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a><span data-ttu-id="62594-103">Vytvoření virtuální sítě partnerský vztah – Resource Manager, stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="62594-103">Create a virtual network peering - Resource Manager, same subscription</span></span>

<span data-ttu-id="62594-104">V tomto kurzu zjistíte toocreate virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="62594-104">In this tutorial, you learn toocreate a virtual network peering between virtual networks created through Resource Manager.</span></span> <span data-ttu-id="62594-105">Obě virtuální sítě existuje v hello stejné předplatné.</span><span class="sxs-lookup"><span data-stu-id="62594-105">Both virtual networks exist in hello same subscription.</span></span> <span data-ttu-id="62594-106">Partnerského vztahu dva virtuální sítě umožňuje prostředky v různých virtuálních sítích toocommunicate s jinými s hello stejné šířky pásma a latence, jako by byl hello prostředky v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="62594-106">Peering two virtual networks enables resources in different virtual networks toocommunicate with each other with hello same bandwidth and latency as though hello resources were in hello same virtual network.</span></span> <span data-ttu-id="62594-107">Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="62594-107">Learn more about [Virtual network peering](virtual-network-peering-overview.md).</span></span> 

<span data-ttu-id="62594-108">Hello kroky toocreate virtuální sítě partnerský vztah se liší, v závislosti na tom, zda text hello virtuální sítě jsou v hello stejný nebo jiný, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuální sítě se vytvářejí prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="62594-108">hello steps toocreate a virtual network peering are different, depending on whether hello virtual networks are in hello same, or different, subscriptions, and which [Azure deployment model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtual networks are created through.</span></span> <span data-ttu-id="62594-109">Zjistěte, jak toocreate a virtuální sítě, partnerský vztah v jiných scénářích kliknutím hello scénář z hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="62594-109">Learn how toocreate a virtual network peering in other scenarios by clicking hello scenario from hello following table:</span></span>

|<span data-ttu-id="62594-110">Model nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="62594-110">Azure deployment model</span></span>  | <span data-ttu-id="62594-111">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="62594-111">Azure subscription</span></span>  |
|--------- |---------|
|[<span data-ttu-id="62594-112">I Resource Manager</span><span class="sxs-lookup"><span data-stu-id="62594-112">Both Resource Manager</span></span>](create-peering-different-subscriptions.md) |<span data-ttu-id="62594-113">Různé</span><span class="sxs-lookup"><span data-stu-id="62594-113">Different</span></span>|
|[<span data-ttu-id="62594-114">Jeden Resource Manager, jeden classic</span><span class="sxs-lookup"><span data-stu-id="62594-114">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models.md) |<span data-ttu-id="62594-115">stejné</span><span class="sxs-lookup"><span data-stu-id="62594-115">Same</span></span>|
|[<span data-ttu-id="62594-116">Jeden Resource Manager, jeden classic</span><span class="sxs-lookup"><span data-stu-id="62594-116">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models-subscriptions.md) |<span data-ttu-id="62594-117">Různé</span><span class="sxs-lookup"><span data-stu-id="62594-117">Different</span></span>|

<span data-ttu-id="62594-118">Mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic hello nelze vytvořit virtuální síť partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="62594-118">A virtual network peering cannot be created between two virtual networks deployed through hello classic deployment model.</span></span> <span data-ttu-id="62594-119">Partnerský vztah virtuální sítě mohou být vytvořeny pouze mezi dvěma virtuálními sítěmi, které existují v hello stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="62594-119">A virtual network peering can only be created between two virtual networks that exist in hello same Azure region.</span></span> <span data-ttu-id="62594-120">Pokud potřebujete tooconnect virtuální sítě, které byly obě vytvořené pomocí modelu nasazení classic hello nebo které existovat v různých oblastech Azure, můžete použít Azure [brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-120">If you need tooconnect virtual networks that were both created through hello classic deployment model, or that exist in different Azure regions, you can use an Azure [VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtual networks.</span></span> 

<span data-ttu-id="62594-121">Můžete použít hello [portál Azure](#portal), hello Azure [rozhraní příkazového řádku](#cli) (CLI) Azure [prostředí PowerShell](#powershell), nebo [šablony Azure Resource Manageru](#template) toocreate partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-121">You can use hello [Azure portal](#portal), hello Azure [command-line interface](#cli) (CLI), Azure [PowerShell](#powershell), or an [Azure Resource Manager template](#template) toocreate a virtual network peering.</span></span> <span data-ttu-id="62594-122">Přímo toohello kroky pro vytvoření virtuální sítě partnerský vztah pomocí vaší nástroje, klepněte na libovolný hello předchozí nástroj odkazy toogo.</span><span class="sxs-lookup"><span data-stu-id="62594-122">Click any of hello previous tool links toogo directly toohello steps for creating a virtual network peering using your tool of choice.</span></span>

## <span data-ttu-id="62594-123"><a name="portal"></a>Vytvoření partnerského vztahu – portál Azure</span><span class="sxs-lookup"><span data-stu-id="62594-123"><a name="portal"></a>Create peering - Azure portal</span></span>

1. <span data-ttu-id="62594-124">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62594-124">Log in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="62594-125">Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-125">hello account you log in with must have hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="62594-126">V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62594-126">See hello [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="62594-127">Klikněte na tlačítko **+ nový**, klikněte na tlačítko **sítě**, pak klikněte na tlačítko **virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="62594-127">Click **+ New**, click **Networking**, then click **Virtual network**.</span></span>
3. <span data-ttu-id="62594-128">V hello **vytvořit virtuální síť** okno, zadejte, nebo vyberte hodnoty pro hello následující nastavení a potom klikněte na tlačítko **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="62594-128">In hello **Create virtual network** blade, enter, or select values for hello following settings, then click **Create**:</span></span>
    - <span data-ttu-id="62594-129">**Název**: *myVnet1*</span><span class="sxs-lookup"><span data-stu-id="62594-129">**Name**: *myVnet1*</span></span>
    - <span data-ttu-id="62594-130">**Adresní prostor**: *10.0.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="62594-130">**Address space**: *10.0.0.0/16*</span></span>
    - <span data-ttu-id="62594-131">**Název podsítě**: *výchozí*</span><span class="sxs-lookup"><span data-stu-id="62594-131">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="62594-132">**Rozsah adres podsítě**: *10.0.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="62594-132">**Subnet address range**: *10.0.0.0/24*</span></span>
    - <span data-ttu-id="62594-133">**Předplatné**: Vyberte předplatné</span><span class="sxs-lookup"><span data-stu-id="62594-133">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="62594-134">**Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="62594-134">**Resource group**: Select **Create new** and enter *myResourceGroup*</span></span>
    - <span data-ttu-id="62594-135">**Umístění**: *východní USA*</span><span class="sxs-lookup"><span data-stu-id="62594-135">**Location**: *East US*</span></span>
4. <span data-ttu-id="62594-136">Proveďte kroky 2 až 3 znovu zadání hello následující hodnoty v kroku 3:</span><span class="sxs-lookup"><span data-stu-id="62594-136">Complete steps 2-3 again specifying hello following values in step 3:</span></span>
    - <span data-ttu-id="62594-137">**Název**: *myVnet2*</span><span class="sxs-lookup"><span data-stu-id="62594-137">**Name**: *myVnet2*</span></span>
    - <span data-ttu-id="62594-138">**Adresní prostor**: *10.1.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="62594-138">**Address space**: *10.1.0.0/16*</span></span>
    - <span data-ttu-id="62594-139">**Název podsítě**: *výchozí*</span><span class="sxs-lookup"><span data-stu-id="62594-139">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="62594-140">**Rozsah adres podsítě**: *10.1.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="62594-140">**Subnet address range**: *10.1.0.0/24*</span></span>
    - <span data-ttu-id="62594-141">**Předplatné**: Vyberte předplatné</span><span class="sxs-lookup"><span data-stu-id="62594-141">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="62594-142">**Skupina prostředků**: vyberte **použít existující** a vyberte *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="62594-142">**Resource group**: Select **Use existing** and select *myResourceGroup*</span></span>
    - <span data-ttu-id="62594-143">**Umístění**: *východní USA*</span><span class="sxs-lookup"><span data-stu-id="62594-143">**Location**: *East US*</span></span>
5. <span data-ttu-id="62594-144">V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="62594-144">In hello **Search resources** box at hello top of hello portal, type *myResourceGroup*.</span></span> <span data-ttu-id="62594-145">Klikněte na tlačítko **myResourceGroup** při zobrazí ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="62594-145">Click **myResourceGroup** when it appears in hello search results.</span></span> <span data-ttu-id="62594-146">Zobrazí se okno pro hello **myresourcegroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="62594-146">A blade appears for hello **myresourcegroup** resource group.</span></span> <span data-ttu-id="62594-147">Skupina prostředků Hello obsahuje hello dvě virtuální sítě vytvořené v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="62594-147">hello resource group contains hello two virtual networks created in previous steps.</span></span>
6. <span data-ttu-id="62594-148">Klikněte na tlačítko **myVNet1**.</span><span class="sxs-lookup"><span data-stu-id="62594-148">Click **myVNet1**.</span></span>
7. <span data-ttu-id="62594-149">V hello **myVnet1** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** z hello svislé seznam možností na levé straně okna hello hello.</span><span class="sxs-lookup"><span data-stu-id="62594-149">In hello **myVnet1** blade that appears, click **Peerings** from hello vertical list of options on hello left side of hello blade.</span></span>
8. <span data-ttu-id="62594-150">V hello **myVnet1 - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**</span><span class="sxs-lookup"><span data-stu-id="62594-150">In hello **myVnet1 - Peerings** blade that appeared, click **+ Add**</span></span>
9. <span data-ttu-id="62594-151">V hello **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte hello následující možnosti a potom klikněte na tlačítko **OK**:</span><span class="sxs-lookup"><span data-stu-id="62594-151">In hello **Add peering** blade that appears, enter, or select hello following options, then click **OK**:</span></span>
     - <span data-ttu-id="62594-152">**Název**: *myVnet1ToMyVnet2*</span><span class="sxs-lookup"><span data-stu-id="62594-152">**Name**: *myVnet1ToMyVnet2*</span></span>
     - <span data-ttu-id="62594-153">**Virtuální síť modelu nasazení**: vyberte **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="62594-153">**Virtual network deployment model**:  Select **Resource Manager**.</span></span> 
     - <span data-ttu-id="62594-154">**Předplatné**: Vyberte předplatné</span><span class="sxs-lookup"><span data-stu-id="62594-154">**Subscription**: Select your subscription</span></span>
     - <span data-ttu-id="62594-155">**Virtuální síť**: klikněte na tlačítko **vyberte virtuální síť**, pak klikněte na tlačítko **myVnet2**.</span><span class="sxs-lookup"><span data-stu-id="62594-155">**Virtual network**:  Click **Choose a virtual network**, then click **myVnet2**.</span></span>
     - <span data-ttu-id="62594-156">**Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="62594-156">**Allow virtual network access:** Ensure that **Enabled** is selected.</span></span>
    <span data-ttu-id="62594-157">Žádné další nastavení použitá v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="62594-157">No other settings are used in this tutorial.</span></span> <span data-ttu-id="62594-158">Přečtěte si toolearn o všech nastaveních partnerského vztahu, [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="62594-158">toolearn about all peering settings, read [Manage virtual network peerings](virtual-network-manage-peering.md#create-a-peering).</span></span>
10. <span data-ttu-id="62594-159">Po kliknutí na **OK** v předchozím kroku hello hello **partnerský vztah přidat** okno se zavře a zobrazí hello **myVnet1 - partnerských vztahů** okno znovu.</span><span class="sxs-lookup"><span data-stu-id="62594-159">After clicking **OK** in hello previous step, hello **Add peering** blade closes and you see hello **myVnet1 - Peerings** blade again.</span></span> <span data-ttu-id="62594-160">Za několik sekund zobrazí se hello partnerský vztah, kterou jste vytvořili v okně hello.</span><span class="sxs-lookup"><span data-stu-id="62594-160">After a few seconds, hello peering you created appears in hello blade.</span></span> <span data-ttu-id="62594-161">**Iniciované** je uvedena v hello **stav partnerského vztahu** sloupec pro hello **myVnet1ToMyVnet2** partnerského vztahu, můžete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="62594-161">**Initiated** is listed in hello **PEERING STATUS** column for hello **myVnet1ToMyVnet2** peering you created.</span></span> <span data-ttu-id="62594-162">Jste peered Vnet1 tooVnet2, ale teď musí peer myVnet2 toomyVnet1.</span><span class="sxs-lookup"><span data-stu-id="62594-162">You've peered Vnet1 tooVnet2, but now you must peer myVnet2 toomyVnet1.</span></span> <span data-ttu-id="62594-163">Hello partnerského vztahu musí být vytvořený v obou směrech tooenable prostředky v toocommunicate hello virtuálních sítí mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="62594-163">hello peering must be created in both directions tooenable resources in hello virtual networks toocommunicate with each other.</span></span>
11. <span data-ttu-id="62594-164">Proveďte kroky 5 až 10 znovu pro myVnet2.</span><span class="sxs-lookup"><span data-stu-id="62594-164">Complete steps 5-10 again for myVnet2.</span></span>  <span data-ttu-id="62594-165">Partnerský vztah hello název *myVnet2ToMyVnet1*.</span><span class="sxs-lookup"><span data-stu-id="62594-165">Name hello peering *myVnet2ToMyVnet1*.</span></span>
12. <span data-ttu-id="62594-166">Několik sekund po kliknutí na **OK** toocreate hello partnerský vztah pro MyVnet2, hello **myVnet2ToMyVnet1** partnerský vztah, kterou jste právě vytvořili je označené **připojeno** v hello  **Partnerský vztah stav** sloupce.</span><span class="sxs-lookup"><span data-stu-id="62594-166">A few seconds after clicking **OK** toocreate hello peering for MyVnet2, hello **myVnet2ToMyVnet1** peering you just created is listed with **Connected** in hello **PEERING STATUS** column.</span></span>
13. <span data-ttu-id="62594-167">Znovu proveďte kroky 5 až 7 pro MyVnet1.</span><span class="sxs-lookup"><span data-stu-id="62594-167">Complete steps 5-7 again for MyVnet1.</span></span> <span data-ttu-id="62594-168">Hello **stav partnerského vztahu** pro hello **myVnet1ToVNet2** partnerského vztahu je nyní také **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="62594-168">hello **PEERING STATUS** for hello **myVnet1ToVNet2** peering is now also **Connected**.</span></span> <span data-ttu-id="62594-169">partnerský vztah Hello je úspěšně vytvořeno po uvidíte **připojeno** v hello **stav partnerského vztahu** sloupec pro obě virtuální sítě v partnerském vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="62594-169">hello peering is successfully established after you see **Connected** in hello **PEERING STATUS** column for both virtual networks in hello peering.</span></span>
14. <span data-ttu-id="62594-170">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.</span><span class="sxs-lookup"><span data-stu-id="62594-170">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
15. <span data-ttu-id="62594-171">**Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="62594-171">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in hello [Delete resources](#delete-portal) section of this article.</span></span>

<span data-ttu-id="62594-172">Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="62594-172">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="62594-173">Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello.</span><span class="sxs-lookup"><span data-stu-id="62594-173">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="62594-174">Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS.</span><span class="sxs-lookup"><span data-stu-id="62594-174">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="62594-175">Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="62594-175">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="62594-176"><a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="62594-176"><a name="cli"></a>Create peering - Azure CLI</span></span>

<span data-ttu-id="62594-177">Hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="62594-177">hello following script:</span></span>

- <span data-ttu-id="62594-178">Vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="62594-178">Requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="62594-179">verze hello toofind, spusťte hello `az --version` příkaz.</span><span class="sxs-lookup"><span data-stu-id="62594-179">toofind hello version, run hello `az --version` command.</span></span> <span data-ttu-id="62594-180">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62594-180">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
- <span data-ttu-id="62594-181">Funguje v prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="62594-181">Works in a Bash shell.</span></span> <span data-ttu-id="62594-182">Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows hello rozhraní příkazového řádku Azure](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62594-182">For options on running Azure CLI scripts on Windows client, see [Running hello Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> 

<span data-ttu-id="62594-183">Místo instalace hello rozhraní příkazového řádku a jeho závislé součásti, můžete použít hello prostředí cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="62594-183">Instead of installing hello CLI and its dependencies, you can use hello Azure Cloud Shell.</span></span> <span data-ttu-id="62594-184">Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="62594-184">hello Azure Cloud Shell is a free Bash shell that you can run directly within hello Azure portal.</span></span> <span data-ttu-id="62594-185">Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="62594-185">It has hello Azure CLI preinstalled and configured toouse with your account.</span></span> <span data-ttu-id="62594-186">Klikněte na tlačítko hello **vyzkoušet** tlačítka na hello skript, který způsobem, které vyvolá prostředí cloudu, který vám umožní přihlásit se tooyour účet Azure s.</span><span class="sxs-lookup"><span data-stu-id="62594-186">Click hello **Try it** button in hello script that follows, which invokes a Cloud Shell that logs you can log in tooyour Azure account with.</span></span> <span data-ttu-id="62594-187">tooexecute hello skriptu, klikněte na tlačítko hello **kopie** tlačítko a vložte obsah hello do své cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="62594-187">tooexecute hello script, click hello **Copy** button and paste hello contents into your Cloud Shell.</span></span>

1. <span data-ttu-id="62594-188">Vytvořte skupinu prostředků a dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-188">Create a resource group and two virtual networks.</span></span>

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
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

2. <span data-ttu-id="62594-189">Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="62594-189">Create a virtual network peering between hello two virtual networks.</span></span>
 
    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get hello id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 tooVNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. <span data-ttu-id="62594-190">Po provedení hello skriptu, zkontrolujte hello partnerských vztahů pro každou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="62594-190">After hello script executes, review hello peerings for each virtual network.</span></span> 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    <span data-ttu-id="62594-191">Opakujte předchozí příkaz hello, nahraďte *myVnet1* s *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="62594-191">Run hello previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="62594-192">výstup Hello obou příkazů ukazuje **připojeno** v hello **PeeringState** sloupce.</span><span class="sxs-lookup"><span data-stu-id="62594-192">hello output of both commands shows **Connected** in hello **PeeringState** column.</span></span>

     <span data-ttu-id="62594-193">Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="62594-193">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="62594-194">Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello.</span><span class="sxs-lookup"><span data-stu-id="62594-194">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="62594-195">Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS.</span><span class="sxs-lookup"><span data-stu-id="62594-195">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="62594-196">Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="62594-196">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

4. <span data-ttu-id="62594-197">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.</span><span class="sxs-lookup"><span data-stu-id="62594-197">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
5. <span data-ttu-id="62594-198">**Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-cli) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="62594-198">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in [Delete resources](#delete-cli) in this article.</span></span>


## <span data-ttu-id="62594-199"><a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="62594-199"><a name="powershell"></a>Create peering - PowerShell</span></span>

1. <span data-ttu-id="62594-200">Nainstalujte nejnovější verzi hello prostředí PowerShell hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu.</span><span class="sxs-lookup"><span data-stu-id="62594-200">Install hello latest version of hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="62594-201">Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62594-201">If you're new tooAzure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="62594-202">toostart relaci prostředí PowerShell, přejděte příliš**spustit**, zadejte **prostředí powershell**a potom klikněte na **prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="62594-202">toostart a PowerShell session, go too**Start**, enter **powershell**, and then click **PowerShell**.</span></span>
3. <span data-ttu-id="62594-203">V prostředí PowerShell přihlásit tooAzure zadáním hello `login-azurermaccount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="62594-203">In PowerShell, log in tooAzure by entering hello `login-azurermaccount` command.</span></span> <span data-ttu-id="62594-204">Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-204">hello account you log in with must have hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="62594-205">V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62594-205">See hello [Permissions](#permissions) section of this article for details.</span></span>
4. <span data-ttu-id="62594-206">Vytvořte skupinu prostředků a dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-206">Create a resource group and two virtual networks.</span></span> <span data-ttu-id="62594-207">tooexecute hello skriptu kopie hello následující skript, vložte jej do prostředí PowerShell a stiskněte klávesu `Enter` po poslední řádek hello se zobrazí na úvodní obrazovka:</span><span class="sxs-lookup"><span data-stu-id="62594-207">tooexecute hello script, copy hello following script, paste it into PowerShell, and then press `Enter` after hello last line appears on hello screen:</span></span>

    ```powershell
    # Variables for common values used throughout hello script.
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

5. <span data-ttu-id="62594-208">Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="62594-208">Create a virtual network peering between hello two virtual networks.</span></span> <span data-ttu-id="62594-209">Kopírování hello následující skript, vložte tooPowerShell a stiskněte klávesu `Enter` po poslední řádek hello se zobrazí na úvodní obrazovka:</span><span class="sxs-lookup"><span data-stu-id="62594-209">Copy hello following script, paste in tooPowerShell, and then press `Enter` after hello last line appears on hello screen:</span></span>
    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 tooVNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. <span data-ttu-id="62594-210">tooreview hello podsítě pro virtuální síť hello, kopie hello následující příkaz, vložte tooPowerShell a potom stiskněte klávesu `Enter`:</span><span class="sxs-lookup"><span data-stu-id="62594-210">tooreview hello subnets for hello virtual network, copy hello following command, paste in tooPowerShell, and then press `Enter`:</span></span>

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    <span data-ttu-id="62594-211">Opakujte předchozí příkaz hello, nahraďte *myVnet1* s *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="62594-211">Run hello previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="62594-212">výstup Hello obou příkazů ukazuje **připojeno** v hello **PeeringState** sloupce.</span><span class="sxs-lookup"><span data-stu-id="62594-212">hello output of both commands shows **Connected** in hello **PeeringState** column.</span></span>
7. <span data-ttu-id="62594-213">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.</span><span class="sxs-lookup"><span data-stu-id="62594-213">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
8. <span data-ttu-id="62594-214">**Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-powershell) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="62594-214">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in [Delete resources](#delete-powershell) in this article.</span></span>

<span data-ttu-id="62594-215">Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="62594-215">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="62594-216">Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello.</span><span class="sxs-lookup"><span data-stu-id="62594-216">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="62594-217">Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS.</span><span class="sxs-lookup"><span data-stu-id="62594-217">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="62594-218">Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="62594-218">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="62594-219"><a name="template"></a>Vytvoření partnerského vztahu - šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="62594-219"><a name="template"></a>Create peering - Resource Manager template</span></span>

1. <span data-ttu-id="62594-220">Referenční dokumentace [vytvoření virtuální sítě partnerského vztahu](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="62594-220">Reference [Create a virtual network peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager template.</span></span> <span data-ttu-id="62594-221">Pokyny jsou k dispozici hello šablonou pro nasazení šablony hello pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="62594-221">Instructions are provided with hello template for deploying hello template using hello Azure portal, PowerShell, or hello Azure CLI.</span></span> <span data-ttu-id="62594-222">Přihlášení toowhichever nástroj, který zvolíte toodeploy hello šablony s použitím účtu, který má hello toocreate potřebná oprávnění, virtuální sítě partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="62594-222">Log in toowhichever tool you choose toodeploy hello template with using an account that has hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="62594-223">V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62594-223">See hello [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="62594-224">**Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.</span><span class="sxs-lookup"><span data-stu-id="62594-224">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
3. <span data-ttu-id="62594-225">**Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete) hello tohoto článku, buď pomocí portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="62594-225">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in hello [Delete resources](#delete) section of this article, using either hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

## <span data-ttu-id="62594-226"><a name="permissions"></a>Oprávnění</span><span class="sxs-lookup"><span data-stu-id="62594-226"><a name="permissions"></a>Permissions</span></span>

<span data-ttu-id="62594-227">Hello účty, že používáte toocreate partnerský vztah virtuální síť musí mít hello potřebné role nebo oprávnění.</span><span class="sxs-lookup"><span data-stu-id="62594-227">hello accounts you use toocreate a virtual network peering must have hello necessary role or permissions.</span></span> <span data-ttu-id="62594-228">Například pokud dvě virtuální sítě s názvem VNet1 a VNet2 byly partnerský vztah, musí váš účet se přiřazenou hello následující minimální role nebo oprávnění pro každou virtuální síť:</span><span class="sxs-lookup"><span data-stu-id="62594-228">For example, if you were peering two virtual networks named VNet1 and VNet2, your account must be assigned hello following minimum role or permissions for each virtual network:</span></span>
    
|<span data-ttu-id="62594-229">Virtuální síť</span><span class="sxs-lookup"><span data-stu-id="62594-229">Virtual network</span></span>|<span data-ttu-id="62594-230">Role</span><span class="sxs-lookup"><span data-stu-id="62594-230">Role</span></span>|<span data-ttu-id="62594-231">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="62594-231">Permissions</span></span>|
|---|---|---|
|<span data-ttu-id="62594-232">VNet1</span><span class="sxs-lookup"><span data-stu-id="62594-232">VNet1</span></span>|[<span data-ttu-id="62594-233">Přispěvatel sítě</span><span class="sxs-lookup"><span data-stu-id="62594-233">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="62594-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span><span class="sxs-lookup"><span data-stu-id="62594-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span></span>|
|<span data-ttu-id="62594-235">VNet2</span><span class="sxs-lookup"><span data-stu-id="62594-235">VNet2</span></span>|[<span data-ttu-id="62594-236">Přispěvatel sítě</span><span class="sxs-lookup"><span data-stu-id="62594-236">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="62594-237">Microsoft.Network/virtualNetworks/peer</span><span class="sxs-lookup"><span data-stu-id="62594-237">Microsoft.Network/virtualNetworks/peer</span></span>|

<span data-ttu-id="62594-238">Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení konkrétní oprávnění příliš[vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="62594-238">Learn more about [built-in roles](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) and assigning specific permissions too[custom roles](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Resource Manager only).</span></span>

## <span data-ttu-id="62594-239"><a name="delete"></a>Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="62594-239"><a name="delete"></a>Delete resources</span></span>
<span data-ttu-id="62594-240">Po dokončení tohoto kurzu, můžete chtít toodelete hello prostředků, kterou jste vytvořili v kurzu hello, aby vám zbytečně nenabíhaly poplatky za používání.</span><span class="sxs-lookup"><span data-stu-id="62594-240">When you've finished this tutorial, you might want toodelete hello resources you created in hello tutorial, so you don't incur usage charges.</span></span> <span data-ttu-id="62594-241">Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="62594-241">Deleting a resource group also deletes all resources that are in hello resource group.</span></span>

### <span data-ttu-id="62594-242"><a name="delete-portal"></a>Portál Azure</span><span class="sxs-lookup"><span data-stu-id="62594-242"><a name="delete-portal"></a>Azure portal</span></span>

1. <span data-ttu-id="62594-243">Hello portálu vyhledávacího pole zadejte **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="62594-243">In hello portal search box, enter **myResourceGroup**.</span></span> <span data-ttu-id="62594-244">Ve výsledcích hledání hello, klikněte na tlačítko **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="62594-244">In hello search results, click **myResourceGroup**.</span></span>
2. <span data-ttu-id="62594-245">Na hello **myResourceGroup** okně klikněte na tlačítko hello **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="62594-245">On hello **myResourceGroup** blade, click hello **Delete** icon.</span></span>
3. <span data-ttu-id="62594-246">odstranění hello tooconfirm, v hello **typ hello název skupiny prostředků** zadejte **myResourceGroup**a potom klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="62594-246">tooconfirm hello deletion, in hello **TYPE hello RESOURCE GROUP NAME** box, enter **myResourceGroup**, and then click **Delete**.</span></span>

### <span data-ttu-id="62594-247"><a name="delete-cli"></a>Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="62594-247"><a name="delete-cli"></a>Azure CLI</span></span>

<span data-ttu-id="62594-248">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="62594-248">Enter hello following command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <span data-ttu-id="62594-249"><a name="delete-powershell"></a>Prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="62594-249"><a name="delete-powershell"></a>PowerShell</span></span>

<span data-ttu-id="62594-250">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="62594-250">Enter hello following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a><span data-ttu-id="62594-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62594-251">Next steps</span></span>

- <span data-ttu-id="62594-252">Důkladně Seznamte se s důležité [partnerského vztahu omezení virtuální sítě a chování](virtual-network-manage-peering.md#requirements-and-constraints) před vytvořením virtuální sítě partnerský vztah pro produkční použití.</span><span class="sxs-lookup"><span data-stu-id="62594-252">Thoroughly familiarize yourself with important [virtual network peering constraints and behaviors](virtual-network-manage-peering.md#requirements-and-constraints) before creating a virtual network peering for production use.</span></span>
- <span data-ttu-id="62594-253">Další informace o všech [partnerského vztahu nastavení virtuální sítě](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="62594-253">Learn about all [virtual network peering settings](virtual-network-manage-peering.md#create-a-peering).</span></span>
- <span data-ttu-id="62594-254">Zjistěte, jak příliš[vytvořit rozbočovače a příčkou topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) s partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="62594-254">Learn how too[create a hub and spoke network topology](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) with virtual network peering.</span></span>
