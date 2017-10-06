---
title: "aaaConnect tooSQL databázi pomocí aplikace SQL Server Management Studio v Azure Remoteappu | Microsoft Docs"
description: "Jak používat tento kurz toolearn toouse SQL Server Management Studio v Azure Remoteappu pro zabezpečení a výkonu při připojování tooSQL databáze"
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
ms.openlocfilehash: 73994f9a1eb3e48efa5d7c4f976b00cfcbc88d75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-tooconnect-toosql-database"></a>Pomocí SQL Server Management Studio v Azure Remoteappu tooconnect tooSQL databáze

> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
>

## <a name="introduction"></a>Úvod
Tento kurz ukazuje, jak toouse SQL Server Management Studio (SSMS) v Azure Remoteappu tooconnect tooSQL databáze. Vás provede procesem hello nastavení SQL Server Management Studio v Azure Remoteappu, vysvětluje výhody hello a zobrazuje funkce zabezpečení, které můžete použít v Azure Active Directory.

**Odhadovaný čas toocomplete:** 45 minut

## <a name="ssms-in-azure-remoteapp"></a>Aplikace SSMS v Azure Remoteappu
Azure RemoteApp je služba Vzdálená plocha v Azure, který zajišťuje aplikace. Další informace o něm zde: [co je RemoteApp?](../remoteapp/remoteapp-whatis.md)

Aplikace SSMS spuštění v Azure RemoteApp umožňuje hello stejné prostředí jako aplikace SSMS spuštěná místně.

![Snímek obrazovky aplikace SSMS spuštění v Azure Remoteappu][1]

## <a name="benefits"></a>Výhody
Existuje mnoho výhod toousing SSMS v Azure Remoteappu, včetně:

* Na serveru Azure SQL port 1433 nemá toobe zveřejněné externě (mimo Azure).
* Bez nutnosti tookeep přidávání a odebírání adres IP v bráně firewall serveru Azure SQL hello.
* Všechna připojení Azure RemoteApp probíhá přes protokol HTTPS na portu 443 pomocí šifrované protokolu vzdálené plochy
* Je více uživatelů a možné škálovat.
* Je výkonnější z s SSMS ve hello stejné oblasti jako hello databáze SQL.
* Můžete auditovat použití služby Azure RemoteApp s hello Premium edition služby Azure Active Directory, která má protokolování aktivit uživatelů.
* Můžete povolit vícefaktorové ověřování (MFA).
* Přístup aplikace SSMS kdekoli při použití některého z hello Podporovaní klienti Azure RemoteApp, což zahrnuje iOS, Android, Mac, Windows Phone a počítačích s Windows.

## <a name="create-hello-azure-remoteapp-collection"></a>Vytvořte kolekci Azure Remoteappu hello
Zde jsou kroky toocreate hello kolekcí vzdálené aplikace Azure RemoteApp pomocí SSMS:

### <a name="1-create-a-new-windows-vm-from-image"></a>1. Vytvoření nového virtuálního počítače Windows z bitové kopie
Použijte hello "Windows Server vzdálené plochy relace hostitele Windows serveru 2012 R2" Image z Galerie toomake hello nový virtuální počítač.

### <a name="2-install-ssms-from-sql-express"></a>2. Nainstalujte aplikaci SSMS z SQL Express.
Přejděte do hello nový virtuální počítač a přejděte stránky pro stažení toothis: [Express Microsoft® SQL Server® 2014](https://www.microsoft.com/download/details.aspx?id=42299)

Není stažení tooonly možnost SSMS. Po stažení přejděte do instalačního adresáře hello a spusťte instalační program tooinstall SSMS.

Budete také potřebovat tooinstall SQL Server 2014 Service Pack 1. Si můžete stáhnout tady: [aktualizace Service Pack 1 (SP1) pro systém Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 zahrnuje základní funkce pro práci s Azure SQL Database.

### <a name="3-run-validate-script-and-sysprep"></a>3. Spuštění skriptu ověřením a nástroj Sysprep
Na hello ploše hello virtuálního počítače je skript prostředí PowerShell s názvem ověřením. Spusťte tento dvojitým kliknutím. Ověří, že tento hello virtuálního počítače je připraven toobe používá pro vzdálené hostování aplikací. Po dokončení ověření požádá toorun sysprep – zvolte toorun ho.

Po dokončení nástroj sysprep vypne hello virtuálních počítačů.

toolearn Další informace o vytvoření image Azure Remoteappu, najdete v části: [jak toocreate šablony vzdálené aplikace RemoteApp bitové kopie v Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)

### <a name="4-capture-image"></a>4. Zachycení bitové kopie
Hello virtuálních počítačů byla zastavena, když ji najít v aktuálním portálu hello a zachycení ho.

toolearn Další informace o zachycení bitové kopie, najdete v části [zaznamenat bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic hello](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="5-add-tooazure-remoteapp-template-images"></a>5. Přidat tooAzure Image šablony vzdálené aplikace RemoteApp
V hello části hello aktuální portál Azure RemoteApp přejděte na kartu Image šablony toohello a klikněte na tlačítko Přidat. V poli hello automaticky otevírané okno Vyberte "Importovat bitovou kopii z knihovny virtuální počítače" a potom zvolte hello bitovou kopii, kterou jste právě vytvořili.

### <a name="6-create-cloud-collection"></a>6. Vytvoření cloudové kolekce
Hello aktuálním portálu vytvořte nové cloudové kolekce Azure Remoteappu. Zvolte hello Image šablony, který jste právě naimportovali pomocí SSMS na něm nainstalován.

![Vytvořit nové cloudové kolekce][2]

### <a name="7-publish-ssms"></a>7. Publikování aplikace SSMS
Na hello publikování kartě nové cloudové kolekce, vyberte možnost publikovat aplikace z hello nabídce Start a poté zvolte ze seznamu hello SSMS.

![Publikování aplikace][5]

### <a name="8-add-users"></a>8. Přidání uživatelů
Na kartě hello přístupu uživatele můžete vybrat hello uživatele, kteří budou mít kolekci Azure Remoteappu toothis přístup pouze zahrnující SSMS.

![Přidání uživatele][6]

### <a name="9-install-hello-azure-remoteapp-client-application"></a>9. Nainstalovat klientské aplikace Azure RemoteApp hello
Můžete stáhnout a nainstalovat klienta Azure RemoteApp zde: [stáhnout | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)

## <a name="configure-azure-sql-server"></a>Konfigurace serveru Azure SQL
Hello pouze potřeba konfigurace je tooensure, která obsluhuje Azure je povoleno pro bránu firewall hello. Pokud použijete toto řešení, pak nepotřebujete tooadd všechny IP adresy tooopen hello firewall. Hello síťový provoz, který je povolen toohello systému SQL Server je z jiných služeb systému Azure.

![Povolit Azure][4]

## <a name="multi-factor-authentication-mfa"></a>Vícefaktorové ověřování (MFA)
Vícefaktorové ověřování můžete konkrétně povolit pro tuto aplikaci. Přejděte toohello karta aplikace služby Azure Active Directory. Bude najít položku aplikace Microsoft Azure RemoteApp. Pokud kliknutím na tuto aplikaci a pak nakonfigurujte, zobrazí se stránka hello níže, kde můžete povolit MFA pro tuto aplikaci.

![Povolit MFA][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Audit aktivity uživatelů s Azure Active Directory Premium
Pokud nemáte Azure AD Premium, pak máte tooturn ho na hello licence oddílu adresáře. Premium povolená můžete přiřadit uživatele toohello úrovně Premium.

Jakmile se uživatel tooa ve vašem Azure Active Directory, můžete přejít toohello aktivity karta toosee přihlašovací informace tooAzure vzdálené aplikace RemoteApp.

## <a name="next-steps"></a>Další kroky
Po dokončení všech hello výše kroky se vám bude klientovi Azure Remoteappu možné toorun hello a přihlásit se s přiřazený uživatel. Zobrazí pomocí SSMS jako jednu z vašich aplikací a můžete ji spustit jako při byly nainstalovány v počítači se serverem SQL tooAzure přístup.

Další informace o tom, jak toomake hello tooSQL připojení databáze najdete v tématu [připojení tooSQL databáze s SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).

To je všechno, co teď. Užijte si ji!

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png