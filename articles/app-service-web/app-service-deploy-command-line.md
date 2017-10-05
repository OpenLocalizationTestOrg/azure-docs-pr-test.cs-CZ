---
title: "Automatizovat nasazení vaší aplikace Azure pomocí nástroje příkazového řádku | Microsoft Docs"
description: "Zjistit informace o tom, jak můžete nasadit aplikaci Azure z příkazového řádku"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 8b65980c-eb75-44a2-8e0f-f9eb9e617d16
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: e0e2e65557911bcac06d4dc355f47e9331934f8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Automatizovat nasazení vaší aplikace Azure pomocí nástroje příkazového řádku
Je možné automatizovat nasazení aplikace Azure pomocí nástroje příkazového řádku. Tento článek obsahuje seznam nástrojů dostupných a užitečné odkazy, které ukazují, jak je používat v pracovním postupu nasazení. 

## <a name="msbuild"></a>Automatizovat nasazení pomocí nástroje MSBuild
Pokud použijete [Visual Studio IDE](#vs) pro vývoj, můžete použít [MSBuild](http://msbuildbook.com/) k automatizaci nic můžete provést ve vaší IDE. Můžete nakonfigurovat MSBuild a použít některou [Web Deploy](#webdeploy) nebo [FTP/FTPS](#ftp) ke kopírování souborů. Nasazení webu také můžete automatizovat celou řadu dalších týkající se nasazení úloh, jako je nasazení databáze.

Další informace o použití nástroje MSBuild příkazového řádku nasazení najdete v následujících zdrojích informací:

* [Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Desetinu v řadě kurzy o nasazení do Azure pomocí sady Visual Studio. Ukazuje, jak nasazení po nastavení Profily publikování v sadě Visual Studio pomocí příkazového řádku.
* [Uvnitř stroje Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com/). Tištěné adresáře, který zahrnuje kapitolám o tom, jak pomocí nástroje MSBuild pro nasazení.

## <a name="powershell"></a>Automatizovat nasazení v prostředí Windows PowerShell
Můžete provést nasazení funkce MSBuild nebo FTP z [prostředí Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Pokud tak učiníte, můžete také použít kolekci rutin prostředí Windows PowerShell, které usnadňují volání rozhraní API pro správu Azure REST.

Další informace najdete v následujících materiálech:

* [Nasazení webové aplikace propojené s úložišti GitHub](app-service-web-arm-from-github-provision.md)
* [Zřízení webové aplikace s databází SQL](app-service-web-arm-with-sql-database-provision.md)
* [Zřídit a nasadit mikroslužeb předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Vytváření reálných cloudových aplikací s Azure - automatizovat všechno, co](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). Elektronická kniha kapitoly, která vysvětluje, jak ukazuje elektronická kniha ukázková aplikace používá skripty prostředí Windows PowerShell k vytvoření Azure testovací prostředí a nasazení do ní. Najdete v článku [prostředky](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) odkazy na další dokumentaci prostředí Azure PowerShell.
* [Pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývojáře a testovací prostředí](../vs-azure-tools-publishing-using-powershell-scripts.md). Jak pomocí prostředí Windows PowerShell nasazení skriptů, který generuje Visual Studio.

## <a name="api"></a>Automatizovat nasazení pomocí rozhraní API pro správu rozhraní .NET
Můžete napsat kód C# k provádění funkcí nástroje MSBuild nebo FTP pro nasazení. Pokud tak učiníte, můžete přístup k Azure rozhraní API REST k provádění funkcí správy, lokality pro správu.

Další informace najdete v následujícím zdroji:

* [Automatizace všechno, co se knihovny správy Azure a rozhraní .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Úvod do rozhraní API pro správu rozhraní .NET a odkazy na další dokumentaci.

## <a name="cli"></a>Nasazení z Azure rozhraní příkazového řádku (Azure CLI)
Příkazového řádku v systému Windows, Mac nebo Linux počítače můžete použít k nasazení pomocí protokolu FTP. Pokud tak učiníte, můžete taky přejít rozhraní API pomocí rozhraní příkazového řádku Azure pro správu Azure REST.

Další informace najdete v následujícím zdroji:

* [Nástroje příkazového řádku Azure](https://azure.microsoft.com/downloads/). Stránka portálu v Azure.com informace nástroj příkazového řádku.

## <a name="webdeploy"></a>Nasazení z příkazového řádku nástroje nasazení webu
[Nasazení webové](http://www.iis.net/downloads/microsoft/web-deploy) je software společnosti Microsoft pro nasazení do služby IIS, nejen poskytující inteligentního soubor synchronizaci funkce, ale také můžete provést nebo koordinaci mnoho dalších týkající se nasazení úlohy, které nelze automatizované při použití serveru FTP. Například nasazení webu můžete nasadit novou databázi nebo aktualizace databáze spolu s vaší webové aplikace. Nasazení webu také minimalizovat čas potřebný k aktualizaci existující webový server, protože inteligentně se může zkopírovat pouze změněné soubory. Microsoft Visual Studio a Team Foundation Server je podpora pro nasazení webu předdefinované, ale můžete také použít Web Deploy přímo z příkazového řádku k automatizaci nasazení. Příkazy nasadit webové jsou velmi výkonné, ale může být jeho zvládnutí.

## <a name="more-resources"></a>Další zdroje informací
Další možností nasazení do příkazového řádku automatizace je pomocí cloudové služby, jako například [Chobotnice nasazení](http://en.wikipedia.org/wiki/Octopus_Deploy). Další informace najdete v tématu [ASP.NET nasazení aplikace na weby Azure](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Další informace o nástrojích příkazového řádku najdete v následujícím zdroji:

* [Jednoduché webové aplikace: Nasazení](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog podle David Ebbo o nástroj, který napsal, aby bylo snazší službu Web Deploy používat.
* [Webové nástroje pro nasazení](http://technet.microsoft.com/library/dd568996). Oficiální dokumentaci na webu Microsoft TechNet. Ze ale stále dobrým místem, kde spustit.
* [Pomocí webové nasadit](http://www.iis.net/learn/publish/using-web-deploy). Oficiální dokumentaci na webu Microsoft IIS.NET. Také ze ale vhodné oddělení na zahájení.
* [Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild je modul sestavení využívá sada Visual Studio a může taky sloužit z příkazového řádku pro nasazení webových aplikací do webové aplikace. V tomto kurzu je součástí série, která je hlavně o nasazení sady Visual Studio.

