---
title: "aaaExcel add-in pro Machine Learning webové služby | Microsoft Docs"
description: "Jak toouse Azure Machine Learning webové služby přímo v aplikaci Excel bez psaní jakéhokoli kódu."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: c52f40d33c9907f284e4750afe47181dc3365fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a><span data-ttu-id="58847-103">Doplněk Excelu pro webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="58847-103">Excel Add-in for Azure Machine Learning web services</span></span>
<span data-ttu-id="58847-104">Aplikace Excel je snadno toocall webové služby přímo bez hello potřebovat toowrite žádný kód.</span><span class="sxs-lookup"><span data-stu-id="58847-104">Excel makes it easy toocall web services directly without hello need toowrite any code.</span></span>

## <a name="steps-toouse-an-existing-web-service-in-hello-workbook"></a><span data-ttu-id="58847-105">Kroky tooUse webové služby existující v hello sešitu</span><span class="sxs-lookup"><span data-stu-id="58847-105">Steps tooUse an Existing web service in hello Workbook</span></span>

1. <span data-ttu-id="58847-106">Otevřete hello [ukázkový soubor aplikace Excel](http://aka.ms/amlexcel-sample-2), který obsahuje doplněk hello aplikace Excel a data o cestujících na hello Titanic.</span><span class="sxs-lookup"><span data-stu-id="58847-106">Open hello [sample Excel file](http://aka.ms/amlexcel-sample-2), which contains hello Excel add-in and data about passengers on hello Titanic.</span></span>
2. <span data-ttu-id="58847-107">Kliknutím vyberte hello webové služby-"Titanic pozůstalým předpověď (ukázka doplňku Excel) [skóre]" v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="58847-107">Choose hello web service by clicking it - "Titanic Survivor Predictor (Excel Add-in Sample) [Score]" in this example.</span></span>
   
    ![Vyberte webovou službu][01]
3. <span data-ttu-id="58847-109">Tím přejdete toohello **Predict** části.</span><span class="sxs-lookup"><span data-stu-id="58847-109">This takes you toohello **Predict** section.</span></span>  <span data-ttu-id="58847-110">Tento sešit už obsahuje ukázková data, ale pro prázdný sešit můžete vybrat buňku v aplikaci Excel a klikněte na tlačítko **pomocí ukázkových dat**.</span><span class="sxs-lookup"><span data-stu-id="58847-110">This workbook already contains sample data, but for a blank workbook you can select a cell in Excel and click **Use sample data**.</span></span>
4. <span data-ttu-id="58847-111">Vyberte data hello se záhlavími a klikněte na ikonu rozsah hello vstupní data.</span><span class="sxs-lookup"><span data-stu-id="58847-111">Select hello data with headers and click hello input data range icon.</span></span>  <span data-ttu-id="58847-112">Ujistěte se, že je zaškrtnuté políčko "data obsahují záhlaví" hello.</span><span class="sxs-lookup"><span data-stu-id="58847-112">Make sure hello "My data has headers" box is checked.</span></span>
5. <span data-ttu-id="58847-113">V části **výstup**, zadejte počet buněk hello místo, kam chcete hello toobe výstup, například "H1" sem.</span><span class="sxs-lookup"><span data-stu-id="58847-113">Under **Output**, enter hello cell number where you want hello output toobe, for example "H1" here.</span></span>
6. <span data-ttu-id="58847-114">Klikněte na tlačítko **předpovědi**.</span><span class="sxs-lookup"><span data-stu-id="58847-114">Click **Predict**.</span></span>
   
    ![Předpověď části][02]

<span data-ttu-id="58847-116">Nasazení webové služby nebo použijte existující webovou službu.</span><span class="sxs-lookup"><span data-stu-id="58847-116">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="58847-117">Další informace o nasazení webové služby najdete v tématu [návod krok 5: nasazení služby Azure Machine Learning webové hello](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="58847-117">For more information on deploying a web service, see [Walkthrough Step 5: Deploy hello Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

<span data-ttu-id="58847-118">Získáte hello klíč rozhraní API pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="58847-118">Get hello API key for your web service.</span></span> <span data-ttu-id="58847-119">Tam, kde můžete provést tuto akci, závisí na tom, jestli jste publikovali webovou službu Classic Machine Learning webové služby Machine Learning nové.</span><span class="sxs-lookup"><span data-stu-id="58847-119">Where you perform this action depends on whether you published a Classic Machine Learning web service of a New Machine Learning web service.</span></span>

<span data-ttu-id="58847-120">**Použít klasický webovou službu**</span><span class="sxs-lookup"><span data-stu-id="58847-120">**Use a Classic web service**</span></span> 

1. <span data-ttu-id="58847-121">V nástroji Machine Learning Studio, klikněte na tlačítko hello **webové služby** v levém podokně hello a pak vyberte hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="58847-121">In Machine Learning Studio, click hello **WEB SERVICES** section in hello left pane, and then select hello web service.</span></span>
   
    ![Studio vyberte webové služby][04]
2. <span data-ttu-id="58847-123">Zkopírujte hello klíč rozhraní API pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="58847-123">Copy hello API key for hello web service.</span></span>
   
    ![Klíč Studio rozhraní API][05]
3. <span data-ttu-id="58847-125">Na hello **řídicí panel** pro hello webovou službu, klikněte na hello **požadavků a odpovědí** odkaz.</span><span class="sxs-lookup"><span data-stu-id="58847-125">On hello **DASHBOARD** tab for hello web service, click hello **REQUEST/RESPONSE** link.</span></span>
4. <span data-ttu-id="58847-126">Vyhledejte hello **URI požadavku** části.</span><span class="sxs-lookup"><span data-stu-id="58847-126">Look for hello **Request URI** section.</span></span>  <span data-ttu-id="58847-127">Zkopírujte a uložte hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="58847-127">Copy and save hello URL.</span></span>

> [!NOTE]
> <span data-ttu-id="58847-128">Nyní je možné toosign do hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu tooobtain klíč hello rozhraní API pro webovou službu Classic Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="58847-128">It is now possible toosign into hello [Azure Machine Learning Web Services](https://services.azureml.net) portal tooobtain hello API key for a Classic Machine Learning web service.</span></span>
> 
> 

<span data-ttu-id="58847-129">**Použít novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="58847-129">**Use a New web service**</span></span>

1. <span data-ttu-id="58847-130">V hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu klikněte na **webové služby**, potom vyberte svoji webovou službu.</span><span class="sxs-lookup"><span data-stu-id="58847-130">In hello [Azure Machine Learning Web Services](https://services.azureml.net) portal, click **Web Services**, then select your web service.</span></span> 
2. <span data-ttu-id="58847-131">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="58847-131">Click **Consume**.</span></span>
3. <span data-ttu-id="58847-132">Vyhledejte hello **informace o základní spotřeby** části.</span><span class="sxs-lookup"><span data-stu-id="58847-132">Look for hello **Basic consumption info** section.</span></span> <span data-ttu-id="58847-133">Zkopírujte a uložte hello **primární klíč** a hello **požadavků a odpovědí** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="58847-133">Copy and save hello **Primary Key** and hello **Request-Response** URL.</span></span>

## <a name="steps-tooadd-a-new-web-service"></a><span data-ttu-id="58847-134">Kroky tooAdd novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="58847-134">Steps tooAdd a New web service</span></span>

1. <span data-ttu-id="58847-135">Nasazení webové služby nebo použijte existující webovou službu.</span><span class="sxs-lookup"><span data-stu-id="58847-135">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="58847-136">Další informace o nasazení webové služby najdete v tématu [návod krok 5: nasazení služby Azure Machine Learning webové hello](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="58847-136">For more information on deploying a web service, see [Walkthrough Step 5: Deploy hello Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
2. <span data-ttu-id="58847-137">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="58847-137">Click **Consume**.</span></span>
3. <span data-ttu-id="58847-138">Vyhledejte hello **informace o základní spotřeby** části.</span><span class="sxs-lookup"><span data-stu-id="58847-138">Look for hello **Basic consumption info** section.</span></span> <span data-ttu-id="58847-139">Zkopírujte a uložte hello **primární klíč** a hello **požadavků a odpovědí** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="58847-139">Copy and save hello **Primary Key** and hello **Request-Response** URL.</span></span>
4. <span data-ttu-id="58847-140">V aplikaci Excel přejděte toohello **webové služby** část (Pokud jste v hello **Predict** klikněte na šipku zpět hello toogo toohello seznam webových služeb).</span><span class="sxs-lookup"><span data-stu-id="58847-140">In Excel, go toohello **Web Services** section (if you are in hello **Predict** section, click hello back arrow toogo toohello list of web services).</span></span>
   
    ![Přejděte výběr tooWeb služby][03]
5. <span data-ttu-id="58847-142">Klikněte na tlačítko **přidat webovou službu**.</span><span class="sxs-lookup"><span data-stu-id="58847-142">Click **Add Web Service**.</span></span>
6. <span data-ttu-id="58847-143">Vložte adresu URL hello do hello Excel doplňku textové pole s popiskem **URL**.</span><span class="sxs-lookup"><span data-stu-id="58847-143">Paste hello URL into hello Excel add-in text box labeled **URL**.</span></span>
7. <span data-ttu-id="58847-144">Vložení hello rozhraní API nebo primární klíč do hello textového pole s názvem bez přípony **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="58847-144">Paste hello API/Primary key into hello text box labeled **API key**.</span></span>
8. <span data-ttu-id="58847-145">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="58847-145">Click **Add**.</span></span>
   
    ![Adresa URL a klíč API pro classic webovou službu.][06]
9. <span data-ttu-id="58847-147">toouse hello webové služby, postupujte podle předchozích pokynů hello, "Kroky tooUse existující webové služby."</span><span class="sxs-lookup"><span data-stu-id="58847-147">toouse hello web service, follow hello preceding directions, "Steps tooUse an Existing web Service."</span></span>

## <a name="sharing-your-workbook"></a><span data-ttu-id="58847-148">Sdílení sešitu</span><span class="sxs-lookup"><span data-stu-id="58847-148">Sharing Your Workbook</span></span>
<span data-ttu-id="58847-149">Pokud jste uložili vašeho sešitu, hello rozhraní API nebo primární klíč pro hello webové služby, které jste přidali také uloženy.</span><span class="sxs-lookup"><span data-stu-id="58847-149">If you save your workbook, then hello API/Primary key for hello web services you have added is also saved.</span></span> <span data-ttu-id="58847-150">To znamená, že sešit hello by sdílet jenom s osob, kterým důvěřujete.</span><span class="sxs-lookup"><span data-stu-id="58847-150">That means you should only share hello workbook with individuals you trust.</span></span>

<span data-ttu-id="58847-151">Požádejte všechny dotazy v hello následující části komentář nebo na našem [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="58847-151">Ask any questions in hello following comment section or on our [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span></span>

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
