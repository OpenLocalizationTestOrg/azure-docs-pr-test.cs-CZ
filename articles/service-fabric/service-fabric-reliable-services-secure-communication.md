---
title: "Pomůže zabezpečené komunikace pro služby v Azure Service Fabric | Microsoft Docs"
description: "Přehled o tom, jak pomoci zabezpečenou komunikaci pro spolehlivé služby, které jsou spuštěny v clusteru služby Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: 53119244f8f09c0c6c43f43761af1cc074f8d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="ac513-103">Nápověda zabezpečené komunikace pro služby v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ac513-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ac513-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="ac513-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="ac513-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="ac513-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

<span data-ttu-id="ac513-106">Zabezpečení je jedním z nejdůležitějších aspektů komunikace.</span><span class="sxs-lookup"><span data-stu-id="ac513-106">Security is one of the most important aspects of communication.</span></span> <span data-ttu-id="ac513-107">Rozhraní spolehlivé služby poskytuje několik předem komunikace zásobníky a nástroje, které můžete použít k vylepšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ac513-107">The Reliable Services application framework provides a few prebuilt communication stacks and tools that you can use to improve security.</span></span> <span data-ttu-id="ac513-108">Tento článek pojednává o tom, jak zlepšit zabezpečení, když používáte vzdálené komunikace služby a služby Windows Communication Foundation (WCF) komunikačního balíku.</span><span class="sxs-lookup"><span data-stu-id="ac513-108">This article talks about how to improve security when you're using service remoting and the Windows Communication Foundation (WCF) communication stack.</span></span>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="ac513-109">Pomoc se zabezpečením služby, pokud používáte vzdálenou komunikaci služby</span><span class="sxs-lookup"><span data-stu-id="ac513-109">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="ac513-110">Používáme existující [příklad](service-fabric-reliable-services-communication-remoting.md) to vysvětluje, jak nastavit vzdálenou komunikaci pro spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="ac513-110">We are using an existing [example](service-fabric-reliable-services-communication-remoting.md) that explains how to set up remoting for reliable services.</span></span> <span data-ttu-id="ac513-111">Chcete-li pomoc se zabezpečením služby, pokud používáte vzdálenou komunikaci služby, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ac513-111">To help secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="ac513-112">Vytvořit rozhraní, `IHelloWorldStateful`, který definuje metody, které budou k dispozici pro vzdálené volání procedury vaší služby.</span><span class="sxs-lookup"><span data-stu-id="ac513-112">Create an interface, `IHelloWorldStateful`, that defines the methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="ac513-113">Bude vaše služba používat `FabricTransportServiceRemotingListener`, kterého je deklarovaná v `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ac513-113">Your service will use `FabricTransportServiceRemotingListener`, which is declared in the `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` namespace.</span></span> <span data-ttu-id="ac513-114">Jedná se `ICommunicationListener` implementace, která poskytuje funkce vzdálené komunikace.</span><span class="sxs-lookup"><span data-stu-id="ac513-114">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span>

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. <span data-ttu-id="ac513-115">Přidejte nastavení naslouchacího procesu a zabezpečovací pověření.</span><span class="sxs-lookup"><span data-stu-id="ac513-115">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="ac513-116">Ujistěte se, že certifikát, který chcete použít k zabezpečení komunikace vaší služby je nainstalována na všech uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ac513-116">Make sure that the certificate that you want to use to help secure your service communication is installed on all the nodes in the cluster.</span></span> <span data-ttu-id="ac513-117">Zadejte nastavení naslouchacího procesu a zabezpečovací pověření dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="ac513-117">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="ac513-118">Poskytněte přímo v kódu služby:</span><span class="sxs-lookup"><span data-stu-id="ac513-118">Provide them directly in the service code:</span></span>

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. <span data-ttu-id="ac513-119">Poskytněte pomocí [konfigurační balíček](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="ac513-119">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="ac513-120">Přidat `TransportSettings` v souborech settings.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="ac513-120">Add a `TransportSettings` section in the settings.xml file.</span></span>

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       <span data-ttu-id="ac513-121">V takovém případě `CreateServiceReplicaListeners` metoda bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="ac513-121">In this case, the `CreateServiceReplicaListeners` method will look like this:</span></span>

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        <span data-ttu-id="ac513-122">Pokud přidáte `TransportSettings` v souborech settings.xml souboru `FabricTransportRemotingListenerSettings ` načte všechna nastavení z tohoto oddílu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ac513-122">If you add a `TransportSettings` section in the settings.xml file , `FabricTransportRemotingListenerSettings ` will load all the settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="ac513-123">V takovém případě `CreateServiceReplicaListeners` metoda bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="ac513-123">In this case, the `CreateServiceReplicaListeners` method will look like this:</span></span>

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. <span data-ttu-id="ac513-124">Při volání metody ve službě zabezpečené pomocí vzdálené komunikace zásobníku, místo použití `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` třídy vytvořit proxy služby, použijte `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="ac513-124">When you call methods on a secured service by using the remoting stack, instead of using the `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class to create a service proxy, use `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span></span> <span data-ttu-id="ac513-125">Předejte `FabricTransportRemotingSettings`, který obsahuje `SecurityCredentials`.</span><span class="sxs-lookup"><span data-stu-id="ac513-125">Pass in `FabricTransportRemotingSettings`, which contains `SecurityCredentials`.</span></span>

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    <span data-ttu-id="ac513-126">Pokud kód klienta je spuštěn jako součást služby, můžete načíst `FabricTransportRemotingSettings` ze souboru souborech settings.xml.</span><span class="sxs-lookup"><span data-stu-id="ac513-126">If the client code is running as part of a service, you can load `FabricTransportRemotingSettings` from the settings.xml file.</span></span> <span data-ttu-id="ac513-127">Vytvořte HelloWorldClientTransportSettings oddíl, který je podobný kódu služby, jako je uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="ac513-127">Create a HelloWorldClientTransportSettings section that is similar to the service code, as shown earlier.</span></span> <span data-ttu-id="ac513-128">Na kód klienta, proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="ac513-128">Make the following changes to the client code:</span></span>

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    <span data-ttu-id="ac513-129">Pokud klient není spuštěna jako součást služby, můžete vytvořit soubor client_name.settings.xml ve stejném umístění, kde je client_name.exe.</span><span class="sxs-lookup"><span data-stu-id="ac513-129">If the client is not running as part of a service, you can create a client_name.settings.xml file in the same location where the client_name.exe is.</span></span> <span data-ttu-id="ac513-130">Pak vytvořte TransportSettings části v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="ac513-130">Then create a TransportSettings section in that file.</span></span>

    <span data-ttu-id="ac513-131">Podobně jako službu, pokud přidáte `TransportSettings` část v klienta settings.xml/client_name.settings.xml `FabricTransportRemotingSettings` načte všechna nastavení z tohoto oddílu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ac513-131">Similar to the service, if you add a `TransportSettings` section in client settings.xml/client_name.settings.xml, `FabricTransportRemotingSettings` loads all the settings from this section by default.</span></span>

    <span data-ttu-id="ac513-132">V takovém případě je zjednodušený i další starší kód:</span><span class="sxs-lookup"><span data-stu-id="ac513-132">In that case, the earlier code is even further simplified:</span></span>  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a><span data-ttu-id="ac513-133">Pomoc se zabezpečením služby při použití zásobníku komunikace na základě WCF</span><span class="sxs-lookup"><span data-stu-id="ac513-133">Help secure a service when you're using a WCF-based communication stack</span></span>
<span data-ttu-id="ac513-134">Používáme existující [příklad](service-fabric-reliable-services-communication-wcf.md) to vysvětluje, jak nastavit zásobníku komunikace na základě WCF pro spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="ac513-134">We are using an existing [example](service-fabric-reliable-services-communication-wcf.md) that explains how to set up a WCF-based communication stack for reliable services.</span></span> <span data-ttu-id="ac513-135">K zajištění služby, pokud používáte zásobníku komunikace na základě WCF, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ac513-135">To help secure a service when you're using a WCF-based communication stack, follow these steps:</span></span>

1. <span data-ttu-id="ac513-136">Pro službu, potřebujete pomoc se zabezpečením naslouchací proces komunikace WCF (`WcfCommunicationListener`), které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="ac513-136">For the service, you need to help secure the WCF communication listener (`WcfCommunicationListener`) that you create.</span></span> <span data-ttu-id="ac513-137">Chcete-li to provést, změňte `CreateServiceReplicaListeners` metoda.</span><span class="sxs-lookup"><span data-stu-id="ac513-137">To do this, modify the `CreateServiceReplicaListeners` method.</span></span>

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```
2. <span data-ttu-id="ac513-138">V klientovi `WcfCommunicationClient` třídu, která byla vytvořena v předchozí [příklad](service-fabric-reliable-services-communication-wcf.md) zůstává beze změny.</span><span class="sxs-lookup"><span data-stu-id="ac513-138">In the client, the `WcfCommunicationClient` class that was created in the previous [example](service-fabric-reliable-services-communication-wcf.md) remains unchanged.</span></span> <span data-ttu-id="ac513-139">Je nutné přepsat, ale `CreateClientAsync` metodu `WcfCommunicationClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="ac513-139">But you need to override the `CreateClientAsync` method of `WcfCommunicationClientFactory`:</span></span>

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    <span data-ttu-id="ac513-140">Použití `SecureWcfCommunicationClientFactory` k vytvoření klienta WCF komunikace (`WcfCommunicationClient`).</span><span class="sxs-lookup"><span data-stu-id="ac513-140">Use `SecureWcfCommunicationClientFactory` to create a WCF communication client (`WcfCommunicationClient`).</span></span> <span data-ttu-id="ac513-141">Klient použijte pro vyvolání metody služeb.</span><span class="sxs-lookup"><span data-stu-id="ac513-141">Use the client to invoke service methods.</span></span>

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a><span data-ttu-id="ac513-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac513-142">Next steps</span></span>
* [<span data-ttu-id="ac513-143">Webové rozhraní API s OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="ac513-143">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
