
1. <span data-ttu-id="4f1b3-101">tooescalate oprávněními, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-101">tooescalate privileges, type:</span></span>
   
        sudo -s
   
    <span data-ttu-id="4f1b3-102">Zadejte svoje heslo.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-102">Enter your password.</span></span>
2. <span data-ttu-id="4f1b3-103">tooinstall MySQL Community Server edition, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-103">tooinstall MySQL Community Server edition, type:</span></span>
   
        zypper install mysql-community-server
   
    <span data-ttu-id="4f1b3-104">Počkejte, než MySQL se stáhne a nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-104">Wait while MySQL downloads and installs.</span></span>
3. <span data-ttu-id="4f1b3-105">toostart tooset MySQL, když se spustí hello system, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-105">tooset MySQL toostart when hello system boots, type:</span></span>
   
        insserv mysql
4. <span data-ttu-id="4f1b3-106">Démon MySQL hello (mysqld) spusťte ručně pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-106">Start hello MySQL daemon (mysqld) manually with this command:</span></span>
   
        rcmysql start
   
    <span data-ttu-id="4f1b3-107">Stav hello toocheck hello démona MySQL, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-107">toocheck hello status of hello MySQL daemon, type:</span></span>
   
        rcmysql status
   
    <span data-ttu-id="4f1b3-108">toostop hello démona MySQL, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-108">toostop hello MySQL daemon, type:</span></span>
   
        rcmysql stop
   
   > [!IMPORTANT]
   > <span data-ttu-id="4f1b3-109">Po instalaci hello MySQL kořenové heslo je prázdné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-109">After installation, hello MySQL root password is empty by default.</span></span> <span data-ttu-id="4f1b3-110">Doporučujeme spustit **mysql\_zabezpečené\_instalace**, skript, který pomáhá zabezpečené MySQL.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-110">We recommended that you run **mysql\_secure\_installation**, a script that helps secure MySQL.</span></span> <span data-ttu-id="4f1b3-111">Hello skript zobrazí výzvu toochange hello MySQL kořenové heslo, odeberte anonymní uživatelské účty, zakažte kořenové vzdáleného přihlášení, odebrat testovací databáze a znovu načíst tabulku oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-111">hello script prompts you toochange hello MySQL root password, remove anonymous user accounts, disable remote root logins, remove test databases, and reload hello privileges table.</span></span> <span data-ttu-id="4f1b3-112">Je vhodné zodpovědět Ano tooall z těchto možností a hello kořenové heslo změnit.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-112">We recommended that you answer yes tooall of these options and change hello root password.</span></span>
   > 
   > 
5. <span data-ttu-id="4f1b3-113">Zadejte tento skript hello toorun MySQL instalační skript:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-113">Type this toorun hello script MySQL installation script:</span></span>
   
        mysql_secure_installation
6. <span data-ttu-id="4f1b3-114">Přihlaste se tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-114">Log in tooMySQL:</span></span>
   
        mysql -u root -p
   
    <span data-ttu-id="4f1b3-115">Zadejte hello MySQL kořenové heslo (který jste změnili v předchozím kroku hello) a zobrazí vám s výzvou kde můžete vydat toointeract příkazy SQL s databází hello.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-115">Enter hello MySQL root password (which you changed in hello previous step) and you'll be presented with a prompt where you can issue SQL statements toointeract with hello database.</span></span>
7. <span data-ttu-id="4f1b3-116">toocreate nového uživatele databáze MySQL, spusťte následující hello v hello **mysql >** řádku:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-116">toocreate a new MySQL user, run hello following at hello **mysql>** prompt:</span></span>
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="4f1b3-117">Poznámka: hello středníkem (;) na konci hello hello řádky jsou zásadní pro ukončení hello příkazy.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-117">Note, hello semi-colons (;) at hello end of hello lines are crucial for ending hello commands.</span></span>
8. <span data-ttu-id="4f1b3-118">toocreate databáze a udělte hello `mysqluser` oprávnění uživatele na něm problém hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-118">toocreate a database and grant hello `mysqluser` user permissions on it, issue hello following commands:</span></span>
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* too'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="4f1b3-119">Všimněte si, že databáze uživatelská jména a hesla používají pouze připojení databáze toohello skripty.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-119">Note that database user names and passwords are only used by scripts connecting toohello database.</span></span>  <span data-ttu-id="4f1b3-120">Názvy databáze uživatelských účtů nutně nepředstavuje skutečný uživatelské účty v systému hello.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-120">Database user account names do not necessarily represent actual user accounts on hello system.</span></span>
9. <span data-ttu-id="4f1b3-121">toolog v z jiného počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-121">toolog in from another computer, type:</span></span>
   
        GRANT ALL ON testdatabase.* too'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    <span data-ttu-id="4f1b3-122">kde `ip-address` je IP adresa hello hello počítače, ze kterého se připojí tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-122">where `ip-address` is hello IP address of hello computer from which you will connect tooMySQL.</span></span>
10. <span data-ttu-id="4f1b3-123">hello tooexit nástroj pro správu databáze MySQL, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-123">tooexit hello MySQL database administration utility, type:</span></span>
    
        quit

## <a name="add-an-endpoint"></a><span data-ttu-id="4f1b3-124">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="4f1b3-124">Add an endpoint</span></span>
1. <span data-ttu-id="4f1b3-125">Po instalaci MySQL, budete potřebovat tooconfigure koncový bod tooaccess MySQL vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-125">After MySQL is installed, you'll need tooconfigure an endpoint tooaccess MySQL remotely.</span></span> <span data-ttu-id="4f1b3-126">Přihlaste se toohello [portál Azure classic][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="4f1b3-126">Log in toohello [Azure  classic portal][AzurePortal].</span></span> <span data-ttu-id="4f1b3-127">Klikněte na tlačítko **virtuální počítače**, klikněte na tlačítko hello název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-127">Click **Virtual Machines**, click hello name of your new virtual machine, and then click **Endpoints**.</span></span>
2. <span data-ttu-id="4f1b3-128">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4f1b3-128">Click **Add** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="4f1b3-129">Přidat koncový bod s názvem "MySQL" s protokolem **TCP**, a **veřejné** a **privátní** porty sadu příliš "3306".</span><span class="sxs-lookup"><span data-stu-id="4f1b3-129">Add an endpoint named "MySQL" with protocol **TCP**, and **Public** and **Private** ports set too"3306".</span></span>
4. <span data-ttu-id="4f1b3-130">tooremotely připojit toohello virtuálního počítače z vašeho počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-130">tooremotely connect toohello virtual machine from your computer, type:</span></span>
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    <span data-ttu-id="4f1b3-131">Například pomocí hello virtuální počítače, který jsme vytvořili v tomto kurzu, zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f1b3-131">For example, using hello virual machine we created in this tutorial, type this command:</span></span>
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
