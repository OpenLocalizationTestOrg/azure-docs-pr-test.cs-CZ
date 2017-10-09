### <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a><span data-ttu-id="a5f4a-101">Mobile Engagement udělení přístupu tooyour klíč rozhraní API GCM</span><span class="sxs-lookup"><span data-stu-id="a5f4a-101">Grant Mobile Engagement access tooyour GCM API Key</span></span>
<span data-ttu-id="a5f4a-102">tooallow Mobile Engagement toosend nabízená oznámení vaším jménem, je nutné toogrant, že přístup tooyour klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-102">tooallow Mobile Engagement toosend push notifications on your behalf, you need toogrant it access tooyour API Key.</span></span> <span data-ttu-id="a5f4a-103">K tomu je potřeba nakonfigurujete a zadáte svůj klíč do hello portál Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-103">This is done by configuring and entering your key into hello Mobile Engagement portal.</span></span>

1. <span data-ttu-id="a5f4a-104">Z klasického portálu Azure, zkontrolujte, jestli pracujete v aplikaci hello používáte pro tento projekt a pak klikněte na tlačítko hello **Engage** tlačítko dole v hello:</span><span class="sxs-lookup"><span data-stu-id="a5f4a-104">From your Azure Classic Portal, ensure you're in hello app we're using for this project, and then click hello **Engage** button at hello bottom:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. <span data-ttu-id="a5f4a-105">Pak klikněte na tlačítko hello **nastavení** -> **nativní oznámení** části tooenter klíč GCM:</span><span class="sxs-lookup"><span data-stu-id="a5f4a-105">Then click hello **Settings** -> **Native Push** section tooenter your GCM Key:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. <span data-ttu-id="a5f4a-106">Klikněte na tlačítko hello **upravit** ikonu **klíč rozhraní API** v hello **nastavení GCM** části, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="a5f4a-106">Click hello **Edit** icon in front of **API Key** in hello **GCM Settings** section as shown below:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. <span data-ttu-id="a5f4a-107">V místní nabídce hello vložte klíč serveru GCM jste předtím získali hello a pak klikněte na **Ok**.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-107">In hello pop-up, paste hello GCM Server Key you obtained before and then click **Ok**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <span data-ttu-id="a5f4a-108"><a id="send"></a>Poslat tooyour aplikaci oznámení</span><span class="sxs-lookup"><span data-stu-id="a5f4a-108"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="a5f4a-109">Nyní vytvoříme kampaně jednoduché nabízených oznámení, který odesílá nabízená oznámení tooour aplikace pro práci s.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-109">We will now create a simple push notification campaign that sends a push notification tooour app.</span></span>

1. <span data-ttu-id="a5f4a-110">Přejděte toohello **dosáhnout** na portálu Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-110">Navigate toohello **REACH** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="a5f4a-111">Klikněte na tlačítko **nové oznámení** toocreate kampaň nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-111">Click **New announcement** toocreate your push notification campaign.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. <span data-ttu-id="a5f4a-112">Nastavte první pole kampaně pomocí následujících kroků hello hello:</span><span class="sxs-lookup"><span data-stu-id="a5f4a-112">Set up hello first field of your campaign through hello following steps:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    <span data-ttu-id="a5f4a-113">a.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-113">a.</span></span> <span data-ttu-id="a5f4a-114">Zadejte název kampaně.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-114">Name your campaign.</span></span>
   
    <span data-ttu-id="a5f4a-115">b.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-115">b.</span></span> <span data-ttu-id="a5f4a-116">Vyberte hello **typ doručení** jako *systémové oznámení -> jednoduché*: Toto je typ hello jednoduché Android nabízeného oznámení, který obsahuje název a řádek menšího textu.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-116">Select hello **Delivery type** as *System notification -> Simple*: This is hello simple Android push notification type that features a title and a small line of text.</span></span>
   
    <span data-ttu-id="a5f4a-117">c.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-117">c.</span></span> <span data-ttu-id="a5f4a-118">Vyberte **čas doručení** jako *kdykoli* tooallow hello aplikace tooreceive oznámení, zda je spuštěná aplikace hello, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-118">Select **Delivery time** as *Any time* tooallow hello app tooreceive a notification whether hello app is started or not.</span></span>
   
    <span data-ttu-id="a5f4a-119">d.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-119">d.</span></span> <span data-ttu-id="a5f4a-120">V hello oznámení text typ hello **název** který bude v tučné v nabízené hello.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-120">In hello notification text type hello **Title** which will be in bold in hello push.</span></span>
   
    <span data-ttu-id="a5f4a-121">e.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-121">e.</span></span> <span data-ttu-id="a5f4a-122">Do pole **Zpráva** zadejte zprávu.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-122">Then type your **Message**</span></span>
4. <span data-ttu-id="a5f4a-123">Přejděte dolů a v hello **obsahu** vyberte **pouze oznámení**.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-123">Scroll down, and in hello **Content** section, select **Notification only**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. <span data-ttu-id="a5f4a-124">Dokončení nastavení hello nejzákladnější kampaně možné.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-124">You're done setting hello most basic campaign possible.</span></span> <span data-ttu-id="a5f4a-125">Nyní znovu přejděte dolů a klikněte na tlačítko hello **vytvořit** tlačítko toosave kampaně.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-125">Now scroll down again and click hello **Create** button toosave your campaign.</span></span>
6. <span data-ttu-id="a5f4a-126">Poslední krok: Kliknutím na **aktivovat** tooactivate toosend kampaň nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a5f4a-126">Last step: click **Activate** tooactivate your campaign toosend push notifications.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

