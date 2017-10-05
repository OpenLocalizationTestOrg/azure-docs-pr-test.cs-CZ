---
title: "Zabezpečené připojení ke clusteru Azure Service Fabric | Microsoft Docs"
description: "Popisuje, jak k ověření přístupu klientů k cluster Service Fabric a postupy pro zabezpečení komunikace mezi klienty a cluster."
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
ms.openlocfilehash: d6a13ceb8ccd9207ecacc166247535d496d5dec7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="9495f-103">Připojení k zabezpečenému clusteru</span><span class="sxs-lookup"><span data-stu-id="9495f-103">Connect to a secure cluster</span></span>

<span data-ttu-id="9495f-104">Když se klient připojí k uzlu clusteru Service Fabric, klient může být ověřený a zabezpečenou komunikaci navázat pomocí certifikátů, zabezpečení nebo Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="9495f-104">When a client connects to a Service Fabric cluster node, the client can be authenticated and secure communication established using certificate security or Azure Active Directory (AAD).</span></span> <span data-ttu-id="9495f-105">Toto ověřování zajistí jenom autorizovaným uživatelům můžete přístup ke clusteru a nasazené aplikace a provádět úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="9495f-105">This authentication ensures that only authorized users can access the cluster and deployed applications and perform management tasks.</span></span>  <span data-ttu-id="9495f-106">Certifikát nebo AAD zabezpečení musí být dříve povolen v clusteru při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-106">Certificate or AAD security must have been previously enabled on the cluster when the cluster was created.</span></span>  <span data-ttu-id="9495f-107">Další informace o scénáře zabezpečení clusteru najdete v tématu [clusteru zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="9495f-107">For more information on cluster security scenarios, see [Cluster security](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="9495f-108">Pokud se připojujete ke clusteru zabezpečené pomocí certifikátů, [nastavit certifikát klienta](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) na počítači, který připojí ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-108">If you are connecting to a cluster secured with certificates, [set up the client certificate](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) on the computer that connects to the cluster.</span></span> 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="9495f-109">Připojení ke clusteru zabezpečené pomocí Azure Service Fabric rozhraní příkazového řádku (sfctl)</span><span class="sxs-lookup"><span data-stu-id="9495f-109">Connect to a secure cluster using Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="9495f-110">Pro připojení k zabezpečení clusteru Service Fabric rozhraní příkazového řádku (sfctl) pomocí několika různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="9495f-110">There are a few different ways to connect to a secure cluster using the Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="9495f-111">Pokud k ověřování používáte klientský certifikát, podrobnosti o certifikátu musí odpovídat certifikátu nasazenému do uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-111">When using a client certificate for authentication, the certificate details must match a certificate deployed to the cluster nodes.</span></span> <span data-ttu-id="9495f-112">Pokud má váš certifikát certifikační autority (CA), budete muset zadat kromě důvěryhodných certifikačních autorit.</span><span class="sxs-lookup"><span data-stu-id="9495f-112">If your certificate has Certificate Authorities (CAs), you need to additionally specify the trusted CAs.</span></span>

<span data-ttu-id="9495f-113">Připojením ke clusteru pomocí `sfctl cluster select` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9495f-113">You can connect to a cluster using the `sfctl cluster select` command.</span></span>

<span data-ttu-id="9495f-114">Klientské certifikáty lze zadat v dva různé způsoby, buď jako dvojice certifikát a klíč, nebo jako soubor pem jeden.</span><span class="sxs-lookup"><span data-stu-id="9495f-114">Client certificates can be specified in two different fashions, either as a cert and key pair, or as a single pem file.</span></span> <span data-ttu-id="9495f-115">Pro chráněné heslem `pem` soubory, zobrazí se výzva automaticky k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="9495f-115">For password protected `pem` files, you will be prompted automatically to enter the password.</span></span>

<span data-ttu-id="9495f-116">Pokud chcete zadat klientský certifikát jako soubor pem, zadejte cestu k souboru v `--pem` argument.</span><span class="sxs-lookup"><span data-stu-id="9495f-116">To specify the client certificate as a pem file, specify the file path in the `--pem` argument.</span></span> <span data-ttu-id="9495f-117">Například:</span><span class="sxs-lookup"><span data-stu-id="9495f-117">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="9495f-118">Pem, který soubory výzvou k zadání hesla před spuštěním jakékoli příkazu je chráněný heslem.</span><span class="sxs-lookup"><span data-stu-id="9495f-118">Password protected pem files will prompt for password prior to running any command.</span></span>

<span data-ttu-id="9495f-119">Pokud chcete zadat certifikát, klíče dvojice použití `--cert` a `--key` argumenty k určení cesty k souborům pro každý odpovídající soubor.</span><span class="sxs-lookup"><span data-stu-id="9495f-119">To specify a cert, key pair use the `--cert` and `--key` arguments to specify the file paths to each respective file.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

<span data-ttu-id="9495f-120">Někdy certifikátů používaných pro zabezpečenou testu nebo dev clustery nezdaří ověření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9495f-120">Sometimes certificates used to secure test or dev clusters fail certificate validation.</span></span> <span data-ttu-id="9495f-121">Obejít ověření certifikátu, zadejte `--no-verify` možnost.</span><span class="sxs-lookup"><span data-stu-id="9495f-121">To bypass certificate verification, specify the `--no-verify` option.</span></span> <span data-ttu-id="9495f-122">Například:</span><span class="sxs-lookup"><span data-stu-id="9495f-122">For example:</span></span>

> [!WARNING]
> <span data-ttu-id="9495f-123">Nepoužívejte `no-verify` možnost při připojování k produkci clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9495f-123">Do not use the `no-verify` option when connecting to production Service Fabric clusters.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

<span data-ttu-id="9495f-124">Kromě toho můžete určit cest k adresářům certifikátů důvěryhodné certifikační Autority nebo jednotlivých certifikátů.</span><span class="sxs-lookup"><span data-stu-id="9495f-124">In addition, you can specify paths to directories of trusted CA certs, or individual certs.</span></span> <span data-ttu-id="9495f-125">Pokud chcete zadat tyto cesty, použijte `--ca` argument.</span><span class="sxs-lookup"><span data-stu-id="9495f-125">To specify these paths, use the `--ca` argument.</span></span> <span data-ttu-id="9495f-126">Například:</span><span class="sxs-lookup"><span data-stu-id="9495f-126">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

<span data-ttu-id="9495f-127">Když se připojíte, byste měli moct [spouštění jiných příkazů sfctl](service-fabric-cli.md) k interakci s clusterem.</span><span class="sxs-lookup"><span data-stu-id="9495f-127">After you connect, you should be able to [run other sfctl commands](service-fabric-cli.md) to interact with the cluster.</span></span>

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a><span data-ttu-id="9495f-128">Připojení ke clusteru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9495f-128">Connect to a cluster using PowerShell</span></span>
<span data-ttu-id="9495f-129">Před provedením operace na clusteru s podporou pomocí prostředí PowerShell, nejprve vytvořit připojení ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-129">Before you perform operations on a cluster through PowerShell, first establish a connection to the cluster.</span></span> <span data-ttu-id="9495f-130">Připojení clusteru se používá pro všechny následné příkazy v dané relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9495f-130">The cluster connection is used for all subsequent commands in the given PowerShell session.</span></span>

### <a name="connect-to-an-unsecure-cluster"></a><span data-ttu-id="9495f-131">Připojení ke clusteru nezabezpečená</span><span class="sxs-lookup"><span data-stu-id="9495f-131">Connect to an unsecure cluster</span></span>

<span data-ttu-id="9495f-132">Pro připojení k nezabezpečená clusteru, zadejte adresu koncový bod clusteru k **Connect-ServiceFabricCluster** příkaz:</span><span class="sxs-lookup"><span data-stu-id="9495f-132">To connect to an unsecure cluster, provide the cluster endpoint address to the **Connect-ServiceFabricCluster** command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="9495f-133">Připojení ke clusteru zabezpečené pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9495f-133">Connect to a secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="9495f-134">Pro připojení k zabezpečení clusteru, který používá Azure Active Directory k autorizaci přístupu správce clusteru, zadejte kryptografický otisk certifikátu clusteru a použít *azureactivedirectory selhala* příznak.</span><span class="sxs-lookup"><span data-stu-id="9495f-134">To connect to a secure cluster that uses Azure Active Directory to authorize cluster administrator access, provide the cluster certificate thumbprint and use the *AzureActiveDirectory* flag.</span></span>  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="9495f-135">Připojení ke clusteru zabezpečené pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="9495f-135">Connect to a secure cluster using a client certificate</span></span>
<span data-ttu-id="9495f-136">Spusťte následující příkaz prostředí PowerShell pro připojení k zabezpečení clusteru, který používá klientské certifikáty k autorizaci přístupu správce.</span><span class="sxs-lookup"><span data-stu-id="9495f-136">Run the following PowerShell command to connect to a secure cluster that uses client certificates to authorize administrator access.</span></span> <span data-ttu-id="9495f-137">Zadejte kryptografický otisk certifikátu clusteru a kryptografický otisk certifikátu klienta, kterému byla udělena oprávnění pro správu clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-137">Provide the cluster certificate thumbprint and the thumbprint of the client certificate that has been granted permissions for cluster management.</span></span> <span data-ttu-id="9495f-138">Podrobnosti o certifikátu musí odpovídat certifikát na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-138">The certificate details must match a certificate on the cluster nodes.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="9495f-139">*ServerCertThumbprint* je kryptografický otisk certifikátu serveru nainstalované na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-139">*ServerCertThumbprint* is the thumbprint of the server certificate installed on the cluster nodes.</span></span> <span data-ttu-id="9495f-140">*FindValue* je kryptografický otisk certifikátu klienta správce.</span><span class="sxs-lookup"><span data-stu-id="9495f-140">*FindValue* is the thumbprint of the admin client certificate.</span></span>
<span data-ttu-id="9495f-141">Když jsou vyplněna parametry, vypadá příkaz v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9495f-141">When the parameters are filled in, the command looks like the following example:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a><span data-ttu-id="9495f-142">Připojení ke clusteru zabezpečené pomocí služby Windows Active Directory</span><span class="sxs-lookup"><span data-stu-id="9495f-142">Connect to a secure cluster using Windows Active Directory</span></span>
<span data-ttu-id="9495f-143">Pokud váš cluster samostatné se nasazuje pomocí zabezpečení AD, připojte se ke clusteru připojením přepínač "WindowsCredential".</span><span class="sxs-lookup"><span data-stu-id="9495f-143">If your standalone cluster is deployed using AD security, connect to the cluster by appending the switch "WindowsCredential".</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a><span data-ttu-id="9495f-144">Připojení ke clusteru pomocí rozhraní API FabricClient</span><span class="sxs-lookup"><span data-stu-id="9495f-144">Connect to a cluster using the FabricClient APIs</span></span>
<span data-ttu-id="9495f-145">Sada Service Fabric SDK poskytuje [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) třídy pro správu clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-145">The Service Fabric SDK provides the [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) class for cluster management.</span></span> <span data-ttu-id="9495f-146">Pokud chcete používat rozhraní API FabricClient, získáte balíček Microsoft.ServiceFabric NuGet.</span><span class="sxs-lookup"><span data-stu-id="9495f-146">To use the FabricClient APIs, get the Microsoft.ServiceFabric NuGet package.</span></span>

### <a name="connect-to-an-unsecure-cluster"></a><span data-ttu-id="9495f-147">Připojení ke clusteru nezabezpečená</span><span class="sxs-lookup"><span data-stu-id="9495f-147">Connect to an unsecure cluster</span></span>

<span data-ttu-id="9495f-148">Připojení ke vzdálené zabezpečená clusteru, vytvořte instanci FabricClient a zadejte adresu clusteru:</span><span class="sxs-lookup"><span data-stu-id="9495f-148">To connect to a remote unsecured cluster, create a FabricClient instance and provide the cluster address:</span></span>

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

<span data-ttu-id="9495f-149">Pro kód, který je spuštěn z v rámci clusteru, například v spolehlivě, vytvořte FabricClient *bez* zadání adresy clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-149">For code that is running from within a cluster, for example, in a Reliable Service, create a FabricClient *without* specifying the cluster address.</span></span> <span data-ttu-id="9495f-150">FabricClient připojí k bráně místní správy na uzlu, na který, aktuálně běží kód zabraňující další síti směrování.</span><span class="sxs-lookup"><span data-stu-id="9495f-150">FabricClient connects to the local management gateway on the node the code is currently running on, avoiding an extra network hop.</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="9495f-151">Připojení ke clusteru zabezpečené pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="9495f-151">Connect to a secure cluster using a client certificate</span></span>

<span data-ttu-id="9495f-152">Uzly v clusteru musí mít platné certifikáty, jejichž běžný název nebo název DNS v síti SAN se zobrazí v [RemoteCommonNames vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) nastavit na [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="9495f-152">The nodes in the cluster must have valid certificates whose common name or DNS name in SAN appears in the [RemoteCommonNames property](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) set on [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span></span> <span data-ttu-id="9495f-153">Provedení tohoto postupu se umožní vzájemné ověřování mezi klientem a uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-153">Following this process enables mutual authentication between the client and the cluster nodes.</span></span>

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

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a><span data-ttu-id="9495f-154">Připojení ke clusteru zabezpečené interaktivně pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9495f-154">Connect to a secure cluster interactively using Azure Active Directory</span></span>

<span data-ttu-id="9495f-155">Následující příklad používá Azure Active Directory pro klienta certifikát identity a server pro identitu serveru.</span><span class="sxs-lookup"><span data-stu-id="9495f-155">The following example uses Azure Active Directory for client identity and server certificate for server identity.</span></span>

<span data-ttu-id="9495f-156">Dialogové okno se automaticky objeví pro interaktivní přihlášení při připojování ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-156">A dialog window automatically pops up for interactive sign-in upon connecting to the cluster.</span></span>

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

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a><span data-ttu-id="9495f-157">Připojení ke clusteru zabezpečené interaktivně pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9495f-157">Connect to a secure cluster non-interactively using Azure Active Directory</span></span>

<span data-ttu-id="9495f-158">Následující příklad spoléhá na Microsoft.IdentityModel.Clients.ActiveDirectory, verze: 2.19.208020213.</span><span class="sxs-lookup"><span data-stu-id="9495f-158">The following example relies on Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span></span>

<span data-ttu-id="9495f-159">Další informace o získání tokenu AAD najdete v tématu [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span><span class="sxs-lookup"><span data-stu-id="9495f-159">For more information on AAD token acquisition, see [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span></span>

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

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a><span data-ttu-id="9495f-160">Připojení ke clusteru zabezpečené bez vědomí předchozí metadat pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9495f-160">Connect to a secure cluster without prior metadata knowledge using Azure Active Directory</span></span>

<span data-ttu-id="9495f-161">Následující příklad používá neinteraktivní získání tokenu, ale ve stejný přístup lze použít k vytvoření vlastní interaktivní tokenu získávání zkušeností.</span><span class="sxs-lookup"><span data-stu-id="9495f-161">The following example uses non-interactive token acquisition, but the same approach can be used to build a custom interactive token acquisition experience.</span></span> <span data-ttu-id="9495f-162">Azure Active Directory metadata potřebná pro získání tokenu je pro čtení z konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-162">The Azure Active Directory metadata needed for token acquisition is read from cluster configuration.</span></span>

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

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a><span data-ttu-id="9495f-163">Připojení k zabezpečení clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="9495f-163">Connect to a secure cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="9495f-164">K dosažení [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) pro daný cluster, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="9495f-164">To reach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) for a given cluster, point your browser to:</span></span>

`http://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="9495f-165">Úplná adresa URL je také dostupná v podokně clusteru essentials na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9495f-165">The full URL is also available in the cluster essentials pane of the Azure portal.</span></span>

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="9495f-166">Připojení ke clusteru zabezpečené pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9495f-166">Connect to a secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="9495f-167">Chcete-li připojit ke clusteru, který je zabezpečen AAD, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="9495f-167">To connect to a cluster that is secured with AAD, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="9495f-168">Automaticky zobrazí se výzva k přihlášení pomocí AAD.</span><span class="sxs-lookup"><span data-stu-id="9495f-168">You are automatically be prompted to log in with AAD.</span></span>

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="9495f-169">Připojení ke clusteru zabezpečené pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="9495f-169">Connect to a secure cluster using a client certificate</span></span>

<span data-ttu-id="9495f-170">Chcete-li připojit ke clusteru, který je zabezpečen pomocí certifikátů, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="9495f-170">To connect to a cluster that is secured with certificates, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="9495f-171">Automaticky zobrazí se výzva k výběru certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="9495f-171">You are automatically be prompted to select a client certificate.</span></span>

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-the-remote-computer"></a><span data-ttu-id="9495f-172">Nastavit klientský certifikát na vzdáleném počítači</span><span class="sxs-lookup"><span data-stu-id="9495f-172">Set up a client certificate on the remote computer</span></span>
<span data-ttu-id="9495f-173">Alespoň dva certifikáty slouží k zabezpečení clusteru, jeden pro certifikát cluster a server a druhý pro klientský přístup.</span><span class="sxs-lookup"><span data-stu-id="9495f-173">At least two certificates should be used for securing the cluster, one for the cluster and server certificate and another for client access.</span></span>  <span data-ttu-id="9495f-174">Doporučujeme také používat další sekundární certifikátů a certifikátů přístup klienta.</span><span class="sxs-lookup"><span data-stu-id="9495f-174">We recommend that you also use additional secondary certificates and client access certificates.</span></span>  <span data-ttu-id="9495f-175">K zabezpečení komunikace mezi klientem a uzlem clusteru, používá certifikát zabezpečení, musíte nejprve získat a nainstalovat certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="9495f-175">To secure the communication between a client and a cluster node using certificate security, you first need to obtain and install the client certificate.</span></span> <span data-ttu-id="9495f-176">Lze nainstalovat certifikát do osobního (My) úložiště místního počítače nebo aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="9495f-176">The certificate can be installed into the Personal (My) store of the local computer or the current user.</span></span>  <span data-ttu-id="9495f-177">Musíte taky kryptografický otisk certifikátu serveru, takže klient může ověřit clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-177">You also need the thumbprint of the server certificate so that the client can authenticate the cluster.</span></span>

<span data-ttu-id="9495f-178">Spuštěním následující rutiny prostředí PowerShell nastavit klientský certifikát na počítači, ze kterého jste přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="9495f-178">Run the following PowerShell cmdlet to set up the client certificate on the computer from which you access the cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

<span data-ttu-id="9495f-179">Pokud je certifikát podepsaný svým držitelem, musíte ho importovat do úložiště "důvěryhodné osoby" váš počítač před použitím tohoto certifikátu pro připojení do clusteru s podporou zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9495f-179">If it is a self-signed certificate, you need to import it to your machine's "trusted people" store before you can use this certificate to connect to a secure cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a><span data-ttu-id="9495f-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9495f-180">Next steps</span></span>

* [<span data-ttu-id="9495f-181">Proces upgradu Service Fabric Cluster a očekávání od vás</span><span class="sxs-lookup"><span data-stu-id="9495f-181">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="9495f-182">Správu aplikací Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9495f-182">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="9495f-183">Stav služby Fabric modelu Úvod</span><span class="sxs-lookup"><span data-stu-id="9495f-183">Service Fabric Health model introduction</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="9495f-184">Zabezpečení aplikací a RunAs</span><span class="sxs-lookup"><span data-stu-id="9495f-184">Application Security and RunAs</span></span>](service-fabric-application-runas-security.md)
* [<span data-ttu-id="9495f-185">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="9495f-185">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
