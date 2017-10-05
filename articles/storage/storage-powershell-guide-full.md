---
title: "Použití Azure PowerShell s Azure Storage | Microsoft Docs"
description: "Další informace o použití rutin prostředí Azure PowerShell pro Azure Storage k vytváření a správě účtů úložiště; Práce s objekty BLOB, tabulky, fronty a soubory; Nakonfigurujte a dotaz analytika úložiště a vytvořte sdílených přístupových podpisů."
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
ms.openlocfilehash: 51e3e93ebedd31370857e61a00139294bcee9237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Použití Azure Powershell s Azure Storage
## <a name="overview"></a>Přehled
Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell. Je to prostředí příkazového řádku založené na úlohách a skriptovací jazyk určený speciálně pro správu systému. Pomocí prostředí PowerShell můžete snadno řídit a automatizovat správu systému Azure služby a aplikace. Například můžete použít rutiny k provádění stejných úloh, které můžete provádět prostřednictvím [portál Azure](https://portal.azure.com).

V této příručce, jsme budete prozkoumat postup použití [rutiny úložiště Azure](/powershell/module/azurerm.storage/#storage) k provádění různých úloh vývoj a správu s Azure Storage.

Tato příručka předpokládá, že máte zkušenosti s použitím [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) a [prostředí Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Průvodce poskytuje řadu skripty, které ukazují použití prostředí PowerShell s Azure Storage. Skript proměnné na základě vaší konfigurace před spuštěním každý skript by měl aktualizovat.

V první části v tomto průvodci poskytuje stručný přehled Azure Storage a prostředí PowerShell. Podrobné informace a pokyny, spusťte z [požadavky pro použití prostředí Azure PowerShell s Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Začínáme s Azure Storage a prostředí PowerShell během 5 minut
V této části se dozvíte, jak pro přístup k úložišti Azure pomocí prostředí PowerShell během 5 minut.

**Nové do Azure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému. Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).

V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.

**Po vytvoření odběru služby Microsoft Azure a účet:**

1. Stáhněte a nainstalujte nejnovější [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).
2. Spuštění Windows PowerShell integrované skriptování prostředí (ISE): V místním počítači, přejděte do **spustit** nabídky. Typ **nástroje pro správu** a klikněte na tlačítko ji spustit. V **nástroje pro správu** okna, klikněte pravým tlačítkem na **Windows PowerShell ISE**, klikněte na tlačítko **spustit jako správce**.
3. V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** k vytvoření nového souboru skriptu.
4. Nyní sdělíme vám jednoduchý skript, který zobrazuje základní příkazy prostředí PowerShell pro přístup k úložišti Azure. Skript se nejdřív požádat vašeho Azure přihlašovací údaje účtu pro vaši Azure přidat účet místní prostředí PowerShell. Skript bude poté nastavit výchozí předplatného Azure a vytvořte nový účet úložiště v Azure. V dalším kroku skript vytvoříte nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující soubor bitové kopie (binární rozsáhlý objekt) do tohoto kontejneru. Po skript obsahuje seznam všech objektů BLOB v kontejneru, vytvoří nový cílový adresář v místním počítači a stáhnout soubor bitové kopie.
5. V následující části kódu, vyberte skript mezi poznámky **#begin** a **#end**. Stisknutím kláves CTRL + C zkopírujte jej do schránky.

    ```powershell
    # begin
    # Update with the name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account to the local PowerShell environment.
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
       
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. V **Windows PowerShell ISE**, stiskněte klávesy CTRL + V zkopírujte skript. Klikněte na tlačítko **soubor** > **Uložit**. V **uložit jako** dialogového okna zadejte název souboru skriptu, jako je například "mystoragescript." Klikněte na **Uložit**.
7. Teď je potřeba aktualizovat proměnné skriptu na základě svého nastavení konfigurace. Je nutné aktualizovat **$SubscriptionName** proměnné pomocí svého vlastního předplatného. Můžete ponechat jiné proměnné uvedeného ve skriptu nebo je aktualizovat podle potřeby.
   
   * **$SubscriptionName:** musíte aktualizovat tuto proměnnou s vlastní název odběru. Proveďte jeden z následujících tří způsobů vyhledejte název předplatného:
     
    a. V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** k vytvoření nového souboru skriptu. Zkopírujte následující skript na nový soubor skriptu a klikněte na tlačítko **ladění** > **spustit**. Následující skript se nejdřív požádat vašeho Azure přihlašovací údaje účtu pro vaši Azure přidat účet do místního prostředí PowerShell a pak zobrazíte všechny odběry, které jsou připojené k místní relaci prostředí PowerShell. Poznamenejte si název odběru, který chcete použít během provádění v tomto kurzu:
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    b. Vyhledejte a zkopírujte název odběru v [portál Azure](https://portal.azure.com), v nabídce centra na levé straně klikněte na **odběry**. Zkopírujte název odběru, který chcete použít při spouštění skriptů v této příručce.
     
     ![portál Azure](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    c. Vyhledejte a zkopírujte název odběru v [portálu Azure Classic](https://manage.windowsazure.com/), posuňte se dolů a klikněte na **nastavení** na levé straně na portálu. Klikněte na tlačítko **odběry** zobrazíte seznam předplatných. Zkopírujte název odběru, který chcete použít při spouštění skriptů zadané v této příručce.
     
     ![portál Azure Classic](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * **$StorageAccountName:** použít daným názvem ve skriptu nebo zadejte nový název pro účet úložiště. **Důležité:** název účtu úložiště musí být jedinečný v Azure. Musí být malými písmeny, příliš!
   * **$Location:** použít daný "západní USA" ve skriptu nebo zvolte jiné umístění Azure, jako například Východ USA, Severní Evropa a tak dále.
   * **$ContainerName:** použít daným názvem ve skriptu nebo zadejte nový název pro váš kontejner.
   * **$ImageToUpload:** zadejte cestu k obrázku v místním počítači, jako například: "C:\Images\HelloWorld.png".
   * **$DestinationFolder:** zadejte cestu k místnímu adresáři ukládat soubory stahované z Azure Storage, například: "C:\DownloadImages".
8. Po aktualizaci proměnné skript v souboru "mystoragescript.ps1", klikněte na tlačítko **soubor** > **Uložit**. Potom klikněte na **ladění** > **spustit** nebo stiskněte klávesu **F5** pro spuštění skriptu.  

Po spuštění skriptu, měli byste mít místní cílovou složku, která obsahuje soubor stažený bitové kopie. Následující snímek obrazovky ukazuje příklad výstupu:

![Stáhnout objekty BLOB](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> V části "Začínáme s Azure Storage a prostředí PowerShell během 5 minut" poskytuje rychlý úvod o tom, jak pomocí prostředí Azure PowerShell s Azure Storage. Podrobné informace a pokyny doporučujeme vám přečíst v následujících částech.
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Požadavky pro použití prostředí Azure PowerShell s Azure Storage
Potřebujete předplatné Azure a účet, který chcete spustit rutiny prostředí PowerShell uvedených v této příručce, jak je popsáno výše.

Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell. Informace o instalaci a nastavení prostředí Azure PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Doporučujeme stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell před použitím tohoto průvodce.

Spuštěním rutiny v konzole pro standardní prostředí Windows PowerShell nebo Windows PowerShell Integrované skriptovací prostředí (ISE). Chcete-li například otevřete **Windows PowerShell ISE**, přejděte do nabídky Start, typ nástroje pro správu a klikněte na tlačítko ji spustit. V okně nástroje pro správu klikněte pravým tlačítkem na Windows PowerShell ISE, klikněte na tlačítko Spustit jako správce.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Jak spravovat účty úložiště v Azure

Podívejme se na správu účty úložiště v Azure pomocí prostředí PowerShell

### <a name="how-to-set-a-default-azure-subscription"></a>Jak nastavit výchozí předplatné Azure
Ke správě úložiště Azure pomocí Azure PowerShell, budete muset ověřit prostředí klienta s nástrojem Azure přes Azure Active Directory ověřování nebo ověřování pomocí certifikátů. Podrobné informace najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kurzu. Tento průvodce pomocí ověřování Azure Active Directory.

1. V systému Windows PowerShell ISE, zadejte následující příkaz pro přidání vaší Azure účet místní prostředí PowerShell:

    ```powershell
    Add-AzureAccount
    ```

2. V okně "Přihlášení v k Microsoft Azure" Zadejte e-mailovou adresu a heslo spojené s vaším účtem. Azure přihlašovací údaje ověří, uloží je a pak zavře okno.

3. Potom spusťte následující příkaz k zobrazení účtů Azure ve vašem místním prostředí PowerShell a zkontrolujte, zda váš účet:
   
    ```powershell
    Get-AzureAccount
    ```
4. Potom spusťte následující rutinu zobrazíte všechny odběry, které jsou připojené k místní relaci prostředí PowerShell a zkontrolujte, zda vaše předplatné:

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. Pokud chcete nastavit výchozí předplatné Azure, spusťte rutinu Select-AzureSubscription:

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. Ověřte název výchozí předplatné spuštěním rutiny Get-AzureSubscription:

    ```powershell
    Get-AzureSubscription -Default
    ```

7. Chcete-li zobrazit všechny dostupné rutiny prostředí PowerShell pro Azure Storage, spusťte:
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-to-create-a-new-azure-storage-account"></a>Jak vytvořit nový účet úložiště Azure
Pokud chcete používat úložiště Azure, potřebujete účet úložiště. Po nakonfigurování počítače pro připojení k vašemu předplatnému, můžete vytvořit nový účet úložiště Azure.

1. Spusťte rutinu Get-AzureLocation pro vyhledání všech dostupných datacenter umístění:

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. Potom spusťte rutinu New-AzureStorageAccount k vytvoření nového účtu úložiště. Následující příklad vytvoří nový účet úložiště v datovém centru "Západní USA".
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> Název účtu úložiště musí být jedinečný v rámci Azure a musí být psaný malými písmeny. Zásady vytváření názvů a omezení najdete v tématu [o účtech úložiště Azure](storage-create-storage-account.md) a [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](http://msdn.microsoft.com/library/azure/dd135715.aspx).
> 
> 

### <a name="how-to-set-a-default-azure-storage-account"></a>Jak nastavit výchozí účet úložiště Azure
Můžete mít více účtů úložiště v rámci vašeho předplatného. Můžete zvolit jeden z nich a nastavte jej jako výchozí účet úložiště pro všechny příkazy úložiště ve stejné relaci prostředí PowerShell. Díky tomu můžete ke spuštění příkazů prostředí Azure PowerShell úložiště bez zadání explicitně kontext úložiště.

1. Pokud chcete nastavit výchozí účet úložiště pro vaše předplatné, můžete spustit rutinu Set-AzureSubscription.

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. Potom spusťte rutinu Get-AzureSubscription k ověřte, zda je přiřazená k vašemu účtu předplatného výchozí účet úložiště. Tento příkaz vrátí vlastnosti odběru v aktuálním předplatném, včetně jeho aktuální účet úložiště.

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Postup seznamu všech účtů úložiště Azure v předplatném.
Každé předplatné Azure může mít až 100 účtů úložiště. Nejaktuálnější informace o limitech najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

Spusťte následující rutiny můžete zjistit název a stav účtů úložiště v aktuálním předplatném:

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-to-create-an-azure-storage-context"></a>Postup vytvoření kontextu úložiště Azure
Kontext úložiště Azure je objekt v prostředí PowerShell pro zapouzdření přihlašovacích údajů úložiště. Použitím odlišného kontextu, úložiště, při spuštění libovolné další rutiny umožňuje vaši žádost o ověření bez zadání účtu úložiště a jeho přístupový klíč explicitně. Kontext úložiště můžete vytvořit v mnoha způsoby, například pomocí názvu a přístupový klíč účtu úložiště, sdílený přístupový podpis (SAS) token, připojovací řetězec, nebo anonymní. Další informace najdete v tématu [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).  

Použijte jednu z následujících tří způsobů vytvořit kontext úložiště:

* Spustit [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) rutiny získat přístupový klíč primárního úložiště pro váš účet úložiště Azure. Pak zavolejte [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) vytvořte kontext úložiště:

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* Vygenerování tokenu sdíleného přístupového podpisu pro kontejner úložiště Azure a použít ho k vytvoření kontextu úložiště:

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    Další informace najdete v tématu [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) a [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).

* V některých případech můžete určovat koncové body služby, když vytvoříte nový kontext úložiště. To může být nezbytné, pokud je zaregistrovaný vlastního názvu domény pro váš účet úložiště se službou objektů Blob nebo chcete použít sdílený přístupový podpis pro přístup k prostředkům úložiště. Nastavte koncové body služby v připojovacím řetězci a použít k vytvoření nový kontext úložiště, jak je uvedeno níže:

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

Další informace o tom, jak nakonfigurovat připojovací řetězec úložiště najdete v tématu [konfiguraci připojovacích řetězců](storage-configure-connection-string.md).

Teď, když máte v počítači a zjistili, jak Správa předplatných a účtů úložiště pomocí prostředí Azure PowerShell, přejděte k další části se dozvíte, jak spravovat Azure objekty BLOB a objektů blob snímky.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Jak načíst a znovu vygenerovat klíče úložiště Azure
Účet úložiště Azure obsahuje dva klíče účtu. Následující rutiny můžete načíst vaše klíče.

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

Použijte následující rutinu k načtení konkrétního klíče. Platné hodnoty jsou primární i sekundární.  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

Pokud jste chtěli znovu vygenerovat klíče, použijte následující rutinu. Typ_klíče - platné hodnoty jsou "Primární" a "Sekundární"

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-to-manage-azure-blobs"></a>Správa objektů Azure BLOB
Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolu HTTP nebo HTTPS. V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště objektů Blob Azure. Podrobné informace najdete v tématu [Začínáme s úložištěm Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Postup vytvoření kontejneru
Každý objekt blob v úložišti Azure musí být v kontejneru. Můžete vytvořit kontejner privátní, pomocí rutiny New-AzureStorageContainer:

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**. Chcete-li zabránit anonymní přístup k objektům BLOB, nastavte parametr oprávnění na **vypnout**. Ve výchozím nastavení je nový kontejner je privátní a můžete přistupovat pouze pomocí vlastníka účtu. Chcete-li povolit anonymní veřejný přístup pro čtení k prostředkům blob, ale ne pro metadata kontejneru nebo do seznamu objektů BLOB v kontejneru, nastavte parametr oprávnění na **Blob**. Povolit úplné veřejný přístup pro čtení do objektu blob prostředků, metadata kontejneru a seznamu objektů BLOB v kontejneru, nastavte parametr oprávnění na **kontejneru**. Další informace najdete v tématu [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).
> 
> 

### <a name="how-to-upload-a-blob-into-a-container"></a>Jak nahrát objekt blob do kontejneru
Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky. Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Pokud chcete nahrát do kontejneru objektů BLOB v, můžete použít [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny. Ve výchozím nastavení odešle tento příkaz místních souborů do objekt blob bloku. Chcete-li určit typ pro tento objekt blob, můžete použít parametr - BlobType.

Následující příklad spustí [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) rutiny všechny soubory v zadané složce a předává je do další rutiny pomocí operátor kanálu. [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny odešle místních souborů do vašeho kontejneru:

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-to-download-blobs-from-a-container"></a>Tom, jak stáhnout objekty BLOB z kontejneru
Následující příklad ukazuje, jak stáhnout objekty BLOB kontejneru. V příkladu nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho primární přístupový klíč. Potom příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny. V dalším kroku v příkladu se používá [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) rutiny stáhnout objekty BLOB do místní cílovou složku.

```powershell
#Define the variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Jak kopírovat z jednoho kontejneru úložiště objektů BLOB do jiného
Mezi účty úložiště a oblastí, můžete zkopírovat objekty BLOB asynchronně. Následující příklad ukazuje, jak zkopírovat objekty BLOB z jednoho kontejneru úložiště do druhého v dva účty jiného úložiště. V příkladu nejprve nastaví proměnné pro zdrojové a cílové účty úložiště a poté vytvoří kontext úložiště pro každý účet. V dalším kroku v příkladu kopíruje objekty BLOB z kontejneru zdrojového na cílový kontejner pomocí [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny. Příklad předpokládá, že zdrojový a cílový účty úložiště a kontejnery již existuje.

```powershell
#Define the source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define the destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference to blobs in the source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container to another.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

Pamatujte, že v tomto příkladu asynchronní kopírování. Můžete monitorovat stav každé kopie spuštěním [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) rutiny.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Postup kopírování objekty BLOB ze sekundárního umístění
Objekty BLOB můžete zkopírovat ze sekundárního umístění účtu povoleno RA-GRS.

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-to-delete-a-blob"></a>Jak odstranit objekt blob
Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte rutinu Remove-AzureStorageBlob na něm. Následující příklad odstraní všechny objekty BLOB v daném kontejneru. V příkladu nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště. V dalším kroku příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí [odebrat AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) pro odstranění objektů blob z kontejneru v úložišti Azure.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to all the blobs in the container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-to-manage-azure-blob-snapshots"></a>Jak spravovat snímky objektů blob v Azure
Azure umožňuje vytvoření snímku objektu blob. Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase. Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně. Snímky poskytují způsob, jak zálohovat objekt blob, jak se objevuje v časovém okamžiku. Další informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Postup vytvoření snímku objektů blob
Chcete-li vytvořit snímek objektu blob, nejdřív získejte odkaz na objekt blob a potom volejte `ICloudBlob.CreateSnapshot` metoda na něm. Následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště. V dalším kroku příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metodu pro vytvoření snímku.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of the blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-to-list-a-blobs-snapshots"></a>Jak zobrazit objekt blob snímky
Můžete vytvořit libovolný počet snímků, jak chcete použít pro objekt blob. Můžete vytvořit seznam snímků přidružených objektu blob služby sledovat vaše aktuální snímky. Následující příklad používá předdefinovaných objektů blob a volání [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny seznamu snímky tomuto objektu blob.  

```powershell
#Define the blob name.
$BlobName = "yourblobname"

#List the snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Postup kopírování snímku objektu blob
Můžete zkopírovat snímek objektu blob obnovení snímku. Omezení a podrobné informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště. V příkladu dále definuje název proměnné kontejnerů a objektů blob. Příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metodu pro vytvoření snímku. Poté, v příkladu spustí [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny se kopírování snímku objektu blob pomocí objektu ICloudBlob pro zdrojový objekt blob. Nezapomeňte aktualizovat proměnné na základě vaší konfigurace před spuštěním v příkladu. Všimněte si, že v následujícím příkladu se předpokládá, že zdrojové a cílové kontejnery a zdrojový objekt blob již existují.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define the variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy the snapshot to another container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

Teď, když jste se naučili jak spravovat Azure objekty BLOB a objektů blob snímky s prostředím Azure PowerShell, přejděte k další části se dozvíte, jak spravovat tabulky, fronty a soubory.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Správa Azure tabulky a entity tabulky
Služby úložiště Azure Table je úložiště dat typu NoSQL, který můžete použít k ukládání a dotazování obrovských sad strukturovaných, nerelačních data. Hlavní komponenty služby jsou tabulky, entit a vlastnosti. Tabulka je kolekce entit. Entita je sada vlastností. Každá entita může mít až 252 vlastností, které jsou všechny páry název hodnota. V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště Azure Table. Podrobné informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx) a [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).

V následující témata dozvíte, jak spravovat službu úložiště Azure Table pomocí prostředí Azure PowerShell. Pokryté scénáře zahrnují **vytváření**, **odstraňování**, a **načítání** **tabulky**, a také **přidání**, **dotazování**, a **odstranění entity tabulky**.

### <a name="how-to-create-a-table"></a>Postup vytvoření tabulky
Každá tabulka musí nacházet v účtu úložiště Azure. Následující příklad ukazuje, jak vytvořit tabulku ve službě Azure Storage. V příkladu nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč. Dále je použita [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutiny k vytvoření tabulky ve službě Azure Storage.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-retrieve-a-table"></a>Jak načíst tabulku
Můžete zadat dotaz a načtení jedné nebo všech tabulek v účtu úložiště. Následující příklad ukazuje, jak načíst dané tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

Při volání rutiny Get-AzureStorageTable bez parametrů, získá všechny tabulky úložiště pro účet úložiště.

### <a name="how-to-delete-a-table"></a>Postup odstranění tabulky
Můžete odstranit tabulku z účtu úložiště pomocí [odebrat AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) rutiny.  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-manage-table-entities"></a>Jak spravovat entity tabulky
V současné době prostředí Azure PowerShell neposkytuje rutin určených ke správě entity tabulky přímo. K provádění operací na entity tabulky, můžete použít třídy zadaná v [Klientská knihovna pro úložiště Azure pro .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Postup přidání entity tabulky
Chcete-li do tabulky přidat entitu, nejprve vytvořte objekt, který definuje vlastnosti vaší entity. Entita může mít maximálně 255 vlastnosti, včetně 3 Vlastnosti systému: **PartitionKey**, **RowKey**, a **časové razítko**. Jste zodpovědní za vložení a aktualizace hodnoty **PartitionKey** a **RowKey**. Server spravuje hodnotu **časové razítko**, který nemůže být upraven. Společně **PartitionKey** a **RowKey** jednoznačně identifikovat každou entitu v tabulce.

* **PartitionKey**: Určuje uložených entity v oddílu.
* **RowKey**: jednoznačně identifikuje entity v oddílu.

Můžete definovat vlastní až 252 vlastností entity. Další informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Následující příklad ukazuje, jak přidat entity do tabulky. Tento příklad ukazuje, jak načíst tabulky zaměstnanců a přidejte do ní několik entit. Nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč. V dalším kroku načte dané tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny. Pokud tabulka neexistuje, [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutina se používá k vytvoření tabulky ve službě Azure Storage. V příkladu dále definuje vlastní funkci Přidat Entity k přidání entity do tabulky se zadáním každé entity oddílu a klíč řádku. Volání funkcí přidat Entity [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na rutinu [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) třídy za účelem vytvoření objektu entity. Později v příkladu volá [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) metoda na tento objekt entity přidat do tabulky.

```powershell
#Function Add-Entity: Adds an employee entity to a table.
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

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve the table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities to a table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-to-query-table-entities"></a>Postup dotazování entity tabulky
Dotaz na tabulku, použijte [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) třídy. V následujícím příkladu se předpokládá, že jste již spusťte skript v pokynů pro přidání entity části této příručky. V příkladu nejprve vytvoří připojení k Azure Storage pomocí storage kontext, který obsahuje název účtu úložiště a jeho přístupový klíč. Další, pokusí se načíst dříve vytvořenou "zaměstnanci" tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny. Volání [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na třídě Microsoft.WindowsAzure.Storage.Table.TableQuery rutinu vytvoří nový objekt dotazu. V příkladu vypadá pro entity, které mají na sloupec 'ID', jehož hodnota je 1 zadané v řetězec filtru. Podrobné informace najdete v tématu [dotazování tabulky a entity](http://msdn.microsoft.com/library/azure/dd894031.aspx). Při spuštění tohoto dotazu vrátí všechny entity, které odpovídají kritériím filtru.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference to a table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns to select.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute the query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with the table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-to-delete-table-entities"></a>Postup odstranění entity tabulky
Můžete odstranit pomocí jeho klíče oddílu a řádku entity. V následujícím příkladu se předpokládá, že jste již spusťte skript v pokynů pro přidání entity části této příručky. V příkladu nejprve vytvoří připojení k Azure Storage pomocí storage kontext, který obsahuje název účtu úložiště a jeho přístupový klíč. Další, pokusí se načíst dříve vytvořenou "zaměstnanci" tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny. Pokud v tabulce existuje, zavolá na příklad [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metoda pro načtení entity podle jeho oddílu a řádku klíčové hodnoty. Pak předejte entity, která má [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metoda odstranit.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If the table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together the PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete the entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Správa Azure fronty a zprávy fronty
Azure Queue Storage je služba pro ukládání velkého počtu zpráv, ke které můžete získat přístup z jakéhokoli místa na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. V této části se předpokládá, že jste již obeznámeni s koncepty Azure fronty úložiště služby. Podrobné informace najdete v tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md).

V této části vám ukáže, jak spravovat službu úložiště Azure Queue pomocí Azure PowerShell. Pokryté scénáře zahrnují **vkládání** a **odstraňování** fronty zpráv, a také **vytváření**, **odstraňování**, a **načítání fronty**.

### <a name="how-to-create-a-queue"></a>Postup vytvoření fronty
Následující příklad nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč. Dále volá [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) rutiny vytvořit frontu s názvem 'queuename'.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

Informace o vytváření názvů pro službu front Azure, najdete v části [pojmenování front a Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Jak načíst fronty
Můžete zadat dotaz a načíst konkrétní fronty nebo seznam všech front, které jsou v účtu úložiště. Následující příklad ukazuje, jak načíst zadané frontě pomocí [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny.

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

Když zavoláte [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny bez parametrů, získá seznam všech front.

### <a name="how-to-delete-a-queue"></a>Postup odstranění fronty
Chcete-li odstranit frontu se všemi zprávami, které v ní, zavolejte rutinu Remove-AzureStorageQueue. Následující příklad ukazuje, jak odstranit pomocí rutiny Remove-AzureStorageQueue zadanou frontu.

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-to-insert-a-message-into-a-queue"></a>Postup vložit zprávu do fronty
Pokud chcete vložit zprávu do existující fronty, nejprve vytvořit novou instanci třídy [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy. Pak zavolejte metodu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx). CloudQueueMessage můžete vytvořit z řetězce (ve formátu UTF-8) nebo pole bajtů.

Následující příklad ukazuje, jak přidat zprávu do fronty. V příkladu nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč. V dalším kroku načte zadané frontě pomocí [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) rutiny. Pokud fronta existuje, [New-Object](http://technet.microsoft.com/library/hh849885.aspx) rutina se používá k vytvoření instance [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy. Později v příkladu volá [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metoda na tento objekt zpráva tím ho přidáte do fronty. Zde je kód, který načte fronty a vloží zprávu "MessageInfo":

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If the queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of the CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message to the queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-to-de-queue-at-the-next-message"></a>Postup zrušte další zprávy fronty
Váš kód vyřazuje zprávy z fronty ve dvou krocích. Při volání [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metoda, získáte další zprávu ve frontě. Zpráva vrácená metodou **GetMessage** se stane neviditelnou pro jakýkoli jiný kód, který čte zprávy z této fronty. K dokončení odebrání zprávy z fronty, musíte také zavolat [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metoda. Tento dvoukrokový proces odebrání zprávy zaručuje, aby v případě, že se vašemu kódu nepodaří zprávu zpracovat z důvodu selhání hardwaru nebo softwaru, mohla stejnou zprávu získat jiná instance vašeho kódu a bylo možné to zkusit znovu. Váš kód zavolá metodu **DeleteMessage** hned po zpracování zprávy.

```powershell
# Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get the message object from the queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete the message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-to-manage-azure-file-shares-and-files"></a>Správa Azure sdílených složek a souborů
Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB. Virtuální počítače Microsoft Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím rozhraní API úložiště souboru nebo Azure PowerShell.

Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](storage-dotnet-how-to-use-files.md) a [rozhraní API REST služby soubor](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Jak nastavit a analytika úložiště dotazů
Můžete použít [Azure Storage Analytics](storage-analytics.md) ke shromažďování metrik pro účty úložiště Azure a protokolovat data o požadavky odeslané na účtu úložiště. Úložiště metriky můžete monitorovat stav účtu úložiště a úložiště protokolování pro diagnostiku a řešení problémů s vaším účtem úložiště. Můžete nakonfigurovat monitorování pomocí portálu Azure nebo prostředí Windows PowerShell nebo programově pomocí klientské knihovny pro úložiště. Protokolování úložiště se stane, serverový a umožní vám si zaznamenejte podrobnosti pro úspěšné i neúspěšné požadavky ve vašem účtu úložiště. Tyto protokoly umožňují najdete v části Podrobnosti o čtení, zápisu a odstraňování operace u tabulky, fronty a objekty BLOB, jakož i důvody pro chybné žádosti.

Informace o povolení a zobrazovat data metriky úložiště pomocí prostředí PowerShell najdete v tématu [povolení metrik úložiště pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Informace o povolení a načtení dat protokolování úložiště pomocí prostředí PowerShell najdete v tématu [povolení protokolování úložiště pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) a [hledání data protokolu protokolování úložiště](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Podrobné informace o použití úložiště metrik a protokolování úložiště k řešení problémů s úložištěm najdete v tématu [monitorování, diagnostikování a řešení potíží s Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Správa sdíleného přístupového podpisu (SAS) a uložené zásady přístupu
Sdílené přístupové podpisy jsou důležitou součástí model zabezpečení pro všechny aplikace pomocí Azure Storage. Jsou užitečné pro zajištění omezené oprávnění k účtu úložiště na klienty, kteří by neměl mít klíč účtu. Ve výchozím nastavení může přístup pouze vlastník účtu úložiště objektů BLOB, tabulek a v rámci tohoto účtu. Pokud je potřeba zpřístupnit tyto prostředky pro ostatní klienty bez sdílení přístupový klíč služby nebo aplikace, máte tři možnosti:

* Nastavte oprávnění kontejner pro povolení anonymního přístupu pro čtení ke kontejneru a jeho objekty BLOB. To není povolené pro tabulky a fronty.
* Použijte sdílený přístupový podpis, udělí omezený přístupová práva k kontejnerů, objektů BLOB, fronty a tabulky pro určitého časového intervalu.
* Pomocí zásad uložené přístup získat další úroveň kontroly nad sdílené přístupové podpisy pro kontejner nebo jeho objekty BLOB, fronty nebo tabulky. Zásady uložené přístupu vám umožní změnit čas zahájení, čas vypršení platnosti nebo oprávnění pro podpis, nebo ho zablokujte po jeho bylo vydáno.

Sdílený přístupový podpis může být v jednom ze dvou formách:

* **Ad hoc SAS**: Když vytvoříte ad hoc SAS, čas zahájení, čas vypršení platnosti a oprávnění pro SAS se zadaný v identifikátoru URI SAS. Tento typ SAS mohou být vytvořeny na kontejner, objektů blob, tabulka nebo fronty a je jiný odvolatelné.
* **SAS se zásadami přístupu uložené**: zásadu uložené přístupu je definován na kontejner prostředků kontejner objektů blob, tabulka nebo fronta – a můžete ji použít ke správě omezení pro jeden nebo více sdílených přístupových podpisů. Pokud přidružíte SAS se zásadami přístupu uložené, zdědí SAS omezení – čas spuštění, čas vypršení platnosti a - definována pro zásady uložené přístupu oprávnění. Tento typ SAS se odvolatelné.

Další informace najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md) a [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).

V následujících částech se dozvíte, jak vytvořit zásady sdíleného přístupu podpis tokenu a uložené přístup pro tabulky Azure. Prostředí Azure PowerShell poskytuje podobné rutiny pro kontejnery objektů BLOB a fronty i. Chcete-li spustit skripty v této části, stáhněte [prostředí Azure PowerShell verze 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) nebo novější.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Jak vytvořit token podpis sdíleného přístupu na základě zásad
Pomocí rutiny New-AzureStorageTableStoredAccessPolicy vytvořit novou zásadu uložené přístup. Potom zavolejte [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) vytvořte nový token podpis sdíleného přístupu na základě zásad pro tabulku Azure Storage.

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-to-create-an-ad-hoc-non-revocable-shared-access-signature-token"></a>Postup vytvoření tokenu ad hoc sdíleného přístupového podpisu (bez odvolatelné)
Použití [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) vytvořte nový ad hoc (bez odvolatelné) sdíleného přístupového podpisu token pro tabulku Azure Storage:

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-to-create-a-stored-access-policy"></a>Postup vytvoření zásady uložené přístupu
Pomocí rutiny New-AzureStorageTableStoredAccessPolicy vytvořit novou zásadu uložené přístup tabulce Azure Storage:

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-to-update-a-stored-access-policy"></a>Postup aktualizace zásad uložené přístupu
Použijte rutinu Set-AzureStorageTableStoredAccessPolicy aktualizovat existující zásady uložené přístup tabulce Azure Storage:

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-to-delete-a-stored-access-policy"></a>Postup odstranění zásady uložené přístupu
Použijte rutinu Remove-AzureStorageTableStoredAccessPolicy odstranění zásady přístupu uložené v tabulce Azure Storage:

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Jak používat Azure Storage pro vládou USA a Azure China
Prostředí Azure je nezávislé nasazení ve službě Microsoft Azure, jako například [Azure Government pro US government](https://azure.microsoft.com/features/gov/), [AzureCloud pro globální Azure](https://portal.azure.com), a [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/). Můžete nasadit nové prostředí Azure government ministerstvo a Azure China.

Používání Azure Storage s AzureChinaCloud, musíte vytvořit kontext úložiště, který je přidružen AzureChinaCloud. Postupujte podle těchto kroků, které vám pomůžou začít:

1. Spustit [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) rutiny zobrazíte dostupné prostředí Azure:
   
    ```powershell
    Get-AzureEnvironment
    ```

2. Přidání účtu Azure China do prostředí Windows PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. Vytvoření kontextu úložiště pro účet AzureChinaCloud:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

Používání Azure Storage s [USA Azure Government](https://azure.microsoft.com/features/gov/), měli byste definovat nové prostředí a pak vytvořte nový kontext úložiště s prostředím tohoto nástroje:

1. Spustit [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) rutiny zobrazíte dostupné prostředí Azure:

    ```powershell
    Get-AzureEnvironment
    ```

2. Přidání účtu Azure US Government do prostředí Windows PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. Vytvoření kontextu úložiště pro účet AzureUSGovernment:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
Další informace naleznete v tématu:

* [Příručka vývojáře pro službu Microsoft Azure Government](../azure-government-developer-guide.md).
* [Přehled rozdíly při vytváření aplikace v Číně služby](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Další kroky
V této příručce když jste se naučili jak spravovat úložiště Azure pomocí prostředí Azure PowerShell. Tady jsou některé související články a zdroje informací o nich víc.

* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Rutiny prostředí PowerShell úložiště Azure](/powershell/module/azurerm.storage/#storage)
* [Referenční informace prostředí PowerShell systému Windows](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
