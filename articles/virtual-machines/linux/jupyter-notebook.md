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
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a><span data-ttu-id="d5d19-103">Vytváření virtuálního počítače Azure, instalaci Jupyter a systémem Azure poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="d5d19-103">Creating an Azure VM, installing Jupyter, and running a Jupyter Notebook on Azure</span></span>
<span data-ttu-id="d5d19-104">[Jupyter projektu](http://jupyter.org), dříve [IPython projektu](http://ipython.org), poskytuje kolekci nástrojů pro vědecké výpočty pomocí výkonné interaktivní shells aplikací, které spojují provádění kódu s vytváření živé výpočetní dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d5d19-104">The [Jupyter project](http://jupyter.org), formerly the [IPython project](http://ipython.org), provides a collection of tools for scientific computing using powerful interactive shells that combine code execution with the creation of a live computational document.</span></span> <span data-ttu-id="d5d19-105">Tyto soubory poznámkového bloku může obsahovat libovolný text, matematické vzorce, vstupní kódu, výsledky, grafiky, videa a jakýkoli jiný druh média, jestli je schopná zobrazit moderní webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="d5d19-105">These notebook files can contain arbitrary text, mathematical formulas, input code, results, graphics, videos and any other kind of media that a modern web browser is capable of displaying.</span></span> <span data-ttu-id="d5d19-106">Jestli jste absolutně nové Python a chcete další v prostředí zábavné, interaktivní nebo provést některé závažné paralelní a technické computing, je poznámkového bloku Jupyter je služba skvělou volbou.</span><span class="sxs-lookup"><span data-stu-id="d5d19-106">Whether you're absolutely new to Python and want to learn it in a fun, interactive environment or do some serious parallel/technical computing, the Jupyter Notebook is a great choice.</span></span>

<span data-ttu-id="d5d19-107">![Snímek obrazovky](./media/jupyter-notebook/ipy-notebook-spectral.png) pomocí SciPy a Matplotlib balíčků struktura záznamu zvuku.</span><span class="sxs-lookup"><span data-stu-id="d5d19-107">![Screenshot](./media/jupyter-notebook/ipy-notebook-spectral.png) Using SciPy and Matplotlib packages to analyze the structure of a sound recording.</span></span>

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a><span data-ttu-id="d5d19-108">Jupyter dvěma způsoby: Vlastní nasazení nebo Azure poznámkové bloky</span><span class="sxs-lookup"><span data-stu-id="d5d19-108">Jupyter Two Ways: Azure Notebooks or Custom Deployment</span></span>
<span data-ttu-id="d5d19-109">Azure poskytuje službu, která vám pomůže [rychle začít používat Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5d19-109">Azure provides a service that you can use to [quickly start using Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span></span>  <span data-ttu-id="d5d19-110">Pomocí poznámkového bloku služby Azure, můžete snadno získat přístup k rozhraní přístupné na webu na Jupyter pro škálovatelný výpočetní prostředky všechny sílu jazyka Python a jeho mnoho knihovny.</span><span class="sxs-lookup"><span data-stu-id="d5d19-110">By using the Azure Notebook Service, you can easily gain access to Jupyter's web-accessible interface to scalable computational resources with all the power of Python and its many libraries.</span></span>  <span data-ttu-id="d5d19-111">Vzhledem k tomu, že instalace používá služba, uživatelé můžou používat tyto prostředky bez nutnosti pro správu a konfiguraci uživatelem.</span><span class="sxs-lookup"><span data-stu-id="d5d19-111">Since the installation is handled by the service, users can access these resources without the need for administration and configuration by the user.</span></span>

<span data-ttu-id="d5d19-112">Pokud službu poznámkového bloku nefunguje pro váš scénář pokračujte ke čtení tohoto článku, které se vám ukáže, jak nasadit poznámkového bloku Jupyter v Microsoft Azure, pomocí Linuxové virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="d5d19-112">If the notebook service does not work for your scenario please continue to read this article which will will show you how to deploy the Jupyter Notebook on Microsoft Azure, using Linux virtual machines (VMs).</span></span>

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a><span data-ttu-id="d5d19-113">Vytvoření a konfigurace virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="d5d19-113">Create and configure a VM on Azure</span></span>
<span data-ttu-id="d5d19-114">Prvním krokem je vytvoření virtuálního počítače (VM) spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="d5d19-114">The first step is to create a virtual machine (VM)  running on Azure.</span></span>
<span data-ttu-id="d5d19-115">Tento virtuální počítač je úplný operační systém v cloudu a se použije ke spuštění poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d5d19-115">This VM is a complete operating system in the cloud and will be used to run the Jupyter Notebook.</span></span> <span data-ttu-id="d5d19-116">Je schopen spuštění virtuálních počítačů Linux a Windows Azure a vám nabídneme nastavení Jupyter na oba typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5d19-116">Azure is capable of running both Linux and Windows virtual machines, and we will cover the setup of Jupyter on both types of virtual machines.</span></span>

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a><span data-ttu-id="d5d19-117">Vytvoření virtuálního počítače s Linuxem a otevře port pro Jupyter</span><span class="sxs-lookup"><span data-stu-id="d5d19-117">Create a Linux VM and open a port for Jupyter</span></span>
<span data-ttu-id="d5d19-118">Postupujte podle pokynů, [sem] [ portal-vm-linux] k vytvoření virtuálního počítače *Ubuntu* distribuce.</span><span class="sxs-lookup"><span data-stu-id="d5d19-118">Follow the instructions given [here][portal-vm-linux] to create a virtual machine of the *Ubuntu* distribution.</span></span> <span data-ttu-id="d5d19-119">Tento kurz používá Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="d5d19-119">This tutorial uses Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="d5d19-120">Budeme předpokládat uživatelské jméno *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="d5d19-120">We'll assume the user name *azureuser*.</span></span>

<span data-ttu-id="d5d19-121">Po nasazení virtuálního počítače je potřeba otevře pravidlo zabezpečení na skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d5d19-121">After the virtual machine deploys we need to open up a security rule on the network security group.</span></span>  <span data-ttu-id="d5d19-122">Z portálu Azure, přejděte na **skupin zabezpečení sítě** a otevřete kartu pro skupinu zabezpečení odpovídající virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d5d19-122">From the Azure portal, go to **Network Security Groups** and open the tab for the Security Group corresponding to your VM.</span></span> <span data-ttu-id="d5d19-123">Je nutné přidat pravidlo příchozí zabezpečení s následujícím nastavením: **TCP** pro protokol  **\***  pro zdrojový (veřejných) port a **9999** pro cílový (soukromý) port.</span><span class="sxs-lookup"><span data-stu-id="d5d19-123">You need to add an Inbound Security rule with the following settings: **TCP** for the protocol, **\*** for the source (public) port and **9999** for the destination (private) port.</span></span>

![snímek obrazovky](./media/jupyter-notebook/azure-add-endpoint.png)

<span data-ttu-id="d5d19-125">Když ve skupině zabezpečení sítě, klikněte na **síťových rozhraní** a poznamenejte si **veřejnou IP adresu** jako bude potřeba pro připojení k virtuálnímu počítači v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d5d19-125">While in your Network Security Group, click on **Network Interfaces** and note the **Public IP Address** as it will be needed to connect to your VM in the next step.</span></span>

## <a name="install-required-software-on-the-vm"></a><span data-ttu-id="d5d19-126">Do virtuálního počítače nainstalujte požadovaný software</span><span class="sxs-lookup"><span data-stu-id="d5d19-126">Install required software on the VM</span></span>
<span data-ttu-id="d5d19-127">Pro spouštění poznámkového bloku Jupyter v našem virtuálních počítačů, jsme musíte nejprve nainstalovat Jupyter a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="d5d19-127">To run the Jupyter Notebook on our VM, we must first install Jupyter and its dependencies.</span></span> <span data-ttu-id="d5d19-128">Připojte se k linux virtuálního počítače pomocí ssh a uživatelského jména a hesla spárujte jste zvolili při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5d19-128">Connect to your linux vm using ssh and the username/password pair you chose when you created the vm.</span></span> <span data-ttu-id="d5d19-129">V tomto kurzu jsme použití klienta PuTTY a připojení ze systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d5d19-129">In this tutorial we will use PuTTY and connect from Windows.</span></span>

### <a name="installing-jupyter-on-ubuntu"></a><span data-ttu-id="d5d19-130">Instalace na Ubuntu Jupyter</span><span class="sxs-lookup"><span data-stu-id="d5d19-130">Installing Jupyter on Ubuntu</span></span>
<span data-ttu-id="d5d19-131">Nainstalujte Anacondu, distribuci jazyka python vědecké účely oblíbených data, pomocí některého z odkazů uvedených z [poměrně Analytics](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="d5d19-131">Install Anaconda, a popular data science python distribution, using one of the links provided from [Continuum Analytics](https://www.continuum.io/downloads).</span></span>  <span data-ttu-id="d5d19-132">Před sepsáním tohoto dokumentu, níž odkazy jsou nejaktuálnější verze datum.</span><span class="sxs-lookup"><span data-stu-id="d5d19-132">As of the writing of this document, the below links are the most up to date versions.</span></span>

#### <a name="anaconda-installs-for-linux"></a><span data-ttu-id="d5d19-133">Nainstaluje anaconda pro Linux</span><span class="sxs-lookup"><span data-stu-id="d5d19-133">Anaconda Installs for Linux</span></span>
<table>
  <th><span data-ttu-id="d5d19-134">Python 3.4</span><span class="sxs-lookup"><span data-stu-id="d5d19-134">Python 3.4</span></span></th>
  <th><span data-ttu-id="d5d19-135">Python 2.7</span><span class="sxs-lookup"><span data-stu-id="d5d19-135">Python 2.7</span></span></th>
  <tr>
    <td><span data-ttu-id="d5d19-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64bitová verze</href>
    </span><span class="sxs-lookup"><span data-stu-id="d5d19-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
    <td><span data-ttu-id="d5d19-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64bitová verze</href>
    </span><span class="sxs-lookup"><span data-stu-id="d5d19-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="d5d19-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32bitové</href>
    </span><span class="sxs-lookup"><span data-stu-id="d5d19-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>
    <td><span data-ttu-id="d5d19-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32bitové</href>
    </span><span class="sxs-lookup"><span data-stu-id="d5d19-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a><span data-ttu-id="d5d19-140">Instalace Anaconda3 2.3.0 64-bit na Ubuntu</span><span class="sxs-lookup"><span data-stu-id="d5d19-140">Installing Anaconda3 2.3.0 64-bit on Ubuntu</span></span>
<span data-ttu-id="d5d19-141">Jako příklad to je, jak můžete nainstalovat Anaconda Ubuntu</span><span class="sxs-lookup"><span data-stu-id="d5d19-141">As an example, this is how you can install Anaconda on Ubuntu</span></span>

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

### <a name="configuring-jupyter-and-using-ssl"></a><span data-ttu-id="d5d19-143">Konfigurace Jupyter a použití protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="d5d19-143">Configuring Jupyter and using SSL</span></span>
<span data-ttu-id="d5d19-144">Po instalaci je potřeba nastavit konfigurační soubory pro Jupyter chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="d5d19-144">After installing we need to take a moment to setup the configuration files for Jupyter.</span></span> <span data-ttu-id="d5d19-145">Pokud máte potíže při konfiguraci Jupyter může být užitečné prohlédnout si [serverem poznámkového bloku Jupyter dokumentaci](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span><span class="sxs-lookup"><span data-stu-id="d5d19-145">If you experience troubles with configuring Jupyter it may be helpful to look at the [Jupyter Documentation for Running a Notebook Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span></span>

<span data-ttu-id="d5d19-146">Další jsme `cd` k adresáři profil vytvořit certifikát SSL a upravit profily konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="d5d19-146">Next we `cd` to the profile directory to create our SSL certificate and edit the profiles configuration file.</span></span>

<span data-ttu-id="d5d19-147">V systému Linux použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d5d19-147">On Linux use the following command.</span></span>

    cd ~/.jupyter

<span data-ttu-id="d5d19-148">Použijte následující příkaz k vytvoření certifikátu protokolu SSL (Linux a Windows).</span><span class="sxs-lookup"><span data-stu-id="d5d19-148">Use the following command to create the SSL certificate(Linux and Windows).</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="d5d19-149">Pamatujte, že vzhledem k tomu, že se vytváří certifikát podepsaný svým držitelem SSL při připojování k poznámkového bloku prohlížeče vám poskytne upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d5d19-149">Note that since we are creating a self-signed SSL certificate, when connecting to the notebook your browser will give you a security warning.</span></span>  <span data-ttu-id="d5d19-150">Pro dlouhodobou produkční použití budete chtít použít certifikát správně podepsaný přidružené k vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="d5d19-150">For long-term production use, you will want to use a properly signed certificate associated with your organization.</span></span>  <span data-ttu-id="d5d19-151">Vzhledem k tomu, že certifikát správy je nad rámec tuto ukázku, jsme se na certifikát podepsaný svým držitelem přilepit teď.</span><span class="sxs-lookup"><span data-stu-id="d5d19-151">Since certificate management is beyond the scope of this demo, we will stick to a self-signed certificate for now.</span></span>

<span data-ttu-id="d5d19-152">Kromě používá certifikát, musíte také zadat heslo k ochraně Poznámkový blok před neoprávněným použitím.</span><span class="sxs-lookup"><span data-stu-id="d5d19-152">In addition to using a certificate, you must also provide a password to protect your notebook from unauthorized use.</span></span>  <span data-ttu-id="d5d19-153">Z bezpečnostních důvodů Jupyter používá šifrované hesla v konfiguračním souboru, takže budete muset heslo šifrování nejprve.</span><span class="sxs-lookup"><span data-stu-id="d5d19-153">For security reasons Jupyter uses encrypted passwords in its configuration file, so you'll need to encrypt your password first.</span></span>  <span data-ttu-id="d5d19-154">IPython poskytuje nástroj k tomu; na příkazovém řádku spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d5d19-154">IPython provides a utility to do so; at a command prompt run the following command.</span></span>

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

<span data-ttu-id="d5d19-155">Zobrazí výzvu k zadání heslo a potvrzení a bude poté vytiskněte heslo.</span><span class="sxs-lookup"><span data-stu-id="d5d19-155">This will prompt you for a password and confirmation, and will then print the password.</span></span> <span data-ttu-id="d5d19-156">Poznámka: pro následující krok.</span><span class="sxs-lookup"><span data-stu-id="d5d19-156">Note this for the following step.</span></span>

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

<span data-ttu-id="d5d19-157">Potom jsme upravte konfigurační soubor pro tento profil, který je `jupyter_notebook_config.py` soubor v adresáři, jsou v.</span><span class="sxs-lookup"><span data-stu-id="d5d19-157">Next, we will edit the profile's configuration file, which is the `jupyter_notebook_config.py` file in the directory you are in.</span></span>  <span data-ttu-id="d5d19-158">Všimněte si, že tento soubor neexistuje – stačí vytvořit Pokud tomu tak je.</span><span class="sxs-lookup"><span data-stu-id="d5d19-158">Note that this file may not exist -- just create it if that is the case.</span></span>  <span data-ttu-id="d5d19-159">Tento soubor má počet polí a ve výchozím nastavení všechny jsou vloženy do komentáře.</span><span class="sxs-lookup"><span data-stu-id="d5d19-159">This file has a number of fields and by default all are commented out.</span></span>  <span data-ttu-id="d5d19-160">Tento soubor můžete otevřít pomocí libovolného textového editoru z libovolně a měli zkontrolujte, zda má alespoň na následující obsah.</span><span class="sxs-lookup"><span data-stu-id="d5d19-160">You can open this file with any text editor of your liking, and you should ensure that it has at least the following content.</span></span> <span data-ttu-id="d5d19-161">**Nezapomeňte nahradit c.NotebookApp.password v konfiguračním sha1 z předchozího kroku**.</span><span class="sxs-lookup"><span data-stu-id="d5d19-161">**Be sure to replace the c.NotebookApp.password in the config with the sha1 from the previous step**.</span></span>

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

### <a name="run-the-jupyter-notebook"></a><span data-ttu-id="d5d19-162">Spustit poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="d5d19-162">Run the Jupyter Notebook</span></span>
<span data-ttu-id="d5d19-163">V tuto chvíli jsme připraveni začít poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d5d19-163">At this point we are ready to start the Jupyter Notebook.</span></span> <span data-ttu-id="d5d19-164">Chcete-li to provést, přejděte do adresáře, které chcete uložit poznámkových bloků v a spustí se server Poznámkový blok Jupyter pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="d5d19-164">To do this, navigate to the directory you want to store notebooks in and start the Jupyter Notebook server with the following command.</span></span>

    /anaconda3/bin/jupyter-notebook

<span data-ttu-id="d5d19-165">Teď by měla být mít přístup k vaší Poznámkový blok Jupyter na adrese `https://[PUBLIC-IP-ADDRESS]:9999`.</span><span class="sxs-lookup"><span data-stu-id="d5d19-165">You should now be able to access your Jupyter Notebook at the address `https://[PUBLIC-IP-ADDRESS]:9999`.</span></span>

<span data-ttu-id="d5d19-166">Pokud nejprve přístup k Poznámkový blok, požádá přihlašovací stránku k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="d5d19-166">When you first access your notebook, the login page asks for your password.</span></span> <span data-ttu-id="d5d19-167">A jakmile se přihlásíte, zobrazí se vám "Jupyter poznámkového bloku řídicího panelu", který je obrazit řídicí centrum pro všechny operace poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="d5d19-167">And once you log in, you will see the "Jupyter Notebook Dashboard", which is the ontrol center for all notebook operations.</span></span>  <span data-ttu-id="d5d19-168">Z této stránky můžete vytvořit nové poznámkových bloků a otevřít existující.</span><span class="sxs-lookup"><span data-stu-id="d5d19-168">From this page you can create new notebooks and open existing ones.</span></span>

![snímek obrazovky](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a><span data-ttu-id="d5d19-170">Pomocí poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="d5d19-170">Using the Jupyter Notebook</span></span>
<span data-ttu-id="d5d19-171">Pokud kliknete **nový** tlačítko, zobrazí se následující úvodní stránce.</span><span class="sxs-lookup"><span data-stu-id="d5d19-171">If you click the **New** button, you will see the following opening page.</span></span>

![snímek obrazovky](./media/jupyter-notebook/jupyter-untitled-notebook.png)

<span data-ttu-id="d5d19-173">V oblasti označené `In []:` řádku je vstupní oblast a zde je možné zadat libovolný platný kód Python a provede při zásahu `Shift-Enter` nebo klikněte na ikonu "Play" (pravém trojúhelník směřující na panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="d5d19-173">The area marked with an `In []:` prompt is the input area, and here you can type any valid Python code and it will execute when you hit `Shift-Enter` or click on the "Play" icon (the right-pointing triangle in the toolbar).</span></span>

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a><span data-ttu-id="d5d19-174">Efektivní zlepší: výpočetní dokumentů s bohatý mediální za provozu</span><span class="sxs-lookup"><span data-stu-id="d5d19-174">A powerful paradigm: live computational documents with rich media</span></span>
<span data-ttu-id="d5d19-175">Poznámkový blok, samotné by měli považovat velmi přirozené všem uživatelům, kteří se používá Python a textový editor, protože je v některých způsobech kombinaci obou: můžete spustit bloky kódu jazyka Python, ale můžete také ponechat poznámky a další text změnou stylu buňky z "Kód" do "Markdo WN"v rozevírací nabídce panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="d5d19-175">The notebook itself should feel very natural to anyone who has used Python and a word processor, because it is in some ways a mix of both: you can execute blocks of Python code, but you can also keep notes and other text by changing the style of a cell from "Code" to "Markdown" using the drop-down menu in the toolbar.</span></span>

<span data-ttu-id="d5d19-176">Jupyter je mnohem víc než textový editor jako umožňuje směšování výpočtů a bohaté média (text, grafiky, videa a prakticky nic, co můžete zobrazit moderní webový prohlížeč).</span><span class="sxs-lookup"><span data-stu-id="d5d19-176">Jupyter is much more than a word processor as it allows the mixing of computation and rich media (text, graphics, video and virtually anything a modern web browser can display).</span></span> <span data-ttu-id="d5d19-177">Je možné kombinovat, text, kód, videa a další!</span><span class="sxs-lookup"><span data-stu-id="d5d19-177">You can mix, text, code, videos and more!</span></span>

![snímek obrazovky](./media/jupyter-notebook/jupyter-editing-experience.png)

<span data-ttu-id="d5d19-179">A s výkonem mnoho vynikající knihoven jazyka Python pro technické a vědecké výpočty, na následujícím snímku obrazovky že jednoduchý výpočet lze provést stejně snadné jako analýzy komplexní sítě, všechny v jednom prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5d19-179">And with the power of Python's many excellent libraries for scientific and technical computing, in the following screenshot, a simple calculation can be performed with the same ease as a complex network analysis, all in one environment.</span></span>

<span data-ttu-id="d5d19-180">Tato zlepší výkon moderní webové promíchání s za provozu výpočetní nabízí mnoho možností a je ideální pro cloud; můžete použít Poznámkový blok:</span><span class="sxs-lookup"><span data-stu-id="d5d19-180">This paradigm of mixing the power of the modern web with live computation offers many possibilities, and is ideally suited for the cloud; the Notebook can be used:</span></span>

* <span data-ttu-id="d5d19-181">Jako výpočetní zápisník k zaznamenání nahodilého fungovat na problém.</span><span class="sxs-lookup"><span data-stu-id="d5d19-181">As a computational scratchpad to record exploratory work on a problem.</span></span>
* <span data-ttu-id="d5d19-182">Chcete-li sdílet s kolegy výsledky, v "live" výpočetní formuláře nebo v výtisk formátů (HTML, PDF).</span><span class="sxs-lookup"><span data-stu-id="d5d19-182">To share results with colleagues, either in 'live' computational form or in hardcopy formats (HTML, PDF).</span></span>
* <span data-ttu-id="d5d19-183">Distribuovat a prezentovat za provozu vyučující materiály, které zahrnují výpočtů, takže studenty okamžitě můžete experimentovat s kód skutečné, upravit jej a znovu spustit interaktivně.</span><span class="sxs-lookup"><span data-stu-id="d5d19-183">To distribute and present live teaching materials that involve computation, so students can immediately experiment with the real code, modify it and re-execute it interactively.</span></span>
* <span data-ttu-id="d5d19-184">K zajištění "spustitelné papír", která představují výsledky research způsobem, který může být okamžitě opakuje, ověřit a rozšířit pomocí jiné.</span><span class="sxs-lookup"><span data-stu-id="d5d19-184">To provide "executable papers" that present the results of research in a way that can be immediately reproduced, validated and extended by others.</span></span>
* <span data-ttu-id="d5d19-185">Jako platforma pro spolupráci výpočty: více uživatelů se můžete přihlásit na stejný server poznámkového bloku sdílet relaci výpočetní za provozu.</span><span class="sxs-lookup"><span data-stu-id="d5d19-185">As a platform for collaborative computing: multiple users can log in to the same notebook server to share a live computational session.</span></span>

<span data-ttu-id="d5d19-186">Pokud přejdete ke zdrojovému kódu IPython [úložiště][repository], zjistíte, celého adresáře s příklady Poznámkový blok, které můžete stáhnout a potom experimentovat s na své vlastní virtuální počítač Azure Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d5d19-186">If you go to the IPython source code [repository][repository], you will find an entire directory with notebook examples which you can download and then experiment with on your own Azure Jupyter VM.</span></span>  <span data-ttu-id="d5d19-187">Jednoduše Stáhnout `.ipynb` soubory z lokality a odešlete na řídicím panelu v poznámkovém bloku virtuální počítač Azure (nebo je stáhnout přímo do virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="d5d19-187">Simply download the `.ipynb` files from the site and upload them onto the dashboard of your notebook Azure VM (or download them directly into the VM).</span></span>

## <a name="conclusion"></a><span data-ttu-id="d5d19-188">Závěr</span><span class="sxs-lookup"><span data-stu-id="d5d19-188">Conclusion</span></span>
<span data-ttu-id="d5d19-189">Poznámkového bloku Jupyter poskytuje výkonné rozhraní pro přístup k interaktivním power ekosystému Python v Azure.</span><span class="sxs-lookup"><span data-stu-id="d5d19-189">The Jupyter Notebook provides a powerful interface for accessing interactively the power of the Python ecosystem on Azure.</span></span>  <span data-ttu-id="d5d19-190">Pokrývá širokou škálu případy využití, včetně jednoduchého zkoumání a učení Python, analýzy dat a vizualizaci, simulace a paralelní výpočty.</span><span class="sxs-lookup"><span data-stu-id="d5d19-190">It covers a wide range of usage cases including simple exploration and learning Python, data analysis and visualization, simulation and parallel computing.</span></span> <span data-ttu-id="d5d19-191">Výsledný poznámkového bloku dokumenty obsahují úplný záznam výpočtů, u kterých se provádějí a je možné sdílet s ostatními uživateli Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d5d19-191">The resulting Notebook documents contain a complete record of the computations that are performed and can be shared with other Jupyter users.</span></span>  <span data-ttu-id="d5d19-192">Poznámkového bloku Jupyter lze použít jako místní aplikace, ale jeho je ideální pro nasazení cloudu v Azure</span><span class="sxs-lookup"><span data-stu-id="d5d19-192">The Jupyter Notebook can be used as a local application, but it is ideally suited for cloud deployments on Azure</span></span>

<span data-ttu-id="d5d19-193">Hlavní funkce Jupyter jsou také k dispozici v sadě Visual Studio prostřednictvím [Python Tools pro Visual Studio] [ Python Tools for Visual Studio] (PTVS).</span><span class="sxs-lookup"><span data-stu-id="d5d19-193">The core features of Jupyter are also available inside Visual Studio via the [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS).</span></span> <span data-ttu-id="d5d19-194">PTVS je bezplatný a open source od společnosti Microsoft, který profilování a paralelní se změní rozšířené vývoj prostředí Python, které obsahuje editoru upřesnit pomocí IntelliSense, ladění, Visual Studio modulu plug-in computing integrace.</span><span class="sxs-lookup"><span data-stu-id="d5d19-194">PTVS is a free and open-source plug-in from Microsoft that turns Visual Studio into an advanced Python development environment that includes an advanced editor with IntelliSense, debugging, profiling and parallel computing integration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5d19-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5d19-195">Next steps</span></span>
<span data-ttu-id="d5d19-196">Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="d5d19-196">For more information, see the [Python Developer Center](/develop/python/).</span></span>

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
