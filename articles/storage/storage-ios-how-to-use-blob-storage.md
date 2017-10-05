---
title: "Jak používat Azure Blob storage z iOS | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
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
ms.openlocfilehash: cb2810636c8c23dbd476dc2adf58b17d1887d575
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-blob-storage-from-ios"></a><span data-ttu-id="b75b6-103">Používání úložiště Blob z iOS</span><span class="sxs-lookup"><span data-stu-id="b75b6-103">How to use Blob storage from iOS</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="b75b6-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b75b6-104">Overview</span></span>
<span data-ttu-id="b75b6-105">Tento článek vám ukáže, jak provádět běžné scénáře s využitím Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="b75b6-105">This article will show you how to perform common scenarios using Microsoft Azure Blob storage.</span></span> <span data-ttu-id="b75b6-106">Ukázky jsou napsané v Objective-C a použít [Klientská knihovna pro úložiště Azure pro iOS](https://github.com/Azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="b75b6-106">The samples are written in Objective-C and use the [Azure Storage Client Library for iOS](https://github.com/Azure/azure-storage-ios).</span></span> <span data-ttu-id="b75b6-107">Pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="b75b6-107">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="b75b6-108">Další informace o objekty BLOB, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="b75b6-108">For more information on blobs, see the [Next Steps](#next-steps) section.</span></span> <span data-ttu-id="b75b6-109">Můžete také stáhnout [ukázkovou aplikaci](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) rychle zobrazíte pomocí Azure Storage ve aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="b75b6-109">You can also download the [sample app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) to quickly see the use of Azure Storage in an iOS application.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a><span data-ttu-id="b75b6-110">Importovat knihovny iOS Azure Storage do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="b75b6-110">Import the Azure Storage iOS library into your application</span></span>
<span data-ttu-id="b75b6-111">Můžete importovat knihovně iOS Azure Storage do své aplikace buď pomocí [CocoaPod úložiště Azure](https://cocoapods.org/pods/AZSClient) nebo importem **Framework** souboru.</span><span class="sxs-lookup"><span data-stu-id="b75b6-111">You can import the Azure Storage iOS library into your application either by using the [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) or by importing the **Framework** file.</span></span> <span data-ttu-id="b75b6-112">CocoaPod je doporučeným způsobem, protože umožňuje integraci jednodušší, ale import ze souboru framework je šetrnější pro existující projekt knihovny.</span><span class="sxs-lookup"><span data-stu-id="b75b6-112">CocoaPod is the recommended way as it makes integrating the library easier, however importing from the framework file is less intrusive for your existing project.</span></span>

<span data-ttu-id="b75b6-113">Použití této knihovny, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="b75b6-113">To use this library, you need the following:</span></span>
- <span data-ttu-id="b75b6-114">iOS 8 +</span><span class="sxs-lookup"><span data-stu-id="b75b6-114">iOS 8+</span></span>
- <span data-ttu-id="b75b6-115">Xcode 7 +</span><span class="sxs-lookup"><span data-stu-id="b75b6-115">Xcode 7+</span></span>

## <a name="cocoapod"></a><span data-ttu-id="b75b6-116">CocoaPod</span><span class="sxs-lookup"><span data-stu-id="b75b6-116">CocoaPod</span></span>
1. <span data-ttu-id="b75b6-117">Pokud jste to ještě neudělali, [nainstalovat CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) ve vašem počítači otevřete okno terminálu a spuštěním následujícího příkazu</span><span class="sxs-lookup"><span data-stu-id="b75b6-117">If you haven't done so already, [Install CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) on your computer by opening a terminal window and running the following command</span></span>
    
    ```shell   
    sudo gem install cocoapods
    ```

2. <span data-ttu-id="b75b6-118">V dalším kroku v adresáři projektu (adresář obsahující souboru .xcodeproj) vytvořte nový soubor s názvem _Podfile_(bez přípony souboru).</span><span class="sxs-lookup"><span data-stu-id="b75b6-118">Next, in the project directory (the directory containing your .xcodeproj file), create a new file called _Podfile_(no file extension).</span></span> <span data-ttu-id="b75b6-119">Přidejte následující _Podfile_ a uložte.</span><span class="sxs-lookup"><span data-stu-id="b75b6-119">Add the following to _Podfile_ and save.</span></span>

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. <span data-ttu-id="b75b6-120">V okně terminálu přejděte do adresáře projektu a spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="b75b6-120">In the terminal window, navigate to the project directory and run the following command</span></span>

    ```shell    
    pod install
    ```

4. <span data-ttu-id="b75b6-121">Pokud vaše .xcodeproj otevřít v Xcode, zavřete ji.</span><span class="sxs-lookup"><span data-stu-id="b75b6-121">If your .xcodeproj is open in Xcode, close it.</span></span> <span data-ttu-id="b75b6-122">V adresáři projektu otevřete soubor nově vytvořený projekt, který bude mít příponu .xcworkspace.</span><span class="sxs-lookup"><span data-stu-id="b75b6-122">In your project directory open the newly created project file which will have the .xcworkspace extension.</span></span> <span data-ttu-id="b75b6-123">Toto je soubor, kterou budete pracujete z nyní na.</span><span class="sxs-lookup"><span data-stu-id="b75b6-123">This is the file you'll work from for now on.</span></span>

## <a name="framework"></a><span data-ttu-id="b75b6-124">Framework</span><span class="sxs-lookup"><span data-stu-id="b75b6-124">Framework</span></span>
<span data-ttu-id="b75b6-125">Druhý způsob na používání knihovny, je vytvořit rozhraní ručně:</span><span class="sxs-lookup"><span data-stu-id="b75b6-125">The other way to use the library is to build the framework manually:</span></span>

1. <span data-ttu-id="b75b6-126">Nejprve stáhnout nebo klonovat [úložišti azure-storage-ios](https://github.com/azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="b75b6-126">First, download or clone the [azure-storage-ios repo](https://github.com/azure/azure-storage-ios).</span></span>
2. <span data-ttu-id="b75b6-127">Přejděte do *azure-storage-ios* -> *Lib* -> *Klientská knihovna pro úložiště Azure*a otevřete `AZSClient.xcodeproj` v Xcode.</span><span class="sxs-lookup"><span data-stu-id="b75b6-127">Go into *azure-storage-ios* -> *Lib* -> *Azure Storage Client Library*, and open `AZSClient.xcodeproj` in Xcode.</span></span>
3. <span data-ttu-id="b75b6-128">V levém horním pro Xcode změňte schéma active z "Klientská knihovna pro úložiště Azure" na "Framework".</span><span class="sxs-lookup"><span data-stu-id="b75b6-128">At the top-left of Xcode, change the active scheme from "Azure Storage Client Library" to "Framework".</span></span>
4. <span data-ttu-id="b75b6-129">Sestavení projektu (⌘ + B).</span><span class="sxs-lookup"><span data-stu-id="b75b6-129">Build the project (⌘+B).</span></span> <span data-ttu-id="b75b6-130">Tím se vytvoří `AZSClient.framework` soubor na pracovní ploše.</span><span class="sxs-lookup"><span data-stu-id="b75b6-130">This will create an `AZSClient.framework` file on your Desktop.</span></span>

<span data-ttu-id="b75b6-131">Pak můžete importovat soubor framework do své aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b75b6-131">You can then import the framework file into your application by doing the following:</span></span>

1. <span data-ttu-id="b75b6-132">Vytvoření nového projektu nebo otevře existující projekt v Xcode.</span><span class="sxs-lookup"><span data-stu-id="b75b6-132">Create a new project or open up your existing project in Xcode.</span></span>
2. <span data-ttu-id="b75b6-133">Přetáhnout myší `AZSClient.framework` do Xcode navigátoru projektů.</span><span class="sxs-lookup"><span data-stu-id="b75b6-133">Drag and drop the `AZSClient.framework` into your Xcode project navigator.</span></span>
3. <span data-ttu-id="b75b6-134">Vyberte *kopírovat položky v případě potřeby*a klikněte na *Dokončit*.</span><span class="sxs-lookup"><span data-stu-id="b75b6-134">Select *Copy items if needed*, and click on *Finish*.</span></span>
4. <span data-ttu-id="b75b6-135">Klikněte na projekt v levém navigačním panelu a klikněte na *Obecné* v horní části editoru projektu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-135">Click on your project in the left-hand navigation and click the *General* tab at the top of the project editor.</span></span>
5. <span data-ttu-id="b75b6-136">V části *propojené architektury a knihovny* části, klikněte na tlačítko Přidat (+).</span><span class="sxs-lookup"><span data-stu-id="b75b6-136">Under the *Linked Frameworks and Libraries* section, click the Add button (+).</span></span>
6. <span data-ttu-id="b75b6-137">Vyhledejte v seznamu knihoven už poskytuje `libxml2.2.tbd` a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-137">In the list of libraries already provided, search for `libxml2.2.tbd` and add it to your project.</span></span>

## <a name="import-the-library"></a><span data-ttu-id="b75b6-138">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="b75b6-138">Import the Library</span></span> 
```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

<span data-ttu-id="b75b6-139">Pokud používáte Swift, je potřeba vytvořit hlavičku přemostění a importovat < AZSClient/AZSClient.h > existuje:</span><span class="sxs-lookup"><span data-stu-id="b75b6-139">If you are using Swift, you will need to create a bridging header and import <AZSClient/AZSClient.h> there:</span></span>

1. <span data-ttu-id="b75b6-140">Vytvořte soubor hlavičky `Bridging-Header.h`a přidejte výše uvedený příkaz importu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-140">Create a header file `Bridging-Header.h`, and add the above import statement.</span></span>
2. <span data-ttu-id="b75b6-141">Přejděte na *nastavení sestavení* kartě a vyhledejte *hlavičky přemostění jazyka Objective-C*.</span><span class="sxs-lookup"><span data-stu-id="b75b6-141">Go to the *Build Settings* tab, and search for *Objective-C Bridging Header*.</span></span>
3. <span data-ttu-id="b75b6-142">Dvakrát klikněte na pole *hlavičky přemostění jazyka Objective-C* a přidejte cestu k souboru hlavičky:`ProjectName/Bridging-Header.h`</span><span class="sxs-lookup"><span data-stu-id="b75b6-142">Double-click on the field of *Objective-C Bridging Header* and add the path to your header file: `ProjectName/Bridging-Header.h`</span></span>
4. <span data-ttu-id="b75b6-143">Sestavení projektu (⌘ + B) Chcete-li ověřit, že byl hlavičku přemostění zachyceny pomocí Xcode.</span><span class="sxs-lookup"><span data-stu-id="b75b6-143">Build the project (⌘+B) to verify that the bridging header was picked up by Xcode.</span></span>
5. <span data-ttu-id="b75b6-144">Začít používat knihovny přímo v žádném Swift souboru, není nutné pro příkazy pro import.</span><span class="sxs-lookup"><span data-stu-id="b75b6-144">Start using the library directly in any Swift file, there is no need for import statements.</span></span>

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a><span data-ttu-id="b75b6-145">Asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="b75b6-145">Asynchronous Operations</span></span>
> [!NOTE]
> <span data-ttu-id="b75b6-146">Všechny metody, které provádějí požadavek služby jsou asynchronních operací.</span><span class="sxs-lookup"><span data-stu-id="b75b6-146">All methods that perform a request against the service are asynchronous operations.</span></span> <span data-ttu-id="b75b6-147">Ukázky kódu obsahují splnit tyto metody dokončení obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="b75b6-147">In the code samples, you'll find that these methods have a completion handler.</span></span> <span data-ttu-id="b75b6-148">Kód dokončení obslužná rutina se spustí **po** požadavek je dokončen.</span><span class="sxs-lookup"><span data-stu-id="b75b6-148">Code inside the completion handler will run **after** the request is completed.</span></span> <span data-ttu-id="b75b6-149">Kód po dokončení obslužná rutina se spustí **při** se provádí požadavek.</span><span class="sxs-lookup"><span data-stu-id="b75b6-149">Code after the completion handler will run **while** the request is being made.</span></span>
> 
> 

## <a name="create-a-container"></a><span data-ttu-id="b75b6-150">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="b75b6-150">Create a container</span></span>
<span data-ttu-id="b75b6-151">Každý objekt blob v Azure Storage se musí nacházet v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b75b6-151">Every blob in Azure Storage must reside in a container.</span></span> <span data-ttu-id="b75b6-152">Následující příklad ukazuje, jak vytvořit kontejner, nazývá *newcontainer*, ve vašem účtu úložiště, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b75b6-152">The following example shows how to create a container, called *newcontainer*, in your Storage account if it doesn't already exist.</span></span> <span data-ttu-id="b75b6-153">Když vyberete název vašeho kontejneru, mějte na paměti pojmenování pravidel uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="b75b6-153">When choosing a name for your container, be mindful of the naming rules mentioned above.</span></span>

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

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

<span data-ttu-id="b75b6-154">Můžete potvrdit, že to funguje na základě [Microsoft Azure Storage Explorer](http://storageexplorer.com) a ověřování, který *newcontainer* je v seznamu kontejnery pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b75b6-154">You can confirm that this works by looking at the [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that *newcontainer* is in the list of containers for your Storage account.</span></span>

## <a name="set-container-permissions"></a><span data-ttu-id="b75b6-155">Nastavte oprávnění kontejneru</span><span class="sxs-lookup"><span data-stu-id="b75b6-155">Set Container Permissions</span></span>
<span data-ttu-id="b75b6-156">Kontejneru oprávnění jsou nakonfigurovaná pro **privátní** přístup ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b75b6-156">A container's permissions are configured for **Private** access by default.</span></span> <span data-ttu-id="b75b6-157">Kontejnery však obsahují několik různých volbách pro kontejner přístupu:</span><span class="sxs-lookup"><span data-stu-id="b75b6-157">However, containers provide a few different options for container access:</span></span>

* <span data-ttu-id="b75b6-158">**Privátní**: kontejnerů a objektů blob dat může číst pouze vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-158">**Private**: Container and blob data can be read by the account owner only.</span></span>
* <span data-ttu-id="b75b6-159">**Objekt BLOB**: data objektu Blob v tomto kontejneru mohou číst přes anonymní žádost, ale kontejneru data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b75b6-159">**Blob**: Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="b75b6-160">Klienty nelze vytvořit výčet objektů BLOB v kontejneru přes anonymní žádost.</span><span class="sxs-lookup"><span data-stu-id="b75b6-160">Clients cannot enumerate blobs within the container via anonymous request.</span></span>
* <span data-ttu-id="b75b6-161">**Kontejner**: přes anonymní dotazy můžete číst data kontejnerů a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="b75b6-161">**Container**: Container and blob data can be read via anonymous request.</span></span> <span data-ttu-id="b75b6-162">Klienti, můžete vytvořit výčet objektů BLOB v kontejneru přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b75b6-162">Clients can enumerate blobs within the container via anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="b75b6-163">Následující příklad ukazuje, jak vytvořit kontejner s **kontejneru** přístupová oprávnění, které vám umožní přístup k veřejné, jen pro čtení pro všechny uživatele v síti Internet:</span><span class="sxs-lookup"><span data-stu-id="b75b6-163">The following example shows you how to create a container with **Container** access permissions, which will allow public, read-only access for all users on the Internet:</span></span>

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

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="b75b6-164">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="b75b6-164">Upload a blob into a container</span></span>
<span data-ttu-id="b75b6-165">Jak je uvedeno v [koncepty služby objektů Blob](#blob-service-concepts) část, úložiště objektů Blob nabízí tři různé typy objektů blob: objekty BLOB bloků, doplňovací objekty BLOB a objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="b75b6-165">As mentioned in the [Blob service concepts](#blob-service-concepts) section, Blob Storage offers three different types of blobs: block blobs, append blobs, and page blobs.</span></span> <span data-ttu-id="b75b6-166">Knihovna iOS Azure Storage podporuje všechny tři typy objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="b75b6-166">The Azure Storage iOS library supports all three types of blobs.</span></span> <span data-ttu-id="b75b6-167">Ve většině případů se jako vhodný typ k použití doporučuje objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="b75b6-167">In most cases, block blob is the recommended type to use.</span></span>

<span data-ttu-id="b75b6-168">Následující příklad ukazuje, jak nahrát objekt blob bloku z NSString.</span><span class="sxs-lookup"><span data-stu-id="b75b6-168">The following example shows how to upload a block blob from an NSString.</span></span> <span data-ttu-id="b75b6-169">Pokud objekt blob se stejným názvem již existuje v tomto kontejneru, budou přepsány obsah tohoto objektu blob.</span><span class="sxs-lookup"><span data-stu-id="b75b6-169">If a blob with the same name already exists in this container, the contents of this blob will be overwritten.</span></span>

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

                // Upload blob to Storage
                [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

<span data-ttu-id="b75b6-170">Můžete potvrdit, že to funguje na základě [Microsoft Azure Storage Explorer](http://storageexplorer.com) a ověřování, který kontejneru, *containerpublic*, obsahuje objekt blob, *sampleblob*.</span><span class="sxs-lookup"><span data-stu-id="b75b6-170">You can confirm that this works by looking at the [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that the container, *containerpublic*, contains the blob, *sampleblob*.</span></span> <span data-ttu-id="b75b6-171">V této ukázce jsme použili kontejner veřejné, tak můžete také ověřit, zda tato aplikace funguje tak, že přejdete na objekty BLOB URI:</span><span class="sxs-lookup"><span data-stu-id="b75b6-171">In this sample, we used a public container so you can also verify that this application worked by going to the blobs URI:</span></span>

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

<span data-ttu-id="b75b6-172">Kromě nahrát objekt blob bloku z NSString, existují podobné metody pro NSData, NSInputStream nebo místního souboru.</span><span class="sxs-lookup"><span data-stu-id="b75b6-172">In addition to uploading a block blob from an NSString, similar methods exist for NSData, NSInputStream, or a local file.</span></span>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="b75b6-173">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="b75b6-173">List the blobs in a container</span></span>
<span data-ttu-id="b75b6-174">Následující příklad ukazuje, jak získat seznam všech objektů BLOB v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b75b6-174">The following example shows how to list all blobs in a container.</span></span> <span data-ttu-id="b75b6-175">Při provádění této operace, mějte na paměti následující parametry:</span><span class="sxs-lookup"><span data-stu-id="b75b6-175">When performing this operation, be mindful of the following parameters:</span></span>     

* <span data-ttu-id="b75b6-176">**continuationToken** -tokenu představuje pokračování kde by se měl spustit operace výpisu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-176">**continuationToken** - The continuation token represents where the listing operation should start.</span></span> <span data-ttu-id="b75b6-177">Pokud je k dispozici žádné token, zobrazí seznam objektů BLOB od začátku.</span><span class="sxs-lookup"><span data-stu-id="b75b6-177">If no token is provided, it will list blobs from the beginning.</span></span> <span data-ttu-id="b75b6-178">Libovolný počet objektů BLOB může být uvedený od nuly až do maximální sady.</span><span class="sxs-lookup"><span data-stu-id="b75b6-178">Any number of blobs can be listed, from zero up to a set maximum.</span></span> <span data-ttu-id="b75b6-179">I když tato metoda vrátí hodnotu nula výsledky, pokud `results.continuationToken` není nulovou hodnotu, může být další objekty BLOB ve službě nejsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="b75b6-179">Even if this method returns zero results, if `results.continuationToken` is not nil, there may be more blobs on the service that have not been listed.</span></span>
* <span data-ttu-id="b75b6-180">**Předpona** -můžete určit předpona, kterou chcete použít pro seznam objektů blob.</span><span class="sxs-lookup"><span data-stu-id="b75b6-180">**prefix** - You can specify the prefix to use for blob listing.</span></span> <span data-ttu-id="b75b6-181">Pouze objekty BLOB, které začínají s touto předponou budou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="b75b6-181">Only blobs that begin with this prefix will be listed.</span></span>
* <span data-ttu-id="b75b6-182">**useFlatBlobListing** – jak je uvedeno v [pojmenování a odkazování na kontejnery a objekty BLOB](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) části, i když služba objektů Blob je schéma ploché úložiště, můžete vytvořit virtuální hierarchie názvy objektů BLOB s cestou informace.</span><span class="sxs-lookup"><span data-stu-id="b75b6-182">**useFlatBlobListing** - As mentioned in the [Naming and referencing containers and blobs](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) section, although the Blob service is a flat storage scheme, you can create a virtual hierarchy by naming blobs with path information.</span></span> <span data-ttu-id="b75b6-183">Výpis bez dvojrozměrném však není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="b75b6-183">However, non-flat listing is currently not supported.</span></span> <span data-ttu-id="b75b6-184">Tato funkce bude brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b75b6-184">This feature is coming soon.</span></span> <span data-ttu-id="b75b6-185">Prozatím se tato hodnota by měla být **Ano**.</span><span class="sxs-lookup"><span data-stu-id="b75b6-185">For now, this value should be **YES**.</span></span>

* <span data-ttu-id="b75b6-186">**blobListingDetails** -zadáte vhodných při výpisu objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="b75b6-186">**blobListingDetails** - You can specify which items to include when listing blobs</span></span>
  * <span data-ttu-id="b75b6-187">_AZSBlobListingDetailsNone_: seznam pouze potvrdit objektů BLOB a nevrátí metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="b75b6-187">_AZSBlobListingDetailsNone_: List only committed blobs, and do not return blob metadata.</span></span>
  * <span data-ttu-id="b75b6-188">_AZSBlobListingDetailsSnapshots_: seznam potvrdit objektů BLOB a objektů blob snímky.</span><span class="sxs-lookup"><span data-stu-id="b75b6-188">_AZSBlobListingDetailsSnapshots_: List committed blobs and blob snapshots.</span></span>
  * <span data-ttu-id="b75b6-189">_AZSBlobListingDetailsMetadata_: načíst metadata objektu blob pro každý objekt blob vrátila ve výpisu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-189">_AZSBlobListingDetailsMetadata_: Retrieve blob metadata for each blob returned in the listing.</span></span>
  * <span data-ttu-id="b75b6-190">_AZSBlobListingDetailsUncommittedBlobs_: seznam objektů BLOB nepotvrzené a potvrzené.</span><span class="sxs-lookup"><span data-stu-id="b75b6-190">_AZSBlobListingDetailsUncommittedBlobs_: List committed and uncommitted blobs.</span></span>
  * <span data-ttu-id="b75b6-191">_AZSBlobListingDetailsCopy_: zahrnují kopírování vlastnosti ve výpisu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-191">_AZSBlobListingDetailsCopy_: Include copy properties in the listing.</span></span>
  * <span data-ttu-id="b75b6-192">_AZSBlobListingDetailsAll_: seznam dostupných potvrdit objekty BLOB, nepotvrzené objekty BLOB a snímky a vrátí všechny metadata a zkopírujte stav pro tyto objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="b75b6-192">_AZSBlobListingDetailsAll_: List all available committed blobs, uncommitted blobs, and snapshots, and return all metadata and copy status for those blobs.</span></span>
* <span data-ttu-id="b75b6-193">**shluku** – maximální počet výsledků, které se chcete vrátit tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="b75b6-193">**maxResults** - The maximum number of results to return for this operation.</span></span> <span data-ttu-id="b75b6-194">Není nastavit omezení pomocí -1.</span><span class="sxs-lookup"><span data-stu-id="b75b6-194">Use -1 to not set a limit.</span></span>
* <span data-ttu-id="b75b6-195">**completionHandler** – blok kódu provést s výsledky operace výpisu.</span><span class="sxs-lookup"><span data-stu-id="b75b6-195">**completionHandler** - The block of code to execute with the results of the listing operation.</span></span>

<span data-ttu-id="b75b6-196">V tomto příkladu se používá ke Pomocná metoda rekurzivní volání seznamu objektů BLOB metoda pokaždé, když se vrátí token pokračování.</span><span class="sxs-lookup"><span data-stu-id="b75b6-196">In this example, a helper method is used to recursively call the list blobs method every time a continuation token is returned.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="b75b6-197">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="b75b6-197">Download a blob</span></span>
<span data-ttu-id="b75b6-198">Následující příklad ukazuje, jak stáhnout objektu blob na objekt NSString.</span><span class="sxs-lookup"><span data-stu-id="b75b6-198">The following example shows how to download a blob to a NSString object.</span></span>

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

## <a name="delete-a-blob"></a><span data-ttu-id="b75b6-199">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="b75b6-199">Delete a blob</span></span>
<span data-ttu-id="b75b6-200">Následující příklad ukazuje, jak chcete odstranit objekt blob.</span><span class="sxs-lookup"><span data-stu-id="b75b6-200">The following example shows how to delete a blob.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="b75b6-201">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="b75b6-201">Delete a blob container</span></span>
<span data-ttu-id="b75b6-202">Následující příklad ukazuje, jak odstranit kontejner.</span><span class="sxs-lookup"><span data-stu-id="b75b6-202">The following example shows how to delete a container.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b75b6-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b75b6-203">Next steps</span></span>
<span data-ttu-id="b75b6-204">Teď, když jste se naučili používání úložiště Blob z iOS, postupujte podle následujících odkazech na další informace o knihovně iOS a služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="b75b6-204">Now that you've learned how to use Blob Storage from iOS, follow these links to learn more about the iOS library and the Storage service.</span></span>

* [<span data-ttu-id="b75b6-205">Azure Klientská knihovna pro úložiště pro iOS</span><span class="sxs-lookup"><span data-stu-id="b75b6-205">Azure Storage Client Library for iOS</span></span>](https://github.com/azure/azure-storage-ios)
* [<span data-ttu-id="b75b6-206">Azure Storage iOS referenční dokumentaci k nástroji</span><span class="sxs-lookup"><span data-stu-id="b75b6-206">Azure Storage iOS Reference Documentation</span></span>](http://azure.github.io/azure-storage-ios/)
* [<span data-ttu-id="b75b6-207">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b75b6-207">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="b75b6-208">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b75b6-208">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage)

<span data-ttu-id="b75b6-209">Pokud máte dotazy týkající se této knihovny, klidně k vystavování příspěvků na našem [fóru MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) nebo [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span><span class="sxs-lookup"><span data-stu-id="b75b6-209">If you have questions regarding this library, feel free to post to our [MSDN Azure forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) or [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span></span>
<span data-ttu-id="b75b6-210">Pokud máte návrhy na funkce pro Azure Storage, prosím odeslání na [zpětnou vazbu úložiště Azure](https://feedback.azure.com/forums/217298-storage/).</span><span class="sxs-lookup"><span data-stu-id="b75b6-210">If you have feature suggestions for Azure Storage, please post to [Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span></span>

