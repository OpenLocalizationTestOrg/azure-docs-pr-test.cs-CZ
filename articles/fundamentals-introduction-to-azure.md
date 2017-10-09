---
title: aaaIntro tooMicrosoft Azure | Microsoft Docs
description: "Nové tooMicrosoft Azure? Get základní přehled hello ji obsluhuje nabízí s příklady, jak jsou užitečné."
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: 6791506abe813e19ca7b04d78d35cb46352a9028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-microsoft-azure"></a>Úvod do Microsoft Azure
Microsoft Azure je platforma Microsoftu pro aplikace pro hello veřejného cloudu.  Hello cílem tohoto článku je toogive je základ pro pochopení hello základy Azure, i v případě, že bez znalosti o cloud computing.

**Jak tooread v tomto článku**

Azure roste celou dobu hello tak, aby byl snadno tooget přetížený.  Začněte hello základní služby, které jsou uvedeny jako první v tomto článku a potom přejděte na tooadditional služby. Který neznamená právě hello další služby nelze použít samy o sobě, ale základní služby hello tvoří jádro hello aplikace běžící v Azure.

**Váš názor**

Vaše zpětná vazba je důležité. Tento článek měl dát efektivní Přehled služby Azure. Pokud ne, řekněte nám v sekci komentáře hello v hello dolní části stránky hello. Poskytnout některé podrobnosti k tomu, co jste očekávali toosee a jak tooimprove hello článku.  

## <a name="hello-components-of-azure"></a>Hello součásti Azure
Azure skupiny služeb do kategorií v hello portálu pro správu a v různých vizuálních pomůcek jako hello [co je Azure Infografice](https://azure.microsoft.com/documentation/infographics/azure/) . Hello portálu pro správu se používá toomanage nejvíce (ale ne všechny) služby v Azure.

Tento článek se použije **jiné organizaci** tootalk o služby podle podobné funkce a toocall na důležité dílčí služby, které jsou součástí větší ty.  

![Azure součásti](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Obrázek: Azure poskytuje přístupné z Internetu aplikačních služeb běžících v datových centrech Azure.*

## <a name="management-portal"></a>Portál pro správu
Azure je webové rozhraní s názvem hello [portálu pro správu](http://manage.windowsazure.com) umožňuje správci tooaccess a spravovat většinu, ale všechny Azure funkce.  Společnost Microsoft obvykle vydává hello novější uživatelského rozhraní portálu ve verzi beta před vyřazením z provozu starší. Hello novější se nazývá hello ["Portál Azure Preview"](https://portal.azure.com/).

Je to obvykle dlouhé překrývají oba Portály jsou aktivní. Zatímco základní služby se zobrazí v obou portálů, ne všechny funkce mohou být k dispozici v obou. Novější služby může zobrazí v hello novější portálu první a starší služby a funkce může existovat pouze v hello starší.  uvítací zprávu tady je, že Pokud nenajdete něco hello starší portálu, kontrolují hello novější a naopak.

## <a name="compute"></a>Compute
Jedním z hello nejzákladnější věcí, které nemá cloudové platformy je spuštění aplikace. Každý výpočetní modely hello má svou vlastní role tooplay.

Můžete použít tyto technologie samostatně nebo v kombinaci podle potřeby toocreate hello správné foundation pro vaši aplikaci. přístup Hello zvolíte, závisí na jaké problémy, které se snažíte toosolve.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
![Virtuální počítače Azure ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Obrázek: Virtuální počítače Azure získáte plnou kontrolu nad instancí virtuálního počítače v cloudu hello.*

možnost toocreate Hello virtuálního počítače na vyžádání, zda ze standardní bitové kopie nebo z jednoho zadáte, může být velmi užitečná. Tento přístup, obvykle označuje jako infrastruktura jako služba (IaaS), je, co poskytuje virtuální počítače Azure. Obrázek 2 ukazuje kombinaci, jak běží virtuální počítač (VM) a jak toocreate jeden z disku VHD.  

toocreate virtuální počítač, zadejte velikost které virtuálního pevného disku toouse a hello Virtuálního počítače.  Potom platíte dobu hello této hello, který je spuštěný virtuální počítač. Platíte hello minutu a pouze tehdy, když je spuštěná, přestože je minimální úložiště poplatků za pořízení hello virtuálního pevného disku k dispozici. Azure nabízí stock virtuální pevné disky (nazývané "Image") obsahující toostart spouštěcí operačního systému z galerie. Mezi ně patří společnosti Microsoft a partnera možnosti, například Windows Server a Linux, SQL Server, Oracle a mnoho dalších. Jste volné toocreate virtuální pevné disky a bitové kopie a potom odešlete sami. Můžete nahrát i virtuální pevné disky, které obsahují pouze data a poté přistupovat k nim z spuštěné virtuální počítače.

Bez ohledu na hello virtuálního pevného disku pochází z, můžete uložit trvale veškeré změny provedené spuštěného virtuálního počítače. Hello příštím vytvoření virtuálního počítače z tohoto disku VHD, věcí vyberte tam, kde skončil. Hello virtuální pevné disky, které virtuální počítače zpět hello ukládají do objektů BLOB Azure Storage, které se věnuje později.  To znamená, že získají tooensure redundance, které virtuální počítače nebudou zmizí z důvodu selhání toohardware a disku. Jeho také možné toocopy hello změnit virtuálního pevného disku mimo Azure, a potom ji spustit místně.

Vaše aplikace běží v rámci jednoho nebo více virtuálních počítačů, v závislosti na tom, jak ho před vytvořila nebo rozhodnout toocreate ho od začátku teď.

Tento přístup poměrně obecné toocloud computing lze použít tooaddress mnoho různých problémů.

**Virtuální počítač scénáře**

1. **Vývoj/testování** – můžete je použít toocreate cenově nenáročná vývojová a testovací platforma, která můžete vypnout po dokončení jeho použití. Také můžete vytvořit a spuštění aplikace, které používají ať jazyky a knihovny se vám líbí. Tyto aplikace můžete použít některou z hello možnosti správy dat, které poskytuje Azure, a můžete také toouse SQL Server nebo jiný databázového systému spuštěné v jedné nebo více virtuálních počítačů.
2. **Přesunutí aplikace tooAzure (navýšení a shift)** -"Navýšení a shift" odkazuje toomoving aplikace mnohem, jako byste používali vysokozdvižném toomove velkého objektu.  Můžete "navýšení" hello virtuálního pevného disku z vašeho místního datového centra a "posunutí" jej tooAzure a spusťte ji existuje.  Obvykle máte toodo některé pracovní tooremove závislosti na jiných systémech. Pokud jsou moc, můžete zvolit možnost 3 místo.  
3. **Rozšíření vašeho datového centra** -použití Azure spuštěné virtuální počítače jako rozšíření vašeho datového centra místní SharePoint nebo jinými aplikacemi. toosupport, je možné toocreate doménách systému Windows v cloudu hello spuštěním služby Active Directory ve virtuálních počítačích Azure. Můžete použít Azure Virtual Network (uvedených později) tootie vaší místní sítí a sítí v Azure společně.

### <a name="web-apps"></a>Web Apps
![Službě Azure Web Apps ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *Obrázek: Azure Web Apps spustí webovou aplikaci v cloudu hello bez nutnosti toomanage hello základní webový server.*

Jeden z hello nejběžnější uživatelé provést kroky v cloudu hello běží webů a webových aplikací. To umožňuje virtuální počítače Azure, ale je stále vám zůstane hello zodpovědností správě jednoho nebo více virtuálních počítačů a hello základní operační systémy. Cloudové služby webové role můžete to provést, ale nasazení a údržbu, je stále trvá administrativy.  Co dělat, když chcete web kde někdo jiný postará hello správu vhodná?

Toto je přesně co nabízí webové aplikace. Tento model výpočetní nabízí prostředí spravované webové pomocí portálu pro správu Azure hello, jakož i rozhraní API. Existující aplikaci Web můžete přesunout do webové aplikace beze změny, nebo můžete vytvořit novou přímo v cloudu hello. Jakmile je spuštěn web, můžete přidat nebo odebrat instancí dynamicky spoléhat na vyrovnávat požadavky na Azure Web Apps tooload mezi nimi. Aplikace Azure nabízí sdílené možnost, kde webu běží ve virtuálním počítači s jinými weby, a standardní možnost, která umožňuje toorun lokality ve vlastním virtuálním počítači. možnost Standardní Hello také umožňuje zvětšete velikost hello (computing power) vaší instancí v případě potřeby.

Pro vývoj webové aplikace podporuje rozhraní .NET, PHP, Node.js, Java a Python spolu s SQL Database a MySQL (z ClearDB, partnera společnosti Microsoft) pro relační úložiště. Také poskytuje integrovanou podporu pro několik oblíbených aplikací, včetně WordPress, Joomla nebo Drupal. cílem Hello je tooprovide nízkými náklady, škálovatelná a široce užitečné platforma pro vytváření webů a webových aplikací ve veřejném cloudu hello.

**Scénáře webové aplikace**

Webové aplikace je určený toobe vhodný pro společnosti, vývojáři, webové návrhu úřady. Pro společnosti jedná o snadno spravovat, škálovatelná, vysoce zabezpečených a vysoce dostupné řešení pro spouštění přítomnosti weby. Pokud budete potřebovat tooset až web, je nejlepší toostart s Azure Web Apps a pokračovat tooCloud služby, jakmile budete potřebovat funkce, která není k dispozici. Viz hello konec hello části Další odkazy, které mohou pomoci toochoose mezi možnostmi hello "Výpočetní".

### <a name="cloud-services"></a>Cloud Services
![Cloudové služby Azure](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*Obrázek: Azure Cloud Services poskytuje místní toorun vysoce škálovatelné vlastní kód na platformě jako služba (PaaS) prostředí*

Předpokládejme, že chcete toobuild cloudových aplikací, který může podporovat velký počet současně připojených uživatelů, nevyžaduje mnoho správy a nikdy ocitne mimo provoz. Můžete mít zavedené softwaru dodavatele, například to je rozhodla tooembrace Software jako služba (SaaS) ve verzi jednu z vašich aplikací v cloudu hello. Nebo může být Startup vytváření aplikace příjemce, které očekáváte, že se rychle zvětšovat. Pokud vytváříte v Azure, které model spouštění mám použít?

Azure Web Apps umožňuje vytváření tento druh webové aplikace, ale existují některá omezení. Nemáte přístup pro správu, například, což znamená, že libovolný software nelze nainstalovat. Virtuální počítače Azure nabízí pružné, včetně přístup pro správu a určitě můžete ji použít toobuild velmi škálovatelné aplikace, ale budete mít toohandle mnoho aspektů spolehlivosti a správy sami. Jakým způsobem je možnost, která vám dává hello řízení potřebujete ale také obstará většinu práce hello potřebné pro spolehlivosti a správy.

Toto je přesně poskytuje Azure Cloud Services. Tato technologie je vytvořen výslovně toosupport, škálovatelnou a spolehlivé a aplikace Správce nízká a je příkladem co má označovaného jako platforma jako služba (PaaS). toouse, vytvoříte aplikaci pomocí technologie hello si zvolíte, například C#, Java, PHP, Python, Node.js nebo něco jiného. Váš kód pak provede ve virtuálních počítačích (označují tooas instance) s verzí systému Windows Server.

Ale tyto virtuální počítače se liší od hello ty, které vytvoříte pomocí Azure Virtual Machines. Pro jednou z věcí, Azure samotné spravuje je to třeba k instalaci opravy operačního systému a automaticky zavedením nové opravit bitové kopie. To znamená, že vaše aplikace by neměl uchování stavu v instancích role web nebo worker; Místo toho by měly být udržovány v jedné z možností správy dat Azure hello popsané v další části hello. Azure také monitoruje těchto virtuálních počítačů, všechny restartování tohoto selhání. Můžete nastavit cloudové služby tooautomatically vytvořit více nebo méně instancí v toodemand odpovědi. To vám umožní toohandle vyšší využití a škálování zpět, takže nejsou platícího co nejvíc po menší využití.

Máte dvě role toochoose z při vytváření instance, obě založené na Windows serveru. Hello hlavní rozdíl mezi hello dva je, že instance webové role spouští IIS, zatímco instanci role pracovního procesu nemá. Jak se spravují v hello je běžné, že aplikace toouse obou stejným způsobem, ale a. Například instanci webové role může přijímat požadavky od uživatelů a potom předat je tooa instance role pracovního procesu pro zpracování. tooscale aplikace nahoru nebo dolů, můžete požadovat, aby Azure vytvořit více instancí role buď nebo vypnout existující instance. A podobně jako tooAzure virtuálních počítačů, se vám účtována pouze dobu hello že každá instance role web nebo pracovní proces běží.

**Cloud Services scénáře**

Cloudové služby jsou ideální toosupport masivní možností horizontálního rozšíření kapacity, když budete potřebovat větší kontrolu nad hello platformy, než poskytuje Azure Web Apps, ale nemusí kontrolu nad hello příslušný operační systém.

#### <a name="choosing-a-compute-model"></a>Výběr modelu výpočetní
stránku Hello [porovnání Azure Web Apps, cloudové služby a virtuální počítače](app-service-web/choose-web-site-cloud-service-vm.md) poskytuje podrobnější informace o tom, toochoose výpočetní model.

## <a name="data-management"></a>Správa dat
Aplikace potřebují data, a různé druhy aplikací mít různé druhy dat.. Z toho důvodu Azure poskytuje několik různých způsobů toostore a spravovat data. Azure poskytuje mnoho možností úložiště, ale všechny jsou navrženy pro velmi odolné úložiště.  U některé z těchto možností jsou vždy 3 kopií vašich dat, které byly synchronizovány ve datovém centru Azure – Pokud povolíte Azure toouse geografická redundance tooback až tooanother datacenter alespoň 300 miles rychle 6.     

### <a name="in-virtual-machines"></a>Ve virtuálních počítačích
Hello možnost toorun systému SQL Server nebo jiný databázového systému ve virtuálním počítači vytvořit pomocí Azure Virtual Machines má zkopírovali. Uvědomte si, že tato možnost není omezený toorelational systémy; Můžete se také volné toorun NoSQL technologie jako MongoDB a Cassandra. Vlastní databáze je nainstalován systém přehledné it replikují nemohli jsme použít tooin vlastní datových centrech – ale také vyžaduje zpracování hello správu tohoto databázového systému.  V další možnosti Azure zpracovává více nebo všechny hello správy za vás.

Znovu jsou hello stav hello virtuálního počítače a všechny další datový disk vytvoříte nebo nahrát zajišťované úložiště objektů blob (které v souvislosti se později).  

### <a name="azure-sql-database"></a>Azure SQL Database
![Úložiště Azure SQL Database](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Obrázek: Poskytuje Azure SQL Database spravovaná služba relační databáze v cloudu hello.*

Pro relační úložiště Azure poskytuje funkce hello databáze SQL. Nenechte si hello pojmenování podvést vás. To se liší od typických SQL Database poskytované SQL Server běžící na Windows Server.  

Dříve se označovaly jako SQL Azure, Azure SQL Database poskytuje všechny hello klíčových funkcí systému správy relačních databází, včetně jednotlivé transakce, přístup k datům souběžných více uživatelů s integritu dat, dotazů ANSI SQL a známých programování model. Jako SQL Server, SQL Database lze přistupovat pomocí rozhraní Entity Framework, ADO.NET, JDBC a dalších známých dat přístupu technologie. Také podporuje většinu hello jazyka T-SQL, společně s nástroje SQL Server, jako je například SQL Server Management Studio. Kdokoliv obeznámeni s systému SQL Server (nebo jiné relační databáze), pomocí databáze SQL je přehledné.

Databáze SQL není právě databázového systému, ale v hello cloudu it je PaaS služby. Stále řízení dat a kdo k němu přístup, ale pracovní hello správu grunt, jako je například Správa hello hardware infrastruktury a automaticky udržuje hello databáze a operační systém softwaru si toodate postará databáze SQL. Databáze SQL také poskytuje vysokou dostupnost, automatické zálohování, v okamžiku obnovit možnosti a můžete replikovat kopie v zeměpisných oblastech.  

**Scénáře pro databázi SQL.**

Pokud vytváříte aplikaci Azure (pomocí kteréhokoli modelu výpočetní hello), který potřebuje relační úložiště, databáze SQL může být vhodný. Aplikace spuštěné mimo hello cloudu můžete také tuto službu, ale, je mnoho jiné scénáře. Například data uložená v databázi SQL je přístupná z různých klientských systémů, včetně stolní počítače, notebooky, tablety a telefony. A protože poskytuje integrovanou vysokou dostupnost prostřednictvím replikace, použití SQL Database může pomoci minimalizovat prostoje.

### <a name="tables"></a>Tabulky
![Tabulky úložiště Azure](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*Obrázek: Tabulky Azure poskytuje ploché data toostore způsob typu NoSQL.*

Tato funkce se někdy nazývá různé podmínky jako jeho součástí větší funkci "Azure Storage". Pokud se zobrazí "tabulky", "Azure tabulky" nebo "úložiště tabulky", je vše hello samé.  

A nemusíte nezaměňovat podle názvu hello: Tato technologie neposkytuje relační úložiště. Ve skutečnosti je příklad NoSQL přístupu volá úložiště dvojic klíč/hodnota. Tabulky Azure umožní aplikaci uložit vlastnosti různé typy, jako je například řetězce, celá čísla a kalendářní data. Aplikace pak může načíst skupinu vlastností tím, že poskytuje jedinečný klíč pro tuto skupinu. Při komplexních operací jako spojení nepodporuje, tabulky nabízí rychlý přístup tootyped data. Jsou také velmi škálovatelné, se jedné tabulky možné toohold mnohem terabajt data. A odpovídající jejich jednoduchost, tabulky jsou obvykle levnější toouse než relační úložiště databáze SQL.

**Scénáře pro tabulky**

Předpokládejme, že chcete toocreate aplikaci Azure, který potřebuje rychlý přístup k datům tootyped, může být velký počet ji, ale nepotřebuje tooperform složitých dotazů SQL na tato data. Představte si například, že vytváříte příjemce aplikaci, která potřebuje informace o profilu zákazníka toostore pro každého uživatele. Vaše aplikace cílit toobe velmi oblíbených, takže potřebujete tooallow pro velké množství dat, ale není příliš s tato data kromě ukládání, pak je načítání jednoduché způsoby. Toto je přesně hello druh scénář, kde tabulky Azure má smysl.

### <a name="blobs"></a>Objekty blob
![Objekty BLOB úložiště Azure](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Obrázek: Objektů BLOB Azure poskytuje nestrukturovaných binární data.*  

Azure BLOB (znovu "Úložiště objektů Blob" a právě "úložiště objektů BLOB" jsou hello samé) je navrženou toostore nestrukturovaných binární data. Stejně jako tabulky poskytuje nenákladné úložiště objektů BLOB a jediného objektu blob může být větší než 1TB (jeden terabajt). Aplikace Azure můžete používat taky jednotky Azure, které umožňují poskytovat trvalé úložiště pro systém souborů Windows připojené v instanci Azure BLOB. aplikace Hello uvidí obyčejnou soubory systému Windows, ale obsah hello jsou ve skutečnosti uložené v objektu blob.

Úložiště objektů blob je používá mnoho dalších Azure funkcí (včetně virtuálních počítačů), tak příliš ho může úlohy určitě zpracovat.

**Scénáře pro objekty BLOB**

Aplikace, která ukládá videa, obrovské soubory nebo jiné binární informace můžete použít pro jednoduché, levných úložiště objektů BLOB. Objekty BLOB také běžně se používají ve spojení s jinými službami jako Content Delivery Network, který se věnuje později.  

### <a name="import--export"></a>Import/export
![Služba Azure Import exportu](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Obrázek: Azure Import / Export nabízí možnost tooship hello tooor fyzický pevný disk z Azure pro hromadné zrychluje a zlevňuje data import nebo export.*  

Někdy budete chtít toomove velké množství dat do Azure. Který by trvat dlouhou dobu, možná dny a použít spoustu šířky pásma. V těchto případech můžete použít Azure Import/Export, což umožňuje vám tooship šifrované nástrojem Bitlocker 3,5" pevné disky SATA přímo tooAzure datových center, kde společnost Microsoft bude přenos hello dat do úložiště objektů blob pro vás.  Po dokončení nahrávání hello Microsoft dodává back tooyou hello jednotky.  Můžete také požádat, aby velkých objemů dat z úložiště objektů Blob vyexportovat do pevné disky a odeslat zpět tooyou prostřednictvím e-mailu.

**Scénáře pro Import / Export**

* **Velké migrace dat** – kdykoliv máte velké objemy dat (terabajtů), při němž tooupload tooAzure, hello službu Import/Export je často mnohem rychlejší a případně levnější než přenos přes hello Internetu. Jakmile hello data do objektů BLOB, můžete ji zpracovat do jiných formulářů, jako je například úložiště Table nebo databázi SQL.
* **Obnovení dat archivovat** -importu a exportu toohave Microsoft přenáší velké objemy dat uložených v Azure Blob Storage tooa paměťové zařízení odesílat a pak mít toto zařízení doručit back tooa umístění požadavky můžete použít. Protože to bude trvat delší dobu, není vhodný pro zotavení po havárii. Je nejvhodnější pro archivovaná data, která nepotřebujete rychlý přístup k.

### <a name="file-service"></a>Služba File
![Služba Azure souborů](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Obrázek: Azure Souborová služba poskytuje SMB \\ \\server\share cesty pro aplikace běžící v cloudu hello.*

Místní, je běžné toohave velké objemy úložiště souboru, které jsou dostupné prostřednictvím pomocí protokolu Server Message Block (SMB) hello \\ \\Server\share formátu. Azure má teď služba, která vám umožní toouse tento protokol v cloudu hello. Aplikace běžící v Azure ho můžete použít soubory tooshare mezi virtuálními počítači pomocí rozhraní API jako ReadFile a WriteFile známé soubor systému. Kromě toho hello soubory můžete také získat přístup na adrese hello současně přes rozhraní REST, což umožňuje tooaccess hello sdílené složky z místního při také nastavení virtuální sítě. Soubory Azure je postavená na služby objektů blob hello, takže zdědil hello stejné dostupnosti, odolnost, škálovatelnosti a geografická redundance integrovaný do úložiště Azure.

**Scénáře pro soubory Azure**

* **Migrace existující cloudové aplikace toohello** -jeho jednodušší toomigrate místní aplikace toohello cloudu, který pomocí souboru sdílí tooshare dat mezi částí aplikace hello. Každý virtuální počítač připojí toohello sdílené složky a potom může číst a zapisovat soubory stejně, jako je by proti soubor místní sdílené složky.
* **Nastavení sdílených aplikací** -běžný vzor pro distribuované aplikace je toohave konfigurační soubory v centrálním umístění, kde jsou dostupné z mnoha různých virtuálních počítačů. Tyto konfigurační soubory můžete uložený ve sdílené složce Azure File a číst všechny instance aplikace. nastavení Hello můžete navíc musí spravovat přes rozhraní REST hello, který umožňuje přístup po celém světě toohello konfigurační soubory.
* **Diagnostické sdílenou složku** – můžete uložit a sdílet diagnostické souborech, jako jsou protokoly, metriky a výpisů stavu systému. Tyto soubory, které jsou k dispozici prostřednictvím hello protokolu SMB a rozhraní REST umožní aplikace toouse celou řadu nástrojů pro analýzu pro zpracování a analýzu hello diagnostická data.
* **Dev/testovací/Debug** – když vývojáři nebo správci pracují na virtuální počítače v cloudu hello často potřebují sadu nástrojů nebo pomůcek. Instalace a distribuce tyto nástroje na každém virtuálním počítači je časově náročná. Soubory Azure umožňuje vývojáře nebo správce ukládat své oblíbené nástroje do sdílené složky a připojit toothem z jakéhokoli virtuálního počítače.

## <a name="networking"></a>Sítě
Azure spouští dnes v mnoha datových centrech rozloženy hello, world. Při spuštění aplikace nebo ukládání dat, můžete vybrat jednu nebo více z těchto datových centrech toouse. Můžete také připojit datových centrech toothese různými způsoby použití služby hello níže.

### <a name="virtual-network"></a>Virtual Network
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*Obrázek: Virtuální sítě poskytuje privátní sítě v cloudu hello tak různé služby může kontaktovat tooeach dalších nebo tooon místní prostředky Pokud nastavíte připojení k síti VPN mezi různými místy.*  

Jeden užitečný způsob, jak toouse veřejného cloudu je tootreat ji jako rozšíření vlastního datového centra.

Protože virtuální počítače můžete vytvořit na vyžádání, pak je odstranit (a zastavit platícího) Pokud jste již nepotřebujete, můžete mít computing power jenom v případě, že jej. A vzhledem k tomu, že virtuální počítače Azure umožňuje vytvářet virtuální počítače se systémem SharePoint, služby Active Directory a dalších známých místní software, tento postup můžete pracovat hello aplikací, které už máte.

toomake tento opravdu užitečné, ale uživatelé by mělo být možné tootreat toobe tyto aplikace jako by byla spuštěna ve svém vlastním datovém centru. Toto je přesně co Azure Virtual Network umožňují. Pomocí zařízení brány sítě VPN, Správce může nastavit virtuální privátní sítě (VPN) mezi místní sítí a vaše virtuální počítače, které jsou nasazené tooa virtuální sítě v Azure. Protože můžete přiřadit vlastní IP v4 adresy toohello cloudové virtuální počítače, zobrazí se toobe ve vlastní síti. Uživatelé ve vaší organizaci mohou využívat aplikace hello těchto virtuálních počítačů obsahovat, jako by byla spuštěna místně.

Další informace o plánování a vytvoření virtuální sítě, který vám vyhovuje, najdete v části [virtuální sítě](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>ExpressRoute
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Obrázek: ExpressRoute používá virtuální síť Azure, ale trasy připojení prostřednictvím rychlejší vyhrazené místo hello veřejného Internetu.*  

Pokud potřebujete další šířku pásma nebo zabezpečení než virtuální síť Azure připojení můžete zadat, můžou se podívat do ExpressRoute. V některých případech ExpressRoute můžete také ušetřit peníze. Je třeba do virtuální sítě v Azure, ale hello propojení mezi Azure a váš web používá vyhrazené připojení, které nejde přes hello veřejného Internetu. V pořadí toouse této služby, budete potřebovat toohave smlouva se poskytovatele síťové služby nebo poskytovatele exchange.

Nastavení připojení typu ExpressRoute vyžaduje více času a plánování, takže je vhodné toostart prostřednictvím sítě site-to-site VPN, pak migrovat tooan připojení ExpressRoute.

Další informace o ExpressRoute najdete v tématu [technický přehled ExpressRoute](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Obrázek: Azure Traffic Manager umožňuje vám tooroute globální provoz tooyour služby na základě inteligentního pravidel.*

Pokud vaše aplikace Azure běží v několik datových center, můžete použít Azure Traffic Manager tooroute žádosti od uživatelů inteligentně mezi instancemi aplikace hello. Je také možné směrovat provoz hello tooservices není spuštěna v Azure, dokud jsou přístupné z Internetu.  

Aplikaci Azure s uživateli v právě jedné součásti hello, world může spustit pouze jeden datového centra Azure. Aplikace s uživateli na různých kolem hello, world, ale, je vyšší pravděpodobnost toorun v několika datových centrech, může být i všechny z nich. V této druhé situaci čelit problému: jak inteligentně nasměrujete instancí tooapplication uživatelů? Většinu času hello, budete ho zřejmě chtít každý uživatel tooaccess hello datacenter nejbližší tooher, protože pravděpodobně získáte nejlepší doba odezvy jeho hello. Ale co když je dané instance aplikace hello přetížené nebo není k dispozici? V takovém případě by se dobrý toodirect svůj požadavek automaticky tooanother datacenter. Toto je přesně co se provádí pomocí Azure Traffic Manager.

Vlastník Hello aplikace definuje pravidla, které zadejte, jak žádosti od uživatelů by měla být směrovanou toodatacenters, a potom spoléhá na toocarry Traffic Manager se tato pravidla. Uživatelé například obvykle může být směrovanou toohello datové centrum Azure nejbližší, ale získat odeslat tooanother, jeden v případě, že doba odezvy hello z jejich výchozí datového centra překračuje dobu odezvy hello z jiných datových center. Pro globálně distribuované aplikace s mnoha uživateli s problémy toohandle integrovanou službu, jako to je užitečné.

Správce provozu používá Directory Name Service (DNS) tooroute uživatelé tooservice koncových bodů, ale dál provoz nepřekračuje prostřednictvím Správce provozu, po připojení. Díky tomu Traffic Manager nebudou problémové místo, které může zpomalit vaší komunikace služby.

## <a name="developer-services"></a>Vývojářské služby
Azure nabízí řadu nástrojů toohelp vývojáře a IT profesionály vytvořit a spravovat aplikace v cloudu hello.  

### <a name="azure-sdk"></a>Azure SDK
Zpět v 2008 hello úplně první předběžné verze Azure podporované jenom .NET – vývoj. V současné době však můžete vytvořit aplikace Azure v prakticky žádný jazyk. Společnost Microsoft poskytuje aktuálně jazykové sady SDK pro .NET, Java, PHP, Node.js, Ruby nebo Python. Je také obecné Azure SDK, která poskytuje základní podporu pro žádný jazyk, jako je například C++.  

Tyto sady SDK můžete vytvářet, nasazovat a spravovat aplikace na platformě Azure. Jsou k dispozici buď z [www.microsoftazure.com](https://azure.microsoft.com/downloads/) nebo Githubu a lze použít s Visual Studio a Eclipse. Azure nabízí i nástroje příkazového řádku, které mohou vývojáři použít s jakékoli editor nebo vývojovém prostředí, včetně nástrojů pro nasazení aplikace tooAzure ze systémů Linux a Macintosh.

Společně umožňují vytvářet Azure aplikace, tyto sady SDK také poskytují klientské knihovny, které vám pomůžou vytvořit softwaru, která používá služby Azure. Může například sestavit aplikaci, která čte a zapisuje objektů Azure BLOB, nebo vytvořte nástroj, který nasadí aplikací Azure přes rozhraní pro správu Azure hello.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
Visual Studio Team Services je marketing název, které se týkají číslo služeb, které pomůžou toodevelop aplikace v hello Azure.

tooavoid nedorozuměním - neposkytuje hostované nebo webové verzi sady Visual Studio. Stále potřebujete vaše místní spuštěná kopie sady Visual Studio. Ale nabízí celou řadu nástrojů, které může být velmi užitečné.

Nepodporuje, obsahuje systému správy hostované zdrojového názvem Team Foundation Service, který nabízí verzí a sledování pracovních položek.  Pokud dáváte přednost, který, můžete použít i Git pro správu verzí. A můžete měnit hello správy zdrojového kódu, které můžete použít projektu. Můžete vytvořit neomezená privátní týmové projekty přístupné z libovolné místo v hello, world.  

Visual Studio Team Services poskytuje službu, testování zatížení. Vytvořit v sadě Visual Studio na virtuálních počítačích v cloudu hello zátěžové testy můžete spustit. Zadejte celkový počet uživatelů, které chcete tooload testu s hello a Visual Studio Team Services bude automaticky určit, kolik agenti jsou potřeba, začne pracovat hello požadované virtuální počítače a spouštět zátěžové testy. Pokud jste odběratel MSDN, získáváte tisíce volné minut uživatele zatížení testování každý měsíc.

Visual Studio Team Services také poskytuje podporu pro agilní vývoj s funkcí jako sestavení průběžnou integraci, kanbanové karty a virtuální týmové místnosti.

**Visual Studio Team Services scénáře**

Visual Studio Team Services je vhodný pro společnosti, které potřebují toocollaborate po celém světě a již nemají hello infrastrukturu v místě toodo tak. Můžete získat instalační program v minutách, zvolte systému správy zdrojů a začít psaní kódu a sestavování daný den.  nástroje team Hello poskytují místo pro koordinaci a spolupráce a další nástroje hello zadejte, že hello analysis tootest a ladit aplikace rychle.

Ale organizace, které už máte v místním systému můžete otestovat nových projektů na Visual Studio Team Services toosee, pokud je efektivnější.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Obrázek: Application Insights monitorování výkonu a využití vaší živé aplikace web nebo zařízení.*

Po publikování aplikace – jestli běží na mobilních zařízeních, stolní počítače nebo webových prohlížečů - Application Insights se dozvíte, jak je její výkon a co uživatelé dělají s ním. Ponechá počet havárií a pomalé odezvy, výstrahy, vám, zda hello obrázky mezi nepřijatelné prahové hodnoty a pomohl vám je diagnostikuje problémy.

Při vývoji novou funkci, naplánujte toomeasure jeho úspěch s uživateli. Analýzou vzorce používání pochopit, co nejlépe vyhovuje vašim zákazníkům a vylepšení v každé cyklu vývoje aplikace.

I když je hostovaná v Azure, Application Insights funguje pro celou řadu rostoucí aplikací, jak zapnout a vypnout Azure. J2EE a ASP.NET webová aplikace vztahují, a také iOS, Android a Windows a OSX aplikace. Telemetrie je odeslán z sady SDK vytvořené pomocí aplikace hello toobe analyzovat a zobrazí v hello služby Application Insights v Azure.

Pokud chcete více specializované analýzy, exportujte hello telemetrie datového proudu tooa databáze, nebo tooPower BI nebo jiných nástrojů.

**Přehled scénářů aplikace**

Při vývoji aplikace. Může být webové aplikace nebo aplikace na zařízení nebo aplikace na zařízení s webové back-end.

* Vyladění výkonu hello aplikace po publikování, nebo když je v zátěžové testování.  Application Insights agreguje telemetrických dat ze všech instancí hello nainstalován a nabídne vám grafy odezvy, žádost a počty výjimka, závislosti odezvy a další ukazatele výkonu. Ti optimalizaci výkonu vaší aplikace. Můžete vložit kód tooreport konkrétnější údaje, pokud je to nutné.
* Najít a diagnostikovat problémy ve vaší živé aplikaci. Výstrahy můžete získat e-mailem, pokud ukazatele výkonu křížové přijatelné prahové hodnoty. Můžete prozkoumat konkrétní uživatelské relace, například požadavek hello toosee, který způsobil výjimku.
* Sledovat využití tooassess hello úspěch každé nové funkce. Když navrhujete nový uživatelský scénář, naplánujte toomeasure, kolik se používá, a zda uživatelé dosáhli svých cílů očekávané. Application Insights získáte základní informace o využití dat, jako je například zobrazení webové stránky a vložením kódu tootrack hello uživatelské prostředí podrobněji.

### <a name="automation"></a>Automation
Žádná líbí toowaste čas provádění hello stejné ruční zpracovává opakovaně. Azure Automation nabízí způsob, jak pro toocreate můžete monitorovat, spravovat a nasazujte prostředky v prostředí Azure.  

Automatizace používá "sady runbook", který používá pracovní postupy prostředí Windows PowerShell (oproti právě regulární PowerShell) v části zahrnuje hello. Sady Runbook jsou určené toobe provést bez zásahu uživatele. Pracovní postupy prostředí PowerShell umožňuje hello stav skriptu toobe, uložit kontrolní body podél hello způsobem. Pak pokud dojde k selhání, nemáte toostart skript od začátku hello. Můžete ji restartovat na hello posledního kontrolního bodu. Tím ušetříte velké množství pracovních každých možných selhání pokusu o toomake hello skriptu popisovač.

**Scénáře automatizace**

Služby Azure Automation je ruční hello tooautomate dobrou volbou, dlouhotrvajících, problematických a často se opakujících úloh v Azure.

### <a name="api-management"></a>API Management
Vytváření a publikování aplikace programátory rozhraní (API) na hello je Internetu běžné tooapplications služby tooprovide způsobem. Pokud tyto služby jsou resellable (například počasí data), organizace může povolit jiných třetích stran tooaccess tyto stejné služby za úplatu. Při změně měřítka toomore partnery, budete obvykle potřebovat toooptimize a řízení přístupu.  Někteří partneři může být nutné i hello data do jiného formátu.

Azure API Management usnadňuje organizace toopublish rozhraní API toopartners, zaměstnanci a jiných společností bezpečně a ve velkém měřítku. Poskytuje jiný koncový bod rozhraní API a jednání jako proxy toocall hello skutečný koncový bod při současném poskytování služby, jako je ukládání do mezipaměti, transformaci, omezení, řízení přístupu a analýzy agregace.

**Scénáře správy rozhraní API**

Řekněme, že má vaše společnost skupině zařízení, všechny musí toocall back tooa centrální služby tooget dat – například přesouvání společnost, která má zařízení v každé vůz na cestách hello.  Určitě se hello společnosti mají tooset až tootrack systému je vlastní dodávky tak, aby ho spolehlivě předpovědi a doručení časy aktualizace. Může vědět, kolik dodávky má a plánování správně.  Každý vůz bude nutné zařízení, která volá zpět tooa centrálního umístění společně s jeho umístění a rychlost dat a případně další.

Zákazník hello přesouvání společnosti by pravděpodobně také těžit z získávání tato umísťovací data.  Hello zákazník se může použít tooknow, jak daleko produkty mají tootravel, kde se zablokuje, kolik jejich platícího podél určité trasy (Pokud je v kombinaci s co jejich placené tooship). Pokud hello přesouvání společnosti agreguje tato data již, může pro něj platí mnoho zákazníků.  Ale pak hello přesouvání společnosti musí tooprovide způsob, jak zákazníci toogive hello data. Jakmile poskytují přístup toocustomers, se nemusí mít kontrolu nad jak často hello dat je dotazován. Budou mít tooprovide pravidla o tom, kdo má přístup k jaká data. Všechna tato pravidla by měla mít toobe integrovaný do jejich externí rozhraní API. Toto je, kde může pomoci API Management.  

## <a name="identity-and-access"></a>Identita a přístup
Práce s identitou je součástí většinu aplikací. Zjištěním je uživatel umožňuje aplikaci rozhodnout, jak by měla spolupracovat s tímto uživatelem. Azure poskytuje identitu sledovat toohelp služby a také integrovat do úložiště identit, které už používáte.

### <a name="active-directory"></a>Active Directory
Podobně jako většina adresářové služby Azure Active Directory ukládá informace o uživatelích a hello organizace, ke které patří. Umožňuje uživatelům přihlásit a pak poskytuje je s tokeny můžou poskytnout tooapplications tooprove svou identitu. Také umožňuje synchronizaci uživatelských údajů s Windows Server Active Directory spuštěna místně v místní síti. Při hello mechanismy a formáty dat používá Azure Active Directory nejsou identické s těmi, která používá v systému Windows Server Active Directory, jsou velmi podobné hello funkce, které provádí.

Jeho důležité toounderstand že Azure Active Directory je určen především pro použití cloudové aplikace. Může sloužit aplikace spuštěné v Azure, například nebo na jiných platformách cloudu. Použije se také ve společnosti Microsoft vlastní cloudové aplikace, jako jsou v Office 365. Pokud chcete, tooextend vašeho datacentra do cloudu hello pomocí virtuálních počítačích Azure a Azure Virtual Network, ale Azure Active Directory není hello správnou volbou. Místo toho budete muset toorun Windows Server Active Directory ve virtuálních počítačích.

toolet aplikace přístup k hello informace, které obsahuje, Azure Active Directory poskytuje Azure Active Directory Graph volat rozhraní RESTful API. Toto rozhraní API umožňuje aplikacím na všechny objekty adresáře přístup platformy a hello vztahů mezi nimi.  Oprávnění aplikace může například použít toto rozhraní API toolearn o uživatele, skupiny hello, které patří do a další informace. Aplikace můžete také zobrazit vztahy mezi jejich uživatelé sociálního grafu – povolení, je více inteligentně pracovat hello připojení mezi lidí.

Další funkce této služby Azure Active Directory, řízení přístupu, usnadňuje aplikace tooaccept identity informace ze sítě Facebook, Google, Windows Live ID a jiných poskytovatelů identit oblíbených. Místo nutnosti hello aplikace toounderstand hello různých data formátů a protokolech používaných každý z těchto poskytovatelů, řízení přístupu překládá všechny z nich do jednoho běžný formát. Umožňuje také aplikace přijmout přihlášení z jedné nebo více domén služby Active Directory. Dodavatele poskytuje aplikaci SaaS může například použít toogive uživatelů Azure Active Directory řízení přístupu v každé z jeho zákazníci toohello přihlášení aplikace.

Adresářové služby jsou základní podpora místního computing. By neměl být překvapivé jste, že mají také důležité v cloudu hello.

### <a name="multi-factor-authentication"></a>Multi-factor Authentication
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*Obrázek: Multi-Factor Authentication poskytuje hello funkce pro vaše aplikace tooverify více než jednu formu identifikace*

Zabezpečení je vždy důležité. Vícefaktorové ověřování (MFA) pomáhá zajistit, že přístup pouze uživatelé, sami své účty. Vícefaktorové ověřování (také označované jako dvojúrovňové ověřování nebo "2FA") vyžaduje, že uživatelé zadali dvěma z těchto tří způsobů ověření identity pro uživatelská přihlášení a transakce.

* Něco znáte (obvykle heslo)
* Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)
* Něco že se (biometrika)

Když se uživatel přihlásí, nepotřebují tooalso Ověřte svou identitu pomocí mobilní aplikace, telefonního hovoru nebo textové zprávy v kombinaci s své heslo. Ve výchozím nastavení Azure Active Directory podporuje hello použití hesel jako jediné metody ověřování pro přihlášení uživatele. Můžete použít společně s Azure AD nebo pomocí vlastních aplikací MFA a adresáře pomocí hello MFA SDK. Také můžete jej spolu s místními aplikacemi pomocí aplikace Multi-Factor Authentication Server.

**Scénáře vícefaktorového ověřování**

Ochrana přihlášení na citlivé účty, jako jsou banky přihlášení a kde neoprávněným položka může mít vysokou finanční nebo duševní vlastnost náklady přístup ke zdrojovému kódu.   

## <a name="mobile"></a>Mobilní
Pokud vytváříte aplikace pro mobilní zařízení, může pomoct Azure ukládání dat v cloudu hello, ověřování uživatelů a odesílat nabízená oznámení bez nutnosti toowrite značnou část vlastního kódu.

Pokud určitě můžete vytvořit hello back-end mobilní aplikace pomocí virtuálních počítačů, cloudových služeb nebo webových aplikací, strávíte mnohem méně času zápis hello základní součásti služby pomocí služby Azure.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*Obrázek: Mobilních aplikací poskytuje funkce, které jsou běžně požadovaných aplikací, které rozhraní s mobilními zařízeními.*

Azure Mobile Apps poskytuje mnoho užitečné funkce, které můžete ušetřit čas při vytváření back-end mobilní aplikace. Umožňuje vám toodo jednoduché zřizování a správy dat uložených v databázi SQL. V kódu na straně serveru můžete snadno použít možnosti úložiště další data jako úložiště objektů blob nebo MongoDB. Mobile Apps poskytuje podporu pro oznámení, když v některých případech můžete místo toho použít Notification Hubs podle níže uvedeného popisu.  Služba Hello má také rozhraní REST API, mobilní aplikace můžete volat tooget práci. Mobile Apps poskytuje taky možnost hello tooauthenticate uživateli prostřednictvím společnosti Microsoft a služby Active Directory a jiných poskytovatelů identit dobře známé jako je Facebook, Twitter a Google.   

Můžete použít jiné služby Azure, jako je Service Bus a rolí pracovního procesu a připojit místní tooon systémy. Dokonce můžete využívat 3. stran doplňky z hello další funkce tooprovide úložiště Azure (např. sendgrid vám umožňuje se e-mailu).

Nativní klientské knihovny pro Android, iOS, HTML/JavaScript, Windows Phone a Windows Store umožňují snadnější toodevelop pro aplikace na všechny hlavní mobilní platformy. Rozhraní REST API umožňuje toouse Mobile Services data a funkce ověřování s aplikací na různých platformách. Jedné mobilní služby můžete zálohovat více klientských aplikací, můžete poskytovat konzistentní uživatelské prostředí na zařízeních.

Protože Azure již podporuje masivní škálování, může zpracovávat hello provoz, jakmile aplikace k více oblíbených.  Sledování a protokolování jsou podporovány toohelp řešení problémů a správa výkonu.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*Obrázek: Notification Hubs poskytuje funkce, které jsou běžně požadovaných aplikací, které rozhraní s mobilními zařízeními.*

Při toodo oznámení můžete napsat kód v Azure Mobile Apps, Notification Hubs je optimalizované toobroadcast miliony vysoce individuální nabízených oznámení v rámci minut.  Nemáte tooworry o podrobnosti, třeba mobilní operátor nebo výrobce zařízení. Můžete vybrat jednotlivé nebo milionům uživatelů na základě jednoho volání rozhraní API.

Centra oznámení je navrženou toowork s jakýkoli back-end. Můžete použít vlastní back-end v cloudu hello systémem kteréhokoli poskytovatele služeb, nebo místní back-end mobilní aplikace Azure.

**Scénáře centra oznámení** Pokud jste zapisovali mobilní hru kde přehrávače trvalo zapne, může být nutné toonotify player 2 které player 1 dokončení jeho vypněte. Pokud to je všechno že potřebujete toodo, můžete použít právě mobilních aplikací. Pokud jste měli 100 000 uživatelů, ale vaše hře a chcete toosend čas, kdy citlivé nabídka bezplatné tooeveryone, Notification Hubs je lepší volbou hello.

Můžete odeslat novinek, sporting události a produktu oznámení oznámení toomillions uživatelů s nízkou latencí. Podniky uvědomí svým zaměstnancům o nový čas citlivé komunikace, např. prodejní tipy, tak, že zaměstnanci nemusí tooconstantly zkontrolujte e-mailu nebo jiné aplikace toostay informováni. Můžete také odeslat jednoho-jednorázových hesel potřebných pro službu Multi-Factor authentication.

## <a name="back-up"></a>Zálohování
Každý enterprise vyžaduje toobackup a obnovit data. Můžete použít Azure toobackup a obnovení aplikace v hello cloudu nebo místně. Azure nabízí různé možnosti toohelp v závislosti na typu hello zálohy.

### <a name="site-recovery"></a>Site Recovery
Azure Site Recovery (dříve správce obnovení technologie Hyper-V) můžete chránit důležité aplikace ve spolupráci hello replikaci a obnovení na webech. Site Recovery poskytuje schopnost tooprotect aplikace na základě technologie Hyper-v, VMWare nebo sítě SAN tooyour vlastní sekundární lokality, lokality tooa hostitele nebo tooAzure a vyhnout se hello nákladům a složitým vytváření a Správa vlastního sekundárního umístění. Azure šifruje data a komunikace a máte možnost hello příliš povolit šifrování dat na rest.

Nepřetržitě monitoruje hello stavu vašich služeb a pomáhá automatizovat hello řádné obnovení služeb v případě hello výpadku lokality na primární datové centrum hello. Virtuální počítače můžete zapínají v službě orchestrovat způsobem toohelp obnovení rychle i pro složité úlohy několika vrstvami.

Site Recovery spolupracuje s existujících technologií, jako jsou repliky technologie Hyper-V, System Center a SQL serveru Always On. Podívejte se na [přehled Azure Site Recovery](site-recovery/site-recovery-overview.md) další podrobnosti.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*Obrázek: Azure Backup zálohuje data z místních serverů, Windows do cloudu hello.*  

Azure Backup zálohuje data z místních serverů se systémem Windows Server do cloudu hello. Můžete spravovat své zálohy přímo z hello zálohovacích nástrojů v systému Windows Server 2012, Windows Server 2012 Essentials nebo System Center 2012 – Data Protection Manager. Alternativně můžete použít speciální agenta zálohování.

Data je bezpečnější, protože zálohování se šifrují před samotným přenosem a uložené v šifrované Azure a chráněn certifikát, který můžete nahrávat na server. Služba Hello používá hello stejnou ochranu dat redundantní a vysoce dostupné nalezen ve službě Azure Storage.  Můžete zálohovat soubory a složky v pravidelných intervalech nebo okamžitě, úplné nebo přírůstkové zálohování. Po zálohování dat toohello cloudu Autorizovaní uživatelé mohou snadno obnovit zálohy tooany serveru. Nabízí také zásady uchovávání informací konfigurovat data, komprese dat a přenosu dat omezení lze tedy spravovat hello náklady toostore a přenášet data.

**Scénáře pro zálohování Azure**

Pokud jste už používá Windows Server nebo System Center, zálohování Azure je přirozené řešení pro zálohování své servery systému souborů, virtuální počítače a databáze systému SQL Server.  Funguje s šifrovaný zhuštěný a komprimovaných souborů. Existují určitá omezení, takže byste měli [Zkontrolujte požadavky Azure Backup hello](http://technet.microsoft.com/library/dn296608.aspx) první.

## <a name="messaging-and-integration"></a>Zasílání zpráv a integrace
Kód bez ohledu na to, co je to, často potřebuje toointeract s jiný kód.  V některých situacích všechno, co je potřeba je základní zařazených do fronty zasílání zpráv. V ostatních případech se vyžadují složitější interakce. Azure poskytuje několik různých způsobů toosolve tyto problémy. Obrázek 5 ukazuje možnosti hello.

### <a name="queues"></a>Fronty
![Předávání přes Azure Service Bus](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*Obrázek: Fronty povolit přijít spojení mezi částí aplikace a usnadnit tak škálování.*  

Služba Řízení front je jednoduchý: jednu aplikaci umístí zprávu do fronty a zprávy je nakonec pro čtení, jiná aplikace. Pokud aplikace potřebuje pouze této přehledné služby, fronty Azure může být hello nejlepší volbou.

Z důvodu hello hello způsob, jak Azure vzrostla v čase fronty úložiště Azure a fronty služby Service Bus poskytuje podobné služby Řízení front služby. Hello důvodů, proč toouse jeden přes hello jiné jsou popsané v dokumentu poměrně technické hello [fronty Azure a fronty služby Service Bus - porovnání a na rozdíl od aktualizovaného](http://msdn.microsoft.com/library/azure/hh767287.aspx).  V mnoha scénářích buď pracovat.

**Scénáře fronty**

Jednou z běžných použití front je dnes toolet instanci role webové komunikaci s instancí role pracovního procesu v rámci hello stejnou aplikaci cloudové služby.

Předpokládejme například, že vytvoříte Azure aplikace pro sdílení videa. Hello aplikace se skládá z kódu PHP běžící ve webové roli, která umožňuje uživatelům nahrát a sledovat videa, společně s rolí pracovního procesu implementována v jazyce C#, který převádí nahrála video do různých formátech.

Když instanci webové role získá nové video od uživatele, můžete uložit do objektu BLOB hello video, následně odeslat zprávu rolí pracovního procesu tooa prostřednictvím fronty sdělením, že kde toofind tento nový videa. Pracovní role instance-it není vás uvítací zprávu z fronty hello které jeden se pak pro čtení a provede hello požadované video překlady hello pozadí.

Strukturování aplikace tímto způsobem umožňuje asynchronní zpracování, a také umožňuje hello jednodušší tooscale aplikace, vzhledem k tomu, že počet hello instance webových rolí a instancí rolí pracovního procesu můžete upravit nezávisle. Velikost fronty hello můžete použít také jako trigger tooscale hello počet rolí pracovního procesu nahoru či dolů. Příliš vysoké, a můžete přidat více rolí. Pokud získá nižší, můžete snížit počet hello rolí toosave peníze.  

Tento stejný vzor mezi mnoha různých částí aplikace můžete použít i v případě, že nemáte používají webové a pracovní role.  Umožňuje tooscale hello částí na obou stranách hello fronty nahoru či dolů jako vyžádání a vyžaduje doba zpracování.

### <a name="service-bus"></a>Service Bus
Jestli spustit i v cloudu hello ve vašem datovém centru na mobilní zařízení, nebo jinde, musí být aplikace toointeract. cílem Hello Azure Service Bus je, že toolet aplikací běžících prakticky odkudkoli vyměňovat data.

V přidání toohello fronty (1: 1) dříve popisované Service Bus poskytuje také tooother metody komunikace.

#### <a name="service-bus-relay"></a>Service Bus Relay
![Předávání přes Azure Service Bus](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Obrázek: Předávání přes Service Bus umožňuje komunikaci mezi aplikacemi na různých stranách bránou firewall.*

Service Bus umožňuje přímé komunikaci prostřednictvím své službě předávání poskytuje bezpečný toointeract přes brány firewall. Předávací služby Service Bus povolit aplikace toocommunicate výměna zpráv přes koncový bod hostované v cloudu hello, nikoli místně.

**Scénáře předávání přes Service Bus**

Aplikace, které komunikují přes službu Service Bus může být Azure aplikace nebo software spuštěný na některé jiné cloudové platformě. Může být také aplikace, které běží mimo hello cloudu, ale. Například představit letecká společnost, která implementuje rezervace služby v počítačích ve vlastním datovém centru. Hello letecká společnost potřebuje tooexpose tito klienti toomany služby, včetně vrácení se změnami veřejné terminály na letištích, terminály agenta rezervace a možná i zákazníkům telefony. Toodo Service Bus může použít toto vytváření volně párované interakce mezi hello různých aplikací.

#### <a name="service-bus-topics-and-subscriptions"></a>Témata a odběry týkající se Service Bus
![Témata služby Azure Service Bus](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *Obrázek: Témata služby Service Bus umožňuje více zpráv toopost aplikace a další zprávy tooreceive toosubscribe aplikace, které splňují určitá kritéria.*

Service Bus poskytuje mechanismus publikovat a odebírat volá témat a odběrů. S publish-subscribe aplikace může odesílat zprávy tooa tématu, zatímco jiné aplikace můžete vytvářet odběry toothis tématu. To umožňuje na více komunikace mezi sadu aplikací, když necháte hello stejnou zprávu si jej přečíst jakýkoli více příjemců.

**Témata služby Service Bus a odběrů scénáře**

Kdykoliv se nastavení kdy existuje velký počet zpráv, které jsou všechny důležité, ale různých podřízené systémů, stačí toolisten toodiffering podmnožiny tyto komunikace, tématu Service Bus a odběrů jsou dobrou volbou.

### <a name="biztalk-services"></a>BizTalk Services
![BizTalk Services](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *Obrázek: Služba BizTalk Services nabízí hello možnost tootransform XML zprávy formáty v cloudu hello.*

Někdy je nutné připojit systémy, které komunikují pomocí různých formátech zasílání zpráv. Je běžné obchodní toohave jiné databázi schémata a XML zasílání zpráv formáty, i když, což je běžný standard je k dispozici. Místo zapisovat velké množství vlastní kód, můžete použít místní toointegrate BizTalk Server různými systémy.  Služba Azure BizTalk Services nabízí hello stejný typ služby, ale v cloudu hello. Mohou platit pouze co používáte a není starat o škálování, jako byste měli tooon místní.

**BizTalk Services scénáře**

Interakce Business-to-Business (B2B) běžně vyžadují tento typ posunutí.  Například společnost vytváření letadla potřebám tooorder části z něj je různé části poskytovatele. Bude mít mnoho dodavatelů částí.  Tyto objednávky by měl být automatizované toogo přímo ze systémů počítačů letadle hello do systémů dodavatelů hello.  Ani obchodní chce toochange jejich základní systémy a formáty zpráv a je velmi nepravděpodobné, že tyto formáty jsou hello stejné. BizTalk Services můžete provést zprávy a převod mezi hello nové formáty obou směrech. To můžete provést buď dodavatele letadle hello hello pracovní tootranslate nebo hello různých dodavatelů může v závislosti na tom, kdo chce další ovládací prvek a hello množství překlad potřeby.     

## <a name="compute-assistance"></a>Výpočetní pomoc
Azure poskytuje pomoc pro služby, které nepotřebují toorun celou dobu hello.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*Obrázek: Azure Scheduler poskytuje způsob tooschedule úloh v určitém čase pro určitou dobu.*

V některých případech aplikace stačí toorun v určitém čase. V Azure můžete ušetřit peníze s tímto typem aplikace místo aplikace právě běžet 24 x 7 čekání data tooprocess. Azure Scheduler vám umožní tooschedule při aplikace by neměl být spuštěný na základě intervalu, času nebo kalendáře. Je spolehlivé a ověří, že proces běží i v případě, že tam jsou chyby center sítě, počítače a data. Pomocí REST API Scheduleru toomanage hello těchto akcí.

Při výskytu naplánované varování Plánovač odešle protokolu HTTP nebo HTTPS zprávy tooa konkrétní koncového bodu nebo můžete vložit zprávu do fronty úložiště.  Proto musíte toohave aplikace buď použijte přístupném koncovém bodu nebo ji sledovat frontu úložiště. Potom po získá uvítací zprávu, může provádět jakékoli akce je naprogramovaný tak, aby.

**Scénáře plánovače**

* Opakující se akce aplikace: například služby může pravidelně získání dat z twitteru a shromáždit hello data do regulární informačního kanálu.
* Každodenní údržba: protokolu zpracování nebo vyřazování, provádění záloh a dalších občas plánování úkolů.
* Úlohy, které spustí v noci.
* Webové aplikace úlohy, například každodenní vyřazování protokolů, zálohování a další úlohy údržby. Správce může zvolit toobackup jeho databáze v 1: 00 každý den pro hello další 9 měsíců, například.

Hello API Scheduleru vám umožní toocreate, aktualizaci, odstranění, zobrazení a správa kolekcí úloh a naplánované úlohy prostřednictvím kódu programu.

## <a name="performance"></a>Výkon
Výkon, je důležité pro aplikaci. Aplikace mívají tooaccess hello stejná data opakovaně. Jedním ze způsobů tooimprove výkonu je tookeep kopii dat blíže toohello aplikace, minimalizovat hello době potřebné tooretrieve ho. Azure poskytuje různé služby tohoto postupu.

### <a name="azure-caching"></a>Ukládání do mezipaměti Azure
![Ukládání do mezipaměti Azure](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Obrázek: Aplikací Azure můžete data v paměti do mezipaměti a i rozdělit do mnoha rolí pracovního procesu**

Přístup k datům uloženým v některém z Azure, je Správa dat služby SQL Database, tabulky nebo objekty BLOB-je velmi rychlé. Ještě přístup k datům, které jsou uložené v paměti je i rychlejší. Z toho důvodu udržování kopie v paměti často používaná data může zlepšit výkon aplikace. Tímto způsobem můžete toodo ukládání do mezipaměti v paměti Azure.

Cloudové služby aplikace můžete ukládat data do mezipaměti, a pak načíst přímo bez nutnosti tooaccess trvalého úložiště. Hello mezipaměti je možné udržovat, že výhradně toocaching vyhrazené uvnitř vaší aplikace je virtuálních počítačů nebo poskytovaný virtuálních počítačů. V obou případech mohou být distribuovány hello mezipaměti, s daty hello obsahuje šíření mezi více virtuálních počítačů v datovém centru Azure.

Azure má několik různých mezipaměti technologie, které mají zapuštěno v čase. V pořadí hello jejich zavedených, je sdílený, v roli, spravovat a Redis cache. ukládání do mezipaměti Hello sdílené je technologie starší a byste je neměli vytvářet nové implementace s ním. Hello spravované mezipaměti má hello stejné funkce hello v Role do mezipaměti, ale služba se spravuje mimo hello portálu pro správu Azure. Hello Redis Cache je ve verzi preview. implementace Redis Hello má hello největší počet funkcí a doporučuje se při psaní nový kód pro ukládání do mezipaměti.

**Scénáře Azure Cache**

Aplikace, která opakovaně čte katalog produktů může využívat tento druh ukládání do mezipaměti, třeba od hello data musí bude k dispozici rychleji. technologie Hello také podporuje uzamčení, nechá ji použije se pro čtení a zápis, jakož i data jen pro čtení. A aplikace ASP.NET hello služby toostore relace data můžete použít s právě změně konfigurace.

### <a name="content-delivery-network"></a>Content Delivery Network
![Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Obrázek: Kopie objektu blob můžete uložit do mezipaměti v lokalitách kolem hello, world.**

Předpokládejme, že potřebujete toostore data objektů blob, který bude mít přístup uživatelé kolem hello, world. Možná je video hello nejnovější poháru shodu, například aktualizace ovladačů nebo oblíbených elektronických knih. Ukládání kopii dat hello v několika datových centrech Azure pomůže, ale pokud existují velký počet uživatelů, není pravděpodobně dostatek. Ještě lepší výkon můžete použít hello Azure CDN.

Hello CDN má desítek lokalit kolem Dobrý den, každý může ukládání kopií objektů Azure BLOB. Hello prvním přístupu konkrétním objektu blob, uživatele v některé části hello, world hello informace, které obsahuje zkopírován z datového centra Azure do místního úložiště CDN v tomto geography. Poté použije přistupuje z část hello, world hello kopírovat objekt blob uložené v mezipaměti v hello CDN-nebude potřebují toogo všechny toohello způsob hello nejbližší datového centra Azure. Hello výsledkem je rychlejší přístup k datům toofrequently přístup uživatelé kdekoli v hello, world.

**Scénáře CDN**

Je běžné toouse CDN s video toodeliver Media Services po celém světě. Video je obvykle velké a vyžaduje velké šířky pásma.  Služba Media Services je věnovala jinde na této stránce.

## <a name="big-data-and-big-compute"></a>Velké objemy dat a Big Compute
### <a name="hdinsight-hadoop"></a>HDInsight (Hadoop)
![HDInsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Obrázek: HDInsight pomáhá s hello hromadné zpracování obrovské objemy dat.**

Po celá léta hello hromadné analýzy dat bylo na relační data uložená v datovém skladu vytvořené s relační databázového systému. Tento druh obchodní analýza je stále důležité a bude pro toocome dlouhou dobu. Ale co když je tak velká, že relačních databází právě nemůže zpracovat ho hello data, která chcete tooanalyze? A Předpokládejme, že není relační hello data? Může to být, že v protokolech serveru v datacentru, například nebo data historické události ze senzorů nebo něco jiného. V takových případech musíte, která se označuje jako problém velkých objemů dat. Budete potřebovat další přístup.

dominantní technologie Hello dnes pro analýzu velkých objemů dat je Hadoop. Apache otevřete projekt zdroje, tato technologie ukládá data pomocí hello Hadoop Distributed File System (HDFS) a pak umožňuje vývojářům vytvářet tooanalyze úlohy MapReduce tato data. HDFS šíří dat napříč několika servery, pak bloky spustí úlohu MapReduce hello na každé z nich, když necháte hello velkých objemů dat se zpracovávají paralelně.

HDInsight je název hello služeb Apache Hadoop založené hello Azure. HDInsight umožňuje HDFS ukládat data do clusteru hello a distribuovat ji do víc virtuálních počítačů. Šíří hello logiku úlohu MapReduce se také v těchto virtuálních počítačů. Stejně jako s Hadoop v místě, data zpracovaná místně hello logiku a data hello funguje na jsou v hello stejného virtuálního počítače – a paralelní pro dosažení vyššího výkonu. HDInsight také můžete uložit data v Azure Storage trezoru (ASV), který používá objekty BLOB.  Použití ASV umožňuje toosave peníze protože můžete odstranit svému clusteru HDInsight, když není používán, ale stále uchovávat data v cloudu hello.

HDinsight podporuje další součásti hello ekosystém Hadoop také, včetně Hive a Pig. Společnost Microsoft vytvořila také komponent, které jednodušší toowork s dat vytvářených HDInsight pomocí tradičních nástrojů BI, například hello HiveODBC adaptéru a Průzkumníku dat, které pracují v aplikaci Excel.

### <a name="high-performance-computing-big-compute"></a>High-Performance Computing (Big Compute)
Jedním z hello nejvíce atraktivní způsoby toouse cloudové platformy je toorun vysokovýkonné výpočetní (prostředí HPC) a dalších aplikacích "Big Compute". Mezi příklady patří specializované engineering toouse aplikace sestavené hello standardní rozhraní MPI (Message Passing) a také takzvané paralelně zpracovatelné aplikace, tyto modely finančních rizik.

Hello podstatu Big Compute je spouštění kódu v velký počet počítačů v hello stejnou dobu. V Azure to znamená systému velký počet virtuálních počítačů současně, všechny práce v paralelní toosolve nějaký problém. Vyžaduje některé způsob tooresources a tooschedule aplikace, například toodistribute práci mezi tyto instance. Volné HPC Pack společnosti Microsoft a jiných řešení výpočetního clusteru můžete provádět i v Azure, při využití výpočetních a infrastruktury služby Azure tooadd kapacity na vyžádání tooan místní výpočetní cluster nebo spouštět aplikace Big Compute zcela v hello cloud.

Azure poskytuje rozsah VM instance velikosti s různými konfiguracemi jader procesoru, paměti, kapacity disku a dalších vlastností toomeet hello požadavků různých aplikací. Hello nedávno zavedená A8 a A9 instancí pracovních i pro mnoho výpočetních zatížení s intenzivním a paralelních aplikací MPI, protože mají vysokou rychlostí, vícejádrovými procesory a velké objemy paměti. Při některých konfiguracích hello instancí využít k síti s nízkou latencí a vysokou propustností aplikace hello cloudu, který obsahuje do paměti vzdáleného přímý přístup do (počítače RDMA) technologie pro maximální efektivity paralelních aplikací MPI.

Azure také nabízí Big Compute aplikace vývojářům a partnerům úplnou sadu kapacita výpočetních operací, služeb, možností architektur a vývojářských nástrojů. Azure podporuje vlastní pracovní postupy Big Compute zahrnující pracovní postupy specializované dat a úlohy a úlohy plánování vzorů, které je možné škálovat toothousands z výpočetní jader.

## <a name="media"></a>Média
![Azure Media Services](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Obrázek: Služba Media Services je platforma pro aplikace, které poskytují videa a jiné média tooclients kolem hello, world.**

Video tvoří velkou část internetové přenosy dnes, a že procento bude i větší zítra. Ještě zajišťuje videa na webu hello není jednoduché. Mnoho proměnných, jako je například hello kódování algoritmus a hello zobrazit rozlišení obrazovky hello uživatele. Video také obvykle toohave shluky požadavků, jako je sobota noci Špička při mnoha lidmi se rozhodnete, že mu toowatch online film.

S ohledem jeho oblíbenosti je sejfu tipu, že mnoho nové aplikace bude vytvořen použití videa. Ještě toosolve všechny z nich bude nutné některé z hello stejné problémy a provádění každé z nich řešení těchto problémů sama o sobě nemá smysl. Lepším řešením je toocreate platformu, která poskytuje společné řešení pro mnoho toouse aplikace. A sestavování této platformě v cloudu hello má několik výhod zrušte. Může být široce dostupné na základě průběžnými platbami, a můžete také zpracovat hello variabilita požadavků, které často čelí video aplikace.

Tento problém řeší Azure Media Services. Poskytuje sadu cloudu součásti, které usnadňuje životnosti uživatelům vytváření a spouštění aplikací s použitím média videa a dalších.

Jak ukazuje obrázek hello, služba Media Services poskytuje sada komponent pro aplikace, které pracují s videa a jiné médium. Například obsahuje na média ingestování součást tooupload video do Media Services (kde je uložený v Azure BLOB), kódování součást, která podporuje různé formátů videa a zvuku, ochrana obsahu komponenty, která poskytuje digitální rights management součást pro vkládání reklamy do datový proud videa, součásti pro streamování a další. Partneři společnosti Microsoft můžete také zadat součásti pro platformu hello a pak v aplikaci Microsoft distribuovat tyto součásti a účtovat jejich jménem pošle.

Aplikace, které používají tuto platformu lze spustit v Azure nebo jinde. Například aplikace pro úklidové video produkční může uživatelům jeho nahrání video tooMedia služby, potom zpracovat různými způsoby. Alternativně může správy obsahu cloudové služby spuštěné v Azure spoléhají na tooprocess Media Services a distribuovat video. Všude, kde běží a jakým funguje, každá aplikace vybere součástí, které je nutné toouse, k nim přistupovat pomocí rozhraní RESTful.

toodistribute co vytváří, aplikace můžete použít hello Azure CDN, jiné CDN, nebo jenom odeslání bits přímo toousers. Ale získá existuje, video, které jsou vytvořené pomocí služby Media Services mohou být spotřebovávána různé klientské systémy, včetně systému Windows, Macintosh, HTML 5, iOS, Android, Windows Phone, Flash a Silverlight. cílem Hello je toomake je snazší toocreate média moderní aplikace.

**Odkazy**

Víc vizuální zobrazení jak funguje služba Media Services, stáhněte si hello [Azure Media Services plakát][Azure Media Services Poster].

## <a name="commerce"></a>Obchodování
zvýšení Hello softwaru jako služby je transformace, jak jsme vytvořit aplikace. Je také transformace, jak jsme prodeje aplikace. Vzhledem k tomu, že aplikace SaaS žije v cloudu hello, má smysl, který by měl vypadat jeho potenciální zákazníky pro řešení online. A tato změna platí toodata, jakož i tooapplications. Proč by neměl osoby hledat toohello cloudu komerčně dostupných datových sad? Microsoft řeší oba tyto problémy s hello [Azure Marketplace](https://azure.microsoft.com/marketplace/).

![Azure Commerce](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Obrázek: Azure Marketplace a úložiště Azure vám umožní najít a zakoupit aplikace Azure a komerční datové sady a používat jako součást aplikace Azure.**

Hello rozdíl mezi dvěma hello je, že Marketplace je mimo hello portálu pro správu Azure, ale hello úložiště je přístupné z uvnitř hello portálu. Potenciální zákazníky může hledat toofind Azure aplikace, které podle jejich potřeb. Zákazníci můžete vyhledat taky komerční datové sady včetně demografické údaje, finanční údaje, zeměpisné údaje a další. Když najít něco, co se jako, můžete přístup buď od dodavatele hello přímo prostřednictvím hello Marketplace nebo úložiště webových umístění nebo v některých případech z hello portálu pro správu. Aplikace můžete také použít rozhraní API služby Bing Search hello prostřednictvím hello Marketplace, poskytnete jim přístup toohello výsledky vyhledávání na webu.

**Scénáře Commerce**

Sendgrid vám umožňuje je aplikace v hello úložiště Azure, který vám umožní toosend e-mailu. Nabízí další funkce, jako je spolehlivé doručení a statistiky.  Můžete zakoupit této aplikace a související služby místo zkuste toobuild takové infrastruktury sami.  

## <a name="getting-started"></a>Začínáme
Teď, když máte hello velký obrázek, hello dalším krokem je toowrite vaší první aplikace Azure. Vyberte si jazyk [získat hello odpovídající SDK](/downloads/)a pak použijte pro něj. Cloud computing je nový výchozí hello – Začínáme hned.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
