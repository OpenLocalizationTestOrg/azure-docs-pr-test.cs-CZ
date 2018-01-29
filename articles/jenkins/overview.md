---
title: "Přehled volaných a Azure | Microsoft Docs"
description: "Hostování volaných sestavení a nasazení serveru automation v Azure a pomocí Azure výpočetní a úložnou kapacitu rozšířit nepřetržité integrace a nasazení kanály (CI/CD)."
services: jenkins
author: rloutlaw
manager: justhe
ms.service: jenkins
ms.devlang: NA
ms.topic: article
ms.workload: na
ms.date: 08/22/2017
ms.author: routlaw
ms.custom: mvc
ms.openlocfilehash: daa202ddf0dc934c491ead3951ddc4fdc3dd819c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-and-jenkins"></a>Azure a volaných

[Volaných](https://jenkins.io/) je používána k nastavení průběžnou integraci a doručení (CI/CD) pro projekty softwaru server oblíbených open-source automatizace. Můžete hostovat vaše nasazení volaných v Azure nebo rozšířit existující konfiguraci volaných pomocí prostředků Azure. Moduly plug-in volaných jsou také k dispozici ke zjednodušení CI/CD aplikací do Azure.

Tento článek je úvodem do Azure pomocí volaných, s podrobnostmi o základní funkce Azure k dispozici volaných uživatelům. Začínáme s vlastní volaných server v Azure, najdete v tématu naše [rychlý Start](install-jenkins-solution-template.md).

## <a name="host-your-jenkins-servers-in-azure"></a>Hostovat vaše servery volaných v Azure

Růst volaných hostitele v Azure a centralizovat vaše sestavení automatizace a změnit měřítko nasazení potřebám vašich projektů softwaru. Volaných můžete nasadit v Azure pomocí:
 
- [Šablona řešení volaných](install-jenkins-solution-template.md) v Azure Marketplace.
- [Virtuální počítače Azure](/azure/virtual-machines/linux/overview). V tématu naše [kurzu](/azure/virtual-machines/linux/tutorial-jenkins-github-docker-cicd) k vytvoření instance volaných na virtuálním počítači.
- Na Kubernetes cluster se systémem [Azure Container Service](/azure/container-service/kubernetes/container-service-kubernetes-walkthrough), najdete v našich [postupy](/azure/container-service/kubernetes/container-service-kubernetes-jenkin).

Monitorovat a spravovat pomocí nasazení Azure volaných [analýzy protokolů](/azure/log-analytics/log-analytics-overview), [Operations Management Suite](/azure/operations-management-suite/operations-management-suite-overview)a [příkazového řádku Azure] (/ cli nebo azure nebo přehled).

## <a name="scale-your-build-automation-on-demand"></a>Škálovat vaše automatizace sestavení na vyžádání

Přidat agenty sestavení do stávajícího nasazení volaných škálovat vaše volaných sestavení kapacita jako číslo sestavení a složitost úloh a zvýšit kanály. Spuštěním těchto agentů sestavení na virtuálních počítačích Azure pomocí [modul plug-in Azure virtuálních počítačů agentů](jenkins-azure-vm-agents.md). V tématu naše [kurzu](/azure/jenkins/jenkins-azure-vm-agents) další podrobnosti.

Jednou nakonfigurované [Azure instanční objekt](/azure/azure-resource-manager/resource-group-overview), volaných úlohy a kanálů můžete pomocí těchto pověření:

- Bezpečně uložit a archivu sestavení artefaktů [Azure Storage](/azure/storage/common/storage-introduction) pomocí [modul plug-in Azure Storage](https://plugins.jenkins.io/windows-azure-storage). Zkontrolujte [volaných úložiště postupy](/azure/storage/common/storage-java-jenkins-continuous-integration-solution) Další informace.
- Správa a konfigurace prostředků Azure pomocí [rozhraní příkazového řádku Azure](/azure/jenkins/execute-cli-jenkins-pipeline).

## <a name="deploy-your-code-into-azure-services"></a>Nasazení kódu do služby Azure

Moduly plug-in volaných použijte k nasazení aplikace do Azure jako součást kanály volaných CI/CD. Nasazení do [Azure App Service](/azure/app-service/) a [Azure Container Service](/azure/container-service/kubernetes/) umožňuje fáze, testování a aktualizace verzí pro vaše aplikace bez správy odpovídající infrastruktury.

 Moduly plug-in jsou k dispozici pro nasazení pro následující služby a prostředí:

- [Webové aplikace Azure v systému Linux](/azure/app-service/containers/app-service-linux-intro). Najdete v článku [kurzu](java-deploy-webapp-tutorial.md) začít pracovat.
- [Webové aplikace Azure](/azure/app-service/app-service-web-overview). Najdete v článku [postupy](deploy-Jenkins-app-service-plugin.md) začít pracovat.

