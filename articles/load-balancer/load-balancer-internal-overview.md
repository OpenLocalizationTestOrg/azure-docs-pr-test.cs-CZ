---
title: "Nástroj pro vyrovnávání zatížení pro vnitřní přehled | Microsoft Docs"
description: "Přehled pro interní vyrovnávání zátěže a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro Azure a možných scénářů konfigurace vnitřních koncových bodů"
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
ms.openlocfilehash: d324aaf8ec2c8766d5cf11452158d14c19cba4d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="internal-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení interní

Na rozdíl od internetový bod nástroj pro vyrovnávání zatížení vyrovnávání interní zatížení (ILB) přesměruje přenosy pouze na prostředků v cloudové službě nebo pomocí sítě VPN pro přístup k infrastruktuře Azure. Infrastruktury omezuje přístup ke skupině s vyrovnáváním zatížení virtuální IP adresy (VIP) Cloudovou službu nebo virtuální sítě tak, aby se nikdy se zveřejní přímo k Internetu koncový bod. To umožňuje interní řádek obchodní (LOB) aplikace pro spouštění v Azure a získat přístup z cloudu nebo z místní prostředky.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Může být nutná k nástroji pro vyrovnávání zatížení interní

Azure interní načíst vyrovnávání (ILB) poskytuje vyrovnávání zátěže mezi virtuální počítače, které se nacházejí v rámci cloudové služby nebo virtuální síť s místní rozsah. Informace o používání a konfiguraci virtuální sítě s místní rozsah najdete v tématu [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) blog in the Azure. Existující virtuální sítě, které jsou nakonfigurované pro skupinu vztahů, nemůžou interní nástroj pro vyrovnávání zatížení používat.

ILB umožňuje následující typy Vyrovnávání zatížení:

* V rámci cloudové služby, z virtuálních počítačů na sadu virtuálních počítačů, které se nacházejí v rámci stejné cloudové služby (viz obrázek 1).
* V rámci virtuální sítě z virtuálních počítačů ve virtuální síti na sadu virtuálních počítačů, které se nacházejí v rámci stejné Cloudová služba virtuálního sítě (viz obrázek 2).
* Pro virtuální síť mezi různými místy z sadu virtuálních počítačů, které se nacházejí v rámci stejné cloudové služby virtuálního počítače místní sítě (viz obrázek 3).
* Internetový, vícevrstvé aplikace, ve kterých vrstvy back-end nejsou internetového ale vyžadují Vyrovnávání zatížení pro provoz z internetového vrstvy.
* Vyrovnávání zatížení pro obchodní aplikace, které jsou hostované v Azure bez nutnosti další zátěž vyrovnávání hardwaru nebo softwaru. Včetně místních serverů v sadě počítačů, jejichž provoz zatížení vyvážit.

## <a name="internet-facing-multi-tier-applications"></a>Internetový bod vícevrstvé aplikace

Webová vrstva obsahuje internetových koncových bodů pro internetové klienty a je součástí skupiny s vyrovnáváním zatížení. Nástroje pro vyrovnávání zatížení distribuuje příchozí provoz z webových klientů pro port TCP 443 (HTTPS) na webové servery.

Databázové servery jsou za koncovým bodem ILB, které webové servery používají pro úložiště. Koncový bod, jaký provoz je vyrovnáváno zatížení napříč databázové servery v sadě ILB s vyrovnáváním zatížení služby této databáze.

Následující obrázek znázorňuje internetové vícevrstvé aplikace v rámci stejné cloudové služby.

![Interní jeden cloud službou Vyrovnávání zatížení](./media/load-balancer-internal-overview/IC736321.png)

Obrázek 1 – internetový bod vícevrstvé aplikace

Další využití pro vícevrstvé aplikace je při ILB nasazení v jiné cloudové službě než ten, který využívají službu pro ILB.

Cloudové služby pomocí stejné virtuální síti budou mít přístup ke koncovému bodu ILB. Následující obrázek znázorňuje front-end webové servery jsou v rámci různých cloudové služby z databáze back-end a pomocí koncovým bodem ILB v rámci stejné virtuální síti.

![Interní vyrovnávání zátěže mezi cloudové služby](./media/load-balancer-internal-overview/IC744147.png)

Obrázek 2 – Front-end servery v rámci různých cloudové služby

## <a name="intranet-line-of-business-applications"></a>Intranetu řádek obchodní aplikace

Přenosy od klientů v místní síti získat Vyrovnávání zatížení mezi několik serverů LOB pomocí připojení VPN k síti Azure.

Klientský počítač bude mít přístup k IP adrese ze služby Azure VPN pomocí bodu to-site VPN. Umožňuje použít obchodní aplikace hostované za koncovým bodem ILB.

![Interní zátěže pomocí bodu to-site VPN](./media/load-balancer-internal-overview/IC744148.png)

Obrázek 3 - obchodních aplikací, které jsou hostovány za vyrovnáváním zatížení koncového bodu

Jiné scénáře objektu LOB se se sítěmi VPN do virtuální sítě, kde je koncovým bodem ILB nakonfigurované. To umožňuje místní síťový provoz směrovat na koncovým bodem ILB.

![Interní zátěže pomocí sítě site to site VPN](./media/load-balancer-internal-overview/IC744150.png)

Obrázek 4: místní síťový provoz směrovat na koncovým bodem ILB

## <a name="limitations"></a>Omezení

Interní nástroj pro vyrovnávání zatížení konfigurace nepodporují překládat pomocí SNAT. V kontextu tohoto dokumentu překládat pomocí SNAT odkazuje na portu podvržený zdroj překlad síťových adres.  To platí pro scénáře, kde je potřeba dosáhnout příslušné interní nástroj pro vyrovnávání zatížení na front-endovou IP adresu virtuálního počítače ve fondu vyrovnávání zatížení. Tento scénář není podporován pro interní nástroj pro vyrovnávání zatížení. Počet selhání připojení se stane, když tok je Vyrovnávání zatížení do virtuálního počítače, které pocházely toku. Je nutné použít styl proxy serveru, nástroje pro vyrovnávání zatížení pro takové scénáře.

## <a name="next-steps"></a>Další kroky

[Podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení Azure](load-balancer-arm.md)

[Začít konfigurovat internetové nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Začněte konfiguraci Vyrovnávání zatížení interní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
