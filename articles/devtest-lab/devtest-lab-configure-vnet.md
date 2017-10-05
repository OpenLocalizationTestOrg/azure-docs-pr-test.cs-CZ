---
title: "Konfigurace virtuální sítě v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat existující virtuální síť a podsíť a využít ve virtuálním počítači s Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 848752085729df7d98a3a4b7be36d894c12cd033
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a><span data-ttu-id="964b1-103">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="964b1-103">Configure a virtual network in Azure DevTest Labs</span></span>
<span data-ttu-id="964b1-104">Jak je popsáno v článku, [Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md), při vytváření virtuálních počítačů v testovacím prostředí, můžete zadat nakonfigurované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="964b1-104">As explained in the article, [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md), when you create a VM in a lab, you can specify a configured virtual network.</span></span> <span data-ttu-id="964b1-105">Jeden scénář tohoto postupu je, pokud potřebujete přístup k prostředkům corpnet z virtuálních počítačů pomocí virtuální sítě, která byla konfigurovaná s ExpressRoute nebo VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="964b1-105">One scenario for doing this is if you need to access your corpnet resources from your VMs using the virtual network that was configured with ExpressRoute or site-to-site VPN.</span></span> <span data-ttu-id="964b1-106">Následující části ukazují, jak přidat existující virtuální sítě do testovacího prostředí na nastavení virtuální sítě, aby bylo možné zvolit při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="964b1-106">The following sections illustrate how to add your existing virtual network into a lab's Virtual Network settings so that it is available to choose when creating VMs.</span></span>

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a><span data-ttu-id="964b1-107">Konfigurace virtuální sítě pro testovacím prostředí pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="964b1-107">Configure a virtual network for a lab using the Azure portal</span></span>
<span data-ttu-id="964b1-108">Následující postup vás provede procesem přidání existující virtuální síť (a podsítě) do testovacího prostředí, aby se můžete použít při vytváření virtuálního počítače ve stejné testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="964b1-108">The following steps walk you through adding an existing virtual network (and subnet) to a lab so that it can be used when creating a VM in the same lab.</span></span> 

1. <span data-ttu-id="964b1-109">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="964b1-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="964b1-110">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="964b1-110">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="964b1-111">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="964b1-111">From the list of labs, select the desired lab.</span></span> 
4. <span data-ttu-id="964b1-112">V okně v prostředí, vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="964b1-112">On the lab's blade, select **Configuration**.</span></span>
5. <span data-ttu-id="964b1-113">V tomto prostředí **konfigurace** vyberte **virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="964b1-113">On the lab's **Configuration** blade, select **Virtual networks**.</span></span>
6. <span data-ttu-id="964b1-114">Na **virtuální sítě** okno, zobrazí se seznam virtuálních sítí, které jsou nakonfigurované pro aktuální prostředí a také výchozí virtuální síť, která se vytvoří pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="964b1-114">On the **Virtual networks** blade, you see a list of virtual networks configured for the current lab as well as the default virtual network that is created for your lab.</span></span> 
7. <span data-ttu-id="964b1-115">Vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="964b1-115">Select **+ Add**.</span></span>
   
    ![Přidat existující virtuální síť do testovacího prostředí](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. <span data-ttu-id="964b1-117">Na **virtuální síť** vyberte **[vyberte virtuální síť]**.</span><span class="sxs-lookup"><span data-stu-id="964b1-117">On the **Virtual network** blade, select **[Select virtual network]**.</span></span>
   
    ![Vyberte existující virtuální síť](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. <span data-ttu-id="964b1-119">Na **zvolte virtuální síť** okně, vyberte požadované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="964b1-119">On the **Choose virtual network** blade, select the desired virtual network.</span></span> <span data-ttu-id="964b1-120">V okně zobrazí všechny virtuální sítě, které jsou ve stejné oblasti v rámci předplatného jako testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="964b1-120">The blade shows all the virtual networks that are under the same region in the subscription as the lab.</span></span>  
10. <span data-ttu-id="964b1-121">Po výběru virtuální sítě, vrátíte se **virtuální síť** podsíť v seznamu v dolní části okna klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="964b1-121">After selecting a virtual network, you are returned to the **Virtual network** Click the subnet in the list at the bottom of the blade.</span></span>

    ![Seznam podsítí](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    <span data-ttu-id="964b1-123">Zobrazí se okno podsíť testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="964b1-123">The Lab Subnet blade is displayed.</span></span>

    ![Okno podsíť testovacího prostředí](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. <span data-ttu-id="964b1-125">Zadejte **název podsítě testovacího prostředí**.</span><span class="sxs-lookup"><span data-stu-id="964b1-125">Specify a **Lab subnet name**.</span></span>
12. <span data-ttu-id="964b1-126">Chcete-li povolit podsíť, kterou chcete použít v testovacím prostředí vytvoření virtuálního počítače, vyberte **použití v vytvoření virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="964b1-126">To allow a subnet to be used in lab VM creation, select **Use in virtual machine creation**.</span></span>
13. <span data-ttu-id="964b1-127">Chcete-li povolit [sdílené veřejnou IP adresu](devtest-lab-shared-ip.md), vyberte **povolení sdíleného veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="964b1-127">To enable a [shared public IP address](devtest-lab-shared-ip.md), select **Enable shared public IP**.</span></span>
14. <span data-ttu-id="964b1-128">Chcete-li povolit veřejné IP adresy v podsíti, vyberte **povolit vytvoření veřejné IP**.</span><span class="sxs-lookup"><span data-stu-id="964b1-128">To allow public IP addresses in a subnet, select **Allow public IP creation**.</span></span>
15. <span data-ttu-id="964b1-129">V **maximální počet virtuálních počítačů na uživatele** pole zadejte maximální virtuálních počítačů na uživatele pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="964b1-129">In the **Maximum virtual machines per user** field, specify the maximum VMs per user for each subnet.</span></span> <span data-ttu-id="964b1-130">Pokud chcete neomezený počet virtuálních počítačů, nechte pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="964b1-130">If you want an unrestricted number of VMs, leave this field blank.</span></span>
16. <span data-ttu-id="964b1-131">Vyberte **OK** zavřete okno podsíť testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="964b1-131">Select **OK** to close the Lab Subnet blade.</span></span>
17. <span data-ttu-id="964b1-132">Vyberte **Uložit** zavřete okno virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="964b1-132">Select **Save** to close the Virtual network blade.</span></span>
18. <span data-ttu-id="964b1-133">Teď, když je nakonfigurovaný virtuální sítě, můžete vybrat při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="964b1-133">Now that the virtual network is configured, it can be selected when creating a VM.</span></span> 
    <span data-ttu-id="964b1-134">Informace o tom, jak vytvořit virtuální počítač a zadejte virtuální síť, naleznete v článku [Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="964b1-134">To see how to create a VM and specify a virtual network, refer to the article, [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md).</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="964b1-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="964b1-135">Next steps</span></span>
<span data-ttu-id="964b1-136">Po přidání požadované virtuální sítě do vašeho testovacího prostředí, dalším krokem je [přidat virtuální počítač do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="964b1-136">Once you have added the desired virtual network to your lab, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

