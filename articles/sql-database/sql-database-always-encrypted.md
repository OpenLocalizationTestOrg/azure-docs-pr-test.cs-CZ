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
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-hello-windows-certificate-store"></a>Funkce Always Encrypted: Chrání citlivá data v databázi SQL a ukládat šifrovací klíče v úložišti certifikátů Windows hello

Tento článek ukazuje, jak toosecure citlivá data v SQL databázi s šifrování databáze pomocí hello [vždy šifrované průvodce](https://msdn.microsoft.com/library/mt459280.aspx) v [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Také vám ukazuje jak toostore šifrovacích klíčů v certifikátu Windows hello uložit.

Vždy šifrovaný je nová technologie šifrování dat v Azure SQL Database a SQL Server, který pomáhá chránit citlivá data v klidovém stavu na serveru hello během pohybu mezi klientem a serverem a při hello dat se používá, nikdy zajistit, že citlivá data se zobrazí jako ve formátu prostého textu uvnitř hello databázový systém. Po šifrování dat, můžete přistupovat pouze klientské aplikace nebo aplikační servery, které mají přístup toohello klíče data ve formátu prostého textu. Podrobné informace najdete v tématu [vždycky šifrovaná (databázový stroj)](https://msdn.microsoft.com/library/mt163865.aspx).

Po dokončení konfigurace toouse databáze hello vždycky šifrovaná, vytvoříte klientskou aplikaci v jazyce C# pomocí sady Visual Studio toowork s hello zašifrovaná data.

Postupujte podle kroků hello v této toolearn článku jak tooset si vždy šifrována pro Azure SQL database. V tomto článku se dozvíte, jak tooperform hello následující úkoly:

* Průvodce vždy šifrována pomocí hello v aplikaci SSMS toocreate [vždy šifrované klíče](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Vytvoření [sloupce hlavního klíče (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Vytvoření [šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Vytvořit tabulku databáze a šifrování sloupců.
* Vytvořte aplikaci, která vloží, vybere a zobrazí data z hello šifrované sloupce.

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat:

* Účet a předplatné Azure. Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verze 13.0.700.242 nebo novější.
* [Rozhraní .NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) nebo novější (na klientském počítači hello).
* Sadu [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="create-a-blank-sql-database"></a>Vytvořit prázdnou databázi SQL
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nové** > **Data + úložiště** > **databáze SQL**.
3. Vytvoření **prázdné** databáze s názvem **Klinika** na nový nebo existující server. Podrobné pokyny k vytvoření databáze v hello portálu Azure, najdete v části [svoji první databázi Azure SQL](sql-database-get-started-portal.md).
   
    ![Vytvoření prázdné databáze](./media/sql-database-always-encrypted/create-database.png)

Hello připojovací řetězec budete potřebovat později v kurzu hello. Po vytvoření databáze hello přejděte toohello nový Klinika databáze a zkopírujte hello připojovací řetězec. Kdykoli můžete získat hello připojovací řetězec, ale je snadno toocopy ho, když jste v hello portálu Azure.

1. Klikněte na tlačítko **databází SQL** > **Klinika** > **zobrazit databázové připojovací řetězce**.
2. Zkopírujte hello připojovací řetězec pro **ADO.NET**.
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a>Připojit databáze toohello pomocí SSMS
Otevřete aplikaci SSMS a připojte toohello serveru s databází Klinika hello.

1. Otevřete aplikaci SSMS. (Klikněte na tlačítko **připojit** > **databázový stroj** tooopen hello **připojit tooServer** okno, pokud není otevřený).
2. Zadejte název serveru a přihlašovací údaje. název serveru Hello naleznete v okně databáze SQL hello a v připojovacím řetězci hello jste zkopírovali dříve. Typ hello kompletní server název včetně *database.windows.net*.
   
    ![Zkopírujte připojovací řetězec hello](./media/sql-database-always-encrypted/ssms-connect.png)

Pokud hello **nové pravidlo brány Firewall** okno otevře, přihlaste se tooAzure a umožňují SSMS za vás vytvořit nové pravidlo brány firewall.

## <a name="create-a-table"></a>Vytvoření tabulky
V této části vytvoříte toohold pacienta dat tabulky. To bude normální tabulku původně – budete konfigurovat šifrování v další části hello.

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
Poskytuje průvodce, aplikace SSMS tooeasily nakonfigurovat tak, že hello CMK CEK a šifrované sloupce můžete vždy šifrována.

1. Rozbalte položku **databáze** > **Klinika** > **tabulky**.
2. Klikněte pravým tlačítkem na hello **pacientů** tabulky a vyberte **šifrování sloupců** tooopen hello Always Encrypted průvodce:
   
    ![Šifrování sloupců](./media/sql-database-always-encrypted/encrypt-columns.png)

Hello Always Encrypted Průvodce obsahuje následující části hello: **sloupců výběru**, **konfigurace hlavního klíče** (CMK), **ověření**, a  **Souhrn**.

### <a name="column-selection"></a>Výběr sloupce
Klikněte na tlačítko **Další** na hello **ÚVOD** stránku hello tooopen **sloupců výběru** stránky. Na této stránce se vybrat sloupce, které chcete tooencrypt, [hello typ šifrování a jaké šifrovací klíč sloupce (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.

Šifrování **SSN** a **datum narození** informace pro každý pacienta. Hello **SSN** používat deterministickou šifrování, která podporuje rovnosti vyhledávání, spojení a seskupit podle sloupce. Hello **datum narození** sloupec použije náhodnou šifrování, která nepodporuje operace.

Sada hello **typ šifrování** pro hello **SSN** sloupec příliš**Deterministic** a hello **datum narození** sloupec příliš **Náhodně posunut**. Klikněte na **Další**.

![Šifrování sloupců](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Konfigurace hlavního klíče
Hello **hlavní klíč konfigurace** stránka je, kde můžete nastavit CMK a zprostředkovatele úložiště klíčů vyberte hello hello CMK uložení. V současné době můžete uložit CMK v úložišti certifikátů Windows hello, Azure Key Vault nebo modul hardwarového zabezpečení (HSM). Tento kurz ukazuje, jak toostore klíče v certifikátu Windows hello uložit.

Ověřte, že **úložiště certifikátů Windows** je vybrána a klikněte na tlačítko **Další**.

![Konfigurace hlavního klíče](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Ověření
Můžete šifrovat hello sloupce nyní nebo později uložit toorun skript prostředí PowerShell. V tomto kurzu vyberte **teď pokračovat toofinish** a klikněte na tlačítko **Další**.

### <a name="summary"></a>Souhrn
Ověřte, zda jsou všechny správné hello nastavení a klikněte na tlačítko **Dokončit** toocomplete hello nastavení vždy šifrována.

![Souhrn](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-hello-wizards-actions"></a>Ověření akce průvodce hello
Po dokončení Průvodce hello vaše databáze je nastavena pro vždy šifrována. Hello hello na průvodce provést následující akce:

* Vytvořit CMK.
* Vytvořit CEK.
* Nakonfigurované hello vybrané sloupce pro šifrování. Vaše **pacientů** tabulka nyní neobsahuje žádná data, ale stávající data v hello vybrané sloupce je zašifrovaný.

Vytvoření hello hello klíčů v aplikaci SSMS můžete ověřit tak, že přejdete příliš**Klinika** > **zabezpečení** > **vždy šifrované klíče**. Nyní můžete vidět hello nových klíčů, které hello Průvodce pro vás vygeneroval.

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a>Vytvořit klientskou aplikaci, která funguje s hello šifrovaná data
Teď, když je funkce Always Encrypted nastavili, můžete vytvořit aplikaci, která provádí *vloží* a *vybere* na hello šifrované sloupce. toosuccessfully hello ukázkovou aplikaci spustit, je nutné jej spustit na hello stejný počítač, kde jste spustili Průvodce vždycky šifrovaná hello. aplikace hello toorun na jiném počítači, je nutné nasadit Always Encrypted certifikáty toohello počítače se spuštěným systémem hello klientskou aplikaci.  

> [!IMPORTANT]
> Vaše aplikace musí používat [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekty při předávání server toohello data ve formátu prostého textu se vždy šifrované sloupce. Předávání hodnot literál bez použití SqlParameter objekty způsobí výjimku.
> 
> 

1. Otevřete Visual Studio a vytvořte novou aplikaci konzoly C#. Zajistěte, aby váš projekt je nastaven příliš**rozhraní .NET Framework 4.6** nebo novější.
2. Název projektu hello **AlwaysEncryptedConsoleApp** a klikněte na tlačítko **OK**.

![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-tooenable-always-encrypted"></a>Upravit vaše tooenable řetězec připojení vždycky šifrovaná
Tato část vysvětluje, jak tooenable vždy zašifrované v připojovací řetězec databáze. Upraví hello konzolovou aplikaci, kterou jste právě vytvořili, v další části hello "Always Encrypted ukázkovou aplikaci konzoly."

tooenable vždycky šifrovaná, budete potřebovat tooadd hello **nastavení šifrování sloupec** – klíčové slovo tooyour připojení řetězce a nastavte ji příliš**povoleno**.

To můžete nastavit přímo v hello připojovací řetězec, nebo můžete ho nastavit pomocí [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Hello ukázkovou aplikaci v hello další část ukazuje, jak toouse **SqlConnectionStringBuilder**.

> [!NOTE]
> Toto je vyžadována v konkrétní tooAlways klienta aplikace šifrovaný pouze změna hello. Pokud máte existující aplikace, která ukládá externě svém připojovacím řetězci (který je v konfiguračním souboru), je možné tooenable vždy šifrována aniž byste měnili kód.
> 
> 

### <a name="enable-always-encrypted-in-hello-connection-string"></a>Povolení funkce Always Encrypted v připojovacím řetězci hello
Přidejte následující připojovací řetězec – klíčové slovo tooyour hello:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Povolit vždy šifrován SqlConnectionStringBuilder
Hello následující kód ukazuje, jak tooenable vždy šifrována nastavením hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) příliš[povoleno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Vždy šifrovaný ukázkovou aplikaci konzoly
Tento příklad znázorňuje postup:

* Upravte vaše tooenable řetězec připojení vždy šifrována.
* Vložení dat do hello šifrované sloupce.
* Vyberte záznam pomocí filtrování pro konkrétní hodnoty v šifrovaný sloupec.

Nahraďte obsah hello **Program.cs** s hello následující kód. Nahraďte hello připojovací řetězec pro proměnnou globální connectionString hello v řádku hello přímo nad hello metodu Main platný připojovací řetězec z hello portálu Azure. Toto je jedinou změnou hello potřebujete toomake toothis kódu.

Spusťte toosee aplikace hello vždycky šifrovaná v akci.

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


## <a name="verify-that-hello-data-is-encrypted"></a>Ověřte, zda text hello data se šifrují
Můžete rychle zjistit, že hello skutečná data na serveru hello je šifrovaná pomocí dotazu hello **pacientů** dat pomocí aplikace SSMS. (Použít aktuální připojení kde nastavení šifrování pro sloupce hello zatím není povolená.)

Spusťte následující dotaz na databázi Klinika hello hello.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Uvidíte, že hello šifrované sloupce neobsahují žádná data ve formátu prostého textu.

   ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/ssms-encrypted.png)

toouse SSMS tooaccess hello data ve formátu prostého textu, můžete přidat hello **nastavení šifrování sloupec = povoleno** parametr toohello připojení.

1. V aplikaci SSMS, klikněte pravým tlačítkem na váš server v **Průzkumník objektů**a potom klikněte na **odpojení**.
2. Klikněte na tlačítko **připojit** > **databázový stroj** tooopen hello **připojit tooServer** okna a pak klikněte na tlačítko **možnosti**.
3. Klikněte na tlačítko **další parametry připojení** a typ **nastavení šifrování sloupec = povoleno**.
   
    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. Spuštění hello následující dotaz na hello **Klinika** databáze.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Nyní můžete vidět data ve formátu prostého textu hello v hello šifrované sloupce.

    ![Novou konzolovou aplikaci](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> Pokud se připojujete pomocí aplikace SSMS (nebo libovolného klienta) z jiného počítače, nebudete mít přístup toohello šifrovacích klíčů a nebudou moct toodecrypt hello data.
> 
> 

## <a name="next-steps"></a>Další kroky
Po vytvoření databáze, která používá vždycky šifrovaná, může být vhodné toodo hello následující:

* Tuto ukázku spusťte z jiného počítače. Nebude mít přístup toohello šifrovací klíče, takže ho nebude mít přístup k datům ve formátu prostého textu toohello a nespustí úspěšně.
* [Otočit a vyčištění klíče](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migraci dat, která už je šifrovaný pomocí funkce Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).
* [Nasazení funkce Always Encrypted certifikáty tooother klientské počítače](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (viz část "Provedení certifikáty k dispozici tooApplications a uživatelé" hello).

## <a name="related-information"></a>Související informace
* [Funkce Always Encrypted (vývoj pro klienta)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparentní šifrování dat](https://msdn.microsoft.com/library/bb934049.aspx)
* [Šifrování SQL serveru](https://msdn.microsoft.com/library/bb510663.aspx)
* [Vždy šifrované Průvodce](https://msdn.microsoft.com/library/mt459280.aspx)
* [Vždy šifrované Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

