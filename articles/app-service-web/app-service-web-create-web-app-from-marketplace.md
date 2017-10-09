---
title: "aaaCreate webové aplikace z hello Azure Marketplace | Microsoft Docs"
description: "Zjistěte, jak hello toocreate novou webovou aplikaci WordPress z Azure Marketplace hello pomocí portálu Azure."
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
ms.openlocfilehash: 5ad1ca2f3f7831d857c3e9b02738b6b34acf3649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-from-hello-azure-marketplace"></a>Vytvoření webové aplikace z hello Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Hello Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností komunit software s otevřeným zdrojem, např. WordPress a Umbraco CMS. V tomto kurzu zjistíte, jak toocreate aplikace WordPress z Azure marketplace.
vytváří databázi webové aplikace Azure a databáze MySQL. 

![Řídicí panel příklad WordPress webové aplikace](./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png)

## <a name="before-you-begin"></a>Než začnete 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="deploy-from-azure-marketplace"></a>Nasazení z Azure Marketplace
Postupujte podle kroků hello toodeploy WordPress z Azure Marketplace.

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure
Přihlaste se toohello [portál Azure](https://portal.azure.com).

### <a name="deploy-wordpress-template"></a>Nasazení šablony WordPress
Hello Azure Marketplace obsahuje šablony pro nastavení prostředky, instalační program hello [WordPress](https://portal.azure.com/#create/WordPress.WordPress) šablony tooget spuštěna.
   
Zadejte následující hello aplikace WordPress hello toodeploy informace a její prostředky.

  ![WordPress vytvořit toku](./media/app-service-web-create-web-app-from-marketplace/wordpress-portal-create.png)


| Pole         | Navrhovaná hodnota           | Popis  |
| ------------- |-------------------------|-------------|
| Název aplikace.      | mywordpressapp          | Zadejte název jedinečné aplikace pro vaše **název webové aplikace**. Tento název se používá jako součást hello výchozí název DNS pro vaši aplikaci `<app_name>.azurewebsites.net`, takže je nutné toobe jedinečný mezi všechny aplikace v Azure. Později můžete namapovat k aplikaci tooyour název vlastní domény před vystavit tooyour uživatelé |
| Předplatné  | Průběžné platby             | Vyberte možnost **Předplatné**. Pokud máte více předplatných, vyberte odpovídající předplatné hello. |
| Skupina prostředků| mywordpressappgroup                 |    Zadejte **skupiny prostředků**. Skupina prostředků je logický kontejner, do které prostředků Azure stejně jako webové aplikace, databáze, které je nasadit a spravovat. Můžete vytvořit skupinu prostředků nebo použít existující |
| Plán služby App Service | myappplan          | Plány aplikační služby představují hello kolekce toohost fyzické prostředky, které používá vaše aplikace. Vyberte hello **umístění** a hello **cenová úroveň**. Další informace o cenách najdete v tématu [cenová úroveň služby App service](https://azure.microsoft.com/pricing/details/app-service/) |
| Databáze      | mywordpressapp          | Vyberte poskytovatele hello příslušné databáze pro databázi MySQL. Webové aplikace podporuje **ClearDB**, **Azure Database pro databázi MySQL** a **MySQL v aplikaci**. Další podrobnosti najdete v tématu [konfigurace databáze](#database-config) části níže. |
| Application Insights | ON nebo OFF          | Tato položka je nepovinná. [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) zajišťuje služby monitorování pro webovou aplikaci kliknutím **ON**.|

<a name="database-config"></a>

### <a name="database-configuration"></a>Konfigurace databáze
Postupujte podle kroků hello níže, na základě vaší volby zprostředkovatel databáze MySQL.  Je vhodné, že webová aplikace a databáze MySQL databáze v hello stejné umístění.

#### <a name="cleardb"></a>ClearDB 
[ClearDB](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview) je řešení třetí strany pro plně integrovaná služba MySQL v Azure. V pořadí toouse ClearDB databázích, budete potřebovat tooassociate platební karty tooyour [účet Azure](http://account.windowsazure.com/subscriptions). Pokud jste vybrali ClearDB poskytovatel databáze, můžete zobrazit seznam existujících databází toochoose z nebo klikněte na tlačítko **vytvořit nový** tlačítko toocreate databáze.

![Vytvoření ClearDB](./media/app-service-web-create-web-app-from-marketplace/mysqldbcreate.png)

#### <a name="azure-database-for-mysql-preview"></a>Azure databáze pro databázi MySQL (Preview)
[Azure databáze pro databázi MySQL](https://azure.microsoft.com/en-us/services/mysql) poskytuje službu, spravované databáze pro vývoj aplikací a nasazení, které vám umožní toostand zálohu databáze MySQL v minutách a škálování na hello fyzicky dostavili na hello cloud nejvíce důvěřujete. S včetně cenovou modely, získáte všechny možnosti hello chcete jako vysoké dostupnosti, zabezpečení a obnovení – součástí v žádné peníze navíc. Klikněte na tlačítko **cenová úroveň** toochoose jiné [cenová úroveň](https://azure.microsoft.com/pricing/details/mysql). toouse existující databázi nebo existující MySQL server použít existující skupinu prostředků, ve které hello nachází server. 

![Konfigurace nastavení hello databáze pro webovou aplikaci hello](./media/app-service-web-create-web-app-from-marketplace/wordpress-azure-database.PNG)

> [!NOTE]
>  Azure databázi MySQL (Preview) a webové aplikace v systému Linux (Preview) nejsou k dispozici ve všech oblastech. Další informace o toolearn [Azure Database pro databázi MySQL (Preview)](https://docs.microsoft.com/en-us/azure/mysql) a [webové aplikace v systému Linux](./app-service-linux-intro.md) omezení. 

#### <a name="mysql-in-app"></a>MySQL v aplikaci
[MySQL v aplikaci](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app) je funkce služby App Service, která umožňuje nativně systémem MySql hello platformy. základní funkce Hello podporované verze hello hello funkce:

- MySQL server běžící na stejné instanci node souběžně s váš webový server, který je hostitelem lokality hello hello. To zvyšuje výkon aplikace.
- Úložiště jsou sdílena mezi MySQL a souborů vaší webové aplikace. Mějte na paměti, volné a sdílené plány, které můžete narazit, že naše kvótami při používání hello webového serveru na základě hello akce můžete provést. Podívejte se na [omezení kvóty](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/) pro volné a sdílené plány.
- Můžete zapnout protokolování pomalé dotazu a obecné protokolování pro databázi MySQL. Všimněte si, že to může ovlivnit výkon hello lokality a není vždy obrátit. funkce protokolování Hello pomáhá zkoumání problémů s aplikací. 

Další podrobnosti, projděte si to [článku](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/ )

![V aplikaci Správa MySQL](./media/app-service-web-create-web-app-from-marketplace/mysqlinappmanage.PNG)

Kliknutím na ikonu zvonku hello hello horní části stránky portálu hello při hello, který se nasazuje aplikace WordPress můžete sledovat průběh hello.    
![Ukazatel průběhu](./media/app-service-web-create-web-app-from-marketplace/deploy-success.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte toohello Azure portálu tootake podívejte se na hello webovou aplikaci, kterou jste právě vytvořili.

toodo, přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).

V levé nabídce hello, klikněte na **App Services**, pak klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-list.png)


Dostali jste se do _okna_ vaší webové aplikace (stránka portálu, která se otvírá vodorovně).

Ve výchozím nastavení, zobrazí okno vaší webové aplikace hello **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Hello karty na levé straně okna hello hello ukazuje hello jinou konfiguraci stránky, které můžete otevřít.

![Okno App Service na webu Azure Portal](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-detail.png)

Tyto karty v okně hello zobrazit hello mnoho funkcí můžete přidat tooyour webové aplikace. Hello následující seznam vám poskytuje několik možností, jak hello:

* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

Dokončení hello 5 minut WordPress instalace Průvodce toohave aplikace WordPress fungovaly. Podívejte se na [Wordpress dokumentaci](https://codex.WordPress.org/) toodevelop vaší webové aplikace.

![Průvodce instalací WordPress](./media/app-service-web-create-web-app-from-marketplace/wplanguage.png)

## <a name="configuring-your-app"></a>Konfigurace aplikace 
Správu aplikace WordPress, než je připraven k použití v provozním prostředí je několik kroků. Postupujte podle těchto kroků tooconfigure a spravovat aplikace WordPress:

| toodo to... | Postup... |
| --- | --- |
| **Nahrát, nebo můžete ukládat velké soubory** |[Modul plug-in WordPress pomocí úložiště objektů Blob](https://wordpress.org/plugins/windows-azure-storage/)|
| **Odesílání e-mailů** |Nákup [sendgrid vám umožňuje](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SendGrid.SendGrid?tab=Overview) e-mailové služby a používat hello [modulu plug-in WordPress pro používání sendgrid vám umožňuje](https://wordpress.org/plugins/sendgrid-email-delivery-simplified/) tooconfigure ho|
| **Názvy vlastních domén** |[Konfigurace vlastní domény ve službě Azure App Service](app-service-web-tutorial-custom-domain.md) |
| **PROTOKOL HTTPS** |[Povolit HTTPS pro webové aplikace ve službě Azure App Service](app-service-web-tutorial-custom-ssl.md) |
| **Předprodukční režim ověřování** |[Nastavit pracovní a vývojářů prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)|
| **Monitorování a řešení potíží** |[Povolit protokolování diagnostiky pro webové aplikace v Azure App Service](web-sites-enable-diagnostic-log.md) a [monitorování webové aplikace v Azure App Service](app-service-web-tutorial-monitoring.md) |
| **Nasazení webu** |[Nasazení webové aplikace v Azure App Service](app-service-deploy-local-git.md) |


## <a name="secure-your-app"></a>Zabezpečit vaši aplikaci 
Správu aplikace WordPress, než je připraven k použití v provozním prostředí je několik kroků. Postupujte podle těchto kroků tooconfigure a spravovat aplikace WordPress:

| toodo to... | Postup... |
| --- | --- |
| **Silné uživatelské jméno a heslo**|  Často změnit heslo. Proveďte není uživatelských jmen použít běžně používané jako *správce* nebo *wordpress* atd. Vynutí všech jedinečných uživatelských jmen aplikace WordPress uživatelé toouse a silná hesla. |
| **Vždy aktuální** | Zachovat základní WordPress, motivy, moduly plug-in až toodate. Použít hello nejnovější modulu PHP runtime k dispozici ve službě Azure App service |
| **Aktualizace zabezpečení WordPress klíčů** | Aktualizace [klíč zabezpečení WordPress](https://codex.wordpress.org/Editing_wp-config.php#Security_Keys) tooimprove šifrování uložené v souborech cookie|

## <a name="improve-performance"></a>Zlepšení výkonu
Výkon v cloudu hello je dosaženo primárně prostřednictvím ukládání do mezipaměti a Škálováním na více systémů. Však být za hello paměti, šířky pásma a ostatní atributy hostování webových aplikací.

| toodo to... | Postup... |
| --- | --- |
| **Popis možností instanci služby App Service** |[Podrobnosti o cenách, včetně možnosti služby App Service úrovně](https://azure.microsoft.com/en-us/pricing/details/app-service/)|
| **Prostředky mezipaměti** |Použití [Azure Redis cache](https://azure.microsoft.com/en-us/services/cache/), nebo jeden z jiných ukládání do mezipaměti nabídky v hello hello [úložiště Azure](https://azuremarketplace.microsoft.com) |
| **Škálování aplikace** |Je třeba tooscale [hello webové aplikace v Azure App Service](web-sites-scale.md) a databáze MySQL. MySQL v aplikaci neobsahuje podporu Škálováním na více systémů, proto zvolte ClearDB nebo Azure Database pro databázi MySQL (Preview). [Škálování Azure databáze pro databázi MySQL (Preview)](https://azure.microsoft.com/en-us/pricing/details/mysql/) nebo pokud používáte [ClearDB vysoké dostupnosti směrování](http://w2.cleardb.net/faqs/) tooscale zálohu databáze |

## <a name="availability-and-disaster-recovery"></a>Dostupnost a zotavení po havárii
Vysoká dostupnost zahrnuje hello aspekt obnovení po havárii toomaintain kontinuity podnikových procesů. Plánování selhání a havárií v cloudu hello vyžaduje, abyste toorecognize hello selhání rychle. Tato řešení pomůže implementovat strategie pro zajištění vysoké dostupnosti.

| toodo to... | Postup... |
| --- | --- |
| **Lokality Vyrovnávání zatížení** nebo **geo-distribuovat lokalit** |[Směrovat provoz Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) |
| **Zálohování a obnovení** |[Zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md) a [obnovení webové aplikace ve službě Azure App Service](web-sites-restore.md) |

## <a name="next-steps"></a>Další kroky
Další informace o různých funkcích [toodevelop služby App Service a škálování](/app-service-web/).
