---
title: "aaaCreate virtuální sítě Azure (klasické) s několika podsítěmi | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální sítě (klasické) s několika podsítěmi v Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: cc7b9ad08d5c26dba09584762bedf614d2847032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Vytvoření virtuální sítě (klasické) s několika podsítěmi

> [!IMPORTANT]
> Azure má dva [různé modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) pro vytváření a práci s prostředky: Resource Manager a Klasický model. Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje vytvoření většina nové virtuální sítě prostřednictvím hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) modelu nasazení.

V tomto kurzu zjistěte, jak toocreate základní virtuální síť Azure (klasický) s oddělit veřejné a privátní podsítě. Můžete vytvořit prostředky Azure, jako jsou virtuální počítače a cloudové služby v podsíti. Prostředky vytvořené ve virtuální sítě (klasické) mohou komunikovat navzájem a s prostředky v jiných sítích připojených tooa virtuální sítě.

Další informace o všech [virtuální sítě](virtual-network-manage-network.md) a [podsíť](virtual-network-manage-subnet.md) nastavení.

> [!WARNING]
> Virtuální sítě (klasické) se okamžitě odstraní Azure při [předplatné je zakázané](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Bez ohledu na to, jestli existují prostředky ve virtuální síti hello se odstraní virtuální sítě (klasické). Pokud později znovu povolíte hello předplatné, je nutné znovu vytvořit prostředky, které existovaly ve virtuální síti hello.

Virtuální síť (klasická) můžete vytvořit pomocí hello [portál Azure](#portal), hello [rozhraní příkazového řádku Azure (CLI) 1.0](#azure-cli), nebo [prostředí PowerShell](#powershell).

## <a name="portal"></a>Portál

1. V internetovém prohlížeči, přejděte toohello [portál Azure](https://portal.azure.com). Přihlaste se pomocí vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud nemáte účet Azure, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Klikněte na tlačítko **+ nový** hello portálu.
3. Zadejte *virtuální síť* v hello **vyhledávání hello Marketplace** pole hello horní části hello **nový** okno, které se zobrazí.  Klikněte na tlačítko **virtuální síť** při zobrazí ve výsledcích hledání hello.
4. Vyberte **Classic** v hello **vybrat model nasazení** pole v hello **virtuální sítě** okno, které se zobrazí, pak klikněte na tlačítko **vytvořit**. 
5. Zadejte následující hodnoty na hello hello **vytvořit virtuální síť (klasická)** okna a pak klikněte na tlačítko **vytvořit**:

    |Nastavení|Hodnota|
    |---|---|
    |Name (Název)|myVnet|
    |Adresní prostor|10.0.0.0/16|
    |Název podsítě|Veřejné|
    |Rozsah adres podsítě|10.0.0.0/24|
    |Skupina prostředků|Nechte **vytvořit nový** vybrané a potom zadejte **myResourceGroup**.|
    |Předplatné a umístění|Vyberte vaše předplatné a umístění.

    Pokud jste nový tooAzure, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (nazývaná také jen tooas *oblasti*).
4. Hello portálu můžete vytvořit pouze jednu podsíť při vytváření virtuální sítě. V tomto kurzu vytvoříte druhou podsíť po vytvoření virtuální sítě hello. Můžete vytvořit prostředky přístupné z Internetu později v hello **veřejné** podsítě. Můžete také vytvořit prostředky, které nejsou přístupné z Internetu hello v hello **privátní** podsítě. toocreate hello druhou podsíť, zadejte **myVnet** v hello **vyhledávání prostředků** pole v horní části hello hello stránky. Klikněte na tlačítko **myVnet** při zobrazí ve výsledcích hledání hello.
5. Klikněte na tlačítko **podsítě** (v hello **nastavení** část) na hello **vytvořit virtuální síť (klasická)** okno, které se zobrazí.
6. Klikněte na tlačítko **+ přidat** na hello **myVnet - podsítě** okno, které se zobrazí.
7. Zadejte **privátní** pro **název** na hello **přidat podsíť** okno. Zadejte **10.0.1.0/24** pro **rozsahu adres**.  Klikněte na **OK**.
8. Na hello **myVnet - podsítě** okně uvidíte hello **veřejné** a **privátní** podsítě, které jste vytvořili.
9. **Volitelné**: Po dokončení tohoto kurzu můžete toodelete hello prostředky, které jste vytvořili, tak, aby vám zbytečně nenabíhaly poplatky za používání:
    - Klikněte na tlačítko **přehled** na hello **myVnet** okno.
    - Klikněte na tlačítko hello **odstranit** ikonu na hello **myVnet** okno.
    - tooconfirm hello odstranění, klikněte na tlačítko **Ano** v hello **virtuální sítě odstranit** pole.

## <a name="azure-cli"></a>Azure CLI

1. Můžete buď [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), nebo použijte hello rozhraní příkazového řádku v rámci hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `azure <command> --help`. 
2. V relaci příkazového řádku přihlaste se tooAzure s hello příkaz, který následuje. Pokud kliknete na tlačítko **vyzkoušet** pole hello: otevře prostředí cloudu. Tooyour předplatné, můžete přihlásit bez zadávání hello následující příkaz:

    ```azurecli-interactive
    azure login
    ```

3. hello tooensure, rozhraní příkazového řádku je v režimu správy služby, zadejte následující příkaz hello:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Vytvoření virtuální sítě s privátní podsítě:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Vytvoření veřejné podsítě v rámci virtuální sítě hello:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Projděte si hello virtuální sítě a podsítě:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **Volitelné**: můžete chtít toodelete hello prostředky, které jste vytvořili po dokončení tohoto kurzu, tak, aby vám zbytečně nenabíhaly poplatky za používání:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> I když toocreate skupiny prostředků virtuální sítě (klasické) nelze zadat pomocí hello CLI, Azure vytvoří hello virtuální sítě ve skupině prostředků s názvem *výchozí sítě*.

## <a name="powershell"></a>PowerShell

1. Nainstalujte nejnovější verzi hello prostředí PowerShell hello [Azure](https://www.powershellgallery.com/packages/Azure) modulu. Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Spusťte relaci prostředí PowerShell.
3. V prostředí PowerShell přihlásit tooAzure zadáním hello `Add-AzureAccount` příkaz.
4. Změnit hello následující cestu a název souboru, podle potřeby, exportovat pak existující konfigurační soubor sítě:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. toocreate virtuální síť s podsítí veřejné a privátní, použijte všechny textového editoru tooadd hello **VirtualNetworkSite** prvek, který následuje toohello sítě konfigurační soubor.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Úplná kontrola hello [schéma konfiguračního souboru sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Importujte konfigurační soubor hello sítě:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Import konfiguračního souboru změněné sítě může způsobit změny tooexisting virtuální sítě (klasické) v rámci vašeho předplatného. Ujistěte se, můžete přidat pouze hello předchozí virtuální sítě a změníte nebo odeberte všechny existující virtuální sítě ze svého předplatného. 

7. Projděte si hello virtuální sítě a podsítě:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **Volitelné**: můžete chtít toodelete hello prostředky, které jste vytvořili po dokončení tohoto kurzu, tak, aby vám zbytečně nenabíhaly poplatky za používání. toodelete hello virtuální sítě, dokončení kroky 4 až 6 znovu, tento čas odebírání hello **VirtualNetworkSite** elementu, které jste přidali v kroku 5.
 
> [!NOTE]
> Když toocreate skupiny prostředků virtuální sítě (klasické) nelze zadat pomocí prostředí PowerShell, Azure vytvoří hello virtuální síť ve skupině prostředků s názvem *výchozí sítě*.

---

## <a name="next-steps"></a>Další kroky

- toolearn o všechny virtuální sítě a podsítě nastavení, najdete v části [spravovat virtuální sítě](virtual-network-manage-network.md) a [spravovat podsítě virtuální sítě](virtual-network-manage-subnet.md). Máte různé možnosti pro použití virtuálními sítěmi a podsítěmi v jiné požadavky produkční prostředí toomeet.
- toofilter příchozí a odchozí přenosy podsítěmi, vytvoření a použití [skupin zabezpečení sítě](virtual-networks-nsg.md) toosubnets.
- Vytvoření [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuálního počítače a připojte ho tooan existující virtuální síť.
- tooconnect dvou virtuálních sítí v hello stejného umístění Azure, vytvořte [partnerský vztah virtuální sítě](create-peering-different-deployment-models.md) mezi virtuálními sítěmi hello. Mohou párově virtuální sítě (Resource Manager) tooa virtuální sítě (klasické), ale nemůžete vytvořit partnerský vztah mezi dvěma virtuálními sítěmi (klasické).
- Připojit pomocí hello virtuální sítě tooan do místní sítě [brány VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) okruh.
