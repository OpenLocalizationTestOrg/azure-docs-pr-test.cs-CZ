---
title: "Krok 6: Přístup k hello Machine Learning webové služby | Microsoft Docs"
description: "Krok 6 hello vývoj prediktivního řešení návod: přístup k Azure Machine Learning webové služby aktivní."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 211de0294092c6a6b5e6eb608d5d3b88107674c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-6-access-hello-azure-machine-learning-web-service"></a><span data-ttu-id="0661e-103">Návod krok 6: Přístup k webové službě Azure Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="0661e-103">Walkthrough Step 6: Access hello Azure Machine Learning web service</span></span>

<span data-ttu-id="0661e-104">Toto je poslední krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="0661e-104">This is hello last step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="0661e-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0661e-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="0661e-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="0661e-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="0661e-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="0661e-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="0661e-108">Natrénování a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="0661e-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="0661e-109">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="0661e-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. <span data-ttu-id="0661e-110">**Přístup k webové službě hello**</span><span class="sxs-lookup"><span data-stu-id="0661e-110">**Access hello Web service**</span></span>

- - -
<span data-ttu-id="0661e-111">V předchozím kroku hello v tomto návodu jsme nasadili webová služba, která používá našeho úvěrové riziko předpovědi modelu.</span><span class="sxs-lookup"><span data-stu-id="0661e-111">In hello previous step in this walkthrough we deployed a web service that uses our credit risk prediction model.</span></span> <span data-ttu-id="0661e-112">Uživatelé nyní jsou možné toosend data tooit a příjem výsledků.</span><span class="sxs-lookup"><span data-stu-id="0661e-112">Now users are able toosend data tooit and receive results.</span></span> 

<span data-ttu-id="0661e-113">Hello webové služby je Azure webové služby, který může přijímat a vrátit data pomocí rozhraní API REST v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="0661e-113">hello Web service is an Azure web service that can receive and return data using REST APIs in one of two ways:</span></span>  

* <span data-ttu-id="0661e-114">**Požadavek a odpověď** – hello uživatel odešle jednu nebo více řádky dat toohello platební služby s použitím protokolu HTTP a hello služby odpoví jednu nebo více sad výsledků.</span><span class="sxs-lookup"><span data-stu-id="0661e-114">**Request/Response** - hello user sends one or more rows of credit data toohello service by using an HTTP protocol, and hello service responds with one or more sets of results.</span></span>
* <span data-ttu-id="0661e-115">**Spuštění dávky** – hello uživatel uloží jednu nebo více řádky platební dat v Azure blob a poté odešle služby toohello umístění objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="0661e-115">**Batch Execution** - hello user stores one or more rows of credit data in an Azure blob and then sends hello blob location toohello service.</span></span> <span data-ttu-id="0661e-116">skóre Hello služby, které všechny hello řádky dat v hello vstupního objektu blob, úložiště hello výsledkem jiného objektu blob a vrátí hello adresu URL tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0661e-116">hello service scores all hello rows of data in hello input blob, stores hello results in another blob, and returns hello URL of that container.</span></span>  

<span data-ttu-id="0661e-117">Hello tooaccess nejrychlejší a nejjednodušší způsob je webová služba Classic prostřednictvím hello [webové aplikace Azure ML požadavků a odpovědí Service](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) nebo [Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span><span class="sxs-lookup"><span data-stu-id="0661e-117">hello quickest and easiest way tooaccess a Classic web service is through hello [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) or [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span></span>

<span data-ttu-id="0661e-118">Tyto šablony webové aplikace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data a co vrátí webové služby.</span><span class="sxs-lookup"><span data-stu-id="0661e-118">These web app templates can build a custom web app that knows your web service's input data and what it will return.</span></span> <span data-ttu-id="0661e-119">Stačí toodo je poskytnout přístup tooyour webové služby a data a šablony hello hello rest.</span><span class="sxs-lookup"><span data-stu-id="0661e-119">All you need toodo is provide access tooyour web service and data, and hello template does hello rest.</span></span>

<span data-ttu-id="0661e-120">Další informace o používání hello webové aplikace šablony najdete v tématu [využívat Azure Machine Learning webové služby pomocí šablony webové aplikace](machine-learning-consume-web-service-with-web-app-template.md).</span><span class="sxs-lookup"><span data-stu-id="0661e-120">For more information on using hello web app templates, see [Consume an Azure Machine Learning Web service with a web app template](machine-learning-consume-web-service-with-web-app-template.md).</span></span>

<span data-ttu-id="0661e-121">Také lze vytvářet vlastní aplikaci tooaccess hello webové službě pomocí počáteční kód stanovené v R, C# a Python programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="0661e-121">You can also develop a custom application tooaccess hello web service using starter code provided for you in R, C#, and Python programming languages.</span></span>

<span data-ttu-id="0661e-122">Najdete kompletní informace v [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="0661e-122">You can find complete details in [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

