---
title: "Vytvoření poznámkového bloku Jupyter/IPython | Microsoft Docs"
description: "Informace o nasazení poznámkového bloku Jupyter/IPython na virtuální počítač s Linuxem vytvořené pomocí modelu nasazení resource manager v Azure."
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
ms.openlocfilehash: b5940190822cd5c5b78ea0e8f5c8695608d351d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a>Vytváření virtuálního počítače Azure, instalaci Jupyter a systémem Azure poznámkového bloku Jupyter
[Jupyter projektu](http://jupyter.org), dříve [IPython projektu](http://ipython.org), poskytuje kolekci nástrojů pro vědecké výpočty pomocí výkonné interaktivní shells aplikací, které spojují provádění kódu s vytváření živé výpočetní dokumentu. Tyto soubory poznámkového bloku může obsahovat libovolný text, matematické vzorce, vstupní kódu, výsledky, grafiky, videa a jakýkoli jiný druh média, jestli je schopná zobrazit moderní webový prohlížeč. Jestli jste absolutně nové Python a chcete další v prostředí zábavné, interaktivní nebo provést některé závažné paralelní a technické computing, je poznámkového bloku Jupyter je služba skvělou volbou.

![Snímek obrazovky](./media/jupyter-notebook/ipy-notebook-spectral.png) pomocí SciPy a Matplotlib balíčků struktura záznamu zvuku.

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter dvěma způsoby: Vlastní nasazení nebo Azure poznámkové bloky
Azure poskytuje službu, která vám pomůže [rychle začít používat Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Pomocí poznámkového bloku služby Azure, můžete snadno získat přístup k rozhraní přístupné na webu na Jupyter pro škálovatelný výpočetní prostředky všechny sílu jazyka Python a jeho mnoho knihovny.  Vzhledem k tomu, že instalace používá služba, uživatelé můžou používat tyto prostředky bez nutnosti pro správu a konfiguraci uživatelem.

Pokud službu poznámkového bloku nefunguje pro váš scénář pokračujte ke čtení tohoto článku, které se vám ukáže, jak nasadit poznámkového bloku Jupyter v Microsoft Azure, pomocí Linuxové virtuální počítače (VM).

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Vytvoření a konfigurace virtuálního počítače v Azure
Prvním krokem je vytvoření virtuálního počítače (VM) spuštěné v Azure.
Tento virtuální počítač je úplný operační systém v cloudu a se použije ke spuštění poznámkového bloku Jupyter. Je schopen spuštění virtuálních počítačů Linux a Windows Azure a vám nabídneme nastavení Jupyter na oba typy virtuálních počítačů.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Vytvoření virtuálního počítače s Linuxem a otevře port pro Jupyter
Postupujte podle pokynů, [sem] [ portal-vm-linux] k vytvoření virtuálního počítače *Ubuntu* distribuce. Tento kurz používá Ubuntu Server 14.04 LTS. Budeme předpokládat uživatelské jméno *azureuser*.

Po nasazení virtuálního počítače je potřeba otevře pravidlo zabezpečení na skupinu zabezpečení sítě.  Z portálu Azure, přejděte na **skupin zabezpečení sítě** a otevřete kartu pro skupinu zabezpečení odpovídající virtuální počítač. Je nutné přidat pravidlo příchozí zabezpečení s následujícím nastavením: **TCP** pro protokol  **\***  pro zdrojový (veřejných) port a **9999** pro cílový (soukromý) port.

![snímek obrazovky](./media/jupyter-notebook/azure-add-endpoint.png)

Když ve skupině zabezpečení sítě, klikněte na **síťových rozhraní** a poznamenejte si **veřejnou IP adresu** jako bude potřeba pro připojení k virtuálnímu počítači v dalším kroku.

## <a name="install-required-software-on-the-vm"></a>Do virtuálního počítače nainstalujte požadovaný software
Pro spouštění poznámkového bloku Jupyter v našem virtuálních počítačů, jsme musíte nejprve nainstalovat Jupyter a jeho závislosti. Připojte se k linux virtuálního počítače pomocí ssh a uživatelského jména a hesla spárujte jste zvolili při vytváření virtuálního počítače. V tomto kurzu jsme použití klienta PuTTY a připojení ze systému Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Instalace na Ubuntu Jupyter
Nainstalujte Anacondu, distribuci jazyka python vědecké účely oblíbených data, pomocí některého z odkazů uvedených z [poměrně Analytics](https://www.continuum.io/downloads).  Před sepsáním tohoto dokumentu, níž odkazy jsou nejaktuálnější verze datum.

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

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![snímek obrazovky](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a>Konfigurace Jupyter a použití protokolu SSL
Po instalaci je potřeba nastavit konfigurační soubory pro Jupyter chvíli trvat. Pokud máte potíže při konfiguraci Jupyter může být užitečné prohlédnout si [serverem poznámkového bloku Jupyter dokumentaci](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Další jsme `cd` k adresáři profil vytvořit certifikát SSL a upravit profily konfigurační soubor.

V systému Linux použijte následující příkaz.

    cd ~/.jupyter

Použijte následující příkaz k vytvoření certifikátu protokolu SSL (Linux a Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Pamatujte, že vzhledem k tomu, že se vytváří certifikát podepsaný svým držitelem SSL při připojování k poznámkového bloku prohlížeče vám poskytne upozornění zabezpečení.  Pro dlouhodobou produkční použití budete chtít použít certifikát správně podepsaný přidružené k vaší organizaci.  Vzhledem k tomu, že certifikát správy je nad rámec tuto ukázku, jsme se na certifikát podepsaný svým držitelem přilepit teď.

Kromě používá certifikát, musíte také zadat heslo k ochraně Poznámkový blok před neoprávněným použitím.  Z bezpečnostních důvodů Jupyter používá šifrované hesla v konfiguračním souboru, takže budete muset heslo šifrování nejprve.  IPython poskytuje nástroj k tomu; na příkazovém řádku spusťte následující příkaz.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Zobrazí výzvu k zadání heslo a potvrzení a bude poté vytiskněte heslo. Poznámka: pro následující krok.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Potom jsme upravte konfigurační soubor pro tento profil, který je `jupyter_notebook_config.py` soubor v adresáři, jsou v.  Všimněte si, že tento soubor neexistuje – stačí vytvořit Pokud tomu tak je.  Tento soubor má počet polí a ve výchozím nastavení všechny jsou vloženy do komentáře.  Tento soubor můžete otevřít pomocí libovolného textového editoru z libovolně a měli zkontrolujte, zda má alespoň na následující obsah. **Nezapomeňte nahradit c.NotebookApp.password v konfiguračním sha1 z předchozího kroku**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Spustit poznámkového bloku Jupyter
V tuto chvíli jsme připraveni začít poznámkového bloku Jupyter. Chcete-li to provést, přejděte do adresáře, které chcete uložit poznámkových bloků v a spustí se server Poznámkový blok Jupyter pomocí následujícího příkazu.

    /anaconda3/bin/jupyter-notebook

Teď by měla být mít přístup k vaší Poznámkový blok Jupyter na adrese `https://[PUBLIC-IP-ADDRESS]:9999`.

Pokud nejprve přístup k Poznámkový blok, požádá přihlašovací stránku k zadání hesla. A jakmile se přihlásíte, zobrazí se vám "Jupyter poznámkového bloku řídicího panelu", který je obrazit řídicí centrum pro všechny operace poznámkového bloku.  Z této stránky můžete vytvořit nové poznámkových bloků a otevřít existující.

![snímek obrazovky](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Pomocí poznámkového bloku Jupyter
Pokud kliknete **nový** tlačítko, zobrazí se následující úvodní stránce.

![snímek obrazovky](./media/jupyter-notebook/jupyter-untitled-notebook.png)

V oblasti označené `In []:` řádku je vstupní oblast a zde je možné zadat libovolný platný kód Python a provede při zásahu `Shift-Enter` nebo klikněte na ikonu "Play" (pravém trojúhelník směřující na panelu nástrojů).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Efektivní zlepší: výpočetní dokumentů s bohatý mediální za provozu
Poznámkový blok, samotné by měli považovat velmi přirozené všem uživatelům, kteří se používá Python a textový editor, protože je v některých způsobech kombinaci obou: můžete spustit bloky kódu jazyka Python, ale můžete také ponechat poznámky a další text změnou stylu buňky z "Kód" do "Markdo WN"v rozevírací nabídce panelu nástrojů.

Jupyter je mnohem víc než textový editor jako umožňuje směšování výpočtů a bohaté média (text, grafiky, videa a prakticky nic, co můžete zobrazit moderní webový prohlížeč). Je možné kombinovat, text, kód, videa a další!

![snímek obrazovky](./media/jupyter-notebook/jupyter-editing-experience.png)

A s výkonem mnoho vynikající knihoven jazyka Python pro technické a vědecké výpočty, na následujícím snímku obrazovky že jednoduchý výpočet lze provést stejně snadné jako analýzy komplexní sítě, všechny v jednom prostředí.

Tato zlepší výkon moderní webové promíchání s za provozu výpočetní nabízí mnoho možností a je ideální pro cloud; můžete použít Poznámkový blok:

* Jako výpočetní zápisník k zaznamenání nahodilého fungovat na problém.
* Chcete-li sdílet s kolegy výsledky, v "live" výpočetní formuláře nebo v výtisk formátů (HTML, PDF).
* Distribuovat a prezentovat za provozu vyučující materiály, které zahrnují výpočtů, takže studenty okamžitě můžete experimentovat s kód skutečné, upravit jej a znovu spustit interaktivně.
* K zajištění "spustitelné papír", která představují výsledky research způsobem, který může být okamžitě opakuje, ověřit a rozšířit pomocí jiné.
* Jako platforma pro spolupráci výpočty: více uživatelů se můžete přihlásit na stejný server poznámkového bloku sdílet relaci výpočetní za provozu.

Pokud přejdete ke zdrojovému kódu IPython [úložiště][repository], zjistíte, celého adresáře s příklady Poznámkový blok, které můžete stáhnout a potom experimentovat s na své vlastní virtuální počítač Azure Jupyter.  Jednoduše Stáhnout `.ipynb` soubory z lokality a odešlete na řídicím panelu v poznámkovém bloku virtuální počítač Azure (nebo je stáhnout přímo do virtuálního počítače).

## <a name="conclusion"></a>Závěr
Poznámkového bloku Jupyter poskytuje výkonné rozhraní pro přístup k interaktivním power ekosystému Python v Azure.  Pokrývá širokou škálu případy využití, včetně jednoduchého zkoumání a učení Python, analýzy dat a vizualizaci, simulace a paralelní výpočty. Výsledný poznámkového bloku dokumenty obsahují úplný záznam výpočtů, u kterých se provádějí a je možné sdílet s ostatními uživateli Jupyter.  Poznámkového bloku Jupyter lze použít jako místní aplikace, ale jeho je ideální pro nasazení cloudu v Azure

Hlavní funkce Jupyter jsou také k dispozici v sadě Visual Studio prostřednictvím [Python Tools pro Visual Studio] [ Python Tools for Visual Studio] (PTVS). PTVS je bezplatný a open source od společnosti Microsoft, který profilování a paralelní se změní rozšířené vývoj prostředí Python, které obsahuje editoru upřesnit pomocí IntelliSense, ladění, Visual Studio modulu plug-in computing integrace.

## <a name="next-steps"></a>Další kroky
Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
