<span data-ttu-id="fea18-101">Při přidávání datové disky na virtuální počítač s Linuxem, mohou nastat chyby, pokud disk neexistuje na logické jednotce 0.</span><span class="sxs-lookup"><span data-stu-id="fea18-101">When adding data disks to a Linux VM, you may encounter errors if a disk does not exist at LUN 0.</span></span> <span data-ttu-id="fea18-102">Pokud přidáváte ručně pomocí disku `azure vm disk attach-new` příkazu a zadáte logické jednotky (`--lun`) namísto povolení platformy Azure určit příslušnou logickou jednotku LUN, postará že disk již existuje / bude existovat na logické jednotce 0.</span><span class="sxs-lookup"><span data-stu-id="fea18-102">If you are adding a disk manually using the `azure vm disk attach-new` command and you specify a LUN (`--lun`) rather than allowing the Azure platform to determine the appropriate LUN, take care that a disk already exists / will exist at LUN 0.</span></span> 

<span data-ttu-id="fea18-103">Podívejte se na následující příklad zobrazuje fragment výstup `lsscsi`:</span><span class="sxs-lookup"><span data-stu-id="fea18-103">Consider the following example showing a snippet of the output from `lsscsi`:</span></span>

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

<span data-ttu-id="fea18-104">Existují dvě datové disky na LUN 0 a 1 logická jednotka (první sloupec v `lsscsi` výstup podrobnosti `[host:channel:target:lun]`).</span><span class="sxs-lookup"><span data-stu-id="fea18-104">The two data disks exist at LUN 0 and LUN 1 (the first column in the `lsscsi` output details `[host:channel:target:lun]`).</span></span> <span data-ttu-id="fea18-105">Oba disky by měl být accessbile z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fea18-105">Both disks should be accessbile from within the VM.</span></span> <span data-ttu-id="fea18-106">Pokud byl ručně zadané první disk, který má být přidán na logickou jednotku LUN 1 a druhý disk na logické jednotce 2, nemusíte vidět disky správně ze v rámci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fea18-106">If you had manually specified the first disk to be added at LUN 1 and the second disk at LUN 2, you may not see the disks correctly from within your VM.</span></span>

> [!NOTE]
> <span data-ttu-id="fea18-107">Azure `host` hodnota je 5 v těchto příkladech, ale to se může lišit v závislosti na typu úložiště, které vyberete.</span><span class="sxs-lookup"><span data-stu-id="fea18-107">The Azure `host` value is 5 in these examples, but this may vary depending on the type of storage you select.</span></span>
> 
> 

<span data-ttu-id="fea18-108">Toto chování disku není problém s Azure, ale způsob, ve kterém následuje jádra Linux specifikace SCSI.</span><span class="sxs-lookup"><span data-stu-id="fea18-108">This disk behavior is not an Azure problem, but the way in which the Linux kernel follows the SCSI specifications.</span></span> <span data-ttu-id="fea18-109">Sběrnice SCSI, hledá připojená zařízení prohledávání jádra Linux musí být zařízení nalezen na logickou jednotku 0 v pořadí pro systém pokračujte vyhledávání dalších zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea18-109">When the Linux kernel scans the SCSI bus for attached devices, a device must be found at LUN 0 in order for the system to continue scanning for additional devices.</span></span> <span data-ttu-id="fea18-110">Takto:</span><span class="sxs-lookup"><span data-stu-id="fea18-110">As such:</span></span>

* <span data-ttu-id="fea18-111">Prohlédněte si výstup `lsscsi` po přidání datový disk k ověření, že máte na disk na logické jednotce 0.</span><span class="sxs-lookup"><span data-stu-id="fea18-111">Review the output of `lsscsi` after adding a data disk to verify that you have a disk at LUN 0.</span></span>
* <span data-ttu-id="fea18-112">Pokud disk nezobrazuje správně v rámci virtuálního počítače, ověřte, zda že disk na logické jednotce 0 existuje.</span><span class="sxs-lookup"><span data-stu-id="fea18-112">If your disk does not show up correctly within your VM, verify a disk exists at LUN 0.</span></span>

