---
title: "aaaAzure nástroje příkazového řádku založené na správci prostředků a platformy pro webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello nové nástroje příkazového řádku založené na Azure Resource Manager napříč platformami toomanage webových aplikacích Azure."
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
ms.openlocfilehash: 5f5e03edcb01154aef3bd220cd27358f69656ef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Pomocí příkazového řádku XPlat založené na správci prostředků Azure pro službu Azure App Service
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Verze hello nástroje příkazového řádku pro různé platformy Microsoft Azure verze 0.10.5 byly přidány nové příkazy. Tyto příkazy poskytnout hello uživatele hello možnost toouse založené na správci prostředků Azure PowerShell příkazy toomanage služby App Service.

toolearn o správě skupin prostředků, najdete v části [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Také si vyzkoušet [Azure CLI 2.0](https://github.com/Azure/azure-cli), rozhraní příkazového řádku příští generace, napsané v Pythonu pro model nasazení správy prostředků hello.
>
>

## <a name="managing-app-service-plans"></a>Správa služby App Service plány
### <a name="create-an-app-service-plan"></a>Vytvořit plán služby App Service
toocreate plán služby app service, použijte hello **vytvořit azure appserviceplan** příkaz.

Následují popisy hello různé parametry:

* **--resource-group**: skupinu prostředků, která zahrnuje hello nově vytvořený plán služby app service.
* **– název**: název hello plán služby app service.
* **--umístění**: umístění plánu služby app.
* **--sku**: hello potřeby cenovou sku (hello možnosti jsou: F1 (zdarma). D1 (sdílené). B1 (základní malá), B2 (základní střední) a B3 (Basic velké). S1 (standardní malé), S2 (standardní střední) a S3 (standardní velké). P1 (malé Premium), P2 (střední Premium) a P3 (velké Premium).)
* **--instancí**: hello počet pracovních procesů v plánu služby app hello (výchozí hodnota je 1).

Příklad toouse tuto rutinu:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

Pomocí stejné hello **vytvořit azure appserviceplan** příkazu s hello speciálním parametrem **– islinux true**. Všimněte si hello omezení a oblastí, které jsou popsané v [Úvod tooApp služby v systému Linux](app-service-linux-intro.md)

### <a name="list-existing-app-service-plans"></a>Seznam existující plán služby App Service
používat toolist hello existující aplikace služby plány **seznamu azure appserviceplan** příkaz.

toolist všechny plány služby app v určité skupiny zdrojů, použijte:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

tooget plán služeb konkrétní aplikaci, použijte **zobrazit azure appserviceplan** příkaz:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurace existující plán služby App Service
toochange hello nastavení existujícího plánu služby app, použijte hello **konfigurace azure appserviceplan** příkaz. Můžete změnit hello sku a hello počet pracovních procesů 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Škálování plán služby App Service
tooscale existující plán služby App Service, použijte:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-hello-sku-of-an-app-service-plan"></a>Změna hello skladová položka plánu služby App Service
toochange skladová položka hello existující plán služby App Service, použijte:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Odstranit existující plán služby App Service
toodelete existující plán služby app service, všechny přiřazené aplikace potřeba toobe nepřesunul nebo neodstranil nejdřív. Potom pomocí hello **odstranit azure webapp** příkaz odstraníte hello plán služby app service.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Správa aplikací App Service
### <a name="create-a-web-app"></a>Vytvoření webové aplikace
toocreate webové aplikace, použijte hello **vytvořit azure webapp** příkaz.

Následují popisy hello různé parametry:

* **– název**: název webové aplikace hello.
* **--plán**: název pro plán služby hello použít toohost hello webové aplikace.
* **--resource-group**: skupinu prostředků, který je hostitelem hello plán služby App service.
* **--umístění**: hello umístění webové aplikace.

Příklad toouse tuto rutinu:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Odstraňte existující aplikace
toodelete existující aplikaci, můžete použít hello **odstranit azure webapp** příkaz. Potřebujete toospecify hello název aplikace hello a název skupiny prostředků hello.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Zobrazí seznam stávajících aplikací
toolist hello existující aplikace, použijte hello **azure webapp seznamu** příkaz.

toolist všechny aplikace v určité skupiny zdrojů, použijte:

    azure webapp list --resource-group ContosoAzureResourceGroup

tooget konkrétní aplikaci, použijte hello **azure webapp zobrazit** příkaz.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Konfigurace existující aplikace
toochange hello nastavení a konfigurace pro existující aplikace, použijte hello **set config azure webapp** příkaz.

Příklad (1): Změňte hello verzi php aplikace 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Příklad (2): Přidání nebo změna nastavení aplikace

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

tooknow ostatní konfigurace, které je možné změnit, použijte hello **konfigurace azure webapp nastavit -h** příkaz.

### <a name="change-hello-state-of-an-existing-app"></a>Změnit stav hello existující aplikace
#### <a name="restart-an-app"></a>Restartujte aplikace
toorestart na aplikace, je nutné zadat název a prostředek skupiny hello aplikace hello.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Zastavte aplikaci
toostop na aplikace, je nutné zadat název a prostředek skupiny hello aplikace hello.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Spuštění aplikace
toostart na aplikace, je nutné zadat název a prostředek skupiny hello aplikace hello.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Správa profilů publikování aplikace
Každá aplikace má profil publikování, které můžou být použité toopublish vašeho kódu.

#### <a name="get-publishing-profile"></a>Získat profil publikování
hello tooget publikování pro aplikaci, použijte profil:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Tento příkaz vrátí hello publikování profil uživatelské jméno a heslo toohello příkazového řádku.

### <a name="manage-app-hostnames"></a>Spravovat aplikace názvy hostitelů
vazby názvů hostitelů toomanage pro vaši aplikaci, použijte hello **názvy hostitelů konfigurace azure webapp** příkaz  

#### <a name="list-hostname-bindings"></a>Seznam vazby názvů hostitelů.
tooget hello vazby na aktuální název hostitele pro aplikaci, použijte:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Přidat vazby názvů hostitelů.
tooadd hostname vazby tooan aplikace, použijte:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Odstranit vazby názvů hostitelů.
toodelete vazby názvů hostitelů, použijte:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Další kroky
* toolearn týkající se podpory rozhraní příkazového řádku Azure Resource Manager, najdete v části [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)
* toolearn o správě služby App Service pomocí prostředí PowerShell, najdete v části [tooManage Using Azure Resource Manager-Based prostředí PowerShell Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
* toolearn o službě Azure App Service pro systémy Linux, najdete v části [Úvod tooApp služby v systému Linux](app-service-linux-intro.md)
