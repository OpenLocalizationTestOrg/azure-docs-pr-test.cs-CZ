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
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Vytvoření aplikace .NET HDInsight neinteraktivní ověřování
Můžete spustit aplikace .NET Azure HDInsight v části vlastní identitu aplikace (neinteraktivní) nebo identitou hello hello přihlášeného uživatele aplikace hello (interaktivní). Ukázka hello interaktivní aplikace, najdete v části [připojit tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). Tento článek ukazuje, jak toocreate neinteraktivního ověřování .NET aplikace tooconnect tooAzure a spravovat HDInsight.

Ze neinteraktivní aplikace .NET potřebujete:

* Vaše předplatné Azure ID klienta (ID adresáře známé jako). V tématu [získání ID tenanta](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* ID klienta aplikace Azure Active Directory Hello. V tématu [vytvořit aplikaci Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), a [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)
* Hello Azure Active Directory tajný klíč aplikace. V tématu [Get aplikace ověřovací klíč](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)

## <a name="prerequisites"></a>Požadavky
* HDInsight cluster. V tématu [kurz Začínáme](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).



## <a name="assign-azure-ad-application-toorole"></a>Přiřadit toorole aplikace Azure AD
Je nutné přiřadit tooa aplikace hello [role](../active-directory/role-based-access-built-in-roles.md) toogrant ho oprávnění pro provádění akcí. Můžete nastavit obor hello na úrovni hello hello předplatné, skupinu prostředků nebo prostředek. Hello oprávnění jsou zděděné toolower úrovně oboru (například přidání aplikační role čtečky toohello pro skupinu prostředků znamená, že ho mohou číst hello skupinu prostředků a všechny prostředky, které obsahuje). V tomto kurzu nastavíte hello oboru na úrovni skupiny prostředků hello. Další informace najdete v tématu [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md)

**tooadd hello vlastníka role toohello Azure AD aplikace**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **skupiny prostředků** v levém podokně hello.
3. Klikněte na tlačítko hello skupinu prostředků, která obsahuje clusteru HDInsight hello, kde budete spouštět dotaz Hive později v tomto kurzu. Pokud jsou moc velký počet skupin prostředků, můžete použít možnosti filtrovat hello.
4. Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** nabídce hello skupiny prostředků.
5. Klikněte na tlačítko **přidat** z hello **uživatelé** okno.
6. Postupujte podle hello instrukce tooadd hello **vlastníka** role toohello aplikaci Azure AD, který jste vytvořili v posledním postupu hello. Po dokončení se úspěšně, zobrazí se aplikace hello uvedené v okně uživatelé hello s roli vlastníka hello.

## <a name="develop-hdinsight-client-application"></a>Vývoj aplikace HDInsight klienta

1. Vytvořte konzolovou aplikaci C#.
2. Přidejte následující balíčky Nuget hello:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Použijte hello následující ukázka kódu:

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

## <a name="next-steps"></a>Další kroky
* [Vytvoření aplikace Azure Active Directory a objektu zabezpečení pomocí portálu](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Ověření objektu zabezpečení pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md)
