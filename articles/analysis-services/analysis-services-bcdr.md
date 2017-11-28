---
title: "aaaAzure vysokou dostupnost služby Analysis Services | Microsoft Docs"
description: "Zajišťuje vysokou dostupnost služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 6e09536c5bd7dc7f88f9b662e1a9f42d2b8c969b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analysis-services-high-availability"></a><span data-ttu-id="b386d-103">Vysoká dostupnost Analysis Services</span><span class="sxs-lookup"><span data-stu-id="b386d-103">Analysis Services high availability</span></span>
<span data-ttu-id="b386d-104">Tento článek popisuje, zajištění vysoké dostupnosti pro servery Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="b386d-104">This article describes assuring high availability for Azure Analysis Services servers.</span></span> 


## <a name="assuring-high-availability-during-a-service-disruption"></a><span data-ttu-id="b386d-105">Zajištění vysoké dostupnosti během přerušení služby</span><span class="sxs-lookup"><span data-stu-id="b386d-105">Assuring high availability during a service disruption</span></span>
<span data-ttu-id="b386d-106">Když je taková situace vzácná, může mít datové centrum Azure k výpadku.</span><span class="sxs-lookup"><span data-stu-id="b386d-106">While rare, an Azure data center can have an outage.</span></span> <span data-ttu-id="b386d-107">Pokud dojde k výpadku, budou přerušení obchodní, který může trvat několik minut nebo může trvat hodiny.</span><span class="sxs-lookup"><span data-stu-id="b386d-107">When an outage occurs, it causes a business disruption that might last a few minutes or might last for hours.</span></span> <span data-ttu-id="b386d-108">Vysoká dostupnost nejčastěji dosáhnout s nadbytečností serveru.</span><span class="sxs-lookup"><span data-stu-id="b386d-108">High availability is most often achieved with server redundancy.</span></span> <span data-ttu-id="b386d-109">S Azure Analysis Services můžete dosáhnout vytvořením další, sekundární servery v oblastech jeden nebo více redundance.</span><span class="sxs-lookup"><span data-stu-id="b386d-109">With Azure Analysis Services, you can achieve redundancy by creating additional, secondary servers in one or more regions.</span></span> <span data-ttu-id="b386d-110">Při vytváření redundantních serverů, tooassure hello data a metadata na těchto serverech je v synchronizaci se serverem hello v oblasti, který přešel do stavu offline, můžete:</span><span class="sxs-lookup"><span data-stu-id="b386d-110">When creating redundant servers, tooassure hello data and metadata on those servers is in-sync with hello server in a region that has gone offline, you can:</span></span>

* <span data-ttu-id="b386d-111">Nasaďte servery tooredundant modely v jiných oblastech.</span><span class="sxs-lookup"><span data-stu-id="b386d-111">Deploy models tooredundant servers in other regions.</span></span> <span data-ttu-id="b386d-112">Tato metoda vyžaduje zpracování dat na primární server i redundantní servery v paralelní, zajištění všechny servery jsou v synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="b386d-112">This method requires processing data on both your primary server and redundant servers in-parallel, assuring all servers are in-sync.</span></span>

* <span data-ttu-id="b386d-113">Zálohování databází z primárního serveru a obnovení na redundantní serverech.</span><span class="sxs-lookup"><span data-stu-id="b386d-113">Back up databases from your primary server and restore on redundant servers.</span></span> <span data-ttu-id="b386d-114">Například můžete automatizovat úložiště tooAzure noční zálohování a obnovení tooother redundantní serverů v jiných oblastech.</span><span class="sxs-lookup"><span data-stu-id="b386d-114">For example, you can automate nightly backups tooAzure storage, and restore tooother redundant servers in other regions.</span></span> 

<span data-ttu-id="b386d-115">V obou případech Pokud primární server dojde k výpadku, musíte změnit hello připojovací řetězce v reporting server toohello tooconnect klienti v jiném datovém centru místní.</span><span class="sxs-lookup"><span data-stu-id="b386d-115">In either case, if your primary server experiences an outage, you must change hello connection strings in reporting clients tooconnect toohello server in a different regional datacenter.</span></span> <span data-ttu-id="b386d-116">Tato změna považovat za poslední možnost a dochází pouze v případě výpadku center závažné místní data.</span><span class="sxs-lookup"><span data-stu-id="b386d-116">This change should be considered a last resort and only if a catastrophic regional data center outage occurs.</span></span> <span data-ttu-id="b386d-117">Je pravděpodobnější, výpadku datacentra hostování primární server by dostane zpět online, než může aktualizovat připojení na všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="b386d-117">It's more likely a data center outage hosting your primary server would come back online before you could update connections on all clients.</span></span> 



## <a name="related-information"></a><span data-ttu-id="b386d-118">Související informace</span><span class="sxs-lookup"><span data-stu-id="b386d-118">Related information</span></span>
<span data-ttu-id="b386d-119">[Zálohování a obnovení](analysis-services-backup.md) </span><span class="sxs-lookup"><span data-stu-id="b386d-119">[Backup and restore](analysis-services-backup.md) </span></span>  
[<span data-ttu-id="b386d-120">Správa služby Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="b386d-120">Manage Azure Analysis Services</span></span>](analysis-services-manage.md) 

