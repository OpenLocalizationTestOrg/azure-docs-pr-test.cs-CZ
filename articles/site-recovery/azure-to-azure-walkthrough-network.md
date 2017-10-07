---
title: "aaaPlan sítě pro replikaci tooAzure VMware službou Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě požadované při replikaci virtuálních počítačů Azure s Azure Site Recovery"
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
ms.date: 08/01/2017
ms.author: sujayt
ms.openlocfilehash: e4036351ca707bd4966cf2a855d4a162f88153e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>Krok 3: Plánování sítě pro replikaci virtuálního počítače Azure

Jakmile ověříte hello [požadavky nasazení](azure-to-azure-walkthrough-prerequisites.md), přečtěte si tento článek toounderstand hello sítě aspekty při replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti Azure tooanother pomocí hello Služba Azure Site Recovery. 

- Po dokončení hello článku byste měli mít clear povědomí o tom, jak tooset až odchozí přístup pro virtuální počítače Azure můžete má tooreplicate, a jak tooconnect z místní lokality toothose virtuálních počítačů.
- Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
> Azure replikace virtuálního počítače pomocí Site Recovery je aktuálně ve verzi preview.



## <a name="network-overview"></a>Přehled sítě

Virtuální počítače Azure se obvykle nacházejí v Azure virtuální síť nebo podsíť a vytvoření připojení z vaší místní lokality tooAzure pomocí Azure ExpressRoute nebo připojení k síti VPN.

Sítě jsou obvykle chráněné pomocí brány firewall nebo skupin zabezpečení (Nsg) sítě. Brány firewall můžete použít na základě adresy URL v seznamech založenou na protokolu IP, toocontrol síťové připojení. Pravidla na základě rozsahu IP použít skupiny Nsg. 


Pro toowork Site Recovery očekává budete potřebovat toomake některé změny v odchozí síťové připojení z virtuálních počítačů, které chcete tooreplicate. Site Recovery nepodporuje použití k ověřování proxy toocontrol síťové připojení. Pokud máte proxy služby ověřování, nelze povolit replikaci. 


Hello následující diagram znázorňuje typické prostředí pro aplikace běžící na virtuálních počítačích Azure.

![prostředí zákazníka](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Prostředí virtuálních počítačů Azure**

Připojení tooAzure, nastavení z vaší místní lokality, může mít také pomocí Azure ExpressRoute nebo připojení k síti VPN. 

![prostředí zákazníka](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Místní připojení tooAzure**



## <a name="outbound-connectivity-for-urls"></a>Odchozí připojení pro adresy URL

Pokud používáte odchozí připojení brány firewall založená na adresu URL proxy serveru toocontrol, ujistěte se, že povolíte, aby tyto adresy URL používá Site Recovery

**ADRESA URL** | **Podrobnosti**  
--- | ---
*.blob.core.windows.net | Umožňuje toobe data zapsána z hello virtuální počítač, účet úložiště mezipaměti toohello v oblasti zdroj hello.
Login.microsoftonline.com | Poskytuje tooSite autorizaci a ověřování adresy URL služby obnovení.
*.hypervrecoverymanager.windowsazure.com | Umožňuje komunikaci s hello služba Site Recovery z hello virtuálních počítačů.
*. servicebus.windows.net | Vyžaduje, aby data monitorování a Diagnostika hello Site Recovery je možné zapsat z hello virtuálních počítačů.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Odchozí připojení pro rozsahy IP adres

- Na hello NSG můžete vytvořit pravidla hello požadované automaticky stahuje a spuštěním [tento skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Doporučujeme vytvořit pravidla NSG hello vyžaduje na testovací skupina NSG a ověřte, že neexistují žádné problémy, než vytvoříte pravidla hello u produkčních NSG.
- toocreate hello vyžaduje počet pravidel NSG, zajistěte, aby vaše předplatné seznam povolených adres. Limit pravidlo obraťte se na podporu tooincrease hello NSG v rámci vašeho předplatného.
Pokud používáte všechny proxy serveru na základě IP brány firewall nebo NSG pravidla toocontrol odchozí připojení, hello následující rozsahy IP adres třeba toobe povolené, v závislosti na hello zdrojové a cílové umístění hello virtuálních počítačů:

    - Všechny rozsahy IP adres, které odpovídají toohello umístění zdroje. Stažení hello [rozsahy](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Vytvoření seznamu povolených je nutné, aby data je možné zapsat účet úložiště mezipaměti toohello z hello virtuálních počítačů.
    - Všechny rozsahy IP, které odpovídají tooOffice 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity). Pokud rozsahy tooOffice 365 IP adres se přidají nové IP adresy, je třeba toocreate nová pravidla NSG.
-     Lokality obnovení služby koncový bod IP adresy ([k dispozici v souboru XML](https://aka.ms/site-recovery-public-ips)), které závisí na cílové umístění: 

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

## <a name="example-nsg-configuration"></a>Příklad konfigurace NSG

Tato část uvádí, jak pravidla tooconfigure NSG, tak, aby replikace funguje pro virtuální počítač. Pokud používáte odchozí připojení pro toocontrol pravidla NSG, použijte "HTTPS povolit odchozí" pravidla pro všechny požadované hello rozsahy IP adres.

V tomto příkladu hello umístění zdrojového virtuálního počítače je "Východní USA". Hello replikace cílové umístění je střed USA.


### <a name="example"></a>Příklad

#### <a name="east-us"></a>Východ USA

1. Vytvoření pravidla, která odpovídají příliš[rozsahy IP USA – východ](https://www.microsoft.com/download/confirmation.aspx?id=41653). To je potřeba, aby data je možné zapsat účet úložiště mezipaměti toohello z hello virtuálních počítačů.
2. Vytvoření pravidel pro všechny rozsahy IP, které odpovídají tooOffice 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Vytvoření pravidla, která odpovídají toohello cílové umístění:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Střed USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>Střed USA

Tato pravidla jsou vyžadována, takže je možné povolit replikaci z hello cílová oblast toohello zdroj oblast, po převzetí služeb při selhání.

1. Vytvoření pravidla, která odpovídají příliš[rozsahy IP centrální USA](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Vytvoření pravidel pro všechny rozsahy IP, které odpovídají tooOffice 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Vytvoření pravidla, která odpovídají toohello zdrojové umístění:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Východ USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Existující místní připojení

Pokud máte ExpressRoute nebo VPN připojení mezi místními servery a umístění zdroje hello v Azure, postupujte podle těchto pokynů:

**Konfigurace** | **Podrobnosti**
--- | ---
**Vynuceného tunelování** | Výchozí trasa (0.0.0.0/0) obvykle vynutí odchozí provoz tooflow internet prostřednictvím hello místní umístění. Nedoporučujeme to. Provoz replikace a komunikaci Site Recovery může zůstat v rámci Azure.<br/><br/> Jako řešení, přidejte trasy definované uživatelem (udr) pro [tyto rozsahy IP](#outbound-connectivity-for-azure-site-recovery-ip-ranges)tak, aby provoz hello nelze přejít na místě.
**Připojení** | Pokud aplikace potřebují tooconnect tooon místní počítače nebo klienty na místě toohello aplikaci připojit přes místní přes VPN nebo ExpressRoute, ujistěte se, máte alespoň [připojení site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), mezi hello cílové oblasti Azure a Hello místního webu.<br/><br/> Pokud jsou svazky provoz mezi hello cílové oblasti Azure a místními servery hello vysoké, vytvořte další [připojení ExpressRoute](../expressroute/expressroute-introduction.md), mezi hello cílová oblast a místně.<br/><br/> Pokud chcete tooretain IP adresy pro virtuální počítače po převzetí služeb při selhání, zachovat připojení site na lokality nebo ExpressRoute hello cílová oblast v odpojeném stavu. To zajišťuje, neexistují žádné střetům rozsahu mezi hello zdrojové a cílové rozsahy IP adres.
**ExpressRoute** | Vytvoření okruhu ExpressRoute hello zdrojové a cílové oblasti.<br/><br/> Umožňuje vytvořte připojení mezi hello zdrojové síti a hello okruh ExpressRoute a mezi hello Cílová síť a hello okruh.<br/><br/> Doporučujeme použít jiný rozsahy IP adres v zdrojové a cílové oblasti. Hello okruh nebude možné tooconnect toomore než jeden Azure sítě s hello stejné rozsazích IP adres, na hello stejnou dobu.<br/><br/> Virtuální sítě můžete vytvořit pomocí hello stejnou IP Adresou rozsahy v obou oblastech a pak vytvořit okruhy ExpressRoute v obou oblastí. Pro převzetí služeb při selhání odpojte hello okruh z virtuální sítě hello zdroje a připojte hello okruh ve virtuální síti hello cíl.<br/><br/> Pokud primární oblasti hello se úplně dolů, hello odpojení operace může selhat. V takovém případě hello cílové virtuální síti nebude získat připojení ExpressRoute.
**ExpressRoute Premium** | Můžete vytvořit okruhy hello stejné geopolitické oblasti.<br/><br/> toocreate okruhů v různých geopolitických oblastí, potřebujete Azure ExpressRoute Premium.<br/><br/> Premium je přírůstkové hello náklady. Pokud jste ho už používáte, není bez dalších nákladů.<br/><br/> Další informace o [umístění](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) a [ceny](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 4: vytvoření trezoru](azure-to-azure-walkthrough-vault.md)

