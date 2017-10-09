### <a name="gwipnoconnection"></a>toomodify hello místní síťové brány IP adresu – žádné připojení brány

Použijte hello příklad toomodify bránu místní sítě, která nemá připojení brány. Při úpravě tuto hodnotu, můžete také upravit předpony adres hello v hello stejnou dobu.

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.
2. V hello **IP adresu** pole, je třeba změnit hello IP adresu.
3. Klikněte na tlačítko **Uložit** toosave hello nastavení.

### <a name="gwipwithconnection"></a>toomodify hello místní síťové brány IP adresu brány - existující připojení brány

toomodify bránu místní sítě, který má připojení, musíte toofirst odebrat hello připojení. Po odebrání hello připojení můžete upravit IP adresu brány hello a znovu vytvořte nové připojení. Můžete také upravit předpony adres hello v hello stejnou dobu. Způsobí to určitý výpadek připojení VPN. Při úpravě IP adresu brány hello, nepotřebujete toodelete hello VPN gateway. Potřebujete jenom tooremove hello připojení.
 
#### <a name="1-remove-hello-connection"></a>1. Odeberte připojení hello.

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **připojení**.
2. Klikněte na tlačítko hello **...**  na hello řádek pro připojení hello klikněte **odstranit**.
3. Klikněte na tlačítko **Uložit** toosave nastavení.

#### <a name="2-modify-hello-ip-address"></a>2. Upravte hello IP adresu.

Můžete také upravit předpony adres hello v hello stejnou dobu.

1. V hello **IP adresu** pole, je třeba změnit hello IP adresu.
2. Klikněte na tlačítko **Uložit** toosave hello nastavení.

#### <a name="3-recreate-hello-connection"></a>3. Znovu vytvořte připojení hello.

1. Přejděte toohello brány virtuální sítě pro virtuální síť. (Není hello bránu místní sítě.)
2. Na hello brány virtuální sítě, v hello **nastavení** klikněte na tlačítko **připojení**.
3. Klikněte na tlačítko hello **+ přidat** tooopen hello **přidat připojení** okno.
4. Znovu vytvořte připojení.
5. Klikněte na tlačítko **OK** toocreate hello připojení.
