---
title: "skript toocreate aaaAutomated tooconnect aplikace webového portálu Service Manager pomocí konektoru správy služeb IT v OMS | Microsoft Docs"
description: "Vytvoření aplikace webového portálu Service Manager pomocí konektoru správy služeb IT v OMS, tooconnect automatizované skriptu a centrálně monitorovat a spravovat hello ITSM pracovní položky."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 879e819f-d880-41c8-9775-a30907e42059
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: v-jysur
ms.openlocfilehash: cbe6a1f75548ac541fd428a977edf64eea959e4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-manager-web-app-using-hello-automated-script-preview"></a><span data-ttu-id="64c03-103">Vytvoření aplikace webového portálu Service Manager pomocí skriptu hello automatizované (Preview)</span><span class="sxs-lookup"><span data-stu-id="64c03-103">Create Service Manager Web app using hello automated script (Preview)</span></span>

<span data-ttu-id="64c03-104">Použijte hello následující skript toocreate hello webové aplikace pro instanci portálu Service Manager.</span><span class="sxs-lookup"><span data-stu-id="64c03-104">Use hello following script toocreate hello Web app for your Service Manager instance.</span></span> <span data-ttu-id="64c03-105">Zde jsou další informace o připojení k portálu Service Manager: [portálu Service Manager webové aplikace](log-analytics-itsmc-connections.md#create-and-deploy-service-manager-web-app-service)</span><span class="sxs-lookup"><span data-stu-id="64c03-105">More information about Service Manager connection is here: [Service Manager Web app](log-analytics-itsmc-connections.md#create-and-deploy-service-manager-web-app-service)</span></span>

<span data-ttu-id="64c03-106">Spusťte skript hello tím, že poskytuje hello následující požadované podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="64c03-106">Run hello script by providing hello following required details:</span></span>

- <span data-ttu-id="64c03-107">Podrobnosti o předplatném Azure</span><span class="sxs-lookup"><span data-stu-id="64c03-107">Azure subscription details</span></span>
- <span data-ttu-id="64c03-108">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="64c03-108">Resource group name</span></span>
- <span data-ttu-id="64c03-109">Umístění</span><span class="sxs-lookup"><span data-stu-id="64c03-109">Location</span></span>
- <span data-ttu-id="64c03-110">Podrobnosti o serveru portálu Service Manager (název serveru, domény, uživatelské jméno a heslo)</span><span class="sxs-lookup"><span data-stu-id="64c03-110">Service Manager server details (server name,    domain, username and password)</span></span>
- <span data-ttu-id="64c03-111">Předpona názvu lokality pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="64c03-111">Site name prefix for your Web app</span></span>
- <span data-ttu-id="64c03-112">Namespace sběrnice.</span><span class="sxs-lookup"><span data-stu-id="64c03-112">ServiceBus Namespace.</span></span>

<span data-ttu-id="64c03-113">Hello skript vytvoří hello webovou aplikaci pomocí hello název, který jste zadali (spolu s několika toomake další řetězce je jedinečný).</span><span class="sxs-lookup"><span data-stu-id="64c03-113">hello script will create hello Web app using hello name that you specified (along with few additional strings toomake it unique).</span></span> <span data-ttu-id="64c03-114">Vygeneruje hello **adresa URL webové aplikace**, **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="64c03-114">It generates hello **Web app URL**, **client ID** and **client secret**.</span></span>

<span data-ttu-id="64c03-115">Uložení těchto hodnot bude nutné tyto při vytváření připojení pomocí konektoru služby správy IT.</span><span class="sxs-lookup"><span data-stu-id="64c03-115">Save these values, you will need these when you create a connection with IT Service Management Connector.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64c03-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64c03-116">Prerequisites</span></span>

 <span data-ttu-id="64c03-117">Windows Management Framework 5.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="64c03-117">Windows Management Framework 5.0 or above.</span></span>
<span data-ttu-id="64c03-118">Windows 10 má 5.1 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="64c03-118">Windows 10 has 5.1 by default.</span></span> <span data-ttu-id="64c03-119">Hello framework z si můžete stáhnout [sem](https://www.microsoft.com/download/details.aspx?id=53347):</span><span class="sxs-lookup"><span data-stu-id="64c03-119">You can download hello framework from [here](https://www.microsoft.com/download/details.aspx?id=53347):</span></span>

<span data-ttu-id="64c03-120">Použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="64c03-120">Use hello following script:</span></span>

```
####################################
# User Configuration Section Begins
####################################

# Subscription name in Azure account. Check in Azure Portal.
$azureSubscriptionName = ""

# Resource group name for resource deployment. Could be an existing resource group or a new one toobe created.
$resourceGroupName = ""

# Location for existing resource group or new resource group deployment
################################### List of available regions #################################################
# centralus,eastasia,southeastasia,eastus,eastus2,westus,westus2,northcentralus,southcentralus,westcentralus,
# northeurope,westeurope,japaneast,japanwest,brazilsouth,australiasoutheast,australiaeast,westindia,southindia,
# centralindia,canadacentral,canadaeast,uksouth,ukwest.
###############################################################################################################
$location = ""

# Service Manager Authentication Settings
$serverName = ""
$domain = ""
$username = ""
$password = ""


# Azure site Name Prefix. Default is "smoc". It can be configured tooany desired value.
$siteNamePrefix = ""

# Service Bus namespace. Please provide an already existing service bus namespace.
# If it doesn't exist, a new one will be created with name $siteName + "sbn" which can also be later reused for any other hybrid connections.
$serviceName = ""

##################################
# User Configuration Section Ends
##################################

################
# Installations
################

# Allowing hello execution of hello script for current user.  
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

Write-Host "Checking for required modules..."
if(!(Get-PackageProvider -Name NuGet))
{
   Write-Host "Installing NuGet Package Provider..."
   Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Scope CurrentUser -Force -WarningAction SilentlyContinue
}
$module = Get-Module -ListAvailable -Name AzureRM

if(!$module -or ($module[0].Version.Major -lt 4))
{
    Write-Host "Installing AzureRm Module..."  
    try
    {
        # In case of Win 10 Anniversary update
        Install-Module AzureRM -MinimumVersion 4.1.0 -Scope CurrentUser -Force -WarningAction SilentlyContinue -AllowClobber
    }
    catch
    {
        Install-Module AzureRM -MinimumVersion 4.1.0 -Scope CurrentUser -Force -WarningAction SilentlyContinue
    }

}

Write-Host "Requirement check complete!!"

#############
# Parameters
#############

$errorActionPreference = "Stop"

$templateUri = "https://raw.githubusercontent.com/SystemCenterServiceManager/SMOMSConnector/master/azuredeploy.json"

if(!$siteNamePrefix)
{
    $siteNamePrefix = "smoc"
}

Add-AzureRmAccount

$context = Set-AzureRmContext -SubscriptionName $azureSubscriptionName -WarningAction SilentlyContinue

$resourceProvider = Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web

if(!$resourceProvider -or $resourceProvider[0].RegistrationState -ne "Registered")
{
    try
    {
        Write-Host "Registering Microsoft.Web Resource Provider"
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Web
    }
    catch
    {
        Write-Host "Failed tooRegister Microsoft.Web Resource Provider. Please register it in Azure Portal."
        exit
    }   
}
do
{
    $rand = Get-Random -Maximum 32000

    $siteName = $siteNamePrefix + $rand

    $resource = Find-AzureRmResource -ResourceNameContains $siteName -ResourceType Microsoft.Web/sites

}while($resource)

$azureSite = "https://"+$siteName+".azurewebsites.net"

##############
# MAIN Begins
##############

# Web App Deployment
####################

$tenant = $context.Tenant.Id
if(!$tenant)
{
    #For backward compatibility with older versions
    $tenant = $context.Tenant.TenantId
}
try
{
    Get-AzureRmResourceGroup -Name $resourceGroupName
}
catch
{
    New-AzureRmResourceGroup -Location $location -Name $resourceGroupName
}

Write-Output "Web App Deployment in progress...."

New-AzureRmResourceGroupDeployment -TemplateUri $templateUri -siteName $siteName -ResourceGroupName $resourceGroupName

Write-Output "Web App Deployed successfully!!"

# AAD Authentication
####################

Add-Type -AssemblyName System.Web

$clientSecret = [System.Web.Security.Membership]::GeneratePassword(30,2).ToString()

try
{

    Write-Host "Creating AzureAD application..."

    $adApp = New-AzureRmADApplication -DisplayName $siteName -HomePage $azureSite -IdentifierUris $azureSite -Password $clientSecret

    Write-Host "AzureAD application created succesfully!!"
}
catch
{
    # Delete hello deployed web app if Azure AD application fails
    Remove-AzureRmResource -ResourceGroupName $resourceGroupName -ResourceName $siteName -ResourceType Microsoft.Web/sites -Force

    Write-Host "Faiure occured in Azure AD application....Try again!!"

    exit

}

$clientId = $adApp.ApplicationId

$servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $clientId

# Web App Configuration
#######################
try
{

    Write-Host "Configuring deployed Web-App..."
    $webApp = Get-AzureRMWebAppSlot -ResourceGroupName $resourceGroupName -Name $siteName -Slot production -WarningAction SilentlyContinue

    $appSettingList = $webApp.SiteConfig.AppSettings

    $appSettings = @{}
    ForEach ($item in $appSettingList) {
        $appSettings[$item.Name] = $item.Value
    }
    $appSettings['ida:Tenant'] = $tenant
    $appSettings['ida:Audience'] = $azureSite
    $appSettings['ida:ServerName'] = $serverName
    $appSettings['ida:Domain'] = $domain
    $appSettings['ida:Username'] = $userName

    $connStrings = @{}
    $kvp = @{"Type"="Custom"; "Value"=$password}
    $connStrings['ida:Password'] = $kvp

    Set-AzureRMWebAppSlot -ResourceGroupName $resourceGroupName -Name $siteName -AppSettings $appSettings -ConnectionStrings $connStrings -Slot production -WarningAction SilentlyContinue

}
catch
{
    Write-Host "Web App configuration failed. Please ensure all values are provided in Service Manager Authentication Settings in User Configuration Section"

    # Delete hello AzureRm AD Application if confiuration fails
    Remove-AzureRmADApplication -ObjectId $adApp.ObjectId -Force

    # Delete hello deployed web app if configuration fails
    Remove-AzureRmResource -ResourceGroupName $resourceGroupName -ResourceName $siteName -ResourceType Microsoft.Web/sites -Force

    exit
}


# Relay Namespace
###################

if(!$serviceName)
{
    $serviceName = $siteName + "sbn"
}

$resourceProvider = Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Relay

if(!$resourceProvider -or $resourceProvider[0].RegistrationState -ne "Registered")
{
    try
    {
        Write-Host "Registering Microsoft.Relay Resource Provider"
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Relay
    }
    catch
    {
        Write-Host "Failed tooRegister Microsoft.Relay Resource Provider. Please register it in Azure Portal."
    }   
}

$resource = Find-AzureRmResource -ResourceNameContains $serviceName -ResourceType Microsoft.Relay/namespaces

if(!$resource)
{
    $serviceName = $siteName + "sbn"
    $properties = @{
        "sku" = @{
            "name"= "Standard"
            "tier"= "Standard"
            "capacity"= 1
         }
    }
    try
    {
        Write-Host "Creating Service Bus namespace..."
        New-AzureRmResource -ResourceName $serviceName -Location $location -PropertyObject $properties -ResourceGroupName $resourceGroupName -ResourceType Microsoft.Relay/namespaces -ApiVersion 2016-07-01 -Force
    }
    catch
    {
        $err = $TRUE
        Write-Host "Creation of Service Bus Namespace failed...Please create it manually from Azure Portal.`n"
    }

}

Write-Host "Note: Please Configure Hybrid connection in hello Networking section of hello web application in Azure Portal toolink toohello on-premises system.`n"
Write-Host "App Details"
Write-Host "============"
Write-Host "App Name:"  $siteName
Write-Host "Client Id:"  $clientId
Write-Host "Client Secret:"  $clientSecret
Write-Host "URI:"  $azureSite
if(!$err)
{
    Write-Host "ServiceBus Namespace:"  $serviceName  
}

```
## <a name="next-steps"></a><span data-ttu-id="64c03-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64c03-121">Next steps</span></span>
<span data-ttu-id="64c03-122">[Konfigurace hello hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span><span class="sxs-lookup"><span data-stu-id="64c03-122">[Configure hello Hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span></span>
