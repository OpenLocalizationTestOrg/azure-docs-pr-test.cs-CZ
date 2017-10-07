---
title: aaaAdd tooAzure aplikace Java App Service Web Apps
description: "Tento kurz ukazuje, jak tooadd stránky nebo aplikace tooyour instanci Azure App Service Web Apps, který je již nakonfigurován toouse Java."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 2feb464b2933921ad2887779a6b7589634e2e2f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-java-application-tooazure-app-service-web-apps"></a>Přidat tooAzure aplikace Java App Service Web Apps
Když jste inicializovali webové aplikace Java v [Azure App Service] [ Azure App Service] podle postupu uvedeného v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md), tím, že můžete nahrát vaší aplikace vaše WAR v hello **webapps** složky.

Hello navigační cesta toohello **webapps** složky liší v závislosti na tom, jak nastavit instanci webové aplikace.

* Pokud jste nastavili vaší webové aplikace pomocí hello Azure Marketplace, hello cesta toohello **webapps** složka je v podobě hello **d:\home\site\wwwroot\bin\application\_server\webapps**, kde **aplikace\_server** je hello název serveru aplikace hello platí pro instanci vaší webové aplikace. 
* Pokud jste nastavili vaší webové aplikace pomocí hello konfigurace Azure uživatelského rozhraní, hello cesta toohello **webapps** složka je v podobě hello **d:\home\site\wwwroot\webapps**. 

Poznámka tooupload řízení zdroje můžete použít, aplikace nebo webové stránky, včetně [scénáře průběžnou integraci](app-service-continuous-deployment.md). FTP je také možnost pro nahrávání aplikace nebo webové stránky; Další informace o nasazení aplikací přes FTP, najdete v části [nasazení vaší aplikace tooAzure služby App Service].

Poznámka: pro Tomcat webové aplikace: Jakmile jste odeslali vaší toohello soubor WAR **webapps** složky, aplikační server Tomcat hello zjistí, že jste jej přidali a automaticky ho načte. Všimněte si, že při kopírování souborů (jiné než soubory WAR) toohello kořenový adresář, budou potřebovat hello aplikační server, toobe restartován tyto soubory se používají. Funkce autoload Hello hello Tomcat Java webových aplikací běžících na Azure je založena na nový soubor WAR přidávané nebo nové soubory nebo adresáře přidat toohello **webapps** složky. 

<a name="see-also"></a>

## <a name="see-also"></a>Viz také
Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].

[Application-insights-App-insights-Java-Get-Started](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[nasazení vaší aplikace tooAzure služby App Service]: ./web-sites-deploy.md
