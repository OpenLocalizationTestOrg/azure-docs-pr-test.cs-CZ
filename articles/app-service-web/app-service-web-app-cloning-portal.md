---
title: "Webové aplikace klonování pomocí portálu Azure"
description: "Zjistěte, jak vytvořit kopii vaší webové aplikace do nové webové aplikace pomocí portálu Azure."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a><span data-ttu-id="3d5b7-103">Aplikační služby Azure klonování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3d5b7-103">Azure App Service App Cloning Using Azure Portal</span></span>
<span data-ttu-id="3d5b7-104">Funkce klonování v [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) umožňuje snadno klonovat existující webové aplikace do nově vytvořené aplikace v jiné oblasti nebo ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-104">The cloning feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) lets you easily clone existing web apps to a newly created app in a different region or in the same region.</span></span> <span data-ttu-id="3d5b7-105">To vám umožní zákazníkům snadno a rychle nasadit počet aplikací v různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-105">This will enable customers to deploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="3d5b7-106">Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="3d5b7-107">Nová funkce používá stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="3d5b7-107">The new feature uses the same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a><span data-ttu-id="3d5b7-108">Klonování existující aplikace</span><span class="sxs-lookup"><span data-stu-id="3d5b7-108">Cloning an existing App</span></span>
<span data-ttu-id="3d5b7-109">Webové aplikace musí být spuštěna v **Premium** režimu v pořadí, můžete naklonoval pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-109">The web app must be running in the **Premium** mode in order for you to create a clone for the web app.</span></span>

1. <span data-ttu-id="3d5b7-110">V [portálu Azure](https://portal.azure.com/), otevřete okno vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-110">In the [Azure Portal](https://portal.azure.com/), open your web app's blade.</span></span>
2. <span data-ttu-id="3d5b7-111">Klikněte na tlačítko **nástroje**.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-111">Click **Tools**.</span></span> <span data-ttu-id="3d5b7-112">Potom v **nástroje** okně klikněte na tlačítko **klonování aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-112">Then, in the **Tools** blade, click **Clone App**.</span></span>
   
    ![][1]
   
   > [!NOTE]
   > <span data-ttu-id="3d5b7-113">Pokud není webová aplikace už v **Premium** režimu, obdržíte zprávu s upozorněním, podporované režimy pro aplikace klonování.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-113">If the web app is not already in the **Premium** mode, you will receive a message indicating the supported modes for app cloning.</span></span> <span data-ttu-id="3d5b7-114">Nyní máte možnost vybrat **upgradu**.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-114">At this point, you have the option to select **Upgrade**.</span></span>
   > 
   > 
3. <span data-ttu-id="3d5b7-115">V **klonování aplikace** okně zadejte název nové webové aplikace, skupinu prostředků a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-115">In the **Clone App** blade provide a name of the new web app, Resource Group, and App Service Plan.</span></span> <span data-ttu-id="3d5b7-116">Také uživatel bude moct vybrat, jestli klonovat počet zdroj nastavení webové aplikace nebo ne.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-116">Also the user will be able to choose whether to clone a number of source web app settings or not.</span></span>
   
    ![][2]
4. <span data-ttu-id="3d5b7-117">Po kliknutí na **vytvořit** platformou bude začít pracovat na vytvoření kopie zdrojové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-117">After clicking **create** the platform will start working on creating a clone of the source web app.</span></span>

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a><span data-ttu-id="3d5b7-118">Klonování stávající aplikace pro služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="3d5b7-118">Cloning an existing App to an App Service Environment</span></span>
<span data-ttu-id="3d5b7-119">V **klonování aplikace** okno zákazníka bude mít možnost Vybrat fond aplikací ve stávající App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-119">In the **Clone App** blade the customer will have the option to choose an app pool in an existing App Service Environment.</span></span>

## <a name="current-restrictions"></a><span data-ttu-id="3d5b7-120">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="3d5b7-120">Current Restrictions</span></span>
<span data-ttu-id="3d5b7-121">Tato funkce je aktuálně ve verzi preview, pracujeme na přidání nové funkce v čase, v následujícím seznamu jsou známé omezení pro aktuální podporu klonování aplikace v portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="3d5b7-121">This feature is currently in preview, we are working to add new capabilities over time, the following list are the known restrictions on the current support of app cloning in Azure Portal:</span></span>

* <span data-ttu-id="3d5b7-122">Nastavení Azure Traffic Manager nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-122">Azure Traffic Manager settings are not cloned</span></span>
* <span data-ttu-id="3d5b7-123">Nastavení automatického škálování nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-123">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="3d5b7-124">Nastavení plánu zálohování nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-124">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="3d5b7-125">Nastavení sítě VNET nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-125">VNET settings are not cloned</span></span>
* <span data-ttu-id="3d5b7-126">Přehled aplikace nejsou automaticky na nastavit cílové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3d5b7-126">App Insights are not automatically set up on the destination web app</span></span>
* <span data-ttu-id="3d5b7-127">Nejsou klonovat snadno nastavení ověřování</span><span class="sxs-lookup"><span data-stu-id="3d5b7-127">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="3d5b7-128">Kudu rozšíření nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-128">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="3d5b7-129">TiP pravidla nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="3d5b7-129">TiP rules are not cloned</span></span>
* <span data-ttu-id="3d5b7-130">Nejsou klonovat databáze obsahu</span><span class="sxs-lookup"><span data-stu-id="3d5b7-130">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="3d5b7-131">Odkazy</span><span class="sxs-lookup"><span data-stu-id="3d5b7-131">References</span></span>
* [<span data-ttu-id="3d5b7-132">Webové aplikace klonování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3d5b7-132">Web App Cloning using PowerShell</span></span>](app-service-web-app-cloning.md)
* [<span data-ttu-id="3d5b7-133">Zálohování webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3d5b7-133">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="3d5b7-134">Způsob vytvoření prostředí App Service Environment</span><span class="sxs-lookup"><span data-stu-id="3d5b7-134">How to Create an App Service Environment</span></span>](app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="3d5b7-135">Vytvoření webové aplikace v App Service Environment</span><span class="sxs-lookup"><span data-stu-id="3d5b7-135">Create a web app in an App Service Environment</span></span>](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="3d5b7-136">Úvod do prostředí App Service</span><span class="sxs-lookup"><span data-stu-id="3d5b7-136">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
