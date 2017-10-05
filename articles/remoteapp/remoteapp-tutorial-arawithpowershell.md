---
title: "Pomocí rutin prostředí PowerShell s Azure Remoteappem | Microsoft Docs"
description: "Další informace o použití rutin prostředí Windows PowerShell v Azure Remoteappu."
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
ms.openlocfilehash: 699b20a4dadd4ecaff57e2cc80355fe545360663
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Pomocí rutin prostředí Windows PowerShell s Azure Remoteappem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

 Rutiny prostředí Azure RemoteApp PowerShell můžete použít pro správu a údržbu kolekce. Abyste mohli začít, použijte následující informace.

## <a name="get-the-cmdlets"></a>Získat rutiny
- - -
Nejdřív stáhnout rutin prostředí Azure Powershell [zde](http://go.microsoft.com/?linkid=9811175), rutiny vzdálené aplikace RemoteApp jsou zahrnuty v ní. 

Podívejte se [nápovědu rutiny Azure Remoteappu](/powershell/module/azure?view=azuresmps-3.7.0).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfigurace rutiny na používání vašeho předplatného Azure
- - -
Postupujte podle [Tato příručka](/powershell/azure/overview) abyste je mohli používat rutiny pro vaše předplatné Azure.

Abyste mohli rychle začít, můžete použít tyto kroky:

1. Stáhněte a nainstalujte [rutin prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175).
2. Spusťte Microsoft Azure PowerShell.
3. Spustit **Add-AzureAccount** k ověření vašeho předplatného Azure. Po zobrazení výzvy zadejte stejné uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.  
4. Spustit **Get-AzureSubscription** seznam předplatná spojená s vaším uživatelským účtem. 
5. Spustit **Select-AzureSubscription - Název_předplatného &lt;název odběru&gt;**  nebo **Select-AzureSubscription - SubscriptionId &lt;ID předplatného&gt;**  pro určení předplatného, používat.

Blahopřejeme, konzolu prostředí Azure PowerShell je konfigurována a připravena k použití. Uvědomte si, že budete muset repeate kroky 2 až 5 při každém spuštění konzoly Azure PowerShell.  


## <a name="list-all-collections"></a>Zobrazí seznam všech kolekcí
- - -
     Get-AzureRemoteAppCollection

## <a name="delete-a-collection"></a>Odstranit kolekci
- - -
    Odebrat AzureRemoteAppCollection<enter collection name>

Příklad: `Remove-AzureRemoteAppCollection ContosoProduction`.

## <a name="create-a-cloud-collection"></a>Vytvoření cloudové kolekce
- - -
Je jednoduchý, spusťte následující příkaz:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Výše uvedený příkaz automaticky publikuje aplikace Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, aplikace Visio a Word).

Vytvoření kolekce může trvat 30 minut nebo déle. Proto tento příkaz vrátí ID pro sledování, který můžete použít takto:

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Po dokončení kolekce, můžete přidat uživatele do kolekce pomocí následujícího příkazu:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

A jste hotovi! Tento uživatel musí být schopný se připojit k aplikaci pomocí Azure RemoteApp klienta najít [zde](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Dostupné rutiny
Obsahuje velké množství jinými příkazy, které máme, dokumentace pro ně budou přicházet krátce:

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

