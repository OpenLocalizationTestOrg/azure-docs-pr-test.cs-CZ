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
# <a name="ssh-support-for-azure-web-app-on-linux"></a>Podpora SSH pro webové aplikace Azure v systému Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Přehled

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) je kryptografických síťový protokol pro bezpečné používání síťové služby. Je nejčastěji používanou toolog do systému vzdáleně bezpečně z příkazového řádku a správu příkazy spustit vzdáleně.

Webové aplikace v systému Linux poskytuje podporu SSH do kontejneru aplikace hello s jednotlivými hello předdefinované Docker obrázků použitých pro hello Runtime zásobník nové webové aplikace. 

![Zásobníky modulu runtime](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Můžete taky použití SSH se vlastních bitových kopií Docker včetně hello SSH serveru jako součást hello bitové kopie a její konfiguraci, jak je popsáno v tomto tématu.



## <a name="making-a-client-connection"></a>Navazování připojení klienta

toomake připojení klienta SSH, hello hlavního webu musí být spuštěna. 

Vložte hello koncový bod zdroje řízení správy (SCM) pro webovou aplikaci do prohlížeče pomocí hello následující formulář:

        https://<your sitename>.scm.azurewebsites.net/webssh/host

Pokud již nejsou ověřené, jsou požadované tooauthenticate s tooconnect vašeho předplatného Azure.

![Připojení SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a>Podpora SSH s vlastními obrázky Docker

Aby vlastní Docker image toosupport SSH komunikaci mezi hello kontejneru a hello klienta v hello portálu Azure proveďte následující kroky pro bitové kopie Docker hello. 

Tyto kroky nejsou, jsou uvedené v hello úložiště Azure App Service jako příklad [zde](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Zahrnout hello `openssh-server` instalace v [ `RUN` instrukce](https://docs.docker.com/engine/reference/builder/#run) v hello soubor Docker pro bitové kopie a nastavte pro kořenový účet hello hello heslo příliš`"Docker!"`. 

    > [!NOTE] 
    > Tato konfigurace neumožňuje připojení k externím toohello kontejneru. SSH lze přistupovat pouze prostřednictvím hello Kudu / SCM lokalitě, která slouží k ověření hello přihlašovací údaje pro publikování.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. Přidat [ `COPY` instrukce](https://docs.docker.com/engine/reference/builder/#copy) toohello soubor Docker toocopy [sshd_config](http://man.openbsd.org/sshd_config) souboru toohello */atd/ssh/* adresáře. Konfigurační soubor by měla být založena na naše sshd_config souborů v úložišti GitHub službě Azure-App-Service hello [zde](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).

    > [!NOTE] 
    > Hello *sshd_config* nebo hello připojení selže, soubor musí zahrnovat následující hello: 
    > * `Ciphers`musí obsahovat alespoň jeden z následujících hello: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`musí obsahovat alespoň jeden z následujících hello: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. Zahrnout port 2222 hello [ `EXPOSE` instrukce](https://docs.docker.com/engine/reference/builder/#expose) pro hello soubor Docker. I když se označuje hello kořenové heslo, získat přístup k portu 2222 z hello Internetu. Je k interní pouze portu přístupné pouze pomocí kontejnery v rámci sítě most hello privátní virtuální sítě.

    ```docker
    EXPOSE 2222 80
    ```

4. Zkontrolujte, zda text hello toostart ssh služby. Příklad Hello [sem](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) používá skript prostředí v */bin* adresáře.

    ```bash
    #!/bin/bash
    service ssh start
    ```

    soubor Docker Hello používá hello [ `CMD` instrukce](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello skriptu.

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a>Další kroky
V tématu hello následující odkazy na další informace týkající se webové aplikace v systému Linux. Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux](app-service-linux-using-custom-docker-image.md)
* [Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux](app-service-linux-using-nodejs-pm2.md)
* [Pomocí .NET Core v Azure webové aplikace v systému Linux](app-service-linux-using-dotnetcore.md)
* [Použití Ruby v Azure webové aplikace v systému Linux](app-service-linux-ruby-get-started.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)

