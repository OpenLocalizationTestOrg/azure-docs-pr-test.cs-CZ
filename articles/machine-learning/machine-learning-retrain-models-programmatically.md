---
title: "Programově přeučit modely Machine Learning | Microsoft Docs"
description: "Zjistěte, jak programově přeučit modelu a aktualizovat webovou službu, která používá nově trénovaného modelu v Azure Machine Learning."
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
ms.openlocfilehash: cf7a39e14a935d0d0e0df07e66a8f37480ec9687
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a><span data-ttu-id="481d4-103">Programové přeučení modelů Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="481d4-103">Retrain Machine Learning models programmatically</span></span>
<span data-ttu-id="481d4-104">V tomto návodu se dozvíte, jak programově přeučit Azure Machine Learning webové služby pomocí jazyka C# a Služba Machine Learning Batch Execution.</span><span class="sxs-lookup"><span data-stu-id="481d4-104">In this walkthrough, you will learn how to programmatically retrain an Azure Machine Learning Web Service using C# and the Machine Learning Batch Execution service.</span></span>

<span data-ttu-id="481d4-105">Jakmile mají retrained modelu, následující postupy ukazují, jak aktualizovat model prediktivní webové služby:</span><span class="sxs-lookup"><span data-stu-id="481d4-105">Once you have retrained the model, the following walkthroughs show how to update the model in your predictive web service:</span></span>

* <span data-ttu-id="481d4-106">Pokud jste nasadili Classic webovou službu na portálu webové služby Machine Learning, přečtěte si téma [Přeučování webové služby Classic](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-106">If you deployed a Classic web service in the Machine Learning Web Services portal, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span> 
* <span data-ttu-id="481d4-107">Pokud jste nasadili novou webovou službu, najdete v části [Přeučování novou webovou službu pomocí rutin Machine Learning Management](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-107">If you deployed a New web service, see [Retrain a New web service using the Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="481d4-108">Přehled procesu retraining najdete v tématu [Přeučování Model strojového učení](machine-learning-retrain-machine-learning-model.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-108">For an overview of the retraining process, see [Retrain a Machine Learning Model](machine-learning-retrain-machine-learning-model.md).</span></span>

<span data-ttu-id="481d4-109">Pokud chcete spustit s existující webovou službu na základě nové Azure Resource Manager, přečtěte si téma [Přeučování existující prediktivní webovou službu](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-109">If you want to start with your existing New Azure Resource Manager based web service, see [Retrain an existing Predictive web service](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span></span>

## <a name="create-a-training-experiment"></a><span data-ttu-id="481d4-110">Vytvoření experimentu školení</span><span class="sxs-lookup"><span data-stu-id="481d4-110">Create a training experiment</span></span>
<span data-ttu-id="481d4-111">V tomto příkladu budete používat "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" z ukázky Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="481d4-111">For this example, you will use "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" from the Microsoft Azure Machine Learning samples.</span></span> 

<span data-ttu-id="481d4-112">Vytvoření experimentu:</span><span class="sxs-lookup"><span data-stu-id="481d4-112">To create the experiment:</span></span>

1. <span data-ttu-id="481d4-113">Přihlaste se k platformě Microsoft Azure strojového učení Studio.</span><span class="sxs-lookup"><span data-stu-id="481d4-113">Sign into to Microsoft Azure Machine Learning Studio.</span></span> 
2. <span data-ttu-id="481d4-114">V pravém horním rohu obrazovky řídicí panel, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="481d4-114">On the bottom right corner of the dashboard, click **New**.</span></span>
3. <span data-ttu-id="481d4-115">Microsoft Samples vyberte ukázkový 5.</span><span class="sxs-lookup"><span data-stu-id="481d4-115">From the Microsoft Samples, select Sample 5.</span></span>
4. <span data-ttu-id="481d4-116">Přejmenování experimentu v horní části plátna experimentu, vyberte název experimentu "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu".</span><span class="sxs-lookup"><span data-stu-id="481d4-116">To rename the experiment, at the top of the experiment canvas, select the experiment name "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset".</span></span>
5. <span data-ttu-id="481d4-117">Typ modelu úplné zjišťování.</span><span class="sxs-lookup"><span data-stu-id="481d4-117">Type Census Model.</span></span>
6. <span data-ttu-id="481d4-118">V dolní části plátna experimentu, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="481d4-118">At the bottom of the experiment canvas, click **Run**.</span></span>
7. <span data-ttu-id="481d4-119">Klikněte na tlačítko **nastavit webové služby** a vyberte **Retraining webové služby**.</span><span class="sxs-lookup"><span data-stu-id="481d4-119">Click **Set Up web service** and select **Retraining web service**.</span></span> 

<span data-ttu-id="481d4-120">Následující obrázek znázorňuje počáteční experimentu.</span><span class="sxs-lookup"><span data-stu-id="481d4-120">The following shows the initial experiment.</span></span>
   
   ![Počáteční experimentu.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a><span data-ttu-id="481d4-122">Vytvořit prediktivní experiment a publikovat jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="481d4-122">Create a predictive experiment and publish as a web service</span></span>
<span data-ttu-id="481d4-123">Dále vytvoříte Predicative experimentu.</span><span class="sxs-lookup"><span data-stu-id="481d4-123">Next you create a Predicative Experiment.</span></span>

1. <span data-ttu-id="481d4-124">V dolní části plátna experimentu, klikněte na tlačítko **nastavit webové služby** a vyberte **webové služby prediktivní**.</span><span class="sxs-lookup"><span data-stu-id="481d4-124">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Predictive Web Service**.</span></span> <span data-ttu-id="481d4-125">To uloží model jako modulu Trained Model a přidá modulů vstup a výstup webové služby.</span><span class="sxs-lookup"><span data-stu-id="481d4-125">This saves the model as a Trained Model and adds web service Input and Output modules.</span></span> 
2. <span data-ttu-id="481d4-126">Klikněte na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="481d4-126">Click **Run**.</span></span> 
3. <span data-ttu-id="481d4-127">Po dokončení běhu experimentu klikněte na tlačítko **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="481d4-127">After the experiment has finished running, click **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span>

> [!NOTE] 
> <span data-ttu-id="481d4-128">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které, můžete nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="481d4-128">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="481d4-129">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-129">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a><span data-ttu-id="481d4-130">Výukový experiment nasadit jako webovou službu školení</span><span class="sxs-lookup"><span data-stu-id="481d4-130">Deploy the training experiment as a Training web service</span></span>
<span data-ttu-id="481d4-131">Chcete-li přeučování trénovaného modelu, musíte je nasadit výukový experiment, kterou jste vytvořili jako webovou službu Retraining.</span><span class="sxs-lookup"><span data-stu-id="481d4-131">To retrain the trained model, you must deploy the training experiment that you created as a Retraining web service.</span></span> <span data-ttu-id="481d4-132">Potřebuje této webové služby *výstup webové služby* připojené k modulu  *[Train Model] [ train-model]*  modul, abyste mohli vytvořit nový trénované modely.</span><span class="sxs-lookup"><span data-stu-id="481d4-132">This web service needs a *Web Service Output* module connected to the *[Train Model][train-model]* module, to be able to produce new trained models.</span></span>

1. <span data-ttu-id="481d4-133">Pokud chcete vrátit do výukový experiment, klikněte na ikonu experimenty v levém podokně a potom klikněte na tlačítko s názvem úplné zjišťování modelu experimentu.</span><span class="sxs-lookup"><span data-stu-id="481d4-133">To return to the training experiment, click the Experiments icon in the left pane, then click the experiment named Census Model.</span></span>  
2. <span data-ttu-id="481d4-134">Do vyhledávacího pole položek experimentu hledání zadejte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="481d4-134">In the Search Experiment Items search box, type web service.</span></span> 
3. <span data-ttu-id="481d4-135">Přetáhněte *vstup webové služby* plátno modul do experimentu a propojte svůj výstup do *vyčištění chybějících dat* modulu.</span><span class="sxs-lookup"><span data-stu-id="481d4-135">Drag a *Web Service Input* module onto the experiment canvas and connect its output to the *Clean Missing Data* module.</span></span>  <span data-ttu-id="481d4-136">Tím se zajistí, že retraining dat je zpracován stejným způsobem jako původní data školení.</span><span class="sxs-lookup"><span data-stu-id="481d4-136">This ensures that your retraining data is processed the same way as your original training data.</span></span>
4. <span data-ttu-id="481d4-137">Přetáhněte dva *webovou službu výstup* modulů na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="481d4-137">Drag two *web service Output* modules onto the experiment canvas.</span></span> <span data-ttu-id="481d4-138">Připojit výstup *Train Model* modulu k jednomu a výstup *Evaluate Model* modulu na druhý.</span><span class="sxs-lookup"><span data-stu-id="481d4-138">Connect the output of the *Train Model* module to one and the output of the *Evaluate Model* module to the other.</span></span> <span data-ttu-id="481d4-139">Výstup webové služby pro **Train Model** nám poskytuje nové naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="481d4-139">The web service output for **Train Model** gives us the new trained model.</span></span> <span data-ttu-id="481d4-140">Výstup připojené k **Evaluate Model** vrátí výstupu Tenhle modul, který je výsledky výkonu.</span><span class="sxs-lookup"><span data-stu-id="481d4-140">The output attached to **Evaluate Model** returns that module’s output, which is the performance results.</span></span>
5. <span data-ttu-id="481d4-141">Klikněte na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="481d4-141">Click **Run**.</span></span> 

<span data-ttu-id="481d4-142">Dále je nutné nasadit výukový experiment jako webová služba, která vytváří modulu trained model a výsledky vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="481d4-142">Next you must deploy the training experiment as a web service that produces a trained model and model evaluation results.</span></span> <span data-ttu-id="481d4-143">K tomu, jsou závislé na tom, jestli pracujete s webovou službu Classic nebo novou webovou službu vaší další sadu akcí.</span><span class="sxs-lookup"><span data-stu-id="481d4-143">To accomplish this, your next set of actions are dependent on whether you are working with a Classic web service or a New web service.</span></span>  

<span data-ttu-id="481d4-144">**Classic webové služby**</span><span class="sxs-lookup"><span data-stu-id="481d4-144">**Classic web service**</span></span>

<span data-ttu-id="481d4-145">V dolní části plátna experimentu, klikněte na tlačítko **nastavit webové služby** a vyberte **nasazení webové služby [Classic]**.</span><span class="sxs-lookup"><span data-stu-id="481d4-145">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="481d4-146">Webová služba **řídicí panel** se zobrazí s klíč rozhraní API a na stránce nápovědy rozhraní API pro dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="481d4-146">The Web Service **Dashboard** is displayed with the API Key and the API help page for Batch Execution.</span></span> <span data-ttu-id="481d4-147">Pouze metodu Batch Execution můžete použít pro vytvoření Trénované modely.</span><span class="sxs-lookup"><span data-stu-id="481d4-147">Only the Batch Execution method can be used for creating Trained Models.</span></span>

<span data-ttu-id="481d4-148">**Novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="481d4-148">**New web service**</span></span>

<span data-ttu-id="481d4-149">V dolní části plátna experimentu, klikněte na tlačítko **nastavit webové služby** a vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="481d4-149">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="481d4-150">Otevře se na stránku webové služby nasadit portálu webové služby Azure Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="481d4-150">The Web Service Azure Machine Learning Web Services portal opens to the Deploy web service page.</span></span> <span data-ttu-id="481d4-151">Zadejte název pro webovou službu a zvolte plán platby a potom klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="481d4-151">Type a name for your web service and choose a payment plan, then click **Deploy**.</span></span> <span data-ttu-id="481d4-152">Pouze Batch Execution metodu lze použít pro vytvoření Trénované modely</span><span class="sxs-lookup"><span data-stu-id="481d4-152">Only the Batch Execution method can be used for creating Trained Models</span></span>

<span data-ttu-id="481d4-153">V obou případech po dokončení spuštění experimentu výsledné pracovního postupu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="481d4-153">In either case, after experiment has completed running, the resulting workflow should look as follows:</span></span>

![Po spuštění výsledného workflow.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a><span data-ttu-id="481d4-155">Přeučování modelu s použitím BES nová data</span><span class="sxs-lookup"><span data-stu-id="481d4-155">Retrain the model with new data using BES</span></span>
<span data-ttu-id="481d4-156">V tomto příkladu použijete C# k vytvoření retraining aplikace.</span><span class="sxs-lookup"><span data-stu-id="481d4-156">For this example, you are using C# to create the retraining application.</span></span> <span data-ttu-id="481d4-157">Ukázkový kód Python nebo R můžete použít také k provedení této úlohy.</span><span class="sxs-lookup"><span data-stu-id="481d4-157">You can also use the Python or R sample code to accomplish this task.</span></span>

<span data-ttu-id="481d4-158">K volání rozhraní Retraining API:</span><span class="sxs-lookup"><span data-stu-id="481d4-158">To call the Retraining APIs:</span></span>

1. <span data-ttu-id="481d4-159">Vytvořte konzolovou aplikaci C# v sadě Visual Studio: **nový** > **projektu** > **Visual C#** > **Windows Classic Desktop** > **konzolovou aplikaci (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="481d4-159">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="481d4-160">Přihlaste se k portálu webová služba Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="481d4-160">Sign in to the Machine Learning Web Service portal.</span></span>
3. <span data-ttu-id="481d4-161">Pokud pracujete s webovou službou Classic, klikněte na tlačítko **Classic webové služby**.</span><span class="sxs-lookup"><span data-stu-id="481d4-161">If you are working with a Classic web service, click **Classic Web Services**.</span></span>
   1. <span data-ttu-id="481d4-162">Klikněte na tlačítko webové služby, kterou pracujete.</span><span class="sxs-lookup"><span data-stu-id="481d4-162">Click the web service you are working with.</span></span>
   2. <span data-ttu-id="481d4-163">Klikněte na výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="481d4-163">Click the default endpoint.</span></span>
   3. <span data-ttu-id="481d4-164">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="481d4-164">Click **Consume**.</span></span>
   4. <span data-ttu-id="481d4-165">V dolní části **spotřebě** stránky v **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="481d4-165">At the bottom of the **Consume** page, in the **Sample Code** section, click **Batch**.</span></span>
   5. <span data-ttu-id="481d4-166">Přejděte ke kroku 5 tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="481d4-166">Continue to step 5 of this procedure.</span></span>
4. <span data-ttu-id="481d4-167">Pokud pracujete s novou webovou službu, klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="481d4-167">If you are working with a New web service, click **Web Services**.</span></span>
   1. <span data-ttu-id="481d4-168">Klikněte na tlačítko webové služby, kterou pracujete.</span><span class="sxs-lookup"><span data-stu-id="481d4-168">Click the web service you are working with.</span></span>
   2. <span data-ttu-id="481d4-169">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="481d4-169">Click **Consume**.</span></span>
   3. <span data-ttu-id="481d4-170">Na konci spotřebovat, v stránky **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="481d4-170">At the bottom of the Consume page, in the **Sample Code** section, click **Batch**.</span></span>
5. <span data-ttu-id="481d4-171">Ukázka kódu C# pro spuštění dávky zkopírujte a vložte jej do souboru Program.cs, ujistěte se, že obor názvů zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="481d4-171">Copy the sample C# code for batch execution and paste it into the Program.cs file, making sure the namespace remains intact.</span></span>

<span data-ttu-id="481d4-172">Přidejte balíček Nuget Microsoft.AspNet.WebApi.Client jako zadaný v komentářích.</span><span class="sxs-lookup"><span data-stu-id="481d4-172">Add the Nuget package Microsoft.AspNet.WebApi.Client as specified in the comments.</span></span> <span data-ttu-id="481d4-173">Pokud chcete přidat odkaz na Microsoft.WindowsAzure.Storage.dll, je nejdřív potřeba nainstalovat klientské knihovny pro úložiště služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="481d4-173">To add the reference to Microsoft.WindowsAzure.Storage.dll, you might first need to install the client library for Microsoft Azure storage services.</span></span> <span data-ttu-id="481d4-174">Další informace najdete v tématu [služby úložiště systému Windows](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="481d4-174">For more information, see [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

### <a name="update-the-apikey-declaration"></a><span data-ttu-id="481d4-175">Aktualizace apikey deklarace</span><span class="sxs-lookup"><span data-stu-id="481d4-175">Update the apikey declaration</span></span>
<span data-ttu-id="481d4-176">Vyhledejte **apikey** deklarace.</span><span class="sxs-lookup"><span data-stu-id="481d4-176">Locate the **apikey** declaration.</span></span>

    const string apiKey = "abc123"; // Replace this with the API key for the web service

<span data-ttu-id="481d4-177">V **informace o základní spotřeby** části **spotřebě** stránky, vyhledejte primární klíč a zkopírujte ho do **apikey** deklarace.</span><span class="sxs-lookup"><span data-stu-id="481d4-177">In the **Basic consumption info** section of the **Consume** page, locate the primary key and copy it to the **apikey** declaration.</span></span>

### <a name="update-the-azure-storage-information"></a><span data-ttu-id="481d4-178">Aktualizovat informace o Azure Storage</span><span class="sxs-lookup"><span data-stu-id="481d4-178">Update the Azure Storage information</span></span>
<span data-ttu-id="481d4-179">Ukázkový kód BES se uloží soubor z místního disku (například "C:\temp\CensusIpnput.csv") do služby Azure Storage, procesy a zapíše výsledky zpět do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="481d4-179">The BES sample code uploads a file from a local drive (For example "C:\temp\CensusIpnput.csv") to Azure Storage, processes it, and writes the results back to Azure Storage.</span></span>  

<span data-ttu-id="481d4-180">K provedení této úlohy, musí získat informace název, klíč a kontejneru účtu úložiště pro váš účet úložiště z portálu Azure classic a aktualizace odpovídající hodnoty v kódu.</span><span class="sxs-lookup"><span data-stu-id="481d4-180">To accomplish this task, you must retrieve the Storage account name, key, and container information for your Storage account from the classic Azure portal and the update corresponding values in the code.</span></span> 

1. <span data-ttu-id="481d4-181">Přihlaste se k portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="481d4-181">Sign in to the classic Azure portal.</span></span>
2. <span data-ttu-id="481d4-182">Ve sloupci levém navigačním panelu klikněte na tlačítko **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="481d4-182">In the left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="481d4-183">Ze seznamu účtů úložiště vyberte jeden pro uložení retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="481d4-183">From the list of storage accounts, select one to store the retrained model.</span></span>
4. <span data-ttu-id="481d4-184">V dolní části stránky klikněte na tlačítko **spravovat přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="481d4-184">At the bottom of the page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="481d4-185">Zkopírujte a uložte **primární přístupový klíč** a zavřete tento dialog.</span><span class="sxs-lookup"><span data-stu-id="481d4-185">Copy and save the **Primary Access Key** and close the dialog.</span></span> 
6. <span data-ttu-id="481d4-186">V horní části stránky klikněte na tlačítko **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="481d4-186">At the top of the page, click **Containers**.</span></span>
7. <span data-ttu-id="481d4-187">Vyberte existující kontejner nebo vytvořte novou a uložit název.</span><span class="sxs-lookup"><span data-stu-id="481d4-187">Select an existing container or create a new one and save the name.</span></span>

<span data-ttu-id="481d4-188">Vyhledejte *StorageAccountName*, *StorageAccountKey*, a *StorageContainerName* deklarace a aktualizujte hodnoty jste uložili z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="481d4-188">Locate the *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations and update the values you saved from the Azure portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

<span data-ttu-id="481d4-189">Můžete také zajistit, aby byl vstupní soubor k dispozici v umístění, které zadáte v kódu.</span><span class="sxs-lookup"><span data-stu-id="481d4-189">You also must ensure the input file is available at the location you specify in the code.</span></span> 

### <a name="specify-the-output-location"></a><span data-ttu-id="481d4-190">Zadejte umístění výstupu</span><span class="sxs-lookup"><span data-stu-id="481d4-190">Specify the output location</span></span>
<span data-ttu-id="481d4-191">Pokud zadáte umístění výstupu v datové části žádosti, příponu souboru zadaná v *RelativeLocation* musí být zadány jako ilearner.</span><span class="sxs-lookup"><span data-stu-id="481d4-191">When specifying the output location in the Request Payload, the extension of the file specified in *RelativeLocation* must be specified as ilearner.</span></span> 

<span data-ttu-id="481d4-192">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="481d4-192">See the following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> <span data-ttu-id="481d4-193">Názvy výstupních umístění se může lišit od těch v tomto návodu v pořadí, ve které jste přidali modulů výstup webové služby.</span><span class="sxs-lookup"><span data-stu-id="481d4-193">The names of your output locations may be different from the ones in this walkthrough based on the order in which you added the web service output modules.</span></span> <span data-ttu-id="481d4-194">Vzhledem k tomu, že jste nastavili tento výukový experiment s dvěma výstupy, výsledky budou zahrnovat informace o umístění úložiště pro oba dva.</span><span class="sxs-lookup"><span data-stu-id="481d4-194">Since you set up this training experiment with two outputs, the results include storage location information for both of them.</span></span>  
> 
> 

![Retraining výstup][6]

<span data-ttu-id="481d4-196">Obrázek 4: Retraining výstup.</span><span class="sxs-lookup"><span data-stu-id="481d4-196">Diagram 4: Retraining output.</span></span>

## <a name="evaluate-the-retraining-results"></a><span data-ttu-id="481d4-197">Vyhodnoťte Retraining výsledky</span><span class="sxs-lookup"><span data-stu-id="481d4-197">Evaluate the Retraining Results</span></span>
<span data-ttu-id="481d4-198">Při spuštění aplikace výstup obsahuje adresu URL a SAS token potřebné pro přístup k výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="481d4-198">When you run the application, the output includes the URL and SAS token necessary to access the evaluation results.</span></span>

<span data-ttu-id="481d4-199">Zobrazí výsledky retrained modelu tím, že zkombinujete *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup pro *output2* (jak je znázorněno na předchozím obrázku výstup retraining) a vkládání úplnou adresu URL v adresním řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="481d4-199">You can see the performance results of the retrained model by combining the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results for *output2* (as shown in the preceding retraining output image) and pasting the complete URL in the browser address bar.</span></span>  

<span data-ttu-id="481d4-200">Podívejte se na výsledky, abyste zjistili, jestli pro nově cvičný model provádí dostatečně dobře nahradit stávající.</span><span class="sxs-lookup"><span data-stu-id="481d4-200">Examine the results to determine whether the newly trained model performs well enough to replace the existing one.</span></span>

<span data-ttu-id="481d4-201">Kopírování *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výstup výsledků, použijeme je během procesu retraining.</span><span class="sxs-lookup"><span data-stu-id="481d4-201">Copy the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results, you will use them during the retraining process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="481d4-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="481d4-202">Next steps</span></span>
<span data-ttu-id="481d4-203">Pokud jste nasadili prediktivní webovou službu kliknutím **nasazení webové služby [Classic]**, najdete v části [Přeučování webové služby Classic](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-203">If you deployed the predictive web service by clicking **Deploy Web Service [Classic]**, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="481d4-204">Pokud jste nasadili prediktivní webovou službu kliknutím **nasazení [nové] webové služby**, najdete v části [Přeučování novou webovou službu pomocí rutin Machine Learning Management](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="481d4-204">If you deployed the predictive web service by clicking **Deploy Web Service [New]**, see [Retrain a New web service using the Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
