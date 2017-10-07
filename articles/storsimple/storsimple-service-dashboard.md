---
title: "řídicí panel služby Správce aaaStorSimple | Microsoft Docs"
description: "Popisuje řídicí panel služby StorSimple Manager hello a vysvětluje, jak toouse ho toomonitor hello stavu vašeho řešení StorSimple."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: dc1197eb5deac337215b260845631a4f04be1011
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-dashboard"></a>Pomocí řídicího panelu služby StorSimple Manager hello
## <a name="overview"></a>Přehled
stránka řídicího panelu služby StorSimple Manager Hello obsahuje souhrnné zobrazení všech hello zařízení, které jsou připojené toohello služby StorSimple Manager, zvýraznění ty, které vyžadují pozornost. správce systému. Tento kurz představuje stránku hello řídicího panelu, popisuje hello řídicí panel obsah a funkce a popisuje hello úlohy, které můžete provádět z této stránky.

![Řídicí panel služby](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

řídicí panel služby StorSimple Manager Hello zobrazí hello následující informace:

* V hello **grafu** oblasti, uvidíte relevantní metriky hello grafu pro vaše zařízení. Můžete zobrazit hello primárního úložiště (místně ukotvena a víceúrovňová) používané v rámci celé všechny hello zařízení, a také hello cloudového úložiště využívat zařízení v časovém intervalu. Pomocí ovládacích prvků hello v pravém horním rohu hello hello grafu toospecify 1 týden, měsíc 1, 3 měsíců nebo 1 rok časové rozmezí.
* Hello **přehled využití** ukazuje hello primárního úložiště, který je zřízený a spotřebovávají všech zařízení relativní toohello celkové úložiště k dispozici ve všech zařízeních. **Zřízení** odkazuje toohello velikost úložiště, která je připravená a přidělení k použití, při **používá** odkazuje toousage svazků, jak je vidět hello iniciátory, které jsou připojené toohello zařízení.
* Hello **výstrahy** oblasti poskytuje snímek všechny aktivní výstrahy hello ve všech zařízeních hello seskupené podle závažnosti výstrah. Kliknutím na úroveň závažnosti hello otevře hello **výstrahy** stránky, vymezená tooshow tyto výstrahy. Na hello **výstrahy** stránky, můžete kliknout na další podrobnosti o této výstraze, včetně všechny doporučené akce tooview jednotlivé výstrahy. Pokud byl vyřešen problém hello, zrušte hello výstrahy.
* Hello **úlohy** oblasti poskytuje snímek posledních úloh na všech zařízeních, které jsou připojené tooyour služby. Existují odkazy, které můžete použít toolook na úlohy, které jsou aktuálně probíhá, ty, které se nezdařilo hello posledních 24 hodin, nebo ty, které jsou naplánované toorun v hello dalších 24 hodin.
* Hello **rychlého přehledu** oblasti poskytuje užitečné informace, jako je stav služby, počet zařízení připojených toohello service, umístění služby hello a podrobnosti o hello předplatné, které souvisí se službou hello. Je také odkaz toohello operací protokolu. Klikněte na odkaz toosee hello seznam všech dokončených operací služby StorSimple Manager.

Můžete použít hello StorSimple Manager service řídicí panel stránky tooinitiate hello následující úlohy:

* Zobrazení nebo znovu vygenerovat registrační klíč služby hello.
* Změna šifrovacího klíče dat služby hello.
* Zobrazit protokoly operací hello.

## <a name="view-or-regenerate-hello-service-registration-key"></a>Zobrazení nebo znovu vygenerovat registrační klíč služby hello
registrační klíč služby Hello je použité tooregister Microsoft Azure StorSimple zařízení pomocí služby StorSimple Manager hello, takže hello zařízení se zobrazí v hello klasický portál Azure pro další akce správy. Hello klíč je vytvořen na první zařízení hello a sdílet s hello zbytek zařízení.

Kliknutím na tlačítko **registrační klíč** (na hello dolní části stránky hello) otevře hello **registrační klíč služby** dialogové okno, kde můžete provádět buď kopírování hello aktuální služby registrace klíče toohello schránky nebo Znovu vygenerujte registrační klíč služby hello.

Opakované generování klíče hello neovlivní dříve zaregistrovaný zařízení: ovlivňuje pouze hello zařízení, které jsou registrovány hello služby po znovu vygeneruje klíč hello.

Další informace o prohlížení a generování klíčů, přejděte registrace hello služby příliš[registrační klíč služby hello Get](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-hello-service-data-encryption-key"></a>Změna šifrovacího klíče dat služby hello
Šifrovací klíče dat služby jsou použité tooencrypt důvěrné zákaznická data, jako je například pověření účtu, úložiště, které jsou odesílány z zařízení StorSimple toohello služby StorSimple Manager. Budete potřebovat toochange tyto klíče pravidelně Pokud má vaše organizace IT zásad střídání klíče na zařízení úložiště hello. Hello změny klíče proces může být mírně lišit podle toho, jestli je jeden nebo více zařízení spravuje hello služby StorSimple Manager.

Změna šifrovacího klíče dat hello služby je proces, krok 3:

1. Pomocí hello portál Azure classic, autorizaci zařízení toochange hello šifrovacího klíče dat služby.
2. Pomocí Windows Powershellu pro StorSimple, zahajte hello služby dat šifrování klíče změnu.
3. Pokud máte více než jedno zařízení StorSimple, aktualizujte hello služby datového šifrovacího klíče na hello jiná zařízení.

Hello následující kroky popisují proces hello výměna šifrovacího klíče dat služby hello.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-hello-operations-logs"></a>Zobrazit protokoly operací hello
Hello protokoly operací můžete zobrazit kliknutím na odkaz protokoly operaci hello k dispozici v hello **rychlého přehledu** podokně hello řídicího panelu. Tím přejdete toohello správu stránka služby, kde můžete filtrovat a zobrazit hello protokoly konkrétní tooyour služby StorSimple Manager.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[řešení zařízení StorSimple](storsimple-troubleshoot-operational-device.md).
* Další informace o příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

