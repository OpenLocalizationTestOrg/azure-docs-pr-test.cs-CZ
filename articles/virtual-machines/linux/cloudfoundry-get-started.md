---
title: "Začínáme s Cloud Foundry na platformě Microsoft Azure | Microsoft Docs"
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
ms.openlocfilehash: 94fbde7707ea9a91076780fdefc3f5a827e0e7b2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-foundry-on-azure"></a><span data-ttu-id="9988c-103">Cloud Foundry v Azure</span><span class="sxs-lookup"><span data-stu-id="9988c-103">Cloud Foundry on Azure</span></span>

<span data-ttu-id="9988c-104">Foundry cloudu je open-source platformy jako služba (PaaS) pro vytváření, nasazení a provozování 12-factor aplikace vyvinuté v různých jazyků a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9988c-104">Cloud Foundry is an open-source platform-as-a-service (PaaS) for building, deploying, and operating 12-factor applications developed in various languages and frameworks.</span></span> <span data-ttu-id="9988c-105">Tento dokument popisuje možnosti, které jsou na cloudu Foundry systémem Azure a jak můžete začít používat.</span><span class="sxs-lookup"><span data-stu-id="9988c-105">This document describes the options you have for running Cloud Foundry on Azure and how you can get started.</span></span>

## <a name="cloud-foundry-offerings"></a><span data-ttu-id="9988c-106">Pro cloud Foundry</span><span class="sxs-lookup"><span data-stu-id="9988c-106">Cloud Foundry offerings</span></span>

<span data-ttu-id="9988c-107">Existují dvě formy Foundry cloudu k dispozici pro spuštění v Azure: cloudové Foundry (OSS CR) a hrají cloudu Foundry (PCF) open source.</span><span class="sxs-lookup"><span data-stu-id="9988c-107">There are two forms of Cloud Foundry available to run on Azure: open-source Cloud Foundry (OSS CF) and Pivotal Cloud Foundry (PCF).</span></span> <span data-ttu-id="9988c-108">Je OSS CR zcela [open-source](https://github.com/cloudfoundry) verzi cloudu Foundry spravuje Foundation Foundry cloudu.</span><span class="sxs-lookup"><span data-stu-id="9988c-108">OSS CF is an entirely [open-source](https://github.com/cloudfoundry) version of Cloud Foundry managed by the Cloud Foundry Foundation.</span></span> <span data-ttu-id="9988c-109">Otáčení Foundry cloudu je distribuci enterprise Cloud Foundry z hrají Inc. softwaru Podíváme se na některé rozdíly mezi dvěma nabídky.</span><span class="sxs-lookup"><span data-stu-id="9988c-109">Pivotal Cloud Foundry is an enterprise distribution of Cloud Foundry from Pivotal Software Inc. We look at some of the differences between the two offerings.</span></span>

### <a name="open-source-cloud-foundry"></a><span data-ttu-id="9988c-110">Open-source cloudu Foundry</span><span class="sxs-lookup"><span data-stu-id="9988c-110">Open-source Cloud Foundry</span></span>

<span data-ttu-id="9988c-111">Nejprve nasazení BOSH ředitele a pak nasazením Foundry cloudu, můžete nasadit Foundry cloudu operačních systémů v Azure pomocí [pokynů na Githubu](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span><span class="sxs-lookup"><span data-stu-id="9988c-111">You can deploy OSS Cloud Foundry on Azure by first deploying a BOSH director and then deploying Cloud Foundry, using the [instructions provided on GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span></span> <span data-ttu-id="9988c-112">Další informace o používání CR operačních systémů najdete v tématu [dokumentace](https://docs.cloudfoundry.org/) poskytované Foundation Foundry cloudu.</span><span class="sxs-lookup"><span data-stu-id="9988c-112">To learn more about using OSS CF, see the [documentation](https://docs.cloudfoundry.org/) provided by the Cloud Foundry Foundation.</span></span>

<span data-ttu-id="9988c-113">Společnost Microsoft poskytuje podporu best effort pro OSS CR těmito cestami komunity:</span><span class="sxs-lookup"><span data-stu-id="9988c-113">Microsoft provides best-effort support for OSS CF through the following community channels:</span></span>

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a><span data-ttu-id="9988c-114">kanál bosh-azure ISP na [Slack Foundry cloudu](https://slack.cloudfoundry.org/)</span><span class="sxs-lookup"><span data-stu-id="9988c-114">bosh-azure-cpi channel on [Cloud Foundry Slack](https://slack.cloudfoundry.org/)</span></span>
- [<span data-ttu-id="9988c-115">seznamu adresátů bosh CR</span><span class="sxs-lookup"><span data-stu-id="9988c-115">cf-bosh mailing list</span></span>](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- <span data-ttu-id="9988c-116">GitHub problémy pro [ISP](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) a [služby service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span><span class="sxs-lookup"><span data-stu-id="9988c-116">GitHub issues for the [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) and [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span></span>

>[!NOTE]
> <span data-ttu-id="9988c-117">Úroveň podpory pro vaše prostředky Azure, jako jsou virtuální počítače, kde spouštíte cloudu Foundry vychází z vaší smlouvy podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="9988c-117">The level of support for your Azure resources, such as the virtual machines where you run Cloud Foundry, is based on your Azure support agreement.</span></span> <span data-ttu-id="9988c-118">Podpora komunity Best effort platí pouze pro komponenty Foundry specifické pro Cloud.</span><span class="sxs-lookup"><span data-stu-id="9988c-118">Best-effort community support only applies to the Cloud Foundry-specific components.</span></span>

### <a name="pivotal-cloud-foundry"></a><span data-ttu-id="9988c-119">Hrají cloudu Foundry</span><span class="sxs-lookup"><span data-stu-id="9988c-119">Pivotal Cloud Foundry</span></span>

<span data-ttu-id="9988c-120">Otáčení Foundry cloudu obsahuje stejnou základní platformu jako distribuce operačních systémů, společně se sadou Nástroje pro správu proprietární a podnikové podpory.</span><span class="sxs-lookup"><span data-stu-id="9988c-120">Pivotal Cloud Foundry includes the same core platform as the OSS distribution, along with a set of proprietary management tools and enterprise support.</span></span> <span data-ttu-id="9988c-121">Pokud chcete spustit PCF v Azure, musíte získat licenci z Pivotal.</span><span class="sxs-lookup"><span data-stu-id="9988c-121">To run PCF on Azure, you must acquire a license from Pivotal.</span></span> <span data-ttu-id="9988c-122">Nabídka PCF z Azure marketplace zahrnuje-90denní zkušební licence.</span><span class="sxs-lookup"><span data-stu-id="9988c-122">The PCF offer from the Azure marketplace includes a 90-day trial license.</span></span>

<span data-ttu-id="9988c-123">K nástrojům patří [hrají Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), webové aplikace, která zjednodušuje nasazení a správu cloudu Foundry foundation, a [hrají aplikace správce](https://docs.pivotal.io/pivotalcf/console/), webové aplikace pro správu uživatelů a aplikací.</span><span class="sxs-lookup"><span data-stu-id="9988c-123">The tools include [Pivotal Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), a web application that simplifies deployment and management of a Cloud Foundry foundation, and [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), a web application for managing users and applications.</span></span>

<span data-ttu-id="9988c-124">Kromě kanály podpory pro OSS CR výše uvedené licenci PCF vás opravňuje k Pivotal požádat o podporu.</span><span class="sxs-lookup"><span data-stu-id="9988c-124">In addition to the support channels listed for OSS CF above, a PCF license entitles you to contact Pivotal for support.</span></span> <span data-ttu-id="9988c-125">Společnost Microsoft a Pivotal také povolili podporu pracovních postupů, které vám umožní buď strany požádejte o pomoc a mít dotazu směrovány odpovídajícím způsobem v závislosti na tom, kde je problém.</span><span class="sxs-lookup"><span data-stu-id="9988c-125">Microsoft and Pivotal have also enabled support workflows that allow you to contact either party for assistance and have your inquiry routed appropriately depending on where the issue lies.</span></span>

## <a name="azure-service-broker"></a><span data-ttu-id="9988c-126">Zprostředkovatele služby Azure</span><span class="sxs-lookup"><span data-stu-id="9988c-126">Azure Service Broker</span></span>

<span data-ttu-id="9988c-127">Umožňuje Foundry cloudu ["dvanácti factor aplikace"](https://12factor.net/) metody, která se zvýší úroveň čistou oddělit bezstavové aplikace, procesy a stavové služby zálohování.</span><span class="sxs-lookup"><span data-stu-id="9988c-127">Cloud Foundry encourages the ["twelve-factor app"](https://12factor.net/) methodology, which promotes a clean separation of stateless application processes and stateful backing services.</span></span> <span data-ttu-id="9988c-128">[Zprostředkovatelé služeb](https://docs.cloudfoundry.org/services/api.html) nabízejí konzistentní způsob, jak zřídit a vytvořte vazbu základní služby pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="9988c-128">[Service brokers](https://docs.cloudfoundry.org/services/api.html) offer a consistent way to provision and bind backing services to applications.</span></span> <span data-ttu-id="9988c-129">[Služby Azure service broker](https://github.com/Azure/meta-azure-service-broker) obsahuje některé z klíčových služeb Azure prostřednictvím tohoto kanálu, včetně úložiště Azure a Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="9988c-129">The [Azure service broker](https://github.com/Azure/meta-azure-service-broker) provides some of the key Azure services through this channel, including Azure storage and Azure SQL.</span></span>

<span data-ttu-id="9988c-130">Pokud používáte hrají Foundry cloudu, služby service broker je také [k dispozici jako dlaždici](https://docs.pivotal.io/azure-sb/installing.html) z hrají sítě.</span><span class="sxs-lookup"><span data-stu-id="9988c-130">If you are using Pivotal Cloud Foundry, the service broker is also [available as a tile](https://docs.pivotal.io/azure-sb/installing.html) from the Pivotal Network.</span></span>

## <a name="related-resources"></a><span data-ttu-id="9988c-131">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="9988c-131">Related resources</span></span>

### <a name="visual-studio-team-services-plugin"></a><span data-ttu-id="9988c-132">Visual Studio Team Services modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="9988c-132">Visual Studio Team Services plugin</span></span>

<span data-ttu-id="9988c-133">Cloud Foundry je skvěle hodí pro vývoj agilní softwaru, včetně použití průběžnou integraci (CI) a nastavené průběžné doručování (CD).</span><span class="sxs-lookup"><span data-stu-id="9988c-133">Cloud Foundry is well suited to agile software development, including the use of continuous integration (CI) and continuous delivery (CD).</span></span> <span data-ttu-id="9988c-134">Pokud používáte Visual Studio Team Services (VSTS) ke správě vašich projektů a chce nastavit až CI/CD kanálu cílení Foundry cloudu, můžete použít [Foundry cloudové služby VSTS sestavení rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span><span class="sxs-lookup"><span data-stu-id="9988c-134">If you use Visual Studio Team Services (VSTS) to manage your projects and would like to set up a CI/CD pipeline targeting Cloud Foundry, you can use the [VSTS Cloud Foundry build extension](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span></span> <span data-ttu-id="9988c-135">Tento modul plug-in můžete snadno nakonfigurovat a automatizovat nasazení do cloudu Foundry, zda běžící v Azure nebo jiné prostředí.</span><span class="sxs-lookup"><span data-stu-id="9988c-135">The plugin makes it simple to configure and automate deployments to Cloud Foundry, whether running in Azure or another environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9988c-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9988c-136">Next steps</span></span>

- [<span data-ttu-id="9988c-137">Nasazení Foundry hrají cloudu z Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="9988c-137">Deploy Pivotal Cloud Foundry from the Azure Marketplace</span></span>](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [<span data-ttu-id="9988c-138">Nasazení aplikace do cloudu Foundry v Azure</span><span class="sxs-lookup"><span data-stu-id="9988c-138">Deploy an app to Cloud Foundry in Azure</span></span>](./cloudfoundry-deploy-your-first-app.md)