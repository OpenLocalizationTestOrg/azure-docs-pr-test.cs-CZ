---
title: "aaaAdding Azure Active Directory pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Hello Visual Studio přidat připojení služby dialogovém okně Přidat služby Azure Active Directory"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: 26c8f68edf9ec5f7bf65cbab34e4f9b4085ed18d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Přidání Azure Active Directory pomocí připojené služby v sadě Visual Studio
Pomocí služby Azure Active Directory (Azure AD), může podporovat jednotné přihlašování (SSO) pro webové aplikace ASP.NET MVC nebo ověřování Active Directory v webového rozhraní API služby. S Azure Active Directory Authentication mohou uživatelé používat své účty z Azure Active Directory tooconnect tooyour webových aplikací. výhody Hello ověřování Azure Active Directory pomocí webového rozhraní API zahrnují rozšířené zabezpečení při vystavení rozhraní API z webové aplikace. S Azure AD není nutné toomanage samostatného ověření systému se správou svůj vlastní účet a uživatele.

## <a name="prerequisites"></a>Požadavky
- Účet Azure – Pokud nemáte účet Azure, můžete [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-tooazure-active-directory-using-hello-connected-services-dialog"></a>Připojit tooAzure hello připojení služby pomocí služby Active Directory dialogové okno
1. V sadě Visual Studio vytvoření nebo otevření projektu aplikace ASP.NET MVC nebo projekt webového rozhraní API ASP.NET.

1. Z hello Průzkumníku řešení klikněte pravým tlačítkem na hello **připojené služby** uzel a v místní nabídce hello, vyberte **přidat připojení služby**.

1. Na hello **připojené služby** vyberte **ověřování s Azure Active Directory**.
   
    ![Připojená stránka služby](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Na hello **ÚVOD** stránku hello **konfigurovat Azure AD Authentication** průvodci vyberte **Další**.
   
    ![Úvodní stránka](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. Na hello **jednotné přihlašování na** stránku hello **konfigurovat Azure AD Authentication** průvodce, vyberte doménu z hello **domény** rozevíracího seznamu. Hello seznam domén obsahuje všechny domény, které jsou přístupné hello účty uvedené v dialogovém okně Nastavení účtu hello. Jako alternativu, můžete zadat název domény, Pokud nenajdete hello jeden hledáte, jako například `mydomain.onmicrosoft.com`. Můžete vybrat možnost toocreate hello aplikaci Azure Active Directory nebo pomocí nastavení hello z existující aplikaci Azure Active Directory. Vyberte **Další** po dokončení.
   
    ![Jednotné přihlašování na stránce](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. Na hello **přístup k adresářové** stránku hello **konfigurovat Azure AD Authentication** průvodce, ujistěte se, že hello **čtení dat adresáře** zaškrtnutá možnost. 
   
    ![Stránka adresáře](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Vyberte **Dokončit** tooadd hello tooenable nezbytné konfigurace kódu a odkazy na projekt pro ověřování Azure AD. Uvidíte hello domény služby Active Directory na hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Visual Studio se zobrazí [co se stalo](#how-your-project-is-modified) tooshow článku jste jak byla změněna projektu. Pokud chcete toocheck, který všechno fungoval, otevřete ho hello upravit konfigurační soubory a ověřte, zda text hello nastavení uvedené v článku hello existuje. 

## <a name="how-your-project-is-modified"></a>Jak se mění projektu
Když spustíte Průvodce hello, Visual Studio přidá Azure Active Directory a přidružené odkazy tooyour projektu. Konfigurační soubory a soubory kódu v projektu jsou také upravené tooadd podpory pro Azure AD. Hello konkrétní změny, které sada Visual Studio provádí závisí na typu projektu hello. Podrobné informace o tom, jak jsou upraveny projekty ASP.NET MVC najdete v tématu [jaké projekty MVC happened –](http://go.microsoft.com/fwlink/p/?LinkID=513809). Projekty webového rozhraní API, najdete v části [co se stalo – projekty webového rozhraní API](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Další kroky
* [Fórum MSDN pro Azure Active Directory](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Dokumentaci ke službě Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Příspěvek blogu: Úvod tooAzure služby Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

