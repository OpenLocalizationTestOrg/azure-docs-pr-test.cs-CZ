


## <a name="attach-an-empty-disk"></a><span data-ttu-id="b8078-101">Připojení prázdného disku</span><span class="sxs-lookup"><span data-stu-id="b8078-101">Attach an empty disk</span></span>
<span data-ttu-id="b8078-102">Prázdný disk se připojuje je jednoduchý způsob tooadd, data na disku, protože pro vás vytvoří soubor VHD hello Azure a ukládá je v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="b8078-102">Attaching an empty disk is a simple way tooadd a data disk, because Azure creates hello .vhd file for you and stores it in hello storage account.</span></span>

1. <span data-ttu-id="b8078-103">Klikněte na tlačítko **virtuálních počítačů (klasické)**, a pak vyberte hello odpovídající virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b8078-103">Click **Virtual Machines (classic)**, and then select hello appropriate VM.</span></span>

2. <span data-ttu-id="b8078-104">V nabídce nastavení hello, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="b8078-104">In hello Settings menu, click **Disks**.</span></span>

   ![Připojit nový prázdný disk](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. <span data-ttu-id="b8078-106">Na panelu příkazů hello, klikněte na tlačítko **připojit nový**.</span><span class="sxs-lookup"><span data-stu-id="b8078-106">On hello command bar, click **Attach new**.</span></span>  
    <span data-ttu-id="b8078-107">Hello **připojit nový disk** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8078-107">hello **Attach new disk** dialog box appears.</span></span>

    ![Připojit nový disk](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    <span data-ttu-id="b8078-109">Vyplňte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="b8078-109">Fill in hello following information:</span></span>
    - <span data-ttu-id="b8078-110">V **název souboru**, přijměte výchozí název hello nebo zadejte jinou pro soubor VHD hello.</span><span class="sxs-lookup"><span data-stu-id="b8078-110">In **File Name**, accept hello default name or type another one for hello .vhd file.</span></span> <span data-ttu-id="b8078-111">datový disk Hello používá automaticky vygenerovaným názvem, pokud zadáte jiný název souboru VHD hello.</span><span class="sxs-lookup"><span data-stu-id="b8078-111">hello data disk uses an automatically generated name, even if you type another name for hello .vhd file.</span></span>
    - <span data-ttu-id="b8078-112">Vyberte hello **typ** z hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="b8078-112">Select hello **Type** of hello data disk.</span></span> <span data-ttu-id="b8078-113">Všechny virtuální počítače podporují standardní disky.</span><span class="sxs-lookup"><span data-stu-id="b8078-113">All virtual machines support standard disks.</span></span> <span data-ttu-id="b8078-114">Mnoho virtuálních počítačů také nepodporují prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="b8078-114">Many virtual machines also support premium disks.</span></span>
    - <span data-ttu-id="b8078-115">Vyberte hello **velikost (GB)** z hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="b8078-115">Select hello **Size (GB)** of hello data disk.</span></span>
    - <span data-ttu-id="b8078-116">Pro **použití mezipaměti u hostitele**, zvolte Žádný, nebo jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b8078-116">For **Host caching**, choose none or Read Only.</span></span>
    - <span data-ttu-id="b8078-117">Klikněte na tlačítko OK toofinish.</span><span class="sxs-lookup"><span data-stu-id="b8078-117">Click OK toofinish.</span></span>

4. <span data-ttu-id="b8078-118">Jakmile hello datový disk se vytvoří a připojené, je uvedena v části disky hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b8078-118">After hello data disk is created and attached, it's listed in hello disks section of hello VM.</span></span>

   ![Nové a prázdný datový disk se úspěšně připojil](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> <span data-ttu-id="b8078-120">Po přidání datový disk, potřebujete toolog na toohello virtuálních počítačů a inicializovat hello disk, aby ho bylo možné použít.</span><span class="sxs-lookup"><span data-stu-id="b8078-120">After you add a data disk, you need toolog on toohello VM and initialize hello disk so that it can be used.</span></span>

## <a name="how-to-attach-an-existing-disk"></a><span data-ttu-id="b8078-121">Postupy: připojit stávající disk</span><span class="sxs-lookup"><span data-stu-id="b8078-121">How to: Attach an existing disk</span></span>
<span data-ttu-id="b8078-122">Připojení stávajícího disku vyžaduje, aby v účtu úložiště byl dostupný soubor .vhd.</span><span class="sxs-lookup"><span data-stu-id="b8078-122">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span> <span data-ttu-id="b8078-123">Použití hello [přidat AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) rutiny tooupload hello VHD souboru toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b8078-123">Use hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet tooupload hello .vhd file toohello storage account.</span></span> <span data-ttu-id="b8078-124">Po vytvoření a nahrát soubor VHD hello, abyste ho připojili tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b8078-124">After you've created and uploaded hello .vhd file, you can attach it tooa VM.</span></span>

1. <span data-ttu-id="b8078-125">Klikněte na tlačítko **virtuálních počítačů (klasické)**, a pak vyberte hello odpovídající virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b8078-125">Click **Virtual Machines (classic)**, and then select hello appropriate virtual machine.</span></span>

2. <span data-ttu-id="b8078-126">V nabídce nastavení hello, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="b8078-126">In hello Settings menu, click **Disks**.</span></span>

3. <span data-ttu-id="b8078-127">Na panelu příkazů hello, klikněte na tlačítko **připojit existující**.</span><span class="sxs-lookup"><span data-stu-id="b8078-127">On hello command bar, click **Attach existing**.</span></span>

    ![Připojení datového disku](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. <span data-ttu-id="b8078-129">Klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="b8078-129">Click **Location**.</span></span> <span data-ttu-id="b8078-130">účty úložiště k dispozici Hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b8078-130">hello available storage accounts display.</span></span> <span data-ttu-id="b8078-131">Potom vyberte účet odpovídající úložiště ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="b8078-131">Next, select an appropriate storage account from those listed.</span></span>

    ![Zadejte účet úložiště disku](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. <span data-ttu-id="b8078-133">A **účet úložiště** obsahuje jeden nebo více kontejnerů, které obsahují disky (VHD).</span><span class="sxs-lookup"><span data-stu-id="b8078-133">A **Storage account** holds one or more containers that contain disk drives (vhds).</span></span> <span data-ttu-id="b8078-134">Vyberte z uvedených hello odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b8078-134">Select hello appropriate container from those listed.</span></span>

    ![Zadejte kontejner virtuálního počítače windows](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. <span data-ttu-id="b8078-136">Hello **virtuální pevné disky** panel uvádí uchovávat v kontejneru hello hello diskových jednotek.</span><span class="sxs-lookup"><span data-stu-id="b8078-136">hello **vhds** panel lists hello disk drives held in hello container.</span></span> <span data-ttu-id="b8078-137">Klikněte na jednu hello disků a potom klikněte na vybrat.</span><span class="sxs-lookup"><span data-stu-id="b8078-137">Click one of hello disks, and then click Select.</span></span>

    ![Zadejte bitovou kopii disku pro virtuální počítače windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. <span data-ttu-id="b8078-139">Hello **připojit stávající disk** panelu se zobrazí znovu, umístěním hello obsahující hello účet úložiště, kontejneru a vybraného pevného disku (vhd) tooadd toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b8078-139">hello **Attach existing disk** panel displays again, with hello location containing hello storage account, container, and selected hard disk (vhd) tooadd toohello virtual machine.</span></span>

  <span data-ttu-id="b8078-140">Nastavit **použití mezipaměti u hostitele** toonone nebo pro čtení, klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="b8078-140">Set **Host caching** toonone or Read only, then click OK.</span></span>

    ![Datový disk se úspěšně připojil](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
