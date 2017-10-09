---
title: aaaDebug modelu v Azure Machine Learning | Microsoft Docs
description: "Jak toodebug chyby vytvořené Train Model a Score Model moduly v Azure Machine Learning."
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
ms.openlocfilehash: ee38ca8ce38d4fc7add5ba70c80ab9bb2ceaf1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a><span data-ttu-id="4fc04-103">Ladění modelu ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4fc04-103">Debug your Model in Azure Machine Learning</span></span>

<span data-ttu-id="4fc04-104">Tento článek vysvětluje hello potenciální z důvodů, proč některá z následujících selhání dvou hello může být došlo při spuštění modelu:</span><span class="sxs-lookup"><span data-stu-id="4fc04-104">This article explains hello potential reasons why either of hello following two failures might be encountered when running a model:</span></span>

* <span data-ttu-id="4fc04-105">Hello [Train Model] [ train-model] modulu vytvoří chybu</span><span class="sxs-lookup"><span data-stu-id="4fc04-105">hello [Train Model][train-model] module produces an error</span></span> 
* <span data-ttu-id="4fc04-106">Hello [Score Model] [ score-model] modulu produkuje nesprávné výsledky</span><span class="sxs-lookup"><span data-stu-id="4fc04-106">hello [Score Model][score-model] module produces incorrect results</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a><span data-ttu-id="4fc04-107">Modulu Train Model vytvoří chybu</span><span class="sxs-lookup"><span data-stu-id="4fc04-107">Train Model Module produces an error</span></span>

![image1](./media/machine-learning-debug-models/train_model-1.png)

<span data-ttu-id="4fc04-109">Hello [Train Model] [ train-model] modulu očekává dva vstupy:</span><span class="sxs-lookup"><span data-stu-id="4fc04-109">hello [Train Model][train-model] Module expects two inputs:</span></span>

1. <span data-ttu-id="4fc04-110">Typ Hello model strojového učení z kolekce hello modelů, které poskytuje Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4fc04-110">hello type of machine learning model from hello collection of models provided by Azure Machine Learning.</span></span>
2. <span data-ttu-id="4fc04-111">Hello Cvičná data se sloupcem zadaného popisku, který určuje hello proměnné toopredict (hello ostatních sloupců se předpokládá, že funkce toobe).</span><span class="sxs-lookup"><span data-stu-id="4fc04-111">hello training data with a specified Label column which specifies hello variable toopredict (hello other columns are assumed toobe Features).</span></span>

<span data-ttu-id="4fc04-112">Tento modul může vést k chybě v následujících případech hello:</span><span class="sxs-lookup"><span data-stu-id="4fc04-112">This module can produce an error in hello following cases:</span></span>

1. <span data-ttu-id="4fc04-113">Popisek sloupce Hello je nesprávně zadán.</span><span class="sxs-lookup"><span data-stu-id="4fc04-113">hello Label column is specified incorrectly.</span></span> <span data-ttu-id="4fc04-114">To může nastat, když je více než jeden sloupec vybraný jako hello popisek nebo je vybrán nesprávný sloupec indexu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-114">This can happen if either more than one column is selected as hello Label or an incorrect column index is selected.</span></span> <span data-ttu-id="4fc04-115">Hello druhém případě byste například použít, pokud index sloupce 30 používá s vstupní datové sady, která má pouze 25 sloupce.</span><span class="sxs-lookup"><span data-stu-id="4fc04-115">For example, hello second case would apply if a column index of 30 is used with an input dataset which has only 25 columns.</span></span>

2. <span data-ttu-id="4fc04-116">Hello datová sada neobsahuje žádné sloupce funkce.</span><span class="sxs-lookup"><span data-stu-id="4fc04-116">hello dataset does not contain any Feature columns.</span></span> <span data-ttu-id="4fc04-117">Například pokud vstupní datové sady hello má jen jeden sloupec, který je označen jako hello popisek sloupce, by žádné funkce, které toobuild hello modelu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-117">For example, if hello input dataset has only one column, which is marked as hello Label column, there would be no features with which toobuild hello model.</span></span> <span data-ttu-id="4fc04-118">V takovém případě hello [Train Model] [ train-model] modulu vytvoří chybu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-118">In this case, hello [Train Model][train-model] module produces an error.</span></span>

3. <span data-ttu-id="4fc04-119">Hello vstupní datové sady (funkce nebo popis) obsahuje Infinity jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-119">hello input dataset (Features or Label) contains Infinity as a value.</span></span>

## <a name="score-model-module-produces-incorrect-results"></a><span data-ttu-id="4fc04-120">Modul určení skóre modelu produkuje nesprávné výsledky</span><span class="sxs-lookup"><span data-stu-id="4fc04-120">Score Model Module produces incorrect results</span></span>

![image2](./media/machine-learning-debug-models/train_test-2.png)

<span data-ttu-id="4fc04-122">V vytvoří typické školení nebo testování experiment pro učení pod dohledem, hello [rozdělení dat] [ split] modulu rozděluje hello původní datové sady do dvou částí: jednou ze součástí je použité tootrain hello modelu a používá se jednou ze součástí tooscore, jak dobře provede hello trained model.</span><span class="sxs-lookup"><span data-stu-id="4fc04-122">In a typical training/testing experiment for supervised learning, hello [Split Data][split] module divides hello original dataset into two parts: one part is used tootrain hello model and one part is used tooscore how well hello trained model performs.</span></span> <span data-ttu-id="4fc04-123">Hello trained model je pak použít tooscore hello testovací data, po který jsou výsledky hello vyhodnotí toodetermine hello přesnost hello modelu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-123">hello trained model is then used tooscore hello test data, after which hello results are evaluated toodetermine hello accuracy of hello model.</span></span>

<span data-ttu-id="4fc04-124">Hello [Score Model] [ score-model] modulu vyžaduje dva vstupy:</span><span class="sxs-lookup"><span data-stu-id="4fc04-124">hello [Score Model][score-model] module requires two inputs:</span></span>

1. <span data-ttu-id="4fc04-125">Výstup trained model z hello [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-125">A trained model output from hello [Train Model][train-model] module.</span></span>
2. <span data-ttu-id="4fc04-126">Vyhodnocování datovou sadu, která se liší od datovou sadu hello používá tootrain hello modelu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-126">A scoring dataset that is different from hello dataset used tootrain hello model.</span></span>

<span data-ttu-id="4fc04-127">To je možné, že i když hello experimentu úspěšné, hello [Score Model] [ score-model] modulu produkuje nesprávné výsledky.</span><span class="sxs-lookup"><span data-stu-id="4fc04-127">It's possible that even though hello experiment succeeds, hello [Score Model][score-model] module produces incorrect results.</span></span> <span data-ttu-id="4fc04-128">Několik scénářů může způsobit, že tento toohappen:</span><span class="sxs-lookup"><span data-stu-id="4fc04-128">Several scenarios may cause this toohappen:</span></span>

1. <span data-ttu-id="4fc04-129">Pokud hello Zadaný popisek je kategorií a regresní model je trénink na hello data, nesprávné výstup podle hello [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-129">If hello specified Label is categorical and a regression model is trained on hello data, an incorrect output would be produced by hello [Score Model][score-model] module.</span></span> <span data-ttu-id="4fc04-130">To je proto regrese vyžaduje proměnnou průběžné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fc04-130">This is because regression requires a continuous response variable.</span></span> <span data-ttu-id="4fc04-131">V takovém případě by bylo vhodnější toouse model klasifikace.</span><span class="sxs-lookup"><span data-stu-id="4fc04-131">In this case, it would be more suitable toouse a classification model.</span></span> 

2. <span data-ttu-id="4fc04-132">Podobně pokud je model klasifikace trénink na datovou sadu s plovoucí desetinnou čárkou v hello popisek sloupce, může způsobit nežádoucí výsledky.</span><span class="sxs-lookup"><span data-stu-id="4fc04-132">Similarly, if a classification model is trained on a dataset having floating point numbers in hello Label column, it may produce undesirable results.</span></span> <span data-ttu-id="4fc04-133">To je proto klasifikace vyžaduje diskrétní odpovědi proměnné, která umožňuje pouze hodnoty rozsahu v rámci omezené a obvykle poněkud malé sady tříd.</span><span class="sxs-lookup"><span data-stu-id="4fc04-133">This is because classification requires a discrete response variable that only allows values that range over a finite, and usually somewhat small, set of classes.</span></span>

3. <span data-ttu-id="4fc04-134">Pokud hello vyhodnocování datová sada neobsahuje všechny hello funkce, které používá tootrain hello modelu, hello [Score Model] [ score-model] vytvoří chybu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-134">If hello scoring dataset does not contain all hello features used tootrain hello model, hello [Score Model][score-model] produces an error.</span></span>

4. <span data-ttu-id="4fc04-135">Pokud řádek v hello vyhodnocování datová sada obsahuje hodnotu chybí, nebo hodnotu nekonečné pro některý z jeho funkce, hello [Score Model] [ score-model] nebudou vytvořeny žádné odpovídající toothat řádek výstupu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-135">If a row in hello scoring dataset contains a missing value or an infinite value for any of its features, hello [Score Model][score-model] will not produce any output corresponding toothat row.</span></span>

5. <span data-ttu-id="4fc04-136">Hello [Score Model] [ score-model] může vytvořit identické výstupy pro všechny řádky v hello vyhodnocování datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="4fc04-136">hello [Score Model][score-model] may produce identical outputs for all rows in hello scoring dataset.</span></span> <span data-ttu-id="4fc04-137">Tato situace může nastat, například při pokusu o zařazení pomocí rozhodnutí doménové struktury, pokud hello minimální počet vzorků na uzel typu list, které jste vybrali toobe více než hello počet školení příklady, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4fc04-137">This could occur, for example, when attempting classification using Decision Forests if hello minimum number of samples per leaf node is chosen toobe more than hello number of training examples available.</span></span>

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

