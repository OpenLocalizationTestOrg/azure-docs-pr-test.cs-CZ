# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="63173-101">Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a spravovanými a nespravovanými prémiové disky</span><span class="sxs-lookup"><span data-stu-id="63173-101">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="63173-102">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se Azure spravované disky a Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="63173-102">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="63173-103">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="63173-103">Managed Disks</span></span>

<span data-ttu-id="63173-104">**Co je Azure spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-104">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="63173-105">Spravované disků je funkce, která zjednodušuje Správa disku pro virtuální počítače Azure IaaS tím, že zpracování Správa účtů úložiště pro vás.</span><span class="sxs-lookup"><span data-stu-id="63173-105">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="63173-106">Další informace najdete v tématu hello [spravované disky přehled](../articles/virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="63173-106">For more information, see hello [Managed Disks overview](../articles/virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="63173-107">**Když vytvořím standardní se spravovaným diskem z existujícího VHD, který je 80 GB, jaké budou který náklady mi?**</span><span class="sxs-lookup"><span data-stu-id="63173-107">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="63173-108">Standardní se spravovaným diskem vytvořen z disku VHD 80 GB je považována za hello další dostupné standardní velikost disku, který je k dispozici S10 disk.</span><span class="sxs-lookup"><span data-stu-id="63173-108">A standard managed disk created from an 80-GB VHD is treated as hello next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="63173-109">Se vám účtovat podle toohello S10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="63173-109">You're charged according toohello S10 disk pricing.</span></span> <span data-ttu-id="63173-110">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="63173-110">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="63173-111">**Existují žádné transakce náklady na standardní spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-111">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="63173-112">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-112">Yes.</span></span> <span data-ttu-id="63173-113">Vám účtovat každou transakci.</span><span class="sxs-lookup"><span data-stu-id="63173-113">You're charged for each transaction.</span></span> <span data-ttu-id="63173-114">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="63173-114">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="63173-115">**Pro standardní se spravovaným diskem I odečte hello skutečná velikost hello dat na disku hello nebo hello zřídit kapacitu disku hello?**</span><span class="sxs-lookup"><span data-stu-id="63173-115">**For a standard managed disk, will I be charged for hello actual size of hello data on hello disk or for hello provisioned capacity of hello disk?**</span></span>

<span data-ttu-id="63173-116">Že se vám účtovat podle hello zřídit kapacitu disku hello.</span><span class="sxs-lookup"><span data-stu-id="63173-116">You're charged based on hello provisioned capacity of hello disk.</span></span> <span data-ttu-id="63173-117">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="63173-117">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="63173-118">**Jak je ceny disků premium spravované liší od nespravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-118">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="63173-119">Hello ceny disků spravované premium je hello stejné jako nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="63173-119">hello pricing of premium managed disks is hello same as unmanaged premium disks.</span></span>

<span data-ttu-id="63173-120">**Můžete změnit hello typ účtu úložiště (Standard nebo Premium) Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="63173-120">**Can I change hello storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="63173-121">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-121">Yes.</span></span> <span data-ttu-id="63173-122">Typ účtu úložiště hello spravované disky můžete změnit pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="63173-122">You can change hello storage account type of your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="63173-123">**Existuje způsob, že umožní kopírování a exportovat účet spravovaných disků na tooa privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="63173-123">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="63173-124">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-124">Yes.</span></span> <span data-ttu-id="63173-125">Vaše spravované disky můžete exportovat pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="63173-125">You can export your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="63173-126">**Můžete použít soubor virtuálního pevného disku v toocreate účet úložiště Azure se spravovaným diskem s jiného předplatného?**</span><span class="sxs-lookup"><span data-stu-id="63173-126">**Can I use a VHD file in an Azure storage account toocreate a managed disk with a different subscription?**</span></span>

<span data-ttu-id="63173-127">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-127">No.</span></span>

<span data-ttu-id="63173-128">**Můžete použít soubor virtuálního pevného disku v toocreate účet úložiště Azure se spravovaným diskem v jiné oblasti.**</span><span class="sxs-lookup"><span data-stu-id="63173-128">**Can I use a VHD file in an Azure storage account toocreate a managed disk in a different region?**</span></span>

<span data-ttu-id="63173-129">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-129">No.</span></span>

<span data-ttu-id="63173-130">**Existují nějaká omezení škálování pro zákazníky, kteří používají spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-130">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="63173-131">Spravované disky eliminuje hello omezení spojená s účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="63173-131">Managed Disks eliminates hello limits associated with storage accounts.</span></span> <span data-ttu-id="63173-132">Hello počet spravovaných disků na jedno předplatné, je však omezená too2 000 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="63173-132">However, hello number of managed disks per subscription is limited too2,000 by default.</span></span> <span data-ttu-id="63173-133">Podpora tooincrease můžete volat toto číslo.</span><span class="sxs-lookup"><span data-stu-id="63173-133">You can call support tooincrease this number.</span></span>

<span data-ttu-id="63173-134">**Může trvat přírůstkový snímek spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="63173-134">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="63173-135">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-135">No.</span></span> <span data-ttu-id="63173-136">aktuální vytváření snímků Hello provede úplnou kopii se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="63173-136">hello current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="63173-137">Jsme ale plánování toosupport přírůstkové snímky v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="63173-137">However, we are planning toosupport incremental snapshots in hello future.</span></span>

<span data-ttu-id="63173-138">**Virtuální počítače v nastavení dostupnosti se může skládat z kombinace spravovanými a nespravovanými disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-138">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="63173-139">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-139">No.</span></span> <span data-ttu-id="63173-140">Hello virtuálních počítačů v nastavení dostupnosti musí používat disky všechny spravované nebo nespravované všechny disky.</span><span class="sxs-lookup"><span data-stu-id="63173-140">hello VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="63173-141">Když vytvoříte skupinu dostupnosti, můžete typu disky, kterého chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="63173-141">When you create an availability set, you can choose which type of disks you want toouse.</span></span>

<span data-ttu-id="63173-142">**Spravované disky hello výchozí možnost je ve hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="63173-142">**Is Managed Disks hello default option in hello Azure portal?**</span></span>

<span data-ttu-id="63173-143">Aktuálně ale bude výchozí hello v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="63173-143">Not currently, but it will become hello default in hello future.</span></span>

<span data-ttu-id="63173-144">**Můžete vytvořit prázdný disk spravované?**</span><span class="sxs-lookup"><span data-stu-id="63173-144">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="63173-145">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-145">Yes.</span></span> <span data-ttu-id="63173-146">Můžete vytvořit prázdný disk.</span><span class="sxs-lookup"><span data-stu-id="63173-146">You can create an empty disk.</span></span> <span data-ttu-id="63173-147">Spravovaný disk lze vytvořit nezávisle na virtuální počítač, například bez jeho připojení tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="63173-147">A managed disk can be created independently of a VM, for example, without attaching it tooa VM.</span></span>

<span data-ttu-id="63173-148">**Co je podporováno hello počet domén selhání pro skupinu dostupnosti, který používá disky spravované?**</span><span class="sxs-lookup"><span data-stu-id="63173-148">**What is hello supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="63173-149">V závislosti na hello oblasti, kde se nachází skupina dostupnosti hello, který používá disky spravovat je počet domén selhání hello podporované 2 nebo 3.</span><span class="sxs-lookup"><span data-stu-id="63173-149">Depending on hello region where hello availability set that uses Managed Disks is located, hello supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="63173-150">**Jak je hello standardní účet úložiště pro diagnostiku nastavení?**</span><span class="sxs-lookup"><span data-stu-id="63173-150">**How is hello standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="63173-151">Nastavit účet privátní úložiště pro diagnostiku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="63173-151">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="63173-152">V budoucích hello, plánujeme tooswitch diagnostiky i tooManaged disky.</span><span class="sxs-lookup"><span data-stu-id="63173-152">In hello future, we plan tooswitch diagnostics tooManaged Disks as well.</span></span>

<span data-ttu-id="63173-153">**Jaký druh řízení přístupu na základě Role podpora je k dispozici pro spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-153">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="63173-154">Spravované disky podporuje tři klíčové výchozích rolí:</span><span class="sxs-lookup"><span data-stu-id="63173-154">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="63173-155">Vlastník: Můžou spravovat všechno včetně přístupu</span><span class="sxs-lookup"><span data-stu-id="63173-155">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="63173-156">Přispěvatel: Můžou spravovat všechno kromě přístupu</span><span class="sxs-lookup"><span data-stu-id="63173-156">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="63173-157">Čtečka: Zobrazit vše, co, ale nelze provádět změny</span><span class="sxs-lookup"><span data-stu-id="63173-157">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="63173-158">**Existuje způsob, že umožní kopírování a exportovat účet spravovaných disků na tooa privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="63173-158">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="63173-159">Jen pro čtení sdíleného přístupového podpisu identifikátor URI pro hello spravované disk a použít ho můžete získat toocopy hello obsah tooa privátní úložiště účet nebo místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="63173-159">You can get a read-only shared access signature URI for hello managed disk and use it toocopy hello contents tooa private storage account or on-premises storage.</span></span>

<span data-ttu-id="63173-160">**Můžete vytvořit kopii Moje spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="63173-160">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="63173-161">Zákazníci můžete pořízení snímku jejich spravované disky a pak použít hello snímku toocreate spravovaných disků na jiný.</span><span class="sxs-lookup"><span data-stu-id="63173-161">Customers can take a snapshot of their managed disks and then use hello snapshot toocreate another managed disk.</span></span>

<span data-ttu-id="63173-162">**Jsou nespravované disky stále podporovány?**</span><span class="sxs-lookup"><span data-stu-id="63173-162">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="63173-163">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-163">Yes.</span></span> <span data-ttu-id="63173-164">Podporujeme nespravované a spravované disky.</span><span class="sxs-lookup"><span data-stu-id="63173-164">We support unmanaged and managed disks.</span></span> <span data-ttu-id="63173-165">Doporučujeme používat spravované disky pro nové úlohy a vaše aktuální toomanaged disky úlohy migrace.</span><span class="sxs-lookup"><span data-stu-id="63173-165">We recommend that you use managed disks for new workloads and migrate your current workloads toomanaged disks.</span></span>


<span data-ttu-id="63173-166">**Je-li vytvořit 128 GB disk a poté zvýšit hello velikost too130 GB, bude I vám účtována hello další velikost disku (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="63173-166">**If I create a 128-GB disk and then increase hello size too130 GB, will I be charged for hello next disk size (512 GB)?**</span></span>

<span data-ttu-id="63173-167">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-167">Yes.</span></span>

<span data-ttu-id="63173-168">**Můžete vytvořit místně redundantní úložiště, geograficky redundantní úložiště, a zónově redundantní úložiště spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-168">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="63173-169">Disky systému Azure spravované aktuálně podporuje pouze místně redundantního úložiště spravované disky.</span><span class="sxs-lookup"><span data-stu-id="63173-169">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="63173-170">**Můžete zmenšit nebo downsize Moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-170">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="63173-171">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-171">No.</span></span> <span data-ttu-id="63173-172">Tato funkce není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="63173-172">This feature is not supported currently.</span></span> 

<span data-ttu-id="63173-173">**Můžete změnit vlastnost název počítače hello, pokud specializované (ne vytvořili pomocí nástroje pro přípravu systému hello nebo zobecněn) operačního systému disku jsou použité tooprovision virtuálního počítače?**</span><span class="sxs-lookup"><span data-stu-id="63173-173">**Can I change hello computer name property when a specialized (not created by using hello System Preparation tool or generalized) operating system disk is used tooprovision a VM?**</span></span>

<span data-ttu-id="63173-174">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-174">No.</span></span> <span data-ttu-id="63173-175">Nelze aktualizovat vlastnosti název počítače hello.</span><span class="sxs-lookup"><span data-stu-id="63173-175">You can't update hello computer name property.</span></span> <span data-ttu-id="63173-176">Hello nový virtuální počítač zdědí ho hello nadřazený virtuální počítač, který byl disk operačního systému použít toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="63173-176">hello new VM inherits it from hello parent VM, which was used toocreate hello operating system disk.</span></span> 

<span data-ttu-id="63173-177">**Kde najdu ukázka Azure Resource Manager šablony toocreate virtuálních počítačů s spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-177">**Where can I find sample Azure Resource Manager templates toocreate VMs with managed disks?**</span></span>
* [<span data-ttu-id="63173-178">Seznam šablon pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="63173-178">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="63173-179">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="63173-179">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="63173-180">Spravované disky a šifrování služby úložiště</span><span class="sxs-lookup"><span data-stu-id="63173-180">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="63173-181">**Šifrování služby úložiště Azure ve výchozím nastavení zapnutá po vytvoření se spravovaným diskem?**</span><span class="sxs-lookup"><span data-stu-id="63173-181">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="63173-182">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-182">Yes.</span></span>

<span data-ttu-id="63173-183">**Kdo spravuje hello šifrovacích klíčů?**</span><span class="sxs-lookup"><span data-stu-id="63173-183">**Who manages hello encryption keys?**</span></span>

<span data-ttu-id="63173-184">Spravuje Microsoft hello šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="63173-184">Microsoft manages hello encryption keys.</span></span>

<span data-ttu-id="63173-185">**Můžete zakázat šifrování služby úložiště pro moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-185">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="63173-186">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-186">No.</span></span>

<span data-ttu-id="63173-187">**Je šifrování služby úložiště k dispozici pouze v určitých oblastí?**</span><span class="sxs-lookup"><span data-stu-id="63173-187">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="63173-188">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-188">No.</span></span> <span data-ttu-id="63173-189">Je k dispozici ve všech oblastech hello, kde je k dispozici spravované disky.</span><span class="sxs-lookup"><span data-stu-id="63173-189">It's available in all hello regions where Managed Disks is available.</span></span> <span data-ttu-id="63173-190">Spravované disků je k dispozici ve všech veřejných oblastí a Německu.</span><span class="sxs-lookup"><span data-stu-id="63173-190">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="63173-191">**Jak můžete zjistit, pokud je zašifrovaná Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="63173-191">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="63173-192">Můžete zjistit hello čas vytvoření spravovaného disku z hello portálu Azure, hello rozhraní příkazového řádku Azure a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="63173-192">You can find out hello time when a managed disk was created from hello Azure portal, hello Azure CLI, and PowerShell.</span></span> <span data-ttu-id="63173-193">Pokud je doba hello po 9. června 2017, se šifrují na disku.</span><span class="sxs-lookup"><span data-stu-id="63173-193">If hello time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="63173-194">**Jak můžete šifrovat Můj stávající disky, které byly vytvořeny před 10 června 2017?**</span><span class="sxs-lookup"><span data-stu-id="63173-194">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="63173-195">Od verze 10 června 2017 je nová data tooexisting spravované disky šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="63173-195">As of June 10, 2017, new data written tooexisting managed disks is automatically encrypted.</span></span> <span data-ttu-id="63173-196">Můžeme také plánování tooencrypt stávající data a šifrování hello proběhne asynchronně hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="63173-196">We are also planning tooencrypt existing data, and hello encryption will happen asynchronously in hello background.</span></span> <span data-ttu-id="63173-197">Pokud je nutné šifrovat všechna existující data, vytvoří se kopie disku.</span><span class="sxs-lookup"><span data-stu-id="63173-197">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="63173-198">Bude se šifrovat nové disky.</span><span class="sxs-lookup"><span data-stu-id="63173-198">New disks will be encrypted.</span></span>

* [<span data-ttu-id="63173-199">Kopírovat spravovaných disků pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="63173-199">Copy managed disks by using hello Azure CLI</span></span>](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="63173-200">Zkopírujte spravované disky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="63173-200">Copy managed disks by using PowerShell</span></span>](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="63173-201">**Jsou spravované snímky a bitové kopie zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="63173-201">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="63173-202">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-202">Yes.</span></span> <span data-ttu-id="63173-203">Všechny spravované snímky a bitové kopie vytvořené po 9. června 2017, se šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="63173-203">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="63173-204">**Můžete převést virtuální počítače s nespravované disky, které jsou umístěny v účtech úložiště, které jsou nebo byly dříve šifrované toomanaged disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-204">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted toomanaged disks?**</span></span>

<span data-ttu-id="63173-205">Ano</span><span class="sxs-lookup"><span data-stu-id="63173-205">Yes</span></span>

<span data-ttu-id="63173-206">**Budou exportovaného virtuálního pevného disku z spravovaného disku nebo snímek také zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="63173-206">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="63173-207">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-207">No.</span></span> <span data-ttu-id="63173-208">Pokud exportujete tooan virtuálního pevného disku, ale šifrované účet úložiště z šifrované spravovaného disku nebo snímek a potom je zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="63173-208">But if you export a VHD tooan encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="63173-209">Pro prémiové disky: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="63173-209">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="63173-210">**Pokud virtuální počítač používá velikost série, která podporuje službu Premium Storage, jako je například DSv2, můžu připojit premium a standard datové disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-210">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="63173-211">Ano.</span><span class="sxs-lookup"><span data-stu-id="63173-211">Yes.</span></span>

<span data-ttu-id="63173-212">**Můžete připojit premium a standard data disky tooa velikost řady, která nepodporuje Storage úrovně Premium, jako je například D Dv2, G nebo F řady?**</span><span class="sxs-lookup"><span data-stu-id="63173-212">**Can I attach both premium and standard data disks tooa size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="63173-213">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-213">No.</span></span> <span data-ttu-id="63173-214">Můžete připojit pouze disky tooVMs standardní data, která nepoužívají velikost série, která podporuje službu Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="63173-214">You can attach only standard data disks tooVMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="63173-215">**Když vytvořím premium datový disk z existujícího VHD, který byl 80 GB, kolik bude která stojí?**</span><span class="sxs-lookup"><span data-stu-id="63173-215">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="63173-216">Datový disk premium vytvořen z disku VHD 80 GB je považována za hello k dispozici další premium disku velikost, která je P10 disk.</span><span class="sxs-lookup"><span data-stu-id="63173-216">A premium data disk created from an 80-GB VHD is treated as hello next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="63173-217">Se vám účtovat podle toohello P10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="63173-217">You're charged according toohello P10 disk pricing.</span></span>

<span data-ttu-id="63173-218">**Existují transakce náklady toouse Premium Storage?**</span><span class="sxs-lookup"><span data-stu-id="63173-218">**Are there transaction costs toouse Premium Storage?**</span></span>

<span data-ttu-id="63173-219">Není opravené náklady pro každou velikost disku, která pochází zřízené pomocí omezení na IOPS a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="63173-219">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="63173-220">Hello další náklady jsou šířky odchozího pásma a kapacity snímku, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="63173-220">hello other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="63173-221">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="63173-221">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="63173-222">**Jaká jsou omezení hello IOPS a propustnosti, kterou můžete dostat z hello diskové mezipaměti?**</span><span class="sxs-lookup"><span data-stu-id="63173-222">**What are hello limits for IOPS and throughput that I can get from hello disk cache?**</span></span>

<span data-ttu-id="63173-223">Hello kombinované omezení pro mezipaměť a místní SSD pro řady DS 4000 IOPS na základní a 33 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="63173-223">hello combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="63173-224">Hello GS řady nabízí 5 000 IOPS na základní a 50 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="63173-224">hello GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="63173-225">**Je hello nepodporuje místní SSD pro virtuální počítač spravovaný disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-225">**Is hello local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="63173-226">Hello místní SSD je dočasné úložiště, která je součástí spravované virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="63173-226">hello local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="63173-227">Existuje nejsou zpoplatněné pro toto dočasný úložiště.</span><span class="sxs-lookup"><span data-stu-id="63173-227">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="63173-228">Doporučujeme vám, že nepoužijete tento místní SSD toostore data aplikací protože není trvalý v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="63173-228">We recommend that you do not use this local SSD toostore your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="63173-229">**Jsou existuje žádné dopady na hello použít Trim na prémiové disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-229">**Are there any repercussions for hello use of TRIM on premium disks?**</span></span>

<span data-ttu-id="63173-230">Neexistuje žádné nevýhodou použití toohello TRIM na disky Azure na buď premium nebo standardní disky.</span><span class="sxs-lookup"><span data-stu-id="63173-230">There is no downside toohello use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="63173-231">Nové velikosti disků: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="63173-231">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="63173-232">**Co je největší velikost disku hello podporovaná pro operační systém a datové disky?**</span><span class="sxs-lookup"><span data-stu-id="63173-232">**What is hello largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="63173-233">Typ Hello oddílu, který podporuje Azure pro disk operačního systému je hello hlavní spouštěcí záznam (MBR).</span><span class="sxs-lookup"><span data-stu-id="63173-233">hello partition type that Azure supports for an operating system disk is hello master boot record (MBR).</span></span> <span data-ttu-id="63173-234">formát MBR Hello podporuje velikost disku až too2 TB.</span><span class="sxs-lookup"><span data-stu-id="63173-234">hello MBR format supports a disk size up too2 TB.</span></span> <span data-ttu-id="63173-235">Hello největší velikost, který podporuje Azure pro disk operačního systému je 2 TB.</span><span class="sxs-lookup"><span data-stu-id="63173-235">hello largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="63173-236">Azure podporuje až too4 TB pro datové disky.</span><span class="sxs-lookup"><span data-stu-id="63173-236">Azure supports up too4 TB for data disks.</span></span> 

<span data-ttu-id="63173-237">**Co je hello největší stránky velikost objektu blob podporovanou?**</span><span class="sxs-lookup"><span data-stu-id="63173-237">**What is hello largest page blob size that's supported?**</span></span>

<span data-ttu-id="63173-238">Hello největší stránky velikost objektu blob podporující Azure je 8 TB (8 191 GB).</span><span class="sxs-lookup"><span data-stu-id="63173-238">hello largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="63173-239">Nepodporujeme objekty BLOB stránky větší než 4 TB (4095 GB) připojené tooa virtuálního počítače jako data nebo disky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="63173-239">We don't support page blobs larger than 4 TB (4,095 GB) attached tooa VM as data or operating system disks.</span></span>

<span data-ttu-id="63173-240">**Potřebuji toouse novou verzí z nástroje Azure toocreate připojit, přizpůsobit a odeslat disky, které jsou větší než 1 TB?**</span><span class="sxs-lookup"><span data-stu-id="63173-240">**Do I need toouse a new version of Azure tools toocreate, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="63173-241">Nepotřebujete tooupgrade vaší existující toocreate nástroje Azure, připojit nebo změna velikosti disků, které jsou větší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="63173-241">You don't need tooupgrade your existing Azure tools toocreate, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="63173-242">tooupload svůj disk VHD souboru z místní přímo tooAzure jako objekt blob stránky nebo nespravované disku, bude nutné toouse hello nejnovější sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="63173-242">tooupload your VHD file from on-premises directly tooAzure as a page blob or unmanaged disk, you need toouse hello latest tool sets:</span></span>

|<span data-ttu-id="63173-243">Nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="63173-243">Azure tools</span></span>      | <span data-ttu-id="63173-244">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="63173-244">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="63173-245">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="63173-245">Azure PowerShell</span></span> | <span data-ttu-id="63173-246">Číslo verze 4.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="63173-246">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="63173-247">Rozhraní příkazového řádku Azure v1</span><span class="sxs-lookup"><span data-stu-id="63173-247">Azure CLI v1</span></span>     | <span data-ttu-id="63173-248">Číslo verze 0.10.13: pravděpodobně 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="63173-248">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="63173-249">AzCopy</span><span class="sxs-lookup"><span data-stu-id="63173-249">AzCopy</span></span>           | <span data-ttu-id="63173-250">Číslo verze 6.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="63173-250">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="63173-251">Podpora Hello v2 rozhraní příkazového řádku Azure a Azure Storage Explorer tu bude brzo dostupná.</span><span class="sxs-lookup"><span data-stu-id="63173-251">hello support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="63173-252">**Jsou podporovány P4 a P6 velikosti disků pro nespravovaná disky nebo objekty BLOB stránky?**</span><span class="sxs-lookup"><span data-stu-id="63173-252">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="63173-253">Ne.</span><span class="sxs-lookup"><span data-stu-id="63173-253">No.</span></span> <span data-ttu-id="63173-254">P4 (32 GB) a P6 velikosti disků (64 GB) jsou podporovány pouze u spravovaných disky.</span><span class="sxs-lookup"><span data-stu-id="63173-254">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="63173-255">Podpora pro nespravovaná disky a objekty BLOB stránky bude brzo.</span><span class="sxs-lookup"><span data-stu-id="63173-255">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="63173-256">**Pokud mé existující premium spravované disku menší než 64 GB byl vytvořen před povolením malý disk hello (přibližně 15. června 2017), jak se jeho fakturuje?**</span><span class="sxs-lookup"><span data-stu-id="63173-256">**If my existing premium managed disk less than 64 GB was created before hello small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="63173-257">Stávající malých prémiové disky menší než 64 GB pokračovat toobe účtují podle toohello P10 cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="63173-257">Existing small premium disks less than 64 GB continue toobe billed according toohello P10 pricing tier.</span></span> 

<span data-ttu-id="63173-258">**Jak můžete přepnout hello disku úroveň malé prémiové disky menší než 64 GB z P10 tooP4 nebo P6?**</span><span class="sxs-lookup"><span data-stu-id="63173-258">**How can I switch hello disk tier of small premium disks less than 64 GB from P10 tooP4 or P6?**</span></span>

<span data-ttu-id="63173-259">Může pořízení snímku menší disky a poté vytvořit hello disku tooautomatically přepínač cenová úroveň tooP4 nebo P6 podle velikosti hello zřízený.</span><span class="sxs-lookup"><span data-stu-id="63173-259">You can take a snapshot of your small disks and then create a disk tooautomatically switch hello pricing tier tooP4 or P6 based on hello provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="63173-260">Co když není zde odpovědi můj dotaz?</span><span class="sxs-lookup"><span data-stu-id="63173-260">What if my question isn't answered here?</span></span>

<span data-ttu-id="63173-261">Pokud váš dotaz není zde uvedeno, dejte nám vědět, a pomůžeme vám najít odpověď.</span><span class="sxs-lookup"><span data-stu-id="63173-261">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="63173-262">V komentářích hello můžete odeslat dotaz na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="63173-262">You can post a question at hello end of this article in hello comments.</span></span> <span data-ttu-id="63173-263">tooengage týmem hello Azure Storage a ostatními členy komunity o tomto článku použít hello MSDN [fórum pro Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="63173-263">tooengage with hello Azure Storage team and other community members about this article, use hello MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="63173-264">Funkce toorequest odeslání vaší žádosti a nápady toohello [fóru pro zpětnou vazbu Azure Storage](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="63173-264">toorequest features, submit your requests and ideas toohello [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
