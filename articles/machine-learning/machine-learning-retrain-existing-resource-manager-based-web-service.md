---
title: "Přeučování existující prediktivní webovou službu | Microsoft Docs"
description: "Zjistěte, jak aktualizovat webovou službu, která používá nově trénovaného modelu v Azure Machine Learning a přeučování modelu."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
ms.openlocfilehash: bdc994daf441d397157f8e6cbcf84d72584927f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a><span data-ttu-id="9836c-103">Přeučování existující prediktivní webovou službu</span><span class="sxs-lookup"><span data-stu-id="9836c-103">Retrain an existing predictive web service</span></span>
<span data-ttu-id="9836c-104">Tento dokument popisuje proces retraining pro následující scénář:</span><span class="sxs-lookup"><span data-stu-id="9836c-104">This document describes the retraining process for the following scenario:</span></span>

* <span data-ttu-id="9836c-105">Máte výukový experiment a prediktivní experiment, který jste nasadili jako operationalized webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-105">You have a training experiment and a predictive experiment that you have deployed as an operationalized web service.</span></span>
* <span data-ttu-id="9836c-106">Máte nová data, že chcete, aby vaši prediktivní webovou službu používat k provádění jeho vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="9836c-106">You have new data that you want your predictive web service to use to perform its scoring.</span></span>

> [!NOTE] 
> <span data-ttu-id="9836c-107">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které, můžete nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-107">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="9836c-108">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="9836c-108">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="9836c-109">Počínaje existující webovou službu a experimentů, je třeba postupovat podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="9836c-109">Starting with your existing web service and experiments, you need to follow these steps:</span></span>

1. <span data-ttu-id="9836c-110">Aktualizace modelu.</span><span class="sxs-lookup"><span data-stu-id="9836c-110">Update the model.</span></span>
   1. <span data-ttu-id="9836c-111">Upravte experimentu školení pro webové služby vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="9836c-111">Modify your training experiment to allow for web service inputs and outputs.</span></span>
   2. <span data-ttu-id="9836c-112">Výukový experiment nasaďte jako retraining webovou službu.</span><span class="sxs-lookup"><span data-stu-id="9836c-112">Deploy the training experiment as a retraining web service.</span></span>
   3. <span data-ttu-id="9836c-113">Pomocí výukový experiment dávky spuštění služby (BES) přeučování modelu.</span><span class="sxs-lookup"><span data-stu-id="9836c-113">Use the training experiment's Batch Execution Service (BES) to retrain the model.</span></span>
2. <span data-ttu-id="9836c-114">Rutiny Azure Machine Learning PowerShell použijte k aktualizaci prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="9836c-114">Use the Azure Machine Learning PowerShell cmdlets to update the predictive experiment.</span></span>
   1. <span data-ttu-id="9836c-115">Přihlaste se ke svému účtu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9836c-115">Sign in to your Azure Resource Manager account.</span></span>
   2. <span data-ttu-id="9836c-116">Získáte definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-116">Get the web service definition.</span></span>
   3. <span data-ttu-id="9836c-117">Exportujte definice webové služby jako JSON.</span><span class="sxs-lookup"><span data-stu-id="9836c-117">Export the web service definition as JSON.</span></span>
   4. <span data-ttu-id="9836c-118">Aktualizujte odkaz na objekt blob ilearner v kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="9836c-118">Update the reference to the ilearner blob in the JSON.</span></span>
   5. <span data-ttu-id="9836c-119">Importujte JSON do definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-119">Import the JSON into a web service definition.</span></span>
   6. <span data-ttu-id="9836c-120">Aktualizujte webovou službu pomocí nové definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-120">Update the web service with a new web service definition.</span></span>

## <a name="deploy-the-training-experiment"></a><span data-ttu-id="9836c-121">Nasazení výukový experiment</span><span class="sxs-lookup"><span data-stu-id="9836c-121">Deploy the training experiment</span></span>
<span data-ttu-id="9836c-122">Pokud chcete nasadit výukový experiment jako retraining webové služby, musíte přidat webové služby vstupy a výstupy do modelu.</span><span class="sxs-lookup"><span data-stu-id="9836c-122">To deploy the training experiment as a retraining web service, you must add web service inputs and outputs to the model.</span></span> <span data-ttu-id="9836c-123">Připojením *výstup webové služby* modulu experimentu  *[Train Model] [ train-model]*  modulu, povolte výukový experiment k vytvoření nové trained model, který můžete použít v prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="9836c-123">By connecting a *Web Service Output* module to the experiment's *[Train Model][train-model]* module, you enable the training experiment to produce a new trained model that you can use in your predictive experiment.</span></span> <span data-ttu-id="9836c-124">Pokud máte *Evaluate Model* modul, můžete taky přiložit výstup webové služby se získat výsledky hodnocení jako výstup.</span><span class="sxs-lookup"><span data-stu-id="9836c-124">If you have an *Evaluate Model* module, you can also attach web service output to get the evaluation results as output.</span></span>

<span data-ttu-id="9836c-125">Aktualizace výukový experiment:</span><span class="sxs-lookup"><span data-stu-id="9836c-125">To update your training experiment:</span></span>

1. <span data-ttu-id="9836c-126">Připojit *webové služby vstupní* modulu datového vstupu (například *vyčištění chybějících dat* modulu).</span><span class="sxs-lookup"><span data-stu-id="9836c-126">Connect a *Web Service Input* module to your data input (for example, a *Clean Missing Data* module).</span></span> <span data-ttu-id="9836c-127">Obvykle budete chtít zajistit, že se vstupní data zpracovány stejným způsobem jako původní data školení.</span><span class="sxs-lookup"><span data-stu-id="9836c-127">Typically, you want to ensure that your input data is processed in the same way as your original training data.</span></span>
2. <span data-ttu-id="9836c-128">Připojit *výstup webové služby* modulu výstup vaše *Train Model* modulu.</span><span class="sxs-lookup"><span data-stu-id="9836c-128">Connect a *Web Service Output* module to the output of your *Train Model* module.</span></span>
3. <span data-ttu-id="9836c-129">Pokud máte *Evaluate Model* modulu a chcete výstup výsledky hodnocení, připojit *výstup webové služby* modulu výstup vaše *Evaluate Model* modulu.</span><span class="sxs-lookup"><span data-stu-id="9836c-129">If you have an *Evaluate Model* module and you want to output the evaluation results, connect a *Web Service Output* module to the output of your *Evaluate Model* module.</span></span>

<span data-ttu-id="9836c-130">Spuštění experimentu.</span><span class="sxs-lookup"><span data-stu-id="9836c-130">Run your experiment.</span></span>

<span data-ttu-id="9836c-131">Dále je nutné nasadit výukový experiment jako webová služba, která vytváří modulu trained model a výsledky vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="9836c-131">Next, you must deploy the training experiment as a web service that produces a trained model and model evaluation results.</span></span>  

<span data-ttu-id="9836c-132">V dolní části plátna experimentu, klikněte na tlačítko **nastavit webové služby**a potom vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="9836c-132">At the bottom of the experiment canvas, click **Set Up Web Service**, and then select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="9836c-133">Webové služby Azure Machine Learning portál se otevře **nasazení webové služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="9836c-133">The Azure Machine Learning Web Services portal opens to the **Deploy Web Service** page.</span></span> <span data-ttu-id="9836c-134">Zadejte název pro webovou službu, zvolte plán platby a pak klikněte na tlačítko **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="9836c-134">Type a name for your web service, choose a payment plan, and then click **Deploy**.</span></span> <span data-ttu-id="9836c-135">Metoda Batch Execution můžete použít pouze pro vytváření trénované modely.</span><span class="sxs-lookup"><span data-stu-id="9836c-135">You can only use the Batch Execution method for creating trained models.</span></span>

## <a name="retrain-the-model-with-new-data-by-using-bes"></a><span data-ttu-id="9836c-136">Přeučování model s nová data pomocí BES</span><span class="sxs-lookup"><span data-stu-id="9836c-136">Retrain the model with new data by using BES</span></span>
<span data-ttu-id="9836c-137">V tomto příkladu používáme C# k vytvoření retraining aplikace.</span><span class="sxs-lookup"><span data-stu-id="9836c-137">For this example, we're using C# to create the retraining application.</span></span> <span data-ttu-id="9836c-138">Ukázka kódu Pythonu nebo R můžete použít také k provedení této úlohy.</span><span class="sxs-lookup"><span data-stu-id="9836c-138">You can also use Python or R sample code to accomplish this task.</span></span>

<span data-ttu-id="9836c-139">K volání rozhraní retraining API:</span><span class="sxs-lookup"><span data-stu-id="9836c-139">To call the retraining APIs:</span></span>

1. <span data-ttu-id="9836c-140">Vytvořte konzolovou aplikaci C# v sadě Visual Studio: **nový** > **projektu** > **Visual C#** > **Windows Classic Desktop** > **konzolovou aplikaci (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="9836c-140">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="9836c-141">Přihlaste se k portálu webové služby Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9836c-141">Sign in to the Machine Learning Web Services portal.</span></span>
3. <span data-ttu-id="9836c-142">Klikněte na tlačítko webové služby, která kterými pracujete.</span><span class="sxs-lookup"><span data-stu-id="9836c-142">Click the web service that you're working with.</span></span>
4. <span data-ttu-id="9836c-143">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="9836c-143">Click **Consume**.</span></span>
5. <span data-ttu-id="9836c-144">V dolní části **spotřebě** stránky v **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="9836c-144">At the bottom of the **Consume** page, in the **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="9836c-145">Ukázka kódu C# pro spuštění dávky zkopírujte a vložte jej do souboru Program.cs.</span><span class="sxs-lookup"><span data-stu-id="9836c-145">Copy the sample C# code for batch execution and paste it into the Program.cs file.</span></span> <span data-ttu-id="9836c-146">Ujistěte se, že obor názvů zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="9836c-146">Make sure that the namespace remains intact.</span></span>

<span data-ttu-id="9836c-147">Přidejte balíček NuGet Microsoft.AspNet.WebApi.Client, jak je uvedeno v komentářích.</span><span class="sxs-lookup"><span data-stu-id="9836c-147">Add the NuGet package Microsoft.AspNet.WebApi.Client, as specified in the comments.</span></span> <span data-ttu-id="9836c-148">Pokud chcete přidat odkaz na Microsoft.WindowsAzure.Storage.dll, může být nejdřív musíte nainstalovat [Klientská knihovna pro úložiště Azure services](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="9836c-148">To add the reference to Microsoft.WindowsAzure.Storage.dll, you might first need to install the [client library for Azure Storage services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

<span data-ttu-id="9836c-149">Následující snímek obrazovky ukazuje **spotřebě** na portálu Azure Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-149">The following screenshot shows the **Consume** page in the Azure Machine Learning Web Services portal.</span></span>

![Využívat stránky][1]

### <a name="update-the-apikey-declaration"></a><span data-ttu-id="9836c-151">Aktualizace apikey deklarace</span><span class="sxs-lookup"><span data-stu-id="9836c-151">Update the apikey declaration</span></span>
<span data-ttu-id="9836c-152">Vyhledejte **apikey** deklarace:</span><span class="sxs-lookup"><span data-stu-id="9836c-152">Locate the **apikey** declaration:</span></span>

    const string apiKey = "abc123"; // Replace this with the API key for the web service

<span data-ttu-id="9836c-153">V **informace o základní spotřeby** části **spotřebě** stránky, vyhledejte primární klíč a zkopírujte ho do **apikey** deklarace.</span><span class="sxs-lookup"><span data-stu-id="9836c-153">In the **Basic consumption info** section of the **Consume** page, locate the primary key and copy it to the **apikey** declaration.</span></span>

### <a name="update-the-azure-storage-information"></a><span data-ttu-id="9836c-154">Aktualizovat informace o Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9836c-154">Update the Azure Storage information</span></span>
<span data-ttu-id="9836c-155">Ukázkový kód BES se uloží soubor z místního disku (například "C:\temp\CensusIpnput.csv") do služby Azure Storage, procesy a zapíše výsledky zpět do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9836c-155">The BES sample code uploads a file from a local drive (for example, "C:\temp\CensusIpnput.csv") to Azure Storage, processes it, and writes the results back to Azure Storage.</span></span>  

<span data-ttu-id="9836c-156">Aktualizovat informace o Azure Storage, musí získat název účtu úložiště, klíč a informace o kontejneru pro váš účet úložiště z portálu Azure classic, a pak aktualizace correspondi po spuštění experimentu, výsledná pracovního postupu by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="9836c-156">To update the Azure Storage information, you must retrieve the storage account name, key, and container information for your storage account from the Azure classic portal, and then update the correspondi After running your experiment, the resulting workflow should be similar to the following:</span></span>

![Po spuštění výsledného pracovního postupu][4]<span data-ttu-id="9836c-158">NG hodnoty v kódu.</span><span class="sxs-lookup"><span data-stu-id="9836c-158">ng values in the code.</span></span>

1. <span data-ttu-id="9836c-159">Přihlaste se k portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="9836c-159">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="9836c-160">Ve sloupci levém navigačním panelu klikněte na tlačítko **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="9836c-160">In the left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="9836c-161">Ze seznamu účtů úložiště vyberte jeden pro uložení retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="9836c-161">From the list of storage accounts, select one to store the retrained model.</span></span>
4. <span data-ttu-id="9836c-162">V dolní části stránky klikněte na tlačítko **spravovat přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="9836c-162">At the bottom of the page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="9836c-163">Zkopírujte a uložte **primární přístupový klíč** a zavřete tento dialog.</span><span class="sxs-lookup"><span data-stu-id="9836c-163">Copy and save the **Primary Access Key** and close the dialog.</span></span>
6. <span data-ttu-id="9836c-164">V horní části stránky klikněte na tlačítko **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="9836c-164">At the top of the page, click **Containers**.</span></span>
7. <span data-ttu-id="9836c-165">Vyberte existující kontejner, nebo vytvořte novou a uložit název.</span><span class="sxs-lookup"><span data-stu-id="9836c-165">Select an existing container, or create a new one and save the name.</span></span>

<span data-ttu-id="9836c-166">Vyhledejte *StorageAccountName*, *StorageAccountKey*, a *StorageContainerName* deklarace a aktualizujte hodnoty, které jste uložili z klasického portálu.</span><span class="sxs-lookup"><span data-stu-id="9836c-166">Locate the *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations, and update the values that you saved from the classic portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

<span data-ttu-id="9836c-167">Je také nutné zajistit, že vstupní soubor je k dispozici v umístění, které zadáte v kódu.</span><span class="sxs-lookup"><span data-stu-id="9836c-167">You also must ensure that the input file is available at the location that you specify in the code.</span></span>

### <a name="specify-the-output-location"></a><span data-ttu-id="9836c-168">Zadejte umístění výstupu</span><span class="sxs-lookup"><span data-stu-id="9836c-168">Specify the output location</span></span>
<span data-ttu-id="9836c-169">Pokud zadáte umístění výstupu v žádosti o datové části, přípona souboru, který je uveden v *RelativeLocation* musí být zadány jako `ilearner`.</span><span class="sxs-lookup"><span data-stu-id="9836c-169">When you specify the output location in the Request Payload, the extension of the file that is specified in *RelativeLocation* must be specified as `ilearner`.</span></span> <span data-ttu-id="9836c-170">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="9836c-170">See the following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

<span data-ttu-id="9836c-171">Tady je příklad výstupu retraining: ![Retraining výstup][6]</span><span class="sxs-lookup"><span data-stu-id="9836c-171">The following is an example of retraining output: ![Retraining output][6]</span></span>

## <a name="evaluate-the-retraining-results"></a><span data-ttu-id="9836c-172">Vyhodnoťte retraining výsledky</span><span class="sxs-lookup"><span data-stu-id="9836c-172">Evaluate the retraining results</span></span>
<span data-ttu-id="9836c-173">Při spuštění aplikace, zahrnuje adresu URL a token podpisů sdíleného přístupu, který jsou potřebné pro přístup k výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="9836c-173">When you run the application, the output includes the URL and shared access signatures token that are necessary to access the evaluation results.</span></span>

<span data-ttu-id="9836c-174">Zobrazí výsledky retrained modelu tím, že zkombinujete *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup pro *output2* (jak je znázorněno na předchozím obrázku výstup retraining) a vkládání úplnou adresu URL do panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9836c-174">You can see the performance results of the retrained model by combining the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results for *output2* (as shown in the preceding retraining output image) and pasting the complete URL into the browser address bar.</span></span>  

<span data-ttu-id="9836c-175">Podívejte se na výsledky, abyste zjistili, jestli pro nově cvičný model provádí dostatečně dobře nahradit stávající.</span><span class="sxs-lookup"><span data-stu-id="9836c-175">Examine the results to determine whether the newly trained model performs well enough to replace the existing one.</span></span>

<span data-ttu-id="9836c-176">Kopírování *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstupu.</span><span class="sxs-lookup"><span data-stu-id="9836c-176">Copy the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results.</span></span>

## <a name="retrain-the-web-service"></a><span data-ttu-id="9836c-177">Přeučování webové služby</span><span class="sxs-lookup"><span data-stu-id="9836c-177">Retrain the web service</span></span>
<span data-ttu-id="9836c-178">Pokud jste přeučování novou webovou službu, je aktualizovat definice prediktivní webové služby, chcete-li nový trained model.</span><span class="sxs-lookup"><span data-stu-id="9836c-178">When you retrain a new web service, you update the predictive web service definition to reference the new trained model.</span></span> <span data-ttu-id="9836c-179">Definice webové služby je interní reprezentací pro cvičný model webové služby a není přímo změn.</span><span class="sxs-lookup"><span data-stu-id="9836c-179">The web service definition is an internal representation of the trained model of the web service and is not directly modifiable.</span></span> <span data-ttu-id="9836c-180">Ujistěte se, že jsou načítání definice webové služby prediktivní experiment a není experimentu školení.</span><span class="sxs-lookup"><span data-stu-id="9836c-180">Make sure that you are retrieving the web service definition for your predictive experiment and not your training experiment.</span></span>

## <a name="sign-in-to-azure-resource-manager"></a><span data-ttu-id="9836c-181">Přihlaste se k Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9836c-181">Sign in to Azure Resource Manager</span></span>
<span data-ttu-id="9836c-182">Musíte nejprve se přihlásit k účtu Azure z prostředí PowerShell pomocí [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="9836c-182">You must first sign in to your Azure account from within the PowerShell environment by using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-the-web-service-definition-object"></a><span data-ttu-id="9836c-183">Získání objektu definice webové služby</span><span class="sxs-lookup"><span data-stu-id="9836c-183">Get the Web Service Definition object</span></span>
<span data-ttu-id="9836c-184">V dalším kroku získání objektu definice webové služby pomocí volání [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="9836c-184">Next, get the Web Service Definition object by calling the [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="9836c-185">Chcete-li zjistit název skupiny prostředků existující webovou službu, spusťte rutinu Get-AzureRmMlWebService bez parametrů pro zobrazení webové služby v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="9836c-185">To determine the resource group name of an existing web service, run the Get-AzureRmMlWebService cmdlet without any parameters to display the web services in your subscription.</span></span> <span data-ttu-id="9836c-186">Vyhledejte webovou službu a podívejte se na jeho identifikátorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-186">Locate the web service, and then look at its web service ID.</span></span> <span data-ttu-id="9836c-187">Název skupiny prostředků je čtvrtý element v ID, bezprostředně za *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="9836c-187">The name of the resource group is the fourth element in the ID, just after the *resourceGroups* element.</span></span> <span data-ttu-id="9836c-188">V následujícím příkladu je název skupiny prostředků výchozí-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="9836c-188">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="9836c-189">Alternativně k určení názvu skupiny prostředků existující webovou službu, přihlaste se k portálu Azure Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="9836c-189">Alternatively, to determine the resource group name of an existing web service, sign in to the Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="9836c-190">Vyberte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="9836c-190">Select the web service.</span></span> <span data-ttu-id="9836c-191">Název skupiny prostředků je pátý element adresy URL webové služby, bezprostředně za *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="9836c-191">The resource group name is the fifth element of the URL of the web service, just after the *resourceGroups* element.</span></span> <span data-ttu-id="9836c-192">V následujícím příkladu je název skupiny prostředků výchozí-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="9836c-192">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a><span data-ttu-id="9836c-193">Export objektu definice webové služby jako JSON</span><span class="sxs-lookup"><span data-stu-id="9836c-193">Export the Web Service Definition object as JSON</span></span>
<span data-ttu-id="9836c-194">Chcete-li upravit definici pro cvičný model použít nově trained model, musíte nejprve použít [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) k exportu do souboru formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9836c-194">To modify the definition of the trained model to use the newly trained model, you must first use the [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet to export it to a JSON-format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a><span data-ttu-id="9836c-195">Aktualizovat odkaz na objekt blob ilearner</span><span class="sxs-lookup"><span data-stu-id="9836c-195">Update the reference to the ilearner blob</span></span>
<span data-ttu-id="9836c-196">V prostředky, vyhledejte [trained model], aktualizujte *identifikátor uri* hodnotu *locationInfo* uzlu s identifikátorem URI objektu ilearner blob.</span><span class="sxs-lookup"><span data-stu-id="9836c-196">In the assets, locate the [trained model], update the *uri* value in the *locationInfo* node with the URI of the ilearner blob.</span></span> <span data-ttu-id="9836c-197">Identifikátor URI je generován kombinování *BaseLocation* a *RelativeLocation* z výstupu BES retraining volání.</span><span class="sxs-lookup"><span data-stu-id="9836c-197">The URI is generated by combining the *BaseLocation* and the *RelativeLocation* from the output of the BES retraining call.</span></span>

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a><span data-ttu-id="9836c-198">Importujte JSON do objektu definice webové služby</span><span class="sxs-lookup"><span data-stu-id="9836c-198">Import the JSON into a Web Service Definition object</span></span>
<span data-ttu-id="9836c-199">Je nutné použít [Import AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) rutiny převést změněný soubor JSON zpět do objektu definice webové služby, který můžete použít k aktualizaci predicative experimentu.</span><span class="sxs-lookup"><span data-stu-id="9836c-199">You must use the [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet to convert the modified JSON file back into a Web Service Definition object that you can use to update the predicative experiment.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a><span data-ttu-id="9836c-200">Aktualizovat webovou službu</span><span class="sxs-lookup"><span data-stu-id="9836c-200">Update the web service</span></span>
<span data-ttu-id="9836c-201">Nakonec použijte [aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) rutiny aktualizovat prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="9836c-201">Finally, use the [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet to update the predictive experiment.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
