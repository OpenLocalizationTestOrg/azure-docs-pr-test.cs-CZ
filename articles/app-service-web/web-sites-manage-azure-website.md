---
title: "aaaManage webové aplikace ve službě Azure App Service"
description: "Odkazy tooresources pro správu webové aplikace ve službě Azure App Service."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.openlocfilehash: daf69245e66068b0e97e3ae1c3fb5fce45605b91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a>Správa webové aplikace ve službě Azure App Service
Toto téma obsahuje odkazy tooresources pro správu webové aplikace ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Správa zahrnuje všechny hello úlohy, které udržují vaší webové aplikace plynulý chod. 

Průběhu životnosti hello webové aplikace budete provádět úlohy správy různých, jako jste odpojili od počátečního nasazení toonormal operace, Údržba a aktualizace.

Mnoho úloh správy webové aplikace lze provést v hello portálu Azure.

## <a name="before-you-deploy-your-web-app-tooproduction"></a>Před nasazením tooproduction vaší webové aplikace
### <a name="choose-a-tier"></a>Zvolte úroveň
Aplikační služba Azure je k dispozici v pěti vrstev: Free, sdílené, Basic, Standard a Premium. Informace o funkcích hello a ceny pro každou vrstvu najdete v tématu [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/app-service/). 

* [Plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) umožňují seskupit více webových aplikací v rámci hello stejné vrstvě.
* Vždy můžete [přepínač vrstvy](web-sites-scale.md) po vytvoření webové aplikace.

### <a name="configuration"></a>Konfigurace
Použití hello [portálu Azure](https://portal.azure.com/) tooset možnosti konfigurace. Podrobnosti najdete v tématu [konfigurace webových aplikací ve službě Azure App Service](web-sites-configure.md). Tady je rychlé kontrolní seznam:

* Vyberte **runtime verze** pro rozhraní .NET, PHP, Java nebo Python, v případě potřeby.
* Povolit **Websocket** Pokud vaše webová aplikace používá protokol WebSocket hello. (To zahrnuje aplikace, které používají [ASP.NET SignalR](http://www.asp.net/signalr) nebo [socket.io](web-sites-nodejs-chat-app-socketio.md).)
* Běží nepřetržité webové úlohy? Pokud ano, povolit **Always On**.
* Sada hello **výchozí dokument**, jako je například index.html.

V přidání toothese základní nastavení konfigurace může být vhodné tooconfigure hello následující:

* **Secure Socket Layer (SSL)** šifrování. toouse SSL s vlastní název domény, musíte získat protokolem SSL certifikátů a konfigurace vaší webové aplikace toouse ho. V tématu [povolit HTTPS pro webové aplikace ve službě Azure App Service](app-service-web-tutorial-custom-ssl.md).
* **Vlastní název domény.** Webové aplikace automaticky má subdoména pod azurewebsites.net. Můžete přidružit vlastní název domény, například contoso.com. V tématu [konfigurace vlastního názvu domény v Azure App Service](app-service-web-tutorial-custom-domain.md).

Konfigurace pro specifický jazyk:

* **PHP**: [nakonfigurovat PHP ve službě Azure App Service Web Apps](web-sites-php-configure.md).
* **Python**: [konfigurace Python službou Azure App Service webové aplikace](web-sites-python-configure.md)

## <a name="while-your-web-app-is-running"></a>Když běží vaše webová aplikace
Když běží vaše webová aplikace, budete chtít toomake, že je k dispozici, a že se škáluje provozu generovaného uživateli toomeet. Také můžete potřebovat tootroubleshoot chyby.

### <a name="monitoring"></a>Monitorování
* Prostřednictvím hello portálu Azure, můžete [přidat metriky výkonu](web-sites-monitor.md) například využití procesoru a počet klientských požadavků.
* [Škálování webové aplikace](web-sites-scale.md) v tootraffic odpovědi. V závislosti na úrovni je možné škálovat hello počet virtuálních počítačů nebo velikost hello hello instance virtuálních počítačů. V hello Standard a Premium vrstvy můžete také nastavíte automatické škálování, tak škáluje vaší webové aplikace automaticky podle pevného plánu nebo v tooload odpovědi.  

### <a name="backups"></a>Zálohování
* Nastavit [automatické zálohování](web-sites-backup.md) vaší webové aplikace. Další informace o zálohování v [toto video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
* Další informace o možnosti hello [obnovení databáze](../sql-database/sql-database-business-continuity.md) ve službě Azure SQL Database.

### <a name="troubleshooting"></a>Řešení potíží
* Pokud dojde k chybě, můžete [řešení v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), pomocí protokolů diagnostiky a živé ladění v cloudu hello. 
* Mimo Visual Studio existují různé způsoby toocollect diagnostické protokoly. V tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](web-sites-enable-diagnostic-log.md).
* Aplikace Node.js, naleznete v [jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Obnovení dat
* [Obnovit](web-sites-restore.md) webovou aplikaci, která byla dříve zálohovaná.

## <a name="when-you-update-your-web-app"></a>Při aktualizaci webové aplikace
Pokud jste nepovolili automatické zálohování, můžete vytvořit [ruční zálohy](web-sites-backup.md).

Zvažte použití [připravený nasazení](web-sites-staged-publishing.md). Tato možnost umožňuje publikovat aktualizace tooa pracovní nasazení, které běží souběžně sdílená s produkčním nasazení. 


<!-- Anchors. -->

[Before you deploy your site tooproduction]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


