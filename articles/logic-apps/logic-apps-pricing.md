---
title: aaaPricing & fakturace - Azure Logic Apps | Microsoft Docs
description: "Zjistěte, jak funguje pro Azure Logic Apps ceny a fakturace."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: fa10cbbf7657afba7fadb7c76817b7a5e4af7b42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-pricing-model"></a>Cenový model Logic Apps
Azure Logic Apps můžete tooscale a spouštět pracovní postupy integrace v cloudu hello.  Následují informace o fakturaci a cenách plány pro Logic Apps hello.
## <a name="consumption-pricing"></a>Spotřeba – ceny
Nově vytvořený Logic Apps použití plánu spotřeby. S hello Logic Apps spotřeba cenový model platíte jenom se používá.  Aplikace logiky nejsou omezeny při použití plánu spotřeby.
Všechny akce provést při spuštění instance aplikace logiky jsou měření podle objemu.
### <a name="what-are-action-executions"></a>Jaké jsou spuštěních akce?
Každý krok v definici aplikace logiky je, že akce, která zahrnuje aktivačních událostí, řízení toku kroky jako podmínky, oborů, pro každý smyčky provést až smyčky, volá tooconnectors a volání toonative akce.
Aktivační události jsou zvláštní akce, které jsou navržené tooinstantiate novou instanci aplikace logiky, když dojde k určité události.  Existuje několik různých chování pro aktivační události, které mohou ovlivnit jak monitorované aplikace logiky hello.
* **Aktivační událost dotazování** – této aktivační události průběžně dotazuje koncový bod, dokud neobdrží zprávu, která splňuje kritéria hello pro vytvoření instance aplikace logiky.  interval dotazování Hello se dá nakonfigurovat v aktivační události hello v hello návrhář aplikace logiky.  Každý požadavek dotazování i v případě, že ji nebude vytvořit instanci aplikace logiky, počítá jako spuštění akce.
* **Aktivační události Webhooku** – této aktivační události čeká toosend klienta se požadavek na konkrétní koncový bod.  Každý požadavek odeslán, že koncový bod toohello webhooku se počítá jako spuštění akce. Hello požadavku a hello aktivační události Webhooku protokolu HTTP jsou obě aktivační události webhooku.
* **Aktivační událost opakování** – této aktivační události vytvoří instanci aplikace logiky hello podle interval opakování hello nakonfigurované v aktivační události hello.  Aktivační událost opakování například může být nakonfigurované toorun každé tři dny nebo i každou minutu.

Spuštění aktivační události si můžete prohlédnout ve okna prostředků aplikace logiky hello v hello část historie aktivační události.

Všechny akce, které byly provedeny, jestli byly provedené úspěšně nebo neúspěšně se měří jako spuštění akce.  Akce, které byly přeskočeny z důvodu nebyla splněna podmínka tooa nebo akce, které nebylo provést, protože aplikace logiky hello ukončeno před dokončením se počítá jako spuštění akce.

Akce provést v rámci smyčky, se počítají za iteraci smyčky hello.  Například jedna akce v pro každou smyčku iterace v rámci 10 položek seznamu, se počítají jako hello počet položek v seznamu hello (10) vynásobená hello několik akcí ve smyčce hello (1) a jeden pro zahájení hello hello smyčky , který v tomto příkladu by (10 * 1) + 1 = 11 spuštění akce.
Zakázané Logic Apps nemohou mít nové instance vytvořené a proto při zakázáno, není účtován.  Mějte na paměti, že po zakázání aplikace logiky může trvat nějakou dobu pro hello instancí tooquiesce před úplně zakázaná.
### <a name="integration-account-usage"></a>Používání účtů integrace
Součástí na základě spotřeby využití je [integrace účet](logic-apps-enterprise-integration-create-integration-account.md) pro zkoumání, vývoj a umožní vám toouse hello pro účely testování [B2B/EDI](logic-apps-enterprise-integration-b2b.md) a [zpracování kódu XML](logic-apps-enterprise-integration-xml.md)funkce Logic Apps bez dalších poplatků. Je možné toocreate maximálně jeden účet na oblast a uložení too10 smlouvy a 25 mapy. Schémata, certifikáty a partnery mít žádná omezení a můžete nahrát tolik, jako je třeba.

Kromě toho toohello zahrnutí účty pro integraci s spotřebou, můžete také vytvořit účty pro standardní integraci bez těchto omezení a s naše smlouva SLA standardní logiku aplikace. Další informace najdete v tématu [Azure ceny](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>Plány služby App Service
Aplikace logiky vytvořili odkazující plán služby App Service pokračuje toobehave jako před. V závislosti na zvolené hello plán jsou omezené po hello předepsaných denní spuštěních se překročí, ale se účtují pomocí měření provádění akce hello.
EA zákazníky, kteří mají plánu služby App Service v jejich odběr, který nemá toobe explicitně přidružené hello aplikace logiky, získat výhody počty hello zahrnuty.  Například pokud máte standardní plán služby App Service předplatného EA a aplikace logiky v hello stejného předplatného, pak vám není účtován 10 000 akce spuštěních za den (viz následující tabulka). 

Plány služby App Service a jejich denní spuštěních povolené akce:
|  | Uvolněte a sdílet nebo základní | Standard | Premium |
| --- | --- | --- | --- |
| Akce spuštěních za den |200 |10 000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-tooconsumption"></a>Převod plánu služby App Service – ceny tooConsumption
toochange aplikace logiky, která má plán služby App Service přidruženo model spotřeby tooa, odeberte hello odkaz toohello plán služby App Service v definici aplikace logiky hello.  Tato změna se provádí pomocí rutiny prostředí PowerShell tooa volání:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Ceny
Podrobnosti o cenách najdete v části [ceny aplikace logiky](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Další kroky
* [Přehled Logic Apps][whatis]
* [Vytvoření první aplikace logiky][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

