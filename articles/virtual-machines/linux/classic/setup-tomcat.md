---
title: "aaaSet až Apache Tomcat na virtuální počítač s Linuxem | Microsoft Docs"
description: "Zjistěte, jak tooset až Apache Tomcat7 pomocí virtuální počítače Azure s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b837a73e91fcb25d5459d993a0e93ceef1a1fc8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Nastavit Tomcat7 na virtuální počítač s Linuxem v Azure
Apache Tomcat (nebo jednoduše Tomcat, také dříve se označovaly jako Jakarta Tomcat) je webový server s otevřeným zdrojem a kontejner servlet vyvinuté hello Apache Software Foundation (amp). Tomcat implementuje hello Java Servlet a specifikace hello JavaServer stránky (JSP) z Sun Microsystems. Tomcat poskytuje čistý Java HTTP prostředí webového serveru v které toorun kódu v jazyce Java. V nejjednodušší konfiguraci hello Tomcat běží v procesu jednoho operačního systému. Tento proces se spustí nástroje Java virtual machine (JVM). Každý požadavek HTTP z prohlížeče tooTomcat zpracovávány jako samostatné vláken v procesu Tomcat hello.  

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek popisuje, jak toouse hello modelu nasazení classic. Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello. toouse toodeploy správce prostředků šablony virtuálního počítače s Ubuntu s otevřete JDK a Tomcat, najdete v části [v tomto článku](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).

V tomto článku bude instalace Tomcat7 na bitovou kopii systému Linux a nasadit v Azure.  

Co se dozvíte:  

* Jak toocreate virtuálního počítače v Azure.
* Jak tooprepare hello virtuálního počítače pro Tomcat7.
* Jak tooinstall Tomcat7.

Předpokládá se, že už máte předplatné Azure.  Pokud není, můžete si zaregistrovat bezplatnou zkušební verzi na [hello webu Azure](https://azure.microsoft.com/). Pokud máte předplatné MSDN, najdete v části [Microsoft Azure speciální ceny: MSDN, MPN a výhody BizSpark](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). toolearn Další informace o Azure, najdete v části [co je Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Tento článek předpokládá, že máte základní znalosti práce Tomcat a Linux.  

## <a name="phase-1-create-an-image"></a>Fáze 1: Vytvoření image
V této fázi vytvoříte virtuální počítač pomocí bitové kopie systému Linux v Azure.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>Krok 1: Generovat ověřovací klíč SSH
SSH je důležité nástroj pro správce systému. Však není doporučeno konfigurace zabezpečení přístupu na základě určit lidské hesla. Uživatelé se zlými úmysly může rozdělit na váš systém podle uživatelského jména a vytváření silných hesel.

Dobrá zpráva Hello je způsob tooleave vzdáleného přístupu otevřete a nestarat se o hesla. Tato metoda se skládá z ověřování s asymetrické šifrování. Hello soukromého klíče uživatele je hello ten, který uděluje hello ověřování. Můžete dokonce uzamknout hello uživatelský účet toonot povolit ověřování hesla.

Další výhodou této metody je nepotřebujete toosign různá hesla v toodifferent servery. Můžete ověřovat pomocí hello osobní privátní klíče ve všech serverech, což zabraňuje s tooremember několik hesel.



Postupujte podle těchto kroků toogenerate hello SSH ověřovací klíč.

1. Stáhněte a nainstalujte PuTTYgen z hello následující umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Spusťte Puttygen.exe.
3. Klikněte na tlačítko **generování** toogenerate hello klíče. V procesu hello můžete zvýšit náhodnost tím přesunutí myši hello přes hello prázdné místo v okně hello.  
   ![PuTTY snímek obrazovky generátor klíč, který ukazuje hello nového klíče tlačítko Generovat][1]
4. Po hello generovat proces, Puttygen.exe se zobrazí nový veřejný klíč.  
   ![PuTTY snímek obrazovky generátor klíč, který ukazuje hello nový veřejný klíč a hello uložení privátního klíče tlačítko][2]
5. Vyberte a zkopírujte hello veřejný klíč a uložit ho do souboru s názvem publicKey.pem. Nemáte klikněte na tlačítko **uložit veřejný klíč**, protože se liší od hello veřejný klíč chceme hello uloženého formátu souboru veřejného klíče.
6. Klikněte na tlačítko **uložit privátní klíč**a uložte ho do souboru s názvem privateKey.ppk.

### <a name="step-2-create-hello-image-in-hello-azure-portal"></a>Krok 2: Vytvoření bitové kopie hello v hello portálu Azure
1. V hello [portál](https://portal.azure.com/), klikněte na tlačítko **nový** v hello úlohy panelu toocreate bitovou kopii. Potom vyberte image hello Linux, která je založena na vašich potřebách. Hello následující příklad používá image Ubuntu 14.04 hello.
![Snímek obrazovky hello portál, který se zobrazí tlačítko Nový hello][3]

2. Pro **název hostitele**, zadejte název hello hello adresu URL, které a internetové klienty použijete tooaccess tohoto virtuálního počítače. Definujte hello poslední část hello název DNS, například tomcatdemo. Azure pak vygeneruje adresu URL hello jako tomcatdemo.cloudapp.net.  

3. Pro **SSH ověřovací klíč**, zkopírujte hodnotu klíče hello z hello publicKey.pem souboru, který obsahuje veřejný klíč hello generované PuTTYgen.  
![Ověřovací klíč SSH pole hello portálu][4]

4. Podle potřeby nakonfigurujte další nastavení a potom klikněte na **vytvořit**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fáze 2: Příprava virtuálního počítače pro Tomcat7
V této fázi konfigurace koncového bodu pro provoz Tomcat a potom se připojte tooyour nového virtuálního počítače.

### <a name="step-1-open-hello-http-port-tooallow-web-access"></a>Krok 1: Otevřete hello HTTP port tooallow webový přístup
Protokol TCP nebo UDP, společně s veřejné a privátní port obsahovat koncové body v Azure. Hello privátní port je port hello, služba hello naslouchá tooon hello virtuálního počítače. Hello veřejný port je port hello hello cloudové služby Azure naslouchá tooexternally pro příchozí, internetový provoz.  

TCP port 8080 je hello výchozí číslo portu, Tomcat používá toolisten. Tento port se při otevření s koncový bod Azure, můžete a dalších internetoví klienti mohou přistupovat Tomcat stránky.  

1. Hello portálu, klikněte na tlačítko **Procházet** > **virtuální počítače**a potom klikněte na hello virtuální počítač, který jste vytvořili.  
   ![Snímek obrazovky directory hello virtuální počítače][5]
2. tooadd koncový bod tooyour virtuální počítač, klikněte na tlačítko hello **koncové body** pole.
   ![Snímek obrazovky zobrazující hello koncové body pole][6]
3. Klikněte na tlačítko **Přidat**.  

   1. Pro koncový bod hello, zadejte název pro koncový bod hello v **koncový bod**a pak zadejte 80 v **veřejný Port**.  

      Pokud je nastavena too80, nepotřebujete číslo portu hello tooinclude hello adresu URL, která je použité tooaccess Tomcat. Například http://tomcatdemo.cloudapp.net.    

      Pokud je nastavena hodnota tooanother, jako je například 81, je nutné tooadd hello port číslo toohello URL tooaccess Tomcat. Například http://tomcatdemo.cloudapp.net:81 /.
   2. Zadejte 8080 v **privátní Port**. Ve výchozím nastavení Tomcat naslouchá na portu TCP 8080. Pokud jste změnili hello výchozí naslouchání portu Tomcat, by měl aktualizovat **privátní Port** toobe hello stejná hodnota jako hello port pro naslouchání Tomcat.  
      ![Snímek obrazovky z uživatelské rozhraní, které se zobrazuje příkaz přidat, veřejný Port a privátní Port][7]
4. Klikněte na tlačítko **OK** tooadd hello koncový bod tooyour virtuálního počítače.

### <a name="step-2-connect-toohello-image-you-created"></a>Krok 2: Připojení toohello bitovou kopii, kterou jste vytvořili
Můžete použít jakéhokoli SSH nástroj tooconnect tooyour virtuálního počítače. V tomto příkladu používáme PuTTY.  

1. Získání názvu DNS hello virtuálního počítače z portálu hello.
    1. Klikněte na tlačítko **Procházet** > **virtuální počítače**.
    2. Vyberte hello název virtuálního počítače a pak klikněte na tlačítko **vlastnosti**.
    3. V hello **vlastnosti** dlaždici, podívejte se v hello **název domény** název DNS hello tooget pole.  

2. Získat hello číslo portu pro připojení SSH z hello **SSH** pole.  
![Snímek obrazovky, který zobrazuje číslo portu pro připojení SSH hello][8]

3. Stáhněte si [PuTTY](http://www.putty.org/).  

4. Po stažení, klikněte na spustitelný soubor hello Putty.exe. V PuTTY konfiguraci konfigurovat základní možnosti hello hello názvem hostitele a portu číslo, které se získávají z hello vlastnosti virtuálního počítače.   
![Snímek obrazovky, který ukazuje možnosti název a port hostitele PuTTY konfigurace hello][9]

5. V levém podokně hello, klikněte na **připojení** > **SSH** > **Auth**a potom klikněte na **Procházet** toospecify Hello umístění souboru privateKey.ppk hello. Hello privateKey.ppk soubor obsahuje hello privátní klíč, který je generovaný PuTTYgen dříve v hello "fáze 1: vytvoření image" tohoto článku.  
![Snímek obrazovky zobrazující hierarchie adresářů hello připojení a tlačítko Procházet][10]

6. Klikněte na tlačítko **otevřete**. Může být upozorněni podle okno se zprávou. Pokud jste nakonfigurovali hello DNS název a číslo portu správně, klikněte na tlačítko **Ano**.
![Snímek obrazovky, který se zobrazí oznámení o hello][11]

7. Můžete se výzvami tooenter vaše uživatelské jméno.  
![Snímek obrazovky, který ukazuje, kde tooenter uživatelské jméno][12]

8. Zadejte uživatelské jméno hello jste použili toocreate hello virtuálního počítače v hello "fáze 1: vytvoření image" dříve v tomto článku. Zobrazí se něco podobného jako hello následující:  
![Snímek obrazovky zobrazující hello ověřování potvrzení][13]

## <a name="phase-3-install-software"></a>Fáze 3: Instalace softwaru
V této fázi nainstalujte prostředí Java runtime hello, Tomcat7 a další součásti Tomcat7.  

### <a name="java-runtime-environment"></a>Prostředí Java runtime
Tomcat je napsán v jazyce Java. Existují dva typy sad Java Development Kit (JDKs), OpenJDK a Oracle JDK. Můžete zvolit hello, kterou potřebujete.  

> [!NOTE]
> Jak JDKs mít prakticky hello stejný kód hello třídy v hello Java API, ale hello kód pro virtuální počítač hello se liší. OpenJDK obvykle otevřené knihovny toouse, zatímco Oracle JDK obvykle toouse uzavřený těch, které jsou. Oracle JDK má více tříd a některé vyřešili chyb a Oracle JDK je stabilnější než OpenJDK.

#### <a name="install-openjdk"></a>Nainstalujte OpenJDK  

Použijte následující příkaz toodownload OpenJDK hello.   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* toocreate directory toocontain hello JDK soubory:  

        sudo mkdir /usr/lib/jvm  
* tooextract hello JDK soubory do hello/usr/lib/jvm/directory:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a>Nainstalujte Oracle JDK


Použijte následující příkaz toodownload Oracle JDK z webu Oracle hello hello.  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* toocreate directory toocontain hello JDK soubory:  

        sudo mkdir /usr/lib/jvm  
* tooextract hello JDK soubory do hello/usr/lib/jvm/directory:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* tooset Oracle JDK jako virtuální počítač Java výchozí hello:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a>Potvrďte, že Java instalace byla úspěšně dokončena
Můžete použít příkaz jako hello následující tootest, pokud je prostředí Java runtime hello správně nainstalován:  

    java -version  

Pokud jste nainstalovali OpenJDK, zobrazí se zpráva podobná následující hello: ![OpenJDK úspěšné instalace zpráv][14]

Pokud jste nainstalovali Oracle JDK, zobrazí zpráva podobná následující hello: ![zpráva instalace úspěšná JDK Oracle][15]

### <a name="install-tomcat7"></a>Instalace Tomcat7
Použijte následující příkaz tooinstall Tomcat7 hello.  

    sudo apt-get install tomcat7  

Pokud nepoužíváte Tomcat7, použijte odpovídající variantu hello tohoto příkazu.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Potvrďte, že je instalace Tomcat7 úspěšné
toocheck Pokud Tomcat7 je úspěšně nainstalován, vyhledejte název DNS serveru Tomcat tooyour. V tomto článku je příklad adresy URL hello http://tomcatexample.cloudapp.net/. Pokud se zobrazí zpráva podobná následující hello, Tomcat7 je správně nainstalován.
![Zpráva úspěšné instalace Tomcat7][16]

### <a name="install-other-tomcat7-components"></a>Instalace součásti Tomcat7
Existují další volitelné součásti Tomcat, které můžete nainstalovat.  

Použití hello **sudo výstižný mezipaměti vyhledávání tomcat7** příkaz toosee všech součástí hello k dispozici. Použijte následující příkazy tooinstall hello některé užitečné součásti.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools toocreate user instances  

## <a name="phase-4-configure-tomcat7"></a>Fáze 4: Konfigurace Tomcat7
V této fázi spravovat Tomcat.

### <a name="start-and-stop-tomcat7"></a>Spuštění a zastavení Tomcat7
Hello Tomcat7 server se automaticky spustí, když ho nainstalujete. Můžete také spustit s hello následující příkaz:   

    sudo /etc/init.d/tomcat7 start

toostop Tomcat7:

    sudo /etc/init.d/tomcat7 stop

Stav hello tooview Tomcat7:

    sudo /etc/init.d/tomcat7 status

toorestart Tomcat services: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Správa Tomcat7
Můžete upravit hello Tomcat uživatele konfigurační soubor tooset až přihlašovacích údajů správce. Hello použijte následující příkaz:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Zde naleznete příklad:  
![Snímek obrazovky zobrazující výstupu příkazu vi sudo hello][17]  

> [!NOTE]
> Vytvořte silné heslo pro uživatelské jméno správce hello.  

Po úpravě tohoto souboru, musíte restartovat služby Tomcat7 s následující příkaz tooensure hello změny projeví hello:  

    sudo /etc/init.d/tomcat7 restart  

Otevřete prohlížeč a zadejte **http://<your tomcat server DNS name>/manager/html** jako hello adresy URL. Například hello v tomto článku adresa URL hello je http://tomcatexample.cloudapp.net/manager/html.  

Po připojení, měli byste vidět něco podobné toohello následující:  
![Snímek obrazovky hello správce Tomcat webových aplikací][18]

## <a name="common-issues"></a>Běžné problémy
### <a name="cant-access-hello-virtual-machine-with-tomcat-and-moodle-from-hello-internet"></a>Nelze získat přístup hello virtuální počítač s Tomcat a Moodle z hello Internetu
#### <a name="symptom"></a>Příznaky  
  Tomcat běží, ale nemůžete zobrazit hello Tomcat výchozí stránku v prohlížeči.
#### <a name="possible-root-cause"></a>Možné příčiny   

  * port pro naslouchání Tomcat Hello není hello stejné jako hello privátní port koncového bodu virtuálního počítače pro provoz Tomcat.  

     Port pro naslouchání zkontrolujte veřejný port a privátní port nastavení koncového bodu a zajistěte, aby privátní port hello je hello stejná hodnota jako hello Tomcat. Najdete v části "fáze 1: vytvoření image" tohoto článku Pokyny ke konfiguraci koncových bodů pro virtuální počítač.  

     toodetermine hello Tomcat port pro naslouchání, otevřete /etc/httpd/conf/httpd.conf (Red Hat vydání) nebo /etc/tomcat7/server.xml (Debian vydání). Ve výchozím nastavení je hello port pro naslouchání Tomcat 8080. Zde naleznete příklad:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Pokud používáte virtuální počítač jako Debian a Ubuntu a chcete, aby toochange hello výchozí port z Tomcat naslouchání (například 8081), měli byste taky otevřít port hello hello operačního systému. První, otevřete hello profil:  

        sudo vi /etc/default/tomcat7  

     Potom Odkomentujte hello poslední řádek a změňte "žádný" příliš "Ano".  

        AUTHBIND=yes
  2. brány firewall Hello zakázal hello naslouchání portu z Tomcat.

     Zobrazí se pouze hello Tomcat výchozí stránky z místního hostitele hello. problém Hello je velmi pravděpodobné, že hello port, který je naslouchali tooby Tomcat, je blokována bránou hello firewall. Můžete vytvořit webovou stránku hello w3m nástroj toobrowse hello. Hello následujících příkazů nainstalujte w3m a procházet toohello Tomcat výchozí stránky:  


        sudo yum instalace w3m w3m-img


        w3m adrese http://localhost: 8080  
#### <a name="solution"></a>Řešení

  * Pokud hello port pro naslouchání Tomcat není hello stejné jako privátní port hello hello koncového bodu pro provoz toohello virtuální počítač, můžete potřebovat změnit privátní port hello toobe hello stejná hodnota jako hello port pro naslouchání Tomcat.   
  2. Pokud je hello problém způsoben brány firewall nebo iptables, přidejte následující řádky příliš/etc/sysconfig/iptables hello. druhý řádek Hello je potřeba jenom pro provoz https:  

      -A -p tcp -m tcp – dport 80 -j přijmout vstup

      -A -p tcp -m tcp – dport 443 -j přijmout vstup  

     > [!IMPORTANT]
     > Zajistěte, aby hello předchozí řádky jsou umístěny nad všechny řádky, které by globálně omezení přístupu, jako je například následující hello: - A -j ODMÍTNĚTE – odmítněte with icmp hostitele zakázáno vstup



tooreload hello iptables, spusťte následující příkaz hello:

    service iptables restart

To byl otestován v CentOS 6.3.

### <a name="permission-denied-when-you-upload-project-files-toovarlibtomcat7webapps"></a>Oprávnění byla odepřena při nahrávání projektu soubory příliš/var/lib/tomcat7/webapps /
#### <a name="symptom"></a>Příznaky
  Při použití protokolu SFTP klienta (například FileZilla) tooconnect tooyour virtuálního počítače a přejděte příliš/var/lib/tomcat7/webapps/toopublish vašeho webu, dojde k chybě zprávy podobné toohello následující:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Možné příčiny
  Nemáte žádná oprávnění tooaccess hello /var/lib/tomcat7/webapps složka.  
#### <a name="solution"></a>Řešení  
  Potřebujete oprávnění tooget z hello kořenového účtu. Toohello uživatelské jméno kořenové, které jste použili při zřízení hello počítače, můžete změnit vlastnictví hello této složky. Tady je příklad s názvem účtu azureuser hello:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Příliš pomocí hello -R – možnost tooapply hello oprávnění pro všechny soubory v adresáři.  

  Tento příkaz lze použít také pro adresáře. Hello -R – možnost změny hello oprávnění pro všechny soubory a adresáře v rámci adresáře hello. Zde naleznete příklad:  

     sudo chown -R username:group directory  

  Tento příkaz změní vlastnictví (uživatele a skupiny) pro všechny soubory a adresáře, které jsou uvnitř hello adresář.  

  Hello následující příkaz změní jenom oprávnění hello hello složky adresáře. Hello soubory a složky v adresáři hello, nebudou změněny.  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
