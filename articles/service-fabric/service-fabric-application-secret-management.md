---
title: "Správa tajných klíčů v Service Fabric aplikace | Microsoft Docs"
description: "Tento článek popisuje, jak zabezpečit tajný hodnoty v aplikaci Service Fabric."
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
ms.openlocfilehash: d71924cda8bb3bffbe221946d80dba150359e38e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a><span data-ttu-id="750ce-103">Správa tajných klíčů v aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="750ce-103">Managing secrets in Service Fabric applications</span></span>
<span data-ttu-id="750ce-104">Tento průvodce vás provede kroky správy tajných klíčů v aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="750ce-104">This guide walks you through the steps of managing secrets in a Service Fabric application.</span></span> <span data-ttu-id="750ce-105">Tajné klíče může být žádné citlivé informace, jako je například úložiště připojovací řetězce, hesla nebo jiné hodnoty, které by neměly být zpracovány v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="750ce-105">Secrets can be any sensitive information, such as storage connection strings, passwords, or other values that should not be handled in plain text.</span></span>

<span data-ttu-id="750ce-106">Tato příručka používá Azure Key Vault pro správu klíčů a tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="750ce-106">This guide uses Azure Key Vault to manage keys and secrets.</span></span> <span data-ttu-id="750ce-107">Ale *pomocí* tajných klíčů v aplikaci je Cloudová platforma vznikl k aplikacím umožňují nasadit do clusteru s podporou hostovat kdekoli.</span><span class="sxs-lookup"><span data-stu-id="750ce-107">However, *using* secrets in an application is cloud platform-agnostic to allow applications to be deployed to a cluster hosted anywhere.</span></span> 

## <a name="overview"></a><span data-ttu-id="750ce-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="750ce-108">Overview</span></span>
<span data-ttu-id="750ce-109">Doporučený způsob, jak spravovat nastavení konfigurace služby je prostřednictvím [služby balíčky konfigurace][config-package].</span><span class="sxs-lookup"><span data-stu-id="750ce-109">The recommended way to manage service configuration settings is through [service configuration packages][config-package].</span></span> <span data-ttu-id="750ce-110">Konfigurace balíčků jsou verzí a aktualizovat prostřednictvím spravované postupné upgrady s vyhodnocení stavu a automatického vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="750ce-110">Configuration packages are versioned and updatable through managed rolling upgrades with health-validation and auto rollback.</span></span> <span data-ttu-id="750ce-111">Jedná se upřednostňované globální konfiguraci, protože snižuje pravděpodobnost výpadkem globální služby.</span><span class="sxs-lookup"><span data-stu-id="750ce-111">This is preferred to global configuration as it reduces the chances of a global service outage.</span></span> <span data-ttu-id="750ce-112">Šifrované tajné klíče jsou žádná výjimka.</span><span class="sxs-lookup"><span data-stu-id="750ce-112">Encrypted secrets are no exception.</span></span> <span data-ttu-id="750ce-113">Service Fabric obsahuje integrované funkce pro šifrování a dešifrování hodnot v konfiguračním souboru souborech Settings.xml balíček pomocí šifrování certifikátu.</span><span class="sxs-lookup"><span data-stu-id="750ce-113">Service Fabric has built-in features for encrypting and decrypting values in a configuration package Settings.xml file using certificate encryption.</span></span>

<span data-ttu-id="750ce-114">Následující diagram znázorňuje základní postup pro tajný správy v aplikaci Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="750ce-114">The following diagram illustrates the basic flow for secret management in a Service Fabric application:</span></span>

![Přehled tajný správy][overview]

<span data-ttu-id="750ce-116">V tomto toku existují čtyři hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="750ce-116">There are four main steps in this flow:</span></span>

1. <span data-ttu-id="750ce-117">Získejte certifikát dat šifrování.</span><span class="sxs-lookup"><span data-stu-id="750ce-117">Obtain a data encipherment certificate.</span></span>
2. <span data-ttu-id="750ce-118">Nainstalujte certifikát v clusteru.</span><span class="sxs-lookup"><span data-stu-id="750ce-118">Install the certificate in your cluster.</span></span>
3. <span data-ttu-id="750ce-119">Šifrování tajný hodnoty při nasazení aplikace pomocí certifikátu a vložit je do služby souborech Settings.xml konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="750ce-119">Encrypt secret values when deploying an application with the certificate and inject them into a service's Settings.xml configuration file.</span></span>
4. <span data-ttu-id="750ce-120">Číst šifrovaných hodnot mimo souborech Settings.xml dešifrování stejným certifikátem šifrování.</span><span class="sxs-lookup"><span data-stu-id="750ce-120">Read encrypted values out of Settings.xml by decrypting with the same encipherment certificate.</span></span> 

<span data-ttu-id="750ce-121">[Azure Key Vault] [ key-vault-get-started] zde slouží jako umístění úložiště bezpečné pro certifikáty a jako způsob, jak získat certifikáty, které jsou nainstalované na clusterů Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="750ce-121">[Azure Key Vault][key-vault-get-started] is used here as a safe storage location for certificates and as a way to get certificates installed on Service Fabric clusters in Azure.</span></span> <span data-ttu-id="750ce-122">Pokud nejsou nasazení do Azure, není nutné používat ke správě tajných klíčů v Service Fabric aplikace Key Vault.</span><span class="sxs-lookup"><span data-stu-id="750ce-122">If you are not deploying to Azure, you do not need to use Key Vault to manage secrets in Service Fabric applications.</span></span>

## <a name="data-encipherment-certificate"></a><span data-ttu-id="750ce-123">Certifikát pro šifrování dat</span><span class="sxs-lookup"><span data-stu-id="750ce-123">Data encipherment certificate</span></span>
<span data-ttu-id="750ce-124">Certifikát šifrování dat se používají výhradně pro šifrování a dešifrování konfigurace hodnoty v souborech Settings.xml služby a není používá pro ověřování nebo podpisový šifrovací textu.</span><span class="sxs-lookup"><span data-stu-id="750ce-124">A data encipherment certificate is used strictly for encryption and decryption of configuration values in a service's Settings.xml and is not used for authentication or signing of cipher text.</span></span> <span data-ttu-id="750ce-125">Certifikát musí splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="750ce-125">The certificate must meet the following requirements:</span></span>

* <span data-ttu-id="750ce-126">Certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="750ce-126">The certificate must contain a private key.</span></span>
* <span data-ttu-id="750ce-127">Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="750ce-127">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="750ce-128">Použití klíče certifikátu musí obsahovat šifrování dat (10) a nesmí obsahovat Server ověřování nebo ověřování klientů.</span><span class="sxs-lookup"><span data-stu-id="750ce-128">The certificate key usage must include Data Encipherment (10), and should not include Server Authentication or Client Authentication.</span></span> 
  
  <span data-ttu-id="750ce-129">Při vytváření certifikát podepsaný svým držitelem pomocí prostředí PowerShell, například `KeyUsage` musí být nastaven příznak `DataEncipherment`:</span><span class="sxs-lookup"><span data-stu-id="750ce-129">For example, when creating a self-signed certificate using PowerShell, the `KeyUsage` flag must be set to `DataEncipherment`:</span></span>
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a><span data-ttu-id="750ce-130">Instalace certifikátu v clusteru</span><span class="sxs-lookup"><span data-stu-id="750ce-130">Install the certificate in your cluster</span></span>
<span data-ttu-id="750ce-131">Tento certifikát musí být nainstalován na každém uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="750ce-131">This certificate must be installed on each node in the cluster.</span></span> <span data-ttu-id="750ce-132">Použije se v době běhu k dešifrování hodnot uložených v souborech Settings.xml služby.</span><span class="sxs-lookup"><span data-stu-id="750ce-132">It will be used at runtime to decrypt values stored in a service's Settings.xml.</span></span> <span data-ttu-id="750ce-133">V tématu [postup vytvoření clusteru s podporou pomocí Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] pokyny pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="750ce-133">See [how to create a cluster using Azure Resource Manager][service-fabric-cluster-creation-via-arm] for setup instructions.</span></span> 

## <a name="encrypt-application-secrets"></a><span data-ttu-id="750ce-134">Šifrování tajné klíče aplikace</span><span class="sxs-lookup"><span data-stu-id="750ce-134">Encrypt application secrets</span></span>
<span data-ttu-id="750ce-135">Sada Service Fabric SDK obsahuje vestavěné tajný šifrování a dešifrování funkce.</span><span class="sxs-lookup"><span data-stu-id="750ce-135">The Service Fabric SDK has built-in secret encryption and decryption functions.</span></span> <span data-ttu-id="750ce-136">Tajný hodnoty může být v vytvořené čas zašifrované dešifrovat a čtení prostřednictvím kódu programu v kódu služby.</span><span class="sxs-lookup"><span data-stu-id="750ce-136">Secret values can be encrypted at built-time and then decrypted and read programmatically in service code.</span></span> 

<span data-ttu-id="750ce-137">Následující příkaz prostředí PowerShell se používá k šifrování tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="750ce-137">The following PowerShell command is used to encrypt a secret.</span></span> <span data-ttu-id="750ce-138">Tento příkaz šifruje pouze hodnotu parametru. provede **není** přihlásit šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="750ce-138">This command only encrypts the value; it does **not** sign the cipher text.</span></span> <span data-ttu-id="750ce-139">Je nutné použít stejný certifikát šifrování, který je nainstalován v clusteru k vytvoření ciphertext tajný hodnoty:</span><span class="sxs-lookup"><span data-stu-id="750ce-139">You must use the same encipherment certificate that is installed in your cluster to produce ciphertext for secret values:</span></span>

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="750ce-140">Výsledný řetězec kódování base-64 obsahuje jak na tajný šifrovaný text a také informace o certifikátu, který byl použit k jejich zašifrování.</span><span class="sxs-lookup"><span data-stu-id="750ce-140">The resulting base-64 string contains both the secret ciphertext as well as information about the certificate that was used to encrypt it.</span></span>  <span data-ttu-id="750ce-141">Řetězec s kódováním base-64 lze vložit do parametru v konfiguračním souboru na souborech Settings.xml vaší služby pomocí `IsEncrypted` atribut nastaven na `true`:</span><span class="sxs-lookup"><span data-stu-id="750ce-141">The base-64 encoded string can be inserted into a parameter in your service's Settings.xml configuration file with the `IsEncrypted` attribute set to `true`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a><span data-ttu-id="750ce-142">Vložit tajné klíče aplikace do instance aplikace</span><span class="sxs-lookup"><span data-stu-id="750ce-142">Inject application secrets into application instances</span></span>
<span data-ttu-id="750ce-143">V ideálním případě by měl být nasazení do různých prostředí jako automatizované nejblíže.</span><span class="sxs-lookup"><span data-stu-id="750ce-143">Ideally, deployment to different environments should be as automated as possible.</span></span> <span data-ttu-id="750ce-144">To můžete udělat tak, že provádění tajný šifrování v prostředí pro sestavování a poskytování šifrované tajné klíče jako parametry, při vytváření instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="750ce-144">This can be accomplished by performing secret encryption in a build environment and providing the encrypted secrets as parameters when creating application instances.</span></span>

#### <a name="use-overridable-parameters-in-settingsxml"></a><span data-ttu-id="750ce-145">Použití přepsatelnými parametry v souborech Settings.xml</span><span class="sxs-lookup"><span data-stu-id="750ce-145">Use overridable parameters in Settings.xml</span></span>
<span data-ttu-id="750ce-146">Konfigurační soubor souborech Settings.xml umožňuje přepsatelnými parametry, které lze zadat v okamžiku vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="750ce-146">The Settings.xml configuration file allows overridable parameters that can be provided at application creation time.</span></span> <span data-ttu-id="750ce-147">Použití `MustOverride` atribut místo hodnotu pro parametr:</span><span class="sxs-lookup"><span data-stu-id="750ce-147">Use the `MustOverride` attribute instead of providing a value for a parameter:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

<span data-ttu-id="750ce-148">Pokud chcete přepsat hodnoty v souborech Settings.xml, deklarujte parametrem přepsání pro službu v ApplicationManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="750ce-148">To override values in Settings.xml, declare an override parameter for the service in ApplicationManifest.xml:</span></span>

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

<span data-ttu-id="750ce-149">Teď hodnotu lze zadat jako *aplikace parametr* při vytváření instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="750ce-149">Now the value can be specified as an *application parameter* when creating an instance of the application.</span></span> <span data-ttu-id="750ce-150">Vytvoření instance aplikace mohou být skripty pomocí prostředí PowerShell, nebo napsané v jazyce C# pro snadnou integraci v procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="750ce-150">Creating an application instance can be scripted using PowerShell, or written in C#, for easy integration in a build process.</span></span>

<span data-ttu-id="750ce-151">Pomocí prostředí PowerShell, parametr je dodána na `New-ServiceFabricApplication` příkaz jako [zatřiďovací tabulku](https://technet.microsoft.com/library/ee692803.aspx):</span><span class="sxs-lookup"><span data-stu-id="750ce-151">Using PowerShell, the parameter is supplied to the `New-ServiceFabricApplication` command as a [hash table](https://technet.microsoft.com/library/ee692803.aspx):</span></span>

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

<span data-ttu-id="750ce-152">Pomocí jazyka C#, parametry aplikace jsou určené v `ApplicationDescription` jako `NameValueCollection`:</span><span class="sxs-lookup"><span data-stu-id="750ce-152">Using C#, application parameters are specified in an `ApplicationDescription` as a `NameValueCollection`:</span></span>

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

## <a name="decrypt-secrets-from-service-code"></a><span data-ttu-id="750ce-153">Dešifrování tajné klíče z kódu služby</span><span class="sxs-lookup"><span data-stu-id="750ce-153">Decrypt secrets from service code</span></span>
<span data-ttu-id="750ce-154">Služby v Service Fabric běží pod účtem NETWORK SERVICE ve výchozím nastavení v systému Windows a nemají přístup k certifikáty, které jsou nainstalovány na uzlu bez zvláštní nastavení.</span><span class="sxs-lookup"><span data-stu-id="750ce-154">Services in Service Fabric run under NETWORK SERVICE by default on Windows and don't have access to certificates installed on the node without some extra setup.</span></span>

<span data-ttu-id="750ce-155">Když používá certifikát, šifrování dat, je třeba Ujistěte se, zda síťové služby nebo ať uživatelský účet služby je spuštěno má přístup k privátní klíč certifikátu.</span><span class="sxs-lookup"><span data-stu-id="750ce-155">When using a data encipherment certificate, you need to make sure NETWORK SERVICE or whatever user account the service is running under has access to the certificate's private key.</span></span> <span data-ttu-id="750ce-156">Udělení přístupu pro vaši službu automaticky, pokud je třeba nakonfigurovat tak, Service Fabric bude zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="750ce-156">Service Fabric will handle granting access for your service automatically if you configure it to do so.</span></span> <span data-ttu-id="750ce-157">Tuto konfiguraci lze provést v ApplicationManifest.xml definováním uživatelů a zásady zabezpečení pro certifikáty.</span><span class="sxs-lookup"><span data-stu-id="750ce-157">This configuration can be done in ApplicationManifest.xml by defining users and security policies for certificates.</span></span> <span data-ttu-id="750ce-158">V následujícím příkladu je účet NETWORK SERVICE poskytnut přístup pro čtení k definované jeho kryptografický otisk certifikátu:</span><span class="sxs-lookup"><span data-stu-id="750ce-158">In the following example, the NETWORK SERVICE account is given read access to a certificate defined by its thumbprint:</span></span>

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
> <span data-ttu-id="750ce-159">Při kopírování kryptografický otisk certifikátu z modulu certifikát úložiště snap-in v systému Windows, neviditelná znak je umístěn na začátku řetězce kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="750ce-159">When copying a certificate thumbprint from the certificate store snap-in on Windows, an invisible character is placed at the beginning of the thumbprint string.</span></span> <span data-ttu-id="750ce-160">Tento znak neviditelná může způsobit chybu při pokusu o vyhledat certifikát pomocí kryptografického otisku, takže je nutné odstranit tento další znak.</span><span class="sxs-lookup"><span data-stu-id="750ce-160">This invisible character can cause an error when trying to locate a certificate by thumbprint, so be sure to delete this extra character.</span></span>
> 
> 

### <a name="use-application-secrets-in-service-code"></a><span data-ttu-id="750ce-161">Použití aplikace tajných klíčů v kódu služby</span><span class="sxs-lookup"><span data-stu-id="750ce-161">Use application secrets in service code</span></span>
<span data-ttu-id="750ce-162">Rozhraní API pro přístup k hodnoty konfigurace z souborech Settings.xml v balíčku konfigurace umožňuje snadno dešifrování hodnot, které mají `IsEncrypted` atribut nastaven na `true`.</span><span class="sxs-lookup"><span data-stu-id="750ce-162">The API for accessing configuration values from Settings.xml in a configuration package allows for easy decrypting of values that have the `IsEncrypted` attribute set to `true`.</span></span> <span data-ttu-id="750ce-163">Vzhledem k tomu, že šifrované text obsahuje informace o certifikát použitý k šifrování, není potřeba ručně najít certifikát.</span><span class="sxs-lookup"><span data-stu-id="750ce-163">Since the encrypted text contains information about the certificate used for encryption, you do not need to manually find the certificate.</span></span> <span data-ttu-id="750ce-164">Právě musí být nainstalovaný na uzlu, který je služba spuštěná na certifikátu.</span><span class="sxs-lookup"><span data-stu-id="750ce-164">The certificate just needs to be installed on the node that the service is running on.</span></span> <span data-ttu-id="750ce-165">Jednoduše volání `DecryptValue()` metoda pro načtení původní tajná hodnota:</span><span class="sxs-lookup"><span data-stu-id="750ce-165">Simply call the `DecryptValue()` method to retrieve the original secret value:</span></span>

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a><span data-ttu-id="750ce-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="750ce-166">Next Steps</span></span>
<span data-ttu-id="750ce-167">Další informace o [spuštění aplikací pomocí jiné bezpečnostní oprávnění](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="750ce-167">Learn more about [running applications with different security permissions](service-fabric-application-runas-security.md)</span></span>

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
