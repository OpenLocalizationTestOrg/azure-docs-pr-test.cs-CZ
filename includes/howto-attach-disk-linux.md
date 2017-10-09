
<span data-ttu-id="bbf43-101">Další informace o discích najdete v tématu věnovaném [diskům a virtuálním pevným diskům pro virtuální počítače](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bbf43-101">For more information about disks, see [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a><span data-ttu-id="bbf43-102">Připojení prázdného disku</span><span class="sxs-lookup"><span data-stu-id="bbf43-102">Attach an empty disk</span></span>
1. <span data-ttu-id="bbf43-103">Otevřete Azure CLI 1.0 a [připojit tooyour předplatné](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="bbf43-103">Open Azure CLI 1.0 and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="bbf43-104">Zkontrolujte, že jste v režimu Azure Service Management (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="bbf43-104">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="bbf43-105">Zadejte `azure vm disk attach-new` toocreate a připojit nový disk, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="bbf43-105">Enter `azure vm disk attach-new` toocreate and attach a new disk as shown in hello following example.</span></span> <span data-ttu-id="bbf43-106">Nahraďte *Můjvp* s názvem hello systému Linux virtuálního počítače a zadejte velikost hello hello disku v GB, což je *100GB* v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="bbf43-106">Replace *myVM* with hello name of your Linux Virtual Machine and specify hello size of hello disk in GB, which is *100GB* in this example:</span></span>

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. <span data-ttu-id="bbf43-107">Jakmile hello datový disk se vytvoří a připojené, je uvedena ve výstup hello `azure vm disk list <virtual-machine-name>` jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="bbf43-107">After hello data disk is created and attached, it's listed in hello output of `azure vm disk list <virtual-machine-name>` as shown in hello following example:</span></span>
   
    ```azurecli
    azure vm disk list TestVM
    ```

    <span data-ttu-id="bbf43-108">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bbf43-108">hello output is similar toohello following example:</span></span>

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a><span data-ttu-id="bbf43-109">Připojení stávajícího disku</span><span class="sxs-lookup"><span data-stu-id="bbf43-109">Attach an existing disk</span></span>
<span data-ttu-id="bbf43-110">Připojení stávajícího disku vyžaduje, aby v účtu úložiště byl dostupný soubor .vhd.</span><span class="sxs-lookup"><span data-stu-id="bbf43-110">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span>

1. <span data-ttu-id="bbf43-111">Otevřete Azure CLI 1.0 a [připojit tooyour předplatné](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="bbf43-111">Open Azure CLI 1.0 and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="bbf43-112">Zkontrolujte, že jste v režimu Azure Service Management (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="bbf43-112">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="bbf43-113">Zkontrolujte, pokud hello virtuálního pevného disku, které chcete tooattach, je již nahrán tooyour předplatné Azure:</span><span class="sxs-lookup"><span data-stu-id="bbf43-113">Check if hello VHD you want tooattach is already uploaded tooyour Azure subscription:</span></span>
   
    ```azurecli
    azure vm disk list
    ```

    <span data-ttu-id="bbf43-114">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bbf43-114">hello output is similar toohello following example:</span></span>

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. <span data-ttu-id="bbf43-115">Pokud nenajdete hello disku, při němž toouse, místní předplatné tooyour virtuální pevný disk může odeslat pomocí `azure vm disk create` nebo `azure vm disk upload`.</span><span class="sxs-lookup"><span data-stu-id="bbf43-115">If you don't find hello disk that you want toouse, you may upload a local VHD tooyour subscription by using `azure vm disk create` or `azure vm disk upload`.</span></span> <span data-ttu-id="bbf43-116">Příklad `disk create` by jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bbf43-116">An example of `disk create` would be as in hello following example:</span></span>
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    <span data-ttu-id="bbf43-117">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bbf43-117">hello output is similar toohello following example:</span></span>

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   <span data-ttu-id="bbf43-118">Můžete také používat `azure vm disk upload` tooupload účet virtuálního pevného disku tooa konkrétní úložiště.</span><span class="sxs-lookup"><span data-stu-id="bbf43-118">You may also use `azure vm disk upload` tooupload a VHD tooa specific storage account.</span></span> <span data-ttu-id="bbf43-119">Přečtěte si další informace o hello příkazy toomanage vaše datových disků virtuálního počítače Azure [přes zde](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="bbf43-119">Read more about hello commands toomanage your Azure virtual machine data disks [over here](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

4. <span data-ttu-id="bbf43-120">Teď můžete připojit hello potřeby virtuálního pevného disku tooyour virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="bbf43-120">Now you attach hello desired VHD tooyour virtual machine:</span></span>
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   <span data-ttu-id="bbf43-121">Ujistěte se, že tooreplace *Můjvp* s názvem hello virtuálního počítače, a *myVHD* s vaší požadované virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bbf43-121">Make sure tooreplace *myVM* with hello name of your virtual machine, and *myVHD* with your desired VHD.</span></span>

5. <span data-ttu-id="bbf43-122">Můžete ověřit hello disk je připojený toohello virtuální počítač s `azure vm disk list <virtual-machine-name>`:</span><span class="sxs-lookup"><span data-stu-id="bbf43-122">You can verify hello disk is attached toohello virtual machine with `azure vm disk list <virtual-machine-name>`:</span></span>
   
    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="bbf43-123">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bbf43-123">hello output is similar toohello following example:</span></span>

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> <span data-ttu-id="bbf43-124">Po přidání datový disk, budete potřebovat toolog toohello virtuálního počítače a inicializaci hello disku, aby hello virtuálního počítače můžete použít hello disku pro úložiště (viz hello následující kroky pro další informace o tom, jak toodo inicializovat hello disk).</span><span class="sxs-lookup"><span data-stu-id="bbf43-124">After you add a data disk, you'll need toolog on toohello virtual machine and initialize hello disk so hello virtual machine can use hello disk for storage (see hello following steps for more information on how toodo initialize hello disk).</span></span>
> 
> 

