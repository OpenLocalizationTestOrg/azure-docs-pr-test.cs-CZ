---
title: "aaaInstall Python a hello SDK – Azure"
description: "Zjistěte, jak tooinstall Python a hello toouse SDK s Azure."
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
ms.openlocfilehash: c1b394770f9abd3e654a23d79ae179a9af89e2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="installing-python-and-hello-sdk"></a>Instalace jazyka Python a hello SDK
Python je snadno tooset až v systému Windows a dodává se předem nainstalovaná v systému Mac, Linux, a [Bash pro systém Windows](https://msdn.microsoft.com/commandline/wsl/about). Tento průvodce vás provede procesem instalace a získávání váš počítač připravený k použití s Azure.

## <a name="whats-in-hello-python-azure-sdk"></a>Co je v hello Python Azure SDK?
Hello Azure SDK pro jazyk Python obsahuje součásti, které vám umožňují toodevelop, nasazení a Správa aplikací Python pro Azure. Konkrétně hello Azure SDK pro jazyk Python obsahuje hello následující:

* **Správa knihovny**. Tyto knihovny tříd poskytují rozhraní správu prostředků Azure, například účty úložiště, virtuální počítače.
* **Knihovny za běhu**. Tyto knihovny tříd poskytují rozhraní pro přístup k Azure funkce, jako je například úložiště a service bus.

## <a name="which-python-and-which-version-toouse"></a>Které Python a které toouse verze
Nejsou k dispozici několik typů Python překladače – mezi příklady patří:

* CPython - překladač Pythonu standardní a nejčastěji používané hello
* PyPy - tooCPython rychlé, kompatibilní s jinou implementaci
* IronPython - překladač Pythonu, která běží na rozhraní .net/CLR
* Jython - překladač Pythonu, která běží na hello virtuálního počítače Java

**CPython** v2.7 nebo v3.3 + a PyPy 5.4.0 jsou testovány a podporované na hello Python Azure SDK.

## <a name="where-tooget-python"></a>Kde tooget Python?
Existuje několik způsobů tooget CPython:

* Přímo z [www.python.org][www.python.org]
* Z důvěryhodných distro jako [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] nebo [www.activestate.com][www.activestate.com]
* Sestavení ze zdroje!

Pokud máte konkrétní potřebu, doporučujeme hello první dvě možnosti.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Instalace sady SDK v systému Windows, Linux a systému MacOS (pouze klientské knihovny)
Pokud je již nainstalován jazyk Python, můžete použít pip tooinstall sady všech knihoven klienta hello ve vaší stávající Python 2.7 nebo prostředí Python 3.3 +. Tato akce stáhne hello balíčky z hello [indexu balíčků Pythonu] [ Python Package Index] (úložiště PyPI).

Budete pravděpodobně potřebovat oprávnění správce:

* Linux a systému MacOS, použijte hello `sudo` příkaz: `sudo pip install azure-mgmt-compute`.
* Windows: Otevřete prostředí PowerShell nebo příkazový řádek jako správce

Můžete nainstalovat jednotlivě každý knihovny pro každou službu Azure:

```console
   $ pip install azure-batch          # Install hello latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install hello latest Storage management library
```

Náhled balíčky můžete nainstalovat pomocí hello `--pre` příznak:

```console
   $ pip install --pre azure-mgmt-compute # installs only hello latest Compute Management library
```

Můžete taky nainstalovat sadu Azure knihovny na jediném řádku pomocí hello `azure` metabalíček. Vzhledem k tomu, že ne všechny balíčky v tomto balíčku meta publikují se jako stabilní ještě, hello `azure` metabalíček je stále ve verzi preview.
Ale hello základní balíčků z perspektivy kvality/úplnost kód lze považovat za "stabilní" v tuto chvíli

* oficiálně označen jako takový synchronizované s jinými jazyky co nejdříve.
  Jsme nejsou plánování na všechny další hlavní změny do té doby.

Vzhledem k tomu, že je verze preview, budete potřebovat toouse hello `--pre` příznak:

```console
   $ pip install --pre azure
```

nebo přímo

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Získávání další balíčky
Hello [indexu balíčků Pythonu] [ Python Package Index] (úložiště PyPI) má bohaté výběr Python knihovny.  Pokud jste zvolili tooinstall Distro, budete již mít většinu hello zajímavé bitů pro různé scénáře tooTechnical vývoj webové výpočetní.

## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio
[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) je plug-in bez/OSS od společnosti Microsoft, která převede VS do plnohodnotný IDE Python:

![How-k-install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Pomocí PTVS je nepovinný, ale doporučuje se jako nabízí podporu jazyka Python a webový projekt nebo řešení, ladění, profilace, interaktivních okna, úprav šablony a technologii Intellisense.

PTVS také umožňuje snadno toodeploy tooMicrosoft Azure, s podporou pro nasazení příliš[cloudové služby](cloud-services/cloud-services-python-ptvs.md) a [weby](app-service-web/web-sites-python-ptvs-django-mysql.md).

PTVS funguje s vaší stávající instalaci sady Visual Studio 2013, 2015 nebo 2017.  Dokumentace, stahování a diskusí najdete v tématu [Python Tools pro Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scénáře pro systémy Linux a systému MacOS
Pro Linux ani systému MacOS, hlavní scénáře Azure, které jsou podporovány:

1. Využívání služeb Azure pomocí hello klientské knihovny pro jazyk Python
2. Spuštění aplikace v virtuálního počítače s Linuxem
3. Vývoj a publikování tooAzure weby pomocí Git

první scénář Hello vám umožní tooauthor bohatých webových aplikací, které využít výhod hello Azure PaaS možnosti, jako [úložiště objektů blob](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [fronty úložiště](storage/queues/storage-python-how-to-use-queue-storage.md), [tabulky úložiště](cosmos-db/table-storage-how-to-use-python.md) atd. prostřednictvím Pythonic obálek pro hello rozhraní API REST služby Azure. Fungují stejně jako v systému Windows, Mac a Linux.  Můžete taky tyto knihovny klienta z místní vývojovém počítači nebo virtuálního počítače s Linuxem spuštěné v Azure.

Pro scénář hello virtuálních počítačů jednoduše spusťte virtuální počítač s Linuxem podle vašeho výběru (Ubuntu, CentOS, Suse) a spouštět a spravovat co se vám líbí.  Například můžete spustit [IPython] [ IPython] REPL/poznámkového bloku na počítači Windows nebo Mac/Linux a bod prohlížeče tooa Linux nebo spuštění virtuálního počítače pro více procesorů Windows hello IPython modulu v Azure.

Informace o tom tooset si virtuální počítač s Linuxem, najdete v části hello [vytvořit virtuálním počítači systémem Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kurzu.

Pomocí nasazení Git, můžete vyvíjet webovou aplikaci Python a publikujete ji tooan webu Azure ve všech operačních systémech.  Když stisknete tooAzure vašeho úložiště, automaticky vytvoří virtuální prostředí a pip nainstaluje požadované balíčky.

Další informace o vývoji a publikování webů Azure, najdete v části hello kurzy pro [vytváření webů s rozhraním Django](app-service-web/web-sites-python-create-deploy-django-app.md), [vytváření webů s Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), a [vytváření webů s Flask](app-service-web/web-sites-python-create-deploy-flask-app.md). Další obecné informace o použití žádné kompatibilní se standardem WSGI framework najdete v tématu [konfigurace Python s weby Azure](app-service-web/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Další Software a prostředky:
* [Azure SDK pro Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK pro Python Githubu](https://github.com/Azure/azure-sdk-for-python)
* [Oficiální ukázek Azure pro jazyk Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Distribuci jazyka Python poměrně Analytics][Continuum Analytics Python Distribution]
* [Distribuci jazyka Python Enthought][Enthought Python Distribution]
* [Distribuci jazyka Python ActiveState][ActiveState Python Distribution]
* [SciPy - sada knihoven Scientific Python][SciPy - A suite of Scientific Python libraries]
* [NumPy - číslice knihovny pro jazyk Python][NumPy - A numerics library for Python]
* [Projekt Django – vyspělá webové framework/CMS][Django Project - A mature web framework/CMS]
* [IPython - k pokročilé REPL/Poznámkový blok pro jazyk Python][IPython - an advanced REPL/Notebook for Python]
* [Python Tools pro Visual Studio na Githubu][Python Tools for Visual Studio on GitHub]
* [Středisko pro vývojáře programující v Pythonu](/develop/python/)

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
[Setting up a Linux VM via hello Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How toouse hello Azure Command-Line Interface]: crossplat-cmd-tools.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-quick-create-cli.md
[Creating Websites with Django]: web-sites-python-create-deploy-django-app.md
[Creating Websites with Bottle]: web-sites-python-create-deploy-bottle-app.md
[Creating Websites with Flask]: web-sites-python-create-deploy-flask-app.md
[Configuring Python with Azure Websites]: web-sites-python-configure.md
[table storage]: storage-python-how-to-use-table-storage.md
[queue storage]: storage-python-how-to-use-queue-storage.md
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
