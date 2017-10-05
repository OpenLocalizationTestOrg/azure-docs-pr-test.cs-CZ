---
title: "Přidání aplikace v jazyce Java do Azure App Service Web Apps"
description: "V tomto kurzu se dozvíte, jak přidat stránky nebo aplikace na instanci služby Azure App Service Web Apps, která již byla konfigurována pro použití Java."
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
ms.openlocfilehash: c28e7c499ed02b759df580f4b14a971b6aec5b67
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a><span data-ttu-id="3ce34-103">Přidání aplikace v jazyce Java do Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="3ce34-103">Add a Java application to Azure App Service Web Apps</span></span>
<span data-ttu-id="3ce34-104">Když jste inicializovali webové aplikace Java v [Azure App Service] [ Azure App Service] podle postupu uvedeného v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md), můžete nahrát aplikaci tím, že vaše WAR v **webapps** složky.</span><span class="sxs-lookup"><span data-stu-id="3ce34-104">Once you have initialized your Java web app in [Azure App Service][Azure App Service] as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md), you can upload your application by placing your WAR in the **webapps** folder.</span></span>

<span data-ttu-id="3ce34-105">Cesta k navigační **webapps** složky liší v závislosti na tom, jak nastavit instanci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce34-105">The navigation path to the **webapps** folder differs based on how you set up your Web Apps instance.</span></span>

* <span data-ttu-id="3ce34-106">Pokud jste nastavili vaší webové aplikace pomocí Azure Marketplace, cesta k **webapps** složka je ve formátu **d:\home\site\wwwroot\bin\application\_server\webapps**, kde **aplikace\_server** je název serveru aplikace platí pro instanci vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce34-106">If you set up your web app by using the Azure Marketplace, the path to the **webapps** folder is in the form **d:\home\site\wwwroot\bin\application\_server\webapps**, where **application\_server** is the name of the application server in effect for your Web Apps instance.</span></span> 
* <span data-ttu-id="3ce34-107">Pokud jste nastavili vaší webové aplikace pomocí uživatelského rozhraní, cesta ke konfiguraci Azure **webapps** složka je ve formátu **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="3ce34-107">If you set up your web app by using the Azure configuration UI, the path to the **webapps** folder is in the form **d:\home\site\wwwroot\webapps**.</span></span> 

<span data-ttu-id="3ce34-108">Správa zdrojového kódu můžete použít k nahrání aplikace nebo webové stránky, včetně [scénáře průběžnou integraci](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="3ce34-108">Note that you can use source control to upload your application or web pages, including [continuous integration scenarios](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="3ce34-109">FTP je také možnost pro nahrávání aplikace nebo webové stránky; Další informace o nasazení aplikací přes FTP, najdete v části [nasazení vaší aplikace do Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="3ce34-109">FTP is also an option for uploading your application or web pages; for more information about deploying your applications over FTP, see [Deploy your app to Azure App Service].</span></span>

<span data-ttu-id="3ce34-110">Poznámka pro Tomcat webové aplikace: Jakmile jste odeslali soubor WAR **webapps** složky, aplikační server Tomcat zjistí, že jste jej přidali a automaticky ho načte.</span><span class="sxs-lookup"><span data-stu-id="3ce34-110">Note for Tomcat web apps: Once you've uploaded your WAR file to the **webapps** folder, the Tomcat application server will detect that you've added it and will automatically load it.</span></span> <span data-ttu-id="3ce34-111">Všimněte si, že při kopírování souborů (jiné než soubory WAR) do kořenového adresáře, aplikační server bude nutné restartovat předtím, než se používají tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="3ce34-111">Note that if you copy files (other than WAR files) to the ROOT directory, the application server will need to be restarted before those files are used.</span></span> <span data-ttu-id="3ce34-112">Funkce autoload pro webové aplikace Tomcat Java, spuštěné v Azure je založena na nový soubor WAR přidávané nebo nové soubory nebo adresáře přidat do **webapps** složky.</span><span class="sxs-lookup"><span data-stu-id="3ce34-112">The autoload functionality for the Tomcat Java web apps running on Azure is based on a new WAR file being added, or new files or directories added to the **webapps** folder.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="3ce34-113">Viz také</span><span class="sxs-lookup"><span data-stu-id="3ce34-113">See Also</span></span>
<span data-ttu-id="3ce34-114">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="3ce34-114">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

[<span data-ttu-id="3ce34-115">Application-insights-App-insights-Java-Get-Started</span><span class="sxs-lookup"><span data-stu-id="3ce34-115">application-insights-app-insights-java-get-started</span></span>](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

<span data-ttu-id="3ce34-116">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="3ce34-116">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
<span data-ttu-id="3ce34-117">[nasazení vaší aplikace do Azure App Service]: ./web-sites-deploy.md</span><span class="sxs-lookup"><span data-stu-id="3ce34-117">[Deploy your app to Azure App Service]: ./web-sites-deploy.md</span></span>
