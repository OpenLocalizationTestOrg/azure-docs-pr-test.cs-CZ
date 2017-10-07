---
title: "aaaManage databází v Azure SQL Data Warehouse | Microsoft Docs"
description: "Přehled správy databází SQL Data Warehouse. Zahrnuje nástroje pro správu, Dwu a Škálováním na více systémů, řešení potíží s výkon dotazů, vytvoření zásad zabezpečení a obnovení databáze z poškození dat nebo z místní výpadku výkonu."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: caec6572c4ab395107c3b095adc69a53eae8bd88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a><span data-ttu-id="82a89-104">Spravovat databáze v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="82a89-104">Manage databases in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="82a89-105">SQL Data Warehouse automatizuje mnoho aspektů správy vašich databází.</span><span class="sxs-lookup"><span data-stu-id="82a89-105">SQL Data Warehouse automates many aspects of managing your databases.</span></span> <span data-ttu-id="82a89-106">Například tooscale výkonu stačí tooadjust a platit hello přímo úroveň výpočetní prostředky a potom nechat SQL Data Warehouse práci všechny hello škálování a škálování zpět.</span><span class="sxs-lookup"><span data-stu-id="82a89-106">For example, tooscale performance you only need tooadjust and pay for hello right level of compute resources, and then let SQL Data Warehouse do all hello work of scaling out and scaling back.</span></span>

<span data-ttu-id="82a89-107">Nepochybně můžete toomonitor vaše úlohy tooidentify výkon musí a také vyřešit dlouho běžící dotazy.</span><span class="sxs-lookup"><span data-stu-id="82a89-107">You will undoubtedly want toomonitor your workload tooidentify your performance needs as well as troubleshoot long-running queries.</span></span> <span data-ttu-id="82a89-108">Budete také potřebovat tooperform několik zabezpečení úlohy toomanage oprávnění pro uživatele a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="82a89-108">You will also need tooperform a few security tasks toomanage permissions for users and logins.</span></span>

<span data-ttu-id="82a89-109">Tento přehled popisuje tyto aspekty správy SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82a89-109">This overview covers these aspects of managing SQL Data Warehouse.</span></span>

* <span data-ttu-id="82a89-110">Nástroje pro správu</span><span class="sxs-lookup"><span data-stu-id="82a89-110">Management tools</span></span>
* <span data-ttu-id="82a89-111">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="82a89-111">Scale Compute</span></span>
* <span data-ttu-id="82a89-112">Pozastavení a obnovení</span><span class="sxs-lookup"><span data-stu-id="82a89-112">Pause and Resume</span></span>
* <span data-ttu-id="82a89-113">Osvědčené postupy z hlediska výkonu</span><span class="sxs-lookup"><span data-stu-id="82a89-113">Performance Best Practices</span></span>
* <span data-ttu-id="82a89-114">Monitorování dotazu</span><span class="sxs-lookup"><span data-stu-id="82a89-114">Query Monitoring</span></span>
* <span data-ttu-id="82a89-115">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="82a89-115">Security</span></span>
* <span data-ttu-id="82a89-116">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="82a89-116">Backup and restore</span></span>

## <a name="management-tools"></a><span data-ttu-id="82a89-117">Nástroje pro správu</span><span class="sxs-lookup"><span data-stu-id="82a89-117">Management tools</span></span>
<span data-ttu-id="82a89-118">Můžete použít celou řadu nástrojů toomanage databází v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82a89-118">You can use a variety of tools toomanage databases in SQL Data Warehouse.</span></span> <span data-ttu-id="82a89-119">Jak budete spravovat databáze, budete vyvíjet předvoleb nástroje pro každý typ úlohy je nutné tooperform.</span><span class="sxs-lookup"><span data-stu-id="82a89-119">As you manage databases, you will develop tool preferences for each type of task you need tooperform.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="82a89-120">portál Azure</span><span class="sxs-lookup"><span data-stu-id="82a89-120">Azure portal</span></span>
<span data-ttu-id="82a89-121">Hello [portál Azure] [ Azure portal] je webový portál, kde můžete vytvářet, aktualizovat a odstranit databáze a monitorovat prostředky v databázi.</span><span class="sxs-lookup"><span data-stu-id="82a89-121">hello [Azure portal][Azure portal] is a web-based portal where you can create, update, and delete databases and monitor database resources.</span></span> <span data-ttu-id="82a89-122">Tento nástroj je skvělým je, pokud jste se právě Začínáme s Azure, Správa malý počet databází datového skladu, nebo třeba tooquickly něco udělat.</span><span class="sxs-lookup"><span data-stu-id="82a89-122">This tool is great is if you're just getting started with Azure, managing a small number of data warehouse databases, or need tooquickly do something.</span></span>

<span data-ttu-id="82a89-123">tooget začít s hello portál Azure, najdete v části [vytvoření SQL Data Warehouse (portál Azure)][Create a SQL Data Warehouse (Azure portal)].</span><span class="sxs-lookup"><span data-stu-id="82a89-123">tooget started with hello Azure portal, see [Create a SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span></span>

### <a name="sql-server-data-tools-in-visual-studio"></a><span data-ttu-id="82a89-124">Nástroje SQL Server Data Tools v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82a89-124">SQL Server Data Tools in Visual Studio</span></span>
<span data-ttu-id="82a89-125">[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) v sadě Visual Studio můžete tooconnect pro správu a vývoj vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="82a89-125">[SQL Server Data Tools][SQL Server Data Tools] (SSDT) in Visual Studio allows you tooconnect to, manage, and develop your databases.</span></span> <span data-ttu-id="82a89-126">Pokud jste vývojář aplikací obeznámeni s Visual Studio nebo jiné integrované vývojové prostředí (integrovaného vývojového prostředí), zkuste použít rozšíření SSDT v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82a89-126">If you're an application developer familiar with Visual Studio or other integrated development environments (IDEs), try using SSDT in Visual Studio.</span></span>

<span data-ttu-id="82a89-127">Rozšíření SSDT zahrnuje hello Průzkumník objektů SQL serveru, což vám umožní toovisualize, připojit a spouštět skripty pro databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82a89-127">SSDT includes hello SQL Server Object Explorer which enables you toovisualize, connect, and execute scripts against SQL Data Warehouse databases.</span></span> <span data-ttu-id="82a89-128">tooquickly připojit tooSQL datového skladu, jednoduše klikněte na hello **otevřete v sadě Visual Studio** tlačítka na panelu příkazů hello při zobrazení podrobností databáze hello hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="82a89-128">tooquickly connect tooSQL Data Warehouse, you can simply click hello **Open in Visual Studio** button in hello command bar when viewing hello database details in hello Azure Classic Portal.</span></span>  

<span data-ttu-id="82a89-129">tooget spuštění s rozšířením SSDT v sadě Visual Studio, najdete v části [dotazu Azure SQL Data Warehouse pomocí sady Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="82a89-129">tooget started with SSDT in Visual Studio, see [Query Azure SQL Data Warehouse with Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span></span>

### <a name="command-line-tools"></a><span data-ttu-id="82a89-130">Nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="82a89-130">Command-line tools</span></span>
<span data-ttu-id="82a89-131">Nástroje příkazového řádku jsou ideální pro automatizaci vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="82a89-131">Command line tools are ideal for automating your workloads.</span></span>  <span data-ttu-id="82a89-132">Prostředí PowerShell a sqlcmd jsou dva způsoby skvělé tooautomate procesů.</span><span class="sxs-lookup"><span data-stu-id="82a89-132">PowerShell and sqlcmd are two great ways tooautomate your processes.</span></span>  <span data-ttu-id="82a89-133">Doporučujeme, abyste tyto nástroje pro správu velkého počtu logických serverů a nasazení prostředků změny v produkčním prostředí, jako hello úlohy nezbytné jde skriptování a potom automatizovat.</span><span class="sxs-lookup"><span data-stu-id="82a89-133">We recommend these tools for managing a large number of logical servers and deploying resource changes in a production environment as hello tasks necessary can be scripted and then automated.</span></span>

### <a name="dynamic-management-views"></a><span data-ttu-id="82a89-134">Zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="82a89-134">Dynamic management views</span></span>
<span data-ttu-id="82a89-135">Zobrazení dynamické správy jsou hello chléb a másla správy SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82a89-135">DMVs are hello bread and butter of managing SQL Data Warehouse.</span></span> <span data-ttu-id="82a89-136">Téměř všechny informace, které poskytuje portálu hello spoléhá na zobrazení dynamické správy.</span><span class="sxs-lookup"><span data-stu-id="82a89-136">Almost all information that surfaces in hello portal relies on DMVs.</span></span> <span data-ttu-id="82a89-137">toosee seznam SQL Data Warehouse zobrazení dynamické správy, najdete v části [zobrazení systému SQL Data Warehouse][SQL Data Warehouse system views].</span><span class="sxs-lookup"><span data-stu-id="82a89-137">toosee a list of SQL Data Warehouse DMVs, see [SQL Data Warehouse system views][SQL Data Warehouse system views].</span></span>

<span data-ttu-id="82a89-138">tooget začít, najdete v části [připojit a zadávat dotazy pomocí sqlcmd][Connect and query with sqlcmd], a [vytvoření databáze (PowerShell)][Create a database (PowerShell)].</span><span class="sxs-lookup"><span data-stu-id="82a89-138">tooget started, see [Connect and query with sqlcmd][Connect and query with sqlcmd], and [Create a database (PowerShell)][Create a database (PowerShell)].</span></span>

## <a name="scale-compute"></a><span data-ttu-id="82a89-139">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="82a89-139">Scale compute</span></span>
<span data-ttu-id="82a89-140">V SQL Data Warehouse můžete rychle škálovat výkonu out nebo zpět zvýšením nebo snížením výpočetní prostředky procesoru, paměti a šířky pásma vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="82a89-140">In SQL Data Warehouse, you can quickly scale performance out or back by increasing or decreasing compute resources of CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="82a89-141">výkon tooscale, stačí toodo je upravit hello počet že SQL Data Warehouse přiděluje tooyour databáze jednotky datového skladu (Dwu).</span><span class="sxs-lookup"><span data-stu-id="82a89-141">tooscale performance, all you need toodo is adjust hello number of data warehouse units (DWUs) that SQL Data Warehouse allocates tooyour database.</span></span> <span data-ttu-id="82a89-142">SQL Data Warehouse rychle vytvoří hello změnit a zpracovává všechny změny toohardware základní hello nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="82a89-142">SQL Data Warehouse quickly makes hello change and handles all hello underlying changes toohardware or software.</span></span>

<span data-ttu-id="82a89-143">toolearn Další informace o škálování Dwu, najdete v části [škálování výkonu].</span><span class="sxs-lookup"><span data-stu-id="82a89-143">toolearn more about scaling DWUs, see [Scale performance].</span></span>

## <a name="pause-and-resume"></a><span data-ttu-id="82a89-144">Pozastavení a obnovení</span><span class="sxs-lookup"><span data-stu-id="82a89-144">Pause and resume</span></span>
<span data-ttu-id="82a89-145">toosave náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="82a89-145">toosave costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="82a89-146">Například pokud nebudete používat hello databáze během noční hello a o víkendech, můžete pozastavit tyto v době a obnovit během dne hello.</span><span class="sxs-lookup"><span data-stu-id="82a89-146">For example, if you won't be using hello database during hello night and on weekends, you can pause it during those times, and resume it during hello day.</span></span> <span data-ttu-id="82a89-147">Vám nebude nic účtováno pro Dwu při hello databáze byla pozastavena.</span><span class="sxs-lookup"><span data-stu-id="82a89-147">You won't be charged for DWUs while hello database is paused.</span></span>

<span data-ttu-id="82a89-148">Další informace najdete v tématu [pozastavit výpočetní][Pause compute], a [obnovit výpočty][Resume compute].</span><span class="sxs-lookup"><span data-stu-id="82a89-148">For more information, see [Pause compute][Pause compute], and [Resume compute][Resume compute].</span></span>

## <a name="performance-best-practices"></a><span data-ttu-id="82a89-149">Osvědčené postupy z hlediska výkonu</span><span class="sxs-lookup"><span data-stu-id="82a89-149">Performance Best Practices</span></span>
<span data-ttu-id="82a89-150">Při Začínáme s novou technologií, zjišťování hello tipy a triky, které fungují nejlepší vpravo od začátku hello můžete ušetřit spoustu času.</span><span class="sxs-lookup"><span data-stu-id="82a89-150">When getting started with a new technology, discovering hello tips and tricks that work best right from hello start can save you lots of time.</span></span>  <span data-ttu-id="82a89-151">Osvědčené postupy v rámci řadu témata s našimi zjistíte.</span><span class="sxs-lookup"><span data-stu-id="82a89-151">You will find best practices throughout many of our topics.</span></span>

<span data-ttu-id="82a89-152">toosee mnoho souhrn hello nejdůležitější aspekty při vývoji úlohy, najdete v části [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="82a89-152">toosee many a summary of hello most important considerations when developing your workload, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

## <a name="query-monitoring"></a><span data-ttu-id="82a89-153">Monitorování dotazu</span><span class="sxs-lookup"><span data-stu-id="82a89-153">Query Monitoring</span></span>
<span data-ttu-id="82a89-154">Někdy dotazu běží příliš dlouho, ale nejsou opravdu který z nich je který hello.</span><span class="sxs-lookup"><span data-stu-id="82a89-154">Sometimes a query is running too long, but you aren't sure of which one is hello culprit.</span></span> <span data-ttu-id="82a89-155">SQL Data Warehouse má zobrazení dynamické správy (zobrazení dynamické správy), které můžete použít toofigure limitu dotazu, který trvá příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="82a89-155">SQL Data Warehouse has dynamic management views (DMVs) that you can use toofigure out which query is taking too long.</span></span>

<span data-ttu-id="82a89-156">toofind dlouho běžící dotazy, najdete v části [sledovat vaše úlohy pomocí zobrazení dynamické správy][Monitor your workload using DMVs].</span><span class="sxs-lookup"><span data-stu-id="82a89-156">toofind long-running queries, see [Monitor your workload using DMVs][Monitor your workload using DMVs].</span></span>

## <a name="security"></a><span data-ttu-id="82a89-157">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="82a89-157">Security</span></span>
<span data-ttu-id="82a89-158">toomaintain zabezpečení systému, musí být na hello upozornění a chránit proti libovolného typu neoprávněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="82a89-158">toomaintain a secure system, you must be on hello alert and guard against any type of unauthorized access.</span></span> <span data-ttu-id="82a89-159">Zabezpečení systému musí toomake se, že jsou pravidla brány firewall na místě, takže jenom oprávnění IP adresy se mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="82a89-159">A security system needs toomake sure firewall rules are in place so only authorized IP addresses can connect.</span></span> <span data-ttu-id="82a89-160">Musí být správné ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="82a89-160">It needs proper authentication of user credentials.</span></span> <span data-ttu-id="82a89-161">Poté, co uživatel má připojen toohello databáze, hello uživatele by měla mít pouze tooperform oprávnění minimální počet akcí.</span><span class="sxs-lookup"><span data-stu-id="82a89-161">After a user has connected toohello database, hello user should only have permissions tooperform a minimal number of actions.</span></span> <span data-ttu-id="82a89-162">toosecure data, můžete použít šifrování.</span><span class="sxs-lookup"><span data-stu-id="82a89-162">toosecure data, you can use encryption.</span></span> <span data-ttu-id="82a89-163">Je také důležité toohave auditování a sledování tak události můžete vrátit, pokud je podezřelé aktivity.</span><span class="sxs-lookup"><span data-stu-id="82a89-163">It's also important toohave auditing and tracking so you can retrace events if there is any suspicious activity.</span></span>

<span data-ttu-id="82a89-164">toolearn o správě zabezpečení, head přes toohello [Přehled zabezpečení][Security overview].</span><span class="sxs-lookup"><span data-stu-id="82a89-164">toolearn about managing security, head over toohello [Security overview][Security overview].</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="82a89-165">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="82a89-165">Backup and restore</span></span>
<span data-ttu-id="82a89-166">Spolehlivé backps vašich dat je nedílnou součást vámi vyžádaných žádné produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="82a89-166">Having reliable backps of your data is an essential part of any production database.</span></span> <span data-ttu-id="82a89-167">SQL Data Warehouse zajišťuje dat bezpečné zálohováním automaticky aktivní databáze v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="82a89-167">SQL Data Warehouse keeps your data safe by automatically backing up your active databases at regular intervals.</span></span> <span data-ttu-id="82a89-168">Tyto zálohy povolit toorecover z scénáře, kde jste poškozená data nebo neúmyslně vyřazen data nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="82a89-168">These backups allow you toorecover from scenarios where you've corrupted your data or accidentally dropped your data or database.</span></span>  <span data-ttu-id="82a89-169">Pro hello plán zálohování dat, zásady uchovávání informací a jak zjistit, toorestore databáze, [obnovení ze snímku][Restore from snapshot].</span><span class="sxs-lookup"><span data-stu-id="82a89-169">For hello data backup schedule, retention policy and how toorestore a database, see [Restore from snapshot][Restore from snapshot].</span></span>

## <a name="next-steps"></a><span data-ttu-id="82a89-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82a89-170">Next steps</span></span>
<span data-ttu-id="82a89-171">Pomocí zásady designu dobrý databáze bude snazší toomanage vaše databáze v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82a89-171">Using good database design principles will make it easier toomanage your databases in SQL Data Warehouse.</span></span> <span data-ttu-id="82a89-172">Další, toolearn head přes toohello [přehled vývoje][Development overview].</span><span class="sxs-lookup"><span data-stu-id="82a89-172">toolearn more, head over toohello [Development overview][Development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[škálování výkonu]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
