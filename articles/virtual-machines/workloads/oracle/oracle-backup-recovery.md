---
title: "aaaBack nahoru a obnovit databázi Oracle 12c databáze na virtuálním počítači Azure Linux | Microsoft Docs"
description: "Zjistěte, jak tooback nahoru a obnovit databázi Oracle 12c databáze v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 5/17/2017
ms.author: rclaus
ms.openlocfilehash: 68846f4efce5eabdb71cd71772e003838154e93b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Zálohování a obnovení databáze 12c databáze Oracle na virtuálním počítači Azure Linux

Můžete použít rozhraní příkazového řádku Azure toocreate a spravovat prostředky Azure na příkazovém řádku nebo pomocí skriptů. V tomto článku používáme rozhraní příkazového řádku Azure skripty toodeploy databázi Oracle Database 12c z Galerie image Azure Marketplace.

Než začnete, ujistěte se, že je nainstalované rozhraní příkazového řádku Azure. Další informace najdete v tématu hello [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Příprava prostředí hello

### <a name="step-1-prerequisites"></a>Krok 1: požadavky

*   tooperform hello procesu zálohování a obnovení, musíte nejdřív vytvořit virtuální počítač Linux, který má nainstalovanou instanci databáze Oracle 12c. Hello Marketplace image použijete toocreate hello názvem virtuálního počítače *Oracle: Oracle – databáze-Ee:12.1.0.2:latest*.

    toolearn jak toocreate databáze Oracle, najdete v části hello [Oracle vytvořit rychlý start databáze](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-toohello-vm"></a>Krok 2: Připojení toohello virtuálních počítačů

*   toocreate relaci Secure Shell (SSH) s hello virtuálních počítačů, použijte následující příkaz hello. Nahraďte hello hello IP adresu a název hostitele hello `publicIpAddress` hodnotu pro virtuální počítač.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-hello-database"></a>Krok 3: Příprava hello databáze

1.  Tento krok předpokládá, že máte instanci Oracle (cdb1), která běží na virtuálním počítači s názvem *Můjvp*.

    Spustit hello *oracle* superuživatel kořenové a potom hello inicializovat naslouchací proces:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written too/u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting too(ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of hello LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    hello listener supports no services
    hello command completed successfully
    ```

2.  (Volitelné) Zkontrolujte, zda text hello databáze je v režimu protokolu archivu:

    ```bash
    $ sqlplus / as sysdba
    SQL> SELECT log_mode FROM v$database;

    LOG_MODE
    ------------
    NOARCHIVELOG

    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```
3.  (Volitelné) Vytvoření potvrzení změn tabulky tootest hello:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session tooscott;
    Grant succeeded.
    SQL> grant create table tooscott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```
4.  Ověřte nebo změňte hello záložní soubor umístění a velikost:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. Použijte Oracle obnovení správce (RMAN) tooback zálohu databáze hello:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>Krok 4: Zálohování konzistentní s aplikací pro virtuální počítače s Linuxem

Zálohování konzistentní s aplikací je nová funkce v Azure Backup. Můžete vytvořit a vyberte tooexecute skriptů před a po hello snímku virtuálního počítače (snímek před a po pořízení snímku).

1. Stažení souboru JSON hello.

    Stáhněte si VMSnapshotScriptPluginConfig.json z https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig. obsah souboru Hello vypadat podobně jako toohello následující:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. Vytvořte složku /etc/azure hello na hello virtuálních počítačů:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. Zkopírujte soubor JSON hello.

    Zkopírujte složku /etc/azure toohello VMSnapshotScriptPluginConfig.json.

4. Upravte soubor JSON hello.

    Upravit hello VMSnapshotScriptPluginConfig.json souboru tooinclude hello `PreScriptLocation` a `PostScriptlocation` parametry. Například:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Vytvořte hello soubory skriptu snímek před a po vytvoření snímku.

    Tady je příklad snímku před a po pořízení snímku skripty pro "cold zálohování" (zálohu do offline režimu, vypnutí a restartování):

    Pro /etc/azure/pre_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    Pro /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    Tady je příklad snímku před a po pořízení snímku skripty pro "za zálohování" (online zálohování):

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    Pro /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    /Etc/azure/pre_script.sql upravte obsah hello hello soubor pro vaše požadavky:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    /Etc/azure/post_script.sql upravte obsah hello hello soubor pro vaše požadavky:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Změna oprávnění k souboru:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Testování skriptů hello.

    tootest hello skripty, nejprve, přihlaste se jako kořenového adresáře. Potom zkontrolujte, že nejsou žádné chyby:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

Další informace najdete v tématu [zálohování konzistentní s aplikací pro virtuální počítače s Linuxem](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-tooback-up-hello-vm"></a>Krok 5: Trezory služeb zotavení Azure pomocí tooback až hello virtuálních počítačů

1.  V hello portálu Azure, vyhledejte **trezory služeb zotavení**.

    ![Stránka trezory služby zotavení](./media/oracle-backup-recovery/recovery_service_01.png)

2.  Na hello **trezory služeb zotavení** , tooadd nové úložiště, klikněte na **přidat**.

    ![Trezory služeb zotavení přidat stránku](./media/oracle-backup-recovery/recovery_service_02.png)

3.  toocontinue, klikněte na tlačítko **myVault**.

    ![Stránku podrobností trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_03.png)

4.  Na hello **myVault** okně klikněte na tlačítko **zálohování**.

    ![Trezory služeb zotavení zálohování stránky](./media/oracle-backup-recovery/recovery_service_04.png)

5.  Na hello **cíl zálohování** okně použití hello výchozí hodnoty **Azure** a **virtuální počítač**. Klikněte na **OK**.

    ![Stránku podrobností trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_05.png)

6.  Pro **zálohování zásad**, použijte **DefaultPolicy**, nebo vyberte **vytvořit novou zásadu**. Klikněte na **OK**.

    ![Stránka podrobnosti zásady zálohování trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_06.png)

7.  Na hello **vybrat virtuální počítače** okně, vyberte hello **myVM1** zaškrtněte políčko a potom klikněte na **OK**. Klikněte na tlačítko hello **povolit zálohování** tlačítko.

    ![Obnovení služby trezory položky toohello zálohování podrobností stránky](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > Po kliknutí na tlačítko **povolit zálohování**, proces zálohování hello nespustí do hello naplánovanou dobu vypršení platnosti. tooset si zálohu okamžitě, dokončení hello další krok.

8.  Na hello **myVault - zálohování položek** okno, v části **zálohování počet položek**, vyberte počet zálohování položek hello.

    ![Stránka Podrobnosti o obnovení služby trezory myVault](./media/oracle-backup-recovery/recovery_service_08.png)

9.  Na hello **zálohování položek (virtuální počítač Azure)** okno na pravé straně hello hello stránky, klikněte na nabídku s výpustkou hello (**...** ) tlačítko a potom klikněte na **zálohovat nyní**.

    ![Zálohování teď příkaz trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_09.png)

10. Klikněte na tlačítko hello **zálohování** tlačítko. Počkejte toofinish hello proces zálohování. Pak přejděte příliš[krok 6: odebrat soubory databáze hello](#step-6-remove-the-database-files).

    Stav hello tooview hello úloha zálohování, klikněte na tlačítko **úlohy**.

    ![Trezory služeb zotavení úlohy stránky](./media/oracle-backup-recovery/recovery_service_10.png)

    Stav Hello hello úloha zálohování se zobrazí v hello následující bitové kopie:

    ![Stránka se stavem úlohy trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_11.png)

11. Zálohování konzistentní s aplikací vyřešte všechny chyby v souboru protokolu hello. soubor protokolu Hello se nachází v /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.

### <a name="step-6-remove-hello-database-files"></a>Krok 6: Odebrat soubory databáze hello 
Později v tomto článku se dozvíte, jak tootest hello proces obnovení. Chcete-li otestovat proces obnovení hello, máte tooremove hello databázové soubory.

1.  Odebrání souborů hello tabulkového prostoru a zálohování:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (Volitelné) Vypněte instanci Oracle hello:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-hello-deleted-files-from-hello-recovery-services-vaults"></a>Obnovit soubory hello odstraněn z hello trezory služeb zotavení
toorestore hello odstranit soubory, dokončení hello následující kroky:

1. V hello portálu Azure, vyhledejte hello *myVault* položky trezory služeb zotavení. Na hello **přehled** okno, v části **zálohování položek**, vyberte hello počet položek.

    ![Obnovení služby trezory myVault zálohování položek](./media/oracle-backup-recovery/recovery_service_12.png)

2. V části **počet položek zálohování**, vyberte hello počet položek.

    ![Počet položek. zálohování virtuálního počítače Azure trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_13.png)

3. Na hello **myvm1** okně klikněte na tlačítko **obnovení souboru (Preview)**.

    ![Snímek obrazovky hello služeb zotavení trezory stránka obnovení souboru](./media/oracle-backup-recovery/recovery_service_14.png)

4. Na hello **obnovení souboru (Preview)** podokně klikněte na tlačítko **stáhnout skript**. Potom uložte hello stahování (.sh) soubor tooa složku na klientském počítači hello.

    ![Stáhnout možnosti uloží soubor skriptu](./media/oracle-backup-recovery/recovery_service_15.png)

5. Zkopírujte hello .sh souboru toohello virtuálních počítačů.

    Hello následující příklad ukazuje, jak toouse zabezpečené kopírování (scp) příkaz toomove hello toohello soubor virtuálního počítače. Do nového souboru, který je nastavený na hello virtuálních počítačů můžete zkopírovat také hello obsah toohello schránky a vložte obsah hello.

    > [!IMPORTANT]
    > V následujícím příkladu hello Ujistěte se, aktualizovat hello IP adresu a složku hodnoty. Hello hodnoty musí být mapována toohello složku, kde je uložený soubor hello.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Změňte hello souboru tak, aby vlastníkem je kořenový hello.

    V následujícím příkladu hello změňte hello souboru tak, aby vlastníkem je kořenový hello. Potom změňte oprávnění.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    Hello následující příklad ukazuje, co byste měli vidět po spuštění hello předcházející skriptu. Když se zobrazí výzva toocontinue, zadejte **Y**.

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    hello script requires 'open-iscsi' and 'lshw' toorun.
    Do you want us tooinstall 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' toocontinue with installation, 'N' tooabort hello operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting toorecovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of hello recovery point toothis machine...

    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

7. Přístup k toohello připojené svazky je potvrzen.

    tooexit, zadejte **q**a poté vyhledejte hello připojené svazky. seznam hello Přidat svazky, na příkazovém řádku, zadejte toocreate **df – KB**.

    ![příkaz -k df Hello](./media/oracle-backup-recovery/recovery_service_16.png)

8. Použití hello následující skript toocopy hello chybějící soubory a složky back toohello:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```
9. V následující skript hello použijte RMAN toorecover hello databáze:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. Odpojte hello disk.

    V portálu Azure, na hello hello **obnovení souboru (Preview)** okně klikněte na tlačítko **odpojit disky**.

    ![Odpojení příkaz disky](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-hello-entire-vm"></a>Obnovení hello celý virtuální počítač

Místo hello odstranit soubory obnovení ze hello trezory služeb zotavení, můžete obnovit hello celý virtuální počítač.

### <a name="step-1-delete-myvm"></a>Krok 1: Odstranění Můjvp

*   V hello portálu Azure, přejděte toohello **myVM1** trezoru a potom vyberte **odstranit**.

    ![Příkaz delete trezoru](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-hello-vm"></a>Krok 2: Obnovení hello virtuálních počítačů

1.  Přejděte příliš**trezory služeb zotavení**a potom vyberte **myVault**.

    ![Položka myVault](./media/oracle-backup-recovery/recover_vm_02.png)

2.  Na hello **přehled** okno, v části **zálohování položek**, vyberte hello počet položek.

    ![myVault zálohování položek](./media/oracle-backup-recovery/recover_vm_03.png)

3.  Na hello **zálohování položek (virtuální počítač Azure)** vyberte **myvm1**.

    ![Stránku obnovení virtuálního počítače](./media/oracle-backup-recovery/recover_vm_04.png)

4.  Na hello **myvm1** okně klikněte na tlačítko se třemi tečkami hello (**...** ) tlačítko a potom klikněte na **obnovit virtuální počítač**.

    ![Virtuální počítač příkazu Obnovit](./media/oracle-backup-recovery/recover_vm_05.png)

5.  Na hello **vyberte bod obnovení** okně, vyberte hello položka má toorestore a pak klikněte na tlačítko **OK**.

    ![Bod obnovení vyberte hello](./media/oracle-backup-recovery/recover_vm_06.png)

    Pokud jste povolili zálohování konzistentní s aplikací, se zobrazí modré svislá čára.

6.  Na hello **obnovit konfiguraci** okně vyberte hello název virtuálního počítače, vyberte skupinu prostředků hello a pak klikněte na tlačítko **OK**.

    ![Obnovení hodnoty konfigurace](./media/oracle-backup-recovery/recover_vm_07.png)

7.  toorestore hello virtuální počítač, klikněte na tlačítko hello **obnovení** tlačítko.

8.  Stav hello tooview hello procesu obnovení, klikněte na tlačítko **úlohy**a potom klikněte na **úlohy zálohování**.

    ![Příkaz Stav úlohy zálohování](./media/oracle-backup-recovery/recover_vm_08.png)

    Hello následující obrázek znázorňuje hello stav procesu obnovení hello:

    ![Stav procesu obnovení hello](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-hello-public-ip-address"></a>Krok 3: Nastavte hello veřejnou IP adresu
Po dokončení hello je obnovit virtuální počítač nastavte hello veřejnou IP adresu.

1.  Hello vyhledávacího pole zadejte **veřejnou IP adresu**.

    ![Seznam veřejné IP adresy](./media/oracle-backup-recovery/create_ip_00.png)

2.  Na hello **veřejné IP adresy** okně klikněte na tlačítko **přidat**. Na hello **vytvoření veřejné IP adresy** okně pro **název**, vyberte název veřejné IP adresy hello. V části **Skupina prostředků** vyberte **Použít existující**. Poté klikněte na možnost **Vytvořit**.

    ![Vytvoření IP adresy](./media/oracle-backup-recovery/create_ip_01.png)

3.  hledání tooassociate hello veřejnou IP adresu s hello síťové rozhraní pro hello virtuálních počítačů a vyberte možnost **myVMip**. Potom klikněte na **přidružit**.

    ![Přiřadit IP adresu](./media/oracle-backup-recovery/create_ip_02.png)

4.  Pro **typ prostředku**, vyberte **síťové rozhraní**. Vyberte hello síťové rozhraní, které používá hello Můjvp instance a pak klikněte na tlačítko **OK**.

    ![Vyberte typ prostředku a hodnoty síťový adaptér](./media/oracle-backup-recovery/create_ip_03.png)

5.  Vyhledejte a otevřete hello instanci Můjvp, který je přenést z portálu hello. Hello IP adresu, která souvisí s hello virtuálního počítače se zobrazí na hello Můjvp **přehled** okno.

    ![IP adresa hodnotu](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-toohello-vm"></a>Krok 4: Připojení toohello virtuálních počítačů

*   tooconnect toohello virtuální počítač, použijte hello následující skript:

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-hello-database-is-accessible"></a>Krok 5: Test, zda text hello databáze je přístupná
*   usnadnění tootest, použijte hello následující skript:

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Pokud hello databáze **spuštění** příkaz generuje chybě, toorecover hello databáze najdete v tématu [krok 6: použití RMAN toorecover hello databáze](#step-6-optional-use-rman-to-recover-the-database).

### <a name="step-6-optional-use-rman-toorecover-hello-database"></a>Krok 6: (Volitelné) použijte RMAN toorecover hello databáze
*   toorecover hello databáze, použijte hello následující skript:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

Hello zálohování a obnovení databáze 12c hello databáze Oracle na virtuální počítač Azure Linux je nyní dokončena.

## <a name="delete-hello-vm"></a>Odstranit hello virtuálních počítačů

Když už nebude potřebovat hello virtuálních počítačů, můžete použít následující skupiny prostředků hello tooremove příkaz hello virtuálních počítačů a všechny související prostředky hello:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz: Vytvoření vysoce dostupné virtuální počítače](../../linux/create-cli-complete.md)

[Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů](../../linux/cli-samples.md)



