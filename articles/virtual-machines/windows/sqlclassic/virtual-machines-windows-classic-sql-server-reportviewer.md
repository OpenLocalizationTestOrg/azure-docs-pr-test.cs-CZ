---
title: "aaaUse prohlížeče sestav na webu | Microsoft Docs"
description: "Toto téma popisuje, jak toobuild webu služby Microsoft Azure s hello ovládací prvek prohlížeče sestav Visual Studio, který se zobrazí zpráva uložené na virtuální počítač Azure Microsoft."
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 78b76318-d9bf-48ef-9d9e-d1b7d8cf3042
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: 8e0729d6657f96c32a9ac7dffdff7745ff92b48d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Použití ReportVieweru na webu hostovaném v Azure
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Můžete vytvořit web Microsoft Azure s hello ovládací prvek prohlížeče sestav Visual Studio, který se zobrazí zpráva uložené na virtuální počítač Azure Microsoft. Hello ovládacího prvku prohlížeče sestav je ve webové aplikaci sestavení pomocí hello šablony ASP.NET webové aplikace.

> [!IMPORTANT]
> šablony webové aplikace ASP.NET MVC Hello nepodporují hello ovládacího prvku prohlížeče sestav.

tooincorporate prohlížeče sestav do vašeho webu Microsoft Azure, musíte toocomplete hello následující úlohy.

* **Přidat** toohello sestavení balíčku pro nasazení
* **Konfigurace** ověřování a autorizace
* **Publikování** hello tooAzure ASP.NET – webové aplikace

## <a name="prerequisites"></a>Požadavky
Projděte si část "obecné doporučení a osvědčené postupy" hello v [SQL Server Business Intelligence v Azure Virtual Machines](../classic/ps-sql-bi.md).

> [!NOTE]
> Ovládací prvky prohlížeče sestav jsou dodávané s Visual Studio, Standard Edition nebo vyšší. Pokud používáte hello Web Developer Express Edition, je nutné nainstalovat hello [MICROSOFT sestavy VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) toouse hello prohlížeče sestav runtime funkce.
> 
> Prohlížeče sestav nakonfigurovaný v místním zpracování režimu není podporována v Microsoft Azure.

## <a name="adding-assemblies-toohello-deployment-package"></a>Přidání toohello sestavení balíčku pro nasazení
Pokud je hostitelem ASP.NET aplikace místní, hello prohlížeče sestav sestavení obvykle instalovat přímo v hello globální mezipaměti sestavení (GAC) serveru služby IIS hello při instalaci sady Visual Studio a je přístupný přímo z aplikace hello. Pokud jste hostitele, které vaše aplikace ASP.NET v hello cloudu Microsoft Azure nepovoluje nic, co toobe nainstalované do hello GAC, proto je nutné nejprve sestavení hello prohlížeče sestav jsou však k dispozici místně pro vaši aplikaci. Můžete to udělat tak, že přidáte odkazy na toothem ve vašem projektu a nakonfigurovat je toobe zkopírovali do místního úložiště.

V režimu vzdálené zpracování hello ovládacího prvku prohlížeče sestav používá hello následující sestavení:

* **Microsoft.ReportViewer.WebForms.dll**: obsahuje hello prohlížeče sestav kód, který potřebujete toouse prohlížeče sestav v stránku. Při umístění ovládacího prvku prohlížeče sestav na stránky ASP.NET ve vašem projektu, je přidán odkaz pro toto sestavení tooyour projektu.
* **Microsoft.ReportViewer.Common.dll**: obsahuje třídy používané nástrojem hello ovládacího prvku prohlížeče sestav za běhu. Nebyla přidána automaticky tooyour projektu.

### <a name="tooadd-a-reference-toomicrosoftreportviewercommon"></a>tooadd tooMicrosoft.ReportViewer.Common odkaz
* Klikněte pravým tlačítkem na projekt **odkazy** uzel a vyberte možnost **přidat odkaz na**, vyberte hello sestavení v kartě hello .NET a klikněte na tlačítko **OK**.

### <a name="toomake-hello-assemblies-locally-accessible-by-your-aspnet-application"></a>sestavení hello toomake místně přístupný pro vaši aplikaci ASP.NET
1. V hello **odkazy** složku, klikněte na tlačítko hello Microsoft.ReportViewer.Common sestavení tak, aby se v podokně Vlastnosti hello objeví jeho vlastnosti.
2. V podokně Vlastnosti hello nastavte **místní kopie** tooTrue.
3. Opakujte kroky 1 a 2 pro Microsoft.ReportViewer.WebForms.

### <a name="tooget-reportviewer-language-pack"></a>tooget jazyková sada prohlížeče sestav
1. Nainstalujte hello odpovídající Microsoft sestavy Viewer 2012 Runtime redistribuovatelný balíček z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).
2. Vyberte hello jazyka z rozevíracího seznamu hello a stránku hello získá přesměrovaného toohello odpovídající download center stránky.
3. Klikněte na tlačítko **Stáhnout** toostart hello stahování ReportViewerLP.exe.
4. Po stažení ReportViewerLP.exe, klikněte na tlačítko **spustit** tooinstall okamžitě, nebo klikněte na tlačítko **Uložit** toosave ho tooyour počítače. Pokud kliknete na tlačítko **Uložit**, mějte na paměti, hello název hello složky, kam jste uložili soubor hello.
5. Vyhledejte hello složku, kam jste uložili soubor hello. Klikněte pravým tlačítkem na ReportViewerLP.exe, klikněte na tlačítko **spustit jako správce**a potom klikněte na **Ano**.
6. Po spuštění ReportViewerLP.exe, uvidíte hello c:\windows\assembly má soubory prostředků hello **Microsoft.ReportViewer.Webforms.Resources** a **Microsoft.ReportViewer.Common.Resources** .

### <a name="tooconfigure-for-localized-reportviewer-control"></a>tooconfigure pro lokalizované ovládacího prvku prohlížeče sestav
1. Stáhněte a nainstalujte redistribuovatelný balíček Microsoft sestavy Viewer 2012 Runtime hello podle následující hello výše zadaný pokyny.
2. Vytvoření <language> složky v projektu a zkopírujte hello hello přidružené soubory prostředků sestavení. jsou technologie Hello toobe soubory sestavení prostředků zkopírovat: **Microsoft.ReportViewer.Webforms.Resources.dll** a **Microsoft.ReportViewer.Common.Resources.dll**. Vyberte soubory sestavení hello prostředků a v podokně Vlastnosti hello nastavte **zkopírujte tooOutput Directory** příliš "**vždy Kopírovat**".
3. Nastavit hello jazykovou verzi & UICulture pro hello webového projektu. Další informace o tom, jak tooset hello jazykovou verzi a jazyková verze uživatelského rozhraní pro webovou stránku ASP.NET, najdete v části [postup: Sada hello jazykovou verzi a jazyková verze uživatelského rozhraní pro globalizaci webové stránky ASP.NET](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfigurace ověřování a autorizace
Hello prohlížeče sestav vyžaduje toouse tooauthenticate správné přihlašovací údaje se serverem sestav hello a hello pověření musí být autorizovaný hello sestavy serveru tooaccess hello sestavami, které chcete. Informace o ověřování, najdete v dokumentu white paper hello [sestavy služby Reporting Services ovládací prvek prohlížeče a servery sestav na základě virtuálního počítače Microsoft Azure](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-hello-aspnet-web-application-tooazure"></a>Publikování hello tooAzure ASP.NET – webové aplikace
Pokyny rozhraní ASP.NET Web application tooAzure publikování, najdete v části [postup: migrace a publikovat webovou aplikaci tooAzure ze sady Visual Studio](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) a [Začínáme s webovými aplikacemi a ASP.NET](../../../app-service-web/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Pokud hello příkaz Přidat projekt nasazení Azure nebo přidat projekt cloudové služby Azure se nezobrazí v místní nabídce hello v Průzkumníku řešení, může být nutné toochange hello cílové rozhraní pro hello projektu too.NET Framework 4.
> 
> Zadejte Hello dva příkazy v podstatě hello stejné funkce. Jedna nebo hello se zobrazí další příkaz v místní nabídce hello závislosti na instalované verzi hello Microsoft Azure SDK jste nainstalovali.
> 
> 

## <a name="resources"></a>Zdroje
[Sestavy Microsoft](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence v Azure Virtual Machines](../classic/ps-sql-bi.md)

[Pomocí prostředí PowerShell tooCreate Azure virtuálních počítačů s nativní režim serveru sestav](../classic/ps-sql-report.md)

