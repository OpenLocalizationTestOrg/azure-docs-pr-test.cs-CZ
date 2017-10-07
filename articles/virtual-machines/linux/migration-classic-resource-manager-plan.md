---
title: "aaaPlanning pro migraci prostředků IaaS z classic tooAzure Resource Manager | Microsoft Docs"
description: "Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/01/2017
ms.author: kasing
ms.openlocfilehash: 53c6f640425b69cae2ef10afb8c92b8ac4394267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-tooazure-resource-manager"></a>Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager
Azure Resource Manager nabízí mnoho úžasné funkcí, je důležité tooplan se vaše toomake cesty migrace, které zda věcí bezproblémové. Výdaje čas na plánování zajistí nedochází chybám při provádění migrace aktivity. 

> [!NOTE] 
> Hello pokynů bylo výraznou přidružených tooby hello Azure zákazníka poradní tým a práce s zákazníků v migraci velké enviornments architekty cloudové řešení. Tento dokument jako takový bude tooget aktualizovat, protože nové vzorce úspěch vznikat, tak zaškrtnutí zpět z času tootime toosee Pokud neexistují žádné nová doporučení.

Existují čtyři obecné fáze hello migrace cesty:

![Migrace fáze](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Plánování

### <a name="technical-considerations-and-tradeoffs"></a>Technické aspekty a kompromisy

V závislosti na velikosti technické požadavky, zeměpisných a provozní postupy můžete chtít tooconsider:

1. Proč se Azure Resource Manager požaduje pro vaši organizaci?  Jaké jsou hello obchodních důvodů, proč pro migraci?
2. Jaké jsou hello technických důvodů pro Azure Resource Manager?  Co (pokud existuje) další služby Azure, by vám jako tooleverage?
3. Které aplikace (nebo sady virtuálních počítačů) je součástí hello migrace?
4. Jaké scénáře jsou podporovány migrace hello rozhraní API?  Zkontrolujte hello [nepodporované funkce a konfigurace](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations).
5. Týmům provozní nyní podporuje aplikace nebo virtuální počítače v Classic a Azure Resource Manager?
6. Jak (pokud vůbec) Azure Resource Manager mění nasazení virtuálního počítače, správu, monitorování a vytváření sestav procesy?  Mají skripty nasazení toobe aktualizovat?
7. Co je komunikace hello plánu tooalert zúčastněných (koncovým uživatelům, vlastníci aplikace a vlastníkům infrastruktury)?
8. V závislosti na složitosti hello hello prostředí mělo by být období údržby kde hello aplikace je k dispozici tooend uživatelů a vlastníci tooapplication?  Pokud ano, jak dlouho?
9. Co je hello zúčastněným stranám tooensure plán školení jsou znalostmi a znalosti ve službě Správce prostředků Azure?
10. Co je pro správu programu hello nebo plánu projektu správy pro migraci hello?
11. Jaké jsou hello časové osy pro hello při migraci správce prostředků Azure a další související technologie silniční mapy?  Je jejich optimálně zarovnán?

### <a name="patterns-of-success"></a>Vzory úspěch

Úspěšné zákazníkům obsahují podrobné plány, kde jsou popsané, zdokumentované a řídí hello výše otázky.  Zkontrolujte, zda hello migrace plány jsou široce informovat toosponsors a zúčastněnými stranami.  Vybavit sami s informací o možnostech migrace; čtení tímto dokumentem migrace nastavit níže důrazně doporučujeme.

* [Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Pomocí prostředí PowerShell toomigrate IaaS prostředky z classic tooAzure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Pomocí rozhraní příkazového řádku toomigrate IaaS prostředky z classic tooAzure Resource Manager](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Komunita nástroje asistence s migrace prostředky infrastruktury z classic tooAzure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Běžné chyby při migraci](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="pitfalls-tooavoid"></a>Nástrahy tooavoid

- Tooplan selhání.  kroky technologie Hello této migrace jsou ověřené a výsledek hello je předvídatelný.
- Předpokládá, že hello platformy podporované migrace API bude účet pro všechny scénáře. Čtení hello [nepodporované funkce a konfigurace](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) toounderstand, jaké postupy se podporují.
- Není plánování potenciální výpadek aplikace pro koncové uživatele.  Plánování nedostatek vyrovnávací paměti tooadequately upozornit koncoví uživatelé doba potenciálně není k dispozici aplikace.


## <a name="lab-test"></a>Testovací laboratoře 

**Replikovat vaše hodnotit a provést migraci testu**
  > [!NOTE]
  > Přesný replikace vaše stávající prostředí se spustí pomocí nástroje podílí komunity, který není oficiálně podporován Microsoft Support. Proto je **volitelné** krok, ale je hello nejlepší způsob, jak toofind se problémy bez zásahu do provozní prostředí. Pokud pomocí nástroje podílí komunity není možné, přečtěte si o hello ověřením a příprava nebo zrušit vyzkoušet vysílání doporučení níže.
  >
  
  Provádění testu testovacího prostředí vaší přesný scénář (výpočty, síť a úložiště) je nejlepší způsob, jak tooensure hello bezproblémovou migraci. To pomůže zajistit:

  - Zcela samostatné testovacího prostředí nebo existující tootest mimo produkční prostředí. Doporučujeme, abyste zcela samostatné testovacím prostředí, které mohou být migrovány opakovaně a nelze jej destructively změnit.  Skripty toocollect nebo hydrát metadata z reálného odběry hello jsou uvedeny níže.
  - Je vhodné toocreate hello testovacího prostředí v samostatné předplatné. Hello důvod je, že se opakovaně deaktivuje hello testovacího prostředí a s samostatné, izolované předplatného se sníží riziko hello že něco skutečné získá ať už náhodně odstranit.

  To můžete udělat pomocí nástroje AsmMetadataParser hello. [Další informace o tomto nástroji sem](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Vzory úspěch

Následující Hello byly problémy zjištěné v mnoha hello větší migrace. Nejedná se o vyčerpávající seznam a by měla odkazovat toohello [nepodporované funkce a konfigurace](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) další podrobnosti. Může nebo nemusí dojde tyto technické problémy, ale v takovém případě tyto před pokusem o migraci řešení zajistí plynulejší práci.

- **Proveďte vyzkoušet vysílání ověřením a příprava nebo zrušit** -Toto je možná hello nejdůležitější krok tooensure Classic tooAzure Resource Manager migrace úspěch. migrace Hello rozhraní API obsahuje tři hlavní kroky: ověření, přípravě a provést. Ověření se načíst stav hello prostředí classic a vrátit výsledek všechny problémy. Ale vzhledem k tomu, že některé problémy mohou existovat v zásobníku hello Azure Resource Manager, ověřit nebude catch vše. Hello dalším krokem v procesu migrace, Příprava vám pomůže zveřejnění těchto problémů. Příprava bude přesunout hello metadata z klasického tooAzure Resource Manager, ale nebude potvrzení hello přesunout a bude odebrat ani změnit nic sám o hello Classic straně. Hello vyzkoušet vysílání zahrnuje Příprava hello migrace, pak přerušení (**není potvrzení**) Příprava migrace hello. cílem Hello ověření a příprava nebo zrušit vyzkoušet vysílání je toosee všechny hello metadat v zásobníku hello Azure Resource Manager, jej prozkoumat (*prostřednictvím kódu programu nebo portálu*) a ověřte, že všechno migruje správně a fungovat prostřednictvím technické problémy.  Je také získáte představu o dobou trvání migrace, podle toho naplánovat výpadek.  Ověření a příprava nebo zrušit nezpůsobí žádné výpadky uživatele; Proto je omezovaly tooapplication využití.
  - následující položky Hello potřebovat toobe vyřeší před hello vyzkoušet vysílání, ale testu vyzkoušet vysílání se taky bezpečně vyprázdní se tyto přípravné kroky, pokud jejich provedena. Během migrace enterprise jsme zjistili hello vyzkoušet vysílání toobe přípravu migrace tooensure bezpečné a neocenitelnou pomocí způsobem.
  - Při přípravě běží hello řízení roviny (Azure management operace) nebude možné hello celý virtuální síti, takže provedeny žádné změny může být tooVM metadata během ověření a příprava nebo zrušit.  Jinak, ale všechny funkce aplikací (VP, virtuálních počítačů využití, atd.), zůstanou beze změn.  Uživatelé hello virtuální počítače nebude vědět, že tento hello vyzkoušet vysílání je spouštěna.

- **Express okruhy směrování a síť VPN**. Aktuálně Express trasy brány s autorizace odkazy se nedají migrovat bez výpadků. Alternativní řešení hello, najdete v části [migrovat ExpressRoute okruhy a přidružené virtuální sítě z modelu nasazení Resource Manager classic toohello hello](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **Rozšíření virtuálního počítače** -rozšíření virtuálního počítače jsou potenciálně jeden z největších toomigrating roadblocks hello spuštěných virtuálních počítačů. Náprava rozšíření virtuálního počítače může trvat upwards of 1 – 2 dny, takže Plánujte odpovídajícím způsobem.  Funkční Azure agent je potřebné tooreport back stav rozšíření virtuálního počítače spuštěných virtuálních počítačů. Pokud hello stav, je vrácen chybný pro spuštění virtuálního počítače, to se zastaví migrace. vlastní Hello agent není nutné toobe migraci tooenable fungují správně, ale pokud rozšíření existují na hello virtuální počítač, pak oba pracovní agenta a odchozí připojení k Internetu (pomocí DNS), bude potřeba pro migraci toomove dál.
  - Pokud během migrace, všechna rozšíření virtuálních počítačů s výjimkou BGInfo v1 dojde ke ztrátě připojení k serveru DNS tooa. \* potřebovat toofirst odeberou z každý virtuální počítač před Příprava migrace a následně znovu přidat zpět toohello virtuálních počítačů po migraci správce prostředků Azure.  **Toto je pouze pro virtuální počítače, které jsou spuštěné.**  Pokud jsou zastaveny deallocated hello virtuálních počítačů, rozšíření virtuálního počítače není nutné toobe odebrat. **Poznámka:** mnoho rozšíření, jako jsou Azure diagnostics a security center monitorování bude přeinstalovat sami po migraci, odebrání, takže se nejedná o problém.
  - Kromě toho se ujistěte, skupiny zabezpečení sítě nejsou omezení odchozí přístup k Internetu. K tomu dochází s některé konfigurace skupin zabezpečení sítě. Odchozí přístup k Internetu (a DNS) je potřeba pro rozšíření virtuálního počítače toobe migrovat tooAzure Resource Manager. 
  - Existují dvě verze hello rozšíření BGInfo: v1 a v2.  Pokud hello virtuálního počítače byl vytvořen pomocí portálu classic hello nebo prostředí PowerShell, hello virtuálních počítačů pravděpodobně mít hello v1 rozšíření na něm. Toto rozšíření není nutné toobe odebrána a bude vynechán. (není migrovat) hello migrace rozhraní API. Ale pokud hello klasické virtuální počítač byl vytvořen s hello nový portál Azure, bude pravděpodobně mít hello na základě JSON v2 verzi BGInfo, může být migrované tooAzure Resource Manager poskytuje hello agent funguje a má odchozí přístup k Internetu (a DNS). 
  - **Možnost nápravy 1**. Pokud víte, že virtuální počítače nebudou mít odchozí přístupem k Internetu, funkční službu DNS a práci agenty Azure na virtuálních počítačích hello, pak odinstalovat všechny rozšíření virtuálního počítače jako součást migrace hello před Příprava, znovu nainstalujte rozšíření virtuálního počítače hello po migraci. 
  - **Možnost nápravy 2**. Pokud rozšíření virtuálního počítače z mezní příliš velký, Další možností je tooshutdown nebo navrácení všechny virtuální počítače před migrací. Migrace hello navrácena virtuální počítače a potom je znovu spustit na hello straně Azure Resource Manager. Zde Hello výhodou je, že bude migrovat rozšíření virtuálního počítače. Hello nevýhodou je, že budou ztraceny všechny veřejných virtuálních IP adres (to může být jiný starter), a samozřejmě hello virtuální počítače vypne způsobuje mnohem větší vliv na pracovní aplikace.

    > [!NOTE] 
    > Pokud zásady služby Azure Security Center je nakonfigurovaný s hello spuštěných virtuálních počítačů se migruje, zásady zabezpečení hello musí toobe zastaven před odebráním rozšíření, jinak hello zabezpečení, které rozšíření monitorování bude přeinstalaci automaticky na hello virtuálních počítačů po odebráním.

- **Skupiny dostupnosti** – pro virtuální síť (vNet) toobe migrovat tooAzure Resource Manager, hello nasazení (tj. Cloudová služba) obsažené klasické virtuální počítače musí být v jedné sadě dostupnosti nebo hello virtuální počítače musí být všechny není v žádné sadě dostupnosti. S více než jeden dostupnosti v rámci hello cloudové služby není kompatibilní s Azure Resource Manager a migrace se zastaví.  Kromě toho nemůže být některé virtuální počítače v nastavení dostupnosti a některé virtuální počítače není v nastavení dostupnosti. tooresolve, budete potřebovat tooremediate nebo změnit pořadí cloudové služby.  Naplánujte podle toho, jak to může být časově náročná. 

- **Nasazení Role web nebo Worker** -obsahující webové a pracovní role cloudové služby nelze provést migraci tooAzure Resource Manager. Hello webové/role pracovního procesu musí být nejprve odebrány z virtuální sítě hello předtím, než můžete spustit migraci.  Typické řešením je toojust přesunutí web nebo worker role instance tooa samostatné klasické virtuální síť, která je také okruh ExpressRoute propojené tooan nebo toomigrate hello kód toonewer App Services PaaS (Toto pojednání je nad rámec tohoto dokumentu hello). V dřívějším hello znovu nasadit – případ, vytvořit novou virtuální síť v klasickém, přesunutí nebo opětovném nasazení hello web nebo worker role toothat v nové virtuální sítě a pak odstraňte hello nasazení z virtuální sítě hello přesouvání. Bez nutnosti změn kódu. Hello nové [partnerský vztah virtuální sítě](../../virtual-network/virtual-network-peering-overview.md) schopností může být použité toopeer společně hello klasickou virtuální síť obsahující hello webové/role pracovního procesu a dalším virtuálním sítím v hello stejné oblasti Azure, jako je například hello virtuální sítě se migrovat (**po dokončení migrace virtuální sítě, jako peered virtuální sítě se nedají migrovat**), proto poskytování hello stejné funkce nedošlo ke ztrátě výkonu a žádné postihy latenci nebo šířky pásma. Zadané hello přidání [partnerský vztah virtuální sítě](../../virtual-network/virtual-network-peering-overview.md), nasazení role web nebo worker lze nyní snadno zmírnit a nejsou blokovány hello migrace tooAzure Resource Manager.

- **Azure kvóty správce prostředků** -oblastí Azure mají samostatné kvóty nebo limity pro Classic a Azure Resource Manager. I když ve scénáři migrace není spotřebovávanou nový hardware *(jsme se odkládací existujících virtuálních počítačů z klasického tooAzure Resource Manager)*, kvóty správce prostředků Azure potřebujete ještě další toobe zavedené s dostatečnou kapacitu než migrace můžete začít. V následujícím seznamu jsou, že hello hlavní limity, které jsme viděli způsobit problémy.  Otevřete omezení kvóty podporu lístku tooraise hello. 

    > [!NOTE]
    > Tyto limity potřebovat toobe vyvolána v hello stejné oblasti jako vaše aktuální toobe hodnotit migrovat.
    >

    - Síťová rozhraní
    - Nástroje pro vyrovnávání zatížení
    - Veřejné IP adresy
    - Statické veřejné IP adresy
    - Jádra
    - Network Security Groups (Skupiny zabezpečení sítě)
    - Směrovací tabulky

    Můžete zkontrolovat vaše aktuální kvóty správce prostředků Azure pomocí následujících příkazů s hello nejnovější verzi 2.0 rozhraní příkazového řádku Azure hello.

    **Výpočetní** *(jader, Avaiability sady)*

    ```bash
    az vm list-usage -l <azure-region> -o jsonc 
    ```

    **Síť** *(virtuální sítě, statické veřejné IP adresy, veřejné IP adresy, skupin zabezpečení sítě, síťové rozhraní, nástroje pro vyrovnávání zatížení, směrovací tabulky)*
    
    ```bash
    az network list-usages -l <azure-region> -o jsonc
    ```

    **Úložiště** *(účet úložiště)*
    
    ```bash
    az storage account show-usage
    ```

- **Azure Resource Manager API omezení** – Pokud máte prostředí dostatečně velký (např. > 400 virtuální počítače ve virtuální síti), může dosáhl API hello výchozí omezení pro zápis (aktuálně **1200 zápisů za hodinu**) ve službě Správce prostředků Azure. Před zahájením migrace, by měl zvýšíte tooincrease lístek podpory tento limit pro vaše předplatné.

- **Zřizování vypršení časového limitu stav virtuálního počítače** – v případě, že všechny virtuální počítač má stav hello **zřizování vypršel časový limit**tento toobe musí vyřešit před migrací. toodo Hello jediným způsobem, jak je s výpadky zrušení zřízení nebo reprovisioning hello virtuálních počítačů (delete, zachovat hello disku a znovu vytvořte hello virtuálních počítačů). 

- **Stav virtuálního počítače RoleStateUnknown** – Pokud migrace zastaví z důvodu tooa **Neznámý stav role** chyba zprávy, zkontrolujte hello virtuálního počítače pomocí portálu hello, zkontrolujte je spuštěna. Tato chyba obvykle zmizí jeho vlastní (žádná náprava vyžaduje) po několika minutách a je často typu přechodný často můžou vyskytnout během virtuálního počítače **spustit**, **Zastavit**, **restartujte** operace. **Doporučená praxe:** znovu zkuste migrovat znovu za několik minut. 

- **Fabric Cluster neexistuje** – v některých případech některé virtuální počítače nelze migrovat z různých důvodů liché. Jedním z těchto známé případech je pokud hello virtuální počítač byl nedávno vytvořen (v rámci hello minulého týdne nebo tak) a došlo k tooland Azure clusteru, který ještě není vybaven pro úlohy Azure Resource Manager.  Obdržíte chybu, která uvádí, že **fabric cluster neexistuje** a hello virtuálního počítače nelze migrovat. Čeká se na několik dní se obvykle konkrétní problém vyřešit jako hello clusteru bude brzy získat Azure Resource Manager povolena. Ale jeden okamžitou řešení je příliš`stop-deallocate` hello virtuální počítač, pak pokračovat dál migrace a spustit hello virtuálních počítačů zálohování ve službě Správce prostředků Azure po migraci.

### <a name="pitfalls-tooavoid"></a>Nástrahy tooavoid

- Nezadávejte klávesové zkratky a vynechejte hello ověření a příprava nebo zrušit vyzkoušet vysílání migrace.
- Většina, pokud nejsou všechny surface bude vaše potenciální problémy během hello ověření a příprava nebo zrušit.

## <a name="migration"></a>Migrace

### <a name="technical-considerations-and-tradeoffs"></a>Technické aspekty a kompromisy

Nyní jste připraveni vzhledem k tomu, že jste již dříve pracovali prostřednictvím hello známé problémy s vaším prostředím.

Pro hello skutečné migrace můžete tooconsider:

1. Plánování a plánování hello virtuální síť (nejmenší unit migrace) s zvýšení priority.  Nejprve hello jednoduchý virtuální sítě a průběh s hello další komplikovanější, virtuálních sítí.
2. Většina zákazníků bude mít mimo produkční a provozní prostředí.  Poslední naplánujte produkční.
3. **(VOLITELNÉ)**  Plánu údržby výpadek s dostatkem vyrovnávací paměti v případě, že se vyskytnout potíže se neočekávané.
4. Komunikovat s a zarovnané s vaší týmy podpory v případě, že se vyskytnout potíže.

### <a name="patterns-of-success"></a>Vzory úspěch

Hello technické pokyny z hello výše uvedené části testovací laboratoře by měly být považovány za a omezeny předchozí tooa skutečné migrace.  Migrace hello s odpovídající testování, je ve skutečnosti událostmi.  Pro produkční prostředí může to být užitečné toohave další podporu, jako je například důvěryhodné partnera společnosti Microsoft nebo služby Microsoft Premier.

### <a name="pitfalls-tooavoid"></a>Nástrahy tooavoid

Testování není plně může způsobit problémy a zpoždění v hello migrace.  

## <a name="beyond-migration"></a>Nad rámec migrace

### <a name="technical-considerations-and-tradeoffs"></a>Technické aspekty a kompromisy

Teď, když jste ve službě Správce prostředků Azure, maximalizujte hello platformy.  Čtení hello [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) toofind se o další výhody.

Tooconsider věcí:

- Sdružování hello migrace s ostatními aktivitami.  Většina zákazníků zvolit okna údržby aplikace.  Pokud ano, můžete chtít toouse tento výpadek tooenable další možnosti Azure Resource Manager třeba šifrování a migrace tooManaged disky.
- Pokroku hello technické a obchodních důvodů, proč pro Azure Resource Manager; Povolte hello další služby k dispozici pouze na Azure Resource Manager použitých tooyour prostředí.
- Modernizovat prostředí s PaaS služby.

### <a name="patterns-of-success"></a>Vzory úspěch

Být záměrné na co services nyní chcete tooenable ve službě Správce prostředků Azure.  Mnoho zákazníků najde hello níže atraktivní, kterou pro jejich prostředí Azure:

- [Řízení přístupu podle rolí](../../azure-resource-manager/resource-group-overview.md#access-control).
- [Šablony Azure Resource Manageru pro snazší a víc řízené nasazení](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Značky](../../azure-resource-manager/resource-group-using-tags.md).
- [Ovládací prvek aktivity](../../azure-resource-manager/resource-group-audit.md)
- [Zásady prostředků](../../azure-resource-manager/resource-manager-policy.md)

### <a name="pitfalls-tooavoid"></a>Nástrahy tooavoid

Mějte na paměti, proč jste spustili tento Classic tooAzure Resource Manager migrace cesty.  Jaké byly hello původní obchodních důvodů, proč? Dosáhnout hello obchodního důvodu?


## <a name="next-steps"></a>Další kroky

* [Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Pomocí prostředí PowerShell toomigrate IaaS prostředky z classic tooAzure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Komunita nástroje asistence s migrace prostředky infrastruktury z classic tooAzure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Běžné chyby při migraci](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
