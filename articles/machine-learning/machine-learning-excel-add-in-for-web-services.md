---
title: "V aplikaci Excel add-in pro Machine Learning webové služby | Microsoft Docs"
description: "Jak používat Azure Machine Learning webové služby přímo v aplikaci Excel bez psaní jakéhokoli kódu."
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
ms.openlocfilehash: 0d60dd87bbdd4d3eafac0f8876cc9e41412a53ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a><span data-ttu-id="d3081-103">Doplněk Excelu pro webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d3081-103">Excel Add-in for Azure Machine Learning web services</span></span>
<span data-ttu-id="d3081-104">Excel usnadňuje volání webové služby přímo bez nutnosti psaní jakéhokoli kódu.</span><span class="sxs-lookup"><span data-stu-id="d3081-104">Excel makes it easy to call web services directly without the need to write any code.</span></span>

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a><span data-ttu-id="d3081-105">Kroky při použití webové služby existující v sešitu</span><span class="sxs-lookup"><span data-stu-id="d3081-105">Steps to Use an Existing web service in the Workbook</span></span>

1. <span data-ttu-id="d3081-106">Otevřete [ukázkový soubor aplikace Excel](http://aka.ms/amlexcel-sample-2), který obsahuje doplněk aplikace Excel a data o cestujících na Titanic.</span><span class="sxs-lookup"><span data-stu-id="d3081-106">Open the [sample Excel file](http://aka.ms/amlexcel-sample-2), which contains the Excel add-in and data about passengers on the Titanic.</span></span>
2. <span data-ttu-id="d3081-107">Vyberte webovou službu kliknutím-"Titanic pozůstalým předpověď (ukázka doplňku Excel) [skóre]" v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="d3081-107">Choose the web service by clicking it - "Titanic Survivor Predictor (Excel Add-in Sample) [Score]" in this example.</span></span>
   
    ![Vyberte webovou službu][01]
3. <span data-ttu-id="d3081-109">Tím přejdete **Predict** části.</span><span class="sxs-lookup"><span data-stu-id="d3081-109">This takes you to the **Predict** section.</span></span>  <span data-ttu-id="d3081-110">Tento sešit už obsahuje ukázková data, ale pro prázdný sešit můžete vybrat buňku v aplikaci Excel a klikněte na tlačítko **pomocí ukázkových dat**.</span><span class="sxs-lookup"><span data-stu-id="d3081-110">This workbook already contains sample data, but for a blank workbook you can select a cell in Excel and click **Use sample data**.</span></span>
4. <span data-ttu-id="d3081-111">Vyberte data, se záhlavími a klikněte na ikonu rozsah vstupní data.</span><span class="sxs-lookup"><span data-stu-id="d3081-111">Select the data with headers and click the input data range icon.</span></span>  <span data-ttu-id="d3081-112">Ujistěte se, že je zaškrtnuté políčko "data obsahují záhlaví".</span><span class="sxs-lookup"><span data-stu-id="d3081-112">Make sure the "My data has headers" box is checked.</span></span>
5. <span data-ttu-id="d3081-113">V části **výstup**, zadejte číslo buňky, kde má výstup, například "H1" zde být.</span><span class="sxs-lookup"><span data-stu-id="d3081-113">Under **Output**, enter the cell number where you want the output to be, for example "H1" here.</span></span>
6. <span data-ttu-id="d3081-114">Klikněte na tlačítko **předpovědi**.</span><span class="sxs-lookup"><span data-stu-id="d3081-114">Click **Predict**.</span></span>
   
    ![Předpověď části][02]

<span data-ttu-id="d3081-116">Nasazení webové služby nebo použijte existující webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d3081-116">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="d3081-117">Další informace o nasazení webové služby najdete v tématu [návod krok 5: nasazení služby Azure Machine Learning webové](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="d3081-117">For more information on deploying a web service, see [Walkthrough Step 5: Deploy the Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

<span data-ttu-id="d3081-118">Získáte klíč rozhraní API pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d3081-118">Get the API key for your web service.</span></span> <span data-ttu-id="d3081-119">Tam, kde můžete provést tuto akci, závisí na tom, jestli jste publikovali webovou službu Classic Machine Learning webové služby Machine Learning nové.</span><span class="sxs-lookup"><span data-stu-id="d3081-119">Where you perform this action depends on whether you published a Classic Machine Learning web service of a New Machine Learning web service.</span></span>

<span data-ttu-id="d3081-120">**Použít klasický webovou službu**</span><span class="sxs-lookup"><span data-stu-id="d3081-120">**Use a Classic web service**</span></span> 

1. <span data-ttu-id="d3081-121">V nástroji Machine Learning Studio, klikněte **webové služby** v levém podokně a pak vyberte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d3081-121">In Machine Learning Studio, click the **WEB SERVICES** section in the left pane, and then select the web service.</span></span>
   
    ![Studio vyberte webové služby][04]
2. <span data-ttu-id="d3081-123">Zkopírujte klíč rozhraní API pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d3081-123">Copy the API key for the web service.</span></span>
   
    ![Klíč Studio rozhraní API][05]
3. <span data-ttu-id="d3081-125">Na **řídicí panel** pro webovou službu, klikněte na **požadavků a odpovědí** odkaz.</span><span class="sxs-lookup"><span data-stu-id="d3081-125">On the **DASHBOARD** tab for the web service, click the **REQUEST/RESPONSE** link.</span></span>
4. <span data-ttu-id="d3081-126">Vyhledejte **URI požadavku** části.</span><span class="sxs-lookup"><span data-stu-id="d3081-126">Look for the **Request URI** section.</span></span>  <span data-ttu-id="d3081-127">Zkopírujte a uložte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d3081-127">Copy and save the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="d3081-128">Je nyní možné pro přihlášení do [webové služby Azure Machine Learning](https://services.azureml.net) portál k získání klíč rozhraní API pro webovou službu Classic Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d3081-128">It is now possible to sign into the [Azure Machine Learning Web Services](https://services.azureml.net) portal to obtain the API key for a Classic Machine Learning web service.</span></span>
> 
> 

<span data-ttu-id="d3081-129">**Použít novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="d3081-129">**Use a New web service**</span></span>

1. <span data-ttu-id="d3081-130">V [webové služby Azure Machine Learning](https://services.azureml.net) portálu klikněte na **webové služby**, potom vyberte svoji webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d3081-130">In the [Azure Machine Learning Web Services](https://services.azureml.net) portal, click **Web Services**, then select your web service.</span></span> 
2. <span data-ttu-id="d3081-131">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="d3081-131">Click **Consume**.</span></span>
3. <span data-ttu-id="d3081-132">Vyhledejte **informace o základní spotřeby** části.</span><span class="sxs-lookup"><span data-stu-id="d3081-132">Look for the **Basic consumption info** section.</span></span> <span data-ttu-id="d3081-133">Zkopírujte a uložte **primární klíč** a **požadavků a odpovědí** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d3081-133">Copy and save the **Primary Key** and the **Request-Response** URL.</span></span>

## <a name="steps-to-add-a-new-web-service"></a><span data-ttu-id="d3081-134">Postup přidání nové webové služby</span><span class="sxs-lookup"><span data-stu-id="d3081-134">Steps to Add a New web service</span></span>

1. <span data-ttu-id="d3081-135">Nasazení webové služby nebo použijte existující webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d3081-135">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="d3081-136">Další informace o nasazení webové služby najdete v tématu [návod krok 5: nasazení služby Azure Machine Learning webové](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="d3081-136">For more information on deploying a web service, see [Walkthrough Step 5: Deploy the Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
2. <span data-ttu-id="d3081-137">Klikněte na tlačítko **využívat**.</span><span class="sxs-lookup"><span data-stu-id="d3081-137">Click **Consume**.</span></span>
3. <span data-ttu-id="d3081-138">Vyhledejte **informace o základní spotřeby** části.</span><span class="sxs-lookup"><span data-stu-id="d3081-138">Look for the **Basic consumption info** section.</span></span> <span data-ttu-id="d3081-139">Zkopírujte a uložte **primární klíč** a **požadavků a odpovědí** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d3081-139">Copy and save the **Primary Key** and the **Request-Response** URL.</span></span>
4. <span data-ttu-id="d3081-140">V aplikaci Excel, přejděte na **webové služby** část (Pokud jste v **Predict** klikněte na šipku Zpět Přejít na seznam webové služby).</span><span class="sxs-lookup"><span data-stu-id="d3081-140">In Excel, go to the **Web Services** section (if you are in the **Predict** section, click the back arrow to go to the list of web services).</span></span>
   
    ![Přejděte na výběr webové služby][03]
5. <span data-ttu-id="d3081-142">Klikněte na tlačítko **přidat webovou službu**.</span><span class="sxs-lookup"><span data-stu-id="d3081-142">Click **Add Web Service**.</span></span>
6. <span data-ttu-id="d3081-143">Vložte adresu URL do aplikace Excel doplňku textové pole s popiskem **URL**.</span><span class="sxs-lookup"><span data-stu-id="d3081-143">Paste the URL into the Excel add-in text box labeled **URL**.</span></span>
7. <span data-ttu-id="d3081-144">Vložte klíč rozhraní API nebo primární do textového pole s názvem bez přípony **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="d3081-144">Paste the API/Primary key into the text box labeled **API key**.</span></span>
8. <span data-ttu-id="d3081-145">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d3081-145">Click **Add**.</span></span>
   
    ![Adresa URL a klíč API pro classic webovou službu.][06]
9. <span data-ttu-id="d3081-147">Webovou službu, postupujte podle předchozích pokynů, "Postup použijte existující webové služby."</span><span class="sxs-lookup"><span data-stu-id="d3081-147">To use the web service, follow the preceding directions, "Steps to Use an Existing web Service."</span></span>

## <a name="sharing-your-workbook"></a><span data-ttu-id="d3081-148">Sdílení sešitu</span><span class="sxs-lookup"><span data-stu-id="d3081-148">Sharing Your Workbook</span></span>
<span data-ttu-id="d3081-149">Pokud jste uložili vašeho sešitu, rozhraní API nebo primární klíč pro webové služby, které jste přidali také uloženy.</span><span class="sxs-lookup"><span data-stu-id="d3081-149">If you save your workbook, then the API/Primary key for the web services you have added is also saved.</span></span> <span data-ttu-id="d3081-150">To znamená, že sešit by měly sdílet jenom s osob, kterým důvěřujete.</span><span class="sxs-lookup"><span data-stu-id="d3081-150">That means you should only share the workbook with individuals you trust.</span></span>

<span data-ttu-id="d3081-151">Požádejte všechny dotazy v následující části komentář nebo na našem [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="d3081-151">Ask any questions in the following comment section or on our [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span></span>

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
