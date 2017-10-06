---
title: "aaaCreate virtuální sítě | Šablona Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak toocreate a virtuální sítě pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9c289433ff2a84bec19eac25fa28ab40d131c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a>Vytvoření virtuální sítě pomocí šablony Azure Resource Manager

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello. Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.
 
Tento článek vysvětluje, jak toocreate virtuální síť prostřednictvím nasazení Resource Manager hello modelu pomocí šablony Azure Resource Manager. Můžete také vytvořit virtuální síť pomocí Resource Manager pomocí jiných nástrojů nebo vytvoření virtuální sítě pomocí modelu nasazení classic hello výběrem jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
- [Azure Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [Rozhraní příkazového řádku](virtual-networks-create-vnet-arm-cli.md)
- [Šablona](virtual-networks-create-vnet-arm-template-click.md)
- [Portál (Classic)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (Classic)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [Rozhraní příkazového řádku (Classic)](virtual-networks-create-vnet-classic-cli.md)

Se dozvíte, jak toodownload a upravit existující šablonu ARM z Githubu a nasazení hello šablony z Githubu, prostředí PowerShell a hello rozhraní příkazového řádku Azure.

Pokud jednoduše nasazujete šablony ARM hello přímo z Githubu, beze změn, přeskočte příliš[nasazení šablony z githubu](#deploy-the-arm-template-by-using-click-to-deploy).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-hello-azure-resource-manager-template"></a>Stažení a pochopení šablony Azure Resource Manageru hello
Můžete stáhnout existující šablonu hello pro vytvoření virtuální sítě a dvě podsítě z Githubu, proveďte požadované změny, může být vhodné a opakovaně ji používat. toodo tedy dokončit hello následující kroky:

1. Přejděte příliš[stránku hello Ukázka šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.
3. Uložte soubor tooa hello do místní složky v počítači.
4. Pokud jste obeznámeni s šablonami, přeskočte toostep 7.
5. Otevřete soubor hello jste právě uložili a prohlédněte si hello obsah v části **parametry** na řádku 5. Parametry šablony ARM představují zástupce hodnot, které můžete doplnit během nasazování.
   
   | Parametr | Popis |
   | --- | --- |
   | **location** |Oblast Azure, kde bude vytvořena hello virtuální sítě |
   | **vnetName** |Název hello nové sítě VNet |
   | **addressPrefix** |Adresní prostor sítě VNet, hello ve formátu CIDR |
   | **subnet1Name** |Název hello první sítě VNet |
   | **subnet1Prefix** |Blok CIDR pro první podsíť hello |
   | **subnet2Name** |Název hello druhé sítě VNet |
   | **subnet2Prefix** |Blok CIDR pro druhou podsíť hello |
   
   > [!IMPORTANT]
   > Šablony Azure Resource Manageru udržované na webu GitHub se můžou časem změnit. Zajistěte, aby že Zkontrolujte šablonu, hello před jeho použitím.
   > 
   > 
6. Zkontrolujte obsah hello v části **prostředky** a Všimněte si následujících hello:
   
   * **type**. Typ prostředku vytvořeného šablonou hello. V tomto případě se jedná o prostředek **Microsoft.Network/virtualNetworks**, který reprezentuje síť VNet.
   * **name**. Název prostředku hello. Všimněte si použití hello **[parameters('vnetName')]**, což znamená hello se název zadaný jako vstup hello uživatelem nebo ze souboru parametrů během nasazování.
   * **properties**. Seznam vlastností pro prostředek hello. Tato šablona používá během vytváření sítě VNet vlastnosti prostoru a podsítě adresy hello.
7. Vraťte se zpět příliš[stránku hello Ukázka šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klikněte na **azuredeploy-paremeters.json** a potom klikněte na **RAW**.
9. Uložte soubor tooa hello do místní složky v počítači.
10. Otevřete hello soubor, který jste právě uložili a upravte hello hodnoty parametrů hello. Použijte následující hodnoty menší než toodeploy hello sítě VNet popsané v hello scénáři hello:

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. Uložte soubor hello.


## <a name="deploy-hello-template-using-powershell"></a>Nasazení šablony hello pomocí prostředí PowerShell

Proveďte následující kroky toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell hello:

1. Instalace a konfigurace prostředí Azure PowerShell pomocí kroků hello v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. Spusťte následující příkaz toocreate hello novou skupinu prostředků:

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    příkaz Hello vytvoří skupinu prostředků s názvem *TestRG* v hello *střed USA* oblast azure. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

    Očekávaný výstup:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. Spuštění hello následující příkaz toodeploy hello nové sítě VNet pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozích krocích:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    Očekávaný výstup:
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. Hello spusťte následující příkaz tooview hello vlastnosti hello nové virtuální sítě:

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    Očekávaný výstup:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-hello-template-using-click-to-deploy"></a>Nasazení šablony hello pomocí, klikněte na tlačítko nasadit

Předem definované Azure Resource Manager šablony nahrané tooa úložiště GitHub spravován společností Microsoft a otevřete toohello komunity můžete znovu použít. Tyto šablony můžete nasazovat přímo z Githubu, nebo stáhli a upravili toofit vašim potřebám. toodeploy šablonu, která vytvoří síť VNet se dvěma podsítěmi, dokončení hello následující kroky:

1. V prohlížeči přejděte příliš[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Posuňte se dolů hello seznam šablon a klikněte na tlačítko **101-vnet-two-subnets**. Zkontrolujte hello **README.md** souboru, jak je uvedeno níže.

    ![Soubor READEME.md na webu GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klikněte na tlačítko **nasazení tooAzure**. V případě potřeby zadejte své přihlašovací údaje Azure. 
4. V hello **parametry** okno, zadejte hodnoty hello toouse toocreate nové sítě VNet a pak klikněte na **OK**. Hello následující obrázek znázorňuje hello hodnoty pro scénář hello:
   
    ![Parametry šablony ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. Klikněte na tlačítko **skupiny prostředků** a vyberte hello mezi virtuálními tooadd skupiny prostředků, nebo klikněte na **vytvořit nový** tooadd hello virtuální síť tooa novou skupinu prostředků. Hello následující obrázek znázorňuje hello prostředků nastavení skupiny pro novou skupinu prostředků s názvem **TestRG**:

    ![Skupina prostředků](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. V případě potřeby změňte hello **předplatné** a **umístění** nastavení pro virtuální síť.
7. Pokud nechcete, aby toosee hello virtuální síť jako dlaždici v hello **úvodní panel**, zakažte **Pin tooStartboard**.
8. Klikněte na tlačítko **právní podmínky**, přečtěte si podmínky hello a klikněte na tlačítko **koupit** tooagree. 
9. Klikněte na tlačítko **vytvořit** toocreate hello virtuální sítě.
   
    ![Dlaždice Odesílá se nasazení na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. Po dokončení nasazení hello v hello portálu Azure klikněte na **další služby**, typ *virtuální sítě* hello pole filtru, který se zobrazí, pak klikněte na virtuální sítě toosee hello virtuální sítě okno. V okně hello, klikněte na tlačítko *TestVNet*. V hello *TestVNet* okně klikněte na tlačítko **podsítě** toosee hello vytvořit podsítě, jak je znázorněno v následujícím obrázku hello:
    
     ![Vytvoření sítě VNet na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooconnect:

- Virtuální síť virtuálních počítačů (VM) tooa ve čtení hello [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-portal.md) články. Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.
- Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) článku.
- Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute. Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-arm.md) články.
