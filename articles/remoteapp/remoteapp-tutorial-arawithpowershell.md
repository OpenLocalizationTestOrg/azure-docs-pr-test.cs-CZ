---
title: "aaaUse rutiny prostředí PowerShell s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak toouse rutiny prostředí Windows PowerShell v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a09cae2093e2c3a4a2ed673b5d148a22ceb935f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Pomocí rutin prostředí Windows PowerShell s Azure Remoteappem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

 Můžete použít tooadminister rutin prostředí Azure RemoteApp PowerShell hello a spravovat kolekce. Použijte následující informace tooget spuštění hello.

## <a name="get-hello-cmdlets"></a>Získat rutiny hello
- - -
Nejdřív stáhnout rutin prostředí Azure Powershell hello [zde](http://go.microsoft.com/?linkid=9811175), rutiny hello vzdálené aplikace RemoteApp jsou zahrnuty v ní. 

Podívejte se na hello [nápovědu rutiny Azure Remoteappu](/powershell/module/azure?view=azuresmps-3.7.0).

## <a name="configure-azure-cmdlets-toouse-your-subscription"></a>Konfigurovat rutiny Azure toouse předplatné
- - -
Postupujte podle [Tato příručka](/powershell/azure/overview) abyste je mohli používat rutiny hello u vašeho předplatného Azure.

Můžete provádět tyto kroky tooget rychle začít:

1. Stáhněte a nainstalujte hello [rutin prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175).
2. Spusťte Microsoft Azure PowerShell.
3. Spustit **Add-AzureAccount** tooauthenticate tooyour předplatného Azure. Po zobrazení výzvy zadejte hello stejné uživatelské jméno a heslo používané toosign tooAzure portálu.  
4. Spustit **Get-AzureSubscription** toolist hello předplatná spojená s vaším uživatelským účtem. 
5. Spustit **Select-AzureSubscription - Název_předplatného &lt;název odběru&gt;**  nebo **Select-AzureSubscription - SubscriptionId &lt;ID předplatného&gt;**  toospecify hello předplatné toouse.

Blahopřejeme, je konfigurována a připravena toouse konzola prostředí Azure PowerShell. Mějte na paměti, že potřebujete toorepeate kroky 2 až 5 při každém spuštění konzoly Azure PowerShell hello hello.  


## <a name="list-all-collections"></a>Zobrazí seznam všech kolekcí
- - -
     Get-AzureRemoteAppCollection

## <a name="delete-a-collection"></a>Odstranit kolekci
- - -
    Odebrat AzureRemoteAppCollection<enter collection name>

Příklad: `Remove-AzureRemoteAppCollection ContosoProduction`.

## <a name="create-a-cloud-collection"></a>Vytvoření cloudové kolekce
- - -
Je jednoduchý, spusťte následující příkaz hello:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Hello výše příkaz automaticky publikuje aplikace Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, aplikace Visio a Word).

Vytvoření kolekce může trvat 30 minut nebo déle toocomplete. Proto tento příkaz vrátí ID pro sledování, který můžete použít takto:

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Po dokončení hello kolekce, můžete přidat uživatele toohello kolekce s hello následující příkaz:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

A jste hotovi! Tento uživatel by měl být schopný tooconnect toohello aplikací pomocí klienta aplikace Azure RemoteApp hello najít [zde](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Dostupné rutiny
Obsahuje velké množství jinými příkazy, které máme, hello dokumentace pro ně budou přicházet krátce:

Základní rutiny kolekce vzdálené aplikace RemoteApp: 

* New-AzureRemoteAppCollection
* Get-AzureRemoteAppCollection
* Set-AzureRemoteAppCollection
* Update-AzureRemoteAppCollection
* Remove-AzureRemoteAppCollection
* Add-AzureRemoteAppUser
* Get-AzureRemoteAppUser
* Remove-AzureRemoteAppUser
* Get-AzureRemoteAppSession
* Disconnect-AzureRemoteAppSession
* Invoke-AzureRemoteAppSessionLogoff
* Send-AzureRemoteAppSessionMessage
* Get-AzureRemoteAppProgram
* Get-AzureRemoteAppStartMenuProgram
* Publish-AzureRemoteAppProgram
* Unpublish-AzureRemoteAppProgram
* Get-AzureRemoteAppCollectionUsageDetails
* Get-AzureRemoteAppCollectionUsageSummary
* Get-AzureRemoteAppPlan

Rutiny virtuální sítě vzdálené aplikace RemoteApp:

* New-AzureRemoteAppVNet
* Get-AzureRemoteAppVNet
* Set-AzureRemoteAppVNet
* Remove-AzureRemoteAppVNet
* Get-AzureRemoteAppVpnDevice
* Get – AzureRemoteAppVpnDeviceConfigScript
* Reset-AzureRemoteAppVpnSharedKey

Rutiny image šablony vzdálené aplikace RemoteApp:

* New-AzureRemoteAppTemplateImage
* Get-AzureRemoteAppTemplateImage
* Rename-AzureRemoteAppTemplateImage
* Remove-AzureRemoteAppTemplateImage

Ostatní rutiny vzdálené aplikace RemoteApp:

* Get-AzureRemoteAppLocation
* Get-AzureRemoteAppWorkspace
* Set-AzureRemoteAppWorkspace
* Get-AzureRemoteAppOperationResult

