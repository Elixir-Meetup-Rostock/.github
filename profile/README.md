<!--
**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->

# Hallo 👋 und willkommen zum Elixir Meetup Rostock

Hier findest du Informationen, nützliche Links und alles Weitere, was wir gerne mit der Community teilen möchten.

## 🗓️ Meetups

- 2022-11-21
  - Nützliche Twitter Accounts: [@josevalim](https://twitter.com/josevalim) | [@chris_mccord](https://twitter.com/chris_mccord) | [@elixirstatus](https://twitter.com/elixirstatus)
  - Videos der [Elixir-Conf 2022](https://www.youtube.com/playlist?list=PLqj39LCvnOWbmaPrkGCAzFMC_FYZUkmSr)
  - [Formatting Phoenix HEEx Templates in VS Code](https://pragmaticstudio.com/tutorials/formatting-heex-templates-in-vscode)
  - Vortrag: Phoenix LiveView
    - (Ähnliches gibt's auch für andere Sprachen: https://github.com/dbohdan/liveviews)
    - Empfehlung: [Surface UI](https://surface-ui.org)
- 2022-11-07
  - Vortrag: [Run Elixir on Raspberry Pi](https://github.com/Elixir-Meetup-Rostock/deploy-elixir-raspberry-pi)
  - Austausch / Diskussion zu Code-Snippets
- 2022-10-24
  - Vortrag: Deployment mit [Fly.io](https://fly.io)
  - Vortrag: GenServer

## 🗃️ Code-Snippets

### 💡 Prüfen ob ein Key in einer Map fehlt (mit guards)

```elixir
def check(map) when not is_map_key(map, :foo), do: {:error, :missing, :foo}
def check(map) when not is_map_key(map, :bar), do: {:error, :missing, :bar}
def check(map), do: {:ok}
```

### 💡 Pattern-Matching in Enum-Funktionen

#### 🚫 Schlecht

```elixir
Enum.map(users, fn user ->
  case user do
    %Admin{} -> :admin
    %Customer{} -> :customer
    _user -> :unknown
  end
end)
```

#### ✅ Besser

```elixir
Enum.map(users, fn
  %Admin{} -> :admin
  %Customer{} -> :customer
  _user -> :unknown
end)
```
