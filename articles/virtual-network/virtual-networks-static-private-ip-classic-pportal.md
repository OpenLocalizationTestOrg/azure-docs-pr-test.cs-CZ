---
title: "aaaConfigure privátních IP adres pro virtuální počítače (klasické) - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače (klasické) pomocí portálu Azure."
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
ms.openlocfilehash: df4bfa6768fc9e66db89785b633ffdb0274dbc46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-portal"></a><span data-ttu-id="b7371-103">Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b7371-103">Configure private IP addresses for a virtual machine (Classic) using hello Azure portal</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="b7371-104">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="b7371-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="b7371-105">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager hello](virtual-networks-static-private-ip-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="b7371-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="b7371-106">Hello ukázkový postup očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="b7371-106">hello sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="b7371-107">Pokud chcete toorun hello kroky, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="b7371-107">If you want toorun hello steps as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-classic-pportal.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="b7371-108">Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b7371-108">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="b7371-109">virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="b7371-109">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="b7371-110">V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b7371-110">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="b7371-111">Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že hello **vybrat model nasazení** seznam již ukazuje **Classic**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b7371-111">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that hello **Select a deployment model** list already shows **Classic**, and then click **Create**.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. <span data-ttu-id="b7371-113">V hello **vytvoření virtuálního počítače** okno, zadejte název hello toobe hello virtuálního počítače vytvořit (*DNS01* v tomto scénáři), hello účet místního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="b7371-113">In hello **Create VM** blade, enter hello name of hello VM toobe created (*DNS01* in our scenario), hello local administrator account, and password.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. <span data-ttu-id="b7371-115">Klikněte na tlačítko **volitelné konfiguraci** > **sítě** > **virtuální sítě**a potom klikněte na **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="b7371-115">Click **Optional Configuration** > **Network** > **Virtual Network**, and then click **TestVNet**.</span></span> <span data-ttu-id="b7371-116">Pokud **TestVNet** není k dispozici, ujistěte se, že používáte hello *střed USA* umístění a vytvořili hello testovací prostředí popsané na začátku hello v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="b7371-116">If **TestVNet** is not available, make sure you are using hello *Central US* location and have created hello test environment described at hello beginning of this article.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. <span data-ttu-id="b7371-118">V hello **sítě** okno, ujistěte se, hello podsíť aktuálně vybraný je *front-endu*, pak klikněte na tlačítko **IP adresy**v části **přiřazení IP adresy** klikněte na tlačítko **statické**a potom zadejte *192.168.1.101* pro **IP adresu** jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="b7371-118">In hello **Network** blade, make sure hello subnet currently selected is *FrontEnd*, then click **IP addresses**, under **IP address assignment** click **Static**, and then enter *192.168.1.101* for **IP Address** as seen below.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. <span data-ttu-id="b7371-120">Klikněte na tlačítko **OK** v hello **IP adresy** okno, pak klikněte na tlačítko **OK** v hello **sítě** a klikněte na **OK**v hello **volitelné konfigurace** okno.</span><span class="sxs-lookup"><span data-stu-id="b7371-120">Click **OK** in hello **IP addresses** blade, then click **OK** in hello **Network** blade, and click **OK** in hello **Optional config** blade.</span></span>
7. <span data-ttu-id="b7371-121">V hello **vytvoření virtuálního počítače** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b7371-121">In hello **Create VM** blade, click **Create**.</span></span> <span data-ttu-id="b7371-122">Všimněte si hello dlaždice níže zobrazí v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="b7371-122">Notice hello tile below displayed in your dashboard.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="b7371-124">Jak tooretrieve statickou privátní IP adresu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b7371-124">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="b7371-125">tooview hello privátní IP adresu informace statického pro hello, že virtuální počítač vytvořen s hello kroky uvedené výše, provést následující postup hello.</span><span class="sxs-lookup"><span data-stu-id="b7371-125">tooview hello static private IP address information for hello VM created with hello steps above, execute hello steps below.</span></span>

1. <span data-ttu-id="b7371-126">Z portálu Azure Azure hello, klikněte na tlačítko **Procházet vše** > **virtuálních počítačů (klasické)** > **DNS01**  >   **Všechna nastavení** > **IP adresy** a Všimněte si, jak je vidět níže hello přiřazení IP adresy a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b7371-126">From hello Azure Azure portal, click **BROWSE ALL** > **Virtual machines (classic)** > **DNS01** > **All settings** > **IP addresses** and notice hello IP address assignment and IP address as seen below.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="b7371-128">Jak tooremove statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b7371-128">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="b7371-129">tooremove hello statickou privátní IP adresou z hello, kterou virtuální počítač vytvořili výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="b7371-129">tooremove hello static private IP address from hello VM created above, follow hello steps below.</span></span>

1. <span data-ttu-id="b7371-130">Z hello **IP adresy** uvedené výše, klikněte na **dynamické** toohello napravo od **přiřazení IP adresy**, pak klikněte na tlačítko **Uložit**a potom Klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="b7371-130">From hello **IP addresses** blade shown above, click **Dynamic** toohello right of **IP address assignment**, then click **Save**, and then click **Yes**.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="b7371-132">Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b7371-132">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="b7371-133">tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí hello výše uvedených kroků, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="b7371-133">tooadd a static private IP address toohello VM created using hello steps above, follow hello steps below:</span></span>

1. <span data-ttu-id="b7371-134">Z hello **IP adresy** uvedené výše, klikněte na **statické** toohello napravo od **přiřazení IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="b7371-134">From hello **IP addresses** blade shown above, click **Static** toohello right of **IP address assignment**.</span></span>
2. <span data-ttu-id="b7371-135">Typ *192.168.1.101* pro **IP adresu**, pak klikněte na tlačítko **Uložit**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="b7371-135">Type *192.168.1.101* for **IP address**, then click **Save**, and then click **Yes**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7371-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7371-136">Next steps</span></span>
* <span data-ttu-id="b7371-137">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="b7371-137">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="b7371-138">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="b7371-138">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="b7371-139">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7371-139">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

