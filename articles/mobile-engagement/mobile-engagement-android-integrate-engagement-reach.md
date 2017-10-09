---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 4ab6143771bdc0758a548abb529d6bde98fc0e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-android"></a><span data-ttu-id="764a3-103">Jak tooIntegrate Engagement Reach pro Android</span><span class="sxs-lookup"><span data-stu-id="764a3-103">How tooIntegrate Engagement Reach on Android</span></span>
> [!IMPORTANT]
> <span data-ttu-id="764a3-104">Postupujte podle hello integrace postup popsaný v hello jak tooIntegrate Engagement v systému Android dokumentu před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="764a3-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> 

## <a name="standard-integration"></a><span data-ttu-id="764a3-105">Standardní integrace</span><span class="sxs-lookup"><span data-stu-id="764a3-105">Standard integration</span></span>

<span data-ttu-id="764a3-106">Zkopírujte soubory prostředků Reach z hello SDK do projektu:</span><span class="sxs-lookup"><span data-stu-id="764a3-106">Copy Reach resource files from hello SDK in your project :</span></span>

* <span data-ttu-id="764a3-107">Zkopírujte soubory hello z hello `res/layout` složky doručit s hello SDK do hello `res/layout` složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="764a3-107">Copy hello files from hello `res/layout` folder delivered with hello SDK into hello `res/layout` folder of your application.</span></span>
* <span data-ttu-id="764a3-108">Zkopírujte soubory hello z hello `res/drawable` složky doručit s hello SDK do hello `res/drawable` složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="764a3-108">Copy hello files from hello `res/drawable` folder delivered with hello SDK into hello `res/drawable` folder of your application.</span></span>

<span data-ttu-id="764a3-109">Upravit vaše `AndroidManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="764a3-109">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="764a3-110">Přidejte následující části hello (mezi hello `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="764a3-110">Add hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
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
* <span data-ttu-id="764a3-111">Potřebujete tato oprávnění tooreplay systémová oznámení, které nebyly použity při spuštění (jinak zůstanou na disku, ale už se nezobrazí, skutečně máte tooinclude to).</span><span class="sxs-lookup"><span data-stu-id="764a3-111">You need this permission tooreplay system notifications that were not clicked at boot (otherwise they will be kept on disk but won't be displayed anymore, you really have tooinclude this).</span></span>
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* <span data-ttu-id="764a3-112">Určení ikony pro oznámení (v aplikaci a systému ty, které jsou i) používané kopírování a úpravy hello následující části (mezi hello `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="764a3-112">Specify an icon used for notifications (both in app and system ones) by copying and editing hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> <span data-ttu-id="764a3-113">Tato část se **povinné** Pokud máte v úmyslu používat systémová oznámení při vytváření kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="764a3-113">This section is **mandatory** if you plan on using system notifications when creating Reach campaigns.</span></span> <span data-ttu-id="764a3-114">Android zabrání systémová oznámení bez ikony se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="764a3-114">Android prevents system notifications without icons from being shown.</span></span> <span data-ttu-id="764a3-115">Takže pokud není v této části vaši koncoví uživatelé nebudou moct tooreceive je.</span><span class="sxs-lookup"><span data-stu-id="764a3-115">So if you omit this section, your end users will not be able tooreceive them.</span></span>
> 
> 

* <span data-ttu-id="764a3-116">Pokud vytvoříte kampaně s systémová oznámení pomocí velký obrázek, potřebujete následující oprávnění hello tooadd (po hello `</application>` značka) Pokud chybí:</span><span class="sxs-lookup"><span data-stu-id="764a3-116">If you create campaigns with system notifications using big picture, you need tooadd hello following permissions (after hello `</application>` tag) if missing:</span></span>
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * <span data-ttu-id="764a3-117">V systému Android M a pokud aplikace cílí úroveň rozhraní API systému Android 23 nebo vyšší ``WRITE_EXTERNAL_STORAGE`` oprávnění vyžaduje schválení uživatelem.</span><span class="sxs-lookup"><span data-stu-id="764a3-117">On Android M and if your application targets Android API level 23 or greater, ``WRITE_EXTERNAL_STORAGE`` permission requires user approval.</span></span> <span data-ttu-id="764a3-118">Přečtěte si prosím [v této části](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="764a3-118">Please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>
* <span data-ttu-id="764a3-119">Pro systémová oznámení, které můžete také zadat v hello dosáhnout kampaň, pokud by se prstence nebo zavibrovat hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="764a3-119">For system notifications you can also specify in hello Reach campaign if hello device should ring and/or vibrate.</span></span> <span data-ttu-id="764a3-120">Pro něj toowork, máte jistotu deklarovaný hello následující oprávnění toomake (po hello `</application>` značka):</span><span class="sxs-lookup"><span data-stu-id="764a3-120">For it toowork, you have toomake sure you declared hello following permission (after hello `</application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  <span data-ttu-id="764a3-121">Bez tohoto oprávnění zabrání Android systémových oznámení se zobrazí v případě, že je zaškrtnuté hello prstenec nebo hello zavibrovat možnost v manažer kampaně Reach hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-121">Without this permission, Android prevents system notifications from being shown if you checked hello ring or hello vibrate option in hello Reach Campaign manager.</span></span>

## <a name="native-push"></a><span data-ttu-id="764a3-122">Nativního nabízení</span><span class="sxs-lookup"><span data-stu-id="764a3-122">Native Push</span></span>
<span data-ttu-id="764a3-123">Teď, když jste nakonfigurovali modul Reach, je nutné tooconfigure nativního nabízení toobe možné tooreceive hello kampaně v zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-123">Now that you configured Reach module, you need tooconfigure native push toobe able tooreceive hello campaigns on hello device.</span></span>

<span data-ttu-id="764a3-124">V systému Android podporujeme dvě služby:</span><span class="sxs-lookup"><span data-stu-id="764a3-124">We support two services on Android:</span></span>

* <span data-ttu-id="764a3-125">Google Play zařízení: použití [Google Cloud Messaging] podle následující hello [jak tooIntegrate GCM s Engagement průvodce](mobile-engagement-android-gcm-integrate.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="764a3-125">Google Play devices: Use [Google Cloud Messaging] by following hello [How tooIntegrate GCM with Engagement guide](mobile-engagement-android-gcm-integrate.md) guide.</span></span>
* <span data-ttu-id="764a3-126">Zařízení Amazon: použití [Amazon Device Messaging] podle následující hello [jak tooIntegrate ADM s Engagement průvodce](mobile-engagement-android-adm-integrate.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="764a3-126">Amazon devices: Use [Amazon Device Messaging] by following hello [How tooIntegrate ADM with Engagement guide](mobile-engagement-android-adm-integrate.md) guide.</span></span>

<span data-ttu-id="764a3-127">Pokud chcete, aby tootarget Amazon a webu Google Play zařízení, jeho možných toohave všechno uvnitř 1 AndroidManifest.xml/APK pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="764a3-127">If you want tootarget both Amazon and Google Play devices, its possible toohave everything inside 1 AndroidManifest.xml/APK for development.</span></span> <span data-ttu-id="764a3-128">Ale při odesílání tooAmazon, pokud se najdou GCM kódu může zamítnutí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="764a3-128">But when submitting tooAmazon, they may reject your application if they find GCM code.</span></span>

<span data-ttu-id="764a3-129">V takovém případě byste měli používat více APKs.</span><span class="sxs-lookup"><span data-stu-id="764a3-129">You should use multiple APKs in that case.</span></span>

<span data-ttu-id="764a3-130">**Aplikace je nyní připraven tooreceive a zobrazení kampaně reach!**</span><span class="sxs-lookup"><span data-stu-id="764a3-130">**Your application is now ready tooreceive and display reach campaigns!**</span></span>

## <a name="how-toohandle-data-push"></a><span data-ttu-id="764a3-131">Jak nabízená toohandle dat</span><span class="sxs-lookup"><span data-stu-id="764a3-131">How toohandle data push</span></span>
### <a name="integration"></a><span data-ttu-id="764a3-132">Integrace</span><span class="sxs-lookup"><span data-stu-id="764a3-132">Integration</span></span>
<span data-ttu-id="764a3-133">Pokud chcete toobe vaší aplikací mít tooreceive Reach data nabízených oznámení, je třeba toocreate podtřídou třídy `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` a odkazujete v hello `AndroidManifest.xml` souboru (mezi hello `<application>` nebo `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="764a3-133">If you want your application toobe able tooreceive Reach data pushes, you have toocreate a sub-class of `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` and reference it in hello `AndroidManifest.xml` file (between hello `<application>` and/or `</application>` tags):</span></span>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

<span data-ttu-id="764a3-134">Pak můžete přepsat hello `onDataPushStringReceived` a `onDataPushBase64Received` zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="764a3-134">Then you can override hello `onDataPushStringReceived` and `onDataPushBase64Received` callbacks.</span></span> <span data-ttu-id="764a3-135">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="764a3-135">Here is an example:</span></span>

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a><span data-ttu-id="764a3-136">Kategorie</span><span class="sxs-lookup"><span data-stu-id="764a3-136">Category</span></span>
<span data-ttu-id="764a3-137">Hello kategorie parametr je volitelný při vytváření kampaň nabízených Data a umožňuje že vám toofilter data nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="764a3-137">hello category parameter is optional when you create a Data Push campaign and allows you toofilter data pushes.</span></span> <span data-ttu-id="764a3-138">To je užitečné, pokud máte několik všesměrového vysílání příjemci zpracování různé typy dat nabízených oznámení, nebo pokud chcete různé typy toopush z `Base64` dat a chcete tooidentify jejich typů před analýzou je.</span><span class="sxs-lookup"><span data-stu-id="764a3-138">This is useful if you have several broadcast receivers handling different types of data pushes, or if you want toopush different kinds of `Base64` data and want tooidentify their type before parsing them.</span></span>

### <a name="callbacks-return-parameter"></a><span data-ttu-id="764a3-139">Návratový parametr zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="764a3-139">Callbacks' return parameter</span></span>
<span data-ttu-id="764a3-140">Tady jsou některé pokyny tooproperly popisovač hello návratový parametr `onDataPushStringReceived` a `onDataPushBase64Received`:</span><span class="sxs-lookup"><span data-stu-id="764a3-140">Here are some guidelines tooproperly handle hello return parameter of `onDataPushStringReceived` and `onDataPushBase64Received`:</span></span>

* <span data-ttu-id="764a3-141">Všesměrového vysílání příjemce by měla vrátit `null` v hello zpětného volání, pokud nebude vědět, jak nabízená toohandle data.</span><span class="sxs-lookup"><span data-stu-id="764a3-141">A broadcast receiver should return `null` in hello callback if it does not know how toohandle a data push.</span></span> <span data-ttu-id="764a3-142">Měli byste použít hello kategorie toodetermine zda přijímač všesměrového vysílání by měla řídit hello datová oznámení, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="764a3-142">You should use hello category toodetermine whether your broadcast receiver should handle hello data push or not.</span></span>
* <span data-ttu-id="764a3-143">Jeden z hello všesměrového vysílání příjemce by měla vrátit `true` v hello zpětného volání, pokud ji přijímá hello datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="764a3-143">One of hello broadcast receiver should return `true` in hello callback if it accepts hello data push.</span></span>
* <span data-ttu-id="764a3-144">Jeden z hello všesměrového vysílání příjemce by měla vrátit `false` v hello zpětného volání, pokud rozpozná hello datová oznámení, ale zahodí z jakéhokoliv důvodu.</span><span class="sxs-lookup"><span data-stu-id="764a3-144">One of hello broadcast receiver should return `false` in hello callback if it recognizes hello data push, but discards it for whatever reason.</span></span> <span data-ttu-id="764a3-145">Například vrátit `false` při hello přijal data jsou neplatná.</span><span class="sxs-lookup"><span data-stu-id="764a3-145">For example, return `false` when hello received data is invalid.</span></span>
* <span data-ttu-id="764a3-146">Pokud jeden vysílání příjemce vrátí `true` při jiné jeden vrátí `false` pro hello stejné datová oznámení, hello chování není definován, měli byste nikdy udělat.</span><span class="sxs-lookup"><span data-stu-id="764a3-146">If one broadcast receiver returns `true` while another one returns `false` for hello same data push, hello behavior is undefined, you should never do that.</span></span>

<span data-ttu-id="764a3-147">Hello návratový typ se používá pouze pro hello Reach statistiky:</span><span class="sxs-lookup"><span data-stu-id="764a3-147">hello return type is used only for hello Reach statistics:</span></span>

* <span data-ttu-id="764a3-148">`Replied`se zvýší, pokud jeden z všesměrového vysílání příjemci hello vrácen buď `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="764a3-148">`Replied` is incremented if one of hello broadcast receivers returned either `true` or `false`.</span></span>
* <span data-ttu-id="764a3-149">`Actioned`se zvýší, pouze v případě, že jeden z hello vysílání příjemci vrátil `true`.</span><span class="sxs-lookup"><span data-stu-id="764a3-149">`Actioned` is incremented only if one of hello broadcast receivers returned `true`.</span></span>

## <a name="how-toocustomize-campaigns"></a><span data-ttu-id="764a3-150">Jak toocustomize kampaně</span><span class="sxs-lookup"><span data-stu-id="764a3-150">How toocustomize campaigns</span></span>
<span data-ttu-id="764a3-151">toocustomize kampaní, můžete upravit rozložení hello součástí hello Reach SDK.</span><span class="sxs-lookup"><span data-stu-id="764a3-151">toocustomize campaigns, you can modify hello layouts provided in hello Reach SDK.</span></span>

<span data-ttu-id="764a3-152">Měli byste zachovat všechny identifikátory hello použité v hello rozložení a hello typy hello zobrazení, která použijte identifikátor, hlavně pro zobrazení textu zobrazení bitové kopie a zachovat.</span><span class="sxs-lookup"><span data-stu-id="764a3-152">You should keep all hello identifiers used in hello layouts and keep hello types of hello views that use an identifier, especially for text views and image views.</span></span> <span data-ttu-id="764a3-153">Některá zobrazení jsou právě používané toohide nebo zobrazit oblastí, jejich typ může být změněn.</span><span class="sxs-lookup"><span data-stu-id="764a3-153">Some views are just used toohide or show areas so their type may be changed.</span></span> <span data-ttu-id="764a3-154">Pokud máte v úmyslu toochange hello typ zobrazení v hello zadat rozložení zkontrolujte hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="764a3-154">Please check hello source code if you intend toochange hello type of a view in hello provided layouts.</span></span>

### <a name="notifications"></a><span data-ttu-id="764a3-155">Oznámení</span><span class="sxs-lookup"><span data-stu-id="764a3-155">Notifications</span></span>
<span data-ttu-id="764a3-156">Existují dva typy oznámení: systém a v aplikaci oznámení, které používají různé rozložení soubory.</span><span class="sxs-lookup"><span data-stu-id="764a3-156">There are two types of notifications: system and in-app notifications which use different layout files.</span></span>

#### <a name="system-notifications"></a><span data-ttu-id="764a3-157">Systémová oznámení</span><span class="sxs-lookup"><span data-stu-id="764a3-157">System notifications</span></span>
<span data-ttu-id="764a3-158">Systémová oznámení toocustomize potřebujete toouse hello **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="764a3-158">toocustomize system notifications you need toouse hello **categories**.</span></span> <span data-ttu-id="764a3-159">Můžete přeskočit příliš[kategorie](#categories).</span><span class="sxs-lookup"><span data-stu-id="764a3-159">You can jump too[Categories](#categories).</span></span>

#### <a name="in-app-notifications"></a><span data-ttu-id="764a3-160">Oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="764a3-160">In-app notifications</span></span>
<span data-ttu-id="764a3-161">Ve výchozím nastavení, oznámení v aplikaci je zobrazení, které je dynamicky přidané toohello aktuální aktivity uživatelské rozhraní Děkujeme toohello Android metoda `addContentView()`.</span><span class="sxs-lookup"><span data-stu-id="764a3-161">By default, an in-app notification is a view that is dynamically added toohello current activity user interface thanks toohello Android method `addContentView()`.</span></span> <span data-ttu-id="764a3-162">Tomu se říká překrytí oznámení.</span><span class="sxs-lookup"><span data-stu-id="764a3-162">This is called a notification overlay.</span></span> <span data-ttu-id="764a3-163">Překryvy oznámení jsou skvělý pro rychlé integraci, protože nevyžadují toomodify jste žádné rozložení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="764a3-163">Notification overlays are great for a fast integration because they do not require you toomodify any layout in your application.</span></span>

<span data-ttu-id="764a3-164">Vzhled hello toomodify vaše překryvy oznámení, jednoduše soubor můžete upravit hello `engagement_notification_area.xml` tooyour potřebuje.</span><span class="sxs-lookup"><span data-stu-id="764a3-164">toomodify hello look of your notification overlays, you can simply modify hello file `engagement_notification_area.xml` tooyour needs.</span></span>

> [!NOTE]
> <span data-ttu-id="764a3-165">soubor Hello `engagement_notification_overlay.xml` je ten, který je použité toocreate hello oznámení překrytí, obsahuje soubor hello `engagement_notification_area.xml`.</span><span class="sxs-lookup"><span data-stu-id="764a3-165">hello file `engagement_notification_overlay.xml` is hello one that is used toocreate a notification overlay, it includes hello file `engagement_notification_area.xml`.</span></span> <span data-ttu-id="764a3-166">Můžete taky přizpůsobit ho toosuit vašim potřebám (například pro umístění hello oznamovací oblasti v rámci překrytí hello).</span><span class="sxs-lookup"><span data-stu-id="764a3-166">You can also customize it toosuit your needs (such as for positioning hello notification area within hello overlay).</span></span>
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a><span data-ttu-id="764a3-167">Zahrnout rozložení oznámení jako součást rozložení aktivity</span><span class="sxs-lookup"><span data-stu-id="764a3-167">Include notification layout as part of an activity layout</span></span>
<span data-ttu-id="764a3-168">Překryvy se výborně hodí pro rychlé integrace, ale můžete bylo nepraktické nebo mít vedlejší účinky ve zvláštních případech.</span><span class="sxs-lookup"><span data-stu-id="764a3-168">Overlays are great for a fast integration but can be inconvenient or have side effects in special cases.</span></span> <span data-ttu-id="764a3-169">Hello překrytí systému lze přizpůsobit na úrovni aktivity, takže je easy tooprevent vedlejší účinky pro speciální aktivity.</span><span class="sxs-lookup"><span data-stu-id="764a3-169">hello overlay system can be customized at an activity level, making it easy tooprevent side effects for special activities.</span></span>

<span data-ttu-id="764a3-170">Můžete rozhodnout tooinclude naše rozložení oznámení ve vaší stávající toohello Děkujeme rozložení Android **zahrnují** příkaz.</span><span class="sxs-lookup"><span data-stu-id="764a3-170">You can decide tooinclude our notification layout in your existing layout thanks toohello Android **include** statement.</span></span> <span data-ttu-id="764a3-171">Hello tady je příklad upravené `ListActivity` rozložení obsahující jenom `ListView`.</span><span class="sxs-lookup"><span data-stu-id="764a3-171">hello following is an example of a modified `ListActivity` layout containing just a `ListView`.</span></span>

<span data-ttu-id="764a3-172">**Před integraci sady Engagement:**</span><span class="sxs-lookup"><span data-stu-id="764a3-172">**Before Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

<span data-ttu-id="764a3-173">**Po integraci sady Engagement:**</span><span class="sxs-lookup"><span data-stu-id="764a3-173">**After Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

<span data-ttu-id="764a3-174">V tomto příkladu jsme přidali nadřazený kontejner, protože původní rozložení hello používá zobrazení seznamu jako nejvyšší úrovně element hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-174">In this example we added a parent container since hello original layout used a list view as hello top level element.</span></span> <span data-ttu-id="764a3-175">Jsme přidali i `android:layout_weight="1"` toobe možné tooadd nakonfigurované níže zobrazení seznamu zobrazení `android:layout_height="fill_parent"`.</span><span class="sxs-lookup"><span data-stu-id="764a3-175">We also added `android:layout_weight="1"` toobe able tooadd a view below a list view configured with `android:layout_height="fill_parent"`.</span></span>

<span data-ttu-id="764a3-176">Hello Engagement Reach SDK automaticky rozpozná, že rozložení oznámení hello je součástí této aktivity a nepřidá překrytí pro tuto aktivitu.</span><span class="sxs-lookup"><span data-stu-id="764a3-176">hello Engagement Reach SDK automatically detects that hello notification layout is included in this activity and will not add an overlay for this activity.</span></span>

> [!TIP]
> <span data-ttu-id="764a3-177">Pokud používáte ve vaší aplikaci ListActivity, viditelné překrytí Reach zabrání reagující tooclicked položky v zobrazení seznamu hello už.</span><span class="sxs-lookup"><span data-stu-id="764a3-177">If you use a ListActivity in your application, a visible Reach overlay will prevent you from reacting tooclicked items in hello list view anymore.</span></span> <span data-ttu-id="764a3-178">Jedná se o známý problém.</span><span class="sxs-lookup"><span data-stu-id="764a3-178">This is a known issue.</span></span> <span data-ttu-id="764a3-179">toowork tento problém vyřešit, doporučujeme vám tooembed hello oznámení rozložení v rozložení vlastní seznam aktivitu jako v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-179">toowork around this problem we suggest you tooembed hello notification layout in your own list activity layout like in hello previous sample.</span></span>
> 
> 

##### <a name="disabling-application-notification-per-activity"></a><span data-ttu-id="764a3-180">Zakázat oznámení aplikace na aktivitu</span><span class="sxs-lookup"><span data-stu-id="764a3-180">Disabling application notification per activity</span></span>
<span data-ttu-id="764a3-181">Pokud nechcete, aby hello překrytí toobe přidány tooyour aktivity, a pokud nezadáte hello oznámení rozložení v vlastní rozložení, můžete zakázat hello překrytí pro tuto aktivitu v hello `AndroidManifest.xml` přidáním `meta-data` části stejně jako v nástroji následující hello Příklad:</span><span class="sxs-lookup"><span data-stu-id="764a3-181">If you don't want hello overlay toobe added tooyour activity, and if you don't include hello notification layout in your own layout, you can disable hello overlay for this activity in hello `AndroidManifest.xml` by adding a `meta-data` section like in hello following example:</span></span>

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <span data-ttu-id="764a3-182"><a name="categories"></a>Kategorie</span><span class="sxs-lookup"><span data-stu-id="764a3-182"><a name="categories"></a> Categories</span></span>
<span data-ttu-id="764a3-183">Při úpravě hello zadat rozložení upravíte vzhled hello všechna oznámení.</span><span class="sxs-lookup"><span data-stu-id="764a3-183">When you modify hello provided layouts, you modify hello look of all your notifications.</span></span> <span data-ttu-id="764a3-184">Kategorie povolit, že jste toodefine, které se různé cílové hledá oznámení (pravděpodobně chování).</span><span class="sxs-lookup"><span data-stu-id="764a3-184">Categories allow you toodefine various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="764a3-185">Kategorie lze při vytváření kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="764a3-185">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="764a3-186">Mějte na paměti, že kategorie vám také umožní přizpůsobit oznámení a hlasování, který je popsán dále v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="764a3-186">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="764a3-187">tooregister kategorie obslužnou rutinu pro oznámení, musíte tooadd volání při inicializaci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-187">tooregister a category handler for your notifications, you need tooadd a call when hello application is initialized.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="764a3-188">Přečtěte si upozornění hello o atribut android: proces hello \<android-sdk-engagement-process\> v hello jak tooIntegrate Engagement na Android tématu než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="764a3-188">Please read hello warning about hello android:process attribute \<android-sdk-engagement-process\> in hello How tooIntegrate Engagement on Android topic before proceeding.</span></span>
> 
> 

<span data-ttu-id="764a3-189">Hello následující příklad předpokládá potvrzeny hello předchozího upozornění a použít podtřídou třídy `EngagementApplication`:</span><span class="sxs-lookup"><span data-stu-id="764a3-189">hello following example assumes you acknowledged hello previous warning and use a sub-class of `EngagementApplication`:</span></span>

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

<span data-ttu-id="764a3-190">Hello `MyNotifier` objekt je implementace hello hello oznámení kategorie obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="764a3-190">hello `MyNotifier` object is hello implementation of hello notification category handler.</span></span> <span data-ttu-id="764a3-191">Jedná se buď implementaci hello `EngagementNotifier` rozhraní nebo dílčí třídu hello výchozí implementace: `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="764a3-191">It is either an implementation of hello `EngagementNotifier` interface or a sub class of hello default implementation: `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="764a3-192">Všimněte si, že hello téhož oznamovatele dokáže zpracovat několika kategorií, zaregistrujte je takto:</span><span class="sxs-lookup"><span data-stu-id="764a3-192">Note that hello same notifier can handle several categories, you can register them like this:</span></span>

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

<span data-ttu-id="764a3-193">tooreplace hello výchozí kategorie implementace implementaci jako můžete zaregistrovat v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="764a3-193">tooreplace hello default category implementation, you can register your implementation like in hello following example:</span></span>

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

<span data-ttu-id="764a3-194">aktuální kategorie Hello použitá v obslužné rutině se předá jako parametr v většinu metod, můžete přepsat v `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="764a3-194">hello current category used in a handler is passed as a parameter in most methods you can override in `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="764a3-195">Je předán buď jako `String` parametr nebo nepřímo v `EngagementReachContent` objekt, který má `getCategory()` metoda.</span><span class="sxs-lookup"><span data-stu-id="764a3-195">It is passed either as a `String` parameter or indirectly in a `EngagementReachContent` object which has a `getCategory()` method.</span></span>

<span data-ttu-id="764a3-196">Můžete změnit většinu proces vytváření oznámení hello opětovná definice metody na `EngagementDefaultNotifier`pro pokročilejší přizpůsobení myslíte, že volné tootake podívejte se v technické dokumentaci hello a hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="764a3-196">You can change most of hello notification creation process by redefining methods on `EngagementDefaultNotifier`, for more advanced customization feel free tootake a look at hello technical documentation and at hello source code.</span></span>

##### <a name="in-app-notifications"></a><span data-ttu-id="764a3-197">Oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="764a3-197">In-app notifications</span></span>
<span data-ttu-id="764a3-198">Pokud chcete toouse alternativní rozložení pro určitou kategorii, můžete to implementovat jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="764a3-198">If you just want toouse alternate layouts for a specific category, you can implement this as in hello following example:</span></span>

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

<span data-ttu-id="764a3-199">**Příklad `my_notification_overlay.xml` :**</span><span class="sxs-lookup"><span data-stu-id="764a3-199">**Example of `my_notification_overlay.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

<span data-ttu-id="764a3-200">Jak vidíte, se liší od standardní hello jeden identifikátor zobrazit překrytí hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-200">As you can see, hello overlay view identifier is different than hello standard one.</span></span> <span data-ttu-id="764a3-201">Je důležité, aby každé rozložení použít jedinečný identifikátor pro překryvy.</span><span class="sxs-lookup"><span data-stu-id="764a3-201">It is important that each layout use a unique identifier for overlays.</span></span>

<span data-ttu-id="764a3-202">**Příklad `my_notification_area.xml` :**</span><span class="sxs-lookup"><span data-stu-id="764a3-202">**Example of `my_notification_area.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

<span data-ttu-id="764a3-203">Jak vidíte, se liší od standardní hello jeden hello oznámení oblasti zobrazení identifikátor.</span><span class="sxs-lookup"><span data-stu-id="764a3-203">As you can see, hello notification area view identifier is different than hello standard one.</span></span> <span data-ttu-id="764a3-204">Je důležité, že každé rozložení používá jedinečný identifikátor pro oznamovací oblasti.</span><span class="sxs-lookup"><span data-stu-id="764a3-204">It is important that each layout uses a unique identifier for notification areas.</span></span>

<span data-ttu-id="764a3-205">Tento jednoduchý příklad kategorie umožňuje aplikace (nebo v aplikaci) oznámení zobrazí v horní části hello obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-205">This simple example of category makes application (or in-app) notifications displayed at hello top of hello screen.</span></span> <span data-ttu-id="764a3-206">Jsme nezměnila hello standardní identifikátory používané v oznamovací oblasti hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="764a3-206">We did not change hello standard identifiers used in hello notification area itself.</span></span>

<span data-ttu-id="764a3-207">Pokud chcete, aby toochange, že máte tooredefine hello `EngagementDefaultNotifier.prepareInAppArea` metoda.</span><span class="sxs-lookup"><span data-stu-id="764a3-207">If you want toochange that, you have tooredefine hello `EngagementDefaultNotifier.prepareInAppArea` method.</span></span> <span data-ttu-id="764a3-208">Je doporučeno toolook v hello technické dokumentace a zdrojový kód hello `EngagementNotifier` a `EngagementDefaultNotifier` Pokud chcete tato úroveň rozšířené úpravy.</span><span class="sxs-lookup"><span data-stu-id="764a3-208">It's recommended toolook at hello technical documentation and at hello source code of `EngagementNotifier` and `EngagementDefaultNotifier` if you want this level of advanced customization.</span></span>

##### <a name="system-notifications"></a><span data-ttu-id="764a3-209">Systémová oznámení</span><span class="sxs-lookup"><span data-stu-id="764a3-209">System notifications</span></span>
<span data-ttu-id="764a3-210">Tím, že rozšíří `EngagementDefaultNotifier`, můžete přepsat `onNotificationPrepared` tooalter hello oznámení, že byl připraven výchozí implementací hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-210">By extending `EngagementDefaultNotifier`, you can override `onNotificationPrepared` tooalter hello notification that was prepared by hello default implementation.</span></span>

<span data-ttu-id="764a3-211">Například:</span><span class="sxs-lookup"><span data-stu-id="764a3-211">For example:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

<span data-ttu-id="764a3-212">Tento příklad vytvoří systémové oznámení pro obsah, se zobrazuje jako probíhající událost v případě, že se používá kategorie "probíhající" hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-212">This example makes a system notification for a content being displayed as an ongoing event when hello "ongoing" category is used.</span></span>

<span data-ttu-id="764a3-213">Pokud chcete, aby toobuild hello `Notification` objektu od začátku, můžete se vrátit `false` toohello metoda a volání `notify` sami na hello `NotificationManager`.</span><span class="sxs-lookup"><span data-stu-id="764a3-213">If you want toobuild hello `Notification` object from scratch, you can return `false` toohello method and call `notify` yourself on hello `NotificationManager`.</span></span> <span data-ttu-id="764a3-214">V takovém případě je důležité, aby byl `contentIntent`, `deleteIntent` a hello používá identifikátor oznámení `EngagementReachReceiver`.</span><span class="sxs-lookup"><span data-stu-id="764a3-214">In that case it's important that you keep a `contentIntent`, a `deleteIntent` and hello notification identifier used by `EngagementReachReceiver`.</span></span>

<span data-ttu-id="764a3-215">Tady je správný příklad takových implementace:</span><span class="sxs-lookup"><span data-stu-id="764a3-215">Here is a correct example of such an implementation:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice hello call tooget hello right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a><span data-ttu-id="764a3-216">Oznámení pouze oznámení</span><span class="sxs-lookup"><span data-stu-id="764a3-216">Notification only announcements</span></span>
<span data-ttu-id="764a3-217">Hello správu hello kliknutím na oznámení pouze oznámení lze přizpůsobit přepsáním `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello připravený `Intent`.</span><span class="sxs-lookup"><span data-stu-id="764a3-217">hello management of hello click on a notification only announcement can be customized by overriding `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello prepared `Intent`.</span></span> <span data-ttu-id="764a3-218">Pomocí této metody můžete tootune hello příznaky snadno.</span><span class="sxs-lookup"><span data-stu-id="764a3-218">Using this method allows you tootune hello flags easily.</span></span>

<span data-ttu-id="764a3-219">Například tooadd hello `SINGLE_TOP` příznak:</span><span class="sxs-lookup"><span data-stu-id="764a3-219">For example tooadd hello `SINGLE_TOP` flag:</span></span>

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

<span data-ttu-id="764a3-220">Pro starší verze zapojení uživatelů Pamatujte, že systémová oznámení bez akce URL teď spustí aplikace hello Pokud byl v pozadí, takže tato metoda může být volána s hlášení bez adresa URL akce.</span><span class="sxs-lookup"><span data-stu-id="764a3-220">For legacy Engagement users, please note that system notifications without action URL now launches hello application if it was in background, so this method can be called with an announcement without action URL.</span></span> <span data-ttu-id="764a3-221">Měli byste zvážit, když přizpůsobení hello záměr.</span><span class="sxs-lookup"><span data-stu-id="764a3-221">You should consider that when customizing hello intent.</span></span>

<span data-ttu-id="764a3-222">Můžete taky implementovat `EngagementNotifier.executeNotifAnnouncementAction` od začátku.</span><span class="sxs-lookup"><span data-stu-id="764a3-222">You can also implement `EngagementNotifier.executeNotifAnnouncementAction` from scratch.</span></span>

##### <a name="notification-life-cycle"></a><span data-ttu-id="764a3-223">Oznámení životního cyklu</span><span class="sxs-lookup"><span data-stu-id="764a3-223">Notification life cycle</span></span>
<span data-ttu-id="764a3-224">Při použití hello výchozí kategorie, se nazývají některé metody životní cyklus na hello `EngagementReachInteractiveContent` objektu tooreport statistiky a aktualizace hello kampaň stavu:</span><span class="sxs-lookup"><span data-stu-id="764a3-224">When using hello default category, some life cycle methods are called on hello `EngagementReachInteractiveContent` object tooreport statistics and update hello campaign state:</span></span>

* <span data-ttu-id="764a3-225">Když hello oznámení se zobrazí v aplikaci nebo umístit do hello stavového řádku, hello `displayNotification` metoda je volána (který sestavy statistik) podle `EngagementReachAgent` Pokud `handleNotification` vrátí `true`.</span><span class="sxs-lookup"><span data-stu-id="764a3-225">When hello notification is displayed in application or put in hello status bar, hello `displayNotification` method is called (which reports statistics) by `EngagementReachAgent` if `handleNotification` returns `true`.</span></span>
* <span data-ttu-id="764a3-226">Pokud se zavře hello oznámení, hello `exitNotification` metoda je volána, statistiky se použije v hlášení a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="764a3-226">If hello notification is dismissed, hello `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="764a3-227">Po kliknutí na oznámení hello `actionNotification` je volána, statistiky se použije v hlášení a spuštění hello přidružené záměr.</span><span class="sxs-lookup"><span data-stu-id="764a3-227">If hello notification is clicked, `actionNotification` is called, statistic is reported and hello associated intent is launched.</span></span>

<span data-ttu-id="764a3-228">Pokud vaši implementaci `EngagementNotifier` přeskočení hello výchozí chování, máte tyto metody životního cyklu pro toocall samotnými.</span><span class="sxs-lookup"><span data-stu-id="764a3-228">If your implementation of `EngagementNotifier` bypasses hello default behavior, you have toocall these life cycle methods by yourself.</span></span> <span data-ttu-id="764a3-229">Hello následující příklady ilustrují některých případech, kde přeskočí hello výchozí chování:</span><span class="sxs-lookup"><span data-stu-id="764a3-229">hello following examples illustrate some cases where hello default behavior is bypassed:</span></span>

* <span data-ttu-id="764a3-230">Nerozšíříte `EngagementDefaultNotifier`, například implementována kategorie zpracování od začátku.</span><span class="sxs-lookup"><span data-stu-id="764a3-230">You don't extend `EngagementDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="764a3-231">Pro systémová oznámení overrode hello `onNotificationPrepared` a můžete změnit `contentIntent` nebo `deleteIntent` v hello `Notification` objektu.</span><span class="sxs-lookup"><span data-stu-id="764a3-231">For system notifications, you overrode hello `onNotificationPrepared` and you modified `contentIntent` or `deleteIntent` in hello `Notification` object.</span></span>
* <span data-ttu-id="764a3-232">Oznámení v aplikaci, overrode `prepareInAppArea`, že toomap alespoň `actionNotification` tooone U.I ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="764a3-232">For in-app notifications, you overrode `prepareInAppArea`, be sure toomap at least `actionNotification` tooone of your U.I controls.</span></span>

> [!NOTE]
> <span data-ttu-id="764a3-233">Pokud `handleNotification` vyvolá výjimku, hello obsahu se odstraní a `dropContent` je volána.</span><span class="sxs-lookup"><span data-stu-id="764a3-233">If `handleNotification` throws an exception, hello content is deleted and `dropContent` is called.</span></span> <span data-ttu-id="764a3-234">To je uvedený v statistiky a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="764a3-234">This is reported in statistics and next campaigns can now be processed.</span></span>
> 
> 

### <a name="announcements-and-polls"></a><span data-ttu-id="764a3-235">Oznámení a hlasování</span><span class="sxs-lookup"><span data-stu-id="764a3-235">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="764a3-236">Rozložení</span><span class="sxs-lookup"><span data-stu-id="764a3-236">Layouts</span></span>
<span data-ttu-id="764a3-237">Můžete upravit hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` a `engagement_poll.xml` soubory toocustomize textu oznámení, oznámení webové a hlasování.</span><span class="sxs-lookup"><span data-stu-id="764a3-237">You can modify hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` and `engagement_poll.xml` files toocustomize text announcements, web announcements and polls.</span></span>

<span data-ttu-id="764a3-238">Tyto soubory sdílet dvě běžné rozložení pro oblast hello nadpisu a hello tlačítko oblasti.</span><span class="sxs-lookup"><span data-stu-id="764a3-238">These files share two common layouts for hello title area and hello button area.</span></span> <span data-ttu-id="764a3-239">Hello rozložení pro nadpis hello `engagement_content_title.xml` a používá hello eponymous drawable soubor pro pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-239">hello layout for hello title is `engagement_content_title.xml` and uses hello eponymous drawable file for hello background.</span></span> <span data-ttu-id="764a3-240">Hello rozložení pro tlačítek akce a ukončení hello je `engagement_button_bar.xml` a používá hello eponymous drawable soubor pro pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-240">hello layout for hello action and exit buttons is `engagement_button_bar.xml` and uses hello eponymous drawable file for hello background.</span></span>

<span data-ttu-id="764a3-241">V hlasování, hello otázku rozložení a jejich možnosti jsou dynamicky zvětšený pomocí několikrát hello `engagement_question.xml` rozložení souboru hello otázky a hello `engagement_choice.xml` souboru hello volby.</span><span class="sxs-lookup"><span data-stu-id="764a3-241">In a poll, hello question layout and their choices are dynamically inflated using several times hello `engagement_question.xml` layout file for hello questions and hello `engagement_choice.xml` file for hello choices.</span></span>

#### <a name="categories"></a><span data-ttu-id="764a3-242">Kategorie</span><span class="sxs-lookup"><span data-stu-id="764a3-242">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="764a3-243">Alternativní rozložení</span><span class="sxs-lookup"><span data-stu-id="764a3-243">Alternate layouts</span></span>
<span data-ttu-id="764a3-244">Jako oznámení může být hello kampaň kategorie používané toohave alternativní rozložení pro oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="764a3-244">Like notifications, hello campaign's category can be used toohave alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="764a3-245">Například toocreate kategorii pro text oznámení, můžete rozšířit `EngagementTextAnnouncementActivity` a odkazovat na hello `AndroidManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="764a3-245">For example, toocreate a category for a text announcement, you can extend `EngagementTextAnnouncementActivity` and reference it hello `AndroidManifest.xml` file:</span></span>

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

<span data-ttu-id="764a3-246">Všimněte si této kategorie hello v hello záměr filtr se používá toomake hello rozdíl oproti hello výchozí oznámení aktivita.</span><span class="sxs-lookup"><span data-stu-id="764a3-246">Note that hello category in hello intent filter is used toomake hello difference with hello default announcement activity.</span></span>

<span data-ttu-id="764a3-247">Hello Reach SDK používá hello záměrné systému tooresolve hello vpravo aktivity pro určitou kategorii a jeho spadne zpět na výchozí kategorie hello Pokud hello řešení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="764a3-247">hello Reach SDK uses hello intent system tooresolve hello right activity for a specific category and it falls back on hello default category if hello resolution failed.</span></span>

<span data-ttu-id="764a3-248">Pak máte tooimplement `MyCustomTextAnnouncementActivity`, pokud jste právě má toochange hello rozložení (ale zachovat stejné zobrazit identifikátory hello), máte právě toodefine hello třídy, jako třeba v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="764a3-248">Then you have tooimplement `MyCustomTextAnnouncementActivity`, if you just want toochange hello layout (but keep hello same view identifiers), you just have toodefine hello class like in hello following example:</span></span>

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class toouse R.layout.my_text_announcement
              }
            }

<span data-ttu-id="764a3-249">kategorie výchozí hello tooreplace textu oznámení, jednoduše nahradit `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` podle vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="764a3-249">tooreplace hello default category of text announcements, simply replace `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` by your implementation.</span></span>

<span data-ttu-id="764a3-250">Podobně lze přizpůsobit web oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="764a3-250">Web announcements and polls can be customized in a similar fashion.</span></span>

<span data-ttu-id="764a3-251">Pro web oznámení můžete rozšířit `EngagementWebAnnouncementActivity` a deklarovat vaše aktivity v hello `AndroidManifest.xml` stejně jako v nástroji hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="764a3-251">For web announcements you can extend `EngagementWebAnnouncementActivity` and declare your activity in hello `AndroidManifest.xml` like in hello following example:</span></span>

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in hello intent is hello data mime type -->
              </intent-filter>
            </activity>

<span data-ttu-id="764a3-252">Pro dotazování můžete rozšířit `EngagementPollActivity` a deklarovat vaší v hello `AndroidManifest.xml` stejně jako v nástroji hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="764a3-252">For polls you can extend `EngagementPollActivity` and declare your in hello `AndroidManifest.xml` like in hello following example:</span></span>

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a><span data-ttu-id="764a3-253">Implementace od začátku</span><span class="sxs-lookup"><span data-stu-id="764a3-253">Implementation from scratch</span></span>
<span data-ttu-id="764a3-254">Kategorie můžete implementovat vaše aktivity oznámení (a dotazování) bez rozšíření mezi hello `Engagement*Activity` třídy poskytované hello Reach SDK.</span><span class="sxs-lookup"><span data-stu-id="764a3-254">You can implement categories for your announcement (and poll) activities without extending one of hello `Engagement*Activity` classes provided by hello Reach SDK.</span></span> <span data-ttu-id="764a3-255">To je užitečné, například pokud budete chtít toodefine rozložení, který nepoužívá hello stejné zobrazení jako standardní rozložení hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-255">This is useful for example if you want toodefine a layout that does not use hello same views as hello standard layouts.</span></span>

<span data-ttu-id="764a3-256">Jako přizpůsobení pokročilé oznámení se doporučuje toolook v hello zdrojový kód standardní implementace hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-256">Like for advanced notification customization, it is recommended toolook at hello source code of hello standard implementation.</span></span>

<span data-ttu-id="764a3-257">Tady jsou některé věci tookeep pamatovat: Reach spustí aktivita hello konkrétní záměrem (odpovídající záměrné filtr toohello) plus další parametr, což je identifikátor obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-257">Here are some things tookeep in mind: Reach will launch hello activity with a specific intent (corresponding toohello intent filter) plus an extra parameter which is hello content identifier.</span></span>

<span data-ttu-id="764a3-258">To můžete provést hello tooretrieve obsahu se objekt, který obsahovat hello pole, které jste zadali při vytváření hello kampaně na hello webu můžete:</span><span class="sxs-lookup"><span data-stu-id="764a3-258">tooretrieve hello content object which contain hello fields you specified when creating hello campaign on hello web site you can do this:</span></span>

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

<span data-ttu-id="764a3-259">Pro statistiku, byste měli hlásit hello obsah se zobrazí v hello `onResume` událostí:</span><span class="sxs-lookup"><span data-stu-id="764a3-259">For statistics, you should report hello content is displayed in hello `onResume` event:</span></span>

            @Override
            protected void onResume()
            {
             /* Mark hello content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

<span data-ttu-id="764a3-260">Potom toocall buď nezapomeňte `actionContent(this)` nebo `exitContent(this)` v obsahu objektu hello před hello aktivity přejde do pozadí.</span><span class="sxs-lookup"><span data-stu-id="764a3-260">Then, don't forget toocall either `actionContent(this)` or `exitContent(this)` on hello content object before hello activity goes into background.</span></span>

<span data-ttu-id="764a3-261">Pokud nemůžete volat buď `actionContent` nebo `exitContent`, statistiky se neodešlou (tj. žádné analýzy kampaň hello) a další je nejdůležitější – hello další kampaně nebudete upozorněni, až po restartování procesu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="764a3-261">If you don't call either `actionContent` or `exitContent`, statistics won't be sent (i.e. no analytics on hello campaign) and more importantly, hello next campaigns will not be notified until hello application process is restarted.</span></span>

<span data-ttu-id="764a3-262">Orientace nebo jiné změny konfigurace můžete provést hello kód složité toodetermine, zda hello aktivity přejde do pozadí nebo Ne, hello díky standardní implementace zda obsah hello hlášení jako opustil Pokud hello uživatel opustí hello aktivity, (buď Stisknutím `HOME` nebo `BACK`), ale ne v případě změny hello orientace.</span><span class="sxs-lookup"><span data-stu-id="764a3-262">Orientation or other configuration changes can make hello code tricky toodetermine whether hello activity goes into background or not, hello standard implementation makes sure hello content is reported as exited if hello user leaves hello activity (either by pressing `HOME` or `BACK`) but not if hello orientation changes.</span></span>

<span data-ttu-id="764a3-263">Tady je hello zajímavé součást hello implementace:</span><span class="sxs-lookup"><span data-stu-id="764a3-263">Here is hello interesting part of hello implementation:</span></span>

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have toocheck anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

<span data-ttu-id="764a3-264">Jak můžete vidět, pokud jste volali metodu `actionContent(this)` pak dokončení aktivity hello `exitContent(this)` lze bezpečně volat bez nutnosti nijak neprojeví.</span><span class="sxs-lookup"><span data-stu-id="764a3-264">As you can see, if you called `actionContent(this)` then finished hello activity, `exitContent(this)` can be safely called without having any effect.</span></span>

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
