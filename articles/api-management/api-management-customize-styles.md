---
title: "styly aaaCustomize v hello portál pro vývojáře ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak styly toomodify hello používá pro všechny stránky v hello portál pro vývojáře ve službě Azure API Management."
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
ms.openlocfilehash: aaaa86527992ba43e64eab5fd35c7f57b573c812
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-styling-of-hello-developer-portal-in-azure-api-management"></a>Přizpůsobení stylu hello hello portál pro vývojáře ve službě Azure API Management
Existují tři základní způsoby toocustomize hello portál pro vývojáře ve službě Azure API Management:

* [Upravit obsah hello statické stránky a stránky rozložení elementů][modify-content-layout]
* [Aktualizovat hello styly používané pro prvky na stránce přes portál pro vývojáře hello] [ customize-styles] (popsáno v této příručce)
* [Úprava šablony hello používá stránek generovaných hello portál] [ portal-templates] (např. dokumentace rozhraní API, produktů, ověřování uživatelů atd.)

## <a name="change-headers-styling"></a>Změnit hello stylu prvky na stránce

Hello barvy, písma, velikosti, mezery a další související prvky stylu libovolné stránky na hello portálu se definují pomocí pravidel stylu. 

Úprava pravidla stylu hello se provádí z hello **portál pro vývojáře** při právě přihlášeni jako správce. existuje tooget nejdřív otevřete hello portálu Azure a klikněte na tlačítko **portál vydavatele** z panelu nástrojů služby hello instanci služby API Management.

![Portál vydavatele][api-management-management-console]

Potom klikněte na **portál pro vývojáře** v pravém horním hello. 

![Vývojáře portálu odkaz na portál vydavatele hello][api-management-pp-dp-link]

tooopen hello přizpůsobení panelu nástrojů přesunutím ukazatele myši nad ikonu přizpůsobení hello (nebo ho vyberte) a pak klikněte na tlačítko "styly" hello panelu nástrojů.

![Tlačítko panelu nástrojů pro přizpůsobení][api-management-customization-toolbar-button]

Existují dvě hlavní způsob úprav pravidel stylů – můžete projděte hello seznam všech pravidel stylů hello použitých na libovolném, který se zobrazí ve výchozím nastavení a podle potřeby změnit styl, nebo můžete zvolit **vybrat prvek na stránce hello** a potom Klikněte kamkoli na stránku hello toosee pouze hello styly tohoto prvku.

![Přizpůsobení panelu nástrojů][api-management-customization-toolbar]

Klikněte na tlačítko hello **vybrat prvek na stránce hello** možnost v tomto příkladu.  Prvky se zvýrazní najedete s hello myši toosignify jaké styly prvku byste začali upravovat, pokud jste klikli na. Hello přesunutí ukazatele myši hello text záhlaví hello (obvykle jste název společnosti hello zde) a pak klikněte na jeho. Sada pravidel stylů pojmenovaných a podle kategorií se zobrazí v editoru stylů hello. Každé pravidlo představuje vlastnost stylu vybraného prvku hello. Například pro výše vybraného textu záhlaví hello, hello velikost textu hello je v @font-size-h1 během hello název hello písma s alternativami @headings-font-family.

> Pokud jste obeznámeni s [bootstrap][bootstrap], tato pravidla jsou ve skutečnosti [proměnné LESS] [ LESS variables] v rámci hello motivu spuštění, který používá hello portál pro vývojáře.
> 
> 

Umožňuje změnit barvu hello hello text nadpisu. Vyberte položku hello v hello  **@headings-color**  pole a zadejte **#000000**. To je šestnáctkový kód černé barvy hello hello. Když to uděláte, uvidíte, že, zobrazí se na konci hello hello textového pole Čtvercový ukazatel barvy. Pokud je ukazatel kliknete, umožňuje výběr barvy a toochoose barvy.

![Výběr barvy][api-management-customization-toolbar-color-picker]

Jak zvýšit, ale jsou viditelné pouze tooadministrators se seznámili se změny v reálném čase. toomake tyto změny vidět tooeveryone, klikněte na tlačítko hello **publikovat** tlačítko v editoru stylů hello a potvrďte změny hello.

![Publikování nabídky][api-management-customization-toolbar-publish-form]

> toochange hello pravidel stylů použitých tooany jiného elementu na stránce hello, postupujte podle hello stejný postup jako jste to udělali pro záhlaví hello. Klikněte na tlačítko **vybrat prvek na stránce hello** z editoru stylů hello, vyberte hello element vás zajímá a úprava hello hodnoty pravidel stylu hello zobrazené na úvodní obrazovka start.
> 
> 


## <a name="next-steps"></a>Další kroky
* Zjistěte, jak toocustomize hello obsah portálu pro vývojáře stránky pomocí [šablony na portálu vývojáře](api-management-developer-portal-templates.md).

[Change hello styling of hello headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/
