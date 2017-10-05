---
title: "Vytvoření virtuální síti Azure partnerského vztahu – Resource Manager - stejného předplatného. | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků, které existují ve stejném předplatném Azure."
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
ms.openlocfilehash: a32a6b33e04c603325ab3612f61e5852682eac7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>Vytvoření virtuální sítě partnerský vztah – Resource Manager, stejného předplatného.

V tomto kurzu zjistíte vytvořit virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků. Obě virtuální sítě existovat ve stejném předplatném. Partnerský vztah dva prostředky umožňuje virtuální sítě v různých virtuálních sítích ke komunikaci mezi sebou stejným šířky pásma a latence, jako by byl prostředky ve stejné virtuální síti. Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md). 

Postup vytvoření virtuální sítě partnerského vztahu se liší v závislosti na tom, jestli virtuální sítě jsou ve stejné nebo jiné, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuální sítě se vytvářejí pomocí. Naučte se vytvořit virtuální síť partnerský vztah v jiných scénářích kliknutím na scénář z v následující tabulce:

|Model nasazení Azure  | Předplatné Azure  |
|--------- |---------|
|[I Resource Manager](create-peering-different-subscriptions.md) |Různé|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models.md) |stejné|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models-subscriptions.md) |Různé|

Virtuální síť partnerský vztah nelze vytvořit mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic. Partnerský vztah virtuální síť lze vytvořit pouze mezi dvěma virtuálními sítěmi, které existují ve stejné oblasti Azure. Pokud potřebujete připojit virtuální sítě, které byly obě vytvořené pomocí modelu nasazení classic nebo které existovat v různých oblastech Azure, můžete použít Azure [brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) připojení virtuální sítě. 

Můžete použít [portál Azure](#portal), Azure [rozhraní příkazového řádku](#cli) (CLI) Azure [prostředí PowerShell](#powershell), nebo [šablony Azure Resource Manageru](#template)vytvoření virtuální sítě partnerského vztahu. Klikněte na libovolný předchozí odkaz nástroj přejít přímo na kroky pro vytvoření virtuální sítě partnerský vztah nástroji vašeho výběru.

## <a name="portal"></a>Vytvoření partnerského vztahu – portál Azure

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com). Účet, ke kterému se přihlásíte, musí mít potřebná oprávnění k vytvoření virtuální sítě partnerského vztahu. Najdete v článku [oprávnění](#permissions) tohoto článku podrobnosti.
2. Klikněte na tlačítko **+ nový**, klikněte na tlačítko **sítě**, pak klikněte na tlačítko **virtuální síť**.
3. V **vytvořit virtuální síť** okno, zadejte, nebo vyberte hodnoty pro následující nastavení a potom klikněte na tlačítko **vytvořit**:
    - **Název**: *myVnet1*
    - **Adresní prostor**: *10.0.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.0.0.0/24*
    - **Předplatné**: Vyberte předplatné
    - **Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroup*
    - **Umístění**: *východní USA*
4. Proveďte kroky 2 až 3 znovu zadat následující hodnoty v kroku 3:
    - **Název**: *myVnet2*
    - **Adresní prostor**: *10.1.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.1.0.0/24*
    - **Předplatné**: Vyberte předplatné
    - **Skupina prostředků**: vyberte **použít existující** a vyberte *myResourceGroup*
    - **Umístění**: *východní USA*
5. V **vyhledávání prostředků** pole v horní části portálu, typ *myResourceGroup*. Klikněte na tlačítko **myResourceGroup** při zobrazí ve výsledcích hledání. Zobrazí okno **myresourcegroup** skupinu prostředků. Skupina prostředků obsahuje dvě virtuální sítě vytvořené v předchozích krocích.
6. Klikněte na tlačítko **myVNet1**.
7. V **myVnet1** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** ze seznamu svislé možností na levé straně okna.
8. V **myVnet1 - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**
9. V **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte následující možnosti a potom klikněte na tlačítko **OK**:
     - **Název**: *myVnet1ToMyVnet2*
     - **Virtuální síť modelu nasazení**: vyberte **Resource Manager**. 
     - **Předplatné**: Vyberte předplatné
     - **Virtuální síť**: klikněte na tlačítko **vyberte virtuální síť**, pak klikněte na tlačítko **myVnet2**.
     - **Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.
    Žádné další nastavení použitá v tomto kurzu. Další informace o všech nastaveních partnerského vztahu, přečtěte si [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).
10. Po kliknutí na **OK** v předchozím kroku, **partnerský vztah přidat** okno se zavře a zobrazí **myVnet1 - partnerských vztahů** okno znovu. Za několik sekund partnerského vztahu, kterou jste vytvořili se zobrazí v okně. **Iniciované** , je uvedena ve **stav partnerského vztahu** sloupec pro **myVnet1ToMyVnet2** partnerského vztahu, můžete vytvořit. Jste peered Vnet1 k Vnet2, ale teď musí peer myVnet2 k myVnet1. Partnerský vztah, musí být vytvořený v obou směrech povolit prostředky ve virtuálních sítích ke komunikaci mezi sebou.
11. Proveďte kroky 5 až 10 znovu pro myVnet2.  Název partnerského vztahu *myVnet2ToMyVnet1*.
12. Několik sekund po kliknutí na **OK** vytvoření partnerského vztahu pro MyVnet2, **myVnet2ToMyVnet1** partnerský vztah, kterou jste právě vytvořili je označené **připojeno** v  **Partnerský vztah stav** sloupce.
13. Znovu proveďte kroky 5 až 7 pro MyVnet1. **Stav partnerského vztahu** pro **myVnet1ToVNet2** partnerského vztahu je nyní také **připojeno**. Partnerského vztahu je úspěšně vytvořeno po uvidíte **připojeno** v **stav partnerského vztahu** sloupec pro obě virtuální sítě v partnerském vztahu.
14. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.
15. **Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete-portal) tohoto článku.

Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je nyní možné vzájemně komunikovat prostřednictvím jejich IP adresy. Pokud používáte překlad výchozí Azure pro virtuální sítě, nejsou prostředky ve virtuálních sítích překládat názvy virtuálních sítí. Pokud chcete překládat názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak nastavit [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure

Následující skript:

- Vyžaduje Azure CLI verze verze 2.0.4 nebo novější. Chcete-li najít verzi, spusťte `az --version` příkaz. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Funguje v prostředí Bash. Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Místo instalace rozhraní příkazového řádku a jeho závislé součásti, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem. Klikněte **vyzkoušet** tlačítko ve skriptu, který způsobem, které vyvolá prostředí cloudu, který vám umožní přihlásit se k účtu Azure s. Chcete-li spustit skript, klikněte na tlačítko **kopie** tlačítko a umožňuje vložit obsah do své cloudové prostředí.

1. Vytvořte skupinu prostředků a dvě virtuální sítě.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
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

2. Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi.
 
    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get the id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 to VNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. Po spuštění skriptu, zkontrolujte partnerských vztahů pro každou virtuální síť. 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Předchozí příkaz spusťte znovu, nahraďte *myVnet1* s *myVnet2*. Výstup z obou příkazů ukazuje **připojeno** v **PeeringState** sloupce.

     Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je nyní možné vzájemně komunikovat prostřednictvím jejich IP adresy. Pokud používáte překlad výchozí Azure pro virtuální sítě, nejsou prostředky ve virtuálních sítích překládat názvy virtuálních sítí. Pokud chcete překládat názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak nastavit [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

4. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.
5. **Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete-cli) v tomto článku.


## <a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell

1. Nainstalujte nejnovější verzi modulu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) pro PowerShell. Pokud s Azure PowerShellem začínáte, podívejte se na [Přehled Azure PowerShellu](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Pokud chcete spustit relaci PowerShellu, přejděte do nabídky **Start**, zadejte **powershell** a potom klikněte na **PowerShell**.
3. V PowerShellu se přihlaste k Azure zadáním příkazu `login-azurermaccount`. Účet, ke kterému se přihlásíte, musí mít potřebná oprávnění k vytvoření virtuální sítě partnerského vztahu. Najdete v článku [oprávnění](#permissions) tohoto článku podrobnosti.
4. Vytvořte skupinu prostředků a dvě virtuální sítě. Spustit skript, zkopírujte následující skript, vložte jej do prostředí PowerShell a potom stiskněte klávesu `Enter` po poslední řádek se zobrazí na obrazovce:

    ```powershell
    # Variables for common values used throughout the script.
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

5. Vytvoření virtuální sítě partnerský vztah mezi dvěma virtuálními sítěmi. Zkopírujte následující skript, vložte v prostředí PowerShell a potom stiskněte klávesu `Enter` po poslední řádek se zobrazí na obrazovce:
    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 to VNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. Ke kontrole podsítě virtuální sítě, zkopírujte následující příkaz, vložte v prostředí PowerShell a stiskněte klávesu `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Předchozí příkaz spusťte znovu, nahraďte *myVnet1* s *myVnet2*. Výstup z obou příkazů ukazuje **připojeno** v **PeeringState** sloupce.
7. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.
8. **Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete-powershell) v tomto článku.

Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je nyní možné vzájemně komunikovat prostřednictvím jejich IP adresy. Pokud používáte překlad výchozí Azure pro virtuální sítě, nejsou prostředky ve virtuálních sítích překládat názvy virtuálních sítí. Pokud chcete překládat názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak nastavit [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Vytvoření partnerského vztahu - šablony Resource Manageru

1. Referenční dokumentace [vytvoření virtuální sítě partnerského vztahu](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) šablony Resource Manageru. Součástí šablony jsou pokyny pro její nasazení pomocí webu Azure Portal, PowerShellu nebo Azure CLI. Přihlaste se ke podle toho, co nástroj, můžete zvolit pro nasazení šablony s použitím účtu, který má nezbytná oprávnění k vytvoření virtuální sítě partnerského vztahu. Najdete v článku [oprávnění](#permissions) tohoto článku podrobnosti.
2. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jednoho virtuálního počítače na druhý k ověření připojení.
3. **Volitelné**: Pokud chcete odstranit prostředky, které vytvoříte v tomto kurzu, proveďte kroky v [odstranit prostředky](#delete) tohoto článku, pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.

## <a name="permissions"></a>Oprávnění

Účty, které použijete k vytvoření virtuální sítě partnerského vztahu musí mít potřebné role nebo oprávnění. Například pokud dvě virtuální sítě s názvem VNet1 a VNet2 byly partnerský vztah, musí váš účet se přiřazenou následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Role|Oprávnění|
|---|---|---|
|VNet1|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení specifické oprávnění pro [vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="delete"></a>Odstraňte prostředky
Po dokončení tohoto kurzu, můžete chtít odstranit z prostředků, které jste vytvořili v tomto kurzu, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků.

### <a name="delete-portal"></a>Portál Azure

1. V dialogovém okně hledání portálu zadejte **myResourceGroup**. Ve výsledcích hledání klikněte na tlačítko **myResourceGroup**.
2. Na **myResourceGroup** okně klikněte **odstranit** ikonu.
3. Potvrďte odstranění, v **název skupiny prostředků typu** zadejte **myResourceGroup**a potom klikněte na **odstranit**.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

Zadejte následující příkaz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>Prostředí PowerShell

Zadejte následující příkaz:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a>Další kroky

- Důkladně Seznamte se s důležité [partnerského vztahu omezení virtuální sítě a chování](virtual-network-manage-peering.md#requirements-and-constraints) před vytvořením virtuální sítě partnerský vztah pro produkční použití.
- Další informace o všech [partnerského vztahu nastavení virtuální sítě](virtual-network-manage-peering.md#create-a-peering).
- Zjistěte, jak [vytvořit rozbočovače a příčkou topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) s partnerský vztah virtuální sítě.
