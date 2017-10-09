>[!NOTE]
> Komentáře můžete nechat na této stránce pro zpětnou vazbu nebo prostřednictvím [Azure zpětné vazby](https://feedback.azure.com/forums/216843-virtual-machines) s #azerrormessage značkou.

## <a name="error-response-format"></a>Formát odpovědi chyby 
Virtuální počítače Azure pomocí hello formátu JSON pro odpovědi na chybu:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Chybnou odpověď vždy obsahuje stavový kód a objekt chyby. Každý objekt chyba vždy obsahuje kód chyby a zprávu. Pokud hello virtuální počítač je vytvořen pomocí šablony, objekt chyba hello obsahuje také podrobnosti oddíl, který obsahuje vnitřní úroveň kódy chyb a zpráv. Za normálních okolností hello většina vnitřní úroveň chybová zpráva je hello kořenové selhání.


## <a name="common-virtual-machine-management-errors"></a>Běžné chyby správy virtuálního počítače.

Tato část uvádí hello běžné chybové zprávy se můžete setkat při správě virtuálních počítačů:

|  Kód chyby  |  Chybová zpráva  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Při vytváření disku {0}' pomocí {1} identifikátor URI objektu blob selhaly tooacquire zapůjčení. Objekt blob je již používán.  |  
|  AllocationFailed  |  Přidělení se nezdařilo. Prosím zkuste zmenšit velikost virtuálního počítače hello nebo počet virtuálních počítačů, opakujte akci později nebo zkuste nasazení tooa jinou skupinu dostupnosti nebo jiném umístění Azure.  |  
|  AllocationFailed  |  Hello přidělení pro virtuální počítač se nezdařilo z důvodu vnitřní chyby tooan. Opakujte akci později nebo zkuste nasazení tooa jiné umístění.  |
|  ArtifactNotFound  |  Hello rozšíření virtuálního počítače o vydavatele: {0} a typem objekt {1}' nebyl nalezen v umístění '{2}'.  |
|  ArtifactNotFound  |  Objekt {1}' typ rozšíření s vydavatelem: {0}, a verzí obslužné rutiny typu '{2}' nebyl nalezen v úložišti rozšíření hello.  |
|  ArtifactVersionNotFound  |  Žádná verze v úložišti artefaktů hello, který splňuje hello se nenašla požadovaná verze: {0}.  |
|  ArtifactVersionNotFound  |  Žádná verze nalezen v úložišti artefaktů hello, který splňuje hello požadovanou verzi {0}' pro rozšíření virtuálního počítače s vydavatele {1} a typ '{2}'.  |
|  AttachDiskWhileBeingDetached  |  Datový disk {0}' tooVM objekt {1}' nelze připojit, protože hello disk momentálně odpojuje. Počkejte prosím, než je hello disk úplně odpojí a akci opakujte.  |
|  Struktura BadRequest  |  Zarovnání ' skupiny dostupnosti ještě nejsou v této oblasti nepodporuje.  |
|  Struktura BadRequest  |  Přidání virtuálního počítače s spravovaných disků, které toonon spravovat sady dostupnosti nebo přidání virtuálního počítače s toomanaged disky objektů blob na základě sady dostupnosti se nepodporuje. Vytvořte skupiny dostupnosti s vlastností 'spravované' nastavenou v pořadí tooadd virtuální počítač s tooit spravované disky.  |
|  Struktura BadRequest  |  Spravované disky nejsou podporovány v této oblasti.  |
|  Struktura BadRequest  |  Více rozšíření Vmextension na obslužnou rutinu není podporováno pro operační systém, zadejte: {0}. Rozšíření VMExtension objekt {1}' s obslužnou rutinou '{2}' je již přidáno nebo určeno ve vstupu.  |
|  Struktura BadRequest  |  Operace: {0} není podporována u prostředku objekt {1}' u spravovaných disků.  |
|  CertificateImproperlyFormatted  |  Hello reprezentace JSON tajného klíče načíst z {0} obsahuje datové pole, která není správně formátovaným souborem PFX, nebo bylo zadáno heslo hello nedekóduje souboru PFX hello správně.  |
|  CertificateImproperlyFormatted  |  Hello data načtená z {0} není deserializovat do formátu JSON.  |
|  Konflikt  |  Změna velikosti disku je povolená pouze při vytváření virtuálního počítače nebo po deallocated hello virtuálních počítačů.  |
|  ConflictingUserInput  |  Disk {0}' nelze připojit jako hello disk je již vlastněn objekt {1}' virtuální počítač.  |
|  ConflictingUserInput  |  Zdrojové a cílové skupiny prostředků jsou hello stejné.  |
|  ConflictingUserInput  |  Zdrojový účet úložiště pro disk {0} se liší od cílového účtu.  |
|  ContainerAlreadyOnLease  |  V kontejneru úložiště hello podržíte hello objektů blob s identifikátorem URI {0} již existuje zapůjčení.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  žádost o přesun prostředků Hello obsahuje prostředky KeyVault, na které odkazuje jeden nebo více {0} s v žádosti o hello. Aktuálně nejsou podporované mezi předplatnými Move. Zkontrolujte, zda text hello podrobnosti o chybě pro hello identifikátory prostředku KeyVault.  |
|  DiagnosticsOperationInternalError  |  Při zpracovávání diagnostického profilu virtuálního počítače {0} došlo k vnitřní chybě.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  {0} objektu BLOB se už používá jiný disk, který patří tooVM objekt {1}.. Můžete zkontrolovat hello metadata objektu blob pro referenční informace o disku hello.  |
|  DiskBlobNotFound  |  S {0} identifikátor URI pro disk objekt {1}' objekt blob nelze toofind virtuálního pevného disku.  |
|  DiskBlobNotFound  |  Nelze toofind virtuálního pevného disku objektů blob s identifikátorem URI {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  tajný klíč {0} nemá hello {1} značky. Aktualizovat verzi hello tajného klíče, přidejte hello požadované značky a opakujte.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Rozbalení hodnoty tajného {0} hodnotu pomocí klíče {1} se nezdařilo.  |
|  DiskImageNotReady  |  {0} bitové kopie disku je ve stavu {1}. Opakujte akci po obrázek je připravený.  |
|  DiskPreparationError  |  Při přípravě disky virtuálních počítačů došlo k jedné nebo více chybám. Najdete v zobrazení instance disku podrobnosti.  |
|  DiskProcessingError  |  Zpracování disku se zastavilo hello virtuálních počítačů má jiné disky v přírůstcích u disků se nezdařilo.  |
|  ImageBlobNotFound  |  S {0} identifikátor URI pro disk objekt {1}' objekt blob nelze toofind virtuálního pevného disku.  |
|  ImageBlobNotFound  |  Nelze toofind virtuálního pevného disku objektů blob s identifikátorem URI {0}.  |
|  IncorrectDiskBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. {0} objektu BLOB pro objekt {1}' disk je typu objekt BLOB bloku.  |
|  IncorrectDiskBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. Objekt BLOB {0} je typu objekt {1}..  |
|  IncorrectImageBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. {0} objektu BLOB pro objekt {1}' disk je typu objekt BLOB bloku.  |
|  IncorrectImageBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. Objekt BLOB {0} je typu objekt {1}..  |
|  InternalOperationError  |  Nebylo možné přeložit účtu úložiště {0}. Zkontrolujte, že se vytvořil prostřednictvím hello zprostředkovatele prostředku úložiště v hello stejné umístění jako hello výpočetní prostředky.  |
|  InternalOperationError  |  úkolů hledání cíle {0} se nezdařilo.  |
|  InternalOperationError  |  Při ověřování hello síťového profilu virtuálního počítače {0}' došlo k chybě.  |
|  InvalidAccountType  |  Hello AccountType {0} je neplatný.  |
|  InvalidParameter  |  Hello hodnota parametru {0} je neplatný.  |
|  InvalidParameter  |  Zadané heslo správce Hello není povoleno.  |
|  InvalidParameter  |  "hello zadané heslo musí být mezi {0}-\ {1\} znaků a musí splňovat alespoň {2} z hello následující požadavky na složitost hesla: <ol><li> Obsahuje velké písmeno</li><li>Obsahuje malé písmeno.</li><li>Obsahuje číslici</li><li>Obsahuje speciální znak.</li></ol>  |
|  InvalidParameter  |  Hello zadané uživatelské jméno správce není povolené.  |
|  InvalidParameter  |  Nelze připojit existující disk operačního systému, pokud hello virtuální počítač vytvořený z platformy nebo uživatelské image.  |
|  InvalidParameter  |  Název kontejneru {0} je neplatný. Názvy kontejnerů musí být 3 až 63 znaků a může obsahovat jenom malé alfanumerické znaky a spojovníky. Pomlčka musí být dlouhé a následuje alfanumerický znak.  |
|  InvalidParameter  |  {0} název kontejneru v {1} adresa URL je neplatná. Názvy kontejnerů musí být 3 až 63 znaků a může obsahovat jenom malé alfanumerické znaky a spojovníky. Pomlčka musí být dlouhé a následuje alfanumerický znak.  |
|  InvalidParameter  |  Název objektu blob Hello v {0} Adresa URL obsahuje lomítko. V současné době není podporována pro disky.  |
|  InvalidParameter  |  {0} URI Hello nezjišťuje toobe správný identifikátor URI objektu blob.  |
|  InvalidParameter  |  Disk s názvem {0}' již používá stejnou logickou jednotku hello: {1}.  |
|  InvalidParameter  |  Disk s názvem: {0} již existuje.  |
|  InvalidParameter  |  Nelze zadat uživatelská přepsání pro disk již definována v hello zadaný odkaz na obrázek.  |
|  InvalidParameter  |  Disk s názvem {0}' již používá stejnou adresu URL VHD {1} hello.  |
|  InvalidParameter  |  Hello zadaný selhání domény počet {0} musí spadat hello rozsah {1} too\ {2\}.  |
|  InvalidParameter  |  Hello licence typu {0} je neplatný. Platné typy licencí jsou: Windows_Client nebo Windows_Server, rozlišují velká a malá písmena.  |
|  InvalidParameter  |  Název hostitele Linux nemůže být delší než {0} znaků a nesmí obsahovat hello následující znaky: {1}.  |
|  InvalidParameter  |  Cílová cesta pro veřejné klíče Ssh je aktuálně omezená tooits výchozí hodnotu {0} kvůli tooa známý problém s Linuxovým agentem zřizování.  |
|  InvalidParameter  |  Disk na logické jednotce (LUN) {0} již existuje.  |
|  InvalidParameter  |  {0} předplatné hello požadavku musí odpovídat {1} předplatné hello obsažené v id spravovaného disku hello.  |
|  InvalidParameter  |  Vlastní data v OSProfile musí být v kódování Base64 a obsahovat nejvýš {0} znaků.  |
|  InvalidParameter  |  Název objektu BLOB v adrese URL {0} musí mít příponu objekt {1}..  |
|  InvalidParameter  |  {0}' není platná předpona názvu objektu blob zaznamenané virtuálního pevného disku. Platná předpona odpovídá regulárnímu výrazu objekt {1}..  |
|  InvalidParameter  |  Certifikáty nelze přidat tooyour virtuálních počítačů, pokud není zřízený agent virtuálního počítače hello.  |
|  InvalidParameter  |  Disk na logické jednotce (LUN) {0} již existuje.  |
|  InvalidParameter  |  Nelze toocreate hello virtuálních počítačů protože hello požadovaná velikost {0} není k dispozici v hello clusteru, kde je skupina dostupnosti hello právě přiděleno. jsou k dispozici velikosti Hello: {1}. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  virtuální počítač velikost {0} není k dispozici v aktuální oblasti hello požadovaného Hello. Hello jsou k dispozici velikosti v aktuální oblasti hello: {1}. Získat další informace o hello dostupných velikostí virtuálních počítačů v každé oblasti v https://aka.ms/azure-regions.  |
|  InvalidParameter  |  virtuální počítač velikost {0} není k dispozici v aktuální oblasti hello požadovaného Hello. Získat další informace o hello dostupných velikostí virtuálních počítačů v každé oblasti v https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Uživatelské jméno správce Windows nemůže být delší než {0} znaků, tečkou končit ani obsahovat následující znaky hello: {1}.  |
|  InvalidParameter  |  Název počítače Windows nemůže být delší než {0} znaků, složen výhradně z číslic ani obsahovat následující znaky hello: {1}.  |
|  MissingMoveDependentResources  |  žádost o Hello přesun prostředků neobsahuje všechny závislé prostředky hello. Zkontrolujte podrobnosti o chybě pro ID chybějících prostředků.  |
|  MoveResourcesHaveInvalidState  |  Hello žádost o přesunutí prostředků obsahuje virtuální počítače, které jsou spojeny s účty úložiště nejsou platné. Zkontrolujte podrobnosti o těchto ID prostředků a názvy účtů odkazovaných úložiště.  |
|  MoveResourcesHavePendingOperations  |  žádost o přesun prostředků Hello obsahuje prostředky, pro které čeká na vyřízení operace. Zkontrolujte podrobnosti o těchto ID prostředků. Opakujte operaci po dokončení hello čekající operace.  |
|  MoveResourcesNotFound  |  Hello přesunout prostředky požadavek obsahuje prostředky, které nelze nalézt. Zkontrolujte podrobnosti o těchto ID prostředků.  |
|  NetworkingInternalOperationError  |  Neznámá chyba přidělení sítě.  |
|  NetworkingInternalOperationError  |  Neznámá chyba přidělení sítě  |
|  NetworkingInternalOperationError  |  Došlo k vnitřní chybě při zpracování síťového profilu hello virtuálních počítačů.  |
|  notFound  |  Hello sadu dostupnosti {0} nebyl nalezen.  |
|  notFound  |  Zdrojový virtuální počítač: {0} určený v požadavku hello v tomto umístění Azure neexistuje.  |
|  notFound  |  Klient s ID {0} se nenašel.  |
|  notFound  |  Hello Image {0} nebyl nalezen.  |
|  NotSupported  |  Typ licence Hello je {0}, ale není z místního {1} objektu blob bitové kopie hello.  |
|  OperationNotAllowed  |  Dostupnost sady {0} nelze odstranit. Před odstraněním skupiny dostupnosti zkontrolujte, zda neobsahuje žádné virtuální počítače.  |
|  OperationNotAllowed  |  Změna dostupnosti nastavit SKU z too'Classic 'Pevně' ' není povolen.  |
|  OperationNotAllowed  |  Rozšíření v hello virtuálních počítačů nelze změnit, pokud není spuštěna hello virtuálních počítačů.  |
|  OperationNotAllowed  |  Hello akce zachycení je podporována pouze na virtuálním počítači s disky, na základě objektů blob. Použijte prosím hello 'Image' prostředků rozhraní API toocreate bitové kopie ze spravovaných virtuálního počítače.  |
|  OperationNotAllowed  |  Hello prostředku {0} nelze vytvořit z Image {1}, dokud bitové kopie byla úspěšně vytvořena.  |
|  OperationNotAllowed  |  TooencryptionSettings aktualizace není povolená, pokud je virtuální počítač přidělen, zkuste to prosím znovu po zrušení jeho přidělení virtuálního počítače  |
|  OperationNotAllowed  |  Přidání tooa spravovaných disků na virtuální počítač s disky, na základě objektů blob není podporována.  |
|  OperationNotAllowed  |  Hello maximální počet datových disků povolená tooa toobe připojit virtuální počítač tato velikost je {0}.  |
|  OperationNotAllowed  |  Přidání objektu blob na základě disku tooVM s spravované disky není podporováno.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na bitovou kopii objekt {1}' od hello bitové kopie je označena pro odstranění. Můžete pouze operaci odstranění hello (nebo počkejte probíhající jeden toocomplete).  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' od hello virtuální počítač je zobecněný.  |
|  OperationNotAllowed  |  Operace: {0} není povolena, protože kolekce bodu obnovení objekt {1}' je označena pro odstranění.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na rozšíření virtuálního počítače objekt {1}' vzhledem k tomu, že je označena pro odstranění. Můžete pouze operaci odstranění hello (nebo počkejte probíhající jeden toocomplete).  |
|  OperationNotAllowed  |  Operace: {0} není povolená, protože virtuální počítače hello objekt {1}' jsou se zřídí pomocí bitové kopie hello '{2}..  |
|  OperationNotAllowed  |  Operace {0}' není povolena, protože hello virtuálního počítače ScaleSet objekt {1}' právě používá hello Image '{2}'.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' od hello virtuálního počítače je označena pro odstranění. Můžete pouze operaci odstranění hello (nebo počkejte probíhající jeden toocomplete).  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuální počítač {1}, protože hello virtuálních počítačů je deallocated nebo označený toobe navrácena.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' od hello, který je spuštěný virtuální počítač. Prosím vypnutí explicitně v případě, že vypnete hello virtuální počítač z uvnitř hello hostovaného operačního systému.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' od hello virtuálního počítače není navrácena.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že virtuální počítač má příponu '{2}' v chybovém stavu.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}', protože probíhá jiná operace.  |
|  OperationNotAllowed  |  operace Hello: {0} vyžaduje hello virtuálního počítače objekt {1}' toobe zobecněno.  |
|  OperationNotAllowed  |  operace Hello vyžaduje toobe hello virtuální počítač spuštěn (nebo nastavit toorun).  |
|  OperationNotAllowed  |  Disk s velikost {0} GB, což je menší než hello velikost {1}GB odpovídající disku v bitové kopii, není povolená.  |
|  OperationNotAllowed  |  Škálovací sady virtuálního počítače rozšíření obslužné rutiny {0}' lze přidat pouze v době hello vytváření Škálovací sady virtuálního počítače.  |
|  OperationNotAllowed  |  Odstranit lze pouze v době hello odstraňování sady škálování virtuálního počítače sady škálování virtuálního počítače rozšíření obslužné rutiny: {0}.  |
|  OperationNotAllowed  |  Virtuální počítač {0}' již používá spravované disky.  |
|  OperationNotAllowed  |  Virtuální počítač {0}' patří too'Classic' objekt {1}' skupiny dostupnosti. Aktualizace hello dostupnosti nastavte prosím toouse 'pevně, SKU a opakujte hello převod.  |
|  OperationNotAllowed  |  Virtuální počítač vytvořený z Image nemůže mít objekt blob na základě disky. Všechny disky mají toobe spravované disky.  |
|  OperationNotAllowed  |  Zaznamenat operaci nelze dokončit, protože hello virtuální počítač není zobecněný.  |
|  OperationNotAllowed  |  Operace správy na virtuálním počítači {0}' nejsou povoleny, protože disky virtuálních počítačů bude převedený toomanaged disky.  |
|  OperationNotAllowed  |  Probíhající operace mění stav napájení virtuálního počítače {0} too\ {1\}. Proveďte operaci {2} po určité době.  |
|  OperationNotAllowed  |  Nelze tooadd nebo aktualizace hello virtuálních počítačů. Hello požadovaného {0} velikost virtuálního počítače nemusí být k dispozici v hello existující alokační jednotky. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Nelze tooresize hello virtuálních počítačů protože hello požadovaná velikost {0} není k dispozici v hello clusteru, kde je skupina dostupnosti hello právě přiděleno. jsou k dispozici velikosti Hello: {1}. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Nelze tooresize hello virtuálních počítačů protože hello požadovaná velikost {0} není k dispozici v clusteru hello kde hello virtuální počítač je právě přiděleno. tooresize too\ váš virtuální počítač {1\} prosím navrácení (to je zastavení operace v hello portál Azure) a opakujte operaci hello změny velikosti. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Zřizování operačního systému se nezdařila pro virtuální počítač {0}, protože se právě zřizuje hello hostovaný operační systém.  |
|  OSProvisioningClientError  |  Zřizování operačního systému pro virtuální počítač {0} se nezdařilo. Podrobnosti o chybě: {1} zkontrolujte, zda text hello image správně připravena (generalizována). <ul><li>Pokyny pro Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Generování klíčů SSH hostitele se nezdařilo. Podrobnosti o chybě: {0}. tooresolve tento problém, ověřte, pokud je správně nastavený agenta systému Linux. <ul><li>Můžete zkontrolovat pokynů hello: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Uživatelské jméno je zadáno hello že virtuálního počítače je neplatný pro této distribuce systému Linux. Podrobnosti o chybě: {0}.  |
|  OSProvisioningInternalError  |  Zřizování operačního systému se nezdařilo pro virtuální počítač {0}' z důvodu vnitřní chyby tooan.  |
|  OSProvisioningTimedOut  |  Zřizování operačního systému pro virtuální počítač {0}' nebyla dokončena v hello vyhrazených čas. Hello virtuálních počítačů může stále dokončit úspěšně. Zkontrolujte prosím stav zřizování později.  |
|  OSProvisioningTimedOut  |  Zřizování operačního systému pro virtuální počítač {0}' nebyla dokončena v hello vyhrazených čas. Hello virtuálních počítačů může stále dokončit úspěšně. Zkontrolujte prosím stav zřizování později. Taky se ujistěte, hello image správně připravena (generalizována).   <ul><li>Pokyny pro Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Pokyny pro Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  Zřizování operačního systému pro virtuální počítač {0}' nebyla dokončena v hello vyhrazených čas. Agenta hosta virtuálního počítače hello však byla zjištěna systémem. To naznačuje hello hostovaného operačního systému nebyl správně připravený toobe použít jako image virtuálního počítače (CreateOption = FromImage). tooresolve-li tento problém, hello buď použijte virtuální pevný disk, protože je s CreateOption = připojit nebo ji správně připravit pro použití jako obrázek:   <ul><li>Pokyny pro Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Pokyny pro Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Hello požadovaná velikost virtuálního počítače není aktuálně k dispozici v hello vybrané umístění.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  V tuto chvíli kvůli aktualizace tooongoing platformy teď prostředek nelze aktualizovat. Zkuste to prosím znova později.  |
|  StorageAccountLimitation  |  Účet úložiště: {0} nepodporuje objekty BLOB stránky, které jsou požadované toocreate disky.  |
|  StorageAccountLimitation  |  Účet úložiště {0} překročil jemu přidělenou kvótu.  |
|  StorageAccountLocationMismatch  |  Nebylo možné přeložit účtu úložiště {0}. Zkontrolujte, že se vytvořil prostřednictvím hello zprostředkovatele prostředku úložiště v hello stejné umístění jako hello výpočetní prostředky.  |
|  StorageAccountNotFound  |  Úložiště účet {0} nebyl nalezen. Ujistěte se, účet úložiště není odstraněný a že patří toohello stejného umístění Azure jako hello virtuálních počítačů.  |
|  StorageAccountNotRecognized  |  Použijte prosím účet úložiště spravovaný poskytovatelem prostředků úložiště. Použití {0} není podporováno.  |
|  StorageAccountOperationInternalError  |  Při přístupu k účtu úložiště {0} nastala vnitřní chyba.  |
|  StorageAccountSubscriptionMismatch  |  {0} účet úložiště nepatří toosubscription {1}.  |
|  StorageAccountTooBusy  |  Účet úložiště {0}' je aktuálně zaneprázdněna. Zvažte použití jiného účtu.  |
|  StorageAccountTypeNotSupported  |  Disk {0} používá {1}, což je účet úložiště Blob. Zkuste to prosím znovu s účtem úložiště pro obecné účely.  |
|  StorageAccountTypeNotSupported  |  {0} účet úložiště je typu {1}. Diagnostika spouštění podporuje typy účtů úložiště {2}.  |
|  SubscriptionNotAuthorizedForImage  |  Hello předplatné není autorizované.  |
|  TargetDiskBlobAlreadyExists  |  Objekt BLOB {0} již existuje. Zadejte identifikátor URI toocreate jiný objektu blob disku pro nový prázdný datový objekt {1}..  |
|  TargetDiskBlobAlreadyExists  |  Zaznamenat operace nemůže pokračovat, protože cíl image blob {0} již existuje a není nastavený BLOB VHD toooverwrite příznak hello. Buď odstranit objekt blob hello nebo nastavte příznak hello toooverwrite BLOB VHD a opakujte.  |
|  TargetDiskBlobAlreadyExists  |  Operace zachytávání nemůže pokračovat, protože cílový objekt blob bitové kopie {0} má aktivní zapůjčení.   |
|  TargetDiskBlobAlreadyExists  |  Objekt BLOB {0} již existuje. Zadejte jiný identifikátor URI objektu blob jako cíl pro disk objekt {1}..  |
|  TooManyVMRedeploymentRequests  |  Pro virtuální počítač {0}' byly přijaty příliš mnoho požadavků na opětovné nasazení nebo hello hello virtuální počítače ve stejné sadě dostupnosti tohoto virtuálního počítače. Zkuste to prosím znovu později.  |
|  VHDSizeInvalid  |  Hello zadat, že hodnota velikosti disku {0} pro disk objekt {1}' s {2} objektu blob je neplatný. Velikost disku musí být až {3}, {4}.  |
|  VMAgentStatusCommunicationError  |  Virtuální počítač {0}' neohlásil stav pro agenta virtuálního počítače nebo rozšíření. Ověřte, zda text hello virtuálních počítačů má spuštěn agent virtuálního počítače a může vytvořit úložiště tooAzure odchozí připojení.  |
|  VMArtifactRepositoryInternalError  |  Došlo k chybě při komunikaci se službou hello artefaktů úložiště tooretrieve virtuálních počítačů podrobnosti o artefaktu.  |
|  VMArtifactRepositoryInternalError  |  Došlo k vnitřní chybě při načítání dat artefaktů virtuálního počítače hello z úložiště artefaktů hello.  |
|  VMExtensionHandlerNonTransientError  |  Obslužná rutina: {0} oznámil chybu pro rozšíření virtuálního počítače objekt {1}' s kódem chyby terminálu, {2} a chybová zpráva: '{3}.  |
|  VMExtensionManagementInternalError  |  Při zpracování rozšíření virtuálního počítače {0} nastala  vnitřní chyba.  |
|  VMExtensionManagementInternalError  |  Při přípravě rozšíření virtuálního počítače hello došlo k několika chybám. Najdete v zobrazení instance virtuálního počítače rozšíření podrobnosti.  |
|  VMExtensionProvisioningError  |  Virtuální počítač nahlásil chybu při zpracování rozšíření: {0}. Chybová zpráva: "{1}".  |
|  VMExtensionProvisioningError  |  Několik rozšíření virtuálního počítače se nezdařilo toobe zřízený hello virtuálních počítačů. Podrobnosti viz hello virtuálního počítače rozšíření zobrazení instance podrobnosti.  |
|  VMExtensionProvisioningTimeout  |  Zřizování rozšíření virtuálního počítače {0}' časový limit. Instalace rozšíření možná trvá příliš dlouho, nebo nebylo možné získat stav rozšíření.  |
|  VMMarketplaceInvalidInput  |  Vytvoření virtuálního počítače z image pořízenou prostřednictvím Marketplace bez není nutné informace o plánu, odeberte prosím v žádosti o hello hello informace o plánu. Název disku operačního systému je {0}.  |
|  VMMarketplaceInvalidInput  |  informace o nákupu Hello neodpovídá. Nelze toodeploy z Marketplace hello bitové kopie. Název disku operačního systému je {0}.  |
|  VMMarketplaceInvalidInput  |  Vytvoření virtuálního počítače image pořízené na Marketplace vyžaduje hello požadavek obsahovat informace o plánu. Název disku operačního systému je {0}.  |
|  VMNotFound  |  virtuální počítač Hello: {0} nebyl nalezen.  |
|  VMRedeploymentFailed  |  Opětovné nasazení virtuálního počítače {0} se nezdařilo z důvodu vnitřní chyby tooan. Zkuste to prosím znovu později.  |
|  VMRedeploymentTimedOut  |  Opětovné nasazení virtuálního počítače {0}' neskončila v hello vyhrazených čas. Se může úspěšně dokončit v určité době. Jinak můžete opakovat požadavek hello.  |
|  VMStartTimedOut  |  Virtuální počítač {0}' nebyla spuštěna ve vyhrazených čas hello. Hello virtuální počítač stále může úspěšně spustit. Zkontrolujte prosím stav napájení hello později.  |


## <a name="next-steps"></a>Další kroky
Pokud potřebujete další pomoc, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.
