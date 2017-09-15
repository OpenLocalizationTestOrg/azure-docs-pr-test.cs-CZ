<span data-ttu-id="73126-101">Dále pokud všechny servery v clusteru se systémem Windows Server 2008 R2 nebo Windows Server 2012, musíte ověřit, že oprava hotfix [KB2854082](http://support.microsoft.com/kb/2854082) je nainstalován na všech místních serverů a virtuálních počítačích Azure, které jsou součástí clusteru.</span><span class="sxs-lookup"><span data-stu-id="73126-101">Next, if any servers on the cluster are running Windows Server 2008 R2 or Windows Server 2012, you must verify that the hotfix [KB2854082](http://support.microsoft.com/kb/2854082) is installed on each of the on-premises servers or Azure VMs that are part of the cluster.</span></span> <span data-ttu-id="73126-102">Libovolný server nebo virtuální počítač, který je v clusteru, ale není ve skupině dostupnosti, musí být také tato oprava hotfix nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="73126-102">Any server or VM that is in the cluster, but not in the availability group, should also have this hotfix installed.</span></span>

<span data-ttu-id="73126-103">V této relaci vzdálené plochy pro každý z uzlů clusteru, stáhněte si [KB2854082](http://support.microsoft.com/kb/2854082) do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="73126-103">In the remote desktop session for each of the cluster nodes, download [KB2854082](http://support.microsoft.com/kb/2854082) to a local directory.</span></span> <span data-ttu-id="73126-104">Potom nainstalujte opravu hotfix na všech uzlech clusteru postupně.</span><span class="sxs-lookup"><span data-stu-id="73126-104">Then, install the hotfix on each cluster node sequentially.</span></span> <span data-ttu-id="73126-105">Pokud je Clusterová služba aktuálně běží na uzlu clusteru, na konci instalace opravy hotfix restartování serveru.</span><span class="sxs-lookup"><span data-stu-id="73126-105">If the cluster service is currently running on the cluster node, the server is restarted at the end of the hotfix installation.</span></span>

> [!WARNING]
> <span data-ttu-id="73126-106">Zastavením Clusterové služby nebo restartováním serveru ovlivňuje stavu kvora clusteru a skupinu dostupnosti a může způsobit clusteru do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="73126-106">Stopping the cluster service or restarting the server affects the quorum health of your cluster and the availability group, and it might cause your cluster to go offline.</span></span> <span data-ttu-id="73126-107">Aby se zachovala vysokou dostupnost clusteru během instalace, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="73126-107">To maintain the high availability of your cluster during installation, make sure that:</span></span>
> 
> * <span data-ttu-id="73126-108">Cluster je ve stavu optimální kvora.</span><span class="sxs-lookup"><span data-stu-id="73126-108">The cluster is in optimal quorum health.</span></span> 
> * <span data-ttu-id="73126-109">Než nainstalujete opravu hotfix v každém uzlu, jsou všechny uzly clusteru online.</span><span class="sxs-lookup"><span data-stu-id="73126-109">Before you install the hotfix on any node, all cluster nodes are online.</span></span>
> * <span data-ttu-id="73126-110">Před instalací opravy hotfix do jiného uzlu v clusteru, povolit na na jeden uzel, včetně plně restartování serveru spusťte k dokončení instalace opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="73126-110">Before you install the hotfix on any other node in the cluster, allow the hotfix installation to run to completion on one node, including fully restarting the server.</span></span>
> 
> 
