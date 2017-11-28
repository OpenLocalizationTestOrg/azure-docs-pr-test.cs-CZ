---
title: "Poznámky k aaaRelease pro Brána pro správu dat | Microsoft Docs"
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
ms.openlocfilehash: 3165d7537410a0531e0bb7f7fe584767f9155574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-data-management-gateway"></a><span data-ttu-id="b0c8d-103">Poznámky k verzi pro Bránu pro správu dat</span><span class="sxs-lookup"><span data-stu-id="b0c8d-103">Release notes for Data Management Gateway</span></span>
<span data-ttu-id="b0c8d-104">Jedním z hello výzvy pro integraci moderní dat je toomove tooand data z místně toocloud.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-104">One of hello challenges for modern data integration is toomove data tooand from on-premises toocloud.</span></span> <span data-ttu-id="b0c8d-105">Objekt pro vytváření dat umožňuje integraci se Brána pro správu dat, což je agenta můžete nainstalovat přesun dat hybridní tooenable místně.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-105">Data Factory makes this integration with Data Management Gateway, which is an agent that you can install on-premises tooenable hybrid data movement.</span></span>

<span data-ttu-id="b0c8d-106">V tématu hello následující články podrobné informace o Brána pro správu dat a jak toouse ho:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-106">See hello following articles for detailed information about Data Management Gateway and how toouse it:</span></span>

*  [<span data-ttu-id="b0c8d-107">Brána správy dat</span><span class="sxs-lookup"><span data-stu-id="b0c8d-107">Data Management Gateway</span></span>](data-factory-data-management-gateway.md)
*  [<span data-ttu-id="b0c8d-108">Přesun dat mezi místními a cloudu pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b0c8d-108">Move data between on-premises and cloud using Azure Data Factory</span></span>](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a><span data-ttu-id="b0c8d-109">AKTUÁLNÍ VERZE (2.10.6347.7)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-109">CURRENT VERSION (2.10.6347.7)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="b0c8d-110">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="b0c8d-110">Enhancements-</span></span>
- <span data-ttu-id="b0c8d-111">Můžete přidat DNS položky toowhitelist služby service bus, místo vytvoření seznamu povolených všechny Azure IP adresy z brány firewall (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-111">You can add DNS entries toowhitelist service bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="b0c8d-112">Odpovídající položka DNS můžete najít na portálu Azure ('Vytvořit a nasadit' -> objekt pro vytváření dat -> 'Brány' -> "serviceUrls" (ve formátu JSON)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-112">You can find respective DNS entry on Azure portal (Data Factory -> ‘Author and Deploy’ -> ‘Gateways’ -> "serviceUrls" (in JSON)</span></span>
- <span data-ttu-id="b0c8d-113">Konektor HDFS nyní podporuje veřejný certifikát podepsaný svým držitelem tím, že umožňuje přeskočit ověřování SSL.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-113">HDFS connector now supports self-signed public certificate by letting you skip SSL validation.</span></span>
- <span data-ttu-id="b0c8d-114">Opravené: Problém s bránou offline během aktualizace (z důvodu tooclock zkosení)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-114">Fixed: Issue with gateway offline during update (due tooclock skew)</span></span>



## <a name="earlier-versions"></a><span data-ttu-id="b0c8d-115">Starší verze</span><span class="sxs-lookup"><span data-stu-id="b0c8d-115">Earlier versions</span></span>

## <a name="2963132"></a><span data-ttu-id="b0c8d-116">2.9.6313.2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-116">2.9.6313.2</span></span>
### <a name="enhancements-"></a><span data-ttu-id="b0c8d-117">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="b0c8d-117">Enhancements-</span></span>
-   <span data-ttu-id="b0c8d-118">Můžete přidat toowhitelist položky DNS Service Bus, místo vytvoření seznamu povolených všechny Azure IP adresy z brány firewall (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-118">You can add DNS entries toowhitelist Service Bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="b0c8d-119">Další informace v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-119">More details here.</span></span>
-   <span data-ttu-id="b0c8d-120">Nyní můžete zkopírovat data z objektu blob jeden blok až too4.75 TB, což je maximální hello podporované velikost objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-120">You can now copy data to/from a single block blob up too4.75 TB, which is hello max supported size of block blob.</span></span> <span data-ttu-id="b0c8d-121">(dřívější limit byl 195 GB).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-121">(earlier limit was 195 GB).</span></span>
-   <span data-ttu-id="b0c8d-122">Opravené: Nedostatek paměti problém při rozbalení několik malých souborů během aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-122">Fixed: Out of memory issue while unzipping several small files during copy activity.</span></span>
-   <span data-ttu-id="b0c8d-123">Pevné: Index je mimo rozsah problém při kopírování z dokumentu DB tooan místní systém SQL Server s funkcí idempotenci.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-123">Fixed: Index out of range issue while copying from Document DB tooan on-premises SQL Server with idempotency feature.</span></span>
-   <span data-ttu-id="b0c8d-124">Opravené: Skript pro vyčištění SQL nefunguje s místní SQL Server z Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-124">Fixed: SQL cleanup script doesn't work with on-premises SQL Server from Copy Wizard.</span></span>
-   <span data-ttu-id="b0c8d-125">Opravené: Název sloupce s místem na konci hello nefunguje v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-125">Fixed: Column name with space at hello end does not work in copy activity.</span></span>

## <a name="28662833"></a><span data-ttu-id="b0c8d-126">2.8.66283.3</span><span class="sxs-lookup"><span data-stu-id="b0c8d-126">2.8.66283.3</span></span>
### <a name="enhancements-"></a><span data-ttu-id="b0c8d-127">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="b0c8d-127">Enhancements-</span></span>
- <span data-ttu-id="b0c8d-128">Opravené: Problém s chybějícími přihlašovací údaje u restartování počítače brány.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-128">Fixed: Issue with missing credentials on gateway machine reboot.</span></span>
- <span data-ttu-id="b0c8d-129">Opravené: Problém s registrací během brány obnovit záložního souboru.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-129">Fixed: Issue with registration during gateway restore using a backup file.</span></span>


## <a name="2762401"></a><span data-ttu-id="b0c8d-130">2.7.6240.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-130">2.7.6240.1</span></span>
### <a name="enhancements-"></a><span data-ttu-id="b0c8d-131">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="b0c8d-131">Enhancements-</span></span>
- <span data-ttu-id="b0c8d-132">Opravené: Nesprávné čtení desetinnou hodnotu null z databáze Oracle jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-132">Fixed: Incorrect read of Decimal null value from Oracle as source.</span></span>

## <a name="2661922"></a><span data-ttu-id="b0c8d-133">2.6.6192.2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-133">2.6.6192.2</span></span>
### <a name="whats-new"></a><span data-ttu-id="b0c8d-134">Co je nového</span><span class="sxs-lookup"><span data-stu-id="b0c8d-134">What’s new</span></span>
- <span data-ttu-id="b0c8d-135">Zákazníci můžete poskytnout zpětnou vazbu na bránu zaregistrujete činnost.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-135">Customers can provide feedback on gateway registering experience.</span></span>
- <span data-ttu-id="b0c8d-136">Podpora nový formát komprese: ZIP (Deflate)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-136">Support a new compression format: ZIP (Deflate)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="b0c8d-137">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="b0c8d-137">Enhancements-</span></span>
- <span data-ttu-id="b0c8d-138">Zlepšení výkonu pro Oracle jímky, HDFS zdroje.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-138">Performance improvement for Oracle Sink, HDFS source.</span></span>
- <span data-ttu-id="b0c8d-139">Oprava chyby pro brány automatické aktualizace, gateway paralelní zpracování kapacity.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-139">Bug fix for gateway auto update, gateway parallel processing capacity.</span></span>


## <a name="2561641"></a><span data-ttu-id="b0c8d-140">2.5.6164.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-140">2.5.6164.1</span></span>
### <a name="enhancements"></a><span data-ttu-id="b0c8d-141">Vylepšení</span><span class="sxs-lookup"><span data-stu-id="b0c8d-141">Enhancements</span></span>
- <span data-ttu-id="b0c8d-142">Vylepšené a robustnější brány registrace zkušeností – teď můžete sledovat průběh během procesu registrace brány hello, takže je registrace hello prostředí rychlejšího.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-142">Improved and more robust Gateway registration experience- Now you can track progress status during hello Gateway registration process, which makes hello registration experience more responsive.</span></span>
- <span data-ttu-id="b0c8d-143">Zlepšení brány obnovit proces - je stále možné obnovit bránu i v případě, že nemáte hello brány záložní soubor s touto aktualizací.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-143">Improvement in Gateway Restore Process- You can still recover gateway even if you do not have hello gateway backup file with this update.</span></span> <span data-ttu-id="b0c8d-144">To se vyžaduje pověření tooreset propojené služby v portálu.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-144">This would require you tooreset Linked Service credentials in Portal.</span></span>
- <span data-ttu-id="b0c8d-145">Oprava chyby.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-145">Bug fix.</span></span>

## <a name="2461511"></a><span data-ttu-id="b0c8d-146">2.4.6151.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-146">2.4.6151.1</span></span>

### <a name="whats-new"></a><span data-ttu-id="b0c8d-147">Co je nového</span><span class="sxs-lookup"><span data-stu-id="b0c8d-147">What’s new</span></span>

- <span data-ttu-id="b0c8d-148">Nyní můžete ukládat přihlašovací údaje zdroje dat místně.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-148">You can now store data source credentials locally.</span></span> <span data-ttu-id="b0c8d-149">Hello přihlašovací údaje jsou šifrované.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-149">hello credentials are encrypted.</span></span> <span data-ttu-id="b0c8d-150">Hello přihlašovacích údajů zdroje dat lze obnovit a obnovit pomocí hello záložní soubor, který se dá vyexportovat ze hello existující bránu, všechny místní.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-150">hello data source credentials can be recovered and restored using hello backup file that can be exported from hello existing Gateway, all on-premises.</span></span>

### <a name="enhancements-"></a><span data-ttu-id="b0c8d-151">Vylepšení-</span><span class="sxs-lookup"><span data-stu-id="b0c8d-151">Enhancements-</span></span>

- <span data-ttu-id="b0c8d-152">Vylepšené a robustnější brány registrace prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-152">Improved and more robust Gateway registration experience.</span></span>
- <span data-ttu-id="b0c8d-153">Podpora formátu textu v Průvodce kopírováním automatické zjišťování QuoteChar konfigurace a zlepšit hello celkové formátu detekce přesnost.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-153">Support auto detection of QuoteChar configuration for Text format in copy wizard, and improve hello overall format detection accuracy.</span></span>

## <a name="2361002"></a><span data-ttu-id="b0c8d-154">2.3.6100.2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-154">2.3.6100.2</span></span>

- <span data-ttu-id="b0c8d-155">Podpora firstRowAsHeader a SkipLineCount automatické zjišťování v Průvodce kopírováním textových souborů v systému souborů na místě a HDFS.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-155">Support firstRowAsHeader and SkipLineCount auto detection in copy wizard for text files in on-premises File system and HDFS.</span></span>
- <span data-ttu-id="b0c8d-156">Zvýšení stability hello síťové připojení mezi bránou a Service Bus</span><span class="sxs-lookup"><span data-stu-id="b0c8d-156">Enhance hello stability of network connection between gateway and Service Bus</span></span>
- <span data-ttu-id="b0c8d-157">Několik oprav chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-157">A few bug fixes</span></span>


## <a name="2260721"></a><span data-ttu-id="b0c8d-158">2.2.6072.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-158">2.2.6072.1</span></span>

*  <span data-ttu-id="b0c8d-159">Podporuje nastavení proxy serveru HTTP pro použití brány hello hello Správce konfigurace brány.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-159">Supports setting HTTP proxy for hello gateway using hello Gateway Configuration Manager.</span></span> <span data-ttu-id="b0c8d-160">Pokud nakonfigurovaný, objektů Blob v Azure, Azure Table, Azure Data Lake a Documentdb jsou přístupné prostřednictvím proxy serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-160">If configured, Azure Blob, Azure Table, Azure Data Lake, and Document DB are accessed through HTTP proxy.</span></span>
*  <span data-ttu-id="b0c8d-161">Hlavička podporuje zpracování TextFormat při kopírování dat z / tooAzure objektů Blob, Azure Data Lake Store, místní systém souborů a místní HDFS.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-161">Supports header handling for TextFormat when copying data from/tooAzure Blob, Azure Data Lake Store, on-premises File System, and on-premises HDFS.</span></span>
*  <span data-ttu-id="b0c8d-162">Podporuje kopírování dat z objektu Blob připojit a objektů Blob stránky spolu s hello již nepodporuje objekt Blob bloku.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-162">Supports copying data from Append Blob and Page Blob along with hello already supported Block Blob.</span></span>
*  <span data-ttu-id="b0c8d-163">Zavádí nový stav brány **Online (s omezením)**, což znamená, že hlavní funkce hello hello brány funguje s výjimkou hello interaktivní operace podporu pro Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-163">Introduces a new gateway status **Online (Limited)**, which indicates that hello main functionality of hello gateway works except hello interactive operation support for Copy Wizard.</span></span>
*  <span data-ttu-id="b0c8d-164">Zvyšuje odolnost hello registrace brány pomocí registračního klíče.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-164">Enhances hello robustness of gateway registration using registration key.</span></span>

## <a name="216040"></a><span data-ttu-id="b0c8d-165">2.1.6040.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-165">2.1.6040.</span></span>

*  <span data-ttu-id="b0c8d-166">Ovladač DB2 je součástí instalační balíček brány hello teď.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-166">DB2 driver is included in hello gateway installation package now.</span></span> <span data-ttu-id="b0c8d-167">Není nutné tooinstall ho samostatně.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-167">You do not need tooinstall it separately.</span></span>
*  <span data-ttu-id="b0c8d-168">Teď podporuje DB2 ovladače z/OS a DB2 pro i (AS / 400) společně s hello platformách, již podporuje (Linux, Unix a Windows).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-168">DB2 driver now supports z/OS and DB2 for i (AS/400) along with hello platforms already supported (Linux, Unix, and Windows).</span></span>
*  <span data-ttu-id="b0c8d-169">Podporuje používání Azure Cosmos DB jako zdrojový nebo cílový pro místní úložiště dat</span><span class="sxs-lookup"><span data-stu-id="b0c8d-169">Supports using Azure Cosmos DB as a source or destination for on-premises data stores</span></span>
*  <span data-ttu-id="b0c8d-170">Podporuje kopírování dat z/toocold/horkou objektu blob úložiště společně s hello již nepodporuje účet úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-170">Supports copying data from/toocold/hot blob storage along with hello already supported general-purpose storage account.</span></span>
*  <span data-ttu-id="b0c8d-171">Umožňuje tooon místní tooconnect systému SQL Server prostřednictvím brány s oprávněními vzdálené přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-171">Allows you tooconnect tooon-premises SQL Server via gateway with remote login privileges.</span></span>  

## <a name="2060131"></a><span data-ttu-id="b0c8d-172">2.0.6013.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-172">2.0.6013.1</span></span>

*  <span data-ttu-id="b0c8d-173">Můžete vybrat toobe jazyků a kultur hello používá bránu při ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-173">You can select hello language/culture toobe used by a gateway during manual installation.</span></span>

*  <span data-ttu-id="b0c8d-174">Pokud brána nebude fungovat podle očekávání, můžete protokoly brány toosend posledních sedmi dnech tooMicrosoft toofacilitate řešení potíží s hello problém.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-174">When gateway does not work as expected, you can choose toosend gateway logs of last seven days tooMicrosoft toofacilitate troubleshooting of hello issue.</span></span> <span data-ttu-id="b0c8d-175">Pokud brána není připojen toohello cloudovou službu, můžete zvolit toosave a archivaci protokoly gateway.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-175">If gateway is not connected toohello cloud service, you can choose toosave and archive gateway logs.</span></span>  

*  <span data-ttu-id="b0c8d-176">Vylepšení uživatelského rozhraní pro správce konfigurace brány:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-176">User interface improvements for gateway configuration manager:</span></span>

    *  <span data-ttu-id="b0c8d-177">Nastavit stav brány. víc viditelný na kartě Domů hello.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-177">Make gateway status more visible on hello Home tab.</span></span>

    *  <span data-ttu-id="b0c8d-178">Ovládací prvky reorganizovány všechny a jednodušší.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-178">Reorganized and simplified controls.</span></span>

    *  <span data-ttu-id="b0c8d-179">Můžete zkopírovat data z úložiště pomocí hello [nástroj preview bez kódu kopírování](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-179">You can copy data from a storage using hello [code-free copy preview tool](data-factory-copy-data-wizard-tutorial.md).</span></span> <span data-ttu-id="b0c8d-180">V tématu [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) obecné podrobnosti o této funkci.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-180">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details about this feature in general.</span></span>
*  <span data-ttu-id="b0c8d-181">Brána pro správu dat tooingress data přímo z místní databáze systému SQL Server můžete použít do Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-181">You can use Data Management Gateway tooingress data directly from an on-premises SQL Server database into Azure Machine Learning.</span></span>

*  <span data-ttu-id="b0c8d-182">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-182">Performance improvements</span></span>

    * <span data-ttu-id="b0c8d-183">Zlepšení výkonu na zobrazení schématu nebo Preview proti systému SQL Server v nástroji preview bez kódu kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-183">Improve performance on viewing Schema/Preview against SQL Server in code-free copy preview tool.</span></span>

## <a name="11259531"></a><span data-ttu-id="b0c8d-184">1.12.5953.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-184">1.12.5953.1</span></span>

*  <span data-ttu-id="b0c8d-185">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-185">Bug fixes</span></span>

## <a name="11159181"></a><span data-ttu-id="b0c8d-186">1.11.5918.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-186">1.11.5918.1</span></span>

*  <span data-ttu-id="b0c8d-187">Maximální velikost protokolu událostí brány hello zvýšila od 1 MB too40 MB.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-187">Maximum size of hello gateway event log has been increased from 1 MB too40 MB.</span></span>

*  <span data-ttu-id="b0c8d-188">V případě, že během automatickou aktualizaci brány je zapotřebí restartování, zobrazí se dialog s upozorněním.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-188">A warning dialog is displayed in case a restart is needed during gateway auto-update.</span></span> <span data-ttu-id="b0c8d-189">Můžete toorestart vpravo pak nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-189">You can choose toorestart right then or later.</span></span>

*  <span data-ttu-id="b0c8d-190">V případě, že automatické aktualizace nezdaří, instalační program brány opakuje, automatické aktualizace třikrát za maximální.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-190">In case auto-update fails, gateway installer retries auto-updating three times at maximum.</span></span>

*  <span data-ttu-id="b0c8d-191">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-191">Performance improvements</span></span>

    * <span data-ttu-id="b0c8d-192">Zlepšení výkonu pro načítání velké tabulky z místního serveru ve scénáři bez kódu kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-192">Improve performance for loading large tables from on-premises server in code-free copy scenario.</span></span>

*  <span data-ttu-id="b0c8d-193">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-193">Bug fixes</span></span>

## <a name="11058921"></a><span data-ttu-id="b0c8d-194">1.10.5892.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-194">1.10.5892.1</span></span>

*  <span data-ttu-id="b0c8d-195">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-195">Performance improvements</span></span>

*  <span data-ttu-id="b0c8d-196">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-196">Bug fixes</span></span>

## <a name="1958652"></a><span data-ttu-id="b0c8d-197">1.9.5865.2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-197">1.9.5865.2</span></span>

*  <span data-ttu-id="b0c8d-198">Nulové funkcí touch automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="b0c8d-198">Zero touch auto update capability</span></span>
*  <span data-ttu-id="b0c8d-199">Ikona nový panelu s indikátory stavu brány</span><span class="sxs-lookup"><span data-stu-id="b0c8d-199">New tray icon with gateway status indicators</span></span>
*  <span data-ttu-id="b0c8d-200">Možnost příliš "teď"z aktualizovat hello klienta</span><span class="sxs-lookup"><span data-stu-id="b0c8d-200">Ability too“Update now” from hello client</span></span>
*  <span data-ttu-id="b0c8d-201">Čas plánované aktualizace tooset možnost</span><span class="sxs-lookup"><span data-stu-id="b0c8d-201">Ability tooset update schedule time</span></span>
*  <span data-ttu-id="b0c8d-202">Skript prostředí PowerShell pro přepnutím zapnout nebo vypnout automatickou aktualizaci</span><span class="sxs-lookup"><span data-stu-id="b0c8d-202">PowerShell script for toggling auto-update on/off</span></span>
*  <span data-ttu-id="b0c8d-203">Podpora formátu JSON</span><span class="sxs-lookup"><span data-stu-id="b0c8d-203">Support for JSON format</span></span>  
*  <span data-ttu-id="b0c8d-204">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-204">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-205">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-205">Bug fixes</span></span>

## <a name="1858221"></a><span data-ttu-id="b0c8d-206">1.8.5822.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-206">1.8.5822.1</span></span>

*  <span data-ttu-id="b0c8d-207">Lepší řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b0c8d-207">Improve troubleshooting experience</span></span>
*  <span data-ttu-id="b0c8d-208">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-208">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-209">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-209">Bug fixes</span></span>

### <a name="1757951"></a><span data-ttu-id="b0c8d-210">1.7.5795.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-210">1.7.5795.1</span></span>

*  <span data-ttu-id="b0c8d-211">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-211">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-212">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-212">Bug fixes</span></span>

### <a name="1757641"></a><span data-ttu-id="b0c8d-213">1.7.5764.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-213">1.7.5764.1</span></span>

*  <span data-ttu-id="b0c8d-214">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-214">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-215">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-215">Bug fixes</span></span>

### <a name="1657351"></a><span data-ttu-id="b0c8d-216">1.6.5735.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-216">1.6.5735.1</span></span>

*  <span data-ttu-id="b0c8d-217">Podpora místní HDFS zdroj/jímka</span><span class="sxs-lookup"><span data-stu-id="b0c8d-217">Support on-premises HDFS Source/Sink</span></span>
*  <span data-ttu-id="b0c8d-218">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-218">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-219">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-219">Bug fixes</span></span>

### <a name="1656961"></a><span data-ttu-id="b0c8d-220">1.6.5696.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-220">1.6.5696.1</span></span>

*  <span data-ttu-id="b0c8d-221">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-221">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-222">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-222">Bug fixes</span></span>

### <a name="1656761"></a><span data-ttu-id="b0c8d-223">1.6.5676.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-223">1.6.5676.1</span></span>

*  <span data-ttu-id="b0c8d-224">Diagnostické nástroje podpory na Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="b0c8d-224">Support diagnostic tools on Configuration Manager</span></span>
*  <span data-ttu-id="b0c8d-225">Podpora sloupců tabulky pro tabulkové zdroje dat pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b0c8d-225">Support table columns for tabular data sources for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-226">Podpora pro vytváření dat Azure SQL DW</span><span class="sxs-lookup"><span data-stu-id="b0c8d-226">Support SQL DW for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-227">Podpora pro vytváření dat Azure Reclusive v BlobSource a FileSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-227">Support Reclusive in BlobSource and FileSource for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-228">Podpora CopyBehavior – MergeFiles, PreserveHierarchy a FlattenHierarchy v BlobSink a FileSink s binární kopie pro datovou továrnu Azure</span><span class="sxs-lookup"><span data-stu-id="b0c8d-228">Support CopyBehavior – MergeFiles, PreserveHierarchy, and FlattenHierarchy in BlobSink and FileSink with Binary Copy for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-229">Podpora aktivity kopírování hlášení průběhu pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b0c8d-229">Support Copy Activity reporting progress for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-230">Ověření připojení zdroje dat podporu pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="b0c8d-230">Support Data Source Connectivity Validation for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-231">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-231">Bug fixes</span></span>

### <a name="1656721"></a><span data-ttu-id="b0c8d-232">1.6.5672.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-232">1.6.5672.1</span></span>

*  <span data-ttu-id="b0c8d-233">Podpora pro Azure Data Factory název tabulky zdroje dat ODBC</span><span class="sxs-lookup"><span data-stu-id="b0c8d-233">Support table name for ODBC data source for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-234">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-234">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-235">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-235">Bug fixes</span></span>

### <a name="1656581"></a><span data-ttu-id="b0c8d-236">1.6.5658.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-236">1.6.5658.1</span></span>

*  <span data-ttu-id="b0c8d-237">Podpora souboru jímky pro datovou továrnu Azure</span><span class="sxs-lookup"><span data-stu-id="b0c8d-237">Support File Sink for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-238">Podpora pro Azure Data Factory zachování hierarchie v binární kopie</span><span class="sxs-lookup"><span data-stu-id="b0c8d-238">Support preserving hierarchy in binary copy for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-239">Podpora idempotenci aktivity kopírování pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="b0c8d-239">Support Copy Activity Idempotency for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-240">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-240">Bug fixes</span></span>

### <a name="1656401"></a><span data-ttu-id="b0c8d-241">1.6.5640.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-241">1.6.5640.1</span></span>

*  <span data-ttu-id="b0c8d-242">Podpora 3 Další zdroje dat pro Azure Data Factory (rozhraní ODBC, OData, HDFS)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-242">Support 3 more data sources for Azure Data Factory (ODBC, OData, HDFS)</span></span>
*  <span data-ttu-id="b0c8d-243">Podpora znak uvozovky csv analyzátoru pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b0c8d-243">Support quote character in csv parser for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-244">Podpora komprese (BZip2)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-244">Compression support (BZip2)</span></span>
*  <span data-ttu-id="b0c8d-245">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-245">Bug fixes</span></span>

### <a name="1556121"></a><span data-ttu-id="b0c8d-246">1.5.5612.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-246">1.5.5612.1</span></span>

*  <span data-ttu-id="b0c8d-247">Podpora pět relačních databází pro Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata a Sybase)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-247">Support five relational databases for Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata, and Sybase)</span></span>
*  <span data-ttu-id="b0c8d-248">Podpora komprese (Gzip a Deflate)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-248">Compression support (Gzip and Deflate)</span></span>
*  <span data-ttu-id="b0c8d-249">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-249">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-250">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-250">Bug fixes</span></span>

### <a name="1455491"></a><span data-ttu-id="b0c8d-251">1.4.5549.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-251">1.4.5549.1</span></span>

*  <span data-ttu-id="b0c8d-252">Přidání podpory zdroje dat Oracle pro Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b0c8d-252">Add Oracle data source support for Azure Data Factory</span></span>
*  <span data-ttu-id="b0c8d-253">Vylepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b0c8d-253">Performance improvements</span></span>
*  <span data-ttu-id="b0c8d-254">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="b0c8d-254">Bug fixes</span></span>

### <a name="1454921"></a><span data-ttu-id="b0c8d-255">1.4.5492.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-255">1.4.5492.1</span></span>

*  <span data-ttu-id="b0c8d-256">Jednotná binární soubor, který podporuje služby Microsoft Azure Data Factory a Office 365 Power BI</span><span class="sxs-lookup"><span data-stu-id="b0c8d-256">Unified binary that supports both Microsoft Azure Data Factory and Office 365 Power BI services</span></span>
*  <span data-ttu-id="b0c8d-257">Upřesnit hello procesu uživatelské rozhraní konfigurace a registrace</span><span class="sxs-lookup"><span data-stu-id="b0c8d-257">Refine hello Configuration UI and registration process</span></span>
*  <span data-ttu-id="b0c8d-258">Azure Data Factory – Azure příchozí a odchozí podporu pro zdroj dat systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="b0c8d-258">Azure Data Factory – Azure Ingress and Egress support for SQL Server data source</span></span>

### <a name="1253031"></a><span data-ttu-id="b0c8d-259">1.2.5303.1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-259">1.2.5303.1</span></span>

*  <span data-ttu-id="b0c8d-260">Opravte problém toosupport časový limit více časově náročné připojení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-260">Fix timeout issue toosupport more time-consuming data source connections.</span></span>

### <a name="1155268"></a><span data-ttu-id="b0c8d-261">1.1.5526.8</span><span class="sxs-lookup"><span data-stu-id="b0c8d-261">1.1.5526.8</span></span>

*  <span data-ttu-id="b0c8d-262">Vyžaduje rozhraní .NET Framework 4.5.1 předpokladem je během instalace.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-262">Requires .NET Framework 4.5.1 as a prerequisite during setup.</span></span>

### <a name="1051442"></a><span data-ttu-id="b0c8d-263">1.0.5144.2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-263">1.0.5144.2</span></span>

*  <span data-ttu-id="b0c8d-264">Žádné změny, které ovlivňují scénáře Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-264">No changes that affect Azure Data Factory scenarios.</span></span>
