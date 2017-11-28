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
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a><span data-ttu-id="a53cc-104">Nabízená oznámení v monitorované geografické zóně s Azure Notification Hubs a Bing Spatial Data</span><span class="sxs-lookup"><span data-stu-id="a53cc-104">Geo-fenced push notifications with Azure Notification Hubs and Bing Spatial Data</span></span>
> [!NOTE]
> <span data-ttu-id="a53cc-105">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a53cc-105">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="a53cc-106">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="a53cc-106">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a53cc-107">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span><span class="sxs-lookup"><span data-stu-id="a53cc-107">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span></span>
> 
> 

<span data-ttu-id="a53cc-108">V tomto kurzu se dozvíte, jak toodeliver na základě polohy nabízená oznámení pomocí Azure Notification Hubs a Bing Spatial Data využít z v rámci aplikace univerzální platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="a53cc-108">In this tutorial, you will learn how toodeliver location-based push notifications with Azure Notification Hubs and Bing Spatial Data, leveraged from within a Universal Windows Platform application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a53cc-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a53cc-109">Prerequisites</span></span>
<span data-ttu-id="a53cc-110">Především je třeba toomake, že máte všechny hello software a služby předpoklady:</span><span class="sxs-lookup"><span data-stu-id="a53cc-110">First and foremost, you need toomake sure that you have all hello software and service pre-requisites:</span></span>

* <span data-ttu-id="a53cc-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) nebo novější (dostačující bude i [Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409))</span><span class="sxs-lookup"><span data-stu-id="a53cc-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) or later ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) will do as well).</span></span> 
* <span data-ttu-id="a53cc-112">Nejnovější verzi hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a53cc-112">Latest version of hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="a53cc-113">[Účet na webu Dev Center pro Mapy Bing](https://www.bingmapsportal.com/) (Je možné si jej vytvořit zdarma a přidružit si ho k účtu Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="a53cc-113">[Bing Maps Dev Center account](https://www.bingmapsportal.com/) (you can create one for free and associate it with your Microsoft account).</span></span> 

## <a name="getting-started"></a><span data-ttu-id="a53cc-114">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a53cc-114">Getting Started</span></span>
<span data-ttu-id="a53cc-115">Začněme vytvořením projektu hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-115">Let’s start by creating hello project.</span></span> <span data-ttu-id="a53cc-116">V nástroji Visual Studio vytvořte nový projekt typu **Prázdná aplikace (univerzální pro Windows)**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-116">In Visual Studio, start a new project of type **Blank App (Universal Windows)**.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

<span data-ttu-id="a53cc-117">Po dokončení vytváření projektu hello, měli byste mít základ hello hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a53cc-117">Once hello project creation is complete, you should have hello harness for hello app itself.</span></span> <span data-ttu-id="a53cc-118">Nyní nastavme vše pro monitorovanou geografickou zónu hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-118">Now let’s set up everything for hello geo-fencing infrastructure.</span></span> <span data-ttu-id="a53cc-119">Vzhledem k tomu, že jsme se toouse probíhající, které Bing služby pro tento není veřejný koncový bod REST API, která umožňuje nám tooquery konkrétní oblasti lokality:</span><span class="sxs-lookup"><span data-stu-id="a53cc-119">Because we are going toouse Bing services for this, there is a public REST API endpoint that allows us tooquery specific location frames:</span></span>

    http://spatial.virtualearth.net/REST/v1/data/

<span data-ttu-id="a53cc-120">Budete potřebovat toospecify hello následující parametry tooget jeho:</span><span class="sxs-lookup"><span data-stu-id="a53cc-120">You will need toospecify hello following parameters tooget it working:</span></span>

* <span data-ttu-id="a53cc-121">**ID zdroje dat** a **Název zdroje dat** – v rozhraní API Map Bing zdroje dat obsahují různá kategorizovaná metadata, například lokality a pracovní doby provozu.</span><span class="sxs-lookup"><span data-stu-id="a53cc-121">**Data Source ID** and **Data Source Name** – in Bing Maps API, data sources contain various bucketed metadata, such as locations and business hours of operation.</span></span> <span data-ttu-id="a53cc-122">Můžete si o nich zde přečíst více.</span><span class="sxs-lookup"><span data-stu-id="a53cc-122">You can read more about those here.</span></span> 
* <span data-ttu-id="a53cc-123">**Název entity** – hello entity, které chcete použít toouse jako referenční bod pro oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-123">**Entity Name** – hello entity you want toouse as a reference point for hello notification.</span></span> 
* <span data-ttu-id="a53cc-124">**Klíč rozhraní API map Bing** – to je hello klíč, který jste dříve získali při vytváření účtu Dev Center pro Bing hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-124">**Bing Maps API Key** – this is hello key that you obtained earlier when you created hello Bing Dev Center account.</span></span>

<span data-ttu-id="a53cc-125">Umožňuje provést přímý informace o instalaci hello pro každou hello prvků uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="a53cc-125">Let’s do a deep-dive on hello setup for each of hello elements above.</span></span>

## <a name="setting-up-hello-data-source"></a><span data-ttu-id="a53cc-126">Nastavení zdroje dat hello</span><span class="sxs-lookup"><span data-stu-id="a53cc-126">Setting up hello data source</span></span>
<span data-ttu-id="a53cc-127">Můžete provést v hello Dev Center pro mapy Bing.</span><span class="sxs-lookup"><span data-stu-id="a53cc-127">You can do it in hello Bing Maps Dev Center.</span></span> <span data-ttu-id="a53cc-128">Klikněte jednoduše na **zdroje dat** v hello horním navigačním panelu a vyberte možnost **Správa zdrojů dat**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-128">Simply click on **Data sources** in hello top navigation bar and select **Manage Data Sources**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

<span data-ttu-id="a53cc-129">Pokud jste ještě nepracovali s rozhraními API map Bing, s největší pravděpodobností nebude existovat žádné zdroje dat existuje, takže vytvoříte na novou kliknutím na zdroj dat tooa nahrávání dat.</span><span class="sxs-lookup"><span data-stu-id="a53cc-129">If you have not worked with Bing Maps API before, most likely there won’t be any data sources present, so you can just create a new one by clicking on Upload data tooa data source.</span></span> <span data-ttu-id="a53cc-130">Nezapomeňte že vyplnit všechna pole hello vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="a53cc-130">Make sure you fill out all hello required fields:</span></span>

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

<span data-ttu-id="a53cc-131">Budete možná se ptáte: co je hello datový soubor a co byste měli nahrávat?</span><span class="sxs-lookup"><span data-stu-id="a53cc-131">You might be wondering – what is hello data file and what should you be uploading?</span></span> <span data-ttu-id="a53cc-132">Pro účely hello tohoto testu můžeme jednoduše použít hello vzorku na základě kanálu, který vyznačenou oblast waterfront Brno hello:</span><span class="sxs-lookup"><span data-stu-id="a53cc-132">For hello purposes of this test, we can just use hello sample pipe-based that frames an area of hello San Francisco waterfront:</span></span>

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

<span data-ttu-id="a53cc-133">Hello výše představuje tuto entitu:</span><span class="sxs-lookup"><span data-stu-id="a53cc-133">hello above represents this entity:</span></span>

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

<span data-ttu-id="a53cc-134">Jednoduše zkopírujte a vložte hello řetězec výše do nového souboru a uložte ho jako **NotificationHubsGeofence.pipe**a odešlete ji v hello Dev Center pro Bing.</span><span class="sxs-lookup"><span data-stu-id="a53cc-134">Simply copy and paste hello string above into a new file and save it as **NotificationHubsGeofence.pipe**, and upload it in hello Bing Dev Center.</span></span>

> [!NOTE]
> <span data-ttu-id="a53cc-135">Může být výzvami toospecify nový klíč pro hello **hlavní klíč** která se liší od hello **klíč dotazu**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-135">You might be prompted toospecify a new key for hello **Master Key** that is different from hello **Query Key**.</span></span> <span data-ttu-id="a53cc-136">Jednoduše vytvořte nový klíč přes hello řídicí panel a aktualizace hello data zdrojové odeslání stránky.</span><span class="sxs-lookup"><span data-stu-id="a53cc-136">Simply create a new key through hello dashboard and refresh hello data source upload page.</span></span>
> 
> 

<span data-ttu-id="a53cc-137">Jakmile nahrajete datový soubor hello, budete potřebovat toomake publikovat zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-137">Once you upload hello data file, you will need toomake sure that you publish hello data source.</span></span> 

<span data-ttu-id="a53cc-138">Přejděte příliš**Správa zdrojů dat**, postupem uvedeným výše, najít zdroj dat v hello seznamu a klikněte na **publikovat** v hello **akce** sloupce.</span><span class="sxs-lookup"><span data-stu-id="a53cc-138">Go too**Manage Data Sources**, just like we did above, find your data source in hello list and click on **Publish** in hello **Actions** column.</span></span> <span data-ttu-id="a53cc-139">Ve chvíli byste měli vidět zdroje dat v hello **publikované zdroje dat** karty:</span><span class="sxs-lookup"><span data-stu-id="a53cc-139">In a bit, you should see your data source in hello **Published Data Sources** tab:</span></span>

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

<span data-ttu-id="a53cc-140">Pokud kliknete na tlačítko **upravit**, je budou moct toosee na první pohled umístění, ve kterých jsme do něj zahrnuli:</span><span class="sxs-lookup"><span data-stu-id="a53cc-140">If you click **Edit**, you will be able toosee at a glance what locations we introduced in it:</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

<span data-ttu-id="a53cc-141">V tomto okamžiku hello portál nezobrazuje hello hranice pro hello monitorové geografické zóny, který jsme vytvořili – potřebujeme pouze potvrzení, že zadané umístění hello je ve správném okolí hello je.</span><span class="sxs-lookup"><span data-stu-id="a53cc-141">At this point, hello portal does not show you hello boundaries for hello geofence that we created – all we need is a confirmation that hello location specified is in hello right vicinity.</span></span>

<span data-ttu-id="a53cc-142">Nyní máte všechny požadavky hello zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-142">Now you have all hello requirements for hello data source.</span></span> <span data-ttu-id="a53cc-143">Klikněte na tlačítko tooget hello podrobnosti na adrese URL žádosti hello volání hello rozhraní API v hello Dev Center pro Bing Maps, **zdroje dat** a vyberte **informace o zdroji dat**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-143">tooget hello details on hello request URL for hello API call, in hello Bing Maps Dev Center, click **Data sources** and select **Data Source Information**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

<span data-ttu-id="a53cc-144">Hello **URL dotazu** je co zde hledáme.</span><span class="sxs-lookup"><span data-stu-id="a53cc-144">hello **Query URL** is what we’re after here.</span></span> <span data-ttu-id="a53cc-145">Toto je hello koncový bod, oproti kterému můžeme spouštět dotazy toocheck zda hello zařízení je momentálně v rámci hranice hello umístění nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a53cc-145">This is hello endpoint against which we can execute queries toocheck whether hello device is currently within hello boundaries of a location or not.</span></span> <span data-ttu-id="a53cc-146">tooperform to zkontrolovat, potřebujeme jen tooexecute volání GET na adresu URL dotazu hello, s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="a53cc-146">tooperform this check, we simply need tooexecute a GET call against hello query URL, with hello following parameters appended:</span></span>

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

<span data-ttu-id="a53cc-147">Tímto způsobem určujete cílový bod, že získáme ze zařízení hello a mapy Bing automaticky provede výpočty toosee hello toho, jestli je v rámci hello monitorové geografické zóny.</span><span class="sxs-lookup"><span data-stu-id="a53cc-147">That way you're specifying a target point that we obtain from hello device and Bing Maps will automatically perform hello calculations toosee whether it is within hello geofence.</span></span> <span data-ttu-id="a53cc-148">Po spuštění hello žádost přes prohlížeč (nebo adresu cURL), obdržíte standardní odpověď JSON:</span><span class="sxs-lookup"><span data-stu-id="a53cc-148">Once you execute hello request through a browser (or cURL), you will get standard a JSON response:</span></span>

![](./media/notification-hubs-geofence/bing-maps-json.png)

<span data-ttu-id="a53cc-149">Tato odpověď se stane pouze, když hello se bod nachází v rámci hello určené hranice.</span><span class="sxs-lookup"><span data-stu-id="a53cc-149">This response only happens when hello point is actually within hello designated boundaries.</span></span> <span data-ttu-id="a53cc-150">Pokud se tam nenachází, obdržíte prázdný kontejner **výsledku**:</span><span class="sxs-lookup"><span data-stu-id="a53cc-150">If it is not, you will get an empty **results** bucket:</span></span>

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-hello-uwp-application"></a><span data-ttu-id="a53cc-151">Nastavení aplikace UWP hello</span><span class="sxs-lookup"><span data-stu-id="a53cc-151">Setting up hello UWP application</span></span>
<span data-ttu-id="a53cc-152">Teď, když máme připraven zdroj dat hello, můžeme začít pracovat na hello aplikaci UWP, který jsme připravili dříve.</span><span class="sxs-lookup"><span data-stu-id="a53cc-152">Now that we have hello data source ready, we can start working on hello UWP application that we bootstrapped earlier.</span></span>

<span data-ttu-id="a53cc-153">Nejprve musíme pro naši aplikaci povolit zjišťování polohy.</span><span class="sxs-lookup"><span data-stu-id="a53cc-153">First and foremost, we must enable location services for our application.</span></span> <span data-ttu-id="a53cc-154">toodo tento, dvakrát klikněte na `Package.appxmanifest` souboru v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-154">toodo this, double-click on `Package.appxmanifest` file in **Solution Explorer**.</span></span>

![](./media/notification-hubs-geofence/vs-package-manifest.png)

<span data-ttu-id="a53cc-155">Na kartě vlastností balíčku hello která se právě otevřela, klikněte na **možnosti** a ujistěte se, že jste vybrali **umístění**:</span><span class="sxs-lookup"><span data-stu-id="a53cc-155">In hello package properties tab that just opened, click on **Capabilities** and make sure that you select **Location**:</span></span>

![](./media/notification-hubs-geofence/vs-package-location.png)

<span data-ttu-id="a53cc-156">Jako možnost umístění hello je deklarovaná, vytvořte novou složku ve vašem řešení s názvem `Core`a přidat nový soubor v něm volat `LocationHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="a53cc-156">As hello location capability is declared, create a new folder in your solution named `Core`, and add a new file within it, called `LocationHelper.cs`:</span></span>

![](./media/notification-hubs-geofence/vs-location-helper.png)

<span data-ttu-id="a53cc-157">Hello `LocationHelper` vlastní třídy je v tuto chvíli vcelku jednoduchá – všechny dělá se nám umožňují tooobtain hello polohu uživatele přes systémové hello rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="a53cc-157">hello `LocationHelper` class itself is fairly basic at this point – all it does is allow us tooobtain hello user location through hello system API:</span></span>

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

<span data-ttu-id="a53cc-158">Můžete si přečíst další informace o získávání hello umístění uživatele v aplikacích pro UPW v oficiální hello [dokumentu MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span><span class="sxs-lookup"><span data-stu-id="a53cc-158">You can read more about getting hello user’s location in UWP apps in hello official [MSDN document](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span></span>

<span data-ttu-id="a53cc-159">toocheck, který hello získávání polohy skutečně funguje, otevřete hello straně kód hlavní stránky (`MainPage.xaml.cs`).</span><span class="sxs-lookup"><span data-stu-id="a53cc-159">toocheck that hello location acquisition is actually working, open hello code side of your main page (`MainPage.xaml.cs`).</span></span> <span data-ttu-id="a53cc-160">Vytvořte novou obslužnou rutinu události pro hello `Loaded` událost v hello `MainPage` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="a53cc-160">Create a new event handler for hello `Loaded` event in hello `MainPage` constructor:</span></span>

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

<span data-ttu-id="a53cc-161">Hello implementaci obslužné rutiny události hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a53cc-161">hello implementation of hello event handler is as follows:</span></span>

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

<span data-ttu-id="a53cc-162">Všimněte si, že jsme obslužnou rutinu hello deklarován jako asynchronní protože `GetCurrentLocation` může používat await a proto vyžaduje toobe provést v asynchronním kontextu.</span><span class="sxs-lookup"><span data-stu-id="a53cc-162">Notice that we declared hello handler as async because `GetCurrentLocation` is awaitable, and therefore requires toobe executed in an async context.</span></span> <span data-ttu-id="a53cc-163">Navíc vzhledem k tomu, že za určitých okolností můžeme získat nulovou polohu (například jsou zakázány hello umístění služby nebo aplikace hello byl odepřen oprávnění tooaccess umístění), potřebujeme toomake jistotu, že je správné zpracování kontrolou hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="a53cc-163">Also, because under certain circumstances we might end up with a null location (e.g. hello location services are disabled or hello application was denied permissions tooaccess location), we need toomake sure that it is properly handled with a null check.</span></span>

<span data-ttu-id="a53cc-164">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-164">Run hello application.</span></span> <span data-ttu-id="a53cc-165">Nezapomeňte povolit přístup k poloze:</span><span class="sxs-lookup"><span data-stu-id="a53cc-165">Make sure you allow location access:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

<span data-ttu-id="a53cc-166">Jednou hello aplikace spustí, musí být schopný toosee hello souřadnic v hello **výstup** okno:</span><span class="sxs-lookup"><span data-stu-id="a53cc-166">Once hello application launches, you should be able toosee hello coordinates in hello **Output** window:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

<span data-ttu-id="a53cc-167">Teď víte, že získávání polohy funguje působí volné tooremove hello testovací obslužnou rutinu události Loaded, protože jsme nebude možné ji už nebudeme používat.</span><span class="sxs-lookup"><span data-stu-id="a53cc-167">Now you know that location acquisition works – feel free tooremove hello test event handler for Loaded because we won’t be using it anymore.</span></span>

<span data-ttu-id="a53cc-168">dalším krokem Hello je toocapture změny umístění.</span><span class="sxs-lookup"><span data-stu-id="a53cc-168">hello next step is toocapture location changes.</span></span> <span data-ttu-id="a53cc-169">Pro tento, přejděte zpět toohello `LocationHelper` třídu a přidejte hello obslužné rutiny události pro `PositionChanged`:</span><span class="sxs-lookup"><span data-stu-id="a53cc-169">For that, let’s go back toohello `LocationHelper` class and add hello event handler for `PositionChanged`:</span></span>

    geolocator.PositionChanged += Geolocator_PositionChanged;

<span data-ttu-id="a53cc-170">Hello implementace zobrazí souřadnice polohy hello v hello **výstup** okno:</span><span class="sxs-lookup"><span data-stu-id="a53cc-170">hello implementation will show hello location coordinates in hello **Output** window:</span></span>

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-hello-backend"></a><span data-ttu-id="a53cc-171">Nastavení back-end hello</span><span class="sxs-lookup"><span data-stu-id="a53cc-171">Setting up hello backend</span></span>
<span data-ttu-id="a53cc-172">Stáhnout hello [ukázku back-endu .NET z Githubu](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="a53cc-172">Download hello [.NET Backend Sample from GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> <span data-ttu-id="a53cc-173">Po dokončení stahování hello otevřete hello `NotifyUsers` složku a následně – hello `NotifyUsers.sln` souboru.</span><span class="sxs-lookup"><span data-stu-id="a53cc-173">Once hello download completes, open hello `NotifyUsers` folder, and subsequently – hello `NotifyUsers.sln` file.</span></span>

<span data-ttu-id="a53cc-174">Sada hello `AppBackend` projektu jako hello **spouštěný projekt** a spusťte jej.</span><span class="sxs-lookup"><span data-stu-id="a53cc-174">Set hello `AppBackend` project as hello **StartUp Project** and launch it.</span></span>

![](./media/notification-hubs-geofence/vs-startup-project.png)

<span data-ttu-id="a53cc-175">Hello projektu už nakonfigurované toosend nabízená oznámení tootarget zařízení, proto budeme potřebovat toodo jen dvě věci – použít hello správný připojovací řetězec pro Centrum oznámení hello a přidejte hranici identifikace toosend hello oznámení pouze v případě hello Uživatel je v rámci hello monitorové geografické zóny.</span><span class="sxs-lookup"><span data-stu-id="a53cc-175">hello project is already configured toosend push notifications tootarget devices, so we’ll need toodo only two things – swap out hello right connection string for hello notification hub and add boundary identification toosend hello notification only when hello user is within hello geofence.</span></span>

<span data-ttu-id="a53cc-176">tooconfigure hello připojovací řetězec, v hello `Models` otevřete složku `Notifications.cs`.</span><span class="sxs-lookup"><span data-stu-id="a53cc-176">tooconfigure hello connection string, in hello `Models` folder open `Notifications.cs`.</span></span> <span data-ttu-id="a53cc-177">Hello `NotificationHubClient.CreateClientFromConnectionString` funkce by měla obsahovat hello informace o Centru oznámení, které můžete získat v hello [portálu Azure](https://portal.azure.com) (podívejte se do hello **zásady přístupu** okno v  **Nastavení**).</span><span class="sxs-lookup"><span data-stu-id="a53cc-177">hello `NotificationHubClient.CreateClientFromConnectionString` function should contain hello information about your notification hub that you can get in hello [Azure Portal](https://portal.azure.com) (look inside hello **Access Policies** blade in **Settings**).</span></span> <span data-ttu-id="a53cc-178">Uložte aktualizovaný konfigurační soubor hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-178">Save hello updated configuration file.</span></span>

<span data-ttu-id="a53cc-179">Nyní potřebujeme toocreate model pro výsledek rozhraní API map Bing hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-179">Now we need toocreate a model for hello Bing Maps API result.</span></span> <span data-ttu-id="a53cc-180">Nejjednodušší způsob, jak toodo, který je klikněte pravým tlačítkem na hello Hello `Models` složky, **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-180">hello easiest way toodo that is right-click on hello `Models` folder, **Add** > **Class**.</span></span> <span data-ttu-id="a53cc-181">Pojmenujte ji `GeofenceBoundary.cs`.</span><span class="sxs-lookup"><span data-stu-id="a53cc-181">Name it `GeofenceBoundary.cs`.</span></span> <span data-ttu-id="a53cc-182">Po dokončení kopírování hello JSON z odpovědi hello rozhraní API, kterou jsme probírali v první části hello a v sadě Visual Studio pomocí **upravit** > **Vložit jinak** > **Vložit formát JSON jako třídy**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-182">Once done, copy hello JSON from hello API response that we discussed in hello first section and in Visual Studio use **Edit** > **Paste Special** > **Paste JSON as Classes**.</span></span> 

<span data-ttu-id="a53cc-183">Tímto způsobem je zajištěno hello objekt k deserializaci přesně tak, jak jste zamýšleli.</span><span class="sxs-lookup"><span data-stu-id="a53cc-183">That way we ensure that hello object will be deserialized exactly as it was intended.</span></span> <span data-ttu-id="a53cc-184">Výsledná sada tříd by měla vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="a53cc-184">Your resulting class set should resemble this:</span></span>

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

<span data-ttu-id="a53cc-185">Dále otevřete `Controllers` > `NotificationsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="a53cc-185">Next, open `Controllers` > `NotificationsController.cs`.</span></span> <span data-ttu-id="a53cc-186">Potřebujeme tootweak hello Post volání tooaccount pro hello cíl zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="a53cc-186">We need tootweak hello Post call tooaccount for hello target longitude and latitude.</span></span> <span data-ttu-id="a53cc-187">K tomu jednoduše přidejte dva řetězce toohello funkce podpis – `latitude` a `longitude`.</span><span class="sxs-lookup"><span data-stu-id="a53cc-187">For that, simply add two strings toohello function signature – `latitude` and `longitude`.</span></span>

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

<span data-ttu-id="a53cc-188">Vytvořte novou třídu v rámci hello projekt s názvem `ApiHelper.cs` – použijeme ji tooconnect tooBing toocheck bodu hranic průniků.</span><span class="sxs-lookup"><span data-stu-id="a53cc-188">Create a new class within hello project called `ApiHelper.cs` – we’ll use it tooconnect tooBing toocheck point boundary intersections.</span></span> <span data-ttu-id="a53cc-189">Následujícím způsobem implementujte funkci `IsPointWithinBounds`:</span><span class="sxs-lookup"><span data-stu-id="a53cc-189">Implement a `IsPointWithinBounds` function, like this:</span></span>

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
> <span data-ttu-id="a53cc-190">Ujistěte se, že koncový bod toosubstitute hello rozhraní API s hello URL dotazu, který jste dříve získali z hello Dev Center pro Bing (totéž platí klíč toohello rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="a53cc-190">Make sure toosubstitute hello API endpoint with hello query URL that you obtained earlier from hello Bing Dev Center (same applies toohello API key).</span></span> 
> 
> 

<span data-ttu-id="a53cc-191">Pokud jsou výsledky dotazu toohello, to znamená, že hello Zadaný bod nachází v rámci hranice hello hello zóny, proto vrátíme `true`.</span><span class="sxs-lookup"><span data-stu-id="a53cc-191">If there are results toohello query, that means that hello specified point is within hello boundaries of hello geofence, so we return `true`.</span></span> <span data-ttu-id="a53cc-192">Pokud nebyly nalezeny žádné výsledky, Bing nám oznamuje, že bod hello je mimo rámec vyhledávání hello, proto vrátíme `false`.</span><span class="sxs-lookup"><span data-stu-id="a53cc-192">If there are no results, Bing is telling us that hello point is outside hello lookup frame, so we return `false`.</span></span>

<span data-ttu-id="a53cc-193">Zpět v `NotificationsController.cs`, vytvořte kontrolu přímo před příkazem switch hello:</span><span class="sxs-lookup"><span data-stu-id="a53cc-193">Back in `NotificationsController.cs`, create a check right before hello switch statement:</span></span>

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

<span data-ttu-id="a53cc-194">Tímto způsobem hello oznámení je odeslán, pouze je-li bod hello v rámci hranice hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-194">That way, hello notification is only sent when hello point is within hello boundaries.</span></span>

## <a name="testing-push-notifications-in-hello-uwp-app"></a><span data-ttu-id="a53cc-195">Testování nabízených oznámení v aplikaci UWP hello</span><span class="sxs-lookup"><span data-stu-id="a53cc-195">Testing push notifications in hello UWP app</span></span>
<span data-ttu-id="a53cc-196">Návratem toohello aplikace pro UPW bychom nyní měli být schopný tootest oznámení.</span><span class="sxs-lookup"><span data-stu-id="a53cc-196">Going back toohello UWP app, we should now be able tootest notifications.</span></span> <span data-ttu-id="a53cc-197">V rámci hello `LocationHelper` třídy, vytvořte novou funkci – `SendLocationToBackend`:</span><span class="sxs-lookup"><span data-stu-id="a53cc-197">Within hello `LocationHelper` class, create a new function – `SendLocationToBackend`:</span></span>

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
> <span data-ttu-id="a53cc-198">Swap – hello `POST_URL` toohello umístění nasazené webové aplikace, kterou jsme vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-198">Swap hello `POST_URL` toohello location of your deployed web application that we created in hello previous section.</span></span> <span data-ttu-id="a53cc-199">Nyní, je OK toorun ho místně, ale během nasazování veřejné verze, budete potřebovat toohost ho pomocí externího poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="a53cc-199">For now, it’s OK toorun it locally, but as you work on deploying a public version, you will need toohost it with an external provider.</span></span>
> 
> 

<span data-ttu-id="a53cc-200">Nyní si jisti, že jsme registraci aplikace pro UPW hello nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="a53cc-200">Let’s now make sure that we register hello UWP app for push notifications.</span></span> <span data-ttu-id="a53cc-201">V sadě Visual Studio, klikněte na **projektu** > **ukládání** > **propojit aplikaci se hello store**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-201">In Visual Studio, click on **Project** > **Store** > **Associate app with hello store**.</span></span>

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

<span data-ttu-id="a53cc-202">Jakmile se přihlásíte tooyour vývojářský účet, ujistěte se, vybrat existující aplikaci nebo vytvořte novou a přidružit hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="a53cc-202">Once you sign in tooyour developer account, make sure you select an existing app or create a new one and associate hello package with it.</span></span> 

<span data-ttu-id="a53cc-203">Přejděte toohello Dev Center a otevřete hello aplikaci, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a53cc-203">Go toohello Dev Center and open hello app that you just created.</span></span> <span data-ttu-id="a53cc-204">Klikněte na **Služby** > **Nabízená oznámení** > **Web služeb Live Service**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-204">Click **Services** > **Push Notifications** > **Live Services site**.</span></span>

![](./media/notification-hubs-geofence/ms-live-services.png)

<span data-ttu-id="a53cc-205">V lokalitě hello, poznamenejte si hello **tajný klíč aplikace** a hello **SID balíčku**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-205">On hello site, take note of hello **Application Secret** and hello **Package SID**.</span></span> <span data-ttu-id="a53cc-206">Budete potřebovat jak v hello portál Azure – otevřete své Centrum oznámení, klikněte na **nastavení** > **služby oznámení** > **Windows (WNS)**a zadejte informace hello hello požadované pole.</span><span class="sxs-lookup"><span data-stu-id="a53cc-206">You will need both in hello Azure Portal – open your notification hub, click on **Settings** > **Notification Services** > **Windows (WNS)** and enter hello information in hello required fields.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

<span data-ttu-id="a53cc-207">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-207">Click on **Save**.</span></span>

<span data-ttu-id="a53cc-208">V **Průzkumníkovi řešení** klikněte pravým tlačítkem na **Odkazy** a vyberte **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a53cc-208">Right click on **References** in **Solution Explorer** and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="a53cc-209">Je nutné zadat tooadd odkaz toohello **spravovanou knihovnu Microsoft Azure Service Bus** – jednoduše vyhledejte `WindowsAzure.Messaging.Managed` a přidejte ji tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="a53cc-209">We will need tooadd a reference toohello **Microsoft Azure Service Bus managed library** – simply search for `WindowsAzure.Messaging.Managed` and add it tooyour project.</span></span>

![](./media/notification-hubs-geofence/vs-nuget.png)

<span data-ttu-id="a53cc-210">Pro účely testování můžeme vytvořit hello `MainPage_Loaded` obslužné rutiny události ještě jednou a přidejte tento tooit fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="a53cc-210">For testing purposes, we can create hello `MainPage_Loaded` event handler once again, and add this code snippet tooit:</span></span>

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays hello registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

<span data-ttu-id="a53cc-211">Hello výše zaregistruje aplikaci hello hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="a53cc-211">hello above registers hello app with hello notification hub.</span></span> <span data-ttu-id="a53cc-212">Jste připravené toogo!</span><span class="sxs-lookup"><span data-stu-id="a53cc-212">You are ready toogo!</span></span> 

<span data-ttu-id="a53cc-213">V `LocationHelper`, uvnitř hello `Geolocator_PositionChanged` obslužnou rutinu, můžete přidat testovací kód, který bod nuceně umístí hello umístění uvnitř monitorové geografické zóny hello:</span><span class="sxs-lookup"><span data-stu-id="a53cc-213">In `LocationHelper`, inside hello `Geolocator_PositionChanged` handler, you can add a piece of test code that will forcefully put hello location inside hello geofence:</span></span>

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

<span data-ttu-id="a53cc-214">Protože jsme nepředáváme skutečné souřadnice hello, (které nemusí být v rámci hranice hello momentálně hello) a používají předem definované testovací hodnoty, jsme se zobrazí oznámení zobrazí na aktualizace:</span><span class="sxs-lookup"><span data-stu-id="a53cc-214">Because we are not passing hello real coordinates (which might not be within hello boundaries at hello moment) and are using predefined test values, we will see a notification show up on update:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a><span data-ttu-id="a53cc-215">Co dále?</span><span class="sxs-lookup"><span data-stu-id="a53cc-215">What’s next?</span></span>
<span data-ttu-id="a53cc-216">Existuje několik kroků, které můžete potřebovat toofollow v přidání toohello výše toomake se, že je řešení hello produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="a53cc-216">There are a couple of steps that you might need toofollow in addition toohello above toomake sure that hello solution is production-ready.</span></span>

<span data-ttu-id="a53cc-217">Především bude pravděpodobně nutné tooensure monitorovaná geografická zóna je dynamická.</span><span class="sxs-lookup"><span data-stu-id="a53cc-217">First and foremost, you might need tooensure that geofences are dynamic.</span></span> <span data-ttu-id="a53cc-218">To bude vyžadovat další práci s hello rozhraní API Bing v pořadí toobe možné tooupload nové hranice do existujícího zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-218">This will require some extra work with hello Bing API in order toobe able tooupload new boundaries within hello existing data source.</span></span> <span data-ttu-id="a53cc-219">Poraďte se hello [Bing Spatial Data Services API dokumentaci](https://msdn.microsoft.com/library/ff701734.aspx) Další informace o subjektu hello.</span><span class="sxs-lookup"><span data-stu-id="a53cc-219">Consult hello [Bing Spatial Data Services API documentation](https://msdn.microsoft.com/library/ff701734.aspx) for more details on hello subject.</span></span>

<span data-ttu-id="a53cc-220">Druhý, jako jste pracovní tooensure že hello doručuje správným účastníkům toohello, můžete chtít tootarget je prostřednictvím [označování](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="a53cc-220">Second, as you are working tooensure that hello delivery is done toohello right participants, you might want tootarget them via [tagging](notification-hubs-tags-segment-push-message.md).</span></span>

<span data-ttu-id="a53cc-221">Hello řešení uvedené výše popisuje scénář, ve kterém můžete mít širokou škálu cílových platforem, takže jsme nebude omezen hello monitorování geografických zón toosystem specifické možnosti.</span><span class="sxs-lookup"><span data-stu-id="a53cc-221">hello solution shown above describes a scenario in which you might have a wide variety of target platforms, so we did not limit hello geofencing toosystem-specific capabilities.</span></span> <span data-ttu-id="a53cc-222">Ale nutné dodat, hello univerzální platforma Windows nabízí možnosti příliš[zjistit monitorovaná geografická zóna právo out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span><span class="sxs-lookup"><span data-stu-id="a53cc-222">That said, hello Universal Windows Platform offers capabilities too[detect geofences right out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span></span>

<span data-ttu-id="a53cc-223">Další podrobnosti týkající se schopností Notification Hubs najdete na [portálu dokumentace](https://azure.microsoft.com/documentation/services/notification-hubs/).</span><span class="sxs-lookup"><span data-stu-id="a53cc-223">For more details regarding Notification Hubs capabilities, check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/).</span></span>

