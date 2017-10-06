---
title: "aaaCopy strojového učení příklad experimenty - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse příklad machine learning experimenty toocreate nové experimenty s Cortana Intelligence Gallery a Microsoft Azure Machine Learning."
keywords: machine learning examples, sample experiment, machine learning sample
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: cgronlun
ms.openlocfilehash: 555f05cf9af2040d1703707f7583396d5da9f156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-example-experiments-toocreate-new-machine-learning-experiments"></a><span data-ttu-id="0e844-104">Zkopírujte příklad experimenty toocreate nové strojového učení experimenty</span><span class="sxs-lookup"><span data-stu-id="0e844-104">Copy example experiments toocreate new machine learning experiments</span></span>
<span data-ttu-id="0e844-105">Zjistěte, jak toostart příklad experimenty z [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) místo vytvoření strojovým učením od začátku.</span><span class="sxs-lookup"><span data-stu-id="0e844-105">Learn how toostart with example experiments from [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) instead of creating machine learning experiments from scratch.</span></span> <span data-ttu-id="0e844-106">Příklady toobuild hello můžete použít vlastní strojového učení řešení.</span><span class="sxs-lookup"><span data-stu-id="0e844-106">You can use hello examples toobuild your own machine learning solution.</span></span>

<span data-ttu-id="0e844-107">Galerie Hello má příklad experimenty hello tým Microsoft Azure Machine Learning, jakož i příklady sdíleny hello komunita pro Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0e844-107">hello gallery has example experiments by hello Microsoft Azure Machine Learning team as well as examples shared by hello Machine Learning community.</span></span> <span data-ttu-id="0e844-108">Také můžete klást otázky nebo experimenty komentovat.</span><span class="sxs-lookup"><span data-stu-id="0e844-108">You also can ask questions or post comments about experiments.</span></span>

<span data-ttu-id="0e844-109">toosee jak toouse hello galerie, podívejte se na video 3 minut hello [zkopírujte vědecké zpracování dat jiní lidé pracovní toodo](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) z řady hello [vědecké zpracování dat pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="0e844-109">toosee how toouse hello gallery, watch hello 3-minute video [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) from hello series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-toocopy-in-cortana-intelligence-gallery"></a><span data-ttu-id="0e844-110">Najít toocopy experimentu v Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="0e844-110">Find an experiment toocopy in Cortana Intelligence Gallery</span></span>
<span data-ttu-id="0e844-111">toosee jaké experimenty jsou k dispozici, přejděte toohello [Galerie](https://gallery.cortanaintelligence.com/) a klikněte na tlačítko **experimenty** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="0e844-111">toosee what experiments are available, go toohello [Gallery](https://gallery.cortanaintelligence.com/) and click **Experiments** at hello top of hello page.</span></span>

### <a name="find-hello-newest-or-most-popular-experiments"></a><span data-ttu-id="0e844-112">Najde hello nejnovější nebo nejoblíbenější pokusů</span><span class="sxs-lookup"><span data-stu-id="0e844-112">Find hello newest or most popular experiments</span></span>
<span data-ttu-id="0e844-113">Na této stránce se zobrazí **nedávno přidali** experimenty, nebo se posouvají dolů toolook v **nejoblíbenější** nebo hello nejnovější **populární experimenty Microsoftu**.</span><span class="sxs-lookup"><span data-stu-id="0e844-113">On this page, you can see **Recently added** experiments, or scroll down toolook at **What's popular** or hello latest **Popular Microsoft experiments**.</span></span>

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a><span data-ttu-id="0e844-114">Vyhledání experimentu, který splňuje určité požadavky</span><span class="sxs-lookup"><span data-stu-id="0e844-114">Look for an experiment that meets specific requirements</span></span>
<span data-ttu-id="0e844-115">toobrowse všechny experimenty:</span><span class="sxs-lookup"><span data-stu-id="0e844-115">toobrowse all experiments:</span></span>

1. <span data-ttu-id="0e844-116">Klikněte na tlačítko **procházet všechny** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="0e844-116">Click **Browse all** at hello top of hello page.</span></span>
2. <span data-ttu-id="0e844-117">Na levé straně hello pod **Upřesnit podle** v hello **kategorie** vyberte **experimentu** hello toosee všechny hello experimenty v galerii.</span><span class="sxs-lookup"><span data-stu-id="0e844-117">On hello left-hand side, under **Refine by** in hello **Categories** section, select **Experiment** toosee all hello experiments in hello Gallery.</span></span>
3. <span data-ttu-id="0e844-118">Experimenty, které splňují vaše požadavky, můžete najít několika různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="0e844-118">You can find experiments that meet your requirements a couple different ways:</span></span>
   * <span data-ttu-id="0e844-119">**Vyberte filtry na levé straně hello.**</span><span class="sxs-lookup"><span data-stu-id="0e844-119">**Select filters on hello left.**</span></span> <span data-ttu-id="0e844-120">Například toobrowse experimenty, které používají algoritmus detekce anomálií založený na PCA: S **experimentu** vybrané pod **kategorie**, klikněte na tlačítko **Zobrazit vše**.</span><span class="sxs-lookup"><span data-stu-id="0e844-120">For example, toobrowse experiments that use a PCA-based anomaly detection algorithm: With **Experiment** selected under **Categories**, click **Show all**.</span></span> <span data-ttu-id="0e844-121">Potom v části **Použité algoritmy** zvolte **Detekce anomálií založená na PCA**.</span><span class="sxs-lookup"><span data-stu-id="0e844-121">Then, under **Algorithms Used**, choose **PCA-Based Anomaly Detection**.</span></span> <br></br><span data-ttu-id="0e844-122">
     ![Vybrat filtry](./media/machine-learning-sample-experiments/refine-the-view.png)</span><span class="sxs-lookup"><span data-stu-id="0e844-122">
![Select filters](./media/machine-learning-sample-experiments/refine-the-view.png)</span></span>
   * <span data-ttu-id="0e844-123">**Pomocí vyhledávacího pole hello.**</span><span class="sxs-lookup"><span data-stu-id="0e844-123">**Use hello search box.**</span></span> <span data-ttu-id="0e844-124">Například toofind experimenty přispěl Microsoft související rozpoznávání toodigit, který two-class support vector machine algoritmus, zadejte "digit recognition" hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="0e844-124">For example, toofind experiments contributed by Microsoft related toodigit recognition that use a two-class support vector machine algorithm, enter "digit recognition" in hello search box.</span></span> <span data-ttu-id="0e844-125">Potom vyberte hello filtry **experimentu**, **pouze obsah společnosti Microsoft**, a **Two-Class Support Vector Machine**:</span><span class="sxs-lookup"><span data-stu-id="0e844-125">Then, select hello filters **Experiment**, **Microsoft content only**, and **Two-Class Support Vector Machine**:</span></span><br></br><span data-ttu-id="0e844-126">
     ![Použijte pole hledání hello](./media/machine-learning-sample-experiments/search-for-experiments.png)</span><span class="sxs-lookup"><span data-stu-id="0e844-126">
![Use hello search box](./media/machine-learning-sample-experiments/search-for-experiments.png)</span></span>
4. <span data-ttu-id="0e844-127">Kliknutím experiment toolearn Další informace.</span><span class="sxs-lookup"><span data-stu-id="0e844-127">Click an experiment toolearn more about it.</span></span>
5. <span data-ttu-id="0e844-128">toorun a/nebo upravit hello experiment, klikněte na tlačítko **Open in Studio** na stránce experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="0e844-128">toorun and/or modify hello experiment, click **Open in Studio** on hello experiment's page.</span></span> <br></br>

    ![Příklad experimentu](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > <span data-ttu-id="0e844-130">Když otevřete experimentu v nástroji Machine Learning Studio pro hello poprvé, můžete vyzkoušet zdarma nebo zakoupit předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0e844-130">When you open an experiment in Machine Learning Studio for hello first time, you can try it for free or buy an Azure subscription.</span></span> [<span data-ttu-id="0e844-131">Další informace o hello bezplatné zkušební verze Machine Learning Studio oproti placené služby</span><span class="sxs-lookup"><span data-stu-id="0e844-131">Learn about hello Machine Learning Studio free trial vs. paid service</span></span>](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a><span data-ttu-id="0e844-132">Vytvoření nového experimentu s využitím příkladu jako šablony</span><span class="sxs-lookup"><span data-stu-id="0e844-132">Create a new experiment using an example as a template</span></span>
<span data-ttu-id="0e844-133">Nový experiment v nástroji Machine Learning Studio můžete vytvořit také pomocí příkladu z galerie jako šablony.</span><span class="sxs-lookup"><span data-stu-id="0e844-133">You also can create a new experiment in Machine Learning Studio using a Gallery example as a template.</span></span>

1. <span data-ttu-id="0e844-134">Přihlaste se pomocí vaší toohello přihlašovací údaje účtu Microsoft [Studio](https://studio.azureml.net)a potom klikněte na **nový** toocreate experimentu.</span><span class="sxs-lookup"><span data-stu-id="0e844-134">Sign in with your Microsoft account credentials toohello [Studio](https://studio.azureml.net), and then click **New** toocreate an experiment.</span></span>
2. <span data-ttu-id="0e844-135">Procházet obsah příklad hello a klikněte na jednu.</span><span class="sxs-lookup"><span data-stu-id="0e844-135">Browse through hello example content and click one.</span></span>

<span data-ttu-id="0e844-136">Vytvoří se nový experiment v pracovním prostoru Machine Learning Studio pomocí hello příklad experimentu jako šablona.</span><span class="sxs-lookup"><span data-stu-id="0e844-136">A new experiment is created in your Machine Learning Studio workspace using hello example experiment as a template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e844-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e844-137">Next steps</span></span>
* [<span data-ttu-id="0e844-138">Import dat z různých zdrojů</span><span class="sxs-lookup"><span data-stu-id="0e844-138">Import data from various sources</span></span>](machine-learning-data-science-import-data.md)
* [<span data-ttu-id="0e844-139">Rychlý úvodní kurz pro jazyk hello R v Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0e844-139">Quickstart tutorial for hello R language in Machine Learning</span></span>](machine-learning-r-quickstart.md)
* [<span data-ttu-id="0e844-140">Nasazení webové služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0e844-140">Deploy a Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
