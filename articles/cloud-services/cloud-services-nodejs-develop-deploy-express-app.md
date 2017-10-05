---
title: "Webová aplikace s Express (Node.js) | Microsoft Docs"
description: "Kurz, který je založený na kurz cloudové služby a ukazuje, jak použít modul Express."
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
ms.openlocfilehash: 54b715695e24786ec4e8dfcabefc648d76179c8b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a><span data-ttu-id="62b27-103">Vytvoření webové aplikace Node.js pomocí expresního na cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="62b27-103">Build a Node.js web application using Express on an Azure Cloud Service</span></span>
<span data-ttu-id="62b27-104">Node.js zahrnuje minimální sadu funkcí, které v modulu runtime jádra.</span><span class="sxs-lookup"><span data-stu-id="62b27-104">Node.js includes a minimal set of functionality in the core runtime.</span></span>
<span data-ttu-id="62b27-105">3. stran moduly vývojáři často používají k poskytování dalších funkcí při vývoji aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="62b27-105">Developers often use 3rd party modules to provide additional functionality when developing a Node.js application.</span></span> <span data-ttu-id="62b27-106">V tomto kurzu vytvoříte novou aplikaci pomocí [Express] [ Express] modul, který poskytuje MVC technologie pro vytváření webových aplikací Node.js.</span><span class="sxs-lookup"><span data-stu-id="62b27-106">In this tutorial you will create a new application using the [Express][Express] module, which provides an MVC framework for creating Node.js web applications.</span></span>

<span data-ttu-id="62b27-107">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="62b27-107">A screenshot of the completed application is below:</span></span>

![Webový prohlížeč zobrazující Vítá do Express v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="62b27-109">Vytvoření projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="62b27-109">Create a Cloud Service Project</span></span>
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

<span data-ttu-id="62b27-110">Proveďte následující kroky k vytvoření nového projektu cloudové služby s názvem 'expressapp':</span><span class="sxs-lookup"><span data-stu-id="62b27-110">Perform the following steps to create a new cloud service project named 'expressapp':</span></span>

1. <span data-ttu-id="62b27-111">Z **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="62b27-111">From the **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="62b27-112">Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="62b27-112">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell – ikona](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. <span data-ttu-id="62b27-114">Přejděte do adresáře **c:\\uzlu** adresář a potom zadejte následující příkazy a vytvořte nové řešení s názvem **expressapp** a webové role s názvem **WebRole1**:</span><span class="sxs-lookup"><span data-stu-id="62b27-114">Change directories to the **c:\\node** directory and then enter the following commands to create a new solution named **expressapp** and a web role named **WebRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > <span data-ttu-id="62b27-115">Ve výchozím nastavení **Add-AzureNodeWebRole** používá starší verze Node.js.</span><span class="sxs-lookup"><span data-stu-id="62b27-115">By default, **Add-AzureNodeWebRole** uses an older version of Node.js.</span></span> <span data-ttu-id="62b27-116">**Set-AzureServiceProjectRole** výše dává pokyn, Azure a použít v0.10.21 uzlu.</span><span class="sxs-lookup"><span data-stu-id="62b27-116">The **Set-AzureServiceProjectRole** statement above instructs Azure to use v0.10.21 of Node.</span></span>  <span data-ttu-id="62b27-117">Všimněte si, že parametry jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="62b27-117">Note the parameters are case-sensitive.</span></span>  <span data-ttu-id="62b27-118">Můžete ověřit kontrolou byla vybrána správná verze Node.js **moduly** vlastnost **WebRole1\package.json**.</span><span class="sxs-lookup"><span data-stu-id="62b27-118">You can verify the correct version of Node.js has been selected by checking the **engines** property in **WebRole1\package.json**.</span></span>
    > 
    > 

## <a name="install-express"></a><span data-ttu-id="62b27-119">Expresní instalace</span><span class="sxs-lookup"><span data-stu-id="62b27-119">Install Express</span></span>
1. <span data-ttu-id="62b27-120">Generátor Express nainstalujte pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="62b27-120">Install the Express generator by issuing the following command:</span></span>
   
        PS C:\node\expressapp> npm install express-generator -g
   
    <span data-ttu-id="62b27-121">Výstup příkazu npm by měla vypadat podobně jako výsledek níže.</span><span class="sxs-lookup"><span data-stu-id="62b27-121">The output of the npm command should look similar to the result below.</span></span> 
   
    ![Prostředí Windows PowerShell zobrazení výstupu npm express příkaz instalovat.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. <span data-ttu-id="62b27-123">Přejděte do adresáře **WebRole1** adresáře a použijte příkaz express generovat nové aplikace:</span><span class="sxs-lookup"><span data-stu-id="62b27-123">Change directories to the **WebRole1** directory and use the express command to generate a new application:</span></span>
   
        PS C:\node\expressapp\WebRole1> express
   
    <span data-ttu-id="62b27-124">Zobrazí se výzva k přepsání starší aplikace.</span><span class="sxs-lookup"><span data-stu-id="62b27-124">You will be prompted to overwrite your earlier application.</span></span> <span data-ttu-id="62b27-125">Zadejte **y** nebo **Ano** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="62b27-125">Enter **y** or **yes** to continue.</span></span> <span data-ttu-id="62b27-126">Express způsobí vygenerování souboru app.js a strukturu složek pro vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="62b27-126">Express will generate the app.js file and a folder structure for building your application.</span></span>
   
    ![Výstup příkazu express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. <span data-ttu-id="62b27-128">Pokud chcete nainstalovat další závislosti, které jsou definované v souboru package.json, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="62b27-128">To install additional dependencies defined in the package.json file, enter the following command:</span></span>
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![Příkaz instalovat výstup npm](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. <span data-ttu-id="62b27-130">Použijte následující příkaz pro kopírování **bin/www** do souboru **server.js**.</span><span class="sxs-lookup"><span data-stu-id="62b27-130">Use the following command to copy the **bin/www** file to **server.js**.</span></span> <span data-ttu-id="62b27-131">To je proto cloudové služby můžete najít vstupní bod pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="62b27-131">This is so the cloud service can find the entry point for this application.</span></span>
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   <span data-ttu-id="62b27-132">Po dokončení tohoto příkazu, byste měli mít **server.js** soubor v adresáři WebRole1.</span><span class="sxs-lookup"><span data-stu-id="62b27-132">After this command completes, you should have a **server.js** file in the WebRole1 directory.</span></span>
5. <span data-ttu-id="62b27-133">Změnit **server.js** k odebrání jednoho z '.' znaky z následujícího řádku.</span><span class="sxs-lookup"><span data-stu-id="62b27-133">Modify the **server.js** to remove one of the '.' characters from the following line.</span></span>
   
       var app = require('../app');
   
   <span data-ttu-id="62b27-134">Po provedení této úpravy, řádku se zobrazí následující.</span><span class="sxs-lookup"><span data-stu-id="62b27-134">After making this modification, the line should appear as follows.</span></span>
   
       var app = require('./app');
   
   <span data-ttu-id="62b27-135">Tato změna je vyžadována, protože jsme přesunout soubor (dříve **bin/www**,) do stejného adresáře jako soubor aplikace se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="62b27-135">This change is required since we moved the file (formerly **bin/www**,) to the same directory as the app file being required.</span></span> <span data-ttu-id="62b27-136">Po provedení této změny, uložte **server.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="62b27-136">After making this change, save the **server.js** file.</span></span>
6. <span data-ttu-id="62b27-137">Spusťte aplikaci v emulátoru Azure použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="62b27-137">Use the following command to run the application in the Azure emulator:</span></span>
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Webovou stránku obsahující Vítá vás express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a><span data-ttu-id="62b27-139">Úprava zobrazení</span><span class="sxs-lookup"><span data-stu-id="62b27-139">Modifying the View</span></span>
<span data-ttu-id="62b27-140">Nyní upravte zobrazení tak, aby se zpráva "Vítá do Express v Azure".</span><span class="sxs-lookup"><span data-stu-id="62b27-140">Now modify the view to display the message "Welcome to Express in Azure".</span></span>

1. <span data-ttu-id="62b27-141">Zadejte následující příkaz pro otevření souboru index.jade:</span><span class="sxs-lookup"><span data-stu-id="62b27-141">Enter the following command to open the index.jade file:</span></span>
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![Obsah souboru index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   <span data-ttu-id="62b27-143">Jade je výchozí modul zobrazení používané aplikace Express.</span><span class="sxs-lookup"><span data-stu-id="62b27-143">Jade is the default view engine used by Express applications.</span></span> <span data-ttu-id="62b27-144">Další informace o Jade zobrazovací modul, najdete v části [http://jade-lang.com][http://jade-lang.com].</span><span class="sxs-lookup"><span data-stu-id="62b27-144">For more information on the Jade view engine, see [http://jade-lang.com][http://jade-lang.com].</span></span>
2. <span data-ttu-id="62b27-145">Upravit poslední řádek textu připojením **v Azure**.</span><span class="sxs-lookup"><span data-stu-id="62b27-145">Modify the last line of text by appending **in Azure**.</span></span>
   
   ![Načte poslední řádek souboru index.jade: p Vítejte \#{title} v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. <span data-ttu-id="62b27-147">Uložte tento soubor a ukončete Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="62b27-147">Save the file and exit Notepad.</span></span>
4. <span data-ttu-id="62b27-148">Aktualizujte webový prohlížeč a zobrazí se provedené změny.</span><span class="sxs-lookup"><span data-stu-id="62b27-148">Refresh your browser and you will see your changes.</span></span>
   
   ![Okno prohlížeče, tato stránka obsahuje Vítá vás Express v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

<span data-ttu-id="62b27-150">Po otestování aplikace, použijte **Stop-AzureEmulator** pro zastavení emulátor.</span><span class="sxs-lookup"><span data-stu-id="62b27-150">After testing the application, use the **Stop-AzureEmulator** cmdlet to stop the emulator.</span></span>

## <a name="publishing-the-application-to-azure"></a><span data-ttu-id="62b27-151">Publikování aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="62b27-151">Publishing the Application to Azure</span></span>
<span data-ttu-id="62b27-152">V okně prostředí Azure PowerShell, použijte **Publish-AzureServiceProject** rutiny k nasazení aplikace do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="62b27-152">In the Azure PowerShell window, use the **Publish-AzureServiceProject** cmdlet to deploy the application to a cloud service</span></span>

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

<span data-ttu-id="62b27-153">Po dokončení operace nasazení, prohlížeč a zobrazí webové stránky.</span><span class="sxs-lookup"><span data-stu-id="62b27-153">Once the deployment operation completes, your browser will open and display the web page.</span></span>

![Webový prohlížeč zobrazující stránku Express.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a><span data-ttu-id="62b27-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62b27-156">Next steps</span></span>
<span data-ttu-id="62b27-157">Další informace najdete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="62b27-157">For more information, see the [Node.js Developer Center](/develop/nodejs/).</span></span>

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


