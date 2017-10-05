---
title: "Ověřování a autorizace s Power BI Embedded"
description: "Ověřování a autorizace s Power BI Embedded"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 93027f0f5467e0b21c47bbcbc84c67cdd50b26b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a><span data-ttu-id="6940e-103">Ověřování a autorizace s Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="6940e-103">Authenticating and authorizing with Power BI Embedded</span></span>

<span data-ttu-id="6940e-104">Power BI Embedded používá služba **klíče** a **tokeny aplikací** pro ověřování a autorizaci, namísto ověřování explicitní koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="6940e-104">The Power BI Embedded service uses **Keys** and **App Tokens** for authentication and authorization, instead of explicit end-user authentication.</span></span> <span data-ttu-id="6940e-105">V tomto modelu aplikaci spravuje ověřování a autorizace pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="6940e-105">In this model, your application manages authentication and authorization for your end-users.</span></span> <span data-ttu-id="6940e-106">Pokud je to nezbytné, vaše aplikace vytvoří a odešle tokeny aplikací, která říká službě naši službu k vykreslení požadovanou sestavu.</span><span class="sxs-lookup"><span data-stu-id="6940e-106">When necessary, your app creates and sends the App Tokens that tells our service to render the requested report.</span></span> <span data-ttu-id="6940e-107">Tento návrh nevyžaduje vaše aplikace použít Azure Active Directory pro uživatele ověřování a autorizaci, i když stále můžete.</span><span class="sxs-lookup"><span data-stu-id="6940e-107">This design doesn't require your app to use Azure Active Directory for user authentication and authorization, although you still can.</span></span>

## <a name="two-ways-to-authenticate"></a><span data-ttu-id="6940e-108">Dva způsoby ověření</span><span class="sxs-lookup"><span data-stu-id="6940e-108">Two ways to authenticate</span></span>

<span data-ttu-id="6940e-109">**Klíč** -klíče můžete použít pro všechny Power BI Embedded volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="6940e-109">**Key** -  You can use keys for all Power BI Embedded REST API calls.</span></span> <span data-ttu-id="6940e-110">Klíče lze nalézt v **portál Azure** kliknutím na **všechna nastavení** a potom **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="6940e-110">The keys can be found in the **Azure portal** by clicking on **All settings** and then **Access keys**.</span></span> <span data-ttu-id="6940e-111">Vždy považovat klíče, jako by šlo heslo.</span><span class="sxs-lookup"><span data-stu-id="6940e-111">Always treat your key as if it were a password.</span></span> <span data-ttu-id="6940e-112">Tyto klíče mají oprávnění k provádění jakéhokoli rozhraní API REST volání na kolekci konkrétní pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="6940e-112">These keys have permissions to make any REST API call on a particular workspace collection.</span></span>

<span data-ttu-id="6940e-113">Při volání REST pomocí klíče, přidejte následující hlavičku autorizace:</span><span class="sxs-lookup"><span data-stu-id="6940e-113">To use a key on a REST call, add the following authorization header:</span></span>            

    Authorization: AppKey {your key}

<span data-ttu-id="6940e-114">**Tokenu aplikace** -tokeny aplikací se používají pro všechny požadavky vnoření.</span><span class="sxs-lookup"><span data-stu-id="6940e-114">**App token** - App tokens are used for all embedding requests.</span></span> <span data-ttu-id="6940e-115">Jsou navržené ke spuštění straně klienta, takže jste omezena na jednu sestavu a je vhodné nastavit čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="6940e-115">They’re designed to be run client-side, so they're restricted to a single report and it’s best practice to set an expiration time.</span></span>

<span data-ttu-id="6940e-116">Jsou aplikace tokeny JWT (JSON Web Token), který je podepsaný pomocí jedné z vašich klíčů.</span><span class="sxs-lookup"><span data-stu-id="6940e-116">App tokens are a JWT (JSON Web Token) that is signed by one of your keys.</span></span>

<span data-ttu-id="6940e-117">Váš token zabezpečení aplikace může obsahovat následující deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="6940e-117">Your app token can contain the following claims:</span></span>

| <span data-ttu-id="6940e-118">Deklarovat</span><span class="sxs-lookup"><span data-stu-id="6940e-118">Claim</span></span> | <span data-ttu-id="6940e-119">Popis</span><span class="sxs-lookup"><span data-stu-id="6940e-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6940e-120">**ver**</span><span class="sxs-lookup"><span data-stu-id="6940e-120">**ver**</span></span> |<span data-ttu-id="6940e-121">Verze tokenu aplikace.</span><span class="sxs-lookup"><span data-stu-id="6940e-121">The version of the app token.</span></span> <span data-ttu-id="6940e-122">0.2.0 je aktuální verze.</span><span class="sxs-lookup"><span data-stu-id="6940e-122">0.2.0 is the current version.</span></span> |
| <span data-ttu-id="6940e-123">**oblast**</span><span class="sxs-lookup"><span data-stu-id="6940e-123">**aud**</span></span> |<span data-ttu-id="6940e-124">Zamýšlený příjemce tokenu.</span><span class="sxs-lookup"><span data-stu-id="6940e-124">The intended recipient of the token.</span></span> <span data-ttu-id="6940e-125">Pro Power BI Embedded použití: "https://analysis.windows.net/powerbi/api".</span><span class="sxs-lookup"><span data-stu-id="6940e-125">For Power BI Embedded use: “https://analysis.windows.net/powerbi/api”.</span></span> |
| <span data-ttu-id="6940e-126">**iss**</span><span class="sxs-lookup"><span data-stu-id="6940e-126">**iss**</span></span> |<span data-ttu-id="6940e-127">Řetězec označující aplikace, který vydal token.</span><span class="sxs-lookup"><span data-stu-id="6940e-127">A string indicating the application which issued the token.</span></span> |
| <span data-ttu-id="6940e-128">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6940e-128">**type**</span></span> |<span data-ttu-id="6940e-129">Typ tokenu aplikace, který je právě vytvářena.</span><span class="sxs-lookup"><span data-stu-id="6940e-129">The type of app token which is being created.</span></span> <span data-ttu-id="6940e-130">Aktuální se podporuje jen typ **vložení**.</span><span class="sxs-lookup"><span data-stu-id="6940e-130">Current the only supported type is **embed**.</span></span> |
| <span data-ttu-id="6940e-131">**WCN**</span><span class="sxs-lookup"><span data-stu-id="6940e-131">**wcn**</span></span> |<span data-ttu-id="6940e-132">Název kolekce prostoru token se vydává.</span><span class="sxs-lookup"><span data-stu-id="6940e-132">Workspace collection name the token is being issued for.</span></span> |
| <span data-ttu-id="6940e-133">**interní databáze Windows**</span><span class="sxs-lookup"><span data-stu-id="6940e-133">**wid**</span></span> |<span data-ttu-id="6940e-134">ID pracovního prostoru token se vydává.</span><span class="sxs-lookup"><span data-stu-id="6940e-134">Workspace ID the token is being issued for.</span></span> |
| <span data-ttu-id="6940e-135">**identifikátorů RID**</span><span class="sxs-lookup"><span data-stu-id="6940e-135">**rid**</span></span> |<span data-ttu-id="6940e-136">ID sestavy token se vydává.</span><span class="sxs-lookup"><span data-stu-id="6940e-136">Report ID the token is being issued for.</span></span> |
| <span data-ttu-id="6940e-137">**uživatelské jméno** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6940e-137">**username** (optional)</span></span> |<span data-ttu-id="6940e-138">Používá se s RLS, to je řetězec, který může pomoct identifikovat uživatele při použití pravidla RLS.</span><span class="sxs-lookup"><span data-stu-id="6940e-138">Used with RLS, this is a string that can help identify the user when applying RLS rules.</span></span> |
| <span data-ttu-id="6940e-139">**role** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6940e-139">**roles** (optional)</span></span> |<span data-ttu-id="6940e-140">Řetězec obsahující rolí vyberte při použití pravidla zabezpečení na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="6940e-140">A string containing the roles to select when applying Row Level Security rules.</span></span> <span data-ttu-id="6940e-141">Pokud předávání víc než jedné role, že mají být předány jako pole palčivost.</span><span class="sxs-lookup"><span data-stu-id="6940e-141">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="6940e-142">**spojovací bod služby** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6940e-142">**scp** (optional)</span></span> |<span data-ttu-id="6940e-143">Řetězec obsahující obory oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6940e-143">A string containing the permissions scopes.</span></span> <span data-ttu-id="6940e-144">Pokud předávání víc než jedné role, že mají být předány jako pole palčivost.</span><span class="sxs-lookup"><span data-stu-id="6940e-144">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="6940e-145">**Exp** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6940e-145">**exp** (optional)</span></span> |<span data-ttu-id="6940e-146">Označuje čas, kdy vyprší platnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="6940e-146">Indicates the time in which the token will expire.</span></span> <span data-ttu-id="6940e-147">Tyto mají být předány v jako systému Unix časová razítka.</span><span class="sxs-lookup"><span data-stu-id="6940e-147">These should be passed in as Unix timestamps.</span></span> |
| <span data-ttu-id="6940e-148">**NBF** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6940e-148">**nbf** (optional)</span></span> |<span data-ttu-id="6940e-149">Označuje čas, ve kterém začíná token je platný.</span><span class="sxs-lookup"><span data-stu-id="6940e-149">Indicates the time in which the token starts being valid.</span></span> <span data-ttu-id="6940e-150">Tyto mají být předány v jako systému Unix časová razítka.</span><span class="sxs-lookup"><span data-stu-id="6940e-150">These should be passed in as Unix timestamps.</span></span> |

<span data-ttu-id="6940e-151">Ukázkový token pro aplikace bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6940e-151">A sample app token will look like this:</span></span>

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

<span data-ttu-id="6940e-152">Když dekódovat, bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="6940e-152">When decoded, it will look something like this:</span></span>

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

<span data-ttu-id="6940e-153">Nejsou k dispozici v rámci sady SDK, které usnadňují vytváření apptokens metody.</span><span class="sxs-lookup"><span data-stu-id="6940e-153">There are methods available within the SDKs that make creation of apptokens easier.</span></span> <span data-ttu-id="6940e-154">Například pro rozhraní .NET můžete prohlédnout [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) třídy a [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metody.</span><span class="sxs-lookup"><span data-stu-id="6940e-154">For example, for .NET you can look at the [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) class and the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methods.</span></span>

<span data-ttu-id="6940e-155">Pro .NET SDK, můžete se podívat do [obory](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span><span class="sxs-lookup"><span data-stu-id="6940e-155">For the .NET SDK, you can refer to [Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span></span>

## <a name="scopes"></a><span data-ttu-id="6940e-156">Obory</span><span class="sxs-lookup"><span data-stu-id="6940e-156">Scopes</span></span>

<span data-ttu-id="6940e-157">Pokud používáte vložení tokeny, můžete omezit využití prostředků, které vám umožní získat přístup k.</span><span class="sxs-lookup"><span data-stu-id="6940e-157">When using Embed tokens, you may want to restrict usage of the resources you give access to.</span></span> <span data-ttu-id="6940e-158">Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6940e-158">For this reason, you can generate a token with scoped permissions.</span></span>

<span data-ttu-id="6940e-159">Níže jsou k dispozici obory pro Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="6940e-159">The following are the available scopes for Power BI Embedded.</span></span>

|<span data-ttu-id="6940e-160">Rozsah</span><span class="sxs-lookup"><span data-stu-id="6940e-160">Scope</span></span>|<span data-ttu-id="6940e-161">Popis</span><span class="sxs-lookup"><span data-stu-id="6940e-161">Description</span></span>|
|---|---|
|<span data-ttu-id="6940e-162">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="6940e-162">Dataset.Read</span></span>|<span data-ttu-id="6940e-163">Poskytuje oprávnění ke čtení zadané sady dat.</span><span class="sxs-lookup"><span data-stu-id="6940e-163">Provides permission to read the specified dataset.</span></span>|
|<span data-ttu-id="6940e-164">Dataset.Write</span><span class="sxs-lookup"><span data-stu-id="6940e-164">Dataset.Write</span></span>|<span data-ttu-id="6940e-165">Poskytuje oprávnění k zápisu do zadané sady dat.</span><span class="sxs-lookup"><span data-stu-id="6940e-165">Provides permission to write to the specified dataset.</span></span>|
|<span data-ttu-id="6940e-166">Dataset.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="6940e-166">Dataset.ReadWrite</span></span>|<span data-ttu-id="6940e-167">Poskytuje oprávnění ke čtení a zápisu do zadaný formát datové sady.</span><span class="sxs-lookup"><span data-stu-id="6940e-167">Provides permission to read and write to the specificed dataset.</span></span>|
|<span data-ttu-id="6940e-168">Report.Read</span><span class="sxs-lookup"><span data-stu-id="6940e-168">Report.Read</span></span>|<span data-ttu-id="6940e-169">Poskytuje oprávnění k zobrazení zadané sestavy.</span><span class="sxs-lookup"><span data-stu-id="6940e-169">Provides permission to view the specified report.</span></span>|
|<span data-ttu-id="6940e-170">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="6940e-170">Report.ReadWrite</span></span>|<span data-ttu-id="6940e-171">Poskytuje oprávnění k zobrazení a úpravě zadané sestavy.</span><span class="sxs-lookup"><span data-stu-id="6940e-171">Provides permission to view and edit the specified report.</span></span>|
|<span data-ttu-id="6940e-172">Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="6940e-172">Workspace.Report.Create</span></span>|<span data-ttu-id="6940e-173">Poskytuje oprávnění k vytvoření nové sestavy v rámci zadaného pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6940e-173">Provides permission to create a new report within the specified workspace.</span></span>|
|<span data-ttu-id="6940e-174">Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="6940e-174">Workspace.Report.Copy</span></span>|<span data-ttu-id="6940e-175">Poskytuje oprávnění ke klonování existující sestavy v rámci zadaného pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6940e-175">Provides permission to clone an existing report within the specified workspace.</span></span>|

<span data-ttu-id="6940e-176">Můžete zadat více oborů pomocí mezeru mezi obory podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="6940e-176">You can supply multiple scopes by using a space between the scopes like the following.</span></span>

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

<span data-ttu-id="6940e-177">**Požadované deklarace - oborů**</span><span class="sxs-lookup"><span data-stu-id="6940e-177">**Required claims - scopes**</span></span>

<span data-ttu-id="6940e-178">spojovací bod služby: scopesClaim {scopesClaim} může být buď řetězec nebo pole řetězců, poznamenat povolené oprávnění k prostředkům prostoru (sestavy, datové sady atd.)</span><span class="sxs-lookup"><span data-stu-id="6940e-178">scp: {scopesClaim} scopesClaim can be either a string or array of strings, noting the allowed permissions to workspace resources (Report, Dataset, etc.)</span></span>

<span data-ttu-id="6940e-179">Token dekódované s obory, které jsou definovány, by vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="6940e-179">A decoded token, with scopes defined, would look similar to the following.</span></span>

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a><span data-ttu-id="6940e-180">Operace a obory</span><span class="sxs-lookup"><span data-stu-id="6940e-180">Operations and scopes</span></span>

|<span data-ttu-id="6940e-181">Operace</span><span class="sxs-lookup"><span data-stu-id="6940e-181">Operation</span></span>|<span data-ttu-id="6940e-182">Cílový prostředek</span><span class="sxs-lookup"><span data-stu-id="6940e-182">Target resource</span></span>|<span data-ttu-id="6940e-183">Token oprávnění</span><span class="sxs-lookup"><span data-stu-id="6940e-183">Token permissions</span></span>|
|---|---|---|
|<span data-ttu-id="6940e-184">Vytvoření nové sestavy založené na datovou sadu (v paměti).</span><span class="sxs-lookup"><span data-stu-id="6940e-184">Create (in-memory) a new report based on a dataset.</span></span>|<span data-ttu-id="6940e-185">Datová sada</span><span class="sxs-lookup"><span data-stu-id="6940e-185">Dataset</span></span>|<span data-ttu-id="6940e-186">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="6940e-186">Dataset.Read</span></span>|
|<span data-ttu-id="6940e-187">Vytvoření nové sestavy založené na datovou sadu (v paměti) a sestavu uložit.</span><span class="sxs-lookup"><span data-stu-id="6940e-187">Create (in-memory) a new report based on a dataset and save the report.</span></span>|<span data-ttu-id="6940e-188">Datová sada</span><span class="sxs-lookup"><span data-stu-id="6940e-188">Dataset</span></span>|<span data-ttu-id="6940e-189">* Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="6940e-189">* Dataset.Read</span></span><br><span data-ttu-id="6940e-190">* Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="6940e-190">* Workspace.Report.Create</span></span>|
|<span data-ttu-id="6940e-191">Zobrazení a prozkoumat či upravit (v paměti) existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="6940e-191">View and explore/edit (in-memory) an existing report.</span></span> <span data-ttu-id="6940e-192">Report.Read znamená Dataset.Read.</span><span class="sxs-lookup"><span data-stu-id="6940e-192">Report.Read implies Dataset.Read.</span></span> <span data-ttu-id="6940e-193">To nebude povolovat oprávnění k uložení úpravy.</span><span class="sxs-lookup"><span data-stu-id="6940e-193">This will not allow permissions to save edits.</span></span>|<span data-ttu-id="6940e-194">Sestava</span><span class="sxs-lookup"><span data-stu-id="6940e-194">Report</span></span>|<span data-ttu-id="6940e-195">Report.Read</span><span class="sxs-lookup"><span data-stu-id="6940e-195">Report.Read</span></span>|
|<span data-ttu-id="6940e-196">Upravit a uložit existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="6940e-196">Edit and save an existing report.</span></span>|<span data-ttu-id="6940e-197">Sestava</span><span class="sxs-lookup"><span data-stu-id="6940e-197">Report</span></span>|<span data-ttu-id="6940e-198">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="6940e-198">Report.ReadWrite</span></span>|
|<span data-ttu-id="6940e-199">Uložte kopii sestavy (Uložit jako).</span><span class="sxs-lookup"><span data-stu-id="6940e-199">Save a copy of a report (Save As).</span></span>|<span data-ttu-id="6940e-200">Sestava</span><span class="sxs-lookup"><span data-stu-id="6940e-200">Report</span></span>|<span data-ttu-id="6940e-201">* Report.Read</span><span class="sxs-lookup"><span data-stu-id="6940e-201">* Report.Read</span></span><br><span data-ttu-id="6940e-202">* Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="6940e-202">* Workspace.Report.Copy</span></span>|

## <a name="heres-how-the-flow-works"></a><span data-ttu-id="6940e-203">Tady je Princip toku</span><span class="sxs-lookup"><span data-stu-id="6940e-203">Here's how the flow works</span></span>
1. <span data-ttu-id="6940e-204">Zkopírujte tyto klíče rozhraní API do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6940e-204">Copy the API keys to your application.</span></span> <span data-ttu-id="6940e-205">Můžete získat klíče **portálu Azure**.</span><span class="sxs-lookup"><span data-stu-id="6940e-205">You can get the keys in **Azure Portal**.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. <span data-ttu-id="6940e-206">Token vyhodnotí deklarace identity a má čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="6940e-206">Token asserts a claim and has an expiration time.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. <span data-ttu-id="6940e-207">Získá token podepsán s klíči přístup rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6940e-207">Token gets signed with an API access keys.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. <span data-ttu-id="6940e-208">Uživatelské požadavky na Zobrazit sestavu.</span><span class="sxs-lookup"><span data-stu-id="6940e-208">User requests to view a report.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. <span data-ttu-id="6940e-209">Token je ověřit s klíči přístup rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6940e-209">Token is validated with an API access keys.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. <span data-ttu-id="6940e-210">Power BI Embedded odešle zprávu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="6940e-210">Power BI Embedded sends a report to user.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

<span data-ttu-id="6940e-211">Po **Power BI Embedded** zasílá sestav pro uživatele, uživatel může zobrazit sestavy ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6940e-211">After **Power BI Embedded** sends a report to the user, the user can view the report in your custom app.</span></span> <span data-ttu-id="6940e-212">Například, pokud jste importovali [ukázkový soubor PBIX analýza dat prodej](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), ukázkovou webovou aplikaci bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6940e-212">For example, if you imported the [Analyzing Sales Data PBIX sample](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), the sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a><span data-ttu-id="6940e-213">Viz také</span><span class="sxs-lookup"><span data-stu-id="6940e-213">See Also</span></span>

[<span data-ttu-id="6940e-214">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="6940e-214">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="6940e-215">Začínáme s ukázkou Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="6940e-215">Get started with Microsoft Power BI Embedded sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="6940e-216">Běžné scénáře Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="6940e-216">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="6940e-217">Začínáme s Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="6940e-217">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)  
[<span data-ttu-id="6940e-218">Úložiště Git PowerBI CSharp</span><span class="sxs-lookup"><span data-stu-id="6940e-218">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
<span data-ttu-id="6940e-219">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="6940e-219">More questions?</span></span> [<span data-ttu-id="6940e-220">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="6940e-220">Try the Power BI Community</span></span>](http://community.powerbi.com/)

