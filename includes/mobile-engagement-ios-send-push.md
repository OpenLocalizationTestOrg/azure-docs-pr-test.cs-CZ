### <a name="grant-access-tooyour-push-certificate-toomobile-engagement"></a><span data-ttu-id="3c4c1-101">Udělení přístupu tooyour certifikátu Push tooMobile zapojení</span><span class="sxs-lookup"><span data-stu-id="3c4c1-101">Grant access tooyour Push Certificate tooMobile Engagement</span></span>
<span data-ttu-id="3c4c1-102">tooallow Mobile Engagement toosend nabízená oznámení vaším jménem, je nutné toogrant, že přístup tooyour certifikátu.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-102">tooallow Mobile Engagement toosend Push Notifications on your behalf, you need toogrant it access tooyour certificate.</span></span> <span data-ttu-id="3c4c1-103">K tomu je potřeba nakonfigurujete a zadáte vašeho certifikátu na portál Mobile Engagement hello.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-103">This is done by configuring and entering your certificate into hello Mobile Engagement portal.</span></span> <span data-ttu-id="3c4c1-104">Musíte nejdřív získat certifikát .p12 podle pokynů v [dokumentaci od společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).</span><span class="sxs-lookup"><span data-stu-id="3c4c1-104">Make sure you obtain your .p12 certificate as explained in [Apple's documentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

1. <span data-ttu-id="3c4c1-105">Přejděte tooyour portál Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-105">Navigate tooyour Mobile Engagement portal.</span></span> <span data-ttu-id="3c4c1-106">Zkontrolujte, jestli v hello správné a potom klikněte na hello **Engage** tlačítko dole v hello:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-106">Ensure you're in hello correct and then click on hello **Engage** button at hello bottom:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. <span data-ttu-id="3c4c1-107">Klikněte na hello **nastavení** stránky na portálu Engagement.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-107">Click on hello **Settings** page in your Engagement Portal.</span></span> <span data-ttu-id="3c4c1-108">Zde klikněte na hello **nativní oznámení** části tooupload svůj certifikát .p12:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-108">From there click on hello **Native Push** section tooupload your p12 certificate:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. <span data-ttu-id="3c4c1-109">Vyberte svůj soubor .p12, nahrajte ho a zadejte heslo:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-109">Select your p12, upload it and type your password:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <span data-ttu-id="3c4c1-110"><a id="send"></a>Poslat tooyour aplikaci oznámení</span><span class="sxs-lookup"><span data-stu-id="3c4c1-110"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="3c4c1-111">Teď vytvoříme jednoduchou kampaň nabízených oznámení, který bude odesílat aplikace tooour na nabízené:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-111">We will now create a simple Push Notification campaign that will send a push tooour app:</span></span>

1. <span data-ttu-id="3c4c1-112">Přejděte toohello **dosáhnout** na portálu Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-112">Navigate toohello **Reach** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="3c4c1-113">Klikněte na tlačítko **nové oznámení** toocreate kampaň nabízených</span><span class="sxs-lookup"><span data-stu-id="3c4c1-113">Click **New Announcement** toocreate your push campaign</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. <span data-ttu-id="3c4c1-114">Instalační program hello první pole své kampaně:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-114">Setup hello first fields of your campaign:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * <span data-ttu-id="3c4c1-115">Zadejte **Název** kampaně.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-115">Provide a **Name** for your campaign</span></span> 
   * <span data-ttu-id="3c4c1-116">Vyberte hello **čas doručení** jako **pouze mimo aplikaci**: Toto je hello jednoduchý typ nabízeného oznámení Apple, která obsahuje text.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-116">Select hello **Delivery time** as **Out of app only**: this is hello simple Apple push notification type that features some text.</span></span>
   * <span data-ttu-id="3c4c1-117">V textu hello oznámení, zadejte první hello **název** která bude hello první řádek v nabízené hello.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-117">In hello notification text, type first hello **Title** which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="3c4c1-118">Zadejte vaše **zpráva** která bude druhý řádek hello</span><span class="sxs-lookup"><span data-stu-id="3c4c1-118">Then type your **Message** which will be hello second line</span></span>
4. <span data-ttu-id="3c4c1-119">Posuňte se dolů a v hello části obsahu vyberte **pouze oznámení**</span><span class="sxs-lookup"><span data-stu-id="3c4c1-119">Scroll down, and in hello content section select **Notification only**</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. <span data-ttu-id="3c4c1-120">Dokončení nastavení hello nejzákladnější kampaně.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-120">You're done setting hello most basic campaign.</span></span> <span data-ttu-id="3c4c1-121">Nyní přejděte dolů a klikněte na **vytvořit** tlačítko toosave kampaň nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-121">Now scroll down and click on **Create** button toosave your push notification campaign.</span></span> 
6. <span data-ttu-id="3c4c1-122">Nakonec kliknutím na **aktivovat** toosend nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c4c1-122">Finally - click on **Activate** toosend push notification.</span></span> 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. <span data-ttu-id="3c4c1-123">Bude možné přijímat oznámení hello na zařízení s iOS v centru oznámení hello jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-123">You will be able receive hello notification on your iOS device in hello notification center like hello following:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. <span data-ttu-id="3c4c1-124">Pokud máte Apple Watch spárovat s tímto zařízením iOS se zobrazí hello oznámení na vaše Apple Watch:</span><span class="sxs-lookup"><span data-stu-id="3c4c1-124">If you have an Apple Watch paired with this iOS device then you will see hello notification on your Apple Watch:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

