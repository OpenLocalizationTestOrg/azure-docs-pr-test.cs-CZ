### <a name="grant-access-to-your-push-certificate-to-mobile-engagement"></a><span data-ttu-id="9103e-101">Udělení přístupu službě Mobile Engagement k vašemu certifikátu push</span><span class="sxs-lookup"><span data-stu-id="9103e-101">Grant access to your Push Certificate to Mobile Engagement</span></span>
<span data-ttu-id="9103e-102">Pokud chcete aplikaci Mobile Engagement povolit, aby vaším jménem odesílala nabízená oznámení, musíte jí udělit přístup k vašemu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9103e-102">To allow Mobile Engagement to send Push Notifications on your behalf, you need to grant it access to your certificate.</span></span> <span data-ttu-id="9103e-103">To provedete tak, že svůj certifikát nakonfigurujete a zadáte na portál Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="9103e-103">This is done by configuring and entering your certificate into the Mobile Engagement portal.</span></span> <span data-ttu-id="9103e-104">Musíte nejdřív získat certifikát .p12 podle pokynů v [dokumentaci od společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).</span><span class="sxs-lookup"><span data-stu-id="9103e-104">Make sure you obtain your .p12 certificate as explained in [Apple's documentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

1. <span data-ttu-id="9103e-105">Přejděte na portál Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="9103e-105">Navigate to your Mobile Engagement portal.</span></span> <span data-ttu-id="9103e-106">Zkontrolujte, jestli pracujete ve správné aplikaci, a potom klikněte na tlačítko **Engage** (Zpřístupnit), které najdete dole:</span><span class="sxs-lookup"><span data-stu-id="9103e-106">Ensure you're in the correct and then click on the **Engage** button at the bottom:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. <span data-ttu-id="9103e-107">Klikněte na stránku **Nastavení** na portálu Engagement.</span><span class="sxs-lookup"><span data-stu-id="9103e-107">Click on the **Settings** page in your Engagement Portal.</span></span> <span data-ttu-id="9103e-108">Tam klikněte do části **Nativní oznámení**, abyste mohli nahrát svůj certifikát .p12:</span><span class="sxs-lookup"><span data-stu-id="9103e-108">From there click on the **Native Push** section to upload your p12 certificate:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. <span data-ttu-id="9103e-109">Vyberte svůj soubor .p12, nahrajte ho a zadejte heslo:</span><span class="sxs-lookup"><span data-stu-id="9103e-109">Select your p12, upload it and type your password:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <span data-ttu-id="9103e-110"><a id="send"></a>Odeslání oznámení do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="9103e-110"><a id="send"></a>Send a notification to your app</span></span>
<span data-ttu-id="9103e-111">Teď vytvoříme jednoduchou kampaň nabízených oznámení. Ta bude odesílat oznámení do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="9103e-111">We will now create a simple Push Notification campaign that will send a push to our app:</span></span>

1. <span data-ttu-id="9103e-112">Na portálu Mobile Engagement přejděte na kartu **Reach**.</span><span class="sxs-lookup"><span data-stu-id="9103e-112">Navigate to the **Reach** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="9103e-113">Kliknutím na **Nové oznámení** vytvořte kampaň nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="9103e-113">Click **New Announcement** to create your push campaign</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. <span data-ttu-id="9103e-114">Nastavte první pole své kampaně:</span><span class="sxs-lookup"><span data-stu-id="9103e-114">Setup the first fields of your campaign:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * <span data-ttu-id="9103e-115">Zadejte **Název** kampaně.</span><span class="sxs-lookup"><span data-stu-id="9103e-115">Provide a **Name** for your campaign</span></span> 
   * <span data-ttu-id="9103e-116">U položky **Delivery time** (Čas doručení) vyberte **Out of app only** (Pouze mimo aplikaci): Jedná se o jednoduchý typ nabízeného oznámení Apple, který obsahuje text.</span><span class="sxs-lookup"><span data-stu-id="9103e-116">Select the **Delivery time** as **Out of app only**: this is the simple Apple push notification type that features some text.</span></span>
   * <span data-ttu-id="9103e-117">Do textu oznámení nejdřív zadejte **Název**, který se zobrazí na prvním řádku nabízeného oznámení.</span><span class="sxs-lookup"><span data-stu-id="9103e-117">In the notification text, type first the **Title** which will be the first line in the push.</span></span>
   * <span data-ttu-id="9103e-118">Potom zadejte text do pole **Zpráva**, který se zobrazí na druhém řádku.</span><span class="sxs-lookup"><span data-stu-id="9103e-118">Then type your **Message** which will be the second line</span></span>
4. <span data-ttu-id="9103e-119">Přejděte dolů a v části obsahu vyberte **Pouze oznámení**</span><span class="sxs-lookup"><span data-stu-id="9103e-119">Scroll down, and in the content section select **Notification only**</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. <span data-ttu-id="9103e-120">Tím jste dokončili nastavení nejzákladnější kampaně.</span><span class="sxs-lookup"><span data-stu-id="9103e-120">You're done setting the most basic campaign.</span></span> <span data-ttu-id="9103e-121">Teď přejděte dolů a kliknutím na tlačítko **Vytvořit** kampaň nabízených oznámení uložte.</span><span class="sxs-lookup"><span data-stu-id="9103e-121">Now scroll down and click on **Create** button to save your push notification campaign.</span></span> 
6. <span data-ttu-id="9103e-122">Nakonec kliknutím na **Aktivovat** odešlete nabízené oznámení.</span><span class="sxs-lookup"><span data-stu-id="9103e-122">Finally - click on **Activate** to send push notification.</span></span> 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. <span data-ttu-id="9103e-123">Oznámení budete moct na svém zařízení s iOS přijmout v centru oznámení, jako třeba tohle:</span><span class="sxs-lookup"><span data-stu-id="9103e-123">You will be able receive the notification on your iOS device in the notification center like the following:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. <span data-ttu-id="9103e-124">Pokud máte s tímto zařízením s iOS spárované zařízení Apple Watch, uvidíte oznámení na zařízení Apple Watch:</span><span class="sxs-lookup"><span data-stu-id="9103e-124">If you have an Apple Watch paired with this iOS device then you will see the notification on your Apple Watch:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

