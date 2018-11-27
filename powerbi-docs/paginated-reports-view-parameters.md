---
title: Visa parametrar för sidnumrerade rapporter i Power BI-tjänsten | Microsoft Docs
description: I den här artikeln lär du dig att interagera med parametrar för sidnumrerade rapporter i Power BI-tjänsten.
author: maggiesMSFT
manager: kfile
ms.reviewer: ''
ms.service: powerbi
ms.component: report-builder
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: maggies
ms.openlocfilehash: d144030db1d35e103a476af8e96fa4b2b8b1dfaa
ms.sourcegitcommit: b23fdcc0ceff5acd2e4d52b15b310068236cf8c7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51268860"
---
# <a name="view-parameters-for-paginated-reports-in-the-power-bi-service"></a>Visa parametrar för sidnumrerade rapporter i Power BI-tjänsten

I den här artikeln lär du dig att interagera med parametrar för sidnumrerade rapporter i Power BI-tjänsten.  En rapportparameter ger ett sätt att filtrera rapportdata på. Parametrar erbjuder en lista över tillgängliga värden och du kan välja ett eller flera värden. Parametrar har ibland ett standardvärde, och ibland måste du välja ett värde innan du kan se rapporten.  

När du visar en rapport som har parametrar, visar rapportgranskarens verktygsfält varje parameter så att du kan ange värden interaktivt. Följande bild visar parameterområdet för en rapport med parametrar för **Köpa grupp**, **Plats**, ett **Datum från**, och ett **Datum till**.  

## <a name="parameters-pane-in-the-power-bi-service"></a>Parameterfönstret i Power BI-tjänsten

![Visa sidnumrerad rapport med parametrar](media/paginated-reports-view-parameters/power-bi-paginated-view-parameters.png)
  
1.  **Parameterfönstret** rapportgranskarens verktygsfält visar ett meddelande, till exempel ”krävs” eller ett standardvärde för varje parameter.    
  
2.  **Datumparametrar för fakturor från/till**  De två parametrarna har standardvärden. Ange ett datum i textrutan eller välj ett datum i kalendern för att ändra datum.  
  
3.  **Platsparametern** Platsparametern är inställd på att du kan välja en, flera eller alla värden. 
  
4.  **Visa rapporten** När du angett eller ändrat parametervärden, klickar du på **Visa rapport** för att köra rapporten. 

5. **Standardvärden** Om alla parametrar har standardvärden, körs rapporten automatiskt i första vyn. Vissa parametrar i den här rapporten hade inte standardvärden, så du ser inte rapporten förrän du väljer värden.  

## <a name="next-steps"></a>Nästa steg

[Skapa parametrar för sidnumrerade rapporter i Power BI-tjänsten](paginated-reports-parameters.md)