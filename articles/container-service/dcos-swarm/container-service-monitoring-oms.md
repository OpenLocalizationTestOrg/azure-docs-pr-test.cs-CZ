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
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Monitorování clusteru Azure Container Service DC/OS s Operations Management Suite

Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur. Kontejner řešení je řešení v OMS analýzy protokolů, které vám umožní zobrazit hello kontejneru inventář, výkonu a protokoly na jednom místě. Můžete auditovat, řešení potíží s kontejnery zobrazením hello protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.

![](media/container-service-monitoring-oms/image1.png)

Další informace o řešení kontejneru naleznete toothe [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-hello-dcos-universe"></a>Nastavení OMS z universe hello DC/OS


Tento článek předpokládá, že jste nastavili DC/OS a nasadili jednoduchého webového kontejneru aplikace v clusteru hello.

### <a name="pre-requisite"></a>Předpoklad
- [Předplatné služby Microsoft Azure](https://azure.microsoft.com/free/) -vám to zdarma.  
- Instalační program pracovní prostor Microsoft OMS – najdete v části "Krok 3" pod
- [Rozhraní příkazového řádku DC/OS](https://dcos.io/docs/1.8/usage/cli/install/) nainstalována.

1. Na řídicím panelu hello DC/OS klikněte na Universe a vyhledejte "OMS, jak je uvedeno níže.

![](media/container-service-monitoring-oms/image2.png)

2. Klikněte na **Nainstalovat**. Uvidíte pop až s informace o verzi hello OMS a **instalovat balíček** nebo **rozšířený instalace** tlačítko. Když kliknete na tlačítko **rozšířený instalace**, což vede toohello **vlastnosti konkrétní konfigurace OMS** stránky.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Zde, zobrazí se výzva tooenter hello `wsid` (hello ID pracovního prostoru OMS) a `wskey` (hello OMS primární klíč pro id pracovního prostoru hello). tooget obou `wsid` a `wskey` potřebovat toocreate účet OMS na <https://mms.microsoft.com>. Postupujte podle kroků toocreate hello účet. Po dokončení vytváření hello účet, bude třeba tooobtain vaše `wsid` a `wskey` kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux** , jak je uvedeno níže.

 ![](media/container-service-monitoring-oms/image5.png)

4. Vyberte hello číslo vám OMS instancí a klikněte na tlačítko "Zkontrolovat a nainstalovat" hello. Obvykle můžete toohave hello počet OMS instance stejné toohello Virtuálního počítače budete mít v clusteru agenta. Agent OMS pro Linux je nainstaluje jako jednotlivé kontejnery pro každý virtuální počítač, zda chce toocollect informace pro monitorování a informace o protokolování.

## <a name="setting-up-a-simple-oms-dashboard"></a>Nastavení jednoduchý řídicí panel OMS

Po instalaci hello OMS agenta pro Linux na virtuálních počítačích hello, dalším krokem je tooset si řídicí panel hello OMS. Existují dva způsoby toodo to: OMS portál nebo portál Azure.

### <a name="oms-portal"></a>Portálu OMS 

Přihlaste se na portálu OMS toohello (<https://mms.microsoft.com>) a přejděte toohello **řešení Galerie**.

![](media/container-service-monitoring-oms/image6.png)

Jakmile jste na hello **řešení Galerie**, vyberte **kontejnery**.

![](media/container-service-monitoring-oms/image7.png)

Po dokončení výběru hello kontejneru řešení, zobrazí se dlaždice na stránce Přehled OMS řídicí panel hello hello. Jakmile je indexovaný hello požity kontejner dat, zobrazí se dlaždice hello naplněný informace o řešení dlaždice zobrazení.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Přihlášení na portál tooAzure <https://portal.microsoft.com/>. Přejděte na **Marketplace**, vyberte **monitorování + správu** a klikněte na tlačítko **najdete v článku všechny**. Pak zadejte `containers` ve vyhledávání. Zobrazí se ve výsledcích hledání hello – "kontejnery". Vyberte **kontejnery** a klikněte na tlačítko **vytvořit**.

![](media/container-service-monitoring-oms/image9.png)

Po kliknutí na tlačítko **vytvořit**, se zobrazí výzvu k pracovního prostoru. Vyberte pracovní prostor nebo pokud jeden nemáte, vytvořte nový pracovní prostor.

![](media/container-service-monitoring-oms/image10.PNG)

Po dokončení výběru pracovního prostoru, klikněte na tlačítko **vytvořit**.

![](media/container-service-monitoring-oms/image11.png)

Další informace o hello OMS kontejneru řešení naleznete toothe [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).

### <a name="how-tooscale-oms-agent-with-acs-dcos"></a>Jak tooscale agenta OMS s ACS DC/OS 

V případě, že potřebujete toohave nainstalovaný agent OMS souborem hello uzlu skutečný počet nebo jsou vertikálním navýšení kapacity VMSS přidáním více virtuálních počítačů, můžete tak učinit pomocí příjmu hello `msoms` služby.

Můžete se vrátit tooMarathon nebo hello kartě služeb uživatelského rozhraní DC/OS a škálovat vaše počet uzlů.

![](media/container-service-monitoring-oms/image12.PNG)

To nasadí tooother uzly, které ještě nebyly nasadit agenta OMS hello.

## <a name="uninstall-ms-oms"></a>Odinstalujte MS OMS

toouninstall MS OMS zadejte hello následující příkaz:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Dejte nám vědět!
Co funguje? Co je chybějící? Co potřebujete pro tento toobe užitečné pro vás? Dejte nám vědět v <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Další kroky

 Teď, když jste nastavili OMS toomonitor kontejnerů,[najdete v části řídicího panelu kontejneru](../../log-analytics/log-analytics-containers.md).
