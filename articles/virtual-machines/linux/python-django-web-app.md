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
# <a name="django-hello-world-web-app-on-a-linux-vm"></a>Django Hello World webové aplikace na virtuální počítač s Linuxem
> [!div class="op_single_selector"]
> * [Windows](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

Tento kurz ukazuje, jak toohost web na základě Django v systému Linux v Azure Virtual Machines. V kurzu hello předpokládáme žádné předchozí zkušenosti s Azure. Po dokončení kurzu hello může mít jiné aplikace založené na rozhraní Django nahoru a spouštění v cloudu hello.

Naučte se:

* Nastavte virtuální počítač Azure toohost Django. I když tento kurz vysvětluje, jak toodo to pro **Linux**, můžete provést stejný hello pro virtuální počítač s Windows Server hostované v Azure. 
* Vytvořte novou aplikaci Django v systému Linux.

Hello kurzu se dozvíte, jak toobuild základní Hello World webové aplikace. Hello aplikace je hostitelem virtuálního počítače Azure.

Hello následující snímek obrazovky ukazuje hello dokončit aplikace:

![Okno prohlížeče zobrazí stránku Hello World hello v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a>Vytvoření a nastavení virtuální počítač Azure toohost Django

1. toocreate virtuální počítač Azure s hello distribuci Ubuntu Server 14.04 LTS, najdete v části [vytvořit virtuální počítač s Linuxem v hello portál Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Můžete také zvolit ověřování hesla místo použití veřejného klíče SSH.
2. tooedit hello sítě zabezpečení skupiny tooallow příchozí HTTP provoz tooport 80, najdete v části [vytvoření skupin zabezpečení sítě v portálu Azure hello](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).
3. (Volitelné) Ve výchozím nastavení nový virtuální počítač nemá platný plně kvalifikovaný název domény (FQDN).  toocreate virtuální počítač s plně kvalifikovaný název domény, najdete v části [v hello portálu Azure vytvořit plně kvalifikovaný název domény pro virtuální počítač s Windows](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tento krok se nevyžaduje pro dokončení tohoto kurzu.

## <a id="setup"></a>Nastavit hello vývojového prostředí
> [!NOTE]
> Pokud potřebujete tooinstall Python nebo chcete toouse hello klientské knihovny, přečtěte si téma hello [Průvodce instalací Python](../../python-how-to-install.md).

Hello virtuálního počítače s Ubuntu Linux má Python 2.7 předinstalována, ale není součástí Apache nebo Django. Dokončení hello následující kroky tooconnect tooyour virtuálních počítačů a nainstalujte Apache a Django:

1. Otevřete nové okno terminálu.
2. tooconnect toohello virtuálního počítače Azure, zadejte následující příkaz hello. Pokud jste nevytvořili plně kvalifikovaný název domény, můžete připojit pomocí hello veřejnou IP adresu, která se zobrazí ve virtuálním počítači hello souhrnu v hello portálu Azure.
   
       $ ssh yourusername@yourVmUrl
3. tooinstall Django, zadejte hello následující příkazy:
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. tooinstall Apache s mod-wsgi, zadejte následující příkaz hello:
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a>Vytvořit novou aplikaci Django
1. virtuální počítač, hello otevřete okno terminálu, kterou jste použili v předcházející části hello tooaccess SSH toouse.
2. toocreate nový projekt Django, zadejte hello následující příkazy:
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   Hello `django-admin.py` skript generuje základní strukturu pro Django webových stránek:
   
   * `helloworld/manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.
   * `helloworld/helloworld/settings.py`má Django nastavení pro vaši aplikaci.
   * `helloworld/helloworld/urls.py`má kód hello mapování mezi každou adresu URL a jeho zobrazení.
3. V adresáři /var/www/helloworld/helloworld hello vytvořte nový soubor s názvem views.py. Tento soubor má hello zobrazení, která vykreslí stránku hello "hello, world". V editoru kódu zadejte hello následující příkazy:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Nahraďte hello obsah souboru urls.py hello hello následující příkazy:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a>Nastavit Apache
1. Ve složce /etc/apache2/sites-available/helloworld.conf hello vytvořte konfigurační soubor Apache virtuálního hostitele. Nastavit toohello obsah hello následující hodnoty. Nahraďte *yourVmName* s hello skutečný název počítače hello používáte (například *pyubuntu*).
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. tooactivate hello lokalita, hello použijte následující příkaz:
   
       $ sudo a2ensite helloworld
3. toorestart Apache hello použijte následující příkaz:
   
       $ sudo service apache2 reload
4. Načte hello webovou stránku v prohlížeči:
   
   ![Okno prohlížeče zobrazí hello hello world stránku v Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a>Vypněte virtuální počítač Azure
Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure, které jste vytvořili pro kurz hello hello. Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.

