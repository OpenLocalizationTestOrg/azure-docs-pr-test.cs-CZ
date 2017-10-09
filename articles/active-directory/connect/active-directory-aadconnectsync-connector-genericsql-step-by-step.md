---
title: aaaGeneric krok konektor SQL za krok | Microsoft Docs
description: "Tento článek je proti můžete prostřednictvím jednoduchý systém HR podrobné pomocí hello obecné konektor SQL."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b1b5f89ab588de6f92f173a7bc00f97180067669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-step-by-step"></a><span data-ttu-id="9eb79-103">Podrobný postup pro obecný konektor SQL</span><span class="sxs-lookup"><span data-stu-id="9eb79-103">Generic SQL Connector step-by-step</span></span>
<span data-ttu-id="9eb79-104">Toto téma je podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="9eb79-104">This topic is a step-by-step guide.</span></span> <span data-ttu-id="9eb79-105">Vytvoří databáze HR jednoduchého ukázkového a použít jej pro import někteří uživatelé a jejich členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="9eb79-105">It creates a simple sample HR database and use it for importing some users and their group membership.</span></span>

## <a name="prepare-hello-sample-database"></a><span data-ttu-id="9eb79-106">Příprava hello ukázkové databáze</span><span class="sxs-lookup"><span data-stu-id="9eb79-106">Prepare hello sample database</span></span>
<span data-ttu-id="9eb79-107">Na serveru se systémem SQL Server, spusťte skript SQL hello najít v [příloha A](#appendix-a). Tento skript vytvoří ukázkovou databázi s názvem hello GSQLDEMO.</span><span class="sxs-lookup"><span data-stu-id="9eb79-107">On a server running SQL Server, run hello SQL script found in [Appendix A](#appendix-a). This script creates a sample database with hello name GSQLDEMO.</span></span> <span data-ttu-id="9eb79-108">Hello objektový model pro hello vytvořit databázi vypadá podobně jako tento obrázek:</span><span class="sxs-lookup"><span data-stu-id="9eb79-108">hello object model for hello created database looks like this picture:</span></span>  
<span data-ttu-id="9eb79-109">![Objektový Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-109">![Object Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span></span>

<span data-ttu-id="9eb79-110">Také vytvořte uživatele chcete toouse tooconnect toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="9eb79-110">Also create a user you want toouse tooconnect toohello database.</span></span> <span data-ttu-id="9eb79-111">V tomto návodu je uživatel hello názvem FABRIKAM\SQLUser a umístěné v doméně hello.</span><span class="sxs-lookup"><span data-stu-id="9eb79-111">In this walkthrough, hello user is called FABRIKAM\SQLUser and located in hello domain.</span></span>

## <a name="create-hello-odbc-connection-file"></a><span data-ttu-id="9eb79-112">Vytvoření souboru připojení ODBC hello</span><span class="sxs-lookup"><span data-stu-id="9eb79-112">Create hello ODBC connection file</span></span>
<span data-ttu-id="9eb79-113">Hello obecné konektor SQL je pomocí rozhraní ODBC tooconnect toohello vzdáleného serveru.</span><span class="sxs-lookup"><span data-stu-id="9eb79-113">hello Generic SQL Connector is using ODBC tooconnect toohello remote server.</span></span> <span data-ttu-id="9eb79-114">Nejdřív potřebujeme toocreate soubor s hello informace o připojení ODBC.</span><span class="sxs-lookup"><span data-stu-id="9eb79-114">First we need toocreate a file with hello ODBC connection information.</span></span>

1. <span data-ttu-id="9eb79-115">Spusťte nástroj pro správu rozhraní ODBC hello na serveru:</span><span class="sxs-lookup"><span data-stu-id="9eb79-115">Start hello ODBC management utility on your server:</span></span>  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. <span data-ttu-id="9eb79-117">Karta vyberte hello **souborové DSN**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-117">Select hello tab **File DSN**.</span></span> <span data-ttu-id="9eb79-118">Klikněte na tlačítko **přidat...** .</span><span class="sxs-lookup"><span data-stu-id="9eb79-118">Click **Add...**.</span></span>  
   <span data-ttu-id="9eb79-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span></span>
3. <span data-ttu-id="9eb79-120">Hello out-of-box ovladačů funguje přesně, takže vyberte ho a klikněte na tlačítko **Další >**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-120">hello out-of-box driver works fine, so select it and click **Next>**.</span></span>  
   <span data-ttu-id="9eb79-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span></span>
4. <span data-ttu-id="9eb79-122">Pojmenujte soubor hello, jako například **GenericSQL**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-122">Give hello file a name, such as **GenericSQL**.</span></span>  
   <span data-ttu-id="9eb79-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span></span>
5. <span data-ttu-id="9eb79-124">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-124">Click **Finish**.</span></span>  
   <span data-ttu-id="9eb79-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span></span>
6. <span data-ttu-id="9eb79-126">Čas tooconfigure hello připojení.</span><span class="sxs-lookup"><span data-stu-id="9eb79-126">Time tooconfigure hello connection.</span></span> <span data-ttu-id="9eb79-127">Zadejte zdroj dat hello funkční popis a zadejte název hello hello serveru se systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9eb79-127">Give hello data source a good description and provide hello name of hello server running SQL Server.</span></span>  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. <span data-ttu-id="9eb79-129">Vyberte, jak tooauthenticate pomocí SQL.</span><span class="sxs-lookup"><span data-stu-id="9eb79-129">Select how tooauthenticate with SQL.</span></span> <span data-ttu-id="9eb79-130">V tomto případě používáme ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9eb79-130">In this case, we use Windows Authentication.</span></span>  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. <span data-ttu-id="9eb79-132">Zadejte jméno hello hello ukázkové databáze, **GSQLDEMO**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-132">Provide hello name of hello sample database, **GSQLDEMO**.</span></span>  
   <span data-ttu-id="9eb79-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span></span>
9. <span data-ttu-id="9eb79-134">Udržování všechno výchozí na této obrazovce.</span><span class="sxs-lookup"><span data-stu-id="9eb79-134">Keep everything default on this screen.</span></span> <span data-ttu-id="9eb79-135">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-135">Click **Finish**.</span></span>  
   <span data-ttu-id="9eb79-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span></span>
10. <span data-ttu-id="9eb79-137">Klikněte na možnost vše funguje podle očekávání, tooverify **Test zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-137">tooverify everything is working as expected, click **Test Data Source**.</span></span>  
    <span data-ttu-id="9eb79-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span></span>
11. <span data-ttu-id="9eb79-139">Zajistěte, aby hello test je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="9eb79-139">Make sure hello test is successful.</span></span>  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. <span data-ttu-id="9eb79-141">konfigurační soubor rozhraní ODBC Hello by teď měly být viditelné v souboru DSN.</span><span class="sxs-lookup"><span data-stu-id="9eb79-141">hello ODBC configuration file should now be visible in File DSN.</span></span>  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

<span data-ttu-id="9eb79-143">Nyní je k dispozici soubor hello budeme potřebovat a můžete začít vytvářet hello konektor.</span><span class="sxs-lookup"><span data-stu-id="9eb79-143">We now have hello file we need and can start creating hello Connector.</span></span>

## <a name="create-hello-generic-sql-connector"></a><span data-ttu-id="9eb79-144">Vytvoření hello obecné konektor SQL</span><span class="sxs-lookup"><span data-stu-id="9eb79-144">Create hello Generic SQL Connector</span></span>
1. <span data-ttu-id="9eb79-145">V hello uživatelského rozhraní Správce služby synchronizace, vyberte **konektory** a **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-145">In hello Synchronization Service Manager UI, select **Connectors** and **Create**.</span></span> <span data-ttu-id="9eb79-146">Vyberte **obecné SQL (Microsoft)** a pojmenujte ho popisný název.</span><span class="sxs-lookup"><span data-stu-id="9eb79-146">Select **Generic SQL (Microsoft)** and give it a descriptive name.</span></span>  
   <span data-ttu-id="9eb79-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span></span>
2. <span data-ttu-id="9eb79-148">Najít soubor hello názvu DSN, kterou jste vytvořili v předchozí části hello a nahrajte ho toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="9eb79-148">Find hello DSN file you created in hello previous section and upload it toohello server.</span></span> <span data-ttu-id="9eb79-149">Zadejte hello pověření tooconnect toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="9eb79-149">Provide hello credentials tooconnect toohello database.</span></span>  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. <span data-ttu-id="9eb79-151">V tomto návodu budeme jsou a usnadnit tak nám a Řekněme, že existují dva typy objektů, **uživatele** a **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-151">In this walkthrough, we are making it easy for us and say that there are two object types, **User** and **Group**.</span></span>
   <span data-ttu-id="9eb79-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span></span>
4. <span data-ttu-id="9eb79-153">atributy hello toofind, chceme hello konektor toodetect těmito atributy prohlížením samotné tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="9eb79-153">toofind hello attributes, we want hello Connector toodetect those attributes by looking at hello table itself.</span></span> <span data-ttu-id="9eb79-154">Vzhledem k tomu **uživatelé** je vyhrazené slovo v SQL, potřebujeme tooprovide v hranaté závorky [].</span><span class="sxs-lookup"><span data-stu-id="9eb79-154">Since **Users** is a reserved word in SQL, we need tooprovide it in square brackets [ ].</span></span>  
   <span data-ttu-id="9eb79-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span></span>
5. <span data-ttu-id="9eb79-156">Čas toodefine hello ukotvení atribut a atribut rozlišující název hello.</span><span class="sxs-lookup"><span data-stu-id="9eb79-156">Time toodefine hello anchor attribute and hello DN attribute.</span></span> <span data-ttu-id="9eb79-157">Pro **uživatelé**, používáme hello kombinace uživatelského jména hello dva atributy a EmployeeID.</span><span class="sxs-lookup"><span data-stu-id="9eb79-157">For **Users**, we use hello combination of hello two attributes username and EmployeeID.</span></span> <span data-ttu-id="9eb79-158">Pro **skupiny**, používáme GroupName (ne realistické ve skutečném, ale pro tento postup funguje).</span><span class="sxs-lookup"><span data-stu-id="9eb79-158">For **group**, we use GroupName (not realistic in real-life, but for this walkthrough it works).</span></span>
   <span data-ttu-id="9eb79-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span></span>
6. <span data-ttu-id="9eb79-160">Ne všechny typy atributů lze zjistit v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="9eb79-160">Not all attribute types can be detected in a SQL database.</span></span> <span data-ttu-id="9eb79-161">Typ atributu Hello odkaz na konkrétní nelze.</span><span class="sxs-lookup"><span data-stu-id="9eb79-161">hello reference attribute type in particular cannot.</span></span> <span data-ttu-id="9eb79-162">Pro typ objektu skupiny hello potřebujeme toochange hello ID vlastníka a MemberID tooreference.</span><span class="sxs-lookup"><span data-stu-id="9eb79-162">For hello group object type, we need toochange hello OwnerID and MemberID tooreference.</span></span>  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. <span data-ttu-id="9eb79-164">atributy Hello jsme vybrali jako atributy typu odkaz v předchozím kroku hello vyžadují hello typ objektu, který tyto hodnoty jsou odkaz na.</span><span class="sxs-lookup"><span data-stu-id="9eb79-164">hello attributes we selected as reference attributes in hello previous step require hello object type these values are a reference to.</span></span> <span data-ttu-id="9eb79-165">V našem případě hello typ objektu uživatele.</span><span class="sxs-lookup"><span data-stu-id="9eb79-165">In our case, hello User object type.</span></span>  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. <span data-ttu-id="9eb79-167">Na stránce globální parametry hello vyberte **vodoznak** jako hello rozdílů strategie.</span><span class="sxs-lookup"><span data-stu-id="9eb79-167">On hello Global Parameters page, select **Watermark** as hello delta strategy.</span></span> <span data-ttu-id="9eb79-168">Také zadat ve formátu data a času hello **rrrr MM-dd hh: mm:**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-168">Also type in hello date/time format **yyyy-MM-dd HH:mm:ss**.</span></span>
   <span data-ttu-id="9eb79-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span></span>
9. <span data-ttu-id="9eb79-170">Na hello **konfigurace oddílů a hierarchií** vyberte oba typy objektů.</span><span class="sxs-lookup"><span data-stu-id="9eb79-170">On hello **Configure Partitions and Hierarchies** page, select both object types.</span></span>
   <span data-ttu-id="9eb79-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span></span>
10. <span data-ttu-id="9eb79-172">Na hello **vyberte typy objektů** a **vybrat atributy**, vyberte typy objektů a všech atributů.</span><span class="sxs-lookup"><span data-stu-id="9eb79-172">On hello **Select Object Types** and **Select Attributes**, select both object types and all attributes.</span></span> <span data-ttu-id="9eb79-173">Na hello **konfigurace kotvy** klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-173">On hello **Configure Anchors** page, click **Finish**.</span></span>

## <a name="create-run-profiles"></a><span data-ttu-id="9eb79-174">Vytvoření profilů spuštění</span><span class="sxs-lookup"><span data-stu-id="9eb79-174">Create Run Profiles</span></span>
1. <span data-ttu-id="9eb79-175">V hello uživatelského rozhraní Správce služby synchronizace, vyberte **konektory**, a **konfigurovat profily spuštění**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-175">In hello Synchronization Service Manager UI, select **Connectors**, and **Configure Run Profiles**.</span></span> <span data-ttu-id="9eb79-176">Klikněte na tlačítko **nový profil**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-176">Click **New Profile**.</span></span> <span data-ttu-id="9eb79-177">Začneme s **úplný Import**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-177">We start with **Full Import**.</span></span>  
   <span data-ttu-id="9eb79-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span></span>
2. <span data-ttu-id="9eb79-179">Vyberte typ hello **úplný Import (jenom fázi)**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-179">Select hello type **Full Import (Stage Only)**.</span></span>  
   <span data-ttu-id="9eb79-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span></span>
3. <span data-ttu-id="9eb79-181">Vyberte oddíl hello **OBJEKT = uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-181">Select hello partition **OBJECT=User**.</span></span>  
   <span data-ttu-id="9eb79-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span></span>
4. <span data-ttu-id="9eb79-183">Vyberte **tabulky** a typ **[uživatelé]**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-183">Select **Table** and type **[USERS]**.</span></span> <span data-ttu-id="9eb79-184">Posuňte se dolů toohello více hodnot objektu typu oddílu a zadejte hello data jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="9eb79-184">Scroll down toohello multi-valued object type section and enter hello data as in hello following picture.</span></span> <span data-ttu-id="9eb79-185">Vyberte **Dokončit** toosave hello krok.</span><span class="sxs-lookup"><span data-stu-id="9eb79-185">Select **Finish** toosave hello step.</span></span>  
   <span data-ttu-id="9eb79-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span></span>  
   <span data-ttu-id="9eb79-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span></span>  
5. <span data-ttu-id="9eb79-188">Vyberte **nový krok**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-188">Select **New Step**.</span></span> <span data-ttu-id="9eb79-189">Tentokrát vyberte **OBJEKT = skupiny**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-189">This time, select **OBJECT=Group**.</span></span> <span data-ttu-id="9eb79-190">Na poslední stránku hello použijte hello konfiguraci jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="9eb79-190">On hello last page, use hello configuration as in hello following picture.</span></span> <span data-ttu-id="9eb79-191">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-191">Click **Finish**.</span></span>  
   <span data-ttu-id="9eb79-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span></span>  
   <span data-ttu-id="9eb79-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span><span class="sxs-lookup"><span data-stu-id="9eb79-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span></span>  
6. <span data-ttu-id="9eb79-194">Volitelné: Pokud chcete, můžete nakonfigurovat další profilů spuštění.</span><span class="sxs-lookup"><span data-stu-id="9eb79-194">Optional: If you want to, you can configure additional run profiles.</span></span> <span data-ttu-id="9eb79-195">V tomto návodu se používá pouze hello úplný Import.</span><span class="sxs-lookup"><span data-stu-id="9eb79-195">For this walkthrough, only hello Full Import is used.</span></span>
7. <span data-ttu-id="9eb79-196">Klikněte na tlačítko **OK** toofinish změna profilů spuštění.</span><span class="sxs-lookup"><span data-stu-id="9eb79-196">Click **OK** toofinish changing run profiles.</span></span>

## <a name="add-some-test-data-and-test-hello-import"></a><span data-ttu-id="9eb79-197">Přidat některé testovací data a testovací import hello</span><span class="sxs-lookup"><span data-stu-id="9eb79-197">Add some test data and test hello import</span></span>
<span data-ttu-id="9eb79-198">Vyplňte nějaká testovací data v ukázkové databázi.</span><span class="sxs-lookup"><span data-stu-id="9eb79-198">Fill out some test data in your sample database.</span></span> <span data-ttu-id="9eb79-199">Až budete připravení, vyberte **spustit** a **úplný import**.</span><span class="sxs-lookup"><span data-stu-id="9eb79-199">When you are ready, select **Run** and **Full import**.</span></span>

<span data-ttu-id="9eb79-200">Zde je uživatel s dvě telefonní čísla a skupiny se některé členy.</span><span class="sxs-lookup"><span data-stu-id="9eb79-200">Here is a user with two phone numbers and a group with some members.</span></span>  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a><span data-ttu-id="9eb79-203">Příloha A</span><span class="sxs-lookup"><span data-stu-id="9eb79-203">Appendix A</span></span>
<span data-ttu-id="9eb79-204">**SQL skriptu toocreate hello ukázkové databáze**</span><span class="sxs-lookup"><span data-stu-id="9eb79-204">**SQL script toocreate hello sample database**</span></span>

```SQL
---Creating hello Database---------
Create Database GSQLDEMO
Go
-------Using hello Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
