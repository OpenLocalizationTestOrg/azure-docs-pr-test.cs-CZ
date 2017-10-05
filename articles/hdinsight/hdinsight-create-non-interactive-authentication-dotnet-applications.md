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
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Vytvoření aplikace .NET HDInsight neinteraktivní ověřování
Můžete spustit aplikace .NET Azure HDInsight v části vlastní identitu aplikace (neinteraktivní) nebo v části identita přihlášeného uživatele (interaktivní) aplikace. Ukázka interaktivní aplikace, najdete v části [připojit k Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). Tento článek ukazuje, jak vytvořit aplikaci .NET neinteraktivního ověřování pro připojení k Azure a spravovat HDInsight.

Ze neinteraktivní aplikace .NET potřebujete:

* Vaše předplatné Azure ID klienta (ID adresáře známé jako). V tématu [získání ID tenanta](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* ID klienta aplikace Azure Active Directory. V tématu [vytvořit aplikaci Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), a [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)
* Aplikace Azure Active Directory tajný klíč. V tématu [Get aplikace ověřovací klíč](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)

## <a name="prerequisites"></a>Požadavky
* HDInsight cluster. V tématu [kurz Začínáme](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).



## <a name="assign-azure-ad-application-to-role"></a>Přiřaďte aplikaci Azure AD k roli
Je nutné přiřadit aplikaci [role](../active-directory/role-based-access-built-in-roles.md) jí udělit oprávnění pro provádění akcí. Rozsah můžete nastavit na úrovni předplatné, skupinu prostředků nebo prostředek. Oprávnění jsou děděné na nižších úrovních oboru (například přidání aplikace do role Čtenář pro skupinu prostředků znamená, že ho mohou číst skupině prostředků a všechny prostředky, které obsahuje). V tomto kurzu nastavíte oboru na úrovni skupiny prostředků. Další informace najdete v tématu [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](../active-directory/role-based-access-control-configure.md)

**Chcete-li přidat roli vlastníka do aplikace Azure AD**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **skupiny prostředků** v levém podokně.
3. Klikněte na skupinu prostředků, která obsahuje clusteru HDInsight, kde budete spouštět dotaz Hive později v tomto kurzu. Pokud jsou moc velký počet skupin prostředků, můžete použít filtr.
4. Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** z nabídky skupiny prostředků.
5. Klikněte na tlačítko **přidat** z **uživatelé** okno.
6. Postupujte podle pokynů pro přidání **vlastníka** role do aplikace Azure AD, který jste vytvořili v posledním postupu. Po dokončení se úspěšně, zobrazí se aplikace uvedené v okně uživatelé s rolí vlastníka.

## <a name="develop-hdinsight-client-application"></a>Vývoj aplikace HDInsight klienta

1. Vytvořte konzolovou aplikaci C#.
2. Přidejte následující balíčky Nuget:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Použijte následující ukázka kódu:

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

## <a name="next-steps"></a>Další kroky
* [Vytvoření aplikace Azure Active Directory a objektu zabezpečení pomocí portálu](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Ověření objektu zabezpečení pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md)
