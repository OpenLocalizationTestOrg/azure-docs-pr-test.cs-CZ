---
title: "aaaAzure Site Recovery sítě pokyny pro replikaci virtuálních počítačů z Azure tooAzure | Microsoft Docs"
description: "Sítě pokyny pro replikaci virtuálních počítačů Azure"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/13/2017
ms.author: sujayt
ms.openlocfilehash: 3a3391b8c3512932d243458fd17d2a2b39248448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Sítě pokyny pro replikaci virtuálních počítačů Azure

>[!NOTE]
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.

Tento článek hello podrobnosti sítě pokyny pro Azure Site Recovery, kdy jste replikaci a obnovení virtuálních počítačů Azure z jedné oblasti tooanother oblast. Další informace o požadavcích na Azure Site Recovery najdete v tématu hello [požadavky](site-recovery-prereq.md) článku.

## <a name="site-recovery-architecture"></a>Architektura obnovení lokality

Site Recovery poskytuje jednoduché a snadný způsob tooreplicate aplikací běžících na virtuálních počítačích Azure tooanother oblast Azure, aby se může být obnoven, pokud je narušení v primární oblasti hello. Další informace o [tento scénář a architektuře Site Recovery](site-recovery-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Infrastruktura vaší sítě

Hello následující diagram znázorňuje hello typické prostředí Azure pro aplikace běžící na virtuálních počítačích Azure:

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Pokud používáte Azure ExpressRoute nebo VPN připojení místní síti tooAzure, hello prostředí vypadá takto:

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Obvykle se zákazníci chránit jejich sítím pomocí brány firewall nebo skupiny zabezpečení sítě (Nsg). Hello brány firewall můžete použít buď na základě adresy URL nebo na základě IP povolených pro řízení připojení k síti. Skupiny Nsg povolí pravidla pro používání IP rozsahy toocontrol síťové připojení.

>[!IMPORTANT]
> Pokud používáte připojení k síti toocontrol ověřeného proxy, není podporována a Site Recovery replikaci nejde povolit. 

Hello následující části popisují hello sítě odchozí připojení změny, které jsou požadovány z virtuálních počítačů Azure pro replikaci toowork Site Recovery.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Odchozí připojení pro adresy URL Azure Site Recovery

Pokud používáte odchozí připojení žádné brány firewall založená na adresu URL proxy serveru toocontrol, že toowhitelist se následující požadované adresy URL služby Azure Site Recovery:


**ADRESA URL** | **Účel**  
--- | ---
*.blob.core.windows.net | Vyžaduje, aby data lze zapsat účet úložiště mezipaměti toohello v oblasti zdroj hello z hello virtuálních počítačů.
Login.microsoftonline.com | Vyžaduje se pro autorizaci a ověřování toohello Site Recovery adresy URL služby.
*.hypervrecoverymanager.windowsazure.com | Vyžaduje, aby komunikace služby Site Recovery hello může dojít z hello virtuálních počítačů.
*. servicebus.windows.net | Vyžaduje, aby data monitorování a Diagnostika hello Site Recovery je možné zapsat z hello virtuálních počítačů.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Odchozí připojení pro rozsahy Azure Site Recovery IP

>[!NOTE]
> tooautomatically vytvořit hello požadované pravidla NSG na skupinu zabezpečení sítě hello, můžete [stáhnout a použít tento skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Doporučujeme vytvořit pravidla NSG hello vyžaduje na testovací skupiny zabezpečení sítě a ověřte, že předtím, než vytvoříte pravidla hello na skupinu zabezpečení sítě produkční neexistují žádné problémy.
> * toocreate hello vyžaduje počet pravidel NSG, zajistěte, aby vaše předplatné seznam povolených adres. Limit pravidlo obraťte se na podporu tooincrease hello NSG v rámci vašeho předplatného.

Pokud používáte všechny proxy serveru na základě IP brány firewall nebo NSG pravidla toocontrol odchozí připojení, hello následující rozsahy IP adres třeba toobe povolené, v závislosti na hello zdrojové a cílové umístění hello virtuálních počítačů:

- Všechny rozsahy IP, které odpovídají toohello umístění zdroje. (Můžete stáhnout hello [rozsahy IP adres](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Vytvoření seznamu povolených je potřeba, data lze zapsat účet úložiště mezipaměti toohello z hello virtuálních počítačů.

- Všechny rozsahy IP, které odpovídají tooOffice 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Pokud nové IP adres se přidají tooOffice 365 rozsahy IP v hello budoucí, je třeba toocreate nová pravidla NSG.
    
- Lokality koncový bod obnovení služby IP adresy ([k dispozici v souboru XML](https://aka.ms/site-recovery-public-ips)), které závisí na cílové umístění: 

   **Cílové umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
   --- | --- | ---
   Východní Asie | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Jihovýchodní Asie | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Střed Indie | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Indie – jih | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   Střed USA – sever | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Severní Evropa | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Západní Evropa | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   Východ USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   Západní USA | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   Střed USA – jih | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   Střed USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   Východní USA 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Japonsko – východ | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Japonsko – západ | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Brazílie – jih | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Austrálie – východ | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Austrálie – jihovýchod | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Střední Kanada | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Východní Kanada | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   Západní střed USA | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   Západní USA 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Spojené království – západ | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Spojené království – jih | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="sample-nsg-configuration"></a>Ukázková konfigurace NSG
Tato část popisuje pravidla NSG tooconfigure hello kroky, aby mohl pracovat replikace Site Recovery na virtuálním počítači. Pokud používáte odchozí připojení pro toocontrol pravidla NSG, použijte "HTTPS povolit odchozí" pravidla pro všechny požadované hello rozsahy IP adres.

>[!Note]
> tooautomatically vytvořit hello požadované pravidla NSG na skupinu zabezpečení sítě hello, můžete [stáhnout a použít tento skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Například pokud zdrojové umístění Virtuálního počítače je "East US" a cílové umístění vaší replikace je "Střední USA", postupujte podle kroků hello v následujících dvou částech hello.

>[!IMPORTANT]
> * Doporučujeme vytvořit pravidla NSG hello vyžaduje na testovací skupiny zabezpečení sítě a ověřte, že předtím, než vytvoříte pravidla hello na skupinu zabezpečení sítě produkční neexistují žádné problémy.
> * toocreate hello vyžaduje počet pravidel NSG, zajistěte, aby vaše předplatné seznam povolených adres. Limit pravidlo obraťte se na podporu tooincrease hello NSG v rámci vašeho předplatného. 

### <a name="nsg-rules-on-hello-east-us-network-security-group"></a>Pravidla NSG na skupinu zabezpečení sítě hello východní USA

* Vytvoření pravidla, která odpovídají příliš[rozsahy IP USA – východ](https://www.microsoft.com/download/confirmation.aspx?id=41653). To je potřeba, aby data je možné zapsat účet úložiště mezipaměti toohello z hello virtuálních počítačů.

* Vytvoření pravidel pro všechny rozsahy IP, které odpovídají tooOffice 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Vytvoření pravidla, která odpovídají toohello cílové umístění:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Střed USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

### <a name="nsg-rules-on-hello-central-us-network-security-group"></a>Pravidla NSG na skupinu zabezpečení sítě hello střed USA

Tato pravidla jsou vyžadována, takže je možné povolit replikaci z hello cílové oblasti toohello zdroj oblast post-převzetí služeb při selhání:

* Pravidla, která odpovídají příliš[rozsahy IP centrální USA](https://www.microsoft.com/download/confirmation.aspx?id=41653). Toto jsou požadované, tak, aby data je možné zapsat účet úložiště mezipaměti toohello z hello virtuálních počítačů.

* Pravidla pro všechny rozsahy IP, které odpovídají tooOffice 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Pravidla, která odpovídají toohello zdrojové umístění:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Východ USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Pokyny pro existující konfiguraci Azure na místní ExpressRoute nebo VPN

Pokud máte ExpressRoute nebo VPN připojení mezi místními a hello zdrojové umístění v Azure, postupujte podle pokynů hello v této části.

### <a name="forced-tunneling-configuration"></a>Vynutit tunelové konfigurace

Obvyklé konfigurace zákazníka je toodefine výchozí trasa (0.0.0.0/0), který vynutí odchozí provoz tooflow Internet prostřednictvím hello místní umístění. To nedoporučujeme. provoz replikace Hello a komunikace služby Site Recovery neměli ponechat hello Azure hranic. Hello řešení je tooadd trasy definované uživatelem (udr) pro [tyto rozsahy IP](#outbound-connectivity-for-azure-site-recovery-ip-ranges) tak, aby provoz replikace hello nelze přejít na místě.

### <a name="connectivity-between-hello-target-and-on-premises-location"></a>Připojení mezi hello cíle a místní umístění

Postupujte podle následujících pokynů pro připojení mezi hello cílové umístění a hello místní umístění:
- Pokud aplikace potřebuje tooconnect toohello místní počítače, nebo pokud jsou klienti, kteří připojují přes VPN nebo ExpressRoute toohello aplikace z místního, zajistěte, abyste měli alespoň [připojení site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mezi vaším datovým centrem cíl Azure oblast a hello místně.

- Pokud očekáváte velké množství tooflow provoz mezi cílové oblasti Azure a hello místního datového centra, měli byste vytvořit další [připojení ExpressRoute](../expressroute/expressroute-introduction.md) mezi hello cíl Azure oblast a hello místní datacentra.

- Pokud chcete tooretain IP adresy pro virtuální počítače hello po jejich převzetí služeb při selhání, zachovat připojení site na lokality nebo ExpressRoute hello cílová oblast v odpojeném stavu. Toto je toomake se není žádná rozsah kolidovat mezi rozsahů IP hello zdroj oblasti a cílová oblast rozsahy IP adres.

### <a name="best-practices-for-expressroute-configuration"></a>Osvědčené postupy pro konfiguraci ExpressRoute
Postupujte podle těchto osvědčené postupy pro konfiguraci ExpressRoute:

- Je nutné toocreate okruh ExpressRoute v obou hello zdrojové a cílové oblasti. Pak musíte toocreate připojení mezi:
  - Hello zdrojové virtuální sítě a hello okruh ExpressRoute.
  - Hello cílový virtuální sítě a hello okruh ExpressRoute.

- Jako součást standardní ExpressRoute, můžete vytvořit okruhy hello stejné geopolitické oblasti. okruhy ExpressRoute toocreate v různých geopolitických oblastí, Azure ExpressRoute Premium je požadováno, což zahrnuje přírůstkové náklady. (Pokud už používáte ExpressRoute Premium, že nejsou zpoplatněné.) Další podrobnosti najdete v tématu hello [dokumentu umístění ExpressRoute](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) a [ExpressRoute ceny](https://azure.microsoft.com/pricing/details/expressroute/).

- Doporučujeme použít jiný rozsahy IP adres v zdrojové a cílové oblasti. Hello okruh ExpressRoute, nebude možné tooconnect s dvou virtuálních sítí Azure z hello stejné rozsahy IP adres v hello stejnou dobu.

- Virtuální sítě můžete vytvořit pomocí hello stejnou IP Adresou rozsahy v obou oblastech a pak vytvořte okruhy ExpressRoute v obou oblastí. V případě hello události převzetí služeb při selhání odpojte hello okruh z virtuální sítě hello zdroje a připojte hello okruh ve virtuální síti hello cíl.

 >[!IMPORTANT]
 > Pokud primární oblasti hello se úplně dolů, hello odpojení operace může selhat. Získání připojení ExpressRoute, nebude možné hello cílové virtuální síti.

## <a name="next-steps"></a>Další kroky
Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](site-recovery-azure-to-azure.md).
