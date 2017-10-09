---
title: "aaaCreate virtuální sítě Azure s několika podsítěmi | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální síť s více podsítěmi v Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 0f56fa6ac24537d33b8e217f5b03f387826ab487
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Vytvoření virtuální sítě s několika podsítěmi

V tomto kurzu zjistěte, jak toocreate základní virtuální síť Azure, která má oddělit veřejné a privátní podsítě. Prostředky Azure, jako jsou virtuální počítače, služby App Service Environment, sady škálování virtuálního počítače, Azure HDInsight a cloudové služby můžete vytvořit v podsíti. Prostředky ve virtuálních sítích můžete komunikaci mezi sebou a s prostředky v jiných sítích připojených tooa virtuální sítě.

Hello následující části obsahují kroky, které můžete využít toocreate virtuální sítě pomocí hello [portál Azure](#portal), hello rozhraní příkazového řádku Azure ([rozhraní příkazového řádku Azure](#azure-cli)), [prostředí Azure PowerShell ](#powershell)a [šablony Azure Resource Manageru](#resource-manager-template). Výsledkem Hello je hello stejný bez ohledu na to, který nástroj pomocí toocreate hello virtuální sítě. Klikněte na odkaz nástroj toogo toothat část kurzu hello. Další informace o všech [virtuální sítě](virtual-network-manage-network.md) a [podsíť](virtual-network-manage-subnet.md) nastavení.

Tento článek obsahuje kroky toocreate virtuální sítě pomocí modelu nasazení Resource Manager hello, což je hello model nasazení, které vám doporučujeme používat při vytváření nové virtuální sítě. Pokud potřebujete toocreate virtuální sítě (klasické), přečtěte si [vytvoření virtuální sítě (klasické)](create-virtual-network-classic.md). Pokud si nejste obeznámeni s modelech nasazení Azure, najdete v části [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Portál Azure

1. V internetovém prohlížeči, přejděte toohello [portál Azure](https://portal.azure.com). Přihlaste se pomocí vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud nemáte účet Azure, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello portálu, klikněte na tlačítko **+ nový** > **sítě** > **virtuální síť**.
3. Na hello **vytvořit virtuální síť** okno, zadejte následující hodnoty hello a pak klikněte na **vytvořit**:

    |Nastavení|Hodnota|
    |---|---|
    |Name (Název)|myVnet|
    |Adresní prostor|10.0.0.0/16|
    |Název podsítě|Veřejné|
    |Rozsah adres podsítě|10.0.0.0/24|
    |Skupina prostředků|Nechte **vytvořit nový** vybrané a potom zadejte **myResourceGroup**.|
    |Předplatné a umístění|Vyberte vaše předplatné a umístění.

    Pokud jste nový tooAzure, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (nazývaná také jen tooas *oblasti*).
4. Hello portálu můžete vytvořit pouze jednu podsíť při vytváření virtuální sítě. V tomto kurzu vytvoříte druhou podsíť po vytvoření virtuální sítě hello. Můžete vytvořit prostředky přístupné z Internetu později v hello **veřejné** podsítě. Můžete také vytvořit prostředky, které nejsou přístupné z Internetu hello v hello **privátní** podsítě. podsíť, druhý toocreate hello v hello **vyhledávání prostředků** pole v horní části hello hello stránky, zadejte **myVnet**. Ve výsledcích hledání hello, klikněte na tlačítko **myVnet**. Pokud máte více virtuálních sítí s hello stejný název v rámci vašeho předplatného, zkontrolujte hello skupin prostředků, které jsou obsaženy v každé virtuální sítě. Ujistěte se, klikněte na tlačítko hello **myVnet** hledání výsledek, který má skupinu prostředků hello **myResourceGroup**.
5. Na hello **myVnet** okno, v části **nastavení**, klikněte na tlačítko **podsítě**.
6. Na hello **myVnet - podsítě** okně klikněte na tlačítko **+ podsítě**.
7. Na hello **přidat podsíť** okně pro **název**, zadejte **privátní**. Pro **rozsahu adres**, zadejte **10.0.1.0/24**.  Klikněte na **OK**.
8. Na hello **myVnet - podsítě** okno, zkontrolujte hello podsítě. Můžete zobrazit hello **veřejné** a **privátní** podsítě, které jste vytvořili.
9. **Volitelné:** toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-portal) v tomto článku.

## <a name="azure-cli"></a>Azure CLI

Rozhraní příkazového řádku Azure jsou hello stejné, zda spuštěním příkazů hello ze systému Windows, Linux nebo systému macOS. Existují však skriptování rozdíly mezi součásti pro operační systém. Hello skript v hello následující kroky se spustí v prostředí Bash. 

1. [Instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Hello cloudové prostředí má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello prostředí cloudu (**> _**) tlačítko hello horní části hello [portál](https://portal.azure.com) nebo stačí kliknout na hello *vyzkoušet* tlačítko hello kroky, které následují. 
2. Pokud je spuštěn místně hello rozhraní příkazového řádku, přihlaste se tooAzure s hello `az login` příkaz. Pokud používáte hello cloudové prostředí, jste již přihlášeni.
3. Zkontrolujte hello následující skript a její komentáře. V prohlížeči zkopírujte hello skript a vložte jej do relace rozhraní příkazového řádku:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in hello virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Po dokončení skriptu hello spuštěna, zkontrolujte hello podsítě pro virtuální síť hello. Zkopírujte hello následující příkaz a pak ji vložit do relace rozhraní příkazového řádku:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-cli) v tomto článku.

## <a name="powershell"></a>PowerShell

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. V relaci prostředí PowerShell přihlásit tooAzure s vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) pomocí hello `login-azurermaccount` příkaz.

3. Zkontrolujte hello následující skript a její komentáře. V prohlížeči zkopírujte hello skript a vložte ho do relace prostředí PowerShell:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create hello public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. tooreview hello podsítě pro virtuální síť hello, zkopírujte hello následující příkaz a pak ji vložit do relace prostředí PowerShell:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-powershell) v tomto článku.

## <a name="resource-manager-template"></a>Šablona Resource Manageru

Virtuální síť můžete nasadit pomocí šablony Azure Resource Manager. toolearn Další informace o šablony, najdete v části [co je správce prostředků](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Šablona hello tooaccess a toolearn o jeho parametrech najdete v části hello [vytvořit virtuální síť se dvěma podsítěmi](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) šablony. Hello šablony můžete nasadit pomocí hello [portál](#template-portal), [rozhraní příkazového řádku Azure](#template-cli), nebo [prostředí PowerShell](#template-powershell).

**Volitelné:** toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky v jakékoli pododdílu [odstranit prostředky](#delete) v tomto článku.

### <a name="template-portal"></a>Portál Azure

1. V prohlížeči otevřete hello [stránku šablony](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klikněte na tlačítko hello **nasazení tooAzure** tlačítko. Pokud jste již přihlášeni tooAzure, přihlaste se na úvodní obrazovka přihlášení k portálu Azure, který se zobrazí.
3. Přihlaste se pomocí portálu toohello vaše [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud nemáte účet Azure, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Zadejte následující hodnoty parametrů hello hello:

    |Parametr|Hodnota|
    |---|---|
    |Předplatné|Vyberte předplatné|
    |Skupina prostředků|myResourceGroup|
    |Umístění|Vyberte umístění|
    |Název virtuální sítě|myVnet|
    |Předpona adresy virtuální sítě|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Veřejné|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Privátní|

5. Souhlasím toohello podmínky a ujednání a pak klikněte na **nákupu** toodeploy hello virtuální sítě.

### <a name="template-cli"></a>Rozhraní příkazového řádku Azure

1. [Instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Hello cloudové prostředí má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello cloudové prostředí **> _** tlačítko hello horní části hello [portál](https://portal.azure.com), nebo stačí kliknout na hello **vyzkoušet** tlačítko hello kroky, které následují. 
2. Pokud je spuštěn místně hello rozhraní příkazového řádku, přihlaste se tooAzure s hello `az login` příkaz. Pokud používáte hello cloudové prostředí, jste již přihlášeni.
3. toocreate skupinu prostředků pro virtuální síť hello kopie hello následující příkaz a vložte jej do relace rozhraní příkazového řádku:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Hello šablony můžete nasadit pomocí jedné z hello následující možnosti Parametry:
    - **Výchozí hodnoty parametrů**. Zadejte hello následující příkaz:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Hodnoty parametru vlastní**. Stáhnout a upravit šablonu hello před nasazením hello šablony. Také můžete nasadit šablonu hello pomocí parametrů příkazového řádku hello nebo nasazení šablony hello souborem samostatné parametry. šablony a parametry soubory hello toodownload klikněte na tlačítko hello **Procházet na Githubu** na hello tlačítko [vytvořit virtuální síť se dvěma podsítěmi](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) stránku šablony. V Githubu, klikněte na tlačítko hello **azuredeploy.parameters.json** nebo **azuredeploy.json** souboru. Potom klikněte na hello **Raw** tlačítko toodisplay hello souboru. V prohlížeči zkopírujte hello obsah souboru hello. Uložte soubor tooa hello obsah ve vašem počítači. Můžete upravit hello hodnoty parametrů v šabloně hello nebo nasazení šablony hello souborem samostatné parametry.  

    Další informace o toolearn toodeploy šablon pomocí těchto metod, jak zadejte `az group deployment create --help`.

### <a name="template-powershell"></a>Prostředí PowerShell

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. V relaci prostředí PowerShell toosign s vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), zadejte `login-azurermaccount`.
3. toocreate skupinu prostředků pro hello virtuální síť, zadejte následující příkaz hello:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Hello šablony můžete nasadit pomocí jedné z hello následující možnosti Parametry:
    - **Výchozí hodnoty parametrů**. Zadejte hello následující příkaz:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Hodnoty parametru vlastní**. Stáhnout a upravit šablonu hello před nasazením. Také můžete nasadit šablonu hello pomocí parametrů příkazového řádku hello nebo nasazení šablony hello souborem samostatné parametry. šablony a parametry soubory hello toodownload klikněte na tlačítko hello **Procházet na Githubu** na hello tlačítko [vytvořit virtuální síť se dvěma podsítěmi](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) stránku šablony. V Githubu, klikněte na tlačítko hello **azuredeploy.parameters.json** nebo **azuredeploy.json** souboru. Potom klikněte na hello **Raw** tlačítko toodisplay hello souboru. V prohlížeči zkopírujte hello obsah souboru hello. Uložte soubor tooa hello obsah ve vašem počítači. Můžete upravit hello hodnoty parametrů v šabloně hello nebo nasazení šablony hello souborem samostatné parametry.  

    Další informace o toolearn toodeploy šablon pomocí těchto metod, jak zadejte `Get-Help New-AzureRmResourceGroupDeployment`. 

## <a name="delete"></a>Odstraňte prostředky

Po dokončení tohoto kurzu toodelete hello prostředky, které jste vytvořili, můžete chtít, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků hello.

### <a name="delete-portal"></a>Portál Azure

1. Hello portálu vyhledávacího pole zadejte **myResourceGroup**. Ve výsledcích hledání hello, klikněte na tlačítko **myResourceGroup**.
2. Na hello **myResourceGroup** okně klikněte na tlačítko hello **odstranit** ikonu.
3. odstranění hello tooconfirm, v hello **typ hello název skupiny prostředků** zadejte **myResourceGroup**a potom klikněte na **odstranit**.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

V relaci příkazového řádku zadejte následující příkaz hello:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>Prostředí PowerShell

V relaci prostředí PowerShell zadejte následující příkaz hello:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Další kroky

- toolearn o všechny virtuální sítě a podsítě nastavení, najdete v části [spravovat virtuální sítě](virtual-network-manage-network.md#view-vnet) a [spravovat podsítě virtuální sítě](virtual-network-manage-subnet.md#create-subnet). Máte různé možnosti pro použití virtuálními sítěmi a podsítěmi v jiné požadavky produkční prostředí toomeet.
- toofilter příchozí a odchozí přenosy podsítěmi, vytvoření a použití [skupin zabezpečení sítě](virtual-networks-nsg.md) toosubnets.
- Vytvoření [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuálního počítače a připojte ho tooan existující virtuální síť.
- tooconnect dvou virtuálních sítí v hello stejného umístění Azure, vytvořte [partnerský vztah virtuální sítě](virtual-network-peering-overview.md) mezi virtuálními sítěmi hello.
- Připojit pomocí hello virtuální sítě tooan do místní sítě [brány VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) okruh.
