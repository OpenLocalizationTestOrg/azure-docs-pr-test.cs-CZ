---
title: "Jak zvýšit souběžnost webové služby Azure Machine Learning | Microsoft Docs"
description: "Zjistěte, jak zvýšit souběžnost webové služby Azure Machine Learning přidáním další koncové body."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "Azure machine learningu, webové služby, operationalization, škálování, koncový bod, souběžnosti"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: 013354515d841003c912ac0338690dd975a79ef7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a><span data-ttu-id="c8319-104">Škálování webové služby Azure Machine Learning přidáním další koncové body</span><span class="sxs-lookup"><span data-stu-id="c8319-104">Scaling an Azure Machine Learning web service by adding additional endpoints</span></span>
> [!NOTE]
> <span data-ttu-id="c8319-105">Toto téma popisuje techniky pro **Classic** Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8319-105">This topic describes techniques applicable to a **Classic** Machine Learning Web service.</span></span> 
> 
> 

<span data-ttu-id="c8319-106">Ve výchozím nastavení každý publikované webové služby je nakonfigurován pro podporu 20 souběžnými požadavky a může být až 200 souběžných požadavků.</span><span class="sxs-lookup"><span data-stu-id="c8319-106">By default, each published Web service is configured to support 20 concurrent requests and can be as high as 200 concurrent requests.</span></span> <span data-ttu-id="c8319-107">Zatímco portál Azure classic poskytuje způsob, jak nastavit tuto hodnotu, Azure Machine Learning automaticky optimalizuje nastavení pro poskytovat nejlepší výkon pro webové služby a portálu hodnota je ignorována.</span><span class="sxs-lookup"><span data-stu-id="c8319-107">While the Azure classic portal provides a way to set this value, Azure Machine Learning automatically optimizes the setting to provide the best performance for your web service and the portal value is ignored.</span></span> 

<span data-ttu-id="c8319-108">Pokud bude podporovat plán pro volání rozhraní API se zatížením vyšší než maximální počet souběžných volání hodnota 200, měli byste vytvořit několik koncových bodů na stejné webové službě.</span><span class="sxs-lookup"><span data-stu-id="c8319-108">If you plan to call the API with a higher load than a Max Concurrent Calls value of 200 will support, you should create multiple endpoints on the same Web service.</span></span> <span data-ttu-id="c8319-109">Potom můžete náhodně distribuovat zatížení napříč všemi z nich.</span><span class="sxs-lookup"><span data-stu-id="c8319-109">You can then randomly distribute your load across all of them.</span></span>

<span data-ttu-id="c8319-110">Škálování webové služby je běžné úlohy.</span><span class="sxs-lookup"><span data-stu-id="c8319-110">The scaling of a Web service is a common task.</span></span> <span data-ttu-id="c8319-111">Některé z důvodů škálování jsou pro podporu více než 200 souběžných požadavků, zvýšit dostupnost prostřednictvím několik koncových bodů nebo poskytovat samostatný koncové body pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="c8319-111">Some reasons to scale are to support more than 200 concurrent requests, increase availability through multiple endpoints, or provide separate endpoints for the web service.</span></span> <span data-ttu-id="c8319-112">Měřítko můžete zvýšit tak, že přidáte další koncové body pro službu Web prostřednictvím [portál Azure classic](https://manage.windowsazure.com/) nebo [webovou službu Azure Machine Learning](https://services.azureml.net/) portálu.</span><span class="sxs-lookup"><span data-stu-id="c8319-112">You can increase the scale by adding additional endpoints for the same Web service through [Azure classic portal](https://manage.windowsazure.com/) or the [Azure Machine Learning Web Service](https://services.azureml.net/) portal.</span></span>

<span data-ttu-id="c8319-113">Další informace o přidání nové koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="c8319-113">For more information on adding new endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span>

<span data-ttu-id="c8319-114">Mějte na paměti, který používá souběžnosti vysoký počet může být škodlivé, pokud nejsou volání rozhraní API s odpovídajícím způsobem vysokou míru.</span><span class="sxs-lookup"><span data-stu-id="c8319-114">Keep in mind that using a high concurrency count can be detrimental if you're not calling the API with a correspondingly high rate.</span></span> <span data-ttu-id="c8319-115">Můžete se setkat ojediněle vypršení časových limitů nebo špičky v latenci, když vložíte relativně nízký zatížení na rozhraní API nakonfigurovaný pro vysokého zatížení.</span><span class="sxs-lookup"><span data-stu-id="c8319-115">You might see sporadic timeouts and/or spikes in the latency if you put a relatively low load on an API configured for high load.</span></span>

<span data-ttu-id="c8319-116">V situacích, kde je žádoucí s nízkou latencí jsou obvykle používány synchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8319-116">The synchronous APIs are typically used in situations where a low latency is desired.</span></span> <span data-ttu-id="c8319-117">Latence zde znamená doba potřebná pro rozhraní API pro jeden požadavek na dokončení a nemá účet pro všechny zpoždění sítě.</span><span class="sxs-lookup"><span data-stu-id="c8319-117">Latency here implies the time it takes for the API to complete one request, and doesn't account for any network delays.</span></span> <span data-ttu-id="c8319-118">Řekněme, že máte rozhraní API s latencí 50 ms.</span><span class="sxs-lookup"><span data-stu-id="c8319-118">Let's say you have an API with a 50-ms latency.</span></span> <span data-ttu-id="c8319-119">Chcete-li plně využívat s omezení úrovní vysoce dostupné kapacity a maximální počet souběžných volání = 20, je třeba volat toto rozhraní API 20 * 1000 / 50 = 400 times za sekundu.</span><span class="sxs-lookup"><span data-stu-id="c8319-119">To fully consume the available capacity with throttle level High and Max Concurrent Calls = 20, you need to call this API 20 * 1000 / 50 = 400 times per second.</span></span> <span data-ttu-id="c8319-120">Tato další rozšíření, maximální počet souběžných volání 200 vám umožní volat rozhraní API 4000 časy za sekundu, za předpokladu, že latence 50-ms.</span><span class="sxs-lookup"><span data-stu-id="c8319-120">Extending this further, a Max Concurrent Calls of 200 allows you to call the API 4000 times per second, assuming a 50-ms latency.</span></span>

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
