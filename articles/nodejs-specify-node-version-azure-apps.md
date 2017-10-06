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
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Určení verze Node.js v aplikaci Azure
Při hostování aplikace Node.js, může být vhodné tooensure, kterou vaše aplikace používá na konkrétní verzi Node.js. Existuje několik způsobů tooaccomplish to pro aplikace hostované v Azure.

## <a name="default-versions"></a>Výchozí verze
verze Node.js Hello poskytovaný platformou Azure jsou neustále aktualizovány. Pokud není uvedeno jinak, hello výchozí verze, který je uveden v hello `WEBSITE_NODE_DEFAULT_VERSION` proměnné prostředí se použije. toooverride tuto výchozí hodnotu, postupujte podle kroků hello v následující části tohoto článku

> [!NOTE]
> Pokud je hostitelem vaší aplikace v cloudové službě Azure (role web nebo worker) a je hello poprvé jste nasadili aplikaci hello, Azure pokusí toouse hello stejnou verzi Node.js, jako jste nainstalovali na vývojového prostředí, pokud ji odpovídá jednomu z verze výchozí hello k dispozici v Azure.
>
>

## <a name="versioning-with-packagejson"></a>Správa verzí pomocí package.json
Můžete zadat hello verze Node.js toobe použít přidáním hello následující tooyour **package.json** souboru:

    "engines":{"node":version}

Kde *verze* je číslo toouse hello konkrétní verzi. Složitější podmínky pro verzi, můžete zadat například:

    "engines":{"node": "0.6.22 || 0.8.x"}

Vzhledem k tomu, že 0.6.22 není některá z verzí hello k dispozici v hello hostování prostředí, hello nejvyšší verzi hello 0,8 série, která je k dispozici bude místo toho použít - 0.8.4.

## <a name="versioning-websites-with-app-settings"></a>Správa verzí webů s nastavení aplikace
Pokud hostujete hello aplikace na webu, můžete nastavit proměnné prostředí hello **WEBSITE_NODE_DEFAULT_VERSION** toohello požadovanou verzi.

## <a name="versioning-cloud-services-with-powershell"></a>Správa verzí cloudové služby pomocí prostředí PowerShell
Pokud jsou hostiteli hello aplikace v cloudové službě a nasazení aplikace hello pomocí Azure PowerShell, můžete přepsat hello výchozí Node.js verze pomocí hello **Set-AzureServiceProjectRole** rutiny prostředí PowerShell. Například:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Poznámka hello parametry v hello výše příkaz rozlišují velká a malá písmena.  Můžete ověřit kontrolou hello byla vybrána správná verze Node.js hello **moduly** vlastnost vaší role **package.json**.

Můžete taky hello **Get-AzureServiceProjectRoleRuntime** tooretrieve seznam Node.js verze, které jsou k dispozici pro aplikace hostované jako cloudová služba.  Vždy ověřte, zda text hello verze Node.js projektu závisí na je v tomto seznamu.

## <a name="using-a-custom-version-with-azure-websites"></a>Použití vlastní verzi s weby Azure
Zatímco Azure poskytuje několik verzí výchozí Node.js, může být vhodné toouse na verzi, která není uvedena ve výchozím nastavení. Pokud je vaše aplikace hostována jako web Azure, můžete dosáhnout pomocí hello **iisnode.yml** souboru. Hello následující kroky provede hello proces vlastní verzi Node.Js pomocí webu Azure:

1. Vytvořte nový adresář a pak vytvořte **server.js** soubor v adresáři hello. Hello **server.js** soubor by měl obsahovat hello následující:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Tato akce zobrazí verze Node.js hello používá při procházení webu hello.
2. Vytvořte nový web a Poznámka hello název lokality hello. Například následující hello používá hello [nástroje příkazového řádku Azure] toocreate nový web Azure s názvem **mywebsite**a pak povolte úložiště Git pro web hello.

        azure site create mywebsite --git
3. Vytvořte nový adresář s názvem **bin** jako podřízenou hello adresář obsahující hello **server.js** souboru.
4. Stáhnout hello konkrétní verzi nástroje **node.exe** (verze Windows hello) chcete toouse s vaší aplikací. Například následující používá hello **curl** toodownload verze 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Uložit hello **node.exe** souboru do hello **bin** složky, které jste vytvořili dříve.
5. Vytvoření **iisnode.yml** souboru v hello stejný adresář jako hello **server.js** souboru a poté přidejte následující obsahu toohello hello **iisnode.yml** souboru:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Tato cesta je, kde hello **node.exe** soubor v projektu budou umístěné, když jste publikovali vaší aplikace toohello webu Azure.
6. Publikování aplikace. Například vzhledem k tomu, že jste vytvořili nový web pomocí parametru – git hello dříve, hello následující příkazy přidáte hello aplikace soubory toomy místní úložiště Git a vložit je toohello webu úložiště:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Vydaného aplikace hello otevřete hello web v prohlížeči. Měla by se zobrazit zpráva s oznámením "Hello z Azure spuštěné verze uzlu: v0.8.1".

## <a name="next-steps"></a>Další kroky
Teď, když budete rozumět tomu, jak se používají toospecify hello verze Node.js v aplikaci, zjistěte, jak příliš[práce s moduly], [sestavení a nasazení webu Node.js](app-service-web/app-service-web-get-started-nodejs.md), a [jak toouse hello Azure Nástroje příkazového řádku pro Mac a Linux].

Další informace najdete v tématu hello [středisku pro vývojáře Node.js](https://azure.microsoft.com/develop/nodejs/).

[jak toouse hello Azure Nástroje příkazového řádku pro Mac a Linux]:cli-install-nodejs.md
[nástroje příkazového řádku Azure]:cli-install-nodejs.md
[práce s moduly]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
