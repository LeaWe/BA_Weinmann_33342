# BA (kommentierter Code)
## Legende
*g* = Gesamtnetzwerk  
*g2* = bereinigtes Netzwerk, alle gelöscht mit degree < 3  
*g3* = Elite in der Elite, alle gelöscht mit degree < 15  
*preistraegerfinal* = Preisträgernetzwerk  
  nach Jahren: *preistraeger2015, preistraeger2016,...*  
*juryfinal* = Jurynetzwerk  
   nach Jahren: *juryfinal2015, juryfinal2016,...*  
*ind-/outd-* = Präfix für In-/Outdegree-Netzwerke  
*preistraeger_xx* = Preisträgernetzwerk eines Teilnetzwerks  
*jury_xx* = Jurynetzwerk eines Teilnetzwerks  
*gruppenpreise2015, gruppenpreise2016...* = Teilnetzwerke mit Gruppenpreisen der einzelnen Jahre  
*einzelpreise2015,...* = Teilnetzwerke mit Einzelpreisen der einzelnen Jahre 

### Bezeichnungen der Preise  
Alternativer Medienpreis:  
```
id: alternativer_medienpreis
name: "Alternativer Medienpreis"
```
Axel-Springer-Preis:  
```
id: axel_springer_preis 
name: "Axel-Springer-Preis für junge Journalisten"
```
Bremer Fernsehpreis:  
```
id: bremer_fernsehpreis
name: "Bremer Fernsehpreis"
```
Deutscher Journalistenpreis Wirtschaft:  
```
id: djp  
name: "Deutscher Journalistenpreis Wirtschaft | Börse | Finanzen (djp)"
```
Deutscher Radiopreis:  
```
id: deutscher_radiopreis
name: "Deutscher Radiopreis"
```
Deutscher Reporterpreis:  
```
id: deutscher_reporterpreis
name: "Deutscher Reporterpreis"
```
Ernst-Schneider-Preis:  
```
id: ernst_schneider_preis
name: "Ernst-Schneider-Preis"
```
Georg v. Holtzbrinck-Preis:  
```
id: georg_von_holtzbrinck_preis_fuer_wirtschaftspublizistik
name: "Georg von Holtzbrinck-Preis für Wirtschaftspublizistik bzw. Ferdinand Simoneit-Nachwuchspreis"
```
Grimme Online Award:  
```
id: grimme_online_award
name: "Grimme Online Award"
```
Helmut-Schmidt-Preis:  
```
id: helmut_schmidt_journalistenpreis
name: "Helmut-Schmidt-Journalistenpreis"
```
Herbert-Quandt-Preis:  
```
id: herbert_quandt_medienpreis
name: "Herbert Quandt Medienpreis"
```
Journalist des Jahres:  
```
id: journalist_des_jahres
name: "Journalist des Jahres"
```
Katholischer Medienpreis:  
```
id: katholischer_medienpreis
name: "Katholischer Medienpreis"
```
Kurt-Magnus-Preis:
```
id: kurtmagnuspreis
name: "Kurt-Magnus-Preis"
```
Kurt-Tucholsky-Preis:  
```
id: kurttucholskypreis
name: "Kurt-Tucholsky-Preis für literarische Publizistik"
```
Leuchtturm:  
```
id: leuchtturm
name: "Leuchtturm für besondere publizistische Leistungen"
```
Ludwig-Börne-Preis:  
```
id: ludwig_boerne_preis
name: "Ludwig-Börne-Preis"
```
Ludwig-Erhard-Preis:  
```
id: ludwig_erhard_preis
name: "Ludwig-Erhard-Preis für Wirtschaftspublizistik"
```
Nannenpreis:  
```
id: nannenpreis
name: "Nannen Preis"
```
Otto Brenner Preis:
```
id: otto_brenner_preis
name: "Otto Brenner Preis"
```
Preis für die Freiheit und die Zukunft der Medien:
```
id: preis_fuer_freiheit
name: "Preis für die Freiheit und die Zukunft der Medien"
```
Robert-Geisendörfer-Preis: 
```
id: robert_geisendoerfer_preis
name: "Robert Geisendörfer Preis"
```
Theodor-Wolff-Preis:  
```
id: theodor_wolff_preis
name: "Theodor-Wolff-Preis"
```
Wächterpreis:  
```
id: waechterpreis 
name: "Wächterpreis der Tagespresse"
```
Standard-Visualisierung zum Kopieren:
```
plot(xx, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

## Netzwerk laden
```
library(igraph)
el <- read.csv("el.csv", header = TRUE, as.is=T, sep = ";")
nl <- read.csv("nl.csv", header = TRUE, as.is=T, sep = ";")
is.data.frame(el)
is.data.frame(nl)
g <- graph.data.frame(el, directed=TRUE, vertices=nl)
g
plot(g)
head(el)
head(nl)

plot(g)
```

Zeigt Attribute des Netzwerks an
```
E(g)
V(g)
edge_attr(g)
vertex_attr(g)
```

## Beschreibung Gesamtnetzwerk

Erzeugt Teilnetzwerk mit ausschließlich Preisen (Frage: Wie viele Preise im Gesamtnetzwerk?)
```
preise <- induced_subgraph(g, V(g)[which (institutiontype == 1)])
preise
```
Erzeugt Teilnetzwerk mit ausschließlich Medien (Frage: Wie viele Medien im Gesamtnetzwerk?)
```
medien <- induced_subgraph(g, V(g)[which (institutiontype == 2)])
medien
```
Erzeugt Teilnetzwerk mit ausschließlich Personen (Frage: Wie viele Personen im Gesamtnetzwerk?)
```
personen <- induced_subgraph(g, V(g)[which (type == 0)])
personen
```
Erzeugt Teilnetzwerk mit ausschließlich Preisträgern (Frage: Wie viele Preisträger im Gesamtnetzwerk?)
```
preistraeger <- subgraph.edges(g, E(g)[which (relation == 1)])
preistraeger_personen <- induced.subgraph(preistraeger, V(preistraeger)[type == 0])
preistraeger_personen
```
Erzeugt Teilnetzwerk mit ausschließlich Juroren (Frage: Wie viele Juroren im Gesamtnetzwerk?)
```
juroren <- subgraph.edges(g, E(g)[which (relation == 3)])
juroren_personen <- induced.subgraph(juroren, V(juroren)[type == 0])
juroren_personen
```
Erzeugt Teilnetzwerk mit ausschließlich Arbeitsverhältnissen (Frage: Wie viele Arbeitsverhältnisse im Gesamtnetzwerk?)
```
av <- subgraph.edges(g, E(g)[which (relation == 2)])
av
```
Wie viele Juroren arbeiten nicht in den Medien?  
```
notinmedia <- V(juroren_personen)$workinmedia == 2
sum(notinmedia, na.rm = T)
```
Wie viele Jury-Beziehungen gibt es im Netzwerk und wie viele davon zu Personen, die nicht in den Medien arbeiten?  
```
juroren
juroren_notinmedia <- delete.vertices(juroren, V(juroren)[which(workinmedia == 1)])
juroren_notinmedia <- delete_vertices (juroren_notinmedia, V(juroren_notinmedia)[degree(juroren_notinmedia, mode="all")=="0"])
juroren_notinmedia
```
Erzeugt Teilnetzwerk mit ausschließlich Männern (Frage: Wie viele Männer im Gesamtnetzwerk?)
```
maenner <- induced_subgraph(personen, V(personen)[which (sex == 1)])
maenner
```
Erzeugt Teilnetzwerk mit ausschließlich Frauen (Frage: Wie viele Frauen im Gesamtnetzwerk?)
```
frauen <- induced_subgraph(personen, V(personen)[which (sex == 2)])
frauen
```
Erzeugt Teilnetzwerk mit ausschließlich Preisverleihungen (Frage: Wie viele Preisverleihungen an Einzelpersonen gab es?)
```
verleihung <- subgraph.edges(g, E(g)[relation == 1])
verleihung
```


## Netzwerkmaße

**Out-Degree im Gesamtnetzwerk**
```
outd <- degree(g, mode="out")
outd
sort (outd)
```

**In-Degree im Gesamtnetzwerk**
```
ind <- degree(g, mode="in")
ind
sort(ind)
```

**Dichte**
```
edge_density(g)
```

**Betweenness-Zentralität und Betweenness**
```
centr_betw(g, directed=FALSE)
betw <- betweenness(g, directed=FALSE)
```

**Degree-Zentralität**
```
centralization.degree(g, mode = "all")
centralization.degree(g, mode = "in")
centralization.degree(g, mode = "out")
```

**Closeness-Zentralität**
(Closeness centrality measures how many steps is required to access every other vertex from a given vertex)
```
clo <- closeness(g)
sort(clo)
```

**Zentralisierter Closeness-Wert (wie dicht die Knoten zusammenstehen)**
```
centr_clo(g, mode = "all")$centralization
```

**Pfaddistanz**
Durchschnittliche Pfaddistanz:
```
mean_distance(g, directed = FALSE) 
```
Längste Pfaddistanz im Netzwerk:
```
diameter(g, directed = FALSE)
```
Von wo nach wo führt die längste Pfaddistanz im Netzwerk?
```
farthest_vertices(g, directed = FALSE)
```

## PREISTRÄGER-NETZWERK

1. Entfernt alle Jury-relations (relation = 3)
```
preistraeger <- delete_edges (g, E(g)[relation == 3])
```
2. Namen extrahieren
```
Personen <- delete.vertices(preistraeger, V(preistraeger)[type!=0])
Namen <- V(Personen)$name
Namen
```
3. Schleife: Alle Namen durchschleifen: Hat eine Person keine relation = 1? --> Dann: Löschen.
```
for (i in (1:length(Namen))){
  ego <- delete_vertices(preistraeger, V(preistraeger)[(name != Namen[i]) & (type == 0)])
  preistraegerbeziehungen <- E(ego)[relation ==1]
  if (sum(preistraegerbeziehungen) == 0){
    preistraeger <- delete_vertices(preistraeger, V(preistraeger)[name == Namen[i]])
  }
}
```
4. Löscht alleinstehende Nodes und wirft das Preisträgernetzwerk aus
```
preistraeger <- delete_vertices (preistraeger, V(preistraeger)[degree(preistraeger, mode="all")=="0"]) 
preistraeger
```
Berechnet Out-Degree von Preisträgernetzwerk 
```
outdpreistraeger <- degree(preistraeger, mode="out")
outdpreistraeger
sort(outdpreistraeger)
```
Berechnet In-Degree von Preisträgernetzwerk 
```
indpreistraeger <- degree(preistraeger, mode="in")
indpreistraeger
sort(indpreistraeger)
```
Wer hat die meisten Preise abgeräumt? (Netzwerk mit nur Preisträgern, relation == 1, sort nach Outdegree)
```
preistraeger_ohneag <- delete.edges(preistraeger, E(preistraeger)[which(relation == 2)])
preistraeger_ohneag <- delete_vertices (preistraeger_ohneag, V(preistraeger_ohneag)[degree(preistraeger_ohneag, mode="all")=="0"]) 
outdpreistraeger_ohneag <- degree(preistraeger_ohneag, mode="out")
sort(outdpreistraeger_ohneag)
```
**Nach Jahren** selektieren/untersuchen (2015 beliebig ersetzen)
```
preistraegeryear <- subgraph.edges(preistraeger, E(preistraeger)[which(year == 2015)])
preistraegeryear
```
Selektiere PT, die zwei oder mehr Preise gewonnen haben:
```
preistraegeryear <- delete_vertices (preistraegeryear, V(preistraegeryear)[(type == 0) &(degree(preistraegeryear, mode="out")<3)])
```
Sortiere die übrigen nach Outdegree (Wer hat zwei oder mehr Preise gewonnen in Jahr x?):
```
outdpreistraeger <- degree(preistraegeryear, mode="out")
sort(outdpreistraeger)
```
Sortiere die übrigen nach Indegree (Wo haben die Personen gearbeitet, die im Jahr x zwei oder mehr Preise gewonnen haben?):  
(ACHTUNG: Verzerrrung durch mehrere Arbeitgeber / Person --> visuelle Auswertung durch Plot!)
```
indpreistraegeryear <- degree(preistraegeryear, mode="in")
sort(indpreistraegeryear)
```


## JURY-NETZWERK

1. Entfernt alle Preisträger-Beziehungen aus edgelist
```
jury <- delete_edges (g, E(g)[relation == 1])
```
2. Namen extrahieren
```
Personen <- delete.vertices(jury, V(jury)[type!=0])
Namen <- V(Personen)$name
Namen
```
3. Schleife: Alle Namen durchschleifen: Hat eine Person keine relation = 1? --> Dann: Löschen.
```
for (i in (1:length(Namen))){
  ego <- delete_vertices(jury, V(jury)[(name != Namen[i]) & (type == 0)])
  jurybeziehungen <- E(ego)[relation == 3]
  if (sum(jurybeziehungen) == 0){
    jury <- delete_vertices(jury, V(jury)[name == Namen[i]])
  }
}
```
4. Löscht alleinstehende Nodes und wirft das Jurynetzwerk aus
```
jury <- delete_vertices (jury, V(jury)[degree(jury, mode="all")=="0"]) 
jury
```
Berechnet Out-Degree von Jurynetzwerk
```
outdjury <- degree(jury, mode="out")
outdjury
sort(outdjury)
```
Berechnet In-Degree von Jurynetzwerk
```
indjury <- degree(jury, mode="in")
indjury
sort(indjury)
```
Wer saß am Häufigsten in einer Jury? (Netzwerk mit nur Jurymitgliedern, relation == 3, sort nach Outdegree)
```
jury_ohneag <- delete.edges(jury, E(jury)[which(relation == 2)])
jury_ohneag <- delete_vertices (jury_ohneag, V(jury_ohneag)[degree(jury_ohneag, mode="all")=="0"]) 
outdjury_ohneag <- degree(jury_ohneag, mode="out")
sort(outdjury_ohneag)
```

## TEILNETZWERKE

### Elite-Netzwerke

Schritt 1: **Bereinigtes Netzwerk (Stufe 1)** (Entferne alle Nodes mit degree < 5)
```
g2_1 <- delete_vertices (g, V(g)[(type == 0) &(degree(g, mode="out")<5)])
g2 <- delete_vertices (g2_1, V(g2_1)[degree(g2_1, mode="all")=="0"])
g2
plot(g2)
```
Schritt 2: **Bereinigtes Netzwerk (Stufe 2)** (Entferne alle Nodes mit degree < 10)
```
g3_1 <- delete_vertices (g, V(g)[(type == 0) &(degree(g, mode="out")<10)])
g3 <- delete_vertices (g3_1, V(g3_1)[degree(g3_1, mode="all")=="0"])
g3
plot(g3)
```

Schritt 3: **Elite in Elite** (Entferne alle Nodes mit degree < 15)
```
g4_1 <- delete_vertices (g, V(g)[(type == 0) &(degree(g, mode="out")<15)])
g4 <- delete_vertices (g4_1, V(g4_1)[degree(g4_1, mode="all")=="0"])
g4
plot(g4)
```

### Netzwerk ohne Arbeitsverhältnisse
Lösche alle Arbeitsbeziehungen und freistehende Nodes im Netzwerk:  
```
ohneav <- delete.edges(g, E(g)[which(relation == 2)])
ohneav <- delete_vertices (ohneav, V(ohneav)[degree(ohneav, mode="all")=="0"])
ohneav  
```
Sind die Knoten im Netzwerk alle miteinander verbunden?
```
is.connected(ohneav)
```
Clusteranalyse:  
```
cl_ohneav <- cluster_walktrap(ohneav)
modularity(cl_ohneav)
communities(cl_ohneav)
sizes(cl_ohneav)
mean_distance(ohneav, directed=F)
betw <- betweenness(ohneav, directed=F)
sort(betw)
```

### Netzwerk mit ausschließlich Arbeitsverhältnissen

Selektiere alle Arbeitsverhältnisse(AV):
```
av <- subgraph.edges(g, E(g)[relation == 2])
av
plot(av)
```
Wie viele Arbeitgeber im Netzwerk?
```
arbeitgeber <- V(av)$type==1
sum(arbeitgeber, na.rm=T)
```
Selektiere Knoten mit Indegree < 4 und zähle Arbeitgeber darin:
```
V(av)$ind <- ind

av_klein <- induced.subgraph(av, V(av)[ind<4])
av_klein

arbeitgeber_klein <- V(av_klein)$type==1
sum(arbeitgeber_klein, na.rm=T)
```
Selektiere Knoten mit Indegree > 49:
```
av_groß <- induced.subgraph(av, V(av)[ind>49])
av_groß
```
Clusteranalyse:
```
is.connected(av)
clusters(av)
cl_av <- cluster_walktrap(av)
sizes <- sizes(cl_av)
sort(sizes)

modularity(cl_av)
edge_density(av)
```
Erster plot: Zeigt große Verdichtung zur Mitte
```
plot(av,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label=NA,
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk,
     main="Arbeitsbeziehungen im Gesamtnetzwerk",
     asp=0)
```
Auftrennen der einzelnen Komponenen --> Komponente Nr. 3 ist besonders groß:
```
av_comp <- decompose(av)
av_comp

av_comp[[3]]
```
Plot Komponente Nr.3:
```
V(av_comp[[3]])$label <- V(av_comp[[3]])$name
V(av_comp[[3]])$label <- ifelse(V(av_comp[[3]])$type==1, V(av_comp[[3]])$label, NA)
plot(av_comp[[3]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.6,
     vertex.label.cex=.55,
     layout = layout_with_kk,
     main="av_comp[[3]]",
     asp=0)
```

### Männer- & Frauennetzwerk
Frauen:
```
frauen <- delete_vertices(g, V(g)[which (sex == 1)])
frauen
```
weibliche PT
```
preistraeger_frauen <- subgraph.edges(frauen, E(frauen)[relation == 1])
preistraeger_frauen
```
Jurorinnen
```
jury_frauen <- subgraph.edges(frauen, E(frauen)[relation == 3])
jury_frauen
```
Männer:
```
maenner <- delete_vertices(g, V(g)[which (sex == 2)])
maenner
```
männliche PT
```
preistraeger_maenner <- subgraph.edges(maenner, E(maenner)[relation == 1])
preistraeger_maenner
```
Juroren
```
jury_maenner <- subgraph.edges(maenner, E(maenner)[relation == 3])
jury_maenner
```


### PREISSPEZIFISCHE NETZWERKE

Erstellt Teilnetzwerke der einzelnen Preise

#### Preisauswahl
Zuweisung des einzelnen Preisnamen (Bsp. "Alternativer Medienpreis", siehe Legende), dann run der Befehle
```
Preis <- "Alternativer Medienpreis"
```
Erzeuge Preisnetzwerk **ersten Grades** (keine Bereinigung notwendig)
```
x1 <- subgraph <- make_ego_graph(g, order=1, Preis)
plot(x1[[1]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Erzeuge unbereinigtes Preisnetzwerk **zweiten Grades**
```
x2 <- subgraph <- make_ego_graph(g, order=2, Preis)
plot(x2[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

#### Bereinigung

Erzeuge unbereinigte Preisnetzwerke 2. Grades **nach Jahren**
```
x15 <- subgraph <- make_ego_graph(year2015, order=2, Preis)
x15 <- delete_vertices(x15[[1]], V(x15[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x15, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x16 <- subgraph <- make_ego_graph(year2016, order=2, Preis)
x16 <- delete_vertices(x16[[1]], V(x16[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x16, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x17 <- subgraph <- make_ego_graph(year2017, order=2, Preis)
x17 <- delete_vertices(x17[[1]], V(x17[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x17, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x18 <- subgraph <- make_ego_graph(year2018, order=2, Preis)
x18 <- delete_vertices(x18[[1]], V(x18[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x18, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x19 <- subgraph <- make_ego_graph(year2019, order=2, Preis)
x19 <- delete_vertices(x19[[1]], V(x19[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x19, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

Alle nicht zugehörigen Arbeitsverhältnisse löschen
```
x2 <- x2[[1]] - (x2[[1]] - x15 - x16 - x17 - x18 - x19)
```
Ausnahme für Kurt-Tucholsky-Preis (keine Preisträger in den Jahren 2016 und 2018):
```
x <- x[[1]] - (x[[1]] - x15 - x17 - x19)
```
Alle freistehende Nodes löschen
```
x2 <- delete_vertices (x2, V(x2)[degree(x2, mode="all")=="0"])
```
Bereinigtes Preisnetzwerk
```
plot(x2, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

#### Auswertung
Wie viele **Personen** gibt es (einfach gezählt) im Preisnetzwerk?
davon Jurymitglieder (einfach gezählt)
davon Preisträger (einfach gezählt)
--> wie oben mit g Gesamtnetzwerk!  

Wie viele **Beziehungen zum Preis** (über Jurymitgliedschaft oder Preisträgerschaft) gibt es im Preisnetzwerk?
```
beziehungen_x <- (E(x1[[1]])$relation == 1) + (E(x1[[1]])$relation == 3)
sum(beziehungen_x, na.rm = TRUE)
```
Anzahl **Jurymitglieder-Beziehungen** (alle gezählt)
```
jury_relations_x <- E(x1[[1]])$relation == 3
sum(jury_relations_x, na.rm = TRUE)
```
Anzahl **Preisträger-Beziehungen** (alle gezählt)
```
preistraeger_relations_x <- E(x1[[1]])$relation == 1
sum(preistraeger_relations_x, na.rm = TRUE)
```

Nach Indegree sortieren (**häufigster Arbeitgeber**):
```
ind_x2 <- degree(x2, mode="in")
sort(ind_x2)
```

Anzahl **Arbeitgeberbeziehungen**
```
ag_x <- (E(x2)$relation == 2)
sum(ag_x, na.rm = TRUE)
```
Anzahl **Arbeitgeberbeziehungen unter Preisträgern**
1. Lösche Jury-Beziehungen
```
x2_pt <- delete.edges(x2, E(x2)[which(relation == 3)])
```
2. Lösche alleinstehende Nodes
```
x2_pt <- delete.vertices(x2_pt, V(x2_pt)[degree(x2_pt, mode="all")==0])
```
3. Bilde neuen ego-graph 2. Grades um Preis
```
x2_pt <- subgraph <- make_ego_graph(x2_pt, order=2, Preis)
plot(x2_pt[[1]], vertex.size=4, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Selektiere Arbeitsbeziehungen (wie oben)
```
ag_x2_pt <- (E(x2_pt[[1]])$relation == 2)
sum(ag_x2_pt, na.rm = TRUE)
```
Nach Indegree sortieren (**häufigster Arbeitgeber unter Preisträgern**):
```
ind_x_pt <- degree(x2_pt[[1]], mode="in")
sort(ind_x_pt)
```

#### Männer-/Frauenverteilung in Teilnetzwerken der Preise
Frauen gesamt
```
frauen_x <- delete_vertices(x1[[1]], V(x1[[1]])[which (sex == 1)])
gsize(frauen_x)
```
Anzahl Frauen in Jury (alle einzeln gezählt)
```
frauen_jury_x <- E(frauen_x)$relation == 3
sum(frauen_jury_x, na.rm = TRUE)
```
Anzahl Frauen unter Preisträgern
```
frauen_preistraeger_x <- E(frauen_x)$relation == 1	
sum(frauen_preistraeger_x, na.rm = TRUE)
```


### Gruppenpreis- und Einzelnetzwerke  

#### Wie viele Einzel- und Gruppenpreise wurden verliehen?**  
```
einzelpreise_count <- subgraph.edges(g, E(g)[which(format == 1)])
gsize(einzelpreise_count)
gruppenpreise_count <- subgraph.edges(g, E(g)[which(format == 2)])
gsize(gruppenpreise_count)
```

#### Einzelpreisnetzwerk  
Netzwerk erstellen:  
```
einzelpreise1 <- delete_edges(g, E(g)[which(format == 2)])
einzelpreise <- delete_vertices (einzelpreise1, V(einzelpreise1)[degree(einzelpreise1, mode="all")=="0"])
einzelpreise
```

nach **Jahren**
```
einzelpreise15 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == 2015])
einzelpreise16 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == 2016])
einzelpreise17 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == 2017])
einzelpreise18 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == 2018])
einzelpreise19 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == 2019])
```
alternativ: siehe unten --> Auswertung nach Jahren

**Indegree** von Einzelpreisen  
```
indeinzelpreise <- degree(einzelpreise, mode="in")
sort(indeinzelpreise)
```
**Outdegree** von Einzelpreisen  
```
outdeinzelpreise <- degree(einzelpreise, mode="out")
sort(outdeinzelpreise)
```


#### Gruppenpreisnetzwerk  

Netzwerk **erstellen**:
```
gruppenpreise1 <- delete_edges(g, E(g)[which(format == 1)])
gruppenpreise <- delete_vertices (gruppenpreise1, V(gruppenpreise1)[degree(gruppenpreise1, mode="all")=="0"])
gruppenpreise
```
sonst: Auswertung wie Einzelpreise (s.o.), graph ersetzen!

### ARD-Netzwerk

1. Ego-Netzwerke aller ÖR-Sender erstellen
```
swr <- subgraph <- make_ego_graph(g, order = 1,  c("Südwestrundfunk (SWR)"))
wdr <- subgraph <- make_ego_graph(g, order = 1,  c("Westdeutscher Rundfunk (WDR)"))
radiobremen <- subgraph <- make_ego_graph(g, order = 1,  c("Radio Bremen"))
br <- subgraph <- make_ego_graph(g, order = 1,  c("Bayerischer Rundfunk (BR)"))
hr <- subgraph <- make_ego_graph(g, order = 1,  c("Hessischer Rundfunk (HR)"))
mdr <- subgraph <- make_ego_graph(g, order = 1,  c("Mitteldeutscher Rundfunk (MDR)"))
ndr <- subgraph <- make_ego_graph(g, order = 1,  c("Norddeutscher Rundfunk (NDR)"))
sr <- subgraph <- make_ego_graph(g, order = 1,  c("Saarländischer Rundfunk (SR)"))
funk <- subgraph <- make_ego_graph(g, order = 1,  c("Funk (ARD)"))
rbb <- subgraph <- make_ego_graph(g, order = 1,  c("Radio Berlin Brandeburg (rbb)"))
ard1 <- subgraph <- make_ego_graph(g, order = 1,  c("ARD"))
dlr <- subgraph <- make_ego_graph(g, order = 1,  c("Deutschlandradio"))
dw <- subgraph <- make_ego_graph(g, order = 1,  c("Deutsche Welle"))
```
2. Von Gesamtnetzwerk doppelt subtrahieren (= addieren):
```
ard <- g - (g - swr[[1]] - wdr[[1]] - radiobremen[[1]] - br[[1]] - hr[[1]] - mdr[[1]] - ndr[[1]] - sr[[1]] - funk[[1]] - rbb[[1]] - ard1[[1]] - dlr[[1]] - dw[[1]]) 
```
3. Nodes ohne Beziehungen löschen und plotten:
```
ard <- delete_vertices (ard, V(ard)[degree(ard, mode="all")=="0"])
plot(ard)
```

### Das KONSERVATIVE Netzwerk
(besteht aus: Theodor-Wolff-Preis, Herbert-Quandt-Preis, Ludwig-Erhard-Preis --> größter Anteil konservativer Medien unter PT & Jury)

**1. Ludwig-Erhard-Preis**
```
Preis <- "Ludwig-Erhard-Preis für Wirtschaftspublizistik"
```
Erzeuge Preisnetzwerk ersten Grades (keine Bereinigung notwendig)
```
x1 <- subgraph <- make_ego_graph(g, order=1, Preis)
plot(x1[[1]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Erzeuge unbereinigtes Preisnetzwerk zweiten Grades
```
x2 <- subgraph <- make_ego_graph(g, order=2, Preis)
plot(x2[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

Bereinigung
Erzeuge unbereinigte Preisnetzwerke 2. Grades nach Jahren
  ```
x15 <- subgraph <- make_ego_graph(year2015, order=2, Preis)
x15 <- delete_vertices(x15[[1]], V(x15[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x15
plot(x15, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x16 <- subgraph <- make_ego_graph(year2016, order=2, Preis)
x16 <- delete_vertices(x16[[1]], V(x16[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x16
plot(x16, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x17 <- subgraph <- make_ego_graph(year2017, order=2, Preis)
x17 <- delete_vertices(x17[[1]], V(x17[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x17
plot(x17, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x18 <- subgraph <- make_ego_graph(year2018, order=2, Preis)
x18 <- delete_vertices(x18[[1]], V(x18[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x18
plot(x18, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x19 <- subgraph <- make_ego_graph(year2019, order=2, Preis)
x19 <- delete_vertices(x19[[1]], V(x19[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x19
plot(x19, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Alle nicht zugehörigen Arbeitsverhältnisse löschen
```
x2 <- x2[[1]] - (x2[[1]] - x15 - x16 - x17 - x18 - x19)
```
Alle freistehende Nodes löschen
```
ludwigerhardpreis <- delete_vertices (x2, V(x2)[degree(x2, mode="all")=="0"])
```
Bereinigtes Preisnetzwerk
```
plot(ludwigerhardpreis, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
**2. Theodor-Wolff-Preis**
```
Preis <- "Theodor-Wolff-Preis"
```
Erzeuge Preisnetzwerk ersten Grades (keine Bereinigung notwendig)
```
x1 <- subgraph <- make_ego_graph(g, order=1, Preis)
plot(x1[[1]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Erzeuge unbereinigtes Preisnetzwerk zweiten Grades
  ```
x2 <- subgraph <- make_ego_graph(g, order=2, Preis)
plot(x2[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Bereinigung
Erzeuge unbereinigte Preisnetzwerke 2. Grades nach Jahren
  ```
x15 <- subgraph <- make_ego_graph(year2015, order=2, Preis)
x15 <- delete_vertices(x15[[1]], V(x15[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x15
plot(x15, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x16 <- subgraph <- make_ego_graph(year2016, order=2, Preis)
x16 <- delete_vertices(x16[[1]], V(x16[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x16
plot(x16, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x17 <- subgraph <- make_ego_graph(year2017, order=2, Preis)
x17 <- delete_vertices(x17[[1]], V(x17[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x17
plot(x17, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x18 <- subgraph <- make_ego_graph(year2018, order=2, Preis)
x18 <- delete_vertices(x18[[1]], V(x18[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x18
plot(x18, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x19 <- subgraph <- make_ego_graph(year2019, order=2, Preis)
x19 <- delete_vertices(x19[[1]], V(x19[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x19
plot(x19, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Alle nicht zugehörigen Arbeitsverhältnisse löschen
```
x2 <- x2[[1]] - (x2[[1]] - x15 - x16 - x17 - x18 - x19)
```
Alle freistehende Nodes löschen
```
theodorwolffpreis <- delete_vertices (x2, V(x2)[degree(x2, mode="all")=="0"])
```
Bereinigtes Preisnetzwerk
```
plot(theodorwolffpreis, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
**3. Herbert-Quandt-Preis**
```
Preis <- "Herbert Quandt Medienpreis"
```
Erzeuge Preisnetzwerk ersten Grades (keine Bereinigung notwendig)
```
x1 <- subgraph <- make_ego_graph(g, order=1, Preis)
plot(x1[[1]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Erzeuge unbereinigtes Preisnetzwerk zweiten Grades
  ```
x2 <- subgraph <- make_ego_graph(g, order=2, Preis)
plot(x2[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Bereinigung:
Erzeuge unbereinigte Preisnetzwerke 2. Grades nach Jahren
  ```
x15 <- subgraph <- make_ego_graph(year2015, order=2, Preis)
x15 <- delete_vertices(x15[[1]], V(x15[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x15
plot(x15, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x16 <- subgraph <- make_ego_graph(year2016, order=2, Preis)
x16 <- delete_vertices(x16[[1]], V(x16[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x16
plot(x16, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x17 <- subgraph <- make_ego_graph(year2017, order=2, Preis)
x17 <- delete_vertices(x17[[1]], V(x17[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x17
plot(x17, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x18 <- subgraph <- make_ego_graph(year2018, order=2, Preis)
x18 <- delete_vertices(x18[[1]], V(x18[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x18
plot(x18, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

x19 <- subgraph <- make_ego_graph(year2019, order=2, Preis)
x19 <- delete_vertices(x19[[1]], V(x19[[1]])[which ((institutiontype == 1)  & (name != Preis))])
x19
plot(x19, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Alle nicht zugehörigen Arbeitsverhältnisse löschen
```
x2 <- x2[[1]] - (x2[[1]] - x15 - x16 - x17 - x18 - x19)
```
Alle freistehende Nodes löschen
```
herbertquandtpreis <- delete_vertices (x2, V(x2)[degree(x2, mode="all")=="0"])
```
Bereinigtes Preisnetzwerk
```
plot(herbertquandtpreis, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
**Einzelnetzwerke zusammenfassen**
Alle nicht zugehörigen Arbeitsverhältnisse löschen
```
ludwigerhardpreis
theodorwolffpreis
herbertquandtpreis

konservativ <- g - (g - ludwigerhardpreis - theodorwolffpreis - herbertquandtpreis)
konservativ
```
Alle freistehende Nodes löschen
```
konservativ <- delete_vertices (konservativ, V(konservativ)[degree(konservativ, mode="all")=="0"])
```
Bereinigtes Preisnetzwerk
```
plot(konservativ, edge.arrow.size=.1, vertex.size=5, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_fr)
```

### Das LINKE Netzwerk
(besteht aus Leuchtturm, Dt. Reporterpreis und Robert-Geisendörfer-Preis, weil dort sehr geringer Anteil konservativerer Medien)
Aufbau: wie oben, nur mit drei linkeren Preisen...


## Auswertung nach Jahren

Jahresauswahl
```
Jahr <- "2019"
```
Jahresnetzwerk erzeugen
```
year20xx <- subgraph.edges(g, E(g)[year == Jahr]) 
year20xx
plot(year20xx,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Anzahl Preisträger-Beziehungen im Netzwerk:
```
preistraeger_relations_x <- E(year20xx)$relation == 1
sum(preistraeger_relations_x, na.rm = TRUE)
```
Anzahl Jury-Beziehungen im Netzwerk:
```
jury_relations_x <- E(year20xx)$relation == 3
sum(jury_relations_x, na.rm = TRUE)
```
  
#### Auswertung

**Geschlecht** nach Jahren zählen
```
maenner20xx <- V(year20xx)$sex == 1
sum (maenner20xx, na.rm = TRUE)

frauen20xx <- V(year20xx)$sex == 2
sum (frauen20xx, na.rm = TRUE)
```
**Jury** nach Jahren zählen
```
jury20xx <- subgraph.edges(jury, E(jury)[year == Jahr])
plot(jury20xx,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Geschlechter in Jury nach Jahren
```
frauenjury20xx <- V(jury20xx)$sex == 2
sum (frauenjury20xx, na.rm = TRUE)
maennerjury20xx <- V(jury20xx)$sex == 1
sum (maennerjury20xx, na.rm = TRUE)

```
**Preisträger** nach Jahren zählen
```
preistraeger20xx <- subgraph.edges(preistraeger, E(preistraeger)[year == Jahr])
plot(preistraeger20xx,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Geschlechter unter Preisträgern nach Jahren
```
frauenpreistraeger20xx <- V(preistraeger20xx)$sex == 2
sum (frauenpreistraeger20xx, na.rm = TRUE)
maennerpreistraeger20xx <- V(preistraeger20xx)$sex == 1
sum (maennerpreistraeger20xx, na.rm = TRUE)
```

**Dominanz einzelner Medienunternehmen** nach Jahren  

Größten Indegree (_gesamt_) nach Jahren berechnen:
```
ind20xx <- degree(year20xx, mode="in")
sort(ind20xx)
```
Größten Indegree (_unter PT_) nach Jahren berechnen:
```
indpreistraeger20xx <- degree(preistraeger20xx, mode="in")
sort(indpreistraeger20xx)
```
Größten Indegree (_unter Jury_) nach Jahren berechnen:
```
indjury20xx <- degree(juryfinal20xx, mode="in")
sort(indjury20xx)
```

Auswertung der **Gruppen- und Einzelpreise** nach Jahren
Für **Gruppenpreisnetzwerk**:
```
gruppenpreise20xx <- subgraph.edges(gruppenpreise, E(gruppenpreise)[year == Jahr]) 
indgruppenpreise20xx <- degree(gruppenpreise20xx, mode="in")
sort(indgruppenpreise20xx)
```
Für **Einzelpreisnetzwerk**:
```
einzelpreise20xx <- subgraph.edges(einzelpreise, E(einzelpreise)[year == Jahr]) 
indeinzelpreise20xx <- degree(einzelpreise20xx, mode="in")
sort(indeinzelpreise20xx)
```
Gruppenpreise nach Jahren zählen (**Summe**)
```
gruppenpreise20xx_einzeln <- E(year20xx)$format == 2
gruppenpreise20xx_einzeln
sum(gruppenpreise20xx_einzeln, na.rm = TRUE)
```
Einzelpreise nach Jahren zählen (**Summe**)
```
einzelpreise20xx_einzeln <- E(year2016)$format == 1
einzelpreise20xx_einzeln
sum(einzelpreise20xx_einzeln, na.rm = TRUE)
```


## Netzwerke von Medienunternehmen
Unternehmen auswählen (z.B. Zeit)  
```
Unternehmen <- "Die ZEIT"
```
Ego-Netzwerk (zweiten Grades) erstellen  
```
unternehmen <- subgraph <- make_ego_graph(g, order=2, Unternehmen)
unternehmen
plot(unternehmen[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
**Auswertung** der Netzwerke:  
Preisträger-Beziehungen:  
```
preistraeger_relations_unternehmen <- E(unternehmen[[1]])$relation == 1
sum(preistraeger_relations_unternehmen, na.rm = TRUE)
```
Jury-Beziehungen:  
```
jury_relations_unternehmen <- E(unternehmen[[1]])$relation == 3
sum(jury_relations_unternehmen, na.rm = TRUE)
```
Wie viele Personen im Netzwerk?  
```
unternehmen_personen <- induced_subgraph(unternehmen[[1]], V(unternehmen[[1]])[which (type == 0)])
unternehmen_personen
```
Wie viele Preisträger im Netzwerk?  
```
preistraeger <- subgraph.edges(g, E(g)[which (relation == 1)])
preistraeger_personen <- induced.subgraph(preistraeger, V(preistraeger)[type == 0])
preistraeger_personen
```
Wie viele Juroren im Netzwerk?  
```
juroren <- subgraph.edges(g, E(g)[which (relation == 3)])
juroren_personen <- induced.subgraph(juroren, V(juroren)[type == 0])
juroren_personen
```
Wie viele Gruppenpreise wurden verliehen?  
```
gruppenpreise_unternehmen <- (E(unternehmen[[1]])$format == 2)
sum(gruppenpreise_unternehmen, na.rm = TRUE)
```
Bei wie vielen Preisen haben Mitarbeiter des Unternehmens etwas gewonnen?  
```
unternehmen_preise <- induced_subgraph(unternehmen[[1]], V(unternehmen[[1]])[which (institutiontype == 1)])
unternehmen_preise
```


## Ressorts

**Gesamt**

1. Alle Nicht-Arbeitsbeziehungen löschen
```
task <- delete.edges(x2, E(x2)[relation != 2])
```
2. Alle Arbeitsbeziehungen löschen, die task = "NA" sind
```
task <- delete.edges(task, E(task)[(relation == 2) & (is.na(task))])
```
3. Alle freistehenden Nodes löschen
```
task <- delete_vertices(task, V(task)[degree(task, mode="all")=="0"])
task
```
ODER:
```
task <- subgraph.edges(g, E(g)[which (relation == 2) & (!is.na(task))])
task
```
4. Weise "tasks" die verschiedenen Kategorien der Ressorts zu
```
tasks <- c("leader", "investigative", "data", "economics", "digital", "local", "freelancer", "former", "reporter", "trainee", "presenter", "politics", "empty", "empty", "others")
```
5. Schleife alle Kategorien durch alle übrig gebliebenen relations == 2 und summiere für jede Kategorie
```
for (i in (1:15)){
  summe <- 0
  taskx <- E(task)$task == i
  summe <- sum(taskx, na.rm = TRUE)
  print(tasks[i])
  print(summe)
  }

length(taskx)
```

unter **Preisträgern**  
(wie oben, nur Selektion der Preisträger-relations durch preistraeger-Netzwerk)  
```
task_ber <- preistraeger
task_ber <- delete.edges(preistraeger, E(preistraeger)[relation != 2])
task_ber <- delete.edges(task_ber, E(task_ber)[(relation == 2) & (is.na(task))])
task_ber <- delete_vertices(task_ber, V(task_ber)[degree(task_ber, mode="all")=="0"])
task_ber
```
ODER:
```
preistraeger_av <- subgraph.edges(preistraeger, E(preistraeger)[which (relation == 2)])
preistraeger_task <- subgraph.edges(preistraeger_av, E(preistraeger_av)[which (!is.na(task))])
preistraeger_task
```
Schleife:
```
for (i in (1:15)){
  summe <- 0
  taskx <- E(task_ber)$task == i
  summe <- sum(taskx, na.rm = TRUE)
  print(tasks[i])
  print(summe)
}
```

unter **Jurymitgliedern**  
(wie oben, nur Selektion der Jury-relations durch jury-Netzwerk)  
```
task_ber <- jury
task_ber <- delete.edges(jury, E(jury)[relation != 2])
task_ber <- delete.edges(task_ber, E(task_ber)[(relation == 2) & (is.na(task))])
task_ber <- delete_vertices(task_ber, V(task_ber)[degree(task_ber, mode="all")=="0"])
task_ber
```
ODER:
```
jury_av <- subgraph.edges(jury, E(jury)[which (relation == 2)])
jury_task <- subgraph.edges(jury_av, E(jury_av)[which (!is.na(task))])
jury_task
```
Schleife:
```
for (i in (1:15)){
  summe <- 0
  taskx <- E(task_ber)$task == i
  summe <- sum(taskx, na.rm = TRUE)
  print(tasks[i])
  print(summe)
}
```

## SONSTIGES GESAMTNETZWERK

### CLUSTERANALYSE
Clusteranalyse im **Gesamtnetzwerk**:  
```
clusters(g)
clg <- cluster_walktrap(g)

membership(clg)
modularity(clg)
communities(clg)
size_clg2 <- sizes(clg)
sort(size_clg)
```
Clusteranalyse im **Netzwerk Stufe 1**:  
```
clusters(g2)
clg2 <- cluster_walktrap(g2)
  
membership(clg2)
modularity(clg2)
communities(clg2)
size_clg2 <- sizes(clg2)
sort(size_clg2)
```  
Clusteranalyse im **Netzwerk Stufe 2**:  
```
clusters(g3)
```
Zwei Komponenten im Netzwerk. Auftrennen mit decompose():  
```
g3_comp <- decompose.graph(g3)
g3_comp
clg3 <- cluster_walktrap(g3)
  
membership(clg3)
modularity(clg3)
communities(clg3)
size_clg3 <- sizes(clg3)
sort(size_clg3)
```
Clusteranalyse im **Elitenetzwerk**:  
```
clusters(g4)
clg4 <- cluster_walktrap(g4)
  
membership(clg4)
modularity(clg4)
communities(clg4)
size_clg4 <- sizes(clg4)
sort(size_clg4)
```  

### Personen, die bei einem Preis Preisträger sind und in Jury sitzen:

**2015**  
```
x15 <- subgraph <- make_ego_graph(year2015, order=2, Preis)
x15 <- delete_vertices(x15[[1]], V(x15[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x15, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

#Test: Jurymitglied und Preisträger bei gleichem Arbeitgeber?
AGlist <- V(x15)[(V(x15)$type == 1) & (institutiontype == 2)]
countcases <- 0

anzahl_jurymitglieder <- E(x15)$relation == 3
anzahl_preistraeger <- E(x15)$relation == 1

for(i in 1:length(AGlist)) {
  AG <- subgraph <- make_ego_graph(x15, order=2, AGlist[i])
  anzahl_jurymitgliederAG <- E(AG[[1]])$relation == 3       
  anzahl_preistraegerAG <- E(AG[[1]])$relation == 1    
  
  if ((sum(anzahl_jurymitgliederAG) > 0) & (sum(anzahl_preistraegerAG) > 0)) 
  {
    print(AGlist[i])
    print("Nummer in AGlist:")
    print(i)
    print("Anzahl Preisträger absolut:")
    print(sum(anzahl_preistraegerAG))
    print("Anzahl Jurymitglieder absolut:")
    print(sum(anzahl_jurymitgliederAG))
    print("Anteil an Preisträgern:")
    print((sum(anzahl_preistraegerAG) / sum(anzahl_preistraeger)))
    print("Anteil an Jurymitgliedern")
    print((sum(anzahl_jurymitgliederAG) / sum(anzahl_jurymitglieder)))
    
    countcases <- countcases + 1
    plot(AG[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
  } 
  
  else
  {
    print("OK")
  }
}
print(countcases)

#zum Plotten der Einzelnetzwerke bei mehreren Fällen
AGtest <- subgraph <- make_ego_graph(x15, order=2, AGlist[9])
plot(AGtest[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

**2016**  
```
x16 <- subgraph <- make_ego_graph(year2016, order=2, Preis)
x16 <- delete_vertices(x16[[1]], V(x16[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x16, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

#Test: Jurymitglied und Preisträger bei gleichem Arbeitgeber?
AGlist <- V(x16)[(V(x16)$type == 1) & (institutiontype == 2)]
countcases <- 0

anzahl_jurymitglieder <- E(x16)$relation == 3
anzahl_preistraeger <- E(x16)$relation == 1

for(i in 1:length(AGlist)) {
  AG <- subgraph <- make_ego_graph(x16, order=2, AGlist[i])
  anzahl_jurymitgliederAG <- E(AG[[1]])$relation == 3       
  anzahl_preistraegerAG <- E(AG[[1]])$relation == 1    
  
  if ((sum(anzahl_jurymitgliederAG) > 0) & (sum(anzahl_preistraegerAG) > 0)) 
  {
    print(AGlist[i])
    print("Nummer in AGlist:")
    print(i)
    print("Anzahl Preisträger absolut:")
    print(sum(anzahl_preistraegerAG))
    print("Anzahl Jurymitglieder absolut:")
    print(sum(anzahl_jurymitgliederAG))
    print("Anteil an Preisträgern:")
    print((sum(anzahl_preistraegerAG) / sum(anzahl_preistraeger)))
    print("Anteil an Jurymitgliedern")
    print((sum(anzahl_jurymitgliederAG) / sum(anzahl_jurymitglieder)))
    
    countcases <- countcases + 1
    plot(AG[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
  } 
  
  else
  {
    print("OK")
  }
}
print(countcases)

#zum Plotten der Einzelnetzwerke bei mehreren Fällen
AGtest <- subgraph <- make_ego_graph(x16, order=2, AGlist[i])
plot(AGtest[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

**2017**  

x17 <- subgraph <- make_ego_graph(year2017, order=2, Preis)
x17 <- delete_vertices(x17[[1]], V(x17[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x17, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

#Test: Jurymitglied und Preisträger bei gleichem Arbeitgeber?
AGlist <- V(x17)[(V(x17)$type == 1) & (institutiontype == 2)]
countcases <- 0

anzahl_jurymitglieder <- E(x17)$relation == 3
anzahl_preistraeger <- E(x17)$relation == 1

for(i in 1:length(AGlist)) {
  AG <- subgraph <- make_ego_graph(x17, order=2, AGlist[i])
  anzahl_jurymitgliederAG <- E(AG[[1]])$relation == 3       
  anzahl_preistraegerAG <- E(AG[[1]])$relation == 1    
  
  if ((sum(anzahl_jurymitgliederAG) > 0) & (sum(anzahl_preistraegerAG) > 0)) 
  {
    print(AGlist[i])
    print("Nummer in AGlist:")
    print(i)
    print("Anzahl Preisträger absolut:")
    print(sum(anzahl_preistraegerAG))
    print("Anzahl Jurymitglieder absolut:")
    print(sum(anzahl_jurymitgliederAG))
    print("Anteil an Preisträgern:")
    print((sum(anzahl_preistraegerAG) / sum(anzahl_preistraeger)))
    print("Anteil an Jurymitgliedern")
    print((sum(anzahl_jurymitgliederAG) / sum(anzahl_jurymitglieder)))
    
    countcases <- countcases + 1
    plot(AG[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
  } 
  
  else
  {
    print("OK")
  }
}
print(countcases)

#zum Plotten der Einzelnetzwerke bei mehreren Fällen
AGtest <- subgraph <- make_ego_graph(x17, order=2, AGlist[i])
plot(AGtest[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

**2018**  
```
x18 <- subgraph <- make_ego_graph(year2018, order=2, Preis)
x18 <- delete_vertices(x18[[1]], V(x18[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x18, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

#Test: Jurymitglied und Preisträger bei gleichem Arbeitgeber?
AGlist <- V(x18)[(V(x18)$type == 1) & (institutiontype == 2)]
countcases <- 0

anzahl_jurymitglieder <- E(x18)$relation == 3
anzahl_preistraeger <- E(x18)$relation == 1

for(i in 1:length(AGlist)) {
  AG <- subgraph <- make_ego_graph(x18, order=2, AGlist[i])
  anzahl_jurymitgliederAG <- E(AG[[1]])$relation == 3       
  anzahl_preistraegerAG <- E(AG[[1]])$relation == 1    
  
  if ((sum(anzahl_jurymitgliederAG) > 0) & (sum(anzahl_preistraegerAG) > 0)) 
  {
    print(AGlist[i])
    print("Nummer in AGlist:")
    print(i)
    print("Anzahl Preisträger absolut:")
    print(sum(anzahl_preistraegerAG))
    print("Anzahl Jurymitglieder absolut:")
    print(sum(anzahl_jurymitgliederAG))
    print("Anteil an Preisträgern:")
    print((sum(anzahl_preistraegerAG) / sum(anzahl_preistraeger)))
    print("Anteil an Jurymitgliedern")
    print((sum(anzahl_jurymitgliederAG) / sum(anzahl_jurymitglieder)))
    
    countcases <- countcases + 1
    plot(AG[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
  } 
  
  else
  {
    print("OK")
  }
}
print(countcases)

#zum Plotten der Einzelnetzwerke bei mehreren Fällen
AGtest <- subgraph <- make_ego_graph(x18, order=2, AGlist[i])
plot(AGtest[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

**2019**  
```
x19 <- subgraph <- make_ego_graph(year2019, order=2, Preis)
x19 <- delete_vertices(x19[[1]], V(x19[[1]])[which ((institutiontype == 1)  & (name != Preis))])
plot(x19, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)

#Test: Jurymitglied und Preisträger bei gleichem Arbeitgeber?
AGlist <- V(x19)[(V(x19)$type == 1) & (institutiontype == 2)]
countcases <- 0

anzahl_jurymitglieder <- E(x19)$relation == 3
anzahl_preistraeger <- E(x19)$relation == 1

for(i in 1:length(AGlist)) {
  AG <- subgraph <- make_ego_graph(x19, order=2, AGlist[i])
  anzahl_jurymitgliederAG <- E(AG[[1]])$relation == 3       
  anzahl_preistraegerAG <- E(AG[[1]])$relation == 1    
  
  if ((sum(anzahl_jurymitgliederAG) > 0) & (sum(anzahl_preistraegerAG) > 0)) 
  {
    print(AGlist[i])
    print("Nummer in AGlist:")
    print(i)
    print("Anzahl Preisträger absolut:")
    print(sum(anzahl_preistraegerAG))
    print("Anzahl Jurymitglieder absolut:")
    print(sum(anzahl_jurymitgliederAG))
    print("Anteil an Preisträgern:")
    print((sum(anzahl_preistraegerAG) / sum(anzahl_preistraeger)))
    print("Anteil an Jurymitgliedern")
    print((sum(anzahl_jurymitgliederAG) / sum(anzahl_jurymitglieder)))
    
    countcases <- countcases + 1
    plot(AG[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
  } 
  
  else
  {
    print("OK")
  }
}
print(countcases)
```
zum Plotten der Einzelnetzwerke bei mehreren Fällen  
```
AGtest <- subgraph <- make_ego_graph(x19, order=2, AGlist[10])
plot(AGtest[[1]], vertex.size = 5, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

#### Preisgeld  

**Summe aller Preisgelder**  
nur Preisträgerbeziehungen selektieren:  
```
preise <- subgraph.edges(g,E(g)[relation ==1])
```
alle NA löschen:
```
preise2 <- delete.edges(preise, E(preise)[is.na(money)])
```
Selektion des edge-Attributs "money":
```
money_all <- E(preise2)$money
```
Durchschleifen der Preisgelder, dabei numerischen Wert zuweisen und aufaddieren:
```
length(money_all)
a <- money_all[1]
a

summe <- 0
for(i in (1:(length(money_all)))){
  a <- as.numeric(money_all[i])
  summe <- summe + a
}
summe
```

**Preisgelder, die nur an Einzelpreisträger gingen**  
Nur Preisverleihungen selektieren, die an Einzelpreisträger gingen:
```
preise3 <- subgraph.edges(preise2, E(preise2)[format == 1])
```
Dann wieder Durchschleifen und addieren:
```
money_all <- E(preise3)$money

length(money_all)
a <- money_all[1]
a

summe <- 0
for(i in (1:(length(money_all)))){
  a <- as.numeric(money_all[i])
  summe <- summe + a
}
summe
```

**Wer hat am meisten Geld abgeräumt in den letzten fünf Jahren?**  
Alle Preise löschen, die nicht dotiert sind:
```
preise3 <- delete.edges(preise2, E(preise2)[money == 0])
```
Alle freistehenden Nodes löschen:
```
preise4 <- delete.vertices(preise3, V(preise3)[degree(preise3, mode="all")==0])
```
Edge-Attribut "Money" selektieren und wieder durchschleifen: 
```
money_all <- E(preise4)$money
money_all <- as.numeric(money_all)
money_all

sum(money_all)
plot(preise4)
```
Anzahl der Personen (Nodes), die Preisgeld bekommen haben:
```
anzahl_personen_preisgeld <- V(preise4)$type == 0
anzahl_personen_preisgeld
sum(anzahl_personen_preisgeld)
```
Die Namen aller Personen im Netzwerk selektieren und Begriff "Namen" zuweisen:
```
Personen <- delete.vertices(preise4, V(preise4)[type!=0])
Namen <- V(Personen)$name
Namen
length(Namen)
```
Erzeuge Tabelle, die alle Preisträger und zusammengerechnete Preisgelder enthalten kann
```
Preisgelder <- c(1:length(Namen))
Preisgelder
Preistraeger <- c(1:length(Namen))
matrix <- cbind(Preisgelder, Preistraeger)
matrix
```
Alle Preisträger durchschleifen (und in erste Spalte der Tabelle eintragen)...
```
for (i in (1:length(Namen))){
  Name <- Namen[i]
  PT <- subgraph <- make_ego_graph(preise4, order=1, Namen[i])
  matrix[i, 1] <- Namen[i]
  money <- E(PT[[1]])$money
```
... und ihre Preisgelder aufaddieren (und in zweite Spalte der Tabelle eintragen):
```
  summe <- 0
  for(j in (1:(length(money)))){
    summe <- summe + money[j]
  }
  matrix[i, 2] <- summe     
}
```
Zeige Matrix mit Werten von oben:
```
matrix
length(matrix)
```
! Werte sind unsortiert, muss nun händisch nach größten Werten (+ Preisträger dazu) durchsucht werden, weil sort() nicht funktioniert.


### WANDERUNG
**Wer wandert innerhalb eines Preises von Preisträgern in Jury und umgekehrt?** 
Arbeitgeber löschen
```
ohneAG <- delete_vertices(g, V(g)[which (institutiontype == 2)])
```
Namen extrahieren
```
Personen <- delete.vertices(ohneAG, V(ohneAG)[type!=0])
Namen <- V(Personen)$name
Namen
```
Personen löschen, die keine Preisbeziegung oder keine Jurybeziehung haben
(dazu: Schleife alle Namen durch und lösche diejenigen, die Jurybeziehung oder Preisbeziehung = 0 haben)
```
PuJ <- ohneAG
for (i in (1:(length(Namen)))){
  ego <- subgraph <- make_ego_graph(g, order=1, Namen[i])
  jurybeziehung <- E(ego[[1]])$relation == 3
  preisbeziehung <- E(ego[[1]])$relation == 1
  if ((sum(jurybeziehung) == 0) | ((sum(preisbeziehung) == 0))){
    PuJ <- delete_vertices(PuJ, V(PuJ)[name == Namen[i]])
  }
}
```
Zeige Netzwerk
```
PuJ
```
**Welche Personen sind Preisträger und Jurymitglied im gleichen Preis (und im gleichen Jahr)?**
Personen und Preise extrahieren
```
Personen <- delete.vertices(PuJ, V(PuJ)[type!=0])
Namen <- V(Personen)$name
Namen

Preise <- delete.vertices(PuJ, V(PuJ)[type == 0])
Preise <- V(Preise)$name
Preise
```
1. Schleife alle Preise und print
```
for (i in (1:(length(Preise)))){
  ego <- subgraph <- make_ego_graph(g, order=1, Preise[i])
  print("######################")
  print(Preise[i])
  print(i)
  ```
  2. Schleife alle Personen
  ```
  for (j in (1:(length(Namen)))){
    ```
    3. alle anderen Personen löschen
    ```
    ego2 <- delete_vertices(ego[[1]], V(ego[[1]])[(name != Namen[j]) & (type == 0)])
    ```
    4. Beziehungen zählen
    ```
    jurybeziehung <- E(ego2)$relation == 3
    preisbeziehung <- E(ego2)$relation == 1
    ```
    5. wenn sowohl Preisträger als auch Jurymitglied --> print
    ```
    if ((sum(jurybeziehung) > 0) & ((sum(preisbeziehung) > 0))){
      print(Namen[j])
      print(j)
    ```
      6. Schleife alle Jahre
      ```
      for (k in (2015:2019)){
        Jahr <- as.character(k)
        ego3 <- delete_edges(ego2, E(ego2)[year != Jahr])
        jurybeziehung <- 0
        preisbeziehung <- 0
        jurybeziehung <- E(ego3)$relation == 3
        preisbeziehung <- E(ego3)$relation == 1
        ```
        7. Wenn Preisträger und Jurymitglied im selben Jahr --> print!
        ```
        if ((sum(jurybeziehung) > 0) & ((sum(preisbeziehung) > 0))){
          print(k)
          print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        }
      }
    }
  }
  print("   ")
}
```
Einzeltests: Bei Preise[] Nummer aus Liste eintragen (Preisnummer), ebenso bei Name[] (Namensnummer), um einzelne Personen zu plotten
```
ego <- subgraph <- make_ego_graph(g, order=1, Preise[12])                            #i = Preisnummer
ego2 <- delete_vertices(ego[[1]], V(ego[[1]])[(name != Namen[17]) & (type == 0)])    #i = Namensnummer
liste <- E(ego2)
edge_attr(ego2)

plot(ego2,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```

### Teilnetzwerk PuJ
= Teilnetzwerk mit ausschließlich Personen, die Jurymitglied & Preisträger zugleich im Netzwerk sind und ihren Beziehungen (ohne Arbeitsverhältnisse):  

Erstelle Netzwerk (siehe oben):  
```
PuJ
```
Wie viele Personen im Netzwerk?
```
Personen <- V(PuJ)$type==0
sum(Personen, na.rm=T)
```
Zeige Namen:
```
Namen <- V(PuJ)$name
Namen
```
Berechne Dichte:
```
edge_density(PuJ)
```
Clusteranalyse:
```
is.connected(PuJ)

cl_PuJ <- cluster_walktrap(PuJ)
modularity(cl_PuJ)
communities(cl_PuJ)
sizes(cl_PuJ)
betw <- betweenness(PuJ, directed=F)
sort(betw)
```
**Netzwerk ohne djp und J.d.J**
```
PuJ_ber <- delete.vertices(PuJ, V(PuJ)[49, 108])
PuJ_ber <- delete_vertices (PuJ_ber, V(PuJ_ber)[degree(PuJ_ber, mode="all")=="0"]) 
PuJ_ber
```
Wie viele Personen im Netzwerk?
```
Personen <- V(PuJ_ber)$type==0
sum(Personen, na.rm=T)
```
Zeige Namen:
```
Namen <- V(PuJ_ber)$name
Namen
```
Berechne Dichte:
```
edge_density(PuJ_ber)
```
Clusteranalyse:
```
is.connected(PuJ_ber)

cl_PuJ_ber <- cluster_walktrap(PuJ_ber)
modularity(cl_PuJ_ber)
communities(cl_PuJ_ber)
sizes(cl_PuJ_ber)
betw <- betweenness(PuJ_ber, directed=F)
sort(betw)
```

## Visualisierungen

### Grundsätzliche Darstellung
```
E(g)$curved=.2 
E(g)$vertex.label.family="Helvetica"
```

### Darstellung von Personen und Institutionen, bi-partite

Färbt edges unterschiedlicher types verschieden ein & gib ihnen unterscheidbare Formen
```
V(g)[V(g)$type == 1]$shape <- "square"
V(g)[V(g)$type == 0]$shape <- "circle"
V(g)[V(g)$type == 0]$color <- "cornflowerblue"
V(g)[V(g)$type == 1]$color <- "aquamarine2"
V(g)[(V(g)$type == 1) & (institutiontype == 2)]$color <- "grey"
```

Färbt verschiedene relations unterschiedlich ein
```
E(g)[relation == 3]$color <- "green"
E(g)[relation == 2]$color <- "blue"
E(g)[relation == 1]$color <- orange"
E(g)[(relation == 1) & (format == 2)]$color <- "red"
```

### Namen verkürzen
```
options(max.print = 99999)
Namen <- V(g)$name
Namen
Namen[1:1195]
g
```
**Preise**
```
Namen[[60]] <- "Alt. Medienpreis"
Namen[[193]] <- "Axel-Springer-Preis"
Namen[[500]] <- "Dt. Radiopreis"
Namen[[501]] <- "Dt. Reporterpreis"
Namen[[528]] <- "djp"
Namen[[705]] <- "G. v. H.-Preis"
Namen[[820]] <- "Helmut-Schmidt-Preis"
Namen[[840]] <- "Herbert-Quandt-Preis"
Namen[[1032]] <- "Journ. d. Jahres"
Namen[[1196]] <- "Kurt-Tucholsky-Preis"
Namen[[1242]] <- "Leuchtturm"
Namen[[1263]] <- "Ludwig-Erhard-Preis"
Namen[[1695]] <- "Preis für Freiheit"
Namen[[1757]] <- "Rob. Geisendö. Preis"
Namen[[2144]] <- "Wächterpreis"
```
**Personen**
```
Namen[[1167]] <- "Klaus Meier"
```
**Medien**
```
Namen[[215]] <- "BR"
Namen[[496]] <- "DJS"
Namen[[497]] <- "dpa"
Namen[[588]] <- "epd"
Namen[[654]] <- "FAS"
Namen[[655]] <- "FAZ"
Namen[[706]] <- "G. v. H.-Schule"
Namen[[886]] <- "ifp"
Namen[[1117]] <- "Uni Eichstätt"
Namen[[1508]] <- "MDR"
Namen[[1548]] <- "NZZ"
Namen[[1595]] <- "NDR"
Namen[[1700]] <- "rbb"
Namen[[1795]] <- "SR"
Namen[[1949]] <- "SZ"
Namen[[1950]] <- "SZ Magazin"
Namen[[1954]] <- "SWR"
Namen[[1980]] <- "taz"
Namen[[2152]] <- "WDR"
Namen[[2190]] <- "ZEIT"
Namen[[2195]] <- "ZDF"
V(g)$name <- Namen
V(g)$label <- V(g)$name
```

### Das Gesamtnetzwerk

#### Gesamtes Netzwerk mit Indgree-Verteilung (Preise hervorgehoben)
```
V(g)[[1118]]$label <- "2" ## Katholischer Medienpreis
V(g)[[2144]]$label <- "1" ## Wächterpreis
V(g)[[1757]]$label <- "3" ## Robert-Geisendörfer-Preis
V(g)[[1695]]$label <- "4" ## Preis für Freiheit
V(g)[[705]]$label <- "5" ## G. v. H.-Preis
V(g)[[840]]$label <- "6" ## Herbert-Quandt-Preis
V(g)[[2001]]$label <- "8" ## Theodor-Wolff-Preis
V(g)[[1532]]$label <- "7" ## Nannenpreis
V(g)[[1624]]$label <- "9" ## Otto-Brenner-Preis


V(g)$label <- ifelse(V(g)$institutiontype == 1, V(g)$label, NA)

ind <- degree(g, mode="all", normalized=T)

l <- layout_with_fr(g)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(g,
     edge.arrow.size=0.02,
     vertex.size=ind*50,
     edge.color="grey",
     vertex.label.family="Helvetica",
     vertex.frame.color="white",
     vertex.label.cex=1.3,
     vertex.label.dist=0.5,
     vertex.label.degree=-0.5,
     layout = l*1.2,
     main="Das Gesamtnetzwerk",
     rescale=F,
     asp=0)
```

#### Outdegree-Verteilung im Gesamtnetzwerk (Personen)
```
V(g)[V(g)$type == 1]$shape <- "square"
V(g)[V(g)$type == 0]$shape <- "circle"

install.packages('RColorBrewer')
library('RColorBrewer')

outd <- degree(g, mode="out", normalized=T)
fine = 9
palette = colorRampPalette(c (rgb(0,1,1,0.5), rgb(0,0,0.7,1)))
graphCol = palette(fine)[as.numeric(cut(outd, breaks = fine))]

plot(g,
     vertex.color=graphCol,
     vertex.size=outd*800,
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label=NA,
     layout = layout_with_kk,
     main="Outdegree-Verteilung im Gesamtnetzwerk",
     asp=0,
     rescale=T)
```

#### Indegreeverteilung im Netzwerk mit nur Arbeitgebern  
(Preise ausgeklammert, um Degree-Verteilung der Medien zeigen zu können)  
```
av <- subgraph.edges(g, E(g)[relation == 2])
av

V(av)[V(av)$type == 1]$shape <- "square"
V(av)[V(av)$type == 0]$shape <- "circle"

ind_av <- degree(av, mode="in", normalized=T)
fine = 9
palette = colorRampPalette(c (rgb(0.4,1,0.2,0.5), rgb(0,0.4,0,1)))
graphCol = palette(fine)[as.numeric(cut(ind_av, breaks = fine))]

plot(av,
     vertex.color=graphCol,
     vertex.size=ind_av*200,
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label=NA,
     layout = layout_with_kk,
     main="Indegree-Verteilung im Mediennetzwerk",
     sub="(enthält ausschließlich Arbeitsbeziehungen)",
     asp=0,
     rescale=T)
```

#### Broker-Medien Visualisierung
```
av
betw <- betweenness(av, directed=F, normalized = T)
sort(betw)

V(av)$betw <- betw
V(av)$label <- V(av)$name
V(av)$label <- ifelse(V(av)$betw> 4.508710e-02, V(av)$label, NA)
V(av)$label <- ifelse(V(av)$institutiontype==2, V(av)$label, NA)

V(av)[type==1]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[type==0]$color <- "darkgrey"

l <- layout.kamada.kawai(av)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(av,
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.size=betw*50,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.dist=-0.6,
     vertex.label.cex=1.2,
     layout = l*1.1,
     main="Broker-Medien im Gesamtnetzwerk",
     asp=0,
     rescale=F)
```

#### Broker-Personen Visualisierung:
```
ohneav
betw <- betweenness(ohneav, directed=F, normalized = T)
sort(betw)

V(ohneav)$betw <- betw
V(ohneav)$label <- V(ohneav)$name

V(ohneav)$label <- ifelse(V(ohneav)$type==0, V(ohneav)$name, NA)
V(ohneav)$label <- ifelse(V(ohneav)$betw> 2.781636e-02, V(ohneav)$label, NA)

V(ohneav)[type==0]$color <- rgb(0, 0.7, 1, 1)
V(ohneav)[type==1]$color <- "darkgrey"

l <- layout.kamada.kawai(ohneav)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(ohneav,
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.size=betw*60,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.dist=0.6,
     vertex.label.cex=1.2,
     layout = l*1.2,
     main="Broker-Personen im Gesamtnetzwerk",
     asp=0,
     rescale=F)
```


### Netzwerk ohne Arbeitsverhältnisse
Netzwerk ohne Label visualisieren, zeigt Beziehungen:
```
ohneav
edge_attr(ohneav)

normalize_01 <- function(ohneav) (ohneav - min(ohneav)) / (max(ohneav) - min(ohneav)) + 0.25
V(ohneav)$size <- normalize_01(degree(ohneav)) *5

V(ohneav)[type==0]$color <- "cornflowerblue"

V(ohneav)$label <- V(ohneav)$name
V(ohneav)$label <- ifelse(V(ohneav)$institutiontype==1, V(ohneav)$label, NA) 

E(ohneav)[relation == 3]$color <- rgb(0, 1, 0, 0.3)
E(ohneav)[relation == 1]$color <- rgb(1, 0.7, 0, 0.5)
E(ohneav)[(relation == 1) & (format == 2)]$color <- rgb(1, 0, 0, 0.5)

plot(ohneav, edge.arrow.size=.02,
     vertex.frame.color="white",
     vertex.label=NA,
     main="Netzwerk ohne Arbeitsbeziehungen",
     layout = layout_with_kk,
     asp=0)
```
#### Netzwerkvisualisierung mit wichtigen Broker-Preisen:
```
betw <- betweenness(ohneav, directed=F)
options(max.print = 999999)
sort(betw)

V(ohneav)$betw <- betw
V(ohneav)$betw
V(ohneav)$label <- V(ohneav)$name
V(ohneav)$label <- ifelse(V(ohneav)$betw>106203.42359, V(ohneav)$name, NA) 

V(ohneav)[type==0]$color="grey"

normalize_01 <- function(ohneav) (ohneav - min(ohneav)) / (max(ohneav) - min(ohneav)) + 0.25
V(ohneav)$size <- normalize_01(degree(ohneav)) * 7

plot(ohneav, edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.color="black",
     vertex.label.cex=1.4,
     main="Die Broker im Netzwerk ohne Arbeitsbeziehungen",
     layout = layout_with_kk,
     asp=0)
```


### Das konservative Netzwerk
```
konservativ

V(konservativ)$label <- ifelse(V(konservativ)$institutiontype == 1, V(konservativ)$name, NA)
V(konservativ)[type == 0]$color <- "lightgrey"

Namen <- V(konservativ)$name
Namen
V(konservativ)[[33]]$color <- "red"
V(konservativ)[[149]]$color <- "red"
V(konservativ)[[150]]$color <- "red"
V(konservativ)[[34]]$color <- "red"
V(konservativ)[[40]]$color <- "orange"
V(konservativ)[[151]]$color <- "orange"
V(konservativ)[[20]]$color <- "orange"

V(konservativ)[[33]]$label <- V(konservativ)[[33]]$name
V(konservativ)[[149]]$label <- V(konservativ)[[149]]$name
V(konservativ)[[150]]$label <- V(konservativ)[[150]]$name
V(konservativ)[[34]]$label <- V(konservativ)[[34]]$name
V(konservativ)[[40]]$label <- V(konservativ)[[40]]$name
V(konservativ)[[151]]$label <- V(konservativ)[[151]]$name
V(konservativ)[[20]]$label <- V(konservativ)[[20]]$name
V(konservativ)[[52]]$label <- V(konservativ)[[52]]$name
V(konservativ)[[78]]$label <- V(konservativ)[[78]]$name
V(konservativ)[[137]]$label <- V(konservativ)[[137]]$name

E(konservativ)$curved=0.2
E(konservativ)[relation == 3]$color <- rgb(0, 1, 0, 0.3)
E(konservativ)[relation == 2]$color <- rgb(0, 0, 1, 0.3)
E(konservativ)[relation == 1]$color <- rgb(1, 0.7, 0, 0.3)
E(konservativ)[(relation == 1) & (format == 2)]$color <- rgb(1, 0, 0, 0.3)

l <- layout.fruchterman.reingold(konservativ)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(konservativ,
     edge.arrow.size=.02,
     vertex.size=4,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.dist=0.6,
     vertex.label.cex=1.7,
     layout = l*1.2,
     asp=0,
     rescale=F)
```


### Das Elitenetzwerk

#### Gesamtes Netzwerk (einfache Visualisierung)
```
V(g4)$label <- V(g4)$name

E(g4)$curved=0.2
E(g4)[relation == 3]$color <- rgb(0, 1, 0, 0.3)
E(g4)[relation == 2]$color <- rgb(0, 0, 1, 0.3)
E(g4)[relation == 1]$color <- rgb(1, 0.7, 0, 0.3)
E(g4)[(relation == 1) & (format == 2)]$color <- rgb(1, 0, 0, 0.3)

outd <- degree(g4, mode="all", normalized=T)

plot(g4,
     edge.arrow.size=.02,
     vertex.size=outd*10,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.dist=0.4,
     vertex.label.cex=1.2,
     main="Die Elite in der Elite",
     asp=0,
     layout=layout_with_dh,
     rescale=T)
```

#### Dendrogramm des Elitenetzwerks
1. Selektiere Namen aus Elitenetzwerk:  
```
Namen <- V(g4)$name
Namen
g4
```
3. Überführe geänderte Namen wieder in Vertex-Attribute:  
```
V(g4)$name <- Namen
```
4. Führe Clusteranalyse durch und wird Dendrogramm aus:  
```
clg4 <- cluster_walktrap(g4)
plot_dendrogram(clg4)
```


### Das Netzwerk der "Doppelrollen":

#### Die Broker-Preise im Netzwerk:
```
betw <- betweenness(PuJ, directed=F, normalized=T)
sort(betw)

V(PuJ)$betw <- betw
V(PuJ)$betw

V(PuJ)$label <- V(PuJ)$name
V(PuJ)$label <- ifelse(V(PuJ)$betw>6.924608e-02, V(PuJ)$label, NA) 

V(PuJ)[type==1]$color <- rgb(0, 1, 1, 0.6)
V(PuJ)[type==0]$color <- "darkgrey"

l <- layout.kamada.kawai(PuJ)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(PuJ, 
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.size=betw*30,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.dist=0.5,
     vertex.label.cex=1.4,
     main="Die Broker-Preise im Netzwerk der Doppelakteure",
     layout = l*1.3,
     asp=0)
```

#### Die Broker-Personen im Netzwerk:
```
betw <- betweenness(PuJ, directed=F, normalized=T)
sort(betw)

V(PuJ)$betw <- betw
V(PuJ)$betw

V(PuJ)[[580]]$name <- "2"  ## Nr. 2 = Frederik Obermaier
V(PuJ)[[193]]$name <- "1"  ## Nr. 1 = Bastian Obermayer

V(PuJ)$label <- V(PuJ)$name
V(PuJ)$label <- ifelse(V(PuJ)$betw>2.377628e-02, V(PuJ)$label, NA) 
V(PuJ)$label <- ifelse(V(PuJ)$type==0, V(PuJ)$label, NA)

V(PuJ)$color <- ifelse(V(PuJ)$betw>2.377628e-02, rgb(0, 0.8, 1, 1), "grey")
V(PuJ)[type==1]$color <- "darkgrey"

normalize_01 <- function(PuJ) (PuJ - min(PuJ)) / (max(PuJ) - min(PuJ)) + 0.25
V(PuJ)$size <- normalize_01(degree(PuJ)) * 4

V(PuJ)[[580]]$size <- 5
V(PuJ)[[193]]$size <- 5
V(PuJ)[[401]]$size <- 5
V(PuJ)[[372]]$size <- 5
V(PuJ)[[477]]$size <- 5
V(PuJ)[[200]]$size <- 5
V(PuJ)[[1080]]$size <- 5
V(PuJ)[[1299]]$size <- 5
V(PuJ)[[1486]]$size <- 5
V(PuJ)[[1106]]$size <- 5


l <- layout.fruchterman.reingold(PuJ)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(PuJ, 
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.dist=0.7,
     vertex.label.cex=1.4,
     vertex.lebel.deg=0.5,
     #main="Die Broker-Personen im Netzwerk der Doppelakteure",
     layout = l*1.2,
     rescale=F,
     asp=0)
```


### Die Dominanz der ARD
```
av
ind_av <- degree(av, mode="in", normalized=T)

V(av)[V(av)$type == 1]$shape <- "square"
V(av)[V(av)$type == 0]$shape <- "circle"

V(av)$label <- NA
V(av)$color= "darkgrey"

V(av)[[140]]$label <- V(av)[[140]]$name
V(av)[[140]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[172]]$label <- V(av)[[172]]$name
V(av)[[172]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[408]]$label <- V(av)[[408]]$name
V(av)[[408]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[410]]$label <- V(av)[[410]]$name
V(av)[[410]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[564]]$label <- V(av)[[564]]$name
V(av)[[564]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[703]]$name <- "HR"
V(av)[[703]]$label <- V(av)[[703]]$name
V(av)[[703]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[1260]]$label <- V(av)[[1260]]$name
V(av)[[1260]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[1330]]$label <- V(av)[[1330]]$name
V(av)[[1330]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[1422]]$label <- V(av)[[1422]]$name
V(av)[[1422]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[1423]]$label <- V(av)[[1423]]$name
V(av)[[1423]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[1637]]$label <- V(av)[[1637]]$name
V(av)[[1637]]$color <- rgb(0.8, 0, 0, 0.7)
V(av)[[1799]]$label <- V(av)[[1799]]$name
V(av)[[1799]]$color <- rgb(0.8, 0, 0, 0.7)

l <- layout.kamada.kawai(av)
l <- layout.norm(l, ymin=-1, ymax=1, xmin=-1, xmax=1)

plot(av,
     vertex.size=ind_av*80,
     edge.arrow.size=.02,
     edge.color="lightgrey",
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.color="black",
     vertex.label.size=4,
     vertex.label.dist=-0.5,
     layout = l*1.1,
     main="Dominanz des ARD-Netzwerks",
     asp=0,
     rescale=F)
```


### Ideensammlung

bestimmte Hubs visualisieren:
```
hs <- hub_score(g4, weights=NA)$vector
plot(g4, vertex_size=hs*50....)
```
