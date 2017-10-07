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
# <a name="managing-secrets-in-service-fabric-applications"></a>Správa tajných klíčů v aplikace Service Fabric
Tento průvodce vás provede kroky hello správy tajných klíčů v aplikace Service Fabric. Tajné klíče může být žádné citlivé informace, jako je například úložiště připojovací řetězce, hesla nebo jiné hodnoty, které by neměly být zpracovány v prostém textu.

Tato příručka používá Azure Key Vault toomanage klíče a tajné klíče. Ale *pomocí* tajných klíčů v aplikaci je cloudové bez ohledu na platformu tooallow aplikace toobe nasazené tooa clusteru hostované odkudkoli. 

## <a name="overview"></a>Přehled
Hello nastavení konfigurace služby toomanage doporučeným způsobem je prostřednictvím [služby balíčky konfigurace][config-package]. Konfigurace balíčků jsou verzí a aktualizovat prostřednictvím spravované postupné upgrady s vyhodnocení stavu a automatického vrácení zpět. Toto je upřednostňovaný tooglobal konfigurace, jako snižuje pravděpodobnost hello výpadkem globální služby. Šifrované tajné klíče jsou žádná výjimka. Service Fabric obsahuje integrované funkce pro šifrování a dešifrování hodnot v konfiguračním souboru souborech Settings.xml balíček pomocí šifrování certifikátu.

Hello následující diagram znázorňuje hello základní postup pro tajný správy v aplikaci Service Fabric:

![Přehled tajný správy][overview]

V tomto toku existují čtyři hlavní kroky:

1. Získejte certifikát dat šifrování.
2. Nainstalujte certifikát hello v clusteru.
3. Šifrování tajný hodnoty při nasazení aplikace s hello certifikátu a vložit je do služby souborech Settings.xml konfigurační soubor.
4. Čtení šifrované hodnoty mimo souborech Settings.xml dešifrováním s hello stejný certifikát pro šifrování. 

[Azure Key Vault] [ key-vault-get-started] zde slouží jako umístění úložiště bezpečné pro certifikáty a jako způsob tooget certifikátů nainstalovaných na clusterů Service Fabric v Azure. Pokud nejsou nasazení tooAzure, není nutné toouse Key Vault toomanage tajných klíčů v Service Fabric aplikace.

## <a name="data-encipherment-certificate"></a>Certifikát pro šifrování dat
Certifikát šifrování dat se používají výhradně pro šifrování a dešifrování konfigurace hodnoty v souborech Settings.xml služby a není používá pro ověřování nebo podpisový šifrovací textu. certifikát Hello musí splňovat následující požadavky hello:

* Hello certifikát musí obsahovat privátní klíč.
* Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).
* použití klíče certifikátu Hello musí zahrnovat šifrování dat (10) a nesmí obsahovat Server ověřování nebo ověřování klientů. 
  
  Například při vytváření certifikát podepsaný svým držitelem pomocí prostředí PowerShell, hello `KeyUsage` příliš musí být nastaven příznak`DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-hello-certificate-in-your-cluster"></a>Nainstalujte certifikát hello v clusteru
Tento certifikát musí být nainstalován na každém uzlu v clusteru hello. Použije se v modulu runtime toodecrypt hodnotami uloženými v souborech Settings.xml služby. V tématu [jak toocreate clusteru pomocí Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] pokyny pro instalaci. 

## <a name="encrypt-application-secrets"></a>Šifrování tajné klíče aplikace
Hello Service Fabric SDK obsahuje vestavěné tajný šifrování a dešifrování funkce. Tajný hodnoty může být v vytvořené čas zašifrované dešifrovat a čtení prostřednictvím kódu programu v kódu služby. 

Následující příkaz prostředí PowerShell Hello je použité tooencrypt tajný klíč. Tento příkaz šifruje pouze hodnota hello; provede **není** přihlásit hello šifrovaný text. Hello je nutné použít stejný certifikát šifrování, které jsou nainstalovány ve vaší ciphertext tooproduce clusteru pro tajný hodnoty:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Hello výsledný řetězec kódování base-64 obsahuje jak hello tajný šifrovaný text a také informace o hello certifikátu, který byl použité tooencrypt ho.  Hello řetězec s kódováním base-64 lze vložit do parametru v konfiguračním souboru na souborech Settings.xml vaší služby s hello `IsEncrypted` atribut nastaven příliš`true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Vložit tajné klíče aplikace do instance aplikace
V ideálním případě by měl být nasazení toodifferent prostředí jako automatizované nejblíže. To můžete provést provádění tajný šifrování v prostředí pro sestavování a poskytnutím hello šifrované tajné klíče jako parametry, při vytváření instancí aplikace.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Použití přepsatelnými parametry v souborech Settings.xml
konfigurační soubor Hello souborech Settings.xml umožňuje přepsatelnými parametry, které lze zadat v okamžiku vytvoření aplikace. Použití hello `MustOverride` atribut místo hodnotu pro parametr:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

toooverride hodnoty v souborech Settings.xml, deklarovat parametrem přepsání pro službu hello v ApplicationManifest.xml:

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

Teď hello hodnotu lze zadat jako *aplikace parametr* při vytváření instance aplikace hello. Vytvoření instance aplikace mohou být skripty pomocí prostředí PowerShell, nebo napsané v jazyce C# pro snadnou integraci v procesu sestavení.

Pomocí prostředí PowerShell, parametr hello je poskytnutá toohello `New-ServiceFabricApplication` příkaz jako [zatřiďovací tabulku](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Pomocí jazyka C#, parametry aplikace jsou určené v `ApplicationDescription` jako `NameValueCollection`:

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

## <a name="decrypt-secrets-from-service-code"></a>Dešifrování tajné klíče z kódu služby
Služby v Service Fabric spustit pod účtem NETWORK SERVICE ve výchozím nastavení v systému Windows a nejsou v uzlu hello bez zvláštní nastavení nainstalována toocertificates přístup.

Pokud používáte certifikát pro šifrování dat, je nutné toomake se, že síťová služba nebo jakoukoli uživatele účtu hello služba běží pod má privátní klíč certifikátu toohello přístup. Service Fabric zpracuje, automaticky udělení přístupu pro vaši službu, pokud jste ho nakonfigurovat toodo tak. Tuto konfiguraci lze provést v ApplicationManifest.xml definováním uživatelů a zásady zabezpečení pro certifikáty. V následujícím příkladu hello je zadána hello účet NETWORK SERVICE přístup pro čtení tooa certifikát definované jeho kryptografický otisk:

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
> Při kopírování kryptografický otisk certifikátu z certifikátu hello úložiště modul snap-in v systému Windows, neviditelná znak je umístěn na začátku hello hello kryptografický otisk řetězec. Tento znak neviditelná může způsobit chybu při pokusu o toolocate certifikát kryptografický otisk, proto být jisti toodelete tento další znak.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Použití aplikace tajných klíčů v kódu služby
Hello rozhraní API pro přístup k hodnoty konfigurace z souborech Settings.xml v balíčku konfigurace umožňuje snadno dešifrování hodnot, které mají hello `IsEncrypted` atribut nastaven příliš`true`. Vzhledem k tomu, že šifrované hello text obsahuje informace o hello certifikát použitý k šifrování, není nutné toomanually najít hello certifikátu. certifikát Hello právě musí toobe nainstalovaný na uzlu hello, zda je spuštěna služba hello na. Jednoduše volání hello `DecryptValue()` metoda tooretrieve hello původní tajná hodnota:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Další kroky
Další informace o [spuštění aplikací pomocí jiné bezpečnostní oprávnění](service-fabric-application-runas-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
