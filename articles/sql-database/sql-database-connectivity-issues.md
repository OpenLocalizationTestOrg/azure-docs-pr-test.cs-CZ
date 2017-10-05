---
title: "Opravte chyby připojení SQL, přechodné chybě. | Microsoft Docs"
description: "Informace o odstraňování potíží, diagnostikovat a zabránit chyba připojení SQL nebo přechodné chybě ve službě Azure SQL Database. "
keywords: "připojení SQL, připojovací řetězec, problémy s připojením, přechodná chyba, došlo k chybě připojení"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: ae081fc0432e36bf9f4d4f06f289386ddce37990
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a><span data-ttu-id="4f1e6-104">Oprava a diagnostika chyb připojení SQL a přechodných chyb služby SQL Database a jejich předcházení</span><span class="sxs-lookup"><span data-stu-id="4f1e6-104">Troubleshoot, diagnose, and prevent SQL connection errors and transient errors for SQL Database</span></span>
<span data-ttu-id="4f1e6-105">Tento článek popisuje, jak zabránit, odstraňování, diagnostikovat a opravit chyby připojení a přechodné chyby, které klientské aplikace, zaznamená při komunikuje se službou Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-105">This article describes how to prevent, troubleshoot, diagnose, and mitigate connection errors and transient errors that your client application encounters when it interacts with Azure SQL Database.</span></span> <span data-ttu-id="4f1e6-106">Zjistěte, jak nakonfigurovat logika opakovaných pokusů, sestavení připojovacího řetězce a další nastavení připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-106">Learn how to configure retry logic, build the connection string, and adjust other connection settings.</span></span>

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a><span data-ttu-id="4f1e6-107">Přechodné chyby (přechodné chyby)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-107">Transient errors (transient faults)</span></span>
<span data-ttu-id="4f1e6-108">Přechodná chyba - navíc přechodná chyba - má základní příčinu, který bude brzy vyřeší.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-108">A transient error - also, transient fault - has an underlying cause that will soon resolve itself.</span></span> <span data-ttu-id="4f1e6-109">Příležitostně příčinu přechodné chyby je, když Azure systému rychle posune hardwarové prostředky lepší vyrovnávání zatížení různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-109">An occasional cause of transient errors is when the Azure system quickly shifts hardware resources to better load-balance various workloads.</span></span> <span data-ttu-id="4f1e6-110">Většinu těchto událostí Rekonfigurace často dokončit za méně než 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-110">Most of these reconfiguration events often complete in less than 60 seconds.</span></span> <span data-ttu-id="4f1e6-111">Během zadaného období Rekonfigurace může mít problémy s připojením k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-111">During this reconfiguration time span, you may have connectivity issues to Azure SQL Database.</span></span> <span data-ttu-id="4f1e6-112">Připojení k databázi SQL Azure aplikace by měly být vytvořeny očekávat tyto přechodné chyby, je zpracování, které implementují logiku opakovaných pokusů v svůj kód místo zpřístupnění je uživatelům jako chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-112">Applications connecting to Azure SQL Database should be built to expect these transient errors, handle them by implementing retry logic in their code instead of surfacing them to users as application errors.</span></span>

<span data-ttu-id="4f1e6-113">Pokud váš klientský program nepoužívá ADO.NET, je vašeho programu podle throw z vás o přechodné chybě vyzval **SqlException**.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-113">If your client program is using ADO.NET, your program is told about the transient error by the throw of an **SqlException**.</span></span> <span data-ttu-id="4f1e6-114">**Číslo** vlastnost může být porovná seznam přechodné chyby v horní části tématu: [kódy chyb SQL pro klientské aplikace SQL Database](sql-database-develop-error-messages.md).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-114">The **Number** property can be compared against the list of transient errors near the top of the topic: [SQL error codes for SQL Database client applications](sql-database-develop-error-messages.md).</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="4f1e6-115">Připojení a příkaz</span><span class="sxs-lookup"><span data-stu-id="4f1e6-115">Connection versus command</span></span>
<span data-ttu-id="4f1e6-116">Budete pokus o připojení SQL nebo znovu vytvořit v závislosti na následující:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-116">You'll retry the SQL connection or establish it again, depending on the following:</span></span>

* <span data-ttu-id="4f1e6-117">**Při pokusu o připojení dojde k přechodné chybě**: připojení je třeba opakovat po zpozdit pro několik sekund.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-117">**A transient error occurs during a connection try**: The connection should be retried after delaying for several seconds.</span></span>
* <span data-ttu-id="4f1e6-118">**Příkaz dotazu SQL došlo k přechodné chybě**: příkaz nesmí opakovat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-118">**A transient error occurs during a SQL query command**: The command should not be immediately retried.</span></span> <span data-ttu-id="4f1e6-119">Místo toho po prodlevě, připojení by se mělo čerstvě vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-119">Instead, after a delay, the connection should be freshly established.</span></span> <span data-ttu-id="4f1e6-120">Příkaz pak můžete opakovat.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-120">Then the command can be retried.</span></span>

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a><span data-ttu-id="4f1e6-121">Logika opakování pro přechodné chyby</span><span class="sxs-lookup"><span data-stu-id="4f1e6-121">Retry logic for transient errors</span></span>
<span data-ttu-id="4f1e6-122">Klient programy, které někdy dojde k přechodné chybě jsou robustnější při obsahují logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-122">Client programs that occasionally encounter a transient error are more robust when they contain retry logic.</span></span>

<span data-ttu-id="4f1e6-123">Pokud váš program komunikuje s Azure SQL Database pomocí 3. stran middleware, Dotázat se s dodavatelem zda middleware obsahuje logiku opakovaných pokusů pro přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-123">When your program communicates with Azure SQL Database through a 3rd party middleware, inquire with the vendor whether the middleware contains retry logic for transient errors.</span></span>

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a><span data-ttu-id="4f1e6-124">Zásady pro opakování</span><span class="sxs-lookup"><span data-stu-id="4f1e6-124">Principles for retry</span></span>
* <span data-ttu-id="4f1e6-125">Pokud je chyba přechodný je třeba opakovat pokus o otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-125">An attempt to open a connection should be retried if the error is transient.</span></span>
* <span data-ttu-id="4f1e6-126">Příkaz SELECT, který selže s přechodná chyba by neměl přímo opakovat.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-126">A SQL SELECT statement that fails with a transient error should not be retried directly.</span></span>
  
  * <span data-ttu-id="4f1e6-127">Místo toho udržování připojení a poté opakujte SELECT.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-127">Instead, establish a fresh connection, and then retry the SELECT.</span></span>
* <span data-ttu-id="4f1e6-128">Při aktualizaci SQL příkazu selže s přechodná chyba transakce, udržování připojení by se mělo vytvořit předtím, než se pokus o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-128">When a SQL UPDATE statement fails with a transient error, a fresh connection should be established before the UPDATE is retried.</span></span>
  
  * <span data-ttu-id="4f1e6-129">Logika opakovaných pokusů musí zajistit, že buď celé databáze transakce byla dokončena, nebo že celá transakce bude vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-129">The retry logic must ensure that either the entire database transaction completed, or that the entire transaction is rolled back.</span></span>

#### <a name="other-considerations-for-retry"></a><span data-ttu-id="4f1e6-130">Další důležité informace pro opakování</span><span class="sxs-lookup"><span data-stu-id="4f1e6-130">Other considerations for retry</span></span>
* <span data-ttu-id="4f1e6-131">Batch program, který se automaticky spustí po pracovní době, a před ráno, která dokončí si může dovolit velmi pacienta s dlouho časových intervalů mezi jeho počet pokusů o opakování.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-131">A batch program that is automatically started after work hours, and which will complete before morning, can afford to very patient with long time intervals between its retry attempts.</span></span>
* <span data-ttu-id="4f1e6-132">Program uživatelského rozhraní by měl účet pro lidské tendence, že uvolňovat po příliš dlouho čekání.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-132">A user interface program should account for the human tendency to give up after too long a wait.</span></span>
  
  * <span data-ttu-id="4f1e6-133">Však řešení nesmí být opakovat každých několik sekund, protože tyto zásady můžete vyplnění systému s žádostí.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-133">However, the solution must not be to retry every few seconds, because that policy can flood the system with requests.</span></span>

#### <a name="interval-increase-between-retries"></a><span data-ttu-id="4f1e6-134">Zvyšte interval mezi opakovanými pokusy</span><span class="sxs-lookup"><span data-stu-id="4f1e6-134">Interval increase between retries</span></span>
<span data-ttu-id="4f1e6-135">Doporučujeme, abyste zpoždění pro 5 sekund před první opakování.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-135">We recommend that you delay for 5 seconds before your first retry.</span></span> <span data-ttu-id="4f1e6-136">Opakování po prodlevě kratší než 5 sekund rizika zahltí cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-136">Retrying after a delay shorter than 5 seconds risks overwhelming the cloud service.</span></span> <span data-ttu-id="4f1e6-137">Pro každý další pokusy exponenciálnímu, růst zpoždění až do maximálního počtu 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-137">For each subsequent retry the delay should grow exponentially, up to a maximum of 60 seconds.</span></span>

<span data-ttu-id="4f1e6-138">Popis *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL sdružování připojení serveru (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-138">A discussion of the *blocking period* for clients that use ADO.NET is available in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span></span>

<span data-ttu-id="4f1e6-139">Také můžete chtít nastavit maximální počet opakovaných pokusů, než program samoobslužné ukončí.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-139">You might also want to set a maximum number of retries before the program self-terminates.</span></span>

#### <a name="code-samples-with-retry-logic"></a><span data-ttu-id="4f1e6-140">Ukázky kódu s logika opakovaných pokusů</span><span class="sxs-lookup"><span data-stu-id="4f1e6-140">Code samples with retry logic</span></span>
<span data-ttu-id="4f1e6-141">Ukázky kódu s logikou opakování v různých programovacích jazyků, jsou k dispozici na:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-141">Code samples with retry logic, in a variety of programming languages, are available at:</span></span>

* [<span data-ttu-id="4f1e6-142">Knihovny připojení k databázi SQL a SQL Server</span><span class="sxs-lookup"><span data-stu-id="4f1e6-142">Connection libraries for SQL Database and SQL Server</span></span>](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a><span data-ttu-id="4f1e6-143">Testování logika opakovaných pokusů</span><span class="sxs-lookup"><span data-stu-id="4f1e6-143">Test your retry logic</span></span>
<span data-ttu-id="4f1e6-144">K testování logika opakovaných pokusů, musíte simulovat nebo způsobit chybu, než může být vyřešen při běhu programu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-144">To test your retry logic, you must simulate or cause an error than can be corrected while your program is still running.</span></span>

##### <a name="test-by-disconnecting-from-the-network"></a><span data-ttu-id="4f1e6-145">Test odpojení od sítě</span><span class="sxs-lookup"><span data-stu-id="4f1e6-145">Test by disconnecting from the network</span></span>
<span data-ttu-id="4f1e6-146">Jedním ze způsobů, které můžete testovat logika opakovaných pokusů je klientský počítač odpojit od sítě, když je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-146">One way you can test your retry logic is to disconnect your client computer from the network while the program is running.</span></span> <span data-ttu-id="4f1e6-147">Chyba bude:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-147">The error will be:</span></span>

* <span data-ttu-id="4f1e6-148">**SqlException.Number** = 11001</span><span class="sxs-lookup"><span data-stu-id="4f1e6-148">**SqlException.Number** = 11001</span></span>
* <span data-ttu-id="4f1e6-149">Zpráva: "znám žádný takový hostitel je"</span><span class="sxs-lookup"><span data-stu-id="4f1e6-149">Message: "No such host is known"</span></span>

<span data-ttu-id="4f1e6-150">Jako součást první pokus opakovat můžete program opravte chyby v pravopisu a pak se pokusíte připojit.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-150">As part of the first retry attempt, your program can correct the misspelling, and then attempt to connect.</span></span>

<span data-ttu-id="4f1e6-151">Chcete-li to praktické, můžete odpojit počítači ze sítě, než aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-151">To make this practical, you unplug your computer from the network before you start your program.</span></span> <span data-ttu-id="4f1e6-152">Potom váš program rozpozná běhu parametr, který vede k:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-152">Then your program recognizes a run time parameter that causes the program to:</span></span>

1. <span data-ttu-id="4f1e6-153">11001 dočasně přidáte do seznamu chyb vzít v úvahu jako přechodný.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-153">Temporarily add 11001 to its list of errors to consider as transient.</span></span>
2. <span data-ttu-id="4f1e6-154">Pokus první připojení jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-154">Attempt its first connection as usual.</span></span>
3. <span data-ttu-id="4f1e6-155">Poté, co je chyba zachycení, odeberte 11001 ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-155">After the error is caught, remove 11001 from the list.</span></span>
4. <span data-ttu-id="4f1e6-156">Zobrazí zprávu, že připojit počítač k síti.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-156">Display a message telling the user to plug the computer into the network.</span></span>
   * <span data-ttu-id="4f1e6-157">Další pozastavit provádění pomocí **Console.ReadLine** metoda nebo dialogovém okně tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-157">Pause further execution by using either the **Console.ReadLine** method or a dialog with an OK button.</span></span> <span data-ttu-id="4f1e6-158">Uživatel stisknutím klávesy Enter po počítače připojené k síti.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-158">The user presses the Enter key after the computer plugged into the network.</span></span>
5. <span data-ttu-id="4f1e6-159">Pokusí znovu připojit, byla očekávána úspěch.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-159">Attempt again to connect, expecting success.</span></span>

##### <a name="test-by-misspelling-the-database-name-when-connecting"></a><span data-ttu-id="4f1e6-160">Test Chyba v pravopisu název databáze při připojování</span><span class="sxs-lookup"><span data-stu-id="4f1e6-160">Test by misspelling the database name when connecting</span></span>
<span data-ttu-id="4f1e6-161">Váš program můžete účelově chybně uživatelské jméno před první pokus o připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-161">Your program can purposely misspell the user name before the first connection attempt.</span></span> <span data-ttu-id="4f1e6-162">Chyba bude:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-162">The error will be:</span></span>

* <span data-ttu-id="4f1e6-163">**SqlException.Number** = 18456</span><span class="sxs-lookup"><span data-stu-id="4f1e6-163">**SqlException.Number** = 18456</span></span>
* <span data-ttu-id="4f1e6-164">Zpráva: "přihlášení se nezdařilo pro uživatele 'WRONG_MyUserName'."</span><span class="sxs-lookup"><span data-stu-id="4f1e6-164">Message: "Login failed for user 'WRONG_MyUserName'."</span></span>

<span data-ttu-id="4f1e6-165">Jako součást první pokus opakovat můžete program opravte chyby v pravopisu a pak se pokusíte připojit.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-165">As part of the first retry attempt, your program can correct the misspelling, and then attempt to connect.</span></span>

<span data-ttu-id="4f1e6-166">Chcete-li to praktické, může váš program rozpoznat běhu parametr, který vede k:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-166">To make this practical, your program could recognize a run time parameter that causes the program to:</span></span>

1. <span data-ttu-id="4f1e6-167">Dočasně 18456 přidejte do jeho seznam chyb vzít v úvahu jako přechodný.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-167">Temporarily add 18456 to its list of errors to consider as transient.</span></span>
2. <span data-ttu-id="4f1e6-168">Přidejte účelově 'WRONG_' na jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-168">Purposely add 'WRONG_' to the user name.</span></span>
3. <span data-ttu-id="4f1e6-169">Poté, co je chyba zachycení, odeberte 18456 ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-169">After the error is caught, remove 18456 from the list.</span></span>
4. <span data-ttu-id="4f1e6-170">Odeberte 'WRONG_' uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-170">Remove 'WRONG_' from the user name.</span></span>
5. <span data-ttu-id="4f1e6-171">Pokusí znovu připojit, byla očekávána úspěch.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-171">Attempt again to connect, expecting success.</span></span>

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a><span data-ttu-id="4f1e6-172">Parametry objektu SqlConnection .NET pro opakování připojení</span><span class="sxs-lookup"><span data-stu-id="4f1e6-172">.NET SqlConnection parameters for connection retry</span></span>
<span data-ttu-id="4f1e6-173">Pokud váš klientský program připojí k databázi SQL Azure s použitím třídy rozhraní .NET Framework **System.Data.SqlClient.SqlConnection**, měli byste použít .NET 4.6.1 nebo novější (nebo .NET Core), můžete použít funkci opakování jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-173">If your client program connects to to Azure SQL Database by using the .NET Framework class **System.Data.SqlClient.SqlConnection**, you should use .NET 4.6.1 or later (or .NET Core) so you can leverage its connection retry feature.</span></span> <span data-ttu-id="4f1e6-174">Podrobnosti o funkce jsou [zde](http://go.microsoft.com/fwlink/?linkid=393996).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-174">Details of the feature are [here](http://go.microsoft.com/fwlink/?linkid=393996).</span></span>

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


<span data-ttu-id="4f1e6-175">Při vytváření [připojovací řetězec](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) pro vaše **SqlConnection** objektu, by měly koordinovat hodnoty mezi následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-175">When you build the [connection string](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) for your **SqlConnection** object, you should coordinate the values among the following parameters:</span></span>

* <span data-ttu-id="4f1e6-176">ConnectRetryCount &nbsp; &nbsp; *(výchozí hodnota je 1. Rozsah je 0 až 255).*</span><span class="sxs-lookup"><span data-stu-id="4f1e6-176">ConnectRetryCount &nbsp;&nbsp;*(Default is 1. Range is 0 through 255.)*</span></span>
* <span data-ttu-id="4f1e6-177">ConnectRetryInterval &nbsp; &nbsp; *(výchozí hodnota je 1 sekunda. Rozsah je 1 až 60).*</span><span class="sxs-lookup"><span data-stu-id="4f1e6-177">ConnectRetryInterval &nbsp;&nbsp;*(Default is 1 second. Range is 1 through 60.)*</span></span>
* <span data-ttu-id="4f1e6-178">Časový limit připojení &nbsp; &nbsp; *(výchozí hodnota je 15 sekund. Rozsah hodnot je 0 až 2147483647)*</span><span class="sxs-lookup"><span data-stu-id="4f1e6-178">Connection Timeout &nbsp;&nbsp;*(Default is 15 seconds. Range is 0 through 2147483647)*</span></span>

<span data-ttu-id="4f1e6-179">Konkrétně vybrané hodnoty měli následující true rovnosti:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-179">Specifically, your chosen values should make the following equality true:</span></span>

* <span data-ttu-id="4f1e6-180">Časový limit připojení = ConnectRetryCount * ConnectionRetryInterval</span><span class="sxs-lookup"><span data-stu-id="4f1e6-180">Connection Timeout = ConnectRetryCount * ConnectionRetryInterval</span></span>

<span data-ttu-id="4f1e6-181">Například pokud je počet = 3 a interval = 10 sekund, vypršení časového limitu z jen 29 sekund nebude poměrně poskytnout systému dostatek času pro jeho 3. a finální opakování v připojení: 29 < 3 * 10.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-181">For example, if the count = 3, and interval = 10 seconds, a timeout of only 29 seconds would not quite give the system enough time for its 3rd and final retry at connecting: 29 < 3 * 10.</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="4f1e6-182">Připojení a příkaz</span><span class="sxs-lookup"><span data-stu-id="4f1e6-182">Connection versus command</span></span>
<span data-ttu-id="4f1e6-183">**ConnectRetryCount** a **ConnectRetryInterval** parametry umožní vaše **SqlConnection** objekt opakujte operaci připojení bez informuje nebo bothering programu, například vrácení řízení vašeho programu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-183">The **ConnectRetryCount** and **ConnectRetryInterval** parameters let your **SqlConnection** object retry the connect operation without telling or bothering your program, such as returning control to your program.</span></span> <span data-ttu-id="4f1e6-184">Opakované pokusy se může objevit v následujících situacích:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-184">The retries can occur in the following situations:</span></span>

* <span data-ttu-id="4f1e6-185">volání metody mySqlConnection.Open</span><span class="sxs-lookup"><span data-stu-id="4f1e6-185">mySqlConnection.Open method call</span></span>
* <span data-ttu-id="4f1e6-186">volání metody mySqlConnection.Execute</span><span class="sxs-lookup"><span data-stu-id="4f1e6-186">mySqlConnection.Execute method call</span></span>

<span data-ttu-id="4f1e6-187">Není k dispozici subtlety.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-187">There is a subtlety.</span></span> <span data-ttu-id="4f1e6-188">Pokud dojde k přechodné chybě. při vaší *dotazu* je spouštěna, vaše **SqlConnection** objekt neopakuje operace připojení a jeho určitě neopakuje dotazu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-188">If a transient error occurs while your *query* is being executed, your **SqlConnection** object does not retry the connect operation, and it certainly does not retry your query.</span></span> <span data-ttu-id="4f1e6-189">Ale **SqlConnection** velmi rychle zkontroluje připojení před odesláním dotazu pro provedení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-189">However, **SqlConnection** very quickly checks the connection before sending your query for execution.</span></span> <span data-ttu-id="4f1e6-190">Pokud Rychlá kontrola zjistí problému s připojením **SqlConnection** opakování operace připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-190">If the quick check detects a connection problem, **SqlConnection** retries the connect operation.</span></span> <span data-ttu-id="4f1e6-191">Pokud opakovaném úspěšné, odešle dotaz je pro provedení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-191">If the retry succeeds, you query is sent for execution.</span></span>

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a><span data-ttu-id="4f1e6-192">Měli ConnectRetryCount nelze kombinovat s logika opakovaných pokusů aplikace?</span><span class="sxs-lookup"><span data-stu-id="4f1e6-192">Should ConnectRetryCount be combined with application retry logic?</span></span>
<span data-ttu-id="4f1e6-193">Předpokládejme, že aplikace má robustní vlastní opakování logiku.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-193">Suppose your application has robust custom retry logic.</span></span> <span data-ttu-id="4f1e6-194">Může opakujte operaci připojení 4 časy.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-194">It might retry the connect operation 4 times.</span></span> <span data-ttu-id="4f1e6-195">Pokud přidáte **ConnectRetryInterval** a **ConnectRetryCount** = 3 pro připojovací řetězec, zvýšíte počet opakování do 4 * 3 = 12 opakování.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-195">If you add **ConnectRetryInterval** and **ConnectRetryCount** =3 to your connection string, you will increase the retry count to 4 * 3 = 12 retries.</span></span> <span data-ttu-id="4f1e6-196">Hodláte nemusí takové velký počet opakování.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-196">You might not intend such a high number of retries.</span></span>

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a><span data-ttu-id="4f1e6-197">Připojení k databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="4f1e6-197">Connections to Azure SQL Database</span></span>
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a><span data-ttu-id="4f1e6-198">Připojení: Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="4f1e6-198">Connection: Connection string</span></span>
<span data-ttu-id="4f1e6-199">Připojovací řetězec potřebné pro připojení k databázi SQL Azure se mírně liší z řetězce pro připojení k serveru Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-199">The connection string necessary for connecting to Azure SQL Database is slightly different from the string for connecting to Microsoft SQL Server.</span></span> <span data-ttu-id="4f1e6-200">Připojovací řetězec můžete zkopírovat pro vaši databázi z [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-200">You can copy the connection string for your database from the [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a><span data-ttu-id="4f1e6-201">Připojení: IP adresa</span><span class="sxs-lookup"><span data-stu-id="4f1e6-201">Connection: IP address</span></span>
<span data-ttu-id="4f1e6-202">Je nutné nakonfigurovat databázi SQL serveru tak, aby přijímal komunikaci z IP adresy počítače, který hostuje váš klientský program.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-202">You must configure the SQL Database server to accept communication from the IP address of the computer that hosts your client program.</span></span> <span data-ttu-id="4f1e6-203">To uděláte tak, že upravíte nastavení brány firewall pomocí [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-203">You do this by editing the firewall settings through the [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="4f1e6-204">Pokud zapomenete konfigurace IP adresy, váš program selže s užitečný chybovou zprávu, která stavy nezbytné IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-204">If you forget to configure the IP address, your program will fail with a handy error message that states the necessary IP address.</span></span>

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

<span data-ttu-id="4f1e6-205">Další informace najdete v tématu: [postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-205">For more information, see: [How to: Configure firewall settings on SQL Database](sql-database-configure-firewall-settings.md)</span></span>

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a><span data-ttu-id="4f1e6-206">Připojení: porty</span><span class="sxs-lookup"><span data-stu-id="4f1e6-206">Connection: Ports</span></span>
<span data-ttu-id="4f1e6-207">Obvykle potřebujete jenom zajistěte, aby byl port 1433 otevřený pro odchozí komunikaci, na počítači, který je hostitelem program klienta.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-207">Typically you only need to ensure that port 1433 is open for outbound communication, on the computer that hosts you client program.</span></span>

<span data-ttu-id="4f1e6-208">Například pokud je váš klientský program hostované na počítači se systémem Windows, brána Windows Firewall na hostiteli vám umožní otevřít port 1433:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-208">For example, when your client program is hosted on a Windows computer, the Windows Firewall on the host enables you to open port 1433:</span></span>

1. <span data-ttu-id="4f1e6-209">Otevřete ovládací panely</span><span class="sxs-lookup"><span data-stu-id="4f1e6-209">Open the Control Panel</span></span>
2. <span data-ttu-id="4f1e6-210">&gt;Všechny položky Ovládacích panelů</span><span class="sxs-lookup"><span data-stu-id="4f1e6-210">&gt; All Control Panel Items</span></span>
3. <span data-ttu-id="4f1e6-211">&gt;Brána Windows Firewall</span><span class="sxs-lookup"><span data-stu-id="4f1e6-211">&gt; Windows Firewall</span></span>
4. <span data-ttu-id="4f1e6-212">&gt;Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="4f1e6-212">&gt; Advanced Settings</span></span>
5. <span data-ttu-id="4f1e6-213">&gt;Odchozí pravidla</span><span class="sxs-lookup"><span data-stu-id="4f1e6-213">&gt; Outbound Rules</span></span>
6. <span data-ttu-id="4f1e6-214">&gt;Akce</span><span class="sxs-lookup"><span data-stu-id="4f1e6-214">&gt; Actions</span></span>
7. <span data-ttu-id="4f1e6-215">&gt;Nové pravidlo</span><span class="sxs-lookup"><span data-stu-id="4f1e6-215">&gt; New Rule</span></span>

<span data-ttu-id="4f1e6-216">Pokud je váš klientský program hostované na virtuální počítač Azure (VM), měli byste si přečíst:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-216">If your client program is hosted on an Azure virtual machine (VM), you should read:</span></span><br/><span data-ttu-id="4f1e6-217">[Porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-217">[Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

<span data-ttu-id="4f1e6-218">Základní informace o konfigurace portů a IP adresy, najdete v tématu: [brány firewall databáze Azure SQL Database](sql-database-firewall-configure.md)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-218">For background information about cofiguration of ports and IP address, see: [Azure SQL Database firewall](sql-database-firewall-configure.md)</span></span>

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a><span data-ttu-id="4f1e6-219">Připojení: ADO.NET 4.6.1</span><span class="sxs-lookup"><span data-stu-id="4f1e6-219">Connection: ADO.NET 4.6.1</span></span>
<span data-ttu-id="4f1e6-220">Pokud váš program používá třídy ADO.NET jako **System.Data.SqlClient.SqlConnection** pro připojení k databázi SQL Azure, doporučujeme použít rozhraní .NET Framework verze 4.6.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-220">If your program uses ADO.NET classes like **System.Data.SqlClient.SqlConnection** to connect to Azure SQL Database, we recommend that you use .NET Framework version 4.6.1 or higher.</span></span>

<span data-ttu-id="4f1e6-221">ADO.NET 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-221">ADO.NET 4.6.1:</span></span>

* <span data-ttu-id="4f1e6-222">Pro databázi SQL Azure, je lepší spolehlivostí při otevření připojení pomocí **SqlConnection.Open** metoda.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-222">For Azure SQL Database, there is improved reliability when you open a connection by using the **SqlConnection.Open** method.</span></span> <span data-ttu-id="4f1e6-223">**Otevřete** metoda nyní zahrnuje nejlepší mechanismy úsilí opakování v reakci na přechodných chyb pro určitým chybám v časovém limitu připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-223">The **Open** method now incorporates best effort retry mechanisms in response to transient faults, for certain errors within the Connection Timeout period.</span></span>
* <span data-ttu-id="4f1e6-224">Podporuje sdružování připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-224">Supports connection pooling.</span></span> <span data-ttu-id="4f1e6-225">To zahrnuje efektivní ověření, který funguje objekt připojení nabízí vašeho programu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-225">This includes an efficient verification that the connection object it gives your program is functioning.</span></span>

<span data-ttu-id="4f1e6-226">Použijete-li objekt připojení z fondu připojení, doporučujeme vám, že vašeho programu dočasně uzavřít připojení, když není okamžitě používat.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-226">When you use a connection object from a connection pool, we recommend that your program temporarily close the connection when not immediately using it.</span></span> <span data-ttu-id="4f1e6-227">Opakovaném otevření připojení není nákladné, je způsob vytvoření nového připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-227">Re-opening a connection is not expensive the way creating a new connection is.</span></span>

<span data-ttu-id="4f1e6-228">Pokud používáte ADO.NET 4.0 nebo starší, doporučujeme upgradovat na nejnovější ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-228">If you are using ADO.NET 4.0 or earlier, we recommend that you upgrade to the latest ADO.NET.</span></span>

* <span data-ttu-id="4f1e6-229">Od listopadu 2015, můžete [stáhnout ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-229">As of November 2015, you can [download ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span></span>

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a><span data-ttu-id="4f1e6-230">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="4f1e6-230">Diagnostics</span></span>
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a><span data-ttu-id="4f1e6-231">Diagnostika: Test, zda může připojit nástroje</span><span class="sxs-lookup"><span data-stu-id="4f1e6-231">Diagnostics: Test whether utilities can connect</span></span>
<span data-ttu-id="4f1e6-232">Pokud váš program se nedaří připojit k databázi SQL Azure, je pokusí připojit k programu nástroj jednu možnost diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-232">If your program is failing to connect to Azure SQL Database, one diagnostic option is to try to connect with a utility program.</span></span> <span data-ttu-id="4f1e6-233">V ideálním případě by nástroj připojit pomocí stejné knihovny, který program používá.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-233">Ideally the utility would connect by using the same library that your program uses.</span></span>

<span data-ttu-id="4f1e6-234">Na libovolném počítači Windows můžete vyzkoušet tyto nástroje:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-234">On any Windows computer, you can try these utilities:</span></span>

* <span data-ttu-id="4f1e6-235">SQL Server Management Studio (ssms.exe), který se připojuje pomocí ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-235">SQL Server Management Studio (ssms.exe), which connects by using ADO.NET.</span></span>
* <span data-ttu-id="4f1e6-236">Sqlcmd.exe, který se připojuje pomocí [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-236">sqlcmd.exe, which connects by using [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span></span>

<span data-ttu-id="4f1e6-237">Po připojení otestujte, zda funguje krátké SQL SELECT dotazu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-237">Once connected, test whether a short SQL SELECT query works.</span></span>

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a><span data-ttu-id="4f1e6-238">Diagnostika: Zkontrolujte otevřené porty</span><span class="sxs-lookup"><span data-stu-id="4f1e6-238">Diagnostics: Check the open ports</span></span>
<span data-ttu-id="4f1e6-239">Předpokládejme, že máte podezření, že pokusy o připojení se nedaří kvůli problémům s portu.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-239">Suppose you suspect that connection attempts are failing due to port issues.</span></span> <span data-ttu-id="4f1e6-240">V počítači můžete spustit nástroj, který sestavy na konfiguraci portů.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-240">On your computer you can run a utility that reports on the port configurations.</span></span>

<span data-ttu-id="4f1e6-241">V systému Linux mohou být užitečné následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-241">On Linux the following utilities might be helpful:</span></span>

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * <span data-ttu-id="4f1e6-242">(Změnit příklad hodnota, která má být IP adresa.)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-242">(Change the example value to be your IP address.)</span></span>

<span data-ttu-id="4f1e6-243">V systému Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) nástroj může být užitečné.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-243">On Windows the [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utility might be helpful.</span></span> <span data-ttu-id="4f1e6-244">Tady je příklad spuštění, dotaz situace port na serveru Azure SQL Database a která byla spuštěna na přenosný počítač:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-244">Here is an example execution that queried the port situation on an Azure SQL Database server, and which was run on a laptop computer:</span></span>

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a><span data-ttu-id="4f1e6-245">Diagnostiky: Chyby protokolu</span><span class="sxs-lookup"><span data-stu-id="4f1e6-245">Diagnostics: Log your errors</span></span>
<span data-ttu-id="4f1e6-246">Občasný problém je někdy nejlépe zjištěném detekce obecné vzoru přes dny nebo týdny.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-246">An intermittent problem is sometimes best diagnosed by detection of a general pattern over days or weeks.</span></span>

<span data-ttu-id="4f1e6-247">Váš klient může být užitečné při diagnostiku protokolováním všechny chyby, který nalezne.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-247">Your client can assist in a diagnosis by logging all errors it encounters.</span></span> <span data-ttu-id="4f1e6-248">Je možné ke korelaci položky protokolu s daty chyba, která interně Azure SQL Database samotné protokoly.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-248">You might be able to correlate the log entries with error data that Azure SQL Database logs itself internally.</span></span>

<span data-ttu-id="4f1e6-249">Enterprise knihovny 6 (EntLib60) nabízí spravované rozhraní .NET třídy vám pomůže při protokolování:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-249">Enterprise Library 6 (EntLib60) offers .NET managed classes to assist with logging:</span></span>

* [<span data-ttu-id="4f1e6-250">5 - stejně snadná jako návratem vypnout protokolu: používání bloku protokolování aplikace</span><span class="sxs-lookup"><span data-stu-id="4f1e6-250">5 - As Easy As Falling Off a Log: Using the Logging Application Block</span></span>](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a><span data-ttu-id="4f1e6-251">Diagnostika: Zkontrolujte protokoly systému chyby</span><span class="sxs-lookup"><span data-stu-id="4f1e6-251">Diagnostics: Examine system logs for errors</span></span>
<span data-ttu-id="4f1e6-252">Tady jsou některé příkazy jazyka Transact-SQL, vyberte tento dotaz protokoly chyby a dalšími informacemi.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-252">Here are some Transact-SQL SELECT statements that query logs of error and other information.</span></span>

| <span data-ttu-id="4f1e6-253">Dotaz protokolu</span><span class="sxs-lookup"><span data-stu-id="4f1e6-253">Query of log</span></span> | <span data-ttu-id="4f1e6-254">Popis</span><span class="sxs-lookup"><span data-stu-id="4f1e6-254">Description</span></span> |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |<span data-ttu-id="4f1e6-255">[Sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) zobrazení nabízí informace o jednotlivých událostech, včetně některých, která mohou způsobit přechodné chyby nebo chyby připojení.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-255">The [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) view offers information about individual events, including some that can cause transient errors or connectivity failures.</span></span><br/><br/><span data-ttu-id="4f1e6-256">V ideálním případě mohou korelovat **start_time** nebo **end_time** hodnoty s informacemi o když váš klientský program došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-256">Ideally you can correlate the **start_time** or **end_time** values with information about when your client program experienced problems.</span></span><br/><br/><span data-ttu-id="4f1e6-257">**TIP:** musíte se připojit k **hlavní** databázi, aby ji spustit.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-257">**TIP:** You must connect to the **master** database to run this.</span></span> |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |<span data-ttu-id="4f1e6-258">[Sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) zobrazení nabízí souhrnné počty typů událostí pro další diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-258">The [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) view offers aggregated counts of event types, for additional diagnostics.</span></span><br/><br/><span data-ttu-id="4f1e6-259">**TIP:** musíte se připojit k **hlavní** databázi, aby ji spustit.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-259">**TIP:** You must connect to the **master** database to run this.</span></span> |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a><span data-ttu-id="4f1e6-260">Diagnostika: Vyhledejte problém události v protokolu databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4f1e6-260">Diagnostics: Search for problem events in the SQL Database log</span></span>
<span data-ttu-id="4f1e6-261">Můžete hledat položky o problém události v protokolu databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-261">You can search for entries about problem events in the log of Azure SQL Database.</span></span> <span data-ttu-id="4f1e6-262">Zkuste následující příkaz Transact-SQL, vyberte v **hlavní** databáze:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-262">Try the following Transact-SQL SELECT statement in the **master** database:</span></span>

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a><span data-ttu-id="4f1e6-263">Několik řádků vrácená z sys.fn_xe_telemetry_blob_target_read_file</span><span class="sxs-lookup"><span data-stu-id="4f1e6-263">A few returned rows from sys.fn_xe_telemetry_blob_target_read_file</span></span>
<span data-ttu-id="4f1e6-264">Dále je, jak může vypadat vrácený řádek.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-264">Next is what a returned row might look like.</span></span> <span data-ttu-id="4f1e6-265">Hodnoty null, zobrazí se často není null v dalších řádcích.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-265">The null values shown are often not null in other rows.</span></span>

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a><span data-ttu-id="4f1e6-266">Knihovna Enterprise 6</span><span class="sxs-lookup"><span data-stu-id="4f1e6-266">Enterprise Library 6</span></span>
<span data-ttu-id="4f1e6-267">Enterprise knihovny 6 (EntLib60) je architektura tříd rozhraní .NET usnadňující implementovat robustní klienti cloudové služby, z nichž jeden je služba Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-267">Enterprise Library 6 (EntLib60) is a framework of .NET classes that helps you implement robust clients of cloud services, one of which is the Azure SQL Database service.</span></span> <span data-ttu-id="4f1e6-268">Můžete vyhledat témata vyhrazený pro každou oblast, ve kterém může být užitečné EntLib60 první navštivte stránky:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-268">You can locate topics dedicated to each area in which EntLib60 can assist by first visiting:</span></span>

* [<span data-ttu-id="4f1e6-269">Knihovna Enterprise 6 – duben 2013</span><span class="sxs-lookup"><span data-stu-id="4f1e6-269">Enterprise Library 6 - April 2013</span></span>](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

<span data-ttu-id="4f1e6-270">Logika opakovaných pokusů pro přechodné chyby zpracování je jedné oblasti, ve kterém může být užitečné EntLib60:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-270">Retry logic for handling transient errors is one area in which EntLib60 can assist:</span></span>

* [<span data-ttu-id="4f1e6-271">4 - perseverance, tajný klíč všechny vítězství: použití bloku aplikace přechodná chyba zpracování</span><span class="sxs-lookup"><span data-stu-id="4f1e6-271">4 - Perseverance, Secret of All Triumphs: Using the Transient Fault Handling Application Block</span></span>](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> <span data-ttu-id="4f1e6-272">Zdrojový kód pro EntLib60 je k dispozici pro veřejné [Stáhnout](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-272">The source code for EntLib60 is available for public [download](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span></span> <span data-ttu-id="4f1e6-273">Společnost Microsoft nemá žádné plány pro EntLib provést další funkce aktualizace nebo aktualizace údržby.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-273">Microsoft has no plans to make further feature updates or maintenance updates to EntLib.</span></span>
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a><span data-ttu-id="4f1e6-274">Třídy EntLib60 pro přechodné chyby a zkuste to znovu</span><span class="sxs-lookup"><span data-stu-id="4f1e6-274">EntLib60 classes for transient errors and retry</span></span>
<span data-ttu-id="4f1e6-275">Následující třídy EntLib60 jsou obzvláště užitečná pro logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-275">The following EntLib60 classes are particularly useful for retry logic.</span></span> <span data-ttu-id="4f1e6-276">Všechny tyto v, nebo další pod oborem názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-276">All these  are in, or are further under, the namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span></span>

<span data-ttu-id="4f1e6-277">*V oboru názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span><span class="sxs-lookup"><span data-stu-id="4f1e6-277">*In the namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span></span>

* <span data-ttu-id="4f1e6-278">**RetryPolicy** – třída</span><span class="sxs-lookup"><span data-stu-id="4f1e6-278">**RetryPolicy** class</span></span>
  
  * <span data-ttu-id="4f1e6-279">**ExecuteAction** – metoda</span><span class="sxs-lookup"><span data-stu-id="4f1e6-279">**ExecuteAction** method</span></span>
* <span data-ttu-id="4f1e6-280">**ExponentialBackoff** – třída</span><span class="sxs-lookup"><span data-stu-id="4f1e6-280">**ExponentialBackoff** class</span></span>
* <span data-ttu-id="4f1e6-281">**SqlDatabaseTransientErrorDetectionStrategy** – třída</span><span class="sxs-lookup"><span data-stu-id="4f1e6-281">**SqlDatabaseTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="4f1e6-282">**ReliableSqlConnection** – třída</span><span class="sxs-lookup"><span data-stu-id="4f1e6-282">**ReliableSqlConnection** class</span></span>
  
  * <span data-ttu-id="4f1e6-283">**Parametr ExecuteCommand** – metoda</span><span class="sxs-lookup"><span data-stu-id="4f1e6-283">**ExecuteCommand** method</span></span>

<span data-ttu-id="4f1e6-284">V oboru názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-284">In the namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span></span>

* <span data-ttu-id="4f1e6-285">**AlwaysTransientErrorDetectionStrategy** – třída</span><span class="sxs-lookup"><span data-stu-id="4f1e6-285">**AlwaysTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="4f1e6-286">**NeverTransientErrorDetectionStrategy** – třída</span><span class="sxs-lookup"><span data-stu-id="4f1e6-286">**NeverTransientErrorDetectionStrategy** class</span></span>

<span data-ttu-id="4f1e6-287">Tady jsou odkazy na informace o EntLib60:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-287">Here are links to information about EntLib60:</span></span>

* <span data-ttu-id="4f1e6-288">Volné [sešit stažení: Příručka pro vývojáře do knihovny Microsoft Enterprise, verze 2.](http://www.microsoft.com/download/details.aspx?id=41145)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-288">Free [Book Download: Developer's Guide to Microsoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span></span>
* <span data-ttu-id="4f1e6-289">Osvědčené postupy: [opakujte obecné pokyny](../best-practices-retry-general.md) má vynikající podrobné informace o logika opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-289">Best practices: [Retry general guidance](../best-practices-retry-general.md) has an excellent in-depth discussion of retry logic.</span></span>
* <span data-ttu-id="4f1e6-290">Stahování NuGet [Enterprise Library - přechodných chyb aplikace bloku 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-290">NuGet download of [Enterprise Library - Transient Fault Handling application block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span></span>

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a><span data-ttu-id="4f1e6-291">EntLib60: Protokolování bloku</span><span class="sxs-lookup"><span data-stu-id="4f1e6-291">EntLib60: The logging block</span></span>
* <span data-ttu-id="4f1e6-292">Blok protokolování je velmi flexibilní a konfigurovat řešení, které vám umožní:</span><span class="sxs-lookup"><span data-stu-id="4f1e6-292">The Logging block is a highly flexible and configurable solution that allows you to:</span></span>
  
  * <span data-ttu-id="4f1e6-293">Vytvoření a uložení zprávy protokolu v celé řadě umístění.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-293">Create and store log messages in a wide variety of locations.</span></span>
  * <span data-ttu-id="4f1e6-294">Kategorizaci a filtrování zprávy.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-294">Categorize and filter messages.</span></span>
  * <span data-ttu-id="4f1e6-295">Shromážděte kontextové informace, které jsou užitečné pro ladění a trasování, a také pro auditování a obecné protokolování požadavky.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-295">Collect contextual information that is useful for debugging and tracing, as well as for auditing and general logging requirements.</span></span>
* <span data-ttu-id="4f1e6-296">Protokolování bloku abstrahuje funkce protokolování cíl protokolu tak, aby je konzistentní, bez ohledu na umístění a typ úložiště cíl protokolování kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-296">The Logging block abstracts the logging functionality from the log destination so that the application code is consistent, irrespective of the location and type of the target logging store.</span></span>

<span data-ttu-id="4f1e6-297">Podrobnosti najdete v tématu: [5 – jako snadno jako návratem vypnout protokolu: pomocí blokem protokolování aplikace](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-297">For details see: [5 - As Easy As Falling Off a Log: Using the Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span></span>

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a><span data-ttu-id="4f1e6-298">EntLib60 IsTransient metoda zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="4f1e6-298">EntLib60 IsTransient method source code</span></span>
<span data-ttu-id="4f1e6-299">Další z **SqlDatabaseTransientErrorDetectionStrategy** třídy, je C# zdrojový kód **IsTransient** metoda.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-299">Next, from the **SqlDatabaseTransientErrorDetectionStrategy** class, is the C# source code for the **IsTransient** method.</span></span> <span data-ttu-id="4f1e6-300">Zdrojový kód vysvětluje, které chyby se považuje za přechodný a worthy z opakování od dubna 2013.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-300">The source code clarifies which errors were considered to be transient and worthy of retry, as of April 2013.</span></span>

<span data-ttu-id="4f1e6-301">Řada **//comment** řádky byly odebrány z této kopie zdůraznit čitelnost.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-301">Numerous **//comment** lines have been removed from this copy to emphasize readability.</span></span>

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a><span data-ttu-id="4f1e6-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f1e6-302">Next steps</span></span>
* <span data-ttu-id="4f1e6-303">Řešení potíží s další běžné problémy s připojením databáze SQL Azure, najdete v článku [řešení problémů s připojením k databázi SQL Azure](sql-database-troubleshoot-common-connection-issues.md).</span><span class="sxs-lookup"><span data-stu-id="4f1e6-303">For troubleshooting other common Azure SQL Database connection issues, visit [Troubleshoot connection issues to Azure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span></span>
* [<span data-ttu-id="4f1e6-304">Připojení k serveru SQL sdružování (ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="4f1e6-304">SQL Server Connection Pooling (ADO.NET)</span></span>](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [<span data-ttu-id="4f1e6-305">*Opakováním* je Apache 2.0 licenci pro obecné účely, opakování knihovny, které jsou napsané v **Python**, aby se zjednodušila úkolu přidávání opakování chování pro jakoukoli.</span><span class="sxs-lookup"><span data-stu-id="4f1e6-305">*Retrying* is an Apache 2.0 licensed general-purpose retrying library, written in **Python**, to simplify the task of adding retry behavior to just about anything.</span></span>](https://pypi.python.org/pypi/retrying)

