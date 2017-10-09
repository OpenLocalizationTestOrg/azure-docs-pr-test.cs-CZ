### <a name="is-custom-ipsecike-policy-supported-on-all-azure-vpn-gateway-skus"></a>Jsou vlastní zásady IPsec/IKE podporovány ve všech skladových jednotkách (SKU) služby Azure VPN Gateway?
Vlastní zásady IPsec/IKE jsou podporovány v branách Azure VPN Gateway úrovně **VpnGw1, VpnGw2, VpnGw3, Standard** a **HighPerformance**. Pro SKU **Basic** NENÍ podporován.

### <a name="how-many-policies-can-i-specify-on-a-connection"></a>Kolik zásad můžu zadat pro jedno připojení?
Pro jedno připojení můžete zadat pouze ***jednu*** kombinaci zásad.

### <a name="can-i-specify-a-partial-policy-on-a-connection-eg-only-ike-algorithms-but-not-ipsec"></a>Můžu pro připojení zadat částečné zásady? (např. pouze algoritmy IKE, ale ne IPsec)
Ne, musíte zadat všechny algoritmy a parametry pro protokol IKE (hlavní režim) i protokol IPsec (rychlý režim). Zadání částečných zásad není povoleno.

### <a name="what-are-hello-algorithms-and-key-strengths-supported-in-hello-custom-policy"></a>Co jsou hello algoritmy a klíče síly podporované ve vlastních zásadách hello?
Následující tabulka Hello uvádí hello podporované kryptografické algoritmy a klíče síly konfigurovat hello zákazníků. Pro každé pole musíte vybrat jednu možnost.

| **IPsec/IKEv2**  | **Možnosti**                                                                   |
| ---              | ---                                                                           |
| Šifrování protokolem IKEv2 | AES256, AES192, AES128, DES3, DES                                             |
| Integrita protokolu IKEv2  | SHA384, SHA256, SHA1, MD5                                                     |
| Skupina DH         | DHGroup24, ECP384, ECP256, DHGroup14 (DHGroup2048), DHGroup2, DHGroup1, Žádná |
| Šifrování protokolem IPsec | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Žádné      |
| Integrita protokolu IPsec  | GCMAES256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                            |
| Skupina PFS        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Žádná                              |
| Doba života přidružení zabezpečení v rychlém režimu   | Sekundy (integer; **min. 300** / výchozí hodnota 27 000 sekund)<br>Kilobajty (integer; **min. 1024** / výchozí hodnota 102 400 000 kilobajtů)           |
| Selektor provozu | UsePolicyBasedTrafficSelectors ($True nebo $False; výchozí hodnota je $False)                 |
|                  |                                                                               |

> [!IMPORTANT]
> 1. DHGroup2048 & PFS2048 jsou hello stejná jako skupina Diffie-Hellman **14** v IKE a PFS protokolu IPsec. Najdete v tématu [skupiny Diffie-Hellman](#DH) pro hello proveďte mapování.
> 2. Pro GCMAES algoritmů, je nutné zadat hello stejnou délku GCMAES algoritmus a klíče pro šifrování pomocí protokolu IPsec a Integrity.
> 3. Životnost přidružení zabezpečení hlavního režimu IKEv2 vyřešen v 28 800 sekund na branách Azure VPN hello
> 4. Doby života přidružení zabezpečení v rychlém režimu jsou volitelné parametry. Pokud nebyla zadána žádná doba života, použijí se výchozí hodnoty 27 000 sekund (7,5 hodiny) a 102 400 000 kilobajtů (102 GB).
> 5. UsePolicyBasedTrafficSelector je možnost parametr hello připojení. Najdete v tématu Nejčastější dotazy týkající se další hello položku pro "UsePolicyBasedTrafficSelectors"

### <a name="does-everything-need-toomatch-between-hello-azure-vpn-gateway-policy-and-my-on-premises-vpn-device-configurations"></a>Potřebuje všechno, co toomatch mezi hello zásada brány Azure VPN a konfigurace zařízení VPN Moje místně?
Místní konfiguraci zařízení VPN musí odpovídat nebo obsahovat hello následující algoritmy a parametry, které jste zadali na hello zásad protokolu IPsec/IKE Azure:

* Algoritmus šifrování protokolem IKE
* Algoritmus integrity protokolu IKE
* Skupina DH
* Algoritmus šifrování protokolem IPsec
* Algoritmus integrity protokolu IPsec
* Skupina PFS
* Selektor provozu (*)

Hello SA životnosti jsou pouze místní specifikace, není nutné toomatch.

Pokud povolíte **UsePolicyBasedTrafficSelectors**, je nutné tooensure zařízení VPN má hello odpovídající provoz selektory definovaný s všechny kombinace předponami vaší místní sítě (brány místní sítě) z hello Virtuální síť Azure předpony, místo any-to-any. Například pokud předponami vaší místní sítě se 10.1.0.0/16 a 10.2.0.0/16 a předponami vaší virtuální sítě jsou 192.168.0.0/16 a 172.16.0.0/16, je třeba toospecify hello následující selektory provozu:
* 10.1.0.0/16 <====> 192.168.0.0/16
* 10.1.0.0/16 <====> 172.16.0.0/16
* 10.2.0.0/16 <====> 192.168.0.0/16
* 10.2.0.0/16 <====> 172.16.0.0/16

Odkazovat příliš[připojení více místně na základě zásad zařízení VPN](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) získáte další informace o tom toouse tuto možnost.

### <a name ="DH"></a>Které skupiny Diffie-Hellman jsou podporovány?
Následující tabulka Hello uvádí skupiny Diffie-Hellman hello podporované pro IKE (DHGroup) a protokolu IPsec (PFSGroup):

| **Skupina Diffie-Hellman**  | **DHGroup**              | **PFSGroup** | **Délka klíče** |
| ---                       | ---                      | ---          | ---            |
| 1                         | DHGroup1                 | PFS1         | 768bitová skupina MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024bitová skupina MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048bitová skupina MODP  |
| 19                        | ECP256                   | ECP256       | 256bitová skupina ECP    |
| 20                        | ECP384                   | ECP284       | 384bitová skupina ECP    |
| 24                        | DHGroup24                | PFS24        | 2048bitová skupina MODP  |
|                           |                          |              |                |

Odkazovat příliš[RFC3526](https://tools.ietf.org/html/rfc3526) a [RFC5114](https://tools.ietf.org/html/rfc5114) další podrobnosti.

### <a name="does-hello-custom-policy-replace-hello-default-ipsecike-policy-sets-for-azure-vpn-gateways"></a>Nahradila hello vlastní zásady protokolu IPsec/IKE hello výchozí zásady sady Azure VPN Gateway?
Ano, po připojení je zadán pro vlastní zásady, bránu Azure VPN použije pouze zásady hello hello připojení, jak jako iniciátor IKE a IKE respondér.

### <a name="if-i-remove-a-custom-ipsecike-policy-does-hello-connection-become-unprotected"></a>Pokud lze odebrat vlastní zásady protokolu IPsec/IKE, hello připojení stanou nechráněná?
Ne, hello připojení bude chránit pomocí protokolu IPsec/IKE. Vlastní zásady hello odebraný z připojení brány Azure VPN hello vrátíte zpět toohello [výchozí seznam protokolu IPsec/IKE návrhy](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) a znovu spusťte hello IKE ověření typu handshake s vaše místní zařízení VPN.

### <a name="would-adding-or-updating-an-ipsecike-policy-disrupt-my-vpn-connection"></a>Přerušilo by přidání nebo aktualizace zásad IPsec/IKE připojení VPN?
Ano, může se stát, malé přerušení (několik sekund) jako hello Azure VPN gateway bude přerušit hello existující připojení a znovu spusťte hello IKE handshake toore – vytvoření tunelu IPsec hello s parametry a hello nových šifrovacích algoritmů. Zkontrolujte, zda že vaše místní zařízení VPN je také nakonfigurovaný se hello odpovídající algoritmy a klíče síly toominimize hello přerušení.

### <a name="can-i-use-different-policies-on-different-connections"></a>Můžou se pro různá připojení použít různé zásady?
Ano. Vlastní zásady se aplikují na jednotlivá připojení. Pro různá připojení můžete vytvořit a použít různé zásady IPsec/IKE. Můžete také tooapply vlastní zásady na podmnožinu připojení. Hello zbývající ty, které budou používat sady zásad protokolu IPsec/IKE Azure výchozí hello.

### <a name="can-i-use-hello-custom-policy-on-vnet-to-vnet-connection-as-well"></a>Můžete použít vlastní zásady hello i připojení VNet-to-VNet?
Ano, vlastní zásady můžete použít pro připojení IPsec mezi různými místy i pro připojení typu VNet-to-VNet.

### <a name="do-i-need-toospecify-hello-same-policy-on-both-vnet-to-vnet-connection-resources"></a>Potřebuji toospecify hello stejné zásady na obou prostředky připojení VNet-to-VNet?
Ano. Tunel typu VNet-to-VNet se skládá ze dvou prostředků připojení v Azure (jeden pro každý směr). Je třeba tooensure obě připojení prostředky mít hello stejné zásady, nebude navázat othereise hello připojení VNet-to-VNet.

### <a name="does-custom-ipsecike-policy-work-on-expressroute-connection"></a>Fungují zásady IPsec/IKE na připojení ExpressRoute?
Ne. Zásady protokolu IPsec/IKE funguje pouze na připojení S2S VPN a připojení VNet-to-VNet přes brány Azure VPN hello.
