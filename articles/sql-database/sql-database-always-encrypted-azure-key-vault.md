---
title: "Funkce Always Encrypted: SQL Database – Azure Key Vault | Microsoft Docs"
description: "Tento článek ukazuje, jak zajistit citlivá data v databázi SQL s šifrování dat pomocí Průvodce vždycky šifrovaná v aplikaci SQL Server Management Studio. Zahrnuje také pokyny, které vám ukáže, jak ukládat každý šifrovací klíč v Azure Key Vault."
keywords: "šifrování dat, šifrovací klíč, šifrování cloudu"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 6ca16644-5969-497b-a413-d28c3b835c9b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: sstein
ms.openlocfilehash: 61bfd420425b4740f6d4ebc01a403a88ff351382
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a><span data-ttu-id="04da0-105">Funkce Always Encrypted: Chrání citlivá data v databázi SQL a ukládat šifrovací klíče v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="04da0-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in Azure Key Vault</span></span>

<span data-ttu-id="04da0-106">V tomto článku se dozvíte, jak zajistit citlivá data v databázi SQL s použitím šifrování dat [vždy šifrované průvodce](https://msdn.microsoft.com/library/mt459280.aspx) v [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-106">This article shows you how to secure sensitive data in a SQL database with data encryption using the [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="04da0-107">Zahrnuje také pokyny, které vám ukáže, jak ukládat každý šifrovací klíč v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="04da0-107">It also includes instructions that will show you how to store each encryption key in Azure Key Vault.</span></span>

<span data-ttu-id="04da0-108">Vždy šifrovaný je nová technologie šifrování dat v Azure SQL Database a SQL Server, který pomáhá chránit citlivá data v klidovém stavu na serveru během pohybu mezi klientem a serverem, a když data právě používá.</span><span class="sxs-lookup"><span data-stu-id="04da0-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on the server, during movement between client and server, and while the data is in use.</span></span> <span data-ttu-id="04da0-109">Vždy šifrovaný zajistí, že citlivá data nikdy zobrazí jako prostý text v databázi systému.</span><span class="sxs-lookup"><span data-stu-id="04da0-109">Always Encrypted ensures that sensitive data never appears as plaintext inside the database system.</span></span> <span data-ttu-id="04da0-110">Po dokončení konfigurace šifrování dat, můžete přístup jenom klientské aplikace nebo aplikační servery, které mají přístup ke klíčům data ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="04da0-110">After you configure data encryption, only client applications or app servers that have access to the keys can access plaintext data.</span></span> <span data-ttu-id="04da0-111">Podrobné informace najdete v tématu [vždycky šifrovaná (databázový stroj)](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-111">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="04da0-112">Po dokončení konfigurace databáze pro použití funkce Always Encrypted, vytvoříte klientskou aplikaci v jazyce C# pomocí sady Visual Studio pro práci s šifrovaná data.</span><span class="sxs-lookup"><span data-stu-id="04da0-112">After you configure the database to use Always Encrypted, you will create a client application in C# with Visual Studio to work with the encrypted data.</span></span>

<span data-ttu-id="04da0-113">Postupujte podle kroků v tomto článku a zjistěte, jak nastavit vždy šifrována pro Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="04da0-113">Follow the steps in this article and learn how to set up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="04da0-114">V tomto článku se dozvíte, jak provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="04da0-114">In this article you will learn how to perform the following tasks:</span></span>

* <span data-ttu-id="04da0-115">Pomocí Průvodce vždycky šifrovaná v aplikaci SSMS vytvořit [vždy šifrována klíče](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="04da0-115">Use the Always Encrypted wizard in SSMS to create [Always Encrypted keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="04da0-116">Vytvoření [k hlavnímu klíči sloupce (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-116">Create a [column master key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="04da0-117">Vytvoření [šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-117">Create a [column encryption key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="04da0-118">Vytvořit tabulku databáze a šifrování sloupců.</span><span class="sxs-lookup"><span data-stu-id="04da0-118">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="04da0-119">Vytvořte aplikaci, která vloží, vybere a zobrazí data z šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="04da0-119">Create an application that inserts, selects, and displays data from the encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04da0-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04da0-120">Prerequisites</span></span>
<span data-ttu-id="04da0-121">V tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="04da0-121">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="04da0-122">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="04da0-122">An Azure account and subscription.</span></span> <span data-ttu-id="04da0-123">Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04da0-123">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="04da0-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verze 13.0.700.242 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="04da0-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="04da0-125">[Rozhraní .NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) nebo novější (v klientském počítači).</span><span class="sxs-lookup"><span data-stu-id="04da0-125">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on the client computer).</span></span>
* <span data-ttu-id="04da0-126">Sadu [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>
* <span data-ttu-id="04da0-127">[Prostředí Azure PowerShell](/powershell/azure/overview), verze 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="04da0-127">[Azure PowerShell](/powershell/azure/overview), version  1.0 or later.</span></span> <span data-ttu-id="04da0-128">Typ **(Get-Module azure - ListAvailable). Verze** zobrazíte, jaká verze prostředí PowerShell, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="04da0-128">Type **(Get-Module azure -ListAvailable).Version** to see what version of PowerShell you are running.</span></span>

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a><span data-ttu-id="04da0-129">Povolit klientské aplikaci přístup ke službě SQL Database</span><span class="sxs-lookup"><span data-stu-id="04da0-129">Enable your client application to access the SQL Database service</span></span>
<span data-ttu-id="04da0-130">Je nutné povolit klientské aplikaci přístup ke službě SQL Database nastavením potřebného ověřování a získání *ClientId* a *tajný klíč* kterou budete potřebovat k ověření vaší aplikace v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="04da0-130">You must enable your client application to access the SQL Database service by setting up the required authentication and acquiring the *ClientId* and *Secret* that you will need to authenticate your application in the following code.</span></span>

1. <span data-ttu-id="04da0-131">Otevřete [portál Azure classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="04da0-131">Open the [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="04da0-132">Vyberte **služby Active Directory** a klikněte na instanci služby Active Directory, která vaše aplikace bude používat.</span><span class="sxs-lookup"><span data-stu-id="04da0-132">Select **Active Directory** and click the Active Directory instance that your application will use.</span></span>
3. <span data-ttu-id="04da0-133">Klikněte na tlačítko **aplikace**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="04da0-133">Click **Applications**, and then click **ADD**.</span></span>
4. <span data-ttu-id="04da0-134">Zadejte název pro vaši aplikaci (například: *myClientApp*), vyberte **webové aplikace**a klikněte na šipku pokračujte.</span><span class="sxs-lookup"><span data-stu-id="04da0-134">Type a name for your application (for example: *myClientApp*), select **WEB APPLICATION**, and click the arrow to continue.</span></span>
5. <span data-ttu-id="04da0-135">Pro **adresa URL přihlašování** a **identifikátor ID URI aplikace** můžete zadat platnou adresu URL (například *http://myClientApp*) a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="04da0-135">For the **SIGN-ON URL** and **APP ID URI** you can type a valid URL (for example, *http://myClientApp*) and continue.</span></span>
6. <span data-ttu-id="04da0-136">Klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="04da0-136">Click **CONFIGURE**.</span></span>
7. <span data-ttu-id="04da0-137">Kopie vašeho **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="04da0-137">Copy your **CLIENT ID**.</span></span> <span data-ttu-id="04da0-138">(Budete potřebovat tuto hodnotu v kódu později.)</span><span class="sxs-lookup"><span data-stu-id="04da0-138">(You will need this value in your code later.)</span></span>
8. <span data-ttu-id="04da0-139">V **klíče** vyberte **1 rok** z **vyberte dobu trvání** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="04da0-139">In the **keys** section, select **1 year** from the  **Select duration** drop-down list.</span></span> <span data-ttu-id="04da0-140">(Klíč bude zkopírujte po uložení v kroku 13.)</span><span class="sxs-lookup"><span data-stu-id="04da0-140">(You will copy the key after you save in step 13.)</span></span>
9. <span data-ttu-id="04da0-141">Posuňte se dolů a klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="04da0-141">Scroll down and click **Add application**.</span></span>
10. <span data-ttu-id="04da0-142">Nechte **zobrazit** nastavena na **Microsoft Apps** a vyberte **Microsoft Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="04da0-142">Leave **SHOW** set to **Microsoft Apps** and select **Microsoft Azure Service Management API**.</span></span> <span data-ttu-id="04da0-143">Kliknutím na značku zaškrtnutí pokračujte.</span><span class="sxs-lookup"><span data-stu-id="04da0-143">Click the checkmark to continue.</span></span>
11. <span data-ttu-id="04da0-144">Vyberte **přístup k Azure Service Management...**  z **delegovaná oprávnění** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="04da0-144">Select **Access Azure Service Management...** from the **Delegated Permissions** drop-down list.</span></span>
12. <span data-ttu-id="04da0-145">Klikněte na **ULOŽIT**.</span><span class="sxs-lookup"><span data-stu-id="04da0-145">Click **SAVE**.</span></span>
13. <span data-ttu-id="04da0-146">Po uložení dokončí, zkopírujte hodnotu klíče v **klíče** části.</span><span class="sxs-lookup"><span data-stu-id="04da0-146">After the save finishes, copy the key value in the **keys** section.</span></span> <span data-ttu-id="04da0-147">(Budete potřebovat tuto hodnotu v kódu později.)</span><span class="sxs-lookup"><span data-stu-id="04da0-147">(You will need this value in your code later.)</span></span>

## <a name="create-a-key-vault-to-store-your-keys"></a><span data-ttu-id="04da0-148">Vytvoření trezoru klíčů k ukládání svých klíčů</span><span class="sxs-lookup"><span data-stu-id="04da0-148">Create a key vault to store your keys</span></span>
<span data-ttu-id="04da0-149">Teď, když je nastavené vaší klientské aplikace a máte vaše ID klienta, je čas na vytvoření trezoru klíčů a nakonfigurujte jeho zásady přístupu tak, aby vám a vaší aplikace mohou přistupovat k trezoru tajné klíče (Always Encrypted klíče).</span><span class="sxs-lookup"><span data-stu-id="04da0-149">Now that your client app is configured and you have your client ID, it's time to create a key vault and configure its access policy so you and your application can access the vault's secrets (the Always Encrypted keys).</span></span> <span data-ttu-id="04da0-150">*Vytvořit*, *získat*, *seznamu*, *přihlašovací*, *ověřte*, *wrapKey*, a *unwrapKey* oprávnění jsou nutné pro vytvoření nové k hlavnímu klíči sloupce a nastavení šifrování s SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="04da0-150">The *create*, *get*, *list*, *sign*, *verify*, *wrapKey*, and *unwrapKey* permissions are required for creating a new column master key and for setting up encryption with SQL Server Management Studio.</span></span>

<span data-ttu-id="04da0-151">Spuštěním následujícího skriptu můžete rychle vytvořit trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="04da0-151">You can quickly create a key vault by running the following script.</span></span> <span data-ttu-id="04da0-152">Podrobné vysvětlení těchto rutin a další informace o vytváření a konfiguraci trezoru klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="04da0-152">For a detailed explanation of these cmdlets and more information about creating and configuring a key vault, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>

    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).Id
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a><span data-ttu-id="04da0-153">Vytvořit prázdnou databázi SQL</span><span class="sxs-lookup"><span data-stu-id="04da0-153">Create a blank SQL database</span></span>
1. <span data-ttu-id="04da0-154">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="04da0-154">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="04da0-155">Přejděte na **nové** > **Data + úložiště** > **databáze SQL**.</span><span class="sxs-lookup"><span data-stu-id="04da0-155">Go to **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="04da0-156">Vytvoření **prázdné** databáze s názvem **Klinika** na nový nebo existující server.</span><span class="sxs-lookup"><span data-stu-id="04da0-156">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="04da0-157">Podrobné pokyny o tom, jak vytvořit databázi na portálu Azure, najdete v části [svoji první databázi Azure SQL](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="04da0-157">For detailed directions about how to create a database in the Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Vytvoření prázdné databáze](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

<span data-ttu-id="04da0-159">Budete potřebovat připojení řetězec později v tomto kurzu, takže po vytvoření databáze, přejděte k nové databázi Klinika a zkopírujte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="04da0-159">You will need the connection string later in the tutorial, so after you create the database, browse to the new  Clinic database and copy the connection string.</span></span> <span data-ttu-id="04da0-160">Kdykoli můžete získat připojovací řetězec, ale je snadné a zkopírujte ho na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="04da0-160">You can get the connection string at any time, but it's easy to copy it in the Azure portal.</span></span>

1. <span data-ttu-id="04da0-161">Přejděte na **databází SQL** > **Klinika** > **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="04da0-161">Go to **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="04da0-162">Zkopírujte připojovací řetězec pro **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="04da0-162">Copy the connection string for **ADO.NET**.</span></span>
   
    ![Zkopírujte připojovací řetězec](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a><span data-ttu-id="04da0-164">Připojení k databázi pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="04da0-164">Connect to the database with SSMS</span></span>
<span data-ttu-id="04da0-165">Otevřete aplikaci SSMS a připojení k serveru s databází Klinika.</span><span class="sxs-lookup"><span data-stu-id="04da0-165">Open SSMS and connect to the server with the Clinic database.</span></span>

1. <span data-ttu-id="04da0-166">Otevřete aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="04da0-166">Open SSMS.</span></span> <span data-ttu-id="04da0-167">(Přejít na **připojit** > **databázový stroj** otevřete **připojit k serveru** okno, pokud není otevřený.)</span><span class="sxs-lookup"><span data-stu-id="04da0-167">(Go to **Connect** > **Database Engine** to open the **Connect to Server** window if it isn't open.)</span></span>
2. <span data-ttu-id="04da0-168">Zadejte název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="04da0-168">Enter your server name and credentials.</span></span> <span data-ttu-id="04da0-169">Název serveru naleznete v okně databáze SQL a v připojovacím řetězci jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="04da0-169">The server name can be found on the SQL database blade and in the connection string you copied earlier.</span></span> <span data-ttu-id="04da0-170">Zadejte úplný název serveru, včetně *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="04da0-170">Type the complete server name, including *database.windows.net*.</span></span>
   
    ![Zkopírujte připojovací řetězec](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

<span data-ttu-id="04da0-172">Pokud **nové pravidlo brány Firewall** okno otevře, přihlaste k Azure a umožňují SSMS za vás vytvořit nové pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="04da0-172">If the **New Firewall Rule** window opens, sign in to Azure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="04da0-173">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="04da0-173">Create a table</span></span>
<span data-ttu-id="04da0-174">V této části vytvoříte tabulku pro uložení pacienta data.</span><span class="sxs-lookup"><span data-stu-id="04da0-174">In this section, you will create a table to hold patient data.</span></span> <span data-ttu-id="04da0-175">Není původně zašifrována – budete konfigurovat šifrování v další části.</span><span class="sxs-lookup"><span data-stu-id="04da0-175">It's not initially encrypted--you will configure encryption in the next section.</span></span>

1. <span data-ttu-id="04da0-176">Rozbalte položku **databáze**.</span><span class="sxs-lookup"><span data-stu-id="04da0-176">Expand **Databases**.</span></span>
2. <span data-ttu-id="04da0-177">Klikněte pravým tlačítkem myši **Klinika** databáze a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="04da0-177">Right-click the **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="04da0-178">Vložte následující Transact-SQL (T-SQL) do nového okna dotazu a **Execute** ho.</span><span class="sxs-lookup"><span data-stu-id="04da0-178">Paste the following Transact-SQL (T-SQL) into the new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="04da0-179">Šifrování sloupců (nakonfigurujte funkce Always Encrypted)</span><span class="sxs-lookup"><span data-stu-id="04da0-179">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="04da0-180">Aplikace SSMS poskytuje průvodce, který vám pomůže snadno nakonfigurovat tak, že k hlavnímu klíči sloupce, šifrovací klíč sloupce a šifrované sloupce můžete vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="04da0-180">SSMS provides a wizard that helps you easily configure Always Encrypted by setting up the column master key, column encryption key, and encrypted columns for you.</span></span>

1. <span data-ttu-id="04da0-181">Rozbalte položku **databáze** > **Klinika** > **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="04da0-181">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="04da0-182">Klikněte pravým tlačítkem myši **pacientů** tabulky a vyberte **šifrování sloupců** otevřete Průvodce vždycky šifrovaná:</span><span class="sxs-lookup"><span data-stu-id="04da0-182">Right-click the **Patients** table and select **Encrypt Columns** to open the Always Encrypted wizard:</span></span>
   
    ![Šifrování sloupců](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

<span data-ttu-id="04da0-184">Průvodce vždy šifrována obsahuje následující oddíly: **sloupců výběru**, **konfigurace hlavního klíče**, **ověření**, a **Souhrn**.</span><span class="sxs-lookup"><span data-stu-id="04da0-184">The Always Encrypted wizard includes the following sections: **Column Selection**, **Master Key Configuration**, **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="04da0-185">Výběr sloupce</span><span class="sxs-lookup"><span data-stu-id="04da0-185">Column Selection</span></span>
<span data-ttu-id="04da0-186">Klikněte na tlačítko **Další** na **ÚVOD** otevřít stránku **sloupců výběru** stránky.</span><span class="sxs-lookup"><span data-stu-id="04da0-186">Click **Next** on the **Introduction** page to open the **Column Selection** page.</span></span> <span data-ttu-id="04da0-187">Na této stránce se vybrat sloupce, které chcete šifrovat, [typ šifrování a jaké šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) používat.</span><span class="sxs-lookup"><span data-stu-id="04da0-187">On this page, you will select which columns you want to encrypt, [the type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) to use.</span></span>

<span data-ttu-id="04da0-188">Šifrování **SSN** a **datum narození** informace pro každý pacienta.</span><span class="sxs-lookup"><span data-stu-id="04da0-188">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="04da0-189">Sloupec SSN použije deterministickou šifrování, která podporuje rovnosti vyhledávání, spojení a seskupit podle.</span><span class="sxs-lookup"><span data-stu-id="04da0-189">The SSN column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="04da0-190">Sloupec Datum narození použije náhodnou šifrování, která nepodporuje operace.</span><span class="sxs-lookup"><span data-stu-id="04da0-190">The BirthDate column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="04da0-191">Nastavte **typ šifrování** sloupce SSN **Deterministic** a sloupci Datum narození **Randomized**.</span><span class="sxs-lookup"><span data-stu-id="04da0-191">Set the **Encryption Type** for the SSN column to **Deterministic** and the BirthDate column to **Randomized**.</span></span> <span data-ttu-id="04da0-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="04da0-192">Click **Next**.</span></span>

![Šifrování sloupců](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="04da0-194">Konfigurace hlavního klíče</span><span class="sxs-lookup"><span data-stu-id="04da0-194">Master Key Configuration</span></span>
<span data-ttu-id="04da0-195">**Hlavní klíč konfigurace** stránka je kde nastavení vaší CMK a vybrat zprostředkovatele úložiště klíčů uložení CMK.</span><span class="sxs-lookup"><span data-stu-id="04da0-195">The **Master Key Configuration** page is where you set up your CMK and select the key store provider where the CMK will be stored.</span></span> <span data-ttu-id="04da0-196">V současné době můžete uložit CMK v úložišti certifikátů systému Windows, Azure Key Vault nebo modul hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="04da0-196">Currently, you can store a CMK in the Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span>

<span data-ttu-id="04da0-197">Tento kurz ukazuje, jak uložit vaše klíče do Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="04da0-197">This tutorial shows how to store your keys in Azure Key Vault.</span></span>

1. <span data-ttu-id="04da0-198">Vyberte **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="04da0-198">Select **Azure Key Vault**.</span></span>
2. <span data-ttu-id="04da0-199">V rozevíracím seznamu vyberte požadovaný trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="04da0-199">Select the desired key vault from the drop-down list.</span></span>
3. <span data-ttu-id="04da0-200">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="04da0-200">Click **Next**.</span></span>

![Konfigurace hlavního klíče](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="04da0-202">Ověření</span><span class="sxs-lookup"><span data-stu-id="04da0-202">Validation</span></span>
<span data-ttu-id="04da0-203">Můžete teď šifrování sloupců nebo uložení skriptu prostředí PowerShell spustit později.</span><span class="sxs-lookup"><span data-stu-id="04da0-203">You can encrypt the columns now or save a PowerShell script to run later.</span></span> <span data-ttu-id="04da0-204">V tomto kurzu vyberte **přejít k dokončení teď** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="04da0-204">For this tutorial, select **Proceed to finish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="04da0-205">Souhrn</span><span class="sxs-lookup"><span data-stu-id="04da0-205">Summary</span></span>
<span data-ttu-id="04da0-206">Ověřte, zda jsou všechny správné nastavení a klikněte na tlačítko **Dokončit** dokončit nastavení pro vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="04da0-206">Verify that the settings are all correct and click **Finish** to complete the setup for Always Encrypted.</span></span>

![Souhrn](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a><span data-ttu-id="04da0-208">Ověřte v Průvodci akce</span><span class="sxs-lookup"><span data-stu-id="04da0-208">Verify the wizard's actions</span></span>
<span data-ttu-id="04da0-209">Po dokončení Průvodce vaše databáze je nastavena pro vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="04da0-209">After the wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="04da0-210">Průvodce provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="04da0-210">The wizard performed the following actions:</span></span>

* <span data-ttu-id="04da0-211">K hlavnímu klíči sloupce vytvoří a uloží ji do Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="04da0-211">Created a column master key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="04da0-212">Šifrovací klíč sloupce vytvoří a uloží ji do Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="04da0-212">Created a column encryption key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="04da0-213">Nakonfigurovat vybrané sloupce pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="04da0-213">Configured the selected columns for encryption.</span></span> <span data-ttu-id="04da0-214">Tabulky Pacienti aktuálně neobsahuje žádná data, ale je zašifrovaný jakákoli stávající data ve vybraných sloupcích.</span><span class="sxs-lookup"><span data-stu-id="04da0-214">The Patients table currently has no data, but any existing data in the selected columns is now encrypted.</span></span>

<span data-ttu-id="04da0-215">Vytvoření klíče v aplikaci SSMS můžete ověřit rozšířením **Klinika** > **zabezpečení** > **vždy šifrované klíče**.</span><span class="sxs-lookup"><span data-stu-id="04da0-215">You can verify the creation of the keys in SSMS by expanding **Clinic** > **Security** > **Always Encrypted Keys**.</span></span>

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a><span data-ttu-id="04da0-216">Vytvořit klientskou aplikaci, která funguje s šifrovaná data</span><span class="sxs-lookup"><span data-stu-id="04da0-216">Create a client application that works with the encrypted data</span></span>
<span data-ttu-id="04da0-217">Teď, když je funkce Always Encrypted nastavili, můžete vytvořit aplikaci, která provádí *vloží* a *vybere* pro šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="04da0-217">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on the encrypted columns.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="04da0-218">Vaše aplikace musí používat [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekty při předávání dat ve formátu prostého textu na server s vždy šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="04da0-218">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data to the server with Always Encrypted columns.</span></span> <span data-ttu-id="04da0-219">Předávání hodnot literál bez použití SqlParameter objekty způsobí výjimku.</span><span class="sxs-lookup"><span data-stu-id="04da0-219">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="04da0-220">Otevřete Visual Studio a vytvoření nového jazyka C# **konzolové aplikace** (Visual Studio 2015 a starší) nebo **konzolovou aplikaci (rozhraní .NET Framework)** (Visual Studio 2017 a novější).</span><span class="sxs-lookup"><span data-stu-id="04da0-220">Open Visual Studio and create a new C# **Console Application** (Visual Studio 2015 and earlier) or **Console App (.NET Framework)** (Visual Studio 2017 and later).</span></span> <span data-ttu-id="04da0-221">Ujistěte se, že váš projekt je nastaven na **rozhraní .NET Framework 4.6** nebo novější.</span><span class="sxs-lookup"><span data-stu-id="04da0-221">Make sure your project is set to **.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="04da0-222">Název projektu **AlwaysEncryptedConsoleAKVApp** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="04da0-222">Name the project **AlwaysEncryptedConsoleAKVApp** and click **OK**.</span></span>
3. <span data-ttu-id="04da0-223">Instalace následujících balíčků NuGet přechodem na **nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="04da0-223">Install the following NuGet packages by going to **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="04da0-224">Spusťte tyto dva řádky kódu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="04da0-224">Run these two lines of code in the Package Manager Console.</span></span>

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a><span data-ttu-id="04da0-225">Upravit připojovací řetězec k povolení funkce Always Encrypted</span><span class="sxs-lookup"><span data-stu-id="04da0-225">Modify your connection string to enable Always Encrypted</span></span>
<span data-ttu-id="04da0-226">Tato část vysvětluje, jak povolit funkce Always Encrypted v připojovací řetězec databáze.</span><span class="sxs-lookup"><span data-stu-id="04da0-226">This section  explains how to enable Always Encrypted in your database connection string.</span></span>

<span data-ttu-id="04da0-227">Pokud chcete povolit, vždycky šifrovaná, je nutné přidat **nastavení šifrování sloupec** – klíčové slovo k připojení k řetězec a nastavte ji na **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="04da0-227">To enable Always Encrypted, you need to add the **Column Encryption Setting** keyword to your connection string and set it to **Enabled**.</span></span>

<span data-ttu-id="04da0-228">To můžete nastavit přímo v připojovacím řetězci, nebo můžete ho nastavit pomocí [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-228">You can set this directly in the connection string, or you can set it by using [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="04da0-229">Ukázkovou aplikaci v další části ukazuje, jak používat **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="04da0-229">The sample application in the next section shows how to use **SqlConnectionStringBuilder**.</span></span>

### <a name="enable-always-encrypted-in-the-connection-string"></a><span data-ttu-id="04da0-230">Povolení funkce Always Encrypted v připojovacím řetězci</span><span class="sxs-lookup"><span data-stu-id="04da0-230">Enable Always Encrypted in the connection string</span></span>
<span data-ttu-id="04da0-231">Přidejte následující klíčové slovo do připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="04da0-231">Add the following keyword to your connection string.</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a><span data-ttu-id="04da0-232">Povolit vždy šifrován SqlConnectionStringBuilder</span><span class="sxs-lookup"><span data-stu-id="04da0-232">Enable Always Encrypted with SqlConnectionStringBuilder</span></span>
<span data-ttu-id="04da0-233">Následující kód ukazuje, jak povolit funkce Always Encrypted nastavením [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) k [povoleno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-233">The following code shows how to enable Always Encrypted by setting [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) to [Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a><span data-ttu-id="04da0-234">Zaregistrujte zprostředkovatele Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="04da0-234">Register the Azure Key Vault provider</span></span>
<span data-ttu-id="04da0-235">Následující kód ukazuje, jak zaregistrovat zprostředkovatele Azure Key Vault ovladač technologie ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="04da0-235">The following code shows how to register the Azure Key Vault provider with the ADO.NET driver.</span></span>

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="04da0-236">Vždy šifrovaný ukázkovou aplikaci konzoly</span><span class="sxs-lookup"><span data-stu-id="04da0-236">Always Encrypted sample console application</span></span>
<span data-ttu-id="04da0-237">Tento příklad znázorňuje postup:</span><span class="sxs-lookup"><span data-stu-id="04da0-237">This sample demonstrates how to:</span></span>

* <span data-ttu-id="04da0-238">Upravte připojovací řetězec k povolení funkce Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="04da0-238">Modify your connection string to enable Always Encrypted.</span></span>
* <span data-ttu-id="04da0-239">Zaregistrujte Azure Key Vault jako zprostředkovatele úložiště klíčů aplikace.</span><span class="sxs-lookup"><span data-stu-id="04da0-239">Register Azure Key Vault as the application's key store provider.</span></span>  
* <span data-ttu-id="04da0-240">Vložení dat do šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="04da0-240">Insert data into the encrypted columns.</span></span>
* <span data-ttu-id="04da0-241">Vyberte záznam pomocí filtrování pro konkrétní hodnoty v šifrovaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="04da0-241">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="04da0-242">Nahraďte obsah **Program.cs** následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="04da0-242">Replace the contents of **Program.cs** with the following code.</span></span> <span data-ttu-id="04da0-243">Nahraďte připojovací řetězec pro globální connectionString proměnné v řádku, který přímo předchází metodu Main platný připojovacím řetězcem z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="04da0-243">Replace the connection string for the global connectionString variable in the line that directly precedes the Main method with your valid connection string from the Azure portal.</span></span> <span data-ttu-id="04da0-244">Toto je pouze změny, které musíte udělat na tento kód.</span><span class="sxs-lookup"><span data-stu-id="04da0-244">This is the only change you need to make to this code.</span></span>

<span data-ttu-id="04da0-245">Spusťte aplikaci v akci zobrazovat vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="04da0-245">Run the app to see Always Encrypted in action.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
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

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
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
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
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


        // This method simply deletes all records in the Patients table to reset our demo.
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



## <a name="verify-that-the-data-is-encrypted"></a><span data-ttu-id="04da0-246">Ověřte, že je šifrovaná data</span><span class="sxs-lookup"><span data-stu-id="04da0-246">Verify that the data is encrypted</span></span>
<span data-ttu-id="04da0-247">Můžete rychle zjistit, že skutečná data na serveru je šifrovaná pomocí dotazu na data pacientů pomocí SSMS (pomocí aktuálního připojení kde **nastavení šifrování sloupec** zatím není povolená).</span><span class="sxs-lookup"><span data-stu-id="04da0-247">You can quickly check that the actual data on the server is encrypted by querying the Patients data with SSMS (using your current connection where **Column Encryption Setting** is not yet enabled).</span></span>

<span data-ttu-id="04da0-248">Spuštěním následujícího dotazu v databázi Klinika.</span><span class="sxs-lookup"><span data-stu-id="04da0-248">Run the following query on the Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="04da0-249">Uvidíte, že šifrované sloupce neobsahují žádná data ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="04da0-249">You can see that the encrypted columns do not contain any plaintext data.</span></span>

   ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

<span data-ttu-id="04da0-251">Pomocí aplikace SSMS přístup k datům ve formátu prostého textu, můžete přidat *nastavení šifrování sloupec = povoleno* parametr pro připojení.</span><span class="sxs-lookup"><span data-stu-id="04da0-251">To use SSMS to access the plaintext data, you can add the *Column Encryption Setting=enabled* parameter to the connection.</span></span>

1. <span data-ttu-id="04da0-252">V aplikaci SSMS, klikněte pravým tlačítkem na váš server v **Průzkumník objektů** a zvolte **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="04da0-252">In SSMS, right-click your server in **Object Explorer** and choose **Disconnect**.</span></span>
2. <span data-ttu-id="04da0-253">Klikněte na tlačítko **připojit** > **databázový stroj** otevřete **připojit k serveru** a klikněte na **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="04da0-253">Click **Connect** > **Database Engine** to open the **Connect to Server** window and click **Options**.</span></span>
3. <span data-ttu-id="04da0-254">Klikněte na tlačítko **další parametry připojení** a typ **nastavení šifrování sloupec = povoleno**.</span><span class="sxs-lookup"><span data-stu-id="04da0-254">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. <span data-ttu-id="04da0-256">Spuštěním následujícího dotazu v databázi Klinika.</span><span class="sxs-lookup"><span data-stu-id="04da0-256">Run the following query on the Clinic database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="04da0-257">Nyní můžete vidět data jako prostý text v šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="04da0-257">You can now see the plaintext data in the encrypted columns.</span></span>

    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a><span data-ttu-id="04da0-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04da0-259">Next steps</span></span>
<span data-ttu-id="04da0-260">Po vytvoření databáze, která používá vždycky šifrovaná, můžete provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="04da0-260">After you create a database that uses Always Encrypted, you may want to do the following:</span></span>

* <span data-ttu-id="04da0-261">[Otočit a vyčištění klíče](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-261">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="04da0-262">[Migraci dat, která už je šifrovaný pomocí funkce Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="04da0-262">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>

## <a name="related-information"></a><span data-ttu-id="04da0-263">Související informace</span><span class="sxs-lookup"><span data-stu-id="04da0-263">Related information</span></span>
* [<span data-ttu-id="04da0-264">Funkce Always Encrypted (vývoj pro klienta)</span><span class="sxs-lookup"><span data-stu-id="04da0-264">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="04da0-265">Transparentní šifrování dat</span><span class="sxs-lookup"><span data-stu-id="04da0-265">Transparent data encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="04da0-266">Šifrování SQL serveru</span><span class="sxs-lookup"><span data-stu-id="04da0-266">SQL Server encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="04da0-267">Vždy šifrovaný Průvodce</span><span class="sxs-lookup"><span data-stu-id="04da0-267">Always Encrypted wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="04da0-268">Vždy šifrované blog</span><span class="sxs-lookup"><span data-stu-id="04da0-268">Always Encrypted blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

