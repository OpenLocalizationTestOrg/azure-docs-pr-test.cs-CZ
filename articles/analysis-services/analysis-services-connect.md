---
title: "aaaConnect tooAzure služby Analysis Services | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooand načíst data ze serveru služby Analysis Services v Azure."
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
ms.openlocfilehash: 5df94492feb48034f156b72e83e1009683988fc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-analysis-services-server"></a><span data-ttu-id="cb6e3-103">Připojení serveru Azure Analysis Services tooan</span><span class="sxs-lookup"><span data-stu-id="cb6e3-103">Connect tooan Azure Analysis Services server</span></span>

<span data-ttu-id="cb6e3-104">Tento článek popisuje připojování tooa serveru pomocí modelování dat a správu aplikace jako SQL Server Management Studio (SSMS) nebo SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="cb6e3-104">This article describes connecting tooa server by using data modeling and management applications like SQL Server Management Studio (SSMS) or SQL Server Data Tools (SSDT).</span></span> <span data-ttu-id="cb6e3-105">Nebo s klientem služby generování sestav aplikace, jako je Microsoft Excel, Power BI Desktop nebo vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-105">Or, with client reporting applications like Microsoft Excel, Power BI Desktop, or custom applications.</span></span> <span data-ttu-id="cb6e3-106">Připojení tooAzure Analysis Services používat protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-106">Connections tooAzure Analysis Services use HTTPS.</span></span>

## <a name="client-libraries"></a><span data-ttu-id="cb6e3-107">Klientské knihovny</span><span class="sxs-lookup"><span data-stu-id="cb6e3-107">Client libraries</span></span>
[<span data-ttu-id="cb6e3-108">Získat nejnovější knihovny klienta hello</span><span class="sxs-lookup"><span data-stu-id="cb6e3-108">Get hello latest Client libraries</span></span>](analysis-services-data-providers.md)

<span data-ttu-id="cb6e3-109">Všechny server tooa připojení, bez ohledu na typ, vyžadovat aktualizované AMO ADOMD.NET a OLEDB knihovny tooconnect tooand rozhraní klienta se serverem služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-109">All connections tooa server, regardless of type, require updated AMO, ADOMD.NET, and OLEDB client libraries tooconnect tooand interface with an Analysis Services server.</span></span> <span data-ttu-id="cb6e3-110">Pro aplikace SSMS, rozšíření SSDT, aplikace Excel 2016 a Power BI knihovny klienta nejnovější hello instalaci nebo aktualizaci s měsíční verzemi.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-110">For SSMS, SSDT, Excel 2016, and Power BI, hello latest client libraries are installed or updated with monthly releases.</span></span> <span data-ttu-id="cb6e3-111">V některých případech je však možné že Hello nemusí mít nejnovější aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-111">However, in some cases, it's possible an application may not have hello latest.</span></span> <span data-ttu-id="cb6e3-112">Například když zásady zpoždění aktualizace nebo aktualizace Office 365 jsou na hello odložené kanálu.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-112">For example, when policies delay updates, or Office 365 updates are on hello Deferred Channel.</span></span>

## <a name="server-name"></a><span data-ttu-id="cb6e3-113">Název serveru</span><span class="sxs-lookup"><span data-stu-id="cb6e3-113">Server name</span></span>

<span data-ttu-id="cb6e3-114">Při vytváření serveru služby Analysis Services v Azure, zadejte jedinečný název a hello oblasti, kde hello server je toobe vytvořen.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-114">When you create an Analysis Services server in Azure, you specify a unique name and hello region where hello server is toobe created.</span></span> <span data-ttu-id="cb6e3-115">Při zadání názvu serveru hello v připojení, je schéma pojmenování server hello:</span><span class="sxs-lookup"><span data-stu-id="cb6e3-115">When specifying hello server name in a connection, hello server naming scheme is:</span></span>

```
<protocol>://<region>/<servername>
```
 <span data-ttu-id="cb6e3-116">Kde protokol je řetězec **asazure**, je oblast hello identifikátor Uri, které bylo vytvořeno hello serveru (například westus.asazure.windows.net) a servername je název hello jedinečný serveru v rámci oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-116">Where protocol is string **asazure**, region is hello Uri where hello server was created (for example, westus.asazure.windows.net) and servername is hello name of your unique server within hello region.</span></span>

### <a name="get-hello-server-name"></a><span data-ttu-id="cb6e3-117">Získat název serveru hello</span><span class="sxs-lookup"><span data-stu-id="cb6e3-117">Get hello server name</span></span>
<span data-ttu-id="cb6e3-118">V **portál Azure** > serveru > **přehled** > **název serveru**, název kopie hello celý server.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-118">In **Azure portal** > server > **Overview** > **Server name**, copy hello entire server name.</span></span> <span data-ttu-id="cb6e3-119">Pokud ostatní uživatelé ve vaší organizaci příliš připojení toothis serveru, můžete tento název serveru sdílet s nimi.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-119">If other users in your organization are connecting toothis server too, you can share this server name with them.</span></span> <span data-ttu-id="cb6e3-120">Při zadání názvu serveru, se musí použít celou cestu hello.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-120">When specifying a server name, hello entire path must be used.</span></span>

![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a><span data-ttu-id="cb6e3-122">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="cb6e3-122">Connection string</span></span>

<span data-ttu-id="cb6e3-123">Při připojování tooAzure Analysis Services pomocí hello tabulkový objektový Model, použijte hello následující formáty řetězců připojení:</span><span class="sxs-lookup"><span data-stu-id="cb6e3-123">When connecting tooAzure Analysis Services using hello Tabular Object Model, use hello following connection string formats:</span></span>

###### <a name="integrated-azure-active-directory-authentication"></a><span data-ttu-id="cb6e3-124">Integrované ověřování služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb6e3-124">Integrated Azure Active Directory authentication</span></span>
<span data-ttu-id="cb6e3-125">Integrované ověřování převezme hello mezipaměti přihlašovacích údajů Azure Active Directory, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-125">Integrated authentication picks up hello Azure Active Directory credential cache if available.</span></span> <span data-ttu-id="cb6e3-126">V opačném případě se zobrazí okno hello přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-126">If not, hello Azure login window is shown.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a><span data-ttu-id="cb6e3-127">Azure Active Directory ověřování pomocí uživatelského jména a hesla</span><span class="sxs-lookup"><span data-stu-id="cb6e3-127">Azure Active Directory authentication with username and password</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a><span data-ttu-id="cb6e3-128">Ověřování systému Windows (integrované zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="cb6e3-128">Windows authentication (Integrated security)</span></span>
<span data-ttu-id="cb6e3-129">Použijte účet Windows hello systémem hello aktuálním procesu.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-129">Use hello Windows account running hello current process.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a><span data-ttu-id="cb6e3-130">Připojit pomocí souboru ODC</span><span class="sxs-lookup"><span data-stu-id="cb6e3-130">Connect using an .odc file</span></span>
<span data-ttu-id="cb6e3-131">Ve starších verzích aplikace Excel uživatelé tooan Azure Analysis Services server připojte pomocí souboru (ODC) Office Data Connection.</span><span class="sxs-lookup"><span data-stu-id="cb6e3-131">With older versions of Excel, users can connect tooan Azure Analysis Services server by using an Office Data Connection (.odc) file.</span></span> <span data-ttu-id="cb6e3-132">Další, najdete v části toolearn [vytvořit soubor dat ODC (Office Connection)](analysis-services-odc.md).</span><span class="sxs-lookup"><span data-stu-id="cb6e3-132">toolearn more, see [Create an Office Data Connection (.odc) file](analysis-services-odc.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="cb6e3-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb6e3-133">Next steps</span></span>
<span data-ttu-id="cb6e3-134">[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="cb6e3-134">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
<span data-ttu-id="cb6e3-135">[Připojit k Power BI](analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="cb6e3-135">[Connect with Power BI](analysis-services-connect-pbi.md) </span></span>  
[<span data-ttu-id="cb6e3-136">Správa serveru</span><span class="sxs-lookup"><span data-stu-id="cb6e3-136">Manage your server</span></span>](analysis-services-manage.md)   

