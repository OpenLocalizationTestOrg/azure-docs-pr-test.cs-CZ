---
title: "aaaCreate chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service"
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
ms.openlocfilehash: 3bd7867ccc297dc0a21c7a00cc9db06358877f5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service
Socket.IO poskytuje v reálném čase komunikaci mezi serverem node.js a klienty, kteří používají objekty WebSockets. Podporuje také záložní tooother přenosů (například dlouhým dotazováním), které pracují s starší prohlížeče. Tento kurz vás provede procesem hostování Socket.IO na základě chatovací aplikace jako webové aplikace Azure a ukazují, jak tooscale hello aplikace pomocí [Azure Redis Cache]. Další informace o Socket.IO najdete v tématu <http://socket.io/>.

> [!NOTE]
> Hello postupy v této úloze platí příliš[App Service Web Apps]; pro cloudové služby, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].
> 
> 

## <a name="download-hello-chat-example"></a>Stáhněte si příklad chat hello
Pro tento projekt použijeme hello chat příklad z hello [úložiště Socket.IO GitHub]. Proveďte následující kroky toodownload hello ukázka hello a přidejte ho toohello projektu, které jste vytvořili.

1. Stáhněte si [ZIP nebo GZ archivovat verze] hello Socket.IO projektu (verze 1.3.5 byl použit pro tento dokument)
2. Extrahování hello archivu a zkopírujte hello **příklady\\chat** directory tooa nové umístění. Například  **\\uzlu\\chat**.

## <a name="modify-appjs-and-install-modules"></a>Úprava souboru app.js a instalace modulů
1. Přejmenujte hello **index.js** souboru příliš**app.js**. To umožňuje Azure toodetect, že se jedná o aplikace Node.js.
2. Otevřete hello **app.js** soubor v textovém editoru. Změna hello řádek obsahující `var io = require('../..')(server);` jak je uvedeno níže:
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. Otevřete hello **package.json** souboru a přidejte odkaz toosocket.io pod `dependencies`, jak je uvedeno níže:
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. Z příkazového řádku hello, změnit toohello  **\\uzlu\\chat** adresáře a použijte tooinstall hello moduly npm vyžaduje této aplikace:
   
        npm install
   
    Moduly hello nainstaluje do složky s názvem **node_modules**.

## <a name="create-an-azure-web-app"></a>Vytvoření webové aplikace Azure
Postupujte podle těchto kroků toocreate webové aplikace Azure, povolit publikování Git a potom povolit podporu protokolu WebSocket pro hello webovou aplikaci.

> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Bezplatná zkušební verze Azure</a>.
> 
> 

1. Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI) a připojit tooyour předplatného Azure. V tématu [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).
2. Pokud je vaše první čas nastavení úložiště v Azure, musíte toocreate přihlašovací údaje. Z hello rozhraní příkazového řádku Azure zadejte následující příkaz hello:
   
        azure site deployment user set [username] [password]
3. Změnit toohello  **\\node\chat** adresář a použití hello následující příkaz toocreate nové webové aplikace Azure a místní úložiště Git. Tento příkaz vytvoří také Git vzdálené s názvem "azure".
   
        azure site create mysitename --git
   
    'Mysitename, je potřeba nahradit jedinečný název pro vaši webovou aplikaci.
4. Potvrďte hello existující soubory toohello místní úložiště pomocí hello následující příkazy:
   
        git add .
        git commit -m "Initial commit"
5. Push hello soubory toohello Azure Web Apps úložiště s hello následující příkaz:
   
        git push azure master
   
    Po zobrazení výzvy zadejte přihlašovací údaje z kroku 2. Zobrazí se stavové zprávy a jako moduly importují na serveru hello. Po dokončení tohoto procesu se bude hostovat aplikace hello na vaší webové aplikace Azure.
   
   > [!NOTE]
   > Během instalace modulu, může dojít k chybám, "hello importované projekt... nebyla nalezena". Ty můžete bezpečně ignorovat.
   > 
   > 
6. Socket.IO používá, které nejsou povolené ve výchozím nastavení v Azure. tooenable webové sokety, hello použijte následující příkaz:
   
        azure site set -w
   
    Pokud se zobrazí výzva, zadejte název hello hello webové aplikace.
   
   > [!NOTE]
   > Hello příkaz bude "azure lokality set -w" pracovní pouze s verzí 0.7.4 nebo vyšší hello rozhraní příkazového řádku Azure. Můžete také povolit podporu protokolu WebSocket pomocí hello [portálu Azure](https://portal.azure.com).
   > 
   > pomocí technologie WebSockets tooenable hello portálu Azure, klikněte na tlačítko hello webové aplikace v okně webové aplikace hello, klikněte na **všechna nastavení** > **nastavení aplikace**. V části **webové sokety**, klikněte na tlačítko **na**. Potom klikněte na **Uložit**.
   > 
   > 
7. tooview hello webové aplikace na Azure, použijte hello následující příkaz toolaunch webový prohlížeč a přejděte toohello hostované webové aplikace:
   
        azure site browse

Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.

## <a name="scale-out"></a>Horizontální navýšení kapacity
Socket.IO aplikace lze škálovat pomocí **adaptér** toodistribute zprávy a události mezi více instancí aplikace. Když jsou k dispozici několik adaptérů, hello [socket.io redis] adaptéru lze snadno použít s Azure Redis Cache funkce hello.

> [!NOTE]
> Dodatečný požadavek pro škálování Socket.IO řešení je podpora pro trvalé relace. Trvalé relace jsou povolené ve výchozím nastavení pro webové aplikace Azure prostřednictvím směrování žádostí na Azure. Další informace najdete v tématu [spřažení instanci v Azure weby].
> 
> 

### <a name="create-a-redis-cache"></a>Vytvoření mezipaměti Redis
Proveďte kroky hello v [vytvoření mezipaměti ve službě Azure Redis Cache] toocreate novou mezipaměť.

> [!NOTE]
> Uložit hello **název hostitele** a **primární klíč** pro mezipaměť, jak to bude potřeba v hello další kroky.
> 
> 

### <a name="add-hello-redis-and-socketio-redis-modules"></a>Přidat hello redis a moduly socket.io redis
1. Z příkazového řádku, změňte toohello  **\\uzlu\\chat** hello adresáře a použijte následující příkaz.
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > Hello verze uvedené v tomto příkazu jsou použité při testování v tomto článku verze hello.
   > 
   > 
2. Upravit hello **app.js** souboru tooadd hello následující řádků ihned po`var io = require('socket.io')(server);`
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    Nahraďte **redishostname** a **rediskey** s názvem hostitele hello a klíč pro vaše mezipaměť Redis.
   
    Tato akce vytvoří publikování a přihlášení k odběru toohello klienta Redis cache, které jste vytvořili dříve. Hello klientům jsou pak použít společně s hello adaptér tooconfigure Socket.IO toouse hello Redis cache pro předávání zpráv a událostí mezi instancemi aplikace
   
   > [!NOTE]
   > Při hello **socket.io redis** adaptér může komunikovat přímo tooRedis, aktuální verze hello nepodporuje hello ověřování, který Azure Redis cache. Takže hello počátečního připojení se vytvoří pomocí hello **redis** modulu, pak klient hello je předán toohello **socket.io redis** adaptéru.
   > 
   > Zatímco Azure Redis Cache podporuje zabezpečené připojení pomocí portu 6380, hello moduly používané v tomto příkladu nepodporují zabezpečené připojení od 7/14/2014. Hello výše kód používá hello výchozím nezabezpečená port 6379.
   > 
   > 
3. Uložit hello upravit **app.js**

### <a name="commit-changes-and-redeploy"></a>Potvrzení změn a znovu nasaďte
Z příkazového řádku v hello hello  **\\uzlu\\chat** adresáře, použijte následující příkazy toocommit změny hello a znovu nasaďte aplikace hello.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Jakmile se nabídne hello změny toohello serveru, je možné škálovat vaší lokality ve více instancích pomocí hello následující příkaz.

    azure site scale instances --instances #

Kde  **#**  je hello počet instancí toocreate.

Tooyour webové aplikace se můžete připojit z více tooverify prohlížeče nebo počítače, klienti tooall správně odesílání zpráv.

## <a name="troubleshooting"></a>Řešení potíží
### <a name="connection-limits"></a>Omezení počtu připojení
Azure Web Apps je k dispozici v několika SKU, které určují hello prostředky k dispozici tooyour lokality. To zahrnuje hello počet povolených připojení protokolu WebSocket. Další informace najdete v tématu hello [stránce s cenami webové aplikace].

### <a name="messages-arent-being-sent-using-websockets"></a>Nejsou odesílány zprávy pomocí technologie WebSockets
Pokud klientský prohlížeč zachovat návratem zpět toolong dotazování místo použití technologie WebSockets, může být způsobeno jedním z následujících hello.

* **Pokuste se omezit přenos toojust hello objekty WebSockets**
  
    Aby Socket.IO toouse Websocket jako hello zasílání zpráv na úrovni přenosu musí podporovat hello serverů i klientů objekty WebSockets. Pokud jeden nebo hello jiných není, bude Socket.IO vyjednávat jiná přenosu, například dlouhým dotazováním. je výchozím seznamu Hello přenosů používá Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`. Můžete vynutit jeho použití tooonly objekty WebSockets přidáním hello následující kód toohello **app.js** po hello řádek obsahující soubor `, nicknames = {};`.
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > Poznámka: starší prohlížeče, které nepodporují objekty WebSockets nebude možné tooconnect toohello lokality při hello výše kód je aktivní, omezuje jenom tooWebSockets komunikace.
  > 
  > 
* **Používat protokol SSL**
  
    Technologie WebSockets spoléhá na některé nižší úrovně použít hlavičky protokolu HTTP, jako je například hello **Upgrade** záhlaví. Některé zprostředkující síťová zařízení, jako jsou webové proxy servery, může odebrat tyto hlavičky. tooavoid tento problém, můžete vytvořit připojení protokolu WebSocket hello přes protokol SSL.
  
    Snadný způsob tooaccomplish to tooconfigure Socket.IO je příliš`match origin protocol`. To se dá pokyn Socket.IO toosecure Websocket komunikace hello stejný jako hello pocházející protokolu HTTP nebo HTTPS žádosti pro webovou stránku hello. Pokud prohlížeč používá toovisit adresy URL HTTPS vašeho webu, nebude další komunikace protokolu WebSocket pomocí Socket.IO zabezpečené přes protokol SSL.
  
    toomodify tento příklad tooenable tuto konfiguraci, přidejte následující kód toohello hello **app.js** souboru po hello řádek obsahující `, nicknames = {};`.
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* **Ověřte nastavení v souboru web.config**
  
    Použití Azure webové aplikace, které jsou hostiteli aplikací Node.js hello **web.config** souboru tooroute příchozí požadavky aplikace Node.js toohello. Pro objekty WebSockets toofunction správně s aplikacemi Node.js, hello **web.config** musí obsahovat následující položku hello.
  
        <webSocket enabled="false"/>
  
    To zakáže hello IIS Websocket modul, který obsahuje vlastní implementace technologie WebSockets a je v konfliktu s Node.js konkrétní protokolu WebSocket moduly, například Socket.IO. Pokud tento řádek není přítomen nebo je nastaven příliš`true`, může to být hello důvod, který přenos protokolu WebSocket hello nefunguje pro vaši aplikaci.
  
    Za normálních okolností aplikací Node.js nezahrnují **web.config** souboru, takže weby Azure bude automaticky generovat za aplikací Node.js při jejich nasazení. Vzhledem k tomu, že tento soubor je automaticky generovaný serverem hello, musíte použít hello FTP nebo FTPS adresa URL pro váš web tooview tento soubor. Můžete najít hello FTP a FTPS adresy URL pro svůj web portálu classic hello výběrem webové aplikace a pak hello **řídicí panel** odkaz. Hello adresy URL se zobrazí v hello **rychlého přehledu** části.
  
  > [!NOTE]
  > Hello **web.config** souboru se vygeneruje pouze weby Azure, pokud vaše aplikace neposkytuje jeden. Pokud jste zadali **web.config** soubor v kořenovém hello projektu aplikace, použije se ve službě Azure Web Apps.
  > 
  > 
  
    Pokud není k dispozici položka hello, nebo je nastavena hodnota tooa `true`, pak byste měli vytvořit **web.config** v kořenovém adresáři aplikace Node.js hello a zadejte hodnotu `false`.  Pro referenci hello níže je výchozí **web.config** pro aplikaci, která používá **app.js** jako hello vstupní bod.
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used toorun node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that hello server.js file is a node.js web app toobe handled by hello iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether hello incoming URL matches a physical file in hello /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped toohello node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using hello following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes toorestart hello server
                * node_env: will be propagated toonode as NODE_ENV environment variable
                * debuggingEnabled - controls whether hello built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    Pokud vaše aplikace používá vstupní bod jiné než **app.js**, musí nahraďte všechny výskyty **app.js** s hello opravte vstupní bod. Například nahrazení **app.js** s **server.js**.

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service], kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak toocreate chatovací aplikace hostované ve webové aplikace Azure. Můžete také hostovat tuto aplikaci jako cloudová služba Azure. Postup pro tooaccomplish, najdete v tématu [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].

Další informace najdete v tématu taky hello [středisku pro vývojáře Node.js].

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[stránce s cenami webové aplikace]: http://go.microsoft.com/fwlink/?LinkId=511643
[sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure hello Azure CLI]: ../cli-install-nodejs.md
[Azure App Service a její vliv na stávající služby Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[středisku pro vývojáře Node.js]: /develop/nodejs/
[vyzkoušet službu App Service]: https://azure.microsoft.com/try/app-service/
[spřažení instanci v Azure weby]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[vytvoření mezipaměti ve službě Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io redis]: https://github.com/socketio/socket.io-redis
[úložiště Socket.IO GitHub]: https://github.com/socketio/socket.io
[ZIP nebo GZ archivovat verze]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
