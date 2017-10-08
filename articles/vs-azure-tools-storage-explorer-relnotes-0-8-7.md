---
title: aaaMicrosoft Azure Storage Explorer (Preview) 0.8.7 | Microsoft Docs
description: "Poznámky k verzi pro Microsoft Azure Storage Explorer 0.8.7 (Preview)"
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
ms.date: 01/18/2017
ms.author: cawa
ms.openlocfilehash: 9fdd491a3ea838e20f9d4f82c176cfb02fbe306b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-087-preview"></a>Microsoft Azure Storage Explorer 0.8.7 (Preview)
## <a name="overview"></a>Přehled
Tento článek obsahuje hello poznámky k verzi pro Azure Storage Explorer 0.8.7 verzi preview.

[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.

## <a name="azure-storage-explorer-087-preview"></a>Azure Storage Explorer 0.8.7 (Preview)
### <a name="download-azure-storage-explorer-087-preview"></a>Stažení Azure Storage Explorer 0.8.7 (Preview)
- [Náhled Explorer 0.8.7 úložiště Azure pro Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Náhled Explorer 0.8.7 úložiště Azure pro Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage Explorer 0.8.7 Preview pro Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new-updates"></a>Nové aktualizace
* Můžete vybrat jak tooresolve konflikty od začátku hello aktualizace, stažení nebo zkopírujte relace v hello **aktivity** okno.
* Najeďte myší na kartě toosee hello úplná cesta prostředku úložiště hello.
* Když kliknete na kartě, synchronizuje s jeho umístění v navigačním podokně hello levé straně.

### <a name="fixes"></a>Opravy
* Opravené: Storage Explorer je nyní důvěryhodné aplikace v systému macOS.
* Opravené: Ubuntu 14.04 je znovu podporována.
* Opravené: Někdy hello přidání uživatelského účtu rozhraní bliká při načítání odběry.
* Opravené: Někdy ne všechny prostředky úložiště byly uvedené v levém navigačním podokně hello.
* Opravené: hello akce podokně se zobrazí někdy prázdný akce.
* Opravené: velikost okna hello z hello posledním zavření relace je nyní zachovány.
* Pevná: Můžete otevřít několik karet pro hello stejného zdroje pomocí hello kontextové nabídky.

### <a name="known-issues"></a>Známé problémy
* Rychlý přístup pracuje pouze s předplatným položky. V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS.
* Rychlý přístup, může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků, které máte.
* Více než tří skupin objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby.
* Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkon může být ovlivněno nebo může způsobit neošetřených výjimek.
* Pro hello první přihlášení pomocí hello Průzkumníka úložiště v systému macOS, může se zobrazit více výzev žádostí o uživatelské oprávnění tooaccess hello řetězce klíčů. Doporučujeme vám vybrat **vždy povolit** tak hello řádku není nezobrazovat

## <a name="previous-releases"></a>Předchozí verze
### <a name="features"></a>Funkce
#### <a name="main-features"></a>Hlavní funkce
* systému macOS, Linux a verze systému Windows
* Přihlaste se tooview účtů úložiště seskupené podle předplatného:
    * Pomocí účtu organizace, Microsoft Account, 2FA atd.
    * Konfigurovat a spravovat nastavení proxy serveru
    * Odebrat účty odhlášení
* Připojte se pomocí účtů tooStorage:
    * Název účtu a klíč
    * Vlastní koncové body (včetně Azure China)
    * Identifikátor URI pro SAS pro účty úložiště
* Podpora Azure Resource Manager a Classic úložiště
* Generovat klíče SAS pro objekty BLOB, kontejnery objektů blob, fronty, tabulky nebo sdílené složky
* Připojit tooblob kontejnery, fronty nebo tabulky nebo sdílené složky s klíčem podpisy sdíleného přístupu (SAS)
* Správa uložené zásady přístupu pro kontejnery objektů blob, fronty, tabulky nebo sdílené složky
* Vývoj pro místní úložiště pomocí emulátoru úložiště (jen Windows)
* Vytvářet a odstraňovat kontejnery objektů blob, fronty nebo tabulky
* Zobrazení $logs kontejnery objektů blob a $metrics tabulky
* Vyhledejte konkrétní objekty BLOB, fronty, tabulky nebo sdílené složky
* Přímé odkazy toostorage účty nebo kontejnery, fronty, tabulky nebo soubor složky pro sdílení a snadno přístup k prostředkům
* Přejmenujte kontejnery objektů blob, tabulek, sdílené složky
* Možnost toomanage a nakonfigurovat pravidla CORS
* Snadno zkopírujte primární a sekundární klíč pro účty úložiště
* Algoritmus MD5 kontroly pro nahrávání a stahování pro integritu dat a konzistence
* Hledat kontejnery objektů blob, tabulek, front, sdílené soubory nebo účty úložiště ze hello vyhledávacího pole
* Můžete teď pin nejčastěji používá služby toohello rychlý přístup snadno navigace
* Nyní lze otevřít více editory v různých kartách. Jedním kliknutím tooopen dočasné karta; dvakrát klikněte na tooopen trvalé kartě. Můžete také kliknout na toomake dočasné karta hello je trvalé karta
* Výraznému vylepšení výkonu a stability pro nahrávání a stahování, zejména u velkých souborů na rychlé počítače
* Jsme jsou opětovného zavedení hello rozšířené vymezené vyhledávání a přidané hello koncept rozsahu. Zadejte hello cesta tooa uzlu prostřednictvím hello hover ikonu, klikněte pravým tlačítkem -> "Vyhledávání z sem", nebo ručně tooscope tento uzel. Jakmile obor, můžete přidat hledání termín toohello konec hello cesta toodeep vyhledávání od tohoto uzlu
* Jsme přidali různé motivy: Light (výchozí), světlý, vysoce kontrastní černá a Vysoký kontrast prázdné.
* Přejděte tooEdit -> toochange motivy zúčastníte motivů
* V systému Linux, OS 64-bit je nyní požadované
* Aktualizovali jsme naši logo!
#### <a name="blobs"></a>Objekty blob
* Zobrazit objekty BLOB a procházení adresářů
* Odesílání, stahování, odstranění a zkopírujte objekty BLOB a složky
* Otevírat a zobrazovat hello obsah textu a obrázků objektů BLOB
* Zobrazit a upravit vlastnosti objektů blob a metadat
* Hledat objekty BLOB podle předpony
* Vytvoření a rozdělit zapůjčení pro objekty BLOB a kontejnery objektů blob
* Přetáhněte n vyřadit tooupload soubory
* Přejmenujte objekty BLOB a složek
* Prázdné složky "virtuální" teď lze vytvořit v kontejnerech objektů blob
* Můžete upravit vlastnosti objektů Blob a soubor hello
#### <a name="tables"></a>Tabulky
* Zobrazení a entity ODATA dotazů nebo pomocí dotazu Tvůrce toocreate složitých dotazů
* Přidání, úpravy, odstranění entity
* Import a export obsahu tabulky ve formátu CSV (včetně export výsledků dotazu)
* Přizpůsobení pořadí sloupce
* Možnost toosave dotazy
#### <a name="queues"></a>Fronty
* Přehled o 32 nejnovějších zprávách
* Přidat, vyřazení z fronty, zobrazení zpráv
* Vymazání fronty
* Jsme ještě umožňují vám toodecide jestli se mají tooencode nebo dekódování fronta zprávy
#### <a name="file-shares"></a>Sdílené složky
* Zobrazení souborů a procházení adresářů
* Nahrávání, stahování, odstraňování a kopírování souborů a adresářů
* Zobrazit vlastnosti souboru
* Přejmenování souborů a adresářů

### <a name="bug-fixes"></a>Opravy chyb
* Pevná: Obrazovky zmrazení problémy
* Opravené: Rozšířené zabezpečení

### <a name="known-issues"></a>Známé problémy
* Hledání popisovače hledání mezi uzly přibližně 50 000 - poté výkon může být ovlivněné systému macOS instalace může vyžadovat zvýšenými oprávněními
* Panel nastavení účtu může zobrazovat, je nutné, aby tooreenter pověření toofilter odběrů
* Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky. Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entit
* Azure zásobník nepodporuje aktuálně soubory, tak při tooexpand hello **soubory** uzlu vede k chybě
* Instalace Linux 14.04 musí RSZ verze aktualizovat nebo upgradovat. Hello následující kroky popisují jak tooupgrade:

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

### <a name="previous-versions"></a>Předchozí verze
#### <a name="october-2016-release-version-085"></a>Verze října 2016 (verze 0.8.5)
* [Stažení pro Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Stáhněte si pro Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Stáhněte si pro Linux](https://go.microsoft.com/fwlink/?LinkId=809308)
