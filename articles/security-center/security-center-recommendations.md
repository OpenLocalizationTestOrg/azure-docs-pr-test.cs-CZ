---
title: "aaaManaging doporučení zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vás provede jak doporučení v Azure Security Center pomáhá zůstat souladu se zásadami zabezpečení a ochraně vašich prostředků Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: f6bbe36a7a5636095b339b3e9765b87cc0ab669a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Správa doporučení zabezpečení v Azure Security Center
Tento dokument vás provede procesem způsob toouse doporučení v Azure Security Center toohelp ochrany vašich prostředků Azure.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Tento dokument není to podrobný průvodce.
>
>

## <a name="what-are-security-recommendations"></a>Jaké jsou doporučení zabezpečení?
Security Center pravidelně analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení. Hello doporučení vás provede procesem hello konfigurace hello potřebné ovládací prvky.

## <a name="implementing-security-recommendations"></a>Implementace doporučení zabezpečení
### <a name="set-recommendations"></a>Sadu doporučení
V [nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md), Naučte se:

* Konfigurovat zásady zabezpečení.
* Shromažďování dat zapněte.
* Vyberte, které toosee doporučení v rámci svých zásad zabezpečení.

Aktuální zásady doporučení center kolem aktualizací systému, standardních pravidel, antimalwarových programů [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) na podsítí a síťových rozhraní, auditování databáze SQL, SQL database transparentní šifrování dat, a webové aplikace brány firewall.  [Nastavení zásad zabezpečení](security-center-policies.md) obsahuje popis jednotlivých možností doporučení.

### <a name="monitor-recommendations"></a>Monitorování doporučení
Po nastavení zásad zabezpečení, Security Center analyzuje stav zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení. Hello **doporučení** na hello dlaždici **Security Center** okno vám oznamuje hello celkový počet doporučení identifikovaný Security Center.

![Dlaždice doporučení][1]

Podrobnosti hello toosee jednotlivá doporučení:

Vyberte hello **doporučení dlaždici** na hello **Security Center** okno. Hello **doporučení** otevře se okno.

Hello doporučení se zobrazí ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení. Hello sloupce v této tabulce jsou:

* **Popis**: vysvětluje hello doporučení a toho, co je toobe provádí tooaddress ho.
* **PROSTŘEDEK**: uvádí toowhich prostředky hello toto doporučení se vztahují.
* **Stav**: Popisuje hello aktuální stav doporučení hello:
  * **Otevřete**: dosud nebylo řešeno hello doporučení.
  * **V průběhu**: hello doporučení je v současné době použité toohello prostředky a není třeba žádné akce.
  * **Vyřešit**: hello doporučení již byla dokončena (v tomto případě hello řádku je zobrazena šedě).
* **ZÁVAŽNOST**: Popisuje hello závažnost tohoto konkrétního doporučení:
  * **Vysoká**: ohrožení zabezpečení existuje u významného prostředku (například aplikace, virtuální počítač nebo skupinu zabezpečení sítě) a vyžaduje pozornost.
  * **Střední**: byla zjištěna chyba zabezpečení a nekritické nebo další kroky jsou požadované tooeliminate ho nebo toocomplete proces.
  * **Nízká**: obsahuje chybu, mělo by se řešit, ale nevyžaduje okamžitou pozornost. (Ve výchozím nastavení, nejsou přítomny nízkou doporučení, ale můžete filtrovat podle nízkou doporučení, pokud chcete, aby toosee jejich.)

Použití hello tabulce jako referenční toohelp porozumíte hello k dispozici doporučení a co každé z nich dělá, pokud ji použijete.

> [!NOTE]
> Můžete toounderstand hello [classic a modelech nasazení Resource Manager](../azure-classic-rm.md) pro prostředky Azure.
>
>

| Doporučení | Popis |
| --- | --- |
| [Povolení shromažďování dat pro předplatná](security-center-enable-data-collection.md) |Doporučuje se v rámci vašich předplatných shromažďování dat v zásadách zabezpečení hello pro každé z vašich předplatných a všechny virtuální počítače (VM) zapnout. |
| [Náprava ohrožení zabezpečení operačního systému](security-center-remediate-os-vulnerabilities.md) |Doporučuje se přiblížili hello doporučená pravidla konfigurace, například vaše konfigurace operačního systému, zakázat toobe hesla uložit. |
| [Instalace aktualizací systému](security-center-apply-system-updates.md) |Doporučuje se nasadit chybějící zabezpečení systému a tooVMs důležité aktualizace. |
| [Použít pouze v době provedená sítě řízení přístupu](security-center-just-in-time.md) | Doporučuje se použít jenom v přístup k časovému virtuálních počítačů. Hello pouze ve funkci čas je ve verzi preview a je k dispozici na hello úrovně Standard služby Security Center. V tématu [cenová](security-center-pricing.md) toolearn Další informace o Security Center je cenové úrovně. |
| [Restartování po aktualizacích systému](security-center-apply-system-updates.md#reboot-after-system-updates) |Doporučuje se, že restartujete proces virtuálních počítačů toocomplete hello použití aktualizací systému. |
| [Přidání brány firewall webových aplikací](security-center-add-web-application-firewall.md) |Doporučuje, která můžete nasadit brány firewall webových aplikací (firewall webových aplikací) pro koncových bodů webové. Doporučení firewall webových aplikací je zobrazený pro všechny veřejné přístupných IP adresy (IP úrovni Instance nebo IP skupinu s vyrovnáváním zatížení), skupinu zabezpečení sítě spojenou s otevřete příchozí webovými porty (80,443). </br>Security Center doporučí, že zřídit toohelp firewall webových aplikací bránit proti útokům na cílení na vaše webové aplikace na virtuální počítače a služby App Service Environment. Je aplikaci služby prostředí (App Service Environment) [Premium](https://azure.microsoft.com/pricing/details/app-service/) služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service. toolearn Další informace o App Service Environment, najdete v části hello [dokumentace k aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).</br>Několika webových aplikací ve službě Security Center můžete chránit tak, že přidáte tyto aplikace tooyour existující firewall webových aplikací nasazení. |
| [Finalizace ochrany aplikací](security-center-add-web-application-firewall.md#finalize-application-protection) |Konfigurace hello toocomplete firewall webových aplikací, provoz musí být zařízení přesměrovanou toohello firewall webových aplikací. Následující toto doporučení dokončení změny potřebné instalační hello. |
| [Přidání brány firewall příští generace](security-center-add-next-generation-firewall.md) |Doporučuje se, že přidáte brány Firewall pro další generace (NGFW) z tooincrease partnera Microsoft vaše ochranu zabezpečení. |
| [Směrování provozu jenom přes NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Doporučuje konfiguraci pravidla zabezpečení skupiny (NSG) sítě, které vynutí tooyour příchozí přenosy virtuálních počítačů prostřednictvím vaší NGFW. |
| [Instalace Endpoint Protection](security-center-install-endpoint-protection.md) |Doporučuje zřízení tooVMs antimalwarových programů (jenom Windows VM). |
| [Vyřešení výstrah stavu služby Endpoint Protection](security-center-resolve-endpoint-protection-health-alerts.md) |Doporučuje, abyste vyřešili selhání ochrany koncových bodů. |
| [Povolení skupin zabezpečení sítě pro podsítě nebo virtuální počítače](security-center-enable-network-security-groups.md) |Doporučuje, abyste povolili skupiny Nsg na podsítě nebo virtuálních počítačů. |
| [Omezení přístupu prostřednictvím internetové koncový bod](security-center-restrict-access-through-internet-facing-endpoints.md) |Doporučuje konfigurace pravidla pro příchozí provoz pro skupiny Nsg. |
| [Povolení auditování a detekce hrozeb na SQL serverech](security-center-enable-auditing-on-sql-servers.md) |Doporučuje zapnout auditování a zjišťování hrozeb pro servery Azure SQL. (Jenom služba azure SQL. Neobsahuje SQL běžících na virtuálních počítačích.) |
| [Povolení auditování a detekce hrozeb v databázích SQL](security-center-enable-auditing-on-sql-databases.md) |Doporučuje zapnout auditování a zjišťování hrozeb pro databáze Azure SQL. (Jenom služba azure SQL. Neobsahuje SQL běžících na virtuálních počítačích.) |
| [Povolit transparentní šifrování dat v databázích SQL](security-center-enable-transparent-data-encryption.md) |Doporučuje se, že povolíte šifrování pro databáze SQL. (Služba azure SQL pouze.) |
| [Povolení agenta virtuálního počítače](security-center-enable-vm-agent.md) |Umožňuje vám toosee které virtuální počítače vyžadují hello agenta virtuálního počítače. Hello agenta virtuálního počítače musí být nainstalován na virtuálních počítačích tooprovision oprava kontrolu, kontrolu směrného plánu a antimalwarových programů. Hello agenta virtuálního počítače je nainstalována ve výchozím nastavení pro virtuální počítače, které byly nasazeny pomocí hello Azure Marketplace. článek Hello [agenta virtuálního počítače a rozšíření – část 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) poskytuje informace o tom, jak tooinstall hello agenta virtuálního počítače. |
| [Použití šifrování disku](security-center-apply-disk-encryption.md) |Doporučuje, abyste disky svých virtuálních počítačů zašifrovali pomocí služby Azure Disk Encryption (platí pro virtuální počítače s Windows a Linuxem). Šifrování se doporučuje pro hello operačního systému a datové svazky na vašem virtuálním počítači. |
| [Poskytnutí podrobností kontaktů zabezpečení](security-center-provide-security-contact-details.md) |Doporučuje se, že zadáte zabezpečení kontaktní informace pro každé z vašich předplatných. Kontaktní informace je e-mailovou adresu a telefonní číslo. Hello informace je použité toocontact vám, zda náš tým zabezpečení zjistí, že jsou ohrožení zabezpečení vašich prostředků. |
| [Aktualizace verze operačního systému](security-center-update-os-version.md) |Doporučuje se aktualizovat verzi operačního systému (OS) hello ke cloudové službě toohello nejnovější verzi k dispozici pro operačních systémů.  toolearn Další informace o cloudové služby, najdete v části hello [cloudové služby přehled](../cloud-services/cloud-services-choose-me.md). |
| [Není nainstalováno posouzení ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md) |Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení. |
| [Náprava ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Umožní vám toosee systému a aplikací chyb zabezpečení detekovaných službou nainstalovaná na vašem virtuálním počítači řešení pro vyhodnocení hello ohrožení zabezpečení. |
| [Povolit šifrování pro účet úložiště Azure](security-center-enable-encryption-for-storage-account.md) | Doporučuje povolit šifrování služby úložiště Azure pro data v klidovém stavu. Šifrování služby úložiště (SSE) funguje tak, že šifrování dat hello, pokud je napsána tooAzure úložiště a dešifruje před načtení. SSE aktuálně nejsou k dispozici pouze pro hello služby objektů Blob Azure lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB. Další, najdete v části toolearn [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).</br>SSE je podporována pouze na účty úložiště Resource Manager. |

Můžete filtrovat a zavřít doporučení.

1. Vyberte **filtru** na hello **doporučení** okno. Hello **filtru** otevře se okno a vybrat závažnost a stavu hodnoty hello chcete toosee.

    ![Filtrovat doporučení][2]
2. Pokud zjistíte, že doporučení se nedá použít, můžete zavřít hello doporučení a pak ji odfiltrovat ze zobrazení. Existují dva způsoby toodismiss doporučení. Jedním ze způsobů je tooright klikněte na položku a potom vyberte **přeskočení**. Hello jiných je toohover nad položku, klikněte na tlačítko hello třemi tečkami, které jsou uvedeny toohello vpravo a pak vyberte **přeskočení**. Kliknutím můžete zobrazit dismissed doporučení **filtru**a potom vyberete **zamítnuto**.

    ![Zavření doporučení][3]

### <a name="apply-recommendations"></a>Použít doporučení
Po kontrole všech doporučení, rozhodněte, který jeden byste měli použít první. Doporučujeme používat hello hodnocení závažnosti, jak hello hlavní parametr tooevaluate doporučení, které bude použito první.

V tabulce hello předchozí doporučení, vyberte doporučení a provede ji jako příklad tooapply doporučení.

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste přináší toosecurity doporučení ve službě Security Center. toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
