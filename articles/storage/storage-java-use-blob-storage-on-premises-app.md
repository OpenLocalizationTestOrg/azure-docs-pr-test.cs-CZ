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
# <a name="on-premises-application-with-blob-storage"></a>Místní aplikace pomocí úložiště objektů blob
## <a name="overview"></a>Přehled
Následující příklad ukazuje, jak můžete použít úložiště Azure pro uložení bitové kopie v Azure. Kód v tomto článku je konzolovou aplikaci, která odesílá bitovou kopii do Azure a poté vytvoří souboru HTML, který zobrazí obrázek v prohlížeči.

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), verze 1.6 nebo novější, je nainstalována.
* Je nainstalována sada Azure SDK.
* JAR pro knihovny Azure Libraries for Java a všechny příslušné závislostí JAR jsou nainstalované a jsou v cestě sestavení používané vaší kompilátor Javy. Informace o instalaci knihovny Azure Libraries for Java najdete v tématu [stáhněte si sadu Azure SDK pro jazyk Java](../java-download-azure-sdk.md).
* Má nastaven účet úložiště Azure. Název účtu a klíč účtu pro účet úložiště se použije kód v tomto článku. V tématu [postup vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) informace o vytvoření účtu úložiště, a [zobrazení a zkopírování přístupových klíčů úložiště](storage-create-storage-account.md#view-and-copy-storage-access-keys) informace o načtení klíč účtu.
* Vytvoříte místní bitové kopie souboru s názvem uložené na jednotce c: cesta\\myimages\\image1.jpg. Můžete taky upravit **FileInputStream** konstruktor v příkladu je název a cesta k souboru jinou bitovou kopii.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Používání úložiště objektů blob v Azure pro nahrání souboru
Podrobný postup je uveden zde. Pokud chcete přeskočit celý kód se zobrazí později v tomto článku.

Začněte tím, že importy pro třídy úložiště Azure základní třídy klienta objektů blob v Azure, třídy Java vstupně-výstupní operace, včetně kódu a **URISyntaxException** třídy.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

Deklarace třídy s názvem **StorageSample**a zahrnovat otevřete závorky **{**.

```java
public class StorageSample {
```

V rámci **StorageSample** třídy, deklarovat proměnnou string, který bude obsahovat výchozí koncový bod protokol, název účtu úložiště a přístupový klíč k úložišti, jak je uvedeno v účtu úložiště Azure. Nahraďte zástupný symbol hodnoty **vaše\_účet\_název** a **vaše\_účet\_klíč** vlastní účet názvu a klíče účtu, v uvedeném pořadí.

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

Přidat do vaší deklaraci pro **hlavní**, zahrnout **zkuste** blokovat a zahrnovat potřeby otevřete hranatých závorkách **{**.

```java
    public static void main(String[] args)
    {
        try
        {
```

Deklarujte proměnné následujícího typu (popis je pro jejich použití v tomto příkladu):

* **CloudStorageAccount**: slouží k inicializaci objektu účtu se název účtu úložiště Azure a klíč a pro vytvoření klientského objektu blob.
* **CloudBlobClient**: slouží k přístupu k služby objektů blob.
* **CloudBlobContainer**: slouží k vytvoření kontejneru objektů blob, seznam objektů BLOB v kontejneru a odstranit kontejner.
* **CloudBlockBlob**: umožňuje nahrát obrázek místní soubor do kontejneru.

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

Přiřazení hodnoty k **účet** proměnné.

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

Přiřazení hodnoty k **serviceClient** proměnné.

```java
serviceClient = account.createCloudBlobClient();
```

Přiřazení hodnoty k **kontejneru** proměnné. Budete se nám získat odkaz na kontejner s názvem **gettingstarted**.

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

Vytvoření kontejneru. Tato metoda vytvoří kontejner, pokud neexistuje (a vrátit **true**). Pokud kontejner neexistuje, vrátí **false**. Alternativu k **createIfNotExists** je **vytvořit** (která vrátí chybu, pokud kontejner již existuje).

```java
container.createIfNotExists();
```

Pro kontejner nastaven anonymní přístup.

```java
// Set anonymous access on the container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

Získáte odkaz na objekt blob bloku, která bude představovat objektů blob v úložišti Azure.

```java
blob = container.getBlockBlobReference("image1.jpg");
```

Použití **souboru** konstruktor získat odkaz na soubor místně vytvořené, který bude nahrávat. Zkontrolujte, zda že jste vytvořili tento soubor před spuštěním kódu.

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

Nahrát místního souboru prostřednictvím volání **CloudBlockBlob.upload** metoda. První parametr **CloudBlockBlob.upload** je metoda **FileInputStream** objekt, který představuje místní soubor, který bude nahrán do úložiště Azure. Druhý parametr je velikost v bajtech souboru.

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

Volání pomocné funkce s názvem **MakeHTMLPage**, aby základní stránky HTML, který obsahuje  **&lt;image&gt;**  element se zdrojem nastaven na objekt blob, který je teď ve vašem účtu úložiště Azure. Kód pro **MakeHTMLPage** bude probírat později v tomto článku.

```java
MakeHTMLPage(container);
```

Vytiskněte stavové zprávy a informace o vytvoření stránky HTML.

```java
System.out.println("Processing complete.");
System.out.println("Open index.html to see the images stored in your storage account.");
```

Zavřete **zkuste** bloku vložením znak pravé hranaté závorky: **}**

Zpracování následující výjimky:

* **FileNotFoundException**: může být vyvolána **FileInputStream** nebo **FileOutputStream** konstruktory.
* **StorageException**: může být vyvolána knihovny klienta Azure storage.
* **URISyntaxException**: může být vyvolána **ListBlobItem.getUri** metoda.
* **Výjimka**: Obecné výjimek.

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

Zavřete **hlavní** vložením znak pravé hranaté závorky: **}**

Vytvořit metodu s názvem **MakeHTMLPage** pro vytvoření základní stránky HTML. Tato metoda má parametr typu **CloudBlobContainer**, který se použije k iteraci v rámci seznamu nahrané objekty BLOB. Tato metoda vyvolá výjimku výjimky typu **FileNotFoundException**, který může být vyvolána **FileOutputStream** konstruktoru, a **URISyntaxException**, který může být vyvolána **ListBlobItem.getUri** metoda. Zahrnout levá hranatá závorka, **{**.

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

Vytvořte místní soubor s názvem **index.html**.

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

Zapisovat do místního souboru, přidání do  **&lt;html&gt;**,  **&lt;záhlaví&gt;**, a  **&lt;textu&gt;**  elementy.

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

Iteraci v rámci seznamu nahrané objekty BLOB. Pro každý objekt blob, vytvořte na stránce HTML  **&lt;img&gt;**  element, který má jeho **src** atribut odeslaný na identifikátor URI objektu blob, protože existuje ve vašem účtu úložiště Azure.
I když jste přidali pouze jeden obrázek v této ukázce, pokud jste přidali více, by tento kód iteraci všechny z nich.

Pro zjednodušení tento příklad předpokládá, že každý nahraném objektu blob je bitovou kopii. Pokud jste aktualizovali objektů BLOB než bitové kopie nebo objekty BLOB stránky místo objekty BLOB bloku, podle potřeby upravte kód.

```java
// Enumerate the uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in the HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

Zavřít  **&lt;textu&gt;**  elementu a  **&lt;html&gt;**  element.

```java
stream.println("</body>");
stream.println("</html>");
```

Zavřete místní soubor.

```java
stream.close();
```

Zavřete **MakeHTMLPage** vložením znak pravé hranaté závorky: **}**

Zavřete **StorageSample** vložením znak pravé hranaté závorky: **}**

Následuje kompletní kód v tomto příkladu. Nezapomeňte upravit zástupné hodnoty **vaše\_účet\_název** a **vaše\_účet\_klíč** použít název účtu a klíč účtu, v uvedeném pořadí.

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

Kromě obrázek místní soubor se nahrává do úložiště Azure, příklad kódu vytvoří namedindex.html místního souboru, které lze otevřít v prohlížeči zobrazit nahrané image.

Protože kód obsahuje název účtu a klíč účtu, ujistěte se, že zdrojový kód je bezpečné.

## <a name="to-delete-a-container"></a>Chcete-li odstranit kontejner
Vzhledem k tomu, že musíte platit za úložiště, můžete chtít odstranit **gettingstarted** kontejner po skončení experimentování se v tomto příkladu. Pokud chcete odstranit kontejner, použijte **CloudBlobContainer.delete** metoda.

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

K volání **CloudBlobContainer.delete** metoda, proces inicializace **CloudStorageAccount**, **ClodBlobClient**, a **CloudBlobContainer** objektů je stejný, jak je uvedeno pro **createIfNotExist** metoda. Tady je kompletní příklad, který odstraní kontejner s názvem **gettingstarted**.

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

Přehled jiné třídy úložiště objektů blob a metody, najdete v tématu [používání úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Další kroky
Další informace o složitějších úlohách úložiště na následujících odkazech.

* [Úložiště Azure SDK pro jazyk Java](https://github.com/azure/azure-storage-java)
* [Odkaz na sadu SDK klienta úložiště Azure](http://dl.windowsazure.com/storage/javadoc/)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)

