---
title: "Centra oznámení nejnovější zprávy přes kurz - Android"
description: "Naučte se používat Azure Service Bus Notification Hubs k odesílání oznámení o aktuálních zprávách do zařízení se systémem Android."
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
ms.openlocfilehash: 76ec01c874fceedab7d76b2ef58e4b45b5489f58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="f2a81-103">Používání centra oznámení k odesílání novinek</span><span class="sxs-lookup"><span data-stu-id="f2a81-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="f2a81-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f2a81-104">Overview</span></span>
<span data-ttu-id="f2a81-105">Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="f2a81-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to an Android app.</span></span> <span data-ttu-id="f2a81-106">Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-106">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="f2a81-107">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.</span><span class="sxs-lookup"><span data-stu-id="f2a81-107">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="f2a81-108">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="f2a81-109">Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-109">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="f2a81-110">Protože značky jsou jednoduše řetězce, nemají být předem zřízená.</span><span class="sxs-lookup"><span data-stu-id="f2a81-110">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="f2a81-111">Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="f2a81-111">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2a81-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f2a81-112">Prerequisites</span></span>
<span data-ttu-id="f2a81-113">Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="f2a81-113">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="f2a81-114">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="f2a81-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="f2a81-115">Přidat výběru kategorie do aplikace</span><span class="sxs-lookup"><span data-stu-id="f2a81-115">Add category selection to the app</span></span>
<span data-ttu-id="f2a81-116">Prvním krokem je přidání prvky uživatelského rozhraní pro vaše stávající hlavní aktivitu, který uživateli umožňuje výběr kategorií k registraci.</span><span class="sxs-lookup"><span data-stu-id="f2a81-116">The first step is to add the UI elements to your existing main activity that enable the user to select categories to register.</span></span> <span data-ttu-id="f2a81-117">Kategorie, které uživatel jsou uloženy v zařízení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-117">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="f2a81-118">Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="f2a81-118">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="f2a81-119">Otevřete soubor res/layout/activity_main.xml a nahraďte obsah s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="f2a81-119">Open your res/layout/activity_main.xml file, and substitute the content with the following:</span></span>
   
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
2. <span data-ttu-id="f2a81-120">Otevřete soubor res/values/strings.xml a přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="f2a81-120">Open your res/values/strings.xml file and add the following lines:</span></span>
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    <span data-ttu-id="f2a81-121">Grafické rozložení main_activity.xml by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="f2a81-121">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
3. <span data-ttu-id="f2a81-122">Teď vytvořte třídu **oznámení** ve stejném balíčku jako vaše **MainActivity** třídy.</span><span class="sxs-lookup"><span data-stu-id="f2a81-122">Now create a class **Notifications** in the same package as your **MainActivity** class.</span></span>
   
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
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
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
   
    <span data-ttu-id="f2a81-123">Tato třída používá místní úložiště k ukládání kategorie příspěvků, který toto zařízení má přijmout.</span><span class="sxs-lookup"><span data-stu-id="f2a81-123">This class uses the local storage to store the categories of news that this device has to receive.</span></span> <span data-ttu-id="f2a81-124">Obsahuje také metody pro registraci pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-124">It also contains methods to register for these categories.</span></span>
4. <span data-ttu-id="f2a81-125">Ve vaší **MainActivity** třída odebrat vaší privátní pole pro **NotificationHub** a **GoogleCloudMessaging**, a přidejte pole pro **oznámení**:</span><span class="sxs-lookup"><span data-stu-id="f2a81-125">In your **MainActivity** class remove your private fields for **NotificationHub** and **GoogleCloudMessaging**, and add a field for **Notifications**:</span></span>
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. <span data-ttu-id="f2a81-126">Potom v **onCreate** metoda, odeberte inicializace **rozbočovače** pole a **registerWithNotificationHubs** metoda.</span><span class="sxs-lookup"><span data-stu-id="f2a81-126">Then, in the **onCreate** method, remove the initialization of the **hub** field and the **registerWithNotificationHubs** method.</span></span> <span data-ttu-id="f2a81-127">Pak přidejte následující řádky, které inicializovat instanci **oznámení** třídy.</span><span class="sxs-lookup"><span data-stu-id="f2a81-127">Then add the following lines which initialize an instance of the **Notifications** class.</span></span> 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    <span data-ttu-id="f2a81-128">`HubName`a `HubListenConnectionString` by měl být již nastaven s `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="f2a81-128">`HubName` and `HubListenConnectionString` should already be set with the `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>

    > [AZURE.NOTE] <span data-ttu-id="f2a81-129">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2a81-129">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="f2a81-130">Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-130">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="f2a81-131">Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="f2a81-131">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>


1. <span data-ttu-id="f2a81-132">Pak přidejte následující importy a `subscribe` metodu ke zpracování na tlačítko přihlásit k odběru, klikněte na událost:</span><span class="sxs-lookup"><span data-stu-id="f2a81-132">Then, add the following imports and `subscribe` method to handle the subscribe button click event:</span></span>
   
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
   
    <span data-ttu-id="f2a81-133">Tato metoda vytvoří seznam kategorií a používá **oznámení** značky třída pro uložení seznamu v místním úložišti a zaregistrujte odpovídající pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-133">This method creates a list of categories and uses the **Notifications** class to store the list in the local storage and register the corresponding tags with your notification hub.</span></span> <span data-ttu-id="f2a81-134">Při změně kategorií, registrace se znovu vytvoří se nové kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-134">When categories are changed, the registration is recreated with the new categories.</span></span>

<span data-ttu-id="f2a81-135">Aplikace je teď možné uložit sadu kategorií místní úložiště v zařízení a zaregistrovat do centra oznámení pokaždé, když uživatel změní výběr kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-135">Your app is now able to store a set of categories in local storage on the device and register with the notification hub whenever the user changes the selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="f2a81-136">Registrace pro oznámení</span><span class="sxs-lookup"><span data-stu-id="f2a81-136">Register for notifications</span></span>
<span data-ttu-id="f2a81-137">Tyto kroky zaregistrovat do centra oznámení na spouštění pomocí kategorií, které byly uloženy v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="f2a81-137">These steps register with the notification hub on startup using the categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="f2a81-138">Protože registrationId přiřazené pomocí zasílání zpráv cloudu Google (GCM) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-138">Because the registrationId assigned by Google Cloud Messaging (GCM) can change at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="f2a81-139">V tomto příkladu se zaregistruje pro oznámení při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2a81-139">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="f2a81-140">Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="f2a81-140">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
> 
> 

1. <span data-ttu-id="f2a81-141">Přidejte následující kód na konci **onCreate** metoda v **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="f2a81-141">Add the following code at the end of the **onCreate** method in the **MainActivity** class:</span></span>
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    <span data-ttu-id="f2a81-142">Tím je zajištěno, že při každém spuštění aplikace načte kategorie z místního úložiště a požadavky registrace pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-142">This makes sure that every time the app starts it retrieves the categories from local storage and requests a registeration for these categories.</span></span> 
2. <span data-ttu-id="f2a81-143">Aktualizujte `onStart()` metodu `MainActivity` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f2a81-143">Then update the `onStart()` method of the `MainActivity` class as follows:</span></span>
   
    <span data-ttu-id="f2a81-144">@Overridechráněné void onStart() {</span><span class="sxs-lookup"><span data-stu-id="f2a81-144">@Override  protected void onStart() {</span></span>
   
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
    <span data-ttu-id="f2a81-145">}</span><span class="sxs-lookup"><span data-stu-id="f2a81-145">}</span></span>
   
    <span data-ttu-id="f2a81-146">Tím se aktualizuje v hlavní aktivitě na základě stavu dříve uloženou kategorií.</span><span class="sxs-lookup"><span data-stu-id="f2a81-146">This updates the main activity based on the status of previously saved categories.</span></span>

<span data-ttu-id="f2a81-147">Aplikace je nyní dokončen a sadu kategorií můžete uložit do místního úložiště zařízení používá k registraci do centra oznámení pokaždé, když uživatel změní výběr kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-147">The app is now complete and can store a set of categories in the device local storage used to register with the notification hub whenever the user changes the selection of categories.</span></span> <span data-ttu-id="f2a81-148">V dalším kroku bude definujeme back-end, který kategorie oznámení můžete odesílat do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2a81-148">Next, we will define a backend that can send category notifications to this app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="f2a81-149">Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="f2a81-149">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="f2a81-150">Spusťte aplikaci a generovat upozornění</span><span class="sxs-lookup"><span data-stu-id="f2a81-150">Run the app and generate notifications</span></span>
1. <span data-ttu-id="f2a81-151">V nástroji Android Studio sestavení aplikace a spusťte jej na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-151">In Android Studio, build the app and start it on a device or emulator.</span></span>
   
    <span data-ttu-id="f2a81-152">Všimněte si, že aplikace uživatelského rozhraní, poskytuje sadu přepínačů, která vám umožní vybrat kategorie pro přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="f2a81-152">Note that the app UI provides a set of toggles that lets you choose the categories to subscribe to.</span></span>
2. <span data-ttu-id="f2a81-153">Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="f2a81-153">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="f2a81-154">Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="f2a81-154">The app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span> <span data-ttu-id="f2a81-155">Registrovaný kategorie jsou vráceny a zobrazeny v oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="f2a81-155">The registered categories are returned and displayed in a toast notification.</span></span>
3. <span data-ttu-id="f2a81-156">Nové oznámení odesílat spuštěním aplikace konzoly .NET.</span><span class="sxs-lookup"><span data-stu-id="f2a81-156">Send a new notification by running the .NET Console app.</span></span>  <span data-ttu-id="f2a81-157">Alternativně můžete odeslat oznámení s příznakem šablony pomocí karty ladění centra oznámení v [portálu Azure Classic].</span><span class="sxs-lookup"><span data-stu-id="f2a81-157">Alternatively, you can send tagged template notifications using the debug tab of your notification hub in the [Azure Classic Portal].</span></span>
   
    <span data-ttu-id="f2a81-158">Oznámení pro vybrané kategorie se zobrazí jako informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="f2a81-158">Notifications for the selected categories appear as toast notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2a81-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2a81-159">Next steps</span></span>
<span data-ttu-id="f2a81-160">V tomto kurzu jsme zjistili, jak k vysílání novinek podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="f2a81-160">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="f2a81-161">Vezměte v úvahu dokončení jednu z následujících kurzů upozorňující na jiné pokročilé scénáře centra oznámení:</span><span class="sxs-lookup"><span data-stu-id="f2a81-161">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="f2a81-162">[Použití centra oznámení k vysílání lokalizované novinek]</span><span class="sxs-lookup"><span data-stu-id="f2a81-162">[Use Notification Hubs to broadcast localized breaking news]</span></span>
  
    <span data-ttu-id="f2a81-163">Zjistěte, jak rozšířit aplikace nejnovější zprávy k povolení odesílání lokalizované upozornění.</span><span class="sxs-lookup"><span data-stu-id="f2a81-163">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
<span data-ttu-id="f2a81-164">[Použití centra oznámení k vysílání lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span><span class="sxs-lookup"><span data-stu-id="f2a81-164">[Use Notification Hubs to broadcast localized breaking news]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span></span>
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
<span data-ttu-id="f2a81-165">[portálu Azure Classic]: https://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="f2a81-165">[Azure Classic Portal]: https://manage.windowsazure.com</span></span>
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
