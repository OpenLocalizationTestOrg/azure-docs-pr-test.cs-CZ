# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="30bab-101">Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a spravovanými a nespravovanými prémiové disky</span><span class="sxs-lookup"><span data-stu-id="30bab-101">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="30bab-102">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se Azure spravované disky a Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="30bab-102">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="30bab-103">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="30bab-103">Managed Disks</span></span>

<span data-ttu-id="30bab-104">**Co je Azure spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-104">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="30bab-105">Spravované disků je funkce, která zjednodušuje Správa disku pro virtuální počítače Azure IaaS tím, že zpracování Správa účtů úložiště pro vás.</span><span class="sxs-lookup"><span data-stu-id="30bab-105">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="30bab-106">Další informace najdete v tématu [spravované disky přehled](../articles/virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="30bab-106">For more information, see the [Managed Disks overview](../articles/virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="30bab-107">**Když vytvořím standardní se spravovaným diskem z existujícího VHD, který je 80 GB, jaké budou který náklady mi?**</span><span class="sxs-lookup"><span data-stu-id="30bab-107">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="30bab-108">Standardní se spravovaným diskem vytvořen z disku VHD 80 GB je považována za další velikost standardní disku, která je k dispozici S10 disk.</span><span class="sxs-lookup"><span data-stu-id="30bab-108">A standard managed disk created from an 80-GB VHD is treated as the next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="30bab-109">Že se vám účtovat podle S10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="30bab-109">You're charged according to the S10 disk pricing.</span></span> <span data-ttu-id="30bab-110">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="30bab-110">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="30bab-111">**Existují žádné transakce náklady na standardní spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-111">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="30bab-112">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-112">Yes.</span></span> <span data-ttu-id="30bab-113">Vám účtovat každou transakci.</span><span class="sxs-lookup"><span data-stu-id="30bab-113">You're charged for each transaction.</span></span> <span data-ttu-id="30bab-114">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="30bab-114">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="30bab-115">**Pro standardní se spravovaným diskem I odečte skutečná velikost dat na disku nebo zřízená kapacita disku?**</span><span class="sxs-lookup"><span data-stu-id="30bab-115">**For a standard managed disk, will I be charged for the actual size of the data on the disk or for the provisioned capacity of the disk?**</span></span>

<span data-ttu-id="30bab-116">Že se vám účtovat podle zřízená kapacita disku.</span><span class="sxs-lookup"><span data-stu-id="30bab-116">You're charged based on the provisioned capacity of the disk.</span></span> <span data-ttu-id="30bab-117">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="30bab-117">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="30bab-118">**Jak je ceny disků premium spravované liší od nespravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-118">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="30bab-119">Ceny spravované prémiové disky je stejný jako nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-119">The pricing of premium managed disks is the same as unmanaged premium disks.</span></span>

<span data-ttu-id="30bab-120">**Můžete změnit typ účtu úložiště (Standard nebo Premium) Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="30bab-120">**Can I change the storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="30bab-121">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-121">Yes.</span></span> <span data-ttu-id="30bab-122">Typ účtu úložiště spravovaného disky můžete změnit pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="30bab-122">You can change the storage account type of your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="30bab-123">**Existuje způsob, že umožní kopírování a exportovat spravovaných disků na účet privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="30bab-123">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="30bab-124">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-124">Yes.</span></span> <span data-ttu-id="30bab-125">Vaše spravované disky můžete exportovat pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="30bab-125">You can export your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="30bab-126">**Můžete použít soubor virtuálního pevného disku v účtu úložiště Azure k vytvoření spravovaného disk pomocí jiné předplatné?**</span><span class="sxs-lookup"><span data-stu-id="30bab-126">**Can I use a VHD file in an Azure storage account to create a managed disk with a different subscription?**</span></span>

<span data-ttu-id="30bab-127">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-127">No.</span></span>

<span data-ttu-id="30bab-128">**Můžete použít soubor virtuálního pevného disku v účtu úložiště Azure k vytvoření spravovaného disku v jiné oblasti?**</span><span class="sxs-lookup"><span data-stu-id="30bab-128">**Can I use a VHD file in an Azure storage account to create a managed disk in a different region?**</span></span>

<span data-ttu-id="30bab-129">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-129">No.</span></span>

<span data-ttu-id="30bab-130">**Existují nějaká omezení škálování pro zákazníky, kteří používají spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-130">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="30bab-131">Spravované disky eliminuje omezení spojená s účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="30bab-131">Managed Disks eliminates the limits associated with storage accounts.</span></span> <span data-ttu-id="30bab-132">Počet spravovaných disků na jedno předplatné je však omezená na 2 000 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="30bab-132">However, the number of managed disks per subscription is limited to 2,000 by default.</span></span> <span data-ttu-id="30bab-133">Obraťte se na podporu, aby tento počet zvýšit.</span><span class="sxs-lookup"><span data-stu-id="30bab-133">You can call support to increase this number.</span></span>

<span data-ttu-id="30bab-134">**Může trvat přírůstkový snímek spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="30bab-134">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="30bab-135">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-135">No.</span></span> <span data-ttu-id="30bab-136">Aktuální vytváření snímků provede úplnou kopii se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="30bab-136">The current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="30bab-137">Doporučujeme však hodláte podporují přírůstkové snímky v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="30bab-137">However, we are planning to support incremental snapshots in the future.</span></span>

<span data-ttu-id="30bab-138">**Virtuální počítače v nastavení dostupnosti se může skládat z kombinace spravovanými a nespravovanými disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-138">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="30bab-139">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-139">No.</span></span> <span data-ttu-id="30bab-140">Virtuální počítače v nastavení dostupnosti musí používat disky všechny spravované nebo nespravované všechny disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-140">The VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="30bab-141">Když vytvoříte skupinu dostupnosti, můžete typu disky, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="30bab-141">When you create an availability set, you can choose which type of disks you want to use.</span></span>

<span data-ttu-id="30bab-142">**Spravované disků je výchozí možnost na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="30bab-142">**Is Managed Disks the default option in the Azure portal?**</span></span>

<span data-ttu-id="30bab-143">Není v současné době ale výchozí hodnota se stane v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="30bab-143">Not currently, but it will become the default in the future.</span></span>

<span data-ttu-id="30bab-144">**Můžete vytvořit prázdný disk spravované?**</span><span class="sxs-lookup"><span data-stu-id="30bab-144">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="30bab-145">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-145">Yes.</span></span> <span data-ttu-id="30bab-146">Můžete vytvořit prázdný disk.</span><span class="sxs-lookup"><span data-stu-id="30bab-146">You can create an empty disk.</span></span> <span data-ttu-id="30bab-147">Spravovaný disk můžete vytvořit nezávisle na virtuální počítač, například bez připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="30bab-147">A managed disk can be created independently of a VM, for example, without attaching it to a VM.</span></span>

<span data-ttu-id="30bab-148">**Co je počet domén selhání podporované pro dostupnosti nastavit používající spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-148">**What is the supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="30bab-149">V závislosti na oblasti, kde se nachází skupiny dostupnosti, který používá disky spravovat je počet domén selhání podporované 2 nebo 3.</span><span class="sxs-lookup"><span data-stu-id="30bab-149">Depending on the region where the availability set that uses Managed Disks is located, the supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="30bab-150">**Jak je účet standardního úložiště pro diagnostiku nastavení?**</span><span class="sxs-lookup"><span data-stu-id="30bab-150">**How is the standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="30bab-151">Nastavit účet privátní úložiště pro diagnostiku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="30bab-151">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="30bab-152">V budoucnu plánujeme také přepnout diagnostiky spravované disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-152">In the future, we plan to switch diagnostics to Managed Disks as well.</span></span>

<span data-ttu-id="30bab-153">**Jaký druh řízení přístupu na základě Role podpora je k dispozici pro spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-153">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="30bab-154">Spravované disky podporuje tři klíčové výchozích rolí:</span><span class="sxs-lookup"><span data-stu-id="30bab-154">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="30bab-155">Vlastník: Můžou spravovat všechno včetně přístupu</span><span class="sxs-lookup"><span data-stu-id="30bab-155">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="30bab-156">Přispěvatel: Můžou spravovat všechno kromě přístupu</span><span class="sxs-lookup"><span data-stu-id="30bab-156">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="30bab-157">Čtečka: Zobrazit vše, co, ale nelze provádět změny</span><span class="sxs-lookup"><span data-stu-id="30bab-157">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="30bab-158">**Existuje způsob, že umožní kopírování a exportovat spravovaných disků na účet privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="30bab-158">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="30bab-159">Můžete získat jen pro čtení sdíleného přístupového podpisu identifikátor URI pro spravované disk a použít ho zkopírovat obsah do privátní úložiště účet nebo místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="30bab-159">You can get a read-only shared access signature URI for the managed disk and use it to copy the contents to a private storage account or on-premises storage.</span></span>

<span data-ttu-id="30bab-160">**Můžete vytvořit kopii Moje spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="30bab-160">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="30bab-161">Zákazníci můžete pořízení snímku jejich spravované disky a potom pomocí snímku vytvoření spravovaných disků na jiný.</span><span class="sxs-lookup"><span data-stu-id="30bab-161">Customers can take a snapshot of their managed disks and then use the snapshot to create another managed disk.</span></span>

<span data-ttu-id="30bab-162">**Jsou nespravované disky stále podporovány?**</span><span class="sxs-lookup"><span data-stu-id="30bab-162">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="30bab-163">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-163">Yes.</span></span> <span data-ttu-id="30bab-164">Podporujeme nespravované a spravované disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-164">We support unmanaged and managed disks.</span></span> <span data-ttu-id="30bab-165">Doporučujeme používat spravované disky pro nové úlohy a migraci vašich aktuální zatížení na spravované disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-165">We recommend that you use managed disks for new workloads and migrate your current workloads to managed disks.</span></span>


<span data-ttu-id="30bab-166">**Je-li vytvořit 128 GB disk a poté zvýšit velikost 130 GB, bude I vám účtována další velikost disku (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="30bab-166">**If I create a 128-GB disk and then increase the size to 130 GB, will I be charged for the next disk size (512 GB)?**</span></span>

<span data-ttu-id="30bab-167">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-167">Yes.</span></span>

<span data-ttu-id="30bab-168">**Můžete vytvořit místně redundantní úložiště, geograficky redundantní úložiště, a zónově redundantní úložiště spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-168">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="30bab-169">Disky systému Azure spravované aktuálně podporuje pouze místně redundantního úložiště spravované disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-169">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="30bab-170">**Můžete zmenšit nebo downsize Moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-170">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="30bab-171">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-171">No.</span></span> <span data-ttu-id="30bab-172">Tato funkce není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="30bab-172">This feature is not supported currently.</span></span> 

<span data-ttu-id="30bab-173">**Můžete změnit vlastnost název počítače při specializované (ne vytvořili pomocí nástroje pro přípravu systému nebo zobecněn) disk operačního systému slouží ke zřízení virtuálního počítače?**</span><span class="sxs-lookup"><span data-stu-id="30bab-173">**Can I change the computer name property when a specialized (not created by using the System Preparation tool or generalized) operating system disk is used to provision a VM?**</span></span>

<span data-ttu-id="30bab-174">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-174">No.</span></span> <span data-ttu-id="30bab-175">Nelze aktualizovat vlastnosti název počítače.</span><span class="sxs-lookup"><span data-stu-id="30bab-175">You can't update the computer name property.</span></span> <span data-ttu-id="30bab-176">Nový virtuální počítač se dědí z nadřazený virtuální počítač, který byl použit k vytvoření disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="30bab-176">The new VM inherits it from the parent VM, which was used to create the operating system disk.</span></span> 

<span data-ttu-id="30bab-177">**Kde najdu ukázkových šablon Azure Resource Manageru k vytvoření virtuálních počítačů s spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-177">**Where can I find sample Azure Resource Manager templates to create VMs with managed disks?**</span></span>
* [<span data-ttu-id="30bab-178">Seznam šablon pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="30bab-178">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="30bab-179">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="30bab-179">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="30bab-180">Spravované disky a šifrování služby úložiště</span><span class="sxs-lookup"><span data-stu-id="30bab-180">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="30bab-181">**Šifrování služby úložiště Azure ve výchozím nastavení zapnutá po vytvoření se spravovaným diskem?**</span><span class="sxs-lookup"><span data-stu-id="30bab-181">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="30bab-182">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-182">Yes.</span></span>

<span data-ttu-id="30bab-183">**Kdo spravuje šifrovacích klíčů?**</span><span class="sxs-lookup"><span data-stu-id="30bab-183">**Who manages the encryption keys?**</span></span>

<span data-ttu-id="30bab-184">Microsoft spravuje šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="30bab-184">Microsoft manages the encryption keys.</span></span>

<span data-ttu-id="30bab-185">**Můžete zakázat šifrování služby úložiště pro moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-185">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="30bab-186">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-186">No.</span></span>

<span data-ttu-id="30bab-187">**Je šifrování služby úložiště k dispozici pouze v určitých oblastí?**</span><span class="sxs-lookup"><span data-stu-id="30bab-187">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="30bab-188">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-188">No.</span></span> <span data-ttu-id="30bab-189">Je k dispozici ve všech oblastech, kde je k dispozici spravované disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-189">It's available in all the regions where Managed Disks is available.</span></span> <span data-ttu-id="30bab-190">Spravované disků je k dispozici ve všech veřejných oblastí a Německu.</span><span class="sxs-lookup"><span data-stu-id="30bab-190">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="30bab-191">**Jak můžete zjistit, pokud je zašifrovaná Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="30bab-191">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="30bab-192">Můžete získat čas vytvoření spravovaného disku z portálu Azure, rozhraní příkazového řádku Azure a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="30bab-192">You can find out the time when a managed disk was created from the Azure portal, the Azure CLI, and PowerShell.</span></span> <span data-ttu-id="30bab-193">Pokud je doba po 9. června 2017, se šifrují na disku.</span><span class="sxs-lookup"><span data-stu-id="30bab-193">If the time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="30bab-194">**Jak můžete šifrovat Můj stávající disky, které byly vytvořeny před 10 června 2017?**</span><span class="sxs-lookup"><span data-stu-id="30bab-194">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="30bab-195">Od verze 10 června 2017 je nová data stávající spravovaný disky šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="30bab-195">As of June 10, 2017, new data written to existing managed disks is automatically encrypted.</span></span> <span data-ttu-id="30bab-196">Můžeme také plánování šifrování stávající data a šifrování asynchronně proběhne na pozadí.</span><span class="sxs-lookup"><span data-stu-id="30bab-196">We are also planning to encrypt existing data, and the encryption will happen asynchronously in the background.</span></span> <span data-ttu-id="30bab-197">Pokud je nutné šifrovat všechna existující data, vytvoří se kopie disku.</span><span class="sxs-lookup"><span data-stu-id="30bab-197">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="30bab-198">Bude se šifrovat nové disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-198">New disks will be encrypted.</span></span>

* [<span data-ttu-id="30bab-199">Zkopírovat disky spravované pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="30bab-199">Copy managed disks by using the Azure CLI</span></span>](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="30bab-200">Zkopírujte spravované disky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="30bab-200">Copy managed disks by using PowerShell</span></span>](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="30bab-201">**Jsou spravované snímky a bitové kopie zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="30bab-201">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="30bab-202">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-202">Yes.</span></span> <span data-ttu-id="30bab-203">Všechny spravované snímky a bitové kopie vytvořené po 9. června 2017, se šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="30bab-203">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="30bab-204">**Můžete převést virtuální počítače s nespravované disky, které se nacházejí na účtech úložiště, které jsou nebo byly dříve šifrovaná na spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-204">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted to managed disks?**</span></span>

<span data-ttu-id="30bab-205">Ano</span><span class="sxs-lookup"><span data-stu-id="30bab-205">Yes</span></span>

<span data-ttu-id="30bab-206">**Budou exportovaného virtuálního pevného disku z spravovaného disku nebo snímek také zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="30bab-206">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="30bab-207">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-207">No.</span></span> <span data-ttu-id="30bab-208">Pokud exportujete virtuální pevný disk k účtu úložiště šifrované z šifrované, ale spravované disku nebo snímek a potom je zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="30bab-208">But if you export a VHD to an encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="30bab-209">Pro prémiové disky: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="30bab-209">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="30bab-210">**Pokud virtuální počítač používá velikost série, která podporuje službu Premium Storage, jako je například DSv2, můžu připojit premium a standard datové disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-210">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="30bab-211">Ano.</span><span class="sxs-lookup"><span data-stu-id="30bab-211">Yes.</span></span>

<span data-ttu-id="30bab-212">**Můžete připojit premium a standard datových disků pro velikost série, která nepodporuje Storage úrovně Premium, jako je například D Dv2, G nebo F řady?**</span><span class="sxs-lookup"><span data-stu-id="30bab-212">**Can I attach both premium and standard data disks to a size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="30bab-213">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-213">No.</span></span> <span data-ttu-id="30bab-214">Pouze standardní datových disků můžete připojit k virtuálním počítačům, které nepoužívají velikost série, která podporuje službu Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="30bab-214">You can attach only standard data disks to VMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="30bab-215">**Když vytvořím premium datový disk z existujícího VHD, který byl 80 GB, kolik bude která stojí?**</span><span class="sxs-lookup"><span data-stu-id="30bab-215">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="30bab-216">Datový disk premium vytvořen z disku VHD 80 GB je považována za velikost disku k dispozici další premium, což je P10 disku.</span><span class="sxs-lookup"><span data-stu-id="30bab-216">A premium data disk created from an 80-GB VHD is treated as the next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="30bab-217">Že se vám účtovat podle P10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="30bab-217">You're charged according to the P10 disk pricing.</span></span>

<span data-ttu-id="30bab-218">**Existují transakce náklady na použití služby Premium Storage?**</span><span class="sxs-lookup"><span data-stu-id="30bab-218">**Are there transaction costs to use Premium Storage?**</span></span>

<span data-ttu-id="30bab-219">Není opravené náklady pro každou velikost disku, která pochází zřízené pomocí omezení na IOPS a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="30bab-219">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="30bab-220">Další náklady jsou šířky odchozího pásma a kapacity snímku, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="30bab-220">The other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="30bab-221">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="30bab-221">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="30bab-222">**Jaké jsou limity pro IOPS a propustnosti, kterou můžete dostat z mezipaměti disku?**</span><span class="sxs-lookup"><span data-stu-id="30bab-222">**What are the limits for IOPS and throughput that I can get from the disk cache?**</span></span>

<span data-ttu-id="30bab-223">Kombinované omezení pro mezipaměť a místní SSD pro řady DS jsou 4 000 IOPS na základní a 33 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="30bab-223">The combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="30bab-224">GS řady nabízí 5 000 IOPS na základní a 50 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="30bab-224">The GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="30bab-225">**Je podporovaný na místní SSD pro virtuální počítač spravovaný disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-225">**Is the local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="30bab-226">Na místní SSD je dočasné úložiště, která je součástí spravované virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-226">The local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="30bab-227">Existuje nejsou zpoplatněné pro toto dočasný úložiště.</span><span class="sxs-lookup"><span data-stu-id="30bab-227">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="30bab-228">Doporučujeme vám, nepoužívejte tento místní SSD ukládat data aplikací, protože není trvalý v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="30bab-228">We recommend that you do not use this local SSD to store your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="30bab-229">**Existují jakékoli následky pro použití operace TRIM na prémiové disky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-229">**Are there any repercussions for the use of TRIM on premium disks?**</span></span>

<span data-ttu-id="30bab-230">Neexistuje žádné nevýhodou použití operace TRIM na disky Azure na buď premium nebo standardní disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-230">There is no downside to the use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="30bab-231">Nové velikosti disků: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="30bab-231">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="30bab-232">**Co je podporováno pro operační systém a datové disky největší velikost disku?**</span><span class="sxs-lookup"><span data-stu-id="30bab-232">**What is the largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="30bab-233">Typ oddílu, který podporuje Azure pro disk operačního systému je hlavní spouštěcí záznam (MBR).</span><span class="sxs-lookup"><span data-stu-id="30bab-233">The partition type that Azure supports for an operating system disk is the master boot record (MBR).</span></span> <span data-ttu-id="30bab-234">Podporuje formátu MBR disk velikost až 2 TB.</span><span class="sxs-lookup"><span data-stu-id="30bab-234">The MBR format supports a disk size up to 2 TB.</span></span> <span data-ttu-id="30bab-235">Největší velikost, kterou podporuje Azure pro disk operačního systému je 2 TB.</span><span class="sxs-lookup"><span data-stu-id="30bab-235">The largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="30bab-236">Azure podporuje až 4 TB pro datové disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-236">Azure supports up to 4 TB for data disks.</span></span> 

<span data-ttu-id="30bab-237">**Co je největší velikost objektu blob stránky, která je podporována?**</span><span class="sxs-lookup"><span data-stu-id="30bab-237">**What is the largest page blob size that's supported?**</span></span>

<span data-ttu-id="30bab-238">Největší velikost objektu blob stránky, který podporuje Azure je 8 TB (8 191 GB).</span><span class="sxs-lookup"><span data-stu-id="30bab-238">The largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="30bab-239">Nepodporujeme objekty BLOB stránky větší než 4 TB (4095 GB) připojené k virtuálnímu počítači jako data nebo disky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="30bab-239">We don't support page blobs larger than 4 TB (4,095 GB) attached to a VM as data or operating system disks.</span></span>

<span data-ttu-id="30bab-240">**Je potřeba použít novou verzi nástroje Azure vytvořit, připojení, přizpůsobit a odeslat disky, které jsou větší než 1 TB?**</span><span class="sxs-lookup"><span data-stu-id="30bab-240">**Do I need to use a new version of Azure tools to create, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="30bab-241">Není nutné upgradovat existující nástroje Azure k vytvoření, připojit nebo změna velikosti disků, které jsou větší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="30bab-241">You don't need to upgrade your existing Azure tools to create, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="30bab-242">K odeslání souboru virtuálního pevného disku z místního přímo do Azure jako objekt blob stránky nebo nespravované disku, budete muset použít nejnovější sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="30bab-242">To upload your VHD file from on-premises directly to Azure as a page blob or unmanaged disk, you need to use the latest tool sets:</span></span>

|<span data-ttu-id="30bab-243">Nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="30bab-243">Azure tools</span></span>      | <span data-ttu-id="30bab-244">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="30bab-244">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="30bab-245">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="30bab-245">Azure PowerShell</span></span> | <span data-ttu-id="30bab-246">Číslo verze 4.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="30bab-246">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="30bab-247">Rozhraní příkazového řádku Azure v1</span><span class="sxs-lookup"><span data-stu-id="30bab-247">Azure CLI v1</span></span>     | <span data-ttu-id="30bab-248">Číslo verze 0.10.13: pravděpodobně 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="30bab-248">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="30bab-249">AzCopy</span><span class="sxs-lookup"><span data-stu-id="30bab-249">AzCopy</span></span>           | <span data-ttu-id="30bab-250">Číslo verze 6.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="30bab-250">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="30bab-251">Podpora rozhraní příkazového řádku Azure v2 a Azure Storage Explorer tu bude brzo dostupná.</span><span class="sxs-lookup"><span data-stu-id="30bab-251">The support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="30bab-252">**Jsou podporovány P4 a P6 velikosti disků pro nespravovaná disky nebo objekty BLOB stránky?**</span><span class="sxs-lookup"><span data-stu-id="30bab-252">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="30bab-253">Ne.</span><span class="sxs-lookup"><span data-stu-id="30bab-253">No.</span></span> <span data-ttu-id="30bab-254">P4 (32 GB) a P6 velikosti disků (64 GB) jsou podporovány pouze u spravovaných disky.</span><span class="sxs-lookup"><span data-stu-id="30bab-254">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="30bab-255">Podpora pro nespravovaná disky a objekty BLOB stránky bude brzo.</span><span class="sxs-lookup"><span data-stu-id="30bab-255">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="30bab-256">**Pokud mé existující premium spravované disku menší než 64 GB byl vytvořen před povolením malý disk (přibližně 15. června 2017), jak se jeho fakturuje?**</span><span class="sxs-lookup"><span data-stu-id="30bab-256">**If my existing premium managed disk less than 64 GB was created before the small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="30bab-257">Stávající malých premium disky menší než 64 GB dál účtovat podle cenové úrovně P10.</span><span class="sxs-lookup"><span data-stu-id="30bab-257">Existing small premium disks less than 64 GB continue to be billed according to the P10 pricing tier.</span></span> 

<span data-ttu-id="30bab-258">**Jak můžete přepnout na úrovni disku malé prémiové disky menší než 64 GB z P10 P4 nebo P6?**</span><span class="sxs-lookup"><span data-stu-id="30bab-258">**How can I switch the disk tier of small premium disks less than 64 GB from P10 to P4 or P6?**</span></span>

<span data-ttu-id="30bab-259">Můžete pořízení snímku menší disky a potom vytvořit disk automaticky přepínat cenové úrovně P4 nebo P6 podle zřízené velikost.</span><span class="sxs-lookup"><span data-stu-id="30bab-259">You can take a snapshot of your small disks and then create a disk to automatically switch the pricing tier to P4 or P6 based on the provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="30bab-260">Co když není zde odpovědi můj dotaz?</span><span class="sxs-lookup"><span data-stu-id="30bab-260">What if my question isn't answered here?</span></span>

<span data-ttu-id="30bab-261">Pokud váš dotaz není zde uvedeno, dejte nám vědět, a pomůžeme vám najít odpověď.</span><span class="sxs-lookup"><span data-stu-id="30bab-261">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="30bab-262">V komentářích můžete odeslat dotaz na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="30bab-262">You can post a question at the end of this article in the comments.</span></span> <span data-ttu-id="30bab-263">Chcete-li se spojte s týmu Azure Storage a ostatními členy komunity o tomto článku, použijte webu MSDN [fórum pro Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="30bab-263">To engage with the Azure Storage team and other community members about this article, use the MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="30bab-264">Chcete-li požádat o funkce, odeslání požadavků a nápady, jak [fóru pro zpětnou vazbu Azure Storage](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="30bab-264">To request features, submit your requests and ideas to the [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
