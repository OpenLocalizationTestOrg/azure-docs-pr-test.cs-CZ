---
title: aaaSpecifying verze Node.js
description: "Zjistěte, jak toospecify hello verze Node.js používané weby Azure a cloudové služby"
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 09c27bfc43c132b6d66f9a2943179e06ee75bedc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a><span data-ttu-id="4aa4e-103">Určení verze Node.js v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="4aa4e-103">Specifying a Node.js version in an Azure application</span></span>
<span data-ttu-id="4aa4e-104">Při hostování aplikace Node.js, může být vhodné tooensure, kterou vaše aplikace používá na konkrétní verzi Node.js.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-104">When hosting a Node.js application, you may want tooensure that your application uses a specific version of Node.js.</span></span> <span data-ttu-id="4aa4e-105">Existuje několik způsobů tooaccomplish to pro aplikace hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-105">There are several ways tooaccomplish this for applications hosted on Azure.</span></span>

## <a name="default-versions"></a><span data-ttu-id="4aa4e-106">Výchozí verze</span><span class="sxs-lookup"><span data-stu-id="4aa4e-106">Default versions</span></span>
<span data-ttu-id="4aa4e-107">verze Node.js Hello poskytovaný platformou Azure jsou neustále aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-107">hello Node.js versions provided by Azure are constantly updated.</span></span> <span data-ttu-id="4aa4e-108">Pokud není uvedeno jinak, hello výchozí verze, který je uveden v hello `WEBSITE_NODE_DEFAULT_VERSION` proměnné prostředí se použije.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-108">Unless otherwise specified, hello default version that is specified in hello `WEBSITE_NODE_DEFAULT_VERSION` environment variable will be used.</span></span> <span data-ttu-id="4aa4e-109">toooverride tuto výchozí hodnotu, postupujte podle kroků hello v následující části tohoto článku</span><span class="sxs-lookup"><span data-stu-id="4aa4e-109">toooverride this default value, follow hello steps in following sections of this article</span></span>

> [!NOTE]
> <span data-ttu-id="4aa4e-110">Pokud je hostitelem vaší aplikace v cloudové službě Azure (role web nebo worker) a je hello poprvé jste nasadili aplikaci hello, Azure pokusí toouse hello stejnou verzi Node.js, jako jste nainstalovali na vývojového prostředí, pokud ji odpovídá jednomu z verze výchozí hello k dispozici v Azure.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-110">If you are hosting your application in an Azure Cloud Service (web or worker role,) and it is hello first time you have deployed hello application, Azure will attempt toouse hello same version of Node.js as you have installed on your development environment if it matches one of hello default versions available on Azure.</span></span>
>
>

## <a name="versioning-with-packagejson"></a><span data-ttu-id="4aa4e-111">Správa verzí pomocí package.json</span><span class="sxs-lookup"><span data-stu-id="4aa4e-111">Versioning with package.json</span></span>
<span data-ttu-id="4aa4e-112">Můžete zadat hello verze Node.js toobe použít přidáním hello následující tooyour **package.json** souboru:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-112">You can specify hello version of Node.js toobe used by adding hello following tooyour **package.json** file:</span></span>

    "engines":{"node":version}

<span data-ttu-id="4aa4e-113">Kde *verze* je číslo toouse hello konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-113">Where *version* is hello specific version number toouse.</span></span> <span data-ttu-id="4aa4e-114">Složitější podmínky pro verzi, můžete zadat například:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-114">You can specify more complex conditions for version, such as:</span></span>

    "engines":{"node": "0.6.22 || 0.8.x"}

<span data-ttu-id="4aa4e-115">Vzhledem k tomu, že 0.6.22 není některá z verzí hello k dispozici v hello hostování prostředí, hello nejvyšší verzi hello 0,8 série, která je k dispozici bude místo toho použít - 0.8.4.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-115">Since 0.6.22 is not one of hello versions available in hello hosting environment, hello highest version of hello 0.8 series that is available will be used instead - 0.8.4.</span></span>

## <a name="versioning-websites-with-app-settings"></a><span data-ttu-id="4aa4e-116">Správa verzí webů s nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="4aa4e-116">Versioning Websites with App Settings</span></span>
<span data-ttu-id="4aa4e-117">Pokud hostujete hello aplikace na webu, můžete nastavit proměnné prostředí hello **WEBSITE_NODE_DEFAULT_VERSION** toohello požadovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-117">If you are hosting hello application in a Website, you can set hello environment variable **WEBSITE_NODE_DEFAULT_VERSION** toohello desired version.</span></span>

## <a name="versioning-cloud-services-with-powershell"></a><span data-ttu-id="4aa4e-118">Správa verzí cloudové služby pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4aa4e-118">Versioning Cloud Services with PowerShell</span></span>
<span data-ttu-id="4aa4e-119">Pokud jsou hostiteli hello aplikace v cloudové službě a nasazení aplikace hello pomocí Azure PowerShell, můžete přepsat hello výchozí Node.js verze pomocí hello **Set-AzureServiceProjectRole** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-119">If you are hosting hello application in a Cloud Service, and are deploying hello application using Azure PowerShell, you can override hello default Node.js version by using hello **Set-AzureServiceProjectRole** PowerShell cmdlet.</span></span> <span data-ttu-id="4aa4e-120">Například:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-120">For example:</span></span>

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

<span data-ttu-id="4aa4e-121">Poznámka hello parametry v hello výše příkaz rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-121">Note hello parameters in hello above statement are case-sensitive.</span></span>  <span data-ttu-id="4aa4e-122">Můžete ověřit kontrolou hello byla vybrána správná verze Node.js hello **moduly** vlastnost vaší role **package.json**.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-122">You can verify hello correct version of Node.js has been selected by checking hello **engines** property in your role's **package.json**.</span></span>

<span data-ttu-id="4aa4e-123">Můžete taky hello **Get-AzureServiceProjectRoleRuntime** tooretrieve seznam Node.js verze, které jsou k dispozici pro aplikace hostované jako cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-123">You can also use hello **Get-AzureServiceProjectRoleRuntime** tooretrieve a list of Node.js versions available for applications hosted as a Cloud Service.</span></span>  <span data-ttu-id="4aa4e-124">Vždy ověřte, zda text hello verze Node.js projektu závisí na je v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-124">Always verify hello version of Node.js your project depends on is in this list.</span></span>

## <a name="using-a-custom-version-with-azure-websites"></a><span data-ttu-id="4aa4e-125">Použití vlastní verzi s weby Azure</span><span class="sxs-lookup"><span data-stu-id="4aa4e-125">Using a custom version with Azure Websites</span></span>
<span data-ttu-id="4aa4e-126">Zatímco Azure poskytuje několik verzí výchozí Node.js, může být vhodné toouse na verzi, která není uvedena ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-126">While Azure provides several default versions of Node.js, you may want toouse a version that is not provided by default.</span></span> <span data-ttu-id="4aa4e-127">Pokud je vaše aplikace hostována jako web Azure, můžete dosáhnout pomocí hello **iisnode.yml** souboru.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-127">If your application is hosted as an Azure Website, you can accomplish this by using hello **iisnode.yml** file.</span></span> <span data-ttu-id="4aa4e-128">Hello následující kroky provede hello proces vlastní verzi Node.Js pomocí webu Azure:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-128">hello following steps walk through hello process of using a custom version of Node.Js with an Azure Website:</span></span>

1. <span data-ttu-id="4aa4e-129">Vytvořte nový adresář a pak vytvořte **server.js** soubor v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-129">Create a new directory, and then create a **server.js** file within hello directory.</span></span> <span data-ttu-id="4aa4e-130">Hello **server.js** soubor by měl obsahovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-130">hello **server.js** file should contain hello following:</span></span>

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    <span data-ttu-id="4aa4e-131">Tato akce zobrazí verze Node.js hello používá při procházení webu hello.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-131">This will display hello Node.js version being used when you browse hello website.</span></span>
2. <span data-ttu-id="4aa4e-132">Vytvořte nový web a Poznámka hello název lokality hello.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-132">Create a new Website and note hello name of hello site.</span></span> <span data-ttu-id="4aa4e-133">Například následující hello používá hello [nástroje příkazového řádku Azure] toocreate nový web Azure s názvem **mywebsite**a pak povolte úložiště Git pro web hello.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-133">For example, hello following uses hello [Azure Command-line tools] toocreate a new Azure Website named **mywebsite**, and then enable a Git repository for hello website.</span></span>

        azure site create mywebsite --git
3. <span data-ttu-id="4aa4e-134">Vytvořte nový adresář s názvem **bin** jako podřízenou hello adresář obsahující hello **server.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-134">Create a new directory named **bin** as a child of hello directory containing hello **server.js** file.</span></span>
4. <span data-ttu-id="4aa4e-135">Stáhnout hello konkrétní verzi nástroje **node.exe** (verze Windows hello) chcete toouse s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-135">Download hello specific version of **node.exe** (hello Windows version) that you wish toouse with your application.</span></span> <span data-ttu-id="4aa4e-136">Například následující používá hello **curl** toodownload verze 0.8.1:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-136">For example, hello following uses **curl** toodownload version 0.8.1:</span></span>

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    <span data-ttu-id="4aa4e-137">Uložit hello **node.exe** souboru do hello **bin** složky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-137">Save hello **node.exe** file into hello **bin** folder created previously.</span></span>
5. <span data-ttu-id="4aa4e-138">Vytvoření **iisnode.yml** souboru v hello stejný adresář jako hello **server.js** souboru a poté přidejte následující obsahu toohello hello **iisnode.yml** souboru:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-138">Create an **iisnode.yml** file in hello same directory as hello **server.js** file, and then add hello following content toohello **iisnode.yml** file:</span></span>

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    <span data-ttu-id="4aa4e-139">Tato cesta je, kde hello **node.exe** soubor v projektu budou umístěné, když jste publikovali vaší aplikace toohello webu Azure.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-139">This path is where hello **node.exe** file within your project will be located once you have published your application toohello Azure Website.</span></span>
6. <span data-ttu-id="4aa4e-140">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-140">Publish your application.</span></span> <span data-ttu-id="4aa4e-141">Například vzhledem k tomu, že jste vytvořili nový web pomocí parametru – git hello dříve, hello následující příkazy přidáte hello aplikace soubory toomy místní úložiště Git a vložit je toohello webu úložiště:</span><span class="sxs-lookup"><span data-stu-id="4aa4e-141">For example, since I created a new website with hello --git parameter earlier, hello following commands will add hello application files toomy local Git repository, and then push them toohello website repository:</span></span>

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    <span data-ttu-id="4aa4e-142">Vydaného aplikace hello otevřete hello web v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4aa4e-142">After hello application has published, open hello website in a browser.</span></span> <span data-ttu-id="4aa4e-143">Měla by se zobrazit zpráva s oznámením "Hello z Azure spuštěné verze uzlu: v0.8.1".</span><span class="sxs-lookup"><span data-stu-id="4aa4e-143">You should see a message stating "Hello from Azure running node version: v0.8.1".</span></span>

## <a name="next-steps"></a><span data-ttu-id="4aa4e-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4aa4e-144">Next Steps</span></span>
<span data-ttu-id="4aa4e-145">Teď, když budete rozumět tomu, jak se používají toospecify hello verze Node.js v aplikaci, zjistěte, jak příliš[práce s moduly], [sestavení a nasazení webu Node.js](app-service-web/app-service-web-get-started-nodejs.md), a [jak toouse hello Azure Nástroje příkazového řádku pro Mac a Linux].</span><span class="sxs-lookup"><span data-stu-id="4aa4e-145">Now that you understand how toospecify hello version of Node.js used by your application, learn how too[work with modules], [build and deploy a Node.js Web Site](app-service-web/app-service-web-get-started-nodejs.md), and [How toouse hello Azure Command-Line Tools for Mac and Linux].</span></span>

<span data-ttu-id="4aa4e-146">Další informace najdete v tématu hello [středisku pro vývojáře Node.js](https://azure.microsoft.com/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="4aa4e-146">For more information, see hello [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).</span></span>

[jak toouse hello Azure Nástroje příkazového řádku pro Mac a Linux]:cli-install-nodejs.md
[nástroje příkazového řádku Azure]:cli-install-nodejs.md
[práce s moduly]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
