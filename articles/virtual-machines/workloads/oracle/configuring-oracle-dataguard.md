---
title: "Implementace Oracle Data Guard na virtuální počítač Azure s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: fe8b635936c74c5154ec83d34160b9aae61c45e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="implement-oracle-data-guard-on-azure-linux-vm"></a><span data-ttu-id="5e365-103">Implementace Oracle Data Guard na virtuální počítač Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="5e365-103">Implement Oracle Data Guard on Azure Linux VM</span></span> 

<span data-ttu-id="5e365-104">Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="5e365-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="5e365-105">Tato příručka údaje, pomocí rozhraní příkazového řádku Azure k nasazení z image Galerie Marketplace 12c databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="5e365-105">This guide details using the Azure CLI to deploy an Oracle 12c Database from the Marketplace gallery image.</span></span> <span data-ttu-id="5e365-106">Po vytvoření databáze Oracle tento dokument popisuje krok za krokem nainstalovat a nakonfigurovat ochranu dat na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="5e365-106">Once the Oracle database is created, this document shows you step-by-step how to install and configure Data Guard on Azure VM.</span></span>

<span data-ttu-id="5e365-107">Než začnete, ujistěte se, že je rozhraní Azure CLI nainstalované.</span><span class="sxs-lookup"><span data-stu-id="5e365-107">Before you start, make sure that the Azure CLI has been installed.</span></span> <span data-ttu-id="5e365-108">Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5e365-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="5e365-109">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="5e365-109">Prepare the environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="5e365-110">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="5e365-110">Assumptions</span></span>

<span data-ttu-id="5e365-111">Pokud chcete provést instalaci Oracle Data Guard, potřebujete vytvořit dva virtuální počítače Azure ve stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5e365-111">To perform the Oracle Data Guard install, you need to create two Azure VMs on the same availability set.</span></span> <span data-ttu-id="5e365-112">Bitová kopie Marketplace, které použijete k vytvoření virtuálních počítačů "Oracle: Oracle – databáze-Ee:12.1.0.2:latest".</span><span class="sxs-lookup"><span data-stu-id="5e365-112">The Marketplace image you use to create the VMs is "Oracle:Oracle-Database-Ee:12.1.0.2:latest".</span></span>

<span data-ttu-id="5e365-113">Primární virtuální počítač (myVM1) má spuštěnou instanci Oracle.</span><span class="sxs-lookup"><span data-stu-id="5e365-113">The primary VM (myVM1) has a running Oracle instance.</span></span>

<span data-ttu-id="5e365-114">V úsporném režimu virtuálních počítačů (Můjvp2) je nainstalován pouze software Oracle.</span><span class="sxs-lookup"><span data-stu-id="5e365-114">The standby VM (myVM2) has the Oracle software installed only.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="5e365-115">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="5e365-115">Log in to Azure</span></span> 

<span data-ttu-id="5e365-116">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="5e365-116">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="5e365-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5e365-117">Create a resource group</span></span>

<span data-ttu-id="5e365-118">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5e365-118">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="5e365-119">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="5e365-119">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="5e365-120">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westus`.</span><span class="sxs-lookup"><span data-stu-id="5e365-120">The following example creates a resource group named `myResourceGroup` in the `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-availability-set"></a><span data-ttu-id="5e365-121">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="5e365-121">Create availability set</span></span>

<span data-ttu-id="5e365-122">Tento krok je volitelný, ale doporučuje se.</span><span class="sxs-lookup"><span data-stu-id="5e365-122">This step is optional, but is recommended.</span></span> <span data-ttu-id="5e365-123">v tématu [Azure dostupnosti nastaví průvodce](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) Další informace.</span><span class="sxs-lookup"><span data-stu-id="5e365-123">see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) for more information.</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-virtual-machine"></a><span data-ttu-id="5e365-124">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5e365-124">Create virtual machine</span></span>

<span data-ttu-id="5e365-125">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="5e365-125">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="5e365-126">Následující příklad vytvoří 2 virtuální počítače s názvem `myVM1` a `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="5e365-126">The following example creates 2 VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="5e365-127">Pokud už neexistují v umístění klíče výchozí, vytvoří klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="5e365-127">Creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="5e365-128">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="5e365-128">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>

<span data-ttu-id="5e365-129">Vytvoření myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5e365-129">Create myVM1 (primary)</span></span>
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

<span data-ttu-id="5e365-130">Po vytvoření virtuálního počítače Azure CLI zobrazí informace o podobně jako v následujícím příkladu: Poznamenejte si `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="5e365-130">Once the VM has been created, the Azure CLI shows information similar to the following example: Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="5e365-131">Tato adresa se používá pro přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5e365-131">This address is used to access the VM.</span></span>

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

<span data-ttu-id="5e365-132">Vytvoření Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="5e365-132">Create myVM2 (standby)</span></span>
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

<span data-ttu-id="5e365-133">Poznamenejte si `publicIpAddress` i po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="5e365-133">Take note of the `publicIpAddress` as well once it created.</span></span>

### <a name="open-the-tcp-port-for-connectivity"></a><span data-ttu-id="5e365-134">Otevřete port TCP pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="5e365-134">Open the TCP port for connectivity</span></span>

<span data-ttu-id="5e365-135">V kroku se nakonfigurovat externí koncové body, které umožňuje vzdálený přístup k databázi Oracle, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e365-135">The step is to configure external endpoints, which allows accessing the Oracle DB remotely, you execute the following command.</span></span>

<span data-ttu-id="5e365-136">Otevřete port pro myVM1</span><span class="sxs-lookup"><span data-stu-id="5e365-136">Open port for myVM1</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVmN1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="5e365-137">Výsledek by měl vypadat podobně jako následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="5e365-137">Result should look similar to the following response:</span></span>

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

<span data-ttu-id="5e365-138">Otevřete port pro Můjvp2</span><span class="sxs-lookup"><span data-stu-id="5e365-138">Open port for myVM2</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2N1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-virtual-machine"></a><span data-ttu-id="5e365-139">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="5e365-139">Connect to virtual machine</span></span>

<span data-ttu-id="5e365-140">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="5e365-140">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="5e365-141">IP adresu nahraďte pomocí adresy `publicIpAddress` vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5e365-141">Replace the IP address with the `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh azureuser@<publicIpAddress>
```

### <a name="create-database-on-myvm1-primary"></a><span data-ttu-id="5e365-142">Vytvořit databázi na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5e365-142">Create Database on myVM1 (Primary)</span></span>

<span data-ttu-id="5e365-143">Oracle software je již nainstalována na bitovou kopii Marketplace, takže dalším krokem je pro instalaci databáze.</span><span class="sxs-lookup"><span data-stu-id="5e365-143">The Oracle software is already installed on the Marketplace image, so the next step is to install the database.</span></span> <span data-ttu-id="5e365-144">prvním krokem je spuštěna jako superuživatele, oracle'.</span><span class="sxs-lookup"><span data-stu-id="5e365-144">the first step is running as the 'oracle' superuser.</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="5e365-145">Vytvoření databáze:</span><span class="sxs-lookup"><span data-stu-id="5e365-145">create the database:</span></span>

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
<span data-ttu-id="5e365-146">Výstupy by měl vypadat podobně jako následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="5e365-146">Outputs should look similar to the following response:</span></span>

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

<span data-ttu-id="5e365-147">Nastavení proměnných ORACLE_SID a ORACLE_HOME</span><span class="sxs-lookup"><span data-stu-id="5e365-147">Set the ORACLE_SID and ORACLE_HOME variables</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="5e365-148">Volitelně můžete přidané ORACLE_HOME a ORACLE_SID soubor .bashrc, tak, aby tato nastavení se uloží pro budoucí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5e365-148">Optionally, You can added ORACLE_HOME and ORACLE_SID to the .bashrc file, so that these settings are saved for future logins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="data-guard-configurations"></a><span data-ttu-id="5e365-149">Konfigurace ochrana dat</span><span class="sxs-lookup"><span data-stu-id="5e365-149">Data Guard Configurations</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="5e365-150">Povolit režim protokolu archiv na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5e365-150">Enable Archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="5e365-151">Povolit protokolování platnost a ujistěte se, že je k dispozici alespoň jeden soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="5e365-151">Enable force logging, and make sure at least one logfile is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="5e365-152">Vytvoření pohotovostní opakování protokoly</span><span class="sxs-lookup"><span data-stu-id="5e365-152">Create standby redo logs</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="5e365-153">Zapnout Flashback (který provedli obnovení mnohem dříve) a nastavte STANDBY_FILE_MANAGEMENT na automatické</span><span class="sxs-lookup"><span data-stu-id="5e365-153">Turn on Flashback (which made the recovery a lot earlier) and set STANDBY_FILE_MANAGEMENT to auto</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
```

### <a name="service-setup-on-myvm1-primary"></a><span data-ttu-id="5e365-154">Instalační program služby na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5e365-154">Service setup on myVM1 (primary)</span></span>

<span data-ttu-id="5e365-155">Upravovat nebo vytvářet tnsnames.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="5e365-155">Edit or create the tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="5e365-156">Přidejte následující položky</span><span class="sxs-lookup"><span data-stu-id="5e365-156">Add the following entries</span></span>

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

<span data-ttu-id="5e365-157">Upravovat nebo vytvářet listener.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="5e365-157">Edit or create the listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="5e365-158">Přidejte následující položky</span><span class="sxs-lookup"><span data-stu-id="5e365-158">Add the following entries</span></span>

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

<span data-ttu-id="5e365-159">Spuštění nástroje pro sledování</span><span class="sxs-lookup"><span data-stu-id="5e365-159">Start the listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="service-setup-on-myvm2-standby"></a><span data-ttu-id="5e365-160">Instalační program služby na Můjvp2 (Úsporný režim)</span><span class="sxs-lookup"><span data-stu-id="5e365-160">Service setup on myVM2 (Standby)</span></span>

<span data-ttu-id="5e365-161">Upravovat nebo vytvářet tnsnames.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="5e365-161">Edit or create the tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="5e365-162">Přidejte následující položky</span><span class="sxs-lookup"><span data-stu-id="5e365-162">Add the following entries</span></span>

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

<span data-ttu-id="5e365-163">Upravovat nebo vytvářet listener.ora souboru, který je umístěný ve složce ORACLE_HOME\network\admin $</span><span class="sxs-lookup"><span data-stu-id="5e365-163">Edit or create the listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="5e365-164">Přidejte následující položky</span><span class="sxs-lookup"><span data-stu-id="5e365-164">Add the following entries</span></span>

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

<span data-ttu-id="5e365-165">Spuštění nástroje pro sledování</span><span class="sxs-lookup"><span data-stu-id="5e365-165">Start the listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

<span data-ttu-id="5e365-166">Povolit ochranu zprostředkovatele dat</span><span class="sxs-lookup"><span data-stu-id="5e365-166">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="restore-database-to-myvm2-standby"></a><span data-ttu-id="5e365-167">Obnovení databáze do Můjvp2 (Úsporný režim)</span><span class="sxs-lookup"><span data-stu-id="5e365-167">Restore database to myVM2 (Standby)</span></span>

<span data-ttu-id="5e365-168">Vytvořte soubor s parametry, nebo tmp/initcdb1_stby.ora' s obsah těchto hodnot</span><span class="sxs-lookup"><span data-stu-id="5e365-168">Create a parameter file '/tmp/initcdb1_stby.ora' with the followings contents</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="5e365-169">Vytváření složek</span><span class="sxs-lookup"><span data-stu-id="5e365-169">Create folders</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="5e365-170">Vytvoření souboru heslo</span><span class="sxs-lookup"><span data-stu-id="5e365-170">Create password file</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0.2/db_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="5e365-171">Spustit zálohu databáze na Můjvp2</span><span class="sxs-lookup"><span data-stu-id="5e365-171">Start up database on myVM2</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="5e365-172">Obnovte databázi pomocí RMAN nástroj</span><span class="sxs-lookup"><span data-stu-id="5e365-172">Restore database using RMAN utility</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="5e365-173">Spuštěním následujících příkazů v RMAN</span><span class="sxs-lookup"><span data-stu-id="5e365-173">Execute the following commands in RMAN</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="5e365-174">Povolit ochranu zprostředkovatele dat</span><span class="sxs-lookup"><span data-stu-id="5e365-174">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="5e365-175">Konfigurace zprostředkovatele Data Guard na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5e365-175">Configure Data Guard broker on myVM1 (primary)</span></span>

<span data-ttu-id="5e365-176">Spusťte správce Data Guard a přihlášení pomocí SYS a hesla (do nepoužíváte ověřování operačního systému).</span><span class="sxs-lookup"><span data-stu-id="5e365-176">Start the Data Guard manager and login using SYS and password (do not using OS authentication).</span></span> <span data-ttu-id="5e365-177">Provedení těchto hodnot</span><span class="sxs-lookup"><span data-stu-id="5e365-177">Perform the followings</span></span>

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

<span data-ttu-id="5e365-178">Zkontrolujte konfiguraci</span><span class="sxs-lookup"><span data-stu-id="5e365-178">Review the configuration</span></span>
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

<span data-ttu-id="5e365-179">To Oracle Data Guard instalace dokončena.</span><span class="sxs-lookup"><span data-stu-id="5e365-179">This completed the Oracle Data Guard setup.</span></span> <span data-ttu-id="5e365-180">V další části se dozvíte, jak k testování připojení a přepínání</span><span class="sxs-lookup"><span data-stu-id="5e365-180">The next section shows you how to test the connectivity and switching over</span></span>

### <a name="connect-database-from-client-machine"></a><span data-ttu-id="5e365-181">Připojit databáze z klientského počítače</span><span class="sxs-lookup"><span data-stu-id="5e365-181">Connect database from client machine</span></span>

<span data-ttu-id="5e365-182">Aktualizujte nebo vytvoření souboru tnsnames.ora do klientského počítače, která se obvykle nachází v $ORACLE_HOME\network\admin.</span><span class="sxs-lookup"><span data-stu-id="5e365-182">Update or create the tnsnames.ora file on your client machine which usually is located at $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="5e365-183">Nahraďte IP s vaší `publicIpAddress` myVM1 a Můjvp2</span><span class="sxs-lookup"><span data-stu-id="5e365-183">Replace the IP with your `publicIpAddress` for myVM1 and myVM2</span></span>

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

<span data-ttu-id="5e365-184">Spustit sqlplus</span><span class="sxs-lookup"><span data-stu-id="5e365-184">Start sqlplus</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-data-guard-configuration"></a><span data-ttu-id="5e365-185">Konfigurace testovací Data Guard</span><span class="sxs-lookup"><span data-stu-id="5e365-185">Test Data Guard configuration</span></span>

### <a name="database-switchover-on-myvm1-primary"></a><span data-ttu-id="5e365-186">Databáze přepnutí na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="5e365-186">Database switchover on myVM1 (primary)</span></span>

<span data-ttu-id="5e365-187">Přepnutí z primární do úsporného režimu (cdb1 k cdb1_stby)</span><span class="sxs-lookup"><span data-stu-id="5e365-187">To switch from primary to standby (cdb1 to cdb1_stby)</span></span>

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

<span data-ttu-id="5e365-188">Teď by měla být schopni připojit k databázi pohotovostní</span><span class="sxs-lookup"><span data-stu-id="5e365-188">You should now be able to connect to the standby database</span></span>

<span data-ttu-id="5e365-189">Spustit sqlplus</span><span class="sxs-lookup"><span data-stu-id="5e365-189">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="database-switch-back-on-myvm2-standby"></a><span data-ttu-id="5e365-190">Parametr databáze zpět na Můjvp2 (pohotovostní)</span><span class="sxs-lookup"><span data-stu-id="5e365-190">Database switch back on myVM2 (standby)</span></span>

<span data-ttu-id="5e365-191">Chcete-li přepnout zpět, spusťte na Můjvp2 těchto hodnot</span><span class="sxs-lookup"><span data-stu-id="5e365-191">To switch back, run the followings on myVM2</span></span>
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

<span data-ttu-id="5e365-192">Znovu musí teď nebudete moct připojit k primární databázi</span><span class="sxs-lookup"><span data-stu-id="5e365-192">Once again, You should now be able to connect to the primary database</span></span>

<span data-ttu-id="5e365-193">Spustit sqlplus</span><span class="sxs-lookup"><span data-stu-id="5e365-193">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="5e365-194">To dokončit instalaci a konfiguraci Data Guard na Oracle linux.</span><span class="sxs-lookup"><span data-stu-id="5e365-194">This completed the installation and configuration of Data Guard on Oracle linux.</span></span>


## <a name="delete-virtual-machine"></a><span data-ttu-id="5e365-195">Odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5e365-195">Delete virtual machine</span></span>

<span data-ttu-id="5e365-196">Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e365-196">When no longer needed, the following command can be used to remove the Resource Group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="5e365-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e365-197">Next steps</span></span>

[<span data-ttu-id="5e365-198">Kurz vytvoření virtuálního počítače s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="5e365-198">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="5e365-199">Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5e365-199">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
