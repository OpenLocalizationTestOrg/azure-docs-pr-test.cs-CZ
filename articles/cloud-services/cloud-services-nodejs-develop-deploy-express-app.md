---
title: "aaaWeb aplikace pomocí Express (Node.js) | Microsoft Docs"
description: "Kurz, který je založený na hello cloudové služby kurzu a ukazuje, jak toouse hello modulu Express."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 91921bfbe137eeca9a110d4cb18eb57b46b0060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a><span data-ttu-id="02784-103">Vytvoření webové aplikace Node.js pomocí expresního na cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="02784-103">Build a Node.js web application using Express on an Azure Cloud Service</span></span>
<span data-ttu-id="02784-104">Node.js zahrnuje minimální sadu funkcí, které v hello core runtime.</span><span class="sxs-lookup"><span data-stu-id="02784-104">Node.js includes a minimal set of functionality in hello core runtime.</span></span>
<span data-ttu-id="02784-105">Vývojáři často používají 3. stran moduly tooprovide další funkce, při vývoji aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="02784-105">Developers often use 3rd party modules tooprovide additional functionality when developing a Node.js application.</span></span> <span data-ttu-id="02784-106">V tomto kurzu vytvoříte novou aplikaci pomocí hello [Express] [ Express] modul, který poskytuje MVC technologie pro vytváření webových aplikací Node.js.</span><span class="sxs-lookup"><span data-stu-id="02784-106">In this tutorial you will create a new application using hello [Express][Express] module, which provides an MVC framework for creating Node.js web applications.</span></span>

<span data-ttu-id="02784-107">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="02784-107">A screenshot of hello completed application is below:</span></span>

![Webový prohlížeč zobrazující úvodní tooExpress v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="02784-109">Vytvoření projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="02784-109">Create a Cloud Service Project</span></span>
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

<span data-ttu-id="02784-110">Proveďte následující hello kroky toocreate nový projekt cloudové služby s názvem 'expressapp':</span><span class="sxs-lookup"><span data-stu-id="02784-110">Perform hello following steps toocreate a new cloud service project named 'expressapp':</span></span>

1. <span data-ttu-id="02784-111">Z hello **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="02784-111">From hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="02784-112">Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="02784-112">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell – ikona](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. <span data-ttu-id="02784-114">Změna adresáře toohello **c:\\uzlu** adresář a potom zadejte následující příkazy toocreate nové řešení s názvem hello **expressapp** a webové role s názvem **WebRole1** :</span><span class="sxs-lookup"><span data-stu-id="02784-114">Change directories toohello **c:\\node** directory and then enter hello following commands toocreate a new solution named **expressapp** and a web role named **WebRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > <span data-ttu-id="02784-115">Ve výchozím nastavení **Add-AzureNodeWebRole** používá starší verze Node.js.</span><span class="sxs-lookup"><span data-stu-id="02784-115">By default, **Add-AzureNodeWebRole** uses an older version of Node.js.</span></span> <span data-ttu-id="02784-116">Hello **Set-AzureServiceProjectRole** výše dává pokyn Azure toouse v0.10.21 uzlu.</span><span class="sxs-lookup"><span data-stu-id="02784-116">hello **Set-AzureServiceProjectRole** statement above instructs Azure toouse v0.10.21 of Node.</span></span>  <span data-ttu-id="02784-117">Všimněte si, že jsou parametry hello malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="02784-117">Note hello parameters are case-sensitive.</span></span>  <span data-ttu-id="02784-118">Můžete ověřit kontrolou hello byla vybrána správná verze Node.js hello **moduly** vlastnost **WebRole1\package.json**.</span><span class="sxs-lookup"><span data-stu-id="02784-118">You can verify hello correct version of Node.js has been selected by checking hello **engines** property in **WebRole1\package.json**.</span></span>
    > 
    > 

## <a name="install-express"></a><span data-ttu-id="02784-119">Expresní instalace</span><span class="sxs-lookup"><span data-stu-id="02784-119">Install Express</span></span>
1. <span data-ttu-id="02784-120">Nainstalujte generátor Express hello vydáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="02784-120">Install hello Express generator by issuing hello following command:</span></span>
   
        PS C:\node\expressapp> npm install express-generator -g
   
    <span data-ttu-id="02784-121">výstup Hello hello npm příkazu by měla vypadat podobně jako výsledek toohello níže.</span><span class="sxs-lookup"><span data-stu-id="02784-121">hello output of hello npm command should look similar toohello result below.</span></span> 
   
    ![Prostředí Windows PowerShell zobrazování hello výstup hello npm express příkaz instalovat.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. <span data-ttu-id="02784-123">Změna adresáře toohello **WebRole1** adresáře a použijte hello express příkaz toogenerate nové aplikace:</span><span class="sxs-lookup"><span data-stu-id="02784-123">Change directories toohello **WebRole1** directory and use hello express command toogenerate a new application:</span></span>
   
        PS C:\node\expressapp\WebRole1> express
   
    <span data-ttu-id="02784-124">Můžete se výzvami toooverwrite starší aplikace.</span><span class="sxs-lookup"><span data-stu-id="02784-124">You will be prompted toooverwrite your earlier application.</span></span> <span data-ttu-id="02784-125">Zadejte **y** nebo **Ano** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="02784-125">Enter **y** or **yes** toocontinue.</span></span> <span data-ttu-id="02784-126">Express způsobí vygenerování souboru app.js hello a strukturu složek pro vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="02784-126">Express will generate hello app.js file and a folder structure for building your application.</span></span>
   
    ![výstup Hello express příkazu hello](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. <span data-ttu-id="02784-128">Další závislosti tooinstall definované v souboru package.json hello, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="02784-128">tooinstall additional dependencies defined in hello package.json file, enter hello following command:</span></span>
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![příkaz instalovat Hello výstup hello npm](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. <span data-ttu-id="02784-130">Použití hello následující příkaz toocopy hello **bin/www** souboru příliš**server.js**.</span><span class="sxs-lookup"><span data-stu-id="02784-130">Use hello following command toocopy hello **bin/www** file too**server.js**.</span></span> <span data-ttu-id="02784-131">To je proto hello cloudové služby můžete najít hello vstupní bod pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02784-131">This is so hello cloud service can find hello entry point for this application.</span></span>
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   <span data-ttu-id="02784-132">Po dokončení tohoto příkazu, byste měli mít **server.js** soubor v adresáři hello WebRole1.</span><span class="sxs-lookup"><span data-stu-id="02784-132">After this command completes, you should have a **server.js** file in hello WebRole1 directory.</span></span>
5. <span data-ttu-id="02784-133">Upravit hello **server.js** tooremove mezi hello "." znaky z hello následující řádek.</span><span class="sxs-lookup"><span data-stu-id="02784-133">Modify hello **server.js** tooremove one of hello '.' characters from hello following line.</span></span>
   
       var app = require('../app');
   
   <span data-ttu-id="02784-134">Po provedení této úpravy, hello řádku se zobrazí následující.</span><span class="sxs-lookup"><span data-stu-id="02784-134">After making this modification, hello line should appear as follows.</span></span>
   
       var app = require('./app');
   
   <span data-ttu-id="02784-135">Tato změna je vyžadována, protože jsme přesunout soubor hello (dříve **bin/www**,) toohello stejného adresáře jako soubor aplikace hello nich vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="02784-135">This change is required since we moved hello file (formerly **bin/www**,) toohello same directory as hello app file being required.</span></span> <span data-ttu-id="02784-136">Po provedení této změny, uložte hello **server.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="02784-136">After making this change, save hello **server.js** file.</span></span>
6. <span data-ttu-id="02784-137">Použijte následující příkaz toorun hello aplikace hello Azure emulátoru hello:</span><span class="sxs-lookup"><span data-stu-id="02784-137">Use hello following command toorun hello application in hello Azure emulator:</span></span>
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Webovou stránku obsahující úvodní tooexpress.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-hello-view"></a><span data-ttu-id="02784-139">Úprava hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="02784-139">Modifying hello View</span></span>
<span data-ttu-id="02784-140">Nyní změňte hello zobrazení toodisplay uvítací zprávu "Vítá tooExpress v Azure".</span><span class="sxs-lookup"><span data-stu-id="02784-140">Now modify hello view toodisplay hello message "Welcome tooExpress in Azure".</span></span>

1. <span data-ttu-id="02784-141">Zadejte následující příkaz tooopen hello index.jade soubor hello:</span><span class="sxs-lookup"><span data-stu-id="02784-141">Enter hello following command tooopen hello index.jade file:</span></span>
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![Hello obsah souboru index.jade hello.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   <span data-ttu-id="02784-143">Jade je hello výchozí zobrazovací modul používá aplikace Express.</span><span class="sxs-lookup"><span data-stu-id="02784-143">Jade is hello default view engine used by Express applications.</span></span> <span data-ttu-id="02784-144">Další informace o Jade zobrazovací modul hello najdete v tématu [http://jade-lang.com][http://jade-lang.com].</span><span class="sxs-lookup"><span data-stu-id="02784-144">For more information on hello Jade view engine, see [http://jade-lang.com][http://jade-lang.com].</span></span>
2. <span data-ttu-id="02784-145">Upravit poslední řádek textu hello připojením **v Azure**.</span><span class="sxs-lookup"><span data-stu-id="02784-145">Modify hello last line of text by appending **in Azure**.</span></span>
   
   ![přečte hello poslední řádek souboru index.jade Hello: p Vítejte příliš\#{title} v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. <span data-ttu-id="02784-147">Uložte soubor hello a ukončete Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="02784-147">Save hello file and exit Notepad.</span></span>
4. <span data-ttu-id="02784-148">Aktualizujte webový prohlížeč a zobrazí se provedené změny.</span><span class="sxs-lookup"><span data-stu-id="02784-148">Refresh your browser and you will see your changes.</span></span>
   
   ![Okno prohlížeče, hello stránka obsahuje úvodní tooExpress v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

<span data-ttu-id="02784-150">Po testování hello aplikace, použijte hello **Stop-AzureEmulator** rutiny toostop hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="02784-150">After testing hello application, use hello **Stop-AzureEmulator** cmdlet toostop hello emulator.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="02784-151">Publikování aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="02784-151">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="02784-152">V okně prostředí Azure PowerShell text hello, použijte hello **Publish-AzureServiceProject** rutiny toodeploy hello aplikace tooa cloudové služby</span><span class="sxs-lookup"><span data-stu-id="02784-152">In hello Azure PowerShell window, use hello **Publish-AzureServiceProject** cmdlet toodeploy hello application tooa cloud service</span></span>

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

<span data-ttu-id="02784-153">Po dokončení operace nasazení hello prohlížeč a zobrazí hello webové stránky.</span><span class="sxs-lookup"><span data-stu-id="02784-153">Once hello deployment operation completes, your browser will open and display hello web page.</span></span>

![Webový prohlížeč zobrazující stránku hello Express.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a><span data-ttu-id="02784-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02784-156">Next steps</span></span>
<span data-ttu-id="02784-157">Další informace najdete v tématu hello [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="02784-157">For more information, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


