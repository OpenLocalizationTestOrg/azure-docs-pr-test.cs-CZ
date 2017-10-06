---
title: "aaaConsume pomocí šablony webové aplikace webové služby Machine Learning | Microsoft Docs"
description: "Použití šablony webové aplikace v Azure Marketplace tooconsume prediktivní webové služby v Azure Machine Learning."
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
ms.openlocfilehash: 1199377bead470807d58ca7f7a667175cbb88450
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a><span data-ttu-id="38325-104">Využívání webové služby Azure Machine Learning pomocí šablony webové aplikace</span><span class="sxs-lookup"><span data-stu-id="38325-104">Consume an Azure Machine Learning web service with a web app template</span></span>

<span data-ttu-id="38325-105">Jednou jste vyvinuté prediktivního modelu a nasadit jako Azure webové služby pomocí Machine Learning Studio, nebo pomocí nástrojů, jako je R nebo Python, můžete přístup hello operationalized model pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="38325-105">Once you've developed your predictive model and deployed it as an Azure web service using Machine Learning Studio, or using tools such as R or Python, you can access hello operationalized model using a REST API.</span></span>

<span data-ttu-id="38325-106">Existuje několik způsobů tooconsume hello REST API a přístup hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="38325-106">There are a number of ways tooconsume hello REST API and access hello web service.</span></span> <span data-ttu-id="38325-107">Například můžete napsat aplikaci v jazyce C#, R, nebo Python pomocí hello ukázkový kód pro vás vygeneroval při nasazení hello webové služby (k dispozici v hello [Machine Learning Web Services portálu](https://services.azureml.net/quickstart) nebo v řídicím panelu hello webových služeb v Machine Learning Studio).</span><span class="sxs-lookup"><span data-stu-id="38325-107">For example, you can write an application in C#, R, or Python using hello sample code generated for you when you deployed hello web service (available in hello [Machine Learning Web Services Portal](https://services.azureml.net/quickstart) or in hello web service dashboard in Machine Learning Studio).</span></span> <span data-ttu-id="38325-108">Nebo můžete použít sešit aplikace Excel ukázka hello vytvořená v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="38325-108">Or you can use hello sample Microsoft Excel workbook created for you at hello same time.</span></span>

<span data-ttu-id="38325-109">Ale hello tooaccess nejrychlejší a nejjednodušší způsob je webová služba prostřednictvím hello webové aplikace šablony dostupné na hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span><span class="sxs-lookup"><span data-stu-id="38325-109">But hello quickest and easiest way tooaccess your web service is through hello Web App Templates available in hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-azure-machine-learning-web-app-templates"></a><span data-ttu-id="38325-110">Hello Azure Machine Learning webové aplikace šablony</span><span class="sxs-lookup"><span data-stu-id="38325-110">hello Azure Machine Learning Web App Templates</span></span>
<span data-ttu-id="38325-111">Hello webové aplikace šablony dostupné na hello Azure Marketplace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data webové služby a očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="38325-111">hello web app templates available in hello Azure Marketplace can build a custom web app that knows your web service's input data and expected results.</span></span> <span data-ttu-id="38325-112">Stačí toodo je poskytnout webové služby přístup tooyour hello webové aplikace a data a šablony hello hello rest.</span><span class="sxs-lookup"><span data-stu-id="38325-112">All you need toodo is give hello web app access tooyour web service and data, and hello template does hello rest.</span></span>

<span data-ttu-id="38325-113">Dvě šablony jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="38325-113">Two templates are available:</span></span>

* [<span data-ttu-id="38325-114">Šablony aplikace webové služby Azure ML požadavků a odpovědí</span><span class="sxs-lookup"><span data-stu-id="38325-114">Azure ML Request-Response Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [<span data-ttu-id="38325-115">Šablony aplikace webové služby provedení dávky Azure ML</span><span class="sxs-lookup"><span data-stu-id="38325-115">Azure ML Batch Execution Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

<span data-ttu-id="38325-116">Každá šablona vytvoří ukázkovou aplikaci ASP.NET, pomocí hello API URI a klíč pro webovou službu a nasadí jej jako tooAzure webu.</span><span class="sxs-lookup"><span data-stu-id="38325-116">Each template creates a sample ASP.NET application, using hello API URI and Key for your web service, and deploys it as a web site tooAzure.</span></span> <span data-ttu-id="38325-117">Šablona služby požadavků a odpovědí (záznamy RR) Hello vytvoří webovou aplikaci, která vám umožní toosend jednoho řádku dat toohello webové služby tooget jeden výsledek.</span><span class="sxs-lookup"><span data-stu-id="38325-117">hello Request-Response Service (RRS) template creates a web app that allows you toosend a single row of data toohello web service tooget a single result.</span></span> <span data-ttu-id="38325-118">Hello spuštění služby Batch (BES) šablona vytvoří webovou aplikaci, která vám umožní toosend mnoho řádků dat tooget více výsledků.</span><span class="sxs-lookup"><span data-stu-id="38325-118">hello Batch Execution Service (BES) template creates a web app that allows you toosend many rows of data tooget multiple results.</span></span>

<span data-ttu-id="38325-119">Žádné kódování je požadovaná toouse tyto šablony.</span><span class="sxs-lookup"><span data-stu-id="38325-119">No coding is required toouse these templates.</span></span> <span data-ttu-id="38325-120">Stačí zadat hello klíč rozhraní API a identifikátoru URI a hello šablona vytvoří hello aplikaci za vás.</span><span class="sxs-lookup"><span data-stu-id="38325-120">You just supply hello API Key and URI, and hello template builds hello application for you.</span></span>

<span data-ttu-id="38325-121">klíč tooget hello rozhraní API a URI požadavku pro webovou službu:</span><span class="sxs-lookup"><span data-stu-id="38325-121">tooget hello API key and Request URI for a web service:</span></span>

1. <span data-ttu-id="38325-122">V hello [webový portál služby](https://services.azureml.net/quickstart), novou webovou službu, klikněte na tlačítko **webové služby** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="38325-122">In hello [Web Services Portal](https://services.azureml.net/quickstart), for a New web service, click **Web Services** at hello top.</span></span> <span data-ttu-id="38325-123">Nebo pro web Classic, klikněte na tlačítko služby **Classic webové služby**.</span><span class="sxs-lookup"><span data-stu-id="38325-123">Or for a Classic web service click **Classic Web Services**.</span></span>
2. <span data-ttu-id="38325-124">Klikněte na tlačítko chcete tooaccess hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="38325-124">Click hello web service you want tooaccess.</span></span>
3. <span data-ttu-id="38325-125">Klasické webové služby klikněte na koncový bod hello chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="38325-125">For a Classic web service, click hello endpoint you want tooaccess.</span></span>
4. <span data-ttu-id="38325-126">Klikněte na tlačítko **spotřebě** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="38325-126">Click **Consume** at hello top.</span></span>
5. <span data-ttu-id="38325-127">Kopírování hello **primární** nebo **sekundární klíč** a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="38325-127">Copy hello **Primary** or **Secondary Key** and save it.</span></span>
6. <span data-ttu-id="38325-128">Pokud vytváříte šablonu služby požadavků a odpovědí (záznamy RR), zkopírovat hello **požadavků a odpovědí** URI a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="38325-128">If you're creating a Request-Response Service (RRS) template, copy hello **Request-Response** URI and save it.</span></span> <span data-ttu-id="38325-129">Pokud vytváříte šablonu spuštění služby Batch (BES), zkopírovat hello **dávkových žádostí** URI a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="38325-129">If you're creating a Batch Execution Service (BES) template, copy hello **Batch Requests** URI and save it.</span></span>


## <a name="how-toouse-hello-request-response-service-rrs-template"></a><span data-ttu-id="38325-130">Jak toouse hello šablony služby požadavků a odpovědí (záznamy RR)</span><span class="sxs-lookup"><span data-stu-id="38325-130">How toouse hello Request-Response Service (RRS) template</span></span>
<span data-ttu-id="38325-131">Postupujte podle těchto kroků toouse hello RRS webové aplikace šablony, jak je znázorněno v následujícím diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="38325-131">Follow these steps toouse hello RRS web app template, as shown in hello following diagram.</span></span>

![Proces toouse RRS webové šablony][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. <span data-ttu-id="38325-133">Přejděte toohello [portál Azure](https://portal.azure.com), **přihlášení**, klikněte na tlačítko **nový**, vyhledejte a vyberte **webové aplikace Azure ML požadavků a odpovědí Service**, pak klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38325-133">Go toohello [Azure portal](https://portal.azure.com), **Login**, click **New**, search for and select **Azure ML Request-Response Service Web App**, then click **Create**.</span></span> 
   
   * <span data-ttu-id="38325-134">Zadejte jedinečný název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="38325-134">Give your web app a unique name.</span></span> <span data-ttu-id="38325-135">Adresa URL Hello hello webové aplikace bude tento název, za nímž následuje `.azurewebsites.net.` například`http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="38325-135">hello URL of hello web app will be this name followed by `.azurewebsites.net.` For example, `http://carprediction.azurewebsites.net.`</span></span>
   * <span data-ttu-id="38325-136">Vyberte hello předplatného Azure a služby, pod kterým běží webové služby.</span><span class="sxs-lookup"><span data-stu-id="38325-136">Select hello Azure subscription and services under which your web service is running.</span></span>
   * <span data-ttu-id="38325-137">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38325-137">Click **Create**.</span></span>
     
     ![Vytvoření webové aplikace][image5]

4. <span data-ttu-id="38325-139">Po dokončení nasazení hello webové aplikace Azure, klikněte na tlačítko hello **URL** na hello webové stránky nastavení aplikace v Azure, nebo zadejte adresu URL hello ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="38325-139">When Azure has finished deploying hello web app, click hello **URL** on hello web app settings page in Azure, or enter hello URL in a web browser.</span></span> <span data-ttu-id="38325-140">Například `http://carprediction.azurewebsites.net.`.</span><span class="sxs-lookup"><span data-stu-id="38325-140">For example, `http://carprediction.azurewebsites.net.`</span></span>
5. <span data-ttu-id="38325-141">Když hello spustí první webové aplikace se zobrazí výzvu k hello **Post URL rozhraní API** a **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="38325-141">When hello web app first runs it will ask you for hello **API Post URL** and **API Key**.</span></span>
   <span data-ttu-id="38325-142">Zadejte hello hodnoty, které jste předtím uložili (**URI požadavku** a **klíč rozhraní API**, v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="38325-142">Enter hello values you saved earlier (**Request URI** and **API Key**, respectively).</span></span>
     
     <span data-ttu-id="38325-143">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="38325-143">Click **Submit**.</span></span>
     
     ![Zadejte Post URI a klíč rozhraní API][image6]

6. <span data-ttu-id="38325-145">Hello webové aplikace zobrazí jeho **konfiguraci webové aplikace** stránka s hello aktuální nastavení webové služby.</span><span class="sxs-lookup"><span data-stu-id="38325-145">hello web app displays its **Web App Configuration** page with hello current web service settings.</span></span> <span data-ttu-id="38325-146">Zde můžete provádět změny nastavení toohello používané hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="38325-146">Here you can make changes toohello settings used by hello web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="38325-147">Nastavení hello zde pouze změna pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38325-147">Changing hello settings here only changes them for this web app.</span></span> <span data-ttu-id="38325-148">Neprojeví hello výchozí nastavení webové služby.</span><span class="sxs-lookup"><span data-stu-id="38325-148">It doesn't change hello default settings of your web service.</span></span> <span data-ttu-id="38325-149">Například, pokud změníte hello **popis** zde nemění hello Popis zobrazený na řídicí panel hello webové služby v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="38325-149">For example, if you change hello **Description** here it doesn't change hello description shown on hello web service dashboard in Machine Learning Studio.</span></span>
   > 
   > 
   
    <span data-ttu-id="38325-150">Když jste hotovi, klikněte na tlačítko **uložit změny**a potom klikněte na **přejděte tooHome stránky**.</span><span class="sxs-lookup"><span data-stu-id="38325-150">When you're done, click **Save changes**, and then click **Go tooHome Page**.</span></span>

7. <span data-ttu-id="38325-151">Z hello domovské stránky, které můžete zadat hodnoty toosend tooyour webové služby.</span><span class="sxs-lookup"><span data-stu-id="38325-151">From hello home page you can enter values toosend tooyour web service.</span></span> <span data-ttu-id="38325-152">Klikněte na tlačítko **odeslání** když jste hotovi, a bude vrácen hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="38325-152">Click **Submit** when you're done, and hello result will be returned.</span></span>

<span data-ttu-id="38325-153">Pokud chcete, aby tooreturn toohello **konfigurace** stránky, přejděte toohello `setting.aspx` stránku hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="38325-153">If you want tooreturn toohello **Configuration** page, go toohello `setting.aspx` page of hello web app.</span></span> <span data-ttu-id="38325-154">Příklad: `http://carprediction.azurewebsites.net/setting.aspx.` bude výzvami tooenter hello API klíč znovu – budete potřebovat, tooaccess hello stránky a aktualizovat nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="38325-154">For example: `http://carprediction.azurewebsites.net/setting.aspx.` You will be prompted tooenter hello API key again - you need that tooaccess hello page and update hello settings.</span></span>

<span data-ttu-id="38325-155">Můžete zastavit, restartovat nebo odstranit hello webové aplikace ve hello portálu Azure stejně jako jiná webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="38325-155">You can stop, restart, or delete hello web app in hello Azure portal like any other web app.</span></span> <span data-ttu-id="38325-156">Tak dlouho, dokud běží můžete procházet toohello domácí webovou adresu a zadejte nové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="38325-156">As long as it is running you can browse toohello home web address and enter new values.</span></span>

## <a name="how-toouse-hello-batch-execution-service-bes-template"></a><span data-ttu-id="38325-157">Jak toouse hello šablony spuštění služby Batch (BES)</span><span class="sxs-lookup"><span data-stu-id="38325-157">How toouse hello Batch Execution Service (BES) template</span></span>
<span data-ttu-id="38325-158">Můžete použít hello BES šablony webové aplikace v hello stejný způsobem jako hello záznamy o prostředku šablony, s výjimkou tohoto hello webovou aplikaci, která je vytvořena vám umožní toosubmit více řádků dat a příjem více výsledků.</span><span class="sxs-lookup"><span data-stu-id="38325-158">You can use hello BES web app template in hello same way as hello RRS template, except that hello web app that's created will allow you toosubmit multiple rows of data and receive multiple results.</span></span>

<span data-ttu-id="38325-159">Hello vstupní hodnoty pro spuštění webové služby batch mohou pocházet z úložiště Azure nebo místního souboru; Hello výsledky ukládat do kontejneru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="38325-159">hello input values for a batch execution web service can come from Azure storage or a local file; hello results are stored in an Azure storage container.</span></span>
<span data-ttu-id="38325-160">Ano, budete potřebovat toohold kontejneru úložiště Azure hello výsledků vrácených hello webové aplikace a budete potřebovat tooget vstupní data připravena.</span><span class="sxs-lookup"><span data-stu-id="38325-160">So, you'll need an Azure storage container toohold hello results returned by hello web app, and you'll need tooget your input data ready.</span></span>

![Zpracování toouse BES webové šablony][image2]

1. <span data-ttu-id="38325-162">Postupujte podle hello stejný postup toocreate hello BES webová aplikace jako šablony hello RRS, s výjimkou přejděte příliš[Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES šablony v Azure Marketplace a klikněte na tlačítko **vytvoření webové aplikace** .</span><span class="sxs-lookup"><span data-stu-id="38325-162">Follow hello same procedure toocreate hello BES web app as for hello RRS template, except go too[Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES template on Azure Marketplace and click **Create Web App**.</span></span>

2. <span data-ttu-id="38325-163">toospecify místo hello výsledky uložené, zadejte informace o kontejneru cílové hello na domovskou stránku hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="38325-163">toospecify where you want hello results stored, enter hello destination container information on hello web app home page.</span></span> <span data-ttu-id="38325-164">Také určete, kde hello webovou aplikaci můžete získat hello vstupní hodnoty, buď v případě místního souboru nebo kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="38325-164">Also specify where hello web app can get hello input values, either in a local file or an Azure storage container.</span></span>
   <span data-ttu-id="38325-165">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="38325-165">Click **Submit**.</span></span>
   
    ![Informace o úložiště][image7]

<span data-ttu-id="38325-167">Hello webové aplikace se zobrazí stránka s stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="38325-167">hello web app will display a page with job status.</span></span>
<span data-ttu-id="38325-168">Po dokončení úlohy hello budete mít k dispozici hello umístění hello výsledky v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="38325-168">When hello job has completed you'll be given hello location of hello results in Azure blob storage.</span></span> <span data-ttu-id="38325-169">Máte také možnost hello stahování hello výsledky tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="38325-169">You also have hello option of downloading hello results tooa local file.</span></span>

## <a name="for-more-information"></a><span data-ttu-id="38325-170">Další informace</span><span class="sxs-lookup"><span data-stu-id="38325-170">For more information</span></span>
<span data-ttu-id="38325-171">Další informace o toolearn...</span><span class="sxs-lookup"><span data-stu-id="38325-171">toolearn more about...</span></span>

* <span data-ttu-id="38325-172">Vytvoření experimentu machine learning s Machine Learning Studio najdete v části [vytvoření prvního experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md)</span><span class="sxs-lookup"><span data-stu-id="38325-172">creating a machine learning experiment with Machine Learning Studio, see [Create your first experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)</span></span>
* <span data-ttu-id="38325-173">jak zjistit, toodeploy strojové učení experimentovat jako webovou službu, [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md)</span><span class="sxs-lookup"><span data-stu-id="38325-173">how toodeploy your machine learning experiment as a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md)</span></span>
* <span data-ttu-id="38325-174">jiné způsoby tooaccess vaší webové služby, najdete v části [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="38325-174">other ways tooaccess your web service, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span></span>

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
