> [!NOTE]
> * Brána sítě VPN Hello veřejná IP adresa se změní při migraci z původní tooa SKU nové SKU.
> * Nelze migrovat classic toohello brány VPN nové SKU. Classic VPN Gateway může používat jenom hello starší verze (starý) SKU.
> 

Nelze změnit velikost Azure VPN Gateway mezi hello staré SKU a hello nové rodiny SKU. Pokud máte brány sítě VPN v modelu nasazení Resource Manager hello, které používají starší verzi hello hello SKU, můžete migrovat toohello nové SKU. toomigrate, odstraňte existující bránu VPN hello vaší virtuální sítě, a vytvořte novou.

Pracovní postup migrace:

1. Odeberte všechny brány virtuální sítě toohello připojení.
2. Odstraňte staré brány VPN hello.
3. Vytvořte novou bránu VPN hello.
4. Aktualizujte vaše místní zařízení VPN s hello novou IP adresu brány VPN (pro připojení Site-to-Site).
5. Aktualizujte hello brány IP adresu hodnotu pro všechny brány místní sítě VNet-to-VNet, která se budou připojovat toothis brány.
6. Stáhněte nové balíčky konfigurace klienta VPN pro P2S klienti připojení virtuální sítě toohello prostřednictvím této brány VPN.
7. Znovu vytvořte bránu virtuální sítě toohello hello připojení.
