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
# <a name="inserting-ads-on-hello-client-side"></a>Vkládání reklam na straně klienta hello
Toto téma obsahuje informace o tom, tooinsert různých typů reklamy na straně klienta hello.

Informace o podpoře uzavřené přidávání titulků a ad v živé streamování videa najdete v tématu [podporované titulky a standardy vložení Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Azure Media Player v současné době nepodporuje reklamy.
> 
> 

## <a id="insert_ads_into_media"></a>Vkládání reklamy do vašeho média
Azure Media Services poskytuje podporu pro vkládání reklam prostřednictvím hello platformu Windows Media: architektury přehrávačů. Architektury přehrávačů s podporou ad jsou dostupné pro zařízení s Windows 8, Silverlight, Windows Phone 8 a iOS. Každý player framework obsahuje ukázkový kód, který ukazuje, jak tooimplement aplikace přehrávače. Existují tři různé druhy služby Active Directory, které lze vložit do seznamu média: seznam.

* **Lineární** – úplné rámce služby Active Directory, které pozastavit hlavní video hello.
* **Nelineární** – reklamy překrytí, které se zobrazují jako přehrávání videa hlavní hello, obvykle logo nebo jiné statický obrázek umístit hello přehrávač.
* **Doprovodná** – reklamy, které se zobrazují mimo hello přehrávač.

Služby Active Directory mohou být umístěny v libovolném bodě hello video hlavní časovou osu. Se musí zjistit hello player při tooplay hello ad a které tooplay služby Active Directory. To se provádí pomocí sady standardní soubory formátu XML: Video Ad služby šablony (VAST), více Ad seznam stop (VMAP) ve digitální Video, šablony abstraktní sekvencování na média (STOŽÁRŮ) a digitální Video Player Ad rozhraní definice (VPAID). VELKÁ soubory zadejte co toodisplay služby Active Directory. Soubory VMAP určete, kdy tooplay různé reklamy a obsahovat velká XML. STOŽÁRŮ soubory jsou jiný způsob toosequence reklamy, které také může obsahovat velká XML. Soubory VPAID definovat rozhraní mezi hello přehrávání videa a hello ad nebo serveru služby ad.

Každý player framework funguje jinak, každý bude uvedena v jeho vlastní tématu. Toto téma popisuje hello základní mechanismy, které používá tooinsert služby Active Directory. Aplikací pro přehrávání videa žádost služby Active Directory ze serveru služby ad. server služby ad Hello může reagovat v mnoha různými způsoby:

* Vrátí soubor velká
* Vrátí VMAP soubor (s embedded VAST)
* Vrátit STOŽÁRŮ soubor (s embedded VAST)
* Vrátí velká soubor s VPAID služby Active Directory

### <a name="using-a-video-ad-service-template-vast-file"></a>Pomocí souboru šablony (VAST) služby Video Ad
Soubor velká Určuje, jaké ad nebo toodisplay služby Active Directory. Hello následující kód XML je příkladem soubor velká pro lineární ad:

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

Lineární ad Hello je popsán hello <**lineární**> elementu. Určuje dobu trvání hello hello ad, sledování událostí, klikněte na tlačítko prostřednictvím, klikněte na sledování a několika **MediaFile** elementy. Sledování události jsou určené v rámci hello <**TrackingEvents**> elementu a povolit služby ad serveru tootrack různých událostí, ke kterým došlo při prohlížení hello ad. V takovém případě hello start a středový, dokončení a rozbalte události se sledují. Hello počáteční události dojde, když se zobrazí hello ad. Hello středový události dojde, když alespoň, že byl zobrazen 50 % hello ad osy. událost complete Hello nastane, když hello ad byla spuštěna toohello end. Hello rozšířené události dojde, když uživatel hello rozbalí úvodní obrazovka toofull přehrávání videa. Se zadaným Clickthroughs <**interaktivní**> v rámci <**VideoClicks**> elementu a určuje toodisplay prostředků tooa identifikátoru URI při hello uživatel klikne na hello ad. ClickTracking je uveden v <**ClickTracking**> element také v rámci hello <**VideoClicks**> elementu a určuje sledování prostředků pro hello player toorequest při kliknutí hello na hello ad.hello <**MediaFile**> elementy zadejte informace o konkrétní kódování ad. Když je více než jedna <**MediaFile**> elementu přehrávání videa hello můžete zvolit nejvhodnější kódování hello pro platformu hello. 

Lineární reklamy lze zobrazit v zadaném pořadí. toodo, přidejte další <Ad> elementy toohello VAST souboru a zadejte hello pořadí pomocí hello atribut sekvence. To ukazuje následující příklad Hello:

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

Nelineární reklamy jsou určené v <Creative> také element. Následující příklad ukazuje Hello <Creative> element, který popisuje nelineární ad.

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


Hello <**NonLinearAds**> element může obsahovat jednu nebo více <**NonLinear**> prvky, z nichž každý lze popsat nelineární ad. Hello <**NonLinear**> element určuje hello prostředků pro nelineární ad hello. Hello prostředek může být <**StaticResouce**>, <**IFrameResource**>, nebo <**HTMLResouce**>. <**StaticResource**> popisuje prostředků jiného typu než HTML a definuje creativeType atribut, který určuje, jak se zobrazí hello prostředků:

Bitové kopie nebo gif, image nebo jpeg, image nebo png – hello prostředků se zobrazí v kódu HTML <**img**> značky.

Application/x-javascript – hello prostředků se zobrazí v kódu HTML <**skriptu**> značky.

Application/x-shockwave-flash – hello prostředků se zobrazí v Flash player.

**IFrameResource** popisuje prostředek HTML, který lze zobrazit v elementu IFrame. **HTMLResource** popisuje kód HTML, který lze vložit do webové stránky. **TrackingEvents** zadejte sledování událostí a hello URI toorequest při výskytu události hello. V této ukázkové hello jsou sledovány acceptInvitation a sbalit události. Další informace o hello **NonLinearAds** elementu a jeho podřízených položek, najdete v části IAB.NET/VAST. Všimněte si, že hello **TrackingEvents** element se nachází v rámci hello **NonLinearAds** prvek spíše než hello **NonLinear** element.

Doprovodná reklamy jsou definovány v rámci <CompanionAds> elementu. Hello <CompanionAds> element může obsahovat jednu nebo více <Companion> elementy. Každý <Companion> element popisuje doprovodné ad a může obsahovat <StaticResource>, <IFrameResource>, nebo <HTMLResource> které jsou určené v hello stejným způsobem jako v nelineárních ad. VELKÁ soubor může obsahovat více doprovodných reklamy a aplikace přehrávače hello můžete zvolit nejvhodnější ad toodisplay hello. Další informace o VAST najdete v tématu [velká 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Použití více soubor seznamu stop (VMAP) Ad digitální Video
Soubor VMAP vám umožní toospecify, když dojde k zalomení ad, jak dlouho je každý konec, kolik reklamy, může se zobrazit v rámci zalomení a jaké typy služby Active Directory může být při zalomení zobrazovat. Hello následující v soubor VMAP příklad, který definuje zalomení jeden ad:

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

Soubor VMAP začíná <VMAP> element, který obsahuje jeden nebo více <AdBreak> elementy, každý definování přerušení služby ad. Každý ad zalomení Určuje typ ukončení, pozastavení ID a posun času. Určuje typ hello ad, která může být přehráván během pozastavení hello Hello breakType atribut: lineární, nelineární, nebo zobrazení. Zobrazení reklam mapovat tooVAST doprovodné reklamy. Více než jeden typ ad můžete zadat seznam oddělený čárkami (bez mezer). Hello breakID je volitelné identifikátor pro hello ad. Hello timeOffset Určuje, kdy má být zobrazena hello ad. Určit lze v jednom z následujících způsobů hello:

1. Čas – ve formátu hh: mm: nebo hh:mm:ss.mmm kde .mmm je milisekundách. Hodnota tohoto atributu Hello Určuje dobu hello z hello začátku hello video časová osa toohello začátku hello ad přerušení.
2. Procento – ve formátu n %, kde n je procento hello tooplay hello video časová osa před přehrávání hello ad
3. Počáteční nebo koncové – Určuje, že ad by měla zobrazit před nebo po hello video byla zobrazena.
4. Pozice – určuje pořadí hello ad zalomení při hello načasování zalomení ad hello je neznámý, například v živé vysílání datového proudu. Hello pořadí od konce každé ad zadané ve formátu hello #n, kde n je celé číslo větší nebo 1. 1 znamená, že neexistuje hello ad by měla být přehráván při první příležitosti hello 2 označuje hello ad by měla být přehráván při hello druhý příležitosti a tak dále.

V rámci hello <**AdBreak**> existuje element může být jedna <**AdSource**> elementu. Hello <**AdSource**> element obsahuje hello následující atributy:

1. ID – Určuje identifikátor zdroje ad hello
2. allowMultipleAds – logická hodnota, která určuje, zda lze zobrazit více reklamy během pozastavení ad hello
3. followRedirects – volitelné logickou hodnotu, která určuje, pokud by měl respektovat přehrávání videa hello přesměruje v rámci odpověď ad

Hello <**AdSource**> element poskytuje hello player odpověď ad vložené nebo odpověď ad tooan odkaz. Může obsahovat jednu z hello následující prvky:

* <VASTAdData>Označuje, že odpověď velká ad vložené v souboru VMAP hello
* <AdTagURI>identifikátor URI, který odkazuje na odpověď ad z jiného systému
* <CustomAdData>-an libovolný řetězec této respresents-velká odpověď

V tomto příkladu je definován odpověď ad v řádku s <VASTAdData> element, který obsahuje odpověď velká ad. Další informace o hello najdete v části Další prvky [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Hello <**AdBreak**> element může také obsahovat jednu <**TrackingEvents**> elementu. Hello <**TrackingEvents**> element umožňuje tootrack hello počáteční nebo koncový přerušení služby ad nebo zda došlo k chybě během pozastavení ad hello. Hello <**TrackingEvents**> element obsahuje jeden nebo více <**sledování**> prvky, z nichž každý určuje sledování událostí a sledování identifikátor URI. jsou Hello možné sledování události:

1. breakStart – sleduje hello začátku přerušení služby ad
2. breakEnd – dokončení hello sledovat přerušení služby ad
3. Chyba – sleduje chybu, ke které došlo k chybě během pozastavení ad hello

Hello následující příklad ukazuje VMAP soubor, který určuje sledování událostí

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

Další informace o hello <**TrackingEvents**> elementu a jeho podřízených položek, najdete v části http://iab.org/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Pomocí médií abstraktní, pořadí úloh souboru šablony (STOŽÁRŮ)
Soubor STOŽÁRŮ umožňuje toospecify aktivační události, které definují, kdy se má zobrazit ad. Hello následující příklad je STOŽÁRŮ soubor obsahující aktivačních událostí pro před kumulativní ad, střední kumulativní ad a po vrácení ad.

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



Soubor STOŽÁRŮ začíná **STOŽÁRŮ** element, který obsahuje jeden **aktivační události** elementu. Hello <triggers> element obsahuje jeden nebo více **aktivační událost** prvky, které definují, kdy by měly být přehrány ad. 

Hello **aktivační událost** obsahuje element **startConditions** element, který zadat, když ad by měl začínat tooplay. Hello **startConditions** element obsahuje jeden nebo více <condition> elementy. Při každé <condition> vyhodnotí tootrue aktivační procedury je zahájeno nebo odebrán, podle toho, jestli se hello <condition> je obsažena v **startConditions** nebo **endConditions** – element v uvedeném pořadí. Když více <condition> elementy jsou přítomny, jsou považovány za implicitní OR, způsobí, že všechny podmínky vyhodnocení tootrue tooinitiate hello aktivační události. <condition>elementy mohou být použity. Když podřízené <condition> jsou přednastavení elementy, jsou považovány za implicitní a, všechny podmínky se musí vyhodnotit tootrue pro tooinitiate hello aktivační události. Hello <condition> prvek obsahuje následující atributy, které definují podmínky hello hello: 

1. **typ** – Určuje typ hello podmínku, události nebo vlastnosti
2. **název** – hello název vlastnosti nebo události toobe hello používá během vyhodnocení
3. **Hodnota** – hello hodnotu, která vlastnost vyhodnotí proti
4. **operátor** – hello operaci toouse během vyhodnocení: EQ (stejná), NEQ (není rovno), GTR (větší), GEQ (větší nebo rovna), LT (menší než), LEQ (je menší než nebo rovno), MOD (modulo)

**endConditions** také obsahovat <condition> elementy. Pokud je podmínka vyhodnocena jako aktivační událost hello tootrue není reset.hello <trigger> také obsahuje element <sources> element, který obsahuje jeden nebo více <source> elementy. Hello <source> elementy definovat hello URI toohello ad odpovědi a hello typ odpovědi ad. V tomto příkladu je zadána identifikátoru URI odpovědi tooa velká. 

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


### <a name="using-video-player-ad-interface-definition-vpaid"></a>Pomocí definice rozhraní Video Player ve službě Active Directory (VPAID)
VPAID je rozhraní API pro povolení toocommunicate jednotky spustitelné ad s přehrávání videa. To umožňuje prostředí vysoce interaktivní ad. uživatel Hello komunikovat s hello ad a hello ad může reagovat tooactions provedenou hello prohlížeč. Například ad může zobrazit tlačítka, která umožňují tooview hello uživatele Další informace nebo delší verzi hello ad. přehrávání videa Hello musí podporovat hello VPAID rozhraní API a hello spustitelné ad musí implementovat rozhraní API hello. Když přehrávač požadavků ad serveru služby ad serveru hello odpoví velká odpovědi, která obsahuje VPAID ad.

Spustitelný soubor ad se vytvoří v kódu, který je třeba spustit v prostředí runtime například Adobe Flash™ nebo JavaScript, může být spuštěn ve webovém prohlížeči. Po návratu velká odpověď obsahující VPAID ad serveru služby ad hello hodnotu atributu apiFramework hello hello <MediaFile> element musí být "VPAID". Tento atribut určuje, že tuto reklamu hello obsažené je spustitelný soubor ad VPAID. Hello typ musí být nastaven typ MIME toohello hello spustitelného souboru, například "application/x-shockwave-flash" nebo "application/x-javascript". Hello následující fragment kódu XML zobrazuje hello <MediaFile> element z velká odpověď obsahující spustitelné ad VPAID. 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI tooexecutable ad -->
       </MediaFile>
    </MediaFiles>


Spustitelný soubor ad může být inicializována pomocí hello <AdParameters> v rámci hello <Linear> nebo <NonLinear> elementy v odpovědi na velká. Další informace o hello <AdParameters> elementu, najdete v části [velká 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Další informace o hello VPAID API najdete v tématu [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementace systému Windows nebo Windows Phone 8 hráče s podpora služby Ad
Hello média platformy společnosti Microsoft: Player Framework pro Windows 8 a Windows Phone 8 obsahuje kolekci ukázkové aplikace, které ukazují, jak tooimplement aplikace přehrávání videa pomocí hello framework. Si můžete stáhnout přehrávač Framework a hello ukázky hello z [Player Framework pro Windows 8 a Windows Phone 8](https://playerframework.codeplex.com).

Když otevřete řešení Microsoft.PlayerFramework.Xaml.Samples hello uvidíte počet složek v rámci projektu hello. Hello inzerování složka obsahuje hello ukázkový kód relevantní toocreating přehrávání videa s podpora služby ad. Uvnitř hello inzerování složka je, jak počet XAML nebo cs každý z které zobrazit soubory reklamy tooinsert jiným způsobem. Hello následující seznam popisuje každý:

* AdPodPage.xaml ukazuje, jak pod toodisplay ad.
* Zobrazuje AdSchedulingPage.xaml jak tooschedule reklamy.
* FreeWheelPage.xaml ukazuje, jak toouse hello FreeWheel modulu plug-in tooschedule služby Active Directory.
* Zobrazuje MastPage.xaml jak tooschedule reklam s STOŽÁRŮ souboru.
* ProgrammaticAdPage.xaml ukazuje, jak naplánovat tooprogrammatically reklamy do video.
* Zobrazuje ScheduleClipPage.xaml jak tooschedule ad bez velká souboru.
* Zobrazuje VastLinearCompanionPage.xaml jak tooinsert lineární a doprovodné ad.
* Zobrazuje VastNonLinearPage.xaml jak tooinsert – lineární ad.
* Zobrazuje VmapPage.xaml jak toospecify reklam s VMAP souboru.

Všechny tyto ukázky používá třídu Media Player hello definované hello player framework. Většina ukázek pomocí modulů plug-in, který přidat podporu pro různými formáty odpovědi ad. Ukázka ProgrammaticAdPage Hello prostřednictvím kódu programu komunikuje s instancí Media Player.

### <a name="adpodpage-sample"></a>Ukázka AdPodPage
Tato ukázka používá hello AdSchedulerPlugin toodefine při toodisplay ad. V tomto příkladu je střední kumulativní inzerování naplánované toobe přehrávají po 5 sekund. pod ad Hello (skupina služby Active Directory toodisplay v pořadí) je zadána v velká souboru, kterou vrátil server služby ad. Hello URI toohello velká soubor je zadán v hello <RemoteAdSource> elementu.

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

Další informace o hello AdSchedulerPlugin najdete v tématu [inzerování v hello Player rozhraní Windows 8 a Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Tato ukázka používá také hello AdSchedulerPlugin. Naplánuje tři služby Active Directory, ad před, střední kumulativní ad a po vrácení ad. Hello URI toohello VAST pro každou reklamu je uveden v <RemoteAdSource> elementu.

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


### <a name="freewheelpage"></a>FreeWheelPage
Tato ukázka používá hello FreeWheelPlugin, který určuje zdrojový atribut, který určuje URI souboru SmartXML tooa body, které určuje ad obsah a také informace o plánování ad.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a>MastPage
Tato ukázka používá hello MastSchedulerPlugin, který vám umožní toouse STOŽÁRŮ souboru. Zdrojový atribut Hello určuje hello umístění souboru STOŽÁRŮ hello.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a>ProgrammaticAdPage
Tato ukázka prostřednictvím kódu programu komunikuje s hello Media Player. soubor ProgrammaticAdPage.xaml Hello vytvoří hello Media Player:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

soubor ProgrammaticAdPage.xaml.cs Hello vytvoří AdHandlerPlugin, při ad mají být zobrazeny a potom přidá obslužné rutiny pro hello MarkerReached událost, která načte RemoteAdSource, zadání soubor velká tooa URI a potom hraje přidává TimelineMarker toospecify Hello ad.

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

### <a name="scheduleclippage"></a>ScheduleClipPage
Tato ukázka používá hello AdSchedulerPlugin tooschedule ad střední kumulativní zadáním soubor .wmv, který obsahuje hello ad.

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

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
Tato ukázka znázorňuje, jak toouse hello AdSchedulerPlugin tooschedule střední kumulativní lineární ad s doprovodné ad. Hello <RemoteAdSource> element určuje umístění hello velká souboru hello.

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

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
Tato ukázka používá hello AdSchedulerPlugin tooschedule lineární a -lineární ad. Hello umístění velká souboru je definován s hello <RemoteAdSource> elementu.

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

### <a name="vmappage"></a>VMAPPage
Této ukázky používá hello VmapSchedulerPlugin tooschedule reklam pomocí souboru VMAP. Hello URI toohello VMAP soubor je zadán v hello zdrojový atribut hello <VmapSchedulerPlugin> elementu.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Implementace iOS Video hráče s podpora služby Ad
Hello média platformy společnosti Microsoft: architektura Player pro iOS obsahuje kolekci ukázkové aplikace, které ukazují, jak tooimplement aplikace přehrávání videa pomocí hello framework. Si můžete stáhnout přehrávač Framework a hello ukázky hello z [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Hello github stránce má odkaz tooa Wiki, který obsahuje další informace o hello player framework a úvod toohello player ukázková: [Wiki přehrávač médií Azure](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>Plánování služby Active Directory s VMAP
Následující příklad ukazuje, jak Hello tooschedule služby Active Directory pomocí souboru VMAP.

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

### <a name="scheduling-ads-with-vast"></a>Plánování služby Active Directory s VAST
Hello následující ukázka ukazuje, jak tooschedule ad velká pozdní vazby.

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

   Hello následující ukázka ukazuje, jak tooschedule ad pro velká časné vazby.
Příklad: 4 plán časné vazby velká ad //Download hello VAST souboru pokud (! [[ framework.adResolver downloadManifest: & manifestu withURL: [nsurl, který URLWithString: @"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[vlastní logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;

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

Hello následující ukázka ukazuje, jak tooinsert ad pomocí hrubý vyjmout úpravy (RCE)

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

Hello následující příklad ukazuje, jak pod tooschedule ad.

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

Následující příklad ukazuje, jak Hello tooschedule-rychlých ad střední vrácení. Rychlých ad pouze přehraje po bez ohledu na jakékoli požádat o hello prohlížeč provede.

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

Následující příklad ukazuje, jak Hello tooschedule rychlých ad střední vrácení. Rychlých ad se zobrazí pokaždé, když bod v časové osy video hello je dosaženo specifikovaného hello.

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


Hello následující ukázka ukazuje, jak tooschedule po vrácení ad.

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

Hello následující ukázka ukazuje, jak tooschedule před ad.

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

Následující ukázka Hello ukazuje, jak tooschedule střední kumulativní překrytí ad.

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



## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Vývoj aplikací videopřehrávače](media-services-develop-video-players.md)

