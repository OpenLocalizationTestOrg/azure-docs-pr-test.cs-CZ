---
title: "aaaInserting reklamy na straně klienta hello | Microsoft Docs"
description: "Toto téma ukazuje, jak hello tooinsert reklamy na straně klienta."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: e6eab4aa92918ad734db8ac3a4e7818d02ed7fe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="inserting-ads-on-hello-client-side"></a><span data-ttu-id="87baf-103">Vkládání reklam na straně klienta hello</span><span class="sxs-lookup"><span data-stu-id="87baf-103">Inserting ads on hello client side</span></span>
<span data-ttu-id="87baf-104">Toto téma obsahuje informace o tom, tooinsert různých typů reklamy na straně klienta hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-104">This topic contains information on how tooinsert various types of ads on hello client side.</span></span>

<span data-ttu-id="87baf-105">Informace o podpoře uzavřené přidávání titulků a ad v živé streamování videa najdete v tématu [podporované titulky a standardy vložení Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span><span class="sxs-lookup"><span data-stu-id="87baf-105">For information about closed captioning and ad support in Live streaming videos, see [Supported Closed Captioning and Ad Insertion Standards](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span></span>

> [!NOTE]
> <span data-ttu-id="87baf-106">Azure Media Player v současné době nepodporuje reklamy.</span><span class="sxs-lookup"><span data-stu-id="87baf-106">Azure Media Player does not currently support Ads.</span></span>
> 
> 

## <span data-ttu-id="87baf-107"><a id="insert_ads_into_media"></a>Vkládání reklamy do vašeho média</span><span class="sxs-lookup"><span data-stu-id="87baf-107"><a id="insert_ads_into_media"></a>Inserting Ads into your Media</span></span>
<span data-ttu-id="87baf-108">Azure Media Services poskytuje podporu pro vkládání reklam prostřednictvím hello platformu Windows Media: architektury přehrávačů.</span><span class="sxs-lookup"><span data-stu-id="87baf-108">Azure Media Services provides support for ad insertion through hello Windows Media Platform: Player Frameworks.</span></span> <span data-ttu-id="87baf-109">Architektury přehrávačů s podporou ad jsou dostupné pro zařízení s Windows 8, Silverlight, Windows Phone 8 a iOS.</span><span class="sxs-lookup"><span data-stu-id="87baf-109">Player frameworks with ad support are available for Windows 8, Silverlight, Windows Phone 8, and iOS devices.</span></span> <span data-ttu-id="87baf-110">Každý player framework obsahuje ukázkový kód, který ukazuje, jak tooimplement aplikace přehrávače. Existují tři různé druhy služby Active Directory, které lze vložit do seznamu média: seznam.</span><span class="sxs-lookup"><span data-stu-id="87baf-110">Each player framework contains sample code that shows you how tooimplement a player application.There are three different kinds of ads you can insert into your media:list.</span></span>

* <span data-ttu-id="87baf-111">**Lineární** – úplné rámce služby Active Directory, které pozastavit hlavní video hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-111">**Linear** – full frame ads that pause hello main video.</span></span>
* <span data-ttu-id="87baf-112">**Nelineární** – reklamy překrytí, které se zobrazují jako přehrávání videa hlavní hello, obvykle logo nebo jiné statický obrázek umístit hello přehrávač.</span><span class="sxs-lookup"><span data-stu-id="87baf-112">**Nonlinear** – overlay ads that are displayed as hello main video is playing, usually a logo or other static image placed within hello player.</span></span>
* <span data-ttu-id="87baf-113">**Doprovodná** – reklamy, které se zobrazují mimo hello přehrávač.</span><span class="sxs-lookup"><span data-stu-id="87baf-113">**Companion** – ads that are displayed outside of hello player.</span></span>

<span data-ttu-id="87baf-114">Služby Active Directory mohou být umístěny v libovolném bodě hello video hlavní časovou osu.</span><span class="sxs-lookup"><span data-stu-id="87baf-114">Ads can be placed at any point in hello main video’s time line.</span></span> <span data-ttu-id="87baf-115">Se musí zjistit hello player při tooplay hello ad a které tooplay služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="87baf-115">You must tell hello player when tooplay hello ad and which ads tooplay.</span></span> <span data-ttu-id="87baf-116">To se provádí pomocí sady standardní soubory formátu XML: Video Ad služby šablony (VAST), více Ad seznam stop (VMAP) ve digitální Video, šablony abstraktní sekvencování na média (STOŽÁRŮ) a digitální Video Player Ad rozhraní definice (VPAID).</span><span class="sxs-lookup"><span data-stu-id="87baf-116">This is done using a set of standard XML-based files: Video Ad Service Template (VAST), Digital Video Multiple Ad Playlist (VMAP), Media Abstract Sequencing Template (MAST), and Digital Video Player Ad Interface Definition (VPAID).</span></span> <span data-ttu-id="87baf-117">VELKÁ soubory zadejte co toodisplay služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="87baf-117">VAST files specify what ads toodisplay.</span></span> <span data-ttu-id="87baf-118">Soubory VMAP určete, kdy tooplay různé reklamy a obsahovat velká XML.</span><span class="sxs-lookup"><span data-stu-id="87baf-118">VMAP files specify when tooplay various ads and contain VAST XML.</span></span> <span data-ttu-id="87baf-119">STOŽÁRŮ soubory jsou jiný způsob toosequence reklamy, které také může obsahovat velká XML.</span><span class="sxs-lookup"><span data-stu-id="87baf-119">MAST files are another way toosequence ads which also can contain VAST XML.</span></span> <span data-ttu-id="87baf-120">Soubory VPAID definovat rozhraní mezi hello přehrávání videa a hello ad nebo serveru služby ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-120">VPAID files define an interface between hello video player and hello ad or ad server.</span></span>

<span data-ttu-id="87baf-121">Každý player framework funguje jinak, každý bude uvedena v jeho vlastní tématu.</span><span class="sxs-lookup"><span data-stu-id="87baf-121">Each player framework works differently and each will be covered in its own topic.</span></span> <span data-ttu-id="87baf-122">Toto téma popisuje hello základní mechanismy, které používá tooinsert služby Active Directory. Aplikací pro přehrávání videa žádost služby Active Directory ze serveru služby ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-122">This topic will describe hello basic mechanisms used tooinsert ads.Video player applications request ads from an ad server.</span></span> <span data-ttu-id="87baf-123">server služby ad Hello může reagovat v mnoha různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="87baf-123">hello ad server can respond in a number of ways:</span></span>

* <span data-ttu-id="87baf-124">Vrátí soubor velká</span><span class="sxs-lookup"><span data-stu-id="87baf-124">Return a VAST file</span></span>
* <span data-ttu-id="87baf-125">Vrátí VMAP soubor (s embedded VAST)</span><span class="sxs-lookup"><span data-stu-id="87baf-125">Return a VMAP file (with embedded VAST)</span></span>
* <span data-ttu-id="87baf-126">Vrátit STOŽÁRŮ soubor (s embedded VAST)</span><span class="sxs-lookup"><span data-stu-id="87baf-126">Return a MAST file (with embedded VAST)</span></span>
* <span data-ttu-id="87baf-127">Vrátí velká soubor s VPAID služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="87baf-127">Return a VAST file with VPAID ads</span></span>

### <a name="using-a-video-ad-service-template-vast-file"></a><span data-ttu-id="87baf-128">Pomocí souboru šablony (VAST) služby Video Ad</span><span class="sxs-lookup"><span data-stu-id="87baf-128">Using a Video Ad Service Template (VAST) File</span></span>
<span data-ttu-id="87baf-129">Soubor velká Určuje, jaké ad nebo toodisplay služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="87baf-129">A VAST file specifies what ad or ads toodisplay.</span></span> <span data-ttu-id="87baf-130">Hello následující kód XML je příkladem soubor velká pro lineární ad:</span><span class="sxs-lookup"><span data-stu-id="87baf-130">hello following XML is an example of a VAST file for a linear ad:</span></span>

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>

<span data-ttu-id="87baf-131">Lineární ad Hello je popsán hello <**lineární**> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-131">hello linear ad is described by hello <**Linear**> element.</span></span> <span data-ttu-id="87baf-132">Určuje dobu trvání hello hello ad, sledování událostí, klikněte na tlačítko prostřednictvím, klikněte na sledování a několika **MediaFile** elementy.</span><span class="sxs-lookup"><span data-stu-id="87baf-132">It specifies hello duration of hello ad, tracking events, click through, click tracking, and a number of **MediaFile** elements.</span></span> <span data-ttu-id="87baf-133">Sledování události jsou určené v rámci hello <**TrackingEvents**> elementu a povolit služby ad serveru tootrack různých událostí, ke kterým došlo při prohlížení hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-133">Tracking events are specified within hello <**TrackingEvents**> element and allow an ad server tootrack various events that occur while viewing hello ad.</span></span> <span data-ttu-id="87baf-134">V takovém případě hello start a středový, dokončení a rozbalte události se sledují.</span><span class="sxs-lookup"><span data-stu-id="87baf-134">In this case hello start, midpoint, complete, and expand events are tracked.</span></span> <span data-ttu-id="87baf-135">Hello počáteční události dojde, když se zobrazí hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-135">hello start event occurs when hello ad is displayed.</span></span> <span data-ttu-id="87baf-136">Hello středový události dojde, když alespoň, že byl zobrazen 50 % hello ad osy.</span><span class="sxs-lookup"><span data-stu-id="87baf-136">hello midpoint event occurs when at least 50% of hello ad’s timeline has been viewed.</span></span> <span data-ttu-id="87baf-137">událost complete Hello nastane, když hello ad byla spuštěna toohello end.</span><span class="sxs-lookup"><span data-stu-id="87baf-137">hello complete event occurs when hello ad has run toohello end.</span></span> <span data-ttu-id="87baf-138">Hello rozšířené události dojde, když uživatel hello rozbalí úvodní obrazovka toofull přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="87baf-138">hello Expand event occurs when hello user expands hello video player toofull screen.</span></span> <span data-ttu-id="87baf-139">Se zadaným Clickthroughs <**interaktivní**> v rámci <**VideoClicks**> elementu a určuje toodisplay prostředků tooa identifikátoru URI při hello uživatel klikne na hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-139">Clickthroughs are specified with a <**ClickThrough**> element within a <**VideoClicks**> element and specifies a URI tooa resource toodisplay when hello user clicks on hello ad.</span></span> <span data-ttu-id="87baf-140">ClickTracking je uveden v <**ClickTracking**> element také v rámci hello <**VideoClicks**> elementu a určuje sledování prostředků pro hello player toorequest při kliknutí hello na hello ad.hello <**MediaFile**> elementy zadejte informace o konkrétní kódování ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-140">ClickTracking is specified in a <**ClickTracking**> element, also within hello <**VideoClicks**> element and specifies a tracking resource for hello player toorequest when hello user clicks on hello ad.hello <**MediaFile**> elements specify information about a specific encoding of an ad.</span></span> <span data-ttu-id="87baf-141">Když je více než jedna <**MediaFile**> elementu přehrávání videa hello můžete zvolit nejvhodnější kódování hello pro platformu hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-141">When there is more than one <**MediaFile**> element, hello video player can choose hello best encoding for hello platform.</span></span> 

<span data-ttu-id="87baf-142">Lineární reklamy lze zobrazit v zadaném pořadí.</span><span class="sxs-lookup"><span data-stu-id="87baf-142">Linear ads can be displayed in a specified order.</span></span> <span data-ttu-id="87baf-143">toodo, přidejte další <Ad> elementy toohello VAST souboru a zadejte hello pořadí pomocí hello atribut sekvence.</span><span class="sxs-lookup"><span data-stu-id="87baf-143">toodo this, add additional <Ad> elements toohello VAST file and specify hello order using hello sequence attribute.</span></span> <span data-ttu-id="87baf-144">To ukazuje následující příklad Hello:</span><span class="sxs-lookup"><span data-stu-id="87baf-144">hello following example illustrates this:</span></span>

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>

<span data-ttu-id="87baf-145">Nelineární reklamy jsou určené v <Creative> také element.</span><span class="sxs-lookup"><span data-stu-id="87baf-145">Nonlinear ads are specified in a <Creative> element as well.</span></span> <span data-ttu-id="87baf-146">Následující příklad ukazuje Hello <Creative> element, který popisuje nelineární ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-146">hello following example shows a <Creative> element that describes a nonlinear ad.</span></span>

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>


<span data-ttu-id="87baf-147">Hello <**NonLinearAds**> element může obsahovat jednu nebo více <**NonLinear**> prvky, z nichž každý lze popsat nelineární ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-147">hello <**NonLinearAds**> element can contain one or more <**NonLinear**> elements, each of which can describe a nonlinear ad.</span></span> <span data-ttu-id="87baf-148">Hello <**NonLinear**> element určuje hello prostředků pro nelineární ad hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-148">hello <**NonLinear**> element specifies hello resource for hello nonlinear ad.</span></span> <span data-ttu-id="87baf-149">Hello prostředek může být <**StaticResouce**>, <**IFrameResource**>, nebo <**HTMLResouce**>.</span><span class="sxs-lookup"><span data-stu-id="87baf-149">hello resource can be a <**StaticResouce**>, an <**IFrameResource**>, or an <**HTMLResouce**>.</span></span><span data-ttu-id="87baf-150"> <**StaticResource**> popisuje prostředků jiného typu než HTML a definuje creativeType atribut, který určuje, jak se zobrazí hello prostředků:</span><span class="sxs-lookup"><span data-stu-id="87baf-150"> <**StaticResource**> describes a non-HTML resource and defines a creativeType attribute that specifies how hello resource is displayed:</span></span>

<span data-ttu-id="87baf-151">Bitové kopie nebo gif, image nebo jpeg, image nebo png – hello prostředků se zobrazí v kódu HTML <**img**> značky.</span><span class="sxs-lookup"><span data-stu-id="87baf-151">Image/gif, image/jpeg, image/png – hello resource is displayed in an HTML <**img**> tag.</span></span>

<span data-ttu-id="87baf-152">Application/x-javascript – hello prostředků se zobrazí v kódu HTML <**skriptu**> značky.</span><span class="sxs-lookup"><span data-stu-id="87baf-152">Application/x-javascript – hello resource is displayed in an HTML <**script**> tag.</span></span>

<span data-ttu-id="87baf-153">Application/x-shockwave-flash – hello prostředků se zobrazí v Flash player.</span><span class="sxs-lookup"><span data-stu-id="87baf-153">Application/x-shockwave-flash – hello resource is displayed in a Flash player.</span></span>

<span data-ttu-id="87baf-154">**IFrameResource** popisuje prostředek HTML, který lze zobrazit v elementu IFrame.</span><span class="sxs-lookup"><span data-stu-id="87baf-154">**IFrameResource** describes an HTML resource that can be displayed in an IFrame.</span></span> <span data-ttu-id="87baf-155">**HTMLResource** popisuje kód HTML, který lze vložit do webové stránky.</span><span class="sxs-lookup"><span data-stu-id="87baf-155">**HTMLResource** describes a piece of HTML code that can be inserted into a web page.</span></span> <span data-ttu-id="87baf-156">**TrackingEvents** zadejte sledování událostí a hello URI toorequest při výskytu události hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-156">**TrackingEvents** specify tracking events and hello URI toorequest when hello event occurs.</span></span> <span data-ttu-id="87baf-157">V této ukázkové hello jsou sledovány acceptInvitation a sbalit události.</span><span class="sxs-lookup"><span data-stu-id="87baf-157">In this sample hello acceptInvitation and collapse events are tracked.</span></span> <span data-ttu-id="87baf-158">Další informace o hello **NonLinearAds** elementu a jeho podřízených položek, najdete v části IAB.NET/VAST.</span><span class="sxs-lookup"><span data-stu-id="87baf-158">For more information on hello **NonLinearAds** element and its children, see IAB.NET/VAST.</span></span> <span data-ttu-id="87baf-159">Všimněte si, že hello **TrackingEvents** element se nachází v rámci hello **NonLinearAds** prvek spíše než hello **NonLinear** element.</span><span class="sxs-lookup"><span data-stu-id="87baf-159">Note that hello **TrackingEvents** element is located within hello **NonLinearAds** element rather than hello **NonLinear** element.</span></span>

<span data-ttu-id="87baf-160">Doprovodná reklamy jsou definovány v rámci <CompanionAds> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-160">Companion ads are defined within a <CompanionAds> element.</span></span> <span data-ttu-id="87baf-161">Hello <CompanionAds> element může obsahovat jednu nebo více <Companion> elementy.</span><span class="sxs-lookup"><span data-stu-id="87baf-161">hello <CompanionAds> element can contain one or more <Companion> elements.</span></span> <span data-ttu-id="87baf-162">Každý <Companion> element popisuje doprovodné ad a může obsahovat <StaticResource>, <IFrameResource>, nebo <HTMLResource> které jsou určené v hello stejným způsobem jako v nelineárních ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-162">Each <Companion> element describes a companion ad and can contain a <StaticResource>, <IFrameResource>, or <HTMLResource> which are specified in hello same way as in a nonlinear ad.</span></span> <span data-ttu-id="87baf-163">VELKÁ soubor může obsahovat více doprovodných reklamy a aplikace přehrávače hello můžete zvolit nejvhodnější ad toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-163">A VAST file can contain multiple companion ads and hello player application can choose hello most appropriate ad toodisplay.</span></span> <span data-ttu-id="87baf-164">Další informace o VAST najdete v tématu [velká 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="87baf-164">For more information about VAST, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span>

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a><span data-ttu-id="87baf-165">Použití více soubor seznamu stop (VMAP) Ad digitální Video</span><span class="sxs-lookup"><span data-stu-id="87baf-165">Using a Digital Video Multiple Ad Playlist (VMAP) File</span></span>
<span data-ttu-id="87baf-166">Soubor VMAP vám umožní toospecify, když dojde k zalomení ad, jak dlouho je každý konec, kolik reklamy, může se zobrazit v rámci zalomení a jaké typy služby Active Directory může být při zalomení zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="87baf-166">A VMAP file allows you toospecify when ad breaks occur, how long each break is, how many ads can be displayed within a break, and what types of ads may be displayed during a break.</span></span> <span data-ttu-id="87baf-167">Hello následující v soubor VMAP příklad, který definuje zalomení jeden ad:</span><span class="sxs-lookup"><span data-stu-id="87baf-167">hello following in an example VMAP file that defines a single ad break:</span></span>

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

<span data-ttu-id="87baf-168">Soubor VMAP začíná <VMAP> element, který obsahuje jeden nebo více <AdBreak> elementy, každý definování přerušení služby ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-168">A VMAP file begins with a <VMAP> element that contains one or more <AdBreak> elements, each defining an ad break.</span></span> <span data-ttu-id="87baf-169">Každý ad zalomení Určuje typ ukončení, pozastavení ID a posun času.</span><span class="sxs-lookup"><span data-stu-id="87baf-169">Each ad break specifies a break type, break ID, and time offset.</span></span> <span data-ttu-id="87baf-170">Určuje typ hello ad, která může být přehráván během pozastavení hello Hello breakType atribut: lineární, nelineární, nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="87baf-170">hello breakType attribute specifies hello type of ad that can be played during hello break: linear, nonlinear, or display.</span></span> <span data-ttu-id="87baf-171">Zobrazení reklam mapovat tooVAST doprovodné reklamy.</span><span class="sxs-lookup"><span data-stu-id="87baf-171">Display ads map tooVAST companion ads.</span></span> <span data-ttu-id="87baf-172">Více než jeden typ ad můžete zadat seznam oddělený čárkami (bez mezer).</span><span class="sxs-lookup"><span data-stu-id="87baf-172">More than one ad type can be specified in a comma (no spaces) separated list.</span></span> <span data-ttu-id="87baf-173">Hello breakID je volitelné identifikátor pro hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-173">hello breakID is an optional identifier for hello ad.</span></span> <span data-ttu-id="87baf-174">Hello timeOffset Určuje, kdy má být zobrazena hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-174">hello timeOffset specifies when hello ad should be displayed.</span></span> <span data-ttu-id="87baf-175">Určit lze v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="87baf-175">It can be specified in one of hello following ways:</span></span>

1. <span data-ttu-id="87baf-176">Čas – ve formátu hh: mm: nebo hh:mm:ss.mmm kde .mmm je milisekundách.</span><span class="sxs-lookup"><span data-stu-id="87baf-176">Time – in hh:mm:ss or hh:mm:ss.mmm format where .mmm is milliseconds.</span></span> <span data-ttu-id="87baf-177">Hodnota tohoto atributu Hello Určuje dobu hello z hello začátku hello video časová osa toohello začátku hello ad přerušení.</span><span class="sxs-lookup"><span data-stu-id="87baf-177">hello value of this attribute specifies hello time from hello beginning of hello video timeline toohello beginning of hello ad break.</span></span>
2. <span data-ttu-id="87baf-178">Procento – ve formátu n %, kde n je procento hello tooplay hello video časová osa před přehrávání hello ad</span><span class="sxs-lookup"><span data-stu-id="87baf-178">Percentage – in n% format where n is hello percentage of hello video timeline tooplay before playing hello ad</span></span>
3. <span data-ttu-id="87baf-179">Počáteční nebo koncové – Určuje, že ad by měla zobrazit před nebo po hello video byla zobrazena.</span><span class="sxs-lookup"><span data-stu-id="87baf-179">Start/End – specifies that an ad should be displayed before or after hello video has been displayed</span></span>
4. <span data-ttu-id="87baf-180">Pozice – určuje pořadí hello ad zalomení při hello načasování zalomení ad hello je neznámý, například v živé vysílání datového proudu.</span><span class="sxs-lookup"><span data-stu-id="87baf-180">Position – specifies hello order of ad breaks when hello timing of hello ad breaks is unknown, such as in live streaming.</span></span> <span data-ttu-id="87baf-181">Hello pořadí od konce každé ad zadané ve formátu hello #n, kde n je celé číslo větší nebo 1.</span><span class="sxs-lookup"><span data-stu-id="87baf-181">hello order of each ad break is specified in hello #n format where n is an integer 1 or greater.</span></span> <span data-ttu-id="87baf-182">1 znamená, že neexistuje hello ad by měla být přehráván při první příležitosti hello 2 označuje hello ad by měla být přehráván při hello druhý příležitosti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="87baf-182">1 signifies hello ad should be played at hello first opportunity, 2 signifies hello ad should be played at hello second opportunity and so on.</span></span>

<span data-ttu-id="87baf-183">V rámci hello <**AdBreak**> existuje element může být jedna <**AdSource**> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-183">Within hello <**AdBreak**> element there can be one <**AdSource**> element.</span></span> <span data-ttu-id="87baf-184">Hello <**AdSource**> element obsahuje hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="87baf-184">hello <**AdSource**> element contains hello following attributes:</span></span>

1. <span data-ttu-id="87baf-185">ID – Určuje identifikátor zdroje ad hello</span><span class="sxs-lookup"><span data-stu-id="87baf-185">Id – specifies an identifier for hello ad source</span></span>
2. <span data-ttu-id="87baf-186">allowMultipleAds – logická hodnota, která určuje, zda lze zobrazit více reklamy během pozastavení ad hello</span><span class="sxs-lookup"><span data-stu-id="87baf-186">allowMultipleAds – a Boolean value that specifies whether multiple ads can be displayed during hello ad break</span></span>
3. <span data-ttu-id="87baf-187">followRedirects – volitelné logickou hodnotu, která určuje, pokud by měl respektovat přehrávání videa hello přesměruje v rámci odpověď ad</span><span class="sxs-lookup"><span data-stu-id="87baf-187">followRedirects – an optional Boolean value that specifies if hello video player should honor redirects within an ad response</span></span>

<span data-ttu-id="87baf-188">Hello <**AdSource**> element poskytuje hello player odpověď ad vložené nebo odpověď ad tooan odkaz.</span><span class="sxs-lookup"><span data-stu-id="87baf-188">hello <**AdSource**> element provides hello player an inline ad response or a reference tooan ad response.</span></span> <span data-ttu-id="87baf-189">Může obsahovat jednu z hello následující prvky:</span><span class="sxs-lookup"><span data-stu-id="87baf-189">It can contain one of hello following elements:</span></span>

* <span data-ttu-id="87baf-190"><VASTAdData>Označuje, že odpověď velká ad vložené v souboru VMAP hello</span><span class="sxs-lookup"><span data-stu-id="87baf-190"><VASTAdData> indicates a VAST ad response is embedded within hello VMAP file</span></span>
* <span data-ttu-id="87baf-191"><AdTagURI>identifikátor URI, který odkazuje na odpověď ad z jiného systému</span><span class="sxs-lookup"><span data-stu-id="87baf-191"><AdTagURI> a URI that references an ad response from another system</span></span>
* <span data-ttu-id="87baf-192"><CustomAdData>-an libovolný řetězec této respresents-velká odpověď</span><span class="sxs-lookup"><span data-stu-id="87baf-192"><CustomAdData> -an arbitrary string that respresents a non-VAST response</span></span>

<span data-ttu-id="87baf-193">V tomto příkladu je definován odpověď ad v řádku s <VASTAdData> element, který obsahuje odpověď velká ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-193">In this example an in-line ad response is specified with a <VASTAdData> element that contains a VAST ad response.</span></span> <span data-ttu-id="87baf-194">Další informace o hello najdete v části Další prvky [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span><span class="sxs-lookup"><span data-stu-id="87baf-194">For more information about hello other elements, see [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span></span>

<span data-ttu-id="87baf-195">Hello <**AdBreak**> element může také obsahovat jednu <**TrackingEvents**> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-195">hello <**AdBreak**> element can also contain one <**TrackingEvents**> element.</span></span> <span data-ttu-id="87baf-196">Hello <**TrackingEvents**> element umožňuje tootrack hello počáteční nebo koncový přerušení služby ad nebo zda došlo k chybě během pozastavení ad hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-196">hello <**TrackingEvents**> element allows you tootrack hello start or end of an ad break or whether an error occurred during hello ad break.</span></span> <span data-ttu-id="87baf-197">Hello <**TrackingEvents**> element obsahuje jeden nebo více <**sledování**> prvky, z nichž každý určuje sledování událostí a sledování identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="87baf-197">hello <**TrackingEvents**> element contains one or more <**Tracking**> elements, each of which specifies a tracking event and a tracking URI.</span></span> <span data-ttu-id="87baf-198">jsou Hello možné sledování události:</span><span class="sxs-lookup"><span data-stu-id="87baf-198">hello possible tracking events are:</span></span>

1. <span data-ttu-id="87baf-199">breakStart – sleduje hello začátku přerušení služby ad</span><span class="sxs-lookup"><span data-stu-id="87baf-199">breakStart – tracks hello beginning of an ad break</span></span>
2. <span data-ttu-id="87baf-200">breakEnd – dokončení hello sledovat přerušení služby ad</span><span class="sxs-lookup"><span data-stu-id="87baf-200">breakEnd – track hello completion of an ad break</span></span>
3. <span data-ttu-id="87baf-201">Chyba – sleduje chybu, ke které došlo k chybě během pozastavení ad hello</span><span class="sxs-lookup"><span data-stu-id="87baf-201">error – tracks an error that occurred during hello ad break</span></span>

<span data-ttu-id="87baf-202">Hello následující příklad ukazuje VMAP soubor, který určuje sledování událostí</span><span class="sxs-lookup"><span data-stu-id="87baf-202">hello following example shows a VMAP file that specifies tracking events</span></span>

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

<span data-ttu-id="87baf-203">Další informace o hello <**TrackingEvents**> elementu a jeho podřízených položek, najdete v části http://iab.org/VMAP.pdf</span><span class="sxs-lookup"><span data-stu-id="87baf-203">For more information on hello <**TrackingEvents**> element and its children, see http://iab.org/VMAP.pdf</span></span>

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a><span data-ttu-id="87baf-204">Pomocí médií abstraktní, pořadí úloh souboru šablony (STOŽÁRŮ)</span><span class="sxs-lookup"><span data-stu-id="87baf-204">Using a Media Abstract Sequencing Template (MAST) File</span></span>
<span data-ttu-id="87baf-205">Soubor STOŽÁRŮ umožňuje toospecify aktivační události, které definují, kdy se má zobrazit ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-205">A MAST file allows you toospecify triggers that define when an ad is displayed.</span></span> <span data-ttu-id="87baf-206">Hello následující příklad je STOŽÁRŮ soubor obsahující aktivačních událostí pro před kumulativní ad, střední kumulativní ad a po vrácení ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-206">hello following is an example MAST file that contains triggers for a pre roll ad, a mid-roll ad, and a post-roll ad.</span></span>

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' hello trigger for hello next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>



<span data-ttu-id="87baf-207">Soubor STOŽÁRŮ začíná **STOŽÁRŮ** element, který obsahuje jeden **aktivační události** elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-207">A MAST file begins with a **MAST** element that contains one **triggers** element.</span></span> <span data-ttu-id="87baf-208">Hello <triggers> element obsahuje jeden nebo více **aktivační událost** prvky, které definují, kdy by měly být přehrány ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-208">hello <triggers> element contains one or more **trigger** elements that define when an ad should be played.</span></span> 

<span data-ttu-id="87baf-209">Hello **aktivační událost** obsahuje element **startConditions** element, který zadat, když ad by měl začínat tooplay.</span><span class="sxs-lookup"><span data-stu-id="87baf-209">hello **trigger** element contains a **startConditions** element which specify when an ad should begin tooplay.</span></span> <span data-ttu-id="87baf-210">Hello **startConditions** element obsahuje jeden nebo více <condition> elementy.</span><span class="sxs-lookup"><span data-stu-id="87baf-210">hello **startConditions** element contains one or more <condition> elements.</span></span> <span data-ttu-id="87baf-211">Při každé <condition> vyhodnotí tootrue aktivační procedury je zahájeno nebo odebrán, podle toho, jestli se hello <condition> je obsažena v **startConditions** nebo **endConditions** – element v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="87baf-211">When each <condition> evaluates tootrue a trigger is initiated or revoked depending upon whether hello <condition> is contained within a **startConditions** or **endConditions** element respectively.</span></span> <span data-ttu-id="87baf-212">Když více <condition> elementy jsou přítomny, jsou považovány za implicitní OR, způsobí, že všechny podmínky vyhodnocení tootrue tooinitiate hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="87baf-212">When multiple <condition> elements are present, they are treated as an implicit OR, any condition evaluating tootrue will cause hello trigger tooinitiate.</span></span> <span data-ttu-id="87baf-213"><condition>elementy mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="87baf-213"><condition> elements can be nested.</span></span> <span data-ttu-id="87baf-214">Když podřízené <condition> jsou přednastavení elementy, jsou považovány za implicitní a, všechny podmínky se musí vyhodnotit tootrue pro tooinitiate hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="87baf-214">When child <condition> elements are preset, they are treated as an implicit AND, all conditions must evaluate tootrue for hello trigger tooinitiate.</span></span> <span data-ttu-id="87baf-215">Hello <condition> prvek obsahuje následující atributy, které definují podmínky hello hello:</span><span class="sxs-lookup"><span data-stu-id="87baf-215">hello <condition> element contains hello following attributes that define hello condition:</span></span> 

1. <span data-ttu-id="87baf-216">**typ** – Určuje typ hello podmínku, události nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="87baf-216">**type** – specifies hello type of condition, event or property</span></span>
2. <span data-ttu-id="87baf-217">**název** – hello název vlastnosti nebo události toobe hello používá během vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="87baf-217">**name** – hello name of hello property or event toobe used during evaluation</span></span>
3. <span data-ttu-id="87baf-218">**Hodnota** – hello hodnotu, která vlastnost vyhodnotí proti</span><span class="sxs-lookup"><span data-stu-id="87baf-218">**value** – hello value that a property will be evaluated against</span></span>
4. <span data-ttu-id="87baf-219">**operátor** – hello operaci toouse během vyhodnocení: EQ (stejná), NEQ (není rovno), GTR (větší), GEQ (větší nebo rovna), LT (menší než), LEQ (je menší než nebo rovno), MOD (modulo)</span><span class="sxs-lookup"><span data-stu-id="87baf-219">**operator** – hello operation toouse during evaluation: EQ (equal), NEQ (not equal), GTR (greater), GEQ (greater or equal), LT (Less than), LEQ (less than or equal), MOD (modulo)</span></span>

<span data-ttu-id="87baf-220">**endConditions** také obsahovat <condition> elementy.</span><span class="sxs-lookup"><span data-stu-id="87baf-220">**endConditions** also contain <condition> elements.</span></span> <span data-ttu-id="87baf-221">Pokud je podmínka vyhodnocena jako aktivační událost hello tootrue není reset.hello <trigger> také obsahuje element <sources> element, který obsahuje jeden nebo více <source> elementy.</span><span class="sxs-lookup"><span data-stu-id="87baf-221">When a condition evaluates tootrue hello trigger is reset.hello <trigger> element also contains a <sources> element that contains one or more <source> elements.</span></span> <span data-ttu-id="87baf-222">Hello <source> elementy definovat hello URI toohello ad odpovědi a hello typ odpovědi ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-222">hello <source> elements define hello URI toohello ad response and hello type of ad response.</span></span> <span data-ttu-id="87baf-223">V tomto příkladu je zadána identifikátoru URI odpovědi tooa velká.</span><span class="sxs-lookup"><span data-stu-id="87baf-223">In this example a URI is given tooa VAST response.</span></span> 

    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>


### <a name="using-video-player-ad-interface-definition-vpaid"></a><span data-ttu-id="87baf-224">Pomocí definice rozhraní Video Player ve službě Active Directory (VPAID)</span><span class="sxs-lookup"><span data-stu-id="87baf-224">Using Video Player-Ad Interface Definition (VPAID)</span></span>
<span data-ttu-id="87baf-225">VPAID je rozhraní API pro povolení toocommunicate jednotky spustitelné ad s přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="87baf-225">VPAID is an API for enabling executable ad units toocommunicate with a video player.</span></span> <span data-ttu-id="87baf-226">To umožňuje prostředí vysoce interaktivní ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-226">This allows highly interactive ad experiences.</span></span> <span data-ttu-id="87baf-227">uživatel Hello komunikovat s hello ad a hello ad může reagovat tooactions provedenou hello prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="87baf-227">hello user can interact with hello ad and hello ad can respond tooactions taken by hello viewer.</span></span> <span data-ttu-id="87baf-228">Například ad může zobrazit tlačítka, která umožňují tooview hello uživatele Další informace nebo delší verzi hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-228">For example an ad may display buttons that allow hello user tooview more information or a longer version of hello ad.</span></span> <span data-ttu-id="87baf-229">přehrávání videa Hello musí podporovat hello VPAID rozhraní API a hello spustitelné ad musí implementovat rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-229">hello video player must support hello VPAID API and hello executable ad must implement hello API.</span></span> <span data-ttu-id="87baf-230">Když přehrávač požadavků ad serveru služby ad serveru hello odpoví velká odpovědi, která obsahuje VPAID ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-230">When a player requests an ad from an ad server hello server may respond with a VAST response that contains a VPAID ad.</span></span>

<span data-ttu-id="87baf-231">Spustitelný soubor ad se vytvoří v kódu, který je třeba spustit v prostředí runtime například Adobe Flash™ nebo JavaScript, může být spuštěn ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="87baf-231">An executable ad is created in code that must be executed in a runtime environment such as Adobe Flash™ or JavaScript that can be executed in a web browser.</span></span> <span data-ttu-id="87baf-232">Po návratu velká odpověď obsahující VPAID ad serveru služby ad hello hodnotu atributu apiFramework hello hello <MediaFile> element musí být "VPAID".</span><span class="sxs-lookup"><span data-stu-id="87baf-232">When an ad server returns a VAST response containing a VPAID ad, hello value of hello apiFramework attribute in hello <MediaFile> element must be “VPAID”.</span></span> <span data-ttu-id="87baf-233">Tento atribut určuje, že tuto reklamu hello obsažené je spustitelný soubor ad VPAID.</span><span class="sxs-lookup"><span data-stu-id="87baf-233">This attribute specifies that hello contained ad is a VPAID executable ad.</span></span> <span data-ttu-id="87baf-234">Hello typ musí být nastaven typ MIME toohello hello spustitelného souboru, například "application/x-shockwave-flash" nebo "application/x-javascript".</span><span class="sxs-lookup"><span data-stu-id="87baf-234">hello type attribute must be set toohello MIME type of hello executable, such as “application/x-shockwave-flash” or “application/x-javascript”.</span></span> <span data-ttu-id="87baf-235">Hello následující fragment kódu XML zobrazuje hello <MediaFile> element z velká odpověď obsahující spustitelné ad VPAID.</span><span class="sxs-lookup"><span data-stu-id="87baf-235">hello following XML snippet shows hello <MediaFile> element from a VAST response containing a VPAID executable ad.</span></span> 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI tooexecutable ad -->
       </MediaFile>
    </MediaFiles>


<span data-ttu-id="87baf-236">Spustitelný soubor ad může být inicializována pomocí hello <AdParameters> v rámci hello <Linear> nebo <NonLinear> elementy v odpovědi na velká.</span><span class="sxs-lookup"><span data-stu-id="87baf-236">An executable ad can be initialized using hello <AdParameters> element within hello <Linear> or <NonLinear> elements in a VAST response.</span></span> <span data-ttu-id="87baf-237">Další informace o hello <AdParameters> elementu, najdete v části [velká 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="87baf-237">For more information on hello <AdParameters> element, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span> <span data-ttu-id="87baf-238">Další informace o hello VPAID API najdete v tématu [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span><span class="sxs-lookup"><span data-stu-id="87baf-238">For more information about hello VPAID API, see [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span></span>

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a><span data-ttu-id="87baf-239">Implementace systému Windows nebo Windows Phone 8 hráče s podpora služby Ad</span><span class="sxs-lookup"><span data-stu-id="87baf-239">Implementing a Windows or Windows Phone 8 Player with Ad Support</span></span>
<span data-ttu-id="87baf-240">Hello média platformy společnosti Microsoft: Player Framework pro Windows 8 a Windows Phone 8 obsahuje kolekci ukázkové aplikace, které ukazují, jak tooimplement aplikace přehrávání videa pomocí hello framework.</span><span class="sxs-lookup"><span data-stu-id="87baf-240">hello Microsoft Media Platform: Player Framework for Windows 8 and Windows Phone 8 contains a collection of sample applications that show you how tooimplement a video player application using hello framework.</span></span> <span data-ttu-id="87baf-241">Si můžete stáhnout přehrávač Framework a hello ukázky hello z [Player Framework pro Windows 8 a Windows Phone 8](https://playerframework.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="87baf-241">You can download hello Player Framework and hello samples from [Player Framework for Windows 8 and Windows Phone 8](https://playerframework.codeplex.com).</span></span>

<span data-ttu-id="87baf-242">Když otevřete řešení Microsoft.PlayerFramework.Xaml.Samples hello uvidíte počet složek v rámci projektu hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-242">When you open hello Microsoft.PlayerFramework.Xaml.Samples solution you will see a number of folders within hello project.</span></span> <span data-ttu-id="87baf-243">Hello inzerování složka obsahuje hello ukázkový kód relevantní toocreating přehrávání videa s podpora služby ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-243">hello Advertising folder contains hello sample code relevant toocreating a video player with ad support.</span></span> <span data-ttu-id="87baf-244">Uvnitř hello inzerování složka je, jak počet XAML nebo cs každý z které zobrazit soubory reklamy tooinsert jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="87baf-244">Inside hello Advertising folder is a number of XAML/cs files each of which show how tooinsert ads in a different way.</span></span> <span data-ttu-id="87baf-245">Hello následující seznam popisuje každý:</span><span class="sxs-lookup"><span data-stu-id="87baf-245">hello following list describes each:</span></span>

* <span data-ttu-id="87baf-246">AdPodPage.xaml ukazuje, jak pod toodisplay ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-246">AdPodPage.xaml Shows how toodisplay an ad pod.</span></span>
* <span data-ttu-id="87baf-247">Zobrazuje AdSchedulingPage.xaml jak tooschedule reklamy.</span><span class="sxs-lookup"><span data-stu-id="87baf-247">AdSchedulingPage.xaml Shows how tooschedule ads.</span></span>
* <span data-ttu-id="87baf-248">FreeWheelPage.xaml ukazuje, jak toouse hello FreeWheel modulu plug-in tooschedule služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="87baf-248">FreeWheelPage.xaml Shows how toouse hello FreeWheel plugin tooschedule ads.</span></span>
* <span data-ttu-id="87baf-249">Zobrazuje MastPage.xaml jak tooschedule reklam s STOŽÁRŮ souboru.</span><span class="sxs-lookup"><span data-stu-id="87baf-249">MastPage.xaml Shows how tooschedule ads with a MAST file.</span></span>
* <span data-ttu-id="87baf-250">ProgrammaticAdPage.xaml ukazuje, jak naplánovat tooprogrammatically reklamy do video.</span><span class="sxs-lookup"><span data-stu-id="87baf-250">ProgrammaticAdPage.xaml Shows how tooprogrammatically schedule ads into a video.</span></span>
* <span data-ttu-id="87baf-251">Zobrazuje ScheduleClipPage.xaml jak tooschedule ad bez velká souboru.</span><span class="sxs-lookup"><span data-stu-id="87baf-251">ScheduleClipPage.xaml Shows how tooschedule an ad without a VAST file.</span></span>
* <span data-ttu-id="87baf-252">Zobrazuje VastLinearCompanionPage.xaml jak tooinsert lineární a doprovodné ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-252">VastLinearCompanionPage.xaml Shows how tooinsert a linear and companion ad.</span></span>
* <span data-ttu-id="87baf-253">Zobrazuje VastNonLinearPage.xaml jak tooinsert – lineární ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-253">VastNonLinearPage.xaml Shows how tooinsert a non-linear ad.</span></span>
* <span data-ttu-id="87baf-254">Zobrazuje VmapPage.xaml jak toospecify reklam s VMAP souboru.</span><span class="sxs-lookup"><span data-stu-id="87baf-254">VmapPage.xaml Shows how toospecify ads with a VMAP file.</span></span>

<span data-ttu-id="87baf-255">Všechny tyto ukázky používá třídu Media Player hello definované hello player framework.</span><span class="sxs-lookup"><span data-stu-id="87baf-255">Each of these samples uses hello MediaPlayer class defined by hello player framework.</span></span> <span data-ttu-id="87baf-256">Většina ukázek pomocí modulů plug-in, který přidat podporu pro různými formáty odpovědi ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-256">Most samples use plugins that add support for various ad response formats.</span></span> <span data-ttu-id="87baf-257">Ukázka ProgrammaticAdPage Hello prostřednictvím kódu programu komunikuje s instancí Media Player.</span><span class="sxs-lookup"><span data-stu-id="87baf-257">hello ProgrammaticAdPage sample programmatically interacts with a MediaPlayer instance.</span></span>

### <a name="adpodpage-sample"></a><span data-ttu-id="87baf-258">Ukázka AdPodPage</span><span class="sxs-lookup"><span data-stu-id="87baf-258">AdPodPage Sample</span></span>
<span data-ttu-id="87baf-259">Tato ukázka používá hello AdSchedulerPlugin toodefine při toodisplay ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-259">This sample uses hello AdSchedulerPlugin toodefine when toodisplay an ad.</span></span> <span data-ttu-id="87baf-260">V tomto příkladu je střední kumulativní inzerování naplánované toobe přehrávají po 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="87baf-260">In this example a mid-roll advertisement is scheduled toobe played after 5 seconds.</span></span> <span data-ttu-id="87baf-261">pod ad Hello (skupina služby Active Directory toodisplay v pořadí) je zadána v velká souboru, kterou vrátil server služby ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-261">hello ad pod (a group of ads toodisplay in order) is specified in a VAST file returned from an ad server.</span></span> <span data-ttu-id="87baf-262">Hello URI toohello velká soubor je zadán v hello <RemoteAdSource> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-262">hello URI toohello VAST file is specified in hello <RemoteAdSource> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

<span data-ttu-id="87baf-263">Další informace o hello AdSchedulerPlugin najdete v tématu [inzerování v hello Player rozhraní Windows 8 a Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span><span class="sxs-lookup"><span data-stu-id="87baf-263">For more information about hello AdSchedulerPlugin, see [Advertising in hello Player Framework on Windows 8 and Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span></span>

### <a name="adschedulingpage"></a><span data-ttu-id="87baf-264">AdSchedulingPage</span><span class="sxs-lookup"><span data-stu-id="87baf-264">AdSchedulingPage</span></span>
<span data-ttu-id="87baf-265">Tato ukázka používá také hello AdSchedulerPlugin.</span><span class="sxs-lookup"><span data-stu-id="87baf-265">This sample also uses hello AdSchedulerPlugin.</span></span> <span data-ttu-id="87baf-266">Naplánuje tři služby Active Directory, ad před, střední kumulativní ad a po vrácení ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-266">It schedules three ads, a pre-roll ad, a mid-roll ad, and a post-roll ad.</span></span> <span data-ttu-id="87baf-267">Hello URI toohello VAST pro každou reklamu je uveden v <RemoteAdSource> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-267">hello URI toohello VAST for each ad is specified in a <RemoteAdSource> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


### <a name="freewheelpage"></a><span data-ttu-id="87baf-268">FreeWheelPage</span><span class="sxs-lookup"><span data-stu-id="87baf-268">FreeWheelPage</span></span>
<span data-ttu-id="87baf-269">Tato ukázka používá hello FreeWheelPlugin, který určuje zdrojový atribut, který určuje URI souboru SmartXML tooa body, které určuje ad obsah a také informace o plánování ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-269">This sample uses hello FreeWheelPlugin which specifies a Source attribute that specifies a URI that points tooa SmartXML file that specifies ad content as well as ad scheduling information.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a><span data-ttu-id="87baf-270">MastPage</span><span class="sxs-lookup"><span data-stu-id="87baf-270">MastPage</span></span>
<span data-ttu-id="87baf-271">Tato ukázka používá hello MastSchedulerPlugin, který vám umožní toouse STOŽÁRŮ souboru.</span><span class="sxs-lookup"><span data-stu-id="87baf-271">This sample uses hello MastSchedulerPlugin that allows you toouse a MAST file.</span></span> <span data-ttu-id="87baf-272">Zdrojový atribut Hello určuje hello umístění souboru STOŽÁRŮ hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-272">hello Source attribute specifies hello location of hello MAST file.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a><span data-ttu-id="87baf-273">ProgrammaticAdPage</span><span class="sxs-lookup"><span data-stu-id="87baf-273">ProgrammaticAdPage</span></span>
<span data-ttu-id="87baf-274">Tato ukázka prostřednictvím kódu programu komunikuje s hello Media Player.</span><span class="sxs-lookup"><span data-stu-id="87baf-274">This sample programmatically interacts with hello MediaPlayer.</span></span> <span data-ttu-id="87baf-275">soubor ProgrammaticAdPage.xaml Hello vytvoří hello Media Player:</span><span class="sxs-lookup"><span data-stu-id="87baf-275">hello ProgrammaticAdPage.xaml file instantiates hello MediaPlayer:</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

<span data-ttu-id="87baf-276">soubor ProgrammaticAdPage.xaml.cs Hello vytvoří AdHandlerPlugin, při ad mají být zobrazeny a potom přidá obslužné rutiny pro hello MarkerReached událost, která načte RemoteAdSource, zadání soubor velká tooa URI a potom hraje přidává TimelineMarker toospecify Hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-276">hello ProgrammaticAdPage.xaml.cs file creates an AdHandlerPlugin, adds a TimelineMarker toospecify when an ad should be displayed, and then adds a handler for hello MarkerReached event which loads a RemoteAdSource specifying a URI tooa VAST file, and then plays hello ad.</span></span>

    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

### <a name="scheduleclippage"></a><span data-ttu-id="87baf-277">ScheduleClipPage</span><span class="sxs-lookup"><span data-stu-id="87baf-277">ScheduleClipPage</span></span>
<span data-ttu-id="87baf-278">Tato ukázka používá hello AdSchedulerPlugin tooschedule ad střední kumulativní zadáním soubor .wmv, který obsahuje hello ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-278">This sample uses hello AdSchedulerPlugin tooschedule a mid-roll ad by specifying a .wmv file that contains hello ad.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearcompanionpage"></a><span data-ttu-id="87baf-279">VastLinearCompanionPage</span><span class="sxs-lookup"><span data-stu-id="87baf-279">VastLinearCompanionPage</span></span>
<span data-ttu-id="87baf-280">Tato ukázka znázorňuje, jak toouse hello AdSchedulerPlugin tooschedule střední kumulativní lineární ad s doprovodné ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-280">This sample illustrates how toouse hello AdSchedulerPlugin tooschedule a mid-roll linear ad with an companion ad.</span></span> <span data-ttu-id="87baf-281">Hello <RemoteAdSource> element určuje umístění hello velká souboru hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-281">hello <RemoteAdSource> element specifies hello location of hello VAST file.</span></span>

    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearnonlinearpage"></a><span data-ttu-id="87baf-282">VastLinearNonLinearPage</span><span class="sxs-lookup"><span data-stu-id="87baf-282">VastLinearNonLinearPage</span></span>
<span data-ttu-id="87baf-283">Tato ukázka používá hello AdSchedulerPlugin tooschedule lineární a -lineární ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-283">This sample uses hello AdSchedulerPlugin tooschedule a linear and a non-linear ad.</span></span> <span data-ttu-id="87baf-284">Hello umístění velká souboru je definován s hello <RemoteAdSource> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-284">hello VAST file location is specified with hello <RemoteAdSource> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vmappage"></a><span data-ttu-id="87baf-285">VMAPPage</span><span class="sxs-lookup"><span data-stu-id="87baf-285">VMAPPage</span></span>
<span data-ttu-id="87baf-286">Této ukázky používá hello VmapSchedulerPlugin tooschedule reklam pomocí souboru VMAP.</span><span class="sxs-lookup"><span data-stu-id="87baf-286">This samples uses hello VmapSchedulerPlugin tooschedule ads using a VMAP file.</span></span> <span data-ttu-id="87baf-287">Hello URI toohello VMAP soubor je zadán v hello zdrojový atribut hello <VmapSchedulerPlugin> elementu.</span><span class="sxs-lookup"><span data-stu-id="87baf-287">hello URI toohello VMAP file is specified in hello Source attribute of hello <VmapSchedulerPlugin> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a><span data-ttu-id="87baf-288">Implementace iOS Video hráče s podpora služby Ad</span><span class="sxs-lookup"><span data-stu-id="87baf-288">Implementing an iOS Video Player with Ad Support</span></span>
<span data-ttu-id="87baf-289">Hello média platformy společnosti Microsoft: architektura Player pro iOS obsahuje kolekci ukázkové aplikace, které ukazují, jak tooimplement aplikace přehrávání videa pomocí hello framework.</span><span class="sxs-lookup"><span data-stu-id="87baf-289">hello Microsoft Media Platform: Player Framework for iOS contains a collection of sample applications that show you how tooimplement a video player application using hello framework.</span></span> <span data-ttu-id="87baf-290">Si můžete stáhnout přehrávač Framework a hello ukázky hello z [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="87baf-290">You can download hello Player Framework and hello samples from [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span></span> <span data-ttu-id="87baf-291">Hello github stránce má odkaz tooa Wiki, který obsahuje další informace o hello player framework a úvod toohello player ukázková: [Wiki přehrávač médií Azure](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="87baf-291">hello github page has a link tooa Wiki that contains additional information on hello player framework and an introduction toohello player sample: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span></span>

### <a name="scheduling-ads-with-vmap"></a><span data-ttu-id="87baf-292">Plánování služby Active Directory s VMAP</span><span class="sxs-lookup"><span data-stu-id="87baf-292">Scheduling Ads with VMAP</span></span>
<span data-ttu-id="87baf-293">Následující příklad ukazuje, jak Hello tooschedule služby Active Directory pomocí souboru VMAP.</span><span class="sxs-lookup"><span data-stu-id="87baf-293">hello following example shows how tooschedule ads using a VMAP file.</span></span>

    // How tooschedule an Ad using VMAP.
    //First download hello VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using hello downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a><span data-ttu-id="87baf-294">Plánování služby Active Directory s VAST</span><span class="sxs-lookup"><span data-stu-id="87baf-294">Scheduling Ads with VAST</span></span>
<span data-ttu-id="87baf-295">Hello následující ukázka ukazuje, jak tooschedule ad velká pozdní vazby.</span><span class="sxs-lookup"><span data-stu-id="87baf-295">hello following sample shows how tooschedule a late binding VAST ad.</span></span>

    //Example:3 How tooschedule a late binding VAST ad.
    // set hello start time for hello ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify hello URI of hello VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL tooVAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

   <span data-ttu-id="87baf-296">Hello následující ukázka ukazuje, jak tooschedule ad pro velká časné vazby.</span><span class="sxs-lookup"><span data-stu-id="87baf-296">hello following sample shows how tooschedule an early binding VAST ad.</span></span>
<span data-ttu-id="87baf-297">Příklad: 4 plán časné vazby velká ad //Download hello VAST souboru pokud (! [[ framework.adResolver downloadManifest: & manifestu withURL: [nsurl, který URLWithString: @"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[vlastní logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;</span><span class="sxs-lookup"><span data-stu-id="87baf-297">//Example:4 Schedule an early binding VAST ad //Download hello VAST file if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) { [self logFrameworkError]; } else { adLinearTime.startTime = 7; adLinearTime.duration = 0;</span></span>

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

<span data-ttu-id="87baf-298">Hello následující ukázka ukazuje, jak tooinsert ad pomocí hrubý vyjmout úpravy (RCE)</span><span class="sxs-lookup"><span data-stu-id="87baf-298">hello following sample shows how tooinsert an ad using Rough Cut Editing (RCE)</span></span>

    //Example:1 How toouse RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="87baf-299">Hello následující příklad ukazuje, jak pod tooschedule ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-299">hello following example shows how tooschedule an ad pod.</span></span>

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL toocontent
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI tooad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="87baf-300">Následující příklad ukazuje, jak Hello tooschedule-rychlých ad střední vrácení.</span><span class="sxs-lookup"><span data-stu-id="87baf-300">hello following example shows how tooschedule a non-sticky mid-roll ad.</span></span> <span data-ttu-id="87baf-301">Rychlých ad pouze přehraje po bez ohledu na jakékoli požádat o hello prohlížeč provede.</span><span class="sxs-lookup"><span data-stu-id="87baf-301">A non-sticky ad is only played once regardless of any seeking hello viewer performs.</span></span>

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL toocontent
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="87baf-302">Následující příklad ukazuje, jak Hello tooschedule rychlých ad střední vrácení.</span><span class="sxs-lookup"><span data-stu-id="87baf-302">hello following example shows how tooschedule a sticky mid-roll ad.</span></span> <span data-ttu-id="87baf-303">Rychlých ad se zobrazí pokaždé, když bod v časové osy video hello je dosaženo specifikovaného hello.</span><span class="sxs-lookup"><span data-stu-id="87baf-303">A sticky ad will be displayed each time hello specified point on hello video timeline is reached.</span></span>

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI tooad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


<span data-ttu-id="87baf-304">Hello následující ukázka ukazuje, jak tooschedule po vrácení ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-304">hello following sample shows how tooschedule a post-roll ad.</span></span>

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="87baf-305">Hello následující ukázka ukazuje, jak tooschedule před ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-305">hello following sample shows how tooschedule a pre-roll ad.</span></span>

    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="87baf-306">Následující ukázka Hello ukazuje, jak tooschedule střední kumulativní překrytí ad.</span><span class="sxs-lookup"><span data-stu-id="87baf-306">hello following sample shows how tooschedule a mid-roll overlay ad.</span></span>

    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="87baf-307">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="87baf-307">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="87baf-308">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="87baf-308">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="87baf-309">Viz také</span><span class="sxs-lookup"><span data-stu-id="87baf-309">See Also</span></span>
[<span data-ttu-id="87baf-310">Vývoj aplikací videopřehrávače</span><span class="sxs-lookup"><span data-stu-id="87baf-310">Develop video player applications</span></span>](media-services-develop-video-players.md)

