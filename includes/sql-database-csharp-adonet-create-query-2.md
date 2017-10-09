
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a>Příklad programu C#

Hello další části tohoto článku k dispozici programu C#, který používá technologii ADO.NET toosend Transact-SQL příkazy toohello SQL database. Hello C# program provádí hello následující akce:

1. [Připojí tooour SQL database pomocí ADO.NET](#cs_1_connect).
2. [Vytvoří tabulky](#cs_2_createtables).
3. [Naplní hello tabulky s daty, vydáním příkazů T-SQL vložit](#cs_3_insert).
4. [Aktualizace dat pomocí spojení](#cs_4_updatejoin).
5. [Odstraní data pomocí spojení](#cs_5_deletejoin).
6. [Vybere řádky dat pomocí spojení](#cs_6_selectrows).
7. Zavře připojení hello, (které zahodí všechny dočasné tabulky z databáze tempdb).

obsahuje programu Hello C#:

- C# kódu tooconnect toohello databáze.
- Metody, které vracejí hello T-SQL zdrojového kódu.
- Dvě metody, které odesílají databáze toohello hello T-SQL.

#### <a name="toocompile-and-run"></a>toocompile a spustit

Tento program C# je logicky jeden soubor cs. Ale zde programu hello fyzicky rozdělené do několika bloky kódu, toomake každý blokovat jednodušší toosee a pochopit. toocompile a spuštění tohoto programu, hello následující:

1. Vytvoření projektu C# v sadě Visual Studio.
    - Typ projektu Hello musí být *konzoly* aplikace z něco podobného jako hello následující hierarchie: **šablony** > **Visual C#** > **Windows klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.
3. V souboru hello **Program.cs**, vymazat hello malé starter řádků kódu.
3. Do souboru Program.cs hello kopírování a vkládání Dobrý den následující bloky, ve stejné pořadí, které jsou zde uvedeny.
4. V souboru Program.cs upravit hello následující hodnoty v hello **hlavní** metoda:

   - **označení CB. Zdroj dat**
   - **CD. ID uživatele**
   - **označení CB. Heslo**
   - **Vlastnost InitialCatalog**

5. Ověřte, že sestavení hello **System.Data.dll** odkazuje. tooverify, rozbalte položku hello **odkazy** uzel v hello **Průzkumníku řešení** podokně.
6. program hello toobuild v sadě Visual Studio, klikněte na tlačítko hello **sestavení** nabídky.
7. program hello toorun ze sady Visual Studio, klikněte na tlačítko hello **spustit** tlačítko. výstup Hello sestavy se zobrazí v okně cmd.exe.

> [!NOTE]
> Máte možnost hello úpravy tooadd hello T-SQL úvodní  **#**  toohello názvy tabulek, které vytvoří je jako dočasné tabulky v **databáze tempdb**. To může být užitečné pro demonstrační účely, pokud je k dispozici žádná testovací databáze. Dočasné tabulky se automaticky odstraní po ukončení připojení hello. Pro dočasné tabulky nejsou vynucená všechny odkazy pro cizí klíče.
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a>C# block 1: připojení pomocí technologie ADO.NET

- [Další](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View hello report output here, then press any key tooend hello program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-toocreate-tables"></a>C# block 2: T-SQL toocreate tabulky

- [Předchozí](#cs_1_connect) &nbsp;  /  &nbsp; [další](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a>Diagram vztah entity (opravné)

Hello předchozí příkazy CREATE TABLE zahrnují hello **odkazy** toocreate – klíčové slovo *cizí klíč* (Cizíklíč) relaci mezi dvěma tabulkami.  Pokud používáte databázi tempdb, komentář hello `--REFERENCES` – klíčové slovo pomocí pár úvodní pomlčky.

Dále je opravné, která zobrazuje hello relaci mezi dvěma tabulkami hello. Hello hodnoty v hello #tabEmployee.DepartmentCode *podřízené* sloupce jsou omezené toohello hodnoty, které jsou součástí hello #tabDepartment.Department *nadřazené* sloupce.

![Opravné zobrazující cizí klíč](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-tooinsert-data"></a>C# block 3: data tooinsert T-SQL

- [Předchozí](#cs_2_createtables) &nbsp;  /  &nbsp; [další](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- hello company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- hello company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-tooupdate-join"></a>C# block 4: tooupdate připojení k T-SQL

- [Předchozí](#cs_3_insert) &nbsp;  /  &nbsp; [další](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-toodelete-join"></a>C# block 5: toodelete připojení k T-SQL

- [Předchozí](#cs_4_updatejoin) &nbsp;  /  &nbsp; [další](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size hello Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband hello Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-tooselect-rows"></a>C# block 6: T-SQL tooselect řádků

- [Předchozí](#cs_5_deletejoin) &nbsp;  /  &nbsp; [další](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all hello final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a>C# block 6b: ExecuteReader

- [Předchozí](#cs_6_selectrows) &nbsp;  /  &nbsp; [další](#cs_7_executenonquery)

Tato metoda je navrženou toorun hello T-SQL příkazem SELECT, který je sestavena hello **Build_6_Tsql_SelectEmployees** metoda.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a>C# block 7: ExecuteNonQuery

- [Předchozí](#cs_6b_datareader) &nbsp;  /  &nbsp; [další](#cs_8_output)

Tato metoda je volána pro operace, které upravovat obsah data hello tabulek bez vrácení dat řádky.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL too{0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-toohello-console"></a>C# bloku 8: skutečný testovací výstup toohello konzoly

- [Předchozí](#cs_7_executenonquery)

Tato část zaznamená hello výstupu, který hello program odeslané toohello konzoly. Pouze hodnoty guid hello liší mezi test spustí.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
