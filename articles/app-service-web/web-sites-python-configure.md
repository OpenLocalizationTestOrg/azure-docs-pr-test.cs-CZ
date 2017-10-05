---
title: "Konfigurace Python s webovými aplikacemi Azure App Service"
description: "Tento kurz popisuje možnosti pro vytváření obsahu a konfigurace základní serveru webové aplikace Python kompatibilní s brány rozhraní (WSGI) v Azure App Service Web Apps."
services: app-service
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
ms.openlocfilehash: 9683a1af13eeff364d3c4714f0b791324fd82659
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-python-with-azure-app-service-web-apps"></a><span data-ttu-id="e329f-103">Konfigurace Python s webovými aplikacemi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e329f-103">Configuring Python with Azure App Service Web Apps</span></span>
<span data-ttu-id="e329f-104">Tento kurz popisuje možnosti pro vytváření obsahu a konfigurace základní aplikace kompatibilní s Python Webový Server brány rozhraní (WSGI) na [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="e329f-104">This tutorial describes options for authoring and configuring a basic Web Server Gateway Interface (WSGI) compliant Python application on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="e329f-105">Popisuje další funkce nasazení Git, například virtuální prostředí a instalaci balíčku pomocí souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="e329f-105">It describes additional features of Git deployment, such as virtual environment and package installation using requirements.txt.</span></span>

## <a name="bottle-django-or-flask"></a><span data-ttu-id="e329f-106">Bottle, Django nebo Flask?</span><span class="sxs-lookup"><span data-stu-id="e329f-106">Bottle, Django or Flask?</span></span>
<span data-ttu-id="e329f-107">Azure Marketplace obsahuje šablony pro rozhraní Bottle, rozhraní Django a Flask.</span><span class="sxs-lookup"><span data-stu-id="e329f-107">The Azure Marketplace contains templates for the Bottle, Django and Flask frameworks.</span></span> <span data-ttu-id="e329f-108">Pokud vyvíjíte první webové aplikace v Azure App Service nebo nejste obeznámeni s Gitem, doporučujeme podle následujících kurzech, mezi které patří podrobné pokyny pro vytváření funkční aplikaci z Galerie pomocí nasazení Git z Systém Windows nebo Mac:</span><span class="sxs-lookup"><span data-stu-id="e329f-108">If you are developing your first web app in Azure App Service, or you are not familiar with Git, we recommend that you follow one of these tutorials, which include step-by-step instructions for building a working application from the gallery using Git deployment from Windows or Mac:</span></span>

* [<span data-ttu-id="e329f-109">Vytvoření webové aplikace pomocí Bottle</span><span class="sxs-lookup"><span data-stu-id="e329f-109">Creating web apps with Bottle</span></span>](web-sites-python-create-deploy-bottle-app.md)
* [<span data-ttu-id="e329f-110">Vytvoření webové aplikace pomocí rozhraní Django</span><span class="sxs-lookup"><span data-stu-id="e329f-110">Creating web apps with Django</span></span>](web-sites-python-create-deploy-django-app.md)
* [<span data-ttu-id="e329f-111">Vytvoření webové aplikace pomocí Flask</span><span class="sxs-lookup"><span data-stu-id="e329f-111">Creating web apps with Flask</span></span>](web-sites-python-create-deploy-flask-app.md)

## <a name="web-app-creation-on-azure-portal"></a><span data-ttu-id="e329f-112">Vytvoření webové aplikace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e329f-112">Web app creation on Azure Portal</span></span>
<span data-ttu-id="e329f-113">Tento kurz předpokládá stávající předplatné Azure a přístup k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e329f-113">This tutorial assumes an existing Azure subscription and access to the Azure Portal.</span></span>

<span data-ttu-id="e329f-114">Pokud nemáte existující webovou aplikaci, můžete vytvořit jeden z [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e329f-114">If you do not have an existing web app, you can create one from the [Azure Portal](https://portal.azure.com).</span></span>  <span data-ttu-id="e329f-115">Klikněte na tlačítko Nový v levém horním rohu a pak klikněte na tlačítko **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e329f-115">Click the NEW button in the top left corner, then click **Web + Mobile** > **Web app**.</span></span>

## <a name="git-publishing"></a><span data-ttu-id="e329f-116">Publikování Git</span><span class="sxs-lookup"><span data-stu-id="e329f-116">Git Publishing</span></span>
<span data-ttu-id="e329f-117">Pro nově vytvořenou webovou aplikaci nakonfigurujte publikování Git podle pokynů uvedených v tématu [Místní nasazení GIT ve službě Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="e329f-117">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="e329f-118">Tento kurz používá Git pro vytváření, správu a publikovat webovou aplikaci Python do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e329f-118">This tutorial uses Git to create, manage, and publish our Python web app to Azure App Service.</span></span>

<span data-ttu-id="e329f-119">Po publikování Git úložiště Git vytváření a přidružené k vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e329f-119">Once Git publishing is set up, a Git repository will be created and associated with your web app.</span></span> <span data-ttu-id="e329f-120">V úložišti URL se zobrazí a od nynějška umožňuje nabízet data z místního vývojového prostředí do cloudu.</span><span class="sxs-lookup"><span data-stu-id="e329f-120">The repository's URL will be displayed and can henceforth be used to push data from the local development environment to the cloud.</span></span> <span data-ttu-id="e329f-121">K publikování aplikací prostřednictvím Git, zkontrolujte, zda že je zároveň nainstalován klient Git a postupujte podle pokynů uvedených k webovému obsahu aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e329f-121">To publish applications via Git, make sure a Git client is also installed and use the instructions provided to push your web app content to Azure App Service.</span></span>

## <a name="application-overview"></a><span data-ttu-id="e329f-122">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="e329f-122">Application Overview</span></span>
<span data-ttu-id="e329f-123">V následujících částech jsou vytvořeny následující soubory.</span><span class="sxs-lookup"><span data-stu-id="e329f-123">In the next sections, the following files are created.</span></span> <span data-ttu-id="e329f-124">Musí být umístěny v kořenovém úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="e329f-124">They should be placed in the root of the Git repository.</span></span>

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a><span data-ttu-id="e329f-125">Obslužná rutina WSGI</span><span class="sxs-lookup"><span data-stu-id="e329f-125">WSGI Handler</span></span>
<span data-ttu-id="e329f-126">WSGI je standard Python popsaného [období 3333](http://www.python.org/dev/peps/pep-3333/) definování rozhraní mezi webového serveru a Python.</span><span class="sxs-lookup"><span data-stu-id="e329f-126">WSGI is a Python standard described by [PEP 3333](http://www.python.org/dev/peps/pep-3333/) defining an interface between the web server and Python.</span></span> <span data-ttu-id="e329f-127">Poskytuje standardizovaná rozhraní pro zápis různé webové aplikace a rozhraní používá Python.</span><span class="sxs-lookup"><span data-stu-id="e329f-127">It provides a standardized interface for writing various web applications and frameworks using Python.</span></span> <span data-ttu-id="e329f-128">Oblíbených webových rozhraní Python v dnešní době používá WSGI.</span><span class="sxs-lookup"><span data-stu-id="e329f-128">Popular Python web frameworks today use WSGI.</span></span> <span data-ttu-id="e329f-129">Azure App Service Web Apps nabízí podporu pro tyto architektury; Kromě toho Pokročilí uživatelé můžete i vytvářet své vlastní tak dlouho, dokud vlastní obslužná rutina se řídí WSGI specifikace pokyny.</span><span class="sxs-lookup"><span data-stu-id="e329f-129">Azure App Service Web Apps gives you support for any such frameworks; in addition, advanced users can even author their own as long as the custom handler follows the WSGI specification guidelines.</span></span>

<span data-ttu-id="e329f-130">Tady je příklad `app.py` , který definuje vlastní obslužnou rutinu:</span><span class="sxs-lookup"><span data-stu-id="e329f-130">Here's an example of an `app.py` that defines a custom handler:</span></span>

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

<span data-ttu-id="e329f-131">Můžete spustit tuto aplikaci místně s `python app.py`, vyhledejte `http://localhost:5555` ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e329f-131">You can run this application locally with `python app.py`, then browse to `http://localhost:5555` in your web browser.</span></span>

## <a name="virtual-environment"></a><span data-ttu-id="e329f-132">Virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="e329f-132">Virtual Environment</span></span>
<span data-ttu-id="e329f-133">I když výše uvedený příklad aplikace nevyžaduje žádné externí balíčky, je pravděpodobné, že aplikace bude vyžadovat některé.</span><span class="sxs-lookup"><span data-stu-id="e329f-133">Although the example app above doesn't require any external packages, it is likely that your application will require some.</span></span>

<span data-ttu-id="e329f-134">Ke správě závislosti externí balíčků nasazení Git v Azure podporuje vytváření virtuálních prostředí.</span><span class="sxs-lookup"><span data-stu-id="e329f-134">To help manage external package dependencies, Azure Git deployment supports the creation of virtual environments.</span></span>

<span data-ttu-id="e329f-135">Zjistí-li Azure soubor requirements.txt v kořenovém adresáři úložiště, automaticky vytvoří virtuální prostředí s názvem `env`.</span><span class="sxs-lookup"><span data-stu-id="e329f-135">When Azure detects a requirements.txt in the root of the repository, it automatically creates a virtual environment named `env`.</span></span> <span data-ttu-id="e329f-136">K tomu dochází pouze při prvním nasazení nebo během všechna nasazení po vybrané Python runtime změnila.</span><span class="sxs-lookup"><span data-stu-id="e329f-136">This only occurs on the first deployment, or during any deployment after the selected Python runtime has changed.</span></span>

<span data-ttu-id="e329f-137">Pravděpodobně budete chtít vytvořit virtuální prostředí pro vývoj místně, ale nemáte její zahrnutí do úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="e329f-137">You will probably want to create a virtual environment locally for development, but don't include it in your Git repository.</span></span>

## <a name="package-management"></a><span data-ttu-id="e329f-138">Správa balíčků</span><span class="sxs-lookup"><span data-stu-id="e329f-138">Package Management</span></span>
<span data-ttu-id="e329f-139">Balíčky uvedené v souboru requirements.txt se budou instalovat automaticky ve virtuálním prostředí pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="e329f-139">Packages listed in requirements.txt will be installed automatically in the virtual environment using pip.</span></span> <span data-ttu-id="e329f-140">K tomu dojde při každém nasazení, ale pip instalaci přeskočí, pokud balíček je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="e329f-140">This happens on every deployment, but pip will skip installation if a package is already installed.</span></span>

<span data-ttu-id="e329f-141">Příklad `requirements.txt`:</span><span class="sxs-lookup"><span data-stu-id="e329f-141">Example `requirements.txt`:</span></span>

    azure==0.8.4


## <a name="python-version"></a><span data-ttu-id="e329f-142">Verze jazyka Python</span><span class="sxs-lookup"><span data-stu-id="e329f-142">Python Version</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

<span data-ttu-id="e329f-143">Příklad `runtime.txt`:</span><span class="sxs-lookup"><span data-stu-id="e329f-143">Example `runtime.txt`:</span></span>

    python-2.7


## <a name="webconfig"></a><span data-ttu-id="e329f-144">Soubor web.config</span><span class="sxs-lookup"><span data-stu-id="e329f-144">Web.config</span></span>
<span data-ttu-id="e329f-145">Budete muset vytvořit soubor web.config k určení, jak má server pracovat s požadavky.</span><span class="sxs-lookup"><span data-stu-id="e329f-145">You'll need to create a web.config file to specify how the server should handle requests.</span></span>

<span data-ttu-id="e329f-146">Poznámka: Pokud máte soubor web.x.y.config souboru ve svém úložišti, kde x.y odpovídá vybraný modul Python runtime a pak Azure automaticky zkopíruje odpovídající soubor jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="e329f-146">Note that if you have a web.x.y.config file in your repository, where x.y matches the selected Python runtime, then Azure will automatically copy the appropriate file as web.config.</span></span>

<span data-ttu-id="e329f-147">Následující příklady web.config spoléhají na skript virtuální prostředí proxy serveru, který je popsaný v následující části.</span><span class="sxs-lookup"><span data-stu-id="e329f-147">The following web.config examples rely on a virtual environment proxy script, which is described in the next section.</span></span>  <span data-ttu-id="e329f-148">Pracují s obslužná rutina WSGI použitých v tomto příkladu `app.py` výše.</span><span class="sxs-lookup"><span data-stu-id="e329f-148">They work with the WSGI handler used in the example `app.py` above.</span></span>

<span data-ttu-id="e329f-149">Příklad `web.config` pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="e329f-149">Example `web.config` for Python 2.7:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


<span data-ttu-id="e329f-150">Příklad `web.config` pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="e329f-150">Example `web.config` for Python 3.4:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


<span data-ttu-id="e329f-151">Statické soubory budou zpracovány webovým serverem přímo, bez průchodu přes kód Python pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="e329f-151">Static files will be handled by the web server directly, without going through Python code, for improved performance.</span></span>

<span data-ttu-id="e329f-152">V uvedených příkladech umístění statické soubory na disku by měl odpovídat umístění v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="e329f-152">In the above examples, the location of the static files on disk should match the location in the URL.</span></span> <span data-ttu-id="e329f-153">To znamená, že žádost o `http://pythonapp.azurewebsites.net/static/site.css` bude sloužit k souboru na disku v `\static\site.css`.</span><span class="sxs-lookup"><span data-stu-id="e329f-153">This means that a request for `http://pythonapp.azurewebsites.net/static/site.css` will serve the file on disk at `\static\site.css`.</span></span>

<span data-ttu-id="e329f-154">`WSGI_ALT_VIRTUALENV_HANDLER`je, kde můžete určit WSGI obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="e329f-154">`WSGI_ALT_VIRTUALENV_HANDLER` is where you specify the WSGI handler.</span></span> <span data-ttu-id="e329f-155">V uvedených příkladech má `app.wsgi_app` protože rutina je funkce s názvem `wsgi_app` v `app.py` v kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="e329f-155">In the above examples, it's `app.wsgi_app` because the handler is a function named `wsgi_app` in `app.py` in the root folder.</span></span>

<span data-ttu-id="e329f-156">`PYTHONPATH`můžete přizpůsobit, ale pokud nainstalujete všechny závislosti ve virtuálním prostředí zadáním v souboru requirements.txt, neměli byste potřebovat změnit.</span><span class="sxs-lookup"><span data-stu-id="e329f-156">`PYTHONPATH` can be customized, but if you install all your dependencies in the virtual environment by specifying them in requirements.txt, you shouldn't need to change it.</span></span>

## <a name="virtual-environment-proxy"></a><span data-ttu-id="e329f-157">Virtuální prostředí Proxy</span><span class="sxs-lookup"><span data-stu-id="e329f-157">Virtual Environment Proxy</span></span>
<span data-ttu-id="e329f-158">Následující skript se používá k načtení obslužná rutina WSGI, aktivujte virtuální prostředí a protokolu chyb.</span><span class="sxs-lookup"><span data-stu-id="e329f-158">The following script is used to retrieve the WSGI handler, activate the virtual environment and log errors.</span></span> <span data-ttu-id="e329f-159">Je určený jako obecný a použít bez úprav.</span><span class="sxs-lookup"><span data-stu-id="e329f-159">It is designed to be generic and used without modifications.</span></span>

<span data-ttu-id="e329f-160">Obsah `ptvs_virtualenv_proxy.py`:</span><span class="sxs-lookup"><span data-stu-id="e329f-160">Contents of `ptvs_virtualenv_proxy.py`:</span></span>

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a><span data-ttu-id="e329f-161">Přizpůsobení nasazení Git</span><span class="sxs-lookup"><span data-stu-id="e329f-161">Customize Git deployment</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="e329f-162">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="e329f-162">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="e329f-163">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="e329f-163">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="e329f-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e329f-164">Next steps</span></span>
<span data-ttu-id="e329f-165">Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="e329f-165">For more information, see the [Python Developer Center](/develop/python/).</span></span>

> [!NOTE]
> <span data-ttu-id="e329f-166">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e329f-166">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="e329f-167">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="e329f-167">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="e329f-168">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="e329f-168">What's changed</span></span>
* <span data-ttu-id="e329f-169">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="e329f-169">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

