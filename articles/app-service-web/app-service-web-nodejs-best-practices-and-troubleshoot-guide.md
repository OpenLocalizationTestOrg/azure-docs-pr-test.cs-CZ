---
title: "postupy aaaBest a Průvodci odstraňováním potíží aplikace uzlu ve webových aplikacích Azure"
description: "Zjistěte, hello osvědčené postupy a řešení potíží pro uzlu aplikace v Azure Web Apps."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 975898142a224f14df1091a46d16e9074d9e2831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Osvědčené postupy a Průvodci odstraňováním potíží aplikace uzlu ve webových aplikacích Azure
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto článku se dozvíte hello osvědčené postupy a řešení potíží pro [uzel aplikace](app-service-web-get-started-nodejs.md) systémem Azure Webapps (s [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Buďte opatrní při používání při řešení potíží na vašem provozním serveru. Doporučuje se tootroubleshoot vaši aplikaci ve mimo produkční nastavit třeba přípravný slot a když hello problém vyřešen, Prohodit vaší přípravný slot s produkční slot.
> 
> 

## <a name="iisnode-configuration"></a>Konfigurace modulu IISNODE
To [soubor schématu](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) zobrazuje všechny hello nastavení, které mohou být konfigurovány pro modulu iisnode. Některá nastavení hello, která bude užitečná pro vaše aplikace jsou:

* nodeProcessCountPerApplication
  
    Toto nastavení určuje hello počet uzlu procesy, které jsou spouštěny na aplikaci služby IIS. Výchozí hodnota je 1. Nastavením této too0 můžete spustit tolik node.exe jako vaše základní počet virtuálních počítačů. Doporučená hodnota je 0 pro většinu aplikací, takže můžete využít všechny hello jader na váš počítač. Node.exe je zřetězený, jeden node.exe spotřebuje maximálně 1 základní tooget maximální výkon a mimo aplikaci uzlu je vhodnější tooutilize všechny jader.
* nodeProcessCommandLine
  
    Toto nastavení určuje hello cesta toohello node.exe. Můžete nastavit tuto hodnotu toopoint tooyour node.exe verzi.
* maxConcurrentRequestsPerProcess
  
    Toto nastavení určuje maximální počet souběžných požadavků odeslaných modulu iisnode tooeach node.exe hello. Na azure webapps hello výchozí hodnota pro tento je nekonečné. Nebudete mít tooworry o tomto nastavení. Mimo azure webapps hello výchozí hodnota je 1024. Můžete chtít tooconfigure, to v závislosti na tom, kolik požadavků vaší aplikace získává a jak rychle vaše aplikace zpracovává každý požadavek.
* maxNamedPipeConnectionRetry
  
    Toto nastavení určuje hello maximálním počtem pokusů modulu iisnode bude opakovat vytváření připojení s názvem kanálu toosend hello žádosti přes toonode.exe hello. Toto nastavení v kombinaci s namedPipeConnectionRetryDelay Určuje celkový časový limit hello každého požadavku v rámci modulu iisnode. Výchozí hodnota je 200 na Azure Webapps. Celkový časový limit v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Tato nastavení ovládacích prvků hello množství modulu iisnode čas (v ms) bude čekat mezi každou opakování toosend požadavku toonode.exe přes hello pojmenovaný kanál. Výchozí hodnota je 250ms.
    Celkový časový limit v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Ve výchozím nastavení je hello celkový časový limit v modulu iisnode na azure webapps 200 \* 250ms = 50 sekund.
* logDirectory
  
    Toto nastavení určuje hello adresáře, kde bude modulu iisnode protokolu stdout/stderr. Výchozí hodnota je modulu iisnode, která je relativní toohello hlavního skriptu adresář (adresář, kde se nachází hlavní server.js)
* debuggerExtensionDll
  
    Toto nastavení určuje, jaká verze modulu iisnode nástroj node-inspector se bude používat při ladění aplikace uzlu. Aktuálně modulu iisnode. inspector 0.7.3.dll a iisnode inspector.dll jsou hello 2 pouze platné hodnoty pro toto nastavení. Výchozí hodnota je modulu iisnode. inspector 0.7.3.dll. verze modulu iisnode. inspector 0.7.3.dll používá uzlu inspector 0.7.3 a používá, takže bude potřeba tooenable websocket na vaší webové aplikace azure toouse tuto verzi. V tématu <http://www.ranjithr.com/?p=98> další podrobnosti o tom, jak tooconfigure modulu iisnode toouse hello nástroj nové node-inspector.
* flushResponse
  
    Hello výchozí chování služby IIS je, že ukládány data odpovědi až too4MB před vyčištění nebo až hello konce hello odpovědi, nastane dříve. iisnode nabízí nastavení toooverride toto chování: tooflush fragment textu entity odpovědi hello co nejdříve modulu iisnode přijímá je z node.exe, musíte tooset hello iisnode/@flushResponse atribut v souboru web.config too'true':
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Povolení vyprázdnění každý fragment textu entity odpovědi hello zvýší nároky na výkon, sníží hello propustnost systému hello % ~ 5 (od v0.1.13), tak, aby byl co nejlepší tooscope pouze tooendpoints tato nastavení vyžadující odpovědi (např. použití hello vysílání datového proudu. <location> element v souboru web.config hello)
  
    V přidání toothis pro streamování aplikací, budete potřebovat sadu responseBufferLimit tooalso z vaší too0 obslužná rutina modulu iisnode.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Toto je seznam souborů, které budou sledovány změny oddělených středníkem. Soubor tooa změn způsobí, že toorecycle aplikace hello. Každý záznam se skládá z název volitelné adresáře plus název požadované souboru, které jsou relativní toohello adresáře, kde je najít vstupní bod hello hlavní aplikace. Zástupné znaky nejsou povoleny v hello souboru pouze část s názvem. Výchozí hodnota je "\*. js;web.config"
* recycleSignalEnabled
  
    Výchozí hodnota je false. Pokud je povoleno, aplikace uzlu můžete připojit tooa pojmenovaný kanál (proměnnou prostředí modulu IISNODE\_řízení\_kanálu) a odeslat zprávu "recyklaci". To způsobí, že hello w3wp toorecycle řádně.
* idlePageOutTimePeriod
  
    Výchozí hodnota je 0, což znamená, že tato funkce je vypnutá. Pokud bude toosome nastavte hodnotu větší než 0, modulu iisnode stránky se všechny jeho podřízené zpracovává každých milisekund 'idlePageOutTimePeriod'. toounderstand co stránky na prostředky, naleznete toothis [dokumentaci](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Toto nastavení se bude hodit pro aplikace, které spotřebovávají velké množství paměti a má toopageout paměti toodisk příležitostně toofree si některé paměti RAM.

> [!WARNING]
> Buďte opatrní při povolování hello následující nastavení konfigurace na výrobní aplikace. Doporučuje se toonot je povolit v za provozu výrobní aplikace.
> 
> 

* debugHeaderEnabled
  
    Hello výchozí hodnota je false. Pokud nastavení tootrue, modulu iisnode přidá odpověď záhlaví modulu iisnode-debug tooevery HTTP odpovědi HTTP odešle hodnotu hlavičky modulu iisnode-debug hello je adresa URL. Jednotlivé diagnostické informace může shromažďovat informace prohlížením hello fragment adresy URL, ale mnohem lepší vizualizace se dosahuje otevření hello adresy URL v prohlížeči hello.
* loggingEnabled
  
    Toto nastavení řídí hello protokolování stdout a stderr pomocí modulu iisnode. Iisnode zachytíte stdout/stderr z uzlu procesy, které ho spouští a zápis toohello adresáři zadaném v nastavení "logDirectory" hello. Jakmile je povolit, vaše aplikace bude zápis systém toohello souborů protokolů a v závislosti na hello množství protokolování provádí hello aplikace, může být ovlivnit výkon.
* devErrorsEnabled
  
    Výchozí hodnota je false. Pokud nastavíte tootrue, modulu iisnode zobrazí hello HTTP stavový kód a kód chyby Win32 v prohlížeči. Kód win32 Hello bude užitečné při ladění určité typy potíží.
* debuggingEnabled (nepovolujte na provozním serveru)
  
    Toto nastavení řídí funkce ladění. Pomocí nástroje node-inspector je integrovaná modulu Iisnode. Povolením tohoto nastavení můžete povolit ladění aplikace uzlu. Jakmile je toto nastavení povoleno, bude modulu iisnode rozložení soubory hello nástroj nezbytné node-inspector v adresáři 'debuggerVirtualDir' na hello první ladění požadavek tooyour uzel aplikace. Odesláním požadavku toohttp://yoursite/server.js/debug můžete načíst hello nástroj node-inspector. Segment adresy URL ladění hello můžete ovládat nastavení 'debuggerPathSegment'. Ve výchozím nastavení debuggerPathSegment = "debug". Tento identifikátor GUID tooa například můžete nastavit tak, aby se obtížnější toobe zjištěny jinými uživateli.
  
    Zaškrtněte toto políčko [odkaz](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) Další informace o ladění.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scénáře a doporučení nebo řešení potíží
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Moje aplikace uzlu je volání příliš mnoho odchozí.
Mnoho aplikací vhodnější toomake odchozí připojení v rámci jejich regulární operace. Například když přijde žádost, aplikace uzlu by má toocontact rozhraní REST API jinde a získat žádost o některé informace tooprocess hello. Je vhodnější toouse zachování připojení agenta zachovat při volání protokolu http nebo https. Například můžete použít modul agentkeepalive hello jako aktivní agenta zachovat při provádění těchto odchozí volání. Tím je zajištěno, že jsou na azure webapp virtuálních počítačů a snížení režie hello vytváření nových soketů pro každý požadavek odchozí opakovaně hello sockets. Navíc tím je zajištěno, že používáte menší počet sockets toomake mnoho odchozí požadavky a proto vám nepřekračují hello maxSockets, které jsou přiděleny na virtuální počítač. Doporučení na Azure Webapps by tooset hello agentKeepAlive maxSockets hodnotu tooa celkem 160 sockets na virtuální počítač. To znamená, že pokud máte 4 node.exe systémem hello virtuálních počítačů, je vhodnější tooset hello agentKeepAlive maxSockets too40 za node.exe což je 160 celkový na virtuální počítač.

Příklad [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) konfigurace:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Tento příklad předpokládá, že máte 4 node.exe systémem virtuálního počítače. Pokud máte jiný počet node.exe systémem hello virtuálních počítačů, budete mít maxSockets hello toomodify nastavení odpovídajícím způsobem.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Moje uzel aplikace využívala příliš mnoho procesoru.
Doporučení se zobrazí na portálu o vysoké spotřeby procesoru pravděpodobně z Azure Webapps. Můžete také toowatch nastavení monitorování pro určité [metriky](web-sites-monitor.md). Při kontrole využití hello procesoru na hello [řídicího panelu portálu Azure](../application-insights/app-insights-web-monitor-performance.md), Zkontrolujte prosím hello maximální hodnoty pro procesor, nemusíte vynechalo hello maximální hodnoty.
V případech, kde by vaše aplikace využívala příliš mnoho procesorů a důvod, proč nelze vysvětlují budete potřebovat tooprofile aplikace uzlu.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profilace uzlu aplikace v azure webapps s v8: profileru
Například vyslovte vám umožní mít aplikace na hello world má tooprofile, jak je uvedeno níže:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Přejděte tooyour scm lokality https://yoursite.scm.azurewebsites.net/DebugConsole

Zobrazí se příkazový řádek a jak je uvedeno níže. Přejděte do adresáře webu/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Spusťte příkaz hello "npm install profileru v8:"

To by měli nainstalovat v8: profileru pod uzlem\_directory moduly a všechny jeho závislé součásti.
Nyní upravte vaše server.js tooprofile vaší aplikace.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Hello výše změny bude profil funkce WriteConsoleLog hello a zapište si too'profile.cpuprofile výstup hello profil se v souboru pod vaší lokality wwwroot. Odešlete k aplikaci tooyour požadavku. Zobrazí se soubor 'profile.cpuprofile' vytvořil v rámci vaší lokality wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Tento soubor stáhnout a je nutné tooopen tento soubor s nástroji F12 Chrome. Stiskněte tlačítko F12 na chrome, a potom klikněte na hello "Profily kartu". Klikněte na tlačítko "Zatížení". Vyberte souboru profile.cpuprofile, který jste právě stáhli. Klikněte na hello profil, který jste právě načetli.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Zobrazí se, že 95 % času hello se spotřebovávají WriteConsoleLog funkce jak je uvedeno níže. To také dozvíte hello linek čísla a zdrojové soubory, které způsobí hello problém.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Moje uzel aplikace využívala příliš mnoho paměti.
Pravděpodobně obdržíte doporučení z Azure Webapps na portálu o vysoké spotřeby paměti. Můžete také toowatch nastavení monitorování pro určité [metriky](web-sites-monitor.md). Při kontrole hello využití paměti na hello [řídicího panelu portálu Azure](../application-insights/app-insights-web-monitor-performance.md), zkontrolujte hello maximální hodnoty pro paměť, nemáte vynechalo hello maximální hodnoty.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Zjištění paměti a Diffing haldy pro node.js
Můžete použít [uzlu memwatch](https://github.com/lloyd/node-memwatch) nevracení toohelp identifikovat paměti.
Můžete nainstalovat memwatch stejně jako v8: profileru a úpravy, které váš kód toocapture a rozdílové haldách tooidentify hello nevracení paměti v aplikaci.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moje node.exe jsou získávání ukončeny náhodně
Existují z několika důvodů, proč může být děje toto:

1. Aplikace je vyvolání nezachycená výjimek – d: Zkontrolujte prosím\\domácí\\LogFiles\\aplikace\\protokolování errors.txt podrobnosti v souboru hello na hello došlo k výjimce. Tento soubor má trasování zásobníku hello, takže můžete řešit vaší aplikace na základě.
2. Vaše aplikace využívala příliš mnoho paměti, která ovlivňuje jiné procesy z Začínáme. Pokud hello celkovou velikost paměti virtuálního počítače je % zavřít too100, může vaše node.exe ukončeny podle hello proces manager toolet jiné procesy práci prvního toodo některé. toofix, buď zajistěte, aby vaše aplikace není vracena paměť, nebo pokud je aplikace opravdu potřebuje toouse velké množství paměti, prosím škálovat tooa větší virtuální počítač s mnohem víc paměti RAM.

### <a name="my-node-application-does-not-start"></a>Moje aplikace uzlu nespustí.
Pokud vaše aplikace vrací 500 chyby při spuštění, může být z několika důvodů:

1. Node.exe se nenachází ve správném umístění hello. Zkontrolujte nastavení nodeProcessCommandLine.
2. Soubor skriptu hlavní se nenachází ve správném umístění hello. Zkontrolujte soubor web.config a zda hello název souboru hello hlavního skriptu v souboru hello hlavní skriptu části odpovídá hello obslužné rutiny.
3. Konfigurace souboru Web.config není správný – zkontrolujte hodnoty názvy nastavení hello.
4. Studený Start – aplikace trvá příliš dlouho toostartup. Pokud vaše aplikace trvá déle, než (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 sekund modulu iisnode vrátí chybu 500. Zvýšit hello hodnoty těchto nastavení toomatch aplikace čas tooprevent iisnode začínají vypršení časového limitu a vrácení hello 500 Chyba.

### <a name="my-node-application-crashed"></a>Moje aplikace uzlu došlo k chybě
Aplikace je vyvolání nezachycená výjimek – d: Zkontrolujte prosím\\domácí\\LogFiles\\aplikace\\protokolování errors.txt podrobnosti v souboru hello na hello došlo k výjimce. Tento soubor má trasování zásobníku hello, takže můžete řešit vaší aplikace na základě.

### <a name="my-node-application-takes-too-much-time-toostartup-cold-start"></a>Moje aplikace uzlu trvá příliš mnoho času toostartup (studený Start)
Nejčastější příčinou je, že aplikace hello má mnoho souborů v uzlu hello\_moduly a tooload pokusů aplikace hello většinu těchto souborů při spuštění. Ve výchozím nastavení protože vaše soubory jsou umístěny ve sdílené síťové složce hello na Azure Webapps, načítání mnoho souborů může chvíli trvat.
Některá řešení toomake, tím rychleji se:

1. Ujistěte se, že máte ploché závislostí strukturu a žádné duplicitní závislosti pomocí npm3 tooinstall moduly.
2. Zkuste toolazy zatížení vaší uzlu\_moduly a zatížení všechny moduly hello při spuštění. To znamená, že toorequire('module') hello volání by mělo být provedeno, když potřebujete ve skutečnosti v rámci funkce hello pokusíte toouse hello modulu.
3. Azure Webapps nabízí funkci místní mezipaměti. Tato funkce zkopíruje obsah z hello síťové sdílené složky toohello místního disku na hello virtuálních počítačů. Vzhledem k tomu, že jsou soubory hello místní, hello doba uzlu načítání\_moduly je mnohem rychlejší. -Tento [dokumentace](../app-service/app-service-local-cache.md) vysvětluje, jak toouse v místní mezipaměti více podrobností.

## <a name="iisnode-http-status-and-substatus"></a>Stav protokolu http modulu IISNODE a podřízeného stavu
To [zdrojový soubor](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) seznamy všechny hello možné stav nebo substatus kombinace iisnode může vrátit v případě chyby.

Povolit FREB pro kód chyby win32 vaší aplikace toosee hello (zkontrolujte, zda povolit FREB pouze na webech mimo produkční z důvodů výkonu).

| Stav protokolu HTTP | Podřízený stav protokolu HTTP | Možný důvod? |
| --- | --- | --- |
| 500 |1000 |Se některé problém odeslání žádosti o tooIISNODE hello – zaškrtněte, pokud byl zahájen node.exe. Node.exe může mít došlo k chybě při spuštění. Zkontrolujte konfiguraci web.config chyby. |
| 500 |1001 |-Win32Error 0x2 - aplikace neodpovídá toohello adresy URL. Zkontrolujte adresu URL přepsání pravidla nebo pokud má vaše aplikace express hello správné trasy definované. -Win32Error 0x6d – pojmenovaný kanál je zaneprázdněn – Node.exe nepřijímá požadavky, protože hello kanálu je zaneprázdněný. Zkontrolujte vysoké využití procesoru. -Další chyby – zkontrolujte, pokud node.exe došlo k chybě. |
| 500 |1002 |Došlo k chybě Node.exe – zkontrolujte d:\\domácí\\LogFiles\\errors.txt protokolování pro trasování zásobníku. |
| 500 |1003 |Konfigurace kanálu problém – nikdy zobrazí to ale v takovém případě hello s názvem konfigurace kanálu je nesprávný. |
| 500 |1004-1018 |Při odesílání hello požadavek nebo zpracování odpovědi hello z node.exe se nějaká chyba. Zkontrolujte, zda node.exe došlo k chybě. Zkontrolujte d:\\domácí\\LogFiles\\errors.txt protokolování pro trasování zásobníku. |
| 503 |1000 |Není dostatek paměti tooallocate více s názvem kanálu připojení. Zkontrolujte, proč aplikace využívala mnoho paměti. Zkontrolujte hodnotu nastavení maxConcurrentRequestsPerProcess. Pokud jeho není nekonečné a budete mít mnoho požadavků, zvýšit tuto hodnotu tooprevent této chybě. |
| 503 |1001 |Žádost nebylo možné odeslat toonode.exe, protože je recyklace aplikace hello. Po hello aplikace má recyklované, by měl obvykle zpracovat požadavky. |
| 503 |1002 |Kód chyby win32 zkontrolujte skutečné důvodu – žádost nebylo možné odeslat tooa node.exe. |
| 503 |1003 |Pojmenovaný kanál je příliš zaneprázdněn – zaškrtněte, pokud uzel spotřebovává velké množství procesoru |

Je nastavení v rámci NODE.exe názvem uzlu\_čeká na vyřízení\_kanálu\_instance. Ve výchozím nastavení mimo azure webapps tato hodnota je 4. To znamená, že node.exe přijmout jenom 4 požadavků současně na hello pojmenovaný kanál. Na Azure Webapps tato hodnota se nastavuje too5000 a tato hodnota by měla být dostatečně vhodné pro většinu uzlu aplikací běžících na azure webapps. 503.1003 byste neměli vidět na azure webapps, protože máme vysoké hodnoty pro hello uzlu\_čeká na vyřízení\_kanálu\_instance.  |

## <a name="more-resources"></a>Další zdroje informací
Postupujte podle těchto odkazů toolearn více informací o aplikací node.js v Azure App Service.

* [Začínáme s webovými aplikacemi Node.js ve službě Azure App Service](app-service-web-get-started-nodejs.md)
* [Jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md)
* [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Středisko pro vývojáře Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Zkoumání hello Super tajný klíč Kudu ladění konzoly](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

