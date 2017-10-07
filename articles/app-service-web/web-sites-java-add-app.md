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
# <a name="add-a-java-application-tooazure-app-service-web-apps"></a><span data-ttu-id="f349a-103">Přidat tooAzure aplikace Java App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="f349a-103">Add a Java application tooAzure App Service Web Apps</span></span>
<span data-ttu-id="f349a-104">Když jste inicializovali webové aplikace Java v [Azure App Service] [ Azure App Service] podle postupu uvedeného v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md), tím, že můžete nahrát vaší aplikace vaše WAR v hello **webapps** složky.</span><span class="sxs-lookup"><span data-stu-id="f349a-104">Once you have initialized your Java web app in [Azure App Service][Azure App Service] as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md), you can upload your application by placing your WAR in hello **webapps** folder.</span></span>

<span data-ttu-id="f349a-105">Hello navigační cesta toohello **webapps** složky liší v závislosti na tom, jak nastavit instanci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f349a-105">hello navigation path toohello **webapps** folder differs based on how you set up your Web Apps instance.</span></span>

* <span data-ttu-id="f349a-106">Pokud jste nastavili vaší webové aplikace pomocí hello Azure Marketplace, hello cesta toohello **webapps** složka je v podobě hello **d:\home\site\wwwroot\bin\application\_server\webapps**, kde **aplikace\_server** je hello název serveru aplikace hello platí pro instanci vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f349a-106">If you set up your web app by using hello Azure Marketplace, hello path toohello **webapps** folder is in hello form **d:\home\site\wwwroot\bin\application\_server\webapps**, where **application\_server** is hello name of hello application server in effect for your Web Apps instance.</span></span> 
* <span data-ttu-id="f349a-107">Pokud jste nastavili vaší webové aplikace pomocí hello konfigurace Azure uživatelského rozhraní, hello cesta toohello **webapps** složka je v podobě hello **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="f349a-107">If you set up your web app by using hello Azure configuration UI, hello path toohello **webapps** folder is in hello form **d:\home\site\wwwroot\webapps**.</span></span> 

<span data-ttu-id="f349a-108">Poznámka tooupload řízení zdroje můžete použít, aplikace nebo webové stránky, včetně [scénáře průběžnou integraci](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f349a-108">Note that you can use source control tooupload your application or web pages, including [continuous integration scenarios](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="f349a-109">FTP je také možnost pro nahrávání aplikace nebo webové stránky; Další informace o nasazení aplikací přes FTP, najdete v části [nasazení vaší aplikace tooAzure služby App Service].</span><span class="sxs-lookup"><span data-stu-id="f349a-109">FTP is also an option for uploading your application or web pages; for more information about deploying your applications over FTP, see [Deploy your app tooAzure App Service].</span></span>

<span data-ttu-id="f349a-110">Poznámka: pro Tomcat webové aplikace: Jakmile jste odeslali vaší toohello soubor WAR **webapps** složky, aplikační server Tomcat hello zjistí, že jste jej přidali a automaticky ho načte.</span><span class="sxs-lookup"><span data-stu-id="f349a-110">Note for Tomcat web apps: Once you've uploaded your WAR file toohello **webapps** folder, hello Tomcat application server will detect that you've added it and will automatically load it.</span></span> <span data-ttu-id="f349a-111">Všimněte si, že při kopírování souborů (jiné než soubory WAR) toohello kořenový adresář, budou potřebovat hello aplikační server, toobe restartován tyto soubory se používají.</span><span class="sxs-lookup"><span data-stu-id="f349a-111">Note that if you copy files (other than WAR files) toohello ROOT directory, hello application server will need toobe restarted before those files are used.</span></span> <span data-ttu-id="f349a-112">Funkce autoload Hello hello Tomcat Java webových aplikací běžících na Azure je založena na nový soubor WAR přidávané nebo nové soubory nebo adresáře přidat toohello **webapps** složky.</span><span class="sxs-lookup"><span data-stu-id="f349a-112">hello autoload functionality for hello Tomcat Java web apps running on Azure is based on a new WAR file being added, or new files or directories added toohello **webapps** folder.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="f349a-113">Viz také</span><span class="sxs-lookup"><span data-stu-id="f349a-113">See Also</span></span>
<span data-ttu-id="f349a-114">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="f349a-114">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

[<span data-ttu-id="f349a-115">Application-insights-App-insights-Java-Get-Started</span><span class="sxs-lookup"><span data-stu-id="f349a-115">application-insights-app-insights-java-get-started</span></span>](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[nasazení vaší aplikace tooAzure služby App Service]: ./web-sites-deploy.md
