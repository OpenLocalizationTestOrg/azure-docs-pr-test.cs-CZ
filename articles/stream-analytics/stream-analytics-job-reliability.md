---
title: "Zabránilo přerušení služeb s úlohy Azure Stream Analytics | Microsoft Docs"
description: "Pokyny k provedení Stream Analytics úlohy upgradu odolný."
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3ac65c93ecb47e93e963dd9869a7af70f73b19c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a><span data-ttu-id="c0a03-103">Zajistit spolehlivost úlohy Stream Analytics během aktualizace služby</span><span class="sxs-lookup"><span data-stu-id="c0a03-103">Guarantee Stream Analytics job reliability during service updates</span></span>

<span data-ttu-id="c0a03-104">Součástí je plně spravovaná služba je hello schopností toointroduce nové služby funkce a vylepšení rychlé tempem.</span><span class="sxs-lookup"><span data-stu-id="c0a03-104">Part of being a fully managed service is hello capability toointroduce new service functionality and improvements at a rapid pace.</span></span> <span data-ttu-id="c0a03-105">Stream Analytics v důsledku toho může mít aktualizaci služby nasazení na základě týdenní (nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="c0a03-105">As a result, Stream Analytics can have a service update deploy on a weekly (or more frequent) basis.</span></span> <span data-ttu-id="c0a03-106">Bez ohledu na to, kolik testování se provádí stále existuje riziko, že existující, spuštěné úlohy je možné ukončit kvůli toohello zavedení chyby.</span><span class="sxs-lookup"><span data-stu-id="c0a03-106">No matter how much testing is done there is still a risk that an existing, running job may break due toohello introduction of a bug.</span></span> <span data-ttu-id="c0a03-107">Pro zákazníky, kteří spustí kritické úlohy streamování zpracování těchto rizik potřebovat toobe vyhnout.</span><span class="sxs-lookup"><span data-stu-id="c0a03-107">For customers who run critical streaming processing jobs these risks need toobe avoided.</span></span> <span data-ttu-id="c0a03-108">Zákazníci mechanismus můžete použít tooreduce toto riziko je Azure  **[spárované oblast](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modelu.</span><span class="sxs-lookup"><span data-stu-id="c0a03-108">A mechanism customers can use tooreduce this risk is Azure’s **[paired region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model.</span></span> 

## <a name="how-do-azure-paired-regions-address-this-concern"></a><span data-ttu-id="c0a03-109">Jak oblastí Azure spárované tuto situaci řešit?</span><span class="sxs-lookup"><span data-stu-id="c0a03-109">How do Azure paired regions address this concern?</span></span>

<span data-ttu-id="c0a03-110">Stream Analytics zaručuje, že úlohy v oblasti spárované se aktualizují odděleně.</span><span class="sxs-lookup"><span data-stu-id="c0a03-110">Stream Analytics guarantees jobs in paired regions are updated in separate batches.</span></span> <span data-ttu-id="c0a03-111">Mezi hello aktualizace potenciální narušující tooidentify chyb a opravit, je výsledkem dostatečný čas mezera.</span><span class="sxs-lookup"><span data-stu-id="c0a03-111">As a result there is a sufficient time gap between hello updates tooidentify potential breaking bugs and remediate them.</span></span>

<span data-ttu-id="c0a03-112">_S výjimkou hello střed_ (jehož spárované oblast – Jih, Indie, nemá přítomnosti Stream Analytics), nasazení hello aktualizaci tooStream Analytics se nevyskytuje v hello stejný čas v sadě spárované oblasti.</span><span class="sxs-lookup"><span data-stu-id="c0a03-112">_With hello exception of Central India_ (whose paired region, South India, does not have Stream Analytics presence), hello deployment of an update tooStream Analytics would not occur at hello same time in a set of paired regions.</span></span> <span data-ttu-id="c0a03-113">Nasazení v několika oblastech **v hello stejnou skupinu** může dojít, **v hello současně**.</span><span class="sxs-lookup"><span data-stu-id="c0a03-113">Deployments in multiple regions **in hello same group** may occur **at hello same time**.</span></span>

<span data-ttu-id="c0a03-114">článek Hello na  **[dostupnosti a oblastí, spárované](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  má hello nejnovější informace, na kterém jsou spárovat oblasti.</span><span class="sxs-lookup"><span data-stu-id="c0a03-114">hello article on **[availability and paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** has hello most up-to-date information on which regions are paired.</span></span>

<span data-ttu-id="c0a03-115">Zákazníci jsou doporučené toodeploy identické úlohy tooboth spárovat oblasti.</span><span class="sxs-lookup"><span data-stu-id="c0a03-115">Customers are advised toodeploy identical jobs tooboth paired regions.</span></span> <span data-ttu-id="c0a03-116">Kromě toho tooStream Analytics interní možnosti zákazníkům monitorování jsou také doporučujeme úlohy hello toomonitor jako **obě** provozní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c0a03-116">In addition tooStream Analytics internal monitoring capabilities, customers are also advised toomonitor hello jobs as if **both** are production jobs.</span></span> <span data-ttu-id="c0a03-117">Pokud zalomení identifikovaných toobe výsledku hello aktualizace služby Stream Analytics, eskalovat správně a převzít žádný výstup podřízeného příjemci toohello pořádku úlohy.</span><span class="sxs-lookup"><span data-stu-id="c0a03-117">If a break is identified toobe a result of hello Stream Analytics service update, escalate appropriately and fail over any downstream consumers toohello healthy job output.</span></span> <span data-ttu-id="c0a03-118">Eskalace toosupport bude zabránění spárované oblast hello ovlivnění hello nové nasazení a zachovat integritu hello hello spárovat úloh.</span><span class="sxs-lookup"><span data-stu-id="c0a03-118">Escalation toosupport will prevent hello paired region from being affected by hello new deployment and maintain hello integrity of hello paired jobs.</span></span>
