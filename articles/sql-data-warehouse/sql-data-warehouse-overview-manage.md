---
title: "Spravovat databáze v Azure SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: b14d0aad5a1f50c225391dbab27ec6240423a65a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a><span data-ttu-id="4534b-104">Spravovat databáze v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4534b-104">Manage databases in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4534b-105">SQL Data Warehouse automatizuje mnoho aspektů správy vašich databází.</span><span class="sxs-lookup"><span data-stu-id="4534b-105">SQL Data Warehouse automates many aspects of managing your databases.</span></span> <span data-ttu-id="4534b-106">Například škálování výkonu stačí upravit platit pro správnou úroveň výpočetní prostředky a potom umožní provádět všechny operace škálování a škálování zpět SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4534b-106">For example, to scale performance you only need to adjust and pay for the right level of compute resources, and then let SQL Data Warehouse do all the work of scaling out and scaling back.</span></span>

<span data-ttu-id="4534b-107">Nepochybně můžete sledovat vaše úlohy identifikace potřeb výkonu a také vyřešit dlouho běžící dotazy.</span><span class="sxs-lookup"><span data-stu-id="4534b-107">You will undoubtedly want to monitor your workload to identify your performance needs as well as troubleshoot long-running queries.</span></span> <span data-ttu-id="4534b-108">Musíte také provést několik úloh zabezpečení ke správě oprávnění pro uživatele a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4534b-108">You will also need to perform a few security tasks to manage permissions for users and logins.</span></span>

<span data-ttu-id="4534b-109">Tento přehled popisuje tyto aspekty správy SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4534b-109">This overview covers these aspects of managing SQL Data Warehouse.</span></span>

* <span data-ttu-id="4534b-110">Nástroje pro správu</span><span class="sxs-lookup"><span data-stu-id="4534b-110">Management tools</span></span>
* <span data-ttu-id="4534b-111">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="4534b-111">Scale Compute</span></span>
* <span data-ttu-id="4534b-112">Pozastavení a obnovení</span><span class="sxs-lookup"><span data-stu-id="4534b-112">Pause and Resume</span></span>
* <span data-ttu-id="4534b-113">Osvědčené postupy z hlediska výkonu</span><span class="sxs-lookup"><span data-stu-id="4534b-113">Performance Best Practices</span></span>
* <span data-ttu-id="4534b-114">Monitorování dotazu</span><span class="sxs-lookup"><span data-stu-id="4534b-114">Query Monitoring</span></span>
* <span data-ttu-id="4534b-115">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="4534b-115">Security</span></span>
* <span data-ttu-id="4534b-116">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="4534b-116">Backup and restore</span></span>

## <a name="management-tools"></a><span data-ttu-id="4534b-117">Nástroje pro správu</span><span class="sxs-lookup"><span data-stu-id="4534b-117">Management tools</span></span>
<span data-ttu-id="4534b-118">Můžete použít celou řadu nástrojů pro správu databází v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4534b-118">You can use a variety of tools to manage databases in SQL Data Warehouse.</span></span> <span data-ttu-id="4534b-119">Jak budete spravovat databáze, budete vyvíjet předvoleb nástroje pro každý typ úlohy, kterou je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="4534b-119">As you manage databases, you will develop tool preferences for each type of task you need to perform.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="4534b-120">portál Azure</span><span class="sxs-lookup"><span data-stu-id="4534b-120">Azure portal</span></span>
<span data-ttu-id="4534b-121">[Portál Azure] [ Azure portal] je webový portál, kde můžete vytvářet, aktualizovat a odstranit databáze a monitorovat prostředky v databázi.</span><span class="sxs-lookup"><span data-stu-id="4534b-121">The [Azure portal][Azure portal] is a web-based portal where you can create, update, and delete databases and monitor database resources.</span></span> <span data-ttu-id="4534b-122">Tento nástroj je skvělým je, pokud jste právě Začínáme s Azure, Správa malý počet databází datového skladu, nebo třeba rychle něco udělat.</span><span class="sxs-lookup"><span data-stu-id="4534b-122">This tool is great is if you're just getting started with Azure, managing a small number of data warehouse databases, or need to quickly do something.</span></span>

<span data-ttu-id="4534b-123">Chcete-li začít používat portál Azure, přečtěte si téma [vytvoření SQL Data Warehouse (portál Azure)][Create a SQL Data Warehouse (Azure portal)].</span><span class="sxs-lookup"><span data-stu-id="4534b-123">To get started with the Azure portal, see [Create a SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span></span>

### <a name="sql-server-data-tools-in-visual-studio"></a><span data-ttu-id="4534b-124">Nástroje SQL Server Data Tools v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4534b-124">SQL Server Data Tools in Visual Studio</span></span>
<span data-ttu-id="4534b-125">[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) v sadě Visual Studio umožňuje připojit, spravovat a vývoji vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="4534b-125">[SQL Server Data Tools][SQL Server Data Tools] (SSDT) in Visual Studio allows you to connect to, manage, and develop your databases.</span></span> <span data-ttu-id="4534b-126">Pokud jste vývojář aplikací obeznámeni s Visual Studio nebo jiné integrované vývojové prostředí (integrovaného vývojového prostředí), zkuste použít rozšíření SSDT v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4534b-126">If you're an application developer familiar with Visual Studio or other integrated development environments (IDEs), try using SSDT in Visual Studio.</span></span>

<span data-ttu-id="4534b-127">Rozšíření SSDT zahrnuje Průzkumník objektů SQL serveru, která umožňuje vizualizovat, připojit a spouštět skripty pro databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4534b-127">SSDT includes the SQL Server Object Explorer which enables you to visualize, connect, and execute scripts against SQL Data Warehouse databases.</span></span> <span data-ttu-id="4534b-128">Chcete-li rychle se připojit k SQL Data Warehouse, můžete jednoduše klikněte na tlačítko **otevřete v sadě Visual Studio** tlačítko v příkazu panelu při zobrazení databáze podrobnosti v portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="4534b-128">To quickly connect to SQL Data Warehouse, you can simply click the **Open in Visual Studio** button in the command bar when viewing the database details in the Azure Classic Portal.</span></span>  

<span data-ttu-id="4534b-129">Chcete-li začít pracovat s rozšířením SSDT v sadě Visual Studio, přečtěte si téma [dotazu Azure SQL Data Warehouse pomocí sady Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="4534b-129">To get started with SSDT in Visual Studio, see [Query Azure SQL Data Warehouse with Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span></span>

### <a name="command-line-tools"></a><span data-ttu-id="4534b-130">Nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4534b-130">Command-line tools</span></span>
<span data-ttu-id="4534b-131">Nástroje příkazového řádku jsou ideální pro automatizaci vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="4534b-131">Command line tools are ideal for automating your workloads.</span></span>  <span data-ttu-id="4534b-132">Prostředí PowerShell a sqlcmd dvěma způsoby skvělé a automatizovat procesy.</span><span class="sxs-lookup"><span data-stu-id="4534b-132">PowerShell and sqlcmd are two great ways to automate your processes.</span></span>  <span data-ttu-id="4534b-133">Doporučujeme, abyste tyto nástroje pro správu velkého počtu logických serverů a jako úlohy plánování může být skripty a pak automatizované nasazení prostředků změny v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4534b-133">We recommend these tools for managing a large number of logical servers and deploying resource changes in a production environment as the tasks necessary can be scripted and then automated.</span></span>

### <a name="dynamic-management-views"></a><span data-ttu-id="4534b-134">Zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="4534b-134">Dynamic management views</span></span>
<span data-ttu-id="4534b-135">Zobrazení dynamické správy jsou másla a chléb správy SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4534b-135">DMVs are the bread and butter of managing SQL Data Warehouse.</span></span> <span data-ttu-id="4534b-136">Téměř všechny informace, které se zobrazí na portálu spoléhá na zobrazení dynamické správy.</span><span class="sxs-lookup"><span data-stu-id="4534b-136">Almost all information that surfaces in the portal relies on DMVs.</span></span> <span data-ttu-id="4534b-137">Seznam SQL Data Warehouse zobrazení dynamické správy najdete v sekci [zobrazení systému SQL Data Warehouse][SQL Data Warehouse system views].</span><span class="sxs-lookup"><span data-stu-id="4534b-137">To see a list of SQL Data Warehouse DMVs, see [SQL Data Warehouse system views][SQL Data Warehouse system views].</span></span>

<span data-ttu-id="4534b-138">Abyste mohli začít, najdete v části [připojit a zadávat dotazy pomocí sqlcmd][Connect and query with sqlcmd], a [vytvoření databáze (PowerShell)][Create a database (PowerShell)].</span><span class="sxs-lookup"><span data-stu-id="4534b-138">To get started, see [Connect and query with sqlcmd][Connect and query with sqlcmd], and [Create a database (PowerShell)][Create a database (PowerShell)].</span></span>

## <a name="scale-compute"></a><span data-ttu-id="4534b-139">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="4534b-139">Scale compute</span></span>
<span data-ttu-id="4534b-140">V SQL Data Warehouse můžete rychle škálovat výkonu out nebo zpět zvýšením nebo snížením výpočetní prostředky procesoru, paměti a šířky pásma vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="4534b-140">In SQL Data Warehouse, you can quickly scale performance out or back by increasing or decreasing compute resources of CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="4534b-141">Škálování výkonu, všechny, které je potřeba je upravit počet jednotky datového skladu (Dwu) které SQL Data Warehouse přiděluje k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="4534b-141">To scale performance, all you need to do is adjust the number of data warehouse units (DWUs) that SQL Data Warehouse allocates to your database.</span></span> <span data-ttu-id="4534b-142">SQL Data Warehouse rychle provede změnu a zpracovává všechny základní změny hardwaru nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="4534b-142">SQL Data Warehouse quickly makes the change and handles all the underlying changes to hardware or software.</span></span>

<span data-ttu-id="4534b-143">Další informace o škálování Dwu najdete v tématu [škálování výkonu].</span><span class="sxs-lookup"><span data-stu-id="4534b-143">To learn more about scaling DWUs, see [Scale performance].</span></span>

## <a name="pause-and-resume"></a><span data-ttu-id="4534b-144">Pozastavení a obnovení</span><span class="sxs-lookup"><span data-stu-id="4534b-144">Pause and resume</span></span>
<span data-ttu-id="4534b-145">Abyste ušetřili náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="4534b-145">To save costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="4534b-146">Například pokud nebudete používat databázi v noci a o víkendech, můžete pozastavit tyto v době a obnovit během dne.</span><span class="sxs-lookup"><span data-stu-id="4534b-146">For example, if you won't be using the database during the night and on weekends, you can pause it during those times, and resume it during the day.</span></span> <span data-ttu-id="4534b-147">Zatímco databáze byla pozastavena, se nezapočítávají pro Dwu.</span><span class="sxs-lookup"><span data-stu-id="4534b-147">You won't be charged for DWUs while the database is paused.</span></span>

<span data-ttu-id="4534b-148">Další informace najdete v tématu [pozastavit výpočetní][Pause compute], a [obnovit výpočty][Resume compute].</span><span class="sxs-lookup"><span data-stu-id="4534b-148">For more information, see [Pause compute][Pause compute], and [Resume compute][Resume compute].</span></span>

## <a name="performance-best-practices"></a><span data-ttu-id="4534b-149">Osvědčené postupy z hlediska výkonu</span><span class="sxs-lookup"><span data-stu-id="4534b-149">Performance Best Practices</span></span>
<span data-ttu-id="4534b-150">Při Začínáme s novou technologií, zjišťování tipy a triky, které fungují nejlepší vpravo od začátku můžete ušetřit spoustu času.</span><span class="sxs-lookup"><span data-stu-id="4534b-150">When getting started with a new technology, discovering the tips and tricks that work best right from the start can save you lots of time.</span></span>  <span data-ttu-id="4534b-151">Osvědčené postupy v rámci řadu témata s našimi zjistíte.</span><span class="sxs-lookup"><span data-stu-id="4534b-151">You will find best practices throughout many of our topics.</span></span>

<span data-ttu-id="4534b-152">Mnoho souhrn nejdůležitější aspekty při vývoji vaše úlohy najdete v sekci [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="4534b-152">To see many a summary of the most important considerations when developing your workload, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

## <a name="query-monitoring"></a><span data-ttu-id="4534b-153">Monitorování dotazu</span><span class="sxs-lookup"><span data-stu-id="4534b-153">Query Monitoring</span></span>
<span data-ttu-id="4534b-154">Někdy dotazu běží příliš dlouho, ale nejsou opravdu které z nich je který.</span><span class="sxs-lookup"><span data-stu-id="4534b-154">Sometimes a query is running too long, but you aren't sure of which one is the culprit.</span></span> <span data-ttu-id="4534b-155">SQL Data Warehouse má zobrazení dynamické správy (zobrazení dynamické správy), které vám pomůže pochopit dotazu, který trvá příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="4534b-155">SQL Data Warehouse has dynamic management views (DMVs) that you can use to figure out which query is taking too long.</span></span>

<span data-ttu-id="4534b-156">Dlouho běžící dotazy, najdete v tématu [sledovat vaše úlohy pomocí zobrazení dynamické správy][Monitor your workload using DMVs].</span><span class="sxs-lookup"><span data-stu-id="4534b-156">To find long-running queries, see [Monitor your workload using DMVs][Monitor your workload using DMVs].</span></span>

## <a name="security"></a><span data-ttu-id="4534b-157">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="4534b-157">Security</span></span>
<span data-ttu-id="4534b-158">K zachování zabezpečení systému, musí být na výstrahu a chránit proti libovolného typu neoprávněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="4534b-158">To maintain a secure system, you must be on the alert and guard against any type of unauthorized access.</span></span> <span data-ttu-id="4534b-159">Zabezpečení systému je potřeba Ujistěte se, že jsou pravidla brány firewall na místě, takže jenom oprávnění IP adresy se mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="4534b-159">A security system needs to make sure firewall rules are in place so only authorized IP addresses can connect.</span></span> <span data-ttu-id="4534b-160">Musí být správné ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="4534b-160">It needs proper authentication of user credentials.</span></span> <span data-ttu-id="4534b-161">Jakmile se uživatel připojil k databázi, musí mít uživatel jenom oprávnění k provedení minimální počet akcí.</span><span class="sxs-lookup"><span data-stu-id="4534b-161">After a user has connected to the database, the user should only have permissions to perform a minimal number of actions.</span></span> <span data-ttu-id="4534b-162">K zabezpečení dat, můžete použít šifrování.</span><span class="sxs-lookup"><span data-stu-id="4534b-162">To secure data, you can use encryption.</span></span> <span data-ttu-id="4534b-163">Je také důležité, aby byly auditování a sledování tak události můžete vrátit, pokud je podezřelé aktivity.</span><span class="sxs-lookup"><span data-stu-id="4534b-163">It's also important to have auditing and tracking so you can retrace events if there is any suspicious activity.</span></span>

<span data-ttu-id="4534b-164">Další informace o správě zabezpečení, přejděte na [Přehled zabezpečení][Security overview].</span><span class="sxs-lookup"><span data-stu-id="4534b-164">To learn about managing security, head over to the [Security overview][Security overview].</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="4534b-165">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="4534b-165">Backup and restore</span></span>
<span data-ttu-id="4534b-166">Spolehlivé backps vašich dat je nedílnou součást vámi vyžádaných žádné produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="4534b-166">Having reliable backps of your data is an essential part of any production database.</span></span> <span data-ttu-id="4534b-167">SQL Data Warehouse zajišťuje dat bezpečné zálohováním automaticky aktivní databáze v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="4534b-167">SQL Data Warehouse keeps your data safe by automatically backing up your active databases at regular intervals.</span></span> <span data-ttu-id="4534b-168">Tyto zálohy umožňují obnovit z scénáře, kde jste poškozená data nebo neúmyslně vyřazen data nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="4534b-168">These backups allow you to recover from scenarios where you've corrupted your data or accidentally dropped your data or database.</span></span>  <span data-ttu-id="4534b-169">Plán zálohování dat, zásady uchovávání informací a jak obnovit databázi, najdete v části [obnovení ze snímku][Restore from snapshot].</span><span class="sxs-lookup"><span data-stu-id="4534b-169">For the data backup schedule, retention policy and how to restore a database, see [Restore from snapshot][Restore from snapshot].</span></span>

## <a name="next-steps"></a><span data-ttu-id="4534b-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4534b-170">Next steps</span></span>
<span data-ttu-id="4534b-171">Pomocí funkční databáze návrhu zásad bude bylo snazší spravovat databáze v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4534b-171">Using good database design principles will make it easier to manage your databases in SQL Data Warehouse.</span></span> <span data-ttu-id="4534b-172">Další informace, přejděte na [přehled vývoje][Development overview].</span><span class="sxs-lookup"><span data-stu-id="4534b-172">To learn more, head over to the [Development overview][Development overview].</span></span>

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
<span data-ttu-id="4534b-173">[škálování výkonu]: sql-data-warehouse-manage-compute-overview.md#scale-compute</span><span class="sxs-lookup"><span data-stu-id="4534b-173">[Scale performance]: sql-data-warehouse-manage-compute-overview.md#scale-compute</span></span>
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
