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
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a><span data-ttu-id="a3ea9-103">Konfigurace virtuální sítě (klasické) pomocí konfiguračního souboru sítě</span><span class="sxs-lookup"><span data-stu-id="a3ea9-103">Configure a virtual network (classic) using a network configuration file</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a3ea9-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="a3ea9-105">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="a3ea9-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-106">Microsoft recommends that most new deployments use hello Resource Manager deployment model.</span></span>

<span data-ttu-id="a3ea9-107">Můžete vytvořit a nakonfigurovat virtuální síť (klasická) se souborem konfigurace sítě pomocí rozhraní příkazového řádku Azure (CLI) 1.0 hello nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-107">You can create and configure a virtual network (classic) with a network configuration file using hello Azure command-line interface (CLI) 1.0 or Azure PowerShell.</span></span> <span data-ttu-id="a3ea9-108">Nelze vytvořit nebo upravit virtuální sítě pomocí modelu nasazení Azure Resource Manager hello pomocí konfiguračního souboru sítě.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-108">You cannot create or modify a virtual network through hello Azure Resource Manager deployment model using a network configuration file.</span></span> <span data-ttu-id="a3ea9-109">Nelze použít hello Azure portálu toocreate nebo upravit virtuální sítě (klasické) pomocí konfiguračního souboru sítě, ale můžete použít hello Azure portálu toocreate virtuální sítě (klasické), bez použití konfiguračního souboru sítě.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-109">You cannot use hello Azure portal toocreate or modify a virtual network (classic) using a network configuration file, however you can use hello Azure portal toocreate a virtual network (classic), without using a network configuration file.</span></span>

<span data-ttu-id="a3ea9-110">Vytváření a konfigurace virtuální sítě (klasické) s konfiguračním souborem sítě vyžaduje export, změna a importování souboru hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-110">Creating and configuring a virtual network (classic) with a network configuration file requires exporting, changing, and importing hello file.</span></span>

## <span data-ttu-id="a3ea9-111"><a name="export"></a>Exportovat soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="a3ea9-111"><a name="export"></a>Export a network configuration file</span></span>

<span data-ttu-id="a3ea9-112">Můžete použít PowerShell nebo rozhraní příkazového řádku Azure tooexport konfiguračního souboru sítě hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-112">You can use PowerShell or hello Azure CLI tooexport a network configuration file.</span></span> <span data-ttu-id="a3ea9-113">Prostředí PowerShell exportuje soubor XML při hello rozhraní příkazového řádku Azure exportuje soubor json.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-113">PowerShell exports an XML file, while hello Azure CLI exports a json file.</span></span>

### <a name="powershell"></a><span data-ttu-id="a3ea9-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3ea9-114">PowerShell</span></span>
 
1. <span data-ttu-id="a3ea9-115">[Nainstalovat Azure PowerShell a přihlaste se tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-115">[Install Azure PowerShell and sign in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="a3ea9-116">Změnit adresář, hello (a ujistěte se, existuje) a název souboru v hello následující příkaz jako požadovaný a pak spusťte hello příkaz tooexport hello sítě konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="a3ea9-116">Change hello directory (and ensure it exists) and filename in hello following command as desired, then run hello command tooexport hello network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a><span data-ttu-id="a3ea9-117">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a3ea9-117">Azure CLI 1.0</span></span>

1. <span data-ttu-id="a3ea9-118">[Nainstalovat Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-118">[Install hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="a3ea9-119">Dokončit zbývající kroky z příkazového řádku Azure CLI 1.0 hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-119">Complete hello remaining steps from an Azure CLI 1.0 command prompt.</span></span>
2. <span data-ttu-id="a3ea9-120">Přihlaste se zadáním hello tooAzure `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-120">Log in tooAzure by entering hello `azure login` command.</span></span>
3. <span data-ttu-id="a3ea9-121">Zkontrolujte, zda pracujete v režimu asm zadáním hello `azure config mode asm` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-121">Ensure you're in asm mode by entering hello `azure config mode asm` command.</span></span>
4. <span data-ttu-id="a3ea9-122">Změnit adresář, hello (a ujistěte se, existuje) a název souboru v hello následující příkaz jako požadovaný a pak spusťte hello příkaz tooexport hello sítě konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="a3ea9-122">Change hello directory (and ensure it exists) and filename in hello following command as desired, then run hello command tooexport hello network configuration file:</span></span>
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a><span data-ttu-id="a3ea9-123">Vytvářet nebo upravovat soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="a3ea9-123">Create or modify a network configuration file</span></span>

<span data-ttu-id="a3ea9-124">Soubor konfigurace sítě je soubor XML (při použití prostředí PowerShell) nebo soubor json (při použití hello příkazového řádku Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-124">A network configuration file is an XML file (when using PowerShell) or a json file (when using hello Azure CLI).</span></span> <span data-ttu-id="a3ea9-125">Můžete upravit soubor hello v textu nebo editoru XML nebo json.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-125">You can edit hello file in any text, or XML/json editor.</span></span> <span data-ttu-id="a3ea9-126">Hello [síťová nastavení schéma konfiguračního souboru](https://msdn.microsoft.com/library/azure/jj157100.aspx) článek obsahuje podrobnosti pro všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-126">hello [Network configuration file schema settings](https://msdn.microsoft.com/library/azure/jj157100.aspx) article includes details for all settings.</span></span> <span data-ttu-id="a3ea9-127">Další vysvětlení hello nastavení najdete v tématu [zobrazit virtuální sítě a nastavení](virtual-network-manage-network.md#view-vnet).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-127">For additional explanation of hello settings, see [View virtual networks and settings](virtual-network-manage-network.md#view-vnet).</span></span> <span data-ttu-id="a3ea9-128">Hello provedené změny toohello souboru:</span><span class="sxs-lookup"><span data-stu-id="a3ea9-128">hello changes you make toohello file:</span></span>

- <span data-ttu-id="a3ea9-129">Musí být v souladu s hello schéma nebo import hello sítě se nezdaří konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-129">Must comply with hello schema, or importing hello network configuration file will fail.</span></span>
- <span data-ttu-id="a3ea9-130">Přepsat všechny existující nastavení sítě pro vaše předplatné, proto buďte velmi opatrní při provádění změn.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-130">Overwrite any existing network settings for your subscription, so use extreme caution when making modifications.</span></span> <span data-ttu-id="a3ea9-131">Například referenční hello příklad sítě konfigurační soubory, které podle.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-131">For example, reference hello example network configuration files that follow.</span></span> <span data-ttu-id="a3ea9-132">Řekněme hello původní soubor obsahoval dvě **VirtualNetworkSite** instance a změnil, jak je znázorněno v příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-132">Say hello original file contained two **VirtualNetworkSite** instances, and you changed it, as shown in hello examples.</span></span> <span data-ttu-id="a3ea9-133">Při importu souboru hello Azure odstraní hello virtuální sítě pro hello **VirtualNetworkSite** instance, které jste odebrali v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-133">When you import hello file, Azure deletes hello virtual network for hello **VirtualNetworkSite** instance you removed in hello file.</span></span> <span data-ttu-id="a3ea9-134">Tento zjednodušený scénář předpokládá, že nebyly žádné prostředky ve virtuální síti hello, jako kdyby existovalo, hello virtuální síť se nepodařilo odstranit, a dojde k selhání hello importu.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-134">This simplified scenario assumes no resources were in hello virtual network, as if there were, hello virtual network could not be deleted, and hello import would fail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3ea9-135">Azure zvažuje podsíť, která se má něco nasazené tooit jako **používá**.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-135">Azure considers a subnet that has something deployed tooit as **in use**.</span></span> <span data-ttu-id="a3ea9-136">Pokud podsíť je používána, nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-136">When a subnet is in use, it cannot be modified.</span></span> <span data-ttu-id="a3ea9-137">Před změnou informace o podsíti v konfiguračním souboru sítě, přesuňte všechny položky, které jste nasadili toohello podsíť tooa jinou podsíť, která není upravována.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-137">Before modifying subnet information in a network configuration file, move anything that you have deployed toohello subnet tooa different subnet that isn't being modified.</span></span> <span data-ttu-id="a3ea9-138">V tématu [přesunutí virtuálního počítače nebo Role Instance tooa jiné podsíti](virtual-networks-move-vm-role-to-subnet.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-138">See [Move a VM or Role Instance tooa Different Subnet](virtual-networks-move-vm-role-to-subnet.md) for details.</span></span>

### <a name="example-xml-for-use-with-powershell"></a><span data-ttu-id="a3ea9-139">Příklad XML pro použití v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3ea9-139">Example XML for use with PowerShell</span></span>

<span data-ttu-id="a3ea9-140">Hello následujícího konfiguračního souboru sítě příklad vytvoří virtuální síť s názvem *myVirtualNetwork* s adresním prostorem z *10.0.0.0/16* v hello *východní USA* Azure oblast.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-140">hello following example network configuration file creates a virtual network named *myVirtualNetwork* with an address space of *10.0.0.0/16* in hello *East US* Azure region.</span></span> <span data-ttu-id="a3ea9-141">virtuální síť Hello obsahuje jednu podsíť s názvem *mySubnet* s předponu adresy z *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-141">hello virtual network contains one subnet named *mySubnet* with an address prefix of *10.0.0.0/24*.</span></span>

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

<span data-ttu-id="a3ea9-142">Obsahuje-li hello sítě konfigurační soubor, který jste exportovali žádný obsah, můžete zkopírovat hello XML v předchozím příkladu hello a vložte jej do nového souboru.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-142">If hello network configuration file you exported contains no contents, you can copy hello XML in hello previous example, and paste it into a new file.</span></span>

### <a name="example-json-for-use-with-hello-azure-cli-10"></a><span data-ttu-id="a3ea9-143">Příklad JSON pro použití s hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a3ea9-143">Example JSON for use with hello Azure CLI 1.0</span></span>

<span data-ttu-id="a3ea9-144">Hello následujícího konfiguračního souboru sítě příklad vytvoří virtuální síť s názvem *myVirtualNetwork* s adresním prostorem z *10.0.0.0/16* v hello *východní USA* Azure oblast.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-144">hello following example network configuration file creates a virtual network named *myVirtualNetwork* with an address space of *10.0.0.0/16* in hello *East US* Azure region.</span></span> <span data-ttu-id="a3ea9-145">virtuální síť Hello obsahuje jednu podsíť s názvem *mySubnet* s předponu adresy z *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-145">hello virtual network contains one subnet named *mySubnet* with an address prefix of *10.0.0.0/24*.</span></span>

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

<span data-ttu-id="a3ea9-146">Obsahuje-li hello sítě konfigurační soubor, který jste exportovali žádný obsah, můžete zkopírovat hello json v předchozím příkladu hello a vložte jej do nového souboru.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-146">If hello network configuration file you exported contains no contents, you can copy hello json in hello previous example, and paste it into a new file.</span></span>

## <span data-ttu-id="a3ea9-147"><a name="import"></a>Importovat soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="a3ea9-147"><a name="import"></a>Import a network configuration file</span></span>

<span data-ttu-id="a3ea9-148">Můžete použít PowerShell nebo rozhraní příkazového řádku Azure tooimport konfiguračního souboru sítě hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-148">You can use PowerShell or hello Azure CLI tooimport a network configuration file.</span></span> <span data-ttu-id="a3ea9-149">Prostředí PowerShell importuje soubor XML při hello rozhraní příkazového řádku Azure importuje soubor json.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-149">PowerShell imports an XML file, while hello Azure CLI imports a json file.</span></span> <span data-ttu-id="a3ea9-150">Pokud hello import se nepovede, ověřte, že hello soubor odpovídá hello [schéma konfigurace sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-150">If hello import fails, confirm that hello file complies with hello [network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span> 

### <a name="powershell"></a><span data-ttu-id="a3ea9-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3ea9-151">PowerShell</span></span>
 
1. <span data-ttu-id="a3ea9-152">[Nainstalovat Azure PowerShell a přihlaste se tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-152">[Install Azure PowerShell and sign in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="a3ea9-153">Změňte adresář hello a název souboru v hello následující příkaz podle potřeby a potom spusťte hello příkaz tooimport hello sítě konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="a3ea9-153">Change hello directory and filename in hello following command as necessary, then run hello command tooimport hello network configuration file:</span></span>
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a><span data-ttu-id="a3ea9-154">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a3ea9-154">Azure CLI 1.0</span></span>

1. <span data-ttu-id="a3ea9-155">[Nainstalovat Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a3ea9-155">[Install hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="a3ea9-156">Dokončit zbývající kroky z příkazového řádku Azure CLI 1.0 hello.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-156">Complete hello remaining steps from an Azure CLI 1.0 command prompt.</span></span>
2. <span data-ttu-id="a3ea9-157">Přihlaste se zadáním hello tooAzure `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-157">Log in tooAzure by entering hello `azure login` command.</span></span>
3. <span data-ttu-id="a3ea9-158">Zkontrolujte, zda pracujete v režimu asm zadáním hello `azure config mode asm` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3ea9-158">Ensure you're in asm mode by entering hello `azure config mode asm` command.</span></span>
4. <span data-ttu-id="a3ea9-159">Změňte adresář hello a název souboru v hello následující příkaz podle potřeby a potom spusťte hello příkaz tooimport hello sítě konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="a3ea9-159">Change hello directory and filename in hello following command as necessary, then run hello command tooimport hello network configuration file:</span></span>

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
