Každý klientský počítač, který se připojuje tooa Point-to-Site pomocí virtuální síť musí mít nainstalovaný certifikát klienta. Hello klientský certifikát je generována z hello kořenový certifikát a nainstalovaný na každém klientském počítači. Pokud není nainstalovaný platný klientský certifikát a hello klient se pokusí tooconnect toohello virtuální sítě, ověření se nezdaří.

Můžete buď vygenerovat jedinečný certifikát pro každého klienta, nebo můžete použít stejný certifikát pro více klientů hello. Hello využít toogenerating jedinečných klientských certifikátů je hello možnost toorevoke jeden certifikát. Jinak, pokud je více klientů pomocí hello stejný certifikát klienta a potřebujete toorevoke, máte toogenerate a nainstalujte nové certifikáty pro všechny klienty, kteří používají tento certifikát tooauthenticate hello.

Můžete generovat klientské certifikáty pomocí hello následující metody:

- **Podnikový certifikát:**

  - Pokud používáte podnikové certifikační řešení, vygenerování klientského certifikátu s hello běžný název hodnota formát 'name@yourdomain.com', místo formát 'domény název\uživatelské_jméno' hello.
  - Zkontrolujte, zda text hello klientský certifikát je založený na šabloně certifikátu "Uživatelem" hello, která má jako hello první položky v seznamu použití hello, ověření klienta' namísto čipových karet přihlášení atd. Můžete zkontrolovat hello certifikát dvakrát kliknete na soubor hello klientský certifikát a zobrazením **podrobnosti > Rozšířené použití klíče**.

- **Certifikát podepsaný svým držitelem kořenové:** je důležité, postupujte podle hello kroky v jednom z hello P2S certifikát článků níže. Jinak hello klientské certifikáty, které vytvoříte, nebudou kompatibilní s připojení P2S a klienti obdrží chybu při pokusu o tooconnect. Hello kroky v některém z následujících článků hello generovat kompatibilní klientský certifikát: 

  * [Pokyny pro Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): tyto pokyny vyžadují certifikáty toogenerate Windows 10 a prostředí PowerShell. Hello certifikáty, které jsou generovány lze nainstalovat na žádné podporované klientské P2S.
  * [MakeCert pokyny](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): MakeCert použijte, pokud nemáte přístup tooa Windows 10 počítače toouse toogenerate certifikáty. MakeCert zastaralý, ale stále můžete použít certifikáty toogenerate MakeCert. Hello certifikáty, které jsou generovány lze nainstalovat na žádné podporované klientské P2S.

  Při vygenerování klientského certifikátu z certifikát podepsaný svým držitelem kořenové pomocí hello předchozích pokynů, automaticky instalaci na počítači hello, kterou jste použili toogenerate ho. Pokud chcete, aby tooinstall klientský certifikát na jiném počítači klienta, je třeba tooexport jej jako .pfx, společně s hello celý řetěz certifikátů. Tím se vytvoří soubor .pfx, který obsahuje informace hello kořenového certifikátu, který je vyžadován pro ověření klienta toosuccessfully hello. Kroky tooexport certifikát, najdete v části [certifikáty – export certifikátu klienta](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).
