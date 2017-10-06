---
title: "aaaInstall hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Nainstalujte hello Azure CLI 1.0 pro Mac, Linux a Windows toostart pomocí služeb Azure"
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
ms.openlocfilehash: a8cd4e38fde6e4b17a768a7caecd280cd91a70f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-cli-10"></a><span data-ttu-id="20c4c-103">Nainstalujte hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="20c4c-103">Install hello Azure CLI 1.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="20c4c-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="20c4c-104">PowerShell</span></span>](/powershell/azure/overview)
> * [<span data-ttu-id="20c4c-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="20c4c-105">Azure CLI 1.0</span></span>](cli-install-nodejs.md)
> * [<span data-ttu-id="20c4c-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="20c4c-106">Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> <span data-ttu-id="20c4c-107">Toto téma popisuje, jakým způsobem volá tooinstall hello 1.0 rozhraní příkazového řádku Azure, který je založený na nodeJs a podporuje všechny rozhraní API nasazení classic a také velký počet aktivity nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="20c4c-107">This topic describes how tooinstall hello Azure CLI 1.0, which is built on nodeJs and supports all classic deployment API calls as well as a large number of Resource Manager deployment activities.</span></span> <span data-ttu-id="20c4c-108">Měli byste použít hello [Azure CLI 2.0](/cli/azure/overview) pro správu a nasazení nových nebo budoucnost rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="20c4c-108">You should use hello [Azure CLI 2.0](/cli/azure/overview) for new or forward-looking CLI deployments and management.</span></span>

<span data-ttu-id="20c4c-109">Rychle nainstalujte rozhraní příkazového řádku Azure (Azure CLI 1.0) toouse hello sadu příkazů open source založený na prostředí pro vytváření a správu prostředků v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="20c4c-109">Quickly install hello Azure Command-Line Interface (Azure CLI 1.0) toouse a set of open-source shell-based commands for creating and managing resources in Microsoft Azure.</span></span> <span data-ttu-id="20c4c-110">Několik možností tooinstall máte ve vašem počítači těchto nástrojů pro různé platformy:</span><span class="sxs-lookup"><span data-stu-id="20c4c-110">You have several options tooinstall these cross-platform tools on your computer:</span></span>

* <span data-ttu-id="20c4c-111">**balíčku npm** – spuštění npm (hello Správce balíčků pro JavaScript) tooinstall hello nejnovější Azure CLI 1.0 balíček na distribuční Linux nebo OS.</span><span class="sxs-lookup"><span data-stu-id="20c4c-111">**npm package** - Run npm (hello package manager for JavaScript) tooinstall hello latest Azure CLI 1.0 package on your Linux distribution or OS.</span></span> <span data-ttu-id="20c4c-112">Vyžaduje node.js a npm ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20c4c-112">Requires node.js and npm on your computer.</span></span>
* <span data-ttu-id="20c4c-113">**Instalační program** -stáhnout instalační program pro instalaci snadno na Mac nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="20c4c-113">**Installer** - Download an installer for easy installation on Mac or Windows.</span></span>
* <span data-ttu-id="20c4c-114">**Kontejner docker** – začít používat hello nejnovější rozhraní příkazového řádku v kontejner Docker připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="20c4c-114">**Docker container** - Start using hello latest CLI in a ready-to-run Docker container.</span></span> <span data-ttu-id="20c4c-115">Vyžaduje hostitelů Docker v počítači.</span><span class="sxs-lookup"><span data-stu-id="20c4c-115">Requires Docker host on your computer.</span></span>

<span data-ttu-id="20c4c-116">Další možnosti a pozadí, najdete v části hello projektu úložiště na [Githubu](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="20c4c-116">For more options and background, see hello project repository on [GitHub](https://github.com/azure/azure-xplat-cli).</span></span>

<span data-ttu-id="20c4c-117">Po instalaci hello Azure CLI 1.0 [připojit ke svému předplatnému Azure](xplat-cli-connect.md) a spuštění hello **azure** příkazy z vaší rozhraní příkazového řádku (Bash, terminálu, příkazový řádek a tak dále) toowork s vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="20c4c-117">Once hello Azure CLI 1.0 is installed, [connect it with your Azure subscription](xplat-cli-connect.md) and run hello **azure** commands from your command-line interface (Bash, Terminal, Command prompt, and so on) toowork with your Azure resources.</span></span>

## <a name="option-1-install-an-npm-package"></a><span data-ttu-id="20c4c-118">Možnost 1: Instalace balíčku npm</span><span class="sxs-lookup"><span data-stu-id="20c4c-118">Option 1: Install an npm package</span></span>
<span data-ttu-id="20c4c-119">tooinstall hello rozhraní příkazového řádku z balíčku npm, zajistěte, aby byly staženy a nainstalovány hello [nejnovější Node.js a npm](https://nodejs.org/en/download/package-manager/).</span><span class="sxs-lookup"><span data-stu-id="20c4c-119">tooinstall hello CLI from an npm package, make sure you have downloaded and installed hello [latest Node.js and npm](https://nodejs.org/en/download/package-manager/).</span></span> <span data-ttu-id="20c4c-120">Potom spusťte **npm nainstalujte** tooinstall hello rozhraní příkazového řádku azure balíčku:</span><span class="sxs-lookup"><span data-stu-id="20c4c-120">Then, run **npm install** tooinstall hello azure-cli package:</span></span>

```bash
npm install -g azure-cli
```

<span data-ttu-id="20c4c-121">Na Linuxových distribucích, může být nutné toouse **sudo** toosuccessfully spustit hello **npm** příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="20c4c-121">On Linux distributions, you might need toouse **sudo** toosuccessfully run hello **npm** command, as follows:</span></span>

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> <span data-ttu-id="20c4c-122">Pokud potřebujete tooinstall nebo aktualizovat Node.js a npm na operačního systému nebo distribuce systému Linux, doporučujeme nainstalovat poslední Node.js LTS verze hello (4.x).</span><span class="sxs-lookup"><span data-stu-id="20c4c-122">If you need tooinstall or update Node.js and npm on your Linux distribution or OS, we recommend that you install hello most recent Node.js LTS version (4.x).</span></span> <span data-ttu-id="20c4c-123">Pokud používáte starší verzi, může docházet k chybám instalace.</span><span class="sxs-lookup"><span data-stu-id="20c4c-123">If you use an older version, you might get installation errors.</span></span>

<span data-ttu-id="20c4c-124">Pokud dáváte přednost, stáhněte si nejnovější Linux hello [vkládání souborů] [ linux-installer] hello npm balíček místně.</span><span class="sxs-lookup"><span data-stu-id="20c4c-124">If you prefer, download hello latest Linux [tar file][linux-installer] for hello npm package locally.</span></span> <span data-ttu-id="20c4c-125">Potom nainstalovat balíček stažené npm hello následujícím způsobem (na Linuxových distribucích může být nutné toouse **sudo**):</span><span class="sxs-lookup"><span data-stu-id="20c4c-125">Then, install hello downloaded npm package as follows (on Linux distributions you might need toouse **sudo**):</span></span>

```bash
npm install -g <path toodownloaded tar file>
```

## <a name="option-2-use-an-installer"></a><span data-ttu-id="20c4c-126">Možnost 2: Použít instalační program</span><span class="sxs-lookup"><span data-stu-id="20c4c-126">Option 2: Use an installer</span></span>
<span data-ttu-id="20c4c-127">Pokud používáte počítač Mac nebo Windows, jsou k dispozici ke stažení hello následující instalační programy rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="20c4c-127">If you use a Mac or Windows computer, hello following CLI installers are available for download:</span></span>

* <span data-ttu-id="20c4c-128">[Instalátor systému Mac OS X][mac-installer]</span><span class="sxs-lookup"><span data-stu-id="20c4c-128">[Mac OS X installer][mac-installer]</span></span>
* <span data-ttu-id="20c4c-129">[Windows MSI][windows-installer]</span><span class="sxs-lookup"><span data-stu-id="20c4c-129">[Windows MSI][windows-installer]</span></span>

> [!TIP]
> <span data-ttu-id="20c4c-130">V systému Windows, můžete také stáhnout hello [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9828653) tooinstall hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="20c4c-130">On Windows, you can also download hello [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI.</span></span> <span data-ttu-id="20c4c-131">Tento instalační program poskytuje hello možnost tooinstall další Azure SDK a nástroje příkazového řádku po instalaci hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="20c4c-131">This installer gives you hello option tooinstall additional Azure SDK and command-line tools after installing hello CLI.</span></span>

## <a name="option-3-use-a-docker-container"></a><span data-ttu-id="20c4c-132">Možnost 3: Použití kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="20c4c-132">Option 3: Use a Docker container</span></span>
<span data-ttu-id="20c4c-133">Pokud jste nastavili počítač jako [Docker](https://docs.docker.com/engine/understanding-docker/) hostitele, můžete spustit hello nejnovější Azure CLI 1.0 v kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="20c4c-133">If you have set up your computer as a [Docker](https://docs.docker.com/engine/understanding-docker/) host, you can run hello latest Azure CLI 1.0 in a Docker container.</span></span> <span data-ttu-id="20c4c-134">Spuštění hello následující příkaz (na Linuxových distribucích může být nutné toouse **sudo**):</span><span class="sxs-lookup"><span data-stu-id="20c4c-134">Run hello following command (on Linux distributions you might need toouse **sudo**):</span></span>

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a><span data-ttu-id="20c4c-135">Příkazy spouštějte Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="20c4c-135">Run Azure CLI 1.0 commands</span></span>
<span data-ttu-id="20c4c-136">Po instalaci hello Azure CLI 1.0, spusťte hello **azure** příkazu z příkazového řádku uživatelského rozhraní (Bash, terminálu, příkazový řádek a tak dále).</span><span class="sxs-lookup"><span data-stu-id="20c4c-136">After hello Azure CLI 1.0 is installed, run hello **azure** command from your command-line user interface (Bash, Terminal, Command prompt, and so on).</span></span> <span data-ttu-id="20c4c-137">Například toorun hello nápovědy příkazu, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="20c4c-137">For example, toorun hello help command, type hello following:</span></span>

```azurecli
azure help
```

> [!NOTE]
> <span data-ttu-id="20c4c-138">V některých distribucích systému Linux můžete obdržet chybu podobné příliš`/usr/bin/env: ‘node’: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="20c4c-138">On some Linux distributions, you may receive an error similar too`/usr/bin/env: ‘node’: No such file or directory`.</span></span> <span data-ttu-id="20c4c-139">Tato chyba pochází z poslední instalace instaluje na /usr/bin/nodejs Node.js.</span><span class="sxs-lookup"><span data-stu-id="20c4c-139">This error comes from recent installations of Node.js being installed at /usr/bin/nodejs.</span></span> <span data-ttu-id="20c4c-140">toofix, vytvořit symbolický odkaz příliš/usr/bin/uzlu tak, že spustíte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="20c4c-140">toofix it, create a symbolic link too/usr/bin/node by running this command:</span></span>

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

<span data-ttu-id="20c4c-141">verze hello toosee hello Azure CLI 1.0 jste nainstalovali, typ hello následující:</span><span class="sxs-lookup"><span data-stu-id="20c4c-141">toosee hello version of hello Azure CLI 1.0 you installed, type hello following:</span></span>

```azurecli
azure --version
```

<span data-ttu-id="20c4c-142">Nyní jste připraveni!</span><span class="sxs-lookup"><span data-stu-id="20c4c-142">Now you are ready!</span></span> <span data-ttu-id="20c4c-143">všechny tooaccess hello toowork příkazy rozhraní příkazového řádku s vlastní prostředky [připojit tooyour předplatného Azure z rozhraní příkazového řádku Azure hello](xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="20c4c-143">tooaccess all hello CLI commands toowork with your own resources, [connect tooyour Azure subscription from hello Azure CLI](xplat-cli-connect.md).</span></span>

> [!NOTE]
> <span data-ttu-id="20c4c-144">Při prvním použití Azure CLI, zobrazí se zpráva s dotazem, pokud chcete, aby tooallow informace o využití toocollect Microsoft.</span><span class="sxs-lookup"><span data-stu-id="20c4c-144">When you first use Azure CLI, you see a message asking if you want tooallow Microsoft toocollect usage information.</span></span> <span data-ttu-id="20c4c-145">Účast je dobrovolná.</span><span class="sxs-lookup"><span data-stu-id="20c4c-145">Participation is voluntary.</span></span> <span data-ttu-id="20c4c-146">Pokud si zvolíte tooparticipate, můžete kdykoli zastavit spuštěním `azure telemetry --disable`.</span><span class="sxs-lookup"><span data-stu-id="20c4c-146">If you choose tooparticipate, you can stop at any time by running `azure telemetry --disable`.</span></span> <span data-ttu-id="20c4c-147">účast tooenable kdykoli spustit `azure telemetry --enable`.</span><span class="sxs-lookup"><span data-stu-id="20c4c-147">tooenable participation at any time, run `azure telemetry --enable`.</span></span>

## <a name="update-hello-cli"></a><span data-ttu-id="20c4c-148">Aktualizace hello rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="20c4c-148">Update hello CLI</span></span>
<span data-ttu-id="20c4c-149">Společnost Microsoft vydá často aktualizované verze hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="20c4c-149">Microsoft frequently releases updated versions of hello Azure CLI.</span></span> <span data-ttu-id="20c4c-150">Přeinstalujte hello rozhraní příkazového řádku pomocí instalačního programu hello operačního systému, nebo spusťte kontejner Docker nejnovější hello.</span><span class="sxs-lookup"><span data-stu-id="20c4c-150">Reinstall hello CLI using hello installer for your operating system, or run hello latest Docker container.</span></span> <span data-ttu-id="20c4c-151">Nebo, pokud mají hello nejnovější Node.js a npm nainstalovaná, aktualizovat tak, že zadáte následující hello (na Linuxových distribucích může být nutné toouse **sudo**).</span><span class="sxs-lookup"><span data-stu-id="20c4c-151">Or, if you have hello latest Node.js and npm installed, update by typing hello following (on Linux distributions you might need toouse **sudo**).</span></span>

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a><span data-ttu-id="20c4c-152">Povolit dokončování pomocí tabulátorů</span><span class="sxs-lookup"><span data-stu-id="20c4c-152">Enable tab completion</span></span>
<span data-ttu-id="20c4c-153">Dokončování pomocí tabulátorů z rozhraní příkazového řádku je podporována pro Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="20c4c-153">Tab completion of CLI commands is supported for Mac and Linux.</span></span>

<span data-ttu-id="20c4c-154">tooenable v zsh, spusťte:</span><span class="sxs-lookup"><span data-stu-id="20c4c-154">tooenable it in zsh, run:</span></span>

```bash
echo '. <(azure --completion)' >> .zshrc
```

<span data-ttu-id="20c4c-155">tooenable v bash, spusťte:</span><span class="sxs-lookup"><span data-stu-id="20c4c-155">tooenable it in bash, run:</span></span>

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a><span data-ttu-id="20c4c-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20c4c-156">Next steps</span></span>
* <span data-ttu-id="20c4c-157">[Připojení z rozhraní příkazového řádku tooyour hello předplatného Azure](xplat-cli-connect.md) toocreate a správu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="20c4c-157">[Connect from hello CLI tooyour Azure subscription](xplat-cli-connect.md) toocreate and manage Azure resources.</span></span>
* <span data-ttu-id="20c4c-158">toolearn Další informace o hello rozhraní příkazového řádku Azure, zdrojový kód, odesílat zprávy o problémech, nebo přispívat toohello projekt stáhnete na adrese hello [úložiště GitHub pro hello rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="20c4c-158">toolearn more about hello Azure CLI, download source code, report problems, or contribute toohello project, visit hello [GitHub repository for hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="20c4c-159">Pokud máte dotazy týkající se používání hello rozhraní příkazového řádku Azure nebo Azure, navštivte hello [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="20c4c-159">If you have questions about using hello Azure CLI, or Azure, visit hello [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
