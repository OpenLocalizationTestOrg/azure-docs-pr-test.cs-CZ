---
title: "Použití io.js s aplikacemi Azure App Service Web Apps"
description: "Naučte se používat s io.js webové aplikace v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 4800504e1939a46842d15e8c9d4279a4b9cae787
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a><span data-ttu-id="c2410-103">Použití io.js s aplikacemi Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="c2410-103">How to use io.js with Azure App Service Web Apps</span></span>
<span data-ttu-id="c2410-104">Oblíbené rozvětvení uzlu [io.js] funkce různých rozdíly do projektu na Joyent Node.js, včetně více otevřete modelu zásad správného řízení, rychlejší cyklus vydání a rychlejší přijetí nové a povolenými experimentálními funkcemi JavaScriptu.</span><span class="sxs-lookup"><span data-stu-id="c2410-104">The popular Node fork [io.js] features various differences to Joyent's Node.js project, including a more open governance model, a faster release cycle and a faster adoption of new and experimental JavaScript features.</span></span>

<span data-ttu-id="c2410-105">Při [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace má mnoho Node.js verze předinstalována, umožňuje také Node.js binárního souboru zadaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c2410-105">While [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps has many Node.js versions preinstalled, it also allows for an user-provided Node.js binary.</span></span> <span data-ttu-id="c2410-106">Tento článek popisuje dvě metody, které umožňuje použití io.js v App Service Web Apps: použití rozšířeného nasazení skriptu, který automaticky nakonfiguruje Azure a použít nejnovější verzi k dispozici io.js, jakož i ruční odesílání io.js binární.</span><span class="sxs-lookup"><span data-stu-id="c2410-106">This article discusses two methods enabling the use of io.js on App Service Web Apps: The use of an extended deployment script, which automatically configures Azure to use the latest available io.js version, as well as the manual upload of a io.js binary.</span></span> 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a><span data-ttu-id="c2410-107">Pomocí skriptu nasazení</span><span class="sxs-lookup"><span data-stu-id="c2410-107">Using a Deployment Script</span></span>
<span data-ttu-id="c2410-108">Při nasazení aplikace Node.js spustí App Service Web Apps počet malých příkazů prostředí je třeba nastavit správně.</span><span class="sxs-lookup"><span data-stu-id="c2410-108">Upon deployment of a Node.js app, App Service Web Apps runs a number of small commands to ensure that the environment is configured properly.</span></span> <span data-ttu-id="c2410-109">Pomocí skriptu nasazení, tento proces můžete upravit, aby obsahoval stažení a konfiguraci io.js.</span><span class="sxs-lookup"><span data-stu-id="c2410-109">Using a deployment script, this process can be customized to include the download and configuration of io.js.</span></span>

<span data-ttu-id="c2410-110">[Io.js skript nasazení](https://github.com/felixrieseberg/iojs-azure) je dostupná na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c2410-110">The [io.js Deployment Script](https://github.com/felixrieseberg/iojs-azure) is available on GitHub.</span></span> <span data-ttu-id="c2410-111">Pokud chcete povolit io.js na vaší webové aplikace, jednoduše zkopírovat **.deployment**, **deploy.cmd** a **IISNode.yml** kořenové složky vaší aplikace a nasazení do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2410-111">To enable io.js on your web app, simply copy **.deployment**, **deploy.cmd** and **IISNode.yml** to the root of your application folder and deploy to Web Apps.</span></span>  

<span data-ttu-id="c2410-112">První soubor **.deployment**, dá pokyn ke spuštění webové aplikace **deploy.cmd** po nasazení.</span><span class="sxs-lookup"><span data-stu-id="c2410-112">The first file, **.deployment**, instructs Web Apps to run **deploy.cmd** upon deployment.</span></span> <span data-ttu-id="c2410-113">Tento skript spustí všechny obvyklé kroky pro aplikaci Node.js, ale také soubory ke stažení nejnovější verze io.js.</span><span class="sxs-lookup"><span data-stu-id="c2410-113">This script runs all the usual steps for a Node.js application, but also downloads the latest version of io.js.</span></span> <span data-ttu-id="c2410-114">Nakonec **IISNode.yml** nakonfiguruje webové aplikace mohly používat jenom stažené io.js binární místo předem nainstalovaná binární Node.js.</span><span class="sxs-lookup"><span data-stu-id="c2410-114">Finally, **IISNode.yml** configures Web Apps to use just the downloaded io.js binary instead of a pre-installed Node.js binary.</span></span>

> [!NOTE]
> <span data-ttu-id="c2410-115">Aktualizovat binární použité io.js, právě znovu nasaďte aplikaci - stáhne skript novou verzi io.js každých jednorázově, které je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="c2410-115">To update the used io.js binary, just redeploy your application - the script will download a new version of io.js every single time the application is deployed.</span></span>
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a><span data-ttu-id="c2410-116">Pomocí ruční instalace</span><span class="sxs-lookup"><span data-stu-id="c2410-116">Using Manual Installation</span></span>
<span data-ttu-id="c2410-117">Ruční instalace verze vlastní io.js obsahuje pouze dva kroky.</span><span class="sxs-lookup"><span data-stu-id="c2410-117">The manual installation of a custom io.js version includes only two steps.</span></span> <span data-ttu-id="c2410-118">Nejprve stáhnout **win-x64** binární přímo z [io.js distribuční].</span><span class="sxs-lookup"><span data-stu-id="c2410-118">First, download the **win-x64** binary directly from the [io.js distribution].</span></span> <span data-ttu-id="c2410-119">Vyžaduje se dva soubory - **iojs.exe** a **iojs.lib**.</span><span class="sxs-lookup"><span data-stu-id="c2410-119">Required are two files - **iojs.exe** and **iojs.lib**.</span></span> <span data-ttu-id="c2410-120">Uložit oba soubory do složky uvnitř vaší webové aplikaci, například v **bin/iojs**.</span><span class="sxs-lookup"><span data-stu-id="c2410-120">Save both files to a folder inside your web app, for example in **bin/iojs**.</span></span>

<span data-ttu-id="c2410-121">Konfigurace webové aplikace mohly používat **iojs.exe** místo předem nainstalovaná verze uzlu, vytvoření **IISNode.yml** souboru v kořenovém adresáři aplikace a přidejte následující řádek.</span><span class="sxs-lookup"><span data-stu-id="c2410-121">To configure Web Apps to use **iojs.exe** instead of a pre-installed Node version, create a **IISNode.yml** file at the root of your application and add the following line.</span></span>

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="c2410-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2410-122">Next Steps</span></span>
<span data-ttu-id="c2410-123">V tomto článku jste se dozvěděli, jak používat k dispozici io.js s App Service Web Apps, jak pomocí skriptů nasazení také ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="c2410-123">In this article you learned how to use io.js with App Service Web Apps, using both provided deployment scripts as well as manual installation.</span></span> 

> [!NOTE]
> <span data-ttu-id="c2410-124">IO.js se velkou vývoj a aktualizován častěji než Node.js.</span><span class="sxs-lookup"><span data-stu-id="c2410-124">io.js is in heavy development and updated more frequently than Node.js.</span></span> <span data-ttu-id="c2410-125">Počet modulů Node.js nemusí pracovat io.js - prosím naleznete [io.js na Githubu] pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="c2410-125">A number of Node.js modules might not work with io.js - please consult [io.js on GitHub] for troubleshooting.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="c2410-126">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="c2410-126">What's changed</span></span>
* <span data-ttu-id="c2410-127">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="c2410-127">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="c2410-128">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c2410-128">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c2410-129">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="c2410-129">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="c2410-130">[io.js]: https://iojs.org</span><span class="sxs-lookup"><span data-stu-id="c2410-130">[io.js]: https://iojs.org</span></span>
<span data-ttu-id="c2410-131">[io.js distribuční]: https://iojs.org/dist/</span><span class="sxs-lookup"><span data-stu-id="c2410-131">[io.js distribution]: https://iojs.org/dist/</span></span>
<span data-ttu-id="c2410-132">[io.js na Githubu]: https://github.com/iojs/io.js</span><span class="sxs-lookup"><span data-stu-id="c2410-132">[io.js on GitHub]: https://github.com/iojs/io.js</span></span>
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
