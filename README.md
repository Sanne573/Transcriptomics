# Afwijkende genexpressie in de immuunregulerende MAPK-pathway bij personen met reumatoïde artritis
## Structuur
- `Data_RA_raw` - Hierin staat de ruwe data waarmee deze transcriptomics analyse is gedaan
- `Data_beheer` - Hierin staat beschreven hoe de data is beheerd
- `FLowschema` - Hierin staat het flowschema wat gebruikt is in de methode
-  `Resultaten` - Hierin staan de figuren die zijn gebruikt in de resultaten
- `Bronnen` - Hierin staan de bronnen die gebruikt zijn over het gehele verslag
- `README.md` - Het document met het verslag er in
- `script` - Hierin staat het script hoe de transcriptomics analyse is uitgevoerd


## Introductie
Reumatoïde artritis (RA) is een chronische auto-immuunziekte. Het afweersysteem ziet de gewrichten als lichaamsvreemd en valt ze aan. Hierdoor ontstaan ontstekingen in en rond de gewrichten. Vaak ontstaan deze ontstekingen in de pezen, slijmbeurzen of spieren, maar kunnen ook voorkomen in organen of andere weefsels buiten het gewricht (Reumatoïde Artritis (RA) | ReumaNederland, z.d.). De oorzaak van deze auto-immuunziekte is nog onbekend en hier wordt veel onderzoek naar gedaan. Momenteel is er bekend dat het geen erfelijke ziekte is. Wel zijn omgevingsfactoren belangrijk bij het ontstaan van RA (UMC Utrecht, z.d.). Vooral roken is een belangrijk risicofactor (Venken & Elewaut, 2025). Verder is er bekend dat bij Reumatoïde artritis er een ontregeling is in immuungerelateerde genen en pathways (Zhang et al., 2019). Ondanks deze resultaten is er naar Reumatoïde artritis nog veel onderzoek nodig. Er wordt in die onderzoek, met behulp van transcriptomics, gekeken naar de expressie van genen in KEGG-pathways bij personen met Reumatoïde atrits. Het doel van dit onderzoek is meer inzicht te krijgen in het ziektemechanisme van de auto-immuunziekte, door de betrokken genen en pathways in kaart te brengen en te analyseren.

## Methode

<p align="center">
  <img src="Flowschema/Flowschema.png" alt="Flowschema" width="800"/>
</p>

*Figuur 1. Flowschema.*

### Data verkrijgen
In dit onderzoek is de data van 8 RNA sequencing syvonale biopten uit een artikel van Platzer et al. gebruikt. Voor het sequencen van het RNA is Illumina gebruikt. De reads zijn geanalyseerd in R (R 4.5.3) doormiddel van een transcriptomics-analyse en opgeslagen in een [script](Script). 
### Primaire verwerking
Eerst is BiocManager versie 1.30.27 (Morgan & Ramos, 2018) geïnstalleerd. Vervolgens zijn de reads gemapt tegen het [humane referentiegenoom versie hg38 (GRCh38)](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_000001405.40/ ) met Rsubread versie 2.24.0 . Hierbij zijn de reads toegewezen aan hun overeenkomstige genomische locaties, waarna de alignments zijn opgeslagen als BAM-bestanden. Vervolgens zijn, voor de kwantificatie van de genexpressie, de BAM-bestanden gecombineerd met de GTF-annotatie van het humane referentiegenoom hg38 (GRCh38). Met behulp van de functie featureCounts uit Rsubread versie 2.24.0 (Liao et al., 2019) zijn de gemapte reads op basis van deze genannotatie toegewezen aan de overeenkomstige genen. De hieruit verkregen read counts zijn samengevoegd tot een countmatrix. Vervolgens is met DESeq2 versie 1.50.2 (Love et al., 2014) een differentiële expressieanalyse uitgevoerd. De resultaten van deze analyse zijn gevisualiseerd met een volcano plot met behulp van EnhancedVolcano versie 1.28.2 (Blighe et al., 2025).
### Gene Ontrology analyse
Er is een Gene Ontology (GO) analyse gedaan op basis van een script op [MetwareBio](https://www.metwarebio.com/go-enrichment-analysis-clusterprofiler-guide/). Aanpassingen zijn gedaan met behulp van ChatGPT. Als eerst is er een GO-enrichment analyse gedaan met clusterProfiler versie 4.18.4 (Yu, 2024). Hierbij is de P-waarde gecorrigeerd met de Benjamini-Hochenberg procedure. Genen werden als statistische significant beschouwd bij een gecorrigeerde P-waarde van < 0.05 en een q-waarde, False Discovery Rate gecorrigeerde P-waarde, van < 0.02. De top 10 hiervan is weergegeven in een dotplot met enrichplot versie 1.30.5 (Yu, 2026).
### Data visualiseren
Op basis van het dotplot zijn de KEGG-pathways 04660 en 04662 geselecteerd. Deze zijn geanalyserd en visualiseerd door de KEGG-pathway analyse met pathview versie 1.50.0 (Luo & Brouwer, 2013). 

## Resultaten
In dit onderzoek zijn RNA-sequenties van personen met Reumatoïde artritis (RA) en gezonde controles geanalyseerd met behulp van een transcriptomics-analyse. Eerst is een differentiële expressie-analyse uitgevoerd, waarvan de resultaten zijn weergegeven in een volcanoplot (figuur 2). Vervolgens is een Gene Ontology (GO)-analyse uitgevoerd om te bepalen in welke biologische processen de differentieel geëxpresseerde genen betrokken zijn. De resultaten hiervan zijn weergegeven in een dotplot (figuur 3). Op basis van deze resultaten zijn twee KEGG-pathways geselecteerd voor verdere analyse. 
In figuur 2 is de differentiële genexpressie tussen de RA-groep en de controlegroep weergegeven. Op de x-as staat de log2 fold change (log2FC), waarbij een positieve waarde wijst op upregulatie in RA en een negatieve waarde op downregulatie ten opzichte van de controle. De y-as geeft de statistische significantie weer als −log10(p-waarde). De rood weergegeven genen voldoen aan de voorafgestelde criteria voor differentiële expressie, namelijk een log2FC groter dan 5 en een p-waarde <0,05. In totaal kwamen 5119 genen differentieel tot expressie, waarbij zowel up- als downregulatie werd waargenomen. 

<p align="center">
  <img src="Resultaten/VolcanoplotWC.png" alt="Volcanoplot" width="500"/>
</p>

*Figuur 2. Volcano plot van differentiële expressie-analyse van patiënten met Reumatoïde artiritis ten opzichte van gezonde personen. Op de x-as is de log2 fold change weergegeven en op de y-as de -log10 P. De stippellijnen zijn de gestelde grenzen; een log2 fold change van -2 tot 2 en een -log10 p van > 5. De rode punten vallen binnen deze grenswaarden. De groene punten vallen alleen binnen de grenswaarden van de log2 fold change. De grijze punten vallen binnen geen van beide grenswaarden.*

Om te bepalen in welke biologische processen de differentieel geëxpresseerde genen betrokken zijn, is een GO-analyse uitgevoerd. Figuur 3 toont de 10 meest significant verrijkte biologische processen. Deze processen bevatten meer differentieel geëxpresseerde genen dan op basis van toeval verwacht zou worden. Op basis hiervan zijn de "[T cell receptor signaling pathway](https://www.kegg.jp/pathway/hsa04660)" en "[B cell signaling pathway](https://www.kegg.jp/pathway/hsa04662)" geselecteerd voor verdere analyse, vanwege hun betrokkenheid bij immuunresponsen.

<p align="center">
  <img src="Resultaten/GO_dotplot1.png" alt="Dotplot" width="500"/>
</p>

*Figuur 3. Dotplot van de top 10 meest significante 
biologische processen uit de GO-analyse. Op de y-as staan de biologische processen en op de x-as het gen ratio, of wel het aandeel genen dat bij het proces betrokken is. De kleur overgang geeft de p.adjust waarde weer, p-waarde gecorrigeerd met Benjamini-Hochenberg procedure. De grote van de bolletjes geeft het aantal genen weer dat binnen het biologische proces vallen.*

Om de betrokken signaleringsmechanismen verder te onderzoeken, zijn de T- en B-celreceptor-signaleringspathways geanalyseerd (figuren 4 en 5). Beide pathways bevatten het MAPK-signaleringspad. Binnen dit pad zijn Ras, MEK1/2 en ERK upgereguleerd. Daarnaast is in de T-celreceptor-signaleringspathway een sterke upregulatie van MKK7 zichtbaar. Deze veranderingen wijzen op een verhoogde activiteit van signaleringsroutes die betrokken zijn bij de activatie en regulatie van immuuncellen.

<p align="center">
  <img src="Resultaten/hsa04660.pathview.png" alt="Pathway T-cel" width="500"/>
</p>

*Figuur 4. KEGG-pathwaykaart van de T cell receptor signaling pathway. De rood gekleurde genen zijn opgereguleerd en de groen gekleurde genen zijn neerwaarts gereguleerd ten opzichte van de controlegroep. De kleurintensiteit geeft de mate van verandering in genexpressie weer.* 

<p align="center">
  <img src="Resultaten/hsa04662.pathview.png" alt="Pathway B-cel" width="500"/>
</p>

*Figuur 5. KEGG-pathwaykaart van de B cell receptor signaling pathway. De rood gekleurde genen zijn opgereguleerd en de groen gekleurde genen zijn neerwaarts gereguleerd ten opzichte van de controlegroep. De kleurintensiteit geeft de mate van verandering in genexpressie weer.*


## Conclusies
Uit deze transcriptomics analyse zijn 2 KEGG-pathways gekozen die verder zijn uitgelicht; “T cell receptor signaling pathway” en “B cell signaling pathway”. Er is verder gekeken naar het MAPK-pathway die in beide KEGG-pathways voor komt. Dit pathway zorgt onder andere voor groei en differentiatie van de cellen, maar ook voor bijvoorbeeld het produceren van ontstekingscytokinen (Morrison, 2012). Hierin is te zien dat in beide pathways de genen Ras, MEK1/2 en Erk zijn upgereguleerd. Deze genen zorgen onder andere voor bevorderen van de groei, differentiatie en overleving van de cellen (Xie et al., 2025). Wat er dus voor zorgt dat T- en B-cellen beter groeien en overleven. 

In het T-cell signaling pathway is ook te zien dat het gen MKK7 sterk is upgereguleerd. Dit gen stuurt andere genen in het synoviaal weefsel aan om ontstekingscytokinen te produceren. Zo ontstaat er een ontsteking tussen de gewrichten (Lee et al., 2012). 

Een disregulatie in het MAPK pathway zou dus een oorzaak kunnen zijn van Reumatoïde atritis. 

In een volgend onderzoek zou gekeken kunnen worden naar andere immuun pathways. Of andere onderdelen in dit onderzoek uitgezochte pathways. Er zijn veel meer genen die up of down gereguleerd zijn en verder onderzoek kunnen gebruiken. 
