---
title: "Připojit k databázi Azure pro databázi MySQL z databáze MySQL Workbench | Microsoft Docs"
description: "Tento rychlý Start obsahuje kroky k MySQL Workbench připojení a dotazování dat z Azure databáze pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql-database
ms.custom: mvc
ms.topic: article
ms.date: 08/23/2017
ms.openlocfilehash: 20a1f31ce42abb924504c4008f85420fc49aec89
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-to-connect-and-query-data"></a><span data-ttu-id="c00ca-103">Azure databáze pro databázi MySQL: použití MySQL Workbench, aby se připojení a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="c00ca-103">Azure Database for MySQL: Use MySQL Workbench to connect and query data</span></span>
<span data-ttu-id="c00ca-104">Tento rychlý start předvádí, jak se připojit k databázi Azure pro databázi MySQL pomocí aplikace MySQL Workbench.</span><span class="sxs-lookup"><span data-stu-id="c00ca-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using the MySQL Workbench application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c00ca-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c00ca-105">Prerequisites</span></span>
<span data-ttu-id="c00ca-106">Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="c00ca-106">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c00ca-107">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c00ca-107">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="c00ca-108">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c00ca-108">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a><span data-ttu-id="c00ca-109">Nainstalujte MySQL Workbench</span><span class="sxs-lookup"><span data-stu-id="c00ca-109">Install MySQL Workbench</span></span>
<span data-ttu-id="c00ca-110">Stáhněte a nainstalujte MySQL Workbench na vašem počítače od [webu MySQL](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="c00ca-110">Download and install MySQL Workbench on your computer from [the MySQL website](https://dev.mysql.com/downloads/workbench/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c00ca-111">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="c00ca-111">Get connection information</span></span>
<span data-ttu-id="c00ca-112">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="c00ca-112">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="c00ca-113">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c00ca-113">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c00ca-114">Přihlaste se k [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c00ca-114">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="c00ca-115">V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="c00ca-115">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>

3. <span data-ttu-id="c00ca-116">Klikněte na název serveru.</span><span class="sxs-lookup"><span data-stu-id="c00ca-116">Click the server name.</span></span>

4. <span data-ttu-id="c00ca-117">Vyberte stránku **Vlastnosti** serveru.</span><span class="sxs-lookup"><span data-stu-id="c00ca-117">Select the server's **Properties** page.</span></span> <span data-ttu-id="c00ca-118">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="c00ca-118">Make a note of the **Server name** and **Server admin login name**.</span></span>

 ![Databáze Azure pro název serveru MySQL](./media/connect-workbench/1-server-properties-name-login.png)
 
5. <span data-ttu-id="c00ca-120">Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.</span><span class="sxs-lookup"><span data-stu-id="c00ca-120">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-to-the-server-using-mysql-workbench"></a><span data-ttu-id="c00ca-121">Připojit k serveru pomocí MySQL Workbench</span><span class="sxs-lookup"><span data-stu-id="c00ca-121">Connect to the server using MySQL Workbench</span></span> 
<span data-ttu-id="c00ca-122">Připojení k serveru Azure MySQL pomocí nástroje s grafickým uživatelským rozhraním MySQL Workbench:</span><span class="sxs-lookup"><span data-stu-id="c00ca-122">To connect to Azure MySQL server using the GUI tool MySQL Workbench:</span></span>

1.  <span data-ttu-id="c00ca-123">Spusťte aplikaci MySQL Workbench ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c00ca-123">Launch the MySQL Workbench application on your computer.</span></span> 

2.  <span data-ttu-id="c00ca-124">V **nastavit připojení k nové** dialogové okno pole, zadejte následující informace **parametry** karty:</span><span class="sxs-lookup"><span data-stu-id="c00ca-124">In **Setup New Connection** dialog box, enter the following information on the **Parameters** tab:</span></span>

    ![nastavení nového připojení](./media/connect-workbench/2-setup-new-connection.png)

    | <span data-ttu-id="c00ca-126">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="c00ca-126">**Setting**</span></span> | <span data-ttu-id="c00ca-127">**Navrhovaná hodnota**</span><span class="sxs-lookup"><span data-stu-id="c00ca-127">**Suggested value**</span></span> | <span data-ttu-id="c00ca-128">**Popis pole**</span><span class="sxs-lookup"><span data-stu-id="c00ca-128">**Field description**</span></span> |
    |---|---|---|
    |   <span data-ttu-id="c00ca-129">Název připojení</span><span class="sxs-lookup"><span data-stu-id="c00ca-129">Connection Name</span></span> | <span data-ttu-id="c00ca-130">Ukázkové připojení</span><span class="sxs-lookup"><span data-stu-id="c00ca-130">Demo Connection</span></span> | <span data-ttu-id="c00ca-131">Zadejte popisek pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="c00ca-131">Specify a label for this connection.</span></span> |
    | <span data-ttu-id="c00ca-132">Způsob připojení</span><span class="sxs-lookup"><span data-stu-id="c00ca-132">Connection Method</span></span> | <span data-ttu-id="c00ca-133">Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="c00ca-133">Standard (TCP/IP)</span></span> | <span data-ttu-id="c00ca-134">Standard (TCP/IP) je dostačující.</span><span class="sxs-lookup"><span data-stu-id="c00ca-134">Standard (TCP/IP) is sufficient.</span></span> |
    | <span data-ttu-id="c00ca-135">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="c00ca-135">Hostname</span></span> | <span data-ttu-id="c00ca-136">*název serveru*</span><span class="sxs-lookup"><span data-stu-id="c00ca-136">*server name*</span></span> | <span data-ttu-id="c00ca-137">Zadejte hodnotu názvu serveru, kterou jste použili dříve při vytváření služby Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="c00ca-137">Specify the server name value that was used when you created the Azure Database for MySQL earlier.</span></span> <span data-ttu-id="c00ca-138">Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použijte plně kvalifikovaný název domény (\*.mysql.database.azure.com), jak je znázorněno v příkladu.</span><span class="sxs-lookup"><span data-stu-id="c00ca-138">Our example server shown is myserver4demo.mysql.database.azure.com. Use the fully qualified domain name (\*.mysql.database.azure.com) as shown in the example.</span></span> <span data-ttu-id="c00ca-139">Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="c00ca-139">Follow the steps in the previous section to get the connection information if you do not remember your server name.</span></span>  |
    | <span data-ttu-id="c00ca-140">Port</span><span class="sxs-lookup"><span data-stu-id="c00ca-140">Port</span></span> | <span data-ttu-id="c00ca-141">3306</span><span class="sxs-lookup"><span data-stu-id="c00ca-141">3306</span></span> | <span data-ttu-id="c00ca-142">Při připojování ke službě Azure Database for MySQL vždy používejte port 3306.</span><span class="sxs-lookup"><span data-stu-id="c00ca-142">Always use port 3306 when connecting to Azure Database for MySQL.</span></span> |
    | <span data-ttu-id="c00ca-143">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="c00ca-143">Username</span></span> |  <span data-ttu-id="c00ca-144">*přihlašovací jméno správce serveru*</span><span class="sxs-lookup"><span data-stu-id="c00ca-144">*server admin login name*</span></span> | <span data-ttu-id="c00ca-145">Zadejte přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření služby Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="c00ca-145">Type in the server admin login username supplied when you created the Azure Database for MySQL earlier.</span></span> <span data-ttu-id="c00ca-146">Uživatelské jméno v našem příkladu je myadmin@myserver4demo.</span><span class="sxs-lookup"><span data-stu-id="c00ca-146">Our example username is myadmin@myserver4demo.</span></span> <span data-ttu-id="c00ca-147">Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="c00ca-147">Follow the steps in the previous section to get the connection information if you do not remember the username.</span></span> <span data-ttu-id="c00ca-148">Formát je *username@servername*.</span><span class="sxs-lookup"><span data-stu-id="c00ca-148">The format is *username@servername*.</span></span>
    | <span data-ttu-id="c00ca-149">Heslo</span><span class="sxs-lookup"><span data-stu-id="c00ca-149">Password</span></span> | <span data-ttu-id="c00ca-150">vaše heslo</span><span class="sxs-lookup"><span data-stu-id="c00ca-150">your password</span></span> | <span data-ttu-id="c00ca-151">Klikněte na tlačítko **úložiště v trezoru...**  tlačítko Uložit heslo.</span><span class="sxs-lookup"><span data-stu-id="c00ca-151">Click **Store in Vault...** button to save the password.</span></span> |

3.   <span data-ttu-id="c00ca-152">Pokud chcete otestovat, jestli jsou všechny parametry správně nakonfigurované, klikněte na **Test připojení**.</span><span class="sxs-lookup"><span data-stu-id="c00ca-152">Click **Test Connection** to test if all parameters are correctly configured.</span></span> 

4.   <span data-ttu-id="c00ca-153">Klikněte na tlačítko **OK** pro uložení připojení.</span><span class="sxs-lookup"><span data-stu-id="c00ca-153">Click **OK** to save the connection.</span></span> 

5.   <span data-ttu-id="c00ca-154">V seznamu z **MySQL připojení**, klikněte na dlaždici odpovídající vašeho serveru a čekat na připojení lze navázat.</span><span class="sxs-lookup"><span data-stu-id="c00ca-154">In the listing of **MySQL Connections**, click the tile corresponding to your server and wait for the connection to be established.</span></span>

6.   <span data-ttu-id="c00ca-155">Otevře novou kartu SQL s prázdné editoru můžete zadat své dotazy.</span><span class="sxs-lookup"><span data-stu-id="c00ca-155">A new SQL tab opens with a blank editor where you can type your queries.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c00ca-156">Ve výchozím nastavení je zabezpečení připojení protokol SSL vyžaduje a vynucovat u vaší databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="c00ca-156">By default, SSL connection security is required and enforced on your Azure Database for MySQL server.</span></span> <span data-ttu-id="c00ca-157">Žádná další konfigurace s certifikáty protokolu SSL je obvykle potřeba MySQL Workbench, aby se připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="c00ca-157">Typically no additional configuration with SSL certificates is required for MySQL Workbench to connect to your server.</span></span> <span data-ttu-id="c00ca-158">Další informace o SSL najdete v tématu [připojení SSL konfigurace v aplikaci pro zabezpečené připojení k databázi Azure pro databázi MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="c00ca-158">For more information on SSL, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span>  <span data-ttu-id="c00ca-159">Pokud je nutné zakázat protokol SSL, najdete na portálu Azure a klikněte na stránce zabezpečení připojení zakázat přepínací tlačítko připojení SSL vynutit.</span><span class="sxs-lookup"><span data-stu-id="c00ca-159">If you need to disable SSL, visit the Azure portal and click the Connection security page to disable the Enforce SSL connection toggle button.</span></span>

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a><span data-ttu-id="c00ca-160">Umožňuje vytvořit tabulku, vkládání dat, čtení dat, aktualizace dat, odstranit data</span><span class="sxs-lookup"><span data-stu-id="c00ca-160">Create a table, insert data, read data, update data, delete data</span></span>
1. <span data-ttu-id="c00ca-161">Zkopírujte a vložte ukázkový kód SQL do prázdné karty SQL pro ilustraci ukázková data.</span><span class="sxs-lookup"><span data-stu-id="c00ca-161">Copy and paste the sample SQL code into a blank SQL tab to illustrate some sample data.</span></span>

    <span data-ttu-id="c00ca-162">Tento kód vytvoří prázdnou databázi s názvem quickstartdb a poté vytvoří ukázkovou tabulku s názvem inventáře.</span><span class="sxs-lookup"><span data-stu-id="c00ca-162">This code creates an empty database named quickstartdb, and then creates a sample table named inventory.</span></span> <span data-ttu-id="c00ca-163">Vloží některé řádky, potom načte řádky.</span><span class="sxs-lookup"><span data-stu-id="c00ca-163">It inserts some rows, then reads the rows.</span></span> <span data-ttu-id="c00ca-164">Změny dat pomocí příkazu update a znovu načte řádky.</span><span class="sxs-lookup"><span data-stu-id="c00ca-164">It changes the data with an update statement, and reads the rows again.</span></span> <span data-ttu-id="c00ca-165">Nakonec se odstraní řádek a přečte řádky znovu.</span><span class="sxs-lookup"><span data-stu-id="c00ca-165">Finally it deletes a row, and reads the rows again.</span></span>
    
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

    <span data-ttu-id="c00ca-166">Na snímku obrazovky vidíte příklad kódu SQL v SQL Workbench a výstup po jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="c00ca-166">The screenshot shows an example of the SQL code in SQL Workbench and the output after it has been run.</span></span>
    
    ![Karta SQL Workbench MySQL spustit ukázkový kód SQL](media/connect-workbench/3-workbench-sql-tab.png)

2. <span data-ttu-id="c00ca-168">Pokud chcete spustit ukázkový kód SQL, klikněte na ikonu zesvětlením bolt na panelu nástrojů **soubor SQL** kartě.</span><span class="sxs-lookup"><span data-stu-id="c00ca-168">To run the sample SQL Code, click the lightening bolt icon in the toolbar of the **SQL File** tab.</span></span>
3. <span data-ttu-id="c00ca-169">Všimněte si tři záložkách výsledky v **výsledek mřížky** části uprostřed stránky.</span><span class="sxs-lookup"><span data-stu-id="c00ca-169">Notice the three tabbed results in the **Result Grid** section in the middle of the page.</span></span> 
4. <span data-ttu-id="c00ca-170">Upozornění **výstup** seznam v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="c00ca-170">Notice the **Output** list at the bottom of the page.</span></span> <span data-ttu-id="c00ca-171">Stav každého příkazu se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c00ca-171">The status of each command is shown.</span></span> 

<span data-ttu-id="c00ca-172">Nyní připojili k databázi Azure pro databázi MySQL pomocí MySQL Workbench a zkontrolují data pomocí jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="c00ca-172">Now, you have connected to Azure Database for MySQL using MySQL Workbench, and have queried data using the SQL language.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c00ca-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c00ca-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c00ca-174">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="c00ca-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
