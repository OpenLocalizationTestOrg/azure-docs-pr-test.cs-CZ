---
title: "aaaCreate poznámkového bloku Jupyter/IPython | Microsoft Docs"
description: "Zjistěte, jak vytvořit toodeploy hello poznámkového bloku Jupyter/IPython na virtuální počítač s Linuxem pomocí modelu nasazení resource manager hello v Azure."
services: virtual-machines-linux
documentationcenter: python
author: crwilcox
manager: wpickett
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 519f36dd-865e-4c1d-abe7-b87037796aa7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 11/10/2015
ms.author: crwilcox
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7f2e45a8ba95163ebfb0f10babc91a2b3fd9390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a>Vytváření virtuálního počítače Azure, instalaci Jupyter a systémem Azure poznámkového bloku Jupyter
Hello [Jupyter projektu](http://jupyter.org), dříve hello [IPython projektu](http://ipython.org), poskytuje kolekci nástrojů pro vědecké výpočty pomocí výkonné interaktivní shells aplikací, které spojují provádění kódu s hello vytvoření za provozu výpočetní dokumentu. Tyto soubory poznámkového bloku může obsahovat libovolný text, matematické vzorce, vstupní kódu, výsledky, grafiky, videa a jakýkoli jiný druh média, jestli je schopná zobrazit moderní webový prohlížeč. Jestli jste absolutně nové tooPython a chcete toolearn ho v fun, interaktivní prostředí nebo proveďte některé závažné paralelní a technické computing, hello Poznámkový blok Jupyter je je služba skvělou volbou.

![Snímek obrazovky](./media/jupyter-notebook/ipy-notebook-spectral.png) pomocí SciPy a Matplotlib balíčky tooanalyze hello struktura záznamu zvuku.

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter dvěma způsoby: Vlastní nasazení nebo Azure poznámkové bloky
Azure poskytuje službu, kterou můžete použít příliš[rychle začít používat Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Pomocí hello Azure Service Poznámkový blok, můžete snadno získat přístup tooJupyter přístupné na webu rozhraní k škálovatelný výpočetní prostředky s všechny power hello jazyka Python a jeho mnoho knihovny.  Vzhledem k tomu, že instalace hello se proto službou hello, uživatelé můžou používat tyto prostředky bez hello potřeba pro správu a konfiguraci uživatelem hello.

Pokud služba hello Poznámkový blok pro váš scénář nefunguje. Pokračujte prosím tooread tohoto článku, které se vám ukáže jak toodeploy hello Poznámkový blok Jupyter v Microsoft Azure, pomocí Linuxové virtuální počítače (VM).

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Vytvoření a konfigurace virtuálního počítače v Azure
prvním krokem Hello je toocreate virtuální počítač (VM) spuštěné v Azure.
Tento virtuální počítač je v cloudu hello úplný operační systém a se použije ke spuštění hello poznámkového bloku Jupyter. Je schopen spuštění virtuálních počítačů Linux a Windows Azure a vám nabídneme hello nastavení Jupyter na oba typy virtuálních počítačů.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Vytvoření virtuálního počítače s Linuxem a otevře port pro Jupyter
Postupujte podle pokynů hello [sem] [ portal-vm-linux] toocreate virtuální počítač hello *Ubuntu* distribuce. Tento kurz používá Ubuntu Server 14.04 LTS. Budeme předpokládat hello uživatelské jméno *azureuser*.

Po hello virtuální počítač nasadí potřebujeme tooopen až pravidlo zabezpečení na skupinu zabezpečení sítě hello.  Z hello portálu Azure, přejděte příliš**skupin zabezpečení sítě** a otevřete hello karta hello skupiny zabezpečení odpovídající tooyour virtuálních počítačů. Je nutné tooadd pravidlo příchozí zabezpečení s hello následující nastavení: **TCP** pro protokol hello  **\***  pro port hello zdroje (veřejných) a **9999** pro Hello cílový (soukromý) port.

![snímek obrazovky](./media/jupyter-notebook/azure-add-endpoint.png)

Když ve skupině zabezpečení sítě, klikněte na **síťových rozhraní** a Poznámka hello **veřejnou IP adresu** podle potřeby tooconnect tooyour virtuálních počítačů bude v dalším kroku hello.

## <a name="install-required-software-on-hello-vm"></a>Nainstalujte požadovaný software na hello virtuálních počítačů
toorun hello Poznámkový blok Jupyter naše virtuálního počítače, nám nejdřív nainstalovat Jupyter a jeho závislosti. Připojte virtuální počítač s linuxem tooyour pomocí ssh a hello pár hello uživatelské jméno a heslo jste si zvolili při vytváření virtuálního počítače. V tomto kurzu jsme použití klienta PuTTY a připojení ze systému Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Instalace na Ubuntu Jupyter
Nainstalujte Anacondu, distribuci jazyka python vědecké účely oblíbených data, pomocí některého z odkazů hello uvedených z [poměrně Analytics](https://www.continuum.io/downloads).  Od verze hello zápis tohoto dokumentu, jsou hello pod odkazy hello nejvíce až toodate verze.

#### <a name="anaconda-installs-for-linux"></a>Nainstaluje anaconda pro Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64bitová verze</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64bitová verze</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32bitové</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32bitové</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Instalace Anaconda3 2.3.0 64-bit na Ubuntu
Jako příklad to je, jak můžete nainstalovat Anaconda Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter toohello latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![snímek obrazovky](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a>Konfigurace Jupyter a použití protokolu SSL
Po instalaci potřebujeme tootake chvíli toosetup hello konfigurační soubory pro Jupyter. Pokud máte potíže při konfiguraci Jupyter může být užitečné toolook v hello [serverem poznámkového bloku Jupyter dokumentaci](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Další jsme `cd` toohello profilu directory toocreate naše certifikát SSL a upravovat soubor konfigurace profilů hello.

V systému Linux použijte následující příkaz hello.

    cd ~/.jupyter

Použijte následující příkaz toocreate hello certifikát SSL (Linux a Windows) hello.

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Všimněte si, že vzhledem k tomu, že se vytváří certifikát podepsaný svým držitelem SSL při připojování toohello Poznámkový blok prohlížeče získáte upozornění zabezpečení.  Pro dlouhodobou produkční použití budete chtít toouse správně podepsaný držitelem přidružené k vaší organizaci.  Vzhledem k tomu, že certifikát správy je mimo rozsah hello tuto ukázku, jsme se teď přilepit tooa certifikát podepsaný svým držitelem.

Kromě toho toousing certifikát, je taky nutné zadat heslo tooprotect Poznámkový blok před neoprávněným použitím.  Z bezpečnostních důvodů Jupyter používá šifrovaných hesel v konfiguračním souboru, takže budete potřebovat tooencrypt heslo nejprve.  IPython poskytuje nástroj toodo tak; na příkazovém řádku spusťte následující příkaz hello.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Zobrazí výzvu k zadání heslo a potvrzení a bude poté vytiskněte hello heslo. Poznámka: pro hello následující krok.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided hello rest for security)

V dalším kroku upravíme hello profil konfigurační soubor, který je `jupyter_notebook_config.py` soubor v adresáři hello v.  Všimněte si, že tento soubor neexistuje – stačí vytvořit případě hello.  Tento soubor má počet polí a ve výchozím nastavení všechny jsou vloženy do komentáře.  Tento soubor můžete otevřít pomocí libovolného textového editoru z libovolně a měli byste zajistit, že má aspoň hello následující obsah. **Být jisti c.NotebookApp.password hello tooreplace v konfiguračním hello s hello sha1 hello v předchozím kroku**.

    c = get_config()

    # You must give hello path toohello certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-hello-jupyter-notebook"></a>Spustit hello poznámkového bloku Jupyter
V tomto okamžiku se připravené toostart hello poznámkového bloku Jupyter. toodo, přejděte toohello directory chcete toostore poznámkových bloků a začínat hello Poznámkový blok Jupyter server hello následující příkaz.

    /anaconda3/bin/jupyter-notebook

Nyní byste měli být schopný tooaccess Poznámkový blok Jupyter na adrese hello `https://[PUBLIC-IP-ADDRESS]:9999`.

Pokud nejprve přístup k Poznámkový blok, požádá přihlašovací stránku hello k zadání hesla. A jakmile se přihlásíte, zobrazí se vám hello "Jupyter poznámkového bloku řídicího panelu", který je obrazit řídicí centrum pro všechny operace poznámkového bloku.  Z této stránky můžete vytvořit nové poznámkových bloků a otevřít existující.

![snímek obrazovky](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-hello-jupyter-notebook"></a>Pomocí hello poznámkového bloku Jupyter
Pokud kliknete na tlačítko hello **nový** tlačítko, zobrazí se následující úvodní stránce hello.

![snímek obrazovky](./media/jupyter-notebook/jupyter-untitled-notebook.png)

Hello oblasti označené `In []:` řádku je hello vstupní oblast a zde je možné zadat libovolný platný kód Python a provede při zásahu `Shift-Enter` nebo klikněte na ikonu "Přehrání" hello (hello trojúhelník směřující vpravo v panelu nástrojů hello).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Efektivní zlepší: výpočetní dokumentů s bohatý mediální za provozu
Poznámkový blok Hello samotné by měli považovat velmi přirozené tooanyone, který se používá Python a textový editor, protože je v některých způsobech kombinaci obou: můžete spustit bloky kódu jazyka Python, ale můžete také ponechat poznámky a další text příliš změnou styl buněk z "Kód" hello "Ma rkdown"hello rozevírací nabídky v panelu nástrojů.

Jupyter je mnohem víc než textový editor jako umožňuje směšování výpočtů a bohaté média (text, grafiky, videa a prakticky nic, co můžete zobrazit moderní webový prohlížeč). Je možné kombinovat, text, kód, videa a další!

![snímek obrazovky](./media/jupyter-notebook/jupyter-editing-experience.png)

A s výkonem hello mnoho vynikající knihoven jazyka Python pro technické a vědecké výpočty v hello následující snímek obrazovky, lze provést jednoduchý výpočet hello stejné usnadňují jako komplexní sítě analýzy, všechny v jednom prostředí.

Tato zlepší promíchání hello power moderních webových hello s za provozu výpočetní nabízí mnoho možností a je ideální pro hello cloud; dá se Hello Poznámkový blok:

* Jako výpočetní zápisník toorecord nahodilého fungovat na problém.
* tooshare výsledků s kolegy v 'za provozu, výpočetní formuláře nebo v výtisk formátů (HTML, PDF).
* toodistribute a přítomen za provozu vyučující materiálů, které se týkají výpočtů, takže studenty okamžitě můžete experimentovat s hello skutečné kód, upravte ji a znovu spustit interaktivně.
* tooprovide "spustitelné papír", která představují hello výsledky research způsobem, který může být okamžitě opakuje, ověřit a rozšířené třetími stranami.
* Jako platforma pro spolupráci výpočty: více mohou uživatelů přihlásit toothe stejný server poznámkového bloku tooshare výpočetní relaci za provozu.

Pokud přejdete toohello IPython zdrojového kódu [úložiště][repository], zjistíte, celého adresáře s příklady Poznámkový blok, které můžete stáhnout a potom experimentovat s na své vlastní virtuální počítač Azure Jupyter.  Jednoduše stáhnout hello `.ipynb` soubory z hello lokality a odešlete do řídicího panelu hello v poznámkovém bloku virtuální počítač Azure (nebo je stáhnout přímo do hello virtuálních počítačů).

## <a name="conclusion"></a>Závěr
Hello Poznámkový blok Jupyter poskytuje výkonné rozhraní pro přístup k interaktivním hello power ekosystému hello Python v Azure.  Pokrývá širokou škálu případy využití, včetně jednoduchého zkoumání a učení Python, analýzy dat a vizualizaci, simulace a paralelní výpočty. Hello výsledné poznámkového bloku dokumenty obsahují úplný záznam hello výpočtů, u kterých se provádějí a je možné sdílet s ostatními uživateli Jupyter.  Hello Poznámkový blok Jupyter lze použít jako místní aplikace, ale jeho je ideální pro nasazení cloudu v Azure

základní funkce Hello Jupyter jsou také k dispozici v sadě Visual Studio prostřednictvím [Python Tools pro Visual Studio] [ Python Tools for Visual Studio] (PTVS). PTVS je bezplatný a open source od společnosti Microsoft, který profilování a paralelní se změní rozšířené vývoj prostředí Python, které obsahuje editoru upřesnit pomocí IntelliSense, ladění, Visual Studio modulu plug-in computing integrace.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
