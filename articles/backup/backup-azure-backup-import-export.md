---
title: "Služba Azure Import/Export hello aaaAzure zálohování - zálohování Offline nebo počáteční synchronizace replik indexů pomocí | Microsoft Docs"
description: "Zjistěte, jak Azure Backup vám umožní toosend data mimo hello sítě pomocí služby Azure Import/Export hello. Tento článek vysvětluje hello offline synchronizace replik indexů hello dat prvotní zálohy pomocí služby Azure Import Export hello."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/20/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: f1696957c3e9684b800c8d030131255905459f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Pracovní postup offline zálohování ve službě Azure Backup
Zálohování Azure má několik předdefinovaných efektivitu, která ukládají náklady na síť a úložiště během hello počáteční úplné zálohy data tooAzure. Počáteční úplné zálohování obvykle přenos velkých objemů dat a vyžaduje větší šířku pásma sítě při porovnání s toosubsequent zálohování, které přenášejí pouze hello rozdílů/přírůstková. Zálohování Azure komprimaci hello prvotní zálohy. Prostřednictvím hello proces offline synchronizace replik indexů Azure Backup můžete použít offline tooAzure disky tooupload hello komprimované dat prvotní zálohy.  

Hello offline synchronizace replik indexů procesu služby Azure Backup je úzce integrovaná s hello [služba Azure Import/Export](../storage/common/storage-import-export-service.md) , která umožní tooAzure tootransfer dat pomocí disků. Pokud máte terabajtů (TBs) počáteční zálohovací data, která potřebuje toobe přenést přes síť s vysokou latencí a malou šířkou pásma, můžete na jeden nebo více pevných disků tooan datové centrum Azure hello offline synchronizace replik indexů pracovního postupu tooship hello kopie prvotní zálohy. Tento článek obsahuje přehled hello kroky, které dokončit tento pracovní postup.

## <a name="overview"></a>Přehled
Hello funkci offline synchronizace replik indexů Azure Backup a Azure Import/Export je jednoduchý tooupload hello dat offline tooAzure použitím disků. Namísto použití přenosu přes síť hello hello počáteční úplná kopie, hello záložní data jsou zapsána tooa *pracovního umístění*. Po dokončení hello kopie toohello pracovní umístění pomocí nástroje Azure Import/Export hello tato data jsou zapsány tooone nebo SATA další jednotky, v závislosti na množství dat na hello. Tyto disky jsou nakonec uvidíte toohello nejbližší datového centra Azure.

Hello [srpna 2016 aktualizace služby Azure Backup (a novější)](http://go.microsoft.com/fwlink/?LinkID=229525) zahrnuje hello *nástroj pro přípravu Azure Disk*, s názvem AzureOfflineBackupDiskPrep, který:

* Vám pomůže připravit vaše jednotky pro Import úložiště Azure pomocí nástroje Azure Import/Export hello.
* Automaticky vytvoří úlohu Import úložiště Azure pro hello služba Azure Import/Export na hello [portál Azure classic](https://manage.windowsazure.com) jako názvem na rozdíl od toocreating hello stejné ručně pomocí starší verze služby Azure Backup.

Po dokončení nahrávání hello hello zálohovaná data tooAzure Azure Backup zkopíruje hello zálohovaná data toohello úložiště záloh a hello přírůstkové zálohování.

> [!NOTE]
> hello toouse nástroj pro přípravu Azure Disk, ujistěte se, že máte nainstalovanou aktualizaci srpna 2016 hello služby Azure Backup (nebo novější) a proveďte všechny kroky hello hello pracovního postupu s ním. Pokud používáte starší verzi služby Azure Backup, můžete připravit jednotky SATA hello pomocí nástroje Azure Import/Export hello podle popisu v pozdějších částech tohoto článku.
>
>

## <a name="prerequisites"></a>Požadavky
* [Seznamte se s pracovním postupem Azure Import/Export hello](../storage/common/storage-import-export-service.md).
* Před zahájením hello pracovní postup, ověřte následující hello:
  * Byla vytvořena trezoru zálohování Azure.
  * Stažené přihlašovací údaje trezoru.
  * na Windows Server nebo Windows klienta nebo serveru System Center Data Protection Manager se nainstaloval agenta Azure Backup Hello a počítač hello je registrován s úložištěm záloh Azure hello.
* [Stáhnout nastavení publikování Azure souborů hello](https://manage.windowsazure.com/publishsettings) na hello počítači, ze kterého plánujete tooback dat.
* Příprava pracovní umístění, což může být na sdílené síťové složce nebo další jednotku na počítači hello. Hello pracovního umístění je přechodným úložištěm a používá se dočasně během tento pracovní postup. Zajistěte, aby byl tento hello pracovního umístění dostatek toohold místa na disku počáteční kopii. Například pokud se pokoušíte tooback až 500 GB souborový server, zajistěte, že hello pracovní oblasti alespoň 500 GB. (Menší velikost slouží kvůli toocompression.)
* Ujistěte se, zda používáte podporované jednotku. Pouze 2,5 SSD nebo 2.5 nebo 3,5 SATA II/III interní pevné disky jsou podporovány pro použití s hello služby importu a exportu. Můžete si too10 TB pevné disky. Zkontrolujte hello [dokumentace ke službě Azure Import/Export](../storage/common/storage-import-export-service.md#hard-disk-drives) pro hello nejnovější sadu disků, které hello služba podporuje.
* Povolit nástroj BitLocker na toowhich hello počítač je připojen zapisovače jednotky SATA hello.
* [Stáhněte si nástroj Azure Import/Export hello](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) toohello počítače toowhich hello SATA jednotky zapisovače je připojen. Tento krok není požadováno, pokud jste stáhli a nainstalovali hello srpna 2016 aktualizace služby Azure Backup (nebo novější).

## <a name="workflow"></a>Pracovní postup
Hello informace v této části vám pomohou dokončit pracovní postup offline zálohování hello tak, aby vaše data mohou být zajišťovány tooan datové centrum Azure a odeslat tooAzure úložiště. Pokud máte dotazy týkající se služby Import hello či jiného aspektu hello procesu, najdete v části hello [Přehled služby importu](../storage/common/storage-import-export-service.md) dokumentace odkazuje dříve.

### <a name="initiate-offline-backup"></a>Spustit zálohování offline
1. Pokud naplánujete zálohování, zobrazí hello následující obrazovku (v systému Windows Server, klient systému Windows nebo System Center Data Protection Manager).

    ![Import obrazovky](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Tady je úvodní obrazovka odpovídající v System Center Data Protection Manager: <br/>
    ![Aplikace DPM import obrazovky](./media/backup-azure-backup-import-export/dpmoffline.png)

    Popis Hello hello vstupy vypadá takto:

    * **Pracovní umístění**: hello dočasné úložiště umístění toowhich hello kopie prvotní zálohy je zapsán. To může být na sdílené síťové složky nebo místního počítače. Pokud se liší hello kopie počítače a zdrojový počítač, doporučujeme, že zadáváte cestu plné síti hello hello pracovního umístění.
    * **Azure název úlohy importu**: hello jedinečný název, který Import úložiště Azure service a Azure Backup sledovat hello přenos dat odesílaných na tooAzure disky.
    * **Nastavení Azure publikování**: soubor ve formátu XML, který obsahuje informace o profilu vašeho předplatného. Obsahuje taky zabezpečené přihlašovací údaje, které jsou spojeny s předplatným. Můžete [stáhnout soubor hello](https://manage.windowsazure.com/publishsettings). Zadejte soubor nastavení publikování toohello hello místní cesta.
    * **ID předplatného Azure**: hello ID předplatného Azure, pro které máte v plánu úlohy importu v Azure hello tooinitiate předplatné hello. Pokud máte víc předplatných Azure, použijte ID hello hello předplatného, které chcete tooassociate pomocí úlohy importu hello.
    * **Účet služby Azure Storage**: hello klasického typu na účet úložiště v hello zadané předplatné Azure, který bude přidružen hello úlohy importu v Azure.
    * **Kontejner úložiště Azure**: název hello hello cílový objekt blob úložiště v hello účtu úložiště Azure, kde tato úloha importu.

    > [!NOTE]
    > Pokud jste si zaregistrovali váš server tooan trezoru služeb zotavení Azure z hello [portál Azure](https://portal.azure.com) pro zálohování a nejsou ve Cloud Solution Provider (CSP) předplatné, můžete stále vytvořit classic typ účtu úložiště z hello Portál Azure a použít jej pro pracovní postup offline zálohování hello.
    >
    >

     Všechny tyto informace uložit, protože potřebujete tooenter ho znovu za postupem. Pouze hello *pracovního umístění* se vyžaduje, pokud jste použili hello Příprava disku Azure nástroj tooprepare hello disky.    
2. Dokončení hello pracovního postupu a potom vyberte **zálohovat nyní** v hello Azure Backup správy konzoly tooinitiate hello offline zálohování kopírováním. prvotní zálohování Hello je zapsán toohello pracovní oblasti v rámci tohoto kroku.

    ![Zálohovat nyní](./media/backup-azure-backup-import-export/backupnow.png)

    toocomplete hello odpovídající pracovní postup v nástroji System Center Data Protection Manager, klikněte pravým tlačítkem na hello **skupiny ochrany**a potom zvolte hello **vytvořit bod obnovení** možnost. Potom vyberte hello **Online ochrany** možnost.

    ![Aplikace DPM zálohovat nyní](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Po dokončení operace hello hello pracovního umístění je připraven toobe použít pro přípravu disku.

    ![Postup zálohování](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-hello-azure-disk-preparation-tool"></a>Příprava jednotky SATA a vytvořte úlohy importu v Azure pomocí nástroje pro přípravu Azure Disk hello
Nástroj pro přípravu Azure Disk Hello je k dispozici v instalační adresář agenta služeb zotavení hello (aktualizovat srpna 2016 a novější) v následující cestě hello.

   *\Microsoft* *azure* *obnovení* *služby* * Agent\Utils\*

1. Přejděte toohello adresář a kopírování hello **AzureOfflineBackupDiskPrep** directory tooa kopie počítače, na které hello jsou připojené jednotky toobe připravený. Zkontrolujte následující hello s ohledem toohello kopie počítače:

    * Hello kopie počítač získá přístup k hello pracovního umístění pro pracovní postup offline synchronizace replik indexů hello pomocí hello stejná síťová cesta, která byla součástí hello **zahájit zálohování offline** pracovního postupu.
    * Nástroj BitLocker je povolen v počítači hello.
    * Hello počítač získá přístup k hello portálu Azure.

    V případě potřeby hello kopie počítače může být hello stejný jako zdrojový počítač hello.
2. Otevření příkazového řádku se zvýšenými oprávněními v počítači kopie hello adresářem nástroj Příprava disku Azure hello jako aktuální adresář hello a spusťte hello následující příkaz:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path tooPublishSettingsFile*>]`

    | Parametr | Popis |
    | --- | --- |
    | s:&lt;*cesta pracovní umístění*&gt; |Povinné vstup, který byl použit tooprovide hello cesta toohello pracovní umístění, které jste zadali v hello **zahájit zálohování offline** pracovního postupu. |
    | p:&lt;*tooPublishSettingsFile cesta*&gt; |Volitelné vstup, který byl použit tooprovide hello cesta toohello **nastavením publikování v Azure** soubor, který jste zadali v hello **zahájit zálohování offline** pracovního postupu. |

    > [!NOTE]
    > Hello &lt;tooPublishSettingFile cesta&gt; hodnota je povinný, když hello kopie počítače a zdrojový počítač se liší.
    >
    >

    Při spuštění příkazu hello hello nástroj požadavků hello výběr hello úlohy importu v Azure, která odpovídá toohello jednotky, které je třeba toobe připravený. Pokud jenom jeden import úlohy je spojena s hello zadat pracovní umístění, uvidíte obrazovku podobnou hello jeden, který následuje dále.

    ![Azure vstup nástroj Příprava disku](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Zadejte písmeno jednotky hello bez koncové dvojtečkou pro hello připojeného disku má tooprepare pro přenos tooAzure hello. Potvrzení pro hello formátování disku hello po zobrazení výzvy zadejte.

    Nástroj Hello poté zahájí tooprepare hello disk s hello zálohovaná data. Může být nutné tooattach dalších disků. Po zobrazení výzvy nástroj hello v případě, že hello za předpokladu, že disk nemá dostatek místa pro hello zálohovaná data. <br/>

    Na konci hello tohoto úspěšné provedení hello nástroj jeden nebo více disků, které jste zadali připravené pro přesouvání tooAzure. Kromě toho úlohy importu s názvem hello jste zadali při hello **zahájit zálohování offline** pracovní postup je vytvořen na hello portál Azure classic. Nakonec zobrazí nástroj hello hello přenosů toohello adres Azure datacenter, kde musí toobe dodaný hello disky a hello odkaz toolocate hello úlohy importu v hello portál Azure classic.

    ![Příprava Azure disku dokončení](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Příjemce hello disky toohello adresy tohoto hello nástroj, který poskytuje a zachovat hello sledování Číslo pro budoucí použití.<br/>

5. Když přejdete toohello propojit tohoto nástroje hello zobrazí, najdete v části hello účtu úložiště Azure, který jste zadali v hello **zahájit zálohování offline** pracovního postupu. Zde můžete zobrazit úlohy importu hello nově vytvořený na hello **IMPORTU a EXPORTU** kartě hello účtu úložiště.

    ![Úlohy vytvořené importu](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Klikněte na tlačítko **informace o PŘESOUVÁNÍ** dole hello tooupdate stránku hello kontaktu Podrobnosti, jak je znázorněno v následující obrazovce hello. Společnost Microsoft používá tento tooship informace o dokončení vaše disky back tooyou po hello importovat úlohy.

    ![Kontaktní údaje](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Zadejte hello přesouvání podrobnosti na další obrazovce hello. Zadejte hello **doručení poskytovatel** a **sledování Číslo** podrobnosti, které odpovídají toohello disky, můžete dodaný toohello datového centra Azure.

    ![Informace o přesouvání](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-hello-workflow"></a>Pracovní postup dokončení hello
Po dokončení úlohy importu hello je k dispozici ve vašem účtu úložiště dat prvotní zálohy. Hello agenta služeb zotavení pak zkopíruje obsah hello hello dat z tohoto účtu úložiště záloh toohello nebo služeb zotavení trezoru, podle toho, která se vztahuje. V době hello příští plánovaná zálohování provede hello Azure Backup agent hello přírůstkové zálohování přes hello kopie prvotní zálohy.

> [!NOTE]
> Hello následující části platí toousers starší verze služby Azure Backup, kteří nemají nástroj pro přípravu disku Azure toohello přístup.
>
>

### <a name="prepare-a-sata-drive"></a>Příprava jednotky SATA
1. Stáhnout hello [nástroj Microsoft Azure Import/Export](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) toohello kopie počítače. Zkontrolujte, že hello pracovního umístění je přístupné z počítače hello, ve které máte v plánu toorun hello další sadu příkazů. V případě potřeby hello kopie počítače může být hello stejný jako zdrojový počítač hello.

2. Rozbalte soubor WAImportExport.zip hello. Spusťte nástroj WAImportExport hello, která formáty jednotky SATA hello, zapíše hello zálohovaná data toohello SATA jednotky a zašifruje. Než spustíte následující příkaz hello, ujistěte se, povolení nástroje BitLocker v počítači hello. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Pokud máte nainstalovanou aktualizaci srpna 2016 hello služby Azure Backup (nebo novější), ujistěte se, že hello pracovního umístění, kterou jste zadali je hello stejně jako na hello jeden hello **zálohovat nyní** obrazovky a obsahuje soubory AIB a základní objektů Blob.
    >
    >

| Parametr | Popis |
| --- | --- |
| /j: <*JournalFile*> |Hello cestě toohello deníku souboru. Každé jednotky, musí mít přesně jeden soubor deníku. soubor deníku Hello nesmí být na cílové jednotce hello. Přípona souboru deníku Hello je .jrn a je vytvořen jako součást spuštění tohoto příkazu. |
| /ID: <*SessionId*> |ID relace Hello identifikuje relaci kopírování. Je přesné obnovení použité tooensure kopie dojde k přerušení relace. Soubory, které jste zkopírovali v relaci kopie jsou uloženy v adresáři s názvem po ID relace hello na cílové jednotce hello. |
| /Sk: <*StorageAccountKey*> |klíč účtu Hello hello úložiště účet toowhich hello dat je naimportována. Hello klíčových požadavků hello toobe stejný jako jeho byl zadán během vytváření skupiny zálohování zásady/ochrany. |
| / BlobType |Hello typ objektu blob. Tento pracovní postup úspěšné pouze v případě **PageBlob** je zadán. Toto není hello výchozí možnost a by se měla uvádět v tomto příkazu. |
| / t: <*TargetDriveLetter*> |písmeno jednotky Hello bez hello koncové dvojtečkou pevný disk cílového hello hello aktuální kopie relace. |
| / Format |Hello možnost tooformat hello jednotka. Zadejte tento parametr, když hello jednotky potřebuje toobe formátu; jinak vynechejte. Předtím, než nástroj hello formáty hello jednotky, zobrazí výzvu k potvrzení z konzoly hello. toosuppress hello potvrzení, zadejte parametr /silentmode hello. |
| / šifrování |Hello možnost tooencrypt hello jednotka. Tento parametr zadejte, když s nástrojem BitLocker a potřebuje toobe šifrované nástrojem hello nebyl ještě zašifrované jednotky hello. Pokud hello jednotka již byla zašifrována pomocí nástroje BitLocker, tento parametr vynecháte, zadejte parametr /bk hello a zadejte existující klíč nástroje BitLocker hello. Pokud zadáte parametr/Format hello, musíte také zadat hello nebo šifrování parametru. |
| /srcdir: <*SourceDirectory*> |Hello zdrojový adresář, který obsahuje soubory toobe zkopírovat toohello cílové jednotky. Zajistěte, aby byl tento název zadaný adresář hello úplné místo relativní cesty. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |Hello cesta toohello cílový virtuální adresář ve vašem účtu úložiště Azure. Zda toouse být názvy kontejnerů platný při zadávání hello cílové virtuální adresáře nebo objekty BLOB. Uvědomte si, že názvy kontejnerů musí být malými písmeny.  Tento název kontejneru musí být hello ten, který jste zadali při vytváření skupiny zálohování zásady/ochrany. |

> [!NOTE]
> Soubor deníku se vytvoří ve složce WAImportExport hello, shromažďuje informace celý hello hello pracovního postupu. Tento soubor musíte při vytváření úlohy importu v hello portálu Azure.
>
>

  ![Výstup prostředí PowerShell](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-hello-azure-portal"></a>Vytvoření úlohy importu v hello portálu Azure
1. Přejděte tooyour účet úložiště v hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **importu a exportu**a potom **vytvoření úlohy importu** v podokně úloh hello.

    ![Import a export ve hello portálu Azure](./media/backup-azure-backup-import-export/azureportal.png)

2. V kroku 1 Průvodce hello znamenat, že je vše připraveno jednotce a, abyste měli k dispozici souboru hello disku deníku.

3. V kroku 2 Průvodce hello zadejte kontaktní informace pro hello osobě, která je zodpovědná za tuto úlohu importu.

4. V kroku 3 nahrajte soubory deníku hello jednotky, které jste získali v předchozí části hello.

5. V kroku 4 zadejte popisný název úlohy importu hello, kterou jste zadali při vytváření skupiny zálohování zásady/ochrany. Hello název, který zadáte může obsahovat jenom malá písmena, číslice, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery. Hello název, který zvolíte, je použít tootrack úlohách době, kdy jsou v průběhu a po jejich dokončení.

6. V dalším kroku vyberte oblast datacenter hello seznamu. oblast datacenter Hello označuje datacenter a adresu toowhich hello je nutné dodat vašeho balíčku.

    ![Vyberte oblast, datacenter](./media/backup-azure-backup-import-export/dc.png)

7. V kroku 5 vyberte vaše návratový poskytovatel hello seznamu a zadejte číslo účtu poskytovatel. Společnost Microsoft používá tento účet tooship jednotky zpět tooyou po dokončení úlohy importu.

8. Dodávat hello disk a zadejte hello sledování Číslo stav tootrack hello dodávky hello. Po hello disku dorazí v hello datacenter, je zkopírovat toohello účet úložiště a hello stav se aktualizuje.

    ![Stavu dokončení](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-hello-workflow"></a>Pracovní postup dokončení hello
Po počáteční zálohovací data hello je k dispozici ve vašem účtu úložiště hello agenta služeb zotavení Microsoft Azure zkopíruje obsah hello hello dat z tohoto účtu úložiště záloh toohello nebo trezor služeb zotavení, podle toho, která se vztahuje. V dalším plánu hello čas zálohování, hello Azure Backup agent provádí přírůstkové zálohování hello přes hello kopie prvotní zálohy.

## <a name="next-steps"></a>Další kroky
* V pracovním postupu Azure Import/Export hello nějaké dotazy, najdete v části příliš[používat hello Microsoft Azure Import/Export služby tootransfer data tooBlob úložiště](../storage/common/storage-import-export-service.md).
* Najdete v tématu zálohování offline toohello hello Azure Backup [– nejčastější dotazy](backup-azure-backup-faq.md) pro všechny dotazy týkající se hello pracovního postupu.
