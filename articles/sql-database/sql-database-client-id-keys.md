---
title: "aaaGet hodnoty pro ověřování aplikace – Azure SQL Database | Microsoft Docs"
description: "Vytvořte objekt služby pro přístup k SQL Database z kódu."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
tags: 
ms.assetid: b43e43bb-6660-49e6-b069-abde97eb5770
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/30/2016
ms.author: sstein
ms.openlocfilehash: b57dc075ec9e679da9f2f5fa53e02312539cdf07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-required-values-for-authenticating-an-application-tooaccess-sql-database-from-code"></a>Získá hello požadované hodnoty pro ověřování aplikace tooaccess SQL Database z kódu
toocreate a spravovat databáze SQL z kódu vaší aplikace je nutné zaregistrovat v hello Azure Active Directory (AAD) domény v rámci předplatného hello kde byly vytvořeny vašich prostředků Azure.

## <a name="create-a-service-principal-tooaccess-resources-from-an-application"></a>Vytvořit hlavní tooaccess služby prostředků z aplikace
Je třeba toohave hello nejnovější [prostředí Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) nainstalovaná a spuštěná. Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

Hello následující skript prostředí PowerShell vytvoří hello Active Directory (AD) aplikace a služby hello hlavní, že potřebujeme tooauthenticate vaší aplikace C#. Hello skript vypíše hodnoty, které budeme potřebovat pro hello předcházející C# ukázkové. Podrobné informace najdete v tématu [toocreate použití Azure PowerShell objekt služby prostředků tooaccess](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in tooAzure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set toohello subscription you want toowork with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is hello display name for your app, must be unique in your directory.
    # $uri does not need toobe a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for hello app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # tooavoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun hello following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output hello values we need for our C# application toosuccessfully authenticate

    Write-Output "Copy these values into hello C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Viz také
* [Vytvoření databáze SQL pomocí C#](sql-database-get-started-csharp.md)
* [Připojení tooSQL ověřování databáze s pomocí pomocí Azure Active Directory](sql-database-aad-authentication.md)

