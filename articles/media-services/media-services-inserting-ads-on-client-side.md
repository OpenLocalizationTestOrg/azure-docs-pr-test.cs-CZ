---
title: "Vkládání reklam na straně klienta | Microsoft Docs"
description: "Toto téma ukazuje, jak vložit reklamy na straně klienta."
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
ms.openlocfilehash: 52ba731f88c630830560e3cf8406ba2e9613c8a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="inserting-ads-on-the-client-side"></a><span data-ttu-id="f397a-103">Vkládání reklam na straně klienta</span><span class="sxs-lookup"><span data-stu-id="f397a-103">Inserting ads on the client side</span></span>
<span data-ttu-id="f397a-104">Toto téma obsahuje informace o tom, jak vložit různých typů reklamy na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f397a-104">This topic contains information on how to insert various types of ads on the client side.</span></span>

<span data-ttu-id="f397a-105">Informace o podpoře uzavřené přidávání titulků a ad v živé streamování videa najdete v tématu [podporované titulky a standardy vložení Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span><span class="sxs-lookup"><span data-stu-id="f397a-105">For information about closed captioning and ad support in Live streaming videos, see [Supported Closed Captioning and Ad Insertion Standards](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span></span>

> [!NOTE]
> <span data-ttu-id="f397a-106">Azure Media Player v současné době nepodporuje reklamy.</span><span class="sxs-lookup"><span data-stu-id="f397a-106">Azure Media Player does not currently support Ads.</span></span>
> 
> 

## <span data-ttu-id="f397a-107"><a id="insert_ads_into_media"></a>Vkládání reklamy do vašeho média</span><span class="sxs-lookup"><span data-stu-id="f397a-107"><a id="insert_ads_into_media"></a>Inserting Ads into your Media</span></span>
<span data-ttu-id="f397a-108">Azure Media Services poskytuje podporu pro vkládání reklam prostřednictvím platformu Windows Media: architektury přehrávačů.</span><span class="sxs-lookup"><span data-stu-id="f397a-108">Azure Media Services provides support for ad insertion through the Windows Media Platform: Player Frameworks.</span></span> <span data-ttu-id="f397a-109">Architektury přehrávačů s podporou ad jsou dostupné pro zařízení s Windows 8, Silverlight, Windows Phone 8 a iOS.</span><span class="sxs-lookup"><span data-stu-id="f397a-109">Player frameworks with ad support are available for Windows 8, Silverlight, Windows Phone 8, and iOS devices.</span></span> <span data-ttu-id="f397a-110">Každý player framework obsahuje ukázkový kód, který ukazuje, jak implementovat aplikace přehrávače. Existují tři různé druhy služby Active Directory, které lze vložit do seznamu média: seznam.</span><span class="sxs-lookup"><span data-stu-id="f397a-110">Each player framework contains sample code that shows you how to implement a player application.There are three different kinds of ads you can insert into your media:list.</span></span>

* <span data-ttu-id="f397a-111">**Lineární** – úplné rámce služby Active Directory, které pozastavit hlavní video.</span><span class="sxs-lookup"><span data-stu-id="f397a-111">**Linear** – full frame ads that pause the main video.</span></span>
* <span data-ttu-id="f397a-112">**Nelineární** – reklamy překrytí, které se zobrazují jako přehrávání videa hlavní, obvykle logo nebo jiné statický obrázek umístit přehrávač.</span><span class="sxs-lookup"><span data-stu-id="f397a-112">**Nonlinear** – overlay ads that are displayed as the main video is playing, usually a logo or other static image placed within the player.</span></span>
* <span data-ttu-id="f397a-113">**Doprovodná** – reklamu, které se zobrazují mimo přehrávač.</span><span class="sxs-lookup"><span data-stu-id="f397a-113">**Companion** – ads that are displayed outside of the player.</span></span>

<span data-ttu-id="f397a-114">Služby Active Directory mohou být umístěny v libovolném bodě hlavní video časovou osu.</span><span class="sxs-lookup"><span data-stu-id="f397a-114">Ads can be placed at any point in the main video’s time line.</span></span> <span data-ttu-id="f397a-115">Přehrávač musí zjistit, kdy přehrávání ad a které reklamy přehrávání.</span><span class="sxs-lookup"><span data-stu-id="f397a-115">You must tell the player when to play the ad and which ads to play.</span></span> <span data-ttu-id="f397a-116">To se provádí pomocí sady standardní soubory formátu XML: Video Ad služby šablony (VAST), více Ad seznam stop (VMAP) ve digitální Video, šablony abstraktní sekvencování na média (STOŽÁRŮ) a digitální Video Player Ad rozhraní definice (VPAID).</span><span class="sxs-lookup"><span data-stu-id="f397a-116">This is done using a set of standard XML-based files: Video Ad Service Template (VAST), Digital Video Multiple Ad Playlist (VMAP), Media Abstract Sequencing Template (MAST), and Digital Video Player Ad Interface Definition (VPAID).</span></span> <span data-ttu-id="f397a-117">VELKÁ soubory zadejte co zobrazit reklamy.</span><span class="sxs-lookup"><span data-stu-id="f397a-117">VAST files specify what ads to display.</span></span> <span data-ttu-id="f397a-118">Soubory VMAP určete, kdy k přehrávání různých reklamy a obsahují velká XML.</span><span class="sxs-lookup"><span data-stu-id="f397a-118">VMAP files specify when to play various ads and contain VAST XML.</span></span> <span data-ttu-id="f397a-119">STOŽÁRŮ soubory jsou jiný způsob, jak pořadí služby Active Directory, které také může obsahovat velká XML.</span><span class="sxs-lookup"><span data-stu-id="f397a-119">MAST files are another way to sequence ads which also can contain VAST XML.</span></span> <span data-ttu-id="f397a-120">Soubory VPAID definovat rozhraní mezi přehrávání videa a ad nebo serveru služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-120">VPAID files define an interface between the video player and the ad or ad server.</span></span>

<span data-ttu-id="f397a-121">Každý player framework funguje jinak, každý bude uvedena v jeho vlastní tématu.</span><span class="sxs-lookup"><span data-stu-id="f397a-121">Each player framework works differently and each will be covered in its own topic.</span></span> <span data-ttu-id="f397a-122">Toto téma popisuje základní mechanismus používaný k vkládání reklam. Aplikací pro přehrávání videa žádost služby Active Directory ze serveru služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-122">This topic will describe the basic mechanisms used to insert ads.Video player applications request ads from an ad server.</span></span> <span data-ttu-id="f397a-123">Serveru služby ad může reagovat v mnoha různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="f397a-123">The ad server can respond in a number of ways:</span></span>

* <span data-ttu-id="f397a-124">Vrátí soubor velká</span><span class="sxs-lookup"><span data-stu-id="f397a-124">Return a VAST file</span></span>
* <span data-ttu-id="f397a-125">Vrátí VMAP soubor (s embedded VAST)</span><span class="sxs-lookup"><span data-stu-id="f397a-125">Return a VMAP file (with embedded VAST)</span></span>
* <span data-ttu-id="f397a-126">Vrátit STOŽÁRŮ soubor (s embedded VAST)</span><span class="sxs-lookup"><span data-stu-id="f397a-126">Return a MAST file (with embedded VAST)</span></span>
* <span data-ttu-id="f397a-127">Vrátí velká soubor s VPAID služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="f397a-127">Return a VAST file with VPAID ads</span></span>

### <a name="using-a-video-ad-service-template-vast-file"></a><span data-ttu-id="f397a-128">Pomocí souboru šablony (VAST) služby Video Ad</span><span class="sxs-lookup"><span data-stu-id="f397a-128">Using a Video Ad Service Template (VAST) File</span></span>
<span data-ttu-id="f397a-129">Soubor velká Určuje, jaké ad nebo zobrazit reklamy.</span><span class="sxs-lookup"><span data-stu-id="f397a-129">A VAST file specifies what ad or ads to display.</span></span> <span data-ttu-id="f397a-130">Následující kód XML je příkladem soubor velká pro lineární ad:</span><span class="sxs-lookup"><span data-stu-id="f397a-130">The following XML is an example of a VAST file for a linear ad:</span></span>

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

<span data-ttu-id="f397a-131">Lineární ad je popsán <**lineární**> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-131">The linear ad is described by the <**Linear**> element.</span></span> <span data-ttu-id="f397a-132">Určuje dobu trvání ad, sledování událostí, klikněte na tlačítko prostřednictvím, klikněte na sledování a několika **MediaFile** elementy.</span><span class="sxs-lookup"><span data-stu-id="f397a-132">It specifies the duration of the ad, tracking events, click through, click tracking, and a number of **MediaFile** elements.</span></span> <span data-ttu-id="f397a-133">Sledování události jsou určené v rámci <**TrackingEvents**> elementu a povolit serveru služby ad sledovat různé události, ke kterým došlo při prohlížení ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-133">Tracking events are specified within the <**TrackingEvents**> element and allow an ad server to track various events that occur while viewing the ad.</span></span> <span data-ttu-id="f397a-134">V tomto případě začátku středový, dokončení a rozbalte události se sledují.</span><span class="sxs-lookup"><span data-stu-id="f397a-134">In this case the start, midpoint, complete, and expand events are tracked.</span></span> <span data-ttu-id="f397a-135">Událost spuštění nastane, když se zobrazí ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-135">The start event occurs when the ad is displayed.</span></span> <span data-ttu-id="f397a-136">Středový událost nastane, když alespoň, že byl zobrazen 50 % osy ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-136">The midpoint event occurs when at least 50% of the ad’s timeline has been viewed.</span></span> <span data-ttu-id="f397a-137">Událost complete nastane, když ad byla spuštěna na konec.</span><span class="sxs-lookup"><span data-stu-id="f397a-137">The complete event occurs when the ad has run to the end.</span></span> <span data-ttu-id="f397a-138">Rozbalte událost nastane, když uživatel rozšíří přehrávání videa na celou obrazovku.</span><span class="sxs-lookup"><span data-stu-id="f397a-138">The Expand event occurs when the user expands the video player to full screen.</span></span> <span data-ttu-id="f397a-139">Se zadaným Clickthroughs <**interaktivní**> v rámci <**VideoClicks**> elementu a určuje identifikátoru URI prostředku zobrazíte, když uživatel klikne na ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-139">Clickthroughs are specified with a <**ClickThrough**> element within a <**VideoClicks**> element and specifies a URI to a resource to display when the user clicks on the ad.</span></span> <span data-ttu-id="f397a-140">ClickTracking je uveden v <**ClickTracking**> elementu, také uvnitř <**VideoClicks**> elementu a určuje sledování prostředků pro player k vyžádání, když uživatel klikne na ad. <**MediaFile**> elementy zadejte informace o konkrétní kódování ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-140">ClickTracking is specified in a <**ClickTracking**> element, also within the <**VideoClicks**> element and specifies a tracking resource for the player to request when the user clicks on the ad.The <**MediaFile**> elements specify information about a specific encoding of an ad.</span></span> <span data-ttu-id="f397a-141">Když je více než jedna <**MediaFile**> elementu přehrávání videa můžete zvolit nejvhodnější kódování pro platformu.</span><span class="sxs-lookup"><span data-stu-id="f397a-141">When there is more than one <**MediaFile**> element, the video player can choose the best encoding for the platform.</span></span> 

<span data-ttu-id="f397a-142">Lineární reklamy lze zobrazit v zadaném pořadí.</span><span class="sxs-lookup"><span data-stu-id="f397a-142">Linear ads can be displayed in a specified order.</span></span> <span data-ttu-id="f397a-143">Chcete-li to provést, přidejte další <Ad> elementy na VAST souboru a určit pořadí, pomocí pořadí atributu.</span><span class="sxs-lookup"><span data-stu-id="f397a-143">To do this, add additional <Ad> elements to the VAST file and specify the order using the sequence attribute.</span></span> <span data-ttu-id="f397a-144">Následující příklad ilustruje toto:</span><span class="sxs-lookup"><span data-stu-id="f397a-144">The following example illustrates this:</span></span>

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

<span data-ttu-id="f397a-145">Nelineární reklamy jsou určené v <Creative> také element.</span><span class="sxs-lookup"><span data-stu-id="f397a-145">Nonlinear ads are specified in a <Creative> element as well.</span></span> <span data-ttu-id="f397a-146">Následující příklad ukazuje <Creative> element, který popisuje nelineární ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-146">The following example shows a <Creative> element that describes a nonlinear ad.</span></span>

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


<span data-ttu-id="f397a-147"><**NonLinearAds**> element může obsahovat jednu nebo více <**NonLinear**> prvky, z nichž každý lze popsat nelineární ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-147">The <**NonLinearAds**> element can contain one or more <**NonLinear**> elements, each of which can describe a nonlinear ad.</span></span> <span data-ttu-id="f397a-148"><**NonLinear**> element určuje prostředek pro nelineární ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-148">The <**NonLinear**> element specifies the resource for the nonlinear ad.</span></span> <span data-ttu-id="f397a-149">Prostředek může být <**StaticResouce**>, <**IFrameResource**>, nebo <**HTMLResouce**>.</span><span class="sxs-lookup"><span data-stu-id="f397a-149">The resource can be a <**StaticResouce**>, an <**IFrameResource**>, or an <**HTMLResouce**>.</span></span><span data-ttu-id="f397a-150"> <**StaticResource**> popisuje prostředků jiného typu než HTML a definuje creativeType atribut, který určuje, jak se zobrazí prostředku:</span><span class="sxs-lookup"><span data-stu-id="f397a-150"> <**StaticResource**> describes a non-HTML resource and defines a creativeType attribute that specifies how the resource is displayed:</span></span>

<span data-ttu-id="f397a-151">Bitové kopie nebo gif, image nebo jpeg, image nebo png – prostředku se zobrazí v kódu HTML <**img**> značky.</span><span class="sxs-lookup"><span data-stu-id="f397a-151">Image/gif, image/jpeg, image/png – the resource is displayed in an HTML <**img**> tag.</span></span>

<span data-ttu-id="f397a-152">Application/x-javascript – prostředku se zobrazí v kódu HTML <**skriptu**> značky.</span><span class="sxs-lookup"><span data-stu-id="f397a-152">Application/x-javascript – the resource is displayed in an HTML <**script**> tag.</span></span>

<span data-ttu-id="f397a-153">Application/x-shockwave-flash – prostředku se zobrazí v Flash player.</span><span class="sxs-lookup"><span data-stu-id="f397a-153">Application/x-shockwave-flash – the resource is displayed in a Flash player.</span></span>

<span data-ttu-id="f397a-154">**IFrameResource** popisuje prostředek HTML, který lze zobrazit v elementu IFrame.</span><span class="sxs-lookup"><span data-stu-id="f397a-154">**IFrameResource** describes an HTML resource that can be displayed in an IFrame.</span></span> <span data-ttu-id="f397a-155">**HTMLResource** popisuje kód HTML, který lze vložit do webové stránky.</span><span class="sxs-lookup"><span data-stu-id="f397a-155">**HTMLResource** describes a piece of HTML code that can be inserted into a web page.</span></span> <span data-ttu-id="f397a-156">**TrackingEvents** zadejte sledování událostí a identifikátor URI požadavku dojde k události.</span><span class="sxs-lookup"><span data-stu-id="f397a-156">**TrackingEvents** specify tracking events and the URI to request when the event occurs.</span></span> <span data-ttu-id="f397a-157">V této ukázce jsou sledovány acceptInvitation a sbalit události.</span><span class="sxs-lookup"><span data-stu-id="f397a-157">In this sample the acceptInvitation and collapse events are tracked.</span></span> <span data-ttu-id="f397a-158">Další informace o **NonLinearAds** elementu a jeho podřízených položek, najdete v části IAB.NET/VAST.</span><span class="sxs-lookup"><span data-stu-id="f397a-158">For more information on the **NonLinearAds** element and its children, see IAB.NET/VAST.</span></span> <span data-ttu-id="f397a-159">Všimněte si, že **TrackingEvents** se nachází v rámci elementu **NonLinearAds** element místo **NonLinear** element.</span><span class="sxs-lookup"><span data-stu-id="f397a-159">Note that the **TrackingEvents** element is located within the **NonLinearAds** element rather than the **NonLinear** element.</span></span>

<span data-ttu-id="f397a-160">Doprovodná reklamy jsou definovány v rámci <CompanionAds> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-160">Companion ads are defined within a <CompanionAds> element.</span></span> <span data-ttu-id="f397a-161"><CompanionAds> Element může obsahovat jednu nebo více <Companion> elementy.</span><span class="sxs-lookup"><span data-stu-id="f397a-161">The <CompanionAds> element can contain one or more <Companion> elements.</span></span> <span data-ttu-id="f397a-162">Každý <Companion> element popisuje doprovodné ad a může obsahovat <StaticResource>, <IFrameResource>, nebo <HTMLResource> které je určené stejným způsobem jako nelineární ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-162">Each <Companion> element describes a companion ad and can contain a <StaticResource>, <IFrameResource>, or <HTMLResource> which are specified in the same way as in a nonlinear ad.</span></span> <span data-ttu-id="f397a-163">VELKÁ soubor může obsahovat více doprovodných reklamy a aplikace přehrávače můžete zvolit nejvhodnější ad k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f397a-163">A VAST file can contain multiple companion ads and the player application can choose the most appropriate ad to display.</span></span> <span data-ttu-id="f397a-164">Další informace o VAST najdete v tématu [velká 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="f397a-164">For more information about VAST, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span>

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a><span data-ttu-id="f397a-165">Použití více soubor seznamu stop (VMAP) Ad digitální Video</span><span class="sxs-lookup"><span data-stu-id="f397a-165">Using a Digital Video Multiple Ad Playlist (VMAP) File</span></span>
<span data-ttu-id="f397a-166">Soubor VMAP umožňuje zadat, když dojde k zalomení ad, jak dlouho je každý konec, kolik reklamy, může se zobrazit v rámci zalomení a jaké typy služby Active Directory může být při zalomení zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="f397a-166">A VMAP file allows you to specify when ad breaks occur, how long each break is, how many ads can be displayed within a break, and what types of ads may be displayed during a break.</span></span> <span data-ttu-id="f397a-167">Následující soubor VMAP příklad, který definuje zalomení jeden ad:</span><span class="sxs-lookup"><span data-stu-id="f397a-167">The following in an example VMAP file that defines a single ad break:</span></span>

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

<span data-ttu-id="f397a-168">Soubor VMAP začíná <VMAP> element, který obsahuje jeden nebo více <AdBreak> elementy, každý definování přerušení služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-168">A VMAP file begins with a <VMAP> element that contains one or more <AdBreak> elements, each defining an ad break.</span></span> <span data-ttu-id="f397a-169">Každý ad zalomení Určuje typ ukončení, pozastavení ID a posun času.</span><span class="sxs-lookup"><span data-stu-id="f397a-169">Each ad break specifies a break type, break ID, and time offset.</span></span> <span data-ttu-id="f397a-170">Atribut breakType Určuje typ ad, která může být přehráván během rozdělení: lineární, nelineární, nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f397a-170">The breakType attribute specifies the type of ad that can be played during the break: linear, nonlinear, or display.</span></span> <span data-ttu-id="f397a-171">Pro velká doprovodné reklamy zobrazit reklamy mapy.</span><span class="sxs-lookup"><span data-stu-id="f397a-171">Display ads map to VAST companion ads.</span></span> <span data-ttu-id="f397a-172">Více než jeden typ ad můžete zadat seznam oddělený čárkami (bez mezer).</span><span class="sxs-lookup"><span data-stu-id="f397a-172">More than one ad type can be specified in a comma (no spaces) separated list.</span></span> <span data-ttu-id="f397a-173">BreakID je volitelné identifikátor pro služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-173">The breakID is an optional identifier for the ad.</span></span> <span data-ttu-id="f397a-174">TimeOffset Určuje, kdy má být zobrazena ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-174">The timeOffset specifies when the ad should be displayed.</span></span> <span data-ttu-id="f397a-175">Určit lze v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="f397a-175">It can be specified in one of the following ways:</span></span>

1. <span data-ttu-id="f397a-176">Čas – ve formátu hh: mm: nebo hh:mm:ss.mmm kde .mmm je milisekundách.</span><span class="sxs-lookup"><span data-stu-id="f397a-176">Time – in hh:mm:ss or hh:mm:ss.mmm format where .mmm is milliseconds.</span></span> <span data-ttu-id="f397a-177">Hodnota tohoto atributu určuje dobu od začátku video časovou osu na začátek rozdělení ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-177">The value of this attribute specifies the time from the beginning of the video timeline to the beginning of the ad break.</span></span>
2. <span data-ttu-id="f397a-178">Procento – ve formátu n % tam, kde n je procento video časovou osu přehrávání před přehrávání ad</span><span class="sxs-lookup"><span data-stu-id="f397a-178">Percentage – in n% format where n is the percentage of the video timeline to play before playing the ad</span></span>
3. <span data-ttu-id="f397a-179">Počáteční nebo koncové – Určuje, že ad by měla zobrazit před nebo po video byla zobrazena.</span><span class="sxs-lookup"><span data-stu-id="f397a-179">Start/End – specifies that an ad should be displayed before or after the video has been displayed</span></span>
4. <span data-ttu-id="f397a-180">Pozice – určuje pořadí zalomení ad, když načasování zalomení ad je neznámý, například v živé vysílání datového proudu.</span><span class="sxs-lookup"><span data-stu-id="f397a-180">Position – specifies the order of ad breaks when the timing of the ad breaks is unknown, such as in live streaming.</span></span> <span data-ttu-id="f397a-181">Pořadí od konce každé ad je zadána ve formátu #n, kde n je celé číslo větší nebo 1.</span><span class="sxs-lookup"><span data-stu-id="f397a-181">The order of each ad break is specified in the #n format where n is an integer 1 or greater.</span></span> <span data-ttu-id="f397a-182">1 znamená, že neexistuje ad by měla být přehráván při první příležitosti 2 označuje, že ad by měla být přehráván na druhou možnost a tak dále.</span><span class="sxs-lookup"><span data-stu-id="f397a-182">1 signifies the ad should be played at the first opportunity, 2 signifies the ad should be played at the second opportunity and so on.</span></span>

<span data-ttu-id="f397a-183">V rámci <**AdBreak**> existuje element může být jedna <**AdSource**> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-183">Within the <**AdBreak**> element there can be one <**AdSource**> element.</span></span> <span data-ttu-id="f397a-184"><**AdSource**> element obsahuje následující atributy:</span><span class="sxs-lookup"><span data-stu-id="f397a-184">The <**AdSource**> element contains the following attributes:</span></span>

1. <span data-ttu-id="f397a-185">ID – Určuje identifikátor zdroje ad</span><span class="sxs-lookup"><span data-stu-id="f397a-185">Id – specifies an identifier for the ad source</span></span>
2. <span data-ttu-id="f397a-186">allowMultipleAds – logická hodnota, která určuje, zda lze zobrazit více reklamy během pozastavení ad</span><span class="sxs-lookup"><span data-stu-id="f397a-186">allowMultipleAds – a Boolean value that specifies whether multiple ads can be displayed during the ad break</span></span>
3. <span data-ttu-id="f397a-187">followRedirects – přesměruje volitelné logickou hodnotu, která určuje, pokud by měl respektovat přehrávání videa v rámci odpověď ad</span><span class="sxs-lookup"><span data-stu-id="f397a-187">followRedirects – an optional Boolean value that specifies if the video player should honor redirects within an ad response</span></span>

<span data-ttu-id="f397a-188"><**AdSource**> element poskytuje přehrávač odpověď ad vložené nebo odkaz na odpověď ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-188">The <**AdSource**> element provides the player an inline ad response or a reference to an ad response.</span></span> <span data-ttu-id="f397a-189">Může obsahovat jeden z následujících elementů:</span><span class="sxs-lookup"><span data-stu-id="f397a-189">It can contain one of the following elements:</span></span>

* <span data-ttu-id="f397a-190"><VASTAdData>Označuje, že odpověď velká ad vložené v rámci tohoto souboru VMAP</span><span class="sxs-lookup"><span data-stu-id="f397a-190"><VASTAdData> indicates a VAST ad response is embedded within the VMAP file</span></span>
* <span data-ttu-id="f397a-191"><AdTagURI>identifikátor URI, který odkazuje na odpověď ad z jiného systému</span><span class="sxs-lookup"><span data-stu-id="f397a-191"><AdTagURI> a URI that references an ad response from another system</span></span>
* <span data-ttu-id="f397a-192"><CustomAdData>-an libovolný řetězec této respresents-velká odpověď</span><span class="sxs-lookup"><span data-stu-id="f397a-192"><CustomAdData> -an arbitrary string that respresents a non-VAST response</span></span>

<span data-ttu-id="f397a-193">V tomto příkladu je definován odpověď ad v řádku s <VASTAdData> element, který obsahuje odpověď velká ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-193">In this example an in-line ad response is specified with a <VASTAdData> element that contains a VAST ad response.</span></span> <span data-ttu-id="f397a-194">Další informace o dalších prvků najdete v tématu [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span><span class="sxs-lookup"><span data-stu-id="f397a-194">For more information about the other elements, see [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span></span>

<span data-ttu-id="f397a-195"><**AdBreak**> element může také obsahovat jednu <**TrackingEvents**> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-195">The <**AdBreak**> element can also contain one <**TrackingEvents**> element.</span></span> <span data-ttu-id="f397a-196"><**TrackingEvents**> elementu umožňuje sledovat počáteční nebo koncový přerušení služby ad nebo zda došlo k chybě během pozastavení ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-196">The <**TrackingEvents**> element allows you to track the start or end of an ad break or whether an error occurred during the ad break.</span></span> <span data-ttu-id="f397a-197"><**TrackingEvents**> element obsahuje jeden nebo více <**sledování**> prvky, z nichž každý určuje sledování událostí a sledování identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="f397a-197">The <**TrackingEvents**> element contains one or more <**Tracking**> elements, each of which specifies a tracking event and a tracking URI.</span></span> <span data-ttu-id="f397a-198">Jsou možné sledování události:</span><span class="sxs-lookup"><span data-stu-id="f397a-198">The possible tracking events are:</span></span>

1. <span data-ttu-id="f397a-199">breakStart – sleduje začátku přerušení služby ad</span><span class="sxs-lookup"><span data-stu-id="f397a-199">breakStart – tracks the beginning of an ad break</span></span>
2. <span data-ttu-id="f397a-200">breakEnd – sledování dokončení přerušení služby ad</span><span class="sxs-lookup"><span data-stu-id="f397a-200">breakEnd – track the completion of an ad break</span></span>
3. <span data-ttu-id="f397a-201">Chyba – sleduje chybu, ke které došlo k chybě během pozastavení ad</span><span class="sxs-lookup"><span data-stu-id="f397a-201">error – tracks an error that occurred during the ad break</span></span>

<span data-ttu-id="f397a-202">Následující příklad ukazuje VMAP soubor, který určuje sledování událostí</span><span class="sxs-lookup"><span data-stu-id="f397a-202">The following example shows a VMAP file that specifies tracking events</span></span>

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

<span data-ttu-id="f397a-203">Další informace o <**TrackingEvents**> elementu a jeho podřízených položek, najdete v části http://iab.org/VMAP.pdf</span><span class="sxs-lookup"><span data-stu-id="f397a-203">For more information on the <**TrackingEvents**> element and its children, see http://iab.org/VMAP.pdf</span></span>

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a><span data-ttu-id="f397a-204">Pomocí médií abstraktní, pořadí úloh souboru šablony (STOŽÁRŮ)</span><span class="sxs-lookup"><span data-stu-id="f397a-204">Using a Media Abstract Sequencing Template (MAST) File</span></span>
<span data-ttu-id="f397a-205">Soubor STOŽÁRŮ umožňuje zadat aktivační události, které definují, kdy se má zobrazit ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-205">A MAST file allows you to specify triggers that define when an ad is displayed.</span></span> <span data-ttu-id="f397a-206">Následující příklad je STOŽÁRŮ soubor obsahující aktivačních událostí pro před kumulativní ad, střední kumulativní ad a po vrácení ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-206">The following is an example MAST file that contains triggers for a pre roll ad, a mid-roll ad, and a post-roll ad.</span></span>

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
            <!--This 'resets' the trigger for the next clip-->
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



<span data-ttu-id="f397a-207">Soubor STOŽÁRŮ začíná **STOŽÁRŮ** element, který obsahuje jeden **aktivační události** elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-207">A MAST file begins with a **MAST** element that contains one **triggers** element.</span></span> <span data-ttu-id="f397a-208"><triggers> Element obsahuje jeden nebo více **aktivační událost** prvky, které definují, kdy by měly být přehrány ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-208">The <triggers> element contains one or more **trigger** elements that define when an ad should be played.</span></span> 

<span data-ttu-id="f397a-209">**Aktivační událost** obsahuje element **startConditions** element, který zadat, pokud by měl začínat ad přehrávání.</span><span class="sxs-lookup"><span data-stu-id="f397a-209">The **trigger** element contains a **startConditions** element which specify when an ad should begin to play.</span></span> <span data-ttu-id="f397a-210">**StartConditions** element obsahuje jeden nebo více <condition> elementy.</span><span class="sxs-lookup"><span data-stu-id="f397a-210">The **startConditions** element contains one or more <condition> elements.</span></span> <span data-ttu-id="f397a-211">Při každé <condition> vyhodnotí jako true aktivační událost se zahájí nebo odebrán, podle toho, jestli <condition> je obsažena v **startConditions** nebo **endConditions** element v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="f397a-211">When each <condition> evaluates to true a trigger is initiated or revoked depending upon whether the <condition> is contained within a **startConditions** or **endConditions** element respectively.</span></span> <span data-ttu-id="f397a-212">Když více <condition> elementy jsou přítomny, jsou považovány za implicitní nebo, aktivační událost zahájíte způsobí, že všechny podmínky vyhodnocení na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="f397a-212">When multiple <condition> elements are present, they are treated as an implicit OR, any condition evaluating to true will cause the trigger to initiate.</span></span> <span data-ttu-id="f397a-213"><condition>elementy mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="f397a-213"><condition> elements can be nested.</span></span> <span data-ttu-id="f397a-214">Když podřízené <condition> jsou přednastavení elementy, jsou považovány za implicitní a, všechny podmínky musí vyhodnotit na hodnotu true pro aktivační událost k zahájení.</span><span class="sxs-lookup"><span data-stu-id="f397a-214">When child <condition> elements are preset, they are treated as an implicit AND, all conditions must evaluate to true for the trigger to initiate.</span></span> <span data-ttu-id="f397a-215"><condition> Prvek obsahuje následující atributy, které definují podmínky:</span><span class="sxs-lookup"><span data-stu-id="f397a-215">The <condition> element contains the following attributes that define the condition:</span></span> 

1. <span data-ttu-id="f397a-216">**typ** – Určuje typ podmínky, události nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f397a-216">**type** – specifies the type of condition, event or property</span></span>
2. <span data-ttu-id="f397a-217">**název** – název vlastnosti nebo událost, která má použít při vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="f397a-217">**name** – the name of the property or event to be used during evaluation</span></span>
3. <span data-ttu-id="f397a-218">**Hodnota** – hodnota, která vlastnost vyhodnotí proti</span><span class="sxs-lookup"><span data-stu-id="f397a-218">**value** – the value that a property will be evaluated against</span></span>
4. <span data-ttu-id="f397a-219">**operátor** – operaci má použít při vyhodnocení: EQ (stejná), NEQ (není rovno), GTR (větší), GEQ (větší nebo rovna), LT (menší než), LEQ (je menší než nebo rovno), MOD (modulo)</span><span class="sxs-lookup"><span data-stu-id="f397a-219">**operator** – the operation to use during evaluation: EQ (equal), NEQ (not equal), GTR (greater), GEQ (greater or equal), LT (Less than), LEQ (less than or equal), MOD (modulo)</span></span>

<span data-ttu-id="f397a-220">**endConditions** také obsahovat <condition> elementy.</span><span class="sxs-lookup"><span data-stu-id="f397a-220">**endConditions** also contain <condition> elements.</span></span> <span data-ttu-id="f397a-221">Pokud je podmínka vyhodnocena jako true aktivační událost se resetuje. <trigger> Také obsahuje element <sources> element, který obsahuje jeden nebo více <source> elementy.</span><span class="sxs-lookup"><span data-stu-id="f397a-221">When a condition evaluates to true the trigger is reset.The <trigger> element also contains a <sources> element that contains one or more <source> elements.</span></span> <span data-ttu-id="f397a-222"><source> Elementy zadejte identifikátor URI odpovědi ad a typ odpovědi ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-222">The <source> elements define the URI to the ad response and the type of ad response.</span></span> <span data-ttu-id="f397a-223">V tomto příkladu je identifikátor URI zadána odpověď na velká.</span><span class="sxs-lookup"><span data-stu-id="f397a-223">In this example a URI is given to a VAST response.</span></span> 

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


### <a name="using-video-player-ad-interface-definition-vpaid"></a><span data-ttu-id="f397a-224">Pomocí definice rozhraní Video Player ve službě Active Directory (VPAID)</span><span class="sxs-lookup"><span data-stu-id="f397a-224">Using Video Player-Ad Interface Definition (VPAID)</span></span>
<span data-ttu-id="f397a-225">VPAID je rozhraní API pro povolení spustitelné ad jednotky ke komunikaci s přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="f397a-225">VPAID is an API for enabling executable ad units to communicate with a video player.</span></span> <span data-ttu-id="f397a-226">To umožňuje prostředí vysoce interaktivní ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-226">This allows highly interactive ad experiences.</span></span> <span data-ttu-id="f397a-227">Uživatel může komunikovat s ad a služby ad může reagovat na akce provedené v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f397a-227">The user can interact with the ad and the ad can respond to actions taken by the viewer.</span></span> <span data-ttu-id="f397a-228">Například ad může zobrazit tlačítka, která umožňují uživateli zobrazit další informace nebo delší verze služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-228">For example an ad may display buttons that allow the user to view more information or a longer version of the ad.</span></span> <span data-ttu-id="f397a-229">Přehrávání videa musí podporovat rozhraní API VPAID a spustitelné ad musí implementovat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f397a-229">The video player must support the VPAID API and the executable ad must implement the API.</span></span> <span data-ttu-id="f397a-230">Když přehrávač požadavky že ze serveru služby ad serveru ad může být použit velká odpovědi, která obsahuje VPAID ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-230">When a player requests an ad from an ad server the server may respond with a VAST response that contains a VPAID ad.</span></span>

<span data-ttu-id="f397a-231">Spustitelný soubor ad se vytvoří v kódu, který je třeba spustit v prostředí runtime například Adobe Flash™ nebo JavaScript, může být spuštěn ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f397a-231">An executable ad is created in code that must be executed in a runtime environment such as Adobe Flash™ or JavaScript that can be executed in a web browser.</span></span> <span data-ttu-id="f397a-232">Po návratu velká odpověď obsahující VPAID ad serveru služby ad hodnotu apiFramework atribut <MediaFile> element musí být "VPAID".</span><span class="sxs-lookup"><span data-stu-id="f397a-232">When an ad server returns a VAST response containing a VPAID ad, the value of the apiFramework attribute in the <MediaFile> element must be “VPAID”.</span></span> <span data-ttu-id="f397a-233">Tento atribut určuje, že obsahují ad je spustitelný soubor ad VPAID.</span><span class="sxs-lookup"><span data-stu-id="f397a-233">This attribute specifies that the contained ad is a VPAID executable ad.</span></span> <span data-ttu-id="f397a-234">Atribut type musí být nastaven na typ MIME ke spustitelnému souboru, například "application/x-shockwave-flash" nebo "application/x-javascript".</span><span class="sxs-lookup"><span data-stu-id="f397a-234">The type attribute must be set to the MIME type of the executable, such as “application/x-shockwave-flash” or “application/x-javascript”.</span></span> <span data-ttu-id="f397a-235">Následující fragment kódu ukazuje XML <MediaFile> element z velká odpověď obsahující spustitelné ad VPAID.</span><span class="sxs-lookup"><span data-stu-id="f397a-235">The following XML snippet shows the <MediaFile> element from a VAST response containing a VPAID executable ad.</span></span> 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>


<span data-ttu-id="f397a-236">Spustitelný soubor ad může být inicializována pomocí <AdParameters> v rámci <Linear> nebo <NonLinear> elementy v odpovědi na velká.</span><span class="sxs-lookup"><span data-stu-id="f397a-236">An executable ad can be initialized using the <AdParameters> element within the <Linear> or <NonLinear> elements in a VAST response.</span></span> <span data-ttu-id="f397a-237">Další informace o <AdParameters> elementu, najdete v části [velká 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="f397a-237">For more information on the <AdParameters> element, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span> <span data-ttu-id="f397a-238">Další informace o rozhraní API VPAID najdete v tématu [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span><span class="sxs-lookup"><span data-stu-id="f397a-238">For more information about the VPAID API, see [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span></span>

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a><span data-ttu-id="f397a-239">Implementace systému Windows nebo Windows Phone 8 hráče s podpora služby Ad</span><span class="sxs-lookup"><span data-stu-id="f397a-239">Implementing a Windows or Windows Phone 8 Player with Ad Support</span></span>
<span data-ttu-id="f397a-240">Platforma Microsoft média: Player Framework pro Windows 8 a Windows Phone 8 obsahuje kolekci ukázkové aplikace, které ukazují, jak implementovat přehrávání videa aplikace pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f397a-240">The Microsoft Media Platform: Player Framework for Windows 8 and Windows Phone 8 contains a collection of sample applications that show you how to implement a video player application using the framework.</span></span> <span data-ttu-id="f397a-241">Můžete si stáhnout přehrávač Framework a ukázky z [Player Framework pro Windows 8 a Windows Phone 8](https://playerframework.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="f397a-241">You can download the Player Framework and the samples from [Player Framework for Windows 8 and Windows Phone 8](https://playerframework.codeplex.com).</span></span>

<span data-ttu-id="f397a-242">Když otevřete řešení Microsoft.PlayerFramework.Xaml.Samples uvidíte počet složek v projektu.</span><span class="sxs-lookup"><span data-stu-id="f397a-242">When you open the Microsoft.PlayerFramework.Xaml.Samples solution you will see a number of folders within the project.</span></span> <span data-ttu-id="f397a-243">Inzerování složka obsahuje ukázkový kód, který je důležité pro vytvoření přehrávání videa s podpora služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-243">The Advertising folder contains the sample code relevant to creating a video player with ad support.</span></span> <span data-ttu-id="f397a-244">Uvnitř reklama složka je počet XAML nebo cs soubory, které ukazují, jak vložit reklamy jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f397a-244">Inside the Advertising folder is a number of XAML/cs files each of which show how to insert ads in a different way.</span></span> <span data-ttu-id="f397a-245">Následující seznam popisuje všechny:</span><span class="sxs-lookup"><span data-stu-id="f397a-245">The following list describes each:</span></span>

* <span data-ttu-id="f397a-246">AdPodPage.xaml ukazuje způsob zobrazení pod služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-246">AdPodPage.xaml Shows how to display an ad pod.</span></span>
* <span data-ttu-id="f397a-247">AdSchedulingPage.xaml ukazuje, jak při plánování služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f397a-247">AdSchedulingPage.xaml Shows how to schedule ads.</span></span>
* <span data-ttu-id="f397a-248">FreeWheelPage.xaml ukazuje, jak naplánování služby Active Directory pomocí modulu plug-in FreeWheel.</span><span class="sxs-lookup"><span data-stu-id="f397a-248">FreeWheelPage.xaml Shows how to use the FreeWheel plugin to schedule ads.</span></span>
* <span data-ttu-id="f397a-249">MastPage.xaml ukazuje, jak při plánování služby Active Directory pomocí souboru STOŽÁRŮ.</span><span class="sxs-lookup"><span data-stu-id="f397a-249">MastPage.xaml Shows how to schedule ads with a MAST file.</span></span>
* <span data-ttu-id="f397a-250">ProgrammaticAdPage.xaml ukazuje, jak programově naplánování služby Active Directory do video.</span><span class="sxs-lookup"><span data-stu-id="f397a-250">ProgrammaticAdPage.xaml Shows how to programmatically schedule ads into a video.</span></span>
* <span data-ttu-id="f397a-251">ScheduleClipPage.xaml ukazuje, jak naplánovat ad bez velká souboru.</span><span class="sxs-lookup"><span data-stu-id="f397a-251">ScheduleClipPage.xaml Shows how to schedule an ad without a VAST file.</span></span>
* <span data-ttu-id="f397a-252">VastLinearCompanionPage.xaml ukazuje, jak k vložení lineární a doprovodné ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-252">VastLinearCompanionPage.xaml Shows how to insert a linear and companion ad.</span></span>
* <span data-ttu-id="f397a-253">VastNonLinearPage.xaml ukazuje, jak vložit – lineární ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-253">VastNonLinearPage.xaml Shows how to insert a non-linear ad.</span></span>
* <span data-ttu-id="f397a-254">VmapPage.xaml ukazuje, jak zadat soubor VMAP služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f397a-254">VmapPage.xaml Shows how to specify ads with a VMAP file.</span></span>

<span data-ttu-id="f397a-255">Všechny tyto ukázky používá třídu Media Player definované rozhraní přehrávač.</span><span class="sxs-lookup"><span data-stu-id="f397a-255">Each of these samples uses the MediaPlayer class defined by the player framework.</span></span> <span data-ttu-id="f397a-256">Většina ukázek pomocí modulů plug-in, který přidat podporu pro různými formáty odpovědi ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-256">Most samples use plugins that add support for various ad response formats.</span></span> <span data-ttu-id="f397a-257">Ukázka ProgrammaticAdPage prostřednictvím kódu programu komunikuje s instancí Media Player.</span><span class="sxs-lookup"><span data-stu-id="f397a-257">The ProgrammaticAdPage sample programmatically interacts with a MediaPlayer instance.</span></span>

### <a name="adpodpage-sample"></a><span data-ttu-id="f397a-258">Ukázka AdPodPage</span><span class="sxs-lookup"><span data-stu-id="f397a-258">AdPodPage Sample</span></span>
<span data-ttu-id="f397a-259">Tato ukázka používá AdSchedulerPlugin definovat, kdy se mají zobrazit ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-259">This sample uses the AdSchedulerPlugin to define when to display an ad.</span></span> <span data-ttu-id="f397a-260">V tomto příkladu inzerování střední kumulativní oprava má být přehráván po 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="f397a-260">In this example a mid-roll advertisement is scheduled to be played after 5 seconds.</span></span> <span data-ttu-id="f397a-261">Pod ad (skupina služby Active Directory pro zobrazení v pořadí) je zadána v velká souboru, kterou vrátil server služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-261">The ad pod (a group of ads to display in order) is specified in a VAST file returned from an ad server.</span></span> <span data-ttu-id="f397a-262">Identifikátor URI pro velká souboru je uveden v <RemoteAdSource> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-262">The URI to the VAST file is specified in the <RemoteAdSource> element.</span></span>

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

<span data-ttu-id="f397a-263">Další informace o AdSchedulerPlugin najdete v tématu [inzerování v rámci Player ve Windows 8 a Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span><span class="sxs-lookup"><span data-stu-id="f397a-263">For more information about the AdSchedulerPlugin, see [Advertising in the Player Framework on Windows 8 and Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span></span>

### <a name="adschedulingpage"></a><span data-ttu-id="f397a-264">AdSchedulingPage</span><span class="sxs-lookup"><span data-stu-id="f397a-264">AdSchedulingPage</span></span>
<span data-ttu-id="f397a-265">Tato ukázka používá také AdSchedulerPlugin.</span><span class="sxs-lookup"><span data-stu-id="f397a-265">This sample also uses the AdSchedulerPlugin.</span></span> <span data-ttu-id="f397a-266">Naplánuje tři služby Active Directory, ad před, střední kumulativní ad a po vrácení ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-266">It schedules three ads, a pre-roll ad, a mid-roll ad, and a post-roll ad.</span></span> <span data-ttu-id="f397a-267">Zadaný identifikátor URI pro VAST u každé reklamy v <RemoteAdSource> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-267">The URI to the VAST for each ad is specified in a <RemoteAdSource> element.</span></span>

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


### <a name="freewheelpage"></a><span data-ttu-id="f397a-268">FreeWheelPage</span><span class="sxs-lookup"><span data-stu-id="f397a-268">FreeWheelPage</span></span>
<span data-ttu-id="f397a-269">Tato ukázka používá FreeWheelPlugin, který určuje zdrojový atribut, který určuje identifikátor URI, který ukazuje na soubor SmartXML, který určuje ad obsahu a také informace o plánování ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-269">This sample uses the FreeWheelPlugin which specifies a Source attribute that specifies a URI that points to a SmartXML file that specifies ad content as well as ad scheduling information.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a><span data-ttu-id="f397a-270">MastPage</span><span class="sxs-lookup"><span data-stu-id="f397a-270">MastPage</span></span>
<span data-ttu-id="f397a-271">Tato ukázka používá MastSchedulerPlugin, která umožňuje použít soubor STOŽÁRŮ.</span><span class="sxs-lookup"><span data-stu-id="f397a-271">This sample uses the MastSchedulerPlugin that allows you to use a MAST file.</span></span> <span data-ttu-id="f397a-272">Zdrojový atribut určuje umístění souboru STOŽÁRŮ.</span><span class="sxs-lookup"><span data-stu-id="f397a-272">The Source attribute specifies the location of the MAST file.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a><span data-ttu-id="f397a-273">ProgrammaticAdPage</span><span class="sxs-lookup"><span data-stu-id="f397a-273">ProgrammaticAdPage</span></span>
<span data-ttu-id="f397a-274">Tato ukázka prostřednictvím kódu programu komunikuje s Media Player.</span><span class="sxs-lookup"><span data-stu-id="f397a-274">This sample programmatically interacts with the MediaPlayer.</span></span> <span data-ttu-id="f397a-275">Soubor ProgrammaticAdPage.xaml vytvoří Media Player:</span><span class="sxs-lookup"><span data-stu-id="f397a-275">The ProgrammaticAdPage.xaml file instantiates the MediaPlayer:</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

<span data-ttu-id="f397a-276">Soubor ProgrammaticAdPage.xaml.cs vytvoří AdHandlerPlugin, přidá TimelineMarker zadat při ad mají být zobrazeny a potom přidá obslužné rutiny pro MarkerReached událost, která načte RemoteAdSource, určující identifikátor URI pro velká soubor a pak hraje ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-276">The ProgrammaticAdPage.xaml.cs file creates an AdHandlerPlugin, adds a TimelineMarker to specify when an ad should be displayed, and then adds a handler for the MarkerReached event which loads a RemoteAdSource specifying a URI to a VAST file, and then plays the ad.</span></span>

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

### <a name="scheduleclippage"></a><span data-ttu-id="f397a-277">ScheduleClipPage</span><span class="sxs-lookup"><span data-stu-id="f397a-277">ScheduleClipPage</span></span>
<span data-ttu-id="f397a-278">Tato ukázka používá AdSchedulerPlugin k naplánování střední kumulativní ad zadáním soubor .wmv, který obsahuje ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-278">This sample uses the AdSchedulerPlugin to schedule a mid-roll ad by specifying a .wmv file that contains the ad.</span></span>

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

### <a name="vastlinearcompanionpage"></a><span data-ttu-id="f397a-279">VastLinearCompanionPage</span><span class="sxs-lookup"><span data-stu-id="f397a-279">VastLinearCompanionPage</span></span>
<span data-ttu-id="f397a-280">Tato ukázka znázorňuje, jak naplánovat střední kumulativní lineární ad s ad doprovodné pomocí AdSchedulerPlugin.</span><span class="sxs-lookup"><span data-stu-id="f397a-280">This sample illustrates how to use the AdSchedulerPlugin to schedule a mid-roll linear ad with an companion ad.</span></span> <span data-ttu-id="f397a-281"><RemoteAdSource> Element určuje umístění souboru velká.</span><span class="sxs-lookup"><span data-stu-id="f397a-281">The <RemoteAdSource> element specifies the location of the VAST file.</span></span>

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

### <a name="vastlinearnonlinearpage"></a><span data-ttu-id="f397a-282">VastLinearNonLinearPage</span><span class="sxs-lookup"><span data-stu-id="f397a-282">VastLinearNonLinearPage</span></span>
<span data-ttu-id="f397a-283">Tato ukázka používá AdSchedulerPlugin při plánování lineární a -lineární ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-283">This sample uses the AdSchedulerPlugin to schedule a linear and a non-linear ad.</span></span> <span data-ttu-id="f397a-284">Umístění souboru velká zadaný <RemoteAdSource> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-284">The VAST file location is specified with the <RemoteAdSource> element.</span></span>

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

### <a name="vmappage"></a><span data-ttu-id="f397a-285">VMAPPage</span><span class="sxs-lookup"><span data-stu-id="f397a-285">VMAPPage</span></span>
<span data-ttu-id="f397a-286">Této ukázky používá k naplánování služby Active Directory pomocí souboru VMAP VmapSchedulerPlugin.</span><span class="sxs-lookup"><span data-stu-id="f397a-286">This samples uses the VmapSchedulerPlugin to schedule ads using a VMAP file.</span></span> <span data-ttu-id="f397a-287">Zadaný identifikátor URI k souboru VMAP v zdrojový atribut <VmapSchedulerPlugin> elementu.</span><span class="sxs-lookup"><span data-stu-id="f397a-287">The URI to the VMAP file is specified in the Source attribute of the <VmapSchedulerPlugin> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a><span data-ttu-id="f397a-288">Implementace iOS Video hráče s podpora služby Ad</span><span class="sxs-lookup"><span data-stu-id="f397a-288">Implementing an iOS Video Player with Ad Support</span></span>
<span data-ttu-id="f397a-289">Platforma Microsoft média: Architektura Player pro iOS obsahuje kolekci ukázkové aplikace, které ukazují, jak implementovat přehrávání videa aplikace pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f397a-289">The Microsoft Media Platform: Player Framework for iOS contains a collection of sample applications that show you how to implement a video player application using the framework.</span></span> <span data-ttu-id="f397a-290">Můžete si stáhnout přehrávač Framework a ukázky z [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="f397a-290">You can download the Player Framework and the samples from [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span></span> <span data-ttu-id="f397a-291">Stránku githubu obsahuje odkaz na Wiki, který obsahuje další informace o rozhraní player a úvod do ukázka player: [Wiki přehrávač médií Azure](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="f397a-291">The github page has a link to a Wiki that contains additional information on the player framework and an introduction to the player sample: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span></span>

### <a name="scheduling-ads-with-vmap"></a><span data-ttu-id="f397a-292">Plánování služby Active Directory s VMAP</span><span class="sxs-lookup"><span data-stu-id="f397a-292">Scheduling Ads with VMAP</span></span>
<span data-ttu-id="f397a-293">Následující příklad ukazuje, jak při plánování služby Active Directory pomocí souboru VMAP.</span><span class="sxs-lookup"><span data-stu-id="f397a-293">The following example shows how to schedule ads using a VMAP file.</span></span>

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a><span data-ttu-id="f397a-294">Plánování služby Active Directory s VAST</span><span class="sxs-lookup"><span data-stu-id="f397a-294">Scheduling Ads with VAST</span></span>
<span data-ttu-id="f397a-295">Následující příklad ukazuje, jak naplánovat pozdní vazby velká ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-295">The following sample shows how to schedule a late binding VAST ad.</span></span>

    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
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

   <span data-ttu-id="f397a-296">Následující příklad ukazuje, jak naplánovat ad pro velká časné vazby.</span><span class="sxs-lookup"><span data-stu-id="f397a-296">The following sample shows how to schedule an early binding VAST ad.</span></span>
<span data-ttu-id="f397a-297">Příklad: 4 plán časná //Download velká ad vazbu VAST souboru pokud (! [[ framework.adResolver downloadManifest: & manifestu withURL: [nsurl, který URLWithString: @"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[vlastní logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;</span><span class="sxs-lookup"><span data-stu-id="f397a-297">//Example:4 Schedule an early binding VAST ad //Download the VAST file if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) { [self logFrameworkError]; } else { adLinearTime.startTime = 7; adLinearTime.duration = 0;</span></span>

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

<span data-ttu-id="f397a-298">Následující příklad ukazuje, jak vložit ad pomocí hrubý vyjmout úpravy (RCE)</span><span class="sxs-lookup"><span data-stu-id="f397a-298">The following sample shows how to insert an ad using Rough Cut Editing (RCE)</span></span>

    //Example:1 How to use RCE.
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

<span data-ttu-id="f397a-299">Následující příklad ukazuje, jak naplánovat pod služby ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-299">The following example shows how to schedule an ad pod.</span></span>

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
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

<span data-ttu-id="f397a-300">Následující příklad ukazuje, jak naplánovat-rychlých ad střední vrácení.</span><span class="sxs-lookup"><span data-stu-id="f397a-300">The following example shows how to schedule a non-sticky mid-roll ad.</span></span> <span data-ttu-id="f397a-301">Rychlých ad pouze přehraje po bez ohledu na jakékoli vyhledávání se provádí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f397a-301">A non-sticky ad is only played once regardless of any seeking the viewer performs.</span></span>

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
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

<span data-ttu-id="f397a-302">Následující příklad ukazuje, jak naplánovat rychlých ad střední vrácení.</span><span class="sxs-lookup"><span data-stu-id="f397a-302">The following example shows how to schedule a sticky mid-roll ad.</span></span> <span data-ttu-id="f397a-303">Rychlých ad se zobrazí pokaždé, když je dosaženo Zadaný bod na video časovou osu.</span><span class="sxs-lookup"><span data-stu-id="f397a-303">A sticky ad will be displayed each time the specified point on the video timeline is reached.</span></span>

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
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


<span data-ttu-id="f397a-304">Následující příklad ukazuje, jak naplánovat po vrácení ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-304">The following sample shows how to schedule a post-roll ad.</span></span>

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

<span data-ttu-id="f397a-305">Následující příklad ukazuje, jak naplánovat před ad.</span><span class="sxs-lookup"><span data-stu-id="f397a-305">The following sample shows how to schedule a pre-roll ad.</span></span>

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

<span data-ttu-id="f397a-306">Následující příklad ukazuje, jak naplánovat ad střední kumulativní překrytí.</span><span class="sxs-lookup"><span data-stu-id="f397a-306">The following sample shows how to schedule a mid-roll overlay ad.</span></span>

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



## <a name="media-services-learning-paths"></a><span data-ttu-id="f397a-307">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f397a-307">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f397a-308">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f397a-308">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="f397a-309">Viz také</span><span class="sxs-lookup"><span data-stu-id="f397a-309">See Also</span></span>
[<span data-ttu-id="f397a-310">Vývoj aplikací videopřehrávače</span><span class="sxs-lookup"><span data-stu-id="f397a-310">Develop video player applications</span></span>](media-services-develop-video-players.md)

