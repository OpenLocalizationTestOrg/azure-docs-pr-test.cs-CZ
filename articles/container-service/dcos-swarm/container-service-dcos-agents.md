---
title: fondy agenta aaaDC/OS pro Azure Container Service | Microsoft Docs
description: "Jak fungují hello veřejné a privátní agenta fondy s clusteru služby Azure Container Service DC/OS"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: c7d3889db07cb9908e8b68b668bd8a14ef3c2552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>Fondy agenta DC/OS pro Azure Container Service
Clustery DC/OS v Azure Container Service obsahovat agenta uzly ve dvou fondů, veřejné fondu a privátní fondu. Aplikace lze nasadit tooeither fondu, které mají vliv na usnadnění přístupu mezi počítači v kontejneru služby. Hello počítače můžou být zveřejněné toohello Internetu (veřejných) nebo interní (soukromý) zachovány. Tento článek poskytuje stručný přehled proto nejsou veřejné a privátní fondy.


* **Privátní agenti**: uzly privátní agenta spustit přes síť s směrovat. Tato síť je přístupné pouze ze zóny správce hello nebo prostřednictvím hello veřejné zóny hraniční směrovač. Ve výchozím nastavení spustí DC/OS aplikace na uzlech privátní agenta. 

* **Veřejné agenty**: uzly veřejného agenta spustit DC/OS aplikace a služby přes síť s veřejně přístupná. 

Další informace o zabezpečení sítě DC/OS, najdete v části hello [DC/OS dokumentaci](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Nasazení agenta fondy

fondy agenta DC/OS Hello v Azure Container Service vytvoří následujícím způsobem:

* Hello **privátní fondu** obsahuje hello počet uzlů agenta, můžete určit, kdy jste [nasadit cluster DC/OS hello](container-service-deployment.md). 

* Hello **veřejné fondu** původně obsahuje předem určený počet uzlů agenta. Tento fond je automaticky přidáno při zřízení clusteru DC/OS hello.

fond Hello privátní a veřejné fondu hello jsou sady škálování virtuálního počítače Azure. Po nasazení můžete nastavit velikost těchto fondů.

## <a name="use-agent-pools"></a>Používat fondy agenta
Ve výchozím nastavení **Marathon** nasadí žádné nové aplikace toohello *privátní* agenta uzly. Máte tooexplicitly nasazení toohello aplikace hello *veřejné* uzly během vytváření hello aplikace hello. Vyberte hello **volitelné** kartě a zadejte **ACCEPTED** pro hello **role prostředků** hodnotu. Tento proces je popsána [sem](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) a v hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentaci.

## <a name="next-steps"></a>Další kroky
* Další informace o [Správa kontejnerů Váš DC/OS](container-service-mesos-marathon-ui.md).

* Zjistěte, jak příliš[otevřít bránu firewall hello](container-service-enable-public-access.md) poskytované Azure tooallow veřejný přístup tooyour DC/OS kontejnery.

