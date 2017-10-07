---
title: "aaaTroubleshooting žádná data - Application Insights pro rozhraní .NET"
description: "Není zobrazuje data ve službě Azure Application Insights? Zkuste sem."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 261c25c89884c46e41bbc305474ac854360221ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Řešení potíží s chybějícími daty v nástroji Application Insights pro .NET
## <a name="some-of-my-telemetry-is-missing"></a>Chybí některé Moje telemetrie
*Ve službě Application Insights zobrazena pouze zlomek hello události, které generují Moje aplikace.*

* Pokud se zobrazuje konzistentně hello stejný zlomek, pravděpodobně kvůli tooadaptive [vzorkování](app-insights-sampling.md). tooconfirm, otevřete vyhledávání (v okně Přehled hello) a podívejte se na instanci žádost nebo jiná událost. V dolní části hello hello vlastnosti oddílu klikněte na tlačítko "..." tooget úplné vlastnost podrobnosti. Pokud požadavku počet > 1, pak vzorkování je v provozu. 
* Jinak, je možné, že jste nedosáhli [limit rychlosti dat](app-insights-pricing.md#limits-summary) pro cenový plán. Tato omezení platí za minutu.

## <a name="no-data-from-my-server"></a>Žádná data ze svého serveru
*Po instalaci aplikace na webovém serveru, a teď se nezobrazí žádné telemetrie z něj. To šlo OK na můj vývojářském počítači.*

* Pravděpodobně problém brány firewall. [Nastavit výjimky brány firewall pro data toosend Application Insights](app-insights-ip-addresses.md).

*I [nainstalován monitorování stavu](app-insights-monitor-performance-live-website-now.md) na můj stávající aplikace webového serveru toomonitor. Se nezobrazí žádné výsledky.*

* V tématu [řešení potíží s monitorování stavu](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights). 

## <a name="q01"></a>Žádná možnost Přidat Application Insights v sadě Visual Studio
*Při kliknutí pravým tlačítkem myši existující projekt v Průzkumníku řešení, se nezobrazí žádné možnosti Application Insights.*

* Nástroji hello jsou podporovány všechny typy rozhraní .NET projektu. Projekty webové a WCF jsou podporovány. Pro ostatní typy projektu například stolní nebo služby aplikací, můžete stále [přidání Application Insights SDK projektu tooyour ručně](app-insights-windows-desktop.md).
* Zajistěte, aby byla [Visual Studio 2013 Update 3 nebo novější](http://go.microsoft.com/fwlink/?LinkId=397827). Pochází předinstalované Developer Analytics tools, které poskytují hello Application Insights SDK.
* Vyberte **nástroje**, **rozšíření a aktualizace** a zkontrolujte, zda **Developer Analytics Tools** je nainstalovaný a povolený. Pokud ano, klikněte na tlačítko **aktualizace** toosee, pokud je dostupná aktualizace.
* Otevřete dialogové okno Nový projekt hello a zvolte webové aplikace ASP.NET. Pokud se zobrazí hello existuje možnost Application Insights, pak jsou nainstalované nástroje hello. Pokud ne, zkuste odinstalace a opětovné instalaci nástroje Application Insights hello.

## <a name="q02"></a>Nezdařilo se přidání Application Insights
*Při tooadd Application Insights tooan existující projekt se zobrazuje chybovou zprávu.*

Pravděpodobné příčiny:

* Komunikace s portálem Application Insights hello selhalo; nebo
* Je nějaký problém s vaším účtem Azure;
* Můžete mít pouze [přístup pro čtení toohello předplatné nebo skupinu, které jste se pokoušeli toocreate hello nový prostředek](app-insights-resources-roles-access-control.md).

Oprava:

* Zkontrolujte, zda jste zadali přihlašovací údaje pro hello práva účtu Azure. 
* V prohlížeči, zkontrolujte, zda máte přístup k toohello [portál Azure](https://portal.azure.com). Otevřít nastavení a zobrazit, pokud neexistuje žádné omezení.
* [Přidat Application Insights tooyour existující projekt](app-insights-asp-net.md): V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte "Přidat Application Insights."
* Pokud stále nefunguje správně, postupujte podle hello [Ruční postup](app-insights-windows-services.md) tooadd prostředek hello portálu a poté přidejte hello SDK tooyour projektu. 

## <a name="emptykey"></a>Dojde k chybě "klíč instrumentace nemůže být prázdný"
Vypadá to, došlo k chybě při byly instalaci Application Insights nebo může být adaptér protokolování.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte **Application Insights > Konfigurovat Application Insights**. Se vám zobrazí dialogové okno se žádostí můžete toosign v tooAzure a buď vytvořte prostředek Application Insights, nebo znovu použijte stávající.

## <a name="NuGetBuild"></a>"Balíčky NuGet chybí" na serveru Moje sestavení
*Všechno, co když I mě ladění na můj vývojovém počítači, ale zobrazí chybová zpráva NuGet na serveru sestavení hello sestavení OK.*

Najdete v tématu [obnovení balíčků NuGet](http://docs.nuget.org/Consume/Package-Restore) a [automatické obnovení balíčků](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-tooopen-application-insights-from-visual-studio"></a>Chybějící tooopen příkaz nabídky Application Insights v sadě Visual Studio
*Při kliknutí pravým tlačítkem myši projektu Průzkumníku řešení, se nezobrazí žádné příkazy, Application Insights, nebo příkaz otevřete Application Insights se nezobrazí.*

Pravděpodobné příčiny:

* Pokud vytvoříte prostředek Application Insights hello ručně, nebo pokud je projekt hello typu, který není podporován nástrojem nástrojů Application Insights hello.
* v aplikaci Visual Studio jsou zakázány Hello Developer Analytics tools. 
* Visual Studio je starší než 2013 Update 3.

Oprava:

* Zajistěte, aby byl vaší verzí sady Visual Studio 2013 update 3 nebo novější.
* Vyberte **nástroje**, **rozšíření a aktualizace** a zkontrolujte, zda **Developer Analytics tools** je nainstalovaný a povolený. Pokud ano, klikněte na tlačítko **aktualizace** toosee, pokud je dostupná aktualizace.
* Klikněte pravým tlačítkem na projekt v Průzkumníku řešení. Pokud příkaz hello **Application Insights > Konfigurovat Application Insights**, použijte ji tooconnect prostředku toohello projektu v hello služby Application Insights.

Jinak typ vašeho projektu nepodporuje přímo hello nástrojů Application Insights. toosee telemetrie, přihlášení toohello [portál Azure](https://portal.azure.com), vyberte Application Insights na hello levém navigačním panelu a vyberte svou aplikaci.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>Odepření přístupu"' otevírání Application Insights v sadě Visual Studio
*Hello příkazu nabídky, otevřete Application Insights, trvá toohello portálu Azure, ale zobrazí se chyba odepření přístupu.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Hello přihlašování společnosti Microsoft, jste naposledy použili na výchozí prohlížeč nemá přístup příliš[hello prostředek, který byl vytvořen při Application Insights byla přidána aplikace toothis](app-insights-asp-net.md). Existují dvě pravděpodobné příčiny: 

* Máte více než jeden účet Microsoft - možná pracovní a osobní účet Microsoft? Hello přihlášení poslední používaného na výchozí prohlížeč bylo pro jiný účet než ten, který má přístup příliš hello[přidejte Application Insights toohello projekt](app-insights-asp-net.md). 
  
  * Oprava: Klikněte na název v top napravo od okna prohlížeče hello a odhlášení. Pak se přihlaste pomocí účtu hello, který má přístup. V levém navigačním panelu hello, pak klikněte na tlačítko Application Insights a vyberte svou aplikaci.
* Někdo jiný přidat Application Insights toohello projektu a je zapomněli toogive jste [skupině prostředků přístup toohello](app-insights-resources-roles-access-control.md) ve které byly vytvořeny. 
  
  * Oprava: Pokud použili účet organizace, že vás může přidat toohello team; nebo se můžete udělit můžete jednotlivý přístup toohello skupinu prostředků.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Prostředek nebyl nalezen" otevírání Application Insights v sadě Visual Studio
*Hello příkazu nabídky, otevřete Application Insights, trvá toohello portálu Azure, ale zobrazí chyba "prostředek nebyl nalezen".*

Pravděpodobné příčiny:

* odstraněn Hello prostředek Application Insights pro vaši aplikaci. nebo
* klíč instrumentace Hello nastavit nebo změnit v souboru ApplicationInsights.config tak, že upravíte přímo, bez aktualizace souboru projektu hello. 

klíč instrumentace Hello v ovládacích prvcích souboru ApplicationInsights.config, kam je odeslána hello telemetrie. Řádek v souboru projektu hello řídí, který prostředek se otevře při použití příkazu hello v sadě Visual Studio. 

Oprava:

* V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, konfigurovat Application Insights. V dialogovém okně hello můžete vybrat existující prostředek tooan toosend telemetrie, nebo vytvořte novou. nebo:
* Otevřete prostředek hello přímo. Přihlaste se příliš[hello portál Azure](https://portal.azure.com), klikněte na tlačítko Application Insights na hello levém navigačním panelu a pak vyberte svou aplikaci.

## <a name="where-do-i-find-my-telemetry"></a>Kde Moje telemetrie
*Podepsaná v toohello [portálu Microsoft Azure](https://portal.azure.com), a dívám se na hello Azure domovský řídicí panel. Kde lze najít data Application Insights?*

* V levém navigačním panelu hello klikněte na tlačítko Application Insights, pak název vaší aplikace. Pokud nemáte žádné projekty existuje, je třeba příliš[přidat nebo nakonfigurovat Application Insights ve webovém projektu](app-insights-asp-net.md).
  
    Zde se zobrazí některé souhrnné grafy. Můžete kliknout na jejich prostřednictvím toosee více podrobností.
* V sadě Visual Studio při ladění aplikace, klikněte na tlačítko Application Insights hello.

## <a name="q03"></a>Žádná data serveru (nebo žádná data na všechny)
*I spustili mé aplikace a pak otevřít služby Application Insights hello v Microsoft Azure, ale všechny hello zobrazit grafy ' Další informace jak toocollect...' nebo "Není nakonfigurována."* Nebo, *jenom zobrazení stránky a uživatelská data, ale žádná data serveru.*

* Spusťte svoji aplikaci v režimu ladění v sadě Visual Studio (F5). Pomocí aplikace hello tak jako toogenerate nějaké telemetrie. Zkontrolujte, zda se zobrazí události zaznamenané v okně výstupu sady Visual Studio hello. 
  
    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)
* Hello portálu Application Insights, otevřete [diagnostické vyhledávání](app-insights-diagnostic-search.md). Data obvykle se zde zobrazí první.
* Klikněte na tlačítko Aktualizovat hello. pravidelně Hello okno sama aktualizuje, ale můžete také provést ručně. interval obnovování Hello je delší, pro větší časových rozsahů.
* Zkontrolujte, zda hello instrumentace klíče shodují. V hlavním okně hello aplikace portálu služby Application Insights hello v hello **Essentials** rozevíracího seznamu, podívejte se na **klíč instrumentace**. Poté ve vašem projektu v sadě Visual Studio otevřete soubor ApplicationInsights.config a najde hello `<instrumentationkey>`. Zkontrolujte, zda text hello dva klíče jsou stejné. Pokud není:
  
  * Hello portálu klikněte na tlačítko Application Insights a vyhledejte prostředek aplikace hello správné klíčem hello; nebo
  * V Průzkumníku řešení Visual Studio klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, konfigurovat. Resetovat hello aplikace toosend telemetrie toohello správné zdroje.
  * Pokud nemůžete najít hello odpovídající klíče, zkontrolujte, zda používáte hello stejné přihlašovací údaje v sadě Visual Studio jako toohello portálu.
    
    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
* V hello [domovský řídicí panel Microsoft Azure](https://portal.azure.com), podívejte se na stav služby mapy hello. Pokud jsou některé výstrahy indikace, počkejte, dokud se vrátili tooOK a pak zavřete a znovu otevřete okně vaší aplikace Application Insights.
* Zkontrolujte také [náš blog stav](http://blogs.msdn.com/b/applicationinsights-status/).
* Nebyla můžete psaní jakéhokoli kódu pro hello [server na straně SDK](app-insights-api-custom-events-metrics.md) které mohou změnit klíč instrumentace hello v `TelemetryClient` instancí nebo v `TelemetryContext`? Nebo se zapsat [filtru nebo vzorkování konfigurací](app-insights-api-filtering-sampling.md) může být filtrování se příliš mnoho?
* Pokud jste upravili souboru ApplicationInsights.config, pečlivě zkontrolujte konfiguraci hello [TelemetryInitializers a TelemetryProcessors](app-insights-api-filtering-sampling.md). Parametr nebo nesprávně názvem typu způsobit hello SDK toosend žádná data.

## <a name="q04"></a>Žádná data k zobrazení stránky, prohlížečů, použití
*Data v doba odezvy serveru a grafy požadavků na serveru, ale žádná data zobrazen v čas načtení stránky zobrazení nebo v hello prohlížeče nebo použití oken.*

Hello data pocházejí z skripty v hello webové stránky. 

* Pokud jste přidali Application Insights tooan existující webový projekt, [máte skripty hello tooadd ručně](app-insights-javascript.md).
* Ujistěte se, že Internet Explorer není v režimu kompatibility zobrazení vaší lokality.
* Pomocí funkce ladění hello prohlížeče (F12 na některé prohlížeče, pak zvolte síť) tooverify, která data je odesílána příliš`dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Žádná data závislostí nebo výjimky
V tématu [telemetrických závislostí](app-insights-asp-net-dependencies.md) a [telemetrie výjimek](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Žádná data o výkonu
Údaje o výkonu (procesoru, vstupně-výstupní operace rychlost a tak dále) je k dispozici pro [Java webové služby](app-insights-java-collectd.md), [aplikacích klasické pracovní plochy Windows](app-insights-windows-desktop.md), [IIS webové aplikace a služby, pokud nainstalujete monitorování stavu](app-insights-monitor-performance-live-website-now.md), a [Cloudových služeb azure](app-insights-azure.md). najdete ho v části nastavení servery.

Není k dispozici pro weby Azure.

## <a name="no-server-data-since-i-published-hello-app-toomy-server"></a>Žádná data (server), protože publikování serveru toomy aplikace hello
* Zkontrolujte, jestli jste ve skutečnosti zkopírovali všechny hello Microsoft. Server knihovny DLL ApplicationInsights toohello společně s Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
* V bráně firewall, bude pravděpodobně příliš[otevřít některé porty TCP](app-insights-ip-addresses.md#data-access-api).
* Pokud máte toouse proxy toosend mimo vaší podnikové síti, nastavte [defaultProxy –](https://msdn.microsoft.com/library/aa903360.aspx) v souboru Web.config.
* Windows Server 2008: Ujistěte se, máte nainstalovanou hello následující aktualizace: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-toosee-data-but-it-has-stopped"></a>Mohu použít toosee data, ale byla zastavena
* Zkontrolujte hello [stav blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Jste nedosáhli vaše měsíční kvóta datových bodů? Otevřete hello nastavení nebo kvóty a cena toofind limitu. Pokud ano, můžete upgradovat plán nebo platit dodatečnou kapacitu. V tématu hello [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-hello-data-im-expecting"></a>Nejsou zobrazeny všechny hello data, která se byla očekávána
Pokud vaše aplikace odešle velké množství dat a používáte hello Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, hello [adaptivního vzorkování](app-insights-sampling.md) funkce může pracovat a odesílat pouze procento vaší telemetrie. 

Ji můžete vypnout, ale není to doporučeno. Vzorkování je navržené tak, aby související telemetrii správně přenosu, k diagnostickým účelům. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Chybná zeměpisné data v telemetrie uživatele
Hello města, oblast a země dimenze jsou odvozené z IP adres a není vždy přesné.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Výjimka „metoda nebyla nalezena“ při spuštění v Azure Cloud Services
Vytvořili jste sestavení pro .NET 4.6? Verze 4.6 není v rolích Azure Cloud Services podporována automaticky. Před spuštěním aplikace [nainstalujte pro každou roli verzi 4.6](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="still-not-working"></a>Pořád nefunguje...
* [Fórum Application Insights](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

