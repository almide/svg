# almide/svg

SVG document generation for [Almide](https://github.com/almide/almide). Pure Almide, no compiler extensions, works on all targets.

## Install

```bash
almide add svg@v0.1.0
```

## Usage

```almide
import svg
import svg.path

let chart = svg.doc(800.0, 600.0, [
  svg.rect(10.0, 10.0, 200.0, 100.0)
    |> svg.fill("#4A90D9")
    |> svg.rx(8.0),
  svg.circle(400.0, 200.0, 60.0)
    |> svg.stroke("#333")
    |> svg.fill("none")
    |> svg.stroke_width(2.0),
  svg.text(400.0, 350.0, "Hello SVG")
    |> svg.text_anchor("middle")
    |> svg.font_size(24.0),
  svg.path(path.d([
    path.move_to(500.0, 50.0),
    path.cubic(550.0, 20.0, 600.0, 80.0, 650.0, 50.0),
    path.close(),
  ])) |> svg.fill("#E74C3C"),
])

// Write to file or serve over HTTP
fs.write_text("chart.svg", svg.render(chart))
```

## API

### `svg` module

**Element factories** — create SVG elements:

| Function | Description |
|----------|-------------|
| `doc(w, h, children)` | Root `<svg>` with xmlns and viewBox |
| `rect(x, y, w, h)` | Rectangle |
| `circle(cx, cy, r)` | Circle |
| `ellipse(cx, cy, rx, ry)` | Ellipse |
| `line(x1, y1, x2, y2)` | Line |
| `polyline(points)` | Polyline from `List[(Float, Float)]` |
| `polygon(points)` | Closed polygon |
| `path(d)` | Path with `d` attribute |
| `text(x, y, body)` | Text element |
| `group(children)` | `<g>` group |
| `defs(children)` | `<defs>` block |
| `el(tag)` | Arbitrary element |

**Attribute modifiers** — pipe-friendly, return a new `Element`:

| Function | SVG Attribute |
|----------|---------------|
| `fill(el, color)` | `fill` |
| `stroke(el, color)` | `stroke` |
| `stroke_width(el, w)` | `stroke-width` |
| `opacity(el, v)` | `opacity` |
| `transform(el, t)` | `transform` |
| `font_size(el, s)` | `font-size` |
| `font_family(el, f)` | `font-family` |
| `text_anchor(el, a)` | `text-anchor` |
| `rx(el, v)` / `ry(el, v)` | Corner radius |
| `dash_array(el, pattern)` | `stroke-dasharray` |
| `id(el, name)` / `class(el, name)` | `id` / `class` |
| `style(el, css)` | `style` |
| `attr(el, key, value)` | Any attribute |

**Transform helpers** — return transform strings:

```almide
svg.translate(10.0, 20.0)    // "translate(10, 20)"
svg.rotate(45.0)              // "rotate(45)"
svg.scale(2.0, 1.5)           // "scale(2, 1.5)"
```

**Rendering:**

```almide
svg.render(element)  // → SVG string with indentation
```

### `svg.path` module

Build SVG path `d` attribute strings:

```almide
import svg.path

path.d([
  path.move_to(0.0, 0.0),
  path.line_to(100.0, 50.0),
  path.cubic(120.0, 0.0, 180.0, 100.0, 200.0, 50.0),
  path.arc(25.0, 25.0, 0.0, 0, 1, 250.0, 50.0),
  path.close(),
])
// → "M0 0 L100 50 C120 0, 180 100, 200 50 A25 25 0 0 1 250 50 Z"
```

| Function | Command |
|----------|---------|
| `move_to(x, y)` | `M` |
| `line_to(x, y)` | `L` |
| `h_line(x)` | `H` |
| `v_line(y)` | `V` |
| `cubic(x1, y1, x2, y2, x, y)` | `C` |
| `smooth_cubic(x2, y2, x, y)` | `S` |
| `quad(x1, y1, x, y)` | `Q` |
| `smooth_quad(x, y)` | `T` |
| `arc(rx, ry, rot, large, sweep, x, y)` | `A` |
| `close()` | `Z` |
| `d(commands)` | Join commands |
