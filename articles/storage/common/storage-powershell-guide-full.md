---
title: "aaaUsing prostředí Azure PowerShell s Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toouse hello rutin Azure Powershellu pro Azure Storage toocreate a spravovat účty úložiště; Práce s objekty BLOB, tabulky, fronty a soubory; Nakonfigurujte a dotaz analytika úložiště a vytvořte sdílených přístupových podpisů."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 17d638e741911ceafb9777d5c2fce7bfe533e50c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Použití Azure Powershell s Azure Storage
## <a name="overview"></a>Přehled
Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell. Je to prostředí příkazového řádku založené na úlohách a skriptovací jazyk určený speciálně pro správu systému. Pomocí prostředí PowerShell můžete snadno řídit a automatizovat správu hello Azure služby a aplikace. Například můžete použít hello rutiny tooperform hello stejné úlohy, že můžete provádět prostřednictvím hello [portál Azure](https://portal.azure.com).

V této příručce, jsme budete prozkoumat jak toouse hello [rutiny úložiště Azure](/powershell/module/azurerm.storage/#storage) tooperform řadu vývoj a správu úloh s Azure Storage.

Tato příručka předpokládá, že máte zkušenosti s použitím [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) a [prostředí Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Příručka Hello obsahuje řadu skriptů toodemonstrate hello použití prostředí PowerShell s Azure Storage. Proměnné skriptu hello na základě vaší konfigurace před spuštěním každý skript by měl aktualizovat.

první část Hello v této příručce poskytuje stručný přehled Azure Storage a prostředí PowerShell. Podrobné informace a pokyny, spusťte z hello [požadavky pro použití prostředí Azure PowerShell s Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Začínáme s Azure Storage a prostředí PowerShell během 5 minut
V této části se dozvíte, jak tooaccess Azure Storage pomocí prostředí PowerShell během 5 minut.

**Nové tooAzure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému. Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).

V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.

**Po vytvoření odběru služby Microsoft Azure a účet:**

1. Stáhněte a nainstalujte nejnovější hello [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).
2. Spuštění Windows PowerShell integrované skriptování prostředí (ISE): V místním počítači, přejděte toohello **spustit** nabídky. Typ **nástroje pro správu** a klikněte na tlačítko toorun ho. V hello **nástroje pro správu** okna, klikněte pravým tlačítkem na **Windows PowerShell ISE**, klikněte na tlačítko **spustit jako správce**.
3. V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** toocreate nový soubor skriptu.
4. Nyní sdělíme vám jednoduchý skript, který zobrazuje základní tooaccess příkazy prostředí PowerShell Azure Storage. skript Hello požádá vaší tooadd přihlašovací údaje účtu Azure nejprve váš účet Azure toohello místní prostředí PowerShell. Potom hello skript nastavit výchozí hello předplatného Azure a vytvořte nový účet úložiště v Azure. V dalším kroku hello skript vytvořit nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující kontejner toothat bitové kopie souboru (binární rozsáhlý objekt). Po hello skript obsahuje seznam všech objektů BLOB v kontejneru, vytvoří nový cílový adresář v místním počítači a stáhnout soubor bitové kopie hello.
5. V následující části kódu hello, vyberte skript hello mezi hello poznámky **#begin** a **#end**. Stiskněte kombinaci kláves CTRL + C toocopy ho toohello schránky.

    ```powershell
    # begin
    # Update with hello name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name tooyour new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name tooyour new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account toohello local PowerShell environment.
    Add-AzureAccount
       
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
       
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
       
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
       
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
       
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
       
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
       
    # Download blobs from hello container:
    # Get a reference tooa list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create hello destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into hello local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. V **Windows PowerShell ISE**, stiskněte kombinaci kláves CTRL + V toocopy hello skriptu. Klikněte na tlačítko **soubor** > **Uložit**. V hello **uložit jako** dialogového okna, název typu hello hello souboru skriptu, jako je například "mystoragescript." Klikněte na **Uložit**.
7. Nyní musíte tooupdate hello skriptu proměnné na základě svého nastavení konfigurace. Je nutné aktualizovat hello **$SubscriptionName** proměnné pomocí svého vlastního předplatného. Můžete ponechat hello jiné proměnné zadané v hello skriptu nebo je aktualizovat podle potřeby.
   
   * **$SubscriptionName:** musíte aktualizovat tuto proměnnou s vlastní název odběru. Postupujte podle jednoho z hello následující tři způsoby toolocate hello název předplatného:
     
    a. V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** toocreate nový soubor skriptu. Kopírování hello následující skript toohello nový soubor skriptu a klikněte na tlačítko **ladění** > **spustit**. Hello následující skript se nejdřív požádat vašeho tooadd přihlašovací údaje účtu Azure váš účet Azure toohello místní prostředí PowerShell a pak zobrazíte všechny hello odběry, které jsou připojené toohello místní relaci prostředí PowerShell. Poznamenejte si název hello hello předplatného, které chcete toouse během provádění v tomto kurzu:
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    b. toolocate a zkopírujte vaše předplatné názvu hello [portál Azure](https://portal.azure.com), v nabídce centra v levé hello hello, klikněte na **odběry**. Zkopírujte hello název odběru, které chcete toouse při spouštění skriptů hello v této příručce.
     
     ![portál Azure](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    c. toolocate a zkopírujte vaše předplatné názvu hello [portálu Azure Classic](https://manage.windowsazure.com/), posuňte se dolů a klikněte na **nastavení** na levé straně hello portálu hello. Klikněte na tlačítko **odběry** toosee seznam předplatných. Zkopírujte hello název odběru, který chcete toouse při spouštění skriptů hello zadané v této příručce.
     
     ![portál Azure Classic](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * **$StorageAccountName:** použít hello zadané ve skriptu hello název nebo zadejte nový název pro účet úložiště. **Důležité:** hello název účtu úložiště hello musí být jedinečný v Azure. Musí být malými písmeny, příliš!
   * **$Location:** použít zadané "Západní USA" hello ve skriptu hello nebo zvolte jiné umístění Azure, jako například Východ USA, Severní Evropa a tak dále.
   * **$ContainerName:** použít hello zadané ve skriptu hello název nebo zadejte nový název pro váš kontejner.
   * **$ImageToUpload:** zadejte obrázku tooa cestu v místním počítači, jako například: "C:\Images\HelloWorld.png".
   * **$DestinationFolder:** Enter toostore souborů cesta tooa místního adresáře stáhli ze služby Azure Storage, například: "C:\DownloadImages".
8. Po aktualizaci proměnné hello skript v souboru "mystoragescript.ps1" hello, klikněte na tlačítko **soubor** > **Uložit**. Potom klikněte na **ladění** > **spustit** nebo stiskněte klávesu **F5** toorun hello skriptu.  

Po spuštění skriptu hello byste měli mít místní cílovou složku, která zahrnuje hello stáhnout soubor bitové kopie. Hello následující snímek obrazovky ukazuje příklad výstupu:

![Stáhnout objekty BLOB](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> Hello "Začínáme s Azure Storage a prostředí PowerShell během 5 minut" části poskytuje rychlý úvod o toouse prostředí Azure PowerShell s Azure Storage. Podrobné informace a pokyny doporučujeme vám tooread hello následující části.
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Požadavky pro použití prostředí Azure PowerShell s Azure Storage
Je nutné předplatné a účet toorun hello rutiny Azure PowerShell uvedených v této příručce, jak je popsáno výše.

Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell. Informace o instalaci a nastavení prostředí Azure PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Doporučujeme stáhnout a nainstalovat nebo upgradovat nejnovější modul Azure PowerShell toohello před použitím tohoto průvodce.

Spuštěním rutiny hello v konzole Windows PowerShell standardní hello nebo hello Windows PowerShell Integrované skriptovací prostředí (ISE). Například tooopen **Windows PowerShell ISE**přejděte toohello nabídky Start, nástroje pro správu a klikněte na toorun ho. V okně hello nástroje pro správu klikněte pravým tlačítkem na Windows PowerShell ISE, klikněte na tlačítko Spustit jako správce.

## <a name="how-toomanage-storage-accounts-in-azure"></a>Jak účtů toomanage úložiště v Azure

Podívejme se na správu účty úložiště v Azure pomocí prostředí PowerShell

### <a name="how-tooset-a-default-azure-subscription"></a>Jak tooset výchozí předplatné Azure
toomanage Azure Storage pomocí Azure PowerShell, je nutné tooauthenticate prostředí klienta s nástrojem Azure přes Azure Active Directory ověřování nebo ověřování pomocí certifikátů. Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kurzu. Tento průvodce pomocí ověřování Azure Active Directory hello.

1. V systému Windows PowerShell ISE zadejte následující příkaz tooadd hello váš účet Azure toohello místní prostředí PowerShell:

    ```powershell
    Add-AzureAccount
    ```

2. V okně "Přihlásit tooMicrosoft Azure" hello, typ hello e-mailovou adresu a heslo spojené s vaším účtem. Azure ověřuje a uloží hello přihlašovací údaje a potom zavře okno hello.

3. Potom spustíte následující příkaz tooview hello Azure účty ve vašem místním prostředí PowerShell a zkontrolujte, zda váš účet hello:
   
    ```powershell
    Get-AzureAccount
    ```
4. Potom spusťte následující rutiny tooview hello všechny hello odběry, které jsou připojené toohello místní relaci prostředí PowerShell a zkontrolujte, zda vaše předplatné:

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. tooset výchozí předplatné Azure, spusťte rutinu hello Select-AzureSubscription:

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. Ověřte název hello hello výchozí předplatné spuštěním rutiny Get-AzureSubscription hello:

    ```powershell
    Get-AzureSubscription -Default
    ```

7. toosee všechny hello k dispozici rutiny prostředí PowerShell pro Azure Storage, spusťte:
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-toocreate-a-new-azure-storage-account"></a>Jak toocreate nový účet úložiště Azure
toouse úložiště Azure, budete potřebovat účet úložiště. Po nakonfigurování vaše počítače tooconnect tooyour předplatné, můžete vytvořit nový účet úložiště Azure.

1. Všechny dostupné datacenter umístění hello spouštění toofind rutiny Get-AzureLocation hello:

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. Dále proveďte toocreate rutiny New-AzureStorageAccount hello nový účet úložiště. Hello následující příklad vytvoří nový účet úložiště v datovém centru hello "Západní USA".
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> Hello název svého účtu úložiště musí být jedinečný v rámci Azure a musí být psaný malými písmeny. Zásady vytváření názvů a omezení najdete v tématu [o účtech úložiště Azure](../storage-create-storage-account.md) a [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](http://msdn.microsoft.com/library/azure/dd135715.aspx).
> 
> 

### <a name="how-tooset-a-default-azure-storage-account"></a>Jak tooset výchozí účet úložiště Azure
Můžete mít více účtů úložiště v rámci vašeho předplatného. Můžete zvolit jeden z nich a nastavte ji jako hello výchozí účet úložiště pro všechny hello úložiště příkazů v hello stejné relace prostředí PowerShell. To umožňuje toorun hello prostředí Azure PowerShell úložiště příkazy bez zadání kontext úložiště hello explicitně.

1. tooset výchozí účet úložiště pro vaše předplatné, můžete spustit rutinu Set-AzureSubscription hello.

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. Dále proveďte tooverify rutiny Get-AzureSubscription hello, je přiřazená k vašemu účtu předplatného výchozí účet úložiště hello. Tento příkaz vrátí hello vlastnosti odběru na hello aktuální předplatného, včetně jeho aktuální účet úložiště.

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-toolist-all-azure-storage-accounts-in-a-subscription"></a>Jak toolist všechny Azure účtů úložiště v předplatném.
Každé předplatné Azure může mít too100 úložiště účtů. Hello nejaktuálnější informace o limitech najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).

Spusťte následující rutinu toofind hello název a stav hello účty úložiště v aktuálním předplatném hello hello:

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-toocreate-an-azure-storage-context"></a>Jak toocreate kontextu úložiště Azure
Kontext úložiště Azure je objekt v prostředí PowerShell tooencapsulate hello úložiště pověření. Použití úložiště kontextu při spuštění libovolné další rutiny umožňuje vám tooauthenticate vaši žádost o bez zadání hello účet úložiště a jeho přístupový klíč explicitně. Kontext úložiště můžete vytvořit v mnoha způsoby, například pomocí názvu a přístupový klíč účtu úložiště, sdílený přístupový podpis (SAS) token, připojovací řetězec, nebo anonymní. Další informace najdete v tématu [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).  

Použijte jednu z následujících tří způsobů toocreate hello kontext úložiště:

* Spustit hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) toofind rutiny se hello primárního úložiště přístupový klíč pro účet úložiště Azure. Pak zavolejte hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) toocreate rutiny kontext úložiště:

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* Vygenerování tokenu sdíleného přístupového podpisu pro kontejner úložiště Azure a použít ho toocreate kontext úložiště:

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    Další informace najdete v tématu [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) a [pomocí sdíleného přístupového podpisy (SAS)](../storage-dotnet-shared-access-signature-part-1.md).

* V některých případech můžete koncové body služby hello toospecify, když vytvoříte nový kontext úložiště. To může být nezbytné, pokud jste registrovali vlastního názvu domény pro váš účet úložiště s hello služby objektů Blob nebo chcete toouse sdílený přístupový podpis pro přístup k prostředkům úložiště. Nastavit koncové body služby hello v připojovacím řetězci a použít ho toocreate nový kontext úložiště, jak je uvedeno níže:

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

Další informace o tom, najdete v části tooconfigure připojovacího řetězce úložiště [konfiguraci připojovacích řetězců](../storage-configure-connection-string.md).

Teď, když máte v počítači a se dozvěděli, jak toomanage předplatných a účtů úložiště pomocí prostředí Azure PowerShell přejděte toohello další části toolearn jak objekty BLOB toomanage Azure a objekt blob snímky.

### <a name="how-tooretrieve-and-regenerate-azure-storage-keys"></a>Jak tooretrieve a znovu vygenerovat klíče úložiště Azure
Účet úložiště Azure obsahuje dva klíče účtu. Můžete použít následující rutiny tooretrieve hello klíče.

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

Pomocí následující rutiny tooretrieve konkrétního klíče hello. Platné hodnoty jsou primární i sekundární.  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

Pokud chcete tooregenerate klíče, použijte následující rutinu hello. Typ_klíče - platné hodnoty jsou "Primární" a "Sekundární"

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-toomanage-azure-blobs"></a>Jak objekty BLOB toomanage Azure
Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS. V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště objektů Blob Azure hello. Podrobné informace najdete v tématu [Začínáme s úložištěm Blob pomocí rozhraní .NET](../blobs/storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-toocreate-a-container"></a>Jak toocreate kontejner
Každý objekt blob v úložišti Azure musí být v kontejneru. Můžete vytvořit kontejner privátní, pomocí rutiny New-AzureStorageContainer hello:

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**. tooprevent anonymní přístup k tooblobs parametru oprávnění sady hello příliš**vypnout**. Ve výchozím nastavení nový kontejner hello je privátní a je přístupný jenom vlastník účtu hello. čtení tooallow anonymní veřejný přístup k prostředkům tooblob, ale není toocontainer metadata nebo toohello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**Blob**. čtení tooallow úplné veřejný přístup k prostředkům tooblob, metadata kontejneru a hello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**kontejneru**. Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md).
> 
> 

### <a name="how-tooupload-a-blob-into-a-container"></a>Jak tooupload objekt blob do kontejneru
Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky. Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

tooupload objekty BLOB v kontejneru tooa, můžete použít hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny. Ve výchozím nastavení odešle tento příkaz hello objekt blob bloku tooa místní soubory. Typ hello toospecify pro hello blob, můžete pomocí parametru - BlobType hello.

Vítejte v následujícím příkladu spustí hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) tooget rutiny hello všechny soubory v zadané složce hello a pak předá je další rutiny toohello pomocí operátor kanálu hello. Hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny odešle hello místních souborů tooyour kontejneru:

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-toodownload-blobs-from-a-container"></a>Jak toodownload objektů blob z kontejneru
Hello následující příklad ukazuje, jak z kontejneru objektů BLOB toodownload. Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho primární přístupový klíč. Potom hello příklad načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny. V dalším kroku hello příklad používá hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) objekty BLOB toodownload rutiny do hello místní cílovou složku.

```powershell
#Define hello variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-toocopy-blobs-from-one-storage-container-tooanother"></a>Jak toocopy objektů blob z jednoho tooanother kontejneru úložiště
Mezi účty úložiště a oblastí, můžete zkopírovat objekty BLOB asynchronně. Hello následující příklad ukazuje, jak toocopy objektů blob z jednoho úložiště kontejneru tooanother ve dvou jiným účtům úložiště. Příklad Hello nejprve nastaví proměnné pro zdrojové a cílové účty úložiště a potom vytvoří kontext úložiště pro každý účet. V dalším kroku hello příkladu se zkopíruje objekty BLOB z hello zdrojový kontejner toohello cílový kontejner pomocí hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny. Hello příklad předpokládá, že účty hello zdrojového a cílového úložiště a kontejnerů, již existuje.

```powershell
#Define hello source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define hello destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference tooblobs in hello source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container tooanother.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

Pamatujte, že v tomto příkladu asynchronní kopírování. Můžete sledovat stav hello každé kopie spuštěním hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) rutiny.

### <a name="how-toocopy-blobs-from-a-secondary-location"></a>Jak toocopy objekty BLOB ze sekundární lokality
Objekty BLOB můžete zkopírovat z hello sekundárního umístění účtu povoleno RA-GRS.

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-toodelete-a-blob"></a>Jak toodelete objektu blob
toodelete objekt blob, nejdřív získejte odkaz na objekt blob a potom na něm volat rutinu Remove-AzureStorageBlob hello. Následující ukázka Hello odstraní všechny objekty BLOB hello v daném kontejneru. Příklad Hello nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště. V dalším kroku hello příklad načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí hello [odebrat AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) objekty BLOB tooremove rutiny z kontejneru v úložišti Azure.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooall hello blobs in hello container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-toomanage-azure-blob-snapshots"></a>Jak toomanage Azure blob snímky
Azure umožňuje vytvoření snímku objektu blob. Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase. Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně. Snímky zadejte tooback způsob, jak se objekt blob, jak se objevuje v časovém okamžiku. Další informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-toocreate-a-blob-snapshot"></a>Jak toocreate snímku objektů blob
toocreate snímek objektu blob, nejdřív získejte odkaz na objekt blob a pak zavolají hello `ICloudBlob.CreateSnapshot` metoda na něm. Hello následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště. V dalším kroku hello příklad načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoda toocreate snímku.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of hello blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-toolist-a-blobs-snapshots"></a>Jak toolist objekt blob pro snímky
Můžete vytvořit libovolný počet snímků, jak chcete použít pro objekt blob. Můžete vytvořit seznam hello snímky přidružené k vaší blob tootrack vaše aktuální snímky. Hello následující příklad používá předdefinovaných objektů blob a volání hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny toolist hello snímky tomuto objektu blob.  

```powershell
#Define hello blob name.
$BlobName = "yourblobname"

#List hello snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-toocopy-a-snapshot-of-a-blob"></a>Jak toocopy snímek objektu blob
Můžete zkopírovat snímek snímku hello toorestore objektů blob. Omezení a podrobné informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Hello následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště. V dalším kroku hello příklad definuje název proměnné hello kontejnerů a objektů blob. Příklad Hello načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoda toocreate snímku. Potom hello příklad spustí hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny toocopy hello snímku objektu blob pomocí hello ICloudBlob objektu pro objekt hello zdroje blob. Být opravdu tooupdate hello proměnné na základě vaší konfigurace před spuštěné příklad hello. Všimněte si, že hello následující příklad předpokládá, že hello zdrojové a cílové kontejnery a hello zdrojový objekt blob již existují.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define hello variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy hello snapshot tooanother container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

Teď, když jste se naučili, jak objekty BLOB toomanage Azure a objektů blob snímky s prostředím Azure PowerShell, přejděte toohello další části toolearn jak toomanage tabulky, fronty a soubory.

## <a name="how-toomanage-azure-tables-and-table-entities"></a>Jak toomanage Azure tabulky a tabulka entity
Služby úložiště Azure Table je úložiště dat typu NoSQL, které můžete použít toostore a dotazování obrovských sad strukturovaných, nerelačních data. hlavní součásti Hello hello služby jsou tabulky, entit a vlastnosti. Tabulka je kolekce entit. Entita je sada vlastností. Každá entita může mít too252 vlastností, které jsou všechny páry název hodnota. V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště Azure Table hello. Podrobné informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx) a [Začínáme s Azure Table storage pomocí rozhraní .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).

V hello následující témata se dozvíte, jak služba toomanage Azure Table storage pomocí Azure PowerShell. Hello pokryté scénáře zahrnují **vytváření**, **odstraňování**, a **načítání** **tabulky**, a také **přidání**, **dotazování**, a **odstranění entity tabulky**.

### <a name="how-toocreate-a-table"></a>Jak toocreate tabulku
Každá tabulka musí nacházet v účtu úložiště Azure. Hello následující příklad ukazuje, jak toocreate tabulku ve službě Azure Storage. Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč. Dále je použita hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutiny toocreate tabulku ve službě Azure Storage.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-tooretrieve-a-table"></a>Jak tooretrieve tabulku
Můžete zadat dotaz a načtení jedné nebo všech tabulek v účtu úložiště. Hello následující příklad ukazuje, jak tooretrieve dané tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

Při volání rutiny Get-AzureStorageTable hello bez parametrů, získá všechny tabulky úložiště pro účet úložiště.

### <a name="how-toodelete-a-table"></a>Jak toodelete tabulku
Můžete odstranit tabulku z účtu úložiště pomocí hello [odebrat AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) rutiny.  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-toomanage-table-entities"></a>Jak toomanage tabulka entity
V současné době prostředí Azure PowerShell neposkytuje entity tabulky toomanage rutiny přímo. operace tooperform na entity tabulky, můžete použít hello třídy zadaná v hello [Klientská knihovna pro úložiště Azure pro .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-tooadd-table-entities"></a>Jak tooadd tabulka entity
tooadd tooa tabulka entity, nejprve vytvořit objekt, který definuje vlastnosti vaší entity. Entita může mít too255 vlastností, včetně 3 Vlastnosti systému: **PartitionKey**, **RowKey**, a **časové razítko**. Jste zodpovědní za vložení a aktualizace hodnoty hello **PartitionKey** a **RowKey**. Hello server spravuje hello hodnotu **časové razítko**, který nemůže být upraven. Společně hello **PartitionKey** a **RowKey** jednoznačně identifikovat každou entitu v tabulce.

* **PartitionKey**: Určuje hello oddíl, který hello entity je uložen v.
* **RowKey**: jednoznačně identifikuje hello entity v rámci oddílu hello.

Můžete definovat vlastní vlastností too252 pro entitu. Další informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Hello následující příklad ukazuje, jak tooadd entity tooa tabulky. Hello příklad ukazuje, jak tooretrieve hello tabulky zaměstnanců a přidejte do ní několik entit. Nejprve navazuje připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč. V dalším kroku se načte hello zadané tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny. Pokud hello tabulka neexistuje, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutina je použité toocreate tabulku ve službě Azure Storage. V dalším kroku hello příklad definuje vlastní funkci Přidat Entity tooadd entity toohello tabulku zadáním každé entity oddílu a klíč řádku. hello volání funkce Hello přidat Entity [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na hello rutinu [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) toocreate třída objektu entity. Později, hello ukázka volá hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) metoda na tento objekt tooadd entity ho tooa tabulky.

```powershell
#Function Add-Entity: Adds an employee entity tooa table.
function Add-Entity() {
    [CmdletBinding()]
    param(
       $table,
       [String]$partitionKey,
       [String]$rowKey,
       [String]$name,
       [Int]$id
    )

  $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
  $entity.Properties.Add("Name", $name)
  $entity.Properties.Add("ID", $id)

  $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
}

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve hello table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities tooa table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-tooquery-table-entities"></a>Jak tooquery tabulka entity
tooquery tabulky, použijte hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) třídy. Hello následující příklad předpokládá, že jste již spuštění skriptu hello uvedenému v hello jak tooadd entity části této příručky. Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí hello úložiště kontext, který obsahuje název účtu úložiště hello a jeho přístupový klíč. V dalším kroku se pokusí o tooretrieve hello vytvořili "zaměstnanci" tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny. Volání hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na hello Microsoft.WindowsAzure.Storage.Table.TableQuery třída rutinu vytvoří nový objekt dotazu. Příklad Hello hledá hello entity, které mají na sloupec 'ID', jehož hodnota je 1 zadané v řetězec filtru. Podrobné informace najdete v tématu [dotazování tabulky a entity](http://msdn.microsoft.com/library/azure/dd894031.aspx). Při spuštění tohoto dotazu vrátí všechny entity, které odpovídají kritériím filtru hello.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference tooa table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns tooselect.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute hello query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with hello table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-toodelete-table-entities"></a>Jak toodelete tabulka entity
Můžete odstranit pomocí jeho klíče oddílu a řádku entity. Hello následující příklad předpokládá, že jste již spuštění skriptu hello uvedenému v hello jak tooadd entity části této příručky. Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí hello úložiště kontext, který obsahuje název účtu úložiště hello a jeho přístupový klíč. V dalším kroku se pokusí o tooretrieve hello vytvořili "zaměstnanci" tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny. Pokud tabulka hello existuje, zavolá hello příklad hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) tooretrieve metoda entity podle jeho oddílu a řádku klíčové hodnoty. Pak předejte hello entity toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) toodelete metoda.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If hello table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together hello PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete hello entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-toomanage-azure-queues-and-queue-messages"></a>Jak toomanage Azure fronty a zprávy ve frontě
Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště fronty Azure hello. Podrobné informace najdete v tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](../storage-dotnet-how-to-use-queues.md).

V této části se dozvíte, jak služba toomanage Azure Queue storage pomocí Azure PowerShell. Hello pokryté scénáře zahrnují **vkládání** a **odstraňování** fronty zpráv, a také **vytváření**, **odstraňování**a **načítání fronty**.

### <a name="how-toocreate-a-queue"></a>Jak toocreate fronty
Hello následující příklad nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč. Dále volá [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) toocreate rutiny frontu s názvem 'queuename'.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

Informace o vytváření názvů pro službu front Azure, najdete v části [pojmenování front a Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-tooretrieve-a-queue"></a>Jak tooretrieve fronty
Můžete zadat dotaz a načíst konkrétní fronty nebo seznam všech front hello v účtu úložiště. Hello následující příklad ukazuje, jak tooretrieve zadané frontě pomocí hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny.

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

Když zavoláte hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny bez parametrů, získá seznam všech front hello.

### <a name="how-toodelete-a-queue"></a>Jak toodelete fronty
toodelete frontu a všechny zprávy hello obsažené v něm volání rutiny hello AzureStorageQueue odebrat. Hello následující příklad ukazuje, jak hello toodelete zadané frontě pomocí rutiny Remove-AzureStorageQueue.

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-tooinsert-a-message-into-a-queue"></a>Jak tooinsert zprávu do fronty
tooinsert zprávu do existující fronty, nejprve vytvořit novou instanci třídy hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy. Pak zavolejte hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metoda. CloudQueueMessage můžete vytvořit z řetězce (ve formátu UTF-8) nebo pole bajtů.

Hello následující příklad ukazuje, jak tooadd zprávy tooa fronty. Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč. V dalším kroku se načte hello zadané frontě pomocí hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) rutiny. Pokud hello fronta existuje, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) rutina je použité toocreate instanci hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy. Později, hello ukázka volá hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metoda na tuto zprávu objekt tooadd ho tooa fronty. Zde je kód, který načte fronty a vloží zprávu hello 'MessageInfo':

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If hello queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of hello CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message toohello queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-toode-queue-at-hello-next-message"></a>Jak toode fronty v hello další zprávy
Váš kód vyřazuje zprávy z fronty ve dvou krocích. Při volání hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metody get hello další zprávu ve frontě. Zpráva vrácená metodou **GetMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty. toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metoda. Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu. Váš kód zavolá metodu **DeleteMessage** hned po zpracování zprávy hello.

```powershell
# Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get hello message object from hello queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete hello message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-toomanage-azure-file-shares-and-files"></a>Jak toomanage Azure file sdílené složky a soubory
Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB hello. Virtuální počítače Microsoft Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím rozhraní API úložiště souboru hello nebo Azure PowerShell.

Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](../storage-dotnet-how-to-use-files.md) a [rozhraní API REST služby soubor](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-tooset-and-query-storage-analytics"></a>Jak tooset a dotaz analytika úložiště
Můžete použít [Azure Storage Analytics](../storage-analytics.md) toocollect metriky pro účty úložiště Azure a data protokolu o žádosti odeslané tooyour účet úložiště. Můžete použít stav hello toomonitor metriky úložiště z účtu úložiště a toodiagnose protokolování úložiště a řešení problémů s vaším účtem úložiště. Můžete nakonfigurovat monitorování pomocí hello portálu Azure nebo prostředí Windows PowerShell nebo programově pomocí klientské knihovny pro úložiště hello. Protokolování úložiště se stane, serverový a umožní vám toorecord podrobnosti úspěšných i neúspěšných požadavků ve vašem účtu úložiště. Tyto protokoly umožňují toosee podrobnosti o čtení, zápisu a operace odstranění proti vaší tabulky, fronty a objekty BLOB a také hello důvody pro chybné žádosti.

jak zjistit, tooenable a zobrazení data metriky úložiště pomocí prostředí PowerShell, toolearn [jak tooenable metriky úložiště pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

toolearn jak zjistit, tooenable a načtení protokolování úložiště dat pomocí prostředí PowerShell, [jak tooenable úložiště protokolování pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) a [hledání data protokolu protokolování úložiště](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Podrobné informace o použití úložiště metrik a protokolování problémů s úložištěm tootroubleshoot úložiště najdete v tématu [monitorování, diagnostikování a řešení potíží s Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-toomanage-shared-access-signature-sas-and-stored-access-policy"></a>Jak toomanage sdílený přístupový podpis (SAS) a uložené zásady přístupu
Sdílené přístupové podpisy jsou důležitou součástí hello model zabezpečení pro všechny aplikace pomocí Azure Storage. Jsou užitečné pro zajištění omezenými oprávněními tooyour úložiště účet tooclients, který by neměl mít hello klíč účtu. Ve výchozím nastavení může přístup pouze vlastník hello hello účtu úložiště objektů BLOB, tabulek a v rámci tohoto účtu. Pokud služba nebo aplikace potřebuje toomake těchto klientů k dispozici tooother prostředky bez sdílení přístupový klíč, máte tři možnosti:

* Nastavte kontejneru oprávnění toopermit anonymní přístup pro čtení toohello kontejner a jeho objekty BLOB. To není povolené pro tabulky a fronty.
* Použijte sdílený přístupový podpis, která uděluje práva toocontainers s omezeným přístupem, objekty BLOB, fronty a tabulky pro určitého časového intervalu.
* Použijte zásady tooobtain uložené přístup na další úroveň kontroly nad sdílené přístupové podpisy pro kontejner nebo jeho objekty BLOB, fronty nebo pro tabulku. Hello zásady uložené přístupu vám umožní toochange hello počáteční čas, čas vypršení platnosti nebo oprávnění pro podpis, nebo toorevoke po jeho vydání.

Sdílený přístupový podpis může být v jednom ze dvou formách:

* **Ad hoc SAS**: Když vytvoříte ad hoc SAS, čas spuštění hello, čas vypršení platnosti a oprávnění pro hello SAS nejsou zadány na hello identifikátor URI pro SAS. Tento typ SAS mohou být vytvořeny na kontejner, objektů blob, tabulka nebo fronty a je jiný odvolatelné.
* **SAS se zásadami přístupu uložené**: zásadu uložené přístupu je definovaný na kontejner prostředků kontejner objektů blob, tabulka nebo fronta – a můžete ji použít toomanage omezení pro jeden nebo více sdílených přístupových podpisů. Pokud přidružíte SAS se zásadami přístupu uložené, hello SAS dědí omezení hello – hello času spuštění, čas vypršení platnosti a - definována pro zásady přístupu hello uložené oprávnění. Tento typ SAS se odvolatelné.

Další informace najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](../storage-dotnet-shared-access-signature-part-1.md) a [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md).

V dalších částech hello, se dozvíte, jak toocreate zásadu sdílený přístupový podpis tokenu a uložené přístup pro tabulky Azure. Prostředí Azure PowerShell poskytuje podobné rutiny pro kontejnery objektů BLOB a fronty i. toorun hello skriptů v této části stáhněte hello [prostředí Azure PowerShell verze 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) nebo novější.

### <a name="how-toocreate-a-policy-based-shared-access-signature-token"></a>Jak tokenu sdíleného přístupového podpisu toocreate na základě zásad
Použijte toocreate rutiny New-AzureStorageTableStoredAccessPolicy hello nové zásady uložené přístup. Potom zavolejte hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) rutiny toocreate nový token podpis sdíleného přístupu na základě zásad pro tabulku Azure Storage.

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-toocreate-an-ad-hoc-non-revocable-shared-access-signature-token"></a>Jak toocreate tokenu ad hoc sdíleného přístupového podpisu (bez odvolatelné)
Použití hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) rutiny toocreate nový ad hoc (bez odvolatelné) sdíleného přístupového podpisu token pro tabulku Azure Storage:

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-toocreate-a-stored-access-policy"></a>Jak toocreate zásadu uložené přístupu
Použijte hello AzureStorageTableStoredAccessPolicy nové rutiny toocreate nové zásady uložené přístup tabulce Azure Storage:

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-tooupdate-a-stored-access-policy"></a>Jak tooupdate zásadu uložené přístupu
Použijte tooupdate rutiny Set-AzureStorageTableStoredAccessPolicy hello existující zásady uložené přístup tabulce Azure Storage:

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-toodelete-a-stored-access-policy"></a>Jak toodelete zásadu uložené přístupu
Použijte rutiny toodelete hello odebrat AzureStorageTableStoredAccessPolicy zásadu přístupu uložené na tabulky Azure Storage:

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-toouse-azure-storage-for-us-government-and-azure-china"></a>Jak toouse Azure Storage pro Spojených státech amerických a Azure China
Prostředí Azure je nezávislé nasazení ve službě Microsoft Azure, jako například [Azure Government pro US government](https://azure.microsoft.com/features/gov/), [AzureCloud pro globální Azure](https://portal.azure.com), a [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/). Můžete nasadit nové prostředí Azure government ministerstvo a Azure China.

toouse Azure Storage s AzureChinaCloud, musíte toocreate kontext úložiště, který je přidružen AzureChinaCloud. Postupujte podle těchto kroků tooget, kterou jste zahájili:

1. Spustit hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) toosee rutiny hello k dispozici prostředí Azure:
   
    ```powershell
    Get-AzureEnvironment
    ```

2. Přidejte Azure China účet tooWindows prostředí PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. Vytvoření kontextu úložiště pro účet AzureChinaCloud:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

toouse Azure Storage s [USA Azure Government](https://azure.microsoft.com/features/gov/), měli byste definovat nové prostředí a pak vytvořte nový kontext úložiště s prostředím tohoto nástroje:

1. Spustit hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) toosee rutiny hello k dispozici prostředí Azure:

    ```powershell
    Get-AzureEnvironment
    ```

2. Přidejte Azure US Government účet tooWindows prostředí PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. Vytvoření kontextu úložiště pro účet AzureUSGovernment:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
Další informace naleznete v tématu:

* [Příručka vývojáře pro službu Microsoft Azure Government](../../azure-government/documentation-government-developer-guide.md).
* [Přehled rozdíly při vytváření aplikace v Číně služby](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Další kroky
V této příručce, když jste se naučili jak toomanage Azure Storage pomocí prostředí Azure PowerShell. Tady jsou některé související články a zdroje informací o nich víc.

* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Rutiny prostředí PowerShell úložiště Azure](/powershell/module/azurerm.storage/#storage)
* [Referenční informace prostředí PowerShell systému Windows](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How toomanage storage accounts in Azure]: #manageaccount
[How tooset a default Azure subscription]: #setdefsub
[How toocreate a new Azure storage account]: #createaccount
[How tooset a default Azure storage account]: #defaultaccount
[How toolist all Azure storage accounts in a subscription]: #listaccounts
[How toocreate an Azure storage context]: #createctx
[How toomanage Azure blobs and blob snapshots]: #manageblobs
[How toocreate a container]: #container
[How tooupload a blob into a container]: #uploadblob
[How toodownload blobs from a container]: #downblob
[How toocopy blobs from one storage container tooanother]: #copyblob
[How toodelete a blob]: #deleteblob
[How toomanage Azure blob snapshots]: #manageshots
[How toocreate a blob snapshot]: #createshot
[How toolist snapshots of a blob]: #listshot
[How toocopy a snapshot of a blob]: #copyshot
[How toomanage Azure tables and table entities]: #managetables
[How toocreate a table]: #createtable
[How tooretrieve a table]: #gettable
[How toodelete a table]: #remtable
[How toomanage table entities]: #mngentity
[How tooadd table entities]: #addentity
[How tooquery table entities]: #queryentity
[How toodelete table entities]: #deleteentity
[How toomanage Azure queues and queue messages]: #managequeues
[How toocreate a queue]: #createqueue
[How tooretrieve a queue]: #getqueue
[How toodelete a queue]: #remqueue
[How toomanage queue messages]: #mngqueuemsg
[How tooinsert a message into a queue]: #addqueuemsg
[How toode-queue at hello next message]: #dequeuemsg
[How toomanage Azure file shares and files]: #files
[How tooset and query storage analytics]: #stganalytics
[How toomanage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How toouse Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
