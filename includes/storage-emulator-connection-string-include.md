<span data-ttu-id="bde23-101">emulátor úložiště Hello podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověření sdíleným klíčem.</span><span class="sxs-lookup"><span data-stu-id="bde23-101">hello storage emulator supports a single fixed account and a well-known authentication key for Shared Key authentication.</span></span> <span data-ttu-id="bde23-102">Tento účet a klíč jsou hello pouze povolené pro použití s emulátor úložiště hello pověření sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="bde23-102">This account and key are hello only Shared Key credentials permitted for use with hello storage emulator.</span></span> <span data-ttu-id="bde23-103">Jsou:</span><span class="sxs-lookup"><span data-stu-id="bde23-103">They are:</span></span>

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> <span data-ttu-id="bde23-104">ověřovací klíč Hello nepodporuje emulátor úložiště hello je určena pouze pro testování hello funkce kód pro ověřování klienta.</span><span class="sxs-lookup"><span data-stu-id="bde23-104">hello authentication key supported by hello storage emulator is intended only for testing hello functionality of your client authentication code.</span></span> <span data-ttu-id="bde23-105">Neslouží jakýkoli účel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bde23-105">It does not serve any security purpose.</span></span> <span data-ttu-id="bde23-106">Produkční účtu úložiště a klíč nelze použít s emulátor úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="bde23-106">You cannot use your production storage account and key with hello storage emulator.</span></span> <span data-ttu-id="bde23-107">Neměli byste používat účet vývoj hello s provozními daty.</span><span class="sxs-lookup"><span data-stu-id="bde23-107">You should not use hello development account with production data.</span></span>
> 
> <span data-ttu-id="bde23-108">emulátor úložiště Hello podporuje pouze připojení prostřednictvím protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bde23-108">hello storage emulator supports connection via HTTP only.</span></span> <span data-ttu-id="bde23-109">Protokol HTTPS se ale hello doporučuje protokol pro přístup k prostředkům v produkční účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bde23-109">However, HTTPS is hello recommended protocol for accessing resources in a production Azure storage account.</span></span>
> 

#### <a name="connect-toohello-emulator-account-using-a-shortcut"></a><span data-ttu-id="bde23-110">Toohello emulátoru účtu pomocí zástupce Connect</span><span class="sxs-lookup"><span data-stu-id="bde23-110">Connect toohello emulator account using a shortcut</span></span>
<span data-ttu-id="bde23-111">Hello nejjednodušší způsob, jak tooconnect toohello emulátor úložiště z vaší aplikace je tooconfigure připojovací řetězec v konfiguračním souboru aplikace, který odkazuje na zástupce hello `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="bde23-111">hello easiest way tooconnect toohello storage emulator from your application is tooconfigure a connection string in your application's configuration file that references hello shortcut `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="bde23-112">Tady je příklad připojovací řetězec toohello emulátor úložiště v *app.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="bde23-112">Here's an example of a connection string toohello storage emulator in an *app.config* file:</span></span> 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-toohello-emulator-account-using-hello-well-known-account-name-and-key"></a><span data-ttu-id="bde23-113">Připojit toohello emulátoru účtu pomocí hello známý název účtu a klíč</span><span class="sxs-lookup"><span data-stu-id="bde23-113">Connect toohello emulator account using hello well-known account name and key</span></span>
<span data-ttu-id="bde23-114">toocreate připojovací řetězec, odkazy hello emulátoru název účtu a klíče, budete muset zadat hello koncové body pro každý z hello služby můžete chcete toouse z emulátoru hello hello připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="bde23-114">toocreate a connection string that references hello emulator account name and key, you must specify hello endpoints for each of hello services you wish toouse from hello emulator in hello connection string.</span></span> <span data-ttu-id="bde23-115">To je nezbytné, aby hello připojovací řetězec bude odkazovat koncových bodů emulátoru hello, které se liší od zásad pro účet úložiště produkční.</span><span class="sxs-lookup"><span data-stu-id="bde23-115">This is necessary so that hello connection string will reference hello emulator endpoints, which are different than those for a production storage account.</span></span> <span data-ttu-id="bde23-116">Například hodnota hello připojovací řetězec bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="bde23-116">For example, hello value of your connection string will look like this:</span></span>

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

<span data-ttu-id="bde23-117">Tato hodnota je stejná toohello zástupce uvedené výše, `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="bde23-117">This value is identical toohello shortcut shown above, `UseDevelopmentStorage=true`.</span></span>

#### <a name="specify-an-http-proxy"></a><span data-ttu-id="bde23-118">Zadejte proxy serveru HTTP</span><span class="sxs-lookup"><span data-stu-id="bde23-118">Specify an HTTP proxy</span></span>
<span data-ttu-id="bde23-119">Můžete také zadat toouse proxy protokolu HTTP při testování služby emulátoru úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="bde23-119">You can also specify an HTTP proxy toouse when you're testing your service against hello storage emulator.</span></span> <span data-ttu-id="bde23-120">To může být užitečné pro sledování požadavků a odpovědí HTTP při ladění operace u hello služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="bde23-120">This can be useful for observing HTTP requests and responses while you're debugging operations against hello storage services.</span></span> <span data-ttu-id="bde23-121">toospecify proxy serveru, přidejte hello `DevelopmentStorageProxyUri` možnost toohello připojovací řetězec a nastavit jeho hodnota toohello identifikátor URI proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="bde23-121">toospecify a proxy, add hello `DevelopmentStorageProxyUri` option toohello connection string, and set its value toohello proxy URI.</span></span> <span data-ttu-id="bde23-122">Zde je například připojovací řetězec, který ukazuje emulátor úložiště toohello a nakonfiguruje server proxy protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="bde23-122">For example, here is a connection string that points toohello storage emulator and configures an HTTP proxy:</span></span>

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

