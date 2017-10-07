---
title: "aaaWeb aplikace klonování pomocí prostředí PowerShell"
description: "Zjistěte, jak tooclone vaší webové aplikace toonew webové aplikace pomocí prostředí PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: b8882370d6db6939f8e4473ccc1414091bdcb8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Aplikační služby Azure klonování pomocí prostředí PowerShell
Hello verze služby Microsoft Azure PowerShell verze 1.1.0 novou možnost byla přidána tooNew AzureRMWebApp, kterého by hello uživatele hello možnost tooclone stávající tooa nově vytvořený aplikace webové aplikace v jiné oblasti nebo v hello stejné oblasti. Tím povolíte zákazníci toodeploy počet aplikací v různých oblastech snadno a rychle.

Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium. nové funkce používá Hello hello stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

toolearn o pomocí Azure Resource Manager na základě toomanage rutin prostředí Azure PowerShell vaší webové aplikace zkontrolujte [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Klonování existující aplikace
Scénář: Stávající webovou aplikaci v oblasti jihu USA, hello uživatele chcete tooclone hello obsah tooa nové webové aplikace v oblasti severní jihu USA. To můžete udělat pomocí Azure Resource Manager hello verzi toocreate rutiny prostředí PowerShell hello novou webovou aplikaci s možností - SourceWebApp hello.

Znalost hello prostředků název skupiny, který obsahuje hello zdrojové webové aplikace, můžeme použít hello následující informace o prostředí PowerShell příkaz tooget hello zdrojové webové aplikace (v takovém případě s názvem zdrojového webapp):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

toocreate nový plán služby App Service, můžeme použít příkaz New-AzureRmAppServicePlan jako následující příklad hello

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Pomocí příkazu hello New-AzureRmWebApp, jsme vytvořte hello novou webovou aplikaci v oblasti severní centrální nám hello a tie ho tooan existující plán služby App Service úrovně premium, kromě toho můžeme použít hello stejný zdroj skupiny jako hello zdrojové webové aplikace nebo definovat novou skupinu prostředků , následující hello prokáže, že:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

tooclone stávající webovou aplikaci včetně všech přidružených nasazovací sloty, třeba uživatel hello toouse hello parametr IncludeSourceWebAppSlots, hello následující příkaz prostředí PowerShell ukazuje hello použití tohoto parametru s hello AzureRmWebApp nový příkaz:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

tooclone stávající webovou aplikaci v rámci hello stejné oblasti, třeba uživatel hello toocreate novou skupinu prostředků a nové služby app service plánování v hello stejné oblasti a potom použitím hello následující prostředí PowerShell příkaz tooclone hello webové aplikace

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a>Klonování existující aplikace tooan App Service Environment
Scénář: Stávající webovou aplikaci v oblasti jihu USA, hello uživatele chcete tooclone hello obsah tooa nové webové aplikace tooan stávající prostředí App Service (App Service Environment).

Znalost hello prostředků název skupiny, který obsahuje hello zdrojové webové aplikace, můžeme použít hello následující informace o prostředí PowerShell příkaz tooget hello zdrojové webové aplikace (v takovém případě s názvem zdrojového webapp):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Znalost hello App Service Environment je název a hello název skupiny prostředků, které patří hello App Service Environment, hello uživatele můžete použít příkaz hello New-AzureRmWebApp toocreate hello nové webové aplikace v App Service Environment existující hello, hello následující prokáže, že:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Hello umístění parametr je vyžadován kvůli toolegacy důvod, ale v případě hello vytváření aplikace v App Service Environment se budou ignorovat. 

## <a name="cloning-an-existing-app-slot"></a>Klonování existujícího slotu aplikace
Scénář: hello uživatele chcete tooclone existující Slot webové aplikace tooeither novou webovou aplikaci nebo nový slot webové aplikace. Hello nové webové aplikace může být v hello stejné oblasti jako původní slot webové aplikace hello nebo v jiné oblasti.

Znalost hello prostředků název skupiny, který obsahuje hello zdrojové webové aplikace, můžeme použít následující příkaz prostředí PowerShell tooget hello zdrojový slot webové aplikace na informace (v takovém případě s názvem zdrojového webappslot) vázaný zdroj aplikace tooWeb-webapp hello:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Následující Hello ukazuje vytvoření klonu hello zdrojové webové aplikace tooa nové webové aplikace:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurace Traffic Manager během klonování aplikace
Vytvoření více oblast webové aplikace a konfigurace Azure Traffic Manager tooroute provoz tooall tyto webové aplikace, je tooinsure n důležité scénář, který aplikace zákazníků jsou vysoce dostupný, když klonování stávající webovou aplikaci, kterou máte hello možnost tooconnect obou web aplikace tooeither nový profil správce provozu nebo stávající - Upozorňujeme, že pouze verze Azure Resource Manageru z Traffic Manager je podporována.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Vytvoření nového profilu Traffic Manageru během klonování aplikace
Scénář: přeje hello uživatele tooclone tooanother oblast webové aplikace při konfiguraci profil správce provozu Azure Resource Manager, které zahrnují oba webové aplikace. Následující Hello ukazuje vytvoření klonu hello zdrojové webové aplikace tooa nové webové aplikace při konfiguraci nového profilu Traffic Manageru:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-tooan-existing-traffic-manager-profile"></a>Přidání nové klonovaný webové aplikace tooan existující profil služby Traffic Manager
Scénář: hello uživatele už máte profil správce provozu Azure Resource Manager, by chtěl tooadd i webové aplikace jako koncové body. toodo Ano, je nejprve třeba tooassemble hello stávající id profilu Správce provozu, budeme potřebovat hello id odběru, název skupiny prostředků a název profilu manager stávající provoz hello.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Po s id Manager hello provoz, následující hello ukazuje vytvoření klonu hello zdrojové webové aplikace tooa nové webové aplikace při přidávání je tooan existující profil služby Traffic Manager:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuální omezení
Tato funkce je aktuálně ve verzi preview, pracujeme tooadd nové funkce v čase, hello následujícím seznamu jsou hello známé omezení aktuální verze hello klonování aplikace:

* Nastavení automatického škálování nejsou klonovat.
* Nastavení plánu zálohování nejsou klonovat.
* Nastavení sítě VNET nejsou klonovat.
* Přehled aplikace nejsou automaticky na nastavit hello cílové webové aplikace
* Nejsou klonovat snadno nastavení ověřování
* Kudu rozšíření nejsou klonovat.
* TiP pravidla nejsou klonovat.
* Nejsou klonovat databáze obsahu

### <a name="references"></a>Odkazy
* [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)
* [Webové aplikace klonování pomocí portálu Azure](app-service-web-app-cloning-portal.md)
* [Zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)
* [Azure Resource Manager podpora pro Azure Traffic Manager Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)
* [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md)

