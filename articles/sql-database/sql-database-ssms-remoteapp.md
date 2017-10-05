---
title: "Připojení k SQL Database pomocí SQL Server Management Studio v Azure Remoteappu | Microsoft Docs"
description: "Použití v tomto kurzu se dozvíte, jak používat SQL Server Management Studio v Azure Remoteappu pro zabezpečení a výkonu při připojení k databázi SQL"
services: sql-database
documentationcenter: 
author: adhurwit
manager: jhubbard
ms.assetid: 1052c83c-e7f5-4736-922f-216194d8874b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: adhurwit
ms.openlocfilehash: ae1f2fa38d38fe6c10bc7960fddb07ae330d1eeb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Pomocí SQL Server Management Studio v Azure Remoteappu pro připojení k databázi SQL

> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
>

## <a name="introduction"></a>Úvod
V tomto kurzu se dozvíte, jak používat SQL Server Management Studio (SSMS) v Azure Remoteappu pro připojení k databázi SQL. Vás provede procesem nastavení služby SQL Server Management Studio v Azure Remoteappu, vysvětluje výhody a zobrazuje funkce zabezpečení, které můžete použít v Azure Active Directory.

**Odhadovaný čas dokončení:** 45 minut

## <a name="ssms-in-azure-remoteapp"></a>Aplikace SSMS v Azure Remoteappu
Azure RemoteApp je služba Vzdálená plocha v Azure, který zajišťuje aplikace. Další informace o něm zde: [co je RemoteApp?](../remoteapp/remoteapp-whatis.md)

Aplikace SSMS spuštění v Azure Remoteappu vám dává stejné prostředí jako aplikace SSMS spuštěná místně.

![Snímek obrazovky aplikace SSMS spuštění v Azure Remoteappu][1]

## <a name="benefits"></a>Výhody
Existuje mnoho výhod pomocí aplikace SSMS v Azure Remoteappu, včetně:

* Na serveru Azure SQL port 1433 nemá mají být exponovány externě (mimo Azure).
* Není potřeba zachovat přidávání a odebírání adres IP v bráně firewall serveru Azure SQL.
* Všechna připojení Azure RemoteApp probíhá přes protokol HTTPS na portu 443 pomocí šifrované protokolu vzdálené plochy
* Je více uživatelů a možné škálovat.
* Je výkonnější z s SSMS ve stejné oblasti jako databázi SQL.
* Můžete auditovat použití služby Azure RemoteApp s na edici Premium služby Azure Active Directory, který má protokolování aktivit uživatelů.
* Můžete povolit vícefaktorové ověřování (MFA).
* Přístup aplikace SSMS kdekoli při použití některého z podporovaných klientů Azure RemoteApp, včetně iOS, Android, Mac, Windows Phone a počítačích s Windows.

## <a name="create-the-azure-remoteapp-collection"></a>Vytvořte kolekci Azure RemoteApp
Tady jsou kroky k vytvoření kolekce vzdálené aplikace Azure RemoteApp pomocí SSMS:

### <a name="1-create-a-new-windows-vm-from-image"></a>1. Vytvoření nového virtuálního počítače Windows z bitové kopie
Chcete-li nový virtuální počítač použijte Image "Windows Server vzdálené plochy relace hostitele Windows serveru 2012 R2" z galerie.

### <a name="2-install-ssms-from-sql-express"></a>2. Nainstalujte aplikaci SSMS z SQL Express.
Přejděte na nový virtuální počítač a přejděte na tuto stránku stahování: [Express Microsoft® SQL Server® 2014](https://www.microsoft.com/download/details.aspx?id=42299)

Je k dispozici možnost pro stahování pouze SSMS. Po stažení přejděte do instalačního adresáře a spusťte instalaci aplikace SSMS.

Také musíte nainstalovat aktualizaci Service Pack 1 pro systém SQL Server 2014. Si můžete stáhnout tady: [aktualizace Service Pack 1 (SP1) pro systém Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 zahrnuje základní funkce pro práci s Azure SQL Database.

### <a name="3-run-validate-script-and-sysprep"></a>3. Spuštění skriptu ověřením a nástroj Sysprep
Na ploše virtuálního počítače je skript prostředí PowerShell volána ověřením. Spusťte tento dvojitým kliknutím. Ověří, že je virtuální počítač připravený k použití pro vzdálené hostování aplikací. Po dokončení ověření se zobrazí dotaz spustit nástroj sysprep-zvolte ji spustit.

Po dokončení nástroj sysprep vypne virtuální počítač.

Další informace o vytváření obrazem Azure RemoteApp najdete v tématu: [postup vytvoření image šablony vzdálené aplikace RemoteApp v Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)

### <a name="4-capture-image"></a>4. Zachycení bitové kopie
Když virtuální počítač se zastavila, najít na aktuálním portálu a zachycení ho.

Další informace o zachycení bitové kopie najdete v tématu [zaznamenat bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Přidejte do Image šablony vzdálené aplikace RemoteApp Azure
V části Azure RemoteApp na aktuálním portálu přejděte na kartu Image šablony a klikněte na tlačítko Přidat. V dialogovém okně automaticky otevírané okno Vyberte "Importovat bitovou kopii z knihovny virtuální počítače" a pak vyberte bitovou kopii, kterou jste právě vytvořili.

### <a name="6-create-cloud-collection"></a>6. Vytvoření cloudové kolekce
Na aktuálním portálu vytvořte nové cloudové kolekce Azure Remoteappu. Zvolte Image šablony, který jste právě naimportovali pomocí SSMS na něm nainstalován.

![Vytvořit nové cloudové kolekce][2]

### <a name="7-publish-ssms"></a>7. Publikování aplikace SSMS
Na publikování nové kolekce cloudu, vyberte na kartě publikovat aplikaci z nabídky Start a poté zvolte SSMS ze seznamu.

![Publikování aplikace][5]

### <a name="8-add-users"></a>8. Přidání uživatelů
Na kartě přístup uživatelů můžete vybrat uživatele, kteří budou mít přístup k této kolekci Azure Remoteappu, obsahující pouze SSMS.

![Přidání uživatele][6]

### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Instalace klienta aplikace Azure RemoteApp
Můžete stáhnout a nainstalovat klienta Azure RemoteApp zde: [stáhnout | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)

## <a name="configure-azure-sql-server"></a>Konfigurace serveru Azure SQL
Pouze konfigurace potřebná je zajistit, že pro bránu firewall je povolena služba Azure. Pokud použijete toto řešení, není potřeba přidat všechny IP adresy, otevření brány firewall. Síťový provoz, který je povolen k systému SQL Server je z jiných služeb systému Azure.

![Povolit Azure][4]

## <a name="multi-factor-authentication-mfa"></a>Vícefaktorové ověřování (MFA)
Vícefaktorové ověřování můžete konkrétně povolit pro tuto aplikaci. Přejděte na kartu aplikace služby Azure Active Directory. Bude najít položku aplikace Microsoft Azure RemoteApp. Pokud kliknutím na tuto aplikaci a pak nakonfigurujte, zobrazí se stránka níže, kde můžete povolit MFA pro tuto aplikaci.

![Povolit MFA][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Audit aktivity uživatelů s Azure Active Directory Premium
Pokud nemáte Azure AD Premium, budete muset zapnout v části licence adresáře. S Premium povolená můžete přiřadit uživatele na úrovni Premium.

Když přejdete na uživatele ve vašem Azure Active Directory, můžete pak přejděte na kartu aktivity chcete zobrazit informace o přihlášení k Azure RemoteApp.

## <a name="next-steps"></a>Další kroky
Po dokončení všech výše uvedených kroků, budete moci spustit klientovi Azure Remoteappu a přihlásit se s přiřazený uživatel. Zobrazí pomocí SSMS jako jednu z vašich aplikací a můžete ji spustit jako při byly nainstalovány v počítači s přístupem k serveru Azure SQL.

Další informace o tom, aby se připojení k databázi SQL najdete v tématu [připojit k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).

To je všechno, co teď. Užijte si ji!

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png