## <a name="os-config"></a>Přidat IP adresy tooa virtuální počítač operační systém

Připojení a přihlášení tooa virtuálních počítačů, které jste vytvořili pomocí více privátních IP adres. Je třeba ručně přidat všechny hello privátní IP adresy (včetně hello primární) zda jste přidali toohello virtuálních počítačů. Proveďte následující kroky pro operační systém virtuálního počítače hello:

### <a name="windows"></a>Windows

1. Na příkazovém řádku zadejte *ipconfig /all*.  Zobrazí pouze hello *primární* privátní IP adresu (prostřednictvím protokolu DHCP).
2. Typ *ncpa.cpl* v hello příkazového řádku tooopen hello **síťová připojení** okno.
3. Otevřete vlastnosti hello odpovídající adaptéru hello: **připojení k místní síti**.
4. Dvakrát klikněte na Protokol IPv4 (Internet Protocol verze 4).
5. Vyberte **hello použít následující IP adresu** a zadejte hello následující hodnoty:

    * **IP adresa**: Zadejte hello *primární* privátní IP adresa
    * **Maska podsítě:** Nastavte podle vaší podsítě. Například pokud hello podsíť je/24 podsíť a podsíť hello maska je 255.255.255.0.
    * **Výchozí brána**: hello první IP adresa v podsíti hello. Pokud podsíť 10.0.0.0/24, IP adresa brány hello je 10.0.0.1.
    * Klikněte na tlačítko **hello použijte následující adresy serverů DNS** a zadejte hello následující hodnoty:
        * **Upřednostňovaný server DNS:** Pokud nepoužíváte vlastní server DNS, zadejte 168.63.129.16.  Pokud používáte serveru DNS, zadejte pro váš server hello IP adresu.
    * Klikněte na tlačítko hello **Upřesnit** tlačítko a přidejte další IP adresy. Přidejte všechny hello sekundární soukromé IP adresy uvedené v kroku 8 toohello síťový adaptér s hello stejné podsíti zadána pro hello primární IP adresu.
        >[!WARNING] 
        >Pokud se výše uvedené kroky hello není řídit správně, může dojít ke ztrátě připojení tooyour virtuálních počítačů. Zkontrolujte, zda informace hello zadali v kroku 5 přesné než budete pokračovat.

    * Klikněte na tlačítko **OK** tooclose out hello TCP/IP nastavení a potom **OK** znovu tooclose nastavení adaptéru hello. Vaše připojení RDP je obnoveno.

6. Na příkazovém řádku zadejte *ipconfig /all*. Zobrazí se všechny IP adresy, které jste přidali, a protokol DHCP je vypnutý.


### <a name="validation-windows"></a>Ověření (Windows)

tooensure jsou možné tooconnect toohello, které internet z vaší sekundární konfiguraci IP adresy prostřednictvím hello veřejná IP adresa spojená, po přidání správně pomocí kroků výše, použijte následující příkaz hello:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>Pro sekundární konfigurace IP můžete pouze ping toohello Internetu, pokud konfigurace hello má veřejnou IP adresu s ním spojená. Pro primární konfigurace protokolu IP veřejnou IP adresu se nenachází požadovaná tooping toohello Internetu.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Otevřete okno terminálu.
2. Ujistěte se, že jste hello kořenového uživatele. Pokud si nejste, zadejte následující příkaz hello:

    ```bash
    sudo -i
    ```

3. Aktualizujte konfigurační soubor hello hello síťového rozhraní (za předpokladu, eth0').

    * Zachovat existující položka řádku hello protokolu DHCP. primární adresa IP Hello zůstane nakonfigurované jako dříve.
    * Přidáte konfiguraci pro další statickou IP adresu s hello následující příkazy:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    Měl by se zobrazit soubor .cfg.
4. Soubor otevřete hello. Měli byste vidět hello následující řádky na konci hello hello souboru:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Přidejte následující řádky po hello řádky, které existují v tomto souboru hello:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. Uložte soubor hello pomocí hello následující příkaz:

    ```bash
    :wq
    ```

7. Resetování hello síťové rozhraní s hello následující příkaz:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Spusťte ifdown a ifup v hello stejné řádku při použití vzdáleného připojení.
    >

8. Ověřte, zda text hello IP adresa se přidá toohello síťové rozhraní s hello následující příkaz:

    ```bash
    ip addr list eth0
    ```

    Měli byste vidět hello IP adresy můžete přidat jako součást hello seznamu.

### <a name="linux-redhat-centos-and-others"></a>Linux (Red Hat, CentOS a další)

1. Otevřete okno terminálu.
2. Ujistěte se, že jste hello kořenového uživatele. Pokud si nejste, zadejte následující příkaz hello:

    ```bash
    sudo -i
    ```

3. Zadejte své heslo a postupujte podle zobrazených pokynů. Až se hello kořenového uživatele, přejděte toohello síťové skripty složky s hello následující příkaz:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Seznam hello související s ifcfg soubory pomocí hello následující příkaz:

    ```bash
    ls ifcfg-*
    ```

    Měli byste vidět *ifcfg eth0* jako jeden ze souborů hello.

5. tooadd IP adresu, vytvořte konfigurační soubor pro něj, jak vidíte níže. Pro každou konfiguraci IP adresy se musí vytvořit jeden soubor.

    ```bash
    touch ifcfg-eth0:0
    ```

6. Otevřete hello *ifcfg-eth0:0* soubor s hello následující příkaz:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Přidejte soubor obsahu toohello *eth0:0* v takovém případě s hello následující příkaz. Být jisti tooupdate informace založené na IP adresu.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Uložte soubor hello se hello následující příkaz:

    ```bash
    :wq
    ```

9. Restartujte hello síťové služby a ověřte, že hello změny jsou úspěšné spuštěním hello následující příkazy:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Měli byste vidět hello IP adres jste přidali, *eth0:0*, v seznamu hello vrátila.

### <a name="validation-linux"></a>Ověření (Linux)

tooensure jsou možné tooconnect toohello Internetu z vaší sekundární konfiguraci IP adresy prostřednictvím hello veřejné IP adresy přidružené ji, použijte následující příkaz hello:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>Pro sekundární konfigurace IP můžete pouze ping toohello Internetu, pokud konfigurace hello má veřejnou IP adresu s ním spojená. Pro primární konfigurace protokolu IP veřejnou IP adresu se nenachází požadovaná tooping toohello Internetu.

Pro virtuální počítače s Linuxem, při pokusu o toovalidate odchozí připojení z sekundárního síťového adaptéru může být nutné tooadd odpovídající trasy. Existuje mnoho způsobů toodo to. Další informace najdete v odpovídající dokumentaci k vaší distribuci Linuxu. Následující Hello je jedna metoda tooaccomplish toto:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Být jisti tooreplace:
    - **10.0.0.5** s hello privátní IP adresu, která má veřejnou IP adresou přidružené tooit
    - **10.0.0.1** tooyour výchozí brány
    - **eth2** toohello název vaší sekundární síťové karty
