---
title: "Návrh svoji první databázi Azure SQL | Microsoft Docs"
description: "Postup návrhu svoji první databázi Azure SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/03/2017
ms.author: carlrab
ms.openlocfilehash: 69cfffdae5ce2db53acc6d668dbe468c3ef22dc2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="design-your-first-azure-sql-database"></a><span data-ttu-id="56bfb-103">Návrh svoji první databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="56bfb-103">Design your first Azure SQL database</span></span>

<span data-ttu-id="56bfb-104">Databáze SQL Azure je relační databáze jako a služba (DBaaS) v cloudu Microsoftu ("Azure").</span><span class="sxs-lookup"><span data-stu-id="56bfb-104">Azure SQL Database is a relational database-as-a service (DBaaS) in the Microsoft Cloud ("Azure").</span></span> <span data-ttu-id="56bfb-105">V tomto kurzu zjistíte, jak pomocí portálu Azure a [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) na:</span><span class="sxs-lookup"><span data-stu-id="56bfb-105">In this tutorial, you learn how to use the Azure portal and [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="56bfb-106">Vytvoření databáze na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="56bfb-106">Create a database in the Azure portal</span></span>
> * <span data-ttu-id="56bfb-107">Nastavit pravidlo brány firewall na úrovni serveru, na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="56bfb-107">Set up a server-level firewall rule in the Azure portal</span></span>
> * <span data-ttu-id="56bfb-108">Připojení k databázi pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="56bfb-108">Connect to the database with SSMS</span></span>
> * <span data-ttu-id="56bfb-109">Vytváření tabulek pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="56bfb-109">Create tables with SSMS</span></span>
> * <span data-ttu-id="56bfb-110">Hromadné načtení dat pomocí BCP</span><span class="sxs-lookup"><span data-stu-id="56bfb-110">Bulk load data with BCP</span></span>
> * <span data-ttu-id="56bfb-111">Dotaz na data pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="56bfb-111">Query that data with SSMS</span></span>
> * <span data-ttu-id="56bfb-112">Obnovit databázi do předchozí [obnovení bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="56bfb-112">Restore the database to a previous [point in time restore](sql-database-recovery-using-backups.md#point-in-time-restore) in the Azure portal</span></span>

<span data-ttu-id="56bfb-113">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="56bfb-113">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56bfb-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56bfb-114">Prerequisites</span></span>

<span data-ttu-id="56bfb-115">K dokončení tohoto kurzu, ujistěte se, že jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="56bfb-115">To complete this tutorial, make sure you have installed:</span></span>
- <span data-ttu-id="56bfb-116">Nejnovější verzi [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="56bfb-116">The newest version of [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).</span></span>
- <span data-ttu-id="56bfb-117">Nejnovější verzi [BCP a SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).</span><span class="sxs-lookup"><span data-stu-id="56bfb-117">The newest version of [BCP and SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).</span></span>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="56bfb-118">Přihlášení k portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="56bfb-118">Log in to the Azure portal</span></span>

<span data-ttu-id="56bfb-119">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="56bfb-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-blank-sql-database"></a><span data-ttu-id="56bfb-120">Vytvořit prázdnou databázi SQL</span><span class="sxs-lookup"><span data-stu-id="56bfb-120">Create a blank SQL database</span></span>

<span data-ttu-id="56bfb-121">Databáze SQL Azure se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="56bfb-121">An Azure SQL database is created with a defined set of [compute and storage resources](sql-database-service-tiers.md).</span></span> <span data-ttu-id="56bfb-122">Databáze se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) a na [logickém serveru Azure SQL Database](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="56bfb-122">The database is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) and in an [Azure SQL Database logical server](sql-database-features.md).</span></span> 

<span data-ttu-id="56bfb-123">Postupujte podle těchto kroků můžete vytvořit prázdnou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="56bfb-123">Follow these steps to create a blank SQL database.</span></span> 

1. <span data-ttu-id="56bfb-124">Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="56bfb-124">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="56bfb-125">Na stránce **Nový** vyberte **Databáze** a na stránce **Databáze** vyberte **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-125">Select **Databases** from the **New** page, and select **SQL Database** from the **Databases** page.</span></span> 

   ![Vytvořit prázdná databáze](./media/sql-database-design-first-database/create-empty-database.png)

3. <span data-ttu-id="56bfb-127">Vyplňte formulář databáze SQL pomocí následujících informací, jak je vidět na předchozím obrázku:</span><span class="sxs-lookup"><span data-stu-id="56bfb-127">Fill out the SQL Database form with the following information, as shown on the preceding image:</span></span>   

   | <span data-ttu-id="56bfb-128">Nastavení</span><span class="sxs-lookup"><span data-stu-id="56bfb-128">Setting</span></span>       | <span data-ttu-id="56bfb-129">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="56bfb-129">Suggested value</span></span> | <span data-ttu-id="56bfb-130">Popis</span><span class="sxs-lookup"><span data-stu-id="56bfb-130">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="56bfb-131">**Název databáze**</span><span class="sxs-lookup"><span data-stu-id="56bfb-131">**Database name**</span></span> | <span data-ttu-id="56bfb-132">mySampleDatabase</span><span class="sxs-lookup"><span data-stu-id="56bfb-132">mySampleDatabase</span></span> | <span data-ttu-id="56bfb-133">Platné názvy databází najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="56bfb-133">For valid database names, see [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span></span> | 
   | <span data-ttu-id="56bfb-134">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="56bfb-134">**Subscription**</span></span> | <span data-ttu-id="56bfb-135">Vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="56bfb-135">Your subscription</span></span>  | <span data-ttu-id="56bfb-136">Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="56bfb-136">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="56bfb-137">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="56bfb-137">**Resource group**</span></span> | <span data-ttu-id="56bfb-138">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="56bfb-138">myResourceGroup</span></span> | <span data-ttu-id="56bfb-139">Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="56bfb-139">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="56bfb-140">**Vyberte zdroj**</span><span class="sxs-lookup"><span data-stu-id="56bfb-140">**Select source**</span></span> | <span data-ttu-id="56bfb-141">Prázdnou databázi</span><span class="sxs-lookup"><span data-stu-id="56bfb-141">Blank database</span></span> | <span data-ttu-id="56bfb-142">Určuje, že by měl být vytvořen prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-142">Specifies that a blank database should be created.</span></span> |

4. <span data-ttu-id="56bfb-143">Klikněte na **Server** a vytvořte a nakonfigurujte nový server pro novou databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-143">Click **Server** to create and configure a new server for your new database.</span></span> <span data-ttu-id="56bfb-144">Vyplňte **nového formuláře serveru** s následujícími informacemi:</span><span class="sxs-lookup"><span data-stu-id="56bfb-144">Fill out the **New server form** with the following information:</span></span> 

   | <span data-ttu-id="56bfb-145">Nastavení</span><span class="sxs-lookup"><span data-stu-id="56bfb-145">Setting</span></span>       | <span data-ttu-id="56bfb-146">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="56bfb-146">Suggested value</span></span> | <span data-ttu-id="56bfb-147">Popis</span><span class="sxs-lookup"><span data-stu-id="56bfb-147">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="56bfb-148">**Název serveru**</span><span class="sxs-lookup"><span data-stu-id="56bfb-148">**Server name**</span></span> | <span data-ttu-id="56bfb-149">Libovolný globálně jedinečný název</span><span class="sxs-lookup"><span data-stu-id="56bfb-149">Any globally unique name</span></span> | <span data-ttu-id="56bfb-150">Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="56bfb-150">For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> | 
   | <span data-ttu-id="56bfb-151">**Přihlašovací jméno správce serveru**</span><span class="sxs-lookup"><span data-stu-id="56bfb-151">**Server admin login**</span></span> | <span data-ttu-id="56bfb-152">Libovolné platné jméno</span><span class="sxs-lookup"><span data-stu-id="56bfb-152">Any valid name</span></span> | <span data-ttu-id="56bfb-153">Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="56bfb-153">For valid login names, see [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span></span>|
   | <span data-ttu-id="56bfb-154">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="56bfb-154">**Password**</span></span> | <span data-ttu-id="56bfb-155">Libovolné platné heslo</span><span class="sxs-lookup"><span data-stu-id="56bfb-155">Any valid password</span></span> | <span data-ttu-id="56bfb-156">Heslo musí mít aspoň 8 znaků a musí obsahovat znaky ze tří z následujících kategorií: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="56bfb-156">Your password must have at least 8 characters and must contain characters from three of the following categories: upper case characters, lower case characters, numbers, and non-alphanumeric characters.</span></span> |
   | <span data-ttu-id="56bfb-157">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="56bfb-157">**Location**</span></span> | <span data-ttu-id="56bfb-158">Libovolné platné umístění</span><span class="sxs-lookup"><span data-stu-id="56bfb-158">Any valid location</span></span> | <span data-ttu-id="56bfb-159">Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="56bfb-159">For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/).</span></span> |

   ![create database-server](./media//sql-database-design-first-database/create-database-server.png)

5. <span data-ttu-id="56bfb-161">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-161">Click **Select**.</span></span>

6. <span data-ttu-id="56bfb-162">Klikněte na **Cenová úroveň** a určete úroveň služby a úroveň výkonu pro novou databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-162">Click **Pricing tier** to specify the service tier and performance level for your new database.</span></span> <span data-ttu-id="56bfb-163">V tomto kurzu vyberte **20 Dtu** a **250** GB úložiště.</span><span class="sxs-lookup"><span data-stu-id="56bfb-163">For this tutorial, select **20 DTUs** and **250** GB of storage.</span></span>

   ![create database-s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. <span data-ttu-id="56bfb-165">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-165">Click **Apply**.</span></span>  

8. <span data-ttu-id="56bfb-166">Vyberte **kolace** pro prázdnou databázi (v tomto kurzu použijte výchozí hodnotu).</span><span class="sxs-lookup"><span data-stu-id="56bfb-166">Select a **collation** for the blank database (for this tutorial, use the default value).</span></span> <span data-ttu-id="56bfb-167">Další informace o kolacích najdete v tématu [kolace](https://docs.microsoft.com/sql/t-sql/statements/collations)</span><span class="sxs-lookup"><span data-stu-id="56bfb-167">For more information about collations, see [Collations](https://docs.microsoft.com/sql/t-sql/statements/collations)</span></span>

9. <span data-ttu-id="56bfb-168">Klikněte na **Vytvořit**, aby se databáze zřídila.</span><span class="sxs-lookup"><span data-stu-id="56bfb-168">Click **Create** to provision the database.</span></span> <span data-ttu-id="56bfb-169">Zřizování trvá o minutu a půl k dokončení.</span><span class="sxs-lookup"><span data-stu-id="56bfb-169">Provisioning takes about a minute and a half to complete.</span></span> 

10. <span data-ttu-id="56bfb-170">Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.</span><span class="sxs-lookup"><span data-stu-id="56bfb-170">On the toolbar, click **Notifications** to monitor the deployment process.</span></span>

   ![oznámení](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a><span data-ttu-id="56bfb-172">Vytvoření pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-172">Create a server-level firewall rule</span></span>

<span data-ttu-id="56bfb-173">Služba SQL Database vytvoří bránu firewall na úrovni serveru, aby zabránila externím aplikacím a nástrojům v připojení k serveru nebo ke kterékoli databázi na serveru, pokud není vytvořené pravidlo brány firewall k otevření brány firewall pro konkrétní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="56bfb-173">The SQL Database service creates a firewall at the server-level that prevents external applications and tools from connecting to the server or any databases on the server unless a firewall rule is created to open the firewall for specific IP addresses.</span></span> <span data-ttu-id="56bfb-174">Postupujte podle těchto kroků a vytvořte [pravidlo brány firewall na úrovni serveru služby SQL Database](sql-database-firewall-configure.md) pro vaši IP adresu klienta a umožněte externí připojení přes bránu firewall služby SQL Database pouze pro vaši IP adresu.</span><span class="sxs-lookup"><span data-stu-id="56bfb-174">Follow these steps to create a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your client's IP address and enable external connectivity through the SQL Database firewall for your IP address only.</span></span> 

> [!NOTE]
> <span data-ttu-id="56bfb-175">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="56bfb-175">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="56bfb-176">Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="56bfb-176">If you are trying to connect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="56bfb-177">Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="56bfb-177">If so, you cannot connect to your Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

1. <span data-ttu-id="56bfb-178">Po dokončení nasazení klikněte na **Databáze SQL** z nabídky na levé straně a klikněte na **mySampleDatabase** na stránce **Databáze SQL**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-178">After the deployment completes, click **SQL databases** from the left-hand menu and then click **mySampleDatabase** on the **SQL databases** page.</span></span> <span data-ttu-id="56bfb-179">Otevře se stránka s přehledem pro vaši databázi, na které se zobrazí plně kvalifikovaný název serveru (například **mynewserver20170313.database.windows.net**) a možnosti pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56bfb-179">The overview page for your database opens, showing you the fully qualified server name (such as **mynewserver20170313.database.windows.net**) and provides options for further configuration.</span></span> <span data-ttu-id="56bfb-180">Tento plně kvalifikovaný název serveru zkopírujte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="56bfb-180">Copy this fully qualified server name for use later.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="56bfb-181">Tento plně kvalifikovaný název serveru budete potřebovat pro připojení k serveru a jeho databázím v následujících rychlých startech.</span><span class="sxs-lookup"><span data-stu-id="56bfb-181">You need this fully qualified server name to connect to your server and its databases in subsequent quick starts.</span></span>
   > 

   ![název serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

2. <span data-ttu-id="56bfb-183">Klikněte na **Nastavit bránu firewall serveru** na panelu nástrojů, jak je vidět na předchozím obrázku.</span><span class="sxs-lookup"><span data-stu-id="56bfb-183">Click **Set server firewall** on the toolbar as shown in the previous image.</span></span> <span data-ttu-id="56bfb-184">Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56bfb-184">The **Firewall settings** page for the SQL Database server opens.</span></span> 

   ![pravidlo brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule.png) 


3. <span data-ttu-id="56bfb-186">Klikněte na **Přidat IP adresu klienta** na panelu nástrojů a přidejte svoji aktuální IP adresu do nového pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="56bfb-186">Click **Add client IP** on the toolbar to add your current IP address to a new firewall rule.</span></span> <span data-ttu-id="56bfb-187">Pravidlo brány firewall může otevřít port 1433 pro jednu IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="56bfb-187">A firewall rule can open port 1433 for a single IP address or a range of IP addresses.</span></span>

4. <span data-ttu-id="56bfb-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-188">Click **Save**.</span></span> <span data-ttu-id="56bfb-189">Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu, které otevře port 1433 na logickém serveru.</span><span class="sxs-lookup"><span data-stu-id="56bfb-189">A server-level firewall rule is created for your current IP address opening port 1433 on the logical server.</span></span>

   ![nastavení pravidla brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. <span data-ttu-id="56bfb-191">Klikněte na **OK** a pak zavřete stránku **Nastavení brány firewall**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-191">Click **OK** and then close the **Firewall settings** page.</span></span>

<span data-ttu-id="56bfb-192">Nyní se můžete z této IP adresy připojit k serveru SQL Database a jeho databázím pomocí aplikace SQL Server Management Studio nebo jiného nástroje podle vašeho výběru použitím účtu správce serveru vytvořeného dříve.</span><span class="sxs-lookup"><span data-stu-id="56bfb-192">You can now connect to the SQL Database server and its databases using SQL Server Management Studio or another tool of your choice from this IP address using the server admin account created previously.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56bfb-193">Standardně je přístup přes bránu firewall služby SQL Database povolený pro všechny služby Azure.</span><span class="sxs-lookup"><span data-stu-id="56bfb-193">By default, access through the SQL Database firewall is enabled for all Azure services.</span></span> <span data-ttu-id="56bfb-194">Kliknutím na **OFF** na této stránce provedete zákaz pro všechny služby Azure.</span><span class="sxs-lookup"><span data-stu-id="56bfb-194">Click **OFF** on this page to disable for all Azure services.</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="56bfb-195">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-195">SQL server connection information</span></span>

<span data-ttu-id="56bfb-196">Na webu Azure Portal získejte plně kvalifikovaný název serveru služby Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56bfb-196">Get the fully qualified server name for your Azure SQL Database server in the Azure portal.</span></span> <span data-ttu-id="56bfb-197">Plně kvalifikovaný název serveru použijete k připojení k serveru pomocí aplikace SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="56bfb-197">You use the fully qualified server name to connect to your server using SQL Server Management Studio.</span></span>

1. <span data-ttu-id="56bfb-198">Přihlaste se k [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="56bfb-198">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="56bfb-199">V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-199">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="56bfb-200">V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-200">In the **Essentials** pane in the Azure portal page for your database, locate and then copy the **Server name**.</span></span>

   ![informace o připojení](./media/sql-database-connect-query-dotnet/server-name.png)

## <a name="connect-to-the-database-with-ssms"></a><span data-ttu-id="56bfb-202">Připojení k databázi pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="56bfb-202">Connect to the database with SSMS</span></span>

<span data-ttu-id="56bfb-203">Použití [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) k navázání připojení k serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56bfb-203">Use [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) to establish a connection to your Azure SQL Database server.</span></span>

1. <span data-ttu-id="56bfb-204">Otevřete SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="56bfb-204">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="56bfb-205">V dialogovém okně **Připojení k serveru** zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="56bfb-205">In the **Connect to Server** dialog box, enter the following information:</span></span>

   | <span data-ttu-id="56bfb-206">Nastavení</span><span class="sxs-lookup"><span data-stu-id="56bfb-206">Setting</span></span>       | <span data-ttu-id="56bfb-207">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="56bfb-207">Suggested value</span></span> | <span data-ttu-id="56bfb-208">Popis</span><span class="sxs-lookup"><span data-stu-id="56bfb-208">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="56bfb-209">Typ serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-209">Server type</span></span> | <span data-ttu-id="56bfb-210">Databázový stroj</span><span class="sxs-lookup"><span data-stu-id="56bfb-210">Database engine</span></span> | <span data-ttu-id="56bfb-211">Tato hodnota je povinná.</span><span class="sxs-lookup"><span data-stu-id="56bfb-211">This value is required</span></span> |
   | <span data-ttu-id="56bfb-212">Název serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-212">Server name</span></span> | <span data-ttu-id="56bfb-213">Plně kvalifikovaný název serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-213">The fully qualified server name</span></span> | <span data-ttu-id="56bfb-214">Název musí vypadat přibližně takto: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-214">The name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="56bfb-215">Authentication</span><span class="sxs-lookup"><span data-stu-id="56bfb-215">Authentication</span></span> | <span data-ttu-id="56bfb-216">Ověřování SQL Serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-216">SQL Server Authentication</span></span> | <span data-ttu-id="56bfb-217">Ověřování SQL je jediný typ ověřování, který jsme v tomto kurzu nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="56bfb-217">SQL Authentication is the only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="56bfb-218">Přihlásit</span><span class="sxs-lookup"><span data-stu-id="56bfb-218">Login</span></span> | <span data-ttu-id="56bfb-219">Účet správce serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-219">The server admin account</span></span> | <span data-ttu-id="56bfb-220">Jedná se o účet, který jste zadali při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="56bfb-220">This is the account that you specified when you created the server.</span></span> |
   | <span data-ttu-id="56bfb-221">Heslo</span><span class="sxs-lookup"><span data-stu-id="56bfb-221">Password</span></span> | <span data-ttu-id="56bfb-222">Heslo pro účet správce serveru</span><span class="sxs-lookup"><span data-stu-id="56bfb-222">The password for your server admin account</span></span> | <span data-ttu-id="56bfb-223">Jedná se o heslo, které jste zadali při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="56bfb-223">This is the password that you specified when you created the server.</span></span> |

   ![Připojení k serveru](./media/sql-database-connect-query-ssms/connect.png)

3. <span data-ttu-id="56bfb-225">Klikněte na **Možnosti** v dialogovém okně **Připojit k serveru**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-225">Click **Options** in the **Connect to server** dialog box.</span></span> <span data-ttu-id="56bfb-226">V části **Připojit k databázi** zadejte **mySampleDatabase**, abyste se připojili k této databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-226">In the **Connect to database** section, enter **mySampleDatabase** to connect to this database.</span></span>

   ![připojení k databázi na serveru](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. <span data-ttu-id="56bfb-228">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-228">Click **Connect**.</span></span> <span data-ttu-id="56bfb-229">V aplikaci SSMS se otevře okno Průzkumníka objektů.</span><span class="sxs-lookup"><span data-stu-id="56bfb-229">The Object Explorer window opens in SSMS.</span></span> 

5. <span data-ttu-id="56bfb-230">V Průzkumníku objektů zobrazte objekty v ukázkové databázi rozbalením **Databáze** a potom **mySampleDatabase**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-230">In Object Explorer, expand **Databases** and then expand **mySampleDatabase** to view the objects in the sample database.</span></span>

   ![Objekty databáze.](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="create-tables-in-the-database"></a><span data-ttu-id="56bfb-232">Vytváření tabulek v databázi</span><span class="sxs-lookup"><span data-stu-id="56bfb-232">Create tables in the database</span></span> 

<span data-ttu-id="56bfb-233">Vytvořte schéma databáze s čtyři tabulek, které model student systém správy pro použití vysoké školy [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):</span><span class="sxs-lookup"><span data-stu-id="56bfb-233">Create a database schema with four tables that model a student management system for universities using [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):</span></span>

- <span data-ttu-id="56bfb-234">Osoba</span><span class="sxs-lookup"><span data-stu-id="56bfb-234">Person</span></span>
- <span data-ttu-id="56bfb-235">Kurz</span><span class="sxs-lookup"><span data-stu-id="56bfb-235">Course</span></span>
- <span data-ttu-id="56bfb-236">Studenty</span><span class="sxs-lookup"><span data-stu-id="56bfb-236">Student</span></span>
- <span data-ttu-id="56bfb-237">Úvěrového tohoto modelu systém správy student pro vysoké školy</span><span class="sxs-lookup"><span data-stu-id="56bfb-237">Credit that model a student management system for universities</span></span>

<span data-ttu-id="56bfb-238">Následující diagram znázorňuje, jak tyto tabulky jsou vzájemně souvisí.</span><span class="sxs-lookup"><span data-stu-id="56bfb-238">The following diagram shows how these tables are related to each other.</span></span> <span data-ttu-id="56bfb-239">Některé z těchto tabulek odkazovat na sloupce v jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="56bfb-239">Some of these tables reference columns in other tables.</span></span> <span data-ttu-id="56bfb-240">Například studenty tabulka odkazuje **PersonId** sloupec **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="56bfb-240">For example, the Student table references the **PersonId** column of the **Person** table.</span></span> <span data-ttu-id="56bfb-241">Studie diagramu na pochopit, jak jsou vzájemně propojeny v tabulkách v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="56bfb-241">Study the diagram to understand how the tables in this tutorial are related to one another.</span></span> <span data-ttu-id="56bfb-242">Podrobný rozbor toho, jak vytvořit efektivní databázové tabulky, najdete v části [vytvořit efektivní databázových tabulek](https://msdn.microsoft.com/library/cc505842.aspx).</span><span class="sxs-lookup"><span data-stu-id="56bfb-242">For an in-depth look at how to create effective database tables, see [Create effective database tables](https://msdn.microsoft.com/library/cc505842.aspx).</span></span> <span data-ttu-id="56bfb-243">Informace o výběru datových typů najdete v tématu [datové typy](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="56bfb-243">For information about choosing data types, see [Data types](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).</span></span>

> [!NOTE]
> <span data-ttu-id="56bfb-244">Můžete také [návrháře tabulky v aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) vytvoření a tabulek.</span><span class="sxs-lookup"><span data-stu-id="56bfb-244">You can also use the [table designer in SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) to create and design your tables.</span></span> 

![Relace mezi tabulkami](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. <span data-ttu-id="56bfb-246">V Průzkumníku objektů klikněte pravým tlačítkem na **mySampleDatabase** a potom klikněte na **Nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="56bfb-246">In Object Explorer, right-click **mySampleDatabase** and click **New Query**.</span></span> <span data-ttu-id="56bfb-247">Otevře se prázdné okno dotazu připojené k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-247">A blank query window opens that is connected to your database.</span></span>

2. <span data-ttu-id="56bfb-248">V okně dotazu spusťte následující dotaz, který vytvořit čtyři tabulky v databázi:</span><span class="sxs-lookup"><span data-stu-id="56bfb-248">In the query window, execute the following query to create four tables in your database:</span></span> 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Vytváření tabulek](./media/sql-database-design-first-database/create-tables.png)

3. <span data-ttu-id="56bfb-250">Rozbalte uzel 'tabulky' v Průzkumníku objekt SQL Server Management Studio zobrazíte tabulky, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="56bfb-250">Expand the 'tables' node in the SQL Server Management Studio Object explorer to see the tables you created.</span></span>

   ![vytvoření tabulek aplikace ssms](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a><span data-ttu-id="56bfb-252">Načtení dat do tabulky</span><span class="sxs-lookup"><span data-stu-id="56bfb-252">Load data into the tables</span></span>

1. <span data-ttu-id="56bfb-253">Vytvořte složku s názvem **SampleTableData** ve složce soubory ke stažení pro uložení ukázkových dat pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-253">Create a folder called **SampleTableData** in your Downloads folder to store sample data for your database.</span></span> 

2. <span data-ttu-id="56bfb-254">Klikněte pravým tlačítkem na následující odkazy a uložit je do **SampleTableData** složky.</span><span class="sxs-lookup"><span data-stu-id="56bfb-254">Right-click the following links and save them into the **SampleTableData** folder.</span></span> 

   - [<span data-ttu-id="56bfb-255">SampleCourseData</span><span class="sxs-lookup"><span data-stu-id="56bfb-255">SampleCourseData</span></span>](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [<span data-ttu-id="56bfb-256">SamplePersonData</span><span class="sxs-lookup"><span data-stu-id="56bfb-256">SamplePersonData</span></span>](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [<span data-ttu-id="56bfb-257">SampleStudentData</span><span class="sxs-lookup"><span data-stu-id="56bfb-257">SampleStudentData</span></span>](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [<span data-ttu-id="56bfb-258">SampleCreditData</span><span class="sxs-lookup"><span data-stu-id="56bfb-258">SampleCreditData</span></span>](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. <span data-ttu-id="56bfb-259">Otevřete okno příkazového řádku a přejděte do složky SampleTableData.</span><span class="sxs-lookup"><span data-stu-id="56bfb-259">Open a command prompt window and navigate to the SampleTableData folder.</span></span>

4. <span data-ttu-id="56bfb-260">Spuštěním následujících příkazů vložení ukázková data do tabulky nahrazení hodnoty **ServerName**, **DatabaseName**, **uživatelské jméno**, a  **Heslo** s hodnotami pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="56bfb-260">Execute the following commands to insert sample data into the tables replacing the values for **ServerName**, **DatabaseName**, **UserName**, and **Password** with the values for your environment.</span></span>
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

<span data-ttu-id="56bfb-261">Ukázková data mají nyní načtena do tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="56bfb-261">You have now loaded sample data into the tables you created earlier.</span></span>

## <a name="query-data"></a><span data-ttu-id="56bfb-262">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="56bfb-262">Query data</span></span>

<span data-ttu-id="56bfb-263">Spusťte tyto dotazy k načtení informací z tabulek databáze.</span><span class="sxs-lookup"><span data-stu-id="56bfb-263">Execute the following queries to retrieve information from the database tables.</span></span> <span data-ttu-id="56bfb-264">V tématu [zápis dotazů SQL](https://technet.microsoft.com/library/bb264565.aspx) Další informace o vytváření dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="56bfb-264">See [Writing SQL Queries](https://technet.microsoft.com/library/bb264565.aspx) to learn more about writing SQL queries.</span></span> <span data-ttu-id="56bfb-265">První dotaz spojí všechny čtyři tabulky najít všechny studenti výukové podle ' Dominick Pope', kteří mají vyšší než 75 % úrovni ve své třídě.</span><span class="sxs-lookup"><span data-stu-id="56bfb-265">The first query joins all four tables to find all the students taught by 'Dominick Pope' who have a grade higher than 75% in his class.</span></span> <span data-ttu-id="56bfb-266">Druhý dotaz spojuje všechny čtyři tabulky a vyhledá všechny kurzy, ve kterých má někdy zapsaná 'Noe Colemane'.</span><span class="sxs-lookup"><span data-stu-id="56bfb-266">The second query joins all four tables and finds all courses in which 'Noe Coleman' has ever enrolled.</span></span>

1. <span data-ttu-id="56bfb-267">V okně dotazu SQL Server Management Studio spusťte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="56bfb-267">In a SQL Server Management Studio query window, execute the following query:</span></span>

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. <span data-ttu-id="56bfb-268">V okně dotazu SQL Server Management Studio spusťte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="56bfb-268">In a SQL Server Management Studio query window, execute following query:</span></span>

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a><span data-ttu-id="56bfb-269">Obnovení databáze k dřívějšímu bodu v čase</span><span class="sxs-lookup"><span data-stu-id="56bfb-269">Restore a database to a previous point in time</span></span>

<span data-ttu-id="56bfb-270">Představte si, že jste omylem odstranili tabulku.</span><span class="sxs-lookup"><span data-stu-id="56bfb-270">Imagine you have accidentally deleted a table.</span></span> <span data-ttu-id="56bfb-271">Toto je něco, které nelze snadno obnovit z.</span><span class="sxs-lookup"><span data-stu-id="56bfb-271">This is something you cannot easily recover from.</span></span> <span data-ttu-id="56bfb-272">Databáze SQL Azure můžete přejít zpět do libovolného bodu v čase v poslední až do 35 dní a obnovit tento bod v čase pro novou databázi.</span><span class="sxs-lookup"><span data-stu-id="56bfb-272">Azure SQL Database allows you to go back to any point in time in the last up to 35 days and restore this point in time to a new database.</span></span> <span data-ttu-id="56bfb-273">Můžete tuto databázi obnovit odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="56bfb-273">You can you this database to recover your deleted data.</span></span> <span data-ttu-id="56bfb-274">Následující kroky obnovení ukázkové databáze do bodu předtím, než byly přidány tabulky.</span><span class="sxs-lookup"><span data-stu-id="56bfb-274">The following steps restore the sample database to a point before the tables were added.</span></span>

1. <span data-ttu-id="56bfb-275">Na stránce databáze SQL pro vaši databázi, klikněte na **obnovení** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="56bfb-275">On the SQL Database page for your database, click **Restore** on the toolbar.</span></span> <span data-ttu-id="56bfb-276">**Obnovení** otevře se stránka.</span><span class="sxs-lookup"><span data-stu-id="56bfb-276">The **Restore** page opens.</span></span>

   ![Obnovení](./media/sql-database-design-first-database/restore.png)

2. <span data-ttu-id="56bfb-278">Vyplňte **obnovení** formuláře se požadované informace:</span><span class="sxs-lookup"><span data-stu-id="56bfb-278">Fill out the **Restore** form with the required information:</span></span>
    * <span data-ttu-id="56bfb-279">Název databáze: Zadejte název databáze.</span><span class="sxs-lookup"><span data-stu-id="56bfb-279">Database name: Provide a database name</span></span> 
    * <span data-ttu-id="56bfb-280">Bod v čase: Vyberte **v daném okamžiku** kartu na formuláři obnovení</span><span class="sxs-lookup"><span data-stu-id="56bfb-280">Point-in-time: Select the **Point-in-time** tab on the Restore form</span></span> 
    * <span data-ttu-id="56bfb-281">Bod obnovení: vyberte čas, ke kterému dochází před změnou databáze</span><span class="sxs-lookup"><span data-stu-id="56bfb-281">Restore point: Select a time that occurs before the database was changed</span></span>
    * <span data-ttu-id="56bfb-282">Cílový server: tuto hodnotu nelze změnit, při obnovení databáze</span><span class="sxs-lookup"><span data-stu-id="56bfb-282">Target server: You cannot change this value when restoring a database</span></span> 
    * <span data-ttu-id="56bfb-283">Fond elastické databáze: vyberte **None**</span><span class="sxs-lookup"><span data-stu-id="56bfb-283">Elastic database pool: Select **None**</span></span>  
    * <span data-ttu-id="56bfb-284">Cenová úroveň: vyberte **20 Dtu** a **250 GB** úložiště.</span><span class="sxs-lookup"><span data-stu-id="56bfb-284">Pricing tier: Select **20 DTUs** and **250 GB** of storage.</span></span>

   ![bod obnovení](./media/sql-database-design-first-database/restore-point.png)

3. <span data-ttu-id="56bfb-286">Klikněte na tlačítko **OK** k obnovení databázi do [obnovit k určitému bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) předtím, než byly přidány tabulky.</span><span class="sxs-lookup"><span data-stu-id="56bfb-286">Click **OK** to restore the database to [restore to a point in time](sql-database-recovery-using-backups.md#point-in-time-restore) before the tables were added.</span></span> <span data-ttu-id="56bfb-287">Obnovení databáze do jiného bodu v čase vytvoří duplicitní databázi na stejném serveru jako původní databáze od bodu v čase, které zadáte, dokud je v rámci dobu uchování vašeho [vrstvy služby](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="56bfb-287">Restoring a database to a different point in time creates a duplicate database in the same server as the original database as of the point in time you specify, as long as it is within the retention period for your [service tier](sql-database-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56bfb-288">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56bfb-288">Next Steps</span></span> 
<span data-ttu-id="56bfb-289">V tomto kurzu jste se dozvěděli, že databáze basic úlohy, jako například vytvořit databáze a tabulky, načítat a zadávat dotazy na data a obnovit databázi do předchozího bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="56bfb-289">In this tutorial, you learned basic database tasks such as create a database and tables, load and query data, and restore the database to a previous point in time.</span></span> <span data-ttu-id="56bfb-290">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="56bfb-290">You learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="56bfb-291">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="56bfb-291">Create a database</span></span>
> * <span data-ttu-id="56bfb-292">Nastavit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="56bfb-292">Set up a firewall rule</span></span>
> * <span data-ttu-id="56bfb-293">Připojení k databázi s [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)</span><span class="sxs-lookup"><span data-stu-id="56bfb-293">Connect to the database with [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)</span></span>
> * <span data-ttu-id="56bfb-294">Vytváření tabulek</span><span class="sxs-lookup"><span data-stu-id="56bfb-294">Create tables</span></span>
> * <span data-ttu-id="56bfb-295">Hromadné načítání dat</span><span class="sxs-lookup"><span data-stu-id="56bfb-295">Bulk load data</span></span>
> * <span data-ttu-id="56bfb-296">Dotaz na data</span><span class="sxs-lookup"><span data-stu-id="56bfb-296">Query that data</span></span>
> * <span data-ttu-id="56bfb-297">Obnovení databáze do předchozího bodu v čase pomocí SQL Database [obnovení bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) možnosti</span><span class="sxs-lookup"><span data-stu-id="56bfb-297">Restore the database to a previous point in time using SQL Database [point in time restore](sql-database-recovery-using-backups.md#point-in-time-restore) capabilities</span></span>

<span data-ttu-id="56bfb-298">Přechodu na v dalším kurzu se dozvíte o návrhu databáze pomocí sady Visual Studio a C#.</span><span class="sxs-lookup"><span data-stu-id="56bfb-298">Advance to the next tutorial to learn about designing a database using Visual Studio and C#.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="56bfb-299">Návrh Azure SQL database a připojení s C# a ADO.NET</span><span class="sxs-lookup"><span data-stu-id="56bfb-299">Design an Azure SQL database and connect with C# and ADO.NET</span></span>](sql-database-design-first-database-csharp.md)
