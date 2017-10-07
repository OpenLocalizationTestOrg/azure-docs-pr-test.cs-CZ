---
title: aaaHow toouse io.js s Azure App Service Web Apps
description: "Zjistěte, jak toouse webové aplikace v Azure App Service přes io.js."
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
ms.openlocfilehash: 5dfdac37546b36bc91ab43d9e0a39c2126b4fa9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-iojs-with-azure-app-service-web-apps"></a><span data-ttu-id="0ec22-103">Jak toouse io.js s Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="0ec22-103">How toouse io.js with Azure App Service Web Apps</span></span>
<span data-ttu-id="0ec22-104">Hello oblíbených uzlu rozvětvení [io.js] funkce projektu různé rozdíly tooJoyent Node.js, včetně více otevřete modelu zásad správného řízení, rychlejší cyklus vydání a rychlejší přijetí nové a povolenými experimentálními funkcemi JavaScriptu.</span><span class="sxs-lookup"><span data-stu-id="0ec22-104">hello popular Node fork [io.js] features various differences tooJoyent's Node.js project, including a more open governance model, a faster release cycle and a faster adoption of new and experimental JavaScript features.</span></span>

<span data-ttu-id="0ec22-105">Při [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace má mnoho Node.js verze předinstalována, umožňuje také Node.js binárního souboru zadaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="0ec22-105">While [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps has many Node.js versions preinstalled, it also allows for an user-provided Node.js binary.</span></span> <span data-ttu-id="0ec22-106">Tento článek popisuje dvě metody povolení hello použití io.js v App Service Web Apps: hello použití rozšířeného nasazení skriptu, který automaticky nakonfiguruje Azure toouse hello nejnovější verzi k dispozici io.js, jakož i hello ruční odeslání io.js binární.</span><span class="sxs-lookup"><span data-stu-id="0ec22-106">This article discusses two methods enabling hello use of io.js on App Service Web Apps: hello use of an extended deployment script, which automatically configures Azure toouse hello latest available io.js version, as well as hello manual upload of a io.js binary.</span></span> 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a><span data-ttu-id="0ec22-107">Pomocí skriptu nasazení</span><span class="sxs-lookup"><span data-stu-id="0ec22-107">Using a Deployment Script</span></span>
<span data-ttu-id="0ec22-108">Při nasazení aplikace Node.js spustí App Service Web Apps počet malých příkazy, které tooensure, který hello prostředí je správně nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="0ec22-108">Upon deployment of a Node.js app, App Service Web Apps runs a number of small commands tooensure that hello environment is configured properly.</span></span> <span data-ttu-id="0ec22-109">Pomocí skriptu nasazení, tento proces může být vlastní tooinclude hello stažení a konfiguraci io.js.</span><span class="sxs-lookup"><span data-stu-id="0ec22-109">Using a deployment script, this process can be customized tooinclude hello download and configuration of io.js.</span></span>

<span data-ttu-id="0ec22-110">Hello [io.js skript nasazení](https://github.com/felixrieseberg/iojs-azure) je dostupná na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0ec22-110">hello [io.js Deployment Script](https://github.com/felixrieseberg/iojs-azure) is available on GitHub.</span></span> <span data-ttu-id="0ec22-111">tooenable io.js na vaší webové aplikace, jednoduše zkopírovat **.deployment**, **deploy.cmd** a **IISNode.yml** toohello kořenové složky vaší aplikace a nasazovat aplikace tooWeb.</span><span class="sxs-lookup"><span data-stu-id="0ec22-111">tooenable io.js on your web app, simply copy **.deployment**, **deploy.cmd** and **IISNode.yml** toohello root of your application folder and deploy tooWeb Apps.</span></span>  

<span data-ttu-id="0ec22-112">první soubor Hello, **.deployment**, dá pokyn webové aplikace toorun **deploy.cmd** po nasazení.</span><span class="sxs-lookup"><span data-stu-id="0ec22-112">hello first file, **.deployment**, instructs Web Apps toorun **deploy.cmd** upon deployment.</span></span> <span data-ttu-id="0ec22-113">Tento skript spustí všechny hello obvyklé kroky pro aplikaci Node.js, ale také stáhne nejnovější verzi io.js hello.</span><span class="sxs-lookup"><span data-stu-id="0ec22-113">This script runs all hello usual steps for a Node.js application, but also downloads hello latest version of io.js.</span></span> <span data-ttu-id="0ec22-114">Nakonec **IISNode.yml** nakonfiguruje Web Apps toouse jenom hello stáhnout io.js binární místo předem nainstalovaná binární Node.js.</span><span class="sxs-lookup"><span data-stu-id="0ec22-114">Finally, **IISNode.yml** configures Web Apps toouse just hello downloaded io.js binary instead of a pre-installed Node.js binary.</span></span>

> [!NOTE]
> <span data-ttu-id="0ec22-115">tooupdate hello používá io.js binární, právě znovu nasaďte aplikaci – hello skript stáhne novou verzi io.js, které je každá aplikace hello jednorázově nasadili.</span><span class="sxs-lookup"><span data-stu-id="0ec22-115">tooupdate hello used io.js binary, just redeploy your application - hello script will download a new version of io.js every single time hello application is deployed.</span></span>
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a><span data-ttu-id="0ec22-116">Pomocí ruční instalace</span><span class="sxs-lookup"><span data-stu-id="0ec22-116">Using Manual Installation</span></span>
<span data-ttu-id="0ec22-117">Ruční instalace Hello vlastní io.js verze obsahuje pouze dva kroky.</span><span class="sxs-lookup"><span data-stu-id="0ec22-117">hello manual installation of a custom io.js version includes only two steps.</span></span> <span data-ttu-id="0ec22-118">Nejprve stáhnout hello **win-x64** binární přímo z hello [io.js distribuční].</span><span class="sxs-lookup"><span data-stu-id="0ec22-118">First, download hello **win-x64** binary directly from hello [io.js distribution].</span></span> <span data-ttu-id="0ec22-119">Vyžaduje se dva soubory - **iojs.exe** a **iojs.lib**.</span><span class="sxs-lookup"><span data-stu-id="0ec22-119">Required are two files - **iojs.exe** and **iojs.lib**.</span></span> <span data-ttu-id="0ec22-120">Uložit oba soubory tooa složky uvnitř vaší webové aplikaci, například v **bin/iojs**.</span><span class="sxs-lookup"><span data-stu-id="0ec22-120">Save both files tooa folder inside your web app, for example in **bin/iojs**.</span></span>

<span data-ttu-id="0ec22-121">Webové aplikace toouse tooconfigure **iojs.exe** místo předem nainstalovaná verze uzlu, vytvořte **IISNode.yml** souboru v kořenovém adresáři aplikace hello a přidejte následující řádek hello.</span><span class="sxs-lookup"><span data-stu-id="0ec22-121">tooconfigure Web Apps toouse **iojs.exe** instead of a pre-installed Node version, create a **IISNode.yml** file at hello root of your application and add hello following line.</span></span>

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="0ec22-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ec22-122">Next Steps</span></span>
<span data-ttu-id="0ec22-123">V tomto článku jste se dozvěděli, jak zadat toouse io.js s App Service Web Apps, jak pomocí skriptů nasazení a také ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="0ec22-123">In this article you learned how toouse io.js with App Service Web Apps, using both provided deployment scripts as well as manual installation.</span></span> 

> [!NOTE]
> <span data-ttu-id="0ec22-124">IO.js se velkou vývoj a aktualizován častěji než Node.js.</span><span class="sxs-lookup"><span data-stu-id="0ec22-124">io.js is in heavy development and updated more frequently than Node.js.</span></span> <span data-ttu-id="0ec22-125">Počet modulů Node.js nemusí pracovat io.js - prosím naleznete [io.js na Githubu] pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0ec22-125">A number of Node.js modules might not work with io.js - please consult [io.js on GitHub] for troubleshooting.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="0ec22-126">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="0ec22-126">What's changed</span></span>
* <span data-ttu-id="0ec22-127">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="0ec22-127">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="0ec22-128">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="0ec22-128">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="0ec22-129">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="0ec22-129">No credit cards required; no commitments.</span></span>
> 
> 

[io.js]: https://iojs.org
[io.js distribuční]: https://iojs.org/dist/
[io.js na Githubu]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
