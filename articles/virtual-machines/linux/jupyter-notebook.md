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
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a><span data-ttu-id="4b328-103">Vytváření virtuálního počítače Azure, instalaci Jupyter a systémem Azure poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="4b328-103">Creating an Azure VM, installing Jupyter, and running a Jupyter Notebook on Azure</span></span>
<span data-ttu-id="4b328-104">Hello [Jupyter projektu](http://jupyter.org), dříve hello [IPython projektu](http://ipython.org), poskytuje kolekci nástrojů pro vědecké výpočty pomocí výkonné interaktivní shells aplikací, které spojují provádění kódu s hello vytvoření za provozu výpočetní dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4b328-104">hello [Jupyter project](http://jupyter.org), formerly hello [IPython project](http://ipython.org), provides a collection of tools for scientific computing using powerful interactive shells that combine code execution with hello creation of a live computational document.</span></span> <span data-ttu-id="4b328-105">Tyto soubory poznámkového bloku může obsahovat libovolný text, matematické vzorce, vstupní kódu, výsledky, grafiky, videa a jakýkoli jiný druh média, jestli je schopná zobrazit moderní webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="4b328-105">These notebook files can contain arbitrary text, mathematical formulas, input code, results, graphics, videos and any other kind of media that a modern web browser is capable of displaying.</span></span> <span data-ttu-id="4b328-106">Jestli jste absolutně nové tooPython a chcete toolearn ho v fun, interaktivní prostředí nebo proveďte některé závažné paralelní a technické computing, hello Poznámkový blok Jupyter je je služba skvělou volbou.</span><span class="sxs-lookup"><span data-stu-id="4b328-106">Whether you're absolutely new tooPython and want toolearn it in a fun, interactive environment or do some serious parallel/technical computing, hello Jupyter Notebook is a great choice.</span></span>

<span data-ttu-id="4b328-107">![Snímek obrazovky](./media/jupyter-notebook/ipy-notebook-spectral.png) pomocí SciPy a Matplotlib balíčky tooanalyze hello struktura záznamu zvuku.</span><span class="sxs-lookup"><span data-stu-id="4b328-107">![Screenshot](./media/jupyter-notebook/ipy-notebook-spectral.png) Using SciPy and Matplotlib packages tooanalyze hello structure of a sound recording.</span></span>

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a><span data-ttu-id="4b328-108">Jupyter dvěma způsoby: Vlastní nasazení nebo Azure poznámkové bloky</span><span class="sxs-lookup"><span data-stu-id="4b328-108">Jupyter Two Ways: Azure Notebooks or Custom Deployment</span></span>
<span data-ttu-id="4b328-109">Azure poskytuje službu, kterou můžete použít příliš[rychle začít používat Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b328-109">Azure provides a service that you can use too[quickly start using Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span></span>  <span data-ttu-id="4b328-110">Pomocí hello Azure Service Poznámkový blok, můžete snadno získat přístup tooJupyter přístupné na webu rozhraní k škálovatelný výpočetní prostředky s všechny power hello jazyka Python a jeho mnoho knihovny.</span><span class="sxs-lookup"><span data-stu-id="4b328-110">By using hello Azure Notebook Service, you can easily gain access tooJupyter's web-accessible interface to scalable computational resources with all hello power of Python and its many libraries.</span></span>  <span data-ttu-id="4b328-111">Vzhledem k tomu, že instalace hello se proto službou hello, uživatelé můžou používat tyto prostředky bez hello potřeba pro správu a konfiguraci uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-111">Since hello installation is handled by hello service, users can access these resources without hello need for administration and configuration by hello user.</span></span>

<span data-ttu-id="4b328-112">Pokud služba hello Poznámkový blok pro váš scénář nefunguje. Pokračujte prosím tooread tohoto článku, které se vám ukáže jak toodeploy hello Poznámkový blok Jupyter v Microsoft Azure, pomocí Linuxové virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="4b328-112">If hello notebook service does not work for your scenario please continue tooread this article which will will show you how toodeploy hello Jupyter Notebook on Microsoft Azure, using Linux virtual machines (VMs).</span></span>

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a><span data-ttu-id="4b328-113">Vytvoření a konfigurace virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="4b328-113">Create and configure a VM on Azure</span></span>
<span data-ttu-id="4b328-114">prvním krokem Hello je toocreate virtuální počítač (VM) spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="4b328-114">hello first step is toocreate a virtual machine (VM)  running on Azure.</span></span>
<span data-ttu-id="4b328-115">Tento virtuální počítač je v cloudu hello úplný operační systém a se použije ke spuštění hello poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4b328-115">This VM is a complete operating system in hello cloud and will be used to run hello Jupyter Notebook.</span></span> <span data-ttu-id="4b328-116">Je schopen spuštění virtuálních počítačů Linux a Windows Azure a vám nabídneme hello nastavení Jupyter na oba typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4b328-116">Azure is capable of running both Linux and Windows virtual machines, and we will cover hello setup of Jupyter on both types of virtual machines.</span></span>

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a><span data-ttu-id="4b328-117">Vytvoření virtuálního počítače s Linuxem a otevře port pro Jupyter</span><span class="sxs-lookup"><span data-stu-id="4b328-117">Create a Linux VM and open a port for Jupyter</span></span>
<span data-ttu-id="4b328-118">Postupujte podle pokynů hello [sem] [ portal-vm-linux] toocreate virtuální počítač hello *Ubuntu* distribuce.</span><span class="sxs-lookup"><span data-stu-id="4b328-118">Follow hello instructions given [here][portal-vm-linux] toocreate a virtual machine of hello *Ubuntu* distribution.</span></span> <span data-ttu-id="4b328-119">Tento kurz používá Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="4b328-119">This tutorial uses Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="4b328-120">Budeme předpokládat hello uživatelské jméno *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="4b328-120">We'll assume hello user name *azureuser*.</span></span>

<span data-ttu-id="4b328-121">Po hello virtuální počítač nasadí potřebujeme tooopen až pravidlo zabezpečení na skupinu zabezpečení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-121">After hello virtual machine deploys we need tooopen up a security rule on hello network security group.</span></span>  <span data-ttu-id="4b328-122">Z hello portálu Azure, přejděte příliš**skupin zabezpečení sítě** a otevřete hello karta hello skupiny zabezpečení odpovídající tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4b328-122">From hello Azure portal, go too**Network Security Groups** and open hello tab for hello Security Group corresponding tooyour VM.</span></span> <span data-ttu-id="4b328-123">Je nutné tooadd pravidlo příchozí zabezpečení s hello následující nastavení: **TCP** pro protokol hello  **\***  pro port hello zdroje (veřejných) a **9999** pro Hello cílový (soukromý) port.</span><span class="sxs-lookup"><span data-stu-id="4b328-123">You need tooadd an Inbound Security rule with hello following settings: **TCP** for hello protocol, **\*** for hello source (public) port and **9999** for hello destination (private) port.</span></span>

![snímek obrazovky](./media/jupyter-notebook/azure-add-endpoint.png)

<span data-ttu-id="4b328-125">Když ve skupině zabezpečení sítě, klikněte na **síťových rozhraní** a Poznámka hello **veřejnou IP adresu** podle potřeby tooconnect tooyour virtuálních počítačů bude v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-125">While in your Network Security Group, click on **Network Interfaces** and note hello **Public IP Address** as it will be needed tooconnect tooyour VM in hello next step.</span></span>

## <a name="install-required-software-on-hello-vm"></a><span data-ttu-id="4b328-126">Nainstalujte požadovaný software na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4b328-126">Install required software on hello VM</span></span>
<span data-ttu-id="4b328-127">toorun hello Poznámkový blok Jupyter naše virtuálního počítače, nám nejdřív nainstalovat Jupyter a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="4b328-127">toorun hello Jupyter Notebook on our VM, we must first install Jupyter and its dependencies.</span></span> <span data-ttu-id="4b328-128">Připojte virtuální počítač s linuxem tooyour pomocí ssh a hello pár hello uživatelské jméno a heslo jste si zvolili při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4b328-128">Connect tooyour linux vm using ssh and hello username/password pair you chose when you created hello vm.</span></span> <span data-ttu-id="4b328-129">V tomto kurzu jsme použití klienta PuTTY a připojení ze systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4b328-129">In this tutorial we will use PuTTY and connect from Windows.</span></span>

### <a name="installing-jupyter-on-ubuntu"></a><span data-ttu-id="4b328-130">Instalace na Ubuntu Jupyter</span><span class="sxs-lookup"><span data-stu-id="4b328-130">Installing Jupyter on Ubuntu</span></span>
<span data-ttu-id="4b328-131">Nainstalujte Anacondu, distribuci jazyka python vědecké účely oblíbených data, pomocí některého z odkazů hello uvedených z [poměrně Analytics](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="4b328-131">Install Anaconda, a popular data science python distribution, using one of hello links provided from [Continuum Analytics](https://www.continuum.io/downloads).</span></span>  <span data-ttu-id="4b328-132">Od verze hello zápis tohoto dokumentu, jsou hello pod odkazy hello nejvíce až toodate verze.</span><span class="sxs-lookup"><span data-stu-id="4b328-132">As of hello writing of this document, hello below links are hello most up toodate versions.</span></span>

#### <a name="anaconda-installs-for-linux"></a><span data-ttu-id="4b328-133">Nainstaluje anaconda pro Linux</span><span class="sxs-lookup"><span data-stu-id="4b328-133">Anaconda Installs for Linux</span></span>
<table>
  <th><span data-ttu-id="4b328-134">Python 3.4</span><span class="sxs-lookup"><span data-stu-id="4b328-134">Python 3.4</span></span></th>
  <th><span data-ttu-id="4b328-135">Python 2.7</span><span class="sxs-lookup"><span data-stu-id="4b328-135">Python 2.7</span></span></th>
  <tr>
    <td><span data-ttu-id="4b328-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64bitová verze</href>
    </span><span class="sxs-lookup"><span data-stu-id="4b328-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
    <td><span data-ttu-id="4b328-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64bitová verze</href>
    </span><span class="sxs-lookup"><span data-stu-id="4b328-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="4b328-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32bitové</href>
    </span><span class="sxs-lookup"><span data-stu-id="4b328-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>
    <td><span data-ttu-id="4b328-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32bitové</href>
    </span><span class="sxs-lookup"><span data-stu-id="4b328-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a><span data-ttu-id="4b328-140">Instalace Anaconda3 2.3.0 64-bit na Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4b328-140">Installing Anaconda3 2.3.0 64-bit on Ubuntu</span></span>
<span data-ttu-id="4b328-141">Jako příklad to je, jak můžete nainstalovat Anaconda Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4b328-141">As an example, this is how you can install Anaconda on Ubuntu</span></span>

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

### <a name="configuring-jupyter-and-using-ssl"></a><span data-ttu-id="4b328-143">Konfigurace Jupyter a použití protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="4b328-143">Configuring Jupyter and using SSL</span></span>
<span data-ttu-id="4b328-144">Po instalaci potřebujeme tootake chvíli toosetup hello konfigurační soubory pro Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4b328-144">After installing we need tootake a moment toosetup hello configuration files for Jupyter.</span></span> <span data-ttu-id="4b328-145">Pokud máte potíže při konfiguraci Jupyter může být užitečné toolook v hello [serverem poznámkového bloku Jupyter dokumentaci](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span><span class="sxs-lookup"><span data-stu-id="4b328-145">If you experience troubles with configuring Jupyter it may be helpful toolook at hello [Jupyter Documentation for Running a Notebook Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span></span>

<span data-ttu-id="4b328-146">Další jsme `cd` toohello profilu directory toocreate naše certifikát SSL a upravovat soubor konfigurace profilů hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-146">Next we `cd` toohello profile directory toocreate our SSL certificate and edit hello profiles configuration file.</span></span>

<span data-ttu-id="4b328-147">V systému Linux použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-147">On Linux use hello following command.</span></span>

    cd ~/.jupyter

<span data-ttu-id="4b328-148">Použijte následující příkaz toocreate hello certifikát SSL (Linux a Windows) hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-148">Use hello following command toocreate hello SSL certificate(Linux and Windows).</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="4b328-149">Všimněte si, že vzhledem k tomu, že se vytváří certifikát podepsaný svým držitelem SSL při připojování toohello Poznámkový blok prohlížeče získáte upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4b328-149">Note that since we are creating a self-signed SSL certificate, when connecting toohello notebook your browser will give you a security warning.</span></span>  <span data-ttu-id="4b328-150">Pro dlouhodobou produkční použití budete chtít toouse správně podepsaný držitelem přidružené k vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="4b328-150">For long-term production use, you will want toouse a properly signed certificate associated with your organization.</span></span>  <span data-ttu-id="4b328-151">Vzhledem k tomu, že certifikát správy je mimo rozsah hello tuto ukázku, jsme se teď přilepit tooa certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="4b328-151">Since certificate management is beyond hello scope of this demo, we will stick tooa self-signed certificate for now.</span></span>

<span data-ttu-id="4b328-152">Kromě toho toousing certifikát, je taky nutné zadat heslo tooprotect Poznámkový blok před neoprávněným použitím.</span><span class="sxs-lookup"><span data-stu-id="4b328-152">In addition toousing a certificate, you must also provide a password tooprotect your notebook from unauthorized use.</span></span>  <span data-ttu-id="4b328-153">Z bezpečnostních důvodů Jupyter používá šifrovaných hesel v konfiguračním souboru, takže budete potřebovat tooencrypt heslo nejprve.</span><span class="sxs-lookup"><span data-stu-id="4b328-153">For security reasons Jupyter uses encrypted passwords in its configuration file, so you'll need tooencrypt your password first.</span></span>  <span data-ttu-id="4b328-154">IPython poskytuje nástroj toodo tak; na příkazovém řádku spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-154">IPython provides a utility toodo so; at a command prompt run hello following command.</span></span>

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

<span data-ttu-id="4b328-155">Zobrazí výzvu k zadání heslo a potvrzení a bude poté vytiskněte hello heslo.</span><span class="sxs-lookup"><span data-stu-id="4b328-155">This will prompt you for a password and confirmation, and will then print hello password.</span></span> <span data-ttu-id="4b328-156">Poznámka: pro hello následující krok.</span><span class="sxs-lookup"><span data-stu-id="4b328-156">Note this for hello following step.</span></span>

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided hello rest for security)

<span data-ttu-id="4b328-157">V dalším kroku upravíme hello profil konfigurační soubor, který je `jupyter_notebook_config.py` soubor v adresáři hello v.</span><span class="sxs-lookup"><span data-stu-id="4b328-157">Next, we will edit hello profile's configuration file, which is the `jupyter_notebook_config.py` file in hello directory you are in.</span></span>  <span data-ttu-id="4b328-158">Všimněte si, že tento soubor neexistuje – stačí vytvořit případě hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-158">Note that this file may not exist -- just create it if that is hello case.</span></span>  <span data-ttu-id="4b328-159">Tento soubor má počet polí a ve výchozím nastavení všechny jsou vloženy do komentáře.  Tento soubor můžete otevřít pomocí libovolného textového editoru z libovolně a měli byste zajistit, že má aspoň hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="4b328-159">This file has a number of fields and by default all are commented out.  You can open this file with any text editor of your liking, and you should ensure that it has at least hello following content.</span></span> <span data-ttu-id="4b328-160">**Být jisti c.NotebookApp.password hello tooreplace v konfiguračním hello s hello sha1 hello v předchozím kroku**.</span><span class="sxs-lookup"><span data-stu-id="4b328-160">**Be sure tooreplace hello c.NotebookApp.password in hello config with hello sha1 from hello previous step**.</span></span>

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

### <a name="run-hello-jupyter-notebook"></a><span data-ttu-id="4b328-161">Spustit hello poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="4b328-161">Run hello Jupyter Notebook</span></span>
<span data-ttu-id="4b328-162">V tomto okamžiku se připravené toostart hello poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4b328-162">At this point we are ready toostart hello Jupyter Notebook.</span></span> <span data-ttu-id="4b328-163">toodo, přejděte toohello directory chcete toostore poznámkových bloků a začínat hello Poznámkový blok Jupyter server hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4b328-163">toodo this, navigate toohello directory you want toostore notebooks in and start hello Jupyter Notebook server with hello following command.</span></span>

    /anaconda3/bin/jupyter-notebook

<span data-ttu-id="4b328-164">Nyní byste měli být schopný tooaccess Poznámkový blok Jupyter na adrese hello `https://[PUBLIC-IP-ADDRESS]:9999`.</span><span class="sxs-lookup"><span data-stu-id="4b328-164">You should now be able tooaccess your Jupyter Notebook at hello address `https://[PUBLIC-IP-ADDRESS]:9999`.</span></span>

<span data-ttu-id="4b328-165">Pokud nejprve přístup k Poznámkový blok, požádá přihlašovací stránku hello k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="4b328-165">When you first access your notebook, hello login page asks for your password.</span></span> <span data-ttu-id="4b328-166">A jakmile se přihlásíte, zobrazí se vám hello "Jupyter poznámkového bloku řídicího panelu", který je obrazit řídicí centrum pro všechny operace poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="4b328-166">And once you log in, you will see hello "Jupyter Notebook Dashboard", which is the ontrol center for all notebook operations.</span></span>  <span data-ttu-id="4b328-167">Z této stránky můžete vytvořit nové poznámkových bloků a otevřít existující.</span><span class="sxs-lookup"><span data-stu-id="4b328-167">From this page you can create new notebooks and open existing ones.</span></span>

![snímek obrazovky](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-hello-jupyter-notebook"></a><span data-ttu-id="4b328-169">Pomocí hello poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="4b328-169">Using hello Jupyter Notebook</span></span>
<span data-ttu-id="4b328-170">Pokud kliknete na tlačítko hello **nový** tlačítko, zobrazí se následující úvodní stránce hello.</span><span class="sxs-lookup"><span data-stu-id="4b328-170">If you click hello **New** button, you will see hello following opening page.</span></span>

![snímek obrazovky](./media/jupyter-notebook/jupyter-untitled-notebook.png)

<span data-ttu-id="4b328-172">Hello oblasti označené `In []:` řádku je hello vstupní oblast a zde je možné zadat libovolný platný kód Python a provede při zásahu `Shift-Enter` nebo klikněte na ikonu "Přehrání" hello (hello trojúhelník směřující vpravo v panelu nástrojů hello).</span><span class="sxs-lookup"><span data-stu-id="4b328-172">hello area marked with an `In []:` prompt is hello input area, and here you can type any valid Python code and it will execute when you hit `Shift-Enter` or click on hello "Play" icon (hello right-pointing triangle in hello toolbar).</span></span>

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a><span data-ttu-id="4b328-173">Efektivní zlepší: výpočetní dokumentů s bohatý mediální za provozu</span><span class="sxs-lookup"><span data-stu-id="4b328-173">A powerful paradigm: live computational documents with rich media</span></span>
<span data-ttu-id="4b328-174">Poznámkový blok Hello samotné by měli považovat velmi přirozené tooanyone, který se používá Python a textový editor, protože je v některých způsobech kombinaci obou: můžete spustit bloky kódu jazyka Python, ale můžete také ponechat poznámky a další text příliš změnou styl buněk z "Kód" hello "Ma rkdown"hello rozevírací nabídky v panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="4b328-174">hello notebook itself should feel very natural tooanyone who has used Python and a word processor, because it is in some ways a mix of both: you can execute blocks of Python code, but you can also keep notes and other text by changing hello style of a cell from "Code" too"Markdown" using hello drop-down menu in the toolbar.</span></span>

<span data-ttu-id="4b328-175">Jupyter je mnohem víc než textový editor jako umožňuje směšování výpočtů a bohaté média (text, grafiky, videa a prakticky nic, co můžete zobrazit moderní webový prohlížeč).</span><span class="sxs-lookup"><span data-stu-id="4b328-175">Jupyter is much more than a word processor as it allows the mixing of computation and rich media (text, graphics, video and virtually anything a modern web browser can display).</span></span> <span data-ttu-id="4b328-176">Je možné kombinovat, text, kód, videa a další!</span><span class="sxs-lookup"><span data-stu-id="4b328-176">You can mix, text, code, videos and more!</span></span>

![snímek obrazovky](./media/jupyter-notebook/jupyter-editing-experience.png)

<span data-ttu-id="4b328-178">A s výkonem hello mnoho vynikající knihoven jazyka Python pro technické a vědecké výpočty v hello následující snímek obrazovky, lze provést jednoduchý výpočet hello stejné usnadňují jako komplexní sítě analýzy, všechny v jednom prostředí.</span><span class="sxs-lookup"><span data-stu-id="4b328-178">And with hello power of Python's many excellent libraries for scientific and technical computing, in hello following screenshot, a simple calculation can be performed with hello same ease as a complex network analysis, all in one environment.</span></span>

<span data-ttu-id="4b328-179">Tato zlepší promíchání hello power moderních webových hello s za provozu výpočetní nabízí mnoho možností a je ideální pro hello cloud; dá se Hello Poznámkový blok:</span><span class="sxs-lookup"><span data-stu-id="4b328-179">This paradigm of mixing hello power of hello modern web with live computation offers many possibilities, and is ideally suited for hello cloud; hello Notebook can be used:</span></span>

* <span data-ttu-id="4b328-180">Jako výpočetní zápisník toorecord nahodilého fungovat na problém.</span><span class="sxs-lookup"><span data-stu-id="4b328-180">As a computational scratchpad toorecord exploratory work on a problem.</span></span>
* <span data-ttu-id="4b328-181">tooshare výsledků s kolegy v 'za provozu, výpočetní formuláře nebo v výtisk formátů (HTML, PDF).</span><span class="sxs-lookup"><span data-stu-id="4b328-181">tooshare results with colleagues, either in 'live' computational form or in hardcopy formats (HTML, PDF).</span></span>
* <span data-ttu-id="4b328-182">toodistribute a přítomen za provozu vyučující materiálů, které se týkají výpočtů, takže studenty okamžitě můžete experimentovat s hello skutečné kód, upravte ji a znovu spustit interaktivně.</span><span class="sxs-lookup"><span data-stu-id="4b328-182">toodistribute and present live teaching materials that involve computation, so students can immediately experiment with hello real code, modify it and re-execute it interactively.</span></span>
* <span data-ttu-id="4b328-183">tooprovide "spustitelné papír", která představují hello výsledky research způsobem, který může být okamžitě opakuje, ověřit a rozšířené třetími stranami.</span><span class="sxs-lookup"><span data-stu-id="4b328-183">tooprovide "executable papers" that present hello results of research in a way that can be immediately reproduced, validated and extended by others.</span></span>
* <span data-ttu-id="4b328-184">Jako platforma pro spolupráci výpočty: více mohou uživatelů přihlásit toothe stejný server poznámkového bloku tooshare výpočetní relaci za provozu.</span><span class="sxs-lookup"><span data-stu-id="4b328-184">As a platform for collaborative computing: multiple users can log in toothe same notebook server tooshare a live computational session.</span></span>

<span data-ttu-id="4b328-185">Pokud přejdete toohello IPython zdrojového kódu [úložiště][repository], zjistíte, celého adresáře s příklady Poznámkový blok, které můžete stáhnout a potom experimentovat s na své vlastní virtuální počítač Azure Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4b328-185">If you go toohello IPython source code [repository][repository], you will find an entire directory with notebook examples which you can download and then experiment with on your own Azure Jupyter VM.</span></span>  <span data-ttu-id="4b328-186">Jednoduše stáhnout hello `.ipynb` soubory z hello lokality a odešlete do řídicího panelu hello v poznámkovém bloku virtuální počítač Azure (nebo je stáhnout přímo do hello virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="4b328-186">Simply download hello `.ipynb` files from hello site and upload them onto hello dashboard of your notebook Azure VM (or download them directly into hello VM).</span></span>

## <a name="conclusion"></a><span data-ttu-id="4b328-187">Závěr</span><span class="sxs-lookup"><span data-stu-id="4b328-187">Conclusion</span></span>
<span data-ttu-id="4b328-188">Hello Poznámkový blok Jupyter poskytuje výkonné rozhraní pro přístup k interaktivním hello power ekosystému hello Python v Azure.</span><span class="sxs-lookup"><span data-stu-id="4b328-188">hello Jupyter Notebook provides a powerful interface for accessing interactively hello power of hello Python ecosystem on Azure.</span></span>  <span data-ttu-id="4b328-189">Pokrývá širokou škálu případy využití, včetně jednoduchého zkoumání a učení Python, analýzy dat a vizualizaci, simulace a paralelní výpočty.</span><span class="sxs-lookup"><span data-stu-id="4b328-189">It covers a wide range of usage cases including simple exploration and learning Python, data analysis and visualization, simulation and parallel computing.</span></span> <span data-ttu-id="4b328-190">Hello výsledné poznámkového bloku dokumenty obsahují úplný záznam hello výpočtů, u kterých se provádějí a je možné sdílet s ostatními uživateli Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4b328-190">hello resulting Notebook documents contain a complete record of hello computations that are performed and can be shared with other Jupyter users.</span></span>  <span data-ttu-id="4b328-191">Hello Poznámkový blok Jupyter lze použít jako místní aplikace, ale jeho je ideální pro nasazení cloudu v Azure</span><span class="sxs-lookup"><span data-stu-id="4b328-191">hello Jupyter Notebook can be used as a local application, but it is ideally suited for cloud deployments on Azure</span></span>

<span data-ttu-id="4b328-192">základní funkce Hello Jupyter jsou také k dispozici v sadě Visual Studio prostřednictvím [Python Tools pro Visual Studio] [ Python Tools for Visual Studio] (PTVS).</span><span class="sxs-lookup"><span data-stu-id="4b328-192">hello core features of Jupyter are also available inside Visual Studio via the [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS).</span></span> <span data-ttu-id="4b328-193">PTVS je bezplatný a open source od společnosti Microsoft, který profilování a paralelní se změní rozšířené vývoj prostředí Python, které obsahuje editoru upřesnit pomocí IntelliSense, ladění, Visual Studio modulu plug-in computing integrace.</span><span class="sxs-lookup"><span data-stu-id="4b328-193">PTVS is a free and open-source plug-in from Microsoft that turns Visual Studio into an advanced Python development environment that includes an advanced editor with IntelliSense, debugging, profiling and parallel computing integration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b328-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b328-194">Next steps</span></span>
<span data-ttu-id="4b328-195">Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="4b328-195">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
