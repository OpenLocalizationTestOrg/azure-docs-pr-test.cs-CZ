---
title: Koncepty aaaMobile Engagementu | Microsoft Docs
description: Koncepty Azure Mobile Engagementu
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 5aa7f28c00cd641a36a6e040c6b13d802ea6ae41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-concepts"></a>Koncepty Azure Mobile Engagementu
Mobile Engagement definuje několik konceptů běžné tooall podporované platformy. Tento článek tyto koncepty stručně popisuje.

Tento článek je dobré počáteční, pokud jste nový tooMobile zapojení. Ujistěte se taky, že tooread hello dokumentaci toohello konkrétní platformu, kterou používáte, tam podrobnější charakteristiku konceptů hello popsaných v tomto článku se další podrobnosti a příklady a také možných omezení.

## <a name="devices-and-users"></a>Zařízení a uživatelé
Mobile Engagement rozpoznává uživatele tak, že pro každé zařízení generuje jedinečný identifikátor. Tento identifikátor se nazývá identifikátor zařízení hello (nebo `deviceid`). Generuje se tak, že všechny aplikace spuštěné hello stejné sdílené složky zařízení hello stejný identifikátor zařízení.

Implicitně znamená, že Mobile Engagement počítá jeden zařízení toobelong tooexactly jednoho uživatele, a proto uživatelů a zařízení znamenají v podstatě totéž.

## <a name="sessions-and-activities"></a>Relace a aktivity
Relace je jedno použití aplikace hello prováděná uživatelem, od uživatele hello čas hello používáním začne, až skončí hello.

Aktivita je jedno použití dané dílčí části aplikace hello provést jeden uživatel (obvykle je obrazovky, ale může být cokoli vhodný toohello aplikace).

Uživatel může najednou provést pouze jednu aktivitu.

Aktivita je identifikována názvem (omezený too64 znaků) a může volitelně vložit i doplňující data (limit 1024 bajtů hello).

Relace se automaticky vypočítávají hello posloupnost aktivit prováděné uživateli. Relace začíná, když hello uživatel zahájí první aktivitu a zastaví, jakmile skončí poslední aktivita. To znamená, že relaci není nutné toobe zahajovat a ukončovat. Místo toho se explicitně zahajují a ukončují aktivity. Pokud není hlášena žádná aktivita, není hlášena ani relace.

## <a name="events"></a>Události
Události jsou použité tooreport okamžitých akcí (například stisknutí tlačítka nebo přečtení článku uživatelem).

Událost může být související toohello aktuální relaci, tooa spuštěním úlohy, nebo se může jednat o samostatnou událost.

Událost je identifikována názvem (omezený too64 znaků) a může volitelně vložit i doplňující data (limit 1024 bajtů hello).

## <a name="error"></a>Chyba
Chyby jsou použité tooreport problémy správně rozpozná hello aplikace (například chybně použité akce uživatele, nebo chyby volání rozhraní API).

Chyba může být související toohello aktuální relaci, tooa spuštěním úlohy, nebo se může jednat o samostatnou chybu.

Chyba je identifikována názvem (omezený too64 znaků) a může volitelně vložit i doplňující data (limit 1024 bajtů hello).

## <a name="job"></a>Úloha
Úlohy jsou použité tooreport akce, které mají určitou dobu trvání (například doba volání rozhraní API, zobrazí čas reklamy, doba trvání úloh na pozadí nebo délka akcí uživatele).

Úlohu není související tooa relace, protože úloha lze provést v pozadí hello, bez nutnosti zásahu uživatele.

Úloha je identifikována názvem (omezený too64 znaků) a může volitelně vložit i doplňující data (limit 1024 bajtů hello).

## <a name="crash"></a>Zhroucení
Zhroucení vydává automaticky podle hello tooreport aplikace Mobile Engagement SDK selhání, kde nerozpozná aplikace hello bylo havárií.

## <a name="application-information"></a>Informace o aplikaci
Informace o aplikaci (nebo informace o aplikaci) je použité tootag uživatele, tedy tooassociate někteří uživatelé toohello dat aplikace (je to podobné tooweb soubory cookie, s tím rozdílem, že informace o aplikaci je uložený na straně serveru hello na platformě Azure Mobile Engagement hello).

Informace o aplikaci lze registrovat pomocí hello Mobile Engagement SDK API nebo pomocí API zařízení platformy Mobile Engagement hello.

Informace o aplikaci tvoří zařízení přidružených tooa dvojice klíč/hodnota. Hello klíč je název hello hello informace o aplikaci (omezený too64 znaků ASCII – písmena [a-zA-Z], číslice [0-9] a podtržítka [_]). Hello hodnota (omezena too1024 znaků) může být řetězec, celé číslo, datum (rrrr MM-dd) nebo logická hodnota (true nebo false).

Libovolný počet informací o aplikaci může být přidružené tooa zařízení v rámci hello omezení stanoveného cenovými podmínkami Mobile Engagementu hello. Pro každý klíč Mobile Engagement pouze uchovává informace o hello nejnovější nastavenou na hodnotu (žádná historie). Nastavení nebo změna hello informace o aplikaci přinutí Mobile Engagement toore-vyhodnocení kritérií pro cílové skupiny nastavená na tuto aplikaci informace (pokud existuje), což znamená, že informace o aplikaci lze použít tootrigger oznámení v reálném čase.

## <a name="extra-data"></a>Doplňující data
Doplňující data (nebo funkce) je libovolná data, která může být připojené tooevents, chyb, aktivity a úlohy.

Doplňující data jsou strukturovaná podobně jako objekty tooJSON: tvoří je strom z párů klíč/hodnota. Klíče jsou omezené too64 znaků ASCII – písmena [a-zA-Z], číslice [0-9] a podtržítka [_]) a hello celková velikost doplňkových dat je omezená too1024 znaků (po zakódování ve formátu JSON hello Mobile Engagement SDK).

Hello celý strom párů klíč/hodnota je uložena jako objekt JSON. Nicméně pouze první úroveň hello párů klíč/hodnota je rozložená toobe přímo přístupné toosome pokročilé funkce, jako jsou třeba segmenty (například můžete snadno definovat segment názvem "Scifisti", který je tvořen všichni uživatelé odeslali alespoň 10krát hello událostí s názvem "content_viewed" s hello doplňkový klíč "content_type" sady toohello hodnotu "scifi" v hello poslední měsíc). Proto důrazně doporučujeme toosend pouze doplňková data tvořená z jednoduchých seznamů párů klíč/hodnota, které používají skalárních hodnoty (například řetězce, data, celá čísla nebo logická hodnota).

## <a name="next-steps"></a>Další kroky
* [Přehled sady Windows Universal SDK pro Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
* [Přehled sady Windows Phone Silverlight SDK pro Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
* [iOS SDK pro Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
* [Android SDK pro Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

