---
title: "příkazy aaaAzure založené na správci prostředků prostředí PowerShell pro webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello nové toomanage příkazy prostředí PowerShell využívající Azure Resource Manager webových aplikacích Azure."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 4231fbba-61e5-4f92-b958-531c066fb87f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: bbb821e89daa315280436e84e11316217bb644d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-powershell-toomanage-azure-web-apps"></a>Pomocí prostředí PowerShell Azure Resource Manager-Based tooManage Azure Web Apps
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

Microsoft Azure PowerShell verze 1.0.0 byly přidány nové příkazy, které poskytují hello uživatele hello možnost toouse založené na správci prostředků Azure PowerShell příkazy toomanage webové aplikace.

toolearn o správě skupin prostředků, najdete v části [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md). 

toolearn o hello úplný seznam parametrů a možnosti pro hello rutiny prostředí PowerShell najdete v části hello [úplné referenční informace o rutinách rutin prostředí PowerShell využívající webové aplikace Azure Resource Manager](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Správa služby App Service plány
### <a name="create-an-app-service-plan"></a>Vytvořit plán služby App Service
toocreate plán služby app service, použijte hello **New-AzureRmAppServicePlan** rutiny.

Následují popisy hello různé parametry:

* **Název**: název hello plán služby app service.
* **Umístění**: plán umístění služby.
* **Název skupiny prostředků**: skupinu prostředků, která zahrnuje hello nově vytvořený plán služby app service.
* **Úroveň**: hello potřeby cenová úroveň (výchozí hodnota je volné, ostatní možnosti patří sdílené, Basic, Standard a Premium.)
* **WorkerSize**: hello velikost pracovních procesů (výchozí hodnota je malý, pokud byl zadán parametr vrstvy hello jako Basic, Standard nebo Premium. Další možnosti jsou střední a velké).
* **NumberofWorkers**: hello počet pracovních procesů v plánu služby app hello (výchozí hodnota je 1). 

Příklad toouse tuto rutinu:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Vytvořit plán služby App Service ve službě App Service Environment
toocreate aplikační službu plánování ve službě app service environment, použijte hello stejný příkaz **New-AzureRmAppServicePlan** příkaz s názvem další parametry toospecify hello App Service Environment společnosti a App Service Environment je název skupiny prostředků.

Příklad toouse tuto rutinu:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Další informace o službě app service environment, zkontrolujte toolearn [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Seznam existující plán služby App Service
používat toolist hello existující aplikace služby plány **Get-AzureRmAppServicePlan** rutiny.

toolist všechny plány služby app v rámci svého předplatného, použijte: 

    Get-AzureRmAppServicePlan

toolist všechny plány služby app v určité skupiny zdrojů, použijte:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

tooget plán služeb konkrétní aplikaci, použijte:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurace existující plán služby App Service
toochange hello nastavení existujícího plánu služby app, použijte hello **Set-AzureRmAppServicePlan** rutiny. Můžete změnit úroveň hello, velikost pracovního procesu a hello počet pracovních procesů 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Škálování plán služby App Service
tooscale existující plán služby App Service, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-hello-worker-size-of-an-app-service-plan"></a>Změna velikosti pracovní hello plánu služby App Service
velikost hello toochange pracovníků v existující plán služby App Service, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-hello-tier-of-an-app-service-plan"></a>Změna hello úroveň plán služby App Service
toochange úroveň hello existující plán služby App Service, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Odstranit existující plán služby App Service
toodelete existující plán služby app service, všechny přiřazené webové aplikace potřeba toobe nepřesunul nebo neodstranil nejdřív. Potom pomocí hello **odebrat AzureRmAppServicePlan** rutiny můžete odstranit hello plán služby app service.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Správa webové aplikace aplikační služby
### <a name="create-a-web-app"></a>Vytvoření webové aplikace
toocreate webové aplikace, použijte hello **New-AzureRmWebApp** rutiny.

Následují popisy hello různé parametry:

* **Název**: název webové aplikace hello.
* **AppServicePlan**: název pro plán služby hello použít toohost hello webové aplikace.
* **Název skupiny prostředků**: skupinu prostředků, který je hostitelem hello plán služby App service.
* **Umístění**: hello umístění webové aplikace.

Příklad toouse tuto rutinu:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Vytvoření webové aplikace ve službě App Service Environment
toocreate webové aplikace v App Service prostředí (App Service Environment). Použití hello stejné **New-AzureRmWebApp** příkaz s názvem hello App Service Environment toospecify další parametry a hello název skupiny prostředků, které patří hello App Service Environment.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Další informace o službě app service environment, zkontrolujte toolearn [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Odstranit stávající webovou aplikaci
toodelete stávající webovou aplikaci můžete použít hello **odebrat AzureRmWebApp** rutiny, je třeba název hello toospecify hello webové aplikace a název skupiny prostředků hello.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Seznam existující webové aplikace
toolist hello existující webové aplikace, použijte hello **Get-AzureRmWebApp** rutiny.

toolist všechny webové aplikace v rámci svého předplatného, použijte:

    Get-AzureRmWebApp

toolist všechny webové aplikace v rámci určité skupiny zdrojů, použijte:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

tooget konkrétní webovou aplikaci, použijte:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurace existující webové aplikace
toochange hello nastavení a konfigurace pro stávající webovou aplikaci, použijte hello **Set-AzureRmWebApp** rutiny. Úplný seznam parametrů, zkontrolujte hello [rutiny odkazem](https://msdn.microsoft.com/library/mt652487.aspx)

Příklad (1): pomocí této rutiny toochange připojovací řetězce

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Příklad (2): Přidání nebo změna nastavení aplikace

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Příklad (3): nastavte hello webové aplikace toorun v režimu 64-bit

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-hello-state-of-an-existing-web-app"></a>Změnit stav hello stávající webovou aplikaci
#### <a name="restart-a-web-app"></a>Restartování webové aplikace
toorestart webové aplikace, je nutné zadat název a prostředek skupiny hello hello webové aplikace.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Zastavení webové aplikace
toostop webové aplikace, je nutné zadat název a prostředek skupiny hello hello webové aplikace.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Spustí webovou aplikaci
toostart webové aplikace, je nutné zadat název a prostředek skupiny hello hello webové aplikace.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Správa profilů publikování webové aplikace
Každý webové aplikace se profil publikování, které můžou být použité toopublish aplikace, několik operací mohou být provedeny u profilů publikování.

#### <a name="get-publishing-profile"></a>Získat profil publikování
hello tooget publikování profil pro webovou aplikaci, použijte:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Tento příkaz vrátí, že hello publikování profil toohello příkazového řádku také výstup hello publikování profil tooa textového souboru.

#### <a name="reset-publishing-profile"></a>Resetování profilu publikování
tooreset i pro FTP a webové nasazení pro webovou aplikaci, použijte hello publikování heslo:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Spravovat certifikáty webové aplikace
toolearn o tom, jak toomanage webové aplikace certifikáty, najdete v části [vazby certifikátů SSL pomocí prostředí PowerShell](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Další kroky
* toolearn o podpoře Azure Resource Manager PowerShell najdete v části [použití Azure Powershellu s Azure Resource Manager.](../powershell-azure-resource-manager.md)
* toolearn o prostředí App Service najdete v části [Úvod tooApp Service Environment.](app-service-app-service-environment-intro.md)
* toolearn o správě certifikátů App Service SSL pomocí prostředí PowerShell, najdete v části [vazby certifikátů SSL pomocí prostředí PowerShell.](app-service-web-app-powershell-ssl-binding.md)
* toolearn o hello úplný seznam rutin prostředí PowerShell založené na správci prostředků Azure pro webové aplikace Azure, najdete v části [Azure Cmdlet Reference rutin Powershellu webové aplikace Azure Resource Manager.](https://msdn.microsoft.com/library/mt619237.aspx)
* * toolearn o správě služby App Service pomocí rozhraní příkazového řádku, najdete v části [Using Azure Resource Manager-Based XPlat rozhraní příkazového řádku pro webové aplikace Azure.](app-service-web-app-azure-resource-manager-xplat-cli.md)

