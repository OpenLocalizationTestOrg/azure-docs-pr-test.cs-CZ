---
title: aaaInstall v2 serveru Azure Backup | Microsoft Docs
description: "Azure v2 zálohování serveru poskytuje vylepšené možnosti zálohování pro ochranu virtuálních počítačů, soubory a složky, úlohy a další. Zjistěte, jak tooinstall nebo upgradu tooAzure v2 zálohování serveru."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: 5b1699dadd3a173f1c0ef91a1a600bc5e12f20ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-azure-backup-server-v2"></a>Nainstalujte Azure Backup Server v2

Azure Backup Server pomáhá chránit vaše virtuální počítače (VM), úlohy, soubory a složky a další. Azure Backup Server v2 staví na serveru Azure Backup v1 a poskytuje nové funkce, které nejsou k dispozici v v1. Porovnání funkcí mezi v1 a v2 najdete v tématu [matice ochrany serveru Azure Backup](backup-mabs-protection-matrix.md). 

Hello další funkce Zálohování serveru v2 jsou upgrade z verze 1 zálohování serveru. V1 zálohování serveru ale není předpoklady pro instalaci v2 zálohování serveru. Pokud chcete tooupgrade z zálohovat Server v1 tooBackup Server v2, nainstalujte na hello zálohování serveru ochrany v2 zálohování serveru. Existující nastavení zálohování serveru zůstanou beze změn.

Zálohování serveru v2 můžete nainstalovat na Windows Server 2012 R2 nebo Windows Server 2016. tootake výhod nových funkcí, například System Center 2016 Data Protection Manager moderních úložiště záloh, v2 zálohování serveru je třeba nainstalovat do systému Windows Server 2016. Před provedením upgradu tooor instalace v2 Backup Server, přečtěte si informace o hello [požadavky pro instalaci](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure Backup Server má hello stejný kód základní jako System Center Data Protection Manager. Zálohování serveru v1 je ekvivalentní tooData Protection Manager 2012 R2 a v2 zálohování serveru je ekvivalentní tooData Protection Manager 2016. Tento článek příležitostně odkazuje dokumentace k Data Protection Manager hello.
>
>

## <a name="upgrade-backup-server-toov2"></a>Upgrade toov2 zálohování serveru
tooupgrade z zálohovat Server v1 tooBackup v2 serveru, zajistěte, aby vaše instalace má hello požadované aktualizace:

- [Aktualizovat agenty ochrany hello](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) na hello chráněných serverů.
- Upgrade systému Windows Server 2012 R2 tooWindows Server 2016.
- Správce vzdáleného serveru Azure Backup upgradujte na všech provozních serverech.
- Ujistěte se, že jsou zálohy nastaveny toocontinue bez restartování provozním serveru.


### <a name="upgrade-steps-for-backup-server-v2"></a>Postup upgradu serveru zálohování v2

1. V hello Download Center [stažení aktualizace instalačního programu hello](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Po extrahování hello Průvodce instalací, ujistěte se, že **spuštění setup.exe** vybrané, a pak vyberte možnost **Dokončit**.

  ![Instalační program instalaci - spustit instalační program](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. V průvodci Microsoft Azure Backup Server hello pod **nainstalovat**, vyberte **Microsoft Azure Backup Server**.

  ![Instalační program instalaci - vyberte možnost instalace](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Na hello **úvodní** , přečtěte si upozornění hello a pak vyberte **Další**.

  ![Instalační program instalaci – úvodní stránka](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. Průvodce instalací Hello provede toomake kontrol požadovaných součástí se, že můžete upgradovat prostředí. Na hello **požadovaných součástí kontroluje** vyberte **zkontrolujte**.

  ![Instalační program instalaci - požadovaných součástí kontroluje stránky](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Prostředí musí projít hello kontrol požadovaných součástí. Pokud vaše prostředí neprojde hello kontroly, poznamenejte si hello problémy a opravte je. Pak vyberte **zkontrolujte znovu**. Když předáte hello kontroly splnění podmínek, vyberte **Další**.

  ![Instalační program instalace - zkontrolujte znovu tlačítko](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Na hello **nastavení SQL** vyberte hello relevantní možnost pro instalaci SQL a potom vyberte **zkontrolovat a nainstalovat**.

  ![Instalační program instalaci - stránka nastavení SQL](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  Hello kontroly může trvat několik minut. Když jsou hello kontroluje dokončení, vyberte **Další**.

  ![Instalační program instalace - zkontrolujte nastavení SQL a tlačítko Instalovat](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. Na hello **nastavení instalace** stránky, zajistěte, aby všechny změny toohello umístění, kde je nainstalován Backup Server nebo toohello pomocné umístění. Vyberte **Další**.

  ![Instalační program instalaci - stránka nastavení instalace](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. toofinish hello Průvodce instalací, vyberte **Dokončit**.

  ![Instalační program instalaci - dokončit](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Přidání úložiště pro moderní úložiště záloh

tooimprove efektivitu úložiště zálohování, zálohování serveru v2 přidává podporu pro svazky. Podobně jako Backup Server v1 v2 zálohování serveru podporuje disky.

### <a name="add-volumes-and-disks"></a>Přidat svazky a disků
Pokud spustíte v2 zálohování serveru v systému Windows Server 2016, můžete data záloh toostore svazky. Svazky nabízejí úspory úložiště a rychlejší zálohování. Vzhledem k tomu, že jsou svazky tooBackup nový Server, musíte je přidat. 

Když přidáte tooBackup svazku serveru, můžete přiřadit svazek hello popisný název. Klikněte na tlačítko hello **popisný název** sloupec hello svazku chcete tooname. V případě potřeby můžete později změnit název hello. Také můžete použít tooadd prostředí PowerShell nebo změnit popisné názvy pro svazky.

tooadd svazek v hello konzoly pro správu:

1. V konzole správce serveru zálohování Azure hello, vyberte **správy** > **diskového úložiště** > **přidat**.

    ![Průvodce přidáním úložiště disku otevřete hello](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Otevře se Průvodce přidáním úložiště disku hello.

2. Na hello **přidání disku úložiště** stránku hello **dostupných svazků** , vyberte svazek a pak vyberte **přidat**.
3. V hello **vybrané svazky** pole, zadejte popisný název pro hello svazek a potom vyberte **OK**.

      ![Průvodce přidáním úložiště disku – Přidání svazku](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Pokud chcete tooadd disk, hello disk musí patřit tooa ochranné skupiny, která je starší verze úložiště. Tyto disky použít jenom pro tyto skupiny ochrany. Pokud Backup Server nemá zdroje, které mají další ochranu starší verze, se neuvádějí hello disku.

  Další informace o přidávání disků, najdete v tématu [přidávání disků tooincrease starší verze úložiště](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Disk nelze udělit popisný název.


### <a name="assign-workloads-toovolumes"></a>Přiřadit toovolumes úlohy

Zálohování serveru zadejte, které úlohy jsou přiřazeny toowhich svazky. Například můžete nastavit nákladné svazky, které podporují vysoký počet vstupně-výstupních operací na druhý (IOPS) úlohy pouze toostore, které vyžadují časté, vysoký počet záloh. Příkladem je SQL Server s protokoly transakcí.

#### <a name="update-dpmdiskstorage"></a>Aktualizace DPMDiskStorage

Vlastnosti hello tooupdate svazku ve fondu úložiště hello zálohování serveru, použijte rutinu Powershellu hello DPMDiskStorage aktualizace.

Syntaxe:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Všechny změny, které můžete provést pomocí prostředí PowerShell se projeví v hello uživatelského rozhraní.


## <a name="protect-data-sources"></a>Chránit zdroje dat
toobegin ochranu datových zdrojů, vytvořte skupinu ochrany. Hello následující kroky zvýraznění změny nebo přidání toohello průvodci nové skupiny ochrany.

toocreate skupinu ochrany:

1. V hello konzoly správce zálohování serveru, vyberte **ochrany**.

2. Na pásu karet nástroje hello, vyberte **nový**.

    Otevře se Průvodce vytvořením nové skupiny ochrany hello.

  ![Průvodce vytvořením nové skupiny ochrany](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. Na hello **úvodní** vyberte **Další**.
4. Na hello **vybrat typ skupiny ochrany** vyberte hello typ skupiny ochrany má toocreate a potom vyberte **Další**.

  ![Stránka Typ vyberte skupinu ochrany](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Na hello **vybrat členy skupiny** stránku hello **Dostupní členové** podokně, hello členy s jsou uvedeny agenty ochrany. V tomto příkladu vyberte svazek D:\ a E:\ a přidat je toohello **vybrané členy** podokně. Vyberte **Další**.

  ![Vyberte skupiny členy stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Na hello **vyberte způsob ochrany dat** stránky, zadejte **název skupiny ochrany**, vyberte způsob ochrany hello a pak vyberte **Další**. Pokud chcete krátkodobou ochranu, je nutné vybrat hello **disku** zálohování metoda.

  ![Vyberte způsob ochrany dat stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Na hello **zadat krátkodobé cíle** stránky, vyberte hello podrobnosti **rozsah uchování** a **četnost synchronizace**. Pak vyberte **Další**. Volitelně můžete toochange hello plán pro při body obnovení jsou přijatých, vyberte **upravit**.

  ![Určení krátkodobých cílů stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Na hello **zkontrolovat přidělení diskového úložiště** zkontrolujte podrobnosti o zdrojích dat hello jste vybrali, jejich velikosti a hodnoty pro toobe místo hello zřízený a hello cílový svazek úložiště.

  ![Stránka Kontrola přidělení diskového úložiště](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Svazky úložiště jsou založené na hello zatížení svazku přidělení (nastavte pomocí prostředí PowerShell) a hello úložiště k dispozici. Svazky úložiště hello můžete změnit výběrem jiných svazků v rozevírací nabídce hello. Pokud změníte hodnotu hello **úložiště v cíli**, hello hodnotu pro **úložiště disku** dynamicky změní tooreflect hodnoty v části **volného místa** a **Underprovisioned místo**.

  Pokud zdroje dat hello růst podle plánu, hello hodnotu hello **Underprovisioned místo** sloupec v **úložiště disku** odráží hello množství další úložiště, které je potřeba. Použijte tento plán toohelp hodnotu úložiště, musí technologie smooth zálohy. Pokud hello hodnota nula, neexistují v blízké budoucnosti hello potenciální problémy s úložištěm. Pokud je hodnota hello číslo větší než nula, nemáte dostatečné úložiště přidělené (podle vaší ochrany zásady a hello velikost dat chráněných členů).

  ![Nevytížených diskového úložiště](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   toofinish vytváření hello skupiny, dokončení Průvodce ochrany.

## <a name="migrate-legacy-storage-toomodern-backup-storage"></a>Migrovat starší verze úložiště tooModern úložiště záloh
Po upgradu tooor instalace v2 zálohování serveru a tooWindows hello upgradu operačního systému serveru 2016 se aktualizujte vaši toouse skupiny ochrany moderní úložiště záloh. Ve výchozím nastavení se nezmění skupin ochrany. Budou pokračovat v práci toofunction jako původně určené. 

Aktualizace toouse skupiny ochrany moderní úložiště záloh je volitelné. skupiny ochrany hello tooupdate, zastavte ochranu všech zdrojů dat pomocí hello zachovat data možnost. Pak přidejte hello datového zdroje tooa nové skupiny ochrany.

1. V konzole pro správu hello, vyberte hello **ochrany** funkce. V hello **člena skupiny ochrany** seznamu, klikněte pravým tlačítkem na hello člen a pak vyberte **zastavit ochranu člena**.

  ![Zastavit ochranu člena](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. V hello **odebrat ze skupiny** dialogové okno, zkontrolujte místo na disku hello používá a dostupné volné místo pro fond úložiště hello hello. Výchozí Hello je tooleave hello body obnovení na disku hello a mohly tooexpire podle jejich přidružené uchování zásad. Klikněte na **OK**.

  Pokud chcete tooimmediately návratový hello používá disku fondu úložiště volné místo toohello, vyberte hello **odstranit repliku na disku** políčko toodelete hello zálohovaná data (a bodů obnovení) přidružené tohoto člena.

  ![Odebrat ze skupiny, dialogové okno](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Vytvořte skupinu ochrany, která používá moderní úložiště záloh. Zahrnout zdroje dat hello bez ochrany.


## <a name="add-disks-tooincrease-legacy-storage"></a>Přidat disky tooincrease starší verze úložiště

Pokud chcete, aby starší verze úložiště dat toouse pomocí zálohování serveru, bude pravděpodobně nutné tooadd disky tooincrease starší verze úložiště. 

tooadd diskového úložiště:

1. V konzole pro správu hello, vyberte **správy** > **diskového úložiště** > **přidat**.

    ![Přidat dialogové okno diskového úložiště](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. V hello **přidání disku úložiště** dialogovém okně, vyberte **přidat disky**.

5. V seznamu hello dostupných disků, vyberte hello disky, které chcete tooadd, vyberte **přidat**a potom vyberte **OK**.

## <a name="update-hello-data-protection-manager-protection-agent"></a>Aktualizovat agenta ochrany hello Data Protection Manager

Zálohování serveru používá hello System Center Data Protection Manager protection agent aktualizací. Pokud provádíte upgrade agenta ochrany, který není připojený toohello sítí, nemůžete použít hello konzole pro správu Data Protection Manager toocomplete upgrade připojeného agenta. Je nutné upgradovat agenta ochrany hello v prostředí neaktivní domény. Dokud hello klientský počítač je připojený toohello síť, hello konzole pro správu Data Protection Manager ukazuje, že aktualizace agenta ochrany hello čeká na vyřízení.

Hello následující části popisují, jak tooupdate agenty ochrany pro klientské počítače, které jsou připojené a klientské počítače, které nejsou připojené.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Aktualizace agenta ochrany pro připojené klientské počítače

1. V hello konzoly správce zálohování serveru, vyberte **správy** > **agenti**.

2. V podokně zobrazení hello vyberte hello klientské počítače, pro které chcete agenta ochrany tooupdate hello.

  > [!NOTE]
  > Hello **aktualizací agenta** sloupec zobrazuje, když je k dispozici pro každý chráněný počítač aktualizace agenta ochrany. V hello **akce** podokně, hello **aktualizace** akce je dostupná, pouze když je vybrán chráněný počítač a jsou k dispozici aktualizace.
  >
  >

3. tooinstall aktualizovat agenty ochrany na počítačích hello vybrané v hello **akce** podokně, vyberte **aktualizace**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Aktualizace agenta ochrany na klientském počítači, který není připojen

1. V hello konzoly správce zálohování serveru, vyberte **správy** > **agenti**.

2. V podokně zobrazení hello vyberte hello klientské počítače, pro které chcete agenta ochrany tooupdate hello.

  > [!NOTE]
   > Hello **aktualizací agenta** sloupec zobrazuje, když je k dispozici pro každý chráněný počítač aktualizace agenta ochrany. V hello **akce** podokně, hello **aktualizace** akce není dostupná, když je vybrán chráněný počítač, pokud jsou k dispozici aktualizace.
  >
  >

3. tooinstall aktualizovat agenty ochrany na počítačích hello vybrána, vyberte **aktualizace**.

4. Pro klientský počítač, který není připojený toohello síti, dokud hello počítači je připojených toohello síť, hello **stav agenta** sloupci se zobrazuje stav **aktualizace čeká na vyřízení**.

  Až klientský počítač je připojený toohello síť, hello **aktualizací agenta** sloupci hello klientského počítače se zobrazuje stav **aktualizace**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-hello-new-version-with-azure"></a>Přesunout starší verze skupin ochrany z předchozí verzi aplikace a synchronizace hello novou verzi pomocí Azure

Jakmile serveru Azure Backup a hello operačního systému jsou obě aktualizovány, jste nové zdroje dat připravené tooprotect pomocí moderní úložiště záloh. Ale již chráněného zdroje dat bude toobe chráněné v hello starší verze způsobem, jak byly v serveru Azure Backup, ale všechny nové ochrany použije moderní úložiště záloh.

Následující kroky jsou toomigrate zdroje dat z režimu starší verze ochrany tooModern zálohování úložiště.

• Přidat hello nové svazky toohello fondu úložiště DPM a přiřadit popisné názvy a data zdroj značky v případě potřeby.
• Pro každý zdroj dat, který je v režimu starší verze, zastavte ochranu zdroje dat hello a "Uchovat chráněná Data".  To vám umožní obnovení starých bodů obnovení po migraci.

• Vytvořte nové PG a vyberte hello zdroje dat, které jsou toobe uložené pomocí nový formát.
• Aplikace DPM provede kopie repliky ze starší verze úložiště záloh hello do hello moderní úložiště zálohování svazku místně.
Poznámka: Toto se zobrazí jako • operaci po obnovení úlohy, které všechny nové synchronizace a bodů obnovení bude uložen v moderní úložiště záloh.
• Staré body obnovení se vyřazují, protože platnost a nakonec uvolněte místo na disku hello.
• Po všechny starší verze svazky hello jsou odstraněny z hello původní úložiště, hello disku lze odebrat z Azure backup a hello systému.
• Proveďte zálohování hello Azure DPMDB.

Část 2:-důležité položky > Nový server hello potřebovat toobe stejný název jako původní server Azure Backup hello. Pokud chcete toouse staré fondu úložiště a body obnovení tooretain DPMDB - musí mít zálohu databáze aplikace DPMDB je nutné obnovit toobe nelze změnit název hello hello nový server Azure backup

1) Vypnutí hello původní server Azure backup nebo trvat vypnout hello přenosu.
2) Resetovat hello účet počítače ve službě active directory.
3) Nainstalujte Server 2016 na nový počítač a název je hello stejný název počítače jako původní server Azure Backup hello.
4) Připojení k hello domény
5) Nainstalujte server Azure Backup V2 (disků fondu úložiště aplikace DPM přesunout z původní server a import)
6) Obnovení hello DPMDB prováděné od konce část 2
7) Připojte hello úložiště z hello původní backup server toohello nový server.
8) Z obnovení SQL hello DPMDB
9) Z příkazového řádku správce na disku cd tooMicrosoft nový server Azure Backup nainstalujte umístění a složky koše

Příklad cesty: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
zálohování tooAzure spusťte příkaz DPMSYNC-SYNC

10) Spusťte synchronizaci DPMSYNC-SYNC Poznámka: Pokud jste přidali nový fond úložiště DPM toohello disky místo přesunutím hello staré, spusťte příkaz DPMSYNC - Reallocatereplica

## <a name="new-powershell-cmdlets-in-v2"></a>Nové rutiny prostředí PowerShell v v2

Při instalaci serveru Azure Backup v2 jsou k dispozici dvě nové rutiny: 
* [DPMRecoveryPoint připojení](https://technet.microsoft.com/library/mt787159.aspx)
* [Odpojení DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooprepare serveru nebo začít chránit zatížení:
- [Příprava úlohy zálohování serveru](backup-azure-microsoft-azure-backup.md)
- [Pomocí zálohování serveru tooback server VMware](backup-azure-backup-server-vmware.md)
- [Použít tooback zálohování serveru SQL Server](backup-azure-sql-mabs.md)
- [Moderní úložiště zálohování pomocí zálohování serveru](backup-mabs-add-storage.md)

