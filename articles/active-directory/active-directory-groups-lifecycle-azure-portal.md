---
title: "vypršení platnosti v Azure Active Directory skupiny aaaPreview Office 365 | Microsoft Docs"
description: "Jak tooset do vypršení platnosti pro Office 365 skupin v Azure Active Directory (preview)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: a8c99961cff3aad3f4d8b0cc1b2eb8e8a4c9ba95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a>Nakonfigurovat vypršení platnosti skupiny Office 365 (preview)

Teď můžete spravovat životní cyklus hello skupiny Office 365 nastavením vypršení platnosti pro všechny skupiny Office 365, které jste vybrali. Po této vypršení platnosti je nastaveno, vlastníků těchto skupin jsou vyzváni toorenew jejich skupiny, pokud je stále nutné hello skupiny. Odstraní se všechny skupiny Office 365, který není obnoven. Všechny skupiny Office 365, který byl odstraněn lze obnovit do 30 dní hello skupiny Vlastníci nebo hello správce.  


> [!NOTE]
> Můžete nastavit dobu platnosti pro pouze skupiny Office 365.
>
> Nastavení vypršení platnosti pro skupiny O365 vyžaduje, aby se k přiřazenou licenci Azure AD Premium
>   - Hello správce, který konfiguruje nastavení vypršení platnosti hello hello klienta
>   - Všichni členové skupiny hello vybrané pro toto nastavení

## <a name="set-office-365-groups-expiration"></a>Nastavit dobu platnosti skupiny Office 365

1. Otevřete hello [centra pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem v klientovi služby Azure AD.

2. Otevřete Azure AD, vyberte **uživatelů a skupin**.

3. Vyberte **nastavení skupiny** a pak vyberte **vypršení platnosti** nastavení vypršení platnosti tooopen hello.
  
  ![Okno vypršení platnosti](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Na hello **vypršení platnosti** okno, můžete:

  * Nastavení doby platnosti hello skupiny ve dnech. Můžete třeba vybrat jednu z hello přednastavení hodnoty, nebo vlastní hodnota (by měl být 31 dnů nebo déle). 
  * Zadejte e-mailovou adresu, kde mají být odeslána oznámení o vypršení platnosti a obnovení hello když vlastník skupiny. 
  * Vyberte skupiny Office 365, které vyprší. Můžete povolit vypršení platnosti pro **všechny** skupiny Office 365, můžete vybrat některé z těchto skupin hello Office 365, nebo můžete vybrat **žádné** zakázat vypršení platnosti pro všechny skupiny.
  * Uložit nastavení, když jste hotovi výběrem **Uložit**.

Pokyny jak toodownload a nainstalujte hello Microsoft PowerShell modulu tooconfigure vypršení platnosti pro skupiny Office 365 pomocí prostředí PowerShell, najdete v části [Azure modulu Active Directory V2 PowerShell - veřejné verze Preview 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

E-mailová oznámení, jako je tato odešlou toohello Office 365 skupiny Vlastníci 30 dní, 15 dní a 1 den předchozí tooexpiration hello skupiny.

![Vypršení platnosti e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Vlastník skupiny Hello pak můžete vybrat **obnovit skupinu** toorenew hello skupiny Office 365, nebo můžete vybrat **přejděte toogroup** toosee hello členy a další podrobnosti o hello skupiny.

Když vyprší platnost skupinu, odstranění skupiny hello jeden den po vypršení platnosti hello. E-mailové oznámení, jako je tato je odeslaných vlastníků skupiny toohello Office 365, informace o vypršení platnosti hello a následné mazání jejich skupiny Office 365.

![Skupiny odstranění e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Hello skupiny může být obnovena výběrem **obnovení skupiny** nebo pomocí rutin prostředí PowerShell, jak je popsáno v [obnovení odstraněné Office 365 skupiny ve službě Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-Restore-Azure-Portal).
    
Pokud hello skupinu, kterou jste obnovení obsahuje dokumenty, weby Sharepointu nebo jiné trvalé objekty, může to trvat až too24 hodin toofully obnovení hello skupiny a její obsah.

> [!NOTE]
> * Pokud nasazujete hello nastavení vypršení platnosti, může být některé skupiny, které jsou starší než období platnosti hello. Tyto skupiny nebudou odstraněny okamžitě, ale jsou nastavit too30 počet dnů do vypršení platnosti. Hello první obnovení e-mailové oznámení odeslání během dne. Například skupiny A byl vytvořen před 400 dny a interval vypršení platnosti hello nastaven too180 dnů. Při aplikování nastavení vypršení platnosti, skupiny A má 30 dní, než je odstraní, pokud hello vlastníka obnovuje se.
> * Když dynamická skupina je odstranit a obnovit, považuje se za nové skupiny a znovu vyplněná podle toohello pravidlo. Tento proces může trvat až too24 hodin.

## <a name="next-steps"></a>Další kroky
Tyto články poskytují další informace o skupinách Azure AD.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
