---
title: "aplikace Azure Service Fabric aaaManage pomocí příkazového řádku Azure Service Fabric"
description: "Zjistěte, jak toodeploy a odebrat aplikace ze Azure Service Fabric clusteru pomocí příkazového řádku Azure Service Fabric"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: d9f98cee1d70f71a2aab68ff556956619910e4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a>Spravovat aplikace Azure Service Fabric pomocí příkazového řádku Azure Service Fabric

Zjistěte, jak toocreate a odstranění aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.

## <a name="prerequisites"></a>Požadavky

* Nainstalujte Service Fabric rozhraní příkazového řádku. Pak vyberte cluster Service Fabric. Další informace najdete v tématu [začít pracovat s Service Fabric rozhraní příkazového řádku](service-fabric-cli.md).

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

toodeploy novou aplikaci, dokončení hello následující úlohy:

### <a name="upload-a-new-application-package-toohello-image-store"></a>Nahrát nové úložiště bitové kopie balíčku toohello aplikace

Předtím, než vytvoříte aplikaci, nahrajte balíček aplikace hello, toohello úložiště bitových kopií Service Fabric.

Například, pokud je balíčku aplikace v hello `app_package_dir` directory hello použijte následující příkazy tooupload hello adresáře:

```azurecli
sfctl application upload --path ~/app_package_dir
```

U velkých balíčků aplikací, můžete zadat hello `--show-progress` možnost toodisplay hello průběh nahrávání hello.

### <a name="provision-hello-application-type"></a>Typ aplikace hello zřizování

Po dokončení nahrávání hello zřídit hello aplikace. tooprovision hello aplikace hello použijte následující příkaz:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

Hello hodnota `application-type-build-path` je název hello hello adresáře, kde jste nahráli balíčku aplikace.

### <a name="create-an-application-from-an-application-type"></a>Vytvoření aplikace z typ aplikace

Po zřízení hello aplikace, použijte následující příkaz tooname hello a vytvoření aplikace:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`je název hello má toouse pro instanci aplikace hello. Manifest dříve zřízené aplikace můžete získat další parametry.

název aplikace Hello musí začínat hello předponu `fabric:/`.

### <a name="create-services-for-hello-new-application"></a>Vytvoření služby pro novou aplikaci hello

Po vytvoření aplikace služby vytvořte z aplikace hello. V následujícím příkladu hello vytvoříme nové bezstavové služby z naší aplikace. Hello služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace hello.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Ověřte stav a nasazení aplikace

tooverify všechno, co je v pořádku, použijte následující příkazy stavu hello:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

tooverify, že služba hello je v pořádku, použijte podobné příkazy tooretrieve hello stavu hello služby a aplikace:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.

## <a name="remove-an-existing-application"></a>Odeberte stávající aplikaci

tooremove aplikace, dokončení hello následující úlohy:

### <a name="delete-hello-application"></a>Odstranit aplikaci hello

toodelete hello aplikace hello použijte následující příkaz:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a>Zrušit zřízení typu aplikace hello

Po odstranění aplikace hello, můžete zrušit zřízení typu aplikace hello již nepotřebujete. toounprovision hello typ aplikace, hello použijte následující příkaz:

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

Hello zadejte název a typ verze musí odpovídat hello názvem a verzí v manifestu dříve zřízené aplikace hello.

### <a name="delete-hello-application-package"></a>Odstranění balíčku aplikace hello

Po mít zrušil zajišťování hello typu aplikace, můžete odstranit balíček aplikace hello z úložiště bitových kopií hello již nepotřebujete. Odstraňování balíčky aplikací pomáhá uvolnění místa na disku. 

balíček aplikace hello toodelete z úložiště bitových kopií hello, hello použijte následující příkaz:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path`musí být název hello hello adresáře, který jste nahráli při vytváření aplikace hello.

## <a name="upgrade-application"></a>Upgrade aplikace

Po vytvoření aplikace, můžete opakovat hello stejnou sadu kroků tooprovision druhou verzi vaší aplikace. Potom se upgradu aplikace Service Fabric můžete přejít toorunning hello druhá verze aplikace hello. Další informace naleznete v dokumentaci k hello na [upgradů aplikací Service Fabric](service-fabric-application-upgrade.md).

tooperform upgradu první zřídit hello příští verze aplikace pomocí hello hello stejné příkazy jako dříve:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

Doporučuje se pak tooperform monitorovaných automatický upgrade, spusťte hello upgrade spuštěním hello následující příkaz:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Upgrady přepsat existující parametry s jakoukoli sadu je zadán. Parametry aplikačního mají být předány jako argumenty toohello příkaz pro upgrade, v případě potřeby. Parametry aplikačního by měl být zakódován jako objekt JSON.

tooretrieve žádné parametry dříve zadán, můžete použít hello `sfctl application info` příkaz.

Po upgradu aplikace v průběhu hello stav se dá načíst pomocí `sfctl application upgrade-status` příkaz.

Navíc pokud upgradu v průběhu a je třeba toobe zrušen, můžete použít hello `sfctl application upgrade-rollback` tooroll zpět hello upgradu.

## <a name="next-steps"></a>Další kroky

* [Základy služby Fabric rozhraní příkazového řádku](service-fabric-cli.md)
* [Začínáme se službou Service Fabric v systému Linux](service-fabric-get-started-linux.md)
* [Spuštění upgradu aplikace Service Fabric](service-fabric-application-upgrade.md)
