<span data-ttu-id="d5e3e-101">Emulátor úložiště podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověření sdíleným klíčem.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-101">The storage emulator supports a single fixed account and a well-known authentication key for Shared Key authentication.</span></span> <span data-ttu-id="d5e3e-102">Tento účet a klíč jsou pouze povolené pro použití s emulátor úložiště pověření sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-102">This account and key are the only Shared Key credentials permitted for use with the storage emulator.</span></span> <span data-ttu-id="d5e3e-103">Jsou:</span><span class="sxs-lookup"><span data-stu-id="d5e3e-103">They are:</span></span>

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> <span data-ttu-id="d5e3e-104">Ověřovací klíč nepodporuje emulátor úložiště je určena pouze pro testování funkce kód pro ověřování klienta.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-104">The authentication key supported by the storage emulator is intended only for testing the functionality of your client authentication code.</span></span> <span data-ttu-id="d5e3e-105">Neslouží jakýkoli účel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-105">It does not serve any security purpose.</span></span> <span data-ttu-id="d5e3e-106">Produkční účtu úložiště a klíč nelze použít s emulátor úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-106">You cannot use your production storage account and key with the storage emulator.</span></span> <span data-ttu-id="d5e3e-107">Vývoj pro účet byste neměli používat s provozními daty.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-107">You should not use the development account with production data.</span></span>
> 
> <span data-ttu-id="d5e3e-108">Emulátor úložiště podporuje pouze připojení prostřednictvím protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-108">The storage emulator supports connection via HTTP only.</span></span> <span data-ttu-id="d5e3e-109">Ale HTTPS je protokol doporučené pro přístup k prostředkům v produkční účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-109">However, HTTPS is the recommended protocol for accessing resources in a production Azure storage account.</span></span>
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a><span data-ttu-id="d5e3e-110">Připojení k účtu emulátoru pomocí zástupce</span><span class="sxs-lookup"><span data-stu-id="d5e3e-110">Connect to the emulator account using a shortcut</span></span>
<span data-ttu-id="d5e3e-111">Nejjednodušší způsob, jak připojit k emulátor úložiště z vaší aplikace je nakonfigurovat připojovací řetězec v konfiguračním souboru aplikace, který odkazuje na zástupce `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-111">The easiest way to connect to the storage emulator from your application is to configure a connection string in your application's configuration file that references the shortcut `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="d5e3e-112">Tady je příklad připojovacího řetězce pro emulátor úložiště v *app.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="d5e3e-112">Here's an example of a connection string to the storage emulator in an *app.config* file:</span></span> 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a><span data-ttu-id="d5e3e-113">Připojení k účtu emulátor používá známý účet název a klíč</span><span class="sxs-lookup"><span data-stu-id="d5e3e-113">Connect to the emulator account using the well-known account name and key</span></span>
<span data-ttu-id="d5e3e-114">Chcete-li vytvořit připojovací řetězec, který odkazuje na emulátoru název účtu a klíč, musíte zadat koncové body pro jednotlivé služby, které chcete použít z emulátoru serveru v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-114">To create a connection string that references the emulator account name and key, you must specify the endpoints for each of the services you wish to use from the emulator in the connection string.</span></span> <span data-ttu-id="d5e3e-115">To je nezbytné, aby připojovací řetězec bude odkazovat emulátoru koncových bodů, které se liší od zásad pro účet úložiště produkční.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-115">This is necessary so that the connection string will reference the emulator endpoints, which are different than those for a production storage account.</span></span> <span data-ttu-id="d5e3e-116">Například hodnota připojovací řetězec bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d5e3e-116">For example, the value of your connection string will look like this:</span></span>

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

<span data-ttu-id="d5e3e-117">Tato hodnota je stejná jako zástupce uvedené výše, `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-117">This value is identical to the shortcut shown above, `UseDevelopmentStorage=true`.</span></span>

#### <a name="specify-an-http-proxy"></a><span data-ttu-id="d5e3e-118">Zadejte proxy serveru HTTP</span><span class="sxs-lookup"><span data-stu-id="d5e3e-118">Specify an HTTP proxy</span></span>
<span data-ttu-id="d5e3e-119">Můžete také zadat proxy server HTTP použít při testování služby emulátoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-119">You can also specify an HTTP proxy to use when you're testing your service against the storage emulator.</span></span> <span data-ttu-id="d5e3e-120">To může být užitečné pro sledování požadavků a odpovědí HTTP při ladění operace u služby storage.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-120">This can be useful for observing HTTP requests and responses while you're debugging operations against the storage services.</span></span> <span data-ttu-id="d5e3e-121">Chcete-li zadat proxy server, přidejte `DevelopmentStorageProxyUri` možnost připojovací řetězec a jeho hodnotu nastavte identifikátor URI proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="d5e3e-121">To specify a proxy, add the `DevelopmentStorageProxyUri` option to the connection string, and set its value to the proxy URI.</span></span> <span data-ttu-id="d5e3e-122">Zde je například připojovací řetězec, který odkazuje na emulátor úložiště a nakonfiguruje server proxy protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="d5e3e-122">For example, here is a connection string that points to the storage emulator and configures an HTTP proxy:</span></span>

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

