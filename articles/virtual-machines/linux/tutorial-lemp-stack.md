---
title: "aaaDeploy LEMP na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Kurz – zásobník LEMP hello instalace na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: d8f9d84c5e9c0df4e9e985c10fe10f63a2f88214
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a><span data-ttu-id="a85c7-103">Nainstalujte LEMP webový server na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="a85c7-103">Install a LEMP web server on an Azure VM</span></span>
<span data-ttu-id="a85c7-104">Tento článek vás provede jak toodeploy NGINX webový server, MySQL a PHP (hello LEMP stack) na virtuálního počítače s Ubuntu v Azure.</span><span class="sxs-lookup"><span data-stu-id="a85c7-104">This article walks you through how toodeploy an NGINX web server, MySQL, and PHP (hello LEMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="a85c7-105">Zásobník LEMP Hello je alternativní toohello oblíbených [svítilna zásobníku](tutorial-lamp-stack.md), které si můžete také nainstalovat v Azure.</span><span class="sxs-lookup"><span data-stu-id="a85c7-105">hello LEMP stack is an alternative toohello popular [LAMP stack](tutorial-lamp-stack.md), which you can also install in Azure.</span></span> <span data-ttu-id="a85c7-106">toosee hello LEMP serveru v akci, můžete volitelně nainstalovat a nakonfigurovat web WordPress.</span><span class="sxs-lookup"><span data-stu-id="a85c7-106">toosee hello LEMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="a85c7-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="a85c7-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a85c7-108">Vytvoření virtuálního počítače s Ubuntu (hello "L" v zásobníku LEMP hello)</span><span class="sxs-lookup"><span data-stu-id="a85c7-108">Create an Ubuntu VM (hello 'L' in hello LEMP stack)</span></span>
> * <span data-ttu-id="a85c7-109">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="a85c7-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="a85c7-110">Instalace NGINX, MySQL a PHP</span><span class="sxs-lookup"><span data-stu-id="a85c7-110">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="a85c7-111">Ověření instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="a85c7-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="a85c7-112">Instalace aplikace WordPress na serveru LEMP hello</span><span class="sxs-lookup"><span data-stu-id="a85c7-112">Install WordPress on hello LEMP server</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a85c7-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a85c7-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a85c7-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="a85c7-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a85c7-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a85c7-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a><span data-ttu-id="a85c7-116">Instalace NGINX, MySQL a PHP</span><span class="sxs-lookup"><span data-stu-id="a85c7-116">Install NGINX, MySQL, and PHP</span></span>

<span data-ttu-id="a85c7-117">Spusťte následující příkaz tooupdate Ubuntu balíček zdroje hello a nainstalujte NGINX, MySQL a PHP.</span><span class="sxs-lookup"><span data-stu-id="a85c7-117">Run hello following command tooupdate Ubuntu package sources and install NGINX, MySQL, and PHP.</span></span> 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

<span data-ttu-id="a85c7-118">Jste výzvami tooinstall hello balíčky a další závislosti.</span><span class="sxs-lookup"><span data-stu-id="a85c7-118">You are prompted tooinstall hello packages and other dependencies.</span></span> <span data-ttu-id="a85c7-119">Po zobrazení výzvy nastavte kořenové heslo pro MySQL a potom toocontinue [Enter].</span><span class="sxs-lookup"><span data-stu-id="a85c7-119">When prompted, set a root password for MySQL, and then [Enter] toocontinue.</span></span> <span data-ttu-id="a85c7-120">Postupujte podle zbývajících pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="a85c7-120">Follow hello remaining prompts.</span></span> <span data-ttu-id="a85c7-121">Tento proces nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL.</span><span class="sxs-lookup"><span data-stu-id="a85c7-121">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![Stránka MySQL kořenového hesla][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="a85c7-123">Ověření instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="a85c7-123">Verify installation and configuration</span></span>


### <a name="nginx"></a><span data-ttu-id="a85c7-124">NGINX</span><span class="sxs-lookup"><span data-stu-id="a85c7-124">NGINX</span></span>

<span data-ttu-id="a85c7-125">Zkontrolujte verzi hello NGINX s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a85c7-125">Check hello version of NGINX with hello following command:</span></span>
```bash
nginx -v
```

<span data-ttu-id="a85c7-126">S NGINX nainstalovaná a port 80 otevřete tooyour virtuálních počítačů, hello webový server je nyní přístupná z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="a85c7-126">With NGINX installed, and port 80 open tooyour VM, hello web server can now be accessed from hello internet.</span></span> <span data-ttu-id="a85c7-127">tooview hello NGINX úvodní stránku, otevřete webový prohlížeč a zadejte hello veřejnou IP adresu hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a85c7-127">tooview hello NGINX welcome page, open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="a85c7-128">Použijte hello veřejnou IP adresu použitou tooSSH toohello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="a85c7-128">Use hello public IP address you used tooSSH toohello VM:</span></span>

![NGINX výchozí stránka][3]


### <a name="mysql"></a><span data-ttu-id="a85c7-130">MySQL</span><span class="sxs-lookup"><span data-stu-id="a85c7-130">MySQL</span></span>

<span data-ttu-id="a85c7-131">Zkontrolujte verzi hello MySQL s hello následující příkaz (Všimněte si, velké hello `V` parametr):</span><span class="sxs-lookup"><span data-stu-id="a85c7-131">Check hello version of MySQL with hello following command (note hello capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="a85c7-132">Doporučujeme spustit následující skript toohelp zabezpečené hello instalaci MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="a85c7-132">We recommend running hello following script toohelp secure hello installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="a85c7-133">Zadejte své heslo kořenové MySQL a konfigurovat nastavení zabezpečení hello pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="a85c7-133">Enter your MySQL root password, and configure hello security settings for your environment.</span></span>

<span data-ttu-id="a85c7-134">Pokud chcete toocreate databázi MySQL, přidat uživatele nebo změnit nastavení konfigurace, tooMySQL přihlášení:</span><span class="sxs-lookup"><span data-stu-id="a85c7-134">If you want toocreate a MySQL database, add users, or change configuration settings, login tooMySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="a85c7-135">Až budete hotoví, ukončete hello mysql řádku zadáním `\q`.</span><span class="sxs-lookup"><span data-stu-id="a85c7-135">When done, exit hello mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="a85c7-136">PHP</span><span class="sxs-lookup"><span data-stu-id="a85c7-136">PHP</span></span>

<span data-ttu-id="a85c7-137">Zkontrolujte hello verzi PHP s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a85c7-137">Check hello version of PHP with hello following command:</span></span>

```bash
php -v
```

<span data-ttu-id="a85c7-138">Nakonfigurujte NGINX toouse hello správce proces FastCGI PHP (PHP paměť FPM).</span><span class="sxs-lookup"><span data-stu-id="a85c7-138">Configure NGINX toouse hello PHP FastCGI Process Manager (PHP-FPM).</span></span> <span data-ttu-id="a85c7-139">Spusťte následující příkazy tooback původní server NGINX hello blokovat konfiguračního souboru a pak upravte hello původní soubor v editoru podle svého výběru hello:</span><span class="sxs-lookup"><span data-stu-id="a85c7-139">Run hello following commands tooback up hello original NGINX server block config file and then edit hello original file in an editor of your choice:</span></span>

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

<span data-ttu-id="a85c7-140">V editoru hello nahraďte obsah hello `/etc/nginx/sites-available/default` s následující hello.</span><span class="sxs-lookup"><span data-stu-id="a85c7-140">In hello editor, replace hello contents of `/etc/nginx/sites-available/default` with hello following.</span></span> <span data-ttu-id="a85c7-141">Viz hello poznámek vysvětlení hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="a85c7-141">See hello comments for explanation of hello settings.</span></span> <span data-ttu-id="a85c7-142">Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače pro *yourPublicIPAddress*a nechat hello zbývající nastavení.</span><span class="sxs-lookup"><span data-stu-id="a85c7-142">Substitute hello public IP address of your VM for *yourPublicIPAddress*, and leave hello remaining settings.</span></span> <span data-ttu-id="a85c7-143">Pak hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="a85c7-143">Then save hello file.</span></span>

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

<span data-ttu-id="a85c7-144">Zkontrolujte konfiguraci NGINX hello chyby syntaxe:</span><span class="sxs-lookup"><span data-stu-id="a85c7-144">Check hello NGINX configuration for syntax errors:</span></span>

```bash
sudo nginx -t
```

<span data-ttu-id="a85c7-145">Pokud je správná syntaxe hello, restartujte NGINX s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a85c7-145">If hello syntax is correct, restart NGINX with hello following command:</span></span>

```bash
sudo service nginx restart
```

<span data-ttu-id="a85c7-146">Pokud chcete další tootest, vytvořte rychlé tooview PHP informace o stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a85c7-146">If you want tootest further, create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="a85c7-147">Hello následující příkaz vytvoří stránku s informacemi o PHP hello:</span><span class="sxs-lookup"><span data-stu-id="a85c7-147">hello following command creates hello PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



<span data-ttu-id="a85c7-148">Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a85c7-148">Now you can check hello PHP info page you created.</span></span> <span data-ttu-id="a85c7-149">Otevřete prohlížeč a přejděte příliš`http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="a85c7-149">Open a browser and go too`http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="a85c7-150">Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a85c7-150">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="a85c7-151">Měl by vypadat podobně jako toothis bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a85c7-151">It should look similar toothis image.</span></span>

![Stránka informace o PHP][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a><span data-ttu-id="a85c7-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a85c7-153">Next steps</span></span>

<span data-ttu-id="a85c7-154">V tomto kurzu jste nasadili server LEMP v Azure.</span><span class="sxs-lookup"><span data-stu-id="a85c7-154">In this tutorial, you deployed a LEMP server in Azure.</span></span> <span data-ttu-id="a85c7-155">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="a85c7-155">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a85c7-156">Vytvoření virtuálního počítače s Ubuntu</span><span class="sxs-lookup"><span data-stu-id="a85c7-156">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="a85c7-157">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="a85c7-157">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="a85c7-158">Instalace NGINX, MySQL a PHP</span><span class="sxs-lookup"><span data-stu-id="a85c7-158">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="a85c7-159">Ověření instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="a85c7-159">Verify installation and configuration</span></span>
> * <span data-ttu-id="a85c7-160">Instalace aplikace WordPress v zásobníku LEMP hello</span><span class="sxs-lookup"><span data-stu-id="a85c7-160">Install WordPress on hello LEMP stack</span></span>

<span data-ttu-id="a85c7-161">Jak zálohy další kurz toolearn toohello toosecure webových serverů s certifikáty protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="a85c7-161">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a85c7-162">Zabezpečení webového serveru pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="a85c7-162">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
