---
title: "Vytvoření webové aplikace Azure systémem Linux | Microsoft Docs"
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
ms.openlocfilehash: 49091d4a85bed23927850f9c0bbc5ea8b6e8c9e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a>Vytvoření webové aplikace Azure systémem Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-the-azure-portal-to-create-your-web-app"></a>Pomocí portálu Azure k vytvoření webové aplikace
Můžete začít s vytvářením webové aplikace v systému Linux z [portál Azure](https://portal.azure.com) jak je znázorněno na následujícím obrázku:

![Zahájení vytváření webové aplikace na portálu Azure][1]

Dále **vytvořit okno** otevře, jak je znázorněno na následujícím obrázku:

![V okně Vytvořit][2]

1. Zadejte název webové aplikace.
2. Vyberte existující skupinu prostředků nebo vytvořte novou. (Viz dostupné oblasti v [omezení části](app-service-linux-intro.md).)
3. Vyberte existující plán služby Azure App Service nebo vytvořte novou. (Naleznete v poznámkách k plánu služby App Service v [omezení části](app-service-linux-intro.md).)
4. Zvolte zásobník aplikací, který chcete použít. Můžete si vybrat mezi několika verzích rozhraní Node.js, PHP, .net Core a Ruby.

Po vytvoření aplikace můžete změnit zásobník aplikací z nastavení aplikace jak je znázorněno na následujícím obrázku:

![Nastavení aplikace][3]

## <a name="deploy-your-web-app"></a>Nasazení webové aplikace
Výběr **možnosti nasazení** ze správy portál vám dává možnost použít místní úložiště Git nebo GitHub pro nasazení aplikace. Zbývající pokyny jsou podobné jako u jiných Linux webové aplikace. Můžete postupovat podle pokynů v [místní nasazení Git](app-service-deploy-local-git.md) nebo [průběžné nasazování](app-service-continuous-deployment.md) k nasazení své aplikace.

FTP můžete použít taky k nahrání aplikace do vaší lokality. Koncový bod FTP pro vaši webovou aplikaci můžete získat z části protokolů diagnostiky, jak je znázorněno na následujícím obrázku:

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
