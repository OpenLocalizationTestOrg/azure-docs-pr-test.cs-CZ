---
title: "cluster Service Fabric aaaCreate v hello portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak hello tooset zabezpečení clusteru Service Fabric v Azure pomocí portálu Azure a Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 045f71b491260e741ce7a54a75c440e1b33059a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-hello-azure-portal"></a>Vytvořit cluster Service Fabric v Azure pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Toto je podrobný průvodce, který vás provede kroky hello nastavení zabezpečení clusteru Service Fabric v Azure pomocí hello portálu Azure. Tento průvodce vás provede hello následující kroky:

* Nastavení klíče toomanage Key Vault pro zabezpečení clusteru.
* Vytvoření zabezpečeného clusteru v Azure prostřednictvím hello portálu Azure.
* Ověřování pomocí certifikátů správci.

> [!NOTE]
> Pro pokročilejší možnosti zabezpečení, jako je například ověřování uživatelů s Azure Active Directory a nastavení certifikátů pro zabezpečení aplikací [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].
> 
> 

Cluster s podporou zabezpečení je clusteru, který brání neoprávněným přístupem toomanagement operace, která zahrnuje nasazení, upgrade a odstranění aplikací, služeb a hello data, která obsahují. Nezabezpečená clusteru je cluster, že každý, kdo připojit tooat kdykoli a provádět operace správy. Přestože je možné toocreate nezabezpečená clusteru, je **důrazně doporučujeme toocreate cluster zabezpečené**. Nezabezpečená clusteru **nelze zabezpečit později** -je nutné vytvořit nový cluster.

Koncepty Hello jsou hello stejné pro vytvoření zabezpečeného clusterů, ať už hello clustery jsou clusterů systému Linux nebo Windows clusterů. Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v tématu [vytváření zabezpečené clusterů v systému Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters). Hello parametry získané skriptem hello Pomocník poskytuje může být vstup přímo na portálu hello jak je popsáno v části hello [vytvořit cluster v hello portál Azure](#create-cluster-portal).

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure
Tato příručka používá [prostředí Azure PowerShell][azure-powershell]. Při spouštění novou relaci prostředí PowerShell, přihlaste se tooyour účet Azure a vybrat své předplatné před provedením Azure příkazy.

Přihlaste se účtem tooyour azure:

```powershell
Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Nastavení služby Key Vault
Tato část hello Průvodce vás provede procesem vytváření Key Vault pro cluster Service Fabric v Azure a aplikace Service Fabric. Dokončení průvodce v Key Vault, najdete v části hello [Key Vault Příručka Začínáme][key-vault-get-started].

Service Fabric používá toosecure certifikáty X.509 clusteru. Azure Key Vault je použité toomanage certifikáty pro clusterů Service Fabric v Azure. Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure hello zodpovědný za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je hello clusteru virtuálních počítačů.

Hello následující diagram znázorňuje hello vztah mezi Key Vault, cluster Service Fabric a hello poskytovatel prostředků Azure, která používá certifikáty uložené v Key Vault, při vytváření clusteru:

![Instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
prvním krokem Hello je toocreate novou skupinu prostředků speciálně pro Key Vault. Uvedení Key Vault do vlastní skupiny prostředků se doporučuje, aby můžete odebrat skupiny prostředků výpočetního prostředí a úložiště – například hello skupiny prostředků, který má cluster Service Fabric – bez ztráty klíče a tajné klíče. Skupina prostředků Hello, který má Key Vault musí být v hello stejné oblasti jako hello clusteru, který se používá.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: hello output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Vytvoření trezoru klíčů
Vytvoření trezoru klíč v hello novou skupinu prostředků. Hello Key Vault **musí být povolen pro nasazení** tooallow hello Service Fabric prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte na uzly clusteru:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions tooKeys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions tooSecrets   :    all


    Tags                             :
```

Pokud máte existující Key Vault, můžete ji povolit pro nasazení pomocí rozhraní příkazového řádku Azure:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-tookey-vault"></a>Přidejte certifikáty tooKey trezoru
Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace. Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster a server certifikátu (povinné)
Tento certifikát je požadovaná toosecure cluster a zabránit tooit neoprávněného přístupu. Poskytuje zabezpečení clusteru několik způsoby:

* **Ověřování clusteru:** ověřuje komunikaci mezi uzly pro cluster federaci. Pouze uzly, které můžete prokázání své identity s tímto certifikátem můžete připojit hello cluster.
* **Ověření serveru:** ověřuje hello clusteru správy koncové body tooa správy klienta, tak, aby hello klienta pro správu zná ho je rozhovoru toohello skutečné clusteru. Tento certifikát také poskytuje protokol SSL pro hello rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.

tooserve tyto účely, hello certifikátu musí splňovat následující požadavky hello:

* Hello certifikát musí obsahovat privátní klíč.
* Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).
* Hello názvu subjektu certifikátu musí odpovídat cluster Service Fabric hello domény použít tooaccess hello. Toto je požadovaná tooprovide SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer hello clusteru. Nelze získat certifikát SSL od certifikační autority (CA) pro hello `.cloudapp.azure.com` domény. Získejte vlastní název domény pro váš cluster. Při žádosti o certifikát od název subjektu certifikátu certifikační Autority hello musí odpovídat názvu vlastní domény hello používá pro váš cluster.

### <a name="client-authentication-certificates"></a>Certifikáty pro ověřování klientů
Další klientské certifikáty ověřit správci pro úlohy správy clusteru. Service Fabric má dvě úrovně přístupu: **správce** a **jen pro čtení uživatele**. Minimálně je třeba použít jeden certifikát pro přístup pro správu. Pro další přístupu na úrovni uživatele je třeba zadat samostatný certifikát. Další informace o přístupu rolí najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty][service-fabric-cluster-security-roles].

Není nutné tooupload klienta ověřování certifikátů tooKey trezoru toowork s Service Fabric. Tyto certifikáty stačí toobe poskytuje toousers, kteří mají oprávnění pro správu clusteru. 

> [!NOTE]
> Azure Active Directory je, že hello doporučená způsob tooauthenticate klientů pro cluster operace správy. toouse Azure Active Directory, musíte [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].
> 
> 

### <a name="application-certificates-optional"></a>Certifikáty aplikace (volitelné)
Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů. Před vytvořením clusteru, zvažte hello aplikace zabezpečení scénáře, které vyžadují certifikát toobe, nainstalovaná na uzlech hello, jako například:

* Šifrování a dešifrování hodnot konfigurace aplikace
* Šifrování dat mezi uzly během replikace 

Při vytváření clusteru prostřednictvím hello portál Azure se nedá nakonfigurovat certifikáty k aplikaci. certifikáty k aplikaci tooconfigure během instalace clusteru, musíte [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm]. Můžete také přidat aplikace certifikáty toohello clusteru po jeho vytvoření.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certifikáty formátování pro použití poskytovatele prostředků Azure.
Soubory privátních klíčů (.pfx) můžete přidat a používat přímo přes Key Vault. Poskytovatel prostředků Azure hello však vyžaduje toobe klíče uložené ve speciální formátu JSON, který zahrnuje hello .pfx jako kódování base-64 kódovaného řetězec a hello heslo soukromého klíče. tooaccommodate tyto požadavky klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako *tajné klíče* v Key Vault.

modul prostředí PowerShell je toomake to zpracovat snadnější, [dostupná na Githubu][service-fabric-rp-helpers]. Postupujte podle těchto kroků toouse hello modul:

1. Stáhněte si celý obsah hello hello úložiště do místního adresáře. 
2. Import modulu hello v okně prostředí PowerShell:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

Hello `Invoke-AddCertToKeyVault` příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ho tooKey trezoru. Použijte ji tooadd hello clusteru certifikát a všechny další aplikaci certifikáty tooKey trezoru. Tento krok opakujte pro všechny další certifikáty, že chcete tooinstall v clusteru.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: hello output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomyvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Toto jsou všechny požadované součásti hello Key Vault pro konfiguraci šablony správce prostředků clusteru Service Fabric, která nainstaluje certifikáty pro ověřování uzlu, zabezpečení koncového bodu správy a ověření a zabezpečení žádné další aplikace Funkce, které používají certifikáty X.509. V tomto okamžiku teď byste měli mít hello po instalaci v Azure:

* Skupina prostředků Key Vault
  * Key Vault
    * Ověřovací certifikát serverů clusteru

< /a "Vytvoření cluster-portal" ></a>

## <a name="create-cluster-in-hello-azure-portal"></a>Vytvoření clusteru v hello portálu Azure
### <a name="search-for-hello-service-fabric-cluster-resource"></a>Vyhledejte hello prostředku clusteru Service Fabric
![Vyhledejte šablony clusteru Service Fabric na hello portálu Azure.][SearchforServiceFabricClusterTemplate]

1. Přihlaste se toohello [portál Azure][azure-portal].
2. Klikněte na tlačítko **nový** tooadd novou šablonu prostředků. Vyhledejte šablony hello Service Fabric Cluster v hello **Marketplace** pod **všechno, co**.
3. Vyberte **Service Fabric Cluster** hello seznamu.
4. Přejděte toohello **Service Fabric Cluster** okně klikněte na tlačítko **vytvořit**,
5. Hello **cluster Service Fabric vytvořit** okno obsahuje hello následující čtyři kroky.

#### <a name="1-basics"></a>1. Základy
![Snímek obrazovky vytváření nové skupiny prostředků.][CreateRG]

V okně základy hello musíte tooprovide hello základní informace pro váš cluster.

1. Zadejte název hello clusteru.
2. Zadejte **uživatelské jméno** a **heslo** pro vzdálená plocha pro hello virtuálních počítačů.
3. Ujistěte se, zda text hello tooselect **předplatné** má váš clusteru toobe nasazen, zvláště pokud máte více předplatných.
4. Vytvoření **novou skupinu prostředků**. Je nejlepší toogive hello stejný název jako hello clusteru, protože pomáhá při hledání je později, zejména v případě, že se pokoušíte toomake změny tooyour nasazení nebo odstranit cluster.
   
   > [!NOTE]
   > I když toouse existující skupinu prostředků, můžete rozhodnout, je vhodné toocreate novou skupinu prostředků. Díky tomu snadno toodelete clustery, které nepotřebujete.
   > 
   > 
5. Vyberte hello **oblast** ve kterém chcete toocreate hello clusteru. Je nutné použít hello se stejné oblasti, kterou si klíč trezoru.

#### <a name="2-cluster-configuration"></a>2. Konfigurace clusteru
![Vytvoření typu uzlu][CreateNodeType]

Nakonfigurujte uzly clusteru. Typy uzlů definovat velikosti virtuálních počítačů hello hello počet virtuálních počítačů a jejich vlastnosti. Cluster může mít více než jeden typ uzlu, ale typ primárního uzlu hello (hello první, kterou definujete na portálu hello) musí mít aspoň pět virtuálních počítačů, protože tento typ uzlu hello umístění Service Fabric systémových služeb. Nekonfigurujte **vlastnosti umístění** vzhledem k tomu je automaticky přidá výchozí umístění vlastnost "NodeTypeName".

> [!NOTE]
> Běžný scénář pro více typů uzlu je aplikace, která obsahuje službu front-endu a back endové službě. Chcete tooput hello front-endových služeb na menší virtuálních počítačů (velikosti virtuálních počítačů jako D2) s porty otevřené toohello Internet, ale bez internetového portů otevřené, aby tooput hello back-end službu na větší virtuální počítače (s velikostí virtuálního počítače jako D4, D6, D15 a tak dále).
> 
> 

1. Zvolte název pro váš typ uzlu (1 too12 znaků obsahující pouze písmena a číslice).
2. Hello minimální **velikost** virtuálních počítačů pro primární uzel hello typ doprovází hello **odolnost** vrstvy, které jste vybrali pro hello clusteru. Výchozí hodnota Hello pro vrstvu odolnost hello je Bronzová. Další informace o odolnost, najdete v části [jak toochoose hello Service Fabric clusteru spolehlivost a odolnost][service-fabric-cluster-capacity].
3. Vyberte velikost virtuálního počítače hello a cenovou úroveň. Virtuální počítače D-series jednotek SSD a jsou doporučovány stavových aplikací. Použít všechny virtuálních počítačů SKU, který má částečné jader nebo mají méně než 7 GB kapacity disku. 
4. Hello minimální **číslo** virtuálních počítačů pro primární uzel hello typ doprovází hello **spolehlivost** vrstvy, které zvolíte. Výchozí hodnota Hello pro úroveň spolehlivosti hello je Silver. Další informace o spolehlivosti, najdete v části [jak toochoose hello Service Fabric clusteru spolehlivost a odolnost][service-fabric-cluster-capacity].
5. Zvolte hello počet virtuálních počítačů pro typ uzlu hello. Je možné škálovat později na nahoru nebo dolů hello počet virtuálních počítačů v uzlu typu, ale na primárním uzlu, který typ hello, hello minimální doprovází hello úroveň spolehlivosti, která jste vybrali. Ostatní typy uzlů může mít minimálně 1 virtuální počítač.
6. Nakonfigurujte vlastní koncové body. Toto pole umožňuje tooenter textový soubor s oddělovači seznam portů, které chcete tooexpose prostřednictvím hello nástroj pro vyrovnávání zatížení Azure toohello veřejného Internetu pro vaše aplikace. Například pokud máte v plánu toodeploy clusteru tooyour webové aplikace, zadejte "80" zde tooallow přenosy na portu 80 do clusteru. Další informace o koncových bodů najdete v tématu [komunikaci s aplikacemi][service-fabric-connect-and-communicate-with-services]
7. Konfigurace clusteru **diagnostiky**. Ve výchozím nastavení jsou diagnostiky povolené na váš clusteru tooassist s řešení potíží. Pokud chcete změnit toodisable diagnostiky hello **stav** přepnutí příliš**vypnout**. Vypnutí možnosti diagnostiky je **není** nedoporučuje.
8. Vyberte hello Fabric upgradu, které chcete nastavit clusteru režimu. Vyberte **automatické**, pokud chcete vybrat tooautomatically systému hello až hello nejnovější dostupnou verzi a zkuste tooupgrade tooit vašeho clusteru. Nastavit režim hello příliš**ruční**, pokud chcete, aby toochoose na podporovanou verzi.

> [!NOTE]
> Podporujeme pouze clustery, které běží podporovaná verze služby prostředků infrastruktury. Výběrem hello **ruční** režimu přenášíte na hello odpovědnost tooupgrade tooa podporované verze vašeho clusteru. Další podrobnosti o režimu upgradu hello prostředků infrastruktury, najdete v tématu hello [service fabric clusteru upgrade dokumentu.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Zabezpečení
![Snímek obrazovky konfigurace zabezpečení na portálu Azure.][SecurityConfigs]

posledním krokem Hello je tooprovide certifikátu informace toosecure hello clusteru pomocí hello Key Vault a certifikát informace vytvořený dříve.

* Naplnění polí hello primární certifikát získaný odesílání hello výstup hello **clusteru certifikát** pomocí trezoru tooKey hello `Invoke-AddCertToKeyVault` příkaz prostředí PowerShell.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Zkontrolujte hello **konfigurovat upřesňující nastavení** pole tooenter klientské certifikáty pro **správce klienta** a **jen pro čtení klienta**. V těchto polích zadejte hello kryptografický otisk certifikátu klienta správce a hello kryptografický otisk certifikátu klienta uživatele jen pro čtení, pokud je k dispozici. Pokud se správce pokusí o tooconnect toohello clusteru, získají přístup pouze v případě, že mají certifikát s kryptografickým otiskem, který odpovídá hodnoty kryptografického otisku hello zadali tady.  

#### <a name="4-summary"></a>4. Souhrn
![Snímek obrazovky Tabule start hello zobrazení "Nasazení Cluster Service Fabric." ][Notifications]

Vytvoření clusteru hello toocomplete, klikněte na tlačítko **Souhrn** toosee hello konfigurace, které jste zadali, nebo stáhnout hello šablony Azure Resource Manageru, který používá toodeploy vašeho clusteru. Po zadané povinné nastavení hello hello **OK** bude zelené tlačítko a hello procesu vytváření clusteru můžete spustit kliknutím.

Zobrazí se průběh vytváření hello v oznámeních hello. (Klikněte na ikonu zvonku"hello" v blízkosti hello stavového řádku v hello pravé horní části obrazovky.) Pokud jste klikli na **Pin tooStartboard** při vytváření clusteru hello, uvidíte **nasazení Cluster Service Fabric** připnutý toohello **spustit** panelu.

### <a name="view-your-cluster-status"></a>Zobrazení stavu clusteru
![Snímek obrazovky podrobnosti o clusteru hello řídicím panelu.][ClusterDashboard]

Po vytvoření clusteru si můžete prohlédnout cluster hello portálu:

1. Přejděte příliš**Procházet** a klikněte na tlačítko **clustery infrastruktury služby**.
2. Vyhledejte cluster a klikněte na něj.
3. Zobrazí podrobnosti o hello clusteru na řídicím panelu hello, včetně veřejný koncový bod clusteru hello a odkaz tooService Fabric Exploreru.

Hello **uzlu monitorování** část v okně řídicí panel clusteru hello označuje hello počet virtuálních počítačů, které jsou v pořádku a není v pořádku. Můžete najít další podrobnosti o stavu hello clusteru na [Service Fabric stavu modelu ÚVOD][service-fabric-health-introduction].

> [!NOTE]
> Clusterů Service Fabric vyžadují určitý počet uzlů toobe dostupnost vždy toomaintain a zachovávají stav – odkazované tooas "Správa kvora". Proto, je obvykle není bezpečné tooshut dolů všechny počítače v clusteru hello Pokud jste provedli nejprve [úplného zálohování vaší stavu][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-tooa-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Vzdálené připojení instance tooa sadu škálování virtuálního počítače nebo uzlu clusteru
Každý z hello NodeTypes zadáte v clusteru výsledky do sady škálování virtuálního počítače získávání nastavení. V tématu [vzdáleného připojení instance sadu škálování virtuálního počítače tooa] [ remote-connect-to-a-vm-scale-set] podrobnosti.

## <a name="next-steps"></a>Další kroky
V tuto chvíli máte zabezpečené cluster pro správu ověřování pomocí certifikátů. Dále [připojení clusteru tooyour](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak příliš[spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).  Také další informace o [možnosti podpory Service Fabric](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
