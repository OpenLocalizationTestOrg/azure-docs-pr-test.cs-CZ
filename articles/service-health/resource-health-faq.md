---
title: "aaaAzure Resource Health – nejčastější dotazy | Microsoft Docs"
description: "Přehled o stavu prostředků Azure."
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/05/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: fe29b2df1f9fee4b77d0100c7d872df837dc4b4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-faq"></a>Azure Resource Health – nejčastější dotazy
Zjistěte, zda text hello odpovědi toocommon dotazy týkající se stavu prostředků Azure.

## <a name="what-is-azure-resource-health"></a>Jaký je stav prostředku Azure?
Resource Health pomáhá při diagnostice a získání podpory v případě, že potíže s Azure ovlivňují vaše prostředky. To informující o hello aktuální a starší stav svých prostředků a pomáhá zmírnit problémy. Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.  

## <a name="what-is-hello-resource-health-intended-for"></a>Co je hello stav prostředku určená pro?
Jakmile zjistil problém s prostředek, můžete stav prostředku diagnostikovat hello hlavní příčinu. Pokud potřebujete další pomoc s problémy služby Azure poskytuje nápovědu toomitigate hello problém a se na technickou podporu.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Kontroluje, co stavu provádí stav prostředku?
Stav prostředku provádí různé kontroly podle hello [typ prostředku](resource-health-checks-resource-types.md). Těmito kontrolami jsou tři typy navrženou tooimplement problémy: 
- Restartovat neplánované události, například neočekávané hostitele
- Plánované události, jako jsou aktualizace naplánované hostitelský operační systém
- Události aktivované akcí uživatele, například uživatele restartování virtuálního počítače

## <a name="what-does-each-of-hello-health-status-mean"></a>Co každý hello stav znamená?
Existují tři různé stavu stavy:
- K dispozici: Nejsou k dispozici žádné známé problémy v hello platformy Azure, které by mohly ovlivnit tento prostředek
- Není k dispozici: Stav prostředku zjistil problémy, které, které mají vliv hello prostředků
- Neznámé: Stav prostředku nelze určit hello stavu prostředku vzhledem k tomu, že byla zastavena, informace o jeho přijetí. 

## <a name="what-does-hello-unknown-status-mean-is-something-wrong-with-my-resource"></a>Co hello střední Neznámý stav? Je něco špatně s Moje prostředků?
Dobrý stav je nastavený toounknown, pokud stav prostředku zastaví přijímá informace o konkrétní prostředek. Přestože tento stav není spolehlivý údaj o stavu hello hello prostředku v případech, kde dochází k potížím, může to znamenat, že existuje problém s Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Jak můžete získat nápovědu k prostředku, který je k dispozici?
Můžete odeslat žádost o podporu v okně hello stav prostředku. Není k dispozici prostředek hello nepotřebujete podporu smlouvy s Microsoft tooopen žádost protože události platformy.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Stav prostředku rozlišit mezi nedostupnosti použita problémy v platformě versus něco, co jsme to udělali?
Ano, pokud prostředek není k dispozici, stav prostředku identifikuje hello hlavní příčinu v rámci jedné z těchto kategorií: 
-   Akce iniciované uživatelem
-   Plánované událostí 
-   Neplánované událostí

Hello portálu jsou uvedeny akce iniciované uživatelem pomocí ikony v oznamovací blue, během plánovaných a neplánovaných události jsou uvedeny pomocí červená varovná ikona. Další podrobnosti jsou uvedeny v hello [přehled stavu prostředků](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Umožňuje integraci stav prostředku s Moje nástroje monitorování?
Stav prostředku je služba určená toohelp diagnostikovat a zmírnit problémy služby Azure, které mít vliv na vaše prostředky. I když můžete pomocí rozhraní API stavu prostředku tooprogrammatically hello získat hello stav, doporučujeme, že abyste použili metriky toomonitor vašich prostředků. Jakmile se zjistí problém, stav prostředku vám pomůže určit hlavní příčinu hello a provede vás procesem tooaddress akce je. Navštivte [Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) toolearn informace o použití metriky toocheck vašich prostředků.

## <a name="where-do-i-find-resource-health"></a>Kde stav prostředků
Po přihlášení toohello portál Azure několika způsoby dostanete stav prostředku:
- Přejděte tooyour prostředků. V levém navigačním hello, vyberte **stav prostředků**
- Přejděte toohello Azure monitorování okno.  V levém navigačním hello, vyberte **stav prostředku**.
- Otevřete hello **Nápověda a podpora** okno výběrem hello otazník v hello pravého horního rohu portálu hello a potom vyberete **Nápověda a podpora**. Po otevření okna hello vybrat **stav prostředků**

Můžete také použít hello API stavu prostředku tooobtain informace o stavu hello vašich prostředků.

## <a name="is-resource-health-available-for-all-resource-types"></a>Stav prostředku je k dispozici pro všechny typy prostředků?
Hello seznam kontroly stavu a typy prostředků, které jsou podporovány prostřednictvím stav prostředku naleznete [zde](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Co mám dělat, když můj prostředků se zobrazuje dostupné, ale I myslet, že není?"
Při kontrole stavu hello prostředku, vpravo pod hello stav můžete kliknout na **sestavy nesprávný stav**. Před odesláním hello sestavy, máte možnost hello poskytnout další podrobnosti na proč si myslíte, že aktuální stav hello je nesprávný.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Stav prostředku je k dispozici pro všechny oblasti Azure? 
Stav prostředku je k dispozici v napříč všech oblastech Azure s výjimkou hello následující oblasti:
- USA (Gov) – Virginia
- USA (Gov) – Iowa
- US DoD – východ
- US DoD – střed
- Německo – střed
- Německo – severovýchod
- Čína – východ
- Čína – sever

## <a name="how-is-resource-health-different-from-hello-service-health-dashboard-or-hello-azure-portal-service-notifications"></a>Jak se liší od oznámení portálu služby Azure na řídicím panelu stavu služeb nebo hello hello stav prostředku?
Hello informace poskytované stav prostředku je specifičtější než co je poskytováno hello řídicím panelu stavu služeb Azure.

Zatímco [stavu Azure](https://status.azure.com) a hello portálu služby oznámení vás informují o tom o problémů služby, které ovlivňují širokou škálu zákazníků (například oblasti Azure), stav prostředku poskytuje podrobnější událostí, které jsou relevantní jenom toohello konkrétní prostředek. Například pokud hostitel neočekávaně restartuje, stav prostředku výstrahy pouze zákazníků, jejichž virtuální počítače běžely na tomto hostiteli.

Je důležité toonotice této tooprovide dokončení viditelnost událostí vašich prostředků, prostředků stavu události povrchy publikovali také v oznámení o službách, které mají vliv a hello řídicím panelu stavu služeb.

## <a name="do-i-need-tooactivate-resource-health-for-each-resource"></a>Potřebuji tooactivate stav prostředku pro všechny prostředky?
Ne, informace o stavu není k dispozici pro všechny typy prostředků, k dispozici prostřednictvím stav prostředku. 

## <a name="do-we-need-tooenable-resource-health-for-my-organization"></a>Potřebujeme tooenable stav prostředku pro svou organizaci?
Ne.  Azure Resource Health je dostupné v rámci hello portál Azure bez požadavků na instalaci.

## <a name="is-resource-health-available-free-of-charge"></a>Stav prostředku je k dispozici zdarma?
Ano.  Azure stav prostředku je zdarma.

## <a name="what-are-hello-recommendations-that-resource-health-provides"></a>Jaké jsou hello doporučení, které poskytuje stav prostředku?
Na základě hello stavu, stav prostředku poskytuje doporučení s cílem hello zkrácení doby hello, že stráví řešení potíží. Dostupné prostředky hello doporučení zaměřit na to, jak toosolve hello nejběžnější problémy zákazníci setkávají. Pokud není k dispozici z důvodu hello prostředek tooan Azure neplánované událostí hello fokus bude na které jste během a po hello proces obnovení. 

## <a name="next-steps"></a>Další kroky

Další informace o stavu prostředků:
-  [Přehled Azure Resource Health](Resource-health-overview.md)
-  [Typy prostředků a kontroly stavu dostupné prostřednictvím služby Azure Resource Health](resource-health-checks-resource-types.md)
