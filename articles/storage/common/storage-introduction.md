---
title: "aaaIntroduction tooAzure úložiště | Microsoft Docs"
description: "Úvod tooAzure úložiště, úložiště dat společnosti Microsoft v cloudu hello."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: robinsh
ms.openlocfilehash: f61324f98d0a8eb24023e4344acdb4ca58bb27f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
<!-- this is hello same version that is in hello MVC branch -->
# <a name="introduction-toomicrosoft-azure-storage"></a>Úvod tooMicrosoft Azure Storage

Microsoft Azure Storage je cloudová služba spravovaná Microsoftem, která poskytuje vysoce dostupné, zabezpečené, odolné, škálovatelné a redundantní úložiště. Microsoft se stará o údržbu a řeší za vás kritické problémy. 

Azure Storage se skládá ze tří datových služeb: Blob Storage, File Storage a Queue Storage. Úložiště objektů BLOB podporuje standard a premium úložiště pomocí SSD pouze pro nejrychlejší možné výkonu hello Storage úrovně premium. Další funkce je studeného úložiště, což vám toostorage velkých objemů málokdy načítaná data pro nižší náklady.

V tomto článku se dozvíte o hello následující:
* služby Azure Storage Hello
* Hello typy účtů úložiště
* přístup k objektům blob, frontám a souborům
* šifrování
* replikace 
* přenos dat do nebo z úložiště
* Hello mnoho úložiště knihovny klienta k dispozici. 


<!-- RE-ENABLE THESE AFTER MVC GOES LIVE 
tooget up and running with Azure Storage quickly, check out one of hello following Quickstarts:
* [Create a storage account using PowerShell](storage-quick-create-storage-account-powershell.md)
* [Create a storage account using CLI](storage-quick-create-storage-account-cli.md)
-->


## <a name="introducing-hello-azure-storage-services"></a>Představení služby Azure Storage hello

toouse poskytnuté některé z hello služeb úložiště Azure – úložiště objektů Blob, úložiště File a Queue storage – můžete nejdřív vytvořit účet úložiště, a potom můžou přenášet data z specifické služby v daném účtu úložiště. 

## <a name="blob-storage"></a>Blob Storage

Bloby jsou v podstatě soubory podobné těm, které ukládáte ve svém počítači (nebo tabletu, mobilním zařízení atd.). Mohou to být obrázky, soubory Microsoft Excelu, soubory HTML, virtuální pevné disky (VHD), velké objemy dat, jako jsou protokoly nebo zálohy databází – prakticky cokoli. Objekty BLOB jsou uloženy v kontejnerech, které jsou podobné toofolders. 

Po ukládání souborů do úložiště objektů Blob, které můžete k nim přistupovat odkudkoliv v hello, world pomocí adres URL, hello rozhraní REST nebo jeden z knihovny klienta úložiště Azure SDK hello. Klientské knihovny pro úložiště jsou dostupné pro řadu jazyků, včetně Node.js, Javy, PHP, Ruby, Pythonu a .NET. 

Existují tři typy objektů blob – objekty blob bloku, doplňovací objekty blob a objekty blob stránky (používané jako soubory VHD).

* Objekty BLOB bloku jsou běžné soubory používané toohold až tooabout 4.7 TB. 
* Objekty BLOB stránky jsou soubory používané toohold náhodný přístup až velikost too8 TB. Ty se používají pro hello soubory virtuálního pevného disku, které zálohování virtuálních počítačů.
* Připojení je tvoří bloky jako objekty BLOB bloku hello objekty BLOB, ale jsou optimalizované pro doplňovací operace. Ty se používají pro položky, jako protokolování informace toohello stejný objekt blob z více virtuálních počítačů.

U velkých datových sad, kde omezení sítě zkontrolujte nahrávání nebo stahování dat tooBlob úložiště přes přenosu hello reálné můžete dodávat sadu tooimport tooMicrosoft pevné disky nebo exportovat data z hello datového centra. V tématu [použít hello službu Microsoft Azure Import/Export tooTransfer Data tooBlob úložiště](../storage-import-export-service.md).

## <a name="file-storage"></a>File Storage

Hello službu Azure souborů umožňuje tooset až vysoce dostupné síťové sdílené složky, které jsou přístupné pomocí hello standardní protokol Server Message Block (SMB). Prostředky, které můžete sdílet víc virtuálních počítačů hello stejné soubory se pro čtení i zápis. Můžete si také přečíst hello soubory pomocí rozhraní REST hello nebo knihovny klienta úložiště hello. 

Jeden z věcí, které slouží k rozlišení Azure File storage z podnikové sdílené složky souborů je, že vám přístup k souborům hello, z libovolného místa v hello, world pomocí adresy URL, která ukazuje soubor toohello a obsahuje token sdílený přístupový podpis (SAS). Můžete vygenerovat tokeny SAS; umožňují privátní asset tooa konkrétní přístup pro určité množství času. 

Sdílené složky můžete použít pro řadu běžných scénářů: 

* Mnoho místních aplikací používá sdílené složky. Tato funkce umožňuje snazší toomigrate aplikací, u nichž sdílet data tooAzure. Pokud připojíte hello toohello sdílené složky souboru stejné písmeno, který hello jednotky místní aplikace používá, hello součástí aplikace, který přistupuje k hello sdílené složky by měly spolupracovat s minimální, pokud existuje, změny.

* Ve sdílené složce je možné ukládat konfigurační soubory, kde k nim bude mít přístup více virtuálních počítačů. Nástroje, které používají více vývojáři ve skupině mohou být uloženy ve sdílené složce, zajistíte, že každý uživatel, najít a že používají hello stejnou verzi.

* Diagnostické protokoly, metriky a výpisy stavu systému jsou jenom tři příklady data, která můžete zapsat tooa sdílené složky a zpracovat nebo analyzovat později.

V tento čas, ověřování založené na službě Active Directory a přístupu nejsou podporovány seznamy řízení (ACL), ale nebudou se někdy v budoucnu hello. přihlašovací údaje účtu úložiště Hello jsou použité tooprovide ověřování pro přístup k toohello sdílené složky. To znamená, každý, kdo má sdílenou složku hello připojené bude mít čtení/zápisu přístup toohello sdílené složky.

## <a name="queue-storage"></a>Queue Storage

Hello službu front Azure je použité toostore a načtení zprávy. Zprávy fronty může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv. Fronty jsou obvykle použité toostore seznam zpráv toobe asynchronně. 

Řekněme například, chcete mít tooupload obrázky toobe zákazníků a chcete toocreate miniatury pro každou obrázek. Můžete mít zákazníkovi počkejte jste toocreate hello miniatur při odesílání hello obrázky. Alternativou by toouse fronty. Po dokončení jeho nahrání hello zákazníka zápisu toohello front zpráv. Pak máte funkce Azure načítat uvítací zprávu z fronty hello a vytváření miniatur hello. Jednotlivé části hello tohoto zpracování je možné rozšířit samostatně, poskytuje tak větší kontrolu při ladění pro vaše použití.

<!-- this bookmark is used by other articles; you'll need tooupdate them before this goes into production ROBIN-->
## <a name="table-storage"></a>Úložiště Table
<!-- add a link toohello old table storage toothis paragraph once it's moved -->
Azure Table Storage na úrovni Standard je teď součástí Cosmos DB. Pro Azure Table Storage je ale také dostupná úroveň Premium, která nabízí tabulky optimalizované pro zvýšení propustnosti, globální distribuci a automatické sekundární indexy. toolearn další a zkuste se hello nové prostředí premium, zkontrolujte [Cosmos databázi Azure: Tabulka API](https://aka.ms/premiumtables).

## <a name="disk-storage"></a>Diskové úložiště

Hello Azure Storage team vlastní také disky, která obsahuje všechny hello spravovaných a nespravovaných disku možnosti používaných virtuálními počítači. Další informace o těchto funkcích najdete v tématu hello [výpočetní služby dokumentaci](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

## <a name="types-of-storage-accounts"></a>Typy účtů úložiště 

Tato tabulka obsahuje různé typy účtů úložiště a objektů, které lze použít s jednotlivými hello.

|**Typ účtu úložiště**|**Standard pro obecné účely**|**Premium pro obecné účely**|**Blob Storage, horká a studená vrstva přístupu**|
|-----|-----|-----|-----|
|**Podporované služby**| Služby objektů blob, souborů a tabulek | Služba objektů blob | Služba objektů blob|
|**Typy podporovaných objektů blob**|Objekty blob bloku, objekty blob stránky a doplňovací objekty blob | Objekty blob stránky | Objekty blob bloku a doplňovací objekty blob|

### <a name="general-purpose-storage-accounts"></a>Účty úložiště pro obecné účely

Existují dva typy účtů úložiště pro obecné účely. 

#### <a name="standard-storage"></a>Storage úrovně Standard 

účty úložiště standard storage, které se dají použít pro všechny typy dat jsou účty úložiště Hello nejčastěji používá. Účty úložiště Standard storage pomocí dat toostore magnetické média.

#### <a name="premium-storage"></a>Storage úrovně Premium

Storage úrovně Premium poskytuje vysoce výkonné úložiště pro objekty blob stránky, které se primárně využívají pro soubory VHD. Prémiové účty úložiště pomocí SSD toostore data. Microsoft doporučuje používat Storage úrovně Premium pro všechny vaše virtuální počítače.

### <a name="blob-storage-accounts"></a>Účty služby Blob Storage

účet úložiště objektů Blob Hello je specializovaný účet úložiště používá toostore objekty BLOB bloku a doplňovací objekty BLOB. V těchto účtech nejde ukládat objekty blob stránky, a proto nemůžete ukládat soubory VHD. Tyto účty umožňují tooset tooHot úrovně přístupu nebo nástrojů; kdykoli se smí měnit úroveň Hello. 

úroveň přístupu aktivní Hello se používá pro soubory, které se využívají často – platíte vyšší náklady na úložiště, ale je mnohem nižší náklady hello přístupu k objektům BLOB hello. Pro objekty BLOB uložené v úroveň studeného přístupu hello platíte vyšší náklady pro přístup k objektům BLOB hello, ale je mnohem nižší hello náklady na úložiště.

## <a name="accessing-your-blobs-files-and-queues"></a>Přístup k objektům blob, frontám a souborům

Každý účet úložiště má dva ověřovací klíče a každý z nich je možné použít pro libovolnou operaci. Existují dva klíče, můžete se vrátit přes hello klíče příležitostně tooenhance zabezpečení. Je důležité, aby tyto klíče zajistit zabezpečení, protože mít k dispozici, a to společně s názvem účtu hello umožňuje neomezený přístup k datům tooall v účtu úložiště hello. 

Tato část vypadá dva způsoby toosecure hello úložiště účtu a jeho data. Podrobné informace o zabezpečení vašeho účtu úložiště a vaše data, najdete v části hello [Průvodce zabezpečením Azure Storage](storage-security-guide.md).

### <a name="securing-access-toostorage-accounts-using-azure-ad"></a>Zabezpečení přístupu toostorage účty pomocí služby Azure AD

Jedním ze způsobů toosecure přístup tooyour úložiště dat, spočívá v kontrole klíče účtu úložiště toohello přístup. Pomocí Správce prostředků řízení přístupu na základě rolí (RBAC) můžete přiřadit toousers role, skupiny nebo aplikace. Tyto role jsou svázané tooa konkrétní sadu akcí, které jsou povolené nebo zakázané. Pomocí RBAC zpracovává účet úložiště tooa toogrant přístup jenom hello operace správy pro daný účet úložiště, jako je například změna úrovně přístupu hello. Nelze použít RBAC toogrant přístup toodata objekty jako kontejneru nebo ve sdílené složce. Můžete však RBAC toogrant přístup toohello klíče účtu úložiště, které lze poté použít tooread hello datových objektů. 

### <a name="securing-access-using-shared-access-signatures"></a>Zabezpečení přístupu pomocí sdílených přístupových podpisů 

Můžete použít sdílené přístupové podpisy a uložené toosecure zásady přístupu datových objektů. Sdílený přístupový podpis (SAS) je řetězec, který obsahuje token zabezpečení, který může být připojen toohello identifikátor URI pro určitý prostředek, který vám umožní toodelegate přístup toospecific úložiště objektů a omezení toospecify například oprávnění a rozsah hello data a času přístupu. Tato funkce má rozsáhlé možnosti. Podrobné informace najdete v části příliš[pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).

### <a name="public-access-tooblobs"></a>Tooblobs veřejný přístup

Hello služby objektů Blob můžete tooprovide veřejný přístup tooa kontejner a jeho objekty BLOB nebo konkrétní objekt blob. Když nějaký kontejner nebo objekt blob označíte jako veřejně přístupný, kdokoli si ho může anonymně přečíst bez nutnosti ověření. Příklad, kdy se má toodo, to je, pokud máte web, který používá bitové kopie, videa nebo dokumenty z úložiště objektů Blob. Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md) 

## <a name="encryption"></a>Šifrování

Nejsou k dispozici pro služby úložiště hello několika základní typy šifrování. 

### <a name="encryption-at-rest"></a>Šifrování v klidovém stavu 

Můžete povolit šifrování služby úložiště (SSE) na službu souborů buď hello (preview) nebo hello služby objektů Blob pro účet úložiště Azure. Pokud je povoleno, všechny data zapsána toohello specifické služby zašifrována před zapsána. Při čtení hello data se dešifrují před vrácením. 

### <a name="client-side-encryption"></a>Šifrování na straně klienta

Hello knihovny klienta úložiště mají metody můžete volat tooprogrammatically šifrování dat před odesláním napříč hello přenosová z klienta tooAzure hello. Uloží se zašifrovaná a to znamená, že jsou zašifrovaná, i když jsou neaktivní. Při čtení dat hello zpět, je dešifrovat hello informace po jeho přijetí. 

### <a name="encryption-in-transit-with-azure-file-shares"></a>Šifrování během přenosu s využitím sdílených složek Azure

Další informace o sdílených přístupových podpisech najdete v tématu [Použití sdílených přístupových podpisů (SAS)](../storage-dotnet-shared-access-signature-part-1.md). V tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md) a [ověřování pro služby úložiště Azure hello](https://msdn.microsoft.com/library/azure/dd179428.aspx) Další informace o účtu úložiště tooyour zabezpečený přístup.

Další podrobnosti o zabezpečení účtu úložiště a šifrování najdete v tématu hello [Průvodce zabezpečením Azure Storage](storage-security-guide.md).

## <a name="replication"></a>Replikace

V pořadí tooensure vaše data byla odolná Azure Storage má možnost tookeep hello (a spravovat) více kopií vaše data. Označuje se jako replikace, nebo někdy také redundance. Když nastavujete účet úložiště, vybíráte typ replikace. Ve většině případů tato nastavení můžete změnit po vytvoření účtu úložiště hello. 

Všechny účty úložiště mají **místně redundantní úložiště (LRS)**. To znamená, že tři kopie vaše data spravovaná službou Azure Storage v datovém centru hello zadat, když byl nastaven účet úložiště hello. Když jsou změny potvrzeny tooone kopírovat, hello další dvě kopie jsou aktualizovány před vrácením úspěch. To znamená, že hello tři repliky jsou vždy v synchronizaci. Také hello tři kopie nacházejí v domén selhání samostatné a upgradovacích domén, což znamená, že vaše data jsou k dispozici i v případě selhání uzlu úložiště, která uchovává data nebo aktualizaci přijatých toobe offline. 

**Místně redundantní úložiště (LRS)**

Jak jsme vysvětlili výše, při použití LRS máte tři kopie vašich dat v jednom datovém centru. Zpracovává hello problém dat bude nedostupný, pokud uzel úložiště selže nebo je provést offline toobe aktualizovat, ale není hello případ celého datového centra bude nedostupný.

**Zónově redundantní úložiště (ZRS)**

Zónově redundantní úložiště (ZRS) udržuje hello tři místní kopií vašich dat i další sadu tři kopie vaše data. Hello druhé sadě tři kopie se replikují asynchronně přes datových center v rámci jednoho nebo dvou oblastech. Nezapomeňte, že zónově redundantní úložiště je dostupné jenom pro objekty blob bloku v účtech úložiště pro obecné účely. Také po vytvoření účtu úložiště a vybrané ZRS, nedá se převést toouse tooany jiných typu replikace, nebo naopak.

Účty ZRS poskytují větší odolnost než LRS, ale nemají možnosti protokolování nebo metrik. 

**Geograficky redundantní úložiště (GRS)**

Geograficky redundantní úložiště (GRS) udržuje hello tři místní kopií vašich dat v primární oblasti, plus další sadu tři kopie dat v sekundární oblasti stovky miles od primární oblasti hello. V případě selhání v primární oblasti hello hello Azure Storage bude převzetí služeb při selhání toohello sekundární oblast. 

**Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)** 

Geograficky redundantní úložiště s přístupem pro čtení je úplně stejně jako GRS, s tím rozdílem, že získat přístup pro čtení toohello data v hello sekundárního umístění. Když primární datové centrum hello bude dočasně nedostupný, můžete dál tooread hello data ze sekundárního umístění hello. To může být velmi užitečné. Například můžete mít webovou aplikaci, která změní do režimu jen pro čtení a bodů toohello sekundární kopie, povolení některé přístupu, i když nejsou aktualizace k dispozici. 

> [!IMPORTANT]
> Jak vaše data se replikují po vytvoření účtu úložiště, pokud jste zadali při vytváření účtu hello ZRS, můžete změnit. Všimněte si však, se vám účtovat poplatek za Pokud přejdete z LRS tooGRS nebo RA-GRS další jednorázové data přenos.
>

Další informace o replikaci najdete v tématu [Replikace Azure Storage](storage-redundancy.md).

Informace pro obnovení po havárii, najdete v části [co toodo, když dojde k výpadku Azure Storage](storage-disaster-recovery-guidance.md).

Pro příklad tooleverage RA-GRS tooensure vysoká dostupnost úložiště, najdete v části [navrhování vysoce dostupné aplikace pomocí RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="transferring-data-tooand-from-azure-storage"></a>Přenos dat tooand ze služby Azure Storage

Blob toocopy nástroj příkazového řádku AzCopy hello a data souborů můžete použít v rámci účtu úložiště nebo mezi různými účty úložiště. V tématu, že jedna z následujících hello články o pomoc:

* [Přenos dat pomocí nástroje AzCopy pro Windows](storage-use-azcopy.md)
* [Přenos dat pomocí nástroje AzCopy pro Linux](storage-use-azcopy-linux.md)

AzCopy je postavený na hello [knihovna pro přesun dat Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), která je aktuálně dostupná ve verzi preview.

Hello Azure Import/Export služby lze použít tooimport nebo export velkých objemů dat tooor objektů blob z účtu úložiště. Příprava a poštovní více pevných disků tooan datové centrum Azure, kde bude přenos dat hello z hello pevné disky a posílají hello pevné disky tooyou. Další informace o hello službu Import/Export najdete v tématu [použít hello službu Microsoft Azure Import/Export tooTransfer Data tooBlob úložiště](../storage-import-export-service.md).

## <a name="pricing"></a>Ceny

Podrobné informace o cenách pro Azure Storage najdete v tématu hello [cenová stránky](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Rozhraní API, knihovny a nástroje služby Storage
Prostředky Azure Storage jsou dostupné přes jakýkoli jazyk, který umí vytvářet požadavky HTTP/HTTPS. Azure Storage dále nabízí programovací knihovny pro několik oblíbených jazyků. Tyto knihovny zjednodušují spoustu aspektů práce s Azure Storage, protože se starají o drobnosti jako synchronní a asynchronní vyvolání, dávkování operací, řízení výjimek, automatické opakování pokusů, operační chování atd. Knihovny jsou aktuálně dostupné pro hello následující jazyky a platformy, s jinými uživateli v hello kanál:

### <a name="azure-storage-data-services"></a>Datové služby Azure Storage
* [REST API služby Storage](/rest/api/storageservices/)
* [Klientská knihovna pro úložiště pro .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Klientská knihovna pro úložiště pro C++](https://github.com/Azure/azure-storage-cpp)
* [Klientská knihovna pro úložiště pro Javu/Android](https://azure.microsoft.com/develop/java/)
* [Klientská knihovna pro úložiště pro Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Klientská knihovna pro úložiště pro PHP](https://azure.microsoft.com/develop/php/)
* [Klientská knihovna pro úložiště pro Python](https://azure.microsoft.com/develop/python/)
* [Klientská knihovna pro úložiště pro Ruby](https://azure.microsoft.com/develop/ruby/)
* [Rutiny pro úložiště pro PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)
* [Příkazy úložiště pro CLI 2.0](/cli/azure/storage)

## <a name="next-steps"></a>Další kroky

* [Další informace o službě Blob Storage](../blobs/storage-blobs-introduction.md)
* [Další informace o službě File Storage](../storage-files-introduction.md)
* [Další informace o službě Queue Storage](../queues/storage-queues-introduction.md)

<!-- RE-ENABLE THESE AFTER MVC GOES LIVE 
tooget up and running with Azure Storage quickly, check out one of hello following Quickstarts:
* [Create a storage account using PowerShell](storage-quick-create-storage-account-powershell.md)
* [Create a storage account using CLI](storage-quick-create-storage-account-cli.md)
-->

<!-- FIGURE OUT WHAT tooDO WITH ALL THESE LINKS.

Azure Storage resources can be accessed by any language that can make HTTP/HTTPS requests. Additionally, Azure Storage offers programming libraries for several popular languages. These libraries simplify many aspects of working with Azure Storage by handling details such as synchronous and asynchronous invocation, batching of operations, exception management, automatic retries, operational behavior and so forth. Libraries are currently available for hello following languages and platforms, with others in hello pipeline:

### Azure Storage data services
* [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
* [Storage Client Library for Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage Client Library for Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage Client Library for PHP](https://azure.microsoft.com/develop/php/)
* [Storage Client Library for Python](https://azure.microsoft.com/develop/python/)
* [Storage Client Library for Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)

### Azure Storage management services
* [Storage Resource Provider REST API Reference](/rest/api/storagerp/)
* [Storage Resource Provider Client Library for .NET](/dotnet/api/microsoft.azure.management.storage)
* [Storage Resource Provider Cmdlets for PowerShell 1.0](/powershell/module/azure.storage)
* [Storage Service Management REST API (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage data movement services
* [Storage Import/Export Service REST API](../storage-import-export-service.md)
* [Storage Data Movement Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Tools and utilities
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.
* [Azure Storage Client Tools](../storage-explorers.md)
* [Azure SDKs and Tools](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy Command-Line Utility](http://aka.ms/downloadazcopy)

## Next steps
toolearn more about Azure Storage, explore these resources:

### Documentation
* [Azure Storage Documentation](https://azure.microsoft.com/documentation/services/storage/)
* [Create a storage account](../storage-create-storage-account.md)

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!--* [Get started with Azure Storage in five minutes](storage-getting-started-guide.md)
-->

### <a name="for-administrators"></a>Pro správce
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití Azure CLI s Azure Storage](../storage-azure-cli.md)

### <a name="for-net-developers"></a>Pro vývojáře v rozhraní .NET
* [Začínáme s úložištěm Azure Blob pomocí rozhraní .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Začínáme s úložištěm Azure Table pomocí rozhraní .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Začínáme s úložištěm Azure Queue pomocí rozhraní .NET](../storage-dotnet-how-to-use-queues.md)
* [Začínáme s úložištěm Azure File ve Windows](../storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Pro vývojáře v Javě a Androidu
* [Jak toouse úložiště Blob z Javy](../blobs/storage-java-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Javy](../../cosmos-db/table-storage-how-to-use-java.md)
* [Jak toouse úložiště Queue z Javy](../storage-java-how-to-use-queue-storage.md)
* [Jak toouse úložiště File z Javy](../storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Pro vývojáře v Node.js
* [Jak toouse úložiště objektů Blob z Node.js](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Jak toouse úložiště Queue z Node.js](../storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Pro vývojáře v PHP
* [Jak toouse úložiště objektů Blob z PHP](../blobs/storage-php-how-to-use-blobs.md)
* [Jak toouse úložiště Table z PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Jak toouse úložiště Queue z PHP](../storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Pro vývojáře v Ruby
* [Jak toouse úložiště objektů Blob z Ruby](../blobs/storage-ruby-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)
* [Jak toouse úložiště Queue z Ruby](../storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Pro vývojáře v Pythonu
* [Jak toouse úložiště objektů Blob z Pythonu](../blobs/storage-python-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Pythonu](../../cosmos-db/table-storage-how-to-use-python.md)
* [Jak toouse úložiště Queue z Pythonu](../storage-python-how-to-use-queue-storage.md)   
* [Jak toouse úložiště File z Pythonu](../storage-python-how-to-use-file-storage.md) 
-->