---
title: "aaaCreate Azure webová aplikace spuštěna v systému Linux | Microsoft Docs"
description: "Webové aplikace Tvorba pracovního postupu pro webové aplikace Azure v systému Linux."
keywords: "služby Azure app service, webové aplikace, linux, operačních systémů"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: de1bd030345d5e2a8024012067b5bcaa2cca09dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a>Vytvoření webové aplikace Azure systémem Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-hello-azure-portal-toocreate-your-web-app"></a>Použít hello Azure portálu toocreate vaší webové aplikace
Můžete začít s vytvářením webové aplikace v systému Linux z hello [portál Azure](https://portal.azure.com) jak ukazuje následující obrázek hello:

![Zahájení vytváření webové aplikace na hello portálu Azure][1]

V dalším kroku hello **vytvořit okno** otevře, jak ukazuje následující obrázek hello:

![okno Vytvořit Hello][2]

1. Zadejte název webové aplikace.
2. Vyberte existující skupinu prostředků nebo vytvořte novou. (Viz dostupné oblasti v hello [omezení části](app-service-linux-intro.md).)
3. Vyberte existující plán služby Azure App Service nebo vytvořte novou. (Naleznete v poznámkách k plánu služby App Service v hello [omezení části](app-service-linux-intro.md).)
4. Vyberte aplikace hello zásobníku, že máte v úmyslu toouse. Můžete si vybrat mezi několika verzích rozhraní Node.js, PHP, .net Core a Ruby.

Po vytvoření aplikace hello, můžete změnit zásobník aplikací hello z nastavení aplikace hello jak ukazuje následující obrázek hello:

![Nastavení aplikace][3]

## <a name="deploy-your-web-app"></a>Nasazení webové aplikace
Výběr **možnosti nasazení** z portálu poskytuje správu hello je hello možnost toouse místní Git nebo Githubu úložiště toodeploy vaší aplikace. Hello zbytek hello pokyny jsou podobné toothose pro webovou aplikaci systému Linux. Můžete postupovat podle pokynů hello v [místní nasazení Git](app-service-deploy-local-git.md) nebo [průběžné nasazování](app-service-continuous-deployment.md) toodeploy vaší aplikace.

Můžete také pomocí FTP tooupload webu tooyour aplikace. Můžete získat koncový bod hello FTP pro vaši webovou aplikaci z hello diagnostiky protokoly části Jak ukazuje následující obrázek hello:

![Diagnostické protokoly][4]

## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](app-service-linux-intro.md)
* [Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux](app-service-linux-using-nodejs-pm2.md)
* [Použití Ruby v webové aplikace Azure App Service v systému Linux](app-service-linux-ruby-get-started.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
