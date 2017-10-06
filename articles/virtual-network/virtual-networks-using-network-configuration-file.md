---
title: "aaaConfigure virtuální síť Azure (klasický) - sítě konfigurační soubor | Microsoft Docs"
description: "Zjistěte, jak toocreate a upravovat virtuální sítě (klasické) exportováním, změna a Import konfiguračního souboru sítě."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 009108d4315b4b7146d3f1cf2436ee211d2d89ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Konfigurace virtuální sítě (klasické) pomocí konfiguračního souboru sítě
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model nasazení Resource Manager hello.

Můžete vytvořit a nakonfigurovat virtuální síť (klasická) se souborem konfigurace sítě pomocí rozhraní příkazového řádku Azure (CLI) 1.0 hello nebo Azure PowerShell. Nelze vytvořit nebo upravit virtuální sítě pomocí modelu nasazení Azure Resource Manager hello pomocí konfiguračního souboru sítě. Nelze použít hello Azure portálu toocreate nebo upravit virtuální sítě (klasické) pomocí konfiguračního souboru sítě, ale můžete použít hello Azure portálu toocreate virtuální sítě (klasické), bez použití konfiguračního souboru sítě.

Vytváření a konfigurace virtuální sítě (klasické) s konfiguračním souborem sítě vyžaduje export, změna a importování souboru hello.

## <a name="export"></a>Exportovat soubor konfigurace sítě

Můžete použít PowerShell nebo rozhraní příkazového řádku Azure tooexport konfiguračního souboru sítě hello. Prostředí PowerShell exportuje soubor XML při hello rozhraní příkazového řádku Azure exportuje soubor json.

### <a name="powershell"></a>PowerShell
 
1. [Nainstalovat Azure PowerShell a přihlaste se tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Změnit adresář, hello (a ujistěte se, existuje) a název souboru v hello následující příkaz jako požadovaný a pak spusťte hello příkaz tooexport hello sítě konfigurační soubor:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Nainstalovat Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dokončit zbývající kroky z příkazového řádku Azure CLI 1.0 hello.
2. Přihlaste se zadáním hello tooAzure `azure login` příkaz.
3. Zkontrolujte, zda pracujete v režimu asm zadáním hello `azure config mode asm` příkaz.
4. Změnit adresář, hello (a ujistěte se, existuje) a název souboru v hello následující příkaz jako požadovaný a pak spusťte hello příkaz tooexport hello sítě konfigurační soubor:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Vytvářet nebo upravovat soubor konfigurace sítě

Soubor konfigurace sítě je soubor XML (při použití prostředí PowerShell) nebo soubor json (při použití hello příkazového řádku Azure CLI). Můžete upravit soubor hello v textu nebo editoru XML nebo json. Hello [síťová nastavení schéma konfiguračního souboru](https://msdn.microsoft.com/library/azure/jj157100.aspx) článek obsahuje podrobnosti pro všechna nastavení. Další vysvětlení hello nastavení najdete v tématu [zobrazit virtuální sítě a nastavení](virtual-network-manage-network.md#view-vnet). Hello provedené změny toohello souboru:

- Musí být v souladu s hello schéma nebo import hello sítě se nezdaří konfigurační soubor.
- Přepsat všechny existující nastavení sítě pro vaše předplatné, proto buďte velmi opatrní při provádění změn. Například referenční hello příklad sítě konfigurační soubory, které podle. Řekněme hello původní soubor obsahoval dvě **VirtualNetworkSite** instance a změnil, jak je znázorněno v příkladech hello. Při importu souboru hello Azure odstraní hello virtuální sítě pro hello **VirtualNetworkSite** instance, které jste odebrali v souboru hello. Tento zjednodušený scénář předpokládá, že nebyly žádné prostředky ve virtuální síti hello, jako kdyby existovalo, hello virtuální síť se nepodařilo odstranit, a dojde k selhání hello importu.

> [!IMPORTANT]
> Azure zvažuje podsíť, která se má něco nasazené tooit jako **používá**. Pokud podsíť je používána, nemůže být upraven. Před změnou informace o podsíti v konfiguračním souboru sítě, přesuňte všechny položky, které jste nasadili toohello podsíť tooa jinou podsíť, která není upravována. V tématu [přesunutí virtuálního počítače nebo Role Instance tooa jiné podsíti](virtual-networks-move-vm-role-to-subnet.md) podrobnosti.

### <a name="example-xml-for-use-with-powershell"></a>Příklad XML pro použití v prostředí PowerShell

Hello následujícího konfiguračního souboru sítě příklad vytvoří virtuální síť s názvem *myVirtualNetwork* s adresním prostorem z *10.0.0.0/16* v hello *východní USA* Azure oblast. virtuální síť Hello obsahuje jednu podsíť s názvem *mySubnet* s předponu adresy z *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Obsahuje-li hello sítě konfigurační soubor, který jste exportovali žádný obsah, můžete zkopírovat hello XML v předchozím příkladu hello a vložte jej do nového souboru.

### <a name="example-json-for-use-with-hello-azure-cli-10"></a>Příklad JSON pro použití s hello Azure CLI 1.0

Hello následujícího konfiguračního souboru sítě příklad vytvoří virtuální síť s názvem *myVirtualNetwork* s adresním prostorem z *10.0.0.0/16* v hello *východní USA* Azure oblast. virtuální síť Hello obsahuje jednu podsíť s názvem *mySubnet* s předponu adresy z *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Obsahuje-li hello sítě konfigurační soubor, který jste exportovali žádný obsah, můžete zkopírovat hello json v předchozím příkladu hello a vložte jej do nového souboru.

## <a name="import"></a>Importovat soubor konfigurace sítě

Můžete použít PowerShell nebo rozhraní příkazového řádku Azure tooimport konfiguračního souboru sítě hello. Prostředí PowerShell importuje soubor XML při hello rozhraní příkazového řádku Azure importuje soubor json. Pokud hello import se nepovede, ověřte, že hello soubor odpovídá hello [schéma konfigurace sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Nainstalovat Azure PowerShell a přihlaste se tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Změňte adresář hello a název souboru v hello následující příkaz podle potřeby a potom spusťte hello příkaz tooimport hello sítě konfigurační soubor:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Nainstalovat Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dokončit zbývající kroky z příkazového řádku Azure CLI 1.0 hello.
2. Přihlaste se zadáním hello tooAzure `azure login` příkaz.
3. Zkontrolujte, zda pracujete v režimu asm zadáním hello `azure config mode asm` příkaz.
4. Změňte adresář hello a název souboru v hello následující příkaz podle potřeby a potom spusťte hello příkaz tooimport hello sítě konfigurační soubor:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
