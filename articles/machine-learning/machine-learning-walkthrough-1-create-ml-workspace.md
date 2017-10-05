---
title: "Krok 1: Vytvoření pracovního prostoru Machine Learning | Microsoft Docs"
description: "Krok 1 vývoj prediktivního řešení návod: Zjistěte, jak nastavit nový pracovní prostor Azure Machine Learning Studio."
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
ms.openlocfilehash: 8ca42ef8f5314866301f5c9e93caa90dc837a66e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a><span data-ttu-id="ac4c1-103">Krok 1 průvodce: Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ac4c1-103">Walkthrough Step 1: Create a Machine Learning workspace</span></span>
<span data-ttu-id="ac4c1-104">Toto je první krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span><span class="sxs-lookup"><span data-stu-id="ac4c1-104">This is the first step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span></span>

1. <span data-ttu-id="ac4c1-105">**Vytvoření pracovního prostoru Machine Learning**</span><span class="sxs-lookup"><span data-stu-id="ac4c1-105">**Create a Machine Learning workspace**</span></span>
2. [<span data-ttu-id="ac4c1-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="ac4c1-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="ac4c1-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="ac4c1-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="ac4c1-108">Natrénování a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="ac4c1-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="ac4c1-109">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="ac4c1-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="ac4c1-110">Přístup k webové službě</span><span class="sxs-lookup"><span data-stu-id="ac4c1-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

<span data-ttu-id="ac4c1-111">Pokud chcete používat Machine Learning Studio, potřebujete pracovní prostor Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-111">To use Machine Learning Studio, you need to have a Microsoft Azure Machine Learning workspace.</span></span> <span data-ttu-id="ac4c1-112">Tento pracovní prostor obsahuje nástroje, které potřebujete k vytváření, správu a publikování experimenty.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-112">This workspace contains the tools you need to create, manage, and publish experiments.</span></span>  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

<span data-ttu-id="ac4c1-113">Správce pro vaše předplatné Azure je potřeba vytvořit pracovní prostor a pak je přidejte jako roli vlastníka nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-113">The administrator for your Azure subscription needs to create the workspace and then add you as an owner or contributor.</span></span> <span data-ttu-id="ac4c1-114">Podrobnosti najdete v tématu [vytvoření a sdílení pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="ac4c1-114">For details, see [Create and share an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

<span data-ttu-id="ac4c1-115">Po vytvoření pracovního prostoru Machine Learning Studio otevřete ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span><span class="sxs-lookup"><span data-stu-id="ac4c1-115">After your workspace is created, open Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span></span> <span data-ttu-id="ac4c1-116">Pokud máte více než jednoho pracovního prostoru, můžete vybrat pracovním prostoru na panelu nástrojů v pravém horním rohu okna.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-116">If you have more than one workspace, you can select the workspace in the toolbar in the upper-right corner of the window.</span></span>

![Vyberte pracovní prostor v Studio][2]

> [!TIP]
> <span data-ttu-id="ac4c1-118">Pokud byly provedeny vlastníka pracovního prostoru, můžete sdílet experimenty, kterou právě pracujete na podle vyzvání jiných uživatelů do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-118">If you were made an owner of the workspace, you can share the experiments you're working on by inviting others to the workspace.</span></span> <span data-ttu-id="ac4c1-119">To provedete v nástroji Machine Learning Studio na **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-119">You can do this in Machine Learning Studio on the **SETTINGS** page.</span></span> <span data-ttu-id="ac4c1-120">Stačí účtu Microsoft nebo účtu organizace pro každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-120">You just need the Microsoft account or organizational account for each user.</span></span>
> 
> <span data-ttu-id="ac4c1-121">Na **nastavení** klikněte na tlačítko **uživatelé**, pak klikněte na tlačítko **POZVAT uživatele více** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="ac4c1-121">On the **SETTINGS** page, click **USERS**, then click **INVITE MORE USERS** at the bottom of the window.</span></span>
> 
> 

- - -
<span data-ttu-id="ac4c1-122">**Další krok: [nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)**</span><span class="sxs-lookup"><span data-stu-id="ac4c1-122">**Next: [Upload existing data](machine-learning-walkthrough-2-upload-data.md)**</span></span>

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
