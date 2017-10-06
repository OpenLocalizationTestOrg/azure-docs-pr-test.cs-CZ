---
title: "aaaIntroduction tooAzure webové aplikace v systému Linux | Microsoft Docs"
description: "Další informace o Azure webové aplikace v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 43b9865ade251909a77429eb3e18fe0bcaac3bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-web-app-on-linux"></a>Úvod tooAzure webové aplikace v systému Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Přehled
Zákazníci mohou používat webové aplikace ve webových aplikacích toohost Linux nativně v systému Linux pro zásobníky podporovaných aplikací. Hello následující části jsou uvedeny zásobníky hello aplikace, které jsou aktuálně podporovány. 

## <a name="features"></a>Funkce
Webové aplikace v systému Linux aktuálně podporuje následující zásobníky aplikace hello:

* Node.js
    * 4.4
    * 4.5
    * 6.2
    * 6.6
    * 6.9
    * 6.10
    * 6.11
    * 8.0
    * 8.1
* PHP
    * 5.6
    * 7.0
* .NET core
    * 1.0
    * 1.1
* Ruby
    * 2.3

Zákazníci můžou nasazovat svých aplikacích pomocí:

* FTP
* Místní Git
* GitHubu
* Bitbucket

Pro škálování aplikací:

* Zákazníci mohou nahoru a dolů škálování webové aplikace tak, že změníte úroveň hello jejich plánu služby App Service
* Zákazníci můžete škálovat aplikace a spustit více instancí aplikace v rámci hello rámec jejich SKU

Pro Kudu, některé základní funkce hello:

* Prostředí
* Nasazení
* Základní konzoly
* SSH

Pro devops:

* Přípravná prostředí
* ACR a DockerHub CI/CD

## <a name="limitations"></a>Omezení
Hello portálu Azure jsou pouze funkce, které aktuálně fungují pro webovou aplikaci v systému Linux a skryje hello rest. Jak jsme povolit další funkce, bude zobrazovat na portálu hello.

Některé funkce, například integrace virtuální sítě, ověřování Azure Active Directory nebo třetích stran nebo rozšíření lokality Kudu, ještě nejsou k dispozici. Jakmile se tyto funkce jsou k dispozici, budeme aktualizovat naší dokumentaci a blog o změnách hello.

Tuto verzi public preview je aktuálně k dispozici pouze v hello následující oblasti:

* Západní USA
* Východ USA
* Západní Evropa
* Severní Evropa
* Střed USA – jih
* Střed USA – sever
* Jihovýchodní Asie
* Východní Asie
* Austrálie – východ
* Japonsko – východ
* Brazílie – jih
* Indie – jih

Webové aplikace v systému Linux je podporována pouze v rámci plánů služby vyhrazené aplikace hello a nemá úroveň Free nebo sdílené. Také plány služby App Service pro regulární a Linux webové aplikace se vzájemně vylučují, proto nelze vytvořit webovou aplikaci Linux v plánu služby app-systému Linux.

Webové aplikace v systému Linux musí být vytvořeny ve skupině prostředků, který neobsahuje jiné Linux webové aplikace v hello stejné oblasti.

## <a name="troubleshooting"></a>Řešení potíží ##

Když toostart selhání aplikace nebo chcete toocheck hello protokolování z vaší aplikace, zkontrolujte hello Docker protokolů v adresáři LogFiles hello. Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.
toolog hello `stdout` a `stderr` z kontejneru, je nutné tooenable **kontejner Docker protokolování** pod **protokolů diagnostiky**.

![Povolení protokolování][2]

![Pomocí modulu Kudu tooview Docker protokolů][1]

Dostanete hello SCM lokality z **Rozšířené nástroje** v hello **nástroje pro vývoj** nabídky.

## <a name="next-steps"></a>Další kroky
V tématu hello následující odkazy tooget spuštění službou App Service v systému Linux. Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux](app-service-linux-using-custom-docker-image.md)
* [Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux](app-service-linux-using-nodejs-pm2.md)
* [Pomocí .NET Core v webové aplikace Azure App Service v systému Linux](app-service-linux-using-dotnetcore.md)
* [Použití Ruby v webové aplikace Azure App Service v systému Linux](app-service-linux-ruby-get-started.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)
* [Podpora SSH pro webové aplikace Azure v systému Linux](./app-service-linux-ssh-support.md)
* [Nastavení přípravných prostředí v Azure App Service](./web-sites-staged-publishing.md)
* [Docker Hub průběžné nasazování pomocí Azure webové aplikace v systému Linux](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png