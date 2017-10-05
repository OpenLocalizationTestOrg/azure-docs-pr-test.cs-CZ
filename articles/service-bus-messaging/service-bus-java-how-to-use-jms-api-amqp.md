---
title: "Použití protokolu AMQP 1.0 se rozhraní API Java Service Bus | Microsoft Docs"
description: "Jak používat službu zpráva Java (JMS) s Azure Service Bus a rozšířené zpráv služby Řízení front Protodol (AMQP) 1.0."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 0848facd764c4fb0d7f95c1ae89ecb02a32257e1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a><span data-ttu-id="9c1c5-103">Jak používat Java zprávy služby (JMS) rozhraní API se Service Bus a protokolu AMQP 1.0</span><span class="sxs-lookup"><span data-stu-id="9c1c5-103">How to use the Java Message Service (JMS) API with Service Bus and AMQP 1.0</span></span>
<span data-ttu-id="9c1c5-104">Rozšířené zpráv služby Řízení front Protocol (AMQP) 1.0 je efektivní, spolehlivou a úroveň zasílání zpráv protokol, který můžete použít k vytvoření robustní, multiplatformní aplikace zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-104">The Advanced Message Queuing Protocol (AMQP) 1.0 is an efficient, reliable, wire-level messaging protocol that you can use to build robust, cross-platform messaging applications.</span></span>

<span data-ttu-id="9c1c5-105">Podpora pro protokolu AMQP 1.0 v Service Bus znamená, že můžete použít, řazení do fronty a publikování a přihlášení k odběru zprostředkované zasílání zpráv funkcí z rozsahu platforem a efektivní binární protokol.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-105">Support for AMQP 1.0 in Service Bus means that you can use the queuing and publish/subscribe brokered messaging features from a range of platforms using an efficient binary protocol.</span></span> <span data-ttu-id="9c1c5-106">Kromě toho můžete vytvořit aplikace skládá z komponenty sestaven pomocí kombinace jazyky, architektury a operační systémy.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-106">Furthermore, you can build applications comprised of components built using a mix of languages, frameworks, and operating systems.</span></span>

<span data-ttu-id="9c1c5-107">Tento článek vysvětluje, jak k použití služby Service Bus (fronty a témata publikování a přihlášení k odběru) funkce pro zasílání zpráv z aplikací v jazyce Java pomocí oblíbených Java zprávy služby (JMS) standardní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-107">This article explains how to use Service Bus messaging features (queues and publish/subscribe topics) from Java applications using the popular Java Message Service (JMS) API standard.</span></span> <span data-ttu-id="9c1c5-108">Je [doprovodný článek](service-bus-amqp-dotnet.md) to vysvětluje, jak to provést pomocí rozhraní API služby Service Bus .NET stejné.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-108">There is a [companion article](service-bus-amqp-dotnet.md) that explains how to do the same using the Service Bus .NET API.</span></span> <span data-ttu-id="9c1c5-109">Tyto dvě příručky můžete použít společně Další informace o zasílání zpráv mezi platformami pomocí protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-109">You can use these two guides together to learn about cross-platform messaging using AMQP 1.0.</span></span>

## <a name="get-started-with-service-bus"></a><span data-ttu-id="9c1c5-110">Začínáme se službou Service Bus</span><span class="sxs-lookup"><span data-stu-id="9c1c5-110">Get started with Service Bus</span></span>
<span data-ttu-id="9c1c5-111">Tato příručka předpokládá, že už máte obor názvů sběrnice obsahující frontu s názvem **Frontě1**.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-111">This guide assumes that you already have a Service Bus namespace containing a queue named **queue1**.</span></span> <span data-ttu-id="9c1c5-112">Pokud ho použít nechcete, pak můžete [vytvořit obor názvů a fronty](service-bus-create-namespace-portal.md) pomocí [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9c1c5-112">If you do not, then you can [create the namespace and queue](service-bus-create-namespace-portal.md) using the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9c1c5-113">Další informace o tom, jak vytvořit obory názvů Service Bus a fronty najdete v tématu [začít pracovat s fronty Service Bus](service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c5-113">For more information about how to create Service Bus namespaces and queues, see [Get started with Service Bus queues](service-bus-dotnet-get-started-with-queues.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9c1c5-114">Oddílů fronty a témata také podporují AMQP.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-114">Partitioned queues and topics also support AMQP.</span></span> <span data-ttu-id="9c1c5-115">Další informace najdete v tématu [segmentované entity zasílání zpráv](service-bus-partitioning.md) a [podporu protokolu AMQP 1.0 pro Service Bus oddíly fronty a témata](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c5-115">For more information, see [Partitioned messaging entities](service-bus-partitioning.md) and [AMQP 1.0 support for Service Bus partitioned queues and topics](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span></span>
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a><span data-ttu-id="9c1c5-116">Stahuje se knihovna klienta protokolu AMQP 1.0 JMS</span><span class="sxs-lookup"><span data-stu-id="9c1c5-116">Downloading the AMQP 1.0 JMS client library</span></span>
<span data-ttu-id="9c1c5-117">Informace o tom, kde chcete stáhnout nejnovější verzi klientské knihovny Apache Qpid JMS protokolu AMQP 1.0, najdete v článku [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="9c1c5-117">For information about where to download the latest version of the Apache Qpid JMS AMQP 1.0 client library, visit [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span></span>

<span data-ttu-id="9c1c5-118">Následující čtyři JAR soubory z archivu distribuční Apache Qpid JMS protokolu AMQP 1.0 je nutné přidat na cestě třídy Java při vytváření a spouštění aplikací JMS službou Service Bus:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-118">You must add the following four JAR files from the Apache Qpid JMS AMQP 1.0 distribution archive to the Java CLASSPATH when building and running JMS applications with Service Bus:</span></span>

* <span data-ttu-id="9c1c5-119">geronimo jms\_1.1\_specifikace 1.0.jar</span><span class="sxs-lookup"><span data-stu-id="9c1c5-119">geronimo-jms\_1.1\_spec-1.0.jar</span></span>
* <span data-ttu-id="9c1c5-120">qpid-amqp-1-0-Client-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="9c1c5-120">qpid-amqp-1-0-client-[version].jar</span></span>
* <span data-ttu-id="9c1c5-121">qpid-amqp-1-0-Client-jms-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="9c1c5-121">qpid-amqp-1-0-client-jms-[version].jar</span></span>
* <span data-ttu-id="9c1c5-122">qpid-amqp-1-0-Common-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="9c1c5-122">qpid-amqp-1-0-common-[version].jar</span></span>

## <a name="coding-java-applications"></a><span data-ttu-id="9c1c5-123">Kódování aplikací v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="9c1c5-123">Coding Java applications</span></span>
### <a name="java-naming-and-directory-interface-jndi"></a><span data-ttu-id="9c1c5-124">Pojmenování Java a Directory rozhraní (JNDI)</span><span class="sxs-lookup"><span data-stu-id="9c1c5-124">Java Naming and Directory Interface (JNDI)</span></span>
<span data-ttu-id="9c1c5-125">JMS používá k vytvoření oddělení mezi názvy logické a fyzické pojmenovávání Java a Directory rozhraní (JNDI).</span><span class="sxs-lookup"><span data-stu-id="9c1c5-125">JMS uses the Java Naming and Directory Interface (JNDI) to create a separation between logical names and physical names.</span></span> <span data-ttu-id="9c1c5-126">Dva typy objektů JMS se přeloží pomocí JNDI: ConnectionFactory a cíl.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-126">Two types of JMS objects are resolved using JNDI: ConnectionFactory and Destination.</span></span> <span data-ttu-id="9c1c5-127">JNDI používá zprostředkovatele modelu, do kterého je možné připojit jiný adresář služby pro zpracování název řešení povinností.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-127">JNDI uses a provider model into which you can plug different directory services to handle name resolution duties.</span></span> <span data-ttu-id="9c1c5-128">Apache Qpid JMS protokolu AMQP 1.0 knihovny se dodává s jednoduché vlastnosti formátování na základě souborů JNDI zprostředkovatele, který je nakonfigurovaný pomocí vlastnosti souboru z následujících:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-128">The Apache Qpid JMS AMQP 1.0 library comes with a simple properties file-based JNDI Provider that is configured using a properties file of the following format:</span></span>

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a><span data-ttu-id="9c1c5-129">Konfigurace ConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="9c1c5-129">Configure the ConnectionFactory</span></span>
<span data-ttu-id="9c1c5-130">Záznam používá k definování **ConnectionFactory** v souboru vlastnosti Qpid JNDI zprostředkovatele je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-130">The entry used to define a **ConnectionFactory** in the Qpid properties file JNDI provider is of the following format:</span></span>

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

<span data-ttu-id="9c1c5-131">Kde **[jndi_name]** a **[ConnectionURL]** mají následující významy:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-131">Where **[jndi_name]** and **[ConnectionURL]** have the following meanings:</span></span>

* <span data-ttu-id="9c1c5-132">**[jndi_name]** : Logický název ConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-132">**[jndi_name]**: The logical name of the ConnectionFactory.</span></span> <span data-ttu-id="9c1c5-133">Toto je název, který se vyřeší v aplikaci Java pomocí metody JNDI IntialContext.lookup().</span><span class="sxs-lookup"><span data-stu-id="9c1c5-133">This is the name that will be resolved in the Java application using the JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="9c1c5-134">**[ConnectionURL]** : Adresu URL, která poskytuje knihovnu JMS informace požadované pro zprostředkovatele protokolu AMQP.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-134">**[ConnectionURL]**: A URL that provides the JMS library with the information required to the AMQP broker.</span></span>

<span data-ttu-id="9c1c5-135">Formát **ConnectionURL** vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-135">The format of the **ConnectionURL** is as follows:</span></span>

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
<span data-ttu-id="9c1c5-136">Kde **[obor názvů]**, **[SASPolicyName]** a **[SASPolicyKey]** mají následující významy:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-136">Where **[namespace]**, **[SASPolicyName]** and **[SASPolicyKey]** have the following meanings:</span></span>

* <span data-ttu-id="9c1c5-137">**[obor názvů]** : Obor názvů sběrnice služby.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-137">**[namespace]**: The Service Bus namespace.</span></span>
* <span data-ttu-id="9c1c5-138">**[SASPolicyName]** : Název zásady fronty sdíleného přístupového podpisu.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-138">**[SASPolicyName]**: The Queue Shared Access Signature policy name.</span></span>
* <span data-ttu-id="9c1c5-139">**[SASPolicyKey]** : Klíč zásad fronty sdíleného přístupového podpisu.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-139">**[SASPolicyKey]**: The Queue Shared Access Signature policy key.</span></span>

> [!NOTE]
> <span data-ttu-id="9c1c5-140">Je nutné kódování URL heslo ručně.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-140">You must URL-encode the password manually.</span></span> <span data-ttu-id="9c1c5-141">Užitečné kódování URL nástroj je k dispozici na [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="9c1c5-141">A useful URL-encoding utility is available at [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
> 
> 

#### <a name="configure-destinations"></a><span data-ttu-id="9c1c5-142">Konfigurace cíle</span><span class="sxs-lookup"><span data-stu-id="9c1c5-142">Configure destinations</span></span>
<span data-ttu-id="9c1c5-143">Položka používá k definování cíl ve zprostředkovateli Qpid soubor vlastnosti JNDI má následující formát:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-143">The entry used to define a destination in the Qpid properties file JNDI provider is of the following format:</span></span>

```
queue.[jndi_name] = [physical_name]
```

<span data-ttu-id="9c1c5-144">nebo</span><span class="sxs-lookup"><span data-stu-id="9c1c5-144">or</span></span>

```
topic.[jndi_name] = [physical_name]
```

<span data-ttu-id="9c1c5-145">Kde **[jndi\_název]** a **[fyzické\_název]** mají následující významy:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-145">Where **[jndi\_name]** and **[physical\_name]** have the following meanings:</span></span>

* <span data-ttu-id="9c1c5-146">**[jndi_name]** : Logický název cílové.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-146">**[jndi_name]**: The logical name of the destination.</span></span> <span data-ttu-id="9c1c5-147">Toto je název, který se vyřeší v aplikaci Java pomocí metody JNDI IntialContext.lookup().</span><span class="sxs-lookup"><span data-stu-id="9c1c5-147">This is the name that will be resolved in the Java application using the JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="9c1c5-148">**[physical_name]** : Název entity služby sběrnice, ke kterému aplikace odeslání nebo přijetí zprávy.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-148">**[physical_name]**: The name of the Service Bus entity to which the application sends or receives messages.</span></span>

> [!NOTE]
> <span data-ttu-id="9c1c5-149">Při přijímání od odběr tématu Service Bus, fyzický název zadaný v JNDI musí být název tématu.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-149">When receiving from a Service Bus topic subscription, the physical name specified in JNDI should be the name of the topic.</span></span> <span data-ttu-id="9c1c5-150">Při vytvoření odolné odběru v kódu aplikace JMS, je zadán název odběru.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-150">The subscription name is provided when the durable subscription is created in the JMS application code.</span></span> <span data-ttu-id="9c1c5-151">[Service Bus AMQP 1.0 – Příručka vývojáře](service-bus-amqp-dotnet.md) obsahuje další informace o práci s témat sběrnice Service Bus z JMS.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-151">The [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) provides more details on working with Service Bus topics from JMS.</span></span>
> 
> 

### <a name="write-the-jms-application"></a><span data-ttu-id="9c1c5-152">Zapište aplikaci JMS</span><span class="sxs-lookup"><span data-stu-id="9c1c5-152">Write the JMS application</span></span>
<span data-ttu-id="9c1c5-153">Neexistují žádné speciální rozhraní API nebo možnosti, které jsou potřeba při použití JMS službou Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-153">There are no special APIs or options required when using JMS with Service Bus.</span></span> <span data-ttu-id="9c1c5-154">Existují však několik omezení, kterým se budeme později.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-154">However, there are a few restrictions that will be covered later.</span></span> <span data-ttu-id="9c1c5-155">Stejně jako u jakékoli aplikace JMS nejprve thing požadované je konfigurace JNDI prostředí, abyste mohli vyřešit **ConnectionFactory** a cíle.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-155">As with any JMS application, the first thing required is configuration of the JNDI environment, to be able to resolve a **ConnectionFactory** and destinations.</span></span>

#### <a name="configure-the-jndi-initialcontext"></a><span data-ttu-id="9c1c5-156">Konfigurace JNDI InitialContext</span><span class="sxs-lookup"><span data-stu-id="9c1c5-156">Configure the JNDI InitialContext</span></span>
<span data-ttu-id="9c1c5-157">JNDI prostředí je nakonfigurováno pomocí předání do konstruktoru třídy javax.naming.InitialContext zatřiďovací tabulku informací o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-157">The JNDI environment is configured by passing a hashtable of configuration information into the constructor of the javax.naming.InitialContext class.</span></span> <span data-ttu-id="9c1c5-158">Dva požadované prvky v zatřiďovací tabulce jsou název třídy počáteční vytváření kontextu a adresu poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-158">The two required elements in the hashtable are the class name of the Initial Context Factory and the Provider URL.</span></span> <span data-ttu-id="9c1c5-159">Následující kód ukazuje, jak nakonfigurovat prostředí JNDI používat Qpid soubor vlastnosti zprostředkovatele na bázi JNDI s vlastnosti souboru s názvem **servicebus.properties**.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-159">The following code shows how to configure the JNDI environment to use the Qpid properties file based JNDI Provider with a properties file named **servicebus.properties**.</span></span>

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a><span data-ttu-id="9c1c5-160">Jednoduché JMS aplikace pomocí fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="9c1c5-160">A simple JMS application using a Service Bus queue</span></span>
<span data-ttu-id="9c1c5-161">V následujícím příkladu programu odešle JMS TextMessages do fronty Service Bus s JNDI logický název fronty a přijímá zprávy zpět.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-161">The following example program sends JMS TextMessages to a Service Bus queue with the JNDI logical name of QUEUE, and receives the messages back.</span></span>

```java
// SimpleSenderReceiver.java

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;

public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();

    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);

        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");

        // Create Connection
        connection = cf.createConnection();

        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);

        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }

    public static void main(String[] args) {
        try {

            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }

            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));

            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }

    public void close() throws JMSException {
        connection.close();
    }

    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}    
```

### <a name="run-the-application"></a><span data-ttu-id="9c1c5-162">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9c1c5-162">Run the application</span></span>
<span data-ttu-id="9c1c5-163">Spuštění aplikace vytváří výstup ve formátu:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-163">Running the application produces output of the form:</span></span>

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a><span data-ttu-id="9c1c5-164">Napříč platformami zasílání zpráv mezi JMS a rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9c1c5-164">Cross-platform messaging between JMS and .NET</span></span>
<span data-ttu-id="9c1c5-165">Tento průvodce vám ukázal, jak odesílat a přijímat zprávy do a ze služby Service Bus pomocí JMS.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-165">This guide showed how to send and receive messages to and from Service Bus using JMS.</span></span> <span data-ttu-id="9c1c5-166">Mezi klíčové výhody protokolu AMQP 1.0 je však umožňuje aplikace má být sestaven z komponent, které jsou napsané v různých jazycích, pomocí zprávy vyměňují spolehlivé a na úplné přesnost.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-166">However, one of the key benefits of AMQP 1.0 is that it enables applications to be built from components written in different languages, with messages exchanged reliably and at full fidelity.</span></span>

<span data-ttu-id="9c1c5-167">Pomocí ukázkovou aplikaci JMS popsané výše a podobné aplikace .NET prováděné z článku doprovodné [pomocí Service Bus pomocí technologie .NET pomocí protokolu AMQP 1.0](service-bus-amqp-dotnet.md), můžou vyměňovat zprávy mezi .NET a Javu.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-167">Using the sample JMS application described above and a similar .NET application taken from a companion article, [Using Service Bus from .NET with AMQP 1.0](service-bus-amqp-dotnet.md), you can exchange messages between .NET and Java.</span></span> <span data-ttu-id="9c1c5-168">Přečtěte si tento článek Další informace o podrobnosti o platformě zasílání zpráv pomocí sběrnice a protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-168">Read this article for more information about the details of cross-platform messaging using Service Bus and AMQP 1.0.</span></span>

### <a name="jms-to-net"></a><span data-ttu-id="9c1c5-169">JMS na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9c1c5-169">JMS to .NET</span></span>
<span data-ttu-id="9c1c5-170">K předvedení JMS .NET zasílání zpráv:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-170">To demonstrate JMS to .NET messaging:</span></span>

* <span data-ttu-id="9c1c5-171">Spuštění ukázkové aplikace .NET bez argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-171">Start the .NET sample application without any command-line arguments.</span></span>
* <span data-ttu-id="9c1c5-172">Spuštění ukázkové aplikace Java s argumentem příkazového řádku "sendonly".</span><span class="sxs-lookup"><span data-stu-id="9c1c5-172">Start the Java sample application with the "sendonly" command-line argument.</span></span> <span data-ttu-id="9c1c5-173">V tomto režimu, aplikace nebude přijímat zprávy z fronty, kterou bude pouze odesílat.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-173">In this mode, the application will not receive messages from the queue, it will only send.</span></span>
* <span data-ttu-id="9c1c5-174">Stiskněte klávesu **Enter** několikrát v konzole aplikace Java, což způsobí, že odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-174">Press **Enter** a few times in the Java application console, which will cause messages to be sent.</span></span>
* <span data-ttu-id="9c1c5-175">Tyto zprávy jsou přijímány aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-175">These messages are received by the .NET application.</span></span>

#### <a name="output-from-jms-application"></a><span data-ttu-id="9c1c5-176">Výstup z aplikace JMS</span><span class="sxs-lookup"><span data-stu-id="9c1c5-176">Output from JMS application</span></span>
```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a><span data-ttu-id="9c1c5-177">Výstup z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="9c1c5-177">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a><span data-ttu-id="9c1c5-178">.NET na JMS</span><span class="sxs-lookup"><span data-stu-id="9c1c5-178">.NET to JMS</span></span>
<span data-ttu-id="9c1c5-179">K předvedení .NET JMS zasílání zpráv:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-179">To demonstrate .NET to JMS messaging:</span></span>

* <span data-ttu-id="9c1c5-180">Spuštění ukázkové aplikace .NET s argumentem příkazového řádku "sendonly".</span><span class="sxs-lookup"><span data-stu-id="9c1c5-180">Start the .NET sample application with the "sendonly" command-line argument.</span></span> <span data-ttu-id="9c1c5-181">V tomto režimu, aplikace nebude přijímat zprávy z fronty, kterou bude pouze odesílat.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-181">In this mode, the application will not receive messages from the queue, it will only send.</span></span>
* <span data-ttu-id="9c1c5-182">Spuštění ukázkové aplikace Java bez argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-182">Start the Java sample application without any command-line arguments.</span></span>
* <span data-ttu-id="9c1c5-183">Stiskněte klávesu **Enter** několikrát v konzole aplikace .NET, což způsobí, že odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-183">Press **Enter** a few times in the .NET application console, which will cause messages to be sent.</span></span>
* <span data-ttu-id="9c1c5-184">Tyto zprávy jsou přijímány aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-184">These messages are received by the Java application.</span></span>

#### <a name="output-from-net-application"></a><span data-ttu-id="9c1c5-185">Výstup z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="9c1c5-185">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a><span data-ttu-id="9c1c5-186">Výstup z aplikace JMS</span><span class="sxs-lookup"><span data-stu-id="9c1c5-186">Output from JMS application</span></span>
```
> java SimpleSenderReceiver    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a><span data-ttu-id="9c1c5-187">Nepodporované funkce a omezení</span><span class="sxs-lookup"><span data-stu-id="9c1c5-187">Unsupported features and restrictions</span></span>
<span data-ttu-id="9c1c5-188">Při použití JMS prostřednictvím protokolu AMQP 1.0 se Service Bus, konkrétně, existují následující omezení:</span><span class="sxs-lookup"><span data-stu-id="9c1c5-188">The following restrictions exist when using JMS over AMQP 1.0 with Service Bus, namely:</span></span>

* <span data-ttu-id="9c1c5-189">Pouze jeden **MessageProducer** nebo **MessageConsumer** je povolena na **relace**.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-189">Only one **MessageProducer** or **MessageConsumer** is allowed per **Session**.</span></span> <span data-ttu-id="9c1c5-190">Pokud je potřeba vytvořit několik **MessageProducers** nebo **MessageConsumers** v aplikaci, vytvořte vyhrazená **relace** pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-190">If you need to create multiple **MessageProducers** or **MessageConsumers** in an application, create a dedicated **Session** for each of them.</span></span>
* <span data-ttu-id="9c1c5-191">Odběry témat volatile nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-191">Volatile topic subscriptions are not currently supported.</span></span>
* <span data-ttu-id="9c1c5-192">**MessageSelectors** nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-192">**MessageSelectors** are not currently supported.</span></span>
* <span data-ttu-id="9c1c5-193">Dočasné cíle; například **TemporaryQueue**, **TemporaryTopic** nejsou aktuálně podporovány, spolu s **QueueRequestor** a **TopicRequestor** rozhraní API, která je používat.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-193">Temporary destinations; for example, **TemporaryQueue**, **TemporaryTopic** are not currently supported, along with the **QueueRequestor** and **TopicRequestor** APIs that use them.</span></span>
* <span data-ttu-id="9c1c5-194">Zpracované relací a distribuované transakce nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-194">Transacted sessions and distributed transactions are not supported.</span></span>

## <a name="summary"></a><span data-ttu-id="9c1c5-195">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9c1c5-195">Summary</span></span>
<span data-ttu-id="9c1c5-196">Tento postup Průvodce vám ukázal, jak používat funkce Service Bus zprostředkované zasílání zpráv (fronty a témata publikování a přihlášení k odběru) z prostředí Java pomocí Oblíbené JMS rozhraní API a protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-196">This how-to guide showed how to use Service Bus brokered messaging features (queues and publish/subscribe topics) from Java using the popular JMS API and AMQP 1.0.</span></span>

<span data-ttu-id="9c1c5-197">Můžete taky Service Bus protokolu AMQP 1.0 z dalších jazycích včetně .NET, C, Python nebo PHP.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-197">You can also use Service Bus AMQP 1.0 from other languages, including .NET, C, Python, and PHP.</span></span> <span data-ttu-id="9c1c5-198">Součásti vytvořená s využitím těchto různé jazyky můžete výměna zpráv spolehlivě a na úplné věrnosti pomocí protokolu AMQP 1.0 podporovat v Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9c1c5-198">Components built using these different languages can exchange messages reliably and at full fidelity using the AMQP 1.0 support in Service Bus.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c1c5-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c1c5-199">Next steps</span></span>
* [<span data-ttu-id="9c1c5-200">Podpora protokolu AMQP 1.0 v Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="9c1c5-200">AMQP 1.0 support in Azure Service Bus</span></span>](service-bus-amqp-overview.md)
* [<span data-ttu-id="9c1c5-201">Použití protokolu AMQP 1.0 se rozhraní API služby Service Bus rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9c1c5-201">How to use AMQP 1.0 with the Service Bus .NET API</span></span>](service-bus-dotnet-advanced-message-queuing.md)
* [<span data-ttu-id="9c1c5-202">Service Bus AMQP 1.0 – Příručka vývojáře</span><span class="sxs-lookup"><span data-stu-id="9c1c5-202">Service Bus AMQP 1.0 Developer's Guide</span></span>](service-bus-amqp-dotnet.md)
* [<span data-ttu-id="9c1c5-203">Začínáme s frontami služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="9c1c5-203">Get started with Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)
* [<span data-ttu-id="9c1c5-204">Středisko pro vývojáře Java</span><span class="sxs-lookup"><span data-stu-id="9c1c5-204">Java Developer Center</span></span>](https://azure.microsoft.com/develop/java/)

