
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
<span data-ttu-id="66181-101">Abyste ušetřili náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="66181-101">To save costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="66181-102">Například pokud nebudete používat databázi v noci a o víkendech, můžete pozastavit tyto v době a obnovit během dne.</span><span class="sxs-lookup"><span data-stu-id="66181-102">For example, if you won't be using the database during the night and on weekends, you can pause it during those times, and resume it during the day.</span></span> <span data-ttu-id="66181-103">Zatímco databáze byla pozastavena, se nezapočítávají pro Dwu.</span><span class="sxs-lookup"><span data-stu-id="66181-103">You won't be charged for DWUs while the database is paused.</span></span>

<span data-ttu-id="66181-104">Při přesunutí ukazatele myši databáze:</span><span class="sxs-lookup"><span data-stu-id="66181-104">When you pause a database:</span></span>

* <span data-ttu-id="66181-105">Paměťovou a výpočetní prostředky se vrátí do fondu dostupné prostředky v datovém centru</span><span class="sxs-lookup"><span data-stu-id="66181-105">Compute and memory resources are returned to the pool of available resources in the data center</span></span>
* <span data-ttu-id="66181-106">DWU náklady jsou nula po dobu trvání pozastavení.</span><span class="sxs-lookup"><span data-stu-id="66181-106">DWU costs are zero for the duration of the pause.</span></span>
* <span data-ttu-id="66181-107">Úložiště dat není ovlivněná a vaše data zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="66181-107">Data storage is not affected and your data stays intact.</span></span> 
* <span data-ttu-id="66181-108">SQL Data Warehouse zruší všechny spuštěné nebo zařazené ve frontě operace.</span><span class="sxs-lookup"><span data-stu-id="66181-108">SQL Data Warehouse cancels all running or queued operations.</span></span>

