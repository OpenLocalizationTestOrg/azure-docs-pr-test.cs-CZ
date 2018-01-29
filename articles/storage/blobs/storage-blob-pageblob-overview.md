---
title: "Jedinečné funkce objektů BLOB stránky Azure | Microsoft Docs"
description: "Přehled o Azure stránky objekty BLOB, výhody a případy použití se ukázkové skripty."
services: storage
documentationcenter: 
author: anasouma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: wielriac
ms.openlocfilehash: 019b793f6d2b4cb70514d867b78c9501240baeda
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="unique-features-of-azure-page-blobs"></a>Jedinečné funkce objektů BLOB stránky Azure

Azure Storage nabízí tři typy úložiště objektů blob: objekty BLOB bloku a doplňovacích objektů BLOB a objekty BLOB stránky. Objekty BLOB bloku se skládá z bloků a jsou ideální pro ukládání textu nebo binárních souborů a efektivně nahrávání velkých souborů. Append – objekty BLOB jsou také tvoří bloky, ale jsou optimalizované pro doplňovací operace, což je ideální pro scénáře protokolování. Objekty BLOB stránky jsou tvořeny stránky o velikosti 512 bajtů až 8 TB celkové velikosti a jsou efektivnější pro časté náhodné čtení/zápisu operace. Objekty BLOB stránky jsou základem Azure IaaS disků. Tento článek se zaměřuje na vysvětlením funkce a výhody objekty BLOB stránky.

## <a name="overview"></a>Přehled
Objekty BLOB stránky jsou kolekce stránek 512 bajtů, které poskytují možnost čtení/zápisu libovolný rozsah bajtů. Proto jsou ideální pro ukládání dat na základě indexu a zhuštěné struktury jako operačního systému a datové disky pro virtuální počítače a databáze objekty BLOB stránky. Například Azure SQL DB používá objekty BLOB stránek jako základní trvalé úložiště pro jeho databáze. Kromě toho objekty BLOB stránky jsou také často používají pro soubory s aktualizacemi, na základě rozsahu.  

Klíčové funkce Azure objekty BLOB stránky jsou jeho rozhraní REST, odolnost základní úložiště a možnostech bezproblémové migrace do Azure. Tyto funkce v další podrobnosti najdete v další části se budeme zabývat. Kromě toho jsou aktuálně podporovány objekty BLOB stránky Azure na dva typy úložiště: Storage úrovně Premium a Standard Storage. Storage úrovně Premium je určený speciálně pro procesy vyžadující konzistentní vysoký výkon a nízkou latencí díky objekty BLOB stránky premium ideální pro databáze vysoké původce datového úložiště.  Standardní úložiště je nákladově efektivnější pro spuštění úlohy kterému latence nevadí.

## <a name="sample-use-cases"></a>Ukázka případy použití

Probereme několik případů použití pro objekty BLOB stránky počínaje disků Azure IaaS. Azure BLOB stránky jsou základem virtuální disky platformy pro Azure IaaS. Azure operačního systému a datové disky jsou implementované jako virtuální disky, kde data spolehlivě trvalá v platformě Azure Storage a pak doručí virtuálních počítačů pro maximální výkon. Disky systému Azure zůstávají v Hyper-V [formátu virtuálního pevného disku](https://technet.microsoft.com/library/dd979539.aspx) a uložené jako [objekt Blob stránky](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) ve službě Azure Storage. Kromě používání virtuální disky pro virtuální počítače Azure IaaS, objekty BLOB stránky také povolit PaaS a DBaaS scénáře, jako je služba Azure SQL DB, která aktuálně používá objekty BLOB stránky pro ukládání dat SQL, povolení vysoká rychlost náhodného operace čtení a zápis pro databázi. Dalším příkladem může být, pokud máte PaaS služba pro přístup sdílená média pro spolupráci video aplikace pro úpravy, objekty BLOB stránky povolit přístup k náhodné umístění na médiu. Taky umožňuje rychlé a efektivní úpravy a slučování stejná média více uživatelů. 

První strany služby společnosti Microsoft, jako je Azure Site Recovery, Azure Backup, jakož i celá řada vývojářů třetích stran byla implementována špičkový inovace pomocí rozhraní REST objekt blob stránky. Zde jsou některé jedinečné scénáře implementovat v Azure: 
* Správu aplikací směrované přírůstkový snímek: aplikace můžou využívat snímky objekt Blob stránky a rozhraní REST API pro ukládání aplikací kontrolní body, aniž by docházelo k nákladná duplicitních dat. To je možné, protože podporujeme místních snímků pro objekty BLOB stránky, které nevyžadují kopírování celou data. Tyto veřejné snímek rozhraní API také umožní přístup k informacím a kopírování rozdílů mezi snímky.
* Migrace aplikací a dat z místního do cloudu za provozu: místní data kopírovat a používat rozhraní REST API pro zápis přímo na objekt Blob stránky Azure, zatímco místní virtuální počítač stále běží. Po cíl zachytila, můžete rychle převzetí služeb při selhání virtuálního počítače Azure pomocí tato data. To umožňuje migraci virtuálních počítačů a virtuálních disků z místního do cloudu s minimálními výpadky, protože migrace dat probíhá na pozadí, když budete pokračovat v používání virtuálního počítače a výpadek potřebné pro převzetí služeb při selhání bude krátký (v minutách).
* [Na základě SAS](../common/storage-dotnet-shared-access-signature-part-1.md) sdíleného přístupu, která umožňuje různé scénáře jako více čtečky a jedním zapisovače s podporou pro řízení souběžnosti.

## <a name="page-blob-features"></a>Objekt Blob stránky funkce

### <a name="rest-api"></a>REST API
V následujícím dokumentu začít pracovat s [vývoj s použitím objekty BLOB stránky](storage-dotnet-how-to-use-blobs.md). Jako příklad dejte nám podívejte se na informace o přístupu k objekty BLOB stránky pomocí klientské knihovny pro úložiště pro .NET. 

Následující diagram popisuje celkové vztahy mezi účet, kontejnery a objekty BLOB stránky.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-certain-size"></a>Vytváření prázdný objekt Blob stránky určité velikosti
Pokud chcete vytvořit objekt Blob stránky, jsme nejprve vytvořit objekt CloudBlobClient základní identifikátor Uri pro přístup k úložišti objektů blob pro váš účet úložiště (pbaccount na obrázku 1) společně s objekt StorageCredentialsAccountAndKey, jak je uvedeno níže.  Příklad ukazuje pak vytváření odkaz na objekt CloudBlobContainer a následného vytvoření kontejneru (testvhds), pokud ještě neexistuje.  Potom pomocí objektu CloudBlobContainer, můžeme vytvořit odkaz na objekt CloudPageBlob tak, že zadáte název objektu blob stránky (os4.vhd) chceme přístup. Pak vytvořte objekt blob stránky říkáme [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) předávání v maximální velikost pro tento objekt blob, jsme chcete vytvořit.  BlobSize musí být násobkem 512 bajtů.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Změna velikosti objekt Blob stránky
Chcete-li po vytvoření změnit velikost objekt Blob stránky, použijte [změnit velikost](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) rozhraní API. Požadovaná velikost musí být násobkem 512 bajtů.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Vytvoření stránek pro objekt Blob stránky
K vytvoření stránky, použijte [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) metoda.  To umožňuje zapisovat sekvenční sadu stránek až 4MBs. Posun zapisuje do musí začínat na hranici 512 bajtů (startingOffset % 512 == 0) a ukončí se na hranici 512 - 1.  Následující kód ukazuje příklad volání WritePages pro objekt blob, které přistupují k jsme:
```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Jakmile žádost o zápis pro sekvenční sada stránek ve službě blob je úspěšné a se replikují pro odolnost a odolnost proti chybám, byla potvrzena zápis a úspěch se vrátí zpět do klienta.  

Následující diagram ukazuje 2 samostatné operací zápisu:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Operace zápisu začínající na posunu 0 délky 1024 bajtů 
2.  Operace zápisu začínající na posunu 4096 délky 1024 

#### <a name="reading-pages-from-a-page-blob"></a>Čtení stránek z objekt Blob stránky
Chcete-li číst stránky, použijte [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) metoda čtení rozsah bajtů z objektů blob stránky. To umožňuje stáhnout úplný objektů blob nebo rozsah bajtů od všech posun v objektu blob. Při čtení, není nutné spustit v násobkem 512 posun. Při čtení bajtů z stránky NUL, vrátí službu 0 bajtů.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
Následující obrázek znázorňuje operace čtení s BlobOffSet 256 a rangeSize 4352. Data vrácená je označený na oranžová. Pro stránky NUL jsou vráceny nulami

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Pokud máte řídce vyplněná objektů blob můžete stáhnout pouze oblasti platný stránky předejdete platícího pro egressing nula bajtů a snižování latence stahování.  Chcete-li zjistit, které stránky jsou založenou na datech, použijte [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). Můžete vytvořit výčet vrácený rozsahy a stahování dat v každém rozsahu. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### <a name="leasing-a-page-blob"></a>Leasing objekt Blob stránky
Objekt Blob zapůjčení operaci vytváří a spravuje zámku na objektu blob pro zápis a operace odstranění. Tato operace je velmi užitečné v situacích, kdy je objekt blob stránky přistupuje od více klientů k zajištění, že pouze jednoho klienta může zapsat do objektu blob v čase. Disky systému Azure, například využívá to leasing mechanismus pro Ujistěte se, že disk je jenom jeden virtuální počítač spravuje. Doba trvání uzamčení může být 15 až 60 sekund, nebo může být nekonečná. Najdete v dokumentaci [sem](/rest/api/storageservices/lease-blob) další podrobnosti.

> Pomocí následujícího odkazu zobrazíte [ukázky kódu](/resources/samples/?service=storage&term=blob&sort=0) pro mnoho scénářů jiné aplikace. 

Objekty BLOB stránky kromě bohatá rozhraní API REST, zadejte také sdíleného přístupu, odolnost a rozšířené zabezpečení. Obsahuje tyto výhody podrobněji v další odstavcích. 

### <a name="concurrent-access"></a>Souběžný přístup
REST API pro objekty BLOB stránky a je leasing mechanismus umožňuje aplikacím pro přístup z více klientů objekt blob stránky. Řekněme například, že potřebujete k vytváření distribuované cloudové služby, který sdílí úložiště objektů s více uživateli. Může to být webové aplikace slouží velké kolekce obrázků na několik uživatelů. Jednou z možností pro implementaci to je pro použití s připojené disky virtuálního počítače. Downsides tento zahrnout (i), disk může být připojen pouze pro jeden virtuální počítač a omezení škálovatelnost, flexibilitu tak zvýšit rizika omezení. Pokud dojde k potížím s virtuální počítač nebo služba spuštěna na virtuálním počítači, pak z důvodu zapůjčení, bitovou kopii nebudou k dispozici dokud zapůjčení vyprší platnost, nebo je poškozená; a (ii) další náklady na použití virtuálních počítačů IaaS. 

Alternativní možností je použít objekty BLOB stránky přímo prostřednictvím rozhraní API REST úložiště Azure. Tato možnost se eliminuje potřeba nákladná virtuální počítače IaaS, nabízí úplnou flexibilitu přímý přístup z více klientů, zjednodušuje správu služby odstraněním potřeba připojení a odpojení disky a eliminuje riziko problémů ve virtuálním počítači. A poskytuje stejnou úroveň výkonu pro náhodné čtení/zápisu operace jako disk

### <a name="durability-and-high-availability"></a>Odolnost a vysoká dostupnost
Úložiště Standard a premium jsou odolná úložiště, kde se vždy replikují data objektů blob stránky zajistit odolnost a vysokou dostupnost. Další informace o redundance úložiště Azure najdete [dokumentaci](../common/storage-redundancy.md). Azure má konzistentně doručit odolnost podnikové úrovni pro IaaS disky a stránka objektů BLOB s špičkový NULOVÉ % [rozpočítat na roční období míra selhání](https://en.wikipedia.org/wiki/Annualized_failure_rate). To znamená Azure ztratil nikdy data objektů blob stránky zákazníka. 

### <a name="seamless-migration-to-azure"></a>Bezproblémovou migraci na Azure
Pro zákazníky a vývojáře, kteří mají zájem o implementaci vlastní přizpůsobená řešení pro zálohování Azure nabízí i přírůstkové snímky, které obsahují pouze se rozdíly. Tato funkce zabraňuje náklady na počáteční úplná kopie, což značně snižuje náklady na zálohování. Spolu se schopností efektivně pro čtení a zkopírujte rozdílová data je to jiné výkonné funkci, která umožňuje i další inovace od vývojářů, což v rozhraní třídy nejvhodnější zálohování a po havárii (DR) obnovení v Azure. Můžete nastavit vlastní zálohování nebo řešení pro zotavení po Havárii pro virtuální počítače v Azure pomocí [Blob snímku](/rest/api/storageservices/snapshot-blob) spolu s [rozsahů stránek Get](/rest/api/storageservices/get-page-ranges) rozhraní API a [přírůstkové kopie Blob](/rest/api/storageservices/incremental-copy-blob) rozhraní API, které můžete používejte pro snadno kopírování přírůstkových dat pro zotavení po Havárii. 

Kromě toho mnoho podniků má kritickým úlohám, které už běží v místních datových center. Pro migraci úloh do cloudu, jedním z hlavních otázky by doba potřebná pro kopírování dat a riziko neočekávaných problémů po přepnutí výpadku. V mnoha případech může být výpadek showstopper pro migraci do cloudu. Pomocí REST API pro objekty BLOB stránky, Azure tento problém řeší tím, že umožňuje migrace do cloudu s minimálním dopadem na kritické úlohy. 

Příklady na postup pořízení snímku a jak obnovit objekt blob stránky ze snímku, naleznete [nastavení záložní proces, který používá přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md) článku.


## <a name="summary"></a>Souhrn
Azure se snaží aplikací pracovalo nejvhodnější třídy pro naše zákazníky. Jsme integrované platformou úložiště s podnikové úrovni odolnost dat, proto naše zákazníky můžete důvěřovat Azure s jejich důležitá data. Azure nabízí jedinečný API podporu a vývojáře možnosti objekty BLOB stránky, které můžou poskytovat žádné veřejný cloud platforma. To vedlo k mnoha stran 1. nebo 3. stran inovace jako bezproblémové cloudu migrace, prostředí nadřízená zálohování nebo zotavení po Havárii, podpora PaaS a DBaaS, řešení distribuované úložiště a jiné inovace pro virtuální počítače IaaS nebo disky. Obecně Azure vystupoval mezi všechny veřejné cloudové platformy, s těmito možnostmi jedinečný a výhody Azure zákazníků z nich hodnotu – přidá že může poskytovat žádné jiné cloudové platformy.
