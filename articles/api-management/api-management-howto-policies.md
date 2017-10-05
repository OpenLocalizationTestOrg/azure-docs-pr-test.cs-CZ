---
title: "Zásady ve službě Azure API Management | Microsoft Docs"
description: "Naučte se vytvářet, upravovat a konfigurovat zásady ve službě API Management."
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
ms.openlocfilehash: 7c1f235343074ec11c635097f2b094a10f3fe781
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="policies-in-azure-api-management"></a><span data-ttu-id="cf580-103">Zásady ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="cf580-103">Policies in Azure API Management</span></span>
<span data-ttu-id="cf580-104">Ve službě Azure API Management zásady jsou vynikající funkcí systému, která vydavatelům umožňuje měnit chování rozhraní API prostřednictvím konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cf580-104">In Azure API Management, policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="cf580-105">Zásady představují kolekci příkazů, které jsou prováděny postupně v požadavku nebo odezvy z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cf580-105">Policies are a collection of Statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="cf580-106">Oblíbené příkazy patří převod formátu XML do formátu JSON a četnosti omezení omezit množství příchozích volání od vývojáře.</span><span class="sxs-lookup"><span data-stu-id="cf580-106">Popular Statements include format conversion from XML to JSON and call rate limiting to restrict the amount of incoming calls from a developer.</span></span> <span data-ttu-id="cf580-107">Jsou dostupné ihned mnoho další zásady.</span><span class="sxs-lookup"><span data-stu-id="cf580-107">Many more policies are available out of the box.</span></span>

<span data-ttu-id="cf580-108">Najdete v článku [referenční informace o zásadách] [ Policy Reference] pro úplný seznam příkazy zásad a jejich nastavení.</span><span class="sxs-lookup"><span data-stu-id="cf580-108">See the [Policy Reference][Policy Reference] for a full list of policy statements and their settings.</span></span>

<span data-ttu-id="cf580-109">Jsou zásady použity uvnitř bránu, která je umístěna mezi příjemce rozhraní API a spravované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cf580-109">Policies are applied inside the gateway which sits between the API consumer and the managed API.</span></span> <span data-ttu-id="cf580-110">Brána přijme všechny požadavky a obvykle předává je do základního rozhraní API v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="cf580-110">The gateway receives all requests and usually forwards them unaltered to the underlying API.</span></span> <span data-ttu-id="cf580-111">Zásady ale můžete použít změny požadavek příchozí i odchozí odpovědi.</span><span class="sxs-lookup"><span data-stu-id="cf580-111">However a policy can apply changes to both the inbound request and outbound response.</span></span>

<span data-ttu-id="cf580-112">Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných zásadách API Management (pokud zásady neurčí jinak).</span><span class="sxs-lookup"><span data-stu-id="cf580-112">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="cf580-113">Některé zásady, jako [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="cf580-113">Some policies such as the [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="cf580-114">Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions].</span><span class="sxs-lookup"><span data-stu-id="cf580-114">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions].</span></span>

## <span data-ttu-id="cf580-115"><a name="scopes"></a>Postup konfigurace zásad</span><span class="sxs-lookup"><span data-stu-id="cf580-115"><a name="scopes"> </a>How to configure policies</span></span>
<span data-ttu-id="cf580-116">Zásady je možné nakonfigurovat globálně nebo na rozsah [produktu][Product], [rozhraní API] [ API] nebo [operace][Operation].</span><span class="sxs-lookup"><span data-stu-id="cf580-116">Policies can be configured globally or at the scope of a [Product][Product], [API][API] or [Operation][Operation].</span></span> <span data-ttu-id="cf580-117">Konfigurace zásad, přejděte do editoru zásad na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="cf580-117">To configure a policy, navigate to the Policies editor in the publisher portal.</span></span>

![Zásady nabídky][policies-menu]

<span data-ttu-id="cf580-119">Editor zásad se skládá z tři hlavní části: obor zásady (nahoře), definice zásady, kde jsou zásady upravovat (vlevo) a příkazy seznamu (vpravo):</span><span class="sxs-lookup"><span data-stu-id="cf580-119">The policies editor consists of three main sections: the policy scope (top), the policy definition where policies are edited (left) and the statements list (right):</span></span>

![Editor zásad][policies-editor]

<span data-ttu-id="cf580-121">Chcete-li začít, konfigurace zásad, musíte nejdřív vybrat rozsah, kdy mají zásady platit.</span><span class="sxs-lookup"><span data-stu-id="cf580-121">To begin configuring a policy you must first select the scope at which the policy should apply.</span></span> <span data-ttu-id="cf580-122">Na tomto snímku obrazovky **Starter** produktu je vybrána.</span><span class="sxs-lookup"><span data-stu-id="cf580-122">In the screenshot below the **Starter** product is selected.</span></span> <span data-ttu-id="cf580-123">Všimněte si, že odmocnina symbol vedle názvu zásady označuje, že je zásada na této úrovni již použita.</span><span class="sxs-lookup"><span data-stu-id="cf580-123">Note that the square symbol next to the policy name indicates that a policy is already applied at this level.</span></span>

![Rozsah][policies-scope]

<span data-ttu-id="cf580-125">Vzhledem k tomu, že již byla použita zásada, nastavení se zobrazí v definici zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cf580-125">Since a policy has already been applied, the configuration is shown in the definition view.</span></span>

![Konfigurace][policies-configure]

<span data-ttu-id="cf580-127">Zásady se zobrazí, jen pro čtení na první.</span><span class="sxs-lookup"><span data-stu-id="cf580-127">The policy is displayed read-only at first.</span></span> <span data-ttu-id="cf580-128">Chcete-li upravit definici kliknutím **konfigurovat zásadu** akce.</span><span class="sxs-lookup"><span data-stu-id="cf580-128">In order to edit the definition click the **Configure Policy** action.</span></span>

![Upravit][policies-edit]

<span data-ttu-id="cf580-130">Definice zásady je jednoduchý dokument XML, který popisuje posloupnost příchozí a odchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="cf580-130">The policy definition is a simple XML document that describes a sequence of inbound and outbound statements.</span></span> <span data-ttu-id="cf580-131">Soubor XML můžete upravovat přímo v okně definice.</span><span class="sxs-lookup"><span data-stu-id="cf580-131">The XML can be edited directly in the definition window.</span></span> <span data-ttu-id="cf580-132">Seznam příkazů je uvedený vpravo a příkazy pro aktuální obor jsou povolené a zvýrazní; jak je předvedeno pomocí **omezení četnosti volání** příkaz v výše uvedený snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="cf580-132">A list of statements is provided to the right and statements applicable to the current scope are enabled and highlighted; as demonstrated by the **Limit Call Rate** statement in the screenshot above.</span></span>

<span data-ttu-id="cf580-133">Kliknutím na povoleno příkaz přidá příslušný kód XML v umístění kurzoru v definici zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cf580-133">Clicking an enabled statement will add the appropriate XML at the location of the cursor in the definition view.</span></span> 

> [!NOTE]
> <span data-ttu-id="cf580-134">Pokud není povoleno zásadami, které chcete přidat, ujistěte se, že jste ve správném oboru pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="cf580-134">If the policy that you want to add is not enabled, ensure that you are in the correct scope for that policy.</span></span> <span data-ttu-id="cf580-135">Každý prohlášení o zásadách je určená pro použití v určité obory a zásad oddílů.</span><span class="sxs-lookup"><span data-stu-id="cf580-135">Each policy statement is designed for use in certain scopes and policy sections.</span></span> <span data-ttu-id="cf580-136">Chcete-li zkontrolovat zásady části a obory pro zásadu, zkontrolujte **využití** oddíl pro tuto zásadu v [referenční informace o zásadách][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="cf580-136">To review the policy sections and scopes for a policy, check the **Usage** section for that policy in the [Policy Reference][Policy Reference].</span></span>
> 
> 

<span data-ttu-id="cf580-137">Úplný seznam příkazy zásad a jejich nastavení jsou k dispozici v [referenční informace o zásadách][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="cf580-137">A full list of policy statements and their settings are available in the [Policy Reference][Policy Reference].</span></span>

<span data-ttu-id="cf580-138">Například pokud chcete přidat nové prohlášení omezit příchozí požadavky na zadané IP adresy, umístěte kurzor právě do obsah `inbound` – element XML a klikněte na tlačítko **volající omezení IP adres** příkaz.</span><span class="sxs-lookup"><span data-stu-id="cf580-138">For example, to add a new statement to restrict incoming requests to specified IP addresses, place the cursor just inside the content of the `inbound` XML element and click the **Restrict caller IPs** statement.</span></span>

![Zásady omezení][policies-restrict]

<span data-ttu-id="cf580-140">Bude přidáno fragmentu kódu XML do `inbound` element, který poskytuje pokyny o tom, jak nakonfigurovat příkaz.</span><span class="sxs-lookup"><span data-stu-id="cf580-140">This will add an XML snippet to the `inbound` element that provides guidance on how to configure the statement.</span></span>

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

<span data-ttu-id="cf580-141">Omezit příchozí požadavky a přijímat pouze ty z IP adresy 1.2.3.4 upravte soubor XML následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cf580-141">To limit inbound requests and accept only those from an IP address of 1.2.3.4 modify the XML as follows:</span></span>

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Uložit][policies-save]

<span data-ttu-id="cf580-143">Po dokončení konfigurace příkazy pro zásadu, klikněte na tlačítko **Uložit** a změny se rozšíří do rozhraní API správy brány okamžitě.</span><span class="sxs-lookup"><span data-stu-id="cf580-143">When complete configuring the statements for the policy, click **Save** and the changes will be propagated to the API Management gateway immediately.</span></span>

## <span data-ttu-id="cf580-144"><a name="sections"></a>Principy zásad konfigurace</span><span class="sxs-lookup"><span data-stu-id="cf580-144"><a name="sections"> </a>Understanding policy configuration</span></span>
<span data-ttu-id="cf580-145">Zásady je řada příkazů, které jsou spouštěny v pořadí pro požadavek a odpověď.</span><span class="sxs-lookup"><span data-stu-id="cf580-145">A policy is a series of statements that execute in order for a request and a response.</span></span> <span data-ttu-id="cf580-146">Konfigurace je správně rozdělené do `inbound`, `backend`, `outbound`, a `on-error` částech, jak je znázorněno v následující konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cf580-146">The configuration is divided appropriately into `inbound`, `backend`, `outbound`, and `on-error` sections as shown in the following configuration.</span></span>

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

<span data-ttu-id="cf580-147">Pokud dojde k chybě během zpracování požadavku, všechny zbývající kroky `inbound`, `backend`, nebo `outbound` oddíly se přeskočí a provádění přejde na příkazy v `on-error` části.</span><span class="sxs-lookup"><span data-stu-id="cf580-147">If there is an error during the processing of a request, any remaining steps in the `inbound`, `backend`, or `outbound` sections are skipped and execution jumps to the statements in the `on-error` section.</span></span> <span data-ttu-id="cf580-148">Tím, že příkazy zásad v `on-error` části Chyba můžete zkontrolovat pomocí `context.LastError` vlastnost, zkontrolovat a upravit pomocí odpovědi chyba `set-body` zásady a nakonfigurovat, co se stane, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="cf580-148">By placing policy statements in the `on-error` section you can review the error by using the `context.LastError` property, inspect and customize the error response using the `set-body` policy, and configure what happens if an error occurs.</span></span> <span data-ttu-id="cf580-149">Existují kódy chyb pro vestavěné kroky a chyby, které mohou nastat při zpracovávání příkazy zásad.</span><span class="sxs-lookup"><span data-stu-id="cf580-149">There are error codes for built-in steps and for errors that may occur during the processing of policy statements.</span></span> <span data-ttu-id="cf580-150">Další informace najdete v tématu [zpracování chyb v zásady služby API Management](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span><span class="sxs-lookup"><span data-stu-id="cf580-150">For more information, see [Error handling in API Management policies](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span></span>

<span data-ttu-id="cf580-151">Vzhledem k tomu, že zásady můžete zadat na různých úrovních (globální, produktu, rozhraní api a operace) konfigurace poskytuje způsob, jak můžete určit pořadí, ve kterém spustit příkazy definice zásady s ohledem na zásady nadřazené.</span><span class="sxs-lookup"><span data-stu-id="cf580-151">Since policies can be specified at different levels (global, product, api and operation) the configuration provides a way for you to specify the order in which the policy definition's statements execute with respect to the parent policy.</span></span> 

<span data-ttu-id="cf580-152">Zásady obory jsou vyhodnocovány v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="cf580-152">Policy scopes are evaluated in the following order.</span></span>

1. <span data-ttu-id="cf580-153">Globální obor</span><span class="sxs-lookup"><span data-stu-id="cf580-153">Global scope</span></span>
2. <span data-ttu-id="cf580-154">Rozsah produktu</span><span class="sxs-lookup"><span data-stu-id="cf580-154">Product scope</span></span>
3. <span data-ttu-id="cf580-155">Rozhraní API oboru</span><span class="sxs-lookup"><span data-stu-id="cf580-155">API scope</span></span>
4. <span data-ttu-id="cf580-156">Operace oboru</span><span class="sxs-lookup"><span data-stu-id="cf580-156">Operation scope</span></span>

<span data-ttu-id="cf580-157">Příkazy v nich vyhodnoceny podle umístění `base` elementu, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cf580-157">The statements within them are evaluated according to the placement of the `base` element, if it is present.</span></span> <span data-ttu-id="cf580-158">Globální zásady nemá nadřazený zásady a pomocí `<base>` element v ní nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="cf580-158">Global policy has no parent policy and using the `<base>` element in it has no effect.</span></span>

<span data-ttu-id="cf580-159">Například pokud máte zásadu na globální úrovni a zásada nakonfigurovaná pro rozhraní API, pak vždy, když se používá toto rozhraní API konkrétní obě zásady se použijí.</span><span class="sxs-lookup"><span data-stu-id="cf580-159">For example, if you have a policy at the global level and a policy configured for an API, then whenever that particular API is used both policies will be applied.</span></span> <span data-ttu-id="cf580-160">API Management umožňuje deterministickou řazení příkazy kombinované zásad prostřednictvím base element.</span><span class="sxs-lookup"><span data-stu-id="cf580-160">API Management allows for deterministic ordering of combined policy statements via the base element.</span></span> 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

<span data-ttu-id="cf580-161">V definici zásady příkladu výše `cross-domain` příkaz by provést před všechny vyšší zásady, které by naopak následovat `find-and-replace` zásad.</span><span class="sxs-lookup"><span data-stu-id="cf580-161">In the example policy definition above, the `cross-domain` statement would execute before any higher policies which would in turn, be followed by the `find-and-replace` policy.</span></span> 

<span data-ttu-id="cf580-162">Chcete-li zobrazit zásady v aktuálním oboru v editoru zásad, klikněte na tlačítko **přepočítat efektivní zásady pro vybraný obor**.</span><span class="sxs-lookup"><span data-stu-id="cf580-162">To see the policies in the current scope in the policy editor, click **Recalculate effective policy for selected scope**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf580-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf580-163">Next steps</span></span>
<span data-ttu-id="cf580-164">Podívejte se na následující videa na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="cf580-164">Check out following video on policy expressions.</span></span>

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
