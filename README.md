# ring-logger-onelog

OneLog implementation for [ring-logger](https://github.com/nberger/ring-logger)

Migration from ring.middleware.logger
-------------------------------------

The migration is pretty straightforward:

* Replace dependency in `project.clj` from `[ring.middleware.logger "0.5.0"]` to `[ring-logger-onelog "0.7.0-SNAPSHOT"]`
* Replace the require from `[ring.middleware.logger :as logger]` to `[ring.logger.onelog :as logger]`
* Pass options to `wrap-with-logger` using a proper map instead of keyword arguments.

## Usage

In your `project.clj`, add the following dependencies:

```clojure
    [ring-logger-onelog "0.7.0-SNAPSHOT"]
```

Add the middleware to your stack, using the onelog implementation. It's similar to
using the default ring-logger, but requiring the onelog namespace:

```clojure
    (ns foo
      (:require [ring.adapter.jetty :as jetty]
                [ring.logger.onelog :as logger.onelog]))

    (defn my-ring-app [request]
         {:status 200
          :headers {"Content-Type" "text/html"}
          :body "Hello world!"})

    (jetty/run-jetty (logger.onelog/wrap-with-logger my-ring-app {:port 8080}))
```

Differences with plain ring-logger
---------------------------------

Each request is assigned a random 4-hex-digit ID, so that different log messages pertaining to the same request can be cross-referenced.
These IDs are printed in random ANSI colors by default, for easy visual correlation of log messages while reading a log file.

Log Levels with OneLog
----------

The logger logs at `INFO` level by default. More verbose information is logged when the logger is at `DEBUG` level.

ring-logger-onelog uses [OneLog](https://github.com/pjlegato/onelog) internally, so we can use
OneLog's convenience methods to change the log level:


```clojure
(onelog.core/set-debug!)
(onelog.core/set-info!)
(onelog.core/set-warn!)
```


## License

Copyright © 2015 Nicolás Berger, 2012-2014 Paul Legato

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.