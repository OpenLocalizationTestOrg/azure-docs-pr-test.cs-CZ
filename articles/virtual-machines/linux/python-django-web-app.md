---
title: "aaaPython webové aplikace pomocí rozhraní Django ve virtuálním počítači Azure Linux | Microsoft Docs"
description: "Zjistěte, jak toohost a na základě Django webové aplikace v Azure pomocí virtuálního počítače s Linuxem."
services: virtual-machines-linux
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-resource-manager
ms.assetid: 00ad4c2c-4316-4f9a-913f-f7f49b158db7
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 520c47e19e8ffb4bb866f70772d506ddf76e242c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="django-hello-world-web-app-on-a-linux-vm"></a><span data-ttu-id="5901c-103">Django Hello World webové aplikace na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="5901c-103">Django Hello World web app on a Linux VM</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5901c-104">Windows</span><span class="sxs-lookup"><span data-stu-id="5901c-104">Windows</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [<span data-ttu-id="5901c-105">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="5901c-105">Mac/Linux</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

<span data-ttu-id="5901c-106">Tento kurz ukazuje, jak toohost web na základě Django v systému Linux v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="5901c-106">This tutorial shows you how toohost a Django-based website in Linux in Azure Virtual Machines.</span></span> <span data-ttu-id="5901c-107">V kurzu hello předpokládáme žádné předchozí zkušenosti s Azure.</span><span class="sxs-lookup"><span data-stu-id="5901c-107">In hello tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="5901c-108">Po dokončení kurzu hello může mít jiné aplikace založené na rozhraní Django nahoru a spouštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="5901c-108">When you finish hello tutorial, you can have a Django-based application up and running in hello cloud.</span></span>

<span data-ttu-id="5901c-109">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="5901c-109">Learn how to:</span></span>

* <span data-ttu-id="5901c-110">Nastavte virtuální počítač Azure toohost Django.</span><span class="sxs-lookup"><span data-stu-id="5901c-110">Set up an Azure virtual machine toohost Django.</span></span> <span data-ttu-id="5901c-111">I když tento kurz vysvětluje, jak toodo to pro **Linux**, můžete provést stejný hello pro virtuální počítač s Windows Server hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="5901c-111">Although this tutorial explains how toodo this for **Linux**, you can do hello same for a Windows Server VM hosted in Azure.</span></span> 
* <span data-ttu-id="5901c-112">Vytvořte novou aplikaci Django v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="5901c-112">Create a new Django application in Linux.</span></span>

<span data-ttu-id="5901c-113">Hello kurzu se dozvíte, jak toobuild základní Hello World webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5901c-113">hello tutorial shows you how toobuild a basic Hello World web application.</span></span> <span data-ttu-id="5901c-114">Hello aplikace je hostitelem virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="5901c-114">hello application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="5901c-115">Hello následující snímek obrazovky ukazuje hello dokončit aplikace:</span><span class="sxs-lookup"><span data-stu-id="5901c-115">hello following screenshot shows hello completed application:</span></span>

![Okno prohlížeče zobrazí stránku Hello World hello v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a><span data-ttu-id="5901c-117">Vytvoření a nastavení virtuální počítač Azure toohost Django</span><span class="sxs-lookup"><span data-stu-id="5901c-117">Create and set up an Azure virtual machine toohost Django</span></span>

1. <span data-ttu-id="5901c-118">toocreate virtuální počítač Azure s hello distribuci Ubuntu Server 14.04 LTS, najdete v části [vytvořit virtuální počítač s Linuxem v hello portál Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5901c-118">toocreate an Azure virtual machine with hello Ubuntu Server 14.04 LTS distribution, see [Create a Linux virtual machine in hello Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="5901c-119">Můžete také zvolit ověřování hesla místo použití veřejného klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="5901c-119">You also can choose password authentication instead of using an SSH public key.</span></span>
2. <span data-ttu-id="5901c-120">tooedit hello sítě zabezpečení skupiny tooallow příchozí HTTP provoz tooport 80, najdete v části [vytvoření skupin zabezpečení sítě v portálu Azure hello](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="5901c-120">tooedit hello network security group tooallow incoming HTTP traffic tooport 80, see [Create network security groups in hello Azure portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>
3. <span data-ttu-id="5901c-121">(Volitelné) Ve výchozím nastavení nový virtuální počítač nemá platný plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="5901c-121">(Optional) By default, your new virtual machine doesn't have a fully qualified domain name (FQDN).</span></span>  <span data-ttu-id="5901c-122">toocreate virtuální počítač s plně kvalifikovaný název domény, najdete v části [v hello portálu Azure vytvořit plně kvalifikovaný název domény pro virtuální počítač s Windows](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5901c-122">toocreate a VM with an FQDN, see [Create an FQDN in hello Azure portal for a Windows VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="5901c-123">Tento krok se nevyžaduje pro dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5901c-123">This step is not required for completing this tutorial.</span></span>

## <span data-ttu-id="5901c-124"><a id="setup"></a>Nastavit hello vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="5901c-124"><a id="setup"> </a>Set up hello development environment</span></span>
> [!NOTE]
> <span data-ttu-id="5901c-125">Pokud potřebujete tooinstall Python nebo chcete toouse hello klientské knihovny, přečtěte si téma hello [Průvodce instalací Python](../../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="5901c-125">If you need tooinstall Python or want toouse hello client libraries, see hello [Python installation guide](../../python-how-to-install.md).</span></span>

<span data-ttu-id="5901c-126">Hello virtuálního počítače s Ubuntu Linux má Python 2.7 předinstalována, ale není součástí Apache nebo Django.</span><span class="sxs-lookup"><span data-stu-id="5901c-126">hello Ubuntu Linux VM has Python 2.7 preinstalled, but it doesn't come with Apache or Django.</span></span> <span data-ttu-id="5901c-127">Dokončení hello následující kroky tooconnect tooyour virtuálních počítačů a nainstalujte Apache a Django:</span><span class="sxs-lookup"><span data-stu-id="5901c-127">Complete hello following steps tooconnect tooyour VM and install Apache and Django:</span></span>

1. <span data-ttu-id="5901c-128">Otevřete nové okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="5901c-128">Open a new Terminal window.</span></span>
2. <span data-ttu-id="5901c-129">tooconnect toohello virtuálního počítače Azure, zadejte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="5901c-129">tooconnect toohello Azure VM, enter hello following command.</span></span> <span data-ttu-id="5901c-130">Pokud jste nevytvořili plně kvalifikovaný název domény, můžete připojit pomocí hello veřejnou IP adresu, která se zobrazí ve virtuálním počítači hello souhrnu v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5901c-130">If you didn't create an FQDN, you can connect by using hello public IP address that's displayed in hello virtual machine summary in hello Azure portal.</span></span>
   
       $ ssh yourusername@yourVmUrl
3. <span data-ttu-id="5901c-131">tooinstall Django, zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5901c-131">tooinstall Django, enter hello following commands:</span></span>
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. <span data-ttu-id="5901c-132">tooinstall Apache s mod-wsgi, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5901c-132">tooinstall Apache with mod-wsgi, enter hello following command:</span></span>
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a><span data-ttu-id="5901c-133">Vytvořit novou aplikaci Django</span><span class="sxs-lookup"><span data-stu-id="5901c-133">Create a new Django app</span></span>
1. <span data-ttu-id="5901c-134">virtuální počítač, hello otevřete okno terminálu, kterou jste použili v předcházející části hello tooaccess SSH toouse.</span><span class="sxs-lookup"><span data-stu-id="5901c-134">toouse SSH tooaccess your VM, open hello Terminal window you used in hello preceding section.</span></span>
2. <span data-ttu-id="5901c-135">toocreate nový projekt Django, zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5901c-135">toocreate a new Django project, enter hello following commands:</span></span>
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   <span data-ttu-id="5901c-136">Hello `django-admin.py` skript generuje základní strukturu pro Django webových stránek:</span><span class="sxs-lookup"><span data-stu-id="5901c-136">hello `django-admin.py` script generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="5901c-137">`helloworld/manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.</span><span class="sxs-lookup"><span data-stu-id="5901c-137">`helloworld/manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="5901c-138">`helloworld/helloworld/settings.py`má Django nastavení pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5901c-138">`helloworld/helloworld/settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="5901c-139">`helloworld/helloworld/urls.py`má kód hello mapování mezi každou adresu URL a jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5901c-139">`helloworld/helloworld/urls.py` has hello mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="5901c-140">V adresáři /var/www/helloworld/helloworld hello vytvořte nový soubor s názvem views.py.</span><span class="sxs-lookup"><span data-stu-id="5901c-140">In hello /var/www/helloworld/helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="5901c-141">Tento soubor má hello zobrazení, která vykreslí stránku hello "hello, world".</span><span class="sxs-lookup"><span data-stu-id="5901c-141">This file has hello view that renders hello "hello world" page.</span></span> <span data-ttu-id="5901c-142">V editoru kódu zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5901c-142">In your code editor, enter hello following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="5901c-143">Nahraďte hello obsah souboru urls.py hello hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5901c-143">Replace hello contents of hello urls.py file with hello following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a><span data-ttu-id="5901c-144">Nastavit Apache</span><span class="sxs-lookup"><span data-stu-id="5901c-144">Set up Apache</span></span>
1. <span data-ttu-id="5901c-145">Ve složce /etc/apache2/sites-available/helloworld.conf hello vytvořte konfigurační soubor Apache virtuálního hostitele.</span><span class="sxs-lookup"><span data-stu-id="5901c-145">In hello /etc/apache2/sites-available/helloworld.conf folder, create an Apache virtual host configuration file.</span></span> <span data-ttu-id="5901c-146">Nastavit toohello obsah hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5901c-146">Set hello contents toohello following values.</span></span> <span data-ttu-id="5901c-147">Nahraďte *yourVmName* s hello skutečný název počítače hello používáte (například *pyubuntu*).</span><span class="sxs-lookup"><span data-stu-id="5901c-147">Replace *yourVmName* with hello actual name of hello machine you are using (for example, *pyubuntu*).</span></span>
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. <span data-ttu-id="5901c-148">tooactivate hello lokalita, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5901c-148">tooactivate hello site, use hello following command:</span></span>
   
       $ sudo a2ensite helloworld
3. <span data-ttu-id="5901c-149">toorestart Apache hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5901c-149">toorestart Apache, use hello following command:</span></span>
   
       $ sudo service apache2 reload
4. <span data-ttu-id="5901c-150">Načte hello webovou stránku v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="5901c-150">Load hello webpage in your browser:</span></span>
   
   ![Okno prohlížeče zobrazí hello hello world stránku v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="5901c-152">Vypněte virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="5901c-152">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="5901c-153">Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure, které jste vytvořili pro kurz hello hello.</span><span class="sxs-lookup"><span data-stu-id="5901c-153">When you're done with this tutorial, we recommend that you shut down or remove hello Azure VM you created for hello tutorial.</span></span> <span data-ttu-id="5901c-154">Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.</span><span class="sxs-lookup"><span data-stu-id="5901c-154">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

