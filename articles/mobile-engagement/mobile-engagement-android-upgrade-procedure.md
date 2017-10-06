---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: df5c82812fe0a242eaa5df8c906030237215b7eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="57e44-103">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="57e44-103">Upgrade procedures</span></span>
<span data-ttu-id="57e44-104">Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.</span><span class="sxs-lookup"><span data-stu-id="57e44-104">If you already have integrated an older version of our SDK into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="57e44-105">Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="57e44-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="57e44-106">Například pokud migrujete z 1.4.0 too1.6.0 máte toofirst postupujte podle hello "z 1.4.0 too1.5.0" postup pak hello "z 1.5.0 too1.6.0" postup.</span><span class="sxs-lookup"><span data-stu-id="57e44-106">For example if you migrate from 1.4.0 too1.6.0 you have toofirst follow hello "from 1.4.0 too1.5.0" procedure then hello "from 1.5.0 too1.6.0" procedure.</span></span>

<span data-ttu-id="57e44-107">Ať hello verze upgradujete, budete mít tooreplace hello `mobile-engagement-VERSION.jar` s hello nový.</span><span class="sxs-lookup"><span data-stu-id="57e44-107">Whatever hello version you upgrade from, you have tooreplace hello `mobile-engagement-VERSION.jar` with hello new one.</span></span>

## <a name="from-420-too421"></a><span data-ttu-id="57e44-108">Z 4.2.0 too4.2.1</span><span class="sxs-lookup"><span data-stu-id="57e44-108">From 4.2.0 too4.2.1</span></span>
<span data-ttu-id="57e44-109">Tento krok lze provést ve skutečnosti na libovolnou verzi systému hello SDK, je zvýšení zabezpečení při integraci Reach aktivity.</span><span class="sxs-lookup"><span data-stu-id="57e44-109">This step can actually be done on any version of hello SDK, its a security improvement when you integrate Reach activities.</span></span>

<span data-ttu-id="57e44-110">Nyní byste měli přidat `exported="false"` tooall Reach aktivity.</span><span class="sxs-lookup"><span data-stu-id="57e44-110">You should now add `exported="false"` tooall Reach activities.</span></span>

<span data-ttu-id="57e44-111">Reach aktivity by teď měl vypadat takto vaše `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="57e44-111">Reach activities should now look like this on your `AndroidManifest.xml`:</span></span>

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

## <a name="from-400-too410"></a><span data-ttu-id="57e44-112">Z 4.0.0 too4.1.0</span><span class="sxs-lookup"><span data-stu-id="57e44-112">From 4.0.0 too4.1.0</span></span>
<span data-ttu-id="57e44-113">Hello SDK nyní popisovač nové oprávnění modelu ze systému Android M.</span><span class="sxs-lookup"><span data-stu-id="57e44-113">hello SDK now handle new permission model from Android M.</span></span>

<span data-ttu-id="57e44-114">Pokud používáte umístění funkcí nebo oznámení velký obrázek přečtěte [v této části](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="57e44-114">If you use location features or big picture notifications please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>

<span data-ttu-id="57e44-115">Kromě toho toohello nový model oprávnění, teď podporujeme konfiguraci funkcí umístění za běhu.</span><span class="sxs-lookup"><span data-stu-id="57e44-115">In addition toohello new permission model, we now support configuring location features at runtime.</span></span>
<span data-ttu-id="57e44-116">Snažíme se stále kompatibilní s hello manifestu parametry pro umístění, ale je nyní zastaralý.</span><span class="sxs-lookup"><span data-stu-id="57e44-116">We are still compatible with hello manifest parameters for location but it's now deprecated.</span></span> <span data-ttu-id="57e44-117">Konfigurace modulu runtime toouse, odeberte hello následující části z vaší ``AndroidManifest.xml``:</span><span class="sxs-lookup"><span data-stu-id="57e44-117">toouse runtime configuration, remove hello following sections from your ``AndroidManifest.xml``:</span></span>

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

<span data-ttu-id="57e44-118">a číst [Tato aktualizuje postup](mobile-engagement-android-integrate-engagement.md#location-reporting) konfigurace modulu runtime toouse místo.</span><span class="sxs-lookup"><span data-stu-id="57e44-118">and read [this updated procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse runtime configuration instead.</span></span>

## <a name="from-300-too400"></a><span data-ttu-id="57e44-119">Z 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="57e44-119">From 3.0.0 too4.0.0</span></span>
### <a name="native-push"></a><span data-ttu-id="57e44-120">Nativního nabízení</span><span class="sxs-lookup"><span data-stu-id="57e44-120">Native push</span></span>
<span data-ttu-id="57e44-121">Nativního nabízení (GCM/ADM) se teď také používá pro oznámení v aplikaci, musíte nakonfigurovat přihlašovací údaje nativního nabízení hello pro jakýkoli typ nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="57e44-121">Native push (GCM/ADM) is now also used for in app notifications so you must configure hello native push credentials for any type of push campaign.</span></span>

<span data-ttu-id="57e44-122">Není-li již postupujte [tento postup](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span><span class="sxs-lookup"><span data-stu-id="57e44-122">If not already done please follow [this procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="57e44-123">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="57e44-123">AndroidManifest.xml</span></span>
<span data-ttu-id="57e44-124">Integrace reach byl změněn v ``AndroidManifest.xml``.</span><span class="sxs-lookup"><span data-stu-id="57e44-124">Reach integration has been modified in ``AndroidManifest.xml``.</span></span>

<span data-ttu-id="57e44-125">Nahraďte toto:</span><span class="sxs-lookup"><span data-stu-id="57e44-125">Replace this:</span></span>

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

<span data-ttu-id="57e44-126">Od společnosti</span><span class="sxs-lookup"><span data-stu-id="57e44-126">By</span></span>

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

<span data-ttu-id="57e44-127">Je to pravděpodobně načítání obrazovky teď kliknutím na oznámení (s text nebo webového obsahu) nebo hlasování.</span><span class="sxs-lookup"><span data-stu-id="57e44-127">There is possibly a loading screen now when you click on an announcement (with text/web content) or a poll.</span></span>
<span data-ttu-id="57e44-128">Máte tooadd to pro tyto toowork kampaně v 4.0.0:</span><span class="sxs-lookup"><span data-stu-id="57e44-128">You have tooadd this for those campaigns toowork in 4.0.0:</span></span>

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a><span data-ttu-id="57e44-129">Zdroje</span><span class="sxs-lookup"><span data-stu-id="57e44-129">Resources</span></span>
<span data-ttu-id="57e44-130">Vložení nové hello `res/layout/engagement_loading.xml` souboru do projektu.</span><span class="sxs-lookup"><span data-stu-id="57e44-130">Embed hello new `res/layout/engagement_loading.xml` file into your project.</span></span>

## <a name="from-240-too300"></a><span data-ttu-id="57e44-131">Z 2.4.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="57e44-131">From 2.4.0 too3.0.0</span></span>
<span data-ttu-id="57e44-132">Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="57e44-132">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> <span data-ttu-id="57e44-133">Pokud provádíte migraci ze starší verze, informujte hello Capptain webu toomigrate too2.4.0 nejprve a potom použijte hello následující postup.</span><span class="sxs-lookup"><span data-stu-id="57e44-133">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too2.4.0 first and then apply hello following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57e44-134">Capptain Mobile Engagement není hello stejné služby a hello postup vypsáni níže pouze označuje, jak toomigrate hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57e44-134">Capptain and Mobile Engagement are not hello same services, and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="57e44-135">Migrace hello SDK v aplikaci hello se NEPROVÁDÍ migraci dat ze sady Mobile Engagement pro toohello hello Capptain servery.</span><span class="sxs-lookup"><span data-stu-id="57e44-135">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers.</span></span>
> 
> 

### <a name="jar-file"></a><span data-ttu-id="57e44-136">Soubor JAR</span><span class="sxs-lookup"><span data-stu-id="57e44-136">JAR file</span></span>
<span data-ttu-id="57e44-137">Nahraďte `capptain.jar` podle `mobile-engagement-VERSION.jar` ve vaší `libs` složky.</span><span class="sxs-lookup"><span data-stu-id="57e44-137">Replace `capptain.jar` by `mobile-engagement-VERSION.jar` in your `libs` folder.</span></span>

### <a name="resource-files"></a><span data-ttu-id="57e44-138">Soubory prostředků</span><span class="sxs-lookup"><span data-stu-id="57e44-138">Resource files</span></span>
<span data-ttu-id="57e44-139">Každý soubor prostředků, který jsme zadali (s předponou podle `capptain_`) toobe nahradili hello nové (s předponou `engagement_`).</span><span class="sxs-lookup"><span data-stu-id="57e44-139">Every resource file that we provided (prefixed by `capptain_`) has toobe replaced by hello new ones (prefixed with `engagement_`).</span></span>

<span data-ttu-id="57e44-140">Pokud jste si přizpůsobili těchto souborů, máte toore-použít vlastní na hello nové soubory, **všechny identifikátory hello v souborech prostředků hello také přejmenovaná**.</span><span class="sxs-lookup"><span data-stu-id="57e44-140">If you customized those files, you have toore-apply your customization on hello new files, **all hello identifiers in hello resource files have also been renamed**.</span></span>

### <a name="application-id"></a><span data-ttu-id="57e44-141">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="57e44-141">Application ID</span></span>
<span data-ttu-id="57e44-142">Teď používá Engagement SDK identifikátorů připojovacího řetězce tooconfigure hello například identifikátor aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="57e44-142">Now Engagement uses a connection string tooconfigure hello SDK identifiers such as hello application identifier.</span></span>

<span data-ttu-id="57e44-143">Máte toouse `EngagementAgent.init` metoda aktivitou Spouštěče takto:</span><span class="sxs-lookup"><span data-stu-id="57e44-143">You have toouse `EngagementAgent.init` method in your launcher activity like this:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="57e44-144">Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="57e44-144">hello connection string for your application is displayed on Azure Portal.</span></span>

<span data-ttu-id="57e44-145">Odeberte všechny volání příliš`CapptainAgent.configure` jako `EngagementAgent.init` nahrazuje dané metody.</span><span class="sxs-lookup"><span data-stu-id="57e44-145">Please remove any call too`CapptainAgent.configure` as `EngagementAgent.init` replaces that method.</span></span>

<span data-ttu-id="57e44-146">Hello `appId` již jde konfigurovat pomocí `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="57e44-146">hello `appId` can no longer be configured using `AndroidManifest.xml`.</span></span>

<span data-ttu-id="57e44-147">Odeberte z této části vaší `AndroidManifest.xml` pokud:</span><span class="sxs-lookup"><span data-stu-id="57e44-147">Please remove this section from your `AndroidManifest.xml` if you have it:</span></span>

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a><span data-ttu-id="57e44-148">Java API</span><span class="sxs-lookup"><span data-stu-id="57e44-148">Java API</span></span>
<span data-ttu-id="57e44-149">Každé volání tooany třída jazyka Java naše SDK má toobe přejmenovat; například `CapptainAgent.getInstance(this)` musí být přejmenován `EngagementAgent.getInstance(this)`, `extends CapptainActivity` musí být přejmenován `extends EngagementActivity` atd...</span><span class="sxs-lookup"><span data-stu-id="57e44-149">Every call tooany Java class of our SDK has toobe renamed; for example, `CapptainAgent.getInstance(this)` must be renamed `EngagementAgent.getInstance(this)`, `extends CapptainActivity` must be renamed `extends EngagementActivity` etc...</span></span>

<span data-ttu-id="57e44-150">Pokud byly integrovány s výchozí agenta předvoleb soubory, hello výchozí název souboru je nyní `engagement.agent` a hello je klíč `engagement:agent`.</span><span class="sxs-lookup"><span data-stu-id="57e44-150">If you were integrated with default agent preference files, hello default file name is now `engagement.agent` and hello key is `engagement:agent`.</span></span>

<span data-ttu-id="57e44-151">Při vytváření webové oznámení, hello vazač Javascript je nyní `engagementReachContent`.</span><span class="sxs-lookup"><span data-stu-id="57e44-151">When creating web announcements, hello Javascript binder is now `engagementReachContent`.</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="57e44-152">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="57e44-152">AndroidManifest.xml</span></span>
<span data-ttu-id="57e44-153">Mnoho změn došlo k dispozici, není sdíleny hello služby a spoustu příjemci už nejsou exportovatelný.</span><span class="sxs-lookup"><span data-stu-id="57e44-153">A lot of changes happened there, hello service is not shared anymore, and a lot of receivers are not exportable anymore.</span></span>

<span data-ttu-id="57e44-154">deklarace Hello služby je teď jednodušší; Odeberte hello záměrné filtru a všechny metadat je uvnitř a přidejte `exportable=false`.</span><span class="sxs-lookup"><span data-stu-id="57e44-154">hello service declaration is now simpler; remove hello intent filter and all meta-data inside it, and add `exportable=false`.</span></span>

<span data-ttu-id="57e44-155">Plus všechno, co je přejmenován toouse zapojení.</span><span class="sxs-lookup"><span data-stu-id="57e44-155">Plus everything is renamed toouse engagement.</span></span>

<span data-ttu-id="57e44-156">Teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="57e44-156">It now looks like:</span></span>

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

<span data-ttu-id="57e44-157">Když chcete tooenable protokolů testování, hello metadata byla nyní přesunout toohello aplikace značky a bylo přejmenováno:</span><span class="sxs-lookup"><span data-stu-id="57e44-157">When you want tooenable test logs, hello meta-data has now been moved toohello application tag and has been renamed:</span></span>

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

<span data-ttu-id="57e44-158">Všechny ostatní metadata právě přejmenovaná, zde je úplný seznam hello (samozřejmě přejmenovat pouze hello ty, které jsou použijete):</span><span class="sxs-lookup"><span data-stu-id="57e44-158">All other meta-data have just been renamed, here is hello full list (of course rename only hello ones you use):</span></span>

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

<span data-ttu-id="57e44-159">Sledování Google Play a SmartAd byla odebrána ze sady SDK právě máte tooremove to bez nahrazení:</span><span class="sxs-lookup"><span data-stu-id="57e44-159">Google Play and SmartAd tracking has been removed from SDK you just have tooremove this without replacement:</span></span>

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

<span data-ttu-id="57e44-160">Hello Reach aktivity jsou nyní deklarované takto:</span><span class="sxs-lookup"><span data-stu-id="57e44-160">hello Reach activities are now declared like this:</span></span>

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

<span data-ttu-id="57e44-161">Pokud máte vlastní aktivity Reach, potřebujete jenom toochange hello záměrné akce toomatch buď `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` nebo `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span><span class="sxs-lookup"><span data-stu-id="57e44-161">If you have custom Reach activities, you need only toochange hello intent actions toomatch either `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` or `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span></span>

<span data-ttu-id="57e44-162">Hello všesměrového vysílání příjemci přejmenovaná plus nyní přidáme `exported=false`.</span><span class="sxs-lookup"><span data-stu-id="57e44-162">hello broadcast receivers have been renamed, plus we now add `exported=false`.</span></span> <span data-ttu-id="57e44-163">Tady je hello úplný seznam příjemců hello nové specifikace hello (samozřejmě přejmenovat pouze hello ty, které jsou použijete):</span><span class="sxs-lookup"><span data-stu-id="57e44-163">Here is hello full list of hello receivers with hello new specification, (of course rename only hello ones you use):</span></span>

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

<span data-ttu-id="57e44-164">Sledování příjemce byl odebrán, takže budete mít tooremove v této části:</span><span class="sxs-lookup"><span data-stu-id="57e44-164">Tracking receiver has been removed, so you have tooremove this section:</span></span>

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

<span data-ttu-id="57e44-165">Všimněte si, že hello prohlášení o implementaci hello vysílání příjemce **EngagementMessageReceiver** došlo ke změně v hello `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="57e44-165">Note that hello declaration of your implementation of hello broadcast receiver **EngagementMessageReceiver** has changed in hello `AndroidManifest.xml`.</span></span> <span data-ttu-id="57e44-166">To je proto toosend hello rozhraní API a odebrat libovolný protokolu XMPP zprávy z libovolné protokolu XMPP entit a hello toosend rozhraní API a příjem zpráv mezi zařízeními byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="57e44-166">This is because hello API toosend and remove arbitrary XMPP messages from arbitrary XMPP entities and hello API toosend and receive messages between devices have been removed.</span></span> <span data-ttu-id="57e44-167">Proto máte také toodelete hello následující zpětná volání z vaší **EngagementMessageReceiver** implementace:</span><span class="sxs-lookup"><span data-stu-id="57e44-167">Thus, you have also toodelete hello following callbacks from your **EngagementMessageReceiver** implementation :</span></span>

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

<span data-ttu-id="57e44-168">a</span><span class="sxs-lookup"><span data-stu-id="57e44-168">and</span></span>

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

<span data-ttu-id="57e44-169">potom odstraňte v žádném volání, **EngagementAgent** pro:</span><span class="sxs-lookup"><span data-stu-id="57e44-169">then delete any call on **EngagementAgent** for :</span></span>

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

<span data-ttu-id="57e44-170">a</span><span class="sxs-lookup"><span data-stu-id="57e44-170">and</span></span>

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a><span data-ttu-id="57e44-171">Proguard</span><span class="sxs-lookup"><span data-stu-id="57e44-171">Proguard</span></span>
<span data-ttu-id="57e44-172">Proguard konfigurace může být ovlivněno rebranding hello pravidla jsou nyní vyhledávání jako:</span><span class="sxs-lookup"><span data-stu-id="57e44-172">Proguard configuration can be impacted by rebranding, hello rules are now looking like:</span></span>

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

