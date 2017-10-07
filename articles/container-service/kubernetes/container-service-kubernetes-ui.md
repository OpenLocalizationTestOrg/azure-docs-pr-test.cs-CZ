---
title: "aaaManage Azure Kubernetes cluster s webového uživatelského rozhraní | Microsoft Docs"
description: "Pomocí hello Kubernetes webové uživatelské rozhraní v Azure Container Service"
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
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e24ea0b82c94d2fd4610e4442699ef756590e6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-kubernetes-web-ui-with-azure-container-service"></a>Pomocí hello Kubernetes webové uživatelské rozhraní s Azure Container Service

## <a name="prerequisites"></a>Požadavky
Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).


Také předpokládá, že máte hello Azure CLI 2.0 a `kubectl` nástroje nainstalované.

Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:

```console
$ az --version
```

Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).

Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:

```console
$ kubectl version
```

Pokud nemáte `kubectl` nainstalován, můžete spustit:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Přehled

### <a name="connect-toohello-web-ui"></a>Připojit toohello webového uživatelského rozhraní
Spuštěním můžete spustit hello Kubernetes webového uživatelského rozhraní:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

To by měla otevřít webový prohlížeč nakonfigurován tootalk tooa zabezpečené proxy server připojení místního počítače toohello Kubernetes webového uživatelského rozhraní.

### <a name="create-and-expose-a-service"></a>Vytvoření a vystavení služby
1. V hello Kubernetes webového uživatelského rozhraní, klikněte na **vytvořit** tlačítko v pravém horním hello.

    ![Kubernetes vytvoření uživatelského rozhraní](./media/container-service-kubernetes-ui/create.png)

    Otevře se dialogové okno kde můžete začít vytvářet aplikace.

2. Pojmenujte ji hello `hello-nginx`. Použití hello [ `nginx` kontejneru z Docker](https://hub.docker.com/_/nginx/) a nasadit tři repliky této webové služby.

    ![Dialogové okno Vytvořit Kubernetes Pod](./media/container-service-kubernetes-ui/nginx.png)

3. V části **služby**, vyberte **externí** a zadejte port 80.

    Toto nastavení zatížení zůstatky provoz toohello tři repliky.

    ![Dialogové okno Vytvořit Kubernetes služby](./media/container-service-kubernetes-ui/service.png)

4. Klikněte na tlačítko **nasadit** toodeploy tyto kontejnery a služby.

    ![Kubernetes nasazení](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Zobrazit vaše kontejnery
Po kliknutí na tlačítko **nasadit**, hello uživatelského rozhraní ukazuje zobrazení vaší služby jako nasazení:

![Stav Kubernetes](./media/container-service-kubernetes-ui/status.png)

Hello stav každého objektu Kubernetes v kruhu hello na levé straně hello uživatelského rozhraní, můžete zobrazit v části **pracovními stanicemi soustředěnými kolem**. Pokud je částečně úplné kruh, objekt hello stále nasazení. Po nasazení objekt zobrazí zelená značka zaškrtnutí:

![Kubernetes nasazení](./media/container-service-kubernetes-ui/deployed.png)

Jakmile všechno běží, klikněte na jednu z vaší pracovními stanicemi soustředěnými kolem toosee podrobnosti o hello spuštění webové služby.

![Kubernetes pracovními stanicemi soustředěnými kolem](./media/container-service-kubernetes-ui/pods.png)

V hello **pracovními stanicemi soustředěnými kolem** zobrazení, se zobrazí informace o hello kontejnery v hello pod, jakož i prostředků procesoru a paměti hello používá těchto kontejnerů:

![Kubernetes prostředky](./media/container-service-kubernetes-ui/resources.png)

Pokud nevidíte hello prostředky, může pro hello monitorování toopropagate dat potřebovat toowait několik minut.

toosee hello protokoly pro kontejner, klikněte na tlačítko **zobrazit protokoly**.

![Protokoly Kubernetes](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Zobrazení služby
V přidání toorunning kontejnerů, hello uživatelského rozhraní Kubernetes vytvořil externí `Service` která zřizuje zatížení vyrovnávání toobring provoz toohello kontejnery v clusteru.

V levém navigačním podokně hello, klikněte na **služby** tooview všechny služby (měla by existovat pouze jedna).

![Kubernetes služby](./media/container-service-kubernetes-ui/service-deployed.png)

V tomto zobrazení měli byste vidět externí koncový bod (IP adresa) přiřazené tooyour služby.
Pokud kliknete na tuto IP adresu, měli byste vidět vaše kontejner nginx a sváže s za nástrojem pro vyrovnávání zatížení.

![nginx zobrazení](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Změna velikosti služby
V přidání tooviewing vašich objektů v hello uživatelského rozhraní, můžete upravit a aktualizovat objekty rozhraní API Kubernetes hello.

Nejprve, klikněte na tlačítko **nasazení** v hello levé navigační podokně toosee hello nasazení pro vaši službu.

Až se v tomto zobrazení, klikněte na hello sady replik a pak klikněte na tlačítko **upravit** v horním navigačním panelu hello:

![Upravit Kubernetes](./media/container-service-kubernetes-ui/edit.png)

Upravit hello `spec.replicas` pole toobe `2`a klikněte na tlačítko **aktualizace**.

To způsobí, že hello počet replik toodrop tootwo odstraněním vaší pracovními stanicemi soustředěnými kolem.

 

