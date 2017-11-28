<span data-ttu-id="732e0-101">Pokud budete mít adresu URL sdílený přístupový podpis (SAS), který uděluje přístup k prostředkům v účtu úložiště, můžete použít SAS v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="732e0-101">If you possess a shared access signature (SAS) URL that grants you access to resources in a storage account, you can use the SAS in a connection string.</span></span> <span data-ttu-id="732e0-102">Protože SAS obsahuje informace potřebné k ověření požadavku, připojovací řetězec s SAS poskytuje protokol, koncový bod služby a potřebné pověření pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="732e0-102">Because the SAS contains the information required to authenticate the request, a connection string with a SAS provides the protocol, the service endpoint, and the necessary credentials to access the resource.</span></span>

<span data-ttu-id="732e0-103">Pokud chcete vytvořit připojovací řetězec, který zahrnuje sdílený přístupový podpis, zadejte řetězec ve formátu:</span><span class="sxs-lookup"><span data-stu-id="732e0-103">To create a connection string that includes a shared access signature, specify the string in the following format:</span></span>

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

<span data-ttu-id="732e0-104">Každý koncový bod služby je volitelný, i když připojovací řetězec musí obsahovat alespoň jeden.</span><span class="sxs-lookup"><span data-stu-id="732e0-104">Each service endpoint is optional, although the connection string must contain at least one.</span></span>

> [!NOTE]
> <span data-ttu-id="732e0-105">Jako osvědčený postup se doporučuje HTTPS pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="732e0-105">Using HTTPS with a SAS is recommended as a best practice.</span></span>
>
> <span data-ttu-id="732e0-106">Pokud zadáte SAS v připojovacím řetězci v konfiguračním souboru, můžete kódovat speciální znaky v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="732e0-106">If you are specifying a SAS in a connection string in a configuration file, you may need to encode special characters in the URL.</span></span>
>
>

### <a name="service-sas-example"></a><span data-ttu-id="732e0-107">Příklad SAS služby</span><span class="sxs-lookup"><span data-stu-id="732e0-107">Service SAS example</span></span>
<span data-ttu-id="732e0-108">Tady je příklad připojovacího řetězce, který obsahuje službu SAS pro úložiště objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="732e0-108">Here's an example of a connection string that includes a service SAS for Blob storage:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

<span data-ttu-id="732e0-109">A tady je příklad stejný připojovací řetězec s kódováním speciálních znaků:</span><span class="sxs-lookup"><span data-stu-id="732e0-109">And here's an example of the same connection string with encoding of special characters:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a><span data-ttu-id="732e0-110">Příklad SAS účtu</span><span class="sxs-lookup"><span data-stu-id="732e0-110">Account SAS example</span></span>
<span data-ttu-id="732e0-111">Tady je příklad připojovacího řetězce, který zahrnuje SAS účtu pro úložiště Blob a souboru.</span><span class="sxs-lookup"><span data-stu-id="732e0-111">Here's an example of a connection string that includes an account SAS for Blob and File storage.</span></span> <span data-ttu-id="732e0-112">Všimněte si, že jsou zadané koncové body pro obě služby:</span><span class="sxs-lookup"><span data-stu-id="732e0-112">Note that endpoints for both services are specified:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

<span data-ttu-id="732e0-113">A tady je příklad stejný připojovací řetězec s kódováním adresy URL:</span><span class="sxs-lookup"><span data-stu-id="732e0-113">And here's an example of the same connection string with URL encoding:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

