---
title: "aaaImplement Oracle Golden brány na virtuální počítač Azure Linux | Microsoft Docs"
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
ms.openlocfilehash: 320cafd5d23ee472f0af9f92577bc6f432f65778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a><span data-ttu-id="fce8b-103">Implementace Oracle Golden brány ve virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="fce8b-103">Implement Oracle Golden Gate on an Azure Linux VM</span></span> 

<span data-ttu-id="fce8b-104">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="fce8b-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="fce8b-105">Tento průvodce podrobnosti, jak toouse hello rozhraní příkazového řádku Azure toodeploy Oracle 12c databáze z bitové kopie Galerie hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fce8b-105">This guide details how toouse hello Azure CLI toodeploy an Oracle 12c database from hello Azure Marketplace gallery image.</span></span> 

<span data-ttu-id="fce8b-106">Tento dokument vám názorně ukáže, jak toocreate, instalaci a konfiguraci brány Golden Oracle na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="fce8b-106">This document shows you step-by-step how toocreate, install, and configure Oracle Golden Gate on an Azure VM.</span></span>

<span data-ttu-id="fce8b-107">Než začnete, ujistěte se, že byla nainstalována rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-107">Before you start, make sure that hello Azure CLI has been installed.</span></span> <span data-ttu-id="fce8b-108">Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fce8b-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="fce8b-109">Příprava prostředí hello</span><span class="sxs-lookup"><span data-stu-id="fce8b-109">Prepare hello environment</span></span>

<span data-ttu-id="fce8b-110">instalace brány Golden Oracle hello tooperform, je nutné toocreate dva virtuální počítače Azure na hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fce8b-110">tooperform hello Oracle Golden Gate installation, you need toocreate two Azure VMs on hello same availability set.</span></span> <span data-ttu-id="fce8b-111">Hello Marketplace image použít toocreate hello virtuálních počítačů je **Oracle: Oracle – databáze-Ee:12.1.0.2:latest**.</span><span class="sxs-lookup"><span data-stu-id="fce8b-111">hello Marketplace image you use toocreate hello VMs is **Oracle:Oracle-Database-Ee:12.1.0.2:latest**.</span></span>

<span data-ttu-id="fce8b-112">Také potřebujete toobe obeznámeni s Unix editor vi a mají základní znalosti o x11 (X Windows).</span><span class="sxs-lookup"><span data-stu-id="fce8b-112">You also need toobe familiar with Unix editor vi and have a basic understanding of x11 (X Windows).</span></span>

<span data-ttu-id="fce8b-113">Hello Následuje souhrn hello prostředí konfigurace:</span><span class="sxs-lookup"><span data-stu-id="fce8b-113">hello following is a summary of hello environment configuration:</span></span>
> 
> |  | <span data-ttu-id="fce8b-114">**Primární lokalita**</span><span class="sxs-lookup"><span data-stu-id="fce8b-114">**Primary site**</span></span> | <span data-ttu-id="fce8b-115">**Replikace webu**</span><span class="sxs-lookup"><span data-stu-id="fce8b-115">**Replicate site**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="fce8b-116">**Databázi Oracle**</span><span class="sxs-lookup"><span data-stu-id="fce8b-116">**Oracle release**</span></span> |<span data-ttu-id="fce8b-117">Oracle 12c verze 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="fce8b-117">Oracle 12c Release 2 – (12.1.0.2)</span></span> |<span data-ttu-id="fce8b-118">Oracle 12c verze 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="fce8b-118">Oracle 12c Release 2 – (12.1.0.2)</span></span>|
> | <span data-ttu-id="fce8b-119">**Název počítače**</span><span class="sxs-lookup"><span data-stu-id="fce8b-119">**Machine name**</span></span> |<span data-ttu-id="fce8b-120">myVM1</span><span class="sxs-lookup"><span data-stu-id="fce8b-120">myVM1</span></span> |<span data-ttu-id="fce8b-121">Můjvp2</span><span class="sxs-lookup"><span data-stu-id="fce8b-121">myVM2</span></span> |
> | <span data-ttu-id="fce8b-122">**Operační systém**</span><span class="sxs-lookup"><span data-stu-id="fce8b-122">**Operating system**</span></span> |<span data-ttu-id="fce8b-123">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="fce8b-123">Oracle Linux 6.x</span></span> |<span data-ttu-id="fce8b-124">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="fce8b-124">Oracle Linux 6.x</span></span> |
> | <span data-ttu-id="fce8b-125">**Oracle SID**</span><span class="sxs-lookup"><span data-stu-id="fce8b-125">**Oracle SID**</span></span> |<span data-ttu-id="fce8b-126">CDB1</span><span class="sxs-lookup"><span data-stu-id="fce8b-126">CDB1</span></span> |<span data-ttu-id="fce8b-127">CDB1</span><span class="sxs-lookup"><span data-stu-id="fce8b-127">CDB1</span></span> |
> | <span data-ttu-id="fce8b-128">**Schéma replikace**</span><span class="sxs-lookup"><span data-stu-id="fce8b-128">**Replication schema**</span></span> |<span data-ttu-id="fce8b-129">TEST</span><span class="sxs-lookup"><span data-stu-id="fce8b-129">TEST</span></span>|<span data-ttu-id="fce8b-130">TEST</span><span class="sxs-lookup"><span data-stu-id="fce8b-130">TEST</span></span> |
> | <span data-ttu-id="fce8b-131">**Vlastník nebo replikovat Golden brány**</span><span class="sxs-lookup"><span data-stu-id="fce8b-131">**Golden Gate owner/replicate**</span></span> |<span data-ttu-id="fce8b-132">C ##GGADMIN</span><span class="sxs-lookup"><span data-stu-id="fce8b-132">C##GGADMIN</span></span> |<span data-ttu-id="fce8b-133">REPUSER</span><span class="sxs-lookup"><span data-stu-id="fce8b-133">REPUSER</span></span> |
> | <span data-ttu-id="fce8b-134">**Proces Golden brány**</span><span class="sxs-lookup"><span data-stu-id="fce8b-134">**Golden Gate process**</span></span> |<span data-ttu-id="fce8b-135">EXTORA</span><span class="sxs-lookup"><span data-stu-id="fce8b-135">EXTORA</span></span> |<span data-ttu-id="fce8b-136">REPORA</span><span class="sxs-lookup"><span data-stu-id="fce8b-136">REPORA</span></span>|


### <a name="sign-in-tooazure"></a><span data-ttu-id="fce8b-137">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="fce8b-137">Sign in tooAzure</span></span> 

<span data-ttu-id="fce8b-138">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fce8b-138">Sign in tooyour Azure subscription with hello [az login](/cli/azure/#login) command.</span></span> <span data-ttu-id="fce8b-139">Potom postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="fce8b-139">Then follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="fce8b-140">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fce8b-140">Create a resource group</span></span>

<span data-ttu-id="fce8b-141">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fce8b-141">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="fce8b-142">Skupinu prostředků Azure je logický kontejner, do které prostředky Azure jsou nasazeny a z které mohly být spravovány.</span><span class="sxs-lookup"><span data-stu-id="fce8b-142">An Azure resource group is a logical container into which Azure resources are deployed and from which they can be managed.</span></span> 

<span data-ttu-id="fce8b-143">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění.</span><span class="sxs-lookup"><span data-stu-id="fce8b-143">hello following example creates a resource group named `myResourceGroup` in hello `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="fce8b-144">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="fce8b-144">Create an availability set</span></span>

<span data-ttu-id="fce8b-145">Hello následující krok je volitelný, ale doporučené.</span><span class="sxs-lookup"><span data-stu-id="fce8b-145">hello following step is optional but recommended.</span></span> <span data-ttu-id="fce8b-146">Další informace najdete v tématu [Azure dostupnosti nastaví průvodce](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="fce8b-146">For more information, see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="fce8b-147">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fce8b-147">Create a virtual machine</span></span>

<span data-ttu-id="fce8b-148">Vytvoření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fce8b-148">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="fce8b-149">Hello následující příklad vytvoří dva virtuální počítače s názvem `myVM1` a `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="fce8b-149">hello following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="fce8b-150">Pokud už neexistují ve výchozím umístění klíče, vytvoření klíčů SSH.</span><span class="sxs-lookup"><span data-stu-id="fce8b-150">Create SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="fce8b-151">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="fce8b-151">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

#### <a name="create-myvm1-primary"></a><span data-ttu-id="fce8b-152">Vytvořte myVM1 (primární):</span><span class="sxs-lookup"><span data-stu-id="fce8b-152">Create myVM1 (primary):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="fce8b-153">Po hello, kterou virtuální počítač byl vytvořen hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="fce8b-153">After hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="fce8b-154">(Poznamenejte hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="fce8b-154">(Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="fce8b-155">Tato adresa je použité tooaccess hello virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="fce8b-155">This address is used tooaccess hello VM.)</span></span>

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

#### <a name="create-myvm2-replicate"></a><span data-ttu-id="fce8b-156">Vytvoření Můjvp2 (Replikovat):</span><span class="sxs-lookup"><span data-stu-id="fce8b-156">Create myVM2 (replicate):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="fce8b-157">Poznamenejte si hello `publicIpAddress` i po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="fce8b-157">Take note of hello `publicIpAddress` as well after it has been created.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="fce8b-158">Otevřete port TCP hello pro připojení k síti</span><span class="sxs-lookup"><span data-stu-id="fce8b-158">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="fce8b-159">dalším krokem Hello je tooconfigure externí koncové body, které umožňují databáze Oracle hello tooaccess vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="fce8b-159">hello next step is tooconfigure external endpoints,  which enable you tooaccess hello Oracle database remotely.</span></span> <span data-ttu-id="fce8b-160">tooconfigure hello externí koncové body, spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-160">tooconfigure hello external endpoints, run hello following commands.</span></span>

#### <a name="open-hello-port-for-myvm1"></a><span data-ttu-id="fce8b-161">Otevřete port hello pro myVM1:</span><span class="sxs-lookup"><span data-stu-id="fce8b-161">Open hello port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="fce8b-162">výsledky Hello by měl vypadat podobně jako toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="fce8b-162">hello results should look similar toohello following response:</span></span>

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

#### <a name="open-hello-port-for-myvm2"></a><span data-ttu-id="fce8b-163">Otevřete port hello pro Můjvp2:</span><span class="sxs-lookup"><span data-stu-id="fce8b-163">Open hello port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="fce8b-164">Připojit toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fce8b-164">Connect toohello virtual machine</span></span>

<span data-ttu-id="fce8b-165">Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fce8b-165">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="fce8b-166">Nahraďte IP adresu hello hello `publicIpAddress` virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fce8b-166">Replace hello IP address with hello `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh <publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a><span data-ttu-id="fce8b-167">Vytvořit databázi hello na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="fce8b-167">Create hello database on myVM1 (primary)</span></span>

<span data-ttu-id="fce8b-168">Hello Oracle softwaru je již nainstalován na bitovou kopii Marketplace hello, takže hello dalším krokem je tooinstall hello databáze.</span><span class="sxs-lookup"><span data-stu-id="fce8b-168">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> 

<span data-ttu-id="fce8b-169">Spusťte hello softwaru jako superuživatele, oracle, hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-169">Run hello software as hello 'oracle' superuser:</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="fce8b-170">Vytvořte databázi hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-170">Create hello database:</span></span>

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
<span data-ttu-id="fce8b-171">Výstupy by měl vypadat podobně jako toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="fce8b-171">Outputs should look similar toohello following response:</span></span>

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
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

<span data-ttu-id="fce8b-172">Nastavení proměnných ORACLE_SID a ORACLE_HOME hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-172">Set hello ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="fce8b-173">Volitelně můžete přidat ORACLE_HOME a ORACLE_SID toohello .bashrc souboru, tak, aby tato nastavení se uloží pro budoucí přihlášení:</span><span class="sxs-lookup"><span data-stu-id="fce8b-173">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future sign-ins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="fce8b-174">Spuštění nástroje pro sledování Oracle</span><span class="sxs-lookup"><span data-stu-id="fce8b-174">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-hello-database-on-myvm2-replicate"></a><span data-ttu-id="fce8b-175">Vytvořit databázi hello na Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="fce8b-175">Create hello database on myVM2 (replicate)</span></span>

```bash
sudo su - oracle
```
<span data-ttu-id="fce8b-176">Vytvořte databázi hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-176">Create hello database:</span></span>

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
<span data-ttu-id="fce8b-177">Nastavení proměnných ORACLE_SID a ORACLE_HOME hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-177">Set hello ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="fce8b-178">Volitelně můžete přidaný ORACLE_HOME a ORACLE_SID toohello .bashrc soubor, tak, aby tato nastavení se uloží pro budoucí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="fce8b-178">Optionally, you can added ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future sign-ins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="fce8b-179">Spuštění nástroje pro sledování Oracle</span><span class="sxs-lookup"><span data-stu-id="fce8b-179">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a><span data-ttu-id="fce8b-180">Konfigurace brány Golden</span><span class="sxs-lookup"><span data-stu-id="fce8b-180">Configure Golden Gate</span></span> 
<span data-ttu-id="fce8b-181">tooconfigure Golden brány, proveďte kroky hello v této části.</span><span class="sxs-lookup"><span data-stu-id="fce8b-181">tooconfigure Golden Gate, take hello steps in this section.</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="fce8b-182">Povolit režim protokolu archiv na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="fce8b-182">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="fce8b-183">Povolit protokolování platnost a ujistěte se, zda je přítomen alespoň jeden soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="fce8b-183">Enable force logging, and make sure at least one log file is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a><span data-ttu-id="fce8b-184">Stáhnout software Golden brány</span><span class="sxs-lookup"><span data-stu-id="fce8b-184">Download Golden Gate software</span></span>
<span data-ttu-id="fce8b-185">toodownload a příprava softwaru brány Golden Oracle hello, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fce8b-185">toodownload and prepare hello Oracle Golden Gate software, complete hello following steps:</span></span>

1. <span data-ttu-id="fce8b-186">Stáhnout hello **fbo_ggs_Linux_x64_shiphome.zip** soubor z hello [stránky pro stažení Oracle Golden brány](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="fce8b-186">Download hello **fbo_ggs_Linux_x64_shiphome.zip** file from hello [Oracle Golden Gate download page](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span></span> <span data-ttu-id="fce8b-187">V části hello stáhnout název **12.x.x.x Oracle GoldenGate pro Oracle Linux x86-64**, měla by existovat sadu toodownload soubory .zip.</span><span class="sxs-lookup"><span data-stu-id="fce8b-187">Under hello download title **Oracle GoldenGate 12.x.x.x for Oracle Linux x86-64**, there should be a set of .zip files toodownload.</span></span>

2. <span data-ttu-id="fce8b-188">Po stažení hello .zip soubory tooyour klientský počítač pomocí protokolu Secure kopírování (SCP) toocopy hello soubory tooyour virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="fce8b-188">After you download hello .zip files tooyour client computer, use Secure Copy Protocol (SCP) toocopy hello files tooyour VM:</span></span>

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. <span data-ttu-id="fce8b-189">Přesunout toohello soubory .zip hello **/ opt** složky.</span><span class="sxs-lookup"><span data-stu-id="fce8b-189">Move hello .zip files toohello **/opt** folder.</span></span> <span data-ttu-id="fce8b-190">Potom změňte vlastníka hello hello souborů následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fce8b-190">Then change hello owner of hello files as follows:</span></span>

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. <span data-ttu-id="fce8b-191">Rozbalte soubory hello (instalace hello Linux rozbalte nástroj, pokud ještě nejsou nainstalované):</span><span class="sxs-lookup"><span data-stu-id="fce8b-191">Unzip hello files (install hello Linux unzip utility if it's not already installed):</span></span>

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. <span data-ttu-id="fce8b-192">Změna oprávnění:</span><span class="sxs-lookup"><span data-stu-id="fce8b-192">Change permission:</span></span>

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-hello-client-and-vm-toorun-x11-for-windows-clients-only"></a><span data-ttu-id="fce8b-193">Příprava klienta hello a virtuálních počítačů toorun x11 (pro pouze klienty systému Windows)</span><span class="sxs-lookup"><span data-stu-id="fce8b-193">Prepare hello client and VM toorun x11 (for Windows clients only)</span></span>
<span data-ttu-id="fce8b-194">Toto je volitelný krok.</span><span class="sxs-lookup"><span data-stu-id="fce8b-194">This is an optional step.</span></span> <span data-ttu-id="fce8b-195">Pokud používáte klienta Linux nebo už máte x11, můžete přeskočit tento krok instalace.</span><span class="sxs-lookup"><span data-stu-id="fce8b-195">You can skip this step if you are using a Linux client or already have x11 setup.</span></span>

1. <span data-ttu-id="fce8b-196">Stáhněte si PuTTY a Xming tooyour počítač se systémem Windows:</span><span class="sxs-lookup"><span data-stu-id="fce8b-196">Download PuTTY and Xming tooyour Windows computer:</span></span>

  * [<span data-ttu-id="fce8b-197">Stáhněte si PuTTY</span><span class="sxs-lookup"><span data-stu-id="fce8b-197">Download PuTTY</span></span>](http://www.putty.org/)
  * [<span data-ttu-id="fce8b-198">Stáhnout Xming</span><span class="sxs-lookup"><span data-stu-id="fce8b-198">Download Xming</span></span>](https://xming.en.softonic.com/)

2.  <span data-ttu-id="fce8b-199">Po instalaci PuTTY v hello PuTTY složky (například C:\Program Files\PuTTY), spusťte puttygen.exe (generátor PuTTY klíč).</span><span class="sxs-lookup"><span data-stu-id="fce8b-199">After you install PuTTY, in hello PuTTY folder (for example, C:\Program Files\PuTTY), run puttygen.exe (PuTTY Key Generator).</span></span>

3.  <span data-ttu-id="fce8b-200">V generátoru PuTTY klíče:</span><span class="sxs-lookup"><span data-stu-id="fce8b-200">In PuTTY Key Generator:</span></span>

  - <span data-ttu-id="fce8b-201">toogenerate klíč, vyberte hello **generování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fce8b-201">toogenerate a key, select hello **Generate** button.</span></span>
  - <span data-ttu-id="fce8b-202">Kopírovat obsah hello hello klíče (**Ctrl + C**).</span><span class="sxs-lookup"><span data-stu-id="fce8b-202">Copy hello contents of hello key (**Ctrl+C**).</span></span>
  - <span data-ttu-id="fce8b-203">Vyberte hello **uložit privátní klíč** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fce8b-203">Select hello **Save private key** button.</span></span>
  - <span data-ttu-id="fce8b-204">Ignorovat upozornění hello, který se zobrazí a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="fce8b-204">Ignore hello warning that appears, and then select **OK**.</span></span>

    ![Snímek obrazovky stránky PuTTY klíče generátor hello](./media/oracle-golden-gate/puttykeygen.png)

4.  <span data-ttu-id="fce8b-206">Ve vašem virtuálním počítači spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="fce8b-206">In your VM, run these commands:</span></span>

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. <span data-ttu-id="fce8b-207">Vytvořte soubor s názvem **authorized_keys**.</span><span class="sxs-lookup"><span data-stu-id="fce8b-207">Create a file named **authorized_keys**.</span></span> <span data-ttu-id="fce8b-208">Vložte obsah hello hello klíče v tomto souboru a potom uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-208">Paste hello contents of hello key in this file, and then save hello file.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fce8b-209">Hello klíč musí obsahovat řetězec hello `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="fce8b-209">hello key must contain hello string `ssh-rsa`.</span></span> <span data-ttu-id="fce8b-210">Obsah hello hello klíče musí být také, jeden řádek textu.</span><span class="sxs-lookup"><span data-stu-id="fce8b-210">Also, hello contents of hello key must be a single line of text.</span></span>
  >  

6. <span data-ttu-id="fce8b-211">Spusťte PuTTY.</span><span class="sxs-lookup"><span data-stu-id="fce8b-211">Start PuTTY.</span></span> <span data-ttu-id="fce8b-212">V hello **kategorie** podokně, vyberte **připojení** > **SSH** > **Auth**. V hello **soubor privátního klíče pro ověřování** pole, procházet toohello klíč, který jste vygenerovali dříve.</span><span class="sxs-lookup"><span data-stu-id="fce8b-212">In hello **Category** pane, select **Connection** > **SSH** > **Auth**. In hello **Private key file for authentication** box, browse toohello key that you generated earlier.</span></span>

  ![Snímek obrazovky stránky hello nastavit privátní klíč](./media/oracle-golden-gate/setprivatekey.png)

7. <span data-ttu-id="fce8b-214">V hello **kategorie** podokně, vyberte **připojení** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="fce8b-214">In hello **Category** pane, select **Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="fce8b-215">Potom vyberte hello **povolit X11 předávání** pole.</span><span class="sxs-lookup"><span data-stu-id="fce8b-215">Then select hello **Enable X11 forwarding** box.</span></span>

  ![Snímek obrazovky stránky povolit X11 hello](./media/oracle-golden-gate/enablex11.png)

8. <span data-ttu-id="fce8b-217">V hello **kategorie** podokně přejděte příliš**relace**.</span><span class="sxs-lookup"><span data-stu-id="fce8b-217">In hello **Category** pane, go too**Session**.</span></span> <span data-ttu-id="fce8b-218">Zadejte informace o hostiteli hello a potom vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="fce8b-218">Enter hello host information, and then select **Open**.</span></span>

  ![Snímek obrazovky stránky relace hello](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a><span data-ttu-id="fce8b-220">Instalovat software Golden brány</span><span class="sxs-lookup"><span data-stu-id="fce8b-220">Install Golden Gate software</span></span>

<span data-ttu-id="fce8b-221">tooinstall Oracle Golden brány, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fce8b-221">tooinstall Oracle Golden Gate, complete hello following steps:</span></span>

1. <span data-ttu-id="fce8b-222">Přihlaste se jako oracle.</span><span class="sxs-lookup"><span data-stu-id="fce8b-222">Sign in as oracle.</span></span> <span data-ttu-id="fce8b-223">(Musí být schopný toosign v aniž byste byli vyzváni k zadání hesla.) Ujistěte se, že Xming běží před zahájením instalace hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-223">(You should be able toosign in without being prompted for a password.) Make sure that Xming is running before you begin hello installation.</span></span>
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. <span data-ttu-id="fce8b-224">Vyberte, Oracle GoldenGate pro databázi Oracle 12c'.</span><span class="sxs-lookup"><span data-stu-id="fce8b-224">Select 'Oracle GoldenGate for Oracle Database 12c'.</span></span> <span data-ttu-id="fce8b-225">Potom vyberte **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="fce8b-225">Then select **Next** toocontinue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte Instalační program hello](./media/oracle-golden-gate/golden_gate_install_01.png)

3. <span data-ttu-id="fce8b-227">Změnit umístění softwaru hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-227">Change hello software location.</span></span> <span data-ttu-id="fce8b-228">Potom vyberte hello **spustit správce** pole a zadejte umístění databáze hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-228">Then select  hello **Start Manager** box and enter hello database location.</span></span> <span data-ttu-id="fce8b-229">Vyberte **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="fce8b-229">Select **Next** toocontinue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_02.png)

4. <span data-ttu-id="fce8b-231">Změňte adresář hello inventáře a potom vyberte **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="fce8b-231">Change hello inventory directory, and then select **Next** toocontinue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_03.png)

5. <span data-ttu-id="fce8b-233">Na hello **Souhrn** obrazovku, vyberte **nainstalovat** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="fce8b-233">On hello **Summary** screen, select **Install** toocontinue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte Instalační program hello](./media/oracle-golden-gate/golden_gate_install_04.png)

6. <span data-ttu-id="fce8b-235">Může být výzvami toorun skript jako "kořenový".</span><span class="sxs-lookup"><span data-stu-id="fce8b-235">You might be prompted toorun a script as 'root'.</span></span> <span data-ttu-id="fce8b-236">Pokud ano, otevřete relaci samostatné ssh toohello virtuálních počítačů, sudo tooroot a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-236">If so, open a separate session, ssh toohello VM, sudo tooroot, and then run hello script.</span></span> <span data-ttu-id="fce8b-237">Vyberte **OK** pokračovat.</span><span class="sxs-lookup"><span data-stu-id="fce8b-237">Select **OK** continue.</span></span>

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_05.png)

7. <span data-ttu-id="fce8b-239">Po dokončení instalace hello vyberte **Zavřít** toocomplete hello procesu.</span><span class="sxs-lookup"><span data-stu-id="fce8b-239">When hello installation has finished, select **Close** toocomplete hello process.</span></span>

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="fce8b-241">Nastavení služby v myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="fce8b-241">Set up service on myVM1 (primary)</span></span>

1. <span data-ttu-id="fce8b-242">Vytvořit nebo aktualizovat souboru tnsnames.ora hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-242">Create or update hello tnsnames.ora file:</span></span>

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

2. <span data-ttu-id="fce8b-243">Vytvořte hello Golden brány vlastníka a uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="fce8b-243">Create hello Golden Gate owner and user accounts.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fce8b-244">Hello vlastníka účet musí mít předponu C ##.</span><span class="sxs-lookup"><span data-stu-id="fce8b-244">hello owner account must have C## prefix.</span></span>
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA tooC##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. <span data-ttu-id="fce8b-245">Vytvořte hello Golden brány testovací uživatelský účet:</span><span class="sxs-lookup"><span data-stu-id="fce8b-245">Create hello Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. <span data-ttu-id="fce8b-246">Nakonfigurujte soubor parametrů extrakce hello.</span><span class="sxs-lookup"><span data-stu-id="fce8b-246">Configure hello extract parameter file.</span></span>

 <span data-ttu-id="fce8b-247">Spuštění rozhraní příkazového řádku zlaté brány hello (ggsci):</span><span class="sxs-lookup"><span data-stu-id="fce8b-247">Start hello Golden gate command-line interface (ggsci):</span></span>

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
5. <span data-ttu-id="fce8b-248">Přidejte hello následující toohello EXTRAKCE parametr soubor (pomocí příkazů vi).</span><span class="sxs-lookup"><span data-stu-id="fce8b-248">Add hello following toohello EXTRACT parameter file (by using vi commands).</span></span> <span data-ttu-id="fce8b-249">Stisknutím klávesy Esc, ': QW!.</span><span class="sxs-lookup"><span data-stu-id="fce8b-249">Press Esc key, ':wq!'</span></span> <span data-ttu-id="fce8b-250">toosave soubor.</span><span class="sxs-lookup"><span data-stu-id="fce8b-250">toosave file.</span></span> 

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
6. <span data-ttu-id="fce8b-251">Extrahujte registrace--integrované extrakce:</span><span class="sxs-lookup"><span data-stu-id="fce8b-251">Register extract--integrated extract:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. <span data-ttu-id="fce8b-252">Nastavit extrakce kontrolní body a spustit v reálném čase extrakce:</span><span class="sxs-lookup"><span data-stu-id="fce8b-252">Set up extract checkpoints and start real-time extract:</span></span>

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request tooMANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
<span data-ttu-id="fce8b-253">V tomto kroku najít hello od oznámení změny stavu, který se použije později v jiné části:</span><span class="sxs-lookup"><span data-stu-id="fce8b-253">In this step, you find hello starting SCN, which will be used later, in a different section:</span></span>

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

### <a name="set-up-service-on-myvm2-replicate"></a><span data-ttu-id="fce8b-254">Nastavení služby v Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="fce8b-254">Set up service on myVM2 (replicate)</span></span>


1. <span data-ttu-id="fce8b-255">Vytvořit nebo aktualizovat souboru tnsnames.ora hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-255">Create or update hello tnsnames.ora file:</span></span>

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

2. <span data-ttu-id="fce8b-256">Vytvořte účet replikace:</span><span class="sxs-lookup"><span data-stu-id="fce8b-256">Create a replicate account:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba toorepuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. <span data-ttu-id="fce8b-257">Vytvoření uživatelského účtu testovací Golden brány:</span><span class="sxs-lookup"><span data-stu-id="fce8b-257">Create a Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. <span data-ttu-id="fce8b-258">REPLICAT parametr souboru tooreplicate změny:</span><span class="sxs-lookup"><span data-stu-id="fce8b-258">REPLICAT parameter file tooreplicate changes:</span></span> 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  <span data-ttu-id="fce8b-259">Obsah souboru REPORA parametr:</span><span class="sxs-lookup"><span data-stu-id="fce8b-259">Content of REPORA parameter file:</span></span>

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

5. <span data-ttu-id="fce8b-260">Nastavte kontrolní bod replicat:</span><span class="sxs-lookup"><span data-stu-id="fce8b-260">Set up a replicat checkpoint:</span></span>

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

### <a name="set-up-hello-replication-myvm1-and-myvm2"></a><span data-ttu-id="fce8b-261">Nastavení replikace hello (myVM1 a Můjvp2)</span><span class="sxs-lookup"><span data-stu-id="fce8b-261">Set up hello replication (myVM1 and myVM2)</span></span>

#### <a name="1-set-up-hello-replication-on-myvm2-replicate"></a><span data-ttu-id="fce8b-262">1. Nastavení replikace hello na Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="fce8b-262">1. Set up hello replication on myVM2 (replicate)</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
<span data-ttu-id="fce8b-263">Aktualizujte soubor hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="fce8b-263">Update hello file with hello following:</span></span>

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
<span data-ttu-id="fce8b-264">Potom restartujte službu Manager hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-264">Then restart hello Manager service:</span></span>

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-hello-replication-on-myvm1-primary"></a><span data-ttu-id="fce8b-265">2. Nastavení replikace hello na myVM1 (primární)</span><span class="sxs-lookup"><span data-stu-id="fce8b-265">2. Set up hello replication on myVM1 (primary)</span></span>

<span data-ttu-id="fce8b-266">Spusťte počáteční zatížení hello a zkontrolujte chyby:</span><span class="sxs-lookup"><span data-stu-id="fce8b-266">Start hello initial load and check for errors:</span></span>

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-hello-replication-on-myvm2-replicate"></a><span data-ttu-id="fce8b-267">3. Nastavení replikace hello na Můjvp2 (Replikovat)</span><span class="sxs-lookup"><span data-stu-id="fce8b-267">3. Set up hello replication on myVM2 (replicate)</span></span>

<span data-ttu-id="fce8b-268">Změna hello oznámení změny stavu číslo s číslem hello jste předtím získali:</span><span class="sxs-lookup"><span data-stu-id="fce8b-268">Change hello SCN number with hello number you obtained before:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
<span data-ttu-id="fce8b-269">zahájení replikace Hello a jej můžete otestovat pomocí vkládání nových záznamů tooTEST tabulek.</span><span class="sxs-lookup"><span data-stu-id="fce8b-269">hello replication has begun, and you can test it by inserting new records tooTEST tables.</span></span>


### <a name="view-job-status-and-troubleshooting"></a><span data-ttu-id="fce8b-270">Zobrazení stavu úlohy a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="fce8b-270">View job status and troubleshooting</span></span>

#### <a name="view-reports"></a><span data-ttu-id="fce8b-271">Zobrazení sestav</span><span class="sxs-lookup"><span data-stu-id="fce8b-271">View reports</span></span>
<span data-ttu-id="fce8b-272">tooview hlásí myVM1, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-272">tooview reports on myVM1, run hello following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
<span data-ttu-id="fce8b-273">tooview hlásí Můjvp2, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-273">tooview reports on myVM2, run hello following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a><span data-ttu-id="fce8b-274">Zobrazit stav a historie</span><span class="sxs-lookup"><span data-stu-id="fce8b-274">View status and history</span></span>
<span data-ttu-id="fce8b-275">tooview stav a historie na myVM1, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-275">tooview status and history on myVM1, run hello following commands:</span></span>

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

<span data-ttu-id="fce8b-276">tooview stav a historie na Můjvp2, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="fce8b-276">tooview status and history on myVM2, run hello following commands:</span></span>

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
<span data-ttu-id="fce8b-277">Tím se dokončí hello instalaci a konfiguraci brány Golden na Oracle linux.</span><span class="sxs-lookup"><span data-stu-id="fce8b-277">This completes hello installation and configuration of Golden Gate on Oracle linux.</span></span>


## <a name="delete-hello-virtual-machine"></a><span data-ttu-id="fce8b-278">Odstranění hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fce8b-278">Delete hello virtual machine</span></span>

<span data-ttu-id="fce8b-279">Když už je potřeba, může být hello následující příkaz skupiny prostředků použít tooremove hello, virtuálních počítačů a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="fce8b-279">When it's no longer needed, hello following command can be used tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="fce8b-280">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fce8b-280">Next steps</span></span>

[<span data-ttu-id="fce8b-281">Kurz vytvoření virtuálního počítače s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="fce8b-281">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="fce8b-282">Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="fce8b-282">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
