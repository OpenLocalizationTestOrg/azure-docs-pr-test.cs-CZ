<span data-ttu-id="34a9a-101">Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač (VM), můžete ho snadno odpojit.</span><span class="sxs-lookup"><span data-stu-id="34a9a-101">When you no longer need a data disk that's attached tooa virtual machine (VM), you can easily detach it.</span></span> <span data-ttu-id="34a9a-102">Při odpojení disku z hello virtuálního počítače není hello disk odebrat z úložiště.</span><span class="sxs-lookup"><span data-stu-id="34a9a-102">When you detach a disk from hello VM, hello disk is not removed it from storage.</span></span> <span data-ttu-id="34a9a-103">Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.</span><span class="sxs-lookup"><span data-stu-id="34a9a-103">If you want toouse hello existing data on hello disk again, you can reattach it toohello same VM, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="34a9a-104">Virtuální počítač v Azure používá různé typy disků – disk operačního systému, místní dočasný disk a volitelné datové disky.</span><span class="sxs-lookup"><span data-stu-id="34a9a-104">A VM in Azure uses different types of disks - an operating system disk, a local temporary disk, and optional data disks.</span></span> <span data-ttu-id="34a9a-105">Podrobnosti najdete v tématu [Disky a virtuální pevné disky (VHD) pro virtuální počítače](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="34a9a-105">For details, see [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="34a9a-106">Pokud odstraníte hello virtuálních počítačů, nelze odpojit disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="34a9a-106">You cannot detach an operating system disk unless you also delete hello VM.</span></span>

## <a name="find-hello-disk"></a><span data-ttu-id="34a9a-107">Najít hello disk</span><span class="sxs-lookup"><span data-stu-id="34a9a-107">Find hello disk</span></span>
<span data-ttu-id="34a9a-108">Než můžete odpojit disk z virtuálního počítače je třeba toofind out hello číslo logické jednotky, které je identifikátor pro toobe disku hello odpojit.</span><span class="sxs-lookup"><span data-stu-id="34a9a-108">Before you can detach a disk from a VM you need toofind out hello LUN number, which is an identifier for hello disk toobe detached.</span></span> <span data-ttu-id="34a9a-109">toodo, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="34a9a-109">toodo that, follow these steps:</span></span>

1. <span data-ttu-id="34a9a-110">Otevřete rozhraní příkazového řádku Azure a [připojit tooyour předplatné](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="34a9a-110">Open Azure CLI and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="34a9a-111">Zkontrolujte, že jste v režimu Azure Service Management (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="34a9a-111">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="34a9a-112">Zjistěte, které disky jsou připojené tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="34a9a-112">Find out which disks are attached tooyour VM.</span></span> <span data-ttu-id="34a9a-113">Hello následující příklad vypíše disky pro virtuální počítač s názvem hello `myVM`:</span><span class="sxs-lookup"><span data-stu-id="34a9a-113">hello following example lists disks for hello VM named `myVM`:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="34a9a-114">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="34a9a-114">hello output is similar toohello following example:</span></span>

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. <span data-ttu-id="34a9a-115">Poznámka: hello logické jednotky nebo hello **číslo logické jednotky** hello disku, které chcete toodetach.</span><span class="sxs-lookup"><span data-stu-id="34a9a-115">Note hello LUN or hello **logical unit number** for hello disk that you want toodetach.</span></span>

## <a name="remove-operating-system-references-toohello-disk"></a><span data-ttu-id="34a9a-116">Odeberte disk toohello odkazy operačního systému</span><span class="sxs-lookup"><span data-stu-id="34a9a-116">Remove operating system references toohello disk</span></span>
<span data-ttu-id="34a9a-117">Před odpojením hello disku z hello Linux hosta, měli byste si ověřit, že všechny oddíly v hello disku nejsou používány.</span><span class="sxs-lookup"><span data-stu-id="34a9a-117">Before detaching hello disk from hello Linux guest, you should make sure that all partitions on hello disk are not in use.</span></span> <span data-ttu-id="34a9a-118">Ujistěte se, že hello operační systém nebude pokoušet tooremount je po restartování systému.</span><span class="sxs-lookup"><span data-stu-id="34a9a-118">Ensure that hello operating system does not attempt tooremount them after a reboot.</span></span> <span data-ttu-id="34a9a-119">Tyto kroky vrátit zpět konfigurace hello pravděpodobně jste vytvořili při [připojení](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disku.</span><span class="sxs-lookup"><span data-stu-id="34a9a-119">These steps undo hello configuration you likely created when [attaching](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disk.</span></span>

1. <span data-ttu-id="34a9a-120">Použití hello `lsscsi` identifikátoru příkazu toodiscover hello disku.</span><span class="sxs-lookup"><span data-stu-id="34a9a-120">Use hello `lsscsi` command toodiscover hello disk identifier.</span></span> <span data-ttu-id="34a9a-121">`lsscsi` můžete nainstalovat pomocí příkazu `yum install lsscsi` (v distribucích založených na Red Hat) nebo `apt-get install lsscsi` (v distribucích založených na Debian).</span><span class="sxs-lookup"><span data-stu-id="34a9a-121">`lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="34a9a-122">Můžete najít identifikátor disku hello, hledaný pomocí hello číslo logické jednotky.</span><span class="sxs-lookup"><span data-stu-id="34a9a-122">You can find hello disk identifier you are looking for by using hello LUN number.</span></span> <span data-ttu-id="34a9a-123">Poslední číslo hello řazené kolekce členů v jednotlivých řádcích Hello je hello logické jednotky.</span><span class="sxs-lookup"><span data-stu-id="34a9a-123">hello last number in hello tuple in each row is hello LUN.</span></span> <span data-ttu-id="34a9a-124">V následující ukázka z hello `lsscsi`, logickou jednotku LUN 0 mapuje příliš  */dev/sdc*</span><span class="sxs-lookup"><span data-stu-id="34a9a-124">In hello following example from `lsscsi`, LUN 0 maps too*/dev/sdc*</span></span>

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. <span data-ttu-id="34a9a-125">Použití `fdisk -l <disk>` oddíly hello toodiscover přidružené toobe disku hello odpojit.</span><span class="sxs-lookup"><span data-stu-id="34a9a-125">Use `fdisk -l <disk>` toodiscover hello partitions associated with hello disk toobe detached.</span></span> <span data-ttu-id="34a9a-126">Hello následující příklad ukazuje výstup hello `/dev/sdc`:</span><span class="sxs-lookup"><span data-stu-id="34a9a-126">hello following example shows hello output for `/dev/sdc`:</span></span>

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. <span data-ttu-id="34a9a-127">Odpojte každý oddíl uvedené pro hello disk.</span><span class="sxs-lookup"><span data-stu-id="34a9a-127">Unmount each partition listed for hello disk.</span></span> <span data-ttu-id="34a9a-128">Odpojí technologie Hello následující příklad `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="34a9a-128">hello following example unmounts `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

4. <span data-ttu-id="34a9a-129">Použití hello `blkid` příkaz toodiscovery hello identifikátory UUID pro všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="34a9a-129">Use hello `blkid` command toodiscovery hello UUIDs for all partitions.</span></span> <span data-ttu-id="34a9a-130">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="34a9a-130">hello output is similar toohello following example:</span></span>

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. <span data-ttu-id="34a9a-131">Odebrat položky v hello **/etc/fstab** soubor přidružený hello zařízení cesty nebo identifikátory UUID pro všechny oddíly pro toobe disku hello odpojit.</span><span class="sxs-lookup"><span data-stu-id="34a9a-131">Remove entries in hello **/etc/fstab** file associated with either hello device paths or UUIDs for all partitions for hello disk toobe detached.</span></span>  <span data-ttu-id="34a9a-132">Záznamy pro tento příklad můžou být:</span><span class="sxs-lookup"><span data-stu-id="34a9a-132">Entries for this example might be:</span></span>

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    <span data-ttu-id="34a9a-133">nebo</span><span class="sxs-lookup"><span data-stu-id="34a9a-133">or</span></span>
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-hello-disk"></a><span data-ttu-id="34a9a-134">Odpojte hello disk</span><span class="sxs-lookup"><span data-stu-id="34a9a-134">Detach hello disk</span></span>
<span data-ttu-id="34a9a-135">Po nalezení číslo logické jednotky hello hello disku a odkazy na odebrané hello operačního systému jste připravené toodetach ho:</span><span class="sxs-lookup"><span data-stu-id="34a9a-135">After you find hello LUN number of hello disk and removed hello operating system references, you're ready toodetach it:</span></span>

1. <span data-ttu-id="34a9a-136">Odpojit hello vybraný disk z virtuálního počítače hello spuštěním příkazu hello `azure vm disk detach
   <virtual-machine-name> <LUN>`.</span><span class="sxs-lookup"><span data-stu-id="34a9a-136">Detach hello selected disk from hello virtual machine by running hello command `azure vm disk detach
<virtual-machine-name> <LUN>`.</span></span> <span data-ttu-id="34a9a-137">Hello následující příklad odpojí LUN `0` z hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="34a9a-137">hello following example detaches LUN `0` from hello VM named `myVM`:</span></span>
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. <span data-ttu-id="34a9a-138">Můžete zkontrolovat, pokud tu spuštěním odpojit hello disk `azure vm disk list` znovu.</span><span class="sxs-lookup"><span data-stu-id="34a9a-138">You can check if hello disk got detached by running `azure vm disk list` again.</span></span> <span data-ttu-id="34a9a-139">Následující příklad kontroly Hello hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="34a9a-139">hello following example checks hello VM named `myVM`:</span></span>
   
    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="34a9a-140">Hello výstup je podobné toohello následující příklad, který ukazuje, že je již připojen hello datový disk:</span><span class="sxs-lookup"><span data-stu-id="34a9a-140">hello output is similar toohello following example, which shows hello data disk is no longer attached:</span></span>

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

<span data-ttu-id="34a9a-141">Hello odpojit disk zůstane v úložišti, ale je už připojené tooa virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="34a9a-141">hello detached disk remains in storage but is no longer attached tooa virtual machine.</span></span>

