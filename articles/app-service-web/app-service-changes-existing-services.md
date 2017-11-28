---
title: "aaaAzure služby App Service a její vliv na stávající služby Azure"
description: "Vysvětluje, jak hello nové Azure App Service a její funkce vliv na stávající služby v Azure."
services: app-service
documentationcenter: 
author: yochay
manager: nirma
editor: 
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: yochaykk
ms.openlocfilehash: a831a88fee38465e5b0b7c2c2340cf8a0d64c864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a><span data-ttu-id="a3455-103">Azure App Service a stávající služby Azure</span><span class="sxs-lookup"><span data-stu-id="a3455-103">Azure App Service and existing Azure services</span></span>
<span data-ttu-id="a3455-104">Tento článek popisuje tooexisting změny hello Azure services jako součást toobring změnu hello společně několik služeb Azure do [Azure App Service](https://azure.microsoft.com/services/app-service/), integrované novou nabídku.</span><span class="sxs-lookup"><span data-stu-id="a3455-104">This article outlines hello changes tooexisting Azure services as part of hello change toobring together several Azure services into [Azure App Service](https://azure.microsoft.com/services/app-service/), a new integrated offering.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a><span data-ttu-id="a3455-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a3455-105">Overview</span></span>
<span data-ttu-id="a3455-106">[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je nových a jedinečných Cloudová služba, která umožňuje vývojářům toocreate webových a mobilních aplikací pro libovolnou platformu a z jakéhokoliv zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3455-106">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a new and unique cloud service that enables developers toocreate web and mobile apps for any platform and any device.</span></span> <span data-ttu-id="a3455-107">App Service je, že že integrované řešení určené toostreamline opakuje kódování funkcí, integrace s enterprise a SaaS systémy a automatizovat firemní procesy. zároveň pokrývat vašim požadavkům na zabezpečení, spolehlivosti a škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="a3455-107">App Service is an integrated solution designed toostreamline repeated coding functions, integrate with enterprise and SaaS systems, and automate business processes while meeting your needs for security, reliability, and scalability.</span></span>

<span data-ttu-id="a3455-108">Služby App Service spojuje hello následující existující Azure services – [weby](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), a [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) do jednoho kombinaci služby, při Přidání výkonné nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="a3455-108">App Service brings together hello following existing Azure services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), and [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) into a single combined service, while adding powerful new capabilities.</span></span>  <span data-ttu-id="a3455-109">Služby App Service umožňuje toohost hello následující typy aplikací:</span><span class="sxs-lookup"><span data-stu-id="a3455-109">App Service allows you toohost hello following app types:</span></span>

* <span data-ttu-id="a3455-110">Web Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-110">Web Apps</span></span>
* <span data-ttu-id="a3455-111">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-111">Mobile Apps</span></span>
* <span data-ttu-id="a3455-112">API Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-112">API Apps</span></span>
* <span data-ttu-id="a3455-113">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-113">Logic Apps</span></span>

<span data-ttu-id="a3455-114">Hello následující tabulka vysvětluje, jak existující Azure services mapování tooApp služby a hello typy aplikací v rámci je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a3455-114">hello following table explains how existing Azure services map tooApp Service and hello app types available within it.</span></span>

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%"><span data-ttu-id="a3455-115">Stávající služba Azure</span><span class="sxs-lookup"><span data-stu-id="a3455-115">Existing Azure Service</span></span></th>
<th align="left", style="width:10%"><span data-ttu-id="a3455-116">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a3455-116">Azure App Service</span></span></th>
<th align="left", style="width:80%"><span data-ttu-id="a3455-117">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="a3455-117">What changed</span></span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><span data-ttu-id="a3455-118">Azure Websites</span><span class="sxs-lookup"><span data-stu-id="a3455-118">Azure Websites</span></span></td>
<td align="left"><span data-ttu-id="a3455-119">Web Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-119">Web Apps</span></span></td>
<td align="left"><li><span data-ttu-id="a3455-120">Pro weby Azure App Service je výhradně omezené toochanging hello název weby tooWeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3455-120">For Azure Websites, App Service is strictly limited toochanging hello name  Websites tooWeb Apps.</span></span>
<p><li><span data-ttu-id="a3455-121">Všechny existující instance webů, které jsou nyní webové aplikace ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="a3455-121">All your existing instances of Websites are now Web Apps in App Service.</span></span></p>
<p><li><span data-ttu-id="a3455-122">Dostanete vašich existujících webů prostřednictvím hello <a href="http://go.microsoft.com/fwlink/?LinkId=529715">portálu Azure</a>, kde najdete všechny existující weby v rámci <em>webové aplikace</em>.</span><span class="sxs-lookup"><span data-stu-id="a3455-122">You can access your existing websites via hello <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, where you will find all your existing sites under <em>Web Apps</em>.</span></span></p>
<p><li><span data-ttu-id="a3455-123"><em>Web, který je hostitelem plánování</em> je nyní <em>plán služby App Service</em>.</span><span class="sxs-lookup"><span data-stu-id="a3455-123"><em>Web Hosting Plan</em> is now <em>App Service Plan</em>.</span></span> <span data-ttu-id="a3455-124"><em>Plán služby App Service</em> může být hostitelem libovolného typu aplikace služby App Service, jako jsou webové, mobilní, logiku nebo rozhraní API aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3455-124">An <em>App Service Plan</em> can host any app type of App Service, such as Web, Mobile, Logic, or API apps.</span></span></p>
<p><li><span data-ttu-id="a3455-125">Azure App Service Web Apps je obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a3455-125">Azure App Service Web Apps is in General Availability.</span></span></p>
<p><li><span data-ttu-id="a3455-126"><a href="http://azure.microsoft.com/services/app-service/web/">Další informace o webových aplikacích</a>.</span><span class="sxs-lookup"><span data-stu-id="a3455-126"><a href="http://azure.microsoft.com/services/app-service/web/">Learn more about Web Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"><span data-ttu-id="a3455-127">Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="a3455-127">Azure Mobile Services</span></span></td>
<td align="left"><span data-ttu-id="a3455-128">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-128">Mobile Apps</span></span></td>
<td align="left"><p><li><span data-ttu-id="a3455-129">Mobile Services i nadále k dispozici jako samostatné služby toobe a dál plně podporovat.</span><span class="sxs-lookup"><span data-stu-id="a3455-129">Mobile Services continue toobe available as a standalone service and remain fully supported.</span></span></p>
<p><li><span data-ttu-id="a3455-130">Mobilní aplikace je typ aplikace ve službě App Service, která integruje všechny funkce hello mobilní služby a další.</span><span class="sxs-lookup"><span data-stu-id="a3455-130">Mobile Apps is an app type in App Service, which integrates all of hello functionality of Mobile Services and more.</span></span></p>
<p><li><span data-ttu-id="a3455-131">Je příliš snadné<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrovat z mobilní služby tooMobile aplikace</a>.</span><span class="sxs-lookup"><span data-stu-id="a3455-131">It is easy too<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrate from Mobile Services tooMobile Apps</a>.</span></span></p>
<p><li><span data-ttu-id="a3455-132">Jako součást služby App Service Mobile Apps získat nové funkce nad rámec Mobile Services, například integrace s místními a SaaS systémy, přípravné sloty, webové úlohy, možnosti lépe měřítka a další.</span><span class="sxs-lookup"><span data-stu-id="a3455-132">As part of App Service, Mobile Apps get new capabilities beyond Mobile Services, such as  integration with on-premises and SaaS systems, staging slots, WebJobs, better scaling options, and more.</span></span></p>
<p><li><span data-ttu-id="a3455-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Další informace o Mobile Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="a3455-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Learn more about Mobile Apps</a>.</span></span></p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left"><span data-ttu-id="a3455-134">API Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-134">API Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="a3455-135">API Apps je nový typ aplikace ve službě App Service, která vám umožní snadno vytvářet a používat rozhraní API v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a3455-135">API Apps is a new app type in App Service that lets you easily build and consume APIs in hello cloud.</span></span></p>
<p><li><span data-ttu-id="a3455-136"><a href="http://azure.microsoft.com/services/app-service/api/">Další informace o aplikacích API</a>.</span><span class="sxs-lookup"><span data-stu-id="a3455-136"><a href="http://azure.microsoft.com/services/app-service/api/">Learn more about API Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left"><span data-ttu-id="a3455-137">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="a3455-137">Logic Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="a3455-138">Služba Logic Apps je nový typ aplikace ve službě App Service, která umožňuje snadnou automatizaci obchodních procesů.</span><span class="sxs-lookup"><span data-stu-id="a3455-138">Logic Apps is a new app type in App Service that lets you easily automate business processes.</span></span></p>
<p><li><span data-ttu-id="a3455-139"><a href="http://azure.microsoft.com/services/app-service/logic/">Další informace o aplikacích logiky</a>.</span><span class="sxs-lookup"><span data-stu-id="a3455-139"><a href="http://azure.microsoft.com/services/app-service/logic/">Learn more about Logic Apps</a>.</span></span></p></td>
</tr>
<tr class="odd">
<td align="left"><span data-ttu-id="a3455-140">Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="a3455-140">Azure BizTalk Services</span></span></td>
<td align="left"><span data-ttu-id="a3455-141">Aplikacích API BizTalk</span><span class="sxs-lookup"><span data-stu-id="a3455-141">BizTalk API Apps</span></span></td>
<td align="left">
<li><p><span data-ttu-id="a3455-142">BizTalk Services i nadále k dispozici jako samostatné služby toobe a dál plně podporovat.</span><span class="sxs-lookup"><span data-stu-id="a3455-142">BizTalk Services continue toobe available as a standalone service and remain fully supported.</span></span></p>
<li><p><span data-ttu-id="a3455-143">Všechny možnosti hello služby BizTalk Services jsou integrované do služby App Service API Apps povolení uživatelé tooperform integraci podnikových aplikací a scénáře B2B integrace s žádným z hello typech aplikací ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="a3455-143">All hello capabilities of BizTalk Services are integrated into App Service as API Apps enabling users tooperform enterprise application integration and B2B integration scenarios with any of hello app types in App Service.</span></span></p>
<li><p><span data-ttu-id="a3455-144">S Logic Apps, můžete nyní obchodní procesy můžete automatizovat pomocí visual navrhovat pracovní postupy toocreate prostředí.</span><span class="sxs-lookup"><span data-stu-id="a3455-144">With Logic Apps, you can now automate business processes using a visual design experience toocreate workflows.</span></span></p></td>
</tr>
</tbody>
</table>

<span data-ttu-id="a3455-145">toolearn víc, navštivte [dokumentace služby App Service](https://azure.microsoft.com/documentation/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="a3455-145">toolearn more, please visit [App Service documentation](https://azure.microsoft.com/documentation/services/app-service/).</span></span>

