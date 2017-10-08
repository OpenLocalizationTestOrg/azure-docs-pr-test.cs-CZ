---
title: "aaaAccessing virtuálních počítačů Azure z Průzkumníka serveru | Microsoft Docs"
description: "Získáte přehled o tom, jak tooview vytvářet a spravovat virtuální počítače Azure (VM) v Průzkumníku serveru v sadě Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: f8326aed105a64ca558f766d712cc68701f82c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a><span data-ttu-id="8208f-103">Přístup k Azure virtuální počítače z Průzkumníka serveru</span><span class="sxs-lookup"><span data-stu-id="8208f-103">Accessing Azure Virtual Machines from Server Explorer</span></span>
<span data-ttu-id="8208f-104">Pomocí Průzkumníka serveru v sadě Visual Studio, můžete zobrazit informace o virtuální počítače hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="8208f-104">By using Server Explorer in Visual Studio, you can display information about your virtual machines hosted by Azure.</span></span>

## <a name="accessing-virtual-machines-in-server-explorer"></a><span data-ttu-id="8208f-105">Přístup k virtuálním počítačům v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="8208f-105">Accessing virtual machines in Server Explorer</span></span>
<span data-ttu-id="8208f-106">Pokud máte virtuální počítače hostované v Azure, můžete k dispozici v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="8208f-106">If you have virtual machines hosted by Azure, you can access them in Server Explorer.</span></span> <span data-ttu-id="8208f-107">Je nutné se přihlásit v tooyour předplatného Azure tooview mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="8208f-107">You must first sign in tooyour Azure subscription tooview your mobile services.</span></span> <span data-ttu-id="8208f-108">toosign, otevřete hello místní nabídku pro hello Azure uzlu v Průzkumníku serveru a zvolte **připojit tooMicrosoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="8208f-108">toosign in, open hello shortcut menu for hello Azure node in Server Explorer, and choose **Connect tooMicrosoft Azure**.</span></span>

### <a name="tooget-information-about-your-virtual-machines"></a><span data-ttu-id="8208f-109">tooget informace o virtuálních počítačích</span><span class="sxs-lookup"><span data-stu-id="8208f-109">tooget information about your virtual machines</span></span>
1. <span data-ttu-id="8208f-110">V Průzkumníku serveru vyberte virtuální počítač a zvolte klíče tooshow hello F4 jeho vlastnosti – okno.</span><span class="sxs-lookup"><span data-stu-id="8208f-110">In Server Explorer, choose a virtual machine, and then choose hello F4 key tooshow its properties window.</span></span>
   
    <span data-ttu-id="8208f-111">Hello následující tabulka ukazuje, jaké vlastnosti jsou k dispozici, ale jsou všechny jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8208f-111">hello following table shows what properties are available, but they are all read-only.</span></span> <span data-ttu-id="8208f-112">toochange, použijte hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="8208f-112">toochange them, use hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
   
   | <span data-ttu-id="8208f-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8208f-113">Property</span></span> | <span data-ttu-id="8208f-114">Popis</span><span class="sxs-lookup"><span data-stu-id="8208f-114">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="8208f-115">Název DNS</span><span class="sxs-lookup"><span data-stu-id="8208f-115">DNS Name</span></span> |<span data-ttu-id="8208f-116">Adresa URL Hello s hello internetové adresy hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8208f-116">hello URL with hello Internet address of hello virtual machine.</span></span> |
   | <span data-ttu-id="8208f-117">Prostředí</span><span class="sxs-lookup"><span data-stu-id="8208f-117">Environment</span></span> |<span data-ttu-id="8208f-118">Pro virtuální počítač hello hodnota této vlastnosti je vždy produkční.</span><span class="sxs-lookup"><span data-stu-id="8208f-118">For a virtual machine, hello value of this property is always Production.</span></span> |
   | <span data-ttu-id="8208f-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8208f-119">Name</span></span> |<span data-ttu-id="8208f-120">Název Hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8208f-120">hello name of hello virtual machine.</span></span> |
   | <span data-ttu-id="8208f-121">Velikost</span><span class="sxs-lookup"><span data-stu-id="8208f-121">Size</span></span> |<span data-ttu-id="8208f-122">velikost Hello hello virtuálního počítače, které odráží hello množství paměti a místa na disku, je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8208f-122">hello size of hello virtual machine, which reflects hello amount of memory and disk space that’s available.</span></span> <span data-ttu-id="8208f-123">Další informace najdete v tématu Postupy: Konfigurace velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8208f-123">For more information, see How To: Configure Virtual Machine Sizes.</span></span> |
   | <span data-ttu-id="8208f-124">Status</span><span class="sxs-lookup"><span data-stu-id="8208f-124">Status</span></span> |<span data-ttu-id="8208f-125">Hodnoty zahrnují spouštění, spuštěno, zastavit, zastaveno a načítání stavu.</span><span class="sxs-lookup"><span data-stu-id="8208f-125">Values include Starting, Started, Stopping, Stopped, and Retrieving Status.</span></span> <span data-ttu-id="8208f-126">Pokud se zobrazí stav načítání, aktuální stav hello neznámý.</span><span class="sxs-lookup"><span data-stu-id="8208f-126">If Retrieving Status appears, hello current status is unknown.</span></span> <span data-ttu-id="8208f-127">Hello hodnoty této vlastnosti se liší od hello hodnoty, které se používají na hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="8208f-127">hello values for this property differ from hello values that are used on hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> |
   | <span data-ttu-id="8208f-128">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="8208f-128">SubscriptionID</span></span> |<span data-ttu-id="8208f-129">ID předplatného Hello k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="8208f-129">hello subscription ID for your Azure account.</span></span> <span data-ttu-id="8208f-130">Tyto informace můžete zobrazit na hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) zobrazením hello vlastnosti pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="8208f-130">You can show this information on hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) by viewing hello properties for a subscription.</span></span> |
2. <span data-ttu-id="8208f-131">Zvolte do uzlu koncový bod a potom zobrazit hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="8208f-131">Choose an endpoint node, and then view hello **Properties** window.</span></span>
3. <span data-ttu-id="8208f-132">Hello následující tabulka popisuje dostupné vlastnosti hello koncových bodů, ale jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8208f-132">hello following table describes hello available properties of endpoints, but they are read-only.</span></span> <span data-ttu-id="8208f-133">tooadd nebo upravit koncových bodů hello pro virtuální počítač, použijte hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="8208f-133">tooadd or edit hello endpoints for a virtual machine, use hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> 
   
   | <span data-ttu-id="8208f-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8208f-134">Property</span></span> | <span data-ttu-id="8208f-135">Popis</span><span class="sxs-lookup"><span data-stu-id="8208f-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="8208f-136">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8208f-136">Name</span></span> |<span data-ttu-id="8208f-137">Identifikátor pro koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="8208f-137">An identifier for hello endpoint.</span></span> |
   | <span data-ttu-id="8208f-138">Privátní Port</span><span class="sxs-lookup"><span data-stu-id="8208f-138">Private Port</span></span> |<span data-ttu-id="8208f-139">Hello portu pro síťový přístup interní tooyour aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8208f-139">hello port for network access internal tooyour application.</span></span> |
   | <span data-ttu-id="8208f-140">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="8208f-140">Protocol</span></span> |<span data-ttu-id="8208f-141">Hello protokol, který hello přenosové vrstvy pro tento koncový bod používá, TCP nebo UDP.</span><span class="sxs-lookup"><span data-stu-id="8208f-141">hello protocol that hello transport layer for this endpoint uses, either TCP or UDP.</span></span> |
   | <span data-ttu-id="8208f-142">Veřejný port</span><span class="sxs-lookup"><span data-stu-id="8208f-142">Public Port</span></span> |<span data-ttu-id="8208f-143">Hello port, který se používá pro aplikaci tooyour veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="8208f-143">hello port that’s used for public access tooyour application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8208f-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8208f-144">Next steps</span></span>
<span data-ttu-id="8208f-145">toolearn Další informace o použití Azure rolí v sadě Visual Studio, najdete v části [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="8208f-145">toolearn more about using Azure roles in Visual Studio, see [Using Remote Desktop with Azure Roles](vs-azure-tools-remote-desktop-roles.md).</span></span>

