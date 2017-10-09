## <a name="how-toocreate-a-classic-vnet-using-azure-cli"></a>Jak toocreate klasické virtuální sítě pomocí rozhraní příkazového řádku Azure
Můžete použít rozhraní příkazového řádku Azure toomanage hello vašich prostředků Azure z příkazového řádku hello z libovolného počítače se systémem Windows, Linux nebo OSX. toocreate virtuální sítě pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../articles/cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **vytvořit virtuální síť azure sítě** příkaz toocreate virtuální síť a podsíť, jak je uvedeno níže. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Očekávaný výstup:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Název toobe hello virtuální síť vytvořili. V našem scénáři je to *TestVNet*.
   * **-e (nebo--adresní prostor)**. Adresní prostor sítě VNet. Pro náš scénář *192.168.0.0*
   * **-i (nebo - cidr)**. Maska sítě ve formátu CIDR. Pro náš scénář *16*.
   * **-n (nebo--název podsítě**). Název první podsíť hello. V našem scénáři je to *FrontEnd*.
   * **-p (nebo--IP adresu podsítě start)**. Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě. Pro náš scénář *192.168.1.0*.
   * **-r (nebo--podsítě cidr)**. Maska sítě ve formátu CIDR podsítě. Pro náš scénář *24*.
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořen hello virtuální sítě. Pro náš scénář *střed USA*.
3. Spustit hello **sítě azure vnet podsíť vytváření** příkaz toocreate podsíť, jak je uvedeno níže. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Tady je hello očekávaný výstup výše hello příkazu:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (nebo--vnet-name**. Název hello kde hello se vytvoří podsíť virtuální sítě. V našem scénáři je to *TestVNet*.
   * **-n (nebo --name)**. Název nové podsítě hello. Pro náš scénář *back-end*.
   * **-a (nebo --address-prefixes)**. Blok CIDR podsítě. Čtyři našem scénáři *192.168.2.0/24*.
4. Spustit hello **sítě azure vnet show** příkaz tooview hello vlastnosti hello nové sítě vnet, jak je uvedeno níže.
   
            azure network vnet show
   
    Tady je hello očekávaný výstup výše hello příkazu:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

