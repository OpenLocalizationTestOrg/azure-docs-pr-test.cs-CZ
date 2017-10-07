---
title: "Přehled aplikace aaaWeb | Microsoft Docs"
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
ms.openlocfilehash: ce27519bddd62a7fca6ba1fb23c763d0fc378c2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="web-apps-overview"></a>Přehled Web Apps
*App Service Web Apps* je plně spravovaná výpočetní platforma, která je optimalizována pro hostování webů a webových aplikací. To [platforma jako služba](https://en.wikipedia.org/wiki/Platform_as_a_service) nabídka (PaaS) Microsoft Azure vám umožní soustředit na obchodní logiku, zatímco Azure zajistí infrastrukturu toorun hello a škálování aplikací.

Hello následující 5minutové video představuje Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

## <a name="what-is-a-web-app-in-app-service"></a>Co je webová aplikace ve službě App Service?
Ve službě App Service *webové aplikace* je hello výpočetní prostředky, které Azure poskytuje k hostování webu či webové aplikace.  

Hello výpočetní prostředky mohou nacházet ve sdílených nebo vyhrazených virtuálních počítačích (VM), v závislosti na cenové úrovni, který zvolíte, hello. Kód aplikace běží ve spravovaném virtuálním počítači, který je izolován od ostatních zákazníků.

Kód může být v libovolném jazyce nebo rozhraní podporovaném službou [Azure App Service](../app-service/app-service-value-prop-what-is.md), jako je ASP.NET, Node.js, Java, PHP nebo Python. Můžete také spouštět skripty, které ve webové aplikaci používají [PowerShell či jiné skriptovací jazyky](web-sites-create-web-jobs.md#acceptablefiles).

Příklady typických aplikačních scénářů, můžete použít Web Apps pro najdete [scénáře webových aplikací](https://azure.microsoft.com/documentation/scenarios/web-app/) a hello **scénáře a doporučení** části [Azure App Service, Virtual Počítače, Service Fabric a Cloud Services porovnání](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Proč používat službu Web Apps?
Zde jsou některé klíčové funkce, které se vztahují tooWeb aplikace služby App Service:

* **Více jazyků a rozhraní:** App Service zahrnuje prvotřídní podporu pro ASP.NET, Node.js, Javu, PHP a Python. Ve virtuálních počítačích App Service můžete také spouštět [PowerShell a další skripty či spustitelné soubory](web-sites-create-web-jobs.md).
* **Optimalizace DevOps:** Můžete nastavit [průběžnou integraci a nasazení](app-service-continuous-deployment.md) pomocí služeb Visual Studio Team Services, GitHub nebo BitBucket. Aktualizace lze podporovat prostřednictvím [testovacího a přípravného prostředí](web-sites-staged-publishing.md). Máte možnost [testování A/B](app-service-web-test-in-production-get-start.md). Spravovat aplikace v App Service pomocí [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo hello [multiplatformního rozhraní příkazového řádku (CLI)](../cli-install-nodejs.md).
* **Globální škálování s vysokou dostupností:** Můžete ručně i automaticky škálovat pro účely [vertikálního](web-sites-scale.md) nebo [horizontálního](../monitoring-and-diagnostics/insights-how-to-scale.md) navýšení kapacity. Umožňuje hostování vašich aplikací kdekoli v společnosti Microsoft globální infrastruktuře datacenter a hello služby App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) zaručuje vysokou dostupnost.
* **Připojení tooSaaS platformy a místní data** – zvolte z více než 50 [konektory](../connectors/apis-list.md) pro firemní systémy (například SAP, Siebel a Oracle), služby SaaS (například služby Salesforce nebo Office 365) a internet služby (například Facebook nebo Twitter). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuálních sítí Azure](web-sites-integrate-with-vnet.md).
* **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/).
* **Šablony aplikací** – zvolte z rozsáhlého seznamu šablon aplikací hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) které vám umožní používat Průvodce tooinstall oblíbených open-source softwaru jako je WordPress, Joomla nebo Drupal.
* **Integrace aplikace Visual Studio** – vyhrazené nástroje v sadě Visual Studio zjednodušují práci hello při vytváření, nasazování a ladění.

Kromě toho může webová aplikace využívat funkce služeb [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (např. podpora CORS) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (např. nabízená oznámení). Další informace o typech aplikací ve službě App Service naleznete v tématu [Přehled služby Azure App Service](../app-service/app-service-value-prop-what-is.md).

Kromě Web Apps ve službě App Service nabízí Azure další služby, které lze použít k hostování webů a webových aplikací. Webové aplikace pro většinu scénářů je nejlepší volbou hello.  V případě architektury mikroslužby zvažte [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)a pokud potřebujete větší kontrolu nad hello virtuálních počítačů, které kód spuštěn, zvažte [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Další informace o tom, toochoose mezi tyto služby Azure, najdete v části [porovnání služby Azure App Service, virtuální počítače, Service Fabric a Cloud Services](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Začínáme
tooget spustí nasazení ukázkový kód tooa novou webovou aplikaci ve službě App Service, postupujte podle jednoho z hello kurzy v následujících rozevíracím hello. Budete potřebovat bezplatný účet Azure.

> [!div class="op_single_selector"]
> * [Nasazení vaší první tooAzure webové aplikace ASP.NET během 5 minut](app-service-web-get-started-dotnet.md)
> * [Nasazení vaší první tooAzure webové aplikace PHP během 5 minut](app-service-web-get-started-php.md)
> * [Nasazení vaší první tooAzure webové aplikace Node.js během 5 minut](app-service-web-get-started-nodejs.md)
> * [Nasazení vaší první tooAzure webové aplikace Java během 5 minut](app-service-web-get-started-java.md)
> * [Nasazení vaší první tooAzure webové aplikace Python během 5 minut](app-service-web-get-started-python.md)
> * [Nasazení vaší první lokality tooAzure HTML během 5 minut](app-service-web-get-started-html.md)
> 
> 

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 
