---
title: "aaaHow tooreset síťové rozhraní pro virtuální počítač s Windows Azure | Microsoft Docs"
description: "Ukazuje, jak tooreset síťové rozhraní pro virtuální počítač s Windows Azure"
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
ms.openlocfilehash: 1b653820927ef4c3bb8f384a7e752846a8be3da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-network-interface-for-azure-windows-vm"></a><span data-ttu-id="fdd54-103">Jak tooreset síťové rozhraní pro virtuální počítač s Windows Azure</span><span class="sxs-lookup"><span data-stu-id="fdd54-103">How tooreset network interface for Azure Windows VM</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="fdd54-104">Po zakázání hello výchozí síťové rozhraní (NIC) se nemůže připojit tooMicrosoft Azure Windows virtuální počítač (VM) nebo ručně nastaví statickou IP adresu pro síťový adaptér hello.</span><span class="sxs-lookup"><span data-stu-id="fdd54-104">You cannot connect tooMicrosoft Azure Windows Virtual Machine (VM) after you disable hello default Network Interface (NIC) or manually sets a static IP for hello NIC.</span></span> <span data-ttu-id="fdd54-105">Tento článek ukazuje, jak tooreset hello síťové rozhraní pro virtuální počítač Windows Azure, který bude vyřešen problém hello vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="fdd54-105">This article shows how tooreset hello network interface for Azure Windows VM, which will resolve hello remote connection issue.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a><span data-ttu-id="fdd54-106">Resetování síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="fdd54-106">Reset network interface</span></span>

### <a name="for-classic-vms"></a><span data-ttu-id="fdd54-107">Pro klasické virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="fdd54-107">For Classic VMs</span></span>

<span data-ttu-id="fdd54-108">tooreset síťové rozhraní, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fdd54-108">tooreset network interface, follow these steps:</span></span>

1.  <span data-ttu-id="fdd54-109">Přejděte toohello [portál Azure]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fdd54-109">Go toohello [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="fdd54-110">Vyberte **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-110">Select **Virtual Machines (Classic)**.</span></span>
3.  <span data-ttu-id="fdd54-111">Vyberte hello vliv virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fdd54-111">Select hello affected Virtual Machine.</span></span>
4.  <span data-ttu-id="fdd54-112">Vyberte **IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-112">Select **IP addresses**.</span></span>
5.  <span data-ttu-id="fdd54-113">Pokud hello **privátní IP přiřazení** není **statické**, změňte jej příliš**statické**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-113">If hello **Private IP assignment**  is not  **Static**, change it too**Static**.</span></span>
6.  <span data-ttu-id="fdd54-114">Změna hello **IP adresu** tooanother IP adresu, která je k dispozici v hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="fdd54-114">Change hello **IP address** tooanother IP address that is available in hello Subnet.</span></span>
7.  <span data-ttu-id="fdd54-115">Vyberte Uložit.</span><span class="sxs-lookup"><span data-stu-id="fdd54-115">Select Save.</span></span>
8.  <span data-ttu-id="fdd54-116">Hello virtuální počítač se restartuje tooinitialize hello nový síťový adaptér toohello systém.</span><span class="sxs-lookup"><span data-stu-id="fdd54-116">hello virtual machine will restart tooinitialize hello new NIC toohello system.</span></span>
9.  <span data-ttu-id="fdd54-117">Zkuste počítač tooyour tooRDP.</span><span class="sxs-lookup"><span data-stu-id="fdd54-117">Try tooRDP tooyour machine.</span></span> <span data-ttu-id="fdd54-118">V případě úspěchu, pokud chcete, můžete změnit hello privátní IP adresu zpět toohello původní.</span><span class="sxs-lookup"><span data-stu-id="fdd54-118">If successful, you can change hello Private IP address back toohello original if you would like.</span></span> <span data-ttu-id="fdd54-119">Jinak můžete ponechat ji.</span><span class="sxs-lookup"><span data-stu-id="fdd54-119">Otherwise, you can keep it.</span></span> 

### <a name="for-vms-deployed-in-resource-group-model"></a><span data-ttu-id="fdd54-120">Pro virtuální počítače nasazené v modelu skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fdd54-120">For VMs deployed in Resource group model</span></span>

1.  <span data-ttu-id="fdd54-121">Přejděte toohello [portál Azure]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fdd54-121">Go toohello [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="fdd54-122">Vyberte hello vliv virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fdd54-122">Select hello affected Virtual Machine.</span></span>
3.  <span data-ttu-id="fdd54-123">Vyberte **síťových rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-123">Select **Network Interfaces**.</span></span>
4.  <span data-ttu-id="fdd54-124">Vyberte síťové rozhraní přidružené k počítači hello</span><span class="sxs-lookup"><span data-stu-id="fdd54-124">Select hello Network Interface associated with your machine</span></span>
5.  <span data-ttu-id="fdd54-125">Vyberte **konfigurace protokolu IP**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-125">Select **IP configurations**.</span></span>
6.  <span data-ttu-id="fdd54-126">Vyberte hello IP.</span><span class="sxs-lookup"><span data-stu-id="fdd54-126">Select hello IP.</span></span> 
7.  <span data-ttu-id="fdd54-127">Pokud hello **privátní IP přiřazení** není **statické**, změňte jej příliš**statické**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-127">If hello **Private IP assignment**  is not  **Static**, change it too**Static**.</span></span>
8.  <span data-ttu-id="fdd54-128">Změna hello **IP adresu** tooanother IP adresu, která je k dispozici v hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="fdd54-128">Change hello **IP address** tooanother IP address that is available in hello Subnet.</span></span>
9. <span data-ttu-id="fdd54-129">Hello virtuální počítač se restartuje tooinitialize hello nový síťový adaptér toohello systém.</span><span class="sxs-lookup"><span data-stu-id="fdd54-129">hello virtual machine will restart tooinitialize hello new NIC toohello system.</span></span>
10. <span data-ttu-id="fdd54-130">Zkuste počítač tooyour tooRDP.</span><span class="sxs-lookup"><span data-stu-id="fdd54-130">Try tooRDP tooyour machine.</span></span> <span data-ttu-id="fdd54-131">V případě úspěchu, pokud chcete, můžete změnit hello privátní IP adresu zpět toohello původní.</span><span class="sxs-lookup"><span data-stu-id="fdd54-131">If successful, you can change hello Private IP address back toohello original if you would like.</span></span> <span data-ttu-id="fdd54-132">Jinak můžete ponechat ji.</span><span class="sxs-lookup"><span data-stu-id="fdd54-132">Otherwise, you can keep it.</span></span> 

## <a name="delete-hello-unavailable-nics"></a><span data-ttu-id="fdd54-133">Odstranit hello není k dispozici síťové karty</span><span class="sxs-lookup"><span data-stu-id="fdd54-133">Delete hello unavailable NICs</span></span>
<span data-ttu-id="fdd54-134">Poté, co je to možné vzdálené plochy toohello počítače, je nutné odstranit hello staré síťové adaptéry tooavoid hello potenciální problém:</span><span class="sxs-lookup"><span data-stu-id="fdd54-134">After you can remote desktop toohello machine, you must delete hello old NICs tooavoid hello potential problem:</span></span>

1.  <span data-ttu-id="fdd54-135">Otevřete Správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="fdd54-135">Open Device Manager.</span></span>
2.  <span data-ttu-id="fdd54-136">Vyberte **zobrazení** > **zobrazit skrytá zařízení**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-136">Select **View** > **Show hidden devices**.</span></span>
3.  <span data-ttu-id="fdd54-137">Vyberte **síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="fdd54-137">Select **Network Adapters**.</span></span> 
4.  <span data-ttu-id="fdd54-138">Zkontrolujte hello adaptérů s názvem jako "Microsoft Hyper-V síťový adaptér".</span><span class="sxs-lookup"><span data-stu-id="fdd54-138">Check for hello adapters named as "Microsoft Hyper-V Network Adapter".</span></span>
5.  <span data-ttu-id="fdd54-139">Může se zobrazit není k dispozici adaptér, který je zobrazena šedě. Klikněte pravým tlačítkem hello adaptér a potom vyberte možnost odinstalovat.</span><span class="sxs-lookup"><span data-stu-id="fdd54-139">You might see an unavailable adapter that is grayed out. Right-click hello adapter and then select Uninstall.</span></span>

    ![Obrázek Hello hello síťový adaptér](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > <span data-ttu-id="fdd54-141">Pouze odinstalujte hello není k dispozici adaptéry, které mají hello název "Microsoft Hyper-V síťový adaptér".</span><span class="sxs-lookup"><span data-stu-id="fdd54-141">Only uninstall hello unavailable adapters that have hello name "Microsoft Hyper-V Network Adapter".</span></span> <span data-ttu-id="fdd54-142">Pokud odinstalujete žádné hello ostatní skrytá adaptéry, může způsobit další problémy.</span><span class="sxs-lookup"><span data-stu-id="fdd54-142">If you uninstall any of hello other hidden adapters, it could cause additional issues.</span></span>
    >
    >

6.  <span data-ttu-id="fdd54-143">Nyní všechny adaptér není k dispozici je nutné vymazat z vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="fdd54-143">Now all unavailable adapter should be cleared out from your system.</span></span>