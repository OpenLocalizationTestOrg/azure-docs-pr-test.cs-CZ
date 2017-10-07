---
title: "migrace platformy aaaSecurity Center – nejčastější dotazy | Microsoft Docs"
description: "Tyto nejčastější dotazy odpovědi na otázky o hello migrace platformy Azure střediska zabezpečení."
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
ms.openlocfilehash: fcb14ae83167ef79a60371e4fcb625cf99bee6c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-center-platform-migration-faq"></a>Migrace platformy Security Center – nejčastější dotazy
V časná června 2017 začal Azure Security Center pomocí agenta Microsoft Monitoring Agent hello toocollect a úložiště dat. Další, najdete v části toolearn [Azure Security Center platformy migrace](security-center-platform-migration.md). Tyto nejčastější dotazy odpovídá na dotazy týkající se migrace platformy hello.

## <a name="data-collection-agents-and-workspaces"></a>Shromažďování dat, agentů a pracovních prostorů

### <a name="how-is-data-collected"></a>Jak se shromažďují data?
Security Center používá hello agenta Microsoft Monitoring Agent toocollect zabezpečení dat z virtuálních počítačů. Hello zabezpečení data zahrnují informace o konfiguracích zabezpečení, které jsou používané tooidentify ohrožení zabezpečení, a události zabezpečení, které jsou používané toodetect hrozeb. Údaje shromážděné agentem hello je uložen v existující toohello připojené pracovní prostor analýzy protokolů virtuálního počítače nebo nový pracovní prostor vytvořené Security Center. Security Center vytvoří nový pracovní prostor, informace o zeměpisné poloze hello hello virtuálního počítače je vzít v úvahu.

> [!NOTE]
> Hello agenta Microsoft Monitoring Agent je hello používá stejné agenta službou hello Operations Management Suite (OMS), analýzy protokolů a System Center Operations Manager (SCOM).
>
>

Při shromažďování dat je povolené pro hello poprvé nebo při migraci vašich předplatných, Security Center toosee ověří, zda se hello agenta Microsoft Monitoring Agent je již nainstalována jako rozšíření, která byla na všech virtuálních počítačů Azure. Hello agenta Microsoft Monitoring Agent není nainstalován, pak bude Security Center:

- Instalace agenta Microsoft Monitoring hello v hello virtuálních počítačů
   - Pokud pracovní prostor vytvořené Security Center již existuje v hello stejné informace o zeměpisné poloze jako virtuální počítač, hello hello je agent připojené toothis prostoru
   - pracovní prostor neexistuje, Security Center vytvoří novou skupinu prostředků a výchozí pracovního prostoru v této informace o zeměpisné poloze a připojit hello agenta toothat prostoru. zásady vytváření názvů Hello hello pracovní prostor a prostředek skupiny jsou:

       Pracovní prostor: DefaultWorkspace-[ID předplatného]-[geograficky]

       Skupina prostředků: DefaultResouceGroup-[geograficky]
- Instalace řešení Security Center v prostoru hello

umístění Hello hello pracovního prostoru je založena na umístění hello hello virtuálních počítačů. Další, najdete v části toolearn [zabezpečení dat](security-center-data-security.md).

> [!NOTE]
> Migrace předchozí tooplatform Security Center shromážděná data zabezpečení z virtuálních počítačů pomocí hello Azure Monitoring Agent a data byla uložena ve vašem účtu úložiště. Po migraci platformy hello Security Center používá hello agenta Microsoft Monitoring Agent a toocollect prostoru a úložiště hello stejná data. účet úložiště Hello může být po migraci hello odebrána.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-hello-workspaces-created-by-security-center"></a>Mě I účtovány poplatky za analýzy protokolů nebo OMS na hello pracovních prostorů vytvořit pomocí služby Security Center?
Ne. Pracovní prostory, které jsou vytvořené pomocí služby Security Center, zatímco nakonfigurovaný pro OMS na uzlu fakturace, nevznikají OMS poplatky. Fakturace Security Center je vždy založené na vaší Security Center zásady a hello řešení zabezpečení v pracovním prostoru nainstalován:

- **Úroveň Free** – Security Center nainstaluje hello 'SecurityCenterFree' řešení na hello výchozí pracovní prostor. Fakturuje nejsou pro úroveň Free hello.
- **Úroveň standard** – Security Center nainstaluje hello 'SecurityCenterFree' a 'Zabezpečení' řešení na hello výchozí pracovní prostor.

Další informace o cenách najdete v tématu [Security Center ceny](https://azure.microsoft.com/pricing/details/security-center/). ceny adresy stránku Hello změní toosecurity úložiště dat a poměrné fakturace od června 2017.

> [!NOTE]
> cenová úroveň OMS Hello pracovních prostorů vytvořit pomocí služby Security Center neovlivňuje fakturace Security Center.
>
>

### <a name="can-i-delete-hello-default-workspaces-created-by-security-center"></a>Můžete odstranit hello výchozí pracovních prostorů vytvořit pomocí služby Security Center?
**Odstraňuje se pracovní prostor výchozí hello se nedoporučuje.** Security Center používá hello výchozí pracovní prostory toostore zabezpečení data z virtuálních počítačů.  Pokud odstraníte pracovního prostoru, Security Center je nelze toocollect tato data a některé zabezpečení doporučení a výstrahy nejsou k dispozici

toorecover, hello odebrat agenta Microsoft Monitoring Agent do hello virtuální počítače připojené toohello odstranit pracovní prostor. Security Center přeinstaluje hello agenta a vytvoří nový výchozí pracovní prostory.

### <a name="what-if-hello-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-hello-vm"></a>Co když hello agenta Microsoft Monitoring Agent byla již nainstalována jako rozšíření na hello virtuálního počítače?
Security Center nemůže přepsat existující pracovní prostory toouser připojení. Security Center ukládá data zabezpečení z hello virtuálních počítačů v pracovním prostoru hello již připojen.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-hello-machine-but-not-as-an-extension"></a>Co když měl Microsoft Monitoring Agent nainstalovaný na počítači hello, ale ne jako rozšíření?
Pokud je hello agenta Microsoft Monitoring Agent nainstalován přímo na hello virtuálních počítačů (ne jako Azure rozšíření), Security Center nenainstaluje agenta Microsoft Monitoring Agent hello a sledování zabezpečení bude omezena.

### <a name="what-is-hello-impact-of-removing-these-extensions"></a>Co je hello dopady odebrání těchto rozšíření?
Pokud odeberete hello rozšíření služby Microsoft Monitoring, Security Center není možné toocollect data zabezpečení z hello virtuálních počítačů a některé zabezpečení doporučení a výstrahy nejsou k dispozici. Do 24 hodin Security Center zjistí, že hello virtuálních počítačů chybí hello rozšíření, a přeinstaluje hello rozšíření.

### <a name="how-do-i-stop-hello-automatic-agent-installation-and-workspace-creation"></a>Jak zabráním instalace agentů pro automatickou diagnostiku hello a vytváření pracovního prostoru?
Shromažďování dat můžete vypnout pro vaše předplatná v zásadách zabezpečení hello ale to nedoporučujeme. Vypnutí výstrahy a omezení shromažďování dat doporučení služby Security Center. Shromažďování dat je vyžadován pro odběry hello standardní cenovou úroveň. shromažďování dat toodisable:

1. Pokud váš odběr je nakonfigurován pro úroveň Standard hello, otevřete hello zásady zabezpečení pro toto předplatné a vyberte hello **volné** vrstvy.

   ![Cenová úroveň][1]

2. V dalším kroku vypnutí shromažďování dat tak, že vyberete **vypnout** na hello **zásady zabezpečení – shromažďování dat** okno.

   ![Shromažďování dat][2]

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Odebrání OMS rozšíření nainstalované pomocí služby Security Center
Hello agenta Microsoft Monitoring Agent můžete odebrat ručně. Toto se nedoporučuje, protože se omezuje doporučení služby Security Center a výstrahy.

> [!NOTE]
> Pokud se shromažďování dat je povolené, bude Security Center přeinstalujte agenta hello po jeho odebrání.  Je nutné toodisable shromažďování dat před ručním odebráním hello agenta. V tématu [jak zastavit hello agentů pro automatickou diagnostiku instalace a prostoru vytváření?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) pokyny pro zakázání shromažďování dat.
>
>

toomanually odebrání agenta hello:

1.  Hello portálu, otevřete **analýzy protokolů**.
2.  V okně Log Analytics hello vyberte pracovní prostor:
3.  Vyberte každý virtuální počítač, že nechcete toomonitor použít a vyberte **odpojení**.

   ![Odebrat agenta hello][3]

> [!NOTE]
> Pokud virtuální počítač s Linuxem již má jiné rozšíření agenta OMS, odebrání hello rozšíření odebere také hello agenta a hello zákazník má tooreinstall ho.
>
>

## <a name="existing-oms-customers"></a>Stávající zákazníky služby OMS

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Security Center přepsání všech existujících připojení mezi virtuální počítače a pracovní prostory?
Pokud virtuální počítač už má hello Microsoft Monitoring Agent nainstalována jako rozšíření Azure, Security Center nemůže přepsat existující připojení prostoru hello. Místo toho Security Center používá hello existujícímu pracovnímu prostoru.

Řešení Security Center je nainstalován v prostoru hello není-li již k dispozici a hello řešení je použité toohello pouze relevantní virtuální počítače. Když přidáte řešení, je automaticky nasadit ve výchozí tooall Windows a Linux agenty připojené tooyour pracovní prostor analýzy protokolů. [Cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md), což je funkce OMS, vám umožní tooapply obor tooyour řešení.

Pokud je hello agenta Microsoft Monitoring Agent nainstalován přímo na hello virtuálních počítačů (ne jako Azure rozšíření), Security Center nenainstaluje agenta Microsoft Monitoring Agent hello a sledování zabezpečení je omezená.

### <a name="what-should-i-do-if-i-suspect-that-hello-data-platform-migration-broke-hello-connection-between-one-of-my-vms-and-my-workspace"></a>Co mám dělat, když je pravděpodobné, že migrace platformy hello dat překročila hello připojení mezi jeden z virtuálních počítačů a pracovního prostoru?
To nemělo stát. Pokud k tomu dojít, pak [vytvoření žádosti o podporu Azure](../azure-supportability/how-to-create-azure-support-request.md) a zahrnují hello následující podrobnosti:

- ID prostředku Azure Hello hello dopad na počítač
- ID prostředku Azure Hello pracovního prostoru hello nakonfigurovaná na rozšíření hello předtím, než bylo přerušeno hello připojení
- Hello agent a verzi, která byla dříve nainstalovaná

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-hello-billing-implications"></a>Security Center se nenainstaluje řešení v mé existující pracovní prostory OMS? Jaké jsou důsledky fakturace hello?
Pokud Security Center identifikuje, že virtuální počítač je už připojené tooa prostor, který jste vytvořili, Security Center umožňuje řešení na tento pracovní prostor podle tooyour cenová úroveň. Hello řešení jsou použité toohello pouze relevantní virtuální počítače Azure, prostřednictvím [cílení na řešení](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), takže fakturace zůstane hello hello stejné.

- **Úroveň Free** – Security Center nainstaluje hello 'SecurityCenterFree' řešení v prostoru hello. Fakturuje nejsou pro úroveň Free hello.
- **Úroveň standard** – Security Center nainstaluje hello 'SecurityCenterFree' a 'Zabezpečení' řešení na hello pracovního prostoru.

   ![Řešení na výchozí pracovní prostor][4]

> [!NOTE]
> Hello řešení, zabezpečení, v analýzy protokolů je řešení auditu v OMS & hello zabezpečení.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-toocollect-security-data"></a>Už mám pracovní prostory v mé prostředí, lze použít je toocollect zabezpečení dat?
Pokud virtuální počítač už má hello Microsoft Monitoring Agent nainstalována jako rozšíření Azure, Security Center používá existující připojené prostoru hello. Řešení Security Center je nainstalován v prostoru hello není-li již k dispozici a hello řešení je použité toohello pouze relevantní virtuální počítače prostřednictvím [cílení na řešení](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

Pokud Security Center nainstaluje hello agenta Microsoft Monitoring Agent na virtuálních počítačích, používá hello výchozí pracovních prostorů vytvořit pomocí služby Security Center. Zákazníci brzy bude možné tooconfigure se používají které pracovních prostorů.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-hello-billing-implications"></a>Už mám řešení zabezpečení na osobní pracovní prostory. Jaké jsou důsledky fakturace hello?
Hello řešení a Audit zabezpečení je použité tooenable Security Center standardní funkce úrovně pro virtuální počítače Azure. Pokud řešení zabezpečení a Audit hello je již nainstalován v pracovním prostoru, Security Center používá hello existující řešení. Neexistuje žádná změna v fakturace.

## <a name="next-steps"></a>Další kroky
toolearn informace o migraci platformy hello Security Center, najdete v části

- [Migrace pro platformu Azure Security Center](security-center-platform-migration.md)
- [Příručka pro řešení potíží s Azure Security Center](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
