---
title: "aaaDjango webové aplikace na virtuální počítač Windows serveru Azure | Microsoft Docs"
description: "Zjistěte, jak toohost webu na základě Django v Azure pomocí modelu nasazení classic hello virtuální počítač s Windows Server 2012 R2 Datacenter."
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 55847e3c6d6769965be29077e8d4eeebad914637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a>Django Hello World webové aplikace na virtuální počítač Windows serveru

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manageru a hello modelu nasazení classic](../../../resource-manager-deployment-model.md). Tento článek popisuje model nasazení classic hello. Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello.

Tento kurz ukazuje, jak toohost web na základě Django v systému Windows Server v Azure Virtual Machines. V kurzu hello předpokládáme žádné předchozí zkušenosti s Azure. Po dokončení kurzu hello může mít jiné aplikace založené na rozhraní Django nahoru a spouštění v cloudu hello.

Naučte se:

* Nastavte virtuální počítač Azure toohost Django. I když tento kurz vysvětluje, jak toodo to pro **systému Windows Server**, můžete provést stejný hello pro virtuální počítač Linux hostované v Azure.
* Vytvořte novou aplikaci Django v systému Windows.

Hello kurzu se dozvíte, jak toobuild základní Hello World webové aplikace. Hello aplikace je hostitelem virtuálního počítače Azure.

Hello následující snímek obrazovky ukazuje hello dokončit aplikace:

![Okno prohlížeče zobrazí hello hello world stránku v Azure][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a>Vytvoření a nastavení virtuální počítač Azure toohost Django

1. toocreate virtuální počítač Azure s hello distribuci systému Windows Server 2012 R2 Datacenter, najdete v části [vytvoření virtuálního počítače se systémem Windows v portálu Azure hello](tutorial.md).
2. Nastavte Azure toodirect port 80 provoz z hello webové tooport 80 hello virtuálního počítače:
   
   1. V hello portálu Azure přejděte toohello řídicího panelu a vyberte nově vytvořený virtuální počítač.
   2. Klikněte na **Koncové body** a potom na **Přidat**.

     ![Přidání koncového bodu](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. Na hello **přidání koncového bodu** stránky, pro **název**, zadejte **HTTP**. Nastavit hello veřejné a privátní porty TCP příliš**80**.

     ![Zadejte název a nastavte veřejné a privátní porty](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. Klikněte na **OK**.
     
3. V hello řídicí panel vyberte virtuální počítač. Klikněte na tlačítko toouse protokol RDP (Remote Desktop) tooremotely přihlašovací toohello nově vytvořený virtuální počítač Azure, **Connect**.  

> [!IMPORTANT] 
> Hello následující pokyny předpokládají, že jste přihlášení toohello virtuální počítač správně. Také se předpokládá, že k vydání příkazů v hello virtuálního počítače a není v místním počítači.

## <a id="setup"></a>Instalace Python, rozhraní Django a WFastCGI
> [!NOTE]
> toodownload v aplikaci Internet Explorer, můžete mít tooconfigure Internet Explorer **konfigurace rozšířeného zabezpečení aplikace** nastavení. toodo tento, klikněte na tlačítko **spustit** > **nástroje pro správu** > **správce serveru** > **místní Server**. Klikněte na tlačítko **konfigurace rozšířeného zabezpečení aplikace Internet Explorer**a potom vyberte **vypnout**.

1. Instalace nejnovější verze hello Python 2.7 nebo 3.4 Python z [python.org][python.org].
2. Instalovat balíčky wfastcgi a django hello pomocí nástroje pip.
   
    Pro Python 2.7 použijte následující příkaz hello:
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    Pro Python 3.4 použijte následující příkaz hello:
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a>Nainstalovat službu ISS s FastCGI
* Instalace Internetové informační služby (IIS) s podporou rozhraní FastCGI. Může to trvat několik minut tooexecute.
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a>Vytvořte novou aplikaci Django
1. V C:\inetpub\wwwroot toocreate nový projekt Django, zadejte následující příkaz hello:
   
   Pro Python 2.7 použijte následující příkaz hello:
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   Pro Python 3.4 použijte následující příkaz hello:
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![výsledek Hello příkazu hello New-AzureService](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. Hello `django-admin` příkaz generuje základní strukturu pro weby založené na rozhraní Django:
   
   * `helloworld\manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.
   * `helloworld\helloworld\settings.py`má Django nastavení pro vaši aplikaci.
   * `helloworld\helloworld\urls.py`má kód hello mapování mezi každou adresu URL a jeho zobrazení.
3. V adresáři C:\inetpub\wwwroot\helloworld\helloworld hello vytvořte nový soubor s názvem views.py. Tento soubor má hello zobrazení, která vykreslí stránku hello "hello, world". V editoru kódu zadejte hello následující příkazy:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Nahraďte hello obsah souboru urls.py hello hello následující příkazy:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a>Nastavení služby IIS
1. V souboru applicationhost.config globální hello odemkněte oddíl hello obslužné rutiny.  To umožňuje vaší obslužné web.config souborů toouse hello Python. Přidejte tento příkaz:
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. Aktivujte WFastCGI. Tento postup přidá souboru applicationhost.config globální toohello aplikace, který odkazuje tooyour překladač spustitelný soubor a hello wfastcgi.py skript v jazyce Python.
   
    V Python 2.7:
   
        C:\python27\scripts\wfastcgi-enable
   
    V Pythonu 3.4:
   
        C:\python34\scripts\wfastcgi-enable
3. V C:\inetpub\wwwroot\helloworld vytvořte soubor web.config. Hello hodnotu hello `scriptProcessor` atribut by měl odpovídat hello výstup hello předchozím kroku. Další informace o nastavení wfastcgi hello najdete v tématu [úložiště pypi wfastcgi][wfastcgi].
   
   V Python 2.7:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   V Pythonu 3.4:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. Aktualizace umístění hello hello IIS výchozí web toopoint toohello Django složky projektu:
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. Načtěte hello webovou stránku v prohlížeči.

![Okno prohlížeče zobrazí hello hello world stránky v Azure][1]

## <a name="shut-down-your-azure-virtual-machine"></a>Vypněte virtuální počítač Azure
Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure, které jste vytvořili pro kurz hello hello. Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
