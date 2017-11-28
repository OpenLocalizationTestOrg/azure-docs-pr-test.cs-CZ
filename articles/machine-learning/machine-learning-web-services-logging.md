---
title: "Protokolování pro webové služby Machine Learning | Microsoft Docs"
description: "Informace o povolení protokolování pro webové služby Machine Learning. Protokolování poskytuje další informace k řešení rozhraní API."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: 7d0b2db01427430d6b0a317cdfefc265dd4b06e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a><span data-ttu-id="3b8de-104">Povolení protokolování webových služeb Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3b8de-104">Enable logging for Machine Learning web services</span></span>
<span data-ttu-id="3b8de-105">Tento dokument obsahuje informace o možnosti protokolování webových služeb Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3b8de-105">This document provides information on the logging capability of Machine Learning web services.</span></span> <span data-ttu-id="3b8de-106">Protokolování poskytuje další informace, kromě právě číslo chyby a zprávu, která vám pomohou vyřešit váš volání rozhraní API Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3b8de-106">Logging provides additional information, beyond just an error number and a message, that can help you troubleshoot your calls to the Machine Learning APIs.</span></span>  

## <a name="how-to-enable-logging-for-a-web-service"></a><span data-ttu-id="3b8de-107">Povolení protokolování pro webovou službu</span><span class="sxs-lookup"><span data-stu-id="3b8de-107">How to enable logging for a Web service</span></span>

<span data-ttu-id="3b8de-108">Povolit protokolování z [webové služby Azure Machine Learning](https://services.azureml.net) portálu.</span><span class="sxs-lookup"><span data-stu-id="3b8de-108">You enable logging from the [Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span> 

1. <span data-ttu-id="3b8de-109">Přihlaste se k portálu webové služby Azure Machine Learning na [https://services.azureml.net](https://services.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="3b8de-109">Sign in to the Azure Machine Learning Web Services portal at [https://services.azureml.net](https://services.azureml.net).</span></span> <span data-ttu-id="3b8de-110">Pro webovou službu Classic, také získáte na portál kliknutím **nové webové služby prostředí** na stránku webové služby Machine Learning v Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3b8de-110">For a Classic web service, you can also get to the portal by clicking **New Web Services Experience** on the Machine Learning Web Services page in Machine Learning Studio.</span></span>

   ![Nový odkaz webové služby prostředí](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. <span data-ttu-id="3b8de-112">Na panelu horní nabídce klikněte na tlačítko **webové služby** pro nové webové služby, nebo klikněte na tlačítko **Classic webové služby** pro klasický webové služby.</span><span class="sxs-lookup"><span data-stu-id="3b8de-112">On the top menu bar, click **Web Services** for a New web service, or click **Classic Web Services** for a Classic web service.</span></span>

   ![Vyberte nový nebo Classic webových služeb](media/machine-learning-web-services-logging/select-web-service.png)

3. <span data-ttu-id="3b8de-114">Pro novou webovou službu klikněte na název webové služby.</span><span class="sxs-lookup"><span data-stu-id="3b8de-114">For a New web service, click the web service name.</span></span> <span data-ttu-id="3b8de-115">Pro webovou službu Classic klikněte na název webové služby a potom na další stránce klikněte na příslušný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="3b8de-115">For a Classic web service, click the web service name and then on the next page click the appropriate endpoint.</span></span>

4. <span data-ttu-id="3b8de-116">Na panelu horní nabídce klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="3b8de-116">On the top menu bar, click **Configure**.</span></span>

5. <span data-ttu-id="3b8de-117">Nastavte **povolit protokolování** možnost k *chyba* (chyb do protokolu pouze) nebo *všechny* (pro úplné protokolování).</span><span class="sxs-lookup"><span data-stu-id="3b8de-117">Set the **Enable Logging** option to *Error* (to log only errors) or *All* (for full logging).</span></span>

   ![Vyberte úroveň protokolování](media/machine-learning-web-services-logging/enable-logging.png)

6. <span data-ttu-id="3b8de-119">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3b8de-119">Click **Save**.</span></span>

7. <span data-ttu-id="3b8de-120">Webové služby Classic, vytvořte **ml diagnostiky** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3b8de-120">For Classic web services, create the **ml-diagnostics** container.</span></span>

   <span data-ttu-id="3b8de-121">Všechny protokoly webové služby jsou uchovávány v kontejneru objektů blob s názvem **ml diagnostiky** v účtu úložiště přidruženého k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3b8de-121">All web service logs are kept in a blob container named **ml-diagnostics** in the storage account associated with the web service.</span></span> <span data-ttu-id="3b8de-122">Pro nové webové služby je tento kontejner vytvoří při prvním přístup k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3b8de-122">For New web services, this container is created the first time you access the web service.</span></span> <span data-ttu-id="3b8de-123">Webové služby Classic musíte vytvořit kontejner, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3b8de-123">For Classic web services, you need to create the container if it doesn't already exist.</span></span> 

   1. <span data-ttu-id="3b8de-124">V [portál Azure](https://portal.azure.com), přejděte na účet úložiště, který je přidružený k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3b8de-124">In the [Azure portal](https://portal.azure.com), go to the storage account associated with the web service.</span></span>

   2. <span data-ttu-id="3b8de-125">V části **služby objektů Blob**, klikněte na tlačítko **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="3b8de-125">Under **Blob Service**, click **Containers**.</span></span>

   3. <span data-ttu-id="3b8de-126">Pokud kontejner **ml diagnostiky** neexistuje, klikněte na tlačítko **+ kontejner**, zadejte kontejner název "ml – Diagnostika" a vyberte **přistupovat typu** jako "Blob".</span><span class="sxs-lookup"><span data-stu-id="3b8de-126">If the container **ml-diagnostics** doesn't exist, click **+Container**, give the container the name "ml-diagnostics", and select the **Access type** as "Blob".</span></span> <span data-ttu-id="3b8de-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b8de-127">Click **OK**.</span></span>

      ![Vyberte úroveň protokolování](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> <span data-ttu-id="3b8de-129">Pro webovou službu Classic má také řídicího panelu webové služby v nástroji Machine Learning Studio přepínač povolení protokolování.</span><span class="sxs-lookup"><span data-stu-id="3b8de-129">For a Classic web service, the Web Services Dashboard in Machine Learning Studio also has a switch to enable logging.</span></span> <span data-ttu-id="3b8de-130">Ale protože protokolování se teď spravují prostřednictvím portálu webové služby, musíte povolit protokolování prostřednictvím portálu, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3b8de-130">However, because logging is now managed through the Web Services portal, you need to enable logging through the portal as described in this article.</span></span> <span data-ttu-id="3b8de-131">-Li již povoleno protokolování v sadě Studio služby na webovém portálu zakázání protokolování a znovu ji povolte.</span><span class="sxs-lookup"><span data-stu-id="3b8de-131">If you already enabled logging in Studio, then in the Web Services Portal, disable logging and enable it again.</span></span>


## <a name="the-effects-of-enabling-logging"></a><span data-ttu-id="3b8de-132">Účinky povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="3b8de-132">The effects of enabling logging</span></span>
<span data-ttu-id="3b8de-133">Pokud je povoleno přihlašování, jsou přihlášení k chybám z koncový bod webové služby a Diagnostika **ml diagnostiky** kontejneru objektů blob v účtu úložiště Azure propojené s prostoru uživatele.</span><span class="sxs-lookup"><span data-stu-id="3b8de-133">When logging is enabled, the diagnostics and errors from the web service endpoint are logged in the **ml-diagnostics** blob container in the Azure Storage Account linked with the user’s workspace.</span></span> <span data-ttu-id="3b8de-134">Tento kontejner obsahuje všechny diagnostické informace pro všechny webové služby na koncové body pro všechny pracovní prostory spojené s tímto účtem úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b8de-134">This container holds all the diagnostics information for all the web service endpoints for all the workspaces associated with this storage account.</span></span>

<span data-ttu-id="3b8de-135">Protokoly lze zobrazit pomocí kteréhokoli několik nástrojů, které jsou k dispozici a prozkoumejte účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3b8de-135">The logs can be viewed using any of the several tools available to explore an Azure Storage Account.</span></span> <span data-ttu-id="3b8de-136">Nejsnadnější může být přejděte na účet úložiště na portálu Azure, klikněte na **kontejnery**a potom klikněte na kontejner **ml diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="3b8de-136">The easiest may be to navigate to the storage account in the Azure portal, click **Containers**, and then click the container **ml-diagnostics**.</span></span>  

## <a name="log-blob-detail-information"></a><span data-ttu-id="3b8de-137">Podrobné informace o protokolu objektů blob</span><span class="sxs-lookup"><span data-stu-id="3b8de-137">Log blob detail information</span></span>
<span data-ttu-id="3b8de-138">Každý objekt blob v kontejneru obsahuje diagnostické informace pro právě jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="3b8de-138">Each blob in the container holds the diagnostics information for exactly one of the following actions:</span></span>

* <span data-ttu-id="3b8de-139">Spuštění metody Batch Execution</span><span class="sxs-lookup"><span data-stu-id="3b8de-139">An execution of the Batch-Execution method</span></span>  
* <span data-ttu-id="3b8de-140">Spuštění metody požadavků a odpovědí</span><span class="sxs-lookup"><span data-stu-id="3b8de-140">An execution of the Request-Response method</span></span>  
* <span data-ttu-id="3b8de-141">Inicializace kontejner požadavků a odpovědí</span><span class="sxs-lookup"><span data-stu-id="3b8de-141">Initialization of a Request-Response container</span></span>

<span data-ttu-id="3b8de-142">Název každý objekt blob může mít předponu v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="3b8de-142">The name of each blob has a prefix of the following form:</span></span> 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


<span data-ttu-id="3b8de-143">Kde _protokolu zadejte_ je jedním z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="3b8de-143">Where _Log type_ is one of the following values:</span></span>  

* <span data-ttu-id="3b8de-144">Batch</span><span class="sxs-lookup"><span data-stu-id="3b8de-144">batch</span></span>  
* <span data-ttu-id="3b8de-145">skóre nebo požadavky</span><span class="sxs-lookup"><span data-stu-id="3b8de-145">score/requests</span></span>  
* <span data-ttu-id="3b8de-146">skóre/init</span><span class="sxs-lookup"><span data-stu-id="3b8de-146">score/init</span></span>  

