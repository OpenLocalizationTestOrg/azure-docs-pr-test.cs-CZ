---
title: "aaaLoad vyvážit Kubernetes kontejnerů v Azure | Microsoft Docs"
description: "Připojte externě a vyrovnávání zatížení v rámci několika kontejnerů v clusteru s podporou Kubernetes v Azure Container Service."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kubernetes kontejnery, Micro-services, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8073c8d3a015a53a532c326749571cb2582e1bac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a>Kontejnery Vyrovnávání zatížení v clusteru s podporou Kubernetes v Azure Container Service 
Tento článek představuje Vyrovnávání zatížení v clusteru s podporou Kubernetes v Azure Container Service. Vyrovnávání zatížení poskytuje IP adresu externě dostupný pro službu hello a distribuuje síťový provoz mezi hello pracovními stanicemi soustředěnými kolem spuštěna v agentovi virtuálních počítačů.

Můžete nastavit toouse služby Kubernetes [nástroj pro vyrovnávání zatížení Azure](../../load-balancer/load-balancer-overview.md) toomanage externí zatížení sítě (TCP). Další konfiguraci je možné, směrování provozu protokolu HTTP nebo HTTPS nebo pokročilejší scénáře a vyrovnávání zatížení.

## <a name="prerequisites"></a>Požadavky
* [Nasazení clusteru Kubernetes](container-service-kubernetes-walkthrough.md) v Azure Container Service
* [Připojení vašeho klienta](../container-service-connect.md) tooyour clusteru

## <a name="azure-load-balancer"></a>Nástroje pro vyrovnávání zatížení Azure

Ve výchozím nastavení clusteru s podporou Kubernetes nasazené v Azure Container Service zahrnuje pro vyrovnávání zatížení Azure internetového pro agenta hello virtuálních počítačů. (Prostředek pro vyrovnávání zatížení samostatné je nakonfigurován pro hlavní hello virtuálních počítačů). Nástroje pro vyrovnávání zatížení Azure je nástroj pro vyrovnávání zatížení vrstvy 4. Nástroj pro vyrovnávání zatížení hello v současné době podporuje pouze provoz protokolu TCP v Kubernetes.

Při vytváření služby Kubernetes, můžete nakonfigurovat automaticky hello Azure služby Vyrovnávání zatížení při tooallow přístup toohello. tooconfigure hello Vyrovnávání zatížení služby hello sadu `type` příliš`LoadBalancer`. Nástroj pro vyrovnávání zatížení Hello vytvoří pravidlo toomap veřejnou IP adresu a číslo portu příchozí provoz toohello privátní IP adresy služby a čísla portů hello pracovními stanicemi soustředěnými kolem v agentovi virtuální počítače (a naopak přenosům odpověď). 

 Následují dva příklady znázorňující, jak tooset hello Kubernetes služby `type` příliš`LoadBalancer`. (Po snažíme hello příklady odstraňte hello nasazení je již nepotřebujete.)

### <a name="example-use-hello-kubectl-expose-command"></a>Příklad: Použití hello `kubectl expose` příkaz 
Hello [Kubernetes návod](container-service-kubernetes-walkthrough.md) obsahuje příklad tooexpose služby s hello `kubectl expose` příkazu a jeho `--type=LoadBalancer` příznak. Zde jsou kroky hello:

1. Spuštění nového nasazení kontejneru. Například hello následující příkaz spustí názvem nového nasazení `mynginx`. Hello nasazení se skládá z tři kontejnery na základě bitové kopie Docker hello hello Nginx webovém serveru.

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. Ověřte, zda jsou spuštěny hello kontejnery. Například, pokud dotaz pro kontejnery hello s `kubectl get pods`, uvidíte výstup podobný toohello následující:

    ![Získat nové kontejnery Nginx](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. tooconfigure hello zatížení vyrovnávání tooaccept externích přenosů toohello nasazení, spusťte `kubectl expose` s `--type=LoadBalancer`. Hello následující příkaz zpřístupní hello Nginx server na portu 80:

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. Typ `kubectl get svc` toosee hello stavu služby hello v clusteru hello. Při vyrovnávání zatížení hello nakonfiguruje hello pravidlo, hello `EXTERNAL-IP` z hello služby se zobrazí jako `<pending>`. Po několika minutách je nakonfigurované hello externí IP adresu: 

    ![Konfigurace pro vyrovnávání zatížení Azure](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. Ověřte, že máte přístup hello služby hello externí IP adresu. Například otevřete webovou prohlížeče toohello IP adresu vidět. Hello prohlížeč zobrazí hello Nginx webový server spuštěný v jednom z kontejnerů hello. Nebo spusťte hello `curl` nebo `wget` příkaz. Například:

    ```
    curl 13.82.93.130
    ```

    Zobrazený výstup by měl vypadat přibližně takto:

    ![Nginx přístup pomocí curl](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. toosee hello konfigurace nástroje pro vyrovnávání zatížení Azure hello, přejděte toohello [portál Azure](https://portal.azure.com).

7. Vyhledejte hello skupiny prostředků pro váš cluster container service a zvolte hello Vyrovnávání zatížení pro agenta hello virtuálních počítačů. Stejné jako hello kontejneru služby by měl hello její název. (Není zvolte hello Vyrovnávání zatížení pro hello řídicí uzly, jeden jejíž název obsahuje hello **hlavní lb**.) 

    ![Nástroje pro vyrovnávání zatížení ve skupině prostředků](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. Klikněte na tlačítko Podrobnosti hello toosee hello konfigurace služby Vyrovnávání zatížení, **pravidla Vyrovnávání zatížení** a hello název hello pravidla, která byla nakonfigurována.

    ![Pravidla nástroje pro vyrovnávání zatížení](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-hello-service-configuration-file"></a>Příklad: Zadejte `type: LoadBalancer` v konfiguračním souboru služby hello

Pokud nasadíte aplikaci kontejneru Kubernetes z YAML nebo JSON [konfigurační soubor služby](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), zadejte externím vyrovnáváním zatížení přidáním hello následující specifikace service toohello řádku:

```YAML
 "type": "LoadBalancer"
``` 



Hello následující postup použijte hello Kubernetes [návštěv příklad](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook). V tomto příkladu je vícevrstvé webové aplikace založené na imagích Redis a PHP Docker. Můžete zadat v konfiguračním souboru služby hello že serveru PHP front-endu hello používá pro vyrovnávání zatížení Azure hello.

1. Stáhněte si soubor hello `guestbook-all-in-one.yaml` z [Githubu](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one). 
2. Vyhledejte hello `spec` pro hello `frontend` služby.
3. Zrušením komentáře u hello řádku `type: LoadBalancer`.

    ![Nástroje pro vyrovnávání zatížení v konfiguraci služby](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. Uložte soubor hello a nasazení aplikace hello spuštěním hello následující příkaz:

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. Typ `kubectl get svc` toosee hello stavu služby hello v clusteru hello. Při vyrovnávání zatížení hello nakonfiguruje hello pravidlo, hello `EXTERNAL-IP` z hello `frontend` služby se zobrazí jako `<pending>`. Po několika minutách je nakonfigurované hello externí IP adresu: 

    ![Konfigurace pro vyrovnávání zatížení Azure](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. Ověřte, že máte přístup hello služby hello externí IP adresu. Například můžete otevřít webový prohlížeč toohello externí adresu IP hello služby.

    ![Externě návštěv přístup](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    Můžete přidat návštěv položky.

7. toosee hello konfigurace nástroje pro vyrovnávání zatížení Azure hello, vyhledat hello prostředek pro vyrovnávání zatížení pro cluster hello v hello [portál Azure](https://portal.azure.com). Zjistit hello kroků v předchozím příkladu hello.

### <a name="considerations"></a>Požadavky

* Vytvoření pravidla pro vyrovnávání zatížení hello probíhá asynchronně, a informace o vyrovnávání hello zřízený je publikována ve službě hello `status.loadBalancer` pole.
* Všechny služby se automaticky přiřadí svůj vlastní virtuální IP adresu nástroji pro vyrovnávání zatížení hello.
* Pokud chcete nástroj pro vyrovnávání zatížení tooreach hello podle názvu DNS, spolupracovat s vaší domény služby zprostředkovatele toocreate název DNS pro pravidlo hello IP adresu.

## <a name="http-or-https-traffic"></a>Provoz protokolu HTTP nebo HTTPS

tooload zůstatek HTTP nebo HTTPS provoz toocontainer webové aplikace a spravovat certifikáty pro (TLS) transport layer security, můžete použít hello Kubernetes [příjem příchozích dat](https://kubernetes.io/docs/user-guide/ingress/) prostředků. Příjem příchozích dat je kolekce pravidel, které umožňují příchozí připojení tooreach hello Clusterovou službou. Prostředku toowork příjem příchozích dat, musí mít hello Kubernetes cluster [příjem příchozích dat řadiče](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) systémem.

Azure Container Service neimplementuje řadič Kubernetes příchozího automaticky. Několik implementace řadiče jsou k dispozici. V současné době hello [Nginx příchozího řadiče](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) se doporučuje tooconfigure příjem příchozích dat pravidel a vyrovnávání zatížení přenosů dat HTTP a HTTPS. 

Další informace najdete v tématu hello [dokumentace řadiče příjem příchozích dat Nginx](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).

> [!IMPORTANT]
> Při použití hello řadiče Nginx příjem příchozích dat v Azure Container Service, musí vystavit nasazení řadiče hello jako službu s `type: LoadBalancer`. Tím se nakonfiguruje hello zatížení Azure vyrovnávání tooroute provoz toohello řadiče. Další informace najdete v předchozí části hello.


## <a name="next-steps"></a>Další kroky

* V tématu hello [dokumentace Kubernetes nástroj pro vyrovnávání zatížení](https://kubernetes.io/docs/user-guide/load-balancer/)
* Další informace o [řadiče Kubernetes příchozí a příjem příchozích dat](https://kubernetes.io/docs/user-guide/ingress/)
* V tématu [Kubernetes příklady](https://github.com/kubernetes/kubernetes/tree/master/examples)

