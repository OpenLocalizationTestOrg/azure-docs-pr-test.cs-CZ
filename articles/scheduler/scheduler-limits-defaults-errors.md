---
title: "Omezení aaaScheduler a výchozích hodnot"
description: "Omezení a výchozích hodnot"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 6fe0600d3ce3249d5aab1b877369b175316b5437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-limits-and-defaults"></a><span data-ttu-id="be056-103">Omezení a výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="be056-103">Scheduler Limits and Defaults</span></span>
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a><span data-ttu-id="be056-104">Scheduler kvóty, omezení, výchozí hodnoty a omezení</span><span class="sxs-lookup"><span data-stu-id="be056-104">Scheduler Quotas, Limits, Defaults, and Throttles</span></span>
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="hello-x-ms-request-id-header"></a><span data-ttu-id="be056-105">Hello x-ms-request-id hlavičky</span><span class="sxs-lookup"><span data-stu-id="be056-105">hello x-ms-request-id Header</span></span>
<span data-ttu-id="be056-106">Každý požadavek směřovaný na hello služba Plánovač vrátí hlavičky odpovědi s názvem**x-ms-request-id**. Tuto hlavičku obsahuje neprůhledné hodnoty, která jednoznačně identifikuje hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="be056-106">Every request made against hello Scheduler service returns a response header named**x-ms-request-id**. This header contains an opaque value that uniquely identifies hello request.</span></span>

<span data-ttu-id="be056-107">Pokud je požadavek konzistentně selhání a mít ověříte, že je správně formulovali hello požadavku, můžete použít tuto hodnotu tooreport hello chyba tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="be056-107">If a request is consistently failing and you have verified that hello request is properly formulated, you may use this value tooreport hello error tooMicrosoft.</span></span> <span data-ttu-id="be056-108">V sestavě, zahrnují hello hodnotu x-ms-request-id hello Přibližná doba této hello požadavek, hello identifikátor hello předplatné, kolekci úloh nebo úlohy a hello typ operace, která hello pokus o žádost.</span><span class="sxs-lookup"><span data-stu-id="be056-108">In your report, include hello value of x-ms-request-id, hello approximate time that hello request was made, hello identifier of hello subscription, job collection, and/or job, and hello type of operation that hello request attempted.</span></span>

## <a name="see-also"></a><span data-ttu-id="be056-109">Viz také</span><span class="sxs-lookup"><span data-stu-id="be056-109">See Also</span></span>
 [<span data-ttu-id="be056-110">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="be056-110">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="be056-111">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="be056-111">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="be056-112">Začněte používat plánovače v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="be056-112">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="be056-113">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="be056-113">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="be056-114">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="be056-114">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="be056-115">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="be056-115">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="be056-116">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="be056-116">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="be056-117">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="be056-117">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

