# BA (kommentierter Code)
## Legende
*g = Gesamtnetzwerk*


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
```

Färbt verschiedene relations unterschiedlich ein
```
E(g)[relation == 3]$color <- "green"
E(g)[relation == 2]$color <- "orange"
E(g)[relation == 1]$color <- "blue"

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

Zeigt Attribute an, die "institutiontype" = 1 haben (= Medienunternehmen)
```
vertex_attr(g, ("institutiontype" == 1), index=V(g))
```
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
preistraeger <- induced_subgraph(personen, V(personen)[which (is.na(workinmedia))])
preistraeger
```
Erzeugt Teilnetzwerk mit ausschließlich Juroren (Frage: Wie viele Juroren im Gesamtnetzwerk?)
```
juroren <- induced_subgraph(personen, V(personen)[which (!is.na(workinmedia))])
juroren
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
verleihung1 <- delete_edges(g, E(g)[relation == 3])
verleihung <- delete_edges(verleihung1, E(verleihung1)[relation == 2])
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

**andere Netzwerkmaße**
```
edge_density(g) # Dichte
centr_betw(g, directed=TRUE) # Betweenness-Zentralität
betw <- betweenness(g, directed=TRUE) # Betweenness
bet <- betweenness(g, directed = TRUE)

```
## Preisträger-Netzwerk

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
## Jury-Netzwerk

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
## Einzelne Preisnetzwerke

Erstellt Teilnetzwerke der einzelnen Preise

**Alternativer Medienpreis**
Gesamt
```
alternativermedienpreis <- subgraph <- make_ego_graph(g, order=1, c("Alternativer Medienpreis"))
alternativermedienpreis
plot(alternativermedienpreis[[1]])
```
Jury
```
jury_alternativermedienpreis <- E(alternativermedienpreis[[1]])$relation == 3
sum(jury_alternativermedienpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_alternativermedienpreis <- E(alternativermedienpreis[[1]])$relation == 1
sum(preistraeger_alternativermedienpreis, na.rm = TRUE)
```
**Axel-Springer-Preis**
Gesamt
```
axelspringerpreis <- subgraph <- make_ego_graph(g, order=1, c("Axel-Springer-Preis für junge Journalisten"))
axelspringerpreis
plot(axelspringerpreis[[1]])
```
Jury
```
jury_axelspringerpreis <- E(axelspringerpreis[[1]])$relation == 3
sum(jury_axelspringerpreis, na.rm = TRUE)

```
Preisträger
```
preistraeger_axelspringerpreis <- E(axelspringerpreis[[1]])$relation == 1
sum(preistraeger_axelspringerpreis, na.rm = TRUE)
```

**Bremer Fernsehpreis**
Gesamt
```
bremerfernsehpreis <- subgraph <- make_ego_graph(g, order=1, c("Bremer Fernsehpreis"))
bremerfernsehpreis
plot(bremerfernsehpreis[[1]])
```
Jury
```
jury_bremerfernsehpreis <- E(bremerfernsehpreis[[1]])$relation == 3
sum(jury_bremerfernsehpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_bremerfernsehpreis <- E(bremerfernsehpreis[[1]])$relation == 1
sum(preistraeger_bremerfernsehpreis, na.rm = TRUE)
```

**djp**
Gesamt
```
djp <- subgraph <- make_ego_graph(g, order=1, c("Deutscher Journalistenpreis Wirtschaft | Börse | Finanzen (djp)"))
djp
plot(djp[[1]])
```
Jury
```
jury_djp <- E(djp[[1]])$relation == 3
sum(jury_djp, na.rm = TRUE)
```
Preisträger
```
preistraeger_djp <- E(djp[[1]])$relation == 1
sum(preistraeger_djp, na.rm = TRUE)
```

**Deutscher Radiopreis**
Gesamt
```
deutscherradiopreis <- subgraph <- make_ego_graph(g, order=1, c("Deutscher Radiopreis"))
deutscherradiopreis
plot(deutscherradiopreis[[1]])
```
Jury
```
jury_deutscherradiopreis <- E(deutscherradiopreis[[1]])$relation == 3
sum(jury_deutscherradiopreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_deutscherradiopreis <- E(deutscherradiopreis[[1]])$relation == 1
sum(preistraeger_deutscherradiopreis, na.rm = TRUE)
```

**Deutscher Reporterpreis**
Gesamt
```
deutscherreporterpreis <- subgraph <- make_ego_graph(g, order=1, c("Deutscher Reporterpreis"))
deutscherreporterpreis
plot(deutscherreporterpreis[[1]])
```
Jury
```
jury_deutscherreporterpreis <- E(deutscherreporterpreis[[1]])$relation == 3
sum(jury_deutscherreporterpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_deutscherreporterpreis <- E(deutscherreporterpreis[[1]])$relation == 1
sum(preistraeger_deutscherreporterpreis, na.rm = TRUE)
```

**Ernst-Schneider-Preis**
Gesamt
```
ernstschneiderpreis <- subgraph <- make_ego_graph(g, order=1, c("Ernst-Schneider-Preis"))
ernstschneiderpreis
plot(ernstschneiderpreis[[1]])
```
Jury
```
jury_ernstschneiderpreis <- E(ernstschneiderpreis[[1]])$relation == 3
sum(jury_ernstschneiderpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_ernstschneiderpreis <- E(ernstschneiderpreis[[1]])$relation == 1
sum(preistraeger_ernstschneiderpreis, na.rm = TRUE)
```

**Georg von Holtzbrinck-Preis**
Gesamt
```
gvhpreis <- subgraph <- make_ego_graph(g, order=1, c("Georg von Holtzbrinck-Preis für Wirtschaftspublizistik bzw. Ferdinand Simoneit-Nachwuchspreis"))
gvhpreis
plot(gvhpreis[[1]])
```
Jury
```
jury_gvhpreis <- E(gvhpreis[[1]])$relation == 3
sum(jury_gvhpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_gvhpreis <- E(gvhpreis[[1]])$relation == 1
sum(preistraeger_gvhpreis, na.rm = TRUE)
```

**Grimme Online Award**
Gesamt
```
grimmeonlineaward <- subgraph <- make_ego_graph(g, order=1, c("Grimme Online Award"))
grimmeonlineaward
plot(grimmeonlineaward[[1]])
```
Jury
```
jury_grimmeonlineaward <- E(grimmeonlineaward[[1]])$relation == 3
sum(jury_grimmeonlineaward, na.rm = TRUE)
```
Preisträger
```
preistraeger_grimmeonlineaward <- E(grimmeonlineaward[[1]])$relation == 1
sum(preistraeger_grimmeonlineaward, na.rm = TRUE)
```

**Helmut-Schmidt-Preis**
Gesamt
```
helmutschmidtpreis <- subgraph <- make_ego_graph(g, order=1, c("Helmut-Schmidt-Journalistenpreis"))
helmutschmidtpreis
plot(helmutschmidtpreis[[1]])
```
Jury
```
jury_helmutschmidtpreis <- E(helmutschmidtpreis[[1]])$relation == 3
sum(jury_helmutschmidtpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_helmutschmidtpreis <- E(helmutschmidtpreis[[1]])$relation == 1
sum(preistraeger_helmutschmidtpreis, na.rm = TRUE)
```

**Herbert-Quandt-Preis**
Gesamt
```
herbertquandtpreis <- subgraph <- make_ego_graph(g, order=1, c("Herbert Quandt Medienpreis"))
herbertquandtpreis
plot(herbertquandtpreis[[1]])
```
Jury
```
jury_herbertquandtpreis <- E(herbertquandtpreis[[1]])$relation == 3
sum(jury_herbertquandtpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_herbertquandtpreis <- E(herbertquandtpreis[[1]])$relation == 1
sum(preistraeger_herbertquandtpreis, na.rm = TRUE)
```

**Journalist des Jahres**
Gesamt
```
journalistdesjahres <- subgraph <- make_ego_graph(g, order=1, c("Journalist des Jahres"))
journalistdesjahres
plot(journalistdesjahres[[1]])
```
Jury
```
jury_journalistdesjahres <- E(journalistdesjahres[[1]])$relation == 3
sum(jury_journalistdesjahres, na.rm = TRUE)
```
Preisträger
```
preistraeger_journalistdesjahres <- E(journalistdesjahres[[1]])$relation == 1
sum(preistraeger_journalistdesjahres, na.rm = TRUE)
```

**Katholischer Medienpreis**
Gesamt
```
katholischermedienpreis <- subgraph <- make_ego_graph(g, order=1, c("Katholischer Medienpreis"))
katholischermedienpreis
plot(katholischermedienpreis[[1]])
```
Jury
```
jury_katholischermedienpreis <- E(katholischermedienpreis[[1]])$relation == 3
sum(jury_katholischermedienpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_katholischermedienpreis <- E(katholischermedienpreis[[1]])$relation == 1
sum(preistraeger_katholischermedienpreis, na.rm = TRUE)
```

**Kurt-Tucholsky-Preis**
Gesamt
```
kurttucholskypreis <- subgraph <- make_ego_graph(g, order=1, c("Kurt-Tucholsky-Preis für literarische Publizistik"))
kurttucholskypreis
plot(kurttucholskypreis[[1]])
```
Jury
```
jury_kurttucholskypreis <- E(kurttucholskypreis[[1]])$relation == 3
sum(jury_kurttucholskypreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_kurttucholskypreis <- E(kurttucholskypreis[[1]])$relation == 1
sum(preistraeger_kurttucholskypreis, na.rm = TRUE)
```

**Leuchtturm**
Gesamt
```
leuchtturm <- subgraph <- make_ego_graph(g, order=1, c("Leuchtturm für besondere publizistische Leistungen"))
leuchtturm
plot(leuchtturm[[1]])
```
Jury
```
jury_leuchtturm <- E(leuchtturm[[1]])$relation == 3
sum(jury_leuchtturm, na.rm = TRUE)
```
Preisträger
```
preistraeger_leuchtturm <- E(leuchtturm[[1]])$relation == 1
sum(preistraeger_leuchtturm, na.rm = TRUE)
```

**Ludwig-Börne-Preis**
Gesamt
```
ludwigboernepreis <- subgraph <- make_ego_graph(g, order=1, c("Ludwig-Börne-Preis"))
ludwigboernepreis
plot(ludwigboernepreis[[1]])
```
Jury
```
jury_ludwigboernepreis <- E(ludwigboernepreis[[1]])$relation == 3
sum(jury_ludwigboernepreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_ludwigboernepreis <- E(ludwigboernepreis[[1]])$relation == 1
sum(preistraeger_ludwigboernepreis, na.rm = TRUE)
```

**Ludwig-Erhard-Preis**
Gesamt
```
ludwigerhardpreis <- subgraph <- make_ego_graph(g, order=1, c("Ludwig-Erhard-Preis für Wirtschaftspublizistik"))
ludwigerhardpreis
plot(ludwigerhardpreis[[1]])
```
Jury
```
jury_ludwigerhardpreis <- E(ludwigerhardpreis[[1]])$relation == 3
sum(jury_ludwigerhardpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_ludwigerhardpreis <- E(ludwigerhardpreis[[1]])$relation == 1
sum(preistraeger_ludwigerhardpreis, na.rm = TRUE)
```

**Nannen-Preis**
Gesamt
```
nannenpreis <- subgraph <- make_ego_graph(g, order=1, c("Nannen Preis"))
nannenpreis
plot(nannenpreis[[1]])
```
Jury
```
jury_nannenpreis <- E(nannenpreis[[1]])$relation == 3
sum(jury_nannenpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_nannenpreis <- E(nannenpreis[[1]])$relation == 1
sum(preistraeger_nannenpreis, na.rm = TRUE)
```

**Robert-Geisendörfer-Preis**
Gesamt
```
robertgeisendoerferpreis <- subgraph <- make_ego_graph(g, order=1, c("Robert Geisendörfer Preis"))
robertgeisendoerferpreis
plot(robertgeisendoerferpreis[[1]])
```
Jury
```
jury_robertgeisendoerferpreis <- E(robertgeisendoerferpreis[[1]])$relation == 3
sum(jury_robertgeisendoerferpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_robertgeisendoerferpreis <- E(robertgeisendoerferpreis[[1]])$relation == 1
sum(preistraeger_robertgeisendoerferpreis, na.rm = TRUE)
```

**Theodor-Wolff-Preis**
Gesamt
```
theodorwolffpreis <- subgraph <- make_ego_graph(g, order=1, c("Theodor-Wolff-Preis"))
theodorwolffpreis
plot(theodorwolffpreis[[1]])
```
Jury
```
jury_theodorwolffpreis <- E(theodorwolffpreis[[1]])$relation == 3
sum(jury_theodorwolffpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_theodorwolffpreis <- E(theodorwolffpreis[[1]])$relation == 1
sum(preistraeger_theodorwolffpreis, na.rm = TRUE)
```

**Wächterpreis**
Gesamt
```
waechterpreis <- subgraph <- make_ego_graph(g, order=1, c("Wächterpreis der Tagespresse"))
waechterpreis
plot(waechterpreis[[1]])
```
Jury
```
jury_waechterpreis <- E(waechterpreis[[1]])$relation == 3
sum(jury_waechterpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_waechterpreis <- E(waechterpreis[[1]])$relation == 1
sum(preistraeger_waechterpreis, na.rm = TRUE)
```
## Auswertung nach Jahren

### Gesamt

#### 2019
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2019
```
year2019 <- subgraph.edges(g, E(g)[year == "2019"]) 
year2019
plot (year2019)
```

#### 2018
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2018
```
year2018 <- subgraph.edges(g, E(g)[year == "2018"]) 
year2018
plot (year2018)
```

#### 2017
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2017
```
year2017 <- subgraph.edges(g, E(g)[year == "2017"]) 
year2017
plot (year2017)
```

#### 2016
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2016
```
year2016 <- subgraph.edges(g, E(g)[year == "2016"]) 
year2016
plot (year2016)
```

#### 2015
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2015
```
year2015 <- subgraph.edges(g, E(g)[year == "2015"]) 
year2015
plot (year2015)
```

### Gruppenpreise
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2015 (s.o.), bei denen ein Gruppenpreis verliehen wurde
```
gruppenpreise2015 <- E(year2015)$format == 2
gruppenpreise2015
```
Summiert alle Gruppenpreisverleihungen, die es im Jahr 2015 gab
```
sum(gruppenpreise2015, na.rm = TRUE)
```
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2016 (s.o.), bei denen ein Gruppenpreis verliehen wurde
```
gruppenpreise2016 <- E(year2016)$format == 2
gruppenpreise2016
```
Summiert alle Gruppenpreisverleihungen, die es im Jahr 2016 gab
```
sum(gruppenpreise2016, na.rm = TRUE)
```
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2017 (s.o.), bei denen ein Gruppenpreis verliehen wurde
```
gruppenpreise2017 <- E(year2017)$format == 2
gruppenpreise2017
```
Summiert alle Gruppenpreisverleihungen, die es im Jahr 2017 gab
```
sum(gruppenpreise2017, na.rm = TRUE)
```
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2018 (s.o.), bei denen ein Gruppenpreis verliehen wurde
```
gruppenpreise2018 <- E(year2018)$format == 2
gruppenpreise2018
```
Summiert alle Gruppenpreisverleihungen, die es im Jahr 2018 gab
```
sum(gruppenpreise2018, na.rm = TRUE)
```

Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2019 (s.o.), bei denen ein Gruppenpreis verliehen wurde
```
gruppenpreise2019 <- E(year2019)$format == 2
gruppenpreise2019
```
Summiert alle Gruppenpreisverleihungen, die es im Jahr 2019 gab
```
sum(gruppenpreise2019, na.rm = TRUE)
```

### Männer/Frauen
Teilnetzwerk, nur männliche Nodes im Jurynetzwerk (und ihre Summe)
```
maennerjury <- V(juryfinal)$sex == 1
sum (maennerjury, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Jurynetzwerk (und ihre Summe)
```
frauenjury <- V(juryfinal)$sex == 2
sum (frauenjury, na.rm = TRUE)
```

#### ...gesamt
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2015 (und ihre Summe):
```
maenner2015 <- V(year2015)$sex == 1
sum (maenner2015, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2016 (und ihre Summe):
```
maenner2016 <- V(year2016)$sex == 1
sum (maenner2016, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2017 (und ihre Summe):
```
maenner2017<- V(year2017)$sex == 1
sum (maenner2017, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2018 (und ihre Summe):
```
maenner2018 <- V(year2018)$sex == 1
sum (maenner2018, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2019 (und ihre Summe):
```
maenner2019 <- V(year2019)$sex == 1
sum (maenner2019, na.rm = TRUE)
```

Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2015 (und ihre Summe):
```
frauen2015 <- V(year2015)$sex == 2
sum (frauen2015, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2016 (und ihre Summe):
```
frauen2016 <- V(year2016)$sex == 2
sum (frauen2016, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2017 (und ihre Summe):
```
frauen2017 <- V(year2017)$sex == 2
sum (frauen2017, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2018 (und ihre Summe):
```
frauen2018 <- V(year2018)$sex == 2
sum (frauen2018, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2019 (und ihre Summe):
```
frauen2019 <- V(year2019)$sex == 2
sum (frauen2019, na.rm = TRUE)
```

#### ...in Jury
**2015:**
1. Erstelle Teilnetzwerk aus Jurynetzwerk, ausschließlich mit Verbindungen aus 2015
```
juryfinal2015 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2015"])
```
2. Filtere alle Frauen aus dem oberen Netzwerk
```
frauenjury2015 <- V(juryfinal2015)$sex == 2
```
3. Summiere alle Frauen aus dem Netzwerk
```
sum (frauenjury2015, na.rm = TRUE)
```
4. Filtere alle Männer aus dem Netzwerk und summiere alle Männer
```
maennerjury2015 <- V(juryfinal2015)$sex == 1
sum (maennerjury2015, na.rm = TRUE)
```
**2016:**
```
juryfinal2016 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2016"])
frauenjury2016 <- V(juryfinal2016)$sex == 2
sum (frauenjury2016, na.rm = TRUE)
maennerjury2016 <- V(juryfinal2016)$sex == 1
sum (maennerjury2016, na.rm = TRUE)
```
**2017:**
```
juryfinal2017 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2017"])
frauenjury2017 <- V(juryfinal2017)$sex == 2
sum (frauenjury2017, na.rm = TRUE)
maennerjury2017 <- V(juryfinal2017)$sex == 1
sum (maennerjury2017, na.rm = TRUE)
```
**2018:**
```
juryfinal2018 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2018"])
frauenjury2018 <- V(juryfinal2018)$sex == 2
sum (frauenjury2018, na.rm = TRUE)
maennerjury2018 <- V(juryfinal2018)$sex == 1
sum (maennerjury2018, na.rm = TRUE)
```
**2019:**
```
juryfinal2019 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2019"])
frauenjury2019 <- V(juryfinal2019)$sex == 2
sum (frauenjury2019, na.rm = TRUE)
maennerjury2019 <- V(juryfinal2019)$sex == 1
sum (maennerjury2019, na.rm = TRUE)
```

#### ...unter Preisträgern
1. Erstelle Teilnetzwerk um alle Preisträger des Jahres **2015**
```
preistraeger2015 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2015"])
```
2. Filtere alle weiblichen Nodes heraus
```
frauenpreistraeger2015 <- V(preistraeger2015)$sex == 2
```
3. Summiere alle weiblichen Nodes
```
sum (frauenpreistraeger2015, na.rm = TRUE)
```
4. Ebenso für alle männlichen Nodes
```
maennerpreistraeger2015 <- V(preistraeger2015)$sex == 1
sum (maennerpreistraeger2015, na.rm = TRUE)
```

Für **2016**:
```
preistraeger2016 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2016"])
frauenpreistraeger2016 <- V(preistraeger2016)$sex == 2
preistraeger2016
sum (frauenpreistraeger2016, na.rm = TRUE)
maennerpreistraeger2016 <- V(preistraeger2016)$sex == 1
sum (maennerpreistraeger2016, na.rm = TRUE)
```
Für **2017**:
```
preistraeger2017 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2017"])
frauenpreistraeger2017 <- V(preistraeger2017)$sex == 2
sum (frauenpreistraeger2017, na.rm = TRUE)
maennerpreistraeger2017 <- V(preistraeger2017)$sex == 1
sum (maennerpreistraeger2017, na.rm = TRUE)
```
Für **2018**:
```
preistraeger2018 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2018"])
frauenpreistraeger2018 <- V(preistraeger2018)$sex == 2
sum (frauenpreistraeger2018, na.rm = TRUE)
maennerpreistraeger2018 <- V(preistraeger2018)$sex == 1
sum (maennerpreistraeger2018, na.rm = TRUE)
```
Für **2019**:
```
preistraeger2019 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2019"])
frauenpreistraeger2019 <- V(preistraeger2019)$sex == 2
sum (frauenpreistraeger2019, na.rm = TRUE)
maennerpreistraeger2019 <- V(preistraeger2019)$sex == 1
sum (maennerpreistraeger2019, na.rm = TRUE)
```

### Dominanz einzelner Medienunternehmen nach Jahren

**Wer (Personen) hat die meisten Preise abgeräumt (nach Jahren)? --> daraus auch: Dominanz der Medien unter Personen, die 2 oder mehr Preise gewonnen haben.**
```
preistraeger2015 <- subgraph.edges(g, E(g)[year == "2015"]) 
nur_preistraeger2015 <- subgraph.edges(preistraeger2015, E(preistraeger2015)[relation == 1]) # weil: Wenn man Outdegree von gesamtem Preisträgernetzwerk sortiert, werden alle Preisträger, die mehrere Arbetsverhältnisse haben, auch doppelt und dreifach gezählt --> will nur Anzahl der Preis-Relations!! 
outd_nur_preistraeger2015 <- degree(nur_preistraeger2015, mode="out")
sort(outd_nur_preistraeger2015)

preistraeger2016 <- subgraph.edges(g, E(g)[year == "2016"]) 
nur_preistraeger2016 <- subgraph.edges(preistraeger2016, E(preistraeger2016)[relation == 1]) # weil: Wenn man Outdegree von gesamtem Preisträgernetzwerk sortiert, werden alle Preisträger, die mehrere Arbetsverhältnisse haben, auch doppelt und dreifach gezählt --> will nur Anzahl der Preis-Relations!! 
outd_nur_preistraeger2016 <- degree(nur_preistraeger2016, mode="out")
sort(outd_nur_preistraeger2016)

preistraeger2017 <- subgraph.edges(g, E(g)[year == "2017"]) 
nur_preistraeger2017 <- subgraph.edges(preistraeger2017, E(preistraeger2017)[relation == 1]) # weil: Wenn man Outdegree von gesamtem Preisträgernetzwerk sortiert, werden alle Preisträger, die mehrere Arbetsverhältnisse haben, auch doppelt und dreifach gezählt --> will nur Anzahl der Preis-Relations!! 
outd_nur_preistraeger2017 <- degree(nur_preistraeger2017, mode="out")
sort(outd_nur_preistraeger2017)

preistraeger2018 <- subgraph.edges(g, E(g)[year == "2018"]) 
nur_preistraeger2018 <- subgraph.edges(preistraeger2018, E(preistraeger2018)[relation == 1]) # weil: Wenn man Outdegree von gesamtem Preisträgernetzwerk sortiert, werden alle Preisträger, die mehrere Arbetsverhältnisse haben, auch doppelt und dreifach gezählt --> will nur Anzahl der Preis-Relations!! 
outd_nur_preistraeger2018 <- degree(nur_preistraeger2018, mode="out")
sort(outd_nur_preistraeger2018)

preistraeger2019 <- subgraph.edges(g, E(g)[year == "2019"]) 
nur_preistraeger2019 <- subgraph.edges(preistraeger2019, E(preistraeger2019)[relation == 1]) # weil: Wenn man Outdegree von gesamtem Preisträgernetzwerk sortiert, werden alle Preisträger, die mehrere Arbetsverhältnisse haben, auch doppelt und dreifach gezählt --> will nur Anzahl der Preis-Relations!! 
outd_nur_preistraeger2019 <- degree(nur_preistraeger2019, mode="out")
sort(outd_nur_preistraeger2019)
```

**Wer (Medienunternehmen) beschäftigt die meisten Preisträger (nach Jahren)?**

**größter Indegree (gesamt, nach Jahren)**
```
ind2015 <- degree(year2015, mode="in")
sort(ind2015)

ind2016 <- degree(year2016, mode="in")
sort(ind2016)

ind2017 <- degree(year2017, mode="in")
sort(ind2017)

ind2018 <- degree(year2018, mode="in")
sort(ind2018)

ind2019 <- degree(year2019, mode="in")
sort(ind2019)
```
## Homophilie

### Männer/Frauenverteilung

#### Positiv-/Negativbeispiele nach Preisen (Wie viele Frauen waren vertreten?) 

**Alternativer Medienpreis**
1. Zähle, wie oft eine Frau am Preis beteiligt war (Preisträger & Jury, einzelne Personen werden mehrfach gezählt)
```
frauen_alternativermedienpreis <- V(alternativermedienpreis[[1]])$sex == 2
sum (frauen_alternativermedienpreis, na.rm = TRUE)
```

2. Zähle alle Frauen, die in der Jury saßen (einzelne Personen werden mehrfach gezählt)
```
frauen_jury_alternativermedienpreis <- V(jury_alternativermedienpreis[[1]])$sex == 2
sum(frauen_jury_alternativermedienpreis, na.rm = TRUE)
```

3. Zähle alle Frauen, die einen Preis gewonnen haben (einzelne Personen werden mehrfach gezählt)
```
frauen_preistraeger_alternativermedienpreis <- V(preistraeger_alternativermedienpreis[[1]])$sex == 2
sum(frauen_preistraeger_alternativermedienpreis, na.rm = TRUE)
```

**Axel-Springer-Preis**
Gesamt
```
frauen_axelspringerpreis <-  V(axelspringerpreis[[1]])$sex == 2
sum(frauen_axelspringerpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_axelspringerpreis <- V(jury_axelspringerpreis[[1]])$sex == 2
sum(frauen_jury_axelspringerpreis, na.rm = TRUE)

```
Preisträger
```
frauen_preistraeger_axelspringerpreis <- V(preistraeger_axelspringerpreis[[1]])$sex == 2
sum(frauen_preistraeger_axelspringerpreis, na.rm = TRUE)
```

**Bremer Fernsehpreis**
Gesamt
```
frauen_bremerfernsehpreis <-  V(bremerfernsehpreis[[1]])$sex == 2
sum(frauen_bremerfernsehpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_bremerfernsehpreis <- V(jury_bremerfernsehpreis[[1]])$sex == 2
sum(frauen_jury_bremerfernsehpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_bremerfernsehpreis <- V(preistraeger_bremerfernsehpreis[[1]])$sex == 2
sum(frauen_preistraeger_bremerfernsehpreis, na.rm = TRUE)
```

**djp**
Gesamt
```
frauen_djp <- V(djp[[1]])$sex == 2
sum(frauen_djp, na.rm = TRUE)
```
Jury
```
frauen_jury_djp <- V(jury_djp[[1]])$sex == 2
sum(frauen_jury_djp, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_djp <- V(preistraeger_djp[[1]])$sex == 2
sum(frauen_preistraeger_djp, na.rm = TRUE)
```

**Deutscher Radiopreis**
Gesamt
```
frauen_deutscherradiopreis <- V(deutscherradiopreis[[1]])$sex == 2
sum(frauen_deutscherradiopreis, na.rm = TRUE)
```
Jury
```
frauen_jury_deutscherradiopreis <- V(jury_deutscherradiopreis[[1]])$sex == 2
sum(frauen_jury_deutscherradiopreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_deutscherradiopreis <- V(preistraeger_deutscherradiopreis[[1]])$sex == 2
sum(frauen_preistraeger_deutscherradiopreis, na.rm = TRUE)
```

**Deutscher Reporterpreis**
Gesamt
```
frauen_deutscherreporterpreis <- V(deutscherreporterpreis[[1]])$sex == 2
sum(frauen_deutscherreporterpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_deutscherreporterpreis <- V(jury_deutscherreporterpreis[[1]])$sex == 2
sum(frauen_jury_deutscherreporterpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_deutscherreporterpreis <- V(preistraeger_deutscherreporterpreis[[1]])$sex == 2
sum(frauen_preistraeger_deutscherreporterpreis, na.rm = TRUE)
```

**Ernst-Schneider-Preis**
Gesamt
```
frauen_ernstschneiderpreis <- V(ernstschneiderpreis[[1]])$sex == 2
sum(frauen_ernstschneiderpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_ernstschneiderpreis <- V(jury_ernstschneiderpreis[[1]])$sex == 2
sum(frauen_jury_ernstschneiderpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_ernstschneiderpreis <- V(preistraeger_ernstschneiderpreis[[1]])$sex == 2
sum(frauen_preistraeger_ernstschneiderpreis, na.rm = TRUE)
```

**Georg von Holtzbrinck-Preis**
Gesamt
```
frauen_gvhpreis <- V(gvhpreis[[1]])$sex == 2
sum(frauen_gvhpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_gvhpreis <- V(jury_gvhpreis[[1]])$sex == 2
sum(frauen_jury_gvhpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_gvhpreis <- V(preistraeger_gvhpreis[[1]])$sex == 2
sum(frauen_preistraeger_gvhpreis, na.rm = TRUE)
```

**Grimme Online Award**
Gesamt
```
frauen_grimmeonlineaward <- V(grimmeonlineaward[[1]])$sex == 2
sum(frauen_grimmeonlineaward, na.rm = TRUE)
```
Jury
```
frauen_jury_grimmeonlineaward <- V(jury_grimmeonlineaward[[1]])$sex == 2
sum(frauen_jury_grimmeonlineaward, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_grimmeonlineaward <- V(preistraeger_grimmeonlineaward[[1]])$sex == 2
sum(frauen_preistraeger_grimmeonlineaward, na.rm = TRUE)
```

**Helmut-Schmidt-Preis**
Gesamt
```
frauen_helmutschmidtpreis <- V(helmutschmidtpreis[[1]])$sex == 2
sum(frauen_helmutschmidtpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_helmutschmidtpreis <- V(jury_helmutschmidtpreis[[1]])$sex == 2
sum(frauen_jury_helmutschmidtpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_helmutschmidtpreis <- V(preistraeger_helmutschmidtpreis[[1]])$sex == 2
sum(frauen_preistraeger_helmutschmidtpreis, na.rm = TRUE)
```

**Herbert-Quandt-Preis**
Gesamt
```
frauen_herbertquandtpreis <- V(herbertquandtpreis[[1]])$sex == 2
sum(frauen_herbertquandtpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_herbertquandtpreis <- V(jury_herbertquandtpreis[[1]])$sex == 2
sum(frauen_jury_herbertquandtpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_herbertquandtpreis <- V(preistraeger_herbertquandtpreis[[1]])$sex == 2
sum(frauen_preistraeger_herbertquandtpreis, na.rm = TRUE)
```

**Journalist des Jahres**
Gesamt
```
frauen_journalistdesjahres <- V(journalistdesjahres[[1]])$sex == 2
sum(frauen_journalistdesjahres, na.rm = TRUE)
```
Jury
```
frauen_jury_journalistdesjahres <- V(jury_journalistdesjahres[[1]])$sex == 2
sum(frauen_jury_journalistdesjahres, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_journalistdesjahres <- V(preistraeger_journalistdesjahres[[1]])$sex == 2
sum(frauen_preistraeger_journalistdesjahres, na.rm = TRUE)
```

**Katholischer Medienpreis**
Gesamt
```
frauen_katholischermedienpreis<- V(katholischermedienpreis[[1]])$sex == 2
sum(frauen_katholischermedienpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_katholischermedienpreis <- V(jury_katholischermedienpreis[[1]])$sex == 2
sum(frauen_jury_katholischermedienpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_katholischermedienpreis <- V(preistraeger_katholischermedienpreis[[1]])$sex == 2
sum(frauen_preistraeger_katholischermedienpreis, na.rm = TRUE)
```

**Kurt-Tucholsky-Preis**
Gesamt
```
frauen_kurttucholskypreis <- V(kurttucholskypreis[[1]])$sex == 2
sum(frauen_kurttucholskypreis, na.rm = TRUE)
```
Jury
```
frauen_jury_kurttucholskypreis <- V(jury_kurttucholskypreis[[1]])$sex == 2
sum(frauen_jury_kurttucholskypreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_kurttucholskypreis <- V(preistraeger_kurttucholskypreis[[1]])$sex == 2
sum(frauen_preistraeger_kurttucholskypreis, na.rm = TRUE)
```

**Leuchtturm**
Gesamt
```
frauen_leuchtturm<- V(leuchtturm[[1]])$sex == 2
sum(frauen_leuchtturm, na.rm = TRUE)
```
Jury
```
frauen_jury_leuchtturm <- V(jury_leuchtturm[[1]])$sex == 2
sum(frauen_jury_leuchtturm, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_leuchtturm <- V(preistraeger_leuchtturm[[1]])$sex == 2
sum(frauen_preistraeger_leuchtturm, na.rm = TRUE)
```

**Ludwig-Börne-Preis**
Gesamt
```
frauen_ludwigboernepreis <- V(ludwigboernepreis[[1]])$sex == 2
sum(frauen_ludwigboernepreis, na.rm = TRUE)
```
Jury
```
frauen_jury_ludwigboernepreis <- V(jury_ludwigboernepreis[[1]])$sex == 2
sum(frauen_jury_ludwigboernepreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_ludwigboernepreis <- V(preistraeger_ludwigboernepreis[[1]])$sex == 2
sum(frauen_preistraeger_ludwigboernepreis, na.rm = TRUE)
```

**Ludwig-Erhard-Preis**
Gesamt
```
frauen_ludwigerhardpreis <- V(ludwigerhardpreis[[1]])$sex == 2
sum(frauen_ludwigerhardpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_ludwigerhardpreis <- V(jury_ludwigerhardpreis[[1]])$sex == 2
sum(frauen_jury_ludwigerhardpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_ludwigerhardpreis <- V(preistraeger_ludwigerhardpreis[[1]])$sex == 2
sum(frauen_preistraeger_ludwigerhardpreis, na.rm = TRUE)
```

**Nannen-Preis**
Gesamt
```
frauen_nannenpreis <- V(nannenpreis[[1]])$sex == 2
sum(frauen_nannenpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_nannenpreis <- V(jury_nannenpreis[[1]])$sex == 2
sum(frauen_jury_nannenpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_nannenpreis<- V(preistraeger_nannenpreis[[1]])$sex == 2
sum(frauen_preistraeger_nannenpreis, na.rm = TRUE)
```

**Robert-Geisendörfer-Preis**
Gesamt
```
frauen_robertgeisendoerferpreis <- V(robertgeisendoerferpreis[[1]])$sex == 2
sum(frauen_robertgeisendoerferpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_robertgeisendoerferpreis <- V(jury_robertgeisendoerferpreis[[1]])$sex == 2
sum(frauen_jury_robertgeisendoerferpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_robertgeisendoerferpreis <- V(preistraeger_robertgeisendoerferpreis[[1]])$sex == 2
sum(frauen_preistraeger_robertgeisendoerferpreis, na.rm = TRUE)
```

**Theodor-Wolff-Preis**
Gesamt
```
frauen_theodorwolffpreis <- V(theodorwolffpreis[[1]])$sex == 2
sum(frauen_theodorwolffpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_theodorwolffpreis <- V(jury_theodorwolffpreis[[1]])$sex == 2
sum(frauen_jury_theodorwolffpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_theodorwolffpreis <- V(preistraeger_theodorwolffpreis[[1]])$sex == 2
sum(frauen_preistraeger_theodorwolffpreis, na.rm = TRUE)
```

**Wächterpreis**
Gesamt
```
frauen_waechterpreis <- V(waechterpreis[[1]])$sex == 2
sum(frauen_waechterpreis, na.rm = TRUE)
```
Jury
```
frauen_jury_waechterpreis <- V(jury_theodorwolffpreis[[1]])$sex == 2
sum(frauen_jury_waechterpreis, na.rm = TRUE)
```
Preisträger
```
frauen_preistraeger_waechterpreis <- V(preistraeger_waechterpreis[[1]])$sex == 2
sum(frauen_preistraeger_waechterpreis, na.rm = TRUE)
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
