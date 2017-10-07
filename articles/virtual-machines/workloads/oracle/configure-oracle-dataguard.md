---
title: "aaaImplement Oracle Data Guard na virtuálním počítači Azure Linux | Microsoft Docs"
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
ms.openlocfilehash: 6bb530098737e3ca7dd8bab3f4306ecbb620f3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="5a3cb-103">Implementace Oracle Data Guard na virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="5a3cb-103">Implement Oracle Data Guard on an Azure Linux virtual machine</span></span> 

<span data-ttu-id="5a3cb-104">Rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-104">Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="5a3cb-105">Tento článek popisuje, jak toodeploy toouse rozhraní příkazového řádku Azure k databázi Oracle 12c databáze z Azure Marketplace hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-105">This article describes how toouse Azure CLI toodeploy an Oracle Database 12c database from hello Azure Marketplace image.</span></span> <span data-ttu-id="5a3cb-106">Tento článek pak ukazuje, krok za krokem, jak tooinstall a nakonfigurovat ochranu dat na virtuální počítač Azure (VM).</span><span class="sxs-lookup"><span data-stu-id="5a3cb-106">This article then shows you, step by step, how tooinstall and configure Data Guard on an Azure virtual machine (VM).</span></span>

<span data-ttu-id="5a3cb-107">Než začnete, ujistěte se, že je nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-107">Before you start, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="5a3cb-108">Další informace najdete v tématu hello [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5a3cb-108">For more information, see hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="5a3cb-109">Příprava prostředí hello</span><span class="sxs-lookup"><span data-stu-id="5a3cb-109">Prepare hello environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="5a3cb-110">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="5a3cb-110">Assumptions</span></span>

<span data-ttu-id="5a3cb-111">tooinstall Oracle Data Guard, budete potřebovat toocreate dva virtuální počítače Azure na hello stejné skupině dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-111">tooinstall Oracle Data Guard, you need toocreate two Azure VMs on hello same availability set:</span></span>

- <span data-ttu-id="5a3cb-112">Hello primárního virtuálního počítače (myVM1) má spuštěnou instanci Oracle.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-112">hello primary VM (myVM1) has a running Oracle instance.</span></span>
- <span data-ttu-id="5a3cb-113">Hello pohotovostní virtuálních počítačů (Můjvp2) je nainstalován pouze software Oracle hello.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-113">hello standby VM (myVM2) has hello Oracle software installed only.</span></span>

<span data-ttu-id="5a3cb-114">Hello Marketplace image použít toocreate hello virtuálních počítačů je Oracle: Oracle – databáze-Ee:12.1.0.2:latest.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-114">hello Marketplace image that you use toocreate hello VMs is Oracle:Oracle-Database-Ee:12.1.0.2:latest.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="5a3cb-115">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="5a3cb-115">Sign in tooAzure</span></span> 

<span data-ttu-id="5a3cb-116">Přihlaste se pomocí hello tooyour předplatného Azure [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-116">Sign in tooyour Azure subscription by using hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="5a3cb-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5a3cb-117">Create a resource group</span></span>

<span data-ttu-id="5a3cb-118">Vytvořte skupinu prostředků s použitím hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-118">Create a resource group by using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="5a3cb-119">Skupinu prostředků Azure je logický kontejner, ve které jsou nasazené a spravované prostředky.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-119">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="5a3cb-120">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-120">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="5a3cb-121">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="5a3cb-121">Create an availability set</span></span>

<span data-ttu-id="5a3cb-122">Vytvoření skupiny dostupnosti je nepovinný, ale doporučujeme ho.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-122">Creating an availability set is optional, but we recommend it.</span></span> <span data-ttu-id="5a3cb-123">Další informace najdete v tématu [sady Azure dostupnosti pokyny](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="5a3cb-123">For more information, see [Azure availability sets guidelines](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="5a3cb-124">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5a3cb-124">Create a virtual machine</span></span>

<span data-ttu-id="5a3cb-125">Vytvoření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-125">Create a VM by using hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="5a3cb-126">Hello následující příklad vytvoří dva virtuální počítače s názvem `myVM1` a `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-126">hello following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="5a3cb-127">Pokud už neexistují v umístění klíče výchozí klíče SSH, vytvoří se také.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-127">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="5a3cb-128">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-128">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

<span data-ttu-id="5a3cb-129">Vytvořte myVM1 (primární):</span><span class="sxs-lookup"><span data-stu-id="5a3cb-129">Create myVM1 (primary):</span></span>
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

<span data-ttu-id="5a3cb-130">Po vytvoření hello virtuálních počítačů, rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-130">After you create hello VM, Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="5a3cb-131">Poznamenejte si hodnotu hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-131">Note hello value of `publicIpAddress`.</span></span> <span data-ttu-id="5a3cb-132">Můžete použít tuto adresu tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-132">You use this address tooaccess hello VM.</span></span>

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

<span data-ttu-id="5a3cb-133">Vytvoření Můjvp2 (pohotovostní):</span><span class="sxs-lookup"><span data-stu-id="5a3cb-133">Create myVM2 (standby):</span></span>
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

<span data-ttu-id="5a3cb-134">Poznamenejte si hodnotu hello `publicIpAddress` po vytvoření Můjvp2.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-134">Note hello value of `publicIpAddress` after you create myVM2.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="5a3cb-135">Otevřete port TCP hello pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="5a3cb-135">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="5a3cb-136">Tento krok nakonfiguruje externí koncové body, které umožňují databáze Oracle toohello vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-136">This step configures external endpoints, which allow remote access toohello Oracle database.</span></span>

<span data-ttu-id="5a3cb-137">Otevřete port hello pro myVM1:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-137">Open hello port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="5a3cb-138">výsledek Hello by měl vypadat podobně jako toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-138">hello result should look similar toohello following response:</span></span>

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

<span data-ttu-id="5a3cb-139">Otevřete port hello pro Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-139">Open hello port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="5a3cb-140">Připojit toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5a3cb-140">Connect toohello virtual machine</span></span>

<span data-ttu-id="5a3cb-141">Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-141">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="5a3cb-142">Nahraďte IP adresu hello hello `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-142">Replace hello IP address with hello `publicIpAddress` value for your virtual machine.</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a><span data-ttu-id="5a3cb-143">Vytvořit databázi hello na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-143">Create hello database on myVM1 (primary)</span></span>

<span data-ttu-id="5a3cb-144">Hello Oracle softwaru je již nainstalován na bitovou kopii Marketplace hello, takže hello dalším krokem je tooinstall hello databáze.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-144">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> 

<span data-ttu-id="5a3cb-145">Superuživatel Oracle toohello přepínače:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-145">Switch toohello Oracle superuser:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="5a3cb-146">Vytvořte databázi hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-146">Create hello database:</span></span>

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
<span data-ttu-id="5a3cb-147">Výstupy by měl vypadat podobně jako toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-147">Outputs should look similar toohello following response:</span></span>

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

<span data-ttu-id="5a3cb-148">Nastavení proměnných ORACLE_SID a ORACLE_HOME hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-148">Set hello ORACLE_SID and ORACLE_HOME variables:</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="5a3cb-149">Volitelně můžete přidat ORACLE_HOME a ORACLE_SID toohello /home/oracle/.bashrc souboru, tak, aby tato nastavení se uloží pro budoucí přihlášení:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-149">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a><span data-ttu-id="5a3cb-150">Nakonfigurujte ochranu dat</span><span class="sxs-lookup"><span data-stu-id="5a3cb-150">Configure Data Guard</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="5a3cb-151">Povolit režim protokolu archiv na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-151">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="5a3cb-152">Povolit protokolování platnost a ujistěte se, zda je přítomen alespoň jeden soubor protokolu:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-152">Enable force logging, and make sure at least one log file is present:</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="5a3cb-153">Vytvořte znovu pohotovostní protokoly:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-153">Create standby redo logs:</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="5a3cb-154">Zapnout Flashback (díky čemuž obnovení bylo mnohem snazší) a nastavte pohotovostní\_soubor\_tooauto správy.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-154">Turn on Flashback (which makes recovery a lot easier) and set STANDBY\_FILE\_MANAGEMENT tooauto.</span></span> <span data-ttu-id="5a3cb-155">Ukončete SQL * Plus potom.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-155">Exit SQL*Plus after that.</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="5a3cb-156">Nastavení služby v myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-156">Set up service on myVM1 (primary)</span></span>

<span data-ttu-id="5a3cb-157">Upravovat nebo vytvářet hello tnsnames.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-157">Edit or create hello tnsnames.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="5a3cb-158">Přidejte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-158">Add hello following entries:</span></span>

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

<span data-ttu-id="5a3cb-159">Upravovat nebo vytvářet hello listener.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-159">Edit or create hello listener.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="5a3cb-160">Přidejte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-160">Add hello following entries:</span></span>

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

<span data-ttu-id="5a3cb-161">Povolte zprostředkovatele ochrana dat:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-161">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```
<span data-ttu-id="5a3cb-162">Spuštění nástroje pro sledování hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-162">Start hello listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="set-up-service-on-myvm2-standby"></a><span data-ttu-id="5a3cb-163">Nastavení služby v Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-163">Set up service on myVM2 (standby)</span></span>

<span data-ttu-id="5a3cb-164">SSH toomyVM2:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-164">SSH toomyVM2:</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="5a3cb-165">Přihlaste se jako Oracle:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-165">Log in as Oracle:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="5a3cb-166">Upravovat nebo vytvářet hello tnsnames.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-166">Edit or create hello tnsnames.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="5a3cb-167">Přidejte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-167">Add hello following entries:</span></span>

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

<span data-ttu-id="5a3cb-168">Upravovat nebo vytvářet hello listener.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-168">Edit or create hello listener.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="5a3cb-169">Přidejte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-169">Add hello following entries:</span></span>

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

<span data-ttu-id="5a3cb-170">Spuštění nástroje pro sledování hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-170">Start hello listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```


### <a name="restore-hello-database-toomyvm2-standby"></a><span data-ttu-id="5a3cb-171">Obnovení databáze toomyVM2 hello (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-171">Restore hello database toomyVM2 (standby)</span></span>

<span data-ttu-id="5a3cb-172">Vytvořte hello parametr souboru /tmp/initcdb1_stby.ora s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-172">Create hello parameter file /tmp/initcdb1_stby.ora with hello following contents:</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="5a3cb-173">Vytváření složek:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-173">Create folders:</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="5a3cb-174">Vytvořte soubor heslo:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-174">Create a password file:</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0/dbhome_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="5a3cb-175">Spuštění databáze hello na Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-175">Start hello database on myVM2:</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="5a3cb-176">Obnovení databáze hello pomocí nástroje RMAN hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-176">Restore hello database by using hello RMAN tool:</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="5a3cb-177">Spusťte následující příkazy v RMAN hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-177">Run hello following commands in RMAN:</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="5a3cb-178">Po dokončení příkazu hello byste měli vidět následující toohello podobné zprávy.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-178">You should see messages similar toohello following when hello command is completed.</span></span> <span data-ttu-id="5a3cb-179">Ukončete RMAN.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-179">Exit RMAN.</span></span>
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

<span data-ttu-id="5a3cb-180">Volitelně můžete přidat ORACLE_HOME a ORACLE_SID toohello /home/oracle/.bashrc souboru, tak, aby tato nastavení se uloží pro budoucí přihlášení:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-180">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

<span data-ttu-id="5a3cb-181">Povolte zprostředkovatele ochrana dat:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-181">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="5a3cb-182">Konfigurace zprostředkovatele ochrana dat na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-182">Configure Data Guard Broker on myVM1 (primary)</span></span>

<span data-ttu-id="5a3cb-183">Spusťte správce ochrana dat a přihlaste se pomocí SYS a heslo.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-183">Start Data Guard Manager and log in by using SYS and a password.</span></span> <span data-ttu-id="5a3cb-184">(Nepoužívejte ověřování operačního systému.) Proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-184">(Do not use OS authentication.) Perform hello following:</span></span>

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

<span data-ttu-id="5a3cb-185">Kontrola konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-185">Review hello configuration:</span></span>
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

<span data-ttu-id="5a3cb-186">Po dokončení instalace Oracle Data Guard hello.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-186">You've completed hello Oracle Data Guard setup.</span></span> <span data-ttu-id="5a3cb-187">Hello další části se dozvíte, jak tootest hello připojení a přepínačů.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-187">hello next section shows you how tootest hello connectivity and switch over.</span></span>

### <a name="connect-hello-database-from-hello-client-machine"></a><span data-ttu-id="5a3cb-188">Připojit databáze hello z hello klientský počítač</span><span class="sxs-lookup"><span data-stu-id="5a3cb-188">Connect hello database from hello client machine</span></span>

<span data-ttu-id="5a3cb-189">Aktualizujte nebo vytvoření souboru tnsnames.ora hello v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-189">Update or create hello tnsnames.ora file on your client machine.</span></span> <span data-ttu-id="5a3cb-190">Tento soubor je obvykle v $ORACLE_HOME\network\admin.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-190">This file is usually in $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="5a3cb-191">Nahraďte hello IP adresy s vaší `publicIpAddress` hodnoty myVM1 a Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-191">Replace hello IP addresses with your `publicIpAddress` values for myVM1 and myVM2:</span></span>

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

<span data-ttu-id="5a3cb-192">Spusťte SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-192">Start SQL*Plus:</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-hello-data-guard-configuration"></a><span data-ttu-id="5a3cb-193">Konfigurace testovací hello Data Guard</span><span class="sxs-lookup"><span data-stu-id="5a3cb-193">Test hello Data Guard configuration</span></span>

### <a name="switch-over-hello-database-on-myvm1-primary"></a><span data-ttu-id="5a3cb-194">Přepínač přes hello databáze na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-194">Switch over hello database on myVM1 (primary)</span></span>

<span data-ttu-id="5a3cb-195">tooswitch z primární toostandby (cdb1 toocdb1_stby):</span><span class="sxs-lookup"><span data-stu-id="5a3cb-195">tooswitch from primary toostandby (cdb1 toocdb1_stby):</span></span>

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

<span data-ttu-id="5a3cb-196">Teď se můžete připojit toohello pohotovostní databáze.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-196">You can now connect toohello standby database.</span></span>

<span data-ttu-id="5a3cb-197">Spusťte SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-197">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="switch-over-hello-database-on-myvm2-standby"></a><span data-ttu-id="5a3cb-198">Přepínač přes hello databáze na Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="5a3cb-198">Switch over hello database on myVM2 (standby)</span></span>

<span data-ttu-id="5a3cb-199">tooswitch přes, spusťte na Můjvp2 hello následující:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-199">tooswitch over, run hello following on myVM2:</span></span>
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

<span data-ttu-id="5a3cb-200">Znovu nyní měli být schopný tooconnect toohello primární databáze.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-200">Once again, you should now be able tooconnect toohello primary database.</span></span>

<span data-ttu-id="5a3cb-201">Spusťte SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-201">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="5a3cb-202">Dokončili jste hello instalaci a konfiguraci Data Guard na Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="5a3cb-202">You've finished hello installation and configuration of Data Guard on Oracle Linux.</span></span>


## <a name="delete-hello-virtual-machine"></a><span data-ttu-id="5a3cb-203">Odstranění hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5a3cb-203">Delete hello virtual machine</span></span>

<span data-ttu-id="5a3cb-204">Když už nebude potřebovat hello virtuálních počítačů, můžete použít následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello:</span><span class="sxs-lookup"><span data-stu-id="5a3cb-204">When you no longer need hello VM, you can use hello following command tooremove hello resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="5a3cb-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a3cb-205">Next steps</span></span>

[<span data-ttu-id="5a3cb-206">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="5a3cb-206">Tutorial: Create highly available virtual machines</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="5a3cb-207">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5a3cb-207">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
