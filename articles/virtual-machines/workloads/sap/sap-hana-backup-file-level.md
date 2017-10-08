---
title: "aaaSAP HANA Azure Backup na úrovni souborů | Microsoft Docs"
description: "Existují dvě hlavní zálohování možnosti pro SAP HANA na virtuálních počítačích Azure, tento článek se zabývá SAP HANA Azure Backup na úrovni souborů"
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
ms.openlocfilehash: d5a55de5634ac7724e7fd0fa3760c6c408c3db74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-azure-backup-on-file-level"></a>SAP HANA Azure Backup na úrovni souborů

## <a name="introduction"></a>Úvod

Toto je část třemi částmi řady související články v záloze SAP HANA. [Příručce zálohování pro SAP HANA ve virtuálních počítačích Azure](./sap-hana-backup-guide.md) poskytuje přehled a informace o začátcích, a [SAP HANA zálohování podle úložiště snímků](./sap-hana-backup-storage-snapshots.md) zahrnuje hello možnost zálohování na základě snímku úložiště.

Prohlížení hello velikosti virtuálního počítače Azure, jeden můžete vidět, že GS5 umožňuje 64 disků připojených datových. U systémů SAP HANA může být velký počet disků už používá pro data a soubory protokolu, které by mohly mít v kombinaci s softwaru diskového pole RAID pro optimální disk propustnost vstupně-výstupní operace. potom otázku Hello je, kde toostore SAP HANA zálohovat soubory, které může zaplnit data hello připojené disky v čase? V tématu [velikostí pro virtuální počítače s Linuxem v Azure](../../linux/sizes.md) pro tabulky velikost virtuálního počítače Azure hello.

V tuto chvíli není k dispozici u služby zálohování Azure bez integrace zálohování SAP HANA. standardní způsob Hello toomanage zálohování a obnovení na úrovni souborů hello je s zálohu na základě souborů prostřednictvím SAP HANA Studio nebo pomocí příkazů SAP HANA SQL. V tématu [SAP HANA SQL a Reference zobrazení systému](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) Další informace.

![Na tomto obrázku je dialogové okno hello hello zálohování nabídky položky v SAP HANA Studio](media/sap-hana-backup-file-level/image022.png)

Na tomto obrázku zobrazí dialog hello hello zálohování nabídky položky v sadě SAP HANA Studio. Při výběru typu &quot;souboru&quot; jeden toospecify má cestu v systému souborů hello kde SAP HANA zapíše hello záložní soubory. Obnovení funguje hello stejným způsobem.

Když tato volba vyznívá jednoduché a rovnou předat dál, existují některé aspekty. Jak je uvedeno nahoře, virtuální počítač Azure má omezenou na počet datových disků, které je možné připojit. Nemusí být kapacity toostore SAP HANA záložní soubory v systémech souborů hello hello virtuálních počítačů, v závislosti na velikosti hello hello databázi a disk propustnost požadavků, které mohou zahrnovat softwaru RAID pomocí prokládání napříč více dat disky. Různé možnosti pro přesunutí těchto záložních souborů a správu omezení velikosti souborů a výkon při zpracování terabajtů dat, jsou uvedeny dále v tomto článku.

Další možnost, která nabízí dává větší svobodu týkající se celkové kapacity, je úložiště objektů blob Azure. Při jediného objektu blob se také s omezeným přístupem too1 TB celkové kapacity hello jediného objektu blob kontejneru je aktuálně 500 TB. Kromě toho nabízí zákazníkům hello choice tooselect takzvané &quot;nástrojů&quot; úložiště objektů blob, který má výhody náklady. V tématu [Azure Blob Storage: za provozu a cool vrstvy úložiště](../../../storage/blobs/storage-blob-storage-tiers.md) podrobnosti o úložiště na studených objektů blob.

Pro dodatečné zabezpečení použijte zálohy geograficky replikované úložiště účet toostore hello SAP HANA. V tématu [replikace Azure Storage](../../../storage/common/storage-redundancy.md) podrobnosti o replikaci účet úložiště.

V účtu vyhrazeného zálohování úložiště, který je geograficky replikované jeden by mohlo způsobit vyhrazené virtuální pevné disky pro SAP HANA zálohy. Jinak jeden zkopírovat hello virtuální pevné disky, které udržují hello SAP HANA zálohy tooa geograficky replikované úložiště účet nebo účet tooa úložiště, který je v jiné oblasti.

## <a name="azure-backup-agent"></a>Azure backup agent

Azure backup nabízí hello možnost toonot zálohovat pouze kompletní virtuální počítače, ale také soubory a adresáře prostřednictvím hello zálohování agenta, který má toobe nainstalovaná na hello hostovaný operační systém. Ale od prosince 2016 tohoto agenta je podporována pouze v systému Windows (viz [zálohování Windows serveru nebo klienta tooAzure pomocí modelu nasazení Resource Manager hello](../../../backup/backup-configure-vault.md)).

Alternativní řešení je toofirst kopie SAP HANA tooa záložní soubory virtuálního počítače s Windows v Azure (například prostřednictvím SAMBA sdílení) a potom pomocí hello Azure backup agent z ní. I když je technicky možné, ho by přidání složitosti a zpomalit hello zálohování nebo obnovení z odlišují důvodu toohello kopírování mezi hello Linux a hello virtuální počítač s Windows. Není doporučeno toofollow tento přístup.

## <a name="azure-blobxfer-utility-details"></a>Podrobnosti nástroj Azure blobxfer

toostore adresářů a souborů na úložiště Azure, jeden může pomocí rozhraní CLI nebo Powershellu, nebo vytvořte nástroj, pomocí jedné z hello [sady Azure SDK](https://azure.microsoft.com/downloads/). Pro kopírování tooAzure úložiště dat je také nástroj připravené k použití, AzCopy, ale je Windows jenom (viz [přenos dat pomocí hello nástroj příkazového řádku AzCopy](../../../storage/common/storage-use-azcopy.md)).

Proto blobxfer byl použit pro kopírování SAP HANA záložní soubory. Je open source mnoho zákazníků v produkčním prostředí, dostupná a použitá na [Githubu](https://github.com/Azure/blobxfer). Tento nástroj umožňuje jeden datový toocopy přímo tooeither Azure blob storage nebo sdílenou složku Azure. Také nabízí celou řadu užitečných funkcí, jako je hodnota hash md5 nebo automatické paralelismus při kopírování adresáře s více soubory.

## <a name="sap-hana-backup-performance"></a>Výkon zálohování SAP HANA

![Je tento snímek hello SAP HANA zálohování konzoly v SAP HANA Studio](media/sap-hana-backup-file-level/image023.png)

Tomto snímku obrazovky je hello SAP HANA zálohování konzoly v SAP HANA Studio. Trvalo asi 42 minut toodo hello zálohování z hello 230 GB na disku jednoho Azure standardní úložiště připojené toohello HANA počítač používající systém souborů XFS.

![Tomto snímku obrazovky je YaST na hello SAP HANA testovacího virtuálního počítače](media/sap-hana-backup-file-level/image024.png)

Tomto snímku obrazovky je YaST na hello SAP HANA testovacího virtuálního počítače. Hello 1 TB jednoho disku pro zálohu SAP HANA jeden můžete vidět, jak je uvedeno nahoře. Trvalo asi 42 minut toobackup 230 GB. Kromě toho byla připojena pět disků 200 GB a vytvořit md0 softwaru diskového pole RAID, s proložení na základě těchto pět disků dat Azure.

![Opakující se hello stejné zálohování na softwaru diskového pole RAID s proložení mezi pěti připojenými disky dat úložiště Azure úrovně standard](media/sap-hana-backup-file-level/image025.png)

Stejné zálohování na softwaru diskového pole RAID s proložení mezi pěti opakovaných hello připojit čas zálohování disky uvést do režimu hello dat pro úložiště Azure standardní 42 minut dolů too10 minut. bez ukládání do mezipaměti toohello virtuálního počítače byly připojené disky Hello. Proto je zřejmé jak důležité rychlost zapisování na disk je pro hello čas zálohování. Jeden může pak přepínače tooAzure premium storage toofurther urychlit proces hello k zajištění optimálního výkonu. Obecně platí Azure premium storage je třeba použít pro produkční systémy.

## <a name="copy-sap-hana-backup-files-tooazure-blob-storage"></a>Zkopírujte úložiště objektů blob tooAzure záložní soubory SAP HANA

Prosinec 2016 hello nejlépe je možnost tooquickly úložiště SAP HANA záložní soubory úložiště objektů blob Azure. Jednoho kontejneru typu jediného objektu blob může obsahovat maximálně 500 TB, dostatek většina systémů SAP HANA spuštěné ve virtuálním počítači GS5 v Azure, tookeep dostatečná SAP HANA zálohy. Zákazníci mají hello volba mezi &quot;aktivní&quot; a &quot;studenou&quot; úložiště objektů blob (najdete v části [Azure Blob Storage: za provozu a cool vrstvy úložiště](../../../storage/blobs/storage-blob-storage-tiers.md)).

S nástrojem blobxfer hello je snadno toocopy hello SAP HANA záložní soubory přímo tooAzure úložiště objektů blob.

![Zde jeden uvidí hello soubory úplného zálohování souboru SAP HANA](media/sap-hana-backup-file-level/image026.png)

Zde jeden můžete zobrazit soubory hello úplného zálohování souboru SAP HANA. Existují čtyři soubory a hello největších jeden má zhruba 230 GB.

![Trvalo zhruba 3000 sekund kontejner objektů blob účet tooan toocopy hello 230 GB úložiště Azure úrovně standard](media/sap-hana-backup-file-level/image027.png)

V počátečním testu hello nepoužíváte hodnota hash md5, trvalo zhruba 3000 sekund kontejner objektů blob účet tooan toocopy hello 230 GB úložiště Azure úrovně standard.

![Na tomto snímku obrazovky jeden viděli, jak vypadá na hello portálu Azure](media/sap-hana-backup-file-level/image028.png)

Na tomto snímku obrazovky jeden můžete zobrazit, jak vypadá na hello portálu Azure. Kontejner objektů blob s názvem &quot;sap hana zálohování&quot; byl vytvořen a zahrnuje hello čtyři objekty BLOB, které představují hello SAP HANA záložní soubory. Jeden z nich má velikost přibližně 230 GB.

zálohování konzoly Hello HANA Studio umožňuje jeden toorestrict hello maximální velikost souboru HANA záložní soubory. V prostředí ukázkový text hello zvýšení výkonu tím, že ji možné toohave více menší záložní soubory místo jednoho velkého souboru 230 GB.

![Nastavení omezení velikosti záložní soubor hello hello HANA straně nemá & č. 39; t zlepšit čas zálohování hello](media/sap-hana-backup-file-level/image029.png)

Nastavení omezení velikosti záložní soubor hello hello HANA straně nemá & č. 39; t zlepšit hello čas zálohování, protože hello soubory jsou zapsány postupně, jak je vidět na tomto obrázku. limit velikosti souborů Hello byl nastaven too60 GB, tak hello záloha byla vytvořena čtyři velké datové soubory místo hello 230 GB jedním souborem.

![tootest paralelismu hello blobxfer nástroje, hello maximální velikost souboru pro HANA zálohy se potom nastavte too15 GB](media/sap-hana-backup-file-level/image030.png)

tootest paralelismu hello blobxfer nástroje, hello maximální velikost souboru pro zálohování HANA byl nastaven pak too15 GB, což vedlo 19 záložní soubory. Tato konfigurace brzy hello dobu blobxfer toocopy hello 230 GB tooAzure úložiště objektů blob z 3000 sekund dolů too875 sekund.

Tento výsledek je z důvodu omezení toohello 60 MB/s pro zápis objektů blob Azure. Paralelismus prostřednictvím více objektů BLOB řeší hello problémové místo, ale je nevýhodou: zvýšení výkonu hello blobxfer nástroj toocopy všechny tyto tooAzure záložní soubory HANA úložiště objektů blob dochází zatížení hello HANA virtuálního počítače a sítě hello. Operace systému HANA stane dopad.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>Objekt BLOB kopii vyhrazené Azure datové disky v zálohování softwaru diskového pole RAID

Na rozdíl od hello ruční zálohování virtuálních počítačů datového disku v tomto přístupu jeden zálohovat všechny hello datové disky na virtuální počítač toosave hello celou SAP instalaci, včetně dat HANA HANA neprotokoluje soubory a soubory konfigurace. Místo toho je vhodné hello je toohave vyhrazené softwaru diskového pole RAID s proložení napříč více dat Azure virtuální pevné disky pro ukládání úplné zálohování souboru SAP HANA. Jeden zkopíruje pouze tyto disky, které mají hello SAP HANA zálohování. Může být snadno zachovány v vyhrazený účet úložiště pro zálohy HANA, nebo připojené tooa vyhrazené &quot;zálohování správu virtuálních počítačů&quot; pro další zpracování.

![Všechny virtuální pevné disky související se situací byly zkopírovány pomocí hello ** start-azurestorageblobcopy ** příkaz prostředí PowerShell](media/sap-hana-backup-file-level/image031.png)

Po dokončení zálohování toohello hello místního softwaru diskového pole RAID, byly zkopírovány všechny virtuální pevné disky související se situací pomocí hello **start-azurestorageblobcopy** příkaz prostředí PowerShell (najdete v části [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Jak ovlivňuje pouze systém souborů hello vyhrazené pro udržování hello záložních souborů, nejsou žádné pochybnostmi SAP HANA dat či protokolu konzistence souborů na disku hello. Výhodou tohoto příkazu je, že funguje při hello virtuálního počítače zůstane online. být toobe určité, že žádný proces zapíše toohello zálohování stripe sady, že toounmount před hello kopírování objektu blob a ji připojit znovu později. Nebo jeden pomocí odpovídající příliš&quot;freeze&quot; hello systému souborů. Například prostřednictvím xfs\_freeze pro systém souborů XFS hello.

![Tento snímek obrazovky ukazuje hello seznam objektů BLOB v kontejneru hello virtuální pevné disky na hello portálu Azure](media/sap-hana-backup-file-level/image032.png)

Tento snímek obrazovky ukazuje hello seznam objektů BLOB v hello &quot;virtuální pevné disky&quot; kontejneru na hello portálu Azure. Hello – snímek obrazovky ukazuje hello pět virtuální pevné disky, které byly připojené tooserve virtuálních počítačů serveru SAP HANA toohello jako hello softwaru diskového pole RAID tookeep SAP HANA záložní soubory. Také ukazuje hello pět kopií, které byly provedeny prostřednictvím příkazu Kopírovat objekt blob hello.

![Pro účely testování hello kopie disků RAID zálohovací software SAP HANA hello byly připojené toohello aplikačního serveru virtuálního počítače](media/sap-hana-backup-file-level/image033.png)

Pro účely testování hello kopie disků RAID zálohovací software SAP HANA hello byly připojené toohello aplikačního serveru virtuálního počítače.

![server aplikace Hello virtuální počítač byl vypnut kopie disku tooattach hello](media/sap-hana-backup-file-level/image034.png)

server aplikace Hello virtuální počítač byl vypnut kopie disku tooattach hello. Po spuštění hello virtuálních počítačů, hello disky a hello RAID byly zjištěny správně (připojené prostřednictvím UUID). Pouze hello přípojný bod nebyl nalezen, který se vytvořil prostřednictvím dělicí metody YaST hello. Později hello SAP HANA záložní soubor kopie se stala viditelné na úrovni operačního systému.

## <a name="copy-sap-hana-backup-files-toonfs-share"></a>Zkopírujte SAP HANA záložní soubory, které tooNFS sdílet

hello potenciální dopad toolessen na hello systému SAP HANA z hlediska místa na disku nebo výkonu, jeden zvážit ukládat hello SAP HANA záložní soubory do sdílené složky NFS. Technicky funguje, ale znamená použití druhý virtuální počítač Azure jako hostitele hello hello k sdíleným složkám NFS. Neměl by být malá velikost virtuálního počítače, z důvodu toohello šířky pásma sítě virtuálních počítačů. By byla smysl pak tooshut dolů to &quot;zálohování virtuálních počítačů&quot; a převeďte ho pouze pro provádění zálohování hello SAP HANA. Zápis v systému souborů NFS sdílené složky vloží zatížení v síti hello a ovlivňuje hello SAP HANA systému, ale jenom správu hello záložní soubory později na hello &quot;zálohování virtuálních počítačů&quot; nebude vůbec ovlivnit systému SAP HANA hello.

![Sdílené složky NFS z jiného virtuálního počítače Azure se serveru SAP HANA připojené toohello virtuálních počítačů](media/sap-hana-backup-file-level/image035.png)

případ použití tooverify hello systému souborů NFS, sdílené složky NFS z jiného virtuálního počítače Azure byla serveru SAP HANA připojené toohello virtuálních počítačů. Došlo k dispozici žádné zvláštní systému souborů NFS ladění použít.

![Přímo trvalo 1 hodinu a 46 minut toodo hello zálohování](media/sap-hana-backup-file-level/image036.png)

sdílené složky NFS Hello se sadu rychlého stripe jako hello jeden na serveru SAP HANA hello. Nicméně trvalo 1 hodinu a 46 minut toodo hello zálohování přímo na sdílené složky NFS hello místo 10 minut, při zápisu tooa místní stripe set.

![alternativní Hello nebyl mnohem rychlejší v 1 a hod](media/sap-hana-backup-file-level/image037.png)

alternativní Hello provádění zálohování tooa místní prokládané sady a kopírování toohello sdílených složek NFS na úrovni operačního systému (jednoduchou **cp - avr** příkaz) nebyla mnohem rychlejší. Trvalo 1 a hod.

Proto funguje, ale nebyl výkonu vhodné pro testovací zálohování hello 230 GB. Aby vypadal i zhoršení pro více terabajtů.

## <a name="copy-sap-hana-backup-files-tooazure-file-service"></a>Zkopírujte služba souborů tooAzure záložní soubory SAP HANA

Je možné toomount uvnitř virtuálního počítače Azure Linux sdílené složky Azure file. článek Hello [jak toouse Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md) obsahuje podrobné informace o tom, toodo ho. Uvědomte si, že je aktuálně 5 TB kvótu jednu sdílenou složku Azure a maximální velikost souboru 1 TB na soubor. V tématu [a cíle výkonnosti služby Azure Storage Scalability](../../../storage/common/storage-scalability-targets.md) informace o omezení úložiště.

Testy ukázaly, však, že zálohování nemá SAP HANA & č. 39; t aktuálně pracovat přímo s Tento druh CIFS připojení. Je také uvádí [1820529 Poznámka SAP](https://launchpad.support.sap.com/#/notes/1820529) , CIFS se nedoporučuje.

![Tento obrázek zobrazuje chybu v zálohování dialogové okno hello ve SAP HANA Studio](media/sap-hana-backup-file-level/image038.png)

Při pokusu o tooback až přímo tooa CIFS připojit sdílenou složku Azure tento obrázek ukazuje chybu v hello zálohování dialogové okno, ve studiu SAP HANA. Tak, aby jeden má toodo zálohu standardní SAP HANA do systému souborů virtuálního počítače první, a pak hello záložní kopii souborů z tam tooAzure služba souborů.

![Následující obrázek ukazuje, že trvalo o 929 sekund toocopy 19 SAP HANA záložní soubory](media/sap-hana-backup-file-level/image039.png)

Následující obrázek ukazuje, že trvalo o 929 sekund toocopy 19 SAP HANA záložní soubory s celkovou velikost přibližně 230 GB toohello sdílenou složku Azure.

![Hello zdroj strukturu adresáře na hello SAP HANA virtuálního počítače byla zkopírovaný toohello Azure sdílené složky](media/sap-hana-backup-file-level/image040.png)

Na tomto snímku obrazovky jeden uvidí, že hello zdroj strukturu adresáře na hello SAP HANA virtuálního počítače byla zkopírovaný toohello Azure sdílené složky: jeden adresář (hana\_zálohování\_fsl\_15 gb) a 19 jednotlivých záložní soubory.

Ukládání záložních souborů SAP HANA na soubory Azure může být v budoucnu hello zajímavé možnost, při přímo záloh souboru SAP HANA ho podporují. Nebo když se stane možné toomount Azure soubory pomocí systému souborů NFS a maximální kvóty hello je podstatně vyšší než 5 TB.

## <a name="next-steps"></a>Další kroky
* [Příručce zálohování pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md) nabízí přehled a informace o zahájení práce.
* [SAP HANA zálohování podle úložiště snímků](sap-hana-backup-storage-snapshots.md) popisuje hello úložiště založené na snímku možnost zálohování.
* jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA v Azure (velké instance), najdete v části toolearn [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).
