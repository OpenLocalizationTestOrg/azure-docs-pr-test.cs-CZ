---
title: "Poznámky k verzi aaaMicrosoft Azure Storage Explorer (Preview) | Microsoft Docs"
description: "Poznámky k verzi pro Microsoft Azure Storage Explorer (Preview)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 44ff6dc8e2015f4eb71fa13098b832fbbf48ccac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a>Poznámky k verzi Microsoft Azure Storage Explorer (Preview)

Tento článek obsahuje hello verze, kterou verzi poznámky pro Azure Storage Explorer 0.8.16 (Preview), a také poznámky k verzi pro předchozí verze.

[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.

## <a name="version-0816-preview"></a>Verze 0.8.16 (Preview)
8/21/2017

### <a name="download-azure-storage-explorer-0816-preview"></a>Stažení Azure Storage Explorer 0.8.16 (Preview)
- [Azure Storage Explorer (Preview) 0.8.16 pro Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage Explorer (Preview) 0.8.16 pro Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage Explorer (Preview) 0.8.16 pro Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Nový
* Při otevření objektu blob Storage Explorer zobrazí výzvu tooupload hello stáhnout soubor Pokud je zjištěna změna
* Rozšířené zásobník Azure přihlašování uživatelů
* Vylepšené hello výkonu nahrávání nebo stahování mnoho malých souborů na hello stejný čas


### <a name="fixes"></a>Opravy
* Pro některé typy objektů blob volba příliš "replace" při nahrávání došlo ke konfliktu někdy výsledkem by hello nahrávání Probíhá restartování. 
* Ve verzi 0.8.15 by někdy nahrávání stalace v 99 %.
* Při nahrávání souborů tooa sdílené složky, pokud jste zvolili tooupload tooa directory, která ještě neexistuje, dojde k selhání odeslání.
* Storage Explorer byla nesprávně generování časová razítka pro sdílené přístupové podpisy a tabulku.


Známé problémy
* Pomocí názvu a klíče připojovací řetězec momentálně nefunguje. Se problém bude vyřešený v příštím vydání hello. Do té doby můžete připojit pomocí názvu a klíče.
* Pokud se pokusíte tooopen soubor s neplatným názvem souboru systému Windows, způsobí hello stažení souboru nebyla nalezena chyba.
* Po kliknutí na tlačítko Storno"na úlohu, může trvat nějakou dobu toocancel této úlohy. Jedná se o omezení hello knihovně uzlu úložiště Azure.
* Po dokončení nahrávání objektů blob, se aktualizují hello kartu, která zahájila odesílání hello. Toto je liší od předchozí chování a budou také způsobit toobe přesměrováni zpět hello kontejneru, který se v kořenovém adresáři toohello.
* Pokud se rozhodnete hello nesprávný certifikát PIN kód nebo čipová karta, bude nutné toorestart v pořadí toohave Storage Explorer zapomněli rozhodnutí.
* panel nastavení Hello účtu může zobrazovat, je nutné, aby tooreenter pověření toofilter odběry.
* Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky. Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.
* I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.
* Pro uživatele v Ubuntu 14.04, budete potřebovat tooensure RSZ zapnutý toodate – to můžete provést spuštěním hello následující příkazy a restartujte svůj počítač:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Pro uživatele v Ubuntu č. 17.04 budete potřebovat tooinstall GConf – to můžete provést spuštění hello následující příkazy a restartujte svůj počítač:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a>Verze 0.8.14 (Preview)
06/22/2017

### <a name="download-azure-storage-explorer-0814-preview"></a>Stažení Azure Storage Explorer 0.8.14 (Preview)
* [Stažení Azure Storage Explorer (Preview) 0.8.14 pro Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Stažení Azure Storage Explorer (Preview) 0.8.14 pro Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Stažení Azure Storage Explorer (Preview) 0.8.14 pro Linux](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>Nový

* Aktualizované verze too1.7.2 elektronovým v pořadí tootake výhod několik důležité aktualizace zabezpečení
* Nyní můžete rychle k Průvodci odstraňováním potíží online hello z nabídky Nápověda hello
* Storage Explorer řešení potíží s [Průvodce][2]
* [Pokyny] [ 3] na připojení tooan předplatné Azure zásobníku

### <a name="known-issues"></a>Známé problémy

* Tlačítka v dialogovém okně potvrzení hello odstranit složku nezaregistrujete kliknutími myši hello v systému Linux. Alternativní řešení je toouse hello zadejte klíč
* Pokud se rozhodnete hello nesprávný certifikát PIN kód nebo čipová karta, bude nutné toorestart v pořadí toohave Storage Explorer zapomněli hello rozhodnutí
* S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby
* panel nastavení Hello účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter odběrů
* Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky. Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.
* I když zásobník Azure aktuálně nepodporuje sdílené složky, sdílené složky uzel se objeví stále v připojené účtu úložiště Azure zásobníku. 
* Ubuntu 14.04 instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a>Předchozí verze

* [Verze 0.8.13](#version-0813)
* [Verze 0.8.12 / 0.8.11 / 0.8.10](#version-0812--0811--0810)
* [Verze 0.8.9 nebo 0.8.8](#version-089--088)
* [Verze 0.8.7](#version-087)
* [Verze 0.8.6](#version-086)
* [Verze 0.8.5](#version-085)
* [Verze 0.8.4](#version-084)
* [Verze 0.8.3](#version-083)
* [Verze 0.8.2](#version-082)
* [Verze 0.8.0](#version-080)
* [Verze 0.7.20160509.0](#version-07201605090)
* [Verze 0.7.20160325.0](#version-07201603250)
* [Verze 0.7.20160129.1](#version-07201601291)
* [Verze 0.7.20160105.0](#version-07201601050)
* [Verze 0.7.20151116.0](#version-07201511160)


### <a name="version-0813"></a>Verze 0.8.13
05/12/2017

#### <a name="new"></a>Nový

* Storage Explorer řešení potíží s [Průvodce][2]
* [Pokyny] [ 3] na připojení tooan předplatné Azure zásobníku

#### <a name="fixes"></a>Opravy

* Pevná: Odeslání souboru měl vysokou možnost, že nedostatku paměti
* Opravené: Můžete teď přihlásit pomocí PIN kódu nebo čipová karta
* Pevná: Otevřete portálu teď funguje s Azure China, Azure v Německu, Azure US Government a Azure zásobníku
* Pevná: Při nahrávání kontejneru objektů blob tooa složky, "Neplatnou operaci" by někdy, dojde k chybě
* Pevná: Vybrat vše zakázal při správě snímků
* Opravené: hello metadata objektu blob základní hello může získat přepsána po zobrazením vlastností hello jeho snímků

#### <a name="known-issues"></a>Známé problémy

* Pokud se rozhodnete hello nesprávný certifikát PIN kód nebo čipová karta, bude nutné toorestart v pořadí toohave Storage Explorer zapomněli hello rozhodnutí
* Při možnosti příchozí nebo odchozí, úroveň přiblížení hello na okamžik resetován toohello výchozí úroveň
* S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby
* panel nastavení Hello účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter odběrů
* Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky. Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.
* I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku. 
* Ubuntu 14.04 instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a>Verze 0.8.12 / 0.8.11 / 0.8.10
04/07/2017

#### <a name="new"></a>Nový

* Storage Explorer se teď automaticky zavře po instalaci aktualizace z upozornění na aktualizace hello
* Rychlý přístup na místě poskytuje lepší prostředí pro práci se často používané prostředky
* V editoru hello kontejner objektů Blob nyní můžete vidět které virtuálního počítače patří zapůjčených objektů blob
* Nyní lze sbalit panel levé straně hello
* Zjišťování nyní běží v hello stejný čas jako soubor ke stažení
* Statistik použití v kontejneru objektů Blob, sdílené složky a tabulku editory toosee hello velikost prostředku nebo výběr hello
* Můžete teď přihlášení tooAzure Active Directory (AAD) na základě účtů Azure zásobníku. 
* Můžete teď archivu nahrávání souborů víc než 32 MB tooPremium úložiště účtů
* Vylepšená podpora usnadnění přístupu
* Nyní můžete přidat důvěryhodný Base-64 kódovaný certifikáty X.509 SSL pomocí budete tooEdit -&gt; certifikáty SSL -&gt; Import certifikátů

#### <a name="fixes"></a>Opravy

* Opravené: po aktualizovat přihlašovací údaje účtu, hello stromové zobrazení by někdy aktualizace automaticky
* Pevná: generování SAS pro emulátor fronty a tabulky by způsobilo neplatná adresa URL
* Opravené: prémiové účty úložiště lze nyní rozbalit proxy server je povolen program
* Pevná: hello použít tlačítko na hello účty, které stránky správy nebude fungovat, pokud jste měli 1 nebo 0 účty vybrané
* Pevné: odesílání objekty BLOB, které vyžadují řešení konfliktu může selhat - fixed v 0.8.11 
* Pevná: odeslání názoru byla porušena v 0.8.11 - fixed v 0.8.12 

#### <a name="known-issues"></a>Známé problémy

* Po upgradu too0.8.10, budete potřebovat toorefresh všechny svoje přihlašovací údaje.
* Při možnosti příchozí nebo odchozí, úroveň přiblížení hello resetován na okamžik toohello výchozí úroveň.
* Více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby.
* panel nastavení Hello účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter předplatných.
* Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky. Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.
* I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku. 
* Ubuntu 14.04 instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a>Verze 0.8.9 nebo 0.8.8
02/23/2017

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a>Nový

* Storage Explorer 0.8.9 automaticky stáhne nejnovější verzi hello aktualizací.
* Opravy hotfix: portálu vygeneruje tooattach identifikátor URI SAS, které účet úložiště by mělo za následek chybu.
* Teď můžete vytvářet, spravovat a zvýšit úroveň snímky objektů blob.
* Teď může přihlásit tooAzure účty Čína, Azure v Německu a Azure US Government.
* Nyní můžete měnit úroveň přiblížení hello. Pomocí možností hello v hello zobrazení nabídky tooZoom v oddálit a resetovat přiblížení.
* Uživatel metadat pro objekty BLOB a soubory jsou nyní podporovány znaky znakové sady Unicode.
* Vylepšení přístupnosti.
* Poznámky k verzi Hello další verzi si můžete prohlížet hello aktualizace oznámení. Můžete také zobrazit hello aktuální poznámky k verzi z nabídky Nápověda hello.

#### <a name="fixes"></a>Opravy

* Opravené: číslo verze hello je nyní správně zobrazen v Ovládacích panelech v systému Windows
* Pevná: vyhledávání už není omezené too50 000 uzly
* Opravené: nahrávání tooa sdílené složky spuštěné navždy hello cílový adresář již neexistovala
* Opravené: vylepšení stability dlouho nahrávání a stahování

#### <a name="known-issues"></a>Známé problémy

* Při možnosti příchozí nebo odchozí, úroveň přiblížení hello resetován na okamžik toohello výchozí úroveň.
* Rychlý přístup pracuje pouze s odběru na základě položky. V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS.
* Rychlý přístup, může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků, které máte.
* Více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby.

12/16/2016
### <a name="version-087"></a>Verze 0.8.7

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Nový

* Můžete určit, jak tooresolve konflikty od začátku hello aktualizace, stáhnout nebo zkopírujte relace v okně aktivity hello
* Najeďte myší na kartě toosee hello úplnou cestu prostředku úložiště hello
* Když kliknete na kartě, synchronizuje s jeho umístění v hello levé navigační podokno

#### <a name="fixes"></a>Opravy

* Opravené: Storage Explorer je nyní důvěryhodné aplikace v systému Mac
* Opravené: Ubuntu 14.04 se znovu podporuje
* Opravené: Někdy hello přidat účet, který bliká uživatelského rozhraní při načítání odběrů
* Opravené: Někdy ne všechny prostředky úložiště byly uvedené v hello levé navigační podokno
* Opravené: podokna akcí hello někdy zobrazí prázdné akce
* Opravené: velikost okna hello z hello posledním zavření relace je nyní uchována
* Pevná: Můžete otevřít několik karet pro hello stejného zdroje pomocí hello kontextové nabídky

#### <a name="known-issues"></a>Známé problémy

* Rychlý přístup pracuje pouze s odběru na základě položky. V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS
* Rychlý přístup může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků máte
* S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby
* Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkonu může být ovlivněno nebo může způsobit, že neošetřené výjimky
* Pro hello první přihlášení pomocí hello Průzkumníka úložiště v systému macOS, může se zobrazit více výzev žádostí o řetězce klíčů tooaccess oprávnění uživatele. Doporučujeme že vybrat vždy povolit tak nebude znovu objeví hello řádku

11/18/2016
### <a name="version-086"></a>Verze 0.8.6

#### <a name="new"></a>Nový

* Můžete teď pin nejčastěji používá služby toohello rychlý přístup snadno navigace
* Nyní lze otevřít více editory v různých kartách. Jedním kliknutím tooopen dočasné karta; dvakrát klikněte na tlačítko tooopen trvalé kartě. Můžete také kliknutím na hello dočasné karta toomake ho trvalé karta
* Jsme provedli znatelné výkonu a zlepšení stability pro odesílání a stahování souborů, zejména u velkých souborů na rychlé počítače
* Prázdné složky "virtuální" teď lze vytvořit v kontejnerech objektů blob
* Mít znovu zavedli jsme vymezené vyhledávání s naší nové rozšířené dílčí řetězec hledání, takže Teď máte dvě možnosti pro vyhledávání: 
    * Globální vyhledávání – zadejte do textového pole hledání hello jenom hledaný termín
    * Vymezené vyhledávání – klikněte na hello lupy ikonu další tooa uzel, pak přidat konec hledání termín toohello hello cesty, nebo klikněte pravým tlačítkem a vyberte "Vyhledávání z zde"
* Jsme přidali různé motivy: Light (výchozí), světlý, vysoce kontrastní černá a Vysoký kontrast prázdné. Přejděte tooEdit -&gt; toochange motivy zúčastníte motivů
* Můžete upravit vlastnosti objektů Blob a souborů
* Teď podporujeme kódovaného (base64) a nekódovaného fronty zpráv
* V systému Linux, OS 64-bit je nyní vyžaduje. Pro tuto verzi podporujeme pouze 64bitové verze Ubuntu 16.04.1 LTS
* Aktualizovali jsme naši logo!

#### <a name="fixes"></a>Opravy

* Pevná: Obrazovky zmrazení problémy
* Opravené: Rozšířené zabezpečení
* Pevná: Zobrazovat může někdy duplicitní připojené účty
* Opravené: Objekt blob se typ obsahu definovaný může generovat výjimky
* Opravené: Otevírání hello panely dotaz na prázdná tabulka nebylo možné
* Opravené: Se liší chyby ve vyhledávání
* Opravené: Vyšší hello počet prostředků, které jsou načteny z 50 too100 při kliknutí na "Více zatížení"
* Pevná: Při prvním spuštění, pokud je účet přihlášeni, jsme teď vybrat všechna předplatná pro tento účet ve výchozím nastavení 

#### <a name="known-issues"></a>Známé problémy

* Tato verze hello Storage Explorer nefunguje na Ubuntu 14.04
* tooopen několik karet pro stejný prostředek, není nepřetržitě proveďte kliknutím na hello hello stejného zdroje. Klikněte na jiný prostředek a potom přejděte zpět a potom klikněte na původní tooopen prostředků hello ho znovu na jiné kartě 
* Rychlý přístup pracuje pouze s odběru na základě položky. V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS
* Rychlý přístup může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků máte
* S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby
* Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkonu může být ovlivněno nebo může způsobit, že neošetřené výjimky

10/03/2016
### <a name="version-085"></a>Verze 0.8.5

#### <a name="new"></a>Nový

* Můžete teď pomocí portálu vygeneruje SAS klíče tooattach tooStorage účtů a prostředků

#### <a name="fixes"></a>Opravy

* Pevná: časování při hledání někdy způsobila uzly toobecome – rozšíření
* Opravené: "Použít protokol HTTP" nefunguje, pokud připojení tooStorage účtů s názvem účtu a klíč
* Opravené: Klíče SAS (ty speciálně generované portálu) vrátí chybu "koncové lomítko."
* Opravené: import tabulky problémy
    * Někdy měla vrátit klíč oddílu a klíč řádku
    * Nelze tooread "null" klíče oddílů

#### <a name="known-issues"></a>Známé problémy

* Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté může mít dopad na výkon
* Azure zásobník nepodporuje aktuálně soubory, tak při tooexpand soubory se zobrazí chyba

09/12/2016
### <a name="version-084"></a>Verze 0.8.4

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Nový

* Generovat přímé odkazy toostorage účty, kontejnery, front, tabulky, nebo sdílené složky pro sdílení a snadný přístup k prostředkům tooyour - Windows a podporu systému Mac OS
* Hledat kontejnery objektů blob, tabulek, front, sdílené soubory nebo účty úložiště ze hello vyhledávacího pole
* Nyní můžete seskupit klauzule v Tvůrce dotazů tabulky hello
* Přejmenujte a zkopírujte a vložte kontejnery objektů blob, sdílené složky, tabulky, objekty BLOB, objektů blob složek, souborů a adresářů z v rámci SAS připojené účty a kontejnery
* Přejmenování a kopírování objektu blob kontejnery a sdílené složky teď zachovat vlastnosti a metadata

#### <a name="fixes"></a>Opravy

* Opravené: nelze upravit entity tabulky, pokud obsahují logická hodnota nebo binární vlastnosti

#### <a name="known-issues"></a>Známé problémy

* Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté může mít dopad na výkon

08/03/2016
### <a name="version-083"></a>Verze 0.8.3

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Nový

* Přejmenujte kontejnery, tabulky, sdílené složky
* Vylepšené prostředí Tvůrce dotazů
* Možnost toosave a zatížení dotazy
* Přímé odkazy toostorage účty nebo kontejnery, fronty, tabulky nebo sdílené složky pro sdílení a snadno přístup k prostředkům (pouze pro systém Windows - systému macOS podporu brzo!)
* Možnost toomanage a nakonfigurovat pravidla CORS

#### <a name="fixes"></a>Opravy

* Opravené: Accounts Microsoft vyžadovat opakované ověření každých 8 – 12 hodin

#### <a name="known-issues"></a>Známé problémy

* Někdy hello uživatelského rozhraní se může objevit ukotvené – tím se maximalizuje okno hello pomůže tento problém vyřešit
* instalace systému macOS může vyžadovat zvýšenými oprávněními
* Panel nastavení účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter odběrů
* Přejmenování sdílené složky, kontejnery objektů blob a tabulek nezachovává metadata nebo dalších vlastností hello kontejneru, například kvóta pro sdílenou složku, úroveň veřejného přístupu nebo zásady přístupu
* Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky. Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entit
* Kopírování nebo přejmenování prostředky nefunguje v rámci SAS připojené účty

07/07/2016
### <a name="version-082"></a>Verze 0.8.2

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Nový

* Účty úložiště jsou seskupené podle odběry; vývoj pro úložiště a prostředky, které jsou připojené prostřednictvím klíč nebo SAS se zobrazují v uzlu (místní a připojené)
* Odhlaste se z účtů v panelech "Nastavení účtu Azure"
* Konfigurace tooenable proxy nastavení a správa přihlášení
* Vytvoření a rozdělit zapůjčení objektů blob
* Kontejnery otevřete objektů blob, fronty, tabulky a soubory s jedním kliknutím

#### <a name="fixes"></a>Opravy

* Pevná: Vložit s knihovnami .NET nebo Java zprávy fronty nejsou dekódovat správně z formátu base64.
* Opravené: $metrics tabulky nejsou zobrazeny pro účty úložiště objektů Blob
* Opravené: uzel tabulky nefunguje pro místní úložiště (vývoj)

#### <a name="known-issues"></a>Známé problémy

* instalace systému macOS může vyžadovat zvýšenými oprávněními

06/15/2016
### <a name="version-080"></a>Verze 0.8.0

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Nový

* Podpora sdílení souborů: zobrazení, odesílání, stahování, kopírování souborů a adresářů, identifikátory URI SAS (vytvořit a připojit)
* Vylepšené uživatelské prostředí pro připojení tooStorage s identifikátory URI SAS nebo klíče účtu
* Export výsledků dotazu tabulky
* Změny pořadí sloupců tabulky a přizpůsobení
* Zobrazení $logs kontejnery objektů blob a tabulek $metrics pro účty úložiště pomocí povoleno metriky
* Vylepšené export a import chování, teď obsahuje typ hodnoty vlastnosti

#### <a name="fixes"></a>Opravy

* Opravené: nahrávání nebo stahování velké objekty BLOB může způsobit neúplné nahrávání nebo stahování
* Opravené: úpravy, přidáním nebo importováním entitu s hodnotou číselných řetězců ("1") bude převedena toodouble
* Opravené: Uzel nelze tooexpand hello tabulky v hello místní vývojové prostředí

#### <a name="known-issues"></a>Známé problémy

* $metrics tabulky nejsou viditelné pro účty úložiště objektů Blob
* Zprávy fronty přidány prostřednictvím kódu programu, se nemusí zobrazit správně Pokud hello zprávy jsou zakódován pomocí kódování Base64

05/17/2016
### <a name="version-07201605090"></a>Verze 0.7.20160509.0

#### <a name="new"></a>Nový

* Lepší zpracování chyb pro aplikace, dojde k chybě

#### <a name="fixes"></a>Opravy

* Kde informační panel zpráv někdy nezobrazovat až při přihlašovací údaje nebyly potřeba opravené chyby

#### <a name="known-issues"></a>Známé problémy

* Tabulky: Přidávání, úpravám nebo import entita, která má vlastnost s ambiguously číselnou hodnotu, jako je například "1" nebo "1.0", a hello toosend pokusů uživatele jej jako `Edm.String`, hello hodnotu, vrátí se znovu prostřednictvím klienta hello rozhraní API jako Edm.Double

03/31/2016

### <a name="version-07201603250"></a>Verze 0.7.20160325.0

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a>Nový

* Tabulka podporu: zobrazení, dotazování, export, jejich import a operace CRUD pro entity
* Podpora ve frontě: zobrazení, přidání, dequeueing zprávy
* Generování identifikátory URI SAS pro účty úložiště
* Připojení tooStorage účtů s identifikátory URI SAS
* Oznámení o aktualizacích pro budoucí aktualizace tooStorage Explorer
* Aktualizované vzhled a chování

#### <a name="fixes"></a>Opravy

* Vylepšení výkonu a spolehlivosti

### <a name="known-issues-amp-mitigations"></a>Známé problémy &amp; způsoby zmírnění rizik

* Stahování souborů velkého objektu blob nebude správně fungovat – doporučujeme použít AzCopy, když jsme tento problém vyřešit 
* Přihlašovací údaje účtu nebude načíst ani v mezipaměti, pokud hello domovskou složku nelze nalézt nebo nelze zapisovat
* Pokud jsme jsou přidání, úpravy nebo import entita, která má vlastnost s ambiguously číselnou hodnotu, jako je například "1" nebo "1.0", a uživatel hello pokusí toosend jej jako `Edm.String`, hello hodnotu, vrátí se znovu prostřednictvím klienta hello rozhraní API jako Edm.Double
* Při importu souborů CSV pomocí víceřádkovým záznamy, může získat hello data chopped nebo kódována

02/03/2016

### <a name="version-07201601291"></a>Verze 0.7.20160129.1

#### <a name="fixes"></a>Opravy

* Lepší celkový výkon při odesílání, stahování a kopírování objektů BLOB

01/14/2016

### <a name="version-07201601050"></a>Verze 0.7.20160105.0

#### <a name="new"></a>Nový

* Podpora Linuxu (parita funkce tooOSX)
* Přidat kontejnery objektů blob s podpisy sdíleného přístupu (SAS) klíčem.
* Přidat účty úložiště pro Azure Čína
* Přidat účty úložiště s vlastní koncové body
* Otevírat a zobrazovat hello obsah textu a obrázků objektů BLOB
* Zobrazit a upravit vlastnosti objektů blob a metadat

#### <a name="fixes"></a>Opravy

* Pevné: odeslání nebo stažení velkého počtu objektů BLOB (500 +) může v některých případech způsobit toohave aplikace hello bílé obrazovky 
* Opravené: při nastavení úrovně veřejný přístup kontejner objektů blob, nová hodnota hello se neaktualizuje dokud znovu nenastavíte hello zaměřením na kontejneru hello. Navíc hello dialogu vždy použije jako výchozí příliš "žádné veřejný přístup" a ne hello skutečná aktuální hodnota.
* Podpora lepší celkový klávesnice nebo usnadnění a uživatelského rozhraní
* Cesta ke stránce Historie text zalomen během procesu je dlouhá s bílými oblasti
* Dialogové okno SAS podporuje ověřování vstupu
* Místní úložiště pokračovat toobe k dispozici i v případě, že vypršela platnost přihlašovací údaje uživatele
* Při odstranění kontejner otevřen objekt blob je uzavřený hello Průzkumník objektů blob na pravé straně hello

#### <a name="known-issues"></a>Známé problémy

* Linux instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující: 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Verze 0.7.20151116.0

#### <a name="new"></a>Nový

* systému macOS a verze systému Windows
* Přihlaste se tooview účtů úložiště – pomocí účtu organizace, Microsoft Account, 2FA atd.
* Vývoj pro místní úložiště (pomocí emulátoru úložiště pouze pro systém Windows)
* Azure Resource Manager a klasický podporu prostředků
* Vytvářet a odstraňovat objekty BLOB, fronty nebo tabulky
* Vyhledejte konkrétní objekty BLOB, fronty nebo tabulky
* Seznamte se s obsahem hello kontejnerů objektů blob
* Zobrazení a procházení adresářů
* Odesílání, stahování a odstraňování objektů BLOB a složky
* Zobrazit a upravit vlastnosti objektů blob a metadat
* Generovat klíče SAS
* Spravovat a vytvořit uložené zásady přístupu (SAP)
* Hledat objekty BLOB podle předpony
* Přetáhněte severní šířky vyřaďte tooupload soubory nebo stáhnout

#### <a name="known-issues"></a>Známé problémy

* Při nastavení úrovně veřejný přístup kontejner objektů blob, nová hodnota hello není aktualizován, dokud znovu nenastavíte hello zaměřením na kontejneru hello
* Když otevřete hello dialogové okno tooset hello veřejný přístup úroveň, vždy zobrazuje "Žádné veřejný přístup" jako výchozí hello a není hello skutečné aktuální hodnota
* Nelze přejmenovat stažené objektů BLOB
* Položky protokolu aktivity se někdy "zablokuje" v v průběhu stavu, když dojde k chybě, a není zobrazí chyba hello
* V některých případech havárie nebo oplátku zcela bílé při pokusu o tooupload nebo stáhnout velkého počtu objektů BLOB
* Někdy zrušení operace kopírování nefunguje
* Při vytvoření kontejneru (objektů blob nebo fronty nebo tabulky), pokud vstupní neplatný název a pokračovat toocreate jiné v rámci různých kontejneru typu nelze nastavit fokus na nový typ hello
* Nelze vytvořit novou složku nebo přejmenujte složku




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md