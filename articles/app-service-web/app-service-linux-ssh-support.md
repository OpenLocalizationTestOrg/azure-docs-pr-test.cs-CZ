---
title: "aaaSSH podporu pro webové aplikace Azure App Service v systému Linux | Microsoft Docs"
description: "Další informace o použití SSH s Azure webové aplikace v systému Linux."
keywords: "služby Azure app service, webové aplikace, linux, operačních systémů"
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: e00be6d4631e8936a2a8bc106da1fc06237a4b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ssh-support-for-azure-web-app-on-linux"></a><span data-ttu-id="a04da-104">Podpora SSH pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a04da-104">SSH support for Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="a04da-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a04da-105">Overview</span></span>

<span data-ttu-id="a04da-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) je kryptografických síťový protokol pro bezpečné používání síťové služby.</span><span class="sxs-lookup"><span data-stu-id="a04da-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) is a cryptographic network protocol for using network services securely.</span></span> <span data-ttu-id="a04da-107">Je nejčastěji používanou toolog do systému vzdáleně bezpečně z příkazového řádku a správu příkazy spustit vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="a04da-107">It is most commonly used toolog into a system remotely securely from a command-line and execute administrative commands remotely.</span></span>

<span data-ttu-id="a04da-108">Webové aplikace v systému Linux poskytuje podporu SSH do kontejneru aplikace hello s jednotlivými hello předdefinované Docker obrázků použitých pro hello Runtime zásobník nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a04da-108">Web App on Linux provides SSH support into hello app container with each of hello built-in Docker images used for hello Runtime Stack of new web apps.</span></span> 

![Zásobníky modulu runtime](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

<span data-ttu-id="a04da-110">Můžete taky použití SSH se vlastních bitových kopií Docker včetně hello SSH serveru jako součást hello bitové kopie a její konfiguraci, jak je popsáno v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="a04da-110">You can also use SSH with your custom Docker images by including hello SSH server as part of hello image and configuring it as described in this topic.</span></span>



## <a name="making-a-client-connection"></a><span data-ttu-id="a04da-111">Navazování připojení klienta</span><span class="sxs-lookup"><span data-stu-id="a04da-111">Making a client connection</span></span>

<span data-ttu-id="a04da-112">toomake připojení klienta SSH, hello hlavního webu musí být spuštěna.</span><span class="sxs-lookup"><span data-stu-id="a04da-112">toomake an SSH client connection, hello main site must be started.</span></span> 

<span data-ttu-id="a04da-113">Vložte hello koncový bod zdroje řízení správy (SCM) pro webovou aplikaci do prohlížeče pomocí hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="a04da-113">Paste hello Source Control Management (SCM) endpoint for your web app into your browser using hello following form:</span></span>

        https://<your sitename>.scm.azurewebsites.net/webssh/host

<span data-ttu-id="a04da-114">Pokud již nejsou ověřené, jsou požadované tooauthenticate s tooconnect vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="a04da-114">If you are not already authenticated, you are required tooauthenticate with your Azure subscription tooconnect.</span></span>

![Připojení SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a><span data-ttu-id="a04da-116">Podpora SSH s vlastními obrázky Docker</span><span class="sxs-lookup"><span data-stu-id="a04da-116">SSH support with custom Docker images</span></span>

<span data-ttu-id="a04da-117">Aby vlastní Docker image toosupport SSH komunikaci mezi hello kontejneru a hello klienta v hello portálu Azure proveďte následující kroky pro bitové kopie Docker hello.</span><span class="sxs-lookup"><span data-stu-id="a04da-117">In order for a custom Docker image toosupport SSH communication between hello container and hello client in hello Azure portal, perform hello following steps for your Docker image.</span></span> 

<span data-ttu-id="a04da-118">Tyto kroky nejsou, jsou uvedené v hello úložiště Azure App Service jako příklad [zde](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span><span class="sxs-lookup"><span data-stu-id="a04da-118">These steps are are shown in hello Azure App Service repository as an example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span></span>

1. <span data-ttu-id="a04da-119">Zahrnout hello `openssh-server` instalace v [ `RUN` instrukce](https://docs.docker.com/engine/reference/builder/#run) v hello soubor Docker pro bitové kopie a nastavte pro kořenový účet hello hello heslo příliš`"Docker!"`.</span><span class="sxs-lookup"><span data-stu-id="a04da-119">Include hello `openssh-server` installation in [`RUN` instruction](https://docs.docker.com/engine/reference/builder/#run) in hello Dockerfile for your image and set hello password for hello root account too`"Docker!"`.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="a04da-120">Tato konfigurace neumožňuje připojení k externím toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a04da-120">This configuration does not allow external connections toohello container.</span></span> <span data-ttu-id="a04da-121">SSH lze přistupovat pouze prostřednictvím hello Kudu / SCM lokalitě, která slouží k ověření hello přihlašovací údaje pro publikování.</span><span class="sxs-lookup"><span data-stu-id="a04da-121">SSH can only be accessed via hello Kudu / SCM Site, which is authenticated using hello publishing credentials.</span></span>

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. <span data-ttu-id="a04da-122">Přidat [ `COPY` instrukce](https://docs.docker.com/engine/reference/builder/#copy) toohello soubor Docker toocopy [sshd_config](http://man.openbsd.org/sshd_config) souboru toohello */atd/ssh/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="a04da-122">Add a [`COPY` instruction](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy a [sshd_config](http://man.openbsd.org/sshd_config) file toohello */etc/ssh/* directory.</span></span> <span data-ttu-id="a04da-123">Konfigurační soubor by měla být založena na naše sshd_config souborů v úložišti GitHub službě Azure-App-Service hello [zde](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span><span class="sxs-lookup"><span data-stu-id="a04da-123">Your configuration file should be based on our sshd_config file in hello Azure-App-Service GitHub repository [here](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a04da-124">Hello *sshd_config* nebo hello připojení selže, soubor musí zahrnovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="a04da-124">hello *sshd_config* file must include hello following or hello connection fails:</span></span> 
    > * <span data-ttu-id="a04da-125">`Ciphers`musí obsahovat alespoň jeden z následujících hello: `aes128-cbc,3des-cbc,aes256-cbc`.</span><span class="sxs-lookup"><span data-stu-id="a04da-125">`Ciphers` must include at least one of hello following: `aes128-cbc,3des-cbc,aes256-cbc`.</span></span>
    > * <span data-ttu-id="a04da-126">`MACs`musí obsahovat alespoň jeden z následujících hello: `hmac-sha1,hmac-sha1-96`.</span><span class="sxs-lookup"><span data-stu-id="a04da-126">`MACs` must include at least one of hello following: `hmac-sha1,hmac-sha1-96`.</span></span>

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. <span data-ttu-id="a04da-127">Zahrnout port 2222 hello [ `EXPOSE` instrukce](https://docs.docker.com/engine/reference/builder/#expose) pro hello soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="a04da-127">Include port 2222 in hello [`EXPOSE` instruction](https://docs.docker.com/engine/reference/builder/#expose) for hello Dockerfile.</span></span> <span data-ttu-id="a04da-128">I když se označuje hello kořenové heslo, získat přístup k portu 2222 z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="a04da-128">Although hello root password is known, port 2222 cannot be accessed from hello internet.</span></span> <span data-ttu-id="a04da-129">Je k interní pouze portu přístupné pouze pomocí kontejnery v rámci sítě most hello privátní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a04da-129">It is an internal only port accessible only by containers within hello bridge network of a private virtual network.</span></span>

    ```docker
    EXPOSE 2222 80
    ```

4. <span data-ttu-id="a04da-130">Zkontrolujte, zda text hello toostart ssh služby.</span><span class="sxs-lookup"><span data-stu-id="a04da-130">Make sure toostart hello ssh service.</span></span> <span data-ttu-id="a04da-131">Příklad Hello [sem](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) používá skript prostředí v */bin* adresáře.</span><span class="sxs-lookup"><span data-stu-id="a04da-131">hello example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) uses a shell script in */bin* directory.</span></span>

    ```bash
    #!/bin/bash
    service ssh start
    ```

    <span data-ttu-id="a04da-132">soubor Docker Hello používá hello [ `CMD` instrukce](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="a04da-132">hello Dockerfile uses hello [`CMD` instruction](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello script.</span></span>

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a><span data-ttu-id="a04da-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a04da-133">Next steps</span></span>
<span data-ttu-id="a04da-134">V tématu hello následující odkazy na další informace týkající se webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a04da-134">See hello following links for more information regarding Web App on Linux.</span></span> <span data-ttu-id="a04da-135">Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="a04da-135">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="a04da-136">Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a04da-136">How toouse a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="a04da-137">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a04da-137">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="a04da-138">Pomocí .NET Core v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a04da-138">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="a04da-139">Použití Ruby v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a04da-139">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="a04da-140">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a04da-140">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

