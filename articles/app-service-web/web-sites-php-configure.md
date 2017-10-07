---
title: aaaConfigure PHP v Azure App Service Web Apps | Microsoft Docs
description: "Zjistěte, jak tooconfigure hello výchozí instalace PHP a přidat vlastní instalace PHP pro webové aplikace v Azure App Service."
services: app-service
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 2e461e4a269a4ce5614f5f05560f38bc53066251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>Nakonfigurovat PHP ve službě Azure App Service Web Apps
## <a name="introduction"></a>Úvod
Tento průvodce vám ukáže, jak tooconfigure hello předdefinované modulu PHP runtime pro webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)zadejte vlastní modul runtime PHP a povolit rozšíření. toouse App Service, zaregistrujte si hello [bezplatnou zkušební verzi]. tooget hello nejvíce z této příručky, je vhodné nejdříve vytvořit webové aplikace PHP ve službě App Service.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-hello-built-in-php-version"></a>Postupy: Změna hello předdefinované verzi PHP
Ve výchozím nastavení PHP 5.5 je nainstalovaná a okamžitě k dispozici pro použití při vytváření webové aplikace služby App Service. Hello nejlepší způsob, jak toosee hello k dispozici verze revize jeho výchozí konfigurace a hello povolené rozšíření je toodeploy skript, který volá hello [phpinfo()] funkce.

Verze PHP 5.6 a PHP 7.0 jsou také k dispozici, ale není povoleno ve výchozím nastavení. hello tooupdate verze PHP, postupujte podle jednoho z těchto metod:

### <a name="azure-portal"></a>Azure Portal
1. Procházet tooyour webové aplikace ve hello [portálu Azure](https://portal.azure.com) a klikněte na hello **nastavení** tlačítko.
   
    ![Nastavení webové aplikace][settings-button]
2. Z hello **nastavení** okně vyberte **nastavení aplikace** a vyberte novou verzi PHP hello.
   
    ![Nastavení aplikace][application-settings]
3. Klikněte na tlačítko hello **Uložit** tlačítko hello horní části hello **webové nastavení aplikace** okno.
   
    ![Uložit nastavení konfigurace][save-button]

### <a name="azure-powershell-windows"></a>Prostředí Azure PowerShell (Windows)
1. Otevřete prostředí Azure PowerShell a účet přihlášení tooyour:
   
        PS C:\> Login-AzureRmAccount
2. Nastavit verzi PHP hello pro hello webovou aplikaci.
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. verze PHP Hello je teď nastavená. Můžete potvrdit, tato nastavení:
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a>Rozhraní příkazového řádku Azure (Linux, Mac, Windows)
toouse hello rozhraní příkazového řádku Azure, musíte mít **Node.js** v počítači nainstalována.

1. Otevřete terminál a tooyour účet pro přihlášení.
   
        azure login
2. Nastavit verzi PHP hello pro hello webovou aplikaci.
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. verze PHP Hello je teď nastavená. Můžete potvrdit, tato nastavení:
   
        azure site show {app-name}

> [!NOTE] 
> Hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) jsou příkazy, které jsou ekvivalentní toohello výše:
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-hello-built-in-php-configurations"></a>Postupy: Změna hello předdefinovaných konfigurací PHP
Pro žádné předdefinované modul runtime PHP můžete změnit některé možnosti konfigurace hello podle následujících kroků hello. (Informace o souboru php.ini direktivy najdete v tématu [seznam php.ini direktivy].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>Změna PHP\_INI\_uživatele, PHP\_INI\_PERDIR, PHP\_INI\_všechna nastavení konfigurace
1. Přidat [. user.ini] souboru tooyour kořenový adresář.
2. Přidat konfiguraci nastavení toohello `.user.ini` souboru pomocí hello stejnou syntaxi, kterou použijete v `php.ini` souboru. Například pokud byste chtěli tooturn hello `display_errors` nastavení a nastavte `upload_max_filesize` nastavení too10M, vaše `.user.ini` soubor bude obsahovat tento text:
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on toowrite errors tood:\home\LogFiles\php_errors.log
        ; log_errors=On
3. Nasazení webové aplikace.
4. Restartujte hello webové aplikace. (Restartování je nutné, protože hello četnost PHP, který čte `.user.ini` soubory se řídí hello `user_ini.cache_ttl` nastavení, což je úrovně nastavení systému a ve výchozím nastavení je 300 sekund (5 minut). Restartování webové aplikace hello vynutí nové nastavení PHP tooread hello v hello `.user.ini` souboru.)

Jako alternativní toousing `.user.ini` souboru, můžete použít hello [ini_set()] funkce v možnosti konfigurace tooset skripty, které nejsou direktivy úrovni systému.

### <a name="changing-phpinisystem-configuration-settings"></a>Změna PHP\_INI\_nastavení konfigurace systému
1. Přidat nastavení aplikace tooyour webové aplikace s klíčem hello `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`
2. Vytvoření `settings.ini` souboru pomocí konzole Kudu (http://&lt;název lokality&gt;. scm.azurewebsite.net) v hello `d:\home\site\ini` adresáře.
3. Přidat konfiguraci nastavení toohello `settings.ini` souboru pomocí hello stejnou syntaxi, kterou použijete v souboru php.ini. Například pokud byste chtěli toopoint hello `curl.cainfo` nastavení tooa `*.crt` souboru a nastavit wincache.maxfilesize nastavení too512K, vaše `settings.ini` soubor bude obsahovat tento text:
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. Restartujte změny hello tooload webové aplikace.

## <a name="how-to-enable-extensions-in-hello-default-php-runtime"></a>Postupy: povolení rozšíření v hello výchozí PHP runtime
Jak jsme uvedli v předchozí části hello hello nejlepší způsob, jak toosee hello výchozí jazyk PHP verze, jeho výchozí konfigurace a hello povolené rozšíření je toodeploy skript, který volá [phpinfo()]. tooenable další rozšíření, postupujte podle následujících kroků hello:

### <a name="configure-via-ini-settings"></a>Konfigurovat pomocí nastavení ini
1. Přidat `ext` directory toohello `d:\home\site` adresáře.
2. Uveďte `.dll` soubory rozšíření v hello `ext` directory (například `php_xdebug.dll`). Ujistěte se, že jsou kompatibilní se výchozí verze PHP a jsou VC9 a bez bezpečného přístupu (nts) kompatibilní hello rozšíření.
3. Přidat nastavení aplikace tooyour webové aplikace s klíčem hello `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`
4. Vytvoření `ini` souboru v `d:\home\site\ini` názvem `extensions.ini`.
5. Přidat konfiguraci nastavení toohello `extensions.ini` souboru pomocí hello stejnou syntaxi, kterou použijete v souboru php.ini. Například pokud byste chtěli MongoDB a nástroje XDebug rozšíření hello tooenable vaše `extensions.ini` soubor bude obsahovat tento text:
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. Restartujte změny hello tooload webové aplikace.

### <a name="configure-via-app-setting"></a>Konfigurovat pomocí nastavení aplikace
1. Přidat `bin` directory toohello kořenový adresář.
2. Uveďte `.dll` soubory rozšíření v hello `bin` directory (například `php_xdebug.dll`). Ujistěte se, že jsou kompatibilní se výchozí verze PHP a jsou VC9 a bez bezpečného přístupu (nts) kompatibilní hello rozšíření.
3. Nasazení webové aplikace.
4. Procházet tooyour webové aplikace ve hello portálu Azure a klikněte na hello **nastavení** tlačítko.
   
    ![Nastavení webové aplikace][settings-button]
5. Z hello **nastavení** okně vyberte **nastavení aplikace** a posuňte se toohello **nastavení aplikace** části.
6. V hello **nastavení aplikace** , vytvořte **PHP_EXTENSIONS** klíč. Hello hodnota pro tento klíč bude kořenová cesta relativní toowebsite: **bin\your přípona souboru**.
   
    ![Povolit rozšíření v nastavení aplikace][php-extensions]
7. Klikněte na tlačítko hello **Uložit** tlačítko hello horní části hello **webové nastavení aplikace** okno.
   
    ![Uložit nastavení konfigurace][save-button]

Zend rozšíření jsou podporovány také pomocí **PHP_ZENDEXTENSIONS** klíč. tooenable více rozšíření, zahrnují obsahuje čárkami oddělený seznam `.dll` soubory pro hodnotu nastavení aplikace hello.

## <a name="how-to-use-a-custom-php-runtime"></a>Postupy: použití vlastního modulu PHP runtime
App Service Web Apps můžete místo rozšíření modulu PHP runtime výchozí hello použít modulu PHP runtime poskytují tooexecute skripty PHP. dají se nakonfigurovat Hello modul runtime, který zadáte `php.ini` soubor, který je taky zadat. toouse vlastní modulu PHP runtime s webovými aplikacemi, postupujte podle následujících kroků hello.

1. Získejte není bezpečná pro přístup z více vláken, VC9 nebo VC11 kompatibilní verzi PHP pro systém Windows. Poslední verze PHP pro systém Windows naleznete zde: [http://windows.php.net/download/]. Starší verze najdete v archivu hello zde: [http://windows.php.net/downloads/releases/archives/].
2. Upravit hello `php.ini` soubor pro vaše runtime. Všimněte si, že všechna nastavení, které jsou systému úroveň jen direktivy budou ignorovány webové aplikace. (Informace o systému úroveň jen direktivy najdete v tématu [seznam php.ini direktivy]).
3. Volitelně můžete přidat rozšíření tooyour PHP runtime a je povolit v hello `php.ini` souboru.
4. Přidat `bin` directory tooyour kořenový adresář a put hello adresář, který obsahuje vaše modulu PHP runtime v ní (například `bin\php`).
5. Nasazení webové aplikace.
6. Procházet tooyour webové aplikace ve hello portálu Azure a klikněte na hello **nastavení** tlačítko.
   
    ![Nastavení webové aplikace][settings-button]
7. Z hello **nastavení** okně vyberte **nastavení aplikace** a posuňte se toohello **mapování obslužných rutin** části. Přidat `*.php` toohello rozšíření pole a přidejte hello cesta toohello `php-cgi.exe` spustitelný soubor. Když vložíte vaší modulu PHP runtime v hello `bin` directory hello kořenové aplikace, bude cesta hello `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
   
    ![Zadejte rutinu mapování obslužné rutiny][handler-mappings]
8. Klikněte na tlačítko hello **Uložit** tlačítko hello horní části hello **webové nastavení aplikace** okno.
   
    ![Uložit nastavení konfigurace][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Postupy: povolení autora automation v Azure
Ve výchozím nastavení služby App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP. Pokud používáte [nasazení Git](app-service-deploy-local-git.md), můžete povolit composer.json zpracování během `git push` povolením rozšíření autora hello.

> [!NOTE]
> Můžete [hlas podpora prvotřídní autora ve službě App Service zde](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
> 
> 

1. Ve vašem PHP webové okně aplikace v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nástroje** > **rozšíření**.
   
    ![Azure Portal nastavení okno tooenable autora automation v Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.
   
    ![Přidat autora rozšíření tooenable autora automation v Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. Klikněte na tlačítko **OK** tooaccept právní podmínky. Klikněte na tlačítko **OK** znovu tooadd hello rozšíření.
   
    Hello **nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora hello.  
    ![Přijměte právní podmínky tooenable autora automation v Azure](./media/web-sites-php-configure/composer-extension-view.png)
4. Teď, provádět `git add`, `git commit`, a `git push` jako v předchozím oddílu hello. Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.
   
    ![Nasazení Git s autora automation v Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[bezplatnou zkušební verzi]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[seznam php.ini direktivy]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

