---
title: "aaaCreate Azure virtuální sítě partnerský vztah – Resource Manager - různých předplatných | Microsoft Docs"
description: "Zjistěte, jak toocreate partnerský vztah mezi virtuálními sítěmi virtuální síť vytvořili pomocí Správce prostředků, který neexistuje v různých předplatných Azure."
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
ms.openlocfilehash: c7983a86031e061c1155144e5c493ee9578fa583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Vytvoření virtuální sítě partnerský vztah – Resource Manager, různých předplatných 

V tomto kurzu zjistíte toocreate virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí Správce prostředků. virtuální sítě Hello existovat v různých předplatných. Partnerského vztahu dva virtuální sítě umožňuje prostředky v různých virtuálních sítích toocommunicate s jinými s hello stejné šířky pásma a latence, jako by byl hello prostředky v hello stejné virtuální síti. Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md). 

Hello kroky toocreate virtuální sítě partnerský vztah se liší, v závislosti na tom, zda text hello virtuální sítě jsou v hello stejný nebo jiný, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuální sítě se vytvářejí prostřednictvím. Zjistěte, jak toocreate a virtuální sítě, partnerský vztah v jiných scénářích kliknutím hello scénář z hello následující tabulka:

|Model nasazení Azure  | Předplatné Azure  |
|--------- |---------|
|[I Resource Manager](virtual-network-create-peering.md) |stejné|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models.md) |stejné|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models-subscriptions.md) |Různé|

Mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic hello nelze vytvořit virtuální síť partnerský vztah. Partnerský vztah virtuální sítě mohou být vytvořeny pouze mezi dvěma virtuálními sítěmi, které existují v hello stejné oblasti Azure. Při vytváření virtuální sítě partnerský vztah mezi virtuálními sítěmi, které existují v různých předplatných, hello odběry musí být přidružené toohello stejné služby Azure Active Directory klienta. Pokud ještě nemáte klienta služby Azure Active Directory, můžete rychle [vytvořit](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Pokud potřebujete tooconnect virtuálních sítích obě vytvořených pomocí modelu nasazení classic hello, nebo které existovat v různých oblastech Azure, které existují v odběry přidružené toodifferent tenanty Azure Active Directory, můžete použít Azure [Brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuální sítě. 

Můžete použít hello [portál Azure](#portal), hello Azure [rozhraní příkazového řádku](#cli) (CLI), nebo Azure [prostředí PowerShell](#powershell) toocreate partnerský vztah virtuální sítě. Přímo toohello kroky pro vytvoření virtuální sítě partnerský vztah pomocí vaší nástroje, klepněte na libovolný hello předchozí nástroj odkazy toogo.

## <a name="portal"></a>Vytvoření partnerského vztahu – portál Azure

Tento kurz používá různé účty pro každé předplatné. Pokud používáte účet, který má oprávnění tooboth odběry, můžete použít hello stejný účet pro všechny kroky, hello kroky pro protokolování mimo portál hello přeskočit a přejít hello kroky pro přiřazení jiný uživatel oprávnění toohello virtuální sítě.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako uživatele. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
2. Klikněte na tlačítko **+ nový**, klikněte na tlačítko **sítě**, pak klikněte na tlačítko **virtuální síť**.
3. V hello **vytvořit virtuální síť** okno, zadejte, nebo vyberte hodnoty pro hello následující nastavení a potom klikněte na tlačítko **vytvořit**:
    - **Název**: *myVnetA*
    - **Adresní prostor**: *10.0.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.0.0.0/24*
    - **Předplatné**: Vyberte předplatné A.
    - **Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroupA*
    - **Umístění**: *východní USA*
4. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myVnetA*. Klikněte na tlačítko **myVnetA** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myVnetA** virtuální sítě.
5. V hello **myVnetA** okno, které se zobrazí, klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** z hello svislé seznam možností na levé straně okna hello hello.
6. V hello **myVnetA – řízení přístupu (IAM)** okno, které se zobrazí, klikněte na tlačítko **+ přidat**.
7. V hello **přidat oprávnění** okno, které se zobrazí, vyberte **Přispěvatel sítě** v hello **Role** pole.
8. V hello **vyberte** zaškrtněte, b nebo zadejte b na e-mailovou adresu toosearch. Hello seznam uživatelů, zobrazí se z hello stejné klienta Azure Active Directory jako virtuální síť hello nastavujete pro partnerský vztah hello.
9. Klikněte na **Uložit**.
10. V hello **myVnetA – řízení přístupu (IAM)** okně klikněte na tlačítko **vlastnosti** z hello svislé seznam možností na levé straně okna hello hello. Kopírování hello **ID prostředku**, která je použita v pozdější fázi. ID prostředku Hello je podobné toohello následující ukázka: /subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA.
11. Odhlaste se z portálu hello jako uživatele a přihlaste se jako b.
12. Proveďte kroky 2 – 3, zadáním nebo výběrem hello následující hodnoty v kroku 3:

    - **Název**: *myVnetB*
    - **Adresní prostor**: *10.1.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.1.0.0/24*
    - **Předplatné**: Vyberte předplatné B.
    - **Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroupB*
    - **Umístění**: *východní USA*

13. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myVnetB*. Klikněte na tlačítko **myVnetB** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myVnetB** virtuální sítě.
14. V hello **myVnetB** okno, které se zobrazí, klikněte na tlačítko **vlastnosti** z hello svislé seznam možností na levé straně okna hello hello. Kopírování hello **ID prostředku**, která je použita v pozdější fázi. ID prostředku Hello je podobné toohello následující ukázka: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB.
15. Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** v hello **myVnetB** okna a potom úplné kroky 5 až 10 pro myVnetB, zadávání **uživatele** v kroku 8.
16. Odhlaste se z portálu hello jako b a přihlaste se jako uživatele.
17. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myVnetA*. Klikněte na tlačítko **myVnetA** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myVnet** virtuální sítě.
18. Klikněte na tlačítko **myVnetA**.
19. V hello **myVnetA** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** z hello svislé seznam možností na levé straně okna hello hello.
20. V hello **myVnetA - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**
21. V hello **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte hello následující možnosti a potom klikněte na tlačítko **OK**:
     - **Název**: *myVnetAToMyVnetB*
     - **Virtuální síť modelu nasazení**: vyberte **Resource Manager**.
     - **Vím Moje ID prostředku**: Zaškrtněte toto políčko.
     - **ID prostředku**: Zadejte ID prostředku hello z kroku 14.
     - **Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.
    Žádné další nastavení použitá v tomto kurzu. Přečtěte si toolearn o všech nastaveních partnerského vztahu, [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).
22. Po kliknutí na **OK** v předchozím kroku hello hello **partnerský vztah přidat** okno se zavře a zobrazí hello **myVnetA - partnerských vztahů** okno znovu. Za několik sekund zobrazí se hello partnerský vztah, kterou jste vytvořili v okně hello. **Iniciované** je uvedena v hello **stav partnerského vztahu** sloupec pro hello **myVnetAToMyVnetB** partnerského vztahu, můžete vytvořit. Jste peered myVnetA toomyVnetB, ale teď musí peer myVnetB toomyVnetA. Hello partnerského vztahu musí být vytvořený v obou směrech tooenable prostředky v toocommunicate hello virtuálních sítí mezi sebou.
23. Odhlaste se z portálu hello jako uživatele a přihlaste se jako b.
24. Proveďte kroky 17 21 znovu pro myVnetB. V kroku č. 21, název partnerského vztahu hello *myVnetBToMyVnetA*, vyberte *myVnetA* pro **virtuální síť**a zadejte hello ID z kroku 10 v hello **ID prostředku** pole.
25. Několik sekund po kliknutí na **OK** toocreate hello partnerský vztah pro myVnetB, hello **myVnetBToMyVnetA** partnerský vztah, kterou jste právě vytvořili je označené **připojeno** v hello  **Partnerský vztah stav** sloupce.
26. Odhlaste se z portálu hello jako b a přihlaste se jako uživatele.
27. Proveďte kroky 17-19 znovu. Hello **stav partnerského vztahu** pro hello **myVnetAToVNetB** partnerského vztahu je nyní také **připojeno**. partnerský vztah Hello je úspěšně vytvořeno po uvidíte **připojeno** v hello **stav partnerského vztahu** sloupec pro obě virtuální sítě v partnerském vztahu hello. Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
28. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
29. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete-portal) tohoto článku.

## <a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure

Tento kurz používá různé účty pro každé předplatné. Pokud používáte účet, který má oprávnění tooboth odběry, můžete použít hello stejný účet pro všechny kroky, přeskočit hello kroky pro protokolování mimo Azure a odstranit řádky hello skriptu, které vytvořit přiřazení role uživatele. Nahraďte UserA@azure.com a UserB@azure.com ve všech hello následující skripty s hello uživatelských jmen, kterou používáte pro uživatele a b.

Hello následující skript:

- Vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. verze hello toofind, spusťte `az --version`. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Funguje v prostředí Bash. Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows hello rozhraní příkazového řádku Azure](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Místo instalace hello rozhraní příkazového řádku a jeho závislé součásti, můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. Klikněte na tlačítko hello **vyzkoušet** tlačítko ve skriptu hello, který následuje, které vyvolá cloudové prostředí, které se můžete přihlásit tooyour účet Azure s. 

1. Otevřete relaci rozhraní příkazového řádku a přihlaste se jako uživateli a pomocí hello tooAzure `azure login` příkaz. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
2. Zkopírujte následující skript tooa textový editor ve vašem počítači hello, nahraďte `<SubscriptionA-Id>` s hello ID SubscriptionA, pak kopie hello upravit skriptu, vložte jej v relaci příkazového řádku a stiskněte klávesu `Enter`. Pokud si nejste jisti Id předplatného, zadejte příkaz "az účet zobrazit" hello. Hello hodnotu **id** v hello výstupem je ID vašeho předplatného.

    ```azurecli-interactive
    # Create a resource group.
    az group create \
      --name myResourceGroupA \
      --location eastus

    # Create virtual network A.
    az network vnet create \
      --name myVnetA \
      --resource-group myResourceGroupA \
      --location eastus \
      --address-prefix 10.0.0.0/16

    # Assign UserB permissions toovirtual network A.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```
    
     Hello přiřazení oprávnění pro b není povinné. Partnerský vztah lze navázat i v případě, že uživatelé jednotlivě vyvolat partnerského vztahu požadavky pro příslušné virtuální sítě, dokud hello požadavky shodu. Přidání privilegované uživatel myVNetB jako Přispěvatel sítě v hello místní virtuální síť umožňuje snazší toodo hello instalace.
3. Odhlaste se z Azure jako uživateli a pomocí hello `az logout` příkaz a potom přihlásit tooAzure jako b. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
4. Vytvořte myVnetB. Zkopírujte obsah skriptů hello v kroku 2 tooa textový editor ve vašem počítači. Nahraďte `<SubscriptionA-Id>` s ID SubscriptionB hello. Změňte 10.0.0.0/16 too10.1.0.0/16, změn všechny jako tooB a všechny Bs tooA. Zkopírujte skript hello upravit, vložte jej v relaci tooyour rozhraní příkazového řádku a stiskněte klávesu `Enter`. 
5. Odhlaste se z Azure jako b a tooAzure jako uživatele pro přihlášení.
6. Vytvoření partnerského vztahu z myVnetA toomyVnetB virtuální sítě. Zkopírujte následující skript obsah tooa textový editor ve vašem počítači hello. Nahraďte `<SubscriptionB-Id>` s ID SubscriptionB hello. tooexecute hello skriptu, zkopírujte skript hello upravit, vložte jej do relace rozhraní příkazového řádku a stiskněte klávesu Enter.
 
    ```azurecli-interactive
        # Get hello id for myVnetA.
        vnetAId=$(az network vnet show \
          --resource-group myResourceGroupA \
          --name myVnetA \
          --query id --out tsv)
    
        # Peer myVNetA toomyVNetB.
        az network vnet peering create \
          --name myVnetAToMyVnetB \
          --resource-group myResourceGroupA \
          --vnet-name myVnetA \
          --remote-vnet-id /subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/VirtualNetworks/myVnetB \
          --allow-vnet-access
    ```

7. Zobrazit stav partnerského vztahu hello myVnetA.

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroupA \
      --vnet-name myVnetA \
      --output table
    ```

    Stav Hello je **získaných**. Změní příliš**připojeno** po vytvoření partnerského vztahu toomyVnetA hello z myVnetB.

8. Odhlášení uživatele z Azure a přihlaste se tooAzure jako b.
9. Vytvoření, hello partnerského vztahu z myVnetB toomyVnetA. Zkopírujte obsah skriptů hello v kroku 6 tooa textový editor ve vašem počítači. Nahraďte `<SubscriptionB-Id>` s hello ID pro SubscriptionA a změňte všechny jako tooB a všechny Bs tooA. Po provedení změn hello, kopie hello upravit skriptu, vložte jej do relace rozhraní příkazového řádku a stiskněte klávesu `Enter`.
10. Zobrazit stav partnerského vztahu hello myVnetB. Zkopírujte obsah skriptů hello v kroku 7 tooa textový editor ve vašem počítači. Změnit tooB pro skupinu prostředků hello a názvů virtuálních sítí, zkopírujte skript hello, vložte hello upravit skript v relaci tooyour rozhraní příkazového řádku a stiskněte klávesu `Enter`. Hello partnerského vztahu stav je **připojeno**. Hello partnerského vztahu stav myVnetA změny příliš**připojeno** po vytvoření partnerského vztahu hello z myVnetB toomyVnetA. Se můžete přihlásit uživatele zpět tooAzure a dokončení kroku 7 znovu tooverify hello partnerského vztahu stav myVnetA. 

    > [!NOTE]
    > partnerský vztah Hello nebyl určen, dokud je stav partnerského vztahu hello **připojeno** pro obě virtuální sítě.

11. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
12. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-cli) v tomto článku.

Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
 
## <a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell

Tento kurz používá různé účty pro každé předplatné. Pokud používáte účet, který má oprávnění tooboth odběry, můžete použít hello stejný účet pro všechny kroky, přeskočit hello kroky pro protokolování mimo Azure a odstranit řádky hello skriptu, které vytvořit přiřazení role uživatele. Nahraďte UserA@azure.com a UserB@azure.com ve všech hello následující skripty s hello uživatelských jmen, kterou používáte pro uživatele a b.

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Spusťte relaci prostředí PowerShell.
3. V prostředí PowerShell, přihlaste se tooAzure jako uživatele tak, že zadáte hello `login-azurermaccount` příkaz. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
4. Vytvořte skupinu prostředků a virtuální síť A. kopie hello následující text skriptu tooa editor ve vašem počítači. Nahraďte `<SubscriptionA-Id>` s ID SubscriptionA hello. Pokud si nejste jisti Id předplatného, zadejte hello `Get-AzureRmSubscription` příkaz tooview ho. Hello hodnotu **Id** v hello vrátí výstupem je ID vašeho předplatného. skript hello tooexecute, kopie hello upravit skriptu, vložte jej v tooPowerShell a stiskněte klávesu `Enter`.

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions toomyVnetA.
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

    Hello přiřazení oprávnění pro b není povinné. Partnerský vztah lze navázat i v případě, že uživatelé jednotlivě vyvolat partnerského vztahu požadavky pro příslušné virtuální sítě, dokud hello požadavky shodu. Přidání privilegované uživatel hello jiné virtuální síti jako uživatel v hello místní virtuální síť je snazší toodo hello instalace.
5. Odhlášení uživatele z Azure a přihlaste se b. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
6. Zkopírujte obsah skriptů hello v kroku 4 tooa textový editor ve vašem počítači. Nahraďte `<SubscriptionA-Id>` s hello ID pro předplatné too10.1.0.0/16 10.0.0.0/16 B. změnu. Změňte všechny jako tooB a všechny Bs tooA. skript hello tooexecute, kopie hello upravit skriptu, vložte do prostředí PowerShell a stiskněte klávesu `Enter`.
7. Odhlaste se b z Azure a přihlášení uživatele.
8. Vytvoření, hello partnerského vztahu z myVnetA toomyVnetB. Zkopírujte následující skript tooa textový editor ve vašem počítači hello. Nahraďte `<SubscriptionB-Id>` s hello ID předplatného B. tooexecute hello skriptu, zkopírujte skript hello upravit, vložte tooPowerShell a stiskněte klávesu `Enter`.
 
    ```powershell
    # Peer myVnetA toomyVnetB.
    $vNetA=Get-AzureRmVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vNetA `
      -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
    ```

9. Zobrazit stav partnerského vztahu hello myVnetA.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Stav Hello je **získaných**. Změní příliš**připojeno** po nastavení partnerského vztahu toomyVnetA hello z myVnetB.

10. Odhlášení uživatele z Azure a přihlaste se b.
11. Vytvoření, hello partnerského vztahu z myVnetB toomyVnetA. Zkopírujte obsah skriptů hello v kroku 8 tooa textový editor ve vašem počítači. Nahraďte `<SubscriptionB-Id>` s hello ID předplatného A a změňte všechny na tooB a tooA všechny B. skript hello tooexecute, kopie hello upravit skriptu, vložte jej v tooPowerShell a stiskněte klávesu `Enter`.
12. Zobrazit stav partnerského vztahu hello myVnetB. Zkopírujte obsah skriptů hello v kroku 9 tooa textový editor ve vašem počítači. Změňte tooB pro skupinu prostředků hello a názvů virtuálních sítí. Vložte skript hello upravit do prostředí PowerShell tooexecute hello skript a stiskněte klávesu `Enter`. Stav Hello je **připojeno**. Hello partnerského vztahu stav **myVnetA** změní příliš**připojeno** po vytvoření partnerského vztahu hello z **myVnetB** příliš**myVnetA**. Se můžete přihlásit uživatele zpět tooAzure a dokončení kroku 9 znovu tooverify hello partnerského vztahu stav myVnetA. 

    > [!NOTE]
    > partnerský vztah Hello nebyl určen, dokud je stav partnerského vztahu hello **připojeno** pro obě virtuální sítě.

    Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

13. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
14. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-powershell) v tomto článku.

## <a name="permissions"></a>Oprávnění

Hello účty, že používáte toocreate partnerský vztah virtuální síť musí mít hello potřebné role nebo oprávnění. Například, pokud byly partnerský vztah dvě virtuální sítě s názvem **myVnetA** a **myVnetB**, musí mít váš účet přiřazenou hello následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Role|Oprávnění|
|---|---|---|
|myVnetA|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|myVnetB|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení konkrétní oprávnění příliš[vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="delete"></a>Odstraňte prostředky
Po dokončení tohoto kurzu, můžete chtít toodelete hello prostředků, kterou jste vytvořili v kurzu hello, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků hello.

### <a name="delete-portal"></a>Portál Azure

1. Přihlaste se toohello portálu Azure jako uživatele.
2. Hello portálu vyhledávacího pole zadejte **myResourceGroupA**. Ve výsledcích hledání hello, klikněte na tlačítko **myResourceGroupA**.
3. Na hello **myResourceGroupA** okně klikněte na tlačítko hello **odstranit** ikonu.
4. odstranění hello tooconfirm, v hello **typ hello název skupiny prostředků** zadejte **myResourceGroupA**a potom klikněte na **odstranit**.
5. Odhlaste se z portálu hello jako uživatele a přihlaste se jako b.
6. Dokončete kroky 2 až 4 pro myResourceGroupB.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

1. Přihlaste se tooAzure jako uživatele a spustit hello následující příkaz:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```
2. Odhlaste se z Azure jako uživatele a přihlaste se jako b.
3. Spusťte následující příkaz hello:

    ```azurecli-interactive
    az group delete --name myResourceGroupB --yes
    ```

### <a name="delete-powershell"></a>Prostředí PowerShell

1. Přihlaste se tooAzure jako uživatele a spustit hello následující příkaz:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -force
    ```

2. Odhlaste se z Azure jako uživatele a přihlaste se jako b.
3. Spusťte následující příkaz hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupB -force
    ```

## <a name="next-steps"></a>Další kroky

- Důkladně Seznamte se s důležité [partnerského vztahu omezení virtuální sítě a chování](virtual-network-manage-peering.md#requirements-and-constraints) před vytvořením virtuální sítě partnerský vztah pro produkční použití.
- Další informace o všech [partnerského vztahu nastavení virtuální sítě](virtual-network-manage-peering.md#create-a-peering).
- Zjistěte, jak příliš[vytvořit rozbočovače a příčkou topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) s partnerský vztah virtuální sítě.
