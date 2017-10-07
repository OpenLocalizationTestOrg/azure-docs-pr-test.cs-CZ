---
title: "nastavení aaaCustom pro prostředí App Service"
description: "Vlastní konfigurace nastavení pro prostředí App Service"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 3d140688c88b389e71bfdd465c418339cccab3a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Vlastní konfigurace nastavení pro prostředí App Service
## <a name="overview"></a>Přehled
Protože prostředí App Service jsou izolované tooa jednoho zákazníka, existují určité nastavení konfigurace, které mohou být použity výhradně tooApp služby prostředí. Tento článek dokumenty hello různé zvláštními úpravami, které jsou k dispozici pro prostředí App Service.

Pokud nemáte služby App Service Environment, přečtěte si téma [jak tooCreate služby App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).

Přizpůsobení App Service Environment může ukládat pomocí pole v hello nové **clusterSettings** atribut. Tento atribut je součástí slovník "Vlastnosti" hello hello *hostingEnvironments* entity Azure Resource Manager.

Hello následující fragment kódu šablony Resource Manageru zkrácené ukazuje hello **clusterSettings** atribut:

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Hello **clusterSettings** atribut můžou být součástí hello tooupdate šablony správce prostředků služby App Service Environment.

## <a name="use-azure-resource-explorer-tooupdate-an-app-service-environment"></a>Použití Průzkumníka prostředků Azure tooupdate služby App Service Environment
Alternativně můžete aktualizovat hello App Service Environment pomocí [Průzkumníka prostředků Azure](https://resources.azure.com).  

1. V Průzkumníku prostředků, přejděte toohello uzel pro hello App Service Environment (**odběry** > **Skupinyprostředků** > **zprostředkovatelé**  >  **Microsoft.Web** > **hostingEnvironments**). Pak klikněte na tlačítko hello konkrétní App Service Environment, které chcete tooupdate.
2. V pravém podokně hello, klikněte na **pro čtení a zápis** v tooallow horním panelu nástrojů hello interaktivní úpravy v Průzkumníku prostředků.  
3. Klikněte na tlačítko hello blue **upravit** šablony Resource Manageru hello tlačítko toomake je upravovat.
4. Posuňte se konec toohello v pravém podokně hello. Hello **clusterSettings** na velmi dolní hello, kde můžete zadat nebo aktualizovat její hodnota je atribut.
5. Typ (nebo kopírování a vkládání) hello pole hodnot konfigurace, které chcete v hello **clusterSettings** atribut.  
6. Klikněte na zelenou hello **PUT** tlačítko, který je umístěný na začátku hello hello pravém podokně toocommit hello změnu toohello App Service Environment.

Ale odešlete změny hello trvá přibližně 30 minut násobí hodnotou hello počet front zakončení v hello App Service Environment pro hello změnit tootake efekt.
Například pokud služby App Service Environment má čtyři front-end, bude trvat zhruba dvě hodiny pro toofinish aktualizace konfigurace hello. Při změně konfigurace hello je se nasazuje, žádná jiná operace škálování nebo operací změny konfigurace můžete provést v hello App Service Environment.

## <a name="disable-tls-10"></a>Zakázat protokol TLS 1.0
Opakované otázku od zákazníků, hlavně zákazníkům, kteří se zabývají soulad s normami PCI audity, je, jak tooexplicitly zákaz protokolu TLS 1.0 pro svoje aplikace.

Protokol TLS 1.0 je možné zakázat prostřednictvím hello následující **clusterSettings** položku:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Pořadí sady šifer TLS změn
Další otázka od zákazníků se pokud mohou upravit hello seznam šifry vyjednaném jejich serveru a toho lze dosáhnout úpravou hello **clusterSettings** jak je uvedeno níže. seznam Hello k dispozici šifrovací sady lze načíst z [tohoto článku na webu MSDN](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> Pokud nesprávné hodnoty jsou nastavené pro hello šifrovací sada, která SChannel nelze pochopit, všechny TLS komunikace tooyour server může přestat fungovat. V takovém případě budete potřebovat tooremove hello *FrontEndSSLCipherSuiteOrder* položku z **clusterSettings** a odeslání hello aktualizovat Resource Manager šablony toorevert back toohello výchozí šifrovací Sada nastavení.  Použijte prosím tato funkce se zvýšenou opatrností.
> 
> 

## <a name="get-started"></a>Začínáme
Web šablony správce prostředků Azure rychlý start Hello obsahuje šablonu s hello základní definice [vytváření služby App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
