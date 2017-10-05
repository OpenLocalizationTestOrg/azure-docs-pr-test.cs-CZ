---
title: "Cluster Azure DC/OS monitorování - Operations Management | Microsoft Docs"
description: "Monitorování clusteru Azure Container Service DC/OS s Microsoft Operations Management Suite."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 9b8f96b34b53982c469273a3df9751ceb7930d60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a><span data-ttu-id="73831-103">Monitorování clusteru Azure Container Service DC/OS s Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="73831-103">Monitor an Azure Container Service DC/OS cluster with Operations Management Suite</span></span>

<span data-ttu-id="73831-104">Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="73831-104">Microsoft Operations Management Suite (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="73831-105">Kontejner řešení je řešení v OMS analýzy protokolů, který umožňuje zobrazit inventář kontejneru, výkonu a protokoly na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="73831-105">Container Solution is a solution in OMS Log Analytics, which helps you view the container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="73831-106">Můžete auditovat, řešení potíží s kontejnery zobrazením protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="73831-106">You can audit, troubleshoot containers by viewing the logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="73831-107">Další informace o kontejneru řešení, naleznete [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="73831-107">For more information about Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="setting-up-oms-from-the-dcos-universe"></a><span data-ttu-id="73831-108">Nastavení OMS z universe DC/OS</span><span class="sxs-lookup"><span data-stu-id="73831-108">Setting up OMS from the DC/OS universe</span></span>


<span data-ttu-id="73831-109">Tento článek předpokládá, že jste nastavili DC/OS a nasadili jednoduchého webového kontejneru aplikace v clusteru.</span><span class="sxs-lookup"><span data-stu-id="73831-109">This article assumes that you have set up an DC/OS and have deployed simple web container applications on the cluster.</span></span>

### <a name="pre-requisite"></a><span data-ttu-id="73831-110">Předpoklad</span><span class="sxs-lookup"><span data-stu-id="73831-110">Pre-requisite</span></span>
- <span data-ttu-id="73831-111">[Předplatné služby Microsoft Azure](https://azure.microsoft.com/free/) -vám to zdarma.</span><span class="sxs-lookup"><span data-stu-id="73831-111">[Microsoft Azure Subscription](https://azure.microsoft.com/free/) - You can get this for free.</span></span>  
- <span data-ttu-id="73831-112">Instalační program pracovní prostor Microsoft OMS – najdete v části "Krok 3" pod</span><span class="sxs-lookup"><span data-stu-id="73831-112">Microsoft OMS Workspace Setup - see "Step 3" below</span></span>
- <span data-ttu-id="73831-113">[Rozhraní příkazového řádku DC/OS](https://dcos.io/docs/1.8/usage/cli/install/) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="73831-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installed.</span></span>

1. <span data-ttu-id="73831-114">Na řídicím panelu DC/OS klikněte na Universe a vyhledejte "OMS, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="73831-114">In the DC/OS dashboard, click on Universe and search for ‘OMS’ as shown below.</span></span>

![](media/container-service-monitoring-oms/image2.png)

2. <span data-ttu-id="73831-115">Klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="73831-115">Click **Install**.</span></span> <span data-ttu-id="73831-116">Uvidíte pop až s informace o verzi OMS a **instalovat balíček** nebo **rozšířený instalace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="73831-116">You will see a pop up with the OMS version information and an **Install Package** or **Advanced Installation** button.</span></span> <span data-ttu-id="73831-117">Když kliknete na tlačítko **rozšířený instalace**, což vede k **vlastnosti konkrétní konfigurace OMS** stránky.</span><span class="sxs-lookup"><span data-stu-id="73831-117">When you click **Advanced Installation**, which leads you to the **OMS specific configuration properties** page.</span></span>

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. <span data-ttu-id="73831-118">Zde, zobrazí se výzva k zadání `wsid` (ID pracovního prostoru OMS) a `wskey` (OMS primární klíč pro id pracovního prostoru).</span><span class="sxs-lookup"><span data-stu-id="73831-118">Here, you will be asked to enter the `wsid` (the OMS workspace ID) and `wskey` (the OMS primary key for the workspace id).</span></span> <span data-ttu-id="73831-119">Chcete-li získat i `wsid` a `wskey` je potřeba vytvořit na účet OMS <https://mms.microsoft.com>. Postupujte podle kroků pro vytvoření účtu.</span><span class="sxs-lookup"><span data-stu-id="73831-119">To get both `wsid` and `wskey` you need to create an OMS account at <https://mms.microsoft.com>. Please follow the steps to create an account.</span></span> <span data-ttu-id="73831-120">Po dokončení vytváření účtu, je nutné získat vaše `wsid` a `wskey` kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux**, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="73831-120">Once you are done creating the account, you need to obtain your `wsid` and `wskey` by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

4. <span data-ttu-id="73831-121">Vyberte číslo vám OMS instance a klikněte na tlačítko 'Zkontrolovat a nainstalovat'.</span><span class="sxs-lookup"><span data-stu-id="73831-121">Select the number you OMS instances that you want and click the ‘Review and Install’ button.</span></span> <span data-ttu-id="73831-122">Obvykle můžete získat počet instancí OMS rovná počtu Virtuálního počítače budete mít v clusteru agenta.</span><span class="sxs-lookup"><span data-stu-id="73831-122">Typically, you will want to have the number of OMS instances equal to the number of VM’s you have in your agent cluster.</span></span> <span data-ttu-id="73831-123">Agent OMS pro Linux je nainstaluje jako jednotlivé kontejnery pro každý virtuální počítač, který chce shromažďovat informace o monitorování a informace o protokolování.</span><span class="sxs-lookup"><span data-stu-id="73831-123">OMS Agent for Linux is installs as individual containers on each VM that it wants to collect information for monitoring and logging information.</span></span>

## <a name="setting-up-a-simple-oms-dashboard"></a><span data-ttu-id="73831-124">Nastavení jednoduchý řídicí panel OMS</span><span class="sxs-lookup"><span data-stu-id="73831-124">Setting up a simple OMS dashboard</span></span>

<span data-ttu-id="73831-125">Po instalaci agenta OMS pro Linux na virtuálních počítačích, dalším krokem je nastavit řídicím panelu OMS.</span><span class="sxs-lookup"><span data-stu-id="73831-125">Once you have installed the OMS Agent for Linux on the VMs, next step is to set up the OMS dashboard.</span></span> <span data-ttu-id="73831-126">Existují dva způsoby, jak to udělat: OMS portál nebo portál Azure.</span><span class="sxs-lookup"><span data-stu-id="73831-126">There are two ways to do this: OMS Portal or Azure Portal.</span></span>

### <a name="oms-portal"></a><span data-ttu-id="73831-127">Portálu OMS</span><span class="sxs-lookup"><span data-stu-id="73831-127">OMS Portal</span></span> 

<span data-ttu-id="73831-128">Přihlaste se k portálu OMS (<https://mms.microsoft.com>) a přejděte na **řešení Galerie**.</span><span class="sxs-lookup"><span data-stu-id="73831-128">Log in to the OMS portal (<https://mms.microsoft.com>) and go to the **Solution Gallery**.</span></span>

![](media/container-service-monitoring-oms/image6.png)

<span data-ttu-id="73831-129">Jakmile jste na **řešení Galerie**, vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="73831-129">Once you are in the **Solution Gallery**, select **Containers**.</span></span>

![](media/container-service-monitoring-oms/image7.png)

<span data-ttu-id="73831-130">Po dokončení výběru řešení kontejneru, zobrazí se na dlaždici na stránce Přehled OMS řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="73831-130">Once you’ve selected the Container Solution, you will see the tile on the OMS Overview Dashboard page.</span></span> <span data-ttu-id="73831-131">Jakmile je indexovaný ingestovaný kontejner dat, zobrazí se dlaždici naplněný informace o řešení dlaždice zobrazení.</span><span class="sxs-lookup"><span data-stu-id="73831-131">Once the ingested container data is indexed, you will see the tile populated with information on the solution view tiles.</span></span>

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a><span data-ttu-id="73831-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="73831-132">Azure Portal</span></span> 

<span data-ttu-id="73831-133">Přihlášení k portálu Azure v <https://portal.microsoft.com/>.</span><span class="sxs-lookup"><span data-stu-id="73831-133">Login to Azure portal at <https://portal.microsoft.com/>.</span></span> <span data-ttu-id="73831-134">Přejděte na **Marketplace**, vyberte **monitorování + správu** a klikněte na tlačítko **najdete v článku všechny**.</span><span class="sxs-lookup"><span data-stu-id="73831-134">Go to **Marketplace**, select **Monitoring + management** and click **See All**.</span></span> <span data-ttu-id="73831-135">Pak zadejte `containers` ve vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="73831-135">Then Type `containers` in search.</span></span> <span data-ttu-id="73831-136">Zobrazí se "kontejner" ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="73831-136">You will see "containers" in the search results.</span></span> <span data-ttu-id="73831-137">Vyberte **kontejnery** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="73831-137">Select **Containers** and click **Create**.</span></span>

![](media/container-service-monitoring-oms/image9.png)

<span data-ttu-id="73831-138">Po kliknutí na tlačítko **vytvořit**, se zobrazí výzvu k pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="73831-138">Once you click **Create**, it will ask you for your workspace.</span></span> <span data-ttu-id="73831-139">Vyberte pracovní prostor nebo pokud jeden nemáte, vytvořte nový pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="73831-139">Select your workspace or if you do not have one, create a new workspace.</span></span>

![](media/container-service-monitoring-oms/image10.PNG)

<span data-ttu-id="73831-140">Po dokončení výběru pracovního prostoru, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="73831-140">Once you’ve selected your workspace, click **Create**.</span></span>

![](media/container-service-monitoring-oms/image11.png)

<span data-ttu-id="73831-141">Další informace o řešení kontejneru OMS, naleznete [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="73831-141">For more information about the OMS Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a><span data-ttu-id="73831-142">Postup škálování agenta OMS s ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="73831-142">How to scale OMS Agent with ACS DC/OS</span></span> 

<span data-ttu-id="73831-143">V případě, že je potřeba mít nainstalovaný agent OMS souborem uzlu skutečný počet nebo jsou vertikálním navýšení kapacity VMSS přidáním více virtuálních počítačů, můžete tak učinit pomocí příjmu `msoms` služby.</span><span class="sxs-lookup"><span data-stu-id="73831-143">In case you need to have installed OMS agent short of the actual node count or you are scaling up VMSS by adding more VM, you can do so by scaling the `msoms` service.</span></span>

<span data-ttu-id="73831-144">Můžete přejít na Marathon nebo na kartě služeb uživatelského rozhraní DC/OS a škálovat vaše počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="73831-144">You can either go to Marathon or the DC/OS UI Services tab and scale up your node count.</span></span>

![](media/container-service-monitoring-oms/image12.PNG)

<span data-ttu-id="73831-145">To nasadí do dalších uzlů, které ještě nebyly nasadit agenta OMS.</span><span class="sxs-lookup"><span data-stu-id="73831-145">This will deploy to other nodes which have not yet deployed the OMS agent.</span></span>

## <a name="uninstall-ms-oms"></a><span data-ttu-id="73831-146">Odinstalujte MS OMS</span><span class="sxs-lookup"><span data-stu-id="73831-146">Uninstall MS OMS</span></span>

<span data-ttu-id="73831-147">Chcete-li odinstalovat MS OMS zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="73831-147">To uninstall MS OMS enter the following command:</span></span>

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a><span data-ttu-id="73831-148">Dejte nám vědět!</span><span class="sxs-lookup"><span data-stu-id="73831-148">Let us know!!!</span></span>
<span data-ttu-id="73831-149">Co funguje?</span><span class="sxs-lookup"><span data-stu-id="73831-149">What works?</span></span> <span data-ttu-id="73831-150">Co je chybějící?</span><span class="sxs-lookup"><span data-stu-id="73831-150">What is missing?</span></span> <span data-ttu-id="73831-151">Co je potřeba pro to pro vás užitečné?</span><span class="sxs-lookup"><span data-stu-id="73831-151">What else do you need for this to be useful for you?</span></span> <span data-ttu-id="73831-152">Dejte nám vědět v <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span><span class="sxs-lookup"><span data-stu-id="73831-152">Let us know at <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73831-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73831-153">Next steps</span></span>

 <span data-ttu-id="73831-154">Teď, když jste nastavili OMS monitorování kontejnerů,[najdete v části řídicího panelu kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="73831-154">Now that you have set up OMS to monitor your containers,[see your container dashboard](../../log-analytics/log-analytics-containers.md).</span></span>
