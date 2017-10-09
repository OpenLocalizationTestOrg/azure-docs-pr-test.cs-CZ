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
# <a name="overview-b2b-scenarios-and-communication-with-hello-enterprise-integration-pack"></a>Přehled: Scénáře B2B a komunikaci s hello Enterprise integračního balíčku

Pro pracovní postupy business-to-business (B2B) a bezproblémové komunikaci s Azure Logic Apps můžete povolit podnikové scénáře integrace s společnosti Microsoft cloudové řešení, hello Enterprise integračního balíčku. Organizace můžou vyměňovat zprávy elektronicky, i když používají různé protokoly a formáty. Hello pack transformuje různých formátech do formátu, který může organizace systémy interpretovat a zpracovat. Organizace si můžou vyměňovat zprávy přes standardní protokoly, včetně [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), a [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). Také můžete zabezpečit zprávy pomocí šifrování a digitální podpisy.

Pokud jste obeznámeni s BizTalk Server nebo Microsoft Azure BizTalk Services, jsou funkcí pro integraci Enterprise hello snadno toouse, protože jsou podobné jako většina koncepty. Hlavní rozdíl je, že integrace Enterprise používá integrace účty toosimplify hello úložiště a správu artefaktů použitých při komunikaci s B2B. 

Z pohledu architektury hello Enterprise integračního balíčku je založena na "účty integrace". Tyto účty jsou založené na cloudu kontejnery, které ukládají všechny artefaktů, jako je schémat, partnerů, certifikátů, mapy a smlouvy. Můžete použít tyto artefakty toodesign, nasadit a udržovat vaše aplikace B2B a také toobuild B2B pracovní postupy pro logic apps. Ale předtím, než budete moci použít tyto artefakty, je nutné nejprve propojit svou aplikaci logiky tooyour integrace účtu. Poté můžete svou aplikaci logiky přístup k účtu integrace artefakty.

## <a name="why-should-you-use-enterprise-integration"></a>Proč používat integraci podnikových?

* Díky integraci enterprise můžete uložit všechny artefakty na jednom místě, váš účet integrace.
* Můžete vytvářet pracovní postupy B2B a integrovat aplikacím třetích stran software jako služba (SaaS), místní aplikace a vlastních aplikací s použitím modulu Azure Logic Apps hello a všechny jeho konektory.
* Můžete vytvořit vlastní kód pro logic apps, díky Azure functions.

## <a name="how-tooget-started-with-enterprise-integration"></a>Jak tooget pracovat s integrací enterprise?

Můžete vytvářet a spravovat aplikace B2B s hello Enterprise integračního balíčku pomocí návrháře aplikace logiky hello v hello **portál Azure**. Můžete také spravovat aplikace logiky s [prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps prostředí PowerShell témata").

Zde jsou hello hlavní kroky, které je třeba provést před vytvořením aplikace v hello portálu Azure:

![Přehled image](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Jaké jsou některé běžné scénáře?

Integrace Enterprise podporuje tyto oborových standardů:

* EDI - Electronic Data Interchange
* EAI - integraci podnikových aplikací

## <a name="heres-what-you-need-tooget-started"></a>Tady je musíte tooget spuštění

* Předplatné služby Azure pomocí účtu, integrace
* Visual Studio 2015 toocreate mapy a schémata
* [Microsoft Azure Logic Apps Enterprise integrace nástrojů pro Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Vyzkoušet

[Nasazení plně funkční ukázka AS2 odesílání a příjem aplikace logiky](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) používající hello B2B funkce pro Azure Logic Apps.

## <a name="learn-more"></a>Další informace
* [Smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")
* [Obchodní scénáře tooBusiness (B2B)](../logic-apps/logic-apps-enterprise-integration-b2b.md "informace jak toocreate Logic apps s funkcemi B2B")  
* [Certifikáty](logic-apps-enterprise-integration-certificates.md "Další informace o integraci certifikáty pro rozlehlé sítě")
* [Plochý soubor kódování a dekódování](logic-apps-enterprise-integration-flatfile.md "informace jak tooencode a dekódování obsah plochý soubor")  
* [Účty pro integraci](../logic-apps/logic-apps-enterprise-integration-accounts.md "Další informace o účty pro integraci")
* [Mapuje](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")
* [Partneři](logic-apps-enterprise-integration-partners.md "Další informace o partnery integrace enterprise")
* [Schémata](logic-apps-enterprise-integration-schemas.md "Další informace o integraci schémata enterprise")
* [Ověření XML zprávy](logic-apps-enterprise-integration-xml.md "zjistěte, jak toovalidate XML zprávy s Logic apps")
* [Transformace XML](logic-apps-enterprise-integration-transform.md "Další informace o enterprise integrace mapy")
* [Konektory pro integraci](../connectors/apis-list.md "Další informace o konektory pro integraci pack")
* [Integrace účet Metadata](../logic-apps/logic-apps-enterprise-integration-metadata.md "Další informace o integraci účet metadat")
* [Sledování zpráv B2B](logic-apps-monitor-b2b-message.md "Další informace o sledování zpráv B2B")
* [Sledování zpráv B2B na portálu OMS](logic-apps-track-b2b-messages-omsportal.md "Další informace o sledování zpráv B2B na portálu OMS")

