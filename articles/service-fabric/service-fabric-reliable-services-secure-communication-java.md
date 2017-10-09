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
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Nápověda zabezpečené komunikace pro služby v Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java v Linuxu](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Pomoc se zabezpečením služby, pokud používáte vzdálenou komunikaci služby
Budeme používat existující [příklad](service-fabric-reliable-services-communication-remoting-java.md) to vysvětluje, jak tooset až vzdálené komunikace pro spolehlivé služby. toohelp zabezpečení služby, pokud používáte vzdálenou komunikaci služby, postupujte takto:

1. Vytvořit rozhraní, `HelloWorldStateless`, který definuje hello metody, které budou k dispozici pro vzdálené volání procedury vaší služby. Bude vaše služba používat `FabricTransportServiceRemotingListener`, kterého je deklarovaná v hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` balíčku. Jedná se `CommunicationListener` implementace, která poskytuje funkce vzdálené komunikace.

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
2. Přidejte nastavení naslouchacího procesu a zabezpečovací pověření.

    Ujistěte se, že hello certifikát, který má toouse toohelp zabezpečené komunikace vaší služby je nainstalován ve všech uzlech clusteru hello hello. Zadejte nastavení naslouchacího procesu a zabezpečovací pověření dvěma způsoby:

   1. Poskytněte pomocí [konfigurační balíček](service-fabric-application-model.md):

       Přidat `TransportSettings` v souborech settings.xml souboru hello.

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

       V takovém případě hello `createServiceInstanceListeners` metoda bude vypadat například takto:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Pokud přidáte `TransportSettings` v souborech settings.xml souboru hello bez jakékoli předpony `FabricTransportListenerSettings` načte všechna nastavení hello z této části ve výchozím nastavení.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        V takovém případě hello `CreateServiceInstanceListeners` metoda bude vypadat například takto:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Při volání metody ve službě zabezpečené pomocí hello zásobník vzdálenou komunikaci, místo použití hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` toocreate třídy proxy služby, použijte `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Pokud kód klienta hello je spuštěn jako součást služby, můžete načíst `FabricTransportSettings` ze souboru souborech settings.xml hello. Vytvořte TransportSettings oddíl, který je podobný kódu toohello služby, jako je uvedené výše. Proveďte následující změny kódu klienta toohello hello:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
