---
title: "Obecné konektor SQL krok za krokem | Microsoft Docs"
description: "Tento článek je proti prostřednictvím jednoduchý systém HR podrobné pomocí obecné konektoru SQL."
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
ms.openlocfilehash: 3fdc1b405b95180d031aa4ad45b406f7fc149d8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="generic-sql-connector-step-by-step"></a><span data-ttu-id="1ac53-103">Podrobný postup pro obecný konektor SQL</span><span class="sxs-lookup"><span data-stu-id="1ac53-103">Generic SQL Connector step-by-step</span></span>
<span data-ttu-id="1ac53-104">Toto téma je podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="1ac53-104">This topic is a step-by-step guide.</span></span> <span data-ttu-id="1ac53-105">Vytvoří databáze HR jednoduchého ukázkového a použít jej pro import někteří uživatelé a jejich členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="1ac53-105">It creates a simple sample HR database and use it for importing some users and their group membership.</span></span>

## <a name="prepare-the-sample-database"></a><span data-ttu-id="1ac53-106">Příprava ukázkové databáze</span><span class="sxs-lookup"><span data-stu-id="1ac53-106">Prepare the sample database</span></span>
<span data-ttu-id="1ac53-107">Na serveru systémem SQL Server, spusťte skript SQL v nalezen [příloha A](#appendix-a). Tento skript vytvoří ukázkovou databázi s názvem GSQLDEMO.</span><span class="sxs-lookup"><span data-stu-id="1ac53-107">On a server running SQL Server, run the SQL script found in [Appendix A](#appendix-a). This script creates a sample database with the name GSQLDEMO.</span></span> <span data-ttu-id="1ac53-108">Objektový model pro vytvořené databáze vypadá na tomto obrázku:</span><span class="sxs-lookup"><span data-stu-id="1ac53-108">The object model for the created database looks like this picture:</span></span>  
<span data-ttu-id="1ac53-109">![Objektový Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-109">![Object Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span></span>

<span data-ttu-id="1ac53-110">Také vytvořte uživatele, které chcete použít pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="1ac53-110">Also create a user you want to use to connect to the database.</span></span> <span data-ttu-id="1ac53-111">V tomto návodu je uživatel názvem FABRIKAM\SQLUser a umístěné v doméně.</span><span class="sxs-lookup"><span data-stu-id="1ac53-111">In this walkthrough, the user is called FABRIKAM\SQLUser and located in the domain.</span></span>

## <a name="create-the-odbc-connection-file"></a><span data-ttu-id="1ac53-112">Vytvoření souboru připojení rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="1ac53-112">Create the ODBC connection file</span></span>
<span data-ttu-id="1ac53-113">Obecné konektor SQL je pomocí rozhraní ODBC pro připojení ke vzdálenému serveru.</span><span class="sxs-lookup"><span data-stu-id="1ac53-113">The Generic SQL Connector is using ODBC to connect to the remote server.</span></span> <span data-ttu-id="1ac53-114">Nejprve musíme vytvořit soubor s informacemi o připojení ODBC.</span><span class="sxs-lookup"><span data-stu-id="1ac53-114">First we need to create a file with the ODBC connection information.</span></span>

1. <span data-ttu-id="1ac53-115">Spusťte nástroj pro správu rozhraní ODBC na serveru:</span><span class="sxs-lookup"><span data-stu-id="1ac53-115">Start the ODBC management utility on your server:</span></span>  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. <span data-ttu-id="1ac53-117">Vyberte kartu **souborové DSN**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-117">Select the tab **File DSN**.</span></span> <span data-ttu-id="1ac53-118">Klikněte na tlačítko **přidat...** .</span><span class="sxs-lookup"><span data-stu-id="1ac53-118">Click **Add...**.</span></span>  
   <span data-ttu-id="1ac53-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span></span>
3. <span data-ttu-id="1ac53-120">Funguje out-of-box ovladačů, přesně, takže vyberte ho a klikněte na tlačítko **Další >**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-120">The out-of-box driver works fine, so select it and click **Next>**.</span></span>  
   <span data-ttu-id="1ac53-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span></span>
4. <span data-ttu-id="1ac53-122">Pojmenujte soubor, jako například **GenericSQL**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-122">Give the file a name, such as **GenericSQL**.</span></span>  
   <span data-ttu-id="1ac53-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span></span>
5. <span data-ttu-id="1ac53-124">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-124">Click **Finish**.</span></span>  
   <span data-ttu-id="1ac53-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span></span>
6. <span data-ttu-id="1ac53-126">Čas ke konfiguraci připojení.</span><span class="sxs-lookup"><span data-stu-id="1ac53-126">Time to configure the connection.</span></span> <span data-ttu-id="1ac53-127">Zadejte zdroj dat funkční popis a zadejte název serveru se systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1ac53-127">Give the data source a good description and provide the name of the server running SQL Server.</span></span>  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. <span data-ttu-id="1ac53-129">Vyberte způsob ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="1ac53-129">Select how to authenticate with SQL.</span></span> <span data-ttu-id="1ac53-130">V tomto případě používáme ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1ac53-130">In this case, we use Windows Authentication.</span></span>  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. <span data-ttu-id="1ac53-132">Zadejte název ukázkové databáze **GSQLDEMO**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-132">Provide the name of the sample database, **GSQLDEMO**.</span></span>  
   <span data-ttu-id="1ac53-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span></span>
9. <span data-ttu-id="1ac53-134">Udržování všechno výchozí na této obrazovce.</span><span class="sxs-lookup"><span data-stu-id="1ac53-134">Keep everything default on this screen.</span></span> <span data-ttu-id="1ac53-135">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-135">Click **Finish**.</span></span>  
   <span data-ttu-id="1ac53-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span></span>
10. <span data-ttu-id="1ac53-137">Chcete-li ověřit, všechno funguje podle očekávání, klikněte na tlačítko **Test zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-137">To verify everything is working as expected, click **Test Data Source**.</span></span>  
    <span data-ttu-id="1ac53-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span></span>
11. <span data-ttu-id="1ac53-139">Ujistěte se, že test je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="1ac53-139">Make sure the test is successful.</span></span>  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. <span data-ttu-id="1ac53-141">Nyní měli být viditelné v souborové DSN ODBC konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="1ac53-141">The ODBC configuration file should now be visible in File DSN.</span></span>  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

<span data-ttu-id="1ac53-143">Nyní je k dispozici soubor budeme potřebovat a můžete začít vytvářet konektor.</span><span class="sxs-lookup"><span data-stu-id="1ac53-143">We now have the file we need and can start creating the Connector.</span></span>

## <a name="create-the-generic-sql-connector"></a><span data-ttu-id="1ac53-144">Vytvořit konektor obecné SQL</span><span class="sxs-lookup"><span data-stu-id="1ac53-144">Create the Generic SQL Connector</span></span>
1. <span data-ttu-id="1ac53-145">V uživatelském rozhraní Synchronization Service Manager vyberte **konektory** a **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-145">In the Synchronization Service Manager UI, select **Connectors** and **Create**.</span></span> <span data-ttu-id="1ac53-146">Vyberte **obecné SQL (Microsoft)** a pojmenujte ho popisný název.</span><span class="sxs-lookup"><span data-stu-id="1ac53-146">Select **Generic SQL (Microsoft)** and give it a descriptive name.</span></span>  
   <span data-ttu-id="1ac53-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span></span>
2. <span data-ttu-id="1ac53-148">Vyhledejte soubor DSN, který jste vytvořili v předchozí části a nahrajte na server.</span><span class="sxs-lookup"><span data-stu-id="1ac53-148">Find the DSN file you created in the previous section and upload it to the server.</span></span> <span data-ttu-id="1ac53-149">Zadejte pověření pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="1ac53-149">Provide the credentials to connect to the database.</span></span>  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. <span data-ttu-id="1ac53-151">V tomto návodu budeme jsou a usnadnit tak nám a Řekněme, že existují dva typy objektů, **uživatele** a **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-151">In this walkthrough, we are making it easy for us and say that there are two object types, **User** and **Group**.</span></span>
   <span data-ttu-id="1ac53-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span></span>
4. <span data-ttu-id="1ac53-153">Pro vyhledání atributů, chceme konektor ke zjištění těchto atributů prohlížením samotné tabulky.</span><span class="sxs-lookup"><span data-stu-id="1ac53-153">To find the attributes, we want the Connector to detect those attributes by looking at the table itself.</span></span> <span data-ttu-id="1ac53-154">Vzhledem k tomu **uživatelé** je vyhrazené slovo v SQL, je potřeba zadat v hranaté závorky [].</span><span class="sxs-lookup"><span data-stu-id="1ac53-154">Since **Users** is a reserved word in SQL, we need to provide it in square brackets [ ].</span></span>  
   <span data-ttu-id="1ac53-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span></span>
5. <span data-ttu-id="1ac53-156">Čas k definování ukotvení atribut a atribut rozlišující název.</span><span class="sxs-lookup"><span data-stu-id="1ac53-156">Time to define the anchor attribute and the DN attribute.</span></span> <span data-ttu-id="1ac53-157">Pro **uživatelé**, používáme kombinace uživatelského jména dva atributy a EmployeeID.</span><span class="sxs-lookup"><span data-stu-id="1ac53-157">For **Users**, we use the combination of the two attributes username and EmployeeID.</span></span> <span data-ttu-id="1ac53-158">Pro **skupiny**, používáme GroupName (ne realistické ve skutečném, ale pro tento postup funguje).</span><span class="sxs-lookup"><span data-stu-id="1ac53-158">For **group**, we use GroupName (not realistic in real-life, but for this walkthrough it works).</span></span>
   <span data-ttu-id="1ac53-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span></span>
6. <span data-ttu-id="1ac53-160">Ne všechny typy atributů lze zjistit v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="1ac53-160">Not all attribute types can be detected in a SQL database.</span></span> <span data-ttu-id="1ac53-161">Typ atributu odkaz na konkrétní nelze.</span><span class="sxs-lookup"><span data-stu-id="1ac53-161">The reference attribute type in particular cannot.</span></span> <span data-ttu-id="1ac53-162">Typ objektu skupiny je nutné změnit ID vlastníka a MemberID, chcete-li.</span><span class="sxs-lookup"><span data-stu-id="1ac53-162">For the group object type, we need to change the OwnerID and MemberID to reference.</span></span>  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. <span data-ttu-id="1ac53-164">Atributy, které jsme vybrali jako atributy typu odkaz v předchozím kroku vyžaduje typ objektu tyto hodnoty jsou odkaz na.</span><span class="sxs-lookup"><span data-stu-id="1ac53-164">The attributes we selected as reference attributes in the previous step require the object type these values are a reference to.</span></span> <span data-ttu-id="1ac53-165">V našem případě typ objektu uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ac53-165">In our case, the User object type.</span></span>  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. <span data-ttu-id="1ac53-167">Na stránce globální parametry vyberte **vodoznak** jako strategie delta.</span><span class="sxs-lookup"><span data-stu-id="1ac53-167">On the Global Parameters page, select **Watermark** as the delta strategy.</span></span> <span data-ttu-id="1ac53-168">Také zadejte ve formátu data a času **rrrr MM-dd hh: mm:**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-168">Also type in the date/time format **yyyy-MM-dd HH:mm:ss**.</span></span>
   <span data-ttu-id="1ac53-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span></span>
9. <span data-ttu-id="1ac53-170">Na **konfigurace oddílů a hierarchií** vyberte oba typy objektů.</span><span class="sxs-lookup"><span data-stu-id="1ac53-170">On the **Configure Partitions and Hierarchies** page, select both object types.</span></span>
   <span data-ttu-id="1ac53-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span></span>
10. <span data-ttu-id="1ac53-172">Na **vyberte typy objektů** a **vybrat atributy**, vyberte typy objektů a všech atributů.</span><span class="sxs-lookup"><span data-stu-id="1ac53-172">On the **Select Object Types** and **Select Attributes**, select both object types and all attributes.</span></span> <span data-ttu-id="1ac53-173">Na **konfigurace kotvy** klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-173">On the **Configure Anchors** page, click **Finish**.</span></span>

## <a name="create-run-profiles"></a><span data-ttu-id="1ac53-174">Vytvoření profilů spuštění</span><span class="sxs-lookup"><span data-stu-id="1ac53-174">Create Run Profiles</span></span>
1. <span data-ttu-id="1ac53-175">V uživatelském rozhraní Synchronization Service Manager vyberte **konektory**, a **konfigurovat profily spuštění**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-175">In the Synchronization Service Manager UI, select **Connectors**, and **Configure Run Profiles**.</span></span> <span data-ttu-id="1ac53-176">Klikněte na tlačítko **nový profil**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-176">Click **New Profile**.</span></span> <span data-ttu-id="1ac53-177">Začneme s **úplný Import**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-177">We start with **Full Import**.</span></span>  
   <span data-ttu-id="1ac53-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span></span>
2. <span data-ttu-id="1ac53-179">Vyberte typ **úplný Import (jenom fázi)**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-179">Select the type **Full Import (Stage Only)**.</span></span>  
   <span data-ttu-id="1ac53-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span></span>
3. <span data-ttu-id="1ac53-181">Vyberte oddíl, **OBJEKT = uživatele**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-181">Select the partition **OBJECT=User**.</span></span>  
   <span data-ttu-id="1ac53-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span></span>
4. <span data-ttu-id="1ac53-183">Vyberte **tabulky** a typ **[uživatelé]**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-183">Select **Table** and type **[USERS]**.</span></span> <span data-ttu-id="1ac53-184">Přejděte do části Typ objektu s více hodnotami a zadejte data jako na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="1ac53-184">Scroll down to the multi-valued object type section and enter the data as in the following picture.</span></span> <span data-ttu-id="1ac53-185">Vyberte **Dokončit** uložit v kroku.</span><span class="sxs-lookup"><span data-stu-id="1ac53-185">Select **Finish** to save the step.</span></span>  
   <span data-ttu-id="1ac53-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span></span>  
   <span data-ttu-id="1ac53-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span></span>  
5. <span data-ttu-id="1ac53-188">Vyberte **nový krok**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-188">Select **New Step**.</span></span> <span data-ttu-id="1ac53-189">Tentokrát vyberte **OBJEKT = skupiny**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-189">This time, select **OBJECT=Group**.</span></span> <span data-ttu-id="1ac53-190">Na poslední stránce použijte konfiguraci jako na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="1ac53-190">On the last page, use the configuration as in the following picture.</span></span> <span data-ttu-id="1ac53-191">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-191">Click **Finish**.</span></span>  
   <span data-ttu-id="1ac53-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span></span>  
   <span data-ttu-id="1ac53-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span><span class="sxs-lookup"><span data-stu-id="1ac53-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span></span>  
6. <span data-ttu-id="1ac53-194">Volitelné: Pokud chcete, můžete nakonfigurovat další profilů spuštění.</span><span class="sxs-lookup"><span data-stu-id="1ac53-194">Optional: If you want to, you can configure additional run profiles.</span></span> <span data-ttu-id="1ac53-195">V tomto návodu se používá pouze úplný Import.</span><span class="sxs-lookup"><span data-stu-id="1ac53-195">For this walkthrough, only the Full Import is used.</span></span>
7. <span data-ttu-id="1ac53-196">Klikněte na tlačítko **OK** pro dokončení změny profilů spuštění.</span><span class="sxs-lookup"><span data-stu-id="1ac53-196">Click **OK** to finish changing run profiles.</span></span>

## <a name="add-some-test-data-and-test-the-import"></a><span data-ttu-id="1ac53-197">Přidat některé testovacích dat a testování importu</span><span class="sxs-lookup"><span data-stu-id="1ac53-197">Add some test data and test the import</span></span>
<span data-ttu-id="1ac53-198">Vyplňte nějaká testovací data v ukázkové databázi.</span><span class="sxs-lookup"><span data-stu-id="1ac53-198">Fill out some test data in your sample database.</span></span> <span data-ttu-id="1ac53-199">Až budete připravení, vyberte **spustit** a **úplný import**.</span><span class="sxs-lookup"><span data-stu-id="1ac53-199">When you are ready, select **Run** and **Full import**.</span></span>

<span data-ttu-id="1ac53-200">Zde je uživatel s dvě telefonní čísla a skupiny se některé členy.</span><span class="sxs-lookup"><span data-stu-id="1ac53-200">Here is a user with two phone numbers and a group with some members.</span></span>  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a><span data-ttu-id="1ac53-203">Příloha A</span><span class="sxs-lookup"><span data-stu-id="1ac53-203">Appendix A</span></span>
<span data-ttu-id="1ac53-204">**Skript SQL k vytvoření ukázkové databáze**</span><span class="sxs-lookup"><span data-stu-id="1ac53-204">**SQL script to create the sample database**</span></span>

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
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
