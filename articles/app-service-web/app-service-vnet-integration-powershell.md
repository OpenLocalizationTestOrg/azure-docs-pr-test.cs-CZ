---
title: "Připojení aplikace k virtuální síti pomocí PowerShellu"
description: "Pokyny o tom, jak připojit a pracovat s virtuálními sítěmi pomocí prostředí PowerShell"
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
ms.openlocfilehash: 6fae6a6c162fa326161d2b47a259b3151d6e3dd0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Připojení aplikace k virtuální síti pomocí PowerShellu
## <a name="overview"></a>Přehled
Ve službě Azure App Service můžete připojit aplikace (webové, mobilní nebo rozhraní API) pro virtuální sítě Azure (VNet) v rámci vašeho předplatného. Tato funkce je volána integrace virtuální sítě. Funkce integrace virtuální sítě Nezaměňovat s funkci App Service Environment, která umožňuje spustit instanci služby Azure App Service ve virtuální síti.

Funkce integrace virtuální sítě má uživatelské rozhraní (UI) v nové portálu, který můžete použít k integraci s virtuální sítě, které jsou nasazeny pomocí modelu nasazení classic nebo modelu nasazení Azure Resource Manager. Pokud chcete získat další informace o funkci, přečtěte si téma [integraci aplikace s virtuální sítě Azure](web-sites-integrate-with-vnet.md).

Tento článek ne tom, jak pomocí uživatelského rozhraní, ale spíš o tom, jak povolit integraci pomocí prostředí PowerShell. Protože příkazy pro každý model nasazení se liší, tento článek obsahuje oddíl pro každý model nasazení.  

Než budete pokračovat v tomto článku, ujistěte se, zda máte:

* Nejnovější nainstalovat Azure PowerShell SDK. To můžete nainstalovat pomocí instalačního programu webové platformy.
* Aplikace v Azure App Service spuštěna v Standard nebo Premium SKU.

## <a name="classic-virtual-networks"></a>Klasické virtuální sítě
Tato část popisuje tři úlohy pro virtuální sítě, které používají model nasazení classic:

1. Připojení aplikace k dříve existující virtuální síť, která má bránu a je nakonfigurován pro připojení point-to-site.
2. Aktualizujte informace integrace virtuální sítě pro vaši aplikaci.
3. Aplikace se odpojte od vaší virtuální sítě.

### <a name="connect-an-app-to-a-classic-vnet"></a>Připojení aplikace k klasické virtuální sítě
Chcete-li aplikaci připojit k virtuální síti, postupujte tyto tři kroky:

1. Do webové aplikace deklarujte, že se připojení k virtuální síti konkrétní. Aplikace vygeneruje certifikát, který bude mu udělená do virtuální sítě pro připojení point-to-site.
2. Nahrajte certifikát webové aplikace do virtuální sítě a následně načíst balíček point-to-site VPN identifikátor URI.
3. Aktualizujte připojení virtuální sítě do webové aplikace s tímto balíčkem point-to-site identifikátor URI.

První a třetí kroky jsou plně skriptovatelnou, ale druhý krok vyžaduje jednorázové, ruční akce prostřednictvím portálu nebo přístup k provedení **PUT** nebo **oprava** akce v koncovém bodě virtuální sítě Azure Resource Manager. Kontaktujte podporu Azure tak, aby měl tuto funkci povolíte. Než začnete, ujistěte se, že máte klasickou virtuální síť, který má připojení point-to-site již povolené a nasazené brány. K vytvoření brány a povolíte připojení point-to-site, budete muset použít na portálu, jak je popsáno v [vytvoření brány VPN][createvpngateway].

Klasickou virtuální síť musí být ve stejném předplatném jako plán služby App Service, který obsahuje aplikace, budete nástroj integrovat.

##### <a name="set-up-azure-powershell-sdk"></a>Nastavení Azure PowerShell SDK
Otevřete okno prostředí PowerShell a nastavit váš účet a předplatné Azure pomocí:

    Login-AzureRmAccount

Tento příkaz se otevře okno s výzvou k získání přihlašovacích údajů Azure. Po přihlášení, použijte některý z následujících příkazů a vyberte předplatné, které chcete použít. Ujistěte se, že používáte předplatné, které virtuální sítě a plán služby App Service jsou v.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

nebo

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Proměnné používané v tomto článku
Pro zjednodušení příkazy, se nastaví **$Configuration** proměnnou prostředí PowerShell s konkrétní konfigurací.

Nastavte proměnnou následujícím způsobem v prostředí PowerShell s následujícími parametry:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Umístění aplikace musí být umístění bez žádné mezery. Westus je například západní USA.

    $Configuration.WebAppLocation = "[Your web app Location]"

Na další položku je zápis certifikátu. By mělo být zapisovatelnou cestu v místním počítači. Nezapomeňte zahrnout .cer na konci.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Pokud chcete zobrazit, co nastavíte, zadejte **$Configuration**.

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

Zbývající část tohoto oddílu předpokládá, že máte proměnné vytvořené jako právě popsané.

##### <a name="declare-the-virtual-network-to-the-app"></a>Deklarovat virtuální sítě do aplikace
Použijte následující příkaz aplikace říct, že bude pomocí této konkrétní virtuální sítě. To způsobí, že aplikace ke generování potřebné certifikáty:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Pokud tento příkaz úspěšné, **$vnet** by měl mít **vlastnosti** proměnné v ní. **Vlastnosti** proměnná musí obsahovat kryptografický otisk certifikátu a data certifikátu.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Nahrajte certifikát webové aplikace do virtuální sítě.
Ruční, jednorázové krok je vyžadován pro každé předplatné a kombinace virtuální sítě. To znamená pokud se připojujete aplikací v rámci předplatného A do virtuální sítě A, musíte provést tento krok jenom po bez ohledu na to, kolik aplikací je nakonfigurovat. Chcete-li přidat novou aplikaci s jinou virtuální sítí, budete muset udělat to znova. Důvodem je, že sadu certifikáty se generuje na úrovni předplatného v Azure App Service a sada je generována jednou pro každou virtuální síť, která aplikace se budou připojovat k.

Certifikáty budou mít již byla nastavena Pokud jste postupovali podle těchto kroků, nebo pokud je spojen s stejné virtuální síti pomocí portálu.

Prvním krokem je generovat soubor .cer. Druhým krokem je pro nahrání souboru .cer k virtuální síti. Vygenerujte soubor .cer z volání rozhraní API v předchozím kroku, spusťte následující příkazy.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Certifikát se budou nacházet v umístění, **$Configuration.GeneratedCertificatePath** určuje.

Chcete-li nahrát na server certifikát ručně, použijte [portál Azure] [ azureportal] a **procházet virtuální sítě (klasické)** > **připojení k síti VPN** > **Point-to-site** > **spravovat certifikáty**. Tady odeslání vašeho certifikátu.

##### <a name="get-the-point-to-site-package"></a>Načtení balíčku point-to-site
Dalším krokem při nastavování připojení virtuální sítě ve webové aplikaci je získat balíček point-to-site a poskytnout ho do vaší webové aplikace.

Uložte následující šablonu do souboru s názvem GetNetworkPackageUri.json někde v počítači, například C:\Azure\Templates\GetNetworkPackageUri.json.

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

Volání skript:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Proměnná **$output. Outputs.packageUri** bude teď obsahovat balíček URI má být poskytnut do vaší webové aplikace.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Nahrání balíčku point-to-site do vaší aplikace
V posledním kroku je zajistit aplikace s tímto balíčkem. Jednoduše spusťte následující příkaz:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Pokud se výzva k potvrzení, že se přepsal existující prostředek, zajistěte, aby tak, aby ji.

Po úspěšném provedení tohoto příkazu, aplikace by teď připojený k virtuální síti. Pokud chcete ověřit úspěch, přejděte do konzoly aplikace a zadejte následující příkaz:

    SET WEBSITE_

Pokud je proměnná prostředí s názvem WEBSITE_VNETNAME, který má hodnotu, která odpovídá názvu cílové virtuální síti, všechny konfigurace proběhlo úspěšně.

### <a name="update-classic-vnet-integration-information"></a>Aktualizovat informace integrace klasické virtuální sítě
Aktualizace nebo nové synchronizace vaše informace, jednoduše zopakujte kroky, které jste udělali, když vytvoříte integraci na prvním místě. Tyto kroky jsou:

1. Zadejte informace o konfiguraci.
2. Deklarujte virtuální sítě do aplikace.
3. Načtení balíčku point-to-site.
4. Nahrání balíčku point-to-site do vaší aplikace.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Odpojit aplikace od klasické virtuální sítě
Chcete-li odpojit aplikace, musíte informace o konfiguraci, které bylo nastaveno během integrace virtuální sítě. Tyto informace používat, je pak jeden příkaz k odpojení vaší aplikace z vaší virtuální sítě.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Virtuální sítě Resource Manager
Virtuální sítě Resource Manager mají rozhraní API Správce Azure Resource Manager, které zjednodušují některé procesy ve srovnání s klasické virtuální sítě. Máme skript, který vám pomůže dokončit následující úlohy:

* Vytvoření virtuální sítě Resource Manager a integrovat vaši aplikaci.
* Vytvořit bránu, nakonfigurujte připojení point-to-site v dříve existující virtuální sítě Resource Manager a pak zajistit integraci aplikace.
* Integrate dříve existující virtuální sítě Resource Manager, která má brány a point-to-site povoleno připojení k vaší aplikace.
* Aplikace se odpojte od vaší virtuální sítě.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Správce prostředků virtuální sítě App Service integrace skriptu
Zkopírujte následující skript a uložte ho do souboru. Pokud nechcete použít skript, klidně si další informace z něj chcete zjistit, jak nastavit věcí s virtuální sítí Resource Manager.

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

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
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
            Write-Host "Currently, I will create a VNET with the following settings:"
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
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

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

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at the start and the end of the URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
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
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

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

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
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
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
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

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at the start and the end of the URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put the VPN client configuration package onto the App
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
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
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

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

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

Uložte kopii tohoto skriptu. V tomto článku se označuje jako V2VnetAllinOne.ps1, ale můžete použít jiný název. Neexistují žádné argumenty pro tento skript. Můžete jednoduše spusťte ji. První věcí, kterou skript provede je výzva, abyste se přihlásili. Po přihlášení, skript získá informace o vašem účtu a vrátí seznam předplatných. Není počítání žádost o zadání přihlašovacích údajů, provádění skriptu počáteční vypadá takto:

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

    Zadejte prosím skupinu prostředků vaší aplikace: hcdemo-rg, zadejte název vaší aplikace: v2vnetpowershell co chcete udělat?

    1) Přidejte nové virtuální sítě do aplikace
    2) Přidejte existující virtuální sítě do aplikace
    3) Odebrat virtuální sítě z aplikace.

Zbývající část Tato část vysvětluje, každá z těchto tří možností.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Vytvoření virtuální sítě Resource Manager a integrovat s ním
Chcete-li vytvořit novou virtuální síť, která používá model nasazení Resource Manager a integrovat s vaší aplikací, vyberte **1) přidejte nové virtuální sítě do aplikace**. To vás vyzve k zadání názvu virtuální sítě. V mé případu jak můžete vidět v následujícím nastavení, byl použit název v2pshell.

Skript poskytuje podrobnosti o virtuální síť, která je právě vytvářena. Pokud chcete, můžete změnit všechny hodnoty Při provádění tohoto příkladu vytvořené virtuální síti, která má následující nastavení:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Pokud chcete změnit hodnoty, zadejte **Y** a proveďte požadované změny. Pokud jste radostí s nastavení virtuální sítě, zadejte **N** nebo jednoduše stiskněte klávesu Enter při zobrazení výzvy týkající se změna nastavení. Z tohoto místa na až do dokončení skriptu, která vám sdělí některé co IT oddělení "i's provádění, dokud není zahájena vytvořit bránu virtuální sítě. Tento krok může trvat až jednu hodinu. V průběhu této fáze neexistuje žádné indikátor průběhu, ale skript vám umožní vědět, kdy byla vytvořena brány.

Po dokončení skriptu se dozvíte **dokončeno**. V tuto chvíli máte virtuální sítě Resource Manager, která má název a nastavení, které jste vybrali. Toto nové virtuální sítě se také integrovat s vaší aplikací.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integrace aplikace s dříve existující virtuální sítě Resource Manageru
Pokud se integrace s dříve existující virtuální sítě, pokud jste zadali virtuální sítě Resource Manager, která nemá brána nebo připojení point-to-site, skript bude tomuto nastavení. Pokud síť VNET už má tyto věci nastavit, skript přejde přímo na integraci aplikací. Tento proces zahájíte jednoduše vybrat **2) přidejte existující virtuální síť do aplikace**.

Tato možnost funguje jenom v případě, že máte dříve existující virtuální síť Resource Manager, který je ve stejném předplatném jako aplikace. Jakmile vyberete možnost, zobrazí se seznam virtuálních sítí Resource Manager.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Vyberte možnost: 5

Jednoduše vyberte virtuální síť, který chcete integrovat. Pokud již máte bránu, která má povolené připojení point-to-site, skript aplikace jednoduše integrovat s vaší virtuální sítě. Pokud jste bránu, budete muset zadat podsíť brány. Podsíť brány musí být v adresní prostor vaší virtuální sítě a nemůže být v jiné podsíti. Pokud máte virtuální síť bez brány a spusťte tento krok, věcí vypadat takto:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

V tomto příkladu vytvoříme bránu virtuální sítě, který má následující nastavení:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Pokud chcete některé z těchto nastavení změnit, můžete tak učinit. Jinak, stiskněte klávesu Enter a skript vytvoří brány a připojení aplikace k virtuální síti. Čas vytvoření brány je stále hodinu, přestože, proto se ujistěte, že byste, mít na paměti. Po dokončení všechno, co se dozvíte skript **dokončeno**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Odpojit vaší aplikace z virtuální sítě Resource Manageru
Aplikace se odpojuje od vaší virtuální sítě vypnout bránu nebo zakázat připojení point-to-site. Může po všech používáte jej na něco jiného. Je také neodpojí ji z jiných aplikací než ten, který jste zadali. K provedení této akce, vyberte **3) odebrat virtuální sítě z aplikace**. Pokud tak učiníte, zobrazí se přibližně toto:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

I když skript uvádí odstranit, ale neodstraníte virtuální sítě. Právě odebírá integrace. Jakmile potvrdíte, že toto je co chcete udělat, příkaz je zpracován poměrně rychle a zjistíte **True** když se provádí.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
