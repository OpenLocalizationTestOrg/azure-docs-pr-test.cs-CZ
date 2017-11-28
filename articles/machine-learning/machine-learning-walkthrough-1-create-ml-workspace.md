---
title: "Krok 1: Vytvoření pracovního prostoru Machine Learning | Microsoft Docs"
description: "Krok 1 hello vývoj prediktivního řešení návod: Zjistěte, jak tooset si nový pracovní prostor Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 93d2e240826db9768e85b00cab0eb62510b4efb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a><span data-ttu-id="559b2-103">Krok 1 průvodce: Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="559b2-103">Walkthrough Step 1: Create a Machine Learning workspace</span></span>
<span data-ttu-id="559b2-104">Toto je první krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span><span class="sxs-lookup"><span data-stu-id="559b2-104">This is hello first step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span></span>

1. <span data-ttu-id="559b2-105">**Vytvoření pracovního prostoru Machine Learning**</span><span class="sxs-lookup"><span data-stu-id="559b2-105">**Create a Machine Learning workspace**</span></span>
2. [<span data-ttu-id="559b2-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="559b2-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="559b2-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="559b2-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="559b2-108">Natrénování a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="559b2-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="559b2-109">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="559b2-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="559b2-110">Přístup k webové službě hello</span><span class="sxs-lookup"><span data-stu-id="559b2-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs toobe updated toorefer toohello new way of creating workspaces in hello Ibiza portal -->

<span data-ttu-id="559b2-111">toouse Machine Learning Studio, musíte toohave prostoru Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="559b2-111">toouse Machine Learning Studio, you need toohave a Microsoft Azure Machine Learning workspace.</span></span> <span data-ttu-id="559b2-112">Tento pracovní prostor obsahuje hello nástroje, které potřebujete toocreate, spravovat a publikovat experimenty.</span><span class="sxs-lookup"><span data-stu-id="559b2-112">This workspace contains hello tools you need toocreate, manage, and publish experiments.</span></span>  

<!--
## toocreate a workspace
1. Sign in toohello [Azure classic portal](https://manage.windowsazure.com).
2. In hello  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On hello **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

<span data-ttu-id="559b2-113">Hello správce pro vašeho předplatného Azure potřebuje toocreate hello prostoru a potom je přidat jako roli vlastníka nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="559b2-113">hello administrator for your Azure subscription needs toocreate hello workspace and then add you as an owner or contributor.</span></span> <span data-ttu-id="559b2-114">Podrobnosti najdete v tématu [vytvoření a sdílení pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="559b2-114">For details, see [Create and share an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

<span data-ttu-id="559b2-115">Po vytvoření pracovního prostoru Machine Learning Studio otevřete ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span><span class="sxs-lookup"><span data-stu-id="559b2-115">After your workspace is created, open Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span></span> <span data-ttu-id="559b2-116">Pokud máte více než jednoho pracovního prostoru, můžete vybrat hello prostoru v panelu nástrojů hello v pravém horním rohu hello okna hello.</span><span class="sxs-lookup"><span data-stu-id="559b2-116">If you have more than one workspace, you can select hello workspace in hello toolbar in hello upper-right corner of hello window.</span></span>

![Vyberte pracovní prostor v Studio][2]

> [!TIP]
> <span data-ttu-id="559b2-118">Pokud byly provedeny vlastníka hello pracovního prostoru, můžete sdílet hello experimenty pracujete pomocí vyzvání jiných uživatelů toohello prostoru.</span><span class="sxs-lookup"><span data-stu-id="559b2-118">If you were made an owner of hello workspace, you can share hello experiments you're working on by inviting others toohello workspace.</span></span> <span data-ttu-id="559b2-119">To provedete v nástroji Machine Learning Studio na hello **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="559b2-119">You can do this in Machine Learning Studio on hello **SETTINGS** page.</span></span> <span data-ttu-id="559b2-120">Stačí hello účtu Microsoft nebo účtu organizace pro každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="559b2-120">You just need hello Microsoft account or organizational account for each user.</span></span>
> 
> <span data-ttu-id="559b2-121">Na hello **nastavení** klikněte na tlačítko **uživatelé**, pak klikněte na tlačítko **POZVAT uživatele více** v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="559b2-121">On hello **SETTINGS** page, click **USERS**, then click **INVITE MORE USERS** at hello bottom of hello window.</span></span>
> 
> 

- - -
<span data-ttu-id="559b2-122">**Další krok: [nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)**</span><span class="sxs-lookup"><span data-stu-id="559b2-122">**Next: [Upload existing data](machine-learning-walkthrough-2-upload-data.md)**</span></span>

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
