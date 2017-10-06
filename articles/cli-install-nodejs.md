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
# <a name="install-hello-azure-cli-10"></a>Nainstalujte hello Azure CLI 1.0
> [!div class="op_single_selector"]
> * [PowerShell](/powershell/azure/overview)
> * [Azure CLI 1.0](cli-install-nodejs.md)
> * [Azure CLI 2.0](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> Toto téma popisuje, jakým způsobem volá tooinstall hello 1.0 rozhraní příkazového řádku Azure, který je založený na nodeJs a podporuje všechny rozhraní API nasazení classic a také velký počet aktivity nasazení Resource Manager. Měli byste použít hello [Azure CLI 2.0](/cli/azure/overview) pro správu a nasazení nových nebo budoucnost rozhraní příkazového řádku.

Rychle nainstalujte rozhraní příkazového řádku Azure (Azure CLI 1.0) toouse hello sadu příkazů open source založený na prostředí pro vytváření a správu prostředků v Microsoft Azure. Několik možností tooinstall máte ve vašem počítači těchto nástrojů pro různé platformy:

* **balíčku npm** – spuštění npm (hello Správce balíčků pro JavaScript) tooinstall hello nejnovější Azure CLI 1.0 balíček na distribuční Linux nebo OS. Vyžaduje node.js a npm ve vašem počítači.
* **Instalační program** -stáhnout instalační program pro instalaci snadno na Mac nebo Windows.
* **Kontejner docker** – začít používat hello nejnovější rozhraní příkazového řádku v kontejner Docker připravené k použití. Vyžaduje hostitelů Docker v počítači.

Další možnosti a pozadí, najdete v části hello projektu úložiště na [Githubu](https://github.com/azure/azure-xplat-cli).

Po instalaci hello Azure CLI 1.0 [připojit ke svému předplatnému Azure](xplat-cli-connect.md) a spuštění hello **azure** příkazy z vaší rozhraní příkazového řádku (Bash, terminálu, příkazový řádek a tak dále) toowork s vašich prostředků Azure.

## <a name="option-1-install-an-npm-package"></a>Možnost 1: Instalace balíčku npm
tooinstall hello rozhraní příkazového řádku z balíčku npm, zajistěte, aby byly staženy a nainstalovány hello [nejnovější Node.js a npm](https://nodejs.org/en/download/package-manager/). Potom spusťte **npm nainstalujte** tooinstall hello rozhraní příkazového řádku azure balíčku:

```bash
npm install -g azure-cli
```

Na Linuxových distribucích, může být nutné toouse **sudo** toosuccessfully spustit hello **npm** příkaz takto:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Pokud potřebujete tooinstall nebo aktualizovat Node.js a npm na operačního systému nebo distribuce systému Linux, doporučujeme nainstalovat poslední Node.js LTS verze hello (4.x). Pokud používáte starší verzi, může docházet k chybám instalace.

Pokud dáváte přednost, stáhněte si nejnovější Linux hello [vkládání souborů] [ linux-installer] hello npm balíček místně. Potom nainstalovat balíček stažené npm hello následujícím způsobem (na Linuxových distribucích může být nutné toouse **sudo**):

```bash
npm install -g <path toodownloaded tar file>
```

## <a name="option-2-use-an-installer"></a>Možnost 2: Použít instalační program
Pokud používáte počítač Mac nebo Windows, jsou k dispozici ke stažení hello následující instalační programy rozhraní příkazového řádku:

* [Instalátor systému Mac OS X][mac-installer]
* [Windows MSI][windows-installer]

> [!TIP]
> V systému Windows, můžete také stáhnout hello [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9828653) tooinstall hello rozhraní příkazového řádku. Tento instalační program poskytuje hello možnost tooinstall další Azure SDK a nástroje příkazového řádku po instalaci hello rozhraní příkazového řádku.

## <a name="option-3-use-a-docker-container"></a>Možnost 3: Použití kontejner Docker
Pokud jste nastavili počítač jako [Docker](https://docs.docker.com/engine/understanding-docker/) hostitele, můžete spustit hello nejnovější Azure CLI 1.0 v kontejner Docker. Spuštění hello následující příkaz (na Linuxových distribucích může být nutné toouse **sudo**):

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a>Příkazy spouštějte Azure CLI 1.0
Po instalaci hello Azure CLI 1.0, spusťte hello **azure** příkazu z příkazového řádku uživatelského rozhraní (Bash, terminálu, příkazový řádek a tak dále). Například toorun hello nápovědy příkazu, zadejte následující hello:

```azurecli
azure help
```

> [!NOTE]
> V některých distribucích systému Linux můžete obdržet chybu podobné příliš`/usr/bin/env: ‘node’: No such file or directory`. Tato chyba pochází z poslední instalace instaluje na /usr/bin/nodejs Node.js. toofix, vytvořit symbolický odkaz příliš/usr/bin/uzlu tak, že spustíte tento příkaz:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

verze hello toosee hello Azure CLI 1.0 jste nainstalovali, typ hello následující:

```azurecli
azure --version
```

Nyní jste připraveni! všechny tooaccess hello toowork příkazy rozhraní příkazového řádku s vlastní prostředky [připojit tooyour předplatného Azure z rozhraní příkazového řádku Azure hello](xplat-cli-connect.md).

> [!NOTE]
> Při prvním použití Azure CLI, zobrazí se zpráva s dotazem, pokud chcete, aby tooallow informace o využití toocollect Microsoft. Účast je dobrovolná. Pokud si zvolíte tooparticipate, můžete kdykoli zastavit spuštěním `azure telemetry --disable`. účast tooenable kdykoli spustit `azure telemetry --enable`.

## <a name="update-hello-cli"></a>Aktualizace hello rozhraní příkazového řádku
Společnost Microsoft vydá často aktualizované verze hello rozhraní příkazového řádku Azure. Přeinstalujte hello rozhraní příkazového řádku pomocí instalačního programu hello operačního systému, nebo spusťte kontejner Docker nejnovější hello. Nebo, pokud mají hello nejnovější Node.js a npm nainstalovaná, aktualizovat tak, že zadáte následující hello (na Linuxových distribucích může být nutné toouse **sudo**).

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Povolit dokončování pomocí tabulátorů
Dokončování pomocí tabulátorů z rozhraní příkazového řádku je podporována pro Mac a Linux.

tooenable v zsh, spusťte:

```bash
echo '. <(azure --completion)' >> .zshrc
```

tooenable v bash, spusťte:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Další kroky
* [Připojení z rozhraní příkazového řádku tooyour hello předplatného Azure](xplat-cli-connect.md) toocreate a správu prostředků Azure.
* toolearn Další informace o hello rozhraní příkazového řádku Azure, zdrojový kód, odesílat zprávy o problémech, nebo přispívat toohello projekt stáhnete na adrese hello [úložiště GitHub pro hello rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).
* Pokud máte dotazy týkající se používání hello rozhraní příkazového řádku Azure nebo Azure, navštivte hello [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
