<div align="center">

  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%2064%2064%22%3E%3Crect%20width%3D%2264%22%20height%3D%2264%22%20fill%3D%22%231a1a2e%22%20rx%3D%2212%22%2F%3E%3Cellipse%20cx%3D%2232%22%20cy%3D%2232%22%20rx%3D%2222%22%20ry%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22%23ff6b35%22%20stroke-width%3D%221.5%22%20opacity%3D%220.35%22%20transform%3D%22rotate%28-15%2032%2032%29%22%2F%3E%3Cellipse%20cx%3D%2232%22%20cy%3D%2232%22%20rx%3D%2214%22%20ry%3D%2220%22%20fill%3D%22none%22%20stroke%3D%22%23ff6b35%22%20stroke-width%3D%221.5%22%20opacity%3D%220.25%22%20transform%3D%22rotate%2825%2032%2032%29%22%2F%3E%3Ccircle%20cx%3D%2232%22%20cy%3D%2232%22%20r%3D%227%22%20fill%3D%22%23ff6b35%22%2F%3E%3Ccircle%20cx%3D%2232%22%20cy%3D%2232%22%20r%3D%229%22%20fill%3D%22%23ff6b35%22%20opacity%3D%220.2%22%2F%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2224%22%20r%3D%223%22%20fill%3D%22%23ff914d%22%2F%3E%3Ccircle%20cx%3D%2222%22%20cy%3D%2248%22%20r%3D%222.5%22%20fill%3D%22%23ff6b35%22%2F%3E%3C%2Fsvg%3E">
    <img alt="Orbital Sim Logo" src="data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%2064%2064%22%3E%3Crect%20width%3D%2264%22%20height%3D%2264%22%20fill%3D%22%23f0f0f0%22%20rx%3D%2212%22%2F%3E%3Cellipse%20cx%3D%2232%22%20cy%3D%2232%22%20rx%3D%2222%22%20ry%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22%23ff6b35%22%20stroke-width%3D%221.5%22%20opacity%3D%220.4%22%20transform%3D%22rotate%28-15%2032%2032%29%22%2F%3E%3Cellipse%20cx%3D%2232%22%20cy%3D%2232%22%20rx%3D%2214%22%20ry%3D%2220%22%20fill%3D%22none%22%20stroke%3D%22%23ff6b35%22%20stroke-width%3D%221.5%22%20opacity%3D%220.3%22%20transform%3D%22rotate%2825%2032%2032%29%22%2F%3E%3Ccircle%20cx%3D%2232%22%20cy%3D%2232%22%20r%3D%227%22%20fill%3D%22%23ff6b35%22%2F%3E%3Ccircle%20cx%3D%2232%22%20cy%3D%2232%22%20r%3D%229%22%20fill%3D%22%23ff6b35%22%20opacity%3D%220.18%22%2F%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2224%22%20r%3D%223%22%20fill%3D%22%23ff914d%22%2F%3E%3Ccircle%20cx%3D%2222%22%20cy%3D%2248%22%20r%3D%222.5%22%20fill%3D%22%23ff6b35%22%2F%3E%3C%2Fsvg%3E" width="80" height="80">
  </picture>

  <h1>🌌 Orbital Sim</h1>

  <p>
    <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="MIT License"></a>
    <img src="https://img.shields.io/badge/dependencies-0-brightgreen" alt="Zero Dependencies">
    <img src="https://img.shields.io/badge/format-single--file-ff6b6b" alt="Single File">
  </p>

</div>

> A polished 2-body gravity simulation in your browser — click, drag, and watch orbits unfold.

![Orbital Sim](https://raw.githubusercontent.com/soumendrak/orbital-sim/main/docs/screenshot.png)

**Live demo:** [https://soumendrak.github.io/orbital-sim](https://soumendrak.github.io/orbital-sim)

---

## Features

- **Newtonian gravity** — bodies interact via `F = G·m₁·m₂ / r²` with softening for stability
- **Click‑to‑launch** — drag from a point to set a body's initial velocity vector
- **Real‑time trails** — fading orbital paths that preserve history
- **Speed control** — slider from 0.1× slow‑motion to 3× fast‑forward
- **Pause / Resume** — spacebar or button to freeze the simulation
- **Up to 4 orbiting bodies** — each with a distinct colour
- **Hover tooltip** — shows mass, velocity, and distance from the central star
- **Responsive** — works on desktop and mobile (touch support)
- **Keyboard shortcuts** — spacebar toggles pause, `C` clears trails
- **Body removal** — click an orbiting body to delete it
- **Auto‑cleanup** — bodies that escape or crash into the star are removed

---

## How it works

```mermaid
flowchart TD
    A[requestAnimationFrame] --> B{Sim paused?}
    B -->|Yes| C[Draw frame]
    B -->|No| D[Compute elapsed time]
    D --> E[Scale dt by speed slider]
    E --> F[physicsStep]
    F --> G[For each pair:<br/>compute gravitational<br/>acceleration]
    G --> H[Update velocity<br/>symplectic Euler]
    H --> I[Update position]
    I --> J[Record trail point]
    J --> K{Check boundaries}
    K -->|Escaped or<br/>crashed into star| L[Remove body]
    K -->|OK| M[Draw frame]
    C --> N[Render star field<br/>+ trails + bodies<br/>+ tooltip]
    M --> N
    N --> A
```

### Physics details

The simulation uses **symplectic Euler integration** (velocity updated first, then position), which preserves energy better than naive Euler and produces stable elliptical orbits for appropriate initial conditions. A softening term (ε = 20 px) prevents numerical blowup when bodies pass close to each other.

---

## Usage

Open `index.html` in any modern browser. No build step, no server required (though a local server helps with some browser policies).

| Action | Result |
|--------|--------|
| Click and drag | Place an orbiting body with initial velocity in the drag direction |
| Click an orbiting body | Remove it |
| Hover a body | See mass, velocity, distance from star |
| Spacebar | Pause / Resume |
| `C` key | Clear all trails |
| Speed slider | Adjust simulation speed |
| Clear button | Erase orbital trails |

---

## Project structure

```
orbital-sim/
├── index.html    # Single self‑contained HTML file (~25 KB)
├── README.md     # This file
└── LICENSE       # MIT License
```

## Deploy to GitHub Pages

```bash
git clone https://github.com/soumendrak/orbital-sim.git
cd orbital-sim
git checkout -b gh-pages
git push origin gh-pages
```

Then enable GitHub Pages in your repository settings, pointing to the `gh-pages` branch.

---

## License

Licensed under the [MIT License](LICENSE).
