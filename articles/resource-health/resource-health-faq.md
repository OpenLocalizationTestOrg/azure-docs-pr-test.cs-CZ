---
title: "Azure Resource health – nejčastější dotazy | Microsoft Docs"
description: "Přehled o stavu prostředků Azure"
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 06/01/2016
ms.author: BernardoAMunoz
ms.openlocfilehash: 41522a85cac05304b3ae60c45b48920eefbe8f5c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-health-faq"></a>Azure Resource health – nejčastější dotazy
Přečtěte si odpovědi na časté otázky týkající se stavu prostředků Azure.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
* [Co je Azure Resource Health?](#what-is-azure-resource-health)
* [Co je určená stav prostředku?](#what-is-the-resource-health-intended-for)
* [Kontroluje, co stavu provádí stav prostředku?](#what-health-checks-are-performed-by-resource-health)
* [Co každý stav znamená?](#what-does-each-of-the-health-status-mean)
* [Co znamená Neznámý stav? Je něco špatně s Moje prostředků?](#what-does-the-unknown-status-mean-is-something-wrong-with-my-resource)
* [Jak můžete získat nápovědu k prostředku, který je k dispozici?](#how-can-i-get-help-for-a-resource-that-is-unavailable)
* [Stav prostředku rozlišit mezi nedostupnosti použita problémy v platformě versus něco, co jsme to udělali?](#does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did)
* [Umožňuje integraci stav prostředku s Moje nástroje monitorování?](#can-i-integrate-resource-health-with-my-monitoring-tools)
* [Kde stav prostředků](#where-do-i-find-resource-health)
* [Stav prostředku je k dispozici pro všechny typy prostředků?](#is-resource-health-available-for-all-resource-types)
* [Co mám dělat, když můj prostředků se zobrazuje dostupné, ale I myslet, že není?](#what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not)
* [Stav prostředku je k dispozici pro všechny oblasti Azure?](#is-resource-health-available-for-all-azure-regions)
* [Jak se liší od řídicím panelu stavu služeb nebo oznámení portálu služby Azure stav prostředku?](#how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications)
* [Je nutné aktivovat stav prostředku pro všechny prostředky?](#do-i-need-to-activate-resource-health-for-each-resource)
* [Je potřeba povolit resource health Moje organizace?](#do-we-need-to-enable-resource-health-for-my-organization)
* [Stav prostředku je k dispozici zdarma?](#is-resource-health-available-free-of-charge)
* [Jaké jsou doporučení, které poskytuje stav prostředku?](#what-are-the-recommendations-that-resource-health-provides)


## <a name="what-is-azure-resource-health"></a>Co je Azure resource health?
Resource Health pomáhá při diagnostice a získání podpory v případě, že potíže s Azure ovlivňují vaše prostředky. Informuje o aktuálním a dřívějším stavu prostředků a pomáhá zmírnit problémy. Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.  

## <a name="what-is-the-resource-health-intended-for"></a>Co je určená stav prostředku?
Jakmile se zjistil problém s prostředek, můžete stav prostředku diagnostikou hlavní příčiny. Poskytuje nápovědu ke zmírnění problém a se na technickou podporu, pokud potřebujete další pomoc s problémy služby Azure.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Kontroluje, co stavu provádí stav prostředku?
Stav prostředku provádí různé kontroly na základě [typ prostředku](resource-health-checks-resource-types.md). Těmito kontrolami jsou určené k implementaci tři typy problémů: 
1. Restartovat neplánované události, například neočekávané hostitele
2. Plánované události, jako jsou aktualizace naplánované hostitelský operační systém
3. Události aktivované akcí uživatele, například uživatele restartování virtuálního počítače

## <a name="what-does-each-of-the-health-status-mean"></a>Co každý stav znamená?
Existují tři různé stavu stavy:
1. K dispozici: Nejsou k dispozici žádné známé problémy v Azure platformy, na které by mohly ovlivnit tento prostředek
2. Není k dispozici: Stav prostředku zjistil problémy, které, které mají vliv na prostředek
3. Neznámé: Stav prostředku nelze určit stav prostředku vzhledem k tomu, že byla zastavena, informace o jeho přijetí. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Co znamená Neznámý stav? Je něco špatně s Moje prostředků?
Stav je nastavena na neznámý při zastavení stav prostředku přijímá informace o konkrétní prostředek. Přestože tento stav není spolehlivý údaj o stav prostředku, v případech, kde dochází k potížím, může to znamenat, že existuje problém s Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Jak můžete získat nápovědu k prostředku, který je k dispozici?
Můžete odeslat žádost o podporu v okně prostředků stavu. Není nutné podporu smlouvy se společností Microsoft otevření žádosti o, když není k dispozici prostředek protože události platformy.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Stav prostředku rozlišit mezi nedostupnosti použita problémy v platformě versus něco, co jsme to udělali?
Ano, pokud prostředek není k dispozici, stav prostředku identifikuje hlavní příčinu v rámci jedné z těchto kategorií: 
1.  Akce iniciované uživatelem
2.  Plánované událostí 
3.  Neplánované událostí

Na portálu jsou uvedeny akce iniciované uživatelem pomocí ikony v oznamovací blue, během plánovaných a neplánovaných události jsou uvedeny pomocí červená varovná ikona. Další podrobnosti jsou uvedeny v [přehled stavu prostředků](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Umožňuje integraci stav prostředku s Moje nástroje monitorování?
Stav prostředku je služba navržená tak, aby vám pomohou diagnostikovat a zmírnit problémy služby Azure, které mít vliv na vaše prostředky. I když můžete pomocí rozhraní API stavu prostředku prostřednictvím kódu programu získat stav, doporučujeme že použít metriky k monitorování vašich prostředků. Jakmile se zjistí problém, stav prostředku vám pomůže určit hlavní příčinu a provede vás procesem akce je řešit. Navštivte [Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) Další informace o použití metriky zkontrolujte vaše prostředky.

## <a name="where-do-i-find-resource-health"></a>Kde stav prostředků
Po přihlášení na portál Azure několika způsoby dostanete stav prostředku:
1. Přejděte do prostředku. V levém navigačním panelu, klikněte na tlačítko **stav prostředku**.
2. Přejděte do okna Azure monitorování.  V levém navigačním panelu, klikněte na tlačítko **stav prostředku**.
3. Otevřete **Nápověda a podpora** okno kliknutím na tlačítko Nápověda v pravém horním rohu portálu a potom výběrem **Nápověda a podpora**. Po otevření okna klikněte na tlačítko **stav prostředků**

Stav prostředku rozhraní API můžete také získat informace o stavu vašich prostředků.

## <a name="is-resource-health-available-for-all-resource-types"></a>Stav prostředku je k dispozici pro všechny typy prostředků?
Najdete seznam kontroly stavu a typy prostředků, které jsou podporovány prostřednictvím stav prostředku [sem](resource-health-checks-resource-types.md)

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Co mám dělat, když můj prostředků se zobrazuje dostupné, ale I myslet, že není?"
Při kontrole stavu prostředku, vpravo pod stav můžete kliknout na **sestavy nesprávný stav**. Před odesláním sestavy, máte možnost poskytnout další podrobnosti na proč si myslíte, že aktuální stav je nesprávný.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Stav prostředku je k dispozici pro všechny oblasti Azure? 
Stav prostředku je k dispozici v napříč všech oblastech Azure s výjimkou následující oblasti:
* USA (Gov) – Virginia
* USA (Gov) – Iowa
* US DoD – východ
* US DoD – střed
* Německo – střed
* Německo – severovýchod
* Čína – východ
* Čína – sever

## <a name="how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications"></a>Jak se liší od řídicím panelu stavu služeb nebo oznámení portálu služby Azure stav prostředku?
Informace poskytované stav prostředku je specifičtější než co je poskytováno řídicím panelu stavu služeb Azure.

Zatímco [stavu Azure](https://status.azure.com) a oznámení portálu service je informovat o problémů služby, které ovlivňují širokou škálu zákazníků (například oblasti Azure), stav prostředku zpřístupní podrobnější události, které se vztahují pouze k konkrétní prostředek. Například pokud hostitel neočekávaně restartuje, stav prostředku výstrahy pouze zákazníků, jejichž virtuální počítače běžely na tomto hostiteli.

Je důležité si všimněte si, že abyste si mohli kompletní přehled událostí vaše prostředky, které mají vliv, stav prostředku navíc zobrazí události, které jsou publikovány v oznámení o službách a řídicím panelu stavu služeb.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Je nutné aktivovat stav prostředku pro všechny prostředky?
Ne, informace o stavu není k dispozici pro všechny typy prostředků, k dispozici prostřednictvím stav prostředku. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Je potřeba povolit resource health Moje organizace?
Ne.  Stav prostředků Azure je dostupné na portálu Azure bez požadavků na instalaci.

## <a name="is-resource-health-available-free-of-charge"></a>Stav prostředku je k dispozici zdarma?
Ano.  Stav prostředku Azure je zdarma.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Jaké jsou doporučení, které poskytuje stav prostředku?
Podle toho, stav, stav prostředku poskytuje doporučení s cílem snížit dobu, že stráví řešení potíží. Pro dostupné prostředky dojde k doporučení zaměřená na tom, jak vyřešit nejběžnější problémy zákazníků. Pokud prostředek není k dispozici z důvodu Azure neplánované událost, pak se fokus bude na které jste během a po dokončení procesu obnovení. 

## <a name="next-steps"></a>Další kroky

Podívejte se na tyto prostředky Další informace o stavu prostředků:
-  [Přehled stavu prostředků Azure.](Resource-health-overview.md)
-  [Typy prostředků a kontroly stavu dostupné prostřednictvím služby Azure Resource Health](resource-health-checks-resource-types.md)
