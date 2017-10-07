---
title: "aaaCreate webové aplikace WordPress v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak toocreate nový Azure webové aplikace pro blog WordPress pomocí portálu Azure hello."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 3a95fcb6732c15a8200921ce474b6dde2298dec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a>Vytvoření webové aplikace WordPress ve službě Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tento kurz ukazuje, jak lokality toodeploy blogu WordPress z Azure Marketplace hello.

Když jste hotovi s hello kurzu budete mít vlastní funkční web blogu WordPress a spouštění v cloudu hello.

![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Naučíte se:

* Jak toofind šablony aplikace v hello Azure Marketplace.
* Jak toocreate webové aplikace v Azure aplikaci služby, který je založený na šabloně hello.
* Jak tooconfigure nastavení služby Azure App Service pro hello nové webové aplikace a databáze.

Hello Azure Marketplace zpřístupní širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru s otevřeným zdrojem. Hello webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako například [PHP](/develop/nodejs/) v tomto příkladu aplikace WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), a [Python](/develop/python/), tooname a několika. toocreate webové aplikace z Azure Marketplace hello pouze softwaru je nutné je hello prohlížeče, který používáte pro hello hello [portálu Azure](https://portal.azure.com/). 

Hello web WordPress, který nasazujete v tomto kurzu využívá hello databázi MySQL. Pokud chcete použít tooinstead databázi SQL pro databázi hello, najdete v části [Project Nami](http://projectnami.org/). **Projekt Nami** je také k dispozici prostřednictvím hello Marketplace.

> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Pokud chcete, aby tooget začít s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/). Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a>Výběr aplikace WordPress a konfigurace pro službu Azure App Service
1. Přihlaste se toohello [portálu Azure](https://portal.azure.com/).
2. Klikněte na možnost **Nové**.
   
    ![Vytvořit nové][5]
3. Vyhledejte **WordPress** a klikněte na možnost **WordPress**. Pokud chcete toouse SQL Database namísto MySQL, vyhledejte **Project Nami**.
   
    ![WordPress ze seznamu][7]
4. Po přečtení hello popis aplikace WordPress hello, klikněte na tlačítko **vytvořit**.
   
    ![Vytvořit](./media/web-sites-php-web-site-gallery/create.png)
5. Zadejte název webové aplikace hello v hello **webové aplikace** pole.
   
    Tento název musí být jedinečný v doméně azurewebsites.net hello, protože adresa URL hello hello webové aplikace bude {name}. azurewebsites.net. Pokud není jedinečný název hello, kterou zadáte, se zobrazí červený vykřičník hello textového pole.
6. Pokud máte více než jedno předplatné, zvolte hello, kterou chcete toouse. 
7. Vyberte **skupinu prostředků** nebo vytvořte novou.
   
    Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
8. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.
   
    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).    
9. Klikněte na tlačítko **databáze**a potom v hello **nová databáze MySQL** okno Zadejte hello požadované hodnoty pro konfiguraci databáze MySQL.
   
    a. Zadejte nový název nebo ponechte výchozí název hello.
   
    b. Nechte hello **typ databáze** nastavit příliš**sdílené**.
   
    c. Zvolte hello, které stejné umístění jako hello ten, který jste zvolili pro webovou aplikaci hello.
   
    d. Zvolte cenovou úroveň. Mercury (bezplatná úroveň s minimálním počtem povolených připojení a místem na disku) je vhodná pro tento kurz.
10. V hello **nová databáze MySQL** okně klikněte na tlačítko **OK**. 
11. V hello **WordPress** přijměte právní podmínky hello a pak klikněte na tlačítko **vytvořit**. 
    
     ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/configure.png)
    
     Aplikační služba Azure vytvoří hello webové aplikace, obvykle v méně než minutu. Kliknutím na ikonu zvonku hello hello horní části stránky portálu hello můžete sledovat průběh hello.
    
     ![Ukazatel průběhu](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a správa webové aplikace WordPress
1. Po dokončení vytvoření webové aplikace hello přejděte v hello portálu Azure toohello skupině prostředků ve které jste vytvořili hello aplikace, a uvidíte hello webovou aplikaci a databázi hello.
   
    Hello dodatečný prostředek s ikonou žárovky hello je [Application Insights](/services/application-insights/), která zajišťuje služby monitorování pro webovou aplikaci.
2. V hello **skupiny prostředků** okně klikněte na řádek webové aplikace hello.
   
    ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. V okně webové aplikace hello, klikněte na **Procházet**.
   
    ![adresa URL webu][browse]
4. V hello WordPress **úvodní** zadejte informace o konfiguraci hello vyžadované aplikací WordPress a pak klikněte na tlačítko **instalovat WordPress**.
   
    ![Konfigurace WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. Přihlaste se pomocí přihlašovacích údajů hello jste vytvořili na hello **úvodní** stránky.  
6. Otevře se stránka řídicího panelu webu.    
   
    ![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a>Další kroky
Už víte, jak toocreate a nasazení webové aplikace PHP z Galerie hello. Další informace o použití PHP v Azure najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).

Další informace o tom, toowork s App Service Web Apps, najdete v části hello odkazy na levé straně stránky hello (pro široké okno prohlížeče) hello nebo hello horní části stránky hello (pro úzké okno prohlížeče). 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby, najdete v části [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
