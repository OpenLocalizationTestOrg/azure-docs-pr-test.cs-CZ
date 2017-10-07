---
title: "aaaAMQP 1.0 v operacích na základě požadavku odpovědi Azure Service Bus | Microsoft Docs"
description: "Seznam operací založené na požadavku nebo odpovědi Microsoft Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: e4f26219c53b0c4172747af683fe511d6366ff2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>AMQP 1.0 v Microsoft Azure Service Bus: na základě požadavku odpověď operace

Toto téma definuje hello seznam operací založené na požadavku nebo odpovědi Microsoft Azure Service Bus. Tato informace jsou založeny na koncept pracovní hello AMQP správu verze 1.0.  
  
Podrobné úroveň protokolu AMQP 1.0 protokol průvodce, která vysvětluje, jak Service Bus implementuje a je založený na hello technické specifikace OASIS AMQP, najdete v části hello [protokolu AMQP 1.0 v příručce protokol Azure Service Bus a Event Hubs](service-bus-amqp-protocol-guide.md).  
  
## <a name="concepts"></a>Koncepty  
  
### <a name="entity-description"></a>Popis entity  

Odkazuje entity popis tooeither Service Bus [QueueDescription třída](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription třída](/dotnet/api/microsoft.servicebus.messaging.topicdescription), nebo [SubscriptionDescription třída](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) objektu.  
  
### <a name="brokered-message"></a>Zprostředkované zprávy  

Reprezentuje zprávu v Service Bus, které je namapované tooan AMQP zprávy. mapování Hello je definována v hello [Průvodce protokol Service Bus AMQP](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-tooentity-management-node"></a>Připojte tooentity uzel správy  

Všechny operace hello popsané v tomto dokumentu postupujte podle požadavků a odpovědí vzor, jsou vymezená tooan entity a vyžadují připojení uzlu správy tooan entity.  
  
### <a name="create-link-for-sending-requests"></a>Vytvoření odkazu na odesílání žádostí  

Vytvoří odkaz toohello uzlu pro správu pro odesílání požadavků.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Vytvoření odkazu pro příjem odpovědí  

Vytvoří odkaz pro příjem odpovědí z uzlu Správa hello.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Přenos zprávu požadavku  

Přenáší zprávu požadavku.  
  
```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                },  
        )  
```  
  
### <a name="receive-a-response-message"></a>Zobrazí zprávu odpovědi  

Obdrží zprávu odpovědi hello hello odpovědi odkaz.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
zpráva odpovědi Hello je v hello následující formulář:
  
```  
Message(  
properties: {     
        correlation-id: <request id>  
    },  
    application-properties: {  
            "statusCode" -> <status code>,  
            "statusDescription" -> <status description>,  
           },         
)  
  
```  
  
### <a name="service-bus-entity-address"></a>Adresa entity služby Service Bus  

Entit služby Service Bus je potřeba řešit následujícím způsobem:  
  
|Typ entity|Adresa|Příklad|  
|-----------------|-------------|-------------|  
|Fronty|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|Téma|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|předplatné|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Operace zpráv  
  
### <a name="message-renew-lock"></a>Zpráva obnovení zámku  

Rozšíření hello zámek zprávy o hello dobu uvedenou v popis entity hello.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
 tělo zprávy Hello žádost musí obsahovat části amqp hodnotu obsahující mapu s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|pole identifikátoru uuid|Ano|Zpráva toorenew tokeny zámku.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo.|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
zpráva odpovědi Hello musí obsahovat části amqp hodnotu obsahující mapu s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|vypršení platnosti|pole časového razítka|Ano|Zpráva uzamčení tokenu nové vypršení platnosti odpovídající toohello žádosti o zámek tokeny.|  
  
### <a name="peek-message"></a>Prohlížení zpráv  

Prohlížením zprávy, aniž by zámek.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|dlouhá|Ano|Pořadové číslo z které toostart funkce Náhled.|  
|`message-count`|celá čísla|Ano|Maximální počet zpráv toopeek.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, ve kterých každý mapy představuje zprávu.|  
  
Mapa Hello představující zprávu musí obsahovat hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
### <a name="schedule-message"></a>Plán zpráv  

Naplánuje zprávy.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, ve kterých každý mapy představuje zprávu.|  
  
Mapa Hello představující zprávu musí obsahovat hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id zprávy|Řetězec|Ano|`amqpMessage.Properties.MessageId`jako řetězec|  
|id relace|Řetězec|Ano|`amqpMessage.Properties.GroupId as string`|  
|klíč oddílu|Řetězec|Ano|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo.|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** části obsahující mapu s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadové číslo naplánované zpráv. Pořadové číslo je použité toocancel.|  
  
### <a name="cancel-scheduled-message"></a>Zrušit naplánované zpráv  

Zruší naplánované zprávy.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadová čísla toocancel naplánované zprávy.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo.|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** části obsahující mapu s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadové číslo naplánované zpráv. Pořadové číslo je použité toocancel.|  
  
## <a name="session-operations"></a>Operace relace  
  
### <a name="session-renew-lock"></a>Obnovení relace zámku  

Rozšíření hello zámek zprávy o hello dobu uvedenou v popis entity hello.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id relace|Řetězec|Ano|ID relace.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** části obsahující mapu s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|vypršení platnosti|časové razítko|Ano|Nové vypršení platnosti.|  
  
### <a name="peek-session-message"></a>Prohlížet zprávy relace  

Prohlížením zprávy relace bez blokování.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|číslo od pořadí|dlouhá|Ano|Pořadové číslo z které toostart funkce Náhled.|  
|počet zpráv|celá čísla|Ano|Maximální počet zpráv toopeek.|  
|id relace|Řetězec|Ano|ID relace.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** části obsahující mapu s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, ve kterých každý mapy představuje zprávu.|  
  
 Mapa Hello představující zprávu musí obsahovat hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
### <a name="set-session-state"></a>Stav relace sady  

Nastaví hello stavu relace.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id relace|Řetězec|Ano|ID relace.|  
|Stav relace|pole bajtů|Ano|Neprůhledné binární data.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
### <a name="get-session-state"></a>Stav relace GET  

Získá hello stav relace.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id relace|Řetězec|Ano|ID relace.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Stav relace|pole bajtů|Ano|Neprůhledné binární data.|  
  
### <a name="enumerate-sessions"></a>Výčet relací  

Vytvoří výčet relací na entity přenosu zpráv.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|poslední aktualizovat čas|časové razítko|Ano|Filtrujte jenom relací tooinclude aktualizován po daném okamžiku.|  
|Přeskočit|celá čísla|Ano|Přeskočte určitý počet relací.|  
|Horní|celá čísla|Ano|Maximální počet relací.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Přeskočit|celá čísla|Ano|Počet přeskočených relace, pokud se stavovým kódem 200.|  
|ID relace|Pole řetězců.|Ano|Pole ID, pokud je stavový kód 200 relací.|  
  
## <a name="rule-operations"></a>Pravidla operací  
  
### <a name="add-rule"></a>Přidat pravidlo  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Název pravidla|Řetězec|Ano|Název pravidla není včetně předplatného a tématu.|  
|Popis pravidla|mapy|Ano|Popis pravidla uvedeného v další části.|  
  
Hello **popis pravidla** mapa musí obsahovat hello následující položky, kde **sql filtru** a **korelace filtru** se vzájemně vylučují:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Filtr SQL|mapy|Ano|`sql-filter`, jak je uvedeno v další části hello.|  
|korelace filtru|mapy|Ano|`correlation-filter`, jak je uvedeno v další části hello.|  
|Akce pravidla SQL|mapy|Ano|`sql-rule-action`, jak je uvedeno v další části hello.|  
  
Mapa sql filtru Hello musí zahrnovat hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|výraz|Řetězec|Ano|Výraz filtru SQL.|  
  
Hello **korelace filtru** mapa musí obsahovat alespoň jeden z hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id korelace|Řetězec|Ne||  
|id zprávy|Řetězec|Ne||  
|na|Řetězec|Ne||  
|odpověď pro|Řetězec|Ne||  
|Popisek|Řetězec|Ne||  
|id relace|Řetězec|Ne||  
|odpověď k relaci id|Řetězec|Ne||  
|Typ obsahu|Řetězec|Ne||  
|properties|mapy|Ne|Mapuje tooService sběrnice [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
Hello **akce pravidla sql** mapa musí obsahovat hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|výraz|Řetězec|Ano|Výraz akce SQL.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
### <a name="remove-rule"></a>Odebrat pravidlo  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Název pravidla|Řetězec|Ano|Název pravidla není včetně předplatného a tématu.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
## <a name="deferred-message-operations"></a>Operace odložené zpráv  
  
### <a name="receive-by-sequence-number"></a>Pořadové číslo, obdrží  

Přijímá odložené zprávy podle pořadových čísel.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadová čísla.|  
|příjemce. vyrovnání režimu|ubyte|Ano|**Příjemce vyrovnání** režimu tak, jak je uvedeno v protokolu AMQP 1.0 jádra.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|  
  
musí obsahovat text zprávy odpovědi Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, kde každý mapy představuje zprávu.|  
  
Mapa Hello představující zprávu musí obsahovat hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|token zámku|UUID|Ano|Pokud token zámku `receiver-settle-mode` je 1.|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
### <a name="update-disposition-status"></a>Aktualizovat stav dispozice  

Aktualizuje stav dispozice hello odložené zpráv.  
  
#### <a name="request"></a>Žádost  

zpráva požadavku Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
musí obsahovat text zprávy požadavku Hello **amqp hodnotu** obsahující části **mapy** s hello následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Stav dispozice|Řetězec|Ano|byla dokončena<br /><br /> opuštění<br /><br /> pozastaveno|  
|Zámek tokeny|pole identifikátoru uuid|Ano|Stav zpráv zámku tokeny tooupdate dispozice.|  
|Důvod nedoručených zpráv|Řetězec|Ne|Může být nastaven, pokud stav dispozice nastaven příliš**pozastaveno**.|  
|Popis nedoručených zpráv|Řetězec|Ne|Může být nastaven, pokud stav dispozice nastaven příliš**pozastaveno**.|  
|vlastnosti upravit|mapy|Ne|Seznam Service Bus zprostředkované toomodify vlastnosti zprávy.|  
  
#### <a name="response"></a>Odpověď  

zpráva odpovědi Hello musí zahrnovat následující vlastnosti aplikace hello:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu hello.|

## <a name="next-steps"></a>Další kroky

toolearn informace o protokolu AMQP a Service Bus, najdete na adrese hello následující odkazy:

* [Přehled protokolu AMQP Service Bus]
* [Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]
* [AMQP v Service Bus pro systém Windows Server]

[Přehled protokolu AMQP Service Bus]: service-bus-amqp-overview.md
[Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP v Service Bus pro systém Windows Server]: https://msdn.microsoft.com/library/dn574799.asp