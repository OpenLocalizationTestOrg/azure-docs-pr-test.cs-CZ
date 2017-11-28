---
title: "Odesílání zabezpečené nabízených oznámení pomocí Azure Notification Hubs"
description: "Naučte se odesílání zabezpečené nabízených oznámení do aplikace pro Android z Azure. Ukázky kódu jsou vytvořeny v jazyce Java a C#."
documentationcenter: android
keywords: "nabízená oznámení, nabízená oznámení, nabízené zprávy, android nabízená oznámení"
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 29f8c516e611c13fb73c7edc15e7c52708c75bb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a><span data-ttu-id="3c2a4-105">Odesílání zabezpečené nabízených oznámení pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="3c2a4-105">Sending Secure Push Notifications with Azure Notification Hubs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c2a4-106">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="3c2a4-106">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="3c2a4-107">iOS</span><span class="sxs-lookup"><span data-stu-id="3c2a4-107">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="3c2a4-108">Android</span><span class="sxs-lookup"><span data-stu-id="3c2a4-108">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3c2a4-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="3c2a4-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3c2a4-110">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-110">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="3c2a4-111">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3c2a4-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="3c2a4-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="3c2a4-113">Podpora nabízená oznámení v Microsoft Azure umožňuje získat přístup snadno použitelnou, multiplatformní a upraveným nabízená zpráva infrastruktury, což výrazně zjednodušuje implementaci nabízená oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-113">Push notification support in Microsoft Azure enables you to access an easy-to-use, multiplatform, scaled-out push message infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="3c2a4-114">Kvůli zákonným omezení zabezpečení, někdy aplikace může chtít zahrnout něco v oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro standardní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-114">Due to regulatory or security constraints, sometimes an application might want to include something in the notification that cannot be transmitted through the standard push notification infrastructure.</span></span> <span data-ttu-id="3c2a4-115">Tento kurz popisuje, jak zajistit stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi klientské zařízení Android a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-115">This tutorial describes how to achieve the same experience by sending sensitive information through a secure, authenticated connection between the client Android device and the app backend.</span></span>

<span data-ttu-id="3c2a4-116">Na vysoké úrovni tok je následující:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-116">At a high level, the flow is as follows:</span></span>

1. <span data-ttu-id="3c2a4-117">Back-end aplikace:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-117">The app back-end:</span></span>
   * <span data-ttu-id="3c2a4-118">Zabezpečení datové úložiště v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-118">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="3c2a4-119">ID tohoto oznámení se odešle do zařízení s Androidem (zabezpečené nebudou odeslány žádné informace).</span><span class="sxs-lookup"><span data-stu-id="3c2a4-119">Sends the ID of this notification to the Android device (no secure information is sent).</span></span>
2. <span data-ttu-id="3c2a4-120">Aplikace na zařízení, když obdrží oznámení:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-120">The app on the device, when receiving the notification:</span></span>
   * <span data-ttu-id="3c2a4-121">Zařízení s Androidem kontaktuje back-end vyžaduje zabezpečené datové části.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-121">The Android device contacts the back-end requesting the secure payload.</span></span>
   * <span data-ttu-id="3c2a4-122">Aplikace můžete zobrazit datové části jako upozornění na zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-122">The app can show the payload as a notification on the device.</span></span>

<span data-ttu-id="3c2a4-123">Je důležité si uvědomit, že v předchozím toku (a v tomto kurzu) předpokládáme, že zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-123">It is important to note that in the preceding flow (and in this tutorial), we assume that the device stores an authentication token in local storage, after the user logs in.</span></span> <span data-ttu-id="3c2a4-124">Zaručí se tím úplně jednoduché prostředí, protože zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové na oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-124">This guarantees a completely seamless experience, as the device can retrieve the notification’s secure payload using this token.</span></span> <span data-ttu-id="3c2a4-125">Pokud vaše aplikace nejsou uložené tokeny ověřování v zařízení, nebo pokud tyto tokeny můžete vypršela platnost, by měla aplikace zařízení při přijetí nabízeného oznámení zobrazit obecné oznámení uživateli zobrazuje výzvu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-125">If your application does not store authentication tokens on the device, or if these tokens can be expired, the device app, upon receiving the push notification should display a generic notification prompting the user to launch the app.</span></span> <span data-ttu-id="3c2a4-126">Aplikace pak ověřuje uživatele a ukazuje datová část oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-126">The app then authenticates the user and shows the notification payload.</span></span>

<span data-ttu-id="3c2a4-127">Tento kurz ukazuje, jak odesílat zabezpečené nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-127">This tutorial shows how to send secure push notifications.</span></span> <span data-ttu-id="3c2a4-128">Vychází [upozornění uživatelů](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) kurzu, takže byste měli dokončit kroky v tomto kurzu nejprve Pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-128">It builds on the [Notify Users](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) tutorial, so you should complete the steps in that tutorial first if you haven't already.</span></span>

> [!NOTE]
> <span data-ttu-id="3c2a4-129">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3c2a4-129">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a><span data-ttu-id="3c2a4-130">Upravit projektu pro Android</span><span class="sxs-lookup"><span data-stu-id="3c2a4-130">Modify the Android project</span></span>
<span data-ttu-id="3c2a4-131">Teď, když změnit váš back-end aplikace k odesílání jen na *id* nabízená oznámení, budete muset změnit svoji aplikaci pro Android ke zpracování tohoto oznámení a zpětné volání váš back-end pro načtení zabezpečenou zprávu, který se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-131">Now that you modified your app back-end to send just the *id* of a push notification, you have to change your Android app to handle that notification and call back your back-end to retrieve the secure message to be displayed.</span></span>
<span data-ttu-id="3c2a4-132">K dosažení tohoto cíle, budete muset Ujistěte se, že vaše aplikace pro Android umí ke svému ověření s back-end, když obdrží nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-132">To achieve this goal, you have to make sure that your Android app knows how to authenticate itself with your back-end when it receives the push notifications.</span></span>

<span data-ttu-id="3c2a4-133">Nyní jsme upraví *přihlášení* toku a uložte hodnota hlavičky ověřování v sdílet předvolby vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-133">We will now modify the *login* flow in order to save the authentication header value in the shared preferences of your app.</span></span> <span data-ttu-id="3c2a4-134">Podobá mechanismy slouží k uložení žádné ověřovací token (např. tokenů OAuth), která aplikace bude muset používat bez nutnosti přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-134">Analogous mechanisms can be used to store any authentication token (e.g. OAuth tokens) that the app will have to use without requiring user credentials.</span></span>

1. <span data-ttu-id="3c2a4-135">V projektu aplikace pro Android, přidejte následující konstanty v horní části **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-135">In your Android app project, add the following constants at the top of the **MainActivity** class:</span></span>
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. <span data-ttu-id="3c2a4-136">Pořád ještě v **MainActivity** třídy, aktualizaci `getAuthorizationHeader()` metoda obsahuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-136">Still in the **MainActivity** class, update the `getAuthorizationHeader()` method to contain the following code:</span></span>
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. <span data-ttu-id="3c2a4-137">Přidejte následující `import` příkazy v horní části **MainActivity** souboru:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-137">Add the following `import` statements at the top of the **MainActivity** file:</span></span>
   
        import android.content.SharedPreferences;

<span data-ttu-id="3c2a4-138">Nyní změníme obslužná rutina, která je volána, když bylo přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-138">Now we will change the handler that is called when the notification is received.</span></span>

1. <span data-ttu-id="3c2a4-139">V **MyHandler** třídy změnu `OnReceive()` metoda obsahuje:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-139">In the **MyHandler** class change the `OnReceive()` method to contain:</span></span>
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. <span data-ttu-id="3c2a4-140">Pak přidejte `retrieveNotification()` metoda, nahraďte zástupný symbol `{back-end endpoint}` s koncovým bodem back-end získali při nasazování back-end:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-140">Then add the `retrieveNotification()` method, replacing the placeholder `{back-end endpoint}` with the back-end endpoint obtained while deploying your back-end:</span></span>
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

<span data-ttu-id="3c2a4-141">Tato metoda volá váš back-end aplikace k získání obsahu oznámení pomocí pověření uložených v sdílet předvolby a zobrazí jako normální oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-141">This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences and displays it as a normal notification.</span></span> <span data-ttu-id="3c2a4-142">Oznámení vypadá na uživatele aplikace úplně stejně jako ostatní nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-142">The notification looks to the app user exactly like any other push notification.</span></span>

<span data-ttu-id="3c2a4-143">Všimněte si, že je vhodnější pro zpracování v případech chybějící vlastnost hlavičky ověřování nebo odmítání back-end.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-143">Note that it is preferable to handle the cases of missing authentication header property or rejection by the back-end.</span></span> <span data-ttu-id="3c2a4-144">Konkrétní zpracování těchto případech závisí hlavně na cílové činnost koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-144">The specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="3c2a4-145">Jednou z možností je zobrazit oznámení s výzvou obecný pro ověření uživatele pro načtení skutečné oznámení.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-145">One option is to display a notification with a generic prompt for the user to authenticate to retrieve the actual notification.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="3c2a4-146">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="3c2a4-146">Run the Application</span></span>
<span data-ttu-id="3c2a4-147">Ke spuštění aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3c2a4-147">To run the application, do the following:</span></span>

1. <span data-ttu-id="3c2a4-148">Zajistěte, aby **AppBackend** je nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-148">Make sure **AppBackend** is deployed in Azure.</span></span> <span data-ttu-id="3c2a4-149">Pokud používáte Visual Studio, spusťte **AppBackend** aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-149">If using Visual Studio, run the **AppBackend** Web API application.</span></span> <span data-ttu-id="3c2a4-150">Zobrazí se webová stránka ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-150">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="3c2a4-151">V nástroji Eclipse spusťte aplikaci v emulátoru nebo fyzické zařízení Android.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-151">In Eclipse, run the app on a physical Android device or the emulator.</span></span>
3. <span data-ttu-id="3c2a4-152">V systému Android aplikace uživatelského rozhraní zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-152">In the Android app UI, enter a username and password.</span></span> <span data-ttu-id="3c2a4-153">Mohou to být libovolný řetězec, ale musí být stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-153">These can be any string, but they must be the same value.</span></span>
4. <span data-ttu-id="3c2a4-154">V systému Android aplikace uživatelského rozhraní, klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-154">In the Android app UI, click **Log in**.</span></span> <span data-ttu-id="3c2a4-155">Pak klikněte na tlačítko **odeslat nabízené**.</span><span class="sxs-lookup"><span data-stu-id="3c2a4-155">Then click **Send push**.</span></span>

