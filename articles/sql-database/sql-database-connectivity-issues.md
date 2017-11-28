---
title: "aaaFix chyby připojení SQL, přechodné chybě. | Microsoft Docs"
description: "Zjistěte, jak diagnostikovat tootroubleshoot a zabránit chyba připojení SQL nebo přechodné chybě ve službě Azure SQL Database. "
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
ms.openlocfilehash: d225e610b9e88170ab53ca16d615bd07220603cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a><span data-ttu-id="59a45-104">Oprava a diagnostika chyb připojení SQL a přechodných chyb služby SQL Database a jejich předcházení</span><span class="sxs-lookup"><span data-stu-id="59a45-104">Troubleshoot, diagnose, and prevent SQL connection errors and transient errors for SQL Database</span></span>
<span data-ttu-id="59a45-105">Tento článek popisuje, jak tooprevent, odstraňování potíží, diagnostikovat a zmírnit chyb připojení a přechodné chyby, které klientské aplikace, zaznamená při komunikuje se službou Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="59a45-105">This article describes how tooprevent, troubleshoot, diagnose, and mitigate connection errors and transient errors that your client application encounters when it interacts with Azure SQL Database.</span></span> <span data-ttu-id="59a45-106">Zjistěte, jak tooconfigure logika opakovaných pokusů, sestavení hello připojovací řetězec a další nastavení připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-106">Learn how tooconfigure retry logic, build hello connection string, and adjust other connection settings.</span></span>

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a><span data-ttu-id="59a45-107">Přechodné chyby (přechodné chyby)</span><span class="sxs-lookup"><span data-stu-id="59a45-107">Transient errors (transient faults)</span></span>
<span data-ttu-id="59a45-108">Přechodná chyba - navíc přechodná chyba - má základní příčinu, který bude brzy vyřeší.</span><span class="sxs-lookup"><span data-stu-id="59a45-108">A transient error - also, transient fault - has an underlying cause that will soon resolve itself.</span></span> <span data-ttu-id="59a45-109">Příležitostně příčinu přechodné chyby je, když hello Azure systému rychle posune hardwarové prostředky toobetter Vyrovnávání zatížení různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="59a45-109">An occasional cause of transient errors is when hello Azure system quickly shifts hardware resources toobetter load-balance various workloads.</span></span> <span data-ttu-id="59a45-110">Většinu těchto událostí Rekonfigurace často dokončit za méně než 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="59a45-110">Most of these reconfiguration events often complete in less than 60 seconds.</span></span> <span data-ttu-id="59a45-111">Během zadaného období Rekonfigurace může mít připojení k problémy tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="59a45-111">During this reconfiguration time span, you may have connectivity issues tooAzure SQL Database.</span></span> <span data-ttu-id="59a45-112">Aplikace připojení tooAzure SQL databáze by měla být vytvářeny tooexpect tyto přechodné chyby popisovač je implementací opakujte logiku v jejich kódu místo zpřístupnění je toousers jako chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="59a45-112">Applications connecting tooAzure SQL Database should be built tooexpect these transient errors, handle them by implementing retry logic in their code instead of surfacing them toousers as application errors.</span></span>

<span data-ttu-id="59a45-113">Pokud váš klientský program nepoužívá ADO.NET, váš program se nedá instrukce o přechodné chybě hello hello throw z **SqlException**.</span><span class="sxs-lookup"><span data-stu-id="59a45-113">If your client program is using ADO.NET, your program is told about hello transient error by hello throw of an **SqlException**.</span></span> <span data-ttu-id="59a45-114">Hello **číslo** vlastnost může být porovná hello seznam přechodných chyb uvedených hello horní části tématu hello: [kódy chyb SQL pro klientské aplikace SQL Database](sql-database-develop-error-messages.md).</span><span class="sxs-lookup"><span data-stu-id="59a45-114">hello **Number** property can be compared against hello list of transient errors near hello top of hello topic: [SQL error codes for SQL Database client applications](sql-database-develop-error-messages.md).</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="59a45-115">Připojení a příkaz</span><span class="sxs-lookup"><span data-stu-id="59a45-115">Connection versus command</span></span>
<span data-ttu-id="59a45-116">Budete opakujte pokus o připojení SQL hello nebo znovu vytvořit v závislosti na hello následující:</span><span class="sxs-lookup"><span data-stu-id="59a45-116">You'll retry hello SQL connection or establish it again, depending on hello following:</span></span>

* <span data-ttu-id="59a45-117">**Při pokusu o připojení dojde k přechodné chybě**: hello připojení je třeba opakovat po zpozdit pro několik sekund.</span><span class="sxs-lookup"><span data-stu-id="59a45-117">**A transient error occurs during a connection try**: hello connection should be retried after delaying for several seconds.</span></span>
* <span data-ttu-id="59a45-118">**Příkaz dotazu SQL došlo k přechodné chybě**: příkaz hello nesmí opakovat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="59a45-118">**A transient error occurs during a SQL query command**: hello command should not be immediately retried.</span></span> <span data-ttu-id="59a45-119">Místo toho po prodlevě, hello by měl být čerstvě navázat připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-119">Instead, after a delay, hello connection should be freshly established.</span></span> <span data-ttu-id="59a45-120">Potom můžete zkusit znovu příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-120">Then hello command can be retried.</span></span>

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a><span data-ttu-id="59a45-121">Logika opakování pro přechodné chyby</span><span class="sxs-lookup"><span data-stu-id="59a45-121">Retry logic for transient errors</span></span>
<span data-ttu-id="59a45-122">Klient programy, které někdy dojde k přechodné chybě jsou robustnější při obsahují logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="59a45-122">Client programs that occasionally encounter a transient error are more robust when they contain retry logic.</span></span>

<span data-ttu-id="59a45-123">Pokud váš program komunikuje s Azure SQL Database pomocí 3. stran middleware, Dotázat se s dodavatelem hello zda hello middleware obsahuje logiku opakovaných pokusů pro přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="59a45-123">When your program communicates with Azure SQL Database through a 3rd party middleware, inquire with hello vendor whether hello middleware contains retry logic for transient errors.</span></span>

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a><span data-ttu-id="59a45-124">Zásady pro opakování</span><span class="sxs-lookup"><span data-stu-id="59a45-124">Principles for retry</span></span>
* <span data-ttu-id="59a45-125">Pokud hello chyba je přechodná je třeba opakovat tooopen pokusu o připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-125">An attempt tooopen a connection should be retried if hello error is transient.</span></span>
* <span data-ttu-id="59a45-126">Příkaz SELECT, který selže s přechodná chyba by neměl přímo opakovat.</span><span class="sxs-lookup"><span data-stu-id="59a45-126">A SQL SELECT statement that fails with a transient error should not be retried directly.</span></span>
  
  * <span data-ttu-id="59a45-127">Místo toho udržování připojení a poté opakujte hello vyberte.</span><span class="sxs-lookup"><span data-stu-id="59a45-127">Instead, establish a fresh connection, and then retry hello SELECT.</span></span>
* <span data-ttu-id="59a45-128">Nepodaří-li prohlášení aktualizace SQL s přechodná chyba transakce, musí před hello, které se pokus o aktualizaci vytvořit novou připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-128">When a SQL UPDATE statement fails with a transient error, a fresh connection should be established before hello UPDATE is retried.</span></span>
  
  * <span data-ttu-id="59a45-129">Logika opakovaných pokusů Hello musíte zajistit, že buď hello celou databázi transakce byla dokončena, nebo že hello celá transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="59a45-129">hello retry logic must ensure that either hello entire database transaction completed, or that hello entire transaction is rolled back.</span></span>

#### <a name="other-considerations-for-retry"></a><span data-ttu-id="59a45-130">Další důležité informace pro opakování</span><span class="sxs-lookup"><span data-stu-id="59a45-130">Other considerations for retry</span></span>
* <span data-ttu-id="59a45-131">Batch program, který se automaticky spustí po pracovní době, a před ráno, která dokončí si může dovolit toovery pacienta s dlouho časových intervalů mezi jeho počet pokusů o opakování.</span><span class="sxs-lookup"><span data-stu-id="59a45-131">A batch program that is automatically started after work hours, and which will complete before morning, can afford toovery patient with long time intervals between its retry attempts.</span></span>
* <span data-ttu-id="59a45-132">Program uživatelského rozhraní by měl účet pro toogive hello lidského tendence, že se až po příliš dlouho čekání.</span><span class="sxs-lookup"><span data-stu-id="59a45-132">A user interface program should account for hello human tendency toogive up after too long a wait.</span></span>
  
  * <span data-ttu-id="59a45-133">Ale hello řešení nesmí být tooretry každých několik sekund, protože tyto zásady můžete vyplnění hello systému s žádostí.</span><span class="sxs-lookup"><span data-stu-id="59a45-133">However, hello solution must not be tooretry every few seconds, because that policy can flood hello system with requests.</span></span>

#### <a name="interval-increase-between-retries"></a><span data-ttu-id="59a45-134">Zvyšte interval mezi opakovanými pokusy</span><span class="sxs-lookup"><span data-stu-id="59a45-134">Interval increase between retries</span></span>
<span data-ttu-id="59a45-135">Doporučujeme, abyste zpoždění pro 5 sekund před první opakování.</span><span class="sxs-lookup"><span data-stu-id="59a45-135">We recommend that you delay for 5 seconds before your first retry.</span></span> <span data-ttu-id="59a45-136">Opakování po prodlevě kratší než 5 sekund rizika čtenáře hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="59a45-136">Retrying after a delay shorter than 5 seconds risks overwhelming hello cloud service.</span></span> <span data-ttu-id="59a45-137">Pro každý další pokusy hello zpoždění by měl růst exponenciálnímu, až tooa maximálně 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="59a45-137">For each subsequent retry hello delay should grow exponentially, up tooa maximum of 60 seconds.</span></span>

<span data-ttu-id="59a45-138">Diskuzi o hello *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL sdružování připojení serveru (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a45-138">A discussion of hello *blocking period* for clients that use ADO.NET is available in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span></span>

<span data-ttu-id="59a45-139">Můžete také tooset maximální počet opakovaných pokusů před programu hello samoobslužné ukončí.</span><span class="sxs-lookup"><span data-stu-id="59a45-139">You might also want tooset a maximum number of retries before hello program self-terminates.</span></span>

#### <a name="code-samples-with-retry-logic"></a><span data-ttu-id="59a45-140">Ukázky kódu s logika opakovaných pokusů</span><span class="sxs-lookup"><span data-stu-id="59a45-140">Code samples with retry logic</span></span>
<span data-ttu-id="59a45-141">Ukázky kódu s logikou opakování v různých programovacích jazyků, jsou k dispozici na:</span><span class="sxs-lookup"><span data-stu-id="59a45-141">Code samples with retry logic, in a variety of programming languages, are available at:</span></span>

* [<span data-ttu-id="59a45-142">Knihovny připojení k databázi SQL a SQL Server</span><span class="sxs-lookup"><span data-stu-id="59a45-142">Connection libraries for SQL Database and SQL Server</span></span>](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a><span data-ttu-id="59a45-143">Testování logika opakovaných pokusů</span><span class="sxs-lookup"><span data-stu-id="59a45-143">Test your retry logic</span></span>
<span data-ttu-id="59a45-144">tootest logika opakovaných pokusů musí simulovat nebo způsobit chybu, než může být vyřešen při běhu programu.</span><span class="sxs-lookup"><span data-stu-id="59a45-144">tootest your retry logic, you must simulate or cause an error than can be corrected while your program is still running.</span></span>

##### <a name="test-by-disconnecting-from-hello-network"></a><span data-ttu-id="59a45-145">Test odpojení od sítě hello</span><span class="sxs-lookup"><span data-stu-id="59a45-145">Test by disconnecting from hello network</span></span>
<span data-ttu-id="59a45-146">Jedním ze způsobů, které můžete testovat logika opakovaných pokusů je toodisconnect klientský počítač z hello sítě během programu hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-146">One way you can test your retry logic is toodisconnect your client computer from hello network while hello program is running.</span></span> <span data-ttu-id="59a45-147">Chyba Hello bude:</span><span class="sxs-lookup"><span data-stu-id="59a45-147">hello error will be:</span></span>

* <span data-ttu-id="59a45-148">**SqlException.Number** = 11001</span><span class="sxs-lookup"><span data-stu-id="59a45-148">**SqlException.Number** = 11001</span></span>
* <span data-ttu-id="59a45-149">Zpráva: "znám žádný takový hostitel je"</span><span class="sxs-lookup"><span data-stu-id="59a45-149">Message: "No such host is known"</span></span>

<span data-ttu-id="59a45-150">Jako součást hello nejprve opakujte pokus o, můžete vašeho programu opravte hello chybně a pokusíte se tooconnect.</span><span class="sxs-lookup"><span data-stu-id="59a45-150">As part of hello first retry attempt, your program can correct hello misspelling, and then attempt tooconnect.</span></span>

<span data-ttu-id="59a45-151">toomake tomto praktické, můžete před zahájením vašeho programu odpojit počítači ze sítě hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-151">toomake this practical, you unplug your computer from hello network before you start your program.</span></span> <span data-ttu-id="59a45-152">Potom váš program rozpozná běhu parametr, který způsobí, že programu hello:</span><span class="sxs-lookup"><span data-stu-id="59a45-152">Then your program recognizes a run time parameter that causes hello program to:</span></span>

1. <span data-ttu-id="59a45-153">Jako přechodný dočasně přidáte 11001 tooits seznam tooconsider chyby.</span><span class="sxs-lookup"><span data-stu-id="59a45-153">Temporarily add 11001 tooits list of errors tooconsider as transient.</span></span>
2. <span data-ttu-id="59a45-154">Pokus první připojení jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="59a45-154">Attempt its first connection as usual.</span></span>
3. <span data-ttu-id="59a45-155">Po chybě hello se zjistilo, odeberte ze seznamu hello 11001.</span><span class="sxs-lookup"><span data-stu-id="59a45-155">After hello error is caught, remove 11001 from hello list.</span></span>
4. <span data-ttu-id="59a45-156">Zobrazí se zpráva, hello uživatele tooplug hello počítače do sítě hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-156">Display a message telling hello user tooplug hello computer into hello network.</span></span>
   * <span data-ttu-id="59a45-157">Další pozastavení provádění pomocí buď hello **Console.ReadLine** metoda nebo dialogovém okně tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="59a45-157">Pause further execution by using either hello **Console.ReadLine** method or a dialog with an OK button.</span></span> <span data-ttu-id="59a45-158">uživatel Hello stisknutím klávesy Enter hello po hello počítače zapojen do sítě hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-158">hello user presses hello Enter key after hello computer plugged into hello network.</span></span>
5. <span data-ttu-id="59a45-159">Pokus znovu tooconnect, byla očekávána úspěch.</span><span class="sxs-lookup"><span data-stu-id="59a45-159">Attempt again tooconnect, expecting success.</span></span>

##### <a name="test-by-misspelling-hello-database-name-when-connecting"></a><span data-ttu-id="59a45-160">Název databáze hello chybně test při připojování</span><span class="sxs-lookup"><span data-stu-id="59a45-160">Test by misspelling hello database name when connecting</span></span>
<span data-ttu-id="59a45-161">Váš program můžete účelově chybně hello uživatelské jméno před hello první pokus o připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-161">Your program can purposely misspell hello user name before hello first connection attempt.</span></span> <span data-ttu-id="59a45-162">Chyba Hello bude:</span><span class="sxs-lookup"><span data-stu-id="59a45-162">hello error will be:</span></span>

* <span data-ttu-id="59a45-163">**SqlException.Number** = 18456</span><span class="sxs-lookup"><span data-stu-id="59a45-163">**SqlException.Number** = 18456</span></span>
* <span data-ttu-id="59a45-164">Zpráva: "přihlášení se nezdařilo pro uživatele 'WRONG_MyUserName'."</span><span class="sxs-lookup"><span data-stu-id="59a45-164">Message: "Login failed for user 'WRONG_MyUserName'."</span></span>

<span data-ttu-id="59a45-165">Jako součást hello nejprve opakujte pokus o, můžete vašeho programu opravte hello chybně a pokusíte se tooconnect.</span><span class="sxs-lookup"><span data-stu-id="59a45-165">As part of hello first retry attempt, your program can correct hello misspelling, and then attempt tooconnect.</span></span>

<span data-ttu-id="59a45-166">toomake tomto praktické, vaše aplikace může rozeznat běhu parametr, který způsobí, že programu hello:</span><span class="sxs-lookup"><span data-stu-id="59a45-166">toomake this practical, your program could recognize a run time parameter that causes hello program to:</span></span>

1. <span data-ttu-id="59a45-167">Jako přechodný dočasně přidáte 18456 tooits seznam tooconsider chyby.</span><span class="sxs-lookup"><span data-stu-id="59a45-167">Temporarily add 18456 tooits list of errors tooconsider as transient.</span></span>
2. <span data-ttu-id="59a45-168">Přidejte účelově 'WRONG_' toohello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="59a45-168">Purposely add 'WRONG_' toohello user name.</span></span>
3. <span data-ttu-id="59a45-169">Po chybě hello se zjistilo, odeberte ze seznamu hello 18456.</span><span class="sxs-lookup"><span data-stu-id="59a45-169">After hello error is caught, remove 18456 from hello list.</span></span>
4. <span data-ttu-id="59a45-170">Odebrání 'WRONG_' hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="59a45-170">Remove 'WRONG_' from hello user name.</span></span>
5. <span data-ttu-id="59a45-171">Pokus znovu tooconnect, byla očekávána úspěch.</span><span class="sxs-lookup"><span data-stu-id="59a45-171">Attempt again tooconnect, expecting success.</span></span>

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a><span data-ttu-id="59a45-172">Parametry objektu SqlConnection .NET pro opakování připojení</span><span class="sxs-lookup"><span data-stu-id="59a45-172">.NET SqlConnection parameters for connection retry</span></span>
<span data-ttu-id="59a45-173">Pokud váš klientský program připojuje tootooAzure SQL Database pomocí třídy rozhraní .NET Framework hello **System.Data.SqlClient.SqlConnection**, měli byste použít .NET 4.6.1 nebo novější (nebo .NET Core), můžete použít funkci opakování jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-173">If your client program connects tootooAzure SQL Database by using hello .NET Framework class **System.Data.SqlClient.SqlConnection**, you should use .NET 4.6.1 or later (or .NET Core) so you can leverage its connection retry feature.</span></span> <span data-ttu-id="59a45-174">Podrobnosti o hello funkce jsou [zde](http://go.microsoft.com/fwlink/?linkid=393996).</span><span class="sxs-lookup"><span data-stu-id="59a45-174">Details of hello feature are [here](http://go.microsoft.com/fwlink/?linkid=393996).</span></span>

<!--
2015-11-30, FwLink 393996 points toodn632678.aspx, which links tooa downloadable .docx related tooSqlClient and SQL Server 2014.
-->


<span data-ttu-id="59a45-175">Při vytváření hello [připojovací řetězec](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) pro vaše **SqlConnection** objektu, by měly koordinovat hello hodnoty mezi hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="59a45-175">When you build hello [connection string](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) for your **SqlConnection** object, you should coordinate hello values among hello following parameters:</span></span>

* <span data-ttu-id="59a45-176">ConnectRetryCount &nbsp; &nbsp; *(výchozí hodnota je 1. Rozsah je 0 až 255).*</span><span class="sxs-lookup"><span data-stu-id="59a45-176">ConnectRetryCount &nbsp;&nbsp;*(Default is 1. Range is 0 through 255.)*</span></span>
* <span data-ttu-id="59a45-177">ConnectRetryInterval &nbsp; &nbsp; *(výchozí hodnota je 1 sekunda. Rozsah je 1 až 60).*</span><span class="sxs-lookup"><span data-stu-id="59a45-177">ConnectRetryInterval &nbsp;&nbsp;*(Default is 1 second. Range is 1 through 60.)*</span></span>
* <span data-ttu-id="59a45-178">Časový limit připojení &nbsp; &nbsp; *(výchozí hodnota je 15 sekund. Rozsah hodnot je 0 až 2147483647)*</span><span class="sxs-lookup"><span data-stu-id="59a45-178">Connection Timeout &nbsp;&nbsp;*(Default is 15 seconds. Range is 0 through 2147483647)*</span></span>

<span data-ttu-id="59a45-179">Konkrétně vybrané hodnoty měli hello následující rovnosti true:</span><span class="sxs-lookup"><span data-stu-id="59a45-179">Specifically, your chosen values should make hello following equality true:</span></span>

* <span data-ttu-id="59a45-180">Časový limit připojení = ConnectRetryCount * ConnectionRetryInterval</span><span class="sxs-lookup"><span data-stu-id="59a45-180">Connection Timeout = ConnectRetryCount * ConnectionRetryInterval</span></span>

<span data-ttu-id="59a45-181">Například pokud hello count = 3 a interval = 10 sekund, vypršení časového limitu z jen 29 sekund nebude poměrně poskytnout hello systému dostatek času pro jeho 3. a finální opakování v připojení: 29 < 3 * 10.</span><span class="sxs-lookup"><span data-stu-id="59a45-181">For example, if hello count = 3, and interval = 10 seconds, a timeout of only 29 seconds would not quite give hello system enough time for its 3rd and final retry at connecting: 29 < 3 * 10.</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="59a45-182">Připojení a příkaz</span><span class="sxs-lookup"><span data-stu-id="59a45-182">Connection versus command</span></span>
<span data-ttu-id="59a45-183">Hello **ConnectRetryCount** a **ConnectRetryInterval** parametry umožní vaše **SqlConnection** objekt opakování hello operaci připojení bez informuje nebo bothering vaší program, jako je například vrácení řízení tooyour program.</span><span class="sxs-lookup"><span data-stu-id="59a45-183">hello **ConnectRetryCount** and **ConnectRetryInterval** parameters let your **SqlConnection** object retry hello connect operation without telling or bothering your program, such as returning control tooyour program.</span></span> <span data-ttu-id="59a45-184">opakování Hello se může objevit v následujících situacích hello:</span><span class="sxs-lookup"><span data-stu-id="59a45-184">hello retries can occur in hello following situations:</span></span>

* <span data-ttu-id="59a45-185">volání metody mySqlConnection.Open</span><span class="sxs-lookup"><span data-stu-id="59a45-185">mySqlConnection.Open method call</span></span>
* <span data-ttu-id="59a45-186">volání metody mySqlConnection.Execute</span><span class="sxs-lookup"><span data-stu-id="59a45-186">mySqlConnection.Execute method call</span></span>

<span data-ttu-id="59a45-187">Není k dispozici subtlety.</span><span class="sxs-lookup"><span data-stu-id="59a45-187">There is a subtlety.</span></span> <span data-ttu-id="59a45-188">Pokud dojde k přechodné chybě. při vaší *dotazu* je spouštěna, vaše **SqlConnection** objekt nemá hello opakovat operaci připojení a jeho určitě neopakuje dotazu.</span><span class="sxs-lookup"><span data-stu-id="59a45-188">If a transient error occurs while your *query* is being executed, your **SqlConnection** object does not retry hello connect operation, and it certainly does not retry your query.</span></span> <span data-ttu-id="59a45-189">Ale **SqlConnection** velmi rychle kontroly hello připojení před odesláním dotazu pro provedení.</span><span class="sxs-lookup"><span data-stu-id="59a45-189">However, **SqlConnection** very quickly checks hello connection before sending your query for execution.</span></span> <span data-ttu-id="59a45-190">Pokud hello Rychlá kontrola zjistí problému s připojením **SqlConnection** opakování hello operaci připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-190">If hello quick check detects a connection problem, **SqlConnection** retries hello connect operation.</span></span> <span data-ttu-id="59a45-191">Pokud hello opakování úspěšné, odešle dotaz je pro provedení.</span><span class="sxs-lookup"><span data-stu-id="59a45-191">If hello retry succeeds, you query is sent for execution.</span></span>

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a><span data-ttu-id="59a45-192">Měli ConnectRetryCount nelze kombinovat s logika opakovaných pokusů aplikace?</span><span class="sxs-lookup"><span data-stu-id="59a45-192">Should ConnectRetryCount be combined with application retry logic?</span></span>
<span data-ttu-id="59a45-193">Předpokládejme, že aplikace má robustní vlastní opakování logiku.</span><span class="sxs-lookup"><span data-stu-id="59a45-193">Suppose your application has robust custom retry logic.</span></span> <span data-ttu-id="59a45-194">Ho mohou zkuste hello operaci připojení. 4 časy.</span><span class="sxs-lookup"><span data-stu-id="59a45-194">It might retry hello connect operation 4 times.</span></span> <span data-ttu-id="59a45-195">Pokud přidáte **ConnectRetryInterval** a **ConnectRetryCount** = 3 tooyour připojovací řetězec, se zvýší too4 počet opakování hello * 3 = 12 opakování.</span><span class="sxs-lookup"><span data-stu-id="59a45-195">If you add **ConnectRetryInterval** and **ConnectRetryCount** =3 tooyour connection string, you will increase hello retry count too4 * 3 = 12 retries.</span></span> <span data-ttu-id="59a45-196">Hodláte nemusí takové velký počet opakování.</span><span class="sxs-lookup"><span data-stu-id="59a45-196">You might not intend such a high number of retries.</span></span>

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-tooazure-sql-database"></a><span data-ttu-id="59a45-197">TooAzure připojení databáze SQL</span><span class="sxs-lookup"><span data-stu-id="59a45-197">Connections tooAzure SQL Database</span></span>
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a><span data-ttu-id="59a45-198">Připojení: Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="59a45-198">Connection: Connection string</span></span>
<span data-ttu-id="59a45-199">Hello potřebné pro připojení tooAzure připojovací řetězec databáze SQL se mírně liší z hello řetězce pro připojení tooMicrosoft systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="59a45-199">hello connection string necessary for connecting tooAzure SQL Database is slightly different from hello string for connecting tooMicrosoft SQL Server.</span></span> <span data-ttu-id="59a45-200">Hello připojovací řetězec pro vaši databázi můžete zkopírovat z hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="59a45-200">You can copy hello connection string for your database from hello [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a><span data-ttu-id="59a45-201">Připojení: IP adresa</span><span class="sxs-lookup"><span data-stu-id="59a45-201">Connection: IP address</span></span>
<span data-ttu-id="59a45-202">Je nutné nakonfigurovat hello databáze SQL serveru tooaccept komunikaci z IP adresy hello hello počítače, který je hostitelem váš klientský program.</span><span class="sxs-lookup"><span data-stu-id="59a45-202">You must configure hello SQL Database server tooaccept communication from hello IP address of hello computer that hosts your client program.</span></span> <span data-ttu-id="59a45-203">To uděláte tak, že upravíte nastavení brány firewall hello prostřednictvím hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="59a45-203">You do this by editing hello firewall settings through hello [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="59a45-204">Pokud zapomenete tooconfigure hello IP adresu, program se nezdaří s užitečný chybovou zprávu, která stavy hello nezbytné IP adresu.</span><span class="sxs-lookup"><span data-stu-id="59a45-204">If you forget tooconfigure hello IP address, your program will fail with a handy error message that states hello necessary IP address.</span></span>

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

<span data-ttu-id="59a45-205">Další informace najdete v tématu: [postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="59a45-205">For more information, see: [How to: Configure firewall settings on SQL Database](sql-database-configure-firewall-settings.md)</span></span>

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a><span data-ttu-id="59a45-206">Připojení: porty</span><span class="sxs-lookup"><span data-stu-id="59a45-206">Connection: Ports</span></span>
<span data-ttu-id="59a45-207">Obvykle potřebujete jenom tooensure, že port 1433 je otevřený pro odchozí komunikaci, hello počítače, který je hostitelem program klienta.</span><span class="sxs-lookup"><span data-stu-id="59a45-207">Typically you only need tooensure that port 1433 is open for outbound communication, on hello computer that hosts you client program.</span></span>

<span data-ttu-id="59a45-208">Například pokud je váš klientský program hostované na počítači se systémem Windows, hello brány Windows Firewall na hostiteli hello vám umožní tooopen port 1433:</span><span class="sxs-lookup"><span data-stu-id="59a45-208">For example, when your client program is hosted on a Windows computer, hello Windows Firewall on hello host enables you tooopen port 1433:</span></span>

1. <span data-ttu-id="59a45-209">Otevřete ovládací panely hello</span><span class="sxs-lookup"><span data-stu-id="59a45-209">Open hello Control Panel</span></span>
2. <span data-ttu-id="59a45-210">&gt;Všechny položky Ovládacích panelů</span><span class="sxs-lookup"><span data-stu-id="59a45-210">&gt; All Control Panel Items</span></span>
3. <span data-ttu-id="59a45-211">&gt;Brána Windows Firewall</span><span class="sxs-lookup"><span data-stu-id="59a45-211">&gt; Windows Firewall</span></span>
4. <span data-ttu-id="59a45-212">&gt;Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="59a45-212">&gt; Advanced Settings</span></span>
5. <span data-ttu-id="59a45-213">&gt;Odchozí pravidla</span><span class="sxs-lookup"><span data-stu-id="59a45-213">&gt; Outbound Rules</span></span>
6. <span data-ttu-id="59a45-214">&gt;Akce</span><span class="sxs-lookup"><span data-stu-id="59a45-214">&gt; Actions</span></span>
7. <span data-ttu-id="59a45-215">&gt;Nové pravidlo</span><span class="sxs-lookup"><span data-stu-id="59a45-215">&gt; New Rule</span></span>

<span data-ttu-id="59a45-216">Pokud je váš klientský program hostované na virtuální počítač Azure (VM), měli byste si přečíst:</span><span class="sxs-lookup"><span data-stu-id="59a45-216">If your client program is hosted on an Azure virtual machine (VM), you should read:</span></span><br/><span data-ttu-id="59a45-217">[Porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="59a45-217">[Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

<span data-ttu-id="59a45-218">Základní informace o konfigurace portů a IP adresy, najdete v tématu: [brány firewall databáze Azure SQL Database](sql-database-firewall-configure.md)</span><span class="sxs-lookup"><span data-stu-id="59a45-218">For background information about cofiguration of ports and IP address, see: [Azure SQL Database firewall](sql-database-firewall-configure.md)</span></span>

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a><span data-ttu-id="59a45-219">Připojení: ADO.NET 4.6.1</span><span class="sxs-lookup"><span data-stu-id="59a45-219">Connection: ADO.NET 4.6.1</span></span>
<span data-ttu-id="59a45-220">Pokud váš program používá třídy ADO.NET jako **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL Database, doporučujeme vám použít rozhraní .NET Framework verze 4.6.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="59a45-220">If your program uses ADO.NET classes like **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL Database, we recommend that you use .NET Framework version 4.6.1 or higher.</span></span>

<span data-ttu-id="59a45-221">ADO.NET 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="59a45-221">ADO.NET 4.6.1:</span></span>

* <span data-ttu-id="59a45-222">Pro databázi SQL Azure, je lepší spolehlivostí při otevření připojení pomocí hello **SqlConnection.Open** metoda.</span><span class="sxs-lookup"><span data-stu-id="59a45-222">For Azure SQL Database, there is improved reliability when you open a connection by using hello **SqlConnection.Open** method.</span></span> <span data-ttu-id="59a45-223">Hello **otevřete** metoda nyní zahrnuje nejlepší mechanismy úsilí opakování v odpovědi tootransient chyb, některé chyby v době vymezené připojení hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-223">hello **Open** method now incorporates best effort retry mechanisms in response tootransient faults, for certain errors within hello Connection Timeout period.</span></span>
* <span data-ttu-id="59a45-224">Podporuje sdružování připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-224">Supports connection pooling.</span></span> <span data-ttu-id="59a45-225">To zahrnuje efektivní ověření, který hello připojení funguje objekt nabízí vašeho programu.</span><span class="sxs-lookup"><span data-stu-id="59a45-225">This includes an efficient verification that hello connection object it gives your program is functioning.</span></span>

<span data-ttu-id="59a45-226">Použijete-li objekt připojení z fondu připojení, doporučujeme vám, že váš program dočasně uzavřít hello připojení, když není ihned používat.</span><span class="sxs-lookup"><span data-stu-id="59a45-226">When you use a connection object from a connection pool, we recommend that your program temporarily close hello connection when not immediately using it.</span></span> <span data-ttu-id="59a45-227">Opakovaném otevření připojení není nákladné je hello způsob vytvoření nového připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-227">Re-opening a connection is not expensive hello way creating a new connection is.</span></span>

<span data-ttu-id="59a45-228">Pokud používáte ADO.NET 4.0 nebo starší, doporučujeme upgradu toohello nejnovější ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="59a45-228">If you are using ADO.NET 4.0 or earlier, we recommend that you upgrade toohello latest ADO.NET.</span></span>

* <span data-ttu-id="59a45-229">Od listopadu 2015, můžete [stáhnout ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a45-229">As of November 2015, you can [download ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span></span>

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a><span data-ttu-id="59a45-230">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="59a45-230">Diagnostics</span></span>
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a><span data-ttu-id="59a45-231">Diagnostika: Test, zda může připojit nástroje</span><span class="sxs-lookup"><span data-stu-id="59a45-231">Diagnostics: Test whether utilities can connect</span></span>
<span data-ttu-id="59a45-232">Pokud váš program selhává tooconnect tooAzure SQL Database, jeden diagnostiky možnost je tootry tooconnect s programem nástroj.</span><span class="sxs-lookup"><span data-stu-id="59a45-232">If your program is failing tooconnect tooAzure SQL Database, one diagnostic option is tootry tooconnect with a utility program.</span></span> <span data-ttu-id="59a45-233">V ideálním případě by nástroj hello připojit pomocí hello stejnou knihovnu, který program používá.</span><span class="sxs-lookup"><span data-stu-id="59a45-233">Ideally hello utility would connect by using hello same library that your program uses.</span></span>

<span data-ttu-id="59a45-234">Na libovolném počítači Windows můžete vyzkoušet tyto nástroje:</span><span class="sxs-lookup"><span data-stu-id="59a45-234">On any Windows computer, you can try these utilities:</span></span>

* <span data-ttu-id="59a45-235">SQL Server Management Studio (ssms.exe), který se připojuje pomocí ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="59a45-235">SQL Server Management Studio (ssms.exe), which connects by using ADO.NET.</span></span>
* <span data-ttu-id="59a45-236">Sqlcmd.exe, který se připojuje pomocí [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a45-236">sqlcmd.exe, which connects by using [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span></span>

<span data-ttu-id="59a45-237">Po připojení otestujte, zda funguje krátké SQL SELECT dotazu.</span><span class="sxs-lookup"><span data-stu-id="59a45-237">Once connected, test whether a short SQL SELECT query works.</span></span>

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-hello-open-ports"></a><span data-ttu-id="59a45-238">Diagnostika: Zkontrolujte otevřené porty hello</span><span class="sxs-lookup"><span data-stu-id="59a45-238">Diagnostics: Check hello open ports</span></span>
<span data-ttu-id="59a45-239">Předpokládejme, že máte podezření, že pokusy o připojení se nedaří kvůli tooport problémy.</span><span class="sxs-lookup"><span data-stu-id="59a45-239">Suppose you suspect that connection attempts are failing due tooport issues.</span></span> <span data-ttu-id="59a45-240">V počítači můžete spustit nástroj, který hlásí hello konfiguraci portů.</span><span class="sxs-lookup"><span data-stu-id="59a45-240">On your computer you can run a utility that reports on hello port configurations.</span></span>

<span data-ttu-id="59a45-241">V systému Linux hello může být užitečné následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="59a45-241">On Linux hello following utilities might be helpful:</span></span>

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * <span data-ttu-id="59a45-242">(Změnit hello příklad hodnota toobe IP adresu.)</span><span class="sxs-lookup"><span data-stu-id="59a45-242">(Change hello example value toobe your IP address.)</span></span>

<span data-ttu-id="59a45-243">Na Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) nástroj může být užitečné.</span><span class="sxs-lookup"><span data-stu-id="59a45-243">On Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utility might be helpful.</span></span> <span data-ttu-id="59a45-244">Tady je příklad spuštění, dotaz hello situace port na serveru Azure SQL Database a která byla spuštěna na přenosný počítač:</span><span class="sxs-lookup"><span data-stu-id="59a45-244">Here is an example execution that queried hello port situation on an Azure SQL Database server, and which was run on a laptop computer:</span></span>

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting tooresolve name tooIP address...
Name resolved too23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a><span data-ttu-id="59a45-245">Diagnostiky: Chyby protokolu</span><span class="sxs-lookup"><span data-stu-id="59a45-245">Diagnostics: Log your errors</span></span>
<span data-ttu-id="59a45-246">Občasný problém je někdy nejlépe zjištěném detekce obecné vzoru přes dny nebo týdny.</span><span class="sxs-lookup"><span data-stu-id="59a45-246">An intermittent problem is sometimes best diagnosed by detection of a general pattern over days or weeks.</span></span>

<span data-ttu-id="59a45-247">Váš klient může být užitečné při diagnostiku protokolováním všechny chyby, který nalezne.</span><span class="sxs-lookup"><span data-stu-id="59a45-247">Your client can assist in a diagnosis by logging all errors it encounters.</span></span> <span data-ttu-id="59a45-248">Je možné toocorrelate hello položky protokolu s daty chyba, která interně Azure SQL Database samotné protokoly.</span><span class="sxs-lookup"><span data-stu-id="59a45-248">You might be able toocorrelate hello log entries with error data that Azure SQL Database logs itself internally.</span></span>

<span data-ttu-id="59a45-249">Enterprise knihovny 6 (EntLib60) nabízí spravované rozhraní .NET třídy tooassist s protokolováním:</span><span class="sxs-lookup"><span data-stu-id="59a45-249">Enterprise Library 6 (EntLib60) offers .NET managed classes tooassist with logging:</span></span>

* [<span data-ttu-id="59a45-250">5 - jako snadno jako návratem vypnout protokolu: pomocí hello blokem protokolování aplikace</span><span class="sxs-lookup"><span data-stu-id="59a45-250">5 - As Easy As Falling Off a Log: Using hello Logging Application Block</span></span>](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a><span data-ttu-id="59a45-251">Diagnostika: Zkontrolujte protokoly systému chyby</span><span class="sxs-lookup"><span data-stu-id="59a45-251">Diagnostics: Examine system logs for errors</span></span>
<span data-ttu-id="59a45-252">Tady jsou některé příkazy jazyka Transact-SQL, vyberte tento dotaz protokoly chyby a dalšími informacemi.</span><span class="sxs-lookup"><span data-stu-id="59a45-252">Here are some Transact-SQL SELECT statements that query logs of error and other information.</span></span>

| <span data-ttu-id="59a45-253">Dotaz protokolu</span><span class="sxs-lookup"><span data-stu-id="59a45-253">Query of log</span></span> | <span data-ttu-id="59a45-254">Popis</span><span class="sxs-lookup"><span data-stu-id="59a45-254">Description</span></span> |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |<span data-ttu-id="59a45-255">Hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) zobrazení nabízí informace o jednotlivých událostech, včetně některých, která mohou způsobit přechodné chyby nebo chyby připojení.</span><span class="sxs-lookup"><span data-stu-id="59a45-255">hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) view offers information about individual events, including some that can cause transient errors or connectivity failures.</span></span><br/><br/><span data-ttu-id="59a45-256">V ideálním případě mohou korelovat hello **start_time** nebo **end_time** hodnoty s informacemi o když váš klientský program došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="59a45-256">Ideally you can correlate hello **start_time** or **end_time** values with information about when your client program experienced problems.</span></span><br/><br/><span data-ttu-id="59a45-257">**TIP:** je nutné připojit toohello **hlavní** databáze toorun to.</span><span class="sxs-lookup"><span data-stu-id="59a45-257">**TIP:** You must connect toohello **master** database toorun this.</span></span> |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |<span data-ttu-id="59a45-258">Hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) zobrazení nabízí souhrnné počty typů událostí pro další diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="59a45-258">hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) view offers aggregated counts of event types, for additional diagnostics.</span></span><br/><br/><span data-ttu-id="59a45-259">**TIP:** je nutné připojit toohello **hlavní** databáze toorun to.</span><span class="sxs-lookup"><span data-stu-id="59a45-259">**TIP:** You must connect toohello **master** database toorun this.</span></span> |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-hello-sql-database-log"></a><span data-ttu-id="59a45-260">Diagnostika: Vyhledejte problém události v protokolu databáze SQL hello</span><span class="sxs-lookup"><span data-stu-id="59a45-260">Diagnostics: Search for problem events in hello SQL Database log</span></span>
<span data-ttu-id="59a45-261">Můžete vyhledat záznamy o událostech problém v hello protokolu databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="59a45-261">You can search for entries about problem events in hello log of Azure SQL Database.</span></span> <span data-ttu-id="59a45-262">Zkuste hello následující příkaz Transact-SQL, vyberte v hello **hlavní** databáze:</span><span class="sxs-lookup"><span data-stu-id="59a45-262">Try hello following Transact-SQL SELECT statement in hello **master** database:</span></span>

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


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a><span data-ttu-id="59a45-263">Několik řádků vrácená z sys.fn_xe_telemetry_blob_target_read_file</span><span class="sxs-lookup"><span data-stu-id="59a45-263">A few returned rows from sys.fn_xe_telemetry_blob_target_read_file</span></span>
<span data-ttu-id="59a45-264">Dále je, jak může vypadat vrácený řádek.</span><span class="sxs-lookup"><span data-stu-id="59a45-264">Next is what a returned row might look like.</span></span> <span data-ttu-id="59a45-265">hodnoty null Hello vidět nejsou často null v dalších řádcích.</span><span class="sxs-lookup"><span data-stu-id="59a45-265">hello null values shown are often not null in other rows.</span></span>

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a><span data-ttu-id="59a45-266">Knihovna Enterprise 6</span><span class="sxs-lookup"><span data-stu-id="59a45-266">Enterprise Library 6</span></span>
<span data-ttu-id="59a45-267">Enterprise knihovny 6 (EntLib60) je architektura tříd rozhraní .NET usnadňující implementovat robustní klienti cloudové služby, z nichž jeden je služba Azure SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="59a45-267">Enterprise Library 6 (EntLib60) is a framework of .NET classes that helps you implement robust clients of cloud services, one of which is hello Azure SQL Database service.</span></span> <span data-ttu-id="59a45-268">Můžete vyhledat témata vyhrazené tooeach oblasti, ve kterém můžete EntLib60 pomáhat při první navštivte stránky:</span><span class="sxs-lookup"><span data-stu-id="59a45-268">You can locate topics dedicated tooeach area in which EntLib60 can assist by first visiting:</span></span>

* [<span data-ttu-id="59a45-269">Knihovna Enterprise 6 – duben 2013</span><span class="sxs-lookup"><span data-stu-id="59a45-269">Enterprise Library 6 - April 2013</span></span>](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

<span data-ttu-id="59a45-270">Logika opakovaných pokusů pro přechodné chyby zpracování je jedné oblasti, ve kterém může být užitečné EntLib60:</span><span class="sxs-lookup"><span data-stu-id="59a45-270">Retry logic for handling transient errors is one area in which EntLib60 can assist:</span></span>

* [<span data-ttu-id="59a45-271">4 - perseverance, tajný klíč všechny vítězství: pomocí hello přechodné chyby zpracování blokování aplikací</span><span class="sxs-lookup"><span data-stu-id="59a45-271">4 - Perseverance, Secret of All Triumphs: Using hello Transient Fault Handling Application Block</span></span>](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> <span data-ttu-id="59a45-272">Hello zdrojový kód EntLib60 je k dispozici pro veřejné [Stáhnout](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span><span class="sxs-lookup"><span data-stu-id="59a45-272">hello source code for EntLib60 is available for public [download](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span></span> <span data-ttu-id="59a45-273">Společnost Microsoft nemá žádné plány toomake další funkce aktualizace nebo údržby aktualizací tooEntLib.</span><span class="sxs-lookup"><span data-stu-id="59a45-273">Microsoft has no plans toomake further feature updates or maintenance updates tooEntLib.</span></span>
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a><span data-ttu-id="59a45-274">Třídy EntLib60 pro přechodné chyby a zkuste to znovu</span><span class="sxs-lookup"><span data-stu-id="59a45-274">EntLib60 classes for transient errors and retry</span></span>
<span data-ttu-id="59a45-275">Následující třídy EntLib60 Hello jsou obzvláště užitečná pro logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="59a45-275">hello following EntLib60 classes are particularly useful for retry logic.</span></span> <span data-ttu-id="59a45-276">Všechny tyto v, nebo další pod, hello obor názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span><span class="sxs-lookup"><span data-stu-id="59a45-276">All these  are in, or are further under, hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span></span>

<span data-ttu-id="59a45-277">*V oboru názvů hello **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span><span class="sxs-lookup"><span data-stu-id="59a45-277">*In hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span></span>

* <span data-ttu-id="59a45-278">**RetryPolicy** – třída</span><span class="sxs-lookup"><span data-stu-id="59a45-278">**RetryPolicy** class</span></span>
  
  * <span data-ttu-id="59a45-279">**ExecuteAction** – metoda</span><span class="sxs-lookup"><span data-stu-id="59a45-279">**ExecuteAction** method</span></span>
* <span data-ttu-id="59a45-280">**ExponentialBackoff** – třída</span><span class="sxs-lookup"><span data-stu-id="59a45-280">**ExponentialBackoff** class</span></span>
* <span data-ttu-id="59a45-281">**SqlDatabaseTransientErrorDetectionStrategy** – třída</span><span class="sxs-lookup"><span data-stu-id="59a45-281">**SqlDatabaseTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="59a45-282">**ReliableSqlConnection** – třída</span><span class="sxs-lookup"><span data-stu-id="59a45-282">**ReliableSqlConnection** class</span></span>
  
  * <span data-ttu-id="59a45-283">**Parametr ExecuteCommand** – metoda</span><span class="sxs-lookup"><span data-stu-id="59a45-283">**ExecuteCommand** method</span></span>

<span data-ttu-id="59a45-284">V oboru názvů hello **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span><span class="sxs-lookup"><span data-stu-id="59a45-284">In hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span></span>

* <span data-ttu-id="59a45-285">**AlwaysTransientErrorDetectionStrategy** – třída</span><span class="sxs-lookup"><span data-stu-id="59a45-285">**AlwaysTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="59a45-286">**NeverTransientErrorDetectionStrategy** – třída</span><span class="sxs-lookup"><span data-stu-id="59a45-286">**NeverTransientErrorDetectionStrategy** class</span></span>

<span data-ttu-id="59a45-287">Tady jsou odkazy tooinformation o EntLib60:</span><span class="sxs-lookup"><span data-stu-id="59a45-287">Here are links tooinformation about EntLib60:</span></span>

* <span data-ttu-id="59a45-288">Volné [stáhnout adresáře: pro vývojáře průvodce tooMicrosoft knihovny Enterprise Edition 2.](http://www.microsoft.com/download/details.aspx?id=41145)</span><span class="sxs-lookup"><span data-stu-id="59a45-288">Free [Book Download: Developer's Guide tooMicrosoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span></span>
* <span data-ttu-id="59a45-289">Osvědčené postupy: [opakujte obecné pokyny](../best-practices-retry-general.md) má vynikající podrobné informace o logika opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="59a45-289">Best practices: [Retry general guidance](../best-practices-retry-general.md) has an excellent in-depth discussion of retry logic.</span></span>
* <span data-ttu-id="59a45-290">Stahování NuGet [Enterprise Library - přechodných chyb aplikace bloku 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span><span class="sxs-lookup"><span data-stu-id="59a45-290">NuGet download of [Enterprise Library - Transient Fault Handling application block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span></span>

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-hello-logging-block"></a><span data-ttu-id="59a45-291">EntLib60: blok protokolování hello</span><span class="sxs-lookup"><span data-stu-id="59a45-291">EntLib60: hello logging block</span></span>
* <span data-ttu-id="59a45-292">blok protokolování Hello je velmi flexibilní a konfigurovat řešení, které vám umožní:</span><span class="sxs-lookup"><span data-stu-id="59a45-292">hello Logging block is a highly flexible and configurable solution that allows you to:</span></span>
  
  * <span data-ttu-id="59a45-293">Vytvoření a uložení zprávy protokolu v celé řadě umístění.</span><span class="sxs-lookup"><span data-stu-id="59a45-293">Create and store log messages in a wide variety of locations.</span></span>
  * <span data-ttu-id="59a45-294">Kategorizaci a filtrování zprávy.</span><span class="sxs-lookup"><span data-stu-id="59a45-294">Categorize and filter messages.</span></span>
  * <span data-ttu-id="59a45-295">Shromážděte kontextové informace, které jsou užitečné pro ladění a trasování, a také pro auditování a obecné protokolování požadavky.</span><span class="sxs-lookup"><span data-stu-id="59a45-295">Collect contextual information that is useful for debugging and tracing, as well as for auditing and general logging requirements.</span></span>
* <span data-ttu-id="59a45-296">hello přehledů bloku protokolování Hello protokolování funkce z cíl protokolu hello tak, aby kód aplikace hello konzistentní bez ohledu na umístění hello a typ hello cíl protokolování úložiště.</span><span class="sxs-lookup"><span data-stu-id="59a45-296">hello Logging block abstracts hello logging functionality from hello log destination so that hello application code is consistent, irrespective of hello location and type of hello target logging store.</span></span>

<span data-ttu-id="59a45-297">Podrobnosti najdete v tématu: [5 – jako snadno jako návratem vypnout protokolu: pomocí hello blokem protokolování aplikace](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="59a45-297">For details see: [5 - As Easy As Falling Off a Log: Using hello Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span></span>

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a><span data-ttu-id="59a45-298">EntLib60 IsTransient metoda zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="59a45-298">EntLib60 IsTransient method source code</span></span>
<span data-ttu-id="59a45-299">Další z hello **SqlDatabaseTransientErrorDetectionStrategy** třídy, je hello C# zdrojového kódu pro hello **IsTransient** metoda.</span><span class="sxs-lookup"><span data-stu-id="59a45-299">Next, from hello **SqlDatabaseTransientErrorDetectionStrategy** class, is hello C# source code for hello **IsTransient** method.</span></span> <span data-ttu-id="59a45-300">zdrojový kód Hello vysvětluje, které chyby považovány za toobe přechodná a worthy opakování od dubna 2013.</span><span class="sxs-lookup"><span data-stu-id="59a45-300">hello source code clarifies which errors were considered toobe transient and worthy of retry, as of April 2013.</span></span>

<span data-ttu-id="59a45-301">Řada **//comment** řádky byly odebrány z této kopie tooemphasize čitelnost.</span><span class="sxs-lookup"><span data-stu-id="59a45-301">Numerous **//comment** lines have been removed from this copy tooemphasize readability.</span></span>

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in hello exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // hello service is currently busy. Retry hello request after 10 seconds.
            // Code: (reason code toobe decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode hello reason code from hello error message to
            // determine hello grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach hello decoded values as additional attributes to
            // hello original SQL exception.
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
            // hello instance of SQL Server you attempted tooconnect to
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


## <a name="next-steps"></a><span data-ttu-id="59a45-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59a45-302">Next steps</span></span>
* <span data-ttu-id="59a45-303">Řešení potíží s další běžné problémy s připojením databáze SQL Azure, najdete v článku [připojení řešení problémů tooAzure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span><span class="sxs-lookup"><span data-stu-id="59a45-303">For troubleshooting other common Azure SQL Database connection issues, visit [Troubleshoot connection issues tooAzure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span></span>
* [<span data-ttu-id="59a45-304">Připojení k serveru SQL sdružování (ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="59a45-304">SQL Server Connection Pooling (ADO.NET)</span></span>](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [<span data-ttu-id="59a45-305">*Opakováním* je Apache 2.0 licenci pro obecné účely, opakování knihovny, které jsou napsané v **Python**, toosimplify hello úkolu přidávání toojust chování opakování o něco.</span><span class="sxs-lookup"><span data-stu-id="59a45-305">*Retrying* is an Apache 2.0 licensed general-purpose retrying library, written in **Python**, toosimplify hello task of adding retry behavior toojust about anything.</span></span>](https://pypi.python.org/pypi/retrying)

