---
title: "aaaUse Emoji emotikony v rámci Azure Mobile Engagement"
description: "Jak toouse Emoji emotikony v rámci nabízených oznámení"
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
ms.openlocfilehash: 63bfc59ef472e9fe9aa28b5ac8761017b9250e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-emoji-emoticon-within-push-notifications"></a><span data-ttu-id="e8218-103">Použití Emoji emotikonu v rámci nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="e8218-103">Use Emoji emoticon within Push Notifications</span></span>
<span data-ttu-id="e8218-104">Můžete zahrnout Emoji emotikony ve službě nabízených oznámení v několika jednoduchými kroky:</span><span class="sxs-lookup"><span data-stu-id="e8218-104">You can include Emoji emoticons in you push notification in a few easy steps:</span></span> 

1. <span data-ttu-id="e8218-105">Je třeba nejprve všech toofind hello Emoji chcete toosend v uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="e8218-105">First of all you need toofind hello Emoji you want toosend in hello message.</span></span> <span data-ttu-id="e8218-106">Zkontrolujte, že hello Emoji výběru bude podporovat hello cílové zařízení jako zařízení vyrábí nově schválené platformy zařízení toohello Emojis provést některé tooadd čas.</span><span class="sxs-lookup"><span data-stu-id="e8218-106">Please ensure that hello Emoji you are selecting will be supported by hello target device as device manufactures take some time tooadd newly approved Emojis toohello device platforms.</span></span> 
2. <span data-ttu-id="e8218-107">Na **Windows** – můžete procházet toothis [odkaz](http://apps.timwhitlock.info/emoji/tables/unicode) a "Nativní" ikona kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e8218-107">On **Windows** - you can navigate toothis [link](http://apps.timwhitlock.info/emoji/tables/unicode) and copy hello 'Native' icon.</span></span>
   
    ![][7] 
3. <span data-ttu-id="e8218-108">Na **Mac** – můžete najít hello Emojis ve slovníku aplikaci v nabídce Upravit -> Emoji & symboly.</span><span class="sxs-lookup"><span data-stu-id="e8218-108">On **Mac** - you can find hello Emojis in Dictionary application under Edit -> Emoji & Symbols.</span></span>
   
    ![][6] 
4. <span data-ttu-id="e8218-109">Nyní přejděte toohello **dosáhnout** karta na portál Azure Mobile Engagement hello hello.</span><span class="sxs-lookup"><span data-stu-id="e8218-109">Now, go toohello **Reach** tab on hello hello Azure Mobile Engagement portal.</span></span> <span data-ttu-id="e8218-110">Vyberte typ hello nabízených oznámení (oznámení, hlasování atd).</span><span class="sxs-lookup"><span data-stu-id="e8218-110">Select hello type of your push notification (Announcement, polls etc).</span></span> <span data-ttu-id="e8218-111">V tomto příkladu vybereme možnost nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="e8218-111">For this example we choose an announcement push.</span></span>
5. <span data-ttu-id="e8218-112">Zadejte hello různých polí hello oznámení až hello textu hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="e8218-112">Specify hello different fields of hello notification until you reach hello text of hello notification.</span></span> <span data-ttu-id="e8218-113">Toto je, kde budete přidávat emotikonu Emoji.</span><span class="sxs-lookup"><span data-stu-id="e8218-113">This is where you will add your Emoji Emoticon.</span></span> <span data-ttu-id="e8218-114">Můžete zvolit tooput v hello title, uvítací zprávu nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="e8218-114">You can choose tooput it in hello title, hello message, or both.</span></span> <span data-ttu-id="e8218-115">Budete potřebovat toodrag a drop nebo zkopírujte hello Emoji, najít z výše uvedených hello umístění.</span><span class="sxs-lookup"><span data-stu-id="e8218-115">You will need toodrag and drop or copy hello Emoji that you find from hello locations above.</span></span> 
   
    ![][1]
6. <span data-ttu-id="e8218-116">Dokončení hello další pole pro hello oznámení a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="e8218-116">Complete hello other fields for hello notification and save it.</span></span> 
7. <span data-ttu-id="e8218-117">Při spouštění testu nebo aktivovat hello oznámení se zobrazí oznámení s hello emotikonu jako zadaný.</span><span class="sxs-lookup"><span data-stu-id="e8218-117">When you run a test or activate hello announcement you will see a notification with hello emoticon as specified.</span></span>   
   
    <span data-ttu-id="e8218-118">![][3] ![][4] ![][5]</span><span class="sxs-lookup"><span data-stu-id="e8218-118">![][3] ![][4] ![][5]</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

