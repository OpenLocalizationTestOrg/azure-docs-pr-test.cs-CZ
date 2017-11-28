---
title: "aaaHow toouse protokolu AMQP 1.0 s hello rozhraní API Java Service Bus | Microsoft Docs"
description: "Jak toouse hello Java zprávy služby (JMS) s Azure Service Bus a rozšířené zpráv služby Řízení front Protodol (AMQP) 1.0."
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
ms.openlocfilehash: 3e1d0329f2675a2273e12bb7389d3ce38b156a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-java-message-service-jms-api-with-service-bus-and-amqp-10"></a><span data-ttu-id="e5d58-103">Jak toouse hello Java zprávy služby (JMS) rozhraní API se Service Bus a protokolu AMQP 1.0</span><span class="sxs-lookup"><span data-stu-id="e5d58-103">How toouse hello Java Message Service (JMS) API with Service Bus and AMQP 1.0</span></span>
<span data-ttu-id="e5d58-104">Hello Advanced zpráv služby Řízení front Protocol (AMQP) 1.0 je efektivní, spolehlivou a úroveň protokolu zasílání zpráv, které můžete použít toobuild robustní, multiplatformní aplikace zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="e5d58-104">hello Advanced Message Queuing Protocol (AMQP) 1.0 is an efficient, reliable, wire-level messaging protocol that you can use toobuild robust, cross-platform messaging applications.</span></span>

<span data-ttu-id="e5d58-105">Podpora protokolu AMQP 1.0 v Service Bus prostředky, které můžete použít hello služby Řízení front a publikování a přihlášení k odběru zprostředkované zasílání zpráv funkcí z rozsahu platforem a efektivní binární protokol.</span><span class="sxs-lookup"><span data-stu-id="e5d58-105">Support for AMQP 1.0 in Service Bus means that you can use hello queuing and publish/subscribe brokered messaging features from a range of platforms using an efficient binary protocol.</span></span> <span data-ttu-id="e5d58-106">Kromě toho můžete vytvořit aplikace skládá z komponenty sestaven pomocí kombinace jazyky, architektury a operační systémy.</span><span class="sxs-lookup"><span data-stu-id="e5d58-106">Furthermore, you can build applications comprised of components built using a mix of languages, frameworks, and operating systems.</span></span>

<span data-ttu-id="e5d58-107">Tento článek vysvětluje, jak hello toouse funkce Service Bus zasílání zpráv (fronty a publikování a přihlášení k odběru témata) z aplikací v jazyce Java pomocí Oblíbené Java zprávy služby (JMS) standardní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5d58-107">This article explains how toouse Service Bus messaging features (queues and publish/subscribe topics) from Java applications using hello popular Java Message Service (JMS) API standard.</span></span> <span data-ttu-id="e5d58-108">Je [doprovodný článek](service-bus-amqp-dotnet.md) to vysvětluje, jak hello toodo stejné pomocí hello Service Bus .NET API.</span><span class="sxs-lookup"><span data-stu-id="e5d58-108">There is a [companion article](service-bus-amqp-dotnet.md) that explains how toodo hello same using hello Service Bus .NET API.</span></span> <span data-ttu-id="e5d58-109">Můžete použít tyto dvě příručky společně toolearn o zasílání zpráv mezi platformami pomocí protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="e5d58-109">You can use these two guides together toolearn about cross-platform messaging using AMQP 1.0.</span></span>

## <a name="get-started-with-service-bus"></a><span data-ttu-id="e5d58-110">Začínáme se službou Service Bus</span><span class="sxs-lookup"><span data-stu-id="e5d58-110">Get started with Service Bus</span></span>
<span data-ttu-id="e5d58-111">Tato příručka předpokládá, že už máte obor názvů sběrnice obsahující frontu s názvem **Frontě1**.</span><span class="sxs-lookup"><span data-stu-id="e5d58-111">This guide assumes that you already have a Service Bus namespace containing a queue named **queue1**.</span></span> <span data-ttu-id="e5d58-112">Pokud ho použít nechcete, pak můžete [vytvořit obor názvů hello a fronty](service-bus-create-namespace-portal.md) pomocí hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e5d58-112">If you do not, then you can [create hello namespace and queue](service-bus-create-namespace-portal.md) using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e5d58-113">Další informace o tom, jak vidíte obory názvů Service Bus toocreate a fronty, [začít pracovat s fronty Service Bus](service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="e5d58-113">For more information about how toocreate Service Bus namespaces and queues, see [Get started with Service Bus queues](service-bus-dotnet-get-started-with-queues.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e5d58-114">Oddílů fronty a témata také podporují AMQP.</span><span class="sxs-lookup"><span data-stu-id="e5d58-114">Partitioned queues and topics also support AMQP.</span></span> <span data-ttu-id="e5d58-115">Další informace najdete v tématu [segmentované entity zasílání zpráv](service-bus-partitioning.md) a [podporu protokolu AMQP 1.0 pro Service Bus oddíly fronty a témata](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e5d58-115">For more information, see [Partitioned messaging entities](service-bus-partitioning.md) and [AMQP 1.0 support for Service Bus partitioned queues and topics](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span></span>
> 
> 

## <a name="downloading-hello-amqp-10-jms-client-library"></a><span data-ttu-id="e5d58-116">Stahuje se knihovna klienta protokolu AMQP 1.0 JMS hello</span><span class="sxs-lookup"><span data-stu-id="e5d58-116">Downloading hello AMQP 1.0 JMS client library</span></span>
<span data-ttu-id="e5d58-117">Informace o kde toodownload hello nejnovější verzi klientské knihovny hello Apache Qpid JMS protokolu AMQP 1.0, najdete v článku [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="e5d58-117">For information about where toodownload hello latest version of hello Apache Qpid JMS AMQP 1.0 client library, visit [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span></span>

<span data-ttu-id="e5d58-118">Musíte přidat hello následujících čtyř souborů JAR z hello Apache Qpid JMS protokolu AMQP 1.0 distribuční archivu toohello cestě třídy Java, při vytváření a spouštění aplikací JMS službou Service Bus:</span><span class="sxs-lookup"><span data-stu-id="e5d58-118">You must add hello following four JAR files from hello Apache Qpid JMS AMQP 1.0 distribution archive toohello Java CLASSPATH when building and running JMS applications with Service Bus:</span></span>

* <span data-ttu-id="e5d58-119">geronimo jms\_1.1\_specifikace 1.0.jar</span><span class="sxs-lookup"><span data-stu-id="e5d58-119">geronimo-jms\_1.1\_spec-1.0.jar</span></span>
* <span data-ttu-id="e5d58-120">qpid-amqp-1-0-Client-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="e5d58-120">qpid-amqp-1-0-client-[version].jar</span></span>
* <span data-ttu-id="e5d58-121">qpid-amqp-1-0-Client-jms-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="e5d58-121">qpid-amqp-1-0-client-jms-[version].jar</span></span>
* <span data-ttu-id="e5d58-122">qpid-amqp-1-0-Common-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="e5d58-122">qpid-amqp-1-0-common-[version].jar</span></span>

## <a name="coding-java-applications"></a><span data-ttu-id="e5d58-123">Kódování aplikací v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="e5d58-123">Coding Java applications</span></span>
### <a name="java-naming-and-directory-interface-jndi"></a><span data-ttu-id="e5d58-124">Pojmenování Java a Directory rozhraní (JNDI)</span><span class="sxs-lookup"><span data-stu-id="e5d58-124">Java Naming and Directory Interface (JNDI)</span></span>
<span data-ttu-id="e5d58-125">JMS používá hello pojmenovávání Java a Directory rozhraní (JNDI) toocreate oddělení mezi názvy logické a fyzické.</span><span class="sxs-lookup"><span data-stu-id="e5d58-125">JMS uses hello Java Naming and Directory Interface (JNDI) toocreate a separation between logical names and physical names.</span></span> <span data-ttu-id="e5d58-126">Dva typy objektů JMS se přeloží pomocí JNDI: ConnectionFactory a cíl.</span><span class="sxs-lookup"><span data-stu-id="e5d58-126">Two types of JMS objects are resolved using JNDI: ConnectionFactory and Destination.</span></span> <span data-ttu-id="e5d58-127">JNDI používá zprostředkovatele modelu, do kterého je možné připojit jiný adresář služby toohandle název řešení povinností.</span><span class="sxs-lookup"><span data-stu-id="e5d58-127">JNDI uses a provider model into which you can plug different directory services toohandle name resolution duties.</span></span> <span data-ttu-id="e5d58-128">Hello Apache Qpid JMS protokolu AMQP 1.0 knihovny se dodává s jednoduché vlastnosti na základě souborů JNDI zprostředkovatele, který je konfigurován pomocí vlastnosti soubor hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e5d58-128">hello Apache Qpid JMS AMQP 1.0 library comes with a simple properties file-based JNDI Provider that is configured using a properties file of hello following format:</span></span>

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using hello form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using hello form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-hello-connectionfactory"></a><span data-ttu-id="e5d58-129">Konfigurace hello ConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="e5d58-129">Configure hello ConnectionFactory</span></span>
<span data-ttu-id="e5d58-130">Hello toodefine položka používaná **ConnectionFactory** v hello Qpid vlastnosti souboru JNDI zprostředkovatele je hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e5d58-130">hello entry used toodefine a **ConnectionFactory** in hello Qpid properties file JNDI provider is of hello following format:</span></span>

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

<span data-ttu-id="e5d58-131">Kde **[jndi_name]** a **[ConnectionURL]** mají následující významy hello:</span><span class="sxs-lookup"><span data-stu-id="e5d58-131">Where **[jndi_name]** and **[ConnectionURL]** have hello following meanings:</span></span>

* <span data-ttu-id="e5d58-132">**[jndi_name]** : logický název hello ConnectionFactory hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-132">**[jndi_name]**: hello logical name of hello ConnectionFactory.</span></span> <span data-ttu-id="e5d58-133">Toto je hello název, který se vyřeší v aplikaci Java hello pomocí metody JNDI IntialContext.lookup() hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-133">This is hello name that will be resolved in hello Java application using hello JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="e5d58-134">**[ConnectionURL]** : Adresu URL, která poskytuje knihovna JMS hello hello informace požadované toohello AMQP zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="e5d58-134">**[ConnectionURL]**: A URL that provides hello JMS library with hello information required toohello AMQP broker.</span></span>

<span data-ttu-id="e5d58-135">Formát Hello hello **ConnectionURL** vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e5d58-135">hello format of hello **ConnectionURL** is as follows:</span></span>

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
<span data-ttu-id="e5d58-136">Kde **[obor názvů]**, **[SASPolicyName]** a **[SASPolicyKey]** mají následující významy hello:</span><span class="sxs-lookup"><span data-stu-id="e5d58-136">Where **[namespace]**, **[SASPolicyName]** and **[SASPolicyKey]** have hello following meanings:</span></span>

* <span data-ttu-id="e5d58-137">**[obor názvů]** : hello oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="e5d58-137">**[namespace]**: hello Service Bus namespace.</span></span>
* <span data-ttu-id="e5d58-138">**[SASPolicyName]** : název zásady fronty sdíleného přístupového podpisu hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-138">**[SASPolicyName]**: hello Queue Shared Access Signature policy name.</span></span>
* <span data-ttu-id="e5d58-139">**[SASPolicyKey]** : hello fronty sdíleného přístupového podpisu zásad klíč.</span><span class="sxs-lookup"><span data-stu-id="e5d58-139">**[SASPolicyKey]**: hello Queue Shared Access Signature policy key.</span></span>

> [!NOTE]
> <span data-ttu-id="e5d58-140">Kódování URL hello heslo musíte ručně.</span><span class="sxs-lookup"><span data-stu-id="e5d58-140">You must URL-encode hello password manually.</span></span> <span data-ttu-id="e5d58-141">Užitečné kódování URL nástroj je k dispozici na [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="e5d58-141">A useful URL-encoding utility is available at [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
> 
> 

#### <a name="configure-destinations"></a><span data-ttu-id="e5d58-142">Konfigurace cíle</span><span class="sxs-lookup"><span data-stu-id="e5d58-142">Configure destinations</span></span>
<span data-ttu-id="e5d58-143">Položka Hello používá toodefine cíl hello Qpid vlastnosti souboru JNDI zprostředkovatele je dobrý den následující formát:</span><span class="sxs-lookup"><span data-stu-id="e5d58-143">hello entry used toodefine a destination in hello Qpid properties file JNDI provider is of hello following format:</span></span>

```
queue.[jndi_name] = [physical_name]
```

<span data-ttu-id="e5d58-144">nebo</span><span class="sxs-lookup"><span data-stu-id="e5d58-144">or</span></span>

```
topic.[jndi_name] = [physical_name]
```

<span data-ttu-id="e5d58-145">Kde **[jndi\_název]** a **[fyzické\_název]** mají následující významy hello:</span><span class="sxs-lookup"><span data-stu-id="e5d58-145">Where **[jndi\_name]** and **[physical\_name]** have hello following meanings:</span></span>

* <span data-ttu-id="e5d58-146">**[jndi_name]** : hello logický název cílové hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-146">**[jndi_name]**: hello logical name of hello destination.</span></span> <span data-ttu-id="e5d58-147">Toto je hello název, který se vyřeší v aplikaci Java hello pomocí metody JNDI IntialContext.lookup() hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-147">This is hello name that will be resolved in hello Java application using hello JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="e5d58-148">**[physical_name]** : název hello hello Service Bus entity toowhich hello aplikace odeslání nebo přijetí zprávy.</span><span class="sxs-lookup"><span data-stu-id="e5d58-148">**[physical_name]**: hello name of hello Service Bus entity toowhich hello application sends or receives messages.</span></span>

> [!NOTE]
> <span data-ttu-id="e5d58-149">Při přijímání od odběr tématu Service Bus, by měl být hello fyzický název zadaný v JNDI hello název tématu hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-149">When receiving from a Service Bus topic subscription, hello physical name specified in JNDI should be hello name of hello topic.</span></span> <span data-ttu-id="e5d58-150">Název odběru Hello získáte při vytvoření odolné odběru hello v hello JMS kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5d58-150">hello subscription name is provided when hello durable subscription is created in hello JMS application code.</span></span> <span data-ttu-id="e5d58-151">Hello [Service Bus AMQP 1.0 – Příručka vývojáře](service-bus-amqp-dotnet.md) obsahuje další informace o práci s témat sběrnice Service Bus z JMS.</span><span class="sxs-lookup"><span data-stu-id="e5d58-151">hello [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) provides more details on working with Service Bus topics from JMS.</span></span>
> 
> 

### <a name="write-hello-jms-application"></a><span data-ttu-id="e5d58-152">Psát aplikace JMS hello</span><span class="sxs-lookup"><span data-stu-id="e5d58-152">Write hello JMS application</span></span>
<span data-ttu-id="e5d58-153">Neexistují žádné speciální rozhraní API nebo možnosti, které jsou potřeba při použití JMS službou Service Bus.</span><span class="sxs-lookup"><span data-stu-id="e5d58-153">There are no special APIs or options required when using JMS with Service Bus.</span></span> <span data-ttu-id="e5d58-154">Existují však několik omezení, kterým se budeme později.</span><span class="sxs-lookup"><span data-stu-id="e5d58-154">However, there are a few restrictions that will be covered later.</span></span> <span data-ttu-id="e5d58-155">Stejně jako u jakékoli aplikace JMS hello nejprve thing požadované je konfiguraci hello JNDI prostředí, může tooresolve toobe **ConnectionFactory** a cíle.</span><span class="sxs-lookup"><span data-stu-id="e5d58-155">As with any JMS application, hello first thing required is configuration of hello JNDI environment, toobe able tooresolve a **ConnectionFactory** and destinations.</span></span>

#### <a name="configure-hello-jndi-initialcontext"></a><span data-ttu-id="e5d58-156">Konfigurace hello JNDI InitialContext</span><span class="sxs-lookup"><span data-stu-id="e5d58-156">Configure hello JNDI InitialContext</span></span>
<span data-ttu-id="e5d58-157">Hello JNDI prostředí je nakonfigurováno pomocí předání informací o konfiguraci zatřiďovací tabulku do hello konstruktoru třídy javax.naming.InitialContext hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-157">hello JNDI environment is configured by passing a hashtable of configuration information into hello constructor of hello javax.naming.InitialContext class.</span></span> <span data-ttu-id="e5d58-158">dva požadované prvky Hello v zatřiďovací tabulce hello jsou název třídy hello hello počáteční objekt pro vytváření kontextu a hello adresa URL poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="e5d58-158">hello two required elements in hello hashtable are hello class name of hello Initial Context Factory and hello Provider URL.</span></span> <span data-ttu-id="e5d58-159">Hello následující kód ukazuje, jak tooconfigure hello JNDI prostředí toouse hello Qpid soubor vlastností na základě JNDI zprostředkovatele s vlastnosti souboru s názvem **servicebus.properties**.</span><span class="sxs-lookup"><span data-stu-id="e5d58-159">hello following code shows how tooconfigure hello JNDI environment toouse hello Qpid properties file based JNDI Provider with a properties file named **servicebus.properties**.</span></span>

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a><span data-ttu-id="e5d58-160">Jednoduché JMS aplikace pomocí fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="e5d58-160">A simple JMS application using a Service Bus queue</span></span>
<span data-ttu-id="e5d58-161">Hello následující ukázkový program odešle JMS TextMessages tooa fronty Service Bus s hello JNDI logický název fronty a přijímá zprávy hello zpět.</span><span class="sxs-lookup"><span data-stu-id="e5d58-161">hello following example program sends JMS TextMessages tooa Service Bus queue with hello JNDI logical name of QUEUE, and receives hello messages back.</span></span>

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
            System.out.println("Press [enter] toosend a message. Type 'exit' + [enter] tooquit.");
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

### <a name="run-hello-application"></a><span data-ttu-id="e5d58-162">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="e5d58-162">Run hello application</span></span>
<span data-ttu-id="e5d58-163">Spuštění aplikace hello vytváří výstup hello formuláře:</span><span class="sxs-lookup"><span data-stu-id="e5d58-163">Running hello application produces output of hello form:</span></span>

```
> java SimpleSenderReceiver
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a><span data-ttu-id="e5d58-164">Napříč platformami zasílání zpráv mezi JMS a rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e5d58-164">Cross-platform messaging between JMS and .NET</span></span>
<span data-ttu-id="e5d58-165">Tento průvodce vám ukázal, jak toosend a přijímat zprávy tooand ze služby Service Bus pomocí JMS.</span><span class="sxs-lookup"><span data-stu-id="e5d58-165">This guide showed how toosend and receive messages tooand from Service Bus using JMS.</span></span> <span data-ttu-id="e5d58-166">Mezi klíčové výhody protokolu AMQP 1.0 hello je však umožňuje toobe aplikace vytvořené ze součástí, které jsou napsané v různých jazycích, pomocí zprávy vyměňují spolehlivé a na úplné přesnost.</span><span class="sxs-lookup"><span data-stu-id="e5d58-166">However, one of hello key benefits of AMQP 1.0 is that it enables applications toobe built from components written in different languages, with messages exchanged reliably and at full fidelity.</span></span>

<span data-ttu-id="e5d58-167">Pomocí výše popsané hello ukázkové JMS aplikace a podobné aplikace .NET prováděné z článku doprovodné [pomocí Service Bus pomocí technologie .NET pomocí protokolu AMQP 1.0](service-bus-amqp-dotnet.md), můžou vyměňovat zprávy mezi .NET a Javu.</span><span class="sxs-lookup"><span data-stu-id="e5d58-167">Using hello sample JMS application described above and a similar .NET application taken from a companion article, [Using Service Bus from .NET with AMQP 1.0](service-bus-amqp-dotnet.md), you can exchange messages between .NET and Java.</span></span> <span data-ttu-id="e5d58-168">Si tento článek Další informace o hello podrobnosti napříč platformami zasílání zpráv pomocí sběrnice a protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="e5d58-168">Read this article for more information about hello details of cross-platform messaging using Service Bus and AMQP 1.0.</span></span>

### <a name="jms-toonet"></a><span data-ttu-id="e5d58-169">JMS too.NET</span><span class="sxs-lookup"><span data-stu-id="e5d58-169">JMS too.NET</span></span>
<span data-ttu-id="e5d58-170">toodemonstrate JMS too.NET zasílání zpráv:</span><span class="sxs-lookup"><span data-stu-id="e5d58-170">toodemonstrate JMS too.NET messaging:</span></span>

* <span data-ttu-id="e5d58-171">Spuštění ukázkové aplikace .NET hello bez argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e5d58-171">Start hello .NET sample application without any command-line arguments.</span></span>
* <span data-ttu-id="e5d58-172">Ukázkovou aplikaci Java hello začněte argument příkazového řádku "sendonly" hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-172">Start hello Java sample application with hello "sendonly" command-line argument.</span></span> <span data-ttu-id="e5d58-173">V tomto režimu hello aplikace nebude přijímat zprávy z fronty hello pouze odešle.</span><span class="sxs-lookup"><span data-stu-id="e5d58-173">In this mode, hello application will not receive messages from hello queue, it will only send.</span></span>
* <span data-ttu-id="e5d58-174">Stiskněte klávesu **Enter** několikrát v konzole aplikace hello Java, což způsobí, že toobe zprávy odeslané.</span><span class="sxs-lookup"><span data-stu-id="e5d58-174">Press **Enter** a few times in hello Java application console, which will cause messages toobe sent.</span></span>
* <span data-ttu-id="e5d58-175">Tyto zprávy jsou přijímány hello aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="e5d58-175">These messages are received by hello .NET application.</span></span>

#### <a name="output-from-jms-application"></a><span data-ttu-id="e5d58-176">Výstup z aplikace JMS</span><span class="sxs-lookup"><span data-stu-id="e5d58-176">Output from JMS application</span></span>
```
> java SimpleSenderReceiver sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a><span data-ttu-id="e5d58-177">Výstup z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="e5d58-177">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-toojms"></a><span data-ttu-id="e5d58-178">TooJMS rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e5d58-178">.NET tooJMS</span></span>
<span data-ttu-id="e5d58-179">tooJMS .NET toodemonstrate zasílání zpráv:</span><span class="sxs-lookup"><span data-stu-id="e5d58-179">toodemonstrate .NET tooJMS messaging:</span></span>

* <span data-ttu-id="e5d58-180">Spuštění ukázkové aplikace .NET hello s argumentem příkazového řádku "sendonly" hello.</span><span class="sxs-lookup"><span data-stu-id="e5d58-180">Start hello .NET sample application with hello "sendonly" command-line argument.</span></span> <span data-ttu-id="e5d58-181">V tomto režimu hello aplikace nebude přijímat zprávy z fronty hello pouze odešle.</span><span class="sxs-lookup"><span data-stu-id="e5d58-181">In this mode, hello application will not receive messages from hello queue, it will only send.</span></span>
* <span data-ttu-id="e5d58-182">Spusťte ukázkovou aplikaci Java hello bez argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e5d58-182">Start hello Java sample application without any command-line arguments.</span></span>
* <span data-ttu-id="e5d58-183">Stiskněte klávesu **Enter** několikrát v konzole aplikace hello .NET, což způsobí, že toobe zprávy odeslané.</span><span class="sxs-lookup"><span data-stu-id="e5d58-183">Press **Enter** a few times in hello .NET application console, which will cause messages toobe sent.</span></span>
* <span data-ttu-id="e5d58-184">Tyto zprávy jsou přijímány hello aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="e5d58-184">These messages are received by hello Java application.</span></span>

#### <a name="output-from-net-application"></a><span data-ttu-id="e5d58-185">Výstup z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="e5d58-185">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a><span data-ttu-id="e5d58-186">Výstup z aplikace JMS</span><span class="sxs-lookup"><span data-stu-id="e5d58-186">Output from JMS application</span></span>
```
> java SimpleSenderReceiver    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a><span data-ttu-id="e5d58-187">Nepodporované funkce a omezení</span><span class="sxs-lookup"><span data-stu-id="e5d58-187">Unsupported features and restrictions</span></span>
<span data-ttu-id="e5d58-188">Při použití JMS prostřednictvím protokolu AMQP 1.0 se Service Bus, konkrétně existují Hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="e5d58-188">hello following restrictions exist when using JMS over AMQP 1.0 with Service Bus, namely:</span></span>

* <span data-ttu-id="e5d58-189">Pouze jeden **MessageProducer** nebo **MessageConsumer** je povolena na **relace**.</span><span class="sxs-lookup"><span data-stu-id="e5d58-189">Only one **MessageProducer** or **MessageConsumer** is allowed per **Session**.</span></span> <span data-ttu-id="e5d58-190">Pokud potřebujete toocreate více **MessageProducers** nebo **MessageConsumers** v aplikaci, vytvořte vyhrazená **relace** pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="e5d58-190">If you need toocreate multiple **MessageProducers** or **MessageConsumers** in an application, create a dedicated **Session** for each of them.</span></span>
* <span data-ttu-id="e5d58-191">Odběry témat volatile nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="e5d58-191">Volatile topic subscriptions are not currently supported.</span></span>
* <span data-ttu-id="e5d58-192">**MessageSelectors** nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="e5d58-192">**MessageSelectors** are not currently supported.</span></span>
* <span data-ttu-id="e5d58-193">Dočasné cíle; například **TemporaryQueue**, **TemporaryTopic** nejsou aktuálně podporovány, společně s hello **QueueRequestor** a **TopicRequestor**Rozhraní API, která je používat.</span><span class="sxs-lookup"><span data-stu-id="e5d58-193">Temporary destinations; for example, **TemporaryQueue**, **TemporaryTopic** are not currently supported, along with hello **QueueRequestor** and **TopicRequestor** APIs that use them.</span></span>
* <span data-ttu-id="e5d58-194">Zpracované relací a distribuované transakce nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e5d58-194">Transacted sessions and distributed transactions are not supported.</span></span>

## <a name="summary"></a><span data-ttu-id="e5d58-195">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e5d58-195">Summary</span></span>
<span data-ttu-id="e5d58-196">Tento postup tooguide vám ukázal, jak hello toouse funkce Service Bus zprostředkované zasílání zpráv (fronty a publikování a přihlášení k odběru témata) z jazyk Java pomocí Oblíbené JMS rozhraní API a protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="e5d58-196">This how-tooguide showed how toouse Service Bus brokered messaging features (queues and publish/subscribe topics) from Java using hello popular JMS API and AMQP 1.0.</span></span>

<span data-ttu-id="e5d58-197">Můžete taky Service Bus protokolu AMQP 1.0 z dalších jazycích včetně .NET, C, Python nebo PHP.</span><span class="sxs-lookup"><span data-stu-id="e5d58-197">You can also use Service Bus AMQP 1.0 from other languages, including .NET, C, Python, and PHP.</span></span> <span data-ttu-id="e5d58-198">Součásti vytvořená s využitím těchto různých jazycích můžou vyměňovat zprávy, spolehlivé a na úplné věrnosti pomocí hello protokolu AMQP 1.0 podpory v Service Bus.</span><span class="sxs-lookup"><span data-stu-id="e5d58-198">Components built using these different languages can exchange messages reliably and at full fidelity using hello AMQP 1.0 support in Service Bus.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5d58-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5d58-199">Next steps</span></span>
* [<span data-ttu-id="e5d58-200">Podpora protokolu AMQP 1.0 v Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="e5d58-200">AMQP 1.0 support in Azure Service Bus</span></span>](service-bus-amqp-overview.md)
* [<span data-ttu-id="e5d58-201">Jak toouse protokolu AMQP 1.0 s hello rozhraní API služby Service Bus rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e5d58-201">How toouse AMQP 1.0 with hello Service Bus .NET API</span></span>](service-bus-dotnet-advanced-message-queuing.md)
* [<span data-ttu-id="e5d58-202">Service Bus AMQP 1.0 – Příručka vývojáře</span><span class="sxs-lookup"><span data-stu-id="e5d58-202">Service Bus AMQP 1.0 Developer's Guide</span></span>](service-bus-amqp-dotnet.md)
* [<span data-ttu-id="e5d58-203">Začínáme s frontami služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="e5d58-203">Get started with Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)
* [<span data-ttu-id="e5d58-204">Středisko pro vývojáře Java</span><span class="sxs-lookup"><span data-stu-id="e5d58-204">Java Developer Center</span></span>](https://azure.microsoft.com/develop/java/)

