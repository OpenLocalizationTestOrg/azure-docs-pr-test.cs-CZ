---
title: "Kopírování příkladů experimentů se strojovým učením – Azure | Dokumentace Microsoftu"
description: "Naučte se používat příklady experimentů se strojovým učením k vytváření nových experimentů s galerií Cortana Intelligence a službou Microsoft Azure Machine Learning."
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
ms.openlocfilehash: 55f9bd2ed0d555a14d31bf3d262707d65bd70244
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a><span data-ttu-id="4303d-104">Vytváření nových experimentů se strojovým učením na základě kopírování příkladů experimentů</span><span class="sxs-lookup"><span data-stu-id="4303d-104">Copy example experiments to create new machine learning experiments</span></span>
<span data-ttu-id="4303d-105">Zjistěte, jak začít s příklady experimentů z [Galerie Cortana Intelligence](https://gallery.cortanaintelligence.com/) místo vytváření experimentů se strojovým učením od začátku.</span><span class="sxs-lookup"><span data-stu-id="4303d-105">Learn how to start with example experiments from [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) instead of creating machine learning experiments from scratch.</span></span> <span data-ttu-id="4303d-106">Příklady můžete použít k sestavení vlastních řešení strojového učení.</span><span class="sxs-lookup"><span data-stu-id="4303d-106">You can use the examples to build your own machine learning solution.</span></span>

<span data-ttu-id="4303d-107">V galerii jsou příklady experimentů od týmu Microsoft Azure Machine Learning i příklady sdílené komunitou služby Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4303d-107">The gallery has example experiments by the Microsoft Azure Machine Learning team as well as examples shared by the Machine Learning community.</span></span> <span data-ttu-id="4303d-108">Také můžete klást otázky nebo experimenty komentovat.</span><span class="sxs-lookup"><span data-stu-id="4303d-108">You also can ask questions or post comments about experiments.</span></span>

<span data-ttu-id="4303d-109">Abyste se dozvěděli, jak používat galerii, podívejte se na tříminutové video [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Kopírování práce jiných lidí pro vědecké zkoumání dat) z řady [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Vědecké zkoumání dat pro začátečníky).</span><span class="sxs-lookup"><span data-stu-id="4303d-109">To see how to use the gallery, watch the 3-minute video [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) from the series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a><span data-ttu-id="4303d-110">Nalezení experimentu pro zkopírování v Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="4303d-110">Find an experiment to copy in Cortana Intelligence Gallery</span></span>
<span data-ttu-id="4303d-111">Pokud se chcete podívat, jaké experimenty jsou k dispozici, přejděte do [Galerie](https://gallery.cortanaintelligence.com/) a klikněte na **Experimenty** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="4303d-111">To see what experiments are available, go to the [Gallery](https://gallery.cortanaintelligence.com/) and click **Experiments** at the top of the page.</span></span>

### <a name="find-the-newest-or-most-popular-experiments"></a><span data-ttu-id="4303d-112">Nalezení nejnovějších nebo nejoblíbenějších experimentů</span><span class="sxs-lookup"><span data-stu-id="4303d-112">Find the newest or most popular experiments</span></span>
<span data-ttu-id="4303d-113">Na této stránce si můžete zobrazit **naposled přidané** experimenty, přechodem dolů se podívat, **co je oblíbené**, nebo si prohlédnout nejnovější **populární experimenty Microsoftu**.</span><span class="sxs-lookup"><span data-stu-id="4303d-113">On this page, you can see **Recently added** experiments, or scroll down to look at **What's popular** or the latest **Popular Microsoft experiments**.</span></span>

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a><span data-ttu-id="4303d-114">Vyhledání experimentu, který splňuje určité požadavky</span><span class="sxs-lookup"><span data-stu-id="4303d-114">Look for an experiment that meets specific requirements</span></span>
<span data-ttu-id="4303d-115">Procházení všech experimentů:</span><span class="sxs-lookup"><span data-stu-id="4303d-115">To browse all experiments:</span></span>

1. <span data-ttu-id="4303d-116">V horní části stránky klikněte na **Browse all** (Procházet vše).</span><span class="sxs-lookup"><span data-stu-id="4303d-116">Click **Browse all** at the top of the page.</span></span>
2. <span data-ttu-id="4303d-117">Na levé straně v oddílu **Zpřesnit podle** v části **Kategorie** vyberte **Experiment**. Zobrazí se všechny experimenty v galerii.</span><span class="sxs-lookup"><span data-stu-id="4303d-117">On the left-hand side, under **Refine by** in the **Categories** section, select **Experiment** to see all the experiments in the Gallery.</span></span>
3. <span data-ttu-id="4303d-118">Experimenty, které splňují vaše požadavky, můžete najít několika různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="4303d-118">You can find experiments that meet your requirements a couple different ways:</span></span>
   * <span data-ttu-id="4303d-119">**Vyberte filtry vlevo.**</span><span class="sxs-lookup"><span data-stu-id="4303d-119">**Select filters on the left.**</span></span> <span data-ttu-id="4303d-120">Pokud chcete například procházet experimenty, které používají algoritmus detekce anomálií založený na PCA, s vybranou možností **Experiment** v části **Kategorie** klikněte na **Zobrazit vše**.</span><span class="sxs-lookup"><span data-stu-id="4303d-120">For example, to browse experiments that use a PCA-based anomaly detection algorithm: With **Experiment** selected under **Categories**, click **Show all**.</span></span> <span data-ttu-id="4303d-121">Potom v části **Použité algoritmy** zvolte **Detekce anomálií založená na PCA**.</span><span class="sxs-lookup"><span data-stu-id="4303d-121">Then, under **Algorithms Used**, choose **PCA-Based Anomaly Detection**.</span></span> <br></br><span data-ttu-id="4303d-122">
     ![Vybrat filtry](./media/machine-learning-sample-experiments/refine-the-view.png)</span><span class="sxs-lookup"><span data-stu-id="4303d-122">
![Select filters](./media/machine-learning-sample-experiments/refine-the-view.png)</span></span>
   * <span data-ttu-id="4303d-123">**Použijte vyhledávací pole.**</span><span class="sxs-lookup"><span data-stu-id="4303d-123">**Use the search box.**</span></span> <span data-ttu-id="4303d-124">Pokud chcete například najít experimenty, kterými přispěl Microsoft, které se týkají rozpoznávaní číslic a které používají algoritmus podpůrného vektorového stroje se dvěma třídami, zadejte do vyhledávacího pole „digit recognition“.</span><span class="sxs-lookup"><span data-stu-id="4303d-124">For example, to find experiments contributed by Microsoft related to digit recognition that use a two-class support vector machine algorithm, enter "digit recognition" in the search box.</span></span> <span data-ttu-id="4303d-125">Vyberte filtry **Experiment**, **Microsoft content only** (Jen obsah Microsoftu) a **Two-Class Support Vector Machine**:</span><span class="sxs-lookup"><span data-stu-id="4303d-125">Then, select the filters **Experiment**, **Microsoft content only**, and **Two-Class Support Vector Machine**:</span></span><br></br><span data-ttu-id="4303d-126">
     ![Použití vyhledávacího pole](./media/machine-learning-sample-experiments/search-for-experiments.png)</span><span class="sxs-lookup"><span data-stu-id="4303d-126">
![Use the search box](./media/machine-learning-sample-experiments/search-for-experiments.png)</span></span>
4. <span data-ttu-id="4303d-127">Kliknutím na experiment o něm zobrazíte více informací.</span><span class="sxs-lookup"><span data-stu-id="4303d-127">Click an experiment to learn more about it.</span></span>
5. <span data-ttu-id="4303d-128">Pokud chcete experiment spustit nebo upravit, klikněte na stránce experimentu na **Open in Studio** (Otevřít v nástroji Studio).</span><span class="sxs-lookup"><span data-stu-id="4303d-128">To run and/or modify the experiment, click **Open in Studio** on the experiment's page.</span></span> <br></br>

    ![Příklad experimentu](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > <span data-ttu-id="4303d-130">Když poprvé otevřete experiment v Machine Learning Studiu, můžete ho vyzkoušet zdarma, nebo si koupit předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4303d-130">When you open an experiment in Machine Learning Studio for the first time, you can try it for free or buy an Azure subscription.</span></span> [<span data-ttu-id="4303d-131">Další informace o bezplatné zkušební verzi Machine Learning Studia v porovnání s placenou službou</span><span class="sxs-lookup"><span data-stu-id="4303d-131">Learn about the Machine Learning Studio free trial vs. paid service</span></span>](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a><span data-ttu-id="4303d-132">Vytvoření nového experimentu s využitím příkladu jako šablony</span><span class="sxs-lookup"><span data-stu-id="4303d-132">Create a new experiment using an example as a template</span></span>
<span data-ttu-id="4303d-133">Nový experiment v nástroji Machine Learning Studio můžete vytvořit také pomocí příkladu z galerie jako šablony.</span><span class="sxs-lookup"><span data-stu-id="4303d-133">You also can create a new experiment in Machine Learning Studio using a Gallery example as a template.</span></span>

1. <span data-ttu-id="4303d-134">Přihlaste se do [Studia](https://studio.azureml.net) pomocí přihlašovacích údajů účtu Microsoft a kliknutím na **Nový** vytvořte experiment.</span><span class="sxs-lookup"><span data-stu-id="4303d-134">Sign in with your Microsoft account credentials to the [Studio](https://studio.azureml.net), and then click **New** to create an experiment.</span></span>
2. <span data-ttu-id="4303d-135">Projděte si obsah příkladu a na některý klikněte.</span><span class="sxs-lookup"><span data-stu-id="4303d-135">Browse through the example content and click one.</span></span>

<span data-ttu-id="4303d-136">V pracovním prostoru Machine Learning Studio se vytvoří nový experiment s využitím příkladu experimentu jako šablony.</span><span class="sxs-lookup"><span data-stu-id="4303d-136">A new experiment is created in your Machine Learning Studio workspace using the example experiment as a template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4303d-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4303d-137">Next steps</span></span>
* [<span data-ttu-id="4303d-138">Import dat z různých zdrojů</span><span class="sxs-lookup"><span data-stu-id="4303d-138">Import data from various sources</span></span>](machine-learning-data-science-import-data.md)
* [<span data-ttu-id="4303d-139">Stručný úvodní kurz k jazyku R ve službě Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4303d-139">Quickstart tutorial for the R language in Machine Learning</span></span>](machine-learning-r-quickstart.md)
* [<span data-ttu-id="4303d-140">Nasazení webové služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4303d-140">Deploy a Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
