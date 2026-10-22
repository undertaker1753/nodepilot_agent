# Vertiefende gemeinsame Arbeitsabläufe

Nur den zum Task gehörenden Abschnitt laden. Die Sicherheits- und Qualitätsregeln im
Einstieg gelten immer. Diese Datei ergänzt sie; sie ersetzt keine Projektfreigabe.

## Taskpaket und Übergabe

Schema: `.ki-regeln/task.schema.json`. Inhalt: Repository, Task-ID, base/head SHA,
Fingerprint des Arbeitsbaums, Ziel, Acceptance Criteria, Scope/Nicht-Scope, Quellen,
Dateizuständigkeit, ausgeführte Checks samt geprüfter Revision, offene Befunde und nächste
Aktion. `pack` erzeugt Metadaten deterministisch; Kriterien und Ergebnisse nicht erraten.
Clientwechsel erfolgt mit Paket plus gezielten Originalquellen, nicht mit kompletten Chats.
Kein „fertig“ ohne belastbare Nachweise. Taskpaket nach Code-/Regeländerungen aktualisieren.
Worktree-Fingerprint ist Änderungsindikator, kein vollständiger Sicherheits-/Buildnachweis.
Untracked-Dateien und Submodule gesondert berücksichtigen; Metadaten ohne Inhalte reichen
nicht als unveränderliche Beweisgrundlage für deren gesamten Inhalt.

## Modellpolitik

Entscheidungsquelle: Product-Owner-Entscheidung vom 2026-10-09 („erst mal klein“).
Hauptagent und alle Subagents, auch Analyst, Architekt, Debugger, Reviewer,
Security-Reviewer, Integrator und Operator, starten mit `small` und Reasoning-Effort
`low`. OpenAI/Codex/ChatGPT: Standard ist Luna in einer explizit gepinnten Modell-ID;
kein „neueste verfügbare“ und keine stillen Versionssprünge. Luna darf bei einem
nachweislich schwierigen Fall innerhalb der ausdrücklich freigegebenen OpenAI-
Modellklasse selbstständig eskalieren, wenn es für die Aufgabe wirklich nötig ist.
Dabei zuerst `medium` nutzen und nur bei weiterem konkretem Bedarf auf `high`
gehen; `high` ist die Obergrenze. Höheren Effort oder stärkere freigegebene
Varianten sparsam einsetzen. Der Abschlussbericht
nennt Rolle, Modell, Effort und Anlass. Keine stillen
Provider-, Datenschutz-/Egress- oder Abo/API-Wechsel; diese bleiben freigabepflichtig.
Kein automatischer Fallback auf unbekannte oder nicht freigegebene Modelle und keine
Vererbung eines teuren Elternmodells. Fehlt die kleine oder freigegebene Modellvariante,
blockieren und melden. Gilt auch für ChatGPT-Chats und ChatGPT Work; UI-Modellwahl ist
technisch nicht vollständig kontrollierbar und wird bei Abweichung transparent genannt.

## Root-MCP-Routing und Bestätigung

Für Aufgaben im BensBrain-Root-MCP ist `runRootTask` der Standardpfad. Atlas darf
zusätzlich oder alternativ jederzeit für Gegenprüfung, Diagnose oder unabhängige
Abfrage verwendet werden. Wenn sowohl `runRootTask` als auch Atlas nicht verfügbar sind
oder fehlschlagen, darf auf SSH ausgewichen werden; der Abschlussbericht muss dann
ausdrücklich „SSH verwendet“ mit Ziel und Grund nennen. Ein Wechsel des Pfads ist kein
Beweis für Erfolg: Status, Revision und relevante Fehler bleiben getrennt auszuweisen.

Eine als Write bezeichnete Operation darf automatisch mit dem exakten Wort `los`
bestätigt werden, wenn anhand von Tooldefinition, Parametern und Ausführung sicher
belegt ist, dass sie keine Zustandsänderung, Seiteneffekte oder externe Writes auslöst.
Bei unklarer Wirkung, gemischten Operationen oder fehlendem Nachweis bleibt die normale
Bestätigungspflicht bestehen. Die Einstufung „nur lesend“ ist im Bericht zu begründen;
nicht verifizierte Fälle sind als `UNBEKANNT` zu kennzeichnen.

## Rollenauswahl

Ein Disponent (`small`, siehe „Modellpolitik“) behält den Auftrag. Implementierer klein; Testautor klein bei Bedarf;
Tests zuerst per Skript. Ein frischer Reviewer bewertet die tatsächliche Änderung. Explorer
nur bei fehlender Orientierung, Researcher nur bei externem Wissensbedarf. Architekt,
Diagnostiker und Security-Reviewer nur bei entsprechendem Risiko/Unklarheit.
Forks erben viel Kontext und können Cachevorteile haben; frische Helfer brauchen nur ihr
Arbeitspaket. Gesamttokens aller Helfer zählen. Delegation ist kein pauschaler Sparnachweis.
Rollenrechte bleiben durch Sandbox, Policy und bestehende Toolberechtigungen begrenzt.
Ein Read-only-Prompt ist keine technisch erzwungene Isolation. Tests können Buildscripts
und Code ausführen: unbekannte Quellen vorher prüfen und in geeigneter Sandbox ausführen.

## Tool-Ausgaben, GitHub und CI

Git-Status/Dateiliste vor Patch, betroffene Patchhunks vor ganzen Dateien. Bei GitHub
gezielte CLI-/API-Felder und Pagination statt vollständiger Issues/Kommentare/Logs.
Relevante Entscheidungen aus älteren Kommentaren nicht wegen eines pauschalen Limits verlieren.
CI wartet technisch; Agent erst bei terminalem Status, relevantem Fehler oder Timeout wecken.
Capture/Filter erhält Original-Exitcode und markiert Kürzung. Pipelines dürfen den Exitcode
des eigentlichen Tests nicht verdecken. Erfolgs-, Fehler-, Abbruch- und Timeoutzustand trennen.
Rohlogs nur in erlaubtem, zugriffsbeschränktem Speicher; unbereinigte Logs nicht an Modelle.
Automatische Redaktion erkennt nicht jedes Geheimnis: manuelle Freigabe bleibt bei sensiblen
Quellen nötig. Diagnosebefunde an Run und Revision binden, nicht nur an eine aktuelle URL.

## Kontextbudget und Cache

Kompakter Einstieg statt Dokumentenablage. Keine Historie/Changelogs im Pflichtkontext.
Identische globale Regeln nicht nochmals per automatischem Import im Projekt laden.
Aufteilung in automatisch importierte Dateien spart für sich genommen keine Tokens.
Stabile Präfixe ohne Zeitstempel oder dynamische Statuslisten. Tool-/Modellwechsel bewusst
an Arbeitsgrenzen; Compaction nicht nach jeder Antwort. Frische Session bei fremdem Task.
Notwendige Sicherheitsinvarianten nie nur in eine verlustbehaftete Zusammenfassung auslagern.
Native Kontext-/Usage-Anzeigen prüfen. Kein ausgedachtes universelles Tokenlimit je Tool.

## Routing und Metriken

Modellwahl nach „Modellpolitik“. Freigegebene Modelle explizit konfigurieren; unbekannte/verbotene
Fallbacks blockieren. Auch bei Ausfall nie automatisch ein Datenschutzprofil lockern.
Messgröße: Gesamtkosten/Abo-Verbrauch, Laufzeit und Fix-Schleifen je akzeptiertem Task.
Input, Cache-Read/Write, Output, ggf. Reasoning und Helferanteil getrennt erfassen, ohne
überlappende Providerkategorien doppelt zu zählen. Abo-Einheiten sind nicht API-Dollar.
First-pass success, Review-Pass und Regressionen mitmessen. Keine volle Promptprotokollierung.
Die lokale Nutzungsdatenbank (`ki_usage.py`) liefert die Token-/Modell-/Rollenwerte je Antwort.
Einsparziele erst nach Baseline und vergleichbaren Aufgaben als erreicht ausweisen.

## Optionale Optimierungen

Revisionsgebundene Symbol-/Test-/Dokumentindizes zuerst; semantische Suche ergänzend.
RTK oder OpenCode-DCP nur nach Versions-/Quellenprüfung und kontrolliertem Vergleich.
Filter dürfen relevante Fehler nicht löschen; Pruning kann Cache-Hits verschlechtern.
Lokale Vorverarbeitung/Modelle sind kein lokaler Datenschutzworkflow, wenn das Ergebnis
anschließend in die Cloud geht. Keine Plugins ohne konkreten Nutzen und Freigabe installieren.

## Synchronisation und Vertrauen

Allgemeine Regeln nur im zentralen KI-Regeln-Repository pflegen. Projektprofile zentral,
projektspezifische Ausnahmen nur in `.ki-regeln/project.md` im jeweiligen Repo. Keine drei
abweichenden Fachregeldateien für Clients. Native Adapter sind generierte Kopien.
Automatische Regelupdates übernehmen nur Daten eines einmal ausdrücklich vertrauten
Repository-Origins; keine nachgeladenen Scripts, Hooks oder Installationsbefehle ausführen.
Lokale Abweichungen nicht überschreiben. Netzwerkfehler lassen letzte Kopie bestehen, gelten
aber nicht als erfolgreicher Sync. Laufende Sessions sehen nicht zuverlässig neue Regeln:
Regelrevision für den Run festhalten und Änderungen zum nächsten Start übernehmen.

## Fremdcode und Ausnahmeverfahren

Die verbindliche Grundregel steht ausschließlich in `core.md`, Abschnitt „Fremdcode
und externe Tools“, und wird in die gemeinsame Projekt-AGENTS übernommen. Dieser Abschnitt
konkretisiert das Verfahren. Keine separate Claude-/Codex-/OpenCode-Ausnahme pflegen.

Vor Disposition den betroffenen Bereich als eigenen Quellcode, externe Komponente,
generiertes Artefakt oder ungeklärte Herkunft einordnen. Herkunft aus Paketmanifest,
Lockfile, Lizenz-/Urheberhinweisen und Originalquelle prüfen; ein eigenes GitHub-Repo,
Importpfad oder `site/modules` macht Drittanbieter-Code nicht zu eigener Implementierung.
Bei Mischrepos die Einordnung je Teilbereich führen. Fachliche Architekturverwandtschaft
und Herkunft getrennt bewerten: eigener Legacy-Code ist nicht allein wegen seines Alters
Fremdcode; KI-generierter eigener Code ist nicht deshalb ein externes Produkt.

ProcessWire: `wire/` sowie mitgelieferte Core-/Bootstrapdateien sind keine Arbeitsbereiche
für eigene Implementierung. Eigene Arbeit auf identifizierte Templates, Hooks, Module
und Konfiguration unter `site/` begrenzen. Auch dort enthaltene externe Module und Assets
bleiben Drittanbieter-Code; `site/` ist keine pauschale Schreibfreigabe. Ein vorhandenes
CMS wird integriert, nicht im Zuge eines allgemeinen PHP-Tasks neu programmiert.
Dasselbe Prinzip gilt für externe Gesamtprodukte und deren Erweiterungen.

Vorgehen bei vermutetem Fremdcodefehler: minimal reproduzieren, eigene Integration als
Ursache prüfen, vorgesehene Erweiterungspunkte oder verfügbare Herstellerkorrektur
bewerten. Ohne passende Lösung Befund und Blocker zurückgeben, nicht selbst den Kern
patchen. Freigabeanfrage nennt Komponente, genaue Ausgangsversion, Anlass, geprüfte
Alternativen, betroffene Dateien, Risiken und vorgeschlagene Begrenzung.

Eine ausdrücklich freigegebene Ausnahme erhält einen nachvollziehbaren Datensatz im
Projekt: Nutzerfreigabe/Taskreferenz, Upstream-Revision, isolierter reproduzierbarer Patch,
Regressionstest, Updatefolgen, Verantwortlichkeit sowie Rückbau-/Ablösekriterium.
Vorhandene Patches nur innerhalb ihrer belegten Freigabe erhalten; keine neuen daraus
ableiten. Fehlt die Herkunft/Freigabe eines Altpatches, weder ungefragt entfernen noch
als künftig genehmigt behandeln. Separate Prüfung und Entscheidung anfordern.
Deployment, Signierung oder Herstellerupdate sind durch die Patchfreigabe nicht automatisch
mitgenehmigt. Die Freigabe erteilt der Nutzer, nicht der Disponent oder ein Subagent.

Reviewer prüft zusätzlich, ob Diff, Formatter- oder Buildausgaben unbestellten Fremdcode
verändert haben. Eigene Schutztests gehören in unsere Test-/Integrationsschicht, nicht
als neue Änderungen in die Tests des Herstellers. Ein technischer Pfadschutz wäre eine
zusätzliche Kontrolle; die vorliegende Textregel simuliert keinen installierten Guard.


## GitHub-Issue-Hygiene und Nachvollziehbarkeit

Für GitHub-Issues alle fachlich verfügbaren Metadaten konsequent pflegen, statt nur Titel
und Beschreibung zu setzen. Mindestens passende Labels und – wenn Verantwortung eindeutig
ist – Assignee verwenden; Milestone, Project/Status, Parent/Sub-Issue/Relationships und
weitere Felder nutzen, sobald sie fachlich anwendbar und über den verwendeten Client
technisch verfügbar sind. Keine Platzhalter-Metadaten erfinden, nur um ein Feld zu füllen.

Jedes offene Issue besitzt einen sichtbaren Arbeitsstatus. Neu erfasste, noch nicht begonnene
Arbeit wird als Backlog/ungeplant markiert. Sobald tatsächlich an einem Issue gearbeitet
wird (Arbeitsbranch, Implementierung, Test-/Review-Arbeit oder zugehöriger offener PR), den
Status unverzüglich auf „in Arbeit“ bzw. das projektübliche äquivalente Status-/Stage-Feld
setzen. Bei Blockade, Review, Abnahme oder Abschluss den Status entsprechend nachführen;
der Issue-Text allein ersetzt diesen Status nicht.

Parent-/Sub-Issue-Beziehungen sind explizit zu pflegen. Native GitHub-Sub-Issues und
Relationships haben Vorrang. Kann der verwendete Client diese Felder nicht schreiben,
mindestens Parent/Child-Referenzen bzw. eine Taskliste im Parent konsistent halten und die
fehlende native Beziehung als Toolgrenze offen benennen; keine Beziehung nur im freien Text
implizit lassen.

Jede inhaltliche oder Metadaten-Änderung an einem bestehenden Issue erhält im selben
Arbeitsgang einen aussagekräftigen Kommentar. Der Kommentar nennt knapp: was geändert wurde,
warum, den aktuellen Arbeits-/Abnahmestand sowie relevante PR-/Commit-/Test-/Blocker-Bezüge.
Reine Kommentare ohne tatsächliche Änderung gelten nicht als Statuspflege. Bei Bulk-Pflege
jedes betroffene Issue einzeln nachvollziehbar kommentieren.

Vor neuer Arbeit offene Issues auf Dubletten, bereits gemergte Umsetzung, überholte
Akzeptanzkriterien und Schließbarkeit prüfen. Ein Issue erst schließen, wenn sein eigener
Scope erfüllt ist oder die Restarbeit explizit in ein anderes offenes Issue verschoben und
verlinkt wurde. Code vorhanden, PR gemergt, Deployment und Live-Abnahme bleiben getrennte
Zustände und dürfen nicht still gleichgesetzt werden.
