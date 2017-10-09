---
title: "zdroje informací k dokumentaci aaaAzure webové úlohy"
description: "Doporučené zdroje informací jak toouse Azure WebJobs a hello Azure WebJobs SDK."
services: app-service
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: ed005e56-4334-4641-a5e5-15435c2be36b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2017
ms.author: glenga
ms.openlocfilehash: 6616a9d97c9637ec64cb8743dded6ba409a521ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-webjobs-documentation-resources"></a>Prostředky dokumentace Azure WebJobs
## <a name="overview"></a>Přehled
Toto téma odkazy toodocumentation prostředky o toouse Azure WebJobs a hello Azure WebJobs SDK. Azure WebJobs zadejte snadný způsob toorun skriptů nebo programů jako pozadí procesy v kontextu hello [webové aplikace App Service, aplikace API nebo mobilní aplikace](../app-service/app-service-value-prop-what-is.md). Můžete nahrát a spustit spustitelný soubor, například jako cmd, bat, exe (.NET), ps1, TV, php, py, Node.js a jar. Tyto programy spustit jako webové úlohy podle plánu (cron) nebo nepřetržitě.

Hello účel hello [WebJobs SDK](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk) je toosimplify hello kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů. Hello WebJobs SDK obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře. Kromě toho má určený toobe rozšiřitelný a je [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure Functions](../azure-functions/functions-overview.md) (aktuálně ve verzi preview) je založená na verzi hello WebJobs SDK, který funguje s C# skript, Node.js a další jazyky. 

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Vytváření, nasazování a správě webové úlohy je bezproblémové s integrované nástrojů v sadě Visual Studio. Můžete vytvářet webové úlohy ze šablon, publikovat a spravovat (spuštění, zastavení, monitorovat a ladit) je. 

řídicím panelu WebJobs Hello v hello portál Azure poskytuje výkonné funkce, které poskytují plnou kontrolu nad hello spuštění webové úlohy, včetně hello možnost tooinvoke jednotlivých funkcí v rámci webové úlohy. řídicí panel Hello také zobrazuje funkce moduly runtime a výstup protokolování. 

## <a name="getstarted"></a>Začínáme s webové úlohy a hello WebJobs SDK
* [Úvod tooAzure webové úlohy](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs jsou Super a měli byste začít používat, je nyní!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Příspěvek na blogu podle Tróje Hunt.)
* [Funkce Azure WebJobs](https://azure.microsoft.com/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Co je hello WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Pozadí úlohy pokyny podle Microsoft Patterns and Practices](https://docs.microsoft.com/azure/architecture/best-practices/background-jobs)
* [Uvedení hello 1.1.0 RTM z Microsoft Azure WebJobs SDK](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Začínáme s hello Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Jak toouse Azure blob storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Jak toouse Azure table storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Jak toouse služba Azure Service Bus s hello WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Azure WebJobs SDK Stručná referenční příručka (PDF ke stažení)](https://go.microsoft.com/fwlink/p/?linkid=845558)
* [Webové nastavení dokumentaci na webu GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videa
  * [Webové úlohy a hello WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
  * [Azure série videí webové úlohy na webu Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
  * [Představení WebJobs nástrojů pro Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [Webové úlohy nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
  * [Azure WebJobs aktualizaci s Pranav Rastogi – rozšiřitelnost verze 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Viz také následující oddíly hello [nasazení WebJobs](#deploy) a [testování a ladění webové úlohy](#debug).

## <a name="deploy"></a>Nasazení webové úlohy
* [Jak tooDeploy webové úlohy Azure pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Jak pomocí WebJobs toodeploy hello portálu Azure](web-sites-create-web-jobs.md)
* [Povolení příkazového řádku nebo průběžné doručování Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Git nasazení tooAzure konzoly aplikace .NET, pomocí webové úlohy](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Nasazení tooAzure F # webovou úlohu](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Nasazení vlastních služeb jako Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videa
  * [Představení WebJobs nástrojů pro Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [Webové úlohy nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="schedule"></a>Plánování webové úlohy
* [Hello přidat Azure webové úlohy dialogové okno](websites-dotnet-deploy-webjobs.md#configure)
* [Vytvoření plánované webové úlohy v hello portálu Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Zapojování plánovače úloh tooa webové úlohy](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Plánování Azure WebJobs s výrazy procesu cron](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Plánování jednotlivých funkcí webové úlohy pomocí hello WebJobs SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

## <a name="debug"></a>Testování a ladění webové úlohy
* [Nové vývojáře a ladění funkcí pro Azure WebJobs v sadě Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Zobrazení hello řídicím panelu WebJobs](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Jak toowrite protokolů pomocí hello WebJobs SDK a zobrazit v hello řídicí panel](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Vzdálené ladění webové úlohy](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Kdo napsali tomuto objektu blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Hostování interaktivní kód v hello cloudu](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Přidání trasování tooAzure webové úlohy](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Monitorování, Diagnostika a řešení potíží s Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md)
* Videa
  * [Webové úlohy nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="scale"></a>Škálování webové úlohy
* [Škálování webové aplikace s weby Azure](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Aplikační služba Azure: Architektury masivním měřítku připravené pro firemní webové aplikace](https://channel9.msdn.com/Events/Build/2014/3-626). Zahrnuje škálování webové aplikace s webové úlohy, včetně hello WebJobs SDK.
* Videa
  * [Škálování webové úlohy](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

## <a name="additional"></a>Další zdroje informací pro webové úlohy
* [Azure WebJobs GA příspěvku na blogu podle Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Prostředí Powershell webové úlohy běžící na Azure App Service](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Získávání upozorněni, když vaše Azure aktivována webové úlohy dokončení](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Jednoduché zásady uchovávání informací zálohování webové aplikace s webové úlohy](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Webové aplikace Azure a cloudových služeb zpomalit na první požadavek](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Ukazuje, jak toouse WebJobs toosimulate hello funkci AlwaysOn, která je dostupná jenom pro hello standardní cenovou úroveň.
* [Řádné vypnutí WebJobs](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Řádné vypnutí WebJobs SDK, najdete v části [řádné vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatizace zálohy Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videa
  * [Azure WebJobs videa pomocí Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
  * [Azure série videí webové úlohy na webu Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="additionalsdk"></a>Další zdroje informací WebJobs SDK
* [Poznámky k verzi sady SDK webové úlohy](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Sada WebJobs SDK zdrojového kódu](https://github.com/Azure/azure-webjobs-sdk)
* [Rozšíření sady SDK webové služby zdrojový kód](https://github.com/Azure/azure-webjobs-sdk-extensions), s [model rozšiřitelnosti toohello podrobné příručce](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Zdrojový kód skriptu WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-script/) (používá pro [Azure Functions](../azure-functions/functions-overview.md))
* [Webová úloha tooupload FREB soubory tooAzure úložiště pomocí hello WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Hostování Azure webjobs mimo Azure, s hello protokolování výhody z Azure hostované webové úlohy](http://bypassion.dk/?p=510)
* [Vytváření nástroj pro Import dat pomocí Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Přehled Azure Functions](../azure-functions/functions-overview.md)
* Videa
  * [Azure série videí webové úlohy na webu Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="samples"></a>Ukázkové aplikace, webové úlohy
* [Ukázkové aplikace poskytované hello team webové úlohy na Githubu](https://github.com/azure/azure-webjobs-sdk-samples)
* [Jednoduché webové aplikace Azure pomocí WebJobs back-end pomocí hello WebJobs SDK](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Znázorňuje použití plánované a událostmi řízené webové úlohy. Najdete v příspěvku blogu hello [Rebuilding hello SiteMonitR pomocí Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

## <a name="blogs"></a>Blogy
* [Blog o Azure](/blog)
* [Blog Amitu Apple](http://blog.amitapple.com/). Zaměřuje se na webové úlohy (ne hello SDK).
* [Blog Magnus Mårtensson](http://magnusmartensson.com/)

## <a name="gethelp"></a>Získání nápovědy k webové úlohy
* [StackOverflow pro webové úlohy](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow pro hello WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow pro Azure Functions](http://stackoverflow.com/questions/tagged/azure-functions)
* [Fórum pro Azure a ASP.NET](http://forums.asp.net/1247.aspx)
* [Fórum služby Azure App Service Web Apps](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps User Voice lokality](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Použijte hello hashtag #AzureWebJobs.
* [Sestavy chyb webové úlohy nebo problém](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

