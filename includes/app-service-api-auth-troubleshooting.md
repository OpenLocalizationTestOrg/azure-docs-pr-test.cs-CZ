Většina chyb ověřování hello čas v důsledku nastavení nekonzistentní nebo nesprávné konfigurace. Tady jsou některé konkrétní návrhy toocheck věcí.

* Ujistěte se, že nebyla neproběhly hello **Uložit** tlačítko odkudkoli. Tento problém je často snadno toodo a hello výsledkem je, že je budete vidí hello správné hodnoty na stránky portálu, ale ve skutečnosti se nebyly uloženy na hello prostředí Azure nebo v aplikaci Azure AD.
* Pro nastavení nakonfigurovaná v hello **nastavení aplikace** okno hello portál Azure, ujistěte se, že hello správné rozhraní API aplikaci nebo webové aplikaci byl vybrán při nastavení hello byla zadána.  Také se ujistěte, že nastavení hello byla zadána jako **nastavení aplikace** a není **připojovací řetězce**, jako je podobný hello formát hello dvě části.
* Pro ověřování tooa JavaScript front-endu, stažení hello manifest znovu tooverify, `oauth2AllowImplicitFlow` byla úspěšně změněna příliš`true`.
* Ověřte, používá protokol HTTPS, bez ohledu na nakonfigurované adresy URL:
  
  * V projektu kódu
  * V CORS
  * V prostředí Azure nastavení aplikace pro každou aplikaci API a webové aplikace
  * V nastavení aplikace služby Azure AD.
    
    Všimněte si, že pokud zkopírujete adresu URL aplikace API z portálu hello, často má `http://` a máte toomanually ho změnit příliš`https://`.
* Ujistěte se, že byly úspěšně nasazeny změny kódu. Například v řešení více projekty je možné toochange projektu je kód a omylem vyberte jednu z hello ostatní Pokud hodláte toodeploy hello změnu.
* Ujistěte se, že budete tooHTTPS adresy URL v prohlížeči není adres URL protokolu HTTP. Ve výchozím nastavení, Visual Studio vytvoří publikovat profily pomocí adres URL protokolu HTTP, a který je co otevře v prohlížeči hello po nasazení projektu.
* Pro ověřování tooa JavaScript front-endu Ujistěte se, zda CORS správně nakonfigurovaná na aplikaci API hello hello volání kód JavaScript. Pokud máte pochybnosti o tom, zda text hello problém související se sdílením CORS, zkuste "*" jako hello povolené původní adresu URL. 
* Pro JavaScript front-end otevřete tooget karta konzoly vývojářských nástrojů prohlížeče na další informace o chybě a zkontrolujte požadavky HTTP na hello sítě. Konzole chybové zprávy však může být zavádějící. Pokud se zobrazí zprávu s upozorněním na chybu CORS, může být problém skutečné hello ověřování. Můžete se podívat, pokud to je případ hello spuštěním aplikace hello s ověřováním dočasně dočasně zakázána.
* Pro aplikace .NET API, ujistěte se, co nejvíce informací se zobrazuje v chybových zprávách nejdříve nastavením [customErrors režimu tooOff](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Pro aplikace .NET API, spusťte [vzdálené relace ladění](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)a zkontrolujte hodnoty hello hello proměnných, které se předávají toocode, který používá ADAL tooacquire tokenu nosiče nebo kód, který kontroluje deklarace identity s identifikátorem hlavní služby hello očekává. Všimněte si, že váš kód můžete vyzvedne, až hodnoty konfigurace z mnoha různých zdrojů, takže je možné toofind výskyt nečekaných událostí tímto způsobem. Například, pokud jste překlep `ida:ClientId` jako `ida:ClientID` při konfiguraci nastavení prostředí Azure App Service, hello kódu může získat hello `ida:ClientId` hodnotu, která hledá ze souboru Web.config hello, nastavení služby Azure App Service hello je ignorována. 
* Pokud věcí nefungují v normálním okně Internet Exploreru, existující protokolu v může vadit; Zkuste InPrivate a zkuste to Chrome nebo Firefox.

