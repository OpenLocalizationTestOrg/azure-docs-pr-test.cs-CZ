---
title: "Prostředky dokumentace Azure WebJobs"
description: "Doporučené prostředky pro naučit se používat Azure WebJobs a Azure WebJobs SDK."
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
ms.openlocfilehash: 05062d7396bdbb3e589d2ab5f0443d1dca54342a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-webjobs-documentation-resources"></a>Prostředky dokumentace Azure WebJobs
## <a name="overview"></a>Přehled
Toto téma obsahuje odkazy na zdroje informací k dokumentaci o tom, jak používat Azure WebJobs a Azure WebJobs SDK. Azure WebJobs poskytují snadný způsob, jak spustit skripty nebo programy jako procesy na pozadí v kontextu [webové aplikace App Service, aplikace API nebo mobilní aplikace](../app-service/app-service-value-prop-what-is.md). Můžete nahrát a spustit spustitelný soubor, například jako cmd, bat, exe (.NET), ps1, TV, php, py, Node.js a jar. Tyto programy spustit jako webové úlohy podle plánu (cron) nebo nepřetržitě.

Účelem [WebJobs SDK](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk) je můžete zjednodušit kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů. Sada SDK webové úlohy obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře. Kromě toho je určený pro rozšiřitelnost a dojde [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure Functions](../azure-functions/functions-overview.md) (aktuálně ve verzi preview) je založená na verzi sady SDK webové úlohy, která funguje s C# skript, Node.js a další jazyky. 

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Vytváření, nasazování a správě webové úlohy je bezproblémové s integrované nástrojů v sadě Visual Studio. Můžete vytvářet webové úlohy ze šablon, publikovat a spravovat (spuštění, zastavení, monitorovat a ladit) je. 

Na řídicím panelu WebJobs na portálu Azure poskytuje výkonné funkce, které poskytují plnou kontrolu nad spuštění webové úlohy, včetně možnosti vyvolání jednotlivých funkcí v rámci webové úlohy. Na řídicím panelu zobrazí také funkce moduly runtime a výstup protokolování. 

## <a name="getstarted"></a>Začínáme s webové úlohy a WebJobs SDK
* [Úvod do Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs jsou Super a měli byste začít používat, je nyní!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Příspěvek na blogu podle Tróje Hunt.)
* [Funkce Azure WebJobs](https://azure.microsoft.com/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Co je sada WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Pozadí úlohy pokyny podle Microsoft Patterns and Practices](https://docs.microsoft.com/azure/architecture/best-practices/background-jobs)
* [Uvedení 1.1.0 RTM sady Microsoft Azure WebJobs SDK](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Začínáme s Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Použití Azure Queue Storage se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Použití Azure Blob Storage se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Použití Azure Table Storage se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Použití Azure Service Bus se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Azure WebJobs SDK Stručná referenční příručka (PDF ke stažení)](https://go.microsoft.com/fwlink/p/?linkid=845558)
* [Webové nastavení dokumentaci na webu GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videa
  * [Webové úlohy a WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
  * [Azure série videí webové úlohy na webu Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
  * [Představení WebJobs nástrojů pro Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [Webové úlohy nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
  * [Azure WebJobs aktualizaci s Pranav Rastogi – rozšiřitelnost verze 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Další informace naleznete v následujících částech [nasazení WebJobs](#deploy) a [testování a ladění webové úlohy](#debug).

## <a name="deploy"></a>Nasazení webové úlohy
* [Jak nasadit Azure WebJobs pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Postup nasazení webové úlohy pomocí portálu Azure](web-sites-create-web-jobs.md)
* [Povolení příkazového řádku nebo průběžné doručování Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Git nasazení do Azure pomocí WebJobs konzolové aplikace .NET](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Nasazení do Azure Webjobs F #](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Nasazení vlastních služeb jako Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videa
  * [Představení WebJobs nástrojů pro Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [Webové úlohy nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="schedule"></a>Plánování webové úlohy
* [Azure webové úlohy dialogové okno Přidání](websites-dotnet-deploy-webjobs.md#configure)
* [Vytvoření plánované webové úlohy na portálu Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Zapojování k webová úloha plánovače](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Plánování Azure WebJobs s výrazy procesu cron](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Plánování jednotlivých funkcí webové úlohy pomocí TimerTrigger sady SDK webové úlohy](websites-dotnet-webjobs-sdk.md#schedule)

## <a name="debug"></a>Testování a ladění webové úlohy
* [Nové vývojáře a ladění funkcí pro Azure WebJobs v sadě Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Zobrazte na řídicím panelu WebJobs](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Postup zápisu protokolů pomocí WebJobs SDK a zobrazit v řídicím panelu](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Vzdálené ladění webové úlohy](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Kdo napsali tomuto objektu blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Hostování interaktivní kódu v cloudu](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Přidání trasování do Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Monitorování, Diagnostika a řešení potíží s Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md)
* Videa
  * [Webové úlohy nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="scale"></a>Škálování webové úlohy
* [Škálování webové aplikace s weby Azure](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Aplikační služba Azure: Architektury masivním měřítku připravené pro firemní webové aplikace](https://channel9.msdn.com/Events/Build/2014/3-626). Zahrnuje škálování webové aplikace s webové úlohy, včetně WebJobs SDK.
* Videa
  * [Škálování webové úlohy](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

## <a name="additional"></a>Další zdroje informací pro webové úlohy
* [Azure WebJobs GA příspěvku na blogu podle Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Prostředí Powershell webové úlohy běžící na Azure App Service](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Získávání upozorněni, když vaše Azure aktivována webové úlohy dokončení](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Jednoduché zásady uchovávání informací zálohování webové aplikace s webové úlohy](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Webové aplikace Azure a cloudových služeb zpomalit na první požadavek](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Ukazuje, jak webové úlohy použijte k simulaci funkci AlwaysOn, která je dostupná jenom pro standardní cenovou úroveň.
* [Řádné vypnutí WebJobs](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Řádné vypnutí WebJobs SDK, najdete v části [řádné vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatizace zálohy Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videa
  * [Azure WebJobs videa pomocí Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
  * [Azure série videí webové úlohy na webu Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="additionalsdk"></a>Další zdroje informací WebJobs SDK
* [Poznámky k verzi sady SDK webové úlohy](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Sada WebJobs SDK zdrojového kódu](https://github.com/Azure/azure-webjobs-sdk)
* [Rozšíření sady SDK webové služby zdrojový kód](https://github.com/Azure/azure-webjobs-sdk-extensions), s [podrobné příručce k model rozšiřitelnosti](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Zdrojový kód skriptu WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-script/) (používá pro [Azure Functions](../azure-functions/functions-overview.md))
* [Úlohy WebJob k nahrání souborů FREB do úložiště Azure pomocí WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Hostování Azure webjobs mimo Azure, výhod protokolování z Azure hostované webové úlohy](http://bypassion.dk/?p=510)
* [Vytváření nástroj pro Import dat pomocí Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Přehled Azure Functions](../azure-functions/functions-overview.md)
* Videa
  * [Azure série videí webové úlohy na webu Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="samples"></a>Ukázkové aplikace, webové úlohy
* [Ukázkové aplikace poskytovaných týmem webové úlohy na Githubu](https://github.com/azure/azure-webjobs-sdk-samples)
* [Jednoduché webové aplikace Azure pomocí WebJobs back-end pomocí WebJobs SDK](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Znázorňuje použití plánované a událostmi řízené webové úlohy. Naleznete v příspěvku blogu [znovu sestavit SiteMonitR pomocí Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

## <a name="blogs"></a>Blogy
* [Blog o Azure](/blog)
* [Blog Amitu Apple](http://blog.amitapple.com/). Zaměřuje se na webové úlohy (ne sadu SDK).
* [Blog Magnus Mårtensson](http://magnusmartensson.com/)

## <a name="gethelp"></a>Získání nápovědy k webové úlohy
* [StackOverflow pro webové úlohy](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow pro WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow pro Azure Functions](http://stackoverflow.com/questions/tagged/azure-functions)
* [Fórum pro Azure a ASP.NET](http://forums.asp.net/1247.aspx)
* [Fórum služby Azure App Service Web Apps](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps User Voice lokality](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Použijte hashtag #AzureWebJobs.
* [Sestavy chyb webové úlohy nebo problém](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

