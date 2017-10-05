---
title: "Podnikové třídy WordPress v Azure | Microsoft Docs"
description: "Zjistěte, jak hostitelem lokality podnikových WordPress v Azure App Service"
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 22d68588-2511-4600-8527-c518fede8978
ms.service: app-service-web
ms.devlang: php
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
ms.openlocfilehash: 21281955458a2632d96a91d884cab13803f4d296
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enterprise-class-wordpress-on-azure"></a>Podnikové třídy WordPress v Azure
Aplikační služba Azure poskytuje prostředí škálovatelnou, zabezpečenou a snadno použitelné pro kritické, ve velkém měřítku [WordPress] [ wordpress] lokalit. Microsoft, samotné spouští podnikové weby, jako [Office] [ officeblog] a [Bing] [ bingblog] blogy. Tento článek ukazuje, jak vytvořit a udržovat podnikové třídy, založené na cloudu web WordPress, který může zpracovávat velký objem návštěvníky pomocí funkce Web Apps služby Microsoft Azure App Service.

## <a name="architecture-and-planning"></a>Plánování a architektura
Základní instalace WordPress má pouze dva požadavky:

* **Databáze MySQL**: Tento požadavek je k dispozici prostřednictvím [ClearDB v Azure Marketplace][cdbnstore]. Jako alternativu můžete spravovat vlastní MySQL instalace na virtuálních počítačích Azure pomocí [Windows] [ mysqlwindows] nebo [Linux][mysqllinux].

  > [!NOTE]
  > ClearDB poskytuje několik konfigurací MySQL. Každá konfigurace má jiný výkonové charakteristiky. Najdete v článku [úložiště Azure] [ cdbnstore] informace o nabídky, které jsou k dispozici prostřednictvím Azure Store nebo přímo, jak je vidět na [ClearDB webu](http://www.cleardb.com/pricing.view).
  >
  >
* **PHP 5.2.4 nebo větší**: Služba Azure App Service aktuálně poskytuje [verze PHP 5.4, 5.5 a 5.6][phpwebsite].

  > [!NOTE]
  > Doporučujeme, abyste tak, že máte nejnovější opravy zabezpečení vždy používat nejnovější verzi PHP.
  >
  >

### <a name="basic-deployment"></a>Základní nasazení
Pokud používáte pouze základní požadavky, můžete vytvořit základní řešení v oblasti Azure.

![Webové aplikace Azure a databáze MySQL hostované v jedné oblasti Azure][basic-diagram]

I když to by vám umožní vytvořit více instancí webové aplikace lokality tak, aby horizontální navýšení kapacity aplikace, vše, co je hostován v datových centrech v určité geografické oblasti. Návštěvníky z mimo této oblasti mohou se zobrazit pomalé odezvy při použití webu. Pokud datových centrech v této oblasti, přestanou fungovat, takže nemá vaší aplikace.

### <a name="multi-region-deployment"></a>Nasazení s více oblast
Pomocí služby Azure [Traffic Manager][trafficmanager], můžete škálovat web WordPress v různých geografických oblastech a zadejte stejnou adresu URL pro všechny návštěvníky. Všechny návštěvníky přichází prostřednictvím Traffic Manager a poté jsou směrovány do oblasti, která je založená na konfiguraci služby Vyrovnávání zatížení.

![Webové aplikace Azure, hostované v několika oblastech, pomocí směrovače CDBR vysokou dostupnost pro směrování do MySQL v oblastech][multi-region-diagram]

V každé oblasti Web WordPress by stále škálovat napříč více instancemi webové aplikace, ale tato změna měřítka je specifický pro oblast. Vysokým provozem oblasti a nízkým provozem oblastí je možné rozšířit jinak.

Pokud chcete replikovat a směrovat provoz do více databází MySQL, můžete použít [ClearDB vysokou dostupnost směrovače (CDBRs)] [ cleardbscale] (viz levém) nebo [MySQL clusteru poskytovatel úrovni Edition (CGE)][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Nasazení s více oblasti s média úložiště a ukládání do mezipaměti
Pokud server přijímá nahrávání nebo hostitelů mediální soubory, použijte úložiště objektů Blob Azure. Pokud potřebujete ukládání do mezipaměti, vezměte v úvahu [Redis cache][rediscache], [Memcache cloudu](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/), nebo jeden z jiné nabídky ukládání do mezipaměti v [úložiště Azure](http://azure.microsoft.com/gallery/store/).

![Webové aplikace Azure, hostované v několika oblastech, pomocí směrovače CDBR vysoké dostupnosti pro databázi MySQL, spravované mezipaměti, úložiště objektů Blob a Content Delivery Network][performance-diagram]

Úložiště objektů blob je zeměpisné polohy v oblastech, ve výchozím nastavení, takže nemusíte si dělat starosti o replikaci souborů mezi všemi lokalitami. Můžete také povolit Azure [Content Delivery Network] [ cdn] pro úložiště objektů Blob, který distribuuje soubory na konec uzlů, které jsou blíž oblasti návštěvnících.

### <a name="planning"></a>Plánování
#### <a name="additional-requirements"></a>Další požadavky
| Požadovaná akce... | Postup... |
| --- | --- |
| **Nahrát, nebo můžete ukládat velké soubory** |[Modul plug-in WordPress pomocí úložiště objektů Blob][storageplugin] |
| **Odesílání e-mailů** |[Sendgrid vám umožňuje] [ storesendgrid] a [WordPress modulu plug-in pro používání sendgrid vám umožňuje][sendgridplugin] |
| **Názvy vlastních domén** |[Konfigurace vlastního názvu domény v Azure App Service][customdomain] |
| **PROTOKOL HTTPS** |[Povolit HTTPS pro webové aplikace ve službě Azure App Service][httpscustomdomain] |
| **Předprodukční režim ověřování** |[Nastavení přípravných prostředí pro webové aplikace v Azure App Service][staging] <p>Když přesouváte webovou aplikaci z pracovní databáze do produkčního prostředí, je také přesunout konfigurace WordPress. Ujistěte se, že všechna nastavení jsou aktualizovány požadavky pro vaše produkční aplikace před přesunutím dvoufázové instalace aplikace do produkčního prostředí.</p> |
| **Monitorování a řešení potíží** |[Povolit protokolování diagnostiky pro webové aplikace v Azure App Service] [ log] a [monitorování webové aplikace v Azure App Service][monitor] |
| **Nasazení webu** |[Nasazení webové aplikace v Azure App Service][deploy] |

#### <a name="availability-and-disaster-recovery"></a>Dostupnost a zotavení po havárii
| Požadovaná akce... | Postup... |
| --- | --- |
| **Lokality Vyrovnávání zatížení** nebo **geo-distribuovat lokalit** |[Směrovat provoz Azure Traffic Manager][trafficmanager] |
| **Zálohování a obnovení** |[Zálohování webové aplikace ve službě Azure App Service] [ backup] a [obnovení webové aplikace ve službě Azure App Service][restore] |

#### <a name="performance"></a>Výkon
Výkon v cloudu je dosaženo primárně prostřednictvím ukládání do mezipaměti a Škálováním na více systémů. Měli zvážit, ale paměti, šířky pásma a ostatní atributy hostování webových aplikací.

| Požadovaná akce... | Postup... |
| --- | --- |
| **Popis možností instanci služby App Service** |[Podrobnosti o cenách, včetně možnosti služby App Service úrovně][websitepricing] |
| **Prostředky mezipaměti** |[Mezipaměť redis][rediscache], [Memcache cloudu](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/), nebo jeden z jiné nabídky ukládání do mezipaměti v [úložiště Azure](/gallery/store/) |
| **Škálování aplikace** |[Škálování webové aplikace v Azure App Service] [ websitescale] a [ClearDB vysoké dostupnosti směrování][cleardbscale]. Pokud zvolíte možnost hostitele a spravovat vlastní instalaci MySQL, měli byste zvážit [MySQL Cluster CGE] [ cge] pro Škálováním na více systémů. |

#### <a name="migration"></a>Migrace
Existují dvě metody k migraci existujícího webu WordPress do služby Azure App Service:

* **[WordPress export][export]**: Tato metoda exportuje obsah blogu. Pak můžete importovat obsah k novému webu WordPress v Azure App Service pomocí [modulu plug-in – WordPress Importér][import].

  > [!NOTE]
  > Během tohoto procesu můžete migrovat obsah, nepřenese nějaké moduly plug-in, motivy nebo jiných úprav. Tyto součásti musí nainstalovat znovu ručně.
  >
  >
* **Ruční migrace**: [zálohování vaší lokality] [ wordpressbackup] a [databáze][wordpressdbbackup]a pak ručně obnovit do webové aplikace v Azure App Service a přidružené databáze MySQL. Tato metoda je užitečná k migraci vysoce přizpůsobené weby, protože při ní nedochází nebylo nutné pracně ruční instalaci modulů plug-in, motivů a jiných úprav.

## <a name="step-by-step-instructions"></a>Podrobné pokyny
### <a name="create-a-wordpress-site"></a>Vytvoření webu WordPress
1. Použití [Azure Marketplace] [ cdbnstore] k vytvoření databáze MySQL velikosti, který jste určili v [plánování a architektura](#planning) část v oblasti nebo oblasti, kde bude hostitelem vaší lokality.
2. Postupujte podle kroků v [vytvořit webovou aplikaci WordPress v Azure App Service] [ createwordpress] k vytvoření webové aplikace WordPress. Když vytvoříte webovou aplikaci, vyberte **použít existující databázi MySQL**a potom vyberte databázi, kterou jste vytvořili v kroku 1.

Pokud provádíte migraci existujícího webu WordPress, přečtěte si téma [migrovat existující web WordPress do Azure](#Migrate-an-existing-WordPress-site-to-Azure) po vytvoření nové webové aplikace.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migrovat existující web WordPress do Azure
Jak je uvedeno v [plánování a architektura](#planning) část, existují dva způsoby, jak migrovat web WordPress:

* **Pomocí exportu a importu** pro lokality, který není k dispozici mnoho přizpůsobení nebo kde chcete přesunout obsah.
* **Použití zálohování a obnovení** pro lokalit, které mají spoustu přizpůsobení, ve které chcete přesunout vše.

K migraci vaší lokality použijte jeden z těchto částí.

#### <a name="the-export-and-import-method"></a>Metoda export a import
1. Použití [exportovat WordPress] [ export] export existující lokalitu.
2. Vytvoření webové aplikace pomocí kroků v [vytvořit web WordPress](#Create-a-new-WordPress-site) části.
3. Přihlaste se na váš web WordPress [portál Azure][mgmtportal]a potom klikněte na **modulů plug-in** > **přidat nové**. Vyhledat a nainstalovat **– WordPress Importér** modulu plug-in.
4. Po instalaci modulu plug-in – WordPress Importér, klikněte na tlačítko **nástroje** > **Import**a potom klikněte na **WordPress** používat modul plug-in WordPress program pro import.
5. Na **Import WordPress** klikněte na tlačítko **zvolit soubor**. Najít soubor WXR, které se exportovaly z existující web WordPress a potom klikněte na **souboru k odeslání a import**.
6. Klikněte na tlačítko **odeslání**. Zobrazí se výzva, že import proběhl úspěšně.
7. Po dokončení těchto kroků, restartujte váš web z jeho **App Services** v okně [portál Azure][mgmtportal].

Po importu webu může musíte provést následující kroky, kterými povolí nastavení, které nejsou v souboru importu.

| Pokud jste používali to... | To... |
| --- | --- |
| **Trvalých** |Z řídicího panelu WordPress nové lokality, klikněte na tlačítko **nastavení** > **trvalých**a pak aktualizujte trvalých strukturu. |
| **odkazy na bitovou kopii nebo média** |Chcete-li aktualizovat odkazy do nového umístění, použijte [modulu plug-in paroží modré aktualizovat adresy URL][velvet], Hledat a nahradit nástroj, nebo ručně aktualizovat odkazy v databázi. |
| **Motivy** |Přejděte na **vzhled** > **motiv**a podle potřeby aktualizujte motiv webu. |
| **Nabídky** |Pokud vaše motiv podporuje nabídky, odkazy na domovskou stránku mohou mít i nadále staré podadresáři vložených v nich. Přejděte na **vzhled** > **nabídky**a provede jejich aktualizaci. |

#### <a name="the-backup-and-restore-method"></a>Metoda zálohování a obnovení
1. Proveďte zálohu vaší existující WordPress lokality pomocí informací uvedených v [WordPress zálohy][wordpressbackup].
2. Proveďte zálohu existující databáze pomocí informací uvedených v [zálohování databáze][wordpressdbbackup].
3. Vytvoření databáze a obnovení zálohy.

   1. Zakoupení nové databáze z [Azure Marketplace][cdbnstore], nebo nastavit databáze MySQL na [Windows] [ mysqlwindows] nebo [Linux] [ mysqllinux] virtuálního počítače.
   2. Používání databáze MySQL klienta jako [MySQL Workbench] [ workbench] pro připojení k nové databázi a import WordPress databáze.
   3. Změnit doménu položky k nové doméně služby Azure App Service, například mywordpress.azurewebsites.net má databáze aktualizujte. Použití [vyhledávání a nahrazení pro skript databáze WordPress] [ searchandreplace] bezpečně změnit všechny instance.
4. Vytvořit webovou aplikaci na portálu Azure a publikujte WordPress zálohování.

   1. K vytvoření webové aplikace, který má v databázi, [portál Azure][mgmtportal], klikněte na tlačítko **nový** > **Web + mobilní** > **Azure Marketplace** > **webové aplikace** > **Web app + SQL** (nebo **Web app + MySQL**) > **vytvořit**. Nakonfigurujte všechna požadovaná nastavení na vytvořit prázdnou webovou aplikaci.
   2. V záloze WordPress, vyhledejte **wp-config.php** souboru a otevřete v editoru. Nahraďte informace pro novou databázi MySQL následující položky:

      * **%{Db_name/**: uživatelské jméno databáze.
      * **DB_USER**: uživatelské jméno pro přístup k databázi.
      * **DB_PASSWORD**: heslo uživatele.

        Po provedení změny tyto položky, uložte a zavřete **wp-config.php** souboru.
   3. Použít [nasazení webové aplikace v Azure App Service] [ deploy] informace, které umožňují metodu nasazení, kterou chcete použít a pak nasadit WordPress zálohování do vaší webové aplikace v Azure App Service.
5. Po nasazení webu WordPress, byste měli mít přístup k nové lokalitě (jako webová aplikace služby App Service) pomocí *. azurewebsite.net adresa URL webu.

### <a name="configure-your-site"></a>Konfigurace webu
Po web WordPress vytvořila nebo migrovat, použijte následující informace pro zlepšení výkonu nebo povolení dalších funkcí.

| Požadovaná akce... | Postup... |
| --- | --- |
| **Nastavit režim plán služby App Service, velikosti a povolit škálování** |[Škálování webové aplikace v Azure App Service][websitescale]. |
| **Povolit připojení trvalé databáze** |Ve výchozím nastavení nepoužívá WordPress trvalé databázových připojení, které by mohly způsobit připojení k databázi a stát omezeny po více připojení. Chcete-li trvalé připojení, nainstalujte [modulu plug-in trvalé připojení adaptéru](https://wordpress.org/plugins/persistent-database-connection-updater/installation/). |
| **Zlepšení výkonu** |<ul><li><p><a href="https://azure.microsoft.com/en-us/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/">Zakázání souboru cookie směrování žádostí na aplikace</a>, což může zlepšit výkon při spuštění WordPress na několik instancí webové aplikace.</p></li><li><p>Povolte ukládání do mezipaměti. Můžete použít <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis cache</a> (preview) <a href="https://wordpress.org/plugins/redis-object-cache/">modulu plug-in WordPress objekt mezipaměti Redis</a>, nebo můžete použít jednu z dalších ukládání do mezipaměti nabídky z <a href="/gallery/store/">úložiště Azure</a>.</p></li><li><p>[Ujistěte se, WordPress rychlejší s Wincache](https://wordpress.org/plugins/w3-total-cache/). Wincache je povoleno ve výchozím nastavení pro webové aplikace. Při použití WinCache a dynamické mezipaměti společně, vypnout na WinCache souborové mezipaměti, ale nechte uživatele a mezipaměť relace na povoleno. Chcete-li vypnout souborové mezipaměti v souboru .ini úrovni systému, nastavte následující hodnotu:<br/><code>wincache.fcenabled = 0</code></p></li><li><p>[Škálování webové aplikace v Azure App Service] [ websitescale] a používat <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB vysoké dostupnosti směrování</a> nebo <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a>.</p></li></ul> |
| **Použití objektů blob pro úložiště** |<ol><li><p>[Vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md).</p></li><li><p>Další informace jak [používat obsahu distribuční síť](../cdn/cdn-create-new-endpoint.md) do geograficky-distribuovat data se ukládají do objektů BLOB.</p></li><li><p>Instalace a konfigurace <a href="https://wordpress.org/plugins/windows-azure-storage/">úložiště Azure pro modulu plug-in WordPress</a>.</p><p>Podrobné nastavení a informace o konfiguraci pro modul plug-in, najdete v článku <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">uživatelská příručka</a>.</p> </li></ol> |
| **Povolení e-mailu** |Povolit <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">sendgrid vám umožňuje</a> pomocí úložiště Azure. Nainstalujte <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">modulu plug-in sendgrid vám umožňuje</a> WordPress. |
| **Konfigurace vlastního názvu domény** |[Konfigurace vlastního názvu domény v Azure App Service][customdomain]. |
| **Povolit HTTPS pro vlastní název domény** |[Povolit HTTPS pro webové aplikace ve službě Azure App Service][httpscustomdomain]. |
| **Načíst zůstatek nebo geo-distribuovat vaší lokality** |[Směrování provozu Azure Traffic Manager][trafficmanager]. Pokud používáte vlastní doménu, najdete v části [konfigurace vlastního názvu domény v Azure App Service] [ customdomain] informace o tom, jak pomocí služby Traffic Manager pomocí vlastních názvů domén. |
| **Povolit automatické zálohování** |[Zálohování webové aplikace ve službě Azure App Service][backup]. |
| **Povolit protokolování diagnostiky** |[Povolit protokolování diagnostiky pro webové aplikace v Azure App Service][log]. |

## <a name="next-steps"></a>Další kroky
* [Optimalizace WordPress](http://codex.wordpress.org/WordPress_Optimization)
* [Převést WordPress ve více lokalitách ve službě Azure App Service](web-sites-php-convert-wordpress-multisite.md)
* [ClearDB upgrade Průvodce pro Azure.](http://www.cleardb.com/store/azure/upgrade)
* [Hostování WordPress v podsložce vaší webové aplikace v Azure App Service](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)
* [Postup: Vytvoření webu WordPress pomocí Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)
* [Hostování existující blogu WordPress v Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)
* [Povolení velmi trvalých v WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)
* [Jak migrace a spustit blogu WordPress v Azure App Service](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)
* [Jak spustit WordPress v Azure App Service zdarma](http://architects.dzone.com/articles/how-run-wordpress-azure)
* [WordPress v Azure za dvě minuty nebo méně](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)
* [Přesunutí blogu WordPress k Azure – část 1: vytvoření blogu WordPress v Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)
* [Přesunutí blogu WordPress k Azure – část 2: přenos vašeho obsahu](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)
* [Přesunutí blogu WordPress k Azure – část 3: nastavení vaši vlastní doménu.](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)
* [Přesunutí blogu WordPress k Azure – část 4: velmi trvalých a pravidel přepisování adres URL](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)
* [Přesunutí blogu WordPress k Azure – část 5: Přesun z podsložky do kořenového adresáře](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)
* [Jak vytvořit webovou aplikaci WordPress v účtu Azure](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)
* [Propping až WordPress v Azure](http://www.johnpapa.net/wordpress-on-azure/)
* [Tipy pro WordPress v Azure](http://www.johnpapa.net/azurecleardbmysql/)

> [!NOTE]
> Pokud chcete začít pracovat s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Jsou požadovány nevyžaduje se žádná platební karta a bez jakýchkoli závazků.
>
>

## <a name="whats-changed"></a>Co se změnilo
Průvodce změnou z webů na službu App Service, najdete v části [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: app-service-web-tutorial-custom-domain.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: app-service-web-tutorial-custom-ssl.md
[mysqlwindows]:../virtual-machines/windows/classic/mysql-2008r2.md
[mysqllinux]:../virtual-machines/linux/classic/mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: /powershell/azureps-cmdlets-docs
[Azure CLI]:../cli-install-nodejs.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
