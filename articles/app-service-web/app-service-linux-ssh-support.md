---
title: "Podpora SSH pro webové aplikace Azure App Service v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: feee7a5c91d213a6b0bfdaf264a4da4d9e79cbe7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="ssh-support-for-azure-web-app-on-linux"></a><span data-ttu-id="5058c-104">Podpora SSH pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5058c-104">SSH support for Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="5058c-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="5058c-105">Overview</span></span>

<span data-ttu-id="5058c-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) je kryptografických síťový protokol pro bezpečné používání síťové služby.</span><span class="sxs-lookup"><span data-stu-id="5058c-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) is a cryptographic network protocol for using network services securely.</span></span> <span data-ttu-id="5058c-107">Toto pravidlo se používá nejčastěji přihlásit do systému vzdáleně bezpečně z příkazového řádku a spouštět příkazy pro správu vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="5058c-107">It is most commonly used to log into a system remotely securely from a command-line and execute administrative commands remotely.</span></span>

<span data-ttu-id="5058c-108">Webové aplikace v systému Linux poskytuje podporu SSH do kontejneru aplikace s jednotlivými předdefinované Docker obrázků použitých pro modul Runtime zásobník nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5058c-108">Web App on Linux provides SSH support into the app container with each of the built-in Docker images used for the Runtime Stack of new web apps.</span></span> 

![Zásobníky modulu runtime](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

<span data-ttu-id="5058c-110">Můžete taky použití SSH se vlastních bitových kopií Docker včetně SSH serveru v rámci bitové kopie a její konfiguraci, jak je popsáno v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5058c-110">You can also use SSH with your custom Docker images by including the SSH server as part of the image and configuring it as described in this topic.</span></span>



## <a name="making-a-client-connection"></a><span data-ttu-id="5058c-111">Navazování připojení klienta</span><span class="sxs-lookup"><span data-stu-id="5058c-111">Making a client connection</span></span>

<span data-ttu-id="5058c-112">Chcete-li připojení klienta SSH, je nutné spustit hlavního webu.</span><span class="sxs-lookup"><span data-stu-id="5058c-112">To make an SSH client connection, the main site must be started.</span></span> 

<span data-ttu-id="5058c-113">Koncový bod zdroje řízení správy (SCM) pro webovou aplikaci vložte do prohlížeče v následující podobě:</span><span class="sxs-lookup"><span data-stu-id="5058c-113">Paste the Source Control Management (SCM) endpoint for your web app into your browser using the following form:</span></span>

        https://<your sitename>.scm.azurewebsites.net/webssh/host

<span data-ttu-id="5058c-114">Pokud již nejsou ověřené, je nutné k ověření s předplatným Azure připojit.</span><span class="sxs-lookup"><span data-stu-id="5058c-114">If you are not already authenticated, you are required to authenticate with your Azure subscription to connect.</span></span>

![Připojení SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a><span data-ttu-id="5058c-116">Podpora SSH s vlastními obrázky Docker</span><span class="sxs-lookup"><span data-stu-id="5058c-116">SSH support with custom Docker images</span></span>

<span data-ttu-id="5058c-117">Aby pro vlastní image Docker pro podporu SSH komunikace mezi klienta a kontejneru na portálu Azure proveďte následující kroky pro Docker image.</span><span class="sxs-lookup"><span data-stu-id="5058c-117">In order for a custom Docker image to support SSH communication between the container and the client in the Azure portal, perform the following steps for your Docker image.</span></span> 

<span data-ttu-id="5058c-118">Tyto kroky nejsou v úložišti Azure App Service jako příklad jsou uvedeny [zde](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span><span class="sxs-lookup"><span data-stu-id="5058c-118">These steps are are shown in the Azure App Service repository as an example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span></span>

1. <span data-ttu-id="5058c-119">Zahrnout `openssh-server` instalace v [ `RUN` instrukce](https://docs.docker.com/engine/reference/builder/#run) v soubor Docker pro bitovou kopii a heslo pro kořenový účet sady `"Docker!"`.</span><span class="sxs-lookup"><span data-stu-id="5058c-119">Include the `openssh-server` installation in [`RUN` instruction](https://docs.docker.com/engine/reference/builder/#run) in the Dockerfile for your image and set the password for the root account to `"Docker!"`.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="5058c-120">Tato konfigurace neumožňuje externí připojení ke kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5058c-120">This configuration does not allow external connections to the container.</span></span> <span data-ttu-id="5058c-121">SSH lze přistupovat pouze prostřednictvím Kudu nebo webu SCM, což je ověřen pomocí přihlašovací údaje pro publikování.</span><span class="sxs-lookup"><span data-stu-id="5058c-121">SSH can only be accessed via the Kudu / SCM Site, which is authenticated using the publishing credentials.</span></span>

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. <span data-ttu-id="5058c-122">Přidat [ `COPY` instrukce](https://docs.docker.com/engine/reference/builder/#copy) na soubor Docker zkopírovat [sshd_config](http://man.openbsd.org/sshd_config) do souboru */atd/ssh/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="5058c-122">Add a [`COPY` instruction](https://docs.docker.com/engine/reference/builder/#copy) to the Dockerfile to copy a [sshd_config](http://man.openbsd.org/sshd_config) file to the */etc/ssh/* directory.</span></span> <span data-ttu-id="5058c-123">Konfigurační soubor by měla být založena na našem sshd_config souboru v úložišti GitHub službě Azure-App-Service [zde](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span><span class="sxs-lookup"><span data-stu-id="5058c-123">Your configuration file should be based on our sshd_config file in the Azure-App-Service GitHub repository [here](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5058c-124">*Sshd_config* nebo připojení selže, soubor musí zahrnovat následující:</span><span class="sxs-lookup"><span data-stu-id="5058c-124">The *sshd_config* file must include the following or the connection fails:</span></span> 
    > * <span data-ttu-id="5058c-125">`Ciphers`musí obsahovat alespoň jednu z následujících: `aes128-cbc,3des-cbc,aes256-cbc`.</span><span class="sxs-lookup"><span data-stu-id="5058c-125">`Ciphers` must include at least one of the following: `aes128-cbc,3des-cbc,aes256-cbc`.</span></span>
    > * <span data-ttu-id="5058c-126">`MACs`musí obsahovat alespoň jednu z následujících: `hmac-sha1,hmac-sha1-96`.</span><span class="sxs-lookup"><span data-stu-id="5058c-126">`MACs` must include at least one of the following: `hmac-sha1,hmac-sha1-96`.</span></span>

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. <span data-ttu-id="5058c-127">Zahrnout port 2222 v [ `EXPOSE` instrukce](https://docs.docker.com/engine/reference/builder/#expose) pro soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="5058c-127">Include port 2222 in the [`EXPOSE` instruction](https://docs.docker.com/engine/reference/builder/#expose) for the Dockerfile.</span></span> <span data-ttu-id="5058c-128">I když se označuje kořenové heslo, port 2222 není přístupný z Internetu.</span><span class="sxs-lookup"><span data-stu-id="5058c-128">Although the root password is known, port 2222 cannot be accessed from the internet.</span></span> <span data-ttu-id="5058c-129">Je k interní pouze portu přístupné pouze pomocí kontejnery v rámci sítě most privátní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5058c-129">It is an internal only port accessible only by containers within the bridge network of a private virtual network.</span></span>

    ```docker
    EXPOSE 2222 80
    ```

4. <span data-ttu-id="5058c-130">Ujistěte se, spusťte služba ssh.</span><span class="sxs-lookup"><span data-stu-id="5058c-130">Make sure to start the ssh service.</span></span> <span data-ttu-id="5058c-131">V příkladu [sem](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) používá skript prostředí v */bin* adresáře.</span><span class="sxs-lookup"><span data-stu-id="5058c-131">The example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) uses a shell script in */bin* directory.</span></span>

    ```bash
    #!/bin/bash
    service ssh start
    ```

    <span data-ttu-id="5058c-132">Soubor Docker používá [ `CMD` instrukce](https://docs.docker.com/engine/reference/builder/#cmd) pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="5058c-132">The Dockerfile uses the [`CMD` instruction](https://docs.docker.com/engine/reference/builder/#cmd) to run the script.</span></span>

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a><span data-ttu-id="5058c-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5058c-133">Next steps</span></span>
<span data-ttu-id="5058c-134">V následujících tématech Další informace týkající se webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="5058c-134">See the following links for more information regarding Web App on Linux.</span></span> <span data-ttu-id="5058c-135">Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="5058c-135">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="5058c-136">Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5058c-136">How to use a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="5058c-137">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5058c-137">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="5058c-138">Pomocí .NET Core v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5058c-138">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="5058c-139">Použití Ruby v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5058c-139">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="5058c-140">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5058c-140">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

