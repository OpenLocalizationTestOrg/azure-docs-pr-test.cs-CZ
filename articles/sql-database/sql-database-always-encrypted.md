---
title: "Funkce Always Encrypted: Azure SQL Database – úložiště certifikátů Windows | Microsoft Docs"
description: "Tento článek ukazuje, jak toosecure citlivá data v databázi SQL s šifrování databáze pomocí hello vždy šifrované průvodce v serveru SQL Server Management Studio (SSMS). Také vám ukazuje jak toostore šifrovacích klíčů v certifikátu Windows hello uložit."
keywords: "šifrování dat, šifrování sql, šifrování databáze, citlivých dat, vždycky šifrovaná."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: ce7e052e-8bf6-4d7c-9204-4c6f4afeba4b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: sstein
ms.openlocfilehash: 483f9a2120cc42b732142fc07807d3d8830a0c33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-hello-windows-certificate-store"></a><span data-ttu-id="5f9e3-105">Funkce Always Encrypted: Chrání citlivá data v databázi SQL a ukládat šifrovací klíče v úložišti certifikátů Windows hello</span><span class="sxs-lookup"><span data-stu-id="5f9e3-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in hello Windows certificate store</span></span>

<span data-ttu-id="5f9e3-106">Tento článek ukazuje, jak toosecure citlivá data v SQL databázi s šifrování databáze pomocí hello [vždy šifrované průvodce](https://msdn.microsoft.com/library/mt459280.aspx) v [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-106">This article shows you how toosecure sensitive data in a SQL database with database encryption by using hello [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="5f9e3-107">Také vám ukazuje jak toostore šifrovacích klíčů v certifikátu Windows hello uložit.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-107">It also shows you how toostore your encryption keys in hello Windows certificate store.</span></span>

<span data-ttu-id="5f9e3-108">Vždy šifrovaný je nová technologie šifrování dat v Azure SQL Database a SQL Server, který pomáhá chránit citlivá data v klidovém stavu na serveru hello během pohybu mezi klientem a serverem a při hello dat se používá, nikdy zajistit, že citlivá data se zobrazí jako ve formátu prostého textu uvnitř hello databázový systém.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on hello server, during movement between client and server, and while hello data is in use, ensuring that sensitive data never appears as plaintext inside hello database system.</span></span> <span data-ttu-id="5f9e3-109">Po šifrování dat, můžete přistupovat pouze klientské aplikace nebo aplikační servery, které mají přístup toohello klíče data ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-109">After you encrypt data, only client applications or app servers that have access toohello keys can access plaintext data.</span></span> <span data-ttu-id="5f9e3-110">Podrobné informace najdete v tématu [vždycky šifrovaná (databázový stroj)](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-110">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="5f9e3-111">Po dokončení konfigurace toouse databáze hello vždycky šifrovaná, vytvoříte klientskou aplikaci v jazyce C# pomocí sady Visual Studio toowork s hello zašifrovaná data.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-111">After configuring hello database toouse Always Encrypted, you will create a client application in C# with Visual Studio toowork with hello encrypted data.</span></span>

<span data-ttu-id="5f9e3-112">Postupujte podle kroků hello v této toolearn článku jak tooset si vždy šifrována pro Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-112">Follow hello steps in this article toolearn how tooset up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="5f9e3-113">V tomto článku se dozvíte, jak tooperform hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-113">In this article, you will learn how tooperform hello following tasks:</span></span>

* <span data-ttu-id="5f9e3-114">Průvodce vždy šifrována pomocí hello v aplikaci SSMS toocreate [vždy šifrované klíče](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-114">Use hello Always Encrypted wizard in SSMS toocreate [Always Encrypted Keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="5f9e3-115">Vytvoření [sloupce hlavního klíče (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-115">Create a [Column Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="5f9e3-116">Vytvoření [šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-116">Create a [Column Encryption Key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="5f9e3-117">Vytvořit tabulku databáze a šifrování sloupců.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-117">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="5f9e3-118">Vytvořte aplikaci, která vloží, vybere a zobrazí data z hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-118">Create an application that inserts, selects, and displays data from hello encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f9e3-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f9e3-119">Prerequisites</span></span>
<span data-ttu-id="5f9e3-120">V tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-120">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="5f9e3-121">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-121">An Azure account and subscription.</span></span> <span data-ttu-id="5f9e3-122">Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-122">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5f9e3-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verze 13.0.700.242 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="5f9e3-124">[Rozhraní .NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) nebo novější (na klientském počítači hello).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-124">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on hello client computer).</span></span>
* <span data-ttu-id="5f9e3-125">Sadu [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>

## <a name="create-a-blank-sql-database"></a><span data-ttu-id="5f9e3-126">Vytvořit prázdnou databázi SQL</span><span class="sxs-lookup"><span data-stu-id="5f9e3-126">Create a blank SQL database</span></span>
1. <span data-ttu-id="5f9e3-127">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-127">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5f9e3-128">Klikněte na tlačítko **nové** > **Data + úložiště** > **databáze SQL**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-128">Click **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="5f9e3-129">Vytvoření **prázdné** databáze s názvem **Klinika** na nový nebo existující server.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-129">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="5f9e3-130">Podrobné pokyny k vytvoření databáze v hello portálu Azure, najdete v části [svoji první databázi Azure SQL](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-130">For detailed instructions about creating a database in hello Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Vytvoření prázdné databáze](./media/sql-database-always-encrypted/create-database.png)

<span data-ttu-id="5f9e3-132">Hello připojovací řetězec budete potřebovat později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-132">You will need hello connection string later in hello tutorial.</span></span> <span data-ttu-id="5f9e3-133">Po vytvoření databáze hello přejděte toohello nový Klinika databáze a zkopírujte hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-133">After hello database is created, go toohello new Clinic database and copy hello connection string.</span></span> <span data-ttu-id="5f9e3-134">Kdykoli můžete získat hello připojovací řetězec, ale je snadno toocopy ho, když jste v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-134">You can get hello connection string at any time, but it's easy toocopy it when you're in hello Azure portal.</span></span>

1. <span data-ttu-id="5f9e3-135">Klikněte na tlačítko **databází SQL** > **Klinika** > **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-135">Click **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="5f9e3-136">Zkopírujte hello připojovací řetězec pro **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-136">Copy hello connection string for **ADO.NET**.</span></span>
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a><span data-ttu-id="5f9e3-138">Připojit databáze toohello pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="5f9e3-138">Connect toohello database with SSMS</span></span>
<span data-ttu-id="5f9e3-139">Otevřete aplikaci SSMS a připojte toohello serveru s databází Klinika hello.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-139">Open SSMS and connect toohello server with hello Clinic database.</span></span>

1. <span data-ttu-id="5f9e3-140">Otevřete aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-140">Open SSMS.</span></span> <span data-ttu-id="5f9e3-141">(Klikněte na tlačítko **připojit** > **databázový stroj** tooopen hello **připojit tooServer** okno, pokud není otevřený).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-141">(Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window if it is not open).</span></span>
2. <span data-ttu-id="5f9e3-142">Zadejte název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-142">Enter your server name and credentials.</span></span> <span data-ttu-id="5f9e3-143">název serveru Hello naleznete v okně databáze SQL hello a v připojovacím řetězci hello jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-143">hello server name can be found on hello SQL database blade and in hello connection string you copied earlier.</span></span> <span data-ttu-id="5f9e3-144">Typ hello kompletní server název včetně *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-144">Type hello complete server name including *database.windows.net*.</span></span>
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted/ssms-connect.png)

<span data-ttu-id="5f9e3-146">Pokud hello **nové pravidlo brány Firewall** okno otevře, přihlaste se tooAzure a umožňují SSMS za vás vytvořit nové pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-146">If hello **New Firewall Rule** window opens, sign in tooAzure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="5f9e3-147">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="5f9e3-147">Create a table</span></span>
<span data-ttu-id="5f9e3-148">V této části vytvoříte toohold pacienta dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-148">In this section, you will create a table toohold patient data.</span></span> <span data-ttu-id="5f9e3-149">To bude normální tabulku původně – budete konfigurovat šifrování v další části hello.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-149">This will be a normal table initially--you will configure encryption in hello next section.</span></span>

1. <span data-ttu-id="5f9e3-150">Rozbalte položku **databáze**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-150">Expand **Databases**.</span></span>
2. <span data-ttu-id="5f9e3-151">Klikněte pravým tlačítkem na hello **Klinika** databáze a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-151">Right-click hello **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="5f9e3-152">Vložení hello následující Transact-SQL (T-SQL) do nové okno dotazu hello a **Execute** ho.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-152">Paste hello following Transact-SQL (T-SQL) into hello new query window and **Execute** it.</span></span>

        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="5f9e3-153">Šifrování sloupců (nakonfigurujte funkce Always Encrypted)</span><span class="sxs-lookup"><span data-stu-id="5f9e3-153">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="5f9e3-154">Poskytuje průvodce, aplikace SSMS tooeasily nakonfigurovat tak, že hello CMK CEK a šifrované sloupce můžete vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-154">SSMS provides a wizard tooeasily configure Always Encrypted by setting up hello CMK, CEK, and encrypted columns for you.</span></span>

1. <span data-ttu-id="5f9e3-155">Rozbalte položku **databáze** > **Klinika** > **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-155">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="5f9e3-156">Klikněte pravým tlačítkem na hello **pacientů** tabulky a vyberte **šifrování sloupců** tooopen hello Always Encrypted průvodce:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-156">Right-click hello **Patients** table and select **Encrypt Columns** tooopen hello Always Encrypted wizard:</span></span>
   
    ![Šifrování sloupců](./media/sql-database-always-encrypted/encrypt-columns.png)

<span data-ttu-id="5f9e3-158">Hello Always Encrypted Průvodce obsahuje následující části hello: **sloupců výběru**, **konfigurace hlavního klíče** (CMK), **ověření**, a  **Souhrn**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-158">hello Always Encrypted wizard includes hello following sections: **Column Selection**, **Master Key Configuration** (CMK), **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="5f9e3-159">Výběr sloupce</span><span class="sxs-lookup"><span data-stu-id="5f9e3-159">Column Selection</span></span>
<span data-ttu-id="5f9e3-160">Klikněte na tlačítko **Další** na hello **ÚVOD** stránku hello tooopen **sloupců výběru** stránky.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-160">Click **Next** on hello **Introduction** page tooopen hello **Column Selection** page.</span></span> <span data-ttu-id="5f9e3-161">Na této stránce se vybrat sloupce, které chcete tooencrypt, [hello typ šifrování a jaké šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-161">On this page, you will select which columns you want tooencrypt, [hello type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span></span>

<span data-ttu-id="5f9e3-162">Šifrování **SSN** a **datum narození** informace pro každý pacienta.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-162">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="5f9e3-163">Hello **SSN** používat deterministickou šifrování, která podporuje rovnosti vyhledávání, spojení a seskupit podle sloupce.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-163">hello **SSN** column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="5f9e3-164">Hello **datum narození** sloupec použije náhodnou šifrování, která nepodporuje operace.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-164">hello **BirthDate** column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="5f9e3-165">Sada hello **typ šifrování** pro hello **SSN** sloupec příliš**Deterministic** a hello **datum narození** sloupec příliš **Náhodně posunut**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-165">Set hello **Encryption Type** for hello **SSN** column too**Deterministic** and hello **BirthDate** column too**Randomized**.</span></span> <span data-ttu-id="5f9e3-166">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-166">Click **Next**.</span></span>

![Šifrování sloupců](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="5f9e3-168">Konfigurace hlavního klíče</span><span class="sxs-lookup"><span data-stu-id="5f9e3-168">Master Key Configuration</span></span>
<span data-ttu-id="5f9e3-169">Hello **hlavní klíč konfigurace** stránka je, kde můžete nastavit CMK a zprostředkovatele úložiště klíčů vyberte hello hello CMK uložení.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-169">hello **Master Key Configuration** page is where you set up your CMK and select hello key store provider where hello CMK will be stored.</span></span> <span data-ttu-id="5f9e3-170">V současné době můžete uložit CMK v úložišti certifikátů Windows hello, Azure Key Vault nebo modul hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-170">Currently, you can store a CMK in hello Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span> <span data-ttu-id="5f9e3-171">Tento kurz ukazuje, jak toostore klíče v certifikátu Windows hello uložit.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-171">This tutorial shows how toostore your keys in hello Windows certificate store.</span></span>

<span data-ttu-id="5f9e3-172">Ověřte, že **úložiště certifikátů Windows** je vybrána a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-172">Verify that **Windows certificate store** is selected and click **Next**.</span></span>

![Konfigurace hlavního klíče](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="5f9e3-174">Ověření</span><span class="sxs-lookup"><span data-stu-id="5f9e3-174">Validation</span></span>
<span data-ttu-id="5f9e3-175">Můžete šifrovat hello sloupce nyní nebo později uložit toorun skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-175">You can encrypt hello columns now or save a PowerShell script toorun later.</span></span> <span data-ttu-id="5f9e3-176">V tomto kurzu vyberte **teď pokračovat toofinish** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-176">For this tutorial, select **Proceed toofinish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="5f9e3-177">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5f9e3-177">Summary</span></span>
<span data-ttu-id="5f9e3-178">Ověřte, zda jsou všechny správné hello nastavení a klikněte na tlačítko **Dokončit** toocomplete hello nastavení vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-178">Verify that hello settings are all correct and click **Finish** toocomplete hello setup for Always Encrypted.</span></span>

![Souhrn](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-hello-wizards-actions"></a><span data-ttu-id="5f9e3-180">Ověření akce průvodce hello</span><span class="sxs-lookup"><span data-stu-id="5f9e3-180">Verify hello wizard's actions</span></span>
<span data-ttu-id="5f9e3-181">Po dokončení Průvodce hello vaše databáze je nastavena pro vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-181">After hello wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="5f9e3-182">Hello hello na průvodce provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-182">hello wizard performed hello following actions:</span></span>

* <span data-ttu-id="5f9e3-183">Vytvořit CMK.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-183">Created a CMK.</span></span>
* <span data-ttu-id="5f9e3-184">Vytvořit CEK.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-184">Created a CEK.</span></span>
* <span data-ttu-id="5f9e3-185">Nakonfigurované hello vybrané sloupce pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-185">Configured hello selected columns for encryption.</span></span> <span data-ttu-id="5f9e3-186">Vaše **pacientů** tabulka nyní neobsahuje žádná data, ale stávající data v hello vybrané sloupce je zašifrovaný.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-186">Your **Patients** table currently has no data, but any existing data in hello selected columns is now encrypted.</span></span>

<span data-ttu-id="5f9e3-187">Vytvoření hello hello klíčů v aplikaci SSMS můžete ověřit tak, že přejdete příliš**Klinika** > **zabezpečení** > **vždy šifrované klíče**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-187">You can verify hello creation of hello keys in SSMS by going too**Clinic** > **Security** > **Always Encrypted Keys**.</span></span> <span data-ttu-id="5f9e3-188">Nyní můžete vidět hello nových klíčů, které hello Průvodce pro vás vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-188">You can now see hello new keys that hello wizard generated for you.</span></span>

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a><span data-ttu-id="5f9e3-189">Vytvořit klientskou aplikaci, která funguje s hello šifrovaná data</span><span class="sxs-lookup"><span data-stu-id="5f9e3-189">Create a client application that works with hello encrypted data</span></span>
<span data-ttu-id="5f9e3-190">Teď, když je funkce Always Encrypted nastavili, můžete vytvořit aplikaci, která provádí *vloží* a *vybere* na hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-190">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on hello encrypted columns.</span></span> <span data-ttu-id="5f9e3-191">toosuccessfully hello ukázkovou aplikaci spustit, je nutné jej spustit na hello stejný počítač, kde jste spustili Průvodce vždycky šifrovaná hello.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-191">toosuccessfully run hello sample application, you must run it on hello same computer where you ran hello Always Encrypted wizard.</span></span> <span data-ttu-id="5f9e3-192">aplikace hello toorun na jiném počítači, je nutné nasadit Always Encrypted certifikáty toohello počítače se spuštěným systémem hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-192">toorun hello application on another computer, you must deploy your Always Encrypted certificates toohello computer running hello client app.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="5f9e3-193">Vaše aplikace musí používat [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekty při předávání server toohello data ve formátu prostého textu se vždy šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-193">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data toohello server with Always Encrypted columns.</span></span> <span data-ttu-id="5f9e3-194">Předávání hodnot literál bez použití SqlParameter objekty způsobí výjimku.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-194">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="5f9e3-195">Otevřete Visual Studio a vytvořte novou aplikaci konzoly C#.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-195">Open Visual Studio and create a new C# console application.</span></span> <span data-ttu-id="5f9e3-196">Zajistěte, aby váš projekt je nastaven příliš**rozhraní .NET Framework 4.6** nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-196">Make sure your project is set too**.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="5f9e3-197">Název projektu hello **AlwaysEncryptedConsoleApp** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-197">Name hello project **AlwaysEncryptedConsoleApp** and click **OK**.</span></span>

![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-tooenable-always-encrypted"></a><span data-ttu-id="5f9e3-199">Upravit vaše tooenable řetězec připojení vždycky šifrovaná</span><span class="sxs-lookup"><span data-stu-id="5f9e3-199">Modify your connection string tooenable Always Encrypted</span></span>
<span data-ttu-id="5f9e3-200">Tato část vysvětluje, jak tooenable vždy zašifrované v připojovací řetězec databáze.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-200">This section explains how tooenable Always Encrypted in your database connection string.</span></span> <span data-ttu-id="5f9e3-201">Upraví hello konzolovou aplikaci, kterou jste právě vytvořili, v další části hello "Always Encrypted ukázkovou aplikaci konzoly."</span><span class="sxs-lookup"><span data-stu-id="5f9e3-201">You will modify hello console app you just created in hello next section, "Always Encrypted sample console application."</span></span>

<span data-ttu-id="5f9e3-202">tooenable vždycky šifrovaná, budete potřebovat tooadd hello **nastavení šifrování sloupec** – klíčové slovo tooyour připojení řetězce a nastavte ji příliš**povoleno**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-202">tooenable Always Encrypted, you need tooadd hello **Column Encryption Setting** keyword tooyour connection string and set it too**Enabled**.</span></span>

<span data-ttu-id="5f9e3-203">To můžete nastavit přímo v hello připojovací řetězec, nebo můžete ho nastavit pomocí [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-203">You can set this directly in hello connection string, or you can set it by using a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="5f9e3-204">Hello ukázkovou aplikaci v hello další část ukazuje, jak toouse **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-204">hello sample application in hello next section shows how toouse **SqlConnectionStringBuilder**.</span></span>

> [!NOTE]
> <span data-ttu-id="5f9e3-205">Toto je vyžadována v konkrétní tooAlways klienta aplikace šifrovaný pouze změna hello.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-205">This is hello only change required in a client application specific tooAlways Encrypted.</span></span> <span data-ttu-id="5f9e3-206">Pokud máte existující aplikace, která ukládá externě svém připojovacím řetězci (který je v konfiguračním souboru), je možné tooenable vždy šifrována aniž byste měnili kód.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-206">If you have an existing application that stores its connection string externally (that is, in a config file), you might be able tooenable Always Encrypted without changing any code.</span></span>
> 
> 

### <a name="enable-always-encrypted-in-hello-connection-string"></a><span data-ttu-id="5f9e3-207">Povolení funkce Always Encrypted v připojovacím řetězci hello</span><span class="sxs-lookup"><span data-stu-id="5f9e3-207">Enable Always Encrypted in hello connection string</span></span>
<span data-ttu-id="5f9e3-208">Přidejte následující připojovací řetězec – klíčové slovo tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-208">Add hello following keyword tooyour connection string:</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a><span data-ttu-id="5f9e3-209">Povolit vždy šifrován SqlConnectionStringBuilder</span><span class="sxs-lookup"><span data-stu-id="5f9e3-209">Enable Always Encrypted with a SqlConnectionStringBuilder</span></span>
<span data-ttu-id="5f9e3-210">Hello následující kód ukazuje, jak tooenable vždy šifrována nastavením hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) příliš[povoleno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-210">hello following code shows how tooenable Always Encrypted by setting hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) too[Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="5f9e3-211">Vždy šifrovaný ukázkovou aplikaci konzoly</span><span class="sxs-lookup"><span data-stu-id="5f9e3-211">Always Encrypted sample console application</span></span>
<span data-ttu-id="5f9e3-212">Tento příklad znázorňuje postup:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-212">This sample demonstrates how to:</span></span>

* <span data-ttu-id="5f9e3-213">Upravte vaše tooenable řetězec připojení vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-213">Modify your connection string tooenable Always Encrypted.</span></span>
* <span data-ttu-id="5f9e3-214">Vložení dat do hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-214">Insert data into hello encrypted columns.</span></span>
* <span data-ttu-id="5f9e3-215">Vyberte záznam pomocí filtrování pro konkrétní hodnoty v šifrovaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-215">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="5f9e3-216">Nahraďte obsah hello **Program.cs** s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-216">Replace hello contents of **Program.cs** with hello following code.</span></span> <span data-ttu-id="5f9e3-217">Nahraďte hello připojovací řetězec pro proměnnou globální connectionString hello v řádku hello přímo nad hello metodu Main platný připojovací řetězec z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-217">Replace hello connection string for hello global connectionString variable in hello line directly above hello Main method with your valid connection string from hello Azure portal.</span></span> <span data-ttu-id="5f9e3-218">Toto je jedinou změnou hello potřebujete toomake toothis kódu.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-218">This is hello only change you need toomake toothis code.</span></span>

<span data-ttu-id="5f9e3-219">Spusťte toosee aplikace hello vždycky šifrovaná v akci.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-219">Run hello app toosee Always Encrypted in action.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from hello Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from hello Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for hello connection.
            // This is hello only change specific tooAlways Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update hello connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign hello updated connection string tooour global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records toorestart this demo app.
            ResetPatientsTable();

            // Add sample data toohello Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data toohello database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All hello records currently in hello Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching hello encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that hello user entered 11 characters.
            // In production be sure toocheck all user input and use hello best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // hello example allows duplicate SSN entries so we will return all records
            // that match hello provided value and store hello results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter tooexit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("hello following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key tooexit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in hello Patients table tooreset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-hello-data-is-encrypted"></a><span data-ttu-id="5f9e3-220">Ověřte, zda text hello data se šifrují</span><span class="sxs-lookup"><span data-stu-id="5f9e3-220">Verify that hello data is encrypted</span></span>
<span data-ttu-id="5f9e3-221">Můžete rychle zjistit, že hello skutečná data na serveru hello je šifrovaná pomocí dotazu hello **pacientů** dat pomocí aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-221">You can quickly check that hello actual data on hello server is encrypted by querying hello **Patients** data with SSMS.</span></span> <span data-ttu-id="5f9e3-222">(Použít aktuální připojení kde nastavení šifrování pro sloupce hello zatím není povolená.)</span><span class="sxs-lookup"><span data-stu-id="5f9e3-222">(Use your current connection where hello column encryption setting is not yet enabled.)</span></span>

<span data-ttu-id="5f9e3-223">Spusťte následující dotaz na databázi Klinika hello hello.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-223">Run hello following query on hello Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="5f9e3-224">Uvidíte, že hello šifrované sloupce neobsahují žádná data ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-224">You can see that hello encrypted columns do not contain any plaintext data.</span></span>

   ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/ssms-encrypted.png)

<span data-ttu-id="5f9e3-226">toouse SSMS tooaccess hello data ve formátu prostého textu, můžete přidat hello **nastavení šifrování sloupec = povoleno** parametr toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-226">toouse SSMS tooaccess hello plaintext data, you can add hello **Column Encryption Setting=enabled** parameter toohello connection.</span></span>

1. <span data-ttu-id="5f9e3-227">V aplikaci SSMS, klikněte pravým tlačítkem na váš server v **Průzkumník objektů**a potom klikněte na **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-227">In SSMS, right-click your server in **Object Explorer**, and then click **Disconnect**.</span></span>
2. <span data-ttu-id="5f9e3-228">Klikněte na tlačítko **připojit** > **databázový stroj** tooopen hello **připojit tooServer** okna a pak klikněte na tlačítko **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-228">Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window, and then click **Options**.</span></span>
3. <span data-ttu-id="5f9e3-229">Klikněte na tlačítko **další parametry připojení** a typ **nastavení šifrování sloupec = povoleno**.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-229">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. <span data-ttu-id="5f9e3-231">Spuštění hello následující dotaz na hello **Klinika** databáze.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-231">Run hello following query on hello **Clinic** database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="5f9e3-232">Nyní můžete vidět data ve formátu prostého textu hello v hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-232">You can now see hello plaintext data in hello encrypted columns.</span></span>

    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> <span data-ttu-id="5f9e3-234">Pokud se připojujete pomocí aplikace SSMS (nebo libovolného klienta) z jiného počítače, nebudete mít přístup toohello šifrovacích klíčů a nebudou moct toodecrypt hello data.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-234">If you connect with SSMS (or any client) from a different computer, it will not have access toohello encryption keys and will not be able toodecrypt hello data.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5f9e3-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f9e3-235">Next steps</span></span>
<span data-ttu-id="5f9e3-236">Po vytvoření databáze, která používá vždycky šifrovaná, může být vhodné toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="5f9e3-236">After you create a database that uses Always Encrypted, you may want toodo hello following:</span></span>

* <span data-ttu-id="5f9e3-237">Tuto ukázku spusťte z jiného počítače.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-237">Run this sample from a different computer.</span></span> <span data-ttu-id="5f9e3-238">Nebude mít přístup toohello šifrovací klíče, takže ho nebude mít přístup k datům ve formátu prostého textu toohello a nespustí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="5f9e3-238">It won't have access toohello encryption keys, so it will not have access toohello plaintext data and will not run successfully.</span></span>
* <span data-ttu-id="5f9e3-239">[Otočit a vyčištění klíče](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-239">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="5f9e3-240">[Migraci dat, která už je šifrovaný pomocí funkce Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-240">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>
* <span data-ttu-id="5f9e3-241">[Nasazení funkce Always Encrypted certifikáty tooother klientské počítače](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (viz část "Provedení certifikáty k dispozici tooApplications a uživatelé" hello).</span><span class="sxs-lookup"><span data-stu-id="5f9e3-241">[Deploy Always Encrypted certificates tooother client machines](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (see hello "Making Certificates Available tooApplications and Users" section).</span></span>

## <a name="related-information"></a><span data-ttu-id="5f9e3-242">Související informace</span><span class="sxs-lookup"><span data-stu-id="5f9e3-242">Related information</span></span>
* [<span data-ttu-id="5f9e3-243">Funkce Always Encrypted (vývoj pro klienta)</span><span class="sxs-lookup"><span data-stu-id="5f9e3-243">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="5f9e3-244">Transparentní šifrování dat</span><span class="sxs-lookup"><span data-stu-id="5f9e3-244">Transparent Data Encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="5f9e3-245">Šifrování SQL serveru</span><span class="sxs-lookup"><span data-stu-id="5f9e3-245">SQL Server Encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="5f9e3-246">Vždy šifrované Průvodce</span><span class="sxs-lookup"><span data-stu-id="5f9e3-246">Always Encrypted Wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="5f9e3-247">Vždy šifrované Blog</span><span class="sxs-lookup"><span data-stu-id="5f9e3-247">Always Encrypted Blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

