<!--author=alkohli last changed: 06/22/17-->

#### <a name="toocreate-a-volume-container"></a><span data-ttu-id="d7d3e-101">toocreate kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="d7d3e-101">toocreate a volume container</span></span>
1. <span data-ttu-id="d7d3e-102">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-102">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="d7d3e-103">Hello tabulkové výpis hello zařízení vyberte a klikněte na zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-103">From hello tabular listing of hello devices, select and click a device.</span></span> 

    ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. <span data-ttu-id="d7d3e-105">Na řídicím panelu hello zařízení, klikněte na tlačítko **+ přidat kontejner svazků**</span><span class="sxs-lookup"><span data-stu-id="d7d3e-105">In hello device dashboard, click **+ Add volume container**</span></span>

    ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. <span data-ttu-id="d7d3e-107">V hello **přidat kontejner svazků** okno:</span><span class="sxs-lookup"><span data-stu-id="d7d3e-107">In hello **Add volume container** blade:</span></span>
   
   1. <span data-ttu-id="d7d3e-108">Hello zařízení je automaticky vybrán.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-108">hello device is automatically selected.</span></span>
   2. <span data-ttu-id="d7d3e-109">Zadejte **Název** kontejneru svazků.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-109">Supply a **Name** for your volume container.</span></span> <span data-ttu-id="d7d3e-110">Hello název musí mít 3 too32 znaků.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-110">hello name must be 3 too32 characters long.</span></span> <span data-ttu-id="d7d3e-111">Kontejner svazků se po vytvoření už nedá přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-111">You cannot rename a volume container once it is created.</span></span>
   3. <span data-ttu-id="d7d3e-112">Vyberte **povolit šifrování úložiště cloudu** tooenable šifrování hello dat odesílaných ze hello zařízení toohello cloud.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-112">Select **Enable Cloud Storage Encryption** tooenable encryption of hello data sent from hello device toohello cloud.</span></span>
   4. <span data-ttu-id="d7d3e-113">Zadejte a potvrďte **šifrovací klíč cloudového úložiště** tedy 8 too32 znaků.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-113">Provide and confirm a **Cloud Storage Encryption Key** that is 8 too32 characters long.</span></span> <span data-ttu-id="d7d3e-114">Tento klíč se používá hello zařízení tooaccess zašifrovaná data.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-114">This key is used by hello device tooaccess encrypted data.</span></span>
   5. <span data-ttu-id="d7d3e-115">Vyberte **účet úložiště** tooassociate s tímto kontejnerem svazků.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-115">Select a **Storage Account** tooassociate with this volume container.</span></span> <span data-ttu-id="d7d3e-116">Můžete vybrat existující účet úložiště nebo hello výchozí účet, který se vygeneruje při hello čas vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-116">You can choose an existing storage account or hello default account that is generated at hello time of service creation.</span></span> <span data-ttu-id="d7d3e-117">Můžete taky hello **přidat nový** toospecify možnost účet úložiště, které nejsou propojené toothis předplatné služby.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-117">You can also use hello **Add new** option toospecify a storage account that is not linked toothis service subscription.</span></span>
   6. <span data-ttu-id="d7d3e-118">Vyberte **neomezený** v hello **zadejte šířku pásma** rozevíracího seznamu, pokud chcete tooconsume veškerou dostupnou šířku pásma hello.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-118">Select **Unlimited** in hello **Specify bandwidth** drop-down list if you wish tooconsume all hello available bandwidth.</span></span> <span data-ttu-id="d7d3e-119">Můžete také nastavit tuto možnost také**vlastní** tooemploy řízení šířky pásma a zadejte hodnotu v rozmezí 1 až 1 000 MB/s.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-119">You can also set this option too**Custom** tooemploy bandwidth controls, and specify a value between 1 Mbps and 1,000 Mbps.</span></span>
      <span data-ttu-id="d7d3e-120">Pokud máte k dispozici informace o vaší šířky pásma využití, může být schopný tooallocate šířky pásma na základě plánu zadáním **vybrat šablonu šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-120">If you have your bandwidth usage information available, you may be able tooallocate bandwidth based on a schedule by specifying **Select a bandwidth template**.</span></span> <span data-ttu-id="d7d3e-121">Podrobný postup najdete příliš[přidání šablony šířky pásma](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span><span class="sxs-lookup"><span data-stu-id="d7d3e-121">For a step-by-step procedure, go too[Add a bandwidth template](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span></span>

      ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. <span data-ttu-id="d7d3e-123">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-123">Click **Create**.</span></span>

        ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       <span data-ttu-id="d7d3e-125">Upozornění se zobrazí při úspěšném vytvoření kontejneru svazků hello.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-125">You are notified when hello volume container is successfully created.</span></span>

       ![Oznámení o vytvoření kontejneru svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   <span data-ttu-id="d7d3e-127">Hello nově vytvořený kontejner svazků je uvedena v seznamu hello kontejnery svazků pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7d3e-127">hello newly created volume container is listed in hello list of volume containers for your device.</span></span>

   ![Okno Přidat kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


