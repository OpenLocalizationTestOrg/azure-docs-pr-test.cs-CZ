#### <a name="to-create-public-endpoints-on-the-cloud-appliance"></a><span data-ttu-id="c15be-101">Vytvoření veřejných koncových bodů na cloudovém zařízení</span><span class="sxs-lookup"><span data-stu-id="c15be-101">To create public endpoints on the cloud appliance</span></span>

1. <span data-ttu-id="c15be-102">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c15be-102">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="c15be-103">Přejděte na **Virtuální počítače** a vyberte a klikněte na virtuální počítač, který používáte jako cloudové zařízení.</span><span class="sxs-lookup"><span data-stu-id="c15be-103">Go to **Virtual Machines**, and then select and click the virtual machine that is being used as your cloud appliance.</span></span>
    
3. <span data-ttu-id="c15be-104">Potřebujete vytvořit pravidlo skupiny zabezpečení sítě (NSG) pro řízení toku přenosů do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c15be-104">You need to create a network security group (NSG) rule to control the flow of traffic in and out of your virtual machine.</span></span> <span data-ttu-id="c15be-105">Proveďte následující kroky a vytvořte pravidlo skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="c15be-105">Perform the following steps to create an NSG rule.</span></span>
    1. <span data-ttu-id="c15be-106">Vyberte **Skupina zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="c15be-106">Select **Network security group**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. <span data-ttu-id="c15be-107">Klikněte na výchozí skupinu zabezpečení sítě, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c15be-107">Click the default network security group that is presented.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. <span data-ttu-id="c15be-108">Vyberte **Příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="c15be-108">Select **Inbound security rules**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. <span data-ttu-id="c15be-109">Kliknutím na **+ Přidat** vytvořte příchozí pravidlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c15be-109">Click **+ Add** to create an inbound security rule.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        <span data-ttu-id="c15be-110">V okně Přidat příchozí pravidlo zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="c15be-110">In the Add inbound security rule blade:</span></span>

        1. <span data-ttu-id="c15be-111">Do pole **Název** zadejte tento název koncového bodu: WinRMHttps.</span><span class="sxs-lookup"><span data-stu-id="c15be-111">For the **Name**, type the following name for the endpoint: WinRMHttps.</span></span>
        
        2. <span data-ttu-id="c15be-112">V poli **Priorita** vyberte číslo nižší než 1 000 (což je priorita výchozího pravidla).</span><span class="sxs-lookup"><span data-stu-id="c15be-112">For the **Priority**, select a number lesser than 1000 (which is the priority for the default rule).</span></span> <span data-ttu-id="c15be-113">Čím vyšší hodnota, tím nižší priorita.</span><span class="sxs-lookup"><span data-stu-id="c15be-113">Higher the value, lower the priority.</span></span>

        3. <span data-ttu-id="c15be-114">Nastavte **Zdroj** na hodnotu **Všechny**.</span><span class="sxs-lookup"><span data-stu-id="c15be-114">Set the **Source** to **Any**.</span></span>

        4. <span data-ttu-id="c15be-115">V poli **Služba** vyberte **WinRM**.</span><span class="sxs-lookup"><span data-stu-id="c15be-115">For the **Service**, select **WinRM**.</span></span> <span data-ttu-id="c15be-116">**Protokol** je automaticky nastaven na **TCP** a **Rozsah portů** je nastaven na **5986**.</span><span class="sxs-lookup"><span data-stu-id="c15be-116">The **Protocol** is automatically set to **TCP** and the **Port range** is set to **5986**.</span></span>

        5. <span data-ttu-id="c15be-117">Kliknutím na **OK** vytvořte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c15be-117">Click **OK** to create the rule.</span></span>

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. <span data-ttu-id="c15be-118">Posledním krokem je přidružení skupiny zabezpečení sítě k podsíti nebo konkrétnímu síťovému rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c15be-118">Your final step is to associate your network security group with a subnet or a specific network interface.</span></span> <span data-ttu-id="c15be-119">Proveďte následující kroky a přidružte skupinu zabezpečení sítě k podsíti.</span><span class="sxs-lookup"><span data-stu-id="c15be-119">Perform the following steps to associate your network security group with a subnet.</span></span>
    1. <span data-ttu-id="c15be-120">Přejděte na **Podsítě**.</span><span class="sxs-lookup"><span data-stu-id="c15be-120">Go to **Subnets**.</span></span>
    2. <span data-ttu-id="c15be-121">Klikněte na **+ Přidružit**.</span><span class="sxs-lookup"><span data-stu-id="c15be-121">Click **+ Associate**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. <span data-ttu-id="c15be-122">Vyberte virtuální síť a pak vyberte vhodnou podsíť.</span><span class="sxs-lookup"><span data-stu-id="c15be-122">Select your virtual network, and then select the appropriate subnet.</span></span>
    4. <span data-ttu-id="c15be-123">Kliknutím na **OK** vytvořte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c15be-123">Click **OK** to create the rule.</span></span>

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

<span data-ttu-id="c15be-124">Po vytvoření pravidla můžete zobrazit jeho podrobnosti, abyste zjistili veřejnou virtuální IP adresu (VIP).</span><span class="sxs-lookup"><span data-stu-id="c15be-124">After the rule is created, you can view its details to determine the Public Virtual IP (VIP) address.</span></span> <span data-ttu-id="c15be-125">Tuto adresu si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="c15be-125">Record this address.</span></span>


