---
title: "Omezení a výchozích hodnot"
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
ms.openlocfilehash: db6b1c196cb468f41c7a7ce34758de346b522abb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-limits-and-defaults"></a><span data-ttu-id="06ee8-103">Omezení a výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="06ee8-103">Scheduler Limits and Defaults</span></span>
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a><span data-ttu-id="06ee8-104">Scheduler kvóty, omezení, výchozí hodnoty a omezení</span><span class="sxs-lookup"><span data-stu-id="06ee8-104">Scheduler Quotas, Limits, Defaults, and Throttles</span></span>
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a><span data-ttu-id="06ee8-105">Hlavička x-ms-request-id</span><span class="sxs-lookup"><span data-stu-id="06ee8-105">The x-ms-request-id Header</span></span>
<span data-ttu-id="06ee8-106">Každý požadavek směřovaný na službu Plánovač vrátí hlavičky odpovědi s názvem**x-ms-request-id**.</span><span class="sxs-lookup"><span data-stu-id="06ee8-106">Every request made against the Scheduler service returns a response header named**x-ms-request-id**.</span></span> <span data-ttu-id="06ee8-107">Tuto hlavičku obsahuje neprůhledné hodnoty, která jednoznačně identifikuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="06ee8-107">This header contains an opaque value that uniquely identifies the request.</span></span>

<span data-ttu-id="06ee8-108">Pokud požadavek konzistentně selhává a ověříte, že je správně formulovali požadavek, můžete používat tuto hodnotu ohlaste chybu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="06ee8-108">If a request is consistently failing and you have verified that the request is properly formulated, you may use this value to report the error to Microsoft.</span></span> <span data-ttu-id="06ee8-109">V sestavě zahrnují hodnotu x-ms-request-id, Přibližná doba, po který byl požadavek, identifikátor odběru, kolekci úloh nebo úlohy a typ operace, které se pokusili žádosti.</span><span class="sxs-lookup"><span data-stu-id="06ee8-109">In your report, include the value of x-ms-request-id, the approximate time that the request was made, the identifier of the subscription, job collection, and/or job, and the type of operation that the request attempted.</span></span>

## <a name="see-also"></a><span data-ttu-id="06ee8-110">Viz také</span><span class="sxs-lookup"><span data-stu-id="06ee8-110">See Also</span></span>
 [<span data-ttu-id="06ee8-111">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="06ee8-111">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="06ee8-112">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="06ee8-112">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="06ee8-113">Úvod do používání Scheduleru na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="06ee8-113">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="06ee8-114">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="06ee8-114">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="06ee8-115">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="06ee8-115">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="06ee8-116">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="06ee8-116">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="06ee8-117">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="06ee8-117">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="06ee8-118">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="06ee8-118">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

