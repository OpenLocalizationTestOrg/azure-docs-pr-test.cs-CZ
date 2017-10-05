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
ms.openlocfilehash: 8dc19e1b37082c87d2990ad910d1af786f8b9280
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a><span data-ttu-id="c13f1-103">Zajistit spolehlivost úlohy Stream Analytics během aktualizace služby</span><span class="sxs-lookup"><span data-stu-id="c13f1-103">Guarantee Stream Analytics job reliability during service updates</span></span>

<span data-ttu-id="c13f1-104">Součástí je plně spravovaná služba, je schopnost zavádět nové funkce služby a vylepšení rychlé tempem.</span><span class="sxs-lookup"><span data-stu-id="c13f1-104">Part of being a fully managed service is the capability to introduce new service functionality and improvements at a rapid pace.</span></span> <span data-ttu-id="c13f1-105">Stream Analytics v důsledku toho může mít aktualizaci služby nasazení na základě týdenní (nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="c13f1-105">As a result, Stream Analytics can have a service update deploy on a weekly (or more frequent) basis.</span></span> <span data-ttu-id="c13f1-106">Bez ohledu na to, kolik testování se provádí stále existuje riziko, že existující, spuštěné úlohy je možné ukončit z důvodu zavedení chyby.</span><span class="sxs-lookup"><span data-stu-id="c13f1-106">No matter how much testing is done there is still a risk that an existing, running job may break due to the introduction of a bug.</span></span> <span data-ttu-id="c13f1-107">Pro zákazníky, kteří spustí kritické úlohy streamování zpracování těchto rizik muset se vyhnout.</span><span class="sxs-lookup"><span data-stu-id="c13f1-107">For customers who run critical streaming processing jobs these risks need to be avoided.</span></span> <span data-ttu-id="c13f1-108">Azure je mechanismus zákazníci mohou používat pro toto riziko snížit  **[spárované oblast](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modelu.</span><span class="sxs-lookup"><span data-stu-id="c13f1-108">A mechanism customers can use to reduce this risk is Azure’s **[paired region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model.</span></span> 

## <a name="how-do-azure-paired-regions-address-this-concern"></a><span data-ttu-id="c13f1-109">Jak oblastí Azure spárované tuto situaci řešit?</span><span class="sxs-lookup"><span data-stu-id="c13f1-109">How do Azure paired regions address this concern?</span></span>

<span data-ttu-id="c13f1-110">Stream Analytics zaručuje, že úlohy v oblasti spárované se aktualizují odděleně.</span><span class="sxs-lookup"><span data-stu-id="c13f1-110">Stream Analytics guarantees jobs in paired regions are updated in separate batches.</span></span> <span data-ttu-id="c13f1-111">Mezi aktualizace opravit, a identifikovat potenciální chyby ukončování řádků je v důsledku dostatečná časová mezera.</span><span class="sxs-lookup"><span data-stu-id="c13f1-111">As a result there is a sufficient time gap between the updates to identify potential breaking bugs and remediate them.</span></span>

<span data-ttu-id="c13f1-112">_S výjimkou střed_ (jehož spárované oblast – Jih, Indie, nemá přítomnosti Stream Analytics), nasazení aktualizace do služby Stream Analytics se nevyskytuje ve stejnou dobu v sadě spárované oblasti.</span><span class="sxs-lookup"><span data-stu-id="c13f1-112">_With the exception of Central India_ (whose paired region, South India, does not have Stream Analytics presence), the deployment of an update to Stream Analytics would not occur at the same time in a set of paired regions.</span></span> <span data-ttu-id="c13f1-113">Nasazení v několika oblastech **ve stejné skupině** může dojít, **ve stejnou dobu**.</span><span class="sxs-lookup"><span data-stu-id="c13f1-113">Deployments in multiple regions **in the same group** may occur **at the same time**.</span></span>

<span data-ttu-id="c13f1-114">V článku na  **[dostupnosti a oblastí, spárované](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  má nejaktuálnější informace, na kterém jsou spárovat oblasti.</span><span class="sxs-lookup"><span data-stu-id="c13f1-114">The article on **[availability and paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** has the most up-to-date information on which regions are paired.</span></span>

<span data-ttu-id="c13f1-115">Zákazníci by měli nasadit stejné úlohy do obou spárované oblasti.</span><span class="sxs-lookup"><span data-stu-id="c13f1-115">Customers are advised to deploy identical jobs to both paired regions.</span></span> <span data-ttu-id="c13f1-116">Kromě Stream Analytics interní možnosti monitorování, Zákazníci by měli také ke sledování úloh jako **i** provozní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c13f1-116">In addition to Stream Analytics internal monitoring capabilities, customers are also advised to monitor the jobs as if **both** are production jobs.</span></span> <span data-ttu-id="c13f1-117">Pokud zalomení identifikuje jako výsledek aktualizace služby Stream Analytics, eskalovat správně a převzít všechny podřízené příjemcům výstup úlohy v pořádku.</span><span class="sxs-lookup"><span data-stu-id="c13f1-117">If a break is identified to be a result of the Stream Analytics service update, escalate appropriately and fail over any downstream consumers to the healthy job output.</span></span> <span data-ttu-id="c13f1-118">Eskalace pro podporu budou zabránění oblasti spárované ovlivnění nové nasazení a zachovat integritu spárované úloh.</span><span class="sxs-lookup"><span data-stu-id="c13f1-118">Escalation to support will prevent the paired region from being affected by the new deployment and maintain the integrity of the paired jobs.</span></span>
