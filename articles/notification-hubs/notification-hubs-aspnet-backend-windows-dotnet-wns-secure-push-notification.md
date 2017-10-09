---
title: "aaaAzure oznámení Centra zabezpečení Push."
description: "Zjistěte, jak toosend zabezpečené nabízená oznámení v Azure. Ukázky kódu jsou vytvořené v C# s použitím hello .NET API."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: b6fe16c96d28d75ff698278409fb012472ba6396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs zabezpečené Push
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Podpora nabízená oznámení v Microsoft Azure umožňuje tooaccess nabízené snadno použitelnou, multiplatformní a škálovanou infrastrukturu, což výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.

Z důvodu omezení tooregulatory nebo zabezpečení někdy aplikace může být vhodné tooinclude něco v hello oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro hello standardní nabízená oznámení. Tento kurz popisuje, jak tooachieve hello stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi hello klientského zařízení a back-end aplikace hello.

Na vysoké úrovni tok hello vypadá takto:

1. back-end Hello aplikace:
   * Zabezpečení datové úložiště v databázi back-end.
   * Odešle hello ID tohoto zařízení toohello oznámení (zabezpečené nebudou odeslány žádné informace).
2. aplikace Hello na hello zařízení při přijetí oznámení hello:
   * Hello zařízení kontaktuje hello back-end žádajícího hello zabezpečené datové části.
   * Hello aplikace můžete zobrazit datové části hello jako upozornění na hello zařízení.

Je důležité, že toonote, v předchozím toku hello (a v tomto kurzu) předpokládáme, že hello zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele hello. Zaručí se tím úplně jednoduché prostředí, protože hello zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové hello oznámení. Pokud vaše aplikace nejsou uložené ověřovací tokeny na hello zařízení nebo pokud tyto tokeny můžete vypršela platnost, hello aplikaci zařízení při přijetí oznámení hello by měl zobrazit obecné oznámení výzvy hello uživatele toolaunch hello aplikace. aplikace Hello pak ověřuje uživatele hello a ukazuje datová část oznámení hello.

Tento kurz zabezpečení nabízené ukazuje, jak toosend nabízených oznámení bezpečně. Hello kurzu vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) kurzu, a proto hello kroky musí dokončit v tomto kurzu první.

> [!NOTE]
> V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (pro Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Všimněte si také, že Windows Phone 8.1 vyžaduje pověření systému Windows (ne Windows Phone), a že úlohy na pozadí nefungují na Windows Phone 8.0 nebo Silverlight 8.1. Pro aplikace pro Windows Store, můžete přijímat oznámení prostřednictvím úlohy na pozadí pouze v případě, že je povoleno uzamčení obrazovky aplikace hello (klikněte na zaškrtávací políčko hello v hello Appmanifest).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-windows-phone-project"></a>Upravit hello projektu Windows Phone
1. V hello **NotifyUserWindowsPhone** projekt, přidejte následující kód tooApp.xaml.cs tooregister hello nabízené pozadí úloh hello. Přidejte následující řádek kódu na konci hello hello hello `OnLaunched()` metoda:
   
        RegisterBackgroundTask();
2. Stále v souboru App.xaml.cs přidejte následující kód bezprostředně po hello hello `OnLaunched()` metoda:
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. Přidejte následující hello `using` příkazy hello horní části souboru App.xaml.cs hello:
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. Z hello **soubor** nabídky v sadě Visual Studio, klikněte na tlačítko **Uložit vše**.

## <a name="create-hello-push-background-component"></a>Vytvoření hello Push pozadí součásti
dalším krokem Hello je toocreate hello nabízené pozadí součásti.

1. V Průzkumníku řešení klikněte pravým tlačítkem na uzel nejvyšší úrovně hello hello řešení (**řešení SecurePush** v tomto případě), pak klikněte na tlačítko **přidat**, pak klikněte na tlačítko **nový projekt**.
2. Rozbalte položku **aplikacích pro Store**, pak klikněte na tlačítko **aplikace Windows Phone**, pak klikněte na tlačítko **komponenty prostředí Windows Runtime (Windows Phone)**. Název projektu hello **PushBackgroundComponent**a potom klikněte na **OK** toocreate hello projektu.
   
    ![][12]
3. V Průzkumníku řešení klikněte pravým tlačítkem na hello **PushBackgroundComponent (Windows Phone 8.1)** projektu a pak klikněte na **přidat**, pak klikněte na tlačítko **třída**. Pojmenujte novou třídu hello **PushBackgroundTask.cs**. Klikněte na tlačítko **přidat** toogenerate hello třídy.
4. Nahraďte hello celý obsah hello **PushBackgroundComponent** definici oboru názvů s hello následující kód, nahraďte zástupný symbol hello `{back-end endpoint}` s koncovým bodem back-end hello získali při nasazení vaší back-end:
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store hello content received from hello notification so it can be retrieved from hello UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. V Průzkumníku řešení klikněte pravým tlačítkem na hello **PushBackgroundComponent (Windows Phone 8.1)** projektu a pak klikněte na **spravovat balíčky NuGet**.
6. Na levé straně hello, klikněte na tlačítko **Online**.
7. V hello **vyhledávání** zadejte **klienta Http**.
8. V seznamu výsledků hello, klikněte na tlačítko **knihovny klienta HTTP Microsoft**a potom klikněte na **nainstalovat**. Dokončení instalace hello.
9. Zpět v hello NuGet **vyhledávání** zadejte **Json.net**. Nainstalujte hello **Json.NET** balíček a potom hello zavřít okno Správce balíčků NuGet.
10. Přidejte následující hello `using` příkazy hello horní části hello **PushBackgroundTask.cs** souboru:
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. V Průzkumníku řešení v hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projektu, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **přidat odkaz na...** . V dialogovém okně hello správce odkazů, políčko hello vedle příliš**PushBackgroundComponent**a potom klikněte na **OK**.
12. V Průzkumníku řešení klikněte dvakrát na **Package.appxmanifest** v hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projektu. V části **oznámení**, nastavte **informační podporující** příliš**Ano**.
    
    ![][3]
13. Pořád ještě v **Package.appxmanifest**, klikněte na tlačítko hello **deklarace** nabídce v horní hello. V hello **dostupné deklarace** rozevíracího seznamu, klikněte na tlačítko **úlohy na pozadí**a potom klikněte na **přidat**.
14. V **Package.appxmanifest**v části **vlastnosti**, zkontrolujte **nabízená oznámení**.
15. V **Package.appxmanifest**v části **nastavení aplikace**, typ **PushBackgroundComponent.PushBackgroundTask** v hello **vstupní bod** pole.
    
    ![][13]
16. Z hello **soubor** nabídky, klikněte na tlačítko **Uložit vše**.

## <a name="run-hello-application"></a>Spustit hello aplikace
toorun hello aplikace, hello následující:

1. V sadě Visual Studio spustit hello **AppBackend** aplikace webového rozhraní API. Zobrazí se webová stránka ASP.NET.
2. V sadě Visual Studio spustit hello **NotifyUserWindowsPhone (Windows Phone 8.1)** aplikace Windows Phone. emulátor Windows Phone Hello spustí a automaticky načte aplikace hello.
3. V hello **NotifyUserWindowsPhone** aplikace uživatelského rozhraní, zadejte uživatelské jméno a heslo. Mohou to být libovolný řetězec, ale musí být hello stejnou hodnotu.
4. V hello **NotifyUserWindowsPhone** aplikace uživatelského rozhraní, klikněte na tlačítko **přihlášení a registrace**. Pak klikněte na tlačítko **odeslat nabízené**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
