### <a name="noconnection"></a>toomodify místní síťové brány IP předpon adres – žádné připojení brány

#### <a name="tooadd-additional-address-prefixes"></a>tooadd Další předpony adresy:

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.
2. Přidání hello adresní prostor IP adres v hello *přidat další rozsah adres* pole.
3. Klikněte na tlačítko **Uložit** toosave nastavení.

#### <a name="tooremove-address-prefixes"></a>předpony adres tooremove:

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.
2. Klikněte na tlačítko hello **"..."** na hello řádek obsahující hello předponu chcete tooremove.
3. Klikněte na tlačítko **odebrat**.
4. Klikněte na tlačítko **Uložit** toosave nastavení.

### <a name="withconnection"></a>toomodify předpon brány místní sítě IP adresy - existující připojení brány

Chcete-li mít připojení brány a tooadd nebo odebrat předpony IP adres hello obsažené v bránu místní sítě, je nutné toodo hello následující kroky v pořadí. Způsobí to určitý výpadek připojení VPN. Úpravy předpon adres IP, nepotřebujete toodelete hello VPN gateway. Potřebujete jenom tooremove hello připojení.

#### <a name="1-remove-hello-connection"></a>1. Odeberte připojení hello.

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **připojení**.
2. Klikněte na tlačítko hello **...**  na hello řádek pro každé připojení, potom klikněte na **odstranit**.
3. Klikněte na tlačítko **Uložit** toosave nastavení.

#### <a name="2-modify-hello-address-prefixes"></a>2. Úprava předpon adres hello.

tooadd Další předpony adresy:

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.
2. Přidejte hello adresní prostor IP adres.
3. Klikněte na tlačítko **Uložit** toosave nastavení.

předpony adres tooremove:

1. Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.
2. Klikněte na tlačítko hello **...**  na hello řádek obsahující hello předponu chcete tooremove.
3. Klikněte na tlačítko **odebrat**.
4. Klikněte na tlačítko **Uložit** toosave nastavení.

#### <a name="3-recreate-hello-connection"></a>3. Znovu vytvořte připojení hello.

1. Přejděte toohello brány virtuální sítě pro virtuální síť. (Není hello bránu místní sítě.)
2. Na hello brány virtuální sítě, v hello **nastavení** klikněte na tlačítko **připojení**.
3. Klikněte na tlačítko hello **+ přidat** tooopen hello **přidat připojení** okno.
4. Znovu vytvořte připojení.
5. Klikněte na tlačítko **OK** toocreate hello připojení.
