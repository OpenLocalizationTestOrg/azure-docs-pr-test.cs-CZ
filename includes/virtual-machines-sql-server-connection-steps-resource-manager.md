### <a name="configure-a-dns-label-for-the-public-ip-address"></a><span data-ttu-id="dceea-101">Konfigurace názvu DNS veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="dceea-101">Configure a DNS Label for the public IP address</span></span>

<span data-ttu-id="dceea-102">Pokud se chcete z internetu připojit k databázovému stroji SQL Serveru, zvažte vytvoření názvu DNS pro vaši veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="dceea-102">To connect to the SQL Server Database Engine from the Internet, consider creating a DNS Label for your public IP address.</span></span> <span data-ttu-id="dceea-103">Můžete se připojit pomocí IP adresy, ale název DNS vytvoří záznam A, který je snadnější identifikovat a který abstrahuje základní veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="dceea-103">You can connect by IP address, but the DNS Label creates an A Record that is easier to identify and abstracts the underlying public IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="dceea-104">Názvy DNS nejsou nutné, pokud se chcete připojit k instanci systému SQL Server jenom v rámci stejné virtuální sítě nebo jenom místně.</span><span class="sxs-lookup"><span data-stu-id="dceea-104">DNS Labels are not required if you plan to only connect to the SQL Server instance within the same Virtual Network or only locally.</span></span>

<span data-ttu-id="dceea-105">Pokud chcete vytvořit název DNS, nejdřív na portálu vyberte **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="dceea-105">To create a DNS Label, first select **Virtual machines** in the portal.</span></span> <span data-ttu-id="dceea-106">Vyberte virtuální počítač se systémem SQL Server, abyste zobrazili jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dceea-106">Select your SQL Server VM to bring up its properties.</span></span>

1. <span data-ttu-id="dceea-107">V přehledu virtuálního počítače vyberte své nastavení **Veřejná IP adresa**.</span><span class="sxs-lookup"><span data-stu-id="dceea-107">In the virtual machine overview, select your **Public IP address**.</span></span>

    ![Veřejná IP adresa](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. <span data-ttu-id="dceea-109">Ve vlastnostech veřejné IP adresa rozbalte položku **Konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="dceea-109">In the properties for your Public IP address, expand **Configuration**.</span></span>

1. <span data-ttu-id="dceea-110">Zadejte název DNS.</span><span class="sxs-lookup"><span data-stu-id="dceea-110">Enter a DNS Label name.</span></span> <span data-ttu-id="dceea-111">Tento název je záznam A, který se dá použít k přímému připojení k vašemu virtuálnímu počítači se systémem SQL Server pomocí názvu, namísto pomocí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="dceea-111">This name is an A Record that can be used to connect to your SQL Server VM by name instead of by IP Address directly.</span></span>

1. <span data-ttu-id="dceea-112">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dceea-112">Click the **Save** button.</span></span>

    ![Název DNS](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-to-the-database-engine-from-another-computer"></a><span data-ttu-id="dceea-114">Připojení k databázovému stroji z jiného počítače</span><span class="sxs-lookup"><span data-stu-id="dceea-114">Connect to the Database Engine from another computer</span></span>

1. <span data-ttu-id="dceea-115">Na počítači připojeném k internetu otevřete aplikaci SSMS (SQL Server Management Studio).</span><span class="sxs-lookup"><span data-stu-id="dceea-115">On a computer connected to the internet, open SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="dceea-116">Pokud aplikaci SQL Server Management Studio nemáte, [tady](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) si ji můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="dceea-116">If you do not have SQL Server Management Studio, you can download it [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>

1. <span data-ttu-id="dceea-117">V dialogovém okně **Připojit k serveru** nebo **Connect to Database Engine** (Připojit k databázovému stroji) upravte hodnotu **Název serveru**.</span><span class="sxs-lookup"><span data-stu-id="dceea-117">In the **Connect to Server** or **Connect to Database Engine** dialog box, edit the **Server name** value.</span></span> <span data-ttu-id="dceea-118">Zadejte IP adresu nebo úplný název DNS virtuálního počítače (určený v předchozí úloze).</span><span class="sxs-lookup"><span data-stu-id="dceea-118">Enter the IP address or full DNS name of the virtual machine (determined in the previous task).</span></span> <span data-ttu-id="dceea-119">Můžete také přidat čárku a zadat port TCP SQL Serveru.</span><span class="sxs-lookup"><span data-stu-id="dceea-119">You can also add a comma and provide SQL Server's TCP port.</span></span> <span data-ttu-id="dceea-120">Například, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span><span class="sxs-lookup"><span data-stu-id="dceea-120">For example, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span></span>

1. <span data-ttu-id="dceea-121">V poli **Ověřování** vyberte **Ověřování serveru SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="dceea-121">In the **Authentication** box, select **SQL Server Authentication**.</span></span>

1. <span data-ttu-id="dceea-122">Do pole **Přihlášení** zadejte název platného přihlášení SQL.</span><span class="sxs-lookup"><span data-stu-id="dceea-122">In the **Login** box, type the name of a valid SQL login.</span></span>

1. <span data-ttu-id="dceea-123">Do pole **Heslo** zadejte heslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="dceea-123">In the **Password** box, type the password of the login.</span></span>

1. <span data-ttu-id="dceea-124">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="dceea-124">Click **Connect**.</span></span>

    ![Připojení přes SSMS](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)