---
title: "Ladění modelu v Azure Machine Learning | Microsoft Docs"
description: "Postup ladění chyby, vytvořené na moduly Train Model a Score Model v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bradsev;garye
ms.openlocfilehash: d4cc94a6395ea45bccf65d9a9f3118ec98cb258d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a><span data-ttu-id="177b0-103">Ladění modelu ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="177b0-103">Debug your Model in Azure Machine Learning</span></span>

<span data-ttu-id="177b0-104">Tento článek vysvětluje potenciální z důvodů, proč některý z následujících dvou selhání může být došlo při spuštění modelu:</span><span class="sxs-lookup"><span data-stu-id="177b0-104">This article explains the potential reasons why either of the following two failures might be encountered when running a model:</span></span>

* <span data-ttu-id="177b0-105">[Train Model] [ train-model] modulu vytvoří chybu</span><span class="sxs-lookup"><span data-stu-id="177b0-105">the [Train Model][train-model] module produces an error</span></span> 
* <span data-ttu-id="177b0-106">[Score Model] [ score-model] modulu produkuje nesprávné výsledky</span><span class="sxs-lookup"><span data-stu-id="177b0-106">the [Score Model][score-model] module produces incorrect results</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a><span data-ttu-id="177b0-107">Modulu Train Model vytvoří chybu</span><span class="sxs-lookup"><span data-stu-id="177b0-107">Train Model Module produces an error</span></span>

![image1](./media/machine-learning-debug-models/train_model-1.png)

<span data-ttu-id="177b0-109">[Train Model] [ train-model] modulu očekává dva vstupy:</span><span class="sxs-lookup"><span data-stu-id="177b0-109">The [Train Model][train-model] Module expects two inputs:</span></span>

1. <span data-ttu-id="177b0-110">Typ model strojového učení z kolekce modelů, které poskytuje Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="177b0-110">The type of machine learning model from the collection of models provided by Azure Machine Learning.</span></span>
2. <span data-ttu-id="177b0-111">Školení dat pomocí zadaného sloupce popisek, který určuje proměnnou k předvídání (ostatních sloupců se předpokládá, že funkce).</span><span class="sxs-lookup"><span data-stu-id="177b0-111">The training data with a specified Label column which specifies the variable to predict (the other columns are assumed to be Features).</span></span>

<span data-ttu-id="177b0-112">Tento modul může vést k chybě v následujících případech:</span><span class="sxs-lookup"><span data-stu-id="177b0-112">This module can produce an error in the following cases:</span></span>

1. <span data-ttu-id="177b0-113">Popisek sloupce je nesprávně zadán.</span><span class="sxs-lookup"><span data-stu-id="177b0-113">The Label column is specified incorrectly.</span></span> <span data-ttu-id="177b0-114">To může dojít, pokud je vybrána více než jeden sloupec jako popisek nebo je vybrán nesprávný sloupec indexu.</span><span class="sxs-lookup"><span data-stu-id="177b0-114">This can happen if either more than one column is selected as the Label or an incorrect column index is selected.</span></span> <span data-ttu-id="177b0-115">Druhém případě byste například použít, pokud index sloupce 30 používá s vstupní datové sady, která má pouze 25 sloupce.</span><span class="sxs-lookup"><span data-stu-id="177b0-115">For example, the second case would apply if a column index of 30 is used with an input dataset which has only 25 columns.</span></span>

2. <span data-ttu-id="177b0-116">Datová sada neobsahuje žádné sloupce funkce.</span><span class="sxs-lookup"><span data-stu-id="177b0-116">The dataset does not contain any Feature columns.</span></span> <span data-ttu-id="177b0-117">Například pokud vstupní datové sady obsahuje jen jeden sloupec, který je označen jako popisek sloupce, by žádné funkce pro sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="177b0-117">For example, if the input dataset has only one column, which is marked as the Label column, there would be no features with which to build the model.</span></span> <span data-ttu-id="177b0-118">V takovém případě [Train Model] [ train-model] modulu vytvoří chybu.</span><span class="sxs-lookup"><span data-stu-id="177b0-118">In this case, the [Train Model][train-model] module produces an error.</span></span>

3. <span data-ttu-id="177b0-119">Vstupní datové sady (funkce nebo popis) obsahuje Infinity jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="177b0-119">The input dataset (Features or Label) contains Infinity as a value.</span></span>

## <a name="score-model-module-produces-incorrect-results"></a><span data-ttu-id="177b0-120">Modul určení skóre modelu produkuje nesprávné výsledky</span><span class="sxs-lookup"><span data-stu-id="177b0-120">Score Model Module produces incorrect results</span></span>

![image2](./media/machine-learning-debug-models/train_test-2.png)

<span data-ttu-id="177b0-122">V typické experimentu školení/testování pro učení se supervizí [rozdělení dat] [ split] modulu rozděluje původní datové sady do dvou částí: jednou ze součástí se používá pro trénování modelu a jednou ze součástí slouží ke stanovení skóre jak dobře provede naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="177b0-122">In a typical training/testing experiment for supervised learning, the [Split Data][split] module divides the original dataset into two parts: one part is used to train the model and one part is used to score how well the trained model performs.</span></span> <span data-ttu-id="177b0-123">Pro cvičný model se pak použije skóre pro testovací data, po jejímž uplynutí se vyhodnocují výsledky k určení přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="177b0-123">The trained model is then used to score the test data, after which the results are evaluated to determine the accuracy of the model.</span></span>

<span data-ttu-id="177b0-124">[Score Model] [ score-model] modulu vyžaduje dva vstupy:</span><span class="sxs-lookup"><span data-stu-id="177b0-124">The [Score Model][score-model] module requires two inputs:</span></span>

1. <span data-ttu-id="177b0-125">Výstup trained model z [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="177b0-125">A trained model output from the [Train Model][train-model] module.</span></span>
2. <span data-ttu-id="177b0-126">Vyhodnocování datovou sadu, která se liší od datovou sadu použity při cvičení modelu.</span><span class="sxs-lookup"><span data-stu-id="177b0-126">A scoring dataset that is different from the dataset used to train the model.</span></span>

<span data-ttu-id="177b0-127">Je možné, že i když úspěšné experiment, [Score Model] [ score-model] modulu produkuje nesprávné výsledky.</span><span class="sxs-lookup"><span data-stu-id="177b0-127">It's possible that even though the experiment succeeds, the [Score Model][score-model] module produces incorrect results.</span></span> <span data-ttu-id="177b0-128">Může způsobit několik scénářů, k tomu dojít:</span><span class="sxs-lookup"><span data-stu-id="177b0-128">Several scenarios may cause this to happen:</span></span>

1. <span data-ttu-id="177b0-129">Pokud je kategorií zadaným popiskem a regresní model je trénink na data, nesprávné výstup podle [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="177b0-129">If the specified Label is categorical and a regression model is trained on the data, an incorrect output would be produced by the [Score Model][score-model] module.</span></span> <span data-ttu-id="177b0-130">To je proto regrese vyžaduje proměnnou průběžné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="177b0-130">This is because regression requires a continuous response variable.</span></span> <span data-ttu-id="177b0-131">V takovém případě by bylo vhodnější použít model klasifikace.</span><span class="sxs-lookup"><span data-stu-id="177b0-131">In this case, it would be more suitable to use a classification model.</span></span> 

2. <span data-ttu-id="177b0-132">Podobně pokud na datovou sadu s plovoucí desetinnou čárkou ve sloupci popisek cvičení modelu klasifikace, může způsobit nežádoucí výsledky.</span><span class="sxs-lookup"><span data-stu-id="177b0-132">Similarly, if a classification model is trained on a dataset having floating point numbers in the Label column, it may produce undesirable results.</span></span> <span data-ttu-id="177b0-133">To je proto klasifikace vyžaduje diskrétní odpovědi proměnné, která umožňuje pouze hodnoty rozsahu v rámci omezené a obvykle poněkud malé sady tříd.</span><span class="sxs-lookup"><span data-stu-id="177b0-133">This is because classification requires a discrete response variable that only allows values that range over a finite, and usually somewhat small, set of classes.</span></span>

3. <span data-ttu-id="177b0-134">Pokud vyhodnocování datová sada neobsahuje všechny funkce, které jsou použity při cvičení modelu [Score Model] [ score-model] vytvoří chybu.</span><span class="sxs-lookup"><span data-stu-id="177b0-134">If the scoring dataset does not contain all the features used to train the model, the [Score Model][score-model] produces an error.</span></span>

4. <span data-ttu-id="177b0-135">Pokud v řádku v vyhodnocování datová sada obsahuje hodnotu chybí, nebo hodnotu nekonečné jednotlivých funkcí, [Score Model] [ score-model] nebude vytvoření jakéhokoli výstupu odpovídající na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="177b0-135">If a row in the scoring dataset contains a missing value or an infinite value for any of its features, the [Score Model][score-model] will not produce any output corresponding to that row.</span></span>

5. <span data-ttu-id="177b0-136">[Score Model] [ score-model] může vytvořit identické výstupy pro všechny řádky v vyhodnocování datové sadě.</span><span class="sxs-lookup"><span data-stu-id="177b0-136">The [Score Model][score-model] may produce identical outputs for all rows in the scoring dataset.</span></span> <span data-ttu-id="177b0-137">Tato situace může nastat, například při pokusu o zařazení pomocí rozhodnutí doménové struktury, pokud je minimální počet vzorků na uzel typu list zvolena být vyšší než počet školení příklady, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="177b0-137">This could occur, for example, when attempting classification using Decision Forests if the minimum number of samples per leaf node is chosen to be more than the number of training examples available.</span></span>

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

