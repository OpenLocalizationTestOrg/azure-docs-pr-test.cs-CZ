---
title: "aaaConfigure virtuální sítě v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooconfigure existující virtuální síť a podsíť a použít na virtuální počítač s Azure DevTest Labs"
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
ms.openlocfilehash: a11ce8315e3c540e44aeacc9c5ee3dde014d4621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a><span data-ttu-id="77849-103">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="77849-103">Configure a virtual network in Azure DevTest Labs</span></span>
<span data-ttu-id="77849-104">Jak je popsáno v článku hello [přidat virtuální počítač s testovacím tooa artefakty](devtest-lab-add-vm-with-artifacts.md), při vytváření virtuálních počítačů v testovacím prostředí, určíte nakonfigurovaný virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="77849-104">As explained in hello article, [Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md), when you create a VM in a lab, you can specify a configured virtual network.</span></span> <span data-ttu-id="77849-105">Jeden scénář tohoto postupu je, pokud potřebujete tooaccess hello prostředkům corpnet z virtuálních počítačů pomocí virtuální sítě, která byla konfigurovaná s ExpressRoute nebo VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="77849-105">One scenario for doing this is if you need tooaccess your corpnet resources from your VMs using hello virtual network that was configured with ExpressRoute or site-to-site VPN.</span></span> <span data-ttu-id="77849-106">znázornění technologie Hello následující části Jak tooadd existující virtuální sítě do testovacího prostředí na nastavení virtuální sítě, aby byla k dispozici toochoose při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="77849-106">hello following sections illustrate how tooadd your existing virtual network into a lab's Virtual Network settings so that it is available toochoose when creating VMs.</span></span>

## <a name="configure-a-virtual-network-for-a-lab-using-hello-azure-portal"></a><span data-ttu-id="77849-107">Konfigurace virtuální sítě pro testovacím prostředí pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="77849-107">Configure a virtual network for a lab using hello Azure portal</span></span>
<span data-ttu-id="77849-108">Hello následující kroky vás provede přidáním existující virtuální síť (a podsíť) tooa testovacího prostředí, aby se můžete použít při vytváření virtuálního počítače v hello stejné testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="77849-108">hello following steps walk you through adding an existing virtual network (and subnet) tooa lab so that it can be used when creating a VM in hello same lab.</span></span> 

1. <span data-ttu-id="77849-109">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="77849-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="77849-110">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="77849-110">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="77849-111">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="77849-111">From hello list of labs, select hello desired lab.</span></span> 
4. <span data-ttu-id="77849-112">V okně prostředí hello vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="77849-112">On hello lab's blade, select **Configuration**.</span></span>
5. <span data-ttu-id="77849-113">V testovacím hello **konfigurace** vyberte **virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="77849-113">On hello lab's **Configuration** blade, select **Virtual networks**.</span></span>
6. <span data-ttu-id="77849-114">Na hello **virtuální sítě** okno, zobrazí se seznam virtuálních sítí, které jsou nakonfigurované pro aktuální prostředí hello a také výchozí hello virtuální se sítí, který se vytvoří pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="77849-114">On hello **Virtual networks** blade, you see a list of virtual networks configured for hello current lab as well as hello default virtual network that is created for your lab.</span></span> 
7. <span data-ttu-id="77849-115">Vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="77849-115">Select **+ Add**.</span></span>
   
    ![Přidat existující tooyour laboratoře virtuální sítě](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. <span data-ttu-id="77849-117">Na hello **virtuální síť** vyberte **[vyberte virtuální síť]**.</span><span class="sxs-lookup"><span data-stu-id="77849-117">On hello **Virtual network** blade, select **[Select virtual network]**.</span></span>
   
    ![Vyberte existující virtuální síť](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. <span data-ttu-id="77849-119">Na hello **zvolte virtuální síť** okně, vyberte hello požadované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="77849-119">On hello **Choose virtual network** blade, select hello desired virtual network.</span></span> <span data-ttu-id="77849-120">Hello se zobrazí okno všechny virtuální sítě hello, které jsou v části hello stejné oblasti v rámci předplatného hello jako hello testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="77849-120">hello blade shows all hello virtual networks that are under hello same region in hello subscription as hello lab.</span></span>  
10. <span data-ttu-id="77849-121">Po výběru virtuální sítě, jsou vráceny toohello **virtuální síť** klikněte na tlačítko hello podsíť v seznamu hello v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="77849-121">After selecting a virtual network, you are returned toohello **Virtual network** Click hello subnet in hello list at hello bottom of hello blade.</span></span>

    ![Seznam podsítí](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    <span data-ttu-id="77849-123">Zobrazí se okno Hello podsíť testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="77849-123">hello Lab Subnet blade is displayed.</span></span>

    ![Okno podsíť testovacího prostředí](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. <span data-ttu-id="77849-125">Zadejte **název podsítě testovacího prostředí**.</span><span class="sxs-lookup"><span data-stu-id="77849-125">Specify a **Lab subnet name**.</span></span>
12. <span data-ttu-id="77849-126">tooallow podsíť toobe, používá v testovacím prostředí vytvoření virtuálního počítače, vyberte **použití v vytvoření virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="77849-126">tooallow a subnet toobe used in lab VM creation, select **Use in virtual machine creation**.</span></span>
13. <span data-ttu-id="77849-127">tooenable [sdílené veřejnou IP adresu](devtest-lab-shared-ip.md), vyberte **povolení sdíleného veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="77849-127">tooenable a [shared public IP address](devtest-lab-shared-ip.md), select **Enable shared public IP**.</span></span>
14. <span data-ttu-id="77849-128">Vyberte tooallow veřejné IP adresy v podsíti, **povolit vytvoření veřejné IP**.</span><span class="sxs-lookup"><span data-stu-id="77849-128">tooallow public IP addresses in a subnet, select **Allow public IP creation**.</span></span>
15. <span data-ttu-id="77849-129">V hello **maximální počet virtuálních počítačů na uživatele** pole, zadejte text hello maximální virtuálních počítačů na uživatele pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="77849-129">In hello **Maximum virtual machines per user** field, specify hello maximum VMs per user for each subnet.</span></span> <span data-ttu-id="77849-130">Pokud chcete neomezený počet virtuálních počítačů, nechte pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="77849-130">If you want an unrestricted number of VMs, leave this field blank.</span></span>
16. <span data-ttu-id="77849-131">Vyberte **OK** tooclose hello podsíť testovacího prostředí okno.</span><span class="sxs-lookup"><span data-stu-id="77849-131">Select **OK** tooclose hello Lab Subnet blade.</span></span>
17. <span data-ttu-id="77849-132">Vyberte **Uložit** tooclose hello virtuální sítě okno.</span><span class="sxs-lookup"><span data-stu-id="77849-132">Select **Save** tooclose hello Virtual network blade.</span></span>
18. <span data-ttu-id="77849-133">Teď, když hello virtuální síť nakonfigurované, můžete vybrat při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="77849-133">Now that hello virtual network is configured, it can be selected when creating a VM.</span></span> 
    <span data-ttu-id="77849-134">toosee jak toocreate virtuálního počítače a zadejte virtuální síť, najdete v článku toohello [přidat virtuální počítač s testovacím tooa artefakty](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="77849-134">toosee how toocreate a VM and specify a virtual network, refer toohello article, [Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md).</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="77849-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77849-135">Next steps</span></span>
<span data-ttu-id="77849-136">Po přidání hello potřeby testovacím tooyour virtuální sítě, hello dalším krokem je příliš[přidání testovacího prostředí virtuálních počítačů tooyour](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="77849-136">Once you have added hello desired virtual network tooyour lab, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

