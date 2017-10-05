---
title: "Úvod do zachytáváním paketů v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled funkce zachytávání paketů sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4fdd007c2cfad7b42f26ab2cacfba06d95c8dad3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a><span data-ttu-id="1e1df-103">Úvod do proměnné paketu zachycení v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="1e1df-103">Introduction to variable packet capture in Azure Network Watcher</span></span>

<span data-ttu-id="1e1df-104">Zachycení dat ze sítě sledovacích procesů proměnné paketů umožňuje vytvářet relace zachytávání paketů sledovat provoz do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e1df-104">Network Watcher variable packet capture allows you to create packet capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="1e1df-105">Pomáhá zachytávání paketů při diagnostice sítě anomálií reaktivně a proactivity.</span><span class="sxs-lookup"><span data-stu-id="1e1df-105">Packet capture helps to diagnose network anomalies both reactively and proactivity.</span></span> <span data-ttu-id="1e1df-106">Jiné účely zahrnují shromažďování statistiku sítě, získá informace o síti vniknutí, k ladění komunikaci klienta se serverem a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="1e1df-106">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span>

<span data-ttu-id="1e1df-107">Zachytáváním paketů je rozšíření virtuálního počítače, který se spustil vzdáleně přes sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1e1df-107">Packet capture is a virtual machine extension that is remotely started through Network Watcher.</span></span> <span data-ttu-id="1e1df-108">Tato funkce snižuje zátěž spuštěných zachytáváním paketů ručně na požadovaný virtuální počítač, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="1e1df-108">This capability eases the burden of running a packet capture manually on the desired virtual machine, which saves valuable time.</span></span> <span data-ttu-id="1e1df-109">Zachytáváním paketů můžete spustit prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="1e1df-109">Packet capture can be triggered through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="1e1df-110">Je jeden příklad, jak můžete spustit zachytáváním paketů s výstrahami virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e1df-110">One example of how packet capture can be triggered is with Virtual Machine alerts.</span></span> <span data-ttu-id="1e1df-111">Filtry jsou k dispozici pro relaci zachytávání zajistit, že zaznamenávat provoz, který chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="1e1df-111">Filters are provided for the capture session to ensure you capture traffic you want to monitor.</span></span> <span data-ttu-id="1e1df-112">Filtry jsou založené na 5-n-tice (protokol, místní IP adresu, vzdálené IP adresy, místního portu a vzdáleného portu) informace.</span><span class="sxs-lookup"><span data-stu-id="1e1df-112">Filters are based on 5-tuple (protocol, local IP address, remote IP address, local port, and remote port) information.</span></span> <span data-ttu-id="1e1df-113">Zachycená data se ukládají v místním disku nebo objekt blob úložiště.</span><span class="sxs-lookup"><span data-stu-id="1e1df-113">The captured data is stored in the local disk or a storage blob.</span></span> <span data-ttu-id="1e1df-114">Existuje omezení 10 zachytávání relací paketů na oblast na předplatné.</span><span class="sxs-lookup"><span data-stu-id="1e1df-114">There is a limit of 10 packet capture sessions per region per subscription.</span></span> <span data-ttu-id="1e1df-115">Toto omezení se vztahuje pouze na relací a nebude použitelná pro soubory uložené paketu zaznamenat místně na virtuálním počítači nebo v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1e1df-115">This limit applies only to the sessions and does not apply to the saved packet capture files either locally on the VM or in a storage account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e1df-116">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="1e1df-116">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="1e1df-117">Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="1e1df-117">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

<span data-ttu-id="1e1df-118">Pokud chcete informace, které zaznamenáte na informace, které chcete zkrátit, jsou následující možnosti k dispozici pro relaci zachytávání paketů:</span><span class="sxs-lookup"><span data-stu-id="1e1df-118">To reduce the information you capture to only the information you want, the following options are available for a packet capture session:</span></span>

<span data-ttu-id="1e1df-119">**Zachycení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="1e1df-119">**Capture configuration**</span></span>

|<span data-ttu-id="1e1df-120">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1e1df-120">Property</span></span>|<span data-ttu-id="1e1df-121">Popis</span><span class="sxs-lookup"><span data-stu-id="1e1df-121">Description</span></span>|
|---|---|
|<span data-ttu-id="1e1df-122">**Maximální počet bajtů paketu (bajty)**</span><span class="sxs-lookup"><span data-stu-id="1e1df-122">**Maximum bytes per packet (bytes)**</span></span> | <span data-ttu-id="1e1df-123">Počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.</span><span class="sxs-lookup"><span data-stu-id="1e1df-123">The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="1e1df-124">Počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.</span><span class="sxs-lookup"><span data-stu-id="1e1df-124">The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="1e1df-125">Pokud budete potřebovat pouze hlavičce IPv4 – označuje 34 sem</span><span class="sxs-lookup"><span data-stu-id="1e1df-125">If you need only the IPv4 header – indicate 34 here</span></span> |
|<span data-ttu-id="1e1df-126">**Maximální počet bajtů za relace (bajty)**</span><span class="sxs-lookup"><span data-stu-id="1e1df-126">**Maximum bytes per session (bytes)**</span></span> | <span data-ttu-id="1e1df-127">Celkový počet bajtů v tom, že se zaznamená, hodnota v případě dosažení neukončí relace.</span><span class="sxs-lookup"><span data-stu-id="1e1df-127">Total number of bytes in that are captured, once the value is reached the session ends.</span></span>|
|<span data-ttu-id="1e1df-128">**Časový limit (sekundy)**</span><span class="sxs-lookup"><span data-stu-id="1e1df-128">**Time limit (seconds)**</span></span> | <span data-ttu-id="1e1df-129">Sady času omezení paketu zaznamenání relace.</span><span class="sxs-lookup"><span data-stu-id="1e1df-129">Sets a time constraint on the packet capture session.</span></span> <span data-ttu-id="1e1df-130">Výchozí hodnota je 18000 sekund nebo 5 hodin.</span><span class="sxs-lookup"><span data-stu-id="1e1df-130">The default value is 18000 seconds or 5 hours.</span></span>|

<span data-ttu-id="1e1df-131">**Filtrování (volitelné)**</span><span class="sxs-lookup"><span data-stu-id="1e1df-131">**Filtering (optional)**</span></span>

|<span data-ttu-id="1e1df-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1e1df-132">Property</span></span>|<span data-ttu-id="1e1df-133">Popis</span><span class="sxs-lookup"><span data-stu-id="1e1df-133">Description</span></span>|
|---|---|
|<span data-ttu-id="1e1df-134">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="1e1df-134">**Protocol**</span></span> | <span data-ttu-id="1e1df-135">Protokol pro filtrování pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="1e1df-135">The protocol to filter for the packet capture.</span></span> <span data-ttu-id="1e1df-136">Dostupné hodnoty jsou TCP, UDP a všechny.</span><span class="sxs-lookup"><span data-stu-id="1e1df-136">The available values are TCP, UDP, and All.</span></span>|
|<span data-ttu-id="1e1df-137">**Místní IP adresu**</span><span class="sxs-lookup"><span data-stu-id="1e1df-137">**Local IP address**</span></span> | <span data-ttu-id="1e1df-138">Tato hodnota filtruje zachytáváním paketů na pakety, kde místní IP adresa odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="1e1df-138">This value filters the packet capture to packets where the local IP address matches this filter value.</span></span>|
|<span data-ttu-id="1e1df-139">**Místního portu**</span><span class="sxs-lookup"><span data-stu-id="1e1df-139">**Local port**</span></span> | <span data-ttu-id="1e1df-140">Tato hodnota filtruje zachytáváním paketů na pakety, kde místního portu odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="1e1df-140">This value filters the packet capture to packets where the local port matches this filter value.</span></span>|
|<span data-ttu-id="1e1df-141">**Vzdálené IP adresy**</span><span class="sxs-lookup"><span data-stu-id="1e1df-141">**Remote IP address**</span></span> | <span data-ttu-id="1e1df-142">Tato hodnota filtruje zachytáváním paketů na pakety, kde vzdálené IP odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="1e1df-142">This value filters the packet capture to packets where the remote IP matches this filter value.</span></span>|
|<span data-ttu-id="1e1df-143">**Vzdálený port**</span><span class="sxs-lookup"><span data-stu-id="1e1df-143">**Remote port**</span></span> | <span data-ttu-id="1e1df-144">Tato hodnota filtruje zachytáváním paketů na pakety, kde vzdálený port odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="1e1df-144">This value filters the packet capture to packets where the remote port matches this filter value.</span></span>|

### <a name="next-steps"></a><span data-ttu-id="1e1df-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e1df-145">Next steps</span></span>

<span data-ttu-id="1e1df-146">Zjistěte, jak můžete spravovat zachycení paketů prostřednictvím portálu navštivte stránky [spravovat zachytáváním paketů na portálu Azure](network-watcher-packet-capture-manage-portal.md) nebo pomocí prostředí PowerShell, navštivte stránky [spravovat pomocí prostředí PowerShell zachytávání paketů](network-watcher-packet-capture-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1e1df-146">Learn how you can manage packet captures through the portal by visiting [Manage packet capture in the Azure portal](network-watcher-packet-capture-manage-portal.md) or with PowerShell by visiting [Manage Packet Capture with PowerShell](network-watcher-packet-capture-manage-powershell.md).</span></span>

<span data-ttu-id="1e1df-147">Naučte se vytvářet proaktivní paketu zachycení založeny na výstrahách virtuálního počítače, navštivte stránky [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="1e1df-147">Learn how to create proactive packet captures based on virtual machine alerts by visiting [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













