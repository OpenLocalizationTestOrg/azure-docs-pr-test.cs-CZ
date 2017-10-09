<span data-ttu-id="794a2-101">Pokud budete mít adresu URL sdílený přístupový podpis (SAS), která vám uděluje že přístup tooresources v účtu úložiště, můžete použít hello SAS v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="794a2-101">If you possess a shared access signature (SAS) URL that grants you access tooresources in a storage account, you can use hello SAS in a connection string.</span></span> <span data-ttu-id="794a2-102">Protože hello SAS obsahuje hello informace požadované tooauthenticate hello požadavku, připojovací řetězec s SAS poskytuje hello protokolu, koncový bod služby hello a hello potřebná pověření tooaccess hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="794a2-102">Because hello SAS contains hello information required tooauthenticate hello request, a connection string with a SAS provides hello protocol, hello service endpoint, and hello necessary credentials tooaccess hello resource.</span></span>

<span data-ttu-id="794a2-103">toocreate připojovací řetězec, který zahrnuje sdílený přístupový podpis, zadejte řetězec hello v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="794a2-103">toocreate a connection string that includes a shared access signature, specify hello string in hello following format:</span></span>

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

<span data-ttu-id="794a2-104">Každý koncový bod služby je volitelný, i když hello připojovací řetězec musí obsahovat alespoň jeden.</span><span class="sxs-lookup"><span data-stu-id="794a2-104">Each service endpoint is optional, although hello connection string must contain at least one.</span></span>

> [!NOTE]
> <span data-ttu-id="794a2-105">Jako osvědčený postup se doporučuje HTTPS pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="794a2-105">Using HTTPS with a SAS is recommended as a best practice.</span></span>
>
> <span data-ttu-id="794a2-106">Pokud zadáte SAS v připojovacím řetězci v konfiguračním souboru, musíte tooencode speciální znaky v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="794a2-106">If you are specifying a SAS in a connection string in a configuration file, you may need tooencode special characters in hello URL.</span></span>
>
>

### <a name="service-sas-example"></a><span data-ttu-id="794a2-107">Příklad SAS služby</span><span class="sxs-lookup"><span data-stu-id="794a2-107">Service SAS example</span></span>
<span data-ttu-id="794a2-108">Tady je příklad připojovacího řetězce, který obsahuje službu SAS pro úložiště objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="794a2-108">Here's an example of a connection string that includes a service SAS for Blob storage:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

<span data-ttu-id="794a2-109">A tady je příklad hello stejný připojovací řetězec s kódováním speciálních znaků:</span><span class="sxs-lookup"><span data-stu-id="794a2-109">And here's an example of hello same connection string with encoding of special characters:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a><span data-ttu-id="794a2-110">Příklad SAS účtu</span><span class="sxs-lookup"><span data-stu-id="794a2-110">Account SAS example</span></span>
<span data-ttu-id="794a2-111">Tady je příklad připojovacího řetězce, který zahrnuje SAS účtu pro úložiště Blob a souboru.</span><span class="sxs-lookup"><span data-stu-id="794a2-111">Here's an example of a connection string that includes an account SAS for Blob and File storage.</span></span> <span data-ttu-id="794a2-112">Všimněte si, že jsou zadané koncové body pro obě služby:</span><span class="sxs-lookup"><span data-stu-id="794a2-112">Note that endpoints for both services are specified:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

<span data-ttu-id="794a2-113">A tady je příklad hello stejný připojovací řetězec s kódováním adresy URL:</span><span class="sxs-lookup"><span data-stu-id="794a2-113">And here's an example of hello same connection string with URL encoding:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

