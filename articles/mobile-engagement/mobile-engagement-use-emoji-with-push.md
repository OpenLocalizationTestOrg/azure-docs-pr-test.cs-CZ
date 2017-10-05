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
# <a name="use-emoji-emoticon-within-push-notifications"></a><span data-ttu-id="599f9-103">Použití Emoji emotikonu v rámci nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="599f9-103">Use Emoji emoticon within Push Notifications</span></span>
<span data-ttu-id="599f9-104">Můžete zahrnout Emoji emotikony ve službě nabízených oznámení v několika jednoduchými kroky:</span><span class="sxs-lookup"><span data-stu-id="599f9-104">You can include Emoji emoticons in you push notification in a few easy steps:</span></span> 

1. <span data-ttu-id="599f9-105">Nejprve budete muset najít Emoji chcete odeslat ve zprávě.</span><span class="sxs-lookup"><span data-stu-id="599f9-105">First of all you need to find the Emoji you want to send in the message.</span></span> <span data-ttu-id="599f9-106">Ujistěte se, že Emoji výběru bude podporovat cílové zařízení jako zařízení vyrábí trvat delší dobu, chcete-li přidat nově schválené Emojis na platformách zařízení.</span><span class="sxs-lookup"><span data-stu-id="599f9-106">Please ensure that the Emoji you are selecting will be supported by the target device as device manufactures take some time to add newly approved Emojis to the device platforms.</span></span> 
2. <span data-ttu-id="599f9-107">Na **Windows** – můžete přejít na to [odkaz](http://apps.timwhitlock.info/emoji/tables/unicode) a zkopírujte ikonu "Nativní".</span><span class="sxs-lookup"><span data-stu-id="599f9-107">On **Windows** - you can navigate to this [link](http://apps.timwhitlock.info/emoji/tables/unicode) and copy the 'Native' icon.</span></span>
   
    ![][7] 
3. <span data-ttu-id="599f9-108">Na **Mac** – můžete najít Emojis ve slovníku aplikaci v nabídce Upravit -> Emoji & symboly.</span><span class="sxs-lookup"><span data-stu-id="599f9-108">On **Mac** - you can find the Emojis in Dictionary application under Edit -> Emoji & Symbols.</span></span>
   
    ![][6] 
4. <span data-ttu-id="599f9-109">Nyní, přejděte na **dosáhnout** na kartě portálu Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="599f9-109">Now, go to the **Reach** tab on the the Azure Mobile Engagement portal.</span></span> <span data-ttu-id="599f9-110">Vyberte typ nabízených oznámení (oznámení, dotazuje atd).</span><span class="sxs-lookup"><span data-stu-id="599f9-110">Select the type of your push notification (Announcement, polls etc).</span></span> <span data-ttu-id="599f9-111">V tomto příkladu vybereme možnost nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="599f9-111">For this example we choose an announcement push.</span></span>
5. <span data-ttu-id="599f9-112">Zadejte různých polí oznámení, dokud se nedostanete do textu oznámení.</span><span class="sxs-lookup"><span data-stu-id="599f9-112">Specify the different fields of the notification until you reach the text of the notification.</span></span> <span data-ttu-id="599f9-113">Toto je, kde budete přidávat emotikonu Emoji.</span><span class="sxs-lookup"><span data-stu-id="599f9-113">This is where you will add your Emoji Emoticon.</span></span> <span data-ttu-id="599f9-114">Můžete uvést v názvu, zprávy nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="599f9-114">You can choose to put it in the title, the message, or both.</span></span> <span data-ttu-id="599f9-115">Bude potřeba přetáhněte nebo zkopírujte Emoji, najít z výše uvedené umístění.</span><span class="sxs-lookup"><span data-stu-id="599f9-115">You will need to drag and drop or copy the Emoji that you find from the locations above.</span></span> 
   
    ![][1]
6. <span data-ttu-id="599f9-116">Vyplňte příslušná pole pro oznámení a uložte jej.</span><span class="sxs-lookup"><span data-stu-id="599f9-116">Complete the other fields for the notification and save it.</span></span> 
7. <span data-ttu-id="599f9-117">Při spouštění testu nebo aktivovat oznámení se zobrazí oznámení s na emotikonu jako zadaný.</span><span class="sxs-lookup"><span data-stu-id="599f9-117">When you run a test or activate the announcement you will see a notification with the emoticon as specified.</span></span>   
   
    <span data-ttu-id="599f9-118">![][3] ![][4] ![][5]</span><span class="sxs-lookup"><span data-stu-id="599f9-118">![][3] ![][4] ![][5]</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

