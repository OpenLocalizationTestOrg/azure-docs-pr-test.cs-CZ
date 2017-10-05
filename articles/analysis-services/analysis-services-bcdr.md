---
title: "Azure vysokou dostupnost služby Analysis Services | Microsoft Docs"
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
ms.openlocfilehash: 84b4c59bac1feeb8611b3a8d783d093ba073e532
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="analysis-services-high-availability"></a><span data-ttu-id="15460-103">Vysoká dostupnost Analysis Services</span><span class="sxs-lookup"><span data-stu-id="15460-103">Analysis Services high availability</span></span>
<span data-ttu-id="15460-104">Tento článek popisuje, zajištění vysoké dostupnosti pro servery Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="15460-104">This article describes assuring high availability for Azure Analysis Services servers.</span></span> 


## <a name="assuring-high-availability-during-a-service-disruption"></a><span data-ttu-id="15460-105">Zajištění vysoké dostupnosti během přerušení služby</span><span class="sxs-lookup"><span data-stu-id="15460-105">Assuring high availability during a service disruption</span></span>
<span data-ttu-id="15460-106">Když je taková situace vzácná, může mít datové centrum Azure k výpadku.</span><span class="sxs-lookup"><span data-stu-id="15460-106">While rare, an Azure data center can have an outage.</span></span> <span data-ttu-id="15460-107">Pokud dojde k výpadku, budou přerušení obchodní, který může trvat několik minut nebo může trvat hodiny.</span><span class="sxs-lookup"><span data-stu-id="15460-107">When an outage occurs, it causes a business disruption that might last a few minutes or might last for hours.</span></span> <span data-ttu-id="15460-108">Vysoká dostupnost nejčastěji dosáhnout s nadbytečností serveru.</span><span class="sxs-lookup"><span data-stu-id="15460-108">High availability is most often achieved with server redundancy.</span></span> <span data-ttu-id="15460-109">S Azure Analysis Services můžete dosáhnout vytvořením další, sekundární servery v oblastech jeden nebo více redundance.</span><span class="sxs-lookup"><span data-stu-id="15460-109">With Azure Analysis Services, you can achieve redundancy by creating additional, secondary servers in one or more regions.</span></span> <span data-ttu-id="15460-110">Při vytváření redundantních serverů, aby zajistil, data a metadata na těchto serverech je v synchronizaci se serverem v oblasti, který přešel do stavu offline, můžete:</span><span class="sxs-lookup"><span data-stu-id="15460-110">When creating redundant servers, to assure the data and metadata on those servers is in-sync with the server in a region that has gone offline, you can:</span></span>

* <span data-ttu-id="15460-111">Modely nasazení na redundantní servery v jiných oblastech.</span><span class="sxs-lookup"><span data-stu-id="15460-111">Deploy models to redundant servers in other regions.</span></span> <span data-ttu-id="15460-112">Tato metoda vyžaduje zpracování dat na primární server i redundantní servery v paralelní, zajištění všechny servery jsou v synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="15460-112">This method requires processing data on both your primary server and redundant servers in-parallel, assuring all servers are in-sync.</span></span>

* <span data-ttu-id="15460-113">Zálohování databází z primárního serveru a obnovení na redundantní serverech.</span><span class="sxs-lookup"><span data-stu-id="15460-113">Back up databases from your primary server and restore on redundant servers.</span></span> <span data-ttu-id="15460-114">Například můžete automatizovat noční zálohování do úložiště Azure a obnovení na jiné servery redundantní v jiných oblastech.</span><span class="sxs-lookup"><span data-stu-id="15460-114">For example, you can automate nightly backups to Azure storage, and restore to other redundant servers in other regions.</span></span> 

<span data-ttu-id="15460-115">V obou případech Pokud primární server dojde k výpadku, musíte změnit připojovací řetězce v generování sestav klientům připojit se k serveru v jiném datovém centru místní.</span><span class="sxs-lookup"><span data-stu-id="15460-115">In either case, if your primary server experiences an outage, you must change the connection strings in reporting clients to connect to the server in a different regional datacenter.</span></span> <span data-ttu-id="15460-116">Tato změna považovat za poslední možnost a dochází pouze v případě výpadku center závažné místní data.</span><span class="sxs-lookup"><span data-stu-id="15460-116">This change should be considered a last resort and only if a catastrophic regional data center outage occurs.</span></span> <span data-ttu-id="15460-117">Je pravděpodobnější, výpadku datacentra hostování primární server by dostane zpět online, než může aktualizovat připojení na všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="15460-117">It's more likely a data center outage hosting your primary server would come back online before you could update connections on all clients.</span></span> 



## <a name="related-information"></a><span data-ttu-id="15460-118">Související informace</span><span class="sxs-lookup"><span data-stu-id="15460-118">Related information</span></span>
<span data-ttu-id="15460-119">[Zálohování a obnovení](analysis-services-backup.md) </span><span class="sxs-lookup"><span data-stu-id="15460-119">[Backup and restore](analysis-services-backup.md) </span></span>  
[<span data-ttu-id="15460-120">Správa služby Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="15460-120">Manage Azure Analysis Services</span></span>](analysis-services-manage.md) 

