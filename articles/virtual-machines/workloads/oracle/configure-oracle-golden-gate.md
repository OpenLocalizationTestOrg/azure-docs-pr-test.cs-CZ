---
title: "Implementace Oracle Golden brány ve virtuálním počítači Azure Linux | Microsoft Docs"
description: "Rychle získáte bránou Golden Oracle nahoru a spouštění v prostředí Azure."
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
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: a05711357d345267647c02e42336fd37c09e1bff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a><span data-ttu-id="0fb10-103">Implementace Oracle Golden brány ve virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="0fb10-103">Implement Oracle Golden Gate on an Azure Linux VM</span></span> 

<span data-ttu-id="0fb10-104">Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="0fb10-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="0fb10-105">Tento průvodce detailně používání rozhraní příkazového řádku Azure k nasazení databáze Oracle 12c z Galerie image Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0fb10-105">This guide details how to use the Azure CLI to deploy an Oracle 12c database from the Azure Marketplace gallery image.</span></span> 

<span data-ttu-id="0fb10-106">Tento dokument popisuje krok za krokem k vytvoření, instalace a konfigurace brány Golden Oracle na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="0fb10-106">This document shows you step-by-step how to create, install, and configure Oracle Golden Gate on an Azure VM.</span></span>

<span data-ttu-id="0fb10-107">Než začnete, ujistěte se, že je rozhraní Azure CLI nainstalované.</span><span class="sxs-lookup"><span data-stu-id="0fb10-107">Before you start, make sure that the Azure CLI has been installed.</span></span> <span data-ttu-id="0fb10-108">Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0fb10-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="0fb10-109">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="0fb10-109">Prepare the environment</span></span>

<span data-ttu-id="0fb10-110">K provedení instalace brány Golden Oracle, potřebujete vytvořit dva virtuální počítače Azure ve stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="0fb10-110">To perform the Oracle Golden Gate installation, you need to create two Azure VMs on the same availability set.</span></span> <span data-ttu-id="0fb10-111">Bitová kopie Marketplace použijete k vytvoření virtuálních počítačů je **Oracle: Oracle – databáze-Ee:12.1.0.2:latest**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-111">The Marketplace image you use to create the VMs is **Oracle:Oracle-Database-Ee:12.1.0.2:latest**.</span></span>

<span data-ttu-id="0fb10-112">Také musíte znát Unix editor vi a mají základní znalosti o x11 (X Windows).</span><span class="sxs-lookup"><span data-stu-id="0fb10-112">You also need to be familiar with Unix editor vi and have a basic understanding of x11 (X Windows).</span></span>

<span data-ttu-id="0fb10-113">Následuje souhrn konfigurace prostředí:</span><span class="sxs-lookup"><span data-stu-id="0fb10-113">The following is a summary of the environment configuration:</span></span>
> 
> |  | <span data-ttu-id="0fb10-114">**Primární lokalita**</span><span class="sxs-lookup"><span data-stu-id="0fb10-114">**Primary site**</span></span> | <span data-ttu-id="0fb10-115">**Replikace webu**</span><span class="sxs-lookup"><span data-stu-id="0fb10-115">**Replicate site**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="0fb10-116">**Databázi Oracle**</span><span class="sxs-lookup"><span data-stu-id="0fb10-116">**Oracle release**</span></span> |<span data-ttu-id="0fb10-117">Oracle 12c verze 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="0fb10-117">Oracle 12c Release 2 – (12.1.0.2)</span></span> |<span data-ttu-id="0fb10-118">Oracle 12c verze 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="0fb10-118">Oracle 12c Release 2 – (12.1.0.2)</span></span>|
> | <span data-ttu-id="0fb10-119">**Název počítače**</span><span class="sxs-lookup"><span data-stu-id="0fb10-119">**Machine name**</span></span> |<span data-ttu-id="0fb10-120">myVM1</span><span class="sxs-lookup"><span data-stu-id="0fb10-120">myVM1</span></span> |<span data-ttu-id="0fb10-121">Můjvp2</span><span class="sxs-lookup"><span data-stu-id="0fb10-121">myVM2</span></span> |
> | <span data-ttu-id="0fb10-122">**Operační systém**</span><span class="sxs-lookup"><span data-stu-id="0fb10-122">**Operating system**</span></span> |<span data-ttu-id="0fb10-123">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="0fb10-123">Oracle Linux 6.x</span></span> |<span data-ttu-id="0fb10-124">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="0fb10-124">Oracle Linux 6.x</span></span> |
> | <span data-ttu-id="0fb10-125">**Oracle SID**</span><span class="sxs-lookup"><span data-stu-id="0fb10-125">**Oracle SID**</span></span> |<span data-ttu-id="0fb10-126">CDB1</span><span class="sxs-lookup"><span data-stu-id="0fb10-126">CDB1</span></span> |<span data-ttu-id="0fb10-127">CDB1</span><span class="sxs-lookup"><span data-stu-id="0fb10-127">CDB1</span></span> |
> | <span data-ttu-id="0fb10-128">**Schéma replikace**</span><span class="sxs-lookup"><span data-stu-id="0fb10-128">**Replication schema**</span></span> |<span data-ttu-id="0fb10-129">TEST</span><span class="sxs-lookup"><span data-stu-id="0fb10-129">TEST</span></span>|<span data-ttu-id="0fb10-130">TEST</span><span class="sxs-lookup"><span data-stu-id="0fb10-130">TEST</span></span> |
> | <span data-ttu-id="0fb10-131">**Vlastník nebo replikovat Golden brány**</span><span class="sxs-lookup"><span data-stu-id="0fb10-131">**Golden Gate owner/replicate**</span></span> |<span data-ttu-id="0fb10-132">C ##GGADMIN</span><span class="sxs-lookup"><span data-stu-id="0fb10-132">C##GGADMIN</span></span> |<span data-ttu-id="0fb10-133">REPUSER</span><span class="sxs-lookup"><span data-stu-id="0fb10-133">REPUSER</span></span> |
> | <span data-ttu-id="0fb10-134">**Proces Golden brány**</span><span class="sxs-lookup"><span data-stu-id="0fb10-134">**Golden Gate process**</span></span> |<span data-ttu-id="0fb10-135">EXTORA</span><span class="sxs-lookup"><span data-stu-id="0fb10-135">EXTORA</span></span> |<span data-ttu-id="0fb10-136">REPORA</span><span class="sxs-lookup"><span data-stu-id="0fb10-136">REPORA</span></span>|


### <a name="sign-in-to-azure"></a><span data-ttu-id="0fb10-137">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="0fb10-137">Sign in to Azure</span></span> 

<span data-ttu-id="0fb10-138">Přihlaste se k předplatnému Azure s [az přihlášení](/cli/azure/#login) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0fb10-138">Sign in to your Azure subscription with the [az login](/cli/azure/#login) command.</span></span> <span data-ttu-id="0fb10-139">Potom postupujte podle na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="0fb10-139">Then follow the on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="0fb10-140">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0fb10-140">Create a resource group</span></span>

<span data-ttu-id="0fb10-141">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="0fb10-141">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="0fb10-142">Skupinu prostředků Azure je logický kontejner, do které prostředky Azure jsou nasazeny a z které mohly být spravovány.</span><span class="sxs-lookup"><span data-stu-id="0fb10-142">An Azure resource group is a logical container into which Azure resources are deployed and from which they can be managed.</span></span> 

<span data-ttu-id="0fb10-143">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westus`.</span><span class="sxs-lookup"><span data-stu-id="0fb10-143">The following example creates a resource group named `myResourceGroup` in the `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="0fb10-144">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="0fb10-144">Create an availability set</span></span>

<span data-ttu-id="0fb10-145">Následující krok je volitelný, ale doporučené.</span><span class="sxs-lookup"><span data-stu-id="0fb10-145">The following step is optional but recommended.</span></span> <span data-ttu-id="0fb10-146">Další informace najdete v tématu [Azure dostupnosti nastaví průvodce](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="0fb10-146">For more information, see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="0fb10-147">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0fb10-147">Create a virtual machine</span></span>

<span data-ttu-id="0fb10-148">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="0fb10-148">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="0fb10-149">Následující příklad vytvoří dva virtuální počítače s názvem `myVM1` a `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="0fb10-149">The following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="0fb10-150">Pokud už neexistují ve výchozím umístění klíče, vytvoření klíčů SSH.</span><span class="sxs-lookup"><span data-stu-id="0fb10-150">Create SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="0fb10-151">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="0fb10-151">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>

#### <a name="create-myvm1-primary"></a><span data-ttu-id="0fb10-152">Vytvořte myVM1 (primární):</span><span class="sxs-lookup"><span data-stu-id="0fb10-152">Create myVM1 (primary):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="0fb10-153">Po vytvoření virtuálního počítače, rozhraní příkazového řádku Azure se zobrazují informace podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0fb10-153">After the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="0fb10-154">(Poznamenejte si `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="0fb10-154">(Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="0fb10-155">Tato adresa se používá pro přístup k virtuálnímu počítači.)</span><span class="sxs-lookup"><span data-stu-id="0fb10-155">This address is used to access the VM.)</span></span>

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

#### <a name="create-myvm2-replicate"></a><span data-ttu-id="0fb10-156">Vytvoření Můjvp2 (Replikovat):</span><span class="sxs-lookup"><span data-stu-id="0fb10-156">Create myVM2 (replicate):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="0fb10-157">Poznamenejte si `publicIpAddress` i po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="0fb10-157">Take note of the `publicIpAddress` as well after it has been created.</span></span>

### <a name="open-the-tcp-port-for-connectivity"></a><span data-ttu-id="0fb10-158">Otevřete port TCP pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="0fb10-158">Open the TCP port for connectivity</span></span>

<span data-ttu-id="0fb10-159">Dalším krokem je konfigurace externí koncové body, které vám umožní přístup k databázi Oracle vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="0fb10-159">The next step is to configure external endpoints,  which enable you to access the Oracle database remotely.</span></span> <span data-ttu-id="0fb10-160">Pokud chcete nakonfigurovat externí koncové body, spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="0fb10-160">To configure the external endpoints, run the following commands.</span></span>

#### <a name="open-the-port-for-myvm1"></a><span data-ttu-id="0fb10-161">Otevřete port pro myVM1:</span><span class="sxs-lookup"><span data-stu-id="0fb10-161">Open the port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="0fb10-162">Výsledky by měl vypadat podobně jako následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="0fb10-162">The results should look similar to the following response:</span></span>

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

#### <a name="open-the-port-for-myvm2"></a><span data-ttu-id="0fb10-163">Otevřete port pro Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="0fb10-163">Open the port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="0fb10-164">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="0fb10-164">Connect to the virtual machine</span></span>

<span data-ttu-id="0fb10-165">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="0fb10-165">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="0fb10-166">IP adresu nahraďte pomocí adresy `publicIpAddress` vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0fb10-166">Replace the IP address with the `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh <publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a><span data-ttu-id="0fb10-167">Vytvořit databázi na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="0fb10-167">Create the database on myVM1 (primary)</span></span>

<span data-ttu-id="0fb10-168">Oracle software je již nainstalována na bitovou kopii Marketplace, takže dalším krokem je pro instalaci databáze.</span><span class="sxs-lookup"><span data-stu-id="0fb10-168">The Oracle software is already installed on the Marketplace image, so the next step is to install the database.</span></span> 

<span data-ttu-id="0fb10-169">Spusťte software jako superuživatele, oracle':</span><span class="sxs-lookup"><span data-stu-id="0fb10-169">Run the software as the 'oracle' superuser:</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="0fb10-170">Vytvoření databáze:</span><span class="sxs-lookup"><span data-stu-id="0fb10-170">Create the database:</span></span>

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
<span data-ttu-id="0fb10-171">Výstupy by měl vypadat podobně jako následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="0fb10-171">Outputs should look similar to the following response:</span></span>

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
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

<span data-ttu-id="0fb10-172">Nastavení proměnných ORACLE_SID a ORACLE_HOME.</span><span class="sxs-lookup"><span data-stu-id="0fb10-172">Set the ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="0fb10-173">Volitelně můžete přidat ORACLE_HOME a ORACLE_SID soubor .bashrc, tak, aby tato nastavení se uloží pro budoucí přihlášení:</span><span class="sxs-lookup"><span data-stu-id="0fb10-173">Optionally, you can add ORACLE_HOME and ORACLE_SID to the .bashrc file, so that these settings are saved for future sign-ins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="0fb10-174">Spuštění nástroje pro sledování Oracle</span><span class="sxs-lookup"><span data-stu-id="0fb10-174">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-the-database-on-myvm2-replicate"></a><span data-ttu-id="0fb10-175">Vytvořit databázi na Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="0fb10-175">Create the database on myVM2 (replicate)</span></span>

```bash
sudo su - oracle
```
<span data-ttu-id="0fb10-176">Vytvoření databáze:</span><span class="sxs-lookup"><span data-stu-id="0fb10-176">Create the database:</span></span>

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
<span data-ttu-id="0fb10-177">Nastavení proměnných ORACLE_SID a ORACLE_HOME.</span><span class="sxs-lookup"><span data-stu-id="0fb10-177">Set the ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="0fb10-178">Volitelně můžete přidané ORACLE_HOME a ORACLE_SID soubor .bashrc, tak, aby tato nastavení se uloží pro budoucí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0fb10-178">Optionally, you can added ORACLE_HOME and ORACLE_SID to the .bashrc file, so that these settings are saved for future sign-ins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="0fb10-179">Spuštění nástroje pro sledování Oracle</span><span class="sxs-lookup"><span data-stu-id="0fb10-179">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a><span data-ttu-id="0fb10-180">Konfigurace brány Golden</span><span class="sxs-lookup"><span data-stu-id="0fb10-180">Configure Golden Gate</span></span> 
<span data-ttu-id="0fb10-181">Pokud chcete konfigurovat Golden brány, postupujte podle kroků v této části.</span><span class="sxs-lookup"><span data-stu-id="0fb10-181">To configure Golden Gate, take the steps in this section.</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="0fb10-182">Povolit režim protokolu archiv na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="0fb10-182">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="0fb10-183">Povolit protokolování platnost a ujistěte se, zda je přítomen alespoň jeden soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="0fb10-183">Enable force logging, and make sure at least one log file is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a><span data-ttu-id="0fb10-184">Stáhnout software Golden brány</span><span class="sxs-lookup"><span data-stu-id="0fb10-184">Download Golden Gate software</span></span>
<span data-ttu-id="0fb10-185">Chcete-li stáhnout a příprava softwaru Oracle Golden brány, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0fb10-185">To download and prepare the Oracle Golden Gate software, complete the following steps:</span></span>

1. <span data-ttu-id="0fb10-186">Stažení **fbo_ggs_Linux_x64_shiphome.zip** souboru z [stránky pro stažení Oracle Golden brány](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="0fb10-186">Download the **fbo_ggs_Linux_x64_shiphome.zip** file from the [Oracle Golden Gate download page](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span></span> <span data-ttu-id="0fb10-187">Pod názvem stažení **12.x.x.x Oracle GoldenGate pro Oracle Linux x86-64**, měla by existovat sadu .zip soubory ke stažení.</span><span class="sxs-lookup"><span data-stu-id="0fb10-187">Under the download title **Oracle GoldenGate 12.x.x.x for Oracle Linux x86-64**, there should be a set of .zip files to download.</span></span>

2. <span data-ttu-id="0fb10-188">Po stažení soubory .zip na klientský počítač, zkopírujte soubory do virtuálního počítače pomocí protokolu Secure kopírování (SCP):</span><span class="sxs-lookup"><span data-stu-id="0fb10-188">After you download the .zip files to your client computer, use Secure Copy Protocol (SCP) to copy the files to your VM:</span></span>

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. <span data-ttu-id="0fb10-189">Přesuňte soubory .zip **/ opt** složky.</span><span class="sxs-lookup"><span data-stu-id="0fb10-189">Move the .zip files to the **/opt** folder.</span></span> <span data-ttu-id="0fb10-190">Potom změňte vlastníka soubory takto:</span><span class="sxs-lookup"><span data-stu-id="0fb10-190">Then change the owner of the files as follows:</span></span>

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. <span data-ttu-id="0fb10-191">Rozbalte soubory (instalace sady Linux rozbalte nástroj, pokud ještě nejsou nainstalované):</span><span class="sxs-lookup"><span data-stu-id="0fb10-191">Unzip the files (install the Linux unzip utility if it's not already installed):</span></span>

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. <span data-ttu-id="0fb10-192">Změna oprávnění:</span><span class="sxs-lookup"><span data-stu-id="0fb10-192">Change permission:</span></span>

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a><span data-ttu-id="0fb10-193">Příprava klienta a virtuálních počítačů, které ke spuštění x11 (pro pouze klienty systému Windows)</span><span class="sxs-lookup"><span data-stu-id="0fb10-193">Prepare the client and VM to run x11 (for Windows clients only)</span></span>
<span data-ttu-id="0fb10-194">Toto je volitelný krok.</span><span class="sxs-lookup"><span data-stu-id="0fb10-194">This is an optional step.</span></span> <span data-ttu-id="0fb10-195">Pokud používáte klienta Linux nebo už máte x11, můžete přeskočit tento krok instalace.</span><span class="sxs-lookup"><span data-stu-id="0fb10-195">You can skip this step if you are using a Linux client or already have x11 setup.</span></span>

1. <span data-ttu-id="0fb10-196">Stáhněte si PuTTY a Xming do počítače se systémem Windows:</span><span class="sxs-lookup"><span data-stu-id="0fb10-196">Download PuTTY and Xming to your Windows computer:</span></span>

  * [<span data-ttu-id="0fb10-197">Stáhněte si PuTTY</span><span class="sxs-lookup"><span data-stu-id="0fb10-197">Download PuTTY</span></span>](http://www.putty.org/)
  * [<span data-ttu-id="0fb10-198">Stáhnout Xming</span><span class="sxs-lookup"><span data-stu-id="0fb10-198">Download Xming</span></span>](https://xming.en.softonic.com/)

2.  <span data-ttu-id="0fb10-199">Po instalaci PuTTY, ve složce PuTTY (například C:\Program Files\PuTTY), spusťte puttygen.exe (generátor PuTTY klíč).</span><span class="sxs-lookup"><span data-stu-id="0fb10-199">After you install PuTTY, in the PuTTY folder (for example, C:\Program Files\PuTTY), run puttygen.exe (PuTTY Key Generator).</span></span>

3.  <span data-ttu-id="0fb10-200">V generátoru PuTTY klíče:</span><span class="sxs-lookup"><span data-stu-id="0fb10-200">In PuTTY Key Generator:</span></span>

  - <span data-ttu-id="0fb10-201">Chcete-li vygenerovat klíč, vyberte **generování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fb10-201">To generate a key, select the **Generate** button.</span></span>
  - <span data-ttu-id="0fb10-202">Zkopírujte obsah klíče (**Ctrl + C**).</span><span class="sxs-lookup"><span data-stu-id="0fb10-202">Copy the contents of the key (**Ctrl+C**).</span></span>
  - <span data-ttu-id="0fb10-203">Vyberte **uložit privátní klíč** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fb10-203">Select the **Save private key** button.</span></span>
  - <span data-ttu-id="0fb10-204">Ignorovat upozornění, že se zobrazí a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-204">Ignore the warning that appears, and then select **OK**.</span></span>

    ![Snímek obrazovky stránky PuTTY klíče generátor](./media/oracle-golden-gate/puttykeygen.png)

4.  <span data-ttu-id="0fb10-206">Ve vašem virtuálním počítači spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="0fb10-206">In your VM, run these commands:</span></span>

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. <span data-ttu-id="0fb10-207">Vytvořte soubor s názvem **authorized_keys**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-207">Create a file named **authorized_keys**.</span></span> <span data-ttu-id="0fb10-208">Umožňuje vložit obsah klíče v tomto souboru a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="0fb10-208">Paste the contents of the key in this file, and then save the file.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0fb10-209">Klíč musí obsahovat řetězec `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="0fb10-209">The key must contain the string `ssh-rsa`.</span></span> <span data-ttu-id="0fb10-210">Obsah klíče musí být jeden řádek textu.</span><span class="sxs-lookup"><span data-stu-id="0fb10-210">Also, the contents of the key must be a single line of text.</span></span>
  >  

6. <span data-ttu-id="0fb10-211">Spusťte PuTTY.</span><span class="sxs-lookup"><span data-stu-id="0fb10-211">Start PuTTY.</span></span> <span data-ttu-id="0fb10-212">V **kategorie** podokně, vyberte **připojení** > **SSH** > **Auth**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-212">In the **Category** pane, select **Connection** > **SSH** > **Auth**.</span></span> <span data-ttu-id="0fb10-213">V **soubor privátního klíče pro ověřování** pole, přejděte na klíč, který jste vygenerovali dříve.</span><span class="sxs-lookup"><span data-stu-id="0fb10-213">In the **Private key file for authentication** box, browse to the key that you generated earlier.</span></span>

  ![Snímek obrazovky stránky nastavit privátní klíč](./media/oracle-golden-gate/setprivatekey.png)

7. <span data-ttu-id="0fb10-215">V **kategorie** podokně, vyberte **připojení** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-215">In the **Category** pane, select **Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="0fb10-216">Vyberte **povolit X11 předávání** pole.</span><span class="sxs-lookup"><span data-stu-id="0fb10-216">Then select the **Enable X11 forwarding** box.</span></span>

  ![Snímek obrazovky stránky povolit X11](./media/oracle-golden-gate/enablex11.png)

8. <span data-ttu-id="0fb10-218">V **kategorie** podokně, přejděte na **relace**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-218">In the **Category** pane, go to **Session**.</span></span> <span data-ttu-id="0fb10-219">Zadejte informace o hostiteli a potom vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="0fb10-219">Enter the host information, and then select **Open**.</span></span>

  ![Snímek obrazovky stránky relace](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a><span data-ttu-id="0fb10-221">Instalovat software Golden brány</span><span class="sxs-lookup"><span data-stu-id="0fb10-221">Install Golden Gate software</span></span>

<span data-ttu-id="0fb10-222">K instalaci brány Golden Oracle, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0fb10-222">To install Oracle Golden Gate, complete the following steps:</span></span>

1. <span data-ttu-id="0fb10-223">Přihlaste se jako oracle.</span><span class="sxs-lookup"><span data-stu-id="0fb10-223">Sign in as oracle.</span></span> <span data-ttu-id="0fb10-224">(Nyní byste měli mít pro přihlášení vyzváni k zadání hesla.) Ujistěte se, že Xming běží před zahájením instalace.</span><span class="sxs-lookup"><span data-stu-id="0fb10-224">(You should be able to sign in without being prompted for a password.) Make sure that Xming is running before you begin the installation.</span></span>
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. <span data-ttu-id="0fb10-225">Vyberte, Oracle GoldenGate pro databázi Oracle 12c'.</span><span class="sxs-lookup"><span data-stu-id="0fb10-225">Select 'Oracle GoldenGate for Oracle Database 12c'.</span></span> <span data-ttu-id="0fb10-226">Potom vyberte **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="0fb10-226">Then select **Next** to continue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte Instalační program](./media/oracle-golden-gate/golden_gate_install_01.png)

3. <span data-ttu-id="0fb10-228">Změňte umístění softwaru.</span><span class="sxs-lookup"><span data-stu-id="0fb10-228">Change the software location.</span></span> <span data-ttu-id="0fb10-229">Vyberte **spustit správce** pole a zadejte umístění databáze.</span><span class="sxs-lookup"><span data-stu-id="0fb10-229">Then select  the **Start Manager** box and enter the database location.</span></span> <span data-ttu-id="0fb10-230">Vyberte **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="0fb10-230">Select **Next** to continue.</span></span>

  ![Snímek obrazovky stránky vyberte instalace](./media/oracle-golden-gate/golden_gate_install_02.png)

4. <span data-ttu-id="0fb10-232">Změňte adresář inventáře a potom vyberte **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="0fb10-232">Change the inventory directory, and then select **Next** to continue.</span></span>

  ![Snímek obrazovky stránky vyberte instalace](./media/oracle-golden-gate/golden_gate_install_03.png)

5. <span data-ttu-id="0fb10-234">Na **Souhrn** obrazovku, vyberte **nainstalovat** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="0fb10-234">On the **Summary** screen, select **Install** to continue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte Instalační program](./media/oracle-golden-gate/golden_gate_install_04.png)

6. <span data-ttu-id="0fb10-236">Může se zobrazit výzva ke spuštění skriptu jako "kořenový".</span><span class="sxs-lookup"><span data-stu-id="0fb10-236">You might be prompted to run a script as 'root'.</span></span> <span data-ttu-id="0fb10-237">Pokud ano, otevřete relaci samostatné ssh k virtuálnímu počítači, sudo pro kořenový adresář a pak spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="0fb10-237">If so, open a separate session, ssh to the VM, sudo to root, and then run the script.</span></span> <span data-ttu-id="0fb10-238">Vyberte **OK** pokračovat.</span><span class="sxs-lookup"><span data-stu-id="0fb10-238">Select **OK** continue.</span></span>

  ![Snímek obrazovky stránky vyberte instalace](./media/oracle-golden-gate/golden_gate_install_05.png)

7. <span data-ttu-id="0fb10-240">Po dokončení instalace, vyberte **Zavřít** proces dokončete.</span><span class="sxs-lookup"><span data-stu-id="0fb10-240">When the installation has finished, select **Close** to complete the process.</span></span>

  ![Snímek obrazovky stránky vyberte instalace](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="0fb10-242">Nastavení služby v myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="0fb10-242">Set up service on myVM1 (primary)</span></span>

1. <span data-ttu-id="0fb10-243">Vytvořit nebo aktualizovat souboru tnsnames.ora:</span><span class="sxs-lookup"><span data-stu-id="0fb10-243">Create or update the tnsnames.ora file:</span></span>

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. <span data-ttu-id="0fb10-244">Vytvoření brány Golden vlastníka a uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="0fb10-244">Create the Golden Gate owner and user accounts.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0fb10-245">Účet vlastníka, musí mít předponu C ##.</span><span class="sxs-lookup"><span data-stu-id="0fb10-245">The owner account must have C## prefix.</span></span>
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA to C##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. <span data-ttu-id="0fb10-246">Vytvoření brány Golden testovací uživatelský účet:</span><span class="sxs-lookup"><span data-stu-id="0fb10-246">Create the Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. <span data-ttu-id="0fb10-247">Konfigurace souboru parametr extrakce.</span><span class="sxs-lookup"><span data-stu-id="0fb10-247">Configure the extract parameter file.</span></span>

 <span data-ttu-id="0fb10-248">Spuštění rozhraní příkazového řádku zlaté brány (ggsci):</span><span class="sxs-lookup"><span data-stu-id="0fb10-248">Start the Golden gate command-line interface (ggsci):</span></span>

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. <span data-ttu-id="0fb10-249">Přidejte následující EXTRAHOVAT parametr soubor (pomocí příkazů vi).</span><span class="sxs-lookup"><span data-stu-id="0fb10-249">Add the following to the EXTRACT parameter file (by using vi commands).</span></span> <span data-ttu-id="0fb10-250">Stisknutím klávesy Esc, ': QW!.</span><span class="sxs-lookup"><span data-stu-id="0fb10-250">Press Esc key, ':wq!'</span></span> <span data-ttu-id="0fb10-251">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="0fb10-251">to save file.</span></span> 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. <span data-ttu-id="0fb10-252">Extrahujte registrace--integrované extrakce:</span><span class="sxs-lookup"><span data-stu-id="0fb10-252">Register extract--integrated extract:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. <span data-ttu-id="0fb10-253">Nastavit extrakce kontrolní body a spustit v reálném čase extrakce:</span><span class="sxs-lookup"><span data-stu-id="0fb10-253">Set up extract checkpoints and start real-time extract:</span></span>

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request to MANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
<span data-ttu-id="0fb10-254">V tomto kroku můžete najít počáteční oznámení změny stavu, který se použije později v jiné části:</span><span class="sxs-lookup"><span data-stu-id="0fb10-254">In this step, you find the starting SCN, which will be used later, in a different section:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a><span data-ttu-id="0fb10-255">Nastavení služby v Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="0fb10-255">Set up service on myVM2 (replicate)</span></span>


1. <span data-ttu-id="0fb10-256">Vytvořit nebo aktualizovat souboru tnsnames.ora:</span><span class="sxs-lookup"><span data-stu-id="0fb10-256">Create or update the tnsnames.ora file:</span></span>

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. <span data-ttu-id="0fb10-257">Vytvořte účet replikace:</span><span class="sxs-lookup"><span data-stu-id="0fb10-257">Create a replicate account:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba to repuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. <span data-ttu-id="0fb10-258">Vytvoření uživatelského účtu testovací Golden brány:</span><span class="sxs-lookup"><span data-stu-id="0fb10-258">Create a Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. <span data-ttu-id="0fb10-259">Soubor parametrů REPLICAT k replikaci změn:</span><span class="sxs-lookup"><span data-stu-id="0fb10-259">REPLICAT parameter file to replicate changes:</span></span> 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  <span data-ttu-id="0fb10-260">Obsah souboru REPORA parametr:</span><span class="sxs-lookup"><span data-stu-id="0fb10-260">Content of REPORA parameter file:</span></span>

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. <span data-ttu-id="0fb10-261">Nastavte kontrolní bod replicat:</span><span class="sxs-lookup"><span data-stu-id="0fb10-261">Set up a replicat checkpoint:</span></span>

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-the-replication-myvm1-and-myvm2"></a><span data-ttu-id="0fb10-262">Nastavení replikace (myVM1 a Můjvp2)</span><span class="sxs-lookup"><span data-stu-id="0fb10-262">Set up the replication (myVM1 and myVM2)</span></span>

#### <a name="1-set-up-the-replication-on-myvm2-replicate"></a><span data-ttu-id="0fb10-263">1. Nastavení replikace na Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="0fb10-263">1. Set up the replication on myVM2 (replicate)</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
<span data-ttu-id="0fb10-264">Aktualizujte soubor s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="0fb10-264">Update the file with the following:</span></span>

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
<span data-ttu-id="0fb10-265">Potom restartujte službu Manager:</span><span class="sxs-lookup"><span data-stu-id="0fb10-265">Then restart the Manager service:</span></span>

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-the-replication-on-myvm1-primary"></a><span data-ttu-id="0fb10-266">2. Nastavení replikace na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="0fb10-266">2. Set up the replication on myVM1 (primary)</span></span>

<span data-ttu-id="0fb10-267">Spustíte počáteční zatížení a zkontrolujte chyby:</span><span class="sxs-lookup"><span data-stu-id="0fb10-267">Start the initial load and check for errors:</span></span>

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-the-replication-on-myvm2-replicate"></a><span data-ttu-id="0fb10-268">3. Nastavení replikace na Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="0fb10-268">3. Set up the replication on myVM2 (replicate)</span></span>

<span data-ttu-id="0fb10-269">Změna oznámení změny stavu číslo s číslem, které se předtím získali:</span><span class="sxs-lookup"><span data-stu-id="0fb10-269">Change the SCN number with the number you obtained before:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
<span data-ttu-id="0fb10-270">Zahájení replikace a jej můžete otestovat pomocí vkládání nových záznamů do testovací tabulek.</span><span class="sxs-lookup"><span data-stu-id="0fb10-270">The replication has begun, and you can test it by inserting new records to TEST tables.</span></span>


### <a name="view-job-status-and-troubleshooting"></a><span data-ttu-id="0fb10-271">Zobrazení stavu úlohy a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="0fb10-271">View job status and troubleshooting</span></span>

#### <a name="view-reports"></a><span data-ttu-id="0fb10-272">Zobrazení sestav</span><span class="sxs-lookup"><span data-stu-id="0fb10-272">View reports</span></span>
<span data-ttu-id="0fb10-273">Chcete-li zobrazit sestavy myVM1, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0fb10-273">To view reports on myVM1, run the following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
<span data-ttu-id="0fb10-274">Chcete-li zobrazit sestavy Můjvp2, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0fb10-274">To view reports on myVM2, run the following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a><span data-ttu-id="0fb10-275">Zobrazit stav a historie</span><span class="sxs-lookup"><span data-stu-id="0fb10-275">View status and history</span></span>
<span data-ttu-id="0fb10-276">Chcete-li zobrazit stav a historie na myVM1, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0fb10-276">To view status and history on myVM1, run the following commands:</span></span>

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

<span data-ttu-id="0fb10-277">Chcete-li zobrazit stav a historie na Můjvp2, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0fb10-277">To view status and history on myVM2, run the following commands:</span></span>

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
<span data-ttu-id="0fb10-278">Tím dokončíte instalaci a konfiguraci brány Golden na Oracle linux.</span><span class="sxs-lookup"><span data-stu-id="0fb10-278">This completes the installation and configuration of Golden Gate on Oracle linux.</span></span>


## <a name="delete-the-virtual-machine"></a><span data-ttu-id="0fb10-279">Odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0fb10-279">Delete the virtual machine</span></span>

<span data-ttu-id="0fb10-280">Už je potřeba, následující příkaz lze použít k odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="0fb10-280">When it's no longer needed, the following command can be used to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="0fb10-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fb10-281">Next steps</span></span>

[<span data-ttu-id="0fb10-282">Kurz vytvoření virtuálního počítače s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="0fb10-282">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="0fb10-283">Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0fb10-283">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
