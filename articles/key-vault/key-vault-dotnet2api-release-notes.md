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
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a><span data-ttu-id="ba9c7-103">Azure Key Vault rozhraní .NET 2.0 – poznámky k verzi a Průvodci migrací</span><span class="sxs-lookup"><span data-stu-id="ba9c7-103">Azure Key Vault .NET 2.0 - Release Notes and Migration Guide</span></span>
<span data-ttu-id="ba9c7-104">Hello následující poznámky jsou a pokyny pro vývojáře, kteří pracují s Azure Key Vault .NET nebo knihovna jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-104">hello following notes and guidance are for developers working with Azure Key Vault .NET / C# library.</span></span> <span data-ttu-id="ba9c7-105">V přechodném stavu hello z verze toohello 2.0 verze hello 1.0, byly provedeny počet aktualizací se bude vyžadovat migrace práci ve vašem kódu v pořadí pro toobenefit z funkční zdokonalení hello a funkce, jako dodatky **Key Vault certifikáty** podporovat.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-105">In hello transition from hello 1.0 version toohello 2.0 version, a number of updates have been made that will require migration work in your code in order for you toobenefit from hello functional improvements and feature additions such as **Key Vault certificates** support.</span></span>

## <a name="key-vault-certificates"></a><span data-ttu-id="ba9c7-106">Certifikáty Key Vault</span><span class="sxs-lookup"><span data-stu-id="ba9c7-106">Key Vault certificates</span></span>

<span data-ttu-id="ba9c7-107">Podpora certifikáty Key Vault poskytuje správu vašeho x509 certifikáty a hello následující chování:</span><span class="sxs-lookup"><span data-stu-id="ba9c7-107">Key Vault certificates support provides for management of your x509 certificates and hello following behaviors:</span></span>  

* <span data-ttu-id="ba9c7-108">Umožňuje toocreate vlastníka certifikát certifikátu provede procesem vytvoření trezoru klíč nebo prostřednictvím hello importu stávajícího certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-108">Allows a certificate owner toocreate a certificate through a Key Vault creation process or through hello import of an existing certificate.</span></span> <span data-ttu-id="ba9c7-109">To zahrnuje i podepsaného svým držitelem a vygeneruje certifikáty certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-109">This includes both self-signed and Certificate Authority generated certificates.</span></span>
* <span data-ttu-id="ba9c7-110">Umožňuje vlastníka certifikát klíč trezoru tooimplement zabezpečeného úložiště a správu X509 certifikáty bez interakce s materiál privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-110">Allows a Key Vault certificate owner tooimplement secure storage and management of X509 certificates without interaction with private key material.</span></span>  
* <span data-ttu-id="ba9c7-111">Umožňuje toocreate vlastníka certifikát zásadu, která přesměruje Key Vault toomanage hello životního cyklu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-111">Allows a certificate owner toocreate a policy that directs Key Vault toomanage hello life-cycle of a certificate.</span></span>  
* <span data-ttu-id="ba9c7-112">Umožňuje vlastníkům certifikát tooprovide kontaktní informace pro oznámení o události životního cyklu vypršení platnosti a obnovení certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-112">Allows certificate owners tooprovide contact information for notification about life-cycle events of expiration and renewal of certificate.</span></span>  
* <span data-ttu-id="ba9c7-113">Podporuje automatické obnovení se vybrané vystavitelů - Key Vault partnera X509 certifikátu zprostředkovatelé / certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-113">Supports automatic renewal with selected issuers - Key Vault partner X509 certificate providers / certificate authorities.</span></span>
  
  * <span data-ttu-id="ba9c7-114">Všimněte si --spolupráci zprostředkovatelé/autority také mohou, ale nebude podporovat funkce automatického obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-114">NOTE - Non-partnered providers/authorities are also allowed but, will not support hello auto renewal feature.</span></span>

## <a name="net-support"></a><span data-ttu-id="ba9c7-115">Podpora v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ba9c7-115">.NET support</span></span>

* <span data-ttu-id="ba9c7-116">**Rozhraní .NET 4.0** nepodporuje hello 2.0 verzi hello Azure Key Vault .NET nebo knihovna jazyka C#</span><span class="sxs-lookup"><span data-stu-id="ba9c7-116">**.NET 4.0** is not supported by hello 2.0 version of hello Azure Key Vault .NET/C# library</span></span>
* <span data-ttu-id="ba9c7-117">**.NET core** podporuje hello 2.0 verzi hello Azure Key Vault .NET nebo knihovna jazyka C#</span><span class="sxs-lookup"><span data-stu-id="ba9c7-117">**.NET Core** is supported by hello 2.0 version of hello Azure Key Vault .NET/C# library</span></span>

## <a name="namespaces"></a><span data-ttu-id="ba9c7-118">obory názvů</span><span class="sxs-lookup"><span data-stu-id="ba9c7-118">Namespaces</span></span>

* <span data-ttu-id="ba9c7-119">obor názvů pro Hello **modely** změněn z **Microsoft.Azure.KeyVault** příliš**Microsoft.Azure.KeyVault.Models**.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-119">hello namespace for **models** is changed from **Microsoft.Azure.KeyVault** too**Microsoft.Azure.KeyVault.Models**.</span></span>
* <span data-ttu-id="ba9c7-120">Hello **Microsoft.Azure.KeyVault.Internal** obor názvů se ukončí.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-120">hello **Microsoft.Azure.KeyVault.Internal** namespace is dropped.</span></span>
* <span data-ttu-id="ba9c7-121">obor názvů závislosti Hello Azure SDK došlo ke změně z **Hyak.Common** a **Hyak.Common.Internals** příliš**Microsoft.Rest** a  **Microsoft.Rest.Serialization**</span><span class="sxs-lookup"><span data-stu-id="ba9c7-121">hello Azure SDK dependencies namespace are changed from **Hyak.Common** and **Hyak.Common.Internals** too**Microsoft.Rest** and **Microsoft.Rest.Serialization**</span></span>

## <a name="type-changes"></a><span data-ttu-id="ba9c7-122">Typ změny</span><span class="sxs-lookup"><span data-stu-id="ba9c7-122">Type changes</span></span>

* <span data-ttu-id="ba9c7-123">*Tajný klíč* změnit příliš*SecretBundle*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-123">*Secret* changed too*SecretBundle*</span></span>
* <span data-ttu-id="ba9c7-124">*Slovník* změnit příliš*IDictionary*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-124">*Dictionary* changed too*IDictionary*</span></span>
* <span data-ttu-id="ba9c7-125">*Seznam<T>, string []* změnit příliš*rozhraní IList.<T>*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-125">*List<T>, string []* changed too*IList<T>*</span></span>
* <span data-ttu-id="ba9c7-126">*NextList* změnit příliš *NextPageLink*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-126">*NextList* changed too *NextPageLink*</span></span>

## <a name="return-types"></a><span data-ttu-id="ba9c7-127">Návratové typy</span><span class="sxs-lookup"><span data-stu-id="ba9c7-127">Return types</span></span>

* <span data-ttu-id="ba9c7-128">**KeyList** a **SecretList** vrátí *IPage<T>*  místo *ListKeysResponseMessage*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-128">**KeyList** and **SecretList** will return *IPage<T>* instead of *ListKeysResponseMessage*</span></span>
* <span data-ttu-id="ba9c7-129">Hello generované **BackupKeyAsync** vrátí *BackupKeyResult* obsahující *hodnotu* (blob zálohování).</span><span class="sxs-lookup"><span data-stu-id="ba9c7-129">hello generated **BackupKeyAsync** will return *BackupKeyResult* which contains *Value* (back-up blob).</span></span> <span data-ttu-id="ba9c7-130">Před hello byl metoda zabalené a vracejících jediná hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-130">Before hello method was wrapped and returning only hello value.</span></span>

## <a name="exceptions"></a><span data-ttu-id="ba9c7-131">Výjimky</span><span class="sxs-lookup"><span data-stu-id="ba9c7-131">Exceptions</span></span>

* <span data-ttu-id="ba9c7-132">*KeyVaultClientException* mění příliš*KeyVaultErrorException*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-132">*KeyVaultClientException* is changed too*KeyVaultErrorException*</span></span>
* <span data-ttu-id="ba9c7-133">Chyba služby Hello se změnilo z *výjimka. Chyba* příliš*výjimka. Body.Error.Message*.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-133">hello service error is changed from *exception.Error* too*exception.Body.Error.Message*.</span></span>
* <span data-ttu-id="ba9c7-134">Další informace o odeberou hello chybovou zprávu pro **[JsonExtensionData]**.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-134">Removed additional info from hello error message for **[JsonExtensionData]**.</span></span>

## <a name="constructors"></a><span data-ttu-id="ba9c7-135">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="ba9c7-135">Constructors</span></span>

* <span data-ttu-id="ba9c7-136">Místo přijetí *HttpClient* jako argument konstruktoru, hello konstruktor přijímá pouze *HttpClientHandler* nebo *[DelegatingHandler]*.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-136">Instead of accepting an *HttpClient* as a constructor argument, hello constructor only accepts *HttpClientHandler* or *DelegatingHandler[]*.</span></span>

## <a name="downloaded-packages"></a><span data-ttu-id="ba9c7-137">Stažených balíčků</span><span class="sxs-lookup"><span data-stu-id="ba9c7-137">Downloaded packages</span></span>

<span data-ttu-id="ba9c7-138">Když klient zpracovává závislost na byly staženy následující hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="ba9c7-138">When a client is processing a  dependency on Key Vault hello following were downloaded</span></span>

### <a name="previous-package-list"></a><span data-ttu-id="ba9c7-139">Předchozí seznam balíčků</span><span class="sxs-lookup"><span data-stu-id="ba9c7-139">Previous package list</span></span>

* <span data-ttu-id="ba9c7-140">verze balíčku id="Hyak.Common" = "1.0.2" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-140">package id="Hyak.Common" version="1.0.2" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-141">verze balíčku id="Microsoft.Azure.Common" = "verze 2.0.4" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-141">package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-142">verze balíčku id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-142">package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-143">verze balíčku id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-143">package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-144">verze balíčku id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-144">package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-145">verze balíčku id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-145">package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-146">verze balíčku id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-146">package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-147">verze balíčku id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-147">package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"</span></span>

### <a name="current-package-list"></a><span data-ttu-id="ba9c7-148">Aktuální seznam balíčků</span><span class="sxs-lookup"><span data-stu-id="ba9c7-148">Current package list</span></span>

* <span data-ttu-id="ba9c7-149">verze balíčku id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-149">package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-150">verze balíčku id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-150">package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"</span></span>
* <span data-ttu-id="ba9c7-151">verze balíčku id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="ba9c7-151">package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"</span></span>

## <a name="class-changes"></a><span data-ttu-id="ba9c7-152">Změny – třída</span><span class="sxs-lookup"><span data-stu-id="ba9c7-152">Class changes</span></span>

* <span data-ttu-id="ba9c7-153">**UnixEpoch** odebrala – třída</span><span class="sxs-lookup"><span data-stu-id="ba9c7-153">**UnixEpoch** class has been removed</span></span>
* <span data-ttu-id="ba9c7-154">**Base64UrlConverter** třída je přejmenován příliš**Base64UrlJsonConverter**</span><span class="sxs-lookup"><span data-stu-id="ba9c7-154">**Base64UrlConverter** class is renamed too**Base64UrlJsonConverter**</span></span>

## <a name="other-changes"></a><span data-ttu-id="ba9c7-155">Další změny</span><span class="sxs-lookup"><span data-stu-id="ba9c7-155">Other changes</span></span>

* <span data-ttu-id="ba9c7-156">Byla přidána podpora pro konfiguraci hello zásady opakování operace KV o přechodné selhání toothis verzi hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-156">Support for hello configuration of KV operation retry policy on transient failures has been added toothis version of hello API.</span></span>

## <a name="microsoftazuremanagementkeyvault-nuget"></a><span data-ttu-id="ba9c7-157">Microsoft.Azure.Management.KeyVault NuGet</span><span class="sxs-lookup"><span data-stu-id="ba9c7-157">Microsoft.Azure.Management.KeyVault NuGet</span></span>

* <span data-ttu-id="ba9c7-158">Pro hello operace, které vrátil *trezoru*, návratový typ hello byl třídu, která obsahovala vlastnosti trezoru.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-158">For hello operations that returned a *vault*, hello return type was a class that contained a Vault property.</span></span> <span data-ttu-id="ba9c7-159">Hello návratový typ je nyní *trezoru*.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-159">hello return type is now *Vault*.</span></span>
* <span data-ttu-id="ba9c7-160">*PermissionsToKeys* a *PermissionsToSecrets* jsou nyní *Permissions.Keys* a *Permissions.Secrets*</span><span class="sxs-lookup"><span data-stu-id="ba9c7-160">*PermissionsToKeys* and *PermissionsToSecrets* are now *Permissions.Keys* and *Permissions.Secrets*</span></span>
* <span data-ttu-id="ba9c7-161">Některé hello návratové typy změny použít toohello řízení roviny také.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-161">Some of hello return types changes apply toohello control-plane as well.</span></span>

## <a name="microsoftazurekeyvaultextensions-nuget"></a><span data-ttu-id="ba9c7-162">Microsoft.Azure.KeyVault.Extensions NuGet</span><span class="sxs-lookup"><span data-stu-id="ba9c7-162">Microsoft.Azure.KeyVault.Extensions NuGet</span></span>

* <span data-ttu-id="ba9c7-163">balíček Hello příliš rozdělen**Microsoft.Azure.KeyVault.Extensions** a **Microsoft.Azure.KeyVault.Cryptography** pro hello kryptografické operace.</span><span class="sxs-lookup"><span data-stu-id="ba9c7-163">hello package is broken up too**Microsoft.Azure.KeyVault.Extensions** and **Microsoft.Azure.KeyVault.Cryptography** for hello cryptography operations.</span></span>

