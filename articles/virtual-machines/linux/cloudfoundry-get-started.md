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
# <a name="cloud-foundry-on-azure"></a><span data-ttu-id="17970-103">Cloud Foundry v Azure</span><span class="sxs-lookup"><span data-stu-id="17970-103">Cloud Foundry on Azure</span></span>

<span data-ttu-id="17970-104">Foundry cloudu je open-source platformy jako služba (PaaS) pro vytváření, nasazení a provozování 12-factor aplikace vyvinuté v různých jazyků a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="17970-104">Cloud Foundry is an open-source platform-as-a-service (PaaS) for building, deploying, and operating 12-factor applications developed in various languages and frameworks.</span></span> <span data-ttu-id="17970-105">Tento dokument popisuje hello možnosti, které jsou na cloudu Foundry systémem Azure a jak můžete začít používat.</span><span class="sxs-lookup"><span data-stu-id="17970-105">This document describes hello options you have for running Cloud Foundry on Azure and how you can get started.</span></span>

## <a name="cloud-foundry-offerings"></a><span data-ttu-id="17970-106">Pro cloud Foundry</span><span class="sxs-lookup"><span data-stu-id="17970-106">Cloud Foundry offerings</span></span>

<span data-ttu-id="17970-107">Existují dvě formy Foundry cloudu k dispozici toorun v Azure: cloudové Foundry (OSS CR) a hrají cloudu Foundry (PCF) open source.</span><span class="sxs-lookup"><span data-stu-id="17970-107">There are two forms of Cloud Foundry available toorun on Azure: open-source Cloud Foundry (OSS CF) and Pivotal Cloud Foundry (PCF).</span></span> <span data-ttu-id="17970-108">Je OSS CR zcela [open-source](https://github.com/cloudfoundry) verzi cloudu Foundry spravuje hello Foundation Foundry cloudu.</span><span class="sxs-lookup"><span data-stu-id="17970-108">OSS CF is an entirely [open-source](https://github.com/cloudfoundry) version of Cloud Foundry managed by hello Cloud Foundry Foundation.</span></span> <span data-ttu-id="17970-109">Otáčení Foundry cloudu je distribuci enterprise Cloud Foundry z hrají Inc. softwaru Podíváme se na některé z hello rozdíly mezi dvěma nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="17970-109">Pivotal Cloud Foundry is an enterprise distribution of Cloud Foundry from Pivotal Software Inc. We look at some of hello differences between hello two offerings.</span></span>

### <a name="open-source-cloud-foundry"></a><span data-ttu-id="17970-110">Open-source cloudu Foundry</span><span class="sxs-lookup"><span data-stu-id="17970-110">Open-source Cloud Foundry</span></span>

<span data-ttu-id="17970-111">Foundry OSS cloudu na platformě Azure můžete nasadit nejdřív nasazení BOSH ředitele a pak nasazením Foundry cloudu, pomocí hello [pokynů na Githubu](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span><span class="sxs-lookup"><span data-stu-id="17970-111">You can deploy OSS Cloud Foundry on Azure by first deploying a BOSH director and then deploying Cloud Foundry, using hello [instructions provided on GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span></span> <span data-ttu-id="17970-112">toolearn Další informace o použití CR operačních systémů najdete v části hello [dokumentace](https://docs.cloudfoundry.org/) poskytované hello Foundation Foundry cloudu.</span><span class="sxs-lookup"><span data-stu-id="17970-112">toolearn more about using OSS CF, see hello [documentation](https://docs.cloudfoundry.org/) provided by hello Cloud Foundry Foundation.</span></span>

<span data-ttu-id="17970-113">Společnost Microsoft poskytuje podporu best effort pro OSS CR prostřednictvím hello následující kanály komunity:</span><span class="sxs-lookup"><span data-stu-id="17970-113">Microsoft provides best-effort support for OSS CF through hello following community channels:</span></span>

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a><span data-ttu-id="17970-114">kanál bosh-azure ISP na [Slack Foundry cloudu](https://slack.cloudfoundry.org/)</span><span class="sxs-lookup"><span data-stu-id="17970-114">bosh-azure-cpi channel on [Cloud Foundry Slack](https://slack.cloudfoundry.org/)</span></span>
- [<span data-ttu-id="17970-115">seznamu adresátů bosh CR</span><span class="sxs-lookup"><span data-stu-id="17970-115">cf-bosh mailing list</span></span>](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- <span data-ttu-id="17970-116">GitHub problémy hello [ISP](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) a [služby service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span><span class="sxs-lookup"><span data-stu-id="17970-116">GitHub issues for hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) and [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span></span>

>[!NOTE]
> <span data-ttu-id="17970-117">Hello úrovně podpory pro vaše prostředky Azure, jako jsou hello virtuální počítače, kde spouštíte Foundry cloudu, je založena na vaší smlouvě podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="17970-117">hello level of support for your Azure resources, such as hello virtual machines where you run Cloud Foundry, is based on your Azure support agreement.</span></span> <span data-ttu-id="17970-118">Podpora komunity Best effort se vztahuje pouze na součásti toohello Foundry specifické pro Cloud.</span><span class="sxs-lookup"><span data-stu-id="17970-118">Best-effort community support only applies toohello Cloud Foundry-specific components.</span></span>

### <a name="pivotal-cloud-foundry"></a><span data-ttu-id="17970-119">Hrají cloudu Foundry</span><span class="sxs-lookup"><span data-stu-id="17970-119">Pivotal Cloud Foundry</span></span>

<span data-ttu-id="17970-120">Otáčení Foundry cloudu zahrnuje hello stejnou základní platformu jako distribuční hello OSS společně se sadou Nástroje pro správu proprietární a podnikové podpory.</span><span class="sxs-lookup"><span data-stu-id="17970-120">Pivotal Cloud Foundry includes hello same core platform as hello OSS distribution, along with a set of proprietary management tools and enterprise support.</span></span> <span data-ttu-id="17970-121">toorun PCF v Azure, musíte získat licenci z Pivotal.</span><span class="sxs-lookup"><span data-stu-id="17970-121">toorun PCF on Azure, you must acquire a license from Pivotal.</span></span> <span data-ttu-id="17970-122">Nabídka PCF Hello z Azure marketplace hello zahrnuje-90denní zkušební licence.</span><span class="sxs-lookup"><span data-stu-id="17970-122">hello PCF offer from hello Azure marketplace includes a 90-day trial license.</span></span>

<span data-ttu-id="17970-123">Hello nástroje zahrnují [hrají Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), webové aplikace, která zjednodušuje nasazení a správu cloudu Foundry foundation, a [hrají aplikace správce](https://docs.pivotal.io/pivotalcf/console/), webové aplikace pro správu Uživatelé a aplikace.</span><span class="sxs-lookup"><span data-stu-id="17970-123">hello tools include [Pivotal Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), a web application that simplifies deployment and management of a Cloud Foundry foundation, and [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), a web application for managing users and applications.</span></span>

<span data-ttu-id="17970-124">Kromě toho toohello kanály podpory pro OSS CR výše uvedené licenci PCF opravňuje jste toocontact Pivotal pro podporu.</span><span class="sxs-lookup"><span data-stu-id="17970-124">In addition toohello support channels listed for OSS CF above, a PCF license entitles you toocontact Pivotal for support.</span></span> <span data-ttu-id="17970-125">Microsoft a Pivotal také povolili podporu pracovních postupů, které vám umožňují toocontact buď stranu pro pomoc a mají váš dotaz směrovány odpovídajícím způsobem v závislosti na tom, kde je problém hello.</span><span class="sxs-lookup"><span data-stu-id="17970-125">Microsoft and Pivotal have also enabled support workflows that allow you toocontact either party for assistance and have your inquiry routed appropriately depending on where hello issue lies.</span></span>

## <a name="azure-service-broker"></a><span data-ttu-id="17970-126">Zprostředkovatele služby Azure</span><span class="sxs-lookup"><span data-stu-id="17970-126">Azure Service Broker</span></span>

<span data-ttu-id="17970-127">Cloud Foundry podporuje častější hello ["dvanácti factor aplikace"](https://12factor.net/) metody, která se zvýší úroveň čistou oddělit bezstavové aplikace, procesy a stavové služby zálohování.</span><span class="sxs-lookup"><span data-stu-id="17970-127">Cloud Foundry encourages hello ["twelve-factor app"](https://12factor.net/) methodology, which promotes a clean separation of stateless application processes and stateful backing services.</span></span> <span data-ttu-id="17970-128">[Zprostředkovatelé služeb](https://docs.cloudfoundry.org/services/api.html) nabízejí konzistentní způsob tooprovision a vytvořte vazbu tooapplications služby zálohování.</span><span class="sxs-lookup"><span data-stu-id="17970-128">[Service brokers](https://docs.cloudfoundry.org/services/api.html) offer a consistent way tooprovision and bind backing services tooapplications.</span></span> <span data-ttu-id="17970-129">Hello [služby Azure service broker](https://github.com/Azure/meta-azure-service-broker) obsahuje některé z hello klíče služby Azure přes tento kanál, včetně úložiště Azure a Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="17970-129">hello [Azure service broker](https://github.com/Azure/meta-azure-service-broker) provides some of hello key Azure services through this channel, including Azure storage and Azure SQL.</span></span>

<span data-ttu-id="17970-130">Pokud používáte hrají Foundry cloudu, hello služba service broker je také [k dispozici jako dlaždici](https://docs.pivotal.io/azure-sb/installing.html) z hello hrají sítě.</span><span class="sxs-lookup"><span data-stu-id="17970-130">If you are using Pivotal Cloud Foundry, hello service broker is also [available as a tile](https://docs.pivotal.io/azure-sb/installing.html) from hello Pivotal Network.</span></span>

## <a name="related-resources"></a><span data-ttu-id="17970-131">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="17970-131">Related resources</span></span>

### <a name="visual-studio-team-services-plugin"></a><span data-ttu-id="17970-132">Visual Studio Team Services modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="17970-132">Visual Studio Team Services plugin</span></span>

<span data-ttu-id="17970-133">Cloud Foundry je skvěle hodí tooagile vývoj softwaru, včetně použití hello průběžnou integraci (CI) a nastavené průběžné doručování (CD).</span><span class="sxs-lookup"><span data-stu-id="17970-133">Cloud Foundry is well suited tooagile software development, including hello use of continuous integration (CI) and continuous delivery (CD).</span></span> <span data-ttu-id="17970-134">Pokud používáte Visual Studio Team Services (VSTS) toomanage vašich projektů a chcete tooset až CI/CD kanálu cílení Foundry cloudu, můžete použít hello [Foundry cloudové služby VSTS sestavení rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span><span class="sxs-lookup"><span data-stu-id="17970-134">If you use Visual Studio Team Services (VSTS) toomanage your projects and would like tooset up a CI/CD pipeline targeting Cloud Foundry, you can use hello [VSTS Cloud Foundry build extension](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span></span> <span data-ttu-id="17970-135">Hello modul plug-in je jednoduchý tooconfigure a automatizovat nasazení tooCloud Foundry, zda běžící v Azure nebo jiné prostředí.</span><span class="sxs-lookup"><span data-stu-id="17970-135">hello plugin makes it simple tooconfigure and automate deployments tooCloud Foundry, whether running in Azure or another environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17970-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17970-136">Next steps</span></span>

- [<span data-ttu-id="17970-137">Nasazení hrají Foundry cloudu z hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="17970-137">Deploy Pivotal Cloud Foundry from hello Azure Marketplace</span></span>](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [<span data-ttu-id="17970-138">Nasazení aplikace tooCloud Foundry v Azure</span><span class="sxs-lookup"><span data-stu-id="17970-138">Deploy an app tooCloud Foundry in Azure</span></span>](./cloudfoundry-deploy-your-first-app.md)
