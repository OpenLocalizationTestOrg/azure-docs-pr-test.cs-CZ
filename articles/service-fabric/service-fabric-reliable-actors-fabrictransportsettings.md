---
title: "Změna nastavení FabricTransport v Azure mikroslužeb | Microsoft Docs"
description: "Další informace o konfiguraci nastavení komunikace objektu actor Azure Service Fabric."
services: Service-Fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: 75bdd4644f4ccc583271b9169c50a375e2cd6629
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-fabrictransport-settings-for-reliable-actors"></a><span data-ttu-id="992d2-103">Konfigurace nastavení FabricTransport Reliable actors</span><span class="sxs-lookup"><span data-stu-id="992d2-103">Configure FabricTransport settings for Reliable Actors</span></span>

<span data-ttu-id="992d2-104">Zde jsou nastavení, která můžete konfigurovat:</span><span class="sxs-lookup"><span data-stu-id="992d2-104">Here are the settings that you can configure:</span></span>
- <span data-ttu-id="992d2-105">C#: [FabricTransportRemotingSettings](
https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span><span class="sxs-lookup"><span data-stu-id="992d2-105">C#: [FabricTransportRemotingSettings](
https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span></span>
- <span data-ttu-id="992d2-106">Java: [FabricTransportRemotingSettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span><span class="sxs-lookup"><span data-stu-id="992d2-106">Java: [FabricTransportRemotingSettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span></span>

<span data-ttu-id="992d2-107">Můžete upravit výchozí konfigurace FabricTransport následujícími způsoby.</span><span class="sxs-lookup"><span data-stu-id="992d2-107">You can modify the default configuration of FabricTransport in following ways.</span></span>

## <a name="assembly-attribute"></a><span data-ttu-id="992d2-108">Atribut sestavení</span><span class="sxs-lookup"><span data-stu-id="992d2-108">Assembly attribute</span></span>

<span data-ttu-id="992d2-109">[FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute) musí být použity na objektu actor klienta a sestavení služby objektu actor atribut.</span><span class="sxs-lookup"><span data-stu-id="992d2-109">The [FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute) attribute needs to be applied on the actor client and actor service assemblies.</span></span>

<span data-ttu-id="992d2-110">Následující příklad ukazuje, jak změnit výchozí hodnotu FabricTransport OperationTimeout nastavení:</span><span class="sxs-lookup"><span data-stu-id="992d2-110">The following example shows how to change the default value of FabricTransport OperationTimeout settings:</span></span>

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600)]
   ```

   <span data-ttu-id="992d2-111">Druhém příkladu změní výchozí hodnoty FabricTransport MaxMessageSize a OperationTimeoutInSeconds.</span><span class="sxs-lookup"><span data-stu-id="992d2-111">Second example changes default Values of FabricTransport MaxMessageSize and OperationTimeoutInSeconds.</span></span>

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600,MaxMessageSize = 134217728)]
   ```

## <a name="config-package"></a><span data-ttu-id="992d2-112">Konfigurační balíček</span><span class="sxs-lookup"><span data-stu-id="992d2-112">Config package</span></span>

<span data-ttu-id="992d2-113">Můžete použít [konfigurační balíček](service-fabric-application-model.md) Chcete-li změnit výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="992d2-113">You can use a [config package](service-fabric-application-model.md) to modify the default configuration.</span></span>

### <a name="configure-fabrictransport-settings-for-the-actor-service"></a><span data-ttu-id="992d2-114">Konfigurace nastavení FabricTransport služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="992d2-114">Configure FabricTransport settings for the actor service</span></span>

<span data-ttu-id="992d2-115">Přidáte oddíl TransportSettings v souborech settings.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="992d2-115">Add a TransportSettings section in the settings.xml file.</span></span>

<span data-ttu-id="992d2-116">Ve výchozím kódu objektu actor hledá SectionName jako "&lt;ActorName&gt;TransportSettings".</span><span class="sxs-lookup"><span data-stu-id="992d2-116">By default, actor code looks for SectionName as "&lt;ActorName&gt;TransportSettings".</span></span> <span data-ttu-id="992d2-117">Pokud není nalezen, zkontroluje SectionName jako "TransportSettings".</span><span class="sxs-lookup"><span data-stu-id="992d2-117">If that's not found, it checks for SectionName as "TransportSettings".</span></span>

  ```xml
  <Section Name="MyActorServiceTransportSettings">
       <Parameter Name="MaxMessageSize" Value="10000000" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
       <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
       <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
       <Parameter Name="CertificateRemoteThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
       <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
       <Parameter Name="CertificateStoreName" Value="My" />
       <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
       <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
   </Section>
  ```

### <a name="configure-fabrictransport-settings-for-the-actor-client-assembly"></a><span data-ttu-id="992d2-118">Nakonfigurujte nastavení FabricTransport pro klienta sestavení objektu actor</span><span class="sxs-lookup"><span data-stu-id="992d2-118">Configure FabricTransport settings for the actor client assembly</span></span>

<span data-ttu-id="992d2-119">Pokud klient není spuštěna jako součást služby, můžete vytvořit "&lt;název souboru Exe klienta&gt;. souborech settings.xml" soubor ve stejném umístění jako soubor .exe klienta.</span><span class="sxs-lookup"><span data-stu-id="992d2-119">If the client is not running as part of a service, you can create a "&lt;Client Exe Name&gt;.settings.xml" file in the same location as the client .exe file.</span></span> <span data-ttu-id="992d2-120">Pak přidejte TransportSettings části v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="992d2-120">Then add a TransportSettings section in that file.</span></span> <span data-ttu-id="992d2-121">SectionName by měl být "TransportSettings".</span><span class="sxs-lookup"><span data-stu-id="992d2-121">SectionName should be "TransportSettings".</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
    <Section Name="TransportSettings">
      <Parameter Name="SecurityCredentialsType" Value="X509" />
      <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
      <Parameter Name="CertificateFindValue" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
      <Parameter Name="CertificateRemoteThumbprints" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
      <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
      <Parameter Name="CertificateStoreName" Value="My" />
      <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    </Section>
  </Settings>
   ```

  * <span data-ttu-id="992d2-122">Konfigurace nastavení FabricTransport pro zabezpečené objektu Actor nebo klienta služby sekundární certifikátem.</span><span class="sxs-lookup"><span data-stu-id="992d2-122">Configuring FabricTransport Settings for Secure Actor Service/Client With Secondary Certificate.</span></span>
  <span data-ttu-id="992d2-123">Informace o sekundární certifikátu můžete přidat tak, že přidáte parametr CertificateFindValuebySecondary.</span><span class="sxs-lookup"><span data-stu-id="992d2-123">Secondary certificate information can be added by adding parameter CertificateFindValuebySecondary.</span></span>
  <span data-ttu-id="992d2-124">Níže je příklad pro TransportSettings naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="992d2-124">Below is the example for the Listener TransportSettings.</span></span>

    ```xml
    <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
    <Parameter Name="CertificateFindValuebySecondary" Value="h9449b018d0f6839a2c5d62b5b6c6ac822b6f690" />
    <Parameter Name="CertificateRemoteThumbprints" Value="4FEF3950642138446CC364A396E1E881DB76B48C,a9449b018d0f6839a2c5d62b5b6c6ac822b6f667" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
     ```
     <span data-ttu-id="992d2-125">Níže je příklad pro TransportSettings klienta.</span><span class="sxs-lookup"><span data-stu-id="992d2-125">Below is the example for the Client TransportSettings.</span></span>

    ```xml
   <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateFindValuebySecondary" Value="a9449b018d0f6839a2c5d62b5b6c6ac822b6f667" />
    <Parameter Name="CertificateRemoteThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662,h9449b018d0f6839a2c5d62b5b6c6ac822b6f690" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
     ```
    * <span data-ttu-id="992d2-126">Konfigurace nastavení FabricTransport pro zabezpečení služby objektu Actor nebo klienta pomocí názvu subjektu.</span><span class="sxs-lookup"><span data-stu-id="992d2-126">Configuring FabricTransport  Settings for Securing Actor Service/Client Using Subject Name.</span></span>
    <span data-ttu-id="992d2-127">Uživatel musí poskytnout findType jako FindBySubjectName, přidejte CertificateIssuerThumbprints a CertificateRemoteCommonNames hodnoty.</span><span class="sxs-lookup"><span data-stu-id="992d2-127">User needs to provide findType as FindBySubjectName,add CertificateIssuerThumbprints and CertificateRemoteCommonNames values.</span></span>
  <span data-ttu-id="992d2-128">Níže je příklad pro TransportSettings naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="992d2-128">Below is the example for the Listener TransportSettings.</span></span>

     ```xml
    <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindBySubjectName" />
    <Parameter Name="CertificateFindValue" Value="CN = WinFabric-Test-SAN1-Alice" />
    <Parameter Name="CertificateIssuerThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
    <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Bob" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
    ```
  <span data-ttu-id="992d2-129">Níže je příklad pro TransportSettings klienta.</span><span class="sxs-lookup"><span data-stu-id="992d2-129">Below is the example for the Client TransportSettings.</span></span>

    ```xml
     <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindBySubjectName" />
    <Parameter Name="CertificateFindValue" Value="CN = WinFabric-Test-SAN1-Bob" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
     ```
