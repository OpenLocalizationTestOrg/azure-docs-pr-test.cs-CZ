
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a><span data-ttu-id="c2b4f-101">Příklad programu C#</span><span class="sxs-lookup"><span data-stu-id="c2b4f-101">C# program example</span></span>

<span data-ttu-id="c2b4f-102">Hello další části tohoto článku k dispozici programu C#, který používá technologii ADO.NET toosend Transact-SQL příkazy toohello SQL database.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-102">hello next sections of this article present a C# program that uses ADO.NET toosend Transact-SQL statements toohello SQL database.</span></span> <span data-ttu-id="c2b4f-103">Hello C# program provádí hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="c2b4f-103">hello C# program performs hello following actions:</span></span>

1. <span data-ttu-id="c2b4f-104">[Připojí tooour SQL database pomocí ADO.NET](#cs_1_connect).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-104">[Connects tooour SQL database using ADO.NET](#cs_1_connect).</span></span>
2. <span data-ttu-id="c2b4f-105">[Vytvoří tabulky](#cs_2_createtables).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-105">[Creates tables](#cs_2_createtables).</span></span>
3. <span data-ttu-id="c2b4f-106">[Naplní hello tabulky s daty, vydáním příkazů T-SQL vložit](#cs_3_insert).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-106">[Populates hello tables with data, by issuing T-SQL INSERT statements](#cs_3_insert).</span></span>
4. <span data-ttu-id="c2b4f-107">[Aktualizace dat pomocí spojení](#cs_4_updatejoin).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-107">[Updates data by use of a join](#cs_4_updatejoin).</span></span>
5. <span data-ttu-id="c2b4f-108">[Odstraní data pomocí spojení](#cs_5_deletejoin).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-108">[Deletes data by use of a join](#cs_5_deletejoin).</span></span>
6. <span data-ttu-id="c2b4f-109">[Vybere řádky dat pomocí spojení](#cs_6_selectrows).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-109">[Selects data rows by use of a join](#cs_6_selectrows).</span></span>
7. <span data-ttu-id="c2b4f-110">Zavře připojení hello, (které zahodí všechny dočasné tabulky z databáze tempdb).</span><span class="sxs-lookup"><span data-stu-id="c2b4f-110">Closes hello connection (which drops any temporary tables from tempdb).</span></span>

<span data-ttu-id="c2b4f-111">obsahuje programu Hello C#:</span><span class="sxs-lookup"><span data-stu-id="c2b4f-111">hello C# program contains:</span></span>

- <span data-ttu-id="c2b4f-112">C# kódu tooconnect toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-112">C# code tooconnect toohello database.</span></span>
- <span data-ttu-id="c2b4f-113">Metody, které vracejí hello T-SQL zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-113">Methods that return hello T-SQL source code.</span></span>
- <span data-ttu-id="c2b4f-114">Dvě metody, které odesílají databáze toohello hello T-SQL.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-114">Two methods that submit hello T-SQL toohello database.</span></span>

#### <a name="toocompile-and-run"></a><span data-ttu-id="c2b4f-115">toocompile a spustit</span><span class="sxs-lookup"><span data-stu-id="c2b4f-115">toocompile and run</span></span>

<span data-ttu-id="c2b4f-116">Tento program C# je logicky jeden soubor cs.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-116">This C# program is logically one .cs file.</span></span> <span data-ttu-id="c2b4f-117">Ale zde programu hello fyzicky rozdělené do několika bloky kódu, toomake každý blokovat jednodušší toosee a pochopit.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-117">But here hello program is physically divided into several code blocks, toomake each block easier toosee and understand.</span></span> <span data-ttu-id="c2b4f-118">toocompile a spuštění tohoto programu, hello následující:</span><span class="sxs-lookup"><span data-stu-id="c2b4f-118">toocompile and run this program, do hello following:</span></span>

1. <span data-ttu-id="c2b4f-119">Vytvoření projektu C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-119">Create a C# project in Visual Studio.</span></span>
    - <span data-ttu-id="c2b4f-120">Typ projektu Hello musí být *konzoly* aplikace z něco podobného jako hello následující hierarchie: **šablony** > **Visual C#** > **Windows klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-120">hello project type should be a *console* application, from something like hello following hierarchy: **Templates** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
3. <span data-ttu-id="c2b4f-121">V souboru hello **Program.cs**, vymazat hello malé starter řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-121">In hello file **Program.cs**, erase hello small starter lines of code.</span></span>
3. <span data-ttu-id="c2b4f-122">Do souboru Program.cs hello kopírování a vkládání Dobrý den následující bloky, ve stejné pořadí, které jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-122">Into Program.cs, copy and paste each of hello following blocks, in hello same sequence they are presented here.</span></span>
4. <span data-ttu-id="c2b4f-123">V souboru Program.cs upravit hello následující hodnoty v hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="c2b4f-123">In Program.cs, edit hello following values in hello **Main** method:</span></span>

   - <span data-ttu-id="c2b4f-124">**označení CB. Zdroj dat**</span><span class="sxs-lookup"><span data-stu-id="c2b4f-124">**cb.DataSource**</span></span>
   - <span data-ttu-id="c2b4f-125">**CD. ID uživatele**</span><span class="sxs-lookup"><span data-stu-id="c2b4f-125">**cd.UserID**</span></span>
   - <span data-ttu-id="c2b4f-126">**označení CB. Heslo**</span><span class="sxs-lookup"><span data-stu-id="c2b4f-126">**cb.Password**</span></span>
   - <span data-ttu-id="c2b4f-127">**Vlastnost InitialCatalog**</span><span class="sxs-lookup"><span data-stu-id="c2b4f-127">**InitialCatalog**</span></span>

5. <span data-ttu-id="c2b4f-128">Ověřte, že sestavení hello **System.Data.dll** odkazuje.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-128">Verify that hello assembly **System.Data.dll** is referenced.</span></span> <span data-ttu-id="c2b4f-129">tooverify, rozbalte položku hello **odkazy** uzel v hello **Průzkumníku řešení** podokně.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-129">tooverify, expand hello **References** node in hello **Solution Explorer** pane.</span></span>
6. <span data-ttu-id="c2b4f-130">program hello toobuild v sadě Visual Studio, klikněte na tlačítko hello **sestavení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-130">toobuild hello program in Visual Studio, click hello **Build** menu.</span></span>
7. <span data-ttu-id="c2b4f-131">program hello toorun ze sady Visual Studio, klikněte na tlačítko hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-131">toorun hello program from Visual Studio, click hello **Start** button.</span></span> <span data-ttu-id="c2b4f-132">výstup Hello sestavy se zobrazí v okně cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-132">hello report output is displayed in a cmd.exe window.</span></span>

> [!NOTE]
> <span data-ttu-id="c2b4f-133">Máte možnost hello úpravy tooadd hello T-SQL úvodní  **#**  toohello názvy tabulek, které vytvoří je jako dočasné tabulky v **databáze tempdb**.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-133">You have hello option of editing hello T-SQL tooadd a leading **#** toohello table names, which creates them as temporary tables in **tempdb**.</span></span> <span data-ttu-id="c2b4f-134">To může být užitečné pro demonstrační účely, pokud je k dispozici žádná testovací databáze.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-134">This can be useful for demonstration purposes, when no test database is available.</span></span> <span data-ttu-id="c2b4f-135">Dočasné tabulky se automaticky odstraní po ukončení připojení hello.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-135">Temporary tables are deleted automatically when hello connection closes.</span></span> <span data-ttu-id="c2b4f-136">Pro dočasné tabulky nejsou vynucená všechny odkazy pro cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-136">Any REFERENCES for foreign keys are not enforced for temporary tables.</span></span>
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a><span data-ttu-id="c2b4f-137">C# block 1: připojení pomocí technologie ADO.NET</span><span class="sxs-lookup"><span data-stu-id="c2b4f-137">C# block 1: Connect by using ADO.NET</span></span>

- [<span data-ttu-id="c2b4f-138">Další</span><span class="sxs-lookup"><span data-stu-id="c2b4f-138">Next</span></span>](#cs_2_createtables)


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
### <a name="c-block-2-t-sql-toocreate-tables"></a><span data-ttu-id="c2b4f-139">C# block 2: T-SQL toocreate tabulky</span><span class="sxs-lookup"><span data-stu-id="c2b4f-139">C# block 2: T-SQL toocreate tables</span></span>

- <span data-ttu-id="c2b4f-140">[Předchozí](#cs_1_connect) &nbsp;  /  &nbsp; [další](#cs_3_insert)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-140">[Previous](#cs_1_connect) &nbsp; / &nbsp; [Next](#cs_3_insert)</span></span>

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

#### <a name="entity-relationship-diagram-erd"></a><span data-ttu-id="c2b4f-141">Diagram vztah entity (opravné)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-141">Entity Relationship Diagram (ERD)</span></span>

<span data-ttu-id="c2b4f-142">Hello předchozí příkazy CREATE TABLE zahrnují hello **odkazy** toocreate – klíčové slovo *cizí klíč* (Cizíklíč) relaci mezi dvěma tabulkami.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-142">hello preceding CREATE TABLE statements involve hello **REFERENCES** keyword toocreate a *foreign key* (FK) relationship between two tables.</span></span>  <span data-ttu-id="c2b4f-143">Pokud používáte databázi tempdb, komentář hello `--REFERENCES` – klíčové slovo pomocí pár úvodní pomlčky.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-143">If you are using tempdb, comment out hello `--REFERENCES` keyword using a pair of leading dashes.</span></span>

<span data-ttu-id="c2b4f-144">Dále je opravné, která zobrazuje hello relaci mezi dvěma tabulkami hello.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-144">Next is an ERD that displays hello relationship between hello two tables.</span></span> <span data-ttu-id="c2b4f-145">Hello hodnoty v hello #tabEmployee.DepartmentCode *podřízené* sloupce jsou omezené toohello hodnoty, které jsou součástí hello #tabDepartment.Department *nadřazené* sloupce.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-145">hello values in hello #tabEmployee.DepartmentCode *child* column are limited toohello values present in hello #tabDepartment.Department *parent* column.</span></span>

![Opravné zobrazující cizí klíč](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-tooinsert-data"></a><span data-ttu-id="c2b4f-147">C# block 3: data tooinsert T-SQL</span><span class="sxs-lookup"><span data-stu-id="c2b4f-147">C# block 3: T-SQL tooinsert data</span></span>

- <span data-ttu-id="c2b4f-148">[Předchozí](#cs_2_createtables) &nbsp;  /  &nbsp; [další](#cs_4_updatejoin)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-148">[Previous](#cs_2_createtables) &nbsp; / &nbsp; [Next](#cs_4_updatejoin)</span></span>


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
### <a name="c-block-4-t-sql-tooupdate-join"></a><span data-ttu-id="c2b4f-149">C# block 4: tooupdate připojení k T-SQL</span><span class="sxs-lookup"><span data-stu-id="c2b4f-149">C# block 4: T-SQL tooupdate-join</span></span>

- <span data-ttu-id="c2b4f-150">[Předchozí](#cs_3_insert) &nbsp;  /  &nbsp; [další](#cs_5_deletejoin)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-150">[Previous](#cs_3_insert) &nbsp; / &nbsp; [Next](#cs_5_deletejoin)</span></span>


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
### <a name="c-block-5-t-sql-toodelete-join"></a><span data-ttu-id="c2b4f-151">C# block 5: toodelete připojení k T-SQL</span><span class="sxs-lookup"><span data-stu-id="c2b4f-151">C# block 5: T-SQL toodelete-join</span></span>

- <span data-ttu-id="c2b4f-152">[Předchozí](#cs_4_updatejoin) &nbsp;  /  &nbsp; [další](#cs_6_selectrows)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-152">[Previous](#cs_4_updatejoin) &nbsp; / &nbsp; [Next](#cs_6_selectrows)</span></span>


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
### <a name="c-block-6-t-sql-tooselect-rows"></a><span data-ttu-id="c2b4f-153">C# block 6: T-SQL tooselect řádků</span><span class="sxs-lookup"><span data-stu-id="c2b4f-153">C# block 6: T-SQL tooselect rows</span></span>

- <span data-ttu-id="c2b4f-154">[Předchozí](#cs_5_deletejoin) &nbsp;  /  &nbsp; [další](#cs_6b_datareader)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-154">[Previous](#cs_5_deletejoin) &nbsp; / &nbsp; [Next](#cs_6b_datareader)</span></span>


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
### <a name="c-block-6b-executereader"></a><span data-ttu-id="c2b4f-155">C# block 6b: ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="c2b4f-155">C# block 6b: ExecuteReader</span></span>

- <span data-ttu-id="c2b4f-156">[Předchozí](#cs_6_selectrows) &nbsp;  /  &nbsp; [další](#cs_7_executenonquery)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-156">[Previous](#cs_6_selectrows) &nbsp; / &nbsp; [Next](#cs_7_executenonquery)</span></span>

<span data-ttu-id="c2b4f-157">Tato metoda je navrženou toorun hello T-SQL příkazem SELECT, který je sestavena hello **Build_6_Tsql_SelectEmployees** metoda.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-157">This method is designed toorun hello T-SQL SELECT statement that is built by hello **Build_6_Tsql_SelectEmployees** method.</span></span>


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
### <a name="c-block-7-executenonquery"></a><span data-ttu-id="c2b4f-158">C# block 7: ExecuteNonQuery</span><span class="sxs-lookup"><span data-stu-id="c2b4f-158">C# block 7: ExecuteNonQuery</span></span>

- <span data-ttu-id="c2b4f-159">[Předchozí](#cs_6b_datareader) &nbsp;  /  &nbsp; [další](#cs_8_output)</span><span class="sxs-lookup"><span data-stu-id="c2b4f-159">[Previous](#cs_6b_datareader) &nbsp; / &nbsp; [Next](#cs_8_output)</span></span>

<span data-ttu-id="c2b4f-160">Tato metoda je volána pro operace, které upravovat obsah data hello tabulek bez vrácení dat řádky.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-160">This method is called for operations that modify hello data content of tables without returning any data rows.</span></span>


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
### <a name="c-block-8-actual-test-output-toohello-console"></a><span data-ttu-id="c2b4f-161">C# bloku 8: skutečný testovací výstup toohello konzoly</span><span class="sxs-lookup"><span data-stu-id="c2b4f-161">C# block 8: Actual test output toohello console</span></span>

- [<span data-ttu-id="c2b4f-162">Předchozí</span><span class="sxs-lookup"><span data-stu-id="c2b4f-162">Previous</span></span>](#cs_7_executenonquery)

<span data-ttu-id="c2b4f-163">Tato část zaznamená hello výstupu, který hello program odeslané toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-163">This section captures hello output that hello program sent toohello console.</span></span> <span data-ttu-id="c2b4f-164">Pouze hodnoty guid hello liší mezi test spustí.</span><span class="sxs-lookup"><span data-stu-id="c2b4f-164">Only hello guid values vary between test runs.</span></span>


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
