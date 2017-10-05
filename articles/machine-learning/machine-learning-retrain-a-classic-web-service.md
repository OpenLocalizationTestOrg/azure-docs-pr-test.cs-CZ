---
title: "Přeučování Classic webové služby | Microsoft Docs"
description: "Zjistěte, jak programově přeučit modelu a aktualizovat webovou službu, která používá nově trénovaného modelu v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3f9f4cd5ed36262845f7a3139073ccd4e49f472a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-classic-web-service"></a><span data-ttu-id="6dca4-103">Přeučování webové služby Classic</span><span class="sxs-lookup"><span data-stu-id="6dca4-103">Retrain a Classic web service</span></span>
<span data-ttu-id="6dca4-104">Prediktivní webové služby, které jste nasadili je výchozí vyhodnocovací koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6dca4-104">The Predictive Web Service you deployed is the default scoring endpoint.</span></span> <span data-ttu-id="6dca4-105">Výchozí koncové body jsou synchronizovány s původní školení a vyhodnocování experimenty, a proto nelze nahradit pro cvičný model pro výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6dca4-105">Default endpoints are kept in sync with the original training and scoring experiments, and therefore the trained model for the default endpoint cannot be replaced.</span></span> <span data-ttu-id="6dca4-106">Chcete-li přeučování webovou službu, je nutné přidat nový koncový bod k webové službě.</span><span class="sxs-lookup"><span data-stu-id="6dca4-106">To retrain the web service, you must add a new endpoint to the web service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6dca4-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6dca4-107">Prerequisites</span></span>
<span data-ttu-id="6dca4-108">Musí mít nastavíte výukový experiment a prediktivní experiment jak je znázorněno v [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="6dca4-108">You must have set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6dca4-109">Prediktivní experiment musí být nasazený jako Classic strojového učení webové služby.</span><span class="sxs-lookup"><span data-stu-id="6dca4-109">The predictive experiment must be deployed as a Classic machine learning web service.</span></span> 
> 
> 

<span data-ttu-id="6dca4-110">Další informace o nasazení webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6dca4-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="add-a-new-endpoint"></a><span data-ttu-id="6dca4-111">Přidat nový koncový bod</span><span class="sxs-lookup"><span data-stu-id="6dca4-111">Add a new Endpoint</span></span>
<span data-ttu-id="6dca4-112">Prediktivní webové služby, která jste nasadili obsahuje výchozí vyhodnocovací koncový bod, který je uložen synchronizována s původní školení a vyhodnocování experimenty trained model.</span><span class="sxs-lookup"><span data-stu-id="6dca4-112">The Predictive Web Service that you deployed contains a default scoring endpoint that is kept in sync with the original training and scoring experiments trained model.</span></span> <span data-ttu-id="6dca4-113">Pokud chcete aktualizovat webovou službu, která se nový trained model, musíte vytvořit nový koncový bod vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="6dca4-113">To update your web service to with a new trained model, you must create a new scoring endpoint.</span></span> 

<span data-ttu-id="6dca4-114">Chcete-li vytvořit nový koncový bod vyhodnocování, na prediktivní webové služby, která mohou být aktualizovány s trénovaného modelu:</span><span class="sxs-lookup"><span data-stu-id="6dca4-114">To create a new scoring endpoint, on the Predictive Web Service that can be updated with the trained model:</span></span>

> [!NOTE]
> <span data-ttu-id="6dca4-115">Ujistěte se, že přidáváte koncový bod do prediktivní webové služby není školení webovou službu.</span><span class="sxs-lookup"><span data-stu-id="6dca4-115">Be sure you are adding the endpoint to the Predictive Web Service, not the Training Web Service.</span></span> <span data-ttu-id="6dca4-116">Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="6dca4-116">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate web services listed.</span></span> <span data-ttu-id="6dca4-117">Prediktivní webové služby musí končit "[prediktivní exp.]".</span><span class="sxs-lookup"><span data-stu-id="6dca4-117">The Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

<span data-ttu-id="6dca4-118">Existují tři způsoby, ve které můžete přidat nový koncový bod webové služby:</span><span class="sxs-lookup"><span data-stu-id="6dca4-118">There are three ways in which you can add a new end point to a web service:</span></span>

1. <span data-ttu-id="6dca4-119">Prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="6dca4-119">Programmatically</span></span>
2. <span data-ttu-id="6dca4-120">Použití portálu Microsoft Azure webové služby</span><span class="sxs-lookup"><span data-stu-id="6dca4-120">Use the Microsoft Azure Web Services portal</span></span>
3. <span data-ttu-id="6dca4-121">Použití portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="6dca4-121">Use the Azure classic portal</span></span>

### <a name="programmatically-add-an-endpoint"></a><span data-ttu-id="6dca4-122">Přidání koncového bodu prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="6dca4-122">Programmatically add an endpoint</span></span>
<span data-ttu-id="6dca4-123">Můžete přidat vyhodnocování koncové body pomocí ukázkový kód zadaný v tomto [úložiště github](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="6dca4-123">You can add scoring endpoints using the sample code provided in this [github repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span></span>

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a><span data-ttu-id="6dca4-124">Přidání koncového bodu pomocí portálu Microsoft Azure webové služby</span><span class="sxs-lookup"><span data-stu-id="6dca4-124">Use the Microsoft Azure Web Services portal to add an endpoint</span></span>
1. <span data-ttu-id="6dca4-125">Machine Learning Studio v levém navigačním sloupec, klikněte na možnost webové služby.</span><span class="sxs-lookup"><span data-stu-id="6dca4-125">In Machine Learning Studio, on the left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="6dca4-126">V dolní části řídicího panelu webové služby, klikněte na tlačítko **spravovat koncové body preview**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-126">At the bottom of the web service dashboard, click **Manage endpoints preview**.</span></span>
3. <span data-ttu-id="6dca4-127">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-127">Click **Add**.</span></span>
4. <span data-ttu-id="6dca4-128">Zadejte název a popis pro nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6dca4-128">Type a name and description for the new endpoint.</span></span> <span data-ttu-id="6dca4-129">Vyberte úroveň protokolování a určuje, jestli je povolená ukázková data.</span><span class="sxs-lookup"><span data-stu-id="6dca4-129">Select the logging level and whether sample data is enabled.</span></span> <span data-ttu-id="6dca4-130">Další informace o protokolování naleznete v tématu [povolení protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="6dca4-130">For more information on logging, see [Enable logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>

### <a name="use-the-azure-classic-portal-to-add-an-endpoint"></a><span data-ttu-id="6dca4-131">Použití portálu Azure classic k přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="6dca4-131">Use the Azure classic portal to add an endpoint</span></span>
1. <span data-ttu-id="6dca4-132">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6dca4-132">Sign in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="6dca4-133">V nabídce vlevo klikněte na **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-133">In the left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="6dca4-134">Pod názvem, klikněte na pracovní prostor a potom klikněte na **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-134">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="6dca4-135">Pod názvem, klikněte na **modelu sčítání [prediktivní exp.]** .</span><span class="sxs-lookup"><span data-stu-id="6dca4-135">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="6dca4-136">V dolní části stránky klikněte na tlačítko **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-136">At the bottom of the page, click **Add Endpoint**.</span></span> <span data-ttu-id="6dca4-137">Další informace o přidání koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6dca4-137">For more information on adding endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span> 

## <a name="update-the-added-endpoints-trained-model"></a><span data-ttu-id="6dca4-138">Aktualizovat Trained Model přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="6dca4-138">Update the added endpoint’s Trained Model</span></span>
<span data-ttu-id="6dca4-139">Dokončete proces retraining, je třeba aktualizovat pro cvičný model nový koncový bod, který jste přidali.</span><span class="sxs-lookup"><span data-stu-id="6dca4-139">To complete the retraining process, you must update the trained model of the new endpoint that you added.</span></span>

* <span data-ttu-id="6dca4-140">Pokud jste přidali nový koncový bod pomocí portálu Azure classic, můžete kliknout na nový koncový bod název na portálu, pak se **operace UpdateResource** odkaz získat adresu URL, budete muset aktualizovat model pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6dca4-140">If you added the new endpoint using the classic Azure portal, you can click the new endpoint's name in the portal, then the **UpdateResource** link to get the URL you would need to update the endpoint's model.</span></span>
* <span data-ttu-id="6dca4-141">Pokud jste přidali koncový bod pomocí ukázkový kód, jedná se o umístění adresa URL nápovědy identifikovaný *HelpLocationURL* hodnotu ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="6dca4-141">If you added the endpoint using the sample code, this includes location of the help URL identified by the *HelpLocationURL* value in the output.</span></span>

<span data-ttu-id="6dca4-142">Načíst cestu adresy URL:</span><span class="sxs-lookup"><span data-stu-id="6dca4-142">To retrieve the path URL:</span></span>

1. <span data-ttu-id="6dca4-143">Zkopírujte a vložte adresu URL do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6dca4-143">Copy and paste the URL into your browser.</span></span>
2. <span data-ttu-id="6dca4-144">Klikněte na odkaz aktualizace prostředků.</span><span class="sxs-lookup"><span data-stu-id="6dca4-144">Click the Update Resource link.</span></span>
3. <span data-ttu-id="6dca4-145">Zkopírujte adresu URL POST požadavku PATCH.</span><span class="sxs-lookup"><span data-stu-id="6dca4-145">Copy the POST URL of the PATCH request.</span></span> <span data-ttu-id="6dca4-146">Například:</span><span class="sxs-lookup"><span data-stu-id="6dca4-146">For example:</span></span>
   
     <span data-ttu-id="6dca4-147">ADRESA URL OPRAVY: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2</span><span class="sxs-lookup"><span data-stu-id="6dca4-147">PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2</span></span>

<span data-ttu-id="6dca4-148">Nyní můžete trénovaného modelu k aktualizaci vyhodnocování koncového bodu, který jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6dca4-148">You can now use the trained model to update the scoring endpoint that you created previously.</span></span>

<span data-ttu-id="6dca4-149">Následující vzorový kód ukazuje, jak používat *BaseLocation*, *RelativeLocation*, *SasBlobToken*a oprava adresu URL pro aktualizaci koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="6dca4-149">The following sample code shows you how to use the *BaseLocation*, *RelativeLocation*, *SasBlobToken*, and PATCH URL to update the endpoint.</span></span>

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

<span data-ttu-id="6dca4-150">*ApiKey* a *endpointUrl* pro volání můžete získat z řídicího panelu koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6dca4-150">The *apiKey* and the *endpointUrl* for the call can be obtained from endpoint dashboard.</span></span>

<span data-ttu-id="6dca4-151">Hodnota *název* parametr v *prostředky* by měl odpovídat názvu prostředku uložené Trained Model prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="6dca4-151">The value of the *Name* parameter in *Resources* should match the Resource Name of the saved Trained Model in the predictive experiment.</span></span> <span data-ttu-id="6dca4-152">Pokud chcete získat název prostředku:</span><span class="sxs-lookup"><span data-stu-id="6dca4-152">To get the Resource Name:</span></span>

1. <span data-ttu-id="6dca4-153">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6dca4-153">Sign in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="6dca4-154">V nabídce vlevo klikněte na **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-154">In the left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="6dca4-155">Pod názvem, klikněte na pracovní prostor a potom klikněte na **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-155">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="6dca4-156">Pod názvem, klikněte na **modelu sčítání [prediktivní exp.]** .</span><span class="sxs-lookup"><span data-stu-id="6dca4-156">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="6dca4-157">Klikněte na tlačítko Nový koncový bod, který jste přidali.</span><span class="sxs-lookup"><span data-stu-id="6dca4-157">Click the new endpoint you added.</span></span>
6. <span data-ttu-id="6dca4-158">Na řídicím panelu endpoint, klikněte na tlačítko **aktualizace prostředků**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-158">On the endpoint dashboard, click **Update Resource**.</span></span>
7. <span data-ttu-id="6dca4-159">Na stránce dokumentace rozhraní API aktualizace prostředků pro webovou službu, můžete najít **název prostředku** pod **aktualizovat prostředky**.</span><span class="sxs-lookup"><span data-stu-id="6dca4-159">On the Update Resource API Documentation page for the web service, you can find the **Resource Name** under **Updatable Resources**.</span></span>

<span data-ttu-id="6dca4-160">Pokud váš token SAS vyprší dokončení aktualizace koncový bod, je třeba provést GET s Id úlohy získat nový token.</span><span class="sxs-lookup"><span data-stu-id="6dca4-160">If your SAS token expires before you finish updating the endpoint, you must perform a GET with the Job Id to obtain a fresh token.</span></span>

<span data-ttu-id="6dca4-161">Při kód byl úspěšně spuštěn, začněte nový koncový bod pomocí retrained modelu v přibližně 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6dca4-161">When the code has successfully run, the new endpoint should start using the retrained model in approximately 30 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="6dca4-162">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6dca4-162">Summary</span></span>
<span data-ttu-id="6dca4-163">Pomocí rozhraní Retraining API, můžete aktualizovat pro cvičný model prediktivní povolení scénáře, jako webové služby:</span><span class="sxs-lookup"><span data-stu-id="6dca4-163">Using the Retraining APIs, you can update the trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="6dca4-164">Pravidelné model retraining s nová data.</span><span class="sxs-lookup"><span data-stu-id="6dca4-164">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="6dca4-165">Distribuce modelu pro zákazníky s cílem aby přeučování modelu s použitím svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="6dca4-165">Distribution of a model to customers with the goal of letting them retrain the model using their own data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6dca4-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6dca4-166">Next steps</span></span>
[<span data-ttu-id="6dca4-167">Řešení potíží s retraining classic webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6dca4-167">Troubleshooting the retraining of an Azure Machine Learning classic web service</span></span>](machine-learning-troubleshooting-retraining-models.md)

