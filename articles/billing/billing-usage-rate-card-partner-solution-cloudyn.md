---
title: "aaaMicrosoft Azure Usage and RateCard rozhraní API povolit Cloudyn tooProvide ITFM pro zákazníky | Microsoft Docs"
description: "Poskytuje jedinečný perspektivy z Microsoft Azure Billing partnera Cloudyn, v jejich prostředí integraci hello rozhraní API Správce Azure fakturace do svých produktech.  To je obzvláště užitečné pro Azure a Cloudyn zákazníky, kteří se chtějí pomocí nebo pokusu o Cloudyn pro služby Azure."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: f1397397-7e92-4c20-9862-ab6b93afefb7
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 02/03/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: e221ac8b8feebb725a1cc669c8143ab829621a8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-tooprovide-itfm-for-customers"></a>Použití Microsoft Azure a RateCard rozhraní API povolit Cloudyn tooProvide ITFM pro zákazníky
Cloudyn, vývoj partnera společnosti Microsoft a přední poskytovatel možnosti správy cloudu, jste vybrali pro privátní náhled hello nové rozhraní API RateCard a využití prostředků Microsoft Azure.  Hello využití rozhraní API poskytuje přístup k datům využití platformy Azure tooestimated pro předplatné. Hello RateCard API poskytuje kompletní informace o cenách všech služeb Azure, pro zákazníky - Enterprise Agreement EA. Integrované společně, tato rozhraní API poskytují základ úplné informace pro vstup do IT finanční správy (ITFM) nástroje, jako třeba Cloudyn.

## <a name="introduction"></a>Úvod
Hello takzvané "násobení" dat z hello využití rozhraní API s daty z hello RateCard rozhraní API (využití [jednotky] cena [$unit] = podrobné informace o použití a náklady) vytvoří hello granulární, přesnost a spolehlivá fakturační informace k dispozici pro Azure ještě dnes.

![Přehled ITFM][1]

Využívání těchto rozhraní API obsahuje klíčové informace o využití a náklady, povolení Cloudyn tooanalyze zákaznických účtů v jednoduché, programové způsobem a tooperform různé úlohy ITFM pro zákazníky, jeho zákazníků.

## <a name="integrating-cloudyn-with-hello-ratecard-and-usage-apis"></a>Integrace Cloudyn s hello RateCard a využití rozhraní API
Hello RateCard API vyžaduje několik vstupní parametry – jako informace o oblasti, měny a národní prostředí – ale hello nejdůležitější, že jedna je OfferDurableID, která určuje, že typ hello Azure nabízení zákazníků hello používá (průběžnými platbami, starší verze 6 a 12 měsíců závazků plány, nabízí MSDN, MPN nabídky, propagační nabídky a dalších). Hello OfferDurableID lze nalézt v hello [využití Azure a fakturace portál](https://account.windowsazure.com/Subscriptions), v části hello "nabízejí" pro hello dané ID předplatného.

Při registraci [Cloudyn pro Azure](https://www.cloudyn.com/microsoft-azure/) služeb zákazníkům můžete přidat jejich OfferDurableID kód, který umožňuje Cloudyn toopull jejich relevantní informace o cenách prostřednictvím hello RateCard rozhraní API.  Informace o různých typech hello nabízí naleznete jeden hello [Microsoft Azure nabízí podrobnosti](https://azure.microsoft.com/support/legal/offer-details/) stránky.

![Přehled modul Cloudyn ITFM][2]

Používá Cloudyn obě hello využití a RateCard rozhraní API, přidání toohello výkonu rozhraní API služby Azure, toocreate další vrstvy vizualizace, analýzy, výstrahy, vytváření sestav, náklady, správy a možné použít doporučení, poskytuje spolehlivé Azure zákazníků Enterprise cloud ITFM nástroj.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Případy použití Cloudyn ITFM ve využití a rozhraní API RateCard integrace povoleno
Případy ve využití povoleno použití běžné ITFM Cloudyn a rozhraní API RateCard zahrnují:

* **Analýza nákladů** -umožňuje cloudu stojí toobe rozdělit tooany nativní Identifikace dimenze (zprostředkovatele, služby, účet, oblast atd.). Hello využití Azure a rozhraní API RateCard vytvořit jednoduchý úkol, tím, že poskytuje hello nejvíce granulární rozdělení dat využití a náklady na účet, který je pak seskupené a filtrovaná podle Cloudyn a zobrazovat toohello uživatele v grafické nebo tabulkovém formátu.

![Náklady Analysis výsečového grafu][3]

* **Náklady přidělení 360** -umožňuje finance a IT manažery toouncover hello skutečné náklady rozpis, ovladačů a trendy jejich nasazení cloudu. Dále umožňuje správci přidružit nasazení výdaje tooeasily s organizační jednotky, oddělení, oblasti a informace, poskytuje nebývalá přehledy cloudové náklady a usnadnění účtovat enterprise a showbacks. Hello využití Azure a rozhraní API RateCard sloužit jako vstupní tooCloudyn náklady přidělení modul, který doplňuje definováním metody a obchodní logiku pro přidělování prostředků vyžadující nebo untaggable hello rozhraní API.

![Přidělení nákladů 360 grafu][4]

* **Nastavení velikosti nákladově efektivní** -poskytuje optimalizaci velikosti doporučení pro nedostatečně virtuální počítače, proto snižuje výdaje hello zákazníka na příliš velký nebo přepsání zřízené počítače. Dělá to tak prověřením procesor virtuálního počítače a metriky paměti RAM (přes rozhraní API výkonu), čas spuštění (přes rozhraní API služby využití) a náklady (přes rozhraní API RateCard). Cloudyn pak poskytuje optimalizaci velikosti doporučení na základě nedostatečně prostředků procesoru nebo paměti RAM (výkon) a vypočte úspory vynásobením delta ceny hello (RateCard) mezi virtuální počítače hello hello skutečný čas využití (využití) hello nedostatečně počítač.

![Nákladově efektivní změna velikosti][5]

* **Cloud portování doporučení** -poskytuje finanční Rady v cloudu portování. Prozkoumá uživatele aktuální nákladů na cloudové prostředky, které jsou nasazeny na hlavní cloudu dodavatele a porovná je toohello náklady na ekvivalentní nasazení v Azure. Pak poskytuje podrobné, za prostředky, finančně na základě portování tooAzure doporučení. Po vyhodnocení hello ekvivalentní nasazení je nutné v Azure (podle výkonu metriky a uživatelské předvolby), používá Cloudyn náklady na rozhraní API RateCard tooevaluate hello hello hello ekvivalentní nasazení v Azure.
* **Sestavy pro zvýšení výkonu** -ve výkonu Azure API povoleno, tyto sestavy poskytují řadu funkcí využití procesoru a paměti RAM toooptimization doporučení. Dole je příklad instance využití sestav, prezentací instance rozpis podle průměrné využití procesoru.

![Sestavy pro zvýšení výkonu][6]

* **Správce kategorie** -výkonná funkce v Cloudyn, která přináší pořadí toounorganized cloudové prostředky. Poskytuje uživatelům hello volnost toocreate vlastní jedinečné kategorií (značky) pro efektivní měření a vytváření sestav, který je v souladu s obchodní praktiky. Další uživatelé snadno regulovat a kategorizace nekonzistentní označování (tj. překlepům a dalších nesrovnalostí) a automaticky zjišťovat vyžadující prostředky pro uvedení přesné náklady.

![Správce kategorie][7]

## <a name="video"></a>Video
Zde je krátké video, které ukazuje, jak Azure zákazníka pomocí Cloudyn pro Azure a hello fakturace rozhraní API Správce Azure, toogain přehled o svých datech využití platformy Azure.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Cloudyn-Provides-Cloud-ITFM-Tools-Via-Microsoft-Azure-APIs/player]
> 
> 

## <a name="next-steps"></a>Další kroky
* Spustit bezplatný [Cloudyn pro Azure](https://www.cloudyn.com/microsoft-azure/) jak můžete získat zkušební toosee náklady průhlednost s vlastní sestavy a analýzy pro vaše nasazení cloudu Microsoft Azure.
* V tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](billing-usage-rate-card-overview.md) přehled hello využití prostředků Azure a rozhraní API RateCard.
* Podívejte se na hello [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) Další informace o obou rozhraní API, které jsou součástí hello sadu rozhraní API poskytovaných hello Azure Resource Manager.
* Pokud chcete toodive přímo do hello ukázkový kód, podívejte se na naše Microsoft Azure Billing API ukázky kódu na [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Další informace
* toolearn Další informace o Microsoft Azure Enterprise Agreement (EA) nabízí, navštivte [licencování v Azure hello Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/)
* V tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md) článku toolearn více informací o hello Azure Resource Manager.
* Další informace o hello sada nástrojů tráví nezbytné toohelp v tím, že cloudu, naleznete v příliš Gartner článku [trhu Průvodce pro finanční správy IT (ITFM) nástroje](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
