---
title: "aaaUpdate Media Services po vrácení úložiště přístupové klíče | Microsoft Docs"
description: "Tento článek získáte informace o tom, jak tooupdate Media Services po vrácení úložiště přístupové klíče."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 26fa7a75a73397842aaebda59516a00f68ab97f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Po vrácení přístupových klíčů k úložišti aktualizuje Media Services

Když vytvoříte nový účet Azure Media Services (AMS), zobrazí se výzva také, že tooselect Azure Storage účet, který je použit toostore mediální obsah. Můžete přidat více než jeden tooyour účty úložiště účtu Media Services. Toto téma ukazuje, jak toorotate úložiště klíčů. Také ukazuje, jak účtů úložiště tooadd účtu media tooa. 

tooperform hello akce popsané v tomto tématu, měli byste použít [rozhraní API ARM](https://docs.microsoft.com/rest/api/media/mediaservice) a [prostředí Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).  Další informace najdete v tématu [jak toomanage Azure prostředků pomocí prostředí PowerShell a správce prostředků](../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Přehled

Při vytvoření nového účtu úložiště vygeneruje Azure dva 512bitové přístupových klíčů k úložišti, které jsou používané tooauthenticate přístup k účtu úložiště tooyour. tookeep připojení k úložišti bezpečnější, že se doporučuje tooperiodically znovu vygenerovat a otočit přístupový klíč k úložišti. Dva přístupové klíče (primární i sekundární) jsou k dispozici v pořadí tooenable jste toomaintain připojení toohello účet úložiště pomocí jednoho přístup klíč zatímco si znovu vygenerujete hello druhý přístupový klíč. Tento postup je také označován "postupného přístupových klíčů".

Služba Media Services, závisí na zadaný tooit klíč k úložišti. Konkrétně hello lokátory, které jsou používané toostream nebo stáhnout vaše prostředky závisí na hello zadané úložiště přístupový klíč. Při vytváření účtu AMS trvá závislost na hello primárního úložiště přístupový klíč ve výchozím nastavení ale jako uživatel, můžete aktualizovat klíč úložiště hello, který má AMS. Je nutné zkontrolujte, zda že toolet Media Services vědět, které klíče toouse podle následujících kroků popsaných v tomto tématu.  

>[!NOTE]
> Pokud máte více účtů úložiště, můžete provést tento postup se každý účet úložiště. Hello pořadí, ve kterém otočit klíčů k úložišti není pevný. Můžete nejprve otočit hello sekundární klíč a pak hello primární klíč nebo naopak naopak.
>
> Před provedením kroků popsaných v tomto tématu na produkční účtu, ujistěte se, že tootest je na předprodukční režim účtu.
>

## <a name="steps-toorotate-storage-keys"></a>Kroky toorotate úložiště klíčů 
 
 1. Změna hello klíč účtu úložiště primární prostřednictvím rutiny prostředí powershell hello nebo [Azure](https://portal.azure.com/) portálu.
 2. Volání rutiny AzureRmMediaServiceStorageKeys synchronizace s odpovídající parametry tooforce média účet toopick až klíče účtu úložiště
 
    Hello následující příklad ukazuje, jak toosync klíče toostorage účty.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Počkejte, než hodinu. Ověřte, že hello streamování scénářů pracují.
 4. Změnit sekundární klíč účtu úložiště prostřednictvím rutiny prostředí powershell hello nebo portálu Azure.
 5. Volání synchronizace AzureRmMediaServiceStorageKeys prostředí powershell s odpovídající parametry tooforce média účet toopick si nových klíčů účtu úložiště. 
 6. Počkejte, než hodinu. Ověřte, že hello streamování scénářů pracují.
 
### <a name="a-powershell-cmdlet-example"></a>V příkladu rutiny prostředí powershell 

Hello následující příklad ukazuje, jak tooget hello účtu úložiště a synchronizovat s účtem hello AMS.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-tooadd-storage-accounts-tooyour-ams-account"></a>Účet tooyour AMS účtů úložiště tooadd kroky

Hello následující téma ukazuje, jak účtů úložiště tooadd tooyour AMS účet: [připojit více tooa účty úložiště účtu Media Services](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Potvrzování
Rádi bychom znali tooacknowledge hello následující osoby podílí k vytvoření tohoto dokumentu: Cenk Dingiloglu, Gada Milán Seva Titov.
