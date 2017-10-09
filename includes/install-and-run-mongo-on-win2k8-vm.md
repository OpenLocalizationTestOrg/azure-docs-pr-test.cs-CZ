<span data-ttu-id="d3e4b-101">Postupujte podle těchto kroků tooinstall a spusťte MongoDB virtuálního počítače se systémem Windows Server.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-101">Follow these steps tooinstall and run MongoDB on a virtual machine running Windows Server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d3e4b-102">Funkce zabezpečení MongoDB, jako je ověřování a vazbu IP adresy, nejsou povolené ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-102">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="d3e4b-103">Před nasazením MongoDB tooa produkčním prostředí by měl povolit funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-103">Security features should be enabled before deploying MongoDB tooa production environment.</span></span>  <span data-ttu-id="d3e4b-104">Další informace najdete v tématu [zabezpečení a ověřování](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="d3e4b-104">For more information, see [Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>
>
>

1. <span data-ttu-id="d3e4b-105">Po připojení toohello virtuálního počítače pomocí vzdálené plochy, otevřete Internet Explorer z hello **spustit** nabídky hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-105">After you've connected toohello virtual machine using Remote Desktop, open Internet Explorer from hello **Start** menu on hello virtual machine.</span></span>
2. <span data-ttu-id="d3e4b-106">Vyberte hello **nástroje** tlačítko v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-106">Select hello **Tools** button in hello upper right corner.</span></span>  <span data-ttu-id="d3e4b-107">V **Možnosti Internetu**, vyberte hello **zabezpečení** a pak vyberte hello **Důvěryhodné servery** ikonu a nakonec klikněte na tlačítko hello **lokality** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-107">In **Internet Options**, select hello **Security** tab, and then select hello **Trusted Sites** icon, and finally click hello **Sites** button.</span></span> <span data-ttu-id="d3e4b-108">Přidat *https://\*. mongodb.org* toohello seznam důvěryhodných serverů.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-108">Add *https://\*.mongodb.org* toohello list of trusted sites.</span></span>
3. <span data-ttu-id="d3e4b-109">Přejděte příliš[stáhne - MongoDB](https://www.mongodb.com/download-center#community).</span><span class="sxs-lookup"><span data-stu-id="d3e4b-109">Go too[Downloads - MongoDB](https://www.mongodb.com/download-center#community).</span></span>
4. <span data-ttu-id="d3e4b-110">Najde hello **aktuální stabilní verzi** z **komunity serveru**, vyberte nejnovější hello **64-bit** verze ve sloupci Windows hello.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-110">Find hello **Current Stable Release** of **Community Server**, select hello latest **64-bit** version in hello Windows column.</span></span> <span data-ttu-id="d3e4b-111">Stáhněte a spusťte instalační program MSI hello.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-111">Download, then run hello MSI installer.</span></span>
5. <span data-ttu-id="d3e4b-112">MongoDB je obvykle nainstalován v C:\Program Files\MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-112">MongoDB is typically installed in C:\Program Files\MongoDB.</span></span> <span data-ttu-id="d3e4b-113">Vyhledejte proměnné prostředí na ploše hello a přidejte hello MongoDB binární soubory cesta toohello proměnné PATH.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-113">Search for Environment Variables on hello desktop and add hello MongoDB binaries path toohello PATH variable.</span></span> <span data-ttu-id="d3e4b-114">Například můžete zjistit hello binárních souborů v C:\Program Files\MongoDB\Server\3.4\bin na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-114">For example, you might find hello binaries at C:\Program Files\MongoDB\Server\3.4\bin on your machine.</span></span>
6. <span data-ttu-id="d3e4b-115">Vytváření adresářů MongoDB protokolu a data v hello datový disk (například disk **F:**) jste vytvořili v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-115">Create MongoDB data and log directories in hello data disk (such as drive **F:**) you created in hello preceding steps.</span></span> <span data-ttu-id="d3e4b-116">Z **spustit**, vyberte **příkazového řádku** tooopen okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-116">From **Start**, select **Command Prompt** tooopen a command prompt window.</span></span>  <span data-ttu-id="d3e4b-117">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="d3e4b-117">Type:</span></span>

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. <span data-ttu-id="d3e4b-118">toorun hello databáze, spusťte:</span><span class="sxs-lookup"><span data-stu-id="d3e4b-118">toorun hello database, run:</span></span>

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    <span data-ttu-id="d3e4b-119">Všechny zprávy protokolu jsou řízené toohello *F:\MongoLogs\mongolog.log* souborů jako mongod.exe server spustí a preallocates soubory deníku.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-119">All log messages are directed toohello *F:\MongoLogs\mongolog.log* file as mongod.exe server starts and preallocates journal files.</span></span> <span data-ttu-id="d3e4b-120">Může trvat několik minut, než MongoDB toopreallocate soubory deníku hello a zahájit naslouchání pro připojení.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-120">It may take several minutes for MongoDB toopreallocate hello journal files and start listening for connections.</span></span> <span data-ttu-id="d3e4b-121">příkazový řádek Hello zůstává zaměřují se na tato úloha je spuštěna MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-121">hello command prompt stays focused on this task while your MongoDB instance is running.</span></span>
8. <span data-ttu-id="d3e4b-122">hello toostart MongoDB prostředí pro správu, otevřete další okno příkaz z **spustit** a typ hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d3e4b-122">toostart hello MongoDB administrative shell, open another command window from **Start** and type hello following commands:</span></span>

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

    <span data-ttu-id="d3e4b-123">Hello databáze byla vytvořena ve hello insert.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-123">hello database is created by hello insert.</span></span>
9. <span data-ttu-id="d3e4b-124">Alternativně můžete nainstalovat mongod.exe jako služba:</span><span class="sxs-lookup"><span data-stu-id="d3e4b-124">Alternatively, you can install mongod.exe as a service:</span></span>

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    <span data-ttu-id="d3e4b-125">Je nainstalována služba s názvem MongoDB s popisem "Mongo DB".</span><span class="sxs-lookup"><span data-stu-id="d3e4b-125">A service is installed named MongoDB with a description of "Mongo DB".</span></span> <span data-ttu-id="d3e4b-126">Hello `--logpath` možnost musí být použité toospecify soubor protokolu, od hello spuštěna služba nemá na příkaz okna toodisplay výstup.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-126">hello `--logpath` option must be used toospecify a log file, since hello running service does not have a command window toodisplay output.</span></span>  <span data-ttu-id="d3e4b-127">Hello `--logappend` možnost určuje, že restartování služby hello způsobí, že výstupní tooappend toohello existující soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-127">hello `--logappend` option specifies that a restart of hello service causes output tooappend toohello existing log file.</span></span>  <span data-ttu-id="d3e4b-128">Hello `--dbpath` možnost určuje umístění hello hello dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-128">hello `--dbpath` option specifies hello location of hello data directory.</span></span> <span data-ttu-id="d3e4b-129">Další související se službou možnosti příkazového řádku najdete v tématu [související se službou možnosti příkazového řádku][MongoWindowsSvcOptions].</span><span class="sxs-lookup"><span data-stu-id="d3e4b-129">For more service-related command-line options, see [Service-related command-line options][MongoWindowsSvcOptions].</span></span>

    <span data-ttu-id="d3e4b-130">toostart hello služba, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3e4b-130">toostart hello service, run this command:</span></span>

        C:\> net start MongoDB
10. <span data-ttu-id="d3e4b-131">Teď, když MongoDB je nainstalován a spuštěn, je nutné tooopen port v bráně Windows Firewall, abyste mohli vzdáleně připojte tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-131">Now that MongoDB is installed and running, you need tooopen a port in Windows Firewall so you can remotely connect tooMongoDB.</span></span>  <span data-ttu-id="d3e4b-132">Z hello **spustit** nabídce vyberte možnost **nástroje pro správu** a potom **brány Windows Firewall s pokročilým zabezpečením**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-132">From hello **Start** menu, select **Administrative Tools** and then **Windows Firewall with Advanced Security**.</span></span>
11. <span data-ttu-id="d3e4b-133">(a) v levém podokně hello vyberte **příchozí pravidla**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-133">a) In hello left pane, select **Inbound Rules**.</span></span>  <span data-ttu-id="d3e4b-134">V hello **akce** podokně na hello práva, vyberte **nové pravidlo...** .</span><span class="sxs-lookup"><span data-stu-id="d3e4b-134">In hello **Actions** pane on hello right, select **New Rule...**.</span></span>

    ![Brána Windows Firewall][Image1]

    <span data-ttu-id="d3e4b-136">b) v hello **pravidla Průvodce vytvořením nového příchozího**, vyberte **Port** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-136">b) In hello **New Inbound Rule Wizard**, select **Port** and then click **Next**.</span></span>

    ![Brána Windows Firewall][Image2]

    <span data-ttu-id="d3e4b-138">c) vyberte **TCP** a potom **určité místní porty**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-138">c) Select **TCP** and then **Specific local ports**.</span></span>  <span data-ttu-id="d3e4b-139">Zadejte port "27017" (hello výchozí port MongoDB naslouchá na) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-139">Specify a port of "27017" (hello default port MongoDB listens on) and click **Next**.</span></span>

    ![Brána Windows Firewall][Image3]

    <span data-ttu-id="d3e4b-141">d) vyberte **povolit připojení hello** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-141">d) Select **Allow hello connection** and click **Next**.</span></span>

    ![Brána Windows Firewall][Image4]

    <span data-ttu-id="d3e4b-143">e) klikněte na tlačítko **Další** znovu.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-143">e) Click **Next** again.</span></span>

    ![Brána Windows Firewall][Image5]

    <span data-ttu-id="d3e4b-145">f) zadejte název pravidla hello, jako je například "MongoPort" a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-145">f) Specify a name for hello rule, such as "MongoPort", and click **Finish**.</span></span>

    ![Brána Windows Firewall][Image6]

12. <span data-ttu-id="d3e4b-147">Pokud koncový bod nenakonfigurovali pro MongoDB při vytváření hello virtuálního počítače, můžete provést nyní.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-147">If you didn't configure an endpoint for MongoDB when you created hello virtual machine, you can do it now.</span></span> <span data-ttu-id="d3e4b-148">Je třeba pravidla brány firewall hello i hello koncový bod toobe možné tooconnect tooMongoDB vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-148">You need both hello firewall rule and hello endpoint toobe able tooconnect tooMongoDB remotely.</span></span>

  <span data-ttu-id="d3e4b-149">V hello portálu Azure, klikněte na **virtuálních počítačů (klasické)**, klikněte na tlačítko hello název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-149">In hello Azure portal, click **Virtual Machines (classic)**, click hello name of your new virtual machine, and then click **Endpoints**.</span></span>

    ![Koncové body][Image7]

13. <span data-ttu-id="d3e4b-151">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-151">Click **Add**.</span></span>

14. <span data-ttu-id="d3e4b-152">Přidat koncový bod s názvem "Mongo", protokol **TCP**a obě **veřejné** a **privátní** porty sadu příliš "27017".</span><span class="sxs-lookup"><span data-stu-id="d3e4b-152">Add an endpoint with name "Mongo", protocol **TCP**, and both **Public** and **Private** ports set too"27017".</span></span> <span data-ttu-id="d3e4b-153">Otevření tento port umožňuje přístup ke vzdálené toobe MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-153">Opening this port allows MongoDB toobe accessed remotely.</span></span>

    ![Koncové body][Image9]

> [!NOTE]
> <span data-ttu-id="d3e4b-155">Hello 27017 je hello výchozí port používaný MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-155">hello port 27017 is hello default port used by MongoDB.</span></span> <span data-ttu-id="d3e4b-156">Tento výchozí port můžete změnit zadáním hello `--port` parametr při spuštění serveru mongod.exe hello.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-156">You can change this default port by specifying hello `--port` parameter when starting hello mongod.exe server.</span></span> <span data-ttu-id="d3e4b-157">Ujistěte se, že toogive hello stejné číslo portu v bráně firewall hello a hello "Mongo" koncového bodu v předchozích pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="d3e4b-157">Make sure toogive hello same port number in hello firewall and hello "Mongo" endpoint in hello preceding instructions.</span></span>
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
