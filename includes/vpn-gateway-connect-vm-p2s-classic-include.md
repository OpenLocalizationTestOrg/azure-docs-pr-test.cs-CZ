Můžete se připojit tooa virtuální počítač, který je nasazený tooyour virtuální sítě tak, že vytvoříte tooyour připojení ke vzdálené ploše virtuálního počítače. Hello nejlepší způsob, jak tooinitially ověřte, zda můžete připojit virtuální počítač je tooconnect podle tooyour pomocí jeho privátní IP adresa, místo názvu počítače. Tímto způsobem toosee jsou testování, pokud se můžete připojit, a zda překlad není správně nakonfigurována. 

1. Vyhledejte hello privátní IP adresu pro virtuální počítač. Hello privátní IP adresu virtuálního počítače, můžete vyhledat buď na základě vlastnosti hello hello virtuálního počítače v hello portál Azure nebo pomocí prostředí PowerShell.
2. Ověřte, zda jste tooyour připojené virtuální sítě pomocí hello Point-to-Site VPN připojení. 
3. Otevřete připojení ke vzdálené ploše zadáním "RDP" nebo "Připojení ke vzdálené ploše" hello vyhledávacího pole na panelu hello a potom vyberte připojení ke vzdálené ploše. Můžete také otevřít připojení ke vzdálené ploše pomocí příkazu "mstsc" hello v prostředí PowerShell. 
3. Připojení ke vzdálené ploše zadejte hello privátní IP adresu hello virtuálních počítačů. Klikněte na tlačítko "Zobrazit možnosti" tooadjust další nastavení a potom připojit.

### <a name="tootroubleshoot-an-rdp-connection-tooa-vm"></a>tootroubleshoot tooa připojení RDP virtuálních počítačů

Pokud máte potíže s připojením tooa virtuálního počítače přes připojení k síti VPN, existuje několik věcí, které můžete zkontrolovat. Další informace o odstraňování potíží naleznete v části [tooa připojení vzdálené plochy řešení virtuálních počítačů](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).

- Ověřte, že je úspěšně navázáno připojení VPN.
- Ověřte, zda se připojujete toohello privátní IP adresu pro hello virtuálních počítačů.
- Použijte 'ipconfig' toocheck hello IPv4 adresu přiřazenou toohello adaptér Ethernet na hello počítače, ze kterého se chcete připojit. Pokud hello IP adresa je v rámci rozsah adres hello hello virtuální síť, ke kterému se připojujete, nebo rozsah adres hello vaší VPNClientAddressPool, jedná se označují tooas překrývající se adresní prostor. Když adresní prostor se překrývá tímto způsobem, hello síťový provoz není dosáhnout Azure, zůstane v místní síti hello.
- Pokud připojíte toohello virtuálních počítačů pomocí hello privátní IP adresa, ale není hello název počítače, ověřte, že jste správně nakonfigurovali DNS. Další informace o tom, jak funguje překlad IP adres pro virtuální počítače, najdete v tématu [Překlad IP adres pro virtuální počítače](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
- Ověřte, zda že tento balíček konfigurace klienta VPN hello bylo vygenerováno po IP adresy serverů DNS hello byly zadány pro hello virtuální sítě. Pokud jste aktualizovali IP adresy serverů DNS hello, generovat a nainstalovat nový balíček konfigurace klienta VPN.
