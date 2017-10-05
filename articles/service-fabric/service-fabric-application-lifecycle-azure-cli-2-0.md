---
title: "Spravovat aplikace Azure Service Fabric pomocí Azure CLI 2.0"
description: "Zjistěte, jak nasadit a odebrání aplikace z clusteru služby Azure Service Fabric pomocí Azure CLI 2.0."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: 5728339236e3819b301e428f9d7a8add08f02b3e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a>Spravovat aplikace Azure Service Fabric pomocí Azure CLI 2.0

Naučte se vytvářet a odstraňovat aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.

## <a name="prerequisites"></a>Požadavky

* Nainstalujte rozhraní příkazového řádku Azure 2.0. Pak vyberte cluster Service Fabric. Další informace najdete v tématu [Začínáme s Azure CLI 2.0](service-fabric-azure-cli-2-0.md).

* Máte připravený k nasazení balíčku aplikace Service Fabric. Další informace o tom, jak vytvořit a balíček aplikace, přečtěte si informace o [model aplikace Service Fabric](service-fabric-application-model.md).

## <a name="overview"></a>Přehled

Pokud chcete nasadit novou aplikaci, proveďte tyto kroky:

1. Nahrajte balíček aplikace do úložiště bitové kopie Service Fabric.
2. Zřídit typ aplikace.
3. Zadejte a vytvoření aplikace.
4. Zadejte a vytvoření služeb.

Odebrat existující aplikaci, proveďte tyto kroky:

1. Odstraňte aplikaci.
2. Zrušit zřízení typu přidružené aplikaci.
3. Odstranění obsahu úložiště bitové kopie.

## <a name="deploy-a-new-application"></a>Nasazení nové aplikace

Chcete-li nasadit novou aplikaci, proveďte následující úlohy.

### <a name="upload-a-new-application-package-to-the-image-store"></a>Nahrát nový balíček aplikace do úložiště bitové kopie

Než vytvoříte aplikaci, nahrajte balíček aplikace do úložiště bitové kopie Service Fabric. 

Například, pokud probíhá balíčku aplikace `app_package_dir` adresáře, použijte následující příkazy pro nahrání adresáři:

```azurecli
az sf application upload --path ~/app_package_dir
```

U velkých balíčků aplikací, můžete zadat `--show-progress` možnosti zobrazíte průběh nahrávání.

### <a name="provision-the-application-type"></a>Zřízení typu aplikace

Po dokončení nahrávání se zřídit aplikace. Zřídit aplikace, použijte následující příkaz:

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

Hodnota `application-type-build-path` je název adresáře, kde jste nahráli balíčku aplikace.

### <a name="create-an-application-from-an-application-type"></a>Vytvoření aplikace z typ aplikace

Po zřízení aplikace, použijte následující příkaz k pojmenování a vytvoření aplikace:

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`je název, který chcete použít pro instanci aplikace. Manifest dříve zřízené aplikace můžete získat další parametry.

Název aplikace musí začínat předponu `fabric:/`.

### <a name="create-services-for-the-new-application"></a>Vytvoření nové aplikace služby

Po vytvoření aplikace, vytvořte z aplikace služby. V následujícím příkladu vytvoříme nové bezstavové služby z naší aplikace. Služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace.

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Ověřte stav a nasazení aplikace

Pokud chcete ověřit, že byly úspěšně nasazeny aplikace a služby, zkontrolujte, jestli jsou uvedený aplikace a služby:

```azurecli
az sf application list
az sf service list --application-list TestApp
```

Pokud chcete ověřit, že služba je v pořádku, použijte příkazy podobné načíst stav služby a aplikace:

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.

## <a name="remove-an-existing-application"></a>Odeberte stávající aplikaci

Chcete-li odebrat, proveďte následující úlohy.

### <a name="delete-the-application"></a>Odstranit aplikaci

Pokud chcete odstranit aplikaci, použijte následující příkaz:

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Zrušit zřízení typu aplikace

Po odstranění aplikace, můžete zrušit zřízení typu aplikace již nepotřebujete. Chcete-li zrušit zřízení typu aplikace, použijte následující příkaz:

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

Zadejte název a typ verze musí odpovídat názvem a verzí v manifestu dříve zřízené aplikace.

### <a name="delete-the-application-package"></a>Odstranění balíčku aplikace

Po mít zrušil zajišťování typu aplikace, můžete odstranit balíček aplikace z úložiště bitových kopií již nepotřebujete. Odstraňování balíčky aplikací pomáhá uvolnění místa na disku. 

K odstranění balíčku aplikace z úložiště bitových kopií, použijte následující příkaz:

```azurecli
az sf application package-delete --content-path app_package_dir
```

`content-path`musí být název adresáře, který jste nahráli při vytváření aplikace.

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)
