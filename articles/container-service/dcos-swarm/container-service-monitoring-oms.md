---
title: cluster Azure DC/OS aaaMonitor - Operations Management | Microsoft Docs
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
ms.openlocfilehash: 0ebfa3ba3cef8f0205b15731b0e91f5b304bc8fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a><span data-ttu-id="a83cc-103">Monitorování clusteru Azure Container Service DC/OS s Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="a83cc-103">Monitor an Azure Container Service DC/OS cluster with Operations Management Suite</span></span>

<span data-ttu-id="a83cc-104">Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a83cc-104">Microsoft Operations Management Suite (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="a83cc-105">Kontejner řešení je řešení v OMS analýzy protokolů, které vám umožní zobrazit hello kontejneru inventář, výkonu a protokoly na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="a83cc-105">Container Solution is a solution in OMS Log Analytics, which helps you view hello container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="a83cc-106">Můžete auditovat, řešení potíží s kontejnery zobrazením hello protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="a83cc-106">You can audit, troubleshoot containers by viewing hello logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="a83cc-107">Další informace o řešení kontejneru naleznete toothe [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="a83cc-107">For more information about Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="setting-up-oms-from-hello-dcos-universe"></a><span data-ttu-id="a83cc-108">Nastavení OMS z universe hello DC/OS</span><span class="sxs-lookup"><span data-stu-id="a83cc-108">Setting up OMS from hello DC/OS universe</span></span>


<span data-ttu-id="a83cc-109">Tento článek předpokládá, že jste nastavili DC/OS a nasadili jednoduchého webového kontejneru aplikace v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a83cc-109">This article assumes that you have set up an DC/OS and have deployed simple web container applications on hello cluster.</span></span>

### <a name="pre-requisite"></a><span data-ttu-id="a83cc-110">Předpoklad</span><span class="sxs-lookup"><span data-stu-id="a83cc-110">Pre-requisite</span></span>
- <span data-ttu-id="a83cc-111">[Předplatné služby Microsoft Azure](https://azure.microsoft.com/free/) -vám to zdarma.</span><span class="sxs-lookup"><span data-stu-id="a83cc-111">[Microsoft Azure Subscription](https://azure.microsoft.com/free/) - You can get this for free.</span></span>  
- <span data-ttu-id="a83cc-112">Instalační program pracovní prostor Microsoft OMS – najdete v části "Krok 3" pod</span><span class="sxs-lookup"><span data-stu-id="a83cc-112">Microsoft OMS Workspace Setup - see "Step 3" below</span></span>
- <span data-ttu-id="a83cc-113">[Rozhraní příkazového řádku DC/OS](https://dcos.io/docs/1.8/usage/cli/install/) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="a83cc-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installed.</span></span>

1. <span data-ttu-id="a83cc-114">Na řídicím panelu hello DC/OS klikněte na Universe a vyhledejte "OMS, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="a83cc-114">In hello DC/OS dashboard, click on Universe and search for ‘OMS’ as shown below.</span></span>

![](media/container-service-monitoring-oms/image2.png)

2. <span data-ttu-id="a83cc-115">Klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="a83cc-115">Click **Install**.</span></span> <span data-ttu-id="a83cc-116">Uvidíte pop až s informace o verzi hello OMS a **instalovat balíček** nebo **rozšířený instalace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a83cc-116">You will see a pop up with hello OMS version information and an **Install Package** or **Advanced Installation** button.</span></span> <span data-ttu-id="a83cc-117">Když kliknete na tlačítko **rozšířený instalace**, což vede toohello **vlastnosti konkrétní konfigurace OMS** stránky.</span><span class="sxs-lookup"><span data-stu-id="a83cc-117">When you click **Advanced Installation**, which leads you toohello **OMS specific configuration properties** page.</span></span>

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. <span data-ttu-id="a83cc-118">Zde, zobrazí se výzva tooenter hello `wsid` (hello ID pracovního prostoru OMS) a `wskey` (hello OMS primární klíč pro id pracovního prostoru hello).</span><span class="sxs-lookup"><span data-stu-id="a83cc-118">Here, you will be asked tooenter hello `wsid` (hello OMS workspace ID) and `wskey` (hello OMS primary key for hello workspace id).</span></span> <span data-ttu-id="a83cc-119">tooget obou `wsid` a `wskey` potřebovat toocreate účet OMS na <https://mms.microsoft.com>. Postupujte podle kroků toocreate hello účet.</span><span class="sxs-lookup"><span data-stu-id="a83cc-119">tooget both `wsid` and `wskey` you need toocreate an OMS account at <https://mms.microsoft.com>. Please follow hello steps toocreate an account.</span></span> <span data-ttu-id="a83cc-120">Po dokončení vytváření hello účet, bude třeba tooobtain vaše `wsid` a `wskey` kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux** , jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="a83cc-120">Once you are done creating hello account, you need tooobtain your `wsid` and `wskey` by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

4. <span data-ttu-id="a83cc-121">Vyberte hello číslo vám OMS instancí a klikněte na tlačítko "Zkontrolovat a nainstalovat" hello.</span><span class="sxs-lookup"><span data-stu-id="a83cc-121">Select hello number you OMS instances that you want and click hello ‘Review and Install’ button.</span></span> <span data-ttu-id="a83cc-122">Obvykle můžete toohave hello počet OMS instance stejné toohello Virtuálního počítače budete mít v clusteru agenta.</span><span class="sxs-lookup"><span data-stu-id="a83cc-122">Typically, you will want toohave hello number of OMS instances equal toohello number of VM’s you have in your agent cluster.</span></span> <span data-ttu-id="a83cc-123">Agent OMS pro Linux je nainstaluje jako jednotlivé kontejnery pro každý virtuální počítač, zda chce toocollect informace pro monitorování a informace o protokolování.</span><span class="sxs-lookup"><span data-stu-id="a83cc-123">OMS Agent for Linux is installs as individual containers on each VM that it wants toocollect information for monitoring and logging information.</span></span>

## <a name="setting-up-a-simple-oms-dashboard"></a><span data-ttu-id="a83cc-124">Nastavení jednoduchý řídicí panel OMS</span><span class="sxs-lookup"><span data-stu-id="a83cc-124">Setting up a simple OMS dashboard</span></span>

<span data-ttu-id="a83cc-125">Po instalaci hello OMS agenta pro Linux na virtuálních počítačích hello, dalším krokem je tooset si řídicí panel hello OMS.</span><span class="sxs-lookup"><span data-stu-id="a83cc-125">Once you have installed hello OMS Agent for Linux on hello VMs, next step is tooset up hello OMS dashboard.</span></span> <span data-ttu-id="a83cc-126">Existují dva způsoby toodo to: OMS portál nebo portál Azure.</span><span class="sxs-lookup"><span data-stu-id="a83cc-126">There are two ways toodo this: OMS Portal or Azure Portal.</span></span>

### <a name="oms-portal"></a><span data-ttu-id="a83cc-127">Portálu OMS</span><span class="sxs-lookup"><span data-stu-id="a83cc-127">OMS Portal</span></span> 

<span data-ttu-id="a83cc-128">Přihlaste se na portálu OMS toohello (<https://mms.microsoft.com>) a přejděte toohello **řešení Galerie**.</span><span class="sxs-lookup"><span data-stu-id="a83cc-128">Log in toohello OMS portal (<https://mms.microsoft.com>) and go toohello **Solution Gallery**.</span></span>

![](media/container-service-monitoring-oms/image6.png)

<span data-ttu-id="a83cc-129">Jakmile jste na hello **řešení Galerie**, vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="a83cc-129">Once you are in hello **Solution Gallery**, select **Containers**.</span></span>

![](media/container-service-monitoring-oms/image7.png)

<span data-ttu-id="a83cc-130">Po dokončení výběru hello kontejneru řešení, zobrazí se dlaždice na stránce Přehled OMS řídicí panel hello hello.</span><span class="sxs-lookup"><span data-stu-id="a83cc-130">Once you’ve selected hello Container Solution, you will see hello tile on hello OMS Overview Dashboard page.</span></span> <span data-ttu-id="a83cc-131">Jakmile je indexovaný hello požity kontejner dat, zobrazí se dlaždice hello naplněný informace o řešení dlaždice zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a83cc-131">Once hello ingested container data is indexed, you will see hello tile populated with information on the solution view tiles.</span></span>

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a><span data-ttu-id="a83cc-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a83cc-132">Azure Portal</span></span> 

<span data-ttu-id="a83cc-133">Přihlášení na portál tooAzure <https://portal.microsoft.com/>.</span><span class="sxs-lookup"><span data-stu-id="a83cc-133">Login tooAzure portal at <https://portal.microsoft.com/>.</span></span> <span data-ttu-id="a83cc-134">Přejděte na **Marketplace**, vyberte **monitorování + správu** a klikněte na tlačítko **najdete v článku všechny**.</span><span class="sxs-lookup"><span data-stu-id="a83cc-134">Go to **Marketplace**, select **Monitoring + management** and click **See All**.</span></span> <span data-ttu-id="a83cc-135">Pak zadejte `containers` ve vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="a83cc-135">Then Type `containers` in search.</span></span> <span data-ttu-id="a83cc-136">Zobrazí se ve výsledcích hledání hello – "kontejnery".</span><span class="sxs-lookup"><span data-stu-id="a83cc-136">You will see "containers" in hello search results.</span></span> <span data-ttu-id="a83cc-137">Vyberte **kontejnery** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a83cc-137">Select **Containers** and click **Create**.</span></span>

![](media/container-service-monitoring-oms/image9.png)

<span data-ttu-id="a83cc-138">Po kliknutí na tlačítko **vytvořit**, se zobrazí výzvu k pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="a83cc-138">Once you click **Create**, it will ask you for your workspace.</span></span> <span data-ttu-id="a83cc-139">Vyberte pracovní prostor nebo pokud jeden nemáte, vytvořte nový pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="a83cc-139">Select your workspace or if you do not have one, create a new workspace.</span></span>

![](media/container-service-monitoring-oms/image10.PNG)

<span data-ttu-id="a83cc-140">Po dokončení výběru pracovního prostoru, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a83cc-140">Once you’ve selected your workspace, click **Create**.</span></span>

![](media/container-service-monitoring-oms/image11.png)

<span data-ttu-id="a83cc-141">Další informace o hello OMS kontejneru řešení naleznete toothe [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="a83cc-141">For more information about hello OMS Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

### <a name="how-tooscale-oms-agent-with-acs-dcos"></a><span data-ttu-id="a83cc-142">Jak tooscale agenta OMS s ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="a83cc-142">How tooscale OMS Agent with ACS DC/OS</span></span> 

<span data-ttu-id="a83cc-143">V případě, že potřebujete toohave nainstalovaný agent OMS souborem hello uzlu skutečný počet nebo jsou vertikálním navýšení kapacity VMSS přidáním více virtuálních počítačů, můžete tak učinit pomocí příjmu hello `msoms` služby.</span><span class="sxs-lookup"><span data-stu-id="a83cc-143">In case you need toohave installed OMS agent short of hello actual node count or you are scaling up VMSS by adding more VM, you can do so by scaling hello `msoms` service.</span></span>

<span data-ttu-id="a83cc-144">Můžete se vrátit tooMarathon nebo hello kartě služeb uživatelského rozhraní DC/OS a škálovat vaše počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="a83cc-144">You can either go tooMarathon or hello DC/OS UI Services tab and scale up your node count.</span></span>

![](media/container-service-monitoring-oms/image12.PNG)

<span data-ttu-id="a83cc-145">To nasadí tooother uzly, které ještě nebyly nasadit agenta OMS hello.</span><span class="sxs-lookup"><span data-stu-id="a83cc-145">This will deploy tooother nodes which have not yet deployed hello OMS agent.</span></span>

## <a name="uninstall-ms-oms"></a><span data-ttu-id="a83cc-146">Odinstalujte MS OMS</span><span class="sxs-lookup"><span data-stu-id="a83cc-146">Uninstall MS OMS</span></span>

<span data-ttu-id="a83cc-147">toouninstall MS OMS zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a83cc-147">toouninstall MS OMS enter hello following command:</span></span>

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a><span data-ttu-id="a83cc-148">Dejte nám vědět!</span><span class="sxs-lookup"><span data-stu-id="a83cc-148">Let us know!!!</span></span>
<span data-ttu-id="a83cc-149">Co funguje?</span><span class="sxs-lookup"><span data-stu-id="a83cc-149">What works?</span></span> <span data-ttu-id="a83cc-150">Co je chybějící?</span><span class="sxs-lookup"><span data-stu-id="a83cc-150">What is missing?</span></span> <span data-ttu-id="a83cc-151">Co potřebujete pro tento toobe užitečné pro vás?</span><span class="sxs-lookup"><span data-stu-id="a83cc-151">What else do you need for this toobe useful for you?</span></span> <span data-ttu-id="a83cc-152">Dejte nám vědět v <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span><span class="sxs-lookup"><span data-stu-id="a83cc-152">Let us know at <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a83cc-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a83cc-153">Next steps</span></span>

 <span data-ttu-id="a83cc-154">Teď, když jste nastavili OMS toomonitor kontejnerů,[najdete v části řídicího panelu kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="a83cc-154">Now that you have set up OMS toomonitor your containers,[see your container dashboard](../../log-analytics/log-analytics-containers.md).</span></span>
