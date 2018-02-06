## Intro to Node: Lecture

JavaScript was released in 1995. It's called JS because Java was really popular at the time, and it's a scripting language because people used it for little things.
Ecmascript came up a few years later and provided some standardization.
When Web 2.0 came out in ~2002, people really started to see the potential for JavaScript. Web 2.0 allowed for AJAX - things could rerender (using JavaScript to look for updates) without reloading the page. Now you could use JavaScript for interactive Single Page Applications.

Until 2007, everyone was stuck on Internet Explorer because of browser differences - developers couldn't develop for every single browser's implementation of JS, so they focused on the most popular browser, IE. In 2007, jQuery came out, and that helped interpret JS across all browsers. 
JS was still a scripting language - we weren't using it to build applications.

In 2008, Chrome was released. Chrome had a V8 engine, which interpreted the JS in a much more performant way than anything before it. Chrome set the bar really high and created an engine that was incredible at processing JavaScript.

In 2009, Ryan Dahl introduced us to Node. He put a V8 engine on his computer and set up a server - suddenly, JS wasn't just a scripting language. It could be used to write applications too.

#### What Is Node?
Node.js is a JavaScript runtime built on Chrome's V8 engine. Node uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.  
Node's package ecosystem, npm, is very easy to use.  
Not only is Node a server, but it was better than what was there before because of the non-blocking I/O model.  

At the end of the day, Node is just a program, written in C, C++, and some JS. 

Node provides APIs for your filesystem - allows you to run JS outside of your browser and interact with files on your computer.

#### Multitasking
JS in single-threaded. How do we multitask when something is single-threaded?  
A _program_ is a set of machine code. Programs are inert don't do anything. A _process_ is an execution - a program gets run as a process.  
__Time Slice__ - this is how your CPU manages tasks. The OS is the "kitchen manager" that tells the CPU what to spend the most time on and tells it when to check in on other processes. You can also thread your CPU into several processes - called _threading_ (or one process could have multiple threads, one thread could have multiple processes, etc). This is all managed via L3 cache. Why L3? L3 is the first cache that's shared across all CPUs on your computer.

Your machine does not speak the same language as you do, so we need something to translate JavaScript into something that the machine understands - that's where the V8 engine comes in.

In Node, a process is on a direct core and has access to its own memory, but the threads that occur within that process can share the same memory. **Advantage**: We don't need to duplicate memory for two tasks that happen concurrently. **Disadvantage**: One thread may modify something that affects another thread. Basically, this is super complicated - Node knows this and handles the threading (and doesn't let the developer change it) - that's why it works with a single-threaded language like JS (Node itself is actually multi-threaded).

#### Node vs. Chrome
Node & Chrome both use V8, but that's where the similarities end.

#### Modularity
What is the purpose of a modular system?
- Organized and maintable system for code splitting
- Why split code?
  - Single files have more defined responsibility
  - Easier collaboration
  - Visible structure
  - Testing
  - Maintainability
  - Reusability
- The ability to import and export via JS is specified in ECMAScript, but no browsers have implement it (Node has!)
- Node uses "require" to import in exports from another file (eg - let pokemon = require('./pokemon') will import the exports from the "pokemon.js" file in the same folder)

How do you install and use an npm module?  
EX - "Chalk"
``` npm install chalk ```
``` let chalk = require('chalk') ```

We use 'package.json' to track our npm modules. To create a package.json file with default settings, use ```npm init --y```

#### Asynchronous/Non-Blocking I/O
When something will take an unknown amount of time, Node creates a separate thread, and when that operation completes, and then whenever JS finishes whatever else it will doing, it will check on that operation. This is really efficient, because it means that we as developers don't need to worry about the multithreading - Node does it for us.