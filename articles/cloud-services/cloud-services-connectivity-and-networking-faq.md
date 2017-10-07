---
title: "aaaConnectivity a sítě problémy pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje seznam hello časté otázky k připojení a sítě pro Microsoft Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: e725dbbf585a76807362c59299d0a31f511afd3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy s připojením a sítě pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy týkající se problémů s připojením a sítě pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres
Nejdřív zkontrolujte, zda je zapnutý tuto instanci hello virtuální počítač, který se pokoušíte tooreserve hello IP pro. Poté se ujistěte, že používáte vyhrazené IP adresy pro obě hello pracovní a provozní nasazení. **Nechcete** změnit nastavení hello při nasazení hello je upgradu.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Jak mohu vzdálené plochy Pokud skupinu NSG?
Přidat toohello pravidla NSG, který povolí komunikaci na portech **3389** a **20000**.  Vzdálená plocha používá port **3389**.  Cloudové služby instance jsou vyrovnáváním, zatížení, takže nemůže přímo řídit které tooconnect instance k.  Hello *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a povolit hello klienta toosend soubor cookie s RDP a zadejte jednotlivé instance tooconnect k.  Hello *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000** otevřít, což může být zablokován, pokud máte skupinu NSG.

## <a name="can-i-ping-a-cloud-service"></a>Můžete příkaz ping Cloudová služba?

Ne, nikoli pomocí normální hello "ping" / protokolu ICMP. pomocí nástroje pro vyrovnávání zatížení Azure hello není povoleno Hello protokolu ICMP.

tootest připojení, doporučujeme, abyste provedli port ping. Zatímco Ping.exe používá ICMP, jiné nástroje, jako je například Pspingu, Nmap a telnet povolit tootest připojení tooa specifického portu TCP.

Další informace najdete v tématu [místo ICMP tootest připojení virtuálního počítače Azure používat příkazy ping port](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-toohello-cloud-service"></a>Jak zabráním přijetí tisíc přístupů z neznámého IP adres, které indikují nějaká napadením se zlými úmysly toohello Cloudová služba?
Azure implementuje síť s více vrstvami zabezpečení tooprotect jeho služby platformy proti útokům (DDoS) distribuované denial of service. Hello Azure DDoS obrany systému je součástí Azure nepřetržité monitorování procesu, který je neustále zlepšit prostřednictvím průnikům testování. Tento systém obrany DDoS slouží toowithstand nejen útoků hello mimo ale taky od ostatních klientů Azure. Další podrobnosti naleznete v [zabezpečení sítě Microsoft Azure](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Můžete také vytvořit blok spuštění úloh tooselectively některé konkrétní IP adresy. Další informace najdete v tématu [blokovat konkrétní IP adresu](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-toordp-toomy-cloud-service-instance-i-get-hello-message-hello-user-account-has-expired"></a>Při instance tooRDP toomy cloudové služby se zobrazuje zpráva hello, "hello uživatelský účet vypršela platnost."
Při vyřazení hello datum vypršení platnosti, který je nakonfigurovaný v nastavení protokolu RDP, může se zobrazit hello chybová zpráva "Tento uživatelský účet vypršela". Datum vypršení platnosti hello hello portálu můžete změnit pomocí následujících kroků:
1. Přihlaste se toohello konzoly pro správu Azure (https://manage.windowsazure.com), přejděte tooyour cloudové služby a vyberte hello **konfigurace** kartě.
2. Vyberte **vzdáleného**.
3. Změňte hello "Vyprší dne" datum a potom uložte konfiguraci hello.

Nyní musí být schopný tooRDP tooyour počítače.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Proč není nástroj pro vyrovnávání zatížení vyrovnávání přenosů stejně?
Informace o tom, jak interní funguje nástroj pro vyrovnávání zatížení najdete v tématu [nový režim distribuce Vyrovnávání zatížení Azure](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Hello distribuční algoritmus používá je 5-řazené kolekce členů (zdrojová adresa IP, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hash toomap provoz tooavailable servery. Poskytuje věrnosti pouze v rámci relace přenosu. Pakety hello stejné relace TCP nebo UDP bude přesměruje toohello instance stejné datacenter IP (DIP) za vyrovnáváním zatížení hello koncový bod. Pokud se klient hello zavře a znovu otevře hello připojení nebo spustí novou relaci z hello stejné zdrojové IP adresy, zdrojového portu hello změny a způsobí, že hello provoz toogo tooa různých DIP koncový bod.
