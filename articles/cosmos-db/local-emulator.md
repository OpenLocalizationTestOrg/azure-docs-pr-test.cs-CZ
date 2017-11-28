---
title: "aaaDevelop místně s hello emulátoru DB Cosmos Azure | Microsoft Docs"
description: "Pomocí hello emulátoru DB Cosmos Azure, můžete vývoj a testování aplikace místně pro uvolnění bez vytváření předplatného Azure."
services: cosmos-db
documentationcenter: 
keywords: "Emulátor Azure Cosmos DB"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: arramac
ms.openlocfilehash: fb5449489e5f71664e72d8e11e583315be371bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cosmos-db-emulator-for-local-development-and-testing"></a><span data-ttu-id="f9741-104">Použití hello emulátoru DB Cosmos Azure pro místní vývoj a testování</span><span class="sxs-lookup"><span data-stu-id="f9741-104">Use hello Azure Cosmos DB Emulator for local development and testing</span></span>

<table>
<tr>
  <td><span data-ttu-id="f9741-105"><strong>Binární soubory</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-105"><strong>Binaries</strong></span></span></td>
  <td>[<span data-ttu-id="f9741-106">Stáhněte si instalační služby MSI</span><span class="sxs-lookup"><span data-stu-id="f9741-106">Download MSI</span></span>](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-107"><strong>Docker</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-107"><strong>Docker</strong></span></span></td>
  <td>[<span data-ttu-id="f9741-108">Úložiště docker Hub</span><span class="sxs-lookup"><span data-stu-id="f9741-108">Docker Hub</span></span>](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-109"><strong>Zdroj docker</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-109"><strong>Docker source</strong></span></span></td>
  <td>[<span data-ttu-id="f9741-110">GitHub</span><span class="sxs-lookup"><span data-stu-id="f9741-110">Github</span></span>](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
<span data-ttu-id="f9741-111">Hello emulátoru DB Cosmos Azure poskytuje místní prostředí, které emuluje hello služby Azure Cosmos DB pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="f9741-111">hello Azure Cosmos DB Emulator provides a local environment that emulates hello Azure Cosmos DB service for development purposes.</span></span> <span data-ttu-id="f9741-112">Hello emulátoru DB Cosmos Azure můžete vyvíjet a testovat svou aplikaci lokálně, bez vytváření předplatného Azure nebo nákladům.</span><span class="sxs-lookup"><span data-stu-id="f9741-112">Using hello Azure Cosmos DB Emulator, you can develop and test your application locally, without creating an Azure subscription or incurring any costs.</span></span> <span data-ttu-id="f9741-113">Až budete spokojeni s jak aplikace funguje v hello emulátoru DB Cosmos Azure, můžete přepnout toousing účet Azure Cosmos DB v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-113">When you're satisfied with how your application is working in hello Azure Cosmos DB Emulator, you can switch toousing an Azure Cosmos DB account in hello cloud.</span></span>

<span data-ttu-id="f9741-114">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="f9741-114">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="f9741-115">Instalace hello emulátoru</span><span class="sxs-lookup"><span data-stu-id="f9741-115">Installing hello Emulator</span></span>
> * <span data-ttu-id="f9741-116">Systémem Docker pro Windows hello emulátoru</span><span class="sxs-lookup"><span data-stu-id="f9741-116">Running hello Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="f9741-117">Ověřování požadavků</span><span class="sxs-lookup"><span data-stu-id="f9741-117">Authenticating requests</span></span>
> * <span data-ttu-id="f9741-118">Pomocí Průzkumníku dat hello v emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="f9741-118">Using hello Data Explorer in hello Emulator</span></span>
> * <span data-ttu-id="f9741-119">Export certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="f9741-119">Exporting SSL certificates</span></span>
> * <span data-ttu-id="f9741-120">Volání hello emulátoru z příkazového řádku hello</span><span class="sxs-lookup"><span data-stu-id="f9741-120">Calling hello Emulator from hello command line</span></span>
> * <span data-ttu-id="f9741-121">Shromažďování trasovacích souborů</span><span class="sxs-lookup"><span data-stu-id="f9741-121">Collecting trace files</span></span>

<span data-ttu-id="f9741-122">Doporučujeme začít sledování hello následující video, kde Kirill Gavrylyuk ukazuje, jak tooget pracovat s hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-122">We recommend getting started by watching hello following video, where Kirill Gavrylyuk shows how tooget started with hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="f9741-123">Všimněte si, že hello video odkazuje toohello emulátoru jako hello DocumentDB emulátoru, ale byla přejmenována hello nástroj sám sebe hello emulátoru DB Cosmos Azure od zaznamenávat hello video.</span><span class="sxs-lookup"><span data-stu-id="f9741-123">Note that hello video refers toohello emulator as hello DocumentDB Emulator, but hello tool itself has been renamed hello Azure Cosmos DB Emulator since taping hello video.</span></span> <span data-ttu-id="f9741-124">Všechny informace v hello video je stále správné pro hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-124">All information in hello video is still accurate for hello Azure Cosmos DB Emulator.</span></span> 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-hello-emulator-works"></a><span data-ttu-id="f9741-125">Jak funguje hello emulátoru</span><span class="sxs-lookup"><span data-stu-id="f9741-125">How hello Emulator works</span></span>
<span data-ttu-id="f9741-126">Hello emulátoru DB Cosmos Azure poskytuje zachováním emulace Dobrý den služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f9741-126">hello Azure Cosmos DB Emulator provides a high-fidelity emulation of hello Azure Cosmos DB service.</span></span> <span data-ttu-id="f9741-127">Podporuje stejné funkce jako Azure Cosmos databáze, včetně podpory pro vytváření a dotazování dokumentů JSON, zřizování a škálování kolekce a provádění uložené procedury a triggery.</span><span class="sxs-lookup"><span data-stu-id="f9741-127">It supports identical functionality as Azure Cosmos DB, including support for creating and querying JSON documents, provisioning and scaling collections, and executing stored procedures and triggers.</span></span> <span data-ttu-id="f9741-128">Můžete vyvíjet a testovat aplikace pomocí hello emulátoru DB Cosmos Azure a nasadit je tooAzure v globálním měřítku tím, že právě jednu konfiguraci změnit toohello koncového bodu připojení pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f9741-128">You can develop and test applications using hello Azure Cosmos DB Emulator, and deploy them tooAzure at global scale by just making a single configuration change toohello connection endpoint for Azure Cosmos DB.</span></span>

<span data-ttu-id="f9741-129">Když jsme vytvořili místní emulace zachováním hello skutečné Azure Cosmos DB služby, se liší od služby hello hello implementace hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-129">While we created a high-fidelity local emulation of hello actual Azure Cosmos DB service, hello implementation of hello Azure Cosmos DB Emulator is different than that of hello service.</span></span> <span data-ttu-id="f9741-130">Například hello emulátoru DB Cosmos Azure používá standardní součásti operačního systému, například hello místního systému souborů pro trvalosti a zásobník protokolu HTTPS pro připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="f9741-130">For example, hello Azure Cosmos DB Emulator uses standard OS components such as hello local file system for persistence, and HTTPS protocol stack for connectivity.</span></span> <span data-ttu-id="f9741-131">To znamená, že některé funkce, které jsou závislé na infrastrukturu Azure jako globální replikace, jednociferné milisekundu latence pro čtení/zápisu a přizpůsobitelné úrovně konzistence nejsou k dispozici prostřednictvím hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-131">This means that some functionality that relies on Azure infrastructure like global replication, single-digit millisecond latency for reads/writes, and tunable consistency levels are not available via hello Azure Cosmos DB Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="f9741-132">V tento čas hello Průzkumníku dat v hello emulátoru podporuje pouze hello vytvoření kolekce DocumentDB rozhraní API a kolekcí MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f9741-132">At this time hello Data Explorer in hello emulator only supports hello creation of DocumentDB API collections and MongoDB collections.</span></span> <span data-ttu-id="f9741-133">Hello Průzkumníku dat v emulátoru hello v současné době nepodporuje vytvoření hello tabulky a grafy.</span><span class="sxs-lookup"><span data-stu-id="f9741-133">hello Data Explorer in hello emulator does not currently support hello creation of tables and graphs.</span></span> 

## <a name="system-requirements"></a><span data-ttu-id="f9741-134">Požadavky na systém</span><span class="sxs-lookup"><span data-stu-id="f9741-134">System requirements</span></span>
<span data-ttu-id="f9741-135">Hello emulátoru DB Cosmos Azure má hello následující hardwarové a softwarové požadavky:</span><span class="sxs-lookup"><span data-stu-id="f9741-135">hello Azure Cosmos DB Emulator has hello following hardware and software requirements:</span></span>

* <span data-ttu-id="f9741-136">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="f9741-136">Software requirements</span></span>
  * <span data-ttu-id="f9741-137">Windows Server 2012 R2, Windows Server 2016 nebo Windows 10</span><span class="sxs-lookup"><span data-stu-id="f9741-137">Windows Server 2012 R2, Windows Server 2016, or Windows 10</span></span>
*   <span data-ttu-id="f9741-138">Minimální požadavky na Hardware</span><span class="sxs-lookup"><span data-stu-id="f9741-138">Minimum Hardware requirements</span></span>
  * <span data-ttu-id="f9741-139">2 GB PAMĚTI RAM</span><span class="sxs-lookup"><span data-stu-id="f9741-139">2 GB RAM</span></span>
  * <span data-ttu-id="f9741-140">10 GB volného místa na disku</span><span class="sxs-lookup"><span data-stu-id="f9741-140">10 GB available hard disk space</span></span>

## <a name="installation"></a><span data-ttu-id="f9741-141">Instalace</span><span class="sxs-lookup"><span data-stu-id="f9741-141">Installation</span></span>
<span data-ttu-id="f9741-142">Můžete stáhnout a nainstalovat hello emulátoru DB Cosmos Azure z hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="f9741-142">You can download and install hello Azure Cosmos DB Emulator from hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span></span> 

> [!NOTE]
> <span data-ttu-id="f9741-143">tooinstall, konfiguraci a spuštění hello Azure Cosmos DB emulátoru, musíte mít oprávnění správce na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-143">tooinstall, configure, and run hello Azure Cosmos DB Emulator, you must have administrative privileges on hello computer.</span></span>

## <a name="running-on-docker-for-windows"></a><span data-ttu-id="f9741-144">Systémem Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="f9741-144">Running on Docker for Windows</span></span>

<span data-ttu-id="f9741-145">Hello emulátoru DB Cosmos Azure můžete spustit na Docker pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="f9741-145">hello Azure Cosmos DB Emulator can be run on Docker for Windows.</span></span> <span data-ttu-id="f9741-146">Hello emulátoru na Docker pro Oracle Linux nefunguje.</span><span class="sxs-lookup"><span data-stu-id="f9741-146">hello Emulator does not work on Docker for Oracle Linux.</span></span>

<span data-ttu-id="f9741-147">Jakmile máte [Docker pro systém Windows](https://www.docker.com/docker-windows) nainstalován, můžete můžete načítat bitová kopie emulátoru hello z úložiště Docker Hub tak, že spustíte následující příkaz z své oblíbené prostředí hello (cmd.exe, prostředí PowerShell, atd.).</span><span class="sxs-lookup"><span data-stu-id="f9741-147">Once you have [Docker for Windows](https://www.docker.com/docker-windows) installed, you can pull hello Emulator image from Docker Hub by running hello following command from your favorite shell (cmd.exe, PowerShell, etc.).</span></span>

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
<span data-ttu-id="f9741-148">toostart hello bitovou kopii, spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-148">toostart hello image, run hello following commands.</span></span>

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

<span data-ttu-id="f9741-149">Hello odpověď bude vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="f9741-149">hello response looks similar toohello following:</span></span>

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import hello SSL certificate from an administrator command prompt on hello host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

<span data-ttu-id="f9741-150">Zavřením hello interaktivní prostředí po hello spuštění emulátoru bude vypnutí hello emulátoru kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="f9741-150">Closing hello interactive shell once hello Emulator has been started will shutdown hello Emulator’s container.</span></span>

<span data-ttu-id="f9741-151">Použijte hello koncový bod a hlavního klíče v odpovědi hello v vašeho klienta a importovat certifikát SSL hello do svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="f9741-151">Use hello endpoint and master key in from hello response in your client and import hello SSL certificate into your host.</span></span> <span data-ttu-id="f9741-152">tooimport hello certifikát SSL hello následující z příkazového řádku správce:</span><span class="sxs-lookup"><span data-stu-id="f9741-152">tooimport hello SSL certificate, do hello following from an admin command prompt:</span></span>

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-hello-emulator"></a><span data-ttu-id="f9741-153">Spusťte emulátor hello</span><span class="sxs-lookup"><span data-stu-id="f9741-153">Start hello Emulator</span></span>

<span data-ttu-id="f9741-154">toostart hello emulátoru DB Cosmos Azure, vyberte tlačítko Start hello nebo stiskněte klávesu Windows hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-154">toostart hello Azure Cosmos DB Emulator, select hello Start button or press hello Windows key.</span></span> <span data-ttu-id="f9741-155">Začněte psát **emulátoru DB Cosmos Azure**a vyberte hello emulátoru hello seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="f9741-155">Begin typing **Azure Cosmos DB Emulator**, and select hello emulator from hello list of applications.</span></span> 

![Vyberte hello Start tlačítka nebo klikněte na tlačítko hello Windows klíč, začněte psát ** Azure Cosmos DB emulátoru ** a vyberte hello emulátor ze hello seznam aplikací](./media/local-emulator/database-local-emulator-start.png)

<span data-ttu-id="f9741-157">Když je spuštěný emulátor hello, uvidíte na ikonu v oznamovací oblasti hlavního panelu Windows hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-157">When hello emulator is running, you'll see an icon in hello Windows taskbar notification area.</span></span> ![Azure Cosmos DB místní emulátoru panelu oznámení](./media/local-emulator/database-local-emulator-taskbar.png)

<span data-ttu-id="f9741-159">Hello emulátoru DB Cosmos Azure ve výchozím nastavení spouští v hello místním počítači ("localhost") naslouchá na portu 8081.</span><span class="sxs-lookup"><span data-stu-id="f9741-159">hello Azure Cosmos DB Emulator by default runs on hello local machine ("localhost") listening on port 8081.</span></span>

<span data-ttu-id="f9741-160">Hello emulátoru DB Cosmos Azure je nainstalována ve výchozím nastavení toohello `C:\Program Files\Azure Cosmos DB Emulator` adresáře.</span><span class="sxs-lookup"><span data-stu-id="f9741-160">hello Azure Cosmos DB Emulator is installed by default toohello `C:\Program Files\Azure Cosmos DB Emulator` directory.</span></span> <span data-ttu-id="f9741-161">Můžete také spustit a zastavit hello emulátor ze hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f9741-161">You can also start and stop hello emulator from hello command-line.</span></span> <span data-ttu-id="f9741-162">V tématu [odkaz na nástroj příkazového řádku](#command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f9741-162">See [command-line tool reference](#command-line) for more information.</span></span>

## <a name="start-data-explorer"></a><span data-ttu-id="f9741-163">Spuštění Průzkumníka dat</span><span class="sxs-lookup"><span data-stu-id="f9741-163">Start Data Explorer</span></span>

<span data-ttu-id="f9741-164">Při spuštění hello Azure Cosmos DB emulátoru se automaticky otevře hello Průzkumníku dat DB Cosmos Azure v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f9741-164">When hello Azure Cosmos DB emulator launches it will automatically open hello Azure Cosmos DB Data Explorer in your browser.</span></span> <span data-ttu-id="f9741-165">Hello adresa se zobrazí jako [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span><span class="sxs-lookup"><span data-stu-id="f9741-165">hello address will appear as [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span></span> <span data-ttu-id="f9741-166">Pokud jste zavřete hello Průzkumníka a chcete toore otevřete ho později, můžete otevřít hello adresu URL v prohlížeči nebo spustit z hello emulátoru DB Cosmos Azure v hello ikonu na hlavním panelu systému Windows, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f9741-166">If you close hello explorer and would like toore-open it later, you can either open hello URL in your browser or launch it from hello Azure Cosmos DB Emulator in hello Windows Tray Icon as shown below.</span></span>

![Azure Cosmos DB místní emulátoru data Průzkumníka Spouštěče](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a><span data-ttu-id="f9741-168">Kontrola aktualizací</span><span class="sxs-lookup"><span data-stu-id="f9741-168">Checking for updates</span></span>
<span data-ttu-id="f9741-169">Průzkumník dat určuje, zda je k dispozici ke stažení nové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f9741-169">Data Explorer indicates if there is a new update available for download.</span></span> 

> [!NOTE]
> <span data-ttu-id="f9741-170">Data vytvořená ve verzi hello emulátoru DB Azure Cosmos není zaručena toobe dostupné při použití jiné verze.</span><span class="sxs-lookup"><span data-stu-id="f9741-170">Data created in one version of hello Azure Cosmos DB Emulator is not guaranteed toobe accessible when using a different version.</span></span> <span data-ttu-id="f9741-171">Pokud budete potřebovat toopersist data pro dlouhodobé hello, doporučuje se uložit data v Azure Cosmos DB účet, nikoli hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-171">If you need toopersist your data for hello long term, it is recommended that you store that data in an Azure Cosmos DB account, rather than in hello Azure Cosmos DB Emulator.</span></span> 

## <a name="authenticating-requests"></a><span data-ttu-id="f9741-172">Ověřování požadavků</span><span class="sxs-lookup"><span data-stu-id="f9741-172">Authenticating requests</span></span>
<span data-ttu-id="f9741-173">Stejně jako s Azure DB Cosmos v cloudu hello každého požadavku, které vytváříte, proti hello emulátoru DB Cosmos Azure musí ověřit.</span><span class="sxs-lookup"><span data-stu-id="f9741-173">Just as with Azure Cosmos DB in hello cloud, every request that you make against hello Azure Cosmos DB Emulator must be authenticated.</span></span> <span data-ttu-id="f9741-174">Hello Azure Cosmos DB emulátoru podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověřování hlavního klíče.</span><span class="sxs-lookup"><span data-stu-id="f9741-174">hello Azure Cosmos DB Emulator supports a single fixed account and a well-known authentication key for master key authentication.</span></span> <span data-ttu-id="f9741-175">Tento účet a klíč jsou pouze povolené pro použití s hello emulátoru DB Cosmos Azure přihlašovací údaje v hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-175">This account and key are hello only credentials permitted for use with hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="f9741-176">Jsou:</span><span class="sxs-lookup"><span data-stu-id="f9741-176">They are:</span></span>

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> <span data-ttu-id="f9741-177">hlavní klíč Hello nepodporuje hello emulátoru DB Cosmos Azure je určena pro použití pouze pomocí emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-177">hello master key supported by hello Azure Cosmos DB Emulator is intended for use only with hello emulator.</span></span> <span data-ttu-id="f9741-178">Váš účet Azure Cosmos DB produkční a klíč nelze použít s hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-178">You cannot use your production Azure Cosmos DB account and key with hello Azure Cosmos DB Emulator.</span></span> 

> [!NOTE] 
> <span data-ttu-id="f9741-179">Pokud jste spustili hello emulátoru s hello /Key možnost, pak pomocí klíče generované hello místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="</span><span class="sxs-lookup"><span data-stu-id="f9741-179">If you have started hello emulator with hello /Key option, then use hello generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="f9741-180">Navíc, podobně jako hello služby Azure Cosmos DB, hello Azure Cosmos DB emulátoru podporuje pouze zabezpečenou komunikaci prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="f9741-180">Additionally, just as hello Azure Cosmos DB service, hello Azure Cosmos DB Emulator supports only secure communication via SSL.</span></span>

## <a name="running-hello-emulator-on-a-local-network"></a><span data-ttu-id="f9741-181">Spuštěného emulátoru hello v místní síti</span><span class="sxs-lookup"><span data-stu-id="f9741-181">Running hello emulator on a local network</span></span>

<span data-ttu-id="f9741-182">Emulátor hello můžete spustit v místní síti.</span><span class="sxs-lookup"><span data-stu-id="f9741-182">You can run hello emulator on a local network.</span></span> <span data-ttu-id="f9741-183">tooenable přístup k síti, zadejte možnost /AllowNetworkAccess hello v hello [příkazového řádku](#command-line-syntax), což také vyžaduje, že zadáváte /Key = key_string nebo/keyfile = název_souboru.</span><span class="sxs-lookup"><span data-stu-id="f9741-183">tooenable network access, specify hello /AllowNetworkAccess option at hello [command line](#command-line-syntax), which also requires that you specify /Key=key_string or /KeyFile=file_name.</span></span> <span data-ttu-id="f9741-184">Můžete použít /GenKeyFile = název_souboru toogenerate soubor s předem náhodný klíč.</span><span class="sxs-lookup"><span data-stu-id="f9741-184">You can use /GenKeyFile=file_name toogenerate a file with a random key upfront.</span></span>  <span data-ttu-id="f9741-185">Pak můžete předat tuto příliš/KeyFile = název_souboru nebo /Key = contents_of_file.</span><span class="sxs-lookup"><span data-stu-id="f9741-185">Then you can pass that too/KeyFile=file_name or /Key=contents_of_file.</span></span>

<span data-ttu-id="f9741-186">přístup k síti tooenable pro hello hello nový uživatel by měl emulátoru hello vypnutí a odstranit adresář dat hello emulátoru (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span><span class="sxs-lookup"><span data-stu-id="f9741-186">tooenable network access for hello first time hello user should shutdown hello emulator and delete hello emulator’s data directory (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span></span>

## <a name="developing-with-hello-emulator"></a><span data-ttu-id="f9741-187">Vývoj s hello emulátoru</span><span class="sxs-lookup"><span data-stu-id="f9741-187">Developing with hello Emulator</span></span>
<span data-ttu-id="f9741-188">Jakmile máte hello Azure Cosmos DB emulátoru spuštěna na pracovní ploše, můžete použít libovolnou podporované [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) nebo hello [REST API služby Azure Cosmos DB](/rest/api/documentdb/) toointeract s hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="f9741-188">Once you have hello Azure Cosmos DB Emulator running on your desktop, you can use any supported [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) or hello [Azure Cosmos DB REST API](/rest/api/documentdb/) toointeract with hello Emulator.</span></span> <span data-ttu-id="f9741-189">Hello emulátoru DB Cosmos Azure také zahrnuje integrovanou Průzkumníku dat, která umožňuje vytvářet kolekce pro hello DocumentDB a rozhraní API MongoDB a zobrazení a úpravám dokumentů bez psaní jakéhokoli kódu.</span><span class="sxs-lookup"><span data-stu-id="f9741-189">hello Azure Cosmos DB Emulator also includes a built-in Data Explorer that lets you create collections for hello DocumentDB and MongoDB APIs, and view and edit documents without writing any code.</span></span>   

    // Connect toohello Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

<span data-ttu-id="f9741-190">Pokud používáte [Azure Cosmos DB podporou protokolů pro MongoDB](mongodb-introduction.md), použijte prosím hello následující připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="f9741-190">If you're using [Azure Cosmos DB protocol support for MongoDB](mongodb-introduction.md), please use hello following connection string:</span></span>

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

<span data-ttu-id="f9741-191">Můžete použít stávající nástroje, například [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-191">You can use existing tools like [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="f9741-192">Můžete také migrovat data mezi hello emulátoru DB Cosmos Azure a službou Azure Cosmos DB hello pomocí hello [nástroj pro migraci dat Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).</span><span class="sxs-lookup"><span data-stu-id="f9741-192">You can also migrate data between hello Azure Cosmos DB Emulator and hello Azure Cosmos DB service using hello [Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).</span></span>

> [!NOTE] 
> <span data-ttu-id="f9741-193">Pokud jste spustili hello emulátoru s hello /Key možnost, pak pomocí klíče generované hello místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="</span><span class="sxs-lookup"><span data-stu-id="f9741-193">If you have started hello emulator with hello /Key option, then use hello generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="f9741-194">Pomocí emulátoru hello Azure Cosmos DB, ve výchozím nastavení, můžete vytvořit kolekce tvořené jedním oddílem too25 nebo 1 dělenou kolekci.</span><span class="sxs-lookup"><span data-stu-id="f9741-194">Using hello Azure Cosmos DB emulator, by default, you can create up too25 single partition collections or 1 partitioned collection.</span></span> <span data-ttu-id="f9741-195">Další informace o změně tuto hodnotu najdete v tématu [nastavení hodnoty PartitionCount hello](#set-partitioncount).</span><span class="sxs-lookup"><span data-stu-id="f9741-195">For more information about changing this value, see [Setting hello PartitionCount value](#set-partitioncount).</span></span>

## <a name="export-hello-ssl-certificate"></a><span data-ttu-id="f9741-196">Exportovat certifikát SSL hello</span><span class="sxs-lookup"><span data-stu-id="f9741-196">Export hello SSL certificate</span></span>

<span data-ttu-id="f9741-197">Jazyky rozhraní .NET a toosecurely úložiště certifikátů Windows hello použití modulu runtime připojit místní emulátoru toohello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f9741-197">.NET languages and runtime use hello Windows Certificate Store toosecurely connect toohello Azure Cosmos DB local emulator.</span></span> <span data-ttu-id="f9741-198">Další jazyky mít vlastní metodu správy a použití certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f9741-198">Other languages have their own method of managing and using certificates.</span></span> <span data-ttu-id="f9741-199">Java používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) zatímco používá Python [soketu obálky](https://docs.python.org/2/library/ssl.html).</span><span class="sxs-lookup"><span data-stu-id="f9741-199">Java uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) whereas Python uses [socket wrappers](https://docs.python.org/2/library/ssl.html).</span></span>

<span data-ttu-id="f9741-200">V pořadí tooobtain toouse certifikát s jazyky a moduly runtime, který nelze integrovat do úložiště certifikátů Windows hello je budou potřebovat tooexport pomocí Správce certifikátů Windows hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-200">In order tooobtain a certificate toouse with languages and runtimes that do not integrate with hello Windows Certificate Store you will need tooexport it using hello Windows Certificate Manager.</span></span> <span data-ttu-id="f9741-201">Můžete spusťte ji spuštěním certlm.msc nebo postupujte podle pokynů hello krok za krokem v [exportu hello Azure Cosmos DB emulátoru certifikátů](./local-emulator-export-ssl-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="f9741-201">You can start it by running certlm.msc or follow hello step by step instructions in [Export hello Azure Cosmos DB Emulator Certificates](./local-emulator-export-ssl-certificates.md).</span></span> <span data-ttu-id="f9741-202">Jakmile správce certifikátů hello pracuje, otevřete hello osobní certifikáty, jak je uvedeno níže a export hello certifikát s popisným názvem hello "DocumentDBEmulatorCertificate" jako kódováním BASE-64 souboru X.509 (.cer).</span><span class="sxs-lookup"><span data-stu-id="f9741-202">Once hello certificate manager is running, open hello Personal Certificates as shown below and export hello certificate with hello friendly name "DocumentDBEmulatorCertificate" as a BASE-64 encoded X.509 (.cer) file.</span></span>

![Certifikát SSL místní emulátoru služby Azure Cosmos DB](./media/local-emulator/database-local-emulator-ssl_certificate.png)

<span data-ttu-id="f9741-204">certifikát X.509 Hello lze importovat do úložiště certifikátů Java hello podle následujících pokynů hello v [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span><span class="sxs-lookup"><span data-stu-id="f9741-204">hello X.509 certificate can be imported into hello Java certificate store by following hello instructions in [Adding a Certificate toohello Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span></span> <span data-ttu-id="f9741-205">Jakmile je hello certifikát importován do úložiště certifikátů hello, aplikací Java a MongoDB bude mít tooconnect toohello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-205">Once hello certificate is imported into hello certificate store, Java and MongoDB applications will be able tooconnect toohello Azure Cosmos DB Emulator.</span></span>

<span data-ttu-id="f9741-206">Při připojování toohello emulátoru z Pythonu a Node.js SDK, je zakázáno ověřování SSL.</span><span class="sxs-lookup"><span data-stu-id="f9741-206">When connecting toohello emulator from Python and Node.js SDKs, SSL verification is disabled.</span></span>

## <span data-ttu-id="f9741-207"><a id="command-line"></a>Odkaz na nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f9741-207"><a id="command-line"></a>Command-line tool reference</span></span>
<span data-ttu-id="f9741-208">Z umístění instalace hello můžete pomocí příkazového řádku toostart hello a zastavit hello emulátoru, nakonfigurujte možnosti a provádění dalších operací.</span><span class="sxs-lookup"><span data-stu-id="f9741-208">From hello installation location, you can use hello command-line toostart and stop hello emulator, configure options, and perform other operations.</span></span>

### <a name="command-line-syntax"></a><span data-ttu-id="f9741-209">Syntaxe příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f9741-209">Command-line Syntax</span></span>

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

<span data-ttu-id="f9741-210">tooview hello seznam možností, typ `CosmosDB.Emulator.exe /?` hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f9741-210">tooview hello list of options, type `CosmosDB.Emulator.exe /?` at hello command prompt.</span></span>

<table>
<tr>
  <td><span data-ttu-id="f9741-211"><strong>Možnost</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-211"><strong>Option</strong></span></span></td>
  <td><span data-ttu-id="f9741-212"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-212"><strong>Description</strong></span></span></td>
  <td><span data-ttu-id="f9741-213"><strong>Příkaz</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-213"><strong>Command</strong></span></span></td>
  <td><span data-ttu-id="f9741-214"><strong>Argumenty</strong></span><span class="sxs-lookup"><span data-stu-id="f9741-214"><strong>Arguments</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-215">[Žádný argument]</span><span class="sxs-lookup"><span data-stu-id="f9741-215">[No arguments]</span></span></td>
  <td><span data-ttu-id="f9741-216">Spuštění hello emulátoru DB Cosmos Azure s výchozím nastavením.</span><span class="sxs-lookup"><span data-stu-id="f9741-216">Starts up hello Azure Cosmos DB Emulator with default settings.</span></span></td>
  <td><span data-ttu-id="f9741-217">CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="f9741-217">CosmosDB.Emulator.exe</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-218">[Help]</span><span class="sxs-lookup"><span data-stu-id="f9741-218">[Help]</span></span></td>
  <td><span data-ttu-id="f9741-219">Zobrazí seznam hello podporované argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f9741-219">Displays hello list of supported command-line arguments.</span></span></td>
  <td><span data-ttu-id="f9741-220">CosmosDB.Emulator.exe /?</span><span class="sxs-lookup"><span data-stu-id="f9741-220">CosmosDB.Emulator.exe /?</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-221">Vypnutí</span><span class="sxs-lookup"><span data-stu-id="f9741-221">Shutdown</span></span></td>
  <td><span data-ttu-id="f9741-222">Vypne hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-222">Shuts down hello Azure Cosmos DB Emulator.</span></span></td>
  <td><span data-ttu-id="f9741-223">/ CosmosDB.Emulator.exe Shutdown</span><span class="sxs-lookup"><span data-stu-id="f9741-223">CosmosDB.Emulator.exe /Shutdown</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-224">DataPath</span><span class="sxs-lookup"><span data-stu-id="f9741-224">DataPath</span></span></td>
  <td><span data-ttu-id="f9741-225">Určuje cestu hello v které toostore datové soubory.</span><span class="sxs-lookup"><span data-stu-id="f9741-225">Specifies hello path in which toostore data files.</span></span> <span data-ttu-id="f9741-226">Výchozí hodnota je % LocalAppdata%\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="f9741-226">Default is %LocalAppdata%\CosmosDBEmulator.</span></span></td>
  <td><span data-ttu-id="f9741-227">CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-227">CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</span></span></td>
  <td><span data-ttu-id="f9741-228">&lt;DataPath&gt;: přístupná cesta</span><span class="sxs-lookup"><span data-stu-id="f9741-228">&lt;datapath&gt;: An accessible path</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-229">Port</span><span class="sxs-lookup"><span data-stu-id="f9741-229">Port</span></span></td>
  <td><span data-ttu-id="f9741-230">Určuje číslo toouse hello port pro emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-230">Specifies hello port number toouse for hello emulator.</span></span>  <span data-ttu-id="f9741-231">Výchozí hodnota je 8081.</span><span class="sxs-lookup"><span data-stu-id="f9741-231">Default is 8081.</span></span></td>
  <td><span data-ttu-id="f9741-232">/ CosmosDB.Emulator.exe port =&lt;portu&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-232">CosmosDB.Emulator.exe /Port=&lt;port&gt;</span></span></td>
  <td><span data-ttu-id="f9741-233">&lt;port&gt;: jedno číslo portu</span><span class="sxs-lookup"><span data-stu-id="f9741-233">&lt;port&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-234">MongoPort</span><span class="sxs-lookup"><span data-stu-id="f9741-234">MongoPort</span></span></td>
  <td><span data-ttu-id="f9741-235">Určuje hello toouse číslo portu pro MongoDB kompatibility rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f9741-235">Specifies hello port number toouse for MongoDB compatibility API.</span></span> <span data-ttu-id="f9741-236">Výchozí hodnota je 10255.</span><span class="sxs-lookup"><span data-stu-id="f9741-236">Default is 10255.</span></span></td>
  <td><span data-ttu-id="f9741-237">CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-237">CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</span></span></td>
  <td><span data-ttu-id="f9741-238">&lt;mongoport&gt;: jedno číslo portu</span><span class="sxs-lookup"><span data-stu-id="f9741-238">&lt;mongoport&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-239">DirectPorts</span><span class="sxs-lookup"><span data-stu-id="f9741-239">DirectPorts</span></span></td>
  <td><span data-ttu-id="f9741-240">Určuje hello toouse porty pro přímé připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="f9741-240">Specifies hello ports toouse for direct connectivity.</span></span> <span data-ttu-id="f9741-241">Výchozí hodnoty jsou 10251,10252,10253,10254.</span><span class="sxs-lookup"><span data-stu-id="f9741-241">Defaults are 10251,10252,10253,10254.</span></span></td>
  <td><span data-ttu-id="f9741-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span></span></td>
  <td><span data-ttu-id="f9741-243">&lt;directports&gt;: seznam s položkami oddělenými čárkou 4 porty</span><span class="sxs-lookup"><span data-stu-id="f9741-243">&lt;directports&gt;: Comma-delimited list of 4 ports</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-244">Klíč</span><span class="sxs-lookup"><span data-stu-id="f9741-244">Key</span></span></td>
  <td><span data-ttu-id="f9741-245">Autorizační klíč pro emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-245">Authorization key for hello emulator.</span></span> <span data-ttu-id="f9741-246">Klíč musí být hello kódování base-64 vektoru 64 bajtů.</span><span class="sxs-lookup"><span data-stu-id="f9741-246">Key must be hello base-64 encoding of a 64-byte vector.</span></span></td>
  <td><span data-ttu-id="f9741-247">CosmosDB.Emulator.exe /Key:&lt;klíč&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-247">CosmosDB.Emulator.exe /Key:&lt;key&gt;</span></span></td>
  <td><span data-ttu-id="f9741-248">&lt;klíč&gt;: klíč musí být hello kódování base-64 vektoru 64 bajtů</span><span class="sxs-lookup"><span data-stu-id="f9741-248">&lt;key&gt;: Key must be hello base-64 encoding of a 64-byte vector</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-249">EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="f9741-249">EnableRateLimiting</span></span></td>
  <td><span data-ttu-id="f9741-250">Určuje, že rychlost požadavků, omezení chování je povolena.</span><span class="sxs-lookup"><span data-stu-id="f9741-250">Specifies that request rate limiting behavior is enabled.</span></span></td>
  <td><span data-ttu-id="f9741-251">CosmosDB.Emulator.exe /EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="f9741-251">CosmosDB.Emulator.exe /EnableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-252">DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="f9741-252">DisableRateLimiting</span></span></td>
  <td><span data-ttu-id="f9741-253">Určuje, že rychlost požadavků, omezení chování je zakázán.</span><span class="sxs-lookup"><span data-stu-id="f9741-253">Specifies that request rate limiting behavior is disabled.</span></span></td>
  <td><span data-ttu-id="f9741-254">CosmosDB.Emulator.exe /DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="f9741-254">CosmosDB.Emulator.exe /DisableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-255">NoUI</span><span class="sxs-lookup"><span data-stu-id="f9741-255">NoUI</span></span></td>
  <td><span data-ttu-id="f9741-256">Nezobrazovat hello emulátoru uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f9741-256">Do not show hello emulator user interface.</span></span></td>
  <td><span data-ttu-id="f9741-257">/ Noui CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="f9741-257">CosmosDB.Emulator.exe /NoUI</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-258">NoExplorer</span><span class="sxs-lookup"><span data-stu-id="f9741-258">NoExplorer</span></span></td>
  <td><span data-ttu-id="f9741-259">Při spuštění nezobrazovat Průzkumníka dokumentů.</span><span class="sxs-lookup"><span data-stu-id="f9741-259">Don't show document explorer on startup.</span></span></td>
  <td><span data-ttu-id="f9741-260">CosmosDB.Emulator.exe /NoExplorer</span><span class="sxs-lookup"><span data-stu-id="f9741-260">CosmosDB.Emulator.exe /NoExplorer</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-261">PartitionCount</span><span class="sxs-lookup"><span data-stu-id="f9741-261">PartitionCount</span></span></td>
  <td><span data-ttu-id="f9741-262">Určuje maximální počet dělené kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-262">Specifies hello maximum number of partitioned collections.</span></span> <span data-ttu-id="f9741-263">V tématu [změnit hello počet kolekcí](#set-partitioncount) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f9741-263">See [Change hello number of collections](#set-partitioncount) for more information.</span></span></td>
  <td><span data-ttu-id="f9741-264">CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-264">CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</span></span></td>
  <td><span data-ttu-id="f9741-265">&lt;partitioncount&gt;: maximální počet povolených kolekce tvořené jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="f9741-265">&lt;partitioncount&gt;: Maximum number of allowed single partition collections.</span></span> <span data-ttu-id="f9741-266">Výchozí hodnota je 25.</span><span class="sxs-lookup"><span data-stu-id="f9741-266">Default is 25.</span></span> <span data-ttu-id="f9741-267">Maximální povolený počet je 250.</span><span class="sxs-lookup"><span data-stu-id="f9741-267">Maximum allowed is 250.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-268">DefaultPartitionCount</span><span class="sxs-lookup"><span data-stu-id="f9741-268">DefaultPartitionCount</span></span></td>
  <td><span data-ttu-id="f9741-269">Určuje hello výchozí počet oddílů pro dělenou kolekci.</span><span class="sxs-lookup"><span data-stu-id="f9741-269">Specifies hello default number of partitions for a partitioned collection.</span></span></td>
  <td><span data-ttu-id="f9741-270">CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-270">CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</span></span></td>
  <td><span data-ttu-id="f9741-271">&lt;defaultpartitioncount&gt; výchozí hodnota je 25.</span><span class="sxs-lookup"><span data-stu-id="f9741-271">&lt;defaultpartitioncount&gt; Default is 25.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-272">AllowNetworkAccess</span><span class="sxs-lookup"><span data-stu-id="f9741-272">AllowNetworkAccess</span></span></td>
  <td><span data-ttu-id="f9741-273">Umožňuje přístup k emulátoru toohello přes síť.</span><span class="sxs-lookup"><span data-stu-id="f9741-273">Enables access toohello emulator over a network.</span></span> <span data-ttu-id="f9741-274">Je třeba předat také /Key =&lt;key_string&gt; nebo/keyfile =&lt;název_souboru&gt; tooenable přístup k síti.</span><span class="sxs-lookup"><span data-stu-id="f9741-274">You must also pass /Key=&lt;key_string&gt; or /KeyFile=&lt;file_name&gt; tooenable network access.</span></span></td>
  <td><span data-ttu-id="f9741-275">CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-275">CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;</span></span><br><br><span data-ttu-id="f9741-276">nebo</span><span class="sxs-lookup"><span data-stu-id="f9741-276">or</span></span><br><br><span data-ttu-id="f9741-277">/ Keyfile /AllowNetworkAccess CosmosDB.Emulator.exe =&lt;název_souboru&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-277">CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-278">NoFirewall</span><span class="sxs-lookup"><span data-stu-id="f9741-278">NoFirewall</span></span></td>
  <td><span data-ttu-id="f9741-279">Nemáte úprava pravidla brány firewall, když se používá /AllowNetworkAccess.</span><span class="sxs-lookup"><span data-stu-id="f9741-279">Don't adjust firewall rules when /AllowNetworkAccess is used.</span></span></td>
  <td><span data-ttu-id="f9741-280">CosmosDB.Emulator.exe /NoFirewall</span><span class="sxs-lookup"><span data-stu-id="f9741-280">CosmosDB.Emulator.exe /NoFirewall</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-281">GenKeyFile</span><span class="sxs-lookup"><span data-stu-id="f9741-281">GenKeyFile</span></span></td>
  <td><span data-ttu-id="f9741-282">Vygenerovat nový klíč autorizace a uložte toohello zadaný soubor.</span><span class="sxs-lookup"><span data-stu-id="f9741-282">Generate a new authorization key and save toohello specified file.</span></span> <span data-ttu-id="f9741-283">Hello vygenerovat klíč můžete používat s hello /Key nebo možnosti/keyfile.</span><span class="sxs-lookup"><span data-stu-id="f9741-283">hello generated key can be used with hello /Key or /KeyFile options.</span></span></td>
  <td><span data-ttu-id="f9741-284">CosmosDB.Emulator.exe /GenKeyFile =&lt;tookey cestě k souboru&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-284">CosmosDB.Emulator.exe  /GenKeyFile=&lt;path tookey file&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-285">Konzistence</span><span class="sxs-lookup"><span data-stu-id="f9741-285">Consistency</span></span></td>
  <td><span data-ttu-id="f9741-286">Nastaví úroveň konzistence výchozí hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="f9741-286">Set hello default consistency level for hello account.</span></span></td>
  <td><span data-ttu-id="f9741-287">CosmosDB.Emulator.exe /Consistency =&lt;konzistence&gt;</span><span class="sxs-lookup"><span data-stu-id="f9741-287">CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</span></span></td>
  <td><span data-ttu-id="f9741-288">&lt;konzistence&gt;: hodnota musí být jedna z následujících hello [úrovně konzistence](consistency-levels.md): relace silného, Eventual nebo BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="f9741-288">&lt;consistency&gt;: Value must be one of hello following [consistency levels](consistency-levels.md): Session, Strong, Eventual, or BoundedStaleness.</span></span>  <span data-ttu-id="f9741-289">Hello výchozí hodnota je relace.</span><span class="sxs-lookup"><span data-stu-id="f9741-289">hello default value is Session.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f9741-290">?</span><span class="sxs-lookup"><span data-stu-id="f9741-290">?</span></span></td>
  <td><span data-ttu-id="f9741-291">Zobrazit zprávu nápovědy hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-291">Show hello help message.</span></span></td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-hello-azure-cosmos-db-emulator-and-azure-cosmos-db"></a><span data-ttu-id="f9741-292">Rozdíly mezi hello emulátoru DB Cosmos Azure a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f9741-292">Differences between hello Azure Cosmos DB Emulator and Azure Cosmos DB</span></span> 
<span data-ttu-id="f9741-293">Protože hello emulátoru DB Cosmos Azure poskytuje emulované prostředí spuštěna na vývojáře místní pracovní stanici, existují určité rozdíly ve funkcích mezi hello emulátoru a účet Azure Cosmos DB v cloudu hello:</span><span class="sxs-lookup"><span data-stu-id="f9741-293">Because hello Azure Cosmos DB Emulator provides an emulated environment running on a local developer workstation, there are some differences in functionality between hello emulator and an Azure Cosmos DB account in hello cloud:</span></span>

* <span data-ttu-id="f9741-294">Hello Azure Cosmos DB emulátoru podporuje pouze jeden účet opravené a dobře známé hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="f9741-294">hello Azure Cosmos DB Emulator supports only a single fixed account and a well-known master key.</span></span>  <span data-ttu-id="f9741-295">Opětovné generování klíče není možné v hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-295">Key regeneration is not possible in hello Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="f9741-296">Hello emulátoru DB Azure Cosmos není škálovatelné služby a nebude podporovat velké množství kolekcí.</span><span class="sxs-lookup"><span data-stu-id="f9741-296">hello Azure Cosmos DB Emulator is not a scalable service and will not support a large number of collections.</span></span>
* <span data-ttu-id="f9741-297">Hello emulátoru DB Azure Cosmos není simulovat různé [úrovně konzistence Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="f9741-297">hello Azure Cosmos DB Emulator does not simulate different [Azure Cosmos DB consistency levels](consistency-levels.md).</span></span>
* <span data-ttu-id="f9741-298">Hello emulátoru DB Azure Cosmos není simulovat [replikace více oblast](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="f9741-298">hello Azure Cosmos DB Emulator does not simulate [multi-region replication](distribute-data-globally.md).</span></span>
* <span data-ttu-id="f9741-299">Hello emulátoru DB Cosmos Azure nepodporuje přepsání hello služby kvóty, které jsou k dispozici ve službě Azure Cosmos DB hello (např. omezení velikosti dokumentu, dělenou kolekci výraznější úložiště).</span><span class="sxs-lookup"><span data-stu-id="f9741-299">hello Azure Cosmos DB Emulator does not support hello service quota overrides that are available in hello Azure Cosmos DB service (e.g. document size limits, increased partitioned collection storage).</span></span>
* <span data-ttu-id="f9741-300">Jako vaší kopie aplikace hello emulátoru DB Cosmos Azure nemusí být až toodate s hello poslední změny se službou Azure Cosmos DB hello, prosím [Plánovač kapacity Azure Cosmos DB](https://www.documentdb.com/capacityplanner) tooaccurately odhad produkční propustnost (ruština) potřebám vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9741-300">As your copy of hello Azure Cosmos DB Emulator might not be up toodate with hello most recent changes with hello Azure Cosmos DB service, please [Azure Cosmos DB capacity planner](https://www.documentdb.com/capacityplanner) tooaccurately estimate production throughput (RUs) needs of your application.</span></span>

## <span data-ttu-id="f9741-301"><a id="set-partitioncount"></a>Změnit hello počet kolekcí</span><span class="sxs-lookup"><span data-stu-id="f9741-301"><a id="set-partitioncount"></a>Change hello number of collections</span></span>

<span data-ttu-id="f9741-302">Ve výchozím nastavení můžete vytvořit kolekce tvořené jedním oddílem too25 nebo 1 dělenou kolekci pomocí hello emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f9741-302">By default, you can create up too25 single partition collections, or 1 partitioned collection using hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="f9741-303">Změnou hello **PartitionCount** hodnotu, můžete vytvořit kolekce tvořené jedním oddílem too250 nebo 10 dělené kolekce nebo libovolnou kombinaci hello dva, které není delší než 250 jedním oddíly (kde 1 rozdělena na oddíly kolekce = 25 kolekce tvořené jedním oddílem).</span><span class="sxs-lookup"><span data-stu-id="f9741-303">By modifying hello **PartitionCount** value, you can create up too250 single partition collections or 10 partitioned collections, or any combination of hello two that does not exceed 250 single partitions (where 1 partitioned collection = 25 single partition collection).</span></span>

<span data-ttu-id="f9741-304">Když zkusíte toocreate kolekci po překročení hello aktuální počet oddílů, hello emulátoru vyhodí výjimku ServiceUnavailable, s následující zprávou hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-304">If you attempt toocreate a collection after hello current partition count has been exceeded, hello emulator throws a ServiceUnavailable exception, with hello following message.</span></span>

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    toobring more and more capacity online, and encourage you tootry again. 
    Please do not hesitate tooemail docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

<span data-ttu-id="f9741-305">toochange hello počet kolekcí k dispozici toohello Azure Cosmos DB emulátoru, hello následující:</span><span class="sxs-lookup"><span data-stu-id="f9741-305">toochange hello number of collections available toohello Azure Cosmos DB Emulator, do hello following:</span></span>

1. <span data-ttu-id="f9741-306">Odstranit všechna místní data emulátoru DB Cosmos Azure kliknutím pravým tlačítkem na hello **emulátoru DB Cosmos Azure** ikonu na hlavním panelu systému hello a klepnutím **obnovit Data...** .</span><span class="sxs-lookup"><span data-stu-id="f9741-306">Delete all local Azure Cosmos DB Emulator data by right-clicking hello **Azure Cosmos DB Emulator** icon on hello system tray, and then clicking **Reset Data…**.</span></span>
2. <span data-ttu-id="f9741-307">Odstraňte všechna data emulátoru v této složce C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="f9741-307">Delete all emulator data in this folder C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span></span>
3. <span data-ttu-id="f9741-308">Ukončete všechny otevřené instance kliknutím pravým tlačítkem na hello **emulátoru DB Cosmos Azure** ikonu na panelu systému hello a pak levým na **ukončení**.</span><span class="sxs-lookup"><span data-stu-id="f9741-308">Exit all open instances by right-clicking hello **Azure Cosmos DB Emulator** icon on hello system tray, and then clicking **Exit**.</span></span> <span data-ttu-id="f9741-309">Může trvat několik minut pro všechny instance tooexit.</span><span class="sxs-lookup"><span data-stu-id="f9741-309">It may take a minute for all instances tooexit.</span></span>
4. <span data-ttu-id="f9741-310">Nainstalujte nejnovější verzi hello hello [emulátoru DB Cosmos Azure](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="f9741-310">Install hello latest version of hello [Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator).</span></span>
5. <span data-ttu-id="f9741-311">Spusťte emulátor hello s hello PartitionCount příznak nastavením hodnoty < = 250.</span><span class="sxs-lookup"><span data-stu-id="f9741-311">Launch hello emulator with hello PartitionCount flag by setting a value <= 250.</span></span> <span data-ttu-id="f9741-312">Například: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span><span class="sxs-lookup"><span data-stu-id="f9741-312">For example: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f9741-313">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f9741-313">Troubleshooting</span></span>

<span data-ttu-id="f9741-314">Použijte následující tipy toohelp řešení problémů, které zaznamenáte pomocí emulátoru Azure Cosmos DB hello hello:</span><span class="sxs-lookup"><span data-stu-id="f9741-314">Use hello following tips toohelp troubleshoot issues you encounter with hello Azure Cosmos DB emulator:</span></span>

- <span data-ttu-id="f9741-315">Pokud nainstaluje novou verzi hello emulátoru a dochází k chybám, ověřte, že resetovat vaše data.</span><span class="sxs-lookup"><span data-stu-id="f9741-315">If you installed a new version of hello Emulator and are experiencing errors, ensure you reset your data.</span></span> <span data-ttu-id="f9741-316">Pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure hello na hlavním panelu systému hello a potom kliknutím na Obnovit Data můžete obnovit data...</span><span class="sxs-lookup"><span data-stu-id="f9741-316">You can reset your data by right-clicking hello Azure Cosmos DB Emulator icon on hello system tray, and then clicking Reset Data….</span></span> <span data-ttu-id="f9741-317">Pokud se chyby hello nevyřeší, můžete odinstalovat a znovu nainstalujte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-317">If that does not fix hello errors, you can uninstall and reinstall hello app.</span></span> <span data-ttu-id="f9741-318">V tématu [odinstalovat místní emulátoru hello](#uninstall) pokyny.</span><span class="sxs-lookup"><span data-stu-id="f9741-318">See [Uninstall hello local emulator](#uninstall) for instructions.</span></span>

- <span data-ttu-id="f9741-319">Pokud dojde k chybě hello Azure Cosmos DB emulátoru, shromažďovat výpis soubory ze složky c:\Users\user_name\AppData\Local\CrashDumps, zkomprimovat a připojte je příliš e-mailu tooan[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f9741-319">If hello Azure Cosmos DB emulator crashes, collect dump files from c:\Users\user_name\AppData\Local\CrashDumps folder, compress them, and attach them tooan email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="f9741-320">Pokud se setkáváte s havárií v CosmosDB.StartupEntryPoint.exe, spusťte následující příkaz z příkazového řádku správce hello:`lodctr /R`</span><span class="sxs-lookup"><span data-stu-id="f9741-320">If you experience crashes in CosmosDB.StartupEntryPoint.exe, run hello following command from an admin command prompt: `lodctr /R`</span></span> 

- <span data-ttu-id="f9741-321">Pokud narazíte na potíže s připojením [shromažďování trasovacích souborů](#trace-files)zkomprimovat a připojte je příliš tooan e-mailu[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f9741-321">If you encounter a connectivity issue, [collect trace files](#trace-files), compress them, and attach them tooan email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="f9741-322">Pokud se zobrazí **služba není k dispozici** zprávy, hello emulátoru může dojít k selhání tooinitialize hello síťových protokolů.</span><span class="sxs-lookup"><span data-stu-id="f9741-322">If you receive a **Service Unavailable** message, hello emulator might be failing tooinitialize hello network stack.</span></span> <span data-ttu-id="f9741-323">Pokud máte hello Pulse zabezpečené klienta nebo Juniper sítě nainstalovaným klientem, jako jejich ovladače filtru sítě může způsobit hello problém, zkontrolujte toosee.</span><span class="sxs-lookup"><span data-stu-id="f9741-323">Check toosee if you have hello Pulse secure client or Juniper networks client installed, as their network filter drivers may cause hello problem.</span></span> <span data-ttu-id="f9741-324">Odinstalace ovladače filtru třetích stran sítě obvykle řeší problém, hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-324">Uninstalling third party network filter drivers typically fixes hello issue.</span></span>

### <span data-ttu-id="f9741-325"><a id="trace-files"></a>Shromažďování trasovacích souborů</span><span class="sxs-lookup"><span data-stu-id="f9741-325"><a id="trace-files"></a>Collect trace files</span></span>

<span data-ttu-id="f9741-326">toocollect ladění trasování, spusťte následující příkazy z příkazového řádku pro správu hello:</span><span class="sxs-lookup"><span data-stu-id="f9741-326">toocollect debugging traces, run hello following commands from an administrative command prompt:</span></span>

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. <span data-ttu-id="f9741-327">`CosmosDB.Emulator.exe /shutdown`.</span><span class="sxs-lookup"><span data-stu-id="f9741-327">`CosmosDB.Emulator.exe /shutdown`.</span></span> <span data-ttu-id="f9741-328">Kukátko hello systému panelu toomake zda hello program byl vypnut, může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="f9741-328">Watch hello system tray toomake sure hello program has shut down, it may take a minute.</span></span> <span data-ttu-id="f9741-329">Můžete také stačí kliknout na **ukončení** hello Azure Cosmos DB emulátoru uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f9741-329">You can also just click **Exit** in hello Azure Cosmos DB emulator user interface.</span></span>
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. <span data-ttu-id="f9741-330">Reprodukujte problém hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-330">Reproduce hello problem.</span></span> <span data-ttu-id="f9741-331">Pokud Průzkumníku dat nefunguje, potřebujete jenom toowait pro tooopen prohlížeče hello pár sekund toocatch hello došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="f9741-331">If Data Explorer is not working, you only need toowait for hello browser tooopen for a few seconds toocatch hello error.</span></span>
5. `CosmosDB.Emulator.exe /stoptraces`
6. <span data-ttu-id="f9741-332">Přejděte příliš`%ProgramFiles%\Azure Cosmos DB Emulator` a najít soubor docdbemulator_000001.etl hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-332">Navigate too`%ProgramFiles%\Azure Cosmos DB Emulator` and find hello docdbemulator_000001.etl file.</span></span>
7. <span data-ttu-id="f9741-333">Odeslat soubor .etl hello návodem zkopírujte příliš[ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) pro ladění.</span><span class="sxs-lookup"><span data-stu-id="f9741-333">Send hello .etl file along with repro steps too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) for debugging.</span></span>

### <span data-ttu-id="f9741-334"><a id="uninstall"></a>Odinstalujte hello místní emulátoru</span><span class="sxs-lookup"><span data-stu-id="f9741-334"><a id="uninstall"></a>Uninstall hello local Emulator</span></span>

1. <span data-ttu-id="f9741-335">Ukončete všechny otevřené instance hello místní emulátoru pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure hello na hlavním panelu systému hello, a potom kliknutím na ukončení.</span><span class="sxs-lookup"><span data-stu-id="f9741-335">Exit all open instances of hello local Emulator by right-clicking hello Azure Cosmos DB Emulator icon on hello system tray, and then clicking Exit.</span></span> <span data-ttu-id="f9741-336">Může trvat několik minut pro všechny instance tooexit.</span><span class="sxs-lookup"><span data-stu-id="f9741-336">It may take a minute for all instances tooexit.</span></span>
2. <span data-ttu-id="f9741-337">Hello Windows vyhledávacího pole zadejte **aplikace a funkce** a klikněte na hello **aplikace a funkce (nastavení systému)** výsledek.</span><span class="sxs-lookup"><span data-stu-id="f9741-337">In hello Windows search box, type **Apps & features** and click on hello **Apps & features (System settings)** result.</span></span>
3. <span data-ttu-id="f9741-338">V hello seznam aplikací, posuňte příliš**emulátoru DB Cosmos Azure**, vyberte ho, klikněte na **odinstalace**, potvrďte a klikněte na **odinstalovat** znovu.</span><span class="sxs-lookup"><span data-stu-id="f9741-338">In hello list of apps, scroll too**Azure Cosmos DB Emulator**, select it, click **Uninstall**, then confirm and click **Uninstall** again.</span></span>
4. <span data-ttu-id="f9741-339">Při odinstalaci aplikace hello přejděte tooC:\Users\<uživatele > \AppData\Local\CosmosDBEmulator, odstraňte složku hello.</span><span class="sxs-lookup"><span data-stu-id="f9741-339">When hello app is uninstalled, navigate tooC:\Users\<user>\AppData\Local\CosmosDBEmulator and delete hello folder.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f9741-340">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9741-340">Next steps</span></span>

<span data-ttu-id="f9741-341">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="f9741-341">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f9741-342">Nainstalovat hello místní emulátoru</span><span class="sxs-lookup"><span data-stu-id="f9741-342">Installed hello local Emulator</span></span>
> * <span data-ttu-id="f9741-343">Rand – hello emulátoru na Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="f9741-343">Rand hello Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="f9741-344">Ověřené žádosti</span><span class="sxs-lookup"><span data-stu-id="f9741-344">Authenticated requests</span></span>
> * <span data-ttu-id="f9741-345">Použít hello Průzkumníku dat v emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="f9741-345">Used hello Data Explorer in hello Emulator</span></span>
> * <span data-ttu-id="f9741-346">Exportovaný certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="f9741-346">Exported SSL certificates</span></span>
> * <span data-ttu-id="f9741-347">Volat hello emulátoru z příkazového řádku hello</span><span class="sxs-lookup"><span data-stu-id="f9741-347">Called hello Emulator from hello command line</span></span>
> * <span data-ttu-id="f9741-348">Shromážděné trasovací soubory</span><span class="sxs-lookup"><span data-stu-id="f9741-348">Collected trace files</span></span>

<span data-ttu-id="f9741-349">V tomto kurzu naučili jak toouse hello místní emulátor pro volné místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="f9741-349">In this tutorial, you've learned how toouse hello local Emulator for free local development.</span></span> <span data-ttu-id="f9741-350">Teď můžete pokračovat dalším kurzu toohello a zjistěte, jak certifikáty SSL emulátoru tooexport.</span><span class="sxs-lookup"><span data-stu-id="f9741-350">You can now proceed toohello next tutorial and learn how tooexport Emulator SSL certificates.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="f9741-351">Exportu certifikátů emulátoru DB Cosmos Azure hello</span><span class="sxs-lookup"><span data-stu-id="f9741-351">Export hello Azure Cosmos DB Emulator certificates</span></span>](local-emulator-export-ssl-certificates.md)
