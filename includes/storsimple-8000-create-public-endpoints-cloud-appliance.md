#### <a name="toocreate-public-endpoints-on-hello-cloud-appliance"></a><span data-ttu-id="7be5e-101">veřejné koncové body toocreate na zařízení cloudu hello</span><span class="sxs-lookup"><span data-stu-id="7be5e-101">toocreate public endpoints on hello cloud appliance</span></span>

1. <span data-ttu-id="7be5e-102">Přihlaste se toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7be5e-102">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="7be5e-103">Přejděte příliš**virtuální počítače**a poté vyberte a klikněte na tlačítko hello virtuální počítač, který je používán jako vaše zařízení cloudu.</span><span class="sxs-lookup"><span data-stu-id="7be5e-103">Go too**Virtual Machines**, and then select and click hello virtual machine that is being used as your cloud appliance.</span></span>
    
3. <span data-ttu-id="7be5e-104">Je nutné toocreate k zabezpečení sítě (NSG) skupiny pravidlo toocontrol hello toku provozu a deaktivovat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7be5e-104">You need toocreate a network security group (NSG) rule toocontrol hello flow of traffic in and out of your virtual machine.</span></span> <span data-ttu-id="7be5e-105">Proveďte následující kroky toocreate pravidlo NSG hello.</span><span class="sxs-lookup"><span data-stu-id="7be5e-105">Perform hello following steps toocreate an NSG rule.</span></span>
    1. <span data-ttu-id="7be5e-106">Vyberte **Skupina zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-106">Select **Network security group**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. <span data-ttu-id="7be5e-107">Klikněte na tlačítko hello skupinu zabezpečení sítě v výchozí, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="7be5e-107">Click hello default network security group that is presented.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. <span data-ttu-id="7be5e-108">Vyberte **Příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-108">Select **Inbound security rules**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. <span data-ttu-id="7be5e-109">Klikněte na tlačítko **+ přidat** toocreate pravidlo příchozí zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7be5e-109">Click **+ Add** toocreate an inbound security rule.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        <span data-ttu-id="7be5e-110">V okně pravidla zabezpečení příchozích hello přidat:</span><span class="sxs-lookup"><span data-stu-id="7be5e-110">In hello Add inbound security rule blade:</span></span>

        1. <span data-ttu-id="7be5e-111">Pro hello **název**, typ hello následující název koncového bodu hello: WinRMHttps.</span><span class="sxs-lookup"><span data-stu-id="7be5e-111">For hello **Name**, type hello following name for hello endpoint: WinRMHttps.</span></span>
        
        2. <span data-ttu-id="7be5e-112">Pro hello **s prioritou**, vyberte číslo menší než 1 000 (což je hello prioritu hello výchozí pravidlo).</span><span class="sxs-lookup"><span data-stu-id="7be5e-112">For hello **Priority**, select a number lesser than 1000 (which is hello priority for hello default rule).</span></span> <span data-ttu-id="7be5e-113">Vyšší hodnota hello, hello s nižší prioritou.</span><span class="sxs-lookup"><span data-stu-id="7be5e-113">Higher hello value, lower hello priority.</span></span>

        3. <span data-ttu-id="7be5e-114">Sada hello **zdroj** příliš**žádné**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-114">Set hello **Source** too**Any**.</span></span>

        4. <span data-ttu-id="7be5e-115">Pro hello **služby**, vyberte **WinRM**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-115">For hello **Service**, select **WinRM**.</span></span> <span data-ttu-id="7be5e-116">Hello **protokol** bude automaticky nastavena příliš**TCP** a hello **rozsah portů** je nastaven příliš**5986**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-116">hello **Protocol** is automatically set too**TCP** and hello **Port range** is set too**5986**.</span></span>

        5. <span data-ttu-id="7be5e-117">Klikněte na tlačítko **OK** toocreate hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="7be5e-117">Click **OK** toocreate hello rule.</span></span>

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. <span data-ttu-id="7be5e-118">Posledním krokem je tooassociate skupiny zabezpečení sítě se podsíť nebo konkrétní síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7be5e-118">Your final step is tooassociate your network security group with a subnet or a specific network interface.</span></span> <span data-ttu-id="7be5e-119">Proveďte následující kroky tooassociate hello vaše skupina zabezpečení sítě s podsítí.</span><span class="sxs-lookup"><span data-stu-id="7be5e-119">Perform hello following steps tooassociate your network security group with a subnet.</span></span>
    1. <span data-ttu-id="7be5e-120">Přejděte příliš**podsítě**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-120">Go too**Subnets**.</span></span>
    2. <span data-ttu-id="7be5e-121">Klikněte na **+ Přidružit**.</span><span class="sxs-lookup"><span data-stu-id="7be5e-121">Click **+ Associate**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. <span data-ttu-id="7be5e-122">Vyberte virtuální síť a pak vyberte příslušnou podsítí hello.</span><span class="sxs-lookup"><span data-stu-id="7be5e-122">Select your virtual network, and then select hello appropriate subnet.</span></span>
    4. <span data-ttu-id="7be5e-123">Klikněte na tlačítko **OK** toocreate hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="7be5e-123">Click **OK** toocreate hello rule.</span></span>

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

<span data-ttu-id="7be5e-124">Po vytvoření pravidla hello můžete zobrazit jeho podrobnosti toodetermine hello veřejná virtuální IP (VIP) adres.</span><span class="sxs-lookup"><span data-stu-id="7be5e-124">After hello rule is created, you can view its details toodetermine hello Public Virtual IP (VIP) address.</span></span> <span data-ttu-id="7be5e-125">Tuto adresu si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="7be5e-125">Record this address.</span></span>


