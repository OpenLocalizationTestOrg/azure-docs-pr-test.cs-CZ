---
title: "aplikace Azure Service Fabric aaaManage pomocí Azure CLI 2.0"
description: "Zjistěte, jak clusteru toodeploy a odebrat aplikace ze Azure Service Fabric pomocí Azure CLI 2.0."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ae1ba19513978b0f95ffb65d5f1f7a21ed5f2894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a>Spravovat aplikace Azure Service Fabric pomocí Azure CLI 2.0

Zjistěte, jak toocreate a odstranění aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.

## <a name="prerequisites"></a>Požadavky

* Nainstalujte rozhraní příkazového řádku Azure 2.0. Pak vyberte cluster Service Fabric. Další informace najdete v tématu [Začínáme s Azure CLI 2.0](service-fabric-azure-cli-2-0.md).

* Máte Service Fabric aplikace balíčku připravené toobe nasazení. Další informace o tom, tooauthor a balíček aplikace, přečtěte si informace o hello [model aplikace Service Fabric](service-fabric-application-model.md).

## <a name="overview"></a>Přehled

toodeploy novou aplikaci, proveďte tyto kroky:

1. Nahrajte úložišti aplikace Service Fabric toohello balíček bitové kopie.
2. Zřídit typ aplikace.
3. Zadejte a vytvoření aplikace.
4. Zadejte a vytvoření služeb.

tooremove existující aplikaci, proveďte tyto kroky:

1. Odstraňte aplikaci hello.
2. Unprovision hello související typ aplikace.
3. Odstraňte hello obsahu úložiště bitové kopie.

## <a name="deploy-a-new-application"></a>Nasazení nové aplikace

toodeploy novou aplikaci, dokončení hello následující úlohy.

### <a name="upload-a-new-application-package-toohello-image-store"></a>Nahrát nové úložiště bitové kopie balíčku toohello aplikace

Předtím, než vytvoříte aplikaci, nahrajte balíček aplikace hello, toohello úložiště bitových kopií Service Fabric. 

Například, pokud je balíčku aplikace v hello `app_package_dir` directory hello použijte následující příkazy tooupload hello adresáře:

```azurecli
az sf application upload --path ~/app_package_dir
```

U velkých balíčků aplikací, můžete zadat hello `--show-progress` možnost toodisplay hello průběh nahrávání hello.

### <a name="provision-hello-application-type"></a>Typ aplikace hello zřizování

Po dokončení nahrávání hello zřídit hello aplikace. tooprovision hello aplikace hello použijte následující příkaz:

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

Hello hodnota `application-type-build-path` je název hello hello adresáře, kde jste nahráli balíčku aplikace.

### <a name="create-an-application-from-an-application-type"></a>Vytvoření aplikace z typ aplikace

Po zřízení hello aplikace, použijte následující příkaz tooname hello a vytvoření aplikace:

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`je název hello má toouse pro instanci aplikace hello. Další parametry můžete získat z manifestu dříve zřízené aplikace hello.

název aplikace Hello musí začínat hello předponu `fabric:/`.

### <a name="create-services-for-hello-new-application"></a>Vytvoření služby pro novou aplikaci hello

Po vytvoření aplikace služby vytvořte z aplikace hello. V následujícím příkladu hello vytvoříme nové bezstavové služby z naší aplikace. Hello služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace hello.

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Ověřte stav a nasazení aplikace

tooverify, že byly úspěšně nasazeny aplikace a služby, zkontrolujte, že jsou uvedeny hello aplikace a služby:

```azurecli
az sf application list
az sf service list --application-list TestApp
```

tooverify, že služba hello je v pořádku, použijte podobné příkazy tooretrieve hello stavu hello služby a aplikace hello:

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.

## <a name="remove-an-existing-application"></a>Odeberte stávající aplikaci

tooremove aplikace, dokončení hello následující úlohy.

### <a name="delete-hello-application"></a>Odstranit aplikaci hello

toodelete hello aplikace hello použijte následující příkaz:

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a>Zrušit zřízení typu aplikace hello

Po odstranění aplikace hello, můžete zrušit zřízení typu aplikace hello již nepotřebujete. toounprovision hello typ aplikace, hello použijte následující příkaz:

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

Hello zadejte název a typ verze musí odpovídat hello názvem a verzí v manifestu dříve zřízené aplikace hello.

### <a name="delete-hello-application-package"></a>Odstranění balíčku aplikace hello

Po mít zrušil zajišťování hello typu aplikace, můžete odstranit balíček aplikace hello z úložiště bitových kopií hello již nepotřebujete. Odstraňování balíčky aplikací pomáhá uvolnění místa na disku. 

balíček aplikace hello toodelete z úložiště bitových kopií hello, hello použijte následující příkaz:

```azurecli
az sf application package-delete --content-path app_package_dir
```

`content-path`musí být název hello hello adresáře, který jste nahráli při vytváření aplikace hello.

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)
