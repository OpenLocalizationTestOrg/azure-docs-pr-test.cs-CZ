---
title: "Krok 6: Přístup ke službě Machine Learning Web | Microsoft Docs"
description: "Krok 6 vývoj prediktivního řešení návod: přístup k Azure Machine Learning webové služby aktivní."
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
ms.openlocfilehash: d309f6c4749a80c81859b693a2bd5927e8fe0e54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a><span data-ttu-id="98eaf-103">Krok 6 průvodce: Přístup k webové službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="98eaf-103">Walkthrough Step 6: Access the Azure Machine Learning web service</span></span>

<span data-ttu-id="98eaf-104">Toto je poslední krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="98eaf-104">This is the last step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="98eaf-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="98eaf-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="98eaf-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="98eaf-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="98eaf-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="98eaf-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="98eaf-108">Natrénování a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="98eaf-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="98eaf-109">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="98eaf-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. <span data-ttu-id="98eaf-110">**Přístup k webové službě**</span><span class="sxs-lookup"><span data-stu-id="98eaf-110">**Access the Web service**</span></span>

- - -
<span data-ttu-id="98eaf-111">V předchozím kroku v tomto návodu jsme nasadili webová služba, která používá našeho úvěrové riziko předpovědi modelu.</span><span class="sxs-lookup"><span data-stu-id="98eaf-111">In the previous step in this walkthrough we deployed a web service that uses our credit risk prediction model.</span></span> <span data-ttu-id="98eaf-112">Uživatelé jsou nyní může odesílat data a přijímat výsledky.</span><span class="sxs-lookup"><span data-stu-id="98eaf-112">Now users are able to send data to it and receive results.</span></span> 

<span data-ttu-id="98eaf-113">Webová služba je Azure webové služby, který může přijímat a vrátit data pomocí rozhraní API REST v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="98eaf-113">The Web service is an Azure web service that can receive and return data using REST APIs in one of two ways:</span></span>  

* <span data-ttu-id="98eaf-114">**Požadavek a odpověď** – uživatel odesílá jeden nebo více řádků platební dat ve službě s použitím protokolu HTTP a službu odpoví jednu nebo více sad výsledků.</span><span class="sxs-lookup"><span data-stu-id="98eaf-114">**Request/Response** - The user sends one or more rows of credit data to the service by using an HTTP protocol, and the service responds with one or more sets of results.</span></span>
* <span data-ttu-id="98eaf-115">**Spuštění dávky** – uživatel uloží jednu nebo více řádky platební dat v Azure blob a poté odešle umístění objektu blob ve službě.</span><span class="sxs-lookup"><span data-stu-id="98eaf-115">**Batch Execution** - The user stores one or more rows of credit data in an Azure blob and then sends the blob location to the service.</span></span> <span data-ttu-id="98eaf-116">Tato služba skóre všechny řádky dat v vstupního objektu blob, uloží výsledky v jiném objektu blob a vrátí adresu URL tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="98eaf-116">The service scores all the rows of data in the input blob, stores the results in another blob, and returns the URL of that container.</span></span>  

<span data-ttu-id="98eaf-117">Nejrychlejší a nejsnazší způsob pro přístup k webové službě Classic je prostřednictvím [webové aplikace Azure ML požadavků a odpovědí Service](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) nebo [Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span><span class="sxs-lookup"><span data-stu-id="98eaf-117">The quickest and easiest way to access a Classic web service is through the [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) or [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span></span>

<span data-ttu-id="98eaf-118">Tyto šablony webové aplikace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data a co vrátí webové služby.</span><span class="sxs-lookup"><span data-stu-id="98eaf-118">These web app templates can build a custom web app that knows your web service's input data and what it will return.</span></span> <span data-ttu-id="98eaf-119">Jediné, co musíte udělat je poskytnout přístup k webové služby a data a šablona nemá rest.</span><span class="sxs-lookup"><span data-stu-id="98eaf-119">All you need to do is provide access to your web service and data, and the template does the rest.</span></span>

<span data-ttu-id="98eaf-120">Další informace o použití šablony webové aplikace, najdete v části [využívat Azure Machine Learning webové služby pomocí šablony webové aplikace](machine-learning-consume-web-service-with-web-app-template.md).</span><span class="sxs-lookup"><span data-stu-id="98eaf-120">For more information on using the web app templates, see [Consume an Azure Machine Learning Web service with a web app template](machine-learning-consume-web-service-with-web-app-template.md).</span></span>

<span data-ttu-id="98eaf-121">Také lze vytvářet vlastní aplikaci pro přístup k webové službě pomocí počáteční kód stanovené v R, C# a Python programovací jazyky.</span><span class="sxs-lookup"><span data-stu-id="98eaf-121">You can also develop a custom application to access the web service using starter code provided for you in R, C#, and Python programming languages.</span></span>

<span data-ttu-id="98eaf-122">Najdete kompletní informace v [využívání Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="98eaf-122">You can find complete details in [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

