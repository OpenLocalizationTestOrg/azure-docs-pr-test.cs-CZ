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
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a><span data-ttu-id="65ce4-103">Django Hello World webové aplikace na virtuální počítač Windows serveru</span><span class="sxs-lookup"><span data-stu-id="65ce4-103">Django Hello World web app on a Windows Server VM</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="65ce4-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manageru a hello modelu nasazení classic](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="65ce4-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and hello classic deployment model](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="65ce4-105">Tento článek popisuje model nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="65ce4-105">This article describes hello classic deployment model.</span></span> <span data-ttu-id="65ce4-106">Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="65ce4-106">We recommend that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="65ce4-107">Tento kurz ukazuje, jak toohost web na základě Django v systému Windows Server v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="65ce4-107">This tutorial shows you how toohost a Django-based website in Windows Server in Azure Virtual Machines.</span></span> <span data-ttu-id="65ce4-108">V kurzu hello předpokládáme žádné předchozí zkušenosti s Azure.</span><span class="sxs-lookup"><span data-stu-id="65ce4-108">In hello tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="65ce4-109">Po dokončení kurzu hello může mít jiné aplikace založené na rozhraní Django nahoru a spouštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="65ce4-109">When you finish hello tutorial, you can have a Django-based application up and running in hello cloud.</span></span>

<span data-ttu-id="65ce4-110">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="65ce4-110">Learn how to:</span></span>

* <span data-ttu-id="65ce4-111">Nastavte virtuální počítač Azure toohost Django.</span><span class="sxs-lookup"><span data-stu-id="65ce4-111">Set up an Azure virtual machine toohost Django.</span></span> <span data-ttu-id="65ce4-112">I když tento kurz vysvětluje, jak toodo to pro **systému Windows Server**, můžete provést stejný hello pro virtuální počítač Linux hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="65ce4-112">Although this tutorial explains how toodo this for **Windows Server**, you can do hello same for a Linux VM hosted in Azure.</span></span>
* <span data-ttu-id="65ce4-113">Vytvořte novou aplikaci Django v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="65ce4-113">Create a new Django application in Windows.</span></span>

<span data-ttu-id="65ce4-114">Hello kurzu se dozvíte, jak toobuild základní Hello World webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="65ce4-114">hello tutorial shows you how toobuild a basic Hello World web application.</span></span> <span data-ttu-id="65ce4-115">Hello aplikace je hostitelem virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="65ce4-115">hello application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="65ce4-116">Hello následující snímek obrazovky ukazuje hello dokončit aplikace:</span><span class="sxs-lookup"><span data-stu-id="65ce4-116">hello following screenshot shows hello completed application:</span></span>

![Okno prohlížeče zobrazí hello hello world stránku v Azure][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a><span data-ttu-id="65ce4-118">Vytvoření a nastavení virtuální počítač Azure toohost Django</span><span class="sxs-lookup"><span data-stu-id="65ce4-118">Create and set up an Azure virtual machine toohost Django</span></span>

1. <span data-ttu-id="65ce4-119">toocreate virtuální počítač Azure s hello distribuci systému Windows Server 2012 R2 Datacenter, najdete v části [vytvoření virtuálního počítače se systémem Windows v portálu Azure hello](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="65ce4-119">toocreate an Azure virtual machine with hello Windows Server 2012 R2 Datacenter distribution, see [Create a virtual machine running Windows in hello Azure portal](tutorial.md).</span></span>
2. <span data-ttu-id="65ce4-120">Nastavte Azure toodirect port 80 provoz z hello webové tooport 80 hello virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="65ce4-120">Set Azure toodirect port 80 traffic from hello web tooport 80 on hello virtual machine:</span></span>
   
   1. <span data-ttu-id="65ce4-121">V hello portálu Azure přejděte toohello řídicího panelu a vyberte nově vytvořený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="65ce4-121">In hello Azure portal, go toohello dashboard and select your newly created virtual machine.</span></span>
   2. <span data-ttu-id="65ce4-122">Klikněte na **Koncové body** a potom na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-122">Click **Endpoints**, and then click **Add**.</span></span>

     ![Přidání koncového bodu](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. <span data-ttu-id="65ce4-124">Na hello **přidání koncového bodu** stránky, pro **název**, zadejte **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-124">On hello **Add endpoint** page, for **Name**, enter **HTTP**.</span></span> <span data-ttu-id="65ce4-125">Nastavit hello veřejné a privátní porty TCP příliš**80**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-125">Set hello public and private TCP ports too**80**.</span></span>

     ![Zadejte název a nastavte veřejné a privátní porty](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. <span data-ttu-id="65ce4-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-127">Click **OK**.</span></span>
     
3. <span data-ttu-id="65ce4-128">V hello řídicí panel vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="65ce4-128">In hello dashboard, select your VM.</span></span> <span data-ttu-id="65ce4-129">Klikněte na tlačítko toouse protokol RDP (Remote Desktop) tooremotely přihlašovací toohello nově vytvořený virtuální počítač Azure, **Connect**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-129">toouse Remote Desktop Protocol (RDP) tooremotely sign in toohello newly created Azure virtual machine, click **Connect**.</span></span>  

> [!IMPORTANT] 
> <span data-ttu-id="65ce4-130">Hello následující pokyny předpokládají, že jste přihlášení toohello virtuální počítač správně.</span><span class="sxs-lookup"><span data-stu-id="65ce4-130">hello following instructions assume that you signed in toohello virtual machine correctly.</span></span> <span data-ttu-id="65ce4-131">Také se předpokládá, že k vydání příkazů v hello virtuálního počítače a není v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="65ce4-131">They also assume that you are issuing commands in hello virtual machine and not on your local computer.</span></span>

## <span data-ttu-id="65ce4-132"><a id="setup"></a>Instalace Python, rozhraní Django a WFastCGI</span><span class="sxs-lookup"><span data-stu-id="65ce4-132"><a id="setup"> </a>Install Python, Django, and WFastCGI</span></span>
> [!NOTE]
> <span data-ttu-id="65ce4-133">toodownload v aplikaci Internet Explorer, můžete mít tooconfigure Internet Explorer **konfigurace rozšířeného zabezpečení aplikace** nastavení.</span><span class="sxs-lookup"><span data-stu-id="65ce4-133">toodownload by using Internet Explorer, you might have tooconfigure Internet Explorer **Enhanced Security Configuration** settings.</span></span> <span data-ttu-id="65ce4-134">toodo tento, klikněte na tlačítko **spustit** > **nástroje pro správu** > **správce serveru** > **místní Server**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-134">toodo this, click **Start** > **Administrative Tools** > **Server Manager** > **Local Server**.</span></span> <span data-ttu-id="65ce4-135">Klikněte na tlačítko **konfigurace rozšířeného zabezpečení aplikace Internet Explorer**a potom vyberte **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="65ce4-135">Click **IE Enhanced Security Configuration**, and then select **Off**.</span></span>

1. <span data-ttu-id="65ce4-136">Instalace nejnovější verze hello Python 2.7 nebo 3.4 Python z [python.org][python.org].</span><span class="sxs-lookup"><span data-stu-id="65ce4-136">Install hello latest versions of Python 2.7 or Python 3.4 from [python.org][python.org].</span></span>
2. <span data-ttu-id="65ce4-137">Instalovat balíčky wfastcgi a django hello pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="65ce4-137">Install hello wfastcgi and django packages using pip.</span></span>
   
    <span data-ttu-id="65ce4-138">Pro Python 2.7 použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="65ce4-138">For Python 2.7, use hello following command:</span></span>
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    <span data-ttu-id="65ce4-139">Pro Python 3.4 použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="65ce4-139">For Python 3.4, use hello following command:</span></span>
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a><span data-ttu-id="65ce4-140">Nainstalovat službu ISS s FastCGI</span><span class="sxs-lookup"><span data-stu-id="65ce4-140">Install IIS with FastCGI</span></span>
* <span data-ttu-id="65ce4-141">Instalace Internetové informační služby (IIS) s podporou rozhraní FastCGI.</span><span class="sxs-lookup"><span data-stu-id="65ce4-141">Install Internet Information Services (IIS) with FastCGI support.</span></span> <span data-ttu-id="65ce4-142">Může to trvat několik minut tooexecute.</span><span class="sxs-lookup"><span data-stu-id="65ce4-142">This might take several minutes tooexecute.</span></span>
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a><span data-ttu-id="65ce4-143">Vytvořte novou aplikaci Django</span><span class="sxs-lookup"><span data-stu-id="65ce4-143">Create a new Django application</span></span>
1. <span data-ttu-id="65ce4-144">V C:\inetpub\wwwroot toocreate nový projekt Django, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="65ce4-144">In C:\inetpub\wwwroot, toocreate a new Django project, enter hello following command:</span></span>
   
   <span data-ttu-id="65ce4-145">Pro Python 2.7 použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="65ce4-145">For Python 2.7, use hello following command:</span></span>
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   <span data-ttu-id="65ce4-146">Pro Python 3.4 použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="65ce4-146">For Python 3.4, use hello following command:</span></span>
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![výsledek Hello příkazu hello New-AzureService](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. <span data-ttu-id="65ce4-148">Hello `django-admin` příkaz generuje základní strukturu pro weby založené na rozhraní Django:</span><span class="sxs-lookup"><span data-stu-id="65ce4-148">hello `django-admin` command generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="65ce4-149">`helloworld\manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.</span><span class="sxs-lookup"><span data-stu-id="65ce4-149">`helloworld\manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="65ce4-150">`helloworld\helloworld\settings.py`má Django nastavení pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="65ce4-150">`helloworld\helloworld\settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="65ce4-151">`helloworld\helloworld\urls.py`má kód hello mapování mezi každou adresu URL a jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="65ce4-151">`helloworld\helloworld\urls.py` has hello mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="65ce4-152">V adresáři C:\inetpub\wwwroot\helloworld\helloworld hello vytvořte nový soubor s názvem views.py.</span><span class="sxs-lookup"><span data-stu-id="65ce4-152">In hello C:\inetpub\wwwroot\helloworld\helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="65ce4-153">Tento soubor má hello zobrazení, která vykreslí stránku hello "hello, world".</span><span class="sxs-lookup"><span data-stu-id="65ce4-153">This file has hello view that renders hello "hello world" page.</span></span> <span data-ttu-id="65ce4-154">V editoru kódu zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="65ce4-154">In your code editor, enter hello following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="65ce4-155">Nahraďte hello obsah souboru urls.py hello hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="65ce4-155">Replace hello contents of hello urls.py file with hello following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a><span data-ttu-id="65ce4-156">Nastavení služby IIS</span><span class="sxs-lookup"><span data-stu-id="65ce4-156">Set up IIS</span></span>
1. <span data-ttu-id="65ce4-157">V souboru applicationhost.config globální hello odemkněte oddíl hello obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="65ce4-157">In hello global applicationhost.config file, unlock hello handlers section.</span></span>  <span data-ttu-id="65ce4-158">To umožňuje vaší obslužné web.config souborů toouse hello Python.</span><span class="sxs-lookup"><span data-stu-id="65ce4-158">This allows your web.config file toouse hello Python handler.</span></span> <span data-ttu-id="65ce4-159">Přidejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="65ce4-159">Add this command:</span></span>
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. <span data-ttu-id="65ce4-160">Aktivujte WFastCGI.</span><span class="sxs-lookup"><span data-stu-id="65ce4-160">Activate WFastCGI.</span></span> <span data-ttu-id="65ce4-161">Tento postup přidá souboru applicationhost.config globální toohello aplikace, který odkazuje tooyour překladač spustitelný soubor a hello wfastcgi.py skript v jazyce Python.</span><span class="sxs-lookup"><span data-stu-id="65ce4-161">This adds an application toohello global applicationhost.config file, which refers tooyour Python interpreter executable and hello wfastcgi.py script.</span></span>
   
    <span data-ttu-id="65ce4-162">V Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="65ce4-162">In Python 2.7:</span></span>
   
        C:\python27\scripts\wfastcgi-enable
   
    <span data-ttu-id="65ce4-163">V Pythonu 3.4:</span><span class="sxs-lookup"><span data-stu-id="65ce4-163">In Python 3.4:</span></span>
   
        C:\python34\scripts\wfastcgi-enable
3. <span data-ttu-id="65ce4-164">V C:\inetpub\wwwroot\helloworld vytvořte soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="65ce4-164">In C:\inetpub\wwwroot\helloworld, create a web.config file.</span></span> <span data-ttu-id="65ce4-165">Hello hodnotu hello `scriptProcessor` atribut by měl odpovídat hello výstup hello předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="65ce4-165">hello value of hello `scriptProcessor` attribute should match hello output from hello preceding step.</span></span> <span data-ttu-id="65ce4-166">Další informace o nastavení wfastcgi hello najdete v tématu [úložiště pypi wfastcgi][wfastcgi].</span><span class="sxs-lookup"><span data-stu-id="65ce4-166">For more information about hello wfastcgi setting, see [pypi wfastcgi][wfastcgi].</span></span>
   
   <span data-ttu-id="65ce4-167">V Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="65ce4-167">In  Python 2.7:</span></span>
   
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
   
   <span data-ttu-id="65ce4-168">V Pythonu 3.4:</span><span class="sxs-lookup"><span data-stu-id="65ce4-168">In  Python 3.4:</span></span>
   
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
4. <span data-ttu-id="65ce4-169">Aktualizace umístění hello hello IIS výchozí web toopoint toohello Django složky projektu:</span><span class="sxs-lookup"><span data-stu-id="65ce4-169">Update hello location of hello IIS default website toopoint toohello Django project folder:</span></span>
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. <span data-ttu-id="65ce4-170">Načtěte hello webovou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="65ce4-170">Load hello webpage in your browser.</span></span>

![Okno prohlížeče zobrazí hello hello world stránky v Azure][1]

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="65ce4-172">Vypněte virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="65ce4-172">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="65ce4-173">Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure, které jste vytvořili pro kurz hello hello.</span><span class="sxs-lookup"><span data-stu-id="65ce4-173">When you're done with this tutorial, we recommend that you shut down or remove hello Azure VM you created for hello tutorial.</span></span> <span data-ttu-id="65ce4-174">Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.</span><span class="sxs-lookup"><span data-stu-id="65ce4-174">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
