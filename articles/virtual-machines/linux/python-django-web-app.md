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
# <a name="django-hello-world-web-app-on-a-linux-vm"></a>Django Hello World webové aplikace na virtuální počítač s Linuxem
> [!div class="op_single_selector"]
> * [Windows](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

V tomto kurzu se dozvíte, jak k hostování webu na základě Django v systému Linux v Azure Virtual Machines. V tomto kurzu předpokládáme žádné předchozí zkušenosti s Azure. Po dokončení tohoto kurzu, může mít jiné aplikace založené na rozhraní Django nahoru a běží v cloudu.

Naučte se:

* Nastavte virtuální počítač Azure na hostitele Django. I když tento kurz vysvětluje, jak to udělat pro **Linux**, můžete provést stejný pro virtuální počítač s Windows Server hostované v Azure. 
* Vytvořte novou aplikaci Django v systému Linux.

Tento kurz ukazuje, jak pro vytvoření základní webové aplikace Hello World. Aplikace je hostitelem virtuálního počítače Azure.

Na následujícím snímku obrazovky je vidět hotová aplikace:

![Okno prohlížeče zobrazí stránku Hello World v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a>Vytvoření a nastavení virtuálního počítače Azure na hostitele Django

1. Pokud chcete vytvořit virtuální počítač Azure s distribuční Ubuntu Server 14.04 LTS, najdete v části [na portálu Azure vytvořit virtuální počítač s Linuxem](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Můžete také zvolit ověřování hesla místo použití veřejného klíče SSH.
2. Chcete-li upravit skupinu zabezpečení sítě umožňující příchozí přenos HTTP na portu 80, přečtěte si téma [vytvoření skupin zabezpečení sítě na portálu Azure](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).
3. (Volitelné) Ve výchozím nastavení nový virtuální počítač nemá platný plně kvalifikovaný název domény (FQDN).  Vytvoření virtuálního počítače s plně kvalifikovaný název domény, najdete v části [v portálu Azure vytvořit plně kvalifikovaný název domény pro virtuální počítač s Windows](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tento krok se nevyžaduje pro dokončení tohoto kurzu.

## <a id="setup"></a>Nastavení vývojového prostředí
> [!NOTE]
> Pokud potřebujete nainstalovat Python nebo chcete použít knihovny klienta, najdete v článku [Průvodce instalací Python](../../python-how-to-install.md).

Virtuální počítač s Linuxem Ubuntu má Python 2.7 předinstalována, ale není součástí Apache nebo Django. Proveďte následující kroky pro připojení k virtuálnímu počítači a instalaci Apache a Django:

1. Otevřete nové okno terminálu.
2. Pro připojení k virtuálnímu počítači Azure, zadejte následující příkaz. Pokud jste nevytvořili plně kvalifikovaný název domény, můžete připojit pomocí veřejnou IP adresu, která se zobrazí ve virtuálním počítači souhrnné na portálu Azure.
   
       $ ssh yourusername@yourVmUrl
3. Chcete-li nainstalovat rozhraní Django, zadejte následující příkazy:
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. Chcete-li nainstalovat Apache s mod wsgi, zadejte následující příkaz:
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a>Vytvořit novou aplikaci Django
1. Použití SSH pro přístup k virtuálnímu počítači, otevřete okno terminálu, které jste použili v předchozí části.
2. Chcete-li vytvořit nový projekt Django, zadejte následující příkazy:
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   `django-admin.py` Skript generuje základní strukturu pro Django webových stránek:
   
   * `helloworld/manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.
   * `helloworld/helloworld/settings.py`má Django nastavení pro vaši aplikaci.
   * `helloworld/helloworld/urls.py`má kód mapování mezi každou adresu URL a jeho zobrazení.
3. V adresáři /var/www/helloworld/helloworld vytvořte nový soubor s názvem views.py. Tento soubor obsahuje zobrazení, který vykreslí stránku "hello, world". V editoru kódu zadejte následující příkazy:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Nahraďte obsah souboru urls.py pomocí následujících příkazů:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a>Nastavit Apache
1. Ve složce /etc/apache2/sites-available/helloworld.conf vytvoření konfiguračního souboru Apache virtuálního hostitele. Nastavte obsah na následující hodnoty. Nahraďte *yourVmName* skutečným názvem počítače používáte (například *pyubuntu*).
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. Pokud chcete aktivovat webu, použijte následující příkaz:
   
       $ sudo a2ensite helloworld
3. Pokud chcete restartovat Apache, použijte následující příkaz:
   
       $ sudo service apache2 reload
4. Načte webovou stránku v prohlížeči:
   
   ![Okno prohlížeče zobrazí stránku hello world v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a>Vypněte virtuální počítač Azure
Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure jste vytvořili pro tento kurz. Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.

