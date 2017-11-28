---
title: "aaaInstall MongoDB na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall MongoDB ve virtuálním počítači Azure s Windows serverem 2012 R2 vytvořené pomocí modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: becd2c607d098e2bc806139e03f2c42f1f01f6f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="a85d7-103">Instalace a konfigurace MongoDB na virtuální počítač s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="a85d7-103">Install and configure MongoDB on a Windows VM in Azure</span></span>
<span data-ttu-id="a85d7-104">[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="a85d7-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="a85d7-105">Tento článek vás provede instalací a konfigurací MongoDB na Windows Server 2012 R2 virtuálního počítače (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="a85d7-105">This article guides you through installing and configuring MongoDB on a Windows Server 2012 R2 virtual machine (VM) in Azure.</span></span> <span data-ttu-id="a85d7-106">Můžete také [nainstalujte MongoDB na virtuální počítač s Linuxem v Azure](../linux/install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="a85d7-106">You can also [install MongoDB on a Linux VM in Azure](../linux/install-mongodb.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a85d7-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a85d7-107">Prerequisites</span></span>
<span data-ttu-id="a85d7-108">Než nainstalujete a nakonfigurujete MongoDB, můžete potřebovat toocreate virtuálního počítače a, v ideálním případě přidat tooit disku data.</span><span class="sxs-lookup"><span data-stu-id="a85d7-108">Before you install and configure MongoDB, you need toocreate a VM and, ideally, add a data disk tooit.</span></span> <span data-ttu-id="a85d7-109">Zobrazit hello následující články toocreate virtuální počítač a přidat datový disk:</span><span class="sxs-lookup"><span data-stu-id="a85d7-109">See hello following articles toocreate a VM and add a data disk:</span></span>

* <span data-ttu-id="a85d7-110">Vytvoření virtuálního počítače Windows serveru pomocí [hello portál Azure](quick-create-portal.md) nebo [prostředí Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a85d7-110">Create a Windows Server VM using [hello Azure portal](quick-create-portal.md) or [Azure PowerShell](quick-create-powershell.md).</span></span>
* <span data-ttu-id="a85d7-111">Připojit datový disk tooa virtuálního počítače Windows serveru pomocí [hello portál Azure](attach-managed-disk-portal.md) nebo [prostředí Azure PowerShell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a85d7-111">Attach a data disk tooa Windows Server VM using [hello Azure portal](attach-managed-disk-portal.md) or [Azure PowerShell](attach-disk-ps.md).</span></span>

<span data-ttu-id="a85d7-112">instalace a konfigurace MongoDB, toobegin [přihlásit tooyour virtuálního počítače Windows serveru](connect-logon.md) pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="a85d7-112">toobegin installing and configuring MongoDB, [log on tooyour Windows Server VM](connect-logon.md) by using Remote Desktop.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="a85d7-113">Instalace MongoDB</span><span class="sxs-lookup"><span data-stu-id="a85d7-113">Install MongoDB</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a85d7-114">Funkce zabezpečení MongoDB, jako je ověřování a vazbu IP adresy, nejsou povolené ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a85d7-114">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="a85d7-115">Před nasazením MongoDB tooa produkčním prostředí by měl povolit funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a85d7-115">Security features should be enabled before deploying MongoDB tooa production environment.</span></span> <span data-ttu-id="a85d7-116">Další informace najdete v tématu [MongoDB zabezpečení a ověřování](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="a85d7-116">For more information, see [MongoDB Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>


1. <span data-ttu-id="a85d7-117">Po připojení tooyour virtuálního počítače pomocí vzdálené plochy, otevřete Internet Explorer z hello **spustit** nabídce hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a85d7-117">After you've connected tooyour VM using Remote Desktop, open Internet Explorer from hello **Start** menu on hello VM.</span></span>
2. <span data-ttu-id="a85d7-118">Vyberte **doporučuje použít nastavení zabezpečení, ochrany osobních údajů a kompatibility** když Internet Explorer nejprve otevře a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-118">Select **Use recommended security, privacy, and compatibility settings** when Internet Explorer first opens, and click **OK**.</span></span>
3. <span data-ttu-id="a85d7-119">Konfigurace rozšířeného zabezpečení aplikace Internet Explorer je standardně povolená.</span><span class="sxs-lookup"><span data-stu-id="a85d7-119">Internet Explorer enhanced security configuration is enabled by default.</span></span> <span data-ttu-id="a85d7-120">Přidejte hello MongoDB webu toohello seznamu povolených webů:</span><span class="sxs-lookup"><span data-stu-id="a85d7-120">Add hello MongoDB website toohello list of allowed sites:</span></span>
   
   * <span data-ttu-id="a85d7-121">Vyberte hello **nástroje** ikonu v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="a85d7-121">Select hello **Tools** icon in hello upper-right corner.</span></span>
   * <span data-ttu-id="a85d7-122">V **Možnosti Internetu**, vyberte hello **zabezpečení** a pak vyberte hello **Důvěryhodné servery** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a85d7-122">In **Internet Options**, select hello **Security** tab, and then select hello **Trusted Sites** icon.</span></span>
   * <span data-ttu-id="a85d7-123">Klikněte na tlačítko hello **lokality** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a85d7-123">Click hello **Sites** button.</span></span> <span data-ttu-id="a85d7-124">Přidat *https://\*. mongodb.org* toohello seznam důvěryhodných serverů a pak zavřete hello dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a85d7-124">Add *https://\*.mongodb.org* toohello list of trusted sites, and then close hello dialog box.</span></span>
     
     ![Konfigurovat nastavení zabezpečení aplikace Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. <span data-ttu-id="a85d7-126">Procházet toohello [MongoDB - stáhne](http://www.mongodb.org/downloads) stránky (http://www.mongodb.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="a85d7-126">Browse toohello [MongoDB - Downloads](http://www.mongodb.org/downloads) page (http://www.mongodb.org/downloads).</span></span>
5. <span data-ttu-id="a85d7-127">V případě potřeby vyberte hello **komunity serveru** edition a pak vyberte hello nejnovější aktuální stabilní verzi pro Windows Server 2008 R2 64-bit a novější.</span><span class="sxs-lookup"><span data-stu-id="a85d7-127">If needed, select hello **Community Server** edition and then select hello latest current stable release for Windows Server 2008 R2 64-bit and later.</span></span> <span data-ttu-id="a85d7-128">toodownload hello instalačního programu, klikněte na tlačítko **stahování (msi)**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-128">toodownload hello installer, click **DOWNLOAD (msi)**.</span></span>
   
    ![Stažení instalačního programu MongoDB](./media/install-mongodb/download-mongodb.png)
   
    <span data-ttu-id="a85d7-130">Spusťte instalační program hello, po dokončení stahování hello.</span><span class="sxs-lookup"><span data-stu-id="a85d7-130">Run hello installer after hello download is complete.</span></span>
6. <span data-ttu-id="a85d7-131">Přečtěte si a přijměte licenční smlouvu hello.</span><span class="sxs-lookup"><span data-stu-id="a85d7-131">Read and accept hello license agreement.</span></span> <span data-ttu-id="a85d7-132">Když se zobrazí výzva, vyberte **Complete** nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="a85d7-132">When you're prompted, select **Complete** install.</span></span>
7. <span data-ttu-id="a85d7-133">Na úvodní poslední obrazovka, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-133">On hello final screen, click **Install**.</span></span>

## <a name="configure-hello-vm-and-mongodb"></a><span data-ttu-id="a85d7-134">Konfigurace hello virtuálních počítačů a MongoDB</span><span class="sxs-lookup"><span data-stu-id="a85d7-134">Configure hello VM and MongoDB</span></span>
1. <span data-ttu-id="a85d7-135">proměnné cest Hello neaktualizují hello MongoDB instalační službou.</span><span class="sxs-lookup"><span data-stu-id="a85d7-135">hello path variables are not updated by hello MongoDB installer.</span></span> <span data-ttu-id="a85d7-136">Bez hello MongoDB `bin` umístění vaše proměnná path, potřebujete úplná cesta toospecify hello pokaždé, když používáte spustitelný soubor MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a85d7-136">Without hello MongoDB `bin` location in your path variable, you need toospecify hello full path each time you use a MongoDB executable.</span></span> <span data-ttu-id="a85d7-137">Proměnná tooadd hello umístění tooyour cesty:</span><span class="sxs-lookup"><span data-stu-id="a85d7-137">tooadd hello location tooyour path variable:</span></span>
   
   * <span data-ttu-id="a85d7-138">Klikněte pravým tlačítkem na hello **spustit** nabídce a vyberte **systému**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-138">Right-click hello **Start** menu, and select **System**.</span></span>
   * <span data-ttu-id="a85d7-139">Klikněte na tlačítko **Upřesnit nastavení systému**a potom klikněte na **proměnné prostředí**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-139">Click **Advanced system settings**, and then click **Environment Variables**.</span></span>
   * <span data-ttu-id="a85d7-140">V části **systémové proměnné**, vyberte **cesta**a potom klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-140">Under **System variables**, select **Path**, and then click **Edit**.</span></span>
     
     ![Nakonfigurujte proměnné cest](./media/install-mongodb/configure-path-variables.png)
     
     <span data-ttu-id="a85d7-142">Přidat cestu tooyour hello MongoDB `bin` složky.</span><span class="sxs-lookup"><span data-stu-id="a85d7-142">Add hello path tooyour MongoDB `bin` folder.</span></span> <span data-ttu-id="a85d7-143">MongoDB je obvykle nainstalovaný v *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="a85d7-143">MongoDB is typically installed in *C:\Program Files\MongoDB*.</span></span> <span data-ttu-id="a85d7-144">Ověřte cestu instalace hello na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a85d7-144">Verify hello installation path on your VM.</span></span> <span data-ttu-id="a85d7-145">Hello následující příklad přidá hello výchozí MongoDB instalace umístění toohello `PATH` proměnné:</span><span class="sxs-lookup"><span data-stu-id="a85d7-145">hello following example adds hello default MongoDB install location toohello `PATH` variable:</span></span>
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > <span data-ttu-id="a85d7-146">Být jisti středník před hello tooadd (`;`) tooindicate přidáváte umístění tooyour `PATH` proměnné.</span><span class="sxs-lookup"><span data-stu-id="a85d7-146">Be sure tooadd hello leading semicolon (`;`) tooindicate that you are adding a location tooyour `PATH` variable.</span></span>

2. <span data-ttu-id="a85d7-147">Vytváření adresářů MongoDB protokolu a data na datový disk.</span><span class="sxs-lookup"><span data-stu-id="a85d7-147">Create MongoDB data and log directories on your data disk.</span></span> <span data-ttu-id="a85d7-148">Z hello **spustit** nabídce vyberte možnost **příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="a85d7-148">From hello **Start** menu, select **Command Prompt**.</span></span> <span data-ttu-id="a85d7-149">Následující příklady Hello na jednotce F: Vytvořte hello adresáře</span><span class="sxs-lookup"><span data-stu-id="a85d7-149">hello following examples create hello directories on drive F:</span></span>
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. <span data-ttu-id="a85d7-150">MongoDB instance začínat hello následující příkaz, úpravě data tooyour hello cesty a odpovídajícím způsobem protokolu adresáře:</span><span class="sxs-lookup"><span data-stu-id="a85d7-150">Start a MongoDB instance with hello following command, adjusting hello path tooyour data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    <span data-ttu-id="a85d7-151">Může trvat několik minut, než MongoDB tooallocate soubory deníku hello a zahájit naslouchání pro připojení.</span><span class="sxs-lookup"><span data-stu-id="a85d7-151">It may take several minutes for MongoDB tooallocate hello journal files and start listening for connections.</span></span> <span data-ttu-id="a85d7-152">Všechny zprávy protokolu jsou řízené toohello *F:\MongoLogs\mongolog.log* souboru jako `mongod.exe` serveru spustí a přiděluje soubory deníku.</span><span class="sxs-lookup"><span data-stu-id="a85d7-152">All log messages are directed toohello *F:\MongoLogs\mongolog.log* file as `mongod.exe` server starts and allocates journal files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a85d7-153">příkazový řádek Hello zůstává zaměřují se na tato úloha je spuštěna MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="a85d7-153">hello command prompt stays focused on this task while your MongoDB instance is running.</span></span> <span data-ttu-id="a85d7-154">Ponechte hello příkazové okno otevřete toocontinue s MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a85d7-154">Leave hello command prompt window open toocontinue running MongoDB.</span></span> <span data-ttu-id="a85d7-155">Nebo nainstalujte MongoDB jako služba, podle popisu v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a85d7-155">Or, install MongoDB as service, as detailed in hello next step.</span></span>

4. <span data-ttu-id="a85d7-156">Robustnější prostředí MongoDB, nainstalujte hello `mongod.exe` jako služba.</span><span class="sxs-lookup"><span data-stu-id="a85d7-156">For a more robust MongoDB experience, install hello `mongod.exe` as a service.</span></span> <span data-ttu-id="a85d7-157">Vytvoření služby znamená, že nepotřebujete tooleave příkazový řádek s pokaždé, když chcete toouse MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a85d7-157">Creating a service means you don't need tooleave a command prompt running each time you want toouse MongoDB.</span></span> <span data-ttu-id="a85d7-158">Vytvoření služby hello následujícím způsobem, upraví hello cesta tooyour protokolu a data adresáře odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a85d7-158">Create hello service as follows, adjusting hello path tooyour data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    <span data-ttu-id="a85d7-159">Hello předchozí příkaz vytvoří službu s názvem MongoDB, s popisem "Mongo DB".</span><span class="sxs-lookup"><span data-stu-id="a85d7-159">hello preceding command creates a service named MongoDB, with a description of "Mongo DB".</span></span> <span data-ttu-id="a85d7-160">zadáno také Hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="a85d7-160">hello following parameters are also specified:</span></span>
   
   * <span data-ttu-id="a85d7-161">Hello `--dbpath` možnost určuje umístění hello hello dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="a85d7-161">hello `--dbpath` option specifies hello location of hello data directory.</span></span>
   * <span data-ttu-id="a85d7-162">Hello `--logpath` možnost musí být použité toospecify soubor protokolu, protože spuštěnou službu hello nemá toodisplay okno výstupu příkazu.</span><span class="sxs-lookup"><span data-stu-id="a85d7-162">hello `--logpath` option must be used toospecify a log file, because hello running service does not have a command window toodisplay output.</span></span>
   * <span data-ttu-id="a85d7-163">Hello `--logappend` možnost určuje, že restartování služby hello způsobí, že výstupní tooappend toohello existující soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="a85d7-163">hello `--logappend` option specifies that a restart of hello service causes output tooappend toohello existing log file.</span></span>
   
   <span data-ttu-id="a85d7-164">toostart hello MongoDB službu, spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a85d7-164">toostart hello MongoDB service, run hello following command:</span></span>
   
    ```
    net start MongoDB
    ```
   
    <span data-ttu-id="a85d7-165">Další informace o vytváření hello MongoDB služby najdete v tématu [konfigurace služby systému Windows pro MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="a85d7-165">For more information about creating hello MongoDB service, see [Configure a Windows Service for MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span></span>

## <a name="test-hello-mongodb-instance"></a><span data-ttu-id="a85d7-166">Instance MongoDB hello testu</span><span class="sxs-lookup"><span data-stu-id="a85d7-166">Test hello MongoDB instance</span></span>
<span data-ttu-id="a85d7-167">S MongoDB spuštěna jako jedna instance nebo nainstalovat jako službu můžete nyní spustit vytváření a používání vašich databází.</span><span class="sxs-lookup"><span data-stu-id="a85d7-167">With MongoDB running as a single instance or installed as a service, you can now start creating and using your databases.</span></span> <span data-ttu-id="a85d7-168">hello toostart MongoDB prostředí pro správu, otevřete další okno příkazového řádku z hello **spustit** nabídce a zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="a85d7-168">toostart hello MongoDB administrative shell, open another command prompt window from hello **Start** menu, and enter hello following command:</span></span>

```
mongo  
```

<span data-ttu-id="a85d7-169">Můžete vytvořit seznam hello databáze s hello `db` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a85d7-169">You can list hello databases with hello `db` command.</span></span> <span data-ttu-id="a85d7-170">Vložte některá data následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a85d7-170">Insert some data as follows:</span></span>

```
db.foo.insert( { a : 1 } )
```

<span data-ttu-id="a85d7-171">Vyhledejte data následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a85d7-171">Search for data as follows:</span></span>

```
db.foo.find()
```

<span data-ttu-id="a85d7-172">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="a85d7-172">hello output is similar toohello following example:</span></span>

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

<span data-ttu-id="a85d7-173">Ukončení hello `mongo` konzole následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a85d7-173">Exit hello `mongo` console as follows:</span></span>

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a><span data-ttu-id="a85d7-174">Konfigurace pravidel skupin zabezpečení sítě a brány firewall</span><span class="sxs-lookup"><span data-stu-id="a85d7-174">Configure firewall and Network Security Group rules</span></span>
<span data-ttu-id="a85d7-175">Teď, když MongoDB je nainstalovaná a spuštěná, aby se mohli vzdáleně připojit tooMongoDB otevřete port v bráně Windows Firewall.</span><span class="sxs-lookup"><span data-stu-id="a85d7-175">Now that MongoDB is installed and running, open a port in Windows Firewall so you can remotely connect tooMongoDB.</span></span> <span data-ttu-id="a85d7-176">toocreate nový port TCP příchozí pravidlo tooallow 27017, otevřete Správce příkazovém řádku prostředí PowerShell a zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a85d7-176">toocreate a new inbound rule tooallow TCP port 27017, open an administrative PowerShell prompt and enter hello following command:</span></span>

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

<span data-ttu-id="a85d7-177">Můžete také vytvořit pravidlo hello pomocí hello **brány Windows Firewall s pokročilým zabezpečením** nástroj pro správu grafického rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a85d7-177">You can also create hello rule by using hello **Windows Firewall with Advanced Security** graphical management tool.</span></span> <span data-ttu-id="a85d7-178">Vytvořte nový port TCP příchozí pravidlo tooallow 27017.</span><span class="sxs-lookup"><span data-stu-id="a85d7-178">Create a new inbound rule tooallow TCP port 27017.</span></span>

<span data-ttu-id="a85d7-179">V případě potřeby vytvořte skupinu zabezpečení sítě pravidlo tooallow přístup tooMongoDB z mimo hello existující podsíť virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="a85d7-179">If needed, create a Network Security Group rule tooallow access tooMongoDB from outside of hello existing Azure virtual network subnet.</span></span> <span data-ttu-id="a85d7-180">Můžete vytvořit hello pravidel skupin zabezpečení sítě pomocí hello [portál Azure](nsg-quickstart-portal.md) nebo [prostředí Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a85d7-180">You can create hello Network Security Group rules by using hello [Azure portal](nsg-quickstart-portal.md) or [Azure PowerShell](nsg-quickstart-powershell.md).</span></span> <span data-ttu-id="a85d7-181">Stejně jako u hello pravidel brány Windows Firewall, povolit TCP port 27017 toohello rozhraní virtuální sítě virtuálního počítače MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a85d7-181">As with hello Windows Firewall rules, allow TCP port 27017 toohello virtual network interface of your MongoDB VM.</span></span>

> [!NOTE]
> <span data-ttu-id="a85d7-182">TCP port 27017 je výchozí port hello používá MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a85d7-182">TCP port 27017 is hello default port used by MongoDB.</span></span> <span data-ttu-id="a85d7-183">Tento port můžete změnit pomocí hello `--port` parametr při spouštění `mongod.exe` ručně nebo ze služby.</span><span class="sxs-lookup"><span data-stu-id="a85d7-183">You can change this port by using hello `--port` parameter when starting `mongod.exe` manually or from a service.</span></span> <span data-ttu-id="a85d7-184">Pokud změníte hello port, ujistěte se, že tooupdate hello brány Windows Firewall a skupinu zabezpečení sítě pravidla v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="a85d7-184">If you change hello port, make sure tooupdate hello Windows Firewall and Network Security Group rules in hello preceding steps.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a85d7-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a85d7-185">Next steps</span></span>
<span data-ttu-id="a85d7-186">V tomto kurzu jste se naučili jak tooinstall a konfigurace MongoDB na váš virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="a85d7-186">In this tutorial, you learned how tooinstall and configure MongoDB on your Windows VM.</span></span> <span data-ttu-id="a85d7-187">Nyní můžete k MongoDB na vašem virtuálním počítači Windows pomocí následující hello advanced témata v hello [dokumentace k MongoDB](https://docs.mongodb.com/manual/).</span><span class="sxs-lookup"><span data-stu-id="a85d7-187">You can now access MongoDB on your Windows VM, by following hello advanced topics in hello [MongoDB documentation](https://docs.mongodb.com/manual/).</span></span>

