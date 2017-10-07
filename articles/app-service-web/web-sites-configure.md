---
title: "aaaConfigure webové aplikace v Azure App Service"
description: "Jak tooconfigure webové aplikace v Azure App Services"
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 8697ab6f21cfeb470e11f0d82c68692d43142fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a>Konfigurace webových aplikací v prostředí Azure App Service
Toto téma vysvětluje, jak tooconfigure webovou aplikaci pomocí hello [portálu Azure].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Nastavení aplikace
1. V hello [portálu Azure], otevřete okno hello hello webové aplikace.
2. Klikněte na tlačítko **všechna nastavení**.
3. Klikněte na tlačítko **nastavení aplikace**.

![Nastavení aplikace][configure01]

Hello **nastavení aplikace** okno obsahuje nastavení, které jsou seskupené v rámci několika kategorií.

### <a name="general-settings"></a>Obecná nastavení
**Verze Framework**. Pokud vaše aplikace používá tyto architektury, nastavte tyto možnosti: 

* **Rozhraní .NET framework**: Sada hello rozhraní .NET framework verze. 
* **PHP**: nastavit hello PHP verzi nebo **OFF** toodisable PHP. 
* **Java**: verzi Javy vyberte hello nebo **OFF** toodisable Java. Použití hello **webový kontejner** možnost toochoose mezi verzí Tomcat a Jetty.
* **Python**: hello vyberte verzi jazyka Python, nebo **OFF** toodisable Python.

Technické z důvodů povolení pro aplikace Java zakáže možnosti hello .NET, PHP a Python.

<a name="platform"></a>
**Platforma**. Vybere, zda běží vaše webová aplikace v prostředí 32bitové nebo 64bitové verze. Hello 64-bit prostředí vyžaduje režimu Basic nebo Standard. Uvolněte a režimy sdílené vždy spustit v prostředí 32-bit.

**Webové sokety**. Nastavit **ON** tooenable hello protokolu WebSocket; například, pokud vaše webová aplikace používá [ASP.NET SignalR] nebo [socket.io].

<a name="alwayson"></a>
**Always On**. Ve výchozím nastavení jsou uvolněna webové aplikace, pokud jsou některé dobu nečinnosti. To umožňuje ušetřit prostředky systému hello. V režimu Basic nebo Standard, povolit **Always On** aplikace hello tookeep načíst všechny hello čas. Pokud vaše aplikace běží nepřetržité webové úlohy nebo běží webové úlohy aktivaci pomocí výrazu CRON, měli byste povolit **Always On**, nebo nemusí spolehlivě spuštění hello webové úlohy.

**Spravované verze kanálu**. Nastaví hello IIS [režim kanálů]. Nechte toto nastavení tooIntegrated (výchozí hello) pouze tehdy, je starší verze aplikace, který vyžaduje starší verze služby IIS.

**Automatické prohození**. Pokud povolíte automatické prohození pro slot nasazení, služby App Service automaticky Prohodit hello webové aplikace do produkčního prostředí, při nabízené aktualizaci toothat slot. Další informace najdete v tématu [nasazení toostaging sloty pro webové aplikace v Azure App Service](web-sites-staged-publishing.md).

### <a name="debugging"></a>Ladění
**Vzdálené ladění**. Umožňuje vzdálené ladění. Když je povolené, můžete použít vzdáleného ladicího programu hello v sadě Visual Studio tooconnect přímo tooyour webové aplikace. Vzdálené ladění zůstane zapnutá 48 hodin. 

### <a name="app-settings"></a>Nastavení aplikace
Tato část obsahuje dvojice název/hodnota, které vaší webové aplikace načte při spuštění. 

* Pro aplikace .NET, tato nastavení jsou vloženy do vaší konfigurace .NET `AppSettings` v době běhu přepsání stávajícího nastavení. 
* Aplikace PHP, Python, Java a uzel mají přístup k tato nastavení jako proměnné prostředí za běhu. Pro každé nastavení aplikace jsou vytvořeny dvou proměnných prostředí; jednu s názvem hello určeného hello položky nastavení aplikace a druhý s předponou APPSETTING_. Oba obsahují hello stejnou hodnotu.

### <a name="connection-strings"></a>Připojovací řetězce
Připojovací řetězce pro odkazované zdroje. 

Pro aplikace .NET, tyto připojovací řetězce jsou vloženy do konfiguraci .NET `connectionStrings` nastavení v době běhu přepsání existujících položek, kde klíč hello rovná hello název propojené databáze. 

Pro aplikace PHP, Python, Java a uzel budou tato nastavení k dispozici jako proměnné prostředí v době běhu předponu hello typ připojení. předpony proměnné prostředí Hello jsou následující: 

* SQL Server:`SQLCONNSTR_`
* MySQL:`MYSQLCONNSTR_`
* Databáze SQL:`SQLAZURECONNSTR_`
* Vlastní:`CUSTOMCONNSTR_`

Například, pokud byly s názvem připojovací řetězec databáze MySql `connectionstring1`, by přístupná prostřednictvím proměnné prostředí hello `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Výchozí dokumenty
výchozí dokument Hello je hello webové stránky, která se zobrazí v hello adresy URL kořenového adresáře pro web.  první odpovídající soubor Hello v seznamu hello se používá. 

Webové aplikace může použít modulů na adresu URL na základě trasy, že místo obsluhující statický obsah, v takovém případě že je jako takový výchozí dokument.    

### <a name="handler-mappings"></a>Mapování obslužných rutin
Pomocí této oblasti tooadd vlastní skript procesory toohandle požadavků pro konkrétní přípony souborů. 

* **Rozšíření**. Hello souboru rozšíření toobe zpracovává například *.php nebo handler.fcgi. 
* **Skript procesoru cesta**. absolutní cesta Hello hello skriptu procesoru. Požadavky toofiles splňujících příponu souboru hello budou zpracovány procesorem hello skriptu. Použijte hello cestu `D:\home\site\wwwroot` toorefer tooyour aplikace kořenový adresář.
* **Další argumenty**. Volitelné argumenty příkazového řádku pro procesor skriptu hello 

### <a name="virtual-applications-and-directories"></a>Virtuální aplikace a adresáře
tooconfigure virtuální aplikace a adresáře, zadejte každý virtuální adresář a kořenové složky jeho odpovídající fyzická cesta relativní toohello webu. Volitelně můžete vybrat hello **aplikace** políčko toomark virtuální adresář jako aplikace.

## <a name="enabling-diagnostic-logs"></a>Povolení diagnostických protokolů
diagnostické protokoly tooenable:

1. V okně hello pro vaši webovou aplikaci, klikněte na tlačítko **všechna nastavení**.
2. Klikněte na tlačítko **diagnostické protokoly**. 

Možnosti pro zápis z webové aplikace, která podporuje protokolování diagnostických protokolů: 

* **Protokolování aplikací**. Zapíše protokoly aplikací toohello systému souborů. Protokolování vydrží po dobu 12 hodin. 

**Úroveň**. Pokud je povoleno protokolování aplikací, tato možnost určuje hello množství informací, které se bude zaznamenaná (chyba, upozornění, informace nebo podrobné).

**Protokolování webového serveru**. Protokoly se ukládají do hello rozšířený formát protokolu W3C souboru. 

**Podrobné chybové zprávy**. Uloží podrobné chybové zprávy soubory .htm. Hello soubory jsou uloženy v části /LogFiles/DetailedErrors. 

**Trasování neúspěšných žádostí**. Protokoly se nezdařilo požadavky tooXML soubory. Hello soubory jsou uloženy v rámci/LogFiles/W3SVC*xxx*, kde xxx je jedinečný identifikátor. Tato složka obsahuje soubor XSL a jeden nebo více souborů XML. Zda toodownload hello soubor XSL proveďte, protože poskytuje funkce pro formátování a filtrování hello obsah souborů XML hello.

soubory protokolu hello tooview, musíte vytvořit přihlašovací údaje serveru FTP, následujícím způsobem:

1. V okně hello pro vaši webovou aplikaci, klikněte na tlačítko **všechna nastavení**.
2. Klikněte na tlačítko **přihlašovací údaje nasazení**.
3. Zadejte uživatelské jméno a heslo.
4. Klikněte na **Uložit**.

![Nastavení přihlašovacích údajů nasazení][configure03]

Hello úplné FTP uživatelské jméno je "app\username", kde *aplikace* je hello název webové aplikace. Hello uživatelské jméno je uvedena v okně webové aplikace hello, v části **Essentials**.  

![Přihlašovací údaje pro nasazení serveru FTP][configure02]

## <a name="other-configuration-tasks"></a>Další konfigurační úlohy
### <a name="ssl"></a>PROTOKOL SSL
V režimu Basic nebo Standard můžete nahrát certifikáty SSL pro vlastní doménu. Další informace najdete v tématu [Povolit HTTPS pro webovou aplikaci]. 

Klikněte na tlačítko tooview nahrané certifikáty, **všechna nastavení** > **vlastní domény a SSL**.

### <a name="domain-names"></a>Názvy domén
Přidáte vlastní názvy domén pro vaši webovou aplikaci. Další informace najdete v tématu [Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service].

Klikněte na názvy domén, tooview **všechna nastavení** > **vlastní domény a SSL**.

### <a name="deployments"></a>Nasazení
* Nastavte průběžné nasazování. V tématu [toodeploy pomocí Git ve službě Azure App Service Web Apps](web-sites-deploy.md).
* Nasazovací sloty. V tématu [nasazení tooStaging prostředí pro webové aplikace v Azure App Service].

Klikněte na nasazovací sloty, tooview **všechna nastavení** > **nasazovací sloty**.

### <a name="monitoring"></a>Monitorování
V režimu Basic nebo Standard můžete otestovat hello dostupnosti koncových bodů protokolu HTTP nebo HTTPS, z až toothree zeměpisné polohy umístění. Monitorování test se nezdaří, pokud hello kódu odpovědi HTTP dojde k chybě (4xx nebo 5xx) nebo hello odpovědi trvá déle než 30 sekund. Koncový bod je považován za dostupný, pokud hello monitorovací testy úspěšné ze všech hello zadané umístění. 

Další informace najdete v tématu [postupy: sledování stavu koncový bod webové].

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service], kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Konfigurace vlastní domény ve službě Azure App Service]
* [Povolit HTTPS pro aplikace v Azure App Service]
* [Škálování webové aplikace v Azure App Service]
* [Základní informace o monitorování pro webové aplikace v Azure App Service]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[portálu Azure]: https://portal.azure.com/
[Konfigurace vlastní domény ve službě Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[nasazení tooStaging prostředí pro webové aplikace v Azure App Service]: ./web-sites-staged-publishing.md
[Povolit HTTPS pro aplikace v Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[postupy: sledování stavu koncový bod webové]: http://go.microsoft.com/fwLink/?LinkID=279906
[Základní informace o monitorování pro webové aplikace v Azure App Service]: ./web-sites-monitor.md
[režim kanálů]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Škálování webové aplikace v Azure App Service]: ./web-sites-scale.md
[socket.io]: ./web-sites-nodejs-chat-app-socketio.md
[vyzkoušet službu App Service]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
