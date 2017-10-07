---
title: "aaaDeploying novou webovou službu v Azure Machine Learning | Microsoft Docs"
description: "Hello pracovní postup nasazení ARM na základě webové služby"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX
redirect_url: machine-learning-publish-a-machine-learning-web-service
redirect_document_id: True
ms.openlocfilehash: 2cbfda44b97a6b992fbdfdfb0c761e6c9e169035
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-new-web-service"></a><span data-ttu-id="26838-103">Nasazení nové webové služby</span><span class="sxs-lookup"><span data-stu-id="26838-103">Deploy a new web service</span></span>
<span data-ttu-id="26838-104">Microsoft Azure Machine learning teď poskytuje webové služby, které jsou založeny na [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) povolení pro nové fakturace možnosti plánování a nasazení vaší webové služby toomultiple oblasti.</span><span class="sxs-lookup"><span data-stu-id="26838-104">Microsoft Azure Machine learning now provides web services that are based on [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) allowing for new billing plan options and deploying your web service toomultiple regions.</span></span>

<span data-ttu-id="26838-105">Obecný postup toodeploy Hello webové služby pomocí nástroje Microsoft Azure Machine Learning webové služby je:</span><span class="sxs-lookup"><span data-stu-id="26838-105">hello general workflow toodeploy a web service using Microsoft Azure Machine Learning Web Services is:</span></span>

* <span data-ttu-id="26838-106">Vytvořit prediktivní experiment</span><span class="sxs-lookup"><span data-stu-id="26838-106">Create a predictive experiment</span></span>
* <span data-ttu-id="26838-107">nasazení</span><span class="sxs-lookup"><span data-stu-id="26838-107">deploy it</span></span>
* <span data-ttu-id="26838-108">Konfigurace názvu</span><span class="sxs-lookup"><span data-stu-id="26838-108">configure its name</span></span>
* <span data-ttu-id="26838-109">plán fakturace</span><span class="sxs-lookup"><span data-stu-id="26838-109">billing plan</span></span>
* <span data-ttu-id="26838-110">otestovat</span><span class="sxs-lookup"><span data-stu-id="26838-110">test it</span></span>
* <span data-ttu-id="26838-111">ho využívají.</span><span class="sxs-lookup"><span data-stu-id="26838-111">consume it.</span></span>

<span data-ttu-id="26838-112">Hello následující obrázek ukazuje pracovní postup nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="26838-112">hello following graphic illustrates hello workflow.</span></span>

![Pracovní postup nasazení webové služby][1]

## <a name="deploy-web-service-from-studio"></a><span data-ttu-id="26838-114">Nasazení webové služby ze sady Studio</span><span class="sxs-lookup"><span data-stu-id="26838-114">Deploy web service from Studio</span></span>
<span data-ttu-id="26838-115">toodeploy experimentu jako novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="26838-115">toodeploy an experiment as a new web service.</span></span> <span data-ttu-id="26838-116">Přihlaste se k hello Machine Learning Studio a vytvořte novou prediktivní webovou službu.</span><span class="sxs-lookup"><span data-stu-id="26838-116">Sign into hello Machine Learning Studio and create a new predictive web service.</span></span> 

<span data-ttu-id="26838-117">**Poznámka:**: Pokud už mají nasazenou experimentu jako classic webovou službu, nelze ho nasadit jako novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="26838-117">**Note**: If you have already deployed an experiment as a classic web service you cannot deploy it as a new web service.</span></span>

<span data-ttu-id="26838-118">Klikněte na tlačítko **spustit** dole hello hello experimentovat plátno a potom klikněte na **nasazení webové služby** a **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="26838-118">Click **Run** at hello bottom of hello experiment canvas and then click **Deploy Web Service** and **Deploy Web Service [New]**.</span></span> <span data-ttu-id="26838-119">Otevře se stránka nasazení Hello hello webová služba Machine Learning správce.</span><span class="sxs-lookup"><span data-stu-id="26838-119">hello deployment page of hello Machine Learning Web Service manager will open.</span></span>

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a><span data-ttu-id="26838-120">Machine Learning webové služby Manager nasadit stránce experimentu</span><span class="sxs-lookup"><span data-stu-id="26838-120">Machine Learning Web Service Manager Deploy Experiment Page</span></span>
<span data-ttu-id="26838-121">Na stránce experimentu nasazení hello zadejte název pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="26838-121">On hello Deploy Experiment page, enter a name for hello web service.</span></span>
<span data-ttu-id="26838-122">Vyberte cenový plán.</span><span class="sxs-lookup"><span data-stu-id="26838-122">Select a pricing plan.</span></span> <span data-ttu-id="26838-123">Pokud máte existující cenový plán, že můžete ji vybrat, v opačném případě musíte vytvořit nový plán ceny služby hello.</span><span class="sxs-lookup"><span data-stu-id="26838-123">If you have an existing pricing plan you can select it, otherwise you must create a new price plan for hello service.</span></span> 

1. <span data-ttu-id="26838-124">V hello **cena plán** rozevírací nabídky vyberte existující plán nebo hello **vyberte nový plán** možnost.</span><span class="sxs-lookup"><span data-stu-id="26838-124">In hello **Price Plan** drop down, select an existing plan or select hello **Select new plan** option.</span></span>
2. <span data-ttu-id="26838-125">V **název plánu**, zadejte název, který bude identifikovat hello plán ve vašem vyúčtování.</span><span class="sxs-lookup"><span data-stu-id="26838-125">In **Plan Name**, type a name that will identify hello plan on your bill.</span></span>
3. <span data-ttu-id="26838-126">Vyberte jednu z hello **měsíční plánování vrstev**.</span><span class="sxs-lookup"><span data-stu-id="26838-126">Select one of hello **Monthly Plan Tiers**.</span></span> <span data-ttu-id="26838-127">Upozorňujeme, že úrovně plánu hello výchozí toohello plány pro vaši oblast výchozí a webové služby je nasazené toothat oblast.</span><span class="sxs-lookup"><span data-stu-id="26838-127">Note that hello plan tiers default toohello plans for your default region and your web service is deployed toothat region.</span></span>

<span data-ttu-id="26838-128">Klikněte na tlačítko **nasadit** a otevře se stránka hello rychlý start pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="26838-128">Click **Deploy** and hello Quickstart page for your web service opens.</span></span>

## <a name="quickstart-page"></a><span data-ttu-id="26838-129">Stránku rychlý start</span><span class="sxs-lookup"><span data-stu-id="26838-129">Quickstart page</span></span>
<span data-ttu-id="26838-130">Rychlý start stránku Hello webové služby obsahuje přístup a pokyny o hello nejčastějších úloh, které provedete po vytvoření nové webové služby.</span><span class="sxs-lookup"><span data-stu-id="26838-130">hello web service Quickstart page gives you access and guidance on hello most common tasks you will perform after creating a new web service.</span></span> <span data-ttu-id="26838-131">Tady jsou snadno přístupné oba hello **Test** stránky a **spotřebě** stránky.</span><span class="sxs-lookup"><span data-stu-id="26838-131">From here you can easily access both hello **Test** page and **Consume** page.</span></span>

## <a name="testing-your-web-service"></a><span data-ttu-id="26838-132">Testování webové služby</span><span class="sxs-lookup"><span data-stu-id="26838-132">Testing your web service</span></span>
<span data-ttu-id="26838-133">Na stránce Rychlý start hello klikněte na tlačítko Test webové služby v rámci běžné úlohy.</span><span class="sxs-lookup"><span data-stu-id="26838-133">From hello Quickstart page, click Test web service under common tasks.</span></span>   

<span data-ttu-id="26838-134">Webová služba hello tootest jako požadavků a odpovědí služby (RR):</span><span class="sxs-lookup"><span data-stu-id="26838-134">tootest hello web service as a Request-Response Service (RRS):</span></span>

* <span data-ttu-id="26838-135">Klikněte na tlačítko **Test** v řádku nabídek hello.</span><span class="sxs-lookup"><span data-stu-id="26838-135">Click **Test** on hello menu bar.</span></span>
* <span data-ttu-id="26838-136">Klikněte na tlačítko **požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="26838-136">Click **Request-Response**.</span></span>
* <span data-ttu-id="26838-137">Zadejte odpovídající hodnoty pro vstupní sloupce hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="26838-137">Enter appropriate values for hello input columns of your experiment.</span></span>
* <span data-ttu-id="26838-138">Klikněte na tlačítko Test **požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="26838-138">Click Test **Request-Response**.</span></span>

<span data-ttu-id="26838-139">Výsledky se zobrazí na hello pravé straně stránky hello.</span><span class="sxs-lookup"><span data-stu-id="26838-139">You results will display on hello right hand side of hello page.</span></span>

<span data-ttu-id="26838-140">tootest webové služby Batch spuštění služby (BES), budete používat soubor CSV:</span><span class="sxs-lookup"><span data-stu-id="26838-140">tootest a Batch Execution Service (BES) web service, you will use a CSV file:</span></span>

* <span data-ttu-id="26838-141">Klikněte na tlačítko **Test** v řádku nabídek hello.</span><span class="sxs-lookup"><span data-stu-id="26838-141">Click **Test** on hello menu bar.</span></span>
* <span data-ttu-id="26838-142">Klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="26838-142">Click **Batch**.</span></span>
* <span data-ttu-id="26838-143">V části váš vstup klikněte na tlačítko Procházet a přejděte tooyour ukázkový datový soubor.</span><span class="sxs-lookup"><span data-stu-id="26838-143">Under your input, click Browse and navigate tooyour sample data file.</span></span>
* <span data-ttu-id="26838-144">Klikněte na tlačítko **Test**.</span><span class="sxs-lookup"><span data-stu-id="26838-144">Click **Test**.</span></span>

<span data-ttu-id="26838-145">Stav Hello svůj test se zobrazí v části **testovací úlohy Batch**.</span><span class="sxs-lookup"><span data-stu-id="26838-145">hello status of your test is displayed under **Test Batch Jobs**.</span></span>

## <a name="consuming-your-web-service"></a><span data-ttu-id="26838-146">Využívají webové služby</span><span class="sxs-lookup"><span data-stu-id="26838-146">Consuming your Web Service</span></span>
<span data-ttu-id="26838-147">Při nasazení jako webové služby, zadejte experimenty Azure Machine Learning REST API, které mohou být spotřebovávána širokou škálu zařízení a platforem.</span><span class="sxs-lookup"><span data-stu-id="26838-147">When deployed as a web service, Azure Machine Learning experiments provide a REST API that can be consumed by a wide range of devices and platforms.</span></span> <span data-ttu-id="26838-148">To je proto zpráv ve formátu hello jednoduché rozhraní API REST přijme a odpoví JSON.</span><span class="sxs-lookup"><span data-stu-id="26838-148">This is because hello simple REST API accepts and responds with JSON formatted messages.</span></span> <span data-ttu-id="26838-149">Hello portál Azure Machine Learning poskytuje kód, který lze použít toocall hello webové služby v R, C# a Python.</span><span class="sxs-lookup"><span data-stu-id="26838-149">hello Azure Machine Learning portal provides code that can be used toocall hello web service in R, C#, and Python.</span></span>

<span data-ttu-id="26838-150">Na stránce spotřeba hello můžete najít:</span><span class="sxs-lookup"><span data-stu-id="26838-150">On hello Consuming page you can find:</span></span>

* <span data-ttu-id="26838-151">klíč Hello rozhraní API a identifikátorů URI pro použití webové služby v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="26838-151">hello API key and URI's for consuming web service in apps.</span></span>
* <span data-ttu-id="26838-152">Tookick šablony aplikace Excel a webové spuštění procesu vaší spotřeby.</span><span class="sxs-lookup"><span data-stu-id="26838-152">Excel and web app templates tookick start your consumption process.</span></span>
* <span data-ttu-id="26838-153">Ukázkový kód v jazyce C#, python a R tooget jste spustili.</span><span class="sxs-lookup"><span data-stu-id="26838-153">Sample code in C#, python, and R tooget you started.</span></span>

<span data-ttu-id="26838-154">Další informace o využívání webových služeb najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="26838-154">For more information on consuming web services, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="26838-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26838-155">Next Steps</span></span>
<span data-ttu-id="26838-156">Další informace o využívání webových služeb najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="26838-156">For more information on consuming web services, see:</span></span>

[<span data-ttu-id="26838-157">Jak tooconsume Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="26838-157">How tooconsume an Azure Machine Learning Web service</span></span>](machine-learning-consume-web-services.md)

[<span data-ttu-id="26838-158">Azure Machine Learning webové služby: Nasazení a používání</span><span class="sxs-lookup"><span data-stu-id="26838-158">Azure Machine Learning Web Services: Deployment and consumption</span></span>](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
