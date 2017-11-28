---
title: "aaaHow tooincrease souběžnosti webové služby Azure Machine Learning | Microsoft Docs"
description: "Zjistěte, jak tooincrease souběžnosti ze Azure Machine Learning webovou službu tak, že přidáte další koncové body."
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
ms.openlocfilehash: e2ad16ec766820a64f36c31232f6a33a79196af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a><span data-ttu-id="108dd-104">Škálování webové služby Azure Machine Learning přidáním další koncové body</span><span class="sxs-lookup"><span data-stu-id="108dd-104">Scaling an Azure Machine Learning web service by adding additional endpoints</span></span>
> [!NOTE]
> <span data-ttu-id="108dd-105">Toto téma popisuje techniky použít tooa **Classic** Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="108dd-105">This topic describes techniques applicable tooa **Classic** Machine Learning Web service.</span></span> 
> 
> 

<span data-ttu-id="108dd-106">Ve výchozím nastavení každý publikované webové služby je nakonfigurované toosupport 20 souběžnými požadavky a může být až 200 souběžných požadavků.</span><span class="sxs-lookup"><span data-stu-id="108dd-106">By default, each published Web service is configured toosupport 20 concurrent requests and can be as high as 200 concurrent requests.</span></span> <span data-ttu-id="108dd-107">Zatímco hello klasický portál Azure poskytuje tooset způsob, jak tuto hodnotu, Azure Machine Learning automaticky optimalizuje hello nastavení tooprovide hello nejlepší výkon pro webové služby a portálu hodnota hello je ignorována.</span><span class="sxs-lookup"><span data-stu-id="108dd-107">While hello Azure classic portal provides a way tooset this value, Azure Machine Learning automatically optimizes hello setting tooprovide hello best performance for your web service and hello portal value is ignored.</span></span> 

<span data-ttu-id="108dd-108">Pokud máte v plánu toocall hello rozhraní API pomocí zatížení vyšší než maximální počet souběžných volání hodnota 200 bude podporovat, měli byste vytvořit několik koncových bodů na hello stejné webové služby.</span><span class="sxs-lookup"><span data-stu-id="108dd-108">If you plan toocall hello API with a higher load than a Max Concurrent Calls value of 200 will support, you should create multiple endpoints on hello same Web service.</span></span> <span data-ttu-id="108dd-109">Potom můžete náhodně distribuovat zatížení napříč všemi z nich.</span><span class="sxs-lookup"><span data-stu-id="108dd-109">You can then randomly distribute your load across all of them.</span></span>

<span data-ttu-id="108dd-110">Hello škálování webové služby je běžné úlohy.</span><span class="sxs-lookup"><span data-stu-id="108dd-110">hello scaling of a Web service is a common task.</span></span> <span data-ttu-id="108dd-111">Některé z důvodů tooscale jsou toosupport více než 200 souběžných požadavků, zvýšit dostupnost prostřednictvím několik koncových bodů nebo zadejte samostatné koncové body pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="108dd-111">Some reasons tooscale are toosupport more than 200 concurrent requests, increase availability through multiple endpoints, or provide separate endpoints for hello web service.</span></span> <span data-ttu-id="108dd-112">Škálování hello můžete zvýšit tak, že přidáte další koncové body pro hello stejné webové služby prostřednictvím [portál Azure classic](https://manage.windowsazure.com/) nebo hello [webovou službu Azure Machine Learning](https://services.azureml.net/) portálu.</span><span class="sxs-lookup"><span data-stu-id="108dd-112">You can increase hello scale by adding additional endpoints for hello same Web service through [Azure classic portal](https://manage.windowsazure.com/) or hello [Azure Machine Learning Web Service](https://services.azureml.net/) portal.</span></span>

<span data-ttu-id="108dd-113">Další informace o přidání nové koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="108dd-113">For more information on adding new endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span>

<span data-ttu-id="108dd-114">Mějte na paměti, který používá souběžnosti vysoký počet může být škodlivé, pokud nejsou volání hello rozhraní API s odpovídajícím způsobem vysokou míru.</span><span class="sxs-lookup"><span data-stu-id="108dd-114">Keep in mind that using a high concurrency count can be detrimental if you're not calling hello API with a correspondingly high rate.</span></span> <span data-ttu-id="108dd-115">Můžete se setkat ojediněle vypršení časových limitů nebo špičky hello latence když vložíte relativně nízký zatížení na rozhraní API nakonfigurovaný pro vysokého zatížení.</span><span class="sxs-lookup"><span data-stu-id="108dd-115">You might see sporadic timeouts and/or spikes in hello latency if you put a relatively low load on an API configured for high load.</span></span>

<span data-ttu-id="108dd-116">Hello synchronní rozhraní API se obvykle používá v situacích, kde je žádoucí s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="108dd-116">hello synchronous APIs are typically used in situations where a low latency is desired.</span></span> <span data-ttu-id="108dd-117">Latence zde znamená hello doba potřebná pro jeden požadavek toocomplete hello rozhraní API a nemá účet pro všechny zpoždění sítě.</span><span class="sxs-lookup"><span data-stu-id="108dd-117">Latency here implies hello time it takes for hello API toocomplete one request, and doesn't account for any network delays.</span></span> <span data-ttu-id="108dd-118">Řekněme, že máte rozhraní API s latencí 50 ms.</span><span class="sxs-lookup"><span data-stu-id="108dd-118">Let's say you have an API with a 50-ms latency.</span></span> <span data-ttu-id="108dd-119">toofully využívat hello s omezení úrovní vysoce dostupné kapacity a maximální počet souběžných volání = 20, je nutné toocall toto rozhraní API 20 * 1000 / 50 = 400 times za sekundu.</span><span class="sxs-lookup"><span data-stu-id="108dd-119">toofully consume hello available capacity with throttle level High and Max Concurrent Calls = 20, you need toocall this API 20 * 1000 / 50 = 400 times per second.</span></span> <span data-ttu-id="108dd-120">Tato další rozšíření, maximální počet souběžných volání 200 umožňuje vám toocall hello API 4000 časy za sekundu, za předpokladu, že latence 50 ms.</span><span class="sxs-lookup"><span data-stu-id="108dd-120">Extending this further, a Max Concurrent Calls of 200 allows you toocall hello API 4000 times per second, assuming a 50-ms latency.</span></span>

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
