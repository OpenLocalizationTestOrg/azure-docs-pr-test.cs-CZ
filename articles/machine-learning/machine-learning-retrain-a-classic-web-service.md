---
title: "aaaRetrain Classic webové služby | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically přeučování modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
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
ms.openlocfilehash: d3ba21ed75f02868535cb2fcac607643303a9554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-classic-web-service"></a><span data-ttu-id="9f76b-103">Přeučování webové služby Classic</span><span class="sxs-lookup"><span data-stu-id="9f76b-103">Retrain a Classic web service</span></span>
<span data-ttu-id="9f76b-104">Hello prediktivní webové služby, které jste nasadili je výchozí hello vyhodnocovací koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9f76b-104">hello Predictive Web Service you deployed is hello default scoring endpoint.</span></span> <span data-ttu-id="9f76b-105">Výchozí koncové body jsou synchronizovány s hello původní školení a vyhodnocování experimenty, a proto hello trained model pro výchozí koncový bod hello nejde nahradit.</span><span class="sxs-lookup"><span data-stu-id="9f76b-105">Default endpoints are kept in sync with hello original training and scoring experiments, and therefore hello trained model for hello default endpoint cannot be replaced.</span></span> <span data-ttu-id="9f76b-106">tooretrain hello webové služby, je nutné přidat novou webovou službu toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9f76b-106">tooretrain hello web service, you must add a new endpoint toohello web service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9f76b-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f76b-107">Prerequisites</span></span>
<span data-ttu-id="9f76b-108">Musí mít nastavíte výukový experiment a prediktivní experiment jak je znázorněno v [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="9f76b-108">You must have set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9f76b-109">Prediktivní experiment Hello musí být nasazený jako Classic strojového učení webové služby.</span><span class="sxs-lookup"><span data-stu-id="9f76b-109">hello predictive experiment must be deployed as a Classic machine learning web service.</span></span> 
> 
> 

<span data-ttu-id="9f76b-110">Další informace o nasazení webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="9f76b-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="add-a-new-endpoint"></a><span data-ttu-id="9f76b-111">Přidat nový koncový bod</span><span class="sxs-lookup"><span data-stu-id="9f76b-111">Add a new Endpoint</span></span>
<span data-ttu-id="9f76b-112">Hello prediktivní webové služby, který jste nasadili obsahuje výchozí vyhodnocovací koncový bod, který je uložen synchronizována s hello původní školení a vyhodnocování experimenty trained model.</span><span class="sxs-lookup"><span data-stu-id="9f76b-112">hello Predictive Web Service that you deployed contains a default scoring endpoint that is kept in sync with hello original training and scoring experiments trained model.</span></span> <span data-ttu-id="9f76b-113">tooupdate vaše toowith nového modulu trained model služby web musíte vytvořit nový koncový bod vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="9f76b-113">tooupdate your web service toowith a new trained model, you must create a new scoring endpoint.</span></span> 

<span data-ttu-id="9f76b-114">toocreate nový vyhodnocovací koncový bod, na hello prediktivní webová služba, která mohou být aktualizovány s hello trained model:</span><span class="sxs-lookup"><span data-stu-id="9f76b-114">toocreate a new scoring endpoint, on hello Predictive Web Service that can be updated with hello trained model:</span></span>

> [!NOTE]
> <span data-ttu-id="9f76b-115">Ujistěte se, že přidáváte hello koncový bod toohello prediktivní webové služby, hello školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="9f76b-115">Be sure you are adding hello endpoint toohello Predictive Web Service, not hello Training Web Service.</span></span> <span data-ttu-id="9f76b-116">Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="9f76b-116">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate web services listed.</span></span> <span data-ttu-id="9f76b-117">Hello prediktivní webové služby musí končit "[prediktivní exp.]".</span><span class="sxs-lookup"><span data-stu-id="9f76b-117">hello Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

<span data-ttu-id="9f76b-118">Existují tři způsoby, ve které můžete přidat novou webovou službu tooa koncového bodu:</span><span class="sxs-lookup"><span data-stu-id="9f76b-118">There are three ways in which you can add a new end point tooa web service:</span></span>

1. <span data-ttu-id="9f76b-119">Prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="9f76b-119">Programmatically</span></span>
2. <span data-ttu-id="9f76b-120">Pomocí portálu služby Microsoft Azure Web hello</span><span class="sxs-lookup"><span data-stu-id="9f76b-120">Use hello Microsoft Azure Web Services portal</span></span>
3. <span data-ttu-id="9f76b-121">Hello použijte portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="9f76b-121">Use hello Azure classic portal</span></span>

### <a name="programmatically-add-an-endpoint"></a><span data-ttu-id="9f76b-122">Přidání koncového bodu prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="9f76b-122">Programmatically add an endpoint</span></span>
<span data-ttu-id="9f76b-123">Můžete přidat vyhodnocování koncové body pomocí hello ukázkový kód zadaný v tomto [úložiště github](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="9f76b-123">You can add scoring endpoints using hello sample code provided in this [github repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span></span>

### <a name="use-hello-microsoft-azure-web-services-portal-tooadd-an-endpoint"></a><span data-ttu-id="9f76b-124">Použití portálu Microsoft Azure Web Services hello – tooadd koncový bod</span><span class="sxs-lookup"><span data-stu-id="9f76b-124">Use hello Microsoft Azure Web Services portal tooadd an endpoint</span></span>
1. <span data-ttu-id="9f76b-125">Machine Learning Studio v levém navigačním sloupec hello, klikněte na možnost webové služby.</span><span class="sxs-lookup"><span data-stu-id="9f76b-125">In Machine Learning Studio, on hello left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="9f76b-126">V dolní části hello řídicího panelu, hello webové služby, klikněte na tlačítko **spravovat koncové body preview**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-126">At hello bottom of hello web service dashboard, click **Manage endpoints preview**.</span></span>
3. <span data-ttu-id="9f76b-127">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-127">Click **Add**.</span></span>
4. <span data-ttu-id="9f76b-128">Zadejte název a popis pro nový koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="9f76b-128">Type a name and description for hello new endpoint.</span></span> <span data-ttu-id="9f76b-129">Vyberte úroveň protokolování hello a určuje, jestli je povolená ukázková data.</span><span class="sxs-lookup"><span data-stu-id="9f76b-129">Select hello logging level and whether sample data is enabled.</span></span> <span data-ttu-id="9f76b-130">Další informace o protokolování naleznete v tématu [povolení protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="9f76b-130">For more information on logging, see [Enable logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>

### <a name="use-hello-azure-classic-portal-tooadd-an-endpoint"></a><span data-ttu-id="9f76b-131">Použití hello Azure classic portálu tooadd koncový bod</span><span class="sxs-lookup"><span data-stu-id="9f76b-131">Use hello Azure classic portal tooadd an endpoint</span></span>
1. <span data-ttu-id="9f76b-132">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9f76b-132">Sign in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9f76b-133">V levé nabídce hello, klikněte na **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-133">In hello left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="9f76b-134">Pod názvem, klikněte na pracovní prostor a potom klikněte na **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-134">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="9f76b-135">Pod názvem, klikněte na **modelu sčítání [prediktivní exp.]** .</span><span class="sxs-lookup"><span data-stu-id="9f76b-135">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="9f76b-136">V dolní části hello hello stránky, klikněte na tlačítko **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-136">At hello bottom of hello page, click **Add Endpoint**.</span></span> <span data-ttu-id="9f76b-137">Další informace o přidání koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="9f76b-137">For more information on adding endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span> 

## <a name="update-hello-added-endpoints-trained-model"></a><span data-ttu-id="9f76b-138">Aktualizace hello přidat Trained Model pro koncový bod</span><span class="sxs-lookup"><span data-stu-id="9f76b-138">Update hello added endpoint’s Trained Model</span></span>
<span data-ttu-id="9f76b-139">toocomplete hello retraining procesu, je nutné aktualizovat hello trained model nový koncový bod hello, kterou jste přidali.</span><span class="sxs-lookup"><span data-stu-id="9f76b-139">toocomplete hello retraining process, you must update hello trained model of hello new endpoint that you added.</span></span>

* <span data-ttu-id="9f76b-140">Pokud jste přidali hello nový koncový bod pomocí portálu Azure classic hello, můžete kliknutím na hello nový název koncového bodu na portálu hello a pak hello **operace UpdateResource** odkaz tooget hello – adresa URL potřebovali byste tooupdate hello endpoint modelu.</span><span class="sxs-lookup"><span data-stu-id="9f76b-140">If you added hello new endpoint using hello classic Azure portal, you can click hello new endpoint's name in hello portal, then hello **UpdateResource** link tooget hello URL you would need tooupdate hello endpoint's model.</span></span>
* <span data-ttu-id="9f76b-141">Pokud jste přidali hello koncového bodu pomocí hello ukázkový kód, jedná se o umístění adresa URL nápovědy hello identifikovaný hello *HelpLocationURL* hodnotu ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="9f76b-141">If you added hello endpoint using hello sample code, this includes location of hello help URL identified by hello *HelpLocationURL* value in hello output.</span></span>

<span data-ttu-id="9f76b-142">tooretrieve hello cestu adresy URL:</span><span class="sxs-lookup"><span data-stu-id="9f76b-142">tooretrieve hello path URL:</span></span>

1. <span data-ttu-id="9f76b-143">Zkopírujte a vložte adresu URL hello do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9f76b-143">Copy and paste hello URL into your browser.</span></span>
2. <span data-ttu-id="9f76b-144">Kliknutím na odkaz aktualizace prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9f76b-144">Click hello Update Resource link.</span></span>
3. <span data-ttu-id="9f76b-145">Zkopírujte hello POST URL požadavku PATCH hello.</span><span class="sxs-lookup"><span data-stu-id="9f76b-145">Copy hello POST URL of hello PATCH request.</span></span> <span data-ttu-id="9f76b-146">Například:</span><span class="sxs-lookup"><span data-stu-id="9f76b-146">For example:</span></span>
   
     <span data-ttu-id="9f76b-147">ADRESA URL OPRAVY: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2</span><span class="sxs-lookup"><span data-stu-id="9f76b-147">PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2</span></span>

<span data-ttu-id="9f76b-148">Teď můžete použít hello trained model tooupdate hello vyhodnocovací koncový bod, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="9f76b-148">You can now use hello trained model tooupdate hello scoring endpoint that you created previously.</span></span>

<span data-ttu-id="9f76b-149">Hello následující vzorový kód ukazuje, jak toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*a oprava URL tooupdate hello koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="9f76b-149">hello following sample code shows you how toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, and PATCH URL tooupdate hello endpoint.</span></span>

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
                        RelativeLocation = "your endpoint relative location", //from hello output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from hello output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
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

<span data-ttu-id="9f76b-150">Hello *apiKey* a hello *endpointUrl* pro volání hello můžete získat z řídicího panelu koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9f76b-150">hello *apiKey* and hello *endpointUrl* for hello call can be obtained from endpoint dashboard.</span></span>

<span data-ttu-id="9f76b-151">Hello hodnotu hello *název* parametr v *prostředky* mají shodu hello název prostředků hello uloženy Trained Model prediktivní experiment hello.</span><span class="sxs-lookup"><span data-stu-id="9f76b-151">hello value of hello *Name* parameter in *Resources* should match hello Resource Name of hello saved Trained Model in hello predictive experiment.</span></span> <span data-ttu-id="9f76b-152">tooget hello název prostředku:</span><span class="sxs-lookup"><span data-stu-id="9f76b-152">tooget hello Resource Name:</span></span>

1. <span data-ttu-id="9f76b-153">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9f76b-153">Sign in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9f76b-154">V levé nabídce hello, klikněte na **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-154">In hello left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="9f76b-155">Pod názvem, klikněte na pracovní prostor a potom klikněte na **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-155">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="9f76b-156">Pod názvem, klikněte na **modelu sčítání [prediktivní exp.]** .</span><span class="sxs-lookup"><span data-stu-id="9f76b-156">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="9f76b-157">Klikněte na tlačítko Nový koncový bod hello, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="9f76b-157">Click hello new endpoint you added.</span></span>
6. <span data-ttu-id="9f76b-158">Na řídicím panelu endpoint hello, klikněte na tlačítko **aktualizace prostředků**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-158">On hello endpoint dashboard, click **Update Resource**.</span></span>
7. <span data-ttu-id="9f76b-159">Na stránce dokumentace k API prostředku aktualizace hello hello webové služby, můžete najít hello **název prostředku** pod **aktualizovat prostředky**.</span><span class="sxs-lookup"><span data-stu-id="9f76b-159">On hello Update Resource API Documentation page for hello web service, you can find hello **Resource Name** under **Updatable Resources**.</span></span>

<span data-ttu-id="9f76b-160">Pokud před dokončíte, aktualizuje se koncový bod hello vyprší platnost vašeho tokenu SAS, je třeba provést GET s Id úlohy tooobtain hello nový token.</span><span class="sxs-lookup"><span data-stu-id="9f76b-160">If your SAS token expires before you finish updating hello endpoint, you must perform a GET with hello Job Id tooobtain a fresh token.</span></span>

<span data-ttu-id="9f76b-161">Při hello kód byl úspěšně spuštěn, začněte hello nový koncový bod pomocí modelu hello retrained přibližně 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f76b-161">When hello code has successfully run, hello new endpoint should start using hello retrained model in approximately 30 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="9f76b-162">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9f76b-162">Summary</span></span>
<span data-ttu-id="9f76b-163">Pomocí hello Retraining API, můžete aktualizovat hello trained model prediktivní webové služby, například povolení scénáře:</span><span class="sxs-lookup"><span data-stu-id="9f76b-163">Using hello Retraining APIs, you can update hello trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="9f76b-164">Pravidelné model retraining s nová data.</span><span class="sxs-lookup"><span data-stu-id="9f76b-164">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="9f76b-165">Distribuce toocustomers modelu s cílem hello aby přeučování hello model pomocí svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="9f76b-165">Distribution of a model toocustomers with hello goal of letting them retrain hello model using their own data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f76b-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f76b-166">Next steps</span></span>
[<span data-ttu-id="9f76b-167">Řešení potíží s hello retraining classic webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9f76b-167">Troubleshooting hello retraining of an Azure Machine Learning classic web service</span></span>](machine-learning-troubleshooting-retraining-models.md)

