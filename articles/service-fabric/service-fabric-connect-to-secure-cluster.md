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
# <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="d4961-103">Připojte tooa zabezpečené cluster</span><span class="sxs-lookup"><span data-stu-id="d4961-103">Connect tooa secure cluster</span></span>

<span data-ttu-id="d4961-104">Když se klient připojí tooa uzlu clusteru Service Fabric, hello klienta může být ověřený a zabezpečenou komunikaci navázat pomocí certifikátů, zabezpečení nebo Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="d4961-104">When a client connects tooa Service Fabric cluster node, hello client can be authenticated and secure communication established using certificate security or Azure Active Directory (AAD).</span></span> <span data-ttu-id="d4961-105">Toto ověřování zajistí jenom autorizovaným uživatelům můžete přístup ke clusteru hello a nasazené aplikace a provádět úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="d4961-105">This authentication ensures that only authorized users can access hello cluster and deployed applications and perform management tasks.</span></span>  <span data-ttu-id="d4961-106">Certifikát nebo AAD zabezpečení musí být dříve povolen v clusteru hello při vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-106">Certificate or AAD security must have been previously enabled on hello cluster when hello cluster was created.</span></span>  <span data-ttu-id="d4961-107">Další informace o scénáře zabezpečení clusteru najdete v tématu [clusteru zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="d4961-107">For more information on cluster security scenarios, see [Cluster security](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="d4961-108">Pokud se připojujete tooa clusteru zabezpečené pomocí certifikátů, [nastavit hello klientský certifikát](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) hello počítače, který připojí toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-108">If you are connecting tooa cluster secured with certificates, [set up hello client certificate](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) on hello computer that connects toohello cluster.</span></span> 

<a id="connectsecureclustercli"></a> 

## <a name="connect-tooa-secure-cluster-using-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="d4961-109">Připojte tooa zabezpečené cluster pomocí Azure Service Fabric rozhraní příkazového řádku (sfctl)</span><span class="sxs-lookup"><span data-stu-id="d4961-109">Connect tooa secure cluster using Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="d4961-110">Existuje několik různých způsobů tooconnect tooa zabezpečení clusteru pomocí hello Service Fabric rozhraní příkazového řádku (sfctl).</span><span class="sxs-lookup"><span data-stu-id="d4961-110">There are a few different ways tooconnect tooa secure cluster using hello Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="d4961-111">Při ověřování pomocí certifikátu klienta, certifikát hello podrobnosti musí shodovat se certifikát nasadí toohello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-111">When using a client certificate for authentication, hello certificate details must match a certificate deployed toohello cluster nodes.</span></span> <span data-ttu-id="d4961-112">Pokud má váš certifikát certifikační autority (CA), je třeba tooadditionally zadejte hello důvěryhodné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="d4961-112">If your certificate has Certificate Authorities (CAs), you need tooadditionally specify hello trusted CAs.</span></span>

<span data-ttu-id="d4961-113">Můžete připojit pomocí hello clusteru tooa `sfctl cluster select` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d4961-113">You can connect tooa cluster using hello `sfctl cluster select` command.</span></span>

<span data-ttu-id="d4961-114">Klientské certifikáty lze zadat v dva různé způsoby, buď jako dvojice certifikát a klíč, nebo jako soubor pem jeden.</span><span class="sxs-lookup"><span data-stu-id="d4961-114">Client certificates can be specified in two different fashions, either as a cert and key pair, or as a single pem file.</span></span> <span data-ttu-id="d4961-115">Pro chráněné heslem `pem` budou soubory, výzva automaticky tooenter hello heslo.</span><span class="sxs-lookup"><span data-stu-id="d4961-115">For password protected `pem` files, you will be prompted automatically tooenter hello password.</span></span>

<span data-ttu-id="d4961-116">toospecify hello klientský certifikát jako soubor pem, zadejte cestu k souboru hello v hello `--pem` argument.</span><span class="sxs-lookup"><span data-stu-id="d4961-116">toospecify hello client certificate as a pem file, specify hello file path in hello `--pem` argument.</span></span> <span data-ttu-id="d4961-117">Například:</span><span class="sxs-lookup"><span data-stu-id="d4961-117">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="d4961-118">Soubory pem chráněné heslem zobrazí výzvu k předchozí toorunning heslo libovolný příkaz.</span><span class="sxs-lookup"><span data-stu-id="d4961-118">Password protected pem files will prompt for password prior toorunning any command.</span></span>

<span data-ttu-id="d4961-119">toospecify certifikát pár klíčů použijte hello `--cert` a `--key` argumenty toospecify hello odpovídající soubor tooeach cest souborů.</span><span class="sxs-lookup"><span data-stu-id="d4961-119">toospecify a cert, key pair use hello `--cert` and `--key` arguments toospecify hello file paths tooeach respective file.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

<span data-ttu-id="d4961-120">Někdy certifikáty používají toosecure testu nebo clustery dev nezdaří ověření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d4961-120">Sometimes certificates used toosecure test or dev clusters fail certificate validation.</span></span> <span data-ttu-id="d4961-121">toobypass certifikát ověřování, zadejte hello `--no-verify` možnost.</span><span class="sxs-lookup"><span data-stu-id="d4961-121">toobypass certificate verification, specify hello `--no-verify` option.</span></span> <span data-ttu-id="d4961-122">Například:</span><span class="sxs-lookup"><span data-stu-id="d4961-122">For example:</span></span>

> [!WARNING]
> <span data-ttu-id="d4961-123">Nepoužívejte hello `no-verify` možnost při připojování clusterů Service Fabric tooproduction.</span><span class="sxs-lookup"><span data-stu-id="d4961-123">Do not use hello `no-verify` option when connecting tooproduction Service Fabric clusters.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

<span data-ttu-id="d4961-124">Kromě toho můžete zadat cesty toodirectories certifikátů důvěryhodné certifikační Autority nebo jednotlivých certifikátů.</span><span class="sxs-lookup"><span data-stu-id="d4961-124">In addition, you can specify paths toodirectories of trusted CA certs, or individual certs.</span></span> <span data-ttu-id="d4961-125">toospecify tyto cesty použít hello `--ca` argument.</span><span class="sxs-lookup"><span data-stu-id="d4961-125">toospecify these paths, use hello `--ca` argument.</span></span> <span data-ttu-id="d4961-126">Například:</span><span class="sxs-lookup"><span data-stu-id="d4961-126">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

<span data-ttu-id="d4961-127">Když se připojíte, měli byste mít příliš[spouštění jiných příkazů sfctl](service-fabric-cli.md) toointeract s clusterem hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-127">After you connect, you should be able too[run other sfctl commands](service-fabric-cli.md) toointeract with hello cluster.</span></span>

<a id="connectsecurecluster"></a>

## <a name="connect-tooa-cluster-using-powershell"></a><span data-ttu-id="d4961-128">Připojte tooa cluster pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4961-128">Connect tooa cluster using PowerShell</span></span>
<span data-ttu-id="d4961-129">Před provedením operace na clusteru s podporou pomocí prostředí PowerShell, nejprve vytvořit cluster toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="d4961-129">Before you perform operations on a cluster through PowerShell, first establish a connection toohello cluster.</span></span> <span data-ttu-id="d4961-130">připojení clusteru Hello se používá pro všechny následné příkazy v hello danou relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d4961-130">hello cluster connection is used for all subsequent commands in hello given PowerShell session.</span></span>

### <a name="connect-tooan-unsecure-cluster"></a><span data-ttu-id="d4961-131">Připojte tooan nezabezpečená cluster</span><span class="sxs-lookup"><span data-stu-id="d4961-131">Connect tooan unsecure cluster</span></span>

<span data-ttu-id="d4961-132">Nezabezpečená tooconnect tooan clusteru, zadejte hello clusteru koncový bod adresy toohello **Connect-ServiceFabricCluster** příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4961-132">tooconnect tooan unsecure cluster, provide hello cluster endpoint address toohello **Connect-ServiceFabricCluster** command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="d4961-133">Připojte tooa zabezpečené cluster pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4961-133">Connect tooa secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="d4961-134">tooa tooconnect zabezpečené se cluster, který používá Azure Active Directory tooauthorize správce přístup ke clusteru, zadejte kryptografický otisk certifikátu hello clusteru a použít hello *azureactivedirectory selhala* příznak.</span><span class="sxs-lookup"><span data-stu-id="d4961-134">tooconnect tooa secure cluster that uses Azure Active Directory tooauthorize cluster administrator access, provide hello cluster certificate thumbprint and use hello *AzureActiveDirectory* flag.</span></span>  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="d4961-135">Připojte tooa zabezpečené cluster pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="d4961-135">Connect tooa secure cluster using a client certificate</span></span>
<span data-ttu-id="d4961-136">Spuštění hello následující příkaz prostředí PowerShell tooconnect tooa zabezpečení clusteru, který používá přístup správce tooauthorize certifikáty klienta.</span><span class="sxs-lookup"><span data-stu-id="d4961-136">Run hello following PowerShell command tooconnect tooa secure cluster that uses client certificates tooauthorize administrator access.</span></span> <span data-ttu-id="d4961-137">Zadejte kryptografický otisk certifikátu hello clusteru a hello kryptografický otisk hello klientský certifikát, kterému byla udělena oprávnění pro správu clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-137">Provide hello cluster certificate thumbprint and hello thumbprint of hello client certificate that has been granted permissions for cluster management.</span></span> <span data-ttu-id="d4961-138">Podrobnosti o Hello certifikátu musí odpovídat certifikát na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-138">hello certificate details must match a certificate on hello cluster nodes.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="d4961-139">*ServerCertThumbprint* je hello kryptografický otisk certifikátu serveru hello nainstalované na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-139">*ServerCertThumbprint* is hello thumbprint of hello server certificate installed on hello cluster nodes.</span></span> <span data-ttu-id="d4961-140">*FindValue* je hello kryptografický otisk certifikátu klienta správce hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-140">*FindValue* is hello thumbprint of hello admin client certificate.</span></span>
<span data-ttu-id="d4961-141">Když jsou vyplněna hello parametry, hello příkaz vypadá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d4961-141">When hello parameters are filled in, hello command looks like hello following example:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-tooa-secure-cluster-using-windows-active-directory"></a><span data-ttu-id="d4961-142">Připojte tooa zabezpečené cluster pomocí služby Windows Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4961-142">Connect tooa secure cluster using Windows Active Directory</span></span>
<span data-ttu-id="d4961-143">Pokud váš cluster samostatné se nasazuje pomocí zabezpečení AD, připojte toohello cluster připojením hello přepínač "WindowsCredential".</span><span class="sxs-lookup"><span data-stu-id="d4961-143">If your standalone cluster is deployed using AD security, connect toohello cluster by appending hello switch "WindowsCredential".</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-tooa-cluster-using-hello-fabricclient-apis"></a><span data-ttu-id="d4961-144">Připojte tooa cluster pomocí hello FabricClient rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d4961-144">Connect tooa cluster using hello FabricClient APIs</span></span>
<span data-ttu-id="d4961-145">Hello Service Fabric SDK poskytuje hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) třídy pro správu clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-145">hello Service Fabric SDK provides hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) class for cluster management.</span></span> <span data-ttu-id="d4961-146">hello toouse FabricClient rozhraní API, získat hello balíček Microsoft.ServiceFabric NuGet.</span><span class="sxs-lookup"><span data-stu-id="d4961-146">toouse hello FabricClient APIs, get hello Microsoft.ServiceFabric NuGet package.</span></span>

### <a name="connect-tooan-unsecure-cluster"></a><span data-ttu-id="d4961-147">Připojte tooan nezabezpečená cluster</span><span class="sxs-lookup"><span data-stu-id="d4961-147">Connect tooan unsecure cluster</span></span>

<span data-ttu-id="d4961-148">tooconnect tooa vzdálený zabezpečená cluster vytvořit instanci FabricClient a zadejte adresu clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="d4961-148">tooconnect tooa remote unsecured cluster, create a FabricClient instance and provide hello cluster address:</span></span>

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

<span data-ttu-id="d4961-149">Pro kód, který je spuštěn z v rámci clusteru, například v spolehlivě, vytvořte FabricClient *bez* zadání adresy clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-149">For code that is running from within a cluster, for example, in a Reliable Service, create a FabricClient *without* specifying hello cluster address.</span></span> <span data-ttu-id="d4961-150">FabricClient připojí toohello místní správy brány na kód hello hello uzlu na aktuálně probíhá, zabraňující další síti směrování.</span><span class="sxs-lookup"><span data-stu-id="d4961-150">FabricClient connects toohello local management gateway on hello node hello code is currently running on, avoiding an extra network hop.</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="d4961-151">Připojte tooa zabezpečené cluster pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="d4961-151">Connect tooa secure cluster using a client certificate</span></span>

<span data-ttu-id="d4961-152">Hello uzly v clusteru hello musí mít platné certifikáty, jejichž běžný název nebo název DNS v síti SAN se zobrazí v hello [RemoteCommonNames vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) nastavit na [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="d4961-152">hello nodes in hello cluster must have valid certificates whose common name or DNS name in SAN appears in hello [RemoteCommonNames property](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) set on [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span></span> <span data-ttu-id="d4961-153">Provedení tohoto postupu se umožní vzájemné ověřování mezi klientem hello a hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-153">Following this process enables mutual authentication between hello client and hello cluster nodes.</span></span>

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

### <a name="connect-tooa-secure-cluster-interactively-using-azure-active-directory"></a><span data-ttu-id="d4961-154">Připojte tooa zabezpečené cluster interaktivně pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4961-154">Connect tooa secure cluster interactively using Azure Active Directory</span></span>

<span data-ttu-id="d4961-155">Následující příklad používá Azure Active Directory pro klienta certifikát identity a server pro identitu serveru Hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-155">hello following example uses Azure Active Directory for client identity and server certificate for server identity.</span></span>

<span data-ttu-id="d4961-156">Dialogové okno se automaticky objeví pro interaktivní přihlášení při připojování toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-156">A dialog window automatically pops up for interactive sign-in upon connecting toohello cluster.</span></span>

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

### <a name="connect-tooa-secure-cluster-non-interactively-using-azure-active-directory"></a><span data-ttu-id="d4961-157">Připojte tooa zabezpečené cluster interaktivně pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4961-157">Connect tooa secure cluster non-interactively using Azure Active Directory</span></span>

<span data-ttu-id="d4961-158">Hello následující příklad spoléhá na Microsoft.IdentityModel.Clients.ActiveDirectory, verze: 2.19.208020213.</span><span class="sxs-lookup"><span data-stu-id="d4961-158">hello following example relies on Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span></span>

<span data-ttu-id="d4961-159">Další informace o získání tokenu AAD najdete v tématu [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span><span class="sxs-lookup"><span data-stu-id="d4961-159">For more information on AAD token acquisition, see [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span></span>

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

### <a name="connect-tooa-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a><span data-ttu-id="d4961-160">Připojte tooa zabezpečené cluster bez vědomí předchozí metadat pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4961-160">Connect tooa secure cluster without prior metadata knowledge using Azure Active Directory</span></span>

<span data-ttu-id="d4961-161">Hello následující příklad používá neinteraktivní získání tokenu, ale hello stejný postup lze použít toobuild vlastní interaktivní tokenu získávání zkušeností.</span><span class="sxs-lookup"><span data-stu-id="d4961-161">hello following example uses non-interactive token acquisition, but hello same approach can be used toobuild a custom interactive token acquisition experience.</span></span> <span data-ttu-id="d4961-162">Hello Azure Active Directory metadata potřebná k získání tokenu je pro čtení z konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-162">hello Azure Active Directory metadata needed for token acquisition is read from cluster configuration.</span></span>

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

## <a name="connect-tooa-secure-cluster-using-service-fabric-explorer"></a><span data-ttu-id="d4961-163">Připojte tooa zabezpečené cluster pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="d4961-163">Connect tooa secure cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="d4961-164">tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) pro daný cluster, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="d4961-164">tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) for a given cluster, point your browser to:</span></span>

`http://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="d4961-165">úplnou adresu URL Hello je také dostupná v podokně essentials clusteru hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4961-165">hello full URL is also available in hello cluster essentials pane of hello Azure portal.</span></span>

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="d4961-166">Připojte tooa zabezpečené cluster pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4961-166">Connect tooa secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="d4961-167">tooconnect tooa clusteru, který je bezpečně umístěný v AAD, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="d4961-167">tooconnect tooa cluster that is secured with AAD, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="d4961-168">Automaticky se možné výzvami toolog pomocí AAD.</span><span class="sxs-lookup"><span data-stu-id="d4961-168">You are automatically be prompted toolog in with AAD.</span></span>

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="d4961-169">Připojte tooa zabezpečené cluster pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="d4961-169">Connect tooa secure cluster using a client certificate</span></span>

<span data-ttu-id="d4961-170">tooconnect tooa clusteru, která je zabezpečená pomocí certifikátů, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="d4961-170">tooconnect tooa cluster that is secured with certificates, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="d4961-171">Automaticky se možné výzvami tooselect klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="d4961-171">You are automatically be prompted tooselect a client certificate.</span></span>

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-hello-remote-computer"></a><span data-ttu-id="d4961-172">Nastavení certifikátu klienta ve vzdáleném počítači hello</span><span class="sxs-lookup"><span data-stu-id="d4961-172">Set up a client certificate on hello remote computer</span></span>
<span data-ttu-id="d4961-173">Alespoň dva certifikáty slouží k zabezpečení hello clusteru, jeden pro certifikát cluster a server hello a druhý pro klientský přístup.</span><span class="sxs-lookup"><span data-stu-id="d4961-173">At least two certificates should be used for securing hello cluster, one for hello cluster and server certificate and another for client access.</span></span>  <span data-ttu-id="d4961-174">Doporučujeme také používat další sekundární certifikátů a certifikátů přístup klienta.</span><span class="sxs-lookup"><span data-stu-id="d4961-174">We recommend that you also use additional secondary certificates and client access certificates.</span></span>  <span data-ttu-id="d4961-175">toosecure hello komunikace mezi klientem a uzlu clusteru pomocí certifikátu zabezpečení, je nejprve nutné tooobtain a nainstalovat certifikát klienta hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-175">toosecure hello communication between a client and a cluster node using certificate security, you first need tooobtain and install hello client certificate.</span></span> <span data-ttu-id="d4961-176">certifikát Hello lze nainstalovat do hello osobní (My) hello místního počítače nebo hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="d4961-176">hello certificate can be installed into hello Personal (My) store of hello local computer or hello current user.</span></span>  <span data-ttu-id="d4961-177">Musíte taky hello kryptografický otisk certifikátu serveru hello, aby se hello klienta můžete ověřit hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d4961-177">You also need hello thumbprint of hello server certificate so that hello client can authenticate hello cluster.</span></span>

<span data-ttu-id="d4961-178">Spusťte následující tooset rutiny prostředí PowerShell hello certifikátu klienta na počítači hello, ze kterého jste přístup ke clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="d4961-178">Run hello following PowerShell cmdlet tooset up hello client certificate on hello computer from which you access hello cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

<span data-ttu-id="d4961-179">Pokud je certifikát podepsaný svým držitelem, musíte ho tooyour počítač "důvěryhodné osoby" uložit, než budete moci použít tento zabezpečený cluster certifikát tooconnect tooa tooimport.</span><span class="sxs-lookup"><span data-stu-id="d4961-179">If it is a self-signed certificate, you need tooimport it tooyour machine's "trusted people" store before you can use this certificate tooconnect tooa secure cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a><span data-ttu-id="d4961-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4961-180">Next steps</span></span>

* [<span data-ttu-id="d4961-181">Proces upgradu Service Fabric Cluster a očekávání od vás</span><span class="sxs-lookup"><span data-stu-id="d4961-181">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="d4961-182">Správu aplikací Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4961-182">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="d4961-183">Stav služby Fabric modelu Úvod</span><span class="sxs-lookup"><span data-stu-id="d4961-183">Service Fabric Health model introduction</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="d4961-184">Zabezpečení aplikací a RunAs</span><span class="sxs-lookup"><span data-stu-id="d4961-184">Application Security and RunAs</span></span>](service-fabric-application-runas-security.md)
* [<span data-ttu-id="d4961-185">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="d4961-185">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
