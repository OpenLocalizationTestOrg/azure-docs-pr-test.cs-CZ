---
title: cluster Azure Kubernetes aaaMonitor - Operations Management | Microsoft Docs
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí služby Microsoft Operations Management Suite"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 7474ee1571134ffe43ff8e4041cf5a64f5635bb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a><span data-ttu-id="eaceb-103">Monitorování clusteru Azure Container Service s Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="eaceb-103">Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eaceb-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eaceb-104">Prerequisites</span></span>
<span data-ttu-id="eaceb-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="eaceb-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="eaceb-106">Také předpokládá, že máte hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="eaceb-106">It also assumes that you have hello `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="eaceb-107">Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="eaceb-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="eaceb-108">Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="eaceb-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>  
<span data-ttu-id="eaceb-109">Alternativně můžete použít [prostředí cloudu Azure](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), který má hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje pro je již nainstalována.</span><span class="sxs-lookup"><span data-stu-id="eaceb-109">Alternatively, you can use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), which has hello `az` Azure cli and `kubectl` tools already installed for you.</span></span>  

<span data-ttu-id="eaceb-110">Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="eaceb-110">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="eaceb-111">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="eaceb-111">If you don't have `kubectl` installed, you can run:</span></span>
```console
$ az acs kubernetes install-cli
```

<span data-ttu-id="eaceb-112">Pokud máte nainstalovaný ve vašem kubectl nástroj klíče kubernetes tootest můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="eaceb-112">tootest if you have kubernetes keys installed in your kubectl tool you can run:</span></span>
```console
$ kubectl get nodes
```

<span data-ttu-id="eaceb-113">Pokud hello výše příkaz chyby out, musíte do vaší kubectl nástroje tooinstall kubernetes clusteru klíče.</span><span class="sxs-lookup"><span data-stu-id="eaceb-113">If hello above command errors out, you need tooinstall kubernetes cluster keys into your kubectl tool.</span></span> <span data-ttu-id="eaceb-114">Můžete to udělat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="eaceb-114">You can do that with hello following command:</span></span>
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a><span data-ttu-id="eaceb-115">Monitorování kontejnery s služby Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="eaceb-115">Monitoring Containers with Operations Management Suite (OMS)</span></span>

<span data-ttu-id="eaceb-116">Microsoft Operations Management (OMS) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="eaceb-116">Microsoft Operations Management (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="eaceb-117">Kontejner řešení je řešení v OMS analýzy protokolů, které vám umožní zobrazit hello kontejneru inventář, výkonu a protokoly na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="eaceb-117">Container Solution is a solution in OMS Log Analytics, which helps you view hello container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="eaceb-118">Můžete auditovat, řešení potíží s kontejnery zobrazením hello protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="eaceb-118">You can audit, troubleshoot containers by viewing hello logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="eaceb-119">Další informace o řešení kontejneru naleznete toothe [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="eaceb-119">For more information about Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="installing-oms-on-kubernetes"></a><span data-ttu-id="eaceb-120">Instalace OMS na Kubernetes</span><span class="sxs-lookup"><span data-stu-id="eaceb-120">Installing OMS on Kubernetes</span></span>

### <a name="obtain-your-workspace-id-and-key"></a><span data-ttu-id="eaceb-121">Získání ID a klíč</span><span class="sxs-lookup"><span data-stu-id="eaceb-121">Obtain your workspace ID and key</span></span>
<span data-ttu-id="eaceb-122">Pro hello služba toohello tootalk agenta OMS musí toobe nakonfigurovat id pracovního prostoru a klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="eaceb-122">For hello OMS agent tootalk toohello service it needs toobe configured with a workspace id and a workspace key.</span></span> <span data-ttu-id="eaceb-123">id pracovního prostoru hello tooget a klíč musíte toocreate OMS účtu v <https://mms.microsoft.com>. Postupujte podle kroků toocreate hello účet.</span><span class="sxs-lookup"><span data-stu-id="eaceb-123">tooget hello workspace id and key you need toocreate an OMS account at <https://mms.microsoft.com>. Please follow hello steps toocreate an account.</span></span> <span data-ttu-id="eaceb-124">Po dokončení vytváření hello účet, bude třeba tooobtain id a klíč kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux**, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="eaceb-124">Once you are done creating hello account, you need tooobtain your id and key by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-hello-oms-agent-using-a-daemonset"></a><span data-ttu-id="eaceb-125">Instalace agenta OMS hello pomocí DaemonSet</span><span class="sxs-lookup"><span data-stu-id="eaceb-125">Install hello OMS agent using a DaemonSet</span></span>
<span data-ttu-id="eaceb-126">DaemonSets používají Kubernetes toorun jednu instanci kontejner na jednotlivých hostitelích v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="eaceb-126">DaemonSets are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="eaceb-127">Jsou ideální pro spuštění monitorovací agenty.</span><span class="sxs-lookup"><span data-stu-id="eaceb-127">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="eaceb-128">Tady je hello [DaemonSet YAML soubor](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span><span class="sxs-lookup"><span data-stu-id="eaceb-128">Here is hello [DaemonSet YAML file](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span></span> <span data-ttu-id="eaceb-129">Uložte ho tooa soubor s názvem `oms-daemonset.yaml` a nahraďte zástupný symbol hodnoty hello `WSID` a `KEY` s vaše id a klíč v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="eaceb-129">Save it tooa file named `oms-daemonset.yaml` and replace hello place-holder values for `WSID` and `KEY` with your workspace id and key in hello file.</span></span>

<span data-ttu-id="eaceb-130">Po přidání vaše ID a klíč toohello DaemonSet konfigurace, můžete nainstalovat agenta OMS hello v clusteru s hello `kubectl` nástroj pro příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="eaceb-130">Once you have added your workspace ID and key toohello DaemonSet configuration, you can install hello OMS agent on your cluster with hello `kubectl` command line tool:</span></span>

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-hello-oms-agent-using-a-kubernetes-secret"></a><span data-ttu-id="eaceb-131">Instalace agenta OMS hello pomocí Kubernetes tajný klíč</span><span class="sxs-lookup"><span data-stu-id="eaceb-131">Installing hello OMS agent using a Kubernetes Secret</span></span>
<span data-ttu-id="eaceb-132">tooprotect vaše OMS ID a klíč Kubernetes tajný klíč slouží jako součást DaemonSet YAML souboru.</span><span class="sxs-lookup"><span data-stu-id="eaceb-132">tooprotect your OMS workspace ID and key you can use Kubernetes Secret as a part of DaemonSet YAML file.</span></span>

 - <span data-ttu-id="eaceb-133">Zkopírujte skript hello, soubor tajný šablony a hello DaemonSet YAML souboru (z [úložiště](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) a ujistěte se, že jsou na hello stejný adresář.</span><span class="sxs-lookup"><span data-stu-id="eaceb-133">Copy hello script, secret template file and hello DaemonSet YAML file (from [repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) and make sure they are on hello same directory.</span></span> 
      - <span data-ttu-id="eaceb-134">Generování skriptu - tajný klíč gen.sh tajný klíč</span><span class="sxs-lookup"><span data-stu-id="eaceb-134">secret generating script - secret-gen.sh</span></span>
      - <span data-ttu-id="eaceb-135">Šablona tajné – template.yaml tajný klíč</span><span class="sxs-lookup"><span data-stu-id="eaceb-135">secret template - secret-template.yaml</span></span>
   - <span data-ttu-id="eaceb-136">Soubor DaemonSet YAML - omsagent – ds-secrets.yaml</span><span class="sxs-lookup"><span data-stu-id="eaceb-136">DaemonSet YAML file - omsagent-ds-secrets.yaml</span></span>
 - <span data-ttu-id="eaceb-137">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="eaceb-137">Run hello script.</span></span> <span data-ttu-id="eaceb-138">ID pracovního prostoru OMS a primární klíč pro hello vyzve Hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="eaceb-138">hello script will ask for hello OMS Workspace ID and Primary Key.</span></span> <span data-ttu-id="eaceb-139">Vložte, a hello skript vytvoří soubor tajný yaml, takže je možné spustit.</span><span class="sxs-lookup"><span data-stu-id="eaceb-139">Please insert that and hello script will create a secret yaml file so you can run it.</span></span>   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - <span data-ttu-id="eaceb-140">Vytvořte hello tajné klíče pod spuštěním hello následující:``` kubectl create -f omsagentsecret.yaml ```</span><span class="sxs-lookup"><span data-stu-id="eaceb-140">Create hello secrets pod by running hello following: ``` kubectl create -f omsagentsecret.yaml ```</span></span>
 
   - <span data-ttu-id="eaceb-141">toocheck spustit hello následující:</span><span class="sxs-lookup"><span data-stu-id="eaceb-141">toocheck, run hello following:</span></span> 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - <span data-ttu-id="eaceb-142">Vytvoření vaší omsagent démon set spuštěním``` kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="eaceb-142">Create your omsagent daemon-set by running ``` kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

### <a name="conclusion"></a><span data-ttu-id="eaceb-143">Závěr</span><span class="sxs-lookup"><span data-stu-id="eaceb-143">Conclusion</span></span>
<span data-ttu-id="eaceb-144">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="eaceb-144">That's it!</span></span> <span data-ttu-id="eaceb-145">Po několika minutách musí být schopný toosee dat odesílaných tooyour OMS řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="eaceb-145">After a few minutes, you should be able toosee data flowing tooyour OMS dashboard.</span></span>
