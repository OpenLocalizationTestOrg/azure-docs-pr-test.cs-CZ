---
title: "Úloha importu aaaPreparing pevné disky pro Azure importu a exportu | Microsoft Docs"
description: "Zjistěte, jak tooprepare pevných disků pomocí hello WAImportExport nástroj toocreate úlohy importu pro hello služba Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: dc4f4f580f70dbd504317f59df142b3e4c48cb66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Příprava úlohy importu pevných disků

Hello WAImportExport nástroj je hello jednotky přípravy a nástroje pro opravu, můžete použít s hello [služby Microsoft Azure Import/Export](../storage-import-export-service.md). Můžete použít tento nástroj toocopy data toohello pevné disky budete tooship tooan datového centra Azure. Po dokončení úlohy importu, můžete tento nástroj toorepair všechny objekty BLOB, které došlo k poškození, chyběl nebo konflikt s další objekty BLOB. Po přijetí hello jednotky z úlohy dokončené export, můžete použít tento nástroj toorepair všechny soubory, které došlo k poškození nebo na hello jednotky chybí. V tomto článku jsme projít hello použití tohoto nástroje.

## <a name="prerequisites"></a>Požadavky

### <a name="requirements-for-waimportexportexe"></a>Požadavky pro WAImportExport.exe

- **Konfigurace počítače**
  - Windows 7, Windows Server 2008 R2 nebo novější operační systém Windows
  - Musí být nainstalované rozhraní .NET framework 4. Najdete v části [– nejčastější dotazy](#faq) o toocheck, jestli je rozhraní .net Framework nainstalované na počítači hello.
- **Klíč účtu úložiště** -potřebujete aspoň jeden z klíčů hello účtu pro účet úložiště hello.

### <a name="preparing-disk-for-import-job"></a>Příprava disku úlohy importu

- **BitLocker –** BitLocker musí být povolená na hello počítač běžící hello WAImportExport nástroj. V tématu hello [– nejčastější dotazy](#faq) jak tooenable nástroje BitLocker.
- **Disky** přístupné z počítače, na kterém je spuštěn nástroj WAImportExport. V tématu [– nejčastější dotazy](#faq) pro specifikaci disku.
- **Zdrojové soubory** -hello soubory, které máte v plánu tooimport musí být přístupné z počítače kopie hello, ať už jsou sdílené síťové složky nebo místním pevném disku.

### <a name="repairing-a-partially-failed-import-job"></a>Oprava úlohy částečně selhání importu

- **Kopírování souboru protokolu** který se vygeneruje, když služba Azure Import/Export zkopíruje data mezi účtem úložiště a disku. Nachází se ve cílový účet úložiště.

### <a name="repairing-a-partially-failed-export-job"></a>Oprava úlohu částečně selhání exportu

- **Kopírování souboru protokolu** který se vygeneruje, když služba Azure Import/Export zkopíruje data mezi účtem úložiště a disku. Nachází se ve vašem účtu úložiště zdroje.
- **Soubor manifestu** -[Nepovinné] umístěný na exportovaný disku, který byl vrácen společností Microsoft.

## <a name="download-and-install-waimportexport"></a>Stáhněte a nainstalujte WAImportExport

Stáhnout hello [nejnovější verzi WAImportExport.exe](https://www.microsoft.com/download/details.aspx?id=55280). Extrahujte hello komprimované obsahu tooa adresáře v počítači.

Svůj další úkol je toocreate souborů CSV.

## <a name="prepare-hello-dataset-csv-file"></a>Připravte si soubor CSV hello datové sady

### <a name="what-is-dataset-csv"></a>Co je datová sada sdíleného svazku clusteru

Soubor CSV datové sady je hello hodnotou příznaku /dataset je soubor CSV, který obsahuje seznam adresářů a/nebo seznam jednotky tootarget toobe zkopírovat soubory. Hello první krok toocreating úlohy importu je toodetermine které adresářů a souborů, které jste se bude tooimport. To může být seznam adresářů, seznam souborů, jedinečné nebo kombinaci těchto dvou. Po zahrnuté adresáře se budou všechny soubory v adresáři hello a jejích podadresářů součástí úlohy importu hello.

Pro každý adresář nebo soubor toobe importovat musí identifikovat cílový virtuální adresář nebo objekt blob v hello služby objektů Blob Azure. Jako vstupy toohello WAImportExport nástroj použijete tyto cíle. Adresáře musí být oddělen s hello lomítkem znakem "/".

Hello následující tabulka uvádí některé příklady cílů objektů blob:

| Zdrojový soubor nebo adresář | Cílový objekt blob nebo virtuální adresář |
| --- | --- |
| H:\Video | https://mystorageaccount.BLOB.Core.Windows.NET/video |
| H:\Photo | https://mystorageaccount.BLOB.Core.Windows.NET/Photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.BLOB.Core.Windows.NET/Music |

### <a name="sample-datasetcsv"></a>Ukázka dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>Polí v souboru CSV datové sady

| Pole | Popis |
| --- | --- |
| BasePath | **[Vyžaduje]**<br/>Hello hodnota tohoto parametru představuje hello zdroj, kde se nachází toobe hello data importovat. Nástroj Hello bude rekurzivnímu kopírování všech dat umístěných v této cestě.<br><br/>**Povolené hodnoty**: to má toobe platnou cestu v místním počítači nebo cestu ke sdílené složce platná a musí být hello uživatele přístupný. Cesta k adresáři Hello musí být absolutní cesta (není relativní cesta). Pokud cesta hello končí "\\", představuje cestu k ukončení bez jiný adresář"\\" představuje soubor.<br/>V tomto poli je povolené žádné regulární výraz. Pokud hello cesta obsahuje mezery, dejte ho do "".<br><br/>**Příklad**: "c:\Directory\c\Directory\File.txt"<br>"\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **[Vyžaduje]**<br/> Hello cesta toohello cílový virtuální adresář ve vašem účtu úložiště služby Windows Azure. virtuální adresář Hello může nebo již neexistuje. Pokud neexistuje, vytvoří jednu službu Import/Export.<br/><br/>Zda toouse být názvy kontejnerů platný při zadávání cílové virtuální adresáře nebo objekty BLOB. Uvědomte si, že názvy kontejnerů musí být malými písmeny. Pravidla pojmenování kontejneru, najdete v části [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Pokud jenom kořenový není zadaný, se replikují hello adresářovou strukturu hello zdroje v kontejneru objektů blob cílové hello. Pokud se požaduje jinou adresářovou strukturu než hello jeden ve zdroji, více řádků mapování ve sdíleném svazku clusteru<br/><br/>Můžete zadat kontejner nebo objekt blob předpona jako Hudba nebo 70s /. Hello cílový adresář musí začínat řetězcem hello název kontejneru, za nímž následuje lomítkem "/" a může volitelně obsahovat blob virtuální adresáře, který končí textem "/".<br/><br/>Pokud cílový kontejner hello hello Kořenový kontejner, je nutné explicitně zadat hello Kořenový kontejner, včetně hello lomítkem, jako $root /. Vzhledem k tomu, že nesmí obsahovat objekty BLOB v kontejneru kořenové hello "/" v jejich názvy, když hello cílový adresář je kořenový kontejner hello se nezkopírují jakéhokoliv podadresáře v hello zdrojový adresář.<br/><br/>**Příklad**<br/>Pokud cílová cesta k objektu blob hello https://mystorageaccount.blob.core.windows.net/video, hello hodnotu v tomto poli může být video nebo  |
| BlobType | **[Nepovinné]**  bloku &#124; stránky<br/>Aktuálně podporuje Import/Export služby 2 typy objektů BLOB. Stránka objekty BLOB a výchozí BlobsBy blokovat všechny soubory se naimportují jako objekty BLOB bloku. A \*.vhd a \*.vhdx se naimportovat, protože stránka BlobsThere je limit na hello – objekt blob bloku a – objekt blob stránky povolenou velikost. V tématu [cíle škálovatelnosti úložiště](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files) Další informace.  |
| Dispozice | **[Nepovinné]**  přejmenovat &#124; ne přepsat &#124; přepsání <br/> Toto pole určuje hello kopie chování při importu jednofaktorovému Když dat se nahrál toohello účet úložiště z disku hello. Dostupné možnosti jsou: přejmenování &#124; chcete ho přepsat &#124; ne přepsat. Výchozí hodnoty příliš "přejmenovat" Pokud nic zadán. <br/><br/>**Přejmenujte**: Pokud se nachází objekt se stejným názvem, vytvoří se kopie v cílovém umístění.<br/>Přepsat: přepíše soubor hello novější souborem. Hello soubor pomocí poslední úpravy služby wins.<br/>**Přepsat ne**: přeskočí zápis hello soubor, pokud již existuje.|
| MetadataFile | **[Nepovinné]** <br/>pole toothis hodnoty Hello je hello metadata souboru, který lze zadat, pokud hello jeden potřebuje toopreserve hello metadata objektů hello nebo zadat vlastní metadata. Cesta toohello soubor metadat pro objekty BLOB cílové hello. V tématu [importu/exportu služby Metadata a formát vlastností souboru](../storage-import-export-file-format-metadata-and-properties.md) Další informace |
| PropertiesFile | **[Nepovinné]** <br/>Cesta souboru vlastností toohello pro objekty BLOB cílové hello. V tématu [importu/exportu služby Metadata a formát vlastností souboru](../storage-import-export-file-format-metadata-and-properties.md) Další informace. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>Připravte si soubor InitialDriveSet nebo AdditionalDriveSet sdíleného svazku clusteru

### <a name="what-is-driveset-csv"></a>Co je driveset sdíleného svazku clusteru

Hello hello /InitialDriveSet nebo /AdditionalDriveSet příznak hodnotu soubor CSV, který obsahuje seznam hello disky, které jsou písmena jednotek hello toowhich namapované tak, aby hello nástroj může správně zvolit hello seznam toobe disky připravené. Pokud velikost dat hello je větší než velikost jednoho disku, nástroj WAImportExport hello zajistěte distribuci hello dat na více disků v optimálního uvedené v tomto souboru CSV.

Neexistuje žádné omezení počtu hello disky hello dat může být napsán tooin a jedné relace. Nástroj Hello provede distribuci dat na základě velikosti disku a velikost složky. Vybere hello disk, který je většina optimalizované pro objekt velikosti hello. Hello dat po odeslání toohello účet úložiště bude sblížené back toohello adresářovou strukturu, která byla zadaná v souboru datové sady. V pořadí toocreate driveset sdíleného svazku clusteru postupujte podle následujících kroků hello.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Vytvoření základního svazku a přiřadit písmeno jednotky

V pořadí toocreate vytvoření základního svazku a přiřadit písmeno jednotky, podle pokynů hello pro "Jednodušší oddílu Vytvoření" uveden na [přehled správy disků](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>Ukázka InitialDriveSet a AdditionalDriveSet CSV souboru

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Polí v souboru Driveset CSV

| Pole | Hodnota |
| --- | --- |
| Písmeno_jednotky | **[Vyžaduje]**<br/> Každé jednotky, které poskytuje nástroj toohello jako cíl hello musí mít jednoduchý svazek systému souborů NTFS a tooit přiřazeno písmeno jednotky.<br/> <br/>**Příklad**: R nebo r |
| FormatOption | **[Vyžaduje]**  Formátu &#124; AlreadyFormatted<br/><br/> **Formát**: určení to naformátuje všechny hello data na disku hello. <br/>**AlreadyFormatted**: nástroj hello přeskočí, formátování, pokud je zadána tato hodnota. |
| SilentOrPromptOnFormat | **[Vyžaduje]**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: poskytuje tuto hodnotu povolí nástroj hello toorun uživatele v bezobslužném režimu. <br/>**PromptOnFormat**: nástroj hello vyzve uživatele tooconfirm hello jestli akce hello je skutečně určený v každé formátu.<br/><br/>Pokud není nastavená, příkaz k přerušení a zobrazit chybová zpráva: "Nesprávná hodnota pro SilentOrPromptOnFormat: žádné" |
| Šifrování | **[Vyžaduje]**  Šifrování &#124; AlreadyEncrypted<br/> Hello hodnotu tohoto pole rozhodne které tooencrypt disku a které nejsou na. <br/><br/>**Šifrování**: nástroj bude formátování disku hello. Pokud je hodnota "FormatOption" pole "Format" Tato hodnota je požadovaná toobe "Šifrovat". Pokud v tomto případě je zadán "AlreadyEncrypted", způsobí to došlo k chybě "Při zadání formát je šifrování musí být zadaná také".<br/>**AlreadyEncrypted**: nástroj bude dešifrování jednotky hello pomocí hello BitLockerKey zadané v poli "ExistingBitLockerKey". Pokud je hodnota "FormatOption" pole "AlreadyFormatted", pak tato hodnota může být buď "Šifrovat" nebo "AlreadyEncrypted" |
| ExistingBitLockerKey | **[Vyžaduje]**  Pokud je hodnota "Šifrování" pole "AlreadyEncrypted"<br/> Hodnota Hello tohoto pole je hello klíč nástroje BitLocker, která souvisí s konkrétním diskem hello. <br/><br/>Toto pole by měl být ponecháno prázdné, pokud je hodnota hello "Šifrování" pole "Šifrovat".  Pokud v tomto případě není zadaný klíč nástroje BitLocker, způsobí to došlo k chybě "By neměl být určen klíč nástroje Bitlocker".<br/>  **Příklad**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Příprava disku úlohy importu

tooprepare jednotky pro úlohu importu volání hello WAImportExport nástroj s hello **PrepImport** příkaz. Parametry, které zahrnete, závisí na tom, jestli to je hello první kopie relaci nebo relaci další kopie.

### <a name="first-session"></a>První relaci

První kopie relace tooCopy Single/Multiple Directory tooa jedním nebo několika WAImportExport disku (v závislosti na tom, co je určena v souboru CSV) nástroj PrepImport příkaz pro hello nejdříve zkopírovat relace toocopy adresáře nebo soubory s novou relací kopie:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Příklad:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Přidat data v následných relace

V další kopie relace není nutné toospecify hello počátečních parametrů. Je třeba toouse hello stejného deníku souboru v pořadí pro hello nástroj tooremember místa v hello předchozí relace. Hello stavu relace kopie hello se zapíše soubor toohello deníku. Tady je syntaxe hello kopii následné relace toocopy další adresáře nebo soubory:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Příklad:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-toolatest-session"></a>Přidání jednotky toolatest relace

Pokud hello data se nevejdou jednotky v InitialDriveset, jeden použít hello nástroj tooadd další jednotky toosame kopie relace. 

>[!NOTE] 
>id relace Hello by měl odpovídat id hello předchozí relace. Soubor deníku by měl odpovídat hello uvedené v předchozí relaci.
>
```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
```

**Příklad:**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-hello-latest-session"></a>Abort hello nejnovější relace:

Pokud dojde k přerušení relace kopírování a není možné tooresume (například pokud je zdrojový adresář prokázat nedostupné), je nutné přerušit hello aktuální relace tak, aby se vrátit zpět a nové kopie relace lze spustit:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Příklad:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Pouze hello poslední kopie relace, pokud byl ukončen neobvyklým způsobem, můžete přerušena. Všimněte si, že jste nelze přerušit hello první relaci kopie pro jednotku. Místo toho je nutné restartovat hello kopie relace pomocí nového souboru deníku.

### <a name="resume-a-latest-interrupted-session"></a>Obnovte nejnovější dojde k přerušení relace

Pokud z nějakého důvodu dojde k přerušení relace kopírování, můžete ho obnovit tak, že spustíte nástroj hello s pouze hello deníku zadaný soubor:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Příklad:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> Při obnovení relace kopie neměňte přidáním nebo odebráním soubory hello zdrojových datových souborů a adresářů.

## <a name="waimportexport-parameters"></a>Parametry WAImportExport

| Parametry | Popis |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Požadované**<br/> Cesta toohello deníku soubor. Soubor deníku sleduje sadu jednotky a záznamy hello průběh při přípravě tyto disky. je vždy třeba zadat soubor deníku Hello.  |
|     /logdir:&lt;LogDirectory&gt;  | **Volitelné**. Adresář protokolu Hello.<br/> Podrobné soubory protokolů, jakož i některé dočasné soubory, bude napsán toothis adresáře. Pokud není zadaný, aktuální adresář se použije jako adresář protokolu hello. Hello adresář protokolu lze zadat pouze jednou pro hello stejný soubor deníku.  |
|     /ID:&lt;SessionId&gt;  | **Požadované**<br/> Hello relace Id je použité tooidentify relaci kopírování. Je přesné obnovení použité tooensure kopie dojde k přerušení relace.  |
|     / ResumeSession  | Volitelné. Pokud hello poslední kopie relace byla ukončena abnormálním způsobem, tento parametr může být zadaný tooresume hello relace.   |
|     / AbortSession  | Volitelné. Pokud hello poslední kopie relace byla ukončena abnormálním způsobem, tento parametr může být zadaný tooabort hello relace.  |
|     /sn:&lt;StorageAccountName&gt;  | **Požadované**<br/> Platí pouze pro RepairImport a RepairExport. Hello název účtu úložiště hello.  |
|     /Sk:&lt;StorageAccountKey&gt;  | **Požadované**<br/> Hello klíč účtu úložiště hello. |
|     / InitialDriveSet:&lt;driveset.csv&gt;  | **Požadované** při spuštění hello první relaci kopie<br/> Soubor CSV, který obsahuje seznam tooprepare jednotky.  |
|     / AdditionalDriveSet:&lt;driveset.csv&gt; | **Požadované**. Při přidávání jednotky toocurrent kopie relace. <br/> Soubor CSV, který obsahuje seznam další jednotky toobe přidat.  |
|      / r:&lt;RepairFile&gt; | **Požadované** platí pouze pro RepairImport a RepairExport.<br/> Cesta k souboru toohello pro sledování průběhu opravy. Každé jednotky, musí mít pouze jeden soubor opravit.  |
|     / d:&lt;TargetDirectories&gt; | **Požadované**. Platí pouze pro RepairImport a RepairExport. Pro RepairImport jeden nebo více oddělených středníkem adresáře toorepair; Pro RepairExport, jeden adresář toorepair, například kořenový adresář jednotky hello.  |
|     / CopyLogFile:&lt;DriveCopyLogFile&gt; | **Požadované** platí pouze pro RepairImport a RepairExport. Soubor protokolu kopie disku toohello cesta (podrobný nebo chyba).  |
|     / ManifestFile:&lt;DriveManifestFile&gt; | **Požadované** platí pouze pro RepairExport.<br/> Cesta toohello jednotky soubor manifestu.  |
|     / PathMapFile:&lt;DrivePathMapFile&gt; | **Volitelné**. Platí pouze pro RepairImport.<br/> Cesta toohello souboru, který obsahuje mapování souboru cesty relativní toohello jednotky kořenové toolocations skutečné souborů (oddělený tabulátory). Pokud nejprve zadána, ho bude možné naplnit cesty k souborům s prázdnou cíle, to znamená buď nebyly nalezeny v TargetDirectories přístup odepřen s neplatným názvem, nebo existují v několika adresářích. souboru s mapováním Hello cesta může být ručně upravená tooinclude hello správné cílové cesty a znovu pro cesty k souborům hello hello nástroj tooresolve správně zadán.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Požadované**. Platí pouze pro PreviewExport.<br/> Cesta toohello XML soubor obsahující seznam objektů blob cesty nebo objektu blob předpony cestu pro toobe objekty BLOB hello exportovali. Formát souboru Hello je hello stejný jako formát hello blob seznamu objektů blob v hello operaci Put úlohy hello importu/exportu služby REST API.  |
|     / DriveSize:&lt;DriveSize&gt; | **Požadované**. Platí pouze pro PreviewExport.<br/>  Velikost jednotky toobe používá pro export. Například 500 GB, 1,5 TB. Poznámka: 1 GB = 1 000 000 000 bytes1 TB = 1,000,000,000,000 bajtů  |
|     Nebo datové sady:&lt;dataset.csv&gt; | **Požadované**<br/> Soubor CSV, který obsahuje seznam adresářů a/nebo seznam toobe soubory zkopírovat tootarget jednotky.  |
|     /silentmode  | **Volitelné**.<br/> Pokud není zadáno, zobrazí se upozornění hello požadavek jednotek a potřebovat vaše toocontinue potvrzení.  |

## <a name="tool-output"></a>Výstup nástroje

### <a name="sample-drive-manifest-file"></a>Ukázku souboru manifestu jednotky

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DriveManifest Version="2011-MM-DD">
   <Drive>
      <DriveId>drive-id</DriveId>
      <StorageAccountKey>storage-account-key</StorageAccountKey>
      <ClientCreator>client-creator</ClientCreator>
      <!-- First Blob List -->
      <BlobList Id="session#1-0">
         <!-- Global properties and metadata that applies tooall blobs -->
         <MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>
         <PropertiesPath Hash="md5-hash">global-properties-file-path</PropertiesPath>
         <!-- First Blob -->
         <Blob>
            <BlobPath>blob-path-relative-to-account</BlobPath>
            <FilePath>file-path-relative-to-transfer-disk</FilePath>
            <ClientData>client-data</ClientData>
            <Length>content-length</Length>
            <ImportDisposition>import-disposition</ImportDisposition>
            <!-- page-range-list-or-block-list -->
            <!-- page-range-list -->
            <PageRangeList>
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
            </PageRangeList>
            <!-- block-list -->
            <BlockList>
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
            </BlockList>
            <MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>
            <PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>
         </Blob>
      </BlobList>
   </Drive>
</DriveManifest>
```

### <a name="sample-journal-file-xml-for-each-drive"></a>Ukázkový soubor deníku (XML) pro každou jednotku

```xml
[BeginUpdateRecord][2016/11/01 21:22:25.379][Type:ActivityRecord]
ActivityId: DriveInfo
DriveState: [BeginValue]
<?xml version="1.0" encoding="UTF-8"?>
<Drive>
   <DriveId>drive-id</DriveId>
   <BitLockerKey>*******</BitLockerKey>
   <ManifestFile>\DriveManifest.xml</ManifestFile>
   <ManifestHash>D863FE44F861AE0DA4DCEAEEFFCCCE68</ManifestHash> </Drive>
[EndValue]
SaveCommandOutput: Completed
[EndUpdateRecord]
```

### <a name="sample-journal-file-jrn-for-session-that-records-hello-trail-of-sessions"></a>Ukázkový soubor deníku (JRN) pro relace, který zaznamenává hello záznam relací

```
[BeginUpdateRecord][2016/11/02 18:24:14.735][Type:NewJournalFile]
VocabularyVersion: 2013-02-01
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.749][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
LogDirectory: F:\logs
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.754][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
StorageAccountKey: *******
[EndUpdateRecord]
```

## <a name="faq"></a>Nejčastější dotazy

### <a name="general"></a>Obecné

#### <a name="what-is-waimportexport-tool"></a>Co je nástroj WAImportExport?

Nástroj WAImportExport Hello je hello jednotky přípravy a nástroje pro opravu, můžete použít s hello služby Microsoft Azure Import/Export. Můžete použít tento nástroj toocopy data toohello pevné disky budete tooship tooan datového centra Azure. Po dokončení úlohy importu, můžete tento nástroj toorepair všechny objekty BLOB, které došlo k poškození, chyběl nebo konflikt s další objekty BLOB. Po přijetí hello jednotky z úlohy dokončené export, můžete použít tento nástroj toorepair všechny soubory, které došlo k poškození nebo na hello jednotky chybí.

#### <a name="how-does-hello-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>Jak funguje hello nepodporuje nástroj WAImportExport u více dir zdroje a disky?

Pokud velikost dat hello je větší než velikost disku hello, nástroj WAImportExport hello distribuovat optimálního hello data na discích hello. Hello data kopírování toomultiple disků lze provést v paralelní nebo postupně. Neexistuje žádné omezení počtu hello disky hello dat může být napsán toosimultaneously. Nástroj Hello provede distribuci dat na základě velikosti disku a velikost složky. Vybere hello disk, který je většina optimalizované pro objekt velikosti hello. Hello dat po odeslání toohello účet úložiště bude konvergované zpět toohello zadaná struktura adresářů.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>Kde najdu předchozí verzi nástroje WAImportExport?

WAImportExport nástroj obsahuje všechny funkce, které měl nástroj WAImportExport V1. WAImportExport nástroj umožňuje uživatelům toospecify více zdrojů a jednotky toomultiple zápisu. Kromě toho jeden, můžete snadno spravovat více zdrojových umístění, ze kterých je hello data toobe zkopírovali v jednom souboru CSV. Ale v případě musíte SAS podporu, nebo chcete toocopy jednoho zdroje toosingle disk, můžete [nástroj můžete stáhnout WAImportExport V1] (http://go.microsoft.com/fwlink/? LinkID = 301900&amp;clcid = 0x409) a odkazovat příliš[WAImportExport V1 odkaz](storage-import-export-tool-how-to-v1.md) nápovědu k použití WAImportExport V1.

#### <a name="what-is-a-session-id"></a>Co je ID relace?

Nástroj Hello očekává relace kopie hello (nebo id) stejný parametr toobe hello, pokud hello záměrem toospread hello dat na více disků. Zachování hello stejný název relace kopie hello povolí toocopy dat uživatele z jedné nebo více zdrojových umístění do jedné nebo více cílové disky nebo adresáře. Zachování stejné id relace umožňuje hello nástroj toopick až hello kopírování souborů z byl místa hello naposledy.

Stejné relace kopie však nemůže být účty používané tooimport data toodifferent úložiště.

Pokud název kopie relace je stejný napříč více spustí nástroj hello, hello souboru protokolu (/ logdir) a klíč účtu úložiště (nebo sk) je také očekávané toobe hello stejné.

ID relace se může skládat z písmen, 0 ~ 9, understore (\_), pomlčky (-) nebo hodnotu hash (#), a jeho délka musí být 3 ~ 30.

například relace 1 nebo č. 1 nebo relaci\_1

#### <a name="what-is-a-journal-file"></a>Co je soubor deníku?

Pokaždé, když spustíte hello WAImportExport nástroj toocopy soubory toohello pevného disku, nástroj hello vytvoří relaci kopírování. Hello stavu relace kopie hello se zapíše soubor toohello deníku. Pokud dojde k přerušení relaci kopírování (například kvůli výpadku napájení systému tooa), může být obnoveno spuštěním nástroje hello znovu a zadáte hello deníku soubor na příkazovém řádku hello.

Pro každý pevný disk, který připravit s hello nástroj Azure Import/Export, vytvoří nástroj hello soubor jednoho deníku s názvem "&lt;ID jednotky&gt;.xml" kde jednotka Id je sériové číslo hello k toohello jednotku, která hello nástroj čte z Hello disk. Je nutné soubory deníku hello ze všech jednotky toocreate hello import úlohy. Hello deníku soubor může být také použít tooresume jednotky přípravy Pokud dojde k přerušení nástroj hello.

#### <a name="what-is-a-log-directory"></a>Co je adresář protokolu?

Adresář protokolu Hello Určuje že adresář toobe používá toostore podrobné protokoly, jakož i dočasné soubory manifestu. Pokud není zadaný, aktuální adresář hello se použije jako adresář protokolu hello. Hello protokoly jsou podrobné protokoly.

### <a name="prerequisites"></a>Požadavky

#### <a name="what-are-hello-specifications-of-my-disk"></a>Jaké jsou specifikace hello Moje disku?

Jeden nebo více prázdný 2,5 nebo 3,5 SATAII o rychlosti nebo III nebo SSD pevné disky připojené toohello kopie počítače.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>Jak můžete povolit nástroj BitLocker na můj počítač?

Jednoduchý způsob toocheck je kliknutím pravým tlačítkem myši na systémové jednotce. Zobrazí se možnosti pro nástroj Bitlocker Pokud je zapnutá možnost hello. Pokud je vypnuto, se nebude zobrazovat.

![Zkontrolujte nástroj BitLocker](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Tady je článek na [jak tooenable nástroj BitLocker](https://technet.microsoft.com/library/cc766295.aspx)

Je možné, že váš počítač nemá čip TPM. Pokud se nezobrazí pomocí tpm.msc výstupu, podívejte se na nejčastější dotazy týkající se další hello.

#### <a name="how-toodisable-trusted-platform-module-tpm-in-bitlocker"></a>Jak toodisable důvěryhodné Platform Module (TPM) v nástroji BitLocker?
> [!NOTE]
> Pouze v případě, že je jejich serverech bez čipu TPM, je nutné toodisable TPM zásad. Není nutné toodisable TPM Pokud je v serveru uživatele důvěryhodné TPM. 
> 

V pořadí toodisable TPM v nástroji BitLocker projít hello následující kroky:<br/>
1. Spusťte **Editor zásad skupiny** zadáním gpedit.msc na příkazovém řádku. Pokud **Editor zásad skupiny** zobrazí toobe není k dispozici pro první povolení nástroje BitLocker. Viz předchozí – nejčastější dotazy.
2. Otevřete **zásady místního počítače &gt; konfigurace počítače &gt; šablony pro správu &gt; součásti systému Windows&gt; nástroj BitLocker Drive Encryption &gt; jednotky operačního systému**.
3. Upravit **vyžadovat další ověření při spuštění** zásad.
4. Nastavit zásady hello příliš**povoleno** a zajistěte, aby **povolit nástroj BitLocker bez kompatibilním čipem TPM** je zaškrtnuté.

####  <a name="how-toocheck-if-net-4-or-higher-version-is-installed-on-my-machine"></a>Jak toocheck, pokud je v počítači nainstalován .NET 4 nebo vyšší verze?

V následujícím adresáři jsou nainstalovány všechny verze rozhraní Microsoft .NET Framework: %windir%\Microsoft.NET\Framework\

Přejděte toohello výše uvedené části na váš cílový počítač, kde nástroj hello musí toorun. Vyhledejte název složky počínaje "v4". Neexistence takový adresář znamená, že v počítači není nainstalován .NET 4. Rozhraní .net 4 si můžete stáhnout na váš počítač pomocí [rozhraní Microsoft .NET Framework 4 (Webová instalační služba)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Omezení

#### <a name="how-many-drives-can-i-preparesend-at-hello-same-time"></a>Kolik jednotek můžete I příprava nebo odeslat v hello současně?

Neexistuje žádné omezení počtu hello disky, které hello nástroj můžete připravit. Nástroj hello však očekává písmena jednotek jako vstupy. Která se omezuje too25 přípravy disků současně. Jediné úlohy může zpracovávat maximálně 10 disků současně. Pokud připravujete víc než 10 disky cílení hello stejný účet úložiště, disky hello mohou být distribuovány na více úloh.

#### <a name="can-i-target-more-than-one-storage-account"></a>Můžete vybrat více než jeden účet úložiště?

Pouze jeden účet úložiště jde odeslat na úlohu a v relaci jedna kopie.

### <a name="capabilities"></a>Možnosti

#### <a name="does-waimportexportexe-encrypt-my-data"></a>Moje data šifrovat WAImportExport.exe?

Ano. Šifrování nástrojem BitLocker je povolen a požadované pro tento proces.

#### <a name="what-will-be-hello-hierarchy-of-my-data-when-it-appears-in-hello-storage-account"></a>Co když se objeví v účtu úložiště hello bude hello hierarchie svá data?

I když distribuci dat mezi disky hello dat po odeslání toohello účet úložiště bude možné konvergované zpět toohello adresářovou strukturu zadaný v souboru CSV hello datovou sadu.

#### <a name="how-many-of-hello-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Kolik hello vstup, že disky mají active vstupně-výstupní operace, paralelní, když probíhá kopírování?

Nástroj Hello distribuuje data v hello vstupní disky na základě velikosti hello hello vstupních souborů. Ale nutné dodat, hello počet active disků paralelně úplně delends na hello povaha hello vstupní data. V závislosti na velikosti hello jednotlivých souborů v hello vstupní datovou sadu může jeden nebo více disků zobrazovat active vstupně-výstupní operace paralelně. Najdete v části Další otázka další podrobnosti.

#### <a name="how-does-hello-tool-distribute-hello-files-across-hello-disks"></a>Jak hello nástroj distribuovat soubory hello mezi hello disky?

Nástroj WAImportExport čte a zapisuje soubory batch batch, jeden batch obsahuje maximální počet souborů 100000. To znamená, že maximální 100000 lze zapisovat soubory současně. Více disků se budou zapisovat toosimultaneously, pokud jsou tyto soubory 100000 distribuované toomulti jednotky. Ale jestli hello nástroj zapisuje toomultiple disků současně nebo na jediný disk závisí na hello kumulativní velikost dávky hello. Například v případě menších souborů, pokud jsou všechny soubory 10,0000 možné toofit do jedné jednotky, nástroj zapisuje tooonly jeden disk během zpracování hello tuto dávku.

### <a name="waimportexport-output"></a>Výstup WAImportExport

#### <a name="there-are-two-journal-files-which-one-should-i-upload-tooazure-portal"></a>Existují dva soubory deníku, která by I nahrajte tooAzure portál?

**.xml** – pro každý pevný disk, který připravit nástrojem WAImportExport hello, vytvoří nástroj hello soubor jednoho deníku s názvem `<DriveID>.xml` ID jednotky je hello sériové číslo související toohello jednotku, která hello nástroj čtení z disku hello. Je nutné soubory deníku hello ze všech úlohy jednotky toocreate hello importu v hello portálu Azure. Tento soubor deníku může být také použít tooresume jednotky přípravy, pokud bude přerušen přívod hello nástroj.

**.jrn** -hello deníku soubor s příponou `.jrn` obsahuje hello stav pro všechny relace kopie pro pevný disk. Také obsahuje informace hello potřeby toocreate hello úloha importu. Při spuštěné hello WAImportExport nástroj, jakož i relaci kopie ID musíte vždycky zadat soubor deníku

## <a name="next-steps"></a>Další kroky

* [Hello nastavení se nástroj Azure Import/Export](storage-import-export-tool-setup.md)
* [Vlastnosti nastavení a metadata během hello importu](storage-import-export-tool-setting-properties-metadata-import.md)
* [Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [Stručná referenční příručka pro často používané příkazy](storage-import-export-tool-quick-reference.md) 
* [Kontrola stavu úlohy s použitím kopií souborů protokolu](storage-import-export-tool-reviewing-job-status-v1.md)
* [Oprava úlohy importu](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Oprava úlohy exportu](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Řešení potíží s hello nástroj Azure Import/Export](storage-import-export-tool-troubleshooting-v1.md)
