---
title: "Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 5002400a673e2c5cc4290bab488b839fb2282966
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a><span data-ttu-id="769a2-104">Použijte konfiguraci PM2 pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="769a2-104">Use PM2 configuration for Node.js in Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="769a2-105">Pokud nastavíte zásobník aplikací Node.js pro webové aplikace Azure v systému Linux, získáte možnost nastavit spouštěcí soubor Node.js, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="769a2-105">If you set the application stack to Node.js for Azure Web App on Linux, you get the option to set a Node.js startup file as shown in the following image:</span></span>

![Nastavit spouštěcí soubor Node.js][1]

<span data-ttu-id="769a2-107">Tuto možnost můžete použít pro jednu z následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="769a2-107">You can use this option to do one of the following tasks:</span></span>

* <span data-ttu-id="769a2-108">Zadejte spouštěcí skript pro vaši aplikaci Node.js (například: /bin/server.js).</span><span class="sxs-lookup"><span data-stu-id="769a2-108">Specify the startup script for your Node.js app (for example: /bin/server.js).</span></span>
* <span data-ttu-id="769a2-109">Zadejte PM2 konfigurační soubor pro aplikace Node.js (například: /foo/process.json).</span><span class="sxs-lookup"><span data-stu-id="769a2-109">Specify the PM2 configuration file to use for your Node.js app (for example: /foo/process.json).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="769a2-110">Pokud chcete, aby vaše Node.js procesy automaticky restartovat při změně určitých souborů, použijte PM2 konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="769a2-110">If you want your Node.js processes to restart automatically when certain files are modified, use the PM2 configuration.</span></span> <span data-ttu-id="769a2-111">Vaše aplikace nebude jinak restartujte při přijetí oznámení o změnách (například při změně kódu aplikace).</span><span class="sxs-lookup"><span data-stu-id="769a2-111">Otherwise, your application won't restart when it receives change notifications (for example, when your application code changes).</span></span>
  > 
  > 

<span data-ttu-id="769a2-112">Můžete zkontrolovat Node.js [zpracovat soubor dokumentace](http://pm2.keymetrics.io/docs/usage/application-declaration/) pro všechny možnosti, ale následující je ukázka můžete použít jako souboru process.json:</span><span class="sxs-lookup"><span data-stu-id="769a2-112">You can check the Node.js [process file documentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) for all the options, but following is a sample of what you can use as your process.json file:</span></span>

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

<span data-ttu-id="769a2-113">Důležitá poznámka: v této konfiguraci věci:</span><span class="sxs-lookup"><span data-stu-id="769a2-113">Important things to note in this configuration are:</span></span>

* <span data-ttu-id="769a2-114">Vlastnost "skript" Určuje skript spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="769a2-114">The "script" property specifies your application's start script.</span></span>
* <span data-ttu-id="769a2-115">Vlastnost "instances" Určuje, kolik instancí procesu uzlu ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="769a2-115">The "instances" property specifies how many instances of the node process to launch.</span></span> <span data-ttu-id="769a2-116">Pokud vaše aplikace běží na větší virtuální počítače, které mají více jader, je vhodné maximalizovat prostředkům tady nastavením na vyšší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="769a2-116">If you are running your application on larger VMs that have multiple cores, it's a good idea to maximize your resources by setting a higher value here.</span></span>
* <span data-ttu-id="769a2-117">Pole "sledovat" Určuje všechny soubory, které chcete restartovat proces uzlu, když se změní.</span><span class="sxs-lookup"><span data-stu-id="769a2-117">The "watch" array specifies all files that you want to restart the node process for when they change.</span></span>
* <span data-ttu-id="769a2-118">Pro "watch_options" budete muset aktuálně zadat "usePolling" jako hodnotu PRAVDA z důvodu způsobem, ke kterému je připojena obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="769a2-118">For the "watch_options", you currently need to specify "usePolling" as true because of the way your application content is mounted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="769a2-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="769a2-119">Next steps</span></span>
* [<span data-ttu-id="769a2-120">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="769a2-120">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="769a2-121">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="769a2-121">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
