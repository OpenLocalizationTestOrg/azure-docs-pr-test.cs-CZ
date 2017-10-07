---
title: "aaaCreate Azure Service Fabric clusteru ze šablony | Microsoft Docs"
description: "Tento článek popisuje, jak clusteru tooset až zabezpečené Service Fabric v Azure pomocí Azure Resource Manager, Azure Key Vault a Azure Active Directory (Azure AD) pro ověřování klientů."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: a4563c58a68127720a8290c3be0df9d026833eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Vytvořit cluster Service Fabric pomocí Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Tento podrobný průvodce vás provede procesem nastavení zabezpečení clusteru Azure Service Fabric v Azure pomocí Azure Resource Manager. Jsme na vědomí, že tento článek hello je dlouhá. Nicméně pokud jste už znát hello obsah, být zda toofollow každý krok pečlivě.

Průvodce Hello popisuje hello následující postupy:

* Nastavení služby Azure trezoru klíčů tooupload certifikátů pro zabezpečení clusteru a aplikace
* Vytvoření clusteru s podporou zabezpečené v Azure pomocí Azure Resource Manager
* Ověřování uživatelů pomocí služby Azure Active Directory (Azure AD) pro správu clusteru

Cluster s podporou zabezpečení je cluster, který brání neoprávněným přístupem toomanagement operace. To zahrnuje nasazení, upgrade a odstraňování aplikace, služby a hello data, která obsahují. Nezabezpečená clusteru je cluster, že každý, kdo připojit tooat kdykoli a provádět operace správy. Přestože je možné toocreate nezabezpečená clusteru, důrazně doporučujeme vytvořit cluster s podporou zabezpečení z hello outset. Protože nezabezpečená clusteru nelze zabezpečit později, je nutné vytvořit nový cluster.

Hello konceptu vytváření zabezpečené clustery je hello stejné, jestli jsou v systému Linux nebo Windows clusterů. Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v části [vytváření zabezpečené clusterů v systému Linux](#secure-linux-clusters).

## <a name="sign-in-tooyour-azure-account"></a>Přihlaste se tooyour účet Azure
Tato příručka používá [prostředí Azure PowerShell][azure-powershell]. Po spuštění novou relaci prostředí PowerShell se přihlaste tooyour účet Azure a vybrat své předplatné před spuštěním příkazů Azure.

Přihlaste se tooyour účet Azure:

```powershell
Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a>Nastavení trezoru klíčů
Tato část obsahuje informace o vytvoření trezoru klíčů pro cluster Service Fabric v Azure a aplikace Service Fabric. Dokončení Průvodce tooAzure Key Vault, najdete v části toohello [Key Vault Příručka Začínáme][key-vault-get-started].

Service Fabric používá toosecure certifikáty X.509 cluster a poskytují funkce zabezpečení aplikací. Používáte certifikáty toomanage Key Vault pro clusterů Service Fabric v Azure. Pokud cluster je nasazené v Azure, vyžaduje certifikáty od Key Vault hello poskytovatel prostředků Azure, která je odpovědná za vytváření clusterů Service Fabric a nainstaluje je hello clusteru virtuálních počítačů.

Hello následující diagram znázorňuje hello vztah mezi Azure Key Vault, cluster Service Fabric a hello poskytovatel prostředků Azure, která používá certifikáty uložené v trezoru klíčů při vytváření clusteru:

![Diagram instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
prvním krokem Hello je toocreate skupinu prostředků speciálně pro váš trezor klíčů. Doporučujeme převést trezoru klíčů hello do vlastní skupiny prostředků. Tato akce umožňuje odstranit hello výpočetního prostředí a úložiště skupiny prostředků, včetně hello skupinu prostředků, která obsahuje daný cluster Service Fabric bez ztráty klíče a tajné klíče. Hello skupinu prostředků, která obsahuje váš trezor klíčů _musí být v hello stejné oblasti_ jako hello cluster, který se používá.

Pokud máte v plánu toodeploy clustery v několika oblastech, doporučujeme pojmenovat hello skupinu prostředků a trezoru klíčů hello způsobem, který určuje, které oblasti patří do.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
výstup Hello by měl vypadat takto:

```powershell

    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-hello-new-resource-group"></a>Vytvoření trezoru klíčů v hello novou skupinu prostředků
Trezor klíčů Hello _musí být povolen pro nasazení_ tooallow hello výpočetních prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte ji na instancí virtuálního počítače:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

výstup Hello by měl vypadat takto:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
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
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Použít existující trezor klíčů

toouse existující trezor klíčů, můžete _ji musíte povolit pro nasazení_ tooallow hello výpočetních prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte ji na uzlech clusteru:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-tooyour-key-vault"></a>Přidejte certifikáty tooyour klíče trezoru

Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace. Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster a server certifikátu (povinné)
Tento certifikát je požadovaná toosecure cluster a zabránit tooit neoprávněného přístupu. Poskytuje zabezpečení clusteru dvěma způsoby:

* Ověřování clusteru: ověřuje komunikaci mezi uzly pro cluster federaci. Pouze uzly, které můžete prokázání své identity s tímto certifikátem můžete připojit hello cluster.
* Ověření serveru: ověřuje hello clusteru správy koncové body tooa správy klienta, tak, aby hello klienta pro správu zná ho je rozhovoru toohello skutečné clusteru. Tento certifikát také poskytuje protokolem SSL pro hello rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.

tooserve tyto účely, hello certifikátu musí splňovat následující požadavky hello:

* Hello certifikát musí obsahovat privátní klíč.
* Hello certifikátu musí být vytvořeny pro výměnu klíčů, což je exportovatelný tooa soubor Personal Information Exchange (.pfx).
* Název subjektu certifikátu Hello musí odpovídat hello domény, že používáte cluster Service Fabric tooaccess hello. Toto porovnání se vyžaduje tooprovide protokolem SSL pro koncové body správy protokolu HTTPS hello clusteru a Service Fabric Exploreru. Nelze získat certifikát SSL od certifikační autority (CA) pro hello. cloudapp.azure.com domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.

### <a name="application-certificates-optional"></a>Certifikáty aplikace (volitelné)
Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů. Před vytvořením clusteru, zvažte hello aplikace zabezpečení scénáře, které vyžadují certifikát toobe, nainstalovaná na uzlech hello, jako například:

* Šifrování a dešifrování hodnot konfigurace aplikace.
* Šifrování dat mezi uzly během replikace.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certifikáty formátování pro použití poskytovatele prostředků Azure.
Můžete přidat a používat soubory privátních klíčů (.pfx) přímo prostřednictvím trezoru klíčů. Poskytovatel prostředků Azure výpočetní hello však vyžaduje toobe klíče uloženy v speciální formátu JavaScript Object Notation (JSON). Formát Hello obsahuje soubor .pfx hello jako základní řetězec s kódováním base64 a hello heslo soukromého klíče. tooaccommodate tyto požadavky hello klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako "tajemství" v klíči hello trezoru.

toomake to zpracovat snadnější, [modul prostředí PowerShell je dostupná na Githubu][service-fabric-rp-helpers]. modul hello toouse, hello následující:

1. Stáhněte si celý obsah hello hello úložiště do místního adresáře.
2. Přejděte toohello místní adresář.
2. Importujte modulu ServiceFabricRPHelpers hello v okně prostředí PowerShell:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

Hello `Invoke-AddCertToKeyVault` příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ho toohello trezoru klíčů. Použít hello příkaz tooadd hello clusteru certifikát a všechny další aplikaci certifikáty toohello trezoru klíčů. Tento krok opakujte pro všechny další certifikáty, že chcete tooinstall v clusteru.

#### <a name="uploading-an-existing-certificate"></a>Odesílání existující certifikát

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Pokud dojde k chybě, jako je například hello té, kterou tady vidíte obvykle to znamená, že máte konflikt adresy URL prostředku. tooresolve hello konflikt, změnit název trezoru klíčů hello.

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Po vyřešení konfliktu hello hello výstup by měl vypadat přibližně takto:

```

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Je nutné hello tři předchozí řetězce, CertificateThumbprint SourceVault a CertificateURL, tooset zabezpečení clusteru Service Fabric a tooobtain všechny certifikáty aplikace, které používáte pro zabezpečení aplikací. Pokud uložíte není hello řetězce, může být obtížné tooretrieve je dotazováním hello klíč trezoru později.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-toohello-key-vault"></a>Vytvořit certifikát podepsaný svým držitelem a pak ho nahrát toohello trezoru klíčů

Pokud jste již odeslali trezoru klíčů toohello certifikáty, tento krok přeskočte. Tento krok je pro generování nový certifikát podepsaný svým držitelem a pak ho nahrát tooyour trezoru klíčů. Jakmile změňte parametry hello v hello následující skript a potom ho spusťte, vám měla zobrazit výzva k zadání hesla certifikátu.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want hello .PFX toobe stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Pokud dojde k chybě, jako je například hello té, kterou tady vidíte obvykle to znamená, že máte konflikt adresy URL prostředku. konflikt hello tooresolve, název trezoru klíčů hello změnu, název RG a tak dále.

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Po vyřešení konfliktu hello hello výstup by měl vypadat přibližně takto:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context tooSubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: hello output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret toochackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Je nutné hello tři předchozí řetězce, CertificateThumbprint SourceVault a CertificateURL, tooset zabezpečení clusteru Service Fabric a tooobtain všechny certifikáty aplikace, které používáte pro zabezpečení aplikací. Pokud uložíte není hello řetězce, může být obtížné tooretrieve je dotazováním hello klíč trezoru později.

 V tomto okamžiku byste měli mít následující prvky zavedené hello:

* Skupina prostředků Hello trezoru klíčů.
* Hello trezoru klíčů a jeho adresa URL (nazývané SourceVault v hello předcházející výstup prostředí PowerShell).
* ověřovací certifikát serverů clusteru Hello a jeho adresu URL v trezoru klíčů hello.
* Hello aplikace certifikáty a jejich adresy URL v trezoru klíčů hello.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Nastavení Azure Active Directory pro ověřování klientů

Azure AD umožňuje tooapplications přístup uživatele toomanage organizace (známé jako klienty). Aplikace se dál dělí na ty s webové přihlášení uživatelského rozhraní a ty prostředí nativního klienta. V tomto článku předpokládáme, že jste již vytvořili klienta. Pokud máte není, spusťte načtením [jak tooget Azure Active Directory klienta][active-directory-howto-tenant].

Cluster Service Fabric nabízí několik vstupní body tooits funkce správy, včetně hello webové [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] a [Visual Studio] [service-fabric-manage-application-in-visual-studio]. V důsledku toho vytvoříte dva Azure AD aplikace toocontrol přístup toohello cluster, jednu webovou aplikaci a jeden nativní aplikaci.

toosimplify některé z kroků hello součástí konfigurace služby Azure AD se cluster Service Fabric, jsme vytvořili sadu skriptů prostředí Windows PowerShell.

> [!NOTE]
> Je třeba provést následující kroky před vytvořením clusteru hello hello. Skripty hello očekává koncové body a názvy clusterů, hello hodnoty by měla být plánované a není hodnoty, že jste již vytvořili.

1. [Stáhněte si skripty hello] [ sf-aad-ps-script-download] tooyour počítače.
2. Klikněte pravým tlačítkem na hello soubor zip, vyberte **vlastnosti**, vyberte hello **Odblokovat** zaškrtněte políčko a potom klikněte na **použít**.
3. Extrahujte soubor zip hello.
4. Spustit `SetupApplications.ps1`a zadejte hello TenantId, název clusteru a WebApplicationReplyUrl jako parametry. Například:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Vaše ID Tenanta můžete najít spuštěním příkazu Powershellu hello `Get-AzureSubscription`. Provádění tento příkaz zobrazí hello TenantId pro každé předplatné.

    Název clusteru je použité tooprefix hello Azure AD aplikace, které jsou vytvořeny skriptem hello. Přesně nepotřebuje toomatch hello skutečný název clusteru. Je určený jenom toomake je snazší toomap Azure AD artefakty toohello Service Fabric clusteru, který se používá s.

    WebApplicationReplyUrl je výchozí koncový bod hello vracející Azure AD Uživatelé tooyour po jejich dokončení přihlašování. Nastavte tento koncový bod jako koncový bod Service Fabric Explorer hello pro váš cluster, který ve výchozím nastavení je:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Jste výzvami toosign v tooan účtu, který má oprávnění správce pro tenanta hello Azure AD. Po přihlášení, hello skript vytvoří hello web a nativní aplikace toorepresent cluster Service Fabric. Pokud si prohlédnete hello klienta aplikace v hello [portál Azure classic][azure-classic-portal], měli byste vidět dvě nové položky:

   * *Název clusteru*\_clusteru
   * *Název clusteru*\_klienta

   skript Hello vytiskne hello JSON vyžadují šablony Azure Resource Manageru hello při vytváření clusteru hello v další části hello, takže je vhodné okno PowerShell hello tookeep otevřete.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Vytvoření šablony správce prostředků clusteru Service Fabric
V této části hello výstupy z hello předchozí příkazy prostředí PowerShell se používají v šabloně Resource Manager clusteru Service Fabric.

Ukázka Resource Manager šablony jsou dostupné v hello [šablony Azure úvodní Galerie na Githubu][azure-quickstart-templates]. Tyto šablony slouží jako výchozí bod pro šablonu clusteru.

### <a name="create-hello-resource-manager-template"></a>Vytvoření šablony Resource Manageru hello
Tato příručka používá hello [5 uzlu clusteru zabezpečené] [ service-fabric-secure-cluster-5-node-1-nodetype] příklad šablony a parametry šablony. Stáhněte si `azuredeploy.json` a `azuredeploy.parameters.json` tooyour počítače a otevřete oba soubory ve svém oblíbeném textovém editoru.

### <a name="add-certificates"></a>Přidat certifikáty
Pod položkou hello trezoru klíčů, který obsahuje klíče certifikátu hello přidáte šablony správce prostředků clusteru tooa certifikáty. Doporučujeme umístit hello klíč trezoru hodnoty v souboru parametrů šablony Resource Manageru. Tím zajistí, že budou hello Resource Manager soubor šablony opakovaně použitelné volné nasazení konkrétní tooa hodnoty.

#### <a name="add-all-certificates-toohello-virtual-machine-scale-set-osprofile"></a>Přidejte že všechny certifikáty toohello škálovací sady virtuálních počítačů osProfile
Každý certifikát, který je nainstalován v clusteru hello musí být nakonfigurované v části osProfile hello hello škálování sadu prostředků (Microsoft.Compute/virtualMachineScaleSets). Tato akce nastaví hello hello certifikát tooinstall poskytovatele prostředků na hello virtuálních počítačů. Tato instalace zahrnuje hello clusteru certifikátů a certifikáty zabezpečení jakékoli aplikace, abyste naplánovali toouse pro vaše aplikace:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-hello-service-fabric-cluster-certificate"></a>Konfigurace certifikátu clusteru Service Fabric hello
certifikát pro ověřování Hello clusteru musí být konfigurované v prostředku clusteru Service Fabric obou hello (Microsoft.ServiceFabric/clusters) a nastaví hello Service Fabric rozšíření pro škálování virtuálních počítačů v prostředek sady škálování virtuálního počítače hello. Toto uspořádání umožňuje tooconfigure zprostředkovatele prostředků Service Fabric hello ho použít pro ověření clusteru a ověření serveru pro správu koncové body.

##### <a name="virtual-machine-scale-set-resource"></a>Škálovací sady virtuálních počítačů prostředků:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Prostředek infrastruktury služby:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Vložit konfigurace služby Azure AD
Konfigurace Hello Azure AD, kterou jste vytvořili dříve lze vložit přímo do vaší šablony Resource Manageru. Jsme ale doporučujeme, abyste nejdřív extrahovat hello hodnoty do s parametry souboru tookeep hello Resource Manager šablony opakovaně použitelného a bez nasazení konkrétní tooa hodnoty.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < "Konfigurace arm" ></a>parametry šablony konfigurace správce prostředků
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since hello link seems not toobe redirecting correctly --->
Nakonec použijte hodnoty výstup hello z trezoru klíčů hello a souboru parametrů Azure AD PowerShell příkazy toopopulate hello:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
V tomto okamžiku byste měli mít následující prvky zavedené hello:

* Skupina prostředků trezoru klíčů
  * Trezor klíčů
  * Ověřovací certifikát serverů clusteru
  * Certifikát pro šifrování dat
* Klientovi služby Azure Active Directory
  * Aplikace Azure AD pro správu webových a Service Fabric Exploreru
  * Aplikace pro správu nativního klienta Azure AD.
  * Uživatelé a jejich přiřazené role
* Šablony Resource Manageru clusteru Service Fabric
  * Certifikáty nakonfigurovaný pomocí trezoru klíčů
  * Azure Active Directory, které jsou nakonfigurované

Hello následující diagram znázorňuje, kde trezoru klíčů a konfigurace Azure AD zapadnou do vaší šablony Resource Manageru.

![Mapa závislostí Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-hello-cluster"></a>Vytvoření clusteru hello
Nyní jste připravené toocreate hello clusteru pomocí [nasazení šablony Azure resource][resource-group-template-deploy].

#### <a name="test-it"></a>otestovat
V souboru parametrů použijte šablony správce prostředků hello následující tootest příkaz prostředí PowerShell:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>nasazení
Pokud testovací šablony Resource Manageru hello úspěšně projde, použijte hello následující toodeploy příkaz prostředí PowerShell vaše šablony Resource Manageru soubor parametrů:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-tooroles"></a>Přiřazení uživatelů tooroles
Po vytvoření aplikace toorepresent hello cluster, přiřadit uživatelům role toohello nepodporuje Service Fabric: jen pro čtení a správce. Můžete přiřadit role hello pomocí hello [portál Azure classic][azure-classic-portal].

1. V hello portálu Azure, přejděte tooyour klienta a potom vyberte **aplikace**.
2. Vyberte hello webové aplikace, který má název, jako je `myTestCluster_Cluster`.
3. Klikněte na tlačítko hello **uživatelé** kartě.
4. Vyberte uživatele tooassign a potom klikněte na hello **přiřadit** tlačítko dole hello úvodní obrazovka.

    ![Přiřazení uživatelů tooroles tlačítko][assign-users-to-roles-button]
5. Vyberte hello role tooassign toohello uživatele.

    ![Dialogové okno "Přiřazení uživatelů"][assign-users-to-roles-dialog]

> [!NOTE]
> Další informace o rolích v Service Fabric najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused hello wrong redirection of hello link --->

## <a name="create-secure-clusters-on-linux"></a>Vytvoření zabezpečeného clusterů v systému Linux
proces toomake hello jednodušší, uvádíme [pomocné rutiny skriptu](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Než použijete tento skript pomocné rutiny, ujistěte se, že už máte rozhraní příkazového řádku Azure (CLI) nainstalovaná a je v cestě. Ujistěte se, že hello skript má oprávnění tooexecute spuštěním `chmod +x cert_helper.py` po stáhnout. prvním krokem Hello je toosign v tooyour účet Azure pomocí rozhraní příkazového řádku s hello `azure login` příkaz. Po přihlášení tooyour účet Azure, použijte hello pomocné rutiny, skript se certifikační Autorita certifikátu podepsaného držitelem, jako hello následující příkaz ukazuje:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

Parametr - ifile Hello může trvat soubor .pfx nebo soubor .pem jako vstup pro typ certifikátu hello (pfx nebo pem nebo ss, pokud je certifikát podepsaný svým držitelem).
Hello parametr -h vytiskne textu hello nápovědy.


Tento příkaz vrátí hello následující tři řetězce jako výstup hello:

* SourceVaultID, což je hello ID pro hello se vytvoří nový KeyVault ResourceGroup
* CertificateUrl pro přístup k certifikátu hello
* Miniatura certifikátu, který se používá pro ověřování

Hello následující příklad ukazuje, jak toouse hello příkaz:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Provádění hello předcházející příkaz poskytuje hello tři řetězce následujícím způsobem:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

Název subjektu certifikátu Hello musí odpovídat hello domény, že používáte cluster Service Fabric tooaccess hello. Toto porovnání se vyžaduje tooprovide protokolem SSL pro koncové body správy protokolu HTTPS hello clusteru a Service Fabric Exploreru. Nelze získat certifikát SSL od certifikační Autority pro hello `.cloudapp.azure.com` domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.

Tyto názvy subjektu jsou hello položky je třeba toocreate zabezpečení clusteru Service Fabric (bez Azure AD), jak je popsáno v [parametrů šablony Resource Manageru nakonfigurovat](#configure-arm). Zabezpečení clusteru toohello můžete připojit podle pokynů hello pro [ověřování clusteru tooa přístupu klienta](service-fabric-connect-to-secure-cluster.md). Linux preview clustery nepodporují ověřování Azure AD. Můžete přiřadit role správce a klienta, jak je popsáno v hello [přiřadit role toousers](#assign-roles) části. Když zadáte role správce a klienta pro Linux preview clusteru, máte tooprovide kryptografické otisky certifikátů pro ověřování. (Nezadáte název subjektu hello, protože žádné ověření řetězu nebo odvolání je prováděna v této verzi preview.)

Pokud chcete toouse certifikát podepsaný svým držitelem pro testování, můžete použít stejné toogenerate skriptu, jeden hello. Potom můžete nahrát hello certifikát tooyour trezoru klíčů tím, že poskytuje hello příznak `ss` místo hello název a cesta k certifikátu certifikátu. Například najdete hello následující příkaz pro vytvoření a odeslání certifikát podepsaný svým držitelem:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Tento příkaz vrátí hello stejné tři řetězce: SourceVault, CertificateUrl a CertificateThumbprint. Potom můžete hello řetězce toocreate zabezpečené Linux clusteru a umístění, kde je umístěn hello certifikát podepsaný svým držitelem. Je nutné hello certifikát podepsaný svým držitelem tooconnect toohello clusteru. Zabezpečení clusteru toohello můžete připojit podle pokynů hello pro [ověřování clusteru tooa přístupu klienta](service-fabric-connect-to-secure-cluster.md).

Název subjektu certifikátu Hello musí odpovídat hello domény, že používáte cluster Service Fabric tooaccess hello. Toto porovnání se vyžaduje tooprovide protokolem SSL pro koncové body správy protokolu HTTPS hello clusteru a Service Fabric Exploreru. Nelze získat certifikát SSL od certifikační Autority pro hello `.cloudapp.azure.com` domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.

Můžete vyplnit hello parametry z hello pomocná skript v hello portál Azure, jak je popsáno v hello [vytvořit cluster v hello portál Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) části.

## <a name="next-steps"></a>Další kroky
V tuto chvíli máte zabezpečené cluster s poskytnete správu ověřování Azure Active Directory. Dále [připojení clusteru tooyour](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak příliš[spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Řešení potíží s nastavení Azure Active Directory pro ověřování klientů
Pokud narazíte na problém při při instalaci Azure AD pro ověřování klientů, přečtěte si hello možná řešení v této části.

### <a name="service-fabric-explorer-prompts-you-tooselect-a-certificate"></a>Service Fabric Explorer vyzve tooselect certifikát
#### <a name="problem"></a>Problém
Po přihlášení úspěšně tooAzure AD v Service Fabric Exploreru hello prohlížeče vrací toohello domovskou stránku, ale dotaz, zda tooselect certifikát.

![Vyberte certifikát SFX dialogové okno][sfx-select-certificate-dialog]

#### <a name="reason"></a>Důvod
uživatel Hello není přiřazenou roli v hello clusteru aplikaci Azure AD. Ověření Azure AD se proto nezdaří na cluster Service Fabric. Service Fabric Explorer spadne zpět toocertificate ověřování.

#### <a name="solution"></a>Řešení
Postupujte podle pokynů hello pro nastavení služby Azure AD a přiřadit role uživatele. Také doporučujeme zapnout "Uživatele přiřazení tooaccess požadované aplikace," jako `SetupApplications.ps1` nepodporuje.

### <a name="connection-with-powershell-fails-with-an-error-hello-specified-credentials-are-invalid"></a>Připojení pomocí prostředí PowerShell selže s chybou: "hello zadané přihlašovací údaje jsou neplatné"
#### <a name="problem"></a>Problém
Pokud používáte PowerShell tooconnect toohello clusteru pomocí "Azureactivedirectory selhala" režim zabezpečení, po přihlášení úspěšně tooAzure AD, hello připojení se nezdaří s chybou: "hello zadat přihlašovací údaje jsou neplatné."

#### <a name="solution"></a>Řešení
Toto řešení je hello stejný jako hello předcházející jeden.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer vrátí chybu při přihlášení: "AADSTS50011"
#### <a name="problem"></a>Problém
Při pokusu o toosign v tooAzure AD v Service Fabric Exploreru, stránku hello vrátí chybu: "AADSTS50011: hello adresa pro odpověď &lt;url&gt; neodpovídá hello odpovědi adresy nakonfigurované pro aplikace hello: &lt;guid&gt;."

![Adresa pro odpověď SFX neodpovídá.][sfx-reply-address-not-match]

#### <a name="reason"></a>Důvod
aplikace Hello clusteru (web), která představuje Service Fabric Exploreru pokusí tooauthenticate služby Azure AD a jako součást požadavku hello poskytuje návratovou adresu URL přesměrování hello. Adresa URL hello není uvedena v aplikaci hello Azure AD, ale **adresa URL odpovědi** seznamu.

#### <a name="solution"></a>Řešení
Na hello **konfigurace** kartě hello clusteru aplikace (web), přidejte adresu URL Service Fabric Explorer toohello hello **adresa URL odpovědi** seznamu nebo nahradit hello položky v seznamu hello. Po dokončení, uložte změnu.

![Adresa url odpovědi pro webové aplikace][web-application-reply-url]

### <a name="connect-hello-cluster-by-using-azure-ad-authentication-via-powershell"></a>Připojte hello cluster pomocí ověřování Azure AD pomocí prostředí PowerShell
tooconnect hello cluster Service Fabric, použijte hello následující ukázka příkazu prostředí PowerShell:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

najdete v článku o rutině hello Connect-ServiceFabricCluster toolearn [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-hello-same-azure-ad-tenant-in-multiple-clusters"></a>Můžete opakovaně použít klienta hello stejnou službou Azure AD ve více clusterů?
Ano. Ale mějte na paměti, tooadd hello URL Service Fabric Explorer tooyour clusteru (web) aplikace. Service Fabric Exploreru, jinak nebude fungovat.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Proč stále potřebuji certifikát serveru je zapnuto Azure AD?
FabricClient a FabricGateway provést vzájemné ověření. Během ověřování Azure AD integrace Azure AD poskytuje klienta serveru toohello identit a certifikát serveru hello je identita serveru použité tooverify hello. Další informace o certifikátech Service Fabric najdete v tématu [certifikáty X.509 a Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

