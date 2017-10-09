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
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Vytvoření databáze Oracle na virtuálním počítači Azure

Tato příručka podrobně popisuje pomocí rozhraní příkazového řádku Azure toodeploy hello virtuální počítač Azure z hello [image Galerie marketplace Oracle](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) v pořadí toocreate databázi Oracle 12 c. Po hello server nasazen, bude v databázi Oracle hello tooconfigure pořadí připojit pomocí protokolu SSH. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

toocreate virtuální počítač (VM), použijte hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. 

Hello následující příklad vytvoří virtuální počítač s názvem `myVM`. Pokud už neexistují v umístění klíče výchozí klíče SSH, vytvoří se také. toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

Po vytvoření hello virtuálních počítačů Azure CLI zobrazí informace podobné toohello následující ukázka. Poznamenejte si hodnotu hello pro `publicIpAddress`. Můžete použít tuto adresu tooaccess hello virtuálních počítačů.

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

## <a name="connect-toohello-vm"></a>Připojit toohello virtuálních počítačů

toocreate na relace SSH s hello virtuálních počítačů, použijte následující příkaz hello. Nahraďte IP adresu hello hello `publicIpAddress` hodnotu pro virtuální počítač.

```bash 
ssh <publicIpAddress>
```

## <a name="create-hello-database"></a>Vytvoření databáze hello

Hello Oracle softwaru je již nainstalován na bitovou kopii hello Marketplace. Vytvoření ukázkové databáze následujícím způsobem. 

1.  Přepínač toohello *oracle* superuživatele a pak inicializovat naslouchací proces hello protokolování:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    výstup Hello je podobné toohello následující:

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

2.  Vytvořte databázi hello:

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

    Jak dlouho trvá několik minut toocreate hello databáze.

3. Nastavení proměnných Oracle

Než připojíte, je nutné, aby proměnné prostředí tooset dva: *ORACLE_HOME* a *ORACLE_SID*.

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
Také můžete přidat ORACLE_HOME a ORACLE_SID soubor .bashrc toohello proměnné. To by uložit hello proměnných prostředí pro budoucí přihlášení. Potvrďte hello následující příkazy byly přidány toohello `~/.bashrc` soubor pomocí zvoleného editoru.

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a>Připojení Oracle EM Express

Pro nástroj pro správu grafického uživatelského rozhraní, kterou můžete použít tooexplore hello databázi, nastavte Oracle EM Express. tooconnect tooOracle EM Express, musíte nejprve nastavit až hello port v Oracle. 

1. Připojte databáze tooyour pomocí sqlplus:

    ```bash
    sqlplus / as sysdba
    ```

2. Po připojení nastavte hello port 5502 pro expresní EM

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. Otevřete hello kontejneru PDB1 není-li již otevřeného, ale první zkontrolujte stav hello:

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    výstup Hello je podobné toohello následující:

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. Pokud hello OPEN_MODE pro `PDB1` není ČÍST zápisu, spusťte hello tady příkazy tooopen PDB1:

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

Je třeba tootype `quit` tooend hello sqlplus relace a typ `exit` toologout hello oracle uživatele.

## <a name="automate-database-startup-and-shutdown"></a>Automatizovat databáze spuštění a vypnutí

Oracle database Hello ve výchozím nastavení není spustit automaticky při restartování hello virtuálních počítačů. tooset až toostart databáze Oracle hello automaticky, nejprve Přihlaste se jako kořenový adresář. Pak vytvořte a aktualizovat některé soubory systému.

1. Přihlaste se jako kořenového příkazu.
    ```bash
    sudo su -
    ```

2.  Pomocí oblíbeného editoru, upravte soubor hello `/etc/oratab` a změnit výchozí nastavení hello `N` příliš`Y`:

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Vytvořte soubor s názvem `/etc/init.d/dbora` a hello vložte následující obsah:

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

4.  Změna oprávnění u souborů s *chmod* následujícím způsobem:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Vytvořte symbolické odkazy pro spuštění a vypnutí takto:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  tootest změny, restartujte hello virtuálních počítačů:

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a>Otevřené porty pro připojení k síti

Poslední úloha Hello je tooconfigure některé externí koncové body. tooset až hello skupinu zabezpečení sítě Azure, který chrání hello virtuálních počítačů, nejprve ukončete relaci SSH v hello virtuálních počítačů (by měl mít byla spuštěna z SSH při restartování v předchozím kroku). 

1.  koncový bod hello tooopen pomocí databáze Oracle hello tooaccess vzdáleně, vytvořit skupinu zabezpečení sítě pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) následujícím způsobem: 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  koncový bod hello tooopen vzdáleně, použijte tooaccess Oracle EM Express vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) následujícím způsobem:

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. V případě potřeby získat hello veřejnou IP adresu vašeho virtuálního počítače znovu s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show) následujícím způsobem:

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  Připojte EM Express z prohlížeče. Ověřte, zda váš prohlížeč je kompatibilní s EM Express (je vyžadována instalace Flash): 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

Můžete se přihlásit pomocí hello **SYS** účtu a zkontrolujte, zda text hello **jako sysdba** zaškrtávací políčko. Použití hello heslo **OraPasswd1** nastavený během instalace. 

![Snímek obrazovky hello Oracle OEM Express přihlašovací stránky](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Po dokončení zkoumat první databáze Oracle na Azure a hello virtuálního počítače již nepotřebujete, můžete hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

Další informace o dalších [Oracle řešení v Azure](oracle-considerations.md). 

Zkuste hello [instalace a konfigurace Oracle automatizované úložiště správy](configure-oracle-asm.md) kurzu.
