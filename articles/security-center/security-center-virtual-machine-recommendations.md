---
title: "aaaProtecting virtuální počítače v Azure Security Center | Microsoft Docs"
description: "Tento dokument adresy doporučení v Azure Security Center, které vám pomůžou chránit virtuální počítače a zůstat souladu se zásadami zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: 926c04974f61215b4a3e02646f23dafb87c793e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Ochrana virtuálních počítačů v Azure Security Center
Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem hello konfigurace hello potřebné ovládací prvky.  Doporučení se týkají tooAzure typy prostředků: virtuální počítače (VM), sítě, SQL a aplikace.

Tento článek se zaměřuje na doporučení, které se vztahují tooVMs.  Virtuální počítač doporučení center kolem shromažďování dat, použití aktualizací systému, zřizování antimalwaru, šifrování vaše disky virtuálního počítače a další.  Použití hello tabulce jako referenční toohelp porozumíte hello dostupná doporučení pro virtuální počítače a co každé z nich bude dělat, když ho použijete.

## <a name="available-vm-recommendations"></a>K dispozici doporučení pro virtuální počítače
| Doporučení | Popis |
| --- | --- |
| [Povolení shromažďování dat pro předplatná](security-center-enable-data-collection.md) |Doporučuje se v rámci vašich předplatných shromažďování dat v zásadách zabezpečení hello pro každé z vašich předplatných a všechny virtuální počítače (VM) zapnout. |
| [Povolit šifrování pro účet úložiště Azure](security-center-enable-encryption-for-storage-account.md) | Doporučuje povolit šifrování služby úložiště Azure pro data v klidovém stavu. Šifrování služby úložiště (SSE) funguje tak, že šifrování dat hello, pokud je napsána tooAzure úložiště a dešifruje před načtení. SSE aktuálně nejsou k dispozici pouze pro hello služby objektů Blob Azure lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB. Další, najdete v části toolearn [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).</br>SSE je podporována pouze na účty úložiště Resource Manager. Klasické účty úložiště nejsou aktuálně podporovány. toounderstand hello classic a modelech nasazení Resource Manager, najdete v části [modelech nasazení Azure](../azure-classic-rm.md). |
| [Náprava ohrožení zabezpečení operačního systému](security-center-remediate-os-vulnerabilities.md) |Doporučuje zarovnané vaše konfigurace operačního systému s hello doporučená pravidla konfigurace, například neumožňují toobe hesla uložit. |
| [Instalace aktualizací systému](security-center-apply-system-updates.md) |Doporučuje se nasadit chybějící zabezpečení systému a tooVMs důležité aktualizace. |
| [Použít pouze v době provedená sítě řízení přístupu](security-center-just-in-time.md) | Doporučuje se použít jenom v přístup k časovému virtuálních počítačů. Hello pouze ve funkci čas je ve verzi preview a je k dispozici na hello úrovně Standard služby Security Center. V tématu [cenová](security-center-pricing.md) toolearn Další informace o Security Center je cenové úrovně. |
| [Restartování po aktualizacích systému](security-center-apply-system-updates.md#reboot-after-system-updates) |Doporučuje se, že restartujete proces virtuálních počítačů toocomplete hello použití aktualizací systému. |
| [Instalace Endpoint Protection](security-center-install-endpoint-protection.md) |Doporučuje zřízení tooVMs antimalwarových programů (jenom Windows VM). |
| [Vyřešení výstrah stavu služby Endpoint Protection](security-center-resolve-endpoint-protection-health-alerts.md) |Doporučuje, abyste vyřešili selhání ochrany koncových bodů. |
| [Povolení agenta virtuálního počítače](security-center-enable-vm-agent.md) |Umožňuje vám toosee které virtuální počítače vyžadují hello agenta virtuálního počítače. Hello agenta virtuálního počítače musí být nainstalován na virtuálních počítačích v pořadí tooprovision oprava kontrolu, kontrolu směrného plánu a antimalwarových programů. Hello agenta virtuálního počítače je nainstalována ve výchozím nastavení pro virtuální počítače, které byly nasazeny pomocí hello Azure Marketplace. článek Hello [agenta virtuálního počítače a rozšíření – část 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) poskytuje informace o tom, jak tooinstall hello agenta virtuálního počítače. |
| [Použití šifrování disku](security-center-apply-disk-encryption.md) |Doporučuje, abyste disky svých virtuálních počítačů zašifrovali pomocí služby Azure Disk Encryption (platí pro virtuální počítače s Windows a Linuxem). Šifrování se doporučuje pro hello operačního systému a datové svazky na vašem virtuálním počítači. |
| [Aktualizace verze operačního systému](security-center-update-os-version.md) |Doporučuje se aktualizovat verzi operačního systému (OS) hello ke cloudové službě toohello nejnovější verzi k dispozici pro operačních systémů.  toolearn Další informace o cloudové služby, najdete v části hello [cloudové služby přehled](../cloud-services/cloud-services-choose-me.md). |
| [Není nainstalováno posouzení ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md) |Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení. |
| [Náprava ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Umožní vám toosee systému a aplikací chyb zabezpečení detekovaných službou nainstalovaná na vašem virtuálním počítači řešení pro vyhodnocení hello ohrožení zabezpečení. |

## <a name="see-also"></a>Viz také
toolearn Další informace o doporučení, které se vztahují tooother typy prostředků Azure, najdete v části hello následující:

* [Ochrana aplikací v Azure Security Center](security-center-application-recommendations.md)
* [Ochrana sítě v Azure Security Center](security-center-network-recommendations.md)
* [Ochrana služby Azure SQL v Azure Security Center](security-center-sql-service-recommendations.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
