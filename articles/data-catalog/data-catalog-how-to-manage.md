---
title: "aaaManage datových prostředcích v Azure Data Catalog | Microsoft Docs"
description: "článek Hello označuje, jak zaregistrovat toocontrol viditelnost a vlastnictví datových prostředků v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 48a634b92d7da19c32c9e551f295eec257f54f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Spravovat datové prostředky v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog je určen pro zdroj dat zjišťování, takže lze snadno vyhledat a pochopit zdroje dat hello potřebujete tooperform analýzy a rozhodnutí. Tyto funkce zjišťování zajistěte největších dopad hello při vy a ostatní uživatelé můžete najít a pochopit hello nejširší řadu dostupných zdrojů dat. S těmito prvky pamatovat hello výchozí chování katalogu Data Catalog je pro všechny registrované datové zdroje toobe viditelné tooand zjistitelný všichni uživatelé katalogu.

Data Catalog nezískáte přístup toohello samotná data. Vlastníkem hello hello zdroje dat se řídí přístup k datům. Pomocí katalogu Data Catalog můžete zjistit zdroje dat a zobrazit hello metadata, která je související toohello zdroje, které jsou zaregistrovány v katalogu hello.

Můžou nastat situace, ale kde zdroje dat mají jenom viditelné toospecific uživatele nebo toomembers určitých skupin. V takových scénářů uživatelé převzít vlastnictví registrované datové prostředky v rámci katalogu hello a pak řídit viditelnost hello hello prostředků, které vlastní.

> [!NOTE]
> Hello funkcí popsaných v tomto článku je k dispozici pouze v hello Azure Data Catalog, Standard Edition. Edice Free Hello nenabízí možnosti pro vlastnictví a omezení viditelnosti datový prostředek.
>
>

## <a name="manage-ownership-of-data-assets"></a>Spravovat vlastnictví datových prostředků
Ve výchozím nastavení jsou bez přiřazeného vlastnictví datových prostředků, které jsou zaregistrovány v katalogu Data Catalog. Každý uživatel s oprávnění tooaccess hello katalogu můžete zjistit a opatřit poznámkami tyto prostředky. Uživatelé mohou převzít vlastnictví bez přiřazeného vlastnictví datových prostředků a pak omezit viditelnost hello hello prostředků, které vlastní.

Pokud vlastní datový prostředek v katalogu Data Catalog, pouze uživatelé, kteří jsou autorizovány hello vlastníky můžete zjistit hello asset a zobrazit jeho metadata, a pouze hello vlastníky můžete odstranit prostředek hello z katalogu hello.

> [!NOTE]
> Vlastnictví v katalogu Data Catalog ovlivňuje pouze hello metadata, která je uložena v katalogu hello. Vlastnictví neuděluje všechna oprávnění v podkladovém zdroji dat hello.
>
>

### <a name="take-ownership"></a>Přebírat vlastnictví
Uživatelé mohou převzít vlastnictví datových prostředků tak, že vyberete hello **převzít vlastnictví** možnost portálu hello katalogu Data Catalog. Žádná zvláštní oprávnění jsou požadované tootake vlastnictví prostředek bez přiřazeného vlastnictví data. Každý uživatel může převzít vlastnictví prostředek bez přiřazeného vlastnictví data.

### <a name="add-owners-and-co-owners"></a>Přidat vlastníků a spoluvlastníci
Pokud je již vlastněn datový prostředek, nelze ostatní uživatelé jednoduše převzít vlastnictví. Je nutné přidat jako spoluvlastníci existující vlastník. Všechny vlastníka můžete přidat další uživatele nebo skupiny zabezpečení jako spoluvlastníci.

> [!NOTE]
> Ho je nejlepší postup toohave aspoň dva jednotlivce jako vlastníci pro všechny vlastní datový prostředek.
>
>

### <a name="remove-owners"></a>Odebrat vlastníky
Stejně jako všechny vlastník majetku můžete přidat spoluvlastníky, můžete odebrat všechny vlastník majetku žádné spoluvlastník.

Vlastníka prostředku, který odebere mu nebo samu sebe jako vlastníka již nebude možné spravovat hello asset. Pokud vlastník majetku hello odebere mu nebo samu sebe jako vlastníka a neexistují žádné společné vlastníci, vrátí hello asset tooan bez přiřazeného vlastníka stavu.

## <a name="control-visibility"></a>Přehled ovládacího prvku
Datový prostředek vlastníky můžete řídit viditelnost hello hello datových prostředků, které vlastní. viditelnost toorestrict jako výchozí hello, kde všechny katalog Data Catalog smí uživatelé zjistit a zobrazení hello datový prostředek, vlastník majetku hello můžete přepínat hello viditelnost nastavení z **Everyone** příliš**vlastníci a tito uživatelé** v hello vlastnosti pro prostředek hello. Vlastníci poté můžete přidat konkrétní uživatele a skupiny zabezpečení.

> [!NOTE]
> Kdykoli je to možné, měli oprávnění vlastnictví a viditelnost asset přiřadit není tooindividual uživatelé a skupiny toosecurity.
>
>

## <a name="catalog-administrators"></a>Správci katalogu
Správci katalogu dat se implicitně společné vlastníci všechny prostředky v katalogu hello. Vlastníci Asset nelze odebrat viditelnost od správců, a můžou správci spravovat vlastnictví a viditelnost pro všechny datové prostředky v katalogu hello.

## <a name="summary"></a>Souhrn
Hello katalogu Data Catalog crowdsourcingový model toometadata zjišťování a dat asset umožňuje všechny toocontribute uživatelé katalogu a zjišťování. Hello Data Catalog, Standard Edition je určená pro vlastnictví a správu toolimit hello viditelnost a použít konkrétní datových prostředků.
