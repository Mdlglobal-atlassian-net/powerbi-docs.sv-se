---
title: Tips för att skapa mallappar i Power BI (förhandsversion)
description: Tips om hur du kan använda frågor, datamodeller, rapporter och instrumentpaneler för att skapa bra mallappar
author: maggiesMSFT
manager: kfile
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-service
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: maggies
ms.openlocfilehash: 282638c7c1c8a60ee93292602766d63fd0fe436e
ms.sourcegitcommit: 8207c9269363f0945d8d0332b81f1e78dc2414b0
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56250051"
---
# <a name="tips-for-authoring-template-apps-in-power-bi-preview"></a>Tips för att skapa mallappar i Power BI (förhandsversion)

När du [skapar en mallapp](service-template-apps-create.md) i Power BI skapar du en arbetsyta, testar den och börjar använda den i produktionen. Andra viktiga delar är att skapa rapporten och instrumentpanelen. Vi kan bryta ned skapandeprocessen i fyra huvudsakliga delar. Att arbeta med dessa delar hjälper dig att skapa bästa möjliga mallapp:

* Med **frågor** kan du [ansluta](desktop-connect-to-data.md) och [transformera](desktop-query-overview.md) data och definiera [parametrar](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/). 
* I **datamodellen** skapar du [relationer](desktop-create-and-manage-relationships.md), [mått](desktop-measures.md) och vanliga frågor och svar om förbättringar.  
* **[Rapportsidorna](desktop-report-view.md)** innehåller visuella objekt och filter som ger insikter i dina data.  
* **[Instrumentpaneler](consumer/end-user-dashboards.md)** och [paneler](service-dashboard-create.md) ger en översikt över de analyser som ingår.  

Du känner kanske till varje del som befintliga Power BI-funktioner. När du skapar en mallapp finns ytterligare saker att överväga för varje del. Se avsnitten nedan för mer information.

<a name="queries"></a>

## <a name="queries"></a>Frågor
I mallappar används frågor som skapats i Power BI Desktop för att ansluta till din datakälla och importera data. Dessa frågor krävs för att returnera ett konsekvent schema och stöds för schemalagd datauppdatering (DirectQuery stöds inte).

### <a name="connect-to-your-api"></a>Ansluta till din API
Först måste du ansluta till ditt API från Power BI Desktop så att du kan börja skapa dina frågor.

Du kan använda de datakopplingar som finns tillgängliga i Power BI Desktop till att ansluta till ditt API. Du kan använda Web Data Connector (Hämta data -> Webb) till att ansluta till din Rest API, eller OData-koppling (Hämta data -> OData-feed) för att ansluta till din OData-feed. Dessa kopplingar fungerar endast om ditt API stöder grundläggande autentisering.

> [!NOTE]
> Om API:et använder andra autentiseringstyper som OAuth 2.0 eller Web API-nyckel, måste du utveckla en egen datakoppling som låter Power BI Desktop ansluta och autentisera till ditt API. Mer information om hur du utvecklar din egen datakoppling för din mallapp, finns i [dokumentationen om datakopplingar](https://aka.ms/DataConnectors). 
>
>

### <a name="consider-the-source"></a>Fundera över källan
Frågorna definierar vilka data som ingår i datamodellen. Beroende på storleken på ditt system ska dessa frågor också innehålla filter för att dina kunder ska använda en lämplig storlek som passar ditt företagsscenario.

Power BI-mallappar kan köra flera frågor parallellt för många användare samtidigt.  Planera din begränsnings- och samtidighetsstrategi i förväg och fråga oss om hur du ska göra din mallapp feltolerant.

### <a name="schema-enforcement"></a>Schematillämpning
Kontrollera att dina frågor är mottagliga för ändringar i ditt system, ändringar i schemat vid uppdatering kan bryta modellen. Om källan returnerar null eller saknat schemaresultat för vissa frågor, bör du fundera på att returnera en tom tabell eller ett anpassat felmeddelande som ger användaren information.

### <a name="parameters"></a>Parametrar
Med [parametrar](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/) i Power BI Desktop kan dina användare ange databasvärden som anpassar de data som hämtats av användaren. Se på parametrarna uppifrån för att undvika omarbete när du har lagt tid på att skapa detaljerade frågor eller rapporter.

> [!NOTE]
> Mallappar stöder alla parametrar utom Any och Binary.
>

### <a name="additional-query-tips"></a>Ytterligare frågetips

* Kontrollera att alla kolumner att skrivits på rätt sätt.
* Kolumnerna ska ha informativa namn (se [Frågor och svar](#qa)).  
* Vid delad logik kan du använda funktioner eller frågor.  
* Sekretessnivåer stöds för närvarande inte i tjänsten. Om en dialogruta om sekretessnivåer visas kan du behöva skriva om frågan och använda relativa sökvägar.  

## <a name="data-models"></a>Datamodeller

Med en väldefinierad datamodell kan dina kunder enkelt och intuitivt interagera med mallappen. Skapa datamodellen i Power BI Desktop.

> [!NOTE]
> Du bör göra en stor del av den grundläggande modelleringen (skriva, kolumnnamn) i [frågorna](#queries).
>


### <a name="qa"></a>Frågor och svar
Modelleringen påverkar också hur bra Frågor och svar kan visa resultat för dina kunder. Lägg till synonymer i vanligt förekommande kolumner och se till att ge dina kolumner rätt namn i [frågorna](#queries).

### <a name="additional-data-model-tips"></a>Fler datamodelltips

Kontrollera att du har:
* Tillämpat formatering för alla värdekolumner. Använt typer i frågan.  
* Använt formatering för alla mått. 
* Angett standardsammanfattning. I synnerhet ”Summera inte” när det är tillämpligt (t.ex. för unika värden).  
* Angett datakategori, om tillämpligt.  
* Angett relationer, vid behov.  

## <a name="reports"></a>Rapporter
På rapportsidorna finns ytterligare analyser av de data som ingår i mallappen. Använd sidorna i rapporterna för att besvara de viktiga affärsfrågor som din mallapp försöker hantera. Skapa rapporten med Power BI Desktop.

> [!NOTE]
> Endast en rapport kan ingå i en mallapp, så utnyttja de olika sidorna för att anropa särskilda delar i ditt scenario.
>
>

### <a name="additional-report-tips"></a>Fler rapporttips

* Använd mer än ett visuellt objekt per sida för korsfiltering.  
* Justera de visuella objekten noga (ingen överlappning).  
* Sidan är inställd på läget ”4:3” eller ”16:9” för layouten.  
* Alla aggregeringar som visas är numeriska (genomsnitt, unika värden).  
* Utsnitten ger rationella resultat.  
* Logotypen finns på den översta rapporten som ett minimum.  
* Elementen är i klientens färgschema så långt det är möjligt.  

<a name="dashboard"></a>

## <a name="dashboards"></a>Instrumentpaneler
Instrumentpanelen är den främsta interaktionspunkten med mallappen för dina kunder. Den ska innehålla en översikt av innehållet som ingår, i synnerhet viktiga mått för ditt affärsscenario.

Skapa en instrumentpanel för din mallapp genom att helt enkelt ladda upp din PBIX med Hämta data > Filer eller publicera direkt från Power BI Desktop.

> [!NOTE]
> Mallappar kräver för närvarande en enda rapport och datauppsättning per mallapp. Fäst inte innehåll från flera rapporter/datauppsättningar på instrumentpanelen som används i mallappen.
>
>

### <a name="additional-dashboard-tips"></a>Ytterligare instrumentpanelstips

* Behåll samma tema när du fäster så att panelerna på instrumentpanelen är konsekventa.  
* Fäst en logotyp på temat så att kunderna ser var paketet kommer ifrån.  
* Föreslagen layout för de flesta skärmupplösningarna är fem till sex små paneler på bredden.  
* Alla paneler på instrumentpanelen bör ha lämpliga rubriker/underrubriker.  
* Fundera över grupperingen på instrumentpanelen för olika scenarier, antingen lodrätt eller vågrätt.  

## <a name="known-limitations"></a>Kända begränsningar

| Visning av aktuellt objekt | Kända begränsningar |
|---------|---------|
|Innehåll:  Datauppsättningar   | Det ska finnas exakt en datauppsättning. Endast de datauppsättningar som finns inbyggda i Power BI Desktop (.pbix-filer) är tillåtna. <br>Stöds ej: Datauppsättningar från andra mallar, datauppsättningar mellan arbetsytor, sidnumrerade rapporter (RDL-filer), Excel-arbetsböcker |
|Innehåll: Rapporter     | Upp till en rapport    |
| Innehåll: Instrumentpaneler | Upp till en instrumentpanel som inte är tom <br>Stöds ej: Realtidspaneler (d.v.s. inget stöd för PushDataset eller pubnub) |
| Innehåll: Dataflöden | Stöds ej: Dataflöden |
| Innehåll från filer | Endast PBIX-filer är tillåtna. <br>Stöds inte: RDL-filer (sidnumrerade rapporter), Excel-arbetsböcker   |
| Datakällor | Datakällor som har stöd för schemalagd datauppdatering i molnet är tillåtna. <br>Stöds ej: <br>DirectQuery <br>Live-anslutningar (inte Azure AS) <br>Lokala datakällor (personlig gateway och företagsgateway stöds inte) <br>Realtid (pushdataset stöds inte) <br>Sammansatta modeller |
| Datauppsättning: över arbetsytor | Inga datauppsättningar över arbetsytor är tillåtna  |
| Innehåll: Instrumentpaneler | Realtidspaneler är inte tillåtna (d.v.s. inget stöd för PushDataset eller pubnub) |
| Frågeparametrar | Stöds ej: Parametrar av typen ”Any” eller ”Binary” blockerar uppdateringsåtgärden för datauppsättningen |
| Anpassade visuella objekt | Bara som offentligt tillgängliga anpassade visuella objekt stöds. [Anpassade visuella objekt för organisationer](power-bi-custom-visuals-organization.md) stöds inte |

## <a name="next-steps"></a>Nästa steg

[Vad är Power BI-mallappar? (förhandsversion)](service-template-apps-overview.md)