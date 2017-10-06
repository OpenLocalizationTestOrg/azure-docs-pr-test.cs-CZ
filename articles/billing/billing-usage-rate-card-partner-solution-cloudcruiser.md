---
title: "aaaCloud Cruiser a fakturace integrace rozhraní API aplikace Microsoft Azure | Microsoft Docs"
description: "Poskytuje jedinečný perspektivy z Microsoft Azure Billing partnera Cruiser cloudu v jejich prostředí integraci hello rozhraní API Správce Azure fakturace do svých produktech.  To je obzvláště užitečné pro Azure a cloudu Cruiser zákazníky, kteří se chtějí pomocí nebo pokusu o Cruiser cloudu pro Microsoft Azure Pack."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 02/03/2017
ms.author: mobandyo;sirishap;bryanla
ms.openlocfilehash: 74cc19bdeed26c6684210736caa0cb365e8f8821
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser a fakturace integrace rozhraní API Microsoft Azure
Tento článek popisuje, jak hello informace shromážděné z hello nových Microsoft Azure Billing rozhraní API lze použít v cloudu Cruiser pro pracovní postup náklady simulace a analýzy.

## <a name="azure-ratecard-api"></a>Rozhraní API Azure RateCard
Hello RateCard API poskytuje míra informace z Azure. Po ověření se správnými přihlašovacími údaji hello se můžete dotazovat hello rozhraní API toocollect metadata o hello služeb dostupných v Azure, společně s hello sazby související s vaší nabízejí ID.

Hello následuje ukázková odpověď z rozhraní API hello zobrazující hello ceny pro hello A0 (Windows) instance:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-tooazure-ratecard-api"></a>Cloud tooAzure Cruiser na rozhraní RateCard API
Cruiser cloudu můžete využít rozhraní API RateCard informace hello různými způsoby. V tomto článku ukážeme, jak může být použité toomake IaaS zatížení náklady simulace a analýzy.

toodemonstrate tento případ použití, představte si zatížení několik instancí spuštěných na Microsoft Azure Pack (WAP). cílem Hello je toosimulate tento stejné zatížení v Azure a odhad hello náklady provádění takových migrace. V pořadí toocreate této simulace, existují dvě hlavní úlohy toobe provést:

1. **Import a proces hello služby informace shromážděné z hello RateCard rozhraní API.** Tato úloha provádí také na hello sešity, kde hello extrakce z hello RateCard rozhraní API je nový plán míra tooa transformovaných a publikovaná. Tento nový plán míry se budou používat v hello simulace tooestimate hello Azure ceny.
2. **Normalizuje WAP služby a služby Azure IaaS.** Ve výchozím nastavení, jsou na základě služby WAP u jednotlivých prostředků (procesoru, velikosti paměti, velikost disku atd.) při Azure jsou služby založené na velikost instance (A0, A1, A2 atd.). Tato první úloha můžete provést pomocí modulu ETL Cruiser cloudu, názvem sešity, které tyto prostředky je možné seskupit instance velikostí, podobá tooAzure instance služby.

### <a name="import-data-from-hello-ratecard-api"></a>Umožňuje importovat data z hello RateCard rozhraní API
Sešity Cruiser cloudu poskytují automatizovaný způsob, jak toocollect a proces informace z hello RateCard rozhraní API.  Sešity ETL (extrakce zatížení transformace) umožňují tooconfigure hello kolekce, transformaci a publikování dat do databáze cloudu Cruiser hello.

Každý sešit můžete mít jeden nebo více kolekcí, což vám toocorrelate informace z různých zdrojů toocomplement nebo posílení data o využití hello. Následující dva snímky obrazovky zobrazit jak Hello toocreate nový *kolekce* v existující sešit a import informací do hello *kolekce* z hello RateCard rozhraní API:

![Obrázek 1 – Vytvoření nové kolekce][1]

![Obrázek 2 – importovat data z nové kolekce hello][2]

Po importu hello dat do sešitu hello je možné toocreate několika kroky a procesy zpracování, toomodify a modelu hello data. V tomto příkladu vzhledem k tomu, že jsme zajímá jenom infrastruktura jako služba (IaaS), můžeme použít hello transformace kroky tooremove nepotřebné řádky nebo záznamy, související tooservices než IaaS.

Hello následující snímek obrazovky ukazuje, že kroky transformace hello používá tooprocess hello data shromážděná z rozhraní API RateCard:

![Obrázek 3 - transformace kroky tooprocess shromažďovat data z rozhraní API RateCard][3]

### <a name="defining-new-services-and-rate-plans"></a>Definování nových služeb a rychlost plány
V cloudu Cruiser jsou různé způsoby toodefine služby. Jednu z možností hello je tooimport hello služby z dat využití hello. Tato metoda se obvykle používá při práci s veřejné cloudy, kde jsou již definováni hello služby poskytovatelem hello.

Míra plánování je sada sazby nebo ceny, které můžou být použité toodifferent služby, na základě daty platnosti nebo skupinu zákazníků, mezi další možnosti. Míra plány lze také v cloudu Cruiser toocreate simulace nebo "Citlivostních" scénářů, toounderstand vliv hello celkové náklady na zatížení změny ve službě.

V tomto příkladu použijeme informace o službě hello z hello RateCard API toodefine nových služeb v cloudu Cruiser. V hello stejný způsobem, můžeme použít hello sazby související toohello služeb toocreate nové míra plánování na Cruiser cloudu.

Na konci hello hello transformace procesu je možné toocreate nový krok a publikovat data hello z hello RateCard API jako nové služby a sazby.

![Obrázek 4 – publikování hello data z hello RateCard API jako nové služby a sazby][4]

### <a name="verify-azure-services-and-rates"></a>Ověření služby Azure a sazby
Po publikování služby hello a sazby, můžete ověřit hello seznam importovaných služeb v cloudu Cruiser *služby* karty:

![Obrázek 5 – ověření hello nových služeb][5]

Na hello *plány míra* kartě, můžete zkontrolovat hello nové míra plán nazvaný "AzureSimulation" s hello sazby naimportované z hello RateCard rozhraní API.

![Obrázek 6 – ověření hello nový plán rychlost a přidružené sazby][6]

### <a name="normalize-wap-and-azure-services"></a>Normalizuje WAP a službami Azure
Ve výchozím nastavení WAP poskytuje informace o využití, které jsou založeny na použití hello výpočetní, paměti a síťovým prostředkům. V cloudu Cruiser můžete definovat služeb na základě přímo na hello přidělení nebo Měřené využití těchto prostředků. Například můžete nastavit na základní míru pro každou hodinu využití procesoru nebo poplatků hello GB paměti přidělené tooan instance.

V tomto příkladu v pořadí toocompare náklady mezi WAP a Azure potřebujeme tooaggregate využití hello prostředků na WAP do sad, které pak lze mapovat tooAzure služby. Tato transformace lze snadno implementovat v sešitech hello:

![Obrázek 7: transformace WAP datové toonormalize služby][7]

posledním krokem Hello v sešitu hello je toopublish hello data do databáze cloudu Cruiser hello. V tomto kroku data o využití hello je nyní seskupeny do služby (které mapují toohello služby Azure) a vázané toodefault sazby toocreate hello poplatky.

Po dokončení hello sešitu můžete automatizovat hello zpracování dat hello přidáním úlohu pro hello Plánovač a určením hello četnost a čas pro sešit toorun hello.

![Obrázek 8 - sešitu plánování][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Vytváření sestav pro analýzu náklady simulace pracovního vytížení
Až se shromažďují hello využití a poplatky jsou načtena do databáze cloudu Cruiser hello, jsme využít hello cloudu Cruiser Statistika modulu toocreate hello zatížení náklady simulaci, která jsme požadavky.

V pořadí toodemonstrate tento scénář jsme vytvořili hello následující sestavy:

![Cenově porovnání][9]

Hello nejvyšší graf znázorňuje porovnání náklady, službami, porovnání hello cena spuštěného pracovního vytížení hello pro každou konkrétní službu mezi WAP (tmavý modrá) a Azure (světla modrá).

Hello dolní graf znázorňuje hello stejná data ale rozděleno podle oddělení. Ukazuje to hello náklady pro každé oddělení toorun jejich pracovního vytížení na WAP a Azure, společně s hello rozdíl mezi nimi panelu hello úspory (zelený).

## <a name="azure-usage-api"></a>Rozhraní API Azure využití
### <a name="introduction"></a>Úvod
Microsoft nedávno zavedl hello využití rozhraní API služby Azure, povolení tooprogrammatically vyžádání Odběratelé, kteří v využití dat toogain přehledy o jejich používání. Toto je skvělé zprávy pro zákazníky Cruiser cloudu, které můžete využít výhod hello bohatší datová sada k dispozici prostřednictvím tohoto rozhraní API.

Cruiser cloudu můžete využít hello integrace s hello využití rozhraní API několika způsoby. členitost Hello (informace o hodinové využití) a informace metadat prostředků, které jsou k dispozici prostřednictvím rozhraní API poskytuje hello hello modely toosupport nezbytné datovou sadu flexibilní kompletní přehled nákladů nebo vrácení peněz. 

V tomto kurzu jsme nabídne jeden příklad, jak cloudové Cruiser využívat hello informace o využití rozhraní API. Přesněji řečeno jsme se vytvoření skupiny prostředků v Azure, přiřadit značky pro strukturu hello účet a pak popisují proces hello stahování a zpracování informací značky hello do cloudu Cruiser.

cílem konečné Hello je toobe možné toocreate sestavy jako hello následující jeden a být schopný tooanalyze náklady a spotřeba na základě struktury hello účtu naplněn hello značky.

![Obrázek 10 - sestava s členění pomocí značek][10]

### <a name="microsoft-azure-tags"></a>Značky Microsoft Azure
Hello data k dispozici prostřednictvím hello využití rozhraní API služby Azure zahrnují pouze informace o spotřebě, ale také metadata prostředků, včetně všechny značky, které s ním spojená. Značky poskytují snadný způsob tooorganize vašich prostředků, ale v pořadí toobe efektivní, bude nutné tooensure který:

* Značky jsou správně použité toohello prostředky během zřizování
* Značky jsou správně použít ve struktuře účet hello kompletní přehled nákladů/vracení peněz proces tootie hello využití toohello organizace.

Oba tyto požadavky může být náročné, zejména v případě, že je ruční proces na hello zřízení nebo poplatků straně. Chybným, nesprávnou nebo chybějící i značky jsou častých stížností od zákazníků, když pomocí značek a tyto chyby můžete provést životnosti v hello poplatků straně velmi obtížné.

S hello nové rozhraní API Azure využití, Cruiser cloudu můžete prostředků označování informace pro vyžádání obsahu a pomocí sofistikované nástroje ETL názvem sešity, opravte tyto chyby běžné označování. Prostřednictvím transformaci pomocí regulárních výrazů a korelace data můžete cloudové Cruiser identifikovat nesprávně s příznakem prostředky a použít hello správné značky, zajistíte správné přidružení hello prostředků hello k příjemce hello.

Na hello poplatků straně cloudu Cruiser automatizuje hello procesu kompletní přehled nákladů/vracení peněz a můžete využít hello značky informace tootie hello využití toohello odpovídající příjemce (oddělení, dělení, projektu atd.). Tato automatizace poskytuje obrovské zlepšování a zajistit konzistentní a kontrolovatelný plnících procesu.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Vytvoření skupiny prostředků pomocí značek v Microsoft Azure
Hello první krok v tomto kurzu je toocreate skupiny prostředků v hello portál Azure a pak vytvořit nové značky tooassociate toohello prostředky. V tomto příkladu budeme vytvářet hello následující značky: oddělení, prostředí, vlastník projektu.

Následující snímek obrazovky Hello obsahuje ukázku že přidružené skupiny prostředků s hello značky.

![Obrázek 11 – skupina prostředků se přidružených značek na portálu Azure][11]

dalším krokem Hello je toopull hello informace hello využití rozhraní API do cloudu Cruiser. Hello využití rozhraní API aktuálně poskytuje odpovědi ve formátu JSON. Zde je ukázka hello data načtená:

    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-hello-usage-api-into-cloud-cruiser"></a>Umožňuje importovat data z hello využití rozhraní API do cloudu Cruiser
Sešity Cruiser cloudu poskytují automatizovaný způsob, jak toocollect a proces informace z hello využití rozhraní API. Sešit aplikace ETL (extrakce zatížení transformace) vám umožní tooconfigure hello kolekce, transformaci a publikování dat do databáze cloudu Cruiser hello.

Každý sešit může mít jednu nebo více kolekcí. To vám umožní toocorrelate informace z různých zdrojů toocomplement nebo posílení hello data o využití. V tomto příkladu vytvoříme nový list v sešitu šablony Azure hello (*UsageAPI)* a nastavte novou *kolekce* tooimport informace z hello využití rozhraní API.

![Obrázek 3 - importovat do listu UsageAPI hello data o využití rozhraní API][12]

Všimněte si, že tento sešit už obsahuje jiné listů tooimport služeb z Azure (*ImportServices*) a zpracování informací o spotřebě hello z hello fakturace rozhraní API (*PublishData*).

Vedle využijeme hello využití rozhraní API toopopulate hello *UsageAPI* list a correlate hello informace s hello datům o spotřebě z hello fakturace rozhraní API na hello *PublishData* listu.

### <a name="processing-hello-tag-information-from-hello-usage-api"></a>Zpracování hello značky informací z hello využití rozhraní API
Po importu hello dat do sešitu hello vytvoříme transformace kroky v hello *UsageAPI* listu v pořadí tooprocess hello informace z hello rozhraní API. Prvním krokem je toouse "JSON rozdělení" hello tooextract procesoru značky v jednom poli a potom vytvořit pole pro každý jeden z nich (oddělení, projektů, vlastníka a prostředí).

![Obrázek 4 – vytvořit nové pole hello značky informace][13]

Oznámení hello "Síť" služby chybí značka informace hello (žlutý pole), ale nemůžeme ověřit, že je součástí hello stejnou skupinu prostředků prohlížením hello *ResourceGroupName* pole. Vzhledem k tomu, že máme hello značky pro hello další prostředky z této skupiny prostředků, můžeme použít tento hello tooapply informace, chybí prostředek toothis značky později v procesu hello.

Hello dalším krokem je toocreate a vyhledávací tabulky přiřadit hello informace z hello značky toohello *ResourceGroupName*. Tento vyhledávací tabulky se budou používat v hello další krok tooenrich hello datům o spotřebě s informacemi o značky.

### <a name="adding-hello-tag-information-toohello-consumption-data"></a>Přidání hello značky informace toohello spotřeby dat
Nyní jsme přejít toohello *PublishData* seznamu vlastností, které procesy hello informací o spotřebě z hello fakturace rozhraní API a přidat hello pole extrahovat z hello značky. Tento proces se provádí prohlížením hello vyhledávací tabulky vytvořili v předchozím kroku hello pomocí hello *ResourceGroupName* jako hello klíč k vyhledávání hello.

![Obrázek 5 – naplnění struktura účtu hello hello informace z hledání hello][14]

Všimněte si, že byly použity hello odpovídající účet struktura pole pro službu "Síť" hello, opravě hello problém s hello chybí značky. Můžeme také naplněny hello účet struktura pole pro prostředky než naše cíle skupinu prostředků s "Ostatní", v pořadí toodifferentiate je na hello sestavy.

Nyní potřebujeme jen tooadd krok toopublish hello využití dat. Během tohoto kroku bude hello odpovídající sazby u každé služby definované v našich míra plánování informace o využití toohello použité, s hello výsledné poplatků načíst do databáze hello.

nejlepší část Hello je, že máte jenom toogo na tomto procesu jednou. Po dokončení hello sešitu stačí tooadd ho toohello Plánovač a spustí hodinové nebo denní v hello naplánovaný čas. Potom ji stačí vytvoření nové sestavy, nebo přizpůsobení existujících, v pořadí tooanalyze hello data tooget smysluplné přehledy z použití cloudové.

### <a name="next-steps"></a>Další kroky
* Pro podrobné pokyny k vytvoření cloudové Cruiser sešitů a sestav, naleznete je online tooCloud Cruiser [dokumentace](http://docs.cloudcruiser.com/) (vyžaduje se platné přihlášení).  Další informace o cloudu Cruiser, obraťte se na [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* V tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](billing-usage-rate-card-overview.md) přehled hello využití prostředků Azure a rozhraní API RateCard.
* Podívejte se na hello [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) Další informace o obou rozhraní API, které jsou součástí hello sadu rozhraní API poskytovaných hello Azure Resource Manager.
* Pokud chcete toodive přímo do hello ukázkový kód, podívejte se na naše Microsoft Azure Billing API ukázky kódu na [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Další informace
* V tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md) článku toolearn více informací o hello Azure Resource Manager.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Obrázek 1 – Vytvoření nové kolekce"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Obrázek 2 – importovat data z nové kolekce hello"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Obrázek 3 - transformace kroky tooprocess shromažďovat data z rozhraní API RateCard"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Obrázek 4 – publikování hello data z hello RateCard API jako nové služby a sazby"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Obrázek 5 – ověření hello nových služeb"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Obrázek 6 – ověření hello nový plán rychlost a přidružené sazby"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Obrázek 7: transformace WAP datové toonormalize služby"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Obrázek 8 - sešitu plánování"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Obrázek 9 – ukázková sestava pro scénář porovnání náklady zatížení hello"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Obrázek 10 - sestava s členění pomocí značek"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Obrázek 11 – skupina prostředků se přidružených značek na portálu Azure"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Obrázek 12 - importovat do listu UsageAPI hello data o využití rozhraní API"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Obrázek 13 – vytvořit nové pole hello značky informace"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Obrázek 14 - naplnění struktura účtu hello hello informace z hledání hello"
