---
title: "Geograficky aaaMulti nápovědě | Microsoft Docs"
description: "Zjistěte, jak toocreate pracovního prostoru a publikování webové služby v oblasti Azure, liší od hello Jižní střední USA (SCUS) oblasti Azure."
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
ms.openlocfilehash: 77b055950ebfe329131b40e5f0a2f6be1e33c51e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multi-geo-help-documentation"></a><span data-ttu-id="78347-103">Dokumentace nápovědy k více geografickým oblastem</span><span class="sxs-lookup"><span data-stu-id="78347-103">Multi-Geo Help documentation</span></span>
<span data-ttu-id="78347-104">Tento článek popisuje, jak můžete vytvořit pracovní prostor a publikovat webovou službu v různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="78347-104">This article describes how you can create a workspace and publish a web service in different Azure regions.</span></span>  <span data-ttu-id="78347-105">Hello [produkty Azure podle oblasti stránky](https://azure.microsoft.com/en-us/regions/services/) obsahuje seznam oblastí, kde je k dispozici Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="78347-105">hello [Azure Products by Region page](https://azure.microsoft.com/en-us/regions/services/) lists regions where Azure Machine Learning is available.</span></span>

## <a name="create-a-workspace"></a><span data-ttu-id="78347-106">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="78347-106">Create a workspace</span></span>
1. <span data-ttu-id="78347-107">Přihlaste se toohello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="78347-107">Sign in toohello Azure Classic Portal.</span></span>
2. <span data-ttu-id="78347-108">Klikněte na tlačítko **+ nový** > **datové služby** > **MACHINE LEARNING** > **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="78347-108">Click **+NEW** > **DATA SERVICES** > **MACHINE LEARNING** > **QUICK CREATE**.</span></span>  <span data-ttu-id="78347-109">V části **umístění** vybrat jinou oblast, například **jihovýchodní Asie**.</span><span class="sxs-lookup"><span data-stu-id="78347-109">Under **LOCATION** select another region, such as **Southeast Asia**.</span></span>
   <span data-ttu-id="78347-110">![Více geograficky pomůže obrázek 1][1]</span><span class="sxs-lookup"><span data-stu-id="78347-110">![Multi-Geo Help image 1][1]</span></span>
3. <span data-ttu-id="78347-111">Vyberte pracovní prostor hello a pak klikněte na **přihlášení tooML Studio**.</span><span class="sxs-lookup"><span data-stu-id="78347-111">Select hello workspace, and then click **Sign-in tooML Studio**.</span></span>
   <span data-ttu-id="78347-112">![Více geograficky pomůže obrázek 2][2]</span><span class="sxs-lookup"><span data-stu-id="78347-112">![Multi-Geo Help image 2][2]</span></span>
4. <span data-ttu-id="78347-113">Nyní máte pracovního prostoru v jiné oblasti, které můžete používat stejně jako ostatní pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="78347-113">You now have a workspace in another region that you may use just like any other workspace.</span></span> <span data-ttu-id="78347-114">tooswitch mezi pracovní prostory vzhled toohello pravé horní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="78347-114">tooswitch among your workspaces, look toohello upper right of your screen.</span></span> <span data-ttu-id="78347-115">Klikněte na rozevírací hello, vyberte hello oblast a pak vyberte hello prostoru.</span><span class="sxs-lookup"><span data-stu-id="78347-115">Click hello dropdown, select hello region, and then select hello workspace.</span></span> <span data-ttu-id="78347-116">Všechno, co je místní toohello oblast pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="78347-116">Everything is local toohello workspace region.</span></span>  <span data-ttu-id="78347-117">Například všechny vaše webové služby vytvořené z jiného pracovního prostoru bude ve stejné oblasti hello prostoru se nachází v hello.</span><span class="sxs-lookup"><span data-stu-id="78347-117">For example, all of your web services created from a workspace will be in hello same region hello workspace is located in.</span></span>
   <span data-ttu-id="78347-118">![Více geograficky pomůže image 3][3]</span><span class="sxs-lookup"><span data-stu-id="78347-118">![Multi-Geo Help image 3][3]</span></span>

## <a name="open-an-experiment-from-gallery"></a><span data-ttu-id="78347-119">Mohli experiment otevřít z Galerie</span><span class="sxs-lookup"><span data-stu-id="78347-119">Open an experiment from Gallery</span></span>
<span data-ttu-id="78347-120">Pokud jste mohli experiment otevřít z galerie, můžete také vybrat, které oblasti chcete toocopy hello experiment.</span><span class="sxs-lookup"><span data-stu-id="78347-120">If you open an experiment from Gallery, you can also select which region you want toocopy hello experiment to.</span></span>

![Více geograficky pomůže obrázek 4][4a]

## <a name="web-service-management"></a><span data-ttu-id="78347-122">Správa webové služby</span><span class="sxs-lookup"><span data-stu-id="78347-122">Web service management</span></span>
<span data-ttu-id="78347-123">tooprogrammatically Správa webové služby, například pro retraining, použít hello oblast adresu:</span><span class="sxs-lookup"><span data-stu-id="78347-123">tooprogrammatically manage web services, such as for retraining, use hello region-specific address:</span></span>

* <span data-ttu-id="78347-124">https://asiasoutheast.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="78347-124">https://asiasoutheast.management.azureml.net</span></span>
* <span data-ttu-id="78347-125">https://europewest.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="78347-125">https://europewest.management.azureml.net</span></span>

### <a name="things-toonote"></a><span data-ttu-id="78347-126">Toonote věcí</span><span class="sxs-lookup"><span data-stu-id="78347-126">Things toonote</span></span>
1. <span data-ttu-id="78347-127">Kopírovat můžete pouze experimenty mezi pracovní prostory, které patří toohello stejné oblasti tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="78347-127">You can only copy experiments between workspaces that belong toohello same region this way.</span></span> <span data-ttu-id="78347-128">Pokud potřebujete toocopy experimentu napříč pracovní prostory v různých oblastech, můžete použít hello [prostředí PowerShell](http://aka.ms/amlps) příkaz [ *kopie AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish který.</span><span class="sxs-lookup"><span data-stu-id="78347-128">If you need toocopy experiment across workspaces in different regions, you can use hello [PowerShell](http://aka.ms/amlps) commandlet [*Copy-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish that.</span></span> <span data-ttu-id="78347-129">Další alternativní řešení je toopublish hello experimentu do Galerie v seznamu neuvedené režimu a potom ho otevřete v pracovním prostoru hello z hello jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="78347-129">Another workaround is toopublish hello experiment into Gallery in unlisted mode, then open it in hello workspace from hello other region.</span></span>
2. <span data-ttu-id="78347-130">selektor oblast Hello zobrazí pouze pracovní prostory pro jednu oblast v čase.</span><span class="sxs-lookup"><span data-stu-id="78347-130">hello region selector will only show workspaces for one region at a time.</span></span>  
3. <span data-ttu-id="78347-131">Vytváření a hostované ve Jižní střední USA volného prostoru nebo prostoru přístup hosta (anonymní)</span><span class="sxs-lookup"><span data-stu-id="78347-131">A free workspace or Guest Access (anonymous) workspace will be created and hosted in South Central U.S.</span></span>  
4. <span data-ttu-id="78347-132">Webové služby z pracovního prostoru v jihovýchodní Asie nasazena se hostují taky v jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="78347-132">Web services deployed from a workspace in Southeast Asia will also be hosted in Southeast Asia.</span></span>  

## <a name="more-information"></a><span data-ttu-id="78347-133">Další informace</span><span class="sxs-lookup"><span data-stu-id="78347-133">More information</span></span>
<span data-ttu-id="78347-134">Zeptejte se na hello [fórum Azure Machine Learning](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span><span class="sxs-lookup"><span data-stu-id="78347-134">Ask a question on hello [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span></span>

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
