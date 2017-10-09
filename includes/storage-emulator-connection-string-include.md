emulátor úložiště Hello podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověření sdíleným klíčem. Tento účet a klíč jsou hello pouze povolené pro použití s emulátor úložiště hello pověření sdílený klíč. Jsou:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> ověřovací klíč Hello nepodporuje emulátor úložiště hello je určena pouze pro testování hello funkce kód pro ověřování klienta. Neslouží jakýkoli účel zabezpečení. Produkční účtu úložiště a klíč nelze použít s emulátor úložiště hello. Neměli byste používat účet vývoj hello s provozními daty.
> 
> emulátor úložiště Hello podporuje pouze připojení prostřednictvím protokolu HTTP. Protokol HTTPS se ale hello doporučuje protokol pro přístup k prostředkům v produkční účtu úložiště Azure.
> 

#### <a name="connect-toohello-emulator-account-using-a-shortcut"></a>Toohello emulátoru účtu pomocí zástupce Connect
Hello nejjednodušší způsob, jak tooconnect toohello emulátor úložiště z vaší aplikace je tooconfigure připojovací řetězec v konfiguračním souboru aplikace, který odkazuje na zástupce hello `UseDevelopmentStorage=true`. Tady je příklad připojovací řetězec toohello emulátor úložiště v *app.config* souboru: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-toohello-emulator-account-using-hello-well-known-account-name-and-key"></a>Připojit toohello emulátoru účtu pomocí hello známý název účtu a klíč
toocreate připojovací řetězec, odkazy hello emulátoru název účtu a klíče, budete muset zadat hello koncové body pro každý z hello služby můžete chcete toouse z emulátoru hello hello připojovacího řetězce. To je nezbytné, aby hello připojovací řetězec bude odkazovat koncových bodů emulátoru hello, které se liší od zásad pro účet úložiště produkční. Například hodnota hello připojovací řetězec bude vypadat takto:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Tato hodnota je stejná toohello zástupce uvedené výše, `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Zadejte proxy serveru HTTP
Můžete také zadat toouse proxy protokolu HTTP při testování služby emulátoru úložiště hello. To může být užitečné pro sledování požadavků a odpovědí HTTP při ladění operace u hello služby úložiště. toospecify proxy serveru, přidejte hello `DevelopmentStorageProxyUri` možnost toohello připojovací řetězec a nastavit jeho hodnota toohello identifikátor URI proxy serveru. Zde je například připojovací řetězec, který ukazuje emulátor úložiště toohello a nakonfiguruje server proxy protokolu HTTP:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

