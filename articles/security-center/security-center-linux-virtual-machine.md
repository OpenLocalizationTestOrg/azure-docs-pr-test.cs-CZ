---
title: "aaaAzure Security Center a virtuální počítače Azure s Linuxem | Microsoft Docs"
description: "Tento dokument vám pomůže toounderstand jak Azure Security Center můžete zabezpečit můžete virtuální počítače Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: yurid
ms.openlocfilehash: d7aa9e54032272839dabfefa30c4c614d5e5610a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-virtual-machines-with-linux"></a>Azure Security Center a virtuální počítače s Linuxem
[Azure Security Center](https://azure.microsoft.com/services/security-center/) pomáhá zabránit, zjistit a reagovat toothreats. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Tento článek ukazuje, jak vám Security Center může pomoci zabezpečit virtuální počítače Azure s operačním systémem Linux.

## <a name="why-use-security-center"></a>Proč používat Security Center?
Security Center pomáhá chránit data virtuálních počítačů v Azure tím, že poskytuje vhled do nastavení zabezpečení vašich virtuálních počítačů a monitoruje hrozby. Security Center může u virtuálních počítačů monitorovat: 

* Nastavení zabezpečení operačního systému (OS) s hello doporučená pravidla konfigurace
* Zabezpečení systému a chybějící kritické aktualizace
* Doporučení ochrany koncových bodů
* Ověření šifrování disku
* Síťové útoky (k dispozici pouze ve [standardní verzi](https://azure.microsoft.com/en-us/pricing/details/security-center/))

Kromě toho toohelping chránit virtuální počítače Azure, Security Center nabízí také monitorování zabezpečení a správy pro cloudové služby, aplikační služby, virtuální sítě a další. 

> [!NOTE]
> V tématu [Úvod tooAzure Security Center](security-center-intro.md) toolearn Další informace o službě Azure Security Center.
> 
> 

## <a name="prerequisites"></a>Požadavky
tooget začít s Azure Security Center, budete potřebovat tooknow a zvažte následující hello:

* Musíte mít tooMicrosoft předplatné Azure. V tématu [Ceny Security Center](https://azure.microsoft.com/pricing/details/security-center/) najdete další informace o úrovních Free a Standard služby Security Center.
* Plánování vašeho přijetí Security Center, najdete v části [Průvodce plánováním a operace Azure Security Center](security-center-planning-and-operations-guide.md) toolearn další aspekty plánování a operace.
* Informace týkající se podpory operačních systémů najdete v tématu [Nejčastější dotazy k Azure Security Center](security-center-faq.md). 

## <a name="set-security-policy"></a>Nastavení zásad zabezpečení
Toobe potřeb kolekce dat povoleno, že Azure Security Center může shromažďovat hello informace, které potřebuje tooprovide doporučení a výstrahy, které jsou vytvořeny na základě hello zásad zabezpečení, které nakonfigurujete. Hello obrázek, můžete uvidíte, že **shromažďování dat** byla zapnuta **na**.

Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci zadané předplatné nebo prostředek skupiny hello. Než povolíte zásady zabezpečení, musí mít povolené shromažďování dat, Security Center shromáždí data z virtuálních počítačů v pořadí tooassess jejich stavu zabezpečení zadejte doporučení zabezpečení a výstrahy toothreats. V Security Center určíte zásady pro vaše předplatná Azure nebo skupiny prostředků podle potřeb zabezpečení tooyour společnosti a hello typu aplikací nebo citlivosti dat hello v každém předplatném. 

![Zásady zabezpečení](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig1.png)

> [!NOTE]
> více o jednotlivých toolearn **zásada Zabránění** k dispozici, najdete v tématu [nastavovat zásady zabezpečení](security-center-policies.md) článku.
> 

## <a name="manage-security-recommendations"></a>Správa doporučení zabezpečení
Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení. Hello doporučení vás provede procesem hello konfigurace hello potřebné ovládací prvky.

Po nastavení zásad zabezpečení, Security Center analyzuje stav zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení. Hello doporučení se zobrazí ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení. Hello následující tabulka obsahuje některé příklady doporučení pro virtuální počítače Azure spuštěný operační systém Linux a co každé z nich bude dělat, když ho použijete. Když vyberete doporučení, bude třeba zadat informace, které ukazuje, jak tooimplement hello doporučení ve službě Security Center.

| Doporučení | Popis |
| --- | --- |
| [Povolení shromažďování dat pro předplatná](security-center-enable-data-collection.md) |Doporučuje se v rámci vašich předplatných shromažďování dat v zásadách zabezpečení hello pro každé z vašich předplatných a všechny virtuální počítače (VM) zapnout. |
| [Náprava ohrožení zabezpečení operačního systému](security-center-remediate-os-vulnerabilities.md) |Doporučuje zarovnané vaše konfigurace operačního systému s hello doporučená pravidla konfigurace, například neumožňují toobe hesla uložit. |
| [Instalace aktualizací systému](security-center-apply-system-updates.md) |Doporučuje se nasadit chybějící zabezpečení systému a tooVMs důležité aktualizace. |
| [Restartování po aktualizacích systému](security-center-apply-system-updates.md#reboot-after-system-updates) |Doporučuje se, že restartujete proces virtuálních počítačů toocomplete hello použití aktualizací systému. |
| [Povolení agenta virtuálního počítače](security-center-enable-vm-agent.md) |Umožňuje vám toosee které virtuální počítače vyžadují hello agenta virtuálního počítače. Hello agenta virtuálního počítače musí být nainstalován na virtuálních počítačích v pořadí tooprovision oprava kontrolu, kontrolu směrného plánu a antimalwarových programů. Hello agenta virtuálního počítače je nainstalována ve výchozím nastavení pro virtuální počítače, které byly nasazeny pomocí hello Azure Marketplace. článek Hello [agenta virtuálního počítače a rozšíření – část 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) poskytuje informace o tom, jak tooinstall hello agenta virtuálního počítače. |
| [Použití šifrování disku](security-center-apply-disk-encryption.md) |Doporučuje, abyste disky svých virtuálních počítačů zašifrovali pomocí služby Azure Disk Encryption (platí pro virtuální počítače s Windows a Linuxem). Šifrování se doporučuje pro hello operačního systému a datové svazky na vašem virtuálním počítači. |


> [!NOTE]
> toolearn Další informace o doporučení, najdete v části [Správa doporučení zabezpečení](security-center-recommendations.md) článku.
> 

## <a name="monitor-security-health"></a>Monitorování stavu zabezpečení
Po povolení [zásady zabezpečení](security-center-policies.md) pro prostředky předplatného bude Security Center analyzovat zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení.  Můžete zobrazit stav zabezpečení vašich prostředků, spolu s případnými problémy v hello hello **stav zabezpečení prostředků** okno. Když kliknete na tlačítko **virtuální počítače** v hello **zabezpečení prostředků** dlaždice stavu, hello **virtuální počítače** otevře se okno s doporučeními pro virtuální počítače. 

![Stav zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-toosecurity-alerts"></a>Spravovat a reagovat toosecurity výstrahy
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, sítě hello a připojených partnerských řešení (jako jsou brány firewall a endpoint protection řešení), toodetect skutečné hrozby a snížil počet falešných poplachů. S využitím různých agregace [možností detekce](security-center-detection-capabilities.md), Security Center je možné toogenerate nastavovat zabezpečení výstrahy toohelp rychle hello problému a poskytovat doporučení, jak tooremediate možných útoků.

![Výstrahy zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Vyberte toolearn výstrahy zabezpečení informace o hello událostí, který aktivoval výstrahu hello a co, pokud existuje, kroky je nutné tootake tooremediate útoku. Výstrahy zabezpečení jsou seskupené podle [typu](security-center-alerts-type.md) a data.

## <a name="monitor-security-health"></a>Monitorování stavu zabezpečení
Po povolení [zásady zabezpečení](security-center-policies.md) pro prostředky předplatného bude Security Center analyzovat zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení.  Můžete zobrazit stav zabezpečení vašich prostředků, spolu s případnými problémy v hello hello **stav zabezpečení prostředků** okno. Když kliknete na tlačítko **virtuální počítače** v hello **zabezpečení prostředků** dlaždice stavu, hello **virtuální počítače** otevře se okno s doporučeními pro virtuální počítače. 

![Stav zabezpečení](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig4.png)

Pokud kliknete na toto doporučení, zobrazí se další podrobnosti o hello konkrétní akce, které by měly být provedeny tooaddress těchto problémů. Hello podrobností se zobrazí v hello dolní části okna hello, v části **doporučení**. 

![Stav zabezpečení 2](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig5.png)


## <a name="see-also"></a>Viz také
toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.

