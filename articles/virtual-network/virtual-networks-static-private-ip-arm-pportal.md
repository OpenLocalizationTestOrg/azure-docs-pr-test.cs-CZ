---
title: "Nakonfigurovat privátní IP adresy pro virtuální počítače - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat privátní IP adresy pro virtuální počítače pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 672462fad715758e50680fa5bade4b1f9d50e6e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal"></a><span data-ttu-id="f103d-103">Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f103d-103">Configure private IP addresses for a virtual machine using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f103d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f103d-104">Azure portal</span></span>](virtual-networks-static-private-ip-arm-pportal.md)
> * [<span data-ttu-id="f103d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f103d-105">PowerShell</span></span>](virtual-networks-static-private-ip-arm-ps.md)
> * [<span data-ttu-id="f103d-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f103d-106">Azure CLI</span></span>](virtual-networks-static-private-ip-arm-cli.md)
> * [<span data-ttu-id="f103d-107">Portál Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="f103d-107">Azure portal (Classic)</span></span>](virtual-networks-static-private-ip-classic-pportal.md)
> * [<span data-ttu-id="f103d-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="f103d-108">PowerShell (Classic)</span></span>](virtual-networks-static-private-ip-classic-ps.md)
> * [<span data-ttu-id="f103d-109">Rozhraní příkazového řádku Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="f103d-109">Azure CLI (Classic)</span></span>](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="f103d-110">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f103d-110">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="f103d-111">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic](virtual-networks-static-private-ip-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="f103d-111">You can also [manage static private IP address in the classic deployment model](virtual-networks-static-private-ip-classic-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="f103d-112">Následující ukázkový postup očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="f103d-112">The sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="f103d-113">Pokud chcete spustit kroky, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="f103d-113">If you want to run the steps as they are displayed in this document, first build the test environment described in [create a vnet](virtual-networks-create-vnet-arm-pportal.md).</span></span>

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a><span data-ttu-id="f103d-114">Postup vytvoření virtuálního počítače pro testování statickou privátní IP adresy</span><span class="sxs-lookup"><span data-stu-id="f103d-114">How to create a VM for testing static private IP addresses</span></span>
<span data-ttu-id="f103d-115">Statickou privátní IP adresu nelze nastavit při vytváření virtuálního počítače v režimu Resource Manager nasazení pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f103d-115">You cannot set a static private IP address during the creation of a VM in the Resource Manager deployment mode by using the Azure portal.</span></span> <span data-ttu-id="f103d-116">Je nutné nejprve vytvořit virtuální počítač, vložíte jej nastavit jeho privátní IP adresy na být statická.</span><span class="sxs-lookup"><span data-stu-id="f103d-116">You must create the VM first, tehn set its private IP to be static.</span></span>

<span data-ttu-id="f103d-117">Vytvoření virtuálního počítače s názvem *DNS01* v *front-endu* podsíť virtuální sítě s názvem *TestVNet*, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="f103d-117">To create a VM named *DNS01* in the *FrontEnd* subnet of a VNet named *TestVNet*, follow the steps below.</span></span>

1. <span data-ttu-id="f103d-118">V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="f103d-118">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="f103d-119">Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že **vybrat model nasazení** seznam již ukazuje **Resource Manager**a potom klikněte na **vytvořit**, jak je vidět na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="f103d-119">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that the **Select a deployment model** list already shows **Resource Manager**, and then click **Create**, as seen in the figure below.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. <span data-ttu-id="f103d-121">V **Základy** okno, zadejte název virtuálního počítače, který se má vytvořit (*DNS01* v tomto scénáři), účet místního správce a hesla, jak je vidět na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="f103d-121">In the **Basics** blade, enter the name of the VM to be created (*DNS01* in our scenario), the local administrator account, and password, as seen in the figure below.</span></span>
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. <span data-ttu-id="f103d-123">Zajistěte, aby **umístění** vybraná *střed USA*, pak klikněte na tlačítko **vyberte existující** v části **skupiny prostředků**, klikněte **skupiny prostředků** znovu, klikněte *TestRG*a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f103d-123">Make sure the **Location** selected is *Central US*, then click **Select existing** under **Resource group**, then click **Resource group** again, then click *TestRG*, and then click **OK**.</span></span>
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. <span data-ttu-id="f103d-125">V **zvolte velikost** vyberte **A1 standardní**a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="f103d-125">In the **Choose a size** blade, select **A1 Standard**, and then click **Select**.</span></span>
   
    ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. <span data-ttu-id="f103d-127">V **nastavení** okno, zkontrolujte následující vlastnosti jsou nastavené se nastavují s níže uvedené hodnoty a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f103d-127">In teh **Settings** blade, make sure the following properties are set are set with the values below, and then click **OK**.</span></span>
   
    <span data-ttu-id="f103d-128">-**Účet úložiště**: *vnetstorage*</span><span class="sxs-lookup"><span data-stu-id="f103d-128">-**Storage account**: *vnetstorage*</span></span>
   
   * <span data-ttu-id="f103d-129">**Síť**: *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="f103d-129">**Network**: *TestVNet*</span></span>
   * <span data-ttu-id="f103d-130">**Podsíť**: *front-endu*</span><span class="sxs-lookup"><span data-stu-id="f103d-130">**Subnet**: *FrontEnd*</span></span>
     
     ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. <span data-ttu-id="f103d-132">V **Souhrn** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f103d-132">In the **Summary** blade, click **OK**.</span></span> <span data-ttu-id="f103d-133">Všimněte si dlaždice níže zobrazí v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="f103d-133">Notice the tile below displayed in your dashboard.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="f103d-135">Jak načíst statickou privátní IP adresu informace pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f103d-135">How to retrieve static private IP address information for a VM</span></span>
<span data-ttu-id="f103d-136">Chcete-li zobrazit statických informací privátní IP adresu pro virtuální počítač vytvořený s výše uvedených kroků, proveďte následující postup.</span><span class="sxs-lookup"><span data-stu-id="f103d-136">To view the static private IP address information for the VM created with the steps above, execute the steps below.</span></span>

1. <span data-ttu-id="f103d-137">Z portálu Azure Azure, klikněte na tlačítko **Procházet vše** > **virtuální počítače** > **DNS01** > **všechna nastavení** > **síťových rozhraní** a potom klikněte na uvedené jenom síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f103d-137">From the Azure Azure portal, click **BROWSE ALL** > **Virtual machines** > **DNS01** > **All settings** > **Network interfaces** and then click on the only network interface listed.</span></span>
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. <span data-ttu-id="f103d-139">V **síťové rozhraní** okně klikněte na tlačítko **všechna nastavení** > **IP adresy** a Všimněte si **přiřazení** a **IP adresu** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f103d-139">In the **Network interface** blade, click **All settings** > **IP addresses** and notice the **Assignment** and **IP address** values.</span></span>
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a><span data-ttu-id="f103d-141">Postup přidání statickou privátní IP adresu do stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f103d-141">How to add a static private IP address to an existing VM</span></span>
<span data-ttu-id="f103d-142">Chcete-li přidat statickou privátní IP adresu na virtuální počítač vytvořený pomocí výše uvedených kroků, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f103d-142">To add a static private IP address to the VM created using the steps above, follow the steps below:</span></span>

1. <span data-ttu-id="f103d-143">Z **IP adresy** uvedené výše, klikněte na **statické** pod **přiřazení**.</span><span class="sxs-lookup"><span data-stu-id="f103d-143">From the **IP addresses** blade shown above, click **Static** under **Assignment**.</span></span>
2. <span data-ttu-id="f103d-144">Typ *192.168.1.101* pro **IP adresu**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f103d-144">Type *192.168.1.101* for **IP address**, and then click **Save**.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> <span data-ttu-id="f103d-146">Pokud po kliknutí na **Uložit** si všimnete, že je přiřazení stále nastavená na **dynamické**, znamená to, že zadaná adresa IP je již používán.</span><span class="sxs-lookup"><span data-stu-id="f103d-146">If after clicking **Save** you notice that the assignment is still set to **Dynamic**, it means that the IP address you typed is already in use.</span></span> <span data-ttu-id="f103d-147">Zkuste jinou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f103d-147">Try a different IP address.</span></span>
> 
> 

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="f103d-148">Postup odebrání statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f103d-148">How to remove a static private IP address from a VM</span></span>
<span data-ttu-id="f103d-149">Chcete-li odebrat statickou privátní IP adresou z virtuálního počítače vytvořili výše, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="f103d-149">To remove the static private IP address from the VM created above, complete the following step:</span></span>

<span data-ttu-id="f103d-150">Z **IP adresy** uvedené výše, klikněte na **dynamické** pod **přiřazení**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f103d-150">From the **IP addresses** blade shown above, click **Dynamic** under **Assignment**, and then click **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f103d-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f103d-151">Next steps</span></span>
* <span data-ttu-id="f103d-152">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="f103d-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="f103d-153">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="f103d-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="f103d-154">Obrátit [vyhrazené IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="f103d-154">Consult the [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

