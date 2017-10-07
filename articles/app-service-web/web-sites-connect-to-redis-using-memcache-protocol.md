---
title: "aaaConnect App Service webové aplikace tooRedis prostřednictvím hello protokolu Memcache - Azure | Microsoft Docs"
description: "Připojení webové aplikace v Azure App service tooRedis mezipaměti pomocí protokolu Memcache hello"
services: app-service\web
documentationcenter: php
author: SyntaxC4
manager: erikre
editor: riande
ms.assetid: 0fcdf9fa-2995-4839-ba4d-cfa389c4ba06
ms.service: app-service-web
ms.devlang: php
ms.topic: get-started-article
ms.tgt_pltfrm: windows
ms.workload: na
ms.date: 02/29/2016
ms.author: cfowler
ms.openlocfilehash: 48036d60fbbced59eb1e37584f507fffffff753d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Připojení webové aplikace v Azure App Service tooRedis mezipaměti prostřednictvím hello protokolu Memcache
V tomto článku se dozvíte, jak tooconnect WordPress webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) příliš[Azure Redis Cache] [ 12] pomocí hello [Memcache] [ 13] protokolu. Pokud máte existující webovou aplikaci, která pro ukládání do mezipaměti v paměti používá server s protokolem Memcache, můžete migrovat tooAzure služby App Service a použití hello vlastních řešení ukládání do mezipaměti v Microsoft Azure s malými či žádnými změnit tooyour aplikace kód. Kromě toho můžete použít existující Memcache znalosti toocreate vysoce škálovatelnou a distribuované aplikace v Azure App Service pomocí Azure Redis Cache v paměti ukládání do mezipaměti, a to pomocí oblíbených rozhraní aplikací například .NET, PHP, Node.js, Java a Python.  

Služby App Service Web Apps umožňuje tento aplikační scénář pomocí hello Web Apps Memcache shim, což je místní server protokolem Memcache, který funguje jako proxy server Memcache pro ukládání do mezipaměti volání tooAzure Redis Cache. To umožňuje libovolné aplikaci, která komunikuje pomocí hello Memcache protokol toocache data s Redis Cache. Tento shim Memcache pracuje na úrovni protokolu hello, takže jej můžete používat všechny aplikace či rozhraní aplikace, dokud komunikuje pomocí protokolu Memcache hello.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Požadavky
Hello Web Apps Memcache shim lze použít s libovolnou aplikací komunikuje pomocí protokolu Memcache hello. V tomto konkrétním příkladu je hello referenční aplikací škálovatelný web WordPress, který může být zřízen z Azure Marketplace hello.

Postupujte podle kroků hello uvedených v těchto článcích:

* [Zřízení instance hello Azure Redis Cache Service][0]
* [Nasazení škálovatelného webu WordPress v Azure][1]

Jakmile máte hello škálovatelný web WordPress a instanci služby Redis Cache bude připravená tooproceed s povolením hello Memcache shim ve službě Azure App Service Web Apps.

## Povolit Web Apps Memcache shim hello
V pořadí tooconfigure Memcache shim je nutné vytvořit tři nastavení aplikace. To lze provést pomocí různých metod, včetně hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715), hello [portálu classic][3], hello [rutin prostředí Azure PowerShell] [ 5] nebo hello [rozhraní příkazového řádku Azure][5]. Pro účely hello tohoto příspěvku provedeme toouse hello [portálu Azure] [ 4] tooset nastavení aplikace hello. Hello následující hodnoty lze získat z **nastavení** okno instanci služby Redis Cache.

![Okno nastavení služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### Přidání nastavení aplikace REDIS_HOST
prvním nastavením aplikace, budete potřebovat Hello toocreate je hello **REDIS\_hostitele** nastavení aplikace. Toto nastavení nastaví hello toowhich hello shim předává hello mezipaměti informace o cíli. hodnotu nezbytnou hello aplikace redis_host lze získat z hello Hello **vlastnosti** okno instanci služby Redis Cache.

![Název hostitele služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

Sada hello klíč hello aplikace nastavení příliš**REDIS\_hostitele** a hello hodnoty toohello nastavení aplikace hello **hostname** instance hello Redis Cache.

![Nastavení aplikace REDIS_HOST webové aplikace](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### Přidání nastavení aplikace REDIS_KEY
druhým nastavením aplikace, budete potřebovat Hello toocreate je hello **REDIS\_klíč** nastavení aplikace. Toto nastavení poskytuje hello ověřování tokenu požadované toosecurely přístup hello Redis Cache instanci. Můžete načíst hello hodnotu nezbytnou k nastavení aplikace REDIS_KEY hello hello **přístupové klíče** okno hello instanci služby Redis Cache.

![Primární klíč služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

Sada hello klíč hello aplikace nastavení příliš**REDIS\_klíč** a hello hodnoty toohello nastavení aplikace hello **primární klíč** instance hello Redis Cache.

![Nastavení aplikace REDIS_KEY webu Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### Přidání nastavení aplikace MEMCACHESHIM_REDIS_ENABLE
poslední nastavení aplikace Hello je použité tooenable hello Memcache Shim ve webové aplikace, které používá hello nastavení REDIS_HOST a REDIS_KEY tooconnect toohello Azure Redis Cache a předat dál hello volání mezipaměti. Sada hello klíč hello aplikace nastavení příliš**MEMCACHESHIM\_REDIS\_povolit** a hello hodnota příliš**true**.

![Nastavení aplikace MEMCACHESHIM_REDIS_ENABLE webové aplikace](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

Jakmile dokončíte přidání nastavení aplikace hello tří (3), klikněte na tlačítko **Uložit**.

## Povolení rozšíření Memcache pro PHP
Aby hello aplikace toospeak hello protokolu Memcache je nutné tooinstall hello Memcache rozšíření tooPHP – hello jazykové rozhraní pro web WordPress.

### Stažení rozšíření php_memcache hello
Procházet příliš[PECL][6]. V části hello ukládání do mezipaměti kategorii, klikněte na [memcache][7]. V části hello stahování sloupce, klikněte na odkaz DLL hello.

![Web PHP PECL](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Stáhněte si odkaz Non-Thread Safe (NTS) x86 hello hello verzi PHP povolena ve službě Web Apps. (Výchozí hodnota je PHP 5.4)

![Balíček Memcache webu PHP PECL](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### Povolení rozšíření php_memcache hello
Po stažení souboru hello rozbalte a nahrát hello **php\_memcache.dll** do hello **d:\\domácí\\lokality\\wwwroot\\bin\\ext\\**  adresáře. Po hello php_memcache.dll do webové aplikace hello, musíte tooenable hello rozšíření toohello modulu PHP Runtime. tooenable hello rozšíření Memcache na portálu Azure, otevřete hello hello **nastavení aplikace** hello webové aplikace, pak přidejte nové nastavení aplikace hello klíčem **PHP\_rozšíření** a hello Hodnota **bin\\ext\\php_memcache.dll**.

> [!NOTE]
> Pokud webová aplikace hello musí tooload více rozšíření PHP, hello hodnota php_extensions by měla být čárkami oddělený seznam souborů tooDLL relativní cesty.
> 
> 

![Nastavení aplikace PHP_EXTENSIONS webové aplikace](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

Jakmile budete hotovi, klikněte na možnost **Uložit**.

## Instalace modulu plug-in Memcache WordPress
> [!NOTE]
> Můžete také stáhnout hello [modul plug-in mezipaměti objektů Memcached](https://wordpress.org/plugins/memcached/) z webu WordPress.org.
> 
> 

Na stránce modulů plug-in hello WordPress, klikněte na tlačítko **přidat nové**.

![Stránka modulů plug-in webu WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

Hello vyhledávacího pole zadejte **memcached** a stiskněte klávesu **Enter**.

![Přidání nového modulu plug-in webu WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

Najít **Memcached Object Cache** hello seznamu, pak klikněte na **instalovat nyní**.

![Instalace modulu plug-in Memcache webu WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### Povolit hello modulu plug-in Memcache WordPress
> [!NOTE]
> Postupujte podle pokynů hello v tomto blogu [jak tooenable rozšíření webu ve službě Web Apps] [ 8] tooinstall Visual Studio Team Services.
> 
> 

V hello `wp-config.php` soubor, přidejte následující kód výše hello zastavení úpravy komentář téměř hello konec souboru hello hello.

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

Po vložení tohoto kódu monaco automaticky uloží dokument hello.

dalším krokem Hello je modul plug-in mezipaměti objektů tooenable hello. K tomu je potřeba přetahování **object-cache.php** z **wp obsah/plugins/memcached** složky toohello **wp-content** složky tooenable hello objekt Memcache Funkce mezipaměti.

![Vyhledejte hello plug-in object-cache.php memcache](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

Teď tento hello **object-cache.php** soubor je v hello **wp-content** složky, hello Memcached Object Cache je teď povolené.

![Povolení modulu plug-in object-cache.php memcache hello](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## Ověřte hello modulu plug-in Memcache Object Cache
Všechny hello kroky tooenable hello Web Apps Memcache shim jsou nyní dokončeny. Hello zbývá jen tooverify, instance služby Redis Cache zaplňuje daty hello.

### Povolení podpory portu bez SSL hello ve službě Azure Redis Cache
> [!NOTE]
> V době napsání tohoto článku hello hello rozhraní příkazového řádku Redis nepodporuje připojení SSL, proto hello následující kroky jsou nezbytné.
> 
> 

V hello portálu Azure přejděte instanci toohello Redis Cache, kterou jste vytvořili pro tuto webovou aplikaci. Jakmile se otevře okno hello mezipaměti, klikněte na možnost hello **nastavení** ikonu.

![Tlačítko Nastavení služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

Vyberte **přístupové porty** hello seznamu.

![Přístupový port služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

Klikněte na možnost **Ne** v položce **Povolit přístup jen přes SSL**.

![Přístupový port služby Azure Redis Cache pouze SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

Zobrazí se, že je nyní nastaven port bez SSL hello. Klikněte na **Uložit**.

![Přístupový portál Redis bez SSL služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### Připojit tooAzure Redis Cache z rozhraní příkazového řádku redis
> [!NOTE]
> Tento krok předpokládá, že je ve vývojovém počítači místně nainstalován redis. [Nainstalujte místně Redis podle těchto pokynů][9].
> 
> 

Otevřete konzolu příkazového řádku hello volba a zadejte následující příkaz:

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

Nahraďte hello  **&lt;název hostitele pro redis cache&gt;**  s hello skutečným názvem hostitele xxxxx.redis.cache.windows.net a hello  **&lt;primární key pro redis-cache&gt;**  hello přístupovým klíčem pro mezipaměť hello, stiskněte **Enter**. Po hello CLI má připojení instanci služby Redis Cache toohello vydejte příkaz žádné redis. Na snímku obrazovky hello níže I jste vybrali toolist hello klíče.

![Připojit tooAzure Redis Cache z rozhraní příkazového řádku Redis v terminálu](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

Hello volání toolist hello klíče by měl vrátit hodnotu. Pokud ne, zkuste navigace toohello webové aplikace a zkusit to znovu.

## Závěr
Blahopřejeme! Hello aplikace WordPress má nyní centralizovanou mezipaměť v paměti tooaid zvýšit propustnost. Pamatujte si, že Web Apps Memcache Shim lze použít s libovolným klientem Memcache bez ohledu na programovací jazyk nebo aplikace framework hello. tooprovide zpětnou vazbu nebo tooask dotazy týkající se hello Web Apps Memcache shim, post příliš[fórech MSDN] [ 10] nebo [Stackoverflow][11].

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[0]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: /powershell/azureps-cmdlets-docs
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org
