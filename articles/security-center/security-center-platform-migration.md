---
title: aaaAzure migrace platformy Security Center | Microsoft Docs
description: "Tento dokument popisuje způsob toohello některé změny dat Azure Security Center se shromažďují."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: yurid
ms.openlocfilehash: 28cb8d85912a3f62941cf113da51070081b5eda2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-platform-migration"></a>Migrace platformy pro Azure Security Center

Počínaje časná 2017 června, Azure Security Center zavede důležité změny toohello způsob zabezpečení data jsou shromažďována a uložené.  Tyto změny odemknutí nové funkce, jako je hello možnost tooeasily vyhledávání zabezpečení dat a lépe zarovnaná s použitím jiných Azure Správa a monitorování služeb.

> [!NOTE]
> Hello platformy migrace by neměla mít vliv na vaše produkční prostředky a není potřeba z vaší strany žádná akce.


## <a name="whats-happening-during-this-platform-migration"></a>Co se děje během této migrace platformy?

Security Center použil hello Azure Monitoring Agent toocollect zabezpečení dat z virtuálních počítačů. To zahrnuje informace o konfiguracích zabezpečení, které jsou používané tooidentify ohrožení zabezpečení, a události zabezpečení, které jsou používané toodetect hrozeb. Tato data se ukládala ve vašich účtech Storage v Azure.

Do budoucna, že Security Center používá hello Microsoft Monitoring Agent – to je hello používá stejné agenta hello Operations Management Suite a služba analýzy protokolů. Data shromážděná z tohoto agenta je uložen v buď existující *analýzy protokolů* [prostoru](../log-analytics/log-analytics-manage-access.md) přidružený k předplatnému Azure nebo nové pracovních prostorů, s ohledem na zeměpisnou polohu hello účet Dobrý den virtuálních počítačů .

## <a name="agent"></a>Agent

V rámci přechodu hello hello agenta Microsoft Monitoring Agent (pro [Windows](../log-analytics/log-analytics-windows-agents.md) nebo [Linux](../log-analytics/log-analytics-linux-agents.md)) je nainstalována na všech virtuálních počítačích Azure ze kterých aktuálně nejsou shromažďována data.  Pokud má virtuální počítač již hello Microsoft Monitoring Agent nainstalován, hello Security Center využívá hello aktuální nainstalovat agenta.

Dobu (obvykle několik dní) poběží i agenty tooensure vedle sebe s hladkým přechodem bez ztrátě dat. Tato akce povolí Microsoft toovalidate, který hello nový datový kanál je funkční před zrušený použití hello aktuální kanálu. Jednou ověřené hello Azure Monitoring Agent se odebere z virtuálních počítačů. Od vás se nic nevyžaduje. Jakmile budou všichni zákazníci úspěšně migrováni, obdržíte e-mail s oznámením.
 
Není doporučeno odinstalovat hello Azure Monitoring Agent během migrace hello ručně, protože by mohlo vést mezer v datech zabezpečení. Pokud potřebujete další pomoc, obraťte se na [Oddělení zákaznických služeb a podpory společnosti Microsoft](https://support.microsoft.com/contactus/). 

Microsoft Monitoring agenta pro Windows Hello vyžaduje používá port TCP 443, přečtěte si [Průvodce odstraňováním potíží Azure Security Center](security-center-troubleshooting-guide.md) Další informace.


> [!NOTE] 
> Protože hello agenta Microsoft Monitoring Agent může být používán jiných Azure správu a monitorování služeb, hello agent nebude odinstalován automaticky po vypnutí shromažďování dat ve službě Security Center. V případě potřeby však můžete odinstalovat ručně agenta hello.

## <a name="workspace"></a>Pracovní prostor

Jak je popsáno výše, datech shromážděných z hello agenta Microsoft Monitoring Agent (jménem Security Center) jsou uloženy v buď existující analýzy protokolů pracovních prostorů přidružený k předplatnému Azure nebo nové pracovních prostorů, s ohledem na účet hello informace o zeměpisné poloze hello virtuálních počítačů.

V hello portálu Azure můžete procházet toosee seznam vašich analýzy protokolů pracovních prostorů, včetně všech vytvořených pomocí služby Security Center. Pro nové pracovní prostory se vytvoří související skupina prostředků. Obojí se řídí těmito zásadami vytváření názvů:

- Pracovní prostor: *DefaultWorkspace-[ID_předplatného]-[zeměpisné umístění]*
- Skupina prostředků: *DefaultResouceGroup-[zeměpisné_umístění]* 
 
U pracovních prostorů vytvořených službou Security Center se data uchovávají po dobu 30 dnů. Pro existující pracovní prostory uchování vychází z pracovního prostoru hello cenová úroveň.

> [!NOTE]
> Data shromážděná dříve službou Security Center zůstanou ve vašich účtech Storage. Po dokončení migrace hello, můžete odstranit tyto účty úložiště.

### <a name="oms-security-solution"></a>Řešení OMS Security 

Pro zákazníky, kteří řešení OMS Security nemají nainstalované, ho Microsoft nainstaluje v jejich pracovním prostoru, ale bude cílit pouze na virtuální počítače Azure. Nesnažte se toto řešení odinstalovat, protože pokud to provedete z konzoly pro správu OMS, neexistuje žádná možnost automatické nápravy.


## <a name="other-updates"></a>Další aktualizace

Ve spojení s hello platformy migrace jsme se zavedením některé další méně závažné aktualizace:

- Budou podporovány další verze operačních systémů. Viz seznam hello [zde](security-center-faq.md#virtual-machines).
- Hello seznam ohrožení zabezpečení operačního systému bude rozšířena. Viz seznam hello [zde](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- [Ceny](https://azure.microsoft.com/pricing/details/security-center/) se budou účtovat poměrně po hodinách (dříve to bylo po dnech) a díky tomu někteří zákazníci ušetří.
- Shromažďování dat bude potřeba a automaticky povolí pro zákazníky na hello standardní cenovou úroveň.
- Azure Security Center začne zjišťovat antimalwarová řešení, která nebyla nasazena prostřednictvím rozšíření Azure. Jako první bude k dispozici zjišťování nástrojů Symantec Endpoint Protection a Defender pro Windows 2016.
- Zásady prevence a oznámení se dají konfigurovat na hello pouze *předplatné* úroveň, ale ceny můžete nastavit stále v hello *skupiny prostředků* úroveň

