---
title: "Použití Ruby v webové aplikace Azure App Service v systému Linux | Microsoft Docs"
description: "Použití Ruby ve webové aplikace Azure App Service v systému Linux."
keywords: "služby Azure app service, webové aplikace, – nejčastější dotazy, linux, operačních systémů, ruby"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 56105d1bc153e552e12c0c408c8f6075e4eff9d0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a>Použití Ruby v webové aplikace v systému Linux #

Pomocí nejnovější aktualizace na našem back-end zavedli jsme podporu pro poznámky Ruby v.2.3. Nastavením konfigurace webové aplikace v systému Linux, můžete změnit v zásobníku aplikace.

## <a name="using-the-azure-portal"></a>Použití webu Azure Portal ##

Z nové nabídky v [portál Azure](https://portal.azure.com), můžete k vytvoření webové aplikace v systému Linux z Web + mobilní možnosti, jak je znázorněno na následujícím obrázku:

![Zahájení vytváření webové aplikace na portálu Azure][1]

Dále **vytvořit okno** otevře, jak je znázorněno na následujícím obrázku:

![V okně Vytvořit][2]

1. Zadejte název webové aplikace.
2. Vyberte existující skupinu prostředků nebo vytvořte novou. (Viz dostupné oblasti v [omezení části](app-service-linux-intro.md).)
3. Vyberte existující plán služby Azure App Service nebo vytvořte novou. (Naleznete v poznámkách k plánu služby App Service v [omezení části](app-service-linux-intro.md).)
4. Její vyberte z předdefinovaných Runtime zásobníky.

Po vytvoření získá Ruby webové aplikace, můžete nasadit se pomocí Git a FTP.

Další informace o vytváření Ruby aplikace, zkontrolujte [Příručka Začínáme get](app-service-linux-ruby-get-started.md)

## <a name="next-steps"></a>Další kroky
* [Co je webová aplikace v systému Linux?](app-service-linux-intro.md)
* [Místní nasazení z Gitu do služby Azure App Service](app-service-deploy-local-git.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)
* [Vytvoření aplikace pro poznámky Ruby pomocí Azure webové aplikace v systému Linux](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png