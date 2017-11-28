---
title: "Navrhnout první databáze Azure pro PostgreSQL pomocí portálu Azure | Microsoft Docs"
description: "Tento kurz ukazuje, jak navrhnout první databáze Azure pro PostgreSQL pomocí portálu Azure."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 05/10/2017
ms.openlocfilehash: 2aa9d10749b54537495ad3e09566c43718f67a9e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-the-azure-portal"></a><span data-ttu-id="6608b-103">Navrhnout první databáze Azure pro PostgreSQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6608b-103">Design your first Azure Database for PostgreSQL using the Azure portal</span></span>

<span data-ttu-id="6608b-104">Azure Database for PostgreSQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze PostgreSQL v cloudu.</span><span class="sxs-lookup"><span data-stu-id="6608b-104">Azure Database for PostgreSQL is a managed service that enables you to run, manage, and scale highly available PostgreSQL databases in the cloud.</span></span> <span data-ttu-id="6608b-105">Pomocí portálu Azure, můžete snadno spravovat váš server a návrhu databáze.</span><span class="sxs-lookup"><span data-stu-id="6608b-105">Using the Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="6608b-106">V tomto kurzu pomocí portálu Azure další postup:</span><span class="sxs-lookup"><span data-stu-id="6608b-106">In this tutorial, you use the Azure portal to learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="6608b-107">Vytvoření Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6608b-107">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="6608b-108">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="6608b-108">Configure the server firewall</span></span>
> * <span data-ttu-id="6608b-109">Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj k vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="6608b-109">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility to create a database</span></span>
> * <span data-ttu-id="6608b-110">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="6608b-110">Load sample data</span></span>
> * <span data-ttu-id="6608b-111">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="6608b-111">Query data</span></span>
> * <span data-ttu-id="6608b-112">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="6608b-112">Update data</span></span>
> * <span data-ttu-id="6608b-113">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="6608b-113">Restore data</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6608b-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6608b-114">Prerequisites</span></span>
<span data-ttu-id="6608b-115">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="6608b-115">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="6608b-116">Přihlášení k portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6608b-116">Log in to the Azure portal</span></span>
<span data-ttu-id="6608b-117">Přihlaste se k [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6608b-117">Log in to the [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-postgresql"></a><span data-ttu-id="6608b-118">Vytvoření Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6608b-118">Create an Azure Database for PostgreSQL</span></span>

<span data-ttu-id="6608b-119">Server Azure Database for PostgreSQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="6608b-119">An Azure Database for PostgreSQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="6608b-120">Server se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6608b-120">The server is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="6608b-121">Server Azure Database for PostgreSQL vytvoříte pomocí tohoto postupu:</span><span class="sxs-lookup"><span data-stu-id="6608b-121">Follow these steps to create an Azure Database for PostgreSQL server:</span></span>
1.  <span data-ttu-id="6608b-122">Klikněte **+ nový** nalezeno tlačítko v levém horním rohu portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6608b-122">Click the **+ New**  button found on the upper left-hand corner of the Azure portal.</span></span>
2.  <span data-ttu-id="6608b-123">Na stránce **Nový** vyberte **Databáze** a na stránce **Databáze** vyberte **Azure Database for PostgreSQL**.</span><span class="sxs-lookup"><span data-stu-id="6608b-123">Select **Databases** from the **New** page, and select **Azure Database for PostgreSQL** from the **Databases** page.</span></span>
 <span data-ttu-id="6608b-124">![Azure Database for PostgreSQL – vytvoření databáze](./media/tutorial-design-database-using-azure-portal/1-create-database.png)</span><span class="sxs-lookup"><span data-stu-id="6608b-124">![Azure Database for PostgreSQL - Create the database](./media/tutorial-design-database-using-azure-portal/1-create-database.png)</span></span>

3.  <span data-ttu-id="6608b-125">Vyplňte formulář podrobností nového serveru pomocí následujících informací, jak je vidět na předchozím obrázku:</span><span class="sxs-lookup"><span data-stu-id="6608b-125">Fill out the new server details form with the following information, as shown on the preceding image:</span></span>
    - <span data-ttu-id="6608b-126">Název serveru: **mypgserver-20170401** (název serveru se mapuje na název DNS a proto musí být globálně jedinečný)</span><span class="sxs-lookup"><span data-stu-id="6608b-126">Server name: **mypgserver-20170401** (name of a server maps to DNS name and is thus required to be globally unique)</span></span> 
    - <span data-ttu-id="6608b-127">Předplatné: Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém prostředek existuje nebo je účtován.</span><span class="sxs-lookup"><span data-stu-id="6608b-127">Subscription: If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span>
    - <span data-ttu-id="6608b-128">Skupina prostředků: **myresourcegroup**</span><span class="sxs-lookup"><span data-stu-id="6608b-128">Resource group: **myresourcegroup**</span></span>
    - <span data-ttu-id="6608b-129">Přihlašovací jméno správce serveru a heslo dle vašeho výběru</span><span class="sxs-lookup"><span data-stu-id="6608b-129">Server admin login and password of your choice</span></span>
    - <span data-ttu-id="6608b-130">Umístění</span><span class="sxs-lookup"><span data-stu-id="6608b-130">Location</span></span>
    - <span data-ttu-id="6608b-131">Verze PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6608b-131">PostgreSQL Version</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="6608b-132">Zde zadané jméno správce serveru a heslo se vyžadují k přihlášení na server a jeho databáze dále v tomto rychlém startu.</span><span class="sxs-lookup"><span data-stu-id="6608b-132">The server admin login and password that you specify here are required to log in to the server and its databases later in this quick start.</span></span> <span data-ttu-id="6608b-133">Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="6608b-133">Remember or record this information for later use.</span></span>

4.  <span data-ttu-id="6608b-134">Klikněte na **Cenová úroveň** a určete úroveň služby a úroveň výkonu pro novou databázi.</span><span class="sxs-lookup"><span data-stu-id="6608b-134">Click **Pricing tier** to specify the service tier and performance level for your new database.</span></span> <span data-ttu-id="6608b-135">Pro tento rychlý start vyberte úroveň **Basic**, **50 výpočetních jednotek** a **50 GB** zahrnutého úložiště.</span><span class="sxs-lookup"><span data-stu-id="6608b-135">For this quick start, select **Basic** Tier, **50 Compute Units** and **50 GB** of included storage.</span></span>
 <span data-ttu-id="6608b-136">![Azure Database for PostgreSQL – výběr úrovně služby](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)</span><span class="sxs-lookup"><span data-stu-id="6608b-136">![Azure Database for PostgreSQL - pick the service tier](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)</span></span>
5.  <span data-ttu-id="6608b-137">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6608b-137">Click **Ok**.</span></span>
6.  <span data-ttu-id="6608b-138">Klikněte na **Vytvořit**, aby se server zřídil.</span><span class="sxs-lookup"><span data-stu-id="6608b-138">Click **Create** to provision the server.</span></span> <span data-ttu-id="6608b-139">Zřizování trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="6608b-139">Provisioning takes a few minutes.</span></span>

  > [!TIP]
  > <span data-ttu-id="6608b-140">Zaškrtněte možnost **Připnout na řídicí panel**, abyste povolili snadné sledování vašich nasazení.</span><span class="sxs-lookup"><span data-stu-id="6608b-140">Check the **Pin to dashboard** option to allow easy tracking of your deployments.</span></span>

7.  <span data-ttu-id="6608b-141">Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.</span><span class="sxs-lookup"><span data-stu-id="6608b-141">On the toolbar, click **Notifications** to monitor the deployment process.</span></span>
 <span data-ttu-id="6608b-142">![Azure Database for PostgreSQL – zobrazení oznámení](./media/tutorial-design-database-using-azure-portal/3-notifications.png)</span><span class="sxs-lookup"><span data-stu-id="6608b-142">![Azure Database for PostgreSQL - See notifications](./media/tutorial-design-database-using-azure-portal/3-notifications.png)</span></span>
   
  <span data-ttu-id="6608b-143">Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="6608b-143">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="6608b-144">Databáze [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) je výchozí databáze určená pro uživatele, nástroje a aplikace třetích stran.</span><span class="sxs-lookup"><span data-stu-id="6608b-144">The [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 

## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="6608b-145">Konfigurace pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="6608b-145">Configure a server-level firewall rule</span></span>

<span data-ttu-id="6608b-146">Služba Azure Database for PostgreSQL vytváří bránu firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="6608b-146">The Azure Database for PostgreSQL service creates a firewall at the server-level.</span></span> <span data-ttu-id="6608b-147">Tato brána firewall brání externím aplikacím a nástrojům v připojení k serveru a kterékoli databázi na serveru, pokud není vytvořené pravidlo brány firewall k otevření brány firewall pro konkrétní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="6608b-147">This firewall prevents external applications and tools from connecting to the server and any databases on the server unless a firewall rule is created to open the firewall for specific IP addresses.</span></span> 

1.  <span data-ttu-id="6608b-148">Jakmile se nasazení dokončí, klikněte na **Všechny prostředky** v levé nabídce a zadejte název **mypgserver-20170401**. Vyhledáte tak nově vytvořený server.</span><span class="sxs-lookup"><span data-stu-id="6608b-148">After the deployment completes, click **All Resources** from the left-hand menu and type in the name **mypgserver-20170401** to search for your newly created server.</span></span> <span data-ttu-id="6608b-149">Klikněte na název serveru uvedený ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="6608b-149">Click the server name listed in the search result.</span></span> <span data-ttu-id="6608b-150">Otevře se stránka **Přehled** vašeho serveru a poskytne vám možnosti další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6608b-150">The **Overview** page for your server opens and provides options for further configuration.</span></span>
 
 ![<span data-ttu-id="6608b-151">Azure Database for PostgreSQL – hledání serveru</span><span class="sxs-lookup"><span data-stu-id="6608b-151">Azure Database for PostgreSQL - Search for server</span></span> ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2.  <span data-ttu-id="6608b-152">V okně serveru vyberte **Zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="6608b-152">In the server blade, select **Connection Security**.</span></span> 
3.  <span data-ttu-id="6608b-153">Klikněte do textového pole pod **Názvem pravidla** a přidejte nové pravidlo brány firewall, kterým povolíte připojení rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="6608b-153">Click in the text box under **Rule Name,** and add a new firewall rule to whitelist the IP range for connectivity.</span></span> <span data-ttu-id="6608b-154">V tomto kurzu budeme povolit všechny IP adresy a to zadáním **název pravidla = AllowAllIps**, **počáteční IP = 0.0.0.0** a **Koncová IP adresa = 255.255.255.255** a pak klikněte na tlačítko **uložit** .</span><span class="sxs-lookup"><span data-stu-id="6608b-154">For this tutorial, let's allow all IPs by typing in **Rule Name = AllowAllIps**, **Start IP = 0.0.0.0** and **End IP = 255.255.255.255** and then click **Save**.</span></span> <span data-ttu-id="6608b-155">Abyste se mohli připojit z vaší sítě, můžete nastavit pravidlo brány firewall, které pokrývá rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="6608b-155">You can set a firewall rule that covers an IP range to be able to connect from your network.</span></span>
 
 ![Azure Database for PostgreSQL – vytvoření pravidla brány firewall](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4.  <span data-ttu-id="6608b-157">Klikněte na **Uložit** a pak kliknutím na **X** zavřete stránku **Zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="6608b-157">Click **Save** and then click the **X** to close the **Connections Security** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6608b-158">Server Azure PostgreSQL komunikuje přes port 5432.</span><span class="sxs-lookup"><span data-stu-id="6608b-158">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="6608b-159">Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="6608b-159">If you are trying to connect from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="6608b-160">Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 5432.</span><span class="sxs-lookup"><span data-stu-id="6608b-160">If so, you will not be able to connect to your Azure SQL Database server unless your IT department opens port 5432.</span></span>
  >


## <a name="get-the-connection-information"></a><span data-ttu-id="6608b-161">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="6608b-161">Get the connection information</span></span>

<span data-ttu-id="6608b-162">Při vytvoření našeho serveru Azure Database for PostgreSQL se vytvoří i výchozí databáze **postgres**.</span><span class="sxs-lookup"><span data-stu-id="6608b-162">When we created our Azure Database for PostgreSQL server, the default **postgres** database also gets created.</span></span> <span data-ttu-id="6608b-163">Pokud se chcete připojit k databázovému serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.</span><span class="sxs-lookup"><span data-stu-id="6608b-163">To connect to your database server, you need to provide host information and access credentials.</span></span>

1. <span data-ttu-id="6608b-164">V nabídce na levé straně na portálu Azure Portal klikněte na **Všechny prostředky** a vyhledejte právě vytvořený server **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="6608b-164">From the left-hand menu in Azure portal, click **All resources** and search for the server you just created **mypgserver-20170401**.</span></span>

  ![<span data-ttu-id="6608b-165">Azure Database for PostgreSQL – hledání serveru</span><span class="sxs-lookup"><span data-stu-id="6608b-165">Azure Database for PostgreSQL - Search for server</span></span> ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

3. <span data-ttu-id="6608b-166">Klikněte na název serveru **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="6608b-166">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="6608b-167">Vyberte stránku **Přehled** serveru.</span><span class="sxs-lookup"><span data-stu-id="6608b-167">Select the server's **Overview** page.</span></span> <span data-ttu-id="6608b-168">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="6608b-168">Make a note of the **Server name** and **Server admin login name**.</span></span>

 ![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a><span data-ttu-id="6608b-170">Připojení k databázi PostgreSQL pomocí psql ve službě Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="6608b-170">Connect to PostgreSQL database using psql in Cloud Shell</span></span>

<span data-ttu-id="6608b-171">Teď pro připojení k serveru Azure Database for PostgreSQL použijeme nástroj příkazového řádku psql.</span><span class="sxs-lookup"><span data-stu-id="6608b-171">Let's now use the psql command-line utility to connect to the Azure Database for PostgreSQL server.</span></span> 
1. <span data-ttu-id="6608b-172">Pomocí ikony terminálu v horním navigačním podokně spusťte službu Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6608b-172">Launch the Azure Cloud Shell via the terminal icon on the top navigation pane.</span></span>

   ![Azure Database for PostgreSQL – ikona terminálu Azure Cloud Shell](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. <span data-ttu-id="6608b-174">Služba Azure Cloud Shell se otevře v prohlížeči a umožní vám zadat příkazy Bash.</span><span class="sxs-lookup"><span data-stu-id="6608b-174">The Azure Cloud Shell opens in your browser, enabling you to type bash commands.</span></span>

   ![Azure Database for PostgreSQL – příkazový řádek Bash služby Azure Shell](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. <span data-ttu-id="6608b-176">V příkazovém řádku služby Cloud Shell se pomocí příkazů psql připojte k serveru Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="6608b-176">At the Cloud Shell prompt, connect to your Azure Database for PostgreSQL server using the psql commands.</span></span> <span data-ttu-id="6608b-177">Následující formát se používá pro připojení k serveru Azure Database for PostgreSQL s nástrojem [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html):</span><span class="sxs-lookup"><span data-stu-id="6608b-177">The following format is used to connect to an Azure Database for PostgreSQL server with the [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility:</span></span>
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   <span data-ttu-id="6608b-178">Například tento příkaz provádí připojení k výchozí databázi s názvem **postgres** na vašem serveru PostgreSQL **mypgserver-20170401.postgres.database.azure.com** pomocí přihlašovacích údajů k přístupu.</span><span class="sxs-lookup"><span data-stu-id="6608b-178">For example, the following command connects to the default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="6608b-179">Po zobrazení výzvy zadejte heslo správce serveru.</span><span class="sxs-lookup"><span data-stu-id="6608b-179">Enter your server admin password when prompted.</span></span>

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```

## <a name="create-a-new-database"></a><span data-ttu-id="6608b-180">Vytvoření nové databáze</span><span class="sxs-lookup"><span data-stu-id="6608b-180">Create a New Database</span></span>
<span data-ttu-id="6608b-181">Po připojení k serveru vytvořte v příkazovém řádku prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="6608b-181">Once you're connected to the server, create a blank database at the prompt.</span></span>
```bash
CREATE DATABASE mypgsqldb;
```

<span data-ttu-id="6608b-182">V příkazovém řádku proveďte následující příkaz pro přepnutí připojení na nově vytvořenou databázi **mypgsqldb**.</span><span class="sxs-lookup"><span data-stu-id="6608b-182">At the prompt, execute the following command to switch connection to the newly created database **mypgsqldb**.</span></span>
```bash
\c mypgsqldb
```
## <a name="create-tables-in-the-database"></a><span data-ttu-id="6608b-183">Vytváření tabulek v databázi</span><span class="sxs-lookup"><span data-stu-id="6608b-183">Create tables in the database</span></span>
<span data-ttu-id="6608b-184">Teď, když víte, jak se připojit k databázi Azure pro PostgreSQL, jsme projít jak provést některé základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="6608b-184">Now that you know how to connect to the Azure Database for PostgreSQL, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="6608b-185">Jsme nejprve vytvořit tabulku a načíst určitými daty.</span><span class="sxs-lookup"><span data-stu-id="6608b-185">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="6608b-186">Umožňuje vytvořit tabulku, která sleduje informace o inventáři.</span><span class="sxs-lookup"><span data-stu-id="6608b-186">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="6608b-187">Nyní jsou zobrazeny nově vytvořené tabulky v seznamu tabvles zadáním:</span><span class="sxs-lookup"><span data-stu-id="6608b-187">You can see the newly created table in the list of tabvles now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="6608b-188">Načtení dat do tabulky</span><span class="sxs-lookup"><span data-stu-id="6608b-188">Load data into the tables</span></span>
<span data-ttu-id="6608b-189">Teď, když máme tabulku, jsme do něj vložte některá data.</span><span class="sxs-lookup"><span data-stu-id="6608b-189">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="6608b-190">V okně Otevřít příkazového řádku spusťte následující dotaz vložit některé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="6608b-190">At the open command prompt window, run the following query to insert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="6608b-191">Máte nyní dva řádky ukázkových dat do tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6608b-191">You have now two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="6608b-192">Dotaz a aktualizovat data v tabulkách</span><span class="sxs-lookup"><span data-stu-id="6608b-192">Query and update the data in the tables</span></span>
<span data-ttu-id="6608b-193">Spustíte následující dotaz pro načtení informací z tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="6608b-193">Execute the following query to retrieve information from the database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="6608b-194">Můžete také aktualizovat data v tabulkách</span><span class="sxs-lookup"><span data-stu-id="6608b-194">You can also update the data in the tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="6608b-195">Načtení dat získá příslušným způsobem aktualizuje řádek.</span><span class="sxs-lookup"><span data-stu-id="6608b-195">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a><span data-ttu-id="6608b-196">Obnovení dat do předchozího bodu v čase</span><span class="sxs-lookup"><span data-stu-id="6608b-196">Restore data to a previous point in time</span></span>
<span data-ttu-id="6608b-197">Představte si, že jste omylem odstranili této tabulky.</span><span class="sxs-lookup"><span data-stu-id="6608b-197">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="6608b-198">Tato situace je něco, které nelze snadno obnovit z.</span><span class="sxs-lookup"><span data-stu-id="6608b-198">This situation is something you cannot easily recover from.</span></span> <span data-ttu-id="6608b-199">Azure databázi PostgreSQL můžete přejít zpět k žádné bodu v čase (v poslední až do 7 dnů (Basic) a 35 dní (standardní)) a obnovit tento bod v čase na nový server.</span><span class="sxs-lookup"><span data-stu-id="6608b-199">Azure Database for PostgreSQL allows you to go back to any point-in-time (in the last up to 7 days (Basic) and 35 days (Standard)) and restore this point-in-time to a new server.</span></span> <span data-ttu-id="6608b-200">Tento nový server můžete obnovit odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="6608b-200">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="6608b-201">Ukázka serveru bod před přidáním tabulky obnovit následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6608b-201">The following steps restore the sample server to a point before the table was added.</span></span>

1.  <span data-ttu-id="6608b-202">V databázi Azure pro stránku PostgreSQL pro váš server, klikněte na tlačítko **obnovení** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="6608b-202">On the Azure Database for PostgreSQL page for your server, click **Restore** on the toolbar.</span></span> <span data-ttu-id="6608b-203">**Obnovení** otevře se stránka.</span><span class="sxs-lookup"><span data-stu-id="6608b-203">The **Restore** page opens.</span></span>
  <span data-ttu-id="6608b-204">![Portál Azure – možnosti obnovení formuláře](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)</span><span class="sxs-lookup"><span data-stu-id="6608b-204">![Azure portal - Restore form options](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)</span></span>
2.  <span data-ttu-id="6608b-205">Vyplňte **obnovení** formuláře se požadované informace:</span><span class="sxs-lookup"><span data-stu-id="6608b-205">Fill out the **Restore** form with the required information:</span></span>

  ![Portál Azure – možnosti obnovení formuláře](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)
  - <span data-ttu-id="6608b-207">**Bod obnovení**: Vyberte bodu v čase, k níž dojde před server byl změněn</span><span class="sxs-lookup"><span data-stu-id="6608b-207">**Restore point**: Select a point-in-time that occurs before the server was changed</span></span>
  - <span data-ttu-id="6608b-208">**Cílový server**: Zadejte nový název serveru, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="6608b-208">**Target server**: Provide a new server name you want to restore to</span></span>
  - <span data-ttu-id="6608b-209">**Umístění**: nelze vyberte oblast, ve výchozím nastavení je stejné jako na zdrojovém serveru</span><span class="sxs-lookup"><span data-stu-id="6608b-209">**Location**: You cannot select the region, by default it is same as the source server</span></span>
  - <span data-ttu-id="6608b-210">**Cenová úroveň**: tuto hodnotu nelze změnit, při obnovení serveru.</span><span class="sxs-lookup"><span data-stu-id="6608b-210">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="6608b-211">Je stejný jako zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="6608b-211">It is same as the source server.</span></span> 
3.  <span data-ttu-id="6608b-212">Klikněte na tlačítko **OK** a obnovte server do [obnovení v daném okamžiku](./howto-restore-server-portal.md) před tabulky byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="6608b-212">Click **OK** to restore the server to [restore to a point-in-time](./howto-restore-server-portal.md) before the tables was deleted.</span></span> <span data-ttu-id="6608b-213">Obnovení serveru do jiného bodu v čase vytvoří duplicitní nový server jako původní server od bodu v čase, které zadáte, za předpokladu, že je v rámci dobu uchování vašeho [vrstvy služby](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="6608b-213">Restoring a server to a different point in time creates a duplicate new server as the original server as of the point in time you specify, provided that it is within the retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6608b-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6608b-214">Next Steps</span></span>
<span data-ttu-id="6608b-215">V tomto kurzu jste zjistili, jak používat portál Azure a další nástroje na:</span><span class="sxs-lookup"><span data-stu-id="6608b-215">In this tutorial, you learned how to use the Azure portal and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="6608b-216">Vytvoření Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6608b-216">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="6608b-217">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="6608b-217">Configure the server firewall</span></span>
> * <span data-ttu-id="6608b-218">Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj k vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="6608b-218">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility to create a database</span></span>
> * <span data-ttu-id="6608b-219">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="6608b-219">Load sample data</span></span>
> * <span data-ttu-id="6608b-220">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="6608b-220">Query data</span></span>
> * <span data-ttu-id="6608b-221">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="6608b-221">Update data</span></span>
> * <span data-ttu-id="6608b-222">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="6608b-222">Restore data</span></span>

<span data-ttu-id="6608b-223">Dále se naučíte, jak používat rozhraní příkazového řádku Azure k provádění podobných úloh naleznete v tomto kurzu: [navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure](tutorial-design-database-using-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6608b-223">Next, learn how to use Azure CLI to do similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using Azure CLI](tutorial-design-database-using-azure-cli.md)</span></span>
