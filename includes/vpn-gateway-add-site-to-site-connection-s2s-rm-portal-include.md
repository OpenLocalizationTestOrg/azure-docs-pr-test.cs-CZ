1. Přejděte tooand hello otevřete okno pro bránu virtuální sítě. Existuje několik způsobů toonavigate. V našem příkladu jsme přešli toohello brány: VNet1GW' přechodem příliš**virtuální sítě TestVNet1 -> Přehled -> připojeno zařízení -> VNet1GW**.
2. V okně hello VNet1GW, klikněte na tlačítko **připojení**. Hello horní části okna hello připojení, klikněte na tlačítko **+ přidat** tooopen hello **přidat připojení** okno.

    ![Vytvoření připojení typu site-to-site](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. Na hello **přidat připojení** okno vyplňte hello hodnoty toocreate připojení.

  - **Název**: Zadejte název připojení. V našem příkladu používáme název **VNet1toSite2**.
  - **Typ připojení**: Vyberte **Site-to-site (IPSec)**.
  - **Brána virtuální sítě:** má pevnou hodnotu hello, protože se připojují z této brány.
  - **Brána místní sítě:** klikněte na tlačítko **zvolit bránu místní sítě** a vyberte hello brány místní sítě, které chcete toouse. V našem příkladu používáme **Site2**.
  - **Sdílený klíč:** zde hello hodnota musí odpovídat hello hodnotu, která používáte pro vaše místní zařízení VPN. V příkladu hello jsme použili 'abc123', ale můžete (a měli) použít něco složitější. Důležité: co je tuto hodnotu hello, který zde určíte Hello musí být hello stejnou hodnotu, kterou jste zadali při konfiguraci zařízení VPN.
  - Hello zbývající hodnoty pro **předplatné**, **skupiny prostředků**, a **umístění** jsou pevné.

4. Klikněte na tlačítko **OK** toocreate připojení. Uvidíte *vytváření připojení* flash na úvodní obrazovka.
5. Hello připojení si můžete prohlédnout v hello **připojení** okno hello brány virtuální sítě. Hello stav přejde z *neznámé* příliš*připojení*a potom příliš*úspěšné*.
