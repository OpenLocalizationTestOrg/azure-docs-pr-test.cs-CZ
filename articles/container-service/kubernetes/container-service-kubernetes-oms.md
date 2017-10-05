---
title: "Cluster Azure Kubernetes monitorování - Operations Management | Microsoft Docs"
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
ms.openlocfilehash: bd5c81435c091d25bc14710589b7c043e9f56a25
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a><span data-ttu-id="4afc9-103">Monitorování clusteru Azure Container Service s Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="4afc9-103">Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4afc9-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4afc9-104">Prerequisites</span></span>
<span data-ttu-id="4afc9-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="4afc9-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="4afc9-106">Předpokládá také, abyste měli `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="4afc9-106">It also assumes that you have the `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="4afc9-107">Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="4afc9-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="4afc9-108">Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="4afc9-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>  
<span data-ttu-id="4afc9-109">Alternativně můžete použít [prostředí cloudu Azure](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), který má `az` rozhraní příkazového řádku Azure a `kubectl` nástroje pro je již nainstalována.</span><span class="sxs-lookup"><span data-stu-id="4afc9-109">Alternatively, you can use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), which has the `az` Azure cli and `kubectl` tools already installed for you.</span></span>  

<span data-ttu-id="4afc9-110">Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="4afc9-110">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="4afc9-111">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="4afc9-111">If you don't have `kubectl` installed, you can run:</span></span>
```console
$ az acs kubernetes install-cli
```

<span data-ttu-id="4afc9-112">K testování, pokud máte nainstalovaný ve vaší kubectl nástroj, který můžete spustit klíče kubernetes:</span><span class="sxs-lookup"><span data-stu-id="4afc9-112">To test if you have kubernetes keys installed in your kubectl tool you can run:</span></span>
```console
$ kubectl get nodes
```

<span data-ttu-id="4afc9-113">Pokud výše uvedené chyby příkazu si musíte nainstalovat kubernetes clusteru klíče do vaší kubectl nástroje.</span><span class="sxs-lookup"><span data-stu-id="4afc9-113">If the above command errors out, you need to install kubernetes cluster keys into your kubectl tool.</span></span> <span data-ttu-id="4afc9-114">Můžete to udělat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="4afc9-114">You can do that with the following command:</span></span>
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a><span data-ttu-id="4afc9-115">Monitorování kontejnery s služby Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="4afc9-115">Monitoring Containers with Operations Management Suite (OMS)</span></span>

<span data-ttu-id="4afc9-116">Microsoft Operations Management (OMS) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="4afc9-116">Microsoft Operations Management (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="4afc9-117">Kontejner řešení je řešení v OMS analýzy protokolů, který umožňuje zobrazit inventář kontejneru, výkonu a protokoly na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="4afc9-117">Container Solution is a solution in OMS Log Analytics, which helps you view the container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="4afc9-118">Můžete auditovat, řešení potíží s kontejnery zobrazením protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="4afc9-118">You can audit, troubleshoot containers by viewing the logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="4afc9-119">Další informace o kontejneru řešení, naleznete [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="4afc9-119">For more information about Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="installing-oms-on-kubernetes"></a><span data-ttu-id="4afc9-120">Instalace OMS na Kubernetes</span><span class="sxs-lookup"><span data-stu-id="4afc9-120">Installing OMS on Kubernetes</span></span>

### <a name="obtain-your-workspace-id-and-key"></a><span data-ttu-id="4afc9-121">Získání ID a klíč</span><span class="sxs-lookup"><span data-stu-id="4afc9-121">Obtain your workspace ID and key</span></span>
<span data-ttu-id="4afc9-122">Agent komunikovat službu pro OMS je potřeba nakonfigurovat s id pracovního prostoru a klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="4afc9-122">For the OMS agent to talk to the service it needs to be configured with a workspace id and a workspace key.</span></span> <span data-ttu-id="4afc9-123">Id pracovního prostoru a klíč, je potřeba vytvořit na účet OMS <https://mms.microsoft.com>. Postupujte podle kroků pro vytvoření účtu.</span><span class="sxs-lookup"><span data-stu-id="4afc9-123">To get the workspace id and key you need to create an OMS account at <https://mms.microsoft.com>. Please follow the steps to create an account.</span></span> <span data-ttu-id="4afc9-124">Po dokončení vytváření účtu, je nutné získat id a klíč kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux**, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="4afc9-124">Once you are done creating the account, you need to obtain your id and key by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a><span data-ttu-id="4afc9-125">Instalace agenta OMS pomocí DaemonSet</span><span class="sxs-lookup"><span data-stu-id="4afc9-125">Install the OMS agent using a DaemonSet</span></span>
<span data-ttu-id="4afc9-126">DaemonSets Kubernetes používá ke spuštění jednu instanci kontejner na jednotlivých hostitelích v clusteru.</span><span class="sxs-lookup"><span data-stu-id="4afc9-126">DaemonSets are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="4afc9-127">Jsou ideální pro spuštění monitorovací agenty.</span><span class="sxs-lookup"><span data-stu-id="4afc9-127">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="4afc9-128">Tady je [DaemonSet YAML soubor](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span><span class="sxs-lookup"><span data-stu-id="4afc9-128">Here is the [DaemonSet YAML file](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span></span> <span data-ttu-id="4afc9-129">Uložit do souboru s názvem `oms-daemonset.yaml` a nahraďte zástupný symbol hodnoty pro `WSID` a `KEY` s vaše id a klíč v souboru.</span><span class="sxs-lookup"><span data-stu-id="4afc9-129">Save it to a file named `oms-daemonset.yaml` and replace the place-holder values for `WSID` and `KEY` with your workspace id and key in the file.</span></span>

<span data-ttu-id="4afc9-130">Po přidání vaše ID a klíč v konfiguraci DaemonSet, můžete nainstalovat agenta OMS na cluster s `kubectl` nástroj pro příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="4afc9-130">Once you have added your workspace ID and key to the DaemonSet configuration, you can install the OMS agent on your cluster with the `kubectl` command line tool:</span></span>

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a><span data-ttu-id="4afc9-131">Instalace agenta OMS pomocí Kubernetes tajný klíč</span><span class="sxs-lookup"><span data-stu-id="4afc9-131">Installing the OMS agent using a Kubernetes Secret</span></span>
<span data-ttu-id="4afc9-132">K ochraně vašeho ID pracovního prostoru OMS a klíče, které můžete použít Kubernetes tajný klíč jako součást DaemonSet YAML souboru.</span><span class="sxs-lookup"><span data-stu-id="4afc9-132">To protect your OMS workspace ID and key you can use Kubernetes Secret as a part of DaemonSet YAML file.</span></span>

 - <span data-ttu-id="4afc9-133">Zkopírujte skript, soubor tajný šablonu a soubor DaemonSet YAML (z [úložiště](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) a ujistěte se, že jsou na stejném adresáři.</span><span class="sxs-lookup"><span data-stu-id="4afc9-133">Copy the script, secret template file and the DaemonSet YAML file (from [repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) and make sure they are on the same directory.</span></span> 
      - <span data-ttu-id="4afc9-134">Generování skriptu - tajný klíč gen.sh tajný klíč</span><span class="sxs-lookup"><span data-stu-id="4afc9-134">secret generating script - secret-gen.sh</span></span>
      - <span data-ttu-id="4afc9-135">Šablona tajné – template.yaml tajný klíč</span><span class="sxs-lookup"><span data-stu-id="4afc9-135">secret template - secret-template.yaml</span></span>
   - <span data-ttu-id="4afc9-136">Soubor DaemonSet YAML - omsagent – ds-secrets.yaml</span><span class="sxs-lookup"><span data-stu-id="4afc9-136">DaemonSet YAML file - omsagent-ds-secrets.yaml</span></span>
 - <span data-ttu-id="4afc9-137">Spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="4afc9-137">Run the script.</span></span> <span data-ttu-id="4afc9-138">Skript vyzve pro ID pracovního prostoru OMS a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="4afc9-138">The script will ask for the OMS Workspace ID and Primary Key.</span></span> <span data-ttu-id="4afc9-139">Vložte který. skript se vytvoří soubor tajný yaml, můžete ji spustit.</span><span class="sxs-lookup"><span data-stu-id="4afc9-139">Please insert that and the script will create a secret yaml file so you can run it.</span></span>   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - <span data-ttu-id="4afc9-140">Vytvoření tajných klíčů pod spuštěním následujícího:``` kubectl create -f omsagentsecret.yaml ```</span><span class="sxs-lookup"><span data-stu-id="4afc9-140">Create the secrets pod by running the following: ``` kubectl create -f omsagentsecret.yaml ```</span></span>
 
   - <span data-ttu-id="4afc9-141">Pokud chcete zkontrolovat, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="4afc9-141">To check, run the following:</span></span> 

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
 
  - <span data-ttu-id="4afc9-142">Vytvoření vaší omsagent démon set spuštěním``` kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="4afc9-142">Create your omsagent daemon-set by running ``` kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

### <a name="conclusion"></a><span data-ttu-id="4afc9-143">Závěr</span><span class="sxs-lookup"><span data-stu-id="4afc9-143">Conclusion</span></span>
<span data-ttu-id="4afc9-144">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="4afc9-144">That's it!</span></span> <span data-ttu-id="4afc9-145">Po několika minutách byste měli vidět dat odesílaných do řídicího panelu OMS.</span><span class="sxs-lookup"><span data-stu-id="4afc9-145">After a few minutes, you should be able to see data flowing to your OMS dashboard.</span></span>
