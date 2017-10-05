---
title: "Více geograficky pomůže dokumentace | Microsoft Docs"
description: "Naučte se vytvořit pracovní prostor a publikovat webovou službu v různých z – jih centrální USA (SCUS) oblasti Azure oblast Azure."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: 32f80863308c00c32b1496bb92d39a7ae7d0cc6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="multi-geo-help-documentation"></a><span data-ttu-id="f0a41-103">Dokumentace nápovědy k více geografickým oblastem</span><span class="sxs-lookup"><span data-stu-id="f0a41-103">Multi-Geo Help documentation</span></span>
<span data-ttu-id="f0a41-104">Tento článek popisuje, jak můžete vytvořit pracovní prostor a publikovat webovou službu v různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="f0a41-104">This article describes how you can create a workspace and publish a web service in different Azure regions.</span></span>  <span data-ttu-id="f0a41-105">[Produkty Azure podle oblasti stránky](https://azure.microsoft.com/en-us/regions/services/) obsahuje seznam oblastí, kde je k dispozici Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="f0a41-105">The [Azure Products by Region page](https://azure.microsoft.com/en-us/regions/services/) lists regions where Azure Machine Learning is available.</span></span>

## <a name="create-a-workspace"></a><span data-ttu-id="f0a41-106">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="f0a41-106">Create a workspace</span></span>
1. <span data-ttu-id="f0a41-107">Přihlaste se k portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="f0a41-107">Sign in to the Azure Classic Portal.</span></span>
2. <span data-ttu-id="f0a41-108">Klikněte na tlačítko **+ nový** > **datové služby** > **MACHINE LEARNING** > **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f0a41-108">Click **+NEW** > **DATA SERVICES** > **MACHINE LEARNING** > **QUICK CREATE**.</span></span>  <span data-ttu-id="f0a41-109">V části **umístění** vybrat jinou oblast, například **jihovýchodní Asie**.</span><span class="sxs-lookup"><span data-stu-id="f0a41-109">Under **LOCATION** select another region, such as **Southeast Asia**.</span></span>
   <span data-ttu-id="f0a41-110">![Více geograficky pomůže obrázek 1][1]</span><span class="sxs-lookup"><span data-stu-id="f0a41-110">![Multi-Geo Help image 1][1]</span></span>
3. <span data-ttu-id="f0a41-111">Vyberte pracovní prostor a potom klikněte na **přihlášení k ML Studio**.</span><span class="sxs-lookup"><span data-stu-id="f0a41-111">Select the workspace, and then click **Sign-in to ML Studio**.</span></span>
   <span data-ttu-id="f0a41-112">![Více geograficky pomůže obrázek 2][2]</span><span class="sxs-lookup"><span data-stu-id="f0a41-112">![Multi-Geo Help image 2][2]</span></span>
4. <span data-ttu-id="f0a41-113">Nyní máte pracovního prostoru v jiné oblasti, které můžete používat stejně jako ostatní pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="f0a41-113">You now have a workspace in another region that you may use just like any other workspace.</span></span> <span data-ttu-id="f0a41-114">Přepnout mezi pracovní prostory, podívejte se na pravé horní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f0a41-114">To switch among your workspaces, look to the upper right of your screen.</span></span> <span data-ttu-id="f0a41-115">Klikněte na rozevírací nabídce vyberte oblast a pak vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="f0a41-115">Click the dropdown, select the region, and then select the workspace.</span></span> <span data-ttu-id="f0a41-116">Všechno, co je místní pro oblast pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="f0a41-116">Everything is local to the workspace region.</span></span>  <span data-ttu-id="f0a41-117">Například všechny vaše webové služby vytvořené z jiného pracovního prostoru bude ve stejné oblasti, které je umístěn v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="f0a41-117">For example, all of your web services created from a workspace will be in the same region the workspace is located in.</span></span>
   <span data-ttu-id="f0a41-118">![Více geograficky pomůže image 3][3]</span><span class="sxs-lookup"><span data-stu-id="f0a41-118">![Multi-Geo Help image 3][3]</span></span>

## <a name="open-an-experiment-from-gallery"></a><span data-ttu-id="f0a41-119">Mohli experiment otevřít z Galerie</span><span class="sxs-lookup"><span data-stu-id="f0a41-119">Open an experiment from Gallery</span></span>
<span data-ttu-id="f0a41-120">Pokud jste mohli experiment otevřít z galerie, můžete také vybrat které oblasti, kterou chcete zkopírovat experiment a.</span><span class="sxs-lookup"><span data-stu-id="f0a41-120">If you open an experiment from Gallery, you can also select which region you want to copy the experiment to.</span></span>

![Více geograficky pomůže obrázek 4][4a]

## <a name="web-service-management"></a><span data-ttu-id="f0a41-122">Správa webové služby</span><span class="sxs-lookup"><span data-stu-id="f0a41-122">Web service management</span></span>
<span data-ttu-id="f0a41-123">Pro programovou správu webové služby, například pro retraining, použijte adresu oblast:</span><span class="sxs-lookup"><span data-stu-id="f0a41-123">To programmatically manage web services, such as for retraining, use the region-specific address:</span></span>

* <span data-ttu-id="f0a41-124">https://asiasoutheast.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="f0a41-124">https://asiasoutheast.management.azureml.net</span></span>
* <span data-ttu-id="f0a41-125">https://europewest.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="f0a41-125">https://europewest.management.azureml.net</span></span>

### <a name="things-to-note"></a><span data-ttu-id="f0a41-126">Všimněte</span><span class="sxs-lookup"><span data-stu-id="f0a41-126">Things to note</span></span>
1. <span data-ttu-id="f0a41-127">Kopírovat můžete pouze experimenty mezi pracovní prostory, které patří do stejné oblasti tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="f0a41-127">You can only copy experiments between workspaces that belong to the same region this way.</span></span> <span data-ttu-id="f0a41-128">Pokud je nutné zkopírovat experimentu napříč pracovní prostory v různých oblastech, můžete použít [prostředí PowerShell](http://aka.ms/amlps) příkaz [ *kopie AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) provést tuto akci.</span><span class="sxs-lookup"><span data-stu-id="f0a41-128">If you need to copy experiment across workspaces in different regions, you can use the [PowerShell](http://aka.ms/amlps) commandlet [*Copy-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) to accomplish that.</span></span> <span data-ttu-id="f0a41-129">Další alternativní řešení, je publikovat experimentu do Galerie v seznamu neuvedené režimu, potom ho otevřete v pracovním prostoru z jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="f0a41-129">Another workaround is to publish the experiment into Gallery in unlisted mode, then open it in the workspace from the other region.</span></span>
2. <span data-ttu-id="f0a41-130">Výběr oblasti zobrazí pouze pracovní prostory pro jednu oblast najednou.</span><span class="sxs-lookup"><span data-stu-id="f0a41-130">The region selector will only show workspaces for one region at a time.</span></span>  
3. <span data-ttu-id="f0a41-131">Vytváření a hostované ve Jižní střední USA volného prostoru nebo prostoru přístup hosta (anonymní)</span><span class="sxs-lookup"><span data-stu-id="f0a41-131">A free workspace or Guest Access (anonymous) workspace will be created and hosted in South Central U.S.</span></span>  
4. <span data-ttu-id="f0a41-132">Webové služby z pracovního prostoru v jihovýchodní Asie nasazena se hostují taky v jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="f0a41-132">Web services deployed from a workspace in Southeast Asia will also be hosted in Southeast Asia.</span></span>  

## <a name="more-information"></a><span data-ttu-id="f0a41-133">Další informace</span><span class="sxs-lookup"><span data-stu-id="f0a41-133">More information</span></span>
<span data-ttu-id="f0a41-134">Zeptejte se na [fórum Azure Machine Learning](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span><span class="sxs-lookup"><span data-stu-id="f0a41-134">Ask a question on the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span></span>

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
