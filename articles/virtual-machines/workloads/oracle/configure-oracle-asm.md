---
title: "aaaSet až Oracle ASM na virtuálním počítači Azure Linux | Microsoft Docs"
description: "Rychle získáte Oracle ASM nahoru a spouštění v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: d6a7046638e919876477d46943faabcb1872acac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="ebeb9-103">Nastavit Oracle ASM na virtuálním počítači Azure Linux</span><span class="sxs-lookup"><span data-stu-id="ebeb9-103">Set up Oracle ASM on an Azure Linux virtual machine</span></span>  

<span data-ttu-id="ebeb9-104">Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="ebeb9-105">Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení v kombinaci s hello instalace a konfigurace systému Oracle automatizované úložiště ASM (Správa).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-105">This tutorial covers basic Azure virtual machine deployment combined with hello installation and configuration of Oracle Automated Storage Management (ASM).</span></span>  <span data-ttu-id="ebeb9-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ebeb9-107">Vytvořit a připojit tooan databáze Oracle virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ebeb9-107">Create and connect tooan Oracle Database VM</span></span>
> * <span data-ttu-id="ebeb9-108">Instalace a konfigurace Oracle automatizovaná správa úložiště</span><span class="sxs-lookup"><span data-stu-id="ebeb9-108">Install and configure Oracle Automated Storage Management</span></span>
> * <span data-ttu-id="ebeb9-109">Instalace a konfigurace infrastruktury Oracle mřížky</span><span class="sxs-lookup"><span data-stu-id="ebeb9-109">Install and configure Oracle Grid infrastructure</span></span>
> * <span data-ttu-id="ebeb9-110">Inicializace instalace Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="ebeb9-110">Initialize an Oracle ASM installation</span></span>
> * <span data-ttu-id="ebeb9-111">Vytvoření Oracle DB spravuje ASM</span><span class="sxs-lookup"><span data-stu-id="ebeb9-111">Create an Oracle DB managed by ASM</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ebeb9-112">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ebeb9-113">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ebeb9-114">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-hello-environment"></a><span data-ttu-id="ebeb9-115">Příprava prostředí hello</span><span class="sxs-lookup"><span data-stu-id="ebeb9-115">Prepare hello environment</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="ebeb9-116">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ebeb9-116">Create a resource group</span></span>

<span data-ttu-id="ebeb9-117">toocreate skupinu prostředků použijte hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-117">toocreate a resource group, use hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="ebeb9-118">Skupinu prostředků Azure je logický kontejner, ve které jsou nasazené a spravované prostředky.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-118">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> <span data-ttu-id="ebeb9-119">V tomto příkladu skupinu prostředků s názvem *myResourceGroup* v hello *eastus* oblast.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-119">In this example, a resource group named *myResourceGroup* in hello *eastus* region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a><span data-ttu-id="ebeb9-120">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ebeb9-120">Create a VM</span></span>

<span data-ttu-id="ebeb9-121">toocreate virtuálního počítače na základě bitové kopie databáze Oracle hello a nakonfigurovat ji toouse Oracle ASM, použijte hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-121">toocreate a virtual machine based on hello Oracle Database image and configure it toouse Oracle ASM, use hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="ebeb9-122">Hello následující příklad vytvoří virtuální počítač s názvem můjvp přesměrovat, který je velikost Standard_DS2_v2 s čtyři připojené datových disků 50 GB.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-122">hello following example creates a VM named myVM that is a Standard_DS2_v2 size with four attached data disks of 50 GB each.</span></span> <span data-ttu-id="ebeb9-123">Pokud už neexistují v hello umístění klíče výchozí, vytvoří také klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-123">If they do not already exist in hello default key location, it also creates SSH keys.</span></span>  <span data-ttu-id="ebeb9-124">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-124">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

<span data-ttu-id="ebeb9-125">Po vytvoření hello virtuálních počítačů Azure CLI zobrazí informace podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-125">After you create hello VM, Azure CLI displays information similar toohello following example.</span></span> <span data-ttu-id="ebeb9-126">Poznamenejte si hodnotu hello pro `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-126">Note hello value for `publicIpAddress`.</span></span> <span data-ttu-id="ebeb9-127">Můžete použít tuto adresu tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-127">You use this address tooaccess hello VM.</span></span>

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-toohello-vm"></a><span data-ttu-id="ebeb9-128">Připojit toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ebeb9-128">Connect toohello VM</span></span>

<span data-ttu-id="ebeb9-129">toocreate na relace SSH s hello virtuálních počítačů a konfigurovat další nastavení, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-129">toocreate an SSH session with hello VM and configure additional settings, use hello following command.</span></span> <span data-ttu-id="ebeb9-130">Nahraďte IP adresu hello hello `publicIpAddress` hodnotu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-130">Replace hello IP address with hello `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a><span data-ttu-id="ebeb9-131">Nainstalujte Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="ebeb9-131">Install Oracle ASM</span></span>

<span data-ttu-id="ebeb9-132">tooinstall ASM Oracle, dokončení hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-132">tooinstall Oracle ASM, complete hello following steps.</span></span> 

<span data-ttu-id="ebeb9-133">Další informace o instalaci Oracle ASM najdete v tématu [Oracle ASMLib soubory ke stažení pro Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-133">For more information about installing Oracle ASM, see [Oracle ASMLib Downloads for Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).</span></span>  

1. <span data-ttu-id="ebeb9-134">Je třeba toologin jako kořenového adresáře v pořadí toocontinue s instalací ASM:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-134">You need toologin as root in order toocontinue with ASM installation:</span></span>

   ```bash
   sudo su -
   ```
   
2. <span data-ttu-id="ebeb9-135">Spusťte tyto další příkazy tooinstall Oracle ASM součásti:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-135">Run these additional commands tooinstall Oracle ASM components:</span></span>

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. <span data-ttu-id="ebeb9-136">Ověřte, zda je nainstalován Oracle ASM:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-136">Verify that Oracle ASM is installed:</span></span>

   ```bash
   rpm -qa |grep oracleasm
   ```

    <span data-ttu-id="ebeb9-137">Hello výstup tohoto příkazu by měl obsahovat hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-137">hello output of this command should list hello following components:</span></span>

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. <span data-ttu-id="ebeb9-138">ASM vyžaduje konkrétní uživatele a role v pořadí toofunction správně.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-138">ASM requires specific users and roles in order toofunction correctly.</span></span> <span data-ttu-id="ebeb9-139">Následující příkazy Hello vytvořit hello předběžné uživatelské účty a skupiny:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-139">hello following commands create hello pre-requisite user accounts and groups:</span></span> 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. <span data-ttu-id="ebeb9-140">Ověření uživatelů a skupin byly vytvořeny správně:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-140">Verify users and groups were created correctly:</span></span>

   ```bash
   id grid
   ```

    <span data-ttu-id="ebeb9-141">Hello výstup tohoto příkazu by měl obsahovat následující hello uživatelů a skupin:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-141">hello output of this command should list hello following users and groups:</span></span>

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. <span data-ttu-id="ebeb9-142">Vytvořte složku pro uživatele *mřížky* a změnit vlastníka hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-142">Create a folder for user *grid* and change hello owner:</span></span>

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a><span data-ttu-id="ebeb9-143">Nastavit Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="ebeb9-143">Set up Oracle ASM</span></span>

<span data-ttu-id="ebeb9-144">V tomto kurzu hello výchozího uživatele je *mřížky* a hello výchozí skupina je *asmadmin*.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-144">For this tutorial, hello default user is *grid* and hello default group is *asmadmin*.</span></span> <span data-ttu-id="ebeb9-145">Ujistěte se, že hello *oracle* uživatel je součástí skupiny asmadmin hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-145">Ensure that hello *oracle* user is part of hello asmadmin group.</span></span> <span data-ttu-id="ebeb9-146">tooset Oracle ASM Installation, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-146">tooset up your Oracle ASM installation, complete hello following steps:</span></span>

1. <span data-ttu-id="ebeb9-147">Nastavení hello Oracle ASM knihovny ovladačů zahrnuje definování hello výchozího uživatele (mřížky) a výchozí skupiny (asmadmin) a také konfigurace hello jednotky toostart na spouštěcí (zvolte y) a tooscan pro disky na spouštěcí (zvolte y).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-147">Setting up hello Oracle ASM library driver involves defining hello default user (grid) and default group (asmadmin) as well as configuring hello drive toostart on boot (choose y) and tooscan for disks on boot (choose y).</span></span> <span data-ttu-id="ebeb9-148">Je třeba tooanswer hello výzvy z hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-148">You need tooanswer hello prompts from hello following command:</span></span>

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   <span data-ttu-id="ebeb9-149">Hello výstup tohoto příkazu by měla vypadat podobně jako toohello následující, zastavuje s toobe výzvy odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-149">hello output of this command should look similar toohello following, stopping with prompts toobe answered.</span></span>

    ```bash
   Configuring hello Oracle ASM library driver.

   This will configure hello on-boot properties of hello Oracle ASM library
   driver. hello following questions will determine whether hello driver is
   loaded on boot and what permissions it will have. hello current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user tooown hello driver interface []: grid
   Default group tooown hello driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. <span data-ttu-id="ebeb9-150">Konfigurace disku hello zobrazení:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-150">View hello disk configuration:</span></span>
   ```bash
   cat /proc/partitions
   ```

   <span data-ttu-id="ebeb9-151">Hello výstup tohoto příkazu by měl vypadat podobně jako toohello následující seznam dostupných disků</span><span class="sxs-lookup"><span data-stu-id="ebeb9-151">hello output of this command should look similar toohello following listing of available disks</span></span>

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. <span data-ttu-id="ebeb9-152">Formát disku */dev/sdc* spuštěním hello následující příkaz a volaného hello výzvy k:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-152">Format disk */dev/sdc* by running hello following command and answering hello prompts with:</span></span>
   - <span data-ttu-id="ebeb9-153">*n*pro nový oddíl</span><span class="sxs-lookup"><span data-stu-id="ebeb9-153">*n* for new partition</span></span>
   - <span data-ttu-id="ebeb9-154">*p* pro primární oddíl</span><span class="sxs-lookup"><span data-stu-id="ebeb9-154">*p* for primary partition</span></span>
   - <span data-ttu-id="ebeb9-155">*1* tooselect hello první oddíl</span><span class="sxs-lookup"><span data-stu-id="ebeb9-155">*1* tooselect hello first partition</span></span>
   - <span data-ttu-id="ebeb9-156">stiskněte klávesu `enter` pro první cylindr výchozí hello</span><span class="sxs-lookup"><span data-stu-id="ebeb9-156">press `enter` for hello default first cylinder</span></span>
   - <span data-ttu-id="ebeb9-157">stiskněte klávesu `enter` pro hello výchozí poslední cylindr</span><span class="sxs-lookup"><span data-stu-id="ebeb9-157">press `enter` for hello default last cylinder</span></span>
   - <span data-ttu-id="ebeb9-158">stiskněte klávesu *w* toowrite hello změny toohello oddílu tabulky</span><span class="sxs-lookup"><span data-stu-id="ebeb9-158">press *w* toowrite hello changes toohello partition table</span></span>  

   ```bash
   fdisk /dev/sdc
   ```
   
   <span data-ttu-id="ebeb9-159">Pomocí výše uvedeného odpovědi hello, hello výstup hello fdisk příkazu by měl vypadat podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-159">Using hello answers provided above, hello output for hello fdisk command should look like hello following:</span></span>

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide toowrite them.
   After that, of course, hello previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   hello device presents a logical sector size that is smaller than
   hello physical sector size. Aligning tooa physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off hello mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   hello partition table has been altered!

   Calling ioctl() toore-read partition table.
   Syncing disks.
   ```

4. <span data-ttu-id="ebeb9-160">Opakování hello předcházející příkaz fdisk pro `/dev/sdd`, `/dev/sde`, a `/dev/sdf`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-160">Repeat hello preceding fdisk command for `/dev/sdd`, `/dev/sde`, and `/dev/sdf`.</span></span>

5. <span data-ttu-id="ebeb9-161">Kontrola konfigurace disku hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-161">Check hello disk configuration:</span></span>

   ```bash
   cat /proc/partitions
   ```

   <span data-ttu-id="ebeb9-162">výstup Hello hello příkazu by měl vypadat podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-162">hello output of hello command should look like hello following:</span></span>

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. <span data-ttu-id="ebeb9-163">Zkontrolujte stav služby hello Oracle ASM a spuštění služby hello Oracle ASM:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-163">Check hello Oracle ASM service status and start hello Oracle ASM service:</span></span>

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   <span data-ttu-id="ebeb9-164">výstup Hello hello příkazu by měl vypadat podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-164">hello output of hello command should look like hello following:</span></span>
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing hello Oracle ASMLib driver:                     [  OK  ]
   Scanning hello system for Oracle ASMLib disks:               [  OK  ]
   ```

7. <span data-ttu-id="ebeb9-165">Vytvořte disky Oracle ASM:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-165">Create Oracle ASM disks:</span></span>

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   <span data-ttu-id="ebeb9-166">výstup Hello hello příkazu by měl vypadat podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-166">hello output of hello command should look like hello following:</span></span>

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. <span data-ttu-id="ebeb9-167">Seznam disků Oracle ASM:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-167">List Oracle ASM disks:</span></span>

   ```bash
   service oracleasm listdisks
   ```   

   <span data-ttu-id="ebeb9-168">výstup Hello hello příkazu by měl seznamu vypnout hello následující disky Oracle ASM:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-168">hello output of hello command should list off hello following Oracle ASM disks:</span></span>

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. <span data-ttu-id="ebeb9-169">Změnit hello hesla pro uživatele root, oracle a mřížky hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-169">Change hello passwords for hello root, oracle, and grid users.</span></span> <span data-ttu-id="ebeb9-170">**Poznamenejte si tyto nová hesla** jako jejich použijete později v průběhu instalace hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-170">**Make note of these new passwords** as you are using them later during hello installation.</span></span>

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. <span data-ttu-id="ebeb9-171">Změna oprávnění složky hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-171">Change hello folder permission:</span></span>

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a><span data-ttu-id="ebeb9-172">Stáhněte si a příprava infrastruktury mřížky Oracle</span><span class="sxs-lookup"><span data-stu-id="ebeb9-172">Download and prepare Oracle Grid Infrastructure</span></span>

<span data-ttu-id="ebeb9-173">toodownload a příprava softwaru Oracle mřížky infrastruktury hello, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-173">toodownload and prepare hello Oracle Grid Infrastructure software, complete hello following steps:</span></span>

1. <span data-ttu-id="ebeb9-174">Stáhnout Oracle mřížky infrastruktury z hello [stránky pro stažení Oracle ASM](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-174">Download Oracle Grid Infrastructure from hello [Oracle ASM download page](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html).</span></span> 

   <span data-ttu-id="ebeb9-175">V části s názvem stažení hello **databáze Oracle 12c verze 1 mřížky infrastruktury (12.1.0.2.0) pro Linux x86-64**, stáhnout hello dva soubory .zip.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-175">Under hello download titled **Oracle Database 12c Release 1 Grid Infrastructure (12.1.0.2.0) for Linux x86-64**, download hello two .zip files.</span></span>

2. <span data-ttu-id="ebeb9-176">Po stažení hello .zip soubory tooyour klientský počítač, můžete použít protokol Secure kopírování (SCP) toocopy hello soubory tooyour virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-176">After you download hello .zip files tooyour client computer, you can use Secure Copy Protocol (SCP) toocopy hello files tooyour VM:</span></span>

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. <span data-ttu-id="ebeb9-177">SSH zpět do Oracle virtuálního počítače v Azure v pořadí toomove hello .zip soubory do hello / opt složky.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-177">SSH back into your Oracle VM in Azure in order toomove hello .zip files into hello /opt folder.</span></span> <span data-ttu-id="ebeb9-178">Potom změňte vlastníka hello hello souborů:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-178">Then, change hello owner of hello files:</span></span>

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. <span data-ttu-id="ebeb9-179">Rozbalte soubory hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-179">Unzip hello files.</span></span> <span data-ttu-id="ebeb9-180">(Instalace hello Linux nástroj rozbalte, pokud je ještě není nainstalován.)</span><span class="sxs-lookup"><span data-stu-id="ebeb9-180">(Install hello Linux unzip tool if it's not already installed.)</span></span>
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. <span data-ttu-id="ebeb9-181">Změna oprávnění:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-181">Change permission:</span></span>
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. <span data-ttu-id="ebeb9-182">Aktualizace nakonfigurované velikosti odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-182">Update configured swap space.</span></span> <span data-ttu-id="ebeb9-183">Oracle mřížky součásti potřebovat aspoň 6.8 GB tooinstall místa odkládacího souboru mřížky.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-183">Oracle Grid components need at least 6.8 GB of swap space tooinstall Grid.</span></span> <span data-ttu-id="ebeb9-184">Hello velikosti odkládacího souboru výchozí pro Oracle Linux bitové kopie v Azure je 2 048 MB paměti.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-184">hello default swap file size for Oracle Linux images in Azure is only 2048MB.</span></span> <span data-ttu-id="ebeb9-185">Je třeba tooincrease `ResourceDisk.SwapSizeMB` v hello `/etc/waagent.conf` soubor a po restartování služby WALinuxAgent hello hello aktualizovat nastavení tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-185">You need tooincrease `ResourceDisk.SwapSizeMB` in hello `/etc/waagent.conf` file and restart hello WALinuxAgent service in order for hello updated settings tootake effect.</span></span> <span data-ttu-id="ebeb9-186">Protože je jen pro čtení souboru, potřebujete přístup pro zápis tooenable toochange souboru oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-186">Because it is a read-only file, you need toochange file permissions tooenable write access.</span></span>

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   <span data-ttu-id="ebeb9-187">Vyhledejte `ResourceDisk.SwapSizeMB` a změňte hodnotu hello příliš**8192**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-187">Search for `ResourceDisk.SwapSizeMB` and change hello value too**8192**.</span></span> <span data-ttu-id="ebeb9-188">Budete potřebovat toopress `insert` tooenter vložení režimu, zadejte hodnotu hello **8192** a potom stiskněte klávesu `esc` tooreturn toocommand režimu.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-188">You will need toopress `insert` tooenter insert mode, type in hello value of **8192** and then press `esc` tooreturn toocommand mode.</span></span> <span data-ttu-id="ebeb9-189">toowrite hello změny a ukončete hello soubor, zadejte `:wq` a stiskněte klávesu `enter`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-189">toowrite hello changes and quit hello file, type `:wq` and press `enter`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ebeb9-190">Důrazně doporučujeme vždy použít `WALinuxAgent` tooconfigure záměna prostoru tak, aby se vždy vytvoří na hello místní dočasné disk (disk dočasné) pro nejlepší výkon.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-190">We highly recommend that you always use `WALinuxAgent` tooconfigure swap space so that it's always created on hello local ephemeral disk (temporary disk) for best performance.</span></span> <span data-ttu-id="ebeb9-191">Další informace o najdete v tématu [jak tooadd prohození souborů ve virtuálních počítačích Linux Azure](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-191">For more information on, see [How tooadd a swap file in Linux Azure virtual machines](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).</span></span>

## <a name="prepare-your-local-client-and-vm-toorun-x11"></a><span data-ttu-id="ebeb9-192">Příprava místního klienta a virtuálních počítačů toorun x11</span><span class="sxs-lookup"><span data-stu-id="ebeb9-192">Prepare your local client and VM toorun x11</span></span>
<span data-ttu-id="ebeb9-193">Konfigurace Oracle ASM vyžaduje grafické rozhraní toocomplete hello instalace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-193">Configuring Oracle ASM requires a graphical interface toocomplete hello install and configuration.</span></span> <span data-ttu-id="ebeb9-194">Používáme protokol toofacilitate hello x11 této instalace.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-194">We are using hello x11 protocol toofacilitate this installation.</span></span> <span data-ttu-id="ebeb9-195">Pokud používáte systém klienta (Mac nebo Linux), který už má X11 funkce povolena a konfigurován – můžete přeskočit tuto konfiguraci a nastavení výhradní tooWindows počítače.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-195">If you are using a client system (Mac or Linux) that already has X11 capabilities enabled and configured - you can skip this configuration and setup exclusive tooWindows machines.</span></span> 

1. <span data-ttu-id="ebeb9-196">[Stáhněte si PuTTY](http://www.putty.org/) a [stáhnout Xming](https://xming.en.softonic.com/) tooyour počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-196">[Download PuTTY](http://www.putty.org/) and [download Xming](https://xming.en.softonic.com/) tooyour Windows computer.</span></span> <span data-ttu-id="ebeb9-197">Budete potřebovat instalaci hello toocomplete obě tyto aplikace s výchozími hodnotami hello než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-197">You will need toocomplete hello installation of both of these applications with hello default values before proceeding.</span></span>

2. <span data-ttu-id="ebeb9-198">Po instalaci PuTTY, otevřete příkazový řádek, změňte do hello PuTTY složky (například C:\Program Files\PuTTY) a spusťte `puttygen.exe` v pořadí toogenerate klíč.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-198">After you install PuTTY, open a command prompt, change into hello PuTTY folder (for example, C:\Program Files\PuTTY), and run `puttygen.exe` in order toogenerate a key.</span></span>

3. <span data-ttu-id="ebeb9-199">V generátoru PuTTY klíče:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-199">In PuTTY Key Generator:</span></span>
   
   1. <span data-ttu-id="ebeb9-200">Vygenerovat klíč výběrem hello `Generate` tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-200">Generate a key by selecting hello `Generate` button.</span></span>
   2. <span data-ttu-id="ebeb9-201">Kopírovat obsah hello hello klíče (Ctrl + C).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-201">Copy hello contents of hello key (Ctrl+C).</span></span>
   3. <span data-ttu-id="ebeb9-202">Vyberte hello `Save private key` tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-202">Select hello `Save private key` button.</span></span>
   4. <span data-ttu-id="ebeb9-203">Ignorovat upozornění hello o zabezpečení hello klíč s přístupové heslo a potom vyberte `OK`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-203">Ignore hello warning about securing hello key with a passphrase, and then select `OK`.</span></span>

   ![Snímek obrazovky PuTTY klíče generátor](./media/oracle-asm/puttykeygen.png)

4. <span data-ttu-id="ebeb9-205">Ve vašem virtuálním počítači spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-205">In your VM, run these commands:</span></span>

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. <span data-ttu-id="ebeb9-206">Vytvořte soubor s názvem `authorized_keys`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-206">Create a file named `authorized_keys`.</span></span> <span data-ttu-id="ebeb9-207">Vložte obsah hello hello klíče v tomto souboru a potom uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-207">Paste hello contents of hello key in this file, and then save hello file.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ebeb9-208">Hello klíč musí obsahovat řetězec hello `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-208">hello key must contain hello string `ssh-rsa`.</span></span> <span data-ttu-id="ebeb9-209">Obsah hello hello klíče musí být také, jeden řádek textu.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-209">Also, hello contents of hello key must be a single line of text.</span></span>
   >  

6. <span data-ttu-id="ebeb9-210">V systému klienta spusťte PuTTY.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-210">On your client system, start PuTTY.</span></span> <span data-ttu-id="ebeb9-211">V hello **kategorie** podokně přejděte příliš**připojení** > **SSH** > **Auth**. V hello **soubor privátního klíče pro ověřování** pole, procházet toohello klíč, který jste vygenerovali dříve.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-211">In hello **Category** pane, go too**Connection** > **SSH** > **Auth**. In hello **Private key file for authentication** box, browse toohello key that you generated earlier.</span></span>

   ![Snímek obrazovky možností ověřování SSH hello](./media/oracle-asm/setprivatekey.png)

7. <span data-ttu-id="ebeb9-213">V hello **kategorie** podokně přejděte příliš**připojení** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-213">In hello **Category** pane, go too**Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="ebeb9-214">Vyberte hello **povolit X11 předávání** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-214">Select hello **Enable X11 forwarding** check box.</span></span>

   ![Snímek obrazovky hello SSH X11 předávání možnosti](./media/oracle-asm/enablex11.png)

8. <span data-ttu-id="ebeb9-216">V hello **kategorie** podokně přejděte příliš**relace**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-216">In hello **Category** pane, go too**Session**.</span></span> <span data-ttu-id="ebeb9-217">Zadejte virtuální počítač ASM Oracle `<publicIPaddress>` ve hello hostitele název dialogové okno, zadejte nový `Saved Session` název a potom klikněte na `Save`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-217">Enter your Oracle ASM VM `<publicIPaddress>` in hello host name dialog box, fill in a new `Saved Session` name and then click on `Save`.</span></span>  <span data-ttu-id="ebeb9-218">Po uložení, klikněte na `open` tooconnect tooyour Oracle ASM virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-218">Once saved, click on `open` tooconnect tooyour Oracle ASM virtual machine.</span></span>  <span data-ttu-id="ebeb9-219">Hello poprvé připojíte, budete upozorněni, že hello vzdáleného systému není uložena v mezipaměti v registru.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-219">hello first time you connect you are warned  hello remote system is not cached in your registry.</span></span> <span data-ttu-id="ebeb9-220">Klikněte na `yes` tooadd ho a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-220">Click on `yes` tooadd it and continue.</span></span>

   ![Snímek obrazovky možnosti relace PuTTY hello](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a><span data-ttu-id="ebeb9-222">Instalace infrastruktury mřížky Oracle</span><span class="sxs-lookup"><span data-stu-id="ebeb9-222">Install Oracle Grid Infrastructure</span></span>

<span data-ttu-id="ebeb9-223">tooinstall Oracle mřížky infrastruktury, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-223">tooinstall Oracle Grid Infrastructure, complete hello following steps:</span></span>

1. <span data-ttu-id="ebeb9-224">Přihlaste se jako **mřížky**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-224">Sign in as **grid**.</span></span> <span data-ttu-id="ebeb9-225">(Musí být schopný toosign v aniž byste byli vyzváni k zadání hesla.)</span><span class="sxs-lookup"><span data-stu-id="ebeb9-225">(You should be able toosign in without being prompted for a password.)</span></span> 

   > [!NOTE]
   > <span data-ttu-id="ebeb9-226">Pokud používáte systém Windows, ujistěte se, že jste spustili Xming před zahájením instalace hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-226">If you are running Windows, make sure you have started Xming before you begin hello installation.</span></span>

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   <span data-ttu-id="ebeb9-227">Otevře se Oracle mřížky infrastruktury 12c instalační program verze 1.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-227">Oracle Grid Infrastructure 12c Release 1 Installer opens.</span></span> <span data-ttu-id="ebeb9-228">(To může trvat několik minut, než instalační program toostart hello.)</span><span class="sxs-lookup"><span data-stu-id="ebeb9-228">(It might take a few minutes for hello  installer toostart.)</span></span>

2. <span data-ttu-id="ebeb9-229">Na hello **vyberte možnost instalace** vyberte **instalace a konfigurace infrastruktury mřížky Oracle pro samostatný Server**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-229">On hello **Select Installation Option** page, select **Install and Configure Oracle Grid Infrastructure for a Standalone Server**.</span></span>

   ![Snímek obrazovky stránky vyberte možnost instalace instalační hello](./media/oracle-asm/install01.png)

3. <span data-ttu-id="ebeb9-231">Na hello **vyberte jazyky produktu** zkontrolujte **Angličtina** nebo je hello jazyk, který chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-231">On hello **Select Product Languages** page, ensure **English** or hello language that you want is selected.</span></span>  <span data-ttu-id="ebeb9-232">Klikněte na `next` (Další).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-232">Click `next`.</span></span>

4. <span data-ttu-id="ebeb9-233">Na hello **vytvořit skupinu disku ASM** stránky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-233">On hello **Create ASM Disk Group** page:</span></span>
   - <span data-ttu-id="ebeb9-234">Zadejte název pro skupinu disku hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-234">Enter a name for hello disk group.</span></span>
   - <span data-ttu-id="ebeb9-235">V části **redundance**, vyberte **externí**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-235">Under **Redundancy**, select **External**.</span></span>
   - <span data-ttu-id="ebeb9-236">V části **velikost alokační jednotky**, vyberte **4**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-236">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="ebeb9-237">V části **přidat disky**, vyberte **ORCLASMSP**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-237">Under **Add Disks**, select **ORCLASMSP**.</span></span>
   - <span data-ttu-id="ebeb9-238">Klikněte na `next` (Další).</span><span class="sxs-lookup"><span data-stu-id="ebeb9-238">Click `next`.</span></span>

5. <span data-ttu-id="ebeb9-239">Na hello **zadejte heslo ASM** stránky, vyberte hello **používat stejné hesla pro tyto účty** možnost a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-239">On hello **Specify ASM Password** page, select hello **Use same passwords for these accounts** option, and enter a password.</span></span>

   ![Snímek obrazovky stránky zadejte heslo ASM instalační hello](./media/oracle-asm/install04.png)

6. <span data-ttu-id="ebeb9-241">Na hello **zadejte možnosti správy** stránky, máte možnost tooconfigure hello EM cloudu řízení.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-241">On hello **Specify Management Options** page, you have hello option tooconfigure EM Cloud Control.</span></span> <span data-ttu-id="ebeb9-242">Tato možnost jsme přeskočili – klikněte na tlačítko `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-242">We are skipping this option - click `next` toocontinue.</span></span> 

7. <span data-ttu-id="ebeb9-243">Na hello **privilegovaných skupin operačního systému** stránky, použijte výchozí nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-243">On hello **Privileged Operating System Groups** page, use hello default settings.</span></span> <span data-ttu-id="ebeb9-244">Klikněte na tlačítko `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-244">Click `next` toocontinue.</span></span>

8. <span data-ttu-id="ebeb9-245">Na hello **zadejte umístění instalace** stránky, použijte výchozí nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-245">On hello **Specify Installation Location** page, use hello default settings.</span></span> <span data-ttu-id="ebeb9-246">Klikněte na tlačítko `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-246">Click `next` toocontinue.</span></span>

9. <span data-ttu-id="ebeb9-247">Na hello **vytvoření inventáře** změňte hello inventáře Directory příliš`/u01/app/grid/oraInventory`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-247">On hello **Create Inventory** page, change hello Inventory Directory too`/u01/app/grid/oraInventory`.</span></span> <span data-ttu-id="ebeb9-248">Klikněte na tlačítko `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-248">Click `next` toocontinue.</span></span>

   ![Snímek obrazovky stránky vytvořit inventáře instalační hello](./media/oracle-asm/install08.png)

10. <span data-ttu-id="ebeb9-250">Na hello **konfiguraci spuštění skriptu kořenové** stránky, vyberte hello **automaticky spouštět skripty konfigurace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-250">On hello **Root script execution configuration** page, select hello **Automatically run configuration scripts** check box.</span></span> <span data-ttu-id="ebeb9-251">Pak vyberte hello **použít přihlašovací údaje uživatele "root"** možnost a zadejte heslo uživatele root hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-251">Then, select hello **Use "root" user credential** option, and enter hello root user password.</span></span>

    ![Snímek obrazovky instalačního programu hello kořenové skriptu provádění konfiguraci stránky](./media/oracle-asm/install09.png)

11. <span data-ttu-id="ebeb9-253">Na hello **provedení požadovaných součástí kontroluje** stránce hello aktuální instalace se nezdaří s chybami.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-253">On hello **Perform Prerequisite Checks** page, hello current setup will fail with errors.</span></span> <span data-ttu-id="ebeb9-254">Toto je očekávané chování.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-254">This is an expected behavior.</span></span> <span data-ttu-id="ebeb9-255">Vyberte `Fix & Check Again`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-255">Select `Fix & Check Again`.</span></span>

12. <span data-ttu-id="ebeb9-256">V hello **oprava skriptu** dialogové okno, klikněte na tlačítko `OK`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-256">In hello **Fixup Script** dialog box, click `OK`.</span></span>

13. <span data-ttu-id="ebeb9-257">Na hello **Souhrn** stránka, zkontrolujte vybrané nastavení a pak klikněte na tlačítko `Install`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-257">On hello **Summary** page, review your selected settings, and then click `Install`.</span></span>

    ![Snímek obrazovky stránky Souhrn instalační hello](./media/oracle-asm/install12.png)

14. <span data-ttu-id="ebeb9-259">Zobrazí se upozornění dialogové okno oznamující konfigurační skripty, stačí spustit toobe jako privilegovaných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-259">A warning dialog box appears informing you configuration scripts need toobe run as a privileged user.</span></span> <span data-ttu-id="ebeb9-260">Klikněte na tlačítko `Yes` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-260">Click `Yes` toocontinue.</span></span>

15. <span data-ttu-id="ebeb9-261">Na hello **Dokončit** klikněte na tlačítko `Close` toofinish hello instalace.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-261">On hello **Finish** page, click `Close` toofinish hello installation.</span></span>

## <a name="set-up-your-oracle-asm-installation"></a><span data-ttu-id="ebeb9-262">Nastavení instalace Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="ebeb9-262">Set up your Oracle ASM installation</span></span>

<span data-ttu-id="ebeb9-263">tooset Oracle ASM Installation, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-263">tooset up your Oracle ASM installation, complete hello following steps:</span></span>

1. <span data-ttu-id="ebeb9-264">Ujistěte se ještě nejste přihlášení jako **mřížky**, z vaší X11 relace.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-264">Ensure you are still signed in as **grid**, from your X11 session.</span></span> <span data-ttu-id="ebeb9-265">Může být nutné toohit `enter` toorevive hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-265">You might need toohit `enter` toorevive hello terminal.</span></span> <span data-ttu-id="ebeb9-266">Spusťte hello Oracle automatizované úložiště správy konfigurace pomocníka:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-266">Then launch hello Oracle Automated Storage Management Configuration Assistant:</span></span>

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   <span data-ttu-id="ebeb9-267">Otevře se Oracle ASM konfigurace pomocníka.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-267">Oracle ASM Configuration Assistant opens.</span></span>

2. <span data-ttu-id="ebeb9-268">V hello **konfigurace ASM: disku skupiny** dialogové okno pole, klikněte na tlačítko hello `Create` tlačítko a pak klikněte na tlačítko `Show Advanced Options`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-268">In hello **Configure ASM: Disk Groups** dialog box, click hello `Create` button, and then click `Show Advanced Options`.</span></span>

3. <span data-ttu-id="ebeb9-269">V hello **vytvořit skupinu disku** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-269">In hello **Create Disk Group** dialog box:</span></span>

   - <span data-ttu-id="ebeb9-270">Zadejte název skupiny disku hello **DATA**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-270">Enter hello disk group name **DATA**.</span></span>
   - <span data-ttu-id="ebeb9-271">V části **vyberte disky člen**, vyberte **ORCL_DATA** a **ORCL_DATA1**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-271">Under **Select Member Disks**, select **ORCL_DATA** and **ORCL_DATA1**.</span></span>
   - <span data-ttu-id="ebeb9-272">V části **velikost alokační jednotky**, vyberte **4**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-272">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="ebeb9-273">Klikněte na tlačítko `ok` toocreate hello disku skupiny.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-273">Click `ok` toocreate hello disk group.</span></span>
   - <span data-ttu-id="ebeb9-274">Klikněte na tlačítko `ok` tooclose hello potvrzovacím okně.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-274">Click `ok` tooclose hello confirmation window.</span></span>

   ![Snímek obrazovky dialogového okna vytvořit skupinu disku hello](./media/oracle-asm/asm02.png)

4. <span data-ttu-id="ebeb9-276">V hello **konfigurace ASM: disku skupiny** dialogové okno pole, klikněte na tlačítko hello `Create` tlačítko a pak klikněte na tlačítko `Show Advanced Options`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-276">In hello **Configure ASM: Disk Groups** dialog box, click hello `Create` button, and then click `Show Advanced Options`.</span></span>

5. <span data-ttu-id="ebeb9-277">V hello **vytvořit skupinu disku** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-277">In hello **Create Disk Group** dialog box:</span></span>

   - <span data-ttu-id="ebeb9-278">Zadejte název skupiny disku hello **FRA**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-278">Enter hello disk group name **FRA**.</span></span>
   - <span data-ttu-id="ebeb9-279">V části **redundance**, vyberte **externí (none)**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-279">Under **Redundancy**, select **External (none)**.</span></span>
   - <span data-ttu-id="ebeb9-280">V části **vyberte disky člen**, vyberte **ORCL_FRA**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-280">Under **Select Member Disks**, select **ORCL_FRA**.</span></span>
   - <span data-ttu-id="ebeb9-281">V části **velikost alokační jednotky**, vyberte **4**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-281">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="ebeb9-282">Klikněte na tlačítko `ok` toocreate hello disku skupiny.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-282">Click `ok` toocreate hello disk group.</span></span>
   - <span data-ttu-id="ebeb9-283">Klikněte na tlačítko `ok` tooclose hello potvrzovacím okně.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-283">Click `ok` tooclose hello confirmation window.</span></span>

   ![Snímek obrazovky dialogového okna vytvořit skupinu disku hello](./media/oracle-asm/asm04.png)

6. <span data-ttu-id="ebeb9-285">Vyberte **ukončení** tooclose ASM konfigurace pomocníka.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-285">Select **Exit** tooclose ASM Configuration Assistant.</span></span>

   ![Snímek obrazovky hello nakonfigurovat ASM: disku skupiny dialogové okno s tlačítkem ukončení](./media/oracle-asm/asm05.png)

## <a name="create-hello-database"></a><span data-ttu-id="ebeb9-287">Vytvoření databáze hello</span><span class="sxs-lookup"><span data-stu-id="ebeb9-287">Create hello database</span></span>

<span data-ttu-id="ebeb9-288">Hello softwaru databáze Oracle je již nainstalován na bitovou kopii hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-288">hello Oracle database software is already installed on hello Azure Marketplace image.</span></span> <span data-ttu-id="ebeb9-289">toocreate databáze, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-289">toocreate a database, complete hello following steps:</span></span>

1. <span data-ttu-id="ebeb9-290">Přepnout uživatele toohello Oracle superuživatel a potom inicializujte hello naslouchací proces pro protokolování:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-290">Switch users toohello Oracle superuser, and then initialize hello listener for logging:</span></span>

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   <span data-ttu-id="ebeb9-291">Otevře se Pomocník pro konfiguraci databáze.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-291">Database Configuration Assistant opens.</span></span>

2. <span data-ttu-id="ebeb9-292">Na hello **operace databáze** klikněte na tlačítko `Create Database`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-292">On hello **Database Operation** page, click `Create Database`.</span></span>

3. <span data-ttu-id="ebeb9-293">Na hello **režimu vytváření** stránky:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-293">On hello **Creation Mode** page:</span></span>

   - <span data-ttu-id="ebeb9-294">Zadejte název databáze hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-294">Enter a name for hello database.</span></span>
   - <span data-ttu-id="ebeb9-295">Pro **typ úložiště**, ujistěte se, **automatické úložiště Management (ASM)** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-295">For **Storage Type**, ensure **Automatic Storage Management (ASM)** is selected.</span></span>
   - <span data-ttu-id="ebeb9-296">Pro **umístění souborů databáze**, použít výchozí hello ASM navrhované umístění.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-296">For **Database Files Location**, use hello default ASM suggested location.</span></span>
   - <span data-ttu-id="ebeb9-297">Pro **rychlého obnovení oblasti**, použít výchozí hello ASM navrhované umístění.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-297">For **Fast Recovery Area**, use hello default ASM suggested location.</span></span>
   - <span data-ttu-id="ebeb9-298">Zadejte **hesla pro správu** a **potvrzení hesla**.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-298">type in an **Administrative Password** and **confirm password**.</span></span>
   - <span data-ttu-id="ebeb9-299">Ujistěte se, `create as container database` je vybrána.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-299">ensure `create as container database` is selected.</span></span>
   - <span data-ttu-id="ebeb9-300">Zadejte `pluggable database name` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-300">type in a `pluggable database name` value.</span></span>

4. <span data-ttu-id="ebeb9-301">Na hello **Souhrn** stránka, zkontrolujte vybrané nastavení a pak klikněte na tlačítko `Finish` toocreate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-301">On hello **Summary** page, review your selected settings, and then click `Finish` toocreate hello database.</span></span>

   ![Snímek obrazovky stránky Souhrn hello](./media/oracle-asm/createdb03.png)

5. <span data-ttu-id="ebeb9-303">Hello databáze byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-303">hello Database has been created.</span></span> <span data-ttu-id="ebeb9-304">Na hello **Dokončit** stránky, máte možnost toounlock další účty toouse tuto databázi a změnit hesla hello hello.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-304">On hello **Finish** page, you have hello option toounlock additional accounts toouse this database and change hello passwords.</span></span> <span data-ttu-id="ebeb9-305">Pokud toodo chcete udělat, vyberte **Správa hesel** -jinak klikněte na `close`.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-305">If you wish toodo so, select **Password Management** - otherwise click on `close`.</span></span>

## <a name="delete-hello-vm"></a><span data-ttu-id="ebeb9-306">Odstranit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ebeb9-306">Delete hello VM</span></span>

<span data-ttu-id="ebeb9-307">Úspěšně jste nakonfigurovali Oracle automatizovat správu úložiště na bitovou kopii databáze Oracle hello z hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ebeb9-307">You have successfully configured Oracle Automated Storage Management on hello Oracle DB image from hello Azure Marketplace.</span></span>  <span data-ttu-id="ebeb9-308">Pokud již nepotřebujete tento virtuální počítač, můžete použít následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello:</span><span class="sxs-lookup"><span data-stu-id="ebeb9-308">When you no longer need this VM, you can use hello following command tooremove hello resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="ebeb9-309">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebeb9-309">Next steps</span></span>

[<span data-ttu-id="ebeb9-310">Kurz: Konfigurace Oracle DataGuard</span><span class="sxs-lookup"><span data-stu-id="ebeb9-310">Tutorial: Configure Oracle DataGuard</span></span>](configure-oracle-dataguard.md)

[<span data-ttu-id="ebeb9-311">Kurz: Konfigurace Oracle GoldenGate</span><span class="sxs-lookup"><span data-stu-id="ebeb9-311">Tutorial: Configure Oracle GoldenGate</span></span>](Configure-oracle-golden-gate.md)

<span data-ttu-id="ebeb9-312">Zkontrolujte [architektury Oracle DB](oracle-design.md)</span><span class="sxs-lookup"><span data-stu-id="ebeb9-312">Review [Architect an Oracle DB](oracle-design.md)</span></span>
