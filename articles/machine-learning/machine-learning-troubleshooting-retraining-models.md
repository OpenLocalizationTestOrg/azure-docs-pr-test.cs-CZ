---
title: "aaaTroubleshoot retraining Azure Machine Learning Classic webové služby | Microsoft Docs"
description: "Identifikujte a opravte narazil běžné problémy, když jsou retraining hello modelu pro webové služby Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 2b6a78eaba161877106dccdc23437b5e454fca7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-retraining-of-an-azure-machine-learning-classic-web-service"></a><span data-ttu-id="9e976-103">Řešení potíží s hello retraining Azure Machine Learning Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="9e976-103">Troubleshooting hello retraining of an Azure Machine Learning Classic Web service</span></span>
## <a name="retraining-overview"></a><span data-ttu-id="9e976-104">Retraining – přehled</span><span class="sxs-lookup"><span data-stu-id="9e976-104">Retraining overview</span></span>
<span data-ttu-id="9e976-105">Když nasadíte prediktivní experiment jako vyhodnocování webové služby je statický model.</span><span class="sxs-lookup"><span data-stu-id="9e976-105">When you deploy a predictive experiment as a scoring web service it is a static model.</span></span> <span data-ttu-id="9e976-106">Jakmile nová data k dispozici nebo pokud příjemce hello hello API má svá vlastní data, musí hello modelu toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="9e976-106">As new data becomes available or when hello consumer of hello API has their own data, hello model needs toobe retrained.</span></span> 

<span data-ttu-id="9e976-107">Kompletní návod hello retraining proces Classic webové služby, najdete v části [Přeučování Machine Learning modely prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="9e976-107">For a complete walkthrough of hello retraining process of a Classic Web service, see [Retrain Machine Learning Models Programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

## <a name="retraining-process"></a><span data-ttu-id="9e976-108">Retraining procesu</span><span class="sxs-lookup"><span data-stu-id="9e976-108">Retraining process</span></span>
<span data-ttu-id="9e976-109">Pokud budete potřebovat tooretrain hello webové služby, musíte přidat další některé jeho součásti:</span><span class="sxs-lookup"><span data-stu-id="9e976-109">When you need tooretrain hello Web service, you must add some additional pieces:</span></span>

* <span data-ttu-id="9e976-110">Webovou službu nasazenou z hello výukový Experiment.</span><span class="sxs-lookup"><span data-stu-id="9e976-110">A Web service deployed from hello Training Experiment.</span></span> <span data-ttu-id="9e976-111">musí mít Hello experiment **výstup webové služby** modulu připojené toohello výstup hello **Train Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="9e976-111">hello experiment must have a **Web Service Output** module attached toohello output of hello **Train Model** module.</span></span>  
  
    ![Připojte hello webové služby výstupní toohello train model.][image1]
* <span data-ttu-id="9e976-113">Nový koncový bod přidat tooyour vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-113">A new endpoint added tooyour scoring Web service.</span></span>  <span data-ttu-id="9e976-114">Můžete přidat koncový bod hello programově pomocí hello ukázkový kód v hello odkazuje programovém přeučení modelů Machine Learning tématu nebo prostřednictvím hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="9e976-114">You can add hello endpoint programmatically using hello sample code referenced in hello Retrain Machine Learning models programmatically topic or through hello Azure classic portal.</span></span>

<span data-ttu-id="9e976-115">Potom můžete hello ukázkový kód C# z modelu tooretrain stránky nápovědy API hello školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-115">You can then use hello sample C# code from hello Training Web Service's API help page tooretrain model.</span></span> <span data-ttu-id="9e976-116">Jakmile máte vyhodnotit hello výsledky a spokojeni s nimi, aktualizujete hello trained model vyhodnocování webové službě pomocí nový koncový bod hello, kterou jste přidali.</span><span class="sxs-lookup"><span data-stu-id="9e976-116">Once you have evaluated hello results and are satisfied with them, you update hello trained model scoring web service using hello new endpoint that you added.</span></span>

<span data-ttu-id="9e976-117">Hello hlavních kroků, je nutné provést tooretrain hello modelu se všechny kusy hello na místě, jsou následující:</span><span class="sxs-lookup"><span data-stu-id="9e976-117">With all hello pieces in place, hello major steps you must take tooretrain hello model are as follows:</span></span>

1. <span data-ttu-id="9e976-118">Volání hello školení webové služby: volání hello je toohello spuštění služby Batch (BES), není hello žádosti o služby odpovědi (RR).</span><span class="sxs-lookup"><span data-stu-id="9e976-118">Call hello Training Web Service:  hello call is toohello Batch Execution Service (BES), not hello Request Response Service (RRS).</span></span> <span data-ttu-id="9e976-119">Můžete vytvořit hello ukázkový kód C# hello rozhraní API nápovědy stránky toomake hello volání.</span><span class="sxs-lookup"><span data-stu-id="9e976-119">You can use hello sample C# code on hello API help page toomake hello call.</span></span> 
2. <span data-ttu-id="9e976-120">Najít hello hodnoty pro hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken*: tyto hodnoty jsou vráceny ve výstupu hello z vaší toohello volání webové školení Služba.</span><span class="sxs-lookup"><span data-stu-id="9e976-120">Find hello values for hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken*: These values are returned in hello output from your call toohello Training Web Service.</span></span> 
   <span data-ttu-id="9e976-121">![zobrazuje výstup hello hello retraining ukázka a hello BaseLocation, RelativeLocation a SasBlobToken hodnoty.][image6]</span><span class="sxs-lookup"><span data-stu-id="9e976-121">![showing hello output of hello retraining sample and hello BaseLocation, RelativeLocation, and  SasBlobToken values.][image6]</span></span>
3. <span data-ttu-id="9e976-122">Aktualizace hello přidat koncový bod z hello vyhodnocování webové služby s hello nové Trénink modelu: pomocí hello ukázkový kód zadaný v hello Machine Learning Přeučování modelů prostřednictvím kódu programu, aktualizace nový koncový bod hello jste přidali toohello nově vyhodnocování model s hello trained model z hello školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-122">Update hello added endpoint from hello scoring web service with hello new trained model: Using hello sample code provided in hello Retrain Machine Learning models programmatically, update hello new endpoint you added toohello scoring model with hello newly trained model from hello Training Web Service.</span></span>

## <a name="common-obstacles"></a><span data-ttu-id="9e976-123">Běžné překážek</span><span class="sxs-lookup"><span data-stu-id="9e976-123">Common obstacles</span></span>
### <a name="check-toosee-if-you-have-hello-correct-patch-url"></a><span data-ttu-id="9e976-124">Zkontrolujte toosee, pokud máte hello opravte oprava adresy URL</span><span class="sxs-lookup"><span data-stu-id="9e976-124">Check toosee if you have hello correct PATCH URL</span></span>
<span data-ttu-id="9e976-125">Hello oprava URL používáte musí být hello jeden přidružené hello nový vyhodnocovací koncový bod přidat toohello vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-125">hello PATCH URL you are using must be hello one associated with hello new scoring endpoint you added toohello scoring Web service.</span></span> <span data-ttu-id="9e976-126">Existuje několik způsobů tooobtain hello oprava adresy URL:</span><span class="sxs-lookup"><span data-stu-id="9e976-126">There are a number of ways tooobtain hello PATCH URL:</span></span>

<span data-ttu-id="9e976-127">**Možnost 1: programově**</span><span class="sxs-lookup"><span data-stu-id="9e976-127">**Option 1: Programatically**</span></span>

<span data-ttu-id="9e976-128">tooget hello opravte oprava adresy URL:</span><span class="sxs-lookup"><span data-stu-id="9e976-128">tooget hello correct PATCH URL:</span></span>

1. <span data-ttu-id="9e976-129">Spustit hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="9e976-129">Run hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>
2. <span data-ttu-id="9e976-130">Z výstupu hello AddEndpoint, najde hello *HelpLocation* hodnotu a zkopírujte adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="9e976-130">From hello output of AddEndpoint, find hello *HelpLocation* value and copy hello URL.</span></span>
   
   ![HelpLocation ve výstupu hello hello addEndpoint vzorku.][image2]
3. <span data-ttu-id="9e976-132">Vložte hello URL do prohlížeče toonavigate tooa stránky, který poskytuje odkazy na nápovědu pro hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-132">Paste hello URL into a browser toonavigate tooa page that provides help links for hello Web service.</span></span>
4. <span data-ttu-id="9e976-133">Klikněte na tlačítko hello **aktualizace prostředků** stránku nápovědy oprava hello tooopen odkaz.</span><span class="sxs-lookup"><span data-stu-id="9e976-133">Click hello **Update Resource** link tooopen hello patch help page.</span></span>

<span data-ttu-id="9e976-134">**Možnost 2: Použití hello portál Azure classic**</span><span class="sxs-lookup"><span data-stu-id="9e976-134">**Option 2: Use hello Azure classic portal**</span></span>

1. <span data-ttu-id="9e976-135">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9e976-135">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9e976-136">Karta otevřete hello Machine Learning. ![Karta opřené počítače.][image4]</span><span class="sxs-lookup"><span data-stu-id="9e976-136">Open hello Machine Learning tab. ![Machine leaning tab.][image4]</span></span>
3. <span data-ttu-id="9e976-137">Pak klikněte na název pracovního prostoru, **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="9e976-137">Click your workspace name, then **Web Services**.</span></span>
4. <span data-ttu-id="9e976-138">Klikněte na tlačítko hello vyhodnocování webové služby, kterou pracujete.</span><span class="sxs-lookup"><span data-stu-id="9e976-138">Click hello scoring Web service you are working with.</span></span> <span data-ttu-id="9e976-139">(Pokud jste nezměnila hello výchozí název hello webové služby, se bude končit [vyhodnocování Exp.].)</span><span class="sxs-lookup"><span data-stu-id="9e976-139">(If you did not modify hello default name of hello web service, it will end in [Scoring Exp.].)</span></span>
5. <span data-ttu-id="9e976-140">Klikněte na tlačítko **přidání koncového bodu**.</span><span class="sxs-lookup"><span data-stu-id="9e976-140">Click **Add Endpoint**.</span></span>
6. <span data-ttu-id="9e976-141">Po přidání koncového bodu hello klikněte na název koncového bodu hello.</span><span class="sxs-lookup"><span data-stu-id="9e976-141">After hello endpoint is added, click hello endpoint name.</span></span> <span data-ttu-id="9e976-142">Pak klikněte na tlačítko **aktualizace prostředků** tooopen hello oprav stránku nápovědy.</span><span class="sxs-lookup"><span data-stu-id="9e976-142">Then click **Update Resource** tooopen hello patching help page.</span></span>

> [!NOTE]
> <span data-ttu-id="9e976-143">Pokud jste přidali hello koncový bod toohello školení webové služby místo hello prediktivní webové služby, zobrazí se následující chyba po kliknutí na tlačítko hello hello **aktualizace prostředků** odkaz: je nám líto, ale tato funkce není podporována nebo v tomto kontextu dostupné.</span><span class="sxs-lookup"><span data-stu-id="9e976-143">If you have added hello endpoint toohello Training Web Service instead of hello Predictive Web Service, you will receive hello following error when you click hello **Update Resource** link: Sorry, but this feature is not supported or available in this context.</span></span> <span data-ttu-id="9e976-144">Tato webová služba nemá žádné aktualizovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="9e976-144">This Web Service has no updatable resources.</span></span> <span data-ttu-id="9e976-145">Omlouváme se za nepříjemnosti hello a práce na zlepšení tento pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="9e976-145">We apologize for hello inconvenience and are working on improving this workflow.</span></span>
> 
> 

![Nový koncový bod řídicí panel.][image3]

<span data-ttu-id="9e976-147">stránka nápovědy oprava Hello obsahuje hello oprava adresa URL musí používat a poskytuje ukázkový kód můžete použít toocall ho.</span><span class="sxs-lookup"><span data-stu-id="9e976-147">hello PATCH help page contains hello PATCH URL you must use and provides sample code you can use toocall it.</span></span>

![Adresa URL opravy.][image5]

### <a name="check-toosee-that-you-are-updating-hello-correct-scoring-endpoint"></a><span data-ttu-id="9e976-149">Zkontrolujte toosee aktualizaci hello správný vyhodnocování koncový bod</span><span class="sxs-lookup"><span data-stu-id="9e976-149">Check toosee that you are updating hello correct scoring endpoint</span></span>
* <span data-ttu-id="9e976-150">Není oprava hello školení webové služby: hello operace opravy se musí provést na hello vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-150">Do not patch hello Training Web Service: hello patch operation must be performed on hello scoring Web service.</span></span>
* <span data-ttu-id="9e976-151">Není oprava hello výchozí koncový bod webové služby: hello operace opravy se musí provést na hello nové vyhodnocování koncový bod webové služby, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="9e976-151">Do not patch hello default endpoint on Web service: hello patch operation must be performed on hello new scoring Web service endpoint that you added.</span></span>

<span data-ttu-id="9e976-152">Můžete ověřit, které koncový bod webové služby hello je zapnutá ve hostujícími hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="9e976-152">You can verify which Web service hello endpoint is on by visiting hello Azure classic portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="9e976-153">Ujistěte se, že přidáváte hello koncový bod toohello prediktivní webové služby, hello školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-153">Be sure you are adding hello endpoint toohello Predictive Web Service, not hello Training Web Service.</span></span> <span data-ttu-id="9e976-154">Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby uvedené.</span><span class="sxs-lookup"><span data-stu-id="9e976-154">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate Web services listed.</span></span> <span data-ttu-id="9e976-155">Hello prediktivní webové služby musí končit "[prediktivní exp.]".</span><span class="sxs-lookup"><span data-stu-id="9e976-155">hello Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

1. <span data-ttu-id="9e976-156">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9e976-156">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9e976-157">Karta otevřete hello Machine Learning. ![Machine learning prostoru uživatelského rozhraní.][image4]</span><span class="sxs-lookup"><span data-stu-id="9e976-157">Open hello Machine Learning tab. ![Machine learning workspace UI.][image4]</span></span>
3. <span data-ttu-id="9e976-158">Vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="9e976-158">Select your workspace.</span></span>
4. <span data-ttu-id="9e976-159">Klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="9e976-159">Click **Web Services**.</span></span>
5. <span data-ttu-id="9e976-160">Vyberte prediktivní webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-160">Select your Predictive Web Service.</span></span>
6. <span data-ttu-id="9e976-161">Ověřte, že váš nový koncový bod byl přidán toohello webové služby.</span><span class="sxs-lookup"><span data-stu-id="9e976-161">Verify that your new endpoint was added toohello Web service.</span></span>

### <a name="check-hello-workspace-that-your-web-service-is-in-tooensure-it-is-in-hello-correct-region"></a><span data-ttu-id="9e976-162">Zkontrolujte hello pracovní prostor, který je webová služba v tooensure, který je v oblasti správné hello</span><span class="sxs-lookup"><span data-stu-id="9e976-162">Check hello workspace that your web service is in tooensure it is in hello correct region</span></span>
1. <span data-ttu-id="9e976-163">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9e976-163">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9e976-164">Vyberte z nabídky hello Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9e976-164">Select Machine Learning from hello menu.</span></span>
   <span data-ttu-id="9e976-165">![Machine learning oblast uživatelského rozhraní.][image4]</span><span class="sxs-lookup"><span data-stu-id="9e976-165">![Machine learning region UI.][image4]</span></span>
3. <span data-ttu-id="9e976-166">Zkontrolujte umístění hello pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="9e976-166">Verify hello location of your workspace.</span></span>

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
