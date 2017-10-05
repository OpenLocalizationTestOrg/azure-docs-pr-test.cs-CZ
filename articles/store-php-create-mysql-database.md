---
title: "Vytvoření databáze MySQL v Azure a připojení k ní"
description: "Další informace o použití portálu Azure k vytvoření databáze MySQL a potom k němu připojit z webové aplikace PHP v Azure."
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
ms.openlocfilehash: 66f4b7a5f8eb3f6f125c9420b40caffca3d43dd6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Vytvoření databáze MySQL v Azure a připojení k ní
V tomto kurzu se dozvíte, jak k vytvoření databáze MySQL v [portál Azure](https://portal.azure.com) (zprostředkovatel je [ClearDB](http://www.cleardb.com/)) a jak se připojit k němu z webové aplikace PHP spuštěné v [Azure App Service](app-service/app-service-value-prop-what-is.md).

> [!NOTE]
> Můžete také vytvořit databázi MySQL jako součást [šablony aplikace Marketplace](app-service-web/app-service-web-create-web-app-from-marketplace.md).
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Vytvoření databáze MySQL na portálu Azure
K vytvoření databáze MySQL na portálu Azure, postupujte takto:

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. V levé nabídce klikněte na tlačítko **nový** > **Data + úložiště** > **databáze MySQL**.

    ![Vytvoření databáze MySQL v Azure – spuštění](./media/store-php-create-mysql-database/create-db-1-start.png)
3. V nová databáze MySQL [okno](azure-portal-overview.md), nakonfigurujte novou databázi MySQL takto (*okno*: stránky portálu, které se otevře vodorovně):

   * **Název databáze**: Zadejte jedinečné osobní jméno
   * **Předplatné**: Zvolte předplatné použít
   * **Databáze typu**: vyberte **sdílené** pro nízkonákladové nebo volné vrstvy, nebo **Dedicated** získat vyhrazených prostředcích.
   * **Skupina prostředků**: přidejte databáze MySQL na stávající [skupiny prostředků](azure-resource-manager/resource-group-overview.md) nebo ji umístit do nové. Prostředky ve stejné skupině se dají snadno spravovat společně.
   * **Umístění**: Vyberte umístění blízko vás. Při přidávání do existující skupiny prostředků, že je uzamčené umístění skupiny prostředků.
   * **Cenová úroveň**: klikněte na tlačítko **cenová úroveň**, zvolte cenovou možnost (**Mercury** vrstvy je zdarma) a potom klikněte na **vyberte**.
   * **Právní podmínky**: klikněte na tlačítko **právní podmínky**, přečtěte si podrobnosti o nákupu a klikněte na tlačítko **nákupu**.
   * **Připnout na řídicí panel**: vyberte, pokud chcete získat přístup přímo z řídicího panelu. To je obzvláště užitečné, pokud se nevyznáte v portálu navigační ještě.

     Na následujícím snímku obrazovky je právě příklad konfiguraci databáze MySQL.  
     ![Vytvoření databáze MySQL v Azure – konfigurace](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Po dokončení konfigurace, klikněte na tlačítko **vytvořit**.

    ![Vytvoření databáze MySQL v Azure – vytvoření](./media/store-php-create-mysql-database/create-db-3-create.png)

    Zobrazí se automaticky otevírané okno notification informací, víte, že se zahájilo nasazení.

    ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Zobrazí se další automaticky otevírané okno po nasazení proběhla úspěšně. Na portálu také se automaticky otevře okna databáze MySQL.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Připojení k vaší databázi MySQL
Pokud chcete zobrazit informace o připojení pro novou databázi MySQL, stačí kliknout na **vlastnosti** v okně vaší webové aplikace.

![Vytvoření databáze MySQL v Azure – okno databáze MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Teď můžete použít informace o tomto připojení v jakékoli webové aplikaci. Ukázka, která ukazuje, jak používat informace o připojení z jednoduchou aplikaci PHP je k dispozici [zde](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Připojení webové aplikace Laravel (z PHP get kurzu Začínáme)
Předpokládejme, že právě jste dokončili kurz [vytvoření, konfigurace a nasazení webové aplikace PHP do Azure](app-service-web/app-service-web-php-get-started.md) a mít [Laravel](https://www.laravel.com/) webová aplikace spuštěná v Azure. Možnosti databáze můžete snadno přidat do aplikace Laravel. Postupujte podle následujících kroků:

> [!NOTE]
> Následující postup předpokládá, že dokončení tohoto kurzu [vytvoření, konfigurace a nasazení webové aplikace PHP do Azure](app-service-web/app-service-web-php-get-started.md).
>
>

1. Konfigurace aplikace Laravel ve vašem místním vývojovém prostředí tak, aby odkazoval na databázi MySQL. Chcete-li to provést, otevřete `.env` z vaší aplikace Laravel kořenový adresář a nastavit možnosti databáze MySQL.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > V **vlastnosti** okně název vaší databáze MySQL může nebo nemusí být ten ve zobrazeny **název databáze** pole. Je lepší změnami parametr databáze **PŘIPOJOVACÍ řetězec** pole.    
   >
   > ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. Nejrychlejší způsob, jak ověřte, zda máte přístup MySQL teď má používat [Laravel je výchozí ověřování generování uživatelského rozhraní](https://laravel.com/docs/5.2/authentication#authentication-quickstart).
   V terminál příkazového řádku spusťte následující příkazy z kořenového adresáře aplikace Laravel:

         php artisan migrate
         php artisan make:auth

    První příkaz vytvoří tabulky, v závislosti na předdefinované migrace v Azure `database/migrations` directory a v druhém příkazu scaffolds základní zobrazení a trasy pro registraci uživatelů a ověřování.
3. Vývojový server nyní spusťte:

        php artisan serve
4. V prohlížeči přejděte na http://localhost: 8000 a registraci nového uživatele, jak je znázorněno:

    ![Připojení k databázi MySQL v Azure – registrace uživatele](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Po zobrazení výzvy uživatelského rozhraní dokončení registrace. Po dokončení registrace budete přihlášeni.

    ![Připojení k databázi MySQL v Azure – registrace uživatele](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Vyvíjíte aplikace pro databázi MySQL v Azure.
5. Nyní, stačí k replikaci vaší `.env` nastavení vaší webové aplikace Azure. Spusťte následující příkazy rozhraní příkazového řádku Azure:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. V dalším kroku potvrďte a odešlete do Azure místní změny provedené dříve během spuštění `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. Přejděte do webové aplikace Azure.

        azure site browse
8. Přihlaste se pomocí přihlašovacích údajů uživatele, který jste vytvořili dříve.

    ![Připojení k databázi MySQL v Azure – přejděte do webové aplikace Azure](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Po přihlášení, měli byste vidět popisný po přihlašovací obrazovce.

    ![Připojení k databázi MySQL v Azure - přihlášení](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Blahopřejeme, webové aplikace PHP v Azure je nyní přístup k datům z databáze MySQL.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).
