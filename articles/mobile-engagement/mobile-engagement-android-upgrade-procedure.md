---
title: Integraci sady Azure Mobile Engagement Android SDK
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
ms.openlocfilehash: 1f047f93fa8bc852b28c86e91d0c007a94fb4299
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="29ee1-103">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="29ee1-103">Upgrade procedures</span></span>
<span data-ttu-id="29ee1-104">Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, je nutné zvážit následující body při upgradu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="29ee1-104">If you already have integrated an older version of our SDK into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="29ee1-105">Možná budete muset několik postupy použijte, pokud provedena několik verzí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="29ee1-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="29ee1-106">Například pokud migrujete z 1.4.0 1.6.0 budete muset nejdřív postupujte podle pokynů "od 1.4.0 k 1.5.0" pak postupu "od 1.5.0 k 1.6.0".</span><span class="sxs-lookup"><span data-stu-id="29ee1-106">For example if you migrate from 1.4.0 to 1.6.0 you have to first follow the "from 1.4.0 to 1.5.0" procedure then the "from 1.5.0 to 1.6.0" procedure.</span></span>

<span data-ttu-id="29ee1-107">Bez ohledu na verzi upgradujete, budete muset nahradit `mobile-engagement-VERSION.jar` tímto novým připojením.</span><span class="sxs-lookup"><span data-stu-id="29ee1-107">Whatever the version you upgrade from, you have to replace the `mobile-engagement-VERSION.jar` with the new one.</span></span>

## <a name="from-420-to-421"></a><span data-ttu-id="29ee1-108">Z 4.2.0 k 4.2.1</span><span class="sxs-lookup"><span data-stu-id="29ee1-108">From 4.2.0 to 4.2.1</span></span>
<span data-ttu-id="29ee1-109">Tento krok lze provést ve skutečnosti na všechny verze sady SDK, je zvýšení zabezpečení při integraci Reach aktivity.</span><span class="sxs-lookup"><span data-stu-id="29ee1-109">This step can actually be done on any version of the SDK, its a security improvement when you integrate Reach activities.</span></span>

<span data-ttu-id="29ee1-110">Nyní byste měli přidat `exported="false"` pro všechny aktivity Reach.</span><span class="sxs-lookup"><span data-stu-id="29ee1-110">You should now add `exported="false"` to all Reach activities.</span></span>

<span data-ttu-id="29ee1-111">Reach aktivity by teď měl vypadat takto vaše `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="29ee1-111">Reach activities should now look like this on your `AndroidManifest.xml`:</span></span>

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

## <a name="from-400-to-410"></a><span data-ttu-id="29ee1-112">Z 4.0.0 k 4.1.0</span><span class="sxs-lookup"><span data-stu-id="29ee1-112">From 4.0.0 to 4.1.0</span></span>
<span data-ttu-id="29ee1-113">SDK nyní popisovač nové oprávnění modelu ze systému Android M.</span><span class="sxs-lookup"><span data-stu-id="29ee1-113">The SDK now handle new permission model from Android M.</span></span>

<span data-ttu-id="29ee1-114">Pokud používáte umístění funkcí nebo oznámení velký obrázek přečtěte [v této části](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="29ee1-114">If you use location features or big picture notifications please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>

<span data-ttu-id="29ee1-115">Kromě nový model oprávnění teď podporujeme konfiguraci funkce umístění za běhu.</span><span class="sxs-lookup"><span data-stu-id="29ee1-115">In addition to the new permission model, we now support configuring location features at runtime.</span></span>
<span data-ttu-id="29ee1-116">Snažíme se stále kompatibilní s manifestu parametry pro umístění, ale je nyní zastaralý.</span><span class="sxs-lookup"><span data-stu-id="29ee1-116">We are still compatible with the manifest parameters for location but it's now deprecated.</span></span> <span data-ttu-id="29ee1-117">Chcete-li použít konfigurace modulu runtime, odeberte v následujících částech z vaší ``AndroidManifest.xml``:</span><span class="sxs-lookup"><span data-stu-id="29ee1-117">To use runtime configuration, remove the following sections from your ``AndroidManifest.xml``:</span></span>

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

<span data-ttu-id="29ee1-118">a číst [Tato aktualizuje postup](mobile-engagement-android-integrate-engagement.md#location-reporting) místo toho použít konfigurace modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="29ee1-118">and read [this updated procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) to use runtime configuration instead.</span></span>

## <a name="from-300-to-400"></a><span data-ttu-id="29ee1-119">Z 3.0.0 k 4.0.0</span><span class="sxs-lookup"><span data-stu-id="29ee1-119">From 3.0.0 to 4.0.0</span></span>
### <a name="native-push"></a><span data-ttu-id="29ee1-120">Nativního nabízení</span><span class="sxs-lookup"><span data-stu-id="29ee1-120">Native push</span></span>
<span data-ttu-id="29ee1-121">Nativního nabízení (GCM/ADM) se teď také používá pro oznámení v aplikaci, musíte nakonfigurovat přihlašovací údaje nativního nabízení pro jakýkoli typ nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="29ee1-121">Native push (GCM/ADM) is now also used for in app notifications so you must configure the native push credentials for any type of push campaign.</span></span>

<span data-ttu-id="29ee1-122">Není-li již postupujte [tento postup](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span><span class="sxs-lookup"><span data-stu-id="29ee1-122">If not already done please follow [this procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="29ee1-123">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="29ee1-123">AndroidManifest.xml</span></span>
<span data-ttu-id="29ee1-124">Integrace reach byl změněn v ``AndroidManifest.xml``.</span><span class="sxs-lookup"><span data-stu-id="29ee1-124">Reach integration has been modified in ``AndroidManifest.xml``.</span></span>

<span data-ttu-id="29ee1-125">Nahraďte toto:</span><span class="sxs-lookup"><span data-stu-id="29ee1-125">Replace this:</span></span>

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

<span data-ttu-id="29ee1-126">Od společnosti</span><span class="sxs-lookup"><span data-stu-id="29ee1-126">By</span></span>

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

<span data-ttu-id="29ee1-127">Je to pravděpodobně načítání obrazovky teď kliknutím na oznámení (s text nebo webového obsahu) nebo hlasování.</span><span class="sxs-lookup"><span data-stu-id="29ee1-127">There is possibly a loading screen now when you click on an announcement (with text/web content) or a poll.</span></span>
<span data-ttu-id="29ee1-128">Je nutné přidat tato funkce má u těchto kampaně pro práci v 4.0.0:</span><span class="sxs-lookup"><span data-stu-id="29ee1-128">You have to add this for those campaigns to work in 4.0.0:</span></span>

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a><span data-ttu-id="29ee1-129">Zdroje</span><span class="sxs-lookup"><span data-stu-id="29ee1-129">Resources</span></span>
<span data-ttu-id="29ee1-130">Vložení nové `res/layout/engagement_loading.xml` souboru do projektu.</span><span class="sxs-lookup"><span data-stu-id="29ee1-130">Embed the new `res/layout/engagement_loading.xml` file into your project.</span></span>

## <a name="from-240-to-300"></a><span data-ttu-id="29ee1-131">Z 2.4.0 k 3.0.0</span><span class="sxs-lookup"><span data-stu-id="29ee1-131">From 2.4.0 to 3.0.0</span></span>
<span data-ttu-id="29ee1-132">Následující část popisuje postup migrace integraci sady SDK z Capptain služby, které do aplikace používá technologii Azure Mobile Engagement nabízí Capptain SAS.</span><span class="sxs-lookup"><span data-stu-id="29ee1-132">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> <span data-ttu-id="29ee1-133">Pokud provádíte migraci ze starší verze, najdete na webu Capptain nejdřív přenést 2.4.0 a potom použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="29ee1-133">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 2.4.0 first and then apply the following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29ee1-134">Capptain a Mobile Engagement nejsou stejné služby a postup níže uvedené jenom dozvíte, jak migrovat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="29ee1-134">Capptain and Mobile Engagement are not the same services, and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="29ee1-135">Migrace sady SDK v aplikaci není migrovat data ze serverů Capptain na servery Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="29ee1-135">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers.</span></span>
> 
> 

### <a name="jar-file"></a><span data-ttu-id="29ee1-136">Soubor JAR</span><span class="sxs-lookup"><span data-stu-id="29ee1-136">JAR file</span></span>
<span data-ttu-id="29ee1-137">Nahraďte `capptain.jar` podle `mobile-engagement-VERSION.jar` ve vaší `libs` složky.</span><span class="sxs-lookup"><span data-stu-id="29ee1-137">Replace `capptain.jar` by `mobile-engagement-VERSION.jar` in your `libs` folder.</span></span>

### <a name="resource-files"></a><span data-ttu-id="29ee1-138">Soubory prostředků</span><span class="sxs-lookup"><span data-stu-id="29ee1-138">Resource files</span></span>
<span data-ttu-id="29ee1-139">Každý soubor prostředků, který jsme zadali (s předponou podle `capptain_`) má nahradit za nové (s předponou `engagement_`).</span><span class="sxs-lookup"><span data-stu-id="29ee1-139">Every resource file that we provided (prefixed by `capptain_`) has to be replaced by the new ones (prefixed with `engagement_`).</span></span>

<span data-ttu-id="29ee1-140">Pokud jste si přizpůsobili tyto soubory, budete muset znovu použít vlastní u nových souborů **všechny identifikátory v souborech prostředků také přejmenovaná**.</span><span class="sxs-lookup"><span data-stu-id="29ee1-140">If you customized those files, you have to re-apply your customization on the new files, **all the identifiers in the resource files have also been renamed**.</span></span>

### <a name="application-id"></a><span data-ttu-id="29ee1-141">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="29ee1-141">Application ID</span></span>
<span data-ttu-id="29ee1-142">Zapojení teď používá připojovací řetězec konfigurace identifikátory SDK například identifikátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="29ee1-142">Now Engagement uses a connection string to configure the SDK identifiers such as the application identifier.</span></span>

<span data-ttu-id="29ee1-143">Budete muset použít `EngagementAgent.init` metoda aktivitou Spouštěče takto:</span><span class="sxs-lookup"><span data-stu-id="29ee1-143">You have to use `EngagementAgent.init` method in your launcher activity like this:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="29ee1-144">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="29ee1-144">The connection string for your application is displayed on Azure Portal.</span></span>

<span data-ttu-id="29ee1-145">Odeberte všechny volání `CapptainAgent.configure` jako `EngagementAgent.init` nahrazuje dané metody.</span><span class="sxs-lookup"><span data-stu-id="29ee1-145">Please remove any call to `CapptainAgent.configure` as `EngagementAgent.init` replaces that method.</span></span>

<span data-ttu-id="29ee1-146">`appId` Již jde konfigurovat pomocí `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-146">The `appId` can no longer be configured using `AndroidManifest.xml`.</span></span>

<span data-ttu-id="29ee1-147">Odeberte z této části vaší `AndroidManifest.xml` pokud:</span><span class="sxs-lookup"><span data-stu-id="29ee1-147">Please remove this section from your `AndroidManifest.xml` if you have it:</span></span>

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a><span data-ttu-id="29ee1-148">Java API</span><span class="sxs-lookup"><span data-stu-id="29ee1-148">Java API</span></span>
<span data-ttu-id="29ee1-149">Každé volání jakákoli Třída Java naše SDK musí být přejmenována; například `CapptainAgent.getInstance(this)` musí být přejmenován `EngagementAgent.getInstance(this)`, `extends CapptainActivity` musí být přejmenován `extends EngagementActivity` atd...</span><span class="sxs-lookup"><span data-stu-id="29ee1-149">Every call to any Java class of our SDK has to be renamed; for example, `CapptainAgent.getInstance(this)` must be renamed `EngagementAgent.getInstance(this)`, `extends CapptainActivity` must be renamed `extends EngagementActivity` etc...</span></span>

<span data-ttu-id="29ee1-150">Pokud byly integrovány s výchozí agenta předvoleb soubory, výchozí název souboru je nyní `engagement.agent` a zda je klíč `engagement:agent`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-150">If you were integrated with default agent preference files, the default file name is now `engagement.agent` and the key is `engagement:agent`.</span></span>

<span data-ttu-id="29ee1-151">Při vytváření webové oznámení, vazač Javascript je nyní `engagementReachContent`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-151">When creating web announcements, the Javascript binder is now `engagementReachContent`.</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="29ee1-152">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="29ee1-152">AndroidManifest.xml</span></span>
<span data-ttu-id="29ee1-153">Mnoho změn došlo k dispozici, služba není sdíleny a spoustu příjemci už nejsou exportovatelný.</span><span class="sxs-lookup"><span data-stu-id="29ee1-153">A lot of changes happened there, the service is not shared anymore, and a lot of receivers are not exportable anymore.</span></span>

<span data-ttu-id="29ee1-154">Deklarace služby je teď jednodušší; Odeberte záměrné filtru a všechny metadat je uvnitř a přidejte `exportable=false`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-154">The service declaration is now simpler; remove the intent filter and all meta-data inside it, and add `exportable=false`.</span></span>

<span data-ttu-id="29ee1-155">Plus všechno, co je přejmenován na používat engagement.</span><span class="sxs-lookup"><span data-stu-id="29ee1-155">Plus everything is renamed to use engagement.</span></span>

<span data-ttu-id="29ee1-156">Teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="29ee1-156">It now looks like:</span></span>

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

<span data-ttu-id="29ee1-157">Pokud chcete povolit protokolů testování, meta-data nyní byl přesunut do aplikace značky a bylo přejmenováno:</span><span class="sxs-lookup"><span data-stu-id="29ee1-157">When you want to enable test logs, the meta-data has now been moved to the application tag and has been renamed:</span></span>

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

<span data-ttu-id="29ee1-158">Všechny ostatní metadata právě přejmenovaná, zde je úplný seznam (přejmenování kurzu jen ty, které používáte):</span><span class="sxs-lookup"><span data-stu-id="29ee1-158">All other meta-data have just been renamed, here is the full list (of course rename only the ones you use):</span></span>

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

<span data-ttu-id="29ee1-159">Google Play a SmartAd sledování byl odebrán z SDK, musíte se odebrat toto bez nahrazení:</span><span class="sxs-lookup"><span data-stu-id="29ee1-159">Google Play and SmartAd tracking has been removed from SDK you just have to remove this without replacement:</span></span>

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

<span data-ttu-id="29ee1-160">Reach aktivity jsou nyní deklarované takto:</span><span class="sxs-lookup"><span data-stu-id="29ee1-160">The Reach activities are now declared like this:</span></span>

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

<span data-ttu-id="29ee1-161">Pokud máte vlastní aktivity Reach, potřebujete jenom změnit záměrné akce, které odpovídají buď `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` nebo `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-161">If you have custom Reach activities, you need only to change the intent actions to match either `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` or `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span></span>

<span data-ttu-id="29ee1-162">Přejmenovaná všesměrového vysílání příjemci plus nyní přidáme `exported=false`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-162">The broadcast receivers have been renamed, plus we now add `exported=false`.</span></span> <span data-ttu-id="29ee1-163">Tady je úplný seznam příjemců s novou specifikaci, (přejmenování kurzu jen ty, které používáte):</span><span class="sxs-lookup"><span data-stu-id="29ee1-163">Here is the full list of the receivers with the new specification, (of course rename only the ones you use):</span></span>

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

<span data-ttu-id="29ee1-164">Sledování příjemce byl odebrán, takže budete muset odebrat v této části:</span><span class="sxs-lookup"><span data-stu-id="29ee1-164">Tracking receiver has been removed, so you have to remove this section:</span></span>

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

<span data-ttu-id="29ee1-165">Všimněte si, že prohlášení o implementaci všesměrového vysílání příjemce **EngagementMessageReceiver** došlo ke změně v `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="29ee1-165">Note that the declaration of your implementation of the broadcast receiver **EngagementMessageReceiver** has changed in the `AndroidManifest.xml`.</span></span> <span data-ttu-id="29ee1-166">Je to proto, že byly odstraněny rozhraní API k odeslání a odebrat libovolný protokolu XMPP zprávy z libovolné protokolu XMPP entit a rozhraní API pro odesílání a přijímání zpráv mezi zařízeními.</span><span class="sxs-lookup"><span data-stu-id="29ee1-166">This is because the API to send and remove arbitrary XMPP messages from arbitrary XMPP entities and the API to send and receive messages between devices have been removed.</span></span> <span data-ttu-id="29ee1-167">Proto je třeba také odstranit následující zpětná volání z vaší **EngagementMessageReceiver** implementace:</span><span class="sxs-lookup"><span data-stu-id="29ee1-167">Thus, you have also to delete the following callbacks from your **EngagementMessageReceiver** implementation :</span></span>

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

<span data-ttu-id="29ee1-168">a</span><span class="sxs-lookup"><span data-stu-id="29ee1-168">and</span></span>

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

<span data-ttu-id="29ee1-169">potom odstraňte v žádném volání, **EngagementAgent** pro:</span><span class="sxs-lookup"><span data-stu-id="29ee1-169">then delete any call on **EngagementAgent** for :</span></span>

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

<span data-ttu-id="29ee1-170">a</span><span class="sxs-lookup"><span data-stu-id="29ee1-170">and</span></span>

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a><span data-ttu-id="29ee1-171">Proguard</span><span class="sxs-lookup"><span data-stu-id="29ee1-171">Proguard</span></span>
<span data-ttu-id="29ee1-172">Proguard konfigurace může být ovlivněno rebranding, pravidla jsou nyní vyhledávání jako:</span><span class="sxs-lookup"><span data-stu-id="29ee1-172">Proguard configuration can be impacted by rebranding, the rules are now looking like:</span></span>

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

