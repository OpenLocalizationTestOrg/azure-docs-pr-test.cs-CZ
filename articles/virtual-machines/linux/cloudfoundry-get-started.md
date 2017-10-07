---
title: "aaaGetting Začínáme s Cloud Foundry v Microsoft Azure | Microsoft Docs"
description: "Spustit OSS nebo Foundry hrají cloudu na platformě Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 56300d5a0a75b5a9f46145a49ed3f5daa774375a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-foundry-on-azure"></a>Cloud Foundry v Azure

Foundry cloudu je open-source platformy jako služba (PaaS) pro vytváření, nasazení a provozování 12-factor aplikace vyvinuté v různých jazyků a rozhraní. Tento dokument popisuje hello možnosti, které jsou na cloudu Foundry systémem Azure a jak můžete začít používat.

## <a name="cloud-foundry-offerings"></a>Pro cloud Foundry

Existují dvě formy Foundry cloudu k dispozici toorun v Azure: cloudové Foundry (OSS CR) a hrají cloudu Foundry (PCF) open source. Je OSS CR zcela [open-source](https://github.com/cloudfoundry) verzi cloudu Foundry spravuje hello Foundation Foundry cloudu. Otáčení Foundry cloudu je distribuci enterprise Cloud Foundry z hrají Inc. softwaru Podíváme se na některé z hello rozdíly mezi dvěma nabídky hello.

### <a name="open-source-cloud-foundry"></a>Open-source cloudu Foundry

Foundry OSS cloudu na platformě Azure můžete nasadit nejdřív nasazení BOSH ředitele a pak nasazením Foundry cloudu, pomocí hello [pokynů na Githubu](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). toolearn Další informace o použití CR operačních systémů najdete v části hello [dokumentace](https://docs.cloudfoundry.org/) poskytované hello Foundation Foundry cloudu.

Společnost Microsoft poskytuje podporu best effort pro OSS CR prostřednictvím hello následující kanály komunity:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>kanál bosh-azure ISP na [Slack Foundry cloudu](https://slack.cloudfoundry.org/)
- [seznamu adresátů bosh CR](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub problémy hello [ISP](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) a [služby service broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Hello úrovně podpory pro vaše prostředky Azure, jako jsou hello virtuální počítače, kde spouštíte Foundry cloudu, je založena na vaší smlouvě podporu Azure. Podpora komunity Best effort se vztahuje pouze na součásti toohello Foundry specifické pro Cloud.

### <a name="pivotal-cloud-foundry"></a>Hrají cloudu Foundry

Otáčení Foundry cloudu zahrnuje hello stejnou základní platformu jako distribuční hello OSS společně se sadou Nástroje pro správu proprietární a podnikové podpory. toorun PCF v Azure, musíte získat licenci z Pivotal. Nabídka PCF Hello z Azure marketplace hello zahrnuje-90denní zkušební licence.

Hello nástroje zahrnují [hrají Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), webové aplikace, která zjednodušuje nasazení a správu cloudu Foundry foundation, a [hrají aplikace správce](https://docs.pivotal.io/pivotalcf/console/), webové aplikace pro správu Uživatelé a aplikace.

Kromě toho toohello kanály podpory pro OSS CR výše uvedené licenci PCF opravňuje jste toocontact Pivotal pro podporu. Microsoft a Pivotal také povolili podporu pracovních postupů, které vám umožňují toocontact buď stranu pro pomoc a mají váš dotaz směrovány odpovídajícím způsobem v závislosti na tom, kde je problém hello.

## <a name="azure-service-broker"></a>Zprostředkovatele služby Azure

Cloud Foundry podporuje častější hello ["dvanácti factor aplikace"](https://12factor.net/) metody, která se zvýší úroveň čistou oddělit bezstavové aplikace, procesy a stavové služby zálohování. [Zprostředkovatelé služeb](https://docs.cloudfoundry.org/services/api.html) nabízejí konzistentní způsob tooprovision a vytvořte vazbu tooapplications služby zálohování. Hello [služby Azure service broker](https://github.com/Azure/meta-azure-service-broker) obsahuje některé z hello klíče služby Azure přes tento kanál, včetně úložiště Azure a Azure SQL.

Pokud používáte hrají Foundry cloudu, hello služba service broker je také [k dispozici jako dlaždici](https://docs.pivotal.io/azure-sb/installing.html) z hello hrají sítě.

## <a name="related-resources"></a>Související prostředky

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services modulu plug-in

Cloud Foundry je skvěle hodí tooagile vývoj softwaru, včetně použití hello průběžnou integraci (CI) a nastavené průběžné doručování (CD). Pokud používáte Visual Studio Team Services (VSTS) toomanage vašich projektů a chcete tooset až CI/CD kanálu cílení Foundry cloudu, můžete použít hello [Foundry cloudové služby VSTS sestavení rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). Hello modul plug-in je jednoduchý tooconfigure a automatizovat nasazení tooCloud Foundry, zda běžící v Azure nebo jiné prostředí.

## <a name="next-steps"></a>Další kroky

- [Nasazení hrají Foundry cloudu z hello Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Nasazení aplikace tooCloud Foundry v Azure](./cloudfoundry-deploy-your-first-app.md)
