---
title: "aaaNotification nejnovější zprávy kurz Hubs - Android"
description: "Zjistěte, jak toosend Azure Service Bus Notification Hubs toouse nejnovější zprávy oznámení tooAndroid zařízení."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: e6eb41bec95c67d7dc059f560194966d04400494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="12475-103">Použít nejnovější zprávy přes toosend centra oznámení</span><span class="sxs-lookup"><span data-stu-id="12475-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="12475-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="12475-104">Overview</span></span>
<span data-ttu-id="12475-105">Toto téma ukazuje, jak toouse Azure Notification Hubs toobroadcast nejnovější zprávy oznámení tooan aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="12475-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooan Android app.</span></span> <span data-ttu-id="12475-106">Po dokončení budete mít možnost tooregister pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="12475-106">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="12475-107">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení odeslaných toobe toogroups uživatelů, kteří mají dříve deklarované zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.</span><span class="sxs-lookup"><span data-stu-id="12475-107">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="12475-108">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="12475-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="12475-109">Pokud jsou oznámení odesílána tooa značky, všechna zařízení, která jste zaregistrovali hello značky obdrží oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="12475-109">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="12475-110">Protože značky jsou jednoduše řetězce, nemají toobe předem zřízený.</span><span class="sxs-lookup"><span data-stu-id="12475-110">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="12475-111">Další informace o značkách najdete v části příliš[směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="12475-111">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12475-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="12475-112">Prerequisites</span></span>
<span data-ttu-id="12475-113">Toto téma je založený na hello aplikace, které jste vytvořili v [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="12475-113">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="12475-114">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="12475-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="12475-115">Přidat aplikaci toohello výběru kategorie</span><span class="sxs-lookup"><span data-stu-id="12475-115">Add category selection toohello app</span></span>
<span data-ttu-id="12475-116">prvním krokem Hello je tooadd hello uživatelského rozhraní elementy tooyour existující hlavní aktivitu, povolit hello uživatele tooselect kategorie tooregister.</span><span class="sxs-lookup"><span data-stu-id="12475-116">hello first step is tooadd hello UI elements tooyour existing main activity that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="12475-117">kategorie Hello vybrané uživatelem se ukládají na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="12475-117">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="12475-118">Při spuštění aplikace hello, registrace zařízení se vytvoří v centru oznámení s hello vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="12475-118">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="12475-119">Otevřete soubor res/layout/activity_main.xml a nahraďte obsah hello s hello následující:</span><span class="sxs-lookup"><span data-stu-id="12475-119">Open your res/layout/activity_main.xml file, and substitute hello content with hello following:</span></span>
   
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">
   
                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>
2. <span data-ttu-id="12475-120">Otevřete soubor res/values/strings.xml a přidejte následující řádky hello:</span><span class="sxs-lookup"><span data-stu-id="12475-120">Open your res/values/strings.xml file and add hello following lines:</span></span>
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    <span data-ttu-id="12475-121">Grafické rozložení main_activity.xml by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="12475-121">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
3. <span data-ttu-id="12475-122">Teď vytvořte třídu **oznámení** v hello stejný balíček vaše **MainActivity** třídy.</span><span class="sxs-lookup"><span data-stu-id="12475-122">Now create a class **Notifications** in hello same package as your **MainActivity** class.</span></span>
   
        import java.util.HashSet;
        import java.util.Set;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;
   
            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
   
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }
   
            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }
   
            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }
   
            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
   
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed tooregister - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
   
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }
   
        }
   
    <span data-ttu-id="12475-123">Tato třída se používá místní úložiště hello toostore hello kategorie zprávy, že toto zařízení má tooreceive.</span><span class="sxs-lookup"><span data-stu-id="12475-123">This class uses hello local storage toostore hello categories of news that this device has tooreceive.</span></span> <span data-ttu-id="12475-124">Obsahuje také metody tooregister pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="12475-124">It also contains methods tooregister for these categories.</span></span>
4. <span data-ttu-id="12475-125">Ve vaší **MainActivity** třída odebrat vaší privátní pole pro **NotificationHub** a **GoogleCloudMessaging**, a přidejte pole pro **oznámení**:</span><span class="sxs-lookup"><span data-stu-id="12475-125">In your **MainActivity** class remove your private fields for **NotificationHub** and **GoogleCloudMessaging**, and add a field for **Notifications**:</span></span>
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. <span data-ttu-id="12475-126">Potom v hello **onCreate** metoda, odeberte hello inicializace hello **rozbočovače** pole a hello **registerWithNotificationHubs** metoda.</span><span class="sxs-lookup"><span data-stu-id="12475-126">Then, in hello **onCreate** method, remove hello initialization of hello **hub** field and hello **registerWithNotificationHubs** method.</span></span> <span data-ttu-id="12475-127">Pak přidejte následující řádky, které inicializovat instanci hello hello **oznámení** třídy.</span><span class="sxs-lookup"><span data-stu-id="12475-127">Then add hello following lines which initialize an instance of hello **Notifications** class.</span></span> 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    <span data-ttu-id="12475-128">`HubName`a `HubListenConnectionString` by měl být již nastaven s hello `<hub name>` a `<connection string with listen access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultListenSharedAccessSignature* získaný dříve.</span><span class="sxs-lookup"><span data-stu-id="12475-128">`HubName` and `HubListenConnectionString` should already be set with hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>

    > [AZURE.NOTE] <span data-ttu-id="12475-129">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci nejsou obecně bezpečné, musí distribuovat hello klíč pro přístup k naslouchání pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="12475-129">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="12475-130">Poslechněte umožní přístup k vaší aplikaci tooregister oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="12475-130">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="12475-131">Hello úplné přístupový klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="12475-131">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>


1. <span data-ttu-id="12475-132">Pak přidejte následující hello importuje a `subscribe` metoda toohandle hello přihlášení k odběru tlačítko klikněte na událost:</span><span class="sxs-lookup"><span data-stu-id="12475-132">Then, add hello following imports and `subscribe` method toohandle hello subscribe button click event:</span></span>
   
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;
   
        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();
   
            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");
   
            notifications.storeCategoriesAndSubscribe(categories);
        }
   
    <span data-ttu-id="12475-133">Tato metoda vytvoří seznam kategorií a používá hello **oznámení** třídy toostore hello seznamu v místním úložišti hello a zaregistrujte hello značky odpovídající pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="12475-133">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="12475-134">Při změně kategorií, hello registrace se znovu vytvoří se nové kategorie hello.</span><span class="sxs-lookup"><span data-stu-id="12475-134">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="12475-135">Aplikace je nyní možné toostore sadu kategorií v místním úložišti na hello zařízení a zaregistrujte hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.</span><span class="sxs-lookup"><span data-stu-id="12475-135">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="12475-136">Registrace pro oznámení</span><span class="sxs-lookup"><span data-stu-id="12475-136">Register for notifications</span></span>
<span data-ttu-id="12475-137">Tyto kroky zaregistrovat hello centra oznámení na spuštění pomocí hello kategorií, které byly uloženy v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="12475-137">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="12475-138">Protože hello registrationId přiřazené pomocí zasílání zpráv cloudu Google (GCM) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často tooavoid oznámení selhání.</span><span class="sxs-lookup"><span data-stu-id="12475-138">Because hello registrationId assigned by Google Cloud Messaging (GCM) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="12475-139">Tento příklad zaregistruje oznámení pokaždé, když spustí aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="12475-139">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="12475-140">Pro aplikace, které jsou často spouštíte více než jednou denně, pravděpodobně Pokud můžete přeskočit šířky pásma toopreserve registrace od předchozí registrace hello uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="12475-140">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="12475-141">Přidejte následující kód na konci hello hello hello **onCreate** metoda v hello **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="12475-141">Add hello following code at hello end of hello **onCreate** method in hello **MainActivity** class:</span></span>
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    <span data-ttu-id="12475-142">Tím je zajištěno, že při každém spuštění aplikace hello načte kategorie hello z místního úložiště a požadavky registrace pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="12475-142">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registeration for these categories.</span></span> 
2. <span data-ttu-id="12475-143">Aktualizujte hello `onStart()` metoda hello `MainActivity` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="12475-143">Then update hello `onStart()` method of hello `MainActivity` class as follows:</span></span>
   
    <span data-ttu-id="12475-144">@Overridechráněné void onStart() {</span><span class="sxs-lookup"><span data-stu-id="12475-144">@Override  protected void onStart() {</span></span>
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    <span data-ttu-id="12475-145">}</span><span class="sxs-lookup"><span data-stu-id="12475-145">}</span></span>
   
    <span data-ttu-id="12475-146">Tím se aktualizuje hello hlavní činnosti na základě stavu hello dříve uloženou kategorií.</span><span class="sxs-lookup"><span data-stu-id="12475-146">This updates hello main activity based on hello status of previously saved categories.</span></span>

<span data-ttu-id="12475-147">Hello aplikace je nyní dokončen a může ukládat sadu kategorií hello zařízení používá místní úložiště tooregister hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.</span><span class="sxs-lookup"><span data-stu-id="12475-147">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="12475-148">V dalším kroku bude definujeme back-end, který může odesílat kategorie oznámení toothis aplikace.</span><span class="sxs-lookup"><span data-stu-id="12475-148">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="12475-149">Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="12475-149">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="12475-150">Spuštění aplikace hello a generovat oznámení</span><span class="sxs-lookup"><span data-stu-id="12475-150">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="12475-151">V nástroji Android Studio sestavení aplikace hello a spusťte jej na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="12475-151">In Android Studio, build hello app and start it on a device or emulator.</span></span>
   
    <span data-ttu-id="12475-152">Všimněte si, že přepíná hello aplikaci, kterou poskytuje sadu uživatelského rozhraní, která umožňuje vybrat toosubscribe kategorie hello k.</span><span class="sxs-lookup"><span data-stu-id="12475-152">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="12475-153">Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="12475-153">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="12475-154">aplikace Hello převede hello vybrané kategorie značky a požaduje novou registraci zařízení pro hello vybrané značky z centra oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="12475-154">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="12475-155">Hello registrované kategorie jsou vráceny a zobrazeny v oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="12475-155">hello registered categories are returned and displayed in a toast notification.</span></span>
3. <span data-ttu-id="12475-156">Nové oznámení odesílat spuštěním aplikace konzoly .NET hello.</span><span class="sxs-lookup"><span data-stu-id="12475-156">Send a new notification by running hello .NET Console app.</span></span>  <span data-ttu-id="12475-157">Alternativně můžete odeslat oznámení s příznakem šablony pomocí karty ladění hello centra oznámení v hello [portálu Azure Classic].</span><span class="sxs-lookup"><span data-stu-id="12475-157">Alternatively, you can send tagged template notifications using hello debug tab of your notification hub in hello [Azure Classic Portal].</span></span>
   
    <span data-ttu-id="12475-158">Oznámení pro hello vybrané kategorie se zobrazí jako informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="12475-158">Notifications for hello selected categories appear as toast notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12475-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12475-159">Next steps</span></span>
<span data-ttu-id="12475-160">V tomto kurzu jsme se dozvěděli, jak toobroadcast nejnovější zprávy přes podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="12475-160">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="12475-161">Vezměte v úvahu dokončení jednu z následujících návodů, které zvýrazněte pokročilé scénáře Notification Hubs hello:</span><span class="sxs-lookup"><span data-stu-id="12475-161">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="12475-162">[Použití centra oznámení toobroadcast lokalizované novinek]</span><span class="sxs-lookup"><span data-stu-id="12475-162">[Use Notification Hubs toobroadcast localized breaking news]</span></span>
  
    <span data-ttu-id="12475-163">Zjistěte, jak lokalizované tooexpand hello nejnovější novinky aplikace tooenable odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="12475-163">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Použití centra oznámení toobroadcast lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[portálu Azure Classic]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
