---
title: "Konfigurace aaaUsing PM2 pro Node.js v Azure webové aplikace v systému Linux | Microsoft Docs"
description: "Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux"
keywords: "služby Azure app service, webové aplikace, nodejs, pm2, linux, operačních systémů"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: fb420f32-6d74-49c7-992f-0ed5616e66e7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 923783ffe656e01c43318899d1a656b553ebb5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a><span data-ttu-id="b5c17-104">Použijte konfiguraci PM2 pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="b5c17-104">Use PM2 configuration for Node.js in Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="b5c17-105">Pokud jste nastavili tooNode.js zásobníku hello aplikaci pro webové aplikace Azure v systému Linux, získáte možnost tooset hello spuštění souboru Node.js jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="b5c17-105">If you set hello application stack tooNode.js for Azure Web App on Linux, you get hello option tooset a Node.js startup file as shown in hello following image:</span></span>

![Nastavit spouštěcí soubor Node.js][1]

<span data-ttu-id="b5c17-107">Můžete použít tuto možnost toodo jeden Dobrý den následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="b5c17-107">You can use this option toodo one of hello following tasks:</span></span>

* <span data-ttu-id="b5c17-108">Zadejte hello spouštěcí skript pro vaši aplikaci Node.js (například: /bin/server.js).</span><span class="sxs-lookup"><span data-stu-id="b5c17-108">Specify hello startup script for your Node.js app (for example: /bin/server.js).</span></span>
* <span data-ttu-id="b5c17-109">Zadejte hello PM2 konfigurační soubor toouse pro vaši aplikaci Node.js (například: /foo/process.json).</span><span class="sxs-lookup"><span data-stu-id="b5c17-109">Specify hello PM2 configuration file toouse for your Node.js app (for example: /foo/process.json).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="b5c17-110">Pokud chcete, vaše toorestart procesy Node.js automaticky při změně určitých souborů, použijte hello PM2 konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b5c17-110">If you want your Node.js processes toorestart automatically when certain files are modified, use hello PM2 configuration.</span></span> <span data-ttu-id="b5c17-111">Vaše aplikace nebude jinak restartujte při přijetí oznámení o změnách (například při změně kódu aplikace).</span><span class="sxs-lookup"><span data-stu-id="b5c17-111">Otherwise, your application won't restart when it receives change notifications (for example, when your application code changes).</span></span>
  > 
  > 

<span data-ttu-id="b5c17-112">Můžete zkontrolovat hello Node.js [zpracovat soubor dokumentace](http://pm2.keymetrics.io/docs/usage/application-declaration/) pro všechny hello možnosti, ale tady je ukázka lze použít jako souboru process.json:</span><span class="sxs-lookup"><span data-stu-id="b5c17-112">You can check hello Node.js [process file documentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) for all hello options, but following is a sample of what you can use as your process.json file:</span></span>

        {
          "name"        : "worker",
          "script"      : "./bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["./bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

<span data-ttu-id="b5c17-113">Toonote důležité věci v této konfiguraci jsou:</span><span class="sxs-lookup"><span data-stu-id="b5c17-113">Important things toonote in this configuration are:</span></span>

* <span data-ttu-id="b5c17-114">Vlastnost "skript" Hello Určuje skript spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5c17-114">hello "script" property specifies your application's start script.</span></span>
* <span data-ttu-id="b5c17-115">Vlastnost "instances" Hello Určuje, kolik instancí toolaunch procesu uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="b5c17-115">hello "instances" property specifies how many instances of hello node process toolaunch.</span></span> <span data-ttu-id="b5c17-116">Pokud vaše aplikace běží na větší virtuální počítače, které mají více jader, je vhodné toomaximize prostředkům tady nastavením na vyšší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b5c17-116">If you are running your application on larger VMs that have multiple cores, it's a good idea toomaximize your resources by setting a higher value here.</span></span>
* <span data-ttu-id="b5c17-117">Hello "sledovat", že pole určuje všechny soubory, které mají toorestart hello proces uzlu, když se změní.</span><span class="sxs-lookup"><span data-stu-id="b5c17-117">hello "watch" array specifies all files that you want toorestart hello node process for when they change.</span></span>
* <span data-ttu-id="b5c17-118">Pro "watch_options" hello aktuálně musíte toospecify "usePolling" jako hodnotu PRAVDA z důvodu hello způsobem, který je připojený obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5c17-118">For hello "watch_options", you currently need toospecify "usePolling" as true because of hello way your application content is mounted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5c17-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5c17-119">Next steps</span></span>
* [<span data-ttu-id="b5c17-120">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="b5c17-120">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="b5c17-121">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="b5c17-121">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
