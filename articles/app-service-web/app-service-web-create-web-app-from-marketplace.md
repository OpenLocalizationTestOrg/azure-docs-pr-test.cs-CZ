---
title: "Vytvoření webové aplikace v Azure Marketplace | Dokumentace Microsoftu"
description: "Naučte se vytvořit novou webovou aplikaci WordPress v Azure Marketplace pomocí portálu Azure."
services: app-service\web
documentationcenter: 
author: sunbuild
manager: erikre
editor: 
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: sunbuild
ms.custom: mvc
ms.openlocfilehash: 16951ac0fcc350b7176747a7ad4e0bc8e186ab17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Vytvoření webové aplikace v Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností komunit software s otevřeným zdrojem, např. WordPress a Umbraco CMS. V tomto kurzu zjistěte, jak vytvořit aplikaci WordPress z Azure marketplace.
vytváří databázi webové aplikace Azure a databáze MySQL. 

![Řídicí panel příklad WordPress webové aplikace](./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png)

## <a name="before-you-begin"></a>Než začnete 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="deploy-from-azure-marketplace"></a>Nasazení z Azure Marketplace
Použijte následující postup k nasazení WordPress z Azure Marketplace.

### <a name="sign-in-to-azure"></a>Přihlášení k Azure
Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

### <a name="deploy-wordpress-template"></a>Nasazení šablony WordPress
Azure Marketplace obsahuje šablony pro nastavení prostředků, instalační program [WordPress](https://portal.azure.com/#create/WordPress.WordPress) šablonu, abyste mohli začít.
   
Zadejte následující informace k nasazení aplikace WordPress a její prostředky.

  ![WordPress vytvořit toku](./media/app-service-web-create-web-app-from-marketplace/wordpress-portal-create.png)


| Pole         | Navrhovaná hodnota           | Popis  |
| ------------- |-------------------------|-------------|
| Název aplikace.      | mywordpressapp          | Zadejte název jedinečné aplikace pro vaše **název webové aplikace**. Tento název se používá jako součást výchozí název DNS pro vaši aplikaci `<app_name>.azurewebsites.net`, takže ho musí být jedinečný v rámci všech aplikací v Azure. Můžete později namapovat vlastního názvu domény do vaší aplikace ještě před zveřejněním uživatelům. |
| Předplatné  | Průběžné platby             | Vyberte možnost **Předplatné**. Pokud máte více předplatných, vyberte odpovídající předplatné. |
| Skupina prostředků| mywordpressappgroup                 |    Zadejte **skupiny prostředků**. Skupina prostředků je logický kontejner, do které prostředků Azure stejně jako webové aplikace, databáze, které je nasadit a spravovat. Můžete vytvořit skupinu prostředků nebo použít existující |
| Plán služby App Service | myappplan          | Plány aplikační služby představují kolekci fyzické prostředky, které jsou použity k hostování vaší aplikace. Vyberte **umístění** a **cenová úroveň**. Další informace o cenách najdete v tématu [cenová úroveň služby App service](https://azure.microsoft.com/pricing/details/app-service/) |
| Databáze      | mywordpressapp          | Vyberte poskytovatele příslušné databáze pro databázi MySQL. Webové aplikace podporuje **ClearDB**, **Azure Database pro databázi MySQL** a **MySQL v aplikaci**. Další podrobnosti najdete v tématu [konfigurace databáze](#database-config) části níže. |
| Application Insights | ON nebo OFF          | Tato položka je nepovinná. [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) zajišťuje služby monitorování pro webovou aplikaci kliknutím **ON**.|

<a name="database-config"></a>

### <a name="database-configuration"></a>Konfigurace databáze
Postupujte podle následujících kroků na základě vaší volby zprostředkovatel databáze MySQL.  Doporučujeme tuto webovou aplikaci a MySQL databázi být ve stejném umístění.

#### <a name="cleardb"></a>ClearDB 
[ClearDB](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview) je řešení třetí strany pro plně integrovaná služba MySQL v Azure. Chcete-li použít ClearDB databází, je potřeba přidružit platební kartu vaše [účet Azure](http://account.windowsazure.com/subscriptions). Pokud jste vybrali ClearDB poskytovatel databáze, můžete zobrazit seznam existujících databází vybírat nebo kliknutím na tlačítko **vytvořit nový** tlačítko pro vytvoření databáze.

![Vytvoření ClearDB](./media/app-service-web-create-web-app-from-marketplace/mysqldbcreate.png)

#### <a name="azure-database-for-mysql-preview"></a>Azure databáze pro databázi MySQL (Preview)
[Azure databáze pro databázi MySQL](https://azure.microsoft.com/en-us/services/mysql) poskytuje službu, spravované databáze pro vývoj aplikací a nasazení, které vám umožní stát databáze MySQL v minutách a škálování za chodu v cloudu pro většinu vztah důvěryhodnosti. S včetně cenovou modely, získáte všechny funkce, které chcete jako vysoké dostupnosti, zabezpečení a obnovení – součástí v žádné peníze navíc. Klikněte na tlačítko **cenová úroveň** zvolit jinou [cenová úroveň](https://azure.microsoft.com/pricing/details/mysql). Chcete-li použít existující databázi nebo existující server MySQL, použijte existující skupinu prostředků, ve kterém se nachází server. 

![Konfigurace nastavení databáze pro webovou aplikaci](./media/app-service-web-create-web-app-from-marketplace/wordpress-azure-database.PNG)

> [!NOTE]
>  Azure databázi MySQL (Preview) a webové aplikace v systému Linux (Preview) nejsou k dispozici ve všech oblastech. Další informace o [Azure Database pro databázi MySQL (Preview)](https://docs.microsoft.com/en-us/azure/mysql) a [webové aplikace v systému Linux](./app-service-linux-intro.md) omezení. 

#### <a name="mysql-in-app"></a>MySQL v aplikaci
[MySQL v aplikaci](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app) je funkce služby App Service, která umožňuje spuštění MySql nativně na platformě. Základní funkce podporované verze funkce:

- MySQL server spuštěný ve stejné instanci vedle sebe s webovému serveru hostování webu. To zvyšuje výkon aplikace.
- Úložiště jsou sdílena mezi MySQL a souborů vaší webové aplikace. Mějte na paměti, volné a sdílené plány, které můžete narazit, že naše kvótami při použití webu na základě akce, které můžete provést. Podívejte se na [omezení kvóty](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/) pro volné a sdílené plány.
- Můžete zapnout protokolování pomalé dotazu a obecné protokolování pro databázi MySQL. Všimněte si, že může ovlivnit výkon serveru a ne vždy obrátit. Funkce protokolování pomáhá zkoumání problémů s aplikací. 

Další podrobnosti, projděte si to [článku](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/ )

![V aplikaci Správa MySQL](./media/app-service-web-create-web-app-from-marketplace/mysqlinappmanage.PNG)

Průběh můžete sledovat kliknutím na ikonu zvonku v horní části stránky portálu při nasazení aplikace WordPress.    
![Ukazatel průběhu](./media/app-service-web-create-web-app-from-marketplace/deploy-success.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte na web Azure Portal a podívejte se na webovou aplikaci, kterou jste právě vytvořili.

Chcete-li to provést, přihlaste se na adrese [https://portal.azure.com](https://portal.azure.com).

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-list.png)


Dostali jste se do _okna_ vaší webové aplikace (stránka portálu, která se otvírá vodorovně).

Ve výchozím nastavení bude okno vaší webové aplikace obsahovat stránku **Přehled**. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně okna zobrazují další stránky konfigurace, které můžete otevřít.

![Okno App Service na webu Azure Portal](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-detail.png)

Tyto karty v okně zobrazují mnoho skvělých funkcí, které můžete do své webové aplikace přidat. Následující seznam obsahuje jen několik možností:

* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

Dokončete Průvodce instalací WordPress 5 minut do mají aplikace WordPress fungovaly. Podívejte se na [Wordpress dokumentaci](https://codex.WordPress.org/) vyvíjet webové aplikace.

![Průvodce instalací WordPress](./media/app-service-web-create-web-app-from-marketplace/wplanguage.png)

## <a name="configuring-your-app"></a>Konfigurace aplikace 
Správu aplikace WordPress, než je připraven k použití v provozním prostředí je několik kroků. Postupujte podle těchto kroků můžete konfigurovat a spravovat aplikace WordPress:

| Požadovaná akce... | Postup... |
| --- | --- |
| **Nahrát, nebo můžete ukládat velké soubory** |[Modul plug-in WordPress pomocí úložiště objektů Blob](https://wordpress.org/plugins/windows-azure-storage/)|
| **Odesílání e-mailů** |Nákup [sendgrid vám umožňuje](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SendGrid.SendGrid?tab=Overview) e-mailu služby a použít [modulu plug-in WordPress pro používání sendgrid vám umožňuje](https://wordpress.org/plugins/sendgrid-email-delivery-simplified/) jeho konfiguraci|
| **Názvy vlastních domén** |[Konfigurace vlastní domény ve službě Azure App Service](app-service-web-tutorial-custom-domain.md) |
| **PROTOKOL HTTPS** |[Povolit HTTPS pro webové aplikace ve službě Azure App Service](app-service-web-tutorial-custom-ssl.md) |
| **Předprodukční režim ověřování** |[Nastavit pracovní a vývojářů prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)|
| **Monitorování a řešení potíží** |[Povolit protokolování diagnostiky pro webové aplikace v Azure App Service](web-sites-enable-diagnostic-log.md) a [monitorování webové aplikace v Azure App Service](app-service-web-tutorial-monitoring.md) |
| **Nasazení webu** |[Nasazení webové aplikace v Azure App Service](app-service-deploy-local-git.md) |


## <a name="secure-your-app"></a>Zabezpečit vaši aplikaci 
Správu aplikace WordPress, než je připraven k použití v provozním prostředí je několik kroků. Postupujte podle těchto kroků můžete konfigurovat a spravovat aplikace WordPress:

| Požadovaná akce... | Postup... |
| --- | --- |
| **Silné uživatelské jméno a heslo**|  Často změnit heslo. Proveďte není uživatelských jmen použít běžně používané jako *správce* nebo *wordpress* atd. Vynutí všechny WordPress uživatelům používat jedinečných uživatelských jmen a silná hesla. |
| **Vždy aktuální** | Aktuálnost základní WordPress, motivy, moduly plug-in. Použít nejnovější modul runtime PHP, které jsou k dispozici ve službě Azure App service |
| **Aktualizace zabezpečení WordPress klíčů** | Aktualizace [klíč zabezpečení WordPress](https://codex.wordpress.org/Editing_wp-config.php#Security_Keys) ke zlepšení šifrování, které jsou uložené v souborech cookie|

## <a name="improve-performance"></a>Zlepšení výkonu
Výkon v cloudu je dosaženo primárně prostřednictvím ukládání do mezipaměti a Škálováním na více systémů. Měli zvážit, ale paměti, šířky pásma a ostatní atributy hostování webových aplikací.

| Požadovaná akce... | Postup... |
| --- | --- |
| **Popis možností instanci služby App Service** |[Podrobnosti o cenách, včetně možnosti služby App Service úrovně](https://azure.microsoft.com/en-us/pricing/details/app-service/)|
| **Prostředky mezipaměti** |Použití [Azure Redis cache](https://azure.microsoft.com/en-us/services/cache/), nebo jeden z jiné nabídky ukládání do mezipaměti v [úložiště Azure](https://azuremarketplace.microsoft.com) |
| **Škálování aplikace** |Potřebujete škálování [webové aplikace v Azure App Service](web-sites-scale.md) a databáze MySQL. MySQL v aplikaci neobsahuje podporu Škálováním na více systémů, proto zvolte ClearDB nebo Azure Database pro databázi MySQL (Preview). [Škálování Azure databáze pro databázi MySQL (Preview)](https://azure.microsoft.com/en-us/pricing/details/mysql/) nebo pokud používáte [ClearDB vysoké dostupnosti směrování](http://w2.cleardb.net/faqs/) škálování databáze |

## <a name="availability-and-disaster-recovery"></a>Dostupnost a zotavení po havárii
Vysoká dostupnost zahrnuje aspektů zotavení po havárii zachování kontinuity podnikových procesů. Plánování selhání a havárií v cloudu vyžaduje, abyste rychle rozpoznat chyby. Tato řešení pomůže implementovat strategie pro zajištění vysoké dostupnosti.

| Požadovaná akce... | Postup... |
| --- | --- |
| **Lokality Vyrovnávání zatížení** nebo **geo-distribuovat lokalit** |[Směrovat provoz Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) |
| **Zálohování a obnovení** |[Zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md) a [obnovení webové aplikace ve službě Azure App Service](web-sites-restore.md) |

## <a name="next-steps"></a>Další kroky
Další informace o různých funkcích [aplikace služby k vývoji a škálování](/app-service-web/).