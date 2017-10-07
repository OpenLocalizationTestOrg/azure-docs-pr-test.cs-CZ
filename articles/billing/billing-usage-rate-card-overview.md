---
title: "aaaAzure fakturace rozhraní API | Microsoft Docs"
description: "Další informace o využití fakturace Azure a RateCard rozhraní API, které jsou používané tooprovide přehled o využívání prostředků Azure a trendy."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/18/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: b3214996cc3279f76fdc7f0dbd2059c3ae7bb15c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-billing-apis-tooprogrammatically-get-insight-into-your-azure-usage"></a>Použití rozhraní API Správce Azure fakturace tooprogrammatically pohled na použití Azure
Pomocí rozhraní API Správce Azure fakturace toopull využití a prostředků dat do vaší nástrojů pro analýzu upřednostňované data. Hello využití prostředků Azure a rozhraní API RateCard vám může pomoct přesně předpovědět a náklady na správu. Hello rozhraní API jsou implementovány jako poskytovatel prostředků a součástí rodiny hello rozhraní API vystavené hello Azure Resource Manager.  

## <a name="azure-invoice-download-api-preview"></a>Rozhraní API stažení faktury Azure (Preview)
Jednou hello [výslovný souhlas byl dokončení](billing-manage-access.md#opt-in), stažení faktury pomocí verze preview hello [faktury API](/rest/api/billing). Funkce Hello:

* **Řízení přístupu Azure na základě rolí** -konfigurace přístupu zásady na hello [portál Azure](https://portal.azure.com) nebo pomocí [rutin prostředí Azure PowerShell](/powershell/azure/overview) toospecify, které uživatele nebo aplikace můžete získat přístup data o využití toohello předplatné. Volající musí používat standardní tokeny služby Azure Active Directory pro ověřování. Přidejte hello volající tooeither hello fakturace čtečky čtečky, vlastníka nebo přispěvatele role tooget toohello data o využití přístupu pro konkrétní předplatné Azure.
* **Datum filtrování** -použití hello `$filter` parametr tooget všechny faktury hello v obráceném chronologickém pořadí podle hello fakturační období koncové datum. 

> [!NOTE]
> Tato funkce je v první verzi preview a může být předmět toobackward kompatibilní změny. V současné době není k dispozici pro některé nabídky předplatného (EA, CSP AIO není podporována.) a Azure v Německu.

## <a name="azure-resource-usage-api-preview"></a>Rozhraní API (Preview) pro využití prostředků Azure.
Použití hello Azure [API využití prostředků](https://msdn.microsoft.com/library/azure/mt219003) tooget data Odhadované využití platformy Azure. Hello rozhraní API obsahuje:

* **Řízení přístupu Azure na základě rolí** -konfigurace přístupu zásady na hello [portál Azure](https://portal.azure.com) nebo pomocí [rutin prostředí Azure PowerShell](/powershell/azure/overview) toospecify, které uživatele nebo aplikace můžete získat přístup data o využití toohello předplatné. Volající musí používat standardní tokeny služby Azure Active Directory pro ověřování. Přidejte hello volající tooeither hello fakturace čtečky čtečky, vlastníka nebo přispěvatele role tooget toohello data o využití přístupu pro konkrétní předplatné Azure.
* **Hodinové nebo denní agregace** – volající můžete určit, jestli chtějí jejich Azure využití dat každou hodinu intervalů nebo denních intervalů. Výchozí hodnota Hello je denně.
* **Instance metadat (zahrnuje značky prostředku)** – získání podrobností na úrovni instance jako hello identifikátor uri prostředku plně kvalifikovaný (/subscriptions/ {id předplatného} /..), hello informace o skupině prostředků a značky prostředku. Tato metadata vám pomůže deterministicky a prostřednictvím kódu programu přidělit využití podle hello značky pro případy použití jako mezi poplatků.
* **Metadata prostředků** – prostředek podrobnosti, například hello měření název, kategorie měření, měření dílčí kategorie, jednotky a oblast poskytnout hello volajícího lépe porozumět tomu, co se spotřebovala. Také pracujeme tooalign prostředků metadata terminologie napříč hello portálu Azure, Azure použití sdíleného svazku clusteru, EA fakturace sdíleného svazku clusteru a dalších činnostech veřejné toolet vazbu mezi data v prostředí.
* **Použití všech nabízet typy** – data o využití je k dispozici pro všechny typy jako průběžné platby, MSDN, peněžních závazků, peněžního kreditu, který a EA nabízejí.

## <a name="azure-resource-ratecard-api-preview"></a>RateCard prostředků Azure rozhraní API (Preview)
Použití hello [rozhraní API služby Azure prostředků RateCard](https://msdn.microsoft.com/library/azure/mt219005) tooget hello seznam dostupných prostředků Azure a odhadovanou informace o cenách pro jednotlivé. Hello rozhraní API obsahuje:

* **Řízení přístupu Azure na základě rolí** -konfigurace zásad přístupu na hello [portál Azure](https://portal.azure.com) nebo pomocí [rutin prostředí Azure PowerShell](/powershell/azure/overview) toospecify, které uživatele nebo aplikace můžete získat přístup toohello RateCard data. Volající musí používat standardní tokeny služby Azure Active Directory pro ověřování. Přidejte hello volající tooeither hello čtečky, vlastníka nebo přispěvatele role tooget toohello data o využití přístupu pro konkrétní předplatné Azure.
* **Podpora pro průběžné platby, MSDN, peněžních závazků a peněžního kreditu, který nabízí (EA není podporován)** – tato rozhraní API poskytuje Azure míra nabídka úroveň informace.  Hello volající toto rozhraní API musí předat hello nabídka informace tooget prostředků podrobnosti a sazby. Nyní nelze tooprovide EA sazby nám, protože nabízí EA přizpůsobili sazby za registraci. 

## <a name="scenarios"></a>Scénáře
Tady jsou některé hello scénáře, které lze vytvořit pomocí kombinace hello hello využití a hello RateCard rozhraní API:

* **Azure tráví v měsíci hello** -použití hello kombinace hello využití a rozhraní API RateCard tooget lepší přehled o vašem cloudu tráví v měsíci hello. Můžete analyzovat hello každou hodinu a odhadne denních intervalů využití a poplatků.
* **Nastavení výstrah** – pomocí hello využití a hello rozhraní API RateCard tooget odhadované využívání cloud a poplatky a nastavení na základě prostředků nebo peněžní na základě výstrah.
* **Předpovídat faktury** – Get odhadované spotřeby a cloud tráví a použít strojového učení toopredict algoritmy, jaké hello faktury bude na konci hello hello fakturační cyklus.
* **Předběžné spotřeba analýza nákladů** – použít toopredict RateCard API hello kolik vaše faktura by byl očekávané využití při přesunutí tooAzure vaše úlohy. Pokud máte existující úlohy v ostatních cloudů nebo privátní cloudy, můžete také mapovat vašeho využití s tooget Azure sazby hello tráví lepší odhad Azure. Tento odhad poskytuje hello možnost toopivot na nabídku a porovnání a kontrast mezi typy různých nabídka hello nad rámec průběžné platby, jako peněžních závazků a peněžního kreditu. také poskytuje Hello API hello možnost toosee náklady rozdíly podle oblasti a umožňuje vám toodo toohelp analýzy citlivostních náklady provedete rozhodnutí o nasazení.
* **Analýz** -
  
  * Můžete určit, zda je víc úloh nákladově efektivní toorun v jiné oblasti nebo na jinou konfiguraci hello prostředků Azure. Prostředků Azure, které náklady se můžou lišit podle hello oblast Azure, kterou používáte.
  * Můžete také určit, pokud jiný typ nabídky Azure poskytuje lepší rychlost na prostředek služby Azure.
  
## <a name="partner-solutions"></a>Partnerská řešení
[Použití Microsoft Azure a RateCard rozhraní API povolit Cloudyn tooProvide ITFM pro zákazníky](billing-usage-rate-card-partner-solution-cloudyn.md) popisuje činnost integrace hello nabízený partnerem rozhraní API služby Azure fakturace [Cloudyn](https://www.cloudyn.com/microsoft-azure/). Tento článek zmíněn jejich prostředí a zahrnuje video, které ukazuje, jak můžete použít Cloudyn a hello rozhraní API Správce Azure fakturace tooget přehledy z dat využití platformy Azure.

[Cloud Cruiser a fakturace integrace rozhraní API aplikace Microsoft Azure](billing-usage-rate-card-partner-solution-cloudcruiser.md) popisuje, jak [Express Cloud Cruiser pro Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) funguje přímo z portálu Windows Azure Pack (WAP) hello. Obě hello provozní a finanční hlediska hello Microsoft Azure privátní nebo hostované veřejného cloudu, můžete spravovat bezproblémově z jediného uživatelského rozhraní.   

## <a name="next-steps"></a>Další kroky
* Podívejte se na ukázky kódu hello na Githubu:
  * [Ukázka kódu faktury rozhraní API](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Ukázka kódu rozhraní API pro využití](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [Ukázka kódu rozhraní API RateCard](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* toolearn Další informace o hello Azure Resource Manager, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md). 

* Další informace o hello sada nástrojů tráví nezbytné toohelp získat představu o cloudu, najdete v článku Gartner hello [trhu Průvodce pro finanční správy IT (ITFM) nástroje](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

