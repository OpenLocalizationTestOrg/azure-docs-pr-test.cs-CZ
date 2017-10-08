---
title: "aaaCreate Docker hostuje v Azure pomocí Docker počítače | Microsoft Docs"
description: "Popisuje využívání hostitelů docker toocreate Docker počítače v Azure."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: fbf67e8189bbf33f874c4a9b619a931f28ccee12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Vytvoření hostitelů s Dockerem v Azure pomocí Docker-Machine
Spuštění [Docker](https://www.docker.com/) kontejnery vyžaduje hostitele virtuálních počítačů spuštěných hello docker démon.
Toto téma popisuje, jak toouse hello [počítač docker](https://docs.docker.com/machine/) příkaz toocreate nové virtuální počítače s Linuxem, nakonfigurovaný s hello Docker démona, běží v Azure. 

**Poznámka:** 

* *Tento článek závisí na verzi počítač docker 0.9.0-rc2 nebo větší*
* *Kontejnery Windows budou podporovány prostřednictvím docker počítače v blízké budoucnosti hello*

## <a name="create-vms-with-docker-machine"></a>Vytvořit virtuální počítače s počítačem Docker
Vytvořte docker hostitele virtuálních počítačů v Azure s hello `docker-machine create` příkaz pomocí hello `azure` ovladačů. 

Hello ovladač Azure vyžaduje ID vašeho předplatného. Můžete použít hello [rozhraní příkazového řádku Azure](cli-install-nodejs.md) nebo hello [portálu Azure](https://portal.azure.com) tooretrieve předplatného Azure. 

**Pomocí hello portálu Azure**

* Vyberte **odběry** z hello levé navigační stránku a zkopírujte hello id předplatného.

**Pomocí hello rozhraní příkazového řádku Azure**

* Typ ```azure account list``` a id předplatného hello kopírování.

Typ `docker-machine create --driver azure` toosee hello možnosti a jejich výchozí hodnoty.
Můžete také zjistit hello [dokumentace Azure ovladač Docker](https://docs.docker.com/machine/drivers/azure/) Další informace. 

Hello následující příklad závisí na hello [výchozí hodnoty](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), ale můžete nastavit tyto hodnoty: 

* Azure dns pro název hello spojený s veřejnou IP adresu hello a certifikáty generované. Toto je název DNS hello virtuálního počítače. Hello virtuálních počítačů můžete pak bezpečně zastavit, vydání hello dynamické IP a poskytnout možnost tooreconnect hello po hello virtuální počítač spustí znovu s novou IP Adresou. Předpona názvu Hello musí být jedinečný pro danou oblast UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.
* Otevřete port 80 na hello virtuálních počítačů pro odchozí přístup k Internetu
* velikost hello úložiště premium rychlejší tooutilize virtuálního počítače
* použít pro disk hello virtuálních počítačů služby storage úrovně Premium

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Zvolit hostitele s docker počítač docker
Až budete mít záznam v docker počítače pro hostitele, můžete nastavit výchozí hostitel hello při spouštění příkazů docker.

## <a name="using-powershell"></a>Pomocí prostředí PowerShell
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Pomocí Bash
```bash
eval $(docker-machine env MyDockerHost)
```

Teď můžete spustit příkazy docker proti hello zadaného hostitele

```
docker ps
docker info
```

## <a name="run-a-container"></a>Spusťte kontejner
Pomocí konfigurovaného hostitele teď můžete spustit jednoduchého webového serveru tootest zda váš hostitel byla nakonfigurována správně.
Zde jsme použít bitovou kopii standardní nginx, zadejte, že musí naslouchat na portu 80, že pokud se restartuje hello hostitele virtuálních počítačů, hello kontejneru se restartování a také (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

výstup Hello by měl vypadat podobně jako následující hello:

```
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-hello-container"></a>Kontejner testů hello
Zkontrolujte spuštěných kontejnerů pomocí `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

A toosee hello systémem kontejneru, typu `docker-machine ip <VM name>` toofind hello IP adresu tooenter v prohlížeči hello:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Spuštěné ngnix kontejneru](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Souhrn
Pomocí docker počítače můžete snadno zřídit hostitelů docker v Azure pro vaše jednotlivé docker hostitele ověření.
Produkční hostování kontejnerů, najdete v části hello [Azure Container Service](http://aka.ms/AzureContainerService)

toodevelop aplikace .NET Core pomocí sady Visual Studio, najdete v části [Docker Tools pro Visual Studio](http://aka.ms/DockerToolsForVS)

