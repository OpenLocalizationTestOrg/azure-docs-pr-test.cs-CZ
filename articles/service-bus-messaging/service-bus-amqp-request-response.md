---
title: "AMQP 1.0 v operacích na základě požadavku odpovědi Azure Service Bus | Microsoft Docs"
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
ms.openlocfilehash: 756565b3da6e0a818d1ee3d5e17f942d96be14f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>AMQP 1.0 v Microsoft Azure Service Bus: na základě požadavku odpověď operace

Toto téma definuje seznam operací založené na požadavku nebo odpovědi Microsoft Azure Service Bus. Tyto informace jsou založeny na koncept pracovní AMQP správu verze 1.0.  
  
Podrobné úroveň protokolu AMQP 1.0 protokol průvodce, která vysvětluje, jak Service Bus implementuje a je založený na technické specifikace OASIS AMQP, najdete v článku [protokolu AMQP 1.0 v příručce protokol Azure Service Bus a Event Hubs](service-bus-amqp-protocol-guide.md).  
  
## <a name="concepts"></a>Koncepty  
  
### <a name="entity-description"></a>Popis entity  

Popis entity odkazuje buď Service Bus [QueueDescription třída](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription třída](/dotnet/api/microsoft.servicebus.messaging.topicdescription), nebo [SubscriptionDescription třída](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) objektu.  
  
### <a name="brokered-message"></a>Zprostředkované zprávy  

Reprezentuje zprávu v Service Bus, která se mapuje na zprávu protokolu AMQP. Mapování je definována v [Průvodce protokol Service Bus AMQP](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Připojit k uzlu správy entity  

Všechny operace, které jsou popsané v tomto dokumentu postupujte podle požadavků a odpovědí vzor, jsou omezená na entitu a vyžadovat připojení k uzlu správy entity.  
  
### <a name="create-link-for-sending-requests"></a>Vytvoření odkazu na odesílání žádostí  

Vytvoří připojení k uzlu správy pro odesílání požadavků.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Vytvoření odkazu pro příjem odpovědí  

Vytvoří odkaz pro příjem odpovědí z uzlu správy.  
  
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

Obdrží zprávu odpovědi z odkazu odpovědi.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
Zpráva odpovědi je v následující podobě:
  
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

Rozšíření zámek zprávy o za dobu určenou v popis entity.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
 Tělo zprávy žádost musí obsahovat části amqp hodnotu obsahující mapa s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|pole identifikátoru uuid|Ano|Zpráva tokeny zámek k obnovení.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo.|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat části amqp hodnotu obsahující mapa s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|vypršení platnosti|pole časového razítka|Ano|Zpráva zámku nové vypršení platnosti tokenu odpovídající na žádosti o tokeny zámku.|  
  
### <a name="peek-message"></a>Prohlížení zpráv  

Prohlížením zprávy, aniž by zámek.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|dlouhá|Ano|Pořadové číslo, od kterého má začít funkce Náhled.|  
|`message-count`|celá čísla|Ano|Maximální počet zpráv, které mají prohlížet.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, ve kterých každý mapy představuje zprávu.|  
  
Mapa představující zprávu musí obsahovat následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
### <a name="schedule-message"></a>Plán zpráv  

Naplánuje zprávy.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, ve kterých každý mapy představuje zprávu.|  
  
Mapa představující zprávu musí obsahovat následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id zprávy|Řetězec|Ano|`amqpMessage.Properties.MessageId`jako řetězec|  
|id relace|Řetězec|Ano|`amqpMessage.Properties.GroupId as string`|  
|klíč oddílu|Řetězec|Ano|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo.|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** části obsahující mapa s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadové číslo naplánované zpráv. Pořadové číslo se používá k zrušit.|  
  
### <a name="cancel-scheduled-message"></a>Zrušit naplánované zpráv  

Zruší naplánované zprávy.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadová čísla zpráv naplánované zrušit.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo.|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** části obsahující mapa s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadové číslo naplánované zpráv. Pořadové číslo se používá k zrušit.|  
  
## <a name="session-operations"></a>Operace relace  
  
### <a name="session-renew-lock"></a>Obnovení relace zámku  

Rozšíření zámek zprávy o za dobu určenou v popis entity.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id relace|Řetězec|Ano|ID relace.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** části obsahující mapa s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|vypršení platnosti|časové razítko|Ano|Nové vypršení platnosti.|  
  
### <a name="peek-session-message"></a>Prohlížet zprávy relace  

Prohlížením zprávy relace bez blokování.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|číslo od pořadí|dlouhá|Ano|Pořadové číslo, od kterého má začít funkce Náhled.|  
|počet zpráv|celá čísla|Ano|Maximální počet zpráv, které mají prohlížet.|  
|id relace|Řetězec|Ano|ID relace.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** části obsahující mapa s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, ve kterých každý mapy představuje zprávu.|  
  
 Mapa představující zprávu musí obsahovat následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
### <a name="set-session-state"></a>Stav relace sady  

Nastaví stav relace.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id relace|Řetězec|Ano|ID relace.|  
|Stav relace|pole bajtů|Ano|Neprůhledné binární data.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
### <a name="get-session-state"></a>Stav relace GET  

Získá stav relace.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|id relace|Řetězec|Ano|ID relace.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Stav relace|pole bajtů|Ano|Neprůhledné binární data.|  
  
### <a name="enumerate-sessions"></a>Výčet relací  

Vytvoří výčet relací na entity přenosu zpráv.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|poslední aktualizovat čas|časové razítko|Ano|Filtrovat, aby obsahovaly pouze relace aktualizován po daném okamžiku.|  
|Přeskočit|celá čísla|Ano|Přeskočte určitý počet relací.|  
|Horní|celá čísla|Ano|Maximální počet relací.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – má více zpráv<br /><br /> 0xcc: Ne obsahu – žádné další zprávy|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Přeskočit|celá čísla|Ano|Počet přeskočených relace, pokud se stavovým kódem 200.|  
|ID relace|Pole řetězců.|Ano|Pole ID, pokud je stavový kód 200 relací.|  
  
## <a name="rule-operations"></a>Pravidla operací  
  
### <a name="add-rule"></a>Přidat pravidlo  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Název pravidla|Řetězec|Ano|Název pravidla není včetně předplatného a tématu.|  
|Popis pravidla|mapy|Ano|Popis pravidla uvedeného v další části.|  
  
**Popis pravidla** mapy musí zahrnovat následující položky, kde **sql filtru** a **korelace filtru** se vzájemně vylučují:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Filtr SQL|mapy|Ano|`sql-filter`, jak je uvedeno v následující části.|  
|korelace filtru|mapy|Ano|`correlation-filter`, jak je uvedeno v následující části.|  
|Akce pravidla SQL|mapy|Ano|`sql-rule-action`, jak je uvedeno v následující části.|  
  
Mapy filtru sql musí zahrnovat následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|výraz|Řetězec|Ano|Výraz filtru SQL.|  
  
**Korelace filtru** mapa musí obsahovat alespoň jeden z následujících položek:  
  
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
|properties|mapy|Ne|Mapuje sběrnice [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
**Akce pravidla sql** mapy musí zahrnovat následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|výraz|Řetězec|Ano|Výraz akce SQL.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
### <a name="remove-rule"></a>Odebrat pravidlo  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Název pravidla|Řetězec|Ano|Název pravidla není včetně předplatného a tématu.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
## <a name="deferred-message-operations"></a>Operace odložené zpráv  
  
### <a name="receive-by-sequence-number"></a>Pořadové číslo, obdrží  

Přijímá odložené zprávy podle pořadových čísel.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|pořadová čísla|pole dlouho|Ano|Pořadová čísla.|  
|příjemce. vyrovnání režimu|ubyte|Ano|**Příjemce vyrovnání** režimu tak, jak je uvedeno v protokolu AMQP 1.0 jádra.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|  
  
Zpráva odpovědi musí obsahovat **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|cloud-zařízení|seznam mapování|Ano|Seznam zpráv, kde každý mapy představuje zprávu.|  
  
Mapa představující zprávu musí obsahovat následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|token zámku|UUID|Ano|Pokud token zámku `receiver-settle-mode` je 1.|  
|Zpráva|pole bajtů|Ano|Kódování přenosu zpráv protokolu AMQP 1.0.|  
  
### <a name="update-disposition-status"></a>Aktualizovat stav dispozice  

Aktualizuje stav dispozice odložené zpráv.  
  
#### <a name="request"></a>Žádost  

Zpráva požadavku musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|operace|Řetězec|Ano|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Ne|Operace serveru vypršení časového limitu v milisekundách.|  
  
Musí obsahovat text zprávy požadavku **amqp hodnotu** obsahující části **mapy** s následující položky:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|Stav dispozice|Řetězec|Ano|byla dokončena<br /><br /> opuštění<br /><br /> pozastaveno|  
|Zámek tokeny|pole identifikátoru uuid|Ano|Tokeny zámku zpráva se bude aktualizovat stav dispozice.|  
|Důvod nedoručených zpráv|Řetězec|Ne|Může být nastaven, pokud je nastaven stav dispozice **pozastaveno**.|  
|Popis nedoručených zpráv|Řetězec|Ne|Může být nastaven, pokud je nastaven stav dispozice **pozastaveno**.|  
|vlastnosti upravit|mapy|Ne|Seznam Service Bus zprostředkované vlastnosti zprávy, které chcete upravit.|  
  
#### <a name="response"></a>Odpověď  

Zpráva odpovědi musí zahrnovat následující vlastnosti aplikace:  
  
|Klíč|Typ hodnoty|Požaduje se|Hodnota obsahu|  
|---------|----------------|--------------|--------------------|  
|statusCode|celá čísla|Ano|Kód odpovědi HTTP [RFC2616]<br /><br /> 200: OK – úspěch, jinak se nezdařilo|  
|Popis_stavu|Řetězec|Ne|Popis stavu.|

## <a name="next-steps"></a>Další kroky

Další informace o AMQP a Service Bus, najdete na následujících odkazech:

* [Přehled protokolu AMQP Service Bus]
* [Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]
* [AMQP v Service Bus pro systém Windows Server]

[Přehled protokolu AMQP Service Bus]: service-bus-amqp-overview.md
[Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP v Service Bus pro systém Windows Server]: https://msdn.microsoft.com/library/dn574799.asp