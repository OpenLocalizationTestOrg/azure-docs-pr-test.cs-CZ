---
title: "aaaConnect aplikace a data integrovat s pracovními postupy - Azure Logic Apps | Microsoft Docs"
description: "Přečtěte si, jak vytvářet pracovní postupy a automatizovat procesy propojením aplikací a integraci dat pomocí služby Azure Logic Apps."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07765c05-72a6-4169-a8ab-f6420bfbaf07
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: klam
ms.openlocfilehash: 53d4e165bb2205ddd56c1950719389725267ddea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-logic-apps"></a>Co jsou Logic Apps?
Aplikace logiky zadejte způsob toosimplify a implementovat škálovatelné integrace a pracovních postupů v cloudu hello. Poskytuje visual návrháře toomodel a automatizovat váš proces jako sérii kroků, které jsou známé jako pracovní postup.  Existují [mnoho konektory](../connectors/apis-list.md) napříč hello cloudové a místní tooquickly integrovat do různých služby a protokoly.  Aplikace logiky začíná aktivační událost (jako "při přidání účtu tooDynamics CRM') a po pálení můžete začít počet kombinací akcí, převody a logiku podmínku.

výhody Hello pomocí Logic Apps hello následující:  

* Ukládání čas návrhu komplexní procesy používání nástrojů návrhu snadno toounderstand
* Implementace bezproblémově vzory a pracovní postupy, které by jinak byly obtížné tooimplement v kódu
* Začínáme rychle pomocí šablon
* Přizpůsobení aplikace logiky pomocí vlastních rozhraní API, kódu a akcí
* Připojení a synchronizace mezi místními synchronisace různorodých systémů a hello cloudu
* Sestavení ze služeb BizTalk Server, API Management, Azure Functions a Azure Service Bus s prvotřídní podporou integrace

Služba Logic Apps je plně spravovaná iPaaS (integrace platforma jako služba) umožňuje vývojářům není toohave tooworry o vytváření hostování, škálovatelnost, dostupnost dat a správu.  Služba Logic Apps bude škálovat automaticky toomeet vyžádání.

![Návrhář aplikace na základě toku](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Jak už bylo zmíněno, pomocí Logic Apps můžete automatizovat podnikové procesy. Zde je několik příkladů:  

* Přesunutí souborů serveru FTP tooan nahrál do úložiště Azure
* Objednávky procesů a tras ze všech místních i cloudových systémů
* Monitorovat všechny tweety o určité téma, analyzovat hello postojích a vytvářet výstrahy a úlohy pro položky, které vyžadují další nabídka.

Scénáře, jako je to se dá nakonfigurovat vše z vizuálního návrháře hello a bez nutnosti napsat jediný řádek kódu. Začněte [vytvářet své aplikace logiky hned teď][create].  Jakmile ji napíšete, můžete aplikaci logiky [rychle nasadit a překonfigurovat](../logic-apps/logic-apps-create-deploy-template.md) pro různá prostředí a oblasti.

## <a name="why-logic-apps"></a>Proč Logic Apps?
Služba Logic Apps přenese rychlost a škálovatelnost do prostoru integrace enterprise hello.  Hello snadné použití návrháře hello, řadu dostupných triggery a akce a výkonné nástroje pro správu zkontrolujte centralizuje vaše rozhraní API jednodušší než kdy dřív.  Jako podnikům přesunout směrem digitalization, Logic Apps můžete tooconnect starší verze a udržuje náskok systémy společně.

Kromě toho se naše [účet integrace Enterprise] [ biztalk] je možné škálovat scénářům integrace toomature hello výkon [zasílání zpráv XML] [ xml], [Správa obchodních partnerů][tpm]a další.

* **Nástroje pro návrh snadno toouse** -Logic Apps je možné navrženou klient server v prohlížeči hello nebo s nástroji Visual Studio. Spusťte triggerem – od jednoduchého plánu toowhen, je vytvořena potíže Githubu. Potom nastavit orchestraci libovolného počtu akcí pomocí rozsáhlé Galerie konektorů hello.
* **Připojení rozhraní API snadno** – i rozložitelné úkoly, které jsou snadno toodescribe jsou těžko tooimplement v kódu. Služba Logic Apps umožňuje snadno tooconnect různorodých systémů. Chcete tooconnect své cloudové marketingové řešení tooyour místní fakturačních systémů? Chcete toocentralize služby zasílání zpráv na rozhraní API a systémy se podnikové služby Service Bus? Aplikace logiky jsou hello nejrychlejší, nejspolehlivější způsob toodeliver řešení toothese problémy.
* **Rychlý začátek pomocí šablon** -toohelp začnete nabízíme [galerii šablon] [ templates] máte toorapidly vytvářet některá běžná řešení. Od pokročilých řešení B2B toosimple připojení SaaS a dokonce i pár, které jsou právě "pro zábavu" – Galerie hello je nejrychlejší způsob, jak tooget hello začít s hello sílu Logic Apps.
* **Zaručená rozšiřitelnost** -nezobrazí konektor hello potřebujete? Služba Logic Apps je navrženou toowork s vlastní rozhraní API a kódu; můžete snadno vytvořit vlastní toouse aplikace API jako vlastní konektor, nebo volání do [funkce Azure](https://functions.azure.com) tooexecute fragmenty kódu poptávky. 
* **Skutečně efektivní integrace** – Začněte lehce a řiďte svůj růst podle toho, jak se vyvíjí vaše potřeby. Služba Logic Apps dokáže snadno využít síly BizTalku. BizTalk společnosti Microsoft odvětví úvodní integrace řešení tooenable integrace Odborníci v oblasti toobuild hello řešení, které potřebují hello. Další informace o hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Koncepty Logic Apps
Hello Toto jsou některé hello klíče částí, které tvoří hello prostředí Logic Apps. 

* **Pracovní postup** -Logic Apps nabízí grafický způsob toomodel firemních procesů jako řadu kroků nebo pracovního postupu.
* **Spravované konektory** – aplikace logiky potřebují přístup k toodata a službám. Spravovaných konektorů jsou vytvořené speciálně tooaid můžete při připojování tooand práce s daty. Zobrazit hello seznam konektorů, které jsou nyní dostupné v [spravovaných konektorů][managedapis].
* **Triggery** – Některé spravované konektory se mohou chovat i jako trigger. Trigger spouští novou instanci pracovního postupu založeného na konkrétní události, například hello doručení e-mailu nebo změny v účtu úložiště Azure.
* **Akce** -každý krok po hello triggeru ve workflow, se říká akce. Každá akce je obvykle namapována tooan operace na konektoru spravované nebo vlastní aplikace API.
* **Enterprise Integration Pack** – Služba Logic Apps zahrnuje možnosti z BizTalku a nabízí tak pokročilejší scénáře integrace. BizTalk je špičková platforma pro integraci od Microsoftu. konektory Enterprise integračního balíčku Hello umožňují tooeasily zahrnují ověření, transformaci a další v pracovních postupech tooyour aplikace logiky.

## <a name="getting-started"></a>Začínáme
* tooget začít s Logic Apps, postupujte podle hello [vytvoření aplikace logiky] [ create] kurzu.  
* [Zobrazení běžných příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md).
* [S Logic Apps můžete automatizovat firemní procesy](http://channel9.msdn.com/Events/Build/2016/T694). 
* [Zjistěte, jak tooIntegrate vaše systémy s Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md
