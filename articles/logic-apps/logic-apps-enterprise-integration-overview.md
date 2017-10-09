---
title: aaaEnterprise integrace pro B2B - Azure Logic Apps | Microsoft Docs
description: "Vytváření pracovních postupů B2B a podpora podnikové scénáře integrace aplikace logiky s hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8dc866533110b1d07f51cf446056d2ca5ce869ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-b2b-scenarios-and-communication-with-hello-enterprise-integration-pack"></a><span data-ttu-id="144c4-103">Přehled: Scénáře B2B a komunikaci s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="144c4-103">Overview: B2B scenarios and communication with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="144c4-104">Pro pracovní postupy business-to-business (B2B) a bezproblémové komunikaci s Azure Logic Apps můžete povolit podnikové scénáře integrace s společnosti Microsoft cloudové řešení, hello Enterprise integračního balíčku.</span><span class="sxs-lookup"><span data-stu-id="144c4-104">For business-to-business (B2B) workflows and seamless communication with Azure Logic Apps, you can enable enterprise integration scenarios with Microsoft's cloud-based solution, hello Enterprise Integration Pack.</span></span> <span data-ttu-id="144c4-105">Organizace můžou vyměňovat zprávy elektronicky, i když používají různé protokoly a formáty.</span><span class="sxs-lookup"><span data-stu-id="144c4-105">Organizations can exchange messages electronically, even if they use different protocols and formats.</span></span> <span data-ttu-id="144c4-106">Hello pack transformuje různých formátech do formátu, který může organizace systémy interpretovat a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="144c4-106">hello pack transforms different formats into a format that organizations' systems can interpret and process.</span></span> <span data-ttu-id="144c4-107">Organizace si můžou vyměňovat zprávy přes standardní protokoly, včetně [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), a [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span><span class="sxs-lookup"><span data-stu-id="144c4-107">Organizations can exchange messages through industry-standard protocols, including [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), and [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span></span> <span data-ttu-id="144c4-108">Také můžete zabezpečit zprávy pomocí šifrování a digitální podpisy.</span><span class="sxs-lookup"><span data-stu-id="144c4-108">You can also secure messages with both encryption and digital signatures.</span></span>

<span data-ttu-id="144c4-109">Pokud jste obeznámeni s BizTalk Server nebo Microsoft Azure BizTalk Services, jsou funkcí pro integraci Enterprise hello snadno toouse, protože jsou podobné jako většina koncepty.</span><span class="sxs-lookup"><span data-stu-id="144c4-109">If you are familiar with BizTalk Server or Microsoft Azure BizTalk Services, hello Enterprise Integration features are easy toouse because most concepts are similar.</span></span> <span data-ttu-id="144c4-110">Hlavní rozdíl je, že integrace Enterprise používá integrace účty toosimplify hello úložiště a správu artefaktů použitých při komunikaci s B2B.</span><span class="sxs-lookup"><span data-stu-id="144c4-110">One major difference is that Enterprise Integration uses integration accounts toosimplify hello storage and management of artifacts used in B2B communications.</span></span> 

<span data-ttu-id="144c4-111">Z pohledu architektury hello Enterprise integračního balíčku je založena na "účty integrace".</span><span class="sxs-lookup"><span data-stu-id="144c4-111">Architecturally, hello Enterprise Integration Pack is based on "integration accounts".</span></span> <span data-ttu-id="144c4-112">Tyto účty jsou založené na cloudu kontejnery, které ukládají všechny artefaktů, jako je schémat, partnerů, certifikátů, mapy a smlouvy.</span><span class="sxs-lookup"><span data-stu-id="144c4-112">These accounts are cloud-based containers that store all your artifacts, like schemas, partners, certificates, maps, and agreements.</span></span> <span data-ttu-id="144c4-113">Můžete použít tyto artefakty toodesign, nasadit a udržovat vaše aplikace B2B a také toobuild B2B pracovní postupy pro logic apps.</span><span class="sxs-lookup"><span data-stu-id="144c4-113">You can use these artifacts toodesign, deploy, and maintain your B2B apps and also toobuild B2B workflows for logic apps.</span></span> <span data-ttu-id="144c4-114">Ale předtím, než budete moci použít tyto artefakty, je nutné nejprve propojit svou aplikaci logiky tooyour integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="144c4-114">But before you can use these artifacts, you must first link your integration account tooyour logic app.</span></span> <span data-ttu-id="144c4-115">Poté můžete svou aplikaci logiky přístup k účtu integrace artefakty.</span><span class="sxs-lookup"><span data-stu-id="144c4-115">After that, your logic app can access your integration account's artifacts.</span></span>

## <a name="why-should-you-use-enterprise-integration"></a><span data-ttu-id="144c4-116">Proč používat integraci podnikových?</span><span class="sxs-lookup"><span data-stu-id="144c4-116">Why should you use enterprise integration?</span></span>

* <span data-ttu-id="144c4-117">Díky integraci enterprise můžete uložit všechny artefakty na jednom místě, váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="144c4-117">With enterprise integration, you can store all your artifacts in one place -- your integration account.</span></span>
* <span data-ttu-id="144c4-118">Můžete vytvářet pracovní postupy B2B a integrovat aplikacím třetích stran software jako služba (SaaS), místní aplikace a vlastních aplikací s použitím modulu Azure Logic Apps hello a všechny jeho konektory.</span><span class="sxs-lookup"><span data-stu-id="144c4-118">You can build B2B workflows and integrate with third-party software-as-service (SaaS) apps, on-premises apps, and custom apps by using hello Azure Logic Apps engine and all its connectors.</span></span>
* <span data-ttu-id="144c4-119">Můžete vytvořit vlastní kód pro logic apps, díky Azure functions.</span><span class="sxs-lookup"><span data-stu-id="144c4-119">You can create custom code for your logic apps with Azure functions.</span></span>

## <a name="how-tooget-started-with-enterprise-integration"></a><span data-ttu-id="144c4-120">Jak tooget pracovat s integrací enterprise?</span><span class="sxs-lookup"><span data-stu-id="144c4-120">How tooget started with enterprise integration?</span></span>

<span data-ttu-id="144c4-121">Můžete vytvářet a spravovat aplikace B2B s hello Enterprise integračního balíčku pomocí návrháře aplikace logiky hello v hello **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="144c4-121">You can build and manage B2B apps with hello Enterprise Integration Pack through hello Logic App Designer in hello **Azure portal**.</span></span> <span data-ttu-id="144c4-122">Můžete také spravovat aplikace logiky s [prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps prostředí PowerShell témata").</span><span class="sxs-lookup"><span data-stu-id="144c4-122">You can also manage your logic apps with [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell topics").</span></span>

<span data-ttu-id="144c4-123">Zde jsou hello hlavní kroky, které je třeba provést před vytvořením aplikace v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="144c4-123">Here are hello high-level steps you must take before you can create apps in hello Azure portal:</span></span>

![Přehled image](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a><span data-ttu-id="144c4-125">Jaké jsou některé běžné scénáře?</span><span class="sxs-lookup"><span data-stu-id="144c4-125">What are some common scenarios?</span></span>

<span data-ttu-id="144c4-126">Integrace Enterprise podporuje tyto oborových standardů:</span><span class="sxs-lookup"><span data-stu-id="144c4-126">Enterprise Integration supports these industry standards:</span></span>

* <span data-ttu-id="144c4-127">EDI - Electronic Data Interchange</span><span class="sxs-lookup"><span data-stu-id="144c4-127">EDI - Electronic Data Interchange</span></span>
* <span data-ttu-id="144c4-128">EAI - integraci podnikových aplikací</span><span class="sxs-lookup"><span data-stu-id="144c4-128">EAI - Enterprise Application Integration</span></span>

## <a name="heres-what-you-need-tooget-started"></a><span data-ttu-id="144c4-129">Tady je musíte tooget spuštění</span><span class="sxs-lookup"><span data-stu-id="144c4-129">Here's what you need tooget started</span></span>

* <span data-ttu-id="144c4-130">Předplatné služby Azure pomocí účtu, integrace</span><span class="sxs-lookup"><span data-stu-id="144c4-130">An Azure subscription with an integration account</span></span>
* <span data-ttu-id="144c4-131">Visual Studio 2015 toocreate mapy a schémata</span><span class="sxs-lookup"><span data-stu-id="144c4-131">Visual Studio 2015 toocreate maps and schemas</span></span>
* [<span data-ttu-id="144c4-132">Microsoft Azure Logic Apps Enterprise integrace nástrojů pro Visual Studio 2015 2.0</span><span class="sxs-lookup"><span data-stu-id="144c4-132">Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0</span></span>](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a><span data-ttu-id="144c4-133">Vyzkoušet</span><span class="sxs-lookup"><span data-stu-id="144c4-133">Try it now</span></span>

<span data-ttu-id="144c4-134">[Nasazení plně funkční ukázka AS2 odesílání a příjem aplikace logiky](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) používající hello B2B funkce pro Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="144c4-134">[Deploy a fully operational sample AS2 send & receive logic app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) that uses hello B2B features for Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="144c4-135">Další informace</span><span class="sxs-lookup"><span data-stu-id="144c4-135">Learn more</span></span>
* [<span data-ttu-id="144c4-136">Smlouvy</span><span class="sxs-lookup"><span data-stu-id="144c4-136">Agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")
* [<span data-ttu-id="144c4-137">Obchodní scénáře tooBusiness (B2B)</span><span class="sxs-lookup"><span data-stu-id="144c4-137">Business tooBusiness (B2B) scenarios</span></span>](../logic-apps/logic-apps-enterprise-integration-b2b.md "informace jak toocreate Logic apps s funkcemi B2B")  
* [<span data-ttu-id="144c4-138">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="144c4-138">Certificates</span></span>](logic-apps-enterprise-integration-certificates.md "Další informace o integraci certifikáty pro rozlehlé sítě")
* [<span data-ttu-id="144c4-139">Plochý soubor kódování a dekódování</span><span class="sxs-lookup"><span data-stu-id="144c4-139">Flat file encoding/decoding</span></span>](logic-apps-enterprise-integration-flatfile.md "informace jak tooencode a dekódování obsah plochý soubor")  
* [<span data-ttu-id="144c4-140">Účty pro integraci</span><span class="sxs-lookup"><span data-stu-id="144c4-140">Integration accounts</span></span>](../logic-apps/logic-apps-enterprise-integration-accounts.md "Další informace o účty pro integraci")
* [<span data-ttu-id="144c4-141">Mapuje</span><span class="sxs-lookup"><span data-stu-id="144c4-141">Maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")
* [<span data-ttu-id="144c4-142">Partneři</span><span class="sxs-lookup"><span data-stu-id="144c4-142">Partners</span></span>](logic-apps-enterprise-integration-partners.md "Další informace o partnery integrace enterprise")
* [<span data-ttu-id="144c4-143">Schémata</span><span class="sxs-lookup"><span data-stu-id="144c4-143">Schemas</span></span>](logic-apps-enterprise-integration-schemas.md "Další informace o integraci schémata enterprise")
* [<span data-ttu-id="144c4-144">Ověření XML zprávy</span><span class="sxs-lookup"><span data-stu-id="144c4-144">XML message validation</span></span>](logic-apps-enterprise-integration-xml.md "zjistěte, jak toovalidate XML zprávy s Logic apps")
* [<span data-ttu-id="144c4-145">Transformace XML</span><span class="sxs-lookup"><span data-stu-id="144c4-145">XML transform</span></span>](logic-apps-enterprise-integration-transform.md "Další informace o enterprise integrace mapy")
* [<span data-ttu-id="144c4-146">Konektory pro integraci</span><span class="sxs-lookup"><span data-stu-id="144c4-146">Enterprise Integration Connectors</span></span>](../connectors/apis-list.md "Další informace o konektory pro integraci pack")
* [<span data-ttu-id="144c4-147">Integrace účet Metadata</span><span class="sxs-lookup"><span data-stu-id="144c4-147">Integration Account Metadata</span></span>](../logic-apps/logic-apps-enterprise-integration-metadata.md "Další informace o integraci účet metadat")
* [<span data-ttu-id="144c4-148">Sledování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="144c4-148">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md "Další informace o sledování zpráv B2B")
* [<span data-ttu-id="144c4-149">Sledování zpráv B2B na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="144c4-149">Tracking B2B messages in OMS portal</span></span>](logic-apps-track-b2b-messages-omsportal.md "Další informace o sledování zpráv B2B na portálu OMS")

