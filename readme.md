<h1> Welcome to <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a></h1><div class='tw-tiddler-frame' data-tiddler-target='HelloThere' data-tiddler-template='HelloThere'><p>Welcome to <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a>, a reboot of <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a>, the venerable, reusable non-linear personal web notebook <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='History'>first released in 2004</a>. It is a complete interactive wiki that can run from a single HTML file in the browser or as a powerful <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='node.js'>node.js application</a>.</p><p><a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a> offers a radically improved way of managing your data compared to traditional documents and emails. The fundamental idea is that information is more useful and reusable if we cut up into the <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='Tiddlers'>smallest semantically meaningful chunks</a> and use links, tags and macros to model the structured relationships between them.  <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a> aims to provide a fluid interface for working with tiddlers, allowing them to be aggregated and composed into longer narratives.</p><p><a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a> is at an incomplete early alpha stage, and is not yet ready for general use. But it's the best possible time to get involved and support its future development. You can:</p><ul><li> Explore its features online at <a class='tw-tiddlylink tw-tiddlylink-external' href='http://alpha.tiddlywiki.com/'>http://alpha.tiddlywiki.com/</a></li><li> Get involved in the <a class='tw-tiddlylink tw-tiddlylink-external' href='https://github.com/Jermolene/TiddlyWiki5'>development on GitHub</a></li><li> Join the discussions on <a class='tw-tiddlylink tw-tiddlylink-external' href='http://groups.google.com/group/TiddlyWikiDev'>the TiddlyWikiDev Google Group</a></li><li> Follow <a class='tw-tiddlylink tw-tiddlylink-external' href='http://twitter.com/#!/TiddlyWiki'>@TiddlyWiki on Twitter</a> for the latest news</li></ul></div><h1> Usage</h1><div class='tw-tiddler-frame tw-tiddler-missing' data-tiddler-target='CommandLineInterface' data-tiddler-template='CommandLineInterface'></div><h1> Architecture</h1><div class='tw-tiddler-frame' data-tiddler-target='TiddlyWikiArchitecture' data-tiddler-template='TiddlyWikiArchitecture'><h1> Overview</h1><p>The heart of <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a> can be seen as an extensible representation transformation engine. Given the text of a tiddler and its associated <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='ContentType'>ContentType</a>, the engine can produce a rendering of the tiddler in a new <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='ContentType'>ContentType</a>. Furthermore, it can efficiently selectively update the rendering to track any changes in the tiddler or its dependents.</p><p>The most important transformations are from <code>text/x-tiddlywiki</code> wikitext into <code>text/html</code> or <code>text/plain</code> but the engine is used throughout the system for other transformations, such as converting images for display in HTML, sanitising fragments of <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a>, and processing CSS.</p><p>You can explore this mechanism by opening the <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a> console in your browser. Typing this command will replace the text of the tiddler <code>HelloThere</code> with new content:</p><pre>$tw.wiki.addTiddler({title: &quot;HelloThere&quot;, text: &quot;This is some new content&quot;});</pre><p>If the tiddler <code>HelloThere</code> is visible then you'll see it instantly change to reflect the new content. If you create a tiddler that doesn't currently exist then you'll see any displayed links to it instantly change from italicised to normal:</p><pre>$tw.wiki.addTiddler({title: &quot;TiddlyWiki5&quot;, text: &quot;This tiddler now exists&quot;});</pre><p>The power of this mechanism also drives the interactive features of <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a>. For example, try typing the following into the <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a> console:</p><pre>$tw.wiki.addTiddler({title: &quot;ViewDropDownState&quot;, text: &quot;(50,50,200,200)&quot;});</pre><p>You should see the view dropdown appear in the middle of the screen. The underlying mechanism is that the creation of the tiddler with this title triggers the display of the popup at the specified location.</p><p>If you're interested in understanding more about the internal operation of <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a>, it is recommended that you review the <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='Docs'>Docs</a> and read the code &ndash; start with the boot kernel <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='%24%3A%2Fcore%2Fboot.js'>$:/core/boot.js</a>.
</p></div><h1> Plugin Mechanism</h1><div class='tw-tiddler-frame' data-tiddler-target='PluginMechanism' data-tiddler-template='PluginMechanism'><h1>Introduction</h1><p><a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a> is based on a 500 line boot kernel that runs on node.js or in the browser, and everything else is plugins.</p><p>The kernel boots just enough of the <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a> environment to allow it to load tiddlers as plugins and execute them (a barebones tiddler class, a barebones wiki store class, some utilities etc.). Plugin modules are written like <code>node.js</code> modules; you can use <code>require()</code> to invoke sub components and to control load order.</p><p>There are several different types of plugins: parsers, serializers, deserializers, macros etc. It goes much further than you might expect. For example, individual tiddler fields are plugins, too: there's a plugin that knows how to handle the <code>tags</code> field, and another that knows how to handle the special behaviour of
the <code>modified</code> and <code>created</code> fields.</p><p>Some plugins have further sub-plugins: the wikitext parser, for instance, accepts rules as individual plugins.</p><h1>Plugins and Modules</h1><p>In <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a>, a plugin is a bundle of related tiddlers that are distributed together as a single unit. Plugins can include tiddlers which are <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a> modules. </p><p>The file <code>core/boot.js</code> is a barebones <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-resolves' href='TiddlyWiki'>TiddlyWiki</a> kernel that is just sufficient to load the core plugin modules and trigger a startup plugin module to load up the rest of the application.</p><p>The kernel includes:</p><ul><li> Eight short shared utility functions</li><li> Three methods implementing the plugin module mechanism</li><li> The <code>$tw.Tiddler</code> class (and three field definition plugins)</li><li> The <code>$tw.Wiki</code> class (and three tiddler deserialization methods)</li><li> Code for the browser to load tiddlers from the HTML DOM</li><li> Code for the server to load tiddlers from the file system</li></ul><p>Each module is an ordinary <code>node.js</code>-style module, using the <code>require()</code> function to access other modules and the <code>exports</code> global to return <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a> values. The boot kernel smooths over the differences between <code>node.js</code> and the browser, allowing the same plugin modules to execute in both environments.</p><p>In the browser, <code>core/boot.js</code> is packed into a template HTML file that contains the following elements in order:</p><ul><li> Ordinary and shadow tiddlers, packed as HTML <code>&lt;DIV&gt;</code> elements</li><li> <code>core/bootprefix.js</code>, containing a few lines to set up the plugin environment</li><li> Plugin <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a> modules, packed as HTML <code>&lt;SCRIPT&gt;</code> blocks</li><li> <code>core/boot.js</code>, containing the boot kernel</li></ul><p>On the server, <code>core/boot.js</code> is executed directly. It uses the <code>node.js</code> local file API to load plugins directly from the file system in the <code>core/modules</code> directory. The code loading is performed synchronously for brevity (and because the system is in any case inherently blocked until plugins are loaded).</p><p>The boot kernel sets up the <code>$tw</code> global variable that is used to store all the state data of the system.</p><h1>Core </h1><p>The 'core' is the boot kernel plus the set of plugin modules that it loads. It contains plugins of the following types:</p><ul><li> <code>tiddlerfield</code> - defines the characteristics of tiddler fields of a particular name</li><li> <code>tiddlerdeserializer</code> - methods to extract tiddlers from text representations or the DOM</li><li> <code>startup</code> - functions to be called by the kernel after booting</li><li> <code>global</code> - members of the <code>$tw</code> global</li><li> <code>config</code> - values to be merged over the <code>$tw.config</code> global</li><li> <code>utils</code> - general purpose utility functions residing in <code>$tw.utils</code></li><li> <code>tiddlermethod</code> - additional methods for the <code>$tw.Tiddler</code> class</li><li> <code>wikimethod</code> - additional methods for the <code>$tw.Wiki</code> class</li><li> <code>treeutils</code> - static utility methods for parser tree nodes </li><li> <code>treenode</code> - classes of parser tree nodes</li><li> <code>macro</code> - macro definitions</li><li> <code>editor</code> - interactive editors for different types of content</li><li> <code>parser</code> - parsers for different types of content</li><li> <code>wikitextrule</code> - individual rules for the wikitext parser</li><li> <code>command</code> - individual commands for the <code>$tw.Commander</code> class</li></ul><p><a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a> makes extensive use of <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='JavaScript'>JavaScript</a> inheritance:</p><ul><li> Tree nodes defined in <code>$:/core/treenodes/</code> all inherit from <code>$:/core/treenodes/node.js</code></li><li> Macros defined in <code>$:/core/macros/</code> all inherit from <code>$:/core/treenodes/macro.js</code></li></ul><p><code>tiddlywiki.plugin</code> files
</p></div><p><em>This <code>readme</code> file was automatically generated by <a class='tw-tiddlylink tw-tiddlylink-internal tw-tiddlylink-missing' href='TiddlyWiki5'>TiddlyWiki5</a></em>
</p>