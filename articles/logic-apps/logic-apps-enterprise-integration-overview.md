---
title: Integrace Enterprise pro B2B - Azure Logic Apps | Microsoft Docs
description: "Vytváření pracovních postupů B2B a podpora podnikové scénáře integrace aplikace logiky s Enterprise integračního balíčku"
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
ms.openlocfilehash: 9462707db03ecfcc3d5186ce7ded8655ad3bdcc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="overview-b2b-scenarios-and-communication-with-the-enterprise-integration-pack"></a><span data-ttu-id="92efe-103">Přehled: Scénáře B2B a komunikaci s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="92efe-103">Overview: B2B scenarios and communication with the Enterprise Integration Pack</span></span>

<span data-ttu-id="92efe-104">Pro pracovní postupy business-to-business (B2B) a bezproblémové komunikaci s Azure Logic Apps můžete povolit podnikové scénáře integrace s společnosti Microsoft cloudové řešení, Enterprise integračního balíčku.</span><span class="sxs-lookup"><span data-stu-id="92efe-104">For business-to-business (B2B) workflows and seamless communication with Azure Logic Apps, you can enable enterprise integration scenarios with Microsoft's cloud-based solution, the Enterprise Integration Pack.</span></span> <span data-ttu-id="92efe-105">Organizace můžou vyměňovat zprávy elektronicky, i když používají různé protokoly a formáty.</span><span class="sxs-lookup"><span data-stu-id="92efe-105">Organizations can exchange messages electronically, even if they use different protocols and formats.</span></span> <span data-ttu-id="92efe-106">Sada transformuje různých formátech do formátu, který může organizace systémy interpretovat a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="92efe-106">The pack transforms different formats into a format that organizations' systems can interpret and process.</span></span> <span data-ttu-id="92efe-107">Organizace si můžou vyměňovat zprávy přes standardní protokoly, včetně [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), a [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span><span class="sxs-lookup"><span data-stu-id="92efe-107">Organizations can exchange messages through industry-standard protocols, including [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), and [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span></span> <span data-ttu-id="92efe-108">Také můžete zabezpečit zprávy pomocí šifrování a digitální podpisy.</span><span class="sxs-lookup"><span data-stu-id="92efe-108">You can also secure messages with both encryption and digital signatures.</span></span>

<span data-ttu-id="92efe-109">Pokud jste obeznámeni s BizTalk Server nebo Microsoft Azure BizTalk Services, funkcí pro integraci Enterprise lze snadno použít, protože jsou podobné jako většina koncepty.</span><span class="sxs-lookup"><span data-stu-id="92efe-109">If you are familiar with BizTalk Server or Microsoft Azure BizTalk Services, the Enterprise Integration features are easy to use because most concepts are similar.</span></span> <span data-ttu-id="92efe-110">Hlavní rozdíl je, že integrace Enterprise používá účty pro integraci zjednodušit úložiště a správu artefaktů použitých při komunikaci s B2B.</span><span class="sxs-lookup"><span data-stu-id="92efe-110">One major difference is that Enterprise Integration uses integration accounts to simplify the storage and management of artifacts used in B2B communications.</span></span> 

<span data-ttu-id="92efe-111">Z pohledu architektury Enterprise integračního balíčku je založena na "účty integrace".</span><span class="sxs-lookup"><span data-stu-id="92efe-111">Architecturally, the Enterprise Integration Pack is based on "integration accounts".</span></span> <span data-ttu-id="92efe-112">Tyto účty jsou založené na cloudu kontejnery, které ukládají všechny artefaktů, jako je schémat, partnerů, certifikátů, mapy a smlouvy.</span><span class="sxs-lookup"><span data-stu-id="92efe-112">These accounts are cloud-based containers that store all your artifacts, like schemas, partners, certificates, maps, and agreements.</span></span> <span data-ttu-id="92efe-113">Těchto artefaktů můžete použít k návrhu, nasazení a údržbě aplikace B2B a také k vytváření pracovních postupů B2B pro logic apps.</span><span class="sxs-lookup"><span data-stu-id="92efe-113">You can use these artifacts to design, deploy, and maintain your B2B apps and also to build B2B workflows for logic apps.</span></span> <span data-ttu-id="92efe-114">Ale předtím, než budete moci použít tyto artefakty, je nutné nejprve propojit účtu integraci do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="92efe-114">But before you can use these artifacts, you must first link your integration account to your logic app.</span></span> <span data-ttu-id="92efe-115">Poté můžete svou aplikaci logiky přístup k účtu integrace artefakty.</span><span class="sxs-lookup"><span data-stu-id="92efe-115">After that, your logic app can access your integration account's artifacts.</span></span>

## <a name="why-should-you-use-enterprise-integration"></a><span data-ttu-id="92efe-116">Proč používat integraci podnikových?</span><span class="sxs-lookup"><span data-stu-id="92efe-116">Why should you use enterprise integration?</span></span>

* <span data-ttu-id="92efe-117">Díky integraci enterprise můžete uložit všechny artefakty na jednom místě, váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="92efe-117">With enterprise integration, you can store all your artifacts in one place -- your integration account.</span></span>
* <span data-ttu-id="92efe-118">Můžete vytvářet pracovní postupy B2B a integrovat aplikacím třetích stran software jako služba (SaaS), místní aplikace a vlastních aplikací s použitím modulu Azure Logic Apps a všechny jeho konektory.</span><span class="sxs-lookup"><span data-stu-id="92efe-118">You can build B2B workflows and integrate with third-party software-as-service (SaaS) apps, on-premises apps, and custom apps by using the Azure Logic Apps engine and all its connectors.</span></span>
* <span data-ttu-id="92efe-119">Můžete vytvořit vlastní kód pro logic apps, díky Azure functions.</span><span class="sxs-lookup"><span data-stu-id="92efe-119">You can create custom code for your logic apps with Azure functions.</span></span>

## <a name="how-to-get-started-with-enterprise-integration"></a><span data-ttu-id="92efe-120">Jak začít pracovat s integrací enterprise?</span><span class="sxs-lookup"><span data-stu-id="92efe-120">How to get started with enterprise integration?</span></span>

<span data-ttu-id="92efe-121">Můžete vytvářet a spravovat aplikace B2B s Enterprise integračního balíčku pomocí návrháře logiku aplikace v **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="92efe-121">You can build and manage B2B apps with the Enterprise Integration Pack through the Logic App Designer in the **Azure portal**.</span></span> <span data-ttu-id="92efe-122">Můžete také spravovat aplikace logiky s [prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps prostředí PowerShell témata").</span><span class="sxs-lookup"><span data-stu-id="92efe-122">You can also manage your logic apps with [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell topics").</span></span>

<span data-ttu-id="92efe-123">Zde jsou základní kroky, které je nutné provést před vytvořením aplikací na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="92efe-123">Here are the high-level steps you must take before you can create apps in the Azure portal:</span></span>

![Přehled image](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a><span data-ttu-id="92efe-125">Jaké jsou některé běžné scénáře?</span><span class="sxs-lookup"><span data-stu-id="92efe-125">What are some common scenarios?</span></span>

<span data-ttu-id="92efe-126">Integrace Enterprise podporuje tyto oborových standardů:</span><span class="sxs-lookup"><span data-stu-id="92efe-126">Enterprise Integration supports these industry standards:</span></span>

* <span data-ttu-id="92efe-127">EDI - Electronic Data Interchange</span><span class="sxs-lookup"><span data-stu-id="92efe-127">EDI - Electronic Data Interchange</span></span>
* <span data-ttu-id="92efe-128">EAI - integraci podnikových aplikací</span><span class="sxs-lookup"><span data-stu-id="92efe-128">EAI - Enterprise Application Integration</span></span>

## <a name="heres-what-you-need-to-get-started"></a><span data-ttu-id="92efe-129">Tady je co potřebujete, abyste mohli začít</span><span class="sxs-lookup"><span data-stu-id="92efe-129">Here's what you need to get started</span></span>

* <span data-ttu-id="92efe-130">Předplatné služby Azure pomocí účtu, integrace</span><span class="sxs-lookup"><span data-stu-id="92efe-130">An Azure subscription with an integration account</span></span>
* <span data-ttu-id="92efe-131">Visual Studio 2015 k vytvoření mapy a schémata</span><span class="sxs-lookup"><span data-stu-id="92efe-131">Visual Studio 2015 to create maps and schemas</span></span>
* [<span data-ttu-id="92efe-132">Microsoft Azure Logic Apps Enterprise integrace nástrojů pro Visual Studio 2015 2.0</span><span class="sxs-lookup"><span data-stu-id="92efe-132">Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0</span></span>](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a><span data-ttu-id="92efe-133">Vyzkoušet</span><span class="sxs-lookup"><span data-stu-id="92efe-133">Try it now</span></span>

<span data-ttu-id="92efe-134">[Nasazení plně funkční ukázka AS2 odesílání a příjem aplikace logiky](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) používající funkce B2B pro Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="92efe-134">[Deploy a fully operational sample AS2 send & receive logic app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) that uses the B2B features for Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="92efe-135">Další informace</span><span class="sxs-lookup"><span data-stu-id="92efe-135">Learn more</span></span>
* [<span data-ttu-id="92efe-136">Smlouvy</span><span class="sxs-lookup"><span data-stu-id="92efe-136">Agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")
* [<span data-ttu-id="92efe-137">Obchodní na obchodní scénáře (B2B)</span><span class="sxs-lookup"><span data-stu-id="92efe-137">Business to Business (B2B) scenarios</span></span>](../logic-apps/logic-apps-enterprise-integration-b2b.md "informace o vytváření aplikací logiky s funkcemi B2B")  
* [<span data-ttu-id="92efe-138">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="92efe-138">Certificates</span></span>](logic-apps-enterprise-integration-certificates.md "Další informace o integraci certifikáty pro rozlehlé sítě")
* [<span data-ttu-id="92efe-139">Plochý soubor kódování a dekódování</span><span class="sxs-lookup"><span data-stu-id="92efe-139">Flat file encoding/decoding</span></span>](logic-apps-enterprise-integration-flatfile.md "zjistěte, jak ke kódování a dekódování obsah plochý soubor")  
* [<span data-ttu-id="92efe-140">Účty pro integraci</span><span class="sxs-lookup"><span data-stu-id="92efe-140">Integration accounts</span></span>](../logic-apps/logic-apps-enterprise-integration-accounts.md "Další informace o účty pro integraci")
* [<span data-ttu-id="92efe-141">Mapuje</span><span class="sxs-lookup"><span data-stu-id="92efe-141">Maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")
* [<span data-ttu-id="92efe-142">Partneři</span><span class="sxs-lookup"><span data-stu-id="92efe-142">Partners</span></span>](logic-apps-enterprise-integration-partners.md "Další informace o partnery integrace enterprise")
* [<span data-ttu-id="92efe-143">Schémata</span><span class="sxs-lookup"><span data-stu-id="92efe-143">Schemas</span></span>](logic-apps-enterprise-integration-schemas.md "Další informace o integraci schémata enterprise")
* [<span data-ttu-id="92efe-144">Ověření XML zprávy</span><span class="sxs-lookup"><span data-stu-id="92efe-144">XML message validation</span></span>](logic-apps-enterprise-integration-xml.md "postup ověření XML zprávy s Logic apps")
* [<span data-ttu-id="92efe-145">Transformace XML</span><span class="sxs-lookup"><span data-stu-id="92efe-145">XML transform</span></span>](logic-apps-enterprise-integration-transform.md "Další informace o enterprise integrace mapy")
* [<span data-ttu-id="92efe-146">Konektory pro integraci</span><span class="sxs-lookup"><span data-stu-id="92efe-146">Enterprise Integration Connectors</span></span>](../connectors/apis-list.md "Další informace o konektory pro integraci pack")
* [<span data-ttu-id="92efe-147">Integrace účet Metadata</span><span class="sxs-lookup"><span data-stu-id="92efe-147">Integration Account Metadata</span></span>](../logic-apps/logic-apps-enterprise-integration-metadata.md "Další informace o integraci účet metadat")
* [<span data-ttu-id="92efe-148">Sledování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="92efe-148">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md "Další informace o sledování zpráv B2B")
* [<span data-ttu-id="92efe-149">Sledování zpráv B2B na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="92efe-149">Tracking B2B messages in OMS portal</span></span>](logic-apps-track-b2b-messages-omsportal.md "Další informace o sledování zpráv B2B na portálu OMS")

