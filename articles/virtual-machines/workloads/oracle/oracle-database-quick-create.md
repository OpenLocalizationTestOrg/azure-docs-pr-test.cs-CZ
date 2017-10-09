---
title: "aaaCreate databáze Oracle virtuální počítač Azure | Microsoft Docs"
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
ms.openlocfilehash: 83205154c3275d5f57b46c8acfb0cb4e5c68a412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a><span data-ttu-id="fe093-103">Vytvoření databáze Oracle na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="fe093-103">Create an Oracle Database in an Azure VM</span></span>

<span data-ttu-id="fe093-104">Tato příručka podrobně popisuje pomocí rozhraní příkazového řádku Azure toodeploy hello virtuální počítač Azure z hello [image Galerie marketplace Oracle](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) v pořadí toocreate databázi Oracle 12 c.</span><span class="sxs-lookup"><span data-stu-id="fe093-104">This guide details using hello Azure CLI toodeploy an Azure virtual machine from hello [Oracle marketplace gallery image](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) in order toocreate an Oracle 12c database.</span></span> <span data-ttu-id="fe093-105">Po hello server nasazen, bude v databázi Oracle hello tooconfigure pořadí připojit pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="fe093-105">Once hello server is deployed, you will connect via SSH in order tooconfigure hello Oracle database.</span></span> 

<span data-ttu-id="fe093-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="fe093-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fe093-107">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fe093-107">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="fe093-108">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="fe093-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="fe093-109">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fe093-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="fe093-110">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fe093-110">Create a resource group</span></span>

<span data-ttu-id="fe093-111">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe093-111">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="fe093-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="fe093-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="fe093-113">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="fe093-113">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a><span data-ttu-id="fe093-114">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe093-114">Create virtual machine</span></span>

<span data-ttu-id="fe093-115">toocreate virtuální počítač (VM), použijte hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe093-115">toocreate a virtual machine (VM), use hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="fe093-116">Hello následující příklad vytvoří virtuální počítač s názvem `myVM`.</span><span class="sxs-lookup"><span data-stu-id="fe093-116">hello following example creates a VM named `myVM`.</span></span> <span data-ttu-id="fe093-117">Pokud už neexistují v umístění klíče výchozí klíče SSH, vytvoří se také.</span><span class="sxs-lookup"><span data-stu-id="fe093-117">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="fe093-118">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="fe093-118">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="fe093-119">Po vytvoření hello virtuálních počítačů Azure CLI zobrazí informace podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="fe093-119">After you create hello VM, Azure CLI displays information similar toohello following example.</span></span> <span data-ttu-id="fe093-120">Poznamenejte si hodnotu hello pro `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="fe093-120">Note hello value for `publicIpAddress`.</span></span> <span data-ttu-id="fe093-121">Můžete použít tuto adresu tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe093-121">You use this address tooaccess hello VM.</span></span>

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

## <a name="connect-toohello-vm"></a><span data-ttu-id="fe093-122">Připojit toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fe093-122">Connect toohello VM</span></span>

<span data-ttu-id="fe093-123">toocreate na relace SSH s hello virtuálních počítačů, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fe093-123">toocreate an SSH session with hello VM, use hello following command.</span></span> <span data-ttu-id="fe093-124">Nahraďte IP adresu hello hello `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fe093-124">Replace hello IP address with hello `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="create-hello-database"></a><span data-ttu-id="fe093-125">Vytvoření databáze hello</span><span class="sxs-lookup"><span data-stu-id="fe093-125">Create hello database</span></span>

<span data-ttu-id="fe093-126">Hello Oracle softwaru je již nainstalován na bitovou kopii hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fe093-126">hello Oracle software is already installed on hello Marketplace image.</span></span> <span data-ttu-id="fe093-127">Vytvoření ukázkové databáze následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fe093-127">Create a sample database as follows.</span></span> 

1.  <span data-ttu-id="fe093-128">Přepínač toohello *oracle* superuživatele a pak inicializovat naslouchací proces hello protokolování:</span><span class="sxs-lookup"><span data-stu-id="fe093-128">Switch toohello *oracle* superuser, then initialize hello listener for logging:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    <span data-ttu-id="fe093-129">výstup Hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="fe093-129">hello output is similar toohello following:</span></span>

    ```bash
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

2.  <span data-ttu-id="fe093-130">Vytvořte databázi hello:</span><span class="sxs-lookup"><span data-stu-id="fe093-130">Create hello database:</span></span>

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

    <span data-ttu-id="fe093-131">Jak dlouho trvá několik minut toocreate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="fe093-131">It takes a few minutes toocreate hello database.</span></span>

3. <span data-ttu-id="fe093-132">Nastavení proměnných Oracle</span><span class="sxs-lookup"><span data-stu-id="fe093-132">Set Oracle variables</span></span>

<span data-ttu-id="fe093-133">Než připojíte, je nutné, aby proměnné prostředí tooset dva: *ORACLE_HOME* a *ORACLE_SID*.</span><span class="sxs-lookup"><span data-stu-id="fe093-133">Before you connect, you need tooset two environment variables: *ORACLE_HOME* and *ORACLE_SID*.</span></span>

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
<span data-ttu-id="fe093-134">Také můžete přidat ORACLE_HOME a ORACLE_SID soubor .bashrc toohello proměnné.</span><span class="sxs-lookup"><span data-stu-id="fe093-134">You also can add ORACLE_HOME and ORACLE_SID variables toohello .bashrc file.</span></span> <span data-ttu-id="fe093-135">To by uložit hello proměnných prostředí pro budoucí přihlášení. Potvrďte hello následující příkazy byly přidány toohello `~/.bashrc` soubor pomocí zvoleného editoru.</span><span class="sxs-lookup"><span data-stu-id="fe093-135">This would save hello environment variables for future sign-ins. Confirm hello following statements have been added toohello `~/.bashrc` file using editor of your choice.</span></span>

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a><span data-ttu-id="fe093-136">Připojení Oracle EM Express</span><span class="sxs-lookup"><span data-stu-id="fe093-136">Oracle EM Express connectivity</span></span>

<span data-ttu-id="fe093-137">Pro nástroj pro správu grafického uživatelského rozhraní, kterou můžete použít tooexplore hello databázi, nastavte Oracle EM Express.</span><span class="sxs-lookup"><span data-stu-id="fe093-137">For a GUI management tool that you can use tooexplore hello database, set up Oracle EM Express.</span></span> <span data-ttu-id="fe093-138">tooconnect tooOracle EM Express, musíte nejprve nastavit až hello port v Oracle.</span><span class="sxs-lookup"><span data-stu-id="fe093-138">tooconnect tooOracle EM Express, you must first set up hello port in Oracle.</span></span> 

1. <span data-ttu-id="fe093-139">Připojte databáze tooyour pomocí sqlplus:</span><span class="sxs-lookup"><span data-stu-id="fe093-139">Connect tooyour database using sqlplus:</span></span>

    ```bash
    sqlplus / as sysdba
    ```

2. <span data-ttu-id="fe093-140">Po připojení nastavte hello port 5502 pro expresní EM</span><span class="sxs-lookup"><span data-stu-id="fe093-140">Once connected, set hello port 5502 for EM Express</span></span>

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. <span data-ttu-id="fe093-141">Otevřete hello kontejneru PDB1 není-li již otevřeného, ale první zkontrolujte stav hello:</span><span class="sxs-lookup"><span data-stu-id="fe093-141">Open hello container PDB1 if not already opened, but first check hello status:</span></span>

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    <span data-ttu-id="fe093-142">výstup Hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="fe093-142">hello output is similar toohello following:</span></span>

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. <span data-ttu-id="fe093-143">Pokud hello OPEN_MODE pro `PDB1` není ČÍST zápisu, spusťte hello tady příkazy tooopen PDB1:</span><span class="sxs-lookup"><span data-stu-id="fe093-143">If hello OPEN_MODE for `PDB1` is not READ WRITE, then run hello followings commands tooopen PDB1:</span></span>

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

<span data-ttu-id="fe093-144">Je třeba tootype `quit` tooend hello sqlplus relace a typ `exit` toologout hello oracle uživatele.</span><span class="sxs-lookup"><span data-stu-id="fe093-144">You need tootype `quit` tooend hello sqlplus session and type `exit` toologout of hello oracle user.</span></span>

## <a name="automate-database-startup-and-shutdown"></a><span data-ttu-id="fe093-145">Automatizovat databáze spuštění a vypnutí</span><span class="sxs-lookup"><span data-stu-id="fe093-145">Automate database startup and shutdown</span></span>

<span data-ttu-id="fe093-146">Oracle database Hello ve výchozím nastavení není spustit automaticky při restartování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe093-146">hello Oracle database by default doesn't automatically start when you restart hello VM.</span></span> <span data-ttu-id="fe093-147">tooset až toostart databáze Oracle hello automaticky, nejprve Přihlaste se jako kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="fe093-147">tooset up hello Oracle database toostart automatically, first sign in as root.</span></span> <span data-ttu-id="fe093-148">Pak vytvořte a aktualizovat některé soubory systému.</span><span class="sxs-lookup"><span data-stu-id="fe093-148">Then, create and update some system files.</span></span>

1. <span data-ttu-id="fe093-149">Přihlaste se jako kořenového příkazu.</span><span class="sxs-lookup"><span data-stu-id="fe093-149">Sign on as root</span></span>
    ```bash
    sudo su -
    ```

2.  <span data-ttu-id="fe093-150">Pomocí oblíbeného editoru, upravte soubor hello `/etc/oratab` a změnit výchozí nastavení hello `N` příliš`Y`:</span><span class="sxs-lookup"><span data-stu-id="fe093-150">Using your favorite editor, edit hello file `/etc/oratab` and change hello default `N` too`Y`:</span></span>

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  <span data-ttu-id="fe093-151">Vytvořte soubor s názvem `/etc/init.d/dbora` a hello vložte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="fe093-151">Create a file named `/etc/init.d/dbora` and paste hello following contents:</span></span>

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME toobe equivalent too$ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  <span data-ttu-id="fe093-152">Změna oprávnění u souborů s *chmod* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe093-152">Change permissions on files with *chmod* as follows:</span></span>

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  <span data-ttu-id="fe093-153">Vytvořte symbolické odkazy pro spuštění a vypnutí takto:</span><span class="sxs-lookup"><span data-stu-id="fe093-153">Create symbolic links for startup and shutdown as follows:</span></span>

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  <span data-ttu-id="fe093-154">tootest změny, restartujte hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="fe093-154">tootest your changes, restart hello VM:</span></span>

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a><span data-ttu-id="fe093-155">Otevřené porty pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="fe093-155">Open ports for connectivity</span></span>

<span data-ttu-id="fe093-156">Poslední úloha Hello je tooconfigure některé externí koncové body.</span><span class="sxs-lookup"><span data-stu-id="fe093-156">hello final task is tooconfigure some external endpoints.</span></span> <span data-ttu-id="fe093-157">tooset až hello skupinu zabezpečení sítě Azure, který chrání hello virtuálních počítačů, nejprve ukončete relaci SSH v hello virtuálních počítačů (by měl mít byla spuštěna z SSH při restartování v předchozím kroku).</span><span class="sxs-lookup"><span data-stu-id="fe093-157">tooset up hello Azure Network Security Group that protects hello VM, first exit your SSH session in hello VM (should have been kicked out of SSH when rebooting in previous step).</span></span> 

1.  <span data-ttu-id="fe093-158">koncový bod hello tooopen pomocí databáze Oracle hello tooaccess vzdáleně, vytvořit skupinu zabezpečení sítě pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe093-158">tooopen hello endpoint that you use tooaccess hello Oracle database remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span> 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  <span data-ttu-id="fe093-159">koncový bod hello tooopen vzdáleně, použijte tooaccess Oracle EM Express vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe093-159">tooopen hello endpoint that you use tooaccess Oracle EM Express remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span>

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. <span data-ttu-id="fe093-160">V případě potřeby získat hello veřejnou IP adresu vašeho virtuálního počítače znovu s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe093-160">If needed, obtain hello public IP address of your VM again with [az network public-ip show](/cli/azure/network/public-ip#show) as follows:</span></span>

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  <span data-ttu-id="fe093-161">Připojte EM Express z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fe093-161">Connect EM Express from your browser.</span></span> <span data-ttu-id="fe093-162">Ověřte, zda váš prohlížeč je kompatibilní s EM Express (je vyžadována instalace Flash):</span><span class="sxs-lookup"><span data-stu-id="fe093-162">Make sure your browser is compatible with EM Express (Flash install is required):</span></span> 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

<span data-ttu-id="fe093-163">Můžete se přihlásit pomocí hello **SYS** účtu a zkontrolujte, zda text hello **jako sysdba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="fe093-163">You can log in by using hello **SYS** account, and check hello **as sysdba** checkbox.</span></span> <span data-ttu-id="fe093-164">Použití hello heslo **OraPasswd1** nastavený během instalace.</span><span class="sxs-lookup"><span data-stu-id="fe093-164">Use hello password **OraPasswd1** that you set during installation.</span></span> 

![Snímek obrazovky hello Oracle OEM Express přihlašovací stránky](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a><span data-ttu-id="fe093-166">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="fe093-166">Clean up resources</span></span>

<span data-ttu-id="fe093-167">Po dokončení zkoumat první databáze Oracle na Azure a hello virtuálního počítače již nepotřebujete, můžete hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="fe093-167">Once you have finished exploring your first Oracle database on Azure and hello VM is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="fe093-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe093-168">Next steps</span></span>

<span data-ttu-id="fe093-169">Další informace o dalších [Oracle řešení v Azure](oracle-considerations.md).</span><span class="sxs-lookup"><span data-stu-id="fe093-169">Learn about other [Oracle solutions on Azure](oracle-considerations.md).</span></span> 

<span data-ttu-id="fe093-170">Zkuste hello [instalace a konfigurace Oracle automatizované úložiště správy](configure-oracle-asm.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="fe093-170">Try hello [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.</span></span>
