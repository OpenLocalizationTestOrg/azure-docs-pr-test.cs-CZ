---
title: "aaaWeb klonování aplikace pomocí portálu Azure"
description: "Zjistěte, jak tooclone toonew vaší webové aplikace webových aplikací pomocí portálu Azure."
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
ms.openlocfilehash: 605c4879f34d568e9981c34109f9496731c9ed18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a><span data-ttu-id="9ca8c-103">Aplikační služby Azure klonování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9ca8c-103">Azure App Service App Cloning Using Azure Portal</span></span>
<span data-ttu-id="9ca8c-104">funkce v klonování Hello [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) hello umožňuje snadno klonovat existující webové aplikace tooa nově vytvořený aplikace v jiné oblasti nebo ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-104">hello cloning feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) lets you easily clone existing web apps tooa newly created app in a different region or in hello same region.</span></span> <span data-ttu-id="9ca8c-105">Tím povolíte zákazníci toodeploy počet aplikací v různých oblastech snadno a rychle.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-105">This will enable customers toodeploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="9ca8c-106">Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="9ca8c-107">nové funkce používá Hello hello stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="9ca8c-107">hello new feature uses hello same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a><span data-ttu-id="9ca8c-108">Klonování existující aplikace</span><span class="sxs-lookup"><span data-stu-id="9ca8c-108">Cloning an existing App</span></span>
<span data-ttu-id="9ca8c-109">Hello webové aplikace musí být spuštěna v hello **Premium** režimu, aby toocreate klon pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-109">hello web app must be running in hello **Premium** mode in order for you toocreate a clone for hello web app.</span></span>

1. <span data-ttu-id="9ca8c-110">V hello [portálu Azure](https://portal.azure.com/), otevřete okno vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-110">In hello [Azure Portal](https://portal.azure.com/), open your web app's blade.</span></span>
2. <span data-ttu-id="9ca8c-111">Klikněte na tlačítko **nástroje**.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-111">Click **Tools**.</span></span> <span data-ttu-id="9ca8c-112">Potom v hello **nástroje** okně klikněte na tlačítko **klonování aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-112">Then, in hello **Tools** blade, click **Clone App**.</span></span>
   
    ![][1]
   
   > [!NOTE]
   > <span data-ttu-id="9ca8c-113">Pokud hello webové aplikace ještě není v hello **Premium** režimu, obdržíte zprávu s upozorněním hello podporované režimy pro aplikace klonování.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-113">If hello web app is not already in hello **Premium** mode, you will receive a message indicating hello supported modes for app cloning.</span></span> <span data-ttu-id="9ca8c-114">Nyní máte možnost tooselect hello **upgradu**.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-114">At this point, you have hello option tooselect **Upgrade**.</span></span>
   > 
   > 
3. <span data-ttu-id="9ca8c-115">V hello **klonování aplikace** okně zadejte název nové webové aplikace hello, skupinu prostředků a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-115">In hello **Clone App** blade provide a name of hello new web app, Resource Group, and App Service Plan.</span></span> <span data-ttu-id="9ca8c-116">Také hello uživatel se bude moct toochoose jestli tooclone počet zdroj nastavení webové aplikace nebo ne.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-116">Also hello user will be able toochoose whether tooclone a number of source web app settings or not.</span></span>
   
    ![][2]
4. <span data-ttu-id="9ca8c-117">Po kliknutí na **vytvořit** hello platformy bude začít pracovat na vytvoření klonu hello zdrojové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-117">After clicking **create** hello platform will start working on creating a clone of hello source web app.</span></span>

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a><span data-ttu-id="9ca8c-118">Klonování existující aplikace tooan App Service Environment</span><span class="sxs-lookup"><span data-stu-id="9ca8c-118">Cloning an existing App tooan App Service Environment</span></span>
<span data-ttu-id="9ca8c-119">V hello **klonování aplikace** okno hello zákazníka bude mít možnost toochoose hello fondem aplikací ve stávající App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-119">In hello **Clone App** blade hello customer will have hello option toochoose an app pool in an existing App Service Environment.</span></span>

## <a name="current-restrictions"></a><span data-ttu-id="9ca8c-120">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="9ca8c-120">Current Restrictions</span></span>
<span data-ttu-id="9ca8c-121">Tato funkce je aktuálně ve verzi preview, pracujeme tooadd nové funkce v čase, hello následujícím seznamu jsou hello známé omezení podpory aktuální hello klonování aplikace na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="9ca8c-121">This feature is currently in preview, we are working tooadd new capabilities over time, hello following list are hello known restrictions on hello current support of app cloning in Azure Portal:</span></span>

* <span data-ttu-id="9ca8c-122">Nastavení Azure Traffic Manager nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-122">Azure Traffic Manager settings are not cloned</span></span>
* <span data-ttu-id="9ca8c-123">Nastavení automatického škálování nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-123">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="9ca8c-124">Nastavení plánu zálohování nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-124">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="9ca8c-125">Nastavení sítě VNET nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-125">VNET settings are not cloned</span></span>
* <span data-ttu-id="9ca8c-126">Přehled aplikace nejsou automaticky na nastavit hello cílové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9ca8c-126">App Insights are not automatically set up on hello destination web app</span></span>
* <span data-ttu-id="9ca8c-127">Nejsou klonovat snadno nastavení ověřování</span><span class="sxs-lookup"><span data-stu-id="9ca8c-127">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="9ca8c-128">Kudu rozšíření nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-128">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="9ca8c-129">TiP pravidla nejsou klonovat.</span><span class="sxs-lookup"><span data-stu-id="9ca8c-129">TiP rules are not cloned</span></span>
* <span data-ttu-id="9ca8c-130">Nejsou klonovat databáze obsahu</span><span class="sxs-lookup"><span data-stu-id="9ca8c-130">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="9ca8c-131">Odkazy</span><span class="sxs-lookup"><span data-stu-id="9ca8c-131">References</span></span>
* [<span data-ttu-id="9ca8c-132">Webové aplikace klonování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ca8c-132">Web App Cloning using PowerShell</span></span>](app-service-web-app-cloning.md)
* [<span data-ttu-id="9ca8c-133">Zálohování webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9ca8c-133">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="9ca8c-134">Jak tooCreate služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="9ca8c-134">How tooCreate an App Service Environment</span></span>](app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="9ca8c-135">Vytvoření webové aplikace v App Service Environment</span><span class="sxs-lookup"><span data-stu-id="9ca8c-135">Create a web app in an App Service Environment</span></span>](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="9ca8c-136">Úvod tooApp Service Environment</span><span class="sxs-lookup"><span data-stu-id="9ca8c-136">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
