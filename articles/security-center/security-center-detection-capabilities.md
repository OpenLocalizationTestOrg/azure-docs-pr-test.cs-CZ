---
title: "Možnosti aaaDetection v Azure Security Center | Microsoft Docs"
description: "Tento dokument vám pomůže toounderstand fungování funkce zjišťování služby Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 4c5599cc-99a1-430f-895f-601615ef12a0
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: d8001cc2acdd0026bd9b3596bbdfec56f8874513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-detection-capabilities"></a>Funkce detekce ve službě Azure Security Center
Tento dokument popisuje možnosti rozšířené zjišťování Azure Security Center, které pomáhá identifikovat active hrozeb cílení na vaše prostředky Microsoft Azure a zajišťuje, že jste se statistickými hello toorespond rychle.

> [!NOTE]
> Pokročilé detekce jsou k dispozici v hello standardní vrstvě z Azure Security Center. K dispozici je bezplatná 60denní zkušební verze. Upgradovat můžete ze hello cenová úroveň výběr v hello [zásady zabezpečení](security-center-policies.md). Navštivte [Security Center stránky](https://azure.microsoft.com/pricing/details/security-center/) toolearn informace o cenách. 
> 
> 

## <a name="responding-tootodays-threats"></a>Reagovat na tootoday hrozeb
Byly významné změny v hello threat šířku přes hello posledních 20 let. V posledních hello společností obvykle pouze bylo tooworry o poškození vzhledu webu jednotlivých útočníků, kteří byli většinou zajímat vidět "co může provádět". Dnešní útočníci jsou mnohem sofistikovanější a organizovanější. Často mají konkrétní finanční a strategické cíle. Mají také další prostředky k dispozici toothem, jak se může z rozpočtu výhod stavy nebo organizované trestné činnosti.

Tento přístup může mít za následek tooan nebývalá úroveň úroveň profesionality v pořadí útočník hello. Již je nezajímá pouhé poškození vzhledu webu. Jsou nyní máte zájem o krádež informace, finanční účty a privátní data – všechny z nich můžete použít toogenerate platba na trhu otevřete hello nebo tooleverage konkrétní obchodní, politickou nebo vojenský pozice. I další týkající se než ty útočníkům finanční cíl hello útočníky, které porušují tooinfrastructure škodu toodo sítě a osoby.

V odpovědi často nasazováním různá řešení bodu, které se zaměřují na nutné chránit hello rozlehlou síť nebo koncových bodů tak, že vyhledá podpisy známé útoky. Tato řešení zpravidla toogenerate k velkému počtu nízkou věrnosti výstrahy, které se vyžadují tootriage analytik zabezpečení a prozkoumat. Většina organizací nedostatku hello čas a znalosti požadované toorespond toothese výstrahy – tak mnoho přejděte nevyřešená.  Mezitím, útočníci vyvinuly jejich metody toosubvert mnoho obrany založené na podpis a [přizpůsobit prostředí toocloud](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nové přístupy jsou požadované toomore rychle identifikovat vznikající hrozby a urychlit zjišťování a odpovědi. 

## <a name="how-azure-security-center-detects-and-responds-toothreats"></a>Jak Azure Security Center zjišťuje a odpoví toothreats
Zabezpečení výzkumníci společnosti Microsoft jsou neustále na hello hledejte hrozeb. Mají přístup tooan rozsáhlou sadu telemetrie získaných globální přítomnosti společnosti Microsoft v hello cloudu a místně. Tuto kolekci dosažení celou a různých datových sad umožňuje Microsoft toodiscover nových vzorů útoků a trendů v jeho místní spotřebních a podnikových produktech, jakož i jeho online služeb. Díky tomu dokáže Security Center rychle aktualizovat své algoritmy detekce spolu s tím, jak útočníci provádějí nové a stále sofistikovanější kousky. Tento přístup pomáhá udržet krok s rychle se rozvíjejícím prostředím hrozeb. 

Detekce hrozeb Security Center funguje tak, že automaticky shromažďování informací o zabezpečení z vaše prostředky Azure, sítě hello a připojených partnerských řešení. Analyzuje tyto informace, často korelace informace z více zdrojů, tooidentify hrozeb. Výstrahy zabezpečení budou mít vyšší prioritu v Centru zabezpečení včetně doporučení, na tom, jak tooremediate hello hrozeb.

![Shromažďování a prezentace dat ve službě Security Center](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Služba Security Center využívá pokročilou analýzu zabezpečení, která daleko překračuje možnosti detekce založené na signaturách či příznacích. Změnám ve velkých objemů dat a [strojového učení](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologie jsou využity tooevaluate události napříč hello celý cloudových prostředků infrastruktury – zjišťování hrozeb, které by bylo možné tooidentify ruční přístupy a predikci hello vývoj útoků. Do této analýzy zabezpečení patří: 

* **Integrované analýzou hrozeb**: vypadá pro hello známé nesprávnými účastníky s využitím globální analýzou hrozeb z produktů společnosti Microsoft a služeb společnosti Microsoft digitální činů jednotky (DCU), hello Microsoft Security Response Center (MSRC) a externí informační kanály.
* **Pro vypracování analýzy chování**: platí známé vzorce toodiscover škodlivé chování. 
* **Detekce anomálií**: používá statistické profilace toobuild historických směrného plánu. Zobrazuje výstrahy na odchylky od zavedených standardních hodnot, které odpovídají tooa potenciální vektor útoku.

### <a name="threat-intelligence"></a>Analýza hrozeb
Společnost Microsoft má k dispozici rozsáhlé zdroje globální analýzy hrozeb. Telemetrická data proudí z více zdrojů, jako je Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, hello Microsoft digitální činů jednotky (DCU) a Microsoft Security Response Center (MSRC). Výzkumní pracovníci také přijímat intelligence informací o hrozbách, která je sdílena mezi poskytovatele hlavní cloudových služeb a přihlásí se k odběru informačních kanálů intelligence toothreat od jiných výrobců. Azure Security Center můžete použít tento tooalert informace můžete toothreats ze známé nesprávnými účastníky. Možné příklady:

* **Odchozí komunikaci tooa škodlivé IP adresu**: odchozí přenosy tooa známé botnet nebo darknet pravděpodobně označuje, že došlo k ohrožení zabezpečení prostředku a útočník, který se pokouší tooexecute příkazy v systému nebo exfiltrate dat. Azure Security Center porovnává síťové přenosy tooMicrosoft globální threat databáze a upozorní vás, pokud zjistí komunikace tooa škodlivé IP adresy.

## <a name="behavioral-analytics"></a>Behaviorální analýza
Pro vypracování analýzy chování jde o techniku, která analyzuje a porovná tooa shromažďování dat ze známé vzorů. Tato schémata však nepředstavují jednoduché příznaky. Jsou určené prostřednictvím algoritmy komplexní strojového učení, které jsou použité toomassive datové sady. Určují se také prostřednictvím pečlivé analýzy škodlivého chování, kterou provádí zkušení analytici. Azure Security Center může používat prostředky pro vypracování analýzy chování tooidentify ohrožení zabezpečení na základě analýzy protokolů virtuálního počítače, protokolů zařízení virtuální sítě, infrastruktury protokoly, výpisy stavu systému a jiných zdrojů. 

Kromě toho je korelace s další toocheck signály pro podporu důkaz rozšířeným kampaně. Tato korelace pomáhá tooidentify události, které jsou konzistentní s vytvořeným ukazatele ohrožení zabezpečení. Možné příklady:

* **Provádění podezřelé procesu**: útočníci využít několik technik tooexecute škodlivého softwaru bez detekce. Například útočník může poskytnout malwaru hello stejné názvy jako soubory legitimní systému, ale tyto soubory umístit do alternativního umístění, použijte název, který je velmi podobné tooa neškodné soubor nebo soubor hello maska true rozšíření. Security Center modelů procesů chování a monitorování procesu spuštěních toodetect extrémních takovéto.  
* **Skrytý pokusy o malwaru a jeho zneužití**: sofistikované malwaru je možné tooevade tradiční antimalwarové produkty nikdy zápis toodisk nebo šifrování softwarové komponenty, které jsou uložená na disku.  Však takové malwaru lze zjistit pomocí analýza paměti jako hello malware musí zůstat trasování v paměti toofunction pořadí. Když dojde k chybě softwaru, stav systému zaznamená část paměti v době hello hello havárie.  Analýzou hello paměti v hello stav systému, můžete zjišťovat techniky používá tooexploit ohrožení zabezpečení v softwaru, přístup k důvěrných dat a tajně uchování s – v Azure Security Center ohroženého počítače bez dopadu na výkon hello váš počítač.
* **Pomoci odhalit laterální pohyb a interní rekognoskace**: toopersist v ohrožení zabezpečení sítě a vyhledejte/sklizně cenných dat, útočníci často usilují toomove následně k laterálnímu z hello ohrožené hello tooothers počítače ve stejné síti. Security Center sleduje proces a přihlašovací aktivity v pořadí toodiscover pokusí tooexpand útočníka dostane v rámci hello sítě, jako je například vzdáleného příkazu provádění sítě zjišťování a účtů.
* **Škodlivých skriptů prostředí PowerShell**: prostředí PowerShell je stále používán útočníci tooexecute škodlivý kód na cílových virtuálních počítačů pro jiné účely. Služba Security Center kontroluje aktivitu prostředí PowerShell a hledá známky podezřelé aktivity. 
* **Odchozí útoky**: útočníci často cíle cloudových prostředků s cílem hello pomocí těchto prostředků toomount další útoky. Ohrožené virtuální počítače, například může být použité toolaunch útoky hrubou silou na jiné virtuální počítače, odesílání nevyžádané pošty nebo kontrolovat otevřené porty a hello ostatní zařízení na Internetu. Použitím machine learning toonetwork provoz, Security Center může rozpoznat, kdy se odchozí síťovou komunikaci překročit hello norm. V případě hello nevyžádané pošty, Security Center také korelaci provoz neobvyklou e-mailu s intelligence z Office 365 toodetermine zda hello e-mailu je pravděpodobně nefarious nebo výsledek hello legitimní e-mailové kampaně.  

### <a name="anomaly-detection"></a>Detekce anomálií
Azure Security Center používá také hrozeb tooidentify detekce anomálií. V analýzu toobehavioral kontrast (což závisí na známé vzorce, které jsou odvozené z velkých datových sad) detekce anomálií je více "individuální" a se zaměřuje na standardních hodnot, které jsou specifické tooyour nasazení. Machine learning je použité toodetermine normální aktivity nasazení a potom pravidla jsou generované toodefine outlier podmínky, které by mohl představovat událostí zabezpečení. Zde naleznete příklad:

* **Příchozí útoky hrubou silou RDP/SSH**: Vaše nasazení mohou obsahovat velmi vytížené virtuální počítače s velkým množstvím přihlášení každý den a další virtuální počítače, které mají velmi málo nebo žádná přihlášení. Azure Security Center můžete určit činnost přihlášení standardních hodnot pro tyto virtuální počítače a použít machine learning toodefine, co je mimo normální přihlašovací aktivity. Pokud se významně liší od standardních hodnot hello hello počet přihlášení nebo hello času dne hello přihlášení nebo hello umístění, ze které hello jsou požadovány přihlášení nebo dalších vlastností související s přihlášením, může být vygenerována výstraha. A strojové učení tu zase určuje, co je významné.

## <a name="continuous-threat-intelligence-monitoring"></a>Průběžné monitorování analýzy hrozeb
Azure Security Center funguje zabezpečení dat vědecké účely týmy pro výzkum a které neustále monitorovat změny v šířku threat hello. To zahrnuje následující iniciativy hello:

* **Monitorování analýzy hrozeb**: Analýza hrozeb zahrnuje mechanismy, ukazatele, důsledky a praktické rady týkající se stávajících nebo nově vznikajících hrozeb. Tyto informace jsou sdílena v komunitě hello zabezpečení a Microsoft neustále monitoruje threat intelligence informační kanály z interních a externích zdrojů.
* **Sdílení signálu**: Informace od týmu zabezpečení z širokého portfolia cloudových a místních služeb a serverů společnosti Microsoft a z klientských koncových zařízení se sdílí a analyzují. 
* **Odborníci na zabezpečení společnosti Microsoft**: Průběžné zapojování týmů v rámci společnosti Microsoft, které pracují ve specializovaných oblastech zabezpečení, například forenzní analýza nebo detekce webových útoků.
* **Ladění detekce**: algoritmy, které se spouští nad skutečné zákazníka datových sad a výzkumných pracovníků zabezpečení pracovat s výsledky hello toovalidate zákazníků. Hodnota TRUE a false pozitivních jsou algoritmů strojového učení použité toorefine.

Tyto společného úsilí případech v nových a vylepšených detekce, které můžete využívat výhod okamžitě – neexistuje žádná akce pro tootake je.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak fungují možností detekce tooAzure Security Center. toolearn Další informace o Security Center, najdete v části hello následující:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Správa a zpracování výstrah toosecurity v Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Výstrahy zabezpečení podle typu ve službě Azure Security Center](security-center-alerts-type.md)
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

