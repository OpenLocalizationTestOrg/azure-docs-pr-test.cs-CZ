---
title: "Vytvoření databáze Oracle virtuální počítač Azure | Microsoft Docs"
description: "Rychle získáte databázi Oracle Database 12c nahoru a spouštění v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 8683b016c4db2c66fb1dd994405b70c3d137a7fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a><span data-ttu-id="23e4f-103">Vytvoření databáze Oracle na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="23e4f-103">Create an Oracle Database in an Azure VM</span></span>

<span data-ttu-id="23e4f-104">Tato příručka podrobně popisuje pomocí rozhraní příkazového řádku Azure k nasazení virtuální počítač z Azure [image Galerie marketplace Oracle](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) k vytvoření databáze Oracle 12 c.</span><span class="sxs-lookup"><span data-stu-id="23e4f-104">This guide details using the Azure CLI to deploy an Azure virtual machine from the [Oracle marketplace gallery image](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) in order to create an Oracle 12c database.</span></span> <span data-ttu-id="23e4f-105">Po nasazení serveru se připojí prostřednictvím SSH. Chcete-li nakonfigurovat databázi Oracle.</span><span class="sxs-lookup"><span data-stu-id="23e4f-105">Once the server is deployed, you will connect via SSH in order to configure the Oracle database.</span></span> 

<span data-ttu-id="23e4f-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="23e4f-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="23e4f-107">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="23e4f-107">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="23e4f-108">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="23e4f-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="23e4f-109">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="23e4f-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="23e4f-110">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="23e4f-110">Create a resource group</span></span>

<span data-ttu-id="23e4f-111">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="23e4f-111">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="23e4f-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="23e4f-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="23e4f-113">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="23e4f-113">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a><span data-ttu-id="23e4f-114">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="23e4f-114">Create virtual machine</span></span>

<span data-ttu-id="23e4f-115">Chcete-li vytvořit virtuální počítač (VM), použijte [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="23e4f-115">To create a virtual machine (VM), use the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="23e4f-116">Následující příklad vytvoří virtuální počítač `myVM`.</span><span class="sxs-lookup"><span data-stu-id="23e4f-116">The following example creates a VM named `myVM`.</span></span> <span data-ttu-id="23e4f-117">Pokud už neexistují v umístění klíče výchozí klíče SSH, vytvoří se také.</span><span class="sxs-lookup"><span data-stu-id="23e4f-117">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="23e4f-118">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="23e4f-118">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="23e4f-119">Po vytvoření virtuálního počítače Azure CLI zobrazí informace, podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="23e4f-119">After you create the VM, Azure CLI displays information similar to the following example.</span></span> <span data-ttu-id="23e4f-120">Poznamenejte si hodnotu pro `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="23e4f-120">Note the value for `publicIpAddress`.</span></span> <span data-ttu-id="23e4f-121">Použijte tuto adresu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="23e4f-121">You use this address to access the VM.</span></span>

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-the-vm"></a><span data-ttu-id="23e4f-122">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="23e4f-122">Connect to the VM</span></span>

<span data-ttu-id="23e4f-123">Chcete-li vytvořit relace SSH s virtuálním Počítačem, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="23e4f-123">To create an SSH session with the VM, use the following command.</span></span> <span data-ttu-id="23e4f-124">Nahraďte na IP adresu `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="23e4f-124">Replace the IP address with the `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="create-the-database"></a><span data-ttu-id="23e4f-125">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="23e4f-125">Create the database</span></span>

<span data-ttu-id="23e4f-126">Oracle software je již nainstalována na bitovou kopii Marketplace.</span><span class="sxs-lookup"><span data-stu-id="23e4f-126">The Oracle software is already installed on the Marketplace image.</span></span> <span data-ttu-id="23e4f-127">Vytvoření ukázkové databáze následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="23e4f-127">Create a sample database as follows.</span></span> 

1.  <span data-ttu-id="23e4f-128">Přepnout *oracle* superuživatele a pak inicializovat naslouchací proces pro protokolování:</span><span class="sxs-lookup"><span data-stu-id="23e4f-128">Switch to the *oracle* superuser, then initialize the listener for logging:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    <span data-ttu-id="23e4f-129">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="23e4f-129">The output is similar to the following:</span></span>

    ```bash
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

2.  <span data-ttu-id="23e4f-130">Vytvoření databáze:</span><span class="sxs-lookup"><span data-stu-id="23e4f-130">Create the database:</span></span>

    ```bash
    dbca -silent \
           -createDatabase \
           -templateName General_Purpose.dbc \
           -gdbname cdb1 \
           -sid cdb1 \
           -responseFile NO_VALUE \
           -characterSet AL32UTF8 \
           -sysPassword OraPasswd1 \
           -systemPassword OraPasswd1 \
           -createAsContainerDatabase true \
           -numberOfPDBs 1 \
           -pdbName pdb1 \
           -pdbAdminPassword OraPasswd1 \
           -databaseType MULTIPURPOSE \
           -automaticMemoryManagement false \
           -storageType FS \
           -ignorePreReqs
    ```

    <span data-ttu-id="23e4f-131">Jak dlouho trvá několik minut pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="23e4f-131">It takes a few minutes to create the database.</span></span>

3. <span data-ttu-id="23e4f-132">Nastavení proměnných Oracle</span><span class="sxs-lookup"><span data-stu-id="23e4f-132">Set Oracle variables</span></span>

<span data-ttu-id="23e4f-133">Než připojíte, musíte nastavit dvě proměnné prostředí: *ORACLE_HOME* a *ORACLE_SID*.</span><span class="sxs-lookup"><span data-stu-id="23e4f-133">Before you connect, you need to set two environment variables: *ORACLE_HOME* and *ORACLE_SID*.</span></span>

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
<span data-ttu-id="23e4f-134">Proměnné ORACLE_HOME a ORACLE_SID také můžete přidat do souboru .bashrc.</span><span class="sxs-lookup"><span data-stu-id="23e4f-134">You also can add ORACLE_HOME and ORACLE_SID variables to the .bashrc file.</span></span> <span data-ttu-id="23e4f-135">To by uložit proměnných prostředí pro budoucí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="23e4f-135">This would save the environment variables for future sign-ins.</span></span> <span data-ttu-id="23e4f-136">Potvrďte následující příkazy byly přidány do `~/.bashrc` soubor pomocí zvoleného editoru.</span><span class="sxs-lookup"><span data-stu-id="23e4f-136">Confirm the following statements have been added to the `~/.bashrc` file using editor of your choice.</span></span>

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a><span data-ttu-id="23e4f-137">Připojení Oracle EM Express</span><span class="sxs-lookup"><span data-stu-id="23e4f-137">Oracle EM Express connectivity</span></span>

<span data-ttu-id="23e4f-138">Pro nástroj pro správu grafického uživatelského rozhraní, které můžete použít k prozkoumání databázi nastavení Oracle EM Express.</span><span class="sxs-lookup"><span data-stu-id="23e4f-138">For a GUI management tool that you can use to explore the database, set up Oracle EM Express.</span></span> <span data-ttu-id="23e4f-139">Pokud chcete připojit k Oracle EM Express, musíte nejprve nastavit port v Oracle.</span><span class="sxs-lookup"><span data-stu-id="23e4f-139">To connect to Oracle EM Express, you must first set up the port in Oracle.</span></span> 

1. <span data-ttu-id="23e4f-140">Připojení k vaší databázi pomocí sqlplus:</span><span class="sxs-lookup"><span data-stu-id="23e4f-140">Connect to your database using sqlplus:</span></span>

    ```bash
    sqlplus / as sysdba
    ```

2. <span data-ttu-id="23e4f-141">Po připojení nastavte port 5502 pro expresní EM</span><span class="sxs-lookup"><span data-stu-id="23e4f-141">Once connected, set the port 5502 for EM Express</span></span>

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. <span data-ttu-id="23e4f-142">Otevřete kontejneru PDB1 není-li již otevřenou, ale první kontrola stavu:</span><span class="sxs-lookup"><span data-stu-id="23e4f-142">Open the container PDB1 if not already opened, but first check the status:</span></span>

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    <span data-ttu-id="23e4f-143">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="23e4f-143">The output is similar to the following:</span></span>

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. <span data-ttu-id="23e4f-144">Pokud OPEN_MODE pro `PDB1` není ČÍST zápisu, spusťte příkazy tady otevřete PDB1:</span><span class="sxs-lookup"><span data-stu-id="23e4f-144">If the OPEN_MODE for `PDB1` is not READ WRITE, then run the followings commands to open PDB1:</span></span>

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

<span data-ttu-id="23e4f-145">Je třeba zadat `quit` k ukončení relace sqlplus a typ `exit` na Odhlásit uživatele oracle.</span><span class="sxs-lookup"><span data-stu-id="23e4f-145">You need to type `quit` to end the sqlplus session and type `exit` to logout of the oracle user.</span></span>

## <a name="automate-database-startup-and-shutdown"></a><span data-ttu-id="23e4f-146">Automatizovat databáze spuštění a vypnutí</span><span class="sxs-lookup"><span data-stu-id="23e4f-146">Automate database startup and shutdown</span></span>

<span data-ttu-id="23e4f-147">Oracle database ve výchozím nastavení není spustit automaticky při restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23e4f-147">The Oracle database by default doesn't automatically start when you restart the VM.</span></span> <span data-ttu-id="23e4f-148">K nastavení databáze Oracle na automatické spuštění, nejprve Přihlaste se jako kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="23e4f-148">To set up the Oracle database to start automatically, first sign in as root.</span></span> <span data-ttu-id="23e4f-149">Pak vytvořte a aktualizovat některé soubory systému.</span><span class="sxs-lookup"><span data-stu-id="23e4f-149">Then, create and update some system files.</span></span>

1. <span data-ttu-id="23e4f-150">Přihlaste se jako kořenového příkazu.</span><span class="sxs-lookup"><span data-stu-id="23e4f-150">Sign on as root</span></span>
    ```bash
    sudo su -
    ```

2.  <span data-ttu-id="23e4f-151">Pomocí oblíbeného editoru, upravte soubor `/etc/oratab` a změnit výchozí `N` k `Y`:</span><span class="sxs-lookup"><span data-stu-id="23e4f-151">Using your favorite editor, edit the file `/etc/oratab` and change the default `N` to `Y`:</span></span>

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  <span data-ttu-id="23e4f-152">Vytvořte soubor s názvem `/etc/init.d/dbora` a vložte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="23e4f-152">Create a file named `/etc/init.d/dbora` and paste the following contents:</span></span>

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  <span data-ttu-id="23e4f-153">Změna oprávnění u souborů s *chmod* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23e4f-153">Change permissions on files with *chmod* as follows:</span></span>

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  <span data-ttu-id="23e4f-154">Vytvořte symbolické odkazy pro spuštění a vypnutí takto:</span><span class="sxs-lookup"><span data-stu-id="23e4f-154">Create symbolic links for startup and shutdown as follows:</span></span>

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  <span data-ttu-id="23e4f-155">K testování změny, restartujte virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="23e4f-155">To test your changes, restart the VM:</span></span>

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a><span data-ttu-id="23e4f-156">Otevřené porty pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="23e4f-156">Open ports for connectivity</span></span>

<span data-ttu-id="23e4f-157">Poslední úloha je konfigurace některé externí koncové body.</span><span class="sxs-lookup"><span data-stu-id="23e4f-157">The final task is to configure some external endpoints.</span></span> <span data-ttu-id="23e4f-158">Nastavit skupinu zabezpečení sítě Azure, který chrání virtuální počítač, ukončete nejprve relace SSH ve virtuálním počítači (by měl mít byla spuštěna z SSH při restartování v předchozím kroku).</span><span class="sxs-lookup"><span data-stu-id="23e4f-158">To set up the Azure Network Security Group that protects the VM, first exit your SSH session in the VM (should have been kicked out of SSH when rebooting in previous step).</span></span> 

1.  <span data-ttu-id="23e4f-159">Otevření koncového bodu, který používáte pro přístup k databázi Oracle vzdáleně, vytvořte skupinu zabezpečení sítě pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23e4f-159">To open the endpoint that you use to access the Oracle database remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span> 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  <span data-ttu-id="23e4f-160">Otevření koncového bodu, který používáte pro přístup k Oracle EM Express vzdáleně, vytvořit skupinu zabezpečení sítě pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23e4f-160">To open the endpoint that you use to access Oracle EM Express remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span>

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. <span data-ttu-id="23e4f-161">V případě potřeby získat veřejnou IP adresu vašeho virtuálního počítače znovu s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23e4f-161">If needed, obtain the public IP address of your VM again with [az network public-ip show](/cli/azure/network/public-ip#show) as follows:</span></span>

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  <span data-ttu-id="23e4f-162">Připojte EM Express z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="23e4f-162">Connect EM Express from your browser.</span></span> <span data-ttu-id="23e4f-163">Ověřte, zda váš prohlížeč je kompatibilní s EM Express (je vyžadována instalace Flash):</span><span class="sxs-lookup"><span data-stu-id="23e4f-163">Make sure your browser is compatible with EM Express (Flash install is required):</span></span> 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

<span data-ttu-id="23e4f-164">Se můžete přihlásit pomocí **SYS** účtu a zkontrolujte, zda **jako sysdba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="23e4f-164">You can log in by using the **SYS** account, and check the **as sysdba** checkbox.</span></span> <span data-ttu-id="23e4f-165">Heslo použít **OraPasswd1** nastavený během instalace.</span><span class="sxs-lookup"><span data-stu-id="23e4f-165">Use the password **OraPasswd1** that you set during installation.</span></span> 

![Snímek obrazovky přihlašovací stránky Oracle OEM Express](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a><span data-ttu-id="23e4f-167">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="23e4f-167">Clean up resources</span></span>

<span data-ttu-id="23e4f-168">Po dokončení zkoumat první databáze Oracle na Azure a virtuální počítač již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má odeberte skupinu zdrojů, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="23e4f-168">Once you have finished exploring your first Oracle database on Azure and the VM is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="23e4f-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23e4f-169">Next steps</span></span>

<span data-ttu-id="23e4f-170">Další informace o dalších [Oracle řešení v Azure](oracle-considerations.md).</span><span class="sxs-lookup"><span data-stu-id="23e4f-170">Learn about other [Oracle solutions on Azure](oracle-considerations.md).</span></span> 

<span data-ttu-id="23e4f-171">Zkuste [instalace a konfigurace Oracle automatizované úložiště správy](configure-oracle-asm.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="23e4f-171">Try the [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.</span></span>