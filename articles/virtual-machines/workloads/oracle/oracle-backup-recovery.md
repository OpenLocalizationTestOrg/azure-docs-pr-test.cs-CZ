---
title: "Zálohování a obnovení databáze 12c databáze Oracle na virtuálním počítači Azure Linux | Microsoft Docs"
description: "Zjistěte, jak zálohovat a obnovit databázi Oracle Database 12c v prostředí Azure."
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
ms.openlocfilehash: 9a2293f13b90e9a4cb11b4169fad969dd622a9a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="29112-103">Zálohování a obnovení databáze 12c databáze Oracle na virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="29112-103">Back up and recover an Oracle Database 12c database on an Azure Linux virtual machine</span></span>

<span data-ttu-id="29112-104">Rozhraní příkazového řádku Azure můžete vytvořit a spravovat prostředky Azure na příkazovém řádku nebo pomocí skriptů.</span><span class="sxs-lookup"><span data-stu-id="29112-104">You can use Azure CLI to create and manage Azure resources at a command prompt, or use scripts.</span></span> <span data-ttu-id="29112-105">V tomto článku používáme skripty rozhraní příkazového řádku Azure k nasazení databázi Oracle Database 12c z Galerie image Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="29112-105">In this article, we use Azure CLI scripts to deploy an Oracle Database 12c database from an Azure Marketplace gallery image.</span></span>

<span data-ttu-id="29112-106">Než začnete, ujistěte se, že je nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="29112-106">Before you begin, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="29112-107">Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="29112-107">For more information, see the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="29112-108">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="29112-108">Prepare the environment</span></span>

### <a name="step-1-prerequisites"></a><span data-ttu-id="29112-109">Krok 1: požadavky</span><span class="sxs-lookup"><span data-stu-id="29112-109">Step 1: Prerequisites</span></span>

*   <span data-ttu-id="29112-110">Chcete-li provést proces zálohování a obnovení, musíte nejdřív vytvořit virtuální počítač Linux, který má nainstalovanou instanci databáze Oracle 12c.</span><span class="sxs-lookup"><span data-stu-id="29112-110">To perform the backup and recovery process, you must first create a Linux VM that has an installed instance of Oracle Database 12c.</span></span> <span data-ttu-id="29112-111">Název bitové kopie Marketplace, které použijete k vytvoření virtuálního počítače *Oracle: Oracle – databáze-Ee:12.1.0.2:latest*.</span><span class="sxs-lookup"><span data-stu-id="29112-111">The Marketplace image you use to create the VM is named *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.</span></span>

    <span data-ttu-id="29112-112">Naučte se vytvořit databázi Oracle, najdete v článku [Oracle vytvořit rychlý start databáze](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span><span class="sxs-lookup"><span data-stu-id="29112-112">To learn how to create an Oracle database, see the [Oracle create database quickstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span></span>


### <a name="step-2-connect-to-the-vm"></a><span data-ttu-id="29112-113">Krok 2: Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="29112-113">Step 2: Connect to the VM</span></span>

*   <span data-ttu-id="29112-114">Pro vytvoření relace Secure Shell (SSH) s virtuálním Počítačem, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="29112-114">To create a Secure Shell (SSH) session with the VM, use the following command.</span></span> <span data-ttu-id="29112-115">Nahraďte adresu IP a názvem hostitele s `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="29112-115">Replace the IP address and the host name combination with the `publicIpAddress` value for your VM.</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a><span data-ttu-id="29112-116">Krok 3: Příprava databáze</span><span class="sxs-lookup"><span data-stu-id="29112-116">Step 3: Prepare the database</span></span>

1.  <span data-ttu-id="29112-117">Tento krok předpokládá, že máte instanci Oracle (cdb1), která běží na virtuálním počítači s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="29112-117">This step assumes that you have an Oracle instance (cdb1) that is running on a VM named *myVM*.</span></span>

    <span data-ttu-id="29112-118">Spustit *oracle* kořenové superuživatele a pak inicializovat naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="29112-118">Run the *oracle* superuser root, and then initialize the listener:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
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
    The listener supports no services
    The command completed successfully
    ```

2.  <span data-ttu-id="29112-119">(Volitelné) Ujistěte se, že databáze je v režimu protokolu archivu:</span><span class="sxs-lookup"><span data-stu-id="29112-119">(Optional) Make sure the database is in archive log mode:</span></span>

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
3.  <span data-ttu-id="29112-120">(Volitelné) Vytvořte tabulku pro testování potvrzení:</span><span class="sxs-lookup"><span data-stu-id="29112-120">(Optional) Create a table to test the commit:</span></span>

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
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
4.  <span data-ttu-id="29112-121">Ověřte nebo změňte umístění záložního souboru a velikost:</span><span class="sxs-lookup"><span data-stu-id="29112-121">Verify or change the backup file location and size:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. <span data-ttu-id="29112-122">Použijte k zálohování databáze Oracle obnovení správce (RMAN):</span><span class="sxs-lookup"><span data-stu-id="29112-122">Use Oracle Recovery Manager (RMAN) to back up the database:</span></span>

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a><span data-ttu-id="29112-123">Krok 4: Zálohování konzistentní s aplikací pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="29112-123">Step 4: Application-consistent backup for Linux VMs</span></span>

<span data-ttu-id="29112-124">Zálohování konzistentní s aplikací je nová funkce v Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="29112-124">Application-consistent backups is a new feature in Azure Backup.</span></span> <span data-ttu-id="29112-125">Můžete vytvořit a vyberte skripty provést před a po snímek virtuálního počítače (snímek před a po pořízení snímku).</span><span class="sxs-lookup"><span data-stu-id="29112-125">You can create and select scripts to execute before and after the VM snapshot (pre-snapshot and post-snapshot).</span></span>

1. <span data-ttu-id="29112-126">Stažení souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="29112-126">Download the JSON file.</span></span>

    <span data-ttu-id="29112-127">Stáhněte si VMSnapshotScriptPluginConfig.json z https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span><span class="sxs-lookup"><span data-stu-id="29112-127">Download VMSnapshotScriptPluginConfig.json from https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span></span> <span data-ttu-id="29112-128">Obsah souboru vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="29112-128">The file contents look similar to the following:</span></span>

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

2. <span data-ttu-id="29112-129">Vytvořte složku /etc/azure na virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="29112-129">Create the /etc/azure folder on the VM:</span></span>

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. <span data-ttu-id="29112-130">Zkopírujte soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="29112-130">Copy the JSON file.</span></span>

    <span data-ttu-id="29112-131">Zkopírujte VMSnapshotScriptPluginConfig.json do složky /etc/azure.</span><span class="sxs-lookup"><span data-stu-id="29112-131">Copy VMSnapshotScriptPluginConfig.json to the /etc/azure folder.</span></span>

4. <span data-ttu-id="29112-132">Upravte soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="29112-132">Edit the JSON file.</span></span>

    <span data-ttu-id="29112-133">Upravte soubor VMSnapshotScriptPluginConfig.json zahrnout `PreScriptLocation` a `PostScriptlocation` parametry.</span><span class="sxs-lookup"><span data-stu-id="29112-133">Edit the VMSnapshotScriptPluginConfig.json file to include the `PreScriptLocation` and `PostScriptlocation` parameters.</span></span> <span data-ttu-id="29112-134">Například:</span><span class="sxs-lookup"><span data-stu-id="29112-134">For example:</span></span>

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

5. <span data-ttu-id="29112-135">Vytvoření snímku před a po pořízení snímku skriptu souborů.</span><span class="sxs-lookup"><span data-stu-id="29112-135">Create the pre-snapshot and post-snapshot script files.</span></span>

    <span data-ttu-id="29112-136">Tady je příklad snímku před a po pořízení snímku skripty pro "cold zálohování" (zálohu do offline režimu, vypnutí a restartování):</span><span class="sxs-lookup"><span data-stu-id="29112-136">Here's an example of pre-snapshot and post-snapshot scripts for a "cold backup" (an offline backup, with shutdown and restart):</span></span>

    <span data-ttu-id="29112-137">Pro /etc/azure/pre_script.sh:</span><span class="sxs-lookup"><span data-stu-id="29112-137">For /etc/azure/pre_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="29112-138">Pro /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="29112-138">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    <span data-ttu-id="29112-139">Tady je příklad snímku před a po pořízení snímku skripty pro "za zálohování" (online zálohování):</span><span class="sxs-lookup"><span data-stu-id="29112-139">Here's an example of pre-snapshot and post-snapshot scripts for a "hot backup" (an online backup):</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="29112-140">Pro /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="29112-140">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="29112-141">/Etc/azure/pre_script.sql upravte obsah souboru podle požadavků:</span><span class="sxs-lookup"><span data-stu-id="29112-141">For /etc/azure/pre_script.sql, modify the contents of the file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    <span data-ttu-id="29112-142">/Etc/azure/post_script.sql upravte obsah souboru podle požadavků:</span><span class="sxs-lookup"><span data-stu-id="29112-142">For /etc/azure/post_script.sql, modify the contents of the file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. <span data-ttu-id="29112-143">Změna oprávnění k souboru:</span><span class="sxs-lookup"><span data-stu-id="29112-143">Change file permissions:</span></span>

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. <span data-ttu-id="29112-144">Testovat skripty.</span><span class="sxs-lookup"><span data-stu-id="29112-144">Test the scripts.</span></span>

    <span data-ttu-id="29112-145">K testování skriptů, nejprve, přihlaste se jako kořenový.</span><span class="sxs-lookup"><span data-stu-id="29112-145">To test the scripts, first, sign in as root.</span></span> <span data-ttu-id="29112-146">Potom zkontrolujte, že nejsou žádné chyby:</span><span class="sxs-lookup"><span data-stu-id="29112-146">Then, ensure that there are no errors:</span></span>

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

<span data-ttu-id="29112-147">Další informace najdete v tématu [zálohování konzistentní s aplikací pro virtuální počítače s Linuxem](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span><span class="sxs-lookup"><span data-stu-id="29112-147">For more information, see [Application-consistent backup for Linux VMs](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span></span>


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a><span data-ttu-id="29112-148">Krok 5: Trezory služeb zotavení Azure pomocí zálohování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="29112-148">Step 5: Use Azure Recovery Services vaults to back up the VM</span></span>

1.  <span data-ttu-id="29112-149">Na portálu Azure, vyhledejte **trezory služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="29112-149">In the Azure portal, search for **Recovery Services vaults**.</span></span>

    ![Stránka trezory služby zotavení](./media/oracle-backup-recovery/recovery_service_01.png)

2.  <span data-ttu-id="29112-151">Na **trezory služeb zotavení** okno pro přidání nové úložiště, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="29112-151">On the **Recovery Services vaults** blade, to add a new vault, click **Add**.</span></span>

    ![Trezory služeb zotavení přidat stránku](./media/oracle-backup-recovery/recovery_service_02.png)

3.  <span data-ttu-id="29112-153">Chcete-li pokračovat, klikněte na tlačítko **myVault**.</span><span class="sxs-lookup"><span data-stu-id="29112-153">To continue, click **myVault**.</span></span>

    ![Stránku podrobností trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_03.png)

4.  <span data-ttu-id="29112-155">Na **myVault** okně klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="29112-155">On the **myVault** blade, click **Backup**.</span></span>

    ![Trezory služeb zotavení zálohování stránky](./media/oracle-backup-recovery/recovery_service_04.png)

5.  <span data-ttu-id="29112-157">Na **cíl zálohování** okně použít výchozí hodnoty **Azure** a **virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="29112-157">On the **Backup Goal** blade, use the default values of **Azure** and **Virtual machine**.</span></span> <span data-ttu-id="29112-158">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29112-158">Click **OK**.</span></span>

    ![Stránku podrobností trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_05.png)

6.  <span data-ttu-id="29112-160">Pro **zálohování zásad**, použijte **DefaultPolicy**, nebo vyberte **vytvořit novou zásadu**.</span><span class="sxs-lookup"><span data-stu-id="29112-160">For **Backup policy**, use **DefaultPolicy**, or select **Create New policy**.</span></span> <span data-ttu-id="29112-161">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29112-161">Click **OK**.</span></span>

    ![Stránka podrobnosti zásady zálohování trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_06.png)

7.  <span data-ttu-id="29112-163">Na **vybrat virtuální počítače** okně, vyberte **myVM1** zaškrtněte políčko a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29112-163">On the **Select virtual machines** blade, select the **myVM1** check box, and then click **OK**.</span></span> <span data-ttu-id="29112-164">Klikněte **povolit zálohování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29112-164">Click the **Enable backup** button.</span></span>

    ![Položek trezory služeb zotavení na stránku podrobností zálohování](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > <span data-ttu-id="29112-166">Po kliknutí na tlačítko **povolit zálohování**, proces zálohování nelze spustit, až do vypršení platnosti naplánovaném čase.</span><span class="sxs-lookup"><span data-stu-id="29112-166">After you click **Enable backup**, the backup process doesn't start until the scheduled time expires.</span></span> <span data-ttu-id="29112-167">Nastavit okamžitou zálohu, proveďte další krok.</span><span class="sxs-lookup"><span data-stu-id="29112-167">To set up an immediate backup, complete the next step.</span></span>

8.  <span data-ttu-id="29112-168">Na **myVault - zálohování položek** okno, v části **zálohování počet položek**, vyberte počet zálohování položek.</span><span class="sxs-lookup"><span data-stu-id="29112-168">On the **myVault - Backup items** blade, under **BACKUP ITEM COUNT**, select the backup item count.</span></span>

    ![Stránka Podrobnosti o obnovení služby trezory myVault](./media/oracle-backup-recovery/recovery_service_08.png)

9.  <span data-ttu-id="29112-170">Na **zálohování položek (virtuální počítač Azure)** okno na pravé straně stránky klikněte na tlačítko se třemi tečkami (**...** ) tlačítko a potom klikněte na **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="29112-170">On the **Backup Items (Azure Virtual Machine)** blade, on the right side of the page, click the ellipsis (**...**) button, and then click **Backup now**.</span></span>

    ![Zálohování teď příkaz trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_09.png)

10. <span data-ttu-id="29112-172">Klikněte **zálohování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29112-172">Click the **Backup** button.</span></span> <span data-ttu-id="29112-173">Počkejte na dokončení procesu zálohování.</span><span class="sxs-lookup"><span data-stu-id="29112-173">Wait for the backup process to finish.</span></span> <span data-ttu-id="29112-174">Potom přejděte na [krok 6: odebrat soubory databáze](#step-6-remove-the-database-files).</span><span class="sxs-lookup"><span data-stu-id="29112-174">Then, go to [Step 6: Remove the database files](#step-6-remove-the-database-files).</span></span>

    <span data-ttu-id="29112-175">Chcete-li zobrazit stav úlohy zálohování, klikněte na tlačítko **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="29112-175">To view the status of the backup job, click **Jobs**.</span></span>

    ![Trezory služeb zotavení úlohy stránky](./media/oracle-backup-recovery/recovery_service_10.png)

    <span data-ttu-id="29112-177">Na následujícím obrázku se zobrazí stav úlohy zálohování:</span><span class="sxs-lookup"><span data-stu-id="29112-177">The status of the backup job appears in the following image:</span></span>

    ![Stránka se stavem úlohy trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_11.png)

11. <span data-ttu-id="29112-179">Zálohování konzistentní s aplikací vyřešte všechny chyby v souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="29112-179">For an application-consistent backup, address any errors in the log file.</span></span> <span data-ttu-id="29112-180">Soubor protokolu se nachází v /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span><span class="sxs-lookup"><span data-stu-id="29112-180">The log file is located at /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span></span>

### <a name="step-6-remove-the-database-files"></a><span data-ttu-id="29112-181">Krok 6: Odebrat soubory databáze</span><span class="sxs-lookup"><span data-stu-id="29112-181">Step 6: Remove the database files</span></span> 
<span data-ttu-id="29112-182">Později v tomto článku se dozvíte testování procesu obnovení.</span><span class="sxs-lookup"><span data-stu-id="29112-182">Later in this article, you'll learn how to test the recovery process.</span></span> <span data-ttu-id="29112-183">Chcete-li otestovat proces obnovení, budete muset odebrat soubory databáze.</span><span class="sxs-lookup"><span data-stu-id="29112-183">Before you can test the recovery process, you have to remove the database files.</span></span>

1.  <span data-ttu-id="29112-184">Odebrání souborů tabulkového prostoru a zálohování:</span><span class="sxs-lookup"><span data-stu-id="29112-184">Remove the tablespace and backup files:</span></span>

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  <span data-ttu-id="29112-185">(Volitelné) Vypněte instanci Oracle:</span><span class="sxs-lookup"><span data-stu-id="29112-185">(Optional) Shut down the Oracle instance:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a><span data-ttu-id="29112-186">Obnovení odstraněných souborů z trezory služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="29112-186">Restore the deleted files from the Recovery Services vaults</span></span>
<span data-ttu-id="29112-187">Obnovení odstraněných souborů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="29112-187">To restore the deleted files, complete the following steps:</span></span>

1. <span data-ttu-id="29112-188">Na portálu Azure, vyhledejte *myVault* položky trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="29112-188">In the Azure portal, search for the *myVault* Recovery Services vaults item.</span></span> <span data-ttu-id="29112-189">Na **přehled** okno, v části **zálohování položek**, vyberte počet položek.</span><span class="sxs-lookup"><span data-stu-id="29112-189">On the **Overview** blade, under **Backup items**, select the number of items.</span></span>

    ![Obnovení služby trezory myVault zálohování položek](./media/oracle-backup-recovery/recovery_service_12.png)

2. <span data-ttu-id="29112-191">V části **počet položek zálohování**, vyberte počet položek.</span><span class="sxs-lookup"><span data-stu-id="29112-191">Under **BACKUP ITEM COUNT**, select the number of items.</span></span>

    ![Počet položek. zálohování virtuálního počítače Azure trezory služeb zotavení](./media/oracle-backup-recovery/recovery_service_13.png)

3. <span data-ttu-id="29112-193">Na **myvm1** okně klikněte na tlačítko **obnovení souboru (Preview)**.</span><span class="sxs-lookup"><span data-stu-id="29112-193">On the **myvm1** blade, click **File Recovery (Preview)**.</span></span>

    ![Snímek obrazovky služeb zotavení trezory stránka obnovení souboru](./media/oracle-backup-recovery/recovery_service_14.png)

4. <span data-ttu-id="29112-195">Na **obnovení souboru (Preview)** podokně klikněte na tlačítko **stáhnout skript**.</span><span class="sxs-lookup"><span data-stu-id="29112-195">On the **File Recovery (Preview)** pane, click **Download Script**.</span></span> <span data-ttu-id="29112-196">Potom uložte soubor ke stažení (.sh) do složky v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="29112-196">Then, save the download (.sh) file to a folder on the client computer.</span></span>

    ![Stáhnout možnosti uloží soubor skriptu](./media/oracle-backup-recovery/recovery_service_15.png)

5. <span data-ttu-id="29112-198">Zkopírujte soubor .sh k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="29112-198">Copy the .sh file to the VM.</span></span>

    <span data-ttu-id="29112-199">Následující příklad ukazuje, jak používat zabezpečený kopie (scp) příkaz přesunout soubor k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="29112-199">The following example shows how you to use a secure copy (scp) command to move the file to the VM.</span></span> <span data-ttu-id="29112-200">Také můžete zkopírovat obsah do schránky a vložte obsah do nového souboru, který je nastavený na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="29112-200">You also can copy the contents to the clipboard, and then paste the contents in a new file that is set up on the VM.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="29112-201">V následujícím příkladu Ujistěte se, aktualizaci hodnoty IP adres a složky.</span><span class="sxs-lookup"><span data-stu-id="29112-201">In the following example, ensure that you update the IP address and folder values.</span></span> <span data-ttu-id="29112-202">Hodnoty musí být mapována na složku, kam je uložena souboru.</span><span class="sxs-lookup"><span data-stu-id="29112-202">The values must map to the folder where the file is saved.</span></span>

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. <span data-ttu-id="29112-203">Změňte souboru tak, aby vlastníkem kořenu.</span><span class="sxs-lookup"><span data-stu-id="29112-203">Change the file, so that it's owned by the root.</span></span>

    <span data-ttu-id="29112-204">V následujícím příkladu změňte soubor tak, aby vlastníkem kořenu.</span><span class="sxs-lookup"><span data-stu-id="29112-204">In the following example, change the file so that it's owned by the root.</span></span> <span data-ttu-id="29112-205">Potom změňte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="29112-205">Then, change permissions.</span></span>

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    <span data-ttu-id="29112-206">Následující příklad ukazuje, co byste měli vidět po spuštění uvedený skript.</span><span class="sxs-lookup"><span data-stu-id="29112-206">The following example shows what you should see after you run the preceding script.</span></span> <span data-ttu-id="29112-207">Když se zobrazí výzva, chcete-li pokračovat, zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="29112-207">When you're prompted to continue, enter **Y**.</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. <span data-ttu-id="29112-208">Přístup k připojené svazky je potvrzen.</span><span class="sxs-lookup"><span data-stu-id="29112-208">Access to the mounted volumes is confirmed.</span></span>

    <span data-ttu-id="29112-209">Chcete-li ukončit, zadejte **q**a poté vyhledejte připojených svazků.</span><span class="sxs-lookup"><span data-stu-id="29112-209">To exit, enter **q**, and then search for the mounted volumes.</span></span> <span data-ttu-id="29112-210">Chcete-li vytvořit seznam přidaných svazky, na příkazovém řádku, zadejte **df – KB**.</span><span class="sxs-lookup"><span data-stu-id="29112-210">To create a list of the added volumes, at a command prompt, enter **df -k**.</span></span>

    ![-K DF – příkaz](./media/oracle-backup-recovery/recovery_service_16.png)

8. <span data-ttu-id="29112-212">Ke zkopírování chybějící soubory zpět do složky použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="29112-212">Use the following script to copy the missing files back to the folders:</span></span>

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
9. <span data-ttu-id="29112-213">V následující skript použijte k obnovení databázi RMAN:</span><span class="sxs-lookup"><span data-stu-id="29112-213">In the following script, use RMAN to recover the database:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. <span data-ttu-id="29112-214">Odpojte disk.</span><span class="sxs-lookup"><span data-stu-id="29112-214">Unmount the disk.</span></span>

    <span data-ttu-id="29112-215">Na portálu Azure na **obnovení souboru (Preview)** okně klikněte na tlačítko **odpojit disky**.</span><span class="sxs-lookup"><span data-stu-id="29112-215">In the Azure portal, on the **File Recovery (Preview)** blade, click **Unmount Disks**.</span></span>

    ![Odpojení příkaz disky](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a><span data-ttu-id="29112-217">Obnovit celý virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="29112-217">Restore the entire VM</span></span>

<span data-ttu-id="29112-218">Místo obnovení odstraněných souborů ze trezory služeb zotavení, můžete obnovit celý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="29112-218">Instead of restoring the deleted files from the Recovery Services vaults, you can restore the entire VM.</span></span>

### <a name="step-1-delete-myvm"></a><span data-ttu-id="29112-219">Krok 1: Odstranění Můjvp</span><span class="sxs-lookup"><span data-stu-id="29112-219">Step 1: Delete myVM</span></span>

*   <span data-ttu-id="29112-220">V portálu Azure přejděte do **myVM1** trezoru a potom vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="29112-220">In the Azure portal, go to the **myVM1** vault, and then select **Delete**.</span></span>

    ![Příkaz delete trezoru](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a><span data-ttu-id="29112-222">Krok 2: Obnovení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="29112-222">Step 2: Recover the VM</span></span>

1.  <span data-ttu-id="29112-223">Přejděte na **trezory služeb zotavení**a potom vyberte **myVault**.</span><span class="sxs-lookup"><span data-stu-id="29112-223">Go to **Recovery Services vaults**, and then select **myVault**.</span></span>

    ![Položka myVault](./media/oracle-backup-recovery/recover_vm_02.png)

2.  <span data-ttu-id="29112-225">Na **přehled** okno, v části **zálohování položek**, vyberte počet položek.</span><span class="sxs-lookup"><span data-stu-id="29112-225">On the **Overview** blade, under **Backup items**, select the number of items.</span></span>

    ![myVault zálohování položek](./media/oracle-backup-recovery/recover_vm_03.png)

3.  <span data-ttu-id="29112-227">Na **zálohování položek (virtuální počítač Azure)** vyberte **myvm1**.</span><span class="sxs-lookup"><span data-stu-id="29112-227">On the **Backup Items (Azure Virtual Machine)** blade, select **myvm1**.</span></span>

    ![Stránku obnovení virtuálního počítače](./media/oracle-backup-recovery/recover_vm_04.png)

4.  <span data-ttu-id="29112-229">Na **myvm1** okně klikněte na tlačítko se třemi tečkami (**...** ) tlačítko a potom klikněte na **obnovit virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="29112-229">On the **myvm1** blade, click the ellipsis (**...**) button,  and then click **Restore VM**.</span></span>

    ![Virtuální počítač příkazu Obnovit](./media/oracle-backup-recovery/recover_vm_05.png)

5.  <span data-ttu-id="29112-231">Na **vyberte bod obnovení** okno, vyberte položku, kterou chcete obnovit a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="29112-231">On the **Select restore point** blade, select the item that you want to restore, and then click **OK**.</span></span>

    ![Vyberte bod obnovení](./media/oracle-backup-recovery/recover_vm_06.png)

    <span data-ttu-id="29112-233">Pokud jste povolili zálohování konzistentní s aplikací, se zobrazí modré svislá čára.</span><span class="sxs-lookup"><span data-stu-id="29112-233">If you have enabled application-consistent backup, a vertical blue bar appears.</span></span>

6.  <span data-ttu-id="29112-234">Na **obnovit konfiguraci** okně, vyberte název virtuálního počítače, vyberte skupinu prostředků a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="29112-234">On the **Restore configuration** blade, select the virtual machine name, select the resource group, and then click **OK**.</span></span>

    ![Obnovení hodnoty konfigurace](./media/oracle-backup-recovery/recover_vm_07.png)

7.  <span data-ttu-id="29112-236">Chcete-li obnovit virtuální počítač, klikněte na tlačítko **obnovení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29112-236">To restore the VM, click the **Restore** button.</span></span>

8.  <span data-ttu-id="29112-237">Chcete-li zobrazit stav procesu obnovení, klikněte na tlačítko **úlohy**a potom klikněte na **úlohy zálohování**.</span><span class="sxs-lookup"><span data-stu-id="29112-237">To view the status of the restore process, click **Jobs**, and then click **Backup Jobs**.</span></span>

    ![Příkaz Stav úlohy zálohování](./media/oracle-backup-recovery/recover_vm_08.png)

    <span data-ttu-id="29112-239">Následující obrázek ukazuje stav procesu obnovení:</span><span class="sxs-lookup"><span data-stu-id="29112-239">The following figure shows the status of the restore process:</span></span>

    ![Stav procesu obnovení](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a><span data-ttu-id="29112-241">Krok 3: Nastavte veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="29112-241">Step 3: Set the public IP address</span></span>
<span data-ttu-id="29112-242">Po obnovení virtuálního počítače, nastavte veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="29112-242">After the VM is restored, set up the public IP address.</span></span>

1.  <span data-ttu-id="29112-243">Do vyhledávacího pole zadejte **veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="29112-243">In the search box, enter **public IP address**.</span></span>

    ![Seznam veřejné IP adresy](./media/oracle-backup-recovery/create_ip_00.png)

2.  <span data-ttu-id="29112-245">Na **veřejné IP adresy** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="29112-245">On the **Public IP addresses** blade, click **Add**.</span></span> <span data-ttu-id="29112-246">Na **vytvoření veřejné IP adresy** okně pro **název**, vyberte název veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="29112-246">On the **Create public IP address** blade, for **Name**, select the public IP name.</span></span> <span data-ttu-id="29112-247">V části **Skupina prostředků** vyberte **Použít existující**.</span><span class="sxs-lookup"><span data-stu-id="29112-247">For **Resource group**, select **Use existing**.</span></span> <span data-ttu-id="29112-248">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="29112-248">Then, click **Create**.</span></span>

    ![Vytvoření IP adresy](./media/oracle-backup-recovery/create_ip_01.png)

3.  <span data-ttu-id="29112-250">Chcete-li přidružit veřejnou IP adresu pro virtuální počítač se síťovým rozhraním, vyhledejte a vyberte **myVMip**.</span><span class="sxs-lookup"><span data-stu-id="29112-250">To associate the public IP address with the network interface for the VM, search for and select **myVMip**.</span></span> <span data-ttu-id="29112-251">Potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="29112-251">Then, click **Associate**.</span></span>

    ![Přiřadit IP adresu](./media/oracle-backup-recovery/create_ip_02.png)

4.  <span data-ttu-id="29112-253">Pro **typ prostředku**, vyberte **síťové rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="29112-253">For **Resource type**, select **Network interface**.</span></span> <span data-ttu-id="29112-254">Vyberte síťové rozhraní, které používá Můjvp instance a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="29112-254">Select the network interface that is used by the myVM instance, and then click **OK**.</span></span>

    ![Vyberte typ prostředku a hodnoty síťový adaptér](./media/oracle-backup-recovery/create_ip_03.png)

5.  <span data-ttu-id="29112-256">Vyhledejte a otevřete instanci Můjvp, který je přenést z portálu.</span><span class="sxs-lookup"><span data-stu-id="29112-256">Search for and open the instance of myVM that is ported from the portal.</span></span> <span data-ttu-id="29112-257">IP adresu, která souvisí s virtuálním Počítačem se zobrazí na Můjvp **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="29112-257">The IP address that is associated with the VM appears on the myVM **Overview** blade.</span></span>

    ![IP adresa hodnotu](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a><span data-ttu-id="29112-259">Krok 4: Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="29112-259">Step 4: Connect to the VM</span></span>

*   <span data-ttu-id="29112-260">Pokud chcete připojit k virtuálnímu počítači, použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="29112-260">To connect to the VM, use the following script:</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a><span data-ttu-id="29112-261">Krok 5: Test, zda databáze je přístupná</span><span class="sxs-lookup"><span data-stu-id="29112-261">Step 5: Test whether the database is accessible</span></span>
*   <span data-ttu-id="29112-262">K otestování usnadnění, použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="29112-262">To test accessibility, use the following script:</span></span>

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="29112-263">Pokud databázi **spuštění** příkaz, vygeneruje se chyba, chcete-li obnovit databázi, přečtěte si téma [krok 6: RMAN použijte k obnovení databázi](#step-6-optional-use-rman-to-recover-the-database).</span><span class="sxs-lookup"><span data-stu-id="29112-263">If the database **startup** command generates an error, to recover the database, see [Step 6: Use RMAN to recover the database](#step-6-optional-use-rman-to-recover-the-database).</span></span>

### <a name="step-6-optional-use-rman-to-recover-the-database"></a><span data-ttu-id="29112-264">Krok 6: (Volitelné) použijte RMAN k obnovení databáze</span><span class="sxs-lookup"><span data-stu-id="29112-264">Step 6: (Optional) Use RMAN to recover the database</span></span>
*   <span data-ttu-id="29112-265">Chcete-li obnovit databázi, použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="29112-265">To recover the database, use the following script:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

<span data-ttu-id="29112-266">Zálohování a obnovení databáze 12c databáze Oracle na virtuální počítač Azure Linux je nyní dokončena.</span><span class="sxs-lookup"><span data-stu-id="29112-266">The backup and recovery of the Oracle Database 12c database on an Azure Linux VM is now finished.</span></span>

## <a name="delete-the-vm"></a><span data-ttu-id="29112-267">Odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="29112-267">Delete the VM</span></span>

<span data-ttu-id="29112-268">Když virtuální počítač již nepotřebujete, můžete odebrat skupinu prostředků, virtuální počítač a všechny související prostředky následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29112-268">When you no longer need the VM, you can use the following command to remove the resource group, the VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="29112-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29112-269">Next steps</span></span>

[<span data-ttu-id="29112-270">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="29112-270">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="29112-271">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="29112-271">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)



