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
# <a name="how-toouse-properties-in-azure-api-management-policies"></a>Jak toouse vlastnosti zásad Azure API Management
Zásady služby API Management jsou vynikající funkcí hello systému, který povolí hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API. Příkazy zásad se dá vytvořit pomocí literálu textové hodnoty, výrazy zásad a vlastnosti. 

Každá instance služby API Management má vlastnosti kolekci dvojic klíč/hodnota, které jsou globální toohello instance služby. Tyto vlastnosti lze použít toomanage konstantní řetězcové hodnoty ve všech konfigurací rozhraní API a zásady. Každá vlastnost má následující atributy hello.

| Atribut | Typ | Popis |
| --- | --- | --- |
| Name (Název) |Řetězec |Hello název vlastnosti hello. Může obsahovat pouze písmena, číslice, období, pomlčky a podtržítka. |
| Hodnota |Řetězec |Hello hodnota vlastnosti hello. Se nesmí být prázdný a skládat jenom z prázdných znaků. |
| Tajný kód |Logická hodnota |Určuje, zda hodnota hello je tajný klíč a zašifrovat nebo ne. |
| Značky |Pole řetězců |Volitelné značky, pokud je zadaný, může být seznam vlastností použitých toofilter hello. |

Vlastnosti jsou nastavené v portálu vydavatele hello na hello **vlastnosti** kartě. V následujícím příkladu hello jsou tři vlastnosti nastavené.

![Vlastnosti][api-management-properties]

Vlastnost hodnoty mohou obsahovat řetězcové literály a [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx). Hello následující tabulka ukazuje hello předchozí tři ukázkové vlastnosti a jejich atributy. Hello hodnota `ExpressionProperty` je hello výraz zásad, která vrací řetězec obsahující aktuální datum a čas. Hello vlastnost `ContosoHeaderValue` je označena jako tajný klíč, takže jeho hodnota se nezobrazí.

| Name (Název) | Hodnota | Tajný kód | Značky |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="toouse-a-property"></a>toouse vlastnost
jako název vlastnosti hello místní uvnitř dvojitých páru složených závorek toouse vlastnost v zásadách, `{{ContosoHeader}}`, jak ukazuje následující příklad hello.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

V tomto příkladu `ContosoHeader` slouží jako název hello hlavičky v `set-header` zásady, a `ContosoHeaderValue` slouží jako hello hodnotu této hlavičky. Pokud tato zásada se vyhodnotí během požadavku nebo odpovědi toohello API Management bránu, `{{ContosoHeader}}` a `{{ContosoHeaderValue}}` nahradí se jejich hodnoty odpovídající vlastnost.

Vlastnosti lze použít jako hodnoty element jako v předchozím příkladu hello nebo dokončení atributu, ale můžete také měly být vložen do nebo kombinaci s část výrazu literálovou, jak ukazuje následující příklad hello:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Vlastnosti může také obsahovat výrazy zásad. V následujícím příkladu hello, hello `ExpressionProperty` se používá.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Pokud tato zásada je vyhodnocena, `{{ExpressionProperty}}` se nahradí jeho hodnota: `@(DateTime.Now.ToString())`. Vzhledem k tomu, že je hodnota hello výraz zásady, hello výraz vyhodnocen a zásad hello pokračuje v jeho spuštění.

Můžete to zjistit otestovat v portálu pro vývojáře hello voláním operace, která má zásady s vlastností v oboru. V následujícím příkladu hello, se nazývá operace hello dva předchozí příklad `set-header` zásady s vlastnostmi. Všimněte si, že hello odpovědi obsahuje dva vlastní hlavičky, které byly nakonfigurovány pomocí vlastnosti zásad.

![Portál pro vývojáře][api-management-send-results]

Pokud si prohlédnete hello [trasování API Inspector](api-management-howto-api-inspector.md) pro volání, která zahrnuje hello dva předchozí ukázkové zásady s vlastnostmi, uvidíte hello dva `set-header` zásady s hodnotami vlastností hello vložit a také výraz zásady hello vyhodnocení pro hello vlastnost, která obsahovala výraz zásady hello.

![Trasování API Inspector][api-management-api-inspector-trace]

Všimněte si, že při hodnot vlastností mohou obsahovat výrazy zásad, nesmí obsahovat hodnoty vlastností, ostatní vlastnosti. Pokud text obsahující odkaz na vlastnost se používá pro hodnotu vlastnosti, jako `Property value text {{MyProperty}}`, že odkaz na vlastnost nebude nahrazen a bude součástí hello hodnotu vlastnosti.

## <a name="toocreate-a-property"></a>toocreate vlastnost
Klikněte na tlačítko toocreate vlastnost, **přidat vlastnost** na hello **vlastnosti** kartě.

![Přidat vlastnost][api-management-properties-add-property-menu]

**Název** a **hodnotu** jsou požadované hodnoty. Pokud hodnota této vlastnosti je tajný klíč, zkontrolujte hello **Toto je tajný klíč** zaškrtávací políčko. Zadejte jeden nebo více toohelp volitelné značky s uspořádání vlastnosti a klikněte na tlačítko **Uložit**.

![Přidat vlastnost][api-management-properties-add-property]

Při uložení novou vlastnost hello **vyhledávání vlastnost** textové pole se vyplní hello jméno hello nové vlastnosti a zobrazí se nové vlastnosti hello. Vymazat všechny vlastnosti toodisplay hello **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.

![Vlastnosti][api-management-properties-property-saved]

Informace o vytvoření vlastnosti pomocí hello REST API najdete v tématu [vytvoření vlastnosti pomocí hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="tooedit-a-property"></a>tooedit vlastnost
Klikněte na tlačítko tooedit vlastnost, **upravit** vedle tooedit vlastnost hello.

![Upravit vlastnost][api-management-properties-edit]

Proveďte požadované změny a klikněte na tlačítko **Uložit**. Pokud změníte název vlastnosti hello, všechny zásady, které odkazují na tuto vlastnost se automaticky aktualizují toouse hello nový název.

![Upravit vlastnost][api-management-properties-edit-property]

Informace o úpravách vlastnosti pomocí hello REST API najdete v tématu [upravit vlastnosti pomocí hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="toodelete-a-property"></a>toodelete vlastnost
Klikněte na tlačítko toodelete vlastnost, **odstranit** vedle toodelete vlastnost hello.

![Odstranit vlastnost][api-management-properties-delete]

Klikněte na tlačítko **Ano, odstraňte jej** tooconfirm.

![Potvrzení odstranění][api-management-delete-confirm]

> [!IMPORTANT]
> Pokud vlastnost hello odkazuje všechny zásady, nebude možné ji toosuccessfully odstranit, dokud neodeberete hello vlastnost ze všech zásad, které ho používají.
> 
> 

Informace o odstranění vlastnosti pomocí hello REST API najdete v tématu [odstranit vlastnost pomocí hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="toosearch-and-filter-properties"></a>Vlastnosti toosearch a filtru.
Hello **vlastnosti** karta zahrnuje vyhledávání a filtrování toohelp možnosti spravovat vaše vlastnosti. seznam vlastností hello toofilter podle názvu vlastnosti, zadejte hledaný termín v hello **vyhledávání vlastnost** textové pole. Vymazat všechny vlastnosti toodisplay hello **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.

![Search][api-management-properties-search]

seznam vlastností hello toofilter podle hodnoty značky, zadejte jednu nebo více značek do hello **filtrovat podle značky** textové pole. Vymazat všechny vlastnosti toodisplay hello **filtrovat podle značky** textové pole a stiskněte klávesu enter.

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a>Další kroky
* Další informace o práci se zásadami
  * [Zásady ve službě API Management](api-management-howto-policies.md)
  * [referenční příručce o zásadách](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Podívejte se na video s přehledem
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

