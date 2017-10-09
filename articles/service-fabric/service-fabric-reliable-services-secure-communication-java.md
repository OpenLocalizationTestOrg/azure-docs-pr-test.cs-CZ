---
title: "aaaHelp zabezpečené komunikace pro služby v Azure Service Fabric | Microsoft Docs"
description: "Přehled o tom, jak toohelp zabezpečení komunikace pro spolehlivé služby, jsou spuštěny v clusteru služby Azure Service Fabric."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 14db54d50c35478c1f2c156de0dba36f1427c8cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="c309a-103">Nápověda zabezpečené komunikace pro služby v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c309a-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c309a-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="c309a-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="c309a-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="c309a-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="c309a-106">Pomoc se zabezpečením služby, pokud používáte vzdálenou komunikaci služby</span><span class="sxs-lookup"><span data-stu-id="c309a-106">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="c309a-107">Budeme používat existující [příklad](service-fabric-reliable-services-communication-remoting-java.md) to vysvětluje, jak tooset až vzdálené komunikace pro spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="c309a-107">We'll be using an existing [example](service-fabric-reliable-services-communication-remoting-java.md) that explains how tooset up remoting for reliable services.</span></span> <span data-ttu-id="c309a-108">toohelp zabezpečení služby, pokud používáte vzdálenou komunikaci služby, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c309a-108">toohelp secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="c309a-109">Vytvořit rozhraní, `HelloWorldStateless`, který definuje hello metody, které budou k dispozici pro vzdálené volání procedury vaší služby.</span><span class="sxs-lookup"><span data-stu-id="c309a-109">Create an interface, `HelloWorldStateless`, that defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="c309a-110">Bude vaše služba používat `FabricTransportServiceRemotingListener`, kterého je deklarovaná v hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` balíčku.</span><span class="sxs-lookup"><span data-stu-id="c309a-110">Your service will use `FabricTransportServiceRemotingListener`, which is declared in hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` package.</span></span> <span data-ttu-id="c309a-111">Jedná se `CommunicationListener` implementace, která poskytuje funkce vzdálené komunikace.</span><span class="sxs-lookup"><span data-stu-id="c309a-111">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span>

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. <span data-ttu-id="c309a-112">Přidejte nastavení naslouchacího procesu a zabezpečovací pověření.</span><span class="sxs-lookup"><span data-stu-id="c309a-112">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="c309a-113">Ujistěte se, že hello certifikát, který má toouse toohelp zabezpečené komunikace vaší služby je nainstalován ve všech uzlech clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="c309a-113">Make sure that hello certificate that you want toouse toohelp secure your service communication is installed on all hello nodes in hello cluster.</span></span> <span data-ttu-id="c309a-114">Zadejte nastavení naslouchacího procesu a zabezpečovací pověření dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="c309a-114">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="c309a-115">Poskytněte pomocí [konfigurační balíček](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="c309a-115">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="c309a-116">Přidat `TransportSettings` v souborech settings.xml souboru hello.</span><span class="sxs-lookup"><span data-stu-id="c309a-116">Add a `TransportSettings` section in hello settings.xml file.</span></span>

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       <span data-ttu-id="c309a-117">V takovém případě hello `createServiceInstanceListeners` metoda bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="c309a-117">In this case, hello `createServiceInstanceListeners` method will look like this:</span></span>

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        <span data-ttu-id="c309a-118">Pokud přidáte `TransportSettings` v souborech settings.xml souboru hello bez jakékoli předpony `FabricTransportListenerSettings` načte všechna nastavení hello z této části ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c309a-118">If you add a `TransportSettings` section in hello settings.xml file without any prefix, `FabricTransportListenerSettings` will load all hello settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="c309a-119">V takovém případě hello `CreateServiceInstanceListeners` metoda bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="c309a-119">In this case, hello `CreateServiceInstanceListeners` method will look like this:</span></span>

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. <span data-ttu-id="c309a-120">Při volání metody ve službě zabezpečené pomocí hello zásobník vzdálenou komunikaci, místo použití hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` toocreate třídy proxy služby, použijte `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="c309a-120">When you call methods on a secured service by using hello remoting stack, instead of using hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class toocreate a service proxy, use `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span></span>

    <span data-ttu-id="c309a-121">Pokud kód klienta hello je spuštěn jako součást služby, můžete načíst `FabricTransportSettings` ze souboru souborech settings.xml hello.</span><span class="sxs-lookup"><span data-stu-id="c309a-121">If hello client code is running as part of a service, you can load `FabricTransportSettings` from hello settings.xml file.</span></span> <span data-ttu-id="c309a-122">Vytvořte TransportSettings oddíl, který je podobný kódu toohello služby, jako je uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="c309a-122">Create a TransportSettings section that is similar toohello service code, as shown earlier.</span></span> <span data-ttu-id="c309a-123">Proveďte následující změny kódu klienta toohello hello:</span><span class="sxs-lookup"><span data-stu-id="c309a-123">Make hello following changes toohello client code:</span></span>

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
