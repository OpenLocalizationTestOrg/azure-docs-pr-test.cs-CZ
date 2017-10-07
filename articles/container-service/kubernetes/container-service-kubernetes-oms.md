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
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>Monitorování clusteru Azure Container Service s Operations Management Suite (OMS)

## <a name="prerequisites"></a>Požadavky
Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).

Také předpokládá, že máte hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.

Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:

```console
$ az --version
```

Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).  
Alternativně můžete použít [prostředí cloudu Azure](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), který má hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje pro je již nainstalována.  

Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:

```console
$ kubectl version
```

Pokud nemáte `kubectl` nainstalován, můžete spustit:
```console
$ az acs kubernetes install-cli
```

Pokud máte nainstalovaný ve vašem kubectl nástroj klíče kubernetes tootest můžete spustit:
```console
$ kubectl get nodes
```

Pokud hello výše příkaz chyby out, musíte do vaší kubectl nástroje tooinstall kubernetes clusteru klíče. Můžete to udělat pomocí hello následující příkaz:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Monitorování kontejnery s služby Operations Management Suite (OMS)

Microsoft Operations Management (OMS) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury. Kontejner řešení je řešení v OMS analýzy protokolů, které vám umožní zobrazit hello kontejneru inventář, výkonu a protokoly na jednom místě. Můžete auditovat, řešení potíží s kontejnery zobrazením hello protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.

![](media/container-service-monitoring-oms/image1.png)

Další informace o řešení kontejneru naleznete toothe [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>Instalace OMS na Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Získání ID a klíč
Pro hello služba toohello tootalk agenta OMS musí toobe nakonfigurovat id pracovního prostoru a klíč pracovního prostoru. id pracovního prostoru hello tooget a klíč musíte toocreate OMS účtu v <https://mms.microsoft.com>. Postupujte podle kroků toocreate hello účet. Po dokončení vytváření hello účet, bude třeba tooobtain id a klíč kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux**, jak je uvedeno níže.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-hello-oms-agent-using-a-daemonset"></a>Instalace agenta OMS hello pomocí DaemonSet
DaemonSets používají Kubernetes toorun jednu instanci kontejner na jednotlivých hostitelích v clusteru hello.
Jsou ideální pro spuštění monitorovací agenty.

Tady je hello [DaemonSet YAML soubor](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Uložte ho tooa soubor s názvem `oms-daemonset.yaml` a nahraďte zástupný symbol hodnoty hello `WSID` a `KEY` s vaše id a klíč v souboru hello.

Po přidání vaše ID a klíč toohello DaemonSet konfigurace, můžete nainstalovat agenta OMS hello v clusteru s hello `kubectl` nástroj pro příkazový řádek:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-hello-oms-agent-using-a-kubernetes-secret"></a>Instalace agenta OMS hello pomocí Kubernetes tajný klíč
tooprotect vaše OMS ID a klíč Kubernetes tajný klíč slouží jako součást DaemonSet YAML souboru.

 - Zkopírujte skript hello, soubor tajný šablony a hello DaemonSet YAML souboru (z [úložiště](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) a ujistěte se, že jsou na hello stejný adresář. 
      - Generování skriptu - tajný klíč gen.sh tajný klíč
      - Šablona tajné – template.yaml tajný klíč
   - Soubor DaemonSet YAML - omsagent – ds-secrets.yaml
 - Spusťte skript hello. ID pracovního prostoru OMS a primární klíč pro hello vyzve Hello skriptu. Vložte, a hello skript vytvoří soubor tajný yaml, takže je možné spustit.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Vytvořte hello tajné klíče pod spuštěním hello následující:``` kubectl create -f omsagentsecret.yaml ```
 
   - toocheck spustit hello následující: 

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
 
  - Vytvoření vaší omsagent démon set spuštěním``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Závěr
A to je vše! Po několika minutách musí být schopný toosee dat odesílaných tooyour OMS řídicího panelu.
