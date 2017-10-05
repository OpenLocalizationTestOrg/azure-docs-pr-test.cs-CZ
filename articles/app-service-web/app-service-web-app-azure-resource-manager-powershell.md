---
title: "Azure PowerShell využívající Resource Manager příkazy pro webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak používat nové příkazy prostředí PowerShell využívající Azure Resource Manager ke správě webových aplikacích Azure."
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
ms.openlocfilehash: 8d574f051a327ba0409e6f25a5886af673d3d5e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Pomocí prostředí PowerShell založené na správci prostředků Azure ke správě webových aplikacích Azure
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

Microsoft Azure PowerShell verze 1.0.0 byly přidány nové příkazy, které uživateli přidělit možnost použít příkazy prostředí PowerShell využívající Azure Resource Manager ke správě webových aplikací.

Další informace o správě skupin prostředků, najdete v části [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md). 

Další informace o úplný seznam parametrů a možnosti pro rutiny prostředí PowerShell najdete v tématu [úplné referenční informace o rutinách rutin prostředí PowerShell využívající webové aplikace Azure Resource Manager](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Správa služby App Service plány
### <a name="create-an-app-service-plan"></a>Vytvořit plán služby App Service
Chcete-li vytvořit plán služby app service, použijte **New-AzureRmAppServicePlan** rutiny.

Následují popisy různé parametry:

* **Název**: název plánu služby app service.
* **Umístění**: plán umístění služby.
* **Název skupiny prostředků**: skupinu prostředků, která zahrnuje tarifu nově vytvořené aplikace.
* **Úroveň**: s požadovanou cenovou úrovní (výchozí hodnota je volné, ostatní možnosti patří sdílené, Basic, Standard a Premium.)
* **WorkerSize**: velikost pracovních procesů (výchozí hodnota je malý, pokud parametr vrstvy byl zadán jako Basic, Standard nebo Premium. Další možnosti jsou střední a velké).
* **NumberofWorkers**: počet pracovních procesů v aplikaci služby plán (výchozí hodnota je 1). 

Příklad pro použití této rutiny:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Vytvořit plán služby App Service ve službě App Service Environment
Chcete-li vytvořit plán služby app service ve službě app service environment, použijte stejný příkaz **New-AzureRmAppServicePlan** s další parametry zadejte název App Service Environment a App Service Environment je název skupiny prostředků.

Příklad pro použití této rutiny:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Další informace o službě app service environment, zkontrolujte [Úvod do služby App Service Environment](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Seznam existující plán služby App Service
K zobrazení seznamu existující plány služby aplikace, použijte **Get-AzureRmAppServicePlan** rutiny.

K zobrazení seznamu všechny plány služby app v rámci svého předplatného, použijte: 

    Get-AzureRmAppServicePlan

K zobrazení seznamu všech plánů služby app podle určité skupiny zdrojů, použijte:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Plán služby konkrétní aplikaci, použijte:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurace existující plán služby App Service
Chcete-li změnit nastavení existujícího plánu služby app, použijte **Set-AzureRmAppServicePlan** rutiny. Můžete změnit úroveň, pracovní velikost a počet pracovních procesů 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Škálování plán služby App Service
Pokud chcete použít škálování, existující plán služby App Service, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Změna velikosti pracovního procesu plán služby App Service
Chcete-li změnit velikost pracovních procesů v existující plán služby App Service, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Při změně úrovně plán služby App Service
Pokud chcete změnit úroveň existující plán služby App Service, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Odstranit existující plán služby App Service
Pokud chcete odstranit existující plán služby app service, musí všechny přiřazené webové aplikace přesunout ani neodstraní. Potom pomocí **odebrat AzureRmAppServicePlan** rutiny můžete odstranit plán služby app service.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Správa webové aplikace aplikační služby
### <a name="create-a-web-app"></a>Vytvoření webové aplikace
Chcete-li vytvořit webovou aplikaci, použijte **New-AzureRmWebApp** rutiny.

Následují popisy různé parametry:

* **Název**: název webové aplikace.
* **AppServicePlan**: název plánu služby použít k hostování webové aplikace.
* **Název skupiny prostředků**: skupinu prostředků, který je hostitelem plán služby App service.
* **Umístění**: umístění webové aplikace.

Příklad pro použití této rutiny:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Vytvoření webové aplikace ve službě App Service Environment
K vytvoření webové aplikace v App Service prostředí (App Service Environment). Použijte stejný **New-AzureRmWebApp** příkazu s další parametry, zadejte název App Service Environment a skupině prostředků název, který App Service Environment patří.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Další informace o službě app service environment, zkontrolujte [Úvod do služby App Service Environment](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Odstranit stávající webovou aplikaci
Chcete-li odstranit stávající webovou aplikaci můžete použít **odebrat AzureRmWebApp** rutiny, je třeba zadat název webové aplikace a název skupiny prostředků.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Seznam existující webové aplikace
K zobrazení seznamu existující webové aplikace, použijte **Get-AzureRmWebApp** rutiny.

K zobrazení seznamu všech webových aplikací v rámci svého předplatného, použijte:

    Get-AzureRmWebApp

K zobrazení seznamu všech webových aplikací v rámci určité skupiny zdrojů, použijte:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Chcete-li získat konkrétní webovou aplikaci, použijte:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurace existující webové aplikace
Chcete-li změnit nastavení a konfigurace pro existující webovou aplikaci, použijte **Set-AzureRmWebApp** rutiny. Úplný seznam parametrů, najdete [rutiny odkazem](https://msdn.microsoft.com/library/mt652487.aspx)

Příklad (1): tuto rutinu použijte k změnit připojovací řetězce

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Příklad (2): Přidání nebo změna nastavení aplikace

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Příklad (3): nastavte webové aplikace na spouštění v režimu 64-bit

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Změnit stav stávající webovou aplikaci
#### <a name="restart-a-web-app"></a>Restartování webové aplikace
Pokud chcete restartovat webovou aplikaci, musíte zadat název a prostředek skupiny webové aplikace.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Zastavení webové aplikace
Zastavení webové aplikace, musíte zadat název a prostředek skupiny webové aplikace.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Spustí webovou aplikaci
Chcete-li spustí webovou aplikaci, musíte zadat název a prostředek skupiny webové aplikace.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Správa profilů publikování webové aplikace
Každý webové aplikace se profil publikování, kterou můžete použít k publikování aplikace, několik operací mohou být provedeny u profilů publikování.

#### <a name="get-publishing-profile"></a>Získat profil publikování
Profil publikování pro webovou aplikaci, použijte:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Tento příkaz vrátí profil publikování do příkazového řádku jako dobře výstupní profil publikování do textového souboru.

#### <a name="reset-publishing-profile"></a>Resetování profilu publikování
Obě resetovat heslo publikování FTP a webové nasadit pro webovou aplikaci, použijte:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Spravovat certifikáty webové aplikace
Další informace o správě certifikátů webových aplikací najdete v tématu [vazby certifikátů SSL pomocí prostředí PowerShell](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Další kroky
* Další informace o podpoře Azure Resource Manager PowerShell, najdete v části [použití Azure Powershellu s Azure Resource Manager.](../powershell-azure-resource-manager.md)
* Další informace o prostředí App Service najdete v tématu [Úvod do služby App Service Environment.](app-service-app-service-environment-intro.md)
* Další informace o správě certifikátů SSL aplikace služby pomocí prostředí PowerShell najdete v tématu [vazby certifikátů SSL pomocí prostředí PowerShell.](app-service-web-app-powershell-ssl-binding.md)
* Další informace o úplný seznam rutin prostředí PowerShell založené na správci prostředků Azure pro webové aplikace Azure najdete v tématu [Azure Cmdlet Reference rutin Powershellu webové aplikace Azure Resource Manager.](https://msdn.microsoft.com/library/mt619237.aspx)
* * Další informace o správě služby App Service pomocí rozhraní příkazového řádku najdete v tématu [Using Azure Resource Manager-Based XPlat rozhraní příkazového řádku pro webové aplikace Azure.](app-service-web-app-azure-resource-manager-xplat-cli.md)

