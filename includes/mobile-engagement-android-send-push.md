
### <a name="update-manifest-file-tooenable-notifications"></a>Aktualizace souboru manifestu tooenable oznámení
Zkopírujte hello prostředky zasílání zpráv v aplikaci níže do souboru Manifest.xml mezi hello `<application>` a `</application>` značky.

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

### <a name="specify-an-icon-for-notifications"></a>Určení ikony pro oznámení
Hello vložte následující fragment kódu XML do souboru Manifest.xml mezi hello `<application>` a `</application>` značky.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Definuje hello ikonu, která se zobrazí v systému a oznámení v aplikaci. Pro oznámení v aplikaci je ikona volitelná, ale pro systémová oznámení povinná. Android odmítá systémová oznámení s neplatnými ikonami.

Ujistěte se, že použijete ikonu, která existuje v jednom z hello **drawable** složky (například ``engagement_close.png``). Není dostupná podpora složky **mipmap**.

> [!NOTE]
> Neměli byste používat hello **Spouštěč** ikonu. Má jiné rozlišení a je obvykle ve složkách mipmap hello, které nepodporujeme.
> 
> 

Ve skutečných aplikacích můžete použít ikonu vhodnou pro oznámení podle [pokynů pro návrh Androidu](http://developer.android.com/design/patterns/notifications.html).

> [!TIP]
> zda toouse toobe správné rozlišení ikon, můžete se podívat na [tyto příklady](https://www.google.com/design/icons).
> Projděte dolů toohello **oznámení** části, klikněte na ikonu a pak klikněte na tlačítko `PNGS` toodownload hello sadu drawable ikony. Můžete zobrazit složky drawable s které toouse řešení pro každou verzi ikony hello.
> 
> 

### <a name="enable-your-app-tooreceive-gcm-push-notifications"></a>Povolení nabízených oznámení GCM tooreceive vaší aplikace
1. Vložte následující hello do souboru Manifest.xml mezi hello `<application>` a `</application>` značky po nahrazení hello **ID odesílatele** získané z konzoly Firebase projektu. Hello \n je úmyslné proto se ujistěte, že se vám stát čísla projektu hello.
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. Vložte kód hello pod do souboru Manifest.xml mezi hello `<application>` a `</application>` značky. Nahraďte název balíčku hello <Your package name>.
   
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
3. Přidejte poslední sadu oprávnění, která je zvýrazněná, před hello hello `<application>` značky. Nahraďte `<Your package name>` podle hello skutečným názvem balíčku aplikace.
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

