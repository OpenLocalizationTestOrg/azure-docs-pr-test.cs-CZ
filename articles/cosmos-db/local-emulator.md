---
title: "Vývoj místně pomocí emulátoru DB Cosmos Azure | Microsoft Docs"
description: "Pomocí emulátoru DB Cosmos Azure, můžete vývoj a testování vaší aplikace místně pro bezplatné bez vytváření předplatného Azure."
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
ms.openlocfilehash: a0f6a845a345ebd4ef0a58abf4934ce400103109
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a><span data-ttu-id="e9098-104">Použití emulátoru DB Cosmos Azure pro místní vývoj a testování</span><span class="sxs-lookup"><span data-stu-id="e9098-104">Use the Azure Cosmos DB Emulator for local development and testing</span></span>

<table>
<tr>
  <td><span data-ttu-id="e9098-105"><strong>Binární soubory</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-105"><strong>Binaries</strong></span></span></td>
  <td>[<span data-ttu-id="e9098-106">Stáhněte si instalační služby MSI</span><span class="sxs-lookup"><span data-stu-id="e9098-106">Download MSI</span></span>](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-107"><strong>Docker</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-107"><strong>Docker</strong></span></span></td>
  <td>[<span data-ttu-id="e9098-108">Úložiště docker Hub</span><span class="sxs-lookup"><span data-stu-id="e9098-108">Docker Hub</span></span>](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-109"><strong>Zdroj docker</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-109"><strong>Docker source</strong></span></span></td>
  <td>[<span data-ttu-id="e9098-110">GitHub</span><span class="sxs-lookup"><span data-stu-id="e9098-110">Github</span></span>](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
<span data-ttu-id="e9098-111">Emulátor DB Cosmos Azure poskytuje místní prostředí, které emuluje služby Azure Cosmos DB pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="e9098-111">The Azure Cosmos DB Emulator provides a local environment that emulates the Azure Cosmos DB service for development purposes.</span></span> <span data-ttu-id="e9098-112">Pomocí emulátoru DB Cosmos Azure, můžete vyvíjet a testovat svou aplikaci lokálně, bez vytváření předplatného Azure nebo nákladům.</span><span class="sxs-lookup"><span data-stu-id="e9098-112">Using the Azure Cosmos DB Emulator, you can develop and test your application locally, without creating an Azure subscription or incurring any costs.</span></span> <span data-ttu-id="e9098-113">Až budete spokojeni s jak funguje aplikaci v emulátoru DB Cosmos Azure, můžete přejít k používání účtu Azure Cosmos DB v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e9098-113">When you're satisfied with how your application is working in the Azure Cosmos DB Emulator, you can switch to using an Azure Cosmos DB account in the cloud.</span></span>

<span data-ttu-id="e9098-114">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="e9098-114">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="e9098-115">Instalace v emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-115">Installing the Emulator</span></span>
> * <span data-ttu-id="e9098-116">Emulátor systémem Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="e9098-116">Running the Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="e9098-117">Ověřování požadavků</span><span class="sxs-lookup"><span data-stu-id="e9098-117">Authenticating requests</span></span>
> * <span data-ttu-id="e9098-118">Pomocí Průzkumníku dat v emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-118">Using the Data Explorer in the Emulator</span></span>
> * <span data-ttu-id="e9098-119">Export certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="e9098-119">Exporting SSL certificates</span></span>
> * <span data-ttu-id="e9098-120">Volání metody emulátoru z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e9098-120">Calling the Emulator from the command line</span></span>
> * <span data-ttu-id="e9098-121">Shromažďování trasovacích souborů</span><span class="sxs-lookup"><span data-stu-id="e9098-121">Collecting trace files</span></span>

<span data-ttu-id="e9098-122">Doporučujeme začít následujícím videem, kde Kirill Gavrylyuk ukazuje, jak začít pracovat s emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-122">We recommend getting started by watching the following video, where Kirill Gavrylyuk shows how to get started with the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="e9098-123">Všimněte si, že odkazuje na video emulátoru jako DocumentDB emulátor, ale nástroj sám byl přejmenován emulátoru Azure Cosmos DB od zaznamenávat videa.</span><span class="sxs-lookup"><span data-stu-id="e9098-123">Note that the video refers to the emulator as the DocumentDB Emulator, but the tool itself has been renamed the Azure Cosmos DB Emulator since taping the video.</span></span> <span data-ttu-id="e9098-124">Všechny informace ve videu jsou stále správné pro emulátor Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-124">All information in the video is still accurate for the Azure Cosmos DB Emulator.</span></span> 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-the-emulator-works"></a><span data-ttu-id="e9098-125">Jak funguje v emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-125">How the Emulator works</span></span>
<span data-ttu-id="e9098-126">Emulátor DB Cosmos Azure poskytuje zachováním emulace služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e9098-126">The Azure Cosmos DB Emulator provides a high-fidelity emulation of the Azure Cosmos DB service.</span></span> <span data-ttu-id="e9098-127">Podporuje stejné funkce jako Azure Cosmos databáze, včetně podpory pro vytváření a dotazování dokumentů JSON, zřizování a škálování kolekce a provádění uložené procedury a triggery.</span><span class="sxs-lookup"><span data-stu-id="e9098-127">It supports identical functionality as Azure Cosmos DB, including support for creating and querying JSON documents, provisioning and scaling collections, and executing stored procedures and triggers.</span></span> <span data-ttu-id="e9098-128">Můžete vyvíjet a testovat aplikace pomocí emulátoru DB Cosmos Azure a jejich nasazení do Azure v globálním měřítku tím, že právě konfigurací jedné změňte koncového bodu připojení pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e9098-128">You can develop and test applications using the Azure Cosmos DB Emulator, and deploy them to Azure at global scale by just making a single configuration change to the connection endpoint for Azure Cosmos DB.</span></span>

<span data-ttu-id="e9098-129">Když jsme vytvořili místní emulace zachováním skutečné služby Azure Cosmos DB, se liší od služby implementace emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-129">While we created a high-fidelity local emulation of the actual Azure Cosmos DB service, the implementation of the Azure Cosmos DB Emulator is different than that of the service.</span></span> <span data-ttu-id="e9098-130">Například emulátoru DB Cosmos Azure používá standardní součásti operačního systému, například místního systému souborů pro trvalosti a zásobník protokolu HTTPS pro připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="e9098-130">For example, the Azure Cosmos DB Emulator uses standard OS components such as the local file system for persistence, and HTTPS protocol stack for connectivity.</span></span> <span data-ttu-id="e9098-131">To znamená, že některé funkce, které jsou závislé na infrastrukturu Azure jako globální replikace, jednociferné milisekundu latence pro čtení/zápisu a přizpůsobitelné úrovně konzistence nejsou k dispozici prostřednictvím emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-131">This means that some functionality that relies on Azure infrastructure like global replication, single-digit millisecond latency for reads/writes, and tunable consistency levels are not available via the Azure Cosmos DB Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="e9098-132">V tuto chvíli Průzkumníku dat v emulátoru podporuje pouze vytvoření kolekce DocumentDB rozhraní API a kolekcí MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e9098-132">At this time the Data Explorer in the emulator only supports the creation of DocumentDB API collections and MongoDB collections.</span></span> <span data-ttu-id="e9098-133">Průzkumníku dat v emulátoru v současné době nepodporuje vytvoření tabulky a grafy.</span><span class="sxs-lookup"><span data-stu-id="e9098-133">The Data Explorer in the emulator does not currently support the creation of tables and graphs.</span></span> 

## <a name="system-requirements"></a><span data-ttu-id="e9098-134">Požadavky na systém</span><span class="sxs-lookup"><span data-stu-id="e9098-134">System requirements</span></span>
<span data-ttu-id="e9098-135">Emulátor DB Cosmos Azure má následující požadavky na hardware a software:</span><span class="sxs-lookup"><span data-stu-id="e9098-135">The Azure Cosmos DB Emulator has the following hardware and software requirements:</span></span>

* <span data-ttu-id="e9098-136">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="e9098-136">Software requirements</span></span>
  * <span data-ttu-id="e9098-137">Windows Server 2012 R2, Windows Server 2016 nebo Windows 10</span><span class="sxs-lookup"><span data-stu-id="e9098-137">Windows Server 2012 R2, Windows Server 2016, or Windows 10</span></span>
*   <span data-ttu-id="e9098-138">Minimální požadavky na Hardware</span><span class="sxs-lookup"><span data-stu-id="e9098-138">Minimum Hardware requirements</span></span>
  * <span data-ttu-id="e9098-139">2 GB PAMĚTI RAM</span><span class="sxs-lookup"><span data-stu-id="e9098-139">2 GB RAM</span></span>
  * <span data-ttu-id="e9098-140">10 GB volného místa na disku</span><span class="sxs-lookup"><span data-stu-id="e9098-140">10 GB available hard disk space</span></span>

## <a name="installation"></a><span data-ttu-id="e9098-141">Instalace</span><span class="sxs-lookup"><span data-stu-id="e9098-141">Installation</span></span>
<span data-ttu-id="e9098-142">Můžete stáhnout a nainstalovat emulátoru DB Cosmos Azure z [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="e9098-142">You can download and install the Azure Cosmos DB Emulator from the [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span></span> 

> [!NOTE]
> <span data-ttu-id="e9098-143">Pro instalaci, konfiguraci a spuštění emulátoru Azure Cosmos DB, musíte mít oprávnění správce v počítači.</span><span class="sxs-lookup"><span data-stu-id="e9098-143">To install, configure, and run the Azure Cosmos DB Emulator, you must have administrative privileges on the computer.</span></span>

## <a name="running-on-docker-for-windows"></a><span data-ttu-id="e9098-144">Systémem Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="e9098-144">Running on Docker for Windows</span></span>

<span data-ttu-id="e9098-145">Emulátor DB Cosmos Azure můžete spustit na Docker pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="e9098-145">The Azure Cosmos DB Emulator can be run on Docker for Windows.</span></span> <span data-ttu-id="e9098-146">Emulátor na Docker pro Oracle Linux nefunguje.</span><span class="sxs-lookup"><span data-stu-id="e9098-146">The Emulator does not work on Docker for Oracle Linux.</span></span>

<span data-ttu-id="e9098-147">Jakmile máte [Docker pro systém Windows](https://www.docker.com/docker-windows) nainstalován, můžete můžete načítat bitová kopie emulátoru z úložiště Docker Hub spuštěním následujícího příkazu z své oblíbené prostředí (cmd.exe, prostředí PowerShell, atd.).</span><span class="sxs-lookup"><span data-stu-id="e9098-147">Once you have [Docker for Windows](https://www.docker.com/docker-windows) installed, you can pull the Emulator image from Docker Hub by running the following command from your favorite shell (cmd.exe, PowerShell, etc.).</span></span>

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
<span data-ttu-id="e9098-148">Chcete-li spustit bitovou kopii, spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="e9098-148">To start the image, run the following commands.</span></span>

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

<span data-ttu-id="e9098-149">Odpověď bude vypadat podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="e9098-149">The response looks similar to the following:</span></span>

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

<span data-ttu-id="e9098-150">Zavřením interaktivní prostředí, jakmile emulátoru už spustit v emulátoru kontejnerů dojde k vypnutí.</span><span class="sxs-lookup"><span data-stu-id="e9098-150">Closing the interactive shell once the Emulator has been started will shutdown the Emulator’s container.</span></span>

<span data-ttu-id="e9098-151">Použijte koncový bod a hlavního klíče v z odpovědi v vašeho klienta a certifikát SSL naimportovat do svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="e9098-151">Use the endpoint and master key in from the response in your client and import the SSL certificate into your host.</span></span> <span data-ttu-id="e9098-152">Chcete-li importovat certifikát SSL, proveďte následující z příkazového řádku správce:</span><span class="sxs-lookup"><span data-stu-id="e9098-152">To import the SSL certificate, do the following from an admin command prompt:</span></span>

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-the-emulator"></a><span data-ttu-id="e9098-153">Spusťte emulátor</span><span class="sxs-lookup"><span data-stu-id="e9098-153">Start the Emulator</span></span>

<span data-ttu-id="e9098-154">Spuštění emulátoru DB Cosmos Azure, vyberte tlačítko Start a stiskněte klávesu Windows.</span><span class="sxs-lookup"><span data-stu-id="e9098-154">To start the Azure Cosmos DB Emulator, select the Start button or press the Windows key.</span></span> <span data-ttu-id="e9098-155">Začněte psát **emulátoru DB Cosmos Azure**a vyberte emulátor ze seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="e9098-155">Begin typing **Azure Cosmos DB Emulator**, and select the emulator from the list of applications.</span></span> 

![Kliknutím na tlačítko Start nebo stiskněte klávesu Windows, začněte psát ** Azure Cosmos DB emulátoru ** a vyberte emulátor ze seznamu aplikací](./media/local-emulator/database-local-emulator-start.png)

<span data-ttu-id="e9098-157">Když na emulátoru běží, se zobrazí na ikonu v oznamovací oblasti hlavního panelu Windows.</span><span class="sxs-lookup"><span data-stu-id="e9098-157">When the emulator is running, you'll see an icon in the Windows taskbar notification area.</span></span> ![Azure Cosmos DB místní emulátoru panelu oznámení](./media/local-emulator/database-local-emulator-taskbar.png)

<span data-ttu-id="e9098-159">Emulátor DB Cosmos Azure ve výchozím nastavení spouští v místním počítači ("localhost") naslouchá na portu 8081.</span><span class="sxs-lookup"><span data-stu-id="e9098-159">The Azure Cosmos DB Emulator by default runs on the local machine ("localhost") listening on port 8081.</span></span>

<span data-ttu-id="e9098-160">Emulátor DB Cosmos Azure je nainstalována ve výchozím nastavení `C:\Program Files\Azure Cosmos DB Emulator` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e9098-160">The Azure Cosmos DB Emulator is installed by default to the `C:\Program Files\Azure Cosmos DB Emulator` directory.</span></span> <span data-ttu-id="e9098-161">Můžete také spustit a zastavit emulátoru z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e9098-161">You can also start and stop the emulator from the command-line.</span></span> <span data-ttu-id="e9098-162">V tématu [odkaz na nástroj příkazového řádku](#command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e9098-162">See [command-line tool reference](#command-line) for more information.</span></span>

## <a name="start-data-explorer"></a><span data-ttu-id="e9098-163">Spuštění Průzkumníka dat</span><span class="sxs-lookup"><span data-stu-id="e9098-163">Start Data Explorer</span></span>

<span data-ttu-id="e9098-164">Při spuštění v Azure Cosmos DB emulátoru se automaticky otevře Průzkumníku dat Azure Cosmos DB v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e9098-164">When the Azure Cosmos DB emulator launches it will automatically open the Azure Cosmos DB Data Explorer in your browser.</span></span> <span data-ttu-id="e9098-165">Adresa se zobrazí jako [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span><span class="sxs-lookup"><span data-stu-id="e9098-165">The address will appear as [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span></span> <span data-ttu-id="e9098-166">Pokud ho zavřete v Exploreru a chtěli později ho znovu otevřete, můžete otevřít adresu URL v prohlížeči nebo spustit z emulátoru DB Cosmos Azure v oznamovací ikoně Windows, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="e9098-166">If you close the explorer and would like to re-open it later, you can either open the URL in your browser or launch it from the Azure Cosmos DB Emulator in the Windows Tray Icon as shown below.</span></span>

![Azure Cosmos DB místní emulátoru data Průzkumníka Spouštěče](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a><span data-ttu-id="e9098-168">Kontrola aktualizací</span><span class="sxs-lookup"><span data-stu-id="e9098-168">Checking for updates</span></span>
<span data-ttu-id="e9098-169">Průzkumník dat určuje, zda je k dispozici ke stažení nové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e9098-169">Data Explorer indicates if there is a new update available for download.</span></span> 

> [!NOTE]
> <span data-ttu-id="e9098-170">Data vytvořená ve verzi emulátoru DB Cosmos Azure nemusí být dostupné při použití jiné verze.</span><span class="sxs-lookup"><span data-stu-id="e9098-170">Data created in one version of the Azure Cosmos DB Emulator is not guaranteed to be accessible when using a different version.</span></span> <span data-ttu-id="e9098-171">Pokud potřebujete zachovat data pro z dlouhodobého hlediska, doporučujeme uložit data v Azure Cosmos DB účet, nikoli v emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-171">If you need to persist your data for the long term, it is recommended that you store that data in an Azure Cosmos DB account, rather than in the Azure Cosmos DB Emulator.</span></span> 

## <a name="authenticating-requests"></a><span data-ttu-id="e9098-172">Ověřování požadavků</span><span class="sxs-lookup"><span data-stu-id="e9098-172">Authenticating requests</span></span>
<span data-ttu-id="e9098-173">Stejně jako s Azure DB Cosmos v cloudu, musí být ověřeny každého požadavku, které vytváříte emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="e9098-173">Just as with Azure Cosmos DB in the cloud, every request that you make against the Azure Cosmos DB Emulator must be authenticated.</span></span> <span data-ttu-id="e9098-174">Emulátor DB Cosmos Azure podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověřování hlavního klíče.</span><span class="sxs-lookup"><span data-stu-id="e9098-174">The Azure Cosmos DB Emulator supports a single fixed account and a well-known authentication key for master key authentication.</span></span> <span data-ttu-id="e9098-175">Tento účet a klíč jsou pouze povolené pro použití s emulátoru Azure Cosmos DB přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e9098-175">This account and key are the only credentials permitted for use with the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="e9098-176">Jsou:</span><span class="sxs-lookup"><span data-stu-id="e9098-176">They are:</span></span>

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> <span data-ttu-id="e9098-177">Hlavní klíč nepodporuje emulátor DB Cosmos Azure je určený pro použití pouze pomocí emulátoru.</span><span class="sxs-lookup"><span data-stu-id="e9098-177">The master key supported by the Azure Cosmos DB Emulator is intended for use only with the emulator.</span></span> <span data-ttu-id="e9098-178">Váš účet Azure Cosmos DB produkční a klíč nelze použít s emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-178">You cannot use your production Azure Cosmos DB account and key with the Azure Cosmos DB Emulator.</span></span> 

> [!NOTE] 
> <span data-ttu-id="e9098-179">Pokud jste spustili emulátoru s parametrem /Key, použijte generovaný klíč místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="</span><span class="sxs-lookup"><span data-stu-id="e9098-179">If you have started the emulator with the /Key option, then use the generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="e9098-180">Kromě toho právě jako služba Azure Cosmos DB emulátoru DB Cosmos Azure podporuje pouze zabezpečenou komunikaci prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="e9098-180">Additionally, just as the Azure Cosmos DB service, the Azure Cosmos DB Emulator supports only secure communication via SSL.</span></span>

## <a name="running-the-emulator-on-a-local-network"></a><span data-ttu-id="e9098-181">Spuštěný v emulátoru v místní síti</span><span class="sxs-lookup"><span data-stu-id="e9098-181">Running the emulator on a local network</span></span>

<span data-ttu-id="e9098-182">Emulátor serveru můžete spustit v místní síti.</span><span class="sxs-lookup"><span data-stu-id="e9098-182">You can run the emulator on a local network.</span></span> <span data-ttu-id="e9098-183">Pokud chcete povolit přístup k síti, zadejte možnost /AllowNetworkAccess na [příkazového řádku](#command-line-syntax), což také vyžaduje, že zadáváte /Key = key_string nebo/keyfile = název_souboru.</span><span class="sxs-lookup"><span data-stu-id="e9098-183">To enable network access, specify the /AllowNetworkAccess option at the [command line](#command-line-syntax), which also requires that you specify /Key=key_string or /KeyFile=file_name.</span></span> <span data-ttu-id="e9098-184">Můžete použít /GenKeyFile = název_souboru můžete vytvořit soubor s předem náhodný klíč.</span><span class="sxs-lookup"><span data-stu-id="e9098-184">You can use /GenKeyFile=file_name to generate a file with a random key upfront.</span></span>  <span data-ttu-id="e9098-185">Pak můžete předat, že k/keyfile = název_souboru nebo /Key = contents_of_file.</span><span class="sxs-lookup"><span data-stu-id="e9098-185">Then you can pass that to /KeyFile=file_name or /Key=contents_of_file.</span></span>

<span data-ttu-id="e9098-186">Pokud chcete povolit přístup k síti první uživatel by měl vypnutí emulátoru a odstranit adresář data na emulátoru (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span><span class="sxs-lookup"><span data-stu-id="e9098-186">To enable network access for the first time the user should shutdown the emulator and delete the emulator’s data directory (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span></span>

## <a name="developing-with-the-emulator"></a><span data-ttu-id="e9098-187">Vývoj v emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-187">Developing with the Emulator</span></span>
<span data-ttu-id="e9098-188">Jakmile máte emulátoru DB Cosmos Azure spuštěna na pracovní ploše, můžete použít libovolnou podporované [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) nebo [REST API služby Azure Cosmos DB](/rest/api/documentdb/) pro interakci s emulátor.</span><span class="sxs-lookup"><span data-stu-id="e9098-188">Once you have the Azure Cosmos DB Emulator running on your desktop, you can use any supported [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) or the [Azure Cosmos DB REST API](/rest/api/documentdb/) to interact with the Emulator.</span></span> <span data-ttu-id="e9098-189">Emulátor DB Cosmos Azure také zahrnuje integrovanou Průzkumníku dat, která umožňuje vytvářet kolekce pro DocumentDB a rozhraní API MongoDB a zobrazení a úpravám dokumentů bez psaní jakéhokoli kódu.</span><span class="sxs-lookup"><span data-stu-id="e9098-189">The Azure Cosmos DB Emulator also includes a built-in Data Explorer that lets you create collections for the DocumentDB and MongoDB APIs, and view and edit documents without writing any code.</span></span>   

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

<span data-ttu-id="e9098-190">Pokud používáte [Azure Cosmos DB podporou protokolů pro MongoDB](mongodb-introduction.md), použijte následující připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="e9098-190">If you're using [Azure Cosmos DB protocol support for MongoDB](mongodb-introduction.md), please use the following connection string:</span></span>

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

<span data-ttu-id="e9098-191">Můžete použít stávající nástroje, například [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) pro připojení k emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-191">You can use existing tools like [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) to connect to the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="e9098-192">Můžete také migrovat data mezi emulátoru DB Cosmos Azure a pomocí služby Azure Cosmos DB [nástroj pro migraci dat Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).</span><span class="sxs-lookup"><span data-stu-id="e9098-192">You can also migrate data between the Azure Cosmos DB Emulator and the Azure Cosmos DB service using the [Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).</span></span>

> [!NOTE] 
> <span data-ttu-id="e9098-193">Pokud jste spustili emulátoru s parametrem /Key, použijte generovaný klíč místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="</span><span class="sxs-lookup"><span data-stu-id="e9098-193">If you have started the emulator with the /Key option, then use the generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="e9098-194">Pomocí emulátoru služby v Azure Cosmos DB, ve výchozím nastavení, je možné vytvořit až 25 kolekce tvořené jedním oddílem nebo 1 dělenou kolekci.</span><span class="sxs-lookup"><span data-stu-id="e9098-194">Using the Azure Cosmos DB emulator, by default, you can create up to 25 single partition collections or 1 partitioned collection.</span></span> <span data-ttu-id="e9098-195">Další informace o změně tuto hodnotu najdete v tématu [nastavení hodnoty PartitionCount](#set-partitioncount).</span><span class="sxs-lookup"><span data-stu-id="e9098-195">For more information about changing this value, see [Setting the PartitionCount value](#set-partitioncount).</span></span>

## <a name="export-the-ssl-certificate"></a><span data-ttu-id="e9098-196">Export certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="e9098-196">Export the SSL certificate</span></span>

<span data-ttu-id="e9098-197">Jazyky rozhraní .NET a prostředí runtime použití úložiště certifikátů systému Windows k bezpečnému připojování k místní emulátoru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e9098-197">.NET languages and runtime use the Windows Certificate Store to securely connect to the Azure Cosmos DB local emulator.</span></span> <span data-ttu-id="e9098-198">Další jazyky mít vlastní metodu správy a použití certifikátů.</span><span class="sxs-lookup"><span data-stu-id="e9098-198">Other languages have their own method of managing and using certificates.</span></span> <span data-ttu-id="e9098-199">Java používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) zatímco používá Python [soketu obálky](https://docs.python.org/2/library/ssl.html).</span><span class="sxs-lookup"><span data-stu-id="e9098-199">Java uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) whereas Python uses [socket wrappers](https://docs.python.org/2/library/ssl.html).</span></span>

<span data-ttu-id="e9098-200">Aby bylo možné získat certifikát pro použití s jazyky a moduly runtime, který nelze integrovat do úložiště certifikátů Windows, musíte ho exportovat pomocí Správce certifikátů systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e9098-200">In order to obtain a certificate to use with languages and runtimes that do not integrate with the Windows Certificate Store you will need to export it using the Windows Certificate Manager.</span></span> <span data-ttu-id="e9098-201">Můžete spusťte ji spuštěním certlm.msc nebo pokyny krok za krokem v [exportu certifikátů emulátoru Azure Cosmos DB](./local-emulator-export-ssl-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="e9098-201">You can start it by running certlm.msc or follow the step by step instructions in [Export the Azure Cosmos DB Emulator Certificates](./local-emulator-export-ssl-certificates.md).</span></span> <span data-ttu-id="e9098-202">Jakmile správce certifikátů pracuje, otevřete osobní certifikáty, jak je uvedeno níže a exportujte certifikát s popisným názvem "DocumentDBEmulatorCertificate" jako kódováním BASE-64 souboru X.509 (.cer).</span><span class="sxs-lookup"><span data-stu-id="e9098-202">Once the certificate manager is running, open the Personal Certificates as shown below and export the certificate with the friendly name "DocumentDBEmulatorCertificate" as a BASE-64 encoded X.509 (.cer) file.</span></span>

![Certifikát SSL místní emulátoru služby Azure Cosmos DB](./media/local-emulator/database-local-emulator-ssl_certificate.png)

<span data-ttu-id="e9098-204">Certifikát X.509 lze importovat do úložiště certifikátů Java podle pokynů v [přidání certifikátu do úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span><span class="sxs-lookup"><span data-stu-id="e9098-204">The X.509 certificate can be imported into the Java certificate store by following the instructions in [Adding a Certificate to the Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span></span> <span data-ttu-id="e9098-205">Jakmile je certifikát importován do úložiště certifikátů, bude možné se připojit k emulátoru DB Cosmos Azure aplikací Java a MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e9098-205">Once the certificate is imported into the certificate store, Java and MongoDB applications will be able to connect to the Azure Cosmos DB Emulator.</span></span>

<span data-ttu-id="e9098-206">Při připojování k emulátoru z Pythonu a Node.js SDK, je zakázáno ověřování SSL.</span><span class="sxs-lookup"><span data-stu-id="e9098-206">When connecting to the emulator from Python and Node.js SDKs, SSL verification is disabled.</span></span>

## <span data-ttu-id="e9098-207"><a id="command-line"></a>Odkaz na nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e9098-207"><a id="command-line"></a>Command-line tool reference</span></span>
<span data-ttu-id="e9098-208">Z umístění instalace můžete na příkazovém řádku spuštění a zastavení emulátoru, nakonfigurujte možnosti a provádění dalších operací.</span><span class="sxs-lookup"><span data-stu-id="e9098-208">From the installation location, you can use the command-line to start and stop the emulator, configure options, and perform other operations.</span></span>

### <a name="command-line-syntax"></a><span data-ttu-id="e9098-209">Syntaxe příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e9098-209">Command-line Syntax</span></span>

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

<span data-ttu-id="e9098-210">Chcete-li zobrazit seznam možností, zadejte `CosmosDB.Emulator.exe /?` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="e9098-210">To view the list of options, type `CosmosDB.Emulator.exe /?` at the command prompt.</span></span>

<table>
<tr>
  <td><span data-ttu-id="e9098-211"><strong>Možnost</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-211"><strong>Option</strong></span></span></td>
  <td><span data-ttu-id="e9098-212"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-212"><strong>Description</strong></span></span></td>
  <td><span data-ttu-id="e9098-213"><strong>Příkaz</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-213"><strong>Command</strong></span></span></td>
  <td><span data-ttu-id="e9098-214"><strong>Argumenty</strong></span><span class="sxs-lookup"><span data-stu-id="e9098-214"><strong>Arguments</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-215">[Žádný argument]</span><span class="sxs-lookup"><span data-stu-id="e9098-215">[No arguments]</span></span></td>
  <td><span data-ttu-id="e9098-216">Spuštění emulátoru Azure Cosmos databáze s výchozím nastavením.</span><span class="sxs-lookup"><span data-stu-id="e9098-216">Starts up the Azure Cosmos DB Emulator with default settings.</span></span></td>
  <td><span data-ttu-id="e9098-217">CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="e9098-217">CosmosDB.Emulator.exe</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-218">[Help]</span><span class="sxs-lookup"><span data-stu-id="e9098-218">[Help]</span></span></td>
  <td><span data-ttu-id="e9098-219">Zobrazí seznam podporovaných argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e9098-219">Displays the list of supported command-line arguments.</span></span></td>
  <td><span data-ttu-id="e9098-220">CosmosDB.Emulator.exe /?</span><span class="sxs-lookup"><span data-stu-id="e9098-220">CosmosDB.Emulator.exe /?</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-221">Vypnutí</span><span class="sxs-lookup"><span data-stu-id="e9098-221">Shutdown</span></span></td>
  <td><span data-ttu-id="e9098-222">Vypne emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-222">Shuts down the Azure Cosmos DB Emulator.</span></span></td>
  <td><span data-ttu-id="e9098-223">/ CosmosDB.Emulator.exe Shutdown</span><span class="sxs-lookup"><span data-stu-id="e9098-223">CosmosDB.Emulator.exe /Shutdown</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-224">DataPath</span><span class="sxs-lookup"><span data-stu-id="e9098-224">DataPath</span></span></td>
  <td><span data-ttu-id="e9098-225">Určuje cestu, do kterého chcete uložit datové soubory.</span><span class="sxs-lookup"><span data-stu-id="e9098-225">Specifies the path in which to store data files.</span></span> <span data-ttu-id="e9098-226">Výchozí hodnota je % LocalAppdata%\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="e9098-226">Default is %LocalAppdata%\CosmosDBEmulator.</span></span></td>
  <td><span data-ttu-id="e9098-227">CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-227">CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</span></span></td>
  <td><span data-ttu-id="e9098-228">&lt;DataPath&gt;: přístupná cesta</span><span class="sxs-lookup"><span data-stu-id="e9098-228">&lt;datapath&gt;: An accessible path</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-229">Port</span><span class="sxs-lookup"><span data-stu-id="e9098-229">Port</span></span></td>
  <td><span data-ttu-id="e9098-230">Určuje číslo portu pro použití pro emulátor.</span><span class="sxs-lookup"><span data-stu-id="e9098-230">Specifies the port number to use for the emulator.</span></span>  <span data-ttu-id="e9098-231">Výchozí hodnota je 8081.</span><span class="sxs-lookup"><span data-stu-id="e9098-231">Default is 8081.</span></span></td>
  <td><span data-ttu-id="e9098-232">/ CosmosDB.Emulator.exe port =&lt;portu&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-232">CosmosDB.Emulator.exe /Port=&lt;port&gt;</span></span></td>
  <td><span data-ttu-id="e9098-233">&lt;port&gt;: jedno číslo portu</span><span class="sxs-lookup"><span data-stu-id="e9098-233">&lt;port&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-234">MongoPort</span><span class="sxs-lookup"><span data-stu-id="e9098-234">MongoPort</span></span></td>
  <td><span data-ttu-id="e9098-235">Udává číslo portu, který chcete použít pro MongoDB kompatibility rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e9098-235">Specifies the port number to use for MongoDB compatibility API.</span></span> <span data-ttu-id="e9098-236">Výchozí hodnota je 10255.</span><span class="sxs-lookup"><span data-stu-id="e9098-236">Default is 10255.</span></span></td>
  <td><span data-ttu-id="e9098-237">CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-237">CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</span></span></td>
  <td><span data-ttu-id="e9098-238">&lt;mongoport&gt;: jedno číslo portu</span><span class="sxs-lookup"><span data-stu-id="e9098-238">&lt;mongoport&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-239">DirectPorts</span><span class="sxs-lookup"><span data-stu-id="e9098-239">DirectPorts</span></span></td>
  <td><span data-ttu-id="e9098-240">Určuje porty, které chcete použít pro přímé připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="e9098-240">Specifies the ports to use for direct connectivity.</span></span> <span data-ttu-id="e9098-241">Výchozí hodnoty jsou 10251,10252,10253,10254.</span><span class="sxs-lookup"><span data-stu-id="e9098-241">Defaults are 10251,10252,10253,10254.</span></span></td>
  <td><span data-ttu-id="e9098-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span></span></td>
  <td><span data-ttu-id="e9098-243">&lt;directports&gt;: seznam s položkami oddělenými čárkou 4 porty</span><span class="sxs-lookup"><span data-stu-id="e9098-243">&lt;directports&gt;: Comma-delimited list of 4 ports</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-244">Klíč</span><span class="sxs-lookup"><span data-stu-id="e9098-244">Key</span></span></td>
  <td><span data-ttu-id="e9098-245">Autorizační klíč pro emulátor.</span><span class="sxs-lookup"><span data-stu-id="e9098-245">Authorization key for the emulator.</span></span> <span data-ttu-id="e9098-246">Klíč musí být kódování base-64 vektoru 64 bajtů.</span><span class="sxs-lookup"><span data-stu-id="e9098-246">Key must be the base-64 encoding of a 64-byte vector.</span></span></td>
  <td><span data-ttu-id="e9098-247">CosmosDB.Emulator.exe /Key:&lt;klíč&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-247">CosmosDB.Emulator.exe /Key:&lt;key&gt;</span></span></td>
  <td><span data-ttu-id="e9098-248">&lt;klíč&gt;: klíč musí být kódování base-64 vektoru 64 bajtů</span><span class="sxs-lookup"><span data-stu-id="e9098-248">&lt;key&gt;: Key must be the base-64 encoding of a 64-byte vector</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-249">EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="e9098-249">EnableRateLimiting</span></span></td>
  <td><span data-ttu-id="e9098-250">Určuje, že rychlost požadavků, omezení chování je povolena.</span><span class="sxs-lookup"><span data-stu-id="e9098-250">Specifies that request rate limiting behavior is enabled.</span></span></td>
  <td><span data-ttu-id="e9098-251">CosmosDB.Emulator.exe /EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="e9098-251">CosmosDB.Emulator.exe /EnableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-252">DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="e9098-252">DisableRateLimiting</span></span></td>
  <td><span data-ttu-id="e9098-253">Určuje, že rychlost požadavků, omezení chování je zakázán.</span><span class="sxs-lookup"><span data-stu-id="e9098-253">Specifies that request rate limiting behavior is disabled.</span></span></td>
  <td><span data-ttu-id="e9098-254">CosmosDB.Emulator.exe /DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="e9098-254">CosmosDB.Emulator.exe /DisableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-255">NoUI</span><span class="sxs-lookup"><span data-stu-id="e9098-255">NoUI</span></span></td>
  <td><span data-ttu-id="e9098-256">Nezobrazovat emulátoru uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e9098-256">Do not show the emulator user interface.</span></span></td>
  <td><span data-ttu-id="e9098-257">/ Noui CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="e9098-257">CosmosDB.Emulator.exe /NoUI</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-258">NoExplorer</span><span class="sxs-lookup"><span data-stu-id="e9098-258">NoExplorer</span></span></td>
  <td><span data-ttu-id="e9098-259">Při spuštění nezobrazovat Průzkumníka dokumentů.</span><span class="sxs-lookup"><span data-stu-id="e9098-259">Don't show document explorer on startup.</span></span></td>
  <td><span data-ttu-id="e9098-260">CosmosDB.Emulator.exe /NoExplorer</span><span class="sxs-lookup"><span data-stu-id="e9098-260">CosmosDB.Emulator.exe /NoExplorer</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-261">PartitionCount</span><span class="sxs-lookup"><span data-stu-id="e9098-261">PartitionCount</span></span></td>
  <td><span data-ttu-id="e9098-262">Určuje maximální počet dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="e9098-262">Specifies the maximum number of partitioned collections.</span></span> <span data-ttu-id="e9098-263">V tématu [změnit počet kolekcí](#set-partitioncount) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e9098-263">See [Change the number of collections](#set-partitioncount) for more information.</span></span></td>
  <td><span data-ttu-id="e9098-264">CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-264">CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</span></span></td>
  <td><span data-ttu-id="e9098-265">&lt;partitioncount&gt;: maximální počet povolených kolekce tvořené jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="e9098-265">&lt;partitioncount&gt;: Maximum number of allowed single partition collections.</span></span> <span data-ttu-id="e9098-266">Výchozí hodnota je 25.</span><span class="sxs-lookup"><span data-stu-id="e9098-266">Default is 25.</span></span> <span data-ttu-id="e9098-267">Maximální povolený počet je 250.</span><span class="sxs-lookup"><span data-stu-id="e9098-267">Maximum allowed is 250.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-268">DefaultPartitionCount</span><span class="sxs-lookup"><span data-stu-id="e9098-268">DefaultPartitionCount</span></span></td>
  <td><span data-ttu-id="e9098-269">Určuje výchozí počet oddílů pro dělenou kolekci.</span><span class="sxs-lookup"><span data-stu-id="e9098-269">Specifies the default number of partitions for a partitioned collection.</span></span></td>
  <td><span data-ttu-id="e9098-270">CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-270">CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</span></span></td>
  <td><span data-ttu-id="e9098-271">&lt;defaultpartitioncount&gt; výchozí hodnota je 25.</span><span class="sxs-lookup"><span data-stu-id="e9098-271">&lt;defaultpartitioncount&gt; Default is 25.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-272">AllowNetworkAccess</span><span class="sxs-lookup"><span data-stu-id="e9098-272">AllowNetworkAccess</span></span></td>
  <td><span data-ttu-id="e9098-273">Umožňuje přístup k emulátoru přes síť.</span><span class="sxs-lookup"><span data-stu-id="e9098-273">Enables access to the emulator over a network.</span></span> <span data-ttu-id="e9098-274">Je třeba předat také /Key =&lt;key_string&gt; nebo/keyfile =&lt;název_souboru&gt; povolit přístup k síti.</span><span class="sxs-lookup"><span data-stu-id="e9098-274">You must also pass /Key=&lt;key_string&gt; or /KeyFile=&lt;file_name&gt; to enable network access.</span></span></td>
  <td><span data-ttu-id="e9098-275">CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-275">CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;</span></span><br><br><span data-ttu-id="e9098-276">nebo</span><span class="sxs-lookup"><span data-stu-id="e9098-276">or</span></span><br><br><span data-ttu-id="e9098-277">/ Keyfile /AllowNetworkAccess CosmosDB.Emulator.exe =&lt;název_souboru&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-277">CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-278">NoFirewall</span><span class="sxs-lookup"><span data-stu-id="e9098-278">NoFirewall</span></span></td>
  <td><span data-ttu-id="e9098-279">Nemáte úprava pravidla brány firewall, když se používá /AllowNetworkAccess.</span><span class="sxs-lookup"><span data-stu-id="e9098-279">Don't adjust firewall rules when /AllowNetworkAccess is used.</span></span></td>
  <td><span data-ttu-id="e9098-280">CosmosDB.Emulator.exe /NoFirewall</span><span class="sxs-lookup"><span data-stu-id="e9098-280">CosmosDB.Emulator.exe /NoFirewall</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-281">GenKeyFile</span><span class="sxs-lookup"><span data-stu-id="e9098-281">GenKeyFile</span></span></td>
  <td><span data-ttu-id="e9098-282">Vygenerovat nový klíč autorizace a uložte do zadaného souboru.</span><span class="sxs-lookup"><span data-stu-id="e9098-282">Generate a new authorization key and save to the specified file.</span></span> <span data-ttu-id="e9098-283">Generovaný klíč lze použít s možností /Key nebo/keyfile.</span><span class="sxs-lookup"><span data-stu-id="e9098-283">The generated key can be used with the /Key or /KeyFile options.</span></span></td>
  <td><span data-ttu-id="e9098-284">CosmosDB.Emulator.exe /GenKeyFile =&lt;cestu k souboru klíče&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-284">CosmosDB.Emulator.exe  /GenKeyFile=&lt;path to key file&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-285">Konzistence</span><span class="sxs-lookup"><span data-stu-id="e9098-285">Consistency</span></span></td>
  <td><span data-ttu-id="e9098-286">Nastavte výchozí úroveň konzistence pro účet.</span><span class="sxs-lookup"><span data-stu-id="e9098-286">Set the default consistency level for the account.</span></span></td>
  <td><span data-ttu-id="e9098-287">CosmosDB.Emulator.exe /Consistency =&lt;konzistence&gt;</span><span class="sxs-lookup"><span data-stu-id="e9098-287">CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</span></span></td>
  <td><span data-ttu-id="e9098-288">&lt;konzistence&gt;: hodnota musí být jeden z následujících [úrovně konzistence](consistency-levels.md): relace silného, Eventual nebo BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="e9098-288">&lt;consistency&gt;: Value must be one of the following [consistency levels](consistency-levels.md): Session, Strong, Eventual, or BoundedStaleness.</span></span>  <span data-ttu-id="e9098-289">Výchozí hodnota je relace.</span><span class="sxs-lookup"><span data-stu-id="e9098-289">The default value is Session.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="e9098-290">?</span><span class="sxs-lookup"><span data-stu-id="e9098-290">?</span></span></td>
  <td><span data-ttu-id="e9098-291">Zobrazit zprávy nápovědy.</span><span class="sxs-lookup"><span data-stu-id="e9098-291">Show the help message.</span></span></td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-the-azure-cosmos-db-emulator-and-azure-cosmos-db"></a><span data-ttu-id="e9098-292">Rozdíly mezi Azure Cosmos DB emulátoru a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e9098-292">Differences between the Azure Cosmos DB Emulator and Azure Cosmos DB</span></span> 
<span data-ttu-id="e9098-293">Protože emulátor DB Cosmos Azure poskytuje emulované prostředí spuštěna na vývojáře místní pracovní stanici, existují určité rozdíly ve funkcích mezi emulátoru a účet Azure Cosmos DB v cloudu:</span><span class="sxs-lookup"><span data-stu-id="e9098-293">Because the Azure Cosmos DB Emulator provides an emulated environment running on a local developer workstation, there are some differences in functionality between the emulator and an Azure Cosmos DB account in the cloud:</span></span>

* <span data-ttu-id="e9098-294">Emulátor DB Cosmos Azure podporuje pouze jeden účet opravené a dobře známé hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="e9098-294">The Azure Cosmos DB Emulator supports only a single fixed account and a well-known master key.</span></span>  <span data-ttu-id="e9098-295">Opětovné generování klíče není možné v emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-295">Key regeneration is not possible in the Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="e9098-296">Emulátor Azure DB Cosmos není škálovatelné služby a nebude podporovat velké množství kolekcí.</span><span class="sxs-lookup"><span data-stu-id="e9098-296">The Azure Cosmos DB Emulator is not a scalable service and will not support a large number of collections.</span></span>
* <span data-ttu-id="e9098-297">Emulátor Azure DB Cosmos není simulovat různé [úrovně konzistence Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="e9098-297">The Azure Cosmos DB Emulator does not simulate different [Azure Cosmos DB consistency levels](consistency-levels.md).</span></span>
* <span data-ttu-id="e9098-298">Emulátor Azure DB Cosmos není simulovat [replikace více oblast](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="e9098-298">The Azure Cosmos DB Emulator does not simulate [multi-region replication](distribute-data-globally.md).</span></span>
* <span data-ttu-id="e9098-299">Emulátor DB Cosmos Azure nepodporuje přepsání kvót služby, které jsou k dispozici ve službě Azure Cosmos DB (např. omezení velikosti dokumentu, dělenou kolekci výraznější úložiště).</span><span class="sxs-lookup"><span data-stu-id="e9098-299">The Azure Cosmos DB Emulator does not support the service quota overrides that are available in the Azure Cosmos DB service (e.g. document size limits, increased partitioned collection storage).</span></span>
* <span data-ttu-id="e9098-300">Jako vaší kopie emulátoru DB Cosmos Azure nemusí být aktuální. nejnovější změny ve službě Azure Cosmos DB, prosím [Plánovač kapacity Azure Cosmos DB](https://www.documentdb.com/capacityplanner) přesně odhadnout provozním potřebám propustnost (ruština) vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9098-300">As your copy of the Azure Cosmos DB Emulator might not be up to date with the most recent changes with the Azure Cosmos DB service, please [Azure Cosmos DB capacity planner](https://www.documentdb.com/capacityplanner) to accurately estimate production throughput (RUs) needs of your application.</span></span>

## <span data-ttu-id="e9098-301"><a id="set-partitioncount"></a>Změnit počet kolekcí</span><span class="sxs-lookup"><span data-stu-id="e9098-301"><a id="set-partitioncount"></a>Change the number of collections</span></span>

<span data-ttu-id="e9098-302">Ve výchozím nastavení je možné vytvořit až 25 kolekce tvořené jedním oddílem, nebo 1 dělenou kolekci pomocí emulátoru Azure DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e9098-302">By default, you can create up to 25 single partition collections, or 1 partitioned collection using the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="e9098-303">Změnou **PartitionCount** hodnotu, je možné vytvořit až 250 kolekce tvořené jedním oddílem nebo 10 dělené kolekce nebo libovolnou kombinaci dva, které není delší než 250 jeden oddíly (kde 1 oddíly kolekce = 25 kolekce tvořené jedním oddílem).</span><span class="sxs-lookup"><span data-stu-id="e9098-303">By modifying the **PartitionCount** value, you can create up to 250 single partition collections or 10 partitioned collections, or any combination of the two that does not exceed 250 single partitions (where 1 partitioned collection = 25 single partition collection).</span></span>

<span data-ttu-id="e9098-304">Pokud se pokusíte vytvořit kolekci po překročení aktuální počet oddílů, emulátoru vyhodí výjimku ServiceUnavailable, s následující zprávou.</span><span class="sxs-lookup"><span data-stu-id="e9098-304">If you attempt to create a collection after the current partition count has been exceeded, the emulator throws a ServiceUnavailable exception, with the following message.</span></span>

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    to bring more and more capacity online, and encourage you to try again. 
    Please do not hesitate to email docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

<span data-ttu-id="e9098-305">Chcete-li změnit počet kolekcí, které jsou k dispozici na emulátoru DB Cosmos Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e9098-305">To change the number of collections available to the Azure Cosmos DB Emulator, do the following:</span></span>

1. <span data-ttu-id="e9098-306">Odstranit všechna místní data emulátoru DB Cosmos Azure kliknutím pravým tlačítkem myši **emulátoru DB Cosmos Azure** ikonu na hlavním panelu a potom kliknutím na **obnovit Data...** .</span><span class="sxs-lookup"><span data-stu-id="e9098-306">Delete all local Azure Cosmos DB Emulator data by right-clicking the **Azure Cosmos DB Emulator** icon on the system tray, and then clicking **Reset Data…**.</span></span>
2. <span data-ttu-id="e9098-307">Odstraňte všechna data emulátoru v této složce C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="e9098-307">Delete all emulator data in this folder C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span></span>
3. <span data-ttu-id="e9098-308">Ukončete všechny otevřené instance kliknutím pravým tlačítkem myši **emulátoru DB Cosmos Azure** ikonu na hlavním panelu a potom kliknutím na **ukončení**.</span><span class="sxs-lookup"><span data-stu-id="e9098-308">Exit all open instances by right-clicking the **Azure Cosmos DB Emulator** icon on the system tray, and then clicking **Exit**.</span></span> <span data-ttu-id="e9098-309">Může trvat několik minut pro všechny instance ukončíte.</span><span class="sxs-lookup"><span data-stu-id="e9098-309">It may take a minute for all instances to exit.</span></span>
4. <span data-ttu-id="e9098-310">Nainstalujte nejnovější verzi [emulátoru DB Cosmos Azure](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="e9098-310">Install the latest version of the [Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator).</span></span>
5. <span data-ttu-id="e9098-311">Spusťte emulátor s příznakem PartitionCount nastavením hodnoty < = 250.</span><span class="sxs-lookup"><span data-stu-id="e9098-311">Launch the emulator with the PartitionCount flag by setting a value <= 250.</span></span> <span data-ttu-id="e9098-312">Například: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span><span class="sxs-lookup"><span data-stu-id="e9098-312">For example: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e9098-313">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="e9098-313">Troubleshooting</span></span>

<span data-ttu-id="e9098-314">Použijte následující tipy k řešení potíží, které zaznamenáte pomocí emulátoru Cosmos databázi Azure:</span><span class="sxs-lookup"><span data-stu-id="e9098-314">Use the following tips to help troubleshoot issues you encounter with the Azure Cosmos DB emulator:</span></span>

- <span data-ttu-id="e9098-315">Pokud jste nainstalovali novou verzi emulátoru a dochází k chybám, zajistěte, aby že resetovat vaše data.</span><span class="sxs-lookup"><span data-stu-id="e9098-315">If you installed a new version of the Emulator and are experiencing errors, ensure you reset your data.</span></span> <span data-ttu-id="e9098-316">Pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure na hlavním panelu a potom kliknutím na Obnovit Data můžete obnovit data...</span><span class="sxs-lookup"><span data-stu-id="e9098-316">You can reset your data by right-clicking the Azure Cosmos DB Emulator icon on the system tray, and then clicking Reset Data….</span></span> <span data-ttu-id="e9098-317">Pokud se chyby nevyřeší, můžete odinstalovat a znovu nainstalovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e9098-317">If that does not fix the errors, you can uninstall and reinstall the app.</span></span> <span data-ttu-id="e9098-318">V tématu [odinstalovat místní emulátoru](#uninstall) pokyny.</span><span class="sxs-lookup"><span data-stu-id="e9098-318">See [Uninstall the local emulator](#uninstall) for instructions.</span></span>

- <span data-ttu-id="e9098-319">Pokud dojde k chybě v Azure Cosmos DB emulátoru, shromažďovat výpis soubory ze složky c:\Users\user_name\AppData\Local\CrashDumps, zkomprimovat a připojte je k e-mailu na [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e9098-319">If the Azure Cosmos DB emulator crashes, collect dump files from c:\Users\user_name\AppData\Local\CrashDumps folder, compress them, and attach them to an email to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="e9098-320">Pokud se setkáváte s havárií v CosmosDB.StartupEntryPoint.exe, spusťte následující příkaz z příkazového řádku správce:`lodctr /R`</span><span class="sxs-lookup"><span data-stu-id="e9098-320">If you experience crashes in CosmosDB.StartupEntryPoint.exe, run the following command from an admin command prompt: `lodctr /R`</span></span> 

- <span data-ttu-id="e9098-321">Pokud narazíte na potíže s připojením [shromažďování trasovacích souborů](#trace-files)zkomprimovat a připojte je k e-mailu na [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e9098-321">If you encounter a connectivity issue, [collect trace files](#trace-files), compress them, and attach them to an email to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="e9098-322">Pokud se zobrazí **služba není k dispozici** zpráva emulátoru může dojít k selhání inicializace sady síťových protokolů.</span><span class="sxs-lookup"><span data-stu-id="e9098-322">If you receive a **Service Unavailable** message, the emulator might be failing to initialize the network stack.</span></span> <span data-ttu-id="e9098-323">Zkontrolujte, zda máte Pulse zabezpečené klienta nebo Juniper sítě nainstalovaným klientem, jako jejich ovladače filtru sítě může dojít k potížím.</span><span class="sxs-lookup"><span data-stu-id="e9098-323">Check to see if you have the Pulse secure client or Juniper networks client installed, as their network filter drivers may cause the problem.</span></span> <span data-ttu-id="e9098-324">Odinstalace ovladače filtru třetích stran sítě obvykle opravy problému.</span><span class="sxs-lookup"><span data-stu-id="e9098-324">Uninstalling third party network filter drivers typically fixes the issue.</span></span>

### <span data-ttu-id="e9098-325"><a id="trace-files"></a>Shromažďování trasovacích souborů</span><span class="sxs-lookup"><span data-stu-id="e9098-325"><a id="trace-files"></a>Collect trace files</span></span>

<span data-ttu-id="e9098-326">Chcete-li shromažďovat trasování ladění, spusťte následující příkazy z příkazového řádku pro správu:</span><span class="sxs-lookup"><span data-stu-id="e9098-326">To collect debugging traces, run the following commands from an administrative command prompt:</span></span>

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. <span data-ttu-id="e9098-327">`CosmosDB.Emulator.exe /shutdown`.</span><span class="sxs-lookup"><span data-stu-id="e9098-327">`CosmosDB.Emulator.exe /shutdown`.</span></span> <span data-ttu-id="e9098-328">Sledování na hlavním panelu a ujistěte se program byl vypnut, může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="e9098-328">Watch the system tray to make sure the program has shut down, it may take a minute.</span></span> <span data-ttu-id="e9098-329">Můžete také stačí kliknout na **ukončení** v uživatelském rozhraní emulátoru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e9098-329">You can also just click **Exit** in the Azure Cosmos DB emulator user interface.</span></span>
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. <span data-ttu-id="e9098-330">Reprodukujte problém.</span><span class="sxs-lookup"><span data-stu-id="e9098-330">Reproduce the problem.</span></span> <span data-ttu-id="e9098-331">Pokud Průzkumníku dat nefunguje, stačí čekat na prohlížeči otevřít pro několik sekund, catch k chybě.</span><span class="sxs-lookup"><span data-stu-id="e9098-331">If Data Explorer is not working, you only need to wait for the browser to open for a few seconds to catch the error.</span></span>
5. `CosmosDB.Emulator.exe /stoptraces`
6. <span data-ttu-id="e9098-332">Přejděte na `%ProgramFiles%\Azure Cosmos DB Emulator` a najít soubor docdbemulator_000001.etl.</span><span class="sxs-lookup"><span data-stu-id="e9098-332">Navigate to `%ProgramFiles%\Azure Cosmos DB Emulator` and find the docdbemulator_000001.etl file.</span></span>
7. <span data-ttu-id="e9098-333">Odeslat soubor .etl spolu se nepodařilo kroky [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) pro ladění.</span><span class="sxs-lookup"><span data-stu-id="e9098-333">Send the .etl file along with repro steps to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) for debugging.</span></span>

### <span data-ttu-id="e9098-334"><a id="uninstall"></a>Odinstalujte místní emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-334"><a id="uninstall"></a>Uninstall the local Emulator</span></span>

1. <span data-ttu-id="e9098-335">Ukončete všechny otevřené instance místní emulátoru pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure na hlavním panelu a potom kliknutím na ukončení.</span><span class="sxs-lookup"><span data-stu-id="e9098-335">Exit all open instances of the local Emulator by right-clicking the Azure Cosmos DB Emulator icon on the system tray, and then clicking Exit.</span></span> <span data-ttu-id="e9098-336">Může trvat několik minut pro všechny instance ukončíte.</span><span class="sxs-lookup"><span data-stu-id="e9098-336">It may take a minute for all instances to exit.</span></span>
2. <span data-ttu-id="e9098-337">Do vyhledávacího pole Windows, zadejte **aplikace a funkce** a klikněte na **aplikace a funkce (nastavení systému)** výsledek.</span><span class="sxs-lookup"><span data-stu-id="e9098-337">In the Windows search box, type **Apps & features** and click on the **Apps & features (System settings)** result.</span></span>
3. <span data-ttu-id="e9098-338">V seznamu aplikací, posuňte **emulátoru DB Cosmos Azure**, vyberte ho, klikněte na **odinstalace**, potvrďte a klikněte na **odinstalovat** znovu.</span><span class="sxs-lookup"><span data-stu-id="e9098-338">In the list of apps, scroll to **Azure Cosmos DB Emulator**, select it, click **Uninstall**, then confirm and click **Uninstall** again.</span></span>
4. <span data-ttu-id="e9098-339">Při odinstalaci aplikace, přejděte na C:\Users\<uživatele > \AppData\Local\CosmosDBEmulator a odstranit složku.</span><span class="sxs-lookup"><span data-stu-id="e9098-339">When the app is uninstalled, navigate to C:\Users\<user>\AppData\Local\CosmosDBEmulator and delete the folder.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e9098-340">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9098-340">Next steps</span></span>

<span data-ttu-id="e9098-341">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="e9098-341">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e9098-342">Nainstalována na místní emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-342">Installed the local Emulator</span></span>
> * <span data-ttu-id="e9098-343">Rand – emulátoru na Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="e9098-343">Rand the Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="e9098-344">Ověřené žádosti</span><span class="sxs-lookup"><span data-stu-id="e9098-344">Authenticated requests</span></span>
> * <span data-ttu-id="e9098-345">Použít Průzkumníku dat v emulátoru</span><span class="sxs-lookup"><span data-stu-id="e9098-345">Used the Data Explorer in the Emulator</span></span>
> * <span data-ttu-id="e9098-346">Exportovaný certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="e9098-346">Exported SSL certificates</span></span>
> * <span data-ttu-id="e9098-347">Volat emulátoru z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e9098-347">Called the Emulator from the command line</span></span>
> * <span data-ttu-id="e9098-348">Shromážděné trasovací soubory</span><span class="sxs-lookup"><span data-stu-id="e9098-348">Collected trace files</span></span>

<span data-ttu-id="e9098-349">V tomto kurzu jste zjistili, jak používat místní emulátor pro volné místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="e9098-349">In this tutorial, you've learned how to use the local Emulator for free local development.</span></span> <span data-ttu-id="e9098-350">Teď můžete pokračovat v dalším kurzu a zjistěte, jak k exportu certifikátů SSL emulátor.</span><span class="sxs-lookup"><span data-stu-id="e9098-350">You can now proceed to the next tutorial and learn how to export Emulator SSL certificates.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9098-351">Exportu certifikátů emulátoru DB Cosmos Azure</span><span class="sxs-lookup"><span data-stu-id="e9098-351">Export the Azure Cosmos DB Emulator certificates</span></span>](local-emulator-export-ssl-certificates.md)
