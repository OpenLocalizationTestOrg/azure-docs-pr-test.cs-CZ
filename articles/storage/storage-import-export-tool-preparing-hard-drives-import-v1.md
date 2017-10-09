---
title: "aaaPreparing pevné disky pro Azure importu a exportu importovat úlohy - v1 | Microsoft Docs"
description: "Zjistěte, jak tooprepare pevných disků pomocí hello WAImportExport v1 nástroj toocreate úlohy importu pro službu Azure Import/Export hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 3d818245-8b1b-4435-a41f-8e5ec1f194b2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 8803ac01b7c7a2ec2e3199231d7b4c04ceafd5a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Příprava pevných disků pro úlohu importu
tooprepare jeden nebo více pevných disků pro úlohy importu, postupujte takto:

-   Identifikovat hello data tooimport do hello služby objektů Blob

-   Určete cíl virtuální adresáře a objekty BLOB v hello služby objektů Blob

-   Určit, kolik jednotek, které budete potřebovat

-   Zkopírujte data tooeach hello pevných disků

 Ukázkový pracovní postup, najdete v části [ukázkový pracovní postup tooPrepare pevné disky pro úlohy importu](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-hello-data-toobe-imported"></a>Identifikovat toobe hello data importovat
 Hello první krok toocreating úlohy importu je toodetermine které adresářů a souborů, které jste se bude tooimport. To může být seznam adresářů, seznam souborů, jedinečné nebo kombinaci těchto dvou. Po zahrnuté adresáře se budou všechny soubory v adresáři hello a jejích podadresářů součástí úlohy importu hello.

> [!NOTE]
>  Vzhledem k tomu, že nadřazený adresář je zahrnuté podadresáře jsou zahrnuty rekurzivně, zadejte pouze hello nadřazený adresář. Také nezadávejte žádné z jejích podadresářů.
>
>  V současné době hello nástroj Microsoft Azure Import/Export má hello následující omezení: Pokud adresář obsahuje více dat, než může obsahovat pevného disku, pak hello adresář musí toobe rozdělen do menších adresáře. Například pokud adresář obsahuje 2,5 TB dat a hello kapacita pevného disku se pouze 2TB, pak musíte toobreak hello 2,5 TB adresáře do menší adresáře. Toto omezení bude vyřešen v novější verzi nástroje hello.

## <a name="identify-hello-destination-locations-in-hello-blob-service"></a>Identifikovat hello cílová umístění ve službě blob hello
 Pro každý adresář nebo soubor, který bude importován budete potřebovat tooidentify cílový virtuální adresář nebo objekt blob v hello služby objektů Blob Azure. Tyto cíle použije jako vstupy toohello nástroj Azure Import/Export. Všimněte si, že adresáře musí být oddělen s hello lomítkem znakem "/".

 Hello následující tabulka uvádí některé příklady cílů objektů blob:

|Zdrojový soubor nebo adresář|Cílový objekt blob nebo virtuální adresář|
|------------------------------|-------------------------------------------|
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|
|K:\Temp\FavoriteVideo.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|

## <a name="determine-how-many-drives-are-needed"></a>Určit, kolik jednotek jsou potřeba.
 Dále musíte toodetermine:

-   Hello počet pevných disků potřeba toostore hello data.

-   Hello adresáře nebo samostatné soubory, které budou zkopírovaný tooeach pevném disku.

 Ujistěte se, že máte hello počet pevných disků, které budete potřebovat toostore hello dat přenášíte.

## <a name="copy-data-tooyour-hard-drive"></a>Zkopírujte data tooyour pevný disk
 Tato část popisuje, jak toocall hello nástroj Azure Import/Export toocopy vaše data tooone nebo více pevných disků. Pokaždé, když zavoláte hello nástroj Azure Import/Export, můžete vytvořit nový *zkopírujte relace*. Vytvoření relace alespoň jedna kopie pro každou jednotku toowhich zkopírovat data; v některých případech může potřebovat více než jednu kopii relace toocopy všechny jednotky toosingle data. Zde jsou některé důvody, bude pravděpodobně vyžadovat více relací kopie:

-   Je třeba vytvořit relaci samostatná kopie pro každou jednotku, kterou chcete zkopírovat.

-   Kopírování relace můžete zkopírovat jeden adresář nebo jednotku toohello jediného objektu blob. Pokud kopírujete několik adresářů, více objektů BLOB nebo jejich kombinaci, budete potřebovat toocreate více relací kopírování.

-   Můžete zadat vlastnosti a metadata, která bude nastavena na objekty BLOB hello importován jako součást úlohy importu. Vlastnosti Hello nebo metadata, která zadáte pro relaci kopírování použije objekty BLOB tooall určeného této kopie relaci. Pokud chcete použít jiné vlastnosti toospecify nebo metadata pro některé objekty BLOB, budete potřebovat toocreate samostatné zkopírujte relace. V tématu [vlastnosti nastavení a metadat během procesu importu hello](storage-import-export-tool-setting-properties-metadata-import-v1.md)Další informace.

> [!NOTE]
>  Pokud máte více počítačů, které splňují požadavky hello uvedených v [hello nastavení se nástroj Azure Import/Export](storage-import-export-tool-setup-v1.md), můžete zkopírovat data toomultiple pevné disky paralelně spuštěním instance tohoto nástroje na každém počítači.

 Pro každý pevný disk, který připravit s hello nástroj Azure Import/Export vytvoří nástroj hello soubor jednoho deníku. Je nutné soubory deníku hello ze všech jednotky toocreate hello import úlohy. Hello deníku soubor může být také použít tooresume jednotky přípravy Pokud dojde k přerušení nástroj hello.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>Syntaxe Azure Import/Export nástroje pro úlohy importu
 tooprepare jednotky pro úlohu importu volání hello Azure Import/Export nástroj s hello **PrepImport** příkaz. Parametry, které zahrnete, závisí na tom, jestli to je hello první kopie relaci nebo relaci další kopie.

 Hello první relaci kopie pro jednotku vyžaduje některé další parametry toospecify hello klíč účtu úložiště; písmeno jednotky cílového Hello; jestli musí být ve formátu jednotka hello; jestli hello jednotky musí být šifrovaný a pokud ano, hello klíč nástroje BitLocker; a adresář protokolu hello. Tady je syntaxe hello počáteční kopii relace toocopy adresář nebo jeden soubor:

 **Nejdříve zkopírovat relace toocopy jeden adresář**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Nejdříve zkopírovat relace toocopy jeden soubor**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 V další kopie relace není nutné toospecify hello počátečních parametrů. Tady je syntaxe hello toocopy relace následné kopírování adresářů nebo jenom jeden soubor:

 **Další kopie relací toocopy jeden adresář**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Další kopie relací toocopy jeden soubor**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-hello-first-copy-session-for-a-hard-drive"></a>Parametry pro hello nejdříve zkopírovat relace pro pevný disk
 Pokaždé, když spustíte hello nástroj Azure Import/Export toocopy soubory toohello pevného disku, nástroj hello vytvoří relaci kopírování. Každou relaci kopírování kopíruje jeden adresář nebo jeden soubor tooa pevný disk. Hello stavu relace kopie hello se zapíše soubor toohello deníku. Pokud dojde k přerušení relaci kopírování (například kvůli výpadku napájení systému tooa), může být obnoveno spuštěním nástroje hello znovu a zadáte hello deníku soubor na příkazovém řádku hello.

> [!WARNING]
>  Pokud zadáte hello **/formátu** parametr pro hello první relaci kopie hello jednotky se bude formátovat a vymažou se všechna data na jednotce hello. Doporučuje se, že používáte prázdné disky pouze pro relaci kopírování.

 Hello příkazu používaný k hello první relaci kopie pro každou jednotku vyžaduje různé parametry než hello příkazy pro kopírování následné relace. Hello následující tabulka uvádí hello další parametry, které jsou dostupné pro hello první relaci kopie:

|Parametr příkazového řádku|Popis|
|-----------------------------|-----------------|
|**/Sk:**< StorageAccountKey\>|`Optional.`Hello klíč účtu úložiště pro data hello hello úložiště účtu toowhich budou naimportovány. Musí obsahovat buď **/sk:**< StorageAccountKey\> nebo **/csas:**< ContainerSas\> v příkazu hello.|
|**/csas:**< ContainerSas\>|`Optional`. kontejner Hello SAS toouse tooimport data toohello účet úložiště. Musí obsahovat buď **/sk:**< StorageAccountKey\> nebo **/csas:**< ContainerSas\> v příkazu hello.<br /><br /> Hello hodnotu tohoto parametru musí začínat hello název kontejneru, za nímž následuje otazník (?) a tokenu SAS hello. Například:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> Hello oprávnění, zda zadaný na adrese URL hello nebo v zásadách uložené přístupu, musí obsahovat pro čtení, zápisu a odstraňování pro import úlohy a pro čtení, zápisu a seznamu pro úlohy exportu.<br /><br /> Pokud tento parametr zadán, musí být všechny objekty BLOB toobe importovat nebo exportovat zadaný v hello sdílený přístupový podpis kontejneru hello.|
|**/ t:**< TargetDriveLetter\>|`Required.`Hello písmeno jednotky pevného disku cílového hello hello aktuální kopie relace, bez hello koncové dvojtečkou.|
|**/ Format**|`Optional.`Zadejte tento parametr, když hello jednotky potřebuje toobe formátu; jinak vynechejte. Předtím, než nástroj hello formáty hello jednotky, se zobrazí výzva k potvrzení z konzoly. toosuppress hello potvrzení, zadejte parametr /silentmode hello.|
|**/silentmode**|`Optional.`Zadejte tento parametr toosuppress hello potvrzení pro formátování disku targert hello.|
|**/ šifrování**|`Optional.`Tento parametr zadán, při s nástrojem BitLocker a potřebuje toobe šifrované nástrojem hello nebyl ještě zašifrované jednotky hello. Pokud hello jednotka již byla zašifrována pomocí nástroje BitLocker, parametr vynechejte a zadejte hello `/bk` parametr poskytování hello existující klíč nástroje BitLocker.<br /><br /> Pokud zadáte hello `/format` parametr a potom musíte zadat také hello `/encrypt` parametr.|
|**/BK:**< BitLockerKey\>|`Optional.`Pokud `/encrypt` je tento parametr zadán, vynechejte. Pokud `/encrypt` je tento parametr vynechán, je nutné toohave již mít šifrované jednotky hello pomocí nástroje BitLocker. Použijte tento parametr toospecify hello BitLocker klíč. Šifrování nástrojem BitLocker je vyžadována pro všechny pevné disky pro úlohy importu.|
|**/logdir:**< LogDirectory\>|`Optional.`Adresář protokolu Hello Určuje že adresář toobe používá toostore podrobné protokoly, jakož i dočasné soubory manifestu. Pokud není zadaný, aktuální adresář hello se použije jako adresář protokolu hello.|

### <a name="parameters-required-for-all-copy-sessions"></a>Parametrů požadovaných pro všechny relace kopie
 Hello deníku soubor obsahuje hello stav pro všechny relace kopie pro pevný disk. Také obsahuje informace hello potřeby toocreate hello úloha importu. Vždy je nutné zadat soubor deníku při spuštění hello nástroj Azure Import/Export, jakož i ID relace kopie:

|||
|-|-|
|Parametr příkazového řádku|Popis|
|**/j:**< JournalFile\>|`Required.`Hello cestě toohello deníku souboru. Každé jednotky, musí mít přesně jeden soubor deníku. Všimněte si, že tento soubor deníku hello nesmí nacházet na cílové jednotce hello. Přípona souboru deníku Hello je `.jrn`.|
|**/ID:**< ID relace\>|`Required.`ID relace Hello identifikuje relaci kopírování. Je přesné obnovení použité tooensure kopie dojde k přerušení relace. Soubory, které jste zkopírovali v relaci kopie jsou uloženy v adresáři s názvem po ID relace hello na cílové jednotce hello.|

### <a name="parameters-for-copying-a-single-directory"></a>Parametry pro kopírování jeden adresář
 Při kopírování jeden adresář, platí následující hello požadované a volitelné parametry:

|Parametr příkazového řádku|Popis|
|----------------------------|-----------------|
|**/srcdir:**< SourceDirectory\>|`Required.`Hello zdrojový adresář, který obsahuje soubory toobe zkopírovat toohello cílové jednotky. Cesta k adresáři Hello musí být absolutní cesta (není relativní cesta).|
|**/dstdir:**< DestinationBlobVirtualDirectory\>|`Required.`Hello cesta toohello cílový virtuální adresář ve vašem účtu úložiště služby Windows Azure. virtuální adresář Hello může nebo již neexistuje.<br /><br /> Můžete zadat kontejner, nebo jako předpona objektu blob `music/70s/`. Hello cílový adresář musí začínat řetězcem hello název kontejneru, za nímž následuje lomítkem "/" a může volitelně obsahovat blob virtuální adresáře, který končí textem "/".<br /><br /> Pokud cílový kontejner hello hello Kořenový kontejner, je nutné explicitně zadat hello Kořenový kontejner, včetně hello lomítkem, jako `$root/`. Vzhledem k tomu, že nesmí obsahovat objekty BLOB v kontejneru kořenové hello "/" v jejich názvy, když hello cílový adresář je kořenový kontejner hello se nezkopírují jakéhokoliv podadresáře v hello zdrojový adresář.<br /><br /> Zda toouse být názvy kontejnerů platný při zadávání cílové virtuální adresáře nebo objekty BLOB. Uvědomte si, že názvy kontejnerů musí být malými písmeny. Pravidla pojmenování kontejneru, najdete v části [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/ Dispozice:**< přejmenovat &#124; ne přepsat &#124; přepsání >|`Optional.`Určuje chování hello, pokud objekt blob se hello zadaná adresa již existuje. Platné hodnoty tohoto parametru jsou: `rename`, `no-overwrite` a `overwrite`. Všimněte si, že jsou tyto hodnoty malá a velká písmena. Pokud není zadaná žádná hodnota, je výchozí hello `rename`.<br /><br /> Hodnota zadaná pro tento parametr ovlivňuje všechny soubory hello v hello adresář zadaný hello Hello `/srcdir` parametr.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Určuje typ hello objektů blob pro cílový hello objekty BLOB. Platné hodnoty jsou: `BlockBlob` a `PageBlob`. Všimněte si, že jsou tyto hodnoty malá a velká písmena. Pokud není zadaná žádná hodnota, je výchozí hello `BlockBlob`.<br /><br /> Ve většině případů `BlockBlob` se doporučuje. Pokud zadáte `PageBlob`, délka hello každý soubor v adresáři hello musí být násobkem 512, hello velikost stránky pro objekty BLOB stránky.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Cesta souboru vlastností toohello pro objekty BLOB cílové hello. V tématu [importu/exportu služby Metadata a formát vlastností souboru](storage-import-export-file-format-metadata-and-properties.md) Další informace.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Cesta toohello soubor metadat pro objekty BLOB cílové hello. V tématu [importu/exportu služby Metadata a formát vlastností souboru](storage-import-export-file-format-metadata-and-properties.md) Další informace.|

### <a name="parameters-for-copying-a-single-file"></a>Parametry pro kopírování jeden soubor
 Při kopírování jeden soubor, hello požadované a volitelné parametry platné:

|Parametr příkazového řádku|Popis|
|----------------------------|-----------------|
|**/srcfile:**< zdrojový soubor\>|`Required.`Hello úplná cesta toohello souboru toobe zkopírovat. Cesta k adresáři Hello musí být absolutní cesta (není relativní cesta).|
|**/dstblob:**< DestinationBlobPath\>|`Required.`Hello cesta toohello cílový objekt blob v účtu úložiště služby Windows Azure. Objekt blob Hello může nebo již neexistuje.<br /><br /> Zadejte počáteční název objektu blob hello s hello název kontejneru. Název objektu blob Hello nemůže začínat "/" nebo název účtu úložiště hello. Pravidla pojmenování objektů blob, najdete v části [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> Pokud cílový kontejner hello hello Kořenový kontejner, je nutné explicitně zadat `$root` jako hello kontejneru, například `$root/sample.txt`. Všimněte si, že objekty BLOB v kontejneru kořenové hello nesmí obsahovat "/" v jejich názvy.|
|**/ Dispozice:**< přejmenovat &#124; ne přepsat &#124; přepsání >|`Optional.`Určuje chování hello, pokud objekt blob se hello zadaná adresa již existuje. Platné hodnoty tohoto parametru jsou: `rename`, `no-overwrite` a `overwrite`. Všimněte si, že jsou tyto hodnoty malá a velká písmena. Pokud není zadaná žádná hodnota, je výchozí hello `rename`.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Určuje typ hello objektů blob pro cílový hello objekty BLOB. Platné hodnoty jsou: `BlockBlob` a `PageBlob`. Všimněte si, že jsou tyto hodnoty malá a velká písmena. Pokud není zadaná žádná hodnota, je výchozí hello `BlockBlob`.<br /><br /> Ve většině případů `BlockBlob` se doporučuje. Pokud zadáte `PageBlob`, délka hello každý soubor v adresáři hello musí být násobkem 512, hello velikost stránky pro objekty BLOB stránky.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Cesta souboru vlastností toohello pro objekty BLOB cílové hello. V tématu [importu/exportu služby Metadata a formát vlastností souboru](storage-import-export-file-format-metadata-and-properties.md) Další informace.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Cesta toohello soubor metadat pro objekty BLOB cílové hello. V tématu [importu/exportu služby Metadata a formát vlastností souboru](storage-import-export-file-format-metadata-and-properties.md) Další informace.|

### <a name="resuming-an-interrupted-copy-session"></a>Pokračování v relaci dojde k přerušení kopie
 Pokud z nějakého důvodu dojde k přerušení relace kopírování, můžete ho obnovit tak, že spustíte nástroj hello s pouze hello deníku zadaný soubor:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Pouze hello poslední kopie relace, pokud byl ukončen neobvyklým způsobem, můžete obnovit.

> [!IMPORTANT]
>  Při obnovení relace kopie neměňte přidáním nebo odebráním soubory hello zdrojových datových souborů a adresářů.

### <a name="aborting-an-interrupted-copy-session"></a>Přerušení relaci dojde k přerušení kopie
 Pokud dojde k přerušení relace kopírování a není možné tooresume (například pokud je zdrojový adresář prokázat nedostupné), je nutné přerušit hello aktuální relace tak, aby se vrátit zpět a nové kopie relace lze spustit:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Pouze hello poslední kopie relace, pokud byl ukončen neobvyklým způsobem, můžete přerušena. Všimněte si, že jste nelze přerušit hello první relaci kopie pro jednotku. Místo toho je nutné restartovat hello kopie relace pomocí nového souboru deníku.

## <a name="next-steps"></a>Další kroky

* [Hello nastavení se nástroj Azure Import/Export](storage-import-export-tool-setup-v1.md)
* [Vlastnosti nastavení a metadata během hello importu](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [Stručná referenční příručka pro často používané příkazy](storage-import-export-tool-quick-reference-v1.md) 
* [Kontrola stavu úlohy s použitím kopií souborů protokolu](storage-import-export-tool-reviewing-job-status-v1.md)
* [Oprava úlohy importu](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Oprava úlohy exportu](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Řešení potíží s hello nástroj Azure Import/Export](storage-import-export-tool-troubleshooting-v1.md)
