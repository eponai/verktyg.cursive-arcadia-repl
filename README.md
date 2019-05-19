# tools.cursive-arcadia-repl
An [nREPL] bridge for [Arcadia Unity]. Heavily inspired by [arcadia.nrepl](https://github.com/spacepluk/arcadia.nrepl),
with the difference of redirecting eval operations to Arcadia's nREPL rather than its UDP REPL. Cursive specific operations
are not redirected but evaluated in the bridge as they require the JVM.

[nREPL]: https://github.com/clojure/tools.nrepl
[Arcadia Unity]: https://github.com/arcadia-unity/Arcadia

## Prerequisites

* Install [Clojure CLI](https://clojure.org/guides/getting_started) (`brew install clojure` on Mac)
* Install [Intellij](https://www.jetbrains.com/idea/download/) and [Cursive](https://cursive-ide.com)
* [Setup an Arcadia project](https://github.com/arcadia-unity/Arcadia/wiki/Getting-Started)

## How to use Cursive with Arcadia

Having installed the prerequisites, follow these steps to setup a Cursive->Arcadia REPL.

1. Open your Unity project and verify that Arcadia is running. You should see the message "Arcadia Started!" in Unity's console.

2. Start the nREPL bridge provided by this library on default port `7888`.

    ```sh
    git clone git@github.com:eponai/tools.cursive-arcadia-repl.git
    cd tools.cursive-arcadia-repl
    clj -A:repl
    ```

3. Create a new clojure project with a deps.edn file in your Unity project. For example:

    ```sh
    cd <your-unity-project>/Assets
    mkdir <project-name> && cd <project-name>
    mkdir src test target
    touch deps.edn
    ```

4. Add the `Assets/Arcadia` directory to your `deps.edn` as a dependency:

    ```clj
    {:deps {org.clojure/clojure {:mvn/version "1.10.0"}
            arcadia             {:local/root "../Arcadia"}}}
    ```

5. In Intellij/Cursive, open and import the `Assets/<project-name>` directory.

6. Create a [remote REPL in Cursive](https://cursive-ide.com/userguide/repl.html#remote-repls) on port `7888`.

Forms typed into the remote REPL should be evaluated in Unity!

### Testing the setup

Here's some code you can use to test whether it's working correctly:

```clj
(ns <project>.core
  (:require
    [arcadia.core :as arc]
    [clojure.clr.io :as io]))

(comment
  ;; Send this form to the remote REPL (default key binding cmd+shift+p)
  (arc/log "Hi unity!")

  ;; You should see this message being printed in Unity's console!

  ;; Testing Clojure CLR code
  (io/as-file "some.file")
  ;; => #object[FileInfo 0xe64d6600 "some.file"]
  )
```

And you're done!
