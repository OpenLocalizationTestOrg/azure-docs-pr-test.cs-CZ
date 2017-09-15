<!--author=alkohli last changed: 08/16/2016-->

#### <a name="to-create-a-volume"></a><span data-ttu-id="9f3b2-101">Vytvoření svazku</span><span class="sxs-lookup"><span data-stu-id="9f3b2-101">To create a volume</span></span>
1. <span data-ttu-id="9f3b2-102">Na stránce zařízení **Rychlý start** kliknutím na **Přidat svazek** spustíte průvodce přidáním svazku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-102">On the device **Quick Start** page, click **Add a volume** to start the Add a volume wizard.</span></span>
2. <span data-ttu-id="9f3b2-103">V průvodci přidáním svazku v části **Základní nastavení** proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="9f3b2-103">In the Add a volume wizard, under **Basic Settings**:</span></span>
   
   1. <span data-ttu-id="9f3b2-104">Zadejte **Název** svazku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-104">Type a **Name** for your volume.</span></span>
   2. <span data-ttu-id="9f3b2-105">V rozevíracím seznamu **Typ použití** vyberte typ použití svazku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-105">On the drop-down list, select the **Usage Type** for your volume.</span></span> <span data-ttu-id="9f3b2-106">Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte typ svazku **Místně vázaný**.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-106">For workloads that require local guarantees, low latencies, and higher performance, select a **Locally pinned** volume.</span></span> <span data-ttu-id="9f3b2-107">Pro ostatní typy dat vyberte typ svazku **Vrstvený**.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-107">For all other data, select a **Tiered** volume.</span></span> <span data-ttu-id="9f3b2-108">Pokud tento svazek používáte k archivaci dat, zaškrtněte políčko **Použít tento svazek pro archivní data s méně častým přístupem**.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-108">If you are using this volume for archival data, check **Use this volume for less frequently accessed archival data**.</span></span> 
      
       <span data-ttu-id="9f3b2-109">Místně vázaný svazek je tlustě zřízený a zajišťuje, že primární data uložená ve svazku zůstanou uložená v místním zařízení a nebudou distribuována do cloudu.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-109">A locally pinned volume is thickly provisioned and ensures that the primary data on the volume stays local to the device and does not spill to the cloud.</span></span>  <span data-ttu-id="9f3b2-110">Pokud vytvoříte místně vázaný svazek, zařízení zkontroluje dostupné místo v místních vrstvách a zřídí svazek požadované velikosti.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-110">If you create a locally pinned volume, the device checks for available space on the local tiers to provision the volume of the requested size.</span></span> <span data-ttu-id="9f3b2-111">Při operaci vytváření místně vázaného svazku mohou být existující data ze zařízení distribuována do cloudu a vytvoření svazku může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-111">The operation of creating a locally pinned volume may involve spilling existing data from the device to the cloud and the time taken to create the volume may be long.</span></span> <span data-ttu-id="9f3b2-112">Celková doba závisí na velikosti zřizovaného svazku, dostupné šířce pásma sítě a datech uložených v zařízení.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-112">The total time depends on the size of the provisioned volume, available network bandwidth, and the data on your device.</span></span> 
      
       <span data-ttu-id="9f3b2-113">Vrstvený svazek je zřizovaný dynamicky a lze jej rychle vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-113">A tiered volume is thinly provisioned and can be created quickly.</span></span> <span data-ttu-id="9f3b2-114">Výběrem možnosti **Použít tento svazek pro archivní data s méně častým přístupem** u vrstveného svazku určeného k uchovávání archivních dat změníte velikost bloku odstranění duplicit vašeho svazku na 512 kB.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-114">Selecting **Use this volume for less frequently accessed archival data** for tiered volume targeted for archival data changes the deduplication chunk size for your volume to 512 KB.</span></span> <span data-ttu-id="9f3b2-115">Pokud políčko není zaškrtnuté, příslušný vrstvený svazek bude používat velikost bloku 64 kB.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-115">If this field is not checked, the corresponding tiered volume uses a chunk size of 64 KB.</span></span> <span data-ttu-id="9f3b2-116">Větší velikost deduplikačního bloku dat umožňuje zařízení urychlit přenos velkých objemů archivních dat do cloudu.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-116">A larger deduplication chunk size allows the device to expedite the transfer of large archival data to the cloud.</span></span>
   3. <span data-ttu-id="9f3b2-117">Zadejte hodnotu **Zřízená kapacita** svazku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-117">Specify the **Provisioned Capacity** for your volume.</span></span> <span data-ttu-id="9f3b2-118">Poznamenejte si kapacitu dostupnou na základě vybraného typu svazku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-118">Make a note of the capacity that is available based on the volume type selected.</span></span> <span data-ttu-id="9f3b2-119">Zadaná velikost svazku nesmí překročit velikost dostupného místa.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-119">The specified volume size must not exceed the available space.</span></span>
      
       <span data-ttu-id="9f3b2-120">Zařízení 8100 umožňuje zřizovat místně vázané svazky o velikosti až 8.5 TB a vrstvené svazky o velikosti až 200 TB.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-120">You can provision locally pinned volumes up to 8.5 TB or tiered volumes up to 200 TB on the 8100 device.</span></span> <span data-ttu-id="9f3b2-121">Větší zařízení 8600 umožňuje zřizovat místně vázané svazky o velikosti až 22.5 TB a vrstvené svazky o velikosti až 500 TB.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-121">On the larger 8600 device, you can provision locally pinned volumes up to 22.5 TB or tiered volumes up to 500 TB.</span></span> <span data-ttu-id="9f3b2-122">Protože k hostování fungující sady vrstvených svazků je vyžadováno volné místo v zařízení, vytváření místně vázaných svazků ovlivňuje volné místo dostupné ke zřizování vrstvených svazků.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-122">As local space on the device is required to host the working set of tiered volumes, creation of locally pinned volumes impacts the space available for provisioning tiered volumes.</span></span> <span data-ttu-id="9f3b2-123">Pokud vytvoříte místně vázaný svazek, zmenší se dostupné volné místo potřebné k vytváření vrstvených svazků.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-123">Therefore, if you create a locally pinned volume, space available for creation of tiered volumes will be reduced.</span></span> <span data-ttu-id="9f3b2-124">Podobně pokud vytvoříte vrstvený svazek, zmenší se dostupné volné místo potřebné k vytváření místně vázaných svazků.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-124">Similarly, if a tiered volume is created, the available space for creation of locally pinned volumes is reduced.</span></span>
      
       <span data-ttu-id="9f3b2-125">Pokud v zařízení 8100 zřídíte místně vázaný svazek o velikosti 8.5 TB (maximální možná velikost), vyčerpáte tím veškeré volné místo dostupné v zařízení.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-125">If you provision a locally pinned volume of 8.5 TB (maximum allowable size) on your 8100 device, then you have exhausted all the local space available on the device.</span></span> <span data-ttu-id="9f3b2-126">Nebudete už moct vytvořit žádné vrstvené svazky a v zařízení už nezbude žádné volné místo k hostování pracovní sady vrstveného svazku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-126">You will not be able to create any tiered volume from that point onwards as there is no local space on the device to host the working set of the tiered volume.</span></span> <span data-ttu-id="9f3b2-127">Objem dostupného volného místa ovlivňují také vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-127">Existing tiered volumes also affect the space available.</span></span> <span data-ttu-id="9f3b2-128">Pokud například používáte zařízení 8100, ve kterém jsou už zřízeny vrstvené svazky o velikosti zhruba 106 TB, k vytváření místně vázaných svazků zbude už jenom 4 TB dostupného volného místa.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-128">For example, if you have an 8100 device that already has tiered volumes of roughly 106 TB, only 4 TB of space is available for locally pinned volumes.</span></span>
      
       <span data-ttu-id="9f3b2-129">Na následujícím obrázku je dialogové okno **Základní nastavení** pro místně vázaný svazek.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-129">The following image shows the **Basic Settings** dialog box for a locally pinned volume.</span></span>
      
        ![Přidání místního svazku](./media/storsimple-create-volume-u2/add-local-volume-include.png)
      
       <span data-ttu-id="9f3b2-131">Na následujícím obrázku je dialogové okno **Základní nastavení** pro vrstvený svazek.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-131">The following image shows the **Basic Settings** dialog box for a tiered volume.</span></span>
      
        ![Přidání místního svazku](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)
   
   1. <span data-ttu-id="9f3b2-133">Kliknutím na ikonu šipky</span><span class="sxs-lookup"><span data-stu-id="9f3b2-133">Click the arrow icon</span></span> ![ikona šipky](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) <span data-ttu-id="9f3b2-135">přejděte na další stránku.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-135">to go to the next page.</span></span>
3. <span data-ttu-id="9f3b2-136">V dialogovém okně **Další nastavení** přidejte nový záznam řízení přístupu (ACR):</span><span class="sxs-lookup"><span data-stu-id="9f3b2-136">In the **Additional Settings** dialog box, add a new access control record (ACR):</span></span>
   
   1. <span data-ttu-id="9f3b2-137">Zadejte **Název** záznamu ACR.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-137">Supply a **Name** for your ACR.</span></span>
   2. <span data-ttu-id="9f3b2-138">V části **Název iniciátoru iSCSI** zadejte kvalifikovaný název iSCSI (IQN) hostitele s Windows.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-138">Under **iSCSI Initiator Name**, provide the iSCSI Qualified Name (IQN) of your Windows host.</span></span> <span data-ttu-id="9f3b2-139">Pokud název IQN nemáte, přejděte do části [Získání názvu hostitele se systémem Windows Server](#get-the-iqn-of-a-windows-server-host).</span><span class="sxs-lookup"><span data-stu-id="9f3b2-139">If you don't have the IQN, go to [Get the IQN of a Windows Server host](#get-the-iqn-of-a-windows-server-host).</span></span>
   3. <span data-ttu-id="9f3b2-140">V části **Výchozí zálohování tohoto svazku?** zaškrtněte políčko **Povolit**.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-140">Under **Default backup for this volume?**, select the **Enable** check box.</span></span> <span data-ttu-id="9f3b2-141">Výchozí zálohování vytvoří zásadu, která se spustí každý den ve 22:30 (čas zařízení) a vytvoří snímek tohoto svazku v cloudu.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-141">The default backup creates a policy that executes at 22:30 each day (device time) and creates a cloud snapshot of this volume.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="9f3b2-142">Jakmile tady zásadu povolíte, nastavení už se nedá vrátit.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-142">After the backup is enabled here, it cannot be reverted.</span></span> <span data-ttu-id="9f3b2-143">Chcete-li změnit toto nastavení, musíte upravit svazek.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-143">You need to edit the volume to modify this setting.</span></span>
      > 
      > 
      
      ![Přidání svazku](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)
4. <span data-ttu-id="9f3b2-145">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="9f3b2-145">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png)<span data-ttu-id="9f3b2-147">.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-147">.</span></span> <span data-ttu-id="9f3b2-148">Vytvoří se svazek se zadaným nastavením.</span><span class="sxs-lookup"><span data-stu-id="9f3b2-148">A volume is created with the specified settings.</span></span>
