---
title: "Průvodce aaaBackup SAP HANA ve virtuálních počítačích Azure | Microsoft Docs"
description: "Zálohování Příručka pro SAP HANA obsahuje dvě hlavní možnosti zálohování pro SAP HANA na virtuálních počítačích Azure"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: e651091bb5da2698ec8bf80cad405bfce5b192cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Průvodce zálohováním pro SAP HANA na Azure Virtual Machines

## <a name="getting-started"></a>Začínáme

Hello příručce zálohování pro SAP HANA běžících na virtuálních počítačích Azure se popisují pouze témata specifické pro Azure. Pro obecné SAP HANA související položky zálohování, podívejte se do dokumentace SAP HANA hello (viz _zálohování dokumentace SAP HANA_ dále v tomto článku).

Existují dvě hlavní zálohování možnosti pro SAP HANA na virtuálních počítačích Azure je aktivní Hello tohoto článku:

- Systém souborů zálohování toohello HANA v virtuální počítač Linux Azure (viz [SAP HANA Azure Backup na úrovni souborů](sap-hana-backup-file-level.md))
- Zálohování HANA na základě úložiště snímků pomocí hello úložiště Azure blob snímku funkce ručně nebo služby zálohování Azure (viz [SAP HANA zálohování podle úložiště snímků](sap-hana-backup-storage-snapshots.md))

SAP HANA nabízí zálohu rozhraní API, které umožňuje toointegrate nástroje třetích stran zálohování přímo s SAP HANA. (Který není v rámci hello rámec této příručky.) Neexistuje žádná přímá integrace SAP HANA pomocí služby Azure Backup k dispozici pravé na základě na toto rozhraní API.

SAP HANA oficiálně podporované u virtuálního počítače Azure typu GS5 jako jedna instance s úlohami tooOLAP další omezení (viz [najít platformy IaaS certifikované](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) na webu SAP hello). Tento článek se aktualizuje jako nové nabídky pro SAP HANA v Azure k dispozici.

Je také SAP HANA hybridní řešení k dispozici na Azure, kde SAP HANA běží nevirtuálním na fyzických serverech. Ale tento průvodce zálohování SAP HANA Azure popisuje čistý prostředí Azure, kde SAP HANA běží ve virtuálním počítači Azure, ne SAP HANA systémem &quot;velké instancí.&quot; V tématu [přehled SAP HANA (velké instance) a architektura v Azure](hana-overview-architecture.md) Další informace o tomto řešení zálohování na &quot;velké instancí&quot; podle úložiště snímků.

Obecné informace o produktech SAP podporován na platformě Azure můžete najít v [1928533 Poznámka SAP](https://launchpad.support.sap.com/#/notes/1928533).

Hello následující tři obrázky poskytnutí přehledu o hello SAP HANA možnosti zálohování pomocí nativního možnosti Azure aktuálně a také se zobrazují tři možných scénářů budoucí zálohy. Hello souvisejících článcích [SAP HANA Azure Backup na úrovni souborů](sap-hana-backup-file-level.md) a [SAP HANA zálohování podle úložiště snímků](sap-hana-backup-storage-snapshots.md) popisují tyto možnosti podrobněji, včetně velikosti a výkonové požadavky pro SAP HANA zálohování, které jsou více terabajtů velikost.

![Následující obrázek ukazuje existují dvě možnosti pro ukládání hello aktuální stav virtuálního počítače](media/sap-hana-backup-guide/image001.png)

Následující obrázek ukazuje hello možnost ukládání hello aktuální stav virtuálního počítače, prostřednictvím služby Azure Backup nebo ruční snímek disky virtuálních počítačů. S tímto přístupem, jeden nemá & č. 39; nemá toomanage SAP HANA zálohy. výzvy Hello hello disku snímku scénáře je konzistence systému souborů a k dispozici stav disku konzistentní s aplikací. téma konzistence Hello je popsané v části hello _konzistenci dat SAP HANA při pořizování snímků úložiště_ dále v tomto článku. Možnosti a omezení služby Azure Backup související s tooSAP HANA zálohy jsou popsány i později v tomto článku.

![Následující obrázek ukazuje, že zálohování uvnitř hello virtuálních počítačů souborů možnosti za vyjádření SAP HANA](media/sap-hana-backup-guide/image002.png)

Následující obrázek ukazuje možnosti trvá zálohu souboru SAP HANA uvnitř hello virtuálních počítačů a potom ukládání záložních souborů HANA někde jinde pomocí různých nástrojů. Zálohování HANA vyžaduje více času, než řešení zálohování založené na snímku, ale má výhody týkající se integrity a konzistence. Další podrobnosti jsou uvedeny dále v tomto článku.

![Tento obrázek znázorňuje potenciální budoucí zálohy scénář SAP HANA](media/sap-hana-backup-guide/image003.png)

Tento obrázek znázorňuje potenciální budoucí zálohy scénář SAP HANA. Pokud SAP HANA povolený umožňoval vytvářet zálohy z sekundární replikace, by přidat další možnosti pro strategii zálohování. Aktuálně není možné podle tooa post v hello SAP HANA Wiki:

_&quot;Je možné tootake zálohování na sekundární straně hello?_

_Ne, teď můžete pouze trvat dat a protokolu zálohování na primární straně hello. Pokud je povoleno automatické protokolu zálohování, po převzetí toohello sekundární straně zálohy protokolu hello automaticky se zapíšou existuje.&quot;_

## <a name="sap-resources-for-hana-backup"></a>Prostředky SAP HANA zálohování

### <a name="sap-hana-backup-documentation"></a>Zálohování dokumentace SAP HANA

- [Úvod tooSAP HANA správy](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Plánování zálohování a obnovení strategie](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [Plánování zálohování HANA pomocí ABAP DBACOCKPIT](http://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Plán zálohování dat (SAP HANA řídícího panelu)](http://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- Nejčastější dotazy o SAP HANA zálohování v [1642148 Poznámka SAP](https://launchpad.support.sap.com/#/notes/1642148)
- Nejčastější dotazy o SAP HANA snímky databáze a úložiště v [2039883 Poznámka SAP](https://launchpad.support.sap.com/#/notes/2039883)
- Systémy souborů síť není vhodný pro zálohování a obnovení v [1820529 Poznámka SAP](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Proč SAP HANA zálohování?

Azure storage nabízí dostupnost a spolehlivost předinstalované hello (viz [tooMicrosoft Úvod Azure Storage](../../../storage/common/storage-introduction.md) Další informace o úložišti Azure).

Hello minimální pro &quot;zálohování&quot; je toorely na hello Azure SLA, zachování hello SAP HANA data a soubory protokolu na virtuální pevné disky Azure připojené toohello SAP HANA serveru virtuálního počítače. Tento postup popisuje selhání virtuálních počítačů, ale není možné škody toohello SAP HANA data a soubory protokolu nebo logické chyby jako odstraněním dat nebo soubory omylem odstraněný. Zálohy jsou rovněž vyžadovány pro dodržování předpisů nebo právních důvodů. Stručně řečeno je vždy nutné pro SAP HANA zálohy.

### <a name="how-tooverify-correctness-of-sap-hana-backup"></a>Jak tooverify správnost zálohování SAP HANA
Při použití úložiště snímků, se doporučuje spustit test obnovení v jiném systému. Tento přístup poskytuje způsob, jak tooensure, že je záloha správná a interní procesy pro zálohování a obnovení pracovní podle očekávání. Přestože se významným vlivem mezní místně, je mnohem snazší tooaccomplish v cloudu hello dočasně poskytnutím potřebné prostředky pro tento účel.

Zachovat na paměti, díky jednoduché obnovení a kontrole, jestli je HANA a spouštění není dostatečná. V ideálním případě jeden měly být spuštěny se, že je v pořádku, že hello obnovit databáze tabulky toobe kontrolu konzistence. SAP HANA nabízí několik druhů kontroly konzistence, které jsou popsané v [1977584 Poznámka SAP](https://launchpad.support.sap.com/#/notes/1977584).

Informace o kontrolu konzistence hello tabulku je možné také najít na webu SAP hello [tabulky a kontrol konzistence katalogu](http://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

Obnovení testu pro standardních souborových záloh, není nutné. Existují dvě SAP HANA nástroje, které pomáhají toocheck zálohování, které lze použít pro obnovení: hdbbackupdiag a hdbbackupcheck. V tématu [Ruční kontrola zda obnovení je možné](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) Další informace o těchto nástrojů.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Výhody a nevýhody HANA zálohování versus úložiště snímků

SAP nemá & č. 39; t udělení předvoleb tooeither HANA zálohování versus úložiště snímků. Vypíše jejich výhody a nevýhody, takže jeden můžete určit, které toouse v závislosti na situaci hello a technologií úložiště k dispozici (viz [plánování vaše zálohování a obnovení strategie](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

V Azure, mějte na paměti hello to, že hello objektů blob v Azure snímku funkce nemá & č. 39; t zaručit konzistence systému souborů (viz [snímky použití objektů blob v prostředí PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). Hello další části, _konzistenci dat SAP HANA při pořizování snímků úložiště_, popisuje některé aspekty týkající se této funkce.

Kromě toho jeden má toounderstand hello fakturace důsledky při práci často s snímky objektů blob, jak je popsáno v tomto článku: [pochopení jak snímky nabíhat poplatky](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)– ho neběží & č. 39; t jako zřejmé jako pomocí Azure virtuální disky.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>SAP HANA konzistenci dat, když se vezme úložiště snímků

Systém a aplikace konzistence souborů je komplexní problém při pořizování snímků úložiště. Hello nejjednodušší způsob, jak tooavoid problémy by být tooshut dolů SAP HANA nebo možná dokonce hello celý virtuální počítač. Vypnutí může být doable s ukázku nebo prototypu nebo i vývojového systému, ale není možnost pro produkční systému.

V Azure, a jeden má tookeep si, že hello objektů blob v Azure snímku funkce nemá & č. 39; t zaručit konzistence systému souborů. Funguje bez problémů ale pomocí hello SAP HANA snímku funkce, dokud související se situací je jenom jeden virtuální disk. Ale i s jeden disk, další položky mají toobe zaškrtnutí. [SAP Poznámka 2039883](https://launchpad.support.sap.com/#/notes/2039883) obsahuje důležité informace o zálohování SAP HANA pomocí snímků úložiště. Například uvádí, že pomocí systému souborů XFS hello, je nutné toorun **xfs\_freeze** před zahájením úložiště snímku tooguarantee konzistence (najdete v části [xfs\_freeze(8) - Linux man stránka](https://linux.die.net/man/8/xfs_freeze) podrobnosti o **xfs\_freeze**).

Hello tématem konzistence se změní i další náročné, v případě, kde jeden soubor systému zahrnuje několik disky nebo svazky. Například s využitím mdadm nebo LVM a prokládání. Hello Poznámka SAP zmíněné stavy:

_&quot;Ale mějte na paměti, že systém úložiště hello má konzistence tooguarantee vstupně-výstupních operací při vytváření snímku úložiště za SAP HANA datový svazek, tj. snímkování svazku SAP HANA specifických dat služby musí být atomické operace.&quot;_

Za předpokladu, že je systém souborů XFS pokrývání uzlů čtyři Azure virtuální disky, hello následující kroky obsahují konzistentní snímek, který představuje hello HANA datové oblasti:

- Příprava HANA snímku
- Freeze – hello systému souborů (například použít **xfs\_freeze**)
- Vytvářet všechny snímky nezbytné objektů blob v Azure
- Uvolní hello systému souborů
- Potvrďte hello HANA snímku

Doporučuje se toouse hello uvedený postup ve všech případech toobe na straně bezpečné hello, bez ohledu na to, které systém souborů. Nebo pokud se jedná o jediný disk nebo prokládání, prostřednictvím mdadm nebo LVM na více disků.

Je důležité tooconfirm hello HANA snímku. Kvůli toohello &quot;kopie při zápisu,&quot; SAP HANA nemusí vyžadovat další místo na disku v to snímku Příprava režimu. Vyb & č. 39; s také není možné toostart nových záloh dokud hello SAP HANA snímku je potvrzen.

Služba Azure Backup používá tootake rozšíření virtuálního počítače Azure péče o hello konzistence systému souborů. Tato rozšíření virtuálního počítače nejsou k dispozici pro samostatné použití. Jeden má stále toomanage SAP HANA konzistence. Najdete v souvisejícím článku hello [SAP HANA Azure Backup na úrovni souborů](sap-hana-backup-file-level.md) Další informace.

### <a name="sap-hana-backup-scheduling-strategy"></a>Plánování strategie SAP HANA zálohování

článek SAP HANA Hello [plánování vaše zálohování a obnovení strategie](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) stavy a basic plánování toodo zálohování:

- Úložiště snímků (denní)
- Zálohování kompletní datový soubor nebo bacint formátu (jednou týdně)
- Automatické protokolu zálohování

Volitelně můžete jeden může přejít zcela bez snímků úložiště; může být nahrazen HANA rozdílové zálohy, jako přírůstkové nebo rozdílové zálohy (viz [rozdílové zálohy](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

Příručka pro správu HANA Hello obsahuje seznam příklad. Navrhování jeden obnovit SAP HANA tooa určitého bodu v čase pomocí hello následující pořadí zálohování:

1. Úplná záloha
2. Rozdílovou zálohu
3. Přírůstkové zálohování 1
4. Přírůstkové zálohování 2
5. Zálohy protokolů

O přesný plánu jako toowhen a jak často má proběhnout konkrétní typ zálohy, není možné toogive obecných pokynů – je příliš specifické pro zákazníka a závisí na tom, kolik dat změnách v systému hello. Jedním z základní doporučení ze strany SAP, které můžete zobrazit jako obecné pokyny, je toomake jeden úplné HANA zálohování jednou za týden.
Týká se zálohy protokolu, naleznete v dokumentaci k SAP HANA hello [zálohy protokolu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

SAP taky doporučuje provádění některých housekeeping tookeep zálohování katalogu hello z narůstají nekonečnou (najdete v části [spouštění pro katalog zálohování a zálohování úložiště](http://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>Konfigurační soubory SAP HANA

Jak je uvedeno v hello – nejčastější dotazy v [1642148 Poznámka SAP](https://launchpad.support.sap.com/#/notes/1642148), hello SAP HANA konfigurační soubory nejsou součástí standardní HANA zálohování. Nejsou nezbytné toorestore systému. Hello HANA configuration může po obnovení hello ručně změnit. V případě, že jeden byste chtěli tooget hello stejné vlastní konfigurace během procesu obnovení hello, je nutné tooback až hello HANA konfigurační soubory samostatně.

Pokud se standardní HANA zálohy se tooa vyhrazené HANA záložní soubor systém, jeden může také zkopírovat hello konfigurační soubory toohello stejné zálohování systému souborů a poté zkopírujte všechno jako cíl konečné úložiště společně toohello cool úložiště objektů blob.

### <a name="sap-hana-cockpit"></a>Řídící panel SAP HANA

Řídící panel SAP HANA nabízí možnost hello, monitorování a správa SAP HANA prostřednictvím prohlížeče. Také umožňuje zpracování SAP HANA záloh a proto je lze použít jako alternativní tooSAP HANA Studio a ABAP DBACOCKPIT (viz [řídící panel SAP HANA](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) Další informace).

![Následující obrázek ukazuje hello obrazovce pro správu databáze SAP HANA řídící panel](media/sap-hana-backup-guide/image004.png)

Tento obrázek ukazuje hello obrazovce pro správu databáze SAP HANA řídicí panel a zálohování dlaždice hello na zbývajících hello. Vidět hello zálohování dlaždice vyžaduje příslušných uživatelských oprávnění pro účet pro přihlášení.

![Zálohy lze sledovat v řídícím SAP HANA době, kdy jsou probíhající](media/sap-hana-backup-guide/image005.png)

Zálohování se dá sledovat v řídícím SAP HANA jsou probíhající a po jeho dokončení všech hello zálohování podrobnosti jsou k dispozici.

![Příklad použití Firefox na virtuální počítač Azure SLES 12 Gnome plochy](media/sap-hana-backup-guide/image006.png)

Hello předchozích snímcích obrazovky nebyly provedeny z virtuálního počítače Windows Azure. Tato funkce je příklad pomocí prohlížeče Firefox na virtuální počítač Azure SLES 12 Gnome plochy. Zobrazuje hello možnost toodefine SAP HANA zálohování plány v řídícím SAP HANA. Jako jeden, můžete také zjistit, navrhuje datum a čas jako předpona pro záložní soubory hello. V nástroji SAP HANA Studio hello výchozí předpona je &quot;COMPLETE\_DATA\_zálohování&quot; při provádění úplnou zálohu souborů. Doporučuje se použít jedinečnou předponu.

### <a name="sap-hana-backup-encryption"></a>Zálohu šifrovacího SAP HANA

SAP HANA nabízí šifrování dat a protokolu. Pokud nejsou šifrovaná data SAP HANA a protokolu, pak hello záloh nejsou šifrované. Je to toohello zákazníka toouse určitou formu řešení třetí strany tooencrypt hello SAP HANA zálohy. V tématu [dat a šifrování svazku protokolu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) toofind Další informace o šifrování SAP HANA.

V Microsoft Azure může zákazník použít hello tooencrypt funkce šifrování virtuálních počítačů IaaS. Například jeden může použít vlastní datové disky připojené toohello virtuálních počítačů, které jsou používané toostore SAP HANA zálohy, ujistěte se, kopie těchto disků.

Služba Azure Backup dokáže zpracovat šifrované virtuálních počítačů/disků (viz [jak šifrované tooback zálohu a obnovení virtuálních počítačů s Azure Backup](../../../backup/backup-azure-vms-encryption.md)).

Další možností bude toomaintain hello SAP HANA virtuálního počítače a jeho disky bez šifrování a ukládání záložních souborů hello SAP HANA v účtu úložiště, pro které bylo povoleno šifrování (viz [šifrování služby úložiště Azure pro Data v klidovém stavu](../../../storage/common/storage-service-encryption.md)) .

## <a name="test-setup"></a>Nastavení testu

### <a name="test-virtual-machine-on-azure"></a>Testovací virtuální počítač na platformě Azure

Instalace SAP HANA virtuální počítač Azure GS5 byl použit pro hello následující testy zálohování nebo obnovení.

![Následující obrázek ukazuje součástí hello přehled portálu Azure pro hello HANA testovacího virtuálního počítače](media/sap-hana-backup-guide/image007.png)

Následující obrázek ukazuje součástí hello přehled portálu Azure pro hello HANA testovacího virtuálního počítače.

### <a name="test-backup-size"></a>Velikost zálohování testu

![Tento obrázek byl odebrán z hello zálohování konzoly v HANA Studio a zobrazuje hello záložní soubor velikost 229 GB pro hello HANA index server](media/sap-hana-backup-guide/image008.png)

Fiktivní tabulku byla naplněna s tooget dat velikost celková data zálohování dat s více než 200 GB tooderive realistické výkonu. Obrázek Hello pořízení z hello zálohování konzoly v HANA Studio a zobrazí se hello záložní soubor velikost 229 GB pro hello HANA index server. Pro testy hello byl použit hello výchozí zálohování předponu "COMPLETE_DATA_BACKUP" v SAP HANA Studio. V systémech skutečné produkční nesmí být definován užitečnější předpony. Řídící panel SAP HANA navrhuje datum a čas.

### <a name="test-tool-toocopy-files-directly-tooazure-storage"></a>Testovací nástroje toocopy soubory přímo tooAzure úložiště

tootransfer SAP HANA záložní soubory přímo tooAzure úložiště objektů blob nebo sdílené složky Azure, hello blobxfer nástroj se používá, protože umožňuje cíle i ho lze snadno integrovat do skripty pro automatizaci kvůli tooits rozhraní příkazového řádku. Nástroj blobxfer Hello je k dispozici na [Githubu](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Odhad velikosti zálohy testu

Je důležité tooestimate hello zálohování velikost SAP HANA. Tento odhad pomáhá tooimprove výkonu definováním hello záložní soubor maximální velikost pro počet záložních souborů kvůli tooparallelism během kopírování souboru. (Tyto informace jsou vysvětleny dále v tomto článku). Také musíte rozhodnout zda toodo úplné zálohování nebo rozdílové zálohování (přírůstkové nebo rozdílovou).

Naštěstí je jednoduchý příkaz SQL, který odhadne hello velikost hello záložní soubory: **vyberte \* z M\_zálohování\_velikost\_odhady** (viz [ Odhad hello místo potřebné v hello systému souborů pro zálohování dat](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![výstup Hello tento příkaz SQL odpovídá téměř úplně stejné hello skutečná velikost hello úplná data zálohy na disku](media/sap-hana-backup-guide/image009.png)

Výstup hello tento příkaz SQL pro hello testovací systém, odpovídá téměř úplně stejné hello skutečná velikost hello úplná data zálohy na disku.

### <a name="test-hana-backup-file-size"></a>Test HANA záložní soubor velikost

![zálohování konzoly Hello HANA Studio umožňuje jeden toorestrict hello maximální velikost souboru HANA záložní soubory](media/sap-hana-backup-guide/image010.png)

zálohování konzoly Hello HANA Studio umožňuje jeden toorestrict hello maximální velikost souboru HANA záložní soubory. V prostředí ukázka hello této funkce je možné tooget více menší záložní soubory místo jeden záložní soubor 230 GB. Menší velikost souborů má významný dopad na výkon (najdete v souvisejícím článku hello [SAP HANA Azure Backup na úrovni souborů](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Souhrn

Podle výsledků testu hello hello následující tabulky ukazují výhody a nevýhody tooback řešení si databázi SAP HANA běžících na virtuálních počítačích Azure.

**Proveďte zálohu systému souborů toohello SAP HANA a zkopírujte záložní soubory později toohello poslední zálohu**

|Řešení                                           |Odborníci na                                 |Nevýhody                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Zachovat HANA zálohování na disky virtuálních počítačů                      |Žádné další správu úsilí     |Eats místní místa na disku virtuálního počítače           |
|Blobxfer nástroj toocopy záložní soubory tooblob úložiště |Paralelismus toocopy více souborů, volba toouse studený objekt blob úložiště | Další nástroje údržby a vlastní skriptování | 
|Kopírování objektu BLOB prostřednictvím prostředí Powershell nebo rozhraní příkazového řádku                    |Žádné další nástroje, které jsou nezbytné, můžete nakonfigurovat pomocí prostředí Azure Powershell nebo rozhraní příkazového řádku |Proces ručního nastavení, zákazník má tootake péče o skriptování a správu zkopírovaných objektů blob pro obnovení|
|Sdílená složka tooNFS kopie                                  |Po zpracování záložní soubory na jiných virtuálních počítačů bez dopadu na serveru HANA hello|Kopírování pomalé|
|Blobxfer kopie tooAzure služba souborů                |Nebude vyžadovat značné množství místa na místní disky virtuálních počítačů|Žádné přímé zápisu podporu omezením HANA zálohování, velikost sdílené složky aktuálně na 5 TB|
|Azure Backup Agent                                 | Bude upřednostňované řešení         | Momentálně není k dispozici v systému Linux    |



**Zálohování SAP HANA podle úložiště snímků**

|Řešení                                           |Odborníci na                                 |Nevýhody                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Služby zálohování Azure                               | Umožňuje zálohování virtuálních počítačů podle snímky objektů blob | Pokud nepoužíváte obnovení na úrovni souborů, vyžaduje hello vytvoření nového virtuálního počítače pro hello obnovení procesu, který pak znamená hello potřebám nový licenční klíč SAP HANA|
|Snímky ruční objektů blob                              | Flexibilita toocreate a obnovení konkrétní disky virtuálních počítačů beze změny hello jedinečné ID virtuálního počítače|Všechny ručního nastavení, která má toobe provádí hello zákazníka|

## <a name="next-steps"></a>Další kroky
* [SAP HANA Azure Backup na úrovni souborů](sap-hana-backup-file-level.md) popisuje možnost zálohování na základě souborů hello.
* [SAP HANA zálohování podle úložiště snímků](sap-hana-backup-storage-snapshots.md) popisuje hello úložiště založené na snímku možnost zálohování.
* jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA v Azure (velké instance), najdete v části toolearn [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).
