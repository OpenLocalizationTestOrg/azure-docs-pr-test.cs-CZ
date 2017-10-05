---
title: "Připojení ke službě Azure Analysis Services | Microsoft Docs"
description: "Zjistěte, jak se připojit k a získat data ze serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: deb3ef28d20decef01826450bd6091f87dd069de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-an-azure-analysis-services-server"></a><span data-ttu-id="9c8ee-103">Připojit k serveru Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="9c8ee-103">Connect to an Azure Analysis Services server</span></span>

<span data-ttu-id="9c8ee-104">Tento článek popisuje připojení k serveru pomocí modelování dat a správu aplikace jako SQL Server Management Studio (SSMS) nebo SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="9c8ee-104">This article describes connecting to a server by using data modeling and management applications like SQL Server Management Studio (SSMS) or SQL Server Data Tools (SSDT).</span></span> <span data-ttu-id="9c8ee-105">Nebo s klientem služby generování sestav aplikace, jako je Microsoft Excel, Power BI Desktop nebo vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-105">Or, with client reporting applications like Microsoft Excel, Power BI Desktop, or custom applications.</span></span> <span data-ttu-id="9c8ee-106">Připojení k Azure Analysis Services používat protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-106">Connections to Azure Analysis Services use HTTPS.</span></span>

## <a name="client-libraries"></a><span data-ttu-id="9c8ee-107">Klientské knihovny</span><span class="sxs-lookup"><span data-stu-id="9c8ee-107">Client libraries</span></span>
[<span data-ttu-id="9c8ee-108">Získat nejnovější knihovny klienta</span><span class="sxs-lookup"><span data-stu-id="9c8ee-108">Get the latest Client libraries</span></span>](analysis-services-data-providers.md)

<span data-ttu-id="9c8ee-109">Všechna připojení k serveru, bez ohledu na typ, vyžadují aktualizované AMO ADOMD.NET a OLEDB klientské knihovny pro připojení k a rozhraní se serverem služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-109">All connections to a server, regardless of type, require updated AMO, ADOMD.NET, and OLEDB client libraries to connect to and interface with an Analysis Services server.</span></span> <span data-ttu-id="9c8ee-110">Pro aplikace SSMS, rozšíření SSDT, aplikace Excel 2016 a Power BI nejnovější knihovny klienta instalaci nebo aktualizaci s měsíční verzemi.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-110">For SSMS, SSDT, Excel 2016, and Power BI, the latest client libraries are installed or updated with monthly releases.</span></span> <span data-ttu-id="9c8ee-111">V některých případech je však možné že aplikace nemusí mít nejnovější.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-111">However, in some cases, it's possible an application may not have the latest.</span></span> <span data-ttu-id="9c8ee-112">Například když zpoždění zásady aktualizace nebo aktualizace Office 365 jsou na odložené kanálu.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-112">For example, when policies delay updates, or Office 365 updates are on the Deferred Channel.</span></span>

## <a name="server-name"></a><span data-ttu-id="9c8ee-113">Název serveru</span><span class="sxs-lookup"><span data-stu-id="9c8ee-113">Server name</span></span>

<span data-ttu-id="9c8ee-114">Při vytváření serveru služby Analysis Services v Azure, je třeba zadat jedinečný název a oblasti, kde je server, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-114">When you create an Analysis Services server in Azure, you specify a unique name and the region where the server is to be created.</span></span> <span data-ttu-id="9c8ee-115">Při zadávání názvu serveru v připojení, je schéma pojmenování serveru:</span><span class="sxs-lookup"><span data-stu-id="9c8ee-115">When specifying the server name in a connection, the server naming scheme is:</span></span>

```
<protocol>://<region>/<servername>
```
 <span data-ttu-id="9c8ee-116">Kde protokol je řetězec **asazure**, oblast je identifikátor Uri, které bylo vytvořeno serveru (například westus.asazure.windows.net) a servername je název jedinečný serveru v rámci oblasti.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-116">Where protocol is string **asazure**, region is the Uri where the server was created (for example, westus.asazure.windows.net) and servername is the name of your unique server within the region.</span></span>

### <a name="get-the-server-name"></a><span data-ttu-id="9c8ee-117">Získat název serveru</span><span class="sxs-lookup"><span data-stu-id="9c8ee-117">Get the server name</span></span>
<span data-ttu-id="9c8ee-118">V **portál Azure** > serveru > **přehled** > **název serveru**, zkopírujte název celý server.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-118">In **Azure portal** > server > **Overview** > **Server name**, copy the entire server name.</span></span> <span data-ttu-id="9c8ee-119">Pokud ostatní uživatelé ve vaší organizaci se připojují k tomuto serveru příliš, můžete tento název serveru sdílet s nimi.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-119">If other users in your organization are connecting to this server too, you can share this server name with them.</span></span> <span data-ttu-id="9c8ee-120">Při zadávání názvu serveru, třeba zadat celou cestu.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-120">When specifying a server name, the entire path must be used.</span></span>

![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a><span data-ttu-id="9c8ee-122">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="9c8ee-122">Connection string</span></span>

<span data-ttu-id="9c8ee-123">Při připojování k serveru Azure Analysis Services pomocí tabulkový objektový Model, použijte následující formáty řetězec připojení:</span><span class="sxs-lookup"><span data-stu-id="9c8ee-123">When connecting to Azure Analysis Services using the Tabular Object Model, use the following connection string formats:</span></span>

###### <a name="integrated-azure-active-directory-authentication"></a><span data-ttu-id="9c8ee-124">Integrované ověřování služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c8ee-124">Integrated Azure Active Directory authentication</span></span>
<span data-ttu-id="9c8ee-125">Integrované ověřování převezme mezipaměti přihlašovacích údajů Azure Active Directory, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-125">Integrated authentication picks up the Azure Active Directory credential cache if available.</span></span> <span data-ttu-id="9c8ee-126">V opačném případě se zobrazí okno přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-126">If not, the Azure login window is shown.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a><span data-ttu-id="9c8ee-127">Azure Active Directory ověřování pomocí uživatelského jména a hesla</span><span class="sxs-lookup"><span data-stu-id="9c8ee-127">Azure Active Directory authentication with username and password</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a><span data-ttu-id="9c8ee-128">Ověřování systému Windows (integrované zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="9c8ee-128">Windows authentication (Integrated security)</span></span>
<span data-ttu-id="9c8ee-129">Použijte účet systému Windows s aktuálním procesu.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-129">Use the Windows account running the current process.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a><span data-ttu-id="9c8ee-130">Připojit pomocí souboru ODC</span><span class="sxs-lookup"><span data-stu-id="9c8ee-130">Connect using an .odc file</span></span>
<span data-ttu-id="9c8ee-131">Ve starších verzích aplikace Excel mohou uživatelé připojit k serveru Azure Analysis Services pomocí souboru (ODC) Office Data Connection.</span><span class="sxs-lookup"><span data-stu-id="9c8ee-131">With older versions of Excel, users can connect to an Azure Analysis Services server by using an Office Data Connection (.odc) file.</span></span> <span data-ttu-id="9c8ee-132">Další informace najdete v tématu [vytvořit soubor dat ODC (Office Connection)](analysis-services-odc.md).</span><span class="sxs-lookup"><span data-stu-id="9c8ee-132">To learn more, see [Create an Office Data Connection (.odc) file](analysis-services-odc.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="9c8ee-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c8ee-133">Next steps</span></span>
<span data-ttu-id="9c8ee-134">[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="9c8ee-134">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
<span data-ttu-id="9c8ee-135">[Připojit k Power BI](analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="9c8ee-135">[Connect with Power BI](analysis-services-connect-pbi.md) </span></span>  
[<span data-ttu-id="9c8ee-136">Správa serveru</span><span class="sxs-lookup"><span data-stu-id="9c8ee-136">Manage your server</span></span>](analysis-services-manage.md)   

