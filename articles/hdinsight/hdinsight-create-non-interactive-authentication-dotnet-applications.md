---
title: "neinteraktivní ověřování aaaCreate prošetřovat žádosti rozhraní .NET HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak aplikace rozhraní .NET HDInsight toocreate neinteraktivního ověřování."
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
ms.openlocfilehash: 5367c160b0146e6b855486b95f363e8fe7f1c98f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a><span data-ttu-id="b8ee8-103">Vytvoření aplikace .NET HDInsight neinteraktivní ověřování</span><span class="sxs-lookup"><span data-stu-id="b8ee8-103">Create non-interactive authentication .NET HDInsight applications</span></span>
<span data-ttu-id="b8ee8-104">Můžete spustit aplikace .NET Azure HDInsight v části vlastní identitu aplikace (neinteraktivní) nebo identitou hello hello přihlášeného uživatele aplikace hello (interaktivní).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-104">You can run your .NET Azure HDInsight application either under application's own identity (non-interactive) or under hello identity of hello signed-in user of hello application (interactive).</span></span> <span data-ttu-id="b8ee8-105">Ukázka hello interaktivní aplikace, najdete v části [připojit tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-105">For a sample of hello interactive application, see [Connect tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span></span> <span data-ttu-id="b8ee8-106">Tento článek ukazuje, jak toocreate neinteraktivního ověřování .NET aplikace tooconnect tooAzure a spravovat HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-106">This article shows you how toocreate non-interactive authentication .NET application tooconnect tooAzure and manage HDInsight.</span></span>

<span data-ttu-id="b8ee8-107">Ze neinteraktivní aplikace .NET potřebujete:</span><span class="sxs-lookup"><span data-stu-id="b8ee8-107">From your non-interactive .NET application, you need:</span></span>

* <span data-ttu-id="b8ee8-108">Vaše předplatné Azure ID klienta (ID adresáře známé jako).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-108">Your Azure subscription tenant ID (A.K.A directory ID).</span></span> <span data-ttu-id="b8ee8-109">V tématu [získání ID tenanta](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-109">See [Get tenant ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span></span>
* <span data-ttu-id="b8ee8-110">ID klienta aplikace Azure Active Directory Hello.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-110">hello Azure Active Directory application client ID.</span></span> <span data-ttu-id="b8ee8-111">V tématu [vytvořit aplikaci Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), a [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="b8ee8-111">See [Create an Azure Active Directory application](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), and [Get an application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>
* <span data-ttu-id="b8ee8-112">Hello Azure Active Directory tajný klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-112">hello Azure Active Directory application secret key.</span></span> <span data-ttu-id="b8ee8-113">V tématu [Get aplikace ověřovací klíč](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="b8ee8-113">See [Get application authentication key](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8ee8-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8ee8-114">Prerequisites</span></span>
* <span data-ttu-id="b8ee8-115">HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-115">HDInsight cluster.</span></span> <span data-ttu-id="b8ee8-116">V tématu [kurz Začínáme](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-116">See [getting started tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span>



## <a name="assign-azure-ad-application-toorole"></a><span data-ttu-id="b8ee8-117">Přiřadit toorole aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8ee8-117">Assign Azure AD application toorole</span></span>
<span data-ttu-id="b8ee8-118">Je nutné přiřadit tooa aplikace hello [role](../active-directory/role-based-access-built-in-roles.md) toogrant ho oprávnění pro provádění akcí.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-118">You must assign hello application tooa [role](../active-directory/role-based-access-built-in-roles.md) toogrant it permissions for performing actions.</span></span> <span data-ttu-id="b8ee8-119">Můžete nastavit obor hello na úrovni hello hello předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-119">You can set hello scope at hello level of hello subscription, resource group, or resource.</span></span> <span data-ttu-id="b8ee8-120">Hello oprávnění jsou zděděné toolower úrovně oboru (například přidání aplikační role čtečky toohello pro skupinu prostředků znamená, že ho mohou číst hello skupinu prostředků a všechny prostředky, které obsahuje).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-120">hello permissions are inherited toolower levels of scope (for example, adding an application toohello Reader role for a resource group means it can read hello resource group and any resources it contains).</span></span> <span data-ttu-id="b8ee8-121">V tomto kurzu nastavíte hello oboru na úrovni skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-121">In this tutorial, you will set hello scope at hello resource group level.</span></span> <span data-ttu-id="b8ee8-122">Další informace najdete v tématu [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="b8ee8-122">For more information, see [Use role assignments toomanage access tooyour Azure subscription resources](../active-directory/role-based-access-control-configure.md)</span></span>

<span data-ttu-id="b8ee8-123">**tooadd hello vlastníka role toohello Azure AD aplikace**</span><span class="sxs-lookup"><span data-stu-id="b8ee8-123">**tooadd hello Owner role toohello Azure AD application**</span></span>

1. <span data-ttu-id="b8ee8-124">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b8ee8-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b8ee8-125">Klikněte na tlačítko **skupiny prostředků** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-125">Click **Resource Group** from hello left pane.</span></span>
3. <span data-ttu-id="b8ee8-126">Klikněte na tlačítko hello skupinu prostředků, která obsahuje clusteru HDInsight hello, kde budete spouštět dotaz Hive později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-126">Click hello resource group that contains hello HDInsight cluster where you will run your Hive query later in this tutorial.</span></span> <span data-ttu-id="b8ee8-127">Pokud jsou moc velký počet skupin prostředků, můžete použít možnosti filtrovat hello.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-127">If there are too many resource groups, you can use hello filter.</span></span>
4. <span data-ttu-id="b8ee8-128">Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** nabídce hello skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-128">Click **Access control (IAM)** from hello resource group menu.</span></span>
5. <span data-ttu-id="b8ee8-129">Klikněte na tlačítko **přidat** z hello **uživatelé** okno.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-129">Click **Add** from hello **Users** blade.</span></span>
6. <span data-ttu-id="b8ee8-130">Postupujte podle hello instrukce tooadd hello **vlastníka** role toohello aplikaci Azure AD, který jste vytvořili v posledním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-130">Follow hello instruction tooadd hello **Owner** role toohello Azure AD application you created in hello last procedure.</span></span> <span data-ttu-id="b8ee8-131">Po dokončení se úspěšně, zobrazí se aplikace hello uvedené v okně uživatelé hello s roli vlastníka hello.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-131">When you complete it successfully, you shall see hello application listed in hello Users blade with hello Owner role.</span></span>

## <a name="develop-hdinsight-client-application"></a><span data-ttu-id="b8ee8-132">Vývoj aplikace HDInsight klienta</span><span class="sxs-lookup"><span data-stu-id="b8ee8-132">Develop HDInsight client application</span></span>

1. <span data-ttu-id="b8ee8-133">Vytvořte konzolovou aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="b8ee8-133">Create a C# console application.</span></span>
2. <span data-ttu-id="b8ee8-134">Přidejte následující balíčky Nuget hello:</span><span class="sxs-lookup"><span data-stu-id="b8ee8-134">Add hello following Nuget packages:</span></span>

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. <span data-ttu-id="b8ee8-135">Použijte hello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="b8ee8-135">Use hello following code sample:</span></span>

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
                private static string secretKey = "<Enter hello Application Secret Key>";
        
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
                    Console.WriteLine("Press Enter toocontinue");
                    Console.ReadLine();
                }

                /// Get hello access token for a service principal and provided key                
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

## <a name="next-steps"></a><span data-ttu-id="b8ee8-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8ee8-136">Next steps</span></span>
* [<span data-ttu-id="b8ee8-137">Vytvoření aplikace Azure Active Directory a objektu zabezpečení pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="b8ee8-137">Create Azure Active Directory application and service principal using portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="b8ee8-138">Ověření objektu zabezpečení pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b8ee8-138">Authenticate service principal with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="b8ee8-139">Řízení přístupu Azure na základě rolí</span><span class="sxs-lookup"><span data-stu-id="b8ee8-139">Azure Role-Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
