---
title: "Vytvoření kanálu vývoj v Azure pomocí volaných | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač volaných v Azure, který vrátí z webu GitHub na každém potvrzení kódu a vytvoří nový kontejner Docker ke spouštění vaší aplikace"
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
ms.openlocfilehash: d9849b5e061dd7f2ae0744a3522dc2eb1fb37035
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Postup vytvoření vývoj infrastruktury na virtuální počítač s Linuxem v Azure pomocí volaných Githubu a Docker
K automatizaci fázi sestavení a testování pro vývoj aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu. V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače volaných
> * Instalace a konfigurace volaných
> * Vytvoření webhooku integrace mezi Githubu a volaných
> * Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení
> * Vytvoření bitové kopie Docker pro vaši aplikaci.
> * Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Vytvoření instance volaných
V předchozích kurz [postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak automatizovat přizpůsobení virtuálního počítače s inicializací cloudu. Tento kurz používá soubor cloudu init volaných a Docker nainstalujete na virtuální počítač. 

V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace. Například vytvoření souboru v prostředí cloudu není na místním počítači. Zadejte `sensible-editor cloud-init-jenkins.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:

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

Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupJenkins* v *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Použití `--custom-data` parametr předávat do cloudu init konfiguračního souboru. Zadejte úplnou cestu k *cloudu. init jenkins.txt* Pokud jste uložili soubor mimo pracovní adresář existuje.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Trvá několik minut pro virtuální počítač pro vytvoření a konfiguraci.

Povolit webový provoz připojit virtuální počítač, použijte [az virtuálních počítačů open-port](/cli/azure/vm#open-port) otevřít port *8080* volaných provozu a portů *1337* pro aplikaci Node.js, která se používá ke spuštění ukázkové aplikace:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Konfigurace volaných
Chcete-li získat přístup k vaší instanci volaných, získejte veřejnou IP adresu vašeho virtuálního počítače:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Z bezpečnostních důvodů musíte zadat heslo počáteční správce, které je uložený v textovém souboru na vašem virtuálním počítači spusťte instalaci volaných. Použijte veřejnou IP adresu získaných v předchozím kroku do SSH pro virtuální počítač:

```bash
ssh azureuser@<publicIps>
```

Zobrazení `initialAdminPassword` pro vaše volaných nainstalovat a zkopírujte ho:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Pokud se soubor ještě není k dispozici, počkejte několik minut cloudu init pro dokončení instalace volaných a Docker.

Nyní otevřete webový prohlížeč a přejděte na `http://<publicIps>:8080`. Dokončete počáteční nastavení volaných následujícím způsobem:

- Zadejte *initialAdminPassword* získané z virtuálního počítače v předchozím kroku.
- Klikněte na tlačítko **vyberte modulů plug-in pro instalaci**
- Vyhledejte *Githubu* do textového pole v horní části, vyberte *modulu plug-in Githubu*, pak klikněte na tlačítko **instalace**
- Pokud chcete vytvořit uživatelský účet volaných, vyplňte formulář podle potřeby. Z hlediska zabezpečení byste měli vytvořit tohoto prvního uživatele volaných spíše než budete pokračovat, jako výchozí účet správce.
- Po dokončení klikněte na tlačítko **začít používat volaných**


## <a name="create-github-webhook"></a>Vytvoření webhook Githubu
Chcete-li nakonfigurovat integraci s Githubu, otevřete [ukázkovou aplikaci Node.js Hello World](https://github.com/Azure-Samples/nodejs-docs-hello-world) z úložiště ukázek Azure. Chcete-li rozvětvit úložiště k účtu GitHub, klikněte **rozvětvení** tlačítko v horním pravém rohu.

Vytvoření webhooku uvnitř rozvětvení, kterou jste vytvořili:

- Klikněte na tlačítko **nastavení**, pak vyberte **integrace & služby** na levé straně.
- Klikněte na tlačítko **přidat službu**, pak zadejte *volaných* pole filtru.
- Vyberte *volaných (modul plug-in Githubu)*
- Pro **volaných napojit URL**, zadejte `http://<publicIps>:8080/github-webhook/`. Zajistěte, aby zahrnout koncový znak /
- Klikněte na tlačítko **přidat službu**

![Přidat do vašeho úložiště forked webhook Githubu](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Vytvořit úlohu volaných
Pokud chcete, aby volaných reakce na události na Githubu, například potvrzením kódu, vytvořte úlohu volaných. 

Ve vašem webu volaných, klikněte na tlačítko **vytvoření nové úlohy** z domovské stránky:

- Zadejte *HelloWorld* jako název úlohy. Zvolte **volný styl projektu**, pak vyberte **OK**.
- V části **Obecné** vyberte **Githubu** projektu a zadejte URL forked úložiště, jako například *https://github.com/iainfoulds/nodejs-docs-hello-world*
- V části **zdrojového kódu správu** vyberte **Git**, zadejte vaše forked úložišti *.git* adresu URL, například *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- V části **sestavení aktivační události** vyberte **Githubu háku aktivační událost pro dotazování GITscm**.
- V části **sestavení** zvolte **přidat krok sestavení**. Vyberte **spustit prostředí**, pak zadejte `echo "Testing"` v příkazovém okně.
- Klikněte na tlačítko **Uložit** v dolní části okna úlohy.


## <a name="test-github-integration"></a>Testování integrace Githubu
K testování Githubu integraci s volaných, potvrďte změnu ve vašem rozvětvení. 

Zpět v Githubu webové uživatelské rozhraní, vyberte vaše forked úložišti a klikněte **index.js** souboru. Klikněte na ikonu tužky k úpravám tohoto souboru, takže přečte řádek 6:

```nodejs
response.end("Hello World!");
```

Chcete-li potvrdit změny, klikněte na tlačítko **potvrzení změn** tlačítko dole.

Ve volaných, spustí nového sestavení v části **sestavení historie** části levém dolním rohu stránku úlohy. Klikněte na odkaz číslo sestavení a vyberte **konzole výstup** na levé straně velikosti. Můžete si zobrazit kroky volaných přebírá jako kód pocházejí z Githubu a akce sestavení výstupy zprávu `Testing` ke konzole. Pokaždé, když je potvrzení změn provedených v Githubu, webhooku spojí do volaných a aktivovat nové sestavení tímto způsobem.


## <a name="define-docker-build-image"></a>Definujte image Docker sestavení
Umožňuje zobrazit spuštěné na základě na vaše potvrzení Githubu aplikace Node.js sestavení Docker bitové kopie a spusťte aplikaci. Obrázek je sestaven z soubor Docker, která definuje konfiguraci kontejneru, který spouští aplikaci. 

Ze SSH připojení k virtuálnímu počítači přejděte do adresáře prostoru volaných s názvem po úkol, který jste vytvořili v předchozím kroku. V našem příkladu, který byl s názvem *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Vytvořte soubor s v tomto adresáři prostoru s `sudo sensible-editor Dockerfile` a vložte následující obsah. Ujistěte se, jestli je správně, zkopírovali celý soubor Docker obzvláště první řádek:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Tento soubor Docker používá základní Node.js bitové kopie pomocí Alpine Linux, port zpřístupňuje 1337, který běží aplikace Hello World, pak zkopíruje soubory aplikace a inicializaci.


## <a name="create-jenkins-build-rules"></a>Vytvoření pravidla volaných sestavení
V předchozím kroku jste vytvořili základní pravidlo volaných sestavení, které na výstup zprávu do konzoly. Umožňuje vytvořit krok sestavení pomocí našich soubor Docker a spuštění aplikace.

Zpět v instanci volaných vyberte úlohu, kterou jste vytvořili v předchozím kroku. Klikněte na tlačítko **konfigurace** na levé straně a posuňte se dolů **sestavení** části:

- Odeberte stávající `echo "Test"` kroku sestavení. Klikněte na tlačítko červený křížek v pravém horním rohu pole pro stávající krok sestavení.
- Klikněte na tlačítko **přidat krok sestavení**, pak vyberte **spustit prostředí**
- V **příkaz** pole, zadejte následující příkazy Docker a potom vyberte **Uložit**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

Kroky sestavení Docker vytvoření bitové kopie a značky její volaných číslo sestavení tak spravovat historii bitové kopie. Žádné existující kontejnery spuštění aplikace jsou zastavena a pak odebral. Nový kontejner je pak spuštěna pomocí image a spouští aplikace Node.js založené na nejnovější potvrzení v Githubu.


## <a name="test-your-pipeline"></a>Testování vaší kanálu
Chcete-li zobrazit celý kanálu v akci, upravte *index.js* v vaší forked úložiště GitHub znovu soubor a klikněte na tlačítko **Potvrdit změnu**. Nová úloha se spustí v volaných podle webhooku pro GitHub. Jak dlouho trvá několik sekund pro vytvoření bitové kopie Docker a spusťte aplikaci v nový kontejner.

V případě potřeby znovu získejte veřejnou IP adresu vašeho virtuálního počítače:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Otevřete webový prohlížeč a zadejte `http://<publicIps>:1337`. Aplikace Node.js se zobrazí a odráží nejnovější potvrzení v vaší Githubu rozvětvení následujícím způsobem:

![Spuštěné aplikace Node.js](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Nyní provést jiný úprava *index.js* souborů na webu GitHub a potvrďte změny. Počkejte několik sekund pro úlohu dokončit v volaných a potom aktualizujte webový prohlížeč zobrazíte aktualizovanou verzi vaší aplikace spuštěné v nový kontejner následujícím způsobem:

![Spuštění aplikace Node.js po další potvrzení Githubu](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali Githubu spustit úlohu volaných sestavení v každém potvrzení kódu a pak nasadit kontejner Docker k testování aplikace. Jste se dozvěděli, jak na:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače volaných
> * Instalace a konfigurace volaných
> * Vytvoření webhooku integrace mezi Githubu a volaných
> * Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení
> * Vytvoření bitové kopie Docker pro vaši aplikaci.
> * Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace

Přechodu na v dalším kurzu se dozvíte více o tom, jak integrovat volaných Visual Studio Team Services.

> [!div class="nextstepaction"]
> [Nasazení aplikací s volaných a Team Services](tutorial-build-deploy-jenkins.md)