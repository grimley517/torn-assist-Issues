# Torn Assist – Architecture

This document describes the high-level architecture of the Torn Assist system using C4 diagrams (context and container views) expressed in [Mermaid](https://mermaid.js.org/) format.

---

## C4 Context Diagram

The context diagram shows **who** interacts with Torn Assist and **what external systems** it depends on.

```mermaid
C4Context
    title System Context – Torn Assist

    Person(player, "Torn Player", "A player of the online game Torn who wants assistance managing game activities.")

    System(tornAssist, "Torn Assist", "A browser-based tool that helps Torn players track stats, plan attacks, manage items, and optimize gameplay.")

    System_Ext(tornApi, "Torn API", "The official REST API provided by Torn City that exposes player, faction, market, and game data.")

    Rel(player, tornAssist, "Uses", "HTTPS / Browser")
    Rel(tornAssist, tornApi, "Fetches game data", "HTTPS / JSON")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

---

## C4 Container Diagram

The container diagram shows the **internal building blocks** of Torn Assist and how they interact.

```mermaid
C4Container
    title Container Diagram – Torn Assist

    Person(player, "Torn Player", "Accesses Torn Assist via a web browser.")

    System_Boundary(tornAssist, "Torn Assist") {
        Container(spa, "Single-Page Application", "HTML / CSS / JavaScript", "Provides the entire Torn Assist user interface and client-side logic. Hosted as a static site on GitHub Pages.")
        ContainerDb(localStorage, "Local Storage", "Browser Local Storage", "Persists the player's API key and user preferences between sessions.")
    }

    System_Ext(tornApi, "Torn API", "Torn City's official REST API.")
    System_Ext(gitHubPages, "GitHub Pages", "Static hosting platform that serves the SPA assets.")

    Rel(player, spa, "Visits and interacts with", "HTTPS / Browser")
    Rel(spa, localStorage, "Reads and writes settings & API key", "Web Storage API")
    Rel(spa, tornApi, "Requests player, faction and market data", "HTTPS / JSON")
    Rel(gitHubPages, spa, "Serves static assets to browser", "HTTPS")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

---

## Notes

| Term | Meaning |
|------|---------|
| **SPA** | Single-Page Application – the entire front-end runs in the browser with no server-side component. |
| **Torn API** | Third-party API owned by Torn City Ltd; requires an API key provided by the player. |
| **GitHub Pages** | Free static-site hosting used to deploy Torn Assist; no custom back-end is required. |
