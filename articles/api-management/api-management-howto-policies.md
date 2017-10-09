---
title: aaaPolicies v Azure API Management | Microsoft Docs
description: "Zjistěte, jak upravit toocreate a ke konfiguraci zásad ve službě API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 9ab0f884a655004cb10c05085034df1795f512e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="policies-in-azure-api-management"></a><span data-ttu-id="ab728-103">Zásady ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="ab728-103">Policies in Azure API Management</span></span>
<span data-ttu-id="ab728-104">Ve službě Azure API Management zásady jsou vynikající funkcí hello systému, který povolí hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ab728-104">In Azure API Management, policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="ab728-105">Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ab728-105">Policies are a collection of Statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="ab728-106">Oblíbené příkazy patří převod formátu XML tooJSON a četnosti omezení toorestrict hello množství příchozích volání od vývojáře.</span><span class="sxs-lookup"><span data-stu-id="ab728-106">Popular Statements include format conversion from XML tooJSON and call rate limiting toorestrict hello amount of incoming calls from a developer.</span></span> <span data-ttu-id="ab728-107">Mnoho další zásady jsou k dispozici předinstalované hello.</span><span class="sxs-lookup"><span data-stu-id="ab728-107">Many more policies are available out of hello box.</span></span>

<span data-ttu-id="ab728-108">V tématu hello [referenční informace o zásadách] [ Policy Reference] pro úplný seznam příkazy zásad a jejich nastavení.</span><span class="sxs-lookup"><span data-stu-id="ab728-108">See hello [Policy Reference][Policy Reference] for a full list of policy statements and their settings.</span></span>

<span data-ttu-id="ab728-109">Jsou zásady použity uvnitř hello bránu, která je umístěna mezi hello příjemce rozhraní API a hello spravované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ab728-109">Policies are applied inside hello gateway which sits between hello API consumer and hello managed API.</span></span> <span data-ttu-id="ab728-110">Hello gateway přijímá všechny požadavky a obvykle předává je v nezměněném stavu toohello základní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ab728-110">hello gateway receives all requests and usually forwards them unaltered toohello underlying API.</span></span> <span data-ttu-id="ab728-111">Zásady můžete ale použít změny tooboth hello příchozí žádosti a odchozí odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ab728-111">However a policy can apply changes tooboth hello inbound request and outbound response.</span></span>

<span data-ttu-id="ab728-112">Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných hello zásad služby API Management, pokud hello zásady neurčí jinak.</span><span class="sxs-lookup"><span data-stu-id="ab728-112">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="ab728-113">Některé zásady, jako je například hello [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="ab728-113">Some policies such as hello [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="ab728-114">Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions].</span><span class="sxs-lookup"><span data-stu-id="ab728-114">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions].</span></span>

## <span data-ttu-id="ab728-115"><a name="scopes"></a>Jak tooconfigure zásady</span><span class="sxs-lookup"><span data-stu-id="ab728-115"><a name="scopes"> </a>How tooconfigure policies</span></span>
<span data-ttu-id="ab728-116">Zásady je možné nakonfigurovat globálně nebo na hello oboru [produktu][Product], [rozhraní API] [ API] nebo [operaci] [Operation].</span><span class="sxs-lookup"><span data-stu-id="ab728-116">Policies can be configured globally or at hello scope of a [Product][Product], [API][API] or [Operation][Operation].</span></span> <span data-ttu-id="ab728-117">tooconfigure zásadu, přejděte editor zásad toohello portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="ab728-117">tooconfigure a policy, navigate toohello Policies editor in hello publisher portal.</span></span>

![Zásady nabídky][policies-menu]

<span data-ttu-id="ab728-119">editor zásad Hello se skládá z tři hlavní části: hello zásady oboru (nahoře), hello definice zásady kde zásady jsou upravovat (vlevo) a příkazy hello seznamu (vpravo):</span><span class="sxs-lookup"><span data-stu-id="ab728-119">hello policies editor consists of three main sections: hello policy scope (top), hello policy definition where policies are edited (left) and hello statements list (right):</span></span>

![Editor zásad][policies-editor]

<span data-ttu-id="ab728-121">toobegin konfigurace zásad, musíte nejdřív vybrat hello oboru, na které hello mají zásady platit.</span><span class="sxs-lookup"><span data-stu-id="ab728-121">toobegin configuring a policy you must first select hello scope at which hello policy should apply.</span></span> <span data-ttu-id="ab728-122">Na snímku obrazovky hello níže hello **Starter** produktu je vybrána.</span><span class="sxs-lookup"><span data-stu-id="ab728-122">In hello screenshot below hello **Starter** product is selected.</span></span> <span data-ttu-id="ab728-123">Všimněte si, že hello odmocnina symbol další toohello název zásady vyplývá, že je zásada na této úrovni již použita.</span><span class="sxs-lookup"><span data-stu-id="ab728-123">Note that hello square symbol next toohello policy name indicates that a policy is already applied at this level.</span></span>

![Rozsah][policies-scope]

<span data-ttu-id="ab728-125">Vzhledem k tomu, že již byla použita zásada, konfigurace hello je zobrazena v zobrazení definice hello.</span><span class="sxs-lookup"><span data-stu-id="ab728-125">Since a policy has already been applied, hello configuration is shown in hello definition view.</span></span>

![Konfigurace][policies-configure]

<span data-ttu-id="ab728-127">Hello zásada se zobrazí, jen pro čtení na první.</span><span class="sxs-lookup"><span data-stu-id="ab728-127">hello policy is displayed read-only at first.</span></span> <span data-ttu-id="ab728-128">V pořadí tooedit hello definice, klikněte na tlačítko hello **konfigurovat zásadu** akce.</span><span class="sxs-lookup"><span data-stu-id="ab728-128">In order tooedit hello definition click hello **Configure Policy** action.</span></span>

![Upravit][policies-edit]

<span data-ttu-id="ab728-130">Definice zásad Hello je jednoduchý dokument XML, který popisuje posloupnost příchozí a odchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="ab728-130">hello policy definition is a simple XML document that describes a sequence of inbound and outbound statements.</span></span> <span data-ttu-id="ab728-131">Hello XML lze upravovat přímo v okně Definice hello.</span><span class="sxs-lookup"><span data-stu-id="ab728-131">hello XML can be edited directly in hello definition window.</span></span> <span data-ttu-id="ab728-132">Seznam příkazů je zadaný toohello práva a příkazy toohello použít aktuální obor jsou povolené a zvýrazní; jak je předvedeno podle hello **omezení četnosti volání** příkaz v výše uvedený snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="ab728-132">A list of statements is provided toohello right and statements applicable toohello current scope are enabled and highlighted; as demonstrated by hello **Limit Call Rate** statement in hello screenshot above.</span></span>

<span data-ttu-id="ab728-133">Kliknutím na povoleno příkaz přidá hello odpovídající XML v umístění hello hello kurzoru v hello Definice zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ab728-133">Clicking an enabled statement will add hello appropriate XML at hello location of hello cursor in hello definition view.</span></span> 

> [!NOTE]
> <span data-ttu-id="ab728-134">Pokud hello zásady, které chcete tooadd není povolena, ujistěte se, abyste byli v hello správný obor pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="ab728-134">If hello policy that you want tooadd is not enabled, ensure that you are in hello correct scope for that policy.</span></span> <span data-ttu-id="ab728-135">Každý prohlášení o zásadách je určená pro použití v určité obory a zásad oddílů.</span><span class="sxs-lookup"><span data-stu-id="ab728-135">Each policy statement is designed for use in certain scopes and policy sections.</span></span> <span data-ttu-id="ab728-136">části zásady hello tooreview a obory pro zásadu, zkontrolujte hello **využití** oddíl pro tuto zásadu v hello [referenční informace o zásadách][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="ab728-136">tooreview hello policy sections and scopes for a policy, check hello **Usage** section for that policy in hello [Policy Reference][Policy Reference].</span></span>
> 
> 

<span data-ttu-id="ab728-137">Úplný seznam příkazy zásad a jejich nastavení jsou k dispozici v hello [referenční informace o zásadách][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="ab728-137">A full list of policy statements and their settings are available in hello [Policy Reference][Policy Reference].</span></span>

<span data-ttu-id="ab728-138">Například tooadd nový příkaz toorestrict příchozí požadavky toospecified IP adresy, umístěte kurzor hello pouze uvnitř hello obsah hello `inbound` XML elementu a klikněte na tlačítko hello **volající omezení IP adres** příkaz.</span><span class="sxs-lookup"><span data-stu-id="ab728-138">For example, tooadd a new statement toorestrict incoming requests toospecified IP addresses, place hello cursor just inside hello content of hello `inbound` XML element and click hello **Restrict caller IPs** statement.</span></span>

![Zásady omezení][policies-restrict]

<span data-ttu-id="ab728-140">Bude přidáno toohello fragment kódu XML `inbound` element, který obsahuje informace o tom, jak tooconfigure hello příkaz.</span><span class="sxs-lookup"><span data-stu-id="ab728-140">This will add an XML snippet toohello `inbound` element that provides guidance on how tooconfigure hello statement.</span></span>

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

<span data-ttu-id="ab728-141">toolimit příchozí požadavky a přijmout pouze ty z IP adresy 1.2.3.4 hello XML upravit takto:</span><span class="sxs-lookup"><span data-stu-id="ab728-141">toolimit inbound requests and accept only those from an IP address of 1.2.3.4 modify hello XML as follows:</span></span>

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Uložit][policies-save]

<span data-ttu-id="ab728-143">Po dokončení konfigurace hello příkazy hello zásady, klikněte na tlačítko **Uložit** a změny hello bude brána pro správu rozhraní API šířený toohello okamžitě.</span><span class="sxs-lookup"><span data-stu-id="ab728-143">When complete configuring hello statements for hello policy, click **Save** and hello changes will be propagated toohello API Management gateway immediately.</span></span>

## <span data-ttu-id="ab728-144"><a name="sections"></a>Principy zásad konfigurace</span><span class="sxs-lookup"><span data-stu-id="ab728-144"><a name="sections"> </a>Understanding policy configuration</span></span>
<span data-ttu-id="ab728-145">Zásady je řada příkazů, které jsou spouštěny v pořadí pro požadavek a odpověď.</span><span class="sxs-lookup"><span data-stu-id="ab728-145">A policy is a series of statements that execute in order for a request and a response.</span></span> <span data-ttu-id="ab728-146">Konfigurace Hello je správně rozdělené do `inbound`, `backend`, `outbound`, a `on-error` částech, jak je znázorněno v následující konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="ab728-146">hello configuration is divided appropriately into `inbound`, `backend`, `outbound`, and `on-error` sections as shown in hello following configuration.</span></span>

```xml
<policies>
  <inbound>
    <!-- statements toobe applied toohello request go here -->
  </inbound>
  <backend>
    <!-- statements toobe applied before hello request is forwarded too
         hello backend service go here -->
  </backend>
  <outbound>
    <!-- statements toobe applied toohello response go here -->
  </outbound>
  <on-error>
    <!-- statements toobe applied if there is an error condition go here -->
  </on-error>
</policies> 
```

<span data-ttu-id="ab728-147">Pokud dojde k chybě během zpracování hello požadavku, všechny zbývající kroky v hello `inbound`, `backend`, nebo `outbound` oddíly jsou přeskočeny, a provádění skáče toohello příkazy v hello `on-error` části.</span><span class="sxs-lookup"><span data-stu-id="ab728-147">If there is an error during hello processing of a request, any remaining steps in hello `inbound`, `backend`, or `outbound` sections are skipped and execution jumps toohello statements in hello `on-error` section.</span></span> <span data-ttu-id="ab728-148">Tím, že příkazy zásad hello `on-error` části hello chyby můžete zkontrolovat pomocí hello `context.LastError` vlastnost, zkontrolovat a upravit pomocí hello hello chybové odpovědi `set-body` zásady a nakonfigurovat, co se stane, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ab728-148">By placing policy statements in hello `on-error` section you can review hello error by using hello `context.LastError` property, inspect and customize hello error response using hello `set-body` policy, and configure what happens if an error occurs.</span></span> <span data-ttu-id="ab728-149">Existují kódy chyb pro vestavěné kroky a chyby, které mohou nastat během zpracování hello příkazy zásad.</span><span class="sxs-lookup"><span data-stu-id="ab728-149">There are error codes for built-in steps and for errors that may occur during hello processing of policy statements.</span></span> <span data-ttu-id="ab728-150">Další informace najdete v tématu [zpracování chyb v zásady služby API Management](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span><span class="sxs-lookup"><span data-stu-id="ab728-150">For more information, see [Error handling in API Management policies](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span></span>

<span data-ttu-id="ab728-151">Vzhledem k tomu, že zásady můžete zadat na různých úrovních (globální, produktu, rozhraní api a operace) hello konfigurace poskytuje způsob, jak toospecify hello pořadí, ve kterém příkazy definice zásady hello provést s ohledem toohello nadřazené zásady pro vás.</span><span class="sxs-lookup"><span data-stu-id="ab728-151">Since policies can be specified at different levels (global, product, api and operation) hello configuration provides a way for you toospecify hello order in which hello policy definition's statements execute with respect toohello parent policy.</span></span> 

<span data-ttu-id="ab728-152">Zásady obory jsou vyhodnocovány v hello následující pořadí.</span><span class="sxs-lookup"><span data-stu-id="ab728-152">Policy scopes are evaluated in hello following order.</span></span>

1. <span data-ttu-id="ab728-153">Globální obor</span><span class="sxs-lookup"><span data-stu-id="ab728-153">Global scope</span></span>
2. <span data-ttu-id="ab728-154">Rozsah produktu</span><span class="sxs-lookup"><span data-stu-id="ab728-154">Product scope</span></span>
3. <span data-ttu-id="ab728-155">Rozhraní API oboru</span><span class="sxs-lookup"><span data-stu-id="ab728-155">API scope</span></span>
4. <span data-ttu-id="ab728-156">Operace oboru</span><span class="sxs-lookup"><span data-stu-id="ab728-156">Operation scope</span></span>

<span data-ttu-id="ab728-157">Hello příkazy v nich vyhodnocují podle umístění toohello hello `base` elementu, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ab728-157">hello statements within them are evaluated according toohello placement of hello `base` element, if it is present.</span></span> <span data-ttu-id="ab728-158">Globální zásady má žádné nadřazené zásady a pomocí hello `<base>` element v ní nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="ab728-158">Global policy has no parent policy and using hello `<base>` element in it has no effect.</span></span>

<span data-ttu-id="ab728-159">Například pokud máte zásadu na globální úrovni hello a zásada nakonfigurovaná pro rozhraní API, pak vždy, když se používá toto rozhraní API konkrétní obě zásady se použijí.</span><span class="sxs-lookup"><span data-stu-id="ab728-159">For example, if you have a policy at hello global level and a policy configured for an API, then whenever that particular API is used both policies will be applied.</span></span> <span data-ttu-id="ab728-160">API Management umožňuje deterministickou řazení příkazy kombinované zásad prostřednictvím hello base element.</span><span class="sxs-lookup"><span data-stu-id="ab728-160">API Management allows for deterministic ordering of combined policy statements via hello base element.</span></span> 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

<span data-ttu-id="ab728-161">V definici zásady hello v příkladu výše, hello `cross-domain` příkaz by provést před všechny vyšší zásady, které by naopak následovat hello `find-and-replace` zásad.</span><span class="sxs-lookup"><span data-stu-id="ab728-161">In hello example policy definition above, hello `cross-domain` statement would execute before any higher policies which would in turn, be followed by hello `find-and-replace` policy.</span></span> 

<span data-ttu-id="ab728-162">Klikněte na tlačítko toosee hello zásad v aktuálním oboru hello v editoru zásad hello **přepočítat efektivní zásady pro vybraný obor**.</span><span class="sxs-lookup"><span data-stu-id="ab728-162">toosee hello policies in hello current scope in hello policy editor, click **Recalculate effective policy for selected scope**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab728-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab728-163">Next steps</span></span>
<span data-ttu-id="ab728-164">Podívejte se na následující videa na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="ab728-164">Check out following video on policy expressions.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
