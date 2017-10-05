---
title: Integraci sady Azure Mobile Engagement Android SDK
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
ms.openlocfilehash: 26ba47b19f3a503693d60d344ad39b9eba74fe99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-engagement-reach-on-android"></a><span data-ttu-id="90c36-103">Postup při integraci Engagement Reach pro Android</span><span class="sxs-lookup"><span data-stu-id="90c36-103">How to Integrate Engagement Reach on Android</span></span>
> [!IMPORTANT]
> <span data-ttu-id="90c36-104">Postupujte podle integrace postup popsaný v tom, jak integrovat Engagement Android dokumentu před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="90c36-104">You must follow the integration procedure described in the How to Integrate Engagement on Android document before following this guide.</span></span>
> 
> 

## <a name="standard-integration"></a><span data-ttu-id="90c36-105">Standardní integrace</span><span class="sxs-lookup"><span data-stu-id="90c36-105">Standard integration</span></span>

<span data-ttu-id="90c36-106">Zkopírujte soubory prostředků Reach ze sady SDK do projektu:</span><span class="sxs-lookup"><span data-stu-id="90c36-106">Copy Reach resource files from the SDK in your project :</span></span>

* <span data-ttu-id="90c36-107">Kopírování souborů `res/layout` složky doručit pomocí sady SDK do `res/layout` složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="90c36-107">Copy the files from the `res/layout` folder delivered with the SDK into the `res/layout` folder of your application.</span></span>
* <span data-ttu-id="90c36-108">Kopírování souborů `res/drawable` složky doručit pomocí sady SDK do `res/drawable` složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="90c36-108">Copy the files from the `res/drawable` folder delivered with the SDK into the `res/drawable` folder of your application.</span></span>

<span data-ttu-id="90c36-109">Upravit vaše `AndroidManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="90c36-109">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="90c36-110">Přidejte následující část (mezi `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="90c36-110">Add the following section (between the `<application>` and `</application>` tags):</span></span>
  
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
* <span data-ttu-id="90c36-111">Potřebujete tato oprávnění opakování systémová oznámení, které nebyly použity při spuštění (jinak zůstanou na disku, ale už se nezobrazí, je opravdu nutné zahrnout to).</span><span class="sxs-lookup"><span data-stu-id="90c36-111">You need this permission to replay system notifications that were not clicked at boot (otherwise they will be kept on disk but won't be displayed anymore, you really have to include this).</span></span>
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* <span data-ttu-id="90c36-112">Určení ikony pro oznámení (v aplikaci a systému ty, které jsou i) používané kopírování a úpravy v následující části (mezi `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="90c36-112">Specify an icon used for notifications (both in app and system ones) by copying and editing the following section (between the `<application>` and `</application>` tags):</span></span>
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> <span data-ttu-id="90c36-113">Tato část se **povinné** Pokud máte v úmyslu používat systémová oznámení při vytváření kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="90c36-113">This section is **mandatory** if you plan on using system notifications when creating Reach campaigns.</span></span> <span data-ttu-id="90c36-114">Android zabrání systémová oznámení bez ikony se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="90c36-114">Android prevents system notifications without icons from being shown.</span></span> <span data-ttu-id="90c36-115">Proto pokud vynecháte v této části, koncoví uživatelé nebudou moci přijímat.</span><span class="sxs-lookup"><span data-stu-id="90c36-115">So if you omit this section, your end users will not be able to receive them.</span></span>
> 
> 

* <span data-ttu-id="90c36-116">Pokud vytvoříte kampaně s systémová oznámení pomocí velký obrázek, je nutné přidat následující oprávnění (po `</application>` značka) Pokud chybí:</span><span class="sxs-lookup"><span data-stu-id="90c36-116">If you create campaigns with system notifications using big picture, you need to add the following permissions (after the `</application>` tag) if missing:</span></span>
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * <span data-ttu-id="90c36-117">V systému Android M a pokud aplikace cílí úroveň rozhraní API systému Android 23 nebo vyšší ``WRITE_EXTERNAL_STORAGE`` oprávnění vyžaduje schválení uživatelem.</span><span class="sxs-lookup"><span data-stu-id="90c36-117">On Android M and if your application targets Android API level 23 or greater, ``WRITE_EXTERNAL_STORAGE`` permission requires user approval.</span></span> <span data-ttu-id="90c36-118">Přečtěte si prosím [v této části](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="90c36-118">Please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>
* <span data-ttu-id="90c36-119">Pro systémová oznámení je můžete také zadat v kampaně Reach Pokud by se prstence nebo zavibrovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="90c36-119">For system notifications you can also specify in the Reach campaign if the device should ring and/or vibrate.</span></span> <span data-ttu-id="90c36-120">U ho na spolupráci, budete muset zajistěte, aby deklarovaný následující oprávnění (po `</application>` značka):</span><span class="sxs-lookup"><span data-stu-id="90c36-120">For it to work, you have to make sure you declared the following permission (after the `</application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  <span data-ttu-id="90c36-121">Bez tohoto oprávnění zabrání Android systémových oznámení se zobrazí, pokud zkontroluje okruhu nebo možnost vibrate ve Správci kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="90c36-121">Without this permission, Android prevents system notifications from being shown if you checked the ring or the vibrate option in the Reach Campaign manager.</span></span>

## <a name="native-push"></a><span data-ttu-id="90c36-122">Nativního nabízení</span><span class="sxs-lookup"><span data-stu-id="90c36-122">Native Push</span></span>
<span data-ttu-id="90c36-123">Teď, když jste nakonfigurovali modul Reach, budete muset nakonfigurovat nativního nabízení být schopný přijímat kampaně v zařízení.</span><span class="sxs-lookup"><span data-stu-id="90c36-123">Now that you configured Reach module, you need to configure native push to be able to receive the campaigns on the device.</span></span>

<span data-ttu-id="90c36-124">V systému Android podporujeme dvě služby:</span><span class="sxs-lookup"><span data-stu-id="90c36-124">We support two services on Android:</span></span>

* <span data-ttu-id="90c36-125">Google Play zařízení: použití [Google Cloud Messaging] podle [jak integrovat GCM s Engagement průvodce](mobile-engagement-android-gcm-integrate.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="90c36-125">Google Play devices: Use [Google Cloud Messaging] by following the [How to Integrate GCM with Engagement guide](mobile-engagement-android-gcm-integrate.md) guide.</span></span>
* <span data-ttu-id="90c36-126">Zařízení Amazon: použití [Amazon Device Messaging] podle [jak integrovat ADM s Engagement průvodce](mobile-engagement-android-adm-integrate.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="90c36-126">Amazon devices: Use [Amazon Device Messaging] by following the [How to Integrate ADM with Engagement guide](mobile-engagement-android-adm-integrate.md) guide.</span></span>

<span data-ttu-id="90c36-127">Pokud chcete zacílit Amazon a webu Google Play zařízení, jeho možné mít všechno uvnitř 1 AndroidManifest.xml/APK pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="90c36-127">If you want to target both Amazon and Google Play devices, its possible to have everything inside 1 AndroidManifest.xml/APK for development.</span></span> <span data-ttu-id="90c36-128">Ale při odesílání do Amazon, pokud se najdou GCM kódu může zamítnutí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="90c36-128">But when submitting to Amazon, they may reject your application if they find GCM code.</span></span>

<span data-ttu-id="90c36-129">V takovém případě byste měli používat více APKs.</span><span class="sxs-lookup"><span data-stu-id="90c36-129">You should use multiple APKs in that case.</span></span>

<span data-ttu-id="90c36-130">**Aplikace je nyní připraven přijmout a zobrazit kampaně reach!**</span><span class="sxs-lookup"><span data-stu-id="90c36-130">**Your application is now ready to receive and display reach campaigns!**</span></span>

## <a name="how-to-handle-data-push"></a><span data-ttu-id="90c36-131">Postupy: zpracování datová oznámení</span><span class="sxs-lookup"><span data-stu-id="90c36-131">How to handle data push</span></span>
### <a name="integration"></a><span data-ttu-id="90c36-132">Integrace</span><span class="sxs-lookup"><span data-stu-id="90c36-132">Integration</span></span>
<span data-ttu-id="90c36-133">Pokud chcete aplikace nebudou moct přijímat Reach datová oznámení, je nutné vytvořit podtřídou třídy `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` a odkazovat na jeho `AndroidManifest.xml` souboru (mezi `<application>` nebo `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="90c36-133">If you want your application to be able to receive Reach data pushes, you have to create a sub-class of `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` and reference it in the `AndroidManifest.xml` file (between the `<application>` and/or `</application>` tags):</span></span>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

<span data-ttu-id="90c36-134">Pak můžete přepsat `onDataPushStringReceived` a `onDataPushBase64Received` zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="90c36-134">Then you can override the `onDataPushStringReceived` and `onDataPushBase64Received` callbacks.</span></span> <span data-ttu-id="90c36-135">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="90c36-135">Here is an example:</span></span>

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

### <a name="category"></a><span data-ttu-id="90c36-136">Kategorie</span><span class="sxs-lookup"><span data-stu-id="90c36-136">Category</span></span>
<span data-ttu-id="90c36-137">Kategorie parametr je volitelný při vytváření kampaň nabízených Data a umožňuje že vám data filtru nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-137">The category parameter is optional when you create a Data Push campaign and allows you to filter data pushes.</span></span> <span data-ttu-id="90c36-138">To je užitečné, pokud máte několik všesměrového vysílání příjemci zpracování různé typy dat nabízených oznámení, nebo pokud chcete push různých druhů z `Base64` dat a chcete určit jejich typů před analýzou je.</span><span class="sxs-lookup"><span data-stu-id="90c36-138">This is useful if you have several broadcast receivers handling different types of data pushes, or if you want to push different kinds of `Base64` data and want to identify their type before parsing them.</span></span>

### <a name="callbacks-return-parameter"></a><span data-ttu-id="90c36-139">Návratový parametr zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="90c36-139">Callbacks' return parameter</span></span>
<span data-ttu-id="90c36-140">Zde je několik doporučení správně zpracovat návratový parametr `onDataPushStringReceived` a `onDataPushBase64Received`:</span><span class="sxs-lookup"><span data-stu-id="90c36-140">Here are some guidelines to properly handle the return parameter of `onDataPushStringReceived` and `onDataPushBase64Received`:</span></span>

* <span data-ttu-id="90c36-141">Všesměrového vysílání příjemce by měla vrátit `null` v zpětného volání, pokud ho nebude vědět, jak pro zpracování dat push.</span><span class="sxs-lookup"><span data-stu-id="90c36-141">A broadcast receiver should return `null` in the callback if it does not know how to handle a data push.</span></span> <span data-ttu-id="90c36-142">Kategorie používejte k určení, zda má váš všesměrového vysílání přijímač zpracovávat datová oznámení nebo ne.</span><span class="sxs-lookup"><span data-stu-id="90c36-142">You should use the category to determine whether your broadcast receiver should handle the data push or not.</span></span>
* <span data-ttu-id="90c36-143">Jeden z všesměrového vysílání příjemce by měla vrátit `true` v zpětného volání, pokud ji přijímá datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-143">One of the broadcast receiver should return `true` in the callback if it accepts the data push.</span></span>
* <span data-ttu-id="90c36-144">Jeden z všesměrového vysílání příjemce by měla vrátit `false` v zpětného volání, pokud rozpozná datová oznámení, ale zahodí z jakéhokoliv důvodu.</span><span class="sxs-lookup"><span data-stu-id="90c36-144">One of the broadcast receiver should return `false` in the callback if it recognizes the data push, but discards it for whatever reason.</span></span> <span data-ttu-id="90c36-145">Například vrátit `false` při přijatá data jsou neplatná.</span><span class="sxs-lookup"><span data-stu-id="90c36-145">For example, return `false` when the received data is invalid.</span></span>
* <span data-ttu-id="90c36-146">Pokud jeden vysílání příjemce vrátí `true` při jiné jeden vrátí `false` pro stejné datová oznámení chování není definován, měli byste nikdy udělat.</span><span class="sxs-lookup"><span data-stu-id="90c36-146">If one broadcast receiver returns `true` while another one returns `false` for the same data push, the behavior is undefined, you should never do that.</span></span>

<span data-ttu-id="90c36-147">Návratový typ se používá pouze pro Reach statistiky:</span><span class="sxs-lookup"><span data-stu-id="90c36-147">The return type is used only for the Reach statistics:</span></span>

* <span data-ttu-id="90c36-148">`Replied`se zvýší, pokud jeden z všesměrového vysílání příjemci vrácen buď `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="90c36-148">`Replied` is incremented if one of the broadcast receivers returned either `true` or `false`.</span></span>
* <span data-ttu-id="90c36-149">`Actioned`se zvýší, pouze v případě, že jeden všesměrového vysílání příjemci vrátil `true`.</span><span class="sxs-lookup"><span data-stu-id="90c36-149">`Actioned` is incremented only if one of the broadcast receivers returned `true`.</span></span>

## <a name="how-to-customize-campaigns"></a><span data-ttu-id="90c36-150">Postup přizpůsobení kampaně</span><span class="sxs-lookup"><span data-stu-id="90c36-150">How to customize campaigns</span></span>
<span data-ttu-id="90c36-151">Chcete-li přizpůsobit kampaní, můžete upravit rozložení poskytovaných v sadě SDK dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="90c36-151">To customize campaigns, you can modify the layouts provided in the Reach SDK.</span></span>

<span data-ttu-id="90c36-152">Měli byste zachovat všechny identifikátory použité v rozložení a typy zobrazení, která použijte identifikátor, hlavně pro zobrazení textu zobrazení bitové kopie a zachovat.</span><span class="sxs-lookup"><span data-stu-id="90c36-152">You should keep all the identifiers used in the layouts and keep the types of the views that use an identifier, especially for text views and image views.</span></span> <span data-ttu-id="90c36-153">Některá zobrazení se právě používají pro skrytí nebo zobrazení oblasti, tak jejich typ může být změněn.</span><span class="sxs-lookup"><span data-stu-id="90c36-153">Some views are just used to hide or show areas so their type may be changed.</span></span> <span data-ttu-id="90c36-154">Pokud chcete změnit typ zobrazení v zadané rozložení Zkontrolujte zdrojový kód.</span><span class="sxs-lookup"><span data-stu-id="90c36-154">Please check the source code if you intend to change the type of a view in the provided layouts.</span></span>

### <a name="notifications"></a><span data-ttu-id="90c36-155">Oznámení</span><span class="sxs-lookup"><span data-stu-id="90c36-155">Notifications</span></span>
<span data-ttu-id="90c36-156">Existují dva typy oznámení: systém a v aplikaci oznámení, které používají různé rozložení soubory.</span><span class="sxs-lookup"><span data-stu-id="90c36-156">There are two types of notifications: system and in-app notifications which use different layout files.</span></span>

#### <a name="system-notifications"></a><span data-ttu-id="90c36-157">Systémová oznámení</span><span class="sxs-lookup"><span data-stu-id="90c36-157">System notifications</span></span>
<span data-ttu-id="90c36-158">Chcete-li přizpůsobit systémová oznámení, budete muset použít **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="90c36-158">To customize system notifications you need to use the **categories**.</span></span> <span data-ttu-id="90c36-159">Můžete přejít na [kategorie](#categories).</span><span class="sxs-lookup"><span data-stu-id="90c36-159">You can jump to [Categories](#categories).</span></span>

#### <a name="in-app-notifications"></a><span data-ttu-id="90c36-160">Oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="90c36-160">In-app notifications</span></span>
<span data-ttu-id="90c36-161">Ve výchozím nastavení, oznámení v aplikaci je zobrazení, která se dynamicky přidá do uživatelského rozhraní aktuální aktivity díky Android metoda `addContentView()`.</span><span class="sxs-lookup"><span data-stu-id="90c36-161">By default, an in-app notification is a view that is dynamically added to the current activity user interface thanks to the Android method `addContentView()`.</span></span> <span data-ttu-id="90c36-162">Tomu se říká překrytí oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-162">This is called a notification overlay.</span></span> <span data-ttu-id="90c36-163">Překryvy oznámení jsou skvělý pro rychlé integraci, protože nevyžadují, abyste upravili žádné rozložení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90c36-163">Notification overlays are great for a fast integration because they do not require you to modify any layout in your application.</span></span>

<span data-ttu-id="90c36-164">Pokud chcete upravit vzhled vaší překryvy oznámení, můžete jednoduše upravit soubor `engagement_notification_area.xml` vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="90c36-164">To modify the look of your notification overlays, you can simply modify the file `engagement_notification_area.xml` to your needs.</span></span>

> [!NOTE]
> <span data-ttu-id="90c36-165">Soubor `engagement_notification_overlay.xml` je ten, který se používá k vytvoření oznámení překrytí, obsahuje soubor `engagement_notification_area.xml`.</span><span class="sxs-lookup"><span data-stu-id="90c36-165">The file `engagement_notification_overlay.xml` is the one that is used to create a notification overlay, it includes the file `engagement_notification_area.xml`.</span></span> <span data-ttu-id="90c36-166">Můžete také přizpůsobit ho tak, aby vyhovovala vašim potřebám (například pro umístění oznamovací oblasti v rámci překrytí).</span><span class="sxs-lookup"><span data-stu-id="90c36-166">You can also customize it to suit your needs (such as for positioning the notification area within the overlay).</span></span>
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a><span data-ttu-id="90c36-167">Zahrnout rozložení oznámení jako součást rozložení aktivity</span><span class="sxs-lookup"><span data-stu-id="90c36-167">Include notification layout as part of an activity layout</span></span>
<span data-ttu-id="90c36-168">Překryvy se výborně hodí pro rychlé integrace, ale můžete bylo nepraktické nebo mít vedlejší účinky ve zvláštních případech.</span><span class="sxs-lookup"><span data-stu-id="90c36-168">Overlays are great for a fast integration but can be inconvenient or have side effects in special cases.</span></span> <span data-ttu-id="90c36-169">Systém překrytí lze přizpůsobit na úrovni aktivity, a usnadňuje tak zabránit vedlejší účinky pro speciální aktivity.</span><span class="sxs-lookup"><span data-stu-id="90c36-169">The overlay system can be customized at an activity level, making it easy to prevent side effects for special activities.</span></span>

<span data-ttu-id="90c36-170">Můžete se rozhodnout zahrnout naše rozložení oznámení do vaší stávající rozložení díky Android **zahrnují** příkaz.</span><span class="sxs-lookup"><span data-stu-id="90c36-170">You can decide to include our notification layout in your existing layout thanks to the Android **include** statement.</span></span> <span data-ttu-id="90c36-171">Tady je příklad upravené `ListActivity` rozložení obsahující jenom `ListView`.</span><span class="sxs-lookup"><span data-stu-id="90c36-171">The following is an example of a modified `ListActivity` layout containing just a `ListView`.</span></span>

<span data-ttu-id="90c36-172">**Před integraci sady Engagement:**</span><span class="sxs-lookup"><span data-stu-id="90c36-172">**Before Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

<span data-ttu-id="90c36-173">**Po integraci sady Engagement:**</span><span class="sxs-lookup"><span data-stu-id="90c36-173">**After Engagement integration :**</span></span>

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

<span data-ttu-id="90c36-174">V tomto příkladu jsme přidali nadřazený kontejner, protože původní rozložení použity zobrazení seznamu jako element nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="90c36-174">In this example we added a parent container since the original layout used a list view as the top level element.</span></span> <span data-ttu-id="90c36-175">Jsme přidali i `android:layout_weight="1"` moct přidat zobrazení níže zobrazení seznamu nakonfigurované `android:layout_height="fill_parent"`.</span><span class="sxs-lookup"><span data-stu-id="90c36-175">We also added `android:layout_weight="1"` to be able to add a view below a list view configured with `android:layout_height="fill_parent"`.</span></span>

<span data-ttu-id="90c36-176">Engagement Reach SDK automaticky zjistí, že rozložení oznámení je zahrnutá v této aktivitě a nepřidá překrytí pro tuto aktivitu.</span><span class="sxs-lookup"><span data-stu-id="90c36-176">The Engagement Reach SDK automatically detects that the notification layout is included in this activity and will not add an overlay for this activity.</span></span>

> [!TIP]
> <span data-ttu-id="90c36-177">Pokud používáte ListActivity ve vaší aplikaci, bude reagovat na kliknutí na položky v zobrazení seznamu už zabránit viditelné překrytí Reach.</span><span class="sxs-lookup"><span data-stu-id="90c36-177">If you use a ListActivity in your application, a visible Reach overlay will prevent you from reacting to clicked items in the list view anymore.</span></span> <span data-ttu-id="90c36-178">Jedná se o známý problém.</span><span class="sxs-lookup"><span data-stu-id="90c36-178">This is a known issue.</span></span> <span data-ttu-id="90c36-179">Chcete-li vyřešit tento problém, doporučujeme vám vložení rozložení oznámení v rozložení vlastní seznam aktivitu jako v předchozí ukázce aplikací.</span><span class="sxs-lookup"><span data-stu-id="90c36-179">To work around this problem we suggest you to embed the notification layout in your own list activity layout like in the previous sample.</span></span>
> 
> 

##### <a name="disabling-application-notification-per-activity"></a><span data-ttu-id="90c36-180">Zakázat oznámení aplikace na aktivitu</span><span class="sxs-lookup"><span data-stu-id="90c36-180">Disabling application notification per activity</span></span>
<span data-ttu-id="90c36-181">Pokud nechcete, aby překrytí přidávaného do vaší aktivity, a pokud nezadáte rozložení oznámení ve vaší vlastní rozložení, můžete zakázat v překrytí pro tuto aktivitu v `AndroidManifest.xml` přidáním `meta-data` oddílu, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="90c36-181">If you don't want the overlay to be added to your activity, and if you don't include the notification layout in your own layout, you can disable the overlay for this activity in the `AndroidManifest.xml` by adding a `meta-data` section like in the following example:</span></span>

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <span data-ttu-id="90c36-182"><a name="categories"></a>Kategorie</span><span class="sxs-lookup"><span data-stu-id="90c36-182"><a name="categories"></a> Categories</span></span>
<span data-ttu-id="90c36-183">Při úpravě zadané rozložení upravíte vzhledu všechna oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-183">When you modify the provided layouts, you modify the look of all your notifications.</span></span> <span data-ttu-id="90c36-184">Kategorie umožňují definovat různé cílové vypadá (pravděpodobně chování) pro oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-184">Categories allow you to define various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="90c36-185">Kategorie lze při vytváření kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="90c36-185">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="90c36-186">Mějte na paměti, že kategorie vám také umožní přizpůsobit oznámení a hlasování, který je popsán dále v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="90c36-186">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="90c36-187">Chcete-li zaregistrovat kategorie obslužnou rutinu pro oznámení, přidejte volání při inicializaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="90c36-187">To register a category handler for your notifications, you need to add a call when the application is initialized.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90c36-188">Přečtěte si upozornění o atribut android: proces \<android-sdk-engagement-process\> v tom, jak integrovat Engagement na Android tématu než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90c36-188">Please read the warning about the android:process attribute \<android-sdk-engagement-process\> in the How to Integrate Engagement on Android topic before proceeding.</span></span>
> 
> 

<span data-ttu-id="90c36-189">Následující příklad předpokládá můžete potvrdí předchozí upozornění a použít podtřídou třídy `EngagementApplication`:</span><span class="sxs-lookup"><span data-stu-id="90c36-189">The following example assumes you acknowledged the previous warning and use a sub-class of `EngagementApplication`:</span></span>

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

<span data-ttu-id="90c36-190">`MyNotifier` Objektu je implementace obslužné rutiny kategorie oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-190">The `MyNotifier` object is the implementation of the notification category handler.</span></span> <span data-ttu-id="90c36-191">Jedná se buď implementaci `EngagementNotifier` rozhraní nebo dílčí třídu výchozí implementace: `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="90c36-191">It is either an implementation of the `EngagementNotifier` interface or a sub class of the default implementation: `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="90c36-192">Všimněte si, že téhož oznamovatele dokáže zpracovat několika kategorií je můžete zaregistrovat takto:</span><span class="sxs-lookup"><span data-stu-id="90c36-192">Note that the same notifier can handle several categories, you can register them like this:</span></span>

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

<span data-ttu-id="90c36-193">Pokud chcete nahradit výchozí implementace kategorie, můžete zaregistrovat implementaci jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="90c36-193">To replace the default category implementation, you can register your implementation like in the following example:</span></span>

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

<span data-ttu-id="90c36-194">Aktuální kategorie použitá v obslužné rutině se předá jako parametr v většinu metod, můžete přepsat v `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="90c36-194">The current category used in a handler is passed as a parameter in most methods you can override in `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="90c36-195">Je předán buď jako `String` parametr nebo nepřímo v `EngagementReachContent` objekt, který má `getCategory()` metoda.</span><span class="sxs-lookup"><span data-stu-id="90c36-195">It is passed either as a `String` parameter or indirectly in a `EngagementReachContent` object which has a `getCategory()` method.</span></span>

<span data-ttu-id="90c36-196">Můžete změnit většinu oznámení procesu vytváření opětovná definice metody na `EngagementDefaultNotifier`, pro pokročilejší přizpůsobení klidně si prohlédněte v technické dokumentaci a zdrojový kód.</span><span class="sxs-lookup"><span data-stu-id="90c36-196">You can change most of the notification creation process by redefining methods on `EngagementDefaultNotifier`, for more advanced customization feel free to take a look at the technical documentation and at the source code.</span></span>

##### <a name="in-app-notifications"></a><span data-ttu-id="90c36-197">Oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="90c36-197">In-app notifications</span></span>
<span data-ttu-id="90c36-198">Pokud chcete použít alternativní rozložení pro určitou kategorii, můžete to implementovat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="90c36-198">If you just want to use alternate layouts for a specific category, you can implement this as in the following example:</span></span>

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

<span data-ttu-id="90c36-199">**Příklad `my_notification_overlay.xml` :**</span><span class="sxs-lookup"><span data-stu-id="90c36-199">**Example of `my_notification_overlay.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

<span data-ttu-id="90c36-200">Jak vidíte, se liší od standardní jeden identifikátor zobrazit překrytí.</span><span class="sxs-lookup"><span data-stu-id="90c36-200">As you can see, the overlay view identifier is different than the standard one.</span></span> <span data-ttu-id="90c36-201">Je důležité, aby každé rozložení použít jedinečný identifikátor pro překryvy.</span><span class="sxs-lookup"><span data-stu-id="90c36-201">It is important that each layout use a unique identifier for overlays.</span></span>

<span data-ttu-id="90c36-202">**Příklad `my_notification_area.xml` :**</span><span class="sxs-lookup"><span data-stu-id="90c36-202">**Example of `my_notification_area.xml` :**</span></span>

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

<span data-ttu-id="90c36-203">Jak vidíte, se liší od standardní jeden identifikátoru oznámení oblasti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90c36-203">As you can see, the notification area view identifier is different than the standard one.</span></span> <span data-ttu-id="90c36-204">Je důležité, že každé rozložení používá jedinečný identifikátor pro oznamovací oblasti.</span><span class="sxs-lookup"><span data-stu-id="90c36-204">It is important that each layout uses a unique identifier for notification areas.</span></span>

<span data-ttu-id="90c36-205">Tento jednoduchý příklad kategorie umožňuje aplikace (nebo v aplikaci) oznámení zobrazí v horní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="90c36-205">This simple example of category makes application (or in-app) notifications displayed at the top of the screen.</span></span> <span data-ttu-id="90c36-206">Standardní identifikátory používané v oznamovací oblasti samotné jsme nezměnila.</span><span class="sxs-lookup"><span data-stu-id="90c36-206">We did not change the standard identifiers used in the notification area itself.</span></span>

<span data-ttu-id="90c36-207">Pokud chcete změnit, který, budete muset znovu definovat `EngagementDefaultNotifier.prepareInAppArea` metoda.</span><span class="sxs-lookup"><span data-stu-id="90c36-207">If you want to change that, you have to redefine the `EngagementDefaultNotifier.prepareInAppArea` method.</span></span> <span data-ttu-id="90c36-208">Podívejte se na technickou dokumentaci a na zdrojový kód se doporučuje `EngagementNotifier` a `EngagementDefaultNotifier` Pokud chcete tato úroveň rozšířené úpravy.</span><span class="sxs-lookup"><span data-stu-id="90c36-208">It's recommended to look at the technical documentation and at the source code of `EngagementNotifier` and `EngagementDefaultNotifier` if you want this level of advanced customization.</span></span>

##### <a name="system-notifications"></a><span data-ttu-id="90c36-209">Systémová oznámení</span><span class="sxs-lookup"><span data-stu-id="90c36-209">System notifications</span></span>
<span data-ttu-id="90c36-210">Tím, že rozšíří `EngagementDefaultNotifier`, můžete přepsat `onNotificationPrepared` ke změně oznámení, že byl připraven výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="90c36-210">By extending `EngagementDefaultNotifier`, you can override `onNotificationPrepared` to alter the notification that was prepared by the default implementation.</span></span>

<span data-ttu-id="90c36-211">Například:</span><span class="sxs-lookup"><span data-stu-id="90c36-211">For example:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

<span data-ttu-id="90c36-212">Tento příklad vytvoří systémové oznámení pro obsah, se zobrazuje jako probíhající událost v případě, že se používá "probíhající" kategorie.</span><span class="sxs-lookup"><span data-stu-id="90c36-212">This example makes a system notification for a content being displayed as an ongoing event when the "ongoing" category is used.</span></span>

<span data-ttu-id="90c36-213">Pokud chcete vytvořit `Notification` objektu od začátku, můžete se vrátit `false` metoda a volání `notify` sami na `NotificationManager`.</span><span class="sxs-lookup"><span data-stu-id="90c36-213">If you want to build the `Notification` object from scratch, you can return `false` to the method and call `notify` yourself on the `NotificationManager`.</span></span> <span data-ttu-id="90c36-214">V takovém případě je důležité, aby byl `contentIntent`, `deleteIntent` a identifikátor oznámení používá `EngagementReachReceiver`.</span><span class="sxs-lookup"><span data-stu-id="90c36-214">In that case it's important that you keep a `contentIntent`, a `deleteIntent` and the notification identifier used by `EngagementReachReceiver`.</span></span>

<span data-ttu-id="90c36-215">Tady je správný příklad takových implementace:</span><span class="sxs-lookup"><span data-stu-id="90c36-215">Here is a correct example of such an implementation:</span></span>

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
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a><span data-ttu-id="90c36-216">Oznámení pouze oznámení</span><span class="sxs-lookup"><span data-stu-id="90c36-216">Notification only announcements</span></span>
<span data-ttu-id="90c36-217">Správu kliknutím na oznámení pouze oznámení lze přizpůsobit přepsáním `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` k úpravě připraveného `Intent`.</span><span class="sxs-lookup"><span data-stu-id="90c36-217">The management of the click on a notification only announcement can be customized by overriding `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` to modify the prepared `Intent`.</span></span> <span data-ttu-id="90c36-218">Pomocí této metody můžete snadno ladit příznaků.</span><span class="sxs-lookup"><span data-stu-id="90c36-218">Using this method allows you to tune the flags easily.</span></span>

<span data-ttu-id="90c36-219">Chcete-li například přidat `SINGLE_TOP` příznak:</span><span class="sxs-lookup"><span data-stu-id="90c36-219">For example to add the `SINGLE_TOP` flag:</span></span>

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

<span data-ttu-id="90c36-220">Pro starší verze zapojení uživatelů Pamatujte, že systémová oznámení bez akce URL teď spustí aplikaci pokud byl v pozadí, takže tato metoda může být volána s hlášení bez adresa URL akce.</span><span class="sxs-lookup"><span data-stu-id="90c36-220">For legacy Engagement users, please note that system notifications without action URL now launches the application if it was in background, so this method can be called with an announcement without action URL.</span></span> <span data-ttu-id="90c36-221">Měli byste zvážit, když přizpůsobení záměr.</span><span class="sxs-lookup"><span data-stu-id="90c36-221">You should consider that when customizing the intent.</span></span>

<span data-ttu-id="90c36-222">Můžete taky implementovat `EngagementNotifier.executeNotifAnnouncementAction` od začátku.</span><span class="sxs-lookup"><span data-stu-id="90c36-222">You can also implement `EngagementNotifier.executeNotifAnnouncementAction` from scratch.</span></span>

##### <a name="notification-life-cycle"></a><span data-ttu-id="90c36-223">Oznámení životního cyklu</span><span class="sxs-lookup"><span data-stu-id="90c36-223">Notification life cycle</span></span>
<span data-ttu-id="90c36-224">Při použití výchozí kategorie, se nazývají některé metody životní cyklus na `EngagementReachInteractiveContent` do sestavy statistik objektu a aktualizovat stav kampaně:</span><span class="sxs-lookup"><span data-stu-id="90c36-224">When using the default category, some life cycle methods are called on the `EngagementReachInteractiveContent` object to report statistics and update the campaign state:</span></span>

* <span data-ttu-id="90c36-225">Když se zobrazí v aplikaci nebo put ve stavovém řádku oznámení `displayNotification` metoda je volána (který sestavy statistik) podle `EngagementReachAgent` Pokud `handleNotification` vrátí `true`.</span><span class="sxs-lookup"><span data-stu-id="90c36-225">When the notification is displayed in application or put in the status bar, the `displayNotification` method is called (which reports statistics) by `EngagementReachAgent` if `handleNotification` returns `true`.</span></span>
* <span data-ttu-id="90c36-226">Pokud se zavře oznámení, `exitNotification` metoda je volána, statistiky se použije v hlášení a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="90c36-226">If the notification is dismissed, the `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="90c36-227">Po kliknutí na oznámení `actionNotification` je volána, statistiky se použije v hlášení a spuštění přidružené záměr.</span><span class="sxs-lookup"><span data-stu-id="90c36-227">If the notification is clicked, `actionNotification` is called, statistic is reported and the associated intent is launched.</span></span>

<span data-ttu-id="90c36-228">Pokud vaši implementaci `EngagementNotifier` obchází výchozí chování, budete muset pro volání těchto metod životní cyklus sami.</span><span class="sxs-lookup"><span data-stu-id="90c36-228">If your implementation of `EngagementNotifier` bypasses the default behavior, you have to call these life cycle methods by yourself.</span></span> <span data-ttu-id="90c36-229">Následující příklady ilustrují některých případech, kde přeskočí výchozí chování:</span><span class="sxs-lookup"><span data-stu-id="90c36-229">The following examples illustrate some cases where the default behavior is bypassed:</span></span>

* <span data-ttu-id="90c36-230">Nerozšíříte `EngagementDefaultNotifier`, například implementována kategorie zpracování od začátku.</span><span class="sxs-lookup"><span data-stu-id="90c36-230">You don't extend `EngagementDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="90c36-231">Pro systémová oznámení overrode `onNotificationPrepared` a můžete změnit `contentIntent` nebo `deleteIntent` v `Notification` objektu.</span><span class="sxs-lookup"><span data-stu-id="90c36-231">For system notifications, you overrode the `onNotificationPrepared` and you modified `contentIntent` or `deleteIntent` in the `Notification` object.</span></span>
* <span data-ttu-id="90c36-232">Oznámení v aplikaci, overrode `prepareInAppArea`, je nutné namapovat minimálně `actionNotification` na jednu z vaší U.I ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="90c36-232">For in-app notifications, you overrode `prepareInAppArea`, be sure to map at least `actionNotification` to one of your U.I controls.</span></span>

> [!NOTE]
> <span data-ttu-id="90c36-233">Pokud `handleNotification` vyvolá výjimku, obsah se odstraní a `dropContent` je volána.</span><span class="sxs-lookup"><span data-stu-id="90c36-233">If `handleNotification` throws an exception, the content is deleted and `dropContent` is called.</span></span> <span data-ttu-id="90c36-234">To je uvedený v statistiky a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="90c36-234">This is reported in statistics and next campaigns can now be processed.</span></span>
> 
> 

### <a name="announcements-and-polls"></a><span data-ttu-id="90c36-235">Oznámení a hlasování</span><span class="sxs-lookup"><span data-stu-id="90c36-235">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="90c36-236">Rozložení</span><span class="sxs-lookup"><span data-stu-id="90c36-236">Layouts</span></span>
<span data-ttu-id="90c36-237">Můžete upravit `engagement_text_announcement.xml`, `engagement_web_announcement.xml` a `engagement_poll.xml` soubory pro přizpůsobení textu oznámení, oznámení webové a hlasování.</span><span class="sxs-lookup"><span data-stu-id="90c36-237">You can modify the `engagement_text_announcement.xml`, `engagement_web_announcement.xml` and `engagement_poll.xml` files to customize text announcements, web announcements and polls.</span></span>

<span data-ttu-id="90c36-238">Tyto soubory sdílet dvě běžné rozložení pro oblast nadpisu a oblasti tlačítka.</span><span class="sxs-lookup"><span data-stu-id="90c36-238">These files share two common layouts for the title area and the button area.</span></span> <span data-ttu-id="90c36-239">Rozložení pro nadpis `engagement_content_title.xml` a použije eponymous drawable soubor pozadí.</span><span class="sxs-lookup"><span data-stu-id="90c36-239">The layout for the title is `engagement_content_title.xml` and uses the eponymous drawable file for the background.</span></span> <span data-ttu-id="90c36-240">Rozložení pro tlačítek akce a ukončení `engagement_button_bar.xml` a použije eponymous drawable soubor pozadí.</span><span class="sxs-lookup"><span data-stu-id="90c36-240">The layout for the action and exit buttons is `engagement_button_bar.xml` and uses the eponymous drawable file for the background.</span></span>

<span data-ttu-id="90c36-241">V hlasování, rozložení otázku a jejich možnosti jsou dynamicky zvětšený pomocí několikrát `engagement_question.xml` soubor rozložení pro otázky a `engagement_choice.xml` soubor pro výběr.</span><span class="sxs-lookup"><span data-stu-id="90c36-241">In a poll, the question layout and their choices are dynamically inflated using several times the `engagement_question.xml` layout file for the questions and the `engagement_choice.xml` file for the choices.</span></span>

#### <a name="categories"></a><span data-ttu-id="90c36-242">Kategorie</span><span class="sxs-lookup"><span data-stu-id="90c36-242">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="90c36-243">Alternativní rozložení</span><span class="sxs-lookup"><span data-stu-id="90c36-243">Alternate layouts</span></span>
<span data-ttu-id="90c36-244">Jako oznámení kategorie kampaně umožňuje mít alternativní rozložení pro oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="90c36-244">Like notifications, the campaign's category can be used to have alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="90c36-245">Pokud chcete vytvořit kategorii pro text oznámení, například můžete rozšířit `EngagementTextAnnouncementActivity` a na něj odkazovat `AndroidManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="90c36-245">For example, to create a category for a text announcement, you can extend `EngagementTextAnnouncementActivity` and reference it the `AndroidManifest.xml` file:</span></span>

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

<span data-ttu-id="90c36-246">Všimněte si, že kategorie ve záměrné filtru se používá k zajištění rozdíl oproti výchozí aktivita oznámení.</span><span class="sxs-lookup"><span data-stu-id="90c36-246">Note that the category in the intent filter is used to make the difference with the default announcement activity.</span></span>

<span data-ttu-id="90c36-247">Sady Reach SDK používá záměrné systému přeložit vpravo aktivity pro určitou kategorii a jeho spadne zpět na výchozí kategorie Pokud řešení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="90c36-247">The Reach SDK uses the intent system to resolve the right activity for a specific category and it falls back on the default category if the resolution failed.</span></span>

<span data-ttu-id="90c36-248">Je třeba implementovat `MyCustomTextAnnouncementActivity`, pokud chcete změnit rozložení (ale zachovat stejné identifikátory zobrazení), musíte se definice třídy, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="90c36-248">Then you have to implement `MyCustomTextAnnouncementActivity`, if you just want to change the layout (but keep the same view identifiers), you just have to define the class like in the following example:</span></span>

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

<span data-ttu-id="90c36-249">Pokud chcete nahradit kategorii výchozí text oznámení, jednoduše nahradit `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` podle vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="90c36-249">To replace the default category of text announcements, simply replace `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` by your implementation.</span></span>

<span data-ttu-id="90c36-250">Podobně lze přizpůsobit web oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="90c36-250">Web announcements and polls can be customized in a similar fashion.</span></span>

<span data-ttu-id="90c36-251">Pro web oznámení můžete rozšířit `EngagementWebAnnouncementActivity` a deklarovat si v aktivitu `AndroidManifest.xml` jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="90c36-251">For web announcements you can extend `EngagementWebAnnouncementActivity` and declare your activity in the `AndroidManifest.xml` like in the following example:</span></span>

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

<span data-ttu-id="90c36-252">Pro dotazování můžete rozšířit `EngagementPollActivity` a deklarovat vaší v `AndroidManifest.xml` jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="90c36-252">For polls you can extend `EngagementPollActivity` and declare your in the `AndroidManifest.xml` like in the following example:</span></span>

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a><span data-ttu-id="90c36-253">Implementace od začátku</span><span class="sxs-lookup"><span data-stu-id="90c36-253">Implementation from scratch</span></span>
<span data-ttu-id="90c36-254">Kategorie můžete implementovat vaše aktivity oznámení (a dotazování) bez jeden z rozšíření `Engagement*Activity` třídy poskytované Reach SDK.</span><span class="sxs-lookup"><span data-stu-id="90c36-254">You can implement categories for your announcement (and poll) activities without extending one of the `Engagement*Activity` classes provided by the Reach SDK.</span></span> <span data-ttu-id="90c36-255">To je užitečné, například pokud chcete definovat rozložení, který nepoužívá stejnou zobrazení jako standardní rozložení.</span><span class="sxs-lookup"><span data-stu-id="90c36-255">This is useful for example if you want to define a layout that does not use the same views as the standard layouts.</span></span>

<span data-ttu-id="90c36-256">Jako pro přizpůsobení pokročilé oznámení, doporučujeme prohlédnout si zdrojový kód standardní implementace.</span><span class="sxs-lookup"><span data-stu-id="90c36-256">Like for advanced notification customization, it is recommended to look at the source code of the standard implementation.</span></span>

<span data-ttu-id="90c36-257">Tady jsou některé věci, třeba vzít v úvahu: Reach spustí aktivita konkrétní záměrem (odpovídající záměrné filtr) plus další parametr, který je identifikátor obsahu.</span><span class="sxs-lookup"><span data-stu-id="90c36-257">Here are some things to keep in mind: Reach will launch the activity with a specific intent (corresponding to the intent filter) plus an extra parameter which is the content identifier.</span></span>

<span data-ttu-id="90c36-258">Chcete-li načíst objekt obsahu, který obsahovat pole, která jste zadali při vytváření kampaň na webu můžete postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="90c36-258">To retrieve the content object which contain the fields you specified when creating the campaign on the web site you can do this:</span></span>

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

<span data-ttu-id="90c36-259">Pro statistiku, byste měli hlásit obsah se zobrazí v `onResume` událostí:</span><span class="sxs-lookup"><span data-stu-id="90c36-259">For statistics, you should report the content is displayed in the `onResume` event:</span></span>

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

<span data-ttu-id="90c36-260">Potom, nezapomeňte volat buď `actionContent(this)` nebo `exitContent(this)` v obsahu objektu před aktivity přejde do pozadí.</span><span class="sxs-lookup"><span data-stu-id="90c36-260">Then, don't forget to call either `actionContent(this)` or `exitContent(this)` on the content object before the activity goes into background.</span></span>

<span data-ttu-id="90c36-261">Pokud nemůžete volat buď `actionContent` nebo `exitContent`, statistiky se neodešlou (tj. žádné analýzy kampaň) a je důležité, nebude další kampaně upozorněni, až po restartování procesu aplikace.</span><span class="sxs-lookup"><span data-stu-id="90c36-261">If you don't call either `actionContent` or `exitContent`, statistics won't be sent (i.e. no analytics on the campaign) and more importantly, the next campaigns will not be notified until the application process is restarted.</span></span>

<span data-ttu-id="90c36-262">Orientace nebo jiné změny konfigurace můžete provést kód složité určit, zda aktivity přejde do pozadí nebo Ne, standardní implementace zajišťuje obsah hlášení jako byl ukončen, pokud uživatel odejde aktivity (buď stisknutím `HOME`nebo `BACK`), ale ne, pokud se změní orientaci.</span><span class="sxs-lookup"><span data-stu-id="90c36-262">Orientation or other configuration changes can make the code tricky to determine whether the activity goes into background or not, the standard implementation makes sure the content is reported as exited if the user leaves the activity (either by pressing `HOME` or `BACK`) but not if the orientation changes.</span></span>

<span data-ttu-id="90c36-263">Tady je zajímavé součástí implementace:</span><span class="sxs-lookup"><span data-stu-id="90c36-263">Here is the interesting part of the implementation:</span></span>

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
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

<span data-ttu-id="90c36-264">Jak můžete vidět, pokud jste volali metodu `actionContent(this)` pak dokončení aktivity, `exitContent(this)` lze bezpečně volat bez nutnosti nijak neprojeví.</span><span class="sxs-lookup"><span data-stu-id="90c36-264">As you can see, if you called `actionContent(this)` then finished the activity, `exitContent(this)` can be safely called without having any effect.</span></span>

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
<span data-ttu-id="90c36-265">[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html</span><span class="sxs-lookup"><span data-stu-id="90c36-265">[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html</span></span>
<span data-ttu-id="90c36-266">[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html</span><span class="sxs-lookup"><span data-stu-id="90c36-266">[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html</span></span>
