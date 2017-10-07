---
title: "výkon aaaApplication nejčastější dotazy pro webové aplikace Azure | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se dostupnosti, výkonu a problémy s aplikací v hello funkce Web Apps služby Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 7f2383743079e4c630fd548b0efd9993029afe11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Výkon aplikace nejčastější dotazy pro webové aplikace v Azure

Tento článek obsahuje odpovědi toofrequently dotazy (FAQ) o problémy s výkonem aplikace pro hello [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Proč je můj aplikace pomalé?

Několik faktorů může přispět tooslow výkonu aplikace. Podrobné kroky řešení potíží naleznete v tématu [Poradce při potížích pomalé webové aplikace výkonu](app-service-web-troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Jak odstranit scénáři vysoké spotřeby procesoru

V některých scénářích vysoké spotřeby procesoru skutečně vaše aplikace může vyžadovat další výpočetní prostředky. V takovém případě zvažte škálování tooa vyšší úroveň služby, takže aplikace hello získá všechny hello prostředky, které potřebuje. Jinou dobu vysoké spotřeby procesoru může být způsobeno smyčku chybný nebo kódování postupem. Získávání aspekty co je aktivován vyšší spotřeby procesoru je proces dvě části. Nejprve vytvořit výpis procesu a poté analyzovat výpis procesu hello. Další informace najdete v tématu [zachycení a analyzovat soubor výpisu pro vysoké spotřeby procesoru pro webové aplikace](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Jak odstranit scénář vysoké spotřeby paměti?

V některých scénářích vysoké spotřeby paměti skutečně vaše aplikace může vyžadovat další výpočetní prostředky. V takovém případě zvažte škálování tooa vyšší úroveň služby, takže aplikace hello získá všechny hello prostředky, které potřebuje. Jinou dobu nevrácená paměť systému může způsobit chyby v kódu hello. Kódování postup také může zvýšit využití paměti. Získávání aspekty co je aktivován velkého množství paměti, že spotřeba je proces dvě části. Nejprve vytvořit výpis procesu a poté analyzovat výpis procesu hello. Služby Diagnostika havárií z hello Galerie rozšíření lokality Azure můžete efektivně provést oba tyto kroky. Další informace najdete v tématu [zachycení a analyzovat soubor výpisu pro přerušované vysokého využití paměti pro webové aplikace](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>Jak automatizovat webové aplikace služby App Service pomocí prostředí PowerShell?

Můžete použít toomanage rutiny prostředí PowerShell a udržovat webové aplikace aplikační služby. V našem blogu [automatizovat webové aplikace hostované v Azure App Service pomocí prostředí PowerShell](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), jsme popisují, jak toouse založené na správci prostředků Azure PowerShell rutiny tooautomate běžné úlohy. příspěvek blogu Hello má také ukázkový kód pro různé úlohy správy webové aplikace. Popisy a syntaxe pro všechny rutiny webové aplikace služby App Service najdete v tématu [AzureRM.Websites](https://docs.microsoft.com/powershell/module/azurerm.websites/?view=azurermps-4.0.0).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>Jak zobrazit protokoly událostí Moje webová aplikace?

tooview v protokolech událostí vaší webové aplikace:

1. Přihlaste se tooyour [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).
2. V nabídce hello vyberte **ladění konzoly** > **CMD**.
3. Vyberte hello **LogFiles** složky.
4. protokoly událostí tooview, vyberte ikonu tužky hello vedle příliš**eventlog.xml**.
5. protokoly hello toodownload, spusťte rutinu prostředí PowerShell hello `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Jak zaznamenat výpisu paměti uživatelského režimu Moje webové aplikace?

toocapture výpisu paměti uživatelského režimu vaší webové aplikace:

1. Přihlaste se tooyour [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).
2. Vyberte hello **Průzkumníka procesů** nabídky.
3. Klikněte pravým tlačítkem na hello **w3wp.exe** procesu nebo procesu webové úlohy.
4. Vyberte **stáhnout výpis stavu paměti** > **úplný výpis**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Jak zobrazit informace o úrovni procesu pro webová aplikace?

Máte dvě možnosti pro zobrazení informací na úrovni procesu pro webové aplikace:

*   V portálu Azure hello:
    1. Otevřete hello **Průzkumníka procesů** pro hello webovou aplikaci.
    2. toosee hello podrobnosti, vyberte hello **w3wp.exe** procesu.
*   V konzole Kudu hello:
    1. Přihlaste se tooyour [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Vyberte hello **Průzkumníka procesů** nabídky.
    3. Pro hello **w3wp.exe** procesu, vyberte **vlastnosti**.

## <a name="when-i-browse-toomy-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>Při procházení toomy aplikace, I najdete v části "Chyba 403 - této webové aplikace je zastavena." Jak to lze vyřešit?

Tato chyba může způsobit tři podmínky:

* Hello webovou aplikaci byl dosažen limit fakturace a váš web byl zakázán.
* Hello webové aplikace byla zastavena hello portálu.
* webové aplikace Hello byl dosažen limit kvóty prostředku, který může být použito tooa volné nebo sdílený plán služby škálování.

toosee příčinu chyby hello a tooresolve hello problém, hello postupujte podle kroků v [webové aplikace: "Chyba 403 – tato webová aplikace je zastavena"](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Kde můžete další informace o kvóty a omezení pro různé plány služby App Service?

Informace o kvóty a omezení najdete v tématu [omezení služby App Service](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-hello-response-time-for-hello-first-request-after-idle-time"></a>Jak snížit hello doba odezvy pro první požadavek hello po dobu nečinnosti?

Ve výchozím nastavení jsou uvolněna webové aplikace, pokud jsou na nastavenou dobu nečinnosti. Tímto způsobem hello systému můžete ušetřit prostředky. Nevýhodou Hello je hello odpovědi toohello první žádost po hello webová aplikace je odpojen, je delší, tooallow hello tooload webové aplikace a spusťte současné obsluhování odpovědí. V plánech úrovně Basic a Standard service, můžete zapnout hello **Always On** aplikace hello tookeep nastavení vždy načíst. Tím se eliminuje aplikace hello po nečinnosti delší doby zatížení. toochange hello **Always On** nastavení:

1. V hello portálu Azure přejděte tooyour webové aplikace.
2. Vyberte **nastavení aplikace**.
3. Pro **Always On**, vyberte **na**.

## <a name="how-do-i-turned-on-failed-request-tracing"></a>Jak po zapnutí trasování chybných požadavků?

tooturn na trasování chybných požadavků:

1. V hello portálu Azure přejděte tooyour webové aplikace.
3. Vyberte **všechna nastavení** > **protokolů diagnostiky**.
4. Pro **trasování chybných požadavků**, vyberte **na**.
5. Vyberte **Uložit**.
6. V okně webové aplikace hello, vyberte **nástroje**.
7. Vyberte **sadou Visual Studio**.
8. Pokud není nastavení hello **na**, vyberte **na**.
9. Vyberte **přejděte**.
10. Vyberte **Web.config**.
11. Oddíl system.webServer přidejte tuto konfiguraci (toocapture konkrétní adresy URL):

    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*api*" />
    <add path="*api*">
    <traceAreas>
    <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
12. problémy s nízkou rychlostí tootroubleshoot, přidejte tuto konfiguraci (Pokud hello zaznamenání požadavku trvá déle než 30 sekund):
    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*" />
    <add path="*">
    <traceAreas> <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions timeTaken="00:00:30" statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
13. toodownload hello se nezdařilo trasování požadavků v hello [portál](https://portal.azure.com), přejděte tooyour webu.
15. Vyberte **nástroje** > **Kudu** > **přejděte**.
18. V nabídce hello vyberte **ladění konzoly** > **CMD**.
19. Vyberte hello **LogFiles** složku a pak vyberte hello složku s názvem, který začíná **W3SVC**.
20. toosee hello soubor XML, vyberte hello ikonu tužky.

## <a name="i-see-hello-message-worker-process-requested-recycle-due-toopercent-memory-limit-how-do-i-address-this-issue"></a>Zpráva, hello "pracovní proces požadovaný recyklaci kvůli too'Percent paměti se limit." Jak tento problém vyřešit?

Hello maximální dostupné množství paměti pro proces 32-bit (i na 64bitový operační systém) je 2 GB. Ve výchozím nastavení je pracovní proces hello nastaven too32 bitů ve službě App Service (pro kompatibilitu s starší verze webové aplikace).

Umožňuje využívat hello dostupnou paměť navíc ve své role pracovního procesu webové zvažte procesy too64 bitů. Tím se spustí restartování webové aplikace, takže naplánovat odpovídajícím způsobem.

Všimněte si také, že prostředí 64-bit vyžaduje plán služeb Basic nebo Standard. Uvolněte a sdílené plány vždy spustit v prostředí 32-bit.

Další informace najdete v tématu [konfigurace webové aplikace v App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-configure).

## <a name="why-does-my-request-time-out-after-240-seconds"></a>Proč můj časový limit požadavku po 240 sekundách?

Azure nástroj pro vyrovnávání zatížení má výchozí nastavení časového limitu nečinnosti čtyři minut. Obvykle se jedná o přiměřené odpovědi časový limit pro webové žádosti. Pokud vaše webová aplikace vyžaduje zpracování na pozadí, doporučujeme používat Azure WebJobs. Hello webové aplikace Azure můžete volat webové úlohy a upozorněni, až po dokončení zpracování na pozadí. Můžete se z několika metod pro použití webové úlohy, včetně fronty a aktivační události.

Webové úlohy je navržen pro zpracování na pozadí. Můžete to udělat tolik zpracování na pozadí tak, jak chcete v webovou úlohu. Další informace o Webjobech najdete v tématu [spustit úlohy na pozadí s WebJobs](https://docs.microsoft.com/azure/app-service-web/web-sites-create-web-jobs).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>Aplikace ASP.NET Core, které jsou hostované ve službě App Service někdy přestanou reagovat. Jak tento problém vyřešit?

O známý problém s předchozím kroku [Kestrel verze](https://github.com/aspnet/KestrelHttpServer/issues/1182) může způsobit 1.0 ASP.NET základní aplikaci, která je hostovaná v App Service toointermittently přestane reagovat. Také může zobrazit tato zpráva: "hello zadán, aplikace CGI zachytila chybu a hello proces hello serveru byla ukončena."

Tento problém vyřešen v Kestrel verzi 1.0.2. Tato verze je součástí hello ASP.NET Core 1.0.3 aktualizace. tooresolve-li tento problém, ujistěte se, aktualizovat vaše aplikace závislosti toouse Kestrel 1.0.2. Alternativně můžete použít jednu z dva alternativní postupy, které jsou popsané v hello příspěvku na blogu [pomalé výkonu ASP.NET Core 1.0 problémy ve službě App Service web apps](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-hello-file-structure-of-my-web-app-how-can-i-find-them"></a>Nelze najít Moje soubory protokolu ve struktuře souborů hello webová aplikace. Jak je můžete je vyhledat?

Pokud používáte funkci hello místní mezipaměti služby App Service, se vztahuje hello hello LogFiles struktura složek a složek s daty pro vaše instance služby App Service. Pokud se používá místní mezipaměti, jsou podsložky vytvořené v hello úložiště LogFiles a složek s daty. použití podsložky Hello hello pojmenování vzor "Jedinečný identifikátor" + časové razítko. Každé podsložky odpovídá tooa instance virtuálního počítače, ve které hello webová aplikace běží, nebo byla spuštěna.

toodetermine ať používáte místní mezipaměť, zkontrolovat aplikační služby **nastavení aplikace** kartě. Pokud se používá místní mezipaměti, hello nastavení aplikace `WEBSITE_LOCAL_CACHE_OPTION` je nastaven příliš`Always`. Další informace o místní mezipaměti najdete v tématu hello [místní mezipaměti služby aplikace přehled](https://docs.microsoft.com/azure/app-service/app-service-local-cache).

Pokud nepoužíváte místní mezipaměti a se jedná o problém, odešlete žádost o podporu.

## <a name="i-see-hello-message-an-attempt-was-made-tooaccess-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>Zpráva, hello "pokus byla provedena tooaccess soketu způsobem zakázáno podle jeho oprávnění k přístupu." Jak to lze vyřešit?

Této chybě obvykle dochází, pokud jsou vyčerpány hello odchozí připojení TCP pro instanci virtuálního počítače hello. Ve službě App Service jsou vynuceny hello maximální počet odchozí připojení, které můžete provést pro každou instanci virtuálního počítače. Další informace najdete v tématu [Cross-VM numerické limity](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Této chybě může také dojít, pokud se pokusíte tooaccess místní adresu z vaší aplikace. Další informace najdete v tématu [místní adresu požadavky](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Další informace o odchozí připojení ve vaší webové aplikaci najdete v tématu hello blogu o [odchozí připojení tooAzure weby](http://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-tooremote-debug-my-app-service-web-app"></a>Použití sady Visual Studio tooremote ladění webová aplikace služby App Service?

Podrobný návod, který ukazuje, jak toodebug vaší webové aplikace pomocí sady Visual Studio, najdete v části [vzdáleného ladění webové aplikace služby App Service](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
