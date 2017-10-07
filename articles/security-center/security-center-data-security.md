---
title: "aaaAzure zabezpečení dat Security Center | Microsoft Docs"
description: "Tento dokument popisuje způsob správy a ochrany dat ve službě Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: yurid
ms.openlocfilehash: 30f8b11272dc5df6d485608abdaa62ba57e63f23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-data-security"></a>Zabezpečení dat ve službě Azure Security Center
toohelp zákazníkům zabránit, zjistit a reagovat toothreats, Azure Security Center shromažďuje a zpracovává data týkající se zabezpečení, včetně informací o konfiguraci, metadata, protokoly událostí, soubory se stavem systému a další. Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby.

Tento článek popisuje způsob správy a ochrany dat ve službě Azure Security Center.

>[!NOTE] 
>Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data. V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další. Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>


## <a name="data-sources"></a>Zdroje dat
Azure Security Center analyzuje data z hello následující zdroje tooprovide viditelnost do vaší stavu zabezpečení, zjištění chyb zabezpečení a doporučujeme způsoby zmírnění rizik a detekovat hrozby aktivní:

- Azure Services: Používá informace o konfiguraci hello služeb Azure, že jste nasadili navázat komunikaci s poskytovatelem prostředků dané služby.
- Síťový provoz: Využívá vzorkovaná metadata síťového provozu z infrastruktury společnosti Microsoft, jako je třeba zdrojová a cílová IP adresa/port, velikost paketu nebo síťový protokol.
- Partnerská řešení: Využívá výstrahy zabezpečení ze všech integrovaných partnerských řešení, jako jsou třeba brány firewall a antimalwarová řešení. 
- Virtuální počítače a servery: Využívá z vašich virtuálních počítačů konfigurační informace a informace o událostech zabezpečení, jako jsou třeba protokoly událostí a auditů systému Windows, protokoly IIS, zprávy syslog a soubory se stavem systému. Kromě toho při vytvoření výstrahy, Azure Security Center může generovat snímků disku virtuálního počítače hello vliv a extrahovat výstrahy související toohello artefakty počítače z disku hello virtuálních počítačů, jako je například soubor registru pro forenzní účely.


## <a name="data-protection"></a>Ochrana dat
**Oddělení dat**: Data se ukládají na jednotlivých součástí v rámci služby hello logicky samostatné. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu cyklu hello dat a je požadováno v jednotlivých vrstvách služby hello.

**Přístup k datům**: pořadí tooprovide doporučení zabezpečení a prozkoumat potenciální ohrožení zabezpečení, Microsoft pracovníky může přístup k informacím, které jsou shromažďovány nebo analyzovat služby Azure, včetně soubory se stavem systému, zpracování událostí vytváření virtuálních počítačů snímky disku a artefaktů, které můžou neúmyslně obsahovat Data zákazníků nebo osobní data z virtuálních počítačů. Jsme splňovat toohello [Microsoft Online Services podmínky a prohlášení o ochraně osobních údajů](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), který stavu, že Microsoft nebude používat Data zákazníků nebo odvození informací z něj pro obchodní účely reklamy nebo podobné. Data zákazníků používáme pouze jako potřebné tooprovide vám Azure služby, včetně účely, které jsou kompatibilní s poskytování těchto služeb. Zachováte všechny práva tooCustomer Data.

**Data použití**: Společnost Microsoft používá vzory a analýzou hrozeb vidět napříč více klienty tooenhance naše funkce prevence a detekce; jsme učinit v souladu s závazky týkajícími se ochrany osobních údajů hello popsané v našem [ochrany osobních údajů Příkaz](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

## <a name="data-location"></a>Umístění dat

**Vaše pracovních prostorů**: pracovní prostor je zadán pro hello následující oblastech a data shromážděná z Azure virtuální počítače, včetně výpisy stavu systému a některé typy dat výstrah, jsou uloženy v hello nejbližší pracovního prostoru. 

| Geografie virtuálního počítače                        | Geografie pracovního prostoru |
|-------------------------------|---------------|
| Spojené státy, Brazílie, Kanada | Spojené státy |
| Evropa, Spojené království        | Evropa        |
| Asie a Tichomoří, Japonsko, Indie    | Asie a Tichomoří  |
| Austrálie                     | Austrálie     |

 
Disk snímky virtuálních počítačů jsou uložené v hello stejný účet úložiště jako disk hello virtuálních počítačů.
 
Pro virtuální počítače a servery spuštěné v jiných prostředích například na místě, můžete zadat hello prostoru a oblasti, kde je uložen shromážděná data. 

**Úložiště Azure Security Center**: informace o výstrah zabezpečení, včetně partnera výstrahy, jsou uloženy regionální podle umístění toohello hello souvisejících prostředků Azure, zatímco informace o stavu zabezpečení a doporučení je uložený centrálně ve Spojených státech amerických hello nebo Evropa podle toocustomer na umístění.
Azure Security Center shromažďuje dočasné kopie souborů se stavem systému a analyzuje je za účelem detekce stop pokusů o napadení zabezpečení, neúspěšných i úspěšných. Azure Security Center provede tuto analýzu v rámci hello stejném geograficky redundantním jako hello prostoru a odstranění hello dočasné kopie po dokončení analýzy.

Artefakty počítače jsou uložené v centrálně hello stejné oblasti jako hello virtuálních počítačů. 


## <a name="managing-data-collection-from-virtual-machines"></a>Správa shromažďování dat z virtuálních počítačů

Když povolíte službu Security Center v Azure, u každého vašeho předplatného Azure se zapne funkce shromažďování dat. Shromažďování dat můžete také zapnout pro vaše předplatná v hello část zásad zabezpečení služby Azure Security Center. Při shromažďování dat je zapnutá, Azure Security Center zřizuje hello agenta Microsoft Monitoring Agent na všechny stávající podporované virtuální počítače Azure a všechny nové, které jsou vytvořeny. Microsoft Monitoring agent Hello hledá různé zabezpečení související s konfigurací a událostí do [trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) trasování (ETW). Kromě toho hello operačního systému bude vyvolávání událostí v protokolu událostí během hello během spouštění počítače hello. Mezi příklady těchto údajů patří: typ a verze operačního systému, protokoly operačního systému (protokoly událostí systému Windows), spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID klienta. Hello agenta Microsoft Monitoring Agent přečte položky protokolu událostí a trasování událostí pro Windows trasování a zkopíruje je tooyour pracovních prostorů pro analýzu. Hello agenta Microsoft Monitoring Agent také zkopíruje havárií odkládacích souborů tooyour pracovních prostorů.

Pokud používáte Azure Security Center volné, můžete také zakázat shromažďování dat z virtuálních počítačů v hello zásady zabezpečení. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě hello. Shromažďování artefaktů a snímků disku virtuálního počítače bude nadále povolené i v případě, že shromažďování dat je zakázané.


## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli informace o způsobu správy a ochrany ve službě Azure Security Center. toolearn Další informace o službě Azure Security Center, najdete v části:

* [Průvodce Azure Security Center plánováním a provozem](security-center-planning-and-operations-guide.md) – Další informace jak tooplan a pochopit hello návrhu aspekty tooadopt Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů
