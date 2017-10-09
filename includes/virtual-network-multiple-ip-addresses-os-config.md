## <span data-ttu-id="d97e4-101"><a name="os-config"></a>Přidat IP adresy tooa virtuální počítač operační systém</span><span class="sxs-lookup"><span data-stu-id="d97e4-101"><a name="os-config"></a>Add IP addresses tooa VM operating system</span></span>

<span data-ttu-id="d97e4-102">Připojení a přihlášení tooa virtuálních počítačů, které jste vytvořili pomocí více privátních IP adres.</span><span class="sxs-lookup"><span data-stu-id="d97e4-102">Connect and login tooa VM you created with multiple private IP addresses.</span></span> <span data-ttu-id="d97e4-103">Je třeba ručně přidat všechny hello privátní IP adresy (včetně hello primární) zda jste přidali toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d97e4-103">You must manually add all hello private IP addresses (including hello primary) that you added toohello VM.</span></span> <span data-ttu-id="d97e4-104">Proveďte následující kroky pro operační systém virtuálního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="d97e4-104">Complete hello following steps for your VM operating system:</span></span>

### <a name="windows"></a><span data-ttu-id="d97e4-105">Windows</span><span class="sxs-lookup"><span data-stu-id="d97e4-105">Windows</span></span>

1. <span data-ttu-id="d97e4-106">Na příkazovém řádku zadejte *ipconfig /all*.</span><span class="sxs-lookup"><span data-stu-id="d97e4-106">From a command prompt, type *ipconfig /all*.</span></span>  <span data-ttu-id="d97e4-107">Zobrazí pouze hello *primární* privátní IP adresu (prostřednictvím protokolu DHCP).</span><span class="sxs-lookup"><span data-stu-id="d97e4-107">You only see hello *Primary* private IP address (through DHCP).</span></span>
2. <span data-ttu-id="d97e4-108">Typ *ncpa.cpl* v hello příkazového řádku tooopen hello **síťová připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="d97e4-108">Type *ncpa.cpl* in hello command prompt tooopen hello **Network connections** window.</span></span>
3. <span data-ttu-id="d97e4-109">Otevřete vlastnosti hello odpovídající adaptéru hello: **připojení k místní síti**.</span><span class="sxs-lookup"><span data-stu-id="d97e4-109">Open hello properties for hello appropriate adapter: **Local Area Connection**.</span></span>
4. <span data-ttu-id="d97e4-110">Dvakrát klikněte na Protokol IPv4 (Internet Protocol verze 4).</span><span class="sxs-lookup"><span data-stu-id="d97e4-110">Double-click Internet Protocol version 4 (IPv4).</span></span>
5. <span data-ttu-id="d97e4-111">Vyberte **hello použít následující IP adresu** a zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d97e4-111">Select **Use hello following IP address** and enter hello following values:</span></span>

    * <span data-ttu-id="d97e4-112">**IP adresa**: Zadejte hello *primární* privátní IP adresa</span><span class="sxs-lookup"><span data-stu-id="d97e4-112">**IP address**: Enter hello *Primary* private IP address</span></span>
    * <span data-ttu-id="d97e4-113">**Maska podsítě:** Nastavte podle vaší podsítě.</span><span class="sxs-lookup"><span data-stu-id="d97e4-113">**Subnet mask**: Set based on your subnet.</span></span> <span data-ttu-id="d97e4-114">Například pokud hello podsíť je/24 podsíť a podsíť hello maska je 255.255.255.0.</span><span class="sxs-lookup"><span data-stu-id="d97e4-114">For example, if hello subnet is a /24 subnet then hello subnet mask is 255.255.255.0.</span></span>
    * <span data-ttu-id="d97e4-115">**Výchozí brána**: hello první IP adresa v podsíti hello.</span><span class="sxs-lookup"><span data-stu-id="d97e4-115">**Default gateway**: hello first IP address in hello subnet.</span></span> <span data-ttu-id="d97e4-116">Pokud podsíť 10.0.0.0/24, IP adresa brány hello je 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="d97e4-116">If your subnet is 10.0.0.0/24, then hello gateway IP address is 10.0.0.1.</span></span>
    * <span data-ttu-id="d97e4-117">Klikněte na tlačítko **hello použijte následující adresy serverů DNS** a zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d97e4-117">Click **Use hello following DNS server addresses** and enter hello following values:</span></span>
        * <span data-ttu-id="d97e4-118">**Upřednostňovaný server DNS:** Pokud nepoužíváte vlastní server DNS, zadejte 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="d97e4-118">**Preferred DNS server**: If you are not using your own DNS server, enter 168.63.129.16.</span></span>  <span data-ttu-id="d97e4-119">Pokud používáte serveru DNS, zadejte pro váš server hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-119">If you are using your own DNS server, enter hello IP address for your server.</span></span>
    * <span data-ttu-id="d97e4-120">Klikněte na tlačítko hello **Upřesnit** tlačítko a přidejte další IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d97e4-120">Click hello **Advanced** button and add additional IP addresses.</span></span> <span data-ttu-id="d97e4-121">Přidejte všechny hello sekundární soukromé IP adresy uvedené v kroku 8 toohello síťový adaptér s hello stejné podsíti zadána pro hello primární IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-121">Add each of hello secondary private IP addresses listed in step 8 toohello NIC with hello same subnet specified for hello primary IP address.</span></span>
        >[!WARNING] 
        ><span data-ttu-id="d97e4-122">Pokud se výše uvedené kroky hello není řídit správně, může dojít ke ztrátě připojení tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d97e4-122">If you do not follow hello steps above correctly, you may lose connectivity tooyour VM.</span></span> <span data-ttu-id="d97e4-123">Zkontrolujte, zda informace hello zadali v kroku 5 přesné než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d97e4-123">Ensure hello information entered for step 5 is accurate before proceeding.</span></span>

    * <span data-ttu-id="d97e4-124">Klikněte na tlačítko **OK** tooclose out hello TCP/IP nastavení a potom **OK** znovu tooclose nastavení adaptéru hello.</span><span class="sxs-lookup"><span data-stu-id="d97e4-124">Click **OK** tooclose out hello TCP/IP settings and then **OK** again tooclose hello adapter settings.</span></span> <span data-ttu-id="d97e4-125">Vaše připojení RDP je obnoveno.</span><span class="sxs-lookup"><span data-stu-id="d97e4-125">Your RDP connection is re-established.</span></span>

6. <span data-ttu-id="d97e4-126">Na příkazovém řádku zadejte *ipconfig /all*.</span><span class="sxs-lookup"><span data-stu-id="d97e4-126">From a command prompt, type *ipconfig /all*.</span></span> <span data-ttu-id="d97e4-127">Zobrazí se všechny IP adresy, které jste přidali, a protokol DHCP je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="d97e4-127">All IP addresses you added are shown and DHCP is turned off.</span></span>


### <a name="validation-windows"></a><span data-ttu-id="d97e4-128">Ověření (Windows)</span><span class="sxs-lookup"><span data-stu-id="d97e4-128">Validation (Windows)</span></span>

<span data-ttu-id="d97e4-129">tooensure jsou možné tooconnect toohello, které internet z vaší sekundární konfiguraci IP adresy prostřednictvím hello veřejná IP adresa spojená, po přidání správně pomocí kroků výše, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d97e4-129">tooensure you are able tooconnect toohello internet from your secondary IP configuration via hello public IP associated it, once you have added it correctly using steps above, use hello following command:</span></span>

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
><span data-ttu-id="d97e4-130">Pro sekundární konfigurace IP můžete pouze ping toohello Internetu, pokud konfigurace hello má veřejnou IP adresu s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="d97e4-130">For secondary IP configurations, you can only ping toohello Internet if hello configuration has a public IP address associated with it.</span></span> <span data-ttu-id="d97e4-131">Pro primární konfigurace protokolu IP veřejnou IP adresu se nenachází požadovaná tooping toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-131">For primary IP configurations, a public IP address is not required tooping toohello Internet.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="d97e4-132">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="d97e4-132">Linux (Ubuntu)</span></span>

1. <span data-ttu-id="d97e4-133">Otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-133">Open a terminal window.</span></span>
2. <span data-ttu-id="d97e4-134">Ujistěte se, že jste hello kořenového uživatele.</span><span class="sxs-lookup"><span data-stu-id="d97e4-134">Make sure you are hello root user.</span></span> <span data-ttu-id="d97e4-135">Pokud si nejste, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d97e4-135">If you are not, enter hello following command:</span></span>

    ```bash
    sudo -i
    ```

3. <span data-ttu-id="d97e4-136">Aktualizujte konfigurační soubor hello hello síťového rozhraní (za předpokladu, eth0').</span><span class="sxs-lookup"><span data-stu-id="d97e4-136">Update hello configuration file of hello network interface (assuming ‘eth0’).</span></span>

    * <span data-ttu-id="d97e4-137">Zachovat existující položka řádku hello protokolu DHCP.</span><span class="sxs-lookup"><span data-stu-id="d97e4-137">Keep hello existing line item for dhcp.</span></span> <span data-ttu-id="d97e4-138">primární adresa IP Hello zůstane nakonfigurované jako dříve.</span><span class="sxs-lookup"><span data-stu-id="d97e4-138">hello primary IP address remains configured as it was previously.</span></span>
    * <span data-ttu-id="d97e4-139">Přidáte konfiguraci pro další statickou IP adresu s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d97e4-139">Add a configuration for an additional static IP address with hello following commands:</span></span>

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    <span data-ttu-id="d97e4-140">Měl by se zobrazit soubor .cfg.</span><span class="sxs-lookup"><span data-stu-id="d97e4-140">You should see a .cfg file.</span></span>
4. <span data-ttu-id="d97e4-141">Soubor otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="d97e4-141">Open hello file.</span></span> <span data-ttu-id="d97e4-142">Měli byste vidět hello následující řádky na konci hello hello souboru:</span><span class="sxs-lookup"><span data-stu-id="d97e4-142">You should see hello following lines at hello end of hello file:</span></span>

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. <span data-ttu-id="d97e4-143">Přidejte následující řádky po hello řádky, které existují v tomto souboru hello:</span><span class="sxs-lookup"><span data-stu-id="d97e4-143">Add hello following lines after hello lines that exist in this file:</span></span>

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. <span data-ttu-id="d97e4-144">Uložte soubor hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-144">Save hello file by using hello following command:</span></span>

    ```bash
    :wq
    ```

7. <span data-ttu-id="d97e4-145">Resetování hello síťové rozhraní s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-145">Reset hello network interface with hello following command:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="d97e4-146">Spusťte ifdown a ifup v hello stejné řádku při použití vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="d97e4-146">Run both ifdown and ifup in hello same line if using a remote connection.</span></span>
    >

8. <span data-ttu-id="d97e4-147">Ověřte, zda text hello IP adresa se přidá toohello síťové rozhraní s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-147">Verify hello IP address is added toohello network interface with hello following command:</span></span>

    ```bash
    ip addr list eth0
    ```

    <span data-ttu-id="d97e4-148">Měli byste vidět hello IP adresy můžete přidat jako součást hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-148">You should see hello IP address you added as part of hello list.</span></span>

### <a name="linux-redhat-centos-and-others"></a><span data-ttu-id="d97e4-149">Linux (Red Hat, CentOS a další)</span><span class="sxs-lookup"><span data-stu-id="d97e4-149">Linux (Redhat, CentOS, and others)</span></span>

1. <span data-ttu-id="d97e4-150">Otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-150">Open a terminal window.</span></span>
2. <span data-ttu-id="d97e4-151">Ujistěte se, že jste hello kořenového uživatele.</span><span class="sxs-lookup"><span data-stu-id="d97e4-151">Make sure you are hello root user.</span></span> <span data-ttu-id="d97e4-152">Pokud si nejste, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d97e4-152">If you are not, enter hello following command:</span></span>

    ```bash
    sudo -i
    ```

3. <span data-ttu-id="d97e4-153">Zadejte své heslo a postupujte podle zobrazených pokynů.</span><span class="sxs-lookup"><span data-stu-id="d97e4-153">Enter your password and follow instructions as prompted.</span></span> <span data-ttu-id="d97e4-154">Až se hello kořenového uživatele, přejděte toohello síťové skripty složky s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-154">Once you are hello root user, navigate toohello network scripts folder with hello following command:</span></span>

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. <span data-ttu-id="d97e4-155">Seznam hello související s ifcfg soubory pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-155">List hello related ifcfg files using hello following command:</span></span>

    ```bash
    ls ifcfg-*
    ```

    <span data-ttu-id="d97e4-156">Měli byste vidět *ifcfg eth0* jako jeden ze souborů hello.</span><span class="sxs-lookup"><span data-stu-id="d97e4-156">You should see *ifcfg-eth0* as one of hello files.</span></span>

5. <span data-ttu-id="d97e4-157">tooadd IP adresu, vytvořte konfigurační soubor pro něj, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="d97e4-157">tooadd an IP address, create a configuration file for it as shown below.</span></span> <span data-ttu-id="d97e4-158">Pro každou konfiguraci IP adresy se musí vytvořit jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="d97e4-158">Note that one file must be created for each IP configuration.</span></span>

    ```bash
    touch ifcfg-eth0:0
    ```

6. <span data-ttu-id="d97e4-159">Otevřete hello *ifcfg-eth0:0* soubor s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-159">Open hello *ifcfg-eth0:0* file with hello following command:</span></span>

    ```bash
    vi ifcfg-eth0:0
    ```

7. <span data-ttu-id="d97e4-160">Přidejte soubor obsahu toohello *eth0:0* v takovém případě s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d97e4-160">Add content toohello file, *eth0:0* in this case, with hello following command.</span></span> <span data-ttu-id="d97e4-161">Být jisti tooupdate informace založené na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-161">Be sure tooupdate information based on your IP address.</span></span>

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. <span data-ttu-id="d97e4-162">Uložte soubor hello se hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d97e4-162">Save hello file with hello following command:</span></span>

    ```bash
    :wq
    ```

9. <span data-ttu-id="d97e4-163">Restartujte hello síťové služby a ověřte, že hello změny jsou úspěšné spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d97e4-163">Restart hello network services and make sure hello changes are successful by running hello following commands:</span></span>

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    <span data-ttu-id="d97e4-164">Měli byste vidět hello IP adres jste přidali, *eth0:0*, v seznamu hello vrátila.</span><span class="sxs-lookup"><span data-stu-id="d97e4-164">You should see hello IP address you added, *eth0:0*, in hello list returned.</span></span>

### <a name="validation-linux"></a><span data-ttu-id="d97e4-165">Ověření (Linux)</span><span class="sxs-lookup"><span data-stu-id="d97e4-165">Validation (Linux)</span></span>

<span data-ttu-id="d97e4-166">tooensure jsou možné tooconnect toohello Internetu z vaší sekundární konfiguraci IP adresy prostřednictvím hello veřejné IP adresy přidružené ji, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d97e4-166">tooensure you are able tooconnect toohello internet from your secondary IP configuration via hello public IP associated it, use hello following command:</span></span>

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
><span data-ttu-id="d97e4-167">Pro sekundární konfigurace IP můžete pouze ping toohello Internetu, pokud konfigurace hello má veřejnou IP adresu s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="d97e4-167">For secondary IP configurations, you can only ping toohello Internet if hello configuration has a public IP address associated with it.</span></span> <span data-ttu-id="d97e4-168">Pro primární konfigurace protokolu IP veřejnou IP adresu se nenachází požadovaná tooping toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-168">For primary IP configurations, a public IP address is not required tooping toohello Internet.</span></span>

<span data-ttu-id="d97e4-169">Pro virtuální počítače s Linuxem, při pokusu o toovalidate odchozí připojení z sekundárního síťového adaptéru může být nutné tooadd odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="d97e4-169">For Linux VMs, when trying toovalidate outbound connectivity from a secondary NIC, you may need tooadd appropriate routes.</span></span> <span data-ttu-id="d97e4-170">Existuje mnoho způsobů toodo to.</span><span class="sxs-lookup"><span data-stu-id="d97e4-170">There are many ways toodo this.</span></span> <span data-ttu-id="d97e4-171">Další informace najdete v odpovídající dokumentaci k vaší distribuci Linuxu.</span><span class="sxs-lookup"><span data-stu-id="d97e4-171">Please see appropriate documentation for your Linux distribution.</span></span> <span data-ttu-id="d97e4-172">Následující Hello je jedna metoda tooaccomplish toto:</span><span class="sxs-lookup"><span data-stu-id="d97e4-172">hello following is one method tooaccomplish this:</span></span>

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- <span data-ttu-id="d97e4-173">Být jisti tooreplace:</span><span class="sxs-lookup"><span data-stu-id="d97e4-173">Be sure tooreplace:</span></span>
    - <span data-ttu-id="d97e4-174">**10.0.0.5** s hello privátní IP adresu, která má veřejnou IP adresou přidružené tooit</span><span class="sxs-lookup"><span data-stu-id="d97e4-174">**10.0.0.5** with hello private IP address that has a public IP address associated tooit</span></span>
    - <span data-ttu-id="d97e4-175">**10.0.0.1** tooyour výchozí brány</span><span class="sxs-lookup"><span data-stu-id="d97e4-175">**10.0.0.1** tooyour default gateway</span></span>
    - <span data-ttu-id="d97e4-176">**eth2** toohello název vaší sekundární síťové karty</span><span class="sxs-lookup"><span data-stu-id="d97e4-176">**eth2** toohello name of your secondary NIC</span></span>
