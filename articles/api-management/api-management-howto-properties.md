---
title: "Použití vlastností v zásadách Azure API Management"
description: "Další informace o použití vlastnosti zásad Azure API Management."
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
ms.openlocfilehash: 3b0fe2a300038e13cc488bdb4f50f8be270ea8f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a><span data-ttu-id="94f9c-103">Použití vlastností v zásadách Azure API Management</span><span class="sxs-lookup"><span data-stu-id="94f9c-103">How to use properties in Azure API Management policies</span></span>
<span data-ttu-id="94f9c-104">Zásady služby API Management jsou vynikající funkcí systému, která vydavatelům umožňuje měnit chování rozhraní API prostřednictvím konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94f9c-104">API Management policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="94f9c-105">Zásady představují kolekci příkazů, které se postupně provádí na základě požadavku nebo odezvy z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="94f9c-105">Policies are a collection of statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="94f9c-106">Příkazy zásad se dá vytvořit pomocí literálu textové hodnoty, výrazy zásad a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94f9c-106">Policy statements can be constructed using literal text values, policy expressions, and properties.</span></span> 

<span data-ttu-id="94f9c-107">Každá instance služby API Management má vlastnosti kolekci dvojic klíč/hodnota, které jsou globální v instanci služby.</span><span class="sxs-lookup"><span data-stu-id="94f9c-107">Each API Management service instance has a properties collection of key/value pairs that are global to the service instance.</span></span> <span data-ttu-id="94f9c-108">Tyto vlastnosti můžete použít ke správě konstantní hodnoty řetězce ve všech konfigurací rozhraní API a zásady.</span><span class="sxs-lookup"><span data-stu-id="94f9c-108">These properties can be used to manage constant string values across all API configuration and policies.</span></span> <span data-ttu-id="94f9c-109">Každou vlastnost má následující atributy.</span><span class="sxs-lookup"><span data-stu-id="94f9c-109">Each property has the following attributes.</span></span>

| <span data-ttu-id="94f9c-110">Atribut</span><span class="sxs-lookup"><span data-stu-id="94f9c-110">Attribute</span></span> | <span data-ttu-id="94f9c-111">Typ</span><span class="sxs-lookup"><span data-stu-id="94f9c-111">Type</span></span> | <span data-ttu-id="94f9c-112">Popis</span><span class="sxs-lookup"><span data-stu-id="94f9c-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="94f9c-113">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="94f9c-113">Name</span></span> |<span data-ttu-id="94f9c-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="94f9c-114">string</span></span> |<span data-ttu-id="94f9c-115">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="94f9c-115">The name of the property.</span></span> <span data-ttu-id="94f9c-116">Může obsahovat pouze písmena, číslice, období, pomlčky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="94f9c-116">It may contain only letters, digits, period, dash, and underscore characters.</span></span> |
| <span data-ttu-id="94f9c-117">Hodnota</span><span class="sxs-lookup"><span data-stu-id="94f9c-117">Value</span></span> |<span data-ttu-id="94f9c-118">Řetězec</span><span class="sxs-lookup"><span data-stu-id="94f9c-118">string</span></span> |<span data-ttu-id="94f9c-119">Hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94f9c-119">The value of the property.</span></span> <span data-ttu-id="94f9c-120">Se nesmí být prázdný a skládat jenom z prázdných znaků.</span><span class="sxs-lookup"><span data-stu-id="94f9c-120">It may not be empty or consist only of whitespace.</span></span> |
| <span data-ttu-id="94f9c-121">Tajný kód</span><span class="sxs-lookup"><span data-stu-id="94f9c-121">Secret</span></span> |<span data-ttu-id="94f9c-122">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="94f9c-122">boolean</span></span> |<span data-ttu-id="94f9c-123">Určuje, zda hodnota je tajný klíč a zašifrovat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="94f9c-123">Determines whether the value is a secret and should be encrypted or not.</span></span> |
| <span data-ttu-id="94f9c-124">Značky</span><span class="sxs-lookup"><span data-stu-id="94f9c-124">Tags</span></span> |<span data-ttu-id="94f9c-125">Pole řetězců</span><span class="sxs-lookup"><span data-stu-id="94f9c-125">array of string</span></span> |<span data-ttu-id="94f9c-126">Volitelné značky, pokud je zadaný, mohou být použity k filtrování seznamu vlastností.</span><span class="sxs-lookup"><span data-stu-id="94f9c-126">Optional tags that when provided can be used to filter the property list.</span></span> |

<span data-ttu-id="94f9c-127">Vlastnosti jsou nakonfigurované na portálu vydavatele na **vlastnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="94f9c-127">Properties are configured in the publisher portal on the **Properties** tab.</span></span> <span data-ttu-id="94f9c-128">V následujícím příkladu jsou tři vlastnosti nastavené.</span><span class="sxs-lookup"><span data-stu-id="94f9c-128">In the following example, three properties are configured.</span></span>

![Vlastnosti][api-management-properties]

<span data-ttu-id="94f9c-130">Vlastnost hodnoty mohou obsahovat řetězcové literály a [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span><span class="sxs-lookup"><span data-stu-id="94f9c-130">Property values can contain literal strings and [policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span></span> <span data-ttu-id="94f9c-131">V následující tabulce jsou uvedeny předchozí tři ukázkové vlastnosti a jejich atributy.</span><span class="sxs-lookup"><span data-stu-id="94f9c-131">The following table shows the previous three sample properties and their attributes.</span></span> <span data-ttu-id="94f9c-132">Hodnota `ExpressionProperty` výrazu zásad, která vrací řetězec obsahující aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="94f9c-132">The value of `ExpressionProperty` is a policy expression that returns a string containing the current date and time.</span></span> <span data-ttu-id="94f9c-133">Vlastnost `ContosoHeaderValue` je označena jako tajný klíč, takže jeho hodnota se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="94f9c-133">The property `ContosoHeaderValue` is marked as a secret, so its value is not displayed.</span></span>

| <span data-ttu-id="94f9c-134">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="94f9c-134">Name</span></span> | <span data-ttu-id="94f9c-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="94f9c-135">Value</span></span> | <span data-ttu-id="94f9c-136">Tajný kód</span><span class="sxs-lookup"><span data-stu-id="94f9c-136">Secret</span></span> | <span data-ttu-id="94f9c-137">Značky</span><span class="sxs-lookup"><span data-stu-id="94f9c-137">Tags</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="94f9c-138">ContosoHeader</span><span class="sxs-lookup"><span data-stu-id="94f9c-138">ContosoHeader</span></span> |<span data-ttu-id="94f9c-139">trackingId</span><span class="sxs-lookup"><span data-stu-id="94f9c-139">TrackingId</span></span> |<span data-ttu-id="94f9c-140">False</span><span class="sxs-lookup"><span data-stu-id="94f9c-140">False</span></span> |<span data-ttu-id="94f9c-141">Contoso</span><span class="sxs-lookup"><span data-stu-id="94f9c-141">Contoso</span></span> |
| <span data-ttu-id="94f9c-142">ContosoHeaderValue</span><span class="sxs-lookup"><span data-stu-id="94f9c-142">ContosoHeaderValue</span></span> |<span data-ttu-id="94f9c-143">••••••••••••••••••••••</span><span class="sxs-lookup"><span data-stu-id="94f9c-143">••••••••••••••••••••••</span></span> |<span data-ttu-id="94f9c-144">True</span><span class="sxs-lookup"><span data-stu-id="94f9c-144">True</span></span> |<span data-ttu-id="94f9c-145">Contoso</span><span class="sxs-lookup"><span data-stu-id="94f9c-145">Contoso</span></span> |
| <span data-ttu-id="94f9c-146">ExpressionProperty</span><span class="sxs-lookup"><span data-stu-id="94f9c-146">ExpressionProperty</span></span> |<span data-ttu-id="94f9c-147">@(DateTime.Now.ToString())</span><span class="sxs-lookup"><span data-stu-id="94f9c-147">@(DateTime.Now.ToString())</span></span> |<span data-ttu-id="94f9c-148">False</span><span class="sxs-lookup"><span data-stu-id="94f9c-148">False</span></span> | |

## <a name="to-use-a-property"></a><span data-ttu-id="94f9c-149">Chcete-li použít vlastnost</span><span class="sxs-lookup"><span data-stu-id="94f9c-149">To use a property</span></span>
<span data-ttu-id="94f9c-150">Chcete-li použít vlastnost v zásadách, označte název vlastnosti uvnitř pár dvojité složené závorky jako `{{ContosoHeader}}`, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="94f9c-150">To use a property in a policy, place the property name inside a double pair of braces like `{{ContosoHeader}}`, as shown in the following example.</span></span>

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

<span data-ttu-id="94f9c-151">V tomto příkladu `ContosoHeader` slouží jako název záhlaví v `set-header` zásady, a `ContosoHeaderValue` slouží jako hodnotu této hlavičky.</span><span class="sxs-lookup"><span data-stu-id="94f9c-151">In this example, `ContosoHeader` is used as the name of a header in a `set-header` policy, and `ContosoHeaderValue` is used as the value of that header.</span></span> <span data-ttu-id="94f9c-152">Pokud tato zásada se vyhodnotí během požadavku nebo odpovědi k bráně správy rozhraní API `{{ContosoHeader}}` a `{{ContosoHeaderValue}}` nahradí se jejich hodnoty odpovídající vlastnost.</span><span class="sxs-lookup"><span data-stu-id="94f9c-152">When this policy is evaluated during a request or response to the API Management gateway, `{{ContosoHeader}}` and `{{ContosoHeaderValue}}` are replaced with their respective property values.</span></span>

<span data-ttu-id="94f9c-153">Vlastnosti lze použít jako dokončení atribut nebo element hodnoty, jak je znázorněno v předchozím příkladu, ale můžete také měly být vložen do nebo v kombinaci s část výrazu literálovou, jak je znázorněno v následujícím příkladu:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span><span class="sxs-lookup"><span data-stu-id="94f9c-153">Properties can be used as complete attribute or element values as shown in the previous example, but they can also be inserted into or combined with part of a literal text expression as shown in the following example: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span></span>

<span data-ttu-id="94f9c-154">Vlastnosti může také obsahovat výrazy zásad.</span><span class="sxs-lookup"><span data-stu-id="94f9c-154">Properties can also contain policy expressions.</span></span> <span data-ttu-id="94f9c-155">V následujícím příkladu `ExpressionProperty` se používá.</span><span class="sxs-lookup"><span data-stu-id="94f9c-155">In the following example, the `ExpressionProperty` is used.</span></span>

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

<span data-ttu-id="94f9c-156">Pokud tato zásada je vyhodnocena, `{{ExpressionProperty}}` se nahradí jeho hodnota: `@(DateTime.Now.ToString())`.</span><span class="sxs-lookup"><span data-stu-id="94f9c-156">When this policy is evaluated, `{{ExpressionProperty}}` is replaced with its value: `@(DateTime.Now.ToString())`.</span></span> <span data-ttu-id="94f9c-157">Vzhledem k tomu, že hodnota je výraz zásady, je tento výraz vyhodnocen a zásady pokračuje v jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="94f9c-157">Since the value is a policy expression, the expression is evaluated and the policy proceeds with its execution.</span></span>

<span data-ttu-id="94f9c-158">Tuto funkci můžete otestovat v portálu pro vývojáře voláním operace, která má zásady s vlastností v oboru.</span><span class="sxs-lookup"><span data-stu-id="94f9c-158">You can test this out in the developer portal by calling an operation that has a policy with properties in scope.</span></span> <span data-ttu-id="94f9c-159">V následujícím příkladu je volána operace dva předchozí příklad `set-header` zásady s vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="94f9c-159">In the following example, an operation is called with the two previous example `set-header` policies with properties.</span></span> <span data-ttu-id="94f9c-160">Všimněte si, že odpověď obsahuje dva vlastní hlavičky, které byly nakonfigurovány pomocí vlastnosti zásad.</span><span class="sxs-lookup"><span data-stu-id="94f9c-160">Note that the response contains two custom headers that were configured using policies with properties.</span></span>

![Portál pro vývojáře][api-management-send-results]

<span data-ttu-id="94f9c-162">Pokud si prohlédnete [trasování API Inspector](api-management-howto-api-inspector.md) pro volání, která zahrnuje předchozí dva ukázkové zásady s vlastnostmi, uvidíte dvě `set-header` zásad hodnoty vlastností vložit a také vyhodnocení výrazu zásad pro vlastnost, která obsahovala výraz zásady.</span><span class="sxs-lookup"><span data-stu-id="94f9c-162">If you look at the [API Inspector trace](api-management-howto-api-inspector.md) for a call that includes the two previous sample policies with properties, you can see the two `set-header` policies with the property values inserted as well as the policy expression evaluation for the property that contained the policy expression.</span></span>

![Trasování API Inspector][api-management-api-inspector-trace]

<span data-ttu-id="94f9c-164">Všimněte si, že při hodnot vlastností mohou obsahovat výrazy zásad, nesmí obsahovat hodnoty vlastností, ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94f9c-164">Note that while property values can contain policy expressions, property values can't contain other properties.</span></span> <span data-ttu-id="94f9c-165">Pokud text obsahující odkaz na vlastnost se používá pro hodnotu vlastnosti, jako `Property value text {{MyProperty}}`, že odkaz na vlastnost nebude nahrazen a budou zahrnuty jako součást hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94f9c-165">If text containing a property reference is used for a property value, such as `Property value text {{MyProperty}}`, that property reference won't be replaced and will be included as part of the property value.</span></span>

## <a name="to-create-a-property"></a><span data-ttu-id="94f9c-166">K vytvoření vlastnosti</span><span class="sxs-lookup"><span data-stu-id="94f9c-166">To create a property</span></span>
<span data-ttu-id="94f9c-167">Chcete-li vytvořit vlastnosti, klikněte na tlačítko **přidat vlastnost** na **vlastnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="94f9c-167">To create a property, click **Add property** on the **Properties** tab.</span></span>

![Přidat vlastnost][api-management-properties-add-property-menu]

<span data-ttu-id="94f9c-169">**Název** a **hodnotu** jsou požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="94f9c-169">**Name** and **Value** are required values.</span></span> <span data-ttu-id="94f9c-170">Pokud hodnota této vlastnosti je tajný klíč, podívejte se **Toto je tajný klíč** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="94f9c-170">If this property value is a secret, check the **This is a secret** checkbox.</span></span> <span data-ttu-id="94f9c-171">Zadejte jednu nebo více značek volitelné usnadní uspořádání vlastnosti a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="94f9c-171">Enter one or more optional tags to help with organizing your properties, and click **Save**.</span></span>

![Přidat vlastnost][api-management-properties-add-property]

<span data-ttu-id="94f9c-173">Při uložení nové vlastnosti **vyhledávání vlastnost** textové pole se vyplní název nové vlastnosti a zobrazí se nové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94f9c-173">When a new property is saved, the **Search property** textbox is populated with the name of the new property and the new property is displayed.</span></span> <span data-ttu-id="94f9c-174">Chcete-li zobrazit všechny vlastnosti, zrušte **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="94f9c-174">To display all properties, clear the **Search property** textbox and press enter.</span></span>

![Vlastnosti][api-management-properties-property-saved]

<span data-ttu-id="94f9c-176">Informace o vytvoření vlastnosti pomocí rozhraní REST API najdete v tématu [vytvoření vlastnosti pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span><span class="sxs-lookup"><span data-stu-id="94f9c-176">For information on creating a property using the REST API, see [Create a property using the REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span></span>

## <a name="to-edit-a-property"></a><span data-ttu-id="94f9c-177">Chcete-li upravit vlastnosti</span><span class="sxs-lookup"><span data-stu-id="94f9c-177">To edit a property</span></span>
<span data-ttu-id="94f9c-178">Chcete-li upravit vlastnosti, klikněte na tlačítko **upravit** vedle vlastnosti, které chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="94f9c-178">To edit a property, click **Edit** beside the property to edit.</span></span>

![Upravit vlastnost][api-management-properties-edit]

<span data-ttu-id="94f9c-180">Proveďte požadované změny a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="94f9c-180">Make any desired changes, and click **Save**.</span></span> <span data-ttu-id="94f9c-181">Pokud změníte název vlastnosti, všechny zásady, které odkazují na tuto vlastnost se automaticky aktualizují na použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="94f9c-181">If you change the property name, any policies that reference that property are automatically updated to use the new name.</span></span>

![Upravit vlastnost][api-management-properties-edit-property]

<span data-ttu-id="94f9c-183">Informace o úpravách vlastnosti pomocí rozhraní REST API najdete v tématu [upravit vlastnosti pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span><span class="sxs-lookup"><span data-stu-id="94f9c-183">For information on editing a property using the REST API, see [Edit a property using the REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span></span>

## <a name="to-delete-a-property"></a><span data-ttu-id="94f9c-184">K odstranění vlastnosti</span><span class="sxs-lookup"><span data-stu-id="94f9c-184">To delete a property</span></span>
<span data-ttu-id="94f9c-185">Chcete-li odstranit vlastnost, klikněte na tlačítko **odstranit** vedle vlastnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="94f9c-185">To delete a property, click **Delete** beside the property to delete.</span></span>

![Odstranit vlastnost][api-management-properties-delete]

<span data-ttu-id="94f9c-187">Klikněte na tlačítko **Ano, odstraňte jej** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="94f9c-187">Click **Yes, delete it** to confirm.</span></span>

![Potvrzení odstranění][api-management-delete-confirm]

> [!IMPORTANT]
> <span data-ttu-id="94f9c-189">Pokud se všechny zásady odkazuje vlastnost, nebude možné úspěšně ho odstranit, dokud neodeberete vlastnost ze všech zásad, které ho používají.</span><span class="sxs-lookup"><span data-stu-id="94f9c-189">If the property is referenced by any policies, you will be unable to successfully delete it until you remove the property from all policies that use it.</span></span>
> 
> 

<span data-ttu-id="94f9c-190">Informace o odstranění vlastnosti pomocí rozhraní REST API najdete v tématu [odstranit vlastnost pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span><span class="sxs-lookup"><span data-stu-id="94f9c-190">For information on deleting a property using the REST API, see [Delete a property using the REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span></span>

## <a name="to-search-and-filter-properties"></a><span data-ttu-id="94f9c-191">Vyhledávat a filtrovat vlastnosti</span><span class="sxs-lookup"><span data-stu-id="94f9c-191">To search and filter properties</span></span>
<span data-ttu-id="94f9c-192">**Vlastnosti** karta zahrnuje vyhledávání a filtrování funkcí, které vám pomohou při správě vaší vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94f9c-192">The **Properties** tab includes searching and filtering capabilities to help you manage your properties.</span></span> <span data-ttu-id="94f9c-193">Chcete-li filtrovat seznam vlastností podle názvu vlastnosti, zadejte hledaný termín v **vyhledávání vlastnost** textové pole.</span><span class="sxs-lookup"><span data-stu-id="94f9c-193">To filter the property list by property name, enter a search term in the **Search property** textbox.</span></span> <span data-ttu-id="94f9c-194">Chcete-li zobrazit všechny vlastnosti, zrušte **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="94f9c-194">To display all properties, clear the **Search property** textbox and press enter.</span></span>

![Search][api-management-properties-search]

<span data-ttu-id="94f9c-196">Chcete-li filtrovat seznam vlastností podle hodnoty značky, zadejte jednu nebo více značek do **filtrovat podle značky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="94f9c-196">To filter the property list by tag values, enter one or more tags into the **Filter by tags** textbox.</span></span> <span data-ttu-id="94f9c-197">Chcete-li zobrazit všechny vlastnosti, zrušte **filtrovat podle značky** textové pole a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="94f9c-197">To display all properties, clear the **Filter by tags** textbox and press enter.</span></span>

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a><span data-ttu-id="94f9c-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94f9c-199">Next steps</span></span>
* <span data-ttu-id="94f9c-200">Další informace o práci se zásadami</span><span class="sxs-lookup"><span data-stu-id="94f9c-200">Learn more about working with policies</span></span>
  * [<span data-ttu-id="94f9c-201">Zásady ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="94f9c-201">Policies in API Management</span></span>](api-management-howto-policies.md)
  * [<span data-ttu-id="94f9c-202">referenční příručce o zásadách</span><span class="sxs-lookup"><span data-stu-id="94f9c-202">Policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [<span data-ttu-id="94f9c-203">Výrazy zásad</span><span class="sxs-lookup"><span data-stu-id="94f9c-203">Policy expressions</span></span>](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="94f9c-204">Podívejte se na video s přehledem</span><span class="sxs-lookup"><span data-stu-id="94f9c-204">Watch a video overview</span></span>
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

