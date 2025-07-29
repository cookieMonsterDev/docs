## Run code

To run a code ( where "App.hello" is a path to function in module )

```bash
mix compile && mix run -e "App.hello"
```

Or add main module in `mix.exs` file in root like:

```elixir

  # Run "mix help compile.app" to learn about applications.
  def application do
    [
      mod: {App, []},
      extra_applications: [:logger]
    ]
  end
```

And change the main module to look somehow like: 

```elixir
defmodule App do
  use Application

  def start(_type, _args) do
    # Start the application supervisor
    children = []

    opts = [strategy: :one_for_one, name: App.Supervisor]
    Supervisor.start_link(children, opts)
  end
end
```

Now just use `mix run` in console

## Package manager

Install (for root)

```bash
mix local.hex
```

Now deps can be added to `mix.exs` file

```elixir

  # Run "mix help deps" to learn about dependencies.
  defp deps do
    [
      {:uuid, "~> 1.1"}
      # {:dep_from_hexpm, "~> 0.3.0"},
      # {:dep_from_git, git: "https://github.com/elixir-lang/my_dep.git", tag: "0.1.0"}
    ]
  end
```

then run `mix deps.get`

To import package in code be like:

```elixir
defmodule App do
  use Application
  alias UUID

  def start(_type, _args) do
    IO.puts("start")
    IO.puts("UUID: #{UUID.uuid4()}")
    IO.puts(App.stop())

    # Start the application supervisor

    children = []
    opts = [strategy: :one_for_one, name: App.Supervisor]

    Supervisor.start_link(children, opts)
  end

  def stop() do
    :stop
  end
end
```

[Site of hex](https://hex.pm/)