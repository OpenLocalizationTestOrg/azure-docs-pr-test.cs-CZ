---
title: "aaaDeploy aplikace WordPress v hello portálu Azure v pěti minutách | Microsoft Docs"
description: "Zjistěte, jak je snadné toorun webové aplikace ve službě App Service pomocí nasazení aplikace WordPress. Výsledky si můžete okamžitě prohlédnout."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: cephalin
ms.openlocfilehash: 4cd26bbacf89657997847ded1284e472288ddebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-wordpress-app-in-hello-azure-portal-in-five-minutes"></a>Nasazení aplikace WordPress v hello portálu Azure v pěti minutách

Tento kurz ukazuje, jak toodeploy první [WordPress](https://wordpress.org/) webová aplikace příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md) v minutách.

![Web WordPress](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Požadavky
Potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="deploy-hello-wordpress-app"></a>Nasazení aplikace WordPress hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Otevřete [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).

    Tento odkaz je zástupce tooimmediately nakonfigurujte novou aplikaci WordPress v hello portálu Azure.

3. Do pole **Název aplikace** zadejte název webové aplikace. Pokud je v hello jedinečný název hello uvidíte zeleného zaškrtnutí pole hello `azurewebsites.net` domény.
   
5. V **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** toocreate a nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md), zadejte jeho název.

6. V části **Poskytovatel databáze** vyberte **CleaDB**.

7. Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**. Konfigurace hello [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) znázorněné:

    - V **plán služby App Service**, název požadovaného typu hello.
    - V **umístění**, zvolit si plán toohost umístění.
    - Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na tlačítko **OK**.

8. Klikněte na **Databáze** > **Vytvořit novou**. Nakonfigurujte hello SQL Database, jak je znázorněno:

    - Do pole **Název databáze** zadejte požadovaný název. 
    - V **umístění**, zvolte hello stejné umístění jako hello plán služby App Service.
    - Klikněte na **Cenová úroveň** a potom vyberte **Mercury** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na **Právní podmínky** a klikněte na **Koupit**.
    - Klikněte na tlačítko **OK**.

9. Klikněte na možnost **Vytvořit**.

    Azure teď na základě vaší konfigurace vytvoří aplikaci WordPress. Mělo by se zobrazit oznámení **Nasazení začalo...**.

    ![Nasazení začalo – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a správa webové aplikace WordPress

Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.

![Nasazení bylo úspěšné – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Klikněte na tlačítko hello oznámení. Pokud je provedena, je vždy k němu přístup klepnutím na zvonek oznámení hello (![mocí následujících oznámení - první WordPress v Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).

3. V hello horní části stránky přehled hello, klikněte na **Procházet**.
   
    ![Procházet – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    Nyní uvidíte hello WordPress **úvodní** stránky. Instalaci WordPress hello nakonfigurovat a spustit přehrávání s ním!

    ![Konfigurace WordPressu – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Další kroky
* [Vytváření, konfiguraci a nasazení Laravel webové aplikace tooAzure](app-service-web-php-get-started.md) -další základní znalosti hello potřebujete toorun žádné webové aplikace PHP v Azure, jako například:

    * Vytvoření a konfigurace aplikací v Azure z prostředí PowerShell/Bash
    * Nastavení verze PHP
    * Použijte soubor spuštění, který není v kořenovém adresáři aplikace hello.
    * Povolení automatizace nástroje Composer
    * Přístup k proměnným pro konkrétní prostředí
    * Odstraňování běžných chyb

* [Nasazení vaší tooAzure kódu služby App Service](web-sites-deploy.md)– zjistěte, jak řídit toodeploy ze serveru FTP nebo ze zdroje úložiště.
* [Přidání funkce tooyour první webové aplikace](app-service-web-get-started-2.md) -trvat vaše aplikace Azure toohello další úrovně. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí.
