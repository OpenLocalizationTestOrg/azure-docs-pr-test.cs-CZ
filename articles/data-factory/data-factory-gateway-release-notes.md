---
title: "Poznámky k verzi pro Brána pro správu dat | Microsoft Docs"
description: "Brána pro správu dat, text poznámky k verzi"
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: abnarain
published: True
ms.openlocfilehash: c052d7e9f757164429ce867201b96305e405dce9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="release-notes-for-data-management-gateway"></a><span data-ttu-id="cf8f7-103">Poznámky k verzi pro Bránu pro správu dat</span><span class="sxs-lookup"><span data-stu-id="cf8f7-103">Release notes for Data Management Gateway</span></span>
<span data-ttu-id="cf8f7-104">Jedním z problémů pro integraci moderní dat je přesun dat do a z místního do cloudu.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-104">One of the challenges for modern data integration is to move data to and from on-premises to cloud.</span></span> <span data-ttu-id="cf8f7-105">Objekt pro vytváření dat umožňuje integraci se Brána pro správu dat, což je místní povolit hybridní přesun dat můžete nainstalovat agenta.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-105">Data Factory makes this integration with Data Management Gateway, which is an agent that you can install on-premises to enable hybrid data movement.</span></span>

<span data-ttu-id="cf8f7-106">Najdete podrobné informace o Brána pro správu dat a způsobu jeho použití v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="cf8f7-106">See the following articles for detailed information about Data Management Gateway and how to use it:</span></span>

*  [<span data-ttu-id="cf8f7-107">Brána správy dat</span><span class="sxs-lookup"><span data-stu-id="cf8f7-107">Data Management Gateway</span></span>](data-factory-data-management-gateway.md)
*  [<span data-ttu-id="cf8f7-108">Přesun dat mezi místními a cloudu pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cf8f7-108">Move data between on-premises and cloud using Azure Data Factory</span></span>](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a><span data-ttu-id="cf8f7-109">AKTUÁLNÍ VERZE (2.10.6347.7)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-109">CURRENT VERSION (2.10.6347.7)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="cf8f7-110">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="cf8f7-110">Enhancements-</span></span>
- <span data-ttu-id="cf8f7-111">Přidáním položky DNS do seznamu povolených IP adres služby service bus, místo vytvoření seznamu povolených všechny Azure IP adresy z brány firewall (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="cf8f7-111">You can add DNS entries to whitelist service bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="cf8f7-112">Odpovídající položka DNS můžete najít na portálu Azure ('Vytvořit a nasadit' -> objekt pro vytváření dat -> 'Brány' -> "serviceUrls" (ve formátu JSON)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-112">You can find respective DNS entry on Azure portal (Data Factory -> ‘Author and Deploy’ -> ‘Gateways’ -> "serviceUrls" (in JSON)</span></span>
- <span data-ttu-id="cf8f7-113">Konektor HDFS nyní podporuje veřejný certifikát podepsaný svým držitelem tím, že umožňuje přeskočit ověřování SSL.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-113">HDFS connector now supports self-signed public certificate by letting you skip SSL validation.</span></span>
- <span data-ttu-id="cf8f7-114">Opravené: Problém s bránou offline během aktualizace (z důvodu posun hodin)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-114">Fixed: Issue with gateway offline during update (due to clock skew)</span></span>



## <a name="earlier-versions"></a><span data-ttu-id="cf8f7-115">Starší verze</span><span class="sxs-lookup"><span data-stu-id="cf8f7-115">Earlier versions</span></span>

## <a name="2963132"></a><span data-ttu-id="cf8f7-116">2.9.6313.2</span><span class="sxs-lookup"><span data-stu-id="cf8f7-116">2.9.6313.2</span></span>
### <a name="enhancements-"></a><span data-ttu-id="cf8f7-117">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="cf8f7-117">Enhancements-</span></span>
-   <span data-ttu-id="cf8f7-118">Můžete přidat záznamy DNS na seznam povolených adres služby Service Bus, místo vytvoření seznamu povolených všechny Azure IP adresy z brány firewall (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="cf8f7-118">You can add DNS entries to whitelist Service Bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="cf8f7-119">Další informace v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-119">More details here.</span></span>
-   <span data-ttu-id="cf8f7-120">Můžete teď kopírování dat z jeden blok objektů blob až 4.75 TB, což je maximální podporovaná velikost objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-120">You can now copy data to/from a single block blob up to 4.75 TB, which is the max supported size of block blob.</span></span> <span data-ttu-id="cf8f7-121">(dřívější limit byl 195 GB).</span><span class="sxs-lookup"><span data-stu-id="cf8f7-121">(earlier limit was 195 GB).</span></span>
-   <span data-ttu-id="cf8f7-122">Opravené: Nedostatek paměti problém při rozbalení několik malých souborů během aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-122">Fixed: Out of memory issue while unzipping several small files during copy activity.</span></span>
-   <span data-ttu-id="cf8f7-123">Opravené: Index je mimo rozsah problém při kopírování z databáze dokumentu aplikace idempotenci funkce na místní server SQL.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-123">Fixed: Index out of range issue while copying from Document DB to an on-premises SQL Server with idempotency feature.</span></span>
-   <span data-ttu-id="cf8f7-124">Opravené: Skript pro vyčištění SQL nefunguje s místní SQL Server z Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-124">Fixed: SQL cleanup script doesn't work with on-premises SQL Server from Copy Wizard.</span></span>
-   <span data-ttu-id="cf8f7-125">Opravené: Název sloupce s místem na konci nefunguje v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-125">Fixed: Column name with space at the end does not work in copy activity.</span></span>

## <a name="28662833"></a><span data-ttu-id="cf8f7-126">2.8.66283.3</span><span class="sxs-lookup"><span data-stu-id="cf8f7-126">2.8.66283.3</span></span>
### <a name="enhancements-"></a><span data-ttu-id="cf8f7-127">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="cf8f7-127">Enhancements-</span></span>
- <span data-ttu-id="cf8f7-128">Opravené: Problém s chybějícími přihlašovací údaje u restartování počítače brány.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-128">Fixed: Issue with missing credentials on gateway machine reboot.</span></span>
- <span data-ttu-id="cf8f7-129">Opravené: Problém s registrací během brány obnovit záložního souboru.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-129">Fixed: Issue with registration during gateway restore using a backup file.</span></span>


## <a name="2762401"></a><span data-ttu-id="cf8f7-130">2.7.6240.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-130">2.7.6240.1</span></span>
### <a name="enhancements-"></a><span data-ttu-id="cf8f7-131">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="cf8f7-131">Enhancements-</span></span>
- <span data-ttu-id="cf8f7-132">Opravené: Nesprávné čtení desetinnou hodnotu null z databáze Oracle jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-132">Fixed: Incorrect read of Decimal null value from Oracle as source.</span></span>

## <a name="2661922"></a><span data-ttu-id="cf8f7-133">2.6.6192.2</span><span class="sxs-lookup"><span data-stu-id="cf8f7-133">2.6.6192.2</span></span>
### <a name="whats-new"></a><span data-ttu-id="cf8f7-134">Co je nového</span><span class="sxs-lookup"><span data-stu-id="cf8f7-134">What’s new</span></span>
- <span data-ttu-id="cf8f7-135">Zákazníci můžete poskytnout zpětnou vazbu na bránu zaregistrujete činnost.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-135">Customers can provide feedback on gateway registering experience.</span></span>
- <span data-ttu-id="cf8f7-136">Podpora nový formát komprese: ZIP (Deflate)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-136">Support a new compression format: ZIP (Deflate)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="cf8f7-137">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="cf8f7-137">Enhancements-</span></span>
- <span data-ttu-id="cf8f7-138">Zlepšení výkonu pro Oracle jímky, HDFS zdroje.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-138">Performance improvement for Oracle Sink, HDFS source.</span></span>
- <span data-ttu-id="cf8f7-139">Oprava chyby pro brány automatické aktualizace, gateway paralelní zpracování kapacity.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-139">Bug fix for gateway auto update, gateway parallel processing capacity.</span></span>


## <a name="2561641"></a><span data-ttu-id="cf8f7-140">2.5.6164.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-140">2.5.6164.1</span></span>
### <a name="enhancements"></a><span data-ttu-id="cf8f7-141">Vylepšení</span><span class="sxs-lookup"><span data-stu-id="cf8f7-141">Enhancements</span></span>
- <span data-ttu-id="cf8f7-142">Vylepšené a robustnější brány registrace zkušeností – teď můžete sledovat průběh během procesu registrace brány, který umožňuje registraci prostředí rychleji reagovat.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-142">Improved and more robust Gateway registration experience- Now you can track progress status during the Gateway registration process, which makes the registration experience more responsive.</span></span>
- <span data-ttu-id="cf8f7-143">Zlepšení brány obnovit proces - je stále možné obnovit bránu i v případě, že nemáte brány záložní soubor s touto aktualizací.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-143">Improvement in Gateway Restore Process- You can still recover gateway even if you do not have the gateway backup file with this update.</span></span> <span data-ttu-id="cf8f7-144">To bude vyžadovat jste se resetovat přihlašovací údaje propojené služby v portálu.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-144">This would require you to reset Linked Service credentials in Portal.</span></span>
- <span data-ttu-id="cf8f7-145">Oprava chyby.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-145">Bug fix.</span></span>

## <a name="2461511"></a><span data-ttu-id="cf8f7-146">2.4.6151.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-146">2.4.6151.1</span></span>

### <a name="whats-new"></a><span data-ttu-id="cf8f7-147">Co je nového</span><span class="sxs-lookup"><span data-stu-id="cf8f7-147">What’s new</span></span>

- <span data-ttu-id="cf8f7-148">Nyní můžete ukládat přihlašovací údaje zdroje dat místně.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-148">You can now store data source credentials locally.</span></span> <span data-ttu-id="cf8f7-149">Přihlašovací údaje jsou šifrované.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-149">The credentials are encrypted.</span></span> <span data-ttu-id="cf8f7-150">Pověření ke zdroji dat lze obnovit a obnovit pomocí záložní soubor, který se dá vyexportovat ze existující bránu, všechny místní.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-150">The data source credentials can be recovered and restored using the backup file that can be exported from the existing Gateway, all on-premises.</span></span>

### <a name="enhancements-"></a><span data-ttu-id="cf8f7-151">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="cf8f7-151">Enhancements-</span></span>

- <span data-ttu-id="cf8f7-152">Vylepšené a robustnější brány registrace prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-152">Improved and more robust Gateway registration experience.</span></span>
- <span data-ttu-id="cf8f7-153">Podpora automatického zjišťování QuoteChar konfigurace formátu textu v Průvodce kopírováním a zlepšit celkové přesnost detekce formátu.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-153">Support auto detection of QuoteChar configuration for Text format in copy wizard, and improve the overall format detection accuracy.</span></span>

## <a name="2361002"></a><span data-ttu-id="cf8f7-154">2.3.6100.2</span><span class="sxs-lookup"><span data-stu-id="cf8f7-154">2.3.6100.2</span></span>

- <span data-ttu-id="cf8f7-155">Podpora firstRowAsHeader a SkipLineCount automatické zjišťování v Průvodce kopírováním textových souborů v systému souborů na místě a HDFS.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-155">Support firstRowAsHeader and SkipLineCount auto detection in copy wizard for text files in on-premises File system and HDFS.</span></span>
- <span data-ttu-id="cf8f7-156">Zvýšení stability síťové připojení mezi bránou a Service Bus</span><span class="sxs-lookup"><span data-stu-id="cf8f7-156">Enhance the stability of network connection between gateway and Service Bus</span></span>
- <span data-ttu-id="cf8f7-157">Několik oprav chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-157">A few bug fixes</span></span>


## <a name="2260721"></a><span data-ttu-id="cf8f7-158">2.2.6072.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-158">2.2.6072.1</span></span>

*  <span data-ttu-id="cf8f7-159">Podporuje nastavení proxy serveru HTTP pro bránu pomocí Správce konfigurace brány.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-159">Supports setting HTTP proxy for the gateway using the Gateway Configuration Manager.</span></span> <span data-ttu-id="cf8f7-160">Pokud nakonfigurovaný, objektů Blob v Azure, Azure Table, Azure Data Lake a Documentdb jsou přístupné prostřednictvím proxy serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-160">If configured, Azure Blob, Azure Table, Azure Data Lake, and Document DB are accessed through HTTP proxy.</span></span>
*  <span data-ttu-id="cf8f7-161">Místní systém souborů podporuje hlavička zpracování TextFormat při kopírování dat z/do objektu Blob Azure, Azure Data Lake Store a místní HDFS.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-161">Supports header handling for TextFormat when copying data from/to Azure Blob, Azure Data Lake Store, on-premises File System, and on-premises HDFS.</span></span>
*  <span data-ttu-id="cf8f7-162">Podporuje kopírování dat z objektu Blob připojit a objektů Blob stránky spolu s již podporované objekt Blob bloku.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-162">Supports copying data from Append Blob and Page Blob along with the already supported Block Blob.</span></span>
*  <span data-ttu-id="cf8f7-163">Zavádí nový stav brány **Online (s omezením)**, což naznačuje, že hlavní funkce brány funguje s výjimkou interaktivní operace podporu pro Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-163">Introduces a new gateway status **Online (Limited)**, which indicates that the main functionality of the gateway works except the interactive operation support for Copy Wizard.</span></span>
*  <span data-ttu-id="cf8f7-164">Zvyšuje odolnost registrace brány pomocí registračního klíče.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-164">Enhances the robustness of gateway registration using registration key.</span></span>

## <a name="216040"></a><span data-ttu-id="cf8f7-165">2.1.6040.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-165">2.1.6040.</span></span>

*  <span data-ttu-id="cf8f7-166">Ovladač DB2 je nyní obsažena v instalační balíček brány.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-166">DB2 driver is included in the gateway installation package now.</span></span> <span data-ttu-id="cf8f7-167">Není nutné instalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-167">You do not need to install it separately.</span></span>
*  <span data-ttu-id="cf8f7-168">Teď podporuje DB2 ovladače z/OS a DB2 pro i (AS / 400) společně s již podporované platformy (Linux, Unix a Windows).</span><span class="sxs-lookup"><span data-stu-id="cf8f7-168">DB2 driver now supports z/OS and DB2 for i (AS/400) along with the platforms already supported (Linux, Unix, and Windows).</span></span>
*  <span data-ttu-id="cf8f7-169">Podporuje používání Azure Cosmos DB jako zdrojový nebo cílový pro místní úložiště dat</span><span class="sxs-lookup"><span data-stu-id="cf8f7-169">Supports using Azure Cosmos DB as a source or destination for on-premises data stores</span></span>
*  <span data-ttu-id="cf8f7-170">Podporuje kopírování dat z/do za provozu nebo studený objekt blob úložiště společně s účet již podporované úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-170">Supports copying data from/to cold/hot blob storage along with the already supported general-purpose storage account.</span></span>
*  <span data-ttu-id="cf8f7-171">Umožňuje připojení k místnímu SQL serveru prostřednictvím brány s oprávněními vzdálené přihlášení.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-171">Allows you to connect to on-premises SQL Server via gateway with remote login privileges.</span></span>  

## <a name="2060131"></a><span data-ttu-id="cf8f7-172">2.0.6013.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-172">2.0.6013.1</span></span>

*  <span data-ttu-id="cf8f7-173">Můžete vybrat jazyk nebo jazykovou verzi, která během ruční instalace použít bránu.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-173">You can select the language/culture to be used by a gateway during manual installation.</span></span>

*  <span data-ttu-id="cf8f7-174">Pokud brána nebude fungovat podle očekávání, můžete odeslat protokoly gateway posledních sedmi dnů společnosti Microsoft, které usnadňují řešení potíží s problému.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-174">When gateway does not work as expected, you can choose to send gateway logs of last seven days to Microsoft to facilitate troubleshooting of the issue.</span></span> <span data-ttu-id="cf8f7-175">Pokud brána není připojená ke cloudové službě, můžete uložit a archivovat protokoly gateway.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-175">If gateway is not connected to the cloud service, you can choose to save and archive gateway logs.</span></span>  

*  <span data-ttu-id="cf8f7-176">Vylepšení uživatelského rozhraní pro správce konfigurace brány:</span><span class="sxs-lookup"><span data-stu-id="cf8f7-176">User interface improvements for gateway configuration manager:</span></span>

    *  <span data-ttu-id="cf8f7-177">Nastavit stav brány. na kartě Domů více viditelné.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-177">Make gateway status more visible on the Home tab.</span></span>

    *  <span data-ttu-id="cf8f7-178">Ovládací prvky reorganizovány všechny a jednodušší.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-178">Reorganized and simplified controls.</span></span>

    *  <span data-ttu-id="cf8f7-179">Může kopírovat data z úložiště pomocí [nástroj preview bez kódu kopírování](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="cf8f7-179">You can copy data from a storage using the [code-free copy preview tool](data-factory-copy-data-wizard-tutorial.md).</span></span> <span data-ttu-id="cf8f7-180">V tématu [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) obecné podrobnosti o této funkci.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-180">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details about this feature in general.</span></span>
*  <span data-ttu-id="cf8f7-181">Brána pro správu dat na vstupní data přímo z místní databáze systému SQL Server můžete do Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-181">You can use Data Management Gateway to ingress data directly from an on-premises SQL Server database into Azure Machine Learning.</span></span>

*  <span data-ttu-id="cf8f7-182">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-182">Performance improvements</span></span>

    * <span data-ttu-id="cf8f7-183">Zlepšení výkonu na zobrazení schématu nebo Preview proti systému SQL Server v nástroji preview bez kódu kopírování.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-183">Improve performance on viewing Schema/Preview against SQL Server in code-free copy preview tool.</span></span>

## <a name="11259531"></a><span data-ttu-id="cf8f7-184">1.12.5953.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-184">1.12.5953.1</span></span>

*  <span data-ttu-id="cf8f7-185">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-185">Bug fixes</span></span>

## <a name="11159181"></a><span data-ttu-id="cf8f7-186">1.11.5918.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-186">1.11.5918.1</span></span>

*  <span data-ttu-id="cf8f7-187">Maximální velikost protokolu událostí brány byla zvýšena z 1 MB na 40 MB.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-187">Maximum size of the gateway event log has been increased from 1 MB to 40 MB.</span></span>

*  <span data-ttu-id="cf8f7-188">V případě, že během automatickou aktualizaci brány je zapotřebí restartování, zobrazí se dialog s upozorněním.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-188">A warning dialog is displayed in case a restart is needed during gateway auto-update.</span></span> <span data-ttu-id="cf8f7-189">Je možné restartovat právo pak nebo novější.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-189">You can choose to restart right then or later.</span></span>

*  <span data-ttu-id="cf8f7-190">V případě, že automatické aktualizace nezdaří, instalační program brány opakuje, automatické aktualizace třikrát za maximální.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-190">In case auto-update fails, gateway installer retries auto-updating three times at maximum.</span></span>

*  <span data-ttu-id="cf8f7-191">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-191">Performance improvements</span></span>

    * <span data-ttu-id="cf8f7-192">Zlepšení výkonu pro načítání velké tabulky z místního serveru ve scénáři bez kódu kopírování.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-192">Improve performance for loading large tables from on-premises server in code-free copy scenario.</span></span>

*  <span data-ttu-id="cf8f7-193">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-193">Bug fixes</span></span>

## <a name="11058921"></a><span data-ttu-id="cf8f7-194">1.10.5892.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-194">1.10.5892.1</span></span>

*  <span data-ttu-id="cf8f7-195">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-195">Performance improvements</span></span>

*  <span data-ttu-id="cf8f7-196">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-196">Bug fixes</span></span>

## <a name="1958652"></a><span data-ttu-id="cf8f7-197">1.9.5865.2</span><span class="sxs-lookup"><span data-stu-id="cf8f7-197">1.9.5865.2</span></span>

*  <span data-ttu-id="cf8f7-198">Nulové funkcí touch automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="cf8f7-198">Zero touch auto update capability</span></span>
*  <span data-ttu-id="cf8f7-199">Ikona nový panelu s indikátory stavu brány</span><span class="sxs-lookup"><span data-stu-id="cf8f7-199">New tray icon with gateway status indicators</span></span>
*  <span data-ttu-id="cf8f7-200">Možnost "Teď Update" z klienta</span><span class="sxs-lookup"><span data-stu-id="cf8f7-200">Ability to “Update now” from the client</span></span>
*  <span data-ttu-id="cf8f7-201">Možnost nastavit čas plánu aktualizace</span><span class="sxs-lookup"><span data-stu-id="cf8f7-201">Ability to set update schedule time</span></span>
*  <span data-ttu-id="cf8f7-202">Skript prostředí PowerShell pro přepnutím zapnout nebo vypnout automatickou aktualizaci</span><span class="sxs-lookup"><span data-stu-id="cf8f7-202">PowerShell script for toggling auto-update on/off</span></span>
*  <span data-ttu-id="cf8f7-203">Podpora formátu JSON</span><span class="sxs-lookup"><span data-stu-id="cf8f7-203">Support for JSON format</span></span>  
*  <span data-ttu-id="cf8f7-204">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-204">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-205">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-205">Bug fixes</span></span>

## <a name="1858221"></a><span data-ttu-id="cf8f7-206">1.8.5822.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-206">1.8.5822.1</span></span>

*  <span data-ttu-id="cf8f7-207">Lepší řešení potíží</span><span class="sxs-lookup"><span data-stu-id="cf8f7-207">Improve troubleshooting experience</span></span>
*  <span data-ttu-id="cf8f7-208">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-208">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-209">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-209">Bug fixes</span></span>

### <a name="1757951"></a><span data-ttu-id="cf8f7-210">1.7.5795.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-210">1.7.5795.1</span></span>

*  <span data-ttu-id="cf8f7-211">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-211">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-212">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-212">Bug fixes</span></span>

### <a name="1757641"></a><span data-ttu-id="cf8f7-213">1.7.5764.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-213">1.7.5764.1</span></span>

*  <span data-ttu-id="cf8f7-214">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-214">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-215">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-215">Bug fixes</span></span>

### <a name="1657351"></a><span data-ttu-id="cf8f7-216">1.6.5735.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-216">1.6.5735.1</span></span>

*  <span data-ttu-id="cf8f7-217">Podpora místní HDFS zdroj/jímka</span><span class="sxs-lookup"><span data-stu-id="cf8f7-217">Support on-premises HDFS Source/Sink</span></span>
*  <span data-ttu-id="cf8f7-218">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-218">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-219">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-219">Bug fixes</span></span>

### <a name="1656961"></a><span data-ttu-id="cf8f7-220">1.6.5696.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-220">1.6.5696.1</span></span>

*  <span data-ttu-id="cf8f7-221">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-221">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-222">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-222">Bug fixes</span></span>

### <a name="1656761"></a><span data-ttu-id="cf8f7-223">1.6.5676.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-223">1.6.5676.1</span></span>

*  <span data-ttu-id="cf8f7-224">Diagnostické nástroje podpory na Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="cf8f7-224">Support diagnostic tools on Configuration Manager</span></span>
*  <span data-ttu-id="cf8f7-225">Podpora sloupců tabulky pro tabulkové zdroje dat pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cf8f7-225">Support table columns for tabular data sources for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-226">Podpora pro vytváření dat Azure SQL DW</span><span class="sxs-lookup"><span data-stu-id="cf8f7-226">Support SQL DW for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-227">Podpora pro vytváření dat Azure Reclusive v BlobSource a FileSource</span><span class="sxs-lookup"><span data-stu-id="cf8f7-227">Support Reclusive in BlobSource and FileSource for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-228">Podpora CopyBehavior – MergeFiles, PreserveHierarchy a FlattenHierarchy v BlobSink a FileSink s binární kopie pro datovou továrnu Azure</span><span class="sxs-lookup"><span data-stu-id="cf8f7-228">Support CopyBehavior – MergeFiles, PreserveHierarchy, and FlattenHierarchy in BlobSink and FileSink with Binary Copy for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-229">Podpora aktivity kopírování hlášení průběhu pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cf8f7-229">Support Copy Activity reporting progress for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-230">Ověření připojení zdroje dat podporu pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="cf8f7-230">Support Data Source Connectivity Validation for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-231">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-231">Bug fixes</span></span>

### <a name="1656721"></a><span data-ttu-id="cf8f7-232">1.6.5672.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-232">1.6.5672.1</span></span>

*  <span data-ttu-id="cf8f7-233">Podpora pro Azure Data Factory název tabulky zdroje dat ODBC</span><span class="sxs-lookup"><span data-stu-id="cf8f7-233">Support table name for ODBC data source for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-234">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-234">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-235">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-235">Bug fixes</span></span>

### <a name="1656581"></a><span data-ttu-id="cf8f7-236">1.6.5658.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-236">1.6.5658.1</span></span>

*  <span data-ttu-id="cf8f7-237">Podpora souboru jímky pro datovou továrnu Azure</span><span class="sxs-lookup"><span data-stu-id="cf8f7-237">Support File Sink for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-238">Podpora pro Azure Data Factory zachování hierarchie v binární kopie</span><span class="sxs-lookup"><span data-stu-id="cf8f7-238">Support preserving hierarchy in binary copy for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-239">Podpora idempotenci aktivity kopírování pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="cf8f7-239">Support Copy Activity Idempotency for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-240">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-240">Bug fixes</span></span>

### <a name="1656401"></a><span data-ttu-id="cf8f7-241">1.6.5640.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-241">1.6.5640.1</span></span>

*  <span data-ttu-id="cf8f7-242">Podpora 3 Další zdroje dat pro Azure Data Factory (rozhraní ODBC, OData, HDFS)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-242">Support 3 more data sources for Azure Data Factory (ODBC, OData, HDFS)</span></span>
*  <span data-ttu-id="cf8f7-243">Podpora znak uvozovky csv analyzátoru pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cf8f7-243">Support quote character in csv parser for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-244">Podpora komprese (BZip2)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-244">Compression support (BZip2)</span></span>
*  <span data-ttu-id="cf8f7-245">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-245">Bug fixes</span></span>

### <a name="1556121"></a><span data-ttu-id="cf8f7-246">1.5.5612.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-246">1.5.5612.1</span></span>

*  <span data-ttu-id="cf8f7-247">Podpora pět relačních databází pro Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata a Sybase)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-247">Support five relational databases for Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata, and Sybase)</span></span>
*  <span data-ttu-id="cf8f7-248">Podpora komprese (Gzip a Deflate)</span><span class="sxs-lookup"><span data-stu-id="cf8f7-248">Compression support (Gzip and Deflate)</span></span>
*  <span data-ttu-id="cf8f7-249">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-249">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-250">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-250">Bug fixes</span></span>

### <a name="1455491"></a><span data-ttu-id="cf8f7-251">1.4.5549.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-251">1.4.5549.1</span></span>

*  <span data-ttu-id="cf8f7-252">Přidání podpory zdroje dat Oracle pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cf8f7-252">Add Oracle data source support for Azure Data Factory</span></span>
*  <span data-ttu-id="cf8f7-253">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="cf8f7-253">Performance improvements</span></span>
*  <span data-ttu-id="cf8f7-254">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="cf8f7-254">Bug fixes</span></span>

### <a name="1454921"></a><span data-ttu-id="cf8f7-255">1.4.5492.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-255">1.4.5492.1</span></span>

*  <span data-ttu-id="cf8f7-256">Jednotná binární soubor, který podporuje služby Microsoft Azure Data Factory a Office 365 Power BI</span><span class="sxs-lookup"><span data-stu-id="cf8f7-256">Unified binary that supports both Microsoft Azure Data Factory and Office 365 Power BI services</span></span>
*  <span data-ttu-id="cf8f7-257">Upřesnit proces uživatelské rozhraní konfigurace a registrace</span><span class="sxs-lookup"><span data-stu-id="cf8f7-257">Refine the Configuration UI and registration process</span></span>
*  <span data-ttu-id="cf8f7-258">Azure Data Factory – Azure příchozí a odchozí podporu pro zdroj dat systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="cf8f7-258">Azure Data Factory – Azure Ingress and Egress support for SQL Server data source</span></span>

### <a name="1253031"></a><span data-ttu-id="cf8f7-259">1.2.5303.1</span><span class="sxs-lookup"><span data-stu-id="cf8f7-259">1.2.5303.1</span></span>

*  <span data-ttu-id="cf8f7-260">Opravte problém časový limit pro podporu více připojení ke zdroji dat časově náročná.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-260">Fix timeout issue to support more time-consuming data source connections.</span></span>

### <a name="1155268"></a><span data-ttu-id="cf8f7-261">1.1.5526.8</span><span class="sxs-lookup"><span data-stu-id="cf8f7-261">1.1.5526.8</span></span>

*  <span data-ttu-id="cf8f7-262">Vyžaduje rozhraní .NET Framework 4.5.1 předpokladem je během instalace.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-262">Requires .NET Framework 4.5.1 as a prerequisite during setup.</span></span>

### <a name="1051442"></a><span data-ttu-id="cf8f7-263">1.0.5144.2</span><span class="sxs-lookup"><span data-stu-id="cf8f7-263">1.0.5144.2</span></span>

*  <span data-ttu-id="cf8f7-264">Žádné změny, které ovlivňují scénáře Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="cf8f7-264">No changes that affect Azure Data Factory scenarios.</span></span>
