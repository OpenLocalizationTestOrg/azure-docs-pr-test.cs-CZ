---
title: "Nasazení kontejnerů s Helm v Azure Kubernetes | Microsoft Docs"
description: "Pomocí nástroje balení Helm nasazení kontejnerů v clusteru s podporou Kubernetes v Azure Container Service"
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
ms.openlocfilehash: 3cfcc5abbee03ca8fbbec4e4eae711e7c2d9deae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a><span data-ttu-id="10449-103">Použít k nasazení kontejnerů v clusteru s podporou Kubernetes Helm</span><span class="sxs-lookup"><span data-stu-id="10449-103">Use Helm to deploy containers on a Kubernetes cluster</span></span> 

<span data-ttu-id="10449-104">[Helm](https://github.com/kubernetes/helm/) je nástroj balení open source, který vám pomůže nainstalovat a spravovat životní cyklus aplikace Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="10449-104">[Helm](https://github.com/kubernetes/helm/) is an open-source packaging tool that helps you install and manage the lifecycle of Kubernetes applications.</span></span> <span data-ttu-id="10449-105">Podobně jako u správce balíčku Linux například byt č get a Yum, Helm slouží ke správě Kubernetes grafy, které jsou balíčky předkonfigurované Kubernetes prostředků.</span><span class="sxs-lookup"><span data-stu-id="10449-105">Similar to Linux package managers such as Apt-get and Yum, Helm is used to manage Kubernetes charts, which are packages of preconfigured Kubernetes resources.</span></span> <span data-ttu-id="10449-106">Tento článek ukazuje, jak pracovat s Helm na clusteru s podporou Kubernetes nasazené v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="10449-106">This article shows you how to work with Helm on a Kubernetes cluster deployed in Azure Container Service.</span></span>

<span data-ttu-id="10449-107">Helm má dvě součásti:</span><span class="sxs-lookup"><span data-stu-id="10449-107">Helm has two components:</span></span> 
* <span data-ttu-id="10449-108">**Helm CLI** je klient, který běží na vašem počítači místně nebo v cloudu</span><span class="sxs-lookup"><span data-stu-id="10449-108">The **Helm CLI** is a client that runs on your machine locally or in the cloud</span></span>  

* <span data-ttu-id="10449-109">**Výšce kormidelní páky** je server, který běží na clusteru Kubernetes a spravuje životní cyklus aplikace Kubernetes</span><span class="sxs-lookup"><span data-stu-id="10449-109">**Tiller** is a server that runs on the Kubernetes cluster and manages the lifecycle of your Kubernetes applications</span></span> 
 
## <a name="prerequisites"></a><span data-ttu-id="10449-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10449-110">Prerequisites</span></span>

* <span data-ttu-id="10449-111">[Vytvoření clusteru s podporou Kubernetes](container-service-kubernetes-walkthrough.md) v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="10449-111">[Create a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>

* <span data-ttu-id="10449-112">[Instalace a konfigurace `kubectl` ](../container-service-connect.md) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="10449-112">[Install and configure `kubectl`](../container-service-connect.md) on a local computer</span></span>

* <span data-ttu-id="10449-113">[Nainstalujte Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="10449-113">[Install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on a local computer</span></span>

## <a name="helm-basics"></a><span data-ttu-id="10449-114">Základy Helm</span><span class="sxs-lookup"><span data-stu-id="10449-114">Helm basics</span></span> 

<span data-ttu-id="10449-115">Chcete-li zobrazit informace o clusteru Kubernetes, že instalujete výšce kormidelní páky a nasazení vašich aplikací, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10449-115">To view information about the Kubernetes cluster that you are installing Tiller and deploying your applications to, type the following command:</span></span>

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
<span data-ttu-id="10449-117">Po instalaci Helm nainstalujte výšce kormidelní páky Kubernetes cluster tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10449-117">After you have installed Helm, install Tiller on your Kubernetes cluster by typing the following command:</span></span>

```bash
helm init --upgrade
```
<span data-ttu-id="10449-118">Po úspěšném dokončení, zobrazí se výstup takto:</span><span class="sxs-lookup"><span data-stu-id="10449-118">When it completes successfully, you see output like the following:</span></span>

![Instalace výšce kormidelní páky](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
<span data-ttu-id="10449-120">Chcete-li zobrazit dostupné Helm grafy v úložišti, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10449-120">To view all the Helm charts available in the repository, type the following command:</span></span>

```bash 
helm search 
```

<span data-ttu-id="10449-121">Zobrazí výstup takto:</span><span class="sxs-lookup"><span data-stu-id="10449-121">You see output like the following:</span></span>

![Helm vyhledávání](./media/container-service-kubernetes-helm/helm-search.png)
 
<span data-ttu-id="10449-123">Chcete-li aktualizovat grafy získat nejnovější verze, zadejte:</span><span class="sxs-lookup"><span data-stu-id="10449-123">To update the charts to get the latest versions, type:</span></span>

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a><span data-ttu-id="10449-124">Nasazení grafu řadič aplikace Nginx příjem příchozích dat</span><span class="sxs-lookup"><span data-stu-id="10449-124">Deploy an Nginx ingress controller chart</span></span> 
 
<span data-ttu-id="10449-125">Chcete-li nasadit grafu řadič aplikace Nginx příjem příchozích dat, zadejte jeden příkaz:</span><span class="sxs-lookup"><span data-stu-id="10449-125">To deploy an Nginx ingress controller chart, type a single command:</span></span>

```bash
helm install stable/nginx-ingress 
```
![Nasazení řadiče příjem příchozích dat](./media/container-service-kubernetes-helm/nginx-ingress.png)

<span data-ttu-id="10449-127">Pokud zadáte `kubectl get svc` Pokud chcete zobrazit všechny služby, které běží na clusteru, uvidíte, jestli řadič příjem příchozích dat je přiřazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="10449-127">If you type `kubectl get svc` to view all services that are running on the cluster, you see that an IP address is assigned to the ingress controller.</span></span> <span data-ttu-id="10449-128">(Když probíhá přiřazení, uvidíte `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="10449-128">(While the assignment is in progress, you see `<pending>`.</span></span> <span data-ttu-id="10449-129">Trvá několik minut na dokončení.)</span><span class="sxs-lookup"><span data-stu-id="10449-129">It takes a couple of minutes to complete.)</span></span> 

<span data-ttu-id="10449-130">Po IP je přiřazená adresa, přejděte na hodnotu externí IP adresu, kterou najdete v části s back-end Nginx.</span><span class="sxs-lookup"><span data-stu-id="10449-130">After the IP address is assigned, navigate to the value of the external IP address to see the Nginx backend running.</span></span> 
 
![Příjem příchozích dat IP adres](./media/container-service-kubernetes-helm/ingress-ip-address.png)


<span data-ttu-id="10449-132">Chcete-li zobrazit seznam grafy nainstalován v clusteru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="10449-132">To see a list of charts installed on your cluster, type:</span></span>

```bash
helm list 
```

<span data-ttu-id="10449-133">Příkaz k můžete zkrátit `helm ls`.</span><span class="sxs-lookup"><span data-stu-id="10449-133">You can abbreviate the command to `helm ls`.</span></span>
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a><span data-ttu-id="10449-134">Nasazení klienta a graf MariaDB</span><span class="sxs-lookup"><span data-stu-id="10449-134">Deploy a MariaDB chart and client</span></span>

<span data-ttu-id="10449-135">Teď nasaďte MariaDB graf a MariaDB klienta se můžete připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="10449-135">Now deploy a MariaDB chart and a MariaDB client to connect to the database.</span></span>

<span data-ttu-id="10449-136">Pokud chcete nasadit MariaDB grafu, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10449-136">To deploy the MariaDB chart, type the following command:</span></span>

```bash
helm install --name v1 stable/mariadb
```

<span data-ttu-id="10449-137">kde `--name` je značku používá pro verze.</span><span class="sxs-lookup"><span data-stu-id="10449-137">where `--name` is a tag used for releases.</span></span>

> [!TIP]
> <span data-ttu-id="10449-138">Pokud se nasazení nezdaří, spusťte `helm repo update` a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="10449-138">If the deployment fails, run `helm repo update` and try again.</span></span>
>
 
 
<span data-ttu-id="10449-139">Chcete-li zobrazit všechny grafy nasazené v clusteru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="10449-139">To view all the charts deployed on your cluster, type:</span></span>

```bash 
helm list
```
 
<span data-ttu-id="10449-140">Pokud chcete zobrazit všechna nasazení, které jsou spuštěné v clusteru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="10449-140">To view all deployments running on your cluster, type:</span></span>

```bash
kubectl get deployments 
``` 
 
 
<span data-ttu-id="10449-141">Nakonec spustit pod pro přístup k klienta, zadejte:</span><span class="sxs-lookup"><span data-stu-id="10449-141">Finally, to run a pod to access the client, type:</span></span>

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
<span data-ttu-id="10449-142">Pokud chcete připojit ke klientovi, zadejte následující příkaz, nahraďte `v1-mariadb` s názvem vašeho nasazení:</span><span class="sxs-lookup"><span data-stu-id="10449-142">To connect to the client, type the following command, replacing `v1-mariadb` with the name of your deployment:</span></span>

```bash
sudo mysql –h v1-mariadb
```
 
 
<span data-ttu-id="10449-143">Nyní můžete standardní příkazy SQL k vytvoření databáze, tabulek atd. Například `Create DATABASE testdb1;` vytvoří prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="10449-143">You can now use standard SQL commands to create databases, tables, etc. For example, `Create DATABASE testdb1;` creates an empty database.</span></span> 
 
 
 
## <a name="next-steps"></a><span data-ttu-id="10449-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="10449-144">Next steps</span></span>

* <span data-ttu-id="10449-145">Další informace o správě Kubernetes grafy, najdete v článku [Helm dokumentaci](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span><span class="sxs-lookup"><span data-stu-id="10449-145">For more information about managing Kubernetes charts, see the [Helm documentation](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span></span> 

