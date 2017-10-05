---
title: "Python webové aplikace pomocí rozhraní Django ve virtuálním počítači Azure Linux | Microsoft Docs"
description: "Zjistěte, jak k hostování na základě Django webové aplikace v Azure pomocí virtuálního počítače s Linuxem."
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
ms.openlocfilehash: 6e2ab8c7da7496d0e2b567a4bdc9341adcf01552
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="django-hello-world-web-app-on-a-linux-vm"></a><span data-ttu-id="e5b67-103">Django Hello World webové aplikace na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e5b67-103">Django Hello World web app on a Linux VM</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5b67-104">Windows</span><span class="sxs-lookup"><span data-stu-id="e5b67-104">Windows</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [<span data-ttu-id="e5b67-105">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="e5b67-105">Mac/Linux</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

<span data-ttu-id="e5b67-106">V tomto kurzu se dozvíte, jak k hostování webu na základě Django v systému Linux v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="e5b67-106">This tutorial shows you how to host a Django-based website in Linux in Azure Virtual Machines.</span></span> <span data-ttu-id="e5b67-107">V tomto kurzu předpokládáme žádné předchozí zkušenosti s Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b67-107">In the tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="e5b67-108">Po dokončení tohoto kurzu, může mít jiné aplikace založené na rozhraní Django nahoru a běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e5b67-108">When you finish the tutorial, you can have a Django-based application up and running in the cloud.</span></span>

<span data-ttu-id="e5b67-109">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="e5b67-109">Learn how to:</span></span>

* <span data-ttu-id="e5b67-110">Nastavte virtuální počítač Azure na hostitele Django.</span><span class="sxs-lookup"><span data-stu-id="e5b67-110">Set up an Azure virtual machine to host Django.</span></span> <span data-ttu-id="e5b67-111">I když tento kurz vysvětluje, jak to udělat pro **Linux**, můžete provést stejný pro virtuální počítač s Windows Server hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b67-111">Although this tutorial explains how to do this for **Linux**, you can do the same for a Windows Server VM hosted in Azure.</span></span> 
* <span data-ttu-id="e5b67-112">Vytvořte novou aplikaci Django v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="e5b67-112">Create a new Django application in Linux.</span></span>

<span data-ttu-id="e5b67-113">Tento kurz ukazuje, jak pro vytvoření základní webové aplikace Hello World.</span><span class="sxs-lookup"><span data-stu-id="e5b67-113">The tutorial shows you how to build a basic Hello World web application.</span></span> <span data-ttu-id="e5b67-114">Aplikace je hostitelem virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b67-114">The application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="e5b67-115">Na následujícím snímku obrazovky je vidět hotová aplikace:</span><span class="sxs-lookup"><span data-stu-id="e5b67-115">The following screenshot shows the completed application:</span></span>

![Okno prohlížeče zobrazí stránku Hello World v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a><span data-ttu-id="e5b67-117">Vytvoření a nastavení virtuálního počítače Azure na hostitele Django</span><span class="sxs-lookup"><span data-stu-id="e5b67-117">Create and set up an Azure virtual machine to host Django</span></span>

1. <span data-ttu-id="e5b67-118">Pokud chcete vytvořit virtuální počítač Azure s distribuční Ubuntu Server 14.04 LTS, najdete v části [na portálu Azure vytvořit virtuální počítač s Linuxem](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5b67-118">To create an Azure virtual machine with the Ubuntu Server 14.04 LTS distribution, see [Create a Linux virtual machine in the Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e5b67-119">Můžete také zvolit ověřování hesla místo použití veřejného klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="e5b67-119">You also can choose password authentication instead of using an SSH public key.</span></span>
2. <span data-ttu-id="e5b67-120">Chcete-li upravit skupinu zabezpečení sítě umožňující příchozí přenos HTTP na portu 80, přečtěte si téma [vytvoření skupin zabezpečení sítě na portálu Azure](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="e5b67-120">To edit the network security group to allow incoming HTTP traffic to port 80, see [Create network security groups in the Azure portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>
3. <span data-ttu-id="e5b67-121">(Volitelné) Ve výchozím nastavení nový virtuální počítač nemá platný plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="e5b67-121">(Optional) By default, your new virtual machine doesn't have a fully qualified domain name (FQDN).</span></span>  <span data-ttu-id="e5b67-122">Vytvoření virtuálního počítače s plně kvalifikovaný název domény, najdete v části [v portálu Azure vytvořit plně kvalifikovaný název domény pro virtuální počítač s Windows](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5b67-122">To create a VM with an FQDN, see [Create an FQDN in the Azure portal for a Windows VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e5b67-123">Tento krok se nevyžaduje pro dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e5b67-123">This step is not required for completing this tutorial.</span></span>

## <span data-ttu-id="e5b67-124"><a id="setup"></a>Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="e5b67-124"><a id="setup"> </a>Set up the development environment</span></span>
> [!NOTE]
> <span data-ttu-id="e5b67-125">Pokud potřebujete nainstalovat Python nebo chcete použít knihovny klienta, najdete v článku [Průvodce instalací Python](../../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="e5b67-125">If you need to install Python or want to use the client libraries, see the [Python installation guide](../../python-how-to-install.md).</span></span>

<span data-ttu-id="e5b67-126">Virtuální počítač s Linuxem Ubuntu má Python 2.7 předinstalována, ale není součástí Apache nebo Django.</span><span class="sxs-lookup"><span data-stu-id="e5b67-126">The Ubuntu Linux VM has Python 2.7 preinstalled, but it doesn't come with Apache or Django.</span></span> <span data-ttu-id="e5b67-127">Proveďte následující kroky pro připojení k virtuálnímu počítači a instalaci Apache a Django:</span><span class="sxs-lookup"><span data-stu-id="e5b67-127">Complete the following steps to connect to your VM and install Apache and Django:</span></span>

1. <span data-ttu-id="e5b67-128">Otevřete nové okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e5b67-128">Open a new Terminal window.</span></span>
2. <span data-ttu-id="e5b67-129">Pro připojení k virtuálnímu počítači Azure, zadejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e5b67-129">To connect to the Azure VM, enter the following command.</span></span> <span data-ttu-id="e5b67-130">Pokud jste nevytvořili plně kvalifikovaný název domény, můžete připojit pomocí veřejnou IP adresu, která se zobrazí ve virtuálním počítači souhrnné na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b67-130">If you didn't create an FQDN, you can connect by using the public IP address that's displayed in the virtual machine summary in the Azure portal.</span></span>
   
       $ ssh yourusername@yourVmUrl
3. <span data-ttu-id="e5b67-131">Chcete-li nainstalovat rozhraní Django, zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e5b67-131">To install Django, enter the following commands:</span></span>
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. <span data-ttu-id="e5b67-132">Chcete-li nainstalovat Apache s mod wsgi, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b67-132">To install Apache with mod-wsgi, enter the following command:</span></span>
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a><span data-ttu-id="e5b67-133">Vytvořit novou aplikaci Django</span><span class="sxs-lookup"><span data-stu-id="e5b67-133">Create a new Django app</span></span>
1. <span data-ttu-id="e5b67-134">Použití SSH pro přístup k virtuálnímu počítači, otevřete okno terminálu, které jste použili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="e5b67-134">To use SSH to access your VM, open the Terminal window you used in the preceding section.</span></span>
2. <span data-ttu-id="e5b67-135">Chcete-li vytvořit nový projekt Django, zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e5b67-135">To create a new Django project, enter the following commands:</span></span>
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   <span data-ttu-id="e5b67-136">`django-admin.py` Skript generuje základní strukturu pro Django webových stránek:</span><span class="sxs-lookup"><span data-stu-id="e5b67-136">The `django-admin.py` script generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="e5b67-137">`helloworld/manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.</span><span class="sxs-lookup"><span data-stu-id="e5b67-137">`helloworld/manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="e5b67-138">`helloworld/helloworld/settings.py`má Django nastavení pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5b67-138">`helloworld/helloworld/settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="e5b67-139">`helloworld/helloworld/urls.py`má kód mapování mezi každou adresu URL a jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e5b67-139">`helloworld/helloworld/urls.py` has the mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="e5b67-140">V adresáři /var/www/helloworld/helloworld vytvořte nový soubor s názvem views.py.</span><span class="sxs-lookup"><span data-stu-id="e5b67-140">In the /var/www/helloworld/helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="e5b67-141">Tento soubor obsahuje zobrazení, který vykreslí stránku "hello, world".</span><span class="sxs-lookup"><span data-stu-id="e5b67-141">This file has the view that renders the "hello world" page.</span></span> <span data-ttu-id="e5b67-142">V editoru kódu zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e5b67-142">In your code editor, enter the following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="e5b67-143">Nahraďte obsah souboru urls.py pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e5b67-143">Replace the contents of the urls.py file with the following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a><span data-ttu-id="e5b67-144">Nastavit Apache</span><span class="sxs-lookup"><span data-stu-id="e5b67-144">Set up Apache</span></span>
1. <span data-ttu-id="e5b67-145">Ve složce /etc/apache2/sites-available/helloworld.conf vytvoření konfiguračního souboru Apache virtuálního hostitele.</span><span class="sxs-lookup"><span data-stu-id="e5b67-145">In the /etc/apache2/sites-available/helloworld.conf folder, create an Apache virtual host configuration file.</span></span> <span data-ttu-id="e5b67-146">Nastavte obsah na následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e5b67-146">Set the contents to the following values.</span></span> <span data-ttu-id="e5b67-147">Nahraďte *yourVmName* skutečným názvem počítače používáte (například *pyubuntu*).</span><span class="sxs-lookup"><span data-stu-id="e5b67-147">Replace *yourVmName* with the actual name of the machine you are using (for example, *pyubuntu*).</span></span>
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. <span data-ttu-id="e5b67-148">Pokud chcete aktivovat webu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b67-148">To activate the site, use the following command:</span></span>
   
       $ sudo a2ensite helloworld
3. <span data-ttu-id="e5b67-149">Pokud chcete restartovat Apache, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b67-149">To restart Apache, use the following command:</span></span>
   
       $ sudo service apache2 reload
4. <span data-ttu-id="e5b67-150">Načte webovou stránku v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="e5b67-150">Load the webpage in your browser:</span></span>
   
   ![Okno prohlížeče zobrazí stránku hello world v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="e5b67-152">Vypněte virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="e5b67-152">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="e5b67-153">Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure jste vytvořili pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="e5b67-153">When you're done with this tutorial, we recommend that you shut down or remove the Azure VM you created for the tutorial.</span></span> <span data-ttu-id="e5b67-154">Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b67-154">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

