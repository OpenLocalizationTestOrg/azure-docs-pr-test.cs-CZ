---
title: "Prediktivní řešení pro úvěrové riziko v Machine Learning | Dokumentace Microsoftu"
description: "Podrobný průvodce postupem vytvoření řešení prediktivní analýzy pro posuzování úvěrového rizika v nástroji Azure Machine Learning Studio"
keywords: "úvěrové riziko,řešení prediktivní analýzy,posouzení rizika"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: e1af999b2fde8ffa2a0ffd1b88230f0aaab32b37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a><span data-ttu-id="08fb4-104">Názorný průvodce: Vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="08fb4-104">Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning</span></span>

<span data-ttu-id="08fb4-105">V tomto názorném průvodci se zaměříme na proces vývoje řešení prediktivní analýzy v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="08fb4-105">In this walkthrough, we take an extended look at the process of developing a predictive analytics solution in Machine Learning Studio.</span></span> <span data-ttu-id="08fb4-106">Vytvoříme si jednoduchý model v nástroji Machine Learning Studio a pak ho nasadíme jako webovou službu Azure Machine Learning, ve které může model vytvářet predikce na základě nových dat.</span><span class="sxs-lookup"><span data-stu-id="08fb4-106">We develop a simple model in Machine Learning Studio, and then deploy it as an Azure Machine Learning web service where the model can make predictions using new data.</span></span> 

<span data-ttu-id="08fb4-107">Tento názorný průvodce předpokládá, že jste nejméně jednou předtím použili Machine Learning Studio a že rozumíte konceptům strojového učení.</span><span class="sxs-lookup"><span data-stu-id="08fb4-107">This walkthrough assumes that you've used Machine Learning Studio at least once before, and that you have some understanding of machine learning concepts.</span></span> <span data-ttu-id="08fb4-108">Bere ale v úvahu, že nejste odborníkem ani na jedno.</span><span class="sxs-lookup"><span data-stu-id="08fb4-108">But it doesn't assume you're an expert in either.</span></span>

<span data-ttu-id="08fb4-109">Pokud jste nástroj **Azure Machine Learning Studio** ještě nikdy nepoužívali, možná budete chtít začít kurzem [Vytvoření prvního experimentu datové vědy v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="08fb4-109">If you've never used **Azure Machine Learning Studio** before, you might want to start with the tutorial, [Create your first data science experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).</span></span> <span data-ttu-id="08fb4-110">Tento kurz vás poprvé provede nástrojem Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="08fb4-110">That tutorial takes you through Machine Learning Studio for the first time.</span></span> <span data-ttu-id="08fb4-111">Ukáže vám základy toho, jak pomocí myši přetáhnout moduly do experimentu, vzájemně je propojit, spustit experiment a prohlédnout si výsledky.</span><span class="sxs-lookup"><span data-stu-id="08fb4-111">It shows you the basics of how to drag-and-drop modules onto your experiment, connect them together, run the experiment, and look at the results.</span></span> <span data-ttu-id="08fb4-112">Dalším nástrojem, který vám může pomoci začít, je diagram s přehledem možností nástroje Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="08fb4-112">Another tool that may be helpful for getting started is a diagram that gives an overview of the capabilities of Machine Learning Studio.</span></span> <span data-ttu-id="08fb4-113">Stáhnout a vytisknout si ho můžete odtud: [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).</span><span class="sxs-lookup"><span data-stu-id="08fb4-113">You can download and print it from here: [Overview diagram of Azure Machine Learning Studio capabilities](machine-learning-studio-overview-diagram.md).</span></span>
 
<span data-ttu-id="08fb4-114">Pokud se strojovým učením obecně teprve začínáte, může vám pomoci série videí.</span><span class="sxs-lookup"><span data-stu-id="08fb4-114">If you're new to the field of machine learning in general, there's a video series that might be helpful to you.</span></span> <span data-ttu-id="08fb4-115">Má název [Datová věda pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) a poskytuje vynikající úvod do strojového učení s využitím jazyka a konceptů každodenního života.</span><span class="sxs-lookup"><span data-stu-id="08fb4-115">It's called [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) and it can give you a great introduction to machine learning using everyday language and concepts.</span></span>


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a><span data-ttu-id="08fb4-116">Problém</span><span class="sxs-lookup"><span data-stu-id="08fb4-116">The problem</span></span>

<span data-ttu-id="08fb4-117">Předpokládejme, že potřebujete předpovědět úvěrové riziko u jednotlivých zákazníků na základě údajů, které uvedli v žádosti o úvěr.</span><span class="sxs-lookup"><span data-stu-id="08fb4-117">Suppose you need to predict an individual's credit risk based on the information they gave on a credit application.</span></span>  

<span data-ttu-id="08fb4-118">Posouzení úvěrového rizika je komplexní problém, ale pro účely tohoto názorného průvodce jej můžeme zjednodušit.</span><span class="sxs-lookup"><span data-stu-id="08fb4-118">Credit risk assessment is a complex problem, but we can simplify it a bit for this walkthrough.</span></span> <span data-ttu-id="08fb4-119">Použijeme jej jako příklad způsobu, jakým můžete vytvořit řešení prediktivní analýzy pomocí služby Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="08fb4-119">We'll use it as an example of how you can create a predictive analytics solution using Microsoft Azure Machine Learning.</span></span> <span data-ttu-id="08fb4-120">Využijeme k tomu nástroj Azure Machine Learning Studio a webovou službu Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="08fb4-120">To do this, we use Azure Machine Learning Studio and a Machine Learning web service.</span></span>  

## <a name="the-solution"></a><span data-ttu-id="08fb4-121">Řešení</span><span class="sxs-lookup"><span data-stu-id="08fb4-121">The solution</span></span>

<span data-ttu-id="08fb4-122">V tomto podrobném průvodci začneme s veřejně dostupnými daty o úvěrovém riziku, na jejichž základě vyvineme a natrénujeme prediktivní model.</span><span class="sxs-lookup"><span data-stu-id="08fb4-122">In this detailed walkthrough, we start with publicly available credit risk data and develop and train a predictive model based on that data.</span></span> <span data-ttu-id="08fb4-123">Potom model nasadíme jako webovou službu, aby ji pro posuzování úvěrového rizika mohli využívat další uživatelé.</span><span class="sxs-lookup"><span data-stu-id="08fb4-123">Then we deploy the model as a web service so it can be used by others for credit risk assessment.</span></span>

<span data-ttu-id="08fb4-124">Při vytváření řešení pro posuzování úvěrového rizika budeme postupovat po těchto krocích:</span><span class="sxs-lookup"><span data-stu-id="08fb4-124">To create this credit risk assessment solution, we follow these steps:</span></span>  

1. [<span data-ttu-id="08fb4-125">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="08fb4-125">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="08fb4-126">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="08fb4-126">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="08fb4-127">Vytvoření experimentu</span><span class="sxs-lookup"><span data-stu-id="08fb4-127">Create an experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="08fb4-128">Natrénování a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="08fb4-128">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="08fb4-129">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="08fb4-129">Deploy the web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="08fb4-130">Nastavení přístupu k webové službě</span><span class="sxs-lookup"><span data-stu-id="08fb4-130">Access the web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> <span data-ttu-id="08fb4-131">Pracovní kopii experimentu, který vyvíjíme v tomto názorném průvodci, najdete v [Galerii Cortana Intelligence](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="08fb4-131">You can find a working copy of the experiment that we develop in this walkthrough in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="08fb4-132">Přejděte k části **[Názorný průvodce – Predikce úvěrového rizika](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**, klikněte na **Open in Studio** (Otevřít v sadě Studio) a stáhněte kopii experimentu do pracovního prostoru Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="08fb4-132">Go to **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** and click **Open in Studio** to download a copy of the experiment into your Machine Learning Studio workspace.</span></span>
> 
> <span data-ttu-id="08fb4-133">Tento názorný postup je založen na zjednodušené verzi ukázkového experimentu [Binární klasifikace: Predikce úvěrového rizika](http://go.microsoft.com/fwlink/?LinkID=525270), který je také k dispozici v [Galerii](http://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="08fb4-133">This walkthrough is based on a simplified version of the sample experiment, [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), also available in the [Gallery](http://gallery.cortanaintelligence.com/).</span></span>
