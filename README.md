# 404skills — Go

A self-contained, fullstack tutorial app for learning Go end-to-end,
built for engineers who already know Node/TypeScript and need to be
**interview-ready for a senior Go role**.

The curriculum walks the language from scratch, then layers on
generics, memory & performance, senior-level pitfalls, and the
algorithms that show up in coding interviews. Each lesson has a TS
comparison where helpful, an editable example, and runs `go run` on
your machine so the code is real, not simulated.

The backend is written in idiomatic Go using **only the standard
library** so you can read its source as a second tutorial. The
frontend is plain HTML / CSS / vanilla JS, no build step.

```
┌──────────────────────────────────────────────────────────────────┐
│  Sidebar lessons          │  Lesson description + key takeaways  │
│  Search / progress bar    │                                      │
│                           │  ┌────────────────────────────────┐  │
│   ✓ Welcome & Setup       │  │  Editable Go code              │  │
│   ○ Variables & Zero…     │  │  (real go run on backend)      │  │
│   ○ Constants & iota      │  └────────────────────────────────┘  │
│   ○ ...                   │  ┌────────────────────────────────┐  │
│                           │  │  Output / stderr / errors      │  │
│                           │  └────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

## Run it

You need [Go 1.22+](https://go.dev/dl/) installed. The curriculum uses
generics (1.18), `errors.Join` (1.20), `log/slog` + `slices` + `maps` +
`cmp.Ordered` (1.21), and the for-loop variable scoping fix (1.22).
Latest stable is recommended.

```bash
go run .
# Go tutorial running at http://localhost:8080
```

Open <http://localhost:8080> in your browser.

### Optional flags

```bash
go run . -addr :9000      # bind to a different port
```

### Ask AI (optional)

Each lesson has a floating **Ask AI** button that opens a chat panel. The
question is sent to an LLM along with the current lesson's title,
description, code example, key takeaways, AND whatever you've typed in
the editor, so you can ask things like *"why does my version panic?"*
and get a specific answer.

The server auto-detects which provider to use from your environment, in
this priority order: **Google → Anthropic → OpenAI**.

The easiest way to set things up is via a `.env` file. Copy the example
and fill in one key:

```bash
cp .env.example .env
# edit .env and paste your key
go run .
```

A `.env` file in the project root is loaded automatically on startup.
Get a free Gemini key at <https://aistudio.google.com/apikey>.

Real environment variables always win over the `.env` file, so you can
also do either of these without touching the file:

```bash
# One-shot for a single run:
GEMINI_API_KEY=AIza… go run .

# Or shell-export:
export GEMINI_API_KEY=AIza…
go run .
```

Other supported variables:

| Variable | Default | Notes |
| --- | --- | --- |
| `GEMINI_API_KEY` | _none_ | Google Gemini (preferred) |
| `GEMINI_MODEL` | `gemini-2.5-flash` | e.g. `gemini-2.5-pro` |
| `ANTHROPIC_API_KEY` | _none_ | Anthropic Claude |
| `ANTHROPIC_MODEL` | `claude-haiku-4-5` | |
| `OPENAI_API_KEY` | _none_ | OpenAI |
| `OPENAI_MODEL` | `gpt-4o-mini` | |

### Chat abuse protection

When AI chat is enabled on a public deployment, these limits apply per client IP
(Cloud Run sets `X-Forwarded-For` automatically; set `TRUST_PROXY=1` behind other
reverse proxies):

| Variable | Default | Notes |
| --- | --- | --- |
| `CHAT_RATE_PER_MIN` | `5` | Burst limit; set `0` to disable |
| `CHAT_RATE_DAILY` | `50` | Daily quota per IP; set `0` to disable |
| `CHAT_MAX_MESSAGE_CHARS` | `4000` | Max characters per message |
| `CHAT_MAX_CODE_CHARS` | `16000` | Max editor code sent with each turn |
| `CHAT_MAX_BODY_BYTES` | `65536` | Max JSON body size for `/api/chat` |

Exceeded limits return HTTP 429 with a `Retry-After` header. Limits are also
exposed via `GET /api/chat/status` under `limits`.

On startup the server logs which provider is active:

```
AI chat enabled via google (gemini-2.5-flash)
Go tutorial running at http://localhost:8080
```

If no key is set, the chat panel still opens but explains how to
configure one. Everything else in the app still works without an
API key.

Chat history is kept **per lesson, in memory only**: switch lessons and
you get a fresh thread; the **trash icon** clears the current thread.
Press `Esc` or click the **×** to close the panel.

## Curriculum

The curriculum is "zero → interview-ready for a senior Go role." It
assumes you already write Node/TypeScript at a senior level. Each
lesson is editable and runs `go run` for real on your machine.

The 59 lessons are grouped into **15 sections**. The first 10 sections
teach the language end-to-end; the last 5 are the senior / interview
track.

### Foundations

| Section | Lessons |
| --- | --- |
| **Basics** | Welcome & Setup · Coming from TypeScript: Mental Model · Variables & Zero Values · Constants & iota · Basic Types & Conversions |
| **Control Flow** | if / for / switch · Functions & Multiple Returns · Functional Options Pattern · Closures · defer / panic / recover |
| **Data Structures** | Arrays & Slices · Maps · Pointers · Structs |
| **Methods & Interfaces** | Methods (value vs pointer receivers) · Interfaces · Embedding & Composition · Type Assertions & Type Switches |
| **Errors** | Errors, Wrapping, errors.Is/As · Error Design Patterns (sentinel / typed / opaque) |
| **Concurrency** | Goroutines · Channels · select & Timeouts · sync (Mutex/WaitGroup/Once) · context · Worker Pools · errgroup · Race Conditions & the Race Detector |
| **Tooling & Packages** | Packages & Modules · go fmt/vet/build/test/run · Writing Tests · Benchmarks & Profiling |
| **Standard Library** | Files & io.Reader/Writer · encoding/json · time · log/slog (structured logging) |
| **Web Development** | HTTP Server with net/http · HTTP Client & JSON APIs · Middleware Composition · Production HTTP Server (timeouts, graceful shutdown) |
| **Ecosystem** | Frameworks: Gin, Echo, Fiber, Chi · What Go Is Great At |

### Senior / Interview Track

| Section | Lessons |
| --- | --- |
| **Generics** | Type Parameters · Constraints & Patterns (any / comparable / cmp.Ordered / ~T) |
| **Memory & Performance** | Value vs Pointer Semantics · Escape Analysis & Allocations · sync.Pool & Object Reuse |
| **Senior Pitfalls** | The Typed Nil Trap · Loop Variable Capture & Slice Aliasing |
| **Interview Algorithms** | Two Pointers · Sliding Window · Binary Search & Variants · Backtracking · Dynamic Programming · Graph Traversal (BFS/DFS) · Heaps & Top-K · Linked List Patterns · LRU Cache (design classic) |
| **Interview Prep** | Senior Go Interview Cheatsheet |

Progress is tracked in `localStorage`. Use the "Reset progress" button
in the sidebar to clear it.

## Project layout: read this as a second lesson

The codebase is organized by **domain**, not by technical layer.  Each
internal package owns its types AND its access layer; the `api` package
sits on top and depends on all the domains, but no domain depends on
HTTP.  `main.go` is just the composition root.

```
golang-tut/
├── go.mod              # module definition (Go 1.22+, no external deps)
├── main.go             # composition root: load config, wire deps, start server
├── Dockerfile          # multi-stage build, distroless runtime (~8 MB)
├── .dockerignore
│
├── internal/
│   ├── config/         # .env loader
│   │   └── dotenv.go
│   │
│   ├── lesson/         # lesson domain. Lesson type + Repository port + catalog.
│   │   ├── lesson.go              # Lesson, Summary, Repository, InMemoryRepository
│   │   ├── catalog.go             # composes per-category slices into the ordered Catalog
│   │   ├── lessons_basics.go      # Foundations: Basics → Ecosystem (one file per category)
│   │   ├── lessons_control_flow.go
│   │   ├── lessons_data_structures.go
│   │   ├── lessons_methods.go
│   │   ├── lessons_errors.go
│   │   ├── lessons_concurrency.go
│   │   ├── lessons_tooling.go
│   │   ├── lessons_stdlib.go
│   │   ├── lessons_web.go
│   │   ├── lessons_ecosystem.go
│   │   ├── lessons_generics.go    # Senior track: Generics → Interview Prep
│   │   ├── lessons_memory.go
│   │   ├── lessons_pitfalls.go
│   │   ├── lessons_algorithms.go
│   │   └── lessons_interview_prep.go
│   │
│   ├── runner/         # pluggable code sandbox
│   │   ├── runner.go        # Runner interface + NewFromEnv selector
│   │   ├── local.go         # `go run` in a temp dir (dev default)
│   │   └── playground.go    # POST to go.dev/_/compile (cloud default)
│   │
│   ├── tutor/          # LLM chat domain
│   │   ├── tutor.go         # Service + system-prompt builder + stripHTML
│   │   ├── provider.go      # Provider interface + SelectFromEnv
│   │   ├── httpx.go         # shared postJSON helper (DRY across providers)
│   │   ├── gemini.go        # Google Gemini adapter
│   │   ├── anthropic.go     # Anthropic Claude adapter
│   │   └── openai.go        # OpenAI adapter
│   │
│   └── api/            # HTTP transport, no business logic here
│       ├── server.go        # Server struct, routing, request-log middleware
│       ├── json.go          # writeJSON helper
│       ├── lessons.go       # GET /api/lessons{,/{id}}
│       ├── run.go           # POST /api/run
│       └── chat.go          # POST /api/chat, GET /api/chat/status
│
└── web/                # frontend (embedded into the binary)
    ├── index.html
    ├── styles/
    │   ├── tokens.css       # design tokens (light + dark theme)
    │   ├── base.css         # resets, page grid
    │   ├── components.css   # shared buttons + icon-btn
    │   ├── sidebar.css
    │   ├── lesson.css
    │   ├── playground.css
    │   └── chat.css
    └── js/                  # ES modules, no build step
        ├── app.js           # composition root: imports + init order
        ├── state.js         # shared state container + localStorage
        ├── dom.js           # $ / $$ query helpers
        ├── api.js           # all fetch() calls live here
        ├── theme.js
        ├── lessons.js       # sidebar + nav + render + progress
        ├── playground.js    # editor + run + output
        ├── chat.js          # chat panel + per-lesson history
        └── markdown.js      # safe md subset for chat replies
```

**Highlights worth opening in your editor:**

- `main.go`: pure wiring. `embed.FS`, dependency construction, `http.Server` setup.
- `internal/lesson/lesson.go`: the `Repository` interface pattern.  Today
  it's `InMemoryRepository`; tomorrow you could add a `FileRepository`
  without changing the API layer.
- `internal/lesson/catalog.go`: single composition point for lesson order.
  Each `lessons_<category>.go` exposes a private `[]Lesson` slice; the
  catalog stitches them in pedagogical order via a `concat` helper.
- `internal/tutor/provider.go` + `httpx.go`: the `Provider` port and the
  one `postJSON` helper that all three LLM adapters share.  Compare the
  three provider files to see how the protocol-specific differences are
  isolated.
- `internal/runner/`: pluggable sandbox. `runner.go` defines the
  `Runner` interface; `local.go` shells out to `go run` (dev default);
  `playground.go` POSTs to the Go Playground API (cloud default).
  Pick at startup with `RUNNER=local|playground` or rely on auto-detect.
- `web/js/app.js`: the frontend composition root mirrors the backend.

## Build a single binary

```bash
go build -o gotut .
./gotut
```

Cross-compile for another platform (no Docker, no toolchain installs):

```bash
GOOS=linux GOARCH=amd64 go build -o gotut-linux .
```

The web assets are baked into the binary via `//go:embed web/*`.

## Code sandbox (Local vs Playground)

The `/api/run` endpoint executes user-submitted Go code through a
pluggable backend.  Pick one with the `RUNNER` environment variable:

| `RUNNER`     | Backend                          | Use for       | Safety |
| ---          | ---                              | ---           | ---    |
| `local`      | `go run` in a temp dir           | Local dev     | UNSAFE in public |
| `playground` | POST to `https://go.dev/_/compile` | Cloud deploys | Safe — Google's sandbox |
| unset        | Auto: `playground` if `$K_SERVICE` is set (Cloud Run / Knative), else `local` | Anywhere | Picks the safe default |

**Local backend.** Convenient on a laptop, but it runs untrusted code
with the same UID, filesystem, and network access as the server
process.  It's only suitable for `go run .` on your own machine.  Do
not expose a Local-backend instance to the public internet.

**Playground backend.** Sends the source to Google's open-source
Playground service (the same one that powers <https://go.dev/play>).
User code runs inside a hardened VM with no network, no filesystem
beyond `/tmp`, and a hard ~5s wall-clock limit.  This is the only safe
choice for a public-facing deployment.  Override the endpoint with
`PLAYGROUND_URL` if you self-host the playground (the source is at
[golang/playground](https://github.com/golang/playground)).

## Deploy to Google Cloud Run

A multi-stage `Dockerfile` produces a ~8 MB distroless image that
defaults to the Playground runner, listens on `$PORT`, exposes
`/healthz` for probes, and shuts down gracefully on `SIGTERM`.

One-command deploy (uses Cloud Build under the hood, no local Docker
needed):

```bash
gcloud run deploy golang-tut \
  --source . \
  --region us-central1 \
  --allow-unauthenticated
```

If you want LLM-powered "Ask AI", pass the key as a secret-backed env
var (the example uses Gemini; swap in `ANTHROPIC_API_KEY` or
`OPENAI_API_KEY` if you prefer):

```bash
# Store the key in Secret Manager:
echo -n "AIza..." | gcloud secrets create gemini-key --data-file=-

# Reference it on deploy:
gcloud run deploy golang-tut \
  --source . \
  --region us-central1 \
  --allow-unauthenticated \
  --update-secrets=GEMINI_API_KEY=gemini-key:latest
```

The default `RUNNER=playground` is set in the Dockerfile, and the
in-app auto-detection (via `$K_SERVICE`) would pick Playground anyway
if it weren't.

### Manual Docker workflow

If you'd rather build and push the image yourself:

```bash
docker build -t gcr.io/$(gcloud config get-value project)/golang-tut .
docker push  gcr.io/$(gcloud config get-value project)/golang-tut

gcloud run deploy golang-tut \
  --image gcr.io/$(gcloud config get-value project)/golang-tut \
  --region us-central1 \
  --allow-unauthenticated
```

### Run the container locally

```bash
docker build -t golang-tut .
docker run --rm -p 8080:8080 golang-tut
# open http://localhost:8080
```

The image runs as non-root, ignores the bundled `.env` (use
`-e GEMINI_API_KEY=...` instead), and defaults to the Playground
runner.

## Going further

After you finish the lessons, try these projects to cement the language:

1. **JSON CRUD service**: chi router + sqlc + Postgres. Add tests with
   `httptest`.
2. **CLI tool**: cobra + viper. Cross-compile to Linux / macOS / Windows.
3. **Concurrent worker**: fan-out/fan-in over channels, with
   `context.Context` cancellation.
4. **gRPC service**: `protoc` + `grpc-go`, then add interceptors.

Recommended reading:

- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)
- [Standard library reference](https://pkg.go.dev/std)
- [Go by Example](https://gobyexample.com/) (pairs nicely with this app).

## License

[MIT](./LICENSE) © 2026 0xm1kr
