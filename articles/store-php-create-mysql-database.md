---
title: "aaaCreate a připojte tooa databáze MySQL v Azure"
description: "Zjistěte, jak toouse hello Azure portálu toocreate databázi MySQL a potom se připojte tooit z webové aplikace PHP v Azure."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: 3abc02f8887432625666afd847e9dc0c0a4db2e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-connect-tooa-mysql-database-in-azure"></a>Vytvořit a připojit tooa databáze MySQL v Azure
Tento kurz ukazuje, jak databáze toocreate MySQL v hello [portál Azure](https://portal.azure.com) (zprostředkovatel je [ClearDB](http://www.cleardb.com/)) a jak tooconnect tooit z PHP webová aplikace spuštěna [Azure App Service](app-service/app-service-value-prop-what-is.md).

> [!NOTE]
> Můžete také vytvořit databázi MySQL jako součást [šablony aplikace Marketplace](app-service-web/app-service-web-create-web-app-from-marketplace.md).
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Vytvoření databáze MySQL na portálu Azure
toocreate databáze MySQL v hello portál Azure, hello následující:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na **nový** > **Data + úložiště** > **databáze MySQL**.

    ![Vytvoření databáze MySQL v Azure – spuštění](./media/store-php-create-mysql-database/create-db-1-start.png)
3. V hello nová databáze MySQL [okno](azure-portal-overview.md), nakonfigurujte novou databázi MySQL takto (*okno*: stránky portálu, které se otevře vodorovně):

   * **Název databáze**: Zadejte jedinečné osobní jméno
   * **Předplatné**: Zvolte předplatné toouse hello
   * **Databáze typu**: vyberte **sdílené** pro nízkonákladové nebo volné vrstvy, nebo **Dedicated** tooget vyhrazené prostředky.
   * **Skupina prostředků**: Přidat existující tooan databáze MySQL hello [skupiny prostředků](azure-resource-manager/resource-group-overview.md) nebo pro něj nový. Prostředky ve stejné skupině se dají snadno spravovat společně hello.
   * **Umístění**: Vyberte tooyou zavřít umístění. Při přidávání tooan existující skupinu prostředků, jste umístění skupiny prostředků uzamčeném toohello.
   * **Cenová úroveň**: klikněte na tlačítko **cenová úroveň**, zvolte cenovou možnost (**Mercury** vrstvy je zdarma) a potom klikněte na **vyberte**.
   * **Právní podmínky**: klikněte na tlačítko **právní podmínky**, přečtěte si podrobnosti o nákupu hello a klikněte na tlačítko **nákupu**.
   * **PIN kód toodashboard**: vyberte, pokud chcete, aby tooaccess přímo z řídicího panelu hello. To je obzvláště užitečné, pokud se nevyznáte v portálu navigační ještě.

     Následující snímek obrazovky Hello je právě příklad konfiguraci databáze MySQL.  
     ![Vytvoření databáze MySQL v Azure – konfigurace](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Po dokončení konfigurace, klikněte na tlačítko **vytvořit**.

    ![Vytvoření databáze MySQL v Azure – vytvoření](./media/store-php-create-mysql-database/create-db-3-create.png)

    Zobrazí se automaticky otevírané okno notification informací, víte, že se zahájilo nasazení.

    ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Zobrazí se další automaticky otevírané okno po nasazení proběhla úspěšně. portál Hello také se automaticky otevře okna databáze MySQL.

<a name="connect"></a>

## <a name="connect-tooyour-mysql-database"></a>Připojit databáze MySQL tooyour
informace o připojení hello toosee pro novou databázi MySQL, stačí kliknout na **vlastnosti** v okně vaší webové aplikace.

![Vytvoření databáze MySQL v Azure – okno databáze MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Teď můžete použít informace o tomto připojení v jakékoli webové aplikaci. Ukázka, která ukazuje, jak je k dispozici informace o připojení hello toouse z jednoduchou aplikaci PHP [zde](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-hello-php-get-started-tutorial"></a>Připojení webové aplikace Laravel (z hello PHP get kurzu Začínáme)
Předpokládejme, právě dokončení hello kurzu [vytvořit, nakonfigurovat a nasadit tooAzure webové aplikace PHP](app-service-web/app-service-web-php-get-started.md) a mít [Laravel](https://www.laravel.com/) webová aplikace spuštěná v Azure. Můžete snadno přidat databáze možnosti tooyour Laravel aplikace. Postupujte podle následujících kroků hello:

> [!NOTE]
> Hello následující kroky předpokládají, že dokončení kurzu hello [vytvořit, nakonfigurovat a nasadit tooAzure webové aplikace PHP](app-service-web/app-service-web-php-get-started.md).
>
>

1. Konfigurace aplikace hello Laravel v databázi MySQL toohello toopoint místním vývojovém prostředí. toodo tuto, otevřete `.env` z vaší aplikace Laravel kořenového adresáře a nastavit možnosti databáze MySQL hello.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > V hello **vlastnosti** okně hello název vaší databáze MySQL může nebo nemusí být hello jeden zobrazeny v hello **název databáze** pole. Je lepší toocheck hello databáze parametr v hello **PŘIPOJOVACÍ řetězec** pole.    
   >
   > ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. Dobrý den, abyste měli přístup MySQL teď nejrychlejší způsob, jak tooverify je toouse [Laravel je výchozí ověřování generování uživatelského rozhraní](https://laravel.com/docs/5.2/authentication#authentication-quickstart).
   V hello terminál příkazového řádku spusťte následující příkazy z kořenového adresáře aplikace Laravel hello:

         php artisan migrate
         php artisan make:auth

    První příkaz Hello vytváří hello tabulky v Azure podle předdefinovaných migrace v hello `database/migrations` adresář a druhý příkaz hello scaffold hello základní zobrazení a trasy pro registraci uživatelů a ověřování.
3. Nyní spusťte hello vývojového serveru:

        php artisan serve
4. V prohlížeči hello přejděte toohttp://localhost:8000 a registraci nového uživatele, jak je znázorněno:

    ![Připojit databáze tooMySQL ve službě Azure - registraci uživatele](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Postupujte podle hello uživatelského rozhraní výzva hello dokončení registrace. Po dokončení registrace budete přihlášeni.

    ![Připojit databáze tooMySQL ve službě Azure - registraci uživatele](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Vyvíjíte aplikace hello databázi MySQL v Azure.
5. Nyní, stačí tooreplicate vaše `.env` nastavení tooyour webové aplikace Azure. Spusťte následující příkazy příkazového řádku Azure CLI hello:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. V dalším kroku potvrďte a odešlete tooAzure hello místních změn provedených dříve během spuštění `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. Procházejte toohello webové aplikace Azure.

        azure site browse
8. Přihlaste se pomocí přihlašovacích údajů uživatele hello, které jste vytvořili dříve.

    ![Připojit databáze tooMySQL ve službě Azure - procházet tooAzure webové aplikace](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Po přihlášení, měli byste vidět hello popisný po přihlašovací obrazovku.

    ![Připojit databáze tooMySQL ve službě Azure - přihlášení](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Blahopřejeme, webové aplikace PHP v Azure je nyní přístup k datům z databáze MySQL.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).
