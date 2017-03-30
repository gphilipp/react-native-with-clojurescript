# React Native With ClojureScript


This repo is the result from my experimentations writing a native iOS application using react-native with Clojurescript.
I've tried re-natal with om-next first, but found it quite difficult to use due to the lack of documentation. Then I tried re-natal with re-frame and found that I got something working more rapidly.

Below is a collection of links, interesting ramblings on Slack from the #cljsrn channel. I'll try to add simple examples to get started and keep it up-to-date with newest versions of react-native and re-natal.


## Tutorials
- http://cljsrn.org/, talks and videos section
- https://github.com/rockiger/re-natal-tutorial

## Example apps
- https://github.com/Slowyn/DevDayToDo
- https://github.com/areina/elfeed-cljsrn
- https://github.com/madvas/catlantis
- https://github.com/drapanjanas/pneumatic-tubes/tree/master/examples/group-chat-app (web socket w/o sente, w/ http-kit)
- https://github.com/Trustroots/Trustroots-React-Native
- https://github.com/alwx/luno-react-native + [Experience Report](https://medium.com/@alwxdev/react-native-summing-up-86838441d289) 
- https://github.com/tiensonqin/exponent-cljs-template (examples for nav, supports om-next and re-frame)
- https://github.com/Quantisan/re-frame-firebase-example (firebase + re-frame)

## Navigation
- https://github.com/vikeri/re-navigate
- https://github.com/weltan/navigation (building on the first for a more substantive example)
- https://github.com/wix/react-native-navigation/
    > @seantempesta yeah, I just loaded up the example app in react-native-navigation and it’s buttery smooth
- Example : https://github.com/savelichalex/Souptitle/blob/master/src/teach_by_friends/shared/ui.cljs#L75-L99
- Example from lymchat app: https://github.com/tiensonqin/lymchat-exp/blob/master/src/lymchat/util.cljs#L238-L255
NavigationExperimental can work quite well with re-frame, a minimal example here: https://github.com/vikeri/re-navigate

## Run on device
1. Get IP address from iphone settings
2. Run `re-natal use-ios-device` with that IP address.
3. Run `lein prod-build`
4. Open Xcode, changed scheme configuration from _Debug_ to _Release_, change platform to _physical device_
5. Hit Run on Xcode.


## Debugging
- shake the device simulator -> enable browser debugging. It'll print exceptions in the console. some will have only js code "lines", some will have cljs ones (https://www.jetbrains.com/help/idea/2016.1/debugging-javascript.html?origin=old_help)
- Device log: `react-native log-ios`

## Om-next
- Writing om-next reloadable code : https://anmonteiro.com/2016/01/writing-om-next-reloadable-code-a-checklist/


## Re-natal Hack
See the support directory and the build.boot file: https://github.com/kennyjwilli/postal-app

## Re-natal import external components
- `react-native link react-native-sound && npm install react-native-sound —save && re-natal use-component react-native-sound`
- Optionally : `re-natal use-figwheel`

## Shipping
- fastlane allows you to push to 
flight, using a tool called pilot
- Blog post: http://blog.thebakery.io/continuous-integration-for-react-native-applications-with-fastlane-and-bitrise-ios-version/
- https://facebook.github.io/react-native/docs/running-on-device-ios.html#building-your-app-for-production

> @savelichalex: continuous integration that pushes every commit to master to testflight is a total life-saver you can build the archive e.g. using xctool


## Testing

> etherfuse 08:26:28
also, when writing tests, what do you normally use? clojure.test or javascript frameworks? I’ve seen both, so I’m curious what the most common scenario is ?

>vikeri 09:01:28 @etherfuse Have you followed this tutorial: http://presumably.de/boot-react-native.html ? We are using doo + re-frame-test for testing. I talk a little about it here: https://youtu.be/6IYm34nDL64?t=9m3s (didn’t use re-frame-test then though.)


> @vikeri: Seems testing is not really a priority at facebook. I am testing my re-frame logic and has worked with react-native-mock to test the components in node. Works alright.

> @seantempesta
No idea if it works with RN, but I just attended a talk on iOS testing where this guy did 2 months of research and he recommended Calabash. http://calaba.sh/

> @artemyarulin As far as I remember it’s about UI testing first of all. in RN world there is snapshot testing which is also quiet interesting https://facebook.github.io/jest/blog/2016/07/27/jest-14.html

@vikeri uses https://github.com/airbnb/enzyme to test his components with enzymes' `render`function.

> @ilmirajat (author of https://github.com/Trustroots/Trustroots-React-Native)
Btw. do any of you have good refence how unit tests code properly in Cljsrn. The best reference I've found, is this https://github.com/futurice/pepperoni-app-kit. It uses Enzyme (https://github.com/airbnb/enzyme) and React-native-mock for mocking hardware. It was pure pain to get unit tests work reasonably well, and I'm not quite satisfied to my solution.

> artemyarulin We’ve discusses testing theme a bit before here - nothing “production ready”. I guess official RN way would be https://facebook.github.io/jest/blog/2016/07/27/jest-14.html, although now it’s experimental

> @pesterhazy for UI work I'm not convinced unit tests are very helpful. I mean most of the complexity in UIs is in the interconnection of components, and in the interplay with the runtime (browser or RN)

> @ilmirajat I also decided to use Node instead of phantomjs because I wanted to ensure that npm dependencies don't use things (e.g.window object) that React Native doesn't have.

## Logging
- Use `react-native log-ios` in a separate terminal (cluttered with random, useless messages that it's really hard to read says @pesterhazy)

## Building

Avanced compilation for production requires rn-externs: https://github.com/artemyarulin/react-native-externs
> @artemyarulin: well, this is a way how advanced compilation works - without externs file Google Closure rename a lot of important staff 
Then @pesterhazy says that advanced compilation is not necessarily worth the effort on react native because bundle size doesn't matter as much as on the web. @artemyarulin replies that there are still to cases for that: 1) If you want to do hot deploys and do it daily or more often (like we used to do in web) and 2) GC is best obfuscator - not a big deal for certain cases, but it drives me crazy a bit that somebody can unpack bundle and recreate the app in 10 minutes. @pesterhazy agrees and adds that there might be speed bumps too.
(https://clojurians-log.clojureverse.org/cljsrn/2016-08-05.html)

## Frameworks 

## Reagent

Best link: https://reagent-project.github.io/

## Re-frame
https://github.com/Day8/re-frame

## Performance
https://github.com/seantempesta/cljsrn-re-frame-workers


## Boot-react-native
@pesterhazy: one thing that is great is that brn integrates with RN's packager directly, so you get all its features. including pretty fast reloading, requiring images just works
@pesterhazy: boot-react-native badly needs updating, code and docs

Blog post of Aug 2016: http://presumably.de/boot-react-native.html

Troubleshooting: https://github.com/mjmeintjes/boot-react-native/wiki/Troubleshooting

Proper restart
- restart the app (not relying on boot-reload)
- clearing the packager cache `react-native start --refresh-cache true`
- check the bundle output `http://localhost:8081/index.ios.bundle?platform=ios&dev=true&hot=true`


## Fetching Data

```clojure
(def fetch (.-fetch js/window)) (register-handler :load-data (fn [db _] (.then (fetch "[https://api.github.com/repositories](https://api.github.com/repositories)") #((.warn js/console (.stringify (.-JSON js/window) %1)(dispatch :process-data %1))))))
```

pesterhazy 12:02:06
@debug, now what you mention it, I saw a similar issue when porting my code to Android, and also ended up using js/fetch directly

```clojure
(.then 
  (js/window.fetch 
    "https://api.github.com/repositories") 
    #(println %) 
    #(println "rejected" %))
```

```clojure
(defn request*
  [uri {:keys [request-method on-success on-error retry? retries params]
        :or {retries default-retries
             request-method :get}
        :as opts}]
  (assert (#{:get :post} request-method) (str "invalid request method " request-method))
  (let [uri* (str (client-conf-api)
                  (to-uri uri)
                  (when (= request-method :get)
                    (str "?" (query-string params))))]
    (log/info "fetch:" uri*)
    (-> uri*
        (js/fetch (clj->js (merge {:method ({:post "POST" :get "GET"} request-method)
                                   :headers (if (= request-method :post)
                                              {"Accept" "application/transit+json"
                                               "Content-Type" "application/transit+json"}
                                              {"Accept" "application/transit+json"})}
                                  (when (= request-method :post)
                                    {:body (transit/write params)}))))
        (.then (fn [response]
                 (log/info "got resonse")
                 (.text response)))
        (.then (fn [text]
                 (log/info "got text")
                 (log/info "decoded:" (transit/read text))
                 (on-success (transit/read text))))
        (.catch (fn [err]
                  (log/error "Oops, an error occurred:" err))))))
```


Issues 
- https://github.com/facebook/react-native/issues/5347
- https://github.com/JulianBirch/cljs-ajax/blob/master/src/ajax/xml_http_request.cljs#L20


## Animations

oh my! 60 fps UI on iphone 5 device :aw_yeah:
```clojure
(def ReactNative (js/require "react-native")) (def interaction-manager (.-InteractionManager ReactNative))
(defn on-click [f] (fn [] (.-requestAnimationFrame js/window #(.runAfterInteractions interaction-manager f))))
... (touchable-highlight {:onPress (on-click #(...))}
```


## Tips
recently many dot-forms did not work for me because those required symbol instead of expression as a first argument in things like: (.. (expr) -someAttr -anotherAttr)
so I used let, and it worked. (let [s (expr)] (.. s -someAttr -anotherAttr)
- use refs w/ reagent : [example1](https://clojurians.slack.com/files/andre.richards/F1Y699AMV/drawer___ref.clj), [example 2](https://gist.github.com/pesterhazy/4d9df2edc303e5706d547aeabe0e17e1)

## Build plugin offline
https://github.com/vikeri/rn-cljs-tools

## CI
https://pilloxa.gitlab.io/posts/ci-with-gitlab-and-docker/
@vikeri:
You only have to install gitlab-ci-muti-runner not a full Gitlab instance. Actually I’m just doing nodejs tests at the moment. No integration tests with an iOS simulator yet. But yeah that should work in the future as well: https://about.gitlab.com/2016/03/10/setting-up-gitlab-ci-for-ios-projects/

## Local storage

https://realm.io/docs/react-native/latest/#getting-started

> @tiensonqin: I used Realm.js. I have to store contacts and chat history in disk, so datascript along not works for me.


## Re-natal Workflow 
- `re-natal enable-source-maps`


## Re-natal Workflow - Add components
1. `re-natal use-component react-native-store re-natal use-figwheel`
2. Check component is listed in `.re-natal` and in `package.json`
3. `re-natal use-figwheel`
4. `lein figwheel ios` since `re-natal use-figwheel` does a clean first.
5. Check that `localhost:8081/index.ios.bundle` lists the component.
6. Run `(js/require "react-native-store")` in the repl.

## Custom Native Components
- A helper lib to create custom native ios components https://github.com/frostney/react-native-create-library

> another warn is file and properties names, don't use names that start with RTC, use another namespace, don't use properties like margin, font-size and so one, this is cause conflicts and RN overwrite it. Also remember that if you write your file with Manager at the end, then in your code you don't need to write it, i.e file name - MyViewManager in cljs (require-native-component "MyView") (https://clojurians-log.clojureverse.org/cljsrn/2016-11-10.html#inst-2016-11-10T08:59:13.002600Z)


## Troubleshooting

- `Undefined is not an object (evaluating 'blah.core.init.call')`
This is usually a compilation issue. Perform a `lein cljs build` to locate the problem.

- HAXM for Android and Docker cannot run at the same time  (http://stackoverflow.com/a/39155604/50114)

- How to prevent node from running out of memory when bundling js for React Native
http://stackoverflow.com/questions/38198511/how-to-prevent-node-from-running-out-of-memory-when-bundling-js-for-react-native/38198512

- Never ever use a `println` or a `prn` inside your code, use the logging method described above with `react-native log-ios`, it will works correctly with the simulator or in dev mode on real device but will _crash abruptly_ when deployed in production (like with beta testflight).

- If you need react native crash analysis tools: http://stackoverflow.com/questions/36947752/whats-a-good-setup-for-react-native-crash-reporting




