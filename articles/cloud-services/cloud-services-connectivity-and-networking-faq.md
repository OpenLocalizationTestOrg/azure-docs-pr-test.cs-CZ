---
title: "Problémy s připojením a sítě pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy o připojení a sítě pro Microsoft Azure Cloud Services."
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
ms.openlocfilehash: 55d5b692930e273af29c3de9d2b96b6d012b3415
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy s připojením a sítě pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy týkající se problémů s připojením a sítě pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres
Zkontrolujte, jestli je zapnutá instanci virtuálního počítače, který se pokoušíte rezervovat IP adresu pro. Poté se ujistěte, že používáte vyhrazené IP adresy pro pracovní a provozní nasazení. **Nechcete** změnit nastavení při nasazení je upgradu.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Jak mohu vzdálené plochy Pokud skupinu NSG?
Přidat pravidla k této skupině, která povolí komunikaci na portech **3389** a **20000**.  Vzdálená plocha používá port **3389**.  Instance cloudové služby jsou Vyrovnávané, takže nemůže přímo řídit kterou instanci pro připojení k.  *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a umožňují klientu odesílat soubor cookie s RDP a zadejte jednotlivé instance pro připojení k.  *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000** otevřít, což může být zablokován, pokud máte skupinu NSG.

## <a name="can-i-ping-a-cloud-service"></a>Můžete příkaz ping Cloudová služba?

Ne, nikoli pomocí normální "ping" / protokolu ICMP. Protokol ICMP, nemá oprávnění prostřednictvím nástroje pro vyrovnávání zatížení Azure.

K testování připojení, doporučujeme, abyste provedli port ping. Zatímco Ping.exe používá ICMP, jiné nástroje, jako je například Pspingu, Nmap a telnet umožňují test připojení na určitém portu TCP.

Další informace najdete v tématu [používat příkazy ping portu místo ICMP k testování připojení virtuálního počítače Azure](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Jak zabráním přijetí tisíc přístupů z neznámého IP adres, které indikují nějaká napadením se zlými úmysly do cloudové služby?
Azure implementuje zabezpečení s více vrstvami sítě k ochraně jeho služby platformy proti útokům (DDoS) distribuované denial of service. V systému Azure DDoS obrany je součástí Azure nepřetržité monitorování procesu, který je neustále zlepšit prostřednictvím průnikům testování. Tento systém obrany DDoS je určena pro odolat nejen útokům zvenku, ale taky od ostatních klientů Azure. Další podrobnosti naleznete v [zabezpečení sítě Microsoft Azure](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Můžete také vytvořit úlohu spuštění pro selektivní blokování některé konkrétní IP adresy. Další informace najdete v tématu [blokovat konkrétní IP adresu](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Při pokusu protokolu RDP pro instance Moje cloudové služby se zobrazí zpráva, "vypršela platnost uživatelského účtu."
Při vyřazení datum vypršení platnosti, který je nakonfigurovaný v nastavení protokolu RDP, může se zobrazit chybová zpráva "Tento uživatelský účet vypršela". Datum vypršení platnosti z portálu můžete změnit pomocí následujících kroků:
1. Přihlaste se ke konzole pro správu Azure (https://manage.windowsazure.com), přejděte do cloudové služby a vyberte **konfigurace** kartě.
2. Vyberte **vzdáleného**.
3. Změňte na datum "Vyprší dne" a pak konfiguraci uložte.

Nyní nyní byste měli mít pro připojení RDP k vašemu počítači.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Proč není nástroj pro vyrovnávání zatížení vyrovnávání přenosů stejně?
Informace o tom, jak interní funguje nástroj pro vyrovnávání zatížení najdete v tématu [nový režim distribuce Vyrovnávání zatížení Azure](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Jako distribuční algoritmus používá je 5-n-tice (zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hash k mapování provoz na dostupné servery. Poskytuje věrnosti pouze v rámci relace přenosu. Pakety ve stejné relaci TCP nebo UDP se přesměruje na stejnou instanci datacenter IP (DIP) za vyrovnáváním zatížení koncového bodu. Když klient zavře a znovu otevře připojení nebo spustí novou relaci ze stejné zdrojové IP adresy, zdrojového portu změny a způsobí, že provoz přejít k jinému koncovému bodu vyhrazené IP adresy.
