---
title: "Vytvoření aplikace Azure AD pro přístup k rozhraní API služby Azure Media Services pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak pomocí prostředí PowerShell k vytvoření aplikace Azure Active Directory (Azure AD) a nastavte jej tak přístup k rozhraní API služby Azure Media Services."
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
ms.openlocfilehash: eea0f3a03dd77ce56484f32b192299bd97c05300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a><span data-ttu-id="a0f9e-103">Vytvoření aplikace Azure AD pro použití s rozhraní API služby Azure Media Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0f9e-103">Use PowerShell to create an Azure AD app to use with the Azure Media Services API</span></span>

<span data-ttu-id="a0f9e-104">Další informace o použití skriptu prostředí PowerShell k vytvoření objektu služby a aplikace Azure Active Directory (Azure AD) pro přístup k prostředkům Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="a0f9e-104">Learn how to use a PowerShell script to create an Azure Active Directory (Azure AD) application and service principal to access Azure Media Services resources.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="a0f9e-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a0f9e-105">Prerequisites</span></span>

- <span data-ttu-id="a0f9e-106">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a0f9e-106">An Azure account.</span></span> <span data-ttu-id="a0f9e-107">Pokud nemáte účet, začínat [bezplatné zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a0f9e-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="a0f9e-108">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="a0f9e-108">A Media Services account.</span></span> <span data-ttu-id="a0f9e-109">Další informace najdete v tématu [vytvoření účtu Azure Media Services na webu Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="a0f9e-109">For more information, see [Create an Azure Media Services account in the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="a0f9e-110">Azure PowerShell verze 0.8.8 nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="a0f9e-110">Azure PowerShell version 0.8.8 or a later version.</span></span> <span data-ttu-id="a0f9e-111">Další informace najdete v tématu [jak pomocí prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a0f9e-111">For more information, see [How to use Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
- <span data-ttu-id="a0f9e-112">Rutiny Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0f9e-112">Azure Resource Manager cmdlets.</span></span>  

## <a name="create-an-azure-ad-app-by-using-powershell"></a><span data-ttu-id="a0f9e-113">Vytvoření aplikace Azure AD pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0f9e-113">Create an Azure AD app by using PowerShell</span></span>  

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
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

<span data-ttu-id="a0f9e-114">Další informace najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="a0f9e-114">For more information, see the following articles:</span></span>

- [<span data-ttu-id="a0f9e-115">Vytvoření instančního objektu pro přístup k prostředkům pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="a0f9e-115">Use Azure PowerShell to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [<span data-ttu-id="a0f9e-116">Správa řízení přístupu na základě Role pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0f9e-116">Manage Role-Based Access Control by using Azure PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)
- [<span data-ttu-id="a0f9e-117">Jak ručně nakonfigurovat démon aplikace pomocí certifikátů</span><span class="sxs-lookup"><span data-stu-id="a0f9e-117">How to manually configure daemon apps by using certificates</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a><span data-ttu-id="a0f9e-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0f9e-118">Next steps</span></span>

<span data-ttu-id="a0f9e-119">Začínáme s [nahrávání souborů do účtu](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="a0f9e-119">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
