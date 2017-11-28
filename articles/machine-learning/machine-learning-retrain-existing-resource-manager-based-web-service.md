---
title: "aaaRetrain existující prediktivní webové služby | Microsoft Docs"
description: "Zjistěte, jak tooretrain modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
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
ms.openlocfilehash: fb0760d0a2adc34fc5f3df1ae41bdac075f91bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a><span data-ttu-id="61254-103">Přeučování existující prediktivní webovou službu</span><span class="sxs-lookup"><span data-stu-id="61254-103">Retrain an existing predictive web service</span></span>
<span data-ttu-id="61254-104">Tento dokument popisuje hello retraining proces hello následující scénář:</span><span class="sxs-lookup"><span data-stu-id="61254-104">This document describes hello retraining process for hello following scenario:</span></span>

* <span data-ttu-id="61254-105">Máte výukový experiment a prediktivní experiment, který jste nasadili jako operationalized webové služby.</span><span class="sxs-lookup"><span data-stu-id="61254-105">You have a training experiment and a predictive experiment that you have deployed as an operationalized web service.</span></span>
* <span data-ttu-id="61254-106">Máte nová data, které chcete vaší prediktivní webové služby toouse tooperform jeho vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="61254-106">You have new data that you want your predictive web service toouse tooperform its scoring.</span></span>

> [!NOTE] 
> <span data-ttu-id="61254-107">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="61254-107">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="61254-108">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="61254-108">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="61254-109">Počínaje existující webovou službu a experimentů, je třeba toofollow tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="61254-109">Starting with your existing web service and experiments, you need toofollow these steps:</span></span>

1. <span data-ttu-id="61254-110">Aktualizace modelu hello.</span><span class="sxs-lookup"><span data-stu-id="61254-110">Update hello model.</span></span>
   1. <span data-ttu-id="61254-111">Upravte vaše tooallow experimentu školení pro webové služby vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="61254-111">Modify your training experiment tooallow for web service inputs and outputs.</span></span>
   2. <span data-ttu-id="61254-112">Nasaďte hello výukový experiment jako retraining webovou službu.</span><span class="sxs-lookup"><span data-stu-id="61254-112">Deploy hello training experiment as a retraining web service.</span></span>
   3. <span data-ttu-id="61254-113">Pomocí hello výukový experiment spuštění služby Batch (BES) tooretrain hello modelu.</span><span class="sxs-lookup"><span data-stu-id="61254-113">Use hello training experiment's Batch Execution Service (BES) tooretrain hello model.</span></span>
2. <span data-ttu-id="61254-114">Použijte hello Azure Machine Learning PowerShell rutiny tooupdate hello prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="61254-114">Use hello Azure Machine Learning PowerShell cmdlets tooupdate hello predictive experiment.</span></span>
   1. <span data-ttu-id="61254-115">Přihlaste se tooyour účet Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="61254-115">Sign in tooyour Azure Resource Manager account.</span></span>
   2. <span data-ttu-id="61254-116">Získáte definice hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="61254-116">Get hello web service definition.</span></span>
   3. <span data-ttu-id="61254-117">Exportujte hello definice webové služby jako JSON.</span><span class="sxs-lookup"><span data-stu-id="61254-117">Export hello web service definition as JSON.</span></span>
   4. <span data-ttu-id="61254-118">Aktualizace hello odkaz toohello ilearner objektů blob v hello JSON.</span><span class="sxs-lookup"><span data-stu-id="61254-118">Update hello reference toohello ilearner blob in hello JSON.</span></span>
   5. <span data-ttu-id="61254-119">Importujte hello JSON do definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="61254-119">Import hello JSON into a web service definition.</span></span>
   6. <span data-ttu-id="61254-120">Aktualizujte hello webové služby s novou definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="61254-120">Update hello web service with a new web service definition.</span></span>

## <a name="deploy-hello-training-experiment"></a><span data-ttu-id="61254-121">Nasazení výukový experiment hello</span><span class="sxs-lookup"><span data-stu-id="61254-121">Deploy hello training experiment</span></span>
<span data-ttu-id="61254-122">toodeploy hello výukový experiment jako retraining webové služby, je nutné přidat webovou službu vstupy a výstupy toohello modelu.</span><span class="sxs-lookup"><span data-stu-id="61254-122">toodeploy hello training experiment as a retraining web service, you must add web service inputs and outputs toohello model.</span></span> <span data-ttu-id="61254-123">Připojením *výstup webové služby* experimentu modul toohello * [Train Model] [ train-model] * modulu, povolte hello cvičení experimentu tooproduce nový trained model, který můžete použít v prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="61254-123">By connecting a *Web Service Output* module toohello experiment's *[Train Model][train-model]* module, you enable hello training experiment tooproduce a new trained model that you can use in your predictive experiment.</span></span> <span data-ttu-id="61254-124">Pokud máte *Evaluate Model* modul, můžete také připojit výsledky hodnocení hello výstup tooget aplikace webové služby jako výstup.</span><span class="sxs-lookup"><span data-stu-id="61254-124">If you have an *Evaluate Model* module, you can also attach web service output tooget hello evaluation results as output.</span></span>

<span data-ttu-id="61254-125">tooupdate výukový experiment:</span><span class="sxs-lookup"><span data-stu-id="61254-125">tooupdate your training experiment:</span></span>

1. <span data-ttu-id="61254-126">Připojit *webové služby vstupní* vstupních dat tooyour modulu (například *vyčištění chybějících dat* modulu).</span><span class="sxs-lookup"><span data-stu-id="61254-126">Connect a *Web Service Input* module tooyour data input (for example, a *Clean Missing Data* module).</span></span> <span data-ttu-id="61254-127">Obvykle je vhodné tooensure, který se zpracovává vstupní data v hello stejný způsob jako původní data školení.</span><span class="sxs-lookup"><span data-stu-id="61254-127">Typically, you want tooensure that your input data is processed in hello same way as your original training data.</span></span>
2. <span data-ttu-id="61254-128">Připojit *výstup webové služby* toohello výstup modulu vaše *Train Model* modulu.</span><span class="sxs-lookup"><span data-stu-id="61254-128">Connect a *Web Service Output* module toohello output of your *Train Model* module.</span></span>
3. <span data-ttu-id="61254-129">Pokud máte *Evaluate Model* modulu a vy chcete výsledky hodnocení hello toooutput, připojit *výstup webové služby* toohello výstup modulu vaše *Evaluate Model* modul.</span><span class="sxs-lookup"><span data-stu-id="61254-129">If you have an *Evaluate Model* module and you want toooutput hello evaluation results, connect a *Web Service Output* module toohello output of your *Evaluate Model* module.</span></span>

<span data-ttu-id="61254-130">Spuštění experimentu.</span><span class="sxs-lookup"><span data-stu-id="61254-130">Run your experiment.</span></span>

<span data-ttu-id="61254-131">Dále je nutné nasadit hello výukový experiment jako webová služba, která vytváří modulu trained model a výsledky vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="61254-131">Next, you must deploy hello training experiment as a web service that produces a trained model and model evaluation results.</span></span>  

<span data-ttu-id="61254-132">Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby**a potom vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="61254-132">At hello bottom of hello experiment canvas, click **Set Up Web Service**, and then select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="61254-133">Hello webové služby Azure Machine Learning portál otevře toohello **nasazení webové služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="61254-133">hello Azure Machine Learning Web Services portal opens toohello **Deploy Web Service** page.</span></span> <span data-ttu-id="61254-134">Zadejte název pro webovou službu, zvolte plán platby a pak klikněte na tlačítko **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="61254-134">Type a name for your web service, choose a payment plan, and then click **Deploy**.</span></span> <span data-ttu-id="61254-135">Metoda Batch Execution hello můžete použít pouze pro vytváření trénované modely.</span><span class="sxs-lookup"><span data-stu-id="61254-135">You can only use hello Batch Execution method for creating trained models.</span></span>

## <a name="retrain-hello-model-with-new-data-by-using-bes"></a><span data-ttu-id="61254-136">Přeučování model hello se nová data pomocí BES</span><span class="sxs-lookup"><span data-stu-id="61254-136">Retrain hello model with new data by using BES</span></span>
<span data-ttu-id="61254-137">V tomto příkladu používáme toocreate hello C# retraining aplikace.</span><span class="sxs-lookup"><span data-stu-id="61254-137">For this example, we're using C# toocreate hello retraining application.</span></span> <span data-ttu-id="61254-138">Python nebo R tooaccomplish ukázkový kód můžete také pomocí této úlohy.</span><span class="sxs-lookup"><span data-stu-id="61254-138">You can also use Python or R sample code tooaccomplish this task.</span></span>

<span data-ttu-id="61254-139">hello toocall retraining API:</span><span class="sxs-lookup"><span data-stu-id="61254-139">toocall hello retraining APIs:</span></span>

1. <span data-ttu-id="61254-140">Vytvořte konzolovou aplikaci C# v sadě Visual Studio: **nový** > **projektu** > **Visual C#** > **Windows Classic Desktop** > **konzolovou aplikaci (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="61254-140">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="61254-141">Přihlaste se toohello portál webové služby Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="61254-141">Sign in toohello Machine Learning Web Services portal.</span></span>
3. <span data-ttu-id="61254-142">Klikněte na tlačítko hello webová služba, která kterými pracujete.</span><span class="sxs-lookup"><span data-stu-id="61254-142">Click hello web service that you're working with.</span></span>
4. <span data-ttu-id="61254-143">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="61254-143">Click **Consume**.</span></span>
5. <span data-ttu-id="61254-144">Na konci hello hello **spotřebě** stránku hello **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="61254-144">At hello bottom of hello **Consume** page, in hello **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="61254-145">Zkopírujte hello ukázkový kód C# pro spuštění dávky a vložte jej do souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="61254-145">Copy hello sample C# code for batch execution and paste it into hello Program.cs file.</span></span> <span data-ttu-id="61254-146">Ujistěte se, že tento obor názvů hello zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="61254-146">Make sure that hello namespace remains intact.</span></span>

<span data-ttu-id="61254-147">Přidejte hello balíček NuGet Microsoft.AspNet.WebApi.Client, jak je uvedeno v komentářích hello.</span><span class="sxs-lookup"><span data-stu-id="61254-147">Add hello NuGet package Microsoft.AspNet.WebApi.Client, as specified in hello comments.</span></span> <span data-ttu-id="61254-148">tooadd hello odkaz tooMicrosoft.WindowsAzure.Storage.dll, bude pravděpodobně nutné nejprve tooinstall hello [Klientská knihovna pro úložiště Azure services](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="61254-148">tooadd hello reference tooMicrosoft.WindowsAzure.Storage.dll, you might first need tooinstall hello [client library for Azure Storage services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

<span data-ttu-id="61254-149">Hello následující snímek obrazovky ukazuje hello **spotřebě** stránku hello webové služby Azure Machine Learning portálu.</span><span class="sxs-lookup"><span data-stu-id="61254-149">hello following screenshot shows hello **Consume** page in hello Azure Machine Learning Web Services portal.</span></span>

![Využívat stránky][1]

### <a name="update-hello-apikey-declaration"></a><span data-ttu-id="61254-151">Aktualizovat hello apikey deklarace</span><span class="sxs-lookup"><span data-stu-id="61254-151">Update hello apikey declaration</span></span>
<span data-ttu-id="61254-152">Vyhledejte hello **apikey** deklarace:</span><span class="sxs-lookup"><span data-stu-id="61254-152">Locate hello **apikey** declaration:</span></span>

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

<span data-ttu-id="61254-153">V hello **informace o základní spotřeby** části hello **spotřebě** stránky, vyhledejte hello primární klíč a zkopírujte jej toohello **apikey** deklarace.</span><span class="sxs-lookup"><span data-stu-id="61254-153">In hello **Basic consumption info** section of hello **Consume** page, locate hello primary key and copy it toohello **apikey** declaration.</span></span>

### <a name="update-hello-azure-storage-information"></a><span data-ttu-id="61254-154">Aktualizace informací o úložišti Azure hello</span><span class="sxs-lookup"><span data-stu-id="61254-154">Update hello Azure Storage information</span></span>
<span data-ttu-id="61254-155">Hello BES ukázkový kód se uloží soubor z místního disku (například "C:\temp\CensusIpnput.csv") tooAzure úložiště, procesy a zapíše zpět tooAzure výsledky hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="61254-155">hello BES sample code uploads a file from a local drive (for example, "C:\temp\CensusIpnput.csv") tooAzure Storage, processes it, and writes hello results back tooAzure Storage.</span></span>  

<span data-ttu-id="61254-156">informací o úložišti Azure hello tooupdate, je nutné ji načíst hello účet úložiště, že název, klíč a kontejner informace pro váš účet úložiště z hello portál Azure classic a correspondi hello aktualizace po spuštění experimentu, hello Výsledná pracovní postup by měl vypadat podobně jako následující toohello:</span><span class="sxs-lookup"><span data-stu-id="61254-156">tooupdate hello Azure Storage information, you must retrieve hello storage account name, key, and container information for your storage account from hello Azure classic portal, and then update hello correspondi After running your experiment, hello resulting workflow should be similar toohello following:</span></span>

![Po spuštění výsledného pracovního postupu][4]<span data-ttu-id="61254-158">NG hodnoty v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="61254-158">ng values in hello code.</span></span>

1. <span data-ttu-id="61254-159">Přihlaste se toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="61254-159">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="61254-160">V levém navigačním sloupci hello, klikněte na tlačítko **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="61254-160">In hello left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="61254-161">Vyberte jeden toostore hello hello seznamu účtů úložiště retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="61254-161">From hello list of storage accounts, select one toostore hello retrained model.</span></span>
4. <span data-ttu-id="61254-162">V dolní části hello hello stránky, klikněte na tlačítko **spravovat přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="61254-162">At hello bottom of hello page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="61254-163">Zkopírujte a uložte hello **primární přístupový klíč** a dialogové okno zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="61254-163">Copy and save hello **Primary Access Key** and close hello dialog.</span></span>
6. <span data-ttu-id="61254-164">V horní části hello hello stránky, klikněte na tlačítko **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="61254-164">At hello top of hello page, click **Containers**.</span></span>
7. <span data-ttu-id="61254-165">Vyberte existující kontejner, nebo vytvořte novou a uložte hello název.</span><span class="sxs-lookup"><span data-stu-id="61254-165">Select an existing container, or create a new one and save hello name.</span></span>

<span data-ttu-id="61254-166">Vyhledejte hello *StorageAccountName*, *StorageAccountKey*, a *StorageContainerName* deklarace a hodnoty hello aktualizace, které jste uložili z portálu classic hello .</span><span class="sxs-lookup"><span data-stu-id="61254-166">Locate hello *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations, and update hello values that you saved from hello classic portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

<span data-ttu-id="61254-167">Je také nutné zajistit, že vstupní soubor hello je k dispozici v umístění hello, který určíte v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="61254-167">You also must ensure that hello input file is available at hello location that you specify in hello code.</span></span>

### <a name="specify-hello-output-location"></a><span data-ttu-id="61254-168">Zadejte umístění výstupu hello</span><span class="sxs-lookup"><span data-stu-id="61254-168">Specify hello output location</span></span>
<span data-ttu-id="61254-169">Když zadáte umístění výstupu hello v hello datová část požadavku, hello rozšíření hello souboru, který je uveden v *RelativeLocation* musí být zadány jako `ilearner`.</span><span class="sxs-lookup"><span data-stu-id="61254-169">When you specify hello output location in hello Request Payload, hello extension of hello file that is specified in *RelativeLocation* must be specified as `ilearner`.</span></span> <span data-ttu-id="61254-170">Viz následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="61254-170">See hello following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you want toouse for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

<span data-ttu-id="61254-171">Hello následuje příklad výstupu retraining: ![Retraining výstup][6]</span><span class="sxs-lookup"><span data-stu-id="61254-171">hello following is an example of retraining output: ![Retraining output][6]</span></span>

## <a name="evaluate-hello-retraining-results"></a><span data-ttu-id="61254-172">Vyhodnocení hello retraining výsledky</span><span class="sxs-lookup"><span data-stu-id="61254-172">Evaluate hello retraining results</span></span>
<span data-ttu-id="61254-173">Při spuštění aplikace hello výstup hello zahrnuje hello adresy URL a sdílené přístupové podpisy token, který jsou výsledky hodnocení potřeby tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="61254-173">When you run hello application, hello output includes hello URL and shared access signatures token that are necessary tooaccess hello evaluation results.</span></span>

<span data-ttu-id="61254-174">Zobrazí výsledky výkonu hello hello retrained modelu kombinací hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello pro *output2* (jak je uvedeno v předchozí retraining image výstup hello) a vkládání hello úplnou adresu URL do panelu Adresa prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="61254-174">You can see hello performance results of hello retrained model by combining hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results for *output2* (as shown in hello preceding retraining output image) and pasting hello complete URL into hello browser address bar.</span></span>  

<span data-ttu-id="61254-175">Hello výsledky toodetermine zkontrolujte, zda nově trained model hello provede dobře dostatek tooreplace hello existující jeden.</span><span class="sxs-lookup"><span data-stu-id="61254-175">Examine hello results toodetermine whether hello newly trained model performs well enough tooreplace hello existing one.</span></span>

<span data-ttu-id="61254-176">Kopírování hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello.</span><span class="sxs-lookup"><span data-stu-id="61254-176">Copy hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results.</span></span>

## <a name="retrain-hello-web-service"></a><span data-ttu-id="61254-177">Přeučování hello webové služby</span><span class="sxs-lookup"><span data-stu-id="61254-177">Retrain hello web service</span></span>
<span data-ttu-id="61254-178">Když jste přeučování novou webovou službu, aktualizujete hello prediktivní webové služby definice tooreference hello nový trained model.</span><span class="sxs-lookup"><span data-stu-id="61254-178">When you retrain a new web service, you update hello predictive web service definition tooreference hello new trained model.</span></span> <span data-ttu-id="61254-179">definice Hello webové služby je interní reprezentací hello trained model hello webové služby a není přímo změn.</span><span class="sxs-lookup"><span data-stu-id="61254-179">hello web service definition is an internal representation of hello trained model of hello web service and is not directly modifiable.</span></span> <span data-ttu-id="61254-180">Ujistěte se, že dostáváte hello definice webové služby prediktivní experiment a není experimentu školení.</span><span class="sxs-lookup"><span data-stu-id="61254-180">Make sure that you are retrieving hello web service definition for your predictive experiment and not your training experiment.</span></span>

## <a name="sign-in-tooazure-resource-manager"></a><span data-ttu-id="61254-181">Přihlaste se tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="61254-181">Sign in tooAzure Resource Manager</span></span>
<span data-ttu-id="61254-182">Je nutné se přihlásit v tooyour účet Azure z prostředí PowerShell hello pomocí hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61254-182">You must first sign in tooyour Azure account from within hello PowerShell environment by using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-hello-web-service-definition-object"></a><span data-ttu-id="61254-183">Získejte objekt hello definice webové služby</span><span class="sxs-lookup"><span data-stu-id="61254-183">Get hello Web Service Definition object</span></span>
<span data-ttu-id="61254-184">V dalším kroku získat objekt hello definice webové služby podle volání hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61254-184">Next, get hello Web Service Definition object by calling hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="61254-185">toodetermine hello název skupiny prostředků existující webovou službu, spusťte rutinu Get-AzureRmMlWebService hello bez žádné parametry toodisplay hello webové služby v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="61254-185">toodetermine hello resource group name of an existing web service, run hello Get-AzureRmMlWebService cmdlet without any parameters toodisplay hello web services in your subscription.</span></span> <span data-ttu-id="61254-186">Vyhledejte hello webové služby a podívejte se na jeho identifikátorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="61254-186">Locate hello web service, and then look at its web service ID.</span></span> <span data-ttu-id="61254-187">Hello název skupiny prostředků hello je čtvrtý element hello v hello ID bezprostředně za hello *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="61254-187">hello name of hello resource group is hello fourth element in hello ID, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="61254-188">V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="61254-188">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="61254-189">Alternativně toodetermine hello název skupiny prostředků existující webové služby, přihlaste se toohello webové služby Azure Machine Learning portálu.</span><span class="sxs-lookup"><span data-stu-id="61254-189">Alternatively, toodetermine hello resource group name of an existing web service, sign in toohello Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="61254-190">Vyberte hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="61254-190">Select hello web service.</span></span> <span data-ttu-id="61254-191">Název skupiny prostředků Hello je pátý element hello adresy URL hello hello webové služby, bezprostředně za hello *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="61254-191">hello resource group name is hello fifth element of hello URL of hello web service, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="61254-192">V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="61254-192">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-object-as-json"></a><span data-ttu-id="61254-193">Export objektu definice webové služby hello jako JSON</span><span class="sxs-lookup"><span data-stu-id="61254-193">Export hello Web Service Definition object as JSON</span></span>
<span data-ttu-id="61254-194">definice hello toomodify hello trained model toouse hello nově Trénink modelu, je nutné nejprve použít hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) tooexport rutiny se soubor tooa formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="61254-194">toomodify hello definition of hello trained model toouse hello newly trained model, you must first use hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport it tooa JSON-format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob"></a><span data-ttu-id="61254-195">Aktualizace hello odkaz toohello ilearner blob</span><span class="sxs-lookup"><span data-stu-id="61254-195">Update hello reference toohello ilearner blob</span></span>
<span data-ttu-id="61254-196">V hello prostředky, vyhledejte hello [trained model], aktualizace hello *uri* hodnota v hello *locationInfo* uzel s hello identifikátor URI objektu hello ilearner blob.</span><span class="sxs-lookup"><span data-stu-id="61254-196">In hello assets, locate hello [trained model], update hello *uri* value in hello *locationInfo* node with hello URI of hello ilearner blob.</span></span> <span data-ttu-id="61254-197">Hello identifikátor URI je generována kombinováním hello *BaseLocation* a hello *RelativeLocation* z hello výstup hello BES retraining volání.</span><span class="sxs-lookup"><span data-stu-id="61254-197">hello URI is generated by combining hello *BaseLocation* and hello *RelativeLocation* from hello output of hello BES retraining call.</span></span>

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

## <a name="import-hello-json-into-a-web-service-definition-object"></a><span data-ttu-id="61254-198">Importujte hello JSON do objektu definice webové služby</span><span class="sxs-lookup"><span data-stu-id="61254-198">Import hello JSON into a Web Service Definition object</span></span>
<span data-ttu-id="61254-199">Je nutné použít hello [Import AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) rutiny tooconvert hello upravit soubor JSON zpět do objektu definice webové služby, které můžete použít tooupdate hello predicative experimentu.</span><span class="sxs-lookup"><span data-stu-id="61254-199">You must use hello [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello modified JSON file back into a Web Service Definition object that you can use tooupdate hello predicative experiment.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service"></a><span data-ttu-id="61254-200">Aktualizovat hello webové služby</span><span class="sxs-lookup"><span data-stu-id="61254-200">Update hello web service</span></span>
<span data-ttu-id="61254-201">Nakonec použijte hello [aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) tooupdate rutiny hello prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="61254-201">Finally, use hello [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello predictive experiment.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
