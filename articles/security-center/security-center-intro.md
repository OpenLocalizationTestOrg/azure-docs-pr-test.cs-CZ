---
title: "Úvod do Azure Security Center | Microsoft Docs"
description: "Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 8951167213da6ab5341c1ca420353ec625ef5424
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-security-center"></a>Úvod do Azure Security Center
Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce.

> [!NOTE]
> Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>
>

## <a name="what-is-azure-security-center"></a>Co je Azure Security Center?
 Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## <a name="key-capabilities"></a>Klíčové funkce
 Security Center nabízí snadno použitelné a efektivní funkce prevence, zjišťování a reakce na ohrožení, které jsou integrované v Azure. Klíčové funkce:

| Krok | Schopnost |
| --- | --- |
| Prevence |Sleduje stav zabezpečení vašich prostředků Azure. |
| Prevence | Definuje zásady pro předplatné Azure na základě požadavků zabezpečení vaší společnosti, typů aplikací, které používá a citlivosti vašich dat |
| Prevence | Používá doporučení zabezpečení řízená zásadami, aby vedla vlastníky služby procesem implementace potřebných kontrol. |
| Prevence | Rychle nasazuje bezpečnostní služby a zařízení Microsoftu a partnerů. |
| Zjišťování |Automaticky shromažďuje a analyzuje data zabezpečení z vašich prostředků Azure, sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall. |
| Zjišťování | Využívá globální hrozby intelligence z produktů společnosti Microsoft a služeb, Microsoft digitální činů jednotky (přicházejí týmu DCU), Microsoft Security Response Center (MSRC) a externích informačních kanálů. |
| Zjišťování | Používá pokročilou analýzu, včetně machine learningu a analýzy chování. |
| Reakce |Poskytuje incidenty a výstrahy zabezpečení seřazené podle priority. |
| Reakce | Nabízí podrobný náhled na zdroj útoku a zasažené prostředky. |
| Reakce | Navrhuje způsoby, jak aktuální útok zastavit, a pomáhá zabránit budoucím útokům. |

## <a name="introductory-walkthrough"></a>Úvodní prohlídka

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení. Tento dokument není to podrobný průvodce.
>
>

 Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). [Přihlaste se k portálu](https://portal.azure.com). V hlavní nabídce portálu, přejděte k položce **Security Center** nebo vyberte **Security Center** dlaždici, kterou jste dříve připnuli k řídicímu panelu portálu.

![Dlaždice zabezpečení na portálu Azure][1]

Ze služby Security Center můžete nastavovat zásady zabezpečení, sledovat konfigurace zabezpečení a zobrazovat výstrahy zabezpečení.

### <a name="security-policies"></a>Zásady zabezpečení
Můžete definovat zásady pro vaše předplatná Azure podle požadavků zabezpečení vaší společnosti. Můžete je také přizpůsobit typům aplikací, které používáte, nebo citlivosti dat v každém předplatném. Například prostředky používané pro vývoj nebo testování můžou mít jiné požadavky na zabezpečení než ty, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase můžou vyžadovat jinou úroveň zabezpečení.

> [!NOTE]
> Pokud chcete upravit zásadu zabezpečení, musí být správce zabezpečení nebo odběru vlastníka nebo přispěvatele. Další informace o rolích a povolené akce v Centru zabezpečení, najdete v části [oprávnění v Azure Security Center](security-center-permissions.md).
>
>

V okně **Security Center** vyberte dlaždici **Zásady**, kde zobrazíte seznam předplatných a skupin prostředků.   

![Okno Security Center][2]

Na **zásady zabezpečení** okně, vyberte předplatné, abyste zobrazili podrobnosti zásady.

**Shromažďování dat** umožňuje shromažďování dat pro zásady zabezpečení. Povolením získáte následující:

* Denní kontrolu všech podporovaných virtuálních počítačů (VM) pro monitorování zabezpečení a doporučení.
* Shromažďování událostí zabezpečení pro analýzu a zjišťování hrozeb.

> [!NOTE]
> Shromažďování dat je nakonfigurován na úrovni předplatného.
>
>

Vyberte **zásada Zabránění** otevřete **zásada Zabránění** okno. **Zobrazit doporučení pro** umožňuje vybrat ovládacích prvků zabezpečení, které chcete monitorovat a doporučení, které chcete zobrazit podle potřeb zabezpečení prostředků v rámci předplatného.

### <a name="security-recommendations"></a>Doporučení zabezpečení
 Služba Security Center analyzuje stav zabezpečení vašich prostředků Azure, aby identifikovala potenciální ohrožení zabezpečení. Seznam doporučení vás provede procesem konfigurace potřebných kontrol. Příklady obsahují:

* Zřizování antimalwaru, aby se pomohl identifikovat a odebrat škodlivý software
* Konfigurace skupin zabezpečení sítě a pravidla pro řízení přenosu do virtuálních počítačů
* Zřizování firewallů webových aplikací, které pomáhají bránit útokům zaměřeným na vaše webové aplikace
* Nasazení chybějících aktualizací systému
* Adresování konfigurací operačního systému, které neodpovídají doporučeným standardním hodnotám

Seznam doporučení získáte kliknutím na dlaždici **Doporučení**. Kliknutím na jednotlivá doporučení zobrazíte další informace nebo provedete akce vedoucí k vyřešení problému.

![Doporučení zabezpečení v Azure Security Center][5]

### <a name="security-state-of-azure-resources"></a>Stav zabezpečení prostředků Azure
**Prevence** část řídicího panelu ukazuje celkové postavení zabezpečení prostředí podle typů prostředků, včetně virtuálních počítačů, webových aplikací a dalším prostředkům.   

Vyberte typ prostředku v rámci **prevence** zobrazíte další informace, včetně seznamu možných ohrožení zabezpečení, které byly identifikovány. (**Výpočetní** je vybraný v následujícím příkladu.)

![Dlaždice Stav prostředků][6]

### <a name="security-alerts"></a>Výstrahy zabezpečení
 Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall. Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení. Příklady zahrnují zjišťování následujících situací:

* Ohrožené virtuální počítače komunikaci s známé škodlivé IP adresy
* Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows
* Útoky hrubou silou na virtuální počítače
* Výstrahy zabezpečení z integrovaných antimalwarových programů a bran firewall

Kliknutím na dlaždici **Výstrahy zabezpečení** zobrazíte seznam výstrah seřazený podle priority.

![Výstrahy zabezpečení][7]

Výběrem výstrahy zobrazíte další informace o útoku a návrhy na jeho řešení.

![Podrobnosti výstrahy zabezpečení][8]

### <a name="partner-solutions"></a>Partnerská řešení
**Partner solutions** dlaždici umožňuje sledovat na první pohled stav zabezpečení vašich partnerských řešení integrovaných ve vašem předplatném Azure. Security Center zobrazuje výstrahy, které pocházejí z těchto řešení.

Vyberte dlaždici **Partner solutions** (Partnerská řešení). Otevře se okno se seznamem všech připojených partnerských řešení.

![Partnerská řešení][9]

## <a name="get-started"></a>Začínáme
Chcete-li začít využívat Security Center, potřebujete předplatné služby Microsoft Azure. Služba Security Center je povolená s vaším předplatným Azure. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

 Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). Další informace najdete v [dokumentaci k portálu](https://azure.microsoft.com/documentation/services/azure-portal/).

Téma [Začínáme s Azure Security Center](security-center-get-started.md) vás rychle provede komponentami pro monitorování zabezpečení a správu zásad služby Security Center.

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste se seznámili se službou Security Center, jejími klíčovými funkcemi a způsobu zahájení práce. Další informace najdete v následujících zdrojích informací:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
