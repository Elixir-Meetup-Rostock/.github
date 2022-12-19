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


### Vergangene Meetups

[2022-12-05](../meetups/2022-12-05.md) / [2022-11-21](../meetups/2022-11-21.md) / [2022-11-07](../meetups/2022-11-07.md) / [2022-10-24](../meetups/2022-10-24.md)

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
