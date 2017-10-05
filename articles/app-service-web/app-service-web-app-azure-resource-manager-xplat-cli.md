---
title: "Webová aplikace Azure založené na správci prostředků mezi rozhraní příkazového řádku nástroje pro Azure | Microsoft Docs"
description: "Naučte se používat nový na základě Azure Resource Manager napříč platformami příkazového řádku nástroje pro správu webových aplikacích Azure."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: d415b195-4262-416f-b59f-7e1aef200054
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: 7a03e1417617453c43edcc3787da10d171359757
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Pomocí příkazového řádku XPlat založené na správci prostředků Azure pro službu Azure App Service
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

S vydáním verze nástroje příkazového řádku pro různé platformy Microsoft Azure 0.10.5 byly přidány nové příkazy. Tyto příkazy uživateli přidělit možnost použít příkazy prostředí PowerShell využívající Azure Resource Manager ke správě služby App Service.

Další informace o správě skupin prostředků, najdete v části [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Také si vyzkoušet [Azure CLI 2.0](https://github.com/Azure/azure-cli), rozhraní příkazového řádku příští generace, napsané v Pythonu pro model nasazení prostředků správy.
>
>

## <a name="managing-app-service-plans"></a>Správa služby App Service plány
### <a name="create-an-app-service-plan"></a>Vytvořit plán služby App Service
Chcete-li vytvořit plán služby app service, použijte **vytvořit azure appserviceplan** příkaz.

Následují popisy různé parametry:

* **--resource-group**: skupinu prostředků, která zahrnuje tarifu nově vytvořené aplikace.
* **– název**: název plánu služby app service.
* **--umístění**: umístění plánu služby app.
* **--sku**: požadovanou cenovou sku (Možnosti jsou: F1 (Free). D1 (sdílené). B1 (základní malá), B2 (základní střední) a B3 (Basic velké). S1 (standardní malé), S2 (standardní střední) a S3 (standardní velké). P1 (malé Premium), P2 (střední Premium) a P3 (velké Premium).)
* **--instancí**: počet pracovních procesů v aplikaci služby plán (výchozí hodnota je 1).

Příklad pro použití této rutiny:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

Používající stejný **vytvořit azure appserviceplan** příkazu se speciálním parametrem **– islinux true**. Všimněte si, omezení a oblastí, které jsou popsané v [Úvod do služby App Service v systému Linux](app-service-linux-intro.md)

### <a name="list-existing-app-service-plans"></a>Seznam existující plán služby App Service
K zobrazení seznamu existující plány služby aplikace, použijte **seznamu azure appserviceplan** příkaz.

K zobrazení seznamu všech plánů služby app podle určité skupiny zdrojů, použijte:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Plán služby konkrétní aplikaci, použijte **zobrazit azure appserviceplan** příkaz:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurace existující plán služby App Service
Chcete-li změnit nastavení existujícího plánu služby app, použijte **konfigurace azure appserviceplan** příkaz. Můžete změnit sku a počet pracovních procesů 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Škálování plán služby App Service
Pokud chcete použít škálování, existující plán služby App Service, použijte:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Změna skladová položka plán služby App Service
Chcete-li změnit sku systému existující plán služby App Service, použijte:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Odstranit existující plán služby App Service
Pokud chcete odstranit existující plán služby app service, musí všechny aplikace přiřazené přesunout ani neodstraní. Potom pomocí **odstranit azure webapp** příkaz můžete odstranit plán služby app service.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Správa aplikací App Service
### <a name="create-a-web-app"></a>Vytvoření webové aplikace
Chcete-li vytvořit webovou aplikaci, použijte **vytvořit azure webapp** příkaz.

Následují popisy různé parametry:

* **– název**: název webové aplikace.
* **--plán**: název plánu služby použít k hostování webové aplikace.
* **--resource-group**: skupinu prostředků, který je hostitelem plán služby App service.
* **--umístění**: umístění webové aplikace.

Příklad pro použití této rutiny:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Odstraňte existující aplikace
Chcete-li odstranit existující aplikaci, můžete použít **odstranit azure webapp** příkaz. Je třeba zadat název aplikace a název skupiny prostředků.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Zobrazí seznam stávajících aplikací
K zobrazení seznamu existující aplikace, použijte **azure webapp seznamu** příkaz.

K zobrazení seznamu všech aplikací v určité skupiny zdrojů, použijte:

    azure webapp list --resource-group ContosoAzureResourceGroup

Chcete-li získat konkrétní aplikaci, použijte **azure webapp zobrazit** příkaz.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Konfigurace existující aplikace
Chcete-li změnit nastavení a konfigurace pro existující aplikace, použijte **set config azure webapp** příkaz.

Příklad (1): změňte verzi php aplikace 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Příklad (2): Přidání nebo změna nastavení aplikace

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Potřebujete vědět, co může být jiná konfigurace změnit, použijte **konfigurace azure webapp nastavit -h** příkaz.

### <a name="change-the-state-of-an-existing-app"></a>Změnit stav existující aplikace
#### <a name="restart-an-app"></a>Restartujte aplikace
Pokud chcete restartovat aplikaci, musíte zadat název a prostředek skupiny aplikace.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Zastavte aplikaci
Chcete-li zastavit aplikaci, musíte zadat skupině název a prostředků pro aplikace.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Spuštění aplikace
Chcete-li spustit aplikaci, musíte zadat skupině název a prostředků pro aplikace.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Správa profilů publikování aplikace
Každá aplikace má profil publikování, kterou můžete použít k publikování vašeho kódu.

#### <a name="get-publishing-profile"></a>Získat profil publikování
Profil publikování pro aplikaci, použijte:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Tento příkaz vrátí publikování profil uživatelské jméno a heslo na příkazový řádek.

### <a name="manage-app-hostnames"></a>Spravovat aplikace názvy hostitelů
Chcete-li spravovat vazby názvů hostitelů pro vaši aplikaci, použijte **názvy hostitelů konfigurace azure webapp** příkaz  

#### <a name="list-hostname-bindings"></a>Seznam vazby názvů hostitelů.
Chcete-li získat aktuální vazby názvů hostitelů, pro aplikaci, použijte:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Přidat vazby názvů hostitelů.
Chcete-li do aplikace přidat vazby názvů hostitelů, použijte:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Odstranit vazby názvů hostitelů.
Chcete-li odstranit vazby názvů hostitelů, použijte:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Další kroky
* Další informace o podpoře Azure Resource Manager CLI, najdete v části [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)
* Další informace o správě služby App Service pomocí prostředí PowerShell najdete v tématu [Using Azure Resource Manager-Based prostředí PowerShell ke správě Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
* Další informace o službě Azure App Service v systému Linux najdete v tématu [Úvod do služby App Service v systému Linux](app-service-linux-intro.md)
