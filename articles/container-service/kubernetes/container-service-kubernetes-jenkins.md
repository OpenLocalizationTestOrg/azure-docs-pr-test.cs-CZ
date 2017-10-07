---
title: aaaJenkins CI/CD s Kubernetes v Azure Container Service | Microsoft Docs
description: "Jak tooautomate CI/CD zpracovat volaných toodeploy a upgrade kontejnerizované aplikace na Kubernetes v Azure Container Service"
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: "Docker kontejnery, Kubernetes, Azure, volaných"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: e00e13bf06619bed73e82878777e55458ea3dd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Volaných integraci s Azure Container Service a Kubernetes 
V tomto kurzu jsme provede hello proces tooset až průběžnou integraci aplikace s více kontejnerů do Azure kontejneru služby Kubernetes pomocí platformy volaných hello. pracovní postup Hello aktualizací bitové kopie hello kontejneru v úložiště Docker Hub a upgraduje hello Kubernetes pracovními stanicemi soustředěnými kolem pomocí zavedení nasazení. 

## <a name="high-level-process"></a>Proces vysoké úrovně
Hello základní kroky popsané v tomto článku jsou: 
- Instalace clusteru s podporou Kubernetes v kontejneru služby
- Nastavení volaných a konfigurace přístupu tooContainer služby
- Vytvoření pracovního postupu volaných
- Testování tooend koncového procesu CI/CD hello

## <a name="install-a-kubernetes-cluster"></a>Instalace clusteru s podporou Kubernetes
    
Nasazení clusteru Kubernetes hello v Azure Container Service pomocí hello následující kroky. Úplnou dokumentaci se nachází [zde](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>Krok 1: Vytvoření skupiny prostředků
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-hello-cluster"></a>Krok 2: Nasazení clusteru hello
> [!NOTE]
> Hello následující kroky vyžadují místní veřejný klíč SSH uložen ve složce ~/.ssh hello.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-toocontainer-service"></a>Nastavení volaných a konfigurace přístupu tooContainer služby

### <a name="step-1-install-jenkins"></a>Krok 1: Instalace volaných
1. Vytvořte virtuální počítač Azure s Ubuntu 16.04 LTS.  Vzhledem k tomu, že dál v hello kroky nutné tooconnect toothis virtuální počítač pomocí na místním počítači, sada hello "Ověřování typu" too'SSH veřejný klíč bash budou a vložte hello veřejný klíč SSH místně uložená ve složce ~/.ssh.  Také si poznamenejte hello "uživatelské jméno, že zadáváte vzhledem k tomu, že toto uživatelské jméno bude potřebné tooview hello volaných řídicí panel a pro připojení toohello volaných virtuálních počítačů v dalších krocích.
2. Nainstalujte volaných prostřednictvím těchto [pokyny](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Podrobnější kurz je určen v [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. tooview hello volaných řídicího panelu na místním počítači, aktualizovat port tooallow skupiny zabezpečení sítě Azure hello 8080 přidáním pravidlo pro příchozí data, která umožňuje přístup tooport 8080.  Alternativně může nastavit přesměrování portu tak, že spustíte tento příkaz:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Připojení serveru volaných tooyour pomocí prohlížeče hello přechodem toohello veřejnou IP adresu (http:// < your_jenkins_public_ip >: 8080) a odemknutí hello volaných řídicím panelu hello poprvé heslem hello počáteční správce.  heslo správce Hello je uložený v /var/lib/jenkins/secrets/initialAdminPassword na hello volaných virtuálních počítačů.  Snadný způsob tooget toto heslo je tooSSH do hello volaných virtuálních počítačů: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Potom spustíte: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Nainstalujte na počítač volaných hello prostřednictvím těchto Docker [pokyny](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). To umožňuje toobe Docker příkazy spouštět ve volaných úlohy.
6. Nakonfigurujte Docker oprávnění tooallow volaných tooaccess hello Docker koncový bod.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Nainstalujte `kubectl` rozhraní příkazového řádku na volaných. Další podrobnosti najdete v [instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Úlohy volaných použije 'kubectl' toomanage a nasazení toohello Kubernetes clusteru.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-toohello-kubernetes-cluster"></a>Krok 2: Nastavení clusteru Kubernetes toohello přístupu

> [!NOTE]
> Existuje několik přístupů tooaccomplishing hello následující kroky. Použijte hello přístup, který je pro vás nejjednodušší.
>

1. Kopírování hello `kubectl` konfigurační soubor toohello volaných počítač tak, aby volaných úloh clusteru Kubernetes toohello přístupu. Tyto pokyny předpokládají, že používáte bash z jiný počítač, než hello volaných virtuálních počítačů a že místní veřejný klíč SSH je uložen ve složce ~/.ssh hello počítače.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Ověření z volaných této hello Kubernetes clusteru je přístupný.  toodo se SSH do hello volaných virtuálních počítačů: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Dále ověřte volaných můžete úspěšně připojit tooyour cluster: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Vytvoření pracovního postupu volaných

### <a name="prerequisites"></a>Požadavky

- Účet GitHub pro kód úložišti.
- Docker Hub účet toostore a aktualizaci bitové kopie.
- Kontejnerizované aplikace, které je možné znovu sestavit a aktualizovat. Můžete použít této ukázkové kontejneru aplikace napsané v Golang: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> Hello následující kroky je potřeba provést v účtu Githubu. Myslíte, že volné tooclone hello výše úložiště, ale musíte použít vlastní účet tooconfigure hello webhooky a volaných přístup.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>Krok 1: Nasazení počáteční v1 aplikace
1. Sestavení aplikace hello z počítače vývojáře hello s hello následující příkazy. Nahraďte `myrepo` vlastními.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Push image tooDocker rozbočovače.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Nasazení clusteru Kubernetes toohello.
    
    > [!NOTE] 
    > Upravit hello `go-web.yaml` souboru tooupdate kontejneru image a úložišti.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>Krok 2: Konfigurace volaných systému
1. Klikněte na tlačítko **spravovat volaných** > **konfiguraci systému**.
2. V části **Githubu**, vyberte **přidat Server Githubu**.
3. Nechte **adresy URL rozhraní API** jako výchozí.
4. V části **pověření**, přidat pomocí přihlašovacích údajů volaných **tajný text**. Doporučujeme používat Githubu osobní přístupové tokeny, které jsou nakonfigurované v nastavení účtu uživatele Githubu. Další informace o to [sem.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Klikněte na tlačítko **testovací připojení** tooensure to správně nakonfigurovaná.
6. V části **vlastnosti globálních**, přidejte proměnná prostředí `DOCKER_HUB` a zadejte heslo úložiště Docker Hub. (To je užitečné v této ukázce, ale produkční scénář by vyžadovaly bezpečnější přístup.)
7. Uložte.

![Přístup volaných Githubu](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-hello-jenkins-workflow"></a>Krok 3: Vytvoření pracovního postupu volaných hello
1. Vytvořte položku volaných.
2. Zadejte název (například "přejděte – web") a vyberte **volný styl projektu**. 
3. Zkontrolujte **Githubu projektu** a zadat úložiště GitHub tooyour hello adresy URL.
4. V **správu zdrojového kódu**, zadejte adresu URL úložiště GitHub hello a přihlašovací údaje. 
5. Přidat **sestavení krok** typu **spustit prostředí** a hello použijte následující text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Přidejte další **sestavení krok** typu **spustit prostředí** a hello použijte následující text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Kroky sestavení volaných](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Uložit položky volaných hello a testování pomocí **sestavení teď**.

### <a name="step-4-connect-github-webhook"></a>Krok 4: Připojení webhook Githubu
1. V položce volaných hello jste vytvořili, klikněte na tlačítko **konfigurace**.
2. V části **sestavení aktivační události**, vyberte **Githubu háku aktivační událost pro dotazování GITScm** a **Uložit**. Tím se automaticky nakonfiguruje webhook Githubu hello.
3. V úložiště GitHub pro přejděte web, klikněte na **Nastavení > Webhooky**.
4. Ověřte, že hello volaných webhooku, které adresu URL byl úspěšně přidán. Adresa URL Hello musí končit "webhook githubu".

![Konfigurace webhooku volaných](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-hello-cicd-process-end-tooend"></a>Testování tooend koncového procesu CI/CD hello

1. Aktualizujte kód pro hello úložišti a nabízených nebo synchronizace s úložišti GitHub hello.
2. Z konzoly volaných hello, zkontrolujte hello **sestavení historie** a ověřte, že hello úloha byla spuštěna. Zobrazení podrobností toosee výstupu konzoly.
3. Zobrazení podrobností hello z Kubernetes, upgradovat nasazení:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Další kroky

- Nasaďte registru kontejner Azure a uložení bitové kopie v zabezpečené úložiště. V tématu [dokumentace Azure kontejneru registru](https://docs.microsoft.com/azure/container-registry).
- Vytvořte složitější pracovní postup, který obsahuje vedle sebe nasazení a automatizovaných testů ve volaných.
- Další informace o CI/CD s volaných a Kubernetes najdete v tématu hello [volaných blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
