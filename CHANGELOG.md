# Changelog

Alle relevanten Änderungen an diesem Projekt werden hier dokumentiert.

## [2.0.0] - 2024-12-06

### ✨ Neu
- **Mobile-First Redesign** - Komplett überarbeitete Benutzeroberfläche
- **Touch-optimierte Bedienung** - Mindestens 44px große Touch-Targets
- **Slide-up Modale** - Native App-ähnliche Dialoge auf Mobilgeräten
- **KPI-Dashboard** - Budget-Übersicht immer sichtbar oben
- **Kollabierbare Karten** - Platzsparende Darstellung der Sektionen
- **7 Transportmodi** - Auto, Bahn, Taxi, Flug, ÖPNV, Fahrrad, Zu Fuß
- **Automatische Kostenberechnung** - Echtzeit-Berechnung bei Eingabe
- **Progress-Bar mit Farbwechsel** - Grün → Orange → Rot je nach Budget

### 🎨 Design
- Modernes dunkles Farbschema
- Verbesserte Typografie und Lesbarkeit
- Sanfte Animationen und Übergänge
- Responsive Grid-Layouts
- Einheitliche Abstände und Rundungen

### 🔧 Technisch
- Single-File HTML (kein Build-Prozess nötig)
- LocalStorage für persistente Datenspeicherung
- JSON Export/Import für Backups
- Keine externen Abhängigkeiten (außer System-Fonts)
- Unterstützung für `prefers-reduced-motion`

### 🐛 Fixes
- Verbesserte Formularvalidierung
- Konsistente Währungsformatierung
- Korrekte Berechnung bei allen Transportmodi

## [1.0.0] - 2025-01-01

### Initial Release
- Grundfunktionen für Reiseplanung
- Budget-Tracking
- Tickets & Fixkosten
- Etappen-Verwaltung
