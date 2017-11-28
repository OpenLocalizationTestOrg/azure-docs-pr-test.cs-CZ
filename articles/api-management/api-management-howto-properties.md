---
title: "Vlastnosti toouse aaaHow v zásadách Azure API Management"
description: "Zjistěte, jak toouse vlastnosti zásad Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 1ff096deeb97543b48dcf1f40be9dbfcbcd09542
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-properties-in-azure-api-management-policies"></a><span data-ttu-id="22d46-103">Jak toouse vlastnosti zásad Azure API Management</span><span class="sxs-lookup"><span data-stu-id="22d46-103">How toouse properties in Azure API Management policies</span></span>
<span data-ttu-id="22d46-104">Zásady služby API Management jsou vynikající funkcí hello systému, který povolí hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace.</span><span class="sxs-lookup"><span data-stu-id="22d46-104">API Management policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="22d46-105">Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="22d46-105">Policies are a collection of statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="22d46-106">Příkazy zásad se dá vytvořit pomocí literálu textové hodnoty, výrazy zásad a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="22d46-106">Policy statements can be constructed using literal text values, policy expressions, and properties.</span></span> 

<span data-ttu-id="22d46-107">Každá instance služby API Management má vlastnosti kolekci dvojic klíč/hodnota, které jsou globální toohello instance služby.</span><span class="sxs-lookup"><span data-stu-id="22d46-107">Each API Management service instance has a properties collection of key/value pairs that are global toohello service instance.</span></span> <span data-ttu-id="22d46-108">Tyto vlastnosti lze použít toomanage konstantní řetězcové hodnoty ve všech konfigurací rozhraní API a zásady.</span><span class="sxs-lookup"><span data-stu-id="22d46-108">These properties can be used toomanage constant string values across all API configuration and policies.</span></span> <span data-ttu-id="22d46-109">Každá vlastnost má následující atributy hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-109">Each property has hello following attributes.</span></span>

| <span data-ttu-id="22d46-110">Atribut</span><span class="sxs-lookup"><span data-stu-id="22d46-110">Attribute</span></span> | <span data-ttu-id="22d46-111">Typ</span><span class="sxs-lookup"><span data-stu-id="22d46-111">Type</span></span> | <span data-ttu-id="22d46-112">Popis</span><span class="sxs-lookup"><span data-stu-id="22d46-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="22d46-113">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="22d46-113">Name</span></span> |<span data-ttu-id="22d46-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="22d46-114">string</span></span> |<span data-ttu-id="22d46-115">Hello název vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-115">hello name of hello property.</span></span> <span data-ttu-id="22d46-116">Může obsahovat pouze písmena, číslice, období, pomlčky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="22d46-116">It may contain only letters, digits, period, dash, and underscore characters.</span></span> |
| <span data-ttu-id="22d46-117">Hodnota</span><span class="sxs-lookup"><span data-stu-id="22d46-117">Value</span></span> |<span data-ttu-id="22d46-118">Řetězec</span><span class="sxs-lookup"><span data-stu-id="22d46-118">string</span></span> |<span data-ttu-id="22d46-119">Hello hodnota vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-119">hello value of hello property.</span></span> <span data-ttu-id="22d46-120">Se nesmí být prázdný a skládat jenom z prázdných znaků.</span><span class="sxs-lookup"><span data-stu-id="22d46-120">It may not be empty or consist only of whitespace.</span></span> |
| <span data-ttu-id="22d46-121">Tajný kód</span><span class="sxs-lookup"><span data-stu-id="22d46-121">Secret</span></span> |<span data-ttu-id="22d46-122">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="22d46-122">boolean</span></span> |<span data-ttu-id="22d46-123">Určuje, zda hodnota hello je tajný klíč a zašifrovat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="22d46-123">Determines whether hello value is a secret and should be encrypted or not.</span></span> |
| <span data-ttu-id="22d46-124">Značky</span><span class="sxs-lookup"><span data-stu-id="22d46-124">Tags</span></span> |<span data-ttu-id="22d46-125">Pole řetězců</span><span class="sxs-lookup"><span data-stu-id="22d46-125">array of string</span></span> |<span data-ttu-id="22d46-126">Volitelné značky, pokud je zadaný, může být seznam vlastností použitých toofilter hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-126">Optional tags that when provided can be used toofilter hello property list.</span></span> |

<span data-ttu-id="22d46-127">Vlastnosti jsou nastavené v portálu vydavatele hello na hello **vlastnosti** kartě. V následujícím příkladu hello jsou tři vlastnosti nastavené.</span><span class="sxs-lookup"><span data-stu-id="22d46-127">Properties are configured in hello publisher portal on hello **Properties** tab. In hello following example, three properties are configured.</span></span>

![Vlastnosti][api-management-properties]

<span data-ttu-id="22d46-129">Vlastnost hodnoty mohou obsahovat řetězcové literály a [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span><span class="sxs-lookup"><span data-stu-id="22d46-129">Property values can contain literal strings and [policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span></span> <span data-ttu-id="22d46-130">Hello následující tabulka ukazuje hello předchozí tři ukázkové vlastnosti a jejich atributy.</span><span class="sxs-lookup"><span data-stu-id="22d46-130">hello following table shows hello previous three sample properties and their attributes.</span></span> <span data-ttu-id="22d46-131">Hello hodnota `ExpressionProperty` je hello výraz zásad, která vrací řetězec obsahující aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="22d46-131">hello value of `ExpressionProperty` is a policy expression that returns a string containing hello current date and time.</span></span> <span data-ttu-id="22d46-132">Hello vlastnost `ContosoHeaderValue` je označena jako tajný klíč, takže jeho hodnota se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="22d46-132">hello property `ContosoHeaderValue` is marked as a secret, so its value is not displayed.</span></span>

| <span data-ttu-id="22d46-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="22d46-133">Name</span></span> | <span data-ttu-id="22d46-134">Hodnota</span><span class="sxs-lookup"><span data-stu-id="22d46-134">Value</span></span> | <span data-ttu-id="22d46-135">Tajný kód</span><span class="sxs-lookup"><span data-stu-id="22d46-135">Secret</span></span> | <span data-ttu-id="22d46-136">Značky</span><span class="sxs-lookup"><span data-stu-id="22d46-136">Tags</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="22d46-137">ContosoHeader</span><span class="sxs-lookup"><span data-stu-id="22d46-137">ContosoHeader</span></span> |<span data-ttu-id="22d46-138">trackingId</span><span class="sxs-lookup"><span data-stu-id="22d46-138">TrackingId</span></span> |<span data-ttu-id="22d46-139">False</span><span class="sxs-lookup"><span data-stu-id="22d46-139">False</span></span> |<span data-ttu-id="22d46-140">Contoso</span><span class="sxs-lookup"><span data-stu-id="22d46-140">Contoso</span></span> |
| <span data-ttu-id="22d46-141">ContosoHeaderValue</span><span class="sxs-lookup"><span data-stu-id="22d46-141">ContosoHeaderValue</span></span> |<span data-ttu-id="22d46-142">••••••••••••••••••••••</span><span class="sxs-lookup"><span data-stu-id="22d46-142">••••••••••••••••••••••</span></span> |<span data-ttu-id="22d46-143">True</span><span class="sxs-lookup"><span data-stu-id="22d46-143">True</span></span> |<span data-ttu-id="22d46-144">Contoso</span><span class="sxs-lookup"><span data-stu-id="22d46-144">Contoso</span></span> |
| <span data-ttu-id="22d46-145">ExpressionProperty</span><span class="sxs-lookup"><span data-stu-id="22d46-145">ExpressionProperty</span></span> |<span data-ttu-id="22d46-146">@(DateTime.Now.ToString())</span><span class="sxs-lookup"><span data-stu-id="22d46-146">@(DateTime.Now.ToString())</span></span> |<span data-ttu-id="22d46-147">False</span><span class="sxs-lookup"><span data-stu-id="22d46-147">False</span></span> | |

## <a name="toouse-a-property"></a><span data-ttu-id="22d46-148">toouse vlastnost</span><span class="sxs-lookup"><span data-stu-id="22d46-148">toouse a property</span></span>
<span data-ttu-id="22d46-149">jako název vlastnosti hello místní uvnitř dvojitých páru složených závorek toouse vlastnost v zásadách, `{{ContosoHeader}}`, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-149">toouse a property in a policy, place hello property name inside a double pair of braces like `{{ContosoHeader}}`, as shown in hello following example.</span></span>

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

<span data-ttu-id="22d46-150">V tomto příkladu `ContosoHeader` slouží jako název hello hlavičky v `set-header` zásady, a `ContosoHeaderValue` slouží jako hello hodnotu této hlavičky.</span><span class="sxs-lookup"><span data-stu-id="22d46-150">In this example, `ContosoHeader` is used as hello name of a header in a `set-header` policy, and `ContosoHeaderValue` is used as hello value of that header.</span></span> <span data-ttu-id="22d46-151">Pokud tato zásada se vyhodnotí během požadavku nebo odpovědi toohello API Management bránu, `{{ContosoHeader}}` a `{{ContosoHeaderValue}}` nahradí se jejich hodnoty odpovídající vlastnost.</span><span class="sxs-lookup"><span data-stu-id="22d46-151">When this policy is evaluated during a request or response toohello API Management gateway, `{{ContosoHeader}}` and `{{ContosoHeaderValue}}` are replaced with their respective property values.</span></span>

<span data-ttu-id="22d46-152">Vlastnosti lze použít jako hodnoty element jako v předchozím příkladu hello nebo dokončení atributu, ale můžete také měly být vložen do nebo kombinaci s část výrazu literálovou, jak ukazuje následující příklad hello:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span><span class="sxs-lookup"><span data-stu-id="22d46-152">Properties can be used as complete attribute or element values as shown in hello previous example, but they can also be inserted into or combined with part of a literal text expression as shown in hello following example: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span></span>

<span data-ttu-id="22d46-153">Vlastnosti může také obsahovat výrazy zásad.</span><span class="sxs-lookup"><span data-stu-id="22d46-153">Properties can also contain policy expressions.</span></span> <span data-ttu-id="22d46-154">V následujícím příkladu hello, hello `ExpressionProperty` se používá.</span><span class="sxs-lookup"><span data-stu-id="22d46-154">In hello following example, hello `ExpressionProperty` is used.</span></span>

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

<span data-ttu-id="22d46-155">Pokud tato zásada je vyhodnocena, `{{ExpressionProperty}}` se nahradí jeho hodnota: `@(DateTime.Now.ToString())`.</span><span class="sxs-lookup"><span data-stu-id="22d46-155">When this policy is evaluated, `{{ExpressionProperty}}` is replaced with its value: `@(DateTime.Now.ToString())`.</span></span> <span data-ttu-id="22d46-156">Vzhledem k tomu, že je hodnota hello výraz zásady, hello výraz vyhodnocen a zásad hello pokračuje v jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="22d46-156">Since hello value is a policy expression, hello expression is evaluated and hello policy proceeds with its execution.</span></span>

<span data-ttu-id="22d46-157">Můžete to zjistit otestovat v portálu pro vývojáře hello voláním operace, která má zásady s vlastností v oboru.</span><span class="sxs-lookup"><span data-stu-id="22d46-157">You can test this out in hello developer portal by calling an operation that has a policy with properties in scope.</span></span> <span data-ttu-id="22d46-158">V následujícím příkladu hello, se nazývá operace hello dva předchozí příklad `set-header` zásady s vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="22d46-158">In hello following example, an operation is called with hello two previous example `set-header` policies with properties.</span></span> <span data-ttu-id="22d46-159">Všimněte si, že hello odpovědi obsahuje dva vlastní hlavičky, které byly nakonfigurovány pomocí vlastnosti zásad.</span><span class="sxs-lookup"><span data-stu-id="22d46-159">Note that hello response contains two custom headers that were configured using policies with properties.</span></span>

![Portál pro vývojáře][api-management-send-results]

<span data-ttu-id="22d46-161">Pokud si prohlédnete hello [trasování API Inspector](api-management-howto-api-inspector.md) pro volání, která zahrnuje hello dva předchozí ukázkové zásady s vlastnostmi, uvidíte hello dva `set-header` zásady s hodnotami vlastností hello vložit a také výraz zásady hello vyhodnocení pro hello vlastnost, která obsahovala výraz zásady hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-161">If you look at hello [API Inspector trace](api-management-howto-api-inspector.md) for a call that includes hello two previous sample policies with properties, you can see hello two `set-header` policies with hello property values inserted as well as hello policy expression evaluation for hello property that contained hello policy expression.</span></span>

![Trasování API Inspector][api-management-api-inspector-trace]

<span data-ttu-id="22d46-163">Všimněte si, že při hodnot vlastností mohou obsahovat výrazy zásad, nesmí obsahovat hodnoty vlastností, ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="22d46-163">Note that while property values can contain policy expressions, property values can't contain other properties.</span></span> <span data-ttu-id="22d46-164">Pokud text obsahující odkaz na vlastnost se používá pro hodnotu vlastnosti, jako `Property value text {{MyProperty}}`, že odkaz na vlastnost nebude nahrazen a bude součástí hello hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="22d46-164">If text containing a property reference is used for a property value, such as `Property value text {{MyProperty}}`, that property reference won't be replaced and will be included as part of hello property value.</span></span>

## <a name="toocreate-a-property"></a><span data-ttu-id="22d46-165">toocreate vlastnost</span><span class="sxs-lookup"><span data-stu-id="22d46-165">toocreate a property</span></span>
<span data-ttu-id="22d46-166">Klikněte na tlačítko toocreate vlastnost, **přidat vlastnost** na hello **vlastnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="22d46-166">toocreate a property, click **Add property** on hello **Properties** tab.</span></span>

![Přidat vlastnost][api-management-properties-add-property-menu]

<span data-ttu-id="22d46-168">**Název** a **hodnotu** jsou požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="22d46-168">**Name** and **Value** are required values.</span></span> <span data-ttu-id="22d46-169">Pokud hodnota této vlastnosti je tajný klíč, zkontrolujte hello **Toto je tajný klíč** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="22d46-169">If this property value is a secret, check hello **This is a secret** checkbox.</span></span> <span data-ttu-id="22d46-170">Zadejte jeden nebo více toohelp volitelné značky s uspořádání vlastnosti a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="22d46-170">Enter one or more optional tags toohelp with organizing your properties, and click **Save**.</span></span>

![Přidat vlastnost][api-management-properties-add-property]

<span data-ttu-id="22d46-172">Při uložení novou vlastnost hello **vyhledávání vlastnost** textové pole se vyplní hello jméno hello nové vlastnosti a zobrazí se nové vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-172">When a new property is saved, hello **Search property** textbox is populated with hello name of hello new property and hello new property is displayed.</span></span> <span data-ttu-id="22d46-173">Vymazat všechny vlastnosti toodisplay hello **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="22d46-173">toodisplay all properties, clear hello **Search property** textbox and press enter.</span></span>

![Vlastnosti][api-management-properties-property-saved]

<span data-ttu-id="22d46-175">Informace o vytvoření vlastnosti pomocí hello REST API najdete v tématu [vytvoření vlastnosti pomocí hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span><span class="sxs-lookup"><span data-stu-id="22d46-175">For information on creating a property using hello REST API, see [Create a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span></span>

## <a name="tooedit-a-property"></a><span data-ttu-id="22d46-176">tooedit vlastnost</span><span class="sxs-lookup"><span data-stu-id="22d46-176">tooedit a property</span></span>
<span data-ttu-id="22d46-177">Klikněte na tlačítko tooedit vlastnost, **upravit** vedle tooedit vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-177">tooedit a property, click **Edit** beside hello property tooedit.</span></span>

![Upravit vlastnost][api-management-properties-edit]

<span data-ttu-id="22d46-179">Proveďte požadované změny a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="22d46-179">Make any desired changes, and click **Save**.</span></span> <span data-ttu-id="22d46-180">Pokud změníte název vlastnosti hello, všechny zásady, které odkazují na tuto vlastnost se automaticky aktualizují toouse hello nový název.</span><span class="sxs-lookup"><span data-stu-id="22d46-180">If you change hello property name, any policies that reference that property are automatically updated toouse hello new name.</span></span>

![Upravit vlastnost][api-management-properties-edit-property]

<span data-ttu-id="22d46-182">Informace o úpravách vlastnosti pomocí hello REST API najdete v tématu [upravit vlastnosti pomocí hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span><span class="sxs-lookup"><span data-stu-id="22d46-182">For information on editing a property using hello REST API, see [Edit a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span></span>

## <a name="toodelete-a-property"></a><span data-ttu-id="22d46-183">toodelete vlastnost</span><span class="sxs-lookup"><span data-stu-id="22d46-183">toodelete a property</span></span>
<span data-ttu-id="22d46-184">Klikněte na tlačítko toodelete vlastnost, **odstranit** vedle toodelete vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="22d46-184">toodelete a property, click **Delete** beside hello property toodelete.</span></span>

![Odstranit vlastnost][api-management-properties-delete]

<span data-ttu-id="22d46-186">Klikněte na tlačítko **Ano, odstraňte jej** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="22d46-186">Click **Yes, delete it** tooconfirm.</span></span>

![Potvrzení odstranění][api-management-delete-confirm]

> [!IMPORTANT]
> <span data-ttu-id="22d46-188">Pokud vlastnost hello odkazuje všechny zásady, nebude možné ji toosuccessfully odstranit, dokud neodeberete hello vlastnost ze všech zásad, které ho používají.</span><span class="sxs-lookup"><span data-stu-id="22d46-188">If hello property is referenced by any policies, you will be unable toosuccessfully delete it until you remove hello property from all policies that use it.</span></span>
> 
> 

<span data-ttu-id="22d46-189">Informace o odstranění vlastnosti pomocí hello REST API najdete v tématu [odstranit vlastnost pomocí hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span><span class="sxs-lookup"><span data-stu-id="22d46-189">For information on deleting a property using hello REST API, see [Delete a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span></span>

## <a name="toosearch-and-filter-properties"></a><span data-ttu-id="22d46-190">Vlastnosti toosearch a filtru.</span><span class="sxs-lookup"><span data-stu-id="22d46-190">toosearch and filter properties</span></span>
<span data-ttu-id="22d46-191">Hello **vlastnosti** karta zahrnuje vyhledávání a filtrování toohelp možnosti spravovat vaše vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="22d46-191">hello **Properties** tab includes searching and filtering capabilities toohelp you manage your properties.</span></span> <span data-ttu-id="22d46-192">seznam vlastností hello toofilter podle názvu vlastnosti, zadejte hledaný termín v hello **vyhledávání vlastnost** textové pole.</span><span class="sxs-lookup"><span data-stu-id="22d46-192">toofilter hello property list by property name, enter a search term in hello **Search property** textbox.</span></span> <span data-ttu-id="22d46-193">Vymazat všechny vlastnosti toodisplay hello **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="22d46-193">toodisplay all properties, clear hello **Search property** textbox and press enter.</span></span>

![Search][api-management-properties-search]

<span data-ttu-id="22d46-195">seznam vlastností hello toofilter podle hodnoty značky, zadejte jednu nebo více značek do hello **filtrovat podle značky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="22d46-195">toofilter hello property list by tag values, enter one or more tags into hello **Filter by tags** textbox.</span></span> <span data-ttu-id="22d46-196">Vymazat všechny vlastnosti toodisplay hello **filtrovat podle značky** textové pole a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="22d46-196">toodisplay all properties, clear hello **Filter by tags** textbox and press enter.</span></span>

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a><span data-ttu-id="22d46-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22d46-198">Next steps</span></span>
* <span data-ttu-id="22d46-199">Další informace o práci se zásadami</span><span class="sxs-lookup"><span data-stu-id="22d46-199">Learn more about working with policies</span></span>
  * [<span data-ttu-id="22d46-200">Zásady ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="22d46-200">Policies in API Management</span></span>](api-management-howto-policies.md)
  * [<span data-ttu-id="22d46-201">referenční příručce o zásadách</span><span class="sxs-lookup"><span data-stu-id="22d46-201">Policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [<span data-ttu-id="22d46-202">Výrazy zásad</span><span class="sxs-lookup"><span data-stu-id="22d46-202">Policy expressions</span></span>](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="22d46-203">Podívejte se na video s přehledem</span><span class="sxs-lookup"><span data-stu-id="22d46-203">Watch a video overview</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

