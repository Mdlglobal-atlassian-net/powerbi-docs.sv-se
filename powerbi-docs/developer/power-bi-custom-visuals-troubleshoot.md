---
title: Felsökning av hur du utvecklar visuella objekt i Power BI
description: Den här artikeln går igenom några vanliga problem som kan uppstå när du utvecklar eller skapar ett anpassat objekt i Power BI.
author: KesemSharabi
ms.author: kesharab
ms.reviewer: ''
ms.service: powerbi
ms.topic: troubleshooting
ms.subservice: powerbi-custom-visuals
ms.date: 11/06/2018
ms.openlocfilehash: e28df5035e057d485a8122853f6ae88327e3045f
ms.sourcegitcommit: 01de0b01f66f28ca45b8d309d7864f261d6c9a85
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74127756"
---
# <a name="troubleshoot-power-bi-power-bi-visuals"></a>Felsöka anpassade visuella Power BI-objekt

## <a name="debug"></a>Felsök

**Pbiviz-kommandot hittades inte (eller liknande fel)**

När du kör `pbiviz` på kommandoraden i terminalen bör du se hjälpskärmen. Om du inte gör det så har den inte installerats korrekt. Kontrollera att du har installerat version 4.0 av NodeJS eller senare.

**Det gick inte att hitta det visuella felsökningsobjektet på fliken Visualiseringar**

Det visuella felsökningsobjektet ser ut som en frågeikon på fliken **Visualiseringar**.

![Val av visuella objekt](media/power-bi-custom-visuals-troubleshoot/powerbi-developer-visual-selection.png)

Kontrollera att du har aktiverat det på inställningarna för Power BI om du inte kan se det.

> [!NOTE]
> Det går för närvarande endast att felsöka visuella objekt i Power BI-tjänsten och inte i Power BI Desktop eller mobilappen. Det paketerade visuella objektet kommer fortfarande att fungera överallt.

**Det går inte att kontakta den visuella servern**

Kör den visuella servern `pbiviz start` på kommandoraden i terminalen från roten av ditt visuella projekt. Om servern inte körs är det troligt att dina SSL-certifikat inte har installerats korrekt.

Ta gärna kontakt med supporten för visuella Power BI-objekt: *pbicvsupport@microsoft.com*  om du har frågor, kommentarer eller problem.

## <a name="next-steps"></a>Nästa steg

Mer information finns i [Vanliga frågor och svar om anpassade visuella Power BI-objekt](power-bi-custom-visuals-faq.md#organizational-visuals).