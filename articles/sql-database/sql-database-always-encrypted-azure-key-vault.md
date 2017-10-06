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
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Funkce Always Encrypted: Chrání citlivá data v databázi SQL a ukládat šifrovací klíče v Azure Key Vault

Tento článek ukazuje, jak toosecure citlivá data v SQL databázi se šifrování dat pomocí hello [vždy šifrované průvodce](https://msdn.microsoft.com/library/mt459280.aspx) v [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Zahrnuje také pokyny, které vám ukáže, jak toostore každý šifrovací klíč v Azure Key Vault.

Vždy šifrovaný je nová technologie šifrování dat v Azure SQL Database a SQL Server, který pomáhá chránit citlivá data v klidovém stavu na serveru hello během pohybu mezi klientem a serverem, a když hello dat se používá. Vždy šifrovaný zajistí, že citlivá data nikdy zobrazí jako prostý text uvnitř hello databázový systém. Po dokončení konfigurace šifrování dat, můžete přístup jenom klientské aplikace nebo aplikační servery, které mají přístup toohello klíče data ve formátu prostého textu. Podrobné informace najdete v tématu [vždycky šifrovaná (databázový stroj)](https://msdn.microsoft.com/library/mt163865.aspx).

Po dokončení konfigurace toouse databáze hello vždycky šifrovaná, vytvoříte klientskou aplikaci v jazyce C# pomocí sady Visual Studio toowork s hello zašifrovaná data.

Postupujte podle kroků hello v tomto článku a zjistěte, jak tooset si vždy šifrována pro Azure SQL database. V tomto článku se dozvíte, jak tooperform hello následující úkoly:

* Průvodce vždy šifrována pomocí hello v aplikaci SSMS toocreate [vždy šifrována klíče](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Vytvoření [k hlavnímu klíči sloupce (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Vytvoření [šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Vytvořit tabulku databáze a šifrování sloupců.
* Vytvořte aplikaci, která vloží, vybere a zobrazí data z hello šifrované sloupce.

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat:

* Účet a předplatné Azure. Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verze 13.0.700.242 nebo novější.
* [Rozhraní .NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) nebo novější (na klientském počítači hello).
* Sadu [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* [Prostředí Azure PowerShell](/powershell/azure/overview), verze 1.0 nebo novější. Typ **(Get-Module azure - ListAvailable). Verze** toosee jakou verzi prostředí PowerShell, kterou používáte.

## <a name="enable-your-client-application-tooaccess-hello-sql-database-service"></a>Povolení služby SQL Database hello tooaccess klientské aplikace
Je nutné povolit služby klienta aplikace tooaccess hello SQL Database nastavením potřebného ověřování hello a přijetí těchto hello *ClientId* a *tajný klíč* , budete potřebovat tooauthenticate aplikace v hello následující kód.

1. Otevřete hello [portál Azure classic](http://manage.windowsazure.com).
2. Vyberte **služby Active Directory** a klikněte na tlačítko hello instanci Active Directory, která vaše aplikace bude používat.
3. Klikněte na tlačítko **aplikace**a potom klikněte na **přidat**.
4. Zadejte název pro vaši aplikaci (například: *myClientApp*), vyberte **webové aplikace**a klikněte na šipku toocontinue hello.
5. Pro hello **adresa URL přihlašování** a **identifikátor ID URI aplikace** můžete zadat platnou adresu URL (například *http://myClientApp*) a pokračovat.
6. Klikněte na tlačítko **konfigurace**.
7. Kopie vašeho **ID klienta**. (Budete potřebovat tuto hodnotu v kódu později.)
8. V hello **klíče** vyberte **1 rok** z hello **vyberte dobu trvání** rozevíracího seznamu. (Zkopírujete hello klíč po uložení v kroku 13.)
9. Posuňte se dolů a klikněte na tlačítko **přidat aplikaci**.
10. Nechte **zobrazit** nastavit příliš**Microsoft Apps** a vyberte **Microsoft Azure Service Management API**. Klikněte na tlačítko zaškrtnutí toocontinue hello.
11. Vyberte **přístup k Azure Service Management...**  z hello **delegovaná oprávnění** rozevíracího seznamu.
12. Klikněte na **ULOŽIT**.
13. Po hello uložit dokončení, zkopírujte hodnotu klíče hello hello **klíče** části. (Budete potřebovat tuto hodnotu v kódu později.)

## <a name="create-a-key-vault-toostore-your-keys"></a>Vytvoření trezoru klíčů toostore klíče
Teď, když je nastavené vaší klientské aplikace a máte vaše ID klienta, je čas toocreate trezoru klíčů a nakonfigurujte jeho zásady přístupu tak, aby vám a vaší aplikace mohou přistupovat tajné klíče trezoru hello (hello Always Encrypted klíče). Hello *vytvořit*, *získat*, *seznamu*, *přihlašovací*, *ověřte*, *wrapKey*, a *unwrapKey* oprávnění jsou nutné pro vytvoření nové k hlavnímu klíči sloupce a nastavení šifrování s SQL Server Management Studio.

Trezor klíčů můžete rychle vytvořit spuštěním hello následující skript. Podrobné vysvětlení těchto rutin a další informace o vytváření a konfiguraci trezoru klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).

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




## <a name="create-a-blank-sql-database"></a>Vytvořit prázdnou databázi SQL
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Přejděte příliš**nový** > **Data + úložiště** > **SQL Database**.
3. Vytvoření **prázdné** databáze s názvem **Klinika** na nový nebo existující server. Pro podrobné pokyny o tom, jak toocreate databáze v hello portál Azure, najdete v části [svoji první databázi Azure SQL](sql-database-get-started-portal.md).
   
    ![Vytvoření prázdné databáze](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Bude nutné hello připojovací řetězec později v kurzu hello, takže po vytvoření databáze hello procházet toohello nový Klinika databáze a zkopírujte hello připojovací řetězec. Kdykoli můžete získat hello připojovací řetězec, ale je snadno toocopy v hello portálu Azure.

1. Přejděte příliš**databází SQL** > **Klinika** > **zobrazit databázové připojovací řetězce**.
2. Zkopírujte hello připojovací řetězec pro **ADO.NET**.
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a>Připojit databáze toohello pomocí SSMS
Otevřete aplikaci SSMS a připojte toohello serveru s databází Klinika hello.

1. Otevřete aplikaci SSMS. (Přejděte příliš**připojit** > **databázový stroj** tooopen hello **připojit tooServer** okno, pokud není otevřený.)
2. Zadejte název serveru a přihlašovací údaje. název serveru Hello naleznete v okně databáze SQL hello a v připojovacím řetězci hello jste zkopírovali dříve. Typ hello úplný název serveru, včetně *database.windows.net*.
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Pokud hello **nové pravidlo brány Firewall** okno otevře, přihlaste se tooAzure a umožňují SSMS za vás vytvořit nové pravidlo brány firewall.

## <a name="create-a-table"></a>Vytvoření tabulky
V této části vytvoříte toohold pacienta dat tabulky. Není původně zašifrována – budete konfigurovat šifrování v další části hello.

1. Rozbalte položku **databáze**.
2. Klikněte pravým tlačítkem na hello **Klinika** databáze a klikněte na tlačítko **nový dotaz**.
3. Vložení hello následující Transact-SQL (T-SQL) do nové okno dotazu hello a **Execute** ho.

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


## <a name="encrypt-columns-configure-always-encrypted"></a>Šifrování sloupců (nakonfigurujte funkce Always Encrypted)
Aplikace SSMS poskytuje průvodce, který vám pomůže snadno nakonfigurovat tak, že k hlavnímu klíči sloupce hello, šifrovací klíč sloupce a šifrované sloupce můžete vždy šifrována.

1. Rozbalte položku **databáze** > **Klinika** > **tabulky**.
2. Klikněte pravým tlačítkem na hello **pacientů** tabulky a vyberte **šifrování sloupců** tooopen hello Always Encrypted průvodce:
   
    ![Šifrování sloupců](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Hello Always Encrypted Průvodce obsahuje následující části hello: **sloupců výběru**, **konfigurace hlavního klíče**, **ověření**, a **souhrn** .

### <a name="column-selection"></a>Výběr sloupce
Klikněte na tlačítko **Další** na hello **ÚVOD** stránku hello tooopen **sloupců výběru** stránky. Na této stránce se vybrat sloupce, které chcete tooencrypt, [hello typ šifrování a jaké šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.

Šifrování **SSN** a **datum narození** informace pro každý pacienta. sloupec SSN Hello použije deterministickou šifrování, která podporuje rovnosti vyhledávání, spojení a seskupit podle. sloupec Datum narození Hello použije náhodnou šifrování, která nepodporuje operace.

Sada hello **typ šifrování** pro sloupec hello SSN příliš**Deterministic** a hello datum narození sloupec příliš**Randomized**. Klikněte na **Další**.

![Šifrování sloupců](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Konfigurace hlavního klíče
Hello **hlavní klíč konfigurace** stránka je, kde můžete nastavit CMK a zprostředkovatele úložiště klíčů vyberte hello hello CMK uložení. V současné době můžete uložit CMK v úložišti certifikátů Windows hello, Azure Key Vault nebo modul hardwarového zabezpečení (HSM).

Tento kurz ukazuje, jak toostore klíče v Azure Key Vault.

1. Vyberte **Azure Key Vault**.
2. Hello rozevíracím seznamu vyberte požadované trezoru klíčů hello.
3. Klikněte na **Další**.

![Konfigurace hlavního klíče](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Ověření
Můžete šifrovat hello sloupce nyní nebo později uložit toorun skript prostředí PowerShell. V tomto kurzu vyberte **teď pokračovat toofinish** a klikněte na tlačítko **Další**.

### <a name="summary"></a>Souhrn
Ověřte, zda jsou všechny správné hello nastavení a klikněte na tlačítko **Dokončit** toocomplete hello nastavení vždy šifrována.

![Souhrn](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-hello-wizards-actions"></a>Ověření akce průvodce hello
Po dokončení Průvodce hello vaše databáze je nastavena pro vždy šifrována. Hello hello na průvodce provést následující akce:

* K hlavnímu klíči sloupce vytvoří a uloží ji do Azure Key Vault.
* Šifrovací klíč sloupce vytvoří a uloží ji do Azure Key Vault.
* Nakonfigurované hello vybrané sloupce pro šifrování. tabulky Pacienti Hello aktuálně neobsahuje žádná data, ale stávající data v hello vybrané sloupce je zašifrovaný.

Můžete ověřit vytvoření hello hello klíčů v aplikaci SSMS rozšířením **Klinika** > **zabezpečení** > **vždy šifrované klíče**.

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a>Vytvořit klientskou aplikaci, která funguje s hello šifrovaná data
Teď, když je funkce Always Encrypted nastavili, můžete vytvořit aplikaci, která provádí *vloží* a *vybere* na hello šifrované sloupce.  

> [!IMPORTANT]
> Vaše aplikace musí používat [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekty při předávání server toohello data ve formátu prostého textu se vždy šifrované sloupce. Předávání hodnot literál bez použití SqlParameter objekty způsobí výjimku.
> 
> 

1. Otevřete Visual Studio a vytvoření nového jazyka C# **konzolové aplikace** (Visual Studio 2015 a starší) nebo **konzolovou aplikaci (rozhraní .NET Framework)** (Visual Studio 2017 a novější). Zajistěte, aby váš projekt je nastaven příliš**rozhraní .NET Framework 4.6** nebo novější.
2. Název projektu hello **AlwaysEncryptedConsoleAKVApp** a klikněte na tlačítko **OK**.
3. Nainstalujte následující balíčky NuGet přechodem příliš hello**nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.

Spusťte tyto dva řádky kódu v hello Konzola správce balíčků.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-tooenable-always-encrypted"></a>Upravit vaše tooenable řetězec připojení vždycky šifrovaná
Tato část vysvětluje, jak tooenable vždy zašifrované v připojovací řetězec databáze.

tooenable vždycky šifrovaná, budete potřebovat tooadd hello **nastavení šifrování sloupec** – klíčové slovo tooyour připojení řetězce a nastavte ji příliš**povoleno**.

To můžete nastavit přímo v hello připojovací řetězec, nebo můžete ho nastavit pomocí [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Hello ukázkovou aplikaci v hello další část ukazuje, jak toouse **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-hello-connection-string"></a>Povolení funkce Always Encrypted v připojovacím řetězci hello
Přidejte následující připojovací řetězec – klíčové slovo tooyour hello.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Povolit vždy šifrován SqlConnectionStringBuilder
Následující kód ukazuje, jak Hello tooenable vždy šifrována nastavením [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) příliš[povoleno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-hello-azure-key-vault-provider"></a>Registrace zprostředkovatele Azure Key Vault hello
Hello následující kód ukazuje, jak tooregister hello Azure Key Vault zprostředkovatele s ovladač technologie ADO.NET hello.

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



## <a name="always-encrypted-sample-console-application"></a>Vždy šifrovaný ukázkovou aplikaci konzoly
Tento příklad znázorňuje postup:

* Upravte vaše tooenable řetězec připojení vždy šifrována.
* Zaregistrujte Azure Key Vault, protože zprostředkovatel úložiště klíčů aplikace hello.  
* Vložení dat do hello šifrované sloupce.
* Vyberte záznam pomocí filtrování pro konkrétní hodnoty v šifrovaný sloupec.

Nahraďte obsah hello **Program.cs** s hello následující kód. Nahraďte hello připojovací řetězec pro proměnnou globální connectionString hello hello řádek, který přímo předchází metodu Main hello platný připojovacím řetězcem z hello portálu Azure. Toto je jedinou změnou hello potřebujete toomake toothis kódu.

Spusťte toosee aplikace hello vždycky šifrovaná v akci.

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



## <a name="verify-that-hello-data-is-encrypted"></a>Ověřte, zda text hello data se šifrují
Můžete rychle zjistit, že hello skutečná data na serveru hello je šifrovaná pomocí dotazu na data pacientů hello pomocí SSMS (pomocí aktuálního připojení kde **nastavení šifrování sloupec** zatím není povolená).

Spusťte následující dotaz na databázi Klinika hello hello.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Uvidíte, že hello šifrované sloupce neobsahují žádná data ve formátu prostého textu.

   ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

toouse SSMS tooaccess hello data ve formátu prostého textu, můžete přidat hello *nastavení šifrování sloupec = povoleno* parametr toohello připojení.

1. V aplikaci SSMS, klikněte pravým tlačítkem na váš server v **Průzkumník objektů** a zvolte **odpojení**.
2. Klikněte na tlačítko **připojit** > **databázový stroj** tooopen hello **připojit tooServer** a klikněte na **možnosti**.
3. Klikněte na tlačítko **další parametry připojení** a typ **nastavení šifrování sloupec = povoleno**.
   
    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. Spusťte následující dotaz na databázi Klinika hello hello.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Nyní můžete vidět data ve formátu prostého textu hello v hello šifrované sloupce.

    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Další kroky
Po vytvoření databáze, která používá vždycky šifrovaná, může být vhodné toodo hello následující:

* [Otočit a vyčištění klíče](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migraci dat, která už je šifrovaný pomocí funkce Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>Související informace
* [Funkce Always Encrypted (vývoj pro klienta)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparentní šifrování dat](https://msdn.microsoft.com/library/bb934049.aspx)
* [Šifrování SQL serveru](https://msdn.microsoft.com/library/bb510663.aspx)
* [Vždy šifrovaný Průvodce](https://msdn.microsoft.com/library/mt459280.aspx)
* [Vždy šifrované blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

