
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, hello following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
<span data-ttu-id="9ea87-101">toosave náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="9ea87-101">toosave costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="9ea87-102">Například pokud nebudete používat hello databáze během noční hello a o víkendech, můžete pozastavit tyto v době a obnovit během dne hello.</span><span class="sxs-lookup"><span data-stu-id="9ea87-102">For example, if you won't be using hello database during hello night and on weekends, you can pause it during those times, and resume it during hello day.</span></span> <span data-ttu-id="9ea87-103">Vám nebude nic účtováno pro Dwu při hello databáze byla pozastavena.</span><span class="sxs-lookup"><span data-stu-id="9ea87-103">You won't be charged for DWUs while hello database is paused.</span></span>

<span data-ttu-id="9ea87-104">Při přesunutí ukazatele myši databáze:</span><span class="sxs-lookup"><span data-stu-id="9ea87-104">When you pause a database:</span></span>

* <span data-ttu-id="9ea87-105">Paměťovou a výpočetní prostředky jsou vráceny toohello fond prostředků, k dispozici v datovém centru hello</span><span class="sxs-lookup"><span data-stu-id="9ea87-105">Compute and memory resources are returned toohello pool of available resources in hello data center</span></span>
* <span data-ttu-id="9ea87-106">DWU náklady jsou nula hello dobu pozastavit hello.</span><span class="sxs-lookup"><span data-stu-id="9ea87-106">DWU costs are zero for hello duration of hello pause.</span></span>
* <span data-ttu-id="9ea87-107">Úložiště dat není ovlivněná a vaše data zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="9ea87-107">Data storage is not affected and your data stays intact.</span></span> 
* <span data-ttu-id="9ea87-108">SQL Data Warehouse zruší všechny spuštěné nebo zařazené ve frontě operace.</span><span class="sxs-lookup"><span data-stu-id="9ea87-108">SQL Data Warehouse cancels all running or queued operations.</span></span>

