---
title: "Funkce Always Encrypted: SQL Database – Azure Key Vault | Microsoft Docs"
description: "Tento článek ukazuje, jak toosecure citlivá data v databázi SQL s použitím šifrování dat hello vždy šifrované průvodce v nástroji SQL Server Management Studio. Zahrnuje také pokyny, které vám ukáže, jak toostore každý šifrovací klíč v Azure Key Vault."
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
ms.openlocfilehash: 8226bfef584e979643f5bb0747d4df16569f8204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a><span data-ttu-id="5dda9-105">Funkce Always Encrypted: Chrání citlivá data v databázi SQL a ukládat šifrovací klíče v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5dda9-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in Azure Key Vault</span></span>

<span data-ttu-id="5dda9-106">Tento článek ukazuje, jak toosecure citlivá data v SQL databázi se šifrování dat pomocí hello [vždy šifrované průvodce](https://msdn.microsoft.com/library/mt459280.aspx) v [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-106">This article shows you how toosecure sensitive data in a SQL database with data encryption using hello [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="5dda9-107">Zahrnuje také pokyny, které vám ukáže, jak toostore každý šifrovací klíč v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5dda9-107">It also includes instructions that will show you how toostore each encryption key in Azure Key Vault.</span></span>

<span data-ttu-id="5dda9-108">Vždy šifrovaný je nová technologie šifrování dat v Azure SQL Database a SQL Server, který pomáhá chránit citlivá data v klidovém stavu na serveru hello během pohybu mezi klientem a serverem, a když hello dat se používá.</span><span class="sxs-lookup"><span data-stu-id="5dda9-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on hello server, during movement between client and server, and while hello data is in use.</span></span> <span data-ttu-id="5dda9-109">Vždy šifrovaný zajistí, že citlivá data nikdy zobrazí jako prostý text uvnitř hello databázový systém.</span><span class="sxs-lookup"><span data-stu-id="5dda9-109">Always Encrypted ensures that sensitive data never appears as plaintext inside hello database system.</span></span> <span data-ttu-id="5dda9-110">Po dokončení konfigurace šifrování dat, můžete přístup jenom klientské aplikace nebo aplikační servery, které mají přístup toohello klíče data ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="5dda9-110">After you configure data encryption, only client applications or app servers that have access toohello keys can access plaintext data.</span></span> <span data-ttu-id="5dda9-111">Podrobné informace najdete v tématu [vždycky šifrovaná (databázový stroj)](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-111">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="5dda9-112">Po dokončení konfigurace toouse databáze hello vždycky šifrovaná, vytvoříte klientskou aplikaci v jazyce C# pomocí sady Visual Studio toowork s hello zašifrovaná data.</span><span class="sxs-lookup"><span data-stu-id="5dda9-112">After you configure hello database toouse Always Encrypted, you will create a client application in C# with Visual Studio toowork with hello encrypted data.</span></span>

<span data-ttu-id="5dda9-113">Postupujte podle kroků hello v tomto článku a zjistěte, jak tooset si vždy šifrována pro Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="5dda9-113">Follow hello steps in this article and learn how tooset up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="5dda9-114">V tomto článku se dozvíte, jak tooperform hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="5dda9-114">In this article you will learn how tooperform hello following tasks:</span></span>

* <span data-ttu-id="5dda9-115">Průvodce vždy šifrována pomocí hello v aplikaci SSMS toocreate [vždy šifrována klíče](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="5dda9-115">Use hello Always Encrypted wizard in SSMS toocreate [Always Encrypted keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="5dda9-116">Vytvoření [k hlavnímu klíči sloupce (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-116">Create a [column master key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="5dda9-117">Vytvoření [šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-117">Create a [column encryption key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="5dda9-118">Vytvořit tabulku databáze a šifrování sloupců.</span><span class="sxs-lookup"><span data-stu-id="5dda9-118">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="5dda9-119">Vytvořte aplikaci, která vloží, vybere a zobrazí data z hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5dda9-119">Create an application that inserts, selects, and displays data from hello encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dda9-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5dda9-120">Prerequisites</span></span>
<span data-ttu-id="5dda9-121">V tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="5dda9-121">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="5dda9-122">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5dda9-122">An Azure account and subscription.</span></span> <span data-ttu-id="5dda9-123">Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5dda9-123">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5dda9-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verze 13.0.700.242 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5dda9-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="5dda9-125">[Rozhraní .NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) nebo novější (na klientském počítači hello).</span><span class="sxs-lookup"><span data-stu-id="5dda9-125">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on hello client computer).</span></span>
* <span data-ttu-id="5dda9-126">Sadu [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>
* <span data-ttu-id="5dda9-127">[Prostředí Azure PowerShell](/powershell/azure/overview), verze 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5dda9-127">[Azure PowerShell](/powershell/azure/overview), version  1.0 or later.</span></span> <span data-ttu-id="5dda9-128">Typ **(Get-Module azure - ListAvailable). Verze** toosee jakou verzi prostředí PowerShell, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="5dda9-128">Type **(Get-Module azure -ListAvailable).Version** toosee what version of PowerShell you are running.</span></span>

## <a name="enable-your-client-application-tooaccess-hello-sql-database-service"></a><span data-ttu-id="5dda9-129">Povolení služby SQL Database hello tooaccess klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="5dda9-129">Enable your client application tooaccess hello SQL Database service</span></span>
<span data-ttu-id="5dda9-130">Je nutné povolit služby klienta aplikace tooaccess hello SQL Database nastavením potřebného ověřování hello a přijetí těchto hello *ClientId* a *tajný klíč* , budete potřebovat tooauthenticate aplikace v hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="5dda9-130">You must enable your client application tooaccess hello SQL Database service by setting up hello required authentication and acquiring hello *ClientId* and *Secret* that you will need tooauthenticate your application in hello following code.</span></span>

1. <span data-ttu-id="5dda9-131">Otevřete hello [portál Azure classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5dda9-131">Open hello [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5dda9-132">Vyberte **služby Active Directory** a klikněte na tlačítko hello instanci Active Directory, která vaše aplikace bude používat.</span><span class="sxs-lookup"><span data-stu-id="5dda9-132">Select **Active Directory** and click hello Active Directory instance that your application will use.</span></span>
3. <span data-ttu-id="5dda9-133">Klikněte na tlačítko **aplikace**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-133">Click **Applications**, and then click **ADD**.</span></span>
4. <span data-ttu-id="5dda9-134">Zadejte název pro vaši aplikaci (například: *myClientApp*), vyberte **webové aplikace**a klikněte na šipku toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-134">Type a name for your application (for example: *myClientApp*), select **WEB APPLICATION**, and click hello arrow toocontinue.</span></span>
5. <span data-ttu-id="5dda9-135">Pro hello **adresa URL přihlašování** a **identifikátor ID URI aplikace** můžete zadat platnou adresu URL (například *http://myClientApp*) a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="5dda9-135">For hello **SIGN-ON URL** and **APP ID URI** you can type a valid URL (for example, *http://myClientApp*) and continue.</span></span>
6. <span data-ttu-id="5dda9-136">Klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-136">Click **CONFIGURE**.</span></span>
7. <span data-ttu-id="5dda9-137">Kopie vašeho **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-137">Copy your **CLIENT ID**.</span></span> <span data-ttu-id="5dda9-138">(Budete potřebovat tuto hodnotu v kódu později.)</span><span class="sxs-lookup"><span data-stu-id="5dda9-138">(You will need this value in your code later.)</span></span>
8. <span data-ttu-id="5dda9-139">V hello **klíče** vyberte **1 rok** z hello **vyberte dobu trvání** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="5dda9-139">In hello **keys** section, select **1 year** from hello  **Select duration** drop-down list.</span></span> <span data-ttu-id="5dda9-140">(Zkopírujete hello klíč po uložení v kroku 13.)</span><span class="sxs-lookup"><span data-stu-id="5dda9-140">(You will copy hello key after you save in step 13.)</span></span>
9. <span data-ttu-id="5dda9-141">Posuňte se dolů a klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-141">Scroll down and click **Add application**.</span></span>
10. <span data-ttu-id="5dda9-142">Nechte **zobrazit** nastavit příliš**Microsoft Apps** a vyberte **Microsoft Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-142">Leave **SHOW** set too**Microsoft Apps** and select **Microsoft Azure Service Management API**.</span></span> <span data-ttu-id="5dda9-143">Klikněte na tlačítko zaškrtnutí toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-143">Click hello checkmark toocontinue.</span></span>
11. <span data-ttu-id="5dda9-144">Vyberte **přístup k Azure Service Management...**  z hello **delegovaná oprávnění** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="5dda9-144">Select **Access Azure Service Management...** from hello **Delegated Permissions** drop-down list.</span></span>
12. <span data-ttu-id="5dda9-145">Klikněte na **ULOŽIT**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-145">Click **SAVE**.</span></span>
13. <span data-ttu-id="5dda9-146">Po hello uložit dokončení, zkopírujte hodnotu klíče hello hello **klíče** části.</span><span class="sxs-lookup"><span data-stu-id="5dda9-146">After hello save finishes, copy hello key value in hello **keys** section.</span></span> <span data-ttu-id="5dda9-147">(Budete potřebovat tuto hodnotu v kódu později.)</span><span class="sxs-lookup"><span data-stu-id="5dda9-147">(You will need this value in your code later.)</span></span>

## <a name="create-a-key-vault-toostore-your-keys"></a><span data-ttu-id="5dda9-148">Vytvoření trezoru klíčů toostore klíče</span><span class="sxs-lookup"><span data-stu-id="5dda9-148">Create a key vault toostore your keys</span></span>
<span data-ttu-id="5dda9-149">Teď, když je nastavené vaší klientské aplikace a máte vaše ID klienta, je čas toocreate trezoru klíčů a nakonfigurujte jeho zásady přístupu tak, aby vám a vaší aplikace mohou přistupovat tajné klíče trezoru hello (hello Always Encrypted klíče).</span><span class="sxs-lookup"><span data-stu-id="5dda9-149">Now that your client app is configured and you have your client ID, it's time toocreate a key vault and configure its access policy so you and your application can access hello vault's secrets (hello Always Encrypted keys).</span></span> <span data-ttu-id="5dda9-150">Hello *vytvořit*, *získat*, *seznamu*, *přihlašovací*, *ověřte*, *wrapKey*, a *unwrapKey* oprávnění jsou nutné pro vytvoření nové k hlavnímu klíči sloupce a nastavení šifrování s SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="5dda9-150">hello *create*, *get*, *list*, *sign*, *verify*, *wrapKey*, and *unwrapKey* permissions are required for creating a new column master key and for setting up encryption with SQL Server Management Studio.</span></span>

<span data-ttu-id="5dda9-151">Trezor klíčů můžete rychle vytvořit spuštěním hello následující skript.</span><span class="sxs-lookup"><span data-stu-id="5dda9-151">You can quickly create a key vault by running hello following script.</span></span> <span data-ttu-id="5dda9-152">Podrobné vysvětlení těchto rutin a další informace o vytváření a konfiguraci trezoru klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5dda9-152">For a detailed explanation of these cmdlets and more information about creating and configuring a key vault, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>

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




## <a name="create-a-blank-sql-database"></a><span data-ttu-id="5dda9-153">Vytvořit prázdnou databázi SQL</span><span class="sxs-lookup"><span data-stu-id="5dda9-153">Create a blank SQL database</span></span>
1. <span data-ttu-id="5dda9-154">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5dda9-154">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5dda9-155">Přejděte příliš**nový** > **Data + úložiště** > **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-155">Go too**New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="5dda9-156">Vytvoření **prázdné** databáze s názvem **Klinika** na nový nebo existující server.</span><span class="sxs-lookup"><span data-stu-id="5dda9-156">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="5dda9-157">Pro podrobné pokyny o tom, jak toocreate databáze v hello portál Azure, najdete v části [svoji první databázi Azure SQL](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5dda9-157">For detailed directions about how toocreate a database in hello Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Vytvoření prázdné databáze](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

<span data-ttu-id="5dda9-159">Bude nutné hello připojovací řetězec později v kurzu hello, takže po vytvoření databáze hello procházet toohello nový Klinika databáze a zkopírujte hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5dda9-159">You will need hello connection string later in hello tutorial, so after you create hello database, browse toohello new  Clinic database and copy hello connection string.</span></span> <span data-ttu-id="5dda9-160">Kdykoli můžete získat hello připojovací řetězec, ale je snadno toocopy v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5dda9-160">You can get hello connection string at any time, but it's easy toocopy it in hello Azure portal.</span></span>

1. <span data-ttu-id="5dda9-161">Přejděte příliš**databází SQL** > **Klinika** > **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-161">Go too**SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="5dda9-162">Zkopírujte hello připojovací řetězec pro **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-162">Copy hello connection string for **ADO.NET**.</span></span>
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a><span data-ttu-id="5dda9-164">Připojit databáze toohello pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="5dda9-164">Connect toohello database with SSMS</span></span>
<span data-ttu-id="5dda9-165">Otevřete aplikaci SSMS a připojte toohello serveru s databází Klinika hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-165">Open SSMS and connect toohello server with hello Clinic database.</span></span>

1. <span data-ttu-id="5dda9-166">Otevřete aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="5dda9-166">Open SSMS.</span></span> <span data-ttu-id="5dda9-167">(Přejděte příliš**připojit** > **databázový stroj** tooopen hello **připojit tooServer** okno, pokud není otevřený.)</span><span class="sxs-lookup"><span data-stu-id="5dda9-167">(Go too**Connect** > **Database Engine** tooopen hello **Connect tooServer** window if it isn't open.)</span></span>
2. <span data-ttu-id="5dda9-168">Zadejte název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5dda9-168">Enter your server name and credentials.</span></span> <span data-ttu-id="5dda9-169">název serveru Hello naleznete v okně databáze SQL hello a v připojovacím řetězci hello jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="5dda9-169">hello server name can be found on hello SQL database blade and in hello connection string you copied earlier.</span></span> <span data-ttu-id="5dda9-170">Typ hello úplný název serveru, včetně *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="5dda9-170">Type hello complete server name, including *database.windows.net*.</span></span>
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

<span data-ttu-id="5dda9-172">Pokud hello **nové pravidlo brány Firewall** okno otevře, přihlaste se tooAzure a umožňují SSMS za vás vytvořit nové pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="5dda9-172">If hello **New Firewall Rule** window opens, sign in tooAzure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="5dda9-173">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="5dda9-173">Create a table</span></span>
<span data-ttu-id="5dda9-174">V této části vytvoříte toohold pacienta dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="5dda9-174">In this section, you will create a table toohold patient data.</span></span> <span data-ttu-id="5dda9-175">Není původně zašifrována – budete konfigurovat šifrování v další části hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-175">It's not initially encrypted--you will configure encryption in hello next section.</span></span>

1. <span data-ttu-id="5dda9-176">Rozbalte položku **databáze**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-176">Expand **Databases**.</span></span>
2. <span data-ttu-id="5dda9-177">Klikněte pravým tlačítkem na hello **Klinika** databáze a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-177">Right-click hello **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="5dda9-178">Vložení hello následující Transact-SQL (T-SQL) do nové okno dotazu hello a **Execute** ho.</span><span class="sxs-lookup"><span data-stu-id="5dda9-178">Paste hello following Transact-SQL (T-SQL) into hello new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="5dda9-179">Šifrování sloupců (nakonfigurujte funkce Always Encrypted)</span><span class="sxs-lookup"><span data-stu-id="5dda9-179">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="5dda9-180">Aplikace SSMS poskytuje průvodce, který vám pomůže snadno nakonfigurovat tak, že k hlavnímu klíči sloupce hello, šifrovací klíč sloupce a šifrované sloupce můžete vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5dda9-180">SSMS provides a wizard that helps you easily configure Always Encrypted by setting up hello column master key, column encryption key, and encrypted columns for you.</span></span>

1. <span data-ttu-id="5dda9-181">Rozbalte položku **databáze** > **Klinika** > **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-181">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="5dda9-182">Klikněte pravým tlačítkem na hello **pacientů** tabulky a vyberte **šifrování sloupců** tooopen hello Always Encrypted průvodce:</span><span class="sxs-lookup"><span data-stu-id="5dda9-182">Right-click hello **Patients** table and select **Encrypt Columns** tooopen hello Always Encrypted wizard:</span></span>
   
    ![Šifrování sloupců](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

<span data-ttu-id="5dda9-184">Hello Always Encrypted Průvodce obsahuje následující části hello: **sloupců výběru**, **konfigurace hlavního klíče**, **ověření**, a **souhrn** .</span><span class="sxs-lookup"><span data-stu-id="5dda9-184">hello Always Encrypted wizard includes hello following sections: **Column Selection**, **Master Key Configuration**, **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="5dda9-185">Výběr sloupce</span><span class="sxs-lookup"><span data-stu-id="5dda9-185">Column Selection</span></span>
<span data-ttu-id="5dda9-186">Klikněte na tlačítko **Další** na hello **ÚVOD** stránku hello tooopen **sloupců výběru** stránky.</span><span class="sxs-lookup"><span data-stu-id="5dda9-186">Click **Next** on hello **Introduction** page tooopen hello **Column Selection** page.</span></span> <span data-ttu-id="5dda9-187">Na této stránce se vybrat sloupce, které chcete tooencrypt, [hello typ šifrování a jaké šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span><span class="sxs-lookup"><span data-stu-id="5dda9-187">On this page, you will select which columns you want tooencrypt, [hello type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span></span>

<span data-ttu-id="5dda9-188">Šifrování **SSN** a **datum narození** informace pro každý pacienta.</span><span class="sxs-lookup"><span data-stu-id="5dda9-188">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="5dda9-189">sloupec SSN Hello použije deterministickou šifrování, která podporuje rovnosti vyhledávání, spojení a seskupit podle.</span><span class="sxs-lookup"><span data-stu-id="5dda9-189">hello SSN column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="5dda9-190">sloupec Datum narození Hello použije náhodnou šifrování, která nepodporuje operace.</span><span class="sxs-lookup"><span data-stu-id="5dda9-190">hello BirthDate column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="5dda9-191">Sada hello **typ šifrování** pro sloupec hello SSN příliš**Deterministic** a hello datum narození sloupec příliš**Randomized**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-191">Set hello **Encryption Type** for hello SSN column too**Deterministic** and hello BirthDate column too**Randomized**.</span></span> <span data-ttu-id="5dda9-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-192">Click **Next**.</span></span>

![Šifrování sloupců](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="5dda9-194">Konfigurace hlavního klíče</span><span class="sxs-lookup"><span data-stu-id="5dda9-194">Master Key Configuration</span></span>
<span data-ttu-id="5dda9-195">Hello **hlavní klíč konfigurace** stránka je, kde můžete nastavit CMK a zprostředkovatele úložiště klíčů vyberte hello hello CMK uložení.</span><span class="sxs-lookup"><span data-stu-id="5dda9-195">hello **Master Key Configuration** page is where you set up your CMK and select hello key store provider where hello CMK will be stored.</span></span> <span data-ttu-id="5dda9-196">V současné době můžete uložit CMK v úložišti certifikátů Windows hello, Azure Key Vault nebo modul hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="5dda9-196">Currently, you can store a CMK in hello Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span>

<span data-ttu-id="5dda9-197">Tento kurz ukazuje, jak toostore klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5dda9-197">This tutorial shows how toostore your keys in Azure Key Vault.</span></span>

1. <span data-ttu-id="5dda9-198">Vyberte **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-198">Select **Azure Key Vault**.</span></span>
2. <span data-ttu-id="5dda9-199">Hello rozevíracím seznamu vyberte požadované trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-199">Select hello desired key vault from hello drop-down list.</span></span>
3. <span data-ttu-id="5dda9-200">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-200">Click **Next**.</span></span>

![Konfigurace hlavního klíče](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="5dda9-202">Ověření</span><span class="sxs-lookup"><span data-stu-id="5dda9-202">Validation</span></span>
<span data-ttu-id="5dda9-203">Můžete šifrovat hello sloupce nyní nebo později uložit toorun skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5dda9-203">You can encrypt hello columns now or save a PowerShell script toorun later.</span></span> <span data-ttu-id="5dda9-204">V tomto kurzu vyberte **teď pokračovat toofinish** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-204">For this tutorial, select **Proceed toofinish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="5dda9-205">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5dda9-205">Summary</span></span>
<span data-ttu-id="5dda9-206">Ověřte, zda jsou všechny správné hello nastavení a klikněte na tlačítko **Dokončit** toocomplete hello nastavení vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5dda9-206">Verify that hello settings are all correct and click **Finish** toocomplete hello setup for Always Encrypted.</span></span>

![Souhrn](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-hello-wizards-actions"></a><span data-ttu-id="5dda9-208">Ověření akce průvodce hello</span><span class="sxs-lookup"><span data-stu-id="5dda9-208">Verify hello wizard's actions</span></span>
<span data-ttu-id="5dda9-209">Po dokončení Průvodce hello vaše databáze je nastavena pro vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5dda9-209">After hello wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="5dda9-210">Hello hello na průvodce provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="5dda9-210">hello wizard performed hello following actions:</span></span>

* <span data-ttu-id="5dda9-211">K hlavnímu klíči sloupce vytvoří a uloží ji do Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5dda9-211">Created a column master key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="5dda9-212">Šifrovací klíč sloupce vytvoří a uloží ji do Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5dda9-212">Created a column encryption key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="5dda9-213">Nakonfigurované hello vybrané sloupce pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="5dda9-213">Configured hello selected columns for encryption.</span></span> <span data-ttu-id="5dda9-214">tabulky Pacienti Hello aktuálně neobsahuje žádná data, ale stávající data v hello vybrané sloupce je zašifrovaný.</span><span class="sxs-lookup"><span data-stu-id="5dda9-214">hello Patients table currently has no data, but any existing data in hello selected columns is now encrypted.</span></span>

<span data-ttu-id="5dda9-215">Můžete ověřit vytvoření hello hello klíčů v aplikaci SSMS rozšířením **Klinika** > **zabezpečení** > **vždy šifrované klíče**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-215">You can verify hello creation of hello keys in SSMS by expanding **Clinic** > **Security** > **Always Encrypted Keys**.</span></span>

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a><span data-ttu-id="5dda9-216">Vytvořit klientskou aplikaci, která funguje s hello šifrovaná data</span><span class="sxs-lookup"><span data-stu-id="5dda9-216">Create a client application that works with hello encrypted data</span></span>
<span data-ttu-id="5dda9-217">Teď, když je funkce Always Encrypted nastavili, můžete vytvořit aplikaci, která provádí *vloží* a *vybere* na hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5dda9-217">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on hello encrypted columns.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="5dda9-218">Vaše aplikace musí používat [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekty při předávání server toohello data ve formátu prostého textu se vždy šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5dda9-218">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data toohello server with Always Encrypted columns.</span></span> <span data-ttu-id="5dda9-219">Předávání hodnot literál bez použití SqlParameter objekty způsobí výjimku.</span><span class="sxs-lookup"><span data-stu-id="5dda9-219">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="5dda9-220">Otevřete Visual Studio a vytvoření nového jazyka C# **konzolové aplikace** (Visual Studio 2015 a starší) nebo **konzolovou aplikaci (rozhraní .NET Framework)** (Visual Studio 2017 a novější).</span><span class="sxs-lookup"><span data-stu-id="5dda9-220">Open Visual Studio and create a new C# **Console Application** (Visual Studio 2015 and earlier) or **Console App (.NET Framework)** (Visual Studio 2017 and later).</span></span> <span data-ttu-id="5dda9-221">Zajistěte, aby váš projekt je nastaven příliš**rozhraní .NET Framework 4.6** nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5dda9-221">Make sure your project is set too**.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="5dda9-222">Název projektu hello **AlwaysEncryptedConsoleAKVApp** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-222">Name hello project **AlwaysEncryptedConsoleAKVApp** and click **OK**.</span></span>
3. <span data-ttu-id="5dda9-223">Nainstalujte následující balíčky NuGet přechodem příliš hello**nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-223">Install hello following NuGet packages by going too**Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="5dda9-224">Spusťte tyto dva řádky kódu v hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="5dda9-224">Run these two lines of code in hello Package Manager Console.</span></span>

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-tooenable-always-encrypted"></a><span data-ttu-id="5dda9-225">Upravit vaše tooenable řetězec připojení vždycky šifrovaná</span><span class="sxs-lookup"><span data-stu-id="5dda9-225">Modify your connection string tooenable Always Encrypted</span></span>
<span data-ttu-id="5dda9-226">Tato část vysvětluje, jak tooenable vždy zašifrované v připojovací řetězec databáze.</span><span class="sxs-lookup"><span data-stu-id="5dda9-226">This section  explains how tooenable Always Encrypted in your database connection string.</span></span>

<span data-ttu-id="5dda9-227">tooenable vždycky šifrovaná, budete potřebovat tooadd hello **nastavení šifrování sloupec** – klíčové slovo tooyour připojení řetězce a nastavte ji příliš**povoleno**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-227">tooenable Always Encrypted, you need tooadd hello **Column Encryption Setting** keyword tooyour connection string and set it too**Enabled**.</span></span>

<span data-ttu-id="5dda9-228">To můžete nastavit přímo v hello připojovací řetězec, nebo můžete ho nastavit pomocí [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-228">You can set this directly in hello connection string, or you can set it by using [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="5dda9-229">Hello ukázkovou aplikaci v hello další část ukazuje, jak toouse **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-229">hello sample application in hello next section shows how toouse **SqlConnectionStringBuilder**.</span></span>

### <a name="enable-always-encrypted-in-hello-connection-string"></a><span data-ttu-id="5dda9-230">Povolení funkce Always Encrypted v připojovacím řetězci hello</span><span class="sxs-lookup"><span data-stu-id="5dda9-230">Enable Always Encrypted in hello connection string</span></span>
<span data-ttu-id="5dda9-231">Přidejte následující připojovací řetězec – klíčové slovo tooyour hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-231">Add hello following keyword tooyour connection string.</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a><span data-ttu-id="5dda9-232">Povolit vždy šifrován SqlConnectionStringBuilder</span><span class="sxs-lookup"><span data-stu-id="5dda9-232">Enable Always Encrypted with SqlConnectionStringBuilder</span></span>
<span data-ttu-id="5dda9-233">Následující kód ukazuje, jak Hello tooenable vždy šifrována nastavením [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) příliš[povoleno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-233">hello following code shows how tooenable Always Encrypted by setting [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) too[Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-hello-azure-key-vault-provider"></a><span data-ttu-id="5dda9-234">Registrace zprostředkovatele Azure Key Vault hello</span><span class="sxs-lookup"><span data-stu-id="5dda9-234">Register hello Azure Key Vault provider</span></span>
<span data-ttu-id="5dda9-235">Hello následující kód ukazuje, jak tooregister hello Azure Key Vault zprostředkovatele s ovladač technologie ADO.NET hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-235">hello following code shows how tooregister hello Azure Key Vault provider with hello ADO.NET driver.</span></span>

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



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="5dda9-236">Vždy šifrovaný ukázkovou aplikaci konzoly</span><span class="sxs-lookup"><span data-stu-id="5dda9-236">Always Encrypted sample console application</span></span>
<span data-ttu-id="5dda9-237">Tento příklad znázorňuje postup:</span><span class="sxs-lookup"><span data-stu-id="5dda9-237">This sample demonstrates how to:</span></span>

* <span data-ttu-id="5dda9-238">Upravte vaše tooenable řetězec připojení vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="5dda9-238">Modify your connection string tooenable Always Encrypted.</span></span>
* <span data-ttu-id="5dda9-239">Zaregistrujte Azure Key Vault, protože zprostředkovatel úložiště klíčů aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-239">Register Azure Key Vault as hello application's key store provider.</span></span>  
* <span data-ttu-id="5dda9-240">Vložení dat do hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5dda9-240">Insert data into hello encrypted columns.</span></span>
* <span data-ttu-id="5dda9-241">Vyberte záznam pomocí filtrování pro konkrétní hodnoty v šifrovaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="5dda9-241">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="5dda9-242">Nahraďte obsah hello **Program.cs** s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="5dda9-242">Replace hello contents of **Program.cs** with hello following code.</span></span> <span data-ttu-id="5dda9-243">Nahraďte hello připojovací řetězec pro proměnnou globální connectionString hello hello řádek, který přímo předchází metodu Main hello platný připojovacím řetězcem z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5dda9-243">Replace hello connection string for hello global connectionString variable in hello line that directly precedes hello Main method with your valid connection string from hello Azure portal.</span></span> <span data-ttu-id="5dda9-244">Toto je jedinou změnou hello potřebujete toomake toothis kódu.</span><span class="sxs-lookup"><span data-stu-id="5dda9-244">This is hello only change you need toomake toothis code.</span></span>

<span data-ttu-id="5dda9-245">Spusťte toosee aplikace hello vždycky šifrovaná v akci.</span><span class="sxs-lookup"><span data-stu-id="5dda9-245">Run hello app toosee Always Encrypted in action.</span></span>

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
        // Update this line with your Clinic database connection string from hello Azure portal.
        static string connectionString = @"<connection string from hello portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

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
            Console.WriteLine(Environment.NewLine + "All hello records currently in hello Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching hello encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that hello user entered 11 characters.
            // In production be sure toocheck all user input and use hello best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
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
                throw new InvalidOperationException("Failed tooobtain hello access token");
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



## <a name="verify-that-hello-data-is-encrypted"></a><span data-ttu-id="5dda9-246">Ověřte, zda text hello data se šifrují</span><span class="sxs-lookup"><span data-stu-id="5dda9-246">Verify that hello data is encrypted</span></span>
<span data-ttu-id="5dda9-247">Můžete rychle zjistit, že hello skutečná data na serveru hello je šifrovaná pomocí dotazu na data pacientů hello pomocí SSMS (pomocí aktuálního připojení kde **nastavení šifrování sloupec** zatím není povolená).</span><span class="sxs-lookup"><span data-stu-id="5dda9-247">You can quickly check that hello actual data on hello server is encrypted by querying hello Patients data with SSMS (using your current connection where **Column Encryption Setting** is not yet enabled).</span></span>

<span data-ttu-id="5dda9-248">Spusťte následující dotaz na databázi Klinika hello hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-248">Run hello following query on hello Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="5dda9-249">Uvidíte, že hello šifrované sloupce neobsahují žádná data ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="5dda9-249">You can see that hello encrypted columns do not contain any plaintext data.</span></span>

   ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

<span data-ttu-id="5dda9-251">toouse SSMS tooaccess hello data ve formátu prostého textu, můžete přidat hello *nastavení šifrování sloupec = povoleno* parametr toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="5dda9-251">toouse SSMS tooaccess hello plaintext data, you can add hello *Column Encryption Setting=enabled* parameter toohello connection.</span></span>

1. <span data-ttu-id="5dda9-252">V aplikaci SSMS, klikněte pravým tlačítkem na váš server v **Průzkumník objektů** a zvolte **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-252">In SSMS, right-click your server in **Object Explorer** and choose **Disconnect**.</span></span>
2. <span data-ttu-id="5dda9-253">Klikněte na tlačítko **připojit** > **databázový stroj** tooopen hello **připojit tooServer** a klikněte na **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-253">Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window and click **Options**.</span></span>
3. <span data-ttu-id="5dda9-254">Klikněte na tlačítko **další parametry připojení** a typ **nastavení šifrování sloupec = povoleno**.</span><span class="sxs-lookup"><span data-stu-id="5dda9-254">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. <span data-ttu-id="5dda9-256">Spusťte následující dotaz na databázi Klinika hello hello.</span><span class="sxs-lookup"><span data-stu-id="5dda9-256">Run hello following query on hello Clinic database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="5dda9-257">Nyní můžete vidět data ve formátu prostého textu hello v hello šifrované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5dda9-257">You can now see hello plaintext data in hello encrypted columns.</span></span>

    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a><span data-ttu-id="5dda9-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dda9-259">Next steps</span></span>
<span data-ttu-id="5dda9-260">Po vytvoření databáze, která používá vždycky šifrovaná, může být vhodné toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="5dda9-260">After you create a database that uses Always Encrypted, you may want toodo hello following:</span></span>

* <span data-ttu-id="5dda9-261">[Otočit a vyčištění klíče](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-261">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="5dda9-262">[Migraci dat, která už je šifrovaný pomocí funkce Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dda9-262">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>

## <a name="related-information"></a><span data-ttu-id="5dda9-263">Související informace</span><span class="sxs-lookup"><span data-stu-id="5dda9-263">Related information</span></span>
* [<span data-ttu-id="5dda9-264">Funkce Always Encrypted (vývoj pro klienta)</span><span class="sxs-lookup"><span data-stu-id="5dda9-264">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="5dda9-265">Transparentní šifrování dat</span><span class="sxs-lookup"><span data-stu-id="5dda9-265">Transparent data encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="5dda9-266">Šifrování SQL serveru</span><span class="sxs-lookup"><span data-stu-id="5dda9-266">SQL Server encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="5dda9-267">Vždy šifrovaný Průvodce</span><span class="sxs-lookup"><span data-stu-id="5dda9-267">Always Encrypted wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="5dda9-268">Vždy šifrované blog</span><span class="sxs-lookup"><span data-stu-id="5dda9-268">Always Encrypted blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

