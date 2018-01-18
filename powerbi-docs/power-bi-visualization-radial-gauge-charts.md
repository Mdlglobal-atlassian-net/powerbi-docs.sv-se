---
title: "Diagram med radiella mätare i Power BI (Självstudie)"
description: "Självstudie: Diagram med radiella mätare i Power BI"
services: powerbi
documentationcenter: 
author: mihart
manager: kfile
backup: 
editor: 
tags: 
featuredvideoid: xmja6Epqa
qualityfocus: no
qualitydate: 
ms.service: powerbi
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 10/27/2017
ms.author: mihart
ms.openlocfilehash: 7299b95cb3dd1fab4edce1764c69e1b2657ef547
ms.sourcegitcommit: 284b09d579d601e754a05fba2a4025723724f8eb
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/15/2017
---
# <a name="radial-gauge-charts-in-power-bi-tutorial"></a>Diagram med radiella mätare i Power BI (Självstudie)
Ett diagram med radiell mätare har en cirkelformad båge och visar ett värde som mäter framsteg på ett mål/KPI.  Målet, eller målvärdet, representeras av strecket (nålen). Framsteg mot målet representeras av skuggningen.  Och det värde som representerar förloppet visas i fetstil i bågen. Alla möjliga värden är jämnt fördelade längs bågen, från det lägsta (värdet längst till vänster) värdet till det högsta (värdet längst till höger).

I exemplet nedan är vi en bilåterförsäljare som spårar våra säljares genomsnittliga försäljning per månad. Vårt mål är 140, vilket representeras av den svarta nålen.  Minsta möjliga genomsnittlig försäljning är 0 och högsta har konfigurerats som 200.  Den blå skuggningen visar att det aktuella medelvärdet är cirka 120 försäljningar denna månad. Som tur är ha vi fortfarande ytterligare en vecka till vårt mål.

![](media/power-bi-visualization-radial-gauge-charts/gauge_m.png)

## <a name="when-to-use-a-radial-gauge"></a>När är det bra att använda en radiell mätare
Radiella mätare är ett bra val för att:

* visa förlopp mot ett mål.
* representera ett procentmått, t.ex. ett KPI.
* visa hälsotillståndet för ett enda mått.
* visa information som går snabbt att genomsöka och förstå.

## <a name="create-a-basic-radial-gauge"></a>Skapa en grundläggande radiell mätare
Dessa anvisningar använder det finansiella exemplet. För att följa instruktionerna, [hämta exempelfilerna](http://go.microsoft.com/fwlink/?LinkID=521962) till datorn, logga in på Power BI och välj **hämta Data \> Filer \> Lokal fil > Öppna**. 

Du kan också titta på när Will visar hur du skapar ett enskilt visuellt måttobjekt: måttdiagram, kort och KPI:er.

<iframe width="560" height="315" src="https://www.youtube.com/embed/xmja6EpqaO0?list=PL1N57mwBHtN0JFoKSR0n-tBkUJHeMP2cP" frameborder="0" allowfullscreen></iframe>

### <a name="step-1-open-the-financial-sample-excel-file"></a>Steg 1: Öppna Excel-filen med finansexemplet.
1. [Hämta Excel-filen med finansexemplet](sample-financial-download.md).
2. Öppna filen i Power BI genom att välja **Hämta data \> Filer** och bläddra till den plats där du sparade filen. Välj **Importera**. Finansexemplet läggs till på din arbetsyta som en datauppsättning.
3. Välj **finansexempel** att öppna den i utforskningsläge.

### <a name="step-2-create-a-gauge-to-track-gross-sales"></a>Steg 2: Skapa en mätare för att spåra bruttomarginalförsäljning
1. Välj **bruttomarginalförsäljning** i rutan **Fält**.
   
   ![](media/power-bi-visualization-radial-gauge-charts/grosssalesvalue_new.png)
2. Ändra aggregering till **Genomsnitt**.
   
   ![](media/power-bi-visualization-radial-gauge-charts/changetoaverage_new.png)
3. Välj mätarikonen ![](media/power-bi-visualization-radial-gauge-charts/gaugeicon_new.png) för att konvertera stapeldiagrammet till en mätare.
   
   Som standard skapar Power BI ett mätardiagram där det aktuella värdet (i det här fallet genomsnittlig bruttomarginalförsäljning) antas vara mätarens mittpunkt. Eftersom den genomsnittliga bruttomarginalförsäljningen är 182 760 $ är startvärdet (minst) 0 och slutvärdet (max) dubbla det aktuella värdet.
   
   ![](media/power-bi-visualization-radial-gauge-charts/gauge_no_target.png)

### <a name="step-3-set-a-target-value"></a>Steg 3: Ange ett målvärde
1. Dra **kostnad för sålda varor** till den **Målvärde**.
2. Ändra aggregering till **Genomsnitt**.
   Power BI lägger till en nål som representerar vårt målvärde på **145 480 $**. Observera att vi har överskridit våra mål.
   
   ![](media/power-bi-visualization-radial-gauge-charts/gaugeinprogress_new.png)
   
   > [!NOTE]
   > Du kan också ange ett målvärde manuellt.  Se ”Använd formateringsalternativ för att ange värden manuellt för lägsta, högsta och mål” nedan.
   > 
   > 

### <a name="step-4-set-a-maximum-value"></a>Steg 4: Ange ett maxvärde
I steg 2 använde Power BI fältet Värde för att automatiskt ange minimi- och maxvärde.  Men vad händer om du vill ange ett eget maxvärde?  Anta att du i stället för att använda dubbla det aktuella värdet som största möjliga värde vill ange högsta bruttoförsäljningsmarginal i datauppsättningen. 

1. Dra **bruttoförsäljningsmarginal** från listan **Fält** till brunnen **Maxvärde**.
2. Ändra aggregering till **Maxvärde**.
   
   ![](media/power-bi-visualization-radial-gauge-charts/setmaximum_new.png)
   
   Mätaren ritas om med ett nytt slutvärde på 1,21 miljoner i bruttoförsäljning.
   
   ![](media/power-bi-visualization-radial-gauge-charts/power-bi-final-gauge.png)

### <a name="step-5-save-your-report"></a>Steg 5: Spara din rapport
1. [Spara rapporten](service-report-save.md).
2. [Lägga till mätardiagrammet som en panel på instrumentpanelen](service-dashboard-tiles.md). 

## <a name="use-formatting-options-to-manually-set-minimum-maximum-and-target-values"></a>Använd formateringsalternativ för att ange värden manuellt för lägsta, högsta och mål
1. Ta bort **Maxvärde för bruttoförsäljning** från brunnen **Maxvärde**.
2. Välj rollerikonen för att öppna formateringsfönstret.
   
   ![](media/power-bi-visualization-radial-gauge-charts/power-bi-roller.png)
3. Expandera **Mätaraxeln** och ange värden för **Min** och **Max**.
   
    ![](media/power-bi-visualization-radial-gauge-charts/power-bi-gauge-axis.png)
4. Ta bort det aktuella värdet för målet genom att ta bort markeringen bredvid **kostnad för sålda varor**.
   
    ![](media/power-bi-visualization-radial-gauge-charts/pbi_remove_target.png)
5. När fältet **Mål** visas under **Mätaraxeln** och ange ett värde.
   
    ![](media/power-bi-visualization-radial-gauge-charts/power-bi-gauge-target.png)
6. Du kan också fortsätta formatera ditt mätardiagram.

## <a name="next-steps"></a>Nästa steg
[Visualiseringstyper i Power BI](power-bi-visualization-types-for-reports-and-q-and-a.md)

[Lägga till en visualisering till en rapport](power-bi-report-add-visualizations-i.md)

[Fästa en visualisering på en instrumentpanel](service-dashboard-pin-tile-from-report.md)

[Power BI – grundläggande begrepp](service-basic-concepts.md)

Har du fler frågor? [Prova Power BI Community](http://community.powerbi.com/)
