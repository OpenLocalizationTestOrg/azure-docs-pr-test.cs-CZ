---
title: "aaaUsing Docker rozšíření virtuálního počítače pro Linux | Microsoft Docs"
description: "Popisuje Docker a rozšíření Azure Virtual Machines hello a jak hello toocreate virtuálních počítačích Azure, které jsou hostitelů docker pomocí rozhraní příkazového řádku Azure v modelu nasazení classic."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: 9d455b63c48b0c1b6f14862e072f899a73b46153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-with-hello-azure-classic-portal"></a>Hello Docker rozšíření virtuálního počítače pomocí hello portál Azure classic
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

[Docker](https://www.docker.com/) je jedním z hello nejoblíbenější virtualizace přístupy, které používá [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) místo virtuální počítače jako způsob oddělením dat a výpočty na sdílených prostředků. Můžete použít rozšíření virtuálního počítače Docker hello spravuje [Azure Linux Agent] toocreate Docker virtuální počítač, který je hostitelem libovolný počet kontejnerů pro vaše aplikace v Azure.

> [!NOTE]
> Toto téma popisuje, jak hello toocreate a virtuální počítač Docker z portálu Azure classic. toosee jak zjistit, toocreate virtuální počítač Docker v příkazovém řádku hello, [jak toouse hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)]. toosee podrobný popis kontejnery a jejich výhody, najdete v části hello [Docker vysokou úroveň tabulí](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).
> 
> 

## <a name="create-a-new-vm-from-hello-image-gallery"></a>Vytvoření nového virtuálního počítače z Galerie obrázků hello
prvním krokem Hello vyžaduje virtuální počítač Azure z Linux bitovou kopii, která podporuje hello Docker rozšíření virtuálního počítače, pomocí bitové kopie Ubuntu 14.04 LTS z Galerie obrázků hello jako obrázek příkladu server a Ubuntu 14.04 Desktop jako klient. Hello portálu, klikněte na tlačítko **+ nový** v hello zůstane dolního rohu toocreate nové instance virtuálního počítače a vyberte bitovou kopii Ubuntu 14.04 LTS z dostupných možnostech hello nebo hello dokončete Galerie obrázků, jak je uvedeno níže.

> [!NOTE]
> V současné době podporují pouze Ubuntu 14.04 LTS obrázky novější než červenec 2014 hello Docker rozšíření virtuálního počítače.
> 
> 

![Vytvořit novou bitovou kopii Ubuntu](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Vytvoření Docker certifikátů
Po hello virtuálního počítače existuje, zkontrolujte, zda je nainstalován Docker na klientském počítači. (Podrobnosti najdete v tématu [pokyny k instalaci Docker](https://docs.docker.com/installation/#installation).)

Vytvořit certifikát a klíč soubory pro Docker komunikaci podle příliš hello[systémem Docker s protokolem https] a umístit je hello  **`~/.docker`**  adresář na klientském počítači.

> [!NOTE]
> Hello rozšíření virtuálního počítače Docker portálu hello aktuálně vyžaduje přihlašovací údaje, které kódováním base64.
> 
> 

Hello příkazového řádku, použijte  **`base64`**  nebo jiné Oblíbené kódování nástroj toocreate kódováním base64 témata. Díky tomuto s jednoduchou sadou souborů certifikát a klíč může vypadat podobně jako toothis:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-hello-docker-vm-extension"></a>Přidat hello Docker rozšíření virtuálního počítače
tooadd hello Docker rozšíření virtuálního počítače, vyhledejte hello instance virtuálního počítače jste vytvořili a posuňte se dolů příliš**rozšíření** a klikněte na něj toobring si rozšíření virtuálních počítačů, jak je uvedeno níže.

> [!NOTE]
> Tato funkce je podporována na portálu preview hello pouze: https://portal.azure.com/
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a>Přidání rozšíření
Klikněte na tlačítko hello **+ přidat** toodisplay hello možné rozšíření virtuálních počítačů můžete přidat toothis virtuálních počítačů.

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-hello-docker-vm-extension"></a>Vyberte hello Docker rozšíření virtuálního počítače
Zvolte hello rozšíření Docker virtuálních počítačů, které se vyvolá hello Docker popis a důležité odkazy, a pak klikněte na tlačítko **vytvořit** na postup instalace hello dolní toobegin hello.

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a>Přidáte certifikát a soubory klíčů:
Hello pole formuláře zadejte hello kódováním base64 verze váš certifikát certifikační Autority, certifikátu serveru a váš klíč serveru, jak ukazuje následující obrázek hello.

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> Všimněte si, že (jako v předchozím obrázku hello) hello. 2376 se vyplní ve výchozím nastavení. Můžete zadat libovolný koncový bod, ale hello dalším krokem bude tooopen až hello odpovídající koncový bod. Pokud změníte výchozí hello, ujistěte se, že tooopen až hello odpovídající koncového bodu v dalším kroku hello.
> 
> 

## <a name="add-hello-docker-communication-endpoint"></a>Přidat hello bod komunikace Docker
Při zobrazení skupiny prostředků hello jste vytvořili, vyberte hello skupinu zabezpečení sítě spojené s virtuální počítač a klikněte na **příchozí pravidla zabezpečení** tooview hello pravidla, jak je vidět tady.

![](media/portal-use-docker/AddingEndpoint.png)

Klikněte na tlačítko **+ přidat** tooadd jiné pravidlo a v případě výchozí hello, zadejte název pro koncový bod hello (v tomto příkladu **Docker**) a. 2376 'rozsah cílových portů'. Nastavit hello protokol hodnota zobrazující **TCP**a klikněte na tlačítko **OK** toocreate hello pravidlo.

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a>Testování vaší Docker klientských a hostitelských Azure Docker
Vyhledejte a zkopírujte hello název domény Virtuálního počítače a v příkazovém řádku hello klientského počítače, typ `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (kde *dockerextension* je nahrazena hello poddomény pro virtuální počítač).

výsledek Hello by měla vypadat podobně jako toothis:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Po dokončení hello výše kroky máte nyní plně funkční Docker hostitele se systémem na virtuální počítač Azure, nakonfigurované toobe připojené tooremotely z jiných klientů.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
Jsou připraveny toogo toohello [Docker uživatelská příručka] a používat virtuální počítač Docker. Pokud chcete, aby tooautomate vytváření hostitelů Docker na virtuálních počítačích Azure prostřednictvím rozhraní příkazového řádku, přečtěte si téma [jak toouse hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)]

<!--Anchors-->
[Create a new VM from hello Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add hello Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[jak toouse hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]:../agent-user-guide.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md

[systémem Docker s protokolem https]:http://docs.docker.com/articles/https/
[Docker uživatelská příručka]:https://docs.docker.com/userguide/
