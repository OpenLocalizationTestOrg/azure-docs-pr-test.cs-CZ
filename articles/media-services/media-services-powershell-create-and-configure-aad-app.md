---
title: "rozhraní API služby Azure Media Services hello aaaUse prostředí PowerShell toocreate tooaccess aplikace služby Azure AD | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí PowerShell toocreate aplikaci Azure Active Directory (Azure AD) a nastavit tooaccess hello rozhraní API služby Azure Media Services."
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
ms.openlocfilehash: 1a8b4a53ad10b559f6ee4242b95c5bd15ee8e903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-ad-app-toouse-with-hello-azure-media-services-api"></a>Použití prostředí PowerShell toocreate toouse aplikace služby Azure AD s hello rozhraní API služby Azure Media Services

Zjistěte, jak toouse prostředí PowerShell skriptu toocreate služby Azure Active Directory (Azure AD) aplikace a služby hlavní tooaccess prostředky Azure Media Services.  

## <a name="prerequisites"></a>Požadavky

- Účet Azure. Pokud nemáte účet, začínat [bezplatné zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Účet Media Services. Další informace najdete v tématu [vytvoření účtu Azure Media Services na portálu Azure hello](media-services-portal-create-account.md).
- Azure PowerShell verze 0.8.8 nebo vyšší verze. Další informace najdete v tématu [jak toouse prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Rutiny Azure Resource Manager.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Vytvoření aplikace Azure AD pomocí prostředí PowerShell  

```powershell
Login-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Další informace najdete v tématu hello následující články:

- [Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [Správa řízení přístupu na základě Role pomocí prostředí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
- [Konfigurování aplikace démon toomanually pomocí certifikátů](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Další kroky

Začínáme s [nahrávání souborů tooyour účet](media-services-portal-upload-files.md).
