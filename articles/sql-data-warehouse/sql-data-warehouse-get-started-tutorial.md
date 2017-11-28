---
title: "Začínáme aaaAzure SQL Data Warehouse – kurz | Microsoft Docs"
description: "V tomto kurzu se naučíte, jak tooprovision a načtení dat do Azure SQL Data Warehouse. Taky poznáte hello základní informace o škálování, pozastavení a ladění."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: edd2a21b0fe49ca8e9792c7c512310339a822c55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-data-warehouse"></a><span data-ttu-id="c4af0-104">Začínáme s SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c4af0-104">Get started with SQL Data Warehouse</span></span>

<span data-ttu-id="c4af0-105">Tento kurz ukazuje, jak tooprovision a načtení dat do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-105">This tutorial shows how tooprovision and load data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="c4af0-106">Taky poznáte hello základní informace o škálování, pozastavení a ladění.</span><span class="sxs-lookup"><span data-stu-id="c4af0-106">You’ll also learn hello basics about scaling, pausing, and tuning.</span></span> <span data-ttu-id="c4af0-107">Jakmile budete hotovi, budete mít připravené tooquery a prozkoumat datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-107">When you’re finished, you’ll be ready tooquery and explore your data warehouse.</span></span>

<span data-ttu-id="c4af0-108">**Odhadovaný čas toocomplete:** Toto je začátku do konce kurz s ukázkový kód, který trvá asi 30 minut toocomplete po jste splnili požadavky hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-108">**Estimated time toocomplete:** This is an end-to-end tutorial with example code that takes about 30 minutes toocomplete once you have met hello prerequisites.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c4af0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4af0-109">Prerequisites</span></span>

<span data-ttu-id="c4af0-110">Hello kurz předpokládá, že se seznámíte se základními koncepty SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-110">hello tutorial assumes you are familiar with SQL Data Warehouse basic concepts.</span></span> <span data-ttu-id="c4af0-111">Pokud potřebujete úvodní informace, přečtěte si téma [Co je SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="c4af0-111">If you need an introduction, see [What is SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span></span> 

### <a name="sign-up-for-microsoft-azure"></a><span data-ttu-id="c4af0-112">Přihlášení k Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="c4af0-112">Sign up for Microsoft Azure</span></span>
<span data-ttu-id="c4af0-113">Pokud nemáte účet Microsoft Azure, musíte toosign pro jeden toouse této služby.</span><span class="sxs-lookup"><span data-stu-id="c4af0-113">If you don't already have a Microsoft Azure account, you need toosign up for one toouse this service.</span></span> <span data-ttu-id="c4af0-114">Pokud již máte účet, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="c4af0-114">If you already have an account, you may skip this step.</span></span> 

1. <span data-ttu-id="c4af0-115">Procházení stránek účet toohello [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span><span class="sxs-lookup"><span data-stu-id="c4af0-115">Navigate toohello account pages [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span></span>
2. <span data-ttu-id="c4af0-116">Vytvořte si bezplatný účet Azure, nebo si účet zakupte.</span><span class="sxs-lookup"><span data-stu-id="c4af0-116">Create a free Azure account, or purchase an account.</span></span>
3. <span data-ttu-id="c4af0-117">Postupujte podle pokynů hello</span><span class="sxs-lookup"><span data-stu-id="c4af0-117">Follow hello instructions</span></span>

### <a name="install-appropriate-sql-client-drivers-and-tools"></a><span data-ttu-id="c4af0-118">Instalace odpovídajících ovladačů a nástrojů klienta SQL</span><span class="sxs-lookup"><span data-stu-id="c4af0-118">Install appropriate SQL client drivers and tools</span></span>

<span data-ttu-id="c4af0-119">Většina nástroje SQL client tools připojit tooSQL datového skladu pomocí JDBC, ODBC nebo ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="c4af0-119">Most SQL client tools can connect tooSQL Data Warehouse by using JDBC, ODBC, or ADO.NET.</span></span> <span data-ttu-id="c4af0-120">Z důvodu toohello velký počet funkcí T-SQL, které podporuje SQL Data Warehouse nejsou některé klientské aplikace plně kompatibilní s datovým skladem SQL.</span><span class="sxs-lookup"><span data-stu-id="c4af0-120">Due toohello large number of T-SQL features that SQL Data Warehouse supports, some client applications are not fully compatible with SQL Data Warehouse.</span></span>

<span data-ttu-id="c4af0-121">Pokud používáte operační systém Windows, doporučujeme použít buď sadu [Visual Studio], nebo aplikaci [SQL Server Management Studio].</span><span class="sxs-lookup"><span data-stu-id="c4af0-121">If you are running a Windows operating system, we recommend using either [Visual Studio] or [SQL Server Management Studio].</span></span>

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="c4af0-122">Vytvoření SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c4af0-122">Create a SQL Data Warehouse</span></span>

<span data-ttu-id="c4af0-123">SQL Data Warehouse je zvláštním typem databáze, která je navržena pro výkonné paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="c4af0-123">A SQL Data Warehouse is a special type of database that is designed for massively parallel processing.</span></span> <span data-ttu-id="c4af0-124">Hello databáze je rozdělené mezi více uzly a zpracovává dotazy paralelně.</span><span class="sxs-lookup"><span data-stu-id="c4af0-124">hello database is distributed across multiple nodes and processes queries in parallel.</span></span> <span data-ttu-id="c4af0-125">SQL Data Warehouse má řídicí uzel, které orchestrují hello aktivity všech uzlů hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-125">SQL Data Warehouse has a control node that orchestrates hello activities of all hello nodes.</span></span> <span data-ttu-id="c4af0-126">Hello v samotných uzlech použít SQL Database toomanage data.</span><span class="sxs-lookup"><span data-stu-id="c4af0-126">hello nodes themselves use SQL Database toomanage your data.</span></span>  

> [!NOTE]
> <span data-ttu-id="c4af0-127">Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="c4af0-127">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="c4af0-128">Další informace najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="c4af0-128">For more information, see [SQL Data Warehouse pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>
>

### <a name="create-a-data-warehouse"></a><span data-ttu-id="c4af0-129">Vytvoření datového skladu</span><span class="sxs-lookup"><span data-stu-id="c4af0-129">Create a data warehouse</span></span>

1. <span data-ttu-id="c4af0-130">Přihlaste se k hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c4af0-130">Sign into hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c4af0-131">Klikněte na **Nový** > **Databáze** > **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="c4af0-131">Click **New** > **Databases** > **SQL Data Warehouse**.</span></span>

    <span data-ttu-id="c4af0-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span><span class="sxs-lookup"><span data-stu-id="c4af0-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span></span>

3. <span data-ttu-id="c4af0-133">Zadání podrobností o nasazení</span><span class="sxs-lookup"><span data-stu-id="c4af0-133">Fill out deployment details</span></span>

    <span data-ttu-id="c4af0-134">**Název databáze:** Výběr je zcela na vás.</span><span class="sxs-lookup"><span data-stu-id="c4af0-134">**Database Name**: Pick anything you'd like.</span></span> <span data-ttu-id="c4af0-135">Pokud máte více datových skladů, doporučujeme, aby názvy zahrne podrobnosti, jako je například hello oblast, prostředí, například *mydw-westus-1-test*.</span><span class="sxs-lookup"><span data-stu-id="c4af0-135">If you have multiple data warehouses, we recommend your names include details such as hello region, environment, for example *mydw-westus-1-test*.</span></span>

    <span data-ttu-id="c4af0-136">**Předplatné:** Vaše předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="c4af0-136">**Subscription**: Your Azure subscription</span></span>

    <span data-ttu-id="c4af0-137">**Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte existující.</span><span class="sxs-lookup"><span data-stu-id="c4af0-137">**Resource Group**: Create a resource group or use an existing resource group.</span></span>
    > [!NOTE]
    > <span data-ttu-id="c4af0-138">Skupiny prostředků jsou užitečné pro správu prostředků, jako je například určování rozsahu řízení přístupu a nasazení podle šablony.</span><span class="sxs-lookup"><span data-stu-id="c4af0-138">Resource groups are useful for resource administration such as scoping access control and templated deployment.</span></span> <span data-ttu-id="c4af0-139">Další informace o skupinách prostředků Azure a osvědčených postupech si přečtete [zde](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="c4af0-139">Read more about Azure resource groups and best practices [here](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)</span></span>

    <span data-ttu-id="c4af0-140">**Zdroj:** Prázdná databáze</span><span class="sxs-lookup"><span data-stu-id="c4af0-140">**Source**: Blank Database</span></span>

    <span data-ttu-id="c4af0-141">**Server**: Vyberte hello serveru, které jste vytvořili v [požadavky].</span><span class="sxs-lookup"><span data-stu-id="c4af0-141">**Server**: Select hello server you created in [Prerequisites].</span></span>

    <span data-ttu-id="c4af0-142">**Kolace**: ponechte hello výchozí kolace SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="c4af0-142">**Collation**: Leave hello default collation SQL_Latin1_General_CP1_CI_AS.</span></span>

    <span data-ttu-id="c4af0-143">**Vyberte výkonu**: doporučujeme začít s standardní 400DWU hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-143">**Select performance**: We recommend starting with hello standard 400DWU.</span></span>

4. <span data-ttu-id="c4af0-144">Zvolte **Pin toodashboard** ![tooDashboard PIN kód](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="c4af0-144">Choose **Pin toodashboard** ![Pin tooDashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span></span>

5. <span data-ttu-id="c4af0-145">Sledujte a počkat na vaše data warehouse toodeploy!</span><span class="sxs-lookup"><span data-stu-id="c4af0-145">Sit back and wait for your data warehouse toodeploy!</span></span> <span data-ttu-id="c4af0-146">Je běžné, tento proces tootake několik minut.</span><span class="sxs-lookup"><span data-stu-id="c4af0-146">It's normal for this process tootake several minutes.</span></span> <span data-ttu-id="c4af0-147">portál Hello vás upozorní, když váš datový sklad je připraven toouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-147">hello portal notifies you when your data warehouse is ready toouse.</span></span> 

## <a name="connect-toosql-data-warehouse"></a><span data-ttu-id="c4af0-148">Připojit tooSQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="c4af0-148">Connect tooSQL Data Warehouse</span></span>

<span data-ttu-id="c4af0-149">Tento kurz používá SQL Server Management Studio (SSMS) tooconnect toohello datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-149">This tutorial uses SQL Server Management Studio (SSMS) tooconnect toohello data warehouse.</span></span> <span data-ttu-id="c4af0-150">TooSQL datového skladu můžete připojit přes tyto podporované konektory: ADO.NET, JDBC, rozhraní ODBC a PHP.</span><span class="sxs-lookup"><span data-stu-id="c4af0-150">You can connect tooSQL Data Warehouse through these supported connectors: ADO.NET, JDBC, ODBC, and PHP.</span></span> <span data-ttu-id="c4af0-151">Mějte na paměti, že funkce nástrojů, které nejsou podporované Microsoftem, můžou být omezené.</span><span class="sxs-lookup"><span data-stu-id="c4af0-151">Remember, functionality might be limited for tools that are not supported by Microsoft.</span></span>


### <a name="get-connection-information"></a><span data-ttu-id="c4af0-152">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="c4af0-152">Get connection information</span></span>

<span data-ttu-id="c4af0-153">tooconnect tooyour datového skladu, je nutné tooconnect prostřednictvím hello logický SQL server vytvoříte v [požadavky].</span><span class="sxs-lookup"><span data-stu-id="c4af0-153">tooconnect tooyour data warehouse, you need tooconnect through hello logical SQL server you created in [Prerequisites].</span></span>

1. <span data-ttu-id="c4af0-154">Vyberte datový sklad z hello řídicího panelu nebo vyhledejte ji ve vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="c4af0-154">Select your data warehouse from hello dashboard or search for it in your resources.</span></span>

    ![Řídicí panel SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. <span data-ttu-id="c4af0-156">Najít hello úplný název pro hello logický SQL server.</span><span class="sxs-lookup"><span data-stu-id="c4af0-156">Find hello full name for hello logical SQL server.</span></span>

    ![Výběr názvu serveru](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. <span data-ttu-id="c4af0-158">Otevřete aplikaci SSMS a použití objektu explorer tooconnect toothis serveru pomocí pověření správce serveru hello jste vytvořili v [požadavky]</span><span class="sxs-lookup"><span data-stu-id="c4af0-158">Open SSMS and use object explorer tooconnect toothis server using hello server admin credentials you created in [Prerequisites]</span></span>

    ![Připojení přes SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

<span data-ttu-id="c4af0-160">Pokud všechno proběhne správně, můžete nyní měli být připojené tooyour logický SQL server.</span><span class="sxs-lookup"><span data-stu-id="c4af0-160">If all goes correctly, you should now be connected tooyour logical SQL server.</span></span> <span data-ttu-id="c4af0-161">Vzhledem k tomu, že jste se přihlásili jako hello správce serveru, se můžete připojit databázi tooany hostovaný hello serverem, včetně hello hlavní databázi.</span><span class="sxs-lookup"><span data-stu-id="c4af0-161">Since you logged in as hello server admin, you can connect tooany database hosted by hello server, including hello master database.</span></span> 

<span data-ttu-id="c4af0-162">Je účet správce pouze jeden server a většina oprávnění uživatele, který má hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-162">There is only one server admin account and it has hello most privileges of any user.</span></span> <span data-ttu-id="c4af0-163">Dávejte pozor, není tooallow příliš mnoho uživatelů v organizaci tooknow hello správce heslo.</span><span class="sxs-lookup"><span data-stu-id="c4af0-163">Be careful not tooallow too many people in your organization tooknow hello admin password.</span></span> 

<span data-ttu-id="c4af0-164">Můžete mít také účet správce Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4af0-164">You can also have an Azure active directory admin account.</span></span> <span data-ttu-id="c4af0-165">Poskytujeme není zde podrobnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-165">We don't provide hello details here.</span></span> <span data-ttu-id="c4af0-166">Pokud chcete, aby toolearn Další informace o používání ověřování Azure Active Directory, přečtěte si téma [ověřování Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span><span class="sxs-lookup"><span data-stu-id="c4af0-166">If you want toolearn more about using Azure Active Directory authentication, see [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span></span>

<span data-ttu-id="c4af0-167">Dále se budeme věnovat vytváření dalších přihlašovacích údajů a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4af0-167">Next, we explore creating additional logins and users.</span></span>


## <a name="create-a-database-user"></a><span data-ttu-id="c4af0-168">Vytvoření uživatele databáze</span><span class="sxs-lookup"><span data-stu-id="c4af0-168">Create a database user</span></span>

<span data-ttu-id="c4af0-169">V tomto kroku vytvoříte tooaccess účet uživatele datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-169">In this step, you create a user account tooaccess your data warehouse.</span></span> <span data-ttu-id="c4af0-170">Můžeme také ukazují, jak toogive tohoto uživatele hello možnost toorun dotazy s velké množství prostředky paměti a procesoru.</span><span class="sxs-lookup"><span data-stu-id="c4af0-170">We also show you how toogive that user hello ability toorun queries with a large amount of memory and CPU resources.</span></span>

### <a name="notes-about-resource-classes-for-allocating-resources-tooqueries"></a><span data-ttu-id="c4af0-171">Poznámky k třídy prostředků pro přidělování prostředků tooqueries</span><span class="sxs-lookup"><span data-stu-id="c4af0-171">Notes about resource classes for allocating resources tooqueries</span></span>

- <span data-ttu-id="c4af0-172">tookeep data bezpečné, nepoužívejte hello serveru správce toorun dotazy na provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="c4af0-172">tookeep your data safe, don't use hello server admin toorun queries on your production databases.</span></span> <span data-ttu-id="c4af0-173">Většina oprávnění uživatele, který má hello a použití tooperform operací na uživatelských datech vloží vaše data v ohrožení.</span><span class="sxs-lookup"><span data-stu-id="c4af0-173">It has hello most privileges of any user and using it tooperform operations on user data puts your data at risk.</span></span> <span data-ttu-id="c4af0-174">Navíc vzhledem k tomu, že Dobrý den, správce serveru je určen tooperform operace správy, je spuštěna operace s jenom malé přidělení prostředky paměti a procesoru.</span><span class="sxs-lookup"><span data-stu-id="c4af0-174">Also, since hello server admin is meant tooperform management operations, it runs operations with only a small allocation of memory and CPU resources.</span></span> 

- <span data-ttu-id="c4af0-175">SQL Data Warehouse používá předem definovaných databázových rolí, jako třídy prostředků, tooallocate různých množství paměti, prostředky procesoru a toousers sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="c4af0-175">SQL Data Warehouse uses pre-defined database roles, called resource classes, tooallocate different amounts of memory, CPU resources, and concurrency slots toousers.</span></span> <span data-ttu-id="c4af0-176">Každý uživatel může patřit třída tooa malé, střední, velký nebo velmi velká prostředků.</span><span class="sxs-lookup"><span data-stu-id="c4af0-176">Each user can belong tooa small, medium, large, or extra-large resource class.</span></span> <span data-ttu-id="c4af0-177">Hello Třída prostředků uživatele určuje hello prostředky hello uživatel má toorun dotazů a načte operace.</span><span class="sxs-lookup"><span data-stu-id="c4af0-177">hello user's resource class determines hello resources hello user has toorun queries and load operations.</span></span>

- <span data-ttu-id="c4af0-178">Pro optimální data kompresi pravděpodobně bude třeba hello uživatele tooload s velké nebo přidělení velmi velké zdroje.</span><span class="sxs-lookup"><span data-stu-id="c4af0-178">For optimal data compression, hello user may need tooload with large or extra large resource allocations.</span></span> <span data-ttu-id="c4af0-179">Další informace o třídách prostředků najdete [zde](./sql-data-warehouse-develop-concurrency.md#resource-classes):</span><span class="sxs-lookup"><span data-stu-id="c4af0-179">Read more about resource classes [here](./sql-data-warehouse-develop-concurrency.md#resource-classes):</span></span>

### <a name="create-an-account-that-can-control-a-database"></a><span data-ttu-id="c4af0-180">Vytvoření účtu, který může řídit databázi</span><span class="sxs-lookup"><span data-stu-id="c4af0-180">Create an account that can control a database</span></span>

<span data-ttu-id="c4af0-181">Vzhledem k tomu, že jste aktuálně přihlášeni v hello správce serveru nemáte oprávnění toocreate přihlášení a uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4af0-181">Since you are currently logged in as hello server admin you have permissions toocreate logins and users.</span></span>

1. <span data-ttu-id="c4af0-182">Pomocí SSMS nebo jiného klienta dotazů otevřete nový dotaz pro **master**.</span><span class="sxs-lookup"><span data-stu-id="c4af0-182">Using SSMS or another query client, open a new query for **master**.</span></span>

    ![Nový dotaz na Master](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nový dotaz na Master1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. <span data-ttu-id="c4af0-185">V okně dotazu hello spusťte tento příkaz toocreate T-SQL s názvem MedRCLogin přihlášení a uživatele s názvem LoadingUser.</span><span class="sxs-lookup"><span data-stu-id="c4af0-185">In hello query window, run this T-SQL command toocreate a login named MedRCLogin and user named LoadingUser.</span></span> <span data-ttu-id="c4af0-186">Toto přihlášení můžete připojit toohello logický SQL server.</span><span class="sxs-lookup"><span data-stu-id="c4af0-186">This login can connect toohello logical SQL server.</span></span>

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. <span data-ttu-id="c4af0-187">Nyní dotazování hello *databázi SQL Data Warehouse*, vytvořte uživatele databáze založené na hello přihlášení, které jste vytvořili tooaccess a provádění operací v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-187">Now querying hello *SQL Data Warehouse database*, create a database user based on hello login you created tooaccess and perform operations on hello database.</span></span>

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. <span data-ttu-id="c4af0-188">Dejte hello uživatele řízení oprávnění toohello databáze názvem NYT.</span><span class="sxs-lookup"><span data-stu-id="c4af0-188">Give hello database user control permissions toohello database called NYT.</span></span> 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] tooLoadingUser;
    ```
    > [!NOTE]
    > <span data-ttu-id="c4af0-189">Název databáze je pomlčky v něm, nebude se toowrap ji do hranatých závorek!</span><span class="sxs-lookup"><span data-stu-id="c4af0-189">If your database name has hyphens in it, be sure toowrap it in brackets!</span></span> 
    >

### <a name="give-hello-user-medium-resource-allocations"></a><span data-ttu-id="c4af0-190">Poskytnout přidělení střední zdroje hello uživatele</span><span class="sxs-lookup"><span data-stu-id="c4af0-190">Give hello user medium resource allocations</span></span>

1. <span data-ttu-id="c4af0-191">Spusťte tento příkaz toomake T-SQL a členem třídy hello střední prostředků, která se nazývá mediumrc it.</span><span class="sxs-lookup"><span data-stu-id="c4af0-191">Run this T-SQL command toomake it a member of hello medium resource class, which is called mediumrc.</span></span> 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > <span data-ttu-id="c4af0-192">Klikněte na tlačítko [sem](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn více informací o souběžnosti a prostředků třídy!</span><span class="sxs-lookup"><span data-stu-id="c4af0-192">Click [here](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn more about concurrency and resource classes!</span></span> 
    >

2. <span data-ttu-id="c4af0-193">Připojit toohello logického serveru s novými pověřeními hello</span><span class="sxs-lookup"><span data-stu-id="c4af0-193">Connect toohello logical server with hello new credentials</span></span>

    ![Přihlášení pomocí nových přihlašovacích údajů](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="c4af0-195">Načtení dat z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="c4af0-195">Load data from Azure blob storage</span></span>

<span data-ttu-id="c4af0-196">Nyní je připraven tooload data do datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-196">You are now ready tooload data into your data warehouse.</span></span> <span data-ttu-id="c4af0-197">Tento krok ukazuje, jak tooload New Yorku taxíkem souboru cab dat z veřejného úložiště Azure blob.</span><span class="sxs-lookup"><span data-stu-id="c4af0-197">This step shows you how tooload New York City taxi cab data from a public Azure storage blob.</span></span> 

- <span data-ttu-id="c4af0-198">Běžný způsob, jakým tooload dat do SQL Data Warehouse je toofirst přesunout úložiště objektů blob tooAzure hello data a pak můžete načíst do datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-198">A common way tooload data into SQL Data Warehouse is toofirst move hello data tooAzure blob storage, and then load it into your data warehouse.</span></span> <span data-ttu-id="c4af0-199">toomake je snazší toounderstand jak tooload, máme New Yorku taxíkem souboru cab dat již hostované ve veřejné úložiště Azure blob.</span><span class="sxs-lookup"><span data-stu-id="c4af0-199">toomake it easier toounderstand how tooload, we have New York taxi cab data already hosted in a public Azure storage blob.</span></span> 

- <span data-ttu-id="c4af0-200">Pro budoucí použití, toolearn jak tooget tooAzure vaše data objektu blob úložiště nebo tooload ji přímo ze zdroje do SQL Data Warehouse, najdete v části hello [přehledem načítání](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="c4af0-200">For future reference, toolearn how tooget your data tooAzure blob storage or tooload it directly from your source into SQL Data Warehouse, see hello [loading overview](sql-data-warehouse-overview-load.md).</span></span>


### <a name="define-external-data"></a><span data-ttu-id="c4af0-201">Definování externích dat</span><span class="sxs-lookup"><span data-stu-id="c4af0-201">Define external data</span></span>

1. <span data-ttu-id="c4af0-202">Vytvořte hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="c4af0-202">Create a master key.</span></span> <span data-ttu-id="c4af0-203">Potřebujete jenom toocreate hlavní klíč jednou za databáze.</span><span class="sxs-lookup"><span data-stu-id="c4af0-203">You only need toocreate a master key once per database.</span></span> 

    ```sql
    CREATE MASTER KEY;
    ```

2. <span data-ttu-id="c4af0-204">Zadejte umístění hello hello Azure blob, který obsahuje data souboru cab taxíkem hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-204">Define hello location of hello Azure blob that contains hello taxi cab data.</span></span>  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. <span data-ttu-id="c4af0-205">Zadejte externí formátů hello</span><span class="sxs-lookup"><span data-stu-id="c4af0-205">Define hello external file formats</span></span>

    <span data-ttu-id="c4af0-206">Hello ```CREATE EXTERNAL FILE FORMAT``` příkaz je použité toospecify formát soubory, které obsahují externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-206">hello ```CREATE EXTERNAL FILE FORMAT``` command is used toospecify the format of files that contain hello external data.</span></span> <span data-ttu-id="c4af0-207">Obsahují text oddělený jedním nebo několika znaky, kterým se říká oddělovače.</span><span class="sxs-lookup"><span data-stu-id="c4af0-207">They contain text separated by one or more characters called delimiters.</span></span> <span data-ttu-id="c4af0-208">Pro demonstrační účely hello taxíkem souboru cab data uložena jako nekomprimovaných dat a jako gzip komprimována data.</span><span class="sxs-lookup"><span data-stu-id="c4af0-208">For demonstration purposes, hello taxi cab data is stored both as uncompressed data and as gzip compressed data.</span></span>

    <span data-ttu-id="c4af0-209">Spuštění těchto příkazů T-SQL toodefine ve dvou různých formátech: nekomprimovaným a komprimované.</span><span class="sxs-lookup"><span data-stu-id="c4af0-209">Run these T-SQL commands toodefine two different formats: uncompressed and compressed.</span></span>

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  <span data-ttu-id="c4af0-210">Vytvořte schéma pro váš formát externích souborů.</span><span class="sxs-lookup"><span data-stu-id="c4af0-210">Create a schema for your external file format.</span></span> 

    ```sql
    CREATE SCHEMA ext;
    ```
5. <span data-ttu-id="c4af0-211">Vytvořte hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="c4af0-211">Create hello external tables.</span></span> <span data-ttu-id="c4af0-212">Tyto tabulky odkazují na data uložená v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c4af0-212">These tables reference data stored in Azure blob storage.</span></span> <span data-ttu-id="c4af0-213">Několik externí tabulky, že všechny bodu toohello objektů blob v Azure definovaného dříve v našem externí zdroj dat spuštění hello následující toocreate příkazů T-SQL.</span><span class="sxs-lookup"><span data-stu-id="c4af0-213">Run hello following T-SQL commands toocreate several external tables that all point toohello Azure blob we defined previously in our external data source.</span></span>

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-hello-data-from-azure-blob-storage"></a><span data-ttu-id="c4af0-214">Umožňuje importovat hello data z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c4af0-214">Import hello data from Azure blob storage.</span></span>

<span data-ttu-id="c4af0-215">SQL Data Warehouse podporuje klíčový příkaz CREATE TABLE AS SELECT (CTAS).</span><span class="sxs-lookup"><span data-stu-id="c4af0-215">SQL Data Warehouse supports a key statement called CREATE TABLE AS SELECT (CTAS).</span></span> <span data-ttu-id="c4af0-216">Tento příkaz vytvoří novou tabulku na základě výsledků hello příkazu select.</span><span class="sxs-lookup"><span data-stu-id="c4af0-216">This statement creates a new table based on hello results of a select statement.</span></span> <span data-ttu-id="c4af0-217">Hello nová tabulka obsahuje hello stejnou sloupce a datové typy jako hello výsledky hello vyberte příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4af0-217">hello new table has hello same columns and data types as hello results of hello select statement.</span></span>  <span data-ttu-id="c4af0-218">Jedná se způsob tooimport data z Azure blob storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-218">This is an elegant way tooimport data from Azure blob storage into SQL Data Warehouse.</span></span>

1. <span data-ttu-id="c4af0-219">Spusťte tento skript tooimport vaše data.</span><span class="sxs-lookup"><span data-stu-id="c4af0-219">Run this script tooimport your data.</span></span>

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. <span data-ttu-id="c4af0-220">Zobrazte data během načítání.</span><span class="sxs-lookup"><span data-stu-id="c4af0-220">View your data as it loads.</span></span>

   <span data-ttu-id="c4af0-221">Provedete načtení několika GB dat a jejich kompresi do vysoce výkonných clusterovaných indexů columnstore.</span><span class="sxs-lookup"><span data-stu-id="c4af0-221">You’re loading several GBs of data and compressing it into highly performant clustered columnstore indexes.</span></span> <span data-ttu-id="c4af0-222">Spusťte následující dotaz, který používá zobrazení (zobrazení dynamické správy) dynamické správy tooshow hello stav zatížení hello hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-222">Run hello following query that uses a dynamic management views (DMVs) tooshow hello status of hello load.</span></span> <span data-ttu-id="c4af0-223">Po spuštění dotazu hello, získat, kávy a piknik při SQL Data Warehouse nepodporuje některé lifting náročné.</span><span class="sxs-lookup"><span data-stu-id="c4af0-223">After starting hello query, grab a coffee and a snack while SQL Data Warehouse does some heavy lifting.</span></span>
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. <span data-ttu-id="c4af0-224">Zobrazte všechny systémové dotazy.</span><span class="sxs-lookup"><span data-stu-id="c4af0-224">View all system queries.</span></span>

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. <span data-ttu-id="c4af0-225">Užívejte si pohled na to, jak se data do Azure SQL Data Warehouse krásně načítají.</span><span class="sxs-lookup"><span data-stu-id="c4af0-225">Enjoy seeing your data nicely loaded into your Azure SQL Data Warehouse.</span></span>

    ![Zobrazení načtených data](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a><span data-ttu-id="c4af0-227">Vylepšení výkonu dotazů</span><span class="sxs-lookup"><span data-stu-id="c4af0-227">Improve query performance</span></span>

<span data-ttu-id="c4af0-228">Existuje několik způsobů tooimprove dotazu výkonu a hello tooachieve vysokorychlostní se výkonu, který datový sklad SQL je určená tooprovide.</span><span class="sxs-lookup"><span data-stu-id="c4af0-228">There are several ways tooimprove query performance and tooachieve hello high-speed performance that SQL Data Warehouse is designed tooprovide.</span></span>  

### <a name="see-hello-effect-of-scaling-on-query-performance"></a><span data-ttu-id="c4af0-229">V tématu účinku hello škálování na výkon dotazů</span><span class="sxs-lookup"><span data-stu-id="c4af0-229">See hello effect of scaling on query performance</span></span> 

<span data-ttu-id="c4af0-230">Jedním ze způsobů tooimprove výkon dotazů je tooscale prostředky tak, že změníte úroveň služby hello DWU pro datový sklad.</span><span class="sxs-lookup"><span data-stu-id="c4af0-230">One way tooimprove query performance is tooscale resources by changing hello DWU service level for your data warehouse.</span></span> <span data-ttu-id="c4af0-231">Každá úroveň služeb je nákladnější, kdykoli však můžete škálovat směrem dolů nebo pozastavit prostředky.</span><span class="sxs-lookup"><span data-stu-id="c4af0-231">Each service level costs more, but you can scale back or pause resources at any time.</span></span> 

<span data-ttu-id="c4af0-232">V tomto kroku porovnáte výkon pro dvě různá nastavení DWU.</span><span class="sxs-lookup"><span data-stu-id="c4af0-232">In this step, you compare performance at two different DWU settings.</span></span>

<span data-ttu-id="c4af0-233">První, nyní škálování hello velikosti dolů too100 DWU, takže jsme můžete získat představu o tom, jak jednom výpočetním uzlu může provádět svoje vlastní.</span><span class="sxs-lookup"><span data-stu-id="c4af0-233">First, let's scale hello sizing down too100 DWU so we can get an idea of how one compute node might perform on its own.</span></span>

1. <span data-ttu-id="c4af0-234">Přejděte toohello portál a vyberte svoji službu SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-234">Go toohello portal and select your SQL Data Warehouse.</span></span>

2. <span data-ttu-id="c4af0-235">V okně SQL Data Warehouse hello vyberte škálování.</span><span class="sxs-lookup"><span data-stu-id="c4af0-235">Select scale in hello SQL Data Warehouse blade.</span></span> 

    ![Škálování DW z portálu](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. <span data-ttu-id="c4af0-237">Snižovat výkon hello panelu too100 DWU a stiskněte tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="c4af0-237">Scale down hello performance bar too100 DWU and hit save.</span></span>

    ![Škálování a uložení](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. <span data-ttu-id="c4af0-239">Počkejte, než pro vaše toofinish operace škálování.</span><span class="sxs-lookup"><span data-stu-id="c4af0-239">Wait for your scale operation toofinish.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4af0-240">Dotazy nelze spustit při změně měřítka hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-240">Queries cannot run while changing hello scale.</span></span> <span data-ttu-id="c4af0-241">Škálování **ukončí** aktuálně spuštěné dotazy.</span><span class="sxs-lookup"><span data-stu-id="c4af0-241">Scaling **kills** your currently running queries.</span></span> <span data-ttu-id="c4af0-242">Můžete je restartovat po dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-242">You can restart them when hello operation is finished.</span></span>
    >
    
5. <span data-ttu-id="c4af0-243">Proveďte operaci prohledávání na služební cestě data hello, výběr hello nejvyšší mil. položky pro všechny sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-243">Do a scan operation on hello trip data, selecting hello top million entries for all hello columns.</span></span> <span data-ttu-id="c4af0-244">Pokud jste přes toomove na rychle, zaregistrované volné tooselect méně řádků.</span><span class="sxs-lookup"><span data-stu-id="c4af0-244">If you're eager toomove on quickly, feel free tooselect fewer rows.</span></span> <span data-ttu-id="c4af0-245">Poznamenejte si dobu hello trvá toorun tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="c4af0-245">Take note of hello time it takes toorun this operation.</span></span>

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. <span data-ttu-id="c4af0-246">Škálovat datový sklad zpět too400 DWU.</span><span class="sxs-lookup"><span data-stu-id="c4af0-246">Scale your data warehouse back too400 DWU.</span></span> <span data-ttu-id="c4af0-247">Pamatujte si, že se každý 100 DWU je přidání jiného výpočetní uzel tooyour Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-247">Remember, each 100 DWU is adding another compute node tooyour Azure SQL Data Warehouse.</span></span>

7. <span data-ttu-id="c4af0-248">Spusťte znovu dotaz hello!</span><span class="sxs-lookup"><span data-stu-id="c4af0-248">Run hello query again!</span></span> <span data-ttu-id="c4af0-249">Měli byste zaznamenat značný rozdíl.</span><span class="sxs-lookup"><span data-stu-id="c4af0-249">You should notice a significant difference.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="c4af0-250">Protože hello dotaz vrací velké množství dat, může být dostupnosti šířky pásma hello hello počítače spuštěného SSMS kritických bodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-250">Because hello query returns a lot of data, hello bandwidth availability of hello machine running SSMS may be a performance bottleneck.</span></span> <span data-ttu-id="c4af0-251">Důsledkem může být, že neuvidíte žádná zlepšení výkonu!</span><span class="sxs-lookup"><span data-stu-id="c4af0-251">This can result in you not seeing any performance improvements!</span></span>

> [!NOTE]
> <span data-ttu-id="c4af0-252">Služba SQL Data Warehouse je postavena na architektuře MPP (Massively Parallel Processing).</span><span class="sxs-lookup"><span data-stu-id="c4af0-252">Since SQL Data Warehouse uses massively parallel processing.</span></span> <span data-ttu-id="c4af0-253">Dotazy, které kontroly ani provést u miliony řádků analytické funkce prostředí hello true power služby Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4af0-253">Queries that scan or perform analytic functions on millions of rows experience hello true power of Azure SQL Data Warehouse.</span></span>
>

### <a name="see-hello-effect-of-statistics-on-query-performance"></a><span data-ttu-id="c4af0-254">Na výkon dotazů najdete v části hello účinku statistiky</span><span class="sxs-lookup"><span data-stu-id="c4af0-254">See hello effect of statistics on query performance</span></span>

1. <span data-ttu-id="c4af0-255">Spusťte dotaz, zda spojení hello datum tabulku s tabulkou cestě hello</span><span class="sxs-lookup"><span data-stu-id="c4af0-255">Run a query that joins hello Date table with hello Trip table</span></span>

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    <span data-ttu-id="c4af0-256">Tento dotaz zpracování chvíli trvá, protože SQL Data Warehouse má tooshuffle dat, než je možné provést hello spojení.</span><span class="sxs-lookup"><span data-stu-id="c4af0-256">This query takes a while because SQL Data Warehouse has tooshuffle data before it can perform hello join.</span></span> <span data-ttu-id="c4af0-257">Spojení nemáte tooshuffle data, pokud jsou data navrženou toojoin v hello stejným způsobem, jako je distribuován.</span><span class="sxs-lookup"><span data-stu-id="c4af0-257">Joins do not have tooshuffle data if they are designed toojoin data in hello same way it is distributed.</span></span> <span data-ttu-id="c4af0-258">To je na hlubší diskuzi.</span><span class="sxs-lookup"><span data-stu-id="c4af0-258">That's a deeper subject.</span></span> 

2. <span data-ttu-id="c4af0-259">Na statistikách záleží.</span><span class="sxs-lookup"><span data-stu-id="c4af0-259">Statistics make a difference.</span></span> 
3. <span data-ttu-id="c4af0-260">Spusťte tento příkaz toocreate statistiky na sloupce spojení hello.</span><span class="sxs-lookup"><span data-stu-id="c4af0-260">Run this statement toocreate statistics on hello join columns.</span></span>

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > <span data-ttu-id="c4af0-261">SQL DW nespravuje statistiky automaticky.</span><span class="sxs-lookup"><span data-stu-id="c4af0-261">SQL DW does not automatically manage statistics for you.</span></span> <span data-ttu-id="c4af0-262">Statistiky jsou důležité pro výkon dotazů a důrazně se doporučuje statistiky vytvářet a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="c4af0-262">Statistics are important for query performance and it is highly recommended you create and update statistics.</span></span>
    > 
    > <span data-ttu-id="c4af0-263">**Hello získáte tak, že statistiky na sloupce použité ve spojení, sloupce použité v hello, kde najít klauzule a sloupce v GROUP BY využívat výhod.**</span><span class="sxs-lookup"><span data-stu-id="c4af0-263">**You gain hello most benefit by having statistics on columns involved in joins, columns used in hello WHERE clause and columns found in GROUP BY.**</span></span>
    >

3. <span data-ttu-id="c4af0-264">Znovu spustit dotaz hello z požadovaných součástí a sledovat případné rozdíly výkonu.</span><span class="sxs-lookup"><span data-stu-id="c4af0-264">Run hello query from Prerequisites again and observe any performance differences.</span></span> <span data-ttu-id="c4af0-265">Při hello rozdíly ve výkonnosti dotazu nebude jako závažný jako vertikálním navýšení kapacity, měli byste zaznamenat zrychlit.</span><span class="sxs-lookup"><span data-stu-id="c4af0-265">While hello differences in query performance will not be as drastic as scaling up, you should notice a  speed-up.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c4af0-266">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4af0-266">Next steps</span></span>

<span data-ttu-id="c4af0-267">Teď už připravený tooquery a prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="c4af0-267">You're now ready tooquery and explore.</span></span> <span data-ttu-id="c4af0-268">Vyzkoušejte si naše osvědčené postupy a tipy.</span><span class="sxs-lookup"><span data-stu-id="c4af0-268">Check out our best practices or tips.</span></span>

<span data-ttu-id="c4af0-269">Pokud jste hotovi zkoumat hello den, ujistěte se, že toopause instanci!</span><span class="sxs-lookup"><span data-stu-id="c4af0-269">If you're done exploring for hello day, make sure toopause your instance!</span></span> <span data-ttu-id="c4af0-270">V produkčním prostředí, můžete zaznamenat značné úspory pozastavení a škálování toomeet vašim obchodním potřebám.</span><span class="sxs-lookup"><span data-stu-id="c4af0-270">In production, you can experience enormous savings by pausing and scaling toomeet your business needs.</span></span>

![Pozastavení](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a><span data-ttu-id="c4af0-272">Užitečné informace k přečtení</span><span class="sxs-lookup"><span data-stu-id="c4af0-272">Useful readings</span></span>

<span data-ttu-id="c4af0-273">[Souběžnost a správa úloh][]</span><span class="sxs-lookup"><span data-stu-id="c4af0-273">[Concurrency and Workload Management][]</span></span>

<span data-ttu-id="c4af0-274">[Osvědčené postupy pro službu Azure SQL Data Warehouse][]</span><span class="sxs-lookup"><span data-stu-id="c4af0-274">[Best practices for Azure SQL Data Warehouse][]</span></span>

<span data-ttu-id="c4af0-275">[Monitorování dotazů][]</span><span class="sxs-lookup"><span data-stu-id="c4af0-275">[Query Monitoring][]</span></span>

<span data-ttu-id="c4af0-276">[10 nejlepších osvědčených postupů pro sestavení rozsáhlého relačního datového skladu][]</span><span class="sxs-lookup"><span data-stu-id="c4af0-276">[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][]</span></span>

<span data-ttu-id="c4af0-277">[Migrace dat tooAzure SQL Data Warehouse][]</span><span class="sxs-lookup"><span data-stu-id="c4af0-277">[Migrating Data tooAzure SQL Data Warehouse][]</span></span>

[Souběžnost a správa úloh]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example
[Osvědčené postupy pro službu Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Monitorování dotazů]: sql-data-warehouse-manage-monitor.md
[10 nejlepších osvědčených postupů pro sestavení rozsáhlého relačního datového skladu]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrace dat tooAzure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[požadavky]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
