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
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a>Použijte konfiguraci PM2 pro Node.js v Azure webové aplikace v systému Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Pokud jste nastavili tooNode.js zásobníku hello aplikaci pro webové aplikace Azure v systému Linux, získáte možnost tooset hello spuštění souboru Node.js jak ukazuje následující obrázek hello:

![Nastavit spouštěcí soubor Node.js][1]

Můžete použít tuto možnost toodo jeden Dobrý den následující úlohy:

* Zadejte hello spouštěcí skript pro vaši aplikaci Node.js (například: /bin/server.js).
* Zadejte hello PM2 konfigurační soubor toouse pro vaši aplikaci Node.js (například: /foo/process.json).
  
  > [!NOTE]
  > Pokud chcete, vaše toorestart procesy Node.js automaticky při změně určitých souborů, použijte hello PM2 konfiguraci. Vaše aplikace nebude jinak restartujte při přijetí oznámení o změnách (například při změně kódu aplikace).
  > 
  > 

Můžete zkontrolovat hello Node.js [zpracovat soubor dokumentace](http://pm2.keymetrics.io/docs/usage/application-declaration/) pro všechny hello možnosti, ale tady je ukázka lze použít jako souboru process.json:

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

Toonote důležité věci v této konfiguraci jsou:

* Vlastnost "skript" Hello Určuje skript spuštění vaší aplikace.
* Vlastnost "instances" Hello Určuje, kolik instancí toolaunch procesu uzlu hello. Pokud vaše aplikace běží na větší virtuální počítače, které mají více jader, je vhodné toomaximize prostředkům tady nastavením na vyšší hodnotu.
* Hello "sledovat", že pole určuje všechny soubory, které mají toorestart hello proces uzlu, když se změní.
* Pro "watch_options" hello aktuálně musíte toospecify "usePolling" jako hodnotu PRAVDA z důvodu hello způsobem, který je připojený obsahu aplikace.

## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](app-service-linux-intro.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
