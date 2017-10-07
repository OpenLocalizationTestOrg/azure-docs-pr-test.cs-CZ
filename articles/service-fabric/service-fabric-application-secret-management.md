---
title: "aaaManaging tajných klíčů v Service Fabric aplikace | Microsoft Docs"
description: "Tento článek popisuje, jak tajný klíč toosecure hodnoty v aplikaci Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: b8cafcb681d95aaa1b8e9a1afaac78ba5b7f58b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a><span data-ttu-id="55230-103">Správa tajných klíčů v aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="55230-103">Managing secrets in Service Fabric applications</span></span>
<span data-ttu-id="55230-104">Tento průvodce vás provede kroky hello správy tajných klíčů v aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="55230-104">This guide walks you through hello steps of managing secrets in a Service Fabric application.</span></span> <span data-ttu-id="55230-105">Tajné klíče může být žádné citlivé informace, jako je například úložiště připojovací řetězce, hesla nebo jiné hodnoty, které by neměly být zpracovány v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="55230-105">Secrets can be any sensitive information, such as storage connection strings, passwords, or other values that should not be handled in plain text.</span></span>

<span data-ttu-id="55230-106">Tato příručka používá Azure Key Vault toomanage klíče a tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="55230-106">This guide uses Azure Key Vault toomanage keys and secrets.</span></span> <span data-ttu-id="55230-107">Ale *pomocí* tajných klíčů v aplikaci je cloudové bez ohledu na platformu tooallow aplikace toobe nasazené tooa clusteru hostované odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="55230-107">However, *using* secrets in an application is cloud platform-agnostic tooallow applications toobe deployed tooa cluster hosted anywhere.</span></span> 

## <a name="overview"></a><span data-ttu-id="55230-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="55230-108">Overview</span></span>
<span data-ttu-id="55230-109">Hello nastavení konfigurace služby toomanage doporučeným způsobem je prostřednictvím [služby balíčky konfigurace][config-package].</span><span class="sxs-lookup"><span data-stu-id="55230-109">hello recommended way toomanage service configuration settings is through [service configuration packages][config-package].</span></span> <span data-ttu-id="55230-110">Konfigurace balíčků jsou verzí a aktualizovat prostřednictvím spravované postupné upgrady s vyhodnocení stavu a automatického vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="55230-110">Configuration packages are versioned and updatable through managed rolling upgrades with health-validation and auto rollback.</span></span> <span data-ttu-id="55230-111">Toto je upřednostňovaný tooglobal konfigurace, jako snižuje pravděpodobnost hello výpadkem globální služby.</span><span class="sxs-lookup"><span data-stu-id="55230-111">This is preferred tooglobal configuration as it reduces hello chances of a global service outage.</span></span> <span data-ttu-id="55230-112">Šifrované tajné klíče jsou žádná výjimka.</span><span class="sxs-lookup"><span data-stu-id="55230-112">Encrypted secrets are no exception.</span></span> <span data-ttu-id="55230-113">Service Fabric obsahuje integrované funkce pro šifrování a dešifrování hodnot v konfiguračním souboru souborech Settings.xml balíček pomocí šifrování certifikátu.</span><span class="sxs-lookup"><span data-stu-id="55230-113">Service Fabric has built-in features for encrypting and decrypting values in a configuration package Settings.xml file using certificate encryption.</span></span>

<span data-ttu-id="55230-114">Hello následující diagram znázorňuje hello základní postup pro tajný správy v aplikaci Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="55230-114">hello following diagram illustrates hello basic flow for secret management in a Service Fabric application:</span></span>

![Přehled tajný správy][overview]

<span data-ttu-id="55230-116">V tomto toku existují čtyři hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="55230-116">There are four main steps in this flow:</span></span>

1. <span data-ttu-id="55230-117">Získejte certifikát dat šifrování.</span><span class="sxs-lookup"><span data-stu-id="55230-117">Obtain a data encipherment certificate.</span></span>
2. <span data-ttu-id="55230-118">Nainstalujte certifikát hello v clusteru.</span><span class="sxs-lookup"><span data-stu-id="55230-118">Install hello certificate in your cluster.</span></span>
3. <span data-ttu-id="55230-119">Šifrování tajný hodnoty při nasazení aplikace s hello certifikátu a vložit je do služby souborech Settings.xml konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="55230-119">Encrypt secret values when deploying an application with hello certificate and inject them into a service's Settings.xml configuration file.</span></span>
4. <span data-ttu-id="55230-120">Čtení šifrované hodnoty mimo souborech Settings.xml dešifrováním s hello stejný certifikát pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="55230-120">Read encrypted values out of Settings.xml by decrypting with hello same encipherment certificate.</span></span> 

<span data-ttu-id="55230-121">[Azure Key Vault] [ key-vault-get-started] zde slouží jako umístění úložiště bezpečné pro certifikáty a jako způsob tooget certifikátů nainstalovaných na clusterů Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="55230-121">[Azure Key Vault][key-vault-get-started] is used here as a safe storage location for certificates and as a way tooget certificates installed on Service Fabric clusters in Azure.</span></span> <span data-ttu-id="55230-122">Pokud nejsou nasazení tooAzure, není nutné toouse Key Vault toomanage tajných klíčů v Service Fabric aplikace.</span><span class="sxs-lookup"><span data-stu-id="55230-122">If you are not deploying tooAzure, you do not need toouse Key Vault toomanage secrets in Service Fabric applications.</span></span>

## <a name="data-encipherment-certificate"></a><span data-ttu-id="55230-123">Certifikát pro šifrování dat</span><span class="sxs-lookup"><span data-stu-id="55230-123">Data encipherment certificate</span></span>
<span data-ttu-id="55230-124">Certifikát šifrování dat se používají výhradně pro šifrování a dešifrování konfigurace hodnoty v souborech Settings.xml služby a není používá pro ověřování nebo podpisový šifrovací textu.</span><span class="sxs-lookup"><span data-stu-id="55230-124">A data encipherment certificate is used strictly for encryption and decryption of configuration values in a service's Settings.xml and is not used for authentication or signing of cipher text.</span></span> <span data-ttu-id="55230-125">certifikát Hello musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="55230-125">hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="55230-126">Hello certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="55230-126">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="55230-127">Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="55230-127">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="55230-128">použití klíče certifikátu Hello musí zahrnovat šifrování dat (10) a nesmí obsahovat Server ověřování nebo ověřování klientů.</span><span class="sxs-lookup"><span data-stu-id="55230-128">hello certificate key usage must include Data Encipherment (10), and should not include Server Authentication or Client Authentication.</span></span> 
  
  <span data-ttu-id="55230-129">Například při vytváření certifikát podepsaný svým držitelem pomocí prostředí PowerShell, hello `KeyUsage` příliš musí být nastaven příznak`DataEncipherment`:</span><span class="sxs-lookup"><span data-stu-id="55230-129">For example, when creating a self-signed certificate using PowerShell, hello `KeyUsage` flag must be set too`DataEncipherment`:</span></span>
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-hello-certificate-in-your-cluster"></a><span data-ttu-id="55230-130">Nainstalujte certifikát hello v clusteru</span><span class="sxs-lookup"><span data-stu-id="55230-130">Install hello certificate in your cluster</span></span>
<span data-ttu-id="55230-131">Tento certifikát musí být nainstalován na každém uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="55230-131">This certificate must be installed on each node in hello cluster.</span></span> <span data-ttu-id="55230-132">Použije se v modulu runtime toodecrypt hodnotami uloženými v souborech Settings.xml služby.</span><span class="sxs-lookup"><span data-stu-id="55230-132">It will be used at runtime toodecrypt values stored in a service's Settings.xml.</span></span> <span data-ttu-id="55230-133">V tématu [jak toocreate clusteru pomocí Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] pokyny pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="55230-133">See [how toocreate a cluster using Azure Resource Manager][service-fabric-cluster-creation-via-arm] for setup instructions.</span></span> 

## <a name="encrypt-application-secrets"></a><span data-ttu-id="55230-134">Šifrování tajné klíče aplikace</span><span class="sxs-lookup"><span data-stu-id="55230-134">Encrypt application secrets</span></span>
<span data-ttu-id="55230-135">Hello Service Fabric SDK obsahuje vestavěné tajný šifrování a dešifrování funkce.</span><span class="sxs-lookup"><span data-stu-id="55230-135">hello Service Fabric SDK has built-in secret encryption and decryption functions.</span></span> <span data-ttu-id="55230-136">Tajný hodnoty může být v vytvořené čas zašifrované dešifrovat a čtení prostřednictvím kódu programu v kódu služby.</span><span class="sxs-lookup"><span data-stu-id="55230-136">Secret values can be encrypted at built-time and then decrypted and read programmatically in service code.</span></span> 

<span data-ttu-id="55230-137">Následující příkaz prostředí PowerShell Hello je použité tooencrypt tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="55230-137">hello following PowerShell command is used tooencrypt a secret.</span></span> <span data-ttu-id="55230-138">Tento příkaz šifruje pouze hodnota hello; provede **není** přihlásit hello šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="55230-138">This command only encrypts hello value; it does **not** sign hello cipher text.</span></span> <span data-ttu-id="55230-139">Hello je nutné použít stejný certifikát šifrování, které jsou nainstalovány ve vaší ciphertext tooproduce clusteru pro tajný hodnoty:</span><span class="sxs-lookup"><span data-stu-id="55230-139">You must use hello same encipherment certificate that is installed in your cluster tooproduce ciphertext for secret values:</span></span>

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="55230-140">Hello výsledný řetězec kódování base-64 obsahuje jak hello tajný šifrovaný text a také informace o hello certifikátu, který byl použité tooencrypt ho.</span><span class="sxs-lookup"><span data-stu-id="55230-140">hello resulting base-64 string contains both hello secret ciphertext as well as information about hello certificate that was used tooencrypt it.</span></span>  <span data-ttu-id="55230-141">Hello řetězec s kódováním base-64 lze vložit do parametru v konfiguračním souboru na souborech Settings.xml vaší služby s hello `IsEncrypted` atribut nastaven příliš`true`:</span><span class="sxs-lookup"><span data-stu-id="55230-141">hello base-64 encoded string can be inserted into a parameter in your service's Settings.xml configuration file with hello `IsEncrypted` attribute set too`true`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a><span data-ttu-id="55230-142">Vložit tajné klíče aplikace do instance aplikace</span><span class="sxs-lookup"><span data-stu-id="55230-142">Inject application secrets into application instances</span></span>
<span data-ttu-id="55230-143">V ideálním případě by měl být nasazení toodifferent prostředí jako automatizované nejblíže.</span><span class="sxs-lookup"><span data-stu-id="55230-143">Ideally, deployment toodifferent environments should be as automated as possible.</span></span> <span data-ttu-id="55230-144">To můžete provést provádění tajný šifrování v prostředí pro sestavování a poskytnutím hello šifrované tajné klíče jako parametry, při vytváření instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="55230-144">This can be accomplished by performing secret encryption in a build environment and providing hello encrypted secrets as parameters when creating application instances.</span></span>

#### <a name="use-overridable-parameters-in-settingsxml"></a><span data-ttu-id="55230-145">Použití přepsatelnými parametry v souborech Settings.xml</span><span class="sxs-lookup"><span data-stu-id="55230-145">Use overridable parameters in Settings.xml</span></span>
<span data-ttu-id="55230-146">konfigurační soubor Hello souborech Settings.xml umožňuje přepsatelnými parametry, které lze zadat v okamžiku vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="55230-146">hello Settings.xml configuration file allows overridable parameters that can be provided at application creation time.</span></span> <span data-ttu-id="55230-147">Použití hello `MustOverride` atribut místo hodnotu pro parametr:</span><span class="sxs-lookup"><span data-stu-id="55230-147">Use hello `MustOverride` attribute instead of providing a value for a parameter:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

<span data-ttu-id="55230-148">toooverride hodnoty v souborech Settings.xml, deklarovat parametrem přepsání pro službu hello v ApplicationManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="55230-148">toooverride values in Settings.xml, declare an override parameter for hello service in ApplicationManifest.xml:</span></span>

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

<span data-ttu-id="55230-149">Teď hello hodnotu lze zadat jako *aplikace parametr* při vytváření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="55230-149">Now hello value can be specified as an *application parameter* when creating an instance of hello application.</span></span> <span data-ttu-id="55230-150">Vytvoření instance aplikace mohou být skripty pomocí prostředí PowerShell, nebo napsané v jazyce C# pro snadnou integraci v procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="55230-150">Creating an application instance can be scripted using PowerShell, or written in C#, for easy integration in a build process.</span></span>

<span data-ttu-id="55230-151">Pomocí prostředí PowerShell, parametr hello je poskytnutá toohello `New-ServiceFabricApplication` příkaz jako [zatřiďovací tabulku](https://technet.microsoft.com/library/ee692803.aspx):</span><span class="sxs-lookup"><span data-stu-id="55230-151">Using PowerShell, hello parameter is supplied toohello `New-ServiceFabricApplication` command as a [hash table](https://technet.microsoft.com/library/ee692803.aspx):</span></span>

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

<span data-ttu-id="55230-152">Pomocí jazyka C#, parametry aplikace jsou určené v `ApplicationDescription` jako `NameValueCollection`:</span><span class="sxs-lookup"><span data-stu-id="55230-152">Using C#, application parameters are specified in an `ApplicationDescription` as a `NameValueCollection`:</span></span>

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a><span data-ttu-id="55230-153">Dešifrování tajné klíče z kódu služby</span><span class="sxs-lookup"><span data-stu-id="55230-153">Decrypt secrets from service code</span></span>
<span data-ttu-id="55230-154">Služby v Service Fabric spustit pod účtem NETWORK SERVICE ve výchozím nastavení v systému Windows a nejsou v uzlu hello bez zvláštní nastavení nainstalována toocertificates přístup.</span><span class="sxs-lookup"><span data-stu-id="55230-154">Services in Service Fabric run under NETWORK SERVICE by default on Windows and don't have access toocertificates installed on hello node without some extra setup.</span></span>

<span data-ttu-id="55230-155">Pokud používáte certifikát pro šifrování dat, je nutné toomake se, že síťová služba nebo jakoukoli uživatele účtu hello služba běží pod má privátní klíč certifikátu toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="55230-155">When using a data encipherment certificate, you need toomake sure NETWORK SERVICE or whatever user account hello service is running under has access toohello certificate's private key.</span></span> <span data-ttu-id="55230-156">Service Fabric zpracuje, automaticky udělení přístupu pro vaši službu, pokud jste ho nakonfigurovat toodo tak.</span><span class="sxs-lookup"><span data-stu-id="55230-156">Service Fabric will handle granting access for your service automatically if you configure it toodo so.</span></span> <span data-ttu-id="55230-157">Tuto konfiguraci lze provést v ApplicationManifest.xml definováním uživatelů a zásady zabezpečení pro certifikáty.</span><span class="sxs-lookup"><span data-stu-id="55230-157">This configuration can be done in ApplicationManifest.xml by defining users and security policies for certificates.</span></span> <span data-ttu-id="55230-158">V následujícím příkladu hello je zadána hello účet NETWORK SERVICE přístup pro čtení tooa certifikát definované jeho kryptografický otisk:</span><span class="sxs-lookup"><span data-stu-id="55230-158">In hello following example, hello NETWORK SERVICE account is given read access tooa certificate defined by its thumbprint:</span></span>

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> <span data-ttu-id="55230-159">Při kopírování kryptografický otisk certifikátu z certifikátu hello úložiště modul snap-in v systému Windows, neviditelná znak je umístěn na začátku hello hello kryptografický otisk řetězec.</span><span class="sxs-lookup"><span data-stu-id="55230-159">When copying a certificate thumbprint from hello certificate store snap-in on Windows, an invisible character is placed at hello beginning of hello thumbprint string.</span></span> <span data-ttu-id="55230-160">Tento znak neviditelná může způsobit chybu při pokusu o toolocate certifikát kryptografický otisk, proto být jisti toodelete tento další znak.</span><span class="sxs-lookup"><span data-stu-id="55230-160">This invisible character can cause an error when trying toolocate a certificate by thumbprint, so be sure toodelete this extra character.</span></span>
> 
> 

### <a name="use-application-secrets-in-service-code"></a><span data-ttu-id="55230-161">Použití aplikace tajných klíčů v kódu služby</span><span class="sxs-lookup"><span data-stu-id="55230-161">Use application secrets in service code</span></span>
<span data-ttu-id="55230-162">Hello rozhraní API pro přístup k hodnoty konfigurace z souborech Settings.xml v balíčku konfigurace umožňuje snadno dešifrování hodnot, které mají hello `IsEncrypted` atribut nastaven příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="55230-162">hello API for accessing configuration values from Settings.xml in a configuration package allows for easy decrypting of values that have hello `IsEncrypted` attribute set too`true`.</span></span> <span data-ttu-id="55230-163">Vzhledem k tomu, že šifrované hello text obsahuje informace o hello certifikát použitý k šifrování, není nutné toomanually najít hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="55230-163">Since hello encrypted text contains information about hello certificate used for encryption, you do not need toomanually find hello certificate.</span></span> <span data-ttu-id="55230-164">certifikát Hello právě musí toobe nainstalovaný na uzlu hello, zda je spuštěna služba hello na.</span><span class="sxs-lookup"><span data-stu-id="55230-164">hello certificate just needs toobe installed on hello node that hello service is running on.</span></span> <span data-ttu-id="55230-165">Jednoduše volání hello `DecryptValue()` metoda tooretrieve hello původní tajná hodnota:</span><span class="sxs-lookup"><span data-stu-id="55230-165">Simply call hello `DecryptValue()` method tooretrieve hello original secret value:</span></span>

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a><span data-ttu-id="55230-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55230-166">Next Steps</span></span>
<span data-ttu-id="55230-167">Další informace o [spuštění aplikací pomocí jiné bezpečnostní oprávnění](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="55230-167">Learn more about [running applications with different security permissions](service-fabric-application-runas-security.md)</span></span>

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
