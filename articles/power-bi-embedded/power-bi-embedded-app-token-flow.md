---
title: aaaAuthenticating a autorizaci s Power BI Embedded
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
ms.openlocfilehash: 483ca0499e8d03584e1151d3d278c0179d4b8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a><span data-ttu-id="e5fcb-103">Ověřování a autorizace s Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e5fcb-103">Authenticating and authorizing with Power BI Embedded</span></span>

<span data-ttu-id="e5fcb-104">Hello služby Power BI Embedded používá **klíče** a **tokeny aplikací** pro ověřování a autorizaci, namísto ověřování explicitní koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-104">hello Power BI Embedded service uses **Keys** and **App Tokens** for authentication and authorization, instead of explicit end-user authentication.</span></span> <span data-ttu-id="e5fcb-105">V tomto modelu aplikaci spravuje ověřování a autorizace pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-105">In this model, your application manages authentication and authorization for your end-users.</span></span> <span data-ttu-id="e5fcb-106">Pokud je to nezbytné, vaše aplikace vytvoří a odešle tokeny hello aplikace, které informuje naše služby toorender hello požadovanou sestavu.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-106">When necessary, your app creates and sends hello App Tokens that tells our service toorender hello requested report.</span></span> <span data-ttu-id="e5fcb-107">Tento návrh nevyžaduje toouse vaše aplikace Azure Active Directory pro uživatele ověřování a autorizaci, i když stále můžete.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-107">This design doesn't require your app toouse Azure Active Directory for user authentication and authorization, although you still can.</span></span>

## <a name="two-ways-tooauthenticate"></a><span data-ttu-id="e5fcb-108">Dva způsoby, jak tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="e5fcb-108">Two ways tooauthenticate</span></span>

<span data-ttu-id="e5fcb-109">**Klíč** -klíče můžete použít pro všechny Power BI Embedded volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-109">**Key** -  You can use keys for all Power BI Embedded REST API calls.</span></span> <span data-ttu-id="e5fcb-110">Hello klíčů lze nalézt v hello **portál Azure** kliknutím na **všechna nastavení** a potom **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-110">hello keys can be found in hello **Azure portal** by clicking on **All settings** and then **Access keys**.</span></span> <span data-ttu-id="e5fcb-111">Vždy považovat klíče, jako by šlo heslo.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-111">Always treat your key as if it were a password.</span></span> <span data-ttu-id="e5fcb-112">Tyto klíče mají oprávnění toomake jakéhokoli rozhraní API REST volání na kolekci konkrétní pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-112">These keys have permissions toomake any REST API call on a particular workspace collection.</span></span>

<span data-ttu-id="e5fcb-113">toouse klíč na volání REST, přidejte následující autorizační hlavičky hello:</span><span class="sxs-lookup"><span data-stu-id="e5fcb-113">toouse a key on a REST call, add hello following authorization header:</span></span>            

    Authorization: AppKey {your key}

<span data-ttu-id="e5fcb-114">**Tokenu aplikace** -tokeny aplikací se používají pro všechny požadavky vnoření.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-114">**App token** - App tokens are used for all embedding requests.</span></span> <span data-ttu-id="e5fcb-115">Jsou to navrženou toobe spustit straně klienta, tak, aby s omezeným přístupem tooa jednotné sestavy a jeho osvědčených postupů tooset čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-115">They’re designed toobe run client-side, so they're restricted tooa single report and it’s best practice tooset an expiration time.</span></span>

<span data-ttu-id="e5fcb-116">Jsou aplikace tokeny JWT (JSON Web Token), který je podepsaný pomocí jedné z vašich klíčů.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-116">App tokens are a JWT (JSON Web Token) that is signed by one of your keys.</span></span>

<span data-ttu-id="e5fcb-117">Váš token zabezpečení aplikace může obsahovat hello následující deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="e5fcb-117">Your app token can contain hello following claims:</span></span>

| <span data-ttu-id="e5fcb-118">Deklarovat</span><span class="sxs-lookup"><span data-stu-id="e5fcb-118">Claim</span></span> | <span data-ttu-id="e5fcb-119">Popis</span><span class="sxs-lookup"><span data-stu-id="e5fcb-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e5fcb-120">**ver**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-120">**ver**</span></span> |<span data-ttu-id="e5fcb-121">verze Hello tokenu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-121">hello version of hello app token.</span></span> <span data-ttu-id="e5fcb-122">0.2.0 je aktuální verze hello.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-122">0.2.0 is hello current version.</span></span> |
| <span data-ttu-id="e5fcb-123">**oblast**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-123">**aud**</span></span> |<span data-ttu-id="e5fcb-124">Hello určené příjemce hello tokenu.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-124">hello intended recipient of hello token.</span></span> <span data-ttu-id="e5fcb-125">Pro Power BI Embedded použití: "https://analysis.windows.net/powerbi/api".</span><span class="sxs-lookup"><span data-stu-id="e5fcb-125">For Power BI Embedded use: “https://analysis.windows.net/powerbi/api”.</span></span> |
| <span data-ttu-id="e5fcb-126">**iss**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-126">**iss**</span></span> |<span data-ttu-id="e5fcb-127">Řetězec označující hello aplikace, který vydal hello token.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-127">A string indicating hello application which issued hello token.</span></span> |
| <span data-ttu-id="e5fcb-128">**Typ**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-128">**type**</span></span> |<span data-ttu-id="e5fcb-129">Hello typ tokenu aplikace, který je právě vytvářena.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-129">hello type of app token which is being created.</span></span> <span data-ttu-id="e5fcb-130">Je aktuální hello podporován pouze typ **vložení**.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-130">Current hello only supported type is **embed**.</span></span> |
| <span data-ttu-id="e5fcb-131">**WCN**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-131">**wcn**</span></span> |<span data-ttu-id="e5fcb-132">Pracovní prostor kolekce název hello token se vydává.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-132">Workspace collection name hello token is being issued for.</span></span> |
| <span data-ttu-id="e5fcb-133">**interní databáze Windows**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-133">**wid**</span></span> |<span data-ttu-id="e5fcb-134">Token hello ID pracovního prostoru se vydává.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-134">Workspace ID hello token is being issued for.</span></span> |
| <span data-ttu-id="e5fcb-135">**identifikátorů RID**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-135">**rid**</span></span> |<span data-ttu-id="e5fcb-136">Sestava ID hello token se vydává.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-136">Report ID hello token is being issued for.</span></span> |
| <span data-ttu-id="e5fcb-137">**uživatelské jméno** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="e5fcb-137">**username** (optional)</span></span> |<span data-ttu-id="e5fcb-138">Používá se s RLS, to je řetězec, který vám pomůže zjistit hello uživatele při použití pravidla RLS.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-138">Used with RLS, this is a string that can help identify hello user when applying RLS rules.</span></span> |
| <span data-ttu-id="e5fcb-139">**role** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="e5fcb-139">**roles** (optional)</span></span> |<span data-ttu-id="e5fcb-140">Řetězec obsahující hello role tooselect při použití pravidla zabezpečení na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-140">A string containing hello roles tooselect when applying Row Level Security rules.</span></span> <span data-ttu-id="e5fcb-141">Pokud předávání víc než jedné role, že mají být předány jako pole palčivost.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-141">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="e5fcb-142">**spojovací bod služby** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="e5fcb-142">**scp** (optional)</span></span> |<span data-ttu-id="e5fcb-143">Řetězec obsahující obory oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-143">A string containing hello permissions scopes.</span></span> <span data-ttu-id="e5fcb-144">Pokud předávání víc než jedné role, že mají být předány jako pole palčivost.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-144">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="e5fcb-145">**Exp** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="e5fcb-145">**exp** (optional)</span></span> |<span data-ttu-id="e5fcb-146">Označuje hello dobu, ve které hello vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-146">Indicates hello time in which hello token will expire.</span></span> <span data-ttu-id="e5fcb-147">Tyto mají být předány v jako systému Unix časová razítka.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-147">These should be passed in as Unix timestamps.</span></span> |
| <span data-ttu-id="e5fcb-148">**NBF** (volitelné)</span><span class="sxs-lookup"><span data-stu-id="e5fcb-148">**nbf** (optional)</span></span> |<span data-ttu-id="e5fcb-149">Označuje hello dobu, ve které hello spustí token je platný.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-149">Indicates hello time in which hello token starts being valid.</span></span> <span data-ttu-id="e5fcb-150">Tyto mají být předány v jako systému Unix časová razítka.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-150">These should be passed in as Unix timestamps.</span></span> |

<span data-ttu-id="e5fcb-151">Ukázkový token pro aplikace bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="e5fcb-151">A sample app token will look like this:</span></span>

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

<span data-ttu-id="e5fcb-152">Když dekódovat, bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="e5fcb-152">When decoded, it will look something like this:</span></span>

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

<span data-ttu-id="e5fcb-153">Nejsou k dispozici v rámci hello sady SDK, které usnadňují vytváření apptokens metody.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-153">There are methods available within hello SDKs that make creation of apptokens easier.</span></span> <span data-ttu-id="e5fcb-154">Například pro rozhraní .NET můžete se podívat na hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) třídy a hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metody.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-154">For example, for .NET you can look at hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) class and hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methods.</span></span>

<span data-ttu-id="e5fcb-155">Pro hello .NET SDK, naleznete v části příliš[obory](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span><span class="sxs-lookup"><span data-stu-id="e5fcb-155">For hello .NET SDK, you can refer too[Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span></span>

## <a name="scopes"></a><span data-ttu-id="e5fcb-156">Obory</span><span class="sxs-lookup"><span data-stu-id="e5fcb-156">Scopes</span></span>

<span data-ttu-id="e5fcb-157">Pokud používáte vložení tokeny, může být vhodné toorestrict využití hello prostředků, které vám umožní získat přístup k.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-157">When using Embed tokens, you may want toorestrict usage of hello resources you give access to.</span></span> <span data-ttu-id="e5fcb-158">Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-158">For this reason, you can generate a token with scoped permissions.</span></span>

<span data-ttu-id="e5fcb-159">Hello následují hello k dispozici obory pro Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-159">hello following are hello available scopes for Power BI Embedded.</span></span>

|<span data-ttu-id="e5fcb-160">Rozsah</span><span class="sxs-lookup"><span data-stu-id="e5fcb-160">Scope</span></span>|<span data-ttu-id="e5fcb-161">Popis</span><span class="sxs-lookup"><span data-stu-id="e5fcb-161">Description</span></span>|
|---|---|
|<span data-ttu-id="e5fcb-162">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="e5fcb-162">Dataset.Read</span></span>|<span data-ttu-id="e5fcb-163">Poskytuje oprávnění tooread hello zadaný datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-163">Provides permission tooread hello specified dataset.</span></span>|
|<span data-ttu-id="e5fcb-164">Dataset.Write</span><span class="sxs-lookup"><span data-stu-id="e5fcb-164">Dataset.Write</span></span>|<span data-ttu-id="e5fcb-165">Poskytuje oprávnění toowrite toohello zadaný datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-165">Provides permission toowrite toohello specified dataset.</span></span>|
|<span data-ttu-id="e5fcb-166">Dataset.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="e5fcb-166">Dataset.ReadWrite</span></span>|<span data-ttu-id="e5fcb-167">Poskytuje oprávnění tooread a zápis toohello zadaný formát datové sady.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-167">Provides permission tooread and write toohello specificed dataset.</span></span>|
|<span data-ttu-id="e5fcb-168">Report.Read</span><span class="sxs-lookup"><span data-stu-id="e5fcb-168">Report.Read</span></span>|<span data-ttu-id="e5fcb-169">Poskytuje oprávnění tooview hello zadané sestavy.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-169">Provides permission tooview hello specified report.</span></span>|
|<span data-ttu-id="e5fcb-170">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="e5fcb-170">Report.ReadWrite</span></span>|<span data-ttu-id="e5fcb-171">Poskytuje oprávnění tooview a úpravy hello zadané sestavy.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-171">Provides permission tooview and edit hello specified report.</span></span>|
|<span data-ttu-id="e5fcb-172">Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="e5fcb-172">Workspace.Report.Create</span></span>|<span data-ttu-id="e5fcb-173">Poskytuje nové sestavy v rámci hello zadaný oprávnění toocreate pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-173">Provides permission toocreate a new report within hello specified workspace.</span></span>|
|<span data-ttu-id="e5fcb-174">Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="e5fcb-174">Workspace.Report.Copy</span></span>|<span data-ttu-id="e5fcb-175">Poskytuje oprávnění tooclone existující sestavy v rámci hello zadaný pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-175">Provides permission tooclone an existing report within hello specified workspace.</span></span>|

<span data-ttu-id="e5fcb-176">Můžete zadat více oborů pomocí mezeru mezi obory hello jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-176">You can supply multiple scopes by using a space between hello scopes like hello following.</span></span>

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

<span data-ttu-id="e5fcb-177">**Požadované deklarace - oborů**</span><span class="sxs-lookup"><span data-stu-id="e5fcb-177">**Required claims - scopes**</span></span>

<span data-ttu-id="e5fcb-178">spojovací bod služby: scopesClaim {scopesClaim} může být buď řetězec nebo pole řetězců, poznamenat hello povolené oprávnění tooworkspace prostředky (sestavy, datové sady atd.)</span><span class="sxs-lookup"><span data-stu-id="e5fcb-178">scp: {scopesClaim} scopesClaim can be either a string or array of strings, noting hello allowed permissions tooworkspace resources (Report, Dataset, etc.)</span></span>

<span data-ttu-id="e5fcb-179">Token dekódované s obory, které jsou definovány, vypadat podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-179">A decoded token, with scopes defined, would look similar toohello following.</span></span>

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

### <a name="operations-and-scopes"></a><span data-ttu-id="e5fcb-180">Operace a obory</span><span class="sxs-lookup"><span data-stu-id="e5fcb-180">Operations and scopes</span></span>

|<span data-ttu-id="e5fcb-181">Operace</span><span class="sxs-lookup"><span data-stu-id="e5fcb-181">Operation</span></span>|<span data-ttu-id="e5fcb-182">Cílový prostředek</span><span class="sxs-lookup"><span data-stu-id="e5fcb-182">Target resource</span></span>|<span data-ttu-id="e5fcb-183">Token oprávnění</span><span class="sxs-lookup"><span data-stu-id="e5fcb-183">Token permissions</span></span>|
|---|---|---|
|<span data-ttu-id="e5fcb-184">Vytvoření nové sestavy založené na datovou sadu (v paměti).</span><span class="sxs-lookup"><span data-stu-id="e5fcb-184">Create (in-memory) a new report based on a dataset.</span></span>|<span data-ttu-id="e5fcb-185">Datová sada</span><span class="sxs-lookup"><span data-stu-id="e5fcb-185">Dataset</span></span>|<span data-ttu-id="e5fcb-186">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="e5fcb-186">Dataset.Read</span></span>|
|<span data-ttu-id="e5fcb-187">Vytvoření nové sestavy založené na datovou sadu (v paměti) a hello sestavu uložit.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-187">Create (in-memory) a new report based on a dataset and save hello report.</span></span>|<span data-ttu-id="e5fcb-188">Datová sada</span><span class="sxs-lookup"><span data-stu-id="e5fcb-188">Dataset</span></span>|<span data-ttu-id="e5fcb-189">* Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="e5fcb-189">* Dataset.Read</span></span><br><span data-ttu-id="e5fcb-190">* Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="e5fcb-190">* Workspace.Report.Create</span></span>|
|<span data-ttu-id="e5fcb-191">Zobrazení a prozkoumat či upravit (v paměti) existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-191">View and explore/edit (in-memory) an existing report.</span></span> <span data-ttu-id="e5fcb-192">Report.Read znamená Dataset.Read.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-192">Report.Read implies Dataset.Read.</span></span> <span data-ttu-id="e5fcb-193">To nebude povolovat toosave úpravy oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-193">This will not allow permissions toosave edits.</span></span>|<span data-ttu-id="e5fcb-194">Sestava</span><span class="sxs-lookup"><span data-stu-id="e5fcb-194">Report</span></span>|<span data-ttu-id="e5fcb-195">Report.Read</span><span class="sxs-lookup"><span data-stu-id="e5fcb-195">Report.Read</span></span>|
|<span data-ttu-id="e5fcb-196">Upravit a uložit existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-196">Edit and save an existing report.</span></span>|<span data-ttu-id="e5fcb-197">Sestava</span><span class="sxs-lookup"><span data-stu-id="e5fcb-197">Report</span></span>|<span data-ttu-id="e5fcb-198">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="e5fcb-198">Report.ReadWrite</span></span>|
|<span data-ttu-id="e5fcb-199">Uložte kopii sestavy (Uložit jako).</span><span class="sxs-lookup"><span data-stu-id="e5fcb-199">Save a copy of a report (Save As).</span></span>|<span data-ttu-id="e5fcb-200">Sestava</span><span class="sxs-lookup"><span data-stu-id="e5fcb-200">Report</span></span>|<span data-ttu-id="e5fcb-201">* Report.Read</span><span class="sxs-lookup"><span data-stu-id="e5fcb-201">* Report.Read</span></span><br><span data-ttu-id="e5fcb-202">* Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="e5fcb-202">* Workspace.Report.Copy</span></span>|

## <a name="heres-how-hello-flow-works"></a><span data-ttu-id="e5fcb-203">Tady je Princip toku hello</span><span class="sxs-lookup"><span data-stu-id="e5fcb-203">Here's how hello flow works</span></span>
1. <span data-ttu-id="e5fcb-204">Zkopírujte aplikace tooyour klíče rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-204">Copy hello API keys tooyour application.</span></span> <span data-ttu-id="e5fcb-205">Můžete získat hello klíče **portálu Azure**.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-205">You can get hello keys in **Azure Portal**.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. <span data-ttu-id="e5fcb-206">Token vyhodnotí deklarace identity a má čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-206">Token asserts a claim and has an expiration time.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. <span data-ttu-id="e5fcb-207">Získá token podepsán s klíči přístup rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-207">Token gets signed with an API access keys.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. <span data-ttu-id="e5fcb-208">Uživatel požádá o tooview sestavy.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-208">User requests tooview a report.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. <span data-ttu-id="e5fcb-209">Token je ověřit s klíči přístup rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-209">Token is validated with an API access keys.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. <span data-ttu-id="e5fcb-210">Power BI Embedded odešle toouser sestavy.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-210">Power BI Embedded sends a report toouser.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

<span data-ttu-id="e5fcb-211">Po **Power BI Embedded** odešle uživatele toohello sestavy, hello uživatele můžete zobrazit sestavu hello ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5fcb-211">After **Power BI Embedded** sends a report toohello user, hello user can view hello report in your custom app.</span></span> <span data-ttu-id="e5fcb-212">Například, pokud jste importovali hello [ukázkový soubor PBIX analýza dat prodej](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello ukázkovou webovou aplikaci bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="e5fcb-212">For example, if you imported hello [Analyzing Sales Data PBIX sample](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a><span data-ttu-id="e5fcb-213">Viz také</span><span class="sxs-lookup"><span data-stu-id="e5fcb-213">See Also</span></span>

[<span data-ttu-id="e5fcb-214">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="e5fcb-214">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="e5fcb-215">Začínáme s ukázkou Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e5fcb-215">Get started with Microsoft Power BI Embedded sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="e5fcb-216">Běžné scénáře Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e5fcb-216">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="e5fcb-217">Začínáme s Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e5fcb-217">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)  
[<span data-ttu-id="e5fcb-218">Úložiště Git PowerBI CSharp</span><span class="sxs-lookup"><span data-stu-id="e5fcb-218">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
<span data-ttu-id="e5fcb-219">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="e5fcb-219">More questions?</span></span> [<span data-ttu-id="e5fcb-220">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="e5fcb-220">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

