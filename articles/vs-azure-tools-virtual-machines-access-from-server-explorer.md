---
title: "Přístup k Azure virtuální počítače z Průzkumníka serveru | Microsoft Docs"
description: "Get přehled o tom, jak zobrazit vytvářet a spravovat virtuální počítače Azure (VM) v Průzkumníku serveru v sadě Visual Studio."
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
ms.openlocfilehash: fcbb00cc2f00691e25ea84333e8c418b08210a67
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a><span data-ttu-id="cd51f-103">Přístup k Azure virtuální počítače z Průzkumníka serveru</span><span class="sxs-lookup"><span data-stu-id="cd51f-103">Accessing Azure Virtual Machines from Server Explorer</span></span>
<span data-ttu-id="cd51f-104">Pomocí Průzkumníka serveru v sadě Visual Studio, můžete zobrazit informace o virtuální počítače hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="cd51f-104">By using Server Explorer in Visual Studio, you can display information about your virtual machines hosted by Azure.</span></span>

## <a name="accessing-virtual-machines-in-server-explorer"></a><span data-ttu-id="cd51f-105">Přístup k virtuálním počítačům v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="cd51f-105">Accessing virtual machines in Server Explorer</span></span>
<span data-ttu-id="cd51f-106">Pokud máte virtuální počítače hostované v Azure, můžete k dispozici v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="cd51f-106">If you have virtual machines hosted by Azure, you can access them in Server Explorer.</span></span> <span data-ttu-id="cd51f-107">Musíte nejprve se přihlásit k vašemu předplatnému Azure Chcete-li zobrazit mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="cd51f-107">You must first sign in to your Azure subscription to view your mobile services.</span></span> <span data-ttu-id="cd51f-108">Pokud chcete přihlásit, otevřete místní nabídky pro Azure uzel v Průzkumníku serveru a zvolte **připojit k Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="cd51f-108">To sign in, open the shortcut menu for the Azure node in Server Explorer, and choose **Connect to Microsoft Azure**.</span></span>

### <a name="to-get-information-about-your-virtual-machines"></a><span data-ttu-id="cd51f-109">Chcete-li získat informace o virtuálních počítačích</span><span class="sxs-lookup"><span data-stu-id="cd51f-109">To get information about your virtual machines</span></span>
1. <span data-ttu-id="cd51f-110">V Průzkumníku serveru vyberte virtuální počítač a potom vyberte klávesy F4 zobrazíte její vlastnosti – okno.</span><span class="sxs-lookup"><span data-stu-id="cd51f-110">In Server Explorer, choose a virtual machine, and then choose the F4 key to show its properties window.</span></span>
   
    <span data-ttu-id="cd51f-111">Následující tabulka uvádí, které vlastnosti jsou k dispozici, ale jsou všechny jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="cd51f-111">The following table shows what properties are available, but they are all read-only.</span></span> <span data-ttu-id="cd51f-112">Chcete-li změnit, použijte [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="cd51f-112">To change them, use the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
   
   | <span data-ttu-id="cd51f-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="cd51f-113">Property</span></span> | <span data-ttu-id="cd51f-114">Popis</span><span class="sxs-lookup"><span data-stu-id="cd51f-114">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="cd51f-115">Název DNS</span><span class="sxs-lookup"><span data-stu-id="cd51f-115">DNS Name</span></span> |<span data-ttu-id="cd51f-116">Adresa URL s Internetovou adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cd51f-116">The URL with the Internet address of the virtual machine.</span></span> |
   | <span data-ttu-id="cd51f-117">Prostředí</span><span class="sxs-lookup"><span data-stu-id="cd51f-117">Environment</span></span> |<span data-ttu-id="cd51f-118">Pro virtuální počítač hodnota této vlastnosti je vždy produkční.</span><span class="sxs-lookup"><span data-stu-id="cd51f-118">For a virtual machine, the value of this property is always Production.</span></span> |
   | <span data-ttu-id="cd51f-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="cd51f-119">Name</span></span> |<span data-ttu-id="cd51f-120">Název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cd51f-120">The name of the virtual machine.</span></span> |
   | <span data-ttu-id="cd51f-121">Velikost</span><span class="sxs-lookup"><span data-stu-id="cd51f-121">Size</span></span> |<span data-ttu-id="cd51f-122">Velikost virtuálního počítače, které odráží množství paměti a místa na disku, je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cd51f-122">The size of the virtual machine, which reflects the amount of memory and disk space that’s available.</span></span> <span data-ttu-id="cd51f-123">Další informace najdete v tématu Postupy: Konfigurace velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cd51f-123">For more information, see How To: Configure Virtual Machine Sizes.</span></span> |
   | <span data-ttu-id="cd51f-124">Status</span><span class="sxs-lookup"><span data-stu-id="cd51f-124">Status</span></span> |<span data-ttu-id="cd51f-125">Hodnoty zahrnují spouštění, spuštěno, zastavit, zastaveno a načítání stavu.</span><span class="sxs-lookup"><span data-stu-id="cd51f-125">Values include Starting, Started, Stopping, Stopped, and Retrieving Status.</span></span> <span data-ttu-id="cd51f-126">Pokud se zobrazí stav načítání, aktuální stav neznámý.</span><span class="sxs-lookup"><span data-stu-id="cd51f-126">If Retrieving Status appears, the current status is unknown.</span></span> <span data-ttu-id="cd51f-127">Hodnoty pro tuto vlastnost se liší od hodnoty, které jsou použity na [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="cd51f-127">The values for this property differ from the values that are used on the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> |
   | <span data-ttu-id="cd51f-128">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="cd51f-128">SubscriptionID</span></span> |<span data-ttu-id="cd51f-129">ID předplatného pro váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="cd51f-129">The subscription ID for your Azure account.</span></span> <span data-ttu-id="cd51f-130">Tyto informace můžete zobrazit na [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) zobrazením vlastností pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="cd51f-130">You can show this information on the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) by viewing the properties for a subscription.</span></span> |
2. <span data-ttu-id="cd51f-131">Zvolte do uzlu koncového bodu a následně zobrazit **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="cd51f-131">Choose an endpoint node, and then view the **Properties** window.</span></span>
3. <span data-ttu-id="cd51f-132">Následující tabulka popisuje dostupné vlastnosti koncových bodů, ale jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="cd51f-132">The following table describes the available properties of endpoints, but they are read-only.</span></span> <span data-ttu-id="cd51f-133">Chcete-li přidat nebo upravit koncové body pro virtuální počítač, použijte [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="cd51f-133">To add or edit the endpoints for a virtual machine, use the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> 
   
   | <span data-ttu-id="cd51f-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="cd51f-134">Property</span></span> | <span data-ttu-id="cd51f-135">Popis</span><span class="sxs-lookup"><span data-stu-id="cd51f-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="cd51f-136">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="cd51f-136">Name</span></span> |<span data-ttu-id="cd51f-137">Identifikátor pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="cd51f-137">An identifier for the endpoint.</span></span> |
   | <span data-ttu-id="cd51f-138">Privátní Port</span><span class="sxs-lookup"><span data-stu-id="cd51f-138">Private Port</span></span> |<span data-ttu-id="cd51f-139">Port pro přístup k síti interní do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd51f-139">The port for network access internal to your application.</span></span> |
   | <span data-ttu-id="cd51f-140">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="cd51f-140">Protocol</span></span> |<span data-ttu-id="cd51f-141">Protokol, který používá přenosové vrstvy pro tento koncový bod, TCP nebo UDP.</span><span class="sxs-lookup"><span data-stu-id="cd51f-141">The protocol that the transport layer for this endpoint uses, either TCP or UDP.</span></span> |
   | <span data-ttu-id="cd51f-142">Veřejný port</span><span class="sxs-lookup"><span data-stu-id="cd51f-142">Public Port</span></span> |<span data-ttu-id="cd51f-143">Port, který se používá pro veřejný přístup k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cd51f-143">The port that’s used for public access to your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cd51f-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd51f-144">Next steps</span></span>
<span data-ttu-id="cd51f-145">Další informace o používání Azure role v sadě Visual Studio najdete v tématu [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="cd51f-145">To learn more about using Azure roles in Visual Studio, see [Using Remote Desktop with Azure Roles](vs-azure-tools-remote-desktop-roles.md).</span></span>

