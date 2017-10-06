---
title: aaaHow toouse Azure Blob storage z iOS | Microsoft Docs
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: ios
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: df188021-86fc-4d31-a810-1b0e7bcd814b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: objective-c
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: cc08b76b682537a9a51e970c76bd76c7c06a4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ios"></a><span data-ttu-id="7c86f-103">Jak toouse úložiště Blob z iOS</span><span class="sxs-lookup"><span data-stu-id="7c86f-103">How toouse Blob storage from iOS</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="7c86f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7c86f-104">Overview</span></span>
<span data-ttu-id="7c86f-105">Tento článek vám ukáže, jak tooperform běžné scénáře s využitím Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7c86f-105">This article will show you how tooperform common scenarios using Microsoft Azure Blob storage.</span></span> <span data-ttu-id="7c86f-106">Hello ukázky jsou napsané v jazyce Objective-C a používají hello [Klientská knihovna pro úložiště Azure pro iOS](https://github.com/Azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="7c86f-106">hello samples are written in Objective-C and use hello [Azure Storage Client Library for iOS](https://github.com/Azure/azure-storage-ios).</span></span> <span data-ttu-id="7c86f-107">Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="7c86f-107">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="7c86f-108">Další informace o objekty BLOB najdete v tématu hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="7c86f-108">For more information on blobs, see hello [Next Steps](#next-steps) section.</span></span> <span data-ttu-id="7c86f-109">Můžete také stáhnout hello [ukázkovou aplikaci](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) najdete v části tooquickly hello využívání Azure Storage v aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="7c86f-109">You can also download hello [sample app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly see hello use of Azure Storage in an iOS application.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-hello-azure-storage-ios-library-into-your-application"></a><span data-ttu-id="7c86f-110">Importovat knihovny iOS hello Azure Storage do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7c86f-110">Import hello Azure Storage iOS library into your application</span></span>
<span data-ttu-id="7c86f-111">Můžete importovat hello Azure Storage iOS knihovny do své aplikace buď pomocí hello [CocoaPod úložiště Azure](https://cocoapods.org/pods/AZSClient) nebo importem hello **Framework** souboru.</span><span class="sxs-lookup"><span data-stu-id="7c86f-111">You can import hello Azure Storage iOS library into your application either by using hello [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) or by importing hello **Framework** file.</span></span> <span data-ttu-id="7c86f-112">CocoaPod je hello nedoporučuje způsobem, protože umožňuje jednodušší, ale import ze souboru framework hello je šetrnější pro existující projekt integrační hello knihovně.</span><span class="sxs-lookup"><span data-stu-id="7c86f-112">CocoaPod is hello recommended way as it makes integrating hello library easier, however importing from hello framework file is less intrusive for your existing project.</span></span>

<span data-ttu-id="7c86f-113">toouse této knihovny, je nutné hello následující:</span><span class="sxs-lookup"><span data-stu-id="7c86f-113">toouse this library, you need hello following:</span></span>
- <span data-ttu-id="7c86f-114">iOS 8 +</span><span class="sxs-lookup"><span data-stu-id="7c86f-114">iOS 8+</span></span>
- <span data-ttu-id="7c86f-115">Xcode 7 +</span><span class="sxs-lookup"><span data-stu-id="7c86f-115">Xcode 7+</span></span>

## <a name="cocoapod"></a><span data-ttu-id="7c86f-116">CocoaPod</span><span class="sxs-lookup"><span data-stu-id="7c86f-116">CocoaPod</span></span>
1. <span data-ttu-id="7c86f-117">Pokud jste to ještě neudělali, [nainstalovat CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) ve vašem počítači otevřete okno terminálu a spuštěním hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="7c86f-117">If you haven't done so already, [Install CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) on your computer by opening a terminal window and running hello following command</span></span>
    
    ```shell   
    sudo gem install cocoapods
    ```

2. <span data-ttu-id="7c86f-118">V dalším kroku v adresáři projektu hello (hello adresář obsahující souboru .xcodeproj), vytvořte nový soubor s názvem _Podfile_(bez přípony souboru).</span><span class="sxs-lookup"><span data-stu-id="7c86f-118">Next, in hello project directory (hello directory containing your .xcodeproj file), create a new file called _Podfile_(no file extension).</span></span> <span data-ttu-id="7c86f-119">Přidejte následující too_Podfile_ hello a uložte.</span><span class="sxs-lookup"><span data-stu-id="7c86f-119">Add hello following too_Podfile_ and save.</span></span>

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. <span data-ttu-id="7c86f-120">V okně hello terminálu přejděte toohello adresáři projektu a spusťte hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="7c86f-120">In hello terminal window, navigate toohello project directory and run hello following command</span></span>

    ```shell    
    pod install
    ```

4. <span data-ttu-id="7c86f-121">Pokud vaše .xcodeproj otevřít v Xcode, zavřete ji.</span><span class="sxs-lookup"><span data-stu-id="7c86f-121">If your .xcodeproj is open in Xcode, close it.</span></span> <span data-ttu-id="7c86f-122">V souboru projektu otevřete hello nově vytvořený adresář projektu, které budou mít hello .xcworkspace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7c86f-122">In your project directory open hello newly created project file which will have hello .xcworkspace extension.</span></span> <span data-ttu-id="7c86f-123">Toto je soubor hello, kterou budete pracujete z nyní na.</span><span class="sxs-lookup"><span data-stu-id="7c86f-123">This is hello file you'll work from for now on.</span></span>

## <a name="framework"></a><span data-ttu-id="7c86f-124">Framework</span><span class="sxs-lookup"><span data-stu-id="7c86f-124">Framework</span></span>
<span data-ttu-id="7c86f-125">Hello způsobem, který je toouse hello knihovně toobuild hello framework ručně:</span><span class="sxs-lookup"><span data-stu-id="7c86f-125">hello other way toouse hello library is toobuild hello framework manually:</span></span>

1. <span data-ttu-id="7c86f-126">První, stažení nebo klonování hello [úložišti azure-storage-ios](https://github.com/azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="7c86f-126">First, download or clone hello [azure-storage-ios repo](https://github.com/azure/azure-storage-ios).</span></span>
2. <span data-ttu-id="7c86f-127">Přejděte do *azure-storage-ios* -> *Lib* -> *Klientská knihovna pro úložiště Azure*a otevřete `AZSClient.xcodeproj` v Xcode.</span><span class="sxs-lookup"><span data-stu-id="7c86f-127">Go into *azure-storage-ios* -> *Lib* -> *Azure Storage Client Library*, and open `AZSClient.xcodeproj` in Xcode.</span></span>
3. <span data-ttu-id="7c86f-128">V hello levého horního pro Xcode, změna hello active scheme z "Klientská knihovna pro úložiště Azure" příliš "Framework".</span><span class="sxs-lookup"><span data-stu-id="7c86f-128">At hello top-left of Xcode, change hello active scheme from "Azure Storage Client Library" too"Framework".</span></span>
4. <span data-ttu-id="7c86f-129">Sestavení projektu hello (⌘ + B).</span><span class="sxs-lookup"><span data-stu-id="7c86f-129">Build hello project (⌘+B).</span></span> <span data-ttu-id="7c86f-130">Tím se vytvoří `AZSClient.framework` soubor na pracovní ploše.</span><span class="sxs-lookup"><span data-stu-id="7c86f-130">This will create an `AZSClient.framework` file on your Desktop.</span></span>

<span data-ttu-id="7c86f-131">Potom můžete importovat soubor framework hello do vaší aplikace pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="7c86f-131">You can then import hello framework file into your application by doing hello following:</span></span>

1. <span data-ttu-id="7c86f-132">Vytvoření nového projektu nebo otevře existující projekt v Xcode.</span><span class="sxs-lookup"><span data-stu-id="7c86f-132">Create a new project or open up your existing project in Xcode.</span></span>
2. <span data-ttu-id="7c86f-133">Přetažení hello `AZSClient.framework` do Xcode navigátoru projektů.</span><span class="sxs-lookup"><span data-stu-id="7c86f-133">Drag and drop hello `AZSClient.framework` into your Xcode project navigator.</span></span>
3. <span data-ttu-id="7c86f-134">Vyberte *kopírovat položky v případě potřeby*a klikněte na *Dokončit*.</span><span class="sxs-lookup"><span data-stu-id="7c86f-134">Select *Copy items if needed*, and click on *Finish*.</span></span>
4. <span data-ttu-id="7c86f-135">Klikněte na projekt v levém navigačním hello a klikněte na tlačítko hello *Obecné* karty v horní části hello hello projektu editoru.</span><span class="sxs-lookup"><span data-stu-id="7c86f-135">Click on your project in hello left-hand navigation and click hello *General* tab at hello top of hello project editor.</span></span>
5. <span data-ttu-id="7c86f-136">V části hello *propojené architektury a knihovny* klikněte na tlačítko Přidat hello (+).</span><span class="sxs-lookup"><span data-stu-id="7c86f-136">Under hello *Linked Frameworks and Libraries* section, click hello Add button (+).</span></span>
6. <span data-ttu-id="7c86f-137">Vyhledejte v seznamu hello knihoven už poskytuje `libxml2.2.tbd` a přidejte ji tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="7c86f-137">In hello list of libraries already provided, search for `libxml2.2.tbd` and add it tooyour project.</span></span>

## <a name="import-hello-library"></a><span data-ttu-id="7c86f-138">Import hello knihovně</span><span class="sxs-lookup"><span data-stu-id="7c86f-138">Import hello Library</span></span> 
```objc
// Include hello following import statement toouse blob APIs.
#import <AZSClient/AZSClient.h>
```

<span data-ttu-id="7c86f-139">Pokud používáte Swift, bude nutné toocreate hlavičku přemostění a importovat < AZSClient/AZSClient.h > existuje:</span><span class="sxs-lookup"><span data-stu-id="7c86f-139">If you are using Swift, you will need toocreate a bridging header and import <AZSClient/AZSClient.h> there:</span></span>

1. <span data-ttu-id="7c86f-140">Vytvořte soubor hlavičky `Bridging-Header.h`a přidejte hello výše příkaz importu.</span><span class="sxs-lookup"><span data-stu-id="7c86f-140">Create a header file `Bridging-Header.h`, and add hello above import statement.</span></span>
2. <span data-ttu-id="7c86f-141">Přejděte toohello *nastavení sestavení* kartě a vyhledejte *hlavičky přemostění jazyka Objective-C*.</span><span class="sxs-lookup"><span data-stu-id="7c86f-141">Go toohello *Build Settings* tab, and search for *Objective-C Bridging Header*.</span></span>
3. <span data-ttu-id="7c86f-142">Dvakrát klikněte na pole hello *hlavičky přemostění jazyka Objective-C* a přidejte soubor hlaviček tooyour hello cesta:`ProjectName/Bridging-Header.h`</span><span class="sxs-lookup"><span data-stu-id="7c86f-142">Double-click on hello field of *Objective-C Bridging Header* and add hello path tooyour header file: `ProjectName/Bridging-Header.h`</span></span>
4. <span data-ttu-id="7c86f-143">Tooverify sestavení hello projektu (⌘ + B), který hello přemostění záhlaví byla zachyceny pomocí Xcode.</span><span class="sxs-lookup"><span data-stu-id="7c86f-143">Build hello project (⌘+B) tooverify that hello bridging header was picked up by Xcode.</span></span>
5. <span data-ttu-id="7c86f-144">Začít používat hello knihovně přímo v žádném Swift souboru, není nutné pro příkazy pro import.</span><span class="sxs-lookup"><span data-stu-id="7c86f-144">Start using hello library directly in any Swift file, there is no need for import statements.</span></span>

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a><span data-ttu-id="7c86f-145">Asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="7c86f-145">Asynchronous Operations</span></span>
> [!NOTE]
> <span data-ttu-id="7c86f-146">Všechny metody, které provádět požadavek hello služby jsou asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="7c86f-146">All methods that perform a request against hello service are asynchronous operations.</span></span> <span data-ttu-id="7c86f-147">V hello ukázky kódu najdete splnit tyto metody dokončení obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="7c86f-147">In hello code samples, you'll find that these methods have a completion handler.</span></span> <span data-ttu-id="7c86f-148">Kód dokončení hello obslužná rutina se spustí **po** hello požadavek je dokončen.</span><span class="sxs-lookup"><span data-stu-id="7c86f-148">Code inside hello completion handler will run **after** hello request is completed.</span></span> <span data-ttu-id="7c86f-149">Kód po dokončení hello obslužná rutina se spustí **při** probíhá požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="7c86f-149">Code after hello completion handler will run **while** hello request is being made.</span></span>
> 
> 

## <a name="create-a-container"></a><span data-ttu-id="7c86f-150">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c86f-150">Create a container</span></span>
<span data-ttu-id="7c86f-151">Každý objekt blob v Azure Storage se musí nacházet v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7c86f-151">Every blob in Azure Storage must reside in a container.</span></span> <span data-ttu-id="7c86f-152">Hello následující příklad ukazuje, jak toocreate kontejner, nazývá *newcontainer*, ve vašem účtu úložiště, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="7c86f-152">hello following example shows how toocreate a container, called *newcontainer*, in your Storage account if it doesn't already exist.</span></span> <span data-ttu-id="7c86f-153">Když vyberete název vašeho kontejneru, být s vědomím hello názvy pravidel uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="7c86f-153">When choosing a name for your container, be mindful of hello naming rules mentioned above.</span></span>

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

<span data-ttu-id="7c86f-154">Můžete potvrdit, že to funguje prohlížením hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) a ověřování, který *newcontainer* je v seznamu hello kontejnery pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7c86f-154">You can confirm that this works by looking at hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that *newcontainer* is in hello list of containers for your Storage account.</span></span>

## <a name="set-container-permissions"></a><span data-ttu-id="7c86f-155">Nastavte oprávnění kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c86f-155">Set Container Permissions</span></span>
<span data-ttu-id="7c86f-156">Kontejneru oprávnění jsou nakonfigurovaná pro **privátní** přístup ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7c86f-156">A container's permissions are configured for **Private** access by default.</span></span> <span data-ttu-id="7c86f-157">Kontejnery však obsahují několik různých volbách pro kontejner přístupu:</span><span class="sxs-lookup"><span data-stu-id="7c86f-157">However, containers provide a few different options for container access:</span></span>

* <span data-ttu-id="7c86f-158">**Privátní**: kontejnerů a objektů blob dat může číst pouze vlastníka účtu hello.</span><span class="sxs-lookup"><span data-stu-id="7c86f-158">**Private**: Container and blob data can be read by hello account owner only.</span></span>
* <span data-ttu-id="7c86f-159">**Objekt BLOB**: data objektu Blob v tomto kontejneru mohou číst přes anonymní žádost, ale kontejneru data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7c86f-159">**Blob**: Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="7c86f-160">Klienty nelze vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost.</span><span class="sxs-lookup"><span data-stu-id="7c86f-160">Clients cannot enumerate blobs within hello container via anonymous request.</span></span>
* <span data-ttu-id="7c86f-161">**Kontejner**: přes anonymní dotazy můžete číst data kontejnerů a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="7c86f-161">**Container**: Container and blob data can be read via anonymous request.</span></span> <span data-ttu-id="7c86f-162">Klienti, můžete vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7c86f-162">Clients can enumerate blobs within hello container via anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="7c86f-163">Hello následující příklad ukazuje, jak toocreate a kontejner s **kontejneru** přístupová oprávnění, které vám umožní přístup k veřejné, jen pro čtení pro všechny uživatele v hello Internetu:</span><span class="sxs-lookup"><span data-stu-id="7c86f-163">hello following example shows you how toocreate a container with **Container** access permissions, which will allow public, read-only access for all users on hello Internet:</span></span>

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="7c86f-164">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c86f-164">Upload a blob into a container</span></span>
<span data-ttu-id="7c86f-165">Jak je uvedeno v hello [koncepty služby objektů Blob](#blob-service-concepts) část, úložiště objektů Blob nabízí tři různé typy objektů blob: objekty BLOB bloků, doplňovací objekty BLOB a objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="7c86f-165">As mentioned in hello [Blob service concepts](#blob-service-concepts) section, Blob Storage offers three different types of blobs: block blobs, append blobs, and page blobs.</span></span> <span data-ttu-id="7c86f-166">Knihovna iOS Hello Azure Storage podporuje všechny tři typy objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="7c86f-166">hello Azure Storage iOS library supports all three types of blobs.</span></span> <span data-ttu-id="7c86f-167">Ve většině případů je objekt blob bloku hello doporučená toouse typu.</span><span class="sxs-lookup"><span data-stu-id="7c86f-167">In most cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="7c86f-168">Hello následující příklad ukazuje, jak z NSString tooupload blok objektů blob.</span><span class="sxs-lookup"><span data-stu-id="7c86f-168">hello following example shows how tooupload a block blob from an NSString.</span></span> <span data-ttu-id="7c86f-169">Pokud objekt blob se stejným názvem již existuje v tomto kontejneru hello hello obsah tohoto objektu blob budou přepsány.</span><span class="sxs-lookup"><span data-stu-id="7c86f-169">If a blob with hello same name already exists in this container, hello contents of this blob will be overwritten.</span></span>

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob tooStorage
                [blockBlob uploadFromText:@"This text will be uploaded tooBlob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

<span data-ttu-id="7c86f-170">Můžete potvrdit, že to funguje prohlížením hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) a ověření tohoto kontejneru hello *containerpublic*, obsahuje hello blob *sampleblob*.</span><span class="sxs-lookup"><span data-stu-id="7c86f-170">You can confirm that this works by looking at hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that hello container, *containerpublic*, contains hello blob, *sampleblob*.</span></span> <span data-ttu-id="7c86f-171">V této ukázce jsme použili kontejner veřejné, tak můžete také ověřit, zda tato aplikace funguje podle přejdete objekty BLOB toohello identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="7c86f-171">In this sample, we used a public container so you can also verify that this application worked by going toohello blobs URI:</span></span>

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

<span data-ttu-id="7c86f-172">Kromě toho toouploading objekt blob bloku z NSString podobné metody existují NSData, NSInputStream nebo místního souboru.</span><span class="sxs-lookup"><span data-stu-id="7c86f-172">In addition toouploading a block blob from an NSString, similar methods exist for NSData, NSInputStream, or a local file.</span></span>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="7c86f-173">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c86f-173">List hello blobs in a container</span></span>
<span data-ttu-id="7c86f-174">Hello následující příklad ukazuje, jak toolist všechny objekty BLOB v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7c86f-174">hello following example shows how toolist all blobs in a container.</span></span> <span data-ttu-id="7c86f-175">Při provádění této operace, mělo pamatovat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="7c86f-175">When performing this operation, be mindful of hello following parameters:</span></span>     

* <span data-ttu-id="7c86f-176">**continuationToken** -hello tokenu představuje pokračování, kde by se měl spustit hello výpis operaci.</span><span class="sxs-lookup"><span data-stu-id="7c86f-176">**continuationToken** - hello continuation token represents where hello listing operation should start.</span></span> <span data-ttu-id="7c86f-177">Pokud je k dispozici žádné token, zobrazí seznam objektů BLOB od začátku hello.</span><span class="sxs-lookup"><span data-stu-id="7c86f-177">If no token is provided, it will list blobs from hello beginning.</span></span> <span data-ttu-id="7c86f-178">Libovolný počet objektů BLOB může být uvedený od nuly až tooa nastavit maximální.</span><span class="sxs-lookup"><span data-stu-id="7c86f-178">Any number of blobs can be listed, from zero up tooa set maximum.</span></span> <span data-ttu-id="7c86f-179">I když tato metoda vrátí hodnotu nula výsledky, pokud `results.continuationToken` není nulovou hodnotu, může být další objekty BLOB ve službě hello nejsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="7c86f-179">Even if this method returns zero results, if `results.continuationToken` is not nil, there may be more blobs on hello service that have not been listed.</span></span>
* <span data-ttu-id="7c86f-180">**Předpona** -můžete určit předpona toouse hello seznam objektů blob.</span><span class="sxs-lookup"><span data-stu-id="7c86f-180">**prefix** - You can specify hello prefix toouse for blob listing.</span></span> <span data-ttu-id="7c86f-181">Pouze objekty BLOB, které začínají s touto předponou budou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="7c86f-181">Only blobs that begin with this prefix will be listed.</span></span>
* <span data-ttu-id="7c86f-182">**useFlatBlobListing** – jak je uvedeno v hello [pojmenování a odkazování na kontejnery a objekty BLOB](#naming-and-referencing-containers-and-blobs) části, i když hello služby objektů Blob je schéma ploché úložiště, můžete vytvořit virtuální hierarchie názvy objektů BLOB s cestou informace.</span><span class="sxs-lookup"><span data-stu-id="7c86f-182">**useFlatBlobListing** - As mentioned in hello [Naming and referencing containers and blobs](#naming-and-referencing-containers-and-blobs) section, although hello Blob service is a flat storage scheme, you can create a virtual hierarchy by naming blobs with path information.</span></span> <span data-ttu-id="7c86f-183">Výpis bez dvojrozměrném však není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="7c86f-183">However, non-flat listing is currently not supported.</span></span> <span data-ttu-id="7c86f-184">Tato funkce bude brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7c86f-184">This feature is coming soon.</span></span> <span data-ttu-id="7c86f-185">Prozatím se tato hodnota by měla být **Ano**.</span><span class="sxs-lookup"><span data-stu-id="7c86f-185">For now, this value should be **YES**.</span></span>
* <span data-ttu-id="7c86f-186">**blobListingDetails** -tooinclude které položky můžete zadat při výpisu objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="7c86f-186">**blobListingDetails** - You can specify which items tooinclude when listing blobs</span></span>
  * <span data-ttu-id="7c86f-187">_AZSBlobListingDetailsNone_: seznam pouze potvrdit objektů BLOB a nevrátí metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="7c86f-187">_AZSBlobListingDetailsNone_: List only committed blobs, and do not return blob metadata.</span></span>
  * <span data-ttu-id="7c86f-188">_AZSBlobListingDetailsSnapshots_: seznam potvrdit objektů BLOB a objektů blob snímky.</span><span class="sxs-lookup"><span data-stu-id="7c86f-188">_AZSBlobListingDetailsSnapshots_: List committed blobs and blob snapshots.</span></span>
  * <span data-ttu-id="7c86f-189">_AZSBlobListingDetailsMetadata_: načíst metadata objektu blob pro každý objekt blob, vrátí se v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="7c86f-189">_AZSBlobListingDetailsMetadata_: Retrieve blob metadata for each blob returned in hello listing.</span></span>
  * <span data-ttu-id="7c86f-190">_AZSBlobListingDetailsUncommittedBlobs_: seznam objektů BLOB nepotvrzené a potvrzené.</span><span class="sxs-lookup"><span data-stu-id="7c86f-190">_AZSBlobListingDetailsUncommittedBlobs_: List committed and uncommitted blobs.</span></span>
  * <span data-ttu-id="7c86f-191">_AZSBlobListingDetailsCopy_: zahrnují kopírování vlastnosti v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="7c86f-191">_AZSBlobListingDetailsCopy_: Include copy properties in hello listing.</span></span>
  * <span data-ttu-id="7c86f-192">_AZSBlobListingDetailsAll_: seznam dostupných potvrdit objekty BLOB, nepotvrzené objekty BLOB a snímky a vrátí všechny metadata a zkopírujte stav pro tyto objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="7c86f-192">_AZSBlobListingDetailsAll_: List all available committed blobs, uncommitted blobs, and snapshots, and return all metadata and copy status for those blobs.</span></span>
* <span data-ttu-id="7c86f-193">**shluku** -hello maximální počet výsledků tooreturn pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="7c86f-193">**maxResults** - hello maximum number of results tooreturn for this operation.</span></span> <span data-ttu-id="7c86f-194">Použití -1 toonot nastavit omezení.</span><span class="sxs-lookup"><span data-stu-id="7c86f-194">Use -1 toonot set a limit.</span></span>
* <span data-ttu-id="7c86f-195">**completionHandler** -hello blok kódu tooexecute s výsledky hello hello výpis operaci.</span><span class="sxs-lookup"><span data-stu-id="7c86f-195">**completionHandler** - hello block of code tooexecute with hello results of hello listing operation.</span></span>

<span data-ttu-id="7c86f-196">V tomto příkladu je pomocná metoda použité toorecursively volání hello seznamu objektů BLOB metoda pokaždé, když se vrátí token pokračování.</span><span class="sxs-lookup"><span data-stu-id="7c86f-196">In this example, a helper method is used toorecursively call hello list blobs method every time a continuation token is returned.</span></span>

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a><span data-ttu-id="7c86f-197">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="7c86f-197">Download a blob</span></span>
<span data-ttu-id="7c86f-198">Následující příklad ukazuje, jak Hello toodownload objekt blob tooa NSString.</span><span class="sxs-lookup"><span data-stu-id="7c86f-198">hello following example shows how toodownload a blob tooa NSString object.</span></span>

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a><span data-ttu-id="7c86f-199">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="7c86f-199">Delete a blob</span></span>
<span data-ttu-id="7c86f-200">Následující příklad ukazuje, jak Hello toodelete objekt blob.</span><span class="sxs-lookup"><span data-stu-id="7c86f-200">hello following example shows how toodelete a blob.</span></span>

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="7c86f-201">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="7c86f-201">Delete a blob container</span></span>
<span data-ttu-id="7c86f-202">Následující příklad ukazuje, jak Hello toodelete kontejner.</span><span class="sxs-lookup"><span data-stu-id="7c86f-202">hello following example shows how toodelete a container.</span></span>

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a><span data-ttu-id="7c86f-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c86f-203">Next steps</span></span>
<span data-ttu-id="7c86f-204">Teď, když jste se naučili jak toouse úložiště objektů Blob z iOS, použijte tyto odkazy toolearn více o hello knihovně iOS a hello služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="7c86f-204">Now that you've learned how toouse Blob Storage from iOS, follow these links toolearn more about hello iOS library and hello Storage service.</span></span>

* [<span data-ttu-id="7c86f-205">Azure Klientská knihovna pro úložiště pro iOS</span><span class="sxs-lookup"><span data-stu-id="7c86f-205">Azure Storage Client Library for iOS</span></span>](https://github.com/azure/azure-storage-ios)
* [<span data-ttu-id="7c86f-206">Azure Storage iOS referenční dokumentaci k nástroji</span><span class="sxs-lookup"><span data-stu-id="7c86f-206">Azure Storage iOS Reference Documentation</span></span>](http://azure.github.io/azure-storage-ios/)
* [<span data-ttu-id="7c86f-207">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7c86f-207">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="7c86f-208">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7c86f-208">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage)

<span data-ttu-id="7c86f-209">Pokud máte dotazy týkající se této knihovny, klidně volné toopost tooour [fóru MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) nebo [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span><span class="sxs-lookup"><span data-stu-id="7c86f-209">If you have questions regarding this library, feel free toopost tooour [MSDN Azure forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) or [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span></span>
<span data-ttu-id="7c86f-210">Pokud máte návrhy na funkce pro Azure Storage, položit příliš[zpětnou vazbu úložiště Azure](https://feedback.azure.com/forums/217298-storage/).</span><span class="sxs-lookup"><span data-stu-id="7c86f-210">If you have feature suggestions for Azure Storage, please post too[Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span></span>

