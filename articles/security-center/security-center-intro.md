---
title: aaaIntroduction tooAzure Security Center | Microsoft Docs
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
ms.openlocfilehash: 287dbaaa7e2004c522f103595bc316261daf05b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security-center"></a>Úvod tooAzure Security Center
Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce.

> [!NOTE]
> Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data. V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další. Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>
>

## <a name="what-is-azure-security-center"></a>Co je Azure Security Center?
 Security Center pomáhá zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## <a name="key-capabilities"></a>Klíčové funkce
 Security Center nabízí snadno použitelné a efektivní threat prevence, zjišťování a reakce na funkce, které jsou součástí tooAzure. Klíčové funkce:

| Krok | Schopnost |
| --- | --- |
| Prevence |Monitorování hello stav zabezpečení vašich prostředků Azure |
| Prevence | Definuje zásady pro předplatné Azure, na základě požadavků zabezpečení vaší společnosti, hello typy aplikací, které používáte a citlivosti vašich data hello |
| Prevence | Vlastníky služby používá řízená zásadami zabezpečení doporučení tooguide hello procesem implementace potřebných kontrol. |
| Prevence | Rychle nasazuje bezpečnostní služby a zařízení Microsoftu a partnerů. |
| Zjišťování |Automaticky shromažďuje a analyzuje data zabezpečení z vaše prostředky Azure, hello sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall |
| Zjišťování | Využívá globální hrozby intelligence z Microsoft produktů a služeb společnosti Microsoft digitální činů jednotky (DCU), hello hello Microsoft Security Response Center (MSRC) a externích informačních kanálů. |
| Zjišťování | Používá pokročilou analýzu, včetně machine learningu a analýzy chování. |
| Reakce |Poskytuje incidenty a výstrahy zabezpečení seřazené podle priority. |
| Reakce | Nabízí podrobný náhled na zdroj hello hello útoku a zasažené prostředky. |
| Reakce | Navrhuje způsoby toostop hello aktuální útok a pomáhá zabránit budoucím útokům. |

## <a name="introductory-walkthrough"></a>Úvodní prohlídka

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení. Tento dokument není to podrobný průvodce.
>
>

 Security Center je přístupná z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/). [Přihlaste se toohello portál](https://portal.azure.com). Posuňte se v hlavní nabídce portálu hello toohello **Security Center** možnost nebo vyberte hello **Security Center** dlaždici dříve připnuli toohello řídicí panel portálu.

![Dlaždice zabezpečení na portálu Azure][1]

Ze služby Security Center můžete nastavovat zásady zabezpečení, sledovat konfigurace zabezpečení a zobrazovat výstrahy zabezpečení.

### <a name="security-policies"></a>Zásady zabezpečení
Můžete definovat zásady pro vaše předplatná Azure podle tooyour požadavky na zabezpečení společnosti. Vám může také přizpůsobit toohello typy aplikací, které používáte nebo citlivosti toohello hello dat v každém předplatném. Například prostředky používané pro vývoj nebo testování můžou mít jiné požadavky na zabezpečení než ty, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase můžou vyžadovat jinou úroveň zabezpečení.

> [!NOTE]
> toomodify zásady zabezpečení, musíte být vlastníkem nebo přispěvatelem správce zabezpečení nebo hello předplatného. toolearn Další informace o rolí a povolených akcí v Centru zabezpečení, najdete v části [oprávnění v Azure Security Center](security-center-permissions.md).
>
>

Na hello **Security Center** okně, vyberte hello **zásad** dlaždici seznam předplatných a skupin prostředků.   

![Okno Security Center][2]

Na hello **zásady zabezpečení** okně vyberte předplatné tooview hello zásad podrobnosti.

**Shromažďování dat** umožňuje shromažďování dat pro zásady zabezpečení. Povolením získáte následující:

* Denní kontrolu všech podporovaných virtuálních počítačů (VM) pro monitorování zabezpečení a doporučení.
* Shromažďování událostí zabezpečení pro analýzu a zjišťování hrozeb.

> [!NOTE]
> Shromažďování dat je konfigurována na úrovni předplatného hello.
>
>

Vyberte **zásada Zabránění** tooopen hello **zásada Zabránění** okno. **Zobrazit doporučení pro** umožňuje vybrat hello ovládacích prvků zabezpečení, které chcete toomonitor a hello doporučení, které chcete toosee na základě potřeb zabezpečení hello hello prostředků v rámci předplatného hello.

### <a name="security-recommendations"></a>Doporučení zabezpečení
 Security Center analyzuje stav zabezpečení hello vaše prostředky Azure tooidentify potenciální ohrožení zabezpečení. Seznam doporučení vás provede procesem konfigurace potřebných kontrol hello. Příklady obsahují:

* Zřizování antimalwaru, toohelp identifikovat a odebrat škodlivý software
* Konfigurace sítě zabezpečení skupiny a pravidel toocontrol provoz tooVMs
* Zřizování firewallů webových aplikací toohelp bránit proti útokům, které cílí na vaše webové aplikace
* Nasazení chybějících aktualizací systému
* Adresování konfigurací operačního systému, které neodpovídají hello doporučená standardních hodnot

Klikněte na tlačítko hello **doporučení** dlaždici seznam doporučení. Klikněte na každou doporučení tooview Další informace nebo tootake akce tooresolve hello problém.

![Doporučení zabezpečení v Azure Security Center][5]

### <a name="security-state-of-azure-resources"></a>Stav zabezpečení prostředků Azure
Hello **prevence** část řídicího panelu hello hello ukazuje celkové postavení zabezpečení prostředí hello podle typů prostředků, včetně virtuálních počítačů, webových aplikací a dalším prostředkům.   

Vyberte typ prostředku v rámci **prevence** tooview Další informace, včetně seznamu možných ohrožení zabezpečení, které byly identifikovány. (**Výpočetní** je vybraný v následujícím příkladu hello.)

![Dlaždice Stav prostředků][6]

### <a name="security-alerts"></a>Výstrahy zabezpečení
 Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, hello sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall. Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení. Příklady zahrnují zjišťování následujících situací:

* Ohrožené virtuální počítače komunikaci s známé škodlivé IP adresy
* Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows
* Útoky hrubou silou na virtuální počítače
* Výstrahy zabezpečení z integrovaných antimalwarových programů a bran firewall

Kliknutím na hello **výstrahy zabezpečení** dlaždice zobrazí seznam výstrah seřazený podle priority.

![Výstrahy zabezpečení][7]

Výběrem výstrahy zobrazíte další informace o hello útoku a návrhy, jak tooremediate ho.

![Podrobnosti výstrahy zabezpečení][8]

### <a name="partner-solutions"></a>Partnerská řešení
Hello **Partner solutions** dlaždice umožňuje sledovat na první pohled hello zabezpečení stav vašich partnerských řešení integrovaných ve vašem předplatném Azure. Security Center zobrazuje výstrahy, které pocházejí z hello řešení.

Vyberte hello **Partner solutions** dlaždici. Otevře se okno se seznamem všech připojených partnerských řešení.

![Partnerská řešení][9]

## <a name="get-started"></a>Začínáme
tooget začít s Security Center, je nutné tooMicrosoft předplatné Azure. Služba Security Center je povolená s vaším předplatným Azure. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

 Security Center je přístupná z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/). V tématu hello [portálu dokumentace](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn Další.

[Začínáme s Azure Security Center](security-center-get-started.md) vás rychle provede hello monitorování zabezpečení a správu zásad součásti služby Security Center.

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste přináší tooSecurity Center, jejími klíčovými funkcemi a způsobu tooget spuštění. toolearn více, najdete v části hello následující prostředky:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.

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
