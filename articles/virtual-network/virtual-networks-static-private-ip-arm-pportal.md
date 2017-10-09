---
title: "aaaConfigure privátních IP adres pro virtuální počítače - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače pomocí portálu Azure."
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
ms.openlocfilehash: 474161303cdf8cb98e16ffd7cef6b74debdbc49a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-portal"></a><span data-ttu-id="65f92-103">Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="65f92-103">Configure private IP addresses for a virtual machine using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="65f92-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="65f92-104">Azure portal</span></span>](virtual-networks-static-private-ip-arm-pportal.md)
> * [<span data-ttu-id="65f92-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="65f92-105">PowerShell</span></span>](virtual-networks-static-private-ip-arm-ps.md)
> * [<span data-ttu-id="65f92-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="65f92-106">Azure CLI</span></span>](virtual-networks-static-private-ip-arm-cli.md)
> * [<span data-ttu-id="65f92-107">Portál Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="65f92-107">Azure portal (Classic)</span></span>](virtual-networks-static-private-ip-classic-pportal.md)
> * [<span data-ttu-id="65f92-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="65f92-108">PowerShell (Classic)</span></span>](virtual-networks-static-private-ip-classic-ps.md)
> * [<span data-ttu-id="65f92-109">Rozhraní příkazového řádku Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="65f92-109">Azure CLI (Classic)</span></span>](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="65f92-110">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="65f92-110">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="65f92-111">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="65f92-111">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="65f92-112">Hello ukázkový postup očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="65f92-112">hello sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="65f92-113">Pokud chcete toorun hello kroky, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="65f92-113">If you want toorun hello steps as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-pportal.md).</span></span>

## <a name="how-toocreate-a-vm-for-testing-static-private-ip-addresses"></a><span data-ttu-id="65f92-114">Jak toocreate virtuálního počítače pro testování statickou privátní IP adresy</span><span class="sxs-lookup"><span data-stu-id="65f92-114">How toocreate a VM for testing static private IP addresses</span></span>
<span data-ttu-id="65f92-115">Nelze nastavit statickou privátní IP adresou během vytváření hello virtuálního počítače v režimu nasazení Resource Manager hello pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="65f92-115">You cannot set a static private IP address during hello creation of a VM in hello Resource Manager deployment mode by using hello Azure portal.</span></span> <span data-ttu-id="65f92-116">Je nutné nejprve vytvořit hello virtuálních počítačů, vložíte jej nastavit jeho privátní IP toobe statické.</span><span class="sxs-lookup"><span data-stu-id="65f92-116">You must create hello VM first, tehn set its private IP toobe static.</span></span>

<span data-ttu-id="65f92-117">virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet*, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="65f92-117">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet*, follow hello steps below.</span></span>

1. <span data-ttu-id="65f92-118">V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="65f92-118">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="65f92-119">Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že hello **vybrat model nasazení** seznam již ukazuje **Resource Manager**a potom klikněte na **vytvořit**, jak je vidět na následujícím obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="65f92-119">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that hello **Select a deployment model** list already shows **Resource Manager**, and then click **Create**, as seen in hello figure below.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. <span data-ttu-id="65f92-121">V hello **Základy** okno, zadejte název hello toobe hello virtuálního počítače vytvořit (*DNS01* v tomto scénáři), hello účet místního správce a hesla, jak je vidět na následujícím obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="65f92-121">In hello **Basics** blade, enter hello name of hello VM toobe created (*DNS01* in our scenario), hello local administrator account, and password, as seen in hello figure below.</span></span>
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. <span data-ttu-id="65f92-123">Ujistěte se, zda text hello **umístění** vybraná *střed USA*, pak klikněte na tlačítko **vyberte existující** pod **skupiny prostředků**, pak klikněte na tlačítko **Skupiny prostředků** znovu, pak klikněte na tlačítko *TestRG*a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="65f92-123">Make sure hello **Location** selected is *Central US*, then click **Select existing** under **Resource group**, then click **Resource group** again, then click *TestRG*, and then click **OK**.</span></span>
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. <span data-ttu-id="65f92-125">V hello **zvolte velikost** vyberte **A1 standardní**a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="65f92-125">In hello **Choose a size** blade, select **A1 Standard**, and then click **Select**.</span></span>
   
    ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. <span data-ttu-id="65f92-127">V **nastavení** okno, ujistěte se, hello následující vlastnosti jsou nastaveny se nastavují s hello níže uvedené hodnoty a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="65f92-127">In teh **Settings** blade, make sure hello following properties are set are set with hello values below, and then click **OK**.</span></span>
   
    <span data-ttu-id="65f92-128">-**Účet úložiště**: *vnetstorage*</span><span class="sxs-lookup"><span data-stu-id="65f92-128">-**Storage account**: *vnetstorage*</span></span>
   
   * <span data-ttu-id="65f92-129">**Síť**: *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="65f92-129">**Network**: *TestVNet*</span></span>
   * <span data-ttu-id="65f92-130">**Podsíť**: *front-endu*</span><span class="sxs-lookup"><span data-stu-id="65f92-130">**Subnet**: *FrontEnd*</span></span>
     
     ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. <span data-ttu-id="65f92-132">V hello **Souhrn** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="65f92-132">In hello **Summary** blade, click **OK**.</span></span> <span data-ttu-id="65f92-133">Všimněte si hello dlaždice níže zobrazí v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="65f92-133">Notice hello tile below displayed in your dashboard.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="65f92-135">Jak tooretrieve statickou privátní IP adresu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="65f92-135">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="65f92-136">tooview hello privátní IP adresu informace statického pro hello, že virtuální počítač vytvořen s hello kroky uvedené výše, provést následující postup hello.</span><span class="sxs-lookup"><span data-stu-id="65f92-136">tooview hello static private IP address information for hello VM created with hello steps above, execute hello steps below.</span></span>

1. <span data-ttu-id="65f92-137">Z portálu Azure Azure hello, klikněte na tlačítko **Procházet vše** > **virtuální počítače** > **DNS01** > **všechny nastavení** > **síťových rozhraní** a potom klikněte na hello pouze uvedené rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="65f92-137">From hello Azure Azure portal, click **BROWSE ALL** > **Virtual machines** > **DNS01** > **All settings** > **Network interfaces** and then click on hello only network interface listed.</span></span>
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. <span data-ttu-id="65f92-139">V hello **síťové rozhraní** okně klikněte na tlačítko **všechna nastavení** > **IP adresy** a Všimněte si hello **přiřazení** a **IP adresu** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="65f92-139">In hello **Network interface** blade, click **All settings** > **IP addresses** and notice hello **Assignment** and **IP address** values.</span></span>
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="65f92-141">Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="65f92-141">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="65f92-142">tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí hello výše uvedených kroků, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="65f92-142">tooadd a static private IP address toohello VM created using hello steps above, follow hello steps below:</span></span>

1. <span data-ttu-id="65f92-143">Z hello **IP adresy** uvedené výše, klikněte na **statické** pod **přiřazení**.</span><span class="sxs-lookup"><span data-stu-id="65f92-143">From hello **IP addresses** blade shown above, click **Static** under **Assignment**.</span></span>
2. <span data-ttu-id="65f92-144">Typ *192.168.1.101* pro **IP adresu**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="65f92-144">Type *192.168.1.101* for **IP address**, and then click **Save**.</span></span>
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> <span data-ttu-id="65f92-146">Pokud po kliknutí na **Uložit** si všimnete, že přiřazení hello je stále nastavené příliš**dynamické**, znamená to, že jste zadali hello IP adresa je již používán.</span><span class="sxs-lookup"><span data-stu-id="65f92-146">If after clicking **Save** you notice that hello assignment is still set too**Dynamic**, it means that hello IP address you typed is already in use.</span></span> <span data-ttu-id="65f92-147">Zkuste jinou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="65f92-147">Try a different IP address.</span></span>
> 
> 

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="65f92-148">Jak tooremove statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="65f92-148">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="65f92-149">tooremove hello statickou privátní IP adresou z hello, kterou virtuální počítač vytvořili výše, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="65f92-149">tooremove hello static private IP address from hello VM created above, complete hello following step:</span></span>

<span data-ttu-id="65f92-150">Z hello **IP adresy** uvedené výše, klikněte na **dynamické** pod **přiřazení**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="65f92-150">From hello **IP addresses** blade shown above, click **Dynamic** under **Assignment**, and then click **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65f92-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65f92-151">Next steps</span></span>
* <span data-ttu-id="65f92-152">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="65f92-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="65f92-153">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="65f92-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="65f92-154">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="65f92-154">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

