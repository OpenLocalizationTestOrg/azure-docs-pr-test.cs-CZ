---
title: "aaaCreate vývoj kanál v Azure s volaných | Microsoft Docs"
description: "Zjistěte, jak toocreate volaných virtuální počítač v Azure, aby si z webu GitHub na každý kód potvrzení a vytvoří novou Docker kontejneru toorun vaší aplikace"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c079e3c9186c9da0a3e51e1823215779c565e0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Jak toocreate vývoj infrastruktury na virtuální počítač s Linuxem v Azure pomocí volaných Githubu a Docker
tooautomate hello sestavení a testování fáze vývoje aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu. V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače volaných
> * Instalace a konfigurace volaných
> * Vytvoření webhooku integrace mezi Githubu a volaných
> * Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení
> * Vytvoření bitové kopie Docker pro vaši aplikaci.
> * Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Vytvoření instance volaných
V předchozích kurzu na [jak toocustomize virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se naučili jak tooautomate přizpůsobení virtuálního počítače s inicializací cloudu. Tento kurz používá cloudu init souboru tooinstall volaných a Docker na virtuálním počítači. 

V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace. Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači. Zadejte `sensible-editor cloud-init-jenkins.txt` toocreate hello souboru a zobrazit seznam dostupných editory. Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupJenkins* v hello *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Použití hello `--custom-data` parametr toopass v souboru config init cloudu. Zadejte úplnou cestu hello příliš*cloudu. init jenkins.txt* Pokud jste uložili soubor hello mimo pracovní adresář existuje.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Jak dlouho trvá několik minut, než hello virtuálních počítačů toobe vytvořený a nakonfigurovaný.

tooallow virtuálního počítače, použijte webový provoz tooreach [open-port az virtuálního](/cli/azure/vm#open-port) tooopen port *8080* volaných provozu a portů *1337* pro aplikaci Node.js hello, který je použité toorun ukázkovou aplikaci:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Konfigurace volaných
tooaccess vaše volaných instance, získejte hello veřejnou IP adresu vašeho virtuálního počítače:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Z bezpečnostních důvodů musíte heslo tooenter hello počáteční správce, který je uložený v textovém souboru na váš virtuální počítač toostart hello volaných nainstalovat. Použijte hello veřejnou IP adresu získat v hello předchozí krok tooSSH tooyour virtuálních počítačů:

```bash
ssh azureuser@<publicIps>
```

Zobrazení hello `initialAdminPassword` pro vaše volaných nainstalovat a zkopírujte ho:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Pokud ještě není k dispozici soubor hello, počkejte několik minut na cloudu init toocomplete hello volaných a Docker instalaci.

Nyní otevřete webový prohlížeč a přejděte příliš`http://<publicIps>:8080`. Dokončete počáteční nastavení volaných hello následujícím způsobem:

- Zadejte hello *initialAdminPassword* získané z hello virtuálních počítačů v předchozím kroku hello.
- Klikněte na tlačítko **vyberte tooinstall moduly plug-in**
- Vyhledejte *Githubu* hello textového pole v horní části hello, vyberte hello *modulu plug-in Githubu*, pak klikněte na tlačítko **instalace**
- toocreate volaných uživatelský účet, vyplňte formulář hello podle potřeby. Z hlediska zabezpečení byste měli vytvořit tohoto prvního uživatele volaných spíše než budete pokračovat jako hello výchozího správce účtu.
- Po dokončení klikněte na tlačítko **začít používat volaných**


## <a name="create-github-webhook"></a>Vytvoření webhook Githubu
tooconfigure hello integrace s Githubu, otevřete hello [ukázkovou aplikaci Node.js Hello, World](https://github.com/Azure-Samples/nodejs-docs-hello-world) z hello ukázek Azure úložišti. toofork hello úložišti tooyour vlastní účet GitHub, klikněte na tlačítko hello **rozvětvení** tlačítko v horním pravém rohu hello.

Vytvoření webhooku uvnitř hello rozvětvení, kterou jste vytvořili:

- Klikněte na tlačítko **nastavení**, pak vyberte **integrace & služby** na levé straně hello.
- Klikněte na tlačítko **přidat službu**, pak zadejte *volaných* pole filtru.
- Vyberte *volaných (modul plug-in Githubu)*
- Pro hello **volaných napojit URL**, zadejte `http://<publicIps>:8080/github-webhook/`. Zajistěte, aby zahrnete hello koncové nebo
- Klikněte na tlačítko **přidat službu**

![Přidání úložiště tooyour forked webhooku GitHub](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Vytvořit úlohu volaných
toohave volaných reakce tooan událost v Githubu například potvrzením kódu, vytvořit úlohu volaných. 

Ve vašem webu volaných, klikněte na tlačítko **vytvoření nové úlohy** z hello domovské stránky:

- Zadejte *HelloWorld* jako název úlohy. Zvolte **volný styl projektu**, pak vyberte **OK**.
- V části hello **Obecné** vyberte **Githubu** projektu a zadejte URL forked úložiště, jako například *https://github.com/iainfoulds/nodejs-docs-hello-world*
- V části hello **zdrojového kódu správy** vyberte **Git**, zadejte vaše forked úložišti *.git* adresu URL, například *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- V části hello **sestavení aktivační události** vyberte **Githubu háku aktivační událost pro dotazování GITscm**.
- V části hello **sestavení** zvolte **přidat krok sestavení**. Vyberte **spustit prostředí**, pak zadejte `echo "Testing"` v okně toocommand.
- Klikněte na tlačítko **Uložit** v hello dolní části okna úloh hello.


## <a name="test-github-integration"></a>Testování integrace Githubu
hello tootest Githubu integrace s volaných, Potvrdit změnu vaší rozvětvení. 

Zpět v Githubu webové uživatelské rozhraní, vyberte vaše forked úložiště a pak klikněte hello **index.js** souboru. Klikněte na tlačítko tooedit ikonu tužky hello tento soubor tak přečte řádek 6:

```nodejs
response.end("Hello World!");
```

toocommit změny, klikněte na tlačítko hello **potvrzení změn** tlačítko dole v hello.

Ve volaných, spustí nového sestavení v části hello **sestavení historie** části hello levém dolním rohu stránku úlohy. Klikněte na odkaz číslo sestavení hello a vyberte **konzole výstup** na levé straně velikosti hello. Můžete zobrazit kroky hello volaných přebírá jako kód pocházejí z Githubu a hello sestavení akce výstupy uvítací zprávu `Testing` toohello konzoly. Pokaždé, když je potvrzení změn provedených v Githubu, webhooku hello spojí tooJenkins a aktivovat nové sestavení tímto způsobem.


## <a name="define-docker-build-image"></a>Definujte image Docker sestavení
aplikaci Node.js hello toosee spuštěnou na základě na vaše potvrzení Githubu umožňuje sestavení Docker aplikace hello toorun bitové kopie. Obrázek Hello je sestaven z soubor Docker, která definuje, jak tooconfigure hello kontejneru, který spouští aplikace hello. 

Z hello SSH připojení tooyour virtuálních počítačů změňte toohello volaných prostoru adresář s názvem po hello úlohy, kterou jste vytvořili v předchozím kroku. V našem příkladu, který byl s názvem *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Vytvořte soubor s v tomto adresáři prostoru s `sudo sensible-editor Dockerfile` a hello vložte následující obsah. Ujistěte se, že hello celý soubor Docker je zkopírován správně, zejména hello první řádek:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Tento soubor Docker používá základní image Node.js hello pomocí Alpine Linux, zpřístupňuje port 1337 hello Hello World aplikace běží, pak zkopíruje soubory aplikace hello a inicializaci.


## <a name="create-jenkins-build-rules"></a>Vytvoření pravidla volaných sestavení
V předchozím kroku jste vytvořili základní pravidlo volaných sestavení, které výstup konzoly toohello zprávy. Umožňuje vytvořit naše soubor Docker toouse krok hello sestavení a spuštění aplikace hello.

Zpět v instanci volaných vyberte hello úlohy, kterou jste vytvořili v předchozím kroku. Klikněte na tlačítko **konfigurace** na levé straně hello a přejděte dolů toohello **sestavení** části:

- Odeberte stávající `echo "Test"` kroku sestavení. Klikněte na tlačítko hello red křížové v horním pravém rohu hello hello existující sestavení krok pole.
- Klikněte na tlačítko **přidat krok sestavení**, pak vyberte **spustit prostředí**
- V hello **příkaz** pole, zadejte následující příkazy Docker hello a potom vyberte **Uložit**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

kroky sestavení Docker Hello vytvoření bitové kopie a značky její hello volaných číslo sestavení tak spravovat historii bitové kopie. Žádné existující kontejnery systémem hello aplikace jsou zastavena a pak odebral. Nový kontejner je pak spuštěna pomocí bitové kopie hello a spouští aplikace Node.js založené na nejnovější potvrzení hello v Githubu.


## <a name="test-your-pipeline"></a>Testování vaší kanálu
toosee hello celého kanálu v akci, upravit hello *index.js* v vaší forked úložiště GitHub znovu soubor a klikněte na tlačítko **Potvrdit změnu**. Nová úloha se spustí v volaných podle hello webhooku pro GitHub. Trvá několik sekund toocreate hello Docker image a spusťte aplikaci v nový kontejner.

V případě potřeby znovu získejte hello veřejnou IP adresu vašeho virtuálního počítače:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Otevřete webový prohlížeč a zadejte `http://<publicIps>:1337`. Aplikace Node.js se zobrazí a odráží hello nejnovější potvrzení v vaší Githubu rozvětvení následujícím způsobem:

![Spuštěné aplikace Node.js](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Nyní provést další úpravy toohello *index.js* souboru v Githubu a potvrzení změn hello. Počkejte několik sekund pro úlohy toocomplete hello ve volaných, a poté obnovte verze webového prohlížeče toosee hello aktualizovat aplikace spuštěné v nový kontejner následujícím způsobem:

![Spuštění aplikace Node.js po další potvrzení Githubu](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali Githubu toorun úlohu sestavení volaných na každém potvrzení kódu a pak nasadit tootest kontejner Docker vaší aplikace. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače volaných
> * Instalace a konfigurace volaných
> * Vytvoření webhooku integrace mezi Githubu a volaných
> * Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení
> * Vytvoření bitové kopie Docker pro vaši aplikaci.
> * Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace

Posunutí toohello další kurz toolearn informace o toointegrate volaných s Visual Studio Team Services.

> [!div class="nextstepaction"]
> [Nasazení aplikací s volaných a Team Services](tutorial-build-deploy-jenkins.md)