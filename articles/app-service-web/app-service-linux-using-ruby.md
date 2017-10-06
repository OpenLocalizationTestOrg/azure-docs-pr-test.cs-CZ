---
title: "aaaUsing Ruby v Azure App Service webové aplikace v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 45692cb3bf1da9ff65b9466055029bfaef8b7d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a>Použití Ruby v webové aplikace v systému Linux #

U hello nejnovější aktualizace tooour back-end zavedli jsme podporu pro poznámky Ruby v.2.3. Nastavení konfigurace hello Linux webové aplikace můžete změnit zásobník aplikací hello.

## <a name="using-hello-azure-portal"></a>Pomocí hello portálu Azure ##

Z nové nabídky hello v hello [portál Azure](https://portal.azure.com), můžete zvolit toocreate webové aplikace v systému Linux hello Web + mobilní možnost jak ukazuje následující obrázek hello:

![Zahájení vytváření webové aplikace na hello portálu Azure][1]

V dalším kroku hello **vytvořit okno** otevře, jak ukazuje následující obrázek hello:

![okno Vytvořit Hello][2]

1. Zadejte název webové aplikace.
2. Vyberte existující skupinu prostředků nebo vytvořte novou. (Viz dostupné oblasti v hello [omezení části](app-service-linux-intro.md).)
3. Vyberte existující plán služby Azure App Service nebo vytvořte novou. (Naleznete v poznámkách k plánu služby App Service v hello [omezení části](app-service-linux-intro.md).)
4. Hello Ruby vyberte z předdefinovaných Runtime zásobníky hello.

Po vytvoření získá Ruby webové aplikace, můžete nasadit tooit pomocí Git a FTP.

Další informace o toolearn vytvořit aplikaci, Ruby, zkontrolujte hello [Příručka Začínáme get](app-service-linux-ruby-get-started.md)

## <a name="next-steps"></a>Další kroky
* [Co je webová aplikace v systému Linux?](app-service-linux-intro.md)
* [Místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)
* [Vytvoření aplikace pro poznámky Ruby pomocí Azure webové aplikace v systému Linux](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png