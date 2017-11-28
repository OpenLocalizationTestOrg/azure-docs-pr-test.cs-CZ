---
title: "modelů aaaRetrain Machine Learning | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically přeučování modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: edbb64c08f7d9edf3c76e23e0cc7e14c0125d697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a><span data-ttu-id="05fc7-103">Programové přeučení modelů Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="05fc7-103">Retrain Machine Learning models programmatically</span></span>
<span data-ttu-id="05fc7-104">V tomto návodu se dozvíte, jak tooprogrammatically přeučování Azure Machine Learning webové služby pomocí jazyka C# a hello Služba Machine Learning Batch Execution.</span><span class="sxs-lookup"><span data-stu-id="05fc7-104">In this walkthrough, you will learn how tooprogrammatically retrain an Azure Machine Learning Web Service using C# and hello Machine Learning Batch Execution service.</span></span>

<span data-ttu-id="05fc7-105">Jakmile mají retrained hello modelu, hello následující postupy ukazují, jak tooupdate hello model prediktivní webové služby:</span><span class="sxs-lookup"><span data-stu-id="05fc7-105">Once you have retrained hello model, hello following walkthroughs show how tooupdate hello model in your predictive web service:</span></span>

* <span data-ttu-id="05fc7-106">Pokud jste nasadili hello webové služby Machine Learning portálu Classic webové služby, přečtěte si téma [Přeučování webové služby Classic](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-106">If you deployed a Classic web service in hello Machine Learning Web Services portal, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span> 
* <span data-ttu-id="05fc7-107">Pokud jste nasadili novou webovou službu, najdete v části [Přeučování novou webovou službu pomocí rutin Machine Learning Management hello](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-107">If you deployed a New web service, see [Retrain a New web service using hello Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="05fc7-108">Přehled hello retraining procesu najdete v tématu [Přeučování Model strojového učení](machine-learning-retrain-machine-learning-model.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-108">For an overview of hello retraining process, see [Retrain a Machine Learning Model](machine-learning-retrain-machine-learning-model.md).</span></span>

<span data-ttu-id="05fc7-109">Pokud chcete, aby se stávající toostart nové Azure Resource Manager na základě webové služby, najdete v části [Přeučování existující prediktivní webovou službu](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-109">If you want toostart with your existing New Azure Resource Manager based web service, see [Retrain an existing Predictive web service](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span></span>

## <a name="create-a-training-experiment"></a><span data-ttu-id="05fc7-110">Vytvoření experimentu školení</span><span class="sxs-lookup"><span data-stu-id="05fc7-110">Create a training experiment</span></span>
<span data-ttu-id="05fc7-111">V tomto příkladu budete používat "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" z ukázky Microsoft Azure Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-111">For this example, you will use "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" from hello Microsoft Azure Machine Learning samples.</span></span> 

<span data-ttu-id="05fc7-112">toocreate hello experiment:</span><span class="sxs-lookup"><span data-stu-id="05fc7-112">toocreate hello experiment:</span></span>

1. <span data-ttu-id="05fc7-113">Přihlaste se k tooMicrosoft Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="05fc7-113">Sign into tooMicrosoft Azure Machine Learning Studio.</span></span> 
2. <span data-ttu-id="05fc7-114">Na hello pravém dolním rohu hello řídicí panel, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-114">On hello bottom right corner of hello dashboard, click **New**.</span></span>
3. <span data-ttu-id="05fc7-115">Hello Microsoft Samples vyberte ukázkový 5.</span><span class="sxs-lookup"><span data-stu-id="05fc7-115">From hello Microsoft Samples, select Sample 5.</span></span>
4. <span data-ttu-id="05fc7-116">experiment hello toorename hello horní části plátna experimentu hello, vyberte název experimentu hello "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu".</span><span class="sxs-lookup"><span data-stu-id="05fc7-116">toorename hello experiment, at hello top of hello experiment canvas, select hello experiment name "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset".</span></span>
5. <span data-ttu-id="05fc7-117">Typ modelu úplné zjišťování.</span><span class="sxs-lookup"><span data-stu-id="05fc7-117">Type Census Model.</span></span>
6. <span data-ttu-id="05fc7-118">Hello dolní části plátna experimentu hello, klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-118">At hello bottom of hello experiment canvas, click **Run**.</span></span>
7. <span data-ttu-id="05fc7-119">Klikněte na tlačítko **nastavit webové služby** a vyberte **Retraining webové služby**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-119">Click **Set Up web service** and select **Retraining web service**.</span></span> 

<span data-ttu-id="05fc7-120">Následující Hello ukazuje počáteční experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-120">hello following shows hello initial experiment.</span></span>
   
   ![Počáteční experimentu.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a><span data-ttu-id="05fc7-122">Vytvořit prediktivní experiment a publikovat jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="05fc7-122">Create a predictive experiment and publish as a web service</span></span>
<span data-ttu-id="05fc7-123">Dále vytvoříte Predicative experimentu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-123">Next you create a Predicative Experiment.</span></span>

1. <span data-ttu-id="05fc7-124">Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **webové služby prediktivní**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-124">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Predictive Web Service**.</span></span> <span data-ttu-id="05fc7-125">To uloží hello model jako modulu Trained Model a přidá modulů vstup a výstup webové služby.</span><span class="sxs-lookup"><span data-stu-id="05fc7-125">This saves hello model as a Trained Model and adds web service Input and Output modules.</span></span> 
2. <span data-ttu-id="05fc7-126">Klikněte na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="05fc7-126">Click **Run**.</span></span> 
3. <span data-ttu-id="05fc7-127">Po dokončení běhu hello experimentu klikněte na tlačítko **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-127">After hello experiment has finished running, click **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span>

> [!NOTE] 
> <span data-ttu-id="05fc7-128">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="05fc7-128">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="05fc7-129">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-129">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

## <a name="deploy-hello-training-experiment-as-a-training-web-service"></a><span data-ttu-id="05fc7-130">Hello výukový experiment nasadit jako webovou službu školení</span><span class="sxs-lookup"><span data-stu-id="05fc7-130">Deploy hello training experiment as a Training web service</span></span>
<span data-ttu-id="05fc7-131">tooretrain hello trained model, je nutné nasadit hello výukový experiment, kterou jste vytvořili jako Retraining webovou službu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-131">tooretrain hello trained model, you must deploy hello training experiment that you created as a Retraining web service.</span></span> <span data-ttu-id="05fc7-132">Potřebuje této webové služby *výstup webové služby* modulu připojené toohello  *[Train Model] [ train-model]*  modulu, toobe možné tooproduce nové trénované modely.</span><span class="sxs-lookup"><span data-stu-id="05fc7-132">This web service needs a *Web Service Output* module connected toohello *[Train Model][train-model]* module, toobe able tooproduce new trained models.</span></span>

1. <span data-ttu-id="05fc7-133">tooreturn toohello výukový experiment, klikněte v levém podokně hello hello experimenty ikonu a pak klikněte na hello experimentu s názvem úplné zjišťování modelu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-133">tooreturn toohello training experiment, click hello Experiments icon in hello left pane, then click hello experiment named Census Model.</span></span>  
2. <span data-ttu-id="05fc7-134">Hello položek experimentu hledání vyhledávacího pole zadejte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-134">In hello Search Experiment Items search box, type web service.</span></span> 
3. <span data-ttu-id="05fc7-135">Přetáhněte *vstup webové služby* modulu do hello experimentovat plátno a připojte jeho výstup toohello *vyčištění chybějících dat* modulu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-135">Drag a *Web Service Input* module onto hello experiment canvas and connect its output toohello *Clean Missing Data* module.</span></span>  <span data-ttu-id="05fc7-136">To zajistí, že je zpracován retraining data hello stejný způsob jako původní data školení.</span><span class="sxs-lookup"><span data-stu-id="05fc7-136">This ensures that your retraining data is processed hello same way as your original training data.</span></span>
4. <span data-ttu-id="05fc7-137">Přetáhněte dva *webovou službu výstup* modulů do hello experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="05fc7-137">Drag two *web service Output* modules onto hello experiment canvas.</span></span> <span data-ttu-id="05fc7-138">Připojit hello výstup hello *Train Model* modulu tooone a hello výstup hello *Evaluate Model* modulu toohello jiné.</span><span class="sxs-lookup"><span data-stu-id="05fc7-138">Connect hello output of hello *Train Model* module tooone and hello output of hello *Evaluate Model* module toohello other.</span></span> <span data-ttu-id="05fc7-139">Hello výstup webové služby pro **Train Model** nám dává hello nový trained model.</span><span class="sxs-lookup"><span data-stu-id="05fc7-139">hello web service output for **Train Model** gives us hello new trained model.</span></span> <span data-ttu-id="05fc7-140">Hello výstup připojené příliš**Evaluate Model** vrátí výstupu Tenhle modul, který je hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="05fc7-140">hello output attached too**Evaluate Model** returns that module’s output, which is hello performance results.</span></span>
5. <span data-ttu-id="05fc7-141">Klikněte na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="05fc7-141">Click **Run**.</span></span> 

<span data-ttu-id="05fc7-142">Dále je nutné nasadit hello výukový experiment jako webová služba, která vytváří modulu trained model a výsledky vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-142">Next you must deploy hello training experiment as a web service that produces a trained model and model evaluation results.</span></span> <span data-ttu-id="05fc7-143">tooaccomplish to vaše další sadu akcí jsou závislé na tom, jestli pracujete s webovou službu Classic nebo novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-143">tooaccomplish this, your next set of actions are dependent on whether you are working with a Classic web service or a New web service.</span></span>  

<span data-ttu-id="05fc7-144">**Classic webové služby**</span><span class="sxs-lookup"><span data-stu-id="05fc7-144">**Classic web service**</span></span>

<span data-ttu-id="05fc7-145">Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **nasazení webové služby [Classic]**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-145">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="05fc7-146">Hello webové služby **řídicí panel** se zobrazí se stránka nápovědy hello klíč rozhraní API a hello rozhraní API pro dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="05fc7-146">hello Web Service **Dashboard** is displayed with hello API Key and hello API help page for Batch Execution.</span></span> <span data-ttu-id="05fc7-147">Pro vytvoření Trénované modely lze použít pouze hello Batch Execution metoda.</span><span class="sxs-lookup"><span data-stu-id="05fc7-147">Only hello Batch Execution method can be used for creating Trained Models.</span></span>

<span data-ttu-id="05fc7-148">**Novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="05fc7-148">**New web service**</span></span>

<span data-ttu-id="05fc7-149">Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-149">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="05fc7-150">Hello webové služby Azure Machine Learning webové služby portál otevře stránku toohello nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="05fc7-150">hello Web Service Azure Machine Learning Web Services portal opens toohello Deploy web service page.</span></span> <span data-ttu-id="05fc7-151">Zadejte název pro webovou službu a zvolte plán platby a potom klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-151">Type a name for your web service and choose a payment plan, then click **Deploy**.</span></span> <span data-ttu-id="05fc7-152">Pro vytvoření Trénované modely lze použít pouze hello Batch Execution – metoda</span><span class="sxs-lookup"><span data-stu-id="05fc7-152">Only hello Batch Execution method can be used for creating Trained Models</span></span>

<span data-ttu-id="05fc7-153">V obou případech po experiment má dokončené spuštěná hello výsledné pracovního postupu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="05fc7-153">In either case, after experiment has completed running, hello resulting workflow should look as follows:</span></span>

![Po spuštění výsledného workflow.][4]



## <a name="retrain-hello-model-with-new-data-using-bes"></a><span data-ttu-id="05fc7-155">Přeučování hello modelu s použitím BES nová data</span><span class="sxs-lookup"><span data-stu-id="05fc7-155">Retrain hello model with new data using BES</span></span>
<span data-ttu-id="05fc7-156">V tomto příkladu použijete toocreate hello C# retraining aplikace.</span><span class="sxs-lookup"><span data-stu-id="05fc7-156">For this example, you are using C# toocreate hello retraining application.</span></span> <span data-ttu-id="05fc7-157">Můžete taky hello Python nebo R ukázkový kód tooaccomplish této úlohy.</span><span class="sxs-lookup"><span data-stu-id="05fc7-157">You can also use hello Python or R sample code tooaccomplish this task.</span></span>

<span data-ttu-id="05fc7-158">toocall hello Retraining API:</span><span class="sxs-lookup"><span data-stu-id="05fc7-158">toocall hello Retraining APIs:</span></span>

1. <span data-ttu-id="05fc7-159">Vytvořte konzolovou aplikaci C# v sadě Visual Studio: **nový** > **projektu** > **Visual C#** > **Windows Classic Desktop** > **konzolovou aplikaci (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-159">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="05fc7-160">Přihlaste se toohello portál webová služba Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="05fc7-160">Sign in toohello Machine Learning Web Service portal.</span></span>
3. <span data-ttu-id="05fc7-161">Pokud pracujete s webovou službou Classic, klikněte na tlačítko **Classic webové služby**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-161">If you are working with a Classic web service, click **Classic Web Services**.</span></span>
   1. <span data-ttu-id="05fc7-162">Klikněte na tlačítko hello webové služby, kterou pracujete.</span><span class="sxs-lookup"><span data-stu-id="05fc7-162">Click hello web service you are working with.</span></span>
   2. <span data-ttu-id="05fc7-163">Klikněte na tlačítko hello výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="05fc7-163">Click hello default endpoint.</span></span>
   3. <span data-ttu-id="05fc7-164">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-164">Click **Consume**.</span></span>
   4. <span data-ttu-id="05fc7-165">Na konci hello hello **spotřebě** stránku hello **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-165">At hello bottom of hello **Consume** page, in hello **Sample Code** section, click **Batch**.</span></span>
   5. <span data-ttu-id="05fc7-166">Pokračujte toostep 5 tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-166">Continue toostep 5 of this procedure.</span></span>
4. <span data-ttu-id="05fc7-167">Pokud pracujete s novou webovou službu, klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-167">If you are working with a New web service, click **Web Services**.</span></span>
   1. <span data-ttu-id="05fc7-168">Klikněte na tlačítko hello webové služby, kterou pracujete.</span><span class="sxs-lookup"><span data-stu-id="05fc7-168">Click hello web service you are working with.</span></span>
   2. <span data-ttu-id="05fc7-169">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-169">Click **Consume**.</span></span>
   3. <span data-ttu-id="05fc7-170">V dolní části hello hello spotřebě stránky, v hello **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-170">At hello bottom of hello Consume page, in hello **Sample Code** section, click **Batch**.</span></span>
5. <span data-ttu-id="05fc7-171">Zkopírujte hello ukázkový kód C# pro spuštění dávky a vložte jej do souboru Program.cs hello, ujistěte se, že obor názvů hello zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="05fc7-171">Copy hello sample C# code for batch execution and paste it into hello Program.cs file, making sure hello namespace remains intact.</span></span>

<span data-ttu-id="05fc7-172">Jak je uvedeno v komentářích hello přidáte hello balíček Nuget Microsoft.AspNet.WebApi.Client.</span><span class="sxs-lookup"><span data-stu-id="05fc7-172">Add hello Nuget package Microsoft.AspNet.WebApi.Client as specified in hello comments.</span></span> <span data-ttu-id="05fc7-173">tooadd hello odkaz tooMicrosoft.WindowsAzure.Storage.dll, bude pravděpodobně nutné nejprve tooinstall hello Klientská knihovna pro úložiště služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="05fc7-173">tooadd hello reference tooMicrosoft.WindowsAzure.Storage.dll, you might first need tooinstall hello client library for Microsoft Azure storage services.</span></span> <span data-ttu-id="05fc7-174">Další informace najdete v tématu [služby úložiště systému Windows](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="05fc7-174">For more information, see [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

### <a name="update-hello-apikey-declaration"></a><span data-ttu-id="05fc7-175">Aktualizovat hello apikey deklarace</span><span class="sxs-lookup"><span data-stu-id="05fc7-175">Update hello apikey declaration</span></span>
<span data-ttu-id="05fc7-176">Vyhledejte hello **apikey** deklarace.</span><span class="sxs-lookup"><span data-stu-id="05fc7-176">Locate hello **apikey** declaration.</span></span>

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

<span data-ttu-id="05fc7-177">V hello **informace o základní spotřeby** části hello **spotřebě** stránky, vyhledejte hello primární klíč a zkopírujte jej toohello **apikey** deklarace.</span><span class="sxs-lookup"><span data-stu-id="05fc7-177">In hello **Basic consumption info** section of hello **Consume** page, locate hello primary key and copy it toohello **apikey** declaration.</span></span>

### <a name="update-hello-azure-storage-information"></a><span data-ttu-id="05fc7-178">Aktualizace informací o úložišti Azure hello</span><span class="sxs-lookup"><span data-stu-id="05fc7-178">Update hello Azure Storage information</span></span>
<span data-ttu-id="05fc7-179">Hello BES ukázkový kód se uloží soubor z místního disku (například "C:\temp\CensusIpnput.csv") tooAzure úložiště, procesy a zapíše zpět tooAzure výsledky hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="05fc7-179">hello BES sample code uploads a file from a local drive (For example "C:\temp\CensusIpnput.csv") tooAzure Storage, processes it, and writes hello results back tooAzure Storage.</span></span>  

<span data-ttu-id="05fc7-180">tooaccomplish tato úloha musí načíst hello název, klíč a kontejner informace o účtu úložiště pro váš účet úložiště z portálu Azure classic hello a aktualizace hello odpovídající hodnoty v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-180">tooaccomplish this task, you must retrieve hello Storage account name, key, and container information for your Storage account from hello classic Azure portal and hello update corresponding values in hello code.</span></span> 

1. <span data-ttu-id="05fc7-181">Přihlaste se na portálu Azure classic toohello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-181">Sign in toohello classic Azure portal.</span></span>
2. <span data-ttu-id="05fc7-182">V levém navigačním sloupci hello, klikněte na tlačítko **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-182">In hello left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="05fc7-183">Vyberte jeden toostore hello hello seznamu účtů úložiště retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-183">From hello list of storage accounts, select one toostore hello retrained model.</span></span>
4. <span data-ttu-id="05fc7-184">V dolní části hello hello stránky, klikněte na tlačítko **spravovat přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-184">At hello bottom of hello page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="05fc7-185">Zkopírujte a uložte hello **primární přístupový klíč** a dialogové okno zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-185">Copy and save hello **Primary Access Key** and close hello dialog.</span></span> 
6. <span data-ttu-id="05fc7-186">V horní části hello hello stránky, klikněte na tlačítko **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="05fc7-186">At hello top of hello page, click **Containers**.</span></span>
7. <span data-ttu-id="05fc7-187">Vyberte existující kontejner nebo vytvořte novou a uložte hello název.</span><span class="sxs-lookup"><span data-stu-id="05fc7-187">Select an existing container or create a new one and save hello name.</span></span>

<span data-ttu-id="05fc7-188">Vyhledejte hello *StorageAccountName*, *StorageAccountKey*, a *StorageContainerName* deklarace a aktualizace hodnoty hello jste uložili z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="05fc7-188">Locate hello *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations and update hello values you saved from hello Azure portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

<span data-ttu-id="05fc7-189">Můžete také zajistit, aby byl hello vstupní soubor k dispozici v hello umístění, které zadáte v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-189">You also must ensure hello input file is available at hello location you specify in hello code.</span></span> 

### <a name="specify-hello-output-location"></a><span data-ttu-id="05fc7-190">Zadejte umístění výstupu hello</span><span class="sxs-lookup"><span data-stu-id="05fc7-190">Specify hello output location</span></span>
<span data-ttu-id="05fc7-191">Při zadávání hello výstupní umístění do hello datová část požadavku, hello rozšíření hello soubor zadaný v *RelativeLocation* musí být zadány jako ilearner.</span><span class="sxs-lookup"><span data-stu-id="05fc7-191">When specifying hello output location in hello Request Payload, hello extension of hello file specified in *RelativeLocation* must be specified as ilearner.</span></span> 

<span data-ttu-id="05fc7-192">Viz následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="05fc7-192">See hello following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you would like toouse for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> <span data-ttu-id="05fc7-193">názvy Hello výstupních umístění se může lišit od těch, které jsou v tomto návodu podle hello pořadí, ve které jste přidali modulů hello webové služby výstup hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-193">hello names of your output locations may be different from hello ones in this walkthrough based on hello order in which you added hello web service output modules.</span></span> <span data-ttu-id="05fc7-194">Vzhledem k tomu, že jste nastavili tento výukový experiment s dvěma výstupy, hello výsledky obsahovat informace o umístění úložiště pro oba dva.</span><span class="sxs-lookup"><span data-stu-id="05fc7-194">Since you set up this training experiment with two outputs, hello results include storage location information for both of them.</span></span>  
> 
> 

![Retraining výstup][6]

<span data-ttu-id="05fc7-196">Obrázek 4: Retraining výstup.</span><span class="sxs-lookup"><span data-stu-id="05fc7-196">Diagram 4: Retraining output.</span></span>

## <a name="evaluate-hello-retraining-results"></a><span data-ttu-id="05fc7-197">Vyhodnoťte výsledky Retraining hello</span><span class="sxs-lookup"><span data-stu-id="05fc7-197">Evaluate hello Retraining Results</span></span>
<span data-ttu-id="05fc7-198">Při spuštění aplikace hello výstup hello obsahuje adresu URL hello a SAS token nezbytné tooaccess hello výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="05fc7-198">When you run hello application, hello output includes hello URL and SAS token necessary tooaccess hello evaluation results.</span></span>

<span data-ttu-id="05fc7-199">Zobrazí výsledky výkonu hello hello retrained modelu kombinací hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello pro *output2* (jak je uvedeno v předchozí retraining image výstup hello) a vkládání hello úplnou adresu URL v adresním řádku prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="05fc7-199">You can see hello performance results of hello retrained model by combining hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results for *output2* (as shown in hello preceding retraining output image) and pasting hello complete URL in hello browser address bar.</span></span>  

<span data-ttu-id="05fc7-200">Hello výsledky toodetermine zkontrolujte, zda nově trained model hello provede dobře dostatek tooreplace hello existující jeden.</span><span class="sxs-lookup"><span data-stu-id="05fc7-200">Examine hello results toodetermine whether hello newly trained model performs well enough tooreplace hello existing one.</span></span>

<span data-ttu-id="05fc7-201">Kopírování hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello, použijeme je během hello retraining procesu.</span><span class="sxs-lookup"><span data-stu-id="05fc7-201">Copy hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results, you will use them during hello retraining process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05fc7-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05fc7-202">Next steps</span></span>
<span data-ttu-id="05fc7-203">Pokud jste nasadili hello prediktivní webové služby kliknutím **nasazení webové služby [Classic]**, najdete v části [Přeučování webové služby Classic](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-203">If you deployed hello predictive web service by clicking **Deploy Web Service [Classic]**, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="05fc7-204">Pokud jste nasadili hello prediktivní webová služba kliknutím **nasazení [nové] webové služby**, najdete v části [Přeučování novou webovou službu pomocí rutin Machine Learning Management hello](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="05fc7-204">If you deployed hello predictive web service by clicking **Deploy Web Service [New]**, see [Retrain a New web service using hello Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<!-- Retrain a New web service using hello Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
