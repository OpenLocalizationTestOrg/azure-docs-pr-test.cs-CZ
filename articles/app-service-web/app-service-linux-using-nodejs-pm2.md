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
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a>Použijte konfiguraci PM2 pro Node.js v Azure webové aplikace v systému Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Pokud nastavíte zásobník aplikací Node.js pro webové aplikace Azure v systému Linux, získáte možnost nastavit spouštěcí soubor Node.js, jak je znázorněno na následujícím obrázku:

![Nastavit spouštěcí soubor Node.js][1]

Tuto možnost můžete použít pro jednu z následujících úloh:

* Zadejte spouštěcí skript pro vaši aplikaci Node.js (například: /bin/server.js).
* Zadejte PM2 konfigurační soubor pro aplikace Node.js (například: /foo/process.json).
  
  > [!NOTE]
  > Pokud chcete, aby vaše Node.js procesy automaticky restartovat při změně určitých souborů, použijte PM2 konfiguraci. Vaše aplikace nebude jinak restartujte při přijetí oznámení o změnách (například při změně kódu aplikace).
  > 
  > 

Můžete zkontrolovat Node.js [zpracovat soubor dokumentace](http://pm2.keymetrics.io/docs/usage/application-declaration/) pro všechny možnosti, ale následující je ukázka můžete použít jako souboru process.json:

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

Důležitá poznámka: v této konfiguraci věci:

* Vlastnost "skript" Určuje skript spuštění vaší aplikace.
* Vlastnost "instances" Určuje, kolik instancí procesu uzlu ke spuštění. Pokud vaše aplikace běží na větší virtuální počítače, které mají více jader, je vhodné maximalizovat prostředkům tady nastavením na vyšší hodnotu.
* Pole "sledovat" Určuje všechny soubory, které chcete restartovat proces uzlu, když se změní.
* Pro "watch_options" budete muset aktuálně zadat "usePolling" jako hodnotu PRAVDA z důvodu způsobem, ke kterému je připojena obsahu aplikace.

## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](app-service-linux-intro.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
