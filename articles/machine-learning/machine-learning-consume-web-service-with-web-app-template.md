---
title: "Využívat pomocí šablony webové aplikace webové služby Machine Learning | Microsoft Docs"
description: "Použití šablony webové aplikace v Azure Marketplace využívat prediktivní webové služby v Azure Machine Learning."
keywords: "Webová služba, operationalization, REST API strojového učení"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 95aa1fa23d83ec0dcd00870179167e803bafbd16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a><span data-ttu-id="717f9-104">Využívání webové služby Azure Machine Learning pomocí šablony webové aplikace</span><span class="sxs-lookup"><span data-stu-id="717f9-104">Consume an Azure Machine Learning web service with a web app template</span></span>

<span data-ttu-id="717f9-105">Jednou jste vyvinuté prediktivního modelu a nasadit jako Azure webové služby pomocí Machine Learning Studio, nebo pomocí nástrojů, jako je R nebo Python, můžete přístup operationalized modelu s použitím rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="717f9-105">Once you've developed your predictive model and deployed it as an Azure web service using Machine Learning Studio, or using tools such as R or Python, you can access the operationalized model using a REST API.</span></span>

<span data-ttu-id="717f9-106">Existuje několik způsobů, jak používat rozhraní REST API a přístup k webové službě.</span><span class="sxs-lookup"><span data-stu-id="717f9-106">There are a number of ways to consume the REST API and access the web service.</span></span> <span data-ttu-id="717f9-107">Můžete například napsat aplikaci v jazyce C#, R nebo Python pomocí ukázkový kód pro vás vygeneroval při nasazení webové služby (k dispozici v [Machine Learning Web Services portálu](https://services.azureml.net/quickstart) nebo v řídicím panelu webové služby v nástroji Machine Learning Studio).</span><span class="sxs-lookup"><span data-stu-id="717f9-107">For example, you can write an application in C#, R, or Python using the sample code generated for you when you deployed the web service (available in the [Machine Learning Web Services Portal](https://services.azureml.net/quickstart) or in the web service dashboard in Machine Learning Studio).</span></span> <span data-ttu-id="717f9-108">Nebo můžete použít sešit aplikace Excel ukázka vytvořili pro vás ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="717f9-108">Or you can use the sample Microsoft Excel workbook created for you at the same time.</span></span>

<span data-ttu-id="717f9-109">Ale nejrychlejší a nejsnazší způsob pro přístup k webové služby je prostřednictvím šablony webové aplikace k dispozici v [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span><span class="sxs-lookup"><span data-stu-id="717f9-109">But the quickest and easiest way to access your web service is through the Web App Templates available in the [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a><span data-ttu-id="717f9-110">Azure Machine Learning šablony webové aplikace</span><span class="sxs-lookup"><span data-stu-id="717f9-110">The Azure Machine Learning Web App Templates</span></span>
<span data-ttu-id="717f9-111">Webové aplikace šablony dostupné v Azure Marketplace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data webové služby a očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="717f9-111">The web app templates available in the Azure Marketplace can build a custom web app that knows your web service's input data and expected results.</span></span> <span data-ttu-id="717f9-112">Jediné, co musíte udělat je poskytnout přístup k webové aplikaci pro webové služby a data a šablona nemá rest.</span><span class="sxs-lookup"><span data-stu-id="717f9-112">All you need to do is give the web app access to your web service and data, and the template does the rest.</span></span>

<span data-ttu-id="717f9-113">Dvě šablony jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="717f9-113">Two templates are available:</span></span>

* [<span data-ttu-id="717f9-114">Šablony aplikace webové služby Azure ML požadavků a odpovědí</span><span class="sxs-lookup"><span data-stu-id="717f9-114">Azure ML Request-Response Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [<span data-ttu-id="717f9-115">Šablony aplikace webové služby provedení dávky Azure ML</span><span class="sxs-lookup"><span data-stu-id="717f9-115">Azure ML Batch Execution Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

<span data-ttu-id="717f9-116">Každá šablona vytvoří ukázkovou aplikaci ASP.NET, pomocí rozhraní API URI a klíč pro webovou službu a nasadí jej jako webového serveru do Azure.</span><span class="sxs-lookup"><span data-stu-id="717f9-116">Each template creates a sample ASP.NET application, using the API URI and Key for your web service, and deploys it as a web site to Azure.</span></span> <span data-ttu-id="717f9-117">Šablona služby požadavků a odpovědí (záznamy RR) vytvoří webovou aplikaci, která umožňuje odesílání jednoho řádku dat k webové službě získat jeden výsledek.</span><span class="sxs-lookup"><span data-stu-id="717f9-117">The Request-Response Service (RRS) template creates a web app that allows you to send a single row of data to the web service to get a single result.</span></span> <span data-ttu-id="717f9-118">Spuštění služby Batch (BES) šablona vytvoří webovou aplikaci, která umožňuje odeslat mnoho řádků dat. Chcete-li získat více výsledků.</span><span class="sxs-lookup"><span data-stu-id="717f9-118">The Batch Execution Service (BES) template creates a web app that allows you to send many rows of data to get multiple results.</span></span>

<span data-ttu-id="717f9-119">Žádné kódování je potřeba použít tyto šablony.</span><span class="sxs-lookup"><span data-stu-id="717f9-119">No coding is required to use these templates.</span></span> <span data-ttu-id="717f9-120">Stačí zadat klíč rozhraní API a identifikátor URI a šablona vytvoří aplikaci za vás.</span><span class="sxs-lookup"><span data-stu-id="717f9-120">You just supply the API Key and URI, and the template builds the application for you.</span></span>

<span data-ttu-id="717f9-121">Pokud chcete získat klíč rozhraní API a URI požadavku pro webovou službu:</span><span class="sxs-lookup"><span data-stu-id="717f9-121">To get the API key and Request URI for a web service:</span></span>

1. <span data-ttu-id="717f9-122">V [webový portál služby](https://services.azureml.net/quickstart), novou webovou službu, klikněte na tlačítko **webové služby** v horní části.</span><span class="sxs-lookup"><span data-stu-id="717f9-122">In the [Web Services Portal](https://services.azureml.net/quickstart), for a New web service, click **Web Services** at the top.</span></span> <span data-ttu-id="717f9-123">Nebo pro web Classic, klikněte na tlačítko služby **Classic webové služby**.</span><span class="sxs-lookup"><span data-stu-id="717f9-123">Or for a Classic web service click **Classic Web Services**.</span></span>
2. <span data-ttu-id="717f9-124">Klikněte na tlačítko webové služby, kterou chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="717f9-124">Click the web service you want to access.</span></span>
3. <span data-ttu-id="717f9-125">Pro webovou službu Classic klikněte na koncový bod, které chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="717f9-125">For a Classic web service, click the endpoint you want to access.</span></span>
4. <span data-ttu-id="717f9-126">Klikněte na tlačítko **spotřebě** v horní části.</span><span class="sxs-lookup"><span data-stu-id="717f9-126">Click **Consume** at the top.</span></span>
5. <span data-ttu-id="717f9-127">Kopírování **primární** nebo **sekundární klíč** a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="717f9-127">Copy the **Primary** or **Secondary Key** and save it.</span></span>
6. <span data-ttu-id="717f9-128">Pokud vytváříte šablonu služby požadavků a odpovědí (záznamy RR), zkopírovat **požadavků a odpovědí** URI a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="717f9-128">If you're creating a Request-Response Service (RRS) template, copy the **Request-Response** URI and save it.</span></span> <span data-ttu-id="717f9-129">Pokud vytváříte šablonu spuštění služby Batch (BES), zkopírovat **dávkových žádostí** URI a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="717f9-129">If you're creating a Batch Execution Service (BES) template, copy the **Batch Requests** URI and save it.</span></span>


## <a name="how-to-use-the-request-response-service-rrs-template"></a><span data-ttu-id="717f9-130">Použití šablony služby požadavků a odpovědí (záznamy RR)</span><span class="sxs-lookup"><span data-stu-id="717f9-130">How to use the Request-Response Service (RRS) template</span></span>
<span data-ttu-id="717f9-131">Použijte následující postup použijte šablony webové aplikace RRS, jak je znázorněno v následujícím diagramu.</span><span class="sxs-lookup"><span data-stu-id="717f9-131">Follow these steps to use the RRS web app template, as shown in the following diagram.</span></span>

![Postup použití RRS webové šablony][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. <span data-ttu-id="717f9-133">Přejděte na [portál Azure](https://portal.azure.com), **přihlášení**, klikněte na tlačítko **nový**, vyhledejte a vyberte **webové aplikace Azure ML požadavků a odpovědí Service**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="717f9-133">Go to the [Azure portal](https://portal.azure.com), **Login**, click **New**, search for and select **Azure ML Request-Response Service Web App**, then click **Create**.</span></span> 
   
   * <span data-ttu-id="717f9-134">Zadejte jedinečný název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="717f9-134">Give your web app a unique name.</span></span> <span data-ttu-id="717f9-135">Adresa URL webové aplikace bude tento název, za nímž následuje `.azurewebsites.net.` například`http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="717f9-135">The URL of the web app will be this name followed by `.azurewebsites.net.` For example, `http://carprediction.azurewebsites.net.`</span></span>
   * <span data-ttu-id="717f9-136">Vyberte předplatné Azure a služby, pod kterým běží webové služby.</span><span class="sxs-lookup"><span data-stu-id="717f9-136">Select the Azure subscription and services under which your web service is running.</span></span>
   * <span data-ttu-id="717f9-137">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="717f9-137">Click **Create**.</span></span>
     
     ![Vytvoření webové aplikace][image5]

4. <span data-ttu-id="717f9-139">Po dokončení nasazení webové aplikace Azure klikněte **URL** v nastavení aplikace webové stránky v Azure nebo ve webovém prohlížeči zadejte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="717f9-139">When Azure has finished deploying the web app, click the **URL** on the web app settings page in Azure, or enter the URL in a web browser.</span></span> <span data-ttu-id="717f9-140">Například `http://carprediction.azurewebsites.net.`.</span><span class="sxs-lookup"><span data-stu-id="717f9-140">For example, `http://carprediction.azurewebsites.net.`</span></span>
5. <span data-ttu-id="717f9-141">Při prvním spuštění webové aplikace se zobrazí dotaz **Post URL rozhraní API** a **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="717f9-141">When the web app first runs it will ask you for the **API Post URL** and **API Key**.</span></span>
   <span data-ttu-id="717f9-142">Zadejte hodnoty, které jste předtím uložili (**URI požadavku** a **klíč rozhraní API**, v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="717f9-142">Enter the values you saved earlier (**Request URI** and **API Key**, respectively).</span></span>
     
     <span data-ttu-id="717f9-143">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="717f9-143">Click **Submit**.</span></span>
     
     ![Zadejte Post URI a klíč rozhraní API][image6]

6. <span data-ttu-id="717f9-145">Zobrazí aplikace webové jeho **konfiguraci webové aplikace** stránka s aktuální nastavení webové služby.</span><span class="sxs-lookup"><span data-stu-id="717f9-145">The web app displays its **Web App Configuration** page with the current web service settings.</span></span> <span data-ttu-id="717f9-146">Sem můžete provést změny nastavení používaná při webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="717f9-146">Here you can make changes to the settings used by the web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="717f9-147">Nastavení zde pouze změna pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="717f9-147">Changing the settings here only changes them for this web app.</span></span> <span data-ttu-id="717f9-148">Výchozí nastavení webové služby neprojeví.</span><span class="sxs-lookup"><span data-stu-id="717f9-148">It doesn't change the default settings of your web service.</span></span> <span data-ttu-id="717f9-149">Například, pokud změníte **popis** zde nemění Popis zobrazený na řídicím panelu webové služby v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="717f9-149">For example, if you change the **Description** here it doesn't change the description shown on the web service dashboard in Machine Learning Studio.</span></span>
   > 
   > 
   
    <span data-ttu-id="717f9-150">Když jste hotovi, klikněte na tlačítko **uložit změny**a potom klikněte na **přejít na domovskou stránku**.</span><span class="sxs-lookup"><span data-stu-id="717f9-150">When you're done, click **Save changes**, and then click **Go to Home Page**.</span></span>

7. <span data-ttu-id="717f9-151">Na domovské stránce můžete zadat hodnoty k odeslání do webové služby.</span><span class="sxs-lookup"><span data-stu-id="717f9-151">From the home page you can enter values to send to your web service.</span></span> <span data-ttu-id="717f9-152">Klikněte na tlačítko **odeslání** když jste hotovi, a vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="717f9-152">Click **Submit** when you're done, and the result will be returned.</span></span>

<span data-ttu-id="717f9-153">Pokud chcete vrátit **konfigurace** stránky, přejděte na `setting.aspx` stránku webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="717f9-153">If you want to return to the **Configuration** page, go to the `setting.aspx` page of the web app.</span></span> <span data-ttu-id="717f9-154">Příklad: `http://carprediction.azurewebsites.net/setting.aspx.` zobrazí se výzva k opakovanému zadání klíč rozhraní API – budete potřebovat, který pro přístup k stránce a aktualizujte nastavení.</span><span class="sxs-lookup"><span data-stu-id="717f9-154">For example: `http://carprediction.azurewebsites.net/setting.aspx.` You will be prompted to enter the API key again - you need that to access the page and update the settings.</span></span>

<span data-ttu-id="717f9-155">Můžete zastavit, restartovat nebo odstranit webovou aplikaci na portálu Azure stejně jako jiná webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="717f9-155">You can stop, restart, or delete the web app in the Azure portal like any other web app.</span></span> <span data-ttu-id="717f9-156">Tak dlouho, dokud běží můžete procházet domácí webovou adresu a zadejte nové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="717f9-156">As long as it is running you can browse to the home web address and enter new values.</span></span>

## <a name="how-to-use-the-batch-execution-service-bes-template"></a><span data-ttu-id="717f9-157">Použití šablony spuštění služby Batch (BES)</span><span class="sxs-lookup"><span data-stu-id="717f9-157">How to use the Batch Execution Service (BES) template</span></span>
<span data-ttu-id="717f9-158">Šablona BES webové aplikace můžete použít stejným způsobem jako šablonu RRS, s tím rozdílem, že webová aplikace, která je vytvořena vám umožní odeslat více řádků dat a příjem více výsledků.</span><span class="sxs-lookup"><span data-stu-id="717f9-158">You can use the BES web app template in the same way as the RRS template, except that the web app that's created will allow you to submit multiple rows of data and receive multiple results.</span></span>

<span data-ttu-id="717f9-159">Vstupní hodnoty pro spuštění webové služby batch mohou pocházet z úložiště Azure nebo místního souboru; výsledky jsou uloženy v kontejneru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="717f9-159">The input values for a batch execution web service can come from Azure storage or a local file; the results are stored in an Azure storage container.</span></span>
<span data-ttu-id="717f9-160">Ano budete potřebovat kontejner úložiště Azure pro uložení výsledků vrácených webové aplikace a budete muset připravit vstupní data.</span><span class="sxs-lookup"><span data-stu-id="717f9-160">So, you'll need an Azure storage container to hold the results returned by the web app, and you'll need to get your input data ready.</span></span>

![Postup použití BES webové šablony][image2]

1. <span data-ttu-id="717f9-162">Postupujte stejným způsobem, k vytvoření webové aplikace BES jako záznamy o prostředku šablony, s výjimkou, přejděte na [Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) BES šablony na webu Azure Marketplace otevřete a kliknete na **vytvořit webovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="717f9-162">Follow the same procedure to create the BES web app as for the RRS template, except go to [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) to open the BES template on Azure Marketplace and click **Create Web App**.</span></span>

2. <span data-ttu-id="717f9-163">K určení, kde chcete výsledky uložené, zadejte informace o cílovém kontejneru na domovskou stránku webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="717f9-163">To specify where you want the results stored, enter the destination container information on the web app home page.</span></span> <span data-ttu-id="717f9-164">Také určete, kde webové aplikace můžete získat vstupní hodnoty, buď v případě místního souboru nebo kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="717f9-164">Also specify where the web app can get the input values, either in a local file or an Azure storage container.</span></span>
   <span data-ttu-id="717f9-165">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="717f9-165">Click **Submit**.</span></span>
   
    ![Informace o úložiště][image7]

<span data-ttu-id="717f9-167">Webové aplikace se zobrazí stránka s stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="717f9-167">The web app will display a page with job status.</span></span>
<span data-ttu-id="717f9-168">Po dokončení úlohy budete mít k dispozici umístění výsledky v úložišti objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="717f9-168">When the job has completed you'll be given the location of the results in Azure blob storage.</span></span> <span data-ttu-id="717f9-169">Máte také možnost stahování výsledky do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="717f9-169">You also have the option of downloading the results to a local file.</span></span>

## <a name="for-more-information"></a><span data-ttu-id="717f9-170">Další informace</span><span class="sxs-lookup"><span data-stu-id="717f9-170">For more information</span></span>
<span data-ttu-id="717f9-171">Další informace o...</span><span class="sxs-lookup"><span data-stu-id="717f9-171">To learn more about...</span></span>

* <span data-ttu-id="717f9-172">Vytvoření experimentu machine learning s Machine Learning Studio najdete v části [vytvoření prvního experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md)</span><span class="sxs-lookup"><span data-stu-id="717f9-172">creating a machine learning experiment with Machine Learning Studio, see [Create your first experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)</span></span>
* <span data-ttu-id="717f9-173">tom, jak nasadit experimentu machine learning jako webovou službu, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md)</span><span class="sxs-lookup"><span data-stu-id="717f9-173">how to deploy your machine learning experiment as a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md)</span></span>
* <span data-ttu-id="717f9-174">Další způsoby pro přístup k vaší webové služby, najdete v části [využívání Azure Machine Learning webové služby](machine-learning-consume-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="717f9-174">other ways to access your web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span></span>

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
