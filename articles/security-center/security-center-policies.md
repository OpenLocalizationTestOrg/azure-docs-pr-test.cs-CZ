---
title: "aaaSet zásady zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vám pomůže tooconfigure zásady zabezpečení v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: yurid
ms.openlocfilehash: 59226dd84a1c66a2d8367417060ab10a1ff73848
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Nastavení zásad zabezpečení ve službě Azure Security Center
Tento dokument vám pomůže tooconfigure zásady zabezpečení ve službě Security Center a provede vás tento úkol tooperform potřebné kroky hello.

>[!NOTE] 
>Počínaje časná 2017 června, Security Center používá hello agenta Microsoft Monitoring Agent toocollect a uložit data. V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další. Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>

## <a name="what-are-security-policies"></a>Co jsou zásady zabezpečení?
Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci hello zadaný odběr. Security Center určíte zásady pro vaše předplatná Azure podle tooyour společnost požadavky na zabezpečení a hello typu aplikací nebo citlivosti dat hello v každém předplatném.

Například prostředky používané pro vývoj nebo testování mohou mít jiné požadavky na zabezpečení než prostředky, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase mohou vyžadovat vyšší úroveň zabezpečení. Zásady zabezpečení, které jsou povolené v Azure Security Center doporučení zabezpečení jednotky a monitorování toohelp identifikovala potenciální ohrožení zabezpečení a zmírnit hrozby. Čtení [Průvodce Azure Security Center plánováním a provozem](security-center-planning-and-operations-guide.md) pro další informace o tom, jak toodetermine hello možnost, která je vhodná pro vás.

## <a name="set-security-policies"></a>Nastavení zásad zabezpečení
Zásady zabezpečení můžete nakonfigurovat pro každé předplatné. toomodify zásady zabezpečení, musí být roli vlastníka nebo přispěvatele daného předplatného. Přihlaste se toohello portál Azure a postupujte podle hello úspěšné kroky, které zásady tooconfigure zabezpečení ve službě Security Center:

1. Klikněte na tlačítko hello **zásad** dlaždici na řídicím panelu Security Center hello.
2. V okně hello zásady zabezpečení, které se otevře vyberte předplatné hello, na kterém chcete zásady zabezpečení tooenable hello.

    ![Určení zásady](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Hello **zásady zabezpečení** otevře se okno pro hello vybrané předplatné sadu možností. dostupné možnosti Hello v tomto okně jsou:

   * **Zásada Zabránění**: tuto možnost tooconfigure zásady podle předplatného.  
   * **Oznámení e-mailem**: použijte tuto možnost tooconfigure e-mailové oznámení, která je odeslána na hello první denní výskyt výstrahy a pro výstrahy s vysokou závažností. Předvolby e-mailu lze konfigurovat pouze pro zásady předplatného. Čtení [zadejte kontaktní údaje zabezpečení v Azure Security Center](security-center-provide-security-contact-details.md) pro další informace o tooconfigure e-mailové oznámení.
   * **Cenová úroveň**: použijte tuto možnost tooupgrade hello cenová úroveň výběru. V tématu [ceny Security Center](security-center-pricing.md) toolearn informace o cenách možnosti.
4. Ujistěte se, že je u položky **Shromažďovat data z virtuálních počítačů** vybraná možnost **Zapnuto**. Tato možnost povolí automatické shromažďování protokolů u stávajících a nových prostředků pomocí hello agenta Microsoft Monitoring Agent – to je hello hello Operations Management Suite a analýzy protokolů služba používá stejný agenta. Data shromážděná z tohoto agenta je uložit existující pracovních prostorů analýzy protokolů spojené s předplatným Azure nebo nové pracovních prostorů, s ohledem na účet hello geography Dobrý den virtuálních počítačů.

5. V hello **zásady zabezpečení** okně klikněte na tlačítko **zásada Zabránění** toosee hello dostupné možnosti. Klikněte na tlačítko **na** tooenable hello zabezpečení doporučení, které jsou relevantní pro toto předplatné.

    ![Výběr zásad zabezpečení hello](./media/security-center-policies/security-center-policies-fig7.png)

Použijte následující tabulku jako referenční toounderstand hello jednotlivé možnosti:

| Zásada | Pokud je nastavená možnost Zapnuto |
| --- | --- |
| Aktualizace systému |Načte denní seznam dostupných aktualizací zabezpečení a důležitých aktualizací z webu Windows Update nebo ze služby Windows Server Update Services. Načtený seznam Hello závisí na hello služba, která je nakonfigurovaná pro tento virtuální počítač a doporučuje se, že hello chybějící aktualizace použije. Pro systémy Linux používá zásady hello hello distro balíčků správy systému toodetermine balíčků, které jsou k dispozici aktualizace. Také kontroluje aktualizace zabezpečení a důležité aktualizace z virtuálních počítačů služby [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Ohrožení zabezpečení operačního systému |Analyzuje konfigurace denní toodetermine problémy s operačním systémem které by mohly zvýšit citlivé tooattack hello virtuálního počítače. zásady Hello taky doporučuje tooaddress změny konfigurace tyto chyby zabezpečení. V tématu hello [seznamu doporučených standardních hodnot](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) Další informace o hello konkrétní konfigurace, které jsou monitorovány. (V tomto okamžiku není Windows Server 2016 plně podporovaný.) |
| Ochrana koncových bodů |Doporučuje endpoint protection toobe zřízené pro všechny virtuální počítače Windows toohelp identifikovat a odstraňovat viry, spyware a další škodlivý software. |
| Šifrování disku |Doporučuje povolení šifrování disku do ochrany dat tooenhance všechny virtuální počítače v klidovém stavu. |
| Skupiny zabezpečení sítě |Doporučuje [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) být nakonfigurované toocontrol příchozí a odchozí provoz tooVMs, které mají veřejné koncové body. Pokud neurčíte jinak, skupiny zabezpečení sítě nakonfigurované pro určitou podsíť se dědí do všech síťových rozhraní virtuálních počítačů. Kromě toho toochecking nakonfigurovaný skupinu zabezpečení sítě, tato zásada vyhodnocuje zabezpečení příchozích pravidel tooidentify pravidla, která povolují příchozí přenosy. |
| Brána firewall webových aplikací |Doporučuje se, že brány firewall webových aplikací zřídit na virtuálních počítačích, pokud je splněna jedna z následujících hello: </br></br>[Veřejná IP adresa na úrovni instance](../virtual-network/virtual-networks-instance-level-public-ip.md) (splnění) se používá a hello příchozí pravidla zabezpečení pro skupinu zabezpečení sítě spojenou s hello jsou nakonfigurované tooallow přístup tooport 80/443.</br></br>Vyrovnávání zatížení sítě IP se používá a hello přidružené Vyrovnávání zatížení a příchozích síťových pravidla překladu adres jsou nakonfigurované tooallow přístup tooport 80/443. Další informace najdete v tématu [Podpora nástroje pro vyrovnávání zatížení v Azure Resource Manageru](../load-balancer/load-balancer-arm.md). |
| Brána firewall příští generace |Rozšiřuje ochranu sítě nad rámec skupin zabezpečení sítě, které jsou integrované v Azure. Security Center bude zjišťovat nasazení, pro které se doporučuje brána firewall příští generace a povolit tooprovision virtuální zařízení. |
| Auditování SQL a zjišťováním hrozeb |Doporučuje, aby auditování přístupu k tooAzure databáze povolena pro dodržování předpisů a také pokročilé detekce hrozeb pro účely zkoumání. |
| Šifrování SQL |Doporučuje povolení neuplatněného šifrování pro služby Azure SQL Database, přidružené zálohy a soubory protokolů transakcí. I v případě, že dojde k porušení zabezpečení vašich dat, nebudou čitelná. |
| Posouzení ohrožení zabezpečení |Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení. |
| Šifrování služby Storage |Tato funkce je aktuálně dostupná pro Soubory a objekty blob Azure. Po povolení šifrování služby Storage budou šifrována pouze nová data a veškeré stávající soubory v účtu úložiště zůstanou nezašifrované. |
| Síťový přístup JIT |Pokud právě v čase je povoleno, Security Center zamyká tooyour příchozí přenosy virtuálních počítačů Azure ve vytvoření pravidla NSG. Vybrat hello porty na toowhich hello virtuálního počítače by měl být uzamčené příchozí provoz. Další informace najdete v tématu popisujícím [správu přístupu k virtuálním počítačům pomocí metody právě včas](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |

Když nakonfigurujete všechny možnosti, klikněte na tlačítko **OK** v hello **zásady zabezpečení** okno, které má hello doporučení a pak klikněte na tlačítko **Uložit** v hello **zabezpečení Zásady** okno, které má počáteční nastavení hello.

> [!NOTE]
> Hello cenová úroveň je stále platná pro hello úrovni skupiny prostředků. Další informace, kteří navštěvují hello [cenová](https://azure.microsoft.com/pricing/details/security-center/) stránky.
>
>

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak tooconfigure zásady zabezpečení v Azure Security Center. toolearn Další informace o službě Azure Security Center, najdete v části hello následující:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md). Zjistěte, jak tooplan a pochopit hello návrhu aspekty tooadopt Azure Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md). Zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Monitorování partnerských řešení pomocí služby Azure Security Center](security-center-partner-solutions.md). Zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Nejčastější dotazy o použití hello služby najít.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
