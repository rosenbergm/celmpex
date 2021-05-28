---
layout: cover
theme: default
---

# Elixir ⚗️

## Functional language for low-latency services

---
layout: center
---

# Features

---
layout: center
---

![A table of things you can build with Elixir](https://assets.marrrt.in/table.png)

---
---

# Immutability

More control about practically anything because *everything* is stateless.

<v-clicks>

<div>

❌ Wrong

```elixir
map = Map.new
map.put(:name, "David")
# ** (ArgumentError) you attempted to apply a function on %{}.
# Modules (the first argument of apply) must always be an atom
    :erlang.apply(%{}, :put, [:name, "David"])
```

</div>

<div>

✅ Correct

```elixir
map = Map.new
changed_map = Map.put(map, :name, "David")
# %{name: "David"}
```

</div>

</v-clicks>

---
---

# Functional

Pun intended.

```elixir
history =
  Git.log!(repo, [
    "--pretty='%an$<>$%ae$<>$%at$<>$%s'",
    path
  ])
  |> String.split("\n", trim: true)
  |> Enum.map(&String.replace(&1, "'", ""))
  |> Enum.map(fn info ->
    [name, email, timestamp, title] = String.split(info, "$<>$")

    %{
      author: name,
      email: email,
      timestamp: timestamp |> Integer.parse() |> elem(0),
      title: title
    }
  end)
```

---
---

# No statements

Everything is an expression.

```elixir
def ls_r(path \\ ".") do
  cond do
    File.regular?(path) ->
      [path]

    File.dir?(path) ->
      File.ls!(path)
      |> Enum.map(&Path.join(path, &1))
      |> Enum.map(&ls_r/1)
      |> Enum.concat()

    true ->
      []
  end
end
```

---
---

# Concurrency

Multiple processes running side by side to created distributed or stateful systems.

```elixir
defmodule Counter do
  use Agent

  def start_link(_opts) do
    Agent.start_link(fn -> 0 end, name: __MODULE__)
  end

  def get do
    Agent.get(__MODULE__, fn counter -> counter end)
  end

  def increment do
    Agent.update(__MODULE__, fn old_counter -> old_counter + 1 end)
  end
end


```

---
---

# Fault-tolerance

Something *will* fail. And BEAM will save it.

```elixir
children = [
  # Start the Ecto repository
  CmsBackend.Repo,
  # Start the Telemetry supervisor
  CmsBackendWeb.Telemetry,
  # Start the PubSub system
  {Phoenix.PubSub, name: CmsBackend.PubSub},
  # Start the Endpoint (http/https)
  CmsBackendWeb.Endpoint,
  # Facemoji repo watcher
  CmsBackend.FacemojiRepo,
  # The source watcher
  CmsBackend.Sources,
  # Counter from the previous slide
  Counter
]

opts = [strategy: :one_for_one, name: CmsBackend.Supervisor]
Supervisor.start_link(children, opts)
```

<!-- 
:one_for_one - if a child process terminates, only that process is restarted.

:one_for_all - if a child process terminates, all other child processes are terminated and then all child processes (including the terminated one) are restarted.

:rest_for_one - if a child process terminates, the terminated child process and the rest of the children started after it, are terminated and restarted.
-->

---
---

# Rust interop

[Rustler](https://github.com/rusterlium/rustler) allows us to connect Elixir with Rust code using Erlang's NIFs.

<v-clicks>

```rust
#[rustler::nif]
fn add(a: i64, b: i64) -> i64 {
    a + b
}

rustler::init!("Facemoji.Math", [add]);
```

```elixir
defmodule Facemoji.Math do
  use Rustler, otp_app: :math, crate: "facemoji_math"
end

# ...

Facemoji.Math.add(4, 5)
# 9
```

</v-clicks>

---
layout: center
---

# Why to use it?

---
layout: center
---

<v-clicks>

- Fancy
- Quick to learn
- Really fast to code in
- Great concurrency capabilities
- Minimal downtime

</v-clicks>

---
layout: center
---

# Why *not* to use it?

---
layout: center
---

There are none ( ͡° ͜ʖ ͡°)

```elixir
disadvantages = []
```

---
layout: center
---

<v-clicks>

- Community is not as large as in other mainstream langauges **BUT** it is very helpful and is not toxic (erm, JavaScript, ehm)
- Old packages **BUT** their high quality compensates the commit rate
- There are not as many Elixir developers as, let's say, JavaScript developers **BUT** it is easy to teach Elixir to a person

</v-clicks>

---
layout: cover
theme: default
---

# Elm 🌳

## A delightful language for reliable web applications

---
layout: center
---

<v-clicks>

- Type-safe
- Pure
- No runtime exceptions
- JavaScript interop
- Unified code style
- Weird at a first glance but fucking awesome afterwards

</v-clicks>

---
layout: center
---

```elm
-- Show a list of items I need to buy at the grocery store.
--

import Html exposing (..)


main =
  div []
    [ h1 [] [ text "My Grocery List" ]
    , ul []
        [ li [] [ text "Black Beans" ]
        , li [] [ text "Limes" ]
        , li [] [ text "Greek Yogurt" ]
        , li [] [ text "Cilantro" ]
        , li [] [ text "Honey" ]
        , li [] [ text "Sweet Potatoes" ]
        , li [] [ text "Cumin" ]
        , li [] [ text "Chili Powder" ]
        , li [] [ text "Quinoa" ]
        ]
    ]
```

---
layout: center
---

# Some Q's?