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
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="30fdd-103">Zálohování a obnovení databáze 12c databáze Oracle na virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="30fdd-103">Back up and recover an Oracle Database 12c database on an Azure Linux virtual machine</span></span>

<span data-ttu-id="30fdd-104">Můžete použít rozhraní příkazového řádku Azure toocreate a spravovat prostředky Azure na příkazovém řádku nebo pomocí skriptů.</span><span class="sxs-lookup"><span data-stu-id="30fdd-104">You can use Azure CLI toocreate and manage Azure resources at a command prompt, or use scripts.</span></span> <span data-ttu-id="30fdd-105">V tomto článku používáme rozhraní příkazového řádku Azure skripty toodeploy databázi Oracle Database 12c z Galerie image Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="30fdd-105">In this article, we use Azure CLI scripts toodeploy an Oracle Database 12c database from an Azure Marketplace gallery image.</span></span>

<span data-ttu-id="30fdd-106">Než začnete, ujistěte se, že je nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="30fdd-106">Before you begin, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="30fdd-107">Další informace najdete v tématu hello [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="30fdd-107">For more information, see hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="30fdd-108">Příprava prostředí hello</span><span class="sxs-lookup"><span data-stu-id="30fdd-108">Prepare hello environment</span></span>

### <a name="step-1-prerequisites"></a><span data-ttu-id="30fdd-109">Krok 1: požadavky</span><span class="sxs-lookup"><span data-stu-id="30fdd-109">Step 1: Prerequisites</span></span>

*   <span data-ttu-id="30fdd-110">tooperform hello procesu zálohování a obnovení, musíte nejdřív vytvořit virtuální počítač Linux, který má nainstalovanou instanci databáze Oracle 12c.</span><span class="sxs-lookup"><span data-stu-id="30fdd-110">tooperform hello backup and recovery process, you must first create a Linux VM that has an installed instance of Oracle Database 12c.</span></span> <span data-ttu-id="30fdd-111">Hello Marketplace image použijete toocreate hello názvem virtuálního počítače *Oracle: Oracle – databáze-Ee:12.1.0.2:latest*.</span><span class="sxs-lookup"><span data-stu-id="30fdd-111">hello Marketplace image you use toocreate hello VM is named *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.</span></span>

    <span data-ttu-id="30fdd-112">toolearn jak toocreate databáze Oracle, najdete v části hello [Oracle vytvořit rychlý start databáze](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span><span class="sxs-lookup"><span data-stu-id="30fdd-112">toolearn how toocreate an Oracle database, see hello [Oracle create database quickstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span></span>


### <a name="step-2-connect-toohello-vm"></a><span data-ttu-id="30fdd-113">Krok 2: Připojení toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30fdd-113">Step 2: Connect toohello VM</span></span>

*   <span data-ttu-id="30fdd-114">toocreate relaci Secure Shell (SSH) s hello virtuálních počítačů, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-114">toocreate a Secure Shell (SSH) session with hello VM, use hello following command.</span></span> <span data-ttu-id="30fdd-115">Nahraďte hello hello IP adresu a název hostitele hello `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="30fdd-115">Replace hello IP address and hello host name combination with hello `publicIpAddress` value for your VM.</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-hello-database"></a><span data-ttu-id="30fdd-116">Krok 3: Příprava hello databáze</span><span class="sxs-lookup"><span data-stu-id="30fdd-116">Step 3: Prepare hello database</span></span>

1.  <span data-ttu-id="30fdd-117">Tento krok předpokládá, že máte instanci Oracle (cdb1), která běží na virtuálním počítači s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="30fdd-117">This step assumes that you have an Oracle instance (cdb1) that is running on a VM named *myVM*.</span></span>

    <span data-ttu-id="30fdd-118">Spustit hello *oracle* superuživatel kořenové a potom hello inicializovat naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="30fdd-118">Run hello *oracle* superuser root, and then initialize hello listener:</span></span>

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

2.  <span data-ttu-id="30fdd-119">(Volitelné) Zkontrolujte, zda text hello databáze je v režimu protokolu archivu:</span><span class="sxs-lookup"><span data-stu-id="30fdd-119">(Optional) Make sure hello database is in archive log mode:</span></span>

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
3.  <span data-ttu-id="30fdd-120">(Volitelné) Vytvoření potvrzení změn tabulky tootest hello:</span><span class="sxs-lookup"><span data-stu-id="30fdd-120">(Optional) Create a table tootest hello commit:</span></span>

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
4.  <span data-ttu-id="30fdd-121">Ověřte nebo změňte hello záložní soubor umístění a velikost:</span><span class="sxs-lookup"><span data-stu-id="30fdd-121">Verify or change hello backup file location and size:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. <span data-ttu-id="30fdd-122">Použijte Oracle obnovení správce (RMAN) tooback zálohu databáze hello:</span><span class="sxs-lookup"><span data-stu-id="30fdd-122">Use Oracle Recovery Manager (RMAN) tooback up hello database:</span></span>

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a><span data-ttu-id="30fdd-123">Krok 4: Zálohování konzistentní s aplikací pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="30fdd-123">Step 4: Application-consistent backup for Linux VMs</span></span>

<span data-ttu-id="30fdd-124">Zálohování konzistentní s aplikací je nová funkce v Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="30fdd-124">Application-consistent backups is a new feature in Azure Backup.</span></span> <span data-ttu-id="30fdd-125">Můžete vytvořit a vyberte tooexecute skriptů před a po hello snímku virtuálního počítače (snímek před a po pořízení snímku).</span><span class="sxs-lookup"><span data-stu-id="30fdd-125">You can create and select scripts tooexecute before and after hello VM snapshot (pre-snapshot and post-snapshot).</span></span>

1. <span data-ttu-id="30fdd-126">Stažení souboru JSON hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-126">Download hello JSON file.</span></span>

    <span data-ttu-id="30fdd-127">Stáhněte si VMSnapshotScriptPluginConfig.json z https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span><span class="sxs-lookup"><span data-stu-id="30fdd-127">Download VMSnapshotScriptPluginConfig.json from https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span></span> <span data-ttu-id="30fdd-128">obsah souboru Hello vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="30fdd-128">hello file contents look similar toohello following:</span></span>

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

2. <span data-ttu-id="30fdd-129">Vytvořte složku /etc/azure hello na hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="30fdd-129">Create hello /etc/azure folder on hello VM:</span></span>

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. <span data-ttu-id="30fdd-130">Zkopírujte soubor JSON hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-130">Copy hello JSON file.</span></span>

    <span data-ttu-id="30fdd-131">Zkopírujte složku /etc/azure toohello VMSnapshotScriptPluginConfig.json.</span><span class="sxs-lookup"><span data-stu-id="30fdd-131">Copy VMSnapshotScriptPluginConfig.json toohello /etc/azure folder.</span></span>

4. <span data-ttu-id="30fdd-132">Upravte soubor JSON hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-132">Edit hello JSON file.</span></span>

    <span data-ttu-id="30fdd-133">Upravit hello VMSnapshotScriptPluginConfig.json souboru tooinclude hello `PreScriptLocation` a `PostScriptlocation` parametry.</span><span class="sxs-lookup"><span data-stu-id="30fdd-133">Edit hello VMSnapshotScriptPluginConfig.json file tooinclude hello `PreScriptLocation` and `PostScriptlocation` parameters.</span></span> <span data-ttu-id="30fdd-134">Například:</span><span class="sxs-lookup"><span data-stu-id="30fdd-134">For example:</span></span>

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

5. <span data-ttu-id="30fdd-135">Vytvořte hello soubory skriptu snímek před a po vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="30fdd-135">Create hello pre-snapshot and post-snapshot script files.</span></span>

    <span data-ttu-id="30fdd-136">Tady je příklad snímku před a po pořízení snímku skripty pro "cold zálohování" (zálohu do offline režimu, vypnutí a restartování):</span><span class="sxs-lookup"><span data-stu-id="30fdd-136">Here's an example of pre-snapshot and post-snapshot scripts for a "cold backup" (an offline backup, with shutdown and restart):</span></span>

    <span data-ttu-id="30fdd-137">Pro /etc/azure/pre_script.sh:</span><span class="sxs-lookup"><span data-stu-id="30fdd-137">For /etc/azure/pre_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="30fdd-138">Pro /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="30fdd-138">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    <span data-ttu-id="30fdd-139">Tady je příklad snímku před a po pořízení snímku skripty pro "za zálohování" (online zálohování):</span><span class="sxs-lookup"><span data-stu-id="30fdd-139">Here's an example of pre-snapshot and post-snapshot scripts for a "hot backup" (an online backup):</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="30fdd-140">Pro /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="30fdd-140">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="30fdd-141">/Etc/azure/pre_script.sql upravte obsah hello hello soubor pro vaše požadavky:</span><span class="sxs-lookup"><span data-stu-id="30fdd-141">For /etc/azure/pre_script.sql, modify hello contents of hello file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    <span data-ttu-id="30fdd-142">/Etc/azure/post_script.sql upravte obsah hello hello soubor pro vaše požadavky:</span><span class="sxs-lookup"><span data-stu-id="30fdd-142">For /etc/azure/post_script.sql, modify hello contents of hello file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. <span data-ttu-id="30fdd-143">Změna oprávnění k souboru:</span><span class="sxs-lookup"><span data-stu-id="30fdd-143">Change file permissions:</span></span>

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. <span data-ttu-id="30fdd-144">Testování skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-144">Test hello scripts.</span></span>

    <span data-ttu-id="30fdd-145">tootest hello skripty, nejprve, přihlaste se jako kořenového adresáře.</span><span class="sxs-lookup"><span data-stu-id="30fdd-145">tootest hello scripts, first, sign in as root.</span></span> <span data-ttu-id="30fdd-146">Potom zkontrolujte, že nejsou žádné chyby:</span><span class="sxs-lookup"><span data-stu-id="30fdd-146">Then, ensure that there are no errors:</span></span>

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

<span data-ttu-id="30fdd-147">Další informace najdete v tématu [zálohování konzistentní s aplikací pro virtuální počítače s Linuxem](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span><span class="sxs-lookup"><span data-stu-id="30fdd-147">For more information, see [Application-consistent backup for Linux VMs](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span></span>


### <a name="step-5-use-azure-recovery-services-vaults-tooback-up-hello-vm"></a><span data-ttu-id="30fdd-148">Krok 5: Trezory služeb zotavení Azure pomocí tooback až hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30fdd-148">Step 5: Use Azure Recovery Services vaults tooback up hello VM</span></span>

1.  <span data-ttu-id="30fdd-149">V hello portálu Azure, vyhledejte **trezory služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-149">In hello Azure portal, search for **Recovery Services vaults**.</span></span>

    ![Stránka trezory služby zotavení](./media/oracle-backup-recovery/recovery_service_01.png)

2.  <span data-ttu-id="30fdd-151">Na hello **trezory služeb zotavení** , tooadd nové úložiště, klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-151">On hello **Recovery Services vaults** blade, tooadd a new vault, click **Add**.</span></span>

    ![Trezory služeb zotavení přidat stránku](./media/oracle-backup-recovery/recovery_service_02.png)

3.  <span data-ttu-id="30fdd-153">toocontinue, klikněte na tlačítko **myVault**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-153">toocontinue, click **myVault**.</span></span>

    ![Stránku podrobností trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_03.png)

4.  <span data-ttu-id="30fdd-155">Na hello **myVault** okně klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-155">On hello **myVault** blade, click **Backup**.</span></span>

    ![Trezory služeb zotavení zálohování stránky](./media/oracle-backup-recovery/recovery_service_04.png)

5.  <span data-ttu-id="30fdd-157">Na hello **cíl zálohování** okně použití hello výchozí hodnoty **Azure** a **virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-157">On hello **Backup Goal** blade, use hello default values of **Azure** and **Virtual machine**.</span></span> <span data-ttu-id="30fdd-158">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-158">Click **OK**.</span></span>

    ![Stránku podrobností trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_05.png)

6.  <span data-ttu-id="30fdd-160">Pro **zálohování zásad**, použijte **DefaultPolicy**, nebo vyberte **vytvořit novou zásadu**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-160">For **Backup policy**, use **DefaultPolicy**, or select **Create New policy**.</span></span> <span data-ttu-id="30fdd-161">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-161">Click **OK**.</span></span>

    ![Stránka podrobnosti zásady zálohování trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_06.png)

7.  <span data-ttu-id="30fdd-163">Na hello **vybrat virtuální počítače** okně, vyberte hello **myVM1** zaškrtněte políčko a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-163">On hello **Select virtual machines** blade, select hello **myVM1** check box, and then click **OK**.</span></span> <span data-ttu-id="30fdd-164">Klikněte na tlačítko hello **povolit zálohování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30fdd-164">Click hello **Enable backup** button.</span></span>

    ![Obnovení služby trezory položky toohello zálohování podrobností stránky](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > <span data-ttu-id="30fdd-166">Po kliknutí na tlačítko **povolit zálohování**, proces zálohování hello nespustí do hello naplánovanou dobu vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="30fdd-166">After you click **Enable backup**, hello backup process doesn't start until hello scheduled time expires.</span></span> <span data-ttu-id="30fdd-167">tooset si zálohu okamžitě, dokončení hello další krok.</span><span class="sxs-lookup"><span data-stu-id="30fdd-167">tooset up an immediate backup, complete hello next step.</span></span>

8.  <span data-ttu-id="30fdd-168">Na hello **myVault - zálohování položek** okno, v části **zálohování počet položek**, vyberte počet zálohování položek hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-168">On hello **myVault - Backup items** blade, under **BACKUP ITEM COUNT**, select hello backup item count.</span></span>

    ![Stránka Podrobnosti o obnovení služby trezory myVault](./media/oracle-backup-recovery/recovery_service_08.png)

9.  <span data-ttu-id="30fdd-170">Na hello **zálohování položek (virtuální počítač Azure)** okno na pravé straně hello hello stránky, klikněte na nabídku s výpustkou hello (**...** ) tlačítko a potom klikněte na **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-170">On hello **Backup Items (Azure Virtual Machine)** blade, on hello right side of hello page, click hello ellipsis (**...**) button, and then click **Backup now**.</span></span>

    ![Zálohování teď příkaz trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_09.png)

10. <span data-ttu-id="30fdd-172">Klikněte na tlačítko hello **zálohování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30fdd-172">Click hello **Backup** button.</span></span> <span data-ttu-id="30fdd-173">Počkejte toofinish hello proces zálohování.</span><span class="sxs-lookup"><span data-stu-id="30fdd-173">Wait for hello backup process toofinish.</span></span> <span data-ttu-id="30fdd-174">Pak přejděte příliš[krok 6: odebrat soubory databáze hello](#step-6-remove-the-database-files).</span><span class="sxs-lookup"><span data-stu-id="30fdd-174">Then, go too[Step 6: Remove hello database files](#step-6-remove-the-database-files).</span></span>

    <span data-ttu-id="30fdd-175">Stav hello tooview hello úloha zálohování, klikněte na tlačítko **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-175">tooview hello status of hello backup job, click **Jobs**.</span></span>

    ![Trezory služeb zotavení úlohy stránky](./media/oracle-backup-recovery/recovery_service_10.png)

    <span data-ttu-id="30fdd-177">Stav Hello hello úloha zálohování se zobrazí v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="30fdd-177">hello status of hello backup job appears in hello following image:</span></span>

    ![Stránka se stavem úlohy trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_11.png)

11. <span data-ttu-id="30fdd-179">Zálohování konzistentní s aplikací vyřešte všechny chyby v souboru protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-179">For an application-consistent backup, address any errors in hello log file.</span></span> <span data-ttu-id="30fdd-180">soubor protokolu Hello se nachází v /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span><span class="sxs-lookup"><span data-stu-id="30fdd-180">hello log file is located at /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span></span>

### <a name="step-6-remove-hello-database-files"></a><span data-ttu-id="30fdd-181">Krok 6: Odebrat soubory databáze hello</span><span class="sxs-lookup"><span data-stu-id="30fdd-181">Step 6: Remove hello database files</span></span> 
<span data-ttu-id="30fdd-182">Později v tomto článku se dozvíte, jak tootest hello proces obnovení.</span><span class="sxs-lookup"><span data-stu-id="30fdd-182">Later in this article, you'll learn how tootest hello recovery process.</span></span> <span data-ttu-id="30fdd-183">Chcete-li otestovat proces obnovení hello, máte tooremove hello databázové soubory.</span><span class="sxs-lookup"><span data-stu-id="30fdd-183">Before you can test hello recovery process, you have tooremove hello database files.</span></span>

1.  <span data-ttu-id="30fdd-184">Odebrání souborů hello tabulkového prostoru a zálohování:</span><span class="sxs-lookup"><span data-stu-id="30fdd-184">Remove hello tablespace and backup files:</span></span>

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  <span data-ttu-id="30fdd-185">(Volitelné) Vypněte instanci Oracle hello:</span><span class="sxs-lookup"><span data-stu-id="30fdd-185">(Optional) Shut down hello Oracle instance:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-hello-deleted-files-from-hello-recovery-services-vaults"></a><span data-ttu-id="30fdd-186">Obnovit soubory hello odstraněn z hello trezory služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="30fdd-186">Restore hello deleted files from hello Recovery Services vaults</span></span>
<span data-ttu-id="30fdd-187">toorestore hello odstranit soubory, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="30fdd-187">toorestore hello deleted files, complete hello following steps:</span></span>

1. <span data-ttu-id="30fdd-188">V hello portálu Azure, vyhledejte hello *myVault* položky trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="30fdd-188">In hello Azure portal, search for hello *myVault* Recovery Services vaults item.</span></span> <span data-ttu-id="30fdd-189">Na hello **přehled** okno, v části **zálohování položek**, vyberte hello počet položek.</span><span class="sxs-lookup"><span data-stu-id="30fdd-189">On hello **Overview** blade, under **Backup items**, select hello number of items.</span></span>

    ![Obnovení služby trezory myVault zálohování položek](./media/oracle-backup-recovery/recovery_service_12.png)

2. <span data-ttu-id="30fdd-191">V části **počet položek zálohování**, vyberte hello počet položek.</span><span class="sxs-lookup"><span data-stu-id="30fdd-191">Under **BACKUP ITEM COUNT**, select hello number of items.</span></span>

    ![Počet položek. zálohování virtuálního počítače Azure trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_13.png)

3. <span data-ttu-id="30fdd-193">Na hello **myvm1** okně klikněte na tlačítko **obnovení souboru (Preview)**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-193">On hello **myvm1** blade, click **File Recovery (Preview)**.</span></span>

    ![Snímek obrazovky hello služeb zotavení trezory stránka obnovení souboru](./media/oracle-backup-recovery/recovery_service_14.png)

4. <span data-ttu-id="30fdd-195">Na hello **obnovení souboru (Preview)** podokně klikněte na tlačítko **stáhnout skript**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-195">On hello **File Recovery (Preview)** pane, click **Download Script**.</span></span> <span data-ttu-id="30fdd-196">Potom uložte hello stahování (.sh) soubor tooa složku na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-196">Then, save hello download (.sh) file tooa folder on hello client computer.</span></span>

    ![Stáhnout možnosti uloží soubor skriptu](./media/oracle-backup-recovery/recovery_service_15.png)

5. <span data-ttu-id="30fdd-198">Zkopírujte hello .sh souboru toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="30fdd-198">Copy hello .sh file toohello VM.</span></span>

    <span data-ttu-id="30fdd-199">Hello následující příklad ukazuje, jak toouse zabezpečené kopírování (scp) příkaz toomove hello toohello soubor virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="30fdd-199">hello following example shows how you toouse a secure copy (scp) command toomove hello file toohello VM.</span></span> <span data-ttu-id="30fdd-200">Do nového souboru, který je nastavený na hello virtuálních počítačů můžete zkopírovat také hello obsah toohello schránky a vložte obsah hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-200">You also can copy hello contents toohello clipboard, and then paste hello contents in a new file that is set up on hello VM.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="30fdd-201">V následujícím příkladu hello Ujistěte se, aktualizovat hello IP adresu a složku hodnoty.</span><span class="sxs-lookup"><span data-stu-id="30fdd-201">In hello following example, ensure that you update hello IP address and folder values.</span></span> <span data-ttu-id="30fdd-202">Hello hodnoty musí být mapována toohello složku, kde je uložený soubor hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-202">hello values must map toohello folder where hello file is saved.</span></span>

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. <span data-ttu-id="30fdd-203">Změňte hello souboru tak, aby vlastníkem je kořenový hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-203">Change hello file, so that it's owned by hello root.</span></span>

    <span data-ttu-id="30fdd-204">V následujícím příkladu hello změňte hello souboru tak, aby vlastníkem je kořenový hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-204">In hello following example, change hello file so that it's owned by hello root.</span></span> <span data-ttu-id="30fdd-205">Potom změňte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="30fdd-205">Then, change permissions.</span></span>

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    <span data-ttu-id="30fdd-206">Hello následující příklad ukazuje, co byste měli vidět po spuštění hello předcházející skriptu.</span><span class="sxs-lookup"><span data-stu-id="30fdd-206">hello following example shows what you should see after you run hello preceding script.</span></span> <span data-ttu-id="30fdd-207">Když se zobrazí výzva toocontinue, zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-207">When you're prompted toocontinue, enter **Y**.</span></span>

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

7. <span data-ttu-id="30fdd-208">Přístup k toohello připojené svazky je potvrzen.</span><span class="sxs-lookup"><span data-stu-id="30fdd-208">Access toohello mounted volumes is confirmed.</span></span>

    <span data-ttu-id="30fdd-209">tooexit, zadejte **q**a poté vyhledejte hello připojené svazky.</span><span class="sxs-lookup"><span data-stu-id="30fdd-209">tooexit, enter **q**, and then search for hello mounted volumes.</span></span> <span data-ttu-id="30fdd-210">seznam hello Přidat svazky, na příkazovém řádku, zadejte toocreate **df – KB**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-210">toocreate a list of hello added volumes, at a command prompt, enter **df -k**.</span></span>

    ![příkaz -k df Hello](./media/oracle-backup-recovery/recovery_service_16.png)

8. <span data-ttu-id="30fdd-212">Použití hello následující skript toocopy hello chybějící soubory a složky back toohello:</span><span class="sxs-lookup"><span data-stu-id="30fdd-212">Use hello following script toocopy hello missing files back toohello folders:</span></span>

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
9. <span data-ttu-id="30fdd-213">V následující skript hello použijte RMAN toorecover hello databáze:</span><span class="sxs-lookup"><span data-stu-id="30fdd-213">In hello following script, use RMAN toorecover hello database:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. <span data-ttu-id="30fdd-214">Odpojte hello disk.</span><span class="sxs-lookup"><span data-stu-id="30fdd-214">Unmount hello disk.</span></span>

    <span data-ttu-id="30fdd-215">V portálu Azure, na hello hello **obnovení souboru (Preview)** okně klikněte na tlačítko **odpojit disky**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-215">In hello Azure portal, on hello **File Recovery (Preview)** blade, click **Unmount Disks**.</span></span>

    ![Odpojení příkaz disky](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-hello-entire-vm"></a><span data-ttu-id="30fdd-217">Obnovení hello celý virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="30fdd-217">Restore hello entire VM</span></span>

<span data-ttu-id="30fdd-218">Místo hello odstranit soubory obnovení ze hello trezory služeb zotavení, můžete obnovit hello celý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="30fdd-218">Instead of restoring hello deleted files from hello Recovery Services vaults, you can restore hello entire VM.</span></span>

### <a name="step-1-delete-myvm"></a><span data-ttu-id="30fdd-219">Krok 1: Odstranění Můjvp</span><span class="sxs-lookup"><span data-stu-id="30fdd-219">Step 1: Delete myVM</span></span>

*   <span data-ttu-id="30fdd-220">V hello portálu Azure, přejděte toohello **myVM1** trezoru a potom vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-220">In hello Azure portal, go toohello **myVM1** vault, and then select **Delete**.</span></span>

    ![Příkaz delete trezoru](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-hello-vm"></a><span data-ttu-id="30fdd-222">Krok 2: Obnovení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30fdd-222">Step 2: Recover hello VM</span></span>

1.  <span data-ttu-id="30fdd-223">Přejděte příliš**trezory služeb zotavení**a potom vyberte **myVault**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-223">Go too**Recovery Services vaults**, and then select **myVault**.</span></span>

    ![Položka myVault](./media/oracle-backup-recovery/recover_vm_02.png)

2.  <span data-ttu-id="30fdd-225">Na hello **přehled** okno, v části **zálohování položek**, vyberte hello počet položek.</span><span class="sxs-lookup"><span data-stu-id="30fdd-225">On hello **Overview** blade, under **Backup items**, select hello number of items.</span></span>

    ![myVault zálohování položek](./media/oracle-backup-recovery/recover_vm_03.png)

3.  <span data-ttu-id="30fdd-227">Na hello **zálohování položek (virtuální počítač Azure)** vyberte **myvm1**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-227">On hello **Backup Items (Azure Virtual Machine)** blade, select **myvm1**.</span></span>

    ![Stránku obnovení virtuálního počítače](./media/oracle-backup-recovery/recover_vm_04.png)

4.  <span data-ttu-id="30fdd-229">Na hello **myvm1** okně klikněte na tlačítko se třemi tečkami hello (**...** ) tlačítko a potom klikněte na **obnovit virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-229">On hello **myvm1** blade, click hello ellipsis (**...**) button,  and then click **Restore VM**.</span></span>

    ![Virtuální počítač příkazu Obnovit](./media/oracle-backup-recovery/recover_vm_05.png)

5.  <span data-ttu-id="30fdd-231">Na hello **vyberte bod obnovení** okně, vyberte hello položka má toorestore a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-231">On hello **Select restore point** blade, select hello item that you want toorestore, and then click **OK**.</span></span>

    ![Bod obnovení vyberte hello](./media/oracle-backup-recovery/recover_vm_06.png)

    <span data-ttu-id="30fdd-233">Pokud jste povolili zálohování konzistentní s aplikací, se zobrazí modré svislá čára.</span><span class="sxs-lookup"><span data-stu-id="30fdd-233">If you have enabled application-consistent backup, a vertical blue bar appears.</span></span>

6.  <span data-ttu-id="30fdd-234">Na hello **obnovit konfiguraci** okně vyberte hello název virtuálního počítače, vyberte skupinu prostředků hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-234">On hello **Restore configuration** blade, select hello virtual machine name, select hello resource group, and then click **OK**.</span></span>

    ![Obnovení hodnoty konfigurace](./media/oracle-backup-recovery/recover_vm_07.png)

7.  <span data-ttu-id="30fdd-236">toorestore hello virtuální počítač, klikněte na tlačítko hello **obnovení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30fdd-236">toorestore hello VM, click hello **Restore** button.</span></span>

8.  <span data-ttu-id="30fdd-237">Stav hello tooview hello procesu obnovení, klikněte na tlačítko **úlohy**a potom klikněte na **úlohy zálohování**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-237">tooview hello status of hello restore process, click **Jobs**, and then click **Backup Jobs**.</span></span>

    ![Příkaz Stav úlohy zálohování](./media/oracle-backup-recovery/recover_vm_08.png)

    <span data-ttu-id="30fdd-239">Hello následující obrázek znázorňuje hello stav procesu obnovení hello:</span><span class="sxs-lookup"><span data-stu-id="30fdd-239">hello following figure shows hello status of hello restore process:</span></span>

    ![Stav procesu obnovení hello](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-hello-public-ip-address"></a><span data-ttu-id="30fdd-241">Krok 3: Nastavte hello veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="30fdd-241">Step 3: Set hello public IP address</span></span>
<span data-ttu-id="30fdd-242">Po dokončení hello je obnovit virtuální počítač nastavte hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="30fdd-242">After hello VM is restored, set up hello public IP address.</span></span>

1.  <span data-ttu-id="30fdd-243">Hello vyhledávacího pole zadejte **veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-243">In hello search box, enter **public IP address**.</span></span>

    ![Seznam veřejné IP adresy](./media/oracle-backup-recovery/create_ip_00.png)

2.  <span data-ttu-id="30fdd-245">Na hello **veřejné IP adresy** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-245">On hello **Public IP addresses** blade, click **Add**.</span></span> <span data-ttu-id="30fdd-246">Na hello **vytvoření veřejné IP adresy** okně pro **název**, vyberte název veřejné IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-246">On hello **Create public IP address** blade, for **Name**, select hello public IP name.</span></span> <span data-ttu-id="30fdd-247">V části **Skupina prostředků** vyberte **Použít existující**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-247">For **Resource group**, select **Use existing**.</span></span> <span data-ttu-id="30fdd-248">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-248">Then, click **Create**.</span></span>

    ![Vytvoření IP adresy](./media/oracle-backup-recovery/create_ip_01.png)

3.  <span data-ttu-id="30fdd-250">hledání tooassociate hello veřejnou IP adresu s hello síťové rozhraní pro hello virtuálních počítačů a vyberte možnost **myVMip**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-250">tooassociate hello public IP address with hello network interface for hello VM, search for and select **myVMip**.</span></span> <span data-ttu-id="30fdd-251">Potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-251">Then, click **Associate**.</span></span>

    ![Přiřadit IP adresu](./media/oracle-backup-recovery/create_ip_02.png)

4.  <span data-ttu-id="30fdd-253">Pro **typ prostředku**, vyberte **síťové rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-253">For **Resource type**, select **Network interface**.</span></span> <span data-ttu-id="30fdd-254">Vyberte hello síťové rozhraní, které používá hello Můjvp instance a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fdd-254">Select hello network interface that is used by hello myVM instance, and then click **OK**.</span></span>

    ![Vyberte typ prostředku a hodnoty síťový adaptér](./media/oracle-backup-recovery/create_ip_03.png)

5.  <span data-ttu-id="30fdd-256">Vyhledejte a otevřete hello instanci Můjvp, který je přenést z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="30fdd-256">Search for and open hello instance of myVM that is ported from hello portal.</span></span> <span data-ttu-id="30fdd-257">Hello IP adresu, která souvisí s hello virtuálního počítače se zobrazí na hello Můjvp **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="30fdd-257">hello IP address that is associated with hello VM appears on hello myVM **Overview** blade.</span></span>

    ![IP adresa hodnotu](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-toohello-vm"></a><span data-ttu-id="30fdd-259">Krok 4: Připojení toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30fdd-259">Step 4: Connect toohello VM</span></span>

*   <span data-ttu-id="30fdd-260">tooconnect toohello virtuální počítač, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="30fdd-260">tooconnect toohello VM, use hello following script:</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-hello-database-is-accessible"></a><span data-ttu-id="30fdd-261">Krok 5: Test, zda text hello databáze je přístupná</span><span class="sxs-lookup"><span data-stu-id="30fdd-261">Step 5: Test whether hello database is accessible</span></span>
*   <span data-ttu-id="30fdd-262">usnadnění tootest, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="30fdd-262">tootest accessibility, use hello following script:</span></span>

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="30fdd-263">Pokud hello databáze **spuštění** příkaz generuje chybě, toorecover hello databáze najdete v tématu [krok 6: použití RMAN toorecover hello databáze](#step-6-optional-use-rman-to-recover-the-database).</span><span class="sxs-lookup"><span data-stu-id="30fdd-263">If hello database **startup** command generates an error, toorecover hello database, see [Step 6: Use RMAN toorecover hello database](#step-6-optional-use-rman-to-recover-the-database).</span></span>

### <a name="step-6-optional-use-rman-toorecover-hello-database"></a><span data-ttu-id="30fdd-264">Krok 6: (Volitelné) použijte RMAN toorecover hello databáze</span><span class="sxs-lookup"><span data-stu-id="30fdd-264">Step 6: (Optional) Use RMAN toorecover hello database</span></span>
*   <span data-ttu-id="30fdd-265">toorecover hello databáze, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="30fdd-265">toorecover hello database, use hello following script:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

<span data-ttu-id="30fdd-266">Hello zálohování a obnovení databáze 12c hello databáze Oracle na virtuální počítač Azure Linux je nyní dokončena.</span><span class="sxs-lookup"><span data-stu-id="30fdd-266">hello backup and recovery of hello Oracle Database 12c database on an Azure Linux VM is now finished.</span></span>

## <a name="delete-hello-vm"></a><span data-ttu-id="30fdd-267">Odstranit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30fdd-267">Delete hello VM</span></span>

<span data-ttu-id="30fdd-268">Když už nebude potřebovat hello virtuálních počítačů, můžete použít následující skupiny prostředků hello tooremove příkaz hello virtuálních počítačů a všechny související prostředky hello:</span><span class="sxs-lookup"><span data-stu-id="30fdd-268">When you no longer need hello VM, you can use hello following command tooremove hello resource group, hello VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="30fdd-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30fdd-269">Next steps</span></span>

[<span data-ttu-id="30fdd-270">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="30fdd-270">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="30fdd-271">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30fdd-271">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)



