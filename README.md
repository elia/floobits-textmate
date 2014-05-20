# Floobits TextMate (2) adapter

> 11:09 (elia) ggreer, any plan for TM2, (I can help)
> 11:09 (elia) ?
> 11:12 <kansface> no plan
> 11:13 <kansface> sorry :(
> 11:15 (elia) kansface, that's ok, but if there's an api available maybe it can be done by community :)
> 11:15 <kansface> elia: its possible, but prohibitively hard
> 11:15 <kansface> it takes us about 3 -4 man months to get one working
> 11:16 <kansface> and we mostly know what we are doing
> 11:17 (elia) kansface, :(
> 11:18 (elia) the project is cool, but that's a bummer
> 11:18 <kansface> yeah
> 11:18 <kansface> 3-4 months is the shortest amount of time btw
> 11:18 (elia) kansface, not even a subset can be ported, maybe heavily limited
> 11:18 <kansface> hrm
> 11:19 <kansface> I'm not sure off the top of my head
> 11:20 (elia) kansface, at this point even documenting it seems daunting…
> 11:20 (elia) amirite?
> 11:20 <kansface> well
> 11:20 <kansface> there are things that are so much more pressing
> 11:21 <kansface> and we want to clean t up before we document it
> 11:21 (elia) kansface, k, fair nuff, maybe it'll become easier once it's cleaned up a bit
> 11:22 <kansface> what language does it use for plugins?
> 11:22 (elia) kansface, two ways:
> 11:23 (elia) bash scripts with hashbang (language can be anything)
> 11:23 (elia) or…
> 11:23 (elia) obj-C / C++ if you want to integrate deeply
> 11:23 <kansface> hrm
> 11:23 <kansface> ouch
> 11:24 (elia) but best bet is to make some apis available at the obj-c layer that allows projects like this to integrate
> 11:25 (elia) kansface, which languages you use for other plugins (I know it's python for ST)
> 11:26 <kansface> python for emacs/vim/st
> 11:26 <kansface> java for intellij
> 11:26 <kansface> we have some JS too
> 11:26 <kansface> never finished Atom integration
> 11:26 <kansface> too immature
> 11:27 <kansface> although we are working on VS atm
> 11:27 (elia) kansface, so python sounds like the best bet (via #!/bin/env python)
> 11:27 <kansface> so maybe we'll have some C++ soon
> 11:28 (elia) kansface, there's any python core from the emacs/vim/st plugins that can be reused?
> 11:28 <kansface> https://github.com/Floobits/plugin-common-python
> 11:28 <floobot1> Floobits/plugin-common-python · GitHub
> 11:29 (elia) kansface, last thing, if you can give me an idea on the kind of apis that should be available on the hosting editor that would be great
> 11:30 (elia) probably better to add them to the plugin-common-python readme directly…
> 11:30 <kansface> kind?
> 11:30 <kansface> ah
> 11:30 <kansface> so
> 11:30 <kansface> we use both line delimited json over ssl and https
> 11:31 <kansface> OT happens over ssl
> 11:31 <kansface> some set up happens over http
> 11:32 <kansface> events for OT are asyc and flow both ways
> 11:32 <kansface> they are not necessarily handled in order on the backend
> 11:33 <kansface> general event handling for stuff on the wire: https://github.com/Floobits/plugin-common-python/blob/master/handlers/floo_handler.py#L128
> 11:33 <floobot1> plugin-common-python/handlers/floo_handler.py at master · Floobits/plugin-common-python · GitHub
> 11:33 <kansface> (starts with _on
> 11:33 <kansface> )
> 11:33 <kansface> http stuff
> 11:33 <kansface> https://github.com/Floobits/plugin-common-python/blob/master/api.py
> 11:33 <floobot1> plugin-common-python/api.py at master · Floobits/plugin-common-python · GitHub
> 11:34 <kansface> there is no abstraction layer for events from editors really
> 11:35 <kansface> although each plugin defines an editor.py with a consistent, portable interface to call some editor things
> 11:35 <kansface> like so https://github.com/Floobits/floobits-sublime/blob/master/floo/editor.py
> 11:35 <floobot1> floobits-sublime/floo/editor.py at master · Floobits/floobits-sublime · GitHub
> 11:36 (elia) so ideally I just reimplement editor.py for TM2?
> 11:36 <kansface> the plug in aso maintains state for each buffer in the workspace
> 11:36 <kansface> so, that would be necessary but not sufficient
> 11:37 <kansface> you'd need to implement something like this too https://github.com/Floobits/floobits-sublime/blob/master/floo/listener.py
> 11:37 <floobot1> floobits-sublime/floo/listener.py at master · Floobits/floobits-sublime · GitHub
> 11:37 <kansface> i.e., handle changes from the editor
> 11:38 <kansface> like I said, everything is terrible
> 11:38 <kansface> (our code is bad)
> 11:39 (elia) no worrie ;)
> 11:39 (elia) *worries
