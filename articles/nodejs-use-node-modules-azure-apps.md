---
title: "Práce s moduly Node.js"
description: "Naučte se pracovat s modulů Node.js při použití služby Azure App Service nebo cloudové služby."
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 1418c19594f19a15402d494880514298826445df
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-nodejs-modules-with-azure-applications"></a><span data-ttu-id="11a88-103">Používání modulů Node.js s aplikacemi Azure</span><span class="sxs-lookup"><span data-stu-id="11a88-103">Using Node.js Modules with Azure applications</span></span>
<span data-ttu-id="11a88-104">Tento dokument obsahuje pokyny k používání modulů Node.js s aplikacemi, které jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="11a88-104">This document provides guidance on using Node.js modules with applications hosted on Azure.</span></span> <span data-ttu-id="11a88-105">Poskytuje rady, jak zajistit, aby aplikace používala konkrétní verzi modulu, a jak používat nativní moduly s Azure.</span><span class="sxs-lookup"><span data-stu-id="11a88-105">It provides guidance on ensuring that your application uses a specific version of module as well as using native modules with Azure.</span></span>

<span data-ttu-id="11a88-106">Pokud jste již obeznámeni s použitím modulů Node.js **package.json** a **npm shrinkwrap.json** soubory, následující informace poskytují rychlý přehled co je popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="11a88-106">If you are already familiar with using Node.js modules, **package.json** and **npm-shrinkwrap.json** files, the following information provides a quick summary of what is discussed in this article:</span></span>

* <span data-ttu-id="11a88-107">Aplikační služba Azure rozumí **package.json** a **npm shrinkwrap.json** soubory a můžete nainstalovat moduly založené na položky v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="11a88-107">Azure App Service understands **package.json** and **npm-shrinkwrap.json** files and can install modules based on entries in these files.</span></span>

* <span data-ttu-id="11a88-108">Azure Cloud Services očekávat všechny moduly být nainstalovány na vývojového prostředí a **uzlu\_moduly** adresáři, který bude součástí balíčku pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="11a88-108">Azure Cloud Services expect all modules to be installed on the development environment, and the **node\_modules** directory to be included as part of the deployment package.</span></span> <span data-ttu-id="11a88-109">Je možné povolit podporu pro instalaci modulů pomocí **package.json** nebo **npm shrinkwrap.json** soubory na cloudových služeb, ale tato konfigurace vyžaduje vlastní skripty výchozí používané projekty cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="11a88-109">It is possible to enable support for installing modules using **package.json** or **npm-shrinkwrap.json** files on Cloud Services; however, this configuration requires customization of the default scripts used by Cloud Service projects.</span></span> <span data-ttu-id="11a88-110">Příklad způsobu konfigurace tohoto prostředí naleznete v části [úloha spuštění Azure ke spuštění instalace npm předejdete nasazení uzlu moduly](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)</span><span class="sxs-lookup"><span data-stu-id="11a88-110">For an example of how to configure this environment, see [Azure Startup task to run npm install to avoid deploying node modules](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)</span></span>

> [!NOTE]
> <span data-ttu-id="11a88-111">Virtuální počítače Azure nejsou popsané v tomto článku se jako možnosti nasazení do virtuálního počítače, je závislá na operační systém, který je hostitelem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="11a88-111">Azure Virtual Machines are not discussed in this article, as the deployment experience in a VM is dependent on the operating system hosted by the Virtual Machine.</span></span>
> 
> 

## <a name="nodejs-modules"></a><span data-ttu-id="11a88-112">Modulů Node.js</span><span class="sxs-lookup"><span data-stu-id="11a88-112">Node.js Modules</span></span>
<span data-ttu-id="11a88-113">Moduly jsou načíst JavaScript balíčky, které poskytují funkce specifická pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11a88-113">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="11a88-114">Moduly jsou obvykle nainstalován pomocí **npm** příkazového řádku nástroje, ale jsou uvedeny některé moduly (jako je například modul http) jako součást balíčku Node.js jádra.</span><span class="sxs-lookup"><span data-stu-id="11a88-114">Modules are usually installed using the **npm** command-line tool, however some modules (such as the http module) are provided as part of the core Node.js package.</span></span>

<span data-ttu-id="11a88-115">Při instalaci moduly jsou uložené ve **uzlu\_moduly** adresář v kořenovém adresáři aplikace adresářové struktury.</span><span class="sxs-lookup"><span data-stu-id="11a88-115">When modules are installed, they are stored in the **node\_modules** directory at the root of your application directory structure.</span></span> <span data-ttu-id="11a88-116">Každý modul v rámci **uzlu\_moduly** directory udržuje vlastní **uzlu\_moduly** adresář, který obsahuje všechny moduly, které závisí na a toto chování se opakuje pro každý modul zcela v řetězu závislostí.</span><span class="sxs-lookup"><span data-stu-id="11a88-116">Each module within the **node\_modules** directory maintains its own **node\_modules** directory that contains any modules that it depends on, and this behavior repeats for every module all the way down the dependency chain.</span></span> <span data-ttu-id="11a88-117">Toto prostředí umožňuje každý modul nainstalovaný mít vlastní požadavky verze pro moduly, že závisí na, ale může vést k poměrně velké adresářové struktury.</span><span class="sxs-lookup"><span data-stu-id="11a88-117">This environment allows each module installed to have its own version requirements for the modules it depends on, however it can result in quite a large directory structure.</span></span>

<span data-ttu-id="11a88-118">Nasazení **uzlu\_moduly** adresáři jako součást aplikace zvětšuje velikost nasazení ve srovnání s pomocí **package.json** nebo **npm shrinkwrap.json** soubor; však zaručit, že verze moduly používané v produkčním prostředí jsou stejné jako moduly používané v vývoj.</span><span class="sxs-lookup"><span data-stu-id="11a88-118">Deploying the **node\_modules** directory as part of your application increases the size of the deployment when compared to using a **package.json** or **npm-shrinkwrap.json** file; however, it does guarantee that the versions of the modules used in production are the same as the modules used in development.</span></span>

### <a name="native-modules"></a><span data-ttu-id="11a88-119">Nativní moduly</span><span class="sxs-lookup"><span data-stu-id="11a88-119">Native Modules</span></span>
<span data-ttu-id="11a88-120">Většina moduly jsou soubory JavaScript jednoduše prostého textu, některé moduly jsou specifické pro platformu binární bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="11a88-120">While most modules are simply plain-text JavaScript files, some modules are platform-specific binary images.</span></span> <span data-ttu-id="11a88-121">Tyto moduly jsou kompilovány během instalace, obvykle pomocí Pythonu a gyp uzlu.</span><span class="sxs-lookup"><span data-stu-id="11a88-121">These modules are compiled at install time, usually by using Python and node-gyp.</span></span> <span data-ttu-id="11a88-122">Vzhledem k tomu, že Azure Cloud Services spoléhají na **uzlu\_moduly** složky, které jsou nasazované jako součást aplikace, všechny nativní modul, jsou součástí nainstalovaných modulů by měla fungovat v cloudové službě Pokud byl nainstalován a kompilovat v systému Windows, vývoj.</span><span class="sxs-lookup"><span data-stu-id="11a88-122">Since Azure Cloud Services rely on the **node\_modules** folder being deployed as part of the application, any native module included as part of the installed modules should work in a cloud service as long as it was installed and compiled on a Windows development system.</span></span>

<span data-ttu-id="11a88-123">Aplikační služba Azure nepodporuje všechny nativní moduly a může selhat, když kompilujete modulů s konkrétní požadavky.</span><span class="sxs-lookup"><span data-stu-id="11a88-123">Azure App Service does not support all native modules, and might fail when compiling modules with specific prerequisites.</span></span> <span data-ttu-id="11a88-124">I když některé oblíbené moduly jako MongoDB mají volitelné nativní závislosti a bez problémů fungují bez nich, prokázat dvě řešení úspěšné s téměř všechny nativní moduly, které jsou k dispozici dnes:</span><span class="sxs-lookup"><span data-stu-id="11a88-124">While some popular modules like MongoDB have optional native dependencies and work fine without them, two workarounds proved successful with almost all native modules available today:</span></span>

* <span data-ttu-id="11a88-125">Spustit **npm nainstalujte** na počítači s Windows, který má nainstalovány požadované součásti všechny nativní modul na.</span><span class="sxs-lookup"><span data-stu-id="11a88-125">Run **npm install** on a Windows machine that has all the native module's prerequisites installed.</span></span> <span data-ttu-id="11a88-126">Potom nasaďte vytvořený **uzlu\_moduly** složku v rámci aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="11a88-126">Then, deploy the created **node\_modules** folder as part of the application to Azure App Service.</span></span>

  * <span data-ttu-id="11a88-127">Před kompilování, zkontrolujte, zda vaše místní instalaci Node.js má odpovídající architekturu a verze je co nejblíže k jednomu používá v Azure (aktuální hodnoty lze zkontrolovat na runtime z vlastností **process.arch** a **process.version**).</span><span class="sxs-lookup"><span data-stu-id="11a88-127">Before compiling, check that your local Node.js installation has matching architecture and the version is as close as possible to the one used in Azure (the current values can be checked on runtime from properties **process.arch** and **process.version**).</span></span>

* <span data-ttu-id="11a88-128">Aplikační služba Azure je možné nakonfigurovat na provedení vlastní bash nebo skripty prostředí během nasazení, budete moci provést vlastní příkazy a přesně nastavit způsob **npm nainstalujte** je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="11a88-128">Azure App Service can be configured to execute custom bash or shell scripts during deployment, giving you the opportunity to execute custom commands and precisely configure the way **npm install** is being run.</span></span> <span data-ttu-id="11a88-129">Video znázorňující postup konfigurace prostředí najdete v tématu [skriptů nasazení vlastní web s Kudu].</span><span class="sxs-lookup"><span data-stu-id="11a88-129">For a video showing how to configure that environment, see [Custom Website Deployment Scripts with Kudu].</span></span>

### <a name="using-a-packagejson-file"></a><span data-ttu-id="11a88-130">Pomocí souboru package.json</span><span class="sxs-lookup"><span data-stu-id="11a88-130">Using a package.json file</span></span>
<span data-ttu-id="11a88-131">**Package.json** soubor je způsob, jak určit nejvyšší úrovně závislosti vaše aplikace vyžaduje, aby hostující platforma můžete nainstalovat závislosti, nikoli by bylo potřeba zahrnout **uzlu\_balíčky** složku jako součást nasazení.</span><span class="sxs-lookup"><span data-stu-id="11a88-131">The **package.json** file is a way to specify the top-level dependencies your application requires so that the hosting platform can install the dependencies, rather than requiring you to include the **node\_packages** folder as part of the deployment.</span></span> <span data-ttu-id="11a88-132">Po nasazení aplikace **npm nainstalujte** příkaz slouží k analýze **package.json** souboru a nainstalujte všechny uvedené závislosti.</span><span class="sxs-lookup"><span data-stu-id="11a88-132">After the application has been deployed, the **npm install** command is used to parse the **package.json** file and install all the dependencies listed.</span></span>

<span data-ttu-id="11a88-133">Během vývoje, můžete použít **– Uložit**, **– uložit dev**, nebo **– uložit volitelné** parametry při instalaci modulů, které chcete přidat záznam pro modulu vaše **package.json** souboru automaticky.</span><span class="sxs-lookup"><span data-stu-id="11a88-133">During development, you can use the **--save**, **--save-dev**, or **--save-optional** parameters when installing modules to add an entry for the module to your **package.json** file automatically.</span></span> <span data-ttu-id="11a88-134">Další informace najdete v tématu [npm install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="11a88-134">For more information, see [npm-install](https://docs.npmjs.com/cli/install).</span></span>

<span data-ttu-id="11a88-135">Jedním z možných problémů s **package.json** soubor je, že pouze určuje verzi nejvyšší úrovně závislosti.</span><span class="sxs-lookup"><span data-stu-id="11a88-135">One potential problem with the **package.json** file is that it only specifies the version for top-level dependencies.</span></span> <span data-ttu-id="11a88-136">Každý modul nainstalovaný může nebo nemusí zadejte moduly, které závisí na verzi, a proto je možné, že můžete dospět řetězem závislostí jiný než ten, který je používán vývoj.</span><span class="sxs-lookup"><span data-stu-id="11a88-136">Each module installed may or may not specify the version of the modules it depends on, and so it is possible that you may end up with a different dependency chain than the one used in development.</span></span>

> [!NOTE]
> <span data-ttu-id="11a88-137">Při nasazování do služby Azure App Service, pokud vaše <b>package.json</b> soubor odkazuje na nativní modul při publikování aplikace pomocí Git, může se zobrazit chyba podobná v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="11a88-137">When deploying to Azure App Service, if your <b>package.json</b> file references a native module you might see an error similar to the following example when publishing the application using Git:</span></span>
> 
> <span data-ttu-id="11a88-138">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="11a88-138">npm ERR!</span></span> <span data-ttu-id="11a88-139">module-name@0.6.0Nainstalujte: 'uzlu gyp konfigurace buildu.</span><span class="sxs-lookup"><span data-stu-id="11a88-139">module-name@0.6.0 install: 'node-gyp configure build'</span></span>
> 
> <span data-ttu-id="11a88-140">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="11a88-140">npm ERR!</span></span> <span data-ttu-id="11a88-141">' cmd "/ c" "uzlu gyp konfigurace sestavení" ' se nezdařilo s 1</span><span class="sxs-lookup"><span data-stu-id="11a88-141">'cmd "/c" "node-gyp configure build"' failed with 1</span></span>
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a><span data-ttu-id="11a88-142">Pomocí npm shrinkwrap.json souboru.</span><span class="sxs-lookup"><span data-stu-id="11a88-142">Using a npm-shrinkwrap.json file</span></span>
<span data-ttu-id="11a88-143">**Npm shrinkwrap.json** soubor je pokus o adres omezení verze modulu **package.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="11a88-143">The **npm-shrinkwrap.json** file is an attempt to address the module versioning limitations of the **package.json** file.</span></span> <span data-ttu-id="11a88-144">Při **package.json** soubor obsahuje pouze verze pro moduly nejvyšší úrovně, **npm shrinkwrap.json** soubor obsahuje požadavky na verzi řetězu závislostí úplné modulu.</span><span class="sxs-lookup"><span data-stu-id="11a88-144">While the **package.json** file only includes versions for the top-level modules, the **npm-shrinkwrap.json** file contains the version requirements for the full module dependency chain.</span></span>

<span data-ttu-id="11a88-145">Když je aplikace připravená pro vydání, můžete uzamknout požadavky verze a vytvořit **npm shrinkwrap.json** soubor pomocí **npm shrinkwrap** příkaz.</span><span class="sxs-lookup"><span data-stu-id="11a88-145">When your application is ready for production, you can lock down version requirements and create an **npm-shrinkwrap.json** file by using the **npm shrinkwrap** command.</span></span> <span data-ttu-id="11a88-146">Tento příkaz bude používat na aktuálně nainstalovaná verze **uzlu\_moduly** složku a zaznamenejte tyto verze **npm shrinkwrap.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="11a88-146">This command will use the versions currently installed in the **node\_modules** folder, and record these versions to the **npm-shrinkwrap.json** file.</span></span> <span data-ttu-id="11a88-147">Po nasazení aplikace do hostitelského prostředí **npm nainstalujte** příkaz slouží k analýze **npm shrinkwrap.json** souboru a nainstalujte všechny uvedené závislosti.</span><span class="sxs-lookup"><span data-stu-id="11a88-147">After the application has been deployed to the hosting environment, the **npm install** command is used to parse the **npm-shrinkwrap.json** file and install all the dependencies listed.</span></span> <span data-ttu-id="11a88-148">Další informace najdete v tématu [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).</span><span class="sxs-lookup"><span data-stu-id="11a88-148">For more information, see [npm-shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).</span></span>

> [!NOTE]
> <span data-ttu-id="11a88-149">Při nasazování do služby Azure App Service, pokud vaše <b>npm shrinkwrap.json</b> soubor odkazuje na nativní modul při publikování aplikace pomocí Git, může se zobrazit chyba podobná v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="11a88-149">When deploying to Azure App Service, if your <b>npm-shrinkwrap.json</b> file references a native module you might see an error similar to the following example when publishing the application using Git:</span></span>
> 
> <span data-ttu-id="11a88-150">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="11a88-150">npm ERR!</span></span> <span data-ttu-id="11a88-151">module-name@0.6.0Nainstalujte: 'uzlu gyp konfigurace buildu.</span><span class="sxs-lookup"><span data-stu-id="11a88-151">module-name@0.6.0 install: 'node-gyp configure build'</span></span>
> 
> <span data-ttu-id="11a88-152">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="11a88-152">npm ERR!</span></span> <span data-ttu-id="11a88-153">' cmd "/ c" "uzlu gyp konfigurace sestavení" ' se nezdařilo s 1</span><span class="sxs-lookup"><span data-stu-id="11a88-153">'cmd "/c" "node-gyp configure build"' failed with 1</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="11a88-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11a88-154">Next steps</span></span>
<span data-ttu-id="11a88-155">Teď, když chápete, jak používání modulů Node.js s Azure, se naučíte, jak [určení verze Node.js], [sestavení a nasazení webové aplikace Node.js](app-service-web/app-service-web-get-started-nodejs.md), a [jak používat rozhraní příkazového řádku Azure pro Mac a Linux].</span><span class="sxs-lookup"><span data-stu-id="11a88-155">Now that you understand how to use Node.js modules with Azure, learn how to [specify the Node.js version], [build and deploy a Node.js web app](app-service-web/app-service-web-get-started-nodejs.md), and [How to use the Azure Command-Line Interface for Mac and Linux].</span></span>

<span data-ttu-id="11a88-156">Další informace najdete ve [Středisku pro vývojáře Node.js](/nodejs/azure/).</span><span class="sxs-lookup"><span data-stu-id="11a88-156">For more information, see the [Node.js Developer Center](/nodejs/azure/).</span></span>

<span data-ttu-id="11a88-157">[určení verze Node.js]: nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="11a88-157">[specify the Node.js version]: nodejs-specify-node-version-azure-apps.md</span></span>
<span data-ttu-id="11a88-158">[jak používat rozhraní příkazového řádku Azure pro Mac a Linux]:cli-install-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="11a88-158">[How to use the Azure Command-Line Interface for Mac and Linux]:cli-install-nodejs.md</span></span>
<span data-ttu-id="11a88-159">[skriptů nasazení vlastní web s Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo</span><span class="sxs-lookup"><span data-stu-id="11a88-159">[Custom Website Deployment Scripts with Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo</span></span>