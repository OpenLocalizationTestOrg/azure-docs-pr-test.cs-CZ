---
title: "zařízení VPN aaaAbout pro Azure připojení mezi různými místy | Microsoft Docs"
description: "Tento článek probírá zařízení VPN a parametry protokolu IPsec pro připojení mezi různými místy pomocí VPN Gateway typu S2S. Odkazy jsou uvedeny pokyny tooconfiguration a ukázky."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: yushwang;cherylmc
ms.openlocfilehash: 8b84afbf93d807342ecd56ab369d5909a13343e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>O zařízeních VPN a o parametrech protokolu IPsec/IKE pro připojení typu Site-to-Site ke službě VPN Gateway

Zařízení VPN je požadovaná tooconfigure připojení Site-to-Site (S2S) VPN mezi různými místy použitím brány VPN. Připojení Site-to-Site lze použít toocreate hybridní řešení, nebo vždy, když chcete zabezpečené připojení mezi místními sítěmi a virtuálních sítí. Tento článek obsahuje seznam ověřených zařízení VPN a seznam parametrů protokolu IPsec/IKE pro brány VPN.

> [!IMPORTANT]
> Pokud máte problémy s připojením mezi místními zařízeními VPN a brány VPN, podívejte se příliš[známé problémy s kompatibilitou zařízení](#known).
>
>

### <a name="items-toonote-when-viewing-hello-tables"></a>Při zobrazení tabulek hello položky toonote:

* Došlo ke změně terminologie pro služby Azure VPN Gateway. Změnily se pouze názvy hello. Nedošlo k žádné změně funkce.
  * Statické směrování = PolicyBased
  * Dynamické směrování = RouteBased
* Pokud není uvedeno jinak, jsou hello stejné, specifikace HighPerformance VPN gateway a brána sítě VPN RouteBased. Například jsou hello ověřit zařízení VPN, které jsou kompatibilní s bránami RouteBased VPN také kompatibilní s hello HighPerformance VPN gateway.

## <a name="devicetable"></a>Ověřená zařízení VPN a průvodci konfigurací zařízení

> [!NOTE]
> Při konfiguraci připojení typu Site-to-Site je pro vaše zařízení VPN vyžadována veřejná IP adresa IPv4.
>

Ve spolupráci s dodavateli zařízení jsme ověřili sadu standardních zařízení VPN. Všechna zařízení hello v řadách zařízení hello v následujícím seznamu hello by měla spolupracovat s bránami VPN. V tématu [o nastavení brány sítě VPN](vpn-gateway-about-vpn-gateway-settings.md#vpntype) toounderstand hello VPN zadejte použití (PolicyBased nebo RouteBased) pro hello chcete tooconfigure řešení VPN Gateway.

toohelp konfigurace zařízení VPN, získáte toohello odkazy, které odpovídají řada tooappropriate zařízení. Hello odkazy tooconfiguration pokyny na základě typu best effort. Pro podporu zařízení VPN kontaktujte výrobce zařízení.

|**Dodavatel**          |**Řada zařízení**     |**Minimální verze operačního systému** |**Pokyny ke konfiguraci PolicyBased** |**Pokyny ke konfiguraci RouteBased** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Není kompatibilní  |[Průvodce konfigurací](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |Směrovače VPN řady AR |2.9.2                  |Připravuje se     |Není kompatibilní  |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall řady F |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Průvodce konfigurací](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Průvodce konfigurací](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall řady X |Barracuda Firewall 6.5 |[Průvodce konfigurací](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Není kompatibilní |
| Brocade            |Vyatta 5400 vRouter   |Virtual Router 6.6R3 GA|[Průvodce konfigurací](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Není kompatibilní |
| Check Point |Security Gateway |R77.30 |[Průvodce konfigurací](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Průvodce konfigurací](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Není kompatibilní |
| Cisco |ASR |PolicyBased: iOS 15.1<br>RouteBased: iOS 15.2 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |PolicyBased: iOS 15.0<br>RouteBased*: iOS 15.1 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Ukázky konfigurace*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 a vyšší |[Průvodce konfigurací](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Není kompatibilní |
| F5 |Řada BIG-IP |12.0 |[Průvodce konfigurací](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Průvodce konfigurací](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.2 |  |[Průvodce konfigurací](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |Řada SEIL |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Průvodce konfigurací](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Není kompatibilní |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Routebased: JunOS 11.4 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |Řada J |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Služba Směrování a vzdálený přístup |Windows Server 2012 |Není kompatibilní |[Ukázky konfigurace](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Gateway |– |[Průvodce konfigurací](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Není kompatibilní |
| Openswan |Openswan |2.6.32 |(Připravuje se) |Není kompatibilní |
| Palo Alto Networks |Všechna zařízení se systémem PAN-OS |PAN-OS<br>PolicyBased: 6.1.5 nebo novější<br>RouteBased: 7.1.4 |[Průvodce konfigurací](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Průvodce konfigurací](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| SonicWall |Řada TZ, řada NSA<br>Řada SuperMassive<br>Řada E-Class NSA |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |[Průvodce konfigurací pro SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Průvodce konfigurací pro SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Průvodce konfigurací pro SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Průvodce konfigurací pro SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| WatchGuard |Všechny |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Průvodce konfigurací](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Průvodce konfigurací](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) Směrovače řady ISR 7200 podporují pouze sítě VPN typu PolicyBased.

## <a name="additionaldevices"></a>Neověřená zařízení VPN

Pokud nevidíte svoje zařízení nevidíte v hello tabulce ověřená zařízení VPN, vaše zařízení stále může pracovat se připojení Site-to-Site. Kvůli další podpoře a pokynům ke konfiguraci se obraťte na výrobce zařízení.

## <a name="editing"></a>Ukázky úpravy konfigurace zařízení

Po stažení ukázka konfigurace zařízení VPN hello zadaný, budete potřebovat tooreplace některé hello hodnoty tooreflect hello nastavení pro vaše prostředí.

### <a name="tooedit-a-sample"></a>tooedit ukázku:

1. Otevřete hello ukázku pomocí poznámkového bloku.
2. Vyhledávání a nahrazení všech <*text*> řetězce s hodnotami hello, které odpovídají tooyour prostředí. Být jisti tooinclude < a >. Když je zadaný název, musí být jedinečné hello název, který jste vybrali. Pokud příkaz nefunguje, obraťte se na dokumentaci výrobce zařízení.

| **Text v ukázce** | **Změňte na** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Zvolený název pro tento objekt. Příklad: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |Zvolený název pro tento objekt. Příklad: myAzureNetwork |
| &lt;RP_AccessList&gt; |Zvolený název pro tento objekt. Příklad: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |Zvolený název pro tento objekt. Příklad: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Zvolený název pro tento objekt. Příklad: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Zadejte rozsah. Příklad: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Zadejte masku podsítě. Příklad: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Zadejte místní rozsah. Příklad: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Zadejte masku místní podsítě. Příklad: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Tato informace o konkrétní tooyour virtuální síť a najdete ji v hello portálu pro správu jako **IP adresa brány**. |
| &lt;SP_PresharedKey&gt; |Tyto informace je konkrétní tooyour virtuální sítě a se nachází v hello portálu pro správu jako správa klíče. |

## <a name="ipsec"></a>Parametry protokolu IPsec/IKE

> [!NOTE]
> I když hello hodnoty uvedené v následující tabulce hello aktuálně jsou podporovány hello VPN Gateway, že je žádný mechanismus pro vás toospecify nebo vybrat konkrétní kombinaci algoritmů nebo parametry z brány sítě VPN hello. Je nutné zadat jakákoli omezení ze zařízení hello místní VPN. Kromě toho musíte uchytit **MSS** na **1350**.
> 
>

V následujících tabulkách hello:

* SA je přidružení zabezpečení.
* IKE fáze 1 se také nazývá „hlavní režim“.
* IKE fáze 2 se také nazývá „rychlý režim“.

### <a name="ike-phase-1-main-mode-parameters"></a>Parametry protokolu IKE fáze 1 (hlavní režim)

| **Vlastnost**          |**PolicyBased**    | **RouteBased**    |
| ---                   | ---               | ---               |
| Verze IKE           |IKEv1              |IKEv2              |
| Skupina Diffie-Hellman  |Skupina 2 (1 024 bitů) |Skupina 2 (1 024 bitů) |
| Metoda ověřování |Předsdílený klíč     |Předsdílený klíč     |
| Algoritmy šifrování a hash |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| Životnost SA           |28 800 sekund     |28 800 sekund     |

### <a name="ike-phase-2-quick-mode-parameters"></a>Parametry protokolu IKE fáze 2 (rychlý režim)

| **Vlastnost**                  |**PolicyBased**| **RouteBased**                              |
| ---                           | ---           | ---                                         |
| Verze IKE                   |IKEv1          |IKEv2                                        |
| Algoritmy šifrování a hash |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[Nabídky RouteBased QM SA](#RouteBasedOffers) |
| Životnost SA (čas)            |3 600 sekund  |27 000 sekund                                |
| Životnost SA (bajty)           |102 400 000 kB | -                                           |
| Metoda Perfect Forward Secrecy (PFS) |Ne             |[Nabídky RouteBased QM SA](#RouteBasedOffers) |
| Detekce mrtvých partnerských zařízení (DPD)     |Nepodporuje se  |Podporuje se                                    |


### <a name ="RouteBasedOffers"></a>Nabídky RouteBased VPN IPsec Security Association (rychlý režim IKE SA)

Hello následující tabulka uvádí nabídky přidružení zabezpečení IPsec (IKE rychlého režimu). Nabídky jsou uvedené hello pořadí podle priority v rámci této nabídky hello předávání nebo přijímání.

#### <a name="azure-gateway-as-initiator"></a>Služba Azure Gateway jako iniciátor

|-  |**Šifrování**|**Ověřování**|**Skupina PFS**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |Žádný         |
| 2 |AES256        |SHA1              |Žádný         |
| 3 |3DES          |SHA1              |Žádný         |
| 4 |AES256        |SHA256            |Žádný         |
| 5 |AES128        |SHA1              |Žádný         |
| 6 |3DES          |SHA256            |Žádný         |

#### <a name="azure-gateway-as-responder"></a>Služba Azure Gateway jako respondér

|-  |**Šifrování**|**Ověřování**|**Skupina PFS**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |Žádný         |
| 2 |AES256        |SHA1              |Žádný         |
| 3 |3DES          |SHA1              |Žádný         |
| 4 |AES256        |SHA256            |Žádný         |
| 5 |AES128        |SHA1              |Žádný         |
| 6 |3DES          |SHA256            |Žádný         |
| 7 |DES           |SHA1              |Žádný         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |Žádný         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* U vysokovýkonných bran VPN a bran VPN typu RouteBased můžete zadat šifrování protokolem IPsec s prázdným ESP. Prázdné šifrování neposkytuje ochranu toodata při přenosu a musí být použit pouze při maximální propustnost a minimální latence je požadovaná. Klienti mohou tuto možnost vyberte toouse ve scénářích komunikaci VNet-to-VNet, nebo když šifrování dochází jinde v řešení hello.
* Pro připojení mezi různými místy prostřednictvím hello Internetu použijte hello výchozí nastavení brány Azure VPN pomocí šifrování a algoritmy hash uvedenými v tabulce hello výše tooensure bezpečnost důležité komunikace.

## <a name="known"></a>Známé problémy s kompatibilitou zařízení

> [!IMPORTANT]
> Tyto jsou hello problémy s kompatibilitou mezi zařízeními VPN třetích stran a brány Azure VPN. Hello tým Azure aktivně ve spolupráci s dodavateli hello tooaddress hello problémy, které jsou zde uvedeny. Jakmile se hello problémy řeší, tato stránka bude aktualizováno hello nejnovější informace. Pravidelně se sem vracejte.
>
>

### <a name="feb-16-2017"></a>16. února 2017

**Zařízení sítě Palo Alto s too7.1.4 předchozí verze** pro Azure VPN založené na trasách: Pokud používáte zařízení VPN z Palo Alto sítí s too7.1.4 předchozí verze PAN-OS a dochází k připojení problémy tooAzure brány sítě VPN založené na směrování, Proveďte hello následující kroky:

1. Zkontrolujte verzi firmwaru hello vašeho zařízení Palo Alto sítě. Pokud vaše verze PAN-OS je starší než 7.1.4, upgradujte too7.1.4.
2. Na zařízení hello Palo Alto Networks, změňte hello SA fáze 2 (nebo přidružení zabezpečení rychlého režimu) životnost too28, 800 sekund (8 hodin) při připojování toohello Azure VPN gateway.
3. Pokud se stále setkáváte problémy s připojením, otevřete žádost o podporu od hello portálu Azure.
