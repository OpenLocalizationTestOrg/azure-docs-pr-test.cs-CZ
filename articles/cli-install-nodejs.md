---
title: Nainstalovat Azure CLI 1.0 | Microsoft Docs
description: "Instalace 1.0 rozhraní příkazového řádku Azure pro Mac, Linux a Windows, pokud chcete začít používat služby Azure"
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
ms.openlocfilehash: 63b35ed25b809a16b61b685fd35aa67474b0a369
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-cli-10"></a><span data-ttu-id="b7cf8-103">Nainstalovat Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b7cf8-103">Install the Azure CLI 1.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7cf8-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7cf8-104">PowerShell</span></span>](/powershell/azure/overview)
> * [<span data-ttu-id="b7cf8-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b7cf8-105">Azure CLI 1.0</span></span>](cli-install-nodejs.md)
> * [<span data-ttu-id="b7cf8-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b7cf8-106">Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> <span data-ttu-id="b7cf8-107">Toto téma popisuje postup instalace Azure CLI verze 1.0, který je založený na nodeJs a podporuje všechny volání rozhraní API nasazení classic a také velký počet aktivity nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-107">This topic describes how to install the Azure CLI 1.0, which is built on nodeJs and supports all classic deployment API calls as well as a large number of Resource Manager deployment activities.</span></span> <span data-ttu-id="b7cf8-108">Měli byste použít [Azure CLI 2.0](/cli/azure/overview) pro správu a nasazení nových nebo budoucnost rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-108">You should use the [Azure CLI 2.0](/cli/azure/overview) for new or forward-looking CLI deployments and management.</span></span>

<span data-ttu-id="b7cf8-109">Rychle nainstalujte rozhraní příkazového řádku Azure (Azure CLI 1.0) používat sadu příkazů open source založený na prostředí pro vytváření a správu prostředků v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-109">Quickly install the Azure Command-Line Interface (Azure CLI 1.0) to use a set of open-source shell-based commands for creating and managing resources in Microsoft Azure.</span></span> <span data-ttu-id="b7cf8-110">Máte několik možností instalace těchto nástrojů pro různé platformy ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-110">You have several options to install these cross-platform tools on your computer:</span></span>

* <span data-ttu-id="b7cf8-111">**balíčku npm** -spustit npm (Správce balíčků pro JavaScript) nainstalovat nejnovější balíček Azure CLI 1.0 na operačního systému nebo distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-111">**npm package** - Run npm (the package manager for JavaScript) to install the latest Azure CLI 1.0 package on your Linux distribution or OS.</span></span> <span data-ttu-id="b7cf8-112">Vyžaduje node.js a npm ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-112">Requires node.js and npm on your computer.</span></span>
* <span data-ttu-id="b7cf8-113">**Instalační program** -stáhnout instalační program pro instalaci snadno na Mac nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-113">**Installer** - Download an installer for easy installation on Mac or Windows.</span></span>
* <span data-ttu-id="b7cf8-114">**Kontejner docker** – spuštění pomocí nejnovější rozhraní příkazového řádku v kontejner Docker připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-114">**Docker container** - Start using the latest CLI in a ready-to-run Docker container.</span></span> <span data-ttu-id="b7cf8-115">Vyžaduje hostitelů Docker v počítači.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-115">Requires Docker host on your computer.</span></span>

<span data-ttu-id="b7cf8-116">Další možnosti a pozadí, najdete v úložišti projektu na [Githubu](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-116">For more options and background, see the project repository on [GitHub](https://github.com/azure/azure-xplat-cli).</span></span>

<span data-ttu-id="b7cf8-117">Po instalaci Azure CLI 1.0 [připojit ke svému předplatnému Azure](xplat-cli-connect.md) a spusťte **azure** příkazy z vaší rozhraní příkazového řádku (Bash, terminálu, příkazový řádek a tak dále) pro práci s vaší Prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-117">Once the Azure CLI 1.0 is installed, [connect it with your Azure subscription](xplat-cli-connect.md) and run the **azure** commands from your command-line interface (Bash, Terminal, Command prompt, and so on) to work with your Azure resources.</span></span>

## <a name="option-1-install-an-npm-package"></a><span data-ttu-id="b7cf8-118">Možnost 1: Instalace balíčku npm</span><span class="sxs-lookup"><span data-stu-id="b7cf8-118">Option 1: Install an npm package</span></span>
<span data-ttu-id="b7cf8-119">Pokud chcete nainstalovat rozhraní příkazového řádku z balíčku npm, zajistěte, aby byly staženy a nainstalovány [nejnovější Node.js a npm](https://nodejs.org/en/download/package-manager/).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-119">To install the CLI from an npm package, make sure you have downloaded and installed the [latest Node.js and npm](https://nodejs.org/en/download/package-manager/).</span></span> <span data-ttu-id="b7cf8-120">Potom spusťte **npm nainstalujte** k instalaci balíčku rozhraní příkazového řádku azure:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-120">Then, run **npm install** to install the azure-cli package:</span></span>

```bash
npm install -g azure-cli
```

<span data-ttu-id="b7cf8-121">Na Linuxových distribucích, možná budete muset použít **sudo** k úspěšnému spuštění **npm** příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-121">On Linux distributions, you might need to use **sudo** to successfully run the **npm** command, as follows:</span></span>

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> <span data-ttu-id="b7cf8-122">Pokud potřebujete instalaci nebo aktualizaci Node.js a npm na operačního systému nebo distribuce systému Linux, doporučujeme nainstalovat nejnovější verzi Node.js LTS (4.x).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-122">If you need to install or update Node.js and npm on your Linux distribution or OS, we recommend that you install the most recent Node.js LTS version (4.x).</span></span> <span data-ttu-id="b7cf8-123">Pokud používáte starší verzi, může docházet k chybám instalace.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-123">If you use an older version, you might get installation errors.</span></span>

<span data-ttu-id="b7cf8-124">Pokud dáváte přednost, stáhněte si nejnovější Linux [vkládání souborů] [ linux-installer] balíčku npm místně.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-124">If you prefer, download the latest Linux [tar file][linux-installer] for the npm package locally.</span></span> <span data-ttu-id="b7cf8-125">Potom nainstalovat balíček stažené npm následujícím způsobem (na Linuxových distribucích možná budete muset použít **sudo**):</span><span class="sxs-lookup"><span data-stu-id="b7cf8-125">Then, install the downloaded npm package as follows (on Linux distributions you might need to use **sudo**):</span></span>

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a><span data-ttu-id="b7cf8-126">Možnost 2: Použít instalační program</span><span class="sxs-lookup"><span data-stu-id="b7cf8-126">Option 2: Use an installer</span></span>
<span data-ttu-id="b7cf8-127">Pokud používáte počítači se systémem Mac nebo Windows, jsou k dispozici ke stažení následujících instalačních programů rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-127">If you use a Mac or Windows computer, the following CLI installers are available for download:</span></span>

* <span data-ttu-id="b7cf8-128">[Instalátor systému Mac OS X][mac-installer]</span><span class="sxs-lookup"><span data-stu-id="b7cf8-128">[Mac OS X installer][mac-installer]</span></span>
* <span data-ttu-id="b7cf8-129">[Windows MSI][windows-installer]</span><span class="sxs-lookup"><span data-stu-id="b7cf8-129">[Windows MSI][windows-installer]</span></span>

> [!TIP]
> <span data-ttu-id="b7cf8-130">V systému Windows, můžete také stáhnout [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9828653) nainstalovat rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-130">On Windows, you can also download the [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) to install the CLI.</span></span> <span data-ttu-id="b7cf8-131">Tento instalační program vám dává možnost nainstalovat další Azure SDK a nástroje příkazového řádku po instalaci rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-131">This installer gives you the option to install additional Azure SDK and command-line tools after installing the CLI.</span></span>

## <a name="option-3-use-a-docker-container"></a><span data-ttu-id="b7cf8-132">Možnost 3: Použití kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="b7cf8-132">Option 3: Use a Docker container</span></span>
<span data-ttu-id="b7cf8-133">Pokud jste nastavili počítač jako [Docker](https://docs.docker.com/engine/understanding-docker/) hostiteli, nejnovější 1.0 rozhraní příkazového řádku Azure můžete spouštět v kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-133">If you have set up your computer as a [Docker](https://docs.docker.com/engine/understanding-docker/) host, you can run the latest Azure CLI 1.0 in a Docker container.</span></span> <span data-ttu-id="b7cf8-134">Spusťte následující příkaz (na Linuxových distribucích možná budete muset použít **sudo**):</span><span class="sxs-lookup"><span data-stu-id="b7cf8-134">Run the following command (on Linux distributions you might need to use **sudo**):</span></span>

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a><span data-ttu-id="b7cf8-135">Příkazy spouštějte Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b7cf8-135">Run Azure CLI 1.0 commands</span></span>
<span data-ttu-id="b7cf8-136">Po instalaci Azure CLI 1.0 spustit **azure** příkazu z příkazového řádku uživatelského rozhraní (Bash, terminálu, příkazový řádek a tak dále).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-136">After the Azure CLI 1.0 is installed, run the **azure** command from your command-line user interface (Bash, Terminal, Command prompt, and so on).</span></span> <span data-ttu-id="b7cf8-137">Například spusťte příkaz nápovědu, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-137">For example, to run the help command, type the following:</span></span>

```azurecli
azure help
```

> [!NOTE]
> <span data-ttu-id="b7cf8-138">V některých distribucích systému Linux, může se zobrazit chyba podobná `/usr/bin/env: ‘node’: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-138">On some Linux distributions, you may receive an error similar to `/usr/bin/env: ‘node’: No such file or directory`.</span></span> <span data-ttu-id="b7cf8-139">Tato chyba pochází z poslední instalace instaluje na /usr/bin/nodejs Node.js.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-139">This error comes from recent installations of Node.js being installed at /usr/bin/nodejs.</span></span> <span data-ttu-id="b7cf8-140">Chcete-li problém odstranit, vytvořte symbolický odkaz na /usr/bin/node spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-140">To fix it, create a symbolic link to /usr/bin/node by running this command:</span></span>

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

<span data-ttu-id="b7cf8-141">Pokud chcete zobrazit verzi 1.0 rozhraní příkazového řádku Azure, jste nainstalovali, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-141">To see the version of the Azure CLI 1.0 you installed, type the following:</span></span>

```azurecli
azure --version
```

<span data-ttu-id="b7cf8-142">Nyní jste připraveni!</span><span class="sxs-lookup"><span data-stu-id="b7cf8-142">Now you are ready!</span></span> <span data-ttu-id="b7cf8-143">Pro přístup k všechny příkazy rozhraní příkazového řádku pro práci s vlastními prostředky [připojení k předplatnému Azure z rozhraní příkazového řádku Azure](xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-143">To access all the CLI commands to work with your own resources, [connect to your Azure subscription from the Azure CLI](xplat-cli-connect.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b7cf8-144">Při prvním použití Azure CLI, zobrazí se zpráva s dotazem, pokud chcete povolit Microsoftu shromažďovat informace o využití.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-144">When you first use Azure CLI, you see a message asking if you want to allow Microsoft to collect usage information.</span></span> <span data-ttu-id="b7cf8-145">Účast je dobrovolná.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-145">Participation is voluntary.</span></span> <span data-ttu-id="b7cf8-146">Pokud se rozhodnete pro účast můžete kdykoli zastavit spuštěním `azure telemetry --disable`.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-146">If you choose to participate, you can stop at any time by running `azure telemetry --disable`.</span></span> <span data-ttu-id="b7cf8-147">Chcete-li povolit účast kdykoli, spusťte `azure telemetry --enable`.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-147">To enable participation at any time, run `azure telemetry --enable`.</span></span>

## <a name="update-the-cli"></a><span data-ttu-id="b7cf8-148">Aktualizace rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b7cf8-148">Update the CLI</span></span>
<span data-ttu-id="b7cf8-149">Společnost Microsoft vydá často aktualizované verze rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-149">Microsoft frequently releases updated versions of the Azure CLI.</span></span> <span data-ttu-id="b7cf8-150">Přeinstalujte rozhraní příkazového řádku pomocí Instalační služby operačního systému nebo spustit nejnovější kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-150">Reinstall the CLI using the installer for your operating system, or run the latest Docker container.</span></span> <span data-ttu-id="b7cf8-151">Nebo, pokud máte nejnovější Node.js a npm nainstalován, aktualizace pomocí následujícího příkazu (na Linuxových distribucích možná budete muset použít **sudo**).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-151">Or, if you have the latest Node.js and npm installed, update by typing the following (on Linux distributions you might need to use **sudo**).</span></span>

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a><span data-ttu-id="b7cf8-152">Povolit dokončování pomocí tabulátorů</span><span class="sxs-lookup"><span data-stu-id="b7cf8-152">Enable tab completion</span></span>
<span data-ttu-id="b7cf8-153">Dokončování pomocí tabulátorů z rozhraní příkazového řádku je podporována pro Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-153">Tab completion of CLI commands is supported for Mac and Linux.</span></span>

<span data-ttu-id="b7cf8-154">Chcete-li povolit v zsh, spusťte:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-154">To enable it in zsh, run:</span></span>

```bash
echo '. <(azure --completion)' >> .zshrc
```

<span data-ttu-id="b7cf8-155">Chcete-li povolit v bash, spusťte:</span><span class="sxs-lookup"><span data-stu-id="b7cf8-155">To enable it in bash, run:</span></span>

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a><span data-ttu-id="b7cf8-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7cf8-156">Next steps</span></span>
* <span data-ttu-id="b7cf8-157">[Připojení k předplatnému Azure z rozhraní příkazového řádku](xplat-cli-connect.md) k vytváření a správě prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b7cf8-157">[Connect from the CLI to your Azure subscription](xplat-cli-connect.md) to create and manage Azure resources.</span></span>
* <span data-ttu-id="b7cf8-158">Další informace o Azure CLI, stažení zdrojový kód, problémy sestavy, nebo můžete přispět k projektu, najdete [úložiště GitHub pro rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-158">To learn more about the Azure CLI, download source code, report problems, or contribute to the project, visit the [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="b7cf8-159">Pokud máte dotazy týkající se používání rozhraní příkazového řádku Azure nebo Azure, přejděte [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="b7cf8-159">If you have questions about using the Azure CLI, or Azure, visit the [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
