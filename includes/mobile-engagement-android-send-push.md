
### <a name="update-manifest-file-to-enable-notifications"></a><span data-ttu-id="7500a-101">Aktualizace souboru manifest pro povolení oznámení</span><span class="sxs-lookup"><span data-stu-id="7500a-101">Update manifest file to enable notifications</span></span>
<span data-ttu-id="7500a-102">Zkopírujte níže uvedené prostředky zasílání zpráv v aplikaci do souboru Manifest.xml mezi značky `<application>` a `</application>`.</span><span class="sxs-lookup"><span data-stu-id="7500a-102">Copy the in-app messaging resources below into your Manifest.xml between the `<application>` and `</application>` tags.</span></span>

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

### <a name="specify-an-icon-for-notifications"></a><span data-ttu-id="7500a-103">Určení ikony pro oznámení</span><span class="sxs-lookup"><span data-stu-id="7500a-103">Specify an icon for notifications</span></span>
<span data-ttu-id="7500a-104">Vložte následující fragment kódu XML do souboru Manifest.xml mezi značky `<application>` a `</application>`.</span><span class="sxs-lookup"><span data-stu-id="7500a-104">Paste the following XML snippet in your Manifest.xml between the `<application>` and `</application>` tags.</span></span>

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

<span data-ttu-id="7500a-105">Tím se určuje ikona, která se bude zobrazovat v oznámeních v systému i aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7500a-105">This defines the icon that is displayed both in system and in-app notifications.</span></span> <span data-ttu-id="7500a-106">Pro oznámení v aplikaci je ikona volitelná, ale pro systémová oznámení povinná.</span><span class="sxs-lookup"><span data-stu-id="7500a-106">It is optional for in-app notifications however mandatory for system notifications.</span></span> <span data-ttu-id="7500a-107">Android odmítá systémová oznámení s neplatnými ikonami.</span><span class="sxs-lookup"><span data-stu-id="7500a-107">Android will rejects system notifications with invalid icons.</span></span>

<span data-ttu-id="7500a-108">Ujistěte se, že použijete ikonu, která existuje v některé ze složek **drawable** (třeba ``engagement_close.png``).</span><span class="sxs-lookup"><span data-stu-id="7500a-108">Make sure you are using an icon that exists in one of the **drawable** folders (like ``engagement_close.png``).</span></span> <span data-ttu-id="7500a-109">Není dostupná podpora složky **mipmap**.</span><span class="sxs-lookup"><span data-stu-id="7500a-109">**mipmap** folder isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="7500a-110">Neměli byste používat ikonu **launcher**.</span><span class="sxs-lookup"><span data-stu-id="7500a-110">You should not use the **launcher** icon.</span></span> <span data-ttu-id="7500a-111">Má jiné rozlišení a většinou se nachází ve složkách mipmap, které nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="7500a-111">It has a different resolution and is usually in the mipmap folders, which we don't support.</span></span>
> 
> 

<span data-ttu-id="7500a-112">Ve skutečných aplikacích můžete použít ikonu vhodnou pro oznámení podle [pokynů pro návrh Androidu](http://developer.android.com/design/patterns/notifications.html).</span><span class="sxs-lookup"><span data-stu-id="7500a-112">For real apps, you can use an icon that is suitable for notifications per [Android design guidelines](http://developer.android.com/design/patterns/notifications.html).</span></span>

> [!TIP]
> <span data-ttu-id="7500a-113">Pokud chcete mít jistotu, že používáte správné rozlišení ikon, můžete se podívat na [tyto příklady](https://www.google.com/design/icons).</span><span class="sxs-lookup"><span data-stu-id="7500a-113">To be sure to use correct icon resolutions, you can look at [these examples](https://www.google.com/design/icons).</span></span>
> <span data-ttu-id="7500a-114">Přejděte dolů do části **Oznámení** a kliknutím na `PNGS` stáhněte sadu drawable ikony.</span><span class="sxs-lookup"><span data-stu-id="7500a-114">Scroll down to the **Notification** section, click an icon, and then click `PNGS` to download the icon drawable set.</span></span> <span data-ttu-id="7500a-115">Zjistíte, které složky drawable s jakým rozlišením se mají pro každou verzi ikony použít.</span><span class="sxs-lookup"><span data-stu-id="7500a-115">You can see what drawable folders with which resolution to use for each version of the icon.</span></span>
> 
> 

### <a name="enable-your-app-to-receive-gcm-push-notifications"></a><span data-ttu-id="7500a-116">Povolení přijímání nabízených oznámení GCM v aplikaci</span><span class="sxs-lookup"><span data-stu-id="7500a-116">Enable your app to receive GCM push notifications</span></span>
1. <span data-ttu-id="7500a-117">Až nahradíte **získané** z konzoly projektu Firebase, vložte následující položky do souboru Manifest.xml mezi značky `<application>` a `</application>`.</span><span class="sxs-lookup"><span data-stu-id="7500a-117">Paste the following into your Manifest.xml between the `<application>` and `</application>` tags after replacing the **Sender ID** obtained from your Firebase project console.</span></span> <span data-ttu-id="7500a-118">Položka \n je úmyslná, proto se ujistěte, že se nachází na konci čísla projektu.</span><span class="sxs-lookup"><span data-stu-id="7500a-118">The \n is intentional so make sure that you end the project number with it.</span></span>
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. <span data-ttu-id="7500a-119">Vložte následující kód do souboru Manifest.xml mezi značky `<application>` a `</application>`.</span><span class="sxs-lookup"><span data-stu-id="7500a-119">Paste the code below into your Manifest.xml between the `<application>` and `</application>` tags.</span></span> <span data-ttu-id="7500a-120">Nahraďte název balíčku <Your package name>.</span><span class="sxs-lookup"><span data-stu-id="7500a-120">Replace the package name <Your package name>.</span></span>
   
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
3. <span data-ttu-id="7500a-121">Přidejte poslední sadu oprávnění, která je zvýrazněná, před značku `<application>`.</span><span class="sxs-lookup"><span data-stu-id="7500a-121">Add the last set of permissions that are highlighted before the `<application>` tag.</span></span> <span data-ttu-id="7500a-122">Nahraďte `<Your package name>` skutečným názvem balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="7500a-122">Replace `<Your package name>` by the actual package name of your application.</span></span>
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

