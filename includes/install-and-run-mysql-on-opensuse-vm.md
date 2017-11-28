
1. <span data-ttu-id="0490f-101">Pro zvýšení oprávnění, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-101">To escalate privileges, type:</span></span>
   
        sudo -s
   
    <span data-ttu-id="0490f-102">Zadejte svoje heslo.</span><span class="sxs-lookup"><span data-stu-id="0490f-102">Enter your password.</span></span>
2. <span data-ttu-id="0490f-103">Chcete-li nainstalovat MySQL Community Server edition, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-103">To install MySQL Community Server edition, type:</span></span>
   
        zypper install mysql-community-server
   
    <span data-ttu-id="0490f-104">Počkejte, než MySQL se stáhne a nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="0490f-104">Wait while MySQL downloads and installs.</span></span>
3. <span data-ttu-id="0490f-105">Pokud chcete nastavit MySQL spustit při spuštění systému, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-105">To set MySQL to start when the system boots, type:</span></span>
   
        insserv mysql
4. <span data-ttu-id="0490f-106">Démon MySQL (mysqld) spusťte ručně pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="0490f-106">Start the MySQL daemon (mysqld) manually with this command:</span></span>
   
        rcmysql start
   
    <span data-ttu-id="0490f-107">Chcete-li zkontrolovat stav démon MySQL, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-107">To check the status of the MySQL daemon, type:</span></span>
   
        rcmysql status
   
    <span data-ttu-id="0490f-108">Chcete-li zastavit démon MySQL, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-108">To stop the MySQL daemon, type:</span></span>
   
        rcmysql stop
   
   > [!IMPORTANT]
   > <span data-ttu-id="0490f-109">Po instalaci MySQL kořenové heslo je prázdné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0490f-109">After installation, the MySQL root password is empty by default.</span></span> <span data-ttu-id="0490f-110">Doporučujeme spustit **mysql\_zabezpečené\_instalace**, skript, který pomáhá zabezpečené MySQL.</span><span class="sxs-lookup"><span data-stu-id="0490f-110">We recommended that you run **mysql\_secure\_installation**, a script that helps secure MySQL.</span></span> <span data-ttu-id="0490f-111">Skript vyzve k změnit kořenové heslo MySQL, odeberte anonymní uživatelské účty, zakažte kořenové vzdáleného přihlášení, odebrat testovací databáze a znovu načíst tabulky oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0490f-111">The script prompts you to change the MySQL root password, remove anonymous user accounts, disable remote root logins, remove test databases, and reload the privileges table.</span></span> <span data-ttu-id="0490f-112">Doporučujeme Ano odpovědět, aby se všechny tyto možnosti a kořenové heslo změnit.</span><span class="sxs-lookup"><span data-stu-id="0490f-112">We recommended that you answer yes to all of these options and change the root password.</span></span>
   > 
   > 
5. <span data-ttu-id="0490f-113">Zadejte to pro spuštění skriptu MySQL instalační skript:</span><span class="sxs-lookup"><span data-stu-id="0490f-113">Type this to run the script MySQL installation script:</span></span>
   
        mysql_secure_installation
6. <span data-ttu-id="0490f-114">Přihlaste se k MySQL:</span><span class="sxs-lookup"><span data-stu-id="0490f-114">Log in to MySQL:</span></span>
   
        mysql -u root -p
   
    <span data-ttu-id="0490f-115">Zadejte hesla kořenového MySQL (který jste změnili v předchozím kroku) a zobrazí vám s výzvou kde můžete použít příkazy SQL pro interakci s databází.</span><span class="sxs-lookup"><span data-stu-id="0490f-115">Enter the MySQL root password (which you changed in the previous step) and you'll be presented with a prompt where you can issue SQL statements to interact with the database.</span></span>
7. <span data-ttu-id="0490f-116">Pokud chcete vytvořit nového uživatele databáze MySQL, spusťte následující příkaz na **mysql >** řádku:</span><span class="sxs-lookup"><span data-stu-id="0490f-116">To create a new MySQL user, run the following at the **mysql>** prompt:</span></span>
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="0490f-117">Poznámka: středníkem (;) na konci řádky jsou zásadní pro ukončení příkazy.</span><span class="sxs-lookup"><span data-stu-id="0490f-117">Note, the semi-colons (;) at the end of the lines are crucial for ending the commands.</span></span>
8. <span data-ttu-id="0490f-118">K vytvoření databáze a udělit `mysqluser` oprávnění uživatele, vydávání následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0490f-118">To create a database and grant the `mysqluser` user permissions on it, issue the following commands:</span></span>
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="0490f-119">Všimněte si, že databáze uživatelská jména a hesla používají pouze skripty s připojením k databázi.</span><span class="sxs-lookup"><span data-stu-id="0490f-119">Note that database user names and passwords are only used by scripts connecting to the database.</span></span>  <span data-ttu-id="0490f-120">Názvy databáze uživatelských účtů nutně nepředstavuje skutečný uživatelské účty v systému.</span><span class="sxs-lookup"><span data-stu-id="0490f-120">Database user account names do not necessarily represent actual user accounts on the system.</span></span>
9. <span data-ttu-id="0490f-121">Chcete-li se přihlásit z jiného počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-121">To log in from another computer, type:</span></span>
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    <span data-ttu-id="0490f-122">kde `ip-address` je IP adresa počítače, ze kterého budete se připojovat k MySQL.</span><span class="sxs-lookup"><span data-stu-id="0490f-122">where `ip-address` is the IP address of the computer from which you will connect to MySQL.</span></span>
10. <span data-ttu-id="0490f-123">Ukončete nástroj pro správu databáze MySQL, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-123">To exit the MySQL database administration utility, type:</span></span>
    
        quit

## <a name="add-an-endpoint"></a><span data-ttu-id="0490f-124">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="0490f-124">Add an endpoint</span></span>
1. <span data-ttu-id="0490f-125">Po instalaci MySQL, budete potřebovat ke konfiguraci koncového bodu vzdálený přístup MySQL.</span><span class="sxs-lookup"><span data-stu-id="0490f-125">After MySQL is installed, you'll need to configure an endpoint to access MySQL remotely.</span></span> <span data-ttu-id="0490f-126">Přihlaste se k [portál Azure classic][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="0490f-126">Log in to the [Azure  classic portal][AzurePortal].</span></span> <span data-ttu-id="0490f-127">Klikněte na tlačítko **virtuální počítače**, klikněte na název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="0490f-127">Click **Virtual Machines**, click the name of your new virtual machine, and then click **Endpoints**.</span></span>
2. <span data-ttu-id="0490f-128">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="0490f-128">Click **Add** at the bottom of the page.</span></span>
3. <span data-ttu-id="0490f-129">Přidat koncový bod s názvem "MySQL" s protokolem **TCP**, a **veřejné** a **privátní** porty nastavena na "3306".</span><span class="sxs-lookup"><span data-stu-id="0490f-129">Add an endpoint named "MySQL" with protocol **TCP**, and **Public** and **Private** ports set to "3306".</span></span>
4. <span data-ttu-id="0490f-130">Chcete-li vzdáleně připojit k virtuálnímu počítači z vašeho počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0490f-130">To remotely connect to the virtual machine from your computer, type:</span></span>
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    <span data-ttu-id="0490f-131">Například používání virtuální počítače, které jsme vytvořili v tomto kurzu, zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="0490f-131">For example, using the virual machine we created in this tutorial, type this command:</span></span>
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
