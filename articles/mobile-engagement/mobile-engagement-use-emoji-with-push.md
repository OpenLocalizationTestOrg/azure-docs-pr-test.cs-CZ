---
title: "Použití emotikon Emoji v rámci Azure Mobile Engagement"
description: "Jak používat Emoji emotikony v rámci nabízených oznámení"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 663317d7-3c93-4e8f-b13d-c6fb342124ee
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: bbb7ce5e95b229a7505c5e97b6866d5a302a1d27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-emoji-emoticon-within-push-notifications"></a>Použití Emoji emotikonu v rámci nabízená oznámení
Můžete zahrnout Emoji emotikony ve službě nabízených oznámení v několika jednoduchými kroky: 

1. Nejprve budete muset najít Emoji chcete odeslat ve zprávě. Ujistěte se, že Emoji výběru bude podporovat cílové zařízení jako zařízení vyrábí trvat delší dobu, chcete-li přidat nově schválené Emojis na platformách zařízení. 
2. Na **Windows** – můžete přejít na to [odkaz](http://apps.timwhitlock.info/emoji/tables/unicode) a zkopírujte ikonu "Nativní".
   
    ![][7] 
3. Na **Mac** – můžete najít Emojis ve slovníku aplikaci v nabídce Upravit -> Emoji & symboly.
   
    ![][6] 
4. Nyní, přejděte na **dosáhnout** na kartě portálu Azure Mobile Engagement. Vyberte typ nabízených oznámení (oznámení, dotazuje atd). V tomto příkladu vybereme možnost nabízených oznámení.
5. Zadejte různých polí oznámení, dokud se nedostanete do textu oznámení. Toto je, kde budete přidávat emotikonu Emoji. Můžete uvést v názvu, zprávy nebo obojí. Bude potřeba přetáhněte nebo zkopírujte Emoji, najít z výše uvedené umístění. 
   
    ![][1]
6. Vyplňte příslušná pole pro oznámení a uložte jej. 
7. Při spouštění testu nebo aktivovat oznámení se zobrazí oznámení s na emotikonu jako zadaný.   
   
    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

