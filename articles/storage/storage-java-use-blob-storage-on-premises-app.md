---
title: "aaaOn místní aplikace pomocí úložiště objektů blob (Java) | Microsoft Docs"
description: "Zjistěte, jak toocreate konzolovou aplikaci, která odešle tooAzure bitové kopie a potom zobrazí hello bitové kopie v prohlížeči. Ukázky kódu v jazyce Java."
services: storage
documentationcenter: java
author: mmacy
manager: carmonm
editor: tysonn
ms.assetid: ccc9a7d7-6fe4-467b-b7fd-a73f17539e3f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/17/2016
ms.author: marsma
ms.openlocfilehash: ed8eb4c1045691c25abe94bf6c1b18b797adc3e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="on-premises-application-with-blob-storage"></a><span data-ttu-id="dbb2d-104">Místní aplikace pomocí úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="dbb2d-104">On-premises application with blob storage</span></span>
## <a name="overview"></a><span data-ttu-id="dbb2d-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="dbb2d-105">Overview</span></span>
<span data-ttu-id="dbb2d-106">Hello následující příklad ukazuje, je použití úložiště Azure pro uložení bitové kopie v Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-106">hello following example shows you how you can use Azure storage to store images in Azure.</span></span> <span data-ttu-id="dbb2d-107">Hello kód v tomto článku je konzolovou aplikaci, která odešle tooAzure bitové kopie a poté vytvoří souboru HTML, který se zobrazí v prohlížeči obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-107">hello code in this article is for a console application that uploads an image tooAzure, and then creates an HTML file that displays hello image in your browser.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dbb2d-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dbb2d-108">Prerequisites</span></span>
* <span data-ttu-id="dbb2d-109">Java Developer Kit (JDK), verze 1.6 nebo novější, je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-109">A Java Developer Kit (JDK), version 1.6 or later, is installed.</span></span>
* <span data-ttu-id="dbb2d-110">je nainstalována Hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-110">hello Azure SDK is installed.</span></span>
* <span data-ttu-id="dbb2d-111">Hello JAR pro hello knihovny Azure Libraries for Java a všechny příslušné závislostí JAR jsou nainstalované a jsou v cestě sestavení hello používá vaše kompilátor Javy.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-111">hello JAR for hello Azure Libraries for Java, and any applicable dependency JARs, are installed and are in hello build path used by your Java compiler.</span></span> <span data-ttu-id="dbb2d-112">Informace o instalaci hello knihovny Azure Libraries for Java najdete v tématu [hello stažení sady Azure SDK pro jazyk Java](../java-download-azure-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="dbb2d-112">For information about installing hello Azure Libraries for Java, see [Download hello Azure SDK for Java](../java-download-azure-sdk.md).</span></span>
* <span data-ttu-id="dbb2d-113">Má nastaven účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-113">An Azure storage account has been set up.</span></span> <span data-ttu-id="dbb2d-114">Hello název účtu a klíč účtu pro účet úložiště hello se použije hello kód v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-114">hello account name and account key for hello storage account will be used by hello code in this article.</span></span> <span data-ttu-id="dbb2d-115">V tématu [jak tooCreate účet úložiště](storage-create-storage-account.md#create-a-storage-account) informace o vytvoření účtu úložiště, a [zobrazení a zkopírování přístupových klíčů úložiště](storage-create-storage-account.md#view-and-copy-storage-access-keys) informace o načtení hello klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-115">See [How tooCreate a Storage Account](storage-create-storage-account.md#create-a-storage-account) for information about creating a storage account, and [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) for information about retrieving hello account key.</span></span>
* <span data-ttu-id="dbb2d-116">Vytvoříte místní bitové kopie souboru s názvem uložené v hello cesta c:\\myimages\\image1.jpg.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-116">You have created a local image file named stored at hello path c:\\myimages\\image1.jpg.</span></span> <span data-ttu-id="dbb2d-117">Můžete taky upravit **FileInputStream** konstruktor v hello příklad toouse název a cesta k souboru jinou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-117">Alternatively, modify the **FileInputStream** constructor in hello example toouse a different image path and file name.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="toouse-azure-blob-storage-tooupload-a-file"></a><span data-ttu-id="dbb2d-118">tooupload úložiště objektů blob v Azure toouse soubor</span><span class="sxs-lookup"><span data-stu-id="dbb2d-118">toouse Azure blob storage tooupload a file</span></span>
<span data-ttu-id="dbb2d-119">Podrobný postup je uveden zde.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-119">A step-by-step procedure is presented here.</span></span> <span data-ttu-id="dbb2d-120">Pokud chcete tooskip dopředu, se zobrazí celý kód hello později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-120">If you'd like tooskip ahead, hello entire code is presented later in this article.</span></span>

<span data-ttu-id="dbb2d-121">Začněte tím, že včetně importy pro třídy úložiště Azure základní hello, hello třídy klienta objektů blob v Azure, hello Java vstupně-výstupní operace třídy a hello hello kódu **URISyntaxException** třídy.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-121">Begin hello code by including imports for hello Azure core storage classes, hello Azure blob client classes, hello Java IO classes, and hello **URISyntaxException** class.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

<span data-ttu-id="dbb2d-122">Deklarace třídy s názvem **StorageSample**a zahrnovat hello závorka, **{**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-122">Declare a class named **StorageSample**, and include hello open bracket, **{**.</span></span>

```java
public class StorageSample {
```

<span data-ttu-id="dbb2d-123">V rámci hello **StorageSample** třídy, deklarovat proměnnou string, který bude obsahovat hello výchozí koncový bod protokol, název účtu úložiště a přístupový klíč k úložišti, jak je uvedeno v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-123">Within hello **StorageSample** class, declare a string variable that will contain hello default endpoint protocol, your storage account name, and your storage access key, as specified in your Azure storage account.</span></span> <span data-ttu-id="dbb2d-124">Nahraďte zástupný symbol hodnoty hello **vaše\_účet\_název** a **vaše\_účet\_klíč** s vlastní název účtu a klíč účtu v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-124">Replace hello placeholder values **your\_account\_name** and **your\_account\_key** with your own account name and account key, respectively.</span></span>

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

<span data-ttu-id="dbb2d-125">Přidat do vaší deklaraci pro **hlavní**, zahrnout **zkuste** blokovat a zahrnovat hello potřeby otevřete závorky, **{**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-125">Add in your declaration for **main**, include a **try** block, and include hello necessary open brackets, **{**.</span></span>

```java
    public static void main(String[] args)
    {
        try
        {
```

<span data-ttu-id="dbb2d-126">Deklarace proměnné hello následující typ (hello popisy jsou pro jejich použití v tomto příkladu):</span><span class="sxs-lookup"><span data-stu-id="dbb2d-126">Declare variables of hello following type (hello descriptions are for how they are used in this example):</span></span>

* <span data-ttu-id="dbb2d-127">**CloudStorageAccount**: používané tooinitialize hello účet objekt s váš název účtu úložiště Azure a klíč a toocreate klientského objektu blob.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-127">**CloudStorageAccount**: Used tooinitialize hello account object with your Azure storage account name and key, and toocreate the blob client object.</span></span>
* <span data-ttu-id="dbb2d-128">**CloudBlobClient**: používá služby objektů blob tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-128">**CloudBlobClient**: Used tooaccess hello blob service.</span></span>
* <span data-ttu-id="dbb2d-129">**CloudBlobContainer**: používané toocreate kontejner objektů blob seznamu objektů BLOB v kontejneru hello a odstranění hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-129">**CloudBlobContainer**: Used toocreate a blob container, list the blobs in hello container, and delete hello container.</span></span>
* <span data-ttu-id="dbb2d-130">**CloudBlockBlob**: používané tooupload kontejner toothe obrázek místní soubor.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-130">**CloudBlockBlob**: Used tooupload a local image file toothe container.</span></span>

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

<span data-ttu-id="dbb2d-131">Přiřadit hodnotu toohello **účet** proměnné.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-131">Assign a value toohello **account** variable.</span></span>

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

<span data-ttu-id="dbb2d-132">Přiřadit hodnotu toohello **serviceClient** proměnné.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-132">Assign a value toohello **serviceClient** variable.</span></span>

```java
serviceClient = account.createCloudBlobClient();
```

<span data-ttu-id="dbb2d-133">Přiřadit hodnotu toohello **kontejneru** proměnné.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-133">Assign a value toohello **container** variable.</span></span> <span data-ttu-id="dbb2d-134">Budete se nám získat odkaz na kontejner tooa s názvem **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-134">We'll get a reference tooa container named **gettingstarted**.</span></span>

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

<span data-ttu-id="dbb2d-135">Vytvoření kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-135">Create hello container.</span></span> <span data-ttu-id="dbb2d-136">Tato metoda vytvoří kontejner hello, pokud neexistuje (a vrátit **true**).</span><span class="sxs-lookup"><span data-stu-id="dbb2d-136">This method will create hello container if it doesn't exist (and return **true**).</span></span> <span data-ttu-id="dbb2d-137">Pokud hello kontejner neexistuje, vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-137">If hello container does exist, it will return **false**.</span></span> <span data-ttu-id="dbb2d-138">Alternativu příliš**createIfNotExists** je hello **vytvořit** (která vrátí chybu, pokud kontejner hello již existuje).</span><span class="sxs-lookup"><span data-stu-id="dbb2d-138">An alternative too**createIfNotExists** is hello **create** method (which will return an error if hello container already exists).</span></span>

```java
container.createIfNotExists();
```

<span data-ttu-id="dbb2d-139">Nastavit anonymní přístup pro kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-139">Set anonymous access for hello container.</span></span>

```java
// Set anonymous access on hello container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

<span data-ttu-id="dbb2d-140">Získání referenční dokumentace toohello objekt blob bloku, která bude představovat hello objektů blob v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-140">Get a reference toohello block blob, which will represent hello blob in Azure storage.</span></span>

```java
blob = container.getBlockBlobReference("image1.jpg");
```

<span data-ttu-id="dbb2d-141">Použití hello **souboru** konstruktor tooget soubor toohello místně vytvořené odkaz, který bude nahrávat.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-141">Use hello **File** constructor tooget a reference toohello locally created file that you will upload.</span></span> <span data-ttu-id="dbb2d-142">Zkontrolujte, zda že jste vytvořili tento soubor před spuštěním kódu hello.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-142">Ensure you have created this file before running hello code.</span></span>

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

<span data-ttu-id="dbb2d-143">Nahrát hello místního souboru prostřednictvím volání toohello **CloudBlockBlob.upload** metoda.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-143">Upload hello local file through a call toohello **CloudBlockBlob.upload** method.</span></span> <span data-ttu-id="dbb2d-144">první parametr toohello Hello **CloudBlockBlob.upload** je metoda **FileInputStream** objektu, že představuje hello místního souboru, který bude nahrán tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-144">hello first parameter toohello **CloudBlockBlob.upload** method is a **FileInputStream** object that represents hello local file that will be uploaded tooAzure storage.</span></span> <span data-ttu-id="dbb2d-145">druhý parametr Hello je hello velikost v bajtech, hello souboru.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-145">hello second parameter is hello size, in bytes, of hello file.</span></span>

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

<span data-ttu-id="dbb2d-146">Volání pomocné funkce s názvem **MakeHTMLPage**, toomake základní HTML stránky, která obsahuje  **&lt;image&gt;**  element s hello zdroje sady toohello blob, který je teď ve vašem Azure účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-146">Call a helper function named **MakeHTMLPage**, toomake a basic HTML page that contains an **&lt;image&gt;** element with hello source set toohello blob that is now in your Azure storage account.</span></span> <span data-ttu-id="dbb2d-147">Hello kód pro **MakeHTMLPage** bude probírat později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-147">hello code for **MakeHTMLPage** will be discussed later in this article.</span></span>

```java
MakeHTMLPage(container);
```

<span data-ttu-id="dbb2d-148">Vytiskněte stavové zprávy a informace o hello vytvořit stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-148">Print out a status message and information about hello created HTML page.</span></span>

```java
System.out.println("Processing complete.");
System.out.println("Open index.html toosee hello images stored in your storage account.");
```

<span data-ttu-id="dbb2d-149">Zavřít hello **zkuste** bloku vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="dbb2d-149">Close hello **try** block by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dbb2d-150">Popisovač hello následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="dbb2d-150">Handle hello following exceptions:</span></span>

* <span data-ttu-id="dbb2d-151">**FileNotFoundException**: může být vyvolána hello **FileInputStream** nebo **FileOutputStream** konstruktory.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-151">**FileNotFoundException**: Can be thrown by hello **FileInputStream** or **FileOutputStream** constructors.</span></span>
* <span data-ttu-id="dbb2d-152">**StorageException**: může být vyvolána hello Azure Klientská knihovna pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-152">**StorageException**: Can be thrown by hello Azure client storage library.</span></span>
* <span data-ttu-id="dbb2d-153">**URISyntaxException**: může být vyvolána hello **ListBlobItem.getUri** metoda.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-153">**URISyntaxException**: Can be thrown by hello **ListBlobItem.getUri** method.</span></span>
* <span data-ttu-id="dbb2d-154">**Výjimka**: Obecné výjimek.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-154">**Exception**: Generic exception handling.</span></span>

<!-- -->

```java
catch (FileNotFoundException fileNotFoundException)
{
    System.out.print("FileNotFoundException encountered: ");
    System.out.println(fileNotFoundException.getMessage());
    System.exit(-1);
}
catch (StorageException storageException)
{
    System.out.print("StorageException encountered: ");
    System.out.println(storageException.getMessage());
    System.exit(-1);
}
catch (URISyntaxException uriSyntaxException)
{
    System.out.print("URISyntaxException encountered: ");
    System.out.println(uriSyntaxException.getMessage());
    System.exit(-1);
}
catch (Exception e)
{
    System.out.print("Exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="dbb2d-155">Zavřete **hlavní** vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="dbb2d-155">Close **main** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dbb2d-156">Vytvořit metodu s názvem **MakeHTMLPage** toocreate základní HTML stránky.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-156">Create a method named **MakeHTMLPage** toocreate a basic HTML page.</span></span> <span data-ttu-id="dbb2d-157">Tato metoda má parametr typu **CloudBlobContainer**, které budou použité tooiterate prostřednictvím hello seznam nahrané objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-157">This method has a parameter of type **CloudBlobContainer**, which will be used tooiterate through hello list of uploaded blobs.</span></span> <span data-ttu-id="dbb2d-158">Tato metoda vyvolá výjimku výjimky typu **FileNotFoundException**, což může být vyvolána hello **FileOutputStream** konstruktoru, a **URISyntaxException**, což může být vyvolané hello **ListBlobItem.getUri** metoda.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-158">This method will throw exceptions of type **FileNotFoundException**, which can be thrown by hello **FileOutputStream** constructor, and **URISyntaxException**, which can be thrown by hello **ListBlobItem.getUri** method.</span></span> <span data-ttu-id="dbb2d-159">Zahrnout levá hranatá závorka, **{**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-159">Include the opening bracket, **{**.</span></span>

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

<span data-ttu-id="dbb2d-160">Vytvořte místní soubor s názvem **index.html**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-160">Create a local file named **index.html**.</span></span>

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

<span data-ttu-id="dbb2d-161">Zápisu místního souboru toohello přidávání hello  **&lt;html&gt;**,  **&lt;záhlaví&gt;**, a  **&lt;textu&gt;**  elementy.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-161">Write toohello local file, adding in hello **&lt;html&gt;**, **&lt;header&gt;**, and **&lt;body&gt;** elements.</span></span>

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

<span data-ttu-id="dbb2d-162">Iterace hello seznam nahrané objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-162">Iterate through hello list of uploaded blobs.</span></span> <span data-ttu-id="dbb2d-163">Pro každý objekt blob stránky hello HTML vytvořit  **&lt;img&gt;**  element, který má jeho **src** atribut odeslaný hello identifikátor URI objektu hello blob, protože existuje ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-163">For each blob, in hello HTML page create an **&lt;img&gt;** element that has its **src** attribute sent to hello URI of hello blob as it exists in your Azure storage account.</span></span>
<span data-ttu-id="dbb2d-164">I když jste přidali pouze jeden obrázek v této ukázce, pokud jste přidali více, by tento kód iteraci všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-164">Although you added only one image in this sample, if you added more, this code would iterate all of them.</span></span>

<span data-ttu-id="dbb2d-165">Pro zjednodušení tento příklad předpokládá, že každý nahraném objektu blob je bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-165">For simplicity, this example assumes each uploaded blob is an image.</span></span> <span data-ttu-id="dbb2d-166">Pokud jste aktualizovali objektů BLOB než bitové kopie nebo objekty BLOB stránky místo objekty BLOB bloku, upravte kód hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-166">If you've updated blobs other than images, or page blobs instead of block blobs, adjust hello code as needed.</span></span>

```java
// Enumerate hello uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in hello HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

<span data-ttu-id="dbb2d-167">Zavřít hello  **&lt;textu&gt;**  elementu a hello  **&lt;html&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-167">Close hello **&lt;body&gt;** element and hello **&lt;html&gt;** element.</span></span>

```java
stream.println("</body>");
stream.println("</html>");
```

<span data-ttu-id="dbb2d-168">Zavřít hello místního souboru.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-168">Close hello local file.</span></span>

```java
stream.close();
```

<span data-ttu-id="dbb2d-169">Zavřete **MakeHTMLPage** vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="dbb2d-169">Close **MakeHTMLPage** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dbb2d-170">Zavřete **StorageSample** vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="dbb2d-170">Close **StorageSample** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dbb2d-171">Hello následuje hello kompletní kód v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-171">hello following is hello complete code for this example.</span></span> <span data-ttu-id="dbb2d-172">Mějte na paměti, toomodify hello zástupné hodnoty **vaše\_účet\_název** a **vaše\_účet\_klíč** toouse název účtu a účet v uvedeném pořadí klíče.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-172">Remember toomodify hello placeholder values **your\_account\_name** and **your\_account\_key** toouse your account name and account key, respectively.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;

// Create an image, c:\myimages\image1.jpg, prior toorunning this sample.
// Alternatively, change hello value used by hello FileInputStream constructor
// toouse a different image path and file that you have already created.
public class StorageSample {

    public static final String storageConnectionString =
            "DefaultEndpointsProtocol=http;" +
                    "AccountName=your_account_name;" +
                    "AccountKey=your_account_name";

    public static void main(String[] args) {
        try {
            CloudStorageAccount account;
            CloudBlobClient serviceClient;
            CloudBlobContainer container;
            CloudBlockBlob blob;

            account = CloudStorageAccount.parse(storageConnectionString);
            serviceClient = account.createCloudBlobClient();
            // Container name must be lower case.
            container = serviceClient.getContainerReference("gettingstarted");
            container.createIfNotExists();

            // Set anonymous access on hello container.
            BlobContainerPermissions containerPermissions;
            containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // Upload an image file.
            blob = container.getBlockBlobReference("image1.jpg");

            File fileReference = new File("c:\\myimages\\image1.jpg");
            blob.upload(new FileInputStream(fileReference), fileReference.length());

            // At this point hello image is uploaded.
            // Next, create an HTML page that lists all of hello uploaded images.
            MakeHTMLPage(container);

            System.out.println("Processing complete.");
            System.out.println("Open index.html toosee hello images stored in your storage account.");

        } catch (FileNotFoundException fileNotFoundException) {
            System.out.print("FileNotFoundException encountered: ");
            System.out.println(fileNotFoundException.getMessage());
            System.exit(-1);
        } catch (StorageException storageException) {
            System.out.print("StorageException encountered: ");
            System.out.println(storageException.getMessage());
            System.exit(-1);
        } catch (URISyntaxException uriSyntaxException) {
            System.out.print("URISyntaxException encountered: ");
            System.out.println(uriSyntaxException.getMessage());
            System.exit(-1);
        } catch (Exception e) {
            System.out.print("Exception encountered: ");
            System.out.println(e.getMessage());
            System.exit(-1);
        }
    }

    // Create an HTML page that can be used toodisplay hello uploaded images.
    // This example assumes all of hello blobs are for images.
    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {
        PrintStream stream;
        stream = new PrintStream(new FileOutputStream("index.html"));

        // Create hello opening <html>, <header>, and <body> elements.
        stream.println("<html>");
        stream.println("<header/>");
        stream.println("<body>");

        // Enumerate hello uploaded blobs.
        for (ListBlobItem blobItem : container.listBlobs()) {
            // List each blob as an <img> element in hello HTML body.
            stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
        }

        stream.println("</body>");

        // Complete hello <html> element and close hello file.
        stream.println("</html>");
        stream.close();
    }
}
```

<span data-ttu-id="dbb2d-173">Přidání toouploading obrázek místní soubor tooAzure úložiště, hello příklad kódu vytvoří namedindex.html místního souboru, který lze otevřít v toosee váš prohlížeč nahrané image.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-173">In addition toouploading your local image file tooAzure storage, hello example code creates a local file namedindex.html, which you can open in your browser toosee your uploaded image.</span></span>

<span data-ttu-id="dbb2d-174">Protože kód hello obsahuje název účtu a klíč účtu, zajistěte, aby byl zdrojový kód zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-174">Because hello code contains your account name and account key, ensure that your source code is secure.</span></span>

## <a name="toodelete-a-container"></a><span data-ttu-id="dbb2d-175">toodelete kontejner</span><span class="sxs-lookup"><span data-stu-id="dbb2d-175">toodelete a container</span></span>
<span data-ttu-id="dbb2d-176">Vzhledem k tomu, že musíte platit za úložiště, může být vhodné toodelete **gettingstarted** kontejner po skončení experimentování se v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-176">Because you are charged for storage, you may want toodelete the **gettingstarted** container after you are done experimenting with this example.</span></span> <span data-ttu-id="dbb2d-177">toodelete kontejner, použijte hello **CloudBlobContainer.delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-177">toodelete a container, use hello **CloudBlobContainer.delete** method.</span></span>

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

<span data-ttu-id="dbb2d-178">toocall hello **CloudBlobContainer.delete** metoda, hello proces inicializace **CloudStorageAccount**, **ClodBlobClient**, a  **CloudBlobContainer** objektů je hello stejná jak je uvedeno pro **createIfNotExist** metoda.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-178">toocall hello **CloudBlobContainer.delete** method, hello process of initializing **CloudStorageAccount**, **ClodBlobClient**, and **CloudBlobContainer** objects is hello same as shown for the **createIfNotExist** method.</span></span> <span data-ttu-id="dbb2d-179">Hello následuje kompletní příklad, odstraní hello kontejner s názvem **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-179">hello following is a complete example that deletes hello container named **gettingstarted**.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

public class DeleteContainer {

    public static final String storageConnectionString =
            "DefaultEndpointsProtocol=http;" +
                "AccountName=your_account_name;" +
                "AccountKey=your_account_key";

    public static void main(String[] args)
    {
        try
        {
            CloudStorageAccount account;
            CloudBlobClient serviceClient;
            CloudBlobContainer container;

            account = CloudStorageAccount.parse(storageConnectionString);
            serviceClient = account.createCloudBlobClient();
            // Container name must be lower case.
            container = serviceClient.getContainerReference("gettingstarted");
            container.delete();

            System.out.println("Container deleted.");

        }
        catch (StorageException storageException)
        {
            System.out.print("StorageException encountered: ");
            System.out.println(storageException.getMessage());
            System.exit(-1);
        }
        catch (Exception e)
        {
            System.out.print("Exception encountered: ");
            System.out.println(e.getMessage());
            System.exit(-1);
        }
    }
}
```

<span data-ttu-id="dbb2d-180">Přehled jiné třídy úložiště objektů blob a metody, najdete v tématu [jak toouse úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="dbb2d-180">For an overview of other blob storage classes and methods, see [How toouse Blob storage from Java](storage-java-how-to-use-blob-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dbb2d-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dbb2d-181">Next steps</span></span>
<span data-ttu-id="dbb2d-182">Použijte tyto odkazy toolearn Další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="dbb2d-182">Follow these links toolearn more about more complex storage tasks.</span></span>

* [<span data-ttu-id="dbb2d-183">Úložiště Azure SDK pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="dbb2d-183">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="dbb2d-184">Odkaz na sadu SDK klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="dbb2d-184">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="dbb2d-185">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="dbb2d-185">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="dbb2d-186">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="dbb2d-186">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)

