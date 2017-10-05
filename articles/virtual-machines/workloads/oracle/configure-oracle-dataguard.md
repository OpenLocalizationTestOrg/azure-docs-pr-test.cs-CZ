---
title: "Implementace Oracle Data Guard na virtuálním počítači Azure Linux | Microsoft Docs"
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
ms.openlocfilehash: 11492b85e95ddb39489e36c572af2a168b4c7af8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="169df-103">Implementace Oracle Data Guard na virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="169df-103">Implement Oracle Data Guard on an Azure Linux virtual machine</span></span> 

<span data-ttu-id="169df-104">Rozhraní příkazového řádku Azure slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="169df-104">Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="169df-105">Tento článek popisuje, jak používat rozhraní příkazového řádku Azure k nasazení databázi Oracle Database 12c z image Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="169df-105">This article describes how to use Azure CLI to deploy an Oracle Database 12c database from the Azure Marketplace image.</span></span> <span data-ttu-id="169df-106">Tento článek ukazuje pak můžete krok za krokem, jak nainstalovat a nakonfigurovat ochranu dat na virtuální počítač Azure (VM).</span><span class="sxs-lookup"><span data-stu-id="169df-106">This article then shows you, step by step, how to install and configure Data Guard on an Azure virtual machine (VM).</span></span>

<span data-ttu-id="169df-107">Než začnete, ujistěte se, že je nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="169df-107">Before you start, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="169df-108">Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="169df-108">For more information, see the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="169df-109">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="169df-109">Prepare the environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="169df-110">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="169df-110">Assumptions</span></span>

<span data-ttu-id="169df-111">Pokud chcete nainstalovat Oracle Data Guard, potřebujete vytvořit dva virtuální počítače Azure ve stejné skupině dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="169df-111">To install Oracle Data Guard, you need to create two Azure VMs on the same availability set:</span></span>

- <span data-ttu-id="169df-112">Primární virtuální počítač (myVM1) má spuštěnou instanci Oracle.</span><span class="sxs-lookup"><span data-stu-id="169df-112">The primary VM (myVM1) has a running Oracle instance.</span></span>
- <span data-ttu-id="169df-113">V úsporném režimu virtuálních počítačů (Můjvp2) je nainstalován pouze software Oracle.</span><span class="sxs-lookup"><span data-stu-id="169df-113">The standby VM (myVM2) has the Oracle software installed only.</span></span>

<span data-ttu-id="169df-114">Marketplace obrázku, který použijete k vytvoření virtuálních počítačů je Oracle: Oracle – databáze-Ee:12.1.0.2:latest.</span><span class="sxs-lookup"><span data-stu-id="169df-114">The Marketplace image that you use to create the VMs is Oracle:Oracle-Database-Ee:12.1.0.2:latest.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="169df-115">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="169df-115">Sign in to Azure</span></span> 

<span data-ttu-id="169df-116">Přihlaste se k předplatnému Azure pomocí [az přihlášení](/cli/azure/#login) příkazů a postupujte podle na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="169df-116">Sign in to your Azure subscription by using the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="169df-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="169df-117">Create a resource group</span></span>

<span data-ttu-id="169df-118">Vytvořte skupinu prostředků s použitím [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="169df-118">Create a resource group by using the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="169df-119">Skupinu prostředků Azure je logický kontejner, ve které jsou nasazené a spravované prostředky.</span><span class="sxs-lookup"><span data-stu-id="169df-119">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="169df-120">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westus` umístění:</span><span class="sxs-lookup"><span data-stu-id="169df-120">The following example creates a resource group named `myResourceGroup` in the `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="169df-121">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="169df-121">Create an availability set</span></span>

<span data-ttu-id="169df-122">Vytvoření skupiny dostupnosti je nepovinný, ale doporučujeme ho.</span><span class="sxs-lookup"><span data-stu-id="169df-122">Creating an availability set is optional, but we recommend it.</span></span> <span data-ttu-id="169df-123">Další informace najdete v tématu [sady Azure dostupnosti pokyny](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="169df-123">For more information, see [Azure availability sets guidelines](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="169df-124">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="169df-124">Create a virtual machine</span></span>

<span data-ttu-id="169df-125">Vytvoření virtuálního počítače pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="169df-125">Create a VM by using the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="169df-126">Následující příklad vytvoří dva virtuální počítače s názvem `myVM1` a `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="169df-126">The following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="169df-127">Pokud už neexistují v umístění klíče výchozí klíče SSH, vytvoří se také.</span><span class="sxs-lookup"><span data-stu-id="169df-127">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="169df-128">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="169df-128">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>

<span data-ttu-id="169df-129">Vytvořte myVM1 (primární):</span><span class="sxs-lookup"><span data-stu-id="169df-129">Create myVM1 (primary):</span></span>
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

<span data-ttu-id="169df-130">Po vytvoření virtuálního počítače, rozhraní příkazového řádku Azure se zobrazují informace podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="169df-130">After you create the VM, Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="169df-131">Poznamenejte si hodnotu `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="169df-131">Note the value of `publicIpAddress`.</span></span> <span data-ttu-id="169df-132">Použijte tuto adresu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="169df-132">You use this address to access the VM.</span></span>

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

<span data-ttu-id="169df-133">Vytvoření Můjvp2 (pohotovostní):</span><span class="sxs-lookup"><span data-stu-id="169df-133">Create myVM2 (standby):</span></span>
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

<span data-ttu-id="169df-134">Poznamenejte si hodnotu `publicIpAddress` po vytvoření Můjvp2.</span><span class="sxs-lookup"><span data-stu-id="169df-134">Note the value of `publicIpAddress` after you create myVM2.</span></span>

### <a name="open-the-tcp-port-for-connectivity"></a><span data-ttu-id="169df-135">Otevřete port TCP pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="169df-135">Open the TCP port for connectivity</span></span>

<span data-ttu-id="169df-136">Tento krok nakonfiguruje externí koncové body, které umožňují vzdálený přístup k databázi Oracle.</span><span class="sxs-lookup"><span data-stu-id="169df-136">This step configures external endpoints, which allow remote access to the Oracle database.</span></span>

<span data-ttu-id="169df-137">Otevřete port pro myVM1:</span><span class="sxs-lookup"><span data-stu-id="169df-137">Open the port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="169df-138">Výsledek by měl vypadat podobně jako následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="169df-138">The result should look similar to the following response:</span></span>

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

<span data-ttu-id="169df-139">Otevřete port pro Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="169df-139">Open the port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="169df-140">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="169df-140">Connect to the virtual machine</span></span>

<span data-ttu-id="169df-141">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="169df-141">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="169df-142">Nahraďte na IP adresu `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="169df-142">Replace the IP address with the `publicIpAddress` value for your virtual machine.</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a><span data-ttu-id="169df-143">Vytvořit databázi na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="169df-143">Create the database on myVM1 (primary)</span></span>

<span data-ttu-id="169df-144">Oracle software je již nainstalována na bitovou kopii Marketplace, takže dalším krokem je pro instalaci databáze.</span><span class="sxs-lookup"><span data-stu-id="169df-144">The Oracle software is already installed on the Marketplace image, so the next step is to install the database.</span></span> 

<span data-ttu-id="169df-145">Přepnout na superuživatel Oracle:</span><span class="sxs-lookup"><span data-stu-id="169df-145">Switch to the Oracle superuser:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="169df-146">Vytvoření databáze:</span><span class="sxs-lookup"><span data-stu-id="169df-146">Create the database:</span></span>

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
<span data-ttu-id="169df-147">Výstupy by měl vypadat podobně jako následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="169df-147">Outputs should look similar to the following response:</span></span>

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
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
```

<span data-ttu-id="169df-148">Nastavení proměnných ORACLE_SID a ORACLE_HOME:</span><span class="sxs-lookup"><span data-stu-id="169df-148">Set the ORACLE_SID and ORACLE_HOME variables:</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="169df-149">Volitelně můžete přidat ORACLE_HOME a ORACLE_SID soubor /home/oracle/.bashrc, tak, aby tato nastavení se uloží pro budoucí přihlášení:</span><span class="sxs-lookup"><span data-stu-id="169df-149">Optionally, you can add ORACLE_HOME and ORACLE_SID to the /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a><span data-ttu-id="169df-150">Nakonfigurujte ochranu dat</span><span class="sxs-lookup"><span data-stu-id="169df-150">Configure Data Guard</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="169df-151">Povolit režim protokolu archiv na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="169df-151">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="169df-152">Povolit protokolování platnost a ujistěte se, zda je přítomen alespoň jeden soubor protokolu:</span><span class="sxs-lookup"><span data-stu-id="169df-152">Enable force logging, and make sure at least one log file is present:</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="169df-153">Vytvořte znovu pohotovostní protokoly:</span><span class="sxs-lookup"><span data-stu-id="169df-153">Create standby redo logs:</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="169df-154">Zapnout Flashback (díky čemuž obnovení bylo mnohem snazší) a nastavte pohotovostní\_souboru\_správu, aby automaticky.</span><span class="sxs-lookup"><span data-stu-id="169df-154">Turn on Flashback (which makes recovery a lot easier) and set STANDBY\_FILE\_MANAGEMENT to auto.</span></span> <span data-ttu-id="169df-155">Ukončete SQL * Plus potom.</span><span class="sxs-lookup"><span data-stu-id="169df-155">Exit SQL*Plus after that.</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="169df-156">Nastavení služby v myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="169df-156">Set up service on myVM1 (primary)</span></span>

<span data-ttu-id="169df-157">Upravovat nebo vytvářet tnsnames.ora souboru, který je ve složce ORACLE_HOME\network\admin $.</span><span class="sxs-lookup"><span data-stu-id="169df-157">Edit or create the tnsnames.ora file, which is in the $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="169df-158">Přidejte následující položky:</span><span class="sxs-lookup"><span data-stu-id="169df-158">Add the following entries:</span></span>

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

<span data-ttu-id="169df-159">Upravovat nebo vytvářet listener.ora souboru, který je ve složce ORACLE_HOME\network\admin $.</span><span class="sxs-lookup"><span data-stu-id="169df-159">Edit or create the listener.ora file, which is in the $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="169df-160">Přidejte následující položky:</span><span class="sxs-lookup"><span data-stu-id="169df-160">Add the following entries:</span></span>

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

<span data-ttu-id="169df-161">Povolte zprostředkovatele ochrana dat:</span><span class="sxs-lookup"><span data-stu-id="169df-161">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```
<span data-ttu-id="169df-162">Spuštění nástroje pro sledování:</span><span class="sxs-lookup"><span data-stu-id="169df-162">Start the listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="set-up-service-on-myvm2-standby"></a><span data-ttu-id="169df-163">Nastavení služby v Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="169df-163">Set up service on myVM2 (standby)</span></span>

<span data-ttu-id="169df-164">SSH k Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="169df-164">SSH to myVM2:</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="169df-165">Přihlaste se jako Oracle:</span><span class="sxs-lookup"><span data-stu-id="169df-165">Log in as Oracle:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="169df-166">Upravovat nebo vytvářet tnsnames.ora souboru, který je ve složce ORACLE_HOME\network\admin $.</span><span class="sxs-lookup"><span data-stu-id="169df-166">Edit or create the tnsnames.ora file, which is in the $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="169df-167">Přidejte následující položky:</span><span class="sxs-lookup"><span data-stu-id="169df-167">Add the following entries:</span></span>

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

<span data-ttu-id="169df-168">Upravovat nebo vytvářet listener.ora souboru, který je ve složce ORACLE_HOME\network\admin $.</span><span class="sxs-lookup"><span data-stu-id="169df-168">Edit or create the listener.ora file, which is in the $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="169df-169">Přidejte následující položky:</span><span class="sxs-lookup"><span data-stu-id="169df-169">Add the following entries:</span></span>

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

<span data-ttu-id="169df-170">Spuštění nástroje pro sledování:</span><span class="sxs-lookup"><span data-stu-id="169df-170">Start the listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```


### <a name="restore-the-database-to-myvm2-standby"></a><span data-ttu-id="169df-171">Obnovit databázi do Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="169df-171">Restore the database to myVM2 (standby)</span></span>

<span data-ttu-id="169df-172">Vytvoří /tmp/initcdb1_stby.ora soubor parametru s následující obsah:</span><span class="sxs-lookup"><span data-stu-id="169df-172">Create the parameter file /tmp/initcdb1_stby.ora with the following contents:</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="169df-173">Vytváření složek:</span><span class="sxs-lookup"><span data-stu-id="169df-173">Create folders:</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="169df-174">Vytvořte soubor heslo:</span><span class="sxs-lookup"><span data-stu-id="169df-174">Create a password file:</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0/dbhome_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="169df-175">Spuštění databáze na Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="169df-175">Start the database on myVM2:</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="169df-176">Obnovte databázi pomocí nástroje RMAN:</span><span class="sxs-lookup"><span data-stu-id="169df-176">Restore the database by using the RMAN tool:</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="169df-177">Spusťte následující příkazy v RMAN:</span><span class="sxs-lookup"><span data-stu-id="169df-177">Run the following commands in RMAN:</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="169df-178">Po dokončení příkazu, měli byste vidět zpráv podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="169df-178">You should see messages similar to the following when the command is completed.</span></span> <span data-ttu-id="169df-179">Ukončete RMAN.</span><span class="sxs-lookup"><span data-stu-id="169df-179">Exit RMAN.</span></span>
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

<span data-ttu-id="169df-180">Volitelně můžete přidat ORACLE_HOME a ORACLE_SID soubor /home/oracle/.bashrc, tak, aby tato nastavení se uloží pro budoucí přihlášení:</span><span class="sxs-lookup"><span data-stu-id="169df-180">Optionally, you can add ORACLE_HOME and ORACLE_SID to the /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

<span data-ttu-id="169df-181">Povolte zprostředkovatele ochrana dat:</span><span class="sxs-lookup"><span data-stu-id="169df-181">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="169df-182">Konfigurace zprostředkovatele ochrana dat na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="169df-182">Configure Data Guard Broker on myVM1 (primary)</span></span>

<span data-ttu-id="169df-183">Spusťte správce ochrana dat a přihlaste se pomocí SYS a heslo.</span><span class="sxs-lookup"><span data-stu-id="169df-183">Start Data Guard Manager and log in by using SYS and a password.</span></span> <span data-ttu-id="169df-184">(Nepoužívejte ověřování operačního systému.) Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="169df-184">(Do not use OS authentication.) Perform the following:</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome to DGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> CREATE CONFIGURATION my_dg_config AS PRIMARY DATABASE IS cdb1 CONNECT IDENTIFIER IS cdb1;
Configuration "my_dg_config" created with primary database "cdb1"
DGMGRL> ADD DATABASE cdb1_stby AS CONNECT IDENTIFIER IS cdb1_stby MAINTAINED AS PHYSICAL;
Database "cdb1_stby" added
DGMGRL> ENABLE CONFIGURATION;
Enabled.
```

<span data-ttu-id="169df-185">Zkontrolujte konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="169df-185">Review the configuration:</span></span>
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

<span data-ttu-id="169df-186">Po dokončení instalace Oracle Data Guard.</span><span class="sxs-lookup"><span data-stu-id="169df-186">You've completed the Oracle Data Guard setup.</span></span> <span data-ttu-id="169df-187">V další části se dozvíte, jak k testování připojení a přepínačů.</span><span class="sxs-lookup"><span data-stu-id="169df-187">The next section shows you how to test the connectivity and switch over.</span></span>

### <a name="connect-the-database-from-the-client-machine"></a><span data-ttu-id="169df-188">Připojení databáze z klientského počítače</span><span class="sxs-lookup"><span data-stu-id="169df-188">Connect the database from the client machine</span></span>

<span data-ttu-id="169df-189">Aktualizujte nebo vytvoření souboru tnsnames.ora v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="169df-189">Update or create the tnsnames.ora file on your client machine.</span></span> <span data-ttu-id="169df-190">Tento soubor je obvykle v $ORACLE_HOME\network\admin.</span><span class="sxs-lookup"><span data-stu-id="169df-190">This file is usually in $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="169df-191">Nahraďte IP adresy s vaší `publicIpAddress` hodnoty myVM1 a Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="169df-191">Replace the IP addresses with your `publicIpAddress` values for myVM1 and myVM2:</span></span>

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

<span data-ttu-id="169df-192">Spusťte SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="169df-192">Start SQL*Plus:</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-the-data-guard-configuration"></a><span data-ttu-id="169df-193">Otestujte konfiguraci Data Guard</span><span class="sxs-lookup"><span data-stu-id="169df-193">Test the Data Guard configuration</span></span>

### <a name="switch-over-the-database-on-myvm1-primary"></a><span data-ttu-id="169df-194">Přepínač pro databázi na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="169df-194">Switch over the database on myVM1 (primary)</span></span>

<span data-ttu-id="169df-195">Přepnutí z primární do úsporného režimu (cdb1 k cdb1_stby):</span><span class="sxs-lookup"><span data-stu-id="169df-195">To switch from primary to standby (cdb1 to cdb1_stby):</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome to DGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER TO cdb1_stby;
Performing switchover NOW, please wait...
Operation requires a connection to instance "cdb1" on database "cdb1_stby"
Connecting to instance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1_stby" is opening...
Operation requires start up of instance "cdb1" on database "cdb1"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1_stby"
DGMGRL>
```

<span data-ttu-id="169df-196">Nyní můžete připojit k databázi pohotovostní.</span><span class="sxs-lookup"><span data-stu-id="169df-196">You can now connect to the standby database.</span></span>

<span data-ttu-id="169df-197">Spusťte SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="169df-197">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="switch-over-the-database-on-myvm2-standby"></a><span data-ttu-id="169df-198">Přepínač pro databázi na Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="169df-198">Switch over the database on myVM2 (standby)</span></span>

<span data-ttu-id="169df-199">Na přepnutí, spusťte na Můjvp2 následující:</span><span class="sxs-lookup"><span data-stu-id="169df-199">To switch over, run the following on myVM2:</span></span>
```bash
$ dgmgrl sys/OraPasswd1@cdb1_stby
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome to DGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER TO cdb1;
Performing switchover NOW, please wait...
Operation requires a connection to instance "cdb1" on database "cdb1"
Connecting to instance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1" is opening...
Operation requires start up of instance "cdb1" on database "cdb1_stby"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1"
```

<span data-ttu-id="169df-200">Znovu musí teď nebudete moct připojit k primární databázi.</span><span class="sxs-lookup"><span data-stu-id="169df-200">Once again, you should now be able to connect to the primary database.</span></span>

<span data-ttu-id="169df-201">Spusťte SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="169df-201">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="169df-202">Dokončení instalace a konfigurace Data Guard na Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="169df-202">You've finished the installation and configuration of Data Guard on Oracle Linux.</span></span>


## <a name="delete-the-virtual-machine"></a><span data-ttu-id="169df-203">Odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="169df-203">Delete the virtual machine</span></span>

<span data-ttu-id="169df-204">Když virtuální počítač již nepotřebujete, můžete odebrat skupinu prostředků, virtuální počítač a všechny související prostředky následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="169df-204">When you no longer need the VM, you can use the following command to remove the resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="169df-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="169df-205">Next steps</span></span>

[<span data-ttu-id="169df-206">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="169df-206">Tutorial: Create highly available virtual machines</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="169df-207">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="169df-207">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
