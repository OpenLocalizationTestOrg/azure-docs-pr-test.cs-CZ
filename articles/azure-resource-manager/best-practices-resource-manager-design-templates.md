---
title: "aaaDesign Azure šablon pro komplexní řešení | Microsoft Docs"
description: "Ukazuje osvědčené postupy pro navrhování šablon Azure Resource Manageru pro komplexní scénáře"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: aa45e9a46d79a6336b696cff8fd37f65fa449f11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Vzory návrhu pro šablony Azure Resource Manager při nasazení komplexní řešení
Pomocí flexibilní přístup na základě šablon Azure Resource Manager, můžete nasadit komplexních topologiích rychle a konzistentně. Můžete přizpůsobit tato nasazení snadno jako jádra, které nabídky momentální nebo tooaccommodate varianty pro scénáře outlier nebo zákazníků.

Toto téma je součástí větší dokument White Paper. Stáhnout tooread hello úplné papír [World – třída Azure Resource Manager šablony požadavky a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

Šablony kombinovat hello výhod hello základní Azure Resource Manager s hello přizpůsobivost a čitelnost z JSON JavaScript Object Notation (). Pomocí šablony, můžete:

* Topologie a jejich úloh nasaďte konzistentně.
* Spravovat všechny prostředky v aplikaci společně použití skupin prostředků.
* Použijte pro přístup na základě rolí k řízení (RBAC) toogrant odpovídající přístup toousers, skupiny a služby.
* Použijte označování přidružení toostreamline úloh fakturace souhrny.

Tento článek poskytuje podrobné informace o spotřebě scénáře, architekturu a vzory implementace identifikované při našich relace návrhu a implementace reálného šablony u zákazníků Azure poradní tým (AzureCAT). Daleko od academic, jsou tyto přístupy osvědčené postupy o tom informováni prostřednictvím hello vývoj šablon pro 12 hello horní operačních systémů se systémem Linux technologií, včetně: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, DataStax Enterprise s technologií Apache Cassandra, Elasticsearch, volaných, MongoDB, PostgreSQL, Redis a Nagios. 

Tento článek sdílí tyto osvědčené postupy toohelp architektury šablony Azure Resource Manager třídy world.  

V našem práce se zákazníky jsme určili několik možnosti spotřeby šablony správce prostředků mezi podniky, s systémových Integrátorech (SI) a sdílené svazky clusteru. Hello následující části obsahují základní přehled běžné scénáře a vzory pro typy různých zákazníků.

## <a name="enterprises-and-system-integrators"></a>Podniky a integrátory systémů
Ve velkých organizacích, vidíme běžně dvě příjemci šablony Resource Manageru: vnitřní softwaru vývojové týmy a podnikové IT. Jsme zjistili, že hello scénáře pro hello SIs mapování toohello scénáře pro firmy, takže hello stejné aspekty platí.

### <a name="internal-software-development-teams"></a>Interní softwaru vývojové týmy
Pokud váš tým sama vyvinula softwaru toosupport vaši firmu, šablony poskytují snadný způsob tooquickly nasazení technologie pro použití v řešeních pro specifické pro firmy. Můžete také použít šablony toorapidly vytvořte školení prostředí, které umožňují team členy toogain nezbytné dovednosti.

Můžete použít šablony jako-je rozšířit nebo tvoří jejich tooaccommodate vašim potřebám. Pomocí označování šablony, můžete poskytnout fakturace souhrn různých zobrazení, například jako tým, projektu, jednotlivé a vzdělávání.

Podnikům chtít často vývoj softwaru týmy toocreate šablonu pro konzistentní nasazení řešení. Šablona Hello usnadňuje omezení tak, aby některé položky v rámci prostředí pevná a nelze přepsat. Například banka mohou vyžadovat šablony tooinclude RBAC programátorem nelze zkontrolovat, jestli řešení bankovní účet toosend data tooa osobní úložiště.

### <a name="corporate-it"></a>Podnikové IT
Podniková oddělení IT se obvykle používají šablony pro doručování cloudu možnosti kapacity a hostovaných v cloudu.

#### <a name="cloud-capacity"></a>Kapacita cloudu
Běžný způsob pro podnikové IT skupiny tooprovide kapacity cloudu pro týmy je s "tričko velikostí", které jsou standardní nabídky velikosti například malé, střední a velké. Hello tričko velikost nabídky můžete kombinovat různé typy prostředků a počty při současném poskytování úrovně standardizaci, díky které je možné toouse šablony. šablony Hello poskytovat kapacity konzistentní způsobem, který vynucuje podnikové zásady a používá označování tooprovide vrácení peněz tooconsuming organizace.

Může být například nutné tooprovide vývoj, testovací nebo produkční prostředí v rámci které hello můžete nasadit software vývojové týmy jejich řešení. Hello prostředí má předdefinované síťové topologie a elementy, které hello softwaru vývojové týmy nelze změnit, jako například pravidla, kterými se řídí kontroly toohello přístup veřejného Internetu a paketu. Můžete mít rovněž organizace specifické role u těchto prostředí s odlišné přístupová práva pro prostředí hello.

#### <a name="cloud-hosted-capabilities"></a>Možnosti hostovaných v cloudu
Můžete vytvořit šablony toosupport hostovaných v cloudu funkcí, včetně jednotlivých softwarových balíčků nebo složený nabídky, které nabízí toointernal odvětví podnikání. Příkladem složené nabídky může být analýza jako služby – analýzy, vizualizace a další technologie – doručit v konfiguraci optimalizované, propojených na předdefinované síťové topologie.

Možnosti hostovaných v cloudu se týká hello zabezpečení a důležité informace o roli vymezenému kapacity cloudu hello nabídky na kterém jste vytvořené. Tyto možnosti jsou nabízena, jak jsou, nebo jako spravovanou službu. Pro pozdější hello, se vyžaduje omezeného přístupu role tooenable přístup do hello prostředí pro účely správy.

## <a name="cloud-service-vendors"></a>Dodavatelé cloudové služby
Až po rozhovoru toomany sdílené svazky clusteru, myslíme, že více přístupů, které můžete provést toodeploy služby pro vaše zákazníky a přidružené požadavky.

### <a name="csv-hosted-offering"></a>CSV hostované nabídky
Pokud hostujete vaší nabídky v předplatného Azure, jsou společné hostování dva přístupy: nasazení odlišné nasazení pro každý zákazníka nebo nasazování jednotek škálování, které jsou základem sdílené infrastruktury používá pro všechny zákazníky.

* **Odlišné nasazení pro každého zákazníka.** Odlišné nasazení na zákazníka vyžadují pevné topologie různé známé konfigurace. Tato nasazení může mít jiný virtuální počítač (VM) velikosti, proměnlivým počtem uzlů a různé množství přidruženého úložiště. Označení nasazení se používá pro fakturaci souhrnné jednotlivých zákazníků. RBAC může být povoleno tooallow zákazníkům přístup tooaspects jejich cloudového prostředí.
* **Jednotky škálování ve sdíleném víceklientské prostředí.** Jednotka škálování pro prostředí s více klientů může představovat šablonu. V takovém případě hello stejné infrastruktury, jako je použité toosupport všem zákazníkům. nasazení Hello představují skupinu prostředků, které doručují úroveň kapacity pro hello hostované nabídky, jako je počet uživatelů a počet transakcí. Tyto jednotky škálování jsou zvětšit nebo zmenšit, protože vyžaduje.

### <a name="csv-offering-injected-into-customer-subscription"></a>Nabídka CSV vloženy do předplatné zákazníka
Můžete chtít toodeploy váš software do odběrů vlastníkem koncovým zákazníkům. Můžete vytvořit šablony toodeploy odlišné nasazení do Azure účtu zákazníka.

Tato nasazení používat funkci RBAC, abyste mohli aktualizovat a spravovat nasazení hello v rámci účtu hello zákazníka.

### <a name="azure-marketplace"></a>Azure Marketplace
tooadvertise a prodeje nabídkami přes marketplace, jako je například Azure Marketplace, můžete vyvíjet šablony toodeliver odlišné typy nasazení, které běží v Azure účtu zákazníka. Tato odlišné nasazení mohou obvykle popsané jako velikost trička (malé, střední, velký), typ produktu nebo cílové skupiny (komunity, developer, enterprise) nebo typ funkce (základní, vysoké dostupnosti).  V některých případech se tyto typy umožňují toospecify určité atributy hello nasazení, například typ virtuálního počítače nebo počet disků.

## <a name="oss-projects"></a>Projekty operačních systémů
V rámci projekty s otevřeným zdrojem šablony Resource Manageru povolte komunity toodeploy řešení rychle pomocí osvědčené postupy. Šablony můžete ukládat v úložišti GitHub, hello komunity můžete zkontrolovat, jestli je v čase. Uživatelé nasazení těchto šablon do svých vlastních předplatných Azure.

Hello následujících částech jsou uvedené hello věcí, které budete potřebovat tooconsider před navržením řešení.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Identifikace, co je mimo a uvnitř virtuálního počítače
Při navrhování vaší šablony, je užitečné toolook v hello požadavky z hlediska novinky mimo a uvnitř hello virtuální počítače (VM):

* Mimo znamená hello virtuálním počítačům a dalším prostředkům vaše nasazení, jako je například hello síťové topologie, označování, odkazy na toohello certifikátů nebo tajemství a řízení přístupu na základě rolí. Všechny tyto prostředky jsou součástí šablony.
* Uvnitř znamená hello nainstalovaný software a celkové požadovaného stavu konfigurace. Jiné mechanismy, jako je například rozšíření virtuálního počítače nebo skripty, se používají v celé nebo částečně. Tyto mechanismy může identifikovat a provedený hello šablony, ale nejsou v ní.

Běžné činnosti, které by se "uvnitř hello pole" Příklady-  

* Instalace nebo odebrání rolí a funkcí serveru
* Instalace a konfigurace softwaru na úrovni clusteru nebo uzel hello
* Nasazení webů na webovém serveru
* Nasazení databáze schémata
* Správa registru nebo jiné typy nastavení konfigurace
* Správa souborů a adresářů
* Spuštění, zastavení a správu procesů a služeb
* Správa místních skupin a uživatelských účtů
* Instalaci a správě balíčků (.msi, .exe, yum atd.)
* Spravovat proměnné prostředí
* Spouštění skriptů nativní (prostředí Windows PowerShell, bash, atd.)

### <a name="desired-state-configuration-dsc"></a>Konfigurace požadovaného stavu (DSC)
Přemýšlíte o hello vnitřní stav virtuálních počítačů nad rámec nasazení, budete chtít toomake, že toto nasazení nebude "soubor" z hello konfigurace, který jste definovali a zaškrtnutí do správy zdrojového kódu. Tento přístup zajišťuje vaše vývojáři nebo provozní personál nevytvářejte ad hoc změny tooan prostředí, které nejsou vetted, testovat nebo zaznamenávají do správy zdrojového kódu. Tento ovládací prvek je důležité, protože nejsou hello ručních změn ve správě zdrojového kódu. Nejsou také součástí standardní nasazení hello a ovlivní budoucí automatizované nasazení softwaru hello.

Nad rámec zaměstnancům vnitřní konfigurace požadovaného stavu taky je důležité z hlediska zabezpečení. Hackerům pravidelně zkoušíte toocompromise a zneužití systémy softwaru. Po úspěšné, je běžné tooinstall soubory a v opačném případě změňte stav hello ohrožení zabezpečení systému. Konfigurace požadovaného stavu můžete identifikovat rozdílů mezi hello potřeby a skutečný stav a obnovení známé konfigurace.

Existují rozšíření prostředků pro hello nejoblíbenější mechanismy pro DSC - PowerShell DSC, Chef a Puppet. Každý z těchto rozšíření můžete nasadit hello počáteční stav virtuálního počítače a také být použité toomake se, že hello potřeby, že stav je zachován.

## <a name="common-template-scopes"></a>Běžné obory šablony
V našich zkušeností jsme viděli tři obory šablony klíče řešení vznikat. Tyto tři obory – kapacitu, funkce a řešení pro kompletní – jsou popsané v následující části hello.

### <a name="capacity-scope"></a>Kapacity oboru
Obor kapacity přináší sadu prostředků v topologii standardní, který je předem nakonfigurovaný toobe v souladu s předpisy a zásadami. Příklad nejběžnější Hello nasazuje standardní vývojové prostředí v případě pomocí podnikové IT nebo SI.

### <a name="capability-scope"></a>Obor funkce
Obor funkce se zaměřuje na nasazení a konfiguraci topologie pro danou technologii. Běžné scénáře, včetně technologií, jako je například SQL Server, Cassandra, Hadoop.

### <a name="end-to-end-solution-scope"></a>Komplexní řešení rozsahu
Rozsah začátku do konce řešení je cílem nad rámec jednoho schopnosti a místo toho se zaměřuje na řešení s end tooend skládá z více možností.  

Obor s rozsahem řešení šablony projevuje jako sadu jeden nebo více obor funkce šablon s prostředky pro konkrétní řešení, logiku a požadovaný stav. Příklad šablony obor řešení je šablonu end tooend datového kanálu řešení. Šablona Hello může kombinovat specifické řešení topologie a stavu s více šablon funkce obor řešení například Kafka, Storm a Hadoop.

## <a name="choosing-free-form-vs-known-configurations"></a>Výběr vlastní oproti známé konfigurace
Může zpočátku myslíte šablonu měl dát příjemci hello co nejvíce flexibilitu, ale několik rozhodnutí ovlivnit volbu hello jestli toouse vlastní konfigurace oproti známé konfigurace. Tato část popisuje požadavky zákazníků hello a technické aspekty, které ve tvaru hello přístup sdílené v tomto dokumentu.

### <a name="free-form-configurations"></a>Vlastní konfigurace
Na ploše hello zvukových ideální vlastní konfigurace. Umožňují tooselect typ virtuálního počítače a poskytují libovolný počet uzlů a připojené disky pro tyto uzly – a učinit jako parametry tooa šablonu. Tento přístup je však není ideální pro některé scénáře.

V [velikosti virtuálních počítačů](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), hello různé typy virtuálních počítačů a dostupných velikostí jsou identifikovány a každý z hello počet trvale disky (2, 4, 8, 16 nebo 32), který lze připojit. Každý připojený disk poskytuje 500 IOPS a násobků, které z těchto disků může být ve fondu pro násobitel tohoto počtu IOPS. Například 16 disků může být ve fondu tooprovide 8000 IOPS. Sdružování se provádí s konfigurací v hello operačního systému, pomocí prostorů úložiště Microsoft Windows nebo redundantní pole málo nákladných disků (RAID) v systému Linux.

Vlastní konfigurace umožňuje výběr hello několik instancí virtuálních počítačů, virtuální počítač různé typy a velikosti pro tyto instance různých disků pro hello typ virtuálního počítače a jeden nebo více skriptů tooconfigure hello virtuálních počítačů obsah.

Je běžné, že nasazení může mít víc typů uzlů, jako je hlavní server a datové uzly, tak tuto flexibilitu potřebují je často poskytnuté pro každý typ uzlu.

Když začnete toodeploy clustery žádné násobek, začnete toowork se tyto složité scénáře. Pokud byly nasazení clusteru Hadoop, například s 8 řídicí uzly a uzly 200 dat a ve fondu 4 připojených disků na každý hlavní uzel a ve fondu 16 připojených disků na jeden uzel dat by máte 208 virtuální počítače a toomanage 3,232 disky.

Účet úložiště bude omezení jeho identifikovaných 20 000 transakcí za sekundu omezit, tak, aby měli podívejte se na vytváření oddílů účtu úložiště a výpočty, že toodetermine hello odpovídající počet úložiště účtů tooaccommodate Tato topologie, použijte výše uvedené požadavky. Zadané hello velkého množství kombinace nepodporuje hello volného formátu přístup, dynamické výpočty jsou požadované toodetermine hello příslušné oddíly. Hello jazyk šablony Azure Resource Manager v současné době neposkytuje matematické funkce, takže je nutné provést tyto výpočty v kódu, generování šablonu jedinečné, pevně s hello příslušné podrobnosti.

V podnikovém IT a ve scénářích někdo musí udržovat hello šablony a podporovat hello nasazené topologie pro jeden nebo více organizace. Tato další režii – různých konfigurací a šablony pro každého zákazníka – je daleko od žádoucí.

Můžete použít tyto šablony toodeploy prostředí v rámci předplatného Azure vašich zákazníků, ale podnikové IT týmy i sdílené svazky clusteru obvykle nasazení je do svých vlastních předplatných, pomocí funkce toobill vrácení peněz zákazníků. V těchto scénářích hello cílem je toodeploy kapacity pro několik zákazníků napříč fond odběry a udržování nasazení hustě importují do živelný hello odběry toominimize předplatné růst – to znamená další toomanage odběry. S velikostí skutečně dynamické nasazení dosažení tento typ hustotu vyžaduje pečlivé plánování a další vývoj pro generování uživatelského rozhraní pracovní jménem organizace hello.

Kromě toho nelze vytvářet odběry prostřednictvím volání rozhraní API ale musíte to provést ručně přes portál hello. Při rostoucí hello počet odběrů, všechny výsledné živelný růst předplatné vyžaduje lidského zásahu – nelze je možné automatizovat. S mnoho variabilita hello velikosti nasazení byste měli toopre-provision počet odběrů ručně tooensure odběry jsou k dispozici.

S ohledem na tyto faktory je přitažlivými méně než v první blush skutečně vlastní konfigurace.

### <a name="known-configurations--hello-t-shirt-sizing-approach"></a>Známé konfigurace – hello tričko velikosti přístup
Spíše než nabízí šablonu, která poskytuje celkový flexibilitu a obrovském množství variant, v našich zkušeností běžný vzor je tooprovide hello možnost tooselect známé konfigurace – ve skutečnosti standardní tričko velikosti například izolovaného prostoru, malé, střední a velké. Další příklady tričko velikosti jsou nabídky produktů, jako je například community edition nebo enterprise edition.  V jiných případech může být určeného pro konkrétní úlohy konfigurace technologii – například mapy snížit nebo žádné sql.

Mnoho podnikových IT organizace, operačních systémů dodavatelů a SIs zpřístupnit jejich nabídky dnes tímto způsobem ve virtualizovaných prostředích (podniky) na místě, nebo jako nabídek softwaru jako služby (SaaS), (sdílené svazky clusteru a OSVs).

Tento přístup poskytuje dobrý, známé konfigurace různou velikost předem konfigurovaných pro zákazníky. Bez známé konfigurace musí koncovým zákazníkům určení velikosti clusteru samostatně, zohlednit v omezení prostředků platformy a proveďte matematické tooidentify hello výsledná oddíly účty úložiště a dalším prostředkům (z důvodu toocluster velikost a prostředků omezení). Známé konfigurace povolit zákazníkům tooeasily vyberte hello správnou tričko velikost – to znamená, dané nasazení. Kromě toho toomaking lepší prostředí pro hello zákazníka, malý počet známé konfigurace je snazší toosupport a mohou vám pomoci poskytovat vyšší úroveň hustotou.

O přístupu známé konfigurace zaměřené na velikosti tričko také může mít různý počet uzlů v rámci velikost. Malá velikost trička může být například mezi uzly 3 až 10.  Velikost trička Hello by být navrženou tooaccommodate až too10 uzly a zadejte příjemce hello možnost toomake volného formátu výběr až toohello maximální velikost identifikovat hello.  

Velikost trička, v závislosti na typu úloh, může být více volného formátu ve své podstatě z hlediska hello počet uzlů, které mohou být nasazeny, ale bude mít odlišné uzlu velikosti pracovního vytížení a konfigurace softwaru hello na uzlu hello.

Tričko velikosti podle nabídky produktů, například komunity nebo Enterprise, může mít typy různých prostředků a maximální počet uzlů, které mohou být nasazeny, obvykle svázané toolicensing aspekty nebo dostupnosti funkce napříč hello jiné nabídky.

Bylo možné ošetřit zákazníků s jedinečný variant pomocí šablon na základě JSON hello. Při plánování práce s odlehlé hodnoty, můžete začlenit hello odpovídající plánování a důležité informace pro vývoj, podpory a výpočet nákladů.

Na základě hello zákazníka šablony spotřeba scénáře a požadavky identifikovat při spuštění hello tohoto dokumentu se myslíme, vzor šablony rozložením.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Kapacity a schopností obor řešení šablony
Rozložením poskytuje vývoj tootemplate modulární přístup, který podporuje opakované použití, rozšiřitelnosti, testování a nástrojů. Tato část obsahuje podrobnosti o jak rozložením přístup může být použité tootemplates s rozsahem kapacity nebo funkce.

V tento postup hlavní šablonu přijímá hodnoty parametrů z šablony příjemce, a potom po proudu odkazy tooseveral typů šablon a skripty, jak je uvedeno níže. Parametry, statické proměnné a vygenerovaný proměnné jsou hodnoty používané tooprovide směřující hello propojené šablony.

![Parametry šablony](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Parametry se jí předávají tooa hlavní šablony pak toolinked šablony**

Následující části fokus na hello typů šablon a skripty, které je jediné šabloně rozložit na Hello. Hello části jsou uvedeny přístupy k předání informace o stavu mezi hello šablony. Každý typ skriptu šablony a hello hello obrázku jsou popsány společně s příklady. Kontextová příklad najdete v tématu "jeho uvedení společně: Ukázka implementace" dále v tomto dokumentu.

### <a name="template-metadata"></a>Metadata šablony
Metadata šablony (soubor metadata.json hello) obsahuje dvojice klíč/hodnota, které popisují šablonu ve formátu JSON, který může číst člověkem a systémy softwaru.

![Metadata šablony](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Metadata šablony je popsaný v souboru metadata.json hello**

Agenti softwaru můžete načíst soubor metadata.json hello a publikovat hello informace a šablonu toohello odkaz na webovou stránku nebo adresář. Prvky patří *itemDisplayName*, *popis*, *souhrnné*, *githubUsername*, a *dateUpdated*.

Příklad souboru jsou uvedeny níže v celé jeho šíři.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. hello database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Hlavní šablony
Hlavní šablona Hello přijímá parametry od uživatele, používá proměnné komplexní objekt toopopulate této informace a provede hello propojené šablony.

![Hlavní šablony](./media/best-practices-resource-manager-design-templates/main-template.png)

**hlavní šablonu Hello přijímá parametry od uživatele.**

Jeden parametr, který je k dispozici je typ známé konfigurace také označované jako hello parametr velikosti tričko z důvodu jeho standardizované hodnoty, například malé, střední nebo velké. V praxi můžete tento parametr několika způsoby. Podrobnosti najdete v tématu "Známé konfigurace prostředků šablona" dál v tomto dokumentu.

Některé prostředky nasadí bez ohledu na to hello známé konfigurace určený parametrem uživatele. Tyto prostředky se zřizují pomocí šablony jeden sdílený prostředek a jsou sdíleny jiné šablony, takže hello sdílený prostředek šablony je spustit jako první.

Některé prostředky jsou nasazeny volitelně bez ohledu na zadaný známé konfigurace hello.

### <a name="shared-resources-template"></a>Sdílené prostředky šablony
Tato šablona poskytuje prostředky, které jsou společné pro všechny známé konfigurace. Obsahuje hello virtuální sítě, skupiny dostupnosti a další prostředky, které se vyžadují bez ohledu na to hello známé konfigurace šablony, která je nasazena.

![Šablony prostředků](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Sdílené prostředky šablony**

Názvy prostředků, jako je například hello název virtuální sítě, jsou založené na šabloně hlavní hello. Můžete je zadat jako proměnné v rámci dané šablony nebo přijímat jako parametr od uživatele hello podle požadavků vaší organizace.

### <a name="optional-resources-template"></a>Volitelné prostředků šablony
Šablona Hello volitelné prostředků obsahuje prostředky, které jsou nasazeny prostřednictvím kódu programu na základě hodnoty hello parametr nebo proměnná.

![Volitelné prostředky](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**Volitelné prostředků šablony**

Například můžete použít volitelné prostředků šablony tooconfigure jumpbox, která umožňuje prostředí tooa nasazené nepřímý přístup z hello veřejného Internetu. Parametr nebo proměnná tooidentify byste použili, zda by měla být povolená hello jumpbox a hello *concat* funkce, jako název cílového hello toobuild hello šablony *jumpbox_enabled.json*. Propojování šablony využije hello výsledné proměnné tooinstall hello jumpbox.

Můžete propojit hello šablony volitelné prostředky z několika míst:

* Při nasazení příslušné tooevery vytvořit odkaz řízené parametry z hello sdílených prostředků šablony.
* Pokud příslušné tooselect známé konfigurace – například nainstalovat pouze na velkých nasazeních – vytvořit odkaz řízené parametr nebo proměnná řízené z hello známé konfigurace šablony.

Zda je daný prostředek volitelné nemusí bude týkat hello šablony příjemce ale poskytovatelem hello šablony. Například může potřebovat toosatisfy požadavek konkrétní produkt nebo doplňkem produktu (společné pro sdílené svazky clusteru) nebo zásady tooenforce (common pro službu SIs a enterprise IT skupiny). V těchto případech můžete proměnné tooidentify, zda by měly být nasazeny hello prostředků.

### <a name="known-configuration-resources-template"></a>Známé konfigurace prostředků šablony
V šabloně hlavní hello může být parametr zveřejněné tooallow hello šablony příjemce toospecify toodeploy požadované známé konfigurace. Tato známé konfigurace často používá o přístupu velikost tričko s nastavenou velikostí pevné konfigurace, například izolovaného prostoru, malé, střední a velké.

![Známé konfigurace prostředků](./media/best-practices-resource-manager-design-templates/known-config.png)

**Známé konfigurace prostředků šablony**

Hello tričko velikost přístup se běžně používá, ale parametry hello může představovat libovolnou sadu známé konfigurace. Můžete například zadat sadu prostředí pro podniková aplikace například vývoj, testování a produktu. Nebo můžete ho použít pro toorepresent cloudové služby, jiný škálování jednotky, verze produktu nebo konfigurace produktu, jako je například komunity, Developer a Enterprise.

Stejně jako u hello sdílený prostředek šablony, jsou proměnné předat toohello známé konfigurace šablony z buď:

* Koncový uživatel – to znamená, hello parametry odeslané toohello hlavní šablony.
* Organizace – to znamená, hello variables v šabloně hlavní hello, které představují vnitřní požadavky nebo zásady.

### <a name="member-resources-template"></a>Člen prostředků šablony
V rámci známé konfigurace jsou často jeden nebo více typů člen uzlu zahrnuty. Například s Hadoop máte řídicí uzly a datové uzly. Pokud instalujete MongoDB, máte datové uzly a arbiter. Pokud nasazujete DataStax, máte datové uzly a virtuální počítač s OpsCenter nainstalována.

![Členové zdroje](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Člen prostředků šablony**

Každý typ uzly mají různé velikosti virtuálních počítačů, počtu připojených disků, tooinstall skripty a nastavit hello uzlů, konfigurace portů pro hello virtuálních počítačů, počet instancí a další podrobnosti. Tak, že každý typ uzlu získá svůj vlastní člen prostředku šablony, která obsahuje hello podrobnosti o nasazení a konfiguraci infrastruktury a také provádění toodeploy skripty a nakonfigurujte software v rámci hello virtuálních počítačů.

U virtuálních počítačů jsou obvykle dva typy skripty použité, široce opakovaně použitelný a vlastní skripty.

### <a name="widely-reusable-scripts"></a>Široce opakovaně použitelné skripty
Široce opakovaně použitelné skripty lze napříč více typů šablon. Jeden z hello lepší příklady tyto skripty široce opakovaně použitelné nastaví RAID na discích toopool Linux a získat větší počet IOPS. Bez ohledu na to hello software instalován v hello virtuálních počítačů poskytuje tento skript opakované použití osvědčené postupy pro běžné scénáře.

![Skriptů pro opakované použití](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Šablony členských prostředků můžete volat široce opakovaně použitelné skripty**

### <a name="custom-scripts"></a>Vlastní skripty
Šablony běžně volání jeden nebo více skripty, které instalace a konfigurace softwaru v rámci virtuálních počítačů. Běžný vzor se seznámili s velké topologie, kde jsou nasazeny více instancí jedné nebo více typy členů. Instalační skript se zahájí pro každý virtuální počítač, který může běžet paralelně, za nímž následuje instalační skript, která je volána po všech virtuálních počítačů (nebo všech virtuálních počítačů typu daný člen) jsou nasazeny.

![Vlastní skripty](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Šablony členských prostředků můžete volat skripty pro specifické účely, například konfigurace virtuálního počítače**

## <a name="capability-scoped-solution-template-example---redis"></a>Příklad šablony funkce obor řešení - Redis
tooshow způsobu implementace mohou fungovat, podíváme se na praktický příklad vytvoření šablony, která usnadňuje hello nasazení a konfiguraci Redis v standardní tričko velikosti.  

Pro nasazení hello jsou sadu sdílené prostředky (virtuální sítě, účet úložiště, skupiny dostupnosti) a prostředek volitelné (jumpbox). Existuje více známé konfigurace vyjádřené tričko velikosti (malé, střední, velký), ale každý do jednoho uzlu zadejte. Existují také dva skripty zaměřené na konkrétní účel (instalace, konfigurace).

### <a name="creating-hello-template-files"></a>Vytváření hello šablony souborů
Vytvoříte hlavní šablonu s názvem azuredeploy.json.

Vytvoření sdílené prostředky šablony s názvem sdílené resources.json

Vytvořte volitelné šablony prostředků tooenable hello nasazení jumpbox, s názvem jumpbox_enabled.json

Redis používá jenom jeden uzel typu, takže vytvoříte jeden šablonu prostředků člen s názvem resources.json uzlu.

S Redis má tooinstall každé jednotlivé uzel a potom nastavte hello cluster.  Máte skripty tooaccommodate hello instalace a nastavení, redis-cluster-install.sh a redis-cluster-setup.sh.

### <a name="linking-hello-templates"></a>Propojování hello šablony
Pomocí šablony propojení, hello hlavní šablonu odkazy na šablony toohello sdílené prostředky, které vytváří hello virtuální sítě.

Zda mají být nasazeny jumpbox, přidá se logiky v rámci hello hlavní šablonu tooenable spotřebiteli toospecify šablony hello. *Povoleno* hodnotu hello *EnableJumpbox* parametr označuje tohoto zákazníka hello chce toodeploy jumpbox. Pokud je tato hodnota je zadaný, šablony hello zřetězí *_enabled* jako název příponu tooa základní šablony pro funkci jumpbox hello.

Hlavní šablona Hello platí hello *velké* parametr hodnotu jako název základní šablony tooa příponu tričko velikosti a pak používá tuto hodnotu v šabloně propojení se příliš*technology_on_os_large.json*.

topologie Hello by vypadat podobně jako na obrázku.

![Redis šablony](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Struktura šablony pro šablonu Redis**

### <a name="configuring-state"></a>Konfigurace stavu
Pro hello uzly v clusteru hello existují dva kroky tooconfiguring hello stavu, obě reprezentována skriptů pro konkrétní účel.  Redis nainstaluje "redis-cluster-install.sh" a "redis-cluster-setup.sh" nastavuje hello clusteru.

### <a name="supporting-different-size-deployments"></a>Podpora nasazení jinou velikost
Uvnitř proměnné, hello tričko velikost šablona určuje hello počet uzlů každého typu toodeploy pro hello zadat velikost (*velké*). Se pak nasadí počet instancí virtuálního počítače pomocí smyčky prostředků, poskytuje jedinečné názvy tooresources připojením název uzlu s číslem číselného pořadí z *copyIndex()*. Nemá těchto kroků pro oba virtuální počítače horká a záložním zóny, jak jsou definovány v hello tričko název šablony

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>Rozložením a začátku do konce řešení obor šablony
Šablona řešení s oborem začátku do konce řešení se zaměřuje na-komplexní řešení.  Tento přístup je obvykle složení více šablon funkce obor s další prostředky, logiku a stavu.

Jak je znázorněno v následující obrázek hello hello stejný model používá pro šablony funkce obor je rozšířeno pro šablony s oborem řešení začátku do konce.

Sdílené prostředky šablony a volitelné šablony prostředků slouží hello stejnou funkci jako hello kapacity a schopností obor přístupy šablony, ale jsou určené pro hello end tooend řešení.

Jako obor end tooend řešení šablony také můžete obvykle mají i tričko velikosti, hello známé konfigurace prostředků šablony odráží se požaduje pro danou konfiguraci známé hello řešení.

Hello známé konfigurace prostředků šablony odkazy tooone nebo další schopnosti obor šablony řešení, které jsou relevantní toohello end tooend řešení, jakož i hello šablony členských prostředků, které jsou požadovány pro hello end tooend řešení.

Velikost trička hello hello řešení může být jiný než hello jednotlivé funkce obor šablona, jsou proměnné v rámci hello známé konfigurace prostředků šablony použité tooprovide hello odpovídající hodnoty pro příjem dat schopností obor řešení šablony toodeploy hello odpovídající tričko velikost.

![Klient server](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**Hello model používá kapacity nebo schopností obor řešení šablony lze snadno rozšířit pro koncové tooend řešení šablony obory**

## <a name="preparing-templates-for-hello-marketplace"></a>Příprava šablony hello Marketplace.
Hello předcházející přístup snadno přizpůsobí situací, které podniky, SIs a sdílené svazky clusteru chcete tooeither nasazení šablon hello, sami nebo povolit jejich zákazníky toodeploy na své vlastní.

Další požadované scénář se nasazení šablony prostřednictvím hello marketplace.  Tento přístup rozložením funguje pro hello marketplace taky s menšími změnami.

Jak je uvedeno nahoře, šablony může být odlišné nasazení typy používané toooffer pro prodej hello Marketplace. Typy nasazení DISTINCT může být tričko velikosti (malé, střední, velký), typ produktu nebo cílové skupiny (komunity, developer, enterprise) nebo typ funkce (základní, vysoké dostupnosti).

Řešení nebo funkci, kterou vymezená šablony lze snadno použít jako vidíte níže, hello stávající koncové tooend toolist hello různé známé konfigurace hello Marketplace.

Hello parametry toohello hlavní šablona jsou nejprve změněna tooremove hello příchozí parametr s názvem tshirtSize.

Při různých typů hello namapovat toohello známé konfigurace prostředků šablony, potřebují hello společných prostředků a konfigurace v nalezena hello sdílené prostředky šablony a potenciálně těmi ve volitelné šablony prostředků.

Pokud chcete toopublish vaše šablona toohello marketplace, můžete vytvořit odlišné kopie vaší hlavní šablony, který nahrazuje hello dříve k dispozici příchozí parametr tshirtSize tooa proměnné vložené v šabloně hello.

![Marketplace](./media/best-practices-resource-manager-design-templates/marketplace.png)

**Přizpůsobení řešení obor šablonu pro hello marketplace**

## <a name="next-steps"></a>Další kroky
* toolearn o sdílení stavu do a z šablony, najdete v části [sdílení stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

