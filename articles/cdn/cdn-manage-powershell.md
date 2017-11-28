---
title: "aaaManage CDN Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toouse hello toomanage rutin prostředí Azure PowerShell Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 269373136d4ef018e4d31f147456b4be2253b463
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-cdn-with-powershell"></a><span data-ttu-id="a3532-103">Správa Azure CDN pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3532-103">Manage Azure CDN with PowerShell</span></span>
<span data-ttu-id="a3532-104">PowerShell poskytuje jednu z metod toomanage nejpružnější hello koncové body a profilů Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="a3532-104">PowerShell provides one of hello most flexible methods toomanage your Azure CDN profiles and endpoints.</span></span>  <span data-ttu-id="a3532-105">Prostředí PowerShell můžete použít interaktivní nebo psaní skriptů tooautomate úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="a3532-105">You can use PowerShell interactively or by writing scripts tooautomate management tasks.</span></span>  <span data-ttu-id="a3532-106">Tento kurz představuje některé z nejčastějších úloh, hello můžete provést pomocí prostředí PowerShell toomanage koncové body a profilů Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="a3532-106">This tutorial demonstrates several of hello most common tasks you can accomplish with PowerShell toomanage your Azure CDN profiles and endpoints.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3532-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a3532-107">Prerequisites</span></span>
<span data-ttu-id="a3532-108">prostředí PowerShell toomanage toouse profilů Azure CDN a koncových bodů, musíte mít nainstalován modul Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="a3532-108">toouse PowerShell toomanage your Azure CDN profiles and endpoints, you must have hello Azure PowerShell module installed.</span></span>  <span data-ttu-id="a3532-109">toolearn jak tooinstall prostředí Azure PowerShell a připojte se pomocí hello tooAzure `Login-AzureRmAccount` rutiny, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a3532-109">toolearn how tooinstall Azure PowerShell and connect tooAzure using hello `Login-AzureRmAccount` cmdlet, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3532-110">Musíte se přihlásit s `Login-AzureRmAccount` před spuštěním rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a3532-110">You must log in with `Login-AzureRmAccount` before you can execute Azure PowerShell cmdlets.</span></span>
> 
> 

## <a name="listing-hello-azure-cdn-cmdlets"></a><span data-ttu-id="a3532-111">Seznam rutin Azure CDN hello</span><span class="sxs-lookup"><span data-stu-id="a3532-111">Listing hello Azure CDN cmdlets</span></span>
<span data-ttu-id="a3532-112">Můžete vytvořit seznam všech rutin Azure CDN hello pomocí hello `Get-Command` rutiny.</span><span class="sxs-lookup"><span data-stu-id="a3532-112">You can list all hello Azure CDN cmdlets using hello `Get-Command` cmdlet.</span></span>

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a><span data-ttu-id="a3532-113">Získání nápovědy</span><span class="sxs-lookup"><span data-stu-id="a3532-113">Getting help</span></span>
<span data-ttu-id="a3532-114">Můžete získat pomoc s žádným z těchto rutin pomocí hello `Get-Help` rutiny.</span><span class="sxs-lookup"><span data-stu-id="a3532-114">You can get help with any of these cmdlets using hello `Get-Help` cmdlet.</span></span>  <span data-ttu-id="a3532-115">`Get-Help`poskytuje syntaxi a použití a volitelně zobrazuje příklady.</span><span class="sxs-lookup"><span data-stu-id="a3532-115">`Get-Help` provides usage and syntax, and optionally shows examples.</span></span>

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    toosee hello examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a><span data-ttu-id="a3532-116">Výpis existující profilů Azure CDN</span><span class="sxs-lookup"><span data-stu-id="a3532-116">Listing existing Azure CDN profiles</span></span>
<span data-ttu-id="a3532-117">Hello `Get-AzureRmCdnProfile` rutina bez parametrů načte všechny vaše stávající profily CDN.</span><span class="sxs-lookup"><span data-stu-id="a3532-117">hello `Get-AzureRmCdnProfile` cmdlet without any parameters retrieves all your existing CDN profiles.</span></span>

```powershell
Get-AzureRmCdnProfile
```

<span data-ttu-id="a3532-118">Vytvoření kanálu toocmdlets pro výčet může být tento výstup.</span><span class="sxs-lookup"><span data-stu-id="a3532-118">This output can be piped toocmdlets for enumeration.</span></span>

```powershell
# Output hello name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

<span data-ttu-id="a3532-119">Můžete se taky vrátit jediného profilu tak, že zadáte název a prostředek skupiny profilu hello.</span><span class="sxs-lookup"><span data-stu-id="a3532-119">You can also return a single profile by specifying hello profile name and resource group.</span></span>

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> <span data-ttu-id="a3532-120">Je možné toohave CDN více profilů s hello stejný název, tak dlouho, dokud jsou v různých skupinách prostředků.</span><span class="sxs-lookup"><span data-stu-id="a3532-120">It is possible toohave multiple CDN profiles with hello same name, so long as they are in different resource groups.</span></span>  <span data-ttu-id="a3532-121">Vynechání hello `ResourceGroupName` parametr vrátí všechny profily s odpovídajícím názvem.</span><span class="sxs-lookup"><span data-stu-id="a3532-121">Omitting hello `ResourceGroupName` parameter returns all profiles with a matching name.</span></span>
> 
> 

## <a name="listing-existing-cdn-endpoints"></a><span data-ttu-id="a3532-122">Výpis stávající koncové body CDN</span><span class="sxs-lookup"><span data-stu-id="a3532-122">Listing existing CDN endpoints</span></span>
<span data-ttu-id="a3532-123">`Get-AzureRmCdnEndpoint`může načíst koncový bod jednotlivé nebo všechny koncové body hello v profilu.</span><span class="sxs-lookup"><span data-stu-id="a3532-123">`Get-AzureRmCdnEndpoint` can retrieve an individual endpoint or all hello endpoints on a profile.</span></span>  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of hello endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of hello endpoints on all of hello profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of hello endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a><span data-ttu-id="a3532-124">Vytváření profilů CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="a3532-124">Creating CDN profiles and endpoints</span></span>
<span data-ttu-id="a3532-125">`New-AzureRmCdnProfile`a `New-AzureRmCdnEndpoint` jsou koncové body a použít toocreate profilů CDN.</span><span class="sxs-lookup"><span data-stu-id="a3532-125">`New-AzureRmCdnProfile` and `New-AzureRmCdnEndpoint` are used toocreate CDN profiles and endpoints.</span></span>

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a><span data-ttu-id="a3532-126">Kontrola dostupnosti název koncového bodu</span><span class="sxs-lookup"><span data-stu-id="a3532-126">Checking endpoint name availability</span></span>
<span data-ttu-id="a3532-127">`Get-AzureRmCdnEndpointNameAvailability`Vrátí objekt, která udává, zda je název koncového bodu k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a3532-127">`Get-AzureRmCdnEndpointNameAvailability` returns an object indicating if an endpoint name is available.</span></span>

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message toohello console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a><span data-ttu-id="a3532-128">Přidání vlastní domény</span><span class="sxs-lookup"><span data-stu-id="a3532-128">Adding a custom domain</span></span>
<span data-ttu-id="a3532-129">`New-AzureRmCdnCustomDomain`Přidá vlastní domény název tooan existující koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a3532-129">`New-AzureRmCdnCustomDomain` adds a custom domain name tooan existing endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3532-130">Musíte vytvořit hello CNAME u svého poskytovatele DNS jak je popsáno v [jak toomap vlastní domény tooContent Delivery Network (CDN) endpoint](cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="a3532-130">You must set up hello CNAME with your DNS provider as described in [How toomap Custom Domain tooContent Delivery Network (CDN) endpoint](cdn-map-content-to-custom-domain.md).</span></span>  <span data-ttu-id="a3532-131">Můžete otestovat hello mapování před změnou váš koncový bod pomocí `Test-AzureRmCdnCustomDomain`.</span><span class="sxs-lookup"><span data-stu-id="a3532-131">You can test hello mapping before modifying your endpoint using `Test-AzureRmCdnCustomDomain`.</span></span>
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check hello mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create hello custom domain on hello endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a><span data-ttu-id="a3532-132">Úprava koncový bod</span><span class="sxs-lookup"><span data-stu-id="a3532-132">Modifying an endpoint</span></span>
<span data-ttu-id="a3532-133">`Set-AzureRmCdnEndpoint`upravuje existující koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a3532-133">`Set-AzureRmCdnEndpoint` modifies an existing endpoint.</span></span>

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save hello changed endpoint and apply hello changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a><span data-ttu-id="a3532-134">Vymazání/vedlejší-loading CDN prostředky</span><span class="sxs-lookup"><span data-stu-id="a3532-134">Purging/Pre-loading CDN assets</span></span>
<span data-ttu-id="a3532-135">`Unpublish-AzureRmCdnEndpointContent`vyprazdňovat mezipaměti prostředky, zatímco `Publish-AzureRmCdnEndpointContent` předem načte prostředky na podporovaných koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="a3532-135">`Unpublish-AzureRmCdnEndpointContent` purges cached assets, while `Publish-AzureRmCdnEndpointContent` pre-loads assets on supported endpoints.</span></span>

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a><span data-ttu-id="a3532-136">Koncové body CDN spuštění nebo zastavení</span><span class="sxs-lookup"><span data-stu-id="a3532-136">Starting/Stopping CDN endpoints</span></span>
<span data-ttu-id="a3532-137">`Start-AzureRmCdnEndpoint`a `Stop-AzureRmCdnEndpoint` lze použít toostart a ukončete jednotlivé koncové body nebo skupiny koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="a3532-137">`Start-AzureRmCdnEndpoint` and `Stop-AzureRmCdnEndpoint` can be used toostart and stop individual endpoints or groups of endpoints.</span></span>

```powershell
# Stop hello cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a><span data-ttu-id="a3532-138">Odstranění prostředků CDN</span><span class="sxs-lookup"><span data-stu-id="a3532-138">Deleting CDN resources</span></span>
<span data-ttu-id="a3532-139">`Remove-AzureRmCdnProfile`a `Remove-AzureRmCdnEndpoint` lze použít tooremove profily a koncové body.</span><span class="sxs-lookup"><span data-stu-id="a3532-139">`Remove-AzureRmCdnProfile` and `Remove-AzureRmCdnEndpoint` can be used tooremove profiles and endpoints.</span></span>

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all hello endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a><span data-ttu-id="a3532-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3532-140">Next Steps</span></span>
<span data-ttu-id="a3532-141">Zjistěte, jak tooautomate Azure CDN s [.NET](cdn-app-dev-net.md) nebo [Node.js](cdn-app-dev-node.md).</span><span class="sxs-lookup"><span data-stu-id="a3532-141">Learn how tooautomate Azure CDN with [.NET](cdn-app-dev-net.md) or [Node.js](cdn-app-dev-node.md).</span></span>

<span data-ttu-id="a3532-142">toolearn o funkcích CDN, najdete v části [přehled CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a3532-142">toolearn about CDN features, see [CDN Overview](cdn-overview.md).</span></span>

