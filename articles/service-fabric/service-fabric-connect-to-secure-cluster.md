---
title: "aaaConnect bezpečně clusteru Azure Service Fabric tooan | Microsoft Docs"
description: "Popisuje, jak klient tooauthenticate přístup ke clusteru Service Fabric tooa a toosecure komunikace mezi klienty a cluster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 1b6a87a1fefaddce2043c604ca53751157232170
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-secure-cluster"></a>Připojte tooa zabezpečené cluster

Když se klient připojí tooa uzlu clusteru Service Fabric, hello klienta může být ověřený a zabezpečenou komunikaci navázat pomocí certifikátů, zabezpečení nebo Azure Active Directory (AAD). Toto ověřování zajistí jenom autorizovaným uživatelům můžete přístup ke clusteru hello a nasazené aplikace a provádět úlohy správy.  Certifikát nebo AAD zabezpečení musí být dříve povolen v clusteru hello při vytvoření clusteru hello.  Další informace o scénáře zabezpečení clusteru najdete v tématu [clusteru zabezpečení](service-fabric-cluster-security.md). Pokud se připojujete tooa clusteru zabezpečené pomocí certifikátů, [nastavit hello klientský certifikát](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) hello počítače, který připojí toohello clusteru. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-tooa-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Připojte tooa zabezpečené cluster pomocí Azure Service Fabric rozhraní příkazového řádku (sfctl)

Existuje několik různých způsobů tooconnect tooa zabezpečení clusteru pomocí hello Service Fabric rozhraní příkazového řádku (sfctl). Při ověřování pomocí certifikátu klienta, certifikát hello podrobnosti musí shodovat se certifikát nasadí toohello uzly clusteru. Pokud má váš certifikát certifikační autority (CA), je třeba tooadditionally zadejte hello důvěryhodné certifikační autority.

Můžete připojit pomocí hello clusteru tooa `sfctl cluster select` příkaz.

Klientské certifikáty lze zadat v dva různé způsoby, buď jako dvojice certifikát a klíč, nebo jako soubor pem jeden. Pro chráněné heslem `pem` budou soubory, výzva automaticky tooenter hello heslo.

toospecify hello klientský certifikát jako soubor pem, zadejte cestu k souboru hello v hello `--pem` argument. Například:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Soubory pem chráněné heslem zobrazí výzvu k předchozí toorunning heslo libovolný příkaz.

toospecify certifikát pár klíčů použijte hello `--cert` a `--key` argumenty toospecify hello odpovídající soubor tooeach cest souborů.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Někdy certifikáty používají toosecure testu nebo clustery dev nezdaří ověření certifikátu. toobypass certifikát ověřování, zadejte hello `--no-verify` možnost. Například:

> [!WARNING]
> Nepoužívejte hello `no-verify` možnost při připojování clusterů Service Fabric tooproduction.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Kromě toho můžete zadat cesty toodirectories certifikátů důvěryhodné certifikační Autority nebo jednotlivých certifikátů. toospecify tyto cesty použít hello `--ca` argument. Například:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

Když se připojíte, měli byste mít příliš[spouštění jiných příkazů sfctl](service-fabric-cli.md) toointeract s clusterem hello.

<a id="connectsecurecluster"></a>

## <a name="connect-tooa-cluster-using-powershell"></a>Připojte tooa cluster pomocí prostředí PowerShell
Před provedením operace na clusteru s podporou pomocí prostředí PowerShell, nejprve vytvořit cluster toohello připojení. připojení clusteru Hello se používá pro všechny následné příkazy v hello danou relaci prostředí PowerShell.

### <a name="connect-tooan-unsecure-cluster"></a>Připojte tooan nezabezpečená cluster

Nezabezpečená tooconnect tooan clusteru, zadejte hello clusteru koncový bod adresy toohello **Connect-ServiceFabricCluster** příkaz:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a>Připojte tooa zabezpečené cluster pomocí služby Azure Active Directory

tooa tooconnect zabezpečené se cluster, který používá Azure Active Directory tooauthorize správce přístup ke clusteru, zadejte kryptografický otisk certifikátu hello clusteru a použít hello *azureactivedirectory selhala* příznak.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Připojte tooa zabezpečené cluster pomocí klientského certifikátu
Spuštění hello následující příkaz prostředí PowerShell tooconnect tooa zabezpečení clusteru, který používá přístup správce tooauthorize certifikáty klienta. Zadejte kryptografický otisk certifikátu hello clusteru a hello kryptografický otisk hello klientský certifikát, kterému byla udělena oprávnění pro správu clusteru. Podrobnosti o Hello certifikátu musí odpovídat certifikát na uzlech clusteru hello.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* je hello kryptografický otisk certifikátu serveru hello nainstalované na uzlech clusteru hello. *FindValue* je hello kryptografický otisk certifikátu klienta správce hello.
Když jsou vyplněna hello parametry, hello příkaz vypadá hello následující ukázka: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-tooa-secure-cluster-using-windows-active-directory"></a>Připojte tooa zabezpečené cluster pomocí služby Windows Active Directory
Pokud váš cluster samostatné se nasazuje pomocí zabezpečení AD, připojte toohello cluster připojením hello přepínač "WindowsCredential".

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-tooa-cluster-using-hello-fabricclient-apis"></a>Připojte tooa cluster pomocí hello FabricClient rozhraní API
Hello Service Fabric SDK poskytuje hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) třídy pro správu clusteru. hello toouse FabricClient rozhraní API, získat hello balíček Microsoft.ServiceFabric NuGet.

### <a name="connect-tooan-unsecure-cluster"></a>Připojte tooan nezabezpečená cluster

tooconnect tooa vzdálený zabezpečená cluster vytvořit instanci FabricClient a zadejte adresu clusteru hello:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Pro kód, který je spuštěn z v rámci clusteru, například v spolehlivě, vytvořte FabricClient *bez* zadání adresy clusteru hello. FabricClient připojí toohello místní správy brány na kód hello hello uzlu na aktuálně probíhá, zabraňující další síti směrování.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Připojte tooa zabezpečené cluster pomocí klientského certifikátu

Hello uzly v clusteru hello musí mít platné certifikáty, jejichž běžný název nebo název DNS v síti SAN se zobrazí v hello [RemoteCommonNames vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) nastavit na [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Provedení tohoto postupu se umožní vzájemné ověřování mezi klientem hello a hello uzly clusteru.

```csharp
using System.Fabric;
using System.Security.Cryptography.X509Certificates;

string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
var fc = new FabricClient(xc, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "My";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = clientCertThumb;
    xc.RemoteCommonNames.Add(name);
    xc.RemoteCertThumbprints.Add(serverCertThumb);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```

### <a name="connect-tooa-secure-cluster-interactively-using-azure-active-directory"></a>Připojte tooa zabezpečené cluster interaktivně pomocí služby Azure Active Directory

Následující příklad používá Azure Active Directory pro klienta certifikát identity a server pro identitu serveru Hello.

Dialogové okno se automaticky objeví pro interaktivní přihlášení při připojování toohello clusteru.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

### <a name="connect-tooa-secure-cluster-non-interactively-using-azure-active-directory"></a>Připojte tooa zabezpečené cluster interaktivně pomocí služby Azure Active Directory

Hello následující příklad spoléhá na Microsoft.IdentityModel.Clients.ActiveDirectory, verze: 2.19.208020213.

Další informace o získání tokenu AAD najdete v tématu [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

```csharp
string tenantId = "C15CFCEA-02C1-40DC-8466-FBD0EE0B05D2";
string clientApplicationId = "118473C2-7619-46E3-A8E4-6DA8D5F56E12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob");

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    var authContext = new AuthenticationContext(authority);

    var authResult = authContext.AcquireToken(
        resource,
        clientId,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

### <a name="connect-tooa-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Připojte tooa zabezpečené cluster bez vědomí předchozí metadat pomocí služby Azure Active Directory

Hello následující příklad používá neinteraktivní získání tokenu, ale hello stejný postup lze použít toobuild vlastní interaktivní tokenu získávání zkušeností. Hello Azure Active Directory metadata potřebná k získání tokenu je pro čtení z konfigurace clusteru.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

fc.ClaimsRetrieval += (o, e) =>
{
    return GetAccessToken(e.AzureActiveDirectoryMetadata);
};

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(AzureActiveDirectoryMetadata aad)
{
    var authContext = new AuthenticationContext(aad.Authority);

    var authResult = authContext.AcquireToken(
        aad.ClusterApplication,
        aad.ClientApplication,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-tooa-secure-cluster-using-service-fabric-explorer"></a>Připojte tooa zabezpečené cluster pomocí Service Fabric Exploreru
tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) pro daný cluster, přejděte v prohlížeči na:

`http://<your-cluster-endpoint>:19080/Explorer`

úplnou adresu URL Hello je také dostupná v podokně essentials clusteru hello hello portálu Azure.

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a>Připojte tooa zabezpečené cluster pomocí služby Azure Active Directory

tooconnect tooa clusteru, který je bezpečně umístěný v AAD, přejděte v prohlížeči na:

`https://<your-cluster-endpoint>:19080/Explorer`

Automaticky se možné výzvami toolog pomocí AAD.

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Připojte tooa zabezpečené cluster pomocí klientského certifikátu

tooconnect tooa clusteru, která je zabezpečená pomocí certifikátů, přejděte v prohlížeči na:

`https://<your-cluster-endpoint>:19080/Explorer`

Automaticky se možné výzvami tooselect klientský certifikát.

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-hello-remote-computer"></a>Nastavení certifikátu klienta ve vzdáleném počítači hello
Alespoň dva certifikáty slouží k zabezpečení hello clusteru, jeden pro certifikát cluster a server hello a druhý pro klientský přístup.  Doporučujeme také používat další sekundární certifikátů a certifikátů přístup klienta.  toosecure hello komunikace mezi klientem a uzlu clusteru pomocí certifikátu zabezpečení, je nejprve nutné tooobtain a nainstalovat certifikát klienta hello. certifikát Hello lze nainstalovat do hello osobní (My) hello místního počítače nebo hello aktuálního uživatele.  Musíte taky hello kryptografický otisk certifikátu serveru hello, aby se hello klienta můžete ověřit hello clusteru.

Spusťte následující tooset rutiny prostředí PowerShell hello certifikátu klienta na počítači hello, ze kterého jste přístup ke clusteru hello hello.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Pokud je certifikát podepsaný svým držitelem, musíte ho tooyour počítač "důvěryhodné osoby" uložit, než budete moci použít tento zabezpečený cluster certifikát tooconnect tooa tooimport.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a>Další kroky

* [Proces upgradu Service Fabric Cluster a očekávání od vás](service-fabric-cluster-upgrade.md)
* [Správu aplikací Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Stav služby Fabric modelu Úvod](service-fabric-health-introduction.md)
* [Zabezpečení aplikací a RunAs](service-fabric-application-runas-security.md)
* [Začínáme se Service Fabric CLI](service-fabric-cli.md)
