# BA (kommentierter Code)
## Legende
*g* = Gesamtnetzwerk  
*g2* = bereinigtes Netzwerk, alle gelöscht mit degree < 3  
*g3* = Elite in der Elite, alle gelöscht mit degree < 20  
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
id: alternativermedienpreis
name: "Alternativer Medienpreis"
```
Axel-Springer-Preis:  
```
id: axelspringerpreis 
name: "Axel-Springer-Preis für junge Journalisten"
```
Bremer Fernsehpreis:  
```
id: bremerfernsehpreis
name: "Bremer Fernsehpreis"
```
Deutscher Journalistenpreis Wirtschaft:  
```
id: djp  
name: "Deutscher Journalistenpreis Wirtschaft | Börse | Finanzen (djp)"
```
Deutscher Radiopreis:  
```
id: deutscherradiopreis
name: "Deutscher Radiopreis"
```
Deutscher Reporterpreis:  
```
id: deutscherreporterpreis
name: "Deutscher Reporterpreis"
```
Ernst-Schneider-Preis:  
```
id: ernstschneiderpreis 
name: "Ernst-Schneider-Preis"
```
Georg v. Holtzbrinck-Preis:  
```
id: gvhpreis
name: "Georg von Holtzbrinck-Preis für Wirtschaftspublizistik bzw. Ferdinand Simoneit-Nachwuchspreis"
```
Grimme Online Award:  
```
id: grimmeonlineaward
name: "Grimme Online Award"
```
Helmut-Schmidt-Preis:  
```
id: helmutschmidtpreis
name: "Helmut-Schmidt-Journalistenpreis"
```
Herbert-Quandt-Preis:  
```
id: herbertquandtpreis
name: "Herbert Quandt Medienpreis"
```
Journalist des Jahres:  
```
id: journalistdesjahres
name: "Journalist des Jahres"
```
Katholischer Medienpreis:  
```
id: katholischermedienpreis
name: "Katholischer Medienpreis"
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
id: ludwigboernepreis
name: "Ludwig-Börne-Preis"
```
Ludwig-Erhard-Preis:  
```
id: ludwigerhardpreis
name: "Ludwig-Erhard-Preis für Wirtschaftspublizistik"
```
Nannenpreis:  
```
id: nannenpreis
name: "Nannen Preis"
```
Robert-Geisendörfer-Preis: 
```
id: robertgeisendoerferpreis
name: "Robert Geisendörfer Preis"
```
Theodor-Wolff-Preis:  
```
id: theodorwolffpreis
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
```

## Grundsätzliche Darstellung
```
options(max.print = 99999)
E(g)$curved=.2 
E(g)$vertex.label.family="Helvetica"
```

## Darstellung von Personen und Institutionen, bi-partite

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
E(g)[relation == 2]$color <- "orange"
E(g)[relation == 1]$color <- "blue"
E(g)[(relation == 1) & (format == 2)]$color <- "red"

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
centr_betw(g, directed=TRUE)
betw <- betweenness(g, directed=TRUE)
betw <- betweenness(g, directed = TRUE)
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

1. Entfernt alle Nodes, die bei "workinmedia" _nicht_ NA haben (trifft auf alle Nodes außer Jurymitgliedern zu)
```
preistraeger1 <- delete_vertices(g, V(g)[!is.na(workinmedia)])
preistraeger1
```
2. Entfernt aus oberem Netzwerk alle Jury-Beziehungen ("relation" = 3)
```
preistraeger2 <- delete_edges (preistraeger1, E(preistraeger1)[relation==3])
preistraeger2
```
3. Entfernt aus oberem Netzwerk alle Institutionen, die keine Verbindung zu Preisträgern haben (Degree-Wert = 0)
```
preistraegerfinal <- delete_vertices (preistraeger2, V(preistraeger2)[degree(preistraeger2, mode="all")=="0"])
preistraegerfinal
```
4. Wirft das Preisträgernetzwerk aus
```
plot (preistraegerfinal, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Berechnet Out-Degree von Preisträgernetzwerk 
```
outdpreistraeger <- degree(preistraegerfinal, mode="out")
outdpreistraeger
sort(outdpreistraeger)
```
Berechnet In-Degree von Preisträgernetzwerk 
```
indpreistraeger <- degree(preistraegerfinal, mode="in")
indpreistraeger
sort(indpreistraeger)
```


## JURY-NETZWERK

1. Entfernt alle Preisträger-Beziehungen aus edgelist
```
jury1 <- delete_edges (g, E(g)[relation==1])
jury1
```
*V(jury1)$workinmedia=="1"
V(jury1)$workinmedia=="2"*

2. Entfernt aus oberem Netzwerk alle Personen (type = 0), die bei "workinmedia" = NA haben (also alle Preisträger)
```
jury2 <- delete_vertices(jury1, V(jury1)[(is.na(workinmedia)) & (type == 0)])
jury2
```
3. Entfernt aus oberem Netzwerk alle Institutionen, die keine Verbindung zu Jurymitgliedern haben (Degree-Wert = 0)
```
juryfinal <- delete_vertices (jury2, V(jury2)[degree(jury2, mode="all")=="0"])
juryfinal
```
4. Wirft das Jurynetzwerk aus
```
plot (juryfinal, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Berechnet Out-Degree von Jurynetzwerk
```
outdjury <- degree(juryfinal, mode="out")
outdjury
sort(outdjury)
```
Berechnet In-Degree von Jurynetzwerk
```
indjury <- degree(juryfinal, mode="in")
indjury
sort(indjury)
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
```
personen_x <- V(x1[[1]])$type == 0
sum(personen_x, na.rm = TRUE)
```
davon Jurymitglieder (einfach gezählt):
```
jury_x <- V(personen_x)$type == 0
sum(personen_x, na.rm = TRUE)
```
davon Preisträger (einfach gezählt):
```
personen_x <- V(x[[1]])$type == 0
sum(personen_x, na.rm = TRUE)
```

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

Anzahl **Gruppenpreise**
```
gruppenpreise_x <- (E(x1[[1]])$format == 2)
sum(gruppenpreise_x, na.rm = TRUE)
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
Lösche Jurymitglieder
```
x2_pt <- delete_vertices(x2, V(x2)[!is.na(workinmedia)])
x2_pt
```
Anzahl **Arbeitgeberbeziehungen unter Preisträgern**
```
ag_x_pt <- (E(x2_pt)$relation == 2)
sum(ag_x_pt, na.rm = TRUE)
```
Nach Indegree sortieren (**häufigster Arbeitgeber unter Preisträgern**):
```
ind_x_pt <- degree(x2_pt, mode="in")
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

**Gruppenpreisnetzwerk** erstellen
```
gruppenpreise1 <- delete_edges(preistraegerfinal, E(preistraegerfinal)[which(format == 1)])
gruppenpreise2 <- delete_vertices (gruppenpreise1, V(gruppenpreise1)[(degree(gruppenpreise1, mode="out")=="1") & (type == 0)])
gruppenpreise <- delete_vertices (gruppenpreise2, V(gruppenpreise2)[degree(gruppenpreise2, mode="all")=="0"])
```
Indegree von Gruppenpreisen  
(Frage: Welches Unternehmen hat die meisten Gruppenpreisträger & welcher Preis vergibt die meisten Gruppenpreise?
```
indgruppenpreise <- degree(gruppenpreise, mode="in")
sort(indgruppenpreise)
```
**Einzelpreisnetzwerk** erstellen
```
einzelpreise1 <- delete_edges(preistraegerfinal, E(preistraegerfinal)[which(format == 2)])
einzelpreise2 <- delete_vertices (einzelpreise1, V(einzelpreise1)[(degree(einzelpreise1, mode="out")=="1") & (type == 0)])
einzelpreise <- delete_vertices (einzelpreise2, V(einzelpreise2)[degree(einzelpreise2, mode="all")=="0"])
```
Indegree von Einzelpreisen  
(Frage: Welches Unternehmen hat die meisten Einzelpreisträger & welcher Preis vergibt die meisten Einzelpreise?
```
indeinzelpreise <- degree(einzelpreise, mode="in")
sort(indeinzelpreise)
```

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
```
2. Von Gesamtnetzwerk doppelt subtrahieren (= addieren):
```
ard <- g - (g - swr[[1]] - wdr[[1]] - radiobremen[[1]] - br[[1]] - hr[[1]] - mdr[[1]] - ndr[[1]] - sr[[1]] - funk[[1]] - rbb[[1]] - ard1[[1]] - dlr[[1]]) 
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
**Theodor-Wolff-Preis**
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
juryfinal20xx <- subgraph.edges(juryfinal, E(juryfinal)[year == Jahr])
plot(juryfinal20xx,
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
frauenjury20xx <- V(juryfinal20xx)$sex == 2
sum (frauenjury20xx, na.rm = TRUE)
maennerjury20xx <- V(juryfinal20xx)$sex == 1
sum (maennerjury20xx, na.rm = TRUE)

```
**Preisträger** nach Jahren zählen
```
preistraeger20xx <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == Jahr])
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

**Dominanz einzelner Personen** nach Jahren
Frage: Wer hat die meisten Preise gewonnen?
(Entspricht: preisträger20xx ohne Arbeitsverhältnisse)
```
nur_preistraeger20xx <- subgraph.edges(year20xx, E(year20xx)[relation == 1]) 
plot(nur_preistraeger20xx,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Größter Outdegree Preisträger
```
outd_nur_preistraeger20xx <- degree(nur_preistraeger20xx, mode="out")
sort(outd_nur_preistraeger20xx)
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
Auswertung der Netzwerke:
```
preistraeger_relations_unternehmen <- E(unternehmen[[1]])$relation == 1
sum(preistraeger_relations_unternehmen, na.rm = TRUE)

jury_relations_unternehmen <- E(unternehmen[[1]])$relation == 3
sum(jury_relations_unternehmen, na.rm = TRUE)

unternehmen_personen <- induced_subgraph(unternehmen[[1]], V(unternehmen[[1]])[which (type == 0)])
unternehmen_personen

unternehmen_preistraeger <- induced_subgraph(unternehmen_personen, V(unternehmen_personen)[which (is.na(workinmedia))])
unternehmen_preistraeger

unternehmen_juroren <- induced_subgraph(unternehmen_personen, V(unternehmen_personen)[which (!is.na(workinmedia))])
unternehmen_juroren

gruppenpreise_unternehmen <- (E(unternehmen[[1]])$format == 2)
sum(gruppenpreise_unternehmen, na.rm = TRUE)

unternehmen_preise <- induced_subgraph(unternehmen[[1]], V(unternehmen[[1]])[which (institutiontype == 1)])
unternehmen_preise
```


## Ressorts

#### Gesamt

Ressort auswählen
```
taskx <- E(g)$task == 1 (Zahl von Ressort einfügen)
taskx
```
Was war häufigstes Ressort?
```
sum(taskx, na.rm = TRUE)
```

#### unter Preisträgern
Dafür: Aus g alle Jurymitglieder löschen
```
task_pt <- delete_vertices(g, V(g)[(type == 0) & (!is.na(workinmedia))])
task_pt
```
Dann auswerten:
```
taskx_pt <- E(task_pt)$task == 15
sum(taskx_pt, na.rm = TRUE)
```
#### unter Jury
Alle Preisträger löschen
```
task_jury <- delete_vertices(g, V(g)[(type == 0) & (is.na(workinmedia))])
task_jury
```
Dann auswerten:
```
taskx_jury <- E(task_jury)$task == 6
sum(taskx_jury, na.rm = TRUE)
```
## SONSTIGES GESAMTNETZWERK

#### Personen, die bei einem Preis Preisträger sind und in Jury sitzen:

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

## Visualisierungen

### Ideensammlung
nach Degree-Größen visualisieren:  
```
deg <- degree(net, mode="all")
plot(net, vertex.size=deg*3)
```
bestimmte Hubs visualisieren:
```
hs <- hub_score(g4, weights=NA)$vector
plot(g4, vertex_size=hs*50....)
```

#### Die konservativen Preise
#### Die linkeren Preise
(Preisnetzwerke zusammenfassen oder nebneinander plotten, wenn erstes nicht geht)

### Elite in Elite-Netzwerk
(noch nicht schön!)
```
plot(g3be,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```

### Ego-Netzwerke

Auswahl des Objekts (Person, Unternehmen, Preis)
```
Objekt <- "Bastian Obermayer"
```
Erzeuge Ego-Netzwerk 1. Grades
```
ego <- subgraph <- make_ego_graph(g, order=1, Objekt)
ego
```
Edges gewichten
```
E(ego[[1]])$width <- count.multiple(ego[[1]])

plot(ego[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

## ab hier: Spielwiese 

# Ego-Netzwerk Obermayer
```
edge_attr(g, "relation", index = E(g))
h <- simplify(g, edge.attr.comb = "sum"))
h
obermayer <- subgraph <- make_ego_graph(h, order=1, c("Bastian Obermayer"))
obermayer
plot(obermayer[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
