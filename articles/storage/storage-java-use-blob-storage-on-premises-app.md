---
title: "Místní aplikace pomocí úložiště objektů blob (Java) | Microsoft Docs"
description: "Naučte se vytvářet konzolovou aplikaci, která odesílá bitovou kopii do Azure a potom zobrazí obrázek v prohlížeči. Ukázky kódu v jazyce Java."
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
ms.openlocfilehash: a172b881fa38a69f4510df94f5797b7a56940c52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="on-premises-application-with-blob-storage"></a><span data-ttu-id="31155-104">Místní aplikace pomocí úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="31155-104">On-premises application with blob storage</span></span>
## <a name="overview"></a><span data-ttu-id="31155-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="31155-105">Overview</span></span>
<span data-ttu-id="31155-106">Následující příklad ukazuje, jak můžete použít úložiště Azure pro uložení bitové kopie v Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-106">The following example shows you how you can use Azure storage to store images in Azure.</span></span> <span data-ttu-id="31155-107">Kód v tomto článku je konzolovou aplikaci, která odesílá bitovou kopii do Azure a poté vytvoří souboru HTML, který zobrazí obrázek v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="31155-107">The code in this article is for a console application that uploads an image to Azure, and then creates an HTML file that displays the image in your browser.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31155-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31155-108">Prerequisites</span></span>
* <span data-ttu-id="31155-109">Java Developer Kit (JDK), verze 1.6 nebo novější, je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="31155-109">A Java Developer Kit (JDK), version 1.6 or later, is installed.</span></span>
* <span data-ttu-id="31155-110">Je nainstalována sada Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="31155-110">The Azure SDK is installed.</span></span>
* <span data-ttu-id="31155-111">JAR pro knihovny Azure Libraries for Java a všechny příslušné závislostí JAR jsou nainstalované a jsou v cestě sestavení používané vaší kompilátor Javy.</span><span class="sxs-lookup"><span data-stu-id="31155-111">The JAR for the Azure Libraries for Java, and any applicable dependency JARs, are installed and are in the build path used by your Java compiler.</span></span> <span data-ttu-id="31155-112">Informace o instalaci knihovny Azure Libraries for Java najdete v tématu [stáhněte si sadu Azure SDK pro jazyk Java](../java-download-azure-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="31155-112">For information about installing the Azure Libraries for Java, see [Download the Azure SDK for Java](../java-download-azure-sdk.md).</span></span>
* <span data-ttu-id="31155-113">Má nastaven účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-113">An Azure storage account has been set up.</span></span> <span data-ttu-id="31155-114">Název účtu a klíč účtu pro účet úložiště se použije kód v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="31155-114">The account name and account key for the storage account will be used by the code in this article.</span></span> <span data-ttu-id="31155-115">V tématu [postup vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) informace o vytvoření účtu úložiště, a [zobrazení a zkopírování přístupových klíčů úložiště](storage-create-storage-account.md#view-and-copy-storage-access-keys) informace o načtení klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="31155-115">See [How to Create a Storage Account](storage-create-storage-account.md#create-a-storage-account) for information about creating a storage account, and [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) for information about retrieving the account key.</span></span>
* <span data-ttu-id="31155-116">Vytvoříte místní bitové kopie souboru s názvem uložené na jednotce c: cesta\\myimages\\image1.jpg.</span><span class="sxs-lookup"><span data-stu-id="31155-116">You have created a local image file named stored at the path c:\\myimages\\image1.jpg.</span></span> <span data-ttu-id="31155-117">Můžete taky upravit **FileInputStream** konstruktor v příkladu je název a cesta k souboru jinou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="31155-117">Alternatively, modify the **FileInputStream** constructor in the example to use a different image path and file name.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a><span data-ttu-id="31155-118">Používání úložiště objektů blob v Azure pro nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="31155-118">To use Azure blob storage to upload a file</span></span>
<span data-ttu-id="31155-119">Podrobný postup je uveden zde.</span><span class="sxs-lookup"><span data-stu-id="31155-119">A step-by-step procedure is presented here.</span></span> <span data-ttu-id="31155-120">Pokud chcete přeskočit celý kód se zobrazí později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="31155-120">If you'd like to skip ahead, the entire code is presented later in this article.</span></span>

<span data-ttu-id="31155-121">Začněte tím, že importy pro třídy úložiště Azure základní třídy klienta objektů blob v Azure, třídy Java vstupně-výstupní operace, včetně kódu a **URISyntaxException** třídy.</span><span class="sxs-lookup"><span data-stu-id="31155-121">Begin the code by including imports for the Azure core storage classes, the Azure blob client classes, the Java IO classes, and the **URISyntaxException** class.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

<span data-ttu-id="31155-122">Deklarace třídy s názvem **StorageSample**a zahrnovat otevřete závorky **{**.</span><span class="sxs-lookup"><span data-stu-id="31155-122">Declare a class named **StorageSample**, and include the open bracket, **{**.</span></span>

```java
public class StorageSample {
```

<span data-ttu-id="31155-123">V rámci **StorageSample** třídy, deklarovat proměnnou string, který bude obsahovat výchozí koncový bod protokol, název účtu úložiště a přístupový klíč k úložišti, jak je uvedeno v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-123">Within the **StorageSample** class, declare a string variable that will contain the default endpoint protocol, your storage account name, and your storage access key, as specified in your Azure storage account.</span></span> <span data-ttu-id="31155-124">Nahraďte zástupný symbol hodnoty **vaše\_účet\_název** a **vaše\_účet\_klíč** vlastní účet názvu a klíče účtu, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="31155-124">Replace the placeholder values **your\_account\_name** and **your\_account\_key** with your own account name and account key, respectively.</span></span>

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

<span data-ttu-id="31155-125">Přidat do vaší deklaraci pro **hlavní**, zahrnout **zkuste** blokovat a zahrnovat potřeby otevřete hranatých závorkách **{**.</span><span class="sxs-lookup"><span data-stu-id="31155-125">Add in your declaration for **main**, include a **try** block, and include the necessary open brackets, **{**.</span></span>

```java
    public static void main(String[] args)
    {
        try
        {
```

<span data-ttu-id="31155-126">Deklarujte proměnné následujícího typu (popis je pro jejich použití v tomto příkladu):</span><span class="sxs-lookup"><span data-stu-id="31155-126">Declare variables of the following type (the descriptions are for how they are used in this example):</span></span>

* <span data-ttu-id="31155-127">**CloudStorageAccount**: slouží k inicializaci objektu účtu se název účtu úložiště Azure a klíč a pro vytvoření klientského objektu blob.</span><span class="sxs-lookup"><span data-stu-id="31155-127">**CloudStorageAccount**: Used to initialize the account object with your Azure storage account name and key, and to create the blob client object.</span></span>
* <span data-ttu-id="31155-128">**CloudBlobClient**: slouží k přístupu k služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="31155-128">**CloudBlobClient**: Used to access the blob service.</span></span>
* <span data-ttu-id="31155-129">**CloudBlobContainer**: slouží k vytvoření kontejneru objektů blob, seznam objektů BLOB v kontejneru a odstranit kontejner.</span><span class="sxs-lookup"><span data-stu-id="31155-129">**CloudBlobContainer**: Used to create a blob container, list the blobs in the container, and delete the container.</span></span>
* <span data-ttu-id="31155-130">**CloudBlockBlob**: umožňuje nahrát obrázek místní soubor do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="31155-130">**CloudBlockBlob**: Used to upload a local image file to the container.</span></span>

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

<span data-ttu-id="31155-131">Přiřazení hodnoty k **účet** proměnné.</span><span class="sxs-lookup"><span data-stu-id="31155-131">Assign a value to the **account** variable.</span></span>

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

<span data-ttu-id="31155-132">Přiřazení hodnoty k **serviceClient** proměnné.</span><span class="sxs-lookup"><span data-stu-id="31155-132">Assign a value to the **serviceClient** variable.</span></span>

```java
serviceClient = account.createCloudBlobClient();
```

<span data-ttu-id="31155-133">Přiřazení hodnoty k **kontejneru** proměnné.</span><span class="sxs-lookup"><span data-stu-id="31155-133">Assign a value to the **container** variable.</span></span> <span data-ttu-id="31155-134">Budete se nám získat odkaz na kontejner s názvem **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="31155-134">We'll get a reference to a container named **gettingstarted**.</span></span>

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

<span data-ttu-id="31155-135">Vytvoření kontejneru.</span><span class="sxs-lookup"><span data-stu-id="31155-135">Create the container.</span></span> <span data-ttu-id="31155-136">Tato metoda vytvoří kontejner, pokud neexistuje (a vrátit **true**).</span><span class="sxs-lookup"><span data-stu-id="31155-136">This method will create the container if it doesn't exist (and return **true**).</span></span> <span data-ttu-id="31155-137">Pokud kontejner neexistuje, vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="31155-137">If the container does exist, it will return **false**.</span></span> <span data-ttu-id="31155-138">Alternativu k **createIfNotExists** je **vytvořit** (která vrátí chybu, pokud kontejner již existuje).</span><span class="sxs-lookup"><span data-stu-id="31155-138">An alternative to **createIfNotExists** is the **create** method (which will return an error if the container already exists).</span></span>

```java
container.createIfNotExists();
```

<span data-ttu-id="31155-139">Pro kontejner nastaven anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="31155-139">Set anonymous access for the container.</span></span>

```java
// Set anonymous access on the container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

<span data-ttu-id="31155-140">Získáte odkaz na objekt blob bloku, která bude představovat objektů blob v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-140">Get a reference to the block blob, which will represent the blob in Azure storage.</span></span>

```java
blob = container.getBlockBlobReference("image1.jpg");
```

<span data-ttu-id="31155-141">Použití **souboru** konstruktor získat odkaz na soubor místně vytvořené, který bude nahrávat.</span><span class="sxs-lookup"><span data-stu-id="31155-141">Use the **File** constructor to get a reference to the locally created file that you will upload.</span></span> <span data-ttu-id="31155-142">Zkontrolujte, zda že jste vytvořili tento soubor před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="31155-142">Ensure you have created this file before running the code.</span></span>

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

<span data-ttu-id="31155-143">Nahrát místního souboru prostřednictvím volání **CloudBlockBlob.upload** metoda.</span><span class="sxs-lookup"><span data-stu-id="31155-143">Upload the local file through a call to the **CloudBlockBlob.upload** method.</span></span> <span data-ttu-id="31155-144">První parametr **CloudBlockBlob.upload** je metoda **FileInputStream** objekt, který představuje místní soubor, který bude nahrán do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-144">The first parameter to the **CloudBlockBlob.upload** method is a **FileInputStream** object that represents the local file that will be uploaded to Azure storage.</span></span> <span data-ttu-id="31155-145">Druhý parametr je velikost v bajtech souboru.</span><span class="sxs-lookup"><span data-stu-id="31155-145">The second parameter is the size, in bytes, of the file.</span></span>

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

<span data-ttu-id="31155-146">Volání pomocné funkce s názvem **MakeHTMLPage**, aby základní stránky HTML, který obsahuje  **&lt;image&gt;**  element se zdrojem nastaven na objekt blob, který je teď ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-146">Call a helper function named **MakeHTMLPage**, to make a basic HTML page that contains an **&lt;image&gt;** element with the source set to the blob that is now in your Azure storage account.</span></span> <span data-ttu-id="31155-147">Kód pro **MakeHTMLPage** bude probírat později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="31155-147">The code for **MakeHTMLPage** will be discussed later in this article.</span></span>

```java
MakeHTMLPage(container);
```

<span data-ttu-id="31155-148">Vytiskněte stavové zprávy a informace o vytvoření stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="31155-148">Print out a status message and information about the created HTML page.</span></span>

```java
System.out.println("Processing complete.");
System.out.println("Open index.html to see the images stored in your storage account.");
```

<span data-ttu-id="31155-149">Zavřete **zkuste** bloku vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="31155-149">Close the **try** block by inserting a close bracket: **}**</span></span>

<span data-ttu-id="31155-150">Zpracování následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="31155-150">Handle the following exceptions:</span></span>

* <span data-ttu-id="31155-151">**FileNotFoundException**: může být vyvolána **FileInputStream** nebo **FileOutputStream** konstruktory.</span><span class="sxs-lookup"><span data-stu-id="31155-151">**FileNotFoundException**: Can be thrown by the **FileInputStream** or **FileOutputStream** constructors.</span></span>
* <span data-ttu-id="31155-152">**StorageException**: může být vyvolána knihovny klienta Azure storage.</span><span class="sxs-lookup"><span data-stu-id="31155-152">**StorageException**: Can be thrown by the Azure client storage library.</span></span>
* <span data-ttu-id="31155-153">**URISyntaxException**: může být vyvolána **ListBlobItem.getUri** metoda.</span><span class="sxs-lookup"><span data-stu-id="31155-153">**URISyntaxException**: Can be thrown by the **ListBlobItem.getUri** method.</span></span>
* <span data-ttu-id="31155-154">**Výjimka**: Obecné výjimek.</span><span class="sxs-lookup"><span data-stu-id="31155-154">**Exception**: Generic exception handling.</span></span>

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

<span data-ttu-id="31155-155">Zavřete **hlavní** vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="31155-155">Close **main** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="31155-156">Vytvořit metodu s názvem **MakeHTMLPage** pro vytvoření základní stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="31155-156">Create a method named **MakeHTMLPage** to create a basic HTML page.</span></span> <span data-ttu-id="31155-157">Tato metoda má parametr typu **CloudBlobContainer**, který se použije k iteraci v rámci seznamu nahrané objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="31155-157">This method has a parameter of type **CloudBlobContainer**, which will be used to iterate through the list of uploaded blobs.</span></span> <span data-ttu-id="31155-158">Tato metoda vyvolá výjimku výjimky typu **FileNotFoundException**, který může být vyvolána **FileOutputStream** konstruktoru, a **URISyntaxException**, který může být vyvolána **ListBlobItem.getUri** metoda.</span><span class="sxs-lookup"><span data-stu-id="31155-158">This method will throw exceptions of type **FileNotFoundException**, which can be thrown by the **FileOutputStream** constructor, and **URISyntaxException**, which can be thrown by the **ListBlobItem.getUri** method.</span></span> <span data-ttu-id="31155-159">Zahrnout levá hranatá závorka, **{**.</span><span class="sxs-lookup"><span data-stu-id="31155-159">Include the opening bracket, **{**.</span></span>

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

<span data-ttu-id="31155-160">Vytvořte místní soubor s názvem **index.html**.</span><span class="sxs-lookup"><span data-stu-id="31155-160">Create a local file named **index.html**.</span></span>

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

<span data-ttu-id="31155-161">Zapisovat do místního souboru, přidání do  **&lt;html&gt;**,  **&lt;záhlaví&gt;**, a  **&lt;textu&gt;**  elementy.</span><span class="sxs-lookup"><span data-stu-id="31155-161">Write to the local file, adding in the **&lt;html&gt;**, **&lt;header&gt;**, and **&lt;body&gt;** elements.</span></span>

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

<span data-ttu-id="31155-162">Iteraci v rámci seznamu nahrané objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="31155-162">Iterate through the list of uploaded blobs.</span></span> <span data-ttu-id="31155-163">Pro každý objekt blob, vytvořte na stránce HTML  **&lt;img&gt;**  element, který má jeho **src** atribut odeslaný na identifikátor URI objektu blob, protože existuje ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="31155-163">For each blob, in the HTML page create an **&lt;img&gt;** element that has its **src** attribute sent to the URI of the blob as it exists in your Azure storage account.</span></span>
<span data-ttu-id="31155-164">I když jste přidali pouze jeden obrázek v této ukázce, pokud jste přidali více, by tento kód iteraci všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="31155-164">Although you added only one image in this sample, if you added more, this code would iterate all of them.</span></span>

<span data-ttu-id="31155-165">Pro zjednodušení tento příklad předpokládá, že každý nahraném objektu blob je bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="31155-165">For simplicity, this example assumes each uploaded blob is an image.</span></span> <span data-ttu-id="31155-166">Pokud jste aktualizovali objektů BLOB než bitové kopie nebo objekty BLOB stránky místo objekty BLOB bloku, podle potřeby upravte kód.</span><span class="sxs-lookup"><span data-stu-id="31155-166">If you've updated blobs other than images, or page blobs instead of block blobs, adjust the code as needed.</span></span>

```java
// Enumerate the uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in the HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

<span data-ttu-id="31155-167">Zavřít  **&lt;textu&gt;**  elementu a  **&lt;html&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="31155-167">Close the **&lt;body&gt;** element and the **&lt;html&gt;** element.</span></span>

```java
stream.println("</body>");
stream.println("</html>");
```

<span data-ttu-id="31155-168">Zavřete místní soubor.</span><span class="sxs-lookup"><span data-stu-id="31155-168">Close the local file.</span></span>

```java
stream.close();
```

<span data-ttu-id="31155-169">Zavřete **MakeHTMLPage** vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="31155-169">Close **MakeHTMLPage** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="31155-170">Zavřete **StorageSample** vložením znak pravé hranaté závorky: **}**</span><span class="sxs-lookup"><span data-stu-id="31155-170">Close **StorageSample** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="31155-171">Následuje kompletní kód v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="31155-171">The following is the complete code for this example.</span></span> <span data-ttu-id="31155-172">Nezapomeňte upravit zástupné hodnoty **vaše\_účet\_název** a **vaše\_účet\_klíč** použít název účtu a klíč účtu, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="31155-172">Remember to modify the placeholder values **your\_account\_name** and **your\_account\_key** to use your account name and account key, respectively.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;

// Create an image, c:\myimages\image1.jpg, prior to running this sample.
// Alternatively, change the value used by the FileInputStream constructor
// to use a different image path and file that you have already created.
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

            // Set anonymous access on the container.
            BlobContainerPermissions containerPermissions;
            containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // Upload an image file.
            blob = container.getBlockBlobReference("image1.jpg");

            File fileReference = new File("c:\\myimages\\image1.jpg");
            blob.upload(new FileInputStream(fileReference), fileReference.length());

            // At this point the image is uploaded.
            // Next, create an HTML page that lists all of the uploaded images.
            MakeHTMLPage(container);

            System.out.println("Processing complete.");
            System.out.println("Open index.html to see the images stored in your storage account.");

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

    // Create an HTML page that can be used to display the uploaded images.
    // This example assumes all of the blobs are for images.
    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {
        PrintStream stream;
        stream = new PrintStream(new FileOutputStream("index.html"));

        // Create the opening <html>, <header>, and <body> elements.
        stream.println("<html>");
        stream.println("<header/>");
        stream.println("<body>");

        // Enumerate the uploaded blobs.
        for (ListBlobItem blobItem : container.listBlobs()) {
            // List each blob as an <img> element in the HTML body.
            stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
        }

        stream.println("</body>");

        // Complete the <html> element and close the file.
        stream.println("</html>");
        stream.close();
    }
}
```

<span data-ttu-id="31155-173">Kromě obrázek místní soubor se nahrává do úložiště Azure, příklad kódu vytvoří namedindex.html místního souboru, které lze otevřít v prohlížeči zobrazit nahrané image.</span><span class="sxs-lookup"><span data-stu-id="31155-173">In addition to uploading your local image file to Azure storage, the example code creates a local file namedindex.html, which you can open in your browser to see your uploaded image.</span></span>

<span data-ttu-id="31155-174">Protože kód obsahuje název účtu a klíč účtu, ujistěte se, že zdrojový kód je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="31155-174">Because the code contains your account name and account key, ensure that your source code is secure.</span></span>

## <a name="to-delete-a-container"></a><span data-ttu-id="31155-175">Chcete-li odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="31155-175">To delete a container</span></span>
<span data-ttu-id="31155-176">Vzhledem k tomu, že musíte platit za úložiště, můžete chtít odstranit **gettingstarted** kontejner po skončení experimentování se v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="31155-176">Because you are charged for storage, you may want to delete the **gettingstarted** container after you are done experimenting with this example.</span></span> <span data-ttu-id="31155-177">Pokud chcete odstranit kontejner, použijte **CloudBlobContainer.delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="31155-177">To delete a container, use the **CloudBlobContainer.delete** method.</span></span>

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

<span data-ttu-id="31155-178">K volání **CloudBlobContainer.delete** metoda, proces inicializace **CloudStorageAccount**, **ClodBlobClient**, a **CloudBlobContainer** objektů je stejný, jak je uvedeno pro **createIfNotExist** metoda.</span><span class="sxs-lookup"><span data-stu-id="31155-178">To call the **CloudBlobContainer.delete** method, the process of initializing **CloudStorageAccount**, **ClodBlobClient**, and **CloudBlobContainer** objects is the same as shown for the **createIfNotExist** method.</span></span> <span data-ttu-id="31155-179">Tady je kompletní příklad, který odstraní kontejner s názvem **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="31155-179">The following is a complete example that deletes the container named **gettingstarted**.</span></span>

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

<span data-ttu-id="31155-180">Přehled jiné třídy úložiště objektů blob a metody, najdete v tématu [používání úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="31155-180">For an overview of other blob storage classes and methods, see [How to use Blob storage from Java](storage-java-how-to-use-blob-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="31155-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31155-181">Next steps</span></span>
<span data-ttu-id="31155-182">Další informace o složitějších úlohách úložiště na následujících odkazech.</span><span class="sxs-lookup"><span data-stu-id="31155-182">Follow these links to learn more about more complex storage tasks.</span></span>

* [<span data-ttu-id="31155-183">Úložiště Azure SDK pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="31155-183">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="31155-184">Odkaz na sadu SDK klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="31155-184">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="31155-185">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="31155-185">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="31155-186">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="31155-186">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)

