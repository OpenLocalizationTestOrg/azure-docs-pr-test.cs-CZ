---
title: "zachycení tooPacket aaaIntroduction v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled funkce zachytávání paketů sledovací proces sítě hello"
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
ms.openlocfilehash: 2ce01b391b9c1a1c19aa29c8620628c55586df03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toovariable-packet-capture-in-azure-network-watcher"></a><span data-ttu-id="f3e74-103">Úvod toovariable zachytáváním paketů v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="f3e74-103">Introduction toovariable packet capture in Azure Network Watcher</span></span>

<span data-ttu-id="f3e74-104">Zachycení dat ze sítě sledovacích procesů proměnné paketů umožňuje zaznamenat toocreate paketů relace tootrack tooand na provoz z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f3e74-104">Network Watcher variable packet capture allows you toocreate packet capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="f3e74-105">Zachytáváním paketů pomáhá sítě anomálií toodiagnose obou reaktivně a proactivity.</span><span class="sxs-lookup"><span data-stu-id="f3e74-105">Packet capture helps toodiagnose network anomalies both reactively and proactivity.</span></span> <span data-ttu-id="f3e74-106">Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="f3e74-106">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span>

<span data-ttu-id="f3e74-107">Zachytáváním paketů je rozšíření virtuálního počítače, který se spustil vzdáleně přes sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f3e74-107">Packet capture is a virtual machine extension that is remotely started through Network Watcher.</span></span> <span data-ttu-id="f3e74-108">Tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně na hello potřeby virtuální počítač, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="f3e74-108">This capability eases hello burden of running a packet capture manually on hello desired virtual machine, which saves valuable time.</span></span> <span data-ttu-id="f3e74-109">Zachytáváním paketů můžete spustit prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="f3e74-109">Packet capture can be triggered through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f3e74-110">Je jeden příklad, jak můžete spustit zachytáváním paketů s výstrahami virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f3e74-110">One example of how packet capture can be triggered is with Virtual Machine alerts.</span></span> <span data-ttu-id="f3e74-111">Filtry jsou k dispozici pro hello zaznamenání relace tooensure můžete zaznamenávat provoz, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="f3e74-111">Filters are provided for hello capture session tooensure you capture traffic you want toomonitor.</span></span> <span data-ttu-id="f3e74-112">Filtry jsou založené na 5-n-tice (protokol, místní IP adresu, vzdálené IP adresy, místního portu a vzdáleného portu) informace.</span><span class="sxs-lookup"><span data-stu-id="f3e74-112">Filters are based on 5-tuple (protocol, local IP address, remote IP address, local port, and remote port) information.</span></span> <span data-ttu-id="f3e74-113">Hello zachytit data se ukládají v místním disku hello nebo objekt blob úložiště.</span><span class="sxs-lookup"><span data-stu-id="f3e74-113">hello captured data is stored in hello local disk or a storage blob.</span></span> <span data-ttu-id="f3e74-114">Existuje omezení 10 zachytávání relací paketů na oblast na předplatné.</span><span class="sxs-lookup"><span data-stu-id="f3e74-114">There is a limit of 10 packet capture sessions per region per subscription.</span></span> <span data-ttu-id="f3e74-115">Toto omezení platí pouze toohello relace a doporučení se netýká toohello uložené soubory zachytávání paketů místně na hello virtuálních počítačů nebo v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f3e74-115">This limit applies only toohello sessions and does not apply toohello saved packet capture files either locally on hello VM or in a storage account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3e74-116">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="f3e74-116">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="f3e74-117">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="f3e74-117">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

<span data-ttu-id="f3e74-118">informace o hello tooreduce zaznamenáte tooonly hello informace, které chcete, hello jsou dostupné následující možnosti pro relaci zachytávání paketů:</span><span class="sxs-lookup"><span data-stu-id="f3e74-118">tooreduce hello information you capture tooonly hello information you want, hello following options are available for a packet capture session:</span></span>

<span data-ttu-id="f3e74-119">**Zachycení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="f3e74-119">**Capture configuration**</span></span>

|<span data-ttu-id="f3e74-120">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f3e74-120">Property</span></span>|<span data-ttu-id="f3e74-121">Popis</span><span class="sxs-lookup"><span data-stu-id="f3e74-121">Description</span></span>|
|---|---|
|<span data-ttu-id="f3e74-122">**Maximální počet bajtů paketu (bajty)**</span><span class="sxs-lookup"><span data-stu-id="f3e74-122">**Maximum bytes per packet (bytes)**</span></span> | <span data-ttu-id="f3e74-123">Hello počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.</span><span class="sxs-lookup"><span data-stu-id="f3e74-123">hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="f3e74-124">Hello počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.</span><span class="sxs-lookup"><span data-stu-id="f3e74-124">hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="f3e74-125">Pokud budete potřebovat pouze hlavičce IPv4 hello – označuje 34 sem</span><span class="sxs-lookup"><span data-stu-id="f3e74-125">If you need only hello IPv4 header – indicate 34 here</span></span> |
|<span data-ttu-id="f3e74-126">**Maximální počet bajtů za relace (bajty)**</span><span class="sxs-lookup"><span data-stu-id="f3e74-126">**Maximum bytes per session (bytes)**</span></span> | <span data-ttu-id="f3e74-127">Celkový počet bajtů v tom, že jsou zachyceny po dosažení neukončí relace hello hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f3e74-127">Total number of bytes in that are captured, once hello value is reached hello session ends.</span></span>|
|<span data-ttu-id="f3e74-128">**Časový limit (sekundy)**</span><span class="sxs-lookup"><span data-stu-id="f3e74-128">**Time limit (seconds)**</span></span> | <span data-ttu-id="f3e74-129">Sady času omezení hello paketu zaznamenání relace.</span><span class="sxs-lookup"><span data-stu-id="f3e74-129">Sets a time constraint on hello packet capture session.</span></span> <span data-ttu-id="f3e74-130">Hello výchozí hodnota je 18000 sekund nebo 5 hodin.</span><span class="sxs-lookup"><span data-stu-id="f3e74-130">hello default value is 18000 seconds or 5 hours.</span></span>|

<span data-ttu-id="f3e74-131">**Filtrování (volitelné)**</span><span class="sxs-lookup"><span data-stu-id="f3e74-131">**Filtering (optional)**</span></span>

|<span data-ttu-id="f3e74-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f3e74-132">Property</span></span>|<span data-ttu-id="f3e74-133">Popis</span><span class="sxs-lookup"><span data-stu-id="f3e74-133">Description</span></span>|
|---|---|
|<span data-ttu-id="f3e74-134">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="f3e74-134">**Protocol**</span></span> | <span data-ttu-id="f3e74-135">zaznamenat Hello protokol toofilter pro paket hello.</span><span class="sxs-lookup"><span data-stu-id="f3e74-135">hello protocol toofilter for hello packet capture.</span></span> <span data-ttu-id="f3e74-136">Hello dostupné jsou hodnoty TCP, UDP a všechny.</span><span class="sxs-lookup"><span data-stu-id="f3e74-136">hello available values are TCP, UDP, and All.</span></span>|
|<span data-ttu-id="f3e74-137">**Místní IP adresu**</span><span class="sxs-lookup"><span data-stu-id="f3e74-137">**Local IP address**</span></span> | <span data-ttu-id="f3e74-138">Tato hodnota filtruje toopackets zachytávání paketů hello, kde hello místní IP adresa odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="f3e74-138">This value filters hello packet capture toopackets where hello local IP address matches this filter value.</span></span>|
|<span data-ttu-id="f3e74-139">**Místního portu**</span><span class="sxs-lookup"><span data-stu-id="f3e74-139">**Local port**</span></span> | <span data-ttu-id="f3e74-140">Tato hodnota filtry hello paketu zachytit toopackets kde místního portu hello odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="f3e74-140">This value filters hello packet capture toopackets where hello local port matches this filter value.</span></span>|
|<span data-ttu-id="f3e74-141">**Vzdálené IP adresy**</span><span class="sxs-lookup"><span data-stu-id="f3e74-141">**Remote IP address**</span></span> | <span data-ttu-id="f3e74-142">Tato hodnota filtry hello paketu zachytit toopackets kde hello vzdálené IP odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="f3e74-142">This value filters hello packet capture toopackets where hello remote IP matches this filter value.</span></span>|
|<span data-ttu-id="f3e74-143">**Vzdálený port**</span><span class="sxs-lookup"><span data-stu-id="f3e74-143">**Remote port**</span></span> | <span data-ttu-id="f3e74-144">Tato hodnota filtry hello paketu zachytit toopackets kde vzdálených portů hello odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="f3e74-144">This value filters hello packet capture toopackets where hello remote port matches this filter value.</span></span>|

### <a name="next-steps"></a><span data-ttu-id="f3e74-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3e74-145">Next steps</span></span>

<span data-ttu-id="f3e74-146">Zjistěte, jak můžete spravovat zachycení paketů přes portál hello navštivte stránky [spravovat zachytáváním paketů v hello portál Azure](network-watcher-packet-capture-manage-portal.md) nebo pomocí prostředí PowerShell, navštivte stránky [spravovat pomocí prostředí PowerShell zachytávání paketů](network-watcher-packet-capture-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f3e74-146">Learn how you can manage packet captures through hello portal by visiting [Manage packet capture in hello Azure portal](network-watcher-packet-capture-manage-portal.md) or with PowerShell by visiting [Manage Packet Capture with PowerShell](network-watcher-packet-capture-manage-powershell.md).</span></span>

<span data-ttu-id="f3e74-147">Zjistěte, jak proaktivní paketu toocreate zaznamená založeny na výstrahách virtuálního počítače, navštivte stránky [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="f3e74-147">Learn how toocreate proactive packet captures based on virtual machine alerts by visiting [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













