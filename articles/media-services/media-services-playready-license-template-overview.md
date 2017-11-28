---
title: "Přehled šablonu licence PlayReady služby aaaMedia"
description: "Toto téma poskytuje přehled šablonu licence PlayReady, která používá licence technologie PlayReady tooconfigure."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 5a5ba930c56f70038db204681486ebc4308199fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-playready-license-template-overview"></a><span data-ttu-id="ee9df-103">Přehled šablonu licence Media Services PlayReady</span><span class="sxs-lookup"><span data-stu-id="ee9df-103">Media Services PlayReady license template overview</span></span>
<span data-ttu-id="ee9df-104">Azure Media Services nyní poskytuje službu k doručování licencí PlayReady společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ee9df-104">Azure Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="ee9df-105">Když se koncový uživatel player hello (například Silverlight) pokusí tooplay vaše PlayReady chráněný obsah, je požadavek odeslaný toohello licence doručení služby tooobtain licenci.</span><span class="sxs-lookup"><span data-stu-id="ee9df-105">When hello end user player (for example, Silverlight) tries tooplay your PlayReady protected content, a request is sent toohello license delivery service tooobtain a license.</span></span> <span data-ttu-id="ee9df-106">Pokud hello licenční služby schválí požadavek hello, vystavuje hello licenci, která je klient odeslané toohello a může být použité toodecrypt a play hello zadaný obsah.</span><span class="sxs-lookup"><span data-stu-id="ee9df-106">If hello license service approves hello request, it issues hello license which is sent toohello client and can be used toodecrypt and play hello specified content.</span></span>

<span data-ttu-id="ee9df-107">Služba Media Services také poskytuje rozhraní API, která umožňují nakonfigurujte své licence PlayReady.</span><span class="sxs-lookup"><span data-stu-id="ee9df-107">Media Services also provides APIs that let you configure your PlayReady licenses.</span></span> <span data-ttu-id="ee9df-108">Licence obsahovat hello práva a omezení, že chcete pro hello tooenforce runtime PlayReady DRM když uživatel se pokouší tooplayback chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="ee9df-108">Licenses contain hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplayback protected content.</span></span>
<span data-ttu-id="ee9df-109">Tady jsou některé příklady PlayReady licenční omezení, které můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="ee9df-109">Below are some examples of PlayReady license restrictions that you can specify:</span></span>

* <span data-ttu-id="ee9df-110">Hello DateTime, ze které hello je platná licence.</span><span class="sxs-lookup"><span data-stu-id="ee9df-110">hello DateTime from which hello license is valid.</span></span>
* <span data-ttu-id="ee9df-111">Hello hodnotu DateTime, když vyprší platnost licence hello.</span><span class="sxs-lookup"><span data-stu-id="ee9df-111">hello DateTime value when hello license expires.</span></span> 
* <span data-ttu-id="ee9df-112">Pro toobe licence hello ukládány do trvalého úložiště na hello klienta.</span><span class="sxs-lookup"><span data-stu-id="ee9df-112">For hello license toobe saved in persistent storage on hello client.</span></span> <span data-ttu-id="ee9df-113">Trvalé licence jsou obvykle používanými tooallow offline přehrávání obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="ee9df-113">Persistent licenses are typically used tooallow offline playback of hello content.</span></span>
* <span data-ttu-id="ee9df-114">Hello minimální úroveň zabezpečení, přehrávač musí mít tooplay svůj obsah.</span><span class="sxs-lookup"><span data-stu-id="ee9df-114">hello minimum security level that a player must have tooplay your content.</span></span> 
* <span data-ttu-id="ee9df-115">Hello výstupní úroveň ochrany pro ovládací prvky výstup hello audio\video obsahu.</span><span class="sxs-lookup"><span data-stu-id="ee9df-115">hello output protection level for hello output controls for audio\video content.</span></span> 
* <span data-ttu-id="ee9df-116">Další informace najdete v tématu ovládací prvky výstup hello část (3.5) v hello [pravidla dodržování předpisů PlayReady](https://www.microsoft.com/playready/licensing/compliance/) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ee9df-116">For more information, see hello Output Controls section (3.5) in hello [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>

> [!NOTE]
> <span data-ttu-id="ee9df-117">V současné době můžete konfigurovat pouze hello PlayRight licence PlayReady hello (toto právo je vyžadováno).</span><span class="sxs-lookup"><span data-stu-id="ee9df-117">Currently, you can only configure hello PlayRight of hello PlayReady license (this right is required).</span></span> <span data-ttu-id="ee9df-118">Hello PlayRight udává hello klienta hello možnost tooplayback hello obsah.</span><span class="sxs-lookup"><span data-stu-id="ee9df-118">hello PlayRight gives hello client hello ability tooplayback hello content.</span></span> <span data-ttu-id="ee9df-119">Hello PlayRight také umožňuje konfigurovat konkrétní tooplayback omezení.</span><span class="sxs-lookup"><span data-stu-id="ee9df-119">hello PlayRight also allows configuring restrictions specific tooplayback.</span></span> <span data-ttu-id="ee9df-120">Další informace najdete v tématu [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span><span class="sxs-lookup"><span data-stu-id="ee9df-120">For more information, see [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span></span>
> 
> 

<span data-ttu-id="ee9df-121">licence technologie PlayReady tooconfigure pomocí služby Media Services, je nutné nakonfigurovat hello šablona licence Media Services PlayReady.</span><span class="sxs-lookup"><span data-stu-id="ee9df-121">tooconfigure PlayReady licenses using Media Services, you must configure hello Media Services PlayReady license template.</span></span> <span data-ttu-id="ee9df-122">Hello šablona je definována v kódu XML.</span><span class="sxs-lookup"><span data-stu-id="ee9df-122">hello template is defined in XML.</span></span>

<span data-ttu-id="ee9df-123">Hello následující příklad ukazuje hello nejjednodušší (a nejběžnější) šablonu, která nakonfiguruje základní streamování licenci.</span><span class="sxs-lookup"><span data-stu-id="ee9df-123">hello following example shows hello simplest (and most common) template that configures a basic streaming license.</span></span> <span data-ttu-id="ee9df-124">Vaši klienti tuto licenci by být možné tooplayback vaše PlayReady chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="ee9df-124">With this license, your clients would be able tooplayback your PlayReady protected content.</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

<span data-ttu-id="ee9df-125">Hello XML vyhovuje toohello PlayReady licence šablony XML schéma definované v šablona licence PlayReady hello části schématu XML.</span><span class="sxs-lookup"><span data-stu-id="ee9df-125">hello XML conforms toohello PlayReady license template XML schema defined in hello PlayReady license template XML schema section.</span></span>

<span data-ttu-id="ee9df-126">Služba Media Services také definuje sadu tříd rozhraní .NET, které by mohly být použité tooserialized a deserializovat tooand z hello XML.</span><span class="sxs-lookup"><span data-stu-id="ee9df-126">Media Services also defines a set of .NET classes that could be used tooserialized and deserialized tooand from hello XML.</span></span> <span data-ttu-id="ee9df-127">Popis hlavní třídy naleznete v tématu [Media Services .NET třídy](media-services-playready-license-template-overview.md#classes).</span><span class="sxs-lookup"><span data-stu-id="ee9df-127">For description of main classes, see [Media Services .NET classes](media-services-playready-license-template-overview.md#classes).</span></span> <span data-ttu-id="ee9df-128">šablony použité tooconfigure licencí, které jsou.</span><span class="sxs-lookup"><span data-stu-id="ee9df-128">that are used tooconfigure license templates.</span></span>

<span data-ttu-id="ee9df-129">Příklad klient server, který používá rozhraní .NET třídy šablona licence PlayReady hello tooconfigure, najdete v části [pomocí dynamického šifrování PlayReady a službu doručování licencí](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="ee9df-129">For an end-to-end example that uses .NET classes tooconfigure hello PlayReady license template, see [Using PlayReady Dynamic Encryption and License Delivery Service](media-services-protect-with-drm.md).</span></span>

## <span data-ttu-id="ee9df-130"><a id="classes"></a>Media Services .NET třídy, které jsou šablony používané tooconfigure licencí</span><span class="sxs-lookup"><span data-stu-id="ee9df-130"><a id="classes"></a>Media Services .NET classes that are used tooconfigure license templates</span></span>
<span data-ttu-id="ee9df-131">Následující Hello jsou hello hlavní rozhraní .NET třídy jsou používané tooconfigure Media Services PlayReady licence šablony.</span><span class="sxs-lookup"><span data-stu-id="ee9df-131">hello following are hello main .NET classes are used tooconfigure Media Services PlayReady license templates.</span></span> <span data-ttu-id="ee9df-132">Tyto třídy map toohello typy definované v [schématu XML šablony licence PlayReady](media-services-playready-license-template-overview.md#schema).</span><span class="sxs-lookup"><span data-stu-id="ee9df-132">These classes map toohello types defined in [PlayReady license template XML schema](media-services-playready-license-template-overview.md#schema).</span></span>

<span data-ttu-id="ee9df-133">Hello [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) třída je použité tooserialize a deserializovat tooand z šablony licencí Media Services hello XML.</span><span class="sxs-lookup"><span data-stu-id="ee9df-133">hello [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) class is used tooserialize and deserialize tooand from hello Media Services license template XML.</span></span>

### <a name="playreadylicenseresponsetemplate"></a><span data-ttu-id="ee9df-134">PlayReadyLicenseResponseTemplate</span><span class="sxs-lookup"><span data-stu-id="ee9df-134">PlayReadyLicenseResponseTemplate</span></span>
<span data-ttu-id="ee9df-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) -Tato třída reprezentuje hello šablony pro odeslané odpovědi hello back toohello koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="ee9df-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - This class represents hello template for hello response sent back toohello end user.</span></span> <span data-ttu-id="ee9df-136">Obsahuje pole, pro vlastní data řetězec o délce hello licenčního serveru a aplikace hello (může být užitečná pro vlastní aplikaci logiky), jakož i seznam jednu nebo více šablon licence.</span><span class="sxs-lookup"><span data-stu-id="ee9df-136">It contains a field for a custom data string between hello license server and hello application (may be useful for custom app logic) as well as a list of one or more license templates.</span></span>

<span data-ttu-id="ee9df-137">Toto je třída "nejvyšší úrovně" hello v hierarchii šablony hello.</span><span class="sxs-lookup"><span data-stu-id="ee9df-137">This is hello “top level” class in hello template hierarchy.</span></span> <span data-ttu-id="ee9df-138">Což znamená, že šablona hello odpovědi obsahuje seznam šablon licencí, které zahrnují šablony licence hello (přímo ani nepřímo) všechny hello jiné třídy, které tvoří hello šablony dat toobe serializovat.</span><span class="sxs-lookup"><span data-stu-id="ee9df-138">Meaning that hello response template includes a list of license templates and hello license templates include (directly or indirectly) all of hello other classes that make up hello template data toobe serialized.</span></span>

### <a name="playreadylicensetemplate"></a><span data-ttu-id="ee9df-139">PlayReadyLicenseTemplate</span><span class="sxs-lookup"><span data-stu-id="ee9df-139">PlayReadyLicenseTemplate</span></span>
<span data-ttu-id="ee9df-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) -hello třída reprezentuje šablonu licence pro vytváření toobe licence PlayReady vrátil toohello koncovým uživatelům.</span><span class="sxs-lookup"><span data-stu-id="ee9df-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - hello class represents a license template for creating PlayReady licenses toobe returned toohello end users.</span></span> <span data-ttu-id="ee9df-141">Obsahuje data hello na klíč obsahu hello v hello licenci a žádné oprávnění nebo omezení toobe vynuceny hello runtime PlayReady DRM při použití hello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="ee9df-141">It contains hello data on hello content key in hello license and any rights or restrictions toobe enforced by hello PlayReady DRM runtime when using hello content key.</span></span>

### <span data-ttu-id="ee9df-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span><span class="sxs-lookup"><span data-stu-id="ee9df-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span></span>
<span data-ttu-id="ee9df-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) -Tato třída reprezentuje hello PlayRight licence PlayReady.</span><span class="sxs-lookup"><span data-stu-id="ee9df-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - This class represents hello PlayRight of a PlayReady license.</span></span> <span data-ttu-id="ee9df-144">Udělí hello uživatele hello možnost tooplayback hello obsahu subjektu toohello nula nebo více omezení nakonfigurovaná v hello licenci a na hello PlayRight samotné (pro přehrávání specifičtější zásady).</span><span class="sxs-lookup"><span data-stu-id="ee9df-144">It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions configured in hello license and on hello PlayRight itself (for playback specific policy).</span></span> <span data-ttu-id="ee9df-145">Velká část hello zásady na hello PlayRight má toodo s výstup omezení, které řídí hello typy výstupů, které hello obsah může být přehráván přes a nějaká omezení, které musí být použity při pomocí daného výstup.</span><span class="sxs-lookup"><span data-stu-id="ee9df-145">Much of hello policy on hello PlayRight has toodo with output restrictions which control hello types of outputs that hello content can be played over and any restrictions that must be put in place when using a given output.</span></span> <span data-ttu-id="ee9df-146">Například pokud hello DigitalVideoOnlyContentRestriction je povoleno, pak hello DRM runtime povolí pouze hello video toobe zobrazí přes digitální výstupy (analogovým video výstupy nebudou povolena, toopass hello obsahu).</span><span class="sxs-lookup"><span data-stu-id="ee9df-146">For example, if hello DigitalVideoOnlyContentRestriction is enabled, then hello DRM runtime will only allow hello video toobe displayed over digital outputs (analog video outputs won’t be allowed toopass hello content).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee9df-147">Tyto typy omezení, můžou být velmi mocné ale může ovlivnit také hello uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="ee9df-147">These types of restrictions can be very powerful but can also affect hello consumer experience.</span></span> <span data-ttu-id="ee9df-148">Pokud ochranu výstup hello nakonfigurovaných příliš omezující, může být možné přehrát na někteří klienti hello obsah.</span><span class="sxs-lookup"><span data-stu-id="ee9df-148">If hello output protections are configured too restrictive, hello content might be unplayable on some clients.</span></span> <span data-ttu-id="ee9df-149">Další informace najdete v tématu hello [pravidla dodržování předpisů PlayReady](https://www.microsoft.com/playready/licensing/compliance/) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ee9df-149">For more information, see hello [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>
> 
> 

<span data-ttu-id="ee9df-150">Příklad jaké ochrany úrovně Silverlight podporuje, naleznete v části: [Silverlight podporu pro ochranu výstup](http://go.microsoft.com/fwlink/?LinkId=617318).</span><span class="sxs-lookup"><span data-stu-id="ee9df-150">For an example of what protection levels Silverlight supports, see: [Silverlight support for output protections](http://go.microsoft.com/fwlink/?LinkId=617318).</span></span>

## <span data-ttu-id="ee9df-151"><a id="schema"></a>Schéma XML šablony licence PlayReady</span><span class="sxs-lookup"><span data-stu-id="ee9df-151"><a id="schema"></a>PlayReady license template XML schema</span></span>
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



## <a name="media-services-learning-paths"></a><span data-ttu-id="ee9df-152">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="ee9df-152">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ee9df-153">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ee9df-153">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

