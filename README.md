# Nostrand
Standalone runtime environment and REPL for ClojureCLR on Mono.

## Status
Very early, pre-alpha. Everything will change. Don't use for anything critical.

## Installing

```
$ git clone https://github.com/nasser/nostrand.git
$ cd nostrand
$ xbuild
$ ln -s ./bin/Release/nos ~/bin/
$ nos repl
Nostrand 0.0.1.6940 (master/a1e4260* Mon Nov 28 03:51:20 EST 2016)
Mono 4.8.0 (mono-4.8.0-branch/f5fbc32 Mon Nov 14 14:18:03 EST 2016)
Clojure 1.7.0-master-SNAPSHOT
REPL 0.0.0.0:11217
user>
```

## Usage

```
nos [[KEY VALUE]...] FUNCTION [FUNCTION...]
```

Nostrand does two things: it assembles arguments and runs functions.

Arguments are keyword value pairs. They all get merged into a single map that is passed through the functions. They can appear anywhere, and their order does not matter.

Functions are Clojure functions. Without a namespace, they resolve to the `nostrand.tasks` namespace which is built in.

```
$ nos version
Nostrand 0.0.1.6940 (master/a1e4260* Mon Nov 28 03:51:20 EST 2016)
Mono 4.8.0 (mono-4.8.0-branch/f5fbc32 Mon Nov 14 14:18:03 EST 2016)
Clojure 1.7.0-master-SNAPSHOT

$ nos repl
Nostrand 0.0.1.6940 (master/a1e4260* Mon Nov 28 03:51:20 EST 2016)
Mono 4.8.0 (mono-4.8.0-branch/f5fbc32 Mon Nov 14 14:18:03 EST 2016)
Clojure 1.7.0-master-SNAPSHOT
REPL 0.0.0.0:11217
user>
```

With a namespace they are searched for using Clojure's normal namespace resolution machinery. The current directory is on the load path by default.

```
$ cat tasks.clj
(ns tasks)

(defn build []
  (binding [*compile-path* "build"]
    (compile 'important.core)
    (compile 'important.util)))

$ nos tasks/build
```

Command line arguments are passed to the function, and appear as a map.

```
$ cat tasks.clj
(ns tasks)

(defn build [{:keys [utils?]}]
  (binding [*compile-path* "build"]
    (compile 'important.core)
    (when utils?
      (compile 'important.util))))

$ nos tasks/build :utils? true
```

If more functions are specified, they get called in order receiving the result of the previous function as input.

Your entry namespace can also set up your classpath and, soon, your dependencies.  

## Name
[Nostrand Avenue](https://en.wikipedia.org/wiki/Nostrand_Avenue) is a major street and subway stop in Brooklyn near where I was living when I began the project.

## Legal
Copyright © 2016 Ramsey Nasser

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

```
http://www.apache.org/licenses/LICENSE-2.0
```

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.