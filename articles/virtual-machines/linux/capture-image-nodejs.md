---
title: "aaaCapture virtuální počítač Azure s Linuxem toouse jako šablona | Microsoft Docs"
description: "Zjistěte, jak toocapture a generalize bitové kopie založené na systému Linux Azure virtuálního počítače (VM) vytvořené pomocí modelu nasazení Azure Resource Manager hello."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 877eee5c842bebe80e755c2240cdaaef4ade6ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Zachytit virtuální počítač Linux spuštěné v Azure
Postupujte podle kroků hello v toogeneralize Tento článek a zachycení Azure Linux virtuálního počítače (VM) v modelu nasazení Resource Manager hello. Při průchodu generalize hello virtuálních počítačů, můžete odebrat informace o osobní účet a příprava toobe hello virtuálních počítačů použít jako obrázek. Můžete potom zachycení bitové kopie zobecněný virtuální pevný disk (VHD) pro hello operačního systému, virtuální pevné disky pro připojené datových disků, a [šablony Resource Manageru](../../azure-resource-manager/resource-group-overview.md) pro nová nasazení virtuálních počítačů. Tento článek popisuje, jak toocapture virtuálního počítače bitové kopie s hello 1.0 rozhraní příkazového řádku Azure pro virtuální počítač pomocí nespravované disků. Můžete také [zachycení virtuálního počítače pomocí Azure spravované disky s hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Spravované disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je. Další informace najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

toocreate virtuální počítače pomocí bitové kopie hello, nastavit síťovým prostředkům pro každý nový virtuální počítač a použijte hello šablony (soubor JavaScript Object Notation nebo formát JSON,) toodeploy z hello zachycení bitové kopie virtuálního pevného disku. Tímto způsobem můžete replikovat virtuální počítač s své aktuální konfiguraci softwaru, podobně jako toohello způsob, jak používat Image v hello Azure Marketplace.

> [!TIP]
> Pokud chcete toocreate kopii existující virtuální počítač Linux s jejím specializované stavu pro zálohování nebo ladění, najdete v části [vytvoření kopie virtuálního počítače Linux spuštěné v Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). A pokud chcete tooupload Linux virtuální pevný disk z virtuálního počítače místní, najdete v části [odesílání a vytvoření virtuálního počítače s Linuxem z bitové kopie disku vlastní](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#before-you-begin) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="before-you-begin"></a>Než začnete
Ujistěte se, že splňujete hello následující požadavky:

* **Virtuální počítač Azure vytvořené v modelu nasazení Resource Manager hello** – Pokud jste nevytvořili virtuální počítač s Linuxem, můžete použít hello [portál](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [rozhraní příkazového řádku Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), nebo [Resource Manager šablony](create-ssh-secured-vm-from-template.md). 
  
    Nakonfigurujte hello virtuální počítač podle potřeby. Například [přidat datových disků](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), aktualizace a instalovat aplikace. 
* **Rozhraní příkazového řádku Azure** -instalace hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) v místním počítači.

## <a name="step-1-remove-hello-azure-linux-agent"></a>Krok 1: Odebrání hello Azure Linux agent
Nejprve spustit hello **příkaz waagent** s hello **deprovision** parametr na hello virtuálního počítače s Linuxem. Tento příkaz odstraní všechny soubory a data toomake hello připravené pro generalizací virtuálních počítačů. Podrobnosti najdete v tématu hello [Azure Linux Agent uživatelská příručka](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Připojte tooyour virtuálního počítače s Linuxem pomocí klienta SSH.
2. V okně hello SSH zadejte následující příkaz hello:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Spusťte pouze na virtuálním počítači tento příkaz, že máte v úmyslu toocapture jako obrázek. Nezaručuje se této bitové kopie hello vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci.
 
3. Typ **y** toocontinue. Můžete přidat hello **-force** parametr tooavoid tento krok potvrzení.
4. Po dokončení příkazu hello zadejte **ukončete**. Tento krok zavře klient SSH hello.

## <a name="step-2-capture-hello-vm"></a>Krok 2: Zachycení hello virtuálních počítačů
Pomocí rozhraní příkazového řádku Azure toogeneralize hello a zachycení hello virtuálních počítačů. Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad **myResourceGroup**, **myVnet**, a **Můjvp**.

1. Z místního počítače, otevřete hello rozhraní příkazového řádku Azure a [přihlášení tooyour předplatné](../../xplat-cli-connect.md). 
2. Ujistěte se, že jste v režimu Resource Manager.
   
    ```azurecli
    azure config mode arm
    ```
3. Vypněte hello virtuální počítač, který již zrušit pomocí hello následující příkaz:
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. Generalize hello virtuálních počítačů s hello následující příkaz:
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. Teď spustit hello **zachycení virtuálního počítače azure** příkaz, který zachytává hello virtuálních počítačů. V následujícím příkladu hello, bitové kopie hello virtuální pevné disky jsou zachyceny s názvy počínaje **MyVHDNamePrefix**a hello **-t** možnost určuje šablonu cesty toohello **MyTemplate.json**. 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > soubory virtuálního pevného disku image Hello vytvářeny ve výchozím nastavení v hello používá stejný účet úložiště, který hello původní virtuální počítač. Použití hello *stejný účet úložiště* toostore hello virtuální pevné disky pro všechny nové virtuální počítače, můžete vytvořit z hello image. 

6. umístění hello toofind zaznamenané bitové kopie, otevřete hello šablony JSON v textovém editoru. V hello **storageProfile**, najde hello **uri** z hello **image** umístěný v hello **systému** kontejneru. Například hello URI image disku hello operačního systému je podobný příliš`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-hello-captured-image"></a>Krok 3: Vytvoření virtuálního počítače z hello zachycení image
Teď použijte bitovou kopii hello s toocreate šablony virtuálního počítače s Linuxem. Tyto kroky ukazují, jak toouse hello rozhraní příkazového řádku Azure a hello šablona souboru JSON zachycené toocreate hello virtuálních počítačů v nové virtuální sítě.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
toouse hello šablony, je nutné nejprve tooset virtuální sítě a síťovou kartu pro nový virtuální počítač. Doporučujeme že vytvořit skupinu prostředků pro tyto prostředky v hello umístění, kde jsou uložené vaše image virtuálního počítače. Spusťte příkazy podobné toohello následující, nahraďte názvy pro vaše prostředky a příslušné umístění Azure ("centralus" v těchto příkazech):

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-hello-id-of-hello-nic"></a>Získat hello Id hello síťový adaptér
virtuální počítač z bitové kopie hello pomocí hello JSON, které jste uložili během zachycení toodeploy, budete potřebovat hello Id hello síťový adaptér. Ho můžete získejte spuštěním hello následující příkaz:

```azurecli
azure network nic show myResourceGroup1 myNIC
```

Hello **Id** v hello výstup se podobá příliš`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`

### <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Teď hello spusťte následující příkaz toocreate virtuálního počítače z hello zachycenou image virtuálního počítače. Použití hello **-f** parametr toospecify hello cesta JSON toohello šablony soubor uložit.

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

Ve výstupu příkazu hello jsou výzvami toosupply nový název virtuálního počítače, hello správce uživatelské jméno a heslo a hello Id hello seskupování, které jste předtím vytvořili.

```bash
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

Hello následující ukázka uvádí, co vidíte pro úspěšné nasazení:

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment toocomplete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-hello-deployment"></a>Ověření nasazení hello
Nyní SSH toohello virtuálního počítače jste vytvořili tooverify hello nasazení a spuštění pomocí hello nového virtuálního počítače. tooconnect pomocí protokolu SSH, najít IP adresu hello hello virtuálních počítačů, které jste vytvořili spuštěním hello následující příkaz:

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

Hello veřejná IP adresa je uvedena ve výstupu příkazu hello. Ve výchozím nastavení je připojit toohello virtuálního počítače s Linuxem pomocí SSH na port 22.

## <a name="create-additional-vms"></a>Vytvoření dalších virtuálních počítačů
Použití hello zachycení bitové kopie a šablony toodeploy dalších virtuálních počítačů pomocí kroků hello v předcházející části hello. Další možnosti toocreate virtuální počítače z hello image zahrnout pomocí šabloně pro rychlý start nebo systémem hello **vytvoření virtuálního počítače azure** příkaz.

### <a name="use-hello-captured-template"></a>Použít šablonu zaznamenané hello
toouse hello zachycení bitové kopie a šablony, postupujte podle těchto kroků (podrobně v předcházející části hello):

* Zajistěte, aby byl váš image virtuálního počítače v hello stejný účet úložiště, který je hostitelem virtuálního pevného disku Virtuálního počítače.
* Zkopírujte soubor JSON hello šablonu a zadejte jedinečný název pro disk s operačním systémem hello hello nového Virtuálního počítače virtuální pevný disk (nebo virtuální pevné disky). Například v hello **storageProfile**v části **virtuálního pevného disku**v **uri**, zadejte jedinečný název pro hello **osDisk** virtuálního pevného disku, podobně jako příliš`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Vytvořte síťovou kartu v buď hello stejný nebo jiný virtuální sítě.
* Ve skupině prostředků hello, ve kterém můžete nastavit hello virtuální sítě pomocí souboru JSON šablony hello změnit, vytvořte nasazení.

### <a name="use-a-quickstart-template"></a>Použít šabloně pro rychlý start
Pokud chcete nastavit automaticky při vytváření virtuálního počítače z hello image sítě hello, můžete tyto prostředky v šabloně. Například v tématu hello [101-vm z – uživatele – image šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) z Githubu. Tato šablona vytvoří virtuální počítač pomocí vlastní image a hello potřebná virtuální sítě, veřejnou IP adresu a Síťových prostředků. Návod k používání hello šablony v hello portálu Azure, najdete v části [jak toocreate virtuálního počítače z vlastní image pomocí šablony Resource Manageru](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-hello-azure-vm-create-command"></a>Vytvoření virtuálního počítače hello azure použití příkazu
Obvykle je nejjednodušší toouse toocreate správce prostředků šablony virtuálního počítače z hello image. Ale můžete vytvořit hello virtuálních počítačů *imperativní* pomocí hello **vytvoření virtuálního počítače azure** s hello **-Q** (**– image urn**) parametr . Pokud použijete tuto metodu, předáte hello **-d** (**vhd disk operačního systému –**) parametr toospecify hello umístění souboru VHD hello operačního systému pro hello nového virtuálního počítače. Tento soubor musí být v kontejneru virtuální pevné disky hello účtu úložiště hello, kde je uložený soubor VHD hello bitové kopie. příkaz kopie hello virtuálního pevného disku pro hello Hello nový virtuální počítač automaticky toohello **virtuální pevné disky** kontejneru.

Dřív, než spustíte **vytvoření virtuálního počítače azure** hello Image, dokončete hello následující kroky:

1. Vytvořte skupinu prostředků nebo identifikovat existující skupinu prostředků pro nasazení hello.
2. Vytvoření veřejné IP adresu prostředku a Síťových prostředků pro hello nového virtuálního počítače. Pro kroky toocreate virtuální sítě, veřejnou IP adresu a síťový adaptér pomocí hello rozhraní příkazového řádku, viz výše v tomto článku. (**vytvoření virtuálního počítače azure** můžete také vytvořit síťovou kartu, ale potřebujete toopass další parametry pro virtuální síť a podsíť.)

Spusťte příkaz, který předává identifikátory URI tooboth hello nového souboru virtuálního pevného disku operačního systému a hello existující bitovou kopii. V tomto příkladu je vytvořen s velikostí virtuálních počítačů Standard_A1 v oblasti Východ USA hello.

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

Další příkaz Možnosti spustit `azure help vm create`.

## <a name="next-steps"></a>Další kroky
toomanage virtuální počítače s hello CLI, najdete v části úlohy hello v [nasadit a spravovat virtuální počítače pomocí šablony Azure Resource Manager a hello rozhraní příkazového řádku Azure](create-ssh-secured-vm-from-template.md).

