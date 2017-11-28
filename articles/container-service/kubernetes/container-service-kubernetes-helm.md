---
title: kontejnery aaaDeploy s Helm v Azure Kubernetes | Microsoft Docs
description: "Použití hello Helm balení nástroj toodeploy kontejnerů na cluster Kubernetes v Azure Container Service"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: c7bd780afe00084ebe4e3a14873e1e340a29d144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-helm-toodeploy-containers-on-a-kubernetes-cluster"></a><span data-ttu-id="cc96b-103">Použití kontejnerů toodeploy Helm na clusteru s podporou Kubernetes</span><span class="sxs-lookup"><span data-stu-id="cc96b-103">Use Helm toodeploy containers on a Kubernetes cluster</span></span> 

<span data-ttu-id="cc96b-104">[Helm](https://github.com/kubernetes/helm/) je nástroj balení open source, který vám pomůže nainstalovat a spravovat životní cyklus hello Kubernetes aplikací.</span><span class="sxs-lookup"><span data-stu-id="cc96b-104">[Helm](https://github.com/kubernetes/helm/) is an open-source packaging tool that helps you install and manage hello lifecycle of Kubernetes applications.</span></span> <span data-ttu-id="cc96b-105">Podobně jako správce balíčku tooLinux například byt č get a Yum Helm je použité toomanage Kubernetes grafy, které jsou balíčky předkonfigurované Kubernetes prostředků.</span><span class="sxs-lookup"><span data-stu-id="cc96b-105">Similar tooLinux package managers such as Apt-get and Yum, Helm is used toomanage Kubernetes charts, which are packages of preconfigured Kubernetes resources.</span></span> <span data-ttu-id="cc96b-106">Tento článek ukazuje, jak se toowork s Helm na clusteru s podporou Kubernetes nasadit v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="cc96b-106">This article shows you how toowork with Helm on a Kubernetes cluster deployed in Azure Container Service.</span></span>

<span data-ttu-id="cc96b-107">Helm má dvě součásti:</span><span class="sxs-lookup"><span data-stu-id="cc96b-107">Helm has two components:</span></span> 
* <span data-ttu-id="cc96b-108">Hello **Helm CLI** je klient, který běží na vašem počítači místně nebo v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="cc96b-108">hello **Helm CLI** is a client that runs on your machine locally or in hello cloud</span></span>  

* <span data-ttu-id="cc96b-109">**Výšce kormidelní páky** je server, který běží na clusteru Kubernetes hello a spravuje životní cyklus aplikace Kubernetes hello</span><span class="sxs-lookup"><span data-stu-id="cc96b-109">**Tiller** is a server that runs on hello Kubernetes cluster and manages hello lifecycle of your Kubernetes applications</span></span> 
 
## <a name="prerequisites"></a><span data-ttu-id="cc96b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc96b-110">Prerequisites</span></span>

* <span data-ttu-id="cc96b-111">[Vytvoření clusteru s podporou Kubernetes](container-service-kubernetes-walkthrough.md) v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="cc96b-111">[Create a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>

* <span data-ttu-id="cc96b-112">[Instalace a konfigurace `kubectl` ](../container-service-connect.md) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="cc96b-112">[Install and configure `kubectl`](../container-service-connect.md) on a local computer</span></span>

* <span data-ttu-id="cc96b-113">[Nainstalujte Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="cc96b-113">[Install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on a local computer</span></span>

## <a name="helm-basics"></a><span data-ttu-id="cc96b-114">Základy Helm</span><span class="sxs-lookup"><span data-stu-id="cc96b-114">Helm basics</span></span> 

<span data-ttu-id="cc96b-115">tooview informace o hello Kubernetes clusteru, že instalujete výšce kormidelní páky a nasazení aplikace na, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="cc96b-115">tooview information about hello Kubernetes cluster that you are installing Tiller and deploying your applications to, type hello following command:</span></span>

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
<span data-ttu-id="cc96b-117">Po instalaci Helm nainstalujte výšce kormidelní páky v clusteru Kubernetes zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc96b-117">After you have installed Helm, install Tiller on your Kubernetes cluster by typing hello following command:</span></span>

```bash
helm init --upgrade
```
<span data-ttu-id="cc96b-118">Po úspěšném dokončení, zobrazí se výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="cc96b-118">When it completes successfully, you see output like hello following:</span></span>

![Instalace výšce kormidelní páky](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
<span data-ttu-id="cc96b-120">tooview všechny hello Helm grafy k dispozici v úložišti hello, hello zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc96b-120">tooview all hello Helm charts available in hello repository, type hello following command:</span></span>

```bash 
helm search 
```

<span data-ttu-id="cc96b-121">Zobrazí výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="cc96b-121">You see output like hello following:</span></span>

![Helm vyhledávání](./media/container-service-kubernetes-helm/helm-search.png)
 
<span data-ttu-id="cc96b-123">tooupdate hello grafy tooget hello nejnovější verze, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cc96b-123">tooupdate hello charts tooget hello latest versions, type:</span></span>

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a><span data-ttu-id="cc96b-124">Nasazení grafu řadič aplikace Nginx příjem příchozích dat</span><span class="sxs-lookup"><span data-stu-id="cc96b-124">Deploy an Nginx ingress controller chart</span></span> 
 
<span data-ttu-id="cc96b-125">toodeploy Nginx příjem příchozích dat řadiče grafu, zadejte jeden příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc96b-125">toodeploy an Nginx ingress controller chart, type a single command:</span></span>

```bash
helm install stable/nginx-ingress 
```
![Nasazení řadiče příjem příchozích dat](./media/container-service-kubernetes-helm/nginx-ingress.png)

<span data-ttu-id="cc96b-127">Pokud zadáte `kubectl get svc` tooview všechny služby, které jsou spuštěné v clusteru hello, uvidíte, IP adresa je přiřazen toohello příjem příchozích dat řadiče.</span><span class="sxs-lookup"><span data-stu-id="cc96b-127">If you type `kubectl get svc` tooview all services that are running on hello cluster, you see that an IP address is assigned toohello ingress controller.</span></span> <span data-ttu-id="cc96b-128">(Když probíhá hello přiřazení, uvidíte `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="cc96b-128">(While hello assignment is in progress, you see `<pending>`.</span></span> <span data-ttu-id="cc96b-129">Trvá několik minut toocomplete.)</span><span class="sxs-lookup"><span data-stu-id="cc96b-129">It takes a couple of minutes toocomplete.)</span></span> 

<span data-ttu-id="cc96b-130">Po přiřazení hello IP adresu, přejděte toohello hodnotu hello externí IP adresu toosee hello Nginx back-end systémem.</span><span class="sxs-lookup"><span data-stu-id="cc96b-130">After hello IP address is assigned, navigate toohello value of hello external IP address toosee hello Nginx backend running.</span></span> 
 
![Příjem příchozích dat IP adres](./media/container-service-kubernetes-helm/ingress-ip-address.png)


<span data-ttu-id="cc96b-132">toosee seznam grafy nainstalován v clusteru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cc96b-132">toosee a list of charts installed on your cluster, type:</span></span>

```bash
helm list 
```

<span data-ttu-id="cc96b-133">Příkaz hello můžete zkrátit příliš`helm ls`.</span><span class="sxs-lookup"><span data-stu-id="cc96b-133">You can abbreviate hello command too`helm ls`.</span></span>
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a><span data-ttu-id="cc96b-134">Nasazení klienta a graf MariaDB</span><span class="sxs-lookup"><span data-stu-id="cc96b-134">Deploy a MariaDB chart and client</span></span>

<span data-ttu-id="cc96b-135">Teď nasaďte MariaDB graf a MariaDB klienta tooconnect toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="cc96b-135">Now deploy a MariaDB chart and a MariaDB client tooconnect toohello database.</span></span>

<span data-ttu-id="cc96b-136">toodeploy hello MariaDB grafu, hello zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc96b-136">toodeploy hello MariaDB chart, type hello following command:</span></span>

```bash
helm install --name v1 stable/mariadb
```

<span data-ttu-id="cc96b-137">kde `--name` je značku používá pro verze.</span><span class="sxs-lookup"><span data-stu-id="cc96b-137">where `--name` is a tag used for releases.</span></span>

> [!TIP]
> <span data-ttu-id="cc96b-138">Pokud hello nasazení nezdaří, spusťte `helm repo update` a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="cc96b-138">If hello deployment fails, run `helm repo update` and try again.</span></span>
>
 
 
<span data-ttu-id="cc96b-139">tooview všechny grafy hello nasadit v clusteru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cc96b-139">tooview all hello charts deployed on your cluster, type:</span></span>

```bash 
helm list
```
 
<span data-ttu-id="cc96b-140">tooview všechna nasazení, které běží v clusteru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cc96b-140">tooview all deployments running on your cluster, type:</span></span>

```bash
kubectl get deployments 
``` 
 
 
<span data-ttu-id="cc96b-141">Nakonec toorun pod tooaccess hello klienta, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cc96b-141">Finally, toorun a pod tooaccess hello client, type:</span></span>

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
<span data-ttu-id="cc96b-142">tooconnect toohello klienta hello zadejte následující příkaz, nahraďte `v1-mariadb` s názvem hello nasazení:</span><span class="sxs-lookup"><span data-stu-id="cc96b-142">tooconnect toohello client, type hello following command, replacing `v1-mariadb` with hello name of your deployment:</span></span>

```bash
sudo mysql –h v1-mariadb
```
 
 
<span data-ttu-id="cc96b-143">Teď můžete použít standardní databáze toocreate příkazy SQL, tabulek atd. Například `Create DATABASE testdb1;` vytvoří prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="cc96b-143">You can now use standard SQL commands toocreate databases, tables, etc. For example, `Create DATABASE testdb1;` creates an empty database.</span></span> 
 
 
 
## <a name="next-steps"></a><span data-ttu-id="cc96b-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc96b-144">Next steps</span></span>

* <span data-ttu-id="cc96b-145">Další informace o správě Kubernetes grafy, najdete v části hello [Helm dokumentaci](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span><span class="sxs-lookup"><span data-stu-id="cc96b-145">For more information about managing Kubernetes charts, see hello [Helm documentation](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span></span> 

