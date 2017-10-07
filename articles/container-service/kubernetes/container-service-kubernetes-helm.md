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
# <a name="use-helm-toodeploy-containers-on-a-kubernetes-cluster"></a>Použití kontejnerů toodeploy Helm na clusteru s podporou Kubernetes 

[Helm](https://github.com/kubernetes/helm/) je nástroj balení open source, který vám pomůže nainstalovat a spravovat životní cyklus hello Kubernetes aplikací. Podobně jako správce balíčku tooLinux například byt č get a Yum Helm je použité toomanage Kubernetes grafy, které jsou balíčky předkonfigurované Kubernetes prostředků. Tento článek ukazuje, jak se toowork s Helm na clusteru s podporou Kubernetes nasadit v Azure Container Service.

Helm má dvě součásti: 
* Hello **Helm CLI** je klient, který běží na vašem počítači místně nebo v cloudu hello  

* **Výšce kormidelní páky** je server, který běží na clusteru Kubernetes hello a spravuje životní cyklus aplikace Kubernetes hello 
 
## <a name="prerequisites"></a>Požadavky

* [Vytvoření clusteru s podporou Kubernetes](container-service-kubernetes-walkthrough.md) v Azure Container Service

* [Instalace a konfigurace `kubectl` ](../container-service-connect.md) v místním počítači.

* [Nainstalujte Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) v místním počítači.

## <a name="helm-basics"></a>Základy Helm 

tooview informace o hello Kubernetes clusteru, že instalujete výšce kormidelní páky a nasazení aplikace na, zadejte následující příkaz hello:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Po instalaci Helm nainstalujte výšce kormidelní páky v clusteru Kubernetes zadáním hello následující příkaz:

```bash
helm init --upgrade
```
Po úspěšném dokončení, zobrazí se výstup jako hello následující:

![Instalace výšce kormidelní páky](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
tooview všechny hello Helm grafy k dispozici v úložišti hello, hello zadejte následující příkaz:

```bash 
helm search 
```

Zobrazí výstup jako hello následující:

![Helm vyhledávání](./media/container-service-kubernetes-helm/helm-search.png)
 
tooupdate hello grafy tooget hello nejnovější verze, zadejte:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Nasazení grafu řadič aplikace Nginx příjem příchozích dat 
 
toodeploy Nginx příjem příchozích dat řadiče grafu, zadejte jeden příkaz:

```bash
helm install stable/nginx-ingress 
```
![Nasazení řadiče příjem příchozích dat](./media/container-service-kubernetes-helm/nginx-ingress.png)

Pokud zadáte `kubectl get svc` tooview všechny služby, které jsou spuštěné v clusteru hello, uvidíte, IP adresa je přiřazen toohello příjem příchozích dat řadiče. (Když probíhá hello přiřazení, uvidíte `<pending>`. Trvá několik minut toocomplete.) 

Po přiřazení hello IP adresu, přejděte toohello hodnotu hello externí IP adresu toosee hello Nginx back-end systémem. 
 
![Příjem příchozích dat IP adres](./media/container-service-kubernetes-helm/ingress-ip-address.png)


toosee seznam grafy nainstalován v clusteru, zadejte:

```bash
helm list 
```

Příkaz hello můžete zkrátit příliš`helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Nasazení klienta a graf MariaDB

Teď nasaďte MariaDB graf a MariaDB klienta tooconnect toohello databáze.

toodeploy hello MariaDB grafu, hello zadejte následující příkaz:

```bash
helm install --name v1 stable/mariadb
```

kde `--name` je značku používá pro verze.

> [!TIP]
> Pokud hello nasazení nezdaří, spusťte `helm repo update` a zkuste to znovu.
>
 
 
tooview všechny grafy hello nasadit v clusteru, zadejte:

```bash 
helm list
```
 
tooview všechna nasazení, které běží v clusteru, zadejte:

```bash
kubectl get deployments 
``` 
 
 
Nakonec toorun pod tooaccess hello klienta, zadejte:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
tooconnect toohello klienta hello zadejte následující příkaz, nahraďte `v1-mariadb` s názvem hello nasazení:

```bash
sudo mysql –h v1-mariadb
```
 
 
Teď můžete použít standardní databáze toocreate příkazy SQL, tabulek atd. Například `Create DATABASE testdb1;` vytvoří prázdnou databázi. 
 
 
 
## <a name="next-steps"></a>Další kroky

* Další informace o správě Kubernetes grafy, najdete v části hello [Helm dokumentaci](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

