---
title: "aaaUse škálování akce toosend e-mailu a webhooku oznámení výstrah. | Dokumentace Microsoftu"
description: "V tématu Jak toocall akce škálování toouse webové adresy URL nebo poslat e-mailová oznámení v nástroji Sledování Azure. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: f611a18f5a808412fbdd0c89e3addb36437064c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-autoscale-actions-toosend-email-and-webhook-alert-notifications-in-azure-monitor"></a>Použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure
Tento článek ukazuje, jak nastavíte aktivační události tak, aby můžete volat konkrétní webové adresy URL nebo poslat e-mailů podle akce škálování v Azure.  

## <a name="webhooks"></a>Webhooky
Webhooky povolit tooroute hello Azure oznámení o výstrahách tooother systémy pro následné zpracování nebo vlastních oznámení. Například směrování výstrah tooservices hello, které může zpracovat příchozí webové žádosti toosend SMS, protokolu chyb, upozornění týmu pomocí chatu nebo služby zasílání zpráv, atd. hello webhooku identifikátor URI musí být platný koncový bod HTTP nebo HTTPS.

## <a name="email"></a>E-mail
Tooany platné e-mailová adresa může být odeslán e-mail. Také upozorňování správci a spolusprávci předplatného hello, kde je spuštěno pravidlo hello.

## <a name="cloud-services-and-web-apps"></a>Cloudové služby a webové aplikace
Vám může výslovný souhlas z hello portál Azure pro cloudové služby a serverové farmy (webové aplikace).

* Zvolte hello **škálovat podle** metriku.

![škálování podle](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Sady škálování virtuálního počítače
Pro novější virtuální počítače vytvořené pomocí Resource Manageru (sady škálování virtuálního počítače) to můžete nakonfigurovat pomocí rozhraní REST API, šablony Resource Manageru, prostředí PowerShell a rozhraní příkazového řádku. Portál rozhraní dosud nejsou k dispozici.
Pokud používáte hello REST API nebo šablony Resource Manageru, zahrňte hello oznámení prvek s hello následující možnosti.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| Pole | Povinné? | Popis |
| --- | --- | --- |
| operace |Ano |Hodnota musí být "Škálování" |
| sendToSubscriptionAdministrator |Ano |Hodnota musí být "true" nebo "Nepravda" |
| sendToSubscriptionCoAdministrators |Ano |Hodnota musí být "true" nebo "Nepravda" |
| customEmails |Ano |Hodnota může být null [] nebo pole řetězců e-mailů |
| Webhooky |Ano |Hodnota může být null nebo platný identifikátor Uri |
| serviceUri |Ano |platný https identifikátor Uri |
| properties |Ano |Hodnota musí být prázdný {} nebo může obsahovat páry klíč hodnota |

## <a name="authentication-in-webhooks"></a>Ověřování v webhooky
Hello webhooku můžete ověřit pomocí ověřování založeného na token, kam jste uložili hello webhooku identifikátor URI s tokenu ID jako parametr dotazu. Například https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Škálování oznámení webhooku datové části schématu
Generování oznámení škálování hello hello následující metadata je zahrnuta v datové části webhooku hello:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' toocapacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Pole | Povinné? | Popis |
| --- | --- | --- |
| status |Ano |Hello stav, který označuje, že bylo vygenerováno akce škálování |
| operace |Ano |Pro zvýšení instancí bude "Horizontální navýšení kapacity" a pro snížení instancí, bude "Škálování v" |
| Kontext |Ano |kontext akce škálování Hello |
| časové razítko |Ano |Časové razítko, když byla spuštěna akce škálování hello |
| id |Ano |Správce prostředků ID nastavení automatického škálování hello |
| jméno |Ano |Název nastavení automatického škálování hello Hello |
| Podrobnosti |Ano |Vysvětlení hello akci trvalo hello škálování služby a hello změnit v počet instancí hello |
| subscriptionId |Ano |ID odběru hello cílový prostředek, který bude změněno měřítko |
| Název skupiny prostředků |Ano |Název skupiny prostředků hello cílový prostředek, který bude změněno měřítko |
| resourceName |Ano |Název hello cílový prostředek, který bude změněno měřítko |
| Typ prostředku |Ano |Hello tři podporované hodnoty: "microsoft.classiccompute/domainnames/slots/roles" – cloudové služby rolí, "microsoft.compute/virtualmachinescalesets" - sady škálování virtuálního počítače a "Microsoft.Web/serverfarms" - webové aplikace |
| resourceId |Ano |Správce prostředků ID hello cílový prostředek, který bude změněno měřítko |
| portalLink |Ano |Portál Azure odkaz toohello souhrnná stránka hello cílový prostředek |
| oldCapacity |Ano |Hello aktuální (starý) počet instancí při škálování trvalo akce škálování |
| newCapacity |Ano |nový počet instancí Hello škálování příliš škálovat prostředek hello|
| Vlastnosti |Ne |Volitelné. Sada < klíč a hodnotu > páry (například slovník < String, String >). Hello vlastnosti pole je nepovinné. Do vlastní uživatelské rozhraní nebo aplikací na základě logiky pracovního postupu můžete zadat klíče a hodnoty, které lze předat pomocí hello datové části. Alternativní způsob vlastní vlastnosti toopass zpět toohello odchozí webhooku volání je toouse hello webhooku URI samotné (jako parametry dotazu) |
