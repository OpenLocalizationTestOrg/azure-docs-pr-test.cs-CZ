### <a name="configure-a-dns-label-for-hello-public-ip-address"></a><span data-ttu-id="f3716-101">Konfigurace názvu DNS pro hello veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="f3716-101">Configure a DNS Label for hello public IP address</span></span>

<span data-ttu-id="f3716-102">tooconnect toohello databázového stroje SQL Server z hello Internetu, zvažte vytvoření názvu DNS veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f3716-102">tooconnect toohello SQL Server Database Engine from hello Internet, consider creating a DNS Label for your public IP address.</span></span> <span data-ttu-id="f3716-103">Můžete připojit pomocí IP adresy, ale hello popisek DNS vytvoří záznam A, který je snazší tooidentify a přehledů hello základní veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f3716-103">You can connect by IP address, but hello DNS Label creates an A Record that is easier tooidentify and abstracts hello underlying public IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="f3716-104">Názvy DNS nejsou vyžadovány, pokud plán tooonly připojení toohello systému SQL Server instance v rámci hello stejné virtuální sítě nebo jenom místně.</span><span class="sxs-lookup"><span data-stu-id="f3716-104">DNS Labels are not required if you plan tooonly connect toohello SQL Server instance within hello same Virtual Network or only locally.</span></span>

<span data-ttu-id="f3716-105">toocreate název DNS, vyberte nejdřív **virtuální počítače** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="f3716-105">toocreate a DNS Label, first select **Virtual machines** in hello portal.</span></span> <span data-ttu-id="f3716-106">Vyberte váš virtuální počítač s SQL serverem toobring si jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f3716-106">Select your SQL Server VM toobring up its properties.</span></span>

1. <span data-ttu-id="f3716-107">V části Přehled hello virtuálního počítače, vyberte vaše **veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="f3716-107">In hello virtual machine overview, select your **Public IP address**.</span></span>

    ![Veřejná IP adresa](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. <span data-ttu-id="f3716-109">Ve vlastnostech hello pro veřejnou IP adresu, rozbalte položku **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="f3716-109">In hello properties for your Public IP address, expand **Configuration**.</span></span>

1. <span data-ttu-id="f3716-110">Zadejte název DNS.</span><span class="sxs-lookup"><span data-stu-id="f3716-110">Enter a DNS Label name.</span></span> <span data-ttu-id="f3716-111">Tento název je záznam A, který může být přímo použít tooconnect tooyour virtuální počítač SQL Server pomocí názvu, namísto pomocí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f3716-111">This name is an A Record that can be used tooconnect tooyour SQL Server VM by name instead of by IP Address directly.</span></span>

1. <span data-ttu-id="f3716-112">Klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3716-112">Click hello **Save** button.</span></span>

    ![Název DNS](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a><span data-ttu-id="f3716-114">Připojit toohello databázový stroj z jiného počítače</span><span class="sxs-lookup"><span data-stu-id="f3716-114">Connect toohello Database Engine from another computer</span></span>

1. <span data-ttu-id="f3716-115">Na počítači připojené toohello Internetu, otevřete SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="f3716-115">On a computer connected toohello internet, open SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="f3716-116">Pokud aplikaci SQL Server Management Studio nemáte, [tady](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) si ji můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="f3716-116">If you do not have SQL Server Management Studio, you can download it [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>

1. <span data-ttu-id="f3716-117">V hello **připojit tooServer** nebo **připojit tooDatabase modul** dialogové okno, upravit hello **název serveru** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f3716-117">In hello **Connect tooServer** or **Connect tooDatabase Engine** dialog box, edit hello **Server name** value.</span></span> <span data-ttu-id="f3716-118">Zadejte hello IP adresu nebo úplný název DNS hello virtuálního počítače (určený v předchozí úloze hello).</span><span class="sxs-lookup"><span data-stu-id="f3716-118">Enter hello IP address or full DNS name of hello virtual machine (determined in hello previous task).</span></span> <span data-ttu-id="f3716-119">Můžete také přidat čárku a zadat port TCP SQL Serveru.</span><span class="sxs-lookup"><span data-stu-id="f3716-119">You can also add a comma and provide SQL Server's TCP port.</span></span> <span data-ttu-id="f3716-120">Například, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span><span class="sxs-lookup"><span data-stu-id="f3716-120">For example, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span></span>

1. <span data-ttu-id="f3716-121">V hello **ověřování** vyberte **ověřování systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f3716-121">In hello **Authentication** box, select **SQL Server Authentication**.</span></span>

1. <span data-ttu-id="f3716-122">V hello **přihlášení** pole, název typu hello platného přihlášení SQL.</span><span class="sxs-lookup"><span data-stu-id="f3716-122">In hello **Login** box, type hello name of a valid SQL login.</span></span>

1. <span data-ttu-id="f3716-123">V hello **heslo** pole, zadejte heslo hello hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f3716-123">In hello **Password** box, type hello password of hello login.</span></span>

1. <span data-ttu-id="f3716-124">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="f3716-124">Click **Connect**.</span></span>

    ![Připojení přes SSMS](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)