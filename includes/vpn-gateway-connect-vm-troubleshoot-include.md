Pokud se vám nedaří připojit k virtuálnímu počítači přes připojení VPN, zkontrolujte následující:

- Ověřte, že je úspěšně navázáno připojení VPN.
- Ověřte, že se připojujete k privátní IP adrese virtuálního počítače.
- Pokud se k virtuálnímu počítači můžete připojit s použitím privátní IP adresy, ale ne pomocí názvu počítače, ověřte, že jste správně nakonfigurovali DNS. Další informace o tom, jak funguje překlad IP adres pro virtuální počítače, najdete v tématu [Překlad IP adres pro virtuální počítače](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Pokud se připojujete přes Point-to-Site, zkontrolujte navíc následující položky:

- Pomocí příkazu ipconfig zkontrolujte IPv4 adresu přiřazenou adaptéru Ethernet na počítači, ze kterého se připojujete. Pokud je IP adresa v rámci rozsahu adres virtuální sítě, ke které se připojujete, nebo v rámci rozsahu adres VPNClientAddressPool, tato situace se označuje jako překrývající se adresní prostor. Když se adresní prostor tímto způsobem překrývá, síťový provoz nemá přístup do Azure a zůstane v místní síti.
- Ověřte, že se po zadání IP adres serveru DNS pro virtuální síť vygeneroval balíček pro konfiguraci klienta VPN. Pokud jste aktualizovali IP adresy serveru DNS, vygenerujte a nainstalujte nový balíček pro konfiguraci klienta VPN.

Další informace o řešení potíží s připojením ke vzdálené ploše najdete v tématu [Řešení potíží s připojením ke vzdálené ploše virtuálního počítače](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).