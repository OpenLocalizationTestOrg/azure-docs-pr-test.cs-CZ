---
title: "aaaCustomize hello API Management portál pro vývojáře pomocí šablon-Azure | Microsoft Docs"
description: "Zjistěte, jak toocustomize hello portál pro vývojáře Azure API Management pomocí šablon."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: b00d5f1534e9466f30ff3920e7aae048feb8b8c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-hello-azure-api-management-developer-portal-using-templates"></a>Jak toocustomize hello portál pro vývojáře Azure API Management pomocí šablon

Existují tři základní způsoby toocustomize hello portál pro vývojáře ve službě Azure API Management:

* [Upravit obsah hello statické stránky a stránky rozložení elementů][modify-content-layout]
* [Styly hello aktualizace používané pro prvky na stránce přes portál pro vývojáře hello][customize-styles]
* [Úprava šablony hello používá stránek generovaných hello portál] [ portal-templates] (popsáno v této příručce)

Šablony jsou použité toocustomize hello obsahu portálu stránek generována developer (např. dokumentace rozhraní API, produktů, ověřování uživatelů atd.). Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a zadané sadu prostředků lokalizovaný řetězec, ikony a ovládací prvky stránky, máte flexibilitu tooconfigure hello obsah hello stránek podle svých potřeb.

## <a name="developer-portal-templates-overview"></a>Přehled šablon portálu vývojáře
Úprava šablony se provádí z hello **portál pro vývojáře** při právě přihlášeni jako správce. existuje tooget nejdřív otevřete hello portálu Azure a klikněte na tlačítko **portál vydavatele** z panelu nástrojů služby hello instanci služby API Management.

![Portál vydavatele][api-management-management-console]

Potom klikněte na **portál pro vývojáře** v pravém horním hello. 

![Nabídce portálu pro vývojáře][api-management-developer-portal-menu]

tooaccess hello šablony na portálu vývojáře, klikněte na tlačítko hello přizpůsobit ikonu na hello levém toodisplay hello přizpůsobení nabídky a klikněte na **šablony**.

![Šablony na portálu vývojáře][api-management-customize-menu]

seznam šablon Hello se zobrazí několik kategorií šablon pokrývajících hello různých stránkách v portálu pro vývojáře hello. Každé šabloně se liší, ale tooedit kroky hello je a publikovat hello změny jsou hello stejné. tooedit šablonu, klikněte na název hello hello šablony.

![Šablony na portálu vývojáře][api-management-templates-menu]

Klepnutím na šablonu přejdete vývojáře toohello portálu se stránka, která lze přizpůsobit pomocí této šablony. V tento příklad hello **seznam produktů** se šablony zobrazí. Hello **seznam produktů** ovládací prvky šablony hello oblasti indikován hello red obdélníku úvodní obrazovka. 

![Šablona seznam produktů][api-management-developer-portal-templates-overview]

Některé šablony, jako je hello **profil uživatele** šablony, přizpůsobit různé části hello stejné stránce. 

![Šablony profilu uživatele][api-management-user-profile-templates]

editor Hello ke každé šabloně portálu pro vývojáře má dvě části, které se zobrazí v hello dolní části stránky hello. na levé straně Hello zobrazí hello úpravy podokně hello šablony, a pravé straně hello hello datový model pro šablonu hello. 

Šablona Hello úpravy podokně obsahuje hello značky, které řídí hello vzhled a chování hello odpovídající stránku v portálu pro vývojáře hello. Hello značek v šabloně hello používá hello [DotLiquid](http://dotliquidmarkup.org/) syntaxe. Je jeden oblíbených editor pro DotLiquid [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Všechny změny šablony toohello během úprav se zobrazují v v reálném čase v hello prohlížeče, ale nejsou viditelné tooyour zákazníky dokud [Uložit](#to-save-a-template) a [publikování](#to-publish-a-template) hello šablony.

![Kód šablony][api-management-template]

Hello **data šablony** podokně poskytuje vodítko toohello data modelu pro hello entity, které jsou k dispozici pro použití v konkrétní šablonu. Tato příručka poskytuje zobrazením hello dynamická data, která jsou aktuálně zobrazeny v portálu pro vývojáře hello. Podokna Šablony hello můžete rozbalit kliknutím hello obdélníku v pravém horním rohu hello hello **data šablony** podokně.

![Šablona datového modelu][api-management-template-data]

V předchozím příkladu hello jsou dva zobrazí v portálu pro vývojáře hello produkty, které byly získány ze hello data zobrazená v hello **data šablony** podokně, jak ukazuje následující příklad hello.

```json
{
    "Paging": {
        "Page": 1,
        "PageSize": 10,
        "TotalItemCount": 2,
        "ShowAll": false,
        "PageCount": 1
    },
    "Filtering": {
        "Pattern": null,
        "Placeholder": "Search products"
    },
    "Products": [
        {
            "Id": "56ec64c380ed850042060001",
            "Title": "Starter",
            "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
            "Title": "Unlimited",
            "Description": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",
            "Terms": null,
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        }
    ]
}
```

Hello značek v hello **seznam produktů** šablony procesů hello výstup hello potřeby tooprovide dat pomocí iterace kolekce hello informace o toodisplay produkty a jednotlivých produktů tooeach odkaz. Poznámka: hello `<search-control>` a `<page-control>` elementů v hello značek. Tyto řídit hello zobrazení hello vyhledávání a stránkování ovládací prvky na stránce hello. `ProductsStrings|PageTitleProducts`odkaz na lokalizovaný řetězec, který obsahuje hello `h2` text záhlaví pro stránku hello. Seznam řetězcové prostředky, ovládací prvky stránky a ikony, které jsou k dispozici pro použití v šablonách portálu vývojáře najdete v tématu [API managementu vývojáře portálu šablony](api-management-developer-portal-templates-reference.md).

```html
<search-control></search-control>
<div class="row">
    <div class="col-md-9">
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
    {% if products.size > 0 %}
    <ul class="list-unstyled">
    {% for product in products %}
        <li>
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
            {{product.description}}
        </li>    
    {% endfor %}
    </ul>
    <paging-control></paging-control>
    {% else %}
    {% localized "CommonResources|NoItemsToDisplay" %}
    {% endif %}
    </div>
</div>
```

## <a name="toosave-a-template"></a>toosave šablonu
toosave šablonu, klikněte na Uložit v editoru šablony hello.

![Uložení šablony][api-management-save-template]

Uložené změny nejsou v portálu pro vývojáře hello za provozu, dokud nejsou publikovány.

## <a name="toopublish-a-template"></a>toopublish šablonu
Uložené šablony mohou být publikovány buď jednotlivě, nebo všechny společně. toopublish jednotlivé šablony, klikněte na tlačítko Publikovat v editoru šablony hello.

![Publikování šablony][api-management-publish-template]

Klikněte na tlačítko **Ano** tooconfirm a ujistěte se, hello šablony na portálu pro vývojáře hello za provozu.

![Potvrďte publikování][api-management-publish-template-confirm]

toopublish všechny aktuálně Nepublikováno šablony verze, klikněte na tlačítko **publikovat** v seznamu šablon hello. Publikování šablony jsou označeny hvězdičkou následující název šablony hello. V tomto příkladu hello **seznam produktů** a **produktu** publikování šablony.

![Publikování šablon][api-management-publish-templates]

Klikněte na tlačítko **publikovat vlastní nastavení** tooconfirm.

![Potvrďte publikování][api-management-publish-customizations]

Přehled nově publikovaných šablony jsou efektivní hned v portálu pro vývojáře hello.

## <a name="toorevert-a-template-toohello-previous-version"></a>toorevert předchozí verze toohello šablony
toorevert šablony toohello předchozí publikovanou verzi, klikněte na tlačítko Obnovit v editoru šablony hello.

![Obnovit šablonu][api-management-revert-template]

Klikněte na tlačítko **Ano** tooconfirm.

![Potvrďte][api-management-revert-template-confirm]

Hello dříve publikovanou verzi šablony je za provozu v portálu pro vývojáře hello, jakmile hello vrátit operaci byla dokončena.

## <a name="toorestore-a-template-toohello-default-version"></a>toorestore na verzi toohello výchozí šablony
Obnovení šablony tootheir výchozí verze je dvoustupňový proces. První hello šablony, je potřeba obnovit, a pak musí být publikován hello obnovit verze.

toorestore na jednu šablonu toohello výchozí verzi kliknutím na tlačítko Obnovit v editoru šablony hello.

![Obnovit šablonu][api-management-reset-template]

Klikněte na tlačítko **Ano** tooconfirm.

![Potvrďte][api-management-reset-template-confirm]

Klikněte na všechny verze výchozí šablony tootheir, toorestore **obnovit výchozí šablony** na seznam šablon hello.

![Obnovit šablony][api-management-restore-templates]

Hello obnovené šablony musí pak publikovat samostatně nebo všechny najednou podle následujících kroků hello v [toopublish šablonu](#to-publish-a-template).

## <a name="next-steps"></a>Další kroky
Referenční informace pro vývojáře portálu šablony, řetězcové prostředky, ikony a ovládací prvky stránky najdete v tématu [API managementu vývojáře portálu šablony](api-management-developer-portal-templates-reference.md).

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-management-console]: ./media/api-management-developer-portal-templates/api-management-management-console.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







