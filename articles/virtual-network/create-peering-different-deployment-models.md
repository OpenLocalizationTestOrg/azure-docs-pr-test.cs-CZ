---
title: "aaaCreate Azure virtuální sítě partnerského vztahu - různé modely nasazení - stejného předplatného. | Microsoft Docs"
description: "Zjistěte, jak virtuální sítě vztahy mezi virtuálními sítěmi toocreate vytvořena prostřednictvím různých nasazení Azure modely, které existují v hello stejné předplatné Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: 365156d651c9042ed52baeb15bf629fcc5329af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Vytvořit virtuální síť partnerský vztah - různé modely nasazení, stejného předplatného. 

V tomto kurzu zjistíte toocreate virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí různé modely nasazení. Obě virtuální sítě existuje v hello stejné předplatné. Partnerského vztahu dva virtuální sítě umožňuje prostředky v různých virtuálních sítích toocommunicate s jinými s hello stejné šířky pásma a latence, jako by byl hello prostředky v hello stejné virtuální síti. Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md). 

Hello kroky toocreate virtuální sítě partnerský vztah se liší, v závislosti na tom, zda text hello virtuální sítě jsou v hello stejný nebo jiný, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuální sítě se vytvářejí prostřednictvím. Zjistěte, jak toocreate a virtuální sítě, partnerský vztah v jiných scénářích kliknutím hello scénář z hello následující tabulka:

|Model nasazení Azure  | Předplatné Azure  |
|--------- |---------|
|[I Resource Manager](virtual-network-create-peering.md) |stejné|
|[I Resource Manager](create-peering-different-subscriptions.md) |Různé|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models-subscriptions.md) |Různé|

Mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic hello nelze vytvořit virtuální síť partnerský vztah. Partnerský vztah virtuální sítě mohou být vytvořeny pouze mezi dvěma virtuálními sítěmi, které existují v hello stejné oblasti Azure. Pokud potřebujete tooconnect virtuální sítě, které byly obě vytvořené pomocí modelu nasazení classic hello nebo které existovat v různých oblastech Azure, můžete použít Azure [brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuální sítě. 

Můžete použít hello [portál Azure](#portal), hello Azure [rozhraní příkazového řádku](#cli) (CLI), nebo Azure [prostředí PowerShell](#powershell) toocreate partnerský vztah virtuální sítě. Přímo toohello kroky pro vytvoření virtuální sítě partnerský vztah pomocí vaší nástroje, klepněte na libovolný hello předchozí nástroj odkazy toogo.

## <a name="cli"></a>Vytvoření partnerského vztahu - portálu

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
2. Klikněte na tlačítko **+ nový**, klikněte na tlačítko **sítě**, pak klikněte na tlačítko **virtuální síť**.
3. V hello **vytvořit virtuální síť** okno, zadejte, nebo vyberte hodnoty pro hello následující nastavení a potom klikněte na tlačítko **vytvořit**:
    - **Název**: *myVnet1*
    - **Adresní prostor**: *10.0.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.0.0.0/24*
    - **Předplatné**: Vyberte předplatné
    - **Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroup*
    - **Umístění**: *východní USA*
4. Klikněte na tlačítko **+ nový**. V hello **vyhledávání hello Marketplace** zadejte *virtuální síť*. Klikněte na tlačítko **virtuální síť** při zobrazí ve výsledcích hledání hello. 
5. V hello **virtuální síť** vyberte **Classic** v hello **vybrat model nasazení** pole a pak klikněte na **vytvořit**.
6. V hello **vytvořit virtuální síť** okno, zadejte, nebo vyberte hodnoty pro hello následující nastavení a potom klikněte na tlačítko **vytvořit**:
    - **Název**: *myVnet2*
    - **Adresní prostor**: *10.1.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.1.0.0/24*
    - **Předplatné**: Vyberte předplatné
    - **Skupina prostředků**: vyberte **použít existující** a vyberte *myResourceGroup*
    - **Umístění**: *východní USA*
7. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myResourceGroup*. Klikněte na tlačítko **myResourceGroup** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myresourcegroup** skupinu prostředků. Skupina prostředků Hello obsahuje hello dvě virtuální sítě vytvořené v předchozích krocích.
8. Klikněte na tlačítko **myVNet1**.
9. V hello **myVnet1** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** z hello svislé seznam možností na levé straně okna hello hello.
10. V hello **myVnet1 - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**
11. V hello **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte hello následující možnosti a potom klikněte na tlačítko **OK**:
     - **Název**: *myVnet1ToMyVnet2*
     - **Virtuální síť modelu nasazení**: vyberte **Classic**. 
     - **Předplatné**: Vyberte předplatné
     - **Virtuální síť**: klikněte na tlačítko **vyberte virtuální síť**, pak klikněte na tlačítko **myVnet2**.
     - **Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.
    Žádné další nastavení použitá v tomto kurzu. Přečtěte si toolearn o všech nastaveních partnerského vztahu, [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).
12. Po kliknutí na **OK** v předchozím kroku hello hello **partnerský vztah přidat** okno se zavře a zobrazí hello **myVnet1 - partnerských vztahů** okno znovu. Za několik sekund zobrazí se hello partnerský vztah, kterou jste vytvořili v okně hello. **Připojení** je uvedena v hello **stav partnerského vztahu** sloupec pro hello **myVnet1ToMyVnet2** partnerského vztahu, můžete vytvořit.

    partnerský vztah Hello je nyní vytvořeno. Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
13. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
14. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete-portal) tohoto článku.

## <a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure

1. [Nainstalujte](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello Azure CLI 1.0 toocreate hello virtuální sítě (klasické).
2. Otevřete relaci příkazového řádku a přihlaste se pomocí hello tooAzure `azure login` příkaz.
3. Spustit hello rozhraní příkazového řádku v režimu správy služby tak, že zadáte hello `azure config mode asm` příkaz.
4. Zadejte následující příkaz toocreate hello virtuální sítě (klasické) hello:
 
    ```azurecli
    azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

5. Vytvořte skupinu prostředků a virtuální síť (Resource Manager). Můžete použít hello rozhraní příkazového řádku 1.0 nebo 2.0 ([nainstalovat](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)). V tomto kurzu hello 2.0 rozhraní příkazového řádku je použité toocreate hello virtuální sítě (Resource Manager), protože 2.0 musí být partnerský vztah hello použité toocreate. Spusťte hello následující bash skript rozhraní příkazového řádku z místního počítače s hello CLI verze 2.0.4 nainstalovaný nebo novější. Možnosti na spuštění bash skripty rozhraní příkazového řádku na klientovi Windows, najdete v části [běžící ve Windows hello rozhraní příkazového řádku Azure](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Můžete také spustit skript hello pomocí hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. Klikněte na tlačítko hello **vyzkoušet** tlačítka na hello skript, který způsobem, které vyvolá prostředí cloudu, který vám umožní přihlásit se tooyour účet Azure s. tooexecute hello skriptu, klikněte na tlačítko hello **kopie** tlačítko a vložení, hello obsah do své cloudové prostředí, stiskněte `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus

    # Create hello virtual network (Resource Manager).
    az network vnet create \
      --name myVnet1 \
      --resource-group myResourceGroup \
      --location eastus \
      --address-prefix 10.0.0.0/16
    ```

6. Vytvoření virtuální sítě partnerský vztah mezi hello dvě virtuální sítě vytvořené pomocí hello různé modely nasazení. Zkopírujte následující skript tooa textový editor ve vašem počítači hello. Nahraďte `<subscription id>` s ID vašeho předplatného. Pokud si nejste jisti Id předplatného, zadejte hello `az account show` příkaz. Hello hodnotu **id** v hello výstupem je ID vašeho předplatného. Vložte skript hello upravit v relaci tooyour rozhraní příkazového řádku a stiskněte klávesu `Enter`.

    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group myResourceGroup \
      --name myVnet1 \
      --query id --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
      --allow-vnet-access
    ```
7. Po provedení hello skriptu, zkontrolujte hello partnerský vztah pro virtuální síť hello (Resource Manager). Kopírování hello následující příkaz, vložte jej v relaci příkazového řádku a stiskněte klávesu `Enter`:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    výstup ukazuje Hello **připojeno** v hello **PeeringState** sloupce. 

    Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
8. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
9. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-cli) v tomto článku.

## <a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [Azure](https://www.powershellgallery.com/packages/Azure) a [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) moduly. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Spusťte relaci prostředí PowerShell.
3. V prostředí PowerShell přihlásit tooAzure zadáním hello `Add-AzureAccount` příkaz.
4. toocreate virtuální sítě (klasické) v prostředí PowerShell, musíte vytvořit nový, nebo upravte stávající sítě konfigurační soubor. Zjistěte, jak příliš[exportovat, aktualizovat a import konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md). Hello soubor musí zahrnovat následující hello **VirtualNetworkSite** element pro virtuální síť hello použili v tomto kurzu:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Import konfiguračního souboru změněné sítě může způsobit změny tooexisting virtuální sítě (klasické) v rámci vašeho předplatného. Ujistěte se, můžete přidat pouze hello předchozí virtuální sítě a změníte nebo odeberte všechny existující virtuální sítě ze svého předplatného. 
5. Přihlaste se zadáním hello tooAzure toocreate hello virtuální sítě (Resource Manager) `login-azurermaccount` příkaz. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
6. Vytvořte skupinu prostředků a virtuální síť (Resource Manager). Zkopírujte skript hello, vložte jej do prostředí PowerShell a potom stiskněte klávesu `Enter`.

    ```powershell
    # Create a resource group.
      New-AzureRmResourceGroup -Name myResourceGroup -Location eastus

    # Create hello virtual network (Resource Manager).
      $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Vytvoření virtuální sítě partnerský vztah mezi hello dvě virtuální sítě vytvořené pomocí hello různé modely nasazení. Zkopírujte následující skript tooa textový editor ve vašem počítači hello. Nahraďte `<subscription id>` s ID vašeho předplatného. Pokud si nejste jisti Id předplatného, zadejte hello `Get-AzureRmSubscription` příkaz tooview ho. Hello hodnotu **Id** v hello vrátí výstupem je ID vašeho předplatného. skript hello tooexecute, kopie hello upravit skript z textového editoru, klikněte pravým tlačítkem v relaci prostředí PowerShell a potom stiskněte klávesu `Enter`.

    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Po provedení hello skriptu, zkontrolujte hello partnerský vztah pro virtuální síť hello (Resource Manager). Kopírování hello následující příkaz, vložte ho do relace prostředí PowerShell a potom stiskněte klávesu `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    výstup ukazuje Hello **připojeno** v hello **PeeringState** sloupce.

    Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

9. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
10. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-powershell) v tomto článku.
 
## <a name="permissions"></a>Oprávnění

Hello účty, že používáte toocreate partnerský vztah virtuální síť musí mít hello potřebné role nebo oprávnění. Například pokud dvě virtuální sítě s názvem myVnet1 a myVnet2 byly partnerský vztah, musí váš účet se přiřazenou hello následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Model nasazení|Role|Oprávnění|
|---|---|---|---|
|myVnet1|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Není k dispozici|
|myVnet2|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení konkrétní oprávnění příliš[vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="delete"></a>Odstraňte prostředky
Po dokončení tohoto kurzu, můžete chtít toodelete hello prostředků, kterou jste vytvořili v kurzu hello, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků hello.

### <a name="delete-portal"></a>Portál Azure

1. Hello portálu vyhledávacího pole zadejte **myResourceGroup**. Ve výsledcích hledání hello, klikněte na tlačítko **myResourceGroup**.
2. Na hello **myResourceGroup** okně klikněte na tlačítko hello **odstranit** ikonu.
3. odstranění hello tooconfirm, v hello **typ hello název skupiny prostředků** zadejte **myResourceGroup**a potom klikněte na **odstranit**.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

1. Použití Azure CLI 2.0 hello toodelete hello virtuální sítě (Resource Manager) s hello následující příkaz:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Použití Azure CLI 1.0 hello toodelete hello virtuální sítě (klasické) s hello následující příkazy:

    ```azurecli
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>Prostředí PowerShell

1. Zadejte následující příkaz toodelete hello virtuální sítě (Resource Manager) hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. toodelete hello virtuální sítě (klasické) v prostředí PowerShell, musíte upravit existující konfigurační soubor sítě. Zjistěte, jak příliš[exportovat, aktualizovat a import konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md). Odebrání hello následující element VirtualNetworkSite pro virtuální síť hello použili v tomto kurzu:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Import konfiguračního souboru změněné sítě může způsobit změny tooexisting virtuální sítě (klasické) v rámci vašeho předplatného. Zkontrolujte odeberete hello předchozí virtuální sítě a změníte nebo odeberte ostatní existující virtuální sítě ze svého předplatného. 

## <a name="next-steps"></a>Další kroky

- Důkladně Seznamte se s důležité [partnerského vztahu omezení virtuální sítě a chování](virtual-network-manage-peering.md#requirements-and-constraints) před vytvořením virtuální sítě partnerský vztah pro produkční použití.
- Další informace o všech [partnerského vztahu nastavení virtuální sítě](virtual-network-manage-peering.md#create-a-peering).
- Zjistěte, jak příliš[vytvořit rozbočovače a příčkou topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) s partnerský vztah virtuální sítě.
