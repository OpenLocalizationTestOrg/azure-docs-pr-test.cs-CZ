---
title: "Poznámky k verzi rozhraní API .NET trezoru 2.x aaaKey | Microsoft Docs"
description: "Vývojáři rozhraní .NET bude používat toto toocode rozhraní API pro Azure Key Vault"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: d95b84cf73c155f117f37e93893f27b02a75855c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault rozhraní .NET 2.0 – poznámky k verzi a Průvodci migrací
Hello následující poznámky jsou a pokyny pro vývojáře, kteří pracují s Azure Key Vault .NET nebo knihovna jazyka C#. V přechodném stavu hello z verze toohello 2.0 verze hello 1.0, byly provedeny počet aktualizací se bude vyžadovat migrace práci ve vašem kódu v pořadí pro toobenefit z funkční zdokonalení hello a funkce, jako dodatky **Key Vault certifikáty** podporovat.

## <a name="key-vault-certificates"></a>Certifikáty Key Vault

Podpora certifikáty Key Vault poskytuje správu vašeho x509 certifikáty a hello následující chování:  

* Umožňuje toocreate vlastníka certifikát certifikátu provede procesem vytvoření trezoru klíč nebo prostřednictvím hello importu stávajícího certifikátu. To zahrnuje i podepsaného svým držitelem a vygeneruje certifikáty certifikační autority.
* Umožňuje vlastníka certifikát klíč trezoru tooimplement zabezpečeného úložiště a správu X509 certifikáty bez interakce s materiál privátního klíče.  
* Umožňuje toocreate vlastníka certifikát zásadu, která přesměruje Key Vault toomanage hello životního cyklu certifikátu.  
* Umožňuje vlastníkům certifikát tooprovide kontaktní informace pro oznámení o události životního cyklu vypršení platnosti a obnovení certifikátu.  
* Podporuje automatické obnovení se vybrané vystavitelů - Key Vault partnera X509 certifikátu zprostředkovatelé / certifikační autority.
  
  * Všimněte si --spolupráci zprostředkovatelé/autority také mohou, ale nebude podporovat funkce automatického obnovení hello.

## <a name="net-support"></a>Podpora v rozhraní .NET

* **Rozhraní .NET 4.0** nepodporuje hello 2.0 verzi hello Azure Key Vault .NET nebo knihovna jazyka C#
* **.NET core** podporuje hello 2.0 verzi hello Azure Key Vault .NET nebo knihovna jazyka C#

## <a name="namespaces"></a>obory názvů

* obor názvů pro Hello **modely** změněn z **Microsoft.Azure.KeyVault** příliš**Microsoft.Azure.KeyVault.Models**.
* Hello **Microsoft.Azure.KeyVault.Internal** obor názvů se ukončí.
* obor názvů závislosti Hello Azure SDK došlo ke změně z **Hyak.Common** a **Hyak.Common.Internals** příliš**Microsoft.Rest** a  **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Typ změny

* *Tajný klíč* změnit příliš*SecretBundle*
* *Slovník* změnit příliš*IDictionary*
* *Seznam<T>, string []* změnit příliš*rozhraní IList.<T>*
* *NextList* změnit příliš *NextPageLink*

## <a name="return-types"></a>Návratové typy

* **KeyList** a **SecretList** vrátí *IPage<T>*  místo *ListKeysResponseMessage*
* Hello generované **BackupKeyAsync** vrátí *BackupKeyResult* obsahující *hodnotu* (blob zálohování). Před hello byl metoda zabalené a vracejících jediná hodnota hello.

## <a name="exceptions"></a>Výjimky

* *KeyVaultClientException* mění příliš*KeyVaultErrorException*
* Chyba služby Hello se změnilo z *výjimka. Chyba* příliš*výjimka. Body.Error.Message*.
* Další informace o odeberou hello chybovou zprávu pro **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktory

* Místo přijetí *HttpClient* jako argument konstruktoru, hello konstruktor přijímá pouze *HttpClientHandler* nebo *[DelegatingHandler]*.

## <a name="downloaded-packages"></a>Stažených balíčků

Když klient zpracovává závislost na byly staženy následující hello Key Vault

### <a name="previous-package-list"></a>Předchozí seznam balíčků

* verze balíčku id="Hyak.Common" = "1.0.2" targetFramework = "net45"
* verze balíčku id="Microsoft.Azure.Common" = "verze 2.0.4" targetFramework = "net45"
* verze balíčku id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
* verze balíčku id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
* verze balíčku id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
* verze balíčku id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
* verze balíčku id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
* verze balíčku id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

### <a name="current-package-list"></a>Aktuální seznam balíčků

* verze balíčku id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
* verze balíčku id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
* verze balíčku id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"

## <a name="class-changes"></a>Změny – třída

* **UnixEpoch** odebrala – třída
* **Base64UrlConverter** třída je přejmenován příliš**Base64UrlJsonConverter**

## <a name="other-changes"></a>Další změny

* Byla přidána podpora pro konfiguraci hello zásady opakování operace KV o přechodné selhání toothis verzi hello rozhraní API.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Pro hello operace, které vrátil *trezoru*, návratový typ hello byl třídu, která obsahovala vlastnosti trezoru. Hello návratový typ je nyní *trezoru*.
* *PermissionsToKeys* a *PermissionsToSecrets* jsou nyní *Permissions.Keys* a *Permissions.Secrets*
* Některé hello návratové typy změny použít toohello řízení roviny také.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* balíček Hello příliš rozdělen**Microsoft.Azure.KeyVault.Extensions** a **Microsoft.Azure.KeyVault.Cryptography** pro hello kryptografické operace.

