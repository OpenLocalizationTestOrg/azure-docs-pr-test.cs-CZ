---
title: "Instalace jazyka Python a sady SDK – Azure"
description: "Informace o instalaci Pythonu a sady SDK pro použití s Azure."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: c9df4e1f7677b2ed10684f6f3c981f2abf64f171
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="installing-python-and-the-sdk"></a><span data-ttu-id="2feab-103">Instalace jazyka Python a sady SDK</span><span class="sxs-lookup"><span data-stu-id="2feab-103">Installing Python and the SDK</span></span>
<span data-ttu-id="2feab-104">Python je možné snadno nastavit v systému Windows a obsahuje předem nainstalovaná v systému Mac, Linux, a [Bash pro systém Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="2feab-104">Python is easy to set up on Windows and comes pre-installed on Mac, Linux, and [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span> <span data-ttu-id="2feab-105">Tento průvodce vás provede procesem instalace a získávání váš počítač připravený k použití s Azure.</span><span class="sxs-lookup"><span data-stu-id="2feab-105">This guide walks you through installation and getting your machine ready for use with Azure.</span></span>

## <a name="whats-in-the-python-azure-sdk"></a><span data-ttu-id="2feab-106">Co je v Python Azure SDK?</span><span class="sxs-lookup"><span data-stu-id="2feab-106">What's in the Python Azure SDK?</span></span>
<span data-ttu-id="2feab-107">Azure SDK pro jazyk Python obsahuje součásti, které vám umožní vyvíjet, nasazení a Správa aplikací Python pro Azure.</span><span class="sxs-lookup"><span data-stu-id="2feab-107">The Azure SDK for Python includes components that allow you to develop, deploy, and manage Python applications for Azure.</span></span> <span data-ttu-id="2feab-108">Konkrétně sadu Azure SDK pro jazyk Python zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="2feab-108">Specifically, the Azure SDK for Python includes the following:</span></span>

* <span data-ttu-id="2feab-109">**Správa knihovny**.</span><span class="sxs-lookup"><span data-stu-id="2feab-109">**Management libraries**.</span></span> <span data-ttu-id="2feab-110">Tyto knihovny tříd poskytují rozhraní správu prostředků Azure, například účty úložiště, virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2feab-110">These class libraries provide an interface managing Azure resources, such as storage accounts, virtual machines.</span></span>
* <span data-ttu-id="2feab-111">**Knihovny za běhu**.</span><span class="sxs-lookup"><span data-stu-id="2feab-111">**Runtime libraries**.</span></span> <span data-ttu-id="2feab-112">Tyto knihovny tříd poskytují rozhraní pro přístup k Azure funkce, jako je například úložiště a service bus.</span><span class="sxs-lookup"><span data-stu-id="2feab-112">These class libraries provide an interface for accessing Azure features, such as storage and service bus.</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="2feab-113">Které Python a která verze se má použít</span><span class="sxs-lookup"><span data-stu-id="2feab-113">Which Python and which version to use</span></span>
<span data-ttu-id="2feab-114">Nejsou k dispozici několik typů Python překladače – mezi příklady patří:</span><span class="sxs-lookup"><span data-stu-id="2feab-114">There are several flavors of Python interpreters available - examples include:</span></span>

* <span data-ttu-id="2feab-115">CPython – standard a nejčastěji používané překladač Pythonu</span><span class="sxs-lookup"><span data-stu-id="2feab-115">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="2feab-116">PyPy - rychlé, kompatibilní s jinou implementaci na CPython</span><span class="sxs-lookup"><span data-stu-id="2feab-116">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="2feab-117">IronPython - překladač Pythonu, která běží na rozhraní .net/CLR</span><span class="sxs-lookup"><span data-stu-id="2feab-117">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="2feab-118">Jython - překladač Pythonu, který běží na virtuálním počítači Java</span><span class="sxs-lookup"><span data-stu-id="2feab-118">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="2feab-119">**CPython** v2.7 nebo v3.3 + a PyPy 5.4.0 jsou testovány a podporovány pro sady Azure SDK pro Python.</span><span class="sxs-lookup"><span data-stu-id="2feab-119">**CPython** v2.7 or v3.3+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="2feab-120">Kde získat jazyk Python?</span><span class="sxs-lookup"><span data-stu-id="2feab-120">Where to get Python?</span></span>
<span data-ttu-id="2feab-121">Chcete-li získat CPython několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="2feab-121">There are several ways to get CPython:</span></span>

* <span data-ttu-id="2feab-122">Přímo z [www.python.org][www.python.org]</span><span class="sxs-lookup"><span data-stu-id="2feab-122">Directly from [www.python.org][www.python.org]</span></span>
* <span data-ttu-id="2feab-123">Z důvěryhodných distro jako [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] nebo [www.activestate.com][www.activestate.com]</span><span class="sxs-lookup"><span data-stu-id="2feab-123">From a reputable distro such as [www.continuum.io][www.continuum.io], [www.enthought.com][www.enthought.com] or [www.activestate.com][www.activestate.com]</span></span>
* <span data-ttu-id="2feab-124">Sestavení ze zdroje!</span><span class="sxs-lookup"><span data-stu-id="2feab-124">Build from source!</span></span>

<span data-ttu-id="2feab-125">Pokud máte konkrétní potřebu, doporučujeme první dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="2feab-125">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a><span data-ttu-id="2feab-126">Instalace sady SDK v systému Windows, Linux a systému MacOS (pouze klientské knihovny)</span><span class="sxs-lookup"><span data-stu-id="2feab-126">SDK Installation on Windows, Linux, and MacOS (client libraries only)</span></span>
<span data-ttu-id="2feab-127">Pokud je již nainstalován jazyk Python, můžete k instalaci sady všechny knihovny klienta ve vaší stávající Python 2.7 nebo prostředí Python 3.3 + pip.</span><span class="sxs-lookup"><span data-stu-id="2feab-127">If you already have Python installed, you can use pip to install a bundle of all the client libraries in your existing Python 2.7 or Python 3.3+ environment.</span></span> <span data-ttu-id="2feab-128">Tato akce stáhne balíčky ze [indexu balíčků Pythonu] [ Python Package Index] (úložiště PyPI).</span><span class="sxs-lookup"><span data-stu-id="2feab-128">This downloads the packages from the [Python Package Index][Python Package Index] (PyPI).</span></span>

<span data-ttu-id="2feab-129">Budete pravděpodobně potřebovat oprávnění správce:</span><span class="sxs-lookup"><span data-stu-id="2feab-129">You may need administrator rights:</span></span>

* <span data-ttu-id="2feab-130">Linux a systému MacOS, použijte `sudo` příkaz: `sudo pip install azure-mgmt-compute`.</span><span class="sxs-lookup"><span data-stu-id="2feab-130">Linux and MacOS, use the `sudo` command: `sudo pip install azure-mgmt-compute`.</span></span>
* <span data-ttu-id="2feab-131">Windows: Otevřete prostředí PowerShell nebo příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="2feab-131">Windows: open your PowerShell/Command prompt as an administrator</span></span>

<span data-ttu-id="2feab-132">Můžete nainstalovat jednotlivě každý knihovny pro každou službu Azure:</span><span class="sxs-lookup"><span data-stu-id="2feab-132">You can install individually each library for each Azure service:</span></span>

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="2feab-133">Náhled balíčky můžete nainstalovat pomocí `--pre` příznak:</span><span class="sxs-lookup"><span data-stu-id="2feab-133">Preview packages can be installed using the `--pre` flag:</span></span>

```console
   $ pip install --pre azure-mgmt-compute # installs only the latest Compute Management library
```

<span data-ttu-id="2feab-134">Můžete taky nainstalovat sadu Azure knihovny v jediném řádku pomocí `azure` metabalíček.</span><span class="sxs-lookup"><span data-stu-id="2feab-134">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span> <span data-ttu-id="2feab-135">Vzhledem k tomu, že ne všechny balíčky v tomto balíčku meta publikují se jako stabilní ještě `azure` metabalíček je stále ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="2feab-135">Since not all packages in this meta-package are published as stable yet, the `azure` meta-package is still in preview.</span></span>
<span data-ttu-id="2feab-136">Však balíčky jádra, z perspektivy kvality/úplnost kód lze považovat za "stabilní" v tuto chvíli</span><span class="sxs-lookup"><span data-stu-id="2feab-136">However, the core packages, from code quality/completeness perspectives can be considered "stable" at this time</span></span>

* <span data-ttu-id="2feab-137">oficiálně označen jako takový synchronizované s jinými jazyky co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="2feab-137">it is officially labeled as such in sync with other languages as soon as possible.</span></span>
  <span data-ttu-id="2feab-138">Jsme nejsou plánování na všechny další hlavní změny do té doby.</span><span class="sxs-lookup"><span data-stu-id="2feab-138">We are not planning on any further major changes until then.</span></span>

<span data-ttu-id="2feab-139">Vzhledem k tomu, že je verze preview, budete muset použít `--pre` příznak:</span><span class="sxs-lookup"><span data-stu-id="2feab-139">Since it's a preview release, you need to use the `--pre` flag:</span></span>

```console
   $ pip install --pre azure
```

<span data-ttu-id="2feab-140">nebo přímo</span><span class="sxs-lookup"><span data-stu-id="2feab-140">or directly</span></span>

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a><span data-ttu-id="2feab-141">Získávání další balíčky</span><span class="sxs-lookup"><span data-stu-id="2feab-141">Getting More Packages</span></span>
<span data-ttu-id="2feab-142">[Indexu balíčků Pythonu] [ Python Package Index] (úložiště PyPI) má bohaté výběr Python knihovny.</span><span class="sxs-lookup"><span data-stu-id="2feab-142">The [Python Package Index][Python Package Index] (PyPI) has a rich selection of Python libraries.</span></span>  <span data-ttu-id="2feab-143">Pokud jste vybrali k instalaci Distro, budete již mít většinu zajímavé bitů pro různé scénáře vývoje webů na technické projekty.</span><span class="sxs-lookup"><span data-stu-id="2feab-143">If you chose to install a Distro, you'll already have most of the interesting bits for various scenarios from web development to Technical Computing.</span></span>

## <a name="python-tools-for-visual-studio"></a><span data-ttu-id="2feab-144">Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2feab-144">Python Tools for Visual Studio</span></span>
<span data-ttu-id="2feab-145">[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) je plug-in bez/OSS od společnosti Microsoft, která převede VS do plnohodnotný IDE Python:</span><span class="sxs-lookup"><span data-stu-id="2feab-145">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) is a free/OSS plugin from Microsoft, which turns VS into a full-fledged Python IDE:</span></span>

![How-k-install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

<span data-ttu-id="2feab-147">Pomocí PTVS je nepovinný, ale doporučuje se jako nabízí podporu jazyka Python a webový projekt nebo řešení, ladění, profilace, interaktivních okna, úprav šablony a technologii Intellisense.</span><span class="sxs-lookup"><span data-stu-id="2feab-147">Using PTVS is optional, but is recommended as it gives you Python and Web Project/Solution support, debugging, profiling, interactive window, Template editing, and Intellisense.</span></span>

<span data-ttu-id="2feab-148">PTVS také usnadňuje nasazení do služby Microsoft Azure s podporou pro nasazení do [cloudové služby](cloud-services/cloud-services-python-ptvs.md) a [weby](app-service-web/web-sites-python-ptvs-django-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="2feab-148">PTVS also makes it easy to deploy to Microsoft Azure, with support for deployment to [Cloud Services](cloud-services/cloud-services-python-ptvs.md) and [Websites](app-service-web/web-sites-python-ptvs-django-mysql.md).</span></span>

<span data-ttu-id="2feab-149">PTVS funguje s vaší stávající instalaci sady Visual Studio 2013, 2015 nebo 2017.</span><span class="sxs-lookup"><span data-stu-id="2feab-149">PTVS works with your existing Visual Studio 2013, 2015, or 2017 installation.</span></span>  <span data-ttu-id="2feab-150">Dokumentace, stahování a diskusí najdete v tématu [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="2feab-150">For documentation, downloads and discussions, see [Python Tools for Visual Studio].</span></span>  

## <a name="python-azure-scenarios-for-linux-and-macos"></a><span data-ttu-id="2feab-151">Python Azure scénáře pro systémy Linux a systému MacOS</span><span class="sxs-lookup"><span data-stu-id="2feab-151">Python Azure Scenarios for Linux and MacOS</span></span>
<span data-ttu-id="2feab-152">Pro Linux ani systému MacOS, hlavní scénáře Azure, které jsou podporovány:</span><span class="sxs-lookup"><span data-stu-id="2feab-152">For Linux or MacOS, main Azure scenarios that are supported:</span></span>

1. <span data-ttu-id="2feab-153">Využívání služeb Azure pomocí klientské knihovny pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="2feab-153">Consuming Azure Services by using the client libraries for Python</span></span>
2. <span data-ttu-id="2feab-154">Spuštění aplikace v virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="2feab-154">Running your app in a Linux VM</span></span>
3. <span data-ttu-id="2feab-155">Vývoj a publikování na weby Azure pomocí Git</span><span class="sxs-lookup"><span data-stu-id="2feab-155">Developing and publishing to Azure Websites using Git</span></span>

<span data-ttu-id="2feab-156">První scénář umožňuje vytvářet bohaté webových aplikací, které využít výhod funkcí Azure PaaS, jako [úložiště objektů blob](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [fronty úložiště](storage/queues/storage-python-how-to-use-queue-storage.md), [tabulky úložiště](cosmos-db/table-storage-how-to-use-python.md) apod prostřednictvím Pythonic obálek pro rozhraní API REST Azure.</span><span class="sxs-lookup"><span data-stu-id="2feab-156">The first scenario enables you to author rich web apps that take advantage of the Azure PaaS capabilities such as [blob storage](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [queue storage](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) etc. via Pythonic wrappers for the Azure REST APIs.</span></span> <span data-ttu-id="2feab-157">Fungují stejně jako v systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="2feab-157">These work identically on Windows, Mac, and Linux.</span></span>  <span data-ttu-id="2feab-158">Můžete taky tyto knihovny klienta z místní vývojovém počítači nebo virtuálního počítače s Linuxem spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="2feab-158">You can also use these client libraries from your local development machine or a Linux VM running on Azure.</span></span>

<span data-ttu-id="2feab-159">Pro tento scénář virtuální počítač jednoduše spustit virtuální počítač s Linuxem podle vašeho výběru (Ubuntu, CentOS, Suse) a spouštět a spravovat co se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="2feab-159">For the VM scenario, you simply start a Linux VM of your choice (Ubuntu, CentOS, Suse) and run/manage what you like.</span></span>  <span data-ttu-id="2feab-160">Například můžete spustit [IPython] [ IPython] REPL/poznámkového bloku na vašem systému Windows nebo Mac/Linux počítač a přejděte v prohlížeči na systému Linux nebo Windows více procesorů virtuálního počítače modul IPython v Azure.</span><span class="sxs-lookup"><span data-stu-id="2feab-160">As an example, you can run [IPython][IPython] REPL/notebook on your Windows/Mac/Linux machine and point your browser to a Linux or Windows multi-proc VM running the IPython Engine on Azure.</span></span>

<span data-ttu-id="2feab-161">Informace o tom, jak nastavit virtuální počítač s Linuxem najdete v tématu [vytvořit virtuálním počítači systémem Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kurzu.</span><span class="sxs-lookup"><span data-stu-id="2feab-161">For information on how to set up a Linux VM, see the [Create a Virtual Machine Running Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tutorial.</span></span>

<span data-ttu-id="2feab-162">Pomocí nasazení Git, můžete vyvíjet webovou aplikaci Python a publikování na web Azure ve všech operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="2feab-162">Using Git deployment, you can develop a Python web application and publish it to an Azure Website from any operating system.</span></span>  <span data-ttu-id="2feab-163">Když stisknete úložiště do Azure, automaticky vytvoří virtuální prostředí a pip nainstaluje požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="2feab-163">When you push your repository to Azure, it automatically creates a virtual environment and pip installs your required packages.</span></span>

<span data-ttu-id="2feab-164">Další informace o vývoji a publikování webů Azure, najdete v části kurzy pro [vytváření webů s rozhraním Django](app-service-web/web-sites-python-create-deploy-django-app.md), [vytváření webů s Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), a [vytváření webů s Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span><span class="sxs-lookup"><span data-stu-id="2feab-164">For more information on developing and publishing Azure Websites, see the tutorials for [Creating Websites with Django](app-service-web/web-sites-python-create-deploy-django-app.md), [Creating Websites with Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), and [Creating Websites with Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span></span> <span data-ttu-id="2feab-165">Další obecné informace o použití žádné kompatibilní se standardem WSGI framework najdete v tématu [konfigurace Python s weby Azure](app-service-web/web-sites-python-configure.md).</span><span class="sxs-lookup"><span data-stu-id="2feab-165">For more general information on using any WSGI-compliant framework, see [Configuring Python with Azure Websites](app-service-web/web-sites-python-configure.md).</span></span>

## <a name="additional-software-and-resources"></a><span data-ttu-id="2feab-166">Další Software a prostředky:</span><span class="sxs-lookup"><span data-stu-id="2feab-166">Additional Software and Resources:</span></span>
* [<span data-ttu-id="2feab-167">Azure SDK pro Python ReadTheDocs</span><span class="sxs-lookup"><span data-stu-id="2feab-167">Azure SDK for Python ReadTheDocs</span></span>](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [<span data-ttu-id="2feab-168">Azure SDK pro Python Githubu</span><span class="sxs-lookup"><span data-stu-id="2feab-168">Azure SDK for Python GitHub</span></span>](https://github.com/Azure/azure-sdk-for-python)
* [<span data-ttu-id="2feab-169">Oficiální ukázek Azure pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="2feab-169">Official Azure samples for Python</span></span>](https://azure.microsoft.com/documentation/samples/?platform=python)
* <span data-ttu-id="2feab-170">[Distribuci jazyka Python poměrně Analytics][Continuum Analytics Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="2feab-170">[Continuum Analytics Python Distribution][Continuum Analytics Python Distribution]</span></span>
* <span data-ttu-id="2feab-171">[Distribuci jazyka Python Enthought][Enthought Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="2feab-171">[Enthought Python Distribution][Enthought Python Distribution]</span></span>
* <span data-ttu-id="2feab-172">[Distribuci jazyka Python ActiveState][ActiveState Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="2feab-172">[ActiveState Python Distribution][ActiveState Python Distribution]</span></span>
* <span data-ttu-id="2feab-173">[SciPy - sada knihoven Scientific Python][SciPy - A suite of Scientific Python libraries]</span><span class="sxs-lookup"><span data-stu-id="2feab-173">[SciPy - A suite of Scientific Python libraries][SciPy - A suite of Scientific Python libraries]</span></span>
* <span data-ttu-id="2feab-174">[NumPy - číslice knihovny pro jazyk Python][NumPy - A numerics library for Python]</span><span class="sxs-lookup"><span data-stu-id="2feab-174">[NumPy - A numerics library for Python][NumPy - A numerics library for Python]</span></span>
* <span data-ttu-id="2feab-175">[Projekt Django – vyspělá webové framework/CMS][Django Project - A mature web framework/CMS]</span><span class="sxs-lookup"><span data-stu-id="2feab-175">[Django Project - A mature web framework/CMS][Django Project - A mature web framework/CMS]</span></span>
* <span data-ttu-id="2feab-176">[IPython - k pokročilé REPL/Poznámkový blok pro jazyk Python][IPython - an advanced REPL/Notebook for Python]</span><span class="sxs-lookup"><span data-stu-id="2feab-176">[IPython - an advanced REPL/Notebook for Python][IPython - an advanced REPL/Notebook for Python]</span></span>
* <span data-ttu-id="2feab-177">[Python Tools pro Visual Studio na Githubu][Python Tools for Visual Studio on GitHub]</span><span class="sxs-lookup"><span data-stu-id="2feab-177">[Python Tools for Visual Studio on GitHub][Python Tools for Visual Studio on GitHub]</span></span>
* [<span data-ttu-id="2feab-178">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="2feab-178">Python Developer Center</span></span>](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython Notebook on Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-quick-create-cli.md
[Creating Websites with Django]: web-sites-python-create-deploy-django-app.md
[Creating Websites with Bottle]: web-sites-python-create-deploy-bottle-app.md
[Creating Websites with Flask]: web-sites-python-create-deploy-flask-app.md
[Configuring Python with Azure Websites]: web-sites-python-configure.md
[table storage]: storage-python-how-to-use-table-storage.md
[queue storage]: storage-python-how-to-use-queue-storage.md
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
