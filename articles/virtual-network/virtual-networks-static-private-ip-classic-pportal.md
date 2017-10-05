---
title: "Nakonfigurovat privátní IP adresy pro virtuální počítače (klasické) - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat privátní IP adresy pro virtuální počítače (klasické) pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a><span data-ttu-id="0dd00-103">Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0dd00-103">Configure private IP addresses for a virtual machine (Classic) using the Azure portal</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="0dd00-104">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="0dd00-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="0dd00-105">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager](virtual-networks-static-private-ip-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="0dd00-105">You can also [manage a static private IP address in the Resource Manager deployment model](virtual-networks-static-private-ip-arm-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="0dd00-106">Následující ukázkový postup očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="0dd00-106">The sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="0dd00-107">Pokud chcete spustit kroky, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="0dd00-107">If you want to run the steps as they are displayed in this document, first build the test environment described in [create a vnet](virtual-networks-create-vnet-classic-pportal.md).</span></span>

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="0dd00-108">Postup při vytváření virtuálního počítače zadat statickou privátní IP adresu</span><span class="sxs-lookup"><span data-stu-id="0dd00-108">How to specify a static private IP address when creating a VM</span></span>
<span data-ttu-id="0dd00-109">Vytvoření virtuálního počítače s názvem *DNS01* v *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="0dd00-109">To create a VM named *DNS01* in the *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow the steps below:</span></span>

1. <span data-ttu-id="0dd00-110">V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="0dd00-110">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="0dd00-111">Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že **vybrat model nasazení** seznam již ukazuje **Classic**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0dd00-111">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that the **Select a deployment model** list already shows **Classic**, and then click **Create**.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. <span data-ttu-id="0dd00-113">V **vytvoření virtuálního počítače** okno, zadejte název virtuálního počítače, který se má vytvořit (*DNS01* v tomto scénáři), účet místního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="0dd00-113">In the **Create VM** blade, enter the name of the VM to be created (*DNS01* in our scenario), the local administrator account, and password.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. <span data-ttu-id="0dd00-115">Klikněte na tlačítko **volitelné konfiguraci** > **sítě** > **virtuální sítě**a potom klikněte na **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="0dd00-115">Click **Optional Configuration** > **Network** > **Virtual Network**, and then click **TestVNet**.</span></span> <span data-ttu-id="0dd00-116">Pokud **TestVNet** není k dispozici, ujistěte se, že používáte *střed USA* umístění a vytvořili testovací prostředí popsané na začátku tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="0dd00-116">If **TestVNet** is not available, make sure you are using the *Central US* location and have created the test environment described at the beginning of this article.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. <span data-ttu-id="0dd00-118">V **sítě** okno, zkontrolujte, zda je aktuálně vybrané podsítě *front-endu*, pak klikněte na tlačítko **IP adresy**v části **přiřazení IP adresy** klikněte na tlačítko **statické**a potom zadejte *192.168.1.101* pro **IP adresu** jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="0dd00-118">In the **Network** blade, make sure the subnet currently selected is *FrontEnd*, then click **IP addresses**, under **IP address assignment** click **Static**, and then enter *192.168.1.101* for **IP Address** as seen below.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. <span data-ttu-id="0dd00-120">Klikněte na tlačítko **OK** v **IP adresy** okno, pak klikněte na tlačítko **OK** v **sítě** a klikněte na **OK** v **volitelné konfigurace** okno.</span><span class="sxs-lookup"><span data-stu-id="0dd00-120">Click **OK** in the **IP addresses** blade, then click **OK** in the **Network** blade, and click **OK** in the **Optional config** blade.</span></span>
7. <span data-ttu-id="0dd00-121">V **vytvoření virtuálního počítače** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0dd00-121">In the **Create VM** blade, click **Create**.</span></span> <span data-ttu-id="0dd00-122">Všimněte si dlaždice níže zobrazí v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="0dd00-122">Notice the tile below displayed in your dashboard.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="0dd00-124">Jak načíst statickou privátní IP adresu informace pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="0dd00-124">How to retrieve static private IP address information for a VM</span></span>
<span data-ttu-id="0dd00-125">Chcete-li zobrazit statických informací privátní IP adresu pro virtuální počítač vytvořený s výše uvedených kroků, proveďte následující postup.</span><span class="sxs-lookup"><span data-stu-id="0dd00-125">To view the static private IP address information for the VM created with the steps above, execute the steps below.</span></span>

1. <span data-ttu-id="0dd00-126">Z portálu Azure Azure, klikněte na tlačítko **Procházet vše** > **virtuálních počítačů (klasické)** > **DNS01** > **všechna nastavení** > **IP adresy** a Všimněte si přiřazení IP adresy a IP adresu, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="0dd00-126">From the Azure Azure portal, click **BROWSE ALL** > **Virtual machines (classic)** > **DNS01** > **All settings** > **IP addresses** and notice the IP address assignment and IP address as seen below.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="0dd00-128">Postup odebrání statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0dd00-128">How to remove a static private IP address from a VM</span></span>
<span data-ttu-id="0dd00-129">Chcete-li odebrat statickou privátní IP adresou z virtuálního počítače vytvořili výše, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="0dd00-129">To remove the static private IP address from the VM created above, follow the steps below.</span></span>

1. <span data-ttu-id="0dd00-130">Z **IP adresy** uvedené výše, klikněte na **dynamické** napravo od **přiřazení IP adresy**, pak klikněte na tlačítko **Uložit**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0dd00-130">From the **IP addresses** blade shown above, click **Dynamic** to the right of **IP address assignment**, then click **Save**, and then click **Yes**.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a><span data-ttu-id="0dd00-132">Postup přidání statickou privátní IP adresu do stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0dd00-132">How to add a static private IP address to an existing VM</span></span>
<span data-ttu-id="0dd00-133">Chcete-li přidat statickou privátní IP adresu na virtuální počítač vytvořený pomocí výše uvedených kroků, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="0dd00-133">To add a static private IP address to the VM created using the steps above, follow the steps below:</span></span>

1. <span data-ttu-id="0dd00-134">Z **IP adresy** uvedené výše, klikněte na **statické** napravo od **přiřazení IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="0dd00-134">From the **IP addresses** blade shown above, click **Static** to the right of **IP address assignment**.</span></span>
2. <span data-ttu-id="0dd00-135">Typ *192.168.1.101* pro **IP adresu**, pak klikněte na tlačítko **Uložit**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0dd00-135">Type *192.168.1.101* for **IP address**, then click **Save**, and then click **Yes**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0dd00-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0dd00-136">Next steps</span></span>
* <span data-ttu-id="0dd00-137">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="0dd00-137">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="0dd00-138">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="0dd00-138">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="0dd00-139">Obrátit [vyhrazené IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="0dd00-139">Consult the [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

