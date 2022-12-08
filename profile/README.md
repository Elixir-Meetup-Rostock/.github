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

Nächstes [Meetup am 2022-12-19](https://www.meetup.com/altow-academy/events/290042304)

### Letztes Meetup am 2022-12-05

- Einige Menschen haben schon mit [Advent of Code](https://adventofcode.com) rum gespielt
- [Benluc](https://github.com/Benluc) hatte Fragen zur Installation von Elixir unter Windows
  - [scrusi](https://github.com/scrusi) sagte: "Ich habe WSL einfach so wie hier ([Installieren von WSL](https://learn.microsoft.com/de-de/windows/wsl/install)) installiert und habe dann in der ubuntu shell ganz normal asdf / erlang / elixir installiert."
- Ergebnis der letzten Coding-Challenge: [Noah.](https://github.com/NoahLeu) hat uns seinen Stand gezeigt
  - Abfrage der GitHub-API mit HTTPoison und (wegen API-Limit) den Auth-Token mitgeben
- Gemeinsames Mob-Programming
  - Wir haben Prefix-Codes für eine gewisse Sammelaktion generiert und automatisiert geprüft, wie viele Punkte diese bringen
- 👩‍💻 Coding-Challenge für's nächste Mal: Wer schreibt die performanteste Funktion und generiert die meisten Codes?

  - Codes sind 10 Zeichen lang und können aus folgenden Zeichen bestehen: String.graphemes("123456789ABCDEFGHIJKLMNPQRSTUVWXYZ")
  - valide Codes sind z.B. MC29XJMRLY | NXF7KP9RCA | 3L4MJXZ2HW
  - valide Codes beginnen mit einem der folgenden Prefix-Codes:

  ```elixir
  @prefix_codes [
    {"XY", 4}, {"88", 4}, {"44", 4}, {"YK", 3}, {"YE", 3}, {"PM", 3}, {"MC", 3}, {"MA", 3}, {"HJ", 3}, {"9Z", 3}, {"77", 3}, {"3L", 3}, {"33", 3}, {"2F", 3}, {"Z9", 2}, {"XA", 2}, {"X4", 2}, {"W6", 2}, {"TX", 2}, {"RM", 2}, {"RC", 2}, {"NX", 2}, {"MY", 2}, {"LY", 2}, {"JR", 2}, {"EA", 2}, {"AB", 2}, {"9M", 2}, {"7C", 2}, {"6W", 2}, {"66", 2}, {"4L", 2}, {"22", 2}, {"55", 1}, {"11", 1}
  ]
  ```

- Empfehlungen & Links:
  - [Advent of Code](https://adventofcode.com)
  - [Isaac Yonemoto](https://www.youtube.com/@isaacyonemoto6251)
  - [Benchee](https://github.com/bencheeorg/benchee)
  - [Bitstrings](https://elixir-lang.org/getting-started/binaries-strings-and-char-lists.html#bitstrings)

### Vergangene Meetups

[2022-11-21](../meetups/2022-11-21.md) / [2022-11-07](../meetups/2022-11-07.md) / [2022-10-24](../meetups/2022-10-24.md)

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
