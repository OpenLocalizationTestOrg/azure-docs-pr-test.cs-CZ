---
title: "Migrace platformy Security Center – nejčastější dotazy | Microsoft Docs"
description: "Tyto nejčastější dotazy odpovídá na dotazy týkající se migrace platformy Azure střediska zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: terrylan
ms.openlocfilehash: 2ffbaca614d667db565197f3c13b1658fffc2a7c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="security-center-platform-migration-faq"></a>Migrace platformy Security Center – nejčastější dotazy
V časná června 2017 začal Azure Security Center pomocí agenta Microsoft Monitoring Agent shromažďovat a ukládat data. Další informace najdete v tématu [Azure Security Center platformy migrace](security-center-platform-migration.md). Tyto nejčastější dotazy odpovídá na dotazy týkající se migrace platformy.

## <a name="data-collection-agents-and-workspaces"></a>Shromažďování dat, agentů a pracovních prostorů

### <a name="how-is-data-collected"></a>Jak se shromažďují data?
Security Center používá ke shromažďování dat zabezpečení z virtuálních počítačů Microsoft Monitoring Agent. Zabezpečení dat obsahuje informace o konfiguracích zabezpečení, které slouží ke zjištění chyb zabezpečení, a události zabezpečení, které se používají k detekci hrozeb. Údaje shromážděné agentem je uložená v existující pracovní prostor analýzy protokolů připojený k virtuálnímu počítači nebo nový pracovní prostor vytvořené Security Center. Security Center vytvoří nový pracovní prostor, informace o zeměpisné poloze virtuálního počítače je vzít v úvahu.

> [!NOTE]
> Microsoft Monitoring Agent je stejného agenta použít pomocí Operations Management Suite (OMS), analýzy protokolů služby a služby System Center Operations Manager (SCOM).
>
>

Při shromažďování dat je povolené poprvé, nebo když se migrují vašich předplatných, zkontroluje Security Center, pokud Microsoft Monitoring Agent je již nainstalována jako rozšíření, která byla na všech virtuálních počítačů Azure. Microsoft Monitoring Agent není nainstalován, pak bude Security Center:

- do virtuálního počítače nainstalujte Microsoft Monitoring agent
   - Pokud pracovní prostor vytvořené Security Center již existuje ve stejné informace o zeměpisné poloze jako virtuální počítač, agenta je připojen k tomuto pracovnímu prostoru
   - pracovní prostor neexistuje, Security Center vytvoří novou skupinu prostředků a výchozí pracovního prostoru v této informace o zeměpisné poloze a připojit agenta do tohoto pracovního prostoru. Zásady vytváření názvů pro pracovní prostor a prostředek skupiny jsou:

       Pracovní prostor: DefaultWorkspace-[ID předplatného]-[geograficky]

       Skupina prostředků: DefaultResouceGroup-[geograficky]
- v pracovním prostoru pro instalaci řešení Security Center

Umístění pracovního prostoru je založena na umístění virtuálního počítače. Další informace najdete v tématu [zabezpečení dat](security-center-data-security.md).

> [!NOTE]
> Před migrací platformy Security Center shromážděná data zabezpečení z virtuálních počítačů pomocí nástroje Azure Monitoring Agent a data byla uložena ve vašem účtu úložiště. Po dokončení migrace platformy Security Center používá Microsoft Monitoring Agent a prostoru shromáždit a uložit stejná data. Účet úložiště lze odebrat po migraci.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-the-workspaces-created-by-security-center"></a>Mě I účtovány poplatky za analýzy protokolů nebo OMS na pracovních prostorů vytvořit pomocí služby Security Center?
Ne. Pracovní prostory, které jsou vytvořené pomocí služby Security Center, zatímco nakonfigurovaný pro OMS na uzlu fakturace, nevznikají OMS poplatky. Fakturace Security Center je vždy založené na vaše zásady zabezpečení Security Center a řešení v pracovním prostoru nainstalován:

- **Úroveň Free** – Security Center nainstaluje řešení 'SecurityCenterFree' na výchozí pracovní prostor. Fakturuje nejsou pro úroveň Free.
- **Úroveň standard** – Security Center nainstaluje řešení 'SecurityCenterFree' a 'zabezpečení, v pracovním prostoru pro výchozí.

Další informace o cenách najdete v tématu [Security Center ceny](https://azure.microsoft.com/pricing/details/security-center/). Na stránce s cenami řeší změny do úložiště dat zabezpečení a poměrné fakturace od června 2017.

> [!NOTE]
> OMS cenová úroveň pracovních prostorů vytvořit pomocí služby Security Center neovlivňuje fakturace Security Center.
>
>

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Můžete odstranit výchozí pracovních prostorů vytvořit pomocí služby Security Center?
**Odstraňuje se pracovní prostor výchozí se nedoporučuje.** Security Center používá výchozí pracovní prostory k ukládání data zabezpečení z virtuálních počítačů.  Pokud odstraníte pracovního prostoru, Security Center se nepodařilo shromažďovat tato data a některé doporučení zabezpečení a výstrahy nejsou k dispozici

Pokud chcete obnovit, odeberte agenta Microsoft Monitoring Agent na virtuální počítače připojené k odstraněné pracovního prostoru. Security Center opětovná instalace agenta a vytvoří nový výchozí pracovní prostory.

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Co když Microsoft Monitoring Agent byl již nainstalován jako rozšíření ve virtuálním počítači?
Security Center nemůže přepsat existující připojení k pracovním prostorům, uživatele. Security Center ukládá data zabezpečení z virtuálního počítače v pracovním prostoru již připojen.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Co když měl Microsoft Monitoring Agent nainstalovaný na počítači, ale ne jako rozšíření?
Pokud službu Microsoft Monitoring Agent instaluje přímo do virtuálního počítače (ne jako Azure rozšíření), Security Center nenainstaluje agenta Microsoft Monitoring Agent a sledování zabezpečení bude omezena.

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Co je dopad odebrání těchto rozšíření?
Pokud odeberete rozšíření monitorování společnosti Microsoft, Security Center nebude moct shromažďovat data zabezpečení z virtuálního počítače a některé doporučení zabezpečení a výstrahy nejsou k dispozici. Do 24 hodin Security Center zjistí, že chybí rozšíření virtuálního počítače a přeinstaluje rozšíření.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Jak zabráním agentů pro automatickou diagnostiku instalace a prostoru vytváření?
Shromažďování dat můžete vypnout pro vaše předplatná v zásadě zabezpečení, ale to nedoporučujeme. Vypnutí výstrahy a omezení shromažďování dat doporučení služby Security Center. Shromažďování dat je vyžadován pro odběry cenová úroveň Standard. Postup při zakázání shromažďování dat:

1. Pokud váš odběr je nakonfigurován pro úroveň Standard, spustit nástroj Zásady zabezpečení pro toto předplatné a vyberte **volné** vrstvy.

   ![Cenová úroveň][1]

2. V dalším kroku vypnutí shromažďování dat tak, že vyberete **vypnout** na **zásady zabezpečení – shromažďování dat** okno.

   ![Shromažďování dat][2]

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Odebrání OMS rozšíření nainstalované pomocí služby Security Center
Microsoft Monitoring Agent můžete odebrat ručně. Toto se nedoporučuje, protože se omezuje doporučení služby Security Center a výstrahy.

> [!NOTE]
> Pokud se shromažďování dat je povolené, Security Center bude po odebrání již přeinstalujte agenta.  Je nutné zakázat shromažďování dat před ručním odebráním agenta. V tématu [jak zastavit instalace a prostoru vytváření agentů pro automatickou diagnostiku?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) pokyny pro zakázání shromažďování dat.
>
>

Chcete-li ručně odebrat agenta:

1.  Na portálu, otevřete **analýzy protokolů**.
2.  V okně Log Analytics vyberte pracovní prostor:
3.  Vyberte každý virtuální počítač, který nechcete, aby ke sledování a vyberte **odpojení**.

   ![Odeberte agenta][3]

> [!NOTE]
> Pokud virtuální počítač s Linuxem již má jiné rozšíření agenta OMS, odebírání rozšíření odebere i agenta a zákazník má ji přeinstalovat.
>
>

## <a name="existing-oms-customers"></a>Stávající zákazníky služby OMS

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Security Center přepsání všech existujících připojení mezi virtuální počítače a pracovní prostory?
Pokud virtuální počítač už má Microsoft Monitoring Agent nainstalována jako rozšíření Azure, Security Center nemůže přepsat existující připojení pracovního prostoru. Místo toho Security Center používá existujícímu pracovnímu prostoru.

Řešení Security Center je nainstalován v pracovním prostoru není-li již k dispozici, a řešení se použije jenom pro příslušné virtuální počítače. Když přidáte řešení, se automaticky nasadí ve výchozím nastavení všechny agenty systému Windows a Linux připojené k pracovní prostor analýzy protokolů. [Cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md), což je funkce OMS, vám umožní použít pro obor pro vaše řešení.

Pokud je Microsoft Monitoring Agent nainstalován přímo na virtuálním počítači (ne jako Azure rozšíření), Security Center nenainstaluje agenta Microsoft Monitoring Agent a sledování zabezpečení je omezená.

### <a name="what-should-i-do-if-i-suspect-that-the-data-platform-migration-broke-the-connection-between-one-of-my-vms-and-my-workspace"></a>Co mám dělat, když je pravděpodobné že platformy migrace dat překročila připojení mezi jeden z virtuálních počítačů a pracovního prostoru?
To nemělo stát. Pokud k tomu dojít, pak [vytvoření žádosti o podporu Azure](../azure-supportability/how-to-create-azure-support-request.md) a uveďte následující podrobnosti:

- ID prostředku Azure ovlivněné virtuálního počítače
- ID prostředku Azure pracovního prostoru nakonfigurovaná na rozšíření před připojení bylo přerušeno
- Agent a verzi, která byla dříve nainstalovaná

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-the-billing-implications"></a>Security Center se nenainstaluje řešení v mé existující pracovní prostory OMS? Jaké jsou důsledky fakturace?
Pokud Security Center identifikuje, že virtuální počítač je již připojen do pracovního prostoru, který jste vytvořili, Security Center umožňuje řešení na tento pracovní prostor podle cenové úrovně. Řešení se použijí jenom u příslušných virtuálních počítačích Azure, prostřednictvím [cílení na řešení](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), takže fakturaci zůstává stejná.

- **Úroveň Free** – Security Center nainstaluje řešení 'SecurityCenterFree' v pracovním prostoru. Fakturuje nejsou pro úroveň Free.
- **Úroveň standard** – Security Center nainstaluje řešení 'SecurityCenterFree' a 'zabezpečení, v pracovním prostoru.

   ![Řešení na výchozí pracovní prostor][4]

> [!NOTE]
> Řešení, zabezpečení, v analýzy protokolů je zabezpečení & auditu v OMS.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Už mám pracovní prostory v mé prostředí, můžete chci použít ke shromažďování dat zabezpečení?
Pokud virtuální počítač už má Microsoft Monitoring Agent nainstalována jako rozšíření Azure, Security Center používá existující připojené pracovního prostoru. Řešení Security Center je nainstalován v pracovním prostoru není-li již k dispozici a řešení se použije jenom pro příslušné virtuální počítače prostřednictvím [cílení na řešení](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

Pokud Security Center nainstaluje agenta Microsoft Monitoring Agent na virtuálních počítačích, používá výchozí pracovních prostorů, vytvořené Security Center. Brzy zákazníci budou moci konfigurovat, které pracovních prostorů se používají.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Už mám řešení zabezpečení na osobní pracovní prostory. Jaké jsou důsledky fakturace?
Řešení a auditu zabezpečení slouží k povolení funkce Security Center standardní úrovně pro virtuální počítače Azure. Pokud řešení zabezpečení a Audit je již nainstalován v pracovním prostoru, Security Center používá existující řešení. Neexistuje žádná změna v fakturace.

## <a name="next-steps"></a>Další kroky
Další informace o migraci platformy Security Center najdete v tématu

- [Migrace pro platformu Azure Security Center](security-center-platform-migration.md)
- [Příručka pro řešení potíží s Azure Security Center](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
