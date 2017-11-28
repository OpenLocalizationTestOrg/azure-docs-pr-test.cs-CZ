---
title: "aaaImplement Oracle Data Guard na virtuální počítač Azure s Linuxem | Microsoft Docs"
description: "Rychle získáte Oracle Data Guard nahoru a spouštění v prostředí Azure."
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
ms.date: 05/10/2017
ms.author: rclaus
ms.openlocfilehash: 101196b2f50dfca64d3eb1b4be56ff0c108693e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-azure-linux-vm"></a><span data-ttu-id="980dc-103">Implementace Oracle Data Guard na virtuální počítač Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="980dc-103">Implement Oracle Data Guard on Azure Linux VM</span></span> 

<span data-ttu-id="980dc-104">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="980dc-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="980dc-105">Tento průvodce údaje, pomocí rozhraní příkazového řádku Azure toodeploy hello Oracle 12c databáze z image Galerie Marketplace hello.</span><span class="sxs-lookup"><span data-stu-id="980dc-105">This guide details using hello Azure CLI toodeploy an Oracle 12c Database from hello Marketplace gallery image.</span></span> <span data-ttu-id="980dc-106">Po vytvoření databáze Oracle hello tohoto dokumentu se dozvíte, jak podrobné tooinstall a nakonfigurovat ochranu dat na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="980dc-106">Once hello Oracle database is created, this document shows you step-by-step how tooinstall and configure Data Guard on Azure VM.</span></span>

<span data-ttu-id="980dc-107">Než začnete, ujistěte se, že byla nainstalována rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="980dc-107">Before you start, make sure that hello Azure CLI has been installed.</span></span> <span data-ttu-id="980dc-108">Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="980dc-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="980dc-109">Příprava prostředí hello</span><span class="sxs-lookup"><span data-stu-id="980dc-109">Prepare hello environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="980dc-110">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="980dc-110">Assumptions</span></span>

<span data-ttu-id="980dc-111">hello tooperform Oracle Data Guard nainstalovat, je nutné toocreate dva virtuální počítače Azure na hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="980dc-111">tooperform hello Oracle Data Guard install, you need toocreate two Azure VMs on hello same availability set.</span></span> <span data-ttu-id="980dc-112">Hello Marketplace image použít toocreate hello virtuální počítače je "Oracle: Oracle – databáze-Ee:12.1.0.2:latest".</span><span class="sxs-lookup"><span data-stu-id="980dc-112">hello Marketplace image you use toocreate hello VMs is "Oracle:Oracle-Database-Ee:12.1.0.2:latest".</span></span>

<span data-ttu-id="980dc-113">Hello primárního virtuálního počítače (myVM1) má spuštěnou instanci Oracle.</span><span class="sxs-lookup"><span data-stu-id="980dc-113">hello primary VM (myVM1) has a running Oracle instance.</span></span>

<span data-ttu-id="980dc-114">Hello pohotovostní virtuálních počítačů (Můjvp2) je nainstalován pouze software Oracle hello.</span><span class="sxs-lookup"><span data-stu-id="980dc-114">hello standby VM (myVM2) has hello Oracle software installed only.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="980dc-115">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="980dc-115">Log in tooAzure</span></span> 

<span data-ttu-id="980dc-116">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="980dc-116">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="980dc-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="980dc-117">Create a resource group</span></span>

<span data-ttu-id="980dc-118">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="980dc-118">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="980dc-119">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="980dc-119">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="980dc-120">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění.</span><span class="sxs-lookup"><span data-stu-id="980dc-120">hello following example creates a resource group named `myResourceGroup` in hello `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-availability-set"></a><span data-ttu-id="980dc-121">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="980dc-121">Create availability set</span></span>

<span data-ttu-id="980dc-122">Tento krok je volitelný, ale doporučuje se.</span><span class="sxs-lookup"><span data-stu-id="980dc-122">This step is optional, but is recommended.</span></span> <span data-ttu-id="980dc-123">v tématu [Azure dostupnosti nastaví průvodce](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) Další informace.</span><span class="sxs-lookup"><span data-stu-id="980dc-123">see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) for more information.</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-virtual-machine"></a><span data-ttu-id="980dc-124">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="980dc-124">Create virtual machine</span></span>

<span data-ttu-id="980dc-125">Vytvoření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="980dc-125">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="980dc-126">Hello následující příklad vytvoří 2 virtuální počítače s názvem `myVM1` a `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="980dc-126">hello following example creates 2 VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="980dc-127">Pokud už neexistují v umístění klíče výchozí, vytvoří klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="980dc-127">Creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="980dc-128">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="980dc-128">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

<span data-ttu-id="980dc-129">Vytvoření myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="980dc-129">Create myVM1 (primary)</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

<span data-ttu-id="980dc-130">Jednou hello vytvořil virtuální počítač, hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka: Poznamenejte si hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="980dc-130">Once hello VM has been created, hello Azure CLI shows information similar toohello following example: Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="980dc-131">Tato adresa je použité tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="980dc-131">This address is used tooaccess hello VM.</span></span>

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

<span data-ttu-id="980dc-132">Vytvoření Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="980dc-132">Create myVM2 (standby)</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

<span data-ttu-id="980dc-133">Poznamenejte si hello `publicIpAddress` i po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="980dc-133">Take note of hello `publicIpAddress` as well once it created.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="980dc-134">Otevřete port TCP hello pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="980dc-134">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="980dc-135">Krok Hello je tooconfigure externí koncové body, které umožňuje vzdálený přístup k hello Oracle DB, spustit následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="980dc-135">hello step is tooconfigure external endpoints, which allows accessing hello Oracle DB remotely, you execute hello following command.</span></span>

<span data-ttu-id="980dc-136">Otevřete port pro myVM1</span><span class="sxs-lookup"><span data-stu-id="980dc-136">Open port for myVM1</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVmN1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="980dc-137">Výsledek by měl vypadat podobně jako toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="980dc-137">Result should look similar toohello following response:</span></span>

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

<span data-ttu-id="980dc-138">Otevřete port pro Můjvp2</span><span class="sxs-lookup"><span data-stu-id="980dc-138">Open port for myVM2</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2N1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toovirtual-machine"></a><span data-ttu-id="980dc-139">Připojte počítač toovirtual</span><span class="sxs-lookup"><span data-stu-id="980dc-139">Connect toovirtual machine</span></span>

<span data-ttu-id="980dc-140">Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="980dc-140">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="980dc-141">Nahraďte IP adresu hello hello `publicIpAddress` virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="980dc-141">Replace hello IP address with hello `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh azureuser@<publicIpAddress>
```

### <a name="create-database-on-myvm1-primary"></a><span data-ttu-id="980dc-142">Vytvořit databázi na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="980dc-142">Create Database on myVM1 (Primary)</span></span>

<span data-ttu-id="980dc-143">Hello Oracle softwaru je již nainstalován na bitovou kopii Marketplace hello, takže hello dalším krokem je tooinstall hello databáze.</span><span class="sxs-lookup"><span data-stu-id="980dc-143">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> <span data-ttu-id="980dc-144">prvním krokem Hello běží jako superuživatele, oracle, hello.</span><span class="sxs-lookup"><span data-stu-id="980dc-144">hello first step is running as hello 'oracle' superuser.</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="980dc-145">Vytvořte databázi hello:</span><span class="sxs-lookup"><span data-stu-id="980dc-145">create hello database:</span></span>

```bash
$ dbca -silent \
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
<span data-ttu-id="980dc-146">Výstupy by měl vypadat podobně jako toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="980dc-146">Outputs should look similar toohello following response:</span></span>

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
```

<span data-ttu-id="980dc-147">Nastavení proměnných ORACLE_SID a ORACLE_HOME hello</span><span class="sxs-lookup"><span data-stu-id="980dc-147">Set hello ORACLE_SID and ORACLE_HOME variables</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="980dc-148">Volitelně můžete přidaný ORACLE_HOME a ORACLE_SID toohello .bashrc soubor, takže tato nastavení se uloží pro budoucí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="980dc-148">Optionally, You can added ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future logins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="data-guard-configurations"></a><span data-ttu-id="980dc-149">Konfigurace ochrana dat</span><span class="sxs-lookup"><span data-stu-id="980dc-149">Data Guard Configurations</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="980dc-150">Povolit režim protokolu archiv na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="980dc-150">Enable Archive log mode on myVM1 (primary)</span></span>

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
```
<span data-ttu-id="980dc-151">Povolit protokolování platnost a ujistěte se, že je k dispozici alespoň jeden soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="980dc-151">Enable force logging, and make sure at least one logfile is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="980dc-152">Vytvoření pohotovostní opakování protokoly</span><span class="sxs-lookup"><span data-stu-id="980dc-152">Create standby redo logs</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="980dc-153">Zapnout Flashback (který provedli obnovení hello mnohem dříve) a nastavte STANDBY_FILE_MANAGEMENT tooauto</span><span class="sxs-lookup"><span data-stu-id="980dc-153">Turn on Flashback (which made hello recovery a lot earlier) and set STANDBY_FILE_MANAGEMENT tooauto</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
```

### <a name="service-setup-on-myvm1-primary"></a><span data-ttu-id="980dc-154">Instalační program služby na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="980dc-154">Service setup on myVM1 (primary)</span></span>

<span data-ttu-id="980dc-155">Upravovat nebo vytvářet hello tnsnames.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="980dc-155">Edit or create hello tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="980dc-156">Přidejte následující položky hello</span><span class="sxs-lookup"><span data-stu-id="980dc-156">Add hello following entries</span></span>

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

<span data-ttu-id="980dc-157">Upravovat nebo vytvářet hello listener.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="980dc-157">Edit or create hello listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="980dc-158">Přidejte následující položky hello</span><span class="sxs-lookup"><span data-stu-id="980dc-158">Add hello following entries</span></span>

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

<span data-ttu-id="980dc-159">Spuštění nástroje pro sledování hello</span><span class="sxs-lookup"><span data-stu-id="980dc-159">Start hello listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="service-setup-on-myvm2-standby"></a><span data-ttu-id="980dc-160">Instalační program služby na Můjvp2 (Úsporný režim)</span><span class="sxs-lookup"><span data-stu-id="980dc-160">Service setup on myVM2 (Standby)</span></span>

<span data-ttu-id="980dc-161">Upravovat nebo vytvářet hello tnsnames.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="980dc-161">Edit or create hello tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="980dc-162">Přidejte následující položky hello</span><span class="sxs-lookup"><span data-stu-id="980dc-162">Add hello following entries</span></span>

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

<span data-ttu-id="980dc-163">Upravovat nebo vytvářet hello listener.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="980dc-163">Edit or create hello listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="980dc-164">Přidejte následující položky hello</span><span class="sxs-lookup"><span data-stu-id="980dc-164">Add hello following entries</span></span>

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

<span data-ttu-id="980dc-165">Spuštění nástroje pro sledování hello</span><span class="sxs-lookup"><span data-stu-id="980dc-165">Start hello listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

<span data-ttu-id="980dc-166">Povolit ochranu zprostředkovatele dat</span><span class="sxs-lookup"><span data-stu-id="980dc-166">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="restore-database-toomyvm2-standby"></a><span data-ttu-id="980dc-167">Obnovení databáze toomyVM2 (Úsporný režim)</span><span class="sxs-lookup"><span data-stu-id="980dc-167">Restore database toomyVM2 (Standby)</span></span>

<span data-ttu-id="980dc-168">Vytvořte soubor s parametry, nebo tmp/initcdb1_stby.ora' s obsahem tady hello</span><span class="sxs-lookup"><span data-stu-id="980dc-168">Create a parameter file '/tmp/initcdb1_stby.ora' with hello followings contents</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="980dc-169">Vytváření složek</span><span class="sxs-lookup"><span data-stu-id="980dc-169">Create folders</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="980dc-170">Vytvoření souboru heslo</span><span class="sxs-lookup"><span data-stu-id="980dc-170">Create password file</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0.2/db_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="980dc-171">Spustit zálohu databáze na Můjvp2</span><span class="sxs-lookup"><span data-stu-id="980dc-171">Start up database on myVM2</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="980dc-172">Obnovte databázi pomocí RMAN nástroj</span><span class="sxs-lookup"><span data-stu-id="980dc-172">Restore database using RMAN utility</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="980dc-173">Spustit následující příkazy v RMAN hello</span><span class="sxs-lookup"><span data-stu-id="980dc-173">Execute hello following commands in RMAN</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="980dc-174">Povolit ochranu zprostředkovatele dat</span><span class="sxs-lookup"><span data-stu-id="980dc-174">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="980dc-175">Konfigurace zprostředkovatele Data Guard na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="980dc-175">Configure Data Guard broker on myVM1 (primary)</span></span>

<span data-ttu-id="980dc-176">Spusťte správce Data Guard hello a přihlaste se pomocí SYS a hesla (do nepoužíváte ověřování operačního systému).</span><span class="sxs-lookup"><span data-stu-id="980dc-176">Start hello Data Guard manager and login using SYS and password (do not using OS authentication).</span></span> <span data-ttu-id="980dc-177">Provedení těchto hodnot hello</span><span class="sxs-lookup"><span data-stu-id="980dc-177">Perform hello followings</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> CREATE CONFIGURATION my_dg_config AS PRIMARY DATABASE IS cdb1 CONNECT IDENTIFIER IS cdb1;
Configuration "my_dg_config" created with primary database "cdb1"
DGMGRL> ADD DATABASE cdb1_stby AS CONNECT IDENTIFIER IS cdb1_stby MAINTAINED AS PHYSICAL;
Database "cdb1_stby" added
DGMGRL> ENABLE CONFIGURATION;
Enabled.
```

<span data-ttu-id="980dc-178">Zkontrolujte konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="980dc-178">Review hello configuration</span></span>
```bash
DGMGRL> SHOW CONFIGURATION;

Configuration - my_dg_config

  Protection Mode: MaxPerformance
  Members:
  cdb1      - Primary database
    cdb1_stby - Physical standby database

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS   (status updated 26 seconds ago)
```

<span data-ttu-id="980dc-179">To hello Oracle Data Guard instalace dokončena.</span><span class="sxs-lookup"><span data-stu-id="980dc-179">This completed hello Oracle Data Guard setup.</span></span> <span data-ttu-id="980dc-180">Hello další části se dozvíte, jak tootest hello připojení a přepínání</span><span class="sxs-lookup"><span data-stu-id="980dc-180">hello next section shows you how tootest hello connectivity and switching over</span></span>

### <a name="connect-database-from-client-machine"></a><span data-ttu-id="980dc-181">Připojit databáze z klientského počítače</span><span class="sxs-lookup"><span data-stu-id="980dc-181">Connect database from client machine</span></span>

<span data-ttu-id="980dc-182">Aktualizujte nebo vytvoření souboru tnsnames.ora hello do klientského počítače, která se obvykle nachází v $ORACLE_HOME\network\admin.</span><span class="sxs-lookup"><span data-stu-id="980dc-182">Update or create hello tnsnames.ora file on your client machine which usually is located at $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="980dc-183">Nahraďte IP hello s vaší `publicIpAddress` myVM1 a Můjvp2</span><span class="sxs-lookup"><span data-stu-id="980dc-183">Replace hello IP with your `publicIpAddress` for myVM1 and myVM2</span></span>

```bash
cdb1=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM1 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1)
    )
  )

cdb1_stby=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM2 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1_stby)
    )
  )
```

<span data-ttu-id="980dc-184">Spustit sqlplus</span><span class="sxs-lookup"><span data-stu-id="980dc-184">Start sqlplus</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-data-guard-configuration"></a><span data-ttu-id="980dc-185">Konfigurace testovací Data Guard</span><span class="sxs-lookup"><span data-stu-id="980dc-185">Test Data Guard configuration</span></span>

### <a name="database-switchover-on-myvm1-primary"></a><span data-ttu-id="980dc-186">Databáze přepnutí na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="980dc-186">Database switchover on myVM1 (primary)</span></span>

<span data-ttu-id="980dc-187">tooswitch z primární toostandby (cdb1 toocdb1_stby)</span><span class="sxs-lookup"><span data-stu-id="980dc-187">tooswitch from primary toostandby (cdb1 toocdb1_stby)</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1_stby;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1_stby"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1_stby" is opening...
Operation requires start up of instance "cdb1" on database "cdb1"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1_stby"
DGMGRL>
```

<span data-ttu-id="980dc-188">Nyní byste měli mít tooconnect toohello pohotovostní databáze</span><span class="sxs-lookup"><span data-stu-id="980dc-188">You should now be able tooconnect toohello standby database</span></span>

<span data-ttu-id="980dc-189">Spustit sqlplus</span><span class="sxs-lookup"><span data-stu-id="980dc-189">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="database-switch-back-on-myvm2-standby"></a><span data-ttu-id="980dc-190">Parametr databáze zpět na Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="980dc-190">Database switch back on myVM2 (standby)</span></span>

<span data-ttu-id="980dc-191">tooswitch zpět, spusťte na Můjvp2 tady hello</span><span class="sxs-lookup"><span data-stu-id="980dc-191">tooswitch back, run hello followings on myVM2</span></span>
```bash
$ dgmgrl sys/OraPasswd1@cdb1_stby
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1" is opening...
Operation requires start up of instance "cdb1" on database "cdb1_stby"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1"
```

<span data-ttu-id="980dc-192">Znovu nyní byste měli být schopný tooconnect toohello primární databáze</span><span class="sxs-lookup"><span data-stu-id="980dc-192">Once again, You should now be able tooconnect toohello primary database</span></span>

<span data-ttu-id="980dc-193">Spustit sqlplus</span><span class="sxs-lookup"><span data-stu-id="980dc-193">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="980dc-194">To dokončit hello instalaci a konfiguraci ochrana dat v systému Oracle linux.</span><span class="sxs-lookup"><span data-stu-id="980dc-194">This completed hello installation and configuration of Data Guard on Oracle linux.</span></span>


## <a name="delete-virtual-machine"></a><span data-ttu-id="980dc-195">Odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="980dc-195">Delete virtual machine</span></span>

<span data-ttu-id="980dc-196">Pokud již nepotřebujete, hello následující příkaz lze použít tooremove hello skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="980dc-196">When no longer needed, hello following command can be used tooremove hello Resource Group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="980dc-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="980dc-197">Next steps</span></span>

[<span data-ttu-id="980dc-198">Kurz vytvoření virtuálního počítače s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="980dc-198">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="980dc-199">Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="980dc-199">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
