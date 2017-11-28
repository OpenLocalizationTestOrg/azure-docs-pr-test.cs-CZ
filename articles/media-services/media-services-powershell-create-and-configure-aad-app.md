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
# <a name="use-powershell-toocreate-an-azure-ad-app-toouse-with-hello-azure-media-services-api"></a><span data-ttu-id="59984-103">Použití prostředí PowerShell toocreate toouse aplikace služby Azure AD s hello rozhraní API služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="59984-103">Use PowerShell toocreate an Azure AD app toouse with hello Azure Media Services API</span></span>

<span data-ttu-id="59984-104">Zjistěte, jak toouse prostředí PowerShell skriptu toocreate služby Azure Active Directory (Azure AD) aplikace a služby hlavní tooaccess prostředky Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="59984-104">Learn how toouse a PowerShell script toocreate an Azure Active Directory (Azure AD) application and service principal tooaccess Azure Media Services resources.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="59984-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="59984-105">Prerequisites</span></span>

- <span data-ttu-id="59984-106">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="59984-106">An Azure account.</span></span> <span data-ttu-id="59984-107">Pokud nemáte účet, začínat [bezplatné zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="59984-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="59984-108">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="59984-108">A Media Services account.</span></span> <span data-ttu-id="59984-109">Další informace najdete v tématu [vytvoření účtu Azure Media Services na portálu Azure hello](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="59984-109">For more information, see [Create an Azure Media Services account in hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="59984-110">Azure PowerShell verze 0.8.8 nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="59984-110">Azure PowerShell version 0.8.8 or a later version.</span></span> <span data-ttu-id="59984-111">Další informace najdete v tématu [jak toouse prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="59984-111">For more information, see [How toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
- <span data-ttu-id="59984-112">Rutiny Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="59984-112">Azure Resource Manager cmdlets.</span></span>  

## <a name="create-an-azure-ad-app-by-using-powershell"></a><span data-ttu-id="59984-113">Vytvoření aplikace Azure AD pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="59984-113">Create an Azure AD app by using PowerShell</span></span>  

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

<span data-ttu-id="59984-114">Další informace najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="59984-114">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="59984-115">Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="59984-115">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [<span data-ttu-id="59984-116">Správa řízení přístupu na základě Role pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="59984-116">Manage Role-Based Access Control by using Azure PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)
- [<span data-ttu-id="59984-117">Konfigurování aplikace démon toomanually pomocí certifikátů</span><span class="sxs-lookup"><span data-stu-id="59984-117">How toomanually configure daemon apps by using certificates</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a><span data-ttu-id="59984-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59984-118">Next steps</span></span>

<span data-ttu-id="59984-119">Začínáme s [nahrávání souborů tooyour účet](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="59984-119">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
