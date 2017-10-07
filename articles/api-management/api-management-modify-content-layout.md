---
title: "obsah stránky aaaModify v hello portál pro vývojáře ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak obsah stránky tooedit na portál pro vývojáře hello v Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: fd5a854e900d9512518643e593b1b59a0952621f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-content-and-layout-of-pages-on-hello-developer-portal-in-azure-api-management"></a>Upravit obsah hello a rozložení stránek na portál pro vývojáře hello v Azure API Management
Existují tři základní způsoby toocustomize hello portál pro vývojáře ve službě Azure API Management:

* [Upravit obsah hello statické stránky a stránky rozložení elementů] [ modify-content-layout] (popsáno v této příručce)
* [Styly hello aktualizace používané pro prvky na stránce přes portál pro vývojáře hello][customize-styles]
* [Úprava šablony hello používá stránek generovaných hello portál] [ portal-templates] (např. dokumentace rozhraní API, produktů, ověřování uživatelů atd.)

## <a name="page-structure"></a>Struktura stránek portálu pro vývojáře

portál pro vývojáře Hello je založena na systém správy obsahu. Hello rozvržení každé stránky je založena na sadu elementů malé stránky označuje jako pomůcky:

![Struktura stránek portálu pro vývojáře][api-management-customization-widget-structure]

Všechny widgety se dají upravovat. 
* Hello základní obsah konkrétní tooeach jednotlivých stránek nacházet v hello "Obsah" widgetů. Úpravy na stránce znamená úpravy hello obsah tohoto widgetu.
* Všechny prvky rozložení stránky jsou obsaženy s hello zbývající pomůcky. Změny provedené toothese pomůcky uplatní tooall stránky. Budou odkazované tooas "widgetů rozložení".

Každodenní stránce jeden obvykle úpravy pouze upraví hello obsahu pomůcky, který bude mít jiný obsah jednotlivých stránek.

## <a name="modify-layout-widget"></a>Úprava hello obsah widget rozložení

Obsah v rámci hello portál pro vývojáře se mění prostřednictvím portálu vydavatele hello, která je přístupná z hello portálu Azure. tooreach, klikněte na tlačítko **portál vydavatele** z panelu nástrojů služby hello instanci služby API Management.

![Portál vydavatele][api-management-management-console]

Klikněte na tlačítko tooedit hello obsah takového widgetu, **pomůcky** z hello **portál pro vývojáře** nabídky na levé straně hello. Pro tento příklad umožňuje upravte obsah hello hello záhlaví pomůcky. Vyberte hello **záhlaví** pomůcky hello seznamu.

![Záhlaví widgetů][api-management-widgets-header]

Hello obsah hlavičky hello je můžete upravovat v hello **textu** pole. Změna textu hello požadovaným způsobem a potom klikněte na **Uložit** v hello dolní části stránky hello.

Nyní byste měli mít toosee hello nové záhlaví na každé stránce portálu pro vývojáře hello.

> portál pro vývojáře hello tooopen v hello portálu vydavatele, klikněte na tlačítko **portál pro vývojáře** v horním panelu hello.
> 
> 

## <a name="edit-page-contents"></a>Upravit hello obsahu stránky

Klikněte na tlačítko toosee hello seznam všech existujících stránek s obsahem, **obsah** z hello **portál pro vývojáře** nabídky v portálu vydavatele hello.

![Správa obsahu][api-management-customization-manage-content]

Klikněte na tlačítko hello **úvodní** stránka tooedit obsah zobrazený na domovské stránce portálu pro vývojáře hello hello. Proveďte změny hello, je v případě potřeby náhled a pak klikněte na tlačítko **publikovat** toomake je viditelná tooeveryone.

> Hello Domovská stránka používá zvláštní rozložení, které umožňuje toodisplay banner v horní části hello. Tento banner není můžete upravovat hello **obsahu** části. Klikněte na tlačítko tooedit tuto hlavičku **pomůcky** z hello **portál pro vývojáře** nabídce vyberte možnost **domovskou stránku** z hello **aktuální vrstva** rozevíracího seznamu seznam a pak otevřete hello **Banner** položky v části hello **vybrané části**. jsou Hello obsah tohoto widgetu můžete upravovat stejně jako jakoukoli jinou stránku.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Styly hello aktualizace používané pro prvky na stránce přes portál pro vývojáře hello][customize-styles]
* [Úprava šablony hello používá stránek generovaných hello portál] [ portal-templates] (např. dokumentace rozhraní API, produktů, ověřování uživatelů atd.)

[Structure of developer portal pages]: #page-structure
[Modifying hello contents of a layout widget]: #modify-layout-widget
[Edit hello contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png
