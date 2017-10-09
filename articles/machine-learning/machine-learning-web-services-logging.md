---
title: "aaaLogging webové služby Machine Learning | Microsoft Docs"
description: "Zjistěte, jak tooenable protokolování pro Machine Learning webové služby. Protokolování poskytuje další informace o toohelp řešení hello rozhraní API."
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
ms.openlocfilehash: ed23933d52d2151af658af2307d7df8743071f65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a><span data-ttu-id="2808d-104">Povolení protokolování webových služeb Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2808d-104">Enable logging for Machine Learning web services</span></span>
<span data-ttu-id="2808d-105">Tento dokument obsahuje informace o protokolování schopností webových služeb Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="2808d-105">This document provides information on hello logging capability of Machine Learning web services.</span></span> <span data-ttu-id="2808d-106">Protokolování poskytuje další informace, kromě právě číslo chyby a zprávu, která vám pomohou vyřešit váš toohello volání rozhraní API pro Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2808d-106">Logging provides additional information, beyond just an error number and a message, that can help you troubleshoot your calls toohello Machine Learning APIs.</span></span>  

## <a name="how-tooenable-logging-for-a-web-service"></a><span data-ttu-id="2808d-107">Jak tooenable protokolování pro webové služby</span><span class="sxs-lookup"><span data-stu-id="2808d-107">How tooenable logging for a Web service</span></span>

<span data-ttu-id="2808d-108">Povolit protokolování z hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu.</span><span class="sxs-lookup"><span data-stu-id="2808d-108">You enable logging from hello [Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span> 

1. <span data-ttu-id="2808d-109">Přihlaste se na portál webové služby Azure Machine Learning toohello [https://services.azureml.net](https://services.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="2808d-109">Sign in toohello Azure Machine Learning Web Services portal at [https://services.azureml.net](https://services.azureml.net).</span></span> <span data-ttu-id="2808d-110">Pro webovou službu Classic, můžete také získat toohello portál kliknutím **nové webové služby prostředí** na stránku webové služby Machine Learning hello v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="2808d-110">For a Classic web service, you can also get toohello portal by clicking **New Web Services Experience** on hello Machine Learning Web Services page in Machine Learning Studio.</span></span>

   ![Nový odkaz webové služby prostředí](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. <span data-ttu-id="2808d-112">V horní nabídce hello, klikněte na tlačítko **webové služby** pro nové webové služby, nebo klikněte na tlačítko **Classic webové služby** pro klasický webové služby.</span><span class="sxs-lookup"><span data-stu-id="2808d-112">On hello top menu bar, click **Web Services** for a New web service, or click **Classic Web Services** for a Classic web service.</span></span>

   ![Vyberte nový nebo Classic webových služeb](media/machine-learning-web-services-logging/select-web-service.png)

3. <span data-ttu-id="2808d-114">Pro novou webovou službu klikněte na název hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="2808d-114">For a New web service, click hello web service name.</span></span> <span data-ttu-id="2808d-115">Pro webovou službu Classic klikněte na název hello webové služby a potom klikněte na další stránku hello hello příslušný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="2808d-115">For a Classic web service, click hello web service name and then on hello next page click hello appropriate endpoint.</span></span>

4. <span data-ttu-id="2808d-116">V horní nabídce hello, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="2808d-116">On hello top menu bar, click **Configure**.</span></span>

5. <span data-ttu-id="2808d-117">Sada hello **povolit protokolování** možnost příliš*chyba* (pouze chyby toolog) nebo *všechny* (pro úplné protokolování).</span><span class="sxs-lookup"><span data-stu-id="2808d-117">Set hello **Enable Logging** option too*Error* (toolog only errors) or *All* (for full logging).</span></span>

   ![Vyberte úroveň protokolování](media/machine-learning-web-services-logging/enable-logging.png)

6. <span data-ttu-id="2808d-119">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2808d-119">Click **Save**.</span></span>

7. <span data-ttu-id="2808d-120">Webové služby Classic, vytvořit hello **ml diagnostiky** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2808d-120">For Classic web services, create hello **ml-diagnostics** container.</span></span>

   <span data-ttu-id="2808d-121">Všechny protokoly webové služby jsou uchovávány v kontejneru objektů blob s názvem **ml diagnostiky** v účtu úložiště hello přidruženého k hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="2808d-121">All web service logs are kept in a blob container named **ml-diagnostics** in hello storage account associated with hello web service.</span></span> <span data-ttu-id="2808d-122">Pro nové webové služby se tento kontejner vytvoří hello prvním spuštění aplikace hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="2808d-122">For New web services, this container is created hello first time you access hello web service.</span></span> <span data-ttu-id="2808d-123">Webové služby Classic je nutné toocreate hello kontejneru, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2808d-123">For Classic web services, you need toocreate hello container if it doesn't already exist.</span></span> 

   1. <span data-ttu-id="2808d-124">V hello [portál Azure](https://portal.azure.com), přejděte toohello účtu úložiště přidruženého k hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="2808d-124">In hello [Azure portal](https://portal.azure.com), go toohello storage account associated with hello web service.</span></span>

   2. <span data-ttu-id="2808d-125">V části **služby objektů Blob**, klikněte na tlačítko **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="2808d-125">Under **Blob Service**, click **Containers**.</span></span>

   3. <span data-ttu-id="2808d-126">Pokud kontejner hello **ml diagnostiky** neexistuje, klikněte na tlačítko **+ kontejner**přidělte hello kontejneru hello název "ml – Diagnostika" a vyberte hello **přistupovat typu** jako "Blob".</span><span class="sxs-lookup"><span data-stu-id="2808d-126">If hello container **ml-diagnostics** doesn't exist, click **+Container**, give hello container hello name "ml-diagnostics", and select hello **Access type** as "Blob".</span></span> <span data-ttu-id="2808d-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2808d-127">Click **OK**.</span></span>

      ![Vyberte úroveň protokolování](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> <span data-ttu-id="2808d-129">Pro webovou službu Classic má hello řídicího panelu webové služby v nástroji Machine Learning Studio také tooenable protokolování přepínače.</span><span class="sxs-lookup"><span data-stu-id="2808d-129">For a Classic web service, hello Web Services Dashboard in Machine Learning Studio also has a switch tooenable logging.</span></span> <span data-ttu-id="2808d-130">Protože protokolování se teď spravují prostřednictvím portálu hello webové služby, je však třeba tooenable protokolování prostřednictvím hello portálu, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2808d-130">However, because logging is now managed through hello Web Services portal, you need tooenable logging through hello portal as described in this article.</span></span> <span data-ttu-id="2808d-131">Pokud jste povolili protokolování v sadě Studio, v hello webový portál služby, zakázání protokolování a znovu ji povolte.</span><span class="sxs-lookup"><span data-stu-id="2808d-131">If you already enabled logging in Studio, then in hello Web Services Portal, disable logging and enable it again.</span></span>


## <a name="hello-effects-of-enabling-logging"></a><span data-ttu-id="2808d-132">účinky Hello povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="2808d-132">hello effects of enabling logging</span></span>
<span data-ttu-id="2808d-133">Pokud je povoleno protokolování, hello diagnostiky a chyb z koncový bod webové služby hello přihlášeni hello **ml diagnostiky** kontejneru objektů blob v účtu úložiště Azure hello propojené s prostoru hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="2808d-133">When logging is enabled, hello diagnostics and errors from hello web service endpoint are logged in hello **ml-diagnostics** blob container in hello Azure Storage Account linked with hello user’s workspace.</span></span> <span data-ttu-id="2808d-134">Tento kontejner obsahuje všechny hello diagnostické informace pro všechny hello koncových bodů webové služby pro všechny pracovní prostory hello přidruženého k tomuto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2808d-134">This container holds all hello diagnostics information for all hello web service endpoints for all hello workspaces associated with this storage account.</span></span>

<span data-ttu-id="2808d-135">protokoly Hello lze zobrazit pomocí kteréhokoli hello několik nástrojů k dispozici tooexplore účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2808d-135">hello logs can be viewed using any of hello several tools available tooexplore an Azure Storage Account.</span></span> <span data-ttu-id="2808d-136">Hello nejjednodušší může být účet úložiště toohello toonavigate hello portálu Azure, klikněte na tlačítko **kontejnery**a potom klikněte na kontejner hello **ml diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="2808d-136">hello easiest may be toonavigate toohello storage account in hello Azure portal, click **Containers**, and then click hello container **ml-diagnostics**.</span></span>  

## <a name="log-blob-detail-information"></a><span data-ttu-id="2808d-137">Podrobné informace o protokolu objektů blob</span><span class="sxs-lookup"><span data-stu-id="2808d-137">Log blob detail information</span></span>
<span data-ttu-id="2808d-138">Každý objekt blob v kontejneru hello obsahuje hello diagnostické informace pro právě jeden z hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="2808d-138">Each blob in hello container holds hello diagnostics information for exactly one of hello following actions:</span></span>

* <span data-ttu-id="2808d-139">Spuštění metody hello Batch Execution</span><span class="sxs-lookup"><span data-stu-id="2808d-139">An execution of hello Batch-Execution method</span></span>  
* <span data-ttu-id="2808d-140">Spuštění metody hello požadavků a odpovědí</span><span class="sxs-lookup"><span data-stu-id="2808d-140">An execution of hello Request-Response method</span></span>  
* <span data-ttu-id="2808d-141">Inicializace kontejner požadavků a odpovědí</span><span class="sxs-lookup"><span data-stu-id="2808d-141">Initialization of a Request-Response container</span></span>

<span data-ttu-id="2808d-142">Název Hello každý objekt blob může mít předponu hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="2808d-142">hello name of each blob has a prefix of hello following form:</span></span> 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


<span data-ttu-id="2808d-143">Kde _protokolu zadejte_ je jedním z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2808d-143">Where _Log type_ is one of hello following values:</span></span>  

* <span data-ttu-id="2808d-144">Batch</span><span class="sxs-lookup"><span data-stu-id="2808d-144">batch</span></span>  
* <span data-ttu-id="2808d-145">skóre nebo požadavky</span><span class="sxs-lookup"><span data-stu-id="2808d-145">score/requests</span></span>  
* <span data-ttu-id="2808d-146">skóre/init</span><span class="sxs-lookup"><span data-stu-id="2808d-146">score/init</span></span>  

