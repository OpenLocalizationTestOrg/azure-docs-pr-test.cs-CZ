---
title: "Připojit tooAzure databáze pro databázi MySQL z databáze MySQL Workbench | Microsoft Docs"
description: "Tento rychlý start poskytuje hello kroky toouse MySQL Workbench tooconnect a dotaz data z databáze Azure pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql-database
ms.custom: mvc
ms.topic: article
ms.date: 08/23/2017
ms.openlocfilehash: c64fcb9bb99ba06aa3a95eec420d5d5ef4a31d14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-tooconnect-and-query-data"></a><span data-ttu-id="badb5-103">Azure databáze pro databázi MySQL: MySQL Workbench použití tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="badb5-103">Azure Database for MySQL: Use MySQL Workbench tooconnect and query data</span></span>
<span data-ttu-id="badb5-104">Tento rychlý start předvádí jak tooconnect tooan databáze Azure pro používání MySQL hello MySQL Workbench aplikace.</span><span class="sxs-lookup"><span data-stu-id="badb5-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using hello MySQL Workbench application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="badb5-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="badb5-105">Prerequisites</span></span>
<span data-ttu-id="badb5-106">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="badb5-106">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="badb5-107">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="badb5-107">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="badb5-108">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="badb5-108">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a><span data-ttu-id="badb5-109">Nainstalujte MySQL Workbench</span><span class="sxs-lookup"><span data-stu-id="badb5-109">Install MySQL Workbench</span></span>
<span data-ttu-id="badb5-110">Stáhněte a nainstalujte MySQL Workbench na vašem počítače od [hello MySQL webu](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="badb5-110">Download and install MySQL Workbench on your computer from [hello MySQL website](https://dev.mysql.com/downloads/workbench/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="badb5-111">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="badb5-111">Get connection information</span></span>
<span data-ttu-id="badb5-112">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="badb5-112">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="badb5-113">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="badb5-113">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="badb5-114">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="badb5-114">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="badb5-115">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="badb5-115">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **myserver4demo**.</span></span>

3. <span data-ttu-id="badb5-116">Klikněte na název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="badb5-116">Click hello server name.</span></span>

4. <span data-ttu-id="badb5-117">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="badb5-117">Select hello server's **Properties** page.</span></span> <span data-ttu-id="badb5-118">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="badb5-118">Make a note of hello **Server name** and **Server admin login name**.</span></span>

 ![Databáze Azure pro název serveru MySQL](./media/connect-workbench/1-server-properties-name-login.png)
 
5. <span data-ttu-id="badb5-120">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="badb5-120">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-toohello-server-using-mysql-workbench"></a><span data-ttu-id="badb5-121">Připojení serveru toohello pomocí MySQL Workbench</span><span class="sxs-lookup"><span data-stu-id="badb5-121">Connect toohello server using MySQL Workbench</span></span> 
<span data-ttu-id="badb5-122">server databáze MySQL tooAzure tooconnect pomocí hello grafického uživatelského rozhraní nástroje MySQL Workbench:</span><span class="sxs-lookup"><span data-stu-id="badb5-122">tooconnect tooAzure MySQL server using hello GUI tool MySQL Workbench:</span></span>

1.  <span data-ttu-id="badb5-123">Spusťte hello MySQL Workbench aplikace ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="badb5-123">Launch hello MySQL Workbench application on your computer.</span></span> 

2.  <span data-ttu-id="badb5-124">V **nastavit připojení k nové** dialogovém okně zadejte následující informace na hello hello **parametry** karty:</span><span class="sxs-lookup"><span data-stu-id="badb5-124">In **Setup New Connection** dialog box, enter hello following information on hello **Parameters** tab:</span></span>

    ![nastavení nového připojení](./media/connect-workbench/2-setup-new-connection.png)

    | <span data-ttu-id="badb5-126">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="badb5-126">**Setting**</span></span> | <span data-ttu-id="badb5-127">**Navrhovaná hodnota**</span><span class="sxs-lookup"><span data-stu-id="badb5-127">**Suggested value**</span></span> | <span data-ttu-id="badb5-128">**Popis pole**</span><span class="sxs-lookup"><span data-stu-id="badb5-128">**Field description**</span></span> |
    |---|---|---|
    |   <span data-ttu-id="badb5-129">Název připojení</span><span class="sxs-lookup"><span data-stu-id="badb5-129">Connection Name</span></span> | <span data-ttu-id="badb5-130">Ukázkové připojení</span><span class="sxs-lookup"><span data-stu-id="badb5-130">Demo Connection</span></span> | <span data-ttu-id="badb5-131">Zadejte popisek pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="badb5-131">Specify a label for this connection.</span></span> |
    | <span data-ttu-id="badb5-132">Způsob připojení</span><span class="sxs-lookup"><span data-stu-id="badb5-132">Connection Method</span></span> | <span data-ttu-id="badb5-133">Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="badb5-133">Standard (TCP/IP)</span></span> | <span data-ttu-id="badb5-134">Standard (TCP/IP) je dostačující.</span><span class="sxs-lookup"><span data-stu-id="badb5-134">Standard (TCP/IP) is sufficient.</span></span> |
    | <span data-ttu-id="badb5-135">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="badb5-135">Hostname</span></span> | <span data-ttu-id="badb5-136">*název serveru*</span><span class="sxs-lookup"><span data-stu-id="badb5-136">*server name*</span></span> | <span data-ttu-id="badb5-137">Zadejte hello hodnota názvu serveru, která byla použita při dříve vytvořili hello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="badb5-137">Specify hello server name value that was used when you created hello Azure Database for MySQL earlier.</span></span> <span data-ttu-id="badb5-138">Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použití hello plně kvalifikovaný název domény (\*. mysql.database.azure.com) jako v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="badb5-138">Our example server shown is myserver4demo.mysql.database.azure.com. Use hello fully qualified domain name (\*.mysql.database.azure.com) as shown in hello example.</span></span> <span data-ttu-id="badb5-139">Pokud si nepamatujete název serveru, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="badb5-139">Follow hello steps in hello previous section tooget hello connection information if you do not remember your server name.</span></span>  |
    | <span data-ttu-id="badb5-140">Port</span><span class="sxs-lookup"><span data-stu-id="badb5-140">Port</span></span> | <span data-ttu-id="badb5-141">3306</span><span class="sxs-lookup"><span data-stu-id="badb5-141">3306</span></span> | <span data-ttu-id="badb5-142">Vždy používejte port 3306 při připojování tooAzure databáze pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="badb5-142">Always use port 3306 when connecting tooAzure Database for MySQL.</span></span> |
    | <span data-ttu-id="badb5-143">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="badb5-143">Username</span></span> |  <span data-ttu-id="badb5-144">*přihlašovací jméno správce serveru*</span><span class="sxs-lookup"><span data-stu-id="badb5-144">*server admin login name*</span></span> | <span data-ttu-id="badb5-145">Zadejte hello serveru správce přihlašovací uživatelské jméno, když máte vytvořený hello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="badb5-145">Type in hello server admin login username supplied when you created hello Azure Database for MySQL earlier.</span></span> <span data-ttu-id="badb5-146">Uživatelské jméno v našem příkladu je myadmin@myserver4demo.</span><span class="sxs-lookup"><span data-stu-id="badb5-146">Our example username is myadmin@myserver4demo.</span></span> <span data-ttu-id="badb5-147">Pokud si nepamatujete hello uživatelské jméno, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="badb5-147">Follow hello steps in hello previous section tooget hello connection information if you do not remember hello username.</span></span> <span data-ttu-id="badb5-148">Formát Hello je  *username@servername* .</span><span class="sxs-lookup"><span data-stu-id="badb5-148">hello format is *username@servername*.</span></span>
    | <span data-ttu-id="badb5-149">Heslo</span><span class="sxs-lookup"><span data-stu-id="badb5-149">Password</span></span> | <span data-ttu-id="badb5-150">vaše heslo</span><span class="sxs-lookup"><span data-stu-id="badb5-150">your password</span></span> | <span data-ttu-id="badb5-151">Klikněte na tlačítko **úložiště v trezoru...**  tlačítko toosave hello heslo.</span><span class="sxs-lookup"><span data-stu-id="badb5-151">Click **Store in Vault...** button toosave hello password.</span></span> |

3.   <span data-ttu-id="badb5-152">Klikněte na tlačítko **Test připojení** tootest, pokud jsou správně nakonfigurovány všechny parametry.</span><span class="sxs-lookup"><span data-stu-id="badb5-152">Click **Test Connection** tootest if all parameters are correctly configured.</span></span> 

4.   <span data-ttu-id="badb5-153">Klikněte na tlačítko **OK** toosave hello připojení.</span><span class="sxs-lookup"><span data-stu-id="badb5-153">Click **OK** toosave hello connection.</span></span> 

5.   <span data-ttu-id="badb5-154">V seznamu hello z **MySQL připojení**, klikněte na serveru odpovídající tooyour hello dlaždic a počkejte toobe připojení hello navázat.</span><span class="sxs-lookup"><span data-stu-id="badb5-154">In hello listing of **MySQL Connections**, click hello tile corresponding tooyour server and wait for hello connection toobe established.</span></span>

6.   <span data-ttu-id="badb5-155">Otevře novou kartu SQL s prázdné editoru můžete zadat své dotazy.</span><span class="sxs-lookup"><span data-stu-id="badb5-155">A new SQL tab opens with a blank editor where you can type your queries.</span></span>

    > [!NOTE]
    > <span data-ttu-id="badb5-156">Ve výchozím nastavení je zabezpečení připojení protokol SSL vyžaduje a vynucovat u vaší databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="badb5-156">By default, SSL connection security is required and enforced on your Azure Database for MySQL server.</span></span> <span data-ttu-id="badb5-157">Žádná další konfigurace s certifikáty protokolu SSL je obvykle MySQL Workbench tooconnect tooyour server vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="badb5-157">Typically no additional configuration with SSL certificates is required for MySQL Workbench tooconnect tooyour server.</span></span> <span data-ttu-id="badb5-158">Další informace o SSL najdete v tématu [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="badb5-158">For more information on SSL, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span>  <span data-ttu-id="badb5-159">Pokud potřebujete toodisable SSL, navštivte hello portál Azure a klikněte na tlačítko hello připojení zabezpečení stránky toodisable hello vynutit SSL připojení přepínací tlačítko.</span><span class="sxs-lookup"><span data-stu-id="badb5-159">If you need toodisable SSL, visit hello Azure portal and click hello Connection security page toodisable hello Enforce SSL connection toggle button.</span></span>

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a><span data-ttu-id="badb5-160">Umožňuje vytvořit tabulku, vkládání dat, čtení dat, aktualizace dat, odstranit data</span><span class="sxs-lookup"><span data-stu-id="badb5-160">Create a table, insert data, read data, update data, delete data</span></span>
1. <span data-ttu-id="badb5-161">Zkopírujte a vložte kód SQL ukázka hello do prázdné tooillustrate kartě SQL ukázková data.</span><span class="sxs-lookup"><span data-stu-id="badb5-161">Copy and paste hello sample SQL code into a blank SQL tab tooillustrate some sample data.</span></span>

    <span data-ttu-id="badb5-162">Tento kód vytvoří prázdnou databázi s názvem quickstartdb a poté vytvoří ukázkovou tabulku s názvem inventáře.</span><span class="sxs-lookup"><span data-stu-id="badb5-162">This code creates an empty database named quickstartdb, and then creates a sample table named inventory.</span></span> <span data-ttu-id="badb5-163">Vloží některé řádky a pak přečte hello řádků.</span><span class="sxs-lookup"><span data-stu-id="badb5-163">It inserts some rows, then reads hello rows.</span></span> <span data-ttu-id="badb5-164">Změní hello dat pomocí příkazu update a čtení hello řádků znovu.</span><span class="sxs-lookup"><span data-stu-id="badb5-164">It changes hello data with an update statement, and reads hello rows again.</span></span> <span data-ttu-id="badb5-165">Nakonec se odstraní řádek a přečte hello řádků znovu.</span><span class="sxs-lookup"><span data-stu-id="badb5-165">Finally it deletes a row, and reads hello rows again.</span></span>
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    <span data-ttu-id="badb5-166">Hello – snímek obrazovky ukazuje příklad hello kódu SQL ve výstupu SQL Workbench a hello po jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="badb5-166">hello screenshot shows an example of hello SQL code in SQL Workbench and hello output after it has been run.</span></span>
    
    ![Karta SQL Workbench MySQL toorun ukázkový SQL kód](media/connect-workbench/3-workbench-sql-tab.png)

2. <span data-ttu-id="badb5-168">toorun hello ukázkový kód SQL, klikněte na tlačítko hello zesvětlení bolt ikonu na hello nástrojů hello **soubor SQL** kartě.</span><span class="sxs-lookup"><span data-stu-id="badb5-168">toorun hello sample SQL Code, click hello lightening bolt icon in hello toolbar of hello **SQL File** tab.</span></span>
3. <span data-ttu-id="badb5-169">Všimněte si hello tři záložkách výsledkem hello **výsledek mřížky** části uprostřed hello stránku hello.</span><span class="sxs-lookup"><span data-stu-id="badb5-169">Notice hello three tabbed results in hello **Result Grid** section in hello middle of hello page.</span></span> 
4. <span data-ttu-id="badb5-170">Všimněte si hello **výstup** seznam v dolní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="badb5-170">Notice hello **Output** list at hello bottom of hello page.</span></span> <span data-ttu-id="badb5-171">Hello stav každého příkazu se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="badb5-171">hello status of each command is shown.</span></span> 

<span data-ttu-id="badb5-172">Nyní jste připojení tooAzure databáze pro databázi MySQL pomocí MySQL Workbench a zkontrolují data pomocí jazyka SQL hello.</span><span class="sxs-lookup"><span data-stu-id="badb5-172">Now, you have connected tooAzure Database for MySQL using MySQL Workbench, and have queried data using hello SQL language.</span></span>

## <a name="next-steps"></a><span data-ttu-id="badb5-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="badb5-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="badb5-174">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="badb5-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
