---
title: "Azure Portal: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Zjistěte, jak hello toocreate logického serveru SQL Database, pravidla brány firewall na úrovni serveru a databází v portálu Azure. Také zjistíte tooquery Azure SQL database pomocí portálu Azure hello."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 05/30/2017
ms.author: carlrab
ms.openlocfilehash: d30352d834f2007e0b6b3eabfc3c108c61479b22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-sql-database-in-hello-azure-portal"></a><span data-ttu-id="c023c-105">Vytvoření Azure SQL database v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c023c-105">Create an Azure SQL database in hello Azure portal</span></span>

<span data-ttu-id="c023c-106">Tento úvodní kurz vás provede jak toocreate SQL databáze v Azure.</span><span class="sxs-lookup"><span data-stu-id="c023c-106">This quick start tutorial walks through how toocreate a SQL database in Azure.</span></span> <span data-ttu-id="c023c-107">Databáze SQL Azure je "Databáze jako služba" nabídka, která umožňuje toorun a škálování vysoce dostupné databáze systému SQL Server v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-107">Azure SQL Database is a “Database-as-a-Service” offering that enables you toorun and scale highly available SQL Server databases in hello cloud.</span></span> <span data-ttu-id="c023c-108">Tento úvodní ukazuje, jak tooget spuštěna vytvořením SQL database pomocí portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-108">This quick start shows you how tooget started by creating a SQL database using hello Azure portal.</span></span>

<span data-ttu-id="c023c-109">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="c023c-109">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="c023c-110">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c023c-110">Log in toohello Azure portal</span></span>

<span data-ttu-id="c023c-111">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c023c-111">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="c023c-112">Vytvoření databáze SQL</span><span class="sxs-lookup"><span data-stu-id="c023c-112">Create a SQL database</span></span>

<span data-ttu-id="c023c-113">Databáze SQL Azure se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="c023c-113">An Azure SQL database is created with a defined set of [compute and storage resources](sql-database-service-tiers.md).</span></span> <span data-ttu-id="c023c-114">Hello databáze byla vytvořena v rámci [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) a v [logického serveru Azure SQL Database](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="c023c-114">hello database is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) and in an [Azure SQL Database logical server](sql-database-features.md).</span></span> 

<span data-ttu-id="c023c-115">Postupujte podle těchto kroků toocreate obsahující hello společnosti Adventure Works LT ukázková data databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="c023c-115">Follow these steps toocreate a SQL database containing hello Adventure Works LT sample data.</span></span> 

1. <span data-ttu-id="c023c-116">Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c023c-116">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="c023c-117">Vyberte **databáze** z hello **nový** a vyberte **SQL Database** z hello **databáze** stránky.</span><span class="sxs-lookup"><span data-stu-id="c023c-117">Select **Databases** from hello **New** page, and select **SQL Database** from hello **Databases** page.</span></span>

   ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

3. <span data-ttu-id="c023c-119">Vyplňte hello SQL Database formulář s hello následující informace, jak je znázorněno na hello předcházející bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="c023c-119">Fill out hello SQL Database form with hello following information, as shown on hello preceding image:</span></span>   

   | <span data-ttu-id="c023c-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c023c-120">Setting</span></span>       | <span data-ttu-id="c023c-121">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="c023c-121">Suggested value</span></span> | <span data-ttu-id="c023c-122">Popis</span><span class="sxs-lookup"><span data-stu-id="c023c-122">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="c023c-123">**Název databáze**</span><span class="sxs-lookup"><span data-stu-id="c023c-123">**Database name**</span></span> | <span data-ttu-id="c023c-124">mySampleDatabase</span><span class="sxs-lookup"><span data-stu-id="c023c-124">mySampleDatabase</span></span> | <span data-ttu-id="c023c-125">Platné názvy databází najdete v tématu [Identifikátory databází](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="c023c-125">For valid database names, see [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers).</span></span> | 
   | <span data-ttu-id="c023c-126">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="c023c-126">**Subscription**</span></span> | <span data-ttu-id="c023c-127">Vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="c023c-127">Your subscription</span></span>  | <span data-ttu-id="c023c-128">Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="c023c-128">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="c023c-129">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="c023c-129">**Resource group**</span></span>  | <span data-ttu-id="c023c-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c023c-130">myResourceGroup</span></span> | <span data-ttu-id="c023c-131">Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="c023c-131">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="c023c-132">**Zdroj prostředku**</span><span class="sxs-lookup"><span data-stu-id="c023c-132">**Source source**</span></span> | <span data-ttu-id="c023c-133">Ukázka (AdventureWorksLT)</span><span class="sxs-lookup"><span data-stu-id="c023c-133">Sample (AdventureWorksLT)</span></span> | <span data-ttu-id="c023c-134">Načte hello AdventureWorksLT schéma a data do nové databáze</span><span class="sxs-lookup"><span data-stu-id="c023c-134">Loads hello AdventureWorksLT schema and data into your new database</span></span> |

   > [!IMPORTANT]
   > <span data-ttu-id="c023c-135">Hello ukázkové databáze na tomto formuláři je třeba vybrat, protože je používán v hello zbytek této úvodní.</span><span class="sxs-lookup"><span data-stu-id="c023c-135">You must select hello sample database on this form because it is used in hello remainder of this quick start.</span></span>
   > 

4. <span data-ttu-id="c023c-136">V části **Server**, klikněte na tlačítko **konfigurovat požadované nastavení** a výplně se hello SQL server (logický server) formuláře s hello následující informace, jak je znázorněno na hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="c023c-136">Under **Server**, click **Configure required settings** and fill out hello SQL server (logical server) form with hello following information, as shown on hello following image:</span></span>   

   | <span data-ttu-id="c023c-137">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c023c-137">Setting</span></span>       | <span data-ttu-id="c023c-138">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="c023c-138">Suggested value</span></span> | <span data-ttu-id="c023c-139">Popis</span><span class="sxs-lookup"><span data-stu-id="c023c-139">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="c023c-140">**Název serveru**</span><span class="sxs-lookup"><span data-stu-id="c023c-140">**Server name**</span></span> | <span data-ttu-id="c023c-141">Libovolný globálně jedinečný název</span><span class="sxs-lookup"><span data-stu-id="c023c-141">Any globally unique name</span></span> | <span data-ttu-id="c023c-142">Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="c023c-142">For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> | 
   | <span data-ttu-id="c023c-143">**Přihlašovací jméno správce serveru**</span><span class="sxs-lookup"><span data-stu-id="c023c-143">**Server admin login**</span></span> | <span data-ttu-id="c023c-144">Libovolné platné jméno</span><span class="sxs-lookup"><span data-stu-id="c023c-144">Any valid name</span></span> | <span data-ttu-id="c023c-145">Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="c023c-145">For valid login names, see [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers).</span></span> |
   | <span data-ttu-id="c023c-146">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="c023c-146">**Password**</span></span> | <span data-ttu-id="c023c-147">Libovolné platné heslo</span><span class="sxs-lookup"><span data-stu-id="c023c-147">Any valid password</span></span> | <span data-ttu-id="c023c-148">Heslo musí mít aspoň 8 znaků a musí obsahovat znaky ze tří z následujících kategorií hello: velká písmena, malá písmena, čísla, a a jiné než alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="c023c-148">Your password must have at least 8 characters and must contain characters from three of hello following categories: upper case characters, lower case characters, numbers, and and non-alphanumeric characters.</span></span> |
   | <span data-ttu-id="c023c-149">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="c023c-149">**Subscription**</span></span> | <span data-ttu-id="c023c-150">Vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="c023c-150">Your subscription</span></span> | <span data-ttu-id="c023c-151">Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="c023c-151">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="c023c-152">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="c023c-152">**Resource group**</span></span> | <span data-ttu-id="c023c-153">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c023c-153">myResourceGroup</span></span> | <span data-ttu-id="c023c-154">Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="c023c-154">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="c023c-155">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="c023c-155">**Location**</span></span> | <span data-ttu-id="c023c-156">Libovolné platné umístění</span><span class="sxs-lookup"><span data-stu-id="c023c-156">Any valid location</span></span> | <span data-ttu-id="c023c-157">Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="c023c-157">For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/).</span></span> |

   > [!IMPORTANT]
   > <span data-ttu-id="c023c-158">Hello přihlašovací jméno správce serveru a heslo, které tady zadáte jsou požadované toolog toohello serveru a její databáze dále v této úvodní.</span><span class="sxs-lookup"><span data-stu-id="c023c-158">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="c023c-159">Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="c023c-159">Remember or record this information for later use.</span></span> 
   >  

   ![create database-server](./media/sql-database-get-started-portal/create-database-server.png)

5. <span data-ttu-id="c023c-161">Pokud jste vyplnili formulář hello, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="c023c-161">When you have completed hello form, click **Select**.</span></span>

6. <span data-ttu-id="c023c-162">Klikněte na tlačítko **cenová úroveň** toospecify hello služby vrstvy a úroveň výkonu pro novou databázi.</span><span class="sxs-lookup"><span data-stu-id="c023c-162">Click **Pricing tier** toospecify hello service tier and performance level for your new database.</span></span> <span data-ttu-id="c023c-163">Pomocí posuvníku tooselect hello **20 Dtu** a **250** GB úložiště.</span><span class="sxs-lookup"><span data-stu-id="c023c-163">Use hello slider tooselect **20 DTUs** and **250** GB of storage.</span></span> <span data-ttu-id="c023c-164">Další informace o jednotkách DTU najdete v tématu [Co je DTU](sql-database-what-is-a-dtu.md).</span><span class="sxs-lookup"><span data-stu-id="c023c-164">For more information on DTUs, see [What is a DTU?](sql-database-what-is-a-dtu.md).</span></span>

   ![create database-s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. <span data-ttu-id="c023c-166">Po vybrané hello množství Dtu, klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="c023c-166">After selected hello amount of DTUs, click **Apply**.</span></span>  

8. <span data-ttu-id="c023c-167">Teď, když jste dokončili formuláře hello databáze SQL, klikněte na tlačítko **vytvořit** tooprovision hello databáze.</span><span class="sxs-lookup"><span data-stu-id="c023c-167">Now that you have completed hello SQL Database form, click **Create** tooprovision hello database.</span></span> <span data-ttu-id="c023c-168">Zřizování trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="c023c-168">Provisioning takes a few minutes.</span></span> 

9. <span data-ttu-id="c023c-169">Na panelu nástrojů hello, klikněte na tlačítko **oznámení** procesu nasazení toomonitor hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-169">On hello toolbar, click **Notifications** toomonitor hello deployment process.</span></span>

   ![oznámení](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a><span data-ttu-id="c023c-171">Vytvoření pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="c023c-171">Create a server-level firewall rule</span></span>

<span data-ttu-id="c023c-172">Hello služba SQL Database vytvoří brána firewall na serveru – úrovni hello, které zabrání připojení toohello serveru nebo kterékoli databázi na serveru hello, pokud je vytvořeno pravidlo brány firewall tooopen hello brány firewall pro konkrétní IP adresy externí aplikace a nástroje.</span><span class="sxs-lookup"><span data-stu-id="c023c-172">hello SQL Database service creates a firewall at hello server-level that prevents external applications and tools from connecting toohello server or any databases on hello server unless a firewall rule is created tooopen hello firewall for specific IP addresses.</span></span> <span data-ttu-id="c023c-173">Postupujte podle těchto kroků toocreate [pravidlo brány firewall na úrovni serveru SQL Database](sql-database-firewall-configure.md) pro vašeho klienta IP adres a povolte externí připojení přes firewall hello databáze SQL pro vaše IP adresa.</span><span class="sxs-lookup"><span data-stu-id="c023c-173">Follow these steps toocreate a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your client's IP address and enable external connectivity through hello SQL Database firewall for your IP address only.</span></span> 

> [!NOTE]
> <span data-ttu-id="c023c-174">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="c023c-174">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="c023c-175">Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="c023c-175">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="c023c-176">Pokud ano, nemůžete připojit tooyour serveru Azure SQL Database, pokud vaše IT oddělení otevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="c023c-176">If so, you cannot connect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

1. <span data-ttu-id="c023c-177">Po dokončení hello nasazení, klikněte na tlačítko **databází SQL** z nabídky na levé straně hello a pak klikněte na tlačítko **mySampleDatabase** na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="c023c-177">After hello deployment completes, click **SQL databases** from hello left-hand menu and then click **mySampleDatabase** on hello **SQL databases** page.</span></span> <span data-ttu-id="c023c-178">Hello přehledová stránka otevře vaší databáze, zobrazující text hello plně kvalifikovaný název serveru (například **mynewserver20170313.database.windows.net**) a poskytuje možnosti pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c023c-178">hello overview page for your database opens, showing you hello fully qualified server name (such as **mynewserver20170313.database.windows.net**) and provides options for further configuration.</span></span> <span data-ttu-id="c023c-179">Tento plně kvalifikovaný název serveru zkopírujte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="c023c-179">Copy this fully qualified server name for use later.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="c023c-180">Je nutné tento plně kvalifikovaný název tooconnect tooyour serveru a její databáze v následných rychlé zahájení.</span><span class="sxs-lookup"><span data-stu-id="c023c-180">You need this fully qualified server name tooconnect tooyour server and its databases in subsequent quick starts.</span></span>
   > 

   ![název serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

2. <span data-ttu-id="c023c-182">Klikněte na tlačítko **nastavení brány firewall serveru** na panelu nástrojů hello viz předchozí obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-182">Click **Set server firewall** on hello toolbar as shown in hello previous image.</span></span> <span data-ttu-id="c023c-183">Hello **nastavení brány Firewall** otevře se stránka pro hello databáze SQL server.</span><span class="sxs-lookup"><span data-stu-id="c023c-183">hello **Firewall settings** page for hello SQL Database server opens.</span></span> 

   ![pravidlo brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule.png) 

3. <span data-ttu-id="c023c-185">Klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů tooadd hello vaše aktuální IP adres tooa nové pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="c023c-185">Click **Add client IP** on hello toolbar tooadd your current IP address tooa new firewall rule.</span></span> <span data-ttu-id="c023c-186">Pravidlo brány firewall může otevřít port 1433 pro jednu IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="c023c-186">A firewall rule can open port 1433 for a single IP address or a range of IP addresses.</span></span>

4. <span data-ttu-id="c023c-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c023c-187">Click **Save**.</span></span> <span data-ttu-id="c023c-188">Pro aktuální IP adrese otevřít port 1433 na logickém serveru hello je vytvořeno pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="c023c-188">A server-level firewall rule is created for your current IP address opening port 1433 on hello logical server.</span></span>

   ![nastavení pravidla brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. <span data-ttu-id="c023c-190">Klikněte na tlačítko **OK** a pak zavřete hello **nastavení brány Firewall** stránky.</span><span class="sxs-lookup"><span data-stu-id="c023c-190">Click **OK** and then close hello **Firewall settings** page.</span></span>

<span data-ttu-id="c023c-191">Teď se můžete připojit toohello databáze SQL server a její databáze pomocí SQL Server Management Studio nebo jiný nástroj podle svého výběru z tuto IP adresu pomocí účtu správce serveru hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c023c-191">You can now connect toohello SQL Database server and its databases using SQL Server Management Studio or another tool of your choice from this IP address using hello server admin account created previously.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c023c-192">Standardně je povolen přístup přes bránu firewall hello databáze SQL pro všechny služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c023c-192">By default, access through hello SQL Database firewall is enabled for all Azure services.</span></span> <span data-ttu-id="c023c-193">Klikněte na tlačítko **OFF** na této stránce toodisable pro všechny služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c023c-193">Click **OFF** on this page toodisable for all Azure services.</span></span>
>

## <a name="query-hello-sql-database"></a><span data-ttu-id="c023c-194">Hello dotaz do databáze SQL</span><span class="sxs-lookup"><span data-stu-id="c023c-194">Query hello SQL database</span></span>

<span data-ttu-id="c023c-195">Teď, když jste vytvořili ukázkové databáze v Azure, můžeme nástrojem hello integrovaného dotazu v rámci hello Azure portálu tooconfirm, že se můžete připojit toohello databáze a dotaz hello data.</span><span class="sxs-lookup"><span data-stu-id="c023c-195">Now that you have created a sample database in Azure, let’s use hello built-in query tool within hello Azure portal tooconfirm that you can connect toohello database and query hello data.</span></span> 

1. <span data-ttu-id="c023c-196">Na stránce hello databáze SQL pro vaši databázi, klikněte na **nástroje** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-196">On hello SQL Database page for your database, click **Tools** on hello toolbar.</span></span> <span data-ttu-id="c023c-197">Hello **nástroje** otevře se stránka.</span><span class="sxs-lookup"><span data-stu-id="c023c-197">hello **Tools** page opens.</span></span>

   ![nabídka nástroje](./media/sql-database-get-started-portal/tools-menu.png) 

2. <span data-ttu-id="c023c-199">Klikněte na tlačítko **editor dotazů (preview)**, klikněte na tlačítko hello **Náhled podmínky** zaškrtávací políčko a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c023c-199">Click **Query editor (preview)**, click hello **Preview terms** checkbox, and then click **OK**.</span></span> <span data-ttu-id="c023c-200">Otevře se stránka editor dotazů Hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-200">hello Query editor page opens.</span></span>

3. <span data-ttu-id="c023c-201">Klikněte na tlačítko **přihlášení** a po zobrazení výzvy vyberte **ověřování systému SQL server** a pak zadejte přihlašovací jméno správce serveru hello a heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c023c-201">Click **Login** and then, when prompted, select **SQL server authentication** and then provide hello server admin login and password that you created earlier.</span></span>

   ![přihlášení](./media/sql-database-get-started-portal/login.png) 

4. <span data-ttu-id="c023c-203">Klikněte na tlačítko **OK** toolog v.</span><span class="sxs-lookup"><span data-stu-id="c023c-203">Click **OK** toolog in.</span></span>

5. <span data-ttu-id="c023c-204">Když jste se ověřili, zadejte následující dotaz v podokně editor dotazů hello hello.</span><span class="sxs-lookup"><span data-stu-id="c023c-204">After you are authenticated, type hello following query in hello query editor pane.</span></span>

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. <span data-ttu-id="c023c-205">Klikněte na tlačítko **spustit** a poté zkontrolovat výsledky dotazu hello v hello **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="c023c-205">Click **Run** and then review hello query results in hello **Results** pane.</span></span>

   ![výsledky editoru dotazů](./media/sql-database-get-started-portal/query-editor-results.png)

7. <span data-ttu-id="c023c-207">Zavřít hello **editor dotazů** stránku a hello **nástroje** stránky.</span><span class="sxs-lookup"><span data-stu-id="c023c-207">Close hello **Query editor** page and hello **Tools** page.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="c023c-208">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="c023c-208">Clean up resources</span></span>

<span data-ttu-id="c023c-209">Pokud nepotřebujete tyto prostředky pro jiné rychlý start nebo kurzu (viz [další kroky](#next-steps)), můžete je odstranit provedením následujících hello:</span><span class="sxs-lookup"><span data-stu-id="c023c-209">If you don't need these resources for another quickstart/tutorial (see [Next steps](#next-steps)), you can delete them by doing hello following:</span></span>


1. <span data-ttu-id="c023c-210">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="c023c-210">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click **myResourceGroup**.</span></span> 
2. <span data-ttu-id="c023c-211">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**, typ **myResourceGroup** v hello textového pole a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c023c-211">On your resource group page, click **Delete**, type **myResourceGroup** in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c023c-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c023c-212">Next steps</span></span>

<span data-ttu-id="c023c-213">Teď, když máte databázi, můžete se k ní připojit a provádět dotazování pomocí vašich oblíbených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c023c-213">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="c023c-214">Další informace získáte výběrem vašeho nástroje níže:</span><span class="sxs-lookup"><span data-stu-id="c023c-214">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="c023c-215">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="c023c-215">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="c023c-216">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c023c-216">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="c023c-217">.NET</span><span class="sxs-lookup"><span data-stu-id="c023c-217">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="c023c-218">PHP</span><span class="sxs-lookup"><span data-stu-id="c023c-218">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="c023c-219">Node.js</span><span class="sxs-lookup"><span data-stu-id="c023c-219">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="c023c-220">Java</span><span class="sxs-lookup"><span data-stu-id="c023c-220">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="c023c-221">Python</span><span class="sxs-lookup"><span data-stu-id="c023c-221">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="c023c-222">Ruby</span><span class="sxs-lookup"><span data-stu-id="c023c-222">Ruby</span></span>](sql-database-connect-query-ruby.md)
