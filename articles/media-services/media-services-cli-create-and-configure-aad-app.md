---
title: "toocreate aaaUse 2.0 rozhraní příkazového řádku aplikace Azure AD a nakonfigurujte ji tooaccess rozhraní API služby Azure Media Services | Microsoft Docs"
description: "Toto téma ukazuje, jak toocreate toouse 2.0 rozhraní příkazového řádku aplikace Azure AD a nakonfigurujte ji tooaccess rozhraní API služby Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: c865e2701722374b5dd17b0e20fa848c07065006
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-20-toocreate-an-aad-app-and-configure-it-tooaccess-azure-media-services-api"></a>Použijte rozhraní příkazového řádku 2.0 toocreate AAD aplikace a nakonfigurujte ho tooaccess rozhraní API služby Azure Media Services

Toto téma ukazuje, jak toouse 2.0 rozhraní příkazového řádku toocreate aplikaci Azure Active Directory (Azure AD) a hlavní tooaccess služby Azure Media Services prostředky. 

## <a name="prerequisites"></a>Požadavky

- Účet Azure. Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Účet Media Services. Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).

## <a name="use-hello-azure-cloud-shell"></a>Použití hello prostředí cloudu Azure

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Spusťte hello cloudové prostředí v horním navigačním podokně hello hello portálu.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Další informace najdete v tématu [Přehled prostředí cloudu Azure](../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-toohello-media-account-with-cli-20"></a>Vytvoření aplikace Azure AD a nakonfigurovat účet přístupu k toohello média pomocí rozhraní příkazového řádku 2.0
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Například:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

V tomto příkladu hello **oboru** je cesta hello úplné prostředků pro média hello účtu služby. Ale hello **oboru** může být na všech úrovních.

Například je může použít jeden z následujících úrovní hello:
 
* Hello **předplatné** úroveň.
* Hello **skupiny prostředků** úroveň.
* Hello **prostředků** úrovně (například účet Media).

Další informace najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Viz také [Manage Role-Based řízení přístupu pomocí rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md). 

## <a name="next-steps"></a>Další kroky

Začínáme s [nahrávání souborů tooyour účet](media-services-portal-upload-files.md).
