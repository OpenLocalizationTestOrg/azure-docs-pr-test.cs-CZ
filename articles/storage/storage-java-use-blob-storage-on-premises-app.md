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
# <a name="on-premises-application-with-blob-storage"></a>Místní aplikace pomocí úložiště objektů blob
## <a name="overview"></a>Přehled
Hello následující příklad ukazuje, je použití úložiště Azure pro uložení bitové kopie v Azure. Hello kód v tomto článku je konzolovou aplikaci, která odešle tooAzure bitové kopie a poté vytvoří souboru HTML, který se zobrazí v prohlížeči obrázek hello.

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), verze 1.6 nebo novější, je nainstalována.
* je nainstalována Hello Azure SDK.
* Hello JAR pro hello knihovny Azure Libraries for Java a všechny příslušné závislostí JAR jsou nainstalované a jsou v cestě sestavení hello používá vaše kompilátor Javy. Informace o instalaci hello knihovny Azure Libraries for Java najdete v tématu [hello stažení sady Azure SDK pro jazyk Java](../java-download-azure-sdk.md).
* Má nastaven účet úložiště Azure. Hello název účtu a klíč účtu pro účet úložiště hello se použije hello kód v tomto článku. V tématu [jak tooCreate účet úložiště](storage-create-storage-account.md#create-a-storage-account) informace o vytvoření účtu úložiště, a [zobrazení a zkopírování přístupových klíčů úložiště](storage-create-storage-account.md#view-and-copy-storage-access-keys) informace o načtení hello klíč účtu.
* Vytvoříte místní bitové kopie souboru s názvem uložené v hello cesta c:\\myimages\\image1.jpg. Můžete taky upravit **FileInputStream** konstruktor v hello příklad toouse název a cesta k souboru jinou bitovou kopii.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="toouse-azure-blob-storage-tooupload-a-file"></a>tooupload úložiště objektů blob v Azure toouse soubor
Podrobný postup je uveden zde. Pokud chcete tooskip dopředu, se zobrazí celý kód hello později v tomto článku.

Začněte tím, že včetně importy pro třídy úložiště Azure základní hello, hello třídy klienta objektů blob v Azure, hello Java vstupně-výstupní operace třídy a hello hello kódu **URISyntaxException** třídy.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

Deklarace třídy s názvem **StorageSample**a zahrnovat hello závorka, **{**.

```java
public class StorageSample {
```

V rámci hello **StorageSample** třídy, deklarovat proměnnou string, který bude obsahovat hello výchozí koncový bod protokol, název účtu úložiště a přístupový klíč k úložišti, jak je uvedeno v účtu úložiště Azure. Nahraďte zástupný symbol hodnoty hello **vaše\_účet\_název** a **vaše\_účet\_klíč** s vlastní název účtu a klíč účtu v uvedeném pořadí.

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

Přidat do vaší deklaraci pro **hlavní**, zahrnout **zkuste** blokovat a zahrnovat hello potřeby otevřete závorky, **{**.

```java
    public static void main(String[] args)
    {
        try
        {
```

Deklarace proměnné hello následující typ (hello popisy jsou pro jejich použití v tomto příkladu):

* **CloudStorageAccount**: používané tooinitialize hello účet objekt s váš název účtu úložiště Azure a klíč a toocreate klientského objektu blob.
* **CloudBlobClient**: používá služby objektů blob tooaccess hello.
* **CloudBlobContainer**: používané toocreate kontejner objektů blob seznamu objektů BLOB v kontejneru hello a odstranění hello kontejneru.
* **CloudBlockBlob**: používané tooupload kontejner toothe obrázek místní soubor.

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

Přiřadit hodnotu toohello **účet** proměnné.

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

Přiřadit hodnotu toohello **serviceClient** proměnné.

```java
serviceClient = account.createCloudBlobClient();
```

Přiřadit hodnotu toohello **kontejneru** proměnné. Budete se nám získat odkaz na kontejner tooa s názvem **gettingstarted**.

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

Vytvoření kontejneru hello. Tato metoda vytvoří kontejner hello, pokud neexistuje (a vrátit **true**). Pokud hello kontejner neexistuje, vrátí **false**. Alternativu příliš**createIfNotExists** je hello **vytvořit** (která vrátí chybu, pokud kontejner hello již existuje).

```java
container.createIfNotExists();
```

Nastavit anonymní přístup pro kontejner hello.

```java
// Set anonymous access on hello container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

Získání referenční dokumentace toohello objekt blob bloku, která bude představovat hello objektů blob v úložišti Azure.

```java
blob = container.getBlockBlobReference("image1.jpg");
```

Použití hello **souboru** konstruktor tooget soubor toohello místně vytvořené odkaz, který bude nahrávat. Zkontrolujte, zda že jste vytvořili tento soubor před spuštěním kódu hello.

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

Nahrát hello místního souboru prostřednictvím volání toohello **CloudBlockBlob.upload** metoda. první parametr toohello Hello **CloudBlockBlob.upload** je metoda **FileInputStream** objektu, že představuje hello místního souboru, který bude nahrán tooAzure úložiště. druhý parametr Hello je hello velikost v bajtech, hello souboru.

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

Volání pomocné funkce s názvem **MakeHTMLPage**, toomake základní HTML stránky, která obsahuje  **&lt;image&gt;**  element s hello zdroje sady toohello blob, který je teď ve vašem Azure účet úložiště. Hello kód pro **MakeHTMLPage** bude probírat později v tomto článku.

```java
MakeHTMLPage(container);
```

Vytiskněte stavové zprávy a informace o hello vytvořit stránku HTML.

```java
System.out.println("Processing complete.");
System.out.println("Open index.html toosee hello images stored in your storage account.");
```

Zavřít hello **zkuste** bloku vložením znak pravé hranaté závorky: **}**

Popisovač hello následující výjimky:

* **FileNotFoundException**: může být vyvolána hello **FileInputStream** nebo **FileOutputStream** konstruktory.
* **StorageException**: může být vyvolána hello Azure Klientská knihovna pro úložiště.
* **URISyntaxException**: může být vyvolána hello **ListBlobItem.getUri** metoda.
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

Vytvořit metodu s názvem **MakeHTMLPage** toocreate základní HTML stránky. Tato metoda má parametr typu **CloudBlobContainer**, které budou použité tooiterate prostřednictvím hello seznam nahrané objekty BLOB. Tato metoda vyvolá výjimku výjimky typu **FileNotFoundException**, což může být vyvolána hello **FileOutputStream** konstruktoru, a **URISyntaxException**, což může být vyvolané hello **ListBlobItem.getUri** metoda. Zahrnout levá hranatá závorka, **{**.

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

Vytvořte místní soubor s názvem **index.html**.

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

Zápisu místního souboru toohello přidávání hello  **&lt;html&gt;**,  **&lt;záhlaví&gt;**, a  **&lt;textu&gt;**  elementy.

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

Iterace hello seznam nahrané objekty BLOB. Pro každý objekt blob stránky hello HTML vytvořit  **&lt;img&gt;**  element, který má jeho **src** atribut odeslaný hello identifikátor URI objektu hello blob, protože existuje ve vašem účtu úložiště Azure.
I když jste přidali pouze jeden obrázek v této ukázce, pokud jste přidali více, by tento kód iteraci všechny z nich.

Pro zjednodušení tento příklad předpokládá, že každý nahraném objektu blob je bitovou kopii. Pokud jste aktualizovali objektů BLOB než bitové kopie nebo objekty BLOB stránky místo objekty BLOB bloku, upravte kód hello podle potřeby.

```java
// Enumerate hello uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in hello HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

Zavřít hello  **&lt;textu&gt;**  elementu a hello  **&lt;html&gt;**  element.

```java
stream.println("</body>");
stream.println("</html>");
```

Zavřít hello místního souboru.

```java
stream.close();
```

Zavřete **MakeHTMLPage** vložením znak pravé hranaté závorky: **}**

Zavřete **StorageSample** vložením znak pravé hranaté závorky: **}**

Hello následuje hello kompletní kód v tomto příkladu. Mějte na paměti, toomodify hello zástupné hodnoty **vaše\_účet\_název** a **vaše\_účet\_klíč** toouse název účtu a účet v uvedeném pořadí klíče.

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

Přidání toouploading obrázek místní soubor tooAzure úložiště, hello příklad kódu vytvoří namedindex.html místního souboru, který lze otevřít v toosee váš prohlížeč nahrané image.

Protože kód hello obsahuje název účtu a klíč účtu, zajistěte, aby byl zdrojový kód zabezpečení.

## <a name="toodelete-a-container"></a>toodelete kontejner
Vzhledem k tomu, že musíte platit za úložiště, může být vhodné toodelete **gettingstarted** kontejner po skončení experimentování se v tomto příkladu. toodelete kontejner, použijte hello **CloudBlobContainer.delete** metoda.

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

toocall hello **CloudBlobContainer.delete** metoda, hello proces inicializace **CloudStorageAccount**, **ClodBlobClient**, a  **CloudBlobContainer** objektů je hello stejná jak je uvedeno pro **createIfNotExist** metoda. Hello následuje kompletní příklad, odstraní hello kontejner s názvem **gettingstarted**.

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

Přehled jiné třídy úložiště objektů blob a metody, najdete v tématu [jak toouse úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o složitějších úlohách úložiště.

* [Úložiště Azure SDK pro jazyk Java](https://github.com/azure/azure-storage-java)
* [Odkaz na sadu SDK klienta úložiště Azure](http://dl.windowsazure.com/storage/javadoc/)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)

