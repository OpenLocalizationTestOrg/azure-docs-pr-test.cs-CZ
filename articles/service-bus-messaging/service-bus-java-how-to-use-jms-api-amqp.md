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
# <a name="how-toouse-hello-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Jak toouse hello Java zprávy služby (JMS) rozhraní API se Service Bus a protokolu AMQP 1.0
Hello Advanced zpráv služby Řízení front Protocol (AMQP) 1.0 je efektivní, spolehlivou a úroveň protokolu zasílání zpráv, které můžete použít toobuild robustní, multiplatformní aplikace zasílání zpráv.

Podpora protokolu AMQP 1.0 v Service Bus prostředky, které můžete použít hello služby Řízení front a publikování a přihlášení k odběru zprostředkované zasílání zpráv funkcí z rozsahu platforem a efektivní binární protokol. Kromě toho můžete vytvořit aplikace skládá z komponenty sestaven pomocí kombinace jazyky, architektury a operační systémy.

Tento článek vysvětluje, jak hello toouse funkce Service Bus zasílání zpráv (fronty a publikování a přihlášení k odběru témata) z aplikací v jazyce Java pomocí Oblíbené Java zprávy služby (JMS) standardní rozhraní API. Je [doprovodný článek](service-bus-amqp-dotnet.md) to vysvětluje, jak hello toodo stejné pomocí hello Service Bus .NET API. Můžete použít tyto dvě příručky společně toolearn o zasílání zpráv mezi platformami pomocí protokolu AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Začínáme se službou Service Bus
Tato příručka předpokládá, že už máte obor názvů sběrnice obsahující frontu s názvem **Frontě1**. Pokud ho použít nechcete, pak můžete [vytvořit obor názvů hello a fronty](service-bus-create-namespace-portal.md) pomocí hello [portál Azure](https://portal.azure.com). Další informace o tom, jak vidíte obory názvů Service Bus toocreate a fronty, [začít pracovat s fronty Service Bus](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Oddílů fronty a témata také podporují AMQP. Další informace najdete v tématu [segmentované entity zasílání zpráv](service-bus-partitioning.md) a [podporu protokolu AMQP 1.0 pro Service Bus oddíly fronty a témata](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-hello-amqp-10-jms-client-library"></a>Stahuje se knihovna klienta protokolu AMQP 1.0 JMS hello
Informace o kde toodownload hello nejnovější verzi klientské knihovny hello Apache Qpid JMS protokolu AMQP 1.0, najdete v článku [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Musíte přidat hello následujících čtyř souborů JAR z hello Apache Qpid JMS protokolu AMQP 1.0 distribuční archivu toohello cestě třídy Java, při vytváření a spouštění aplikací JMS službou Service Bus:

* geronimo jms\_1.1\_specifikace 1.0.jar
* qpid-amqp-1-0-Client-[Version].JAR
* qpid-amqp-1-0-Client-jms-[Version].JAR
* qpid-amqp-1-0-Common-[Version].JAR

## <a name="coding-java-applications"></a>Kódování aplikací v jazyce Java
### <a name="java-naming-and-directory-interface-jndi"></a>Pojmenování Java a Directory rozhraní (JNDI)
JMS používá hello pojmenovávání Java a Directory rozhraní (JNDI) toocreate oddělení mezi názvy logické a fyzické. Dva typy objektů JMS se přeloží pomocí JNDI: ConnectionFactory a cíl. JNDI používá zprostředkovatele modelu, do kterého je možné připojit jiný adresář služby toohandle název řešení povinností. Hello Apache Qpid JMS protokolu AMQP 1.0 knihovny se dodává s jednoduché vlastnosti na základě souborů JNDI zprostředkovatele, který je konfigurován pomocí vlastnosti soubor hello následující formát:

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

#### <a name="configure-hello-connectionfactory"></a>Konfigurace hello ConnectionFactory
Hello toodefine položka používaná **ConnectionFactory** v hello Qpid vlastnosti souboru JNDI zprostředkovatele je hello následující formát:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Kde **[jndi_name]** a **[ConnectionURL]** mají následující významy hello:

* **[jndi_name]** : logický název hello ConnectionFactory hello. Toto je hello název, který se vyřeší v aplikaci Java hello pomocí metody JNDI IntialContext.lookup() hello.
* **[ConnectionURL]** : Adresu URL, která poskytuje knihovna JMS hello hello informace požadované toohello AMQP zprostředkovatele.

Formát Hello hello **ConnectionURL** vypadá takto:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Kde **[obor názvů]**, **[SASPolicyName]** a **[SASPolicyKey]** mají následující významy hello:

* **[obor názvů]** : hello oboru názvů Service Bus.
* **[SASPolicyName]** : název zásady fronty sdíleného přístupového podpisu hello.
* **[SASPolicyKey]** : hello fronty sdíleného přístupového podpisu zásad klíč.

> [!NOTE]
> Kódování URL hello heslo musíte ručně. Užitečné kódování URL nástroj je k dispozici na [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Konfigurace cíle
Položka Hello používá toodefine cíl hello Qpid vlastnosti souboru JNDI zprostředkovatele je dobrý den následující formát:

```
queue.[jndi_name] = [physical_name]
```

nebo

```
topic.[jndi_name] = [physical_name]
```

Kde **[jndi\_název]** a **[fyzické\_název]** mají následující významy hello:

* **[jndi_name]** : hello logický název cílové hello. Toto je hello název, který se vyřeší v aplikaci Java hello pomocí metody JNDI IntialContext.lookup() hello.
* **[physical_name]** : název hello hello Service Bus entity toowhich hello aplikace odeslání nebo přijetí zprávy.

> [!NOTE]
> Při přijímání od odběr tématu Service Bus, by měl být hello fyzický název zadaný v JNDI hello název tématu hello. Název odběru Hello získáte při vytvoření odolné odběru hello v hello JMS kódu aplikace. Hello [Service Bus AMQP 1.0 – Příručka vývojáře](service-bus-amqp-dotnet.md) obsahuje další informace o práci s témat sběrnice Service Bus z JMS.
> 
> 

### <a name="write-hello-jms-application"></a>Psát aplikace JMS hello
Neexistují žádné speciální rozhraní API nebo možnosti, které jsou potřeba při použití JMS službou Service Bus. Existují však několik omezení, kterým se budeme později. Stejně jako u jakékoli aplikace JMS hello nejprve thing požadované je konfiguraci hello JNDI prostředí, může tooresolve toobe **ConnectionFactory** a cíle.

#### <a name="configure-hello-jndi-initialcontext"></a>Konfigurace hello JNDI InitialContext
Hello JNDI prostředí je nakonfigurováno pomocí předání informací o konfiguraci zatřiďovací tabulku do hello konstruktoru třídy javax.naming.InitialContext hello. dva požadované prvky Hello v zatřiďovací tabulce hello jsou název třídy hello hello počáteční objekt pro vytváření kontextu a hello adresa URL poskytovatele. Hello následující kód ukazuje, jak tooconfigure hello JNDI prostředí toouse hello Qpid soubor vlastností na základě JNDI zprostředkovatele s vlastnosti souboru s názvem **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Jednoduché JMS aplikace pomocí fronty Service Bus
Hello následující ukázkový program odešle JMS TextMessages tooa fronty Service Bus s hello JNDI logický název fronty a přijímá zprávy hello zpět.

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

### <a name="run-hello-application"></a>Spuštění aplikace hello
Spuštění aplikace hello vytváří výstup hello formuláře:

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

## <a name="cross-platform-messaging-between-jms-and-net"></a>Napříč platformami zasílání zpráv mezi JMS a rozhraní .NET
Tento průvodce vám ukázal, jak toosend a přijímat zprávy tooand ze služby Service Bus pomocí JMS. Mezi klíčové výhody protokolu AMQP 1.0 hello je však umožňuje toobe aplikace vytvořené ze součástí, které jsou napsané v různých jazycích, pomocí zprávy vyměňují spolehlivé a na úplné přesnost.

Pomocí výše popsané hello ukázkové JMS aplikace a podobné aplikace .NET prováděné z článku doprovodné [pomocí Service Bus pomocí technologie .NET pomocí protokolu AMQP 1.0](service-bus-amqp-dotnet.md), můžou vyměňovat zprávy mezi .NET a Javu. Si tento článek Další informace o hello podrobnosti napříč platformami zasílání zpráv pomocí sběrnice a protokolu AMQP 1.0.

### <a name="jms-toonet"></a>JMS too.NET
toodemonstrate JMS too.NET zasílání zpráv:

* Spuštění ukázkové aplikace .NET hello bez argumentů příkazového řádku.
* Ukázkovou aplikaci Java hello začněte argument příkazového řádku "sendonly" hello. V tomto režimu hello aplikace nebude přijímat zprávy z fronty hello pouze odešle.
* Stiskněte klávesu **Enter** několikrát v konzole aplikace hello Java, což způsobí, že toobe zprávy odeslané.
* Tyto zprávy jsou přijímány hello aplikace .NET.

#### <a name="output-from-jms-application"></a>Výstup z aplikace JMS
```
> java SimpleSenderReceiver sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Výstup z aplikace .NET
```
> SimpleSenderReceiver.exe    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-toojms"></a>TooJMS rozhraní .NET
tooJMS .NET toodemonstrate zasílání zpráv:

* Spuštění ukázkové aplikace .NET hello s argumentem příkazového řádku "sendonly" hello. V tomto režimu hello aplikace nebude přijímat zprávy z fronty hello pouze odešle.
* Spusťte ukázkovou aplikaci Java hello bez argumentů příkazového řádku.
* Stiskněte klávesu **Enter** několikrát v konzole aplikace hello .NET, což způsobí, že toobe zprávy odeslané.
* Tyto zprávy jsou přijímány hello aplikace v jazyce Java.

#### <a name="output-from-net-application"></a>Výstup z aplikace .NET
```
> SimpleSenderReceiver.exe sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Výstup z aplikace JMS
```
> java SimpleSenderReceiver    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nepodporované funkce a omezení
Při použití JMS prostřednictvím protokolu AMQP 1.0 se Service Bus, konkrétně existují Hello následující omezení:

* Pouze jeden **MessageProducer** nebo **MessageConsumer** je povolena na **relace**. Pokud potřebujete toocreate více **MessageProducers** nebo **MessageConsumers** v aplikaci, vytvořte vyhrazená **relace** pro každý z nich.
* Odběry témat volatile nejsou aktuálně podporovány.
* **MessageSelectors** nejsou aktuálně podporovány.
* Dočasné cíle; například **TemporaryQueue**, **TemporaryTopic** nejsou aktuálně podporovány, společně s hello **QueueRequestor** a **TopicRequestor**Rozhraní API, která je používat.
* Zpracované relací a distribuované transakce nejsou podporovány.

## <a name="summary"></a>Souhrn
Tento postup tooguide vám ukázal, jak hello toouse funkce Service Bus zprostředkované zasílání zpráv (fronty a publikování a přihlášení k odběru témata) z jazyk Java pomocí Oblíbené JMS rozhraní API a protokolu AMQP 1.0.

Můžete taky Service Bus protokolu AMQP 1.0 z dalších jazycích včetně .NET, C, Python nebo PHP. Součásti vytvořená s využitím těchto různých jazycích můžou vyměňovat zprávy, spolehlivé a na úplné věrnosti pomocí hello protokolu AMQP 1.0 podpory v Service Bus.

## <a name="next-steps"></a>Další kroky
* [Podpora protokolu AMQP 1.0 v Azure Service Bus](service-bus-amqp-overview.md)
* [Jak toouse protokolu AMQP 1.0 s hello rozhraní API služby Service Bus rozhraní .NET](service-bus-dotnet-advanced-message-queuing.md)
* [Service Bus AMQP 1.0 – Příručka vývojáře](service-bus-amqp-dotnet.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/)

