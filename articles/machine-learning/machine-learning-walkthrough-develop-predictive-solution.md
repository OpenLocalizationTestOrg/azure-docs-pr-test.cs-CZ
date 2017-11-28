---
title: "aaaA prediktivní řešení pro úvěrové riziko v Machine Learning | Microsoft Docs"
description: "Podrobný průvodce jak toocreate řešení prediktivní analýzy pro platební hodnocení rizik v Azure Machine Learning Studio."
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
ms.openlocfilehash: 00ed39081e6952b8d03b37a8285d8dc3ddff2cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a><span data-ttu-id="cf669-104">Názorný průvodce: Vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cf669-104">Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning</span></span>

<span data-ttu-id="cf669-105">V tomto návodu budeme prohlédněte rozšířené hello proces vývoj řešení prediktivní analýzy v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="cf669-105">In this walkthrough, we take an extended look at hello process of developing a predictive analytics solution in Machine Learning Studio.</span></span> <span data-ttu-id="cf669-106">Jsme vývoj jednoduchého modelu v nástroji Machine Learning Studio a nasaďte ho jako webové služby Azure Machine Learning, kde může hello modelu provádět předpovědi pomocí nová data.</span><span class="sxs-lookup"><span data-stu-id="cf669-106">We develop a simple model in Machine Learning Studio, and then deploy it as an Azure Machine Learning web service where hello model can make predictions using new data.</span></span> 

<span data-ttu-id="cf669-107">Tento názorný průvodce předpokládá, že jste nejméně jednou předtím použili Machine Learning Studio a že rozumíte konceptům strojového učení.</span><span class="sxs-lookup"><span data-stu-id="cf669-107">This walkthrough assumes that you've used Machine Learning Studio at least once before, and that you have some understanding of machine learning concepts.</span></span> <span data-ttu-id="cf669-108">Bere ale v úvahu, že nejste odborníkem ani na jedno.</span><span class="sxs-lookup"><span data-stu-id="cf669-108">But it doesn't assume you're an expert in either.</span></span>

<span data-ttu-id="cf669-109">Pokud jste použili nikdy **Azure Machine Learning Studio** před můžete chtít toostart s hello kurzu [vytvořit první vědecké zpracování dat experiment v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="cf669-109">If you've never used **Azure Machine Learning Studio** before, you might want toostart with hello tutorial, [Create your first data science experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).</span></span> <span data-ttu-id="cf669-110">Tento kurz vás provede Machine Learning Studio pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="cf669-110">That tutorial takes you through Machine Learning Studio for hello first time.</span></span> <span data-ttu-id="cf669-111">Zobrazuje hello základní informace o tom, jak toodrag myší modulů do experimentu, připojte je společně, spustit hello experiment a podívejte se na výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="cf669-111">It shows you hello basics of how toodrag-and-drop modules onto your experiment, connect them together, run hello experiment, and look at hello results.</span></span> <span data-ttu-id="cf669-112">Jiný nástroj, který může být užitečné při zahájení práce je diagram, který bude stručně charakterizovat hello možností nástroje Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="cf669-112">Another tool that may be helpful for getting started is a diagram that gives an overview of hello capabilities of Machine Learning Studio.</span></span> <span data-ttu-id="cf669-113">Stáhnout a vytisknout si ho můžete odtud: [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).</span><span class="sxs-lookup"><span data-stu-id="cf669-113">You can download and print it from here: [Overview diagram of Azure Machine Learning Studio capabilities](machine-learning-studio-overview-diagram.md).</span></span>
 
<span data-ttu-id="cf669-114">Pokud jste nové pole toohello strojového učení obecně, je série videí, který může být užitečné tooyou.</span><span class="sxs-lookup"><span data-stu-id="cf669-114">If you're new toohello field of machine learning in general, there's a video series that might be helpful tooyou.</span></span> <span data-ttu-id="cf669-115">Je volána [vědecké zpracování dat pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) a ho můžete udělit pomocí jazyka pro každý den a koncepty výukový toomachine Skvělý úvod.</span><span class="sxs-lookup"><span data-stu-id="cf669-115">It's called [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) and it can give you a great introduction toomachine learning using everyday language and concepts.</span></span>


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="hello-problem"></a><span data-ttu-id="cf669-116">problém Hello</span><span class="sxs-lookup"><span data-stu-id="cf669-116">hello problem</span></span>

<span data-ttu-id="cf669-117">Předpokládejme, že potřebujete toopredict jednotlivého uživatele úvěrové riziko podle hello informace, které se přiřadil v o úvěr.</span><span class="sxs-lookup"><span data-stu-id="cf669-117">Suppose you need toopredict an individual's credit risk based on hello information they gave on a credit application.</span></span>  

<span data-ttu-id="cf669-118">Posouzení úvěrového rizika je komplexní problém, ale pro účely tohoto názorného průvodce jej můžeme zjednodušit.</span><span class="sxs-lookup"><span data-stu-id="cf669-118">Credit risk assessment is a complex problem, but we can simplify it a bit for this walkthrough.</span></span> <span data-ttu-id="cf669-119">Použijeme jej jako příklad způsobu, jakým můžete vytvořit řešení prediktivní analýzy pomocí služby Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="cf669-119">We'll use it as an example of how you can create a predictive analytics solution using Microsoft Azure Machine Learning.</span></span> <span data-ttu-id="cf669-120">toodo toho používáme Azure Machine Learning Studio a webové službě Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="cf669-120">toodo this, we use Azure Machine Learning Studio and a Machine Learning web service.</span></span>  

## <a name="hello-solution"></a><span data-ttu-id="cf669-121">řešení Hello</span><span class="sxs-lookup"><span data-stu-id="cf669-121">hello solution</span></span>

<span data-ttu-id="cf669-122">V tomto podrobném průvodci začneme s veřejně dostupnými daty o úvěrovém riziku, na jejichž základě vyvineme a natrénujeme prediktivní model.</span><span class="sxs-lookup"><span data-stu-id="cf669-122">In this detailed walkthrough, we start with publicly available credit risk data and develop and train a predictive model based on that data.</span></span> <span data-ttu-id="cf669-123">Pak můžeme hello model nasadit jako webovou službu, takže ho můžete použít třetími stranami pro posuzování úvěrového rizika.</span><span class="sxs-lookup"><span data-stu-id="cf669-123">Then we deploy hello model as a web service so it can be used by others for credit risk assessment.</span></span>

<span data-ttu-id="cf669-124">toocreate toto řešení hodnocení rizik platební jsme postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="cf669-124">toocreate this credit risk assessment solution, we follow these steps:</span></span>  

1. [<span data-ttu-id="cf669-125">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cf669-125">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="cf669-126">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="cf669-126">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="cf669-127">Vytvoření experimentu</span><span class="sxs-lookup"><span data-stu-id="cf669-127">Create an experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="cf669-128">Natrénování a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="cf669-128">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="cf669-129">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="cf669-129">Deploy hello web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="cf669-130">Přístup hello webové služby</span><span class="sxs-lookup"><span data-stu-id="cf669-130">Access hello web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> <span data-ttu-id="cf669-131">Pracovní kopie hello experimentu, který jsme vyvíjet můžete najít v tomto návodu v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="cf669-131">You can find a working copy of hello experiment that we develop in this walkthrough in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="cf669-132">Přejděte příliš**[návod - úvěrové riziko předpovědi](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**  a klikněte na tlačítko **Open in Studio** toodownload kopii hello experimentu do pracovního prostoru Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="cf669-132">Go too**[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** and click **Open in Studio** toodownload a copy of hello experiment into your Machine Learning Studio workspace.</span></span>
> 
> <span data-ttu-id="cf669-133">Tento názorný postup je založen na zjednodušené verzi ukázkového experimentu hello, [binární klasifikace: predikce úvěrového rizika](http://go.microsoft.com/fwlink/?LinkID=525270), je dostupný také ve hello [Galerie](http://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="cf669-133">This walkthrough is based on a simplified version of hello sample experiment, [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), also available in hello [Gallery](http://gallery.cortanaintelligence.com/).</span></span>
