---
title: "Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service"
description: "Kurz ukazuje, jak pomocí socket.io ve webové aplikaci node.js hostované v Azure."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c4c4af36-3ecf-4619-b586-ca90d53ce35b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: e1aa539e1134884261ea7464bfda6d14815618d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service
Socket.IO poskytuje v reálném čase komunikaci mezi serverem node.js a klienty, kteří používají objekty WebSockets. Také podporuje přechod na jiné přenosů (například dlouhým dotazováním), které pracují s starší prohlížeče. Tento kurz vás provede procesem hostování Socket.IO na základě chatovací aplikace jako webové aplikace Azure a ukazují, jak můžete škálovat aplikace pomocí [Azure Redis Cache]. Další informace o Socket.IO najdete v tématu <http://socket.io/>.

> [!NOTE]
> Postupy v této úloze platí pro [App Service Web Apps]; pro cloudové služby, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].
> 
> 

## <a name="download-the-chat-example"></a>Stáhněte si příklad chatu
Pro tento projekt použijeme příklad chat z [úložiště Socket.IO GitHub]. Proveďte následující kroky ke stažení v příkladu a přidat jej do projektu, které jste vytvořili.

1. Stáhněte si [ZIP nebo GZ archivovat verze] Socket.IO projektu (verze 1.3.5 byl použit pro tento dokument)
2. Extrahujte archivní a zkopírujte **příklady\\chat** adresář do nového umístění. Například  **\\uzlu\\chat**.

## <a name="modify-appjs-and-install-modules"></a>Úprava souboru app.js a instalace modulů
1. Přejmenujte **index.js** do souboru **app.js**. To umožňuje Azure a zjistit, že se jedná o aplikace Node.js.
2. Otevřete **app.js** soubor v textovém editoru. Změnit na řádek obsahující `var io = require('../..')(server);` jak je uvedeno níže:
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. Otevřete **package.json** souboru a přidejte odkaz na socket.io pod `dependencies`, jak je uvedeno níže:
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. Z příkazového řádku, změňte  **\\uzlu\\chat** adresáře a použijte npm pro instalaci modulů, které vyžadují tuto aplikaci:
   
        npm install
   
    Moduly nainstaluje do složky s názvem **node_modules**.

## <a name="create-an-azure-web-app"></a>Vytvoření webové aplikace Azure
Postupujte podle těchto kroků k vytvoření webové aplikace Azure, povolit publikování Git a potom povolit podporu protokolu WebSocket pro webovou aplikaci.

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Bezplatná zkušební verze Azure</a>.
> 
> 

1. Instalace rozhraní příkazového řádku Azure (Azure CLI) a připojte se k předplatnému Azure. V tématu [instalace a konfigurace rozhraní příkazového řádku Azure CLI](../cli-install-nodejs.md).
2. Pokud je vaše první čas nastavení úložiště v Azure, budete muset vytvořit přihlašovací údaje. Z příkazového řádku Azure zadejte následující příkaz:
   
        azure site deployment user set [username] [password]
3. Změnit na  **\\node\chat** adresáře a použijte následující příkaz k vytvoření nové Azure web app a místní úložiště Git. Tento příkaz vytvoří také Git vzdálené s názvem "azure".
   
        azure site create mysitename --git
   
    'Mysitename, je potřeba nahradit jedinečný název pro vaši webovou aplikaci.
4. Potvrďte existující soubory v místním úložišti pomocí následujících příkazů:
   
        git add .
        git commit -m "Initial commit"
5. Push soubory do úložiště Azure Web Apps pomocí následujícího příkazu:
   
        git push azure master
   
    Po zobrazení výzvy zadejte přihlašovací údaje z kroku 2. Zobrazí se stavové zprávy a jako moduly importují na serveru. Po dokončení tohoto procesu se bude hostovat aplikaci na vaší webové aplikace Azure.
   
   > [!NOTE]
   > Během instalace modulu, může dojít k chybám, se importované projekt... nebyla nalezena ". Ty můžete bezpečně ignorovat.
   > 
   > 
6. Socket.IO používá, které nejsou povolené ve výchozím nastavení v Azure. Pokud chcete povolit webové sokety, použijte následující příkaz:
   
        azure site set -w
   
    Pokud se zobrazí výzva, zadejte název webové aplikace.
   
   > [!NOTE]
   > Příkaz se "azure lokality set -w", pracovní pouze s verzí 0.7.4 nebo vyšší z rozhraní příkazového řádku Azure. Můžete také povolit podpory protokolu WebSocket pomocí [portálu Azure](https://portal.azure.com).
   > 
   > Chcete-li povolit Websocket pomocí portálu Azure, klikněte na webovou aplikaci v okně webové aplikace, klikněte na **všechna nastavení** > **nastavení aplikace**. V části **webové sokety**, klikněte na tlačítko **na**. Potom klikněte na **Uložit**.
   > 
   > 
7. Chcete-li zobrazit webové aplikace v Azure, použijte následující příkaz Spustit webový prohlížeč a přejít na hostované webové aplikace:
   
        azure site browse

Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.

## <a name="scale-out"></a>Horizontální navýšení kapacity
Socket.IO aplikace lze škálovat pomocí **adaptér** distribuovat zprávy a události mezi více instancí aplikace. Když jsou k dispozici několik adaptérů [socket.io redis] adaptéru lze snadno použít s funkcí Azure Redis Cache.

> [!NOTE]
> Dodatečný požadavek pro škálování Socket.IO řešení je podpora pro trvalé relace. Trvalé relace jsou povolené ve výchozím nastavení pro webové aplikace Azure prostřednictvím směrování žádostí na Azure. Další informace najdete v tématu [spřažení instanci v Azure weby].
> 
> 

### <a name="create-a-redis-cache"></a>Vytvoření mezipaměti Redis
Proveďte kroky v [vytvoření mezipaměti ve službě Azure Redis Cache] k vytvoření nové mezipaměti.

> [!NOTE]
> Uložit **název hostitele** a **primární klíč** pro mezipaměť, jako to bude potřeba v dalších krocích.
> 
> 

### <a name="add-the-redis-and-socketio-redis-modules"></a>Přidejte redis a moduly socket.io redis
1. Z příkazového řádku, změňte  **\\uzlu\\chat** adresáře a použijte následující příkaz.
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > Verze zadané v tomto příkazu jsou použité při testování v tomto článku verze.
   > 
   > 
2. Změnit **app.js** souboru přidejte následující řádků ihned po`var io = require('socket.io')(server);`
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    Nahraďte **redishostname** a **rediskey** s názvem hostitele a klíč pro vaše mezipaměť Redis.
   
    Tato akce vytvoří publikování a přihlášení k odběru klienta do mezipaměti Redis vytvořili dříve. Klienti se pak používají s adaptérem ke konfiguraci Socket.IO použití Redis cache pro předávání zpráv a událostí mezi instancemi aplikace
   
   > [!NOTE]
   > Když **socket.io redis** adaptér může komunikovat přímo k Redis, aktuální verze nepodporuje ověřování vyžaduje Azure Redis cache. Tak vytvoření počátečního připojení se vytvoří pomocí **redis** modul a potom klientovi předána **socket.io redis** adaptér.
   > 
   > Zatímco Azure Redis Cache podporuje zabezpečené připojení pomocí portu 6380, moduly používané v tomto příkladu nepodporují zabezpečené připojení od 7/14/2014. Výše uvedený kód používá výchozí, nezabezpečená port 6379.
   > 
   > 
3. Uložit upravenou **app.js**

### <a name="commit-changes-and-redeploy"></a>Potvrzení změn a znovu nasaďte
Z příkazového řádku v  **\\uzlu\\chat** adresáře, použijte následující příkazy k potvrzení změn a znovu nasaďte aplikaci.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Jakmile se nabídne změny na server, je možné škálovat vaší lokality ve více instancích pomocí následujícího příkazu.

    azure site scale instances --instances #

Kde  **#**  je počet instancí pro vytvoření.

Do webové aplikace se můžete připojit z více prohlížečů nebo počítačů, ověřte, že jsou správně odeslány zprávy pro všechny klienty.

## <a name="troubleshooting"></a>Řešení potíží
### <a name="connection-limits"></a>Omezení počtu připojení
Azure Web Apps je k dispozici v několika SKU, které určují prostředky, které jsou k dispozici pro váš web. To zahrnuje počet povolených připojení protokolu WebSocket. Další informace najdete v tématu [stránce s cenami webové aplikace].

### <a name="messages-arent-being-sent-using-websockets"></a>Nejsou odesílány zprávy pomocí technologie WebSockets
Pokud klientský prohlížeč zachovat návratem zpět k dlouhé dotazování místo použití technologie WebSockets, může to být způsobeno jedním z následujících.

* **Pokuste se omezit přenosu, který je právě objekty WebSockets**
  
    Aby Socket.IO na používat jako zasílání zpráv přenosový protokol Websocket musí podporovat serverem a klientem objekty WebSockets. Pokud jeden z nich není, bude Socket.IO vyjednávat jiná přenosu, například dlouhým dotazováním. Je výchozím seznamu přenosů používá Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`. Můžete vynutit, aby používat pouze objekty WebSockets přidáním následující kód, který **app.js** po řádku obsahující soubor `, nicknames = {};`.
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > Všimněte si, že starší prohlížeče, které nepodporují objekty WebSockets nebudou moci připojit k webu, zatímco ve výše uvedeném kódu je aktivní, omezuje komunikaci pouze pro objekty WebSockets.
  > 
  > 
* **Používat protokol SSL**
  
    Technologie WebSockets spoléhá na některé nižší úrovně použít hlavičky protokolu HTTP, například **Upgrade** záhlaví. Některé zprostředkující síťová zařízení, jako jsou webové proxy servery, může odebrat tyto hlavičky. Chcete-li se tomuto problému vyhnout, můžete vytvořit připojení protokolu WebSocket přes protokol SSL.
  
    Snadný způsob, jak tomu je konfigurace Socket.IO na `match origin protocol`. To se dá pokyn Socket.IO pro zabezpečení komunikace objekty WebSockets stejný jako původní žádosti protokolu HTTP nebo HTTPS pro webovou stránku. Pokud prohlížeč používá adresu URL HTTPS navštíví váš web, nebude další komunikace protokolu WebSocket pomocí Socket.IO zabezpečené přes protokol SSL.
  
    Tento příklad, chcete-li povolit tuto konfiguraci upravit, přidejte následující kód, který **app.js** souboru po řádku obsahující `, nicknames = {};`.
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* **Ověřte nastavení v souboru web.config**
  
    Službě Azure web apps, které jsou hostiteli pomocí aplikace Node.js **web.config** souboru směrovat příchozí požadavky na aplikace Node.js. Pro objekty WebSockets fungovat správně s aplikací Node.js **web.config** musí obsahovat následující položku.
  
        <webSocket enabled="false"/>
  
    To zakáže IIS Websocket modul, který obsahuje vlastní implementace technologie WebSockets a je v konfliktu s Node.js konkrétní protokolu WebSocket moduly, například Socket.IO. Pokud tento řádek není přítomen nebo je nastaven na `true`, může to být z důvodu, který přenos protokolu WebSocket nefunguje pro vaši aplikaci.
  
    Za normálních okolností aplikací Node.js nezahrnují **web.config** souboru, takže weby Azure bude automaticky generovat za aplikací Node.js při jejich nasazení. Vzhledem k tomu, že tento soubor je automaticky generován na serveru, musíte použít protokol FTP nebo FTPS adresu URL pro váš web k zobrazení tohoto souboru. FTP a FTPS adresy URL můžete najít pro svůj web na portálu classic výběrem webové aplikace a potom **řídicí panel** odkaz. Adresy URL se zobrazí v **rychlého přehledu** části.
  
  > [!NOTE]
  > **Web.config** souboru se vygeneruje pouze weby Azure, pokud vaše aplikace neposkytuje jeden. Pokud jste zadali **web.config** soubor v kořenu projektu aplikace, použije se ve službě Azure Web Apps.
  > 
  > 
  
    Pokud položka není k dispozici nebo je nastaven na hodnotu `true`, pak byste měli vytvořit **web.config** v kořenovém aplikace Node.js a zadejte hodnotu `false`.  Pro srovnání dole je výchozí **web.config** pro aplikaci, která používá **app.js** jako vstupní bod.
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    Pokud vaše aplikace používá vstupní bod jiné než **app.js**, musí nahraďte všechny výskyty **app.js** s správný vstupní bod. Například nahrazení **app.js** s **server.js**.

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service], kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak k vytvoření chatovací aplikace hostované na webové aplikace Azure. Můžete také hostovat tuto aplikaci jako cloudová služba Azure. Pokyny o tom, jak to provést, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].

Další informace naleznete také [středisku pro vývojáře Node.js].

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service najdete v tématu: [Azure App Service a její vliv na stávající služby Azure].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[stránce s cenami webové aplikace]: http://go.microsoft.com/fwlink/?LinkId=511643
[sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../cli-install-nodejs.md
[Azure App Service a její vliv na stávající služby Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[středisku pro vývojáře Node.js]: /develop/nodejs/
[možnosti vyzkoušet si App Service]: https://azure.microsoft.com/try/app-service/
[spřažení instanci v Azure weby]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[vytvoření mezipaměti ve službě Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io redis]: https://github.com/socketio/socket.io-redis
[úložiště Socket.IO GitHub]: https://github.com/socketio/socket.io
[ZIP nebo GZ archivovat verze]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
