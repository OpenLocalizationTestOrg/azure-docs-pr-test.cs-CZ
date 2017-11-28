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
# <a name="azure-app-service-app-cloning-using-powershell"></a><span data-ttu-id="cdec1-103">Aplikační služby Azure klonování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cdec1-103">Azure App Service App Cloning Using PowerShell</span></span>
<span data-ttu-id="cdec1-104">Hello verze služby Microsoft Azure PowerShell verze 1.1.0 novou možnost byla přidána tooNew AzureRMWebApp, kterého by hello uživatele hello možnost tooclone stávající tooa nově vytvořený aplikace webové aplikace v jiné oblasti nebo v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="cdec1-104">With hello release of Microsoft Azure PowerShell version 1.1.0 a new option has been added tooNew-AzureRMWebApp that would give hello user hello ability tooclone an existing Web App tooa newly created app in a different region or in hello same region.</span></span> <span data-ttu-id="cdec1-105">Tím povolíte zákazníci toodeploy počet aplikací v různých oblastech snadno a rychle.</span><span class="sxs-lookup"><span data-stu-id="cdec1-105">This will enable customers toodeploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="cdec1-106">Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="cdec1-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="cdec1-107">nové funkce používá Hello hello stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="cdec1-107">hello new feature uses hello same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="cdec1-108">toolearn o pomocí Azure Resource Manager na základě toomanage rutin prostředí Azure PowerShell vaší webové aplikace zkontrolujte [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="cdec1-108">toolearn about using Azure Resource Manager based Azure PowerShell cmdlets toomanage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="cloning-an-existing-app"></a><span data-ttu-id="cdec1-109">Klonování existující aplikace</span><span class="sxs-lookup"><span data-stu-id="cdec1-109">Cloning an existing App</span></span>
<span data-ttu-id="cdec1-110">Scénář: Stávající webovou aplikaci v oblasti jihu USA, hello uživatele chcete tooclone hello obsah tooa nové webové aplikace v oblasti severní jihu USA.</span><span class="sxs-lookup"><span data-stu-id="cdec1-110">Scenario: An existing web app in South Central US region, hello user would like tooclone hello contents tooa new web app in North Central US region.</span></span> <span data-ttu-id="cdec1-111">To můžete udělat pomocí Azure Resource Manager hello verzi toocreate rutiny prostředí PowerShell hello novou webovou aplikaci s možností - SourceWebApp hello.</span><span class="sxs-lookup"><span data-stu-id="cdec1-111">This can be accomplished by using hello Azure Resource Manager version of hello PowerShell cmdlet toocreate a new web app with hello -SourceWebApp option.</span></span>

<span data-ttu-id="cdec1-112">Znalost hello prostředků název skupiny, který obsahuje hello zdrojové webové aplikace, můžeme použít hello následující informace o prostředí PowerShell příkaz tooget hello zdrojové webové aplikace (v takovém případě s názvem zdrojového webapp):</span><span class="sxs-lookup"><span data-stu-id="cdec1-112">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="cdec1-113">toocreate nový plán služby App Service, můžeme použít příkaz New-AzureRmAppServicePlan jako následující příklad hello</span><span class="sxs-lookup"><span data-stu-id="cdec1-113">toocreate a new App Service Plan, we can use New-AzureRmAppServicePlan command as in hello following example</span></span>

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

<span data-ttu-id="cdec1-114">Pomocí příkazu hello New-AzureRmWebApp, jsme vytvořte hello novou webovou aplikaci v oblasti severní centrální nám hello a tie ho tooan existující plán služby App Service úrovně premium, kromě toho můžeme použít hello stejný zdroj skupiny jako hello zdrojové webové aplikace nebo definovat novou skupinu prostředků , následující hello prokáže, že:</span><span class="sxs-lookup"><span data-stu-id="cdec1-114">Using hello New-AzureRmWebApp command, we can create hello new web app in hello North Central US region, and tie it tooan existing premium tier App Service Plan, moreover we can use hello same resource group as hello source web app, or define a new resource group, hello following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

<span data-ttu-id="cdec1-115">tooclone stávající webovou aplikaci včetně všech přidružených nasazovací sloty, třeba uživatel hello toouse hello parametr IncludeSourceWebAppSlots, hello následující příkaz prostředí PowerShell ukazuje hello použití tohoto parametru s hello AzureRmWebApp nový příkaz:</span><span class="sxs-lookup"><span data-stu-id="cdec1-115">tooclone an existing web app including all associated deployment slots, hello user will need toouse hello IncludeSourceWebAppSlots parameter, hello following PowerShell command demonstrates hello use of that parameter with hello New-AzureRmWebApp command:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

<span data-ttu-id="cdec1-116">tooclone stávající webovou aplikaci v rámci hello stejné oblasti, třeba uživatel hello toocreate novou skupinu prostředků a nové služby app service plánování v hello stejné oblasti a potom použitím hello následující prostředí PowerShell příkaz tooclone hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="cdec1-116">tooclone an existing web app within hello same region, hello user will need toocreate a new resource group and a new app service plan in hello same region, and then using hello following PowerShell command tooclone hello web app</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a><span data-ttu-id="cdec1-117">Klonování existující aplikace tooan App Service Environment</span><span class="sxs-lookup"><span data-stu-id="cdec1-117">Cloning an existing App tooan App Service Environment</span></span>
<span data-ttu-id="cdec1-118">Scénář: Stávající webovou aplikaci v oblasti jihu USA, hello uživatele chcete tooclone hello obsah tooa nové webové aplikace tooan stávající prostředí App Service (App Service Environment).</span><span class="sxs-lookup"><span data-stu-id="cdec1-118">Scenario: An existing web app in South Central US region, hello user would like tooclone hello contents tooa new web app tooan existing App Service Environment (ASE).</span></span>

<span data-ttu-id="cdec1-119">Znalost hello prostředků název skupiny, který obsahuje hello zdrojové webové aplikace, můžeme použít hello následující informace o prostředí PowerShell příkaz tooget hello zdrojové webové aplikace (v takovém případě s názvem zdrojového webapp):</span><span class="sxs-lookup"><span data-stu-id="cdec1-119">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="cdec1-120">Znalost hello App Service Environment je název a hello název skupiny prostředků, které patří hello App Service Environment, hello uživatele můžete použít příkaz hello New-AzureRmWebApp toocreate hello nové webové aplikace v App Service Environment existující hello, hello následující prokáže, že:</span><span class="sxs-lookup"><span data-stu-id="cdec1-120">Knowing hello ASE's name, and hello resource group name that hello ASE belongs to, hello user can use hello New-AzureRmWebApp command toocreate hello new web app in hello existing ASE, hello following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

<span data-ttu-id="cdec1-121">Hello umístění parametr je vyžadován kvůli toolegacy důvod, ale v případě hello vytváření aplikace v App Service Environment se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="cdec1-121">hello Location parameter is required due toolegacy reason, but in hello case of creating an app in an ASE it will be ignored.</span></span> 

## <a name="cloning-an-existing-app-slot"></a><span data-ttu-id="cdec1-122">Klonování existujícího slotu aplikace</span><span class="sxs-lookup"><span data-stu-id="cdec1-122">Cloning an existing App Slot</span></span>
<span data-ttu-id="cdec1-123">Scénář: hello uživatele chcete tooclone existující Slot webové aplikace tooeither novou webovou aplikaci nebo nový slot webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdec1-123">Scenario: hello user would like tooclone an existing Web App Slot tooeither a new Web App or a new Web App slot.</span></span> <span data-ttu-id="cdec1-124">Hello nové webové aplikace může být v hello stejné oblasti jako původní slot webové aplikace hello nebo v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="cdec1-124">hello new Web App can be in hello same region as hello original Web App slot or in a different region.</span></span>

<span data-ttu-id="cdec1-125">Znalost hello prostředků název skupiny, který obsahuje hello zdrojové webové aplikace, můžeme použít následující příkaz prostředí PowerShell tooget hello zdrojový slot webové aplikace na informace (v takovém případě s názvem zdrojového webappslot) vázaný zdroj aplikace tooWeb-webapp hello:</span><span class="sxs-lookup"><span data-stu-id="cdec1-125">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app slot's information (in this case named source-webappslot) tied tooWeb App source-webapp:</span></span>

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

<span data-ttu-id="cdec1-126">Následující Hello ukazuje vytvoření klonu hello zdrojové webové aplikace tooa nové webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="cdec1-126">hello following demonstrates creating a clone of hello source web app tooa new web app:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a><span data-ttu-id="cdec1-127">Konfigurace Traffic Manager během klonování aplikace</span><span class="sxs-lookup"><span data-stu-id="cdec1-127">Configuring Traffic Manager while cloning a App</span></span>
<span data-ttu-id="cdec1-128">Vytvoření více oblast webové aplikace a konfigurace Azure Traffic Manager tooroute provoz tooall tyto webové aplikace, je tooinsure n důležité scénář, který aplikace zákazníků jsou vysoce dostupný, když klonování stávající webovou aplikaci, kterou máte hello možnost tooconnect obou web aplikace tooeither nový profil správce provozu nebo stávající - Upozorňujeme, že pouze verze Azure Resource Manageru z Traffic Manager je podporována.</span><span class="sxs-lookup"><span data-stu-id="cdec1-128">Creating multi-region web apps and configuring Azure Traffic Manager tooroute traffic tooall these web apps, is a n important scenario tooinsure that customers' apps are highly available, when cloning an existing web app you have hello option tooconnect both web apps tooeither a new traffic manager profile or an existing one - note that only Azure Resource Manager version of Traffic Manager is supported.</span></span>

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a><span data-ttu-id="cdec1-129">Vytvoření nového profilu Traffic Manageru během klonování aplikace</span><span class="sxs-lookup"><span data-stu-id="cdec1-129">Creating a new Traffic Manager profile while cloning a App</span></span>
<span data-ttu-id="cdec1-130">Scénář: přeje hello uživatele tooclone tooanother oblast webové aplikace při konfiguraci profil správce provozu Azure Resource Manager, které zahrnují oba webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdec1-130">Scenario: hello user would like tooclone an web app tooanother region, while configuring an Azure Resource Manager traffic manager profile that include both web apps.</span></span> <span data-ttu-id="cdec1-131">Následující Hello ukazuje vytvoření klonu hello zdrojové webové aplikace tooa nové webové aplikace při konfiguraci nového profilu Traffic Manageru:</span><span class="sxs-lookup"><span data-stu-id="cdec1-131">hello following demonstrates creating a clone of hello source web app tooa new web app while configuring a new Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-tooan-existing-traffic-manager-profile"></a><span data-ttu-id="cdec1-132">Přidání nové klonovaný webové aplikace tooan existující profil služby Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cdec1-132">Adding new cloned Web App tooan existing Traffic Manager profile</span></span>
<span data-ttu-id="cdec1-133">Scénář: hello uživatele už máte profil správce provozu Azure Resource Manager, by chtěl tooadd i webové aplikace jako koncové body.</span><span class="sxs-lookup"><span data-stu-id="cdec1-133">Scenario: hello user already have an Azure Resource Manager traffic manager profile that he would like tooadd both web apps as endpoints.</span></span> <span data-ttu-id="cdec1-134">toodo Ano, je nejprve třeba tooassemble hello stávající id profilu Správce provozu, budeme potřebovat hello id odběru, název skupiny prostředků a název profilu manager stávající provoz hello.</span><span class="sxs-lookup"><span data-stu-id="cdec1-134">toodo so, we first need tooassemble hello existing traffic manager profile id, we will need hello subscription id, resource group name and hello existing traffic manager profile name.</span></span>

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

<span data-ttu-id="cdec1-135">Po s id Manager hello provoz, následující hello ukazuje vytvoření klonu hello zdrojové webové aplikace tooa nové webové aplikace při přidávání je tooan existující profil služby Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="cdec1-135">After having hello traffic manger id, hello following demonstrates creating a clone of hello source web app tooa new web app while adding them tooan existing Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a><span data-ttu-id="cdec1-136">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="cdec1-136">Current Restrictions</span></span>
<span data-ttu-id="cdec1-137">Tato funkce je aktuálně ve verzi preview, pracujeme tooadd nové funkce v čase, hello následujícím seznamu jsou hello známé omezení aktuální verze hello klonování aplikace:</span><span class="sxs-lookup"><span data-stu-id="cdec1-137">This feature is currently in preview, we are working tooadd new capabilities over time, hello following list are hello known restrictions on hello current version of app cloning:</span></span>

* <span data-ttu-id="cdec1-138">Nastavení automatického škálování nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="cdec1-138">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="cdec1-139">Nastavení plánu zálohování nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="cdec1-139">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="cdec1-140">Nastavení sítě VNET nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="cdec1-140">VNET settings are not cloned</span></span>
* <span data-ttu-id="cdec1-141">Přehled aplikace nejsou automaticky na nastavit hello cílové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="cdec1-141">App Insights are not automatically set up on hello destination web app</span></span>
* <span data-ttu-id="cdec1-142">Nejsou klonovat snadno nastavení ověřování</span><span class="sxs-lookup"><span data-stu-id="cdec1-142">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="cdec1-143">Kudu rozšíření nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="cdec1-143">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="cdec1-144">TiP pravidla nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="cdec1-144">TiP rules are not cloned</span></span>
* <span data-ttu-id="cdec1-145">Nejsou klonovat databáze obsahu</span><span class="sxs-lookup"><span data-stu-id="cdec1-145">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="cdec1-146">Odkazy</span><span class="sxs-lookup"><span data-stu-id="cdec1-146">References</span></span>
* [<span data-ttu-id="cdec1-147">Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="cdec1-147">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="cdec1-148">Webové aplikace klonování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cdec1-148">Web App Cloning using Azure Portal</span></span>](app-service-web-app-cloning-portal.md)
* [<span data-ttu-id="cdec1-149">Zálohování webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cdec1-149">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="cdec1-150">Azure Resource Manager podpora pro Azure Traffic Manager Preview</span><span class="sxs-lookup"><span data-stu-id="cdec1-150">Azure Resource Manager support for Azure Traffic Manager Preview</span></span>](../traffic-manager/traffic-manager-powershell-arm.md)
* [<span data-ttu-id="cdec1-151">Úvod tooApp Service Environment</span><span class="sxs-lookup"><span data-stu-id="cdec1-151">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="cdec1-152">Použití Azure PowerShellu s Azure Resource Managerem</span><span class="sxs-lookup"><span data-stu-id="cdec1-152">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

