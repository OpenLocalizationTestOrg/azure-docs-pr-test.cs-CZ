---
title: "Vytvoření prošetřovat žádosti neinteraktivního ověřování rozhraní .NET HDInsight - Azure | Microsoft Docs"
description: "Naučte se vytvářet neinteraktivního ověřování aplikace .NET HDInsight."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 8e32430f-6404-498a-9fcd-f20338d964af
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 7821a9e60e60ff01cff06db2a6f216a260c1c41a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a><span data-ttu-id="48d02-103">Vytvoření aplikace .NET HDInsight neinteraktivní ověřování</span><span class="sxs-lookup"><span data-stu-id="48d02-103">Create non-interactive authentication .NET HDInsight applications</span></span>
<span data-ttu-id="48d02-104">Můžete spustit aplikace .NET Azure HDInsight v části vlastní identitu aplikace (neinteraktivní) nebo v části identita přihlášeného uživatele (interaktivní) aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d02-104">You can run your .NET Azure HDInsight application either under application's own identity (non-interactive) or under the identity of the signed-in user of the application (interactive).</span></span> <span data-ttu-id="48d02-105">Ukázka interaktivní aplikace, najdete v části [připojit k Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span><span class="sxs-lookup"><span data-stu-id="48d02-105">For a sample of the interactive application, see [Connect to Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span></span> <span data-ttu-id="48d02-106">Tento článek ukazuje, jak vytvořit aplikaci .NET neinteraktivního ověřování pro připojení k Azure a spravovat HDInsight.</span><span class="sxs-lookup"><span data-stu-id="48d02-106">This article shows you how to create non-interactive authentication .NET application to connect to Azure and manage HDInsight.</span></span>

<span data-ttu-id="48d02-107">Ze neinteraktivní aplikace .NET potřebujete:</span><span class="sxs-lookup"><span data-stu-id="48d02-107">From your non-interactive .NET application, you need:</span></span>

* <span data-ttu-id="48d02-108">Vaše předplatné Azure ID klienta (ID adresáře známé jako).</span><span class="sxs-lookup"><span data-stu-id="48d02-108">Your Azure subscription tenant ID (A.K.A directory ID).</span></span> <span data-ttu-id="48d02-109">V tématu [získání ID tenanta](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span><span class="sxs-lookup"><span data-stu-id="48d02-109">See [Get tenant ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span></span>
* <span data-ttu-id="48d02-110">ID klienta aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="48d02-110">The Azure Active Directory application client ID.</span></span> <span data-ttu-id="48d02-111">V tématu [vytvořit aplikaci Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), a [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="48d02-111">See [Create an Azure Active Directory application](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), and [Get an application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>
* <span data-ttu-id="48d02-112">Aplikace Azure Active Directory tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="48d02-112">The Azure Active Directory application secret key.</span></span> <span data-ttu-id="48d02-113">V tématu [Get aplikace ověřovací klíč](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="48d02-113">See [Get application authentication key](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48d02-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="48d02-114">Prerequisites</span></span>
* <span data-ttu-id="48d02-115">HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="48d02-115">HDInsight cluster.</span></span> <span data-ttu-id="48d02-116">V tématu [kurz Začínáme](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="48d02-116">See [getting started tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span>



## <a name="assign-azure-ad-application-to-role"></a><span data-ttu-id="48d02-117">Přiřaďte aplikaci Azure AD k roli</span><span class="sxs-lookup"><span data-stu-id="48d02-117">Assign Azure AD application to role</span></span>
<span data-ttu-id="48d02-118">Je nutné přiřadit aplikaci [role](../active-directory/role-based-access-built-in-roles.md) jí udělit oprávnění pro provádění akcí.</span><span class="sxs-lookup"><span data-stu-id="48d02-118">You must assign the application to a [role](../active-directory/role-based-access-built-in-roles.md) to grant it permissions for performing actions.</span></span> <span data-ttu-id="48d02-119">Rozsah můžete nastavit na úrovni předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="48d02-119">You can set the scope at the level of the subscription, resource group, or resource.</span></span> <span data-ttu-id="48d02-120">Oprávnění jsou děděné na nižších úrovních oboru (například přidání aplikace do role Čtenář pro skupinu prostředků znamená, že ho mohou číst skupině prostředků a všechny prostředky, které obsahuje).</span><span class="sxs-lookup"><span data-stu-id="48d02-120">The permissions are inherited to lower levels of scope (for example, adding an application to the Reader role for a resource group means it can read the resource group and any resources it contains).</span></span> <span data-ttu-id="48d02-121">V tomto kurzu nastavíte oboru na úrovni skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="48d02-121">In this tutorial, you will set the scope at the resource group level.</span></span> <span data-ttu-id="48d02-122">Další informace najdete v tématu [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="48d02-122">For more information, see [Use role assignments to manage access to your Azure subscription resources](../active-directory/role-based-access-control-configure.md)</span></span>

<span data-ttu-id="48d02-123">**Chcete-li přidat roli vlastníka do aplikace Azure AD**</span><span class="sxs-lookup"><span data-stu-id="48d02-123">**To add the Owner role to the Azure AD application**</span></span>

1. <span data-ttu-id="48d02-124">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48d02-124">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="48d02-125">Klikněte na tlačítko **skupiny prostředků** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="48d02-125">Click **Resource Group** from the left pane.</span></span>
3. <span data-ttu-id="48d02-126">Klikněte na skupinu prostředků, která obsahuje clusteru HDInsight, kde budete spouštět dotaz Hive později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="48d02-126">Click the resource group that contains the HDInsight cluster where you will run your Hive query later in this tutorial.</span></span> <span data-ttu-id="48d02-127">Pokud jsou moc velký počet skupin prostředků, můžete použít filtr.</span><span class="sxs-lookup"><span data-stu-id="48d02-127">If there are too many resource groups, you can use the filter.</span></span>
4. <span data-ttu-id="48d02-128">Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** z nabídky skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="48d02-128">Click **Access control (IAM)** from the resource group menu.</span></span>
5. <span data-ttu-id="48d02-129">Klikněte na tlačítko **přidat** z **uživatelé** okno.</span><span class="sxs-lookup"><span data-stu-id="48d02-129">Click **Add** from the **Users** blade.</span></span>
6. <span data-ttu-id="48d02-130">Postupujte podle pokynů pro přidání **vlastníka** role do aplikace Azure AD, který jste vytvořili v posledním postupu.</span><span class="sxs-lookup"><span data-stu-id="48d02-130">Follow the instruction to add the **Owner** role to the Azure AD application you created in the last procedure.</span></span> <span data-ttu-id="48d02-131">Po dokončení se úspěšně, zobrazí se aplikace uvedené v okně uživatelé s rolí vlastníka.</span><span class="sxs-lookup"><span data-stu-id="48d02-131">When you complete it successfully, you shall see the application listed in the Users blade with the Owner role.</span></span>

## <a name="develop-hdinsight-client-application"></a><span data-ttu-id="48d02-132">Vývoj aplikace HDInsight klienta</span><span class="sxs-lookup"><span data-stu-id="48d02-132">Develop HDInsight client application</span></span>

1. <span data-ttu-id="48d02-133">Vytvořte konzolovou aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="48d02-133">Create a C# console application.</span></span>
2. <span data-ttu-id="48d02-134">Přidejte následující balíčky Nuget:</span><span class="sxs-lookup"><span data-stu-id="48d02-134">Add the following Nuget packages:</span></span>

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. <span data-ttu-id="48d02-135">Použijte následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="48d02-135">Use the following code sample:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.HDInsight;
        
        namespace CreateHDICluster
        {
            internal class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;
        
                private static Guid SubscriptionId = new Guid("<Enter Your Azure Subscription ID>");
                private static string tenantID = "<Enter Your Tenant ID (A.K.A. Directory ID)>";
                private static string applicationID = "<Enter Your Application ID>";
                private static string secretKey = "<Enter the Application Secret Key>";
        
                private static void Main(string[] args)
                {
                    var key = new SecureString();
                    foreach (char c in secretKey) { key.AppendChar(c); }

                    var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                    var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
        
                    var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                    resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        
                    _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
        
                    var results = _hdiManagementClient.Clusters.List();
                    foreach (var name in results.Clusters)
                    {
                        Console.WriteLine("Cluster Name: " + name.Name);
                        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                        Console.WriteLine("\t Cluster location: " + name.Location);
                        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                    }
                    Console.WriteLine("Press Enter to continue");
                    Console.ReadLine();
                }

                /// Get the access token for a service principal and provided key                
                public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
                {
                    var authFactory = new AuthenticationFactory();
                    var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                    var accessToken =
                        authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
        
                    return new TokenCloudCredentials(accessToken);
                }
        
                public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
                }
            }
        }

## <a name="next-steps"></a><span data-ttu-id="48d02-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48d02-136">Next steps</span></span>
* [<span data-ttu-id="48d02-137">Vytvoření aplikace Azure Active Directory a objektu zabezpečení pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="48d02-137">Create Azure Active Directory application and service principal using portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="48d02-138">Ověření objektu zabezpečení pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="48d02-138">Authenticate service principal with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="48d02-139">Řízení přístupu Azure na základě rolí</span><span class="sxs-lookup"><span data-stu-id="48d02-139">Azure Role-Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
