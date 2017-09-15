<span data-ttu-id="92bcf-101">Když už nepotřebujete datový disk připojený k virtuálnímu počítači, můžete ho jednoduše odpojit.</span><span class="sxs-lookup"><span data-stu-id="92bcf-101">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="92bcf-102">Odpojení způsobí odebrání disku z virtuálního počítače, ale neodstraní disk z účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="92bcf-102">Detaching a disk removes the disk from the virtual machine, but doesn't delete the disk from the Azure storage account.</span></span>

<span data-ttu-id="92bcf-103">Pokud znovu chcete použít stávající data na disku, můžete ho znovu připojit ke stejnému nebo jinému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="92bcf-103">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="92bcf-104">Pokud chcete odpojit disk operačního systému, musíte nejdřív odstranit příslušný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="92bcf-104">To detach an operating system disk, you first need to delete the virtual machine.</span></span>
>

## <a name="find-the-disk"></a><span data-ttu-id="92bcf-105">Vyhledání disku</span><span class="sxs-lookup"><span data-stu-id="92bcf-105">Find the disk</span></span>
<span data-ttu-id="92bcf-106">Pokud neznáte název disku nebo ho chcete před odpojením ověřit, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="92bcf-106">If you don't know the name of the disk or want to verify it before you detach it, follow these steps.</span></span>

1. <span data-ttu-id="92bcf-107">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="92bcf-107">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="92bcf-108">Klikněte na **Virtuální počítače** a potom vyberte příslušný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="92bcf-108">Click **Virtual Machines**, and then select the appropriate VM.</span></span>

3. <span data-ttu-id="92bcf-109">Na levé straně řídicího panelu virtuálního počítače v části **Nastavení** klikněte na **Disky**.</span><span class="sxs-lookup"><span data-stu-id="92bcf-109">Click **Disks** along the left edge of the virtual machine dashboard, under **Settings**.</span></span>

 <span data-ttu-id="92bcf-110">Na řídicím panelu virtuálního počítače se zobrazuje název a typ všech připojených disků.</span><span class="sxs-lookup"><span data-stu-id="92bcf-110">The virtual machine dashboard lists the name and type of all attached disks.</span></span> <span data-ttu-id="92bcf-111">Třeba tato obrazovka ukazuje virtuální počítač s jedním diskem operačního systému (OS) a jedním datovým diskem:</span><span class="sxs-lookup"><span data-stu-id="92bcf-111">For example, this screen shows a virtual machine with one operating system (OS) disk and one data disk:</span></span>

    ![Vyhledání datového disku](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a><span data-ttu-id="92bcf-113">Odpojení disku</span><span class="sxs-lookup"><span data-stu-id="92bcf-113">Detach the disk</span></span>
1. <span data-ttu-id="92bcf-114">Na webu Azure Portal klikněte na **Virtual Machines** a potom klikněte na název virtuálního počítače s datovým diskem, který chcete odpojit.</span><span class="sxs-lookup"><span data-stu-id="92bcf-114">From the Azure portal, click **Virtual Machines**, and then click the name of the virtual machine that has the data disk you want to detach.</span></span>

2. <span data-ttu-id="92bcf-115">Na levé straně řídicího panelu virtuálního počítače v části **Nastavení** klikněte na **Disky**.</span><span class="sxs-lookup"><span data-stu-id="92bcf-115">Click **Disks** along the left edge of the virtual machine dashboard, under **Settings**.</span></span>

3. <span data-ttu-id="92bcf-116">Klikněte na disk, který chcete odpojit.</span><span class="sxs-lookup"><span data-stu-id="92bcf-116">Click the disk you want to detach.</span></span>

  ![Určení odpojovaného disku](./media/howto-detach-disk-windows-linux/disklist.png)

4. <span data-ttu-id="92bcf-118">Na panelu příkazů klikněte na **Odpojit**.</span><span class="sxs-lookup"><span data-stu-id="92bcf-118">From the command bar, click **Detach**.</span></span>

  ![Vyhledání příkazu pro odpojení](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. <span data-ttu-id="92bcf-120">Kliknutím na **Ano** v potvrzovacím okně odpojíte příslušný disk.</span><span class="sxs-lookup"><span data-stu-id="92bcf-120">In the confirmation window, click **Yes** to detach the disk.</span></span>

  ![Potvrzení odpojení disku](./media/howto-detach-disk-windows-linux/confirmdetach.png)

<span data-ttu-id="92bcf-122">Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="92bcf-122">The disk remains in storage but is no longer attached to a virtual machine.</span></span>
