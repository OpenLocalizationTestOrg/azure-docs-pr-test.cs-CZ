---
title: "aaaCreate virtuální síti Azure partnerský vztah – Resource Manager - stejného předplatného. | Microsoft Docs"
description: "Zjistěte, jak virtuální sítě vztahy mezi virtuálními sítěmi toocreate vytváří pomocí Správce prostředků, který neexistuje v hello stejné předplatné Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: c2d24fdc8103c09c3bfb8e59be12e301d9e9a55a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>Vytvoření virtuální sítě partnerský vztah – Resource Manager, stejného předplatného.

V tomto kurzu zjistíte toocreate virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků. Obě virtuální sítě existuje v hello stejné předplatné. Partnerského vztahu dva virtuální sítě umožňuje prostředky v různých virtuálních sítích toocommunicate s jinými s hello stejné šířky pásma a latence, jako by byl hello prostředky v hello stejné virtuální síti. Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md). 

Hello kroky toocreate virtuální sítě partnerský vztah se liší, v závislosti na tom, zda text hello virtuální sítě jsou v hello stejný nebo jiný, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuální sítě se vytvářejí prostřednictvím. Zjistěte, jak toocreate a virtuální sítě, partnerský vztah v jiných scénářích kliknutím hello scénář z hello následující tabulka:

|Model nasazení Azure  | Předplatné Azure  |
|--------- |---------|
|[I Resource Manager](create-peering-different-subscriptions.md) |Různé|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models.md) |stejné|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models-subscriptions.md) |Různé|

Mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic hello nelze vytvořit virtuální síť partnerský vztah. Partnerský vztah virtuální sítě mohou být vytvořeny pouze mezi dvěma virtuálními sítěmi, které existují v hello stejné oblasti Azure. Pokud potřebujete tooconnect virtuální sítě, které byly obě vytvořené pomocí modelu nasazení classic hello nebo které existovat v různých oblastech Azure, můžete použít Azure [brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuální sítě. 

Můžete použít hello [portál Azure](#portal), hello Azure [rozhraní příkazového řádku](#cli) (CLI) Azure [prostředí PowerShell](#powershell), nebo [šablony Azure Resource Manageru](#template) toocreate partnerský vztah virtuální sítě. Přímo toohello kroky pro vytvoření virtuální sítě partnerský vztah pomocí vaší nástroje, klepněte na libovolný hello předchozí nástroj odkazy toogo.

## <a name="portal"></a>Vytvoření partnerského vztahu – portál Azure

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
4. Proveďte kroky 2 až 3 znovu zadání hello následující hodnoty v kroku 3:
    - **Název**: *myVnet2*
    - **Adresní prostor**: *10.1.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.1.0.0/24*
    - **Předplatné**: Vyberte předplatné
    - **Skupina prostředků**: vyberte **použít existující** a vyberte *myResourceGroup*
    - **Umístění**: *východní USA*
5. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myResourceGroup*. Klikněte na tlačítko **myResourceGroup** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myresourcegroup** skupinu prostředků. Skupina prostředků Hello obsahuje hello dvě virtuální sítě vytvořené v předchozích krocích.
6. Klikněte na tlačítko **myVNet1**.
7. V hello **myVnet1** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** z hello svislé seznam možností na levé straně okna hello hello.
8. V hello **myVnet1 - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**
9. V hello **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte hello následující možnosti a potom klikněte na tlačítko **OK**:
     - **Název**: *myVnet1ToMyVnet2*
     - **Virtuální síť modelu nasazení**: vyberte **Resource Manager**. 
     - **Předplatné**: Vyberte předplatné
     - **Virtuální síť**: klikněte na tlačítko **vyberte virtuální síť**, pak klikněte na tlačítko **myVnet2**.
     - **Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.
    Žádné další nastavení použitá v tomto kurzu. Přečtěte si toolearn o všech nastaveních partnerského vztahu, [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).
10. Po kliknutí na **OK** v předchozím kroku hello hello **partnerský vztah přidat** okno se zavře a zobrazí hello **myVnet1 - partnerských vztahů** okno znovu. Za několik sekund zobrazí se hello partnerský vztah, kterou jste vytvořili v okně hello. **Iniciované** je uvedena v hello **stav partnerského vztahu** sloupec pro hello **myVnet1ToMyVnet2** partnerského vztahu, můžete vytvořit. Jste peered Vnet1 tooVnet2, ale teď musí peer myVnet2 toomyVnet1. Hello partnerského vztahu musí být vytvořený v obou směrech tooenable prostředky v toocommunicate hello virtuálních sítí mezi sebou.
11. Proveďte kroky 5 až 10 znovu pro myVnet2.  Partnerský vztah hello název *myVnet2ToMyVnet1*.
12. Několik sekund po kliknutí na **OK** toocreate hello partnerský vztah pro MyVnet2, hello **myVnet2ToMyVnet1** partnerský vztah, kterou jste právě vytvořili je označené **připojeno** v hello  **Partnerský vztah stav** sloupce.
13. Znovu proveďte kroky 5 až 7 pro MyVnet1. Hello **stav partnerského vztahu** pro hello **myVnet1ToVNet2** partnerského vztahu je nyní také **připojeno**. partnerský vztah Hello je úspěšně vytvořeno po uvidíte **připojeno** v hello **stav partnerského vztahu** sloupec pro obě virtuální sítě v partnerském vztahu hello.
14. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
15. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete-portal) tohoto článku.

Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure

Hello následující skript:

- Vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. verze hello toofind, spusťte hello `az --version` příkaz. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Funguje v prostředí Bash. Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows hello rozhraní příkazového řádku Azure](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Místo instalace hello rozhraní příkazového řádku a jeho závislé součásti, můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. Klikněte na tlačítko hello **vyzkoušet** tlačítka na hello skript, který způsobem, které vyvolá prostředí cloudu, který vám umožní přihlásit se tooyour účet Azure s. tooexecute hello skriptu, klikněte na tlačítko hello **kopie** tlačítko a vložte obsah hello do své cloudové prostředí.

1. Vytvořte skupinu prostředků a dvě virtuální sítě.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi hello.
 
    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get hello id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 tooVNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. Po provedení hello skriptu, zkontrolujte hello partnerských vztahů pro každou virtuální síť. 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Opakujte předchozí příkaz hello, nahraďte *myVnet1* s *myVnet2*. výstup Hello obou příkazů ukazuje **připojeno** v hello **PeeringState** sloupce.

     Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

4. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
5. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-cli) v tomto článku.


## <a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. toostart relaci prostředí PowerShell, přejděte příliš**spustit**, zadejte **prostředí powershell**a potom klikněte na **prostředí PowerShell**.
3. V prostředí PowerShell přihlásit tooAzure zadáním hello `login-azurermaccount` příkaz. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
4. Vytvořte skupinu prostředků a dvě virtuální sítě. tooexecute hello skriptu kopie hello následující skript, vložte jej do prostředí PowerShell a stiskněte klávesu `Enter` po poslední řádek hello se zobrazí na úvodní obrazovka:

    ```powershell
    # Variables for common values used throughout hello script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi hello. Kopírování hello následující skript, vložte tooPowerShell a stiskněte klávesu `Enter` po poslední řádek hello se zobrazí na úvodní obrazovka:
    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 tooVNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. tooreview hello podsítě pro virtuální síť hello, kopie hello následující příkaz, vložte tooPowerShell a potom stiskněte klávesu `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Opakujte předchozí příkaz hello, nahraďte *myVnet1* s *myVnet2*. výstup Hello obou příkazů ukazuje **připojeno** v hello **PeeringState** sloupce.
7. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
8. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-powershell) v tomto článku.

Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Vytvoření partnerského vztahu - šablony Resource Manageru

1. Referenční dokumentace [vytvoření virtuální sítě partnerského vztahu](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) šablony Resource Manageru. Pokyny jsou k dispozici hello šablonou pro nasazení šablony hello pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure. Přihlášení toowhichever nástroj, který zvolíte toodeploy hello šablony s použitím účtu, který má hello toocreate potřebná oprávnění, virtuální sítě partnerský vztah. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
2. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
3. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete) hello tohoto článku, buď pomocí portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.

## <a name="permissions"></a>Oprávnění

Hello účty, že používáte toocreate partnerský vztah virtuální síť musí mít hello potřebné role nebo oprávnění. Například pokud dvě virtuální sítě s názvem VNet1 a VNet2 byly partnerský vztah, musí váš účet se přiřazenou hello následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Role|Oprávnění|
|---|---|---|
|VNet1|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení konkrétní oprávnění příliš[vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="delete"></a>Odstraňte prostředky
Po dokončení tohoto kurzu, můžete chtít toodelete hello prostředků, kterou jste vytvořili v kurzu hello, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků hello.

### <a name="delete-portal"></a>Portál Azure

1. Hello portálu vyhledávacího pole zadejte **myResourceGroup**. Ve výsledcích hledání hello, klikněte na tlačítko **myResourceGroup**.
2. Na hello **myResourceGroup** okně klikněte na tlačítko hello **odstranit** ikonu.
3. odstranění hello tooconfirm, v hello **typ hello název skupiny prostředků** zadejte **myResourceGroup**a potom klikněte na **odstranit**.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

Zadejte hello následující příkaz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>Prostředí PowerShell

Zadejte hello následující příkaz:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a>Další kroky

- Důkladně Seznamte se s důležité [partnerského vztahu omezení virtuální sítě a chování](virtual-network-manage-peering.md#requirements-and-constraints) před vytvořením virtuální sítě partnerský vztah pro produkční použití.
- Další informace o všech [partnerského vztahu nastavení virtuální sítě](virtual-network-manage-peering.md#create-a-peering).
- Zjistěte, jak příliš[vytvořit rozbočovače a příčkou topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) s partnerský vztah virtuální sítě.
