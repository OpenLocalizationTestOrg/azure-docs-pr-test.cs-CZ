---
title: "aaaIssuer název a klíč vystavitele ve službě BizTalk Services | Microsoft Docs"
description: "Zjistěte, jak tooretrieve název vystavitele a klíč vystavitele pro Service Bus nebo řízení přístupu (ACS) ve službě BizTalk Services. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: cc84c2820724ae3e7fc7c40ddbcd83a169add911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Název a klíč vystavitele

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Služba Azure BizTalk Services používá hello název vystavitele sběrnice služby a klíč vystavitele a hello název vystavitele řízení přístupu a klíč vystavitele. Zejména:

| Úkol | Které název vystavitele a klíč vystavitele |
| --- | --- |
| Nasazení aplikace z Visual Studia |Název vystavitele řízení přístupu a klíč vystavitele |
| Konfigurace hello portálu služby Azure BizTalk |Název vystavitele řízení přístupu a klíč vystavitele |
| Vytvoření předávací LOB pomocí hello adaptér služby BizTalk v sadě Visual Studio |Název vystavitele sběrnice služby a klíč vystavitele |

Toto téma uvádí hello kroky tooretrieve hello název vystavitele a klíč vystavitele. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Název vystavitele řízení přístupu a klíč vystavitele
Hello název vystavitele řízení přístupu a klíč vystavitele používá hello následující:

* Aplikace služby BizTalk Azure vytvořit v sadě Visual Studio: toosuccessfully nasazení aplikace služby BizTalk v sadě Visual Studio tooAzure, zadejte hello název vystavitele řízení přístupu a klíč vystavitele. 
* Hello portálu Azure BizTalk Services: při vytváření služby BizTalk a otevřete hello portál služby BizTalk, název vystavitele řízení přístupu a klíč vystavitele se automaticky registruje pro vaše nasazení s hello stejné hodnoty řízení přístupu.

### <a name="get-hello-access-control-issuer-name-and-issuer-key"></a>Získat hello název vystavitele řízení přístupu a klíč vystavitele

toouse ACS pro ověřování a získání hello hodnoty název vystavitele a klíč vystavitele, hello celkové krokům patří:

1. Nainstalujte hello [rutin prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Přidání účtu Azure:`Add-AzureAccount`
3. Vrátí název předplatného:`get-azuresubscription`
4. Vyberte předplatné:`select-azuresubscription <name of your subscription>` 
5. Vytvořte nový obor názvů:`new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Příklad:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. Když hello nový obor názvů služby ACS je vytvořen (který může trvat několik minut), hodnoty název vystavitele a klíč vystavitele hello jsou uvedené v hello připojovací řetězec: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

toosummarize:  
Název vystavitele = SharedSecretIssuer  
Klíče vystavitele = SharedSecretKey

Další informace o hello [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) rutiny. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Název vystavitele sběrnice služby a klíč vystavitele
Název vystavitele sběrnice služby a klíč vystavitele jsou používány adaptér služby BizTalk. V projektu služby BizTalk v sadě Visual Studio používáte hello BizTalk Services adaptér tooconnect tooan v místním obchodní (LOB) systému. tooconnect, vytvořit hello obchodní předávání a zadejte podrobnosti o systémové vaše obchodní. Pokud v takovém případě zadáte také hello název vystavitele sběrnice služby a klíč vystavitele.

### <a name="tooretrieve-hello-service-bus-issuer-name-and-issuer-key"></a>tooretrieve hello název vystavitele sběrnice služby a klíč vystavitele
1. Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně hello vyberte **Service Bus**.
3. Zvolte svůj obor názvů. Hello hlavním panelu vyberte **informace o připojení**. Zobrazí se hello **výchozí vystavitele** (název vystavitele) a **výchozí klíč** (Issuer Key). Jejich hodnoty lze zkopírovat.  

toosummarize:  
Název vystavitele = výchozí vystavitele  
Klíče vystavitele = výchozí klíč

## <a name="next"></a>Další
Další témata týkající se služby Azure BizTalk Services:

* [Instalace hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Kurzy: Služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Viz také
* [Postupy: použití služby ACS Management Service tooConfigure identity služby](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk Services: Developer, Basic, Standard a Premium tabulka edic](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Zřízení pomocí Azure portál classic](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Tabulka stavů zřízení](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Karty Řídicí panel, Sledování a Škálování](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Zálohování a obnovení](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Omezování](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

