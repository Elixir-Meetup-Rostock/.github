# Surface UI Framework - Elixir

Surface UI: [https://surface-ui.org](https://surface-ui.org)

Diese Seite soll eine kurze Einführung in den Surface UI Framework für Elixir - Phoenix Projekte geben, um sich schnell innerhalb des Frameworks zurecht zu finden. Dafür wurden hier die wichtigsten Features des Frameworks (teils im Vergleich zu Phoenix LiveView) dargestellt.

- [TL;DR](#Elixir-SurfaceUIFramework-TL;DR)
- [Was ist Surface UI?](#Elixir-SurfaceUIFramework-WasistSurfaceUI?)
- [Strukturelle Änderungen](#Elixir-SurfaceUIFramework-StrukturelleÄnderungen)
- [Components](#Elixir-SurfaceUIFramework-Components)
- [Context (“globales State”)](<#Elixir-SurfaceUIFramework-Context(“globalesState”)>)
- [JS Interoperability (JavaScript Hooks)](<#Elixir-SurfaceUIFramework-JSInteroperability(JavaScriptHooks)>)
- [Scoped CSS](#Elixir-SurfaceUIFramework-ScopedCSS)
- [Testing](#Elixir-SurfaceUIFramework-Testing)
- [Good to Know](#Elixir-SurfaceUIFramework-GoodtoKnow)
- [Größte Unterschiede Surface vs. Phoenix LiveView](#Elixir-SurfaceUIFramework-GrößteUnterschiedeSurfacevs.PhoenixLiveView)
- [Erster Eindruck aus Entwicklersicht](#Elixir-SurfaceUIFramework-ErsterEindruckausEntwicklersicht)

## TL;DR

- Surface UI erleichtert die Entwicklung von interaktiven Websites / Webapps durch das Einsparen von Logik zwischen Komponenten / ggf. intern weniger Kommunikationslogik

- erlaubt subjektiv gesehen angenehmere Syntax in Templates (wenn man React gewohnt ist)

## Was ist Surface UI?

- UI Framework für Phoenix LiveView (baut also auf LiveView auf und erweitert es)

- Server Side Rendering (SSR) Framework (Phoenix LiveView als Basis ist SSR)

- Ermöglicht interaktive UIs mit wenig Code

## Strukturelle Änderungen

- File-Endungen: page.ex und page.**sface** (sface für templates)

- **Template Syntax:**

  - neues Sigil “~F”

  - ```java
    def render(assigns) do
      ~F"""
        <ExampleComponent>
          Hi there!
        </ExampleComponent>
      """
    end
    ```

- Komponenten in Templates einbinden

  - Phoenix LiveComponents

    ```java
    <.live_component module={CardComponent} title={"Card"} id={card.id} />
    ```

  - Surface UI:

    ```java
    <CardComponent title="Card" id={card.id} />
    ```

- Blöcke im Template (for, if, unless, case, else, …)

  ```java
  {#if @item != nil}
    Item existiert!
  {#else}
    Item existiert nicht!
  {/if}

  {#for item <- @items}
    ...
  {/for}

  {#case @value}
    {#match [first|_]}
      First {first}
    {#match []}
      Value is empty
    {#match _}
      Value is something else
  {/case}
  ```

## Components

- wichtige Aspekte:

  - props

  - data assigns

  - slots

  - events

- **Properties** (declarative properties)

  ```java
  @doc "The type (color) of the button"
   prop type, :string, values: ["primary", "success", "info"]

   @doc "The Button is expanded (full-width)"
   prop expanded, :boolean, default: false

   @doc "Triggers on click"
   prop click, :event, required: false

   @doc "Triggers on focus"
   prop focus, :event, required: false
  ```

  ```java
  <ComponentXYZ type="primary" expanded={true} />
  ```

- **Data** (assign values) gute Übersicht über relevante assigns (+ Datentyp, Wertebereich, …)

  ```java
   data count, :integer, default: 0
  ```

- **Slots** (dynamische children für Komponenten)

  ```java
  defmodule Hero do
    use Surface.Component

    @doc "The content of the Hero"
    slot default, required: true

    def render(assigns) do
      ~F"""
      <section class="hero is-info">
        <div class="hero-body">
          <#slot />
        </div>
      </section>
      """
    end
  end
  ```

- **Events** (innerhalb der Komponente)

  ```java
  defmodule Counter do
    use Surface.LiveComponent

    data count, :integer, default: 0

    def render(assigns) do
      ~F"""
      <div>
        <h1 class="title">
          {@count}
        </h1>
        <div>
          <button class="button is-info" :on-click="dec">-</button>
          <button class="button is-info" :on-click="inc">+</button>
          <button class="button is-danger" :on-click="reset">Reset</button>
        </div>
      </div>
      """
    end

    # Event handlers

    def handle_event("inc", _, socket) do
      {:noreply, update(socket, :count, &(&1 + 1))}
    end

    def handle_event("dec", _, socket) do
      {:noreply, update(socket, :count, &(&1 - 1))}
    end

    def handle_event("reset", _, socket) do
      {:noreply, assign(socket, :count, 0)}
    end
  end
  ```

## Context (“globales State”)

Erlaubt das Anlegen von globalen assigns die von verschiedenen Komponenten unabhängig von deren Beziehung zueinander (parent - child) gesetzt und gelesen werden können

```java
# set context for timezone

def mount(_params, _session, socket) do
  socket = Context.put(socket, timezone: "UTC")
  {:ok, socket}
end
```

```java
# retrieve context for timezone

data timezone, :string, from_context: :timezone

def render(assigns) do
  ~F"""
  <h1>Timezone: {@timezone}<h1>
  """
end
```

## JS Interoperability (JavaScript Hooks)

- Anbindung von JavaScript Hooks wie z.B. scroll-basierten Events auf der Client-Side nicht mehr global ans Socket angeknüpft sondern an eine Komponente

- Hooks können scoped für bestimmte Komponenten gesetzt werden

- Umsetzung dafür: collocated hook file

  - card.ex

  - card.sface

  - card.hook.js

## Scoped CSS

- automatisches Scoping von CSS (auf z.B. eine Komponente)

- Umsetzung: collocated css file

  - card.ex

  - card.sface

  - card.css

## Testing

Makro zum Testen von Komponenten selbstständig (ohne LiveView als parent)

```java
render_surface/1
```

```java
# Import conveniences for testing
use Surface.LiveViewTest

# The default endpoint for testing
@endpoint Endpoint

test "creates a <button> with class" do
  html =
    render_surface do
      ~F"""
      <Button class="btn">
        Ok
      </Button>
      """
    end

  assert html =~ """
        <button type="button" class="btn">
          Ok
        </button>
        """
end
```

## Good to Know

- **UI Component Library** - Ansammlung von Komponenten die Schreibweisen / Komponenten aus Phoenix LiveView in Surface implementieren:

  - Link (vgl. Phoenix `link/2`)

  - Form (vgl. `Phoenix.HTML.Form.form_for/3`)

  - Button

  - Select

  - Input

  - …

- **Compile-Time checking** - Automatisches Prüfen von Templates, Components, props, events, slots, … on compile

## Größte Unterschiede Surface vs. Phoenix LiveView

- Events aus Components werden automatisch innerhalb der Komponente behandelt. Sie müssen anders als bei Phoenix LiveView nicht bis zum LiveView “hochgereicht” und wieder als Ergebnis in die Komponente gegeben werden. In Phoenix LiveView müssen dabei Attribute wie `phx-[target]` gesetzt werden

- strukturelle Änderungen vereinfachen komponentenbasiertes Entwickeln

- Data assigns innerhalb von Komponenten bieten mehr Übersicht über deren State / Attribute

- für Entwickler mit React-Hintergrund durch Syntaxänderungen (s.o.) intuitivere Entwicklung möglich

## Erster Eindruck aus Entwicklersicht

- relativ einfache Umstellung von Phoenix LiveView zu Surface UI (Grundlagen ähnlich)

- abgekapselte Komponenten leicht wiederverwendbar → alle Informationen zur Funktionalität der Komponente an einem Ort

- Syntax sehr angenehm

  - spart Code ein

  - ermöglicht schnelles erkennen von Zusammenhängen (props, data, …)

- kleinere LiveViews möglich
