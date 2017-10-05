---
title: "Použití Microsoft Azure a Cloudyn RateCard rozhraní API povolit, aby se zajistil ITFM pro zákazníky | Microsoft Docs"
description: "Poskytuje jedinečný perspektivy z Microsoft Azure Billing partnera Cloudyn, v jejich prostředí integrace rozhraní API Azure fakturace do svých produktech.  To je obzvláště užitečné pro Azure a Cloudyn zákazníky, kteří se chtějí pomocí nebo pokusu o Cloudyn pro služby Azure."
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
ms.openlocfilehash: fac0ee2e9cbc87c8b3d04675551bba61f7a532b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Použití Microsoft Azure a rozhraní API RateCard povolit Cloudyn zajistit ITFM pro zákazníky
Cloudyn, vývoj partnera společnosti Microsoft a přední poskytovatel možnosti správy cloudu, jste vybrali pro privátní náhled nové rozhraní API RateCard a využití prostředků Microsoft Azure.  Využití rozhraní API poskytuje přístup k datům o spotřebě odhadované Azure pro předplatné. Rozhraní API RateCard poskytuje kompletní informace o cenách všech služeb Azure, pro zákazníky - Enterprise Agreement EA. Integrované společně, tato rozhraní API poskytují základ úplné informace pro vstup do IT finanční správy (ITFM) nástroje, jako třeba Cloudyn.

## <a name="introduction"></a>Úvod
Takzvané "vynásobením" data z rozhraní API pro použití s daty z rozhraní API RateCard (využití [jednotky] cena [$unit] = podrobné informace o použití a náklady) vytvoří nejvíce granulární, přesnost a spolehlivá fakturační informace k dispozici pro Azure ještě dnes.

![Přehled ITFM][1]

Využívání těchto rozhraní API obsahuje klíče informace o využití a náklady, což Cloudyn k analýze účtů zákazníků způsobem jednoduchý, programová a provádět různé úlohy ITFM svým zákazníkům zákazníků.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integrace Cloudyn s RateCard a využití rozhraní API
Rozhraní API RateCard vyžaduje několik vstupní parametry – například informace o oblasti, měny a národní prostředí – ale nejdůležitějším je OfferDurableID, která určuje, že typ nabídky zákazník Azure používá (průběžné platby, starší verze 6 a 12 měsíců závazků plány MSDN nabízí, MPN nabídky, propagační nabídky a jiné). OfferDurableID lze nalézt v [využití Azure a fakturace portál](https://account.windowsazure.com/Subscriptions), v části "Nabízejí ID" pro dané předplatné.

Při registraci [Cloudyn pro Azure](https://www.cloudyn.com/microsoft-azure/) služeb zákazníkům můžete přidat jejich OfferDurableID kód, který umožňuje Cloudyn načítat jejich relevantní informace o cenách prostřednictvím rozhraní API RateCard.  Jeden naleznete informace o různých typech nabízí [Microsoft Azure nabízí podrobnosti](https://azure.microsoft.com/support/legal/offer-details/) stránky.

![Přehled modul Cloudyn ITFM][2]

Cloudyn používá využití i rozhraní API RateCard, kromě rozhraní API pro výkonu služby Azure, chcete-li vytvořit další vrstvy vizualizace, analýzy, výstrahy, vytváření sestav, náklady na správu a řešitelné doporučení, poskytuje spolehlivé Azure zákazníků Enterprise cloud ITFM nástroj.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Případy použití Cloudyn ITFM ve využití a rozhraní API RateCard integrace povoleno
Případy ve využití povoleno použití běžné ITFM Cloudyn a rozhraní API RateCard zahrnují:

* **Analýza nákladů** -umožňuje cloudové náklady rozděleno na všechny nativní identifikační dimenze (zprostředkovatele, služby, účet, oblast atd.). Použití Azure a rozhraní API RateCard vytvořit jednoduchý úkol, tím, že poskytuje nejvíc granulární rozpis využití a náklady dat za účet, který je pak seskupené filtrovaná podle Cloudyn a uživateli v grafické nebo tabulkové podobě.

![Náklady Analysis výsečového grafu][3]

* **Náklady přidělení 360** -umožňuje finance a IT manažerům k odhalení rozpis skutečné náklady, ovladačů a trendy jejich nasazení cloudu. Dále umožňuje správcům nasazení výdaje snadno přidružit organizační jednotky, oddělení, oblasti a informace, poskytuje nebývalá přehledy cloudové náklady a usnadnění účtovat enterprise a showbacks. Použití Azure a rozhraní API RateCard slouží jako vstup na Cloudyn náklady přidělení modul, který doplňuje definováním metody a obchodní logiku pro přidělování prostředků vyžadující nebo untaggable rozhraní API.

![Přidělení nákladů 360 grafu][4]

* **Nastavení velikosti nákladově efektivní** -poskytuje optimalizaci velikosti doporučení pro nedostatečně virtuální počítače, proto snižuje výdaje zákazníka na příliš velký nebo přepsání zřízené počítače. Dělá to tak prověřením procesor virtuálního počítače a metriky paměti RAM (přes rozhraní API výkonu), čas spuštění (přes rozhraní API služby využití) a náklady (přes rozhraní API RateCard). Cloudyn pak poskytuje optimalizaci velikosti doporučení na základě nedostatečně prostředků procesoru nebo paměti RAM (výkon) a vypočte úspory vynásobením delta ceny (RateCard) mezi virtuálními počítači pomocí skutečný čas využití (využití) nedostatečně počítač.

![Nákladově efektivní změna velikosti][5]

* **Cloud portování doporučení** -poskytuje finanční Rady v cloudu portování. Prozkoumá uživatele aktuální nákladů na cloudové prostředky, které jsou nasazeny na hlavní cloudu dodavatele a porovná ho náklady na ekvivalentní nasazení v Azure. Pak poskytuje podrobné, za prostředky, finančně na základě portování doporučení do Azure. Po vyhodnocení ekvivalentní nasazení na Azure (podle výkonu metriky a uživatelské předvolby) vyžadují, Cloudyn používá rozhraní API RateCard k vyhodnocení náklady na ekvivalentní nasazení v Azure.
* **Sestavy pro zvýšení výkonu** -ve výkonu Azure API povoleno, tyto sestavy poskytují řadu funkcí z využití procesoru a paměti RAM optimalizace doporučení. Dole je příklad instance využití sestav, prezentací instance rozpis podle průměrné využití procesoru.

![Sestavy pro zvýšení výkonu][6]

* **Správce kategorie** -výkonná funkce v Cloudyn, která přináší pořadí neuspořádaný cloudové prostředky. Poskytuje uživatelům volnost vytvořit své vlastní jedinečné kategorií (značky) pro efektivní měření a vytváření sestav, který je v souladu s obchodní praktiky. Další uživatelé snadno regulovat a kategorizace nekonzistentní označování (tj. překlepům a dalších nesrovnalostí) a automaticky zjišťovat vyžadující prostředky pro uvedení přesné náklady.

![Správce kategorie][7]

## <a name="video"></a>Video
Zde je krátké video, které ukazuje, jak Azure zákazníků Cloudyn pro Azure a rozhraní API fakturace Azure pomocí získat přehled o svých datech využití platformy Azure.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Cloudyn-Provides-Cloud-ITFM-Tools-Via-Microsoft-Azure-APIs/player]
> 
> 

## <a name="next-steps"></a>Další kroky
* Spustit bezplatný [Cloudyn pro Azure](https://www.cloudyn.com/microsoft-azure/) zkušební verze, které chcete zobrazit, jak můžete získat náklady průhlednost s vlastní sestavy a analýzy pro vaše nasazení cloudu Microsoft Azure.
* V tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](billing-usage-rate-card-overview.md) přehled informací o rozhraní API RateCard a využití prostředků Azure.
* Podívejte se [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) Další informace o obou rozhraní API, které jsou součástí sadu rozhraní API zadaná pomocí Správce prostředků Azure.
* Pokud chcete pusťte do ukázkový kód, podívejte se na naše Microsoft Azure Billing API ukázky kódu na [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Další informace
* Další informace o Microsoft Azure Enterprise Agreement (EA) nabízí, navštivte [licencování Azure pro podnik](https://azure.microsoft.com/pricing/enterprise-agreement/)
* Najdete v článku [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md) článku se dozvíte více o Azure Resource Manager.
* Další informace o sadě nástrojů, které jsou potřebné k usnadnění tím, že cloudu tráví, naleznete v článku Gartner [trhu Průvodce pro finanční správy IT (ITFM) nástroje](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
