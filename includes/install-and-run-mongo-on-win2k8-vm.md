<span data-ttu-id="dcc7f-101">Postupujte podle těchto kroků k instalaci a spuštění MongoDB ve virtuálním počítači s Windows serverem.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-101">Follow these steps to install and run MongoDB on a virtual machine running Windows Server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcc7f-102">Funkce zabezpečení MongoDB, jako je ověřování a vazbu IP adresy, nejsou povolené ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-102">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="dcc7f-103">Před nasazením MongoDB v produkčním prostředí by měl povolit funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-103">Security features should be enabled before deploying MongoDB to a production environment.</span></span>  <span data-ttu-id="dcc7f-104">Další informace najdete v tématu [zabezpečení a ověřování](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="dcc7f-104">For more information, see [Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>
>
>

1. <span data-ttu-id="dcc7f-105">Po připojení k virtuálnímu počítači pomocí vzdálené plochy, otevřete Internet Explorer z **spustit** nabídky na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-105">After you've connected to the virtual machine using Remote Desktop, open Internet Explorer from the **Start** menu on the virtual machine.</span></span>
2. <span data-ttu-id="dcc7f-106">Vyberte **nástroje** tlačítko v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-106">Select the **Tools** button in the upper right corner.</span></span>  <span data-ttu-id="dcc7f-107">V **Možnosti Internetu**, vyberte **zabezpečení** a potom vyberte **Důvěryhodné servery** ikonu a nakonec klikněte **lokality** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-107">In **Internet Options**, select the **Security** tab, and then select the **Trusted Sites** icon, and finally click the **Sites** button.</span></span> <span data-ttu-id="dcc7f-108">Přidat *https://\*. mongodb.org* do seznamu důvěryhodných serverů.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-108">Add *https://\*.mongodb.org* to the list of trusted sites.</span></span>
3. <span data-ttu-id="dcc7f-109">Přejděte na [stáhne - MongoDB](https://www.mongodb.com/download-center#community).</span><span class="sxs-lookup"><span data-stu-id="dcc7f-109">Go to [Downloads - MongoDB](https://www.mongodb.com/download-center#community).</span></span>
4. <span data-ttu-id="dcc7f-110">Najít **aktuální stabilní verzi** z **komunity serveru**, vyberte nejnovější **64-bit** verze ve sloupci systému Windows.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-110">Find the **Current Stable Release** of **Community Server**, select the latest **64-bit** version in the Windows column.</span></span> <span data-ttu-id="dcc7f-111">Stáhněte a spusťte instalační program MSI.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-111">Download, then run the MSI installer.</span></span>
5. <span data-ttu-id="dcc7f-112">MongoDB je obvykle nainstalován v C:\Program Files\MongoDB.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-112">MongoDB is typically installed in C:\Program Files\MongoDB.</span></span> <span data-ttu-id="dcc7f-113">Vyhledejte proměnné prostředí na ploše a přidat cestu binární soubory MongoDB do proměnné PATH.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-113">Search for Environment Variables on the desktop and add the MongoDB binaries path to the PATH variable.</span></span> <span data-ttu-id="dcc7f-114">Například může najít binární soubory na C:\Program Files\MongoDB\Server\3.4\bin na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-114">For example, you might find the binaries at C:\Program Files\MongoDB\Server\3.4\bin on your machine.</span></span>
6. <span data-ttu-id="dcc7f-115">Vytváření adresářů MongoDB protokolu a data na disku data (například disk **F:**) jste vytvořili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-115">Create MongoDB data and log directories in the data disk (such as drive **F:**) you created in the preceding steps.</span></span> <span data-ttu-id="dcc7f-116">Z **spustit**, vyberte **příkazového řádku** otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-116">From **Start**, select **Command Prompt** to open a command prompt window.</span></span>  <span data-ttu-id="dcc7f-117">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="dcc7f-117">Type:</span></span>

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. <span data-ttu-id="dcc7f-118">Chcete-li spustit databázi, spusťte:</span><span class="sxs-lookup"><span data-stu-id="dcc7f-118">To run the database, run:</span></span>

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    <span data-ttu-id="dcc7f-119">Všechny zprávy protokolu jsou směrované *F:\MongoLogs\mongolog.log* souborů jako mongod.exe server spustí a preallocates soubory deníku.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-119">All log messages are directed to the *F:\MongoLogs\mongolog.log* file as mongod.exe server starts and preallocates journal files.</span></span> <span data-ttu-id="dcc7f-120">To může trvat několik minut, než MongoDB předběžné přidělení soubory deníku a zahájit naslouchání pro připojení.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-120">It may take several minutes for MongoDB to preallocate the journal files and start listening for connections.</span></span> <span data-ttu-id="dcc7f-121">Do příkazového řádku zůstává zaměřují se na tato úloha je spuštěna MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-121">The command prompt stays focused on this task while your MongoDB instance is running.</span></span>
8. <span data-ttu-id="dcc7f-122">Pokud chcete spustit prostředí pro správu MongoDB, otevřete další okno příkaz z **spustit** a zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="dcc7f-122">To start the MongoDB administrative shell, open another command window from **Start** and type the following commands:</span></span>

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    <span data-ttu-id="dcc7f-123">Databáze je vytvořená úlohy insert.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-123">The database is created by the insert.</span></span>
9. <span data-ttu-id="dcc7f-124">Alternativně můžete nainstalovat mongod.exe jako služba:</span><span class="sxs-lookup"><span data-stu-id="dcc7f-124">Alternatively, you can install mongod.exe as a service:</span></span>

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    <span data-ttu-id="dcc7f-125">Je nainstalována služba s názvem MongoDB s popisem "Mongo DB".</span><span class="sxs-lookup"><span data-stu-id="dcc7f-125">A service is installed named MongoDB with a description of "Mongo DB".</span></span> <span data-ttu-id="dcc7f-126">`--logpath` Možnost se musí použít k určení souboru protokolu, vzhledem k tomu, že běžící služba nemá okno příkazového řádku k zobrazení výstupu.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-126">The `--logpath` option must be used to specify a log file, since the running service does not have a command window to display output.</span></span>  <span data-ttu-id="dcc7f-127">`--logappend` Možnost určuje, že restartování služby způsobí, že výstupní má být připojen ke stávajícímu souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-127">The `--logappend` option specifies that a restart of the service causes output to append to the existing log file.</span></span>  <span data-ttu-id="dcc7f-128">`--dbpath` Možnost určuje umístění do adresáře dat.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-128">The `--dbpath` option specifies the location of the data directory.</span></span> <span data-ttu-id="dcc7f-129">Další související se službou možnosti příkazového řádku najdete v tématu [související se službou možnosti příkazového řádku][MongoWindowsSvcOptions].</span><span class="sxs-lookup"><span data-stu-id="dcc7f-129">For more service-related command-line options, see [Service-related command-line options][MongoWindowsSvcOptions].</span></span>

    <span data-ttu-id="dcc7f-130">Chcete-li spustit službu, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="dcc7f-130">To start the service, run this command:</span></span>

        C:\> net start MongoDB
10. <span data-ttu-id="dcc7f-131">Teď, když MongoDB je nainstalován a spuštěn, budete muset otevřít port v bráně Windows Firewall, abyste mohli vzdáleně připojte k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-131">Now that MongoDB is installed and running, you need to open a port in Windows Firewall so you can remotely connect to MongoDB.</span></span>  <span data-ttu-id="dcc7f-132">Z **spustit** nabídce vyberte možnost **nástroje pro správu** a potom **brány Windows Firewall s pokročilým zabezpečením**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-132">From the **Start** menu, select **Administrative Tools** and then **Windows Firewall with Advanced Security**.</span></span>
11. <span data-ttu-id="dcc7f-133">(a) v levém podokně vyberte **příchozí pravidla**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-133">a) In the left pane, select **Inbound Rules**.</span></span>  <span data-ttu-id="dcc7f-134">V **akce** podokně na pravé straně vyberte **nové pravidlo...** .</span><span class="sxs-lookup"><span data-stu-id="dcc7f-134">In the **Actions** pane on the right, select **New Rule...**.</span></span>

    ![Brána Windows Firewall][Image1]

    <span data-ttu-id="dcc7f-136">b) v **pravidla Průvodce vytvořením nového příchozího**, vyberte **Port** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-136">b) In the **New Inbound Rule Wizard**, select **Port** and then click **Next**.</span></span>

    ![Brána Windows Firewall][Image2]

    <span data-ttu-id="dcc7f-138">c) vyberte **TCP** a potom **určité místní porty**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-138">c) Select **TCP** and then **Specific local ports**.</span></span>  <span data-ttu-id="dcc7f-139">Zadejte port "27017" (výchozí port MongoDB naslouchá na) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-139">Specify a port of "27017" (the default port MongoDB listens on) and click **Next**.</span></span>

    ![Brána Windows Firewall][Image3]

    <span data-ttu-id="dcc7f-141">d) vyberte **povolit připojení** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-141">d) Select **Allow the connection** and click **Next**.</span></span>

    ![Brána Windows Firewall][Image4]

    <span data-ttu-id="dcc7f-143">e) klikněte na tlačítko **Další** znovu.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-143">e) Click **Next** again.</span></span>

    ![Brána Windows Firewall][Image5]

    <span data-ttu-id="dcc7f-145">f) zadejte název pravidla, jako je například "MongoPort" a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-145">f) Specify a name for the rule, such as "MongoPort", and click **Finish**.</span></span>

    ![Brána Windows Firewall][Image6]

12. <span data-ttu-id="dcc7f-147">Pokud koncový bod nebylo nakonfigurovat pro MongoDB, když vytvoříte virtuální počítač, můžete provést nyní.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-147">If you didn't configure an endpoint for MongoDB when you created the virtual machine, you can do it now.</span></span> <span data-ttu-id="dcc7f-148">Potřebujete pravidlo brány firewall a aby mohli vzdáleně připojit k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-148">You need both the firewall rule and the endpoint to be able to connect to MongoDB remotely.</span></span>

  <span data-ttu-id="dcc7f-149">Na portálu Azure klikněte na tlačítko **virtuálních počítačů (klasické)**, klikněte na název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-149">In the Azure portal, click **Virtual Machines (classic)**, click the name of your new virtual machine, and then click **Endpoints**.</span></span>

    ![Koncové body][Image7]

13. <span data-ttu-id="dcc7f-151">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-151">Click **Add**.</span></span>

14. <span data-ttu-id="dcc7f-152">Přidat koncový bod s názvem "Mongo", protokol **TCP**a obě **veřejné** a **privátní** porty nastavena na "27017".</span><span class="sxs-lookup"><span data-stu-id="dcc7f-152">Add an endpoint with name "Mongo", protocol **TCP**, and both **Public** and **Private** ports set to "27017".</span></span> <span data-ttu-id="dcc7f-153">MongoDB vzdálený přístup k otevření tohoto portu umožňuje.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-153">Opening this port allows MongoDB to be accessed remotely.</span></span>

    ![Koncové body][Image9]

> [!NOTE]
> <span data-ttu-id="dcc7f-155">Port 27017 je výchozí port je používán MongoDB.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-155">The port 27017 is the default port used by MongoDB.</span></span> <span data-ttu-id="dcc7f-156">Tento výchozí port můžete změnit zadáním `--port` parametr při spuštění serveru mongod.exe.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-156">You can change this default port by specifying the `--port` parameter when starting the mongod.exe server.</span></span> <span data-ttu-id="dcc7f-157">Zajistěte, aby dát stejné číslo portu v bráně firewall a endpoint "Mongo" v předchozích pokynů.</span><span class="sxs-lookup"><span data-stu-id="dcc7f-157">Make sure to give the same port number in the firewall and the "Mongo" endpoint in the preceding instructions.</span></span>
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
