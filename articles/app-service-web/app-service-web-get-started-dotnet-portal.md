---
title: "aaaDeploy webové aplikace Umbraco v hello portálu Azure v pěti minutách | Microsoft Docs"
description: "Zjistěte, jak je snadné toorun webové aplikace ve službě App Service pomocí nasazení ukázkové aplikace ASP.NET. Výsledky si můžete okamžitě prohlédnout."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.openlocfilehash: 6da45f99b3043a3684e3d99c14e6443d597b5212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-umbraco-web-app-in-hello-azure-portal-in-five-minutes"></a>Nasazení webové aplikace Umbraco v hello portálu Azure v pěti minutách

Tento kurz vám pomůže nasadit n [Umbraco](https://our.umbraco.org/) webová aplikace příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md) v minutách.

![Aplikace Umbraco](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Požadavky
Potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="deploy-hello-aspnet-app"></a>Nasaďte aplikaci ASP.NET hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Otevřete [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).

    Tento odkaz je zástupce tooimmediately nakonfigurujte novou aplikaci Umbraco v hello portálu Azure.

3. Do pole **Název aplikace** zadejte název webové aplikace. Pokud je v hello jedinečný název hello uvidíte zeleného zaškrtnutí pole hello `azurewebsites.net` domény.
   
5. V **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** toocreate a nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md), zadejte jeho název.

7. Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**. Konfigurace hello [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) znázorněné:

    - V **plán služby App Service**, název požadovaného typu hello.
    - V **umístění**, zvolit si plán toohost umístění.
    - Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na **OK**.

    Konfiguraci Umbraco CMS by teď měl vypadat jako hello následující snímek obrazovky:

    ![Konfigurace probíhá – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Klikněte na **SQL Database** > **Vytvořit novou databázi**. Nakonfigurujte hello SQL Database, jak je znázorněno:

    - Do pole **Název** zadejte název, například **myDB**.
    - Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na **Cílový server** > **Vytvořit nový server**. Hello databázový server nakonfigurujte, jak je znázorněno:

        - Do pole **Název serveru** zadejte název serveru. Pokud je v hello jedinečný název hello uvidíte zeleného zaškrtnutí pole hello `.database.windows.net` domény.
        - V **přihlašovací jméno správce serveru**, typ hello potřeby uživatelské jméno správce.
        - V **heslo** a **potvrzení hesla**, zadejte hello požadované heslo.
        - V umístění vyberte hello stejné umístění, které používáte pro hello webovou aplikaci.
        - Zajistěte, aby **povolit službám azure tooaccess serveru** je vybrána.
        - Klikněte na **Vybrat**.
    
    - Klikněte na **Vybrat**.

13. Klikněte na tlačítko **webové nastavení aplikace**, zadejte hello databáze uživatelské jméno a heslo a klikněte na tlačítko **OK**.

    Konfiguraci Umbraco CMS by teď měl vypadat jako hello následující snímek obrazovky:

    ![Konfigurace dokončena – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Klikněte na možnost **Vytvořit**.
    
    Azure teď na základě vaší konfigurace vytvoří aplikaci Umbraco. Mělo by se zobrazit oznámení **Nasazení začalo...**.

    ![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Spuštění a správa webové aplikace Umbraco

Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.

![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Klikněte na tlačítko hello oznámení. Pokud je provedena, je vždy k němu přístup klepnutím na zvonek oznámení hello (![mocí následujících oznámení - první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).

3. V hello horní části stránky přehled hello, klikněte na **Procházet**.
   
    ![Procházet – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Nyní uvidíte hello Umbraco **úvodní** stránky. Instalaci Umbraco hello nakonfigurovat a spustit přehrávání s ním!

    ![Konfigurace Umbraca – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Další kroky
* [Nasazení aplikace tooAzure web ASP.NET služby App Service pomocí sady Visual Studio](app-service-web-get-started-dotnet.md) – zjistěte, jak toocreate nové webové aplikace Azure ze sady Visual Studio, pomocí některého z hello zahrnuté šablon aplikací.
* [Nasazení vaší tooAzure kódu služby App Service](web-sites-deploy.md)– zjistěte, jak řídit toodeploy ze serveru FTP nebo ze zdroje úložiště.
* [Přidání funkce tooyour první webové aplikace](app-service-web-get-started-2.md) -trvat vaše aplikace Azure toohello další úrovně. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí.
