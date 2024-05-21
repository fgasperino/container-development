# Containerized Development

The suggested method for development is to run a container with required JVM, 
JDK, and Clojure tooling. To do this, build a custom docker image suitable 
for clojure REPL interaction.

## Dockerfile

The Dockerfile contains a minimal set of parameters required to begin 
containerized development.

## Build
```
  docker \
    build \
      --rm \
      --force-rm \
      -t clojure-dev:0.0 \
    .
```

## Run

Start the configured containers:

```
  env \
    PROJECT_HOME=/path/to/project \
  docker-compose up -d
```

In a new terminal, connect to the running container. This will allow
you to confirm the volume mapping is successful as well as starting the
Clojure REPL process.

```
  docker exec -it clojure-dev-1 /bin/bash
```

Start the Clojure REPL process:

```
  cd project && clj ...
```

## Neovim (local)

  ```neovim```

  It may install neovim plugins and lsp clients/servers first run.

## Connect (remote)

  Edit the docker-compose.yml file to add an exposed port to the
  container configuration.

  ```
  services:
    clojure_dev:
      ports:
        - 0.0.0.0:${REPL_PORT}:${REPL_PORT}
  ```

  If using clojure deps (deps.edn) tooling, add an alias in your ~/.clojure/deps.edn
  file for a socket nREPL alias.

  ```
  {:aliases
   {:nrepl
    {:extra-deps {nrepl/nrepl {:mvn/version "1.0.0"}
                  cider/cider-nrepl {:mvn/version "0.48.1"}}
     :main-opts ["-m" "nrepl.cmdline"
                 "--middleware" "[cider.nrepl/cider-middleware]"]}}}
  ```

  Run the clojure repl with the alias:

  ```
  clj -M:nrepl
  ```

  
