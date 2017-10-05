---
title: "Postup resetování síťové rozhraní virtuálního počítače Windows Azure | Microsoft Docs"
description: "Ukazuje, jak resetování síťové rozhraní virtuálního počítače Windows Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.openlocfilehash: 220e426be20086841854d89831f6c9d67529867f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a><span data-ttu-id="12a4c-103">Postup resetování síťové rozhraní virtuálního počítače Windows Azure</span><span class="sxs-lookup"><span data-stu-id="12a4c-103">How to reset network interface for Azure Windows VM</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="12a4c-104">Nemůžete se připojit k Microsoft Azure Windows virtuální počítač (VM), po zakázání výchozí síťové rozhraní (NIC) nebo ručně nastaví statickou IP adresu pro síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="12a4c-104">You cannot connect to Microsoft Azure Windows Virtual Machine (VM) after you disable the default Network Interface (NIC) or manually sets a static IP for the NIC.</span></span> <span data-ttu-id="12a4c-105">Tento článek ukazuje, jak resetovat rozhraní sítě pro virtuální počítač Windows Azure, který bude vyřešen problém vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="12a4c-105">This article shows how to reset the network interface for Azure Windows VM, which will resolve the remote connection issue.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a><span data-ttu-id="12a4c-106">Resetování síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="12a4c-106">Reset network interface</span></span>

### <a name="for-classic-vms"></a><span data-ttu-id="12a4c-107">Pro klasické virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="12a4c-107">For Classic VMs</span></span>

<span data-ttu-id="12a4c-108">Chcete-li obnovit síťové rozhraní, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="12a4c-108">To reset network interface, follow these steps:</span></span>

1.  <span data-ttu-id="12a4c-109">Přejděte na [portál Azure]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="12a4c-109">Go to the [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="12a4c-110">Vyberte **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-110">Select **Virtual Machines (Classic)**.</span></span>
3.  <span data-ttu-id="12a4c-111">Vyberte ovlivněné virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="12a4c-111">Select the affected Virtual Machine.</span></span>
4.  <span data-ttu-id="12a4c-112">Vyberte **IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-112">Select **IP addresses**.</span></span>
5.  <span data-ttu-id="12a4c-113">Pokud **privátní IP přiřazení** není **statické**, změňte ho na **statické**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-113">If the **Private IP assignment**  is not  **Static**, change it to **Static**.</span></span>
6.  <span data-ttu-id="12a4c-114">Změna **IP adresu** jinou IP adresu, která je dostupná v podsíti.</span><span class="sxs-lookup"><span data-stu-id="12a4c-114">Change the **IP address** to another IP address that is available in the Subnet.</span></span>
7.  <span data-ttu-id="12a4c-115">Vyberte Uložit.</span><span class="sxs-lookup"><span data-stu-id="12a4c-115">Select Save.</span></span>
8.  <span data-ttu-id="12a4c-116">Virtuální počítač se restartuje k chybě při inicializaci nového síťového adaptéru do systému.</span><span class="sxs-lookup"><span data-stu-id="12a4c-116">The virtual machine will restart to initialize the new NIC to the system.</span></span>
9.  <span data-ttu-id="12a4c-117">Zkuste pro připojení RDP k vašemu počítači.</span><span class="sxs-lookup"><span data-stu-id="12a4c-117">Try to RDP to your machine.</span></span> <span data-ttu-id="12a4c-118">Pokud bylo úspěšné, můžete změnit privátní IP adresu zpět do původního Pokud vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="12a4c-118">If successful, you can change the Private IP address back to the original if you would like.</span></span> <span data-ttu-id="12a4c-119">Jinak můžete ponechat ji.</span><span class="sxs-lookup"><span data-stu-id="12a4c-119">Otherwise, you can keep it.</span></span> 

### <a name="for-vms-deployed-in-resource-group-model"></a><span data-ttu-id="12a4c-120">Pro virtuální počítače nasazené v modelu skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="12a4c-120">For VMs deployed in Resource group model</span></span>

1.  <span data-ttu-id="12a4c-121">Přejděte na [portál Azure]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="12a4c-121">Go to the [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="12a4c-122">Vyberte ovlivněné virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="12a4c-122">Select the affected Virtual Machine.</span></span>
3.  <span data-ttu-id="12a4c-123">Vyberte **síťových rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-123">Select **Network Interfaces**.</span></span>
4.  <span data-ttu-id="12a4c-124">Vyberte síťové rozhraní přidružené k počítači</span><span class="sxs-lookup"><span data-stu-id="12a4c-124">Select the Network Interface associated with your machine</span></span>
5.  <span data-ttu-id="12a4c-125">Vyberte **konfigurace protokolu IP**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-125">Select **IP configurations**.</span></span>
6.  <span data-ttu-id="12a4c-126">Vyberte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="12a4c-126">Select the IP.</span></span> 
7.  <span data-ttu-id="12a4c-127">Pokud **privátní IP přiřazení** není **statické**, změňte ho na **statické**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-127">If the **Private IP assignment**  is not  **Static**, change it to **Static**.</span></span>
8.  <span data-ttu-id="12a4c-128">Změna **IP adresu** jinou IP adresu, která je dostupná v podsíti.</span><span class="sxs-lookup"><span data-stu-id="12a4c-128">Change the **IP address** to another IP address that is available in the Subnet.</span></span>
9. <span data-ttu-id="12a4c-129">Virtuální počítač se restartuje k chybě při inicializaci nového síťového adaptéru do systému.</span><span class="sxs-lookup"><span data-stu-id="12a4c-129">The virtual machine will restart to initialize the new NIC to the system.</span></span>
10. <span data-ttu-id="12a4c-130">Zkuste pro připojení RDP k vašemu počítači.</span><span class="sxs-lookup"><span data-stu-id="12a4c-130">Try to RDP to your machine.</span></span> <span data-ttu-id="12a4c-131">Pokud bylo úspěšné, můžete změnit privátní IP adresu zpět do původního Pokud vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="12a4c-131">If successful, you can change the Private IP address back to the original if you would like.</span></span> <span data-ttu-id="12a4c-132">Jinak můžete ponechat ji.</span><span class="sxs-lookup"><span data-stu-id="12a4c-132">Otherwise, you can keep it.</span></span> 

## <a name="delete-the-unavailable-nics"></a><span data-ttu-id="12a4c-133">Odstranit není k dispozici síťové karty</span><span class="sxs-lookup"><span data-stu-id="12a4c-133">Delete the unavailable NICs</span></span>
<span data-ttu-id="12a4c-134">Poté, co je to možné vzdálené plochy k počítači, je nutné odstranit staré síťové adaptéry, aby se zabránilo potenciální problém:</span><span class="sxs-lookup"><span data-stu-id="12a4c-134">After you can remote desktop to the machine, you must delete the old NICs to avoid the potential problem:</span></span>

1.  <span data-ttu-id="12a4c-135">Otevřete Správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="12a4c-135">Open Device Manager.</span></span>
2.  <span data-ttu-id="12a4c-136">Vyberte **zobrazení** > **zobrazit skrytá zařízení**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-136">Select **View** > **Show hidden devices**.</span></span>
3.  <span data-ttu-id="12a4c-137">Vyberte **síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="12a4c-137">Select **Network Adapters**.</span></span> 
4.  <span data-ttu-id="12a4c-138">Zkontrolujte u adaptérů s názvem jako "Microsoft Hyper-V síťový adaptér".</span><span class="sxs-lookup"><span data-stu-id="12a4c-138">Check for the adapters named as "Microsoft Hyper-V Network Adapter".</span></span>
5.  <span data-ttu-id="12a4c-139">Může se zobrazit není k dispozici adaptér, který je zobrazena šedě.</span><span class="sxs-lookup"><span data-stu-id="12a4c-139">You might see an unavailable adapter that is grayed out.</span></span> <span data-ttu-id="12a4c-140">Klikněte pravým tlačítkem na adaptéru a potom vyberte možnost odinstalovat.</span><span class="sxs-lookup"><span data-stu-id="12a4c-140">Right-click the adapter and then select Uninstall.</span></span>

    ![bitové kopie na síťový adaptér](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > <span data-ttu-id="12a4c-142">Není k dispozici adaptéry, které mají název "Microsoft Hyper-V síťový adaptér" pouze odinstalujte.</span><span class="sxs-lookup"><span data-stu-id="12a4c-142">Only uninstall the unavailable adapters that have the name "Microsoft Hyper-V Network Adapter".</span></span> <span data-ttu-id="12a4c-143">Pokud odinstalujete všechny ostatní skrytá adaptéry, může způsobit další problémy.</span><span class="sxs-lookup"><span data-stu-id="12a4c-143">If you uninstall any of the other hidden adapters, it could cause additional issues.</span></span>
    >
    >

6.  <span data-ttu-id="12a4c-144">Nyní všechny adaptér není k dispozici je nutné vymazat z vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="12a4c-144">Now all unavailable adapter should be cleared out from your system.</span></span>