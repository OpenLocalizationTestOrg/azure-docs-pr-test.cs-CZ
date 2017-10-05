---
title: "Přeučování strojového učení modelu | Microsoft Docs"
description: "Zjistěte, jak aktualizovat webovou službu, která používá nově trénovaného modelu v Azure Machine Learning a přeučování modelu."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: f86c2bc41dd7ff0bc31454a56b84d136dc7d026c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-machine-learning-model"></a><span data-ttu-id="1c997-103">Přeučování strojového učení modelu</span><span class="sxs-lookup"><span data-stu-id="1c997-103">Retrain a Machine Learning Model</span></span>
<span data-ttu-id="1c997-104">Jako součást procesu operationalization modelů machine learning v Azure Machine Learning je váš model vycvičena a uložit.</span><span class="sxs-lookup"><span data-stu-id="1c997-104">As part of the process of operationalization of machine learning models in Azure Machine Learning, your model is trained and saved.</span></span> <span data-ttu-id="1c997-105">Potom ji používáte k vytvoření predicative webové služby.</span><span class="sxs-lookup"><span data-stu-id="1c997-105">You then use it to create a predicative Web service.</span></span> <span data-ttu-id="1c997-106">Webové služby, pak mohou být využívány v weby, řídicí panely a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c997-106">The Web service can then be consumed in web sites, dashboards, and mobile apps.</span></span> 

<span data-ttu-id="1c997-107">Obvykle nejsou statické modely, které vytvoříte pomocí Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1c997-107">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="1c997-108">Jakmile nová data k dispozici, nebo když příjemce rozhraní API má svá vlastní data musí být retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="1c997-108">As new data becomes available or when the consumer of the API has their own data the model needs to be retrained.</span></span> 

<span data-ttu-id="1c997-109">Retraining může dojít, často.</span><span class="sxs-lookup"><span data-stu-id="1c997-109">Retraining may occur frequently.</span></span> <span data-ttu-id="1c997-110">S funkcí programové Přeučení rozhraní API můžete prostřednictvím kódu programu přeučování modelu s použitím rozhraní Retraining API a aktualizovat webovou službu s nově naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="1c997-110">With the Programmatic Retraining API feature, you can programmatically retrain the model using the Retraining APIs and update the Web service with the newly trained model.</span></span> 

<span data-ttu-id="1c997-111">Tento dokument popisuje proces retraining a ukazuje, jak použít i rozhraní Retraining API.</span><span class="sxs-lookup"><span data-stu-id="1c997-111">This document describes the retraining process, and shows you how to use the Retraining APIs.</span></span>

## <a name="why-retrain-defining-the-problem"></a><span data-ttu-id="1c997-112">Proč přeučování: definování problému</span><span class="sxs-lookup"><span data-stu-id="1c997-112">Why retrain: defining the problem</span></span>
<span data-ttu-id="1c997-113">Jako součást strojového učení proces školení cvičení modelu pomocí datové sadě.</span><span class="sxs-lookup"><span data-stu-id="1c997-113">As part of the machine learning training process, a model is trained using a set of data.</span></span> <span data-ttu-id="1c997-114">Obvykle nejsou statické modely, které vytvoříte pomocí Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1c997-114">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="1c997-115">Jakmile nová data k dispozici, nebo když příjemce rozhraní API má svá vlastní data musí být retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="1c997-115">As new data becomes available or when the consumer of the API has their own data the model needs to be retrained.</span></span>

<span data-ttu-id="1c997-116">V těchto scénářích poskytuje programovací rozhraní API pohodlný způsob, jak povolit jste nebo příjemci vašich rozhraní API pro vytvoření klienta, který může na základě jednorázové nebo regulární přeučování modelu s použitím svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="1c997-116">In these scenarios, a programmatic API provides a convenient way to allow you or the consumer of your APIs to create a client that can, on a one-time or regular basis, retrain the model using their own data.</span></span> <span data-ttu-id="1c997-117">Potom se můžete vyhodnotit výsledky retraining a aktualizovat webového rozhraní API služby použít nově trained model.</span><span class="sxs-lookup"><span data-stu-id="1c997-117">They can then evaluate the results of retraining, and update the Web service API to use the newly trained model.</span></span>

> [!NOTE]
> <span data-ttu-id="1c997-118">Pokud máte existující výukový Experiment a nové webové služby, můžete projít obsloužených existující prediktivní webovou službu místo následující postupy uvedené v následující části.</span><span class="sxs-lookup"><span data-stu-id="1c997-118">If you have an existing Training Experiment and New Web service, you may want to check out Retrain an existing Predictive Web service instead of following the walkthrough mentioned in the following section.</span></span>
> 
> 

## <a name="end-to-end-workflow"></a><span data-ttu-id="1c997-119">Komplexní pracovní postup</span><span class="sxs-lookup"><span data-stu-id="1c997-119">End-to-end workflow</span></span>
<span data-ttu-id="1c997-120">Proces zahrnuje následující součásti: A výukový Experiment a prediktivní Experiment publikované jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="1c997-120">The process involves the following components: A Training Experiment and a Predictive Experiment published as a Web service.</span></span> <span data-ttu-id="1c997-121">Povolit retraining z modulu trained model, je nutné výukový Experiment publikovat jako webovou službu s výstup modulu trained model.</span><span class="sxs-lookup"><span data-stu-id="1c997-121">To enable retraining of a trained model, the Training Experiment must be published as a Web service with the output of a trained model.</span></span> <span data-ttu-id="1c997-122">To umožňuje retraining API přístup k modelu.</span><span class="sxs-lookup"><span data-stu-id="1c997-122">This enables API access to the model for retraining.</span></span> 

<span data-ttu-id="1c997-123">Následující postup platí pro nové a Classic webové služby:</span><span class="sxs-lookup"><span data-stu-id="1c997-123">The following steps apply to both New and Classic Web services:</span></span>

<span data-ttu-id="1c997-124">Vytvoření počáteční prediktivní webové služby:</span><span class="sxs-lookup"><span data-stu-id="1c997-124">Create the initial Predictive Web service:</span></span>

* <span data-ttu-id="1c997-125">Vytvoření experimentu školení</span><span class="sxs-lookup"><span data-stu-id="1c997-125">Create a training experiment</span></span>
* <span data-ttu-id="1c997-126">Vytvoření experimentu prediktivní web</span><span class="sxs-lookup"><span data-stu-id="1c997-126">Create a predictive web experiment</span></span>
* <span data-ttu-id="1c997-127">Nasadit prediktivní webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-127">Deploy a predictive web service</span></span>

<span data-ttu-id="1c997-128">Přeučování webové služby:</span><span class="sxs-lookup"><span data-stu-id="1c997-128">Retrain the Web service:</span></span>

* <span data-ttu-id="1c997-129">Aktualizace výukový experiment umožňující retraining</span><span class="sxs-lookup"><span data-stu-id="1c997-129">Update training experiment to allow for retraining</span></span>
* <span data-ttu-id="1c997-130">Nasazení retraining webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-130">Deploy the retraining web service</span></span>
* <span data-ttu-id="1c997-131">Pomocí služby Batch provádění kódu přeučování modelu</span><span class="sxs-lookup"><span data-stu-id="1c997-131">Use the Batch Execution Service code to retrain the model</span></span>

<span data-ttu-id="1c997-132">Návod, podle předchozích kroků, najdete v části [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="1c997-132">For a walkthrough of the preceding steps, see [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="1c997-133">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které, můžete nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="1c997-133">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="1c997-134">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="1c997-134">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="1c997-135">Pokud jste nasadili Classic webové služby:</span><span class="sxs-lookup"><span data-stu-id="1c997-135">If you deployed a Classic Web Service:</span></span>

* <span data-ttu-id="1c997-136">Vytvořte nový koncový bod na prediktivní webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-136">Create a new Endpoint on the Predictive Web service</span></span>
* <span data-ttu-id="1c997-137">Získat adresu PATCH URL a kódu</span><span class="sxs-lookup"><span data-stu-id="1c997-137">Get the PATCH URL and code</span></span>
* <span data-ttu-id="1c997-138">Použijte adresu URL OPRAVIT tak, aby odkazoval nový koncový bod v retrained modelu</span><span class="sxs-lookup"><span data-stu-id="1c997-138">Use the PATCH URL to point the new Endpoint at the retrained model</span></span> 

<span data-ttu-id="1c997-139">Návod, podle předchozích kroků, najdete v části [Přeučování Classic webové služby](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="1c997-139">For a walkthrough of the preceding steps, see [Retrain a Classic Web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="1c997-140">Pokud narazíte na problémy retraining Classic webové služby, přečtěte si téma [řešení potíží s retraining Azure Machine Learning Classic webové služby](machine-learning-troubleshooting-retraining-models.md).</span><span class="sxs-lookup"><span data-stu-id="1c997-140">If you run into difficulties retraining a Classic Web service, see [Troubleshooting the retraining of an Azure Machine Learning Classic Web service](machine-learning-troubleshooting-retraining-models.md).</span></span>

<span data-ttu-id="1c997-141">Pokud jste nasadili do nové webové služby:</span><span class="sxs-lookup"><span data-stu-id="1c997-141">If you deployed a New Web service:</span></span>

* <span data-ttu-id="1c997-142">Přihlaste se k účtu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1c997-142">Sign in to your Azure Resource Manager account</span></span>
* <span data-ttu-id="1c997-143">Získat definice webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-143">Get the Web service definition</span></span>
* <span data-ttu-id="1c997-144">Exportovat jako JSON definice webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-144">Export the Web Service Definition as JSON</span></span>
* <span data-ttu-id="1c997-145">Aktualizovat odkaz na `ilearner` objektu blob ve formátu JSON</span><span class="sxs-lookup"><span data-stu-id="1c997-145">Update the reference to the `ilearner` blob in the JSON</span></span>
* <span data-ttu-id="1c997-146">Import kódu JSON do definice webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-146">Import the JSON into a Web Service Definition</span></span>
* <span data-ttu-id="1c997-147">Aktualizovat webovou službu pomocí nové definice webové služby</span><span class="sxs-lookup"><span data-stu-id="1c997-147">Update the Web service with new Web Service Definition</span></span>

<span data-ttu-id="1c997-148">Návod, podle předchozích kroků, najdete v části [Přeučování nové webové službě pomocí rutin prostředí PowerShell správu Machine Learning](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1c997-148">For a walkthrough of the preceding steps, see [Retrain a New Web service using the Machine Learning Management PowerShell cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="1c997-149">Proces pro nastavení retraining Classic webové služby zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1c997-149">The process for setting up retraining for a Classic Web service involves the following steps:</span></span>

![Retraining přehled procesu][1]

<span data-ttu-id="1c997-151">Proces pro nastavení retraining pro nové webové služby zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1c997-151">The process for setting up retraining for a New Web service involves the following steps:</span></span>

![Retraining přehled procesu][7]

## <a name="other-resources"></a><span data-ttu-id="1c997-153">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="1c997-153">Other Resources</span></span>
* [<span data-ttu-id="1c997-154">Modely retraining a aktualizaci Azure Machine Learning s Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="1c997-154">Retraining and Updating Azure Machine Learning models with Azure Data Factory</span></span>](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [<span data-ttu-id="1c997-155">Vytvořit mnoho modely Machine Learning a webové koncové body služby z jednoho experimentu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1c997-155">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>](machine-learning-create-models-and-endpoints-with-powershell.md)
* <span data-ttu-id="1c997-156">[AML Přeučení modelů pomocí rozhraní API](https://www.youtube.com/watch?v=wwjglA8xllg) video ukazuje, jak přeučování modelů Machine Learning v Azure Machine Learning vytvořit pomocí rozhraní Retraining API a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1c997-156">The [AML Retraining Models Using APIs](https://www.youtube.com/watch?v=wwjglA8xllg) video shows you how to retrain Machine Learning models created in Azure Machine Learning using the Retraining APIs and PowerShell.</span></span>

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

