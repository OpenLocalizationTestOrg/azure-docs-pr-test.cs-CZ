<span data-ttu-id="f41b6-101">Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač, můžete ho snadno odpojit.</span><span class="sxs-lookup"><span data-stu-id="f41b6-101">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="f41b6-102">Disk se odpojuje odebere hello disk z virtuálního počítače hello, ale neodstraní hello disku z hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f41b6-102">Detaching a disk removes hello disk from hello virtual machine, but doesn't delete hello disk from hello Azure storage account.</span></span>

<span data-ttu-id="f41b6-103">Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.</span><span class="sxs-lookup"><span data-stu-id="f41b6-103">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="f41b6-104">toodetach disk operačního systému, musíte nejdřív toodelete hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f41b6-104">toodetach an operating system disk, you first need toodelete hello virtual machine.</span></span>
>

## <a name="find-hello-disk"></a><span data-ttu-id="f41b6-105">Najít hello disk</span><span class="sxs-lookup"><span data-stu-id="f41b6-105">Find hello disk</span></span>
<span data-ttu-id="f41b6-106">Pokud si nejste jisti hello název hello disku nebo chcete tooverify ji předtím, než jej odpojte, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="f41b6-106">If you don't know hello name of hello disk or want tooverify it before you detach it, follow these steps.</span></span>

1. <span data-ttu-id="f41b6-107">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f41b6-107">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="f41b6-108">Klikněte na tlačítko **virtuální počítače**, a pak vyberte hello odpovídající virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f41b6-108">Click **Virtual Machines**, and then select hello appropriate VM.</span></span>

3. <span data-ttu-id="f41b6-109">Klikněte na tlačítko **disky** podél hello levá strana řídicí panel hello virtuálního počítače, v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="f41b6-109">Click **Disks** along hello left edge of hello virtual machine dashboard, under **Settings**.</span></span>

 <span data-ttu-id="f41b6-110">virtuální počítač Hello řídicí panel uvádí hello název a typ všech připojených discích.</span><span class="sxs-lookup"><span data-stu-id="f41b6-110">hello virtual machine dashboard lists hello name and type of all attached disks.</span></span> <span data-ttu-id="f41b6-111">Třeba tato obrazovka ukazuje virtuální počítač s jedním diskem operačního systému (OS) a jedním datovým diskem:</span><span class="sxs-lookup"><span data-stu-id="f41b6-111">For example, this screen shows a virtual machine with one operating system (OS) disk and one data disk:</span></span>

    ![Vyhledání datového disku](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-hello-disk"></a><span data-ttu-id="f41b6-113">Odpojte hello disk</span><span class="sxs-lookup"><span data-stu-id="f41b6-113">Detach hello disk</span></span>
1. <span data-ttu-id="f41b6-114">Z hello portálu Azure, klikněte na tlačítko **virtuální počítače**a potom klikněte na název hello hello virtuálního počítače, který má datový disk hello chcete toodetach.</span><span class="sxs-lookup"><span data-stu-id="f41b6-114">From hello Azure portal, click **Virtual Machines**, and then click hello name of hello virtual machine that has hello data disk you want toodetach.</span></span>

2. <span data-ttu-id="f41b6-115">Klikněte na tlačítko **disky** podél hello levá strana řídicí panel hello virtuálního počítače, v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="f41b6-115">Click **Disks** along hello left edge of hello virtual machine dashboard, under **Settings**.</span></span>

3. <span data-ttu-id="f41b6-116">Klikněte na disk hello chcete toodetach.</span><span class="sxs-lookup"><span data-stu-id="f41b6-116">Click hello disk you want toodetach.</span></span>

  ![Identifikovat toodetach disku hello](./media/howto-detach-disk-windows-linux/disklist.png)

4. <span data-ttu-id="f41b6-118">Na panelu příkazů hello, klikněte na **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="f41b6-118">From hello command bar, click **Detach**.</span></span>

  ![Vyhledejte hello detach – příkaz](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. <span data-ttu-id="f41b6-120">V okně potvrzení hello klikněte **Ano** toodetach hello disku.</span><span class="sxs-lookup"><span data-stu-id="f41b6-120">In hello confirmation window, click **Yes** toodetach hello disk.</span></span>

  ![Potvrďte odpojuje hello disku](./media/howto-detach-disk-windows-linux/confirmdetach.png)

<span data-ttu-id="f41b6-122">Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f41b6-122">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>
