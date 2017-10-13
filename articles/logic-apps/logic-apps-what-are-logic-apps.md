---
title: "Propojení aplikací a integrace dat s pracovními postupy – Azure Logic Apps | Dokumentace Microsoftu"
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
ms.openlocfilehash: 59d35852d6c703f3c96089a8bf426b57660441a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-logic-apps"></a>Co jsou Logic Apps?
Logic Apps umožňují zjednodušení a implementaci škálovatelných integrací a pracovních postupů v cloudu. Nabízejí vizuálního návrháře pro modelování a automatizaci procesů jako řadu kroků, které nazýváme pracovním postupem.  Existuje mnoho cloudových i místních [konektorů](../connectors/apis-list.md), které můžete rychle integrovat do všech služeb a protokolů.  Aplikace logiky začíná triggerem (například „Když je přidán účet do aplikace Dynamics CRM“) a po jeho spuštění může začít mnoho kombinací akcí, převodů a podmíněné logiky.

Mezi výhody používání Logic Apps patří:  

* úspora času, protože se navrhují komplexní procesy pomocí snadno použitelných návrhových nástrojů,
* hladká implementace vzorů a pracovních postupů, které by jinak bylo obtížné v kódu implementovat.
* Začínáme rychle pomocí šablon
* Přizpůsobení aplikace logiky pomocí vlastních rozhraní API, kódu a akcí
* Připojení a synchronizace různorodých systémů, místních i v cloudu
* Sestavení ze služeb BizTalk Server, API Management, Azure Functions a Azure Service Bus s prvotřídní podporou integrace

Logic Apps je plně spravovaná iPaaS (integrační platforma jako služba), která vývojářům umožňuje pracovat a nezatěžovat se přitom hostováním sestavených projektů, škálovatelností, dostupností a správou.  Logic Apps se škálují automaticky, aby vyhověly vašim nárokům.

![Návrhář aplikace na základě toku](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Jak už bylo zmíněno, pomocí Logic Apps můžete automatizovat podnikové procesy. Zde je několik příkladů:  

* Přesunutí souborů odeslaných na FTP server do úložiště Azure
* Objednávky procesů a tras ze všech místních i cloudových systémů
* Sledujte všechny tweety týkající se určitého tématu, analyzujte myšlenku a vytvořte si upozornění a úkoly k položkám, které vyžadují další sledování.

Podobné scénáře je možné konfigurovat ve všech vizuálních návrhářích bez nutnosti napsat jediný řádek kódu. Začněte [vytvářet své aplikace logiky hned teď][create].  Jakmile ji napíšete, můžete aplikaci logiky [rychle nasadit a překonfigurovat](../logic-apps/logic-apps-create-deploy-template.md) pro různá prostředí a oblasti.

## <a name="why-logic-apps"></a>Proč Logic Apps?
Logic Apps přinášejí do prostoru podnikových integrací rychlost a škálovatelnost.  Snadné používání návrháře, řada dostupných triggerů a akcí a výkonné nástroje pro správu výrazně zjednodušují centralizaci rozhraní API.  Firmy se vydávají směrem k digitalizaci a Logic Apps jim na této cestě umožňují propojit starší systémy s těmi nejmodernějšími.

Navíc můžete pomocí [účtu Enterprise Integration][biztalk] škálovat své integrační scénáře s využitím [zasílání zpráv XML][xml], [správy obchodních partnerů][tpm] a dalších funkcí.

* **Snadno použitelné návrhové nástroje** – Logic Apps je možné od začátku do konce navrhovat v prohlížeči nebo pomocí nástrojů sady Visual Studio. Začněte s triggerem – z jednoduchého plánu až po vytvoření problému v GitHubu. Pak pomocí rozsáhlé galerie konektorů můžete nastavit orchestraci libovolného počtu akcí.
* **Snadné připojení rozhraní API** – I složité úkoly, které je možné snadno popsat, je těžké implementovat v kódu. Díky Logic Apps je propojení různorodých systémů jednoduché. Chcete propojit své cloudové marketingové řešení s lokálním účetním systémem? Chcete centralizovat zasílání zpráv mezi rozhraními API a systémy se sběrnicí Enterprise Service Bus? Aplikace logiky jsou ten nejrychlejší, nejspolehlivější způsob, jak dodat řešení těchto problémů.
* **Rychlý začátek pomocí šablon** – Abychom vám pomohli začít, poskytujeme [galerii šablon][templates], které umožňují rychle vytvářet některá běžná řešení. Od pokročilých řešení B2B po jednoduché připojení SaaS, a dokonce i pár šablon vytvořených jen tak „pro zábavu“ – galerie je prostě nejrychlejší způsob, jak začít využívat sílu Logic Apps.
* **Integrovaná rozšiřitelnost** – Nevidíte konektor, který potřebujete? Logic Apps jsou navrženy pro práci s vaším vlastním rozhraním API a kódem. Můžete snadno vytvořit vlastní aplikace API, které použijte jako vlastní konektor, nebo můžete volat [Azure Functions](https://functions.azure.com) a spustit fragmenty kódu na vyžádání. 
* **Skutečně efektivní integrace** – Začněte lehce a řiďte svůj růst podle toho, jak se vyvíjí vaše potřeby. Služba Logic Apps dokáže snadno využít síly BizTalku. BizTalk je špičkové integrační řešení od Microsoftu, které umožňuje profesionálům v oblasti integrace vytvářet řešení, která potřebují. Další informace o [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Koncepty Logic Apps
Následující pojmy představují některé z klíčových součástí, které dohromady utváří prostředí Logic Apps. 

* **Pracovní postup** – Logic Apps nabízí grafický způsob modelování firemních procesů jako posloupnosti kroků, tedy jako pracovní postup.
* **Spravované konektory** – Aplikace logiky potřebují přístup k datům a službám. Spravované konektory se vytvářejí speciálně k tomu, aby vám pomohly s připojováním k datům a prací s nimi. Podívejte se na seznam konektorů, které jsou nyní dostupné mezi [spravovanými konektory][managedapis].
* **Triggery** – Některé spravované konektory se mohou chovat i jako trigger. Trigger spouští novou instanci pracovního postupu na základě konkrétní události, například doručení e-mailu nebo změny v účtu Azure Storage.
* **Akce** – Každému kroku, který je proveden po triggeru v pracovním postupu, se říká akce. Každá akce je obvykle namapovaná na operaci ve spravovaném konektoru nebo ve vlastních aplikacích API.
* **Enterprise Integration Pack** – Služba Logic Apps zahrnuje možnosti z BizTalku a nabízí tak pokročilejší scénáře integrace. BizTalk je špičková platforma pro integraci od Microsoftu. Konektory Enterprise Integration Pack umožňují snadno zahrnout ověřování, transformaci a další funkce do pracovních postupů Logic App.

## <a name="getting-started"></a>Začínáme
* Pokud chcete začít s Logic Apps, postupujte podle kurzu [vytvoření aplikace logiky][create].  
* [Zobrazení běžných příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md).
* [S Logic Apps můžete automatizovat firemní procesy](http://channel9.msdn.com/Events/Build/2016/T694). 
* [Zjistěte, jak integrovat své systémy s Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462).

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-web-overview.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md
