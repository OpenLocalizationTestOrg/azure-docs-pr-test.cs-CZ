---
title: "aaaAzure úložiště výkon a škálovatelnost kontrolní seznam | Microsoft Docs"
description: "Kontrolní seznam osvědčené postupy pro použití s Azure Storage při vývoji aplikací původce."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 959d831b-a4fd-4634-a646-0d2c0c462ef8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c2970c055d460070288d1810e4a77a7f056a4137
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Kontrolní seznam pro výkon a škálovatelnost Microsoft Azure Storage
## <a name="overview"></a>Přehled
Od vydání hello hello služeb Microsoft Azure Storage společnost Microsoft vyvinula počet osvědčené postupy pro použití těchto služeb způsobem původce a tento článek slouží tooconsolidate hello nejdůležitější z nich do seznamu kontrolní seznam ve stylu. záměr Hello tohoto článku je, že vývojáři aplikace toohelp ověřte, zda že používají osvědčené postupy s Azure Storage a toohelp je identifikovat ostatní osvědčené postupy zvažte přijetí. Tento článek se nepokusí toocover všechny možné optimalizace výkonu a škálovatelnosti – se vyloučí ty, které jsou v jejich dopad malé nebo široce neplatí. toohello rozsah, který hello chování aplikace můžete předpovědět při návrhu, je užitečné tookeep v paměti již v rané fázi na tooavoid návrhů, které se spustí do problémy s výkonem.  

Každý vývojář aplikace pomocí Azure Storage by měl trvat hello čas tooread v tomto článku a zkontrolujte, že své aplikace odpovídá každý hello osvědčené postupy, které jsou uvedeny níže.  

## <a name="checklist"></a>Kontrolní seznam
Tento článek slouží k uspořádání hello osvědčené postupy do hello následující skupiny. Osvědčené postupy platí pro:  

* Všechny služby Azure Storage (objekty BLOB, tabulky, fronty a soubory)
* Objekty blob
* Tabulky
* Fronty  

| Hotovo | Oblast | Kategorie | Otázky |
| --- | --- | --- | --- |
| &nbsp; | Všechny služby |Cíle škálovatelnosti |[Dosahuje vaší aplikace určená tooavoid cíle škálovatelnosti hello?](#subheading1) |
| &nbsp; | Všechny služby |Cíle škálovatelnosti |[Je vaše zásady vytváření názvů navržená tak, tooenable lepší vyrovnávání zatížení?](#subheading47) |
| &nbsp; | Všechny služby |Sítě |[Mají klientské straně zařízení dostatečně velkou šířku pásma a s nízkou latencí tooachieve hello výkonu potřeba?](#subheading2) |
| &nbsp; | Všechny služby |Sítě |[Mají klientských zařízení straně odkaz dostatečně vysoký kvality?](#subheading3) |
| &nbsp; | Všechny služby |Sítě |[Nachází klientská aplikace hello "blíží" hello účet úložiště?](#subheading4) |
| &nbsp; | Všechny služby |Distribuce obsahu |[Používáte název CDN pro distribuci obsahu?](#subheading5) |
| &nbsp; | Všechny služby |Přímý přístup do klienta |[Používáte místo proxy SAS a CORS toostorage tooallow přímý přístup?](#subheading6) |
| &nbsp; | Všechny služby |Ukládání do mezipaměti |[Ukládání do mezipaměti data aplikací, který se používá opakovaně a změny je málokdy?](#subheading7) |
| &nbsp; | Všechny služby |Ukládání do mezipaměti |[Je vaše aplikace dávkování aktualizace (je do mezipaměti na straně klienta a pak odesílání větší sad)?](#subheading8) |
| &nbsp; | Všechny služby |Konfigurace rozhraní .NET |[Nakonfigurovali jste toouse vašeho klienta dostatečný počet souběžných připojení?](#subheading9) |
| &nbsp; | Všechny služby |Konfigurace rozhraní .NET |[Nakonfigurovali jste .NET toouse dostatečný počet vláken?](#subheading10) |
| &nbsp; | Všechny služby |Konfigurace rozhraní .NET |[Používáte rozhraní .NET 4.5 nebo novější, který je vylepšený uvolňování paměti?](#subheading11) |
| &nbsp; | Všechny služby |Paralelismus |[Můžete zajistily paralelismus je tak, že nemáte přetížení funkce klienta nebo cíle škálovatelnosti hello ohraničenou správně?](#subheading12) |
| &nbsp; | Všechny služby |Nástroje |[Používáte nejnovější verzi Microsoft hello klientské knihovny a nástroje jsou poskytovat?](#subheading13) |
| &nbsp; | Všechny služby |Opakování |[Se, že používáte exponenciálního omezení rychlosti zásady omezení chyb a vypršení časových limitů opakování?](#subheading14) |
| &nbsp; | Všechny služby |Opakování |[Je vaše aplikace vyhněte opakování neopakovatelné chyby?](#subheading15) |
| &nbsp; | Objekty blob |Cíle škálovatelnosti |[Máte velký počet klientům přístup k jednoho objektu současně?](#subheading46) |
| &nbsp; | Objekty blob |Cíle škálovatelnosti |[Vaše aplikace nepřekročili hello šířky pásma nebo operations cíle škálovatelnosti pro jediného objektu blob?](#subheading16) |
| &nbsp; | Objekty blob |Kopírování objektů BLOB |[Můžete kopírování objekty BLOB jsou účinným způsobem?](#subheading17) |
| &nbsp; | Objekty blob |Kopírování objektů BLOB |[Používáte pro hromadné kopie objektů BLOB AzCopy?](#subheading18) |
| &nbsp; | Objekty blob |Kopírování objektů BLOB |[Používáte Azure Import/Export tootransfer velmi velkých objemů dat?](#subheading19) |
| &nbsp; | Objekty blob |Používání metadat |[Jsou ukládání často používaných metadata o objekty BLOB v jejich metadatech?](#subheading20) |
| &nbsp; | Objekty blob |Rychlé nahrávání |[Při pokusu o jeden objekt blob tooupload rychle, jsou nahrávání bloky paralelně?](#subheading21) |
| &nbsp; | Objekty blob |Rychlé nahrávání |[Při pokusu o tooupload mnoho objektů BLOB rychle, jsou nahrávání objekty BLOB paralelně?](#subheading22) |
| &nbsp; | Objekty blob |Opravte typu Blob |[Používáte objekty BLOB stránky nebo objekty BLOB bloku v případě nutnosti?](#subheading23) |
| &nbsp; | Tabulky |Cíle škálovatelnosti |[Jsou vám blížící hello cíle škálovatelnosti pro entity za sekundu?](#subheading24) |
| &nbsp; | Tabulky |Konfigurace |[Používáte JSON pro své žádosti tabulky?](#subheading25) |
| &nbsp; | Tabulky |Konfigurace |[Jste vypnuli Nagle tooimprove hello výkon malé požadavků?](#subheading26) |
| &nbsp; | Tabulky |Tabulky a oddíly |[Mít jste správně rozdělena na oddíly svá data?](#subheading27) |
| &nbsp; | Tabulky |Aktivní oddíly |[Zamezení jsou pouze připojení a jen předřazení vzory?](#subheading28) |
| &nbsp; | Tabulky |Aktivní oddíly |[Jsou mnoha oddílů rozloženy vložení nebo aktualizace?](#subheading29) |
| &nbsp; | Tabulky |Obor dotazu |[Byly navrženy tooallow vašeho schématu pro bod dotazy toobe použít ve většině případů a tabulka dotazy toobe šetřit?](#subheading30) |
| &nbsp; | Tabulky |Hustotu dotazu |[Proveďte kontrolu obvykle pouze vaše dotazy a vrátí řádky, které vaše aplikace bude používat?](#subheading31) |
| &nbsp; | Tabulky |Omezení vrácená Data |[Používáte filtrování vracejících entity tooavoid, které nejsou potřebné?](#subheading32) |
| &nbsp; | Tabulky |Omezení vrácená Data |[Používáte projekce tooavoid vracení vlastností, které nejsou potřebné?](#subheading33) |
| &nbsp; | Tabulky |Denormalization |[Máte tak, aby se vyhnout neefektivní dotazy nebo víc požadavků na čtení, při pokusu o tooget data nenormalizovanou svá data?](#subheading34) |
| &nbsp; | Tabulky |Vložení, aktualizaci nebo odstranění |[Jsou dávkování požadavků, které potřebují toobe transakcí nebo lze provést v hello stejný čas odezvy tooreduce?](#subheading35) |
| &nbsp; | Tabulky |Vložení, aktualizaci nebo odstranění |[Se můžete vyhnout načítání toodetermine jenom entity zda toocall vložit nebo aktualizovat?](#subheading36) |
| &nbsp; | Tabulky |Vložení, aktualizaci nebo odstranění |[Zvážení ukládání řadu data, která bude často načten společně v jedné entity jako vlastnosti místo více entit](#subheading37) |
| &nbsp; | Tabulky |Vložení, aktualizaci nebo odstranění |[Pro entity, které budou načteny vždy společně a může být napsán v dávkách (například časové řady data) ke zvážení použití objektů BLOB místo tabulky](#subheading38) |
| &nbsp; | Fronty |Cíle škálovatelnosti |[Jsou vám blížící hello cíle škálovatelnosti zpráv za sekundu?](#subheading39) |
| &nbsp; | Fronty |Konfigurace |[Jste vypnuli Nagle tooimprove hello výkon malé požadavků?](#subheading40) |
| &nbsp; | Fronty |Velikost zprávy |[Jsou vaše zprávy compact tooimprove hello výkonu hello fronty?](#subheading41) |
| &nbsp; | Fronty |Hromadné načtení |[Jsou načítání více zpráv v rámci jedné operace "Get"?](#subheading42) |
| &nbsp; | Fronty |Frekvence cyklického dotazování |[Dotazování jsou často dostatek hello tooreduce zaznamenatelného latence aplikace?](#subheading43) |
| &nbsp; | Fronty |Zpráva aktualizace |[Používáte UpdateMessage toostore průběh zpracování zpráv zabraňující s tooreprocess hello celou zprávu, pokud dojde k chybě?](#subheading44) |
| &nbsp; | Fronty |Architektura |[Používáte fronty toomake celé aplikace větší škálovatelnost tak dlouho běžící úlohy mimo hello kritické cesty a škálování pak nezávisle?](#subheading45) |

## <a name="allservices"></a>Všechny služby
Tato část uvádí osvědčené postupy, které jsou použitelné toohello použití všech služeb Azure Storage hello (objekty BLOB, tabulky, fronty nebo soubory).  

### <a name="subheading1"></a>Cíle škálovatelnosti
Každý služby Azure Storage hello má cíle škálovatelnosti kapacity (GB), rychlost transakcí a šířky pásma. Pokud vaše aplikace blíží nebo překračuje žádné cíle škálovatelnosti hello, setkat vyšší transakce latenci nebo omezení. Když služba úložiště omezí generovaný vaší aplikace, začne služba hello tooreturn "503 Server je zaneprázdněný" nebo "500 časový limit operace" kódy chyb pro některé transakce úložiště. Tato část popisuje obě toodealing hello obecný přístup s cíle škálovatelnost a cíle škálovatelnosti šířky pásma na konkrétní. Další oddíly, které pracují s služby jednotlivých úložiště popisují cíle škálovatelnosti v kontextu hello této konkrétní služby:  

* [Objekt BLOB šířky pásma a počet požadavků za sekundu](#subheading16)
* [Tabulka entity za sekundu](#subheading24)
* [Fronty zpráv za sekundu](#subheading39)  

#### <a name="sub1bandwidth"></a>Cíl škálovatelnost šířky pásma pro všechny služby
V době psaní textu hello hello cíle šířky pásma v rámci účtu hello USA pro geograficky redundantní úložiště (GRS) jsou 10 gigabity za sekundu (Gbps) pro příjem příchozích dat (dat odesílat toohello účet úložiště) a 20 GB/s pro odchozí (data odeslaná z účtu úložiště hello). Pro účet místně redundantní úložiště (LRS) hello omezení jsou vyšší – 20 GB/s pro příjem příchozích dat a 30 GB/s pro odchozí.  Omezení šířky pásma mezinárodní může být nižší a nachází se na naše [stránky cíle škálovatelnosti](http://msdn.microsoft.com/library/azure/dn249410.aspx).  Další informace o možnosti redundance úložiště hello najdete v tématu hello odkazy v [užitečné zdroje](#sub1useful) níže.  

#### <a name="what-toodo-when-approaching-a-scalability-target"></a>Jaké toodo při blíží cíle škálovatelnosti
Pokud vaše aplikace se blíží hello cíle škálovatelnosti účtu jedno úložiště, vezměte v úvahu přijetí jednu z následujících postupů hello:  

* Nebyla hello zatížení, které způsobí, že vaše aplikace tooapproach nebo překročí hello škálovatelnost cíl. Můžete navrhnete ho jinak toouse menší šířku pásma nebo kapacity nebo méně transakce?
* Pokud aplikace nesmí být větší než jedna cíle škálovatelnosti hello, měli byste vytvořit více účtů úložiště a oddíl data aplikací v těchto více účtů úložiště. Pokud používáte tento vzor, pak být zda toodesign vaší aplikace, můžete přidat další účty úložiště v hello budoucí pro vyrovnávání zatížení. V době psaní může mít každé předplatné Azure too100 úložiště účtů.  Účty úložiště také mít žádné náklady než vaše využití z hlediska uložených dat, transakcí provedené nebo data přenesená.
* Pokud vaše aplikace dotkne cíle hello šířky pásma, zvažte komprese dat služby hello klient tooreduce hello pásma potřebná toosend hello data toohello úložiště.  Všimněte si, že i když to může uložit šířky pásma a zlepšit výkon sítě, může také obsahovat některé negativní dopad.  By měly vyhodnotit vliv na výkon hello to z důvodu toohello další zpracování požadavků pro komprese a dekomprese dat v klientovi hello. Kromě toho ukládání komprimovaných dat může být obtížnější tootroubleshoot problémy vzhledem k tomu může být obtížnější tooview uložená data pomocí standardních nástrojů.
* Pokud vaše aplikace dotkne hello cíle škálovatelnosti, pak se ujistěte, že používáte exponenciálního omezení rychlosti pro opakování (v tématu [opakování](#subheading14)).  Její lepší toomake se nikdy přístupu cíle škálovatelnosti hello (pomocí jedné z výše metody hello), ale tím bude zajištěno, vaše aplikace nebude zachovat právě opakování rychle, provedení hello omezení nejhorší.  

#### <a name="useful-resources"></a>Užitečné materiály
Následující odkazy Hello poskytují další podrobnosti o cíle škálovatelnosti:

* V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) informace o cíle škálovatelnosti.
* V tématu [replikace Azure Storage](storage-redundancy.md) a hello příspěvku na blogu [možnosti redundance úložiště Azure a geograficky redundantní úložiště s přístupem pro čtení](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) informace o možnostech redundance úložiště.
* Aktuální informace o cenách pro služby Azure najdete v tématu [Azure ceny](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Zásady vytváření názvů oddílu
Úložiště Azure používá na základě rozsahu rozdělení schéma tooscale a zatížení vyrovnávání hello systém. klíč oddílu Hello je použité toopartition data do rozsahů a tyto rozsahy jsou vyrovnávání zatížení sítě v rámci systému hello. To znamená, že zásady vytváření názvů jako je například lexikální řazení (např. msftpayroll, msftperformance, msftemployees atd.) nebo pomocí časová razítka (log20160101, log20160102, log20160102 atd.) bude jít toohello oddíly se potenciálně společně umístěný na hello stejný server oddílu e-mailů, dokud operace Vyrovnávání zatížení je rozdělí na menší rozsahy. Například všech objektů BLOB do kontejneru, dají obsluhovat jedním serverem, dokud hello zatížení na tyto objekty BLOB vyžaduje další vyrovnává hello oddílu rozsahy. Podobně, může zpracovat skupinu lehkým načíst účty s jejich názvy uspořádané lexikální pořadí v jediném serveru dokud hello zatížení na jednom nebo všechny tyto účty vyžadují, je toobe rozdělení více serverů oddíly. Každé rozložení zátěže operace může mít vliv hello latence úložiště volání během operace hello. možnost toohandle Hello systému, které nečekané shluku provoz tooa oddílu je omezena hello škálovatelnost serveru jednoho oddílu dokud operace rozložení zátěže hello zejména kopance v a znovu vytvoří rovnováhu rozsah klíče oddílu hello.  

Můžete provést některé osvědčené postupy tooreduce hello frekvence těchto operací.  

* Zkontrolujte hello zásady vytváření názvů, který použijete pro účty, kontejnerů, objektů BLOB, tabulek a front, úzce. Vezměte v úvahu prefixu názvy účtů se hodnota hash 3 číslice pomocí funkce algoritmu hash, který nejlépe vyhovuje vašim potřebám.  
* Pokud je uspořádat svá data pomocí funkce časová razítka nebo číselné identifikátory, máte tooensure nepoužíváte vzory přenosů dat připojovacím (nebo jen předřazení). Tyto vzory nejsou vhodné pro rozsah – na základě rozdělení do oddílů systému, a může realizace tooall hello přenosy probíhající tooa jednoho oddílu a omezení systému hello z efektivně Vyrovnávání zatížení. Například pokud máte denně operace, které používají objekt blob s časovým razítkem například RRRRMMDD, pak všechny hello přenosy pro, který je každodenní operace přesměruje tooa jeden objekt, který obsluhovaného serverem jeden oddíl. Podívejte se na tom, jestli hello jednotlivých objektů blob omezení a oddílu omezuje splňují vašim potřebám a vezměte v úvahu rozdělení tuto operaci na více objektů BLOB v případě potřeby. Podobně pokud ukládáte data řady čas v tabulkách, veškerý provoz hello může být přesměrován toohello poslední část hello klíče oboru názvů. Pokud musíte použít časová razítka nebo číselné ID, předponu id hello hash 3 číslice nebo v případě hello časová razítka předponu hello sekund součástí hello čas, jako je třeba ssyyyymmdd. Pokud výpis a dotazy operace se pravidelně provádí, zvolte funkce algoritmu hash, který omezí vaší počet dotazů. V jiných případech může být náhodné předpony.  
* Další informace o vytváření oddílů používané ve službě Azure Storage schéma hello číst hello studie sosp [zde](http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Sítě
Při hello API volá ohledu na to, často omezení fyzické sítě hello hello aplikací mít významný dopad na výkon. Následující Hello popisují některá omezení, které uživatelé setkat.  

#### <a name="client-network-capability"></a>Možnost klienta sítě
##### <a name="subheading2"></a>Propustnost
Šířka pásma hello problém je často hello možnosti hello klienta. Například při účet jednoho úložiště může zpracovat 10 GB/s nebo více příjem příchozích dat (viz [cíle škálovatelnosti šířky pásma](#sub1bandwidth)), hello rychlost sítě v instanci Role pracovního procesu "Malá" Azure je pouze schopná přibližně 100 MB/s. Větší Azure instance mají síťové adaptéry s větší kapacitou, měli byste zvážit použití větší instance nebo více virtuálních počítačů, pokud potřebujete vyšší limity síťové z jednoho počítače. Pokud se přístup k službě úložiště z aplikace na místní, pak hello stejné pravidlo: pochopení možnosti síťového hello hello klientského zařízení a hello síťové připojení toohello umístění úložiště Azure a je buď podle potřeby zvýšit nebo návrh toowork vaší aplikace v rámci jejich možnosti.  

##### <a name="subheading3"></a>Odkaz kvality
Stejně jako u jakékoli využití sítě, mějte na paměti, že síťové podmínky, což vede k chybám a ztrátě paketů zpomalí efektivní propustnost.  Pomocí WireShark nebo NetMon může pomoci při diagnostice tento problém.  

##### <a name="useful-resources"></a>Užitečné materiály
Další informace o velikosti virtuálních počítačů a přidělené šířky pásma najdete v tématu [velikosti virtuálních počítačů Windows](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [velikosti virtuálního počítače s Linuxem](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Umístění
V distribuovaném prostředí umístění klienta hello téměř toohello server poskytuje nejlepší výkon hello. Pro přístup k úložišti Azure s nejnižší latencí hello, hello nejlepší umístění pro vašeho klienta je v rámci hello stejné oblasti Azure. Například pokud máte webovou stránku Azure, která používá Azure Storage, byste měli vyhledat oba v jedné oblasti (například USA – západ nebo jihovýchodní Asie). Tím se snižuje latence hello a hello náklady – v době psaní textu hello je bezplatná využití šířky pásma v jedné oblasti.  

Pokud váš klient, které aplikace nejsou hostované v rámci Azure (například aplikací pro mobilní zařízení nebo na místní enterprise services), pak znovu umístění účtu úložiště hello v oblasti téměř toohello zařízení, které budou přistupovat, se obecně sníží latence. Pokud vaši klienti distribuují široce (pro příklad, některé v Severní Americe a některé v Evropě), pak byste měli zvážit použití více účtů úložiště: jeden umístěný v oblasti Severní Ameriky a druhý v Evropského oblast. To vám pomůže tooreduce latence pro uživatele v obou oblastí. Tento přístup je obvykle jednodušší tooimplement Pokud hello úložiště aplikací hello data tooindividual konkrétní uživatelé a nevyžaduje replikaci dat mezi účty úložiště.  Pro širokou distribuci obsahu doporučuje se název CDN – viz další část hello další podrobnosti.  

### <a name="subheading5"></a>Distribuce obsahu
V některých případech aplikace potřebám tooserve hello stejným uživatelům obsahu toomany (například produktu ukázku použít hello domovské stránce webu video), umístěný ve buď hello stejné nebo více oblastí. V tomto scénáři měli byste použít Content Delivery Network (CDN) jako je například Azure CDN a hello CDN by používat úložiště Azure jako hello původu dat hello. Na rozdíl od účet úložiště Azure, která existuje v jedné oblasti a který nelze doručování obsahu s nízkou latencí tooother oblasti používá Azure CDN servery v několika datových centrech kolem hello, world. Název CDN kromě toho může podporovat obvykle mnohem vyšší limity odchozí než účet jednoho úložiště.  

Další informace o Azure CDN najdete v tématu [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>Pomocí SAS a CORS
Pokud budete potřebovat tooauthorize kódu třeba JavaScript v prohlížeči sítě nebo mobilní telefon aplikace tooaccess dat ve službě Azure Storage, jeden z přístupů je toouse aplikace v webovou roli jako proxy server: zařízení hello uživatele ověří hello webovou roli, které v zapnout ověřuje se službou hello úložiště. Tímto způsobem se můžete vyhnout vystavení klíče účtu úložiště na nezabezpečené zařízení. Ale to umístí big režijní náklady na hello webovou roli protože všechny hello dat přenesených mezi zařízení hello uživatele a hello služba úložiště musí projít hello webové role. Používání webové role jako proxy pro službu úložiště hello pomocí sdíleného přístupového podpisy (SAS), někdy ve spojení s hlavičky sdílení prostředků různých původů (CORS) se můžete vyhnout. Pomocí SAS, můžete povolit uživatele zařízení toomake požadavky přímo tooa úložiště služby prostřednictvím token omezený přístup. Například pokud chce uživatel tooupload k aplikaci tooyour fotografií, webovou roli můžete vygenerovat a odeslat zařízení toohello uživatele token SAS, která uděluje oprávnění toowrite tooa konkrétní blob nebo kontejner pro hello další 30 minut (po jejímž hello tokenu SAS vyprší platnost).

Za normálních okolností neumožní prohlížeče JavaScript na stránce hostované na web jeden domény tooperform konkrétních operací například tooanother domény "PUT". Například pokud hostujete webovou roli na "contosomarketing.cloudapp.net" a chcete toouse klienta straně JavaScript tooupload účtu úložiště blob tooyour v "contosoproducts.blob.core.windows.net", hello prohlížeče "stejné zásady původ" bude nezakazuje to operace. CORS je funkce prohlížeče, která umožňuje hello cílové doméně (v rámci tohoto účtu úložiště případu hello) toocommunicate toohello prohlížeč, který důvěřovat požadavky pocházejících z hello zdrojovou doménu (v této roli webové případu hello).  

Obě tyto technologie vám může pomoct vyhnout nepotřebné zatížení (a kritická místa) na vaši webovou aplikaci.  

#### <a name="useful-resources"></a>Užitečné materiály
Další informace o tokenu SAS naleznete v tématu [sdílené přístupové podpisy, část 1: hello vysvětlení modelu SAS](../storage-dotnet-shared-access-signature-part-1.md).  

Další informace o CORS, najdete v části [Podpora sdílení prostředků různých původů (CORS) pro hello služby úložiště Azure](http://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Ukládání do mezipaměti
#### <a name="subheading7"></a>Získání dat
Obecně platí získávání dat ze služby jednou je lepší, než ho získávání dvakrát. Zvažte hello příklad spuštěna ve webové roli již načtení objektu blob 50MB z hello úložiště služby tooserve jako uživatel obsahu tooa webové aplikaci MVC. aplikace Hello může pak načtení tomuto stejné objektu blob pokaždé, když uživatel požádá, nebo ho může uložení do mezipaměti místně toodisk a opětovné používání hello uložené v mezipaměti verze pro následné požadavky uživatelů. Kromě toho vždy, když uživatel požádá o hello dat, hello aplikace může dojít, problém s podmíněného hlavičku pro čas změny, které by se tak získání celý objekt blob hello, pokud byl změněn. Můžete použít tento stejný vzor tooworking s entity tabulky.  

V některých případech se můžete rozhodnout, že vaše aplikace můžete předpokládat, že tomuto objektu blob hello zůstane platný na krátkou dobu, po načtení a že během tohoto období hello aplikace není nutné toocheck Pokud hello blob byla změněna.

Konfigurace, vyhledávání a další data, která jsou vždy používá aplikace hello jsou skvělí kandidáti pro ukládání do mezipaměti.  

Příklad jak tooget objekt blob vlastnosti toodiscover hello položka Datum poslední úpravy pomocí rozhraní .NET, najdete v tématu [sady a vlastnosti načíst a Metadata](../blobs/storage-properties-metadata.md). Další informace o podmíněného soubory ke stažení najdete v tématu [podmíněně aktualizovat místní kopii objektu Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Nahrávání dat v dávkách
V některých případech aplikace můžete agregovat data v místním počítači a pak ho pravidelně nahrajte v dávce místo odesílání každá položka dat okamžitě. Webové aplikace může například zachovat soubor protokolu aktivit: hello aplikace by buď Odeslat podrobnosti o všech aktivit jako Odehrává se jako entitu tabulky (který vyžaduje mnoho operace úložiště), nebo může uložit aktivity podrobnosti tooa místního souboru protokolu a potom pravidelně nahrajte všechny podrobnosti o aktivitě jako objekt blob tooa souboru s oddělovači. Pokud každé položky protokolu 1KB velikostí, můžete nahrát tisícům v rámci jedné transakce "Put Blob" (můžete nahrát objekt blob z až too64MB velikost v rámci jedné transakce). Samozřejmě pokud místní počítač hello dojde k chybě odesílání předchozí toohello, potenciálně ztratíte některá data protokolu: musí vývojář aplikace hello návrh pro možnost hello klientské zařízení nebo nahrát selhání.  Pokud data aktivit hello potřebuje toobe stáhnout pro timespans (pouze jediné aktivity), objekty BLOB se doporučuje namísto tabulky.

### <a name="net-configuration"></a>Konfigurace rozhraní .NET
Pokud pomocí hello rozhraní .NET Framework, této části jsou uvedené několik rychlé konfiguraci nastavení, které můžete použít toomake významné zlepšení výkonu.  Pokud používáte další jazyky, zkontrolujte toosee Pokud podobné koncepty použít ve zvoleném jazyce.  

#### <a name="subheading9"></a>Zvyšte výchozí limit připojení
V rozhraní .NET, hello následující kód zvyšuje hello výchozí připojení limit (což je obvykle 2 v prostředí klienta nebo v prostředí serverů 10) too100. Obvykle byste měli nastavit hello hodnota tooapproximately hello počet vláken používaných aplikací.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Je nutné nastavit limitu připojení hello před otevřením všechna připojení.  

Pro jiné programovací jazyky najdete v části toodetermine dokumentace tento jazyk, jak omezit tooset hello připojení.  

Další informace najdete v tématu hello blogu [webové služby: souběžných připojení](http://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>Pokud synchronní kódu pomocí úloh s modifikátorem Async zvětšete fondu vláken Min.
Tento kód se zvýší hello podprocesy z fondu podprocesů min:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine hello right number for your application)  
```

Další informace najdete v tématu [ThreadPool.SetMinThreads metoda](http://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>Uvolňování paměti .NET 4.5, výhod
Pro hello klienta aplikace tootake výhod vylepšení výkonu v kolekce paměti na serveru pomocí rozhraní .NET 4.5 nebo novější.

Další informace najdete v článku hello [přehled o vylepšení výkonu v rozhraní .NET 4.5](http://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Bez vazby paralelismus
Paralelismus mohou být ideální pro výkon, být opatrní při používání více oddílů dat tooupload nebo stažení bez vazby paralelismus (bez omezení na hello počet vláken a/nebo paralelní požadavky), pomocí více tooaccess pracovních procesů (kontejnery, fronty, nebo Tabulka oddílů) v hello stejný účet úložiště nebo tooaccess více položek v hello stejného oddílu. Pokud hello paralelismus bez vazby, vaše aplikace můžete překročit možnosti hello klientských zařízení nebo hello cíle škálovatelnosti účtu úložiště, výsledkem je delší latence a omezení.  

### <a name="subheading13"></a>Knihovny klienta úložiště a nástroje
Vždy používejte hello nejnovější společnosti Microsoft klientské knihovny a nástroje. V době psaní hello jsou k dispozici pro rozhraní .NET, Windows Phone, prostředí Windows Runtime, Java a C++ klientské knihovny, jakož i preview knihovny pro jiné jazyky. Kromě toho společnost Microsoft vydala rutiny prostředí PowerShell a rozhraní příkazového řádku Azure pro práci s Azure Storage. Společnost Microsoft aktivně sama vyvinula tyto nástroje s výkonem na paměti, zajišťuje jejich až toodate s hello nejnovější verze služby a zajistí, že jejich řadu hello osvědčené postupy z hlediska výkonu, jako interně zpracovat.  

### <a name="retries"></a>Opakování
#### <a name="subheading14"></a>Omezení šířky pásma nebo ServerBusy
V některých případech hello úložiště služba může omezení aplikace nebo může jednoduše být nelze tooserve hello požadavek z důvodu přechodného stavu toosome a vrátí zprávu "503 Server je zaneprázdněný" nebo "500 časový limit".  Tomu může dojít, pokud vaše aplikace se blíží některé z hello cíle škálovatelnosti, nebo pokud hello systému je vyrovnává vaší tooallow oddílů dat pro vyšší propustnost.  Hello klientská aplikace obvykle zopakovat operaci hello, který způsobuje, že takové chyby: Pokus hello stejné žádosti později mohlo být úspěšné. Ale pokud služba úložiště hello je omezení aplikace, protože přesahuje cíle škálovatelnosti, nebo i v případě, že hello službě se nelze tooserve hello požadavku z jiného důvodu, agresivní opakování obvykle provést nejhorší problém hello. Z tohoto důvodu byste měli používat exponenciální regrese (hello knihovny výchozí toothis chování klienta). Vaše aplikace může například opakovat po 2 sekundy, pak 4 a víc sekund, a 10 sekund a potom 30 sekund a úplně uvolňovat. Toto chování je výsledkem aplikace výrazně snížit jeho zátěž služby hello spíše než ke zhoršení potíže.  

Všimněte si, že k chybám připojení můžete opakovat okamžitě, protože nejsou hello výsledek omezení a jsou očekávané toobe přechodný.  

#### <a name="subheading15"></a>Neopakovatelné chyby
Hello klientské knihovny jsou vědět, které chyby jsou možné opakovat a které nejsou. Pokud vytváříte vlastní kód pro REST API hello úložiště, pamatujte si však některé chyby, které by neměly opakujte: například 400 (Chybný požadavek) v odpovědi vyplývá, že klientská aplikace hello odesílá požadavek, který nelze zpracovat, protože ho nebyla ve formuláři očekávané. Odešlete tento požadavek způsobí hello stejné odpovědi pokaždé, když, proto se nenachází žádný bod v jeho opakování. Pokud vytváříte vlastní kód pro REST API hello úložiště, mějte na jaké chyby hello kódy tooretry správný způsob střední a hello (nebo ne) pro každý z nich.  

#### <a name="useful-resources"></a>Užitečné materiály
Další informace o chybových kódech úložiště najdete v tématu [stavu a kódy chyb](http://msdn.microsoft.com/library/azure/dd179382.aspx) na webu Microsoft Azure hello.  

## <a name="blobs"></a>Objekty blob
V přidání toohello osvědčené postupy pro [všechny služby](#allservices) popsáno výše, hello následující osvědčené postupy, jako se týkají konkrétně toohello služby objektů blob.  

### <a name="blob-specific-scalability-targets"></a>Cíle škálovatelnosti specifické objektů BLOB
#### <a name="subheading46"></a>Víc klientů současně přístup k jednoho objektu.
Pokud máte velký počet klientů přistupovat k jednoho objektu souběžně budete potřebovat tooconsider na objekt a úložiště cíle škálovatelnosti účtu. Hello přesný počet klientů, kteří mohou přistupovat k jednoho objektu budou lišit v závislosti na faktorech, jako je například hello počet klientů najednou, požaduje objekt hello hello velikost objektu hello, síťové podmínky atd.

Pokud objekt hello mohou být distribuovány prostřednictvím obsluhovat CDN například obrázky nebo videa z webu potom byste měli použít název CDN. V tématu [zde](#subheading5).

V dalších scénářích, třeba scientific simulace, kde je důvěrných dat hello máte dvě možnosti. Hello je nejdřív toostagger, které vaše úlohy přístupu taková hello objekt přistupuje během období čas vs přistupuje současně. Alternativně můžete zkopírovat dočasně hello objekt toomultiple účty úložiště a zvýšit tak hello celkový počet IOPS podle objektu a mezi různými účty úložiště. V omezené testování bylo zjištěno, že přibližně 25 virtuálních počítačů může současně stáhnout objekt blob 100GB paralelně (každý virtuální počítač byl paralelním prováděním hello stažení použití 32 vláken). Pokud jste měli 100 klientů tooaccess hello objektu, která potřebuje, nejdřív zkopírujte jej tooa druhého účtu úložiště a potom mít hello prvních 50 virtuálních počítačů přístup hello první objekt blob a hello druhý 50 virtuálních počítačů přístup hello druhý objekt blob. Výsledky se liší podle chování vaší aplikace, takže byste měli otestovat při návrhu. 

#### <a name="subheading16"></a>Operace na objektu Blob a šířky pásma
Můžou číst nebo zapisovat tooa jediného objektu blob na až tooa nesmí být delší než 60 MB za sekundu (Toto je přibližně 480 MB/s překročený hello možnosti mnoho sítí straně klienta (včetně hello fyzickou síťovou kartu hello klientského zařízení). Kromě toho jediného objektu blob podporuje až too500 požadavků za sekundu. Pokud máte více klientů, které je třeba tooread hello stejný objekt blob a může tato omezení překročí, měli byste zvážit použití název CDN pro distribuci hello objektů blob.  

Další informace o cílové propustnost pro objekty BLOB najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Kopírování a přesouvání objektů BLOB
#### <a name="subheading17"></a>Zkopírování objektu Blob
REST API verze 2012-02-12 Hello úložiště zavedená hello užitečné možnost toocopy objekty BLOB mezi účty: klientskou aplikaci můžete vyzvat hello úložiště služby toocopy objekt blob z jiného zdroje (například ve jiný účet úložiště) a nechte hello Služba provést hello kopie asynchronně. To může výrazně snížit šířku pásma hello potřebné pro aplikace hello, pokud jste migraci dat z jiné účty úložiště, protože není nutné toodownload a nahrání dat hello.  

Jednou z těchto okolností, ale je, že při kopírování souborů mezi účty úložiště, je možné, čas na při kopírování hello dokončí. Pokud aplikace potřebuje toocomplete objekt blob zkopírujte rychle ve vaší kontrole, je lepší toocopy hello blob podle stáhnout tooa virtuálních počítačů a odesílá je toohello cílový.  Pro úplnou předvídatelnost v takovém případě zajistit, aby kopie hello je pomocí virtuálního počítače s v hello stejné oblasti Azure, jinak se síťové podmínky může (a pravděpodobně bude) ovlivnit výkon kopírování.  Kromě toho můžete sledovat průběh hello asynchronní kopie prostřednictvím kódu programu.  

Všimněte si, že zkopíruje v rámci hello stejný účet úložiště, samotné jsou obecně dokončit rychle.  

Další informace najdete v tématu [kopírovat objekt Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>Pomocí nástroje AzCopy
týmu Azure Storage Hello vydala nástroj příkazového řádku "AzCopy" tedy určeny toohelp s hromadného přenosu mnoho objektů BLOB do z a mezi různými účty úložiště.  Tento nástroj je optimalizovaná pro tento scénář a můžete dosáhnout vysoké přenosové rychlosti.  Pro hromadné odesílání, stahování a scénáře kopírováním je podporováno jeho používání. Další informace o jeho toolearn a stažení naleznete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Služba Azure Import/Export
Pro velmi velkých objemů dat (více než 1TB) hello Azure Storage nabízí hello importu/exportu služby, která umožňuje odesílání a stahování z úložiště objektů blob podle přenosů pevné disky.  Můžete uvést vaše data na pevném disku a odeslat tooMicrosoft pro odeslání nebo odeslání dat toodownload tooMicrosoft prázdné pevný disk.  Další informace najdete v tématu [použít hello službu Microsoft Azure Import/Export tooTransfer Data tooBlob úložiště](../storage-import-export-service.md).  To může být mnohem efektivnější než nahrávání nebo stahování tento svazek dat přes síť hello.  

### <a name="subheading20"></a>Používání metadat
Služba objektů blob Hello podporuje head požadavků, které mohou zahrnovat metadata o hello objektů blob. Například hello EXIF dat mimo fotografie potřeby vaší aplikace je může načíst hello fotografie a rozbalte ho. toosave šířky pásma a zvýšit výkonnost, vaše aplikace může hello EXIF data ukládat v metadata objektu blob hello při aplikace hello nahrán hello fotografií: můžete pak hello EXIF data načíst v metadatech pomocí pouze žádost HEAD, ukládání významné šířky pásma a doba zpracování hello potřebných tooextract hello EXIF data, která je pro každý objekt blob hello čas čtení. To může být užitečný ve scénářích, kde potřebujete pouze hello metadata a není hello celý obsah objektu blob.  Všimněte si, že pouze 8 KB metadata mohou být uloženy na objekt blob (hello služba nebude přijímat žádosti o toostore více, než který), takže pokud hello dat v této velikosti nevejde, nesmíte mít možnost toouse tento přístup.  

Pro příklad tooget metadata objekt blob pomocí rozhraní .NET, najdete v části [sady a vlastnosti načíst a Metadata](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Rychlé nahrávání
objekty BLOB tooupload rychlé, je první otázku tooanswer hello: jste odesílání jeden objekt blob nebo mnoho?  Použijte hello níže pokyny toodetermine hello správné metoda toouse v závislosti na vašem scénáři.  

#### <a name="subheading21"></a>Rychle odesílání jednoho velkého objektu blob
tooupload jednoho velkého objektu blob rychle, klientskou aplikaci měli nahrávat stránkách paralelně (Probíhá hello cíle škálovatelnosti pro jednotlivé objekty BLOB a účet úložiště hello jako celek s vědomím) nebo jeho bloky.  Všimněte si, že hello oficiální poskytovaný společností Microsoft RTM úložiště knihovny klienta (.NET, Java) mají možnost toodo hello to.  Pro každou z knihovny hello použijte níže zadaný objekt nebo vlastnost tooset hello úroveň souběžnosti hello:  

* .NET: Sada ParallelOperationThreadCount na toobe BlobRequestOptions objekt používat.
* Javě a Androidu: Použít BlobRequestOptions.setConcurrentRequestCount()
* Node.js: Použijte parallelOperationThreadCount na možnosti žádost buď hello nebo hello služby objektů blob.
* C++: Použijte metodu blob_request_options::set_parallelism_factor hello.

#### <a name="subheading22"></a>Rychlé nahrávání mnoho objektů BLOB
tooupload mnoho objektů BLOB rychle, nahrajte objekty BLOB paralelně. Toto je rychlejší než při odesílání jednoho objekty BLOB současně paralelní blok nahrávání ve protože šíří se nahrávání hello napříč více oddílů služby úložiště hello. Jediného objektu blob podporuje pouze propustnost 60 MB za sekundu (přibližně 480 MB/s). V době psaní textu hello účet na základě USA LRS podporuje až too20 příchozího GB/s, což je mnohem víc než hello propustnost nepodporuje jednotlivých objektů blob.  [AzCopy](#subheading18) ve výchozím nastavení provádí nahrávání paralelně a doporučuje se pro tento scénář.  

### <a name="subheading23"></a>Výběr správného typu hello objektu blob
Úložiště Azure podporuje dva typy objektů blob: *stránky* objekty BLOB a *bloku* objekty BLOB. Pro danou využití scénáře ovlivní zvoleného typu Objekt blob hello výkon a škálovatelnost řešení. Objekty BLOB bloku jsou vhodné, když chcete tooupload velké objemy dat, efektivně: například klientské aplikace může být nutné tooupload fotografie nebo video tooblob úložiště. Objekty BLOB stránky jsou vhodné, pokud aplikace hello musí tooperform náhodné zápisy na hello data: například Azure virtuální pevné disky jsou uložené jako objekty BLOB stránky.  

Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tabulky
V přidání toohello osvědčené postupy pro [všechny služby](#allservices) popsáno výše, hello následující osvědčené postupy, jako se týkají konkrétně toohello služby table.  

### <a name="subheading24"></a>Cíle škálovatelnosti specifické pro tabulky
V omezení šířky pásma toohello přidání účtu úložiště celý tabulky obsahovat hello následující limit konkrétní škálovatelnosti.  Všimněte si, že systém hello bude Vyrovnávání zatížení jako vaše zvýšení provozu, ale pokud provozu nečekané shluky, nesmíte být schopný tooget tento svazek propustnost okamžitě.  Pokud má vaše vzor shluky, byste měli očekávat toosee omezení a/nebo vypršení časových limitů během hello shluků jako službu úložiště hello automaticky vyrovnává zatížení na tabulku.  Ramping až pomalu obecně má lepší výsledky jako nabízí hello systému času tooload vyrovnávání správně.  

#### <a name="entities-per-second-account"></a>Entity za sekundu (účet)
limit škálovatelnosti Hello pro přístup k tabulky zapnutý too20 000 entity (1KB každé) za sekundu pro účet.  Obecně platí každá entita, která je vložit, aktualizovat, odstranit nebo zkontrolovat počty směrem k této cílové.  Proto by dávkové vložení, který obsahuje 100 entit počítají jako 100 entit.  Dotaz, který vyhledá 1000 entity a vrátí 5 by se počítají jako 1000 entity.  

#### <a name="entities-per-second-partition"></a>Entity za sekundu (oddíl)
V rámci jednoho oddílu, hello škálovatelnost cíl pro přístup k tabulek je 2 000 entity (1KB každé) za sekundu, pomocí hello stejné počítání jak je popsáno v předchozí části hello.  

### <a name="configuration"></a>Konfigurace
Tato část obsahuje několik nastavení rychlé konfigurace používané toomake výrazné vylepšení výkonu v hello služby table.  

#### <a name="subheading25"></a>Použití JSON
Počínaje verzí služby úložiště 2013-08-15, služby table hello podporuje používání JSON místo hello založený na jazyce XML AtomPub formát pro přenos dat v tabulce. To může snížit velikost datové části tím, co nejvíce 75 % a může výrazně zlepšit výkon hello vaší aplikace.

Další informace najdete v tématu hello post [tabulek Microsoft Azure: Úvod JSON](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) a [Formát datové části pro operace služby s tabulkou](http://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle vypnuto
Na Nagle algoritmus je široce implementovaný v sítích TCP/IP jako výkonu znamená tooimprove sítě. Však není optimální za všech okolností (například vysoce interaktivní prostředí). Pro Azure Storage algoritmus je Nagle má negativní dopad na výkon hello požadavky toohello tabulky a fronty služeb a zakažte je-li to možné.  

Další informace najdete v tématu naše blogu [na Nagle algoritmus není popisný směrem malé požadavky](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), která vysvětluje, proč algoritmus Nagle je špatně komunikuje s tabulky a fronty požadavků a ukazuje, jak toodisable do vašeho klienta aplikace.  

### <a name="schema"></a>Schéma
Jak představují a dotazování na data je hello největších jeden faktor, který ovlivňuje výkon hello služby table hello. Při každá aplikace, se liší, tato část popisuje některé obecné osvědčené postupy, které se vztahují k:  

* Návrh tabulky
* Efektivní dotazy
* Aktualizace efektivní dat  

#### <a name="subheading27"></a>Tabulky a oddíly
Tabulky jsou rozděleny do oddílů. Každé entity uložené v oddílu sdílených složek hello stejným klíčem oddílu a má řádek jedinečné klíče tooidentify ho v rámci oddílu. Oddíly výhody, ale také zavádět omezení škálovatelnosti.  

* Výhody: Můžete aktualizovat entity v hello stejné oddílu v jedné, atomic, batch transakci, která obsahuje operace samostatné úložiště too100 (limit je 4MB, celková velikost). Za předpokladu, že hello stejné číslo z toobe entity načíst, můžete se také dotázat dat v rámci jednoho oddílu efektivnější než data, která rozdělena na oddíly (přestože přečtěte si další doporučení na dotazování na data tabulky).
* Limit škálovatelnosti: tooentities přístup uložená v jednom oddílu nesmí být vyrovnávání zatížení sítě vzhledem k tomu oddíly podporují atomic dávkové transakce. Z tohoto důvodu hello škálovatelnost cíl pro jednotlivé tabulky oddíl je nižší než v případě služby table hello jako celek.  

Z důvodu tyto charakteristiky tabulek a oddíly musí přijmout hello následující zásady designu:  

* Data, která klientské aplikace často aktualizovat nebo spustit dotaz ve stejné logické jednotky práce se musí nacházet ve hello hello stejného oddílu.  To může být, protože aplikace je agregování zápisy, nebo protože chcete, aby tootake výhod atomic dávkových operací.  Data v jednom oddílu může být efektivněji dotazována také v jednom dotazu než dat napříč oddíly.
* Data, která klientské aplikace nemá insert nebo update nebo dotazu v hello stejné logické jednotky práce (jeden dotaz nebo aktualizace batch) by měla být umístěná v samostatné oddíly.  Důležité OneNote je, že se žádný limit toohello počet klíčů oddílů v jediné tabulce, takže s miliony klíče oddílů se nejedná o problém a nebude mít vliv na výkon.  Například vaše aplikace je Oblíbené web s přihlášení uživatele, pomocí hello Id uživatele jako klíč oddílu hello by dobrou volbou.  

#### <a name="hot-partitions"></a>Aktivní oddíly
Aktivní oddíl je ten, který přijímá nesoustředil příliš velký procento hello provoz tooan účtu a nemůže být s vyrovnáváním zatížení se vzhledem k tomu, že je jeden oddíl.  Obecně platí aktivní oddíly jsou vytvořeny jedním ze dvou způsobů:  

##### <a name="subheading28"></a>Připojit pouze a předřazení pouze vzorce
Vzor "Pouze přidat" Hello je jedním kde všechny (nebo téměř všechny) hello provoz tooa zadané Primárníklíč zvyšuje a snižuje podle toohello aktuální čas.  Příkladem je, pokud vaše aplikace používá hello aktuální datum jako klíč oddílu pro data protokolu.  Výsledkem celé hello vloží budete toohello poslední oddíl v tabulce a hello systému nedá vyrovnávat zatížení, protože všechny hello zápisů jsou probíhající toohello konec tabulku.  Pokud svazek hello provoz toothat oddílu je větší než cílový oddíl úrovni škálovatelnost hello, je výsledkem omezení.  Je lepší tooensure, který jsou data odesílána toomultiple oddíly hello Vyrovnávání zatížení tooenable požadavky napříč tabulku.  

##### <a name="subheading29"></a>Data vysokým provozem
Pokud výsledky schéma rozdělení oddílů v jeden oddíl, jenom data, která je mnohem víc než ostatní oddíly, může se také zobrazit omezení jako oddílu blíží hello škálovatelnost cíl pro jeden oddíl.  Je lepší toomake jistotu, že výsledky schéma oddílu v žádné jednoho oddílu blížící cíle škálovatelnosti hello.  

#### <a name="querying"></a>Dotazování
Tato část popisuje osvědčené postupy pro dotazování služby table hello.  

##### <a name="subheading30"></a>Obor dotazu
Existuje několik způsobů toospecify hello řadu tooquery entity.  Hello následuje popis hello používá jednotlivých.  

Obecně platí Vyhněte se kontroly (dotazy větší než jedna entita), ale pokud je musí skenování, zkuste tooorganize vaše data, aby kontrolách načítat data hello, potřebujete bez kontrola nebo vrácení poměrně velké množství entity, které nepotřebujete.  

###### <a name="point-queries"></a>Bod dotazy
Bod dotaz načte přesně jednu entitu. Dělá to tak, že zadáte hello klíč oddílu a klíč řádku hello entity tooretrieve. Tyto dotazy jsou velmi efektivně a jejich použití je to možné.  

###### <a name="partition-queries"></a>Oddíl dotazy
Dotaz oddílu je dotaz, který načte sadu dat, který sdílí společný klíč oddílu. Obvykle hello dotazu určuje rozsah hodnot klíče řádek nebo rozsah hodnot pro vlastnost některé entity v klíč oddílu tooa přidání. Tyto jsou míň efektivní než bodu dotazy a měli šetřit.  

###### <a name="table-queries"></a>Tabulku dotazů
Dotaz na tabulku je dotaz, který načte sada entit, které nesdílí běžné klíč oddílu. Tyto dotazy nejsou efektivní a neměli byste je-li to možné.  

##### <a name="subheading31"></a>Hustotu dotazu
Dalším klíčovým faktorem efektivitu dotazu je hello počet entit, které jsou vrácena jako porovnání toohello počet entit naskenované toofind hello vrátil nastavte. Pokud vaše aplikace provede dotaz na tabulku s filtrem pro hodnotu vlastnosti, pouze 1 % hello dat sdílené složky, hello dotazu vyhledá 100 entit pro každý jedna entita, kterou vrátí. Hello cíle škálovatelnosti tabulky popisovaný dříve všechny související toohello počet entit, zkontrolovat a není hello počet entit vrátil: hustotu nízkou dotaz může snadno způsobit hello tabulky služby toothrottle aplikace protože se musí kontrolovat mnoho Entita hello tooretrieve entity, které hledáte.  Projděte si část hello níže na [denormalization](#subheading34) Další informace o tom, tooavoid to.  

##### <a name="limiting-hello-amount-of-data-returned"></a>Omezení hello velikost vrácená Data
###### <a name="subheading32"></a>Filtrování
Pokud jste si jisti, že dotaz vrátí entit, které nepotřebujete v aplikaci klienta hello, vzít v úvahu pomocí filtru tooreduce hello velikost hello vrátila sadu. Při hello entity nebyla vrácena toohello klienta stále započítává k omezení škálovatelnosti hello, výkon aplikace bude zlepšit kvůli hello snížit velikost datové části sítě a hello menší počet entit, které je nutné zpracovat klientské aplikace .  Viz výše Poznámka na [dotazu hustotu](#subheading31), ale – cíle škálovatelnosti hello týkají toohello počet entit, zkontrolovat, takže dotaz, který filtruje mnoho entit může stále mít za následek omezení, i když jsou vráceny několik entit.  

###### <a name="subheading33"></a>Projekce
Pokud klientské aplikace potřebuje pouze omezenou sadu vlastnosti z hello entit v tabulce, můžete projekce toolimit hello velikost hello vrátil datové sady. Stejně jako u filtrování, pomůžete tooreduce zatížení sítě a zpracování na straně klienta.  

##### <a name="subheading34"></a>Denormalization
Na rozdíl od práce relačních databází, hello osvědčené postupy pro efektivní dotazování na data tabulky vést toodenormalizing vaše data. To znamená, duplikování hello stejná data ve více entit (jeden pro každý klíč může používat toofind hello data) toominimize hello počet entit, musí dotaz kontroly požadavků na klienta hello toofind hello dat, místo aby se tooscan velkého počtu toofind entity Hello data aplikace potřebuje.  Například v webem elektronického obchodování můžete toofind řazení obou podle ID zákazníka hello (mě objednávky tohoto zákazníka) a datu hello (mě objednávky na datum).  Ve službě Table Storage, je nejlepší entity toostore hello (nebo odkaz tooit) dvakrát – jednou pro název tabulky, Primárníklíč a pracovní toofacilitate hledání podle ID zákazníka, jednou toofacilitate hledání podle data hello.  

#### <a name="insertupdatedelete"></a>Vložení, aktualizaci nebo odstranění
Tato část popisuje osvědčené postupy pro úpravy entit, které jsou uložené v hello služby table.  

##### <a name="subheading35"></a>Dávkování
Dávkové transakce jsou známé jako Entity skupiny transakce (ETG) ve službě Azure Storage; všechny operace hello v rámci ETG musí být na jeden oddíl v jediné tabulce. Pokud je to možné, použijte v dávkách ETGs tooperform vložení, aktualizace a odstranění. To snižuje hello počet zpátečních cest z aplikačního serveru toohello klienta, snižuje počet hello fakturovatelný transakce (ETG počítá jako jediná transakce pro účely fakturace a může obsahovat až too100 operace úložiště) a umožňuje atomic aktualizace (všechny operace úspěšné nebo všechny selhání v rámci ETG). Prostředí s vysokou latencí, jako jsou například mobilní zařízení budou využívat výrazně ETGs.  

##### <a name="subheading36"></a>Upsert
Pomocí tabulky **Upsert** operations kdykoli je to možné. Existují dva typy **Upsert**, které může být efektivnější než tradiční **vložit** a **aktualizace** operace:  

* **InsertOrMerge**: to použijte, pokud chcete, aby tooupload podmnožinu vlastností hello entity ale nejste si jisti, zda text hello entita již existuje. Pokud existuje hello entity, toto volání aktualizuje vlastnosti hello součástí hello **Upsert** operace a ponechává všechny existující vlastnosti, jako jsou, pokud hello entita neexistuje, vloží novou entitu hello. Toto je podobné projekce toousing v dotazu, v tom, že potřebujete jenom tooupload hello vlastnosti, které mění.
* **InsertOrReplace**: to použijte, pokud chcete, aby tooupload zcela nové entity, ale nejste si jisti, zda již existuje. Používejte pouze to když víte, že hello nově nahrán entity je zcela správné, protože úplně přepíše původní entity hello. Například chcete tooupdate hello entita, která ukládá aktuální umístění uživatele bez ohledu na to, zda aplikace hello dříve uložil data o umístění pro uživatele hello; nové umístění entity Hello je dokončena a není nutné žádné informace z předchozích entitu.

##### <a name="subheading37"></a>Ukládání datových řad v jedné Entity
V některých případech aplikace ukládá řadu dat, je často nutné tooretrieve všechny najednou: například aplikace může sledovat využití procesoru v čase v tooplot pořadí postupné grafu hello dat z hello posledních 24 hodin. Jeden z přístupů je jedna tabulka entity toohave za hodinu, s každé entity představující konkrétní hodinu a ukládání hello využití procesoru pro danou hodinu. tooplot musí tato data, aplikace hello tooretrieve hello entity, která uchovává data hello z hello nejnovější 24 hodin.  

Alternativně může vaše aplikace ukládat využití hello procesoru pro každou hodinu jako samostatné vlastnost jedné entity: tooupdate každou hodinu, vaše aplikace můžete použít jeden **InsertOrMerge Upsert** volání tooupdate hello hodnoty pro hello poslední hodinu. tooplot hello data, hello aplikace potřebuje pouze tooretrieve jedné entity místo 24, díky velmi efektivní dotazu (viz výše diskuzi na [dotaz oboru](#subheading30)).

##### <a name="subheading38"></a>Ukládání strukturovaných dat do objektů BLOB
Někdy strukturovaných dat funguje stejně, jako by měl přejděte v tabulkách, ale rozsahy entit se vždy načítají společně a lze vložit batch.  Dobrým příkladem toho je soubor protokolu.  V takovém případě může několik minut protokolů služby batch, vložte je a pak se vždy načítání najednou i několik minut protokolů.  V takovém případě pro výkon, je lepší objekty BLOB toouse místo tabulky, protože vám může výrazně snížit počet hello zapsány nebo vráceny objekty, stejně jako obvykle hello počet požadavků, které je třeba vytvořit.  

## <a name="queues"></a>Fronty
V přidání toohello osvědčené postupy pro [všechny služby](#allservices) popsáno výše, hello následující osvědčené postupy, jako se týkají konkrétně toohello fronty služby.  

### <a name="subheading39"></a>Omezení škálovatelnosti
Jednou frontou může zpracovat přibližně 2 000 zprávy (1KB každé) za sekundu (každý AddMessage, GetMessage a DeleteMessage počet jako zprávu sem). Pokud to není dostatečné pro vaši aplikaci, musí používat více fronty a zprávy hello rozloženy je.  

Zobrazit aktuální cíle škálovatelnosti v [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle vypnuto
Části hello na konfigurace tabulky, který popisuje hello Nagle algoritmus – algoritmus Nagle hello je obecně chybný hello výkonu fronty požadavků, a zakažte je.  

### <a name="subheading41"></a>Velikost zprávy
Fronty neklesne jako zvyšuje velikost zprávy výkon a škálovatelnost. Měli byste umístit pouze hello informace hello příjemce musí ve zprávě.  

### <a name="subheading42"></a>Načtení batch
Můžete načíst too32 zpráv z fronty v rámci jedné operace. To může snížit počet hello zbytečné komunikace z hello klientskou aplikaci, což je užitečné zejména pro prostředí, jako jsou například mobilní zařízení, s vysokou latencí.  

### <a name="subheading43"></a>Interval dotazování fronty
Většina aplikací dotazování na zprávy z fronty, která může být jedna z hello největší zdroje transakcí pro tuto aplikaci. Vyberte vaše interval dotazování dobře: dotazování příliš často může způsobit vaše aplikace tooapproach cíle škálovatelnosti hello hello fronty. Ale v 200 000 transakce $0,01 (v době psaní hello), jeden procesor dotazování po méně než 15 centů tak náklady a náklady za sekundu dobu jednoho měsíce není obvykle faktor, který má vliv na vaši volbu interval dotazování.  

Náklady na aktuální informace najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
Můžete použít **UpdateMessage** tooincrease hello neviditelnosti vypršení časového limitu nebo tooupdate informace o stavu zprávy. To je výkonný, mějte na paměti, aby se každý **UpdateMessage** operace, se započítává hello škálovatelnost cíl. To však může být mnohem efektivnější přístup, než má pracovní postup, který předává z se vytvoří jedna fronta toohello úlohu v dalším kroku po dokončení každého kroku hello úlohy. Pomocí hello **UpdateMessage** operace umožňuje vaší aplikace toosave hello úlohy stavu toohello zpráva a poté pokračovat v práci, namísto znovu služby Řízení front zpráv hello hello další krok hello úlohy pokaždé, když se krok je dokončen.  

Další informace najdete v článku hello [postupy: Změna obsahu zpráv zařazených ve frontě hello](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Architektura aplikace
Měli byste použít fronty toomake škálovatelná architektura vaší aplikace. Hello následující seznam obsahuje některé možnosti použití fronty toomake větší škálovatelnost aplikace:  

* Můžete použít ke zpracování nevyřízené položky toocreate fronty pracovní a vyhlazení úlohy v aplikaci. Například může fronty požadavků ze uživatelé tooperform procesoru náročné práce jako je například změna velikosti obrázků nahrané.
* Fronty toodecouple částí aplikace můžete použít tak, aby je možné škálovat nezávisle. Například by mohlo způsobit webového front-endu výsledky zjišťování od uživatelů do fronty pro pozdější analýzu a úložiště. Můžete přidat, že další tooprocess instancí role pracovního procesu hello fronty data podle potřeby.  

## <a name="conclusion"></a>Závěr
Tento článek některé z nejběžnějších, hello popsané, osvědčené postupy pro optimalizaci výkonu při použití služby Azure Storage. Jsme podporují každých tooassess vývojáře aplikace jejich aplikace proti jednotlivým hello výše postupy a zvažte funguje na vysoký výkon tooget hello doporučení pro své aplikace, které používají úložiště Azure.
