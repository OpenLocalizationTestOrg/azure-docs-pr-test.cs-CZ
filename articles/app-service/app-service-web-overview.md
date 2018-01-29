---
title: "Přehled Web Apps | Dokumentace Microsoftu"
description: "Zjistěte, jak služba Azure App Service pomáhá nasazovat, hostovat a používat webové aplikace"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f8510bb6b412e9af8aad30ba32bc74206c22042f
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Přehled Web Apps

*Azure App Service Web Apps* (nebo jenom Web Apps) je služba pro hostování webových aplikací, rozhraní REST API a mobilních back-endů. Můžete vyvíjet ve svém oblíbeném jazyce, ať už jde o .NET, .NET Core, Javu, Ruby, Node.js, PHP nebo Python. Aplikace můžete snadno spouštět a škálovat na virtuálních počítačích s Windows nebo Linuxem (viz [App Service v Linuxu](containers/app-service-linux-intro.md)). 

Služba Web Apps nejen že do vaší aplikace přináší výkon Microsoft Azure v podobě zabezpečení, vyrovnávání zatížení, automatického škálování nebo automatizované správy. Můžete také využívat její možnosti DevOps, jako jsou průběžné nasazování z VSTS, GitHubu, Docker Hubu a dalších zdrojů, správa balíčků, přípravná prostředí, vlastní doména a certifikáty SSL. 

Se službou App Service platíte jenom za výpočetní prostředky Azure, které využijete. Výpočetní prostředky, které využijete, se určují podle _plánu služby App Service_, ve které službu Web Apps spouštíte. Další informace najdete v tématu [Plány služby App Service v Azure Web Apps](azure-web-sites-web-hosting-plans-in-depth-overview.md).

Následující 5minutové video představuje Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Proč používat službu Web Apps?
Toto jsou některé klíčové funkce služby App Service Web Apps:

* **Více jazyků a rozhraní** – Služba Web Apps zahrnuje prvotřídní podporu jazyků ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP nebo Python. Můžete také spouštět [PowerShell a další skripty nebo spustitelné soubory](web-sites-create-web-jobs.md) jako služby na pozadí.
* **Optimalizace DevOps** – Můžete nastavit [průběžnou integraci a nasazování](app-service-continuous-deployment.md) pomocí služeb Visual Studio Team Services, GitHub, BitBucket, Docker Hub nebo Azure Container Service. Aktualizace lze podporovat prostřednictvím [testovacího a přípravného prostředí](web-sites-staged-publishing.md). Aplikace ve službě Web Apps můžete spravovat pomocí [Azure PowerShellu](/powershell/azureps-cmdlets-docs) nebo [multiplatformního rozhraní příkazového řádku (CLI)](/cli/azure/install-azure-cli).
* **Globální škálování s vysokou dostupností:** Můžete ručně i automaticky škálovat pro účely [vertikálního](web-sites-scale.md) nebo [horizontálního](../monitoring-and-diagnostics/insights-how-to-scale.md) navýšení kapacity. Aplikace je možné hostovat kdekoli v globální infrastruktuře datacenter Microsoftu a smlouva [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) pro App Service zaručuje vysokou dostupnost.
* **Připojení k platformám SaaS a místním datům** – Můžete si vybírat z více než 50 [konektorů](../connectors/apis-list.md) pro podnikové systémy (například SAP), služby SaaS (například Salesforce) a internetové služby (například Facebook). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuálních sítí Azure](web-sites-integrate-with-vnet.md).
* **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/). Uživatele můžete ověřovat pomocí služby [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) nebo přes přihlášení prostřednictvím sociální sítě ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) a [Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Můžete vytvořit [omezení IP adres](app-service-ip-restrictions.md) a [spravovat identity služby](app-service-managed-service-identity.md).
* **Šablony aplikací** – Vyberte si z rozsáhlého seznamu šablon aplikací na webu [Azure Marketplace](https://azure.microsoft.com/marketplace/), například pro systémy WordPress, Joomla nebo Drupal.
* **Integrace sady Visual Studio** – Vyhrazené nástroje v sadě Visual Studio zjednodušují práci při vytváření, nasazování a ladění.
* **Rozhraní API a mobilní funkce** – Služba Web Apps nabízí podporu CORS na klíč pro scénáře rozhraní RESTful API a zjednodušuje scénáře mobilních aplikací díky tomu, že umožňuje ověřování, offline synchronizaci dat, nabízená oznámení a další.
* **Kód bez serveru** – Můžete spouštět fragment kódu nebo skript na vyžádání, aniž byste museli explicitně zřizovat nebo spravovat infrastrukturu, a platíte jenom za výpočetní čas, který váš kód skutečně využije (viz [Azure Functions](/azure/azure-functions/)).

Kromě Web Apps ve službě App Service nabízí Azure další služby, které lze použít k hostování webů a webových aplikací. Pro většinu scénářů je nejlepší volbou služba Web Apps.  Pro architekturu mikroslužeb zvažte [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Pokud potřebujete větší kontrolu nad virtuálními počítači, na kterých se kód spouští, zvažte službu [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Příklady typických aplikačních scénářů, k nimž můžete použít službu Web Apps, naleznete v tématu [Srovnání Azure App Service, Virtual Machines, Service Fabric a Cloud Services](choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Další kroky

Vytvořte svou první aplikaci.

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

