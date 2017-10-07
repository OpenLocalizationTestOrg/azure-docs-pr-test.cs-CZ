---
title: "Monitorované zóně aaaGeo nabízená oznámení pomocí Azure Notification Hubs a Bing Spatial Data | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak toodeliver na základě polohy nabízená oznámení pomocí Azure Notification Hubs a Bing Spatial Data."
services: notification-hubs
documentationcenter: windows
keywords: "nabízené oznámení,nabízená oznámení"
author: dend
manager: yuaxu
editor: dend
ms.assetid: f41beea1-0d62-4418-9ffc-c9d70607a1b7
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/31/2016
ms.author: dendeli
ms.openlocfilehash: d6efe473537f2159240c53e780741f12619c045d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Nabízená oznámení v monitorované geografické zóně s Azure Notification Hubs a Bing Spatial Data
> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).
> 
> 

V tomto kurzu se dozvíte, jak toodeliver na základě polohy nabízená oznámení pomocí Azure Notification Hubs a Bing Spatial Data využít z v rámci aplikace univerzální platformu Windows.

## <a name="prerequisites"></a>Požadavky
Především je třeba toomake, že máte všechny hello software a služby předpoklady:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) nebo novější (dostačující bude i [Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409)) 
* Nejnovější verzi hello [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Účet na webu Dev Center pro Mapy Bing](https://www.bingmapsportal.com/) (Je možné si jej vytvořit zdarma a přidružit si ho k účtu Microsoft.) 

## <a name="getting-started"></a>Začínáme
Začněme vytvořením projektu hello. V nástroji Visual Studio vytvořte nový projekt typu **Prázdná aplikace (univerzální pro Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Po dokončení vytváření projektu hello, měli byste mít základ hello hello aplikace. Nyní nastavme vše pro monitorovanou geografickou zónu hello. Vzhledem k tomu, že jsme se toouse probíhající, které Bing služby pro tento není veřejný koncový bod REST API, která umožňuje nám tooquery konkrétní oblasti lokality:

    http://spatial.virtualearth.net/REST/v1/data/

Budete potřebovat toospecify hello následující parametry tooget jeho:

* **ID zdroje dat** a **Název zdroje dat** – v rozhraní API Map Bing zdroje dat obsahují různá kategorizovaná metadata, například lokality a pracovní doby provozu. Můžete si o nich zde přečíst více. 
* **Název entity** – hello entity, které chcete použít toouse jako referenční bod pro oznámení hello. 
* **Klíč rozhraní API map Bing** – to je hello klíč, který jste dříve získali při vytváření účtu Dev Center pro Bing hello.

Umožňuje provést přímý informace o instalaci hello pro každou hello prvků uvedených výše.

## <a name="setting-up-hello-data-source"></a>Nastavení zdroje dat hello
Můžete provést v hello Dev Center pro mapy Bing. Klikněte jednoduše na **zdroje dat** v hello horním navigačním panelu a vyberte možnost **Správa zdrojů dat**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Pokud jste ještě nepracovali s rozhraními API map Bing, s největší pravděpodobností nebude existovat žádné zdroje dat existuje, takže vytvoříte na novou kliknutím na zdroj dat tooa nahrávání dat. Nezapomeňte že vyplnit všechna pole hello vyžaduje:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Budete možná se ptáte: co je hello datový soubor a co byste měli nahrávat? Pro účely hello tohoto testu můžeme jednoduše použít hello vzorku na základě kanálu, který vyznačenou oblast waterfront Brno hello:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

Hello výše představuje tuto entitu:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Jednoduše zkopírujte a vložte hello řetězec výše do nového souboru a uložte ho jako **NotificationHubsGeofence.pipe**a odešlete ji v hello Dev Center pro Bing.

> [!NOTE]
> Může být výzvami toospecify nový klíč pro hello **hlavní klíč** která se liší od hello **klíč dotazu**. Jednoduše vytvořte nový klíč přes hello řídicí panel a aktualizace hello data zdrojové odeslání stránky.
> 
> 

Jakmile nahrajete datový soubor hello, budete potřebovat toomake publikovat zdroj dat hello. 

Přejděte příliš**Správa zdrojů dat**, postupem uvedeným výše, najít zdroj dat v hello seznamu a klikněte na **publikovat** v hello **akce** sloupce. Ve chvíli byste měli vidět zdroje dat v hello **publikované zdroje dat** karty:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Pokud kliknete na tlačítko **upravit**, je budou moct toosee na první pohled umístění, ve kterých jsme do něj zahrnuli:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

V tomto okamžiku hello portál nezobrazuje hello hranice pro hello monitorové geografické zóny, který jsme vytvořili – potřebujeme pouze potvrzení, že zadané umístění hello je ve správném okolí hello je.

Nyní máte všechny požadavky hello zdroje dat hello. Klikněte na tlačítko tooget hello podrobnosti na adrese URL žádosti hello volání hello rozhraní API v hello Dev Center pro Bing Maps, **zdroje dat** a vyberte **informace o zdroji dat**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

Hello **URL dotazu** je co zde hledáme. Toto je hello koncový bod, oproti kterému můžeme spouštět dotazy toocheck zda hello zařízení je momentálně v rámci hranice hello umístění nebo ne. tooperform to zkontrolovat, potřebujeme jen tooexecute volání GET na adresu URL dotazu hello, s hello následující parametry:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Tímto způsobem určujete cílový bod, že získáme ze zařízení hello a mapy Bing automaticky provede výpočty toosee hello toho, jestli je v rámci hello monitorové geografické zóny. Po spuštění hello žádost přes prohlížeč (nebo adresu cURL), obdržíte standardní odpověď JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Tato odpověď se stane pouze, když hello se bod nachází v rámci hello určené hranice. Pokud se tam nenachází, obdržíte prázdný kontejner **výsledku**:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-hello-uwp-application"></a>Nastavení aplikace UWP hello
Teď, když máme připraven zdroj dat hello, můžeme začít pracovat na hello aplikaci UWP, který jsme připravili dříve.

Nejprve musíme pro naši aplikaci povolit zjišťování polohy. toodo tento, dvakrát klikněte na `Package.appxmanifest` souboru v **Průzkumníku řešení**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Na kartě vlastností balíčku hello která se právě otevřela, klikněte na **možnosti** a ujistěte se, že jste vybrali **umístění**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Jako možnost umístění hello je deklarovaná, vytvořte novou složku ve vašem řešení s názvem `Core`a přidat nový soubor v něm volat `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Hello `LocationHelper` vlastní třídy je v tuto chvíli vcelku jednoduchá – všechny dělá se nám umožňují tooobtain hello polohu uživatele přes systémové hello rozhraní API:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Můžete si přečíst další informace o získávání hello umístění uživatele v aplikacích pro UPW v oficiální hello [dokumentu MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

toocheck, který hello získávání polohy skutečně funguje, otevřete hello straně kód hlavní stránky (`MainPage.xaml.cs`). Vytvořte novou obslužnou rutinu události pro hello `Loaded` událost v hello `MainPage` konstruktor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Hello implementaci obslužné rutiny události hello vypadá takto:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Všimněte si, že jsme obslužnou rutinu hello deklarován jako asynchronní protože `GetCurrentLocation` může používat await a proto vyžaduje toobe provést v asynchronním kontextu. Navíc vzhledem k tomu, že za určitých okolností můžeme získat nulovou polohu (například jsou zakázány hello umístění služby nebo aplikace hello byl odepřen oprávnění tooaccess umístění), potřebujeme toomake jistotu, že je správné zpracování kontrolou hodnoty null.

Spusťte aplikaci hello. Nezapomeňte povolit přístup k poloze:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Jednou hello aplikace spustí, musí být schopný toosee hello souřadnic v hello **výstup** okno:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Teď víte, že získávání polohy funguje působí volné tooremove hello testovací obslužnou rutinu události Loaded, protože jsme nebude možné ji už nebudeme používat.

dalším krokem Hello je toocapture změny umístění. Pro tento, přejděte zpět toohello `LocationHelper` třídu a přidejte hello obslužné rutiny události pro `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Hello implementace zobrazí souřadnice polohy hello v hello **výstup** okno:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-hello-backend"></a>Nastavení back-end hello
Stáhnout hello [ukázku back-endu .NET z Githubu](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Po dokončení stahování hello otevřete hello `NotifyUsers` složku a následně – hello `NotifyUsers.sln` souboru.

Sada hello `AppBackend` projektu jako hello **spouštěný projekt** a spusťte jej.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Hello projektu už nakonfigurované toosend nabízená oznámení tootarget zařízení, proto budeme potřebovat toodo jen dvě věci – použít hello správný připojovací řetězec pro Centrum oznámení hello a přidejte hranici identifikace toosend hello oznámení pouze v případě hello Uživatel je v rámci hello monitorové geografické zóny.

tooconfigure hello připojovací řetězec, v hello `Models` otevřete složku `Notifications.cs`. Hello `NotificationHubClient.CreateClientFromConnectionString` funkce by měla obsahovat hello informace o Centru oznámení, které můžete získat v hello [portálu Azure](https://portal.azure.com) (podívejte se do hello **zásady přístupu** okno v  **Nastavení**). Uložte aktualizovaný konfigurační soubor hello.

Nyní potřebujeme toocreate model pro výsledek rozhraní API map Bing hello. Nejjednodušší způsob, jak toodo, který je klikněte pravým tlačítkem na hello Hello `Models` složky, **přidat** > **třída**. Pojmenujte ji `GeofenceBoundary.cs`. Po dokončení kopírování hello JSON z odpovědi hello rozhraní API, kterou jsme probírali v první části hello a v sadě Visual Studio pomocí **upravit** > **Vložit jinak** > **Vložit formát JSON jako třídy**. 

Tímto způsobem je zajištěno hello objekt k deserializaci přesně tak, jak jste zamýšleli. Výsledná sada tříd by měla vypadat přibližně takto:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Dále otevřete `Controllers` > `NotificationsController.cs`. Potřebujeme tootweak hello Post volání tooaccount pro hello cíl zeměpisné šířky a délky. K tomu jednoduše přidejte dva řetězce toohello funkce podpis – `latitude` a `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Vytvořte novou třídu v rámci hello projekt s názvem `ApiHelper.cs` – použijeme ji tooconnect tooBing toocheck bodu hranic průniků. Následujícím způsobem implementujte funkci `IsPointWithinBounds`:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

> [!NOTE]
> Ujistěte se, že koncový bod toosubstitute hello rozhraní API s hello URL dotazu, který jste dříve získali z hello Dev Center pro Bing (totéž platí klíč toohello rozhraní API). 
> 
> 

Pokud jsou výsledky dotazu toohello, to znamená, že hello Zadaný bod nachází v rámci hranice hello hello zóny, proto vrátíme `true`. Pokud nebyly nalezeny žádné výsledky, Bing nám oznamuje, že bod hello je mimo rámec vyhledávání hello, proto vrátíme `false`.

Zpět v `NotificationsController.cs`, vytvořte kontrolu přímo před příkazem switch hello:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Tímto způsobem hello oznámení je odeslán, pouze je-li bod hello v rámci hranice hello.

## <a name="testing-push-notifications-in-hello-uwp-app"></a>Testování nabízených oznámení v aplikaci UWP hello
Návratem toohello aplikace pro UPW bychom nyní měli být schopný tootest oznámení. V rámci hello `LocationHelper` třídy, vytvořte novou funkci – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

> [!NOTE]
> Swap – hello `POST_URL` toohello umístění nasazené webové aplikace, kterou jsme vytvořili v předchozí části hello. Nyní, je OK toorun ho místně, ale během nasazování veřejné verze, budete potřebovat toohost ho pomocí externího poskytovatele.
> 
> 

Nyní si jisti, že jsme registraci aplikace pro UPW hello nabízená oznámení. V sadě Visual Studio, klikněte na **projektu** > **ukládání** > **propojit aplikaci se hello store**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Jakmile se přihlásíte tooyour vývojářský účet, ujistěte se, vybrat existující aplikaci nebo vytvořte novou a přidružit hello balíčku. 

Přejděte toohello Dev Center a otevřete hello aplikaci, kterou jste právě vytvořili. Klikněte na **Služby** > **Nabízená oznámení** > **Web služeb Live Service**.

![](./media/notification-hubs-geofence/ms-live-services.png)

V lokalitě hello, poznamenejte si hello **tajný klíč aplikace** a hello **SID balíčku**. Budete potřebovat jak v hello portál Azure – otevřete své Centrum oznámení, klikněte na **nastavení** > **služby oznámení** > **Windows (WNS)**a zadejte informace hello hello požadované pole.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klikněte na **Uložit**.

V **Průzkumníkovi řešení** klikněte pravým tlačítkem na **Odkazy** a vyberte **Spravovat balíčky NuGet**. Je nutné zadat tooadd odkaz toohello **spravovanou knihovnu Microsoft Azure Service Bus** – jednoduše vyhledejte `WindowsAzure.Messaging.Managed` a přidejte ji tooyour projektu.

![](./media/notification-hubs-geofence/vs-nuget.png)

Pro účely testování můžeme vytvořit hello `MainPage_Loaded` obslužné rutiny události ještě jednou a přidejte tento tooit fragment kódu:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays hello registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Hello výše zaregistruje aplikaci hello hello centra oznámení. Jste připravené toogo! 

V `LocationHelper`, uvnitř hello `Geolocator_PositionChanged` obslužnou rutinu, můžete přidat testovací kód, který bod nuceně umístí hello umístění uvnitř monitorové geografické zóny hello:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Protože jsme nepředáváme skutečné souřadnice hello, (které nemusí být v rámci hranice hello momentálně hello) a používají předem definované testovací hodnoty, jsme se zobrazí oznámení zobrazí na aktualizace:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a>Co dále?
Existuje několik kroků, které můžete potřebovat toofollow v přidání toohello výše toomake se, že je řešení hello produkční prostředí.

Především bude pravděpodobně nutné tooensure monitorovaná geografická zóna je dynamická. To bude vyžadovat další práci s hello rozhraní API Bing v pořadí toobe možné tooupload nové hranice do existujícího zdroje dat hello. Poraďte se hello [Bing Spatial Data Services API dokumentaci](https://msdn.microsoft.com/library/ff701734.aspx) Další informace o subjektu hello.

Druhý, jako jste pracovní tooensure že hello doručuje správným účastníkům toohello, můžete chtít tootarget je prostřednictvím [označování](notification-hubs-tags-segment-push-message.md).

Hello řešení uvedené výše popisuje scénář, ve kterém můžete mít širokou škálu cílových platforem, takže jsme nebude omezen hello monitorování geografických zón toosystem specifické možnosti. Ale nutné dodat, hello univerzální platforma Windows nabízí možnosti příliš[zjistit monitorovaná geografická zóna právo out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Další podrobnosti týkající se schopností Notification Hubs najdete na [portálu dokumentace](https://azure.microsoft.com/documentation/services/notification-hubs/).

