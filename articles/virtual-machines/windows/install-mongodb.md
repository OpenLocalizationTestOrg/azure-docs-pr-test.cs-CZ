---
title: "Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Naučte se nainstalovat na virtuální počítač Azure s Windows serverem 2012 R2 vytvořené pomocí modelu nasazení Resource Manager MongoDB."
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
ms.openlocfilehash: db1a550b9273925b304fe4280f2a1b0e115f856d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="47b3b-103">Instalace a konfigurace MongoDB na virtuální počítač s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="47b3b-103">Install and configure MongoDB on a Windows VM in Azure</span></span>
<span data-ttu-id="47b3b-104">[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="47b3b-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="47b3b-105">Tento článek vás provede instalací a konfigurací MongoDB na Windows Server 2012 R2 virtuálního počítače (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="47b3b-105">This article guides you through installing and configuring MongoDB on a Windows Server 2012 R2 virtual machine (VM) in Azure.</span></span> <span data-ttu-id="47b3b-106">Můžete také [nainstalujte MongoDB na virtuální počítač s Linuxem v Azure](../linux/install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="47b3b-106">You can also [install MongoDB on a Linux VM in Azure](../linux/install-mongodb.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47b3b-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="47b3b-107">Prerequisites</span></span>
<span data-ttu-id="47b3b-108">Než nainstalujete a nakonfigurujete MongoDB, musíte vytvořit virtuální počítač a v ideálním případě přidat datový disk do ní.</span><span class="sxs-lookup"><span data-stu-id="47b3b-108">Before you install and configure MongoDB, you need to create a VM and, ideally, add a data disk to it.</span></span> <span data-ttu-id="47b3b-109">Najdete v následujících článcích pro vytvoření virtuálního počítače a přidat datový disk:</span><span class="sxs-lookup"><span data-stu-id="47b3b-109">See the following articles to create a VM and add a data disk:</span></span>

* <span data-ttu-id="47b3b-110">Vytvoření virtuálního počítače Windows serveru pomocí [portálu Azure](quick-create-portal.md) nebo [prostředí Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="47b3b-110">Create a Windows Server VM using [the Azure portal](quick-create-portal.md) or [Azure PowerShell](quick-create-powershell.md).</span></span>
* <span data-ttu-id="47b3b-111">Připojit datový disk do virtuálního počítače Windows serveru pomocí [portálu Azure](attach-managed-disk-portal.md) nebo [prostředí Azure PowerShell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="47b3b-111">Attach a data disk to a Windows Server VM using [the Azure portal](attach-managed-disk-portal.md) or [Azure PowerShell](attach-disk-ps.md).</span></span>

<span data-ttu-id="47b3b-112">Zahájíte instalaci a konfiguraci MongoDB, [Přihlaste se k vaší virtuální počítač Windows serveru](connect-logon.md) pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="47b3b-112">To begin installing and configuring MongoDB, [log on to your Windows Server VM](connect-logon.md) by using Remote Desktop.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="47b3b-113">Instalace MongoDB</span><span class="sxs-lookup"><span data-stu-id="47b3b-113">Install MongoDB</span></span>
> [!IMPORTANT]
> <span data-ttu-id="47b3b-114">Funkce zabezpečení MongoDB, jako je ověřování a vazbu IP adresy, nejsou povolené ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="47b3b-114">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="47b3b-115">Před nasazením MongoDB v produkčním prostředí by měl povolit funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="47b3b-115">Security features should be enabled before deploying MongoDB to a production environment.</span></span> <span data-ttu-id="47b3b-116">Další informace najdete v tématu [MongoDB zabezpečení a ověřování](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="47b3b-116">For more information, see [MongoDB Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>


1. <span data-ttu-id="47b3b-117">Po připojení k virtuálnímu počítači pomocí vzdálené plochy, otevřete Internet Explorer z **spustit** nabídky na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="47b3b-117">After you've connected to your VM using Remote Desktop, open Internet Explorer from the **Start** menu on the VM.</span></span>
2. <span data-ttu-id="47b3b-118">Vyberte **doporučuje použít nastavení zabezpečení, ochrany osobních údajů a kompatibility** když Internet Explorer nejprve otevře a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-118">Select **Use recommended security, privacy, and compatibility settings** when Internet Explorer first opens, and click **OK**.</span></span>
3. <span data-ttu-id="47b3b-119">Konfigurace rozšířeného zabezpečení aplikace Internet Explorer je standardně povolená.</span><span class="sxs-lookup"><span data-stu-id="47b3b-119">Internet Explorer enhanced security configuration is enabled by default.</span></span> <span data-ttu-id="47b3b-120">Přidejte web MongoDB do seznamu povolených webů:</span><span class="sxs-lookup"><span data-stu-id="47b3b-120">Add the MongoDB website to the list of allowed sites:</span></span>
   
   * <span data-ttu-id="47b3b-121">Vyberte **nástroje** ikonu v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="47b3b-121">Select the **Tools** icon in the upper-right corner.</span></span>
   * <span data-ttu-id="47b3b-122">V **Možnosti Internetu**, vyberte **zabezpečení** a pak vyberte **Důvěryhodné servery** ikonu.</span><span class="sxs-lookup"><span data-stu-id="47b3b-122">In **Internet Options**, select the **Security** tab, and then select the **Trusted Sites** icon.</span></span>
   * <span data-ttu-id="47b3b-123">Klikněte **lokality** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="47b3b-123">Click the **Sites** button.</span></span> <span data-ttu-id="47b3b-124">Přidat *https://\*. mongodb.org* do seznamu důvěryhodných serverů a pak zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="47b3b-124">Add *https://\*.mongodb.org* to the list of trusted sites, and then close the dialog box.</span></span>
     
     ![Konfigurovat nastavení zabezpečení aplikace Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. <span data-ttu-id="47b3b-126">Vyhledejte [MongoDB - stáhne](http://www.mongodb.org/downloads) stránky (http://www.mongodb.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="47b3b-126">Browse to the [MongoDB - Downloads](http://www.mongodb.org/downloads) page (http://www.mongodb.org/downloads).</span></span>
5. <span data-ttu-id="47b3b-127">V případě potřeby vyberte **komunity serveru** edition a potom vyberte nejnovější stabilní aktuální verzi pro Windows Server 2008 R2 64-bit a později.</span><span class="sxs-lookup"><span data-stu-id="47b3b-127">If needed, select the **Community Server** edition and then select the latest current stable release for Windows Server 2008 R2 64-bit and later.</span></span> <span data-ttu-id="47b3b-128">Chcete-li stáhnout instalační program, klikněte na tlačítko **stahování (msi)**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-128">To download the installer, click **DOWNLOAD (msi)**.</span></span>
   
    ![Stažení instalačního programu MongoDB](./media/install-mongodb/download-mongodb.png)
   
    <span data-ttu-id="47b3b-130">Po dokončení stahování spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="47b3b-130">Run the installer after the download is complete.</span></span>
6. <span data-ttu-id="47b3b-131">Přečtěte si a přijměte licenční smlouvu.</span><span class="sxs-lookup"><span data-stu-id="47b3b-131">Read and accept the license agreement.</span></span> <span data-ttu-id="47b3b-132">Když se zobrazí výzva, vyberte **Complete** nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="47b3b-132">When you're prompted, select **Complete** install.</span></span>
7. <span data-ttu-id="47b3b-133">Na poslední obrazovka, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-133">On the final screen, click **Install**.</span></span>

## <a name="configure-the-vm-and-mongodb"></a><span data-ttu-id="47b3b-134">Konfigurace virtuálního počítače a MongoDB</span><span class="sxs-lookup"><span data-stu-id="47b3b-134">Configure the VM and MongoDB</span></span>
1. <span data-ttu-id="47b3b-135">Proměnné cest nejsou aktualizovat instalační program MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47b3b-135">The path variables are not updated by the MongoDB installer.</span></span> <span data-ttu-id="47b3b-136">Bez MongoDB `bin` umístění vaše proměnná path, musíte zadat úplnou cestu pokaždé, když používáte spustitelný soubor MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47b3b-136">Without the MongoDB `bin` location in your path variable, you need to specify the full path each time you use a MongoDB executable.</span></span> <span data-ttu-id="47b3b-137">Přidání umístění do vaše proměnná path:</span><span class="sxs-lookup"><span data-stu-id="47b3b-137">To add the location to your path variable:</span></span>
   
   * <span data-ttu-id="47b3b-138">Klikněte pravým tlačítkem myši **spustit** nabídce a vyberte **systému**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-138">Right-click the **Start** menu, and select **System**.</span></span>
   * <span data-ttu-id="47b3b-139">Klikněte na tlačítko **Upřesnit nastavení systému**a potom klikněte na **proměnné prostředí**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-139">Click **Advanced system settings**, and then click **Environment Variables**.</span></span>
   * <span data-ttu-id="47b3b-140">V části **systémové proměnné**, vyberte **cesta**a potom klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-140">Under **System variables**, select **Path**, and then click **Edit**.</span></span>
     
     ![Nakonfigurujte proměnné cest](./media/install-mongodb/configure-path-variables.png)
     
     <span data-ttu-id="47b3b-142">Přidejte cestu k vaší MongoDB `bin` složky.</span><span class="sxs-lookup"><span data-stu-id="47b3b-142">Add the path to your MongoDB `bin` folder.</span></span> <span data-ttu-id="47b3b-143">MongoDB je obvykle nainstalovaný v *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="47b3b-143">MongoDB is typically installed in *C:\Program Files\MongoDB*.</span></span> <span data-ttu-id="47b3b-144">Ověření cesty instalace na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="47b3b-144">Verify the installation path on your VM.</span></span> <span data-ttu-id="47b3b-145">Následující příklad přidá výchozí umístění pro instalace MongoDB `PATH` proměnné:</span><span class="sxs-lookup"><span data-stu-id="47b3b-145">The following example adds the default MongoDB install location to the `PATH` variable:</span></span>
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > <span data-ttu-id="47b3b-146">Nezapomeňte přidat středník před (`;`) k označení, že přidáváte umístění, do kterého vaše `PATH` proměnné.</span><span class="sxs-lookup"><span data-stu-id="47b3b-146">Be sure to add the leading semicolon (`;`) to indicate that you are adding a location to your `PATH` variable.</span></span>

2. <span data-ttu-id="47b3b-147">Vytváření adresářů MongoDB protokolu a data na datový disk.</span><span class="sxs-lookup"><span data-stu-id="47b3b-147">Create MongoDB data and log directories on your data disk.</span></span> <span data-ttu-id="47b3b-148">Z **spustit** nabídce vyberte možnost **příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="47b3b-148">From the **Start** menu, select **Command Prompt**.</span></span> <span data-ttu-id="47b3b-149">Následující příklady vytvořte na jednotce F: adresáře</span><span class="sxs-lookup"><span data-stu-id="47b3b-149">The following examples create the directories on drive F:</span></span>
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. <span data-ttu-id="47b3b-150">Spuštění MongoDB instance pomocí následujícího příkazu, úpravě cesty k datům a odpovídajícím způsobem protokolu adresáře:</span><span class="sxs-lookup"><span data-stu-id="47b3b-150">Start a MongoDB instance with the following command, adjusting the path to your data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    <span data-ttu-id="47b3b-151">To může trvat několik minut, než MongoDB přidělit soubory deníku a zahájit naslouchání pro připojení.</span><span class="sxs-lookup"><span data-stu-id="47b3b-151">It may take several minutes for MongoDB to allocate the journal files and start listening for connections.</span></span> <span data-ttu-id="47b3b-152">Všechny zprávy protokolu jsou směrované *F:\MongoLogs\mongolog.log* souboru jako `mongod.exe` serveru spustí a přiděluje soubory deníku.</span><span class="sxs-lookup"><span data-stu-id="47b3b-152">All log messages are directed to the *F:\MongoLogs\mongolog.log* file as `mongod.exe` server starts and allocates journal files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="47b3b-153">Do příkazového řádku zůstává zaměřují se na tato úloha je spuštěna MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="47b3b-153">The command prompt stays focused on this task while your MongoDB instance is running.</span></span> <span data-ttu-id="47b3b-154">Nechte okno příkazového řádku otevřené pokračovat v provozu MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47b3b-154">Leave the command prompt window open to continue running MongoDB.</span></span> <span data-ttu-id="47b3b-155">Nebo nainstalujte MongoDB jako služba, jak je podrobně uvedeno v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="47b3b-155">Or, install MongoDB as service, as detailed in the next step.</span></span>

4. <span data-ttu-id="47b3b-156">Robustnější prostředí MongoDB, nainstalujte `mongod.exe` jako služba.</span><span class="sxs-lookup"><span data-stu-id="47b3b-156">For a more robust MongoDB experience, install the `mongod.exe` as a service.</span></span> <span data-ttu-id="47b3b-157">Vytvoření služby znamená, že je nebudete muset ponechte příkazový řádek s pokaždé, když chcete použít MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47b3b-157">Creating a service means you don't need to leave a command prompt running each time you want to use MongoDB.</span></span> <span data-ttu-id="47b3b-158">Vytvoření služby následujícím způsobem, upraví odpovídajícím způsobem cestu do vašich adresářů protokolu a data:</span><span class="sxs-lookup"><span data-stu-id="47b3b-158">Create the service as follows, adjusting the path to your data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    <span data-ttu-id="47b3b-159">Předchozí příkaz vytvoří službu s názvem MongoDB, s popisem "Mongo DB".</span><span class="sxs-lookup"><span data-stu-id="47b3b-159">The preceding command creates a service named MongoDB, with a description of "Mongo DB".</span></span> <span data-ttu-id="47b3b-160">Jsou také zadat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="47b3b-160">The following parameters are also specified:</span></span>
   
   * <span data-ttu-id="47b3b-161">`--dbpath` Možnost určuje umístění do adresáře dat.</span><span class="sxs-lookup"><span data-stu-id="47b3b-161">The `--dbpath` option specifies the location of the data directory.</span></span>
   * <span data-ttu-id="47b3b-162">`--logpath` Možnost se musí použít k určení souboru protokolu, protože spuštěné služby nemá okno příkazového řádku k zobrazení výstupu.</span><span class="sxs-lookup"><span data-stu-id="47b3b-162">The `--logpath` option must be used to specify a log file, because the running service does not have a command window to display output.</span></span>
   * <span data-ttu-id="47b3b-163">`--logappend` Možnost určuje, že restartování služby způsobí, že výstupní má být připojen ke stávajícímu souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="47b3b-163">The `--logappend` option specifies that a restart of the service causes output to append to the existing log file.</span></span>
   
   <span data-ttu-id="47b3b-164">Spusťte službu MongoDB, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="47b3b-164">To start the MongoDB service, run the following command:</span></span>
   
    ```
    net start MongoDB
    ```
   
    <span data-ttu-id="47b3b-165">Další informace o vytváření MongoDB služby najdete v tématu [konfigurace služby systému Windows pro MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="47b3b-165">For more information about creating the MongoDB service, see [Configure a Windows Service for MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span></span>

## <a name="test-the-mongodb-instance"></a><span data-ttu-id="47b3b-166">Testovací MongoDB instance</span><span class="sxs-lookup"><span data-stu-id="47b3b-166">Test the MongoDB instance</span></span>
<span data-ttu-id="47b3b-167">S MongoDB spuštěna jako jedna instance nebo nainstalovat jako službu můžete nyní spustit vytváření a používání vašich databází.</span><span class="sxs-lookup"><span data-stu-id="47b3b-167">With MongoDB running as a single instance or installed as a service, you can now start creating and using your databases.</span></span> <span data-ttu-id="47b3b-168">Pokud chcete spustit prostředí pro správu MongoDB, otevřete další okno příkazového řádku z **spustit** nabídce a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="47b3b-168">To start the MongoDB administrative shell, open another command prompt window from the **Start** menu, and enter the following command:</span></span>

```
mongo  
```

<span data-ttu-id="47b3b-169">Můžete vytvořit seznam databáze s `db` příkaz.</span><span class="sxs-lookup"><span data-stu-id="47b3b-169">You can list the databases with the `db` command.</span></span> <span data-ttu-id="47b3b-170">Vložte některá data následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="47b3b-170">Insert some data as follows:</span></span>

```
db.foo.insert( { a : 1 } )
```

<span data-ttu-id="47b3b-171">Vyhledejte data následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="47b3b-171">Search for data as follows:</span></span>

```
db.foo.find()
```

<span data-ttu-id="47b3b-172">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="47b3b-172">The output is similar to the following example:</span></span>

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

<span data-ttu-id="47b3b-173">Ukončení `mongo` konzole následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="47b3b-173">Exit the `mongo` console as follows:</span></span>

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a><span data-ttu-id="47b3b-174">Konfigurace pravidel skupin zabezpečení sítě a brány firewall</span><span class="sxs-lookup"><span data-stu-id="47b3b-174">Configure firewall and Network Security Group rules</span></span>
<span data-ttu-id="47b3b-175">Teď, když MongoDB je nainstalovaná a spuštěná, aby mohli vzdáleně připojit k MongoDB otevřete port v bráně Windows Firewall.</span><span class="sxs-lookup"><span data-stu-id="47b3b-175">Now that MongoDB is installed and running, open a port in Windows Firewall so you can remotely connect to MongoDB.</span></span> <span data-ttu-id="47b3b-176">Pokud chcete vytvořit nové příchozí pravidlo, které povolí TCP port 27017, otevřete Správce příkazovém řádku prostředí PowerShell a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="47b3b-176">To create a new inbound rule to allow TCP port 27017, open an administrative PowerShell prompt and enter the following command:</span></span>

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

<span data-ttu-id="47b3b-177">Toto pravidlo můžete vytvořit také pomocí **brány Windows Firewall s pokročilým zabezpečením** nástroj pro správu grafického rozhraní.</span><span class="sxs-lookup"><span data-stu-id="47b3b-177">You can also create the rule by using the **Windows Firewall with Advanced Security** graphical management tool.</span></span> <span data-ttu-id="47b3b-178">Vytvořte nové příchozí pravidlo, které povolí TCP port 27017.</span><span class="sxs-lookup"><span data-stu-id="47b3b-178">Create a new inbound rule to allow TCP port 27017.</span></span>

<span data-ttu-id="47b3b-179">V případě potřeby vytvořte skupinu zabezpečení sítě pravidlo povolující přístup k MongoDB z mimo existující podsíť virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="47b3b-179">If needed, create a Network Security Group rule to allow access to MongoDB from outside of the existing Azure virtual network subnet.</span></span> <span data-ttu-id="47b3b-180">Skupina zabezpečení sítě pravidla můžete vytvořit pomocí [portál Azure](nsg-quickstart-portal.md) nebo [prostředí Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="47b3b-180">You can create the Network Security Group rules by using the [Azure portal](nsg-quickstart-portal.md) or [Azure PowerShell](nsg-quickstart-powershell.md).</span></span> <span data-ttu-id="47b3b-181">Stejně jako u pravidla brány Windows Firewall povolí port TCP 27017 k rozhraní virtuální sítě virtuálního počítače MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47b3b-181">As with the Windows Firewall rules, allow TCP port 27017 to the virtual network interface of your MongoDB VM.</span></span>

> [!NOTE]
> <span data-ttu-id="47b3b-182">TCP port 27017 je výchozí port je používán MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47b3b-182">TCP port 27017 is the default port used by MongoDB.</span></span> <span data-ttu-id="47b3b-183">Tento port můžete změnit pomocí `--port` parametr při spouštění `mongod.exe` ručně nebo ze služby.</span><span class="sxs-lookup"><span data-stu-id="47b3b-183">You can change this port by using the `--port` parameter when starting `mongod.exe` manually or from a service.</span></span> <span data-ttu-id="47b3b-184">Pokud změníte port, nezapomeňte aktualizovat pravidla brány Windows Firewall a skupinu zabezpečení sítě v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="47b3b-184">If you change the port, make sure to update the Windows Firewall and Network Security Group rules in the preceding steps.</span></span>


## <a name="next-steps"></a><span data-ttu-id="47b3b-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47b3b-185">Next steps</span></span>
<span data-ttu-id="47b3b-186">V tomto kurzu jste zjistili, jak nainstalovat a nakonfigurovat MongoDB na vašem virtuálním počítači Windows.</span><span class="sxs-lookup"><span data-stu-id="47b3b-186">In this tutorial, you learned how to install and configure MongoDB on your Windows VM.</span></span> <span data-ttu-id="47b3b-187">Nyní můžete k MongoDB na vašem virtuálním počítači Windows pomocí následujících Pokročilá témata v [dokumentace k MongoDB](https://docs.mongodb.com/manual/).</span><span class="sxs-lookup"><span data-stu-id="47b3b-187">You can now access MongoDB on your Windows VM, by following the advanced topics in the [MongoDB documentation](https://docs.mongodb.com/manual/).</span></span>

