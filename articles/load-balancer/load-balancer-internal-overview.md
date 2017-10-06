---
title: "Vyrovnávání zatížení aaaInternal přehled | Microsoft Docs"
description: "Přehled pro interní vyrovnávání zátěže a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro scénáře Azure a možné tooconfigure vnitřních koncových bodů"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 9a901aad224d8821c154e130e142699d57282b25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internal-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení interní

Na rozdíl od hello Internet přístupných Vyrovnávání zatížení vyrovnávání hello interní zatížení (ILB) směrovat provoz jenom tooresources uvnitř hello cloudové služby nebo pomocí sítě VPN tooaccess hello infrastruktury Azure. Hello infrastruktury omezuje přístup toohello skupinu s vyrovnáváním zatížení virtuální IP adresy (VIP) Cloudovou službu nebo virtuální sítě tak, aby nikdy bude Internet přímo zveřejněné tooan koncový bod. To umožňuje interní řádku toorun obchodní (LOB) aplikace v Azure a získat přístup z cloudu hello nebo z místní prostředky.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Může být nutná k nástroji pro vyrovnávání zatížení interní

Azure interní načíst vyrovnávání (ILB) poskytuje vyrovnávání zátěže mezi virtuální počítače, které se nacházejí v rámci cloudové služby nebo virtuální síť s místní rozsah. Informace o použití hello a konfigurace virtuální sítě s místní rozsah najdete v tématu [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) v hello Azure blog. Existující virtuální sítě, které jsou nakonfigurované pro skupinu vztahů, nemůžou interní nástroj pro vyrovnávání zatížení používat.

ILB umožňuje hello následující typy služby Vyrovnávání zatížení:

* V rámci cloudové služby, z virtuálních počítačů tooa sadu virtuálních počítačů, které se nacházejí v hello stejné cloudové služby (viz obrázek 1).
* V rámci virtuální sítě z virtuálních počítačů v sadě tooa hello virtuální sítě virtuálních počítačů, které se nacházejí v hello stejné cloudové služby hello virtuální sítě (viz obrázek 2).
* Pro virtuální síť mezi různými místy z místní počítače tooa sada virtuálních počítačů, které se nacházejí v hello stejný cloudový služby hello virtuální sítě (viz obrázek 3).
* Vyrovnávání pro provoz z vrstvy internetového hello zatížení internetového, vícevrstvé aplikace, ve kterých nejsou internetového hello back-end vrstev, ale vyžadují.
* Vyrovnávání zatížení pro obchodní aplikace, které jsou hostované v Azure bez nutnosti další zátěž vyrovnávání hardwaru nebo softwaru. Včetně místních serverů v hello sadu počítačů, jejichž provoz je zatížení vyvážit.

## <a name="internet-facing-multi-tier-applications"></a>Internetový bod vícevrstvé aplikace

Hello webová vrstva obsahuje internetových koncových bodů pro internetové klienty a je součástí skupiny s vyrovnáváním zatížení. Nástroj pro vyrovnávání zatížení Hello distribuuje příchozí provoz z webových klientů pro TCP port 443 (HTTPS) toohello webové servery.

Hello databázové servery jsou za koncovým bodem ILB, který hello webové servery používají pro úložiště. Koncový bod, jaký provoz je vyrovnáváno zatížení napříč servery databáze hello hello ILB sadu s vyrovnáváním zatížení služby této databáze.

Následující obrázek ukazuje Hello hello internetové vícevrstvé aplikace v rámci hello stejné cloudové služby.

![Interní jeden cloud službou Vyrovnávání zatížení](./media/load-balancer-internal-overview/IC736321.png)

Obrázek 1 – internetový bod vícevrstvé aplikace

Další využití pro vícevrstvé aplikace je při hello ILB nasazení tooa jinou cloudovou službu, než jedna služba hello náročné pro hello ILB hello.

Cloud services pomocí hello stejné virtuální síti bude mít přístup k koncovým bodem ILB toohello. Následující obrázek ukazuje front-end webové servery jsou v rámci různých cloudové služby z databáze hello back-end a pomocí Hello hello koncovým bodem ILB v rámci hello stejné virtuální síti.

![Interní vyrovnávání zátěže mezi cloudové služby](./media/load-balancer-internal-overview/IC744147.png)

Obrázek 2 – Front-end servery v rámci různých cloudové služby

## <a name="intranet-line-of-business-applications"></a>Intranetu řádek obchodní aplikace

Přenosy od klientů v místní síti hello získat Vyrovnávání zatížení napříč hello sadu serverů LOB pomocí sítě tooAzure připojení VPN.

Hello klientský počítač bude mít přístup tooan IP adresu ze služby Azure VPN pomocí toosite bodu VPN. To umožňuje použití hello hello obchodní aplikace hostované za koncovým bodem ILB hello.

![Interní rozložení zátěže pomocí toosite bodu VPN](./media/load-balancer-internal-overview/IC744148.png)

Obrázek 3 - obchodních aplikací, které jsou hostovány za hello LB koncový bod

Další možností pro hello LOB je toohave lokality toosite VPN toohello virtuální síti, kde koncovým bodem ILB hello je nakonfigurován. To umožňuje místní síťové přenosy toobe směrovány toohello koncovým bodem ILB.

![Interní rozložení zátěže pomocí site toosite VPN](./media/load-balancer-internal-overview/IC744150.png)

Obrázek 4 – místní síťový provoz směrovat koncovým bodem ILB toohello

## <a name="limitations"></a>Omezení

Interní nástroj pro vyrovnávání zatížení konfigurace nepodporují překládat pomocí SNAT. V kontextu tohoto dokumentu hello odkazuje překládat pomocí SNAT tooport podvržený zdroj překlad síťových adres.  To platí tooscenarios kde virtuálního počítače ve fondu vyrovnávání zatížení musí tooreach hello příslušné interní nástroj pro vyrovnávání zatížení na front-endovou IP adresu. Tento scénář není podporován pro interní nástroj pro vyrovnávání zatížení. Počet selhání připojení se stane, když hello tok je toohello skupinu s vyrovnáváním zatížení virtuálních počítačů, které pocházely hello toku. Je nutné použít styl proxy serveru, nástroje pro vyrovnávání zatížení pro takové scénáře.

## <a name="next-steps"></a>Další kroky

[Podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení Azure](load-balancer-arm.md)

[Začít konfigurovat internetové nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Začněte konfiguraci Vyrovnávání zatížení interní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
