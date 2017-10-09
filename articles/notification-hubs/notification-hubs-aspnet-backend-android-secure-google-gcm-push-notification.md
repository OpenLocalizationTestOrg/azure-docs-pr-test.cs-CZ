---
title: "aaaSending zabezpečení nabízená oznámení pomocí Azure Notification Hubs"
description: "Zjistěte, jak toosend zabezpečené nabízená oznámení tooan aplikace pro Android z Azure. Ukázky kódu jsou vytvořeny v jazyce Java a C#."
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
ms.openlocfilehash: d07943c4691ed07acb987086228ef565e6281d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a><span data-ttu-id="cb29a-105">Odesílání zabezpečené nabízených oznámení pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="cb29a-105">Sending Secure Push Notifications with Azure Notification Hubs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cb29a-106">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="cb29a-106">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="cb29a-107">iOS</span><span class="sxs-lookup"><span data-stu-id="cb29a-107">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="cb29a-108">Android</span><span class="sxs-lookup"><span data-stu-id="cb29a-108">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="cb29a-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="cb29a-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cb29a-110">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="cb29a-110">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="cb29a-111">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="cb29a-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="cb29a-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="cb29a-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="cb29a-113">Podpora nabízená oznámení v Microsoft Azure umožňuje tooaccess infrastruktury zpráva snadno použitelnou, multiplatformní a upraveným nabízená, což výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="cb29a-113">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push message infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="cb29a-114">Z důvodu omezení tooregulatory nebo zabezpečení někdy aplikace může být vhodné tooinclude něco v hello oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro hello standardní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-114">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="cb29a-115">Tento kurz popisuje, jak tooachieve hello stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi hello klienta zařízení se systémem Android a back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cb29a-115">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client Android device and hello app backend.</span></span>

<span data-ttu-id="cb29a-116">Na vysoké úrovni tok hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cb29a-116">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="cb29a-117">back-end Hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="cb29a-117">hello app back-end:</span></span>
   * <span data-ttu-id="cb29a-118">Zabezpečení datové úložiště v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="cb29a-118">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="cb29a-119">Odešle ID hello tato oznámení toohello zařízení se systémem Android (zabezpečené nebudou odeslány žádné informace).</span><span class="sxs-lookup"><span data-stu-id="cb29a-119">Sends hello ID of this notification toohello Android device (no secure information is sent).</span></span>
2. <span data-ttu-id="cb29a-120">aplikace Hello na hello zařízení při přijetí oznámení hello:</span><span class="sxs-lookup"><span data-stu-id="cb29a-120">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="cb29a-121">zařízení se systémem Android Hello kontaktuje hello back-end žádajícího hello zabezpečené datové části.</span><span class="sxs-lookup"><span data-stu-id="cb29a-121">hello Android device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="cb29a-122">Hello aplikace můžete zobrazit datové části hello jako upozornění na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-122">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="cb29a-123">Je důležité, že toonote, v předchozím toku hello (a v tomto kurzu) předpokládáme, že hello zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="cb29a-123">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="cb29a-124">Zaručí se tím úplně jednoduché prostředí, protože hello zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-124">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="cb29a-125">Pokud vaše aplikace nejsou uložené ověřovací tokeny na hello zařízení nebo pokud tyto tokeny můžete vypršela platnost, by měla aplikace hello zařízení při přijetí nabízeného oznámení hello zobrazit obecné oznámení výzvy hello uživatele toolaunch hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb29a-125">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello push notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="cb29a-126">aplikace Hello pak ověřuje uživatele hello a ukazuje datová část oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="cb29a-126">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="cb29a-127">Tento kurz ukazuje, jak toosend zabezpečené nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-127">This tutorial shows how toosend secure push notifications.</span></span> <span data-ttu-id="cb29a-128">Vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) kurzu, takže byste měli dokončit hello kroky v tomto kurzu nejprve Pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="cb29a-128">It builds on hello [Notify Users](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) tutorial, so you should complete hello steps in that tutorial first if you haven't already.</span></span>

> [!NOTE]
> <span data-ttu-id="cb29a-129">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb29a-129">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-android-project"></a><span data-ttu-id="cb29a-130">Upravit hello projekt pro Android</span><span class="sxs-lookup"><span data-stu-id="cb29a-130">Modify hello Android project</span></span>
<span data-ttu-id="cb29a-131">Teď, když upravit vaše aplikace právě hello back-end toosend *id* nabízená oznámení, máte toochange vaší aplikace pro Android toohandle oznámení a zpětných volání váš back-end tooretrieve hello zabezpečit zprávy toobe zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cb29a-131">Now that you modified your app back-end toosend just hello *id* of a push notification, you have toochange your Android app toohandle that notification and call back your back-end tooretrieve hello secure message toobe displayed.</span></span>
<span data-ttu-id="cb29a-132">tooachieve tohoto cíle, máte jistotu, že zná svoji aplikaci pro Android toomake jak tooauthenticate samotné vaší back-end, pokud obdrží hello nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-132">tooachieve this goal, you have toomake sure that your Android app knows how tooauthenticate itself with your back-end when it receives hello push notifications.</span></span>

<span data-ttu-id="cb29a-133">Nyní jsme upraví hello *přihlášení* tok v pořadí toosave hello ověřování hodnota v hlavičce hello sdílet předvolby vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb29a-133">We will now modify hello *login* flow in order toosave hello authentication header value in hello shared preferences of your app.</span></span> <span data-ttu-id="cb29a-134">Podobá mechanismy lze použít toostore žádné ověřovací token (např. tokenů OAuth), který hello aplikace bude mít toouse bez nutnosti přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb29a-134">Analogous mechanisms can be used toostore any authentication token (e.g. OAuth tokens) that hello app will have toouse without requiring user credentials.</span></span>

1. <span data-ttu-id="cb29a-135">V projektu aplikace pro Android, přidejte následující konstanty hello horní části hello hello **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="cb29a-135">In your Android app project, add hello following constants at hello top of hello **MainActivity** class:</span></span>
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. <span data-ttu-id="cb29a-136">Stále v hello **MainActivity** třídy, aktualizaci hello `getAuthorizationHeader()` hello toocontain metoda následující kód:</span><span class="sxs-lookup"><span data-stu-id="cb29a-136">Still in hello **MainActivity** class, update hello `getAuthorizationHeader()` method toocontain hello following code:</span></span>
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. <span data-ttu-id="cb29a-137">Přidejte následující hello `import` příkazy hello horní části hello **MainActivity** souboru:</span><span class="sxs-lookup"><span data-stu-id="cb29a-137">Add hello following `import` statements at hello top of hello **MainActivity** file:</span></span>
   
        import android.content.SharedPreferences;

<span data-ttu-id="cb29a-138">Nyní změníme hello obslužná rutina, která je volána, když je obdržena hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-138">Now we will change hello handler that is called when hello notification is received.</span></span>

1. <span data-ttu-id="cb29a-139">V hello **MyHandler** třída změnit hello `OnReceive()` toocontain metoda:</span><span class="sxs-lookup"><span data-stu-id="cb29a-139">In hello **MyHandler** class change hello `OnReceive()` method toocontain:</span></span>
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. <span data-ttu-id="cb29a-140">Pak přidejte hello `retrieveNotification()` metoda, nahraďte zástupný symbol hello `{back-end endpoint}` s koncovým bodem back-end hello získali při nasazování back-end:</span><span class="sxs-lookup"><span data-stu-id="cb29a-140">Then add hello `retrieveNotification()` method, replacing hello placeholder `{back-end endpoint}` with hello back-end endpoint obtained while deploying your back-end:</span></span>
   
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
                        Log.e("MainActivity", "Failed tooretrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

<span data-ttu-id="cb29a-141">Tato metoda volá hello oznámení tooretrieve back-end aplikace obsahu pomocí hello přihlašovací údaje uložené v hello sdílet předvolby a zobrazí jako normální oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-141">This method calls your app back-end tooretrieve hello notification content using hello credentials stored in hello shared preferences and displays it as a normal notification.</span></span> <span data-ttu-id="cb29a-142">Hello oznámení vypadá uživatele aplikace toohello úplně stejně jako ostatní nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-142">hello notification looks toohello app user exactly like any other push notification.</span></span>

<span data-ttu-id="cb29a-143">Všimněte si, že je vhodnější toohandle hello případech chybějící vlastnost hlavičky ověřování nebo odmítání podle hello back-end.</span><span class="sxs-lookup"><span data-stu-id="cb29a-143">Note that it is preferable toohandle hello cases of missing authentication header property or rejection by hello back-end.</span></span> <span data-ttu-id="cb29a-144">Hello konkrétní zpracování těchto případech závisí hlavně na cílové činnost koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb29a-144">hello specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="cb29a-145">Jednou z možností je toodisplay oznámení s obecné výzvu hello uživatele tooauthenticate tooretrieve hello skutečné oznámení.</span><span class="sxs-lookup"><span data-stu-id="cb29a-145">One option is toodisplay a notification with a generic prompt for hello user tooauthenticate tooretrieve hello actual notification.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="cb29a-146">Spustit hello aplikace</span><span class="sxs-lookup"><span data-stu-id="cb29a-146">Run hello Application</span></span>
<span data-ttu-id="cb29a-147">toorun hello aplikace, hello následující:</span><span class="sxs-lookup"><span data-stu-id="cb29a-147">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="cb29a-148">Zajistěte, aby **AppBackend** je nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="cb29a-148">Make sure **AppBackend** is deployed in Azure.</span></span> <span data-ttu-id="cb29a-149">Pokud používáte Visual Studio, spusťte hello **AppBackend** aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cb29a-149">If using Visual Studio, run hello **AppBackend** Web API application.</span></span> <span data-ttu-id="cb29a-150">Zobrazí se webová stránka ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cb29a-150">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="cb29a-151">V prostředí Eclipse hello aplikace spusťte na fyzických Android zařízení nebo hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="cb29a-151">In Eclipse, run hello app on a physical Android device or hello emulator.</span></span>
3. <span data-ttu-id="cb29a-152">V hello aplikace pro Android uživatelského rozhraní, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="cb29a-152">In hello Android app UI, enter a username and password.</span></span> <span data-ttu-id="cb29a-153">Mohou to být libovolný řetězec, ale musí být hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cb29a-153">These can be any string, but they must be hello same value.</span></span>
4. <span data-ttu-id="cb29a-154">V hello aplikace pro Android uživatelského rozhraní, klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="cb29a-154">In hello Android app UI, click **Log in**.</span></span> <span data-ttu-id="cb29a-155">Pak klikněte na tlačítko **odeslat nabízené**.</span><span class="sxs-lookup"><span data-stu-id="cb29a-155">Then click **Send push**.</span></span>

