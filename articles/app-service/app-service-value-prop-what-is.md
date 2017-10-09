---
title: "aaaAzure služby App Service pro webové, mobilní a aplikací API | Microsoft Docs"
description: "Zjistěte, jak vám může Azure App Service pomoci s vývojem, nasazením a správou webových a mobilních aplikací."
keywords: "app service, azure app service, cena app service, škálování, škálovatelné, nasazení aplikace, nasazení aplikace azure, paas, platforma jako služba, web, azure mobile"
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: 22f414c7d79092d87406a8d3538b946881fb4580
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-app-service"></a>Co je Azure App Service?
*App Service* je služba Microsoft Azure typu [platforma jako služba](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS). Webové a mobilní aplikace můžete vytvářet pro libovolnou platformu nebo zařízení. Své aplikace můžete integrovat s řešeními SaaS a propojovat s lokálními aplikacemi. A lze také automatizovat firemní procesy. Azure spouští vaše aplikace na plně spravovaných virtuálních počítačích (VM) se sdílenými prostředky, které si sami zvolíte, nebo na vyhrazených virtuálních počítačích.

App Service zahrnuje hello webové a mobilní funkce, které jsme dříve nabízeli samostatně jako weby Azure a Azure Mobile Services. Obsahuje také nové možnosti pro automatizaci obchodních procesů a hostování cloudových rozhraní API. Jako jediná integrovaná služba vám App Service umožňuje vytvářet různé komponenty – weby, back-endy mobilních aplikací, rozhraní RESTful API a firemní procesy – v jediném řešení.

Hello následující 4minutové video obsahuje stručné vysvětlení vztah tooearlier Azure App Service nabídky a co je nového v ní.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Proč používat App Service?
Toto jsou některé klíčové funkce a možnosti služby App Service:

* **Více jazyků a rozhraní:** App Service zahrnuje prvotřídní podporu pro ASP.NET, Node.js, Javu, PHP a Python. Na virtuálních počítačích App Service můžete také spouštět [Windows PowerShell a další skripty nebo spustitelné soubory](../app-service-web/web-sites-create-web-jobs.md).
* **Optimalizace DevOps:** Můžete nastavit [průběžnou integraci a nasazení](../app-service-web/app-service-continuous-deployment.md) pomocí služeb Visual Studio Team Services, GitHub nebo BitBucket. Aktualizace lze podporovat prostřednictvím [testovacího a přípravného prostředí](../app-service-web/web-sites-staged-publishing.md). Máte možnost [testování A/B](../app-service-web/app-service-web-test-in-production-get-start.md). Spravovat aplikace v App Service pomocí [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo hello [multiplatformního rozhraní příkazového řádku (CLI)](../cli-install-nodejs.md).
* **Globální škálování s vysokou dostupností:** Můžete ručně i automaticky škálovat pro účely [vertikálního](../app-service-web/web-sites-scale.md) nebo [horizontálního](../monitoring-and-diagnostics/insights-how-to-scale.md) navýšení kapacity. Umožňuje hostování vašich aplikací kdekoli v společnosti Microsoft globální infrastruktuře datacenter a hello služby App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) zaručuje vysokou dostupnost.
* **Připojení tooSaaS platformy a místní data** – zvolte z více než 50 [konektory](../connectors/apis-list.md) pro firemní systémy (například SAP, Siebel a Oracle), služby SaaS (například služby Salesforce nebo Office 365) a internet služby (například Facebook nebo Twitter). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuálních sítí Azure](../app-service-web/web-sites-integrate-with-vnet.md).
* **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/).
* **Šablony aplikací** – zvolte z rozsáhlého seznamu šablon v hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) které vám umožní používat Průvodce tooinstall oblíbených open-source softwaru jako je WordPress, Joomla nebo Drupal.
* **Integrace aplikace Visual Studio** – vyhrazené nástroje v sadě Visual Studio zjednodušují práci hello při vytváření, nasazování a ladění.

## <a name="app-types-in-app-service"></a>Typy aplikací v App Service
App Service nabízí několik *typy aplikací*, každý z nich je určený toohost konkrétní úlohu:

* [**Webové aplikace**](../app-service-web/app-service-web-overview.md) – k hostování webů a webových aplikací
* [**Mobilní aplikace**](../app-service-mobile/app-service-mobile-value-prop.md) – k hostování back-endu mobilních aplikací
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) – k hostování rozhraní RESTful API.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) – k automatizaci obchodních procesů a integraci systémů a dat napříč cloudy bez psaní kódu.

Hello word *aplikace* zde označují toohello hostování vyhrazené prostředky toorunning zatížení. Pořízení "Webová aplikace" jako příklad, budete pravděpodobně zvykli toothinking webové aplikace jako hello výpočetní prostředky a aplikace code prohlížeč tooa funkce tohoto společně doručit. Ale ve službě App Service *webové aplikace* je hello výpočetní prostředky, které Azure poskytuje k hostování kódu aplikace. 

Vaše aplikace se může skládat z více aplikací App Service různého druhu. Například pokud se vaše aplikace skládá z webového front-endu a back-endu RESTful API, můžete:

- Nasazení (front-endu i rozhraní api) tooa jedné webové aplikace  
- Nasazení webové aplikace tooa kód front-endu a back-end kód aplikace tooan rozhraní API. 



## <a name="app-service-plans"></a>Plány služby App Service
[Plánů služby App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) představují kolekci hello toohost fyzické prostředky, které používá vaše aplikace.

Plány služby App Service definují:

- **Oblast** (Západní USA, Východní USA atd.)
- **Počet škálování** (jedna, dvě, tři instance atd.)
- **Velikost instance** (Malá, Střední, Velká)
- **SKU** (Free, Shared, Basic, Standard, Premium)

Všechny aplikace přiřazené tooan **plán služby App Service** sdílet prostředky hello definovaná tímto povolení toosave nákladů při hostování více aplikací.

Vaše **plán služby App Service** možné škálovat od **volné** a **sdílené** SKU příliš**základní**, **standardní**, a **Premium** SKU, která poskytuje přístup k prostředkům toomore a funkcí společně hello způsobem. Jakmile je plán aplikační služby je nastaven příliš**základní** nebo vyšší můžete také ovládat hello **velikost** a škálovat počet hello virtuálních počítačů.

Hello **SKU** a **škálování** z hello služby App Service určuje plán hello náklady a není hello počet aplikací, které jsou v ní umístěné. 

Pokud potřebujete větší škálovatelnost a izolaci sítě, můžete aplikace spustit ve službě [App Service Environment](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Ceny
Informace o cenách služby App Service najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Vyzkoušejte si App Service
[Vytvořte si ukázkovou webovou aplikaci, mobilní aplikaci nebo aplikaci logiky](https://azure.microsoft.com/try/app-service/), hodinu si s ní hrajte, a to bez platební karty, bez závazků a bez starostí.

Nebo si můžete otevřít [bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/) a vyzkoušet některý z našich úvodních kurzů:

* [Kurz: Vytvoření webové aplikace](../app-service-web/app-service-web-get-started.md)
* [Kurz: Vytvoření mobilní aplikace](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Kurz: Vytvoření aplikace API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Kurz: Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)

