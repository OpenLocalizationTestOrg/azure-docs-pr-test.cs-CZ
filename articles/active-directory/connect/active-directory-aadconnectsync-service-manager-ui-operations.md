---
title: "Operace Správce synchronizace služby Azure AD Connect | Microsoft Docs"
description: "Porozumět hello kartu operace v hello Synchronization Service Manager pro Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: decbc53613d456a71cd116c40c5e1fd761efd4af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-sync-service-manager-operations-tab"></a>Pomocí hello kartu operace synchronizace Service Manager

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Hello operations kartě se zobrazují hello výsledky z nejnovější operace hello. Na této kartě je klíče toounderstand a řešení problémů.

## <a name="understand-hello-information-visible-in-hello-operations-tab"></a>Pochopení hello informace zobrazené na kartě operations hello
horní polovině Hello ukazuje všechny běží v chronologickém pořadí. Ve výchozím nastavení, protokol operations hello udržuje informace o hello posledních sedmi dnů, ale toto nastavení lze změnit pomocí hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md). Chcete toolook pro všechny spuštění, které nejsou zobrazeny stav úspěšného dokončení. Můžete změnit hello řazení kliknutím na záhlaví hello.

Hello **stav** sloupec je nejdůležitější informace hello a ukazuje hello nejzávažnějšího problém pro spuštění. Zde je stručný hello nejběžnější stavů v pořadí podle priority tooinvestigate (kde * znamenat několik řetězců možná chyba).

| Status | Komentář |
| --- | --- |
| ukončeno-* |Hello spuštění se nepodařilo dokončit. Například pokud hello vzdálený systém je vypnutý a nelze kontaktovat. |
| Zastavit limit chyb |Existuje více než 5 000 chyby. Hello spustit automaticky zastavil z důvodu toohello velký počet chyb. |
| dokončené -\*– chyby |Hello spustit dokončené, ale nejsou chyby (méně než 5 000), které by se měly prozkoumat. |
| dokončené -\*– upozornění |Hello spustit dokončilo, ale některá data se nenachází ve stavu hello očekává. Pokud máte chyby, pak tato zpráva je obvykle jenom příznakem. Dokud nebude mít řešit chyby, by neměl prozkoumat upozornění. |
| úspěch |Žádné problémy. |

Když vyberete řádek, aktualizuje hello dolní tooshow hello podrobnosti této spustit. toohello daleko pravé dolní hello, můžete mít tom, že seznam **krok #**. Tento seznam se zobrazí, jenom Pokud máte více domén v doménové struktuře každou doménu, kde je reprezentována krok. název domény Hello naleznete pod nadpisem hello **oddílu**. V části **Statistika synchronizace**, můžete najít další informace o hello počet změn, které byly zpracovány. Můžete kliknout na hello odkazy tooget seznam objektů hello změnit. Pokud máte objektů s chybami, tyto chyby, zobrazí na **chyby synchronizace**.

Další informace najdete v tématu [řešení potíží s objekt, který není synchronizace](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)

## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
