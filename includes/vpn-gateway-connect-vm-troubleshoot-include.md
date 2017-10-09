Pokud máte potíže s připojením tooa virtuálního počítače přes připojení k síti VPN, zkontrolujte následující hello:

- Ověřte, že je úspěšně navázáno připojení VPN.
- Ověřte, zda se připojujete toohello privátní IP adresu pro hello virtuálních počítačů.
- Pokud připojíte toohello virtuálních počítačů pomocí hello privátní IP adresa, ale není hello název počítače, ověřte, že jste správně nakonfigurovali DNS. Další informace o tom, jak funguje překlad IP adres pro virtuální počítače, najdete v tématu [Překlad IP adres pro virtuální počítače](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Jakmile se připojíte přes Point-to-Site, zkontrolujte následující další položky hello:

- Použijte 'ipconfig' toocheck hello IPv4 adresu přiřazenou toohello adaptér Ethernet na hello počítače, ze kterého se chcete připojit. Pokud hello IP adresa je v rámci rozsah adres hello hello virtuální síť, ke kterému se připojujete, nebo rozsah adres hello vaší VPNClientAddressPool, jedná se označují tooas překrývající se adresní prostor. Když adresní prostor se překrývá tímto způsobem, hello síťový provoz není dosáhnout Azure, zůstane v místní síti hello.
- Ověřte, zda že tento balíček konfigurace klienta VPN hello bylo vygenerováno po IP adresy serverů DNS hello byly zadány pro hello virtuální sítě. Pokud jste aktualizovali IP adresy serverů DNS hello, generovat a nainstalovat nový balíček konfigurace klienta VPN.

Další informace o odstraňování potíží s připojení ke vzdálené ploše najdete v tématu [tooa připojení vzdálené plochy řešení virtuálních počítačů](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).
