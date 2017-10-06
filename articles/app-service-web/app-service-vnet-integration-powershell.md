---
title: "aaaConnect aplikace tooyour virtuální sítě pomocí prostředí PowerShell"
description: "Pokyny, jak fungují tooconnect tooand s virtuálními sítěmi pomocí prostředí PowerShell"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: a5c76e77-972a-431c-b14b-3611dae1631b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: ccompy
ms.openlocfilehash: c9d0fa99d02cab7b2c7211a1b2f7b7d0cd27ee8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-app-tooyour-virtual-network-by-using-powershell"></a>Připojit virtuální síť tooyour aplikace pomocí prostředí PowerShell
## <a name="overview"></a>Přehled
Ve službě Azure App Service se můžete připojit aplikace (webové, mobilní nebo rozhraní API) tooan virtuální sítě Azure (VNet) v rámci vašeho předplatného. Tato funkce je volána integrace virtuální sítě. funkce integrace virtuální sítě Hello Nezaměňovat s hello App Service Environment funkci, která vám umožní toorun instanci služby Azure App Service ve virtuální síti.

funkce integrace virtuální sítě Hello má uživatelské rozhraní (UI) v hello nového portálu, které můžete použít toointegrate s virtuálními sítěmi, které jsou nasazeny pomocí modelu nasazení classic hello nebo hello modelu nasazení Azure Resource Manager. Pokud chcete další informace o funkci hello toolearn, najdete v části [integraci aplikace s virtuální sítě Azure](web-sites-integrate-with-vnet.md).

Tento článek není o tom, jak toouse hello uživatelského rozhraní, ale spíš o tooenable integrace pomocí prostředí PowerShell. Protože hello příkazy pro každý model nasazení se liší, tento článek obsahuje oddíl pro každý model nasazení.  

Než budete pokračovat v tomto článku, ujistěte se, zda máte:

* Hello nainstalované nejnovější sadu Azure PowerShell SDK. To můžete nainstalovat s hello instalačního programu webové platformy.
* Aplikace v Azure App Service spuštěna v Standard nebo Premium SKU.

## <a name="classic-virtual-networks"></a>Klasické virtuální sítě
Tato část popisuje tři úlohy pro virtuální sítě, které používají model nasazení classic hello:

1. Připojte vaše aplikace tooa dříve existující virtuální síť, která má bránu a je nakonfigurován pro připojení point-to-site.
2. Aktualizujte informace integrace virtuální sítě pro vaši aplikaci.
3. Aplikace se odpojte od vaší virtuální sítě.

### <a name="connect-an-app-tooa-classic-vnet"></a>Připojit aplikaci tooa klasické virtuální sítě
tooconnect aplikace tooa virtuální síť, proveďte tyto kroky:

1. Deklarujte toohello webovou aplikaci, bude připojení k konkrétní virtuální síti. aplikace Hello vygeneruje certifikát, který bude mu udělená toohello virtuální sítě pro připojení point-to-site.
2. Nahrání hello webové aplikace certifikát toohello virtuální sítě a následně načíst balíček hello point-to-site VPN identifikátor URI.
3. Aktualizujte připojení virtuální sítě hello webové aplikace s balíčkem point-to-site hello identifikátor URI.

Hello první a třetí kroky jsou plně skriptovatelnou, ale druhý krok hello vyžaduje jednorázové, ruční akce prostřednictvím portálu hello nebo přístup tooperform **PUT** nebo **oprava** akce ve virtuální síti hello Koncový bod Azure Resource Manager. Požádejte podporu Azure toohave tuto funkci povolíte. Než začnete, ujistěte se, že máte klasickou virtuální síť, který má připojení point-to-site již povolené a nasazené brány. toocreate hello brány a povolit point-to-site připojení, je nutné, aby portál hello toouse jak je popsáno v [vytvoření brány VPN][createvpngateway].

Hello klasickou virtuální síť musí toobe v hello stejnému předplatnému jako App Service plánování tuto aplikaci hello blokování, který budete nástroj integrovat.

##### <a name="set-up-azure-powershell-sdk"></a>Nastavení Azure PowerShell SDK
Otevřete okno prostředí PowerShell a nastavit váš účet a předplatné Azure pomocí:

    Login-AzureRmAccount

Tento příkaz otevřete příkazový řádek tooget vaše přihlašovací údaje Azure. Po přihlášení, použijte jednu z následujících příkazů tooselect hello předplatné, které chcete toouse hello. Ujistěte se, že používáte hello předplatné, které virtuální sítě a plán služby App Service jsou v.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

nebo

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Proměnné používané v tomto článku
příkazy toosimplify se nastaví **$Configuration** proměnnou prostředí PowerShell s konkrétní konfigurací hello.

Nastavte proměnnou následujícím způsobem v prostředí PowerShell s hello následující parametry:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

umístění aplikace Hello by měl být hello umístění bez žádné mezery. Westus je například západní USA.

    $Configuration.WebAppLocation = "[Your web app Location]"

Další položky Hello je zápis certifikátu hello. By mělo být zapisovatelnou cestu v místním počítači. Ujistěte se, že .cer tooinclude na konci hello.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

toosee co nastavíte, typ **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Hello zbývající část tohoto oddílu předpokládá, že máte proměnné vytvořené jako právě popsané.

##### <a name="declare-hello-virtual-network-toohello-app"></a>Deklarovat hello virtuální sítě toohello aplikace
Hello použijte následující příkaz tootell hello aplikaci, bude pomocí této konkrétní virtuální sítě. To způsobí, že toogenerate aplikace hello potřebné certifikáty:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Pokud tento příkaz úspěšné, **$vnet** by měl mít **vlastnosti** proměnné v ní. Hello **vlastnosti** proměnná by měl obsahovat obě kryptografický otisk a hello certifikát data certifikátu.

##### <a name="upload-hello-web-app-certificate-toohello-virtual-network"></a>Nahrát hello webové aplikace certifikát toohello virtuální sítě
Ruční, jednorázové krok je vyžadován pro každé předplatné a kombinace virtuální sítě. To znamená pokud se připojujete aplikace v předplatném A tooVirtual sítě A, budete potřebovat toodo tento krok jenom po bez ohledu na to, kolik aplikací je nakonfigurovat. Pokud přidáváte nové virtuální sítě tooanother aplikace, budete potřebovat toodo znovu. Hello důvodem je, že sadu certifikáty se generuje na úrovni předplatného v Azure App Service a sadu hello se vygeneruje jednou pro každou virtuální síť, která hello aplikace se budou připojovat k.

Hello certifikáty budou již byly nastaveny Pokud jste postupovali podle těchto kroků, nebo pokud je spojen s hello stejné virtuální sítě pomocí portálu hello.

prvním krokem Hello je soubor .cer toogenerate hello. druhým krokem Hello je tooupload hello .cer souboru tooyour virtuální sítě. soubor .cer hello toogenerate z volání hello rozhraní API v hello z předchozích kroků, spusťte následující příkazy hello.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

certifikát Hello budou nacházet v umístění hello který **$Configuration.GeneratedCertificatePath** určuje.

certifikát hello tooupload ručně, použijte hello [portál Azure] [ azureportal] a **procházet virtuální sítě (klasické)** > **připojení k síti VPN**  >  **Point-to-site** > **spravovat certifikáty**. Tady odeslání vašeho certifikátu.

##### <a name="get-hello-point-to-site-package"></a>Načtení balíčku pro hello point-to-site
dalším krokem Hello k nastavení připojení virtuální sítě ve webové aplikaci je tooget hello point-to-site balíček a poskytnout ho tooyour webové aplikace.

Uložte následující soubor šablony tooa názvem GetNetworkPackageUri.json někde v počítači, například C:\Azure\Templates\GetNetworkPackageUri.json hello.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Nastavte vstupní parametry:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Volání hello skriptu:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Proměnná Hello **$output. Outputs.packageUri** bude teď obsahovat hello balíček URI toobe zadané tooyour webové aplikace.

##### <a name="upload-hello-point-to-site-package-tooyour-app"></a>Nahrání aplikace tooyour hello balíček point-to-site
posledním krokem Hello je tooprovide hello aplikace s tímto balíčkem. Jednoduše spusťte další příkaz hello:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Pokud se zobrazí se dotaz tooconfirm přepisování existující prostředek, ujistěte se, že tooallow ho.

Po úspěšném provedení tohoto příkazu, vaše aplikace by teď měly být připojené toohello virtuální sítě. tooconfirm úspěch, přejděte tooyour aplikaci konzoly, zadejte následující hello:

    SET WEBSITE_

Pokud je proměnná prostředí s názvem WEBSITE_VNETNAME, který má hodnotu, která odpovídá názvu hello hello cílový virtuální sítě, všechny konfigurace proběhlo úspěšně.

### <a name="update-classic-vnet-integration-information"></a>Aktualizovat informace integrace klasické virtuální sítě
tooupdate nebo nové synchronizace vaše informace, jednoduše zopakujte hello kroky, které jste udělali, když vytvoříte hello integrace na prvním místě hello. Tyto kroky jsou:

1. Zadejte informace o konfiguraci.
2. Deklarujte aplikace toohello hello virtuální sítě.
3. Načtení balíčku pro hello point-to-site.
4. Nahrání aplikace tooyour hello balíček point-to-site.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Odpojit aplikace od klasické virtuální sítě
toodisconnect hello aplikace, musíte hello informace o konfiguraci, které bylo nastaveno během integrace virtuální sítě. Tyto informace používat, je pak jeden příkaz toodisconnect aplikace z vaší virtuální sítě.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Virtuální sítě Resource Manager
Virtuální sítě Resource Manager mají rozhraní API Správce Azure Resource Manager, které zjednodušují některé procesy ve srovnání s klasické virtuální sítě. Máme skript, který vám pomůžou dokončit hello následující úlohy:

* Vytvoření virtuální sítě Resource Manager a integrovat vaši aplikaci.
* Vytvořit bránu, nakonfigurujte připojení point-to-site v dříve existující virtuální sítě Resource Manager a pak zajistit integraci aplikace.
* Integrate dříve existující virtuální sítě Resource Manager, která má brány a point-to-site povoleno připojení k vaší aplikace.
* Aplikace se odpojte od vaší virtuální sítě.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Správce prostředků virtuální sítě App Service integrace skriptu
Zkopírujte následující skript a uložte ho tooa soubor hello. Pokud nechcete, aby toouse hello skriptu, myslíte, že volné toolearn z něj toosee jak tooset věcí až s virtuální sítí Resource Manager.

    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate toothis VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up tooan hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with hello following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create hello virtual network and add it here. hello way this works is:
        # 1) Add hello VNET association toohello App. This allows hello App toogenerate certificates, etc. for hello VNET.
        # 2) Create hello VNET and VNET gateway, add hello certificates, create hello public IP, etc., required for hello gateway
        # 3) Get hello VPN package from hello gateway and pass it back toohello App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association tooVNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, hello gateway should be able toobe joined tooan App, but may require some minor tweaking. We will declare toohello App now toouse this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET toointegrate with" $vnets $vnetNames

        # We need toocheck if this VNET is able toobe joined tooa App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have hello right certificate, we will need tooadd it.
                # If it doesn't have a point-to-site range, we will need tooadd it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need toocreate one.
            Write-Host "This Virtual Network has no gateway. I will need toocreate one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in hello address space $($vnet.AddressSpace.AddressPrefixes), with hello following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with hello following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need toocreate one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of hello Vpn type. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need toocheck if hello certificate here exists in hello gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting hello VPN package and giving it toohello App
        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected tooa VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound toothis account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter hello Resource Group of your App"

    $appName = Read-Host "Please enter hello Name of your App"

    $options = @("Add a NEW Virtual Network tooan App", "Add an EXISTING Virtual Network tooan App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want toodo?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Uložení kopie hello skriptu. V tomto článku se označuje jako V2VnetAllinOne.ps1, ale můžete použít jiný název. Neexistují žádné argumenty pro tento skript. Můžete jednoduše spusťte ji. Hello nejprve thing hello skript provede je výzva toosign v. Po přihlášení, skript hello získá informace o vašem účtu a vrátí seznam předplatných. Není počítání hello žádost o zadání přihlašovacích údajů, provedení počáteční skriptu hello vypadá takto:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Ukázkový předplatné (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) Test MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Předplatné fialové ukázku (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Vyberte možnost: 3

    Účet: ccompy@microsoft.com prostředí: AzureCloud předplatné: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klienta: 722278f-fef1-499f-91ab-2323d011db47

    Zadejte prosím skupinu prostředků aplikace hello: hcdemo-rg, zadejte název vaší aplikace hello: v2vnetpowershell, co můžete dělat má toodo?

    1) Přidat nové virtuální sítě tooan aplikace
    2) Přidat existující virtuální síť tooan aplikace
    3) Odebrat virtuální sítě z aplikace.

Hello zbytek Tato část vysvětluje, každá z těchto tří možností.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Vytvoření virtuální sítě Resource Manager a integrovat s ním
Vyberte nové virtuální sítě, že používá hello modelu nasazení Resource Manager a integraci s vaší aplikací, toocreate **1) přidat nové virtuální sítě tooan aplikace**. Toto zobrazí výzvu k zadání názvu hello hello virtuální sítě. V mé případ jak můžete vidět v hello následující nastavení, byl použit název hello, v2pshell.

Hello skript obsahuje hello údaje o hello virtuální síť, která je právě vytvářena. Pokud chcete, můžete změnit všechny hodnoty hello Při provádění tohoto příkladu vytvořené virtuální síti, která má hello následující nastavení:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Pokud chcete, toochange hello hodnot, zadejte **Y** a proveďte změny hello. Pokud jste s nastavení virtuální sítě hello spokojeni, zadejte **N** nebo jednoduše stiskněte klávesu Enter při zobrazení výzvy týkající se změna nastavení hello. Z tohoto místa na až do dokončení skriptu hello zjistíte některé co IT oddělení "i's provádění, dokud není zahájena brány virtuální sítě toocreate hello. Tento krok může trvat až hodinu tooan. V průběhu této fáze neexistuje žádné indikátor průběhu, ale skript hello vám umožní vědět, kdy byla vytvořena hello brány.

Po dokončení skriptu hello se dozvíte **dokončeno**. V tomto okamžiku budete mít virtuální sítě Resource Manager, která má název hello a nastavení, které jste vybrali. Toto nové virtuální sítě se také integrovat s vaší aplikací.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integrace aplikace s dříve existující virtuální sítě Resource Manageru
Pokud se integrace s dříve existující virtuální sítě, pokud jste zadali virtuální sítě Resource Manager, která nemá brána nebo připojení point-to-site, hello skript bude tomuto nastavení. Pokud hello virtuální síť už má tyto věci nastavit, přejde hello skriptu přímých toohello integrace aplikací. toostart tento proces, jednoduše vyberte **2) přidejte existující virtuální síť tooan aplikace**.

Tato možnost funguje jenom v případě, že máte dříve existující virtuální sítě Resource Manager, který je v hello stejnému předplatnému jako aplikace. Když vyberete možnost hello, zobrazí se seznam virtuálních sítí Resource Manager.   

    Select a VNET toointegrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Vyberte možnost: 5

Stačí vyberte hello virtuální síť, kterou chcete toointegrate s. Pokud již máte bránu, která má povolené připojení point-to-site, skript hello jednoduše integraci aplikace s vaší virtuální sítě. Pokud jste bránu, budete potřebovat podsíť brány toospecify hello. Podsíť brány musí být v adresní prostor vaší virtuální sítě a nemůže být v jiné podsíti. Pokud máte virtuální síť bez brány a spusťte tento krok, věcí vypadat takto:

    This Virtual Network has no gateway. I will need toocreate one.
    Your VNET is in hello address space 172.16.0.0/16, with hello following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

V tomto příkladu vytvoříme bránu virtuální sítě, který má hello následující nastavení:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association tooVNET

Pokud chcete, toochange některé z těchto nastavení, můžete tak učinit. Jinak, stiskněte klávesu Enter a skript hello vytvořte bránu a připojení virtuální sítě tooyour aplikace. čas vytvoření brány Hello je stále hodinu, přestože, proto se ujistěte, že byste, mít na paměti. Po dokončení všechno, co se dozvíte hello skriptu **dokončeno**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Odpojit vaší aplikace z virtuální sítě Resource Manageru
Aplikace se odpojuje od vaší virtuální sítě vypnout hello brány nebo zakázat připojení point-to-site. Může po všech používáte jej na něco jiného. Je také neodpojí ji z jiných aplikací než hello jeden, které jste zadali. tooperform tuto akci, vyberte **3) odebrat virtuální sítě z aplikace**. Pokud tak učiníte, zobrazí se přibližně toto:

    Currently connected tooVNET v2pshell

    Confirm
    Are you sure you want toodelete hello following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

I když hello skriptu uvádí odstranit, ale neodstraníte hello virtuální sítě. Právě odebírá hello integrace. Jakmile potvrdíte, že toto je co chcete toodo, příkaz hello zpracování poměrně rychle a dozvíte **True** když se provádí.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
