---
title: "technologie aaaOpen-source časté otázky k Azure webové aplikace | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se technologie open source v hello funkce Web Apps služby Azure App Service."
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
ms.openlocfilehash: 35cff4f322859d25972747cf55aa7c4316381a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Technologie Open source nejčastější dotazy pro webové aplikace v Azure

Tento článek obsahuje odpovědi toofrequently dotazy (FAQ) o problémech s nástrojem technologie open source pro hello [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a>Databázi ClearDB je mimo provoz. Jak to lze vyřešit?

Databáze s problémy, kontaktujte [ClearDB podporu](https://www.cleardb.com/developers/help/support). 

Odpovědi toocommon dotazy týkající se ClearDB, najdete v části [cleardb – nejčastější dotazy k](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-isnt-my-cleardb-database-listed-in-hello-portal"></a>Proč není uvedená Moje databázi ClearDB portálu hello?

Když vytvoříte databázi ClearDB v hello [portál Azure](http://portal.azure.com/), databáze hello se nezobrazí v hello [portál Azure classic](http://manage.windowsazure.com/). toowork tomuto problému vyhnout, můžete ručně propojit vaší databáze toohello webové aplikace.

Podobně platí když vytvoříte databázi ClearDB v hello [portál Azure classic](http://manage.windowsazure.com/), neuvidíte vaše databáze v hello [portál Azure](http://portal.azure.com/). V takovém případě je k dispozici žádné alternativní řešení. 

Další informace najdete v tématu [nejčastější dotazy pro databáze ClearDB MySQL službou Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a>Proč nebyl migrován databázi ClearDB během migrace Moje předplatné?

Když provádíte migraci prostředků ve předplatných, platí určitá omezení. Databáze ClearDB MySQL je služba třetích stran a nemigruje během migrace předplatnému Azure.

Pokud nespravujete hello migraci databáze MySQL před zahájením migrace vašich prostředků Azure, může být databáze MySQL cleardb – není k dispozici. tooavoid to nejdřív ručně migrovat databázi ClearDB a potom migrovat hello předplatné Azure pro webové aplikace.

Další informace najdete v tématu [nejčastější dotazy pro databáze ClearDB MySQL službou Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="how-do-i-turn-on-php-logging-tootroubleshoot-php-issues"></a>Jak zapnout na PHP protokolování tootroubleshoot PHP problémy?

tooturn na PHP protokolování:

1. Přihlaste se tooyour [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).
2. V horní nabídce hello, vyberte **ladění konzoly** > **CMD**.
3. Vyberte hello **lokality** složky.
4. Vyberte hello **wwwroot** složky.
5. Vyberte hello  **+**  ikonu a potom vyberte **nový soubor**.
6. Nastaví název souboru hello příliš**. user.ini**.
7. Vyberte ikonu tužky hello vedle příliš**. user.ini**.
8. V souboru hello přidejte tento kód:`log_errors=on`
9. Vyberte **Uložit**.
10. Vyberte ikonu tužky hello vedle příliš**wp-config.php**.
11. Změna toohello textu hello následující kód:
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging too/wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings tooscreendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors tooscreenini_set('display_errors', 0);
   ```
12. V hello portál Azure, v nabídce webové aplikace hello restartování webové aplikace.

Další informace najdete v tématu [protokoly chyb povolit WordPress](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>Jak se přihlásit chyb aplikací Python v aplikacích, které jsou hostované ve službě App Service?

chyb aplikací Python toocapture:

1. V hello portál Azure, ve vaší webové aplikaci, vyberte **nastavení**.
2. Na hello **nastavení** vyberte **nastavení aplikace**.
3. V části **nastavení aplikace**, zadejte následující dvojice klíč/hodnota hello:
    * Klíč: WSGI_LOG
    * Hodnota: D:\home\site\wwwroot\logs.txt (zadejte vaši volbu názvu souboru)

Teď byste měli vidět chyby v souboru logs.txt hello ve složce wwwroot hello.

## <a name="how-do-i-change-hello-version-of-hello-nodejs-application-that-is-hosted-in-app-service"></a>Změna hello verzi hello aplikace Node.js, který je hostován ve službě App Service

verze hello toochange hello aplikace Node.js, můžete použít jednu z hello následující možnosti:

*   V hello portálu Azure, použijte **nastavení aplikace**.
    1. V hello portálu Azure přejděte tooyour webové aplikace.
    2. Na hello **nastavení** vyberte **nastavení aplikace**.
    3. V **nastavení aplikace**, můžete zahrnout WEBSITE_NODE_DEFAULT_VERSION jako klíč hello a hello verze Node.js chcete používat jako hodnota hello.
    4. Přejděte tooyour [Kudu konzoly](https://*yourwebsitename*.scm.azurewebsites.net).
    5. toocheck hello verze Node.js, zadejte následující příkaz hello:  
   ```
   node -v
   ```
*   Upravte soubor iisnode.yml hello. Změna verze Node.js hello v souboru iisnode.yml hello jenom nastaví hello běhového prostředí, jemuž modul iisnode používá. Vaše Kudu cmd a jiné dál používat verze Node.js hello, který je nastavený v **nastavení aplikace** v hello portálu Azure.

    tooset hello iisnode.yml ručně, vytvořte soubor iisnode.yml v kořenové složce aplikace. V souboru hello patří hello následující řádek:
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   Nastavit soubor iisnode.yml poskytovaný rozhraním hello pomocí package.json během nasazení zdroj ovládacího prvku.
    Hello proces řízení nasazení Azure zdroj zahrnuje hello následující kroky:
    1. Přesune obsahu toohello webové aplikace Azure.
    2. Vytvoří výchozí skript nasazení, pokud není k dispozici (deploy.cmd, soubory .deployment) hello webové aplikace kořenové složky.
    3. Spustí skript nasazení, ve kterém se vytvoří soubor iisnode.yml Pokud zmínili verze Node.js hello v souboru package.json hello > modul`"engines": {"node": "5.9.1","npm": "3.7.3"}`
    4. soubor iisnode.yml poskytovaný rozhraním Hello má hello následující řádek kódu:
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-hello-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>V mé aplikace WordPress, který je hostován ve službě App Service se zobrazují hello zpráva "Chyba při navazování připojení k databázi". Jak odstranit to?

Pokud se zobrazí tato chyba v aplikaci Azure WordPress, tooenable php_errors.log a debug.log, dokončení hello kroky popsané v [protokoly chyb povolit WordPress](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

Pokud jsou povolené protokoly hello, reprodukujte hello chyba a potom zkontrolujte protokoly toosee hello, pokud používáte mimo připojení:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded hello ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Pokud se zobrazí tato chyba v souborech debug.log nebo php_errors.log, vaše aplikace přesahuje hello počet připojení. Pokud máte na ClearDB, ověřte hello počet připojení, které jsou k dispozici ve vaší [plán služby](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>Jak mohu ladit aplikace Node.js, který je hostován ve službě App Service?

1.  Přejděte tooyour [Kudu konzoly](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Protokoly složce aplikace přejděte tooyour (D:\home\LogFiles\Application).
3.  V souboru logging_errors.txt hello kontrolovat obsahu.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Instalace nativních modulů Python v webové aplikace App Service nebo aplikace API

Některé balíčky nemusí nainstalovat pomocí nástroje pip v Azure. balíček Hello nemusí být k dispozici na hello indexu balíčků Pythonu nebo může být potřeba kompilátor (kompilátor není k dispozici na hello počítače se systémem hello webové aplikace ve službě App Service). Informace o instalaci nativních modulů ve webové aplikace aplikační služby a aplikace API najdete v tématu [moduly instalaci jazyka Python ve službě App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-tooapp-service-by-using-git-and-hello-new-version-of-python"></a>Nasazení tooApp aplikace Django služby pomocí Git a hello novou verzi jazyka Python

Informace o instalaci Django najdete v tématu [nasazení tooApp aplikace Django služby](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-hello-tomcat-log-files-located"></a>Kde jsou umístěny soubory pro protokolu hello Tomcat?

Pro Azure Marketplace a vlastní nasazení:

* Umístění složky: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* Soubory, které vás zajímají:
    * catalina. *rrrr mm-dd*.log
    * Správce hostitele. *rrrr mm-dd*.log
    * localhost. *rrrr mm-dd*.log
    * správce. *rrrr mm-dd*.log
    * site_access_log. *rrrr mm-dd*.log


Pro portál **nastavení aplikace** nasazení:

* Umístění složky: D:\home\LogFiles
* Soubory, které vás zajímají:
    * catalina. *rrrr mm-dd*.log
    * Správce hostitele. *rrrr mm-dd*.log
    * localhost. *rrrr mm-dd*.log
    * správce. *rrrr mm-dd*.log
    * site_access_log. *rrrr mm-dd*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>Jak odstranit chyb připojení ovladač JDBC?

Může zobrazit následující zprávy do protokolů Tomcat hello:

```
hello web application[ROOT] registered hello JDBC driver [com.mysql.jdbc.Driver] but failed toounregister it when hello web application was stopped. tooprevent a memory leak,hello JDBC Driver has been forcibly unregistered
```

Chyba tooresolve hello:

1. Odeberte hello sqljdbc*.jar soubor ze složky aplikace/lib.
2. Pokud používáte hello vlastní Tomcat nebo Azure Marketplace Tomcat webového serveru, zkopírujte složku lib toohello Tomcat tento .jar souboru.
3. Pokud chcete povolit Java z hello portálu Azure (vyberte **Java 1.8** > **Tomcat server**), kopírovat hello sqljdbc.* jar soubor ve složce hello, který je paralelní tooyour aplikace. Pak přidejte hello následující soubor web.config toohello nastavení cesty pro třídy:

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path toohello sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-toocopy-live-log-files"></a>Proč se při I pokus soubory za provozu protokolu toocopy zobrazí chyby?

Pokud se pokusíte toocopy soubory protokolu za provozu pro aplikace v jazyce Java (například Tomcat), zobrazí tato chyba FTP:

```
Error transferring file [filename] Copying files from remote side failed.
    
hello process cannot access hello file because it is being used by another process.
```

Hello chybová zpráva se může lišit v závislosti na hello FTP klienta.

Všechny aplikace v jazyce Java mít potíže uzamčení. Pouze Kudu podporuje stahování tohoto souboru je spuštěna aplikace hello.

Ukončení aplikace hello umožňuje přístup FTP toothese soubory.

Jiným řešením je toowrite webové úlohy, který spouští podle plánu a zkopíruje tyto soubory tooa jiný adresář. Ukázkový projekt, najdete v části hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projektu.

## <a name="where-do-i-find-hello-log-files-for-jetty"></a>Kde hello soubory protokolu pro Jetty

Pro vlastní nasazení a Marketplace hello soubor protokolu je ve složce D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs hello. Všimněte si, že umístění složky hello závisí na verzi hello Jetty, kterou používáte. Například cesta hello zde uvedené pro Jetty 9.1.2. Vyhledejte jetty_*YYYY_MM_DD*. stderrout.log.

Pro nasazení portálu nastavení aplikace soubor protokolu hello je v D:\home\LogFiles. Vyhledejte jetty_*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>Můžete odesílat e-maily z Azure webová aplikace?

Služby App Service nemá předdefinované e-mailové funkce. Některé funkční alternativami pro odesílání e-mailu z vaší aplikace, najdete v tématu to [Stack Overflow diskusní](http://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-tooanother-url"></a>Proč můj web WordPress přesměrování tooanother URL?

Pokud jste migrovali nedávno tooAzure, může být přesměrování WordPress toohello starou adresu URL domény. To je způsobeno nastavení v databázi MySQL hello.

WordPress kamarád + je rozšíření lokality Azure, můžete použít adresu URL pro přesměrování tooupdate hello přímo v databázi hello. Další informace o používání WordPress kamarád + najdete v tématu [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

Případně, pokud dáváte přednost toomanually aktualizace hello přesměrování URL pomocí dotazů SQL nebo PHPMyAdmin, najdete v části [WordPress: přesměrování toowrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>Změna hesla přihlášení WordPress

Pokud jste WordPress přihlašovací heslo zapomněli, můžete použít WordPress kamarád + tooupdate ho. tooreset své heslo, instalace hello WordPress kamarád + rozšíření webu Azure a pak dokončení hello kroky popsané v [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-toowordpress-how-do-i-resolve-this"></a>Nelze se přihlásit tooWordPress. Jak to lze vyřešit?

Pokud se přistihnete zamknout z WordPress po instalaci nedávno o modul plug-in, můžete mít vadný modulu plug-in. WordPress kamarád + je rozšíření webu Azure, které vám pomůžou zakažte moduly plug-in v WordPress. Další informace najdete v tématu [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>Jak migraci databáze WordPress?

Máte několik možností pro migraci databáze MySQL hello, který je připojený tooyour web WordPress:

* Vývojáři: Použití hello [příkazový řádek nebo PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Jiný vývojáři: Použít [WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>Jak pomoci, zabezpečit WordPress?

toolearn o osvědčené postupy zabezpečení pro WordPress, najdete v části [osvědčené postupy pro zabezpečení WordPress v Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-toouse-phpmyadmin-and-i-see-hello-message-access-denied-how-do-i-resolve-this"></a>Pokouším toouse PHPMyAdmin a hello zpráva "Přístup byl odepřen". Jak to lze vyřešit?

Tomuto problému může dojít, pokud hello MySQL v aplikaci funkce ještě neběží v této instanci služby App Service. tooresolve hello problém, zkuste tooaccess vašeho webu. Tím se spustí hello požadované procesů, včetně procesu hello MySQL v aplikaci. tooverify, MySQL v aplikace běží v Průzkumníka procesů, ujistěte se, že mysqld.exe je uvedena v hello procesy.

Po zajištění, že je spuštěna MySQL v aplikaci, zkuste toouse PHPMyAdmin.

## <a name="i-get-an-http-403-error-when-i-try-tooimport-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>Zobrazuje chybu HTTP 403 při akci tooimport nebo exportovat pomocí PHPMyadmin databáze MySQL v aplikaci. Jak to lze vyřešit?

Pokud používáte starší verzi Chrome, může se jednat známého problému. tooresolve hello problém, upgradu tooa novější verzi Chrome. Zkuste taky pomocí jiného prohlížeče, jako je Internet Explorer nebo Edge, kde hello problému nedojde.
