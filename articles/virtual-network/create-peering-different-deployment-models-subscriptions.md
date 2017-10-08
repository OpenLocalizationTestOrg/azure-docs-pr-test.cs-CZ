---
title: "virtuální síti Azure partnerský vztah - aaaCreate jiné nasazení modelů - různých předplatných | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální síť partnerský vztah mezi virtuálními sítěmi vytvořena prostřednictvím různých nasazení Azure modely, které existují v různých předplatných Azure."
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
ms.openlocfilehash: 865bdabb5b87523ba943d7b5dcbdc2475b78bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Vytvořit virtuální síť partnerský vztah - různé modely nasazení a odběry

V tomto kurzu zjistíte toocreate virtuální síť partnerský vztah mezi virtuální sítě vytvořené pomocí různé modely nasazení. virtuální sítě Hello existovat v různých předplatných. Partnerského vztahu dva virtuální sítě umožňuje prostředky v různých virtuálních sítích toocommunicate s jinými s hello stejné šířky pásma a latence, jako by byl hello prostředky v hello stejné virtuální síti. Další informace o [partnerský vztah virtuální sítě](virtual-network-peering-overview.md). 

Hello kroky toocreate virtuální sítě partnerský vztah se liší, v závislosti na tom, zda text hello virtuální sítě jsou v hello stejný nebo jiný, odběry a které [modelu nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuální sítě se vytvářejí prostřednictvím. Zjistěte, jak toocreate a virtuální sítě, partnerský vztah v jiných scénářích kliknutím hello scénář z hello následující tabulka:

|Model nasazení Azure  | Předplatné Azure  |
|--------- |---------|
|[I Resource Manager](virtual-network-create-peering.md) |stejné|
|[I Resource Manager](create-peering-different-subscriptions.md) |Různé|
|[Jeden Resource Manager, jeden classic](create-peering-different-deployment-models.md) |stejné|

Mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic hello nelze vytvořit virtuální síť partnerský vztah. Partnerský vztah virtuální sítě mohou být vytvořeny pouze mezi dvěma virtuálními sítěmi, které existují v hello stejné oblasti Azure. Při vytváření virtuální sítě partnerský vztah mezi virtuálními sítěmi, které existují v různých předplatných, hello odběry musí být přidružené toohello stejné služby Azure Active Directory klienta. Pokud ještě nemáte klienta služby Azure Active Directory, můžete rychle [vytvořit](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Pokud potřebujete tooconnect virtuálních sítích obě vytvořených pomocí modelu nasazení classic hello, nebo které existovat v různých oblastech Azure, které existují v odběry přidružené toodifferent tenanty Azure Active Directory, můžete použít Azure [Brány VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuální sítě. 

> [!WARNING]
> Vytvoření virtuální sítě partnerský vztah mezi virtuálními sítěmi v vytvořena prostřednictvím různých nasazení Azure modely, které existují v různých předplatných je aktuálně ve verzi preview. Partnerské vztahy virtuální sítě vytvořené v tomto scénáři nemusí mít stejnou úroveň dostupnost a spolehlivost jako vytvoření virtuální sítě partnerský vztah ve scénářích obecné dostupnosti verze hello. Partnerské vztahy virtuální sítě vytvořené v tomto scénáři nejsou podporovány, může mít omezené možnosti a nemusí být k dispozici ve všech oblastech Azure. Hello nejaktuálnější upozornění na stav této funkce a dostupnost, zkontrolujte hello [aktualizace virtuální sítě Azure](https://azure.microsoft.com/updates/?product=virtual-network) stránky.

Můžete použít hello [portál Azure](#portal), hello Azure [rozhraní příkazového řádku](#cli) (CLI), nebo Azure [prostředí PowerShell](#powershell) toocreate partnerský vztah virtuální sítě. Přímo toohello kroky pro vytvoření virtuální sítě partnerský vztah pomocí vaší nástroje, klepněte na libovolný hello předchozí nástroj odkazy toogo.

## <a name="register"></a>Registrace pro hello preview

tooregister pro verzi preview hello dokončení hello kroky, které dodržujte pro oba odběry, které obsahují hello virtuální sítě má toopeer. Hello pouze nástroj tooregister můžete použít pro hello preview je prostředí PowerShell.

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Spusťte relaci prostředí PowerShell a přihlaste se pomocí hello tooAzure `login-azurermaccount` příkaz.
3. Registrace předplatného pro hello preview zadáním hello následující příkazy:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
    Neprovádějte kroky hello v hello portálu, rozhraní příkazového řádku Azure nebo PowerShell části tohoto článku až hello **RegistrationState** výstupu se zobrazí po zadání hello následující příkaz je **registrovaná** pro oba odběry:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    ```

## <a name="portal"></a>Vytvoření partnerského vztahu – portál Azure

Tento kurz používá různé účty pro každé předplatné. Pokud používáte účet, který má oprávnění tooboth odběry, můžete použít hello stejný účet pro všechny kroky, hello kroky pro protokolování mimo portál hello přeskočit a přejít hello kroky pro přiřazení jiný uživatel oprávnění toohello virtuální sítě. Úspěšné dokončení všech kroků hello, je třeba zaregistrovat pro hello preview. tooregister, dokončení hello kroky hello [hello preview registrovat](#register) tohoto článku. Nepokračujte hello zbývající kroky, dokud jsou oba odběry registrované pro hello preview.
 
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
8. V hello **vyberte** zaškrtněte, b nebo zadejte b na e-mailovou adresu toosearch. Hello seznam uživatelů, zobrazí se z hello stejné klienta Azure Active Directory jako virtuální síť hello nastavujete pro partnerský vztah hello. Klikněte na tlačítko b, když se objeví v seznamu hello.
9. Klikněte na **Uložit**.
10. Odhlaste se z portálu hello jako uživatele a přihlaste se jako b.
11. Klikněte na tlačítko **+ nový**, typ *virtuální síť* v hello **vyhledávání hello Marketplace** pole a pak klikněte na **virtuální síť** ve výsledcích hledání hello .
12. V hello **virtuální sítě** okno, které se zobrazí, vyberte **Classic** v hello **vybrat model nasazení** pole a pak klikněte na **vytvořit**.
13.   V hello vytvořit virtuální síť (klasická) pole, které se zobrazí zadejte hello následující hodnoty:

    - **Název**: *myVnetB*
    - **Adresní prostor**: *10.1.0.0/16*
    - **Název podsítě**: *výchozí*
    - **Rozsah adres podsítě**: *10.1.0.0/24*
    - **Předplatné**: Vyberte předplatné B.
    - **Skupina prostředků**: vyberte **vytvořit nový** a zadejte *myResourceGroupB*
    - **Umístění**: *východní USA*

14. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myVnetB*. Klikněte na tlačítko **myVnetB** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myVnetB** virtuální sítě.
15. V hello **myVnetB** okno, které se zobrazí, klikněte na tlačítko **vlastnosti** z hello svislé seznam možností na levé straně okna hello hello. Kopírování hello **ID prostředku**, která je použita v pozdější fázi. ID prostředku Hello je podobné toohello následující ukázka: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. Proveďte kroky 5 až 9 pro myVnetB, zadávání **uživatele** v kroku 8.
17. Odhlaste se z portálu hello jako b a přihlaste se jako uživatele.
18. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myVnetA*. Klikněte na tlačítko **myVnetA** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myVnet** virtuální sítě.
19. Klikněte na tlačítko **myVnetA**.
20. V hello **myVnetA** okno, které se zobrazí, klikněte na tlačítko **partnerských vztahů** z hello svislé seznam možností na levé straně okna hello hello.
21. V hello **myVnetA - partnerských vztahů** okno, které se zobrazily, klikněte na tlačítko **+ přidat**
22. V hello **partnerský vztah přidat** okno, které se zobrazí, zadejte, nebo vyberte hello následující možnosti a potom klikněte na tlačítko **OK**:
     - **Název**: *myVnetAToMyVnetB*
     - **Virtuální síť modelu nasazení**: vyberte **Classic**.
     - **Vím Moje ID prostředku**: Zaškrtněte toto políčko.
     - **ID prostředku**: Zadejte ID prostředku hello myVnetB z kroku 15.
     - **Povolit přístup k virtuální síti:** Ujistěte se, že **povoleno** je vybrána.
    Žádné další nastavení použitá v tomto kurzu. Přečtěte si toolearn o všech nastaveních partnerského vztahu, [spravovat virtuální sítě partnerských vztahů](virtual-network-manage-peering.md#create-a-peering).
23. Po kliknutí na **OK** v předchozím kroku hello hello **partnerský vztah přidat** okno se zavře a zobrazí hello **myVnetA - partnerských vztahů** okno znovu. Za několik sekund zobrazí se hello partnerský vztah, kterou jste vytvořili v okně hello. **Připojení** je uvedena v hello **stav partnerského vztahu** sloupec pro hello **myVnetAToMyVnetB** partnerského vztahu, můžete vytvořit. partnerský vztah Hello je nyní vytvořeno. Není žádná potřeba toopeer hello virtuální sítě (klasické) toohello virtuální síť (Resource Manager).

    Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

24. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
25. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, dokončení hello kroky hello [odstranit prostředky](#delete-portal) tohoto článku.

## <a name="cli"></a>Vytvoření partnerského vztahu - rozhraní příkazového řádku Azure

Tento kurz používá různé účty pro každé předplatné. Pokud používáte účet, který má oprávnění tooboth odběry, můžete použít hello stejný účet pro všechny kroky, přeskočit hello kroky pro protokolování mimo Azure a odstranit řádky hello skriptu, které vytvořit přiřazení role uživatele. Nahraďte UserA@azure.com a UserB@azure.com ve všech hello následující skripty s hello uživatelských jmen, kterou používáte pro uživatele a b. 

Úspěšné dokončení všech kroků hello, je třeba zaregistrovat pro hello preview. tooregister, dokončení hello kroky hello [hello preview registrovat](#register) tohoto článku. Nepokračujte hello zbývající kroky, dokud jsou oba odběry registrované pro hello preview.

1. [Nainstalujte](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello Azure CLI 1.0 toocreate hello virtuální sítě (klasické).
2. Otevřete relaci rozhraní příkazového řádku a přihlaste se jako b pomocí hello tooAzure `azure login` příkaz.
3. Spustit hello rozhraní příkazového řádku v režimu správy služby tak, že zadáte hello `azure config mode asm` příkaz.
4. Zadejte následující příkaz toocreate hello virtuální sítě (klasické) hello:
 
    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
5. Hello zbývající kroky musí dokončit pomocí prostředí bash s hello příkazového řádku Azure CLI verze 2.0.4 nebo novější [nainstalován](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), nebo pomocí hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. Klikněte na tlačítko hello **vyzkoušet** tlačítka na hello skriptů následujících, což otevře prostředí cloudu, který se přihlásí tooyour účet Azure. Možnosti na spuštění bash skripty rozhraní příkazového řádku v klientovi Windows, najdete v části [běžící ve Windows hello rozhraní příkazového řádku Azure](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
6. Zkopírujte následující skript tooa textový editor ve vašem počítači hello. Nahraďte `<SubscriptionB-Id>` s vaším ID předplatného. Pokud si nejste jisti Id předplatného, zadejte hello `az account show` příkaz. Hello hodnotu **id** v hello výstupem je ID vašeho předplatného. Zkopírujte skript hello upravit, vložte jej v relaci tooyour 2.0 rozhraní příkazového řádku a stiskněte klávesu `Enter`. 

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Pokud jste vytvořili hello virtuální sítě (klasické) v kroku 4, Azure vytvoří hello virtuální sítě v hello *výchozí sítě* skupinu prostředků.
7. Přihlaste se b mimo Azure a přihlaste se jako uživatele hello 2.0 rozhraní příkazového řádku.
8. Vytvořte skupinu prostředků a virtuální síť (Resource Manager). Kopírování hello následující skript, vložte jej v relaci tooyour rozhraní příkazového řádku a stiskněte klávesu `Enter`. 

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get hello id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions toomyVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

9. Vytvoření virtuální sítě partnerský vztah mezi hello dvě virtuální sítě vytvořené pomocí hello různé modely nasazení. Zkopírujte následující skript tooa textový editor ve vašem počítači hello. Nahraďte `<SubscriptionB-id>` s ID vašeho předplatného. Pokud si nejste jisti Id předplatného, zadejte hello `az account show` příkaz. Hello hodnotu **id** v hello výstupem je ID vašeho předplatného. Azure vytvořili hello virtuální síť (klasická) jste vytvořili v kroku 4 ve skupině prostředků s názvem *výchozí sítě*. Vložte skript hello upravit v relaci příkazového řádku a stiskněte klávesu `Enter`.

    ```azurecli-interactive
    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

10. Po provedení hello skriptu, zkontrolujte hello partnerský vztah pro virtuální síť hello (Resource Manager). Zkopírujte hello následující skript a vložte jej v relaci příkazového řádku:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    výstup ukazuje Hello **připojeno** v hello **PeeringState** sloupce.

    Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

11. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
12. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-cli) v tomto článku.

## <a name="powershell"></a>Vytvoření partnerského vztahu – prostředí PowerShell

Tento kurz používá různé účty pro každé předplatné. Pokud používáte účet, který má oprávnění tooboth odběry, můžete použít hello stejný účet pro všechny kroky, přeskočit hello kroky pro protokolování mimo Azure a odstranit řádky hello skriptu, které vytvořit přiřazení role uživatele. Nahraďte UserA@azure.com a UserB@azure.com ve všech hello následující skripty s hello uživatelských jmen, kterou používáte pro uživatele a b. 

Úspěšné dokončení všech kroků hello, je třeba zaregistrovat pro hello preview. tooregister, dokončení hello kroky hello [hello preview registrovat](#register) tohoto článku. Nepokračujte hello zbývající kroky, dokud jsou oba odběry registrované pro hello preview.

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [Azure](https://www.powershellgallery.com/packages/Azure) a [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) moduly. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Spusťte relaci prostředí PowerShell.
3. V prostředí PowerShell, přihlaste se na tooUserB předplatné jako b zadáním hello `Add-AzureAccount` příkaz.
4. toocreate virtuální sítě (klasické) v prostředí PowerShell, musíte vytvořit nový, nebo upravte stávající sítě konfigurační soubor. Zjistěte, jak příliš[exportovat, aktualizovat a import konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md). Hello soubor musí zahrnovat následující hello **VirtualNetworkSite** element pro virtuální síť hello použili v tomto kurzu:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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

5. Přihlaste se na tooUserB předplatné jako b toouse Resource Manager příkazy zadáním hello `login-azurermaccount` příkaz.
6. Přiřadit uživatele oprávnění toovirtual síť B. kopie hello následující skript tooa textový editor ve vašem počítači a nahraďte `<SubscriptionB-id>` s hello ID předplatného B. Pokud si nejste jisti hello Id předplatného, zadejte hello `Get-AzureRmSubscription` příkaz tooview ho. Hello hodnotu **Id** v hello vrátí výstupem je ID vašeho předplatného. Azure vytvořili hello virtuální síť (klasická) jste vytvořili v kroku 4 ve skupině prostředků s názvem *výchozí sítě*. skript hello tooexecute, kopie hello upravit skriptu, vložte jej v tooPowerShell a stiskněte klávesu `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Odhlaste se z Azure jako b a přihlaste se na tooUserA předplatné jako uživatele tak, že zadáte hello `login-azurermaccount` příkaz. Hello účet, ke kterému se přihlásíte pomocí musí mít hello potřebná oprávnění toocreate partnerský vztah virtuální sítě. V tématu hello [oprávnění](#permissions) tohoto článku podrobnosti.
8. Vytvoření virtuální sítě hello (Resource Manager) tak, že zkopírujete následující skript, vkládání v tooPowerShell a stisknutím klávesy hello `Enter`:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. Přiřadíte oprávnění toomyVnetA b. Kopírování hello následující skript tooa textový editor ve vašem počítači a nahraďte `<SubscriptionA-Id>` s hello ID předplatného A. Pokud si nejste jisti hello Id předplatného, zadejte hello `Get-AzureRmSubscription` příkaz tooview ho. Hello hodnotu **Id** v hello vrátí výstupem je ID vašeho předplatného. Vložte hello upravenou verzi skriptu hello v prostředí PowerShell a potom stiskněte klávesu `Enter` tooexecute ho.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Kopírování hello následující skript tooa textový editor ve vašem počítači a nahraďte `<SubscriptionB-id>` s hello ID předplatného B. toopeer myVnetA toomyVNetB, zkopírujte skript hello upravit, vložte jej v tooPowerShell a stiskněte klávesu `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Zobrazení stavu hello partnerského vztahu myVnetA tak, že zkopírujete následující skript, vložením do prostředí PowerShell a stiskněte hello `Enter`.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Stav Hello je **připojeno**. Změní příliš**připojeno** po nastavení partnerského vztahu toomyVnetA hello z myVnetB.

    Veškeré prostředky Azure, kterou vytvoříte na buď virtuální sítě je teď možné toocommunicate mezi sebou prostřednictvím jejich IP adresy. Pokud používáte výchozí Azure překlad hello virtuálních sítí, hello prostředky ve virtuálních sítích hello nejsou možné tooresolve názvy virtuálních sítí hello. Pokud chcete tooresolve názvy virtuálních sítí v partnerský vztah, musíte vytvořit vlastní server DNS. Zjistěte, jak tooset až [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

12. **Volitelné**: když vytváření virtuálních počítačů není zahrnutý v tomto kurzu, můžete vytvořit virtuální počítač v každé virtuální sítě a připojení z jedné virtuální počítač toohello jiných, toovalidate připojení.
13. **Volitelné**: toodelete hello prostředky, které vytvoříte v tomto kurzu, kroky dokončení hello [odstranit prostředky](#delete-powershell) v tomto článku.

## <a name="permissions"></a>Oprávnění

Hello účty, že používáte toocreate partnerský vztah virtuální síť musí mít hello potřebné role nebo oprávnění. Například pokud dvě virtuální sítě s názvem myVnetA a myVnetB byly partnerský vztah, musí váš účet se přiřazenou hello následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Model nasazení|Role|Oprávnění|
|---|---|---|---|
|myVnetA|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Není k dispozici|
|myVnetB|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení konkrétní oprávnění příliš[vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="delete"></a>Odstraňte prostředky
Po dokončení tohoto kurzu, můžete chtít toodelete hello prostředků, kterou jste vytvořili v kurzu hello, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků hello.

### <a name="delete-portal"></a>Portál Azure

1. Hello portálu vyhledávacího pole zadejte **myResourceGroupA**. Ve výsledcích hledání hello, klikněte na tlačítko **myResourceGroupA**.
2. Na hello **myResourceGroupA** okně klikněte na tlačítko hello **odstranit** ikonu.
3. odstranění hello tooconfirm, v hello **typ hello název skupiny prostředků** zadejte **myResourceGroupA**a potom klikněte na **odstranit**.
4. V hello **vyhledávání prostředků** pole v horní části hello hello portálu, typ *myVnetB*. Klikněte na tlačítko **myVnetB** při zobrazí ve výsledcích hledání hello. Zobrazí se okno pro hello **myVnetB** virtuální sítě.
5. V hello **myVnetB** okně klikněte na tlačítko **odstranit**.
6. tooconfirm hello odstranění, klikněte na tlačítko **Ano** v hello **virtuální sítě odstranit** pole.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

1. Přihlaste pomocí hello tooAzure 2.0 rozhraní příkazového řádku toodelete hello virtuální sítě (Resource Manager) s hello následující příkaz:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```

2. Přihlaste pomocí hello tooAzure Azure CLI 1.0 toodelete hello virtuální sítě (klasické) s hello následující příkazy:

    ```azurecli
    azure config mode asm 

    azure network vnet delete --vnet myVnetB --quiet
    ```

### <a name="delete-powershell"></a>Prostředí PowerShell

1. Na příkazovém řádku prostředí PowerShell text hello zadejte následující příkaz toodelete hello virtuální sítě (Resource Manager) hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
    ```

2. toodelete hello virtuální sítě (klasické) v prostředí PowerShell, musíte upravit existující konfigurační soubor sítě. Zjistěte, jak příliš[exportovat, aktualizovat a import konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md). Odebrání hello následující element VirtualNetworkSite pro virtuální síť hello použili v tomto kurzu:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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
