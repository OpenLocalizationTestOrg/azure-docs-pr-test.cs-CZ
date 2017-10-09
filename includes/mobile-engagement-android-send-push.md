
### <a name="update-manifest-file-tooenable-notifications"></a><span data-ttu-id="6f28a-101">Aktualizace souboru manifestu tooenable oznámení</span><span class="sxs-lookup"><span data-stu-id="6f28a-101">Update manifest file tooenable notifications</span></span>
<span data-ttu-id="6f28a-102">Zkopírujte hello prostředky zasílání zpráv v aplikaci níže do souboru Manifest.xml mezi hello `<application>` a `</application>` značky.</span><span class="sxs-lookup"><span data-stu-id="6f28a-102">Copy hello in-app messaging resources below into your Manifest.xml between hello `<application>` and `</application>` tags.</span></span>

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

### <a name="specify-an-icon-for-notifications"></a><span data-ttu-id="6f28a-103">Určení ikony pro oznámení</span><span class="sxs-lookup"><span data-stu-id="6f28a-103">Specify an icon for notifications</span></span>
<span data-ttu-id="6f28a-104">Hello vložte následující fragment kódu XML do souboru Manifest.xml mezi hello `<application>` a `</application>` značky.</span><span class="sxs-lookup"><span data-stu-id="6f28a-104">Paste hello following XML snippet in your Manifest.xml between hello `<application>` and `</application>` tags.</span></span>

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

<span data-ttu-id="6f28a-105">Definuje hello ikonu, která se zobrazí v systému a oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f28a-105">This defines hello icon that is displayed both in system and in-app notifications.</span></span> <span data-ttu-id="6f28a-106">Pro oznámení v aplikaci je ikona volitelná, ale pro systémová oznámení povinná.</span><span class="sxs-lookup"><span data-stu-id="6f28a-106">It is optional for in-app notifications however mandatory for system notifications.</span></span> <span data-ttu-id="6f28a-107">Android odmítá systémová oznámení s neplatnými ikonami.</span><span class="sxs-lookup"><span data-stu-id="6f28a-107">Android will rejects system notifications with invalid icons.</span></span>

<span data-ttu-id="6f28a-108">Ujistěte se, že použijete ikonu, která existuje v jednom z hello **drawable** složky (například ``engagement_close.png``).</span><span class="sxs-lookup"><span data-stu-id="6f28a-108">Make sure you are using an icon that exists in one of hello **drawable** folders (like ``engagement_close.png``).</span></span> <span data-ttu-id="6f28a-109">Není dostupná podpora složky **mipmap**.</span><span class="sxs-lookup"><span data-stu-id="6f28a-109">**mipmap** folder isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="6f28a-110">Neměli byste používat hello **Spouštěč** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6f28a-110">You should not use hello **launcher** icon.</span></span> <span data-ttu-id="6f28a-111">Má jiné rozlišení a je obvykle ve složkách mipmap hello, které nepodporujeme.</span><span class="sxs-lookup"><span data-stu-id="6f28a-111">It has a different resolution and is usually in hello mipmap folders, which we don't support.</span></span>
> 
> 

<span data-ttu-id="6f28a-112">Ve skutečných aplikacích můžete použít ikonu vhodnou pro oznámení podle [pokynů pro návrh Androidu](http://developer.android.com/design/patterns/notifications.html).</span><span class="sxs-lookup"><span data-stu-id="6f28a-112">For real apps, you can use an icon that is suitable for notifications per [Android design guidelines](http://developer.android.com/design/patterns/notifications.html).</span></span>

> [!TIP]
> <span data-ttu-id="6f28a-113">zda toouse toobe správné rozlišení ikon, můžete se podívat na [tyto příklady](https://www.google.com/design/icons).</span><span class="sxs-lookup"><span data-stu-id="6f28a-113">toobe sure toouse correct icon resolutions, you can look at [these examples](https://www.google.com/design/icons).</span></span>
> <span data-ttu-id="6f28a-114">Projděte dolů toohello **oznámení** části, klikněte na ikonu a pak klikněte na tlačítko `PNGS` toodownload hello sadu drawable ikony.</span><span class="sxs-lookup"><span data-stu-id="6f28a-114">Scroll down toohello **Notification** section, click an icon, and then click `PNGS` toodownload hello icon drawable set.</span></span> <span data-ttu-id="6f28a-115">Můžete zobrazit složky drawable s které toouse řešení pro každou verzi ikony hello.</span><span class="sxs-lookup"><span data-stu-id="6f28a-115">You can see what drawable folders with which resolution toouse for each version of hello icon.</span></span>
> 
> 

### <a name="enable-your-app-tooreceive-gcm-push-notifications"></a><span data-ttu-id="6f28a-116">Povolení nabízených oznámení GCM tooreceive vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="6f28a-116">Enable your app tooreceive GCM push notifications</span></span>
1. <span data-ttu-id="6f28a-117">Vložte následující hello do souboru Manifest.xml mezi hello `<application>` a `</application>` značky po nahrazení hello **ID odesílatele** získané z konzoly Firebase projektu.</span><span class="sxs-lookup"><span data-stu-id="6f28a-117">Paste hello following into your Manifest.xml between hello `<application>` and `</application>` tags after replacing hello **Sender ID** obtained from your Firebase project console.</span></span> <span data-ttu-id="6f28a-118">Hello \n je úmyslné proto se ujistěte, že se vám stát čísla projektu hello.</span><span class="sxs-lookup"><span data-stu-id="6f28a-118">hello \n is intentional so make sure that you end hello project number with it.</span></span>
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. <span data-ttu-id="6f28a-119">Vložte kód hello pod do souboru Manifest.xml mezi hello `<application>` a `</application>` značky.</span><span class="sxs-lookup"><span data-stu-id="6f28a-119">Paste hello code below into your Manifest.xml between hello `<application>` and `</application>` tags.</span></span> <span data-ttu-id="6f28a-120">Nahraďte název balíčku hello <Your package name>.</span><span class="sxs-lookup"><span data-stu-id="6f28a-120">Replace hello package name <Your package name>.</span></span>
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>
3. <span data-ttu-id="6f28a-121">Přidejte poslední sadu oprávnění, která je zvýrazněná, před hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="6f28a-121">Add hello last set of permissions that are highlighted before hello `<application>` tag.</span></span> <span data-ttu-id="6f28a-122">Nahraďte `<Your package name>` podle hello skutečným názvem balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f28a-122">Replace `<Your package name>` by hello actual package name of your application.</span></span>
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

