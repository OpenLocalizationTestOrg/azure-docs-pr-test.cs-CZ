### <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a><span data-ttu-id="10a7c-101">Udělení přístupu aplikaci Mobile Engagement k vašemu klíči rozhraní API GCM</span><span class="sxs-lookup"><span data-stu-id="10a7c-101">Grant Mobile Engagement access to your GCM API Key</span></span>
<span data-ttu-id="10a7c-102">Pokud chcete aplikaci Mobile Engagement povolit, aby vaším jménem odesílala nabízená oznámení, musíte jí udělit přístup k vašemu klíči rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="10a7c-102">To allow Mobile Engagement to send push notifications on your behalf, you need to grant it access to your API Key.</span></span> <span data-ttu-id="10a7c-103">To provedete tak, že svůj klíč nakonfigurujete a zadáte na portál Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="10a7c-103">This is done by configuring and entering your key into the Mobile Engagement portal.</span></span>

1. <span data-ttu-id="10a7c-104">Na portálu Azure Classic zkontrolujte, jestli pracujete v aplikaci určené pro tento projekt, a potom klikněte na tlačítko **Engage** (Zpřístupnit), které najdete dole:</span><span class="sxs-lookup"><span data-stu-id="10a7c-104">From your Azure Classic Portal, ensure you're in the app we're using for this project, and then click the **Engage** button at the bottom:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. <span data-ttu-id="10a7c-105">Potom klikněte na **Settings**(Nastavení)  -> **Native Push** (Nativní oznámení) a zadejte klíč GCM:</span><span class="sxs-lookup"><span data-stu-id="10a7c-105">Then click the **Settings** -> **Native Push** section to enter your GCM Key:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. <span data-ttu-id="10a7c-106">Klikněte na ikonu **Upravit** před možností **Klíč rozhraní API** v části **GCM Settings** (Nastavení GCM), jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="10a7c-106">Click the **Edit** icon in front of **API Key** in the **GCM Settings** section as shown below:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. <span data-ttu-id="10a7c-107">V místní nabídce vložte klíč serveru GCM, který jste předtím získali, a klikněte na **Ok**.</span><span class="sxs-lookup"><span data-stu-id="10a7c-107">In the pop-up, paste the GCM Server Key you obtained before and then click **Ok**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <span data-ttu-id="10a7c-108"><a id="send"></a>Odeslání oznámení do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="10a7c-108"><a id="send"></a>Send a notification to your app</span></span>
<span data-ttu-id="10a7c-109">Teď vytvoříme jednoduchou kampaň nabízených oznámení. Ta bude odesílat oznámení do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="10a7c-109">We will now create a simple push notification campaign that sends a push notification to our app.</span></span>

1. <span data-ttu-id="10a7c-110">Na portálu Mobile Engagement přejděte na kartu **REACH**.</span><span class="sxs-lookup"><span data-stu-id="10a7c-110">Navigate to the **REACH** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="10a7c-111">Kliknutím na **Nové oznámení** vytvořte kampaň nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="10a7c-111">Click **New announcement** to create your push notification campaign.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. <span data-ttu-id="10a7c-112">Nastavte první pole kampaně pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="10a7c-112">Set up the first field of your campaign through the following steps:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    <span data-ttu-id="10a7c-113">a.</span><span class="sxs-lookup"><span data-stu-id="10a7c-113">a.</span></span> <span data-ttu-id="10a7c-114">Zadejte název kampaně.</span><span class="sxs-lookup"><span data-stu-id="10a7c-114">Name your campaign.</span></span>
   
    <span data-ttu-id="10a7c-115">b.</span><span class="sxs-lookup"><span data-stu-id="10a7c-115">b.</span></span> <span data-ttu-id="10a7c-116">U položky **Typ doručení** vyberte *Systémové oznámení -> Jednoduché*: Toto typ jednoduchého nabízeného oznámení pro Android, které obsahuje název a řádek menšího textu.</span><span class="sxs-lookup"><span data-stu-id="10a7c-116">Select the **Delivery type** as *System notification -> Simple*: This is the simple Android push notification type that features a title and a small line of text.</span></span>
   
    <span data-ttu-id="10a7c-117">c.</span><span class="sxs-lookup"><span data-stu-id="10a7c-117">c.</span></span> <span data-ttu-id="10a7c-118">U položky **Delivery time** (Čas doručení) vyberte *Any time* (Kdykoli), aby mohla aplikace přijmout oznámení bez ohledu na to, jestli je spuštěná, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="10a7c-118">Select **Delivery time** as *Any time* to allow the app to receive a notification whether the app is started or not.</span></span>
   
    <span data-ttu-id="10a7c-119">d.</span><span class="sxs-lookup"><span data-stu-id="10a7c-119">d.</span></span> <span data-ttu-id="10a7c-120">Do textu oznámení zadejte **Název**, který se v nabízeném oznámení zobrazí tučně.</span><span class="sxs-lookup"><span data-stu-id="10a7c-120">In the notification text type the **Title** which will be in bold in the push.</span></span>
   
    <span data-ttu-id="10a7c-121">e.</span><span class="sxs-lookup"><span data-stu-id="10a7c-121">e.</span></span> <span data-ttu-id="10a7c-122">Do pole **Zpráva** zadejte zprávu.</span><span class="sxs-lookup"><span data-stu-id="10a7c-122">Then type your **Message**</span></span>
4. <span data-ttu-id="10a7c-123">Přejděte dolů a v části **Obsah** vyberte **Pouze oznámení**.</span><span class="sxs-lookup"><span data-stu-id="10a7c-123">Scroll down, and in the **Content** section, select **Notification only**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. <span data-ttu-id="10a7c-124">Tím jste dokončili nastavení nejzákladnější možné kampaně.</span><span class="sxs-lookup"><span data-stu-id="10a7c-124">You're done setting the most basic campaign possible.</span></span> <span data-ttu-id="10a7c-125">Nyní znovu přejděte dolů a kliknutím na tlačítko **Vytvořit** kampaň uložte.</span><span class="sxs-lookup"><span data-stu-id="10a7c-125">Now scroll down again and click the **Create** button to save your campaign.</span></span>
6. <span data-ttu-id="10a7c-126">Poslední krok: Kliknutím na **Aktivovat** aktivujte svoji kampaň, která bude zasílat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="10a7c-126">Last step: click **Activate** to activate your campaign to send push notifications.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

