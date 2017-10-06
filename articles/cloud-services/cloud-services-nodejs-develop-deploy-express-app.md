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
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Vytvoření webové aplikace Node.js pomocí expresního na cloudové služby Azure
Node.js zahrnuje minimální sadu funkcí, které v hello core runtime.
Vývojáři často používají 3. stran moduly tooprovide další funkce, při vývoji aplikace Node.js. V tomto kurzu vytvoříte novou aplikaci pomocí hello [Express] [ Express] modul, který poskytuje MVC technologie pro vytváření webových aplikací Node.js.

Zde je snímek obrazovky aplikace hello dokončena:

![Webový prohlížeč zobrazující úvodní tooExpress v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Vytvoření projektu cloudové služby
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Proveďte následující hello kroky toocreate nový projekt cloudové služby s názvem 'expressapp':

1. Z hello **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**. Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.
   
    ![Azure PowerShell – ikona](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Změna adresáře toohello **c:\\uzlu** adresář a potom zadejte následující příkazy toocreate nové řešení s názvem hello **expressapp** a webové role s názvem **WebRole1** :
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Ve výchozím nastavení **Add-AzureNodeWebRole** používá starší verze Node.js. Hello **Set-AzureServiceProjectRole** výše dává pokyn Azure toouse v0.10.21 uzlu.  Všimněte si, že jsou parametry hello malá a velká písmena.  Můžete ověřit kontrolou hello byla vybrána správná verze Node.js hello **moduly** vlastnost **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Expresní instalace
1. Nainstalujte generátor Express hello vydáním hello následující příkaz:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    výstup Hello hello npm příkazu by měla vypadat podobně jako výsledek toohello níže. 
   
    ![Prostředí Windows PowerShell zobrazování hello výstup hello npm express příkaz instalovat.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Změna adresáře toohello **WebRole1** adresáře a použijte hello express příkaz toogenerate nové aplikace:
   
        PS C:\node\expressapp\WebRole1> express
   
    Můžete se výzvami toooverwrite starší aplikace. Zadejte **y** nebo **Ano** toocontinue. Express způsobí vygenerování souboru app.js hello a strukturu složek pro vytváření aplikace.
   
    ![výstup Hello express příkazu hello](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. Další závislosti tooinstall definované v souboru package.json hello, zadejte následující příkaz hello:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![příkaz instalovat Hello výstup hello npm](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Použití hello následující příkaz toocopy hello **bin/www** souboru příliš**server.js**. To je proto hello cloudové služby můžete najít hello vstupní bod pro tuto aplikaci.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Po dokončení tohoto příkazu, byste měli mít **server.js** soubor v adresáři hello WebRole1.
5. Upravit hello **server.js** tooremove mezi hello "." znaky z hello následující řádek.
   
       var app = require('../app');
   
   Po provedení této úpravy, hello řádku se zobrazí následující.
   
       var app = require('./app');
   
   Tato změna je vyžadována, protože jsme přesunout soubor hello (dříve **bin/www**,) toohello stejného adresáře jako soubor aplikace hello nich vyžaduje. Po provedení této změny, uložte hello **server.js** souboru.
6. Použijte následující příkaz toorun hello aplikace hello Azure emulátoru hello:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Webovou stránku obsahující úvodní tooexpress.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-hello-view"></a>Úprava hello zobrazení
Nyní změňte hello zobrazení toodisplay uvítací zprávu "Vítá tooExpress v Azure".

1. Zadejte následující příkaz tooopen hello index.jade soubor hello:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![Hello obsah souboru index.jade hello.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade je hello výchozí zobrazovací modul používá aplikace Express. Další informace o Jade zobrazovací modul hello najdete v tématu [http://jade-lang.com][http://jade-lang.com].
2. Upravit poslední řádek textu hello připojením **v Azure**.
   
   ![přečte hello poslední řádek souboru index.jade Hello: p Vítejte příliš\#{title} v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Uložte soubor hello a ukončete Poznámkový blok.
4. Aktualizujte webový prohlížeč a zobrazí se provedené změny.
   
   ![Okno prohlížeče, hello stránka obsahuje úvodní tooExpress v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Po testování hello aplikace, použijte hello **Stop-AzureEmulator** rutiny toostop hello emulátor.

## <a name="publishing-hello-application-tooazure"></a>Publikování aplikace tooAzure hello
V okně prostředí Azure PowerShell text hello, použijte hello **Publish-AzureServiceProject** rutiny toodeploy hello aplikace tooa cloudové služby

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Po dokončení operace nasazení hello prohlížeč a zobrazí hello webové stránky.

![Webový prohlížeč zobrazující stránku hello Express. Hello URL značí, že je nyní hostované v Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře Node.js](/develop/nodejs/).

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


