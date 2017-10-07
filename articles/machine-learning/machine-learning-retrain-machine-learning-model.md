---
title: "aaaRetrain Model strojového učení | Microsoft Docs"
description: "Zjistěte, jak tooretrain modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
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
ms.openlocfilehash: 342bb9954105339b4b634ff20968a64f4f9f750e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-machine-learning-model"></a><span data-ttu-id="bf203-103">Přeučování strojového učení modelu</span><span class="sxs-lookup"><span data-stu-id="bf203-103">Retrain a Machine Learning Model</span></span>
<span data-ttu-id="bf203-104">Jako součást procesu hello operationalization modelů machine learning v Azure Machine Learning je váš model vycvičena a uložit.</span><span class="sxs-lookup"><span data-stu-id="bf203-104">As part of hello process of operationalization of machine learning models in Azure Machine Learning, your model is trained and saved.</span></span> <span data-ttu-id="bf203-105">Pak použijete ho toocreate predicative webové služby.</span><span class="sxs-lookup"><span data-stu-id="bf203-105">You then use it toocreate a predicative Web service.</span></span> <span data-ttu-id="bf203-106">Hello webové služby, pak mohou být využívány v weby, řídicí panely a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf203-106">hello Web service can then be consumed in web sites, dashboards, and mobile apps.</span></span> 

<span data-ttu-id="bf203-107">Obvykle nejsou statické modely, které vytvoříte pomocí Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="bf203-107">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="bf203-108">Jakmile nová data k dispozici nebo pokud má příjemce hello hello rozhraní API modelu hello svá vlastní data musí toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="bf203-108">As new data becomes available or when hello consumer of hello API has their own data hello model needs toobe retrained.</span></span> 

<span data-ttu-id="bf203-109">Retraining může dojít, často.</span><span class="sxs-lookup"><span data-stu-id="bf203-109">Retraining may occur frequently.</span></span> <span data-ttu-id="bf203-110">S funkcí rozhraní API programové Přeučení hello můžete programově přeučit hello model pomocí hello Retraining API a aktualizace hello webové služby s nově trained model hello.</span><span class="sxs-lookup"><span data-stu-id="bf203-110">With hello Programmatic Retraining API feature, you can programmatically retrain hello model using hello Retraining APIs and update hello Web service with hello newly trained model.</span></span> 

<span data-ttu-id="bf203-111">Tento dokument popisuje hello retraining procesu a ukazuje, jak toouse hello Retraining API.</span><span class="sxs-lookup"><span data-stu-id="bf203-111">This document describes hello retraining process, and shows you how toouse hello Retraining APIs.</span></span>

## <a name="why-retrain-defining-hello-problem"></a><span data-ttu-id="bf203-112">Proč přeučování: definování problému hello</span><span class="sxs-lookup"><span data-stu-id="bf203-112">Why retrain: defining hello problem</span></span>
<span data-ttu-id="bf203-113">Jako součást hello strojové učení cvičení proces cvičení modelu pomocí datové sadě.</span><span class="sxs-lookup"><span data-stu-id="bf203-113">As part of hello machine learning training process, a model is trained using a set of data.</span></span> <span data-ttu-id="bf203-114">Obvykle nejsou statické modely, které vytvoříte pomocí Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="bf203-114">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="bf203-115">Jakmile nová data k dispozici nebo pokud má příjemce hello hello rozhraní API modelu hello svá vlastní data musí toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="bf203-115">As new data becomes available or when hello consumer of hello API has their own data hello model needs toobe retrained.</span></span>

<span data-ttu-id="bf203-116">V těchto scénářích poskytuje programovací rozhraní API tooallow pohodlný způsob jste nebo hello příjemce toocreate vaše rozhraní API klienta, který může na základě jednorázové nebo regulární přeučování hello model pomocí svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="bf203-116">In these scenarios, a programmatic API provides a convenient way tooallow you or hello consumer of your APIs toocreate a client that can, on a one-time or regular basis, retrain hello model using their own data.</span></span> <span data-ttu-id="bf203-117">Potom se můžete vyhodnotit hello výsledky retraining a aktualizovat hello webové rozhraní API toouse hello nově trained model služby.</span><span class="sxs-lookup"><span data-stu-id="bf203-117">They can then evaluate hello results of retraining, and update hello Web service API toouse hello newly trained model.</span></span>

> [!NOTE]
> <span data-ttu-id="bf203-118">Pokud máte existující výukový Experiment a nové webové služby, můžete toocheck out obsloužených existující prediktivní webové služby místo následující návod hello uvedených v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="bf203-118">If you have an existing Training Experiment and New Web service, you may want toocheck out Retrain an existing Predictive Web service instead of following hello walkthrough mentioned in hello following section.</span></span>
> 
> 

## <a name="end-to-end-workflow"></a><span data-ttu-id="bf203-119">Ucelený pracovní postup</span><span class="sxs-lookup"><span data-stu-id="bf203-119">End-to-end workflow</span></span>
<span data-ttu-id="bf203-120">Hello proces zahrnuje hello následující součásti: A výukový Experiment a prediktivní Experiment publikované jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="bf203-120">hello process involves hello following components: A Training Experiment and a Predictive Experiment published as a Web service.</span></span> <span data-ttu-id="bf203-121">tooenable retraining z modulu trained model, hello výukový Experiment musí být publikován jako webovou službu s hello výstup modulu trained model.</span><span class="sxs-lookup"><span data-stu-id="bf203-121">tooenable retraining of a trained model, hello Training Experiment must be published as a Web service with hello output of a trained model.</span></span> <span data-ttu-id="bf203-122">To umožňuje model toohello přístupu rozhraní API pro přeučení.</span><span class="sxs-lookup"><span data-stu-id="bf203-122">This enables API access toohello model for retraining.</span></span> 

<span data-ttu-id="bf203-123">Hello postupem použít tooboth nové a Classic webové služby:</span><span class="sxs-lookup"><span data-stu-id="bf203-123">hello following steps apply tooboth New and Classic Web services:</span></span>

<span data-ttu-id="bf203-124">Vytvoření hello počáteční prediktivní webové služby:</span><span class="sxs-lookup"><span data-stu-id="bf203-124">Create hello initial Predictive Web service:</span></span>

* <span data-ttu-id="bf203-125">Vytvoření experimentu školení</span><span class="sxs-lookup"><span data-stu-id="bf203-125">Create a training experiment</span></span>
* <span data-ttu-id="bf203-126">Vytvoření experimentu prediktivní web</span><span class="sxs-lookup"><span data-stu-id="bf203-126">Create a predictive web experiment</span></span>
* <span data-ttu-id="bf203-127">Nasadit prediktivní webové služby</span><span class="sxs-lookup"><span data-stu-id="bf203-127">Deploy a predictive web service</span></span>

<span data-ttu-id="bf203-128">Přeučování hello webové služby:</span><span class="sxs-lookup"><span data-stu-id="bf203-128">Retrain hello Web service:</span></span>

* <span data-ttu-id="bf203-129">Aktualizovat tooallow experimentu školení pro retraining</span><span class="sxs-lookup"><span data-stu-id="bf203-129">Update training experiment tooallow for retraining</span></span>
* <span data-ttu-id="bf203-130">Nasazení hello retraining webové služby</span><span class="sxs-lookup"><span data-stu-id="bf203-130">Deploy hello retraining web service</span></span>
* <span data-ttu-id="bf203-131">Použít hello služba Batch provádění kódu tooretrain hello model</span><span class="sxs-lookup"><span data-stu-id="bf203-131">Use hello Batch Execution Service code tooretrain hello model</span></span>

<span data-ttu-id="bf203-132">Návod hello předchozích kroků najdete v tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="bf203-132">For a walkthrough of hello preceding steps, see [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="bf203-133">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="bf203-133">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="bf203-134">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="bf203-134">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="bf203-135">Pokud jste nasadili Classic webové služby:</span><span class="sxs-lookup"><span data-stu-id="bf203-135">If you deployed a Classic Web Service:</span></span>

* <span data-ttu-id="bf203-136">Vytvořte nový koncový bod na hello prediktivní webové služby</span><span class="sxs-lookup"><span data-stu-id="bf203-136">Create a new Endpoint on hello Predictive Web service</span></span>
* <span data-ttu-id="bf203-137">Získat adresu URL hello opravy a kódu</span><span class="sxs-lookup"><span data-stu-id="bf203-137">Get hello PATCH URL and code</span></span>
* <span data-ttu-id="bf203-138">Použití hello oprava URL toopoint hello nový koncový bod v hello retrained modelu</span><span class="sxs-lookup"><span data-stu-id="bf203-138">Use hello PATCH URL toopoint hello new Endpoint at hello retrained model</span></span> 

<span data-ttu-id="bf203-139">Návod hello předchozích kroků najdete v tématu [Přeučování Classic webové služby](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="bf203-139">For a walkthrough of hello preceding steps, see [Retrain a Classic Web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="bf203-140">Pokud narazíte na problémy retraining Classic webové služby, přečtěte si téma [řešení potíží s hello retraining Azure Machine Learning Classic webové služby](machine-learning-troubleshooting-retraining-models.md).</span><span class="sxs-lookup"><span data-stu-id="bf203-140">If you run into difficulties retraining a Classic Web service, see [Troubleshooting hello retraining of an Azure Machine Learning Classic Web service](machine-learning-troubleshooting-retraining-models.md).</span></span>

<span data-ttu-id="bf203-141">Pokud jste nasadili do nové webové služby:</span><span class="sxs-lookup"><span data-stu-id="bf203-141">If you deployed a New Web service:</span></span>

* <span data-ttu-id="bf203-142">Přihlaste se tooyour účet Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bf203-142">Sign in tooyour Azure Resource Manager account</span></span>
* <span data-ttu-id="bf203-143">Získat definice hello webové služby</span><span class="sxs-lookup"><span data-stu-id="bf203-143">Get hello Web service definition</span></span>
* <span data-ttu-id="bf203-144">Exportovat hello definice webové služby jako JSON</span><span class="sxs-lookup"><span data-stu-id="bf203-144">Export hello Web Service Definition as JSON</span></span>
* <span data-ttu-id="bf203-145">Aktualizovat hello odkaz toohello `ilearner` objektů blob v hello JSON</span><span class="sxs-lookup"><span data-stu-id="bf203-145">Update hello reference toohello `ilearner` blob in hello JSON</span></span>
* <span data-ttu-id="bf203-146">Importovat hello JSON do definice webové služby</span><span class="sxs-lookup"><span data-stu-id="bf203-146">Import hello JSON into a Web Service Definition</span></span>
* <span data-ttu-id="bf203-147">Aktualizovat hello webové služby s novou definice webové služby</span><span class="sxs-lookup"><span data-stu-id="bf203-147">Update hello Web service with new Web Service Definition</span></span>

<span data-ttu-id="bf203-148">Návod hello předchozích kroků najdete v tématu [Přeučování nové webové službě pomocí rutiny prostředí PowerShell správu Machine Learning hello](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="bf203-148">For a walkthrough of hello preceding steps, see [Retrain a New Web service using hello Machine Learning Management PowerShell cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="bf203-149">proces Hello nastavení retraining Classic webové služby zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bf203-149">hello process for setting up retraining for a Classic Web service involves hello following steps:</span></span>

![Retraining přehled procesu][1]

<span data-ttu-id="bf203-151">proces Hello nastavení pro novou webovou službu retraining zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bf203-151">hello process for setting up retraining for a New Web service involves hello following steps:</span></span>

![Retraining přehled procesu][7]

## <a name="other-resources"></a><span data-ttu-id="bf203-153">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="bf203-153">Other Resources</span></span>
* [<span data-ttu-id="bf203-154">Modely retraining a aktualizaci Azure Machine Learning s Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="bf203-154">Retraining and Updating Azure Machine Learning models with Azure Data Factory</span></span>](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [<span data-ttu-id="bf203-155">Vytvořit mnoho modely Machine Learning a webové koncové body služby z jednoho experimentu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf203-155">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>](machine-learning-create-models-and-endpoints-with-powershell.md)
* <span data-ttu-id="bf203-156">Hello [AML Přeučení modelů pomocí rozhraní API](https://www.youtube.com/watch?v=wwjglA8xllg) video ukazuje, jak vytvořit tooretrain modely Machine Learning v Azure Machine Learning pomocí hello Retraining API a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bf203-156">hello [AML Retraining Models Using APIs](https://www.youtube.com/watch?v=wwjglA8xllg) video shows you how tooretrain Machine Learning models created in Azure Machine Learning using hello Retraining APIs and PowerShell.</span></span>

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

